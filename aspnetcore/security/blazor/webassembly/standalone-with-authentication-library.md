---
title: Sécuriser une application autonome webassembly Blazor ASP.NET Core avec la bibliothèque d’authentification
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-authentication-library
ms.openlocfilehash: ea50d94835b044f9c3d6a0561868f081d32cb62a
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219001"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-the-authentication-library"></a>Sécuriser une application autonome webassembly Blazor ASP.NET Core avec la bibliothèque d’authentification

Par [Javier Calvarro Nelson](https://github.com/javiercn) et [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

*Pour Azure Active Directory (AAD) et Azure Active Directory B2C (AAD B2C), ne suivez pas les instructions de cette rubrique. Consultez les rubriques AAD et AAD B2C dans ce nœud de table des matières.*

Pour créer une application autonome webassembly Blazor qui utilise `Microsoft.AspNetCore.Components.WebAssembly.Authentication` bibliothèque, exécutez la commande suivante dans une interface de commande :

```dotnetcli
dotnet new blazorwasm -au Individual
```

Pour spécifier l’emplacement de sortie, qui crée un dossier de projet s’il n’existe pas, incluez l’option de sortie dans la commande avec un chemin d’accès (par exemple, `-o BlazorSample`). Le nom du dossier devient également une partie du nom du projet.

Dans Visual Studio, [créez une application Blazor Webassembly](xref:blazor/get-started). Définissez **l’authentification** sur **des comptes d’utilisateur individuels** avec l’option **stocker les comptes d’utilisateur dans l’application** .

## <a name="authentication-package"></a>Package d’authentification

Quand une application est créée pour utiliser des comptes d’utilisateur individuels, l’application reçoit automatiquement une référence de package pour le package `Microsoft.AspNetCore.Components.WebAssembly.Authentication` dans le fichier projet de l’application. Le package fournit un ensemble de primitives qui aident l’application à authentifier les utilisateurs et à obtenir des jetons pour appeler des API protégées.

Si vous ajoutez l’authentification à une application, ajoutez manuellement le package au fichier projet de l’application :

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

Remplacez `{VERSION}` dans la référence de package précédente par la version du package `Microsoft.AspNetCore.Blazor.Templates` présentée dans l’article <xref:blazor/get-started>.

## <a name="authentication-service-support"></a>Prise en charge du service d’authentification

La prise en charge de l’authentification des utilisateurs est inscrite dans le conteneur de service à l’aide de la méthode d’extension `AddOidcAuthentication` fournie par le package `Microsoft.AspNetCore.Components.WebAssembly.Authentication`. Cette méthode configure tous les services requis pour que l’application interagisse avec le fournisseur d’identité (IP).

*Program.cs* :

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    options.ProviderOptions.Authority = "{AUTHORITY}";
    options.ProviderOptions.ClientId = "{CLIENT ID}";
});
```

La prise en charge de l’authentification pour les applications autonomes est proposée à l’aide d’Open ID Connect (OIDC). La méthode `AddOidcAuthentication` accepte un rappel pour configurer les paramètres requis pour authentifier une application à l’aide de OIDC. Les valeurs requises pour la configuration de l’application peuvent être obtenues à partir de l’adresse IP conforme à OIDC. Obtenez les valeurs lors de l’inscription de l’application, qui se produit généralement dans son portail en ligne.

## <a name="index-page"></a>Page d'index

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

## <a name="app-component"></a>Composant d’application

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a>Composant RedirectToLogin

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a>Composant LoginDisplay

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a>Composant d’authentification

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]
