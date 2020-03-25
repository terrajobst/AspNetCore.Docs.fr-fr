---
title: Sécuriser une application hébergée ASP.NET Core Blazor webassembly avec Azure Active Directory B2C
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/22/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory-b2c
ms.openlocfilehash: 0083f179f85371d4751fb179194417681fc1a01d
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219062"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory-b2c"></a><span data-ttu-id="3c7f0-102">Sécuriser une application hébergée ASP.NET Core Blazor webassembly avec Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="3c7f0-102">Secure an ASP.NET Core Blazor WebAssembly hosted app with Azure Active Directory B2C</span></span>

<span data-ttu-id="3c7f0-103">Par [Javier Calvarro Nelson](https://github.com/javiercn) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3c7f0-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="3c7f0-104">Cet article explique comment créer une application autonome webassembly Blazor qui utilise [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) pour l’authentification.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-104">This article describes how to create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) for authentication.</span></span>

## <a name="register-apps-in-aad-b2c-and-create-solution"></a><span data-ttu-id="3c7f0-105">Inscrire des applications dans AAD B2C et créer une solution</span><span class="sxs-lookup"><span data-stu-id="3c7f0-105">Register apps in AAD B2C and create solution</span></span>

### <a name="create-a-tenant"></a><span data-ttu-id="3c7f0-106">Créer un client</span><span class="sxs-lookup"><span data-stu-id="3c7f0-106">Create a tenant</span></span>

<span data-ttu-id="3c7f0-107">Suivez les instructions du [Didacticiel : créer un locataire Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-create-tenant) pour créer un locataire AAD B2C et enregistrer les informations suivantes :</span><span class="sxs-lookup"><span data-stu-id="3c7f0-107">Follow the guidance in [Tutorial: Create an Azure Active Directory B2C tenant](/azure/active-directory-b2c/tutorial-create-tenant) to create an AAD B2C tenant and record the following information:</span></span>

* <span data-ttu-id="3c7f0-108">AAD B2C instance (par exemple, `https://contoso.b2clogin.com/`, qui comprend la barre oblique finale)</span><span class="sxs-lookup"><span data-stu-id="3c7f0-108">AAD B2C instance (for example, `https://contoso.b2clogin.com/`, which includes the trailing slash)</span></span>
* <span data-ttu-id="3c7f0-109">AAD B2C domaine de locataire (par exemple, `contoso.onmicrosoft.com`)</span><span class="sxs-lookup"><span data-stu-id="3c7f0-109">AAD B2C Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>

### <a name="register-a-server-api-app"></a><span data-ttu-id="3c7f0-110">Inscrire une application API serveur</span><span class="sxs-lookup"><span data-stu-id="3c7f0-110">Register a server API app</span></span>

<span data-ttu-id="3c7f0-111">Suivez les instructions du [Didacticiel : inscrire une application dans Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) pour inscrire une application AAD pour l' *application API serveur* dans la zone **Azure Active Directory** > **inscriptions d’applications** de la portail Azure :</span><span class="sxs-lookup"><span data-stu-id="3c7f0-111">Follow the guidance in [Tutorial: Register an application in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) to register an AAD app for the *Server API app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="3c7f0-112">Sélectionnez **Nouvelle inscription**.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-112">Select **New registration**.</span></span>
1. <span data-ttu-id="3c7f0-113">Fournissez un **nom** pour l’application (par exemple, **Blazor Server AAD B2C**).</span><span class="sxs-lookup"><span data-stu-id="3c7f0-113">Provide a **Name** for the app (for example, **Blazor Server AAD B2C**).</span></span>
1. <span data-ttu-id="3c7f0-114">Pour les **types de comptes pris en charge**, sélectionnez les **comptes dans n’importe quel annuaire d’organisation ou n’importe quel fournisseur d’identité. Pour authentifier les utilisateurs avec Azure AD B2C.**</span><span class="sxs-lookup"><span data-stu-id="3c7f0-114">For **Supported account types**, select **Accounts in any organizational directory or any identity provider. For authenticating users with Azure AD B2C.**</span></span> <span data-ttu-id="3c7f0-115">(multi-locataire) pour cette expérience.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-115">(multi-tenant) for this experience.</span></span>
1. <span data-ttu-id="3c7f0-116">L' *application API serveur* ne requiert pas d' **URI de redirection** dans ce scénario, laissez la liste déroulante définie sur **Web** et n’entrez pas d’URI de redirection.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-116">The *Server API app* doesn't require a **Redirect URI** in this scenario, so leave the drop down set to **Web** and don't enter a redirect URI.</span></span>
1. <span data-ttu-id="3c7f0-117">Vérifiez que les **autorisations** > **accorder les autorisations administrateur à OpenID et offline_access** sont activées.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-117">Confirm that **Permissions** > **Grant admin concent to openid and offline_access permissions** is enabled.</span></span>
1. <span data-ttu-id="3c7f0-118">Sélectionnez **Inscription**.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-118">Select **Register**.</span></span>

<span data-ttu-id="3c7f0-119">Dans **exposer une API**:</span><span class="sxs-lookup"><span data-stu-id="3c7f0-119">In **Expose an API**:</span></span>

1. <span data-ttu-id="3c7f0-120">sélectionner **Ajouter une étendue**.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-120">Select **Add a scope**.</span></span>
1. <span data-ttu-id="3c7f0-121">Sélectionnez **Enregistrer et continuer**.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-121">Select **Save and continue**.</span></span>
1. <span data-ttu-id="3c7f0-122">Spécifiez un **nom d’étendue** (par exemple, `API.Access`).</span><span class="sxs-lookup"><span data-stu-id="3c7f0-122">Provide a **Scope name** (for example, `API.Access`).</span></span>
1. <span data-ttu-id="3c7f0-123">Indiquez un **nom d’affichage du consentement** de l’administrateur (par exemple, `Access API`).</span><span class="sxs-lookup"><span data-stu-id="3c7f0-123">Provide an **Admin consent display name** (for example, `Access API`).</span></span>
1. <span data-ttu-id="3c7f0-124">Fournissez une **Description du consentement** de l’administrateur (par exemple, `Allows the app to access server app API endpoints.`).</span><span class="sxs-lookup"><span data-stu-id="3c7f0-124">Provide an **Admin consent description** (for example, `Allows the app to access server app API endpoints.`).</span></span>
1. <span data-ttu-id="3c7f0-125">Confirmez que l' **État** est défini sur **activé**.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-125">Confirm that the **State** is set to **Enabled**.</span></span>
1. <span data-ttu-id="3c7f0-126">Sélectionnez **Ajouter une étendue**.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-126">Select **Add scope**.</span></span>

<span data-ttu-id="3c7f0-127">Notez les informations suivantes :</span><span class="sxs-lookup"><span data-stu-id="3c7f0-127">Record the following information:</span></span>

* <span data-ttu-id="3c7f0-128">*Application API serveur* ID d’application (ID client) (par exemple, `11111111-1111-1111-1111-111111111111`)</span><span class="sxs-lookup"><span data-stu-id="3c7f0-128">*Server API app* Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="3c7f0-129">ID de répertoire (ID de locataire) (par exemple, `222222222-2222-2222-2222-222222222222`)</span><span class="sxs-lookup"><span data-stu-id="3c7f0-129">Directory ID (Tenant ID) (for example, `222222222-2222-2222-2222-222222222222`)</span></span>
* <span data-ttu-id="3c7f0-130">*Application API serveur* URI ID d’application (par exemple, `https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111`, la Portail Azure peut être la valeur par défaut de l’ID client)</span><span class="sxs-lookup"><span data-stu-id="3c7f0-130">*Server API app* App ID URI (for example, `https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111`, the Azure portal might default the value to the Client ID)</span></span>
* <span data-ttu-id="3c7f0-131">Étendue par défaut (par exemple, `API.Access`)</span><span class="sxs-lookup"><span data-stu-id="3c7f0-131">Default scope (for example, `API.Access`)</span></span>

### <a name="register-a-client-app"></a><span data-ttu-id="3c7f0-132">Inscrire une application cliente</span><span class="sxs-lookup"><span data-stu-id="3c7f0-132">Register a client app</span></span>

<span data-ttu-id="3c7f0-133">Suivez les instructions du [Didacticiel : inscrire une application dans Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) à nouveau pour inscrire une application AAD pour l' *application cliente* dans la zone **Azure Active Directory** > **inscriptions d’applications** de la portail Azure :</span><span class="sxs-lookup"><span data-stu-id="3c7f0-133">Follow the guidance in [Tutorial: Register an application in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) again to register an AAD app for the *Client app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="3c7f0-134">Sélectionnez **Nouvelle inscription**.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-134">Select **New registration**.</span></span>
1. <span data-ttu-id="3c7f0-135">Fournissez un **nom** pour l’application (par exemple, **Blazor client AAD B2C**).</span><span class="sxs-lookup"><span data-stu-id="3c7f0-135">Provide a **Name** for the app (for example, **Blazor Client AAD B2C**).</span></span>
1. <span data-ttu-id="3c7f0-136">Pour les **types de comptes pris en charge**, sélectionnez les **comptes dans n’importe quel annuaire d’organisation ou n’importe quel fournisseur d’identité. Pour authentifier les utilisateurs avec Azure AD B2C.**</span><span class="sxs-lookup"><span data-stu-id="3c7f0-136">For **Supported account types**, select **Accounts in any organizational directory or any identity provider. For authenticating users with Azure AD B2C.**</span></span> <span data-ttu-id="3c7f0-137">(multi-locataire) pour cette expérience.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-137">(multi-tenant) for this experience.</span></span>
1. <span data-ttu-id="3c7f0-138">Laissez la liste déroulante **URI de redirection** définie sur **Web**et fournissez un URI de redirection de `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-138">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="3c7f0-139">Vérifiez que les **autorisations** > **accorder les autorisations administrateur à OpenID et offline_access** sont activées.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-139">Confirm that **Permissions** > **Grant admin concent to openid and offline_access permissions** is enabled.</span></span>
1. <span data-ttu-id="3c7f0-140">Sélectionnez **Inscription**.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-140">Select **Register**.</span></span>

<span data-ttu-id="3c7f0-141">Dans > **d’authentification** **configurations de plateforme** > **Web**:</span><span class="sxs-lookup"><span data-stu-id="3c7f0-141">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="3c7f0-142">Confirmez que l' **URI de redirection** de `https://localhost:5001/authentication/login-callback` est présent.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-142">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="3c7f0-143">Pour **octroi implicite**, activez les cases à cocher pour les **jetons d’accès** et les **jetons d’ID**.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-143">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="3c7f0-144">Les valeurs par défaut restantes pour l’application sont acceptables pour cette expérience.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-144">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="3c7f0-145">Sélectionnez le bouton **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-145">Select the **Save** button.</span></span>

<span data-ttu-id="3c7f0-146">Dans **autorisations d’API**:</span><span class="sxs-lookup"><span data-stu-id="3c7f0-146">In **API permissions**:</span></span>

1. <span data-ttu-id="3c7f0-147">Vérifiez que l’application a **Microsoft Graph** > autorisation **User. Read** .</span><span class="sxs-lookup"><span data-stu-id="3c7f0-147">Confirm that the app has **Microsoft Graph** > **User.Read** permission.</span></span>
1. <span data-ttu-id="3c7f0-148">Sélectionnez **Ajouter une autorisation** suivi de **mes API**.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-148">Select **Add a permission** followed by **My APIs**.</span></span>
1. <span data-ttu-id="3c7f0-149">Sélectionnez l' *application API serveur* dans la colonne **nom** (par exemple, **Blazor Server AAD B2C**).</span><span class="sxs-lookup"><span data-stu-id="3c7f0-149">Select the *Server API app* from the **Name** column (for example, **Blazor Server AAD B2C**).</span></span>
1. <span data-ttu-id="3c7f0-150">Ouvrez la liste des **API** .</span><span class="sxs-lookup"><span data-stu-id="3c7f0-150">Open the **API** list.</span></span>
1. <span data-ttu-id="3c7f0-151">Activez l’accès à l’API (par exemple, `API.Access`).</span><span class="sxs-lookup"><span data-stu-id="3c7f0-151">Enable access to the API (for example, `API.Access`).</span></span>
1. <span data-ttu-id="3c7f0-152">Sélectionnez **Ajouter des autorisations**.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-152">Select **Add permissions**.</span></span>
1. <span data-ttu-id="3c7f0-153">Sélectionnez le bouton **Grant admin content for {locataire Name}** .</span><span class="sxs-lookup"><span data-stu-id="3c7f0-153">Select the **Grant admin content for {TENANT NAME}** button.</span></span> <span data-ttu-id="3c7f0-154">Sélectionnez **Oui** pour confirmer.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-154">Select **Yes** to confirm.</span></span>

<span data-ttu-id="3c7f0-155">Dans > d' **hébergement** **Azure ad B2C** > **flux d’utilisateurs**:</span><span class="sxs-lookup"><span data-stu-id="3c7f0-155">In **Home** > **Azure AD B2C** > **User flows**:</span></span>

[<span data-ttu-id="3c7f0-156">Créer un workflow utilisateur d’inscription et de connexion</span><span class="sxs-lookup"><span data-stu-id="3c7f0-156">Create a sign-up and sign-in user flow</span></span>](/azure/active-directory-b2c/tutorial-create-user-flows)

<span data-ttu-id="3c7f0-157">Au minimum, sélectionnez l’attribut d’utilisateur **revendications d’Application** > **nom complet** pour remplir le `context.User.Identity.Name` dans le composant `LoginDisplay` (*Shared/LoginDisplay. Razor*).</span><span class="sxs-lookup"><span data-stu-id="3c7f0-157">At a minimum, select the **Application claims** > **Display Name** user attribute to populate the `context.User.Identity.Name` in the `LoginDisplay` component (*Shared/LoginDisplay.razor*).</span></span>

<span data-ttu-id="3c7f0-158">Notez les informations suivantes :</span><span class="sxs-lookup"><span data-stu-id="3c7f0-158">Record the following information:</span></span>

* <span data-ttu-id="3c7f0-159">Enregistrez l’ID de l’application *cliente* (ID client) (par exemple, `33333333-3333-3333-3333-333333333333`).</span><span class="sxs-lookup"><span data-stu-id="3c7f0-159">Record the *Client app* Application ID (Client ID) (for example, `33333333-3333-3333-3333-333333333333`).</span></span>
* <span data-ttu-id="3c7f0-160">Notez le nom du workflow d’inscription et de connexion de l’utilisateur créé pour l’application (par exemple, `B2C_1_signupsignin`).</span><span class="sxs-lookup"><span data-stu-id="3c7f0-160">Record the sign-up and sign-in user flow name created for the app (for example, `B2C_1_signupsignin`).</span></span>

### <a name="create-the-app"></a><span data-ttu-id="3c7f0-161">Créer l’application</span><span class="sxs-lookup"><span data-stu-id="3c7f0-161">Create the app</span></span>

<span data-ttu-id="3c7f0-162">Remplacez les espaces réservés dans la commande suivante par les informations enregistrées précédemment et exécutez la commande dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="3c7f0-162">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{APP ID URI}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho -ssp "{SIGN UP OR SIGN IN POLICY}" --tenant-id "{TENANT ID}"
```

<span data-ttu-id="3c7f0-163">Pour spécifier l’emplacement de sortie, qui crée un dossier de projet s’il n’existe pas, incluez l’option de sortie dans la commande avec un chemin d’accès (par exemple, `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="3c7f0-163">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="3c7f0-164">Le nom du dossier devient également une partie du nom du projet.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-164">The folder name also becomes part of the project's name.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="3c7f0-165">Configuration de l’application serveur</span><span class="sxs-lookup"><span data-stu-id="3c7f0-165">Server app configuration</span></span>

<span data-ttu-id="3c7f0-166">*Cette section se rapporte à l’application **serveur** de la solution.*</span><span class="sxs-lookup"><span data-stu-id="3c7f0-166">*This section pertains to the solution's **Server** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="3c7f0-167">Package d’authentification</span><span class="sxs-lookup"><span data-stu-id="3c7f0-167">Authentication package</span></span>

<span data-ttu-id="3c7f0-168">La prise en charge de l’authentification et de l’autorisation des appels à ASP.NET Core API Web est assurée par le `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span><span class="sxs-lookup"><span data-stu-id="3c7f0-168">The support for authenticating and authorizing calls to ASP.NET Core Web APIs is provided by the `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
    Version="3.1.0" />
```

### <a name="authentication-service-support"></a><span data-ttu-id="3c7f0-169">Prise en charge du service d’authentification</span><span class="sxs-lookup"><span data-stu-id="3c7f0-169">Authentication service support</span></span>

<span data-ttu-id="3c7f0-170">La méthode `AddAuthentication` définit les services d’authentification au sein de l’application et configure le gestionnaire du porteur JWT comme méthode d’authentification par défaut.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-170">The `AddAuthentication` method sets up authentication services within the app and configures the JWT Bearer handler as the default authentication method.</span></span> <span data-ttu-id="3c7f0-171">La méthode `AddAzureADBearer` définit les paramètres spécifiques dans le gestionnaire du porteur JWT requis pour valider les jetons émis par le Azure Active Directory :</span><span class="sxs-lookup"><span data-stu-id="3c7f0-171">The `AddAzureADBearer` method sets up the specific parameters in the JWT Bearer handler required to validate tokens emitted by the Azure Active Directory:</span></span>

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

<span data-ttu-id="3c7f0-172">`UseAuthentication` et `UseAuthorization` Assurez-vous que :</span><span class="sxs-lookup"><span data-stu-id="3c7f0-172">`UseAuthentication` and `UseAuthorization` ensure that:</span></span>

* <span data-ttu-id="3c7f0-173">L’application tente d’analyser et de valider les jetons sur les demandes entrantes.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-173">The app attempts to parse and validate tokens on incoming requests.</span></span>
* <span data-ttu-id="3c7f0-174">Toute demande d’accès à une ressource protégée sans informations d’identification appropriées échoue.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-174">Any request attempting to access a protected resource without proper credentials fails.</span></span>

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="app-settings"></a><span data-ttu-id="3c7f0-175">Paramètres de l’application</span><span class="sxs-lookup"><span data-stu-id="3c7f0-175">App settings</span></span>

<span data-ttu-id="3c7f0-176">Le fichier *appSettings. JSON* contient les options permettant de configurer le gestionnaire du porteur JWT utilisé pour valider les jetons d’accès.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-176">The *appsettings.json* file contains the options to configure the JWT bearer handler used to validate access tokens.</span></span>

```json
{
  "AzureAd": {
    "Instance": "https://{ORGANIZATION}.b2clogin.com/",
    "ClientId": "{API CLIENT ID}",
    "Domain": "{DOMAIN}",
    "SignUpSignInPolicyId": "{SIGN UP OR SIGN IN POLICY}"
  }
}
```

### <a name="weatherforecast-controller"></a><span data-ttu-id="3c7f0-177">Contrôleur WeatherForecast</span><span class="sxs-lookup"><span data-stu-id="3c7f0-177">WeatherForecast controller</span></span>

<span data-ttu-id="3c7f0-178">Le contrôleur WeatherForecast (*Controllers/WeatherForecastController. cs*) expose une API protégée avec l’attribut `[Authorize]` appliqué au contrôleur.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-178">The WeatherForecast controller (*Controllers/WeatherForecastController.cs*) exposes a protected API with the `[Authorize]` attribute applied to the controller.</span></span> <span data-ttu-id="3c7f0-179">Il est **important** de comprendre que :</span><span class="sxs-lookup"><span data-stu-id="3c7f0-179">It's **important** to understand that:</span></span>

* <span data-ttu-id="3c7f0-180">L’attribut `[Authorize]` dans ce contrôleur d’API est la seule chose qui protège cette API contre tout accès non autorisé.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-180">The `[Authorize]` attribute in this API controller is the only thing that protect this API from unauthorized access.</span></span>
* <span data-ttu-id="3c7f0-181">L’attribut `[Authorize]` utilisé dans l’application Blazor webassembly sert uniquement d’indicateur à l’application que l’utilisateur doit être autorisé à utiliser correctement pour l’application.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-181">The `[Authorize]` attribute used in the Blazor WebAssembly app only serves as a hint to the app that the user should be authorized for the app to work correctly.</span></span>

```csharp
[Authorize]
[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    [HttpGet]
    public IEnumerable<WeatherForecast> Get()
    {
        ...
    }
}
```

## <a name="client-app-configuration"></a><span data-ttu-id="3c7f0-182">Configuration de l’application cliente</span><span class="sxs-lookup"><span data-stu-id="3c7f0-182">Client app configuration</span></span>

<span data-ttu-id="3c7f0-183">*Cette section se rapporte à l’application **cliente** de la solution.*</span><span class="sxs-lookup"><span data-stu-id="3c7f0-183">*This section pertains to the solution's **Client** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="3c7f0-184">Package d’authentification</span><span class="sxs-lookup"><span data-stu-id="3c7f0-184">Authentication package</span></span>

<span data-ttu-id="3c7f0-185">Quand une application est créée pour utiliser un compte B2C (`IndividualB2C`) individuel, l’application reçoit automatiquement une référence de package pour la [bibliothèque d’authentification Microsoft](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span><span class="sxs-lookup"><span data-stu-id="3c7f0-185">When an app is created to use an Individual B2C Account (`IndividualB2C`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="3c7f0-186">Le package fournit un ensemble de primitives qui aident l’application à authentifier les utilisateurs et à obtenir des jetons pour appeler des API protégées.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-186">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="3c7f0-187">Si vous ajoutez l’authentification à une application, ajoutez manuellement le package au fichier projet de l’application :</span><span class="sxs-lookup"><span data-stu-id="3c7f0-187">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="3c7f0-188">Remplacez `{VERSION}` dans la référence de package précédente par la version du package `Microsoft.AspNetCore.Blazor.Templates` présentée dans l’article <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-188">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="3c7f0-189">Le package `Microsoft.Authentication.WebAssembly.Msal` ajoute transitivement le package `Microsoft.AspNetCore.Components.WebAssembly.Authentication` à l’application.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-189">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

### <a name="authentication-service-support"></a><span data-ttu-id="3c7f0-190">Prise en charge du service d’authentification</span><span class="sxs-lookup"><span data-stu-id="3c7f0-190">Authentication service support</span></span>

<span data-ttu-id="3c7f0-191">La prise en charge de l’authentification des utilisateurs est inscrite dans le conteneur de service à l’aide de la méthode d’extension `AddMsalAuthentication` fournie par le package `Microsoft.Authentication.WebAssembly.Msal`.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-191">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="3c7f0-192">Cette méthode configure tous les services requis pour que l’application interagisse avec le fournisseur d’identité (IP).</span><span class="sxs-lookup"><span data-stu-id="3c7f0-192">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="3c7f0-193">*Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="3c7f0-193">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = 
        "{AAD B2C INSTANCE}{DOMAIN}/{SIGN UP OR SIGN IN POLICY}";
    authentication.ClientId = "{CLIENT ID}";
    authentication.ValidateAuthority = false;
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{APP ID URI}/{DEFAULT SCOPE}");
});
```

<span data-ttu-id="3c7f0-194">La méthode `AddMsalAuthentication` accepte un rappel pour configurer les paramètres requis pour authentifier une application.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-194">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="3c7f0-195">Les valeurs requises pour la configuration de l’application peuvent être obtenues à partir de la configuration AAD du portail Azure lorsque vous inscrivez l’application.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-195">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="3c7f0-196">Le modèle Blazor webassembly configure automatiquement l’application pour demander un jeton d’accès pour une API sécurisée pour l’étendue par défaut fournie à la commande `dotnet new` (`{APP ID URI}/{DEFAULT SCOPE}`).</span><span class="sxs-lookup"><span data-stu-id="3c7f0-196">The Blazor WebAssembly template automatically configures the app to request an access token for a secure API for the default scope provided to the `dotnet new` command (`{APP ID URI}/{DEFAULT SCOPE}`).</span></span>

<span data-ttu-id="3c7f0-197">Les étendues de jeton d’accès par défaut représentent la liste des étendues de jeton d’accès qui sont :</span><span class="sxs-lookup"><span data-stu-id="3c7f0-197">The default access token scopes represent the list of access token scopes that are:</span></span>

* <span data-ttu-id="3c7f0-198">Inclus par défaut dans la demande de connexion.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-198">Included by default in the sign in request.</span></span>
* <span data-ttu-id="3c7f0-199">Utilisé pour approvisionner un jeton d’accès immédiatement après l’authentification.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-199">Used to provision an access token immediately after authentication.</span></span>

<span data-ttu-id="3c7f0-200">Toutes les étendues doivent appartenir à la même application par Azure Active Directory règles.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-200">All scopes must belong to the same app per Azure Active Directory rules.</span></span> <span data-ttu-id="3c7f0-201">Des étendues supplémentaires peuvent être ajoutées pour d’autres applications API en fonction des besoins :</span><span class="sxs-lookup"><span data-stu-id="3c7f0-201">Additional scopes can be added for additional API apps as needed:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{APP ID URI}/{SCOPE}");
});
```

### <a name="index-page"></a><span data-ttu-id="3c7f0-202">Page d'index</span><span class="sxs-lookup"><span data-stu-id="3c7f0-202">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

### <a name="app-component"></a><span data-ttu-id="3c7f0-203">Composant d’application</span><span class="sxs-lookup"><span data-stu-id="3c7f0-203">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="3c7f0-204">Composant RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="3c7f0-204">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="3c7f0-205">Composant LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="3c7f0-205">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a><span data-ttu-id="3c7f0-206">Composant d’authentification</span><span class="sxs-lookup"><span data-stu-id="3c7f0-206">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="3c7f0-207">Composant FetchData</span><span class="sxs-lookup"><span data-stu-id="3c7f0-207">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="3c7f0-208">Exécuter l’application</span><span class="sxs-lookup"><span data-stu-id="3c7f0-208">Run the app</span></span>

<span data-ttu-id="3c7f0-209">Exécutez l’application à partir du projet serveur.</span><span class="sxs-lookup"><span data-stu-id="3c7f0-209">Run the app from the Server project.</span></span> <span data-ttu-id="3c7f0-210">Quand vous utilisez Visual Studio, sélectionnez le projet serveur dans **Explorateur de solutions** , puis cliquez sur le bouton **exécuter** dans la barre d’outils ou démarrez l’application à partir du menu **Déboguer** .</span><span class="sxs-lookup"><span data-stu-id="3c7f0-210">When using Visual Studio, select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="3c7f0-211">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="3c7f0-211">Additional resources</span></span>

* <xref:security/authentication/azure-ad-b2c>
* [<span data-ttu-id="3c7f0-212">Didacticiel : créer un locataire Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="3c7f0-212">Tutorial: Create an Azure Active Directory B2C tenant</span></span>](/azure/active-directory-b2c/tutorial-create-tenant)
