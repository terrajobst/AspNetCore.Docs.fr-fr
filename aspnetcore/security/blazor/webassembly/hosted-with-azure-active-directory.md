---
title: Sécuriser une application hébergée ASP.NET Core Blazor webassembly avec Azure Active Directory
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory
ms.openlocfilehash: 0803e436d66ef7df3c68739e674a8652fde11166
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083845"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory"></a><span data-ttu-id="a1bf6-102">Sécuriser une application hébergée ASP.NET Core Blazor webassembly avec Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a1bf6-102">Secure an ASP.NET Core Blazor WebAssembly hosted app with Azure Active Directory</span></span>

<span data-ttu-id="a1bf6-103">Par [Javier Calvarro Nelson](https://github.com/javiercn) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a1bf6-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]



<span data-ttu-id="a1bf6-104">Cet article explique comment créer une [application hébergée parBlazor Webassembly](xref:blazor/hosting-models#blazor-webassembly) qui utilise [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) pour l’authentification.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-104">This article describes how to create a [Blazor WebAssembly hosted app](xref:blazor/hosting-models#blazor-webassembly) that uses [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) for authentication.</span></span>

## <a name="register-apps-in-aad-b2c-and-create-solution"></a><span data-ttu-id="a1bf6-105">Inscrire des applications dans AAD B2C et créer une solution</span><span class="sxs-lookup"><span data-stu-id="a1bf6-105">Register apps in AAD B2C and create solution</span></span>

### <a name="create-a-tenant"></a><span data-ttu-id="a1bf6-106">Créer un client</span><span class="sxs-lookup"><span data-stu-id="a1bf6-106">Create a tenant</span></span>

<span data-ttu-id="a1bf6-107">Suivez les instructions de [démarrage rapide : configurer un locataire](/azure/active-directory/develop/quickstart-create-new-tenant) pour créer un locataire dans AAD.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-107">Follow the guidance in [Quickstart: Set up a tenant](/azure/active-directory/develop/quickstart-create-new-tenant) to create a tenant in AAD.</span></span>

### <a name="register-a-server-api-app"></a><span data-ttu-id="a1bf6-108">Inscrire une application API serveur</span><span class="sxs-lookup"><span data-stu-id="a1bf6-108">Register a server API app</span></span>

<span data-ttu-id="a1bf6-109">Suivez les instructions du Guide de [démarrage rapide : inscrire une application auprès de la plateforme Microsoft Identity et les](/azure/active-directory/develop/quickstart-register-app) rubriques Azure AAD suivantes pour inscrire une application AAD pour l' *application API serveur* dans le **Azure Active Directory** > zone **inscriptions d’applications** de la portail Azure :</span><span class="sxs-lookup"><span data-stu-id="a1bf6-109">Follow the guidance in [Quickstart: Register an application with the Microsoft identity platform](/azure/active-directory/develop/quickstart-register-app) and subsequent Azure AAD topics to register an AAD app for the *Server API app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="a1bf6-110">Sélectionnez **Nouvelle inscription**.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-110">Select **New registration**.</span></span>
1. <span data-ttu-id="a1bf6-111">Fournissez un **nom** pour l’application (par exemple, **Blazor Server AAD**).</span><span class="sxs-lookup"><span data-stu-id="a1bf6-111">Provide a **Name** for the app (for example, **Blazor Server AAD**).</span></span>
1. <span data-ttu-id="a1bf6-112">Choisissez un **type de compte pris en charge**.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-112">Choose a **Supported account types**.</span></span> <span data-ttu-id="a1bf6-113">Pour cette expérience, vous pouvez sélectionner des **comptes dans ce répertoire d’organisation uniquement** (un seul locataire).</span><span class="sxs-lookup"><span data-stu-id="a1bf6-113">You may select **Accounts in this organizational directory only** (single tenant) for this experience.</span></span>
1. <span data-ttu-id="a1bf6-114">L' *application API serveur* ne requiert pas d' **URI de redirection** dans ce scénario, laissez la liste déroulante définie sur **Web** et n’entrez pas d’URI de redirection.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-114">The *Server API app* doesn't require a **Redirect URI** in this scenario, so leave the drop down set to **Web** and don't enter a redirect URI.</span></span>
1. <span data-ttu-id="a1bf6-115">Désactivez la case à cocher accorder les \*\*autorisations > \*\* **accorder à l’administrateur de openid et de offline_access** .</span><span class="sxs-lookup"><span data-stu-id="a1bf6-115">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="a1bf6-116">Sélectionnez **Inscription**.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-116">Select **Register**.</span></span>

<span data-ttu-id="a1bf6-117">Dans **autorisations d’API**, supprimez le **Microsoft Graph** > autorisation **User. Read** , car l’application ne nécessite pas d’accès de profil UER ou de connexion.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-117">In **API permissions**, remove the **Microsoft Graph** > **User.Read** permission, as the app doesn't require sign in or uer profile access.</span></span>

<span data-ttu-id="a1bf6-118">Dans **exposer une API**:</span><span class="sxs-lookup"><span data-stu-id="a1bf6-118">In **Expose an API**:</span></span>

1. <span data-ttu-id="a1bf6-119">sélectionner **Ajouter une étendue**.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-119">Select **Add a scope**.</span></span>
1. <span data-ttu-id="a1bf6-120">Sélectionnez **Enregistrer et continuer**.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-120">Select **Save and continue**.</span></span>
1. <span data-ttu-id="a1bf6-121">Spécifiez un **nom d’étendue** (par exemple, `API.Access`).</span><span class="sxs-lookup"><span data-stu-id="a1bf6-121">Provide a **Scope name** (for example, `API.Access`).</span></span>
1. <span data-ttu-id="a1bf6-122">Indiquez un **nom d’affichage du consentement** de l’administrateur (par exemple, `Access API`).</span><span class="sxs-lookup"><span data-stu-id="a1bf6-122">Provide an **Admin consent display name** (for example, `Access API`).</span></span>
1. <span data-ttu-id="a1bf6-123">Fournissez une **Description du consentement** de l’administrateur (par exemple, `Allows the app to access server app API endpoints.`).</span><span class="sxs-lookup"><span data-stu-id="a1bf6-123">Provide an **Admin consent description** (for example, `Allows the app to access server app API endpoints.`).</span></span>
1. <span data-ttu-id="a1bf6-124">Confirmez que l' **État** est défini sur **activé**.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-124">Confirm that the **State** is set to **Enabled**.</span></span>
1. <span data-ttu-id="a1bf6-125">Sélectionnez **Ajouter une étendue**.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-125">Select **Add scope**.</span></span>

<span data-ttu-id="a1bf6-126">Notez les informations suivantes :</span><span class="sxs-lookup"><span data-stu-id="a1bf6-126">Record the following information:</span></span>

* <span data-ttu-id="a1bf6-127">*Application API serveur* ID d’application (ID client) (par exemple, `11111111-1111-1111-1111-111111111111`)</span><span class="sxs-lookup"><span data-stu-id="a1bf6-127">*Server API app* Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="a1bf6-128">ID de répertoire (ID de locataire) (par exemple, `222222222-2222-2222-2222-222222222222`)</span><span class="sxs-lookup"><span data-stu-id="a1bf6-128">Directory ID (Tenant ID) (for example, `222222222-2222-2222-2222-222222222222`)</span></span>
* <span data-ttu-id="a1bf6-129">Domaine du locataire AAD (par exemple, `contoso.onmicrosoft.com`)</span><span class="sxs-lookup"><span data-stu-id="a1bf6-129">AAD Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>
* <span data-ttu-id="a1bf6-130">Étendue par défaut (par exemple, `API.Access`)</span><span class="sxs-lookup"><span data-stu-id="a1bf6-130">Default scope (for example, `API.Access`)</span></span>

### <a name="register-a-client-app"></a><span data-ttu-id="a1bf6-131">Inscrire une application cliente</span><span class="sxs-lookup"><span data-stu-id="a1bf6-131">Register a client app</span></span>

<span data-ttu-id="a1bf6-132">Suivez les instructions du Guide de [démarrage rapide : inscrire une application auprès de la plateforme Microsoft Identity et les](/azure/active-directory/develop/quickstart-register-app) rubriques Azure AAD suivantes pour inscrire une application AAD pour l' *application cliente* dans la zone de **inscriptions d’applications** > **Azure Active Directory** de la portail Azure :</span><span class="sxs-lookup"><span data-stu-id="a1bf6-132">Follow the guidance in [Quickstart: Register an application with the Microsoft identity platform](/azure/active-directory/develop/quickstart-register-app) and subsequent Azure AAD topics to register a AAD app for the *Client app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="a1bf6-133">Sélectionnez **Nouvelle inscription**.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-133">Select **New registration**.</span></span>
1. <span data-ttu-id="a1bf6-134">Fournissez un **nom** pour l’application (par exemple, **Blazor client AAD**).</span><span class="sxs-lookup"><span data-stu-id="a1bf6-134">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span>
1. <span data-ttu-id="a1bf6-135">Choisissez un **type de compte pris en charge**.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-135">Choose a **Supported account types**.</span></span> <span data-ttu-id="a1bf6-136">Pour cette expérience, vous pouvez sélectionner des **comptes dans ce répertoire d’organisation uniquement** (un seul locataire).</span><span class="sxs-lookup"><span data-stu-id="a1bf6-136">You may select **Accounts in this organizational directory only** (single tenant) for this experience.</span></span>
1. <span data-ttu-id="a1bf6-137">Laissez la liste déroulante **URI de redirection** définie sur **Web**et fournissez un URI de redirection de `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-137">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="a1bf6-138">Désactivez la case à cocher accorder les \*\*autorisations > \*\* **accorder à l’administrateur de openid et de offline_access** .</span><span class="sxs-lookup"><span data-stu-id="a1bf6-138">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="a1bf6-139">Sélectionnez **Inscription**.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-139">Select **Register**.</span></span>

<span data-ttu-id="a1bf6-140">Dans > **d’authentification** **configurations de plateforme** > **Web**:</span><span class="sxs-lookup"><span data-stu-id="a1bf6-140">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="a1bf6-141">Confirmez que l' **URI de redirection** de `https://localhost:5001/authentication/login-callback` est présent.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-141">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="a1bf6-142">Pour **octroi implicite**, activez les cases à cocher pour les **jetons d’accès** et les **jetons d’ID**.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-142">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="a1bf6-143">Les valeurs par défaut restantes pour l’application sont acceptables pour cette expérience.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-143">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="a1bf6-144">Sélectionnez le bouton **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-144">Select the **Save** button.</span></span>

<span data-ttu-id="a1bf6-145">Dans **autorisations d’API**:</span><span class="sxs-lookup"><span data-stu-id="a1bf6-145">In **API permissions**:</span></span>

1. <span data-ttu-id="a1bf6-146">Vérifiez que l’application a **Microsoft Graph** > autorisation **User. Read** .</span><span class="sxs-lookup"><span data-stu-id="a1bf6-146">Confirm that the app has **Microsoft Graph** > **User.Read** permission.</span></span>
1. <span data-ttu-id="a1bf6-147">Sélectionnez **Ajouter une autorisation** suivi de **mes API**.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-147">Select **Add a permission** followed by **My APIs**.</span></span>
1. <span data-ttu-id="a1bf6-148">Sélectionnez l' *application API serveur* dans la colonne **nom** (par exemple, **Blazor Server AAD**).</span><span class="sxs-lookup"><span data-stu-id="a1bf6-148">Select the *Server API app* from the **Name** column (for example, **Blazor Server AAD**).</span></span>
1. <span data-ttu-id="a1bf6-149">Ouvrez la liste des **API** .</span><span class="sxs-lookup"><span data-stu-id="a1bf6-149">Open the **API** list.</span></span>
1. <span data-ttu-id="a1bf6-150">Activez l’accès à l’API (par exemple, `API.Access`).</span><span class="sxs-lookup"><span data-stu-id="a1bf6-150">Enable access to the API (for example, `API.Access`).</span></span>
1. <span data-ttu-id="a1bf6-151">Sélectionnez **Ajouter des autorisations**.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-151">Select **Add permissions**.</span></span>
1. <span data-ttu-id="a1bf6-152">Sélectionnez le bouton **Grant admin content for {locataire Name}** .</span><span class="sxs-lookup"><span data-stu-id="a1bf6-152">Select the **Grant admin content for {TENANT NAME}** button.</span></span> <span data-ttu-id="a1bf6-153">Sélectionnez **Oui** pour confirmer.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-153">Select **Yes** to confirm.</span></span>

<span data-ttu-id="a1bf6-154">Enregistrez l’ID de l’application *cliente* (ID client) (par exemple, `33333333-3333-3333-3333-333333333333`).</span><span class="sxs-lookup"><span data-stu-id="a1bf6-154">Record the *Client app* Application ID (Client ID) (for example, `33333333-3333-3333-3333-333333333333`).</span></span>

### <a name="create-the-app"></a><span data-ttu-id="a1bf6-155">Créer l’application</span><span class="sxs-lookup"><span data-stu-id="a1bf6-155">Create the app</span></span>

<span data-ttu-id="a1bf6-156">Remplacez les espaces réservés dans la commande suivante par les informations enregistrées précédemment et exécutez la commande dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="a1bf6-156">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au SingleOrg --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP CLIENT ID}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho --tenant-id "{TENANT ID}"
```

<span data-ttu-id="a1bf6-157">Pour spécifier l’emplacement de sortie, qui crée un dossier de projet s’il n’existe pas, incluez l’option de sortie dans la commande avec un chemin d’accès (par exemple, `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="a1bf6-157">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="a1bf6-158">Le nom du dossier devient également une partie du nom du projet.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-158">The folder name also becomes part of the project's name.</span></span>

> [!NOTE]
> <span data-ttu-id="a1bf6-159">Consultez la section [prise en charge du service d’authentification](#Authentication service support) pour obtenir une modification importante de la configuration de l’étendue du jeton d’accès par défaut.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-159">See the [Authentication service support](#Authentication service support) section for an important configuration change to the default access token scope.</span></span> <span data-ttu-id="a1bf6-160">La valeur fournie par le modèle Blazor webassembly doit être modifiée manuellement une fois que l' *application cliente* a été créée à partir du modèle.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-160">The value provided by the Blazor WebAssembly template must be manually changed after the *Client app* is created from the template.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="a1bf6-161">Configuration de l’application serveur</span><span class="sxs-lookup"><span data-stu-id="a1bf6-161">Server app configuration</span></span>

<span data-ttu-id="a1bf6-162">*Cette section se rapporte à l’application **serveur** de la solution.*</span><span class="sxs-lookup"><span data-stu-id="a1bf6-162">*This section pertains to the solution's **Server** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="a1bf6-163">Package d’authentification</span><span class="sxs-lookup"><span data-stu-id="a1bf6-163">Authentication package</span></span>

<span data-ttu-id="a1bf6-164">La prise en charge de l’authentification et de l’autorisation des appels à ASP.NET Core API Web est assurée par le `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span><span class="sxs-lookup"><span data-stu-id="a1bf6-164">The support for authenticating and authorizing calls to ASP.NET Core Web APIs is provided by the `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
    Version="3.1.0" />
```

### <a name="authentication-service-support"></a><span data-ttu-id="a1bf6-165">Prise en charge du service d’authentification</span><span class="sxs-lookup"><span data-stu-id="a1bf6-165">Authentication service support</span></span>

<span data-ttu-id="a1bf6-166">La méthode `AddAuthentication` définit les services d’authentification au sein de l’application et configure le gestionnaire du porteur JWT comme méthode d’authentification par défaut.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-166">The `AddAuthentication` method sets up authentication services within the app and configures the JWT Bearer handler as the default authentication method.</span></span> <span data-ttu-id="a1bf6-167">La méthode `AddAzureADBearer` définit les paramètres spécifiques dans le gestionnaire du porteur JWT requis pour valider les jetons émis par le Azure Active Directory :</span><span class="sxs-lookup"><span data-stu-id="a1bf6-167">The `AddAzureADBearer` method sets up the specific parameters in the JWT Bearer handler required to validate tokens emitted by the Azure Active Directory:</span></span>

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

<span data-ttu-id="a1bf6-168">`UseAuthentication` et `UseAuthorization` Assurez-vous que :</span><span class="sxs-lookup"><span data-stu-id="a1bf6-168">`UseAuthentication` and `UseAuthorization` ensure that:</span></span>

* <span data-ttu-id="a1bf6-169">L’application tente d’analyser et de valider les jetons sur les demandes entrantes.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-169">The app attempts to parse and validate tokens on incoming requests.</span></span>
* <span data-ttu-id="a1bf6-170">Toute demande d’accès à une ressource protégée sans informations d’identification appropriées échoue.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-170">Any request attempting to access a protected resource without proper credentials fails.</span></span>

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="app-settings"></a><span data-ttu-id="a1bf6-171">Paramètres de l’application</span><span class="sxs-lookup"><span data-stu-id="a1bf6-171">App settings</span></span>

<span data-ttu-id="a1bf6-172">Le fichier *appSettings. JSON* contient les options permettant de configurer le gestionnaire du porteur JWT utilisé pour valider les jetons d’accès.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-172">The *appsettings.json* file contains the options to configure the JWT bearer handler used to validate access tokens.</span></span>

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "{DOMAIN}",
    "TenantId": "{TENANT ID}",
    "ClientId": "{API CLIENT ID}",
  }
}
```

### <a name="weatherforecast-controller"></a><span data-ttu-id="a1bf6-173">Contrôleur WeatherForecast</span><span class="sxs-lookup"><span data-stu-id="a1bf6-173">WeatherForecast controller</span></span>

<span data-ttu-id="a1bf6-174">Le contrôleur WeatherForecast (*Controllers/WeatherForecastController. cs*) expose une API protégée avec l’attribut `[Authorize]` appliqué au contrôleur.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-174">The WeatherForecast controller (*Controllers/WeatherForecastController.cs*) exposes a protected API with the `[Authorize]` attribute applied to the controller.</span></span> <span data-ttu-id="a1bf6-175">Il est **important** de comprendre que :</span><span class="sxs-lookup"><span data-stu-id="a1bf6-175">It's **important** to understand that:</span></span>

* <span data-ttu-id="a1bf6-176">L’attribut `[Authorize]` dans ce contrôleur d’API est la seule chose qui protège cette API contre tout accès non autorisé.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-176">The `[Authorize]` attribute in this API controller is the only thing that protect this API from unauthorized access.</span></span>
* <span data-ttu-id="a1bf6-177">L’attribut `[Authorize]` utilisé dans l’application Blazor webassembly sert uniquement d’indicateur à l’application que l’utilisateur doit être autorisé à utiliser correctement pour l’application.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-177">The `[Authorize]` attribute used in the Blazor WebAssembly app only serves as a hint to the app that the user should be authorized for the app to work correctly.</span></span>

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

## <a name="client-app-configuration"></a><span data-ttu-id="a1bf6-178">Configuration de l’application cliente</span><span class="sxs-lookup"><span data-stu-id="a1bf6-178">Client app configuration</span></span>

<span data-ttu-id="a1bf6-179">*Cette section se rapporte à l’application **cliente** de la solution.*</span><span class="sxs-lookup"><span data-stu-id="a1bf6-179">*This section pertains to the solution's **Client** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="a1bf6-180">Package d’authentification</span><span class="sxs-lookup"><span data-stu-id="a1bf6-180">Authentication package</span></span>

<span data-ttu-id="a1bf6-181">Quand une application est créée pour utiliser des comptes professionnels ou scolaires (`SingleOrg`), l’application reçoit automatiquement une référence de package pour la [bibliothèque d’authentification Microsoft](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span><span class="sxs-lookup"><span data-stu-id="a1bf6-181">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="a1bf6-182">Le package fournit un ensemble de primitives qui aident l’application à authentifier les utilisateurs et à obtenir des jetons pour appeler des API protégées.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-182">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="a1bf6-183">Si vous ajoutez l’authentification à une application, ajoutez manuellement le package au fichier projet de l’application :</span><span class="sxs-lookup"><span data-stu-id="a1bf6-183">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="a1bf6-184">Remplacez `{VERSION}` dans la référence de package précédente par la version du package `Microsoft.AspNetCore.Blazor.Templates` présentée dans l’article <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-184">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="a1bf6-185">Le package `Microsoft.Authentication.WebAssembly.Msal` ajoute transitivement le package `Microsoft.AspNetCore.Components.WebAssembly.Authentication` à l’application.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-185">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

### <a name="authentication-service-support"></a><span data-ttu-id="a1bf6-186">Prise en charge du service d’authentification</span><span class="sxs-lookup"><span data-stu-id="a1bf6-186">Authentication service support</span></span>

<span data-ttu-id="a1bf6-187">La prise en charge de l’authentification des utilisateurs est inscrite dans le conteneur de service à l’aide de la méthode d’extension `AddMsalAuthentication` fournie par le package `Microsoft.Authentication.WebAssembly.Msal`.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-187">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="a1bf6-188">Cette méthode configure tous les services requis pour que l’application interagisse avec le fournisseur d’identité (IP).</span><span class="sxs-lookup"><span data-stu-id="a1bf6-188">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="a1bf6-189">*Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="a1bf6-189">*Program.cs*:</span></span>

<span data-ttu-id="a1bf6-190">Lorsque l' *application cliente* est générée, l’étendue du jeton d’accès par défaut est au format `api://{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}`.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-190">When the *Client app* is generated, the default access token scope is of the format `api://{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}`.</span></span> <span data-ttu-id="a1bf6-191">**Supprimez la partie `api://` de la valeur de portée.**</span><span class="sxs-lookup"><span data-stu-id="a1bf6-191">**Remove the `api://` portion of the scope value.**</span></span> <span data-ttu-id="a1bf6-192">Ce problème sera résolu dans une prochaine version préliminaire.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-192">This issue will be addressed in a future preview release.</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}");
});
```

> [!NOTE]
> <span data-ttu-id="a1bf6-193">L’étendue du jeton d’accès par défaut doit être au format `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (par exemple, `11111111-1111-1111-1111-111111111111/API.Access`).</span><span class="sxs-lookup"><span data-stu-id="a1bf6-193">The default access token scope must be in the format `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (for example, `11111111-1111-1111-1111-111111111111/API.Access`).</span></span> <span data-ttu-id="a1bf6-194">Si un modèle ou un schéma et un hôte sont fournis au paramètre d’étendue (comme indiqué dans le portail Azure), l' *application cliente* lève une exception non gérée lorsqu’elle reçoit une réponse *non autorisée 401* de l' *application API serveur*.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-194">If a scheme or scheme and host is provided to the scope setting (as shown in the Azure Portal), the *Client app* throws an unhandled exception when it receives a *401 Unauthorized* response from the *Server API app*.</span></span>

<span data-ttu-id="a1bf6-195">La méthode `AddMsalAuthentication` accepte un rappel pour configurer les paramètres requis pour authentifier une application.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-195">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="a1bf6-196">Les valeurs requises pour la configuration de l’application peuvent être obtenues à partir de la configuration AAD du portail Azure lorsque vous inscrivez l’application.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-196">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="a1bf6-197">Les étendues de jeton d’accès par défaut représentent la liste des étendues de jeton d’accès qui sont :</span><span class="sxs-lookup"><span data-stu-id="a1bf6-197">The default access token scopes represent the list of access token scopes that are:</span></span>

* <span data-ttu-id="a1bf6-198">Inclus par défaut dans la demande de connexion.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-198">Included by default in the sign in request.</span></span>
* <span data-ttu-id="a1bf6-199">Utilisé pour approvisionner un jeton d’accès immédiatement après l’authentification.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-199">Used to provision an access token immediately after authentication.</span></span>

<span data-ttu-id="a1bf6-200">Toutes les étendues doivent appartenir à la même application par Azure Active Directory règles.</span><span class="sxs-lookup"><span data-stu-id="a1bf6-200">All scopes must belong to the same app per Azure Active Directory rules.</span></span> <span data-ttu-id="a1bf6-201">Des étendues supplémentaires peuvent être ajoutées pour d’autres applications API en fonction des besoins :</span><span class="sxs-lookup"><span data-stu-id="a1bf6-201">Additional scopes can be added for additional API apps as needed:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{SCOPE}");
});
```

### <a name="index-page"></a><span data-ttu-id="a1bf6-202">Page d'index</span><span class="sxs-lookup"><span data-stu-id="a1bf6-202">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

### <a name="app-component"></a><span data-ttu-id="a1bf6-203">Composant d’application</span><span class="sxs-lookup"><span data-stu-id="a1bf6-203">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="a1bf6-204">Composant RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="a1bf6-204">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="a1bf6-205">Composant LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="a1bf6-205">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a><span data-ttu-id="a1bf6-206">Composant d’authentification</span><span class="sxs-lookup"><span data-stu-id="a1bf6-206">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="a1bf6-207">Composant FetchData</span><span class="sxs-lookup"><span data-stu-id="a1bf6-207">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="a1bf6-208">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="a1bf6-208">Additional resources</span></span>

* <xref:security/authentication/azure-active-directory/index>
