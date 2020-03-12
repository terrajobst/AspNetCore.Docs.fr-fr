---
title: Sécuriser une application autonome webassembly Blazor ASP.NET Core avec Azure Active Directory B2C
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory-b2c
ms.openlocfilehash: 0ea42943c908d8cf9d083c1cfc568c1835588ce9
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083831"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory-b2c"></a>Sécuriser une application autonome webassembly Blazor ASP.NET Core avec Azure Active Directory B2C

Par [Javier Calvarro Nelson](https://github.com/javiercn) et [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Pour créer une application autonome webassembly Blazor qui utilise [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) pour l’authentification :

1. Suivez les instructions des rubriques suivantes pour créer un locataire et inscrire une application Web dans le portail Azure :

   * [Créez un locataire AAD B2C](/azure/active-directory-b2c/tutorial-create-tenant) &ndash; enregistrez les informations suivantes :

     1 \. AAD B2C instance (par exemple, `https://contoso.b2clogin.com/`, qui comprend la barre oblique finale)<br>
     2 \. AAD B2C domaine de locataire (par exemple, `contoso.onmicrosoft.com`)

   * [Inscrire une application web](/azure/active-directory-b2c/tutorial-register-applications) &ndash; effectuer les sélections suivantes lors de l’inscription de l’application :

     1 \. Définissez **application Web/API Web** sur **Oui**.<br>
     2 \. Définissez **autoriser le Flow implicite** sur **Oui**.<br>
     3 \. Ajoutez une **URL de réponse** de `https://localhost:5001/authentication/login-callback`.

     Enregistrez l’ID d’application (ID client) (par exemple, `11111111-1111-1111-1111-111111111111`).

   * [Créer des flux d’utilisateurs](/azure/active-directory-b2c/tutorial-create-user-flows) & ndash ; Créez un workflow utilisateur d’inscription et de connexion.

     Notez le nom du workflow d’inscription et de connexion de l’utilisateur créé pour l’application (par exemple, `B2C_1_signupsignin`).

1. Remplacez les espaces réservés dans la commande suivante par les informations enregistrées précédemment et exécutez la commande dans une interface de commande :

   ```dotnetcli
   dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --client-id "{CLIENT ID}" --domain "{DOMAIN}" -ssp "{SIGN UP OR SIGN IN POLICY}"
   ```

   Pour spécifier l’emplacement de sortie, qui crée un dossier de projet s’il n’existe pas, incluez l’option de sortie dans la commande avec un chemin d’accès (par exemple, `-o BlazorSample`). Le nom du dossier devient également une partie du nom du projet.

## <a name="authentication-package"></a>Package d’authentification

Quand une application est créée pour utiliser un compte B2C (`IndividualB2C`) individuel, l’application reçoit automatiquement une référence de package pour la [bibliothèque d’authentification Microsoft](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`). Le package fournit un ensemble de primitives qui aident l’application à authentifier les utilisateurs et à obtenir des jetons pour appeler des API protégées.

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
    authentication.Authority = 
        "{AAD B2C INSTANCE}{DOMAIN}/{SIGN UP OR SIGN IN POLICY}";
    authentication.ClientId = "{CLIENT ID}";
    authentication.ValidateAuthority = false;
});
```

La méthode `AddMsalAuthentication` accepte un rappel pour configurer les paramètres requis pour authentifier une application. Les valeurs requises pour la configuration de l’application peuvent être obtenues à partir de la configuration AAD du portail Azure lorsque vous inscrivez l’application.

Le modèle Blazor webassembly ne configure pas automatiquement l’application pour demander un jeton d’accès pour une API sécurisée. Pour approvisionner un jeton dans le cadre du processus de connexion, ajoutez l’étendue aux étendues de jeton d’accès par défaut de la `MsalProviderOptions`:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{API ID URI}/{SCOPE}");
});
```

## <a name="index-page"></a>Page d'index

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

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

* <xref:security/authentication/azure-ad-b2c>
* [Didacticiel : créer un locataire Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-create-tenant)
