---
title: Sécuriser une application autonome webassembly Blazor ASP.NET Core avec Azure Active Directory
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory
ms.openlocfilehash: e12c38ed42a4e2714d785ef8f03097246c40d36e
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218975"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory"></a>Sécuriser une application autonome webassembly Blazor ASP.NET Core avec Azure Active Directory

Par [Javier Calvarro Nelson](https://github.com/javiercn) et [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Pour créer une application autonome webassembly Blazor qui utilise [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) pour l’authentification :

[Créez un client et une application Web AAD](/azure/active-directory/develop/v2-overview):

Inscrire une application AAD dans le **Azure Active Directory** > **inscriptions d’applications** zone du portail Azure :

1. Fournissez un **nom** pour l’application (par exemple, **Blazor client AAD**).
1. Choisissez un **type de compte pris en charge**. Vous ne pouvez sélectionner des **comptes dans cet annuaire d’organisation que** pour cette expérience.
1. Laissez la liste déroulante **URI de redirection** définie sur **Web**et fournissez un URI de redirection de `https://localhost:5001/authentication/login-callback`.
1. Désactivez la case à cocher accorder les **autorisations > ** **accorder à l’administrateur de openid et de offline_access** .
1. Sélectionnez **Inscription**.

Dans > **d’authentification** **configurations de plateforme** > **Web**:

1. Confirmez que l' **URI de redirection** de `https://localhost:5001/authentication/login-callback` est présent.
1. Pour **octroi implicite**, activez les cases à cocher pour les **jetons d’accès** et les **jetons d’ID**.
1. Les valeurs par défaut restantes pour l’application sont acceptables pour cette expérience.
1. Sélectionnez le bouton **Enregistrer**.

Notez les informations suivantes :

* ID d’application (ID client) (par exemple, `11111111-1111-1111-1111-111111111111`)
* ID de répertoire (ID de locataire) (par exemple, `22222222-2222-2222-2222-222222222222`)

Remplacez les espaces réservés dans la commande suivante par les informations enregistrées précédemment et exécutez la commande dans une interface de commande :

```dotnetcli
dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "{TENANT ID}"
```

Pour spécifier l’emplacement de sortie, qui crée un dossier de projet s’il n’existe pas, incluez l’option de sortie dans la commande avec un chemin d’accès (par exemple, `-o BlazorSample`). Le nom du dossier devient également une partie du nom du projet.

## <a name="authentication-package"></a>Package d’authentification

Quand une application est créée pour utiliser des comptes professionnels ou scolaires (`SingleOrg`), l’application reçoit automatiquement une référence de package pour la [bibliothèque d’authentification Microsoft](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`). Le package fournit un ensemble de primitives qui aident l’application à authentifier les utilisateurs et à obtenir des jetons pour appeler des API protégées.

Si vous ajoutez l’authentification à une application, ajoutez manuellement le package au fichier projet de l’application :

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

Remplacez `{VERSION}` dans la référence de package précédente par la version du package `Microsoft.AspNetCore.Blazor.Templates` présentée dans l’article <xref:blazor/get-started>.

Le package `Microsoft.Authentication.WebAssembly.Msal` ajoute transitivement le package `Microsoft.AspNetCore.Components.WebAssembly.Authentication` à l’application.

## <a name="authentication-service-support"></a>Prise en charge du service d’authentification

La prise en charge de l’authentification des utilisateurs est inscrite dans le conteneur de service à l’aide de la méthode d’extension `AddMsalAuthentication` fournie par le package `Microsoft.Authentication.WebAssembly.Msal`. Cette méthode configure tous les services requis pour que l’application interagisse avec le fournisseur d’identité (IP).

*Program.cs* :

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
});
```

La méthode `AddMsalAuthentication` accepte un rappel pour configurer les paramètres requis pour authentifier une application. Les valeurs requises pour la configuration de l’application peuvent être obtenues à partir de la configuration AAD du portail Azure lorsque vous inscrivez l’application.

Le modèle Blazor webassembly ne configure pas automatiquement l’application pour demander un jeton d’accès pour une API sécurisée. Pour approvisionner un jeton dans le cadre du processus de connexion, ajoutez l’étendue aux étendues de jeton d’accès par défaut de la `MsalProviderOptions`:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}");
});
```

> [!NOTE]
> L’étendue du jeton d’accès par défaut doit être au format `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (par exemple, `11111111-1111-1111-1111-111111111111/API.Access`). Si un modèle ou un schéma et un hôte sont fournis au paramètre d’étendue (comme indiqué dans le portail Azure), l' *application cliente* lève une exception non gérée lorsqu’elle reçoit une réponse *non autorisée 401* de l' *application API serveur*.

## <a name="index-page"></a>Page d'index

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a>Composant d’application

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a>Composant RedirectToLogin

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a>Composant LoginDisplay

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a>Composant d’authentification

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:security/authentication/azure-active-directory/index>
