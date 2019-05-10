---
title: Migrer d’authentification et identité vers ASP.NET Core 2.0
author: scottaddie
description: Cet article décrit les étapes les plus courantes pour migration ASP.NET Core 1.x l’authentification et identité vers ASP.NET Core 2.0.
ms.author: scaddie
ms.date: 12/18/2018
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: d11d41c82236436096660a24df81a3df4da0fb8e
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892206"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="86189-103">Migrer d’authentification et identité vers ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="86189-103">Migrate authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="86189-104">Par [Scott Addie](https://github.com/scottaddie) et [arts martiaux Hao](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="86189-104">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="86189-105">ASP.NET Core 2.0 offre un nouveau modèle pour l’authentification et [identité](xref:security/authentication/identity) qui simplifie la configuration à l’aide des services.</span><span class="sxs-lookup"><span data-stu-id="86189-105">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) which simplifies configuration by using services.</span></span> <span data-ttu-id="86189-106">Les applications ASP.NET Core 1.x qui utilisent l’authentification ou Identity peuvent être mis à jour pour utiliser le nouveau modèle, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="86189-106">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="86189-107">Intergiciel (middleware) d’authentification et services</span><span class="sxs-lookup"><span data-stu-id="86189-107">Authentication Middleware and services</span></span>

<span data-ttu-id="86189-108">Dans les projets 1.x, l’authentification est configurée via l’intergiciel.</span><span class="sxs-lookup"><span data-stu-id="86189-108">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="86189-109">Une méthode de l’intergiciel (middleware) est appelée pour chaque schéma d’authentification que vous souhaitez prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="86189-109">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="86189-110">L’exemple de 1.x suivant configure l’authentification Facebook avec l’identité dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="86189-110">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.UseIdentity();
    app.UseFacebookAuthentication(new FacebookOptions {
        AppId = Configuration["auth:facebook:appid"],
        AppSecret = Configuration["auth:facebook:appsecret"]
    });
}
```

<span data-ttu-id="86189-111">Dans les 2.0 projets, l’authentification est configurée par le biais de services.</span><span class="sxs-lookup"><span data-stu-id="86189-111">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="86189-112">Chaque schéma d’authentification est enregistré dans le `ConfigureServices` méthode de *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="86189-112">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="86189-113">Le `UseIdentity` méthode est remplacée par `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="86189-113">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="86189-114">L’exemple suivant 2.0 configure l’authentification Facebook avec l’identité dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="86189-114">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();

    // If you want to tweak Identity cookies, they're no longer part of IdentityOptions.
    services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory) {
    app.UseAuthentication();
}
```

<span data-ttu-id="86189-115">Le `UseAuthentication` méthode ajoute un composant d’intergiciel (middleware) d’authentification unique qui est responsable de l’authentification automatique et la gestion des demandes d’authentification à distance.</span><span class="sxs-lookup"><span data-stu-id="86189-115">The `UseAuthentication` method adds a single authentication middleware component which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="86189-116">Il remplace tous les composants d’intergiciel (middleware) individuels par un composant d’intergiciel (middleware) unique et commun.</span><span class="sxs-lookup"><span data-stu-id="86189-116">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="86189-117">Vous trouverez ci-dessous des instructions de migration 2.0 pour chaque schéma d’authentification principale.</span><span class="sxs-lookup"><span data-stu-id="86189-117">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="86189-118">Authentification basée sur les cookies</span><span class="sxs-lookup"><span data-stu-id="86189-118">Cookie-based authentication</span></span>

<span data-ttu-id="86189-119">Sélectionnez une des deux options ci-dessous et apportez les modifications nécessaires dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="86189-119">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="86189-120">Utilisation des cookies avec l’identité</span><span class="sxs-lookup"><span data-stu-id="86189-120">Use cookies with Identity</span></span>
    - <span data-ttu-id="86189-121">Remplacez `UseIdentity` avec `UseAuthentication` dans le `Configure` méthode :</span><span class="sxs-lookup"><span data-stu-id="86189-121">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="86189-122">Appeler le `AddIdentity` méthode dans le `ConfigureServices` méthode pour ajouter les services d’authentification de cookie.</span><span class="sxs-lookup"><span data-stu-id="86189-122">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="86189-123">Si vous le souhaitez, appelez le `ConfigureApplicationCookie` ou `ConfigureExternalCookie` méthode dans le `ConfigureServices` méthode pour modifier les paramètres de cookies d’identité.</span><span class="sxs-lookup"><span data-stu-id="86189-123">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="86189-124">Utilisation des cookies sans Identity</span><span class="sxs-lookup"><span data-stu-id="86189-124">Use cookies without Identity</span></span>
    - <span data-ttu-id="86189-125">Remplacez le `UseCookieAuthentication` appel de méthode dans le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="86189-125">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="86189-126">Appeler le `AddAuthentication` et `AddCookie` méthodes dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="86189-126">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

        ```csharp
        // If you don't want the cookie to be automatically authenticated and assigned to HttpContext.User,
        // remove the CookieAuthenticationDefaults.AuthenticationScheme parameter passed to AddAuthentication.
        services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
                .AddCookie(options =>
                {
                    options.LoginPath = "/Account/LogIn";
                    options.LogoutPath = "/Account/LogOff";
                });
        ```

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="86189-127">Authentification de porteur JWT</span><span class="sxs-lookup"><span data-stu-id="86189-127">JWT Bearer Authentication</span></span>

<span data-ttu-id="86189-128">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="86189-128">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="86189-129">Remplacez le `UseJwtBearerAuthentication` appel de méthode dans le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="86189-129">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="86189-130">Appeler le `AddJwtBearer` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="86189-130">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="86189-131">Cet extrait de code n’utilise pas l’identité, le schéma par défaut doivent donc être défini en passant `JwtBearerDefaults.AuthenticationScheme` à la `AddAuthentication` (méthode).</span><span class="sxs-lookup"><span data-stu-id="86189-131">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="86189-132">Authentification OpenID Connect (OIDC)</span><span class="sxs-lookup"><span data-stu-id="86189-132">OpenID Connect (OIDC) authentication</span></span>

<span data-ttu-id="86189-133">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="86189-133">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="86189-134">Remplacez le `UseOpenIdConnectAuthentication` appel de méthode dans le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="86189-134">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="86189-135">Appeler le `AddOpenIdConnect` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="86189-135">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(options =>
    {
        options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options =>
    {
        options.Authority = Configuration["auth:oidc:authority"];
        options.ClientId = Configuration["auth:oidc:clientid"];
    });
    ```

### <a name="facebook-authentication"></a><span data-ttu-id="86189-136">Authentification Facebook</span><span class="sxs-lookup"><span data-stu-id="86189-136">Facebook authentication</span></span>

<span data-ttu-id="86189-137">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="86189-137">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="86189-138">Remplacez le `UseFacebookAuthentication` appel de méthode dans le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="86189-138">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="86189-139">Appeler le `AddFacebook` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="86189-139">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="86189-140">Authentification Google</span><span class="sxs-lookup"><span data-stu-id="86189-140">Google authentication</span></span>

<span data-ttu-id="86189-141">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="86189-141">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="86189-142">Remplacez le `UseGoogleAuthentication` appel de méthode dans le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="86189-142">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="86189-143">Appeler le `AddGoogle` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="86189-143">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="86189-144">Authentification de Microsoft Account</span><span class="sxs-lookup"><span data-stu-id="86189-144">Microsoft Account authentication</span></span>

<span data-ttu-id="86189-145">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="86189-145">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="86189-146">Remplacez le `UseMicrosoftAccountAuthentication` appel de méthode dans le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="86189-146">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="86189-147">Appeler le `AddMicrosoftAccount` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="86189-147">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a><span data-ttu-id="86189-148">Authentification Twitter</span><span class="sxs-lookup"><span data-stu-id="86189-148">Twitter authentication</span></span>

<span data-ttu-id="86189-149">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="86189-149">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="86189-150">Remplacez le `UseTwitterAuthentication` appel de méthode dans le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="86189-150">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="86189-151">Appeler le `AddTwitter` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="86189-151">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="86189-152">Définition des schémas d’authentification par défaut</span><span class="sxs-lookup"><span data-stu-id="86189-152">Setting default authentication schemes</span></span>

<span data-ttu-id="86189-153">Dans la version 1.x, le `AutomaticAuthenticate` et `AutomaticChallenge` propriétés de la [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) classe de base ont été conçue pour être définie sur un schéma d’authentification unique.</span><span class="sxs-lookup"><span data-stu-id="86189-153">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="86189-154">Il n’a aucune bonne façon d’appliquer cette recommandation.</span><span class="sxs-lookup"><span data-stu-id="86189-154">There was no good way to enforce this.</span></span>

<span data-ttu-id="86189-155">Dans la version 2.0, ces deux propriétés ont été supprimées en tant que propriétés sur chaque `AuthenticationOptions` instance.</span><span class="sxs-lookup"><span data-stu-id="86189-155">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="86189-156">Ils peuvent être configurés dans le `AddAuthentication` appel de méthode dans le `ConfigureServices` méthode de *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="86189-156">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="86189-157">Dans l’extrait de code précédent, le schéma par défaut est défini sur `CookieAuthenticationDefaults.AuthenticationScheme` (« Cookies »).</span><span class="sxs-lookup"><span data-stu-id="86189-157">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="86189-158">Vous pouvez également utiliser une version surchargée de la `AddAuthentication` pour définir plusieurs propriétés.</span><span class="sxs-lookup"><span data-stu-id="86189-158">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="86189-159">Dans l’exemple suivant de la méthode surchargée, le schéma par défaut a la valeur `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="86189-159">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="86189-160">Le schéma d’authentification peut également être spécifié dans votre personne `[Authorize]` attributs ou des stratégies d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="86189-160">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="86189-161">Définir un schéma par défaut dans 2.0 si une des conditions suivantes est remplie :</span><span class="sxs-lookup"><span data-stu-id="86189-161">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="86189-162">Vous souhaitez que l’utilisateur de se connecter automatiquement</span><span class="sxs-lookup"><span data-stu-id="86189-162">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="86189-163">Vous utilisez le `[Authorize]` stratégies d’attribut ou d’autorisation sans spécifier de schémas</span><span class="sxs-lookup"><span data-stu-id="86189-163">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="86189-164">Une exception à cette règle est la `AddIdentity` (méthode).</span><span class="sxs-lookup"><span data-stu-id="86189-164">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="86189-165">Cette méthode ajoute des cookies pour vous et définit la valeur par défaut s’authentifier et défiez les schémas au cookie application `IdentityConstants.ApplicationScheme`.</span><span class="sxs-lookup"><span data-stu-id="86189-165">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="86189-166">En outre, il définit le schéma de connexion par défaut sur le cookie externe `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="86189-166">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="86189-167">Utiliser des extensions d’authentification HttpContext</span><span class="sxs-lookup"><span data-stu-id="86189-167">Use HttpContext authentication extensions</span></span>

<span data-ttu-id="86189-168">Le `IAuthenticationManager` interface est le point d’entrée principal dans le système d’authentification 1.x.</span><span class="sxs-lookup"><span data-stu-id="86189-168">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="86189-169">Il a été remplacé par un nouvel ensemble de `HttpContext` méthodes d’extension dans le `Microsoft.AspNetCore.Authentication` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="86189-169">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="86189-170">Par exemple, les projets 1.x référence un `Authentication` propriété :</span><span class="sxs-lookup"><span data-stu-id="86189-170">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="86189-171">Dans les 2.0 projets, vous devez importer le `Microsoft.AspNetCore.Authentication` espace de noms et supprimer le `Authentication` références de propriété :</span><span class="sxs-lookup"><span data-stu-id="86189-171">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="86189-172">L’authentification Windows (HTTP.sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="86189-172">Windows Authentication (HTTP.sys / IISIntegration)</span></span>

<span data-ttu-id="86189-173">Il existe deux variantes de l’authentification Windows :</span><span class="sxs-lookup"><span data-stu-id="86189-173">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="86189-174">L’hôte autorise uniquement les utilisateurs authentifiés</span><span class="sxs-lookup"><span data-stu-id="86189-174">The host only allows authenticated users</span></span>
2. <span data-ttu-id="86189-175">L’hôte permet à la fois anonymes et les utilisateurs authentifiés</span><span class="sxs-lookup"><span data-stu-id="86189-175">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="86189-176">La première variante décrite ci-dessus n’est pas affectée par les modifications de 2.0.</span><span class="sxs-lookup"><span data-stu-id="86189-176">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="86189-177">La deuxième variante décrite ci-dessus est affectée par les modifications de 2.0.</span><span class="sxs-lookup"><span data-stu-id="86189-177">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="86189-178">Par exemple, vous autorisez peut-être les utilisateurs anonymes dans votre application à IIS ou [HTTP.sys](xref:fundamentals/servers/httpsys) mais autorisant des utilisateurs au niveau du contrôleur de couche.</span><span class="sxs-lookup"><span data-stu-id="86189-178">As an example, you may be allowing anonymous users into your app at the IIS or [HTTP.sys](xref:fundamentals/servers/httpsys) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="86189-179">Dans ce scénario, la valeur est le schéma par défaut `IISDefaults.AuthenticationScheme` dans le `Startup.ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="86189-179">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="86189-180">Impossible de définir le schéma par défaut empêche en conséquence la demande authorize à issu du travail.</span><span class="sxs-lookup"><span data-stu-id="86189-180">Failure to set the default scheme accordingly prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="86189-181">Instances de IdentityCookieOptions</span><span class="sxs-lookup"><span data-stu-id="86189-181">IdentityCookieOptions instances</span></span>

<span data-ttu-id="86189-182">Un effet secondaire des 2.0 modifications est le paramètre permet à l’aide de ces options au lieu d’instances d’options de cookie.</span><span class="sxs-lookup"><span data-stu-id="86189-182">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="86189-183">La possibilité de personnaliser les noms de schéma identité cookie est supprimée.</span><span class="sxs-lookup"><span data-stu-id="86189-183">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="86189-184">Par exemple, des projets 1.x utilisent [l’injection de constructeur](xref:mvc/controllers/dependency-injection#constructor-injection) à passer un `IdentityCookieOptions` paramètre dans *AccountController.cs*.</span><span class="sxs-lookup"><span data-stu-id="86189-184">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs*.</span></span> <span data-ttu-id="86189-185">Le schéma d’authentification externe cookie est accessible à partir de l’instance fournie :</span><span class="sxs-lookup"><span data-stu-id="86189-185">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="86189-186">L’injection de constructeur mentionnés ci-dessus devienne inutile dans les 2.0 projets et le `_externalCookieScheme` champ peut être supprimé :</span><span class="sxs-lookup"><span data-stu-id="86189-186">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="86189-187">Le `IdentityConstants.ExternalScheme` constante peut être utilisée directement :</span><span class="sxs-lookup"><span data-stu-id="86189-187">The `IdentityConstants.ExternalScheme` constant can be used directly:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="86189-188">Ajouter des propriétés de navigation IdentityUser POCO</span><span class="sxs-lookup"><span data-stu-id="86189-188">Add IdentityUser POCO navigation properties</span></span>

<span data-ttu-id="86189-189">Les propriétés de navigation d’Entity Framework (EF) Core de la base de `IdentityUser` POCO (Plain Old CLR Object) ont été supprimés.</span><span class="sxs-lookup"><span data-stu-id="86189-189">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="86189-190">Si votre projet 1.x utilisé ces propriétés, les ajouter manuellement au projet 2.0 :</span><span class="sxs-lookup"><span data-stu-id="86189-190">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

```csharp
/// <summary>
/// Navigation property for the roles this user belongs to.
/// </summary>
public virtual ICollection<IdentityUserRole<int>> Roles { get; } = new List<IdentityUserRole<int>>();

/// <summary>
/// Navigation property for the claims this user possesses.
/// </summary>
public virtual ICollection<IdentityUserClaim<int>> Claims { get; } = new List<IdentityUserClaim<int>>();

/// <summary>
/// Navigation property for this users login accounts.
/// </summary>
public virtual ICollection<IdentityUserLogin<int>> Logins { get; } = new List<IdentityUserLogin<int>>();
```

<span data-ttu-id="86189-191">Pour empêcher les clés étrangères en double lors de l’exécution des Migrations d’EF Core, ajoutez le code suivant à votre `IdentityDbContext` classe `OnModelCreating` (méthode) (une fois que le `base.OnModelCreating();` appeler) :</span><span class="sxs-lookup"><span data-stu-id="86189-191">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Core Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Core Identity table names and more.
    // Add your customizations after calling base.OnModelCreating(builder);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Claims)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Logins)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Roles)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);
}
```

<a name="synchronous-method-removal"></a>

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="86189-192">Remplacez GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="86189-192">Replace GetExternalAuthenticationSchemes</span></span>

<span data-ttu-id="86189-193">La méthode synchrone `GetExternalAuthenticationSchemes` a été supprimé en faveur d’une version asynchrone.</span><span class="sxs-lookup"><span data-stu-id="86189-193">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="86189-194">les projets 1.x ont le code suivant *ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="86189-194">1.x projects have the following code in *ManageController.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="86189-195">Cette méthode s’affiche dans *Login.cshtml* trop :</span><span class="sxs-lookup"><span data-stu-id="86189-195">This method appears in *Login.cshtml* too:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

<span data-ttu-id="86189-196">Dans les 2.0 projets, utilisez la `GetExternalAuthenticationSchemesAsync` méthode :</span><span class="sxs-lookup"><span data-stu-id="86189-196">In 2.0 projects, use the `GetExternalAuthenticationSchemesAsync` method:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="86189-197">Dans *Login.cshtml*, le `AuthenticationScheme` propriété accédée dans le `foreach` boucle devient `Name`:</span><span class="sxs-lookup"><span data-stu-id="86189-197">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="86189-198">Modification de propriété ManageLoginsViewModel</span><span class="sxs-lookup"><span data-stu-id="86189-198">ManageLoginsViewModel property change</span></span>

<span data-ttu-id="86189-199">Un `ManageLoginsViewModel` objet est utilisé dans le `ManageLogins` action de *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="86189-199">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="86189-200">Dans les projets 1.x, l’objet `OtherLogins` propriété type de retour est `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="86189-200">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="86189-201">Ce type de retour nécessite une importation de `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="86189-201">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="86189-202">Dans les 2.0 projets, le type de retour devient `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="86189-202">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="86189-203">Ce nouveau type de retour nécessite en remplaçant le `Microsoft.AspNetCore.Http.Authentication` importer avec un `Microsoft.AspNetCore.Authentication` importer.</span><span class="sxs-lookup"><span data-stu-id="86189-203">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="86189-204">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="86189-204">Additional resources</span></span>

<span data-ttu-id="86189-205">Pour plus d’informations et de discussion, consultez le [Discussion pour Auth 2.0](https://github.com/aspnet/Security/issues/1338) problème sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="86189-205">For additional details and discussion, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
