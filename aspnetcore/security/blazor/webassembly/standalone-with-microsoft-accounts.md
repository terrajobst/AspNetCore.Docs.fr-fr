---
title: Sécuriser une application autonome webassembly Blazor ASP.NET Core avec des comptes Microsoft
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-microsoft-accounts
ms.openlocfilehash: 6883af3486256e7c6905626d8da09e8ae0c4a896
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083824"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-microsoft-accounts"></a><span data-ttu-id="29e2e-102">Sécuriser une application autonome webassembly Blazor ASP.NET Core avec des comptes Microsoft</span><span class="sxs-lookup"><span data-stu-id="29e2e-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Microsoft Accounts</span></span>

<span data-ttu-id="29e2e-103">Par [Javier Calvarro Nelson](https://github.com/javiercn) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="29e2e-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="29e2e-104">Pour créer une application autonome webassembly Blazor qui utilise [des comptes Microsoft avec Azure Active Directory (AAD)](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal) pour l’authentification :</span><span class="sxs-lookup"><span data-stu-id="29e2e-104">To create a Blazor WebAssembly standalone app that uses [Microsoft Accounts with Azure Active Directory (AAD)](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal) for authentication:</span></span>

1. [<span data-ttu-id="29e2e-105">Créer un locataire AAD et une application Web</span><span class="sxs-lookup"><span data-stu-id="29e2e-105">Create an AAD tenant and web application</span></span>](/azure/active-directory/develop/v2-overview)

   <span data-ttu-id="29e2e-106">Inscrire une application AAD dans le **Azure Active Directory** > **inscriptions d’applications** zone du portail Azure :</span><span class="sxs-lookup"><span data-stu-id="29e2e-106">Register a AAD app in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

   <span data-ttu-id="29e2e-107">1 \.</span><span class="sxs-lookup"><span data-stu-id="29e2e-107">1\.</span></span> <span data-ttu-id="29e2e-108">Fournissez un **nom** pour l’application (par exemple, **Blazor client AAD**).</span><span class="sxs-lookup"><span data-stu-id="29e2e-108">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span><br>
   <span data-ttu-id="29e2e-109">2 \.</span><span class="sxs-lookup"><span data-stu-id="29e2e-109">2\.</span></span> <span data-ttu-id="29e2e-110">Dans **types de comptes pris en charge**, sélectionnez **comptes dans n’importe quel annuaire d’organisation**.</span><span class="sxs-lookup"><span data-stu-id="29e2e-110">In **Supported account types**, select **Accounts in any organizational directory**.</span></span><br>
   <span data-ttu-id="29e2e-111">3 \.</span><span class="sxs-lookup"><span data-stu-id="29e2e-111">3\.</span></span> <span data-ttu-id="29e2e-112">Laissez la liste déroulante **URI de redirection** définie sur **Web**et fournissez un URI de redirection de `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="29e2e-112">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span><br>
   <span data-ttu-id="29e2e-113">4 \.</span><span class="sxs-lookup"><span data-stu-id="29e2e-113">4\.</span></span> <span data-ttu-id="29e2e-114">Désactivez la case à cocher accorder les \*\*autorisations > \*\* **accorder à l’administrateur de openid et de offline_access** .</span><span class="sxs-lookup"><span data-stu-id="29e2e-114">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span><br>
   <span data-ttu-id="29e2e-115">5 \.</span><span class="sxs-lookup"><span data-stu-id="29e2e-115">5\.</span></span> <span data-ttu-id="29e2e-116">Sélectionnez **Inscription**.</span><span class="sxs-lookup"><span data-stu-id="29e2e-116">Select **Register**.</span></span>

   <span data-ttu-id="29e2e-117">Dans > **d’authentification** **configurations de plateforme** > **Web**:</span><span class="sxs-lookup"><span data-stu-id="29e2e-117">In **Authentication** > **Platform configurations** > **Web**:</span></span>

   <span data-ttu-id="29e2e-118">1 \.</span><span class="sxs-lookup"><span data-stu-id="29e2e-118">1\.</span></span> <span data-ttu-id="29e2e-119">Confirmez que l' **URI de redirection** de `https://localhost:5001/authentication/login-callback` est présent.</span><span class="sxs-lookup"><span data-stu-id="29e2e-119">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span><br>
   <span data-ttu-id="29e2e-120">2 \.</span><span class="sxs-lookup"><span data-stu-id="29e2e-120">2\.</span></span> <span data-ttu-id="29e2e-121">Pour **octroi implicite**, activez les cases à cocher pour les **jetons d’accès** et les **jetons d’ID**.</span><span class="sxs-lookup"><span data-stu-id="29e2e-121">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span><br>
   <span data-ttu-id="29e2e-122">3 \.</span><span class="sxs-lookup"><span data-stu-id="29e2e-122">3\.</span></span> <span data-ttu-id="29e2e-123">Les valeurs par défaut restantes pour l’application sont acceptables pour cette expérience.</span><span class="sxs-lookup"><span data-stu-id="29e2e-123">The remaining defaults for the app are acceptable for this experience.</span></span><br>
   <span data-ttu-id="29e2e-124">4 \.</span><span class="sxs-lookup"><span data-stu-id="29e2e-124">4\.</span></span> <span data-ttu-id="29e2e-125">Sélectionnez le bouton **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="29e2e-125">Select the **Save** button.</span></span>

   <span data-ttu-id="29e2e-126">Enregistrez l’ID d’application (ID client) (par exemple, `11111111-1111-1111-1111-111111111111`).</span><span class="sxs-lookup"><span data-stu-id="29e2e-126">Record the Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`).</span></span>

1. <span data-ttu-id="29e2e-127">Remplacez les espaces réservés dans la commande suivante par les informations enregistrées précédemment et exécutez la commande dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="29e2e-127">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "common"
   ```

   <span data-ttu-id="29e2e-128">Pour spécifier l’emplacement de sortie, qui crée un dossier de projet s’il n’existe pas, incluez l’option de sortie dans la commande avec un chemin d’accès (par exemple, `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="29e2e-128">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="29e2e-129">Le nom du dossier devient également une partie du nom du projet.</span><span class="sxs-lookup"><span data-stu-id="29e2e-129">The folder name also becomes part of the project's name.</span></span>

<span data-ttu-id="29e2e-130">Après avoir créé l’application, vous devez être en mesure d’effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="29e2e-130">After creating the app, you should be able to:</span></span>

* <span data-ttu-id="29e2e-131">Connectez-vous à l’application à l’aide d’un compte Microsoft.</span><span class="sxs-lookup"><span data-stu-id="29e2e-131">Log into the app using a Microsoft Account.</span></span>
* <span data-ttu-id="29e2e-132">Demandez des jetons d’accès pour les API Microsoft à l’aide de la même approche que pour les applications Blazor autonomes, à condition que vous ayez correctement configuré l’application.</span><span class="sxs-lookup"><span data-stu-id="29e2e-132">Request access tokens for Microsoft APIs using the same approach as for standalone Blazor apps provided that you have configured the app correctly.</span></span> <span data-ttu-id="29e2e-133">Pour plus d’informations, consultez [démarrage rapide : configurer une application pour exposer des API Web](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis).</span><span class="sxs-lookup"><span data-stu-id="29e2e-133">For more information, see [Quickstart: Configure an application to expose web APIs](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis).</span></span>

## <a name="authentication-package"></a><span data-ttu-id="29e2e-134">Package d’authentification</span><span class="sxs-lookup"><span data-stu-id="29e2e-134">Authentication package</span></span>

<span data-ttu-id="29e2e-135">Quand une application est créée pour utiliser des comptes professionnels ou scolaires (`SingleOrg`), l’application reçoit automatiquement une référence de package pour la [bibliothèque d’authentification Microsoft](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span><span class="sxs-lookup"><span data-stu-id="29e2e-135">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="29e2e-136">Le package fournit un ensemble de primitives qui aident l’application à authentifier les utilisateurs et à obtenir des jetons pour appeler des API protégées.</span><span class="sxs-lookup"><span data-stu-id="29e2e-136">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="29e2e-137">Si vous ajoutez l’authentification à une application, ajoutez manuellement le package au fichier projet de l’application :</span><span class="sxs-lookup"><span data-stu-id="29e2e-137">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="29e2e-138">Remplacez `{VERSION}` dans la référence de package précédente par la version du package `Microsoft.AspNetCore.Blazor.Templates` présentée dans l’article <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="29e2e-138">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="29e2e-139">Le package `Microsoft.Authentication.WebAssembly.Msal` ajoute transitivement le package `Microsoft.AspNetCore.Components.WebAssembly.Authentication` à l’application.</span><span class="sxs-lookup"><span data-stu-id="29e2e-139">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="29e2e-140">Prise en charge du service d’authentification</span><span class="sxs-lookup"><span data-stu-id="29e2e-140">Authentication service support</span></span>

<span data-ttu-id="29e2e-141">La prise en charge de l’authentification des utilisateurs est inscrite dans le conteneur de service à l’aide de la méthode d’extension `AddMsalAuthentication` fournie par le package `Microsoft.Authentication.WebAssembly.Msal`.</span><span class="sxs-lookup"><span data-stu-id="29e2e-141">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="29e2e-142">Cette méthode configure tous les services requis pour que l’application interagisse avec le fournisseur d’identité (IP).</span><span class="sxs-lookup"><span data-stu-id="29e2e-142">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="29e2e-143">*Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="29e2e-143">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "{AUTHORITY}";
    authentication.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="29e2e-144">La méthode `AddMsalAuthentication` accepte un rappel pour configurer les paramètres requis pour authentifier une application.</span><span class="sxs-lookup"><span data-stu-id="29e2e-144">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="29e2e-145">Les valeurs requises pour la configuration de l’application peuvent être obtenues à partir de la configuration des comptes Microsoft lorsque vous inscrivez l’application.</span><span class="sxs-lookup"><span data-stu-id="29e2e-145">The values required for configuring the app can be obtained from the Microsoft Accounts configuration when you register the app.</span></span>

## <a name="index-page"></a><span data-ttu-id="29e2e-146">Page d'index</span><span class="sxs-lookup"><span data-stu-id="29e2e-146">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

## <a name="app-component"></a><span data-ttu-id="29e2e-147">Composant d’application</span><span class="sxs-lookup"><span data-stu-id="29e2e-147">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="29e2e-148">Composant RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="29e2e-148">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="29e2e-149">Composant LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="29e2e-149">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="29e2e-150">Composant d’authentification</span><span class="sxs-lookup"><span data-stu-id="29e2e-150">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="29e2e-151">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="29e2e-151">Additional resources</span></span>

* [<span data-ttu-id="29e2e-152">Démarrage rapide : inscrire une application auprès de la plateforme Microsoft Identity</span><span class="sxs-lookup"><span data-stu-id="29e2e-152">Quickstart: Register an application with the Microsoft identity platform</span></span>](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)
* [<span data-ttu-id="29e2e-153">Démarrage rapide : configurer une application pour exposer des API Web</span><span class="sxs-lookup"><span data-stu-id="29e2e-153">Quickstart: Configure an application to expose web APIs</span></span>](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)
