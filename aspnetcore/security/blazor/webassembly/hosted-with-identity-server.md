---
title: Sécuriser une application hébergée par l’ASP.NET Core Blazor webassembly avec le serveur d’identité
author: guardrex
description: Pour créer une application hébergée Blazor avec l’authentification dans Visual Studio qui utilise un backend [IdentityServer](https://identityserver.io/)
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-identity-server
ms.openlocfilehash: a3993bf635e5a7aae408d72796015f2414e13c14
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434471"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-identity-server"></a><span data-ttu-id="206b0-103">Sécuriser une application hébergée par l’ASP.NET Core Blazor webassembly avec le serveur d’identité</span><span class="sxs-lookup"><span data-stu-id="206b0-103">Secure an ASP.NET Core Blazor WebAssembly hosted app with Identity Server</span></span>

<span data-ttu-id="206b0-104">Par [Javier Calvarro Nelson](https://github.com/javiercn) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="206b0-104">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="206b0-105">Pour créer une nouvelle Blazor application hébergée dans Visual Studio qui utilise [IdentityServer](https://identityserver.io/) pour authentifier les utilisateurs et les appels d’API :</span><span class="sxs-lookup"><span data-stu-id="206b0-105">To create a new Blazor hosted app in Visual Studio that uses [IdentityServer](https://identityserver.io/) to authenticate users and API calls:</span></span>

1. <span data-ttu-id="206b0-106">Utilisez Visual Studio pour créer une nouvelle **Blazor application Webassembly** .</span><span class="sxs-lookup"><span data-stu-id="206b0-106">Use Visual Studio to create a new **Blazor WebAssembly** app.</span></span> <span data-ttu-id="206b0-107">Pour plus d’informations, consultez <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="206b0-107">For more information, see <xref:blazor/get-started>.</span></span>
1. <span data-ttu-id="206b0-108">Dans la boîte de dialogue **créer une application de Blazor** , sélectionnez **modifier** dans la section **authentification** .</span><span class="sxs-lookup"><span data-stu-id="206b0-108">In the **Create a new Blazor app** dialog, select **Change** in the **Authentication** section.</span></span>
1. <span data-ttu-id="206b0-109">Sélectionnez **des comptes d’utilisateur individuels** , puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="206b0-109">Select **Individual User Accounts** followed by **OK**.</span></span>
1. <span data-ttu-id="206b0-110">Activez la case à cocher **ASP.net Core hébergé** dans la section **avancé** .</span><span class="sxs-lookup"><span data-stu-id="206b0-110">Select the **ASP.NET Core hosted** checkbox in the **Advanced** section.</span></span>
1. <span data-ttu-id="206b0-111">Cliquez sur le bouton **Créer**.</span><span class="sxs-lookup"><span data-stu-id="206b0-111">Select the **Create** button.</span></span>

<span data-ttu-id="206b0-112">Pour créer l’application dans une interface de commande, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="206b0-112">To create the app in a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new blazorwasm -au Individual -ho
```

<span data-ttu-id="206b0-113">Pour spécifier l’emplacement de sortie, qui crée un dossier de projet s’il n’existe pas, incluez l’option de sortie dans la commande avec un chemin d’accès (par exemple, `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="206b0-113">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="206b0-114">Le nom du dossier devient également une partie du nom du projet.</span><span class="sxs-lookup"><span data-stu-id="206b0-114">The folder name also becomes part of the project's name.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="206b0-115">Configuration de l’application serveur</span><span class="sxs-lookup"><span data-stu-id="206b0-115">Server app configuration</span></span>

<span data-ttu-id="206b0-116">Les sections suivantes décrivent les ajouts au projet lorsque la prise en charge de l’authentification est incluse.</span><span class="sxs-lookup"><span data-stu-id="206b0-116">The following sections describe additions to the project when authentication support is included.</span></span>

### <a name="startup-class"></a><span data-ttu-id="206b0-117">Classe de démarrage</span><span class="sxs-lookup"><span data-stu-id="206b0-117">Startup class</span></span>

<span data-ttu-id="206b0-118">La classe `Startup` présente les ajouts suivants :</span><span class="sxs-lookup"><span data-stu-id="206b0-118">The `Startup` class has the following additions:</span></span>

* <span data-ttu-id="206b0-119">Dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="206b0-119">In `Startup.ConfigureServices`:</span></span>

  * <span data-ttu-id="206b0-120">Identité avec l’interface utilisateur par défaut :</span><span class="sxs-lookup"><span data-stu-id="206b0-120">Identity with the default UI:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * <span data-ttu-id="206b0-121">IdentityServer avec une méthode d’assistance <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> supplémentaire qui définit des conventions de ASP.NET Core par défaut par-dessus IdentityServer :</span><span class="sxs-lookup"><span data-stu-id="206b0-121">IdentityServer with an additional <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper method that sets up some default ASP.NET Core conventions on top of IdentityServer:</span></span>

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * <span data-ttu-id="206b0-122">Authentification avec une méthode d’assistance <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> supplémentaire qui configure l’application pour valider les jetons JWT produits par IdentityServer :</span><span class="sxs-lookup"><span data-stu-id="206b0-122">Authentication with an additional <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper method that configures the app to validate JWT tokens produced by IdentityServer:</span></span>

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* <span data-ttu-id="206b0-123">Dans `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="206b0-123">In `Startup.Configure`:</span></span>

  * <span data-ttu-id="206b0-124">Intergiciel d’authentification responsable de la validation des informations d’identification de la demande et de la définition de l’utilisateur dans le contexte de la requête :</span><span class="sxs-lookup"><span data-stu-id="206b0-124">The authentication middleware that is responsible for validating the request credentials and setting the user on the request context:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

  * <span data-ttu-id="206b0-125">L’intergiciel IdentityServer qui expose les points de terminaison Open ID Connect (OIDC) :</span><span class="sxs-lookup"><span data-stu-id="206b0-125">The IdentityServer middleware that exposes the Open ID Connect (OIDC) endpoints:</span></span>

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="206b0-126">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="206b0-126">AddApiAuthorization</span></span>

<span data-ttu-id="206b0-127">La méthode d’assistance <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> configure [IdentityServer](https://identityserver.io/) pour ASP.net Core scénarios.</span><span class="sxs-lookup"><span data-stu-id="206b0-127">The <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper method configures [IdentityServer](https://identityserver.io/) for ASP.NET Core scenarios.</span></span> <span data-ttu-id="206b0-128">IdentityServer est une infrastructure puissante et extensible pour gérer les problèmes de sécurité des applications.</span><span class="sxs-lookup"><span data-stu-id="206b0-128">IdentityServer is a powerful and extensible framework for handling app security concerns.</span></span> <span data-ttu-id="206b0-129">IdentityServer expose une complexité inutile pour les scénarios les plus courants.</span><span class="sxs-lookup"><span data-stu-id="206b0-129">IdentityServer exposes unnecessary complexity for the most common scenarios.</span></span> <span data-ttu-id="206b0-130">Par conséquent, un ensemble de conventions et d’options de configuration est fourni, que nous considérons comme un bon point de départ.</span><span class="sxs-lookup"><span data-stu-id="206b0-130">Consequently, a set of conventions and configuration options is provided that we consider a good starting point.</span></span> <span data-ttu-id="206b0-131">Une fois vos besoins d’authentification modifiés, toute la puissance de IdentityServer est toujours disponible pour personnaliser l’authentification en fonction des exigences d’une application.</span><span class="sxs-lookup"><span data-stu-id="206b0-131">Once your authentication needs change, the full power of IdentityServer is still available to customize authentication to suit an app's requirements.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="206b0-132">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="206b0-132">AddIdentityServerJwt</span></span>

<span data-ttu-id="206b0-133">La méthode d’assistance <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> configure un modèle de stratégie pour l’application en tant que gestionnaire d’authentification par défaut.</span><span class="sxs-lookup"><span data-stu-id="206b0-133">The <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper method configures a policy scheme for the app as the default authentication handler.</span></span> <span data-ttu-id="206b0-134">La stratégie est configurée pour autoriser l’identité à gérer toutes les demandes routées vers un sous-chemin dans l’espace d’URL d’identité `/Identity`.</span><span class="sxs-lookup"><span data-stu-id="206b0-134">The policy is configured to allow Identity to handle all requests routed to any subpath in the Identity URL space `/Identity`.</span></span> <span data-ttu-id="206b0-135">Le <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> gère toutes les autres requêtes.</span><span class="sxs-lookup"><span data-stu-id="206b0-135">The <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> handles all other requests.</span></span> <span data-ttu-id="206b0-136">En outre, cette méthode :</span><span class="sxs-lookup"><span data-stu-id="206b0-136">Additionally, this method:</span></span>

* <span data-ttu-id="206b0-137">Inscrit une ressource API `{APPLICATION NAME}API` avec IdentityServer avec une étendue par défaut de `{APPLICATION NAME}API`.</span><span class="sxs-lookup"><span data-stu-id="206b0-137">Registers an `{APPLICATION NAME}API` API resource with IdentityServer with a default scope of `{APPLICATION NAME}API`.</span></span>
* <span data-ttu-id="206b0-138">Configure l’intergiciel de jeton de porteur JWT pour valider les jetons émis par IdentityServer pour l’application.</span><span class="sxs-lookup"><span data-stu-id="206b0-138">Configures the JWT Bearer Token Middleware to validate tokens issued by IdentityServer for the app.</span></span>

### <a name="weatherforecastcontroller"></a><span data-ttu-id="206b0-139">WeatherForecastController</span><span class="sxs-lookup"><span data-stu-id="206b0-139">WeatherForecastController</span></span>

<span data-ttu-id="206b0-140">Dans le `WeatherForecastController` (*Controllers/WeatherForecastController. cs*), l’attribut [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) est appliqué à la classe.</span><span class="sxs-lookup"><span data-stu-id="206b0-140">In the `WeatherForecastController` (*Controllers/WeatherForecastController.cs*), the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute is applied to the class.</span></span> <span data-ttu-id="206b0-141">L’attribut indique que l’utilisateur doit être autorisé en fonction de la stratégie par défaut pour accéder à la ressource.</span><span class="sxs-lookup"><span data-stu-id="206b0-141">The attribute indicates that the user must be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="206b0-142">La stratégie d’autorisation par défaut est configurée pour utiliser le schéma d’authentification par défaut, qui est configuré par <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> au schéma de stratégie mentionné précédemment.</span><span class="sxs-lookup"><span data-stu-id="206b0-142">The default authorization policy is configured to use the default authentication scheme, which is set up by <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> to the policy scheme that was mentioned earlier.</span></span> <span data-ttu-id="206b0-143">La méthode d’assistance configure <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> en tant que gestionnaire par défaut pour les demandes à l’application.</span><span class="sxs-lookup"><span data-stu-id="206b0-143">The helper method configures <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> as the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="206b0-144">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="206b0-144">ApplicationDbContext</span></span>

<span data-ttu-id="206b0-145">Dans le `ApplicationDbContext` (*Data/ApplicationDbContext. cs*), la même <xref:Microsoft.EntityFrameworkCore.DbContext> est utilisée dans l’identité, à l’exception près qu’elle étend <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> pour inclure le schéma pour IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="206b0-145">In the `ApplicationDbContext` (*Data/ApplicationDbContext.cs*), the same <xref:Microsoft.EntityFrameworkCore.DbContext> is used in Identity with the exception that it extends <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> to include the schema for IdentityServer.</span></span> <span data-ttu-id="206b0-146"><xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> est dérivé de <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>.</span><span class="sxs-lookup"><span data-stu-id="206b0-146"><xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> is derived from <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>.</span></span>

<span data-ttu-id="206b0-147">Pour obtenir le contrôle total du schéma de base de données, héritez de l’une des classes d’identité <xref:Microsoft.EntityFrameworkCore.DbContext> disponibles et configurez le contexte pour inclure le schéma d’identité en appelant `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` dans la méthode `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="206b0-147">To gain full control of the database schema, inherit from one of the available Identity <xref:Microsoft.EntityFrameworkCore.DbContext> classes and configure the context to include the Identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` in the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="206b0-148">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="206b0-148">OidcConfigurationController</span></span>

<span data-ttu-id="206b0-149">Dans le `OidcConfigurationController` (*Controllers/OidcConfigurationController. cs*), le point de terminaison client est approvisionné pour servir les paramètres OIDC.</span><span class="sxs-lookup"><span data-stu-id="206b0-149">In the `OidcConfigurationController` (*Controllers/OidcConfigurationController.cs*), the client endpoint is provisioned to serve OIDC parameters.</span></span>

### <a name="app-settings-files"></a><span data-ttu-id="206b0-150">Fichiers de paramètres d’application</span><span class="sxs-lookup"><span data-stu-id="206b0-150">App settings files</span></span>

<span data-ttu-id="206b0-151">Dans le fichier de paramètres d’application (*appSettings. JSON*) à la racine du projet, la section `IdentityServer` décrit la liste des clients configurés.</span><span class="sxs-lookup"><span data-stu-id="206b0-151">In the app settings file (*appsettings.json*) at the project root, the `IdentityServer` section describes the list of configured clients.</span></span> <span data-ttu-id="206b0-152">Dans l’exemple suivant, il existe un seul client.</span><span class="sxs-lookup"><span data-stu-id="206b0-152">In the following example, there's a single client.</span></span> <span data-ttu-id="206b0-153">Le nom du client correspond au nom de l’application et est mappé par Convention au paramètre `ClientId` OAuth.</span><span class="sxs-lookup"><span data-stu-id="206b0-153">The client name corresponds to the app name and is mapped by convention to the OAuth `ClientId` parameter.</span></span> <span data-ttu-id="206b0-154">Le profil indique le type d’application en cours de configuration.</span><span class="sxs-lookup"><span data-stu-id="206b0-154">The profile indicates the app type being configured.</span></span> <span data-ttu-id="206b0-155">Le profil est utilisé en interne pour générer des conventions qui simplifient le processus de configuration du serveur.</span><span class="sxs-lookup"><span data-stu-id="206b0-155">The profile is used internally to drive conventions that simplify the configuration process for the server.</span></span> <!-- There are several profiles available, as explained in the [Application profiles](#application-profiles) section. -->

```json
"IdentityServer": {
  "Clients": {
    "BlazorApplicationWithAuthentication.Client": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

<span data-ttu-id="206b0-156">Dans le fichier de paramètres d’application de l’environnement de développement (*appSettings. Development. JSON*) à la racine du projet, la section `IdentityServer` décrit la clé utilisée pour signer les jetons.</span><span class="sxs-lookup"><span data-stu-id="206b0-156">In the Development environment app settings file (*appsettings.Development.json*) at the project root, the `IdentityServer` section describes the key used to sign tokens.</span></span> <!-- When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section. -->

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="client-app-configuration"></a><span data-ttu-id="206b0-157">Configuration de l’application cliente</span><span class="sxs-lookup"><span data-stu-id="206b0-157">Client app configuration</span></span>

### <a name="authentication-package"></a><span data-ttu-id="206b0-158">Package d’authentification</span><span class="sxs-lookup"><span data-stu-id="206b0-158">Authentication package</span></span>

<span data-ttu-id="206b0-159">Quand une application est créée pour utiliser des comptes d’utilisateur individuels (`Individual`), l’application reçoit automatiquement une référence de package pour le package `Microsoft.AspNetCore.Components.WebAssembly.Authentication` dans le fichier projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="206b0-159">When an app is created to use Individual User Accounts (`Individual`), the app automatically receives a package reference for the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package in the app's project file.</span></span> <span data-ttu-id="206b0-160">Le package fournit un ensemble de primitives qui aident l’application à authentifier les utilisateurs et à obtenir des jetons pour appeler des API protégées.</span><span class="sxs-lookup"><span data-stu-id="206b0-160">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="206b0-161">Si vous ajoutez l’authentification à une application, ajoutez manuellement le package au fichier projet de l’application :</span><span class="sxs-lookup"><span data-stu-id="206b0-161">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

<span data-ttu-id="206b0-162">Remplacez `{VERSION}` dans la référence de package précédente par la version du package `Microsoft.AspNetCore.Blazor.Templates` présentée dans l’article <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="206b0-162">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

### <a name="api-authorization-support"></a><span data-ttu-id="206b0-163">Prise en charge des autorisations d’API</span><span class="sxs-lookup"><span data-stu-id="206b0-163">API authorization support</span></span>

<span data-ttu-id="206b0-164">La prise en charge de l’authentification des utilisateurs est insérée dans le conteneur de service par la méthode d’extension fournie dans le package `Microsoft.AspNetCore.Components.WebAssembly.Authentication`.</span><span class="sxs-lookup"><span data-stu-id="206b0-164">The support for authenticating users is plugged into the service container by the extension method provided inside the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span> <span data-ttu-id="206b0-165">Cette méthode configure tous les services nécessaires à l’application pour interagir avec le système d’autorisation existant.</span><span class="sxs-lookup"><span data-stu-id="206b0-165">This method sets up all the services needed for the app to interact with the existing authorization system.</span></span>

```csharp
builder.Services.AddApiAuthorization();
```

<span data-ttu-id="206b0-166">Par défaut, il charge la configuration de l’application par Convention à partir de `_configuration/{client-id}`.</span><span class="sxs-lookup"><span data-stu-id="206b0-166">By default, it loads the configuration for the app by convention from `_configuration/{client-id}`.</span></span> <span data-ttu-id="206b0-167">Par Convention, l’ID client est défini sur le nom de l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="206b0-167">By convention, the client ID is set to the app's assembly name.</span></span> <span data-ttu-id="206b0-168">Cette URL peut être modifiée pour pointer vers un point de terminaison distinct en appelant la surcharge avec des options.</span><span class="sxs-lookup"><span data-stu-id="206b0-168">This URL can be changed to point to a separate endpoint by calling the overload with options.</span></span>

### <a name="index-page"></a><span data-ttu-id="206b0-169">Page d'index</span><span class="sxs-lookup"><span data-stu-id="206b0-169">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

### <a name="app-component"></a><span data-ttu-id="206b0-170">Composant d’application</span><span class="sxs-lookup"><span data-stu-id="206b0-170">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="206b0-171">Composant RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="206b0-171">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="206b0-172">Composant LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="206b0-172">LoginDisplay component</span></span>

<span data-ttu-id="206b0-173">Le composant `LoginDisplay` (*Shared/LoginDisplay. Razor*) est rendu dans le composant `MainLayout` (*Shared/MainLayout. Razor*) et gère les comportements suivants :</span><span class="sxs-lookup"><span data-stu-id="206b0-173">The `LoginDisplay` component (*Shared/LoginDisplay.razor*) is rendered in the `MainLayout` component (*Shared/MainLayout.razor*) and manages the following behaviors:</span></span>

* <span data-ttu-id="206b0-174">Pour les utilisateurs authentifiés :</span><span class="sxs-lookup"><span data-stu-id="206b0-174">For authenticated users:</span></span>
  * <span data-ttu-id="206b0-175">Affiche le nom de l’utilisateur actuel.</span><span class="sxs-lookup"><span data-stu-id="206b0-175">Displays the current user name.</span></span>
  * <span data-ttu-id="206b0-176">Propose un lien vers la page de profil utilisateur dans ASP.NET Core identité.</span><span class="sxs-lookup"><span data-stu-id="206b0-176">Offers a link to the user profile page in ASP.NET Core Identity.</span></span>
  * <span data-ttu-id="206b0-177">Offre un bouton permettant de se déconnecter de l’application.</span><span class="sxs-lookup"><span data-stu-id="206b0-177">Offers a button to log out of the app.</span></span>
* <span data-ttu-id="206b0-178">Pour les utilisateurs anonymes :</span><span class="sxs-lookup"><span data-stu-id="206b0-178">For anonymous users:</span></span>
  * <span data-ttu-id="206b0-179">Offre la possibilité de s’inscrire.</span><span class="sxs-lookup"><span data-stu-id="206b0-179">Offers the option to register.</span></span>
  * <span data-ttu-id="206b0-180">Offre la possibilité de se connecter.</span><span class="sxs-lookup"><span data-stu-id="206b0-180">Offers the option to log in.</span></span>

```razor
@using Microsoft.AspNetCore.Components.Authorization
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@inject NavigationManager Navigation
@inject SignOutSessionStateManager SignOutManager

<AuthorizeView>
    <Authorized>
        <a href="authentication/profile">Hello, @context.User.Identity.Name!</a>
        <button class="nav-link btn btn-link" @onclick="BeginSignOut">
            Log out
        </button>
    </Authorized>
    <NotAuthorized>
        <a href="authentication/register">Register</a>
        <a href="authentication/login">Log in</a>
    </NotAuthorized>
</AuthorizeView>

@code {
    private async Task BeginSignOut(MouseEventArgs args)
    {
        await SignOutManager.SetSignOutState();
        Navigation.NavigateTo("authentication/logout");
    }
}
```

### <a name="authentication-component"></a><span data-ttu-id="206b0-181">Composant d’authentification</span><span class="sxs-lookup"><span data-stu-id="206b0-181">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="206b0-182">Composant FetchData</span><span class="sxs-lookup"><span data-stu-id="206b0-182">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="206b0-183">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="206b0-183">Run the app</span></span>

<span data-ttu-id="206b0-184">Exécutez l’application à partir du projet serveur.</span><span class="sxs-lookup"><span data-stu-id="206b0-184">Run the app from the Server project.</span></span> <span data-ttu-id="206b0-185">Quand vous utilisez Visual Studio, sélectionnez le projet serveur dans **Explorateur de solutions** , puis cliquez sur le bouton **exécuter** dans la barre d’outils ou démarrez l’application à partir du menu **Déboguer** .</span><span class="sxs-lookup"><span data-stu-id="206b0-185">When using Visual Studio, select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]
