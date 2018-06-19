---
title: Migrer l’authentification et identité vers ASP.NET Core 2.0
author: scottaddie
description: Cet article présente les étapes les plus courantes pour l’authentification de migration ASP.NET Core 1.x et l’identité pour ASP.NET 2.0 de base.
manager: wpickett
ms.author: scaddie
ms.date: 10/26/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 0653906996f9f37d436ebefc6a738d2603788d53
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32741457"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="02471-103">Migrer l’authentification et identité vers ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="02471-103">Migrate authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="02471-104">Par [Scott Addie](https://github.com/scottaddie) et [arts martiaux de Hao](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="02471-104">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="02471-105">ASP.NET Core 2.0 a un nouveau modèle pour l’authentification et [identité](xref:security/authentication/identity) qui simplifie la configuration à l’aide des services.</span><span class="sxs-lookup"><span data-stu-id="02471-105">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) which simplifies configuration by using services.</span></span> <span data-ttu-id="02471-106">Les applications ASP.NET Core 1.x qui utilisent l’authentification ou l’identité peuvent être mis à jour pour utiliser le nouveau modèle, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="02471-106">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="02471-107">Intergiciel (middleware) d’authentification et services</span><span class="sxs-lookup"><span data-stu-id="02471-107">Authentication Middleware and services</span></span>
<span data-ttu-id="02471-108">Dans les projets 1.x, l’authentification est configurée via l’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="02471-108">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="02471-109">Une méthode de l’intergiciel (middleware) est appelée pour chaque schéma d’authentification que vous souhaitez prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="02471-109">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="02471-110">L’exemple 1.x suivant configure l’authentification Facebook avec l’identité dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="02471-110">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="02471-111">Dans les 2.0 projets, l’authentification est configurée via les services.</span><span class="sxs-lookup"><span data-stu-id="02471-111">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="02471-112">Chaque schéma d’authentification est enregistré dans le `ConfigureServices` méthode *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="02471-112">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="02471-113">Le `UseIdentity` (méthode) est remplacée par `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="02471-113">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="02471-114">L’exemple 2.0 suivant configure l’authentification Facebook avec l’identité dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="02471-114">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="02471-115">Le `UseAuthentication` méthode ajoute un composant d’intergiciel (middleware) d’authentification unique qui est responsable de l’authentification automatique et la gestion des demandes d’authentification à distance.</span><span class="sxs-lookup"><span data-stu-id="02471-115">The `UseAuthentication` method adds a single authentication middleware component which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="02471-116">Il remplace tous les composants individuels intergiciel (middleware) avec un composant d’intergiciel (middleware) unique et commun.</span><span class="sxs-lookup"><span data-stu-id="02471-116">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="02471-117">Vous trouverez ci-dessous les instructions de migration 2.0 pour chaque schéma d’authentification principales.</span><span class="sxs-lookup"><span data-stu-id="02471-117">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="02471-118">Authentification par cookie</span><span class="sxs-lookup"><span data-stu-id="02471-118">Cookie-based authentication</span></span>
<span data-ttu-id="02471-119">Sélectionnez une des deux options ci-dessous et apportez les modifications nécessaires dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="02471-119">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="02471-120">Utilisation des cookies avec l’identité</span><span class="sxs-lookup"><span data-stu-id="02471-120">Use cookies with Identity</span></span>
    - <span data-ttu-id="02471-121">Remplacez `UseIdentity` avec `UseAuthentication` dans le `Configure` méthode :</span><span class="sxs-lookup"><span data-stu-id="02471-121">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="02471-122">Appeler le `AddIdentity` méthode dans le `ConfigureServices` méthode pour ajouter les services d’authentification de cookie.</span><span class="sxs-lookup"><span data-stu-id="02471-122">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="02471-123">Si vous le souhaitez, appelez le `ConfigureApplicationCookie` ou `ConfigureExternalCookie` méthode dans le `ConfigureServices` méthode pour modifier les paramètres de cookies d’identité.</span><span class="sxs-lookup"><span data-stu-id="02471-123">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="02471-124">Utilisation des cookies sans identité</span><span class="sxs-lookup"><span data-stu-id="02471-124">Use cookies without Identity</span></span>
    - <span data-ttu-id="02471-125">Remplacez le `UseCookieAuthentication` l’appel de méthode le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="02471-125">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - <span data-ttu-id="02471-126">Appeler le `AddAuthentication` et `AddCookie` méthodes dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="02471-126">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

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

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="02471-127">Authentification du support JSON</span><span class="sxs-lookup"><span data-stu-id="02471-127">JWT Bearer Authentication</span></span>
<span data-ttu-id="02471-128">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="02471-128">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="02471-129">Remplacez le `UseJwtBearerAuthentication` l’appel de méthode le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="02471-129">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="02471-130">Appeler le `AddJwtBearer` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="02471-130">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => 
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="02471-131">Cet extrait de code n’utilise pas l’identité, afin du schéma par défaut doit être défini en passant `JwtBearerDefaults.AuthenticationScheme` à la `AddAuthentication` (méthode).</span><span class="sxs-lookup"><span data-stu-id="02471-131">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="02471-132">Authentification de connexion OpenID (OIDC)</span><span class="sxs-lookup"><span data-stu-id="02471-132">OpenID Connect (OIDC) authentication</span></span>
<span data-ttu-id="02471-133">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="02471-133">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="02471-134">Remplacez le `UseOpenIdConnectAuthentication` l’appel de méthode le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="02471-134">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="02471-135">Appeler le `AddOpenIdConnect` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="02471-135">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

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

### <a name="facebook-authentication"></a><span data-ttu-id="02471-136">authentification Facebook</span><span class="sxs-lookup"><span data-stu-id="02471-136">Facebook authentication</span></span>
<span data-ttu-id="02471-137">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="02471-137">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="02471-138">Remplacez le `UseFacebookAuthentication` l’appel de méthode le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="02471-138">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="02471-139">Appeler le `AddFacebook` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="02471-139">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="02471-140">Authentification Google</span><span class="sxs-lookup"><span data-stu-id="02471-140">Google authentication</span></span>
<span data-ttu-id="02471-141">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="02471-141">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="02471-142">Remplacez le `UseGoogleAuthentication` l’appel de méthode le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="02471-142">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="02471-143">Appeler le `AddGoogle` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="02471-143">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => 
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="02471-144">Authentification de Microsoft Account</span><span class="sxs-lookup"><span data-stu-id="02471-144">Microsoft Account authentication</span></span>
<span data-ttu-id="02471-145">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="02471-145">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="02471-146">Remplacez le `UseMicrosoftAccountAuthentication` l’appel de méthode le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="02471-146">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="02471-147">Appeler le `AddMicrosoftAccount` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="02471-147">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => 
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a><span data-ttu-id="02471-148">Authentification Twitter</span><span class="sxs-lookup"><span data-stu-id="02471-148">Twitter authentication</span></span>
<span data-ttu-id="02471-149">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="02471-149">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="02471-150">Remplacez le `UseTwitterAuthentication` l’appel de méthode le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="02471-150">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="02471-151">Appeler le `AddTwitter` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="02471-151">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => 
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="02471-152">Définition des schémas d’authentification par défaut</span><span class="sxs-lookup"><span data-stu-id="02471-152">Setting default authentication schemes</span></span>
<span data-ttu-id="02471-153">Dans 1.x, le `AutomaticAuthenticate` et `AutomaticChallenge` propriétés de la [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) classe de base a été conçue pour être définie sur un schéma d’authentification unique.</span><span class="sxs-lookup"><span data-stu-id="02471-153">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="02471-154">Il n’existait aucun moyen de bonne pour appliquer ceci.</span><span class="sxs-lookup"><span data-stu-id="02471-154">There was no good way to enforce this.</span></span>

<span data-ttu-id="02471-155">Dans la version 2.0, ces deux propriétés ont été supprimées en tant que propriétés sur chaque `AuthenticationOptions` instance.</span><span class="sxs-lookup"><span data-stu-id="02471-155">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="02471-156">Ils peuvent être configurés dans le `AddAuthentication` appel de méthode dans le `ConfigureServices` méthode de *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="02471-156">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="02471-157">Dans l’extrait de code précédent, le schéma par défaut est défini sur `CookieAuthenticationDefaults.AuthenticationScheme` (« Cookies »).</span><span class="sxs-lookup"><span data-stu-id="02471-157">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="02471-158">Vous pouvez également utiliser une version surchargée de la `AddAuthentication` pour définir plusieurs propriétés.</span><span class="sxs-lookup"><span data-stu-id="02471-158">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="02471-159">Dans l’exemple suivant de la méthode surchargée, le schéma par défaut est défini `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="02471-159">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="02471-160">Le schéma d’authentification peut également être spécifié au sein de votre personne `[Authorize]` attributs ou des stratégies d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="02471-160">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="02471-161">Définir un schéma par défaut dans la version 2.0 si une des conditions suivantes est remplie :</span><span class="sxs-lookup"><span data-stu-id="02471-161">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="02471-162">Vous souhaitez que l’utilisateur d’être automatiquement connectés</span><span class="sxs-lookup"><span data-stu-id="02471-162">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="02471-163">Vous utilisez la `[Authorize]` stratégies d’attribut ou d’autorisation sans spécifier de schémas</span><span class="sxs-lookup"><span data-stu-id="02471-163">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="02471-164">Une exception à cette règle est le `AddIdentity` (méthode).</span><span class="sxs-lookup"><span data-stu-id="02471-164">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="02471-165">Cette méthode ajoute des cookies pour vous et définit la valeur par défaut s’authentifier et stimulation des schémas dans le cookie d’application `IdentityConstants.ApplicationScheme`.</span><span class="sxs-lookup"><span data-stu-id="02471-165">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="02471-166">En outre, il définit le schéma de connexion par défaut pour le cookie externe `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="02471-166">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="02471-167">Utiliser les extensions d’authentification HttpContext</span><span class="sxs-lookup"><span data-stu-id="02471-167">Use HttpContext authentication extensions</span></span>
<span data-ttu-id="02471-168">Le `IAuthenticationManager` interface est le point d’entrée principal dans le système d’authentification 1.x.</span><span class="sxs-lookup"><span data-stu-id="02471-168">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="02471-169">Elle a été remplacée par un nouvel ensemble de `HttpContext` les méthodes d’extension dans le `Microsoft.AspNetCore.Authentication` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="02471-169">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="02471-170">Par exemple, les projets 1.x référence un `Authentication` propriété :</span><span class="sxs-lookup"><span data-stu-id="02471-170">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="02471-171">Dans les 2.0 projets, vous devez importer le `Microsoft.AspNetCore.Authentication` espace de noms et supprimer la `Authentication` références de propriété :</span><span class="sxs-lookup"><span data-stu-id="02471-171">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="02471-172">L’authentification Windows (HTTP.sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="02471-172">Windows Authentication (HTTP.sys / IISIntegration)</span></span>
<span data-ttu-id="02471-173">Il existe deux variantes de l’authentification Windows :</span><span class="sxs-lookup"><span data-stu-id="02471-173">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="02471-174">L’hôte autorise uniquement les utilisateurs authentifiés</span><span class="sxs-lookup"><span data-stu-id="02471-174">The host only allows authenticated users</span></span>
2. <span data-ttu-id="02471-175">L’ordinateur hôte permet à la fois anonymes et les utilisateurs authentifiés</span><span class="sxs-lookup"><span data-stu-id="02471-175">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="02471-176">La première variante décrite ci-dessus n’est pas affectée par les modifications de 2.0.</span><span class="sxs-lookup"><span data-stu-id="02471-176">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="02471-177">La deuxième variante décrite ci-dessus est affectée par les modifications de 2.0.</span><span class="sxs-lookup"><span data-stu-id="02471-177">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="02471-178">Par exemple, vous pouvez autoriser les utilisateurs anonymes à votre application sur IIS ou [HTTP.sys](xref:fundamentals/servers/weblistener) mais autorisant des utilisateurs au niveau du contrôleur de couche.</span><span class="sxs-lookup"><span data-stu-id="02471-178">As an example, you may be allowing anonymous users into your application at the IIS or [HTTP.sys](xref:fundamentals/servers/weblistener) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="02471-179">Dans ce scénario, définissez le schéma par défaut sur `IISDefaults.AuthenticationScheme` dans les `ConfigureServices` méthode *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="02471-179">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="02471-180">Échec de définition de schéma par défaut empêche en conséquence la demande authorize au défi de fonctionner.</span><span class="sxs-lookup"><span data-stu-id="02471-180">Failure to set the default scheme accordingly prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="02471-181">Instances de IdentityCookieOptions</span><span class="sxs-lookup"><span data-stu-id="02471-181">IdentityCookieOptions instances</span></span>
<span data-ttu-id="02471-182">Un effet secondaire des 2.0 modifications est le commutateur à l’aide de ces options au lieu d’instances d’options de cookie.</span><span class="sxs-lookup"><span data-stu-id="02471-182">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="02471-183">La possibilité de personnaliser les noms de schéma identité cookie est supprimée.</span><span class="sxs-lookup"><span data-stu-id="02471-183">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="02471-184">Par exemple, des projets utilisent 1.x [injection de constructeur](xref:mvc/controllers/dependency-injection#constructor-injection) pour passer un `IdentityCookieOptions` paramètre dans *AccountController.cs*.</span><span class="sxs-lookup"><span data-stu-id="02471-184">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs*.</span></span> <span data-ttu-id="02471-185">Le schéma d’authentification externe cookie est accessible à partir de l’instance fournie :</span><span class="sxs-lookup"><span data-stu-id="02471-185">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="02471-186">L’injection de constructeur susmentionnés devienne inutile dans les 2.0 projets et le `_externalCookieScheme` champ peut être supprimé :</span><span class="sxs-lookup"><span data-stu-id="02471-186">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="02471-187">Le `IdentityConstants.ExternalScheme` constante peut être utilisée directement :</span><span class="sxs-lookup"><span data-stu-id="02471-187">The `IdentityConstants.ExternalScheme` constant can be used directly:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="02471-188">Ajouter des propriétés de navigation IdentityUser POCO</span><span class="sxs-lookup"><span data-stu-id="02471-188">Add IdentityUser POCO navigation properties</span></span>
<span data-ttu-id="02471-189">Les propriétés de navigation de base d’Entity Framework (EF) de la base de `IdentityUser` POCO (Plain objet CLR) ont été supprimées.</span><span class="sxs-lookup"><span data-stu-id="02471-189">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="02471-190">Si votre projet 1.x utilisé ces propriétés, les ajouter manuellement au projet 2.0 :</span><span class="sxs-lookup"><span data-stu-id="02471-190">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

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

<span data-ttu-id="02471-191">Pour empêcher les clés étrangères en double lors de l’exécution de Migrations de noyaux EF, ajoutez le code suivant à votre `IdentityDbContext` classe `OnModelCreating` (méthode) (une fois la `base.OnModelCreating();` appeler) :</span><span class="sxs-lookup"><span data-stu-id="02471-191">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Identity table names and more.
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

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="02471-192">Remplacez GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="02471-192">Replace GetExternalAuthenticationSchemes</span></span>
<span data-ttu-id="02471-193">La méthode synchrone `GetExternalAuthenticationSchemes` a été supprimé en faveur d’une version asynchrone.</span><span class="sxs-lookup"><span data-stu-id="02471-193">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="02471-194">les projets de 1.x ont le code suivant dans *ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="02471-194">1.x projects have the following code in *ManageController.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="02471-195">Cette méthode s’affiche dans *Login.cshtml* trop :</span><span class="sxs-lookup"><span data-stu-id="02471-195">This method appears in *Login.cshtml* too:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

<span data-ttu-id="02471-196">Dans les 2.0 projets, utilisez la `GetExternalAuthenticationSchemesAsync` méthode :</span><span class="sxs-lookup"><span data-stu-id="02471-196">In 2.0 projects, use the `GetExternalAuthenticationSchemesAsync` method:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="02471-197">Dans *Login.cshtml*, le `AuthenticationScheme` propriété accédée dans le `foreach` boucle devient `Name`:</span><span class="sxs-lookup"><span data-stu-id="02471-197">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="02471-198">Modification de la propriété ManageLoginsViewModel</span><span class="sxs-lookup"><span data-stu-id="02471-198">ManageLoginsViewModel property change</span></span>
<span data-ttu-id="02471-199">A `ManageLoginsViewModel` objet est utilisé dans le `ManageLogins` action de *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="02471-199">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="02471-200">Dans les projets 1.x, l’objet `OtherLogins` propriété type de retour est `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="02471-200">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="02471-201">Ce type de retour nécessite une importation de `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="02471-201">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="02471-202">Dans les 2.0 projets, le type de retour devient `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="02471-202">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="02471-203">Ce nouveau type de retour nécessite en remplaçant le `Microsoft.AspNetCore.Http.Authentication` importer avec un `Microsoft.AspNetCore.Authentication` importer.</span><span class="sxs-lookup"><span data-stu-id="02471-203">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="02471-204">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="02471-204">Additional resources</span></span>
<span data-ttu-id="02471-205">Pour plus d’informations et la description, consultez le [discussion sur la version 2.0 d’authentification](https://github.com/aspnet/Security/issues/1338) problème sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="02471-205">For additional details and discussion, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
