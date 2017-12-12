---
title: "Migration d’authentification et de l’identité au cœur d’ASP.NET 2.0"
author: scottaddie
description: "Cet article présente les étapes les plus courantes pour l’authentification de migration ASP.NET Core 1.x et l’identité pour ASP.NET 2.0 de base."
keywords: "Authentification ASP.NET Core, identité,"
ms.author: scaddie
manager: wpickett
ms.date: 10/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 1d8c75a21cd7110b3e414f0c600e9f05cbaeff45
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
# <a name="migrating-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="907f4-104">Migration de l’authentification et identité au cœur d’ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="907f4-104">Migrating Authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="907f4-105">Par [Scott Addie](https://github.com/scottaddie) et [arts martiaux de Hao](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="907f4-105">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="907f4-106">ASP.NET Core 2.0 a un nouveau modèle pour l’authentification et [identité](xref:security/authentication/identity) qui simplifie la configuration à l’aide des services.</span><span class="sxs-lookup"><span data-stu-id="907f4-106">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) which simplifies configuration by using services.</span></span> <span data-ttu-id="907f4-107">Les applications ASP.NET Core 1.x qui utilisent l’authentification ou l’identité peuvent être mis à jour pour utiliser le nouveau modèle, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="907f4-107">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="907f4-108">Intergiciel (middleware) d’authentification et services</span><span class="sxs-lookup"><span data-stu-id="907f4-108">Authentication Middleware and Services</span></span>
<span data-ttu-id="907f4-109">Dans les projets 1.x, l’authentification est configurée via l’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="907f4-109">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="907f4-110">Une méthode de l’intergiciel (middleware) est appelée pour chaque schéma d’authentification que vous souhaitez prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="907f4-110">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="907f4-111">L’exemple 1.x suivant configure l’authentification Facebook avec l’identité dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="907f4-111">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="907f4-112">Dans les 2.0 projets, l’authentification est configurée via les services.</span><span class="sxs-lookup"><span data-stu-id="907f4-112">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="907f4-113">Chaque schéma d’authentification est enregistré dans le `ConfigureServices` méthode *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="907f4-113">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="907f4-114">Le `UseIdentity` (méthode) est remplacée par `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="907f4-114">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="907f4-115">L’exemple 2.0 suivant configure l’authentification Facebook avec l’identité dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="907f4-115">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="907f4-116">Le `UseAuthentication` méthode ajoute un composant d’intergiciel (middleware) d’authentification unique qui est responsable de l’authentification automatique et la gestion des demandes d’authentification à distance.</span><span class="sxs-lookup"><span data-stu-id="907f4-116">The `UseAuthentication` method adds a single authentication middleware component which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="907f4-117">Il remplace tous les composants individuels intergiciel (middleware) avec un composant d’intergiciel (middleware) unique et commun.</span><span class="sxs-lookup"><span data-stu-id="907f4-117">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="907f4-118">Vous trouverez ci-dessous les instructions de migration 2.0 pour chaque schéma d’authentification principales.</span><span class="sxs-lookup"><span data-stu-id="907f4-118">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="907f4-119">Authentification par cookie</span><span class="sxs-lookup"><span data-stu-id="907f4-119">Cookie-based Authentication</span></span>
<span data-ttu-id="907f4-120">Sélectionnez une des deux options ci-dessous et apportez les modifications nécessaires dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="907f4-120">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="907f4-121">Utilisation des cookies avec l’identité</span><span class="sxs-lookup"><span data-stu-id="907f4-121">Use cookies with Identity</span></span>
    - <span data-ttu-id="907f4-122">Remplacez `UseIdentity` avec `UseAuthentication` dans le `Configure` méthode :</span><span class="sxs-lookup"><span data-stu-id="907f4-122">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="907f4-123">Appeler le `AddIdentity` méthode dans le `ConfigureServices` méthode pour ajouter les services d’authentification de cookie.</span><span class="sxs-lookup"><span data-stu-id="907f4-123">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="907f4-124">Si vous le souhaitez, appelez le `ConfigureApplicationCookie` ou `ConfigureExternalCookie` méthode dans le `ConfigureServices` méthode pour modifier les paramètres de cookies d’identité.</span><span class="sxs-lookup"><span data-stu-id="907f4-124">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="907f4-125">Utilisation des cookies sans identité</span><span class="sxs-lookup"><span data-stu-id="907f4-125">Use cookies without Identity</span></span>
    - <span data-ttu-id="907f4-126">Remplacez le `UseCookieAuthentication` l’appel de méthode le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="907f4-126">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - <span data-ttu-id="907f4-127">Appeler le `AddAuthentication` et `AddCookie` méthodes dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="907f4-127">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

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

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="907f4-128">Authentification du support JSON</span><span class="sxs-lookup"><span data-stu-id="907f4-128">JWT Bearer Authentication</span></span>
<span data-ttu-id="907f4-129">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="907f4-129">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="907f4-130">Remplacez le `UseJwtBearerAuthentication` l’appel de méthode le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="907f4-130">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="907f4-131">Appeler le `AddJwtBearer` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="907f4-131">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => 
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="907f4-132">Cet extrait de code n’utilise pas l’identité, afin du schéma par défaut doit être défini en passant `JwtBearerDefaults.AuthenticationScheme` à la `AddAuthentication` (méthode).</span><span class="sxs-lookup"><span data-stu-id="907f4-132">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="907f4-133">OpenID Connect (OIDC) authentification</span><span class="sxs-lookup"><span data-stu-id="907f4-133">OpenID Connect (OIDC) Authentication</span></span>
<span data-ttu-id="907f4-134">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="907f4-134">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="907f4-135">Remplacez le `UseOpenIdConnectAuthentication` l’appel de méthode le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="907f4-135">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="907f4-136">Appeler le `AddOpenIdConnect` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="907f4-136">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

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

### <a name="facebook-authentication"></a><span data-ttu-id="907f4-137">Authentification Facebook</span><span class="sxs-lookup"><span data-stu-id="907f4-137">Facebook Authentication</span></span>
<span data-ttu-id="907f4-138">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="907f4-138">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="907f4-139">Remplacez le `UseFacebookAuthentication` l’appel de méthode le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="907f4-139">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="907f4-140">Appeler le `AddFacebook` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="907f4-140">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="907f4-141">Authentification Google</span><span class="sxs-lookup"><span data-stu-id="907f4-141">Google Authentication</span></span>
<span data-ttu-id="907f4-142">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="907f4-142">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="907f4-143">Remplacez le `UseGoogleAuthentication` l’appel de méthode le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="907f4-143">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="907f4-144">Appeler le `AddGoogle` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="907f4-144">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => 
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="907f4-145">Authentification de compte Microsoft</span><span class="sxs-lookup"><span data-stu-id="907f4-145">Microsoft Account Authentication</span></span>
<span data-ttu-id="907f4-146">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="907f4-146">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="907f4-147">Remplacez le `UseMicrosoftAccountAuthentication` l’appel de méthode le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="907f4-147">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="907f4-148">Appeler le `AddMicrosoftAccount` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="907f4-148">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => 
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a><span data-ttu-id="907f4-149">Authentification Twitter</span><span class="sxs-lookup"><span data-stu-id="907f4-149">Twitter Authentication</span></span>
<span data-ttu-id="907f4-150">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="907f4-150">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="907f4-151">Remplacez le `UseTwitterAuthentication` l’appel de méthode le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="907f4-151">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="907f4-152">Appeler le `AddTwitter` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="907f4-152">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => 
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="907f4-153">Définition des schémas d’authentification par défaut</span><span class="sxs-lookup"><span data-stu-id="907f4-153">Setting Default Authentication Schemes</span></span>
<span data-ttu-id="907f4-154">Dans 1.x, le `AutomaticAuthenticate` et `AutomaticChallenge` propriétés de la [AuthenticationOptions](https://docs.microsoft.com/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) classe de base a été conçue pour être définie sur un schéma d’authentification unique.</span><span class="sxs-lookup"><span data-stu-id="907f4-154">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](https://docs.microsoft.com/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="907f4-155">Il n’existait aucun moyen de bonne pour appliquer ceci.</span><span class="sxs-lookup"><span data-stu-id="907f4-155">There was no good way to enforce this.</span></span>

<span data-ttu-id="907f4-156">Dans la version 2.0, ces deux propriétés ont été supprimées en tant que propriétés sur chaque `AuthenticationOptions` instance.</span><span class="sxs-lookup"><span data-stu-id="907f4-156">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="907f4-157">Ils peuvent être configurés dans le `AddAuthentication` appel de méthode dans le `ConfigureServices` méthode de *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="907f4-157">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="907f4-158">Dans l’extrait de code précédent, le schéma par défaut est défini sur `CookieAuthenticationDefaults.AuthenticationScheme` (« Cookies »).</span><span class="sxs-lookup"><span data-stu-id="907f4-158">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="907f4-159">Vous pouvez également utiliser une version surchargée de la `AddAuthentication` pour définir plusieurs propriétés.</span><span class="sxs-lookup"><span data-stu-id="907f4-159">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="907f4-160">Dans l’exemple suivant de la méthode surchargée, le schéma par défaut est défini `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="907f4-160">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="907f4-161">Le schéma d’authentification peut également être spécifié au sein de votre personne `[Authorize]` attributs ou des stratégies d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="907f4-161">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="907f4-162">Définir un schéma par défaut dans la version 2.0 si une des conditions suivantes est remplie :</span><span class="sxs-lookup"><span data-stu-id="907f4-162">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="907f4-163">Vous souhaitez que l’utilisateur d’être automatiquement connectés</span><span class="sxs-lookup"><span data-stu-id="907f4-163">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="907f4-164">Vous utilisez la `[Authorize]` stratégies d’attribut ou d’autorisation sans spécifier de schémas</span><span class="sxs-lookup"><span data-stu-id="907f4-164">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="907f4-165">Une exception à cette règle est le `AddIdentity` (méthode).</span><span class="sxs-lookup"><span data-stu-id="907f4-165">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="907f4-166">Cette méthode ajoute des cookies pour vous et définit la valeur par défaut s’authentifier et stimulation des schémas dans le cookie d’application `IdentityConstants.ApplicationScheme`.</span><span class="sxs-lookup"><span data-stu-id="907f4-166">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="907f4-167">En outre, il définit le schéma de connexion par défaut pour le cookie externe `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="907f4-167">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="907f4-168">Utiliser les Extensions d’authentification HttpContext</span><span class="sxs-lookup"><span data-stu-id="907f4-168">Use HttpContext Authentication Extensions</span></span>
<span data-ttu-id="907f4-169">Le `IAuthenticationManager` interface est le point d’entrée principal dans le système d’authentification 1.x.</span><span class="sxs-lookup"><span data-stu-id="907f4-169">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="907f4-170">Elle a été remplacée par un nouvel ensemble de `HttpContext` les méthodes d’extension dans le `Microsoft.AspNetCore.Authentication` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="907f4-170">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="907f4-171">Par exemple, les projets 1.x référence un `Authentication` propriété :</span><span class="sxs-lookup"><span data-stu-id="907f4-171">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="907f4-172">Dans les 2.0 projets, vous devez importer le `Microsoft.AspNetCore.Authentication` espace de noms et supprimer la `Authentication` références de propriété :</span><span class="sxs-lookup"><span data-stu-id="907f4-172">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="907f4-173">L’authentification Windows (HTTP.sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="907f4-173">Windows Authentication (HTTP.sys / IISIntegration)</span></span>
<span data-ttu-id="907f4-174">Il existe deux variantes de l’authentification Windows :</span><span class="sxs-lookup"><span data-stu-id="907f4-174">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="907f4-175">L’hôte autorise uniquement les utilisateurs authentifiés</span><span class="sxs-lookup"><span data-stu-id="907f4-175">The host only allows authenticated users</span></span>
2. <span data-ttu-id="907f4-176">L’ordinateur hôte permet à la fois anonymes et les utilisateurs authentifiés</span><span class="sxs-lookup"><span data-stu-id="907f4-176">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="907f4-177">La première variante décrite ci-dessus n’est pas affectée par les modifications de 2.0.</span><span class="sxs-lookup"><span data-stu-id="907f4-177">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="907f4-178">La deuxième variante décrite ci-dessus est affectée par les modifications de 2.0.</span><span class="sxs-lookup"><span data-stu-id="907f4-178">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="907f4-179">Par exemple, vous pouvez autoriser les utilisateurs anonymes à votre application sur IIS ou [HTTP.sys](xref:fundamentals/servers/weblistener) mais autorisant des utilisateurs au niveau du contrôleur de couche.</span><span class="sxs-lookup"><span data-stu-id="907f4-179">As an example, you may be allowing anonymous users into your application at the IIS or [HTTP.sys](xref:fundamentals/servers/weblistener) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="907f4-180">Dans ce scénario, définissez le schéma par défaut sur `IISDefaults.AuthenticationScheme` dans les `ConfigureServices` méthode *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="907f4-180">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="907f4-181">Échec de définition de schéma par défaut empêche en conséquence la demande authorize au défi de fonctionner.</span><span class="sxs-lookup"><span data-stu-id="907f4-181">Failure to set the default scheme accordingly prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="907f4-182">Instances de IdentityCookieOptions</span><span class="sxs-lookup"><span data-stu-id="907f4-182">IdentityCookieOptions Instances</span></span>
<span data-ttu-id="907f4-183">Un effet secondaire des 2.0 modifications est le commutateur à l’aide de ces options au lieu d’instances d’options de cookie.</span><span class="sxs-lookup"><span data-stu-id="907f4-183">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="907f4-184">La possibilité de personnaliser les noms de schéma identité cookie est supprimée.</span><span class="sxs-lookup"><span data-stu-id="907f4-184">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="907f4-185">Par exemple, des projets utilisent 1.x [injection de constructeur](xref:mvc/controllers/dependency-injection#constructor-injection) pour passer un `IdentityCookieOptions` paramètre dans *AccountController.cs*.</span><span class="sxs-lookup"><span data-stu-id="907f4-185">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs*.</span></span> <span data-ttu-id="907f4-186">Le schéma d’authentification externe cookie est accessible à partir de l’instance fournie :</span><span class="sxs-lookup"><span data-stu-id="907f4-186">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="907f4-187">L’injection de constructeur susmentionnés devienne inutile dans les 2.0 projets et le `_externalCookieScheme` champ peut être supprimé :</span><span class="sxs-lookup"><span data-stu-id="907f4-187">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="907f4-188">Le `IdentityConstants.ExternalScheme` constante peut être utilisée directement :</span><span class="sxs-lookup"><span data-stu-id="907f4-188">The `IdentityConstants.ExternalScheme` constant can be used directly:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="907f4-189">Ajouter des propriétés de Navigation IdentityUser POCO</span><span class="sxs-lookup"><span data-stu-id="907f4-189">Add IdentityUser POCO Navigation Properties</span></span>
<span data-ttu-id="907f4-190">Les propriétés de navigation de base d’Entity Framework (EF) de la base de `IdentityUser` POCO (Plain objet CLR) ont été supprimées.</span><span class="sxs-lookup"><span data-stu-id="907f4-190">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="907f4-191">Si votre projet 1.x utilisé ces propriétés, les ajouter manuellement au projet 2.0 :</span><span class="sxs-lookup"><span data-stu-id="907f4-191">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

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

<span data-ttu-id="907f4-192">Pour empêcher les clés étrangères en double lors de l’exécution de Migrations de noyaux EF, ajoutez le code suivant à votre `IdentityDbContext` classe `OnModelCreating` (méthode) (une fois la `base.OnModelCreating();` appeler) :</span><span class="sxs-lookup"><span data-stu-id="907f4-192">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

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

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="907f4-193">Remplacez GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="907f4-193">Replace GetExternalAuthenticationSchemes</span></span>
<span data-ttu-id="907f4-194">La méthode synchrone `GetExternalAuthenticationSchemes` a été supprimé en faveur d’une version asynchrone.</span><span class="sxs-lookup"><span data-stu-id="907f4-194">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="907f4-195">les projets de 1.x ont le code suivant dans *ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="907f4-195">1.x projects have the following code in *ManageController.cs*:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="907f4-196">Cette méthode s’affiche dans *Login.cshtml* trop :</span><span class="sxs-lookup"><span data-stu-id="907f4-196">This method appears in *Login.cshtml* too:</span></span>

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

<span data-ttu-id="907f4-197">Dans les 2.0 projets, utilisez la `GetExternalAuthenticationSchemesAsync` méthode :</span><span class="sxs-lookup"><span data-stu-id="907f4-197">In 2.0 projects, use the `GetExternalAuthenticationSchemesAsync` method:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="907f4-198">Dans *Login.cshtml*, le `AuthenticationScheme` propriété accédée dans le `foreach` boucle devient `Name`:</span><span class="sxs-lookup"><span data-stu-id="907f4-198">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="907f4-199">Modification de la propriété de ManageLoginsViewModel</span><span class="sxs-lookup"><span data-stu-id="907f4-199">ManageLoginsViewModel Property Change</span></span>
<span data-ttu-id="907f4-200">A `ManageLoginsViewModel` objet est utilisé dans le `ManageLogins` action de *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="907f4-200">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="907f4-201">Dans les projets 1.x, l’objet `OtherLogins` propriété type de retour est `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="907f4-201">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="907f4-202">Ce type de retour nécessite une importation de `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="907f4-202">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="907f4-203">Dans les 2.0 projets, le type de retour devient `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="907f4-203">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="907f4-204">Ce nouveau type de retour nécessite en remplaçant le `Microsoft.AspNetCore.Http.Authentication` importer avec un `Microsoft.AspNetCore.Authentication` importer.</span><span class="sxs-lookup"><span data-stu-id="907f4-204">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="907f4-205">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="907f4-205">Additional Resources</span></span>
<span data-ttu-id="907f4-206">Pour plus d’informations et la description, consultez le [discussion sur la version 2.0 d’authentification](https://github.com/aspnet/Security/issues/1338) problème sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="907f4-206">For additional details and discussion, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
