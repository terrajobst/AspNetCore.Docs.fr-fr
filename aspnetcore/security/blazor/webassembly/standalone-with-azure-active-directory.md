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
ms.openlocfilehash: 76bcac29d86a236e56c0eaea24a694c4845ecbcf
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083747"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory"></a><span data-ttu-id="8a4ed-102">Sécuriser une application autonome webassembly Blazor ASP.NET Core avec Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8a4ed-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Azure Active Directory</span></span>

<span data-ttu-id="8a4ed-103">Par [Javier Calvarro Nelson](https://github.com/javiercn) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8a4ed-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="8a4ed-104">Pour créer une application autonome webassembly Blazor qui utilise [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) pour l’authentification :</span><span class="sxs-lookup"><span data-stu-id="8a4ed-104">To create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) for authentication:</span></span>

<span data-ttu-id="8a4ed-105">[Créez un client et une application Web AAD](/azure/active-directory/develop/v2-overview):</span><span class="sxs-lookup"><span data-stu-id="8a4ed-105">[Create an AAD tenant and web application](/azure/active-directory/develop/v2-overview):</span></span>

<span data-ttu-id="8a4ed-106">Inscrire une application AAD dans le **Azure Active Directory** > **inscriptions d’applications** zone du portail Azure :</span><span class="sxs-lookup"><span data-stu-id="8a4ed-106">Register a AAD app in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="8a4ed-107">Fournissez un **nom** pour l’application (par exemple, **Blazor client AAD**).</span><span class="sxs-lookup"><span data-stu-id="8a4ed-107">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span>
1. <span data-ttu-id="8a4ed-108">Choisissez un **type de compte pris en charge**.</span><span class="sxs-lookup"><span data-stu-id="8a4ed-108">Choose a **Supported account types**.</span></span> <span data-ttu-id="8a4ed-109">Vous ne pouvez sélectionner des **comptes dans cet annuaire d’organisation que** pour cette expérience.</span><span class="sxs-lookup"><span data-stu-id="8a4ed-109">You may select **Accounts in this organizational directory only** for this experience.</span></span>
1. <span data-ttu-id="8a4ed-110">Laissez la liste déroulante **URI de redirection** définie sur **Web**et fournissez un URI de redirection de `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="8a4ed-110">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="8a4ed-111">Désactivez la case à cocher accorder les \*\*autorisations > \*\* **accorder à l’administrateur de openid et de offline_access** .</span><span class="sxs-lookup"><span data-stu-id="8a4ed-111">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="8a4ed-112">Sélectionnez **Inscription**.</span><span class="sxs-lookup"><span data-stu-id="8a4ed-112">Select **Register**.</span></span>

<span data-ttu-id="8a4ed-113">Dans > **d’authentification** **configurations de plateforme** > **Web**:</span><span class="sxs-lookup"><span data-stu-id="8a4ed-113">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="8a4ed-114">Confirmez que l' **URI de redirection** de `https://localhost:5001/authentication/login-callback` est présent.</span><span class="sxs-lookup"><span data-stu-id="8a4ed-114">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="8a4ed-115">Pour **octroi implicite**, activez les cases à cocher pour les **jetons d’accès** et les **jetons d’ID**.</span><span class="sxs-lookup"><span data-stu-id="8a4ed-115">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="8a4ed-116">Les valeurs par défaut restantes pour l’application sont acceptables pour cette expérience.</span><span class="sxs-lookup"><span data-stu-id="8a4ed-116">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="8a4ed-117">Sélectionnez le bouton **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="8a4ed-117">Select the **Save** button.</span></span>

<span data-ttu-id="8a4ed-118">Notez les informations suivantes :</span><span class="sxs-lookup"><span data-stu-id="8a4ed-118">Record the following information:</span></span>

* <span data-ttu-id="8a4ed-119">ID d’application (ID client) (par exemple, `11111111-1111-1111-1111-111111111111`)</span><span class="sxs-lookup"><span data-stu-id="8a4ed-119">Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="8a4ed-120">ID de répertoire (ID de locataire) (par exemple, `22222222-2222-2222-2222-222222222222`)</span><span class="sxs-lookup"><span data-stu-id="8a4ed-120">Directory ID (Tenant ID) (for example, `22222222-2222-2222-2222-222222222222`)</span></span>

<span data-ttu-id="8a4ed-121">Remplacez les espaces réservés dans la commande suivante par les informations enregistrées précédemment et exécutez la commande dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="8a4ed-121">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "{TENANT ID}"
```

<span data-ttu-id="8a4ed-122">Pour spécifier l’emplacement de sortie, qui crée un dossier de projet s’il n’existe pas, incluez l’option de sortie dans la commande avec un chemin d’accès (par exemple, `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="8a4ed-122">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="8a4ed-123">Le nom du dossier devient également une partie du nom du projet.</span><span class="sxs-lookup"><span data-stu-id="8a4ed-123">The folder name also becomes part of the project's name.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="8a4ed-124">Package d’authentification</span><span class="sxs-lookup"><span data-stu-id="8a4ed-124">Authentication package</span></span>

<span data-ttu-id="8a4ed-125">Quand une application est créée pour utiliser des comptes professionnels ou scolaires (`SingleOrg`), l’application reçoit automatiquement une référence de package pour la [bibliothèque d’authentification Microsoft](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span><span class="sxs-lookup"><span data-stu-id="8a4ed-125">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="8a4ed-126">Le package fournit un ensemble de primitives qui aident l’application à authentifier les utilisateurs et à obtenir des jetons pour appeler des API protégées.</span><span class="sxs-lookup"><span data-stu-id="8a4ed-126">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="8a4ed-127">Si vous ajoutez l’authentification à une application, ajoutez manuellement le package au fichier projet de l’application :</span><span class="sxs-lookup"><span data-stu-id="8a4ed-127">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="8a4ed-128">Remplacez `{VERSION}` dans la référence de package précédente par la version du package `Microsoft.AspNetCore.Blazor.Templates` présentée dans l’article <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="8a4ed-128">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="8a4ed-129">Le package `Microsoft.Authentication.WebAssembly.Msal` ajoute transitivement le package `Microsoft.AspNetCore.Components.WebAssembly.Authentication` à l’application.</span><span class="sxs-lookup"><span data-stu-id="8a4ed-129">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="8a4ed-130">Prise en charge du service d’authentification</span><span class="sxs-lookup"><span data-stu-id="8a4ed-130">Authentication service support</span></span>

<span data-ttu-id="8a4ed-131">La prise en charge de l’authentification des utilisateurs est inscrite dans le conteneur de service à l’aide de la méthode d’extension `AddMsalAuthentication` fournie par le package `Microsoft.Authentication.WebAssembly.Msal`.</span><span class="sxs-lookup"><span data-stu-id="8a4ed-131">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="8a4ed-132">Cette méthode configure tous les services requis pour que l’application interagisse avec le fournisseur d’identité (IP).</span><span class="sxs-lookup"><span data-stu-id="8a4ed-132">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="8a4ed-133">*Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="8a4ed-133">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="8a4ed-134">La méthode `AddMsalAuthentication` accepte un rappel pour configurer les paramètres requis pour authentifier une application.</span><span class="sxs-lookup"><span data-stu-id="8a4ed-134">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="8a4ed-135">Les valeurs requises pour la configuration de l’application peuvent être obtenues à partir de la configuration AAD du portail Azure lorsque vous inscrivez l’application.</span><span class="sxs-lookup"><span data-stu-id="8a4ed-135">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="8a4ed-136">Le modèle Blazor webassembly ne configure pas automatiquement l’application pour demander un jeton d’accès pour une API sécurisée.</span><span class="sxs-lookup"><span data-stu-id="8a4ed-136">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="8a4ed-137">Pour approvisionner un jeton dans le cadre du processus de connexion, ajoutez l’étendue aux étendues de jeton d’accès par défaut de la `MsalProviderOptions`:</span><span class="sxs-lookup"><span data-stu-id="8a4ed-137">To provision a token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}");
});
```

> [!NOTE]
> <span data-ttu-id="8a4ed-138">L’étendue du jeton d’accès par défaut doit être au format `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (par exemple, `11111111-1111-1111-1111-111111111111/API.Access`).</span><span class="sxs-lookup"><span data-stu-id="8a4ed-138">The default access token scope must be in the format `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (for example, `11111111-1111-1111-1111-111111111111/API.Access`).</span></span> <span data-ttu-id="8a4ed-139">Si un modèle ou un schéma et un hôte sont fournis au paramètre d’étendue (comme indiqué dans le portail Azure), l' *application cliente* lève une exception non gérée lorsqu’elle reçoit une réponse *non autorisée 401* de l' *application API serveur*.</span><span class="sxs-lookup"><span data-stu-id="8a4ed-139">If a scheme or scheme and host is provided to the scope setting (as shown in the Azure Portal), the *Client app* throws an unhandled exception when it receives a *401 Unauthorized* response from the *Server API app*.</span></span>

## <a name="index-page"></a><span data-ttu-id="8a4ed-140">Page d'index</span><span class="sxs-lookup"><span data-stu-id="8a4ed-140">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

## <a name="app-component"></a><span data-ttu-id="8a4ed-141">Composant d’application</span><span class="sxs-lookup"><span data-stu-id="8a4ed-141">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="8a4ed-142">Composant RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="8a4ed-142">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="8a4ed-143">Composant LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="8a4ed-143">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="8a4ed-144">Composant d’authentification</span><span class="sxs-lookup"><span data-stu-id="8a4ed-144">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="8a4ed-145">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="8a4ed-145">Additional resources</span></span>

* <xref:security/authentication/azure-active-directory/index>
