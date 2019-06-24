---
title: Migrer d’authentification et identité vers ASP.NET Core 2.0
author: scottaddie
description: Cet article décrit les étapes les plus courantes pour migration ASP.NET Core 1.x l’authentification et identité vers ASP.NET Core 2.0.
ms.author: scaddie
ms.date: 06/21/2019
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: c83356e12fa5ae581b369265b9d857b08445ed51
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313746"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="28ebf-103">Migrer d’authentification et identité vers ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="28ebf-103">Migrate authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="28ebf-104">Par [Scott Addie](https://github.com/scottaddie) et [arts martiaux Hao](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="28ebf-104">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="28ebf-105">ASP.NET Core 2.0 offre un nouveau modèle pour l’authentification et [identité](xref:security/authentication/identity) qui simplifie la configuration à l’aide des services.</span><span class="sxs-lookup"><span data-stu-id="28ebf-105">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) that simplifies configuration by using services.</span></span> <span data-ttu-id="28ebf-106">Les applications ASP.NET Core 1.x qui utilisent l’authentification ou Identity peuvent être mis à jour pour utiliser le nouveau modèle, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="28ebf-106">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

## <a name="update-namespaces"></a><span data-ttu-id="28ebf-107">Mettre à jour les espaces de noms</span><span class="sxs-lookup"><span data-stu-id="28ebf-107">Update namespaces</span></span>

<span data-ttu-id="28ebf-108">Dans la version 1.x, les classes telles `IdentityRole` et `IdentityUser` ont été trouvés dans le `Microsoft.AspNetCore.Identity.EntityFrameworkCore` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="28ebf-108">In 1.x, classes such `IdentityRole` and `IdentityUser` were found in the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` namespace.</span></span>

<span data-ttu-id="28ebf-109">Dans la version 2.0, le <xref:Microsoft.AspNetCore.Identity> espace de noms est devenue la nouvelle page d’accueil pour plusieurs de ces classes.</span><span class="sxs-lookup"><span data-stu-id="28ebf-109">In 2.0, the <xref:Microsoft.AspNetCore.Identity> namespace became the new home for several of such classes.</span></span> <span data-ttu-id="28ebf-110">Avec le code d’identité par défaut, les classes concernées incluent `ApplicationUser` et `Startup`.</span><span class="sxs-lookup"><span data-stu-id="28ebf-110">With the default Identity code, affected classes include `ApplicationUser` and `Startup`.</span></span> <span data-ttu-id="28ebf-111">Ajuster votre `using` instructions pour résoudre les références concernées.</span><span class="sxs-lookup"><span data-stu-id="28ebf-111">Adjust your `using` statements to resolve the affected references.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="28ebf-112">Intergiciel (middleware) d’authentification et services</span><span class="sxs-lookup"><span data-stu-id="28ebf-112">Authentication Middleware and services</span></span>

<span data-ttu-id="28ebf-113">Dans les projets 1.x, l’authentification est configurée via l’intergiciel.</span><span class="sxs-lookup"><span data-stu-id="28ebf-113">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="28ebf-114">Une méthode de l’intergiciel (middleware) est appelée pour chaque schéma d’authentification que vous souhaitez prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="28ebf-114">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="28ebf-115">L’exemple de 1.x suivant configure l’authentification Facebook avec l’identité dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="28ebf-115">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="28ebf-116">Dans les 2.0 projets, l’authentification est configurée par le biais de services.</span><span class="sxs-lookup"><span data-stu-id="28ebf-116">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="28ebf-117">Chaque schéma d’authentification est enregistré dans le `ConfigureServices` méthode de *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="28ebf-117">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="28ebf-118">Le `UseIdentity` méthode est remplacée par `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="28ebf-118">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="28ebf-119">L’exemple suivant 2.0 configure l’authentification Facebook avec l’identité dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="28ebf-119">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="28ebf-120">Le `UseAuthentication` méthode ajoute un composant d’intergiciel d’authentification unique, qui est responsable de l’authentification automatique et la gestion des demandes d’authentification à distance.</span><span class="sxs-lookup"><span data-stu-id="28ebf-120">The `UseAuthentication` method adds a single authentication middleware component, which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="28ebf-121">Il remplace tous les composants d’intergiciel (middleware) individuels par un composant d’intergiciel (middleware) unique et commun.</span><span class="sxs-lookup"><span data-stu-id="28ebf-121">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="28ebf-122">Vous trouverez ci-dessous des instructions de migration 2.0 pour chaque schéma d’authentification principale.</span><span class="sxs-lookup"><span data-stu-id="28ebf-122">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="28ebf-123">Authentification basée sur les cookies</span><span class="sxs-lookup"><span data-stu-id="28ebf-123">Cookie-based authentication</span></span>

<span data-ttu-id="28ebf-124">Sélectionnez une des deux options ci-dessous et apportez les modifications nécessaires dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="28ebf-124">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="28ebf-125">Utilisation des cookies avec l’identité</span><span class="sxs-lookup"><span data-stu-id="28ebf-125">Use cookies with Identity</span></span>
    - <span data-ttu-id="28ebf-126">Remplacez `UseIdentity` avec `UseAuthentication` dans le `Configure` méthode :</span><span class="sxs-lookup"><span data-stu-id="28ebf-126">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="28ebf-127">Appeler le `AddIdentity` méthode dans le `ConfigureServices` méthode pour ajouter les services d’authentification de cookie.</span><span class="sxs-lookup"><span data-stu-id="28ebf-127">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="28ebf-128">Si vous le souhaitez, appelez le `ConfigureApplicationCookie` ou `ConfigureExternalCookie` méthode dans le `ConfigureServices` méthode pour modifier les paramètres de cookies d’identité.</span><span class="sxs-lookup"><span data-stu-id="28ebf-128">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="28ebf-129">Utilisation des cookies sans Identity</span><span class="sxs-lookup"><span data-stu-id="28ebf-129">Use cookies without Identity</span></span>
    - <span data-ttu-id="28ebf-130">Remplacez le `UseCookieAuthentication` appel de méthode dans le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="28ebf-130">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="28ebf-131">Appeler le `AddAuthentication` et `AddCookie` méthodes dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="28ebf-131">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

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

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="28ebf-132">Authentification de porteur JWT</span><span class="sxs-lookup"><span data-stu-id="28ebf-132">JWT Bearer Authentication</span></span>

<span data-ttu-id="28ebf-133">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="28ebf-133">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="28ebf-134">Remplacez le `UseJwtBearerAuthentication` appel de méthode dans le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="28ebf-134">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="28ebf-135">Appeler le `AddJwtBearer` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="28ebf-135">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="28ebf-136">Cet extrait de code n’utilise pas l’identité, le schéma par défaut doivent donc être défini en passant `JwtBearerDefaults.AuthenticationScheme` à la `AddAuthentication` (méthode).</span><span class="sxs-lookup"><span data-stu-id="28ebf-136">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="28ebf-137">Authentification OpenID Connect (OIDC)</span><span class="sxs-lookup"><span data-stu-id="28ebf-137">OpenID Connect (OIDC) authentication</span></span>

<span data-ttu-id="28ebf-138">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="28ebf-138">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="28ebf-139">Remplacez le `UseOpenIdConnectAuthentication` appel de méthode dans le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="28ebf-139">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="28ebf-140">Appeler le `AddOpenIdConnect` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="28ebf-140">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

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

- <span data-ttu-id="28ebf-141">Remplacez le `PostLogoutRedirectUri` propriété dans le `OpenIdConnectOptions` action avec `SignedOutRedirectUri`:</span><span class="sxs-lookup"><span data-stu-id="28ebf-141">Replace the `PostLogoutRedirectUri` property in the `OpenIdConnectOptions` action with `SignedOutRedirectUri`:</span></span>

    ```csharp
    .AddOpenIdConnect(options =>
    {
        options.SignedOutRedirectUri = "https://contoso.com";
    });
    ```
    
### <a name="facebook-authentication"></a><span data-ttu-id="28ebf-142">Authentification Facebook</span><span class="sxs-lookup"><span data-stu-id="28ebf-142">Facebook authentication</span></span>

<span data-ttu-id="28ebf-143">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="28ebf-143">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="28ebf-144">Remplacez le `UseFacebookAuthentication` appel de méthode dans le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="28ebf-144">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="28ebf-145">Appeler le `AddFacebook` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="28ebf-145">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="28ebf-146">Authentification Google</span><span class="sxs-lookup"><span data-stu-id="28ebf-146">Google authentication</span></span>

<span data-ttu-id="28ebf-147">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="28ebf-147">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="28ebf-148">Remplacez le `UseGoogleAuthentication` appel de méthode dans le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="28ebf-148">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="28ebf-149">Appeler le `AddGoogle` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="28ebf-149">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="28ebf-150">Authentification de Microsoft Account</span><span class="sxs-lookup"><span data-stu-id="28ebf-150">Microsoft Account authentication</span></span>

<span data-ttu-id="28ebf-151">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="28ebf-151">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="28ebf-152">Remplacez le `UseMicrosoftAccountAuthentication` appel de méthode dans le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="28ebf-152">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="28ebf-153">Appeler le `AddMicrosoftAccount` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="28ebf-153">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a><span data-ttu-id="28ebf-154">Authentification Twitter</span><span class="sxs-lookup"><span data-stu-id="28ebf-154">Twitter authentication</span></span>

<span data-ttu-id="28ebf-155">Apportez les modifications suivantes dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="28ebf-155">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="28ebf-156">Remplacez le `UseTwitterAuthentication` appel de méthode dans le `Configure` méthode avec `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="28ebf-156">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="28ebf-157">Appeler le `AddTwitter` méthode dans le `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="28ebf-157">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="28ebf-158">Définition des schémas d’authentification par défaut</span><span class="sxs-lookup"><span data-stu-id="28ebf-158">Setting default authentication schemes</span></span>

<span data-ttu-id="28ebf-159">Dans la version 1.x, le `AutomaticAuthenticate` et `AutomaticChallenge` propriétés de la [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) classe de base ont été conçue pour être définie sur un schéma d’authentification unique.</span><span class="sxs-lookup"><span data-stu-id="28ebf-159">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="28ebf-160">Il n’a aucune bonne façon d’appliquer cette recommandation.</span><span class="sxs-lookup"><span data-stu-id="28ebf-160">There was no good way to enforce this.</span></span>

<span data-ttu-id="28ebf-161">Dans la version 2.0, ces deux propriétés ont été supprimées en tant que propriétés sur chaque `AuthenticationOptions` instance.</span><span class="sxs-lookup"><span data-stu-id="28ebf-161">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="28ebf-162">Ils peuvent être configurés dans le `AddAuthentication` appel de méthode dans le `ConfigureServices` méthode de *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="28ebf-162">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="28ebf-163">Dans l’extrait de code précédent, le schéma par défaut est défini sur `CookieAuthenticationDefaults.AuthenticationScheme` (« Cookies »).</span><span class="sxs-lookup"><span data-stu-id="28ebf-163">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="28ebf-164">Vous pouvez également utiliser une version surchargée de la `AddAuthentication` pour définir plusieurs propriétés.</span><span class="sxs-lookup"><span data-stu-id="28ebf-164">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="28ebf-165">Dans l’exemple suivant de la méthode surchargée, le schéma par défaut a la valeur `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="28ebf-165">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="28ebf-166">Le schéma d’authentification peut également être spécifié dans votre personne `[Authorize]` attributs ou des stratégies d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="28ebf-166">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="28ebf-167">Définir un schéma par défaut dans 2.0 si une des conditions suivantes est remplie :</span><span class="sxs-lookup"><span data-stu-id="28ebf-167">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="28ebf-168">Vous souhaitez que l’utilisateur de se connecter automatiquement</span><span class="sxs-lookup"><span data-stu-id="28ebf-168">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="28ebf-169">Vous utilisez le `[Authorize]` stratégies d’attribut ou d’autorisation sans spécifier de schémas</span><span class="sxs-lookup"><span data-stu-id="28ebf-169">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="28ebf-170">Une exception à cette règle est la `AddIdentity` (méthode).</span><span class="sxs-lookup"><span data-stu-id="28ebf-170">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="28ebf-171">Cette méthode ajoute des cookies pour vous et définit la valeur par défaut s’authentifier et défiez les schémas au cookie application `IdentityConstants.ApplicationScheme`.</span><span class="sxs-lookup"><span data-stu-id="28ebf-171">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="28ebf-172">En outre, il définit le schéma de connexion par défaut sur le cookie externe `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="28ebf-172">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="28ebf-173">Utiliser des extensions d’authentification HttpContext</span><span class="sxs-lookup"><span data-stu-id="28ebf-173">Use HttpContext authentication extensions</span></span>

<span data-ttu-id="28ebf-174">Le `IAuthenticationManager` interface est le point d’entrée principal dans le système d’authentification 1.x.</span><span class="sxs-lookup"><span data-stu-id="28ebf-174">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="28ebf-175">Il a été remplacé par un nouvel ensemble de `HttpContext` méthodes d’extension dans le `Microsoft.AspNetCore.Authentication` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="28ebf-175">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="28ebf-176">Par exemple, les projets 1.x référence un `Authentication` propriété :</span><span class="sxs-lookup"><span data-stu-id="28ebf-176">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="28ebf-177">Dans les 2.0 projets, vous devez importer le `Microsoft.AspNetCore.Authentication` espace de noms et supprimer le `Authentication` références de propriété :</span><span class="sxs-lookup"><span data-stu-id="28ebf-177">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="28ebf-178">L’authentification Windows (HTTP.sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="28ebf-178">Windows Authentication (HTTP.sys / IISIntegration)</span></span>

<span data-ttu-id="28ebf-179">Il existe deux variantes de l’authentification Windows :</span><span class="sxs-lookup"><span data-stu-id="28ebf-179">There are two variations of Windows authentication:</span></span>

* <span data-ttu-id="28ebf-180">L’hôte autorise uniquement les utilisateurs authentifiés.</span><span class="sxs-lookup"><span data-stu-id="28ebf-180">The host only allows authenticated users.</span></span> <span data-ttu-id="28ebf-181">Cette variation n’est pas affectée par les modifications de 2.0.</span><span class="sxs-lookup"><span data-stu-id="28ebf-181">This variation isn't affected by the 2.0 changes.</span></span>
* <span data-ttu-id="28ebf-182">L’hôte permet à la fois anonymes et les utilisateurs authentifiés.</span><span class="sxs-lookup"><span data-stu-id="28ebf-182">The host allows both anonymous and authenticated users.</span></span> <span data-ttu-id="28ebf-183">Cette variation est affectée par les modifications de 2.0.</span><span class="sxs-lookup"><span data-stu-id="28ebf-183">This variation is affected by the 2.0 changes.</span></span> <span data-ttu-id="28ebf-184">Par exemple, l’application doit autoriser les utilisateurs anonymes à le [IIS](xref:host-and-deploy/iis/index) ou [HTTP.sys](xref:fundamentals/servers/httpsys) de couche, mais autoriser les utilisateurs au niveau du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="28ebf-184">For example, the app should allow anonymous users at the [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys) layer but authorize users at the controller level.</span></span> <span data-ttu-id="28ebf-185">Dans ce scénario, définissez le schéma par défaut dans le `Startup.ConfigureServices` (méthode).</span><span class="sxs-lookup"><span data-stu-id="28ebf-185">In this scenario, set the default scheme in the `Startup.ConfigureServices` method.</span></span>

  <span data-ttu-id="28ebf-186">Pour [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/), le schéma par défaut la valeur `IISDefaults.AuthenticationScheme`:</span><span class="sxs-lookup"><span data-stu-id="28ebf-186">For [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/), set the default scheme to `IISDefaults.AuthenticationScheme`:</span></span>

  ```csharp
  using Microsoft.AspNetCore.Server.IISIntegration;

  services.AddAuthentication(IISDefaults.AuthenticationScheme);
  ```

  <span data-ttu-id="28ebf-187">Pour [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/), le schéma par défaut la valeur `HttpSysDefaults.AuthenticationScheme`:</span><span class="sxs-lookup"><span data-stu-id="28ebf-187">For [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/), set the default scheme to `HttpSysDefaults.AuthenticationScheme`:</span></span>

  ```csharp
  using Microsoft.AspNetCore.Server.HttpSys;

  services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
  ```

  <span data-ttu-id="28ebf-188">Impossible de définir le schéma par défaut empêche la demande authorize (test) de fonctionner avec l’exception suivante :</span><span class="sxs-lookup"><span data-stu-id="28ebf-188">Failure to set the default scheme prevents the authorize (challenge) request from working with the following exception:</span></span>

  > <span data-ttu-id="28ebf-189">`System.InvalidOperationException`: Aucun authenticationScheme a été spécifié et il n’a aucun DefaultChallengeScheme trouvé.</span><span class="sxs-lookup"><span data-stu-id="28ebf-189">`System.InvalidOperationException`: No authenticationScheme was specified, and there was no DefaultChallengeScheme found.</span></span>

<span data-ttu-id="28ebf-190">Pour plus d'informations, consultez <xref:security/authentication/windowsauth>.</span><span class="sxs-lookup"><span data-stu-id="28ebf-190">For more information, see <xref:security/authentication/windowsauth>.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="28ebf-191">Instances de IdentityCookieOptions</span><span class="sxs-lookup"><span data-stu-id="28ebf-191">IdentityCookieOptions instances</span></span>

<span data-ttu-id="28ebf-192">Un effet secondaire des 2.0 modifications est le paramètre permet à l’aide de ces options au lieu d’instances d’options de cookie.</span><span class="sxs-lookup"><span data-stu-id="28ebf-192">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="28ebf-193">La possibilité de personnaliser les noms de schéma identité cookie est supprimée.</span><span class="sxs-lookup"><span data-stu-id="28ebf-193">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="28ebf-194">Par exemple, des projets 1.x utilisent [l’injection de constructeur](xref:mvc/controllers/dependency-injection#constructor-injection) à passer un `IdentityCookieOptions` paramètre dans *AccountController.cs* et *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="28ebf-194">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs* and *ManageController.cs*.</span></span> <span data-ttu-id="28ebf-195">Le schéma d’authentification externe cookie est accessible à partir de l’instance fournie :</span><span class="sxs-lookup"><span data-stu-id="28ebf-195">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="28ebf-196">L’injection de constructeur mentionnés ci-dessus devienne inutile dans les 2.0 projets et le `_externalCookieScheme` champ peut être supprimé :</span><span class="sxs-lookup"><span data-stu-id="28ebf-196">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="28ebf-197">les projets 1.x utilisés le `_externalCookieScheme` champ comme suit :</span><span class="sxs-lookup"><span data-stu-id="28ebf-197">1.x projects used the `_externalCookieScheme` field as follows:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="28ebf-198">Dans les 2.0 projets, remplacez le code précédent avec les éléments suivants.</span><span class="sxs-lookup"><span data-stu-id="28ebf-198">In 2.0 projects, replace the preceding code with the following.</span></span> <span data-ttu-id="28ebf-199">Le `IdentityConstants.ExternalScheme` constante peut être utilisée directement.</span><span class="sxs-lookup"><span data-stu-id="28ebf-199">The `IdentityConstants.ExternalScheme` constant can be used directly.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="28ebf-200">Résoudre récemment ajouté `SignOutAsync` appeler en important l’espace de noms suivant :</span><span class="sxs-lookup"><span data-stu-id="28ebf-200">Resolve the newly added `SignOutAsync` call by importing the following namespace:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationImport)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="28ebf-201">Ajouter des propriétés de navigation IdentityUser POCO</span><span class="sxs-lookup"><span data-stu-id="28ebf-201">Add IdentityUser POCO navigation properties</span></span>

<span data-ttu-id="28ebf-202">Les propriétés de navigation d’Entity Framework (EF) Core de la base de `IdentityUser` POCO (Plain Old CLR Object) ont été supprimés.</span><span class="sxs-lookup"><span data-stu-id="28ebf-202">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="28ebf-203">Si votre projet 1.x utilisé ces propriétés, les ajouter manuellement au projet 2.0 :</span><span class="sxs-lookup"><span data-stu-id="28ebf-203">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

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

<span data-ttu-id="28ebf-204">Pour empêcher les clés étrangères en double lors de l’exécution des Migrations d’EF Core, ajoutez le code suivant à votre `IdentityDbContext` classe `OnModelCreating` (méthode) (une fois que le `base.OnModelCreating();` appeler) :</span><span class="sxs-lookup"><span data-stu-id="28ebf-204">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

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

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="28ebf-205">Remplacez GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="28ebf-205">Replace GetExternalAuthenticationSchemes</span></span>

<span data-ttu-id="28ebf-206">La méthode synchrone `GetExternalAuthenticationSchemes` a été supprimé en faveur d’une version asynchrone.</span><span class="sxs-lookup"><span data-stu-id="28ebf-206">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="28ebf-207">les projets 1.x ont le code suivant *Controllers/ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="28ebf-207">1.x projects have the following code in *Controllers/ManageController.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="28ebf-208">Cette méthode s’affiche dans *Views/Account/Login.cshtml* trop :</span><span class="sxs-lookup"><span data-stu-id="28ebf-208">This method appears in *Views/Account/Login.cshtml* too:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemes&highlight=2)]

<span data-ttu-id="28ebf-209">Dans les 2.0 projets, utilisez le <xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*> (méthode).</span><span class="sxs-lookup"><span data-stu-id="28ebf-209">In 2.0 projects, use the <xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*> method.</span></span> <span data-ttu-id="28ebf-210">La modification de *ManageController.cs* ressemble au code suivant :</span><span class="sxs-lookup"><span data-stu-id="28ebf-210">The change in *ManageController.cs* resembles the following code:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="28ebf-211">Dans *Login.cshtml*, le `AuthenticationScheme` propriété accédée dans le `foreach` boucle devient `Name`:</span><span class="sxs-lookup"><span data-stu-id="28ebf-211">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemesAsync&highlight=2,19)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="28ebf-212">Modification de propriété ManageLoginsViewModel</span><span class="sxs-lookup"><span data-stu-id="28ebf-212">ManageLoginsViewModel property change</span></span>

<span data-ttu-id="28ebf-213">Un `ManageLoginsViewModel` objet est utilisé dans le `ManageLogins` action de *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="28ebf-213">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="28ebf-214">Dans les projets 1.x, l’objet `OtherLogins` propriété type de retour est `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="28ebf-214">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="28ebf-215">Ce type de retour nécessite une importation de `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="28ebf-215">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="28ebf-216">Dans les 2.0 projets, le type de retour devient `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="28ebf-216">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="28ebf-217">Ce nouveau type de retour nécessite en remplaçant le `Microsoft.AspNetCore.Http.Authentication` importer avec un `Microsoft.AspNetCore.Authentication` importer.</span><span class="sxs-lookup"><span data-stu-id="28ebf-217">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="28ebf-218">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="28ebf-218">Additional resources</span></span>

<span data-ttu-id="28ebf-219">Pour plus d’informations, consultez le [Discussion pour Auth 2.0](https://github.com/aspnet/Security/issues/1338) problème sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="28ebf-219">For more information, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
