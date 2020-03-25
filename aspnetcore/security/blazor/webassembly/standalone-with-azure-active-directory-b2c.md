---
title: Sécuriser une application autonome webassembly Blazor ASP.NET Core avec Azure Active Directory B2C
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory-b2c
ms.openlocfilehash: bb03ef1e6d216cfc06e2b91919c64f92f2ef634e
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219270"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory-b2c"></a><span data-ttu-id="5fba8-102">Sécuriser une application autonome webassembly Blazor ASP.NET Core avec Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="5fba8-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Azure Active Directory B2C</span></span>

<span data-ttu-id="5fba8-103">Par [Javier Calvarro Nelson](https://github.com/javiercn) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5fba8-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="5fba8-104">Pour créer une application autonome webassembly Blazor qui utilise [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) pour l’authentification :</span><span class="sxs-lookup"><span data-stu-id="5fba8-104">To create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) for authentication:</span></span>

1. <span data-ttu-id="5fba8-105">Suivez les instructions des rubriques suivantes pour créer un locataire et inscrire une application Web dans le portail Azure :</span><span class="sxs-lookup"><span data-stu-id="5fba8-105">Follow the guidance in the following topics to create a tenant and register a web app in the Azure Portal:</span></span>

   * <span data-ttu-id="5fba8-106">[Créez un locataire AAD B2C](/azure/active-directory-b2c/tutorial-create-tenant) &ndash; enregistrez les informations suivantes :</span><span class="sxs-lookup"><span data-stu-id="5fba8-106">[Create an AAD B2C tenant](/azure/active-directory-b2c/tutorial-create-tenant) &ndash; Record the following information:</span></span>

     <span data-ttu-id="5fba8-107">1 \.</span><span class="sxs-lookup"><span data-stu-id="5fba8-107">1\.</span></span> <span data-ttu-id="5fba8-108">AAD B2C instance (par exemple, `https://contoso.b2clogin.com/`, qui comprend la barre oblique finale)</span><span class="sxs-lookup"><span data-stu-id="5fba8-108">AAD B2C instance (for example, `https://contoso.b2clogin.com/`, which includes the trailing slash)</span></span><br>
     <span data-ttu-id="5fba8-109">2 \.</span><span class="sxs-lookup"><span data-stu-id="5fba8-109">2\.</span></span> <span data-ttu-id="5fba8-110">AAD B2C domaine de locataire (par exemple, `contoso.onmicrosoft.com`)</span><span class="sxs-lookup"><span data-stu-id="5fba8-110">AAD B2C Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>

   * <span data-ttu-id="5fba8-111">[Inscrire une application web](/azure/active-directory-b2c/tutorial-register-applications) &ndash; effectuer les sélections suivantes lors de l’inscription de l’application :</span><span class="sxs-lookup"><span data-stu-id="5fba8-111">[Register a web application](/azure/active-directory-b2c/tutorial-register-applications) &ndash; Make the following selections during app registration:</span></span>

     <span data-ttu-id="5fba8-112">1 \.</span><span class="sxs-lookup"><span data-stu-id="5fba8-112">1\.</span></span> <span data-ttu-id="5fba8-113">Définissez **application Web/API Web** sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="5fba8-113">Set **Web App / Web API** to **Yes**.</span></span><br>
     <span data-ttu-id="5fba8-114">2 \.</span><span class="sxs-lookup"><span data-stu-id="5fba8-114">2\.</span></span> <span data-ttu-id="5fba8-115">Définissez **autoriser le Flow implicite** sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="5fba8-115">Set **Allow implicit flow** to **Yes**.</span></span><br>
     <span data-ttu-id="5fba8-116">3 \.</span><span class="sxs-lookup"><span data-stu-id="5fba8-116">3\.</span></span> <span data-ttu-id="5fba8-117">Ajoutez une **URL de réponse** de `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="5fba8-117">Add a **Reply URL** of `https://localhost:5001/authentication/login-callback`.</span></span>

     <span data-ttu-id="5fba8-118">Enregistrez l’ID d’application (ID client) (par exemple, `11111111-1111-1111-1111-111111111111`).</span><span class="sxs-lookup"><span data-stu-id="5fba8-118">Record the Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`).</span></span>

   * <span data-ttu-id="5fba8-119">[Créer des flux d’utilisateurs](/azure/active-directory-b2c/tutorial-create-user-flows) &ndash; créer un flux d’utilisateur d’inscription et de connexion.</span><span class="sxs-lookup"><span data-stu-id="5fba8-119">[Create user flows](/azure/active-directory-b2c/tutorial-create-user-flows) &ndash; Create a sign-up and sign-in user flow.</span></span>

     <span data-ttu-id="5fba8-120">Au minimum, sélectionnez l’attribut d’utilisateur **revendications d’Application** > **nom complet** pour remplir le `context.User.Identity.Name` dans le composant `LoginDisplay` (*Shared/LoginDisplay. Razor*).</span><span class="sxs-lookup"><span data-stu-id="5fba8-120">At a minimum, select the **Application claims** > **Display Name** user attribute to populate the `context.User.Identity.Name` in the `LoginDisplay` component (*Shared/LoginDisplay.razor*).</span></span>

     <span data-ttu-id="5fba8-121">Notez le nom du workflow d’inscription et de connexion de l’utilisateur créé pour l’application (par exemple, `B2C_1_signupsignin`).</span><span class="sxs-lookup"><span data-stu-id="5fba8-121">Record the sign-up and sign-in user flow name created for the app (for example, `B2C_1_signupsignin`).</span></span>

1. <span data-ttu-id="5fba8-122">Remplacez les espaces réservés dans la commande suivante par les informations enregistrées précédemment et exécutez la commande dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="5fba8-122">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --client-id "{CLIENT ID}" --domain "{DOMAIN}" -ssp "{SIGN UP OR SIGN IN POLICY}"
   ```

   <span data-ttu-id="5fba8-123">Pour spécifier l’emplacement de sortie, qui crée un dossier de projet s’il n’existe pas, incluez l’option de sortie dans la commande avec un chemin d’accès (par exemple, `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="5fba8-123">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="5fba8-124">Le nom du dossier devient également une partie du nom du projet.</span><span class="sxs-lookup"><span data-stu-id="5fba8-124">The folder name also becomes part of the project's name.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="5fba8-125">Package d’authentification</span><span class="sxs-lookup"><span data-stu-id="5fba8-125">Authentication package</span></span>

<span data-ttu-id="5fba8-126">Quand une application est créée pour utiliser un compte B2C (`IndividualB2C`) individuel, l’application reçoit automatiquement une référence de package pour la [bibliothèque d’authentification Microsoft](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span><span class="sxs-lookup"><span data-stu-id="5fba8-126">When an app is created to use an Individual B2C Account (`IndividualB2C`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="5fba8-127">Le package fournit un ensemble de primitives qui aident l’application à authentifier les utilisateurs et à obtenir des jetons pour appeler des API protégées.</span><span class="sxs-lookup"><span data-stu-id="5fba8-127">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="5fba8-128">Si vous ajoutez l’authentification à une application, ajoutez manuellement le package au fichier projet de l’application :</span><span class="sxs-lookup"><span data-stu-id="5fba8-128">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="5fba8-129">Remplacez `{VERSION}` dans la référence de package précédente par la version du package `Microsoft.AspNetCore.Blazor.Templates` présentée dans l’article <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="5fba8-129">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="5fba8-130">Le package `Microsoft.Authentication.WebAssembly.Msal` ajoute transitivement le package `Microsoft.AspNetCore.Components.WebAssembly.Authentication` à l’application.</span><span class="sxs-lookup"><span data-stu-id="5fba8-130">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="5fba8-131">Prise en charge du service d’authentification</span><span class="sxs-lookup"><span data-stu-id="5fba8-131">Authentication service support</span></span>

<span data-ttu-id="5fba8-132">La prise en charge de l’authentification des utilisateurs est inscrite dans le conteneur de service à l’aide de la méthode d’extension `AddMsalAuthentication` fournie par le package `Microsoft.Authentication.WebAssembly.Msal`.</span><span class="sxs-lookup"><span data-stu-id="5fba8-132">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="5fba8-133">Cette méthode configure tous les services requis pour que l’application interagisse avec le fournisseur d’identité (IP).</span><span class="sxs-lookup"><span data-stu-id="5fba8-133">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="5fba8-134">*Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="5fba8-134">*Program.cs*:</span></span>

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

<span data-ttu-id="5fba8-135">La méthode `AddMsalAuthentication` accepte un rappel pour configurer les paramètres requis pour authentifier une application.</span><span class="sxs-lookup"><span data-stu-id="5fba8-135">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="5fba8-136">Les valeurs requises pour la configuration de l’application peuvent être obtenues à partir de la configuration AAD du portail Azure lorsque vous inscrivez l’application.</span><span class="sxs-lookup"><span data-stu-id="5fba8-136">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="5fba8-137">Le modèle Blazor webassembly ne configure pas automatiquement l’application pour demander un jeton d’accès pour une API sécurisée.</span><span class="sxs-lookup"><span data-stu-id="5fba8-137">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="5fba8-138">Pour approvisionner un jeton dans le cadre du processus de connexion, ajoutez l’étendue aux étendues de jeton d’accès par défaut de la `MsalProviderOptions`:</span><span class="sxs-lookup"><span data-stu-id="5fba8-138">To provision a token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{API ID URI}/{SCOPE}");
});
```

## <a name="index-page"></a><span data-ttu-id="5fba8-139">Page d'index</span><span class="sxs-lookup"><span data-stu-id="5fba8-139">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="5fba8-140">Composant d’application</span><span class="sxs-lookup"><span data-stu-id="5fba8-140">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="5fba8-141">Composant RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="5fba8-141">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="5fba8-142">Composant LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="5fba8-142">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="5fba8-143">Composant d’authentification</span><span class="sxs-lookup"><span data-stu-id="5fba8-143">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="5fba8-144">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="5fba8-144">Additional resources</span></span>

* <xref:security/authentication/azure-ad-b2c>
* [<span data-ttu-id="5fba8-145">Didacticiel : créer un locataire Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="5fba8-145">Tutorial: Create an Azure Active Directory B2C tenant</span></span>](/azure/active-directory-b2c/tutorial-create-tenant)
