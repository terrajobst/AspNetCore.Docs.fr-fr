---
title: Autoriser avec un schéma spécifique dans ASP.NET Core
author: rick-anderson
description: Cet article explique comment limiter l’identité à un schéma spécifique lorsque vous travaillez avec plusieurs méthodes d’authentification.
ms.author: riande
ms.date: 10/22/2018
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 778bb61f472ab2e76f85da5999d3c79238188f19
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248197"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a><span data-ttu-id="febbe-103">Autoriser avec un schéma spécifique dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="febbe-103">Authorize with a specific scheme in ASP.NET Core</span></span>

<span data-ttu-id="febbe-104">Dans certains scénarios, tels que des Applications à Page unique (SPA), il est courant d’utiliser plusieurs méthodes d’authentification.</span><span class="sxs-lookup"><span data-stu-id="febbe-104">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="febbe-105">Par exemple, l’application peut utiliser l’authentification par cookie pour vous connecter et l’authentification du support JSON pour les demandes de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="febbe-105">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="febbe-106">Dans certains cas, l’application peut avoir plusieurs instances d’un gestionnaire d’authentification.</span><span class="sxs-lookup"><span data-stu-id="febbe-106">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="febbe-107">Par exemple, deux gestionnaires de cookie où un premier contient une identité de base et l’autre est créé lorsqu’une authentification multifacteur (MFA) a été déclenchée.</span><span class="sxs-lookup"><span data-stu-id="febbe-107">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="febbe-108">L’authentification Multifacteur peut être déclenchée, car l’utilisateur a demandé une opération qui requiert une sécurité supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="febbe-108">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="febbe-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="febbe-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="febbe-110">Un schéma d’authentification est appelé lorsque le service d’authentification est configuré lors de l’authentification.</span><span class="sxs-lookup"><span data-stu-id="febbe-110">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="febbe-111">Exemple :</span><span class="sxs-lookup"><span data-stu-id="febbe-111">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication()
        .AddCookie(options => {
            options.LoginPath = "/Account/Unauthorized/";
            options.AccessDeniedPath = "/Account/Forbidden/";
        })
        .AddJwtBearer(options => {
            options.Audience = "http://localhost:5001/";
            options.Authority = "http://localhost:5000/";
        });
```

<span data-ttu-id="febbe-112">Dans le code précédent, les deux gestionnaires d’authentification ont été ajoutés : un pour les cookies et l’autre pour porteur.</span><span class="sxs-lookup"><span data-stu-id="febbe-112">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="febbe-113">Spécifier un schéma par défaut entraîne que la propriété `HttpContext.User` soit définie pour cette identité.</span><span class="sxs-lookup"><span data-stu-id="febbe-113">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="febbe-114">Si ce comportement n’est pas souhaité, désactivez-le en appelant le formulaire sans paramètre de `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="febbe-114">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="febbe-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="febbe-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="febbe-116">Les schémas d’authentification sont nommés lors de l’authentification middlewares et sont configurés lors de l’authentification.</span><span class="sxs-lookup"><span data-stu-id="febbe-116">Authentication schemes are named when authentication middlewares are configured during authentication.</span></span> <span data-ttu-id="febbe-117">Exemple :</span><span class="sxs-lookup"><span data-stu-id="febbe-117">For example:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    // Code omitted for brevity

    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AuthenticationScheme = "Cookie",
        LoginPath = "/Account/Unauthorized/",
        AccessDeniedPath = "/Account/Forbidden/",
        AutomaticAuthenticate = false
    });
    
    app.UseJwtBearerAuthentication(new JwtBearerOptions()
    {
        AuthenticationScheme = "Bearer",
        AutomaticAuthenticate = false,
        Audience = "http://localhost:5001/",
        Authority = "http://localhost:5000/",
        RequireHttpsMetadata = false
    });
```

<span data-ttu-id="febbe-118">Dans le code précédent, deux middlewares d’authentification ont été ajoutés : un pour les cookies et l’autre pour le support.</span><span class="sxs-lookup"><span data-stu-id="febbe-118">In the preceding code, two authentication middlewares have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="febbe-119">Spécifier un schéma par défaut entraîne que la propriété `HttpContext.User` soit définie pour cette identité.</span><span class="sxs-lookup"><span data-stu-id="febbe-119">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="febbe-120">Si ce comportement n’est pas souhaité, désactivez-la en définissant le `AuthenticationOptions.AutomaticAuthenticate` propriété `false`.</span><span class="sxs-lookup"><span data-stu-id="febbe-120">If that behavior isn't desired, disable it by setting the `AuthenticationOptions.AutomaticAuthenticate` property to `false`.</span></span>

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="febbe-121">Sélectionnez le schéma avec l’attribut Authorize</span><span class="sxs-lookup"><span data-stu-id="febbe-121">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="febbe-122">Au moment de l’autorisation, l’application indique le gestionnaire à utiliser.</span><span class="sxs-lookup"><span data-stu-id="febbe-122">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="febbe-123">Sélectionnez le gestionnaire avec lequel l’application autorise en passant une liste délimitée par des virgules des schémas d’authentification pour `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="febbe-123">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="febbe-124">Le `[Authorize]` attribut spécifie le schéma d’authentification ou les schémas à utiliser que par défaut soit configuré.</span><span class="sxs-lookup"><span data-stu-id="febbe-124">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="febbe-125">Exemple :</span><span class="sxs-lookup"><span data-stu-id="febbe-125">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="febbe-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="febbe-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="febbe-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="febbe-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

---

<span data-ttu-id="febbe-128">Dans l’exemple précédent, le cookie et le support de gestionnaires s’exécuteront et ont la possibilité de créer et d'ajouter une identité pour l’utilisateur actuel.</span><span class="sxs-lookup"><span data-stu-id="febbe-128">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="febbe-129">En spécifiant un schéma unique uniquement, le gestionnaire correspondant s’exécute.</span><span class="sxs-lookup"><span data-stu-id="febbe-129">By specifying a single scheme only, the corresponding handler runs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="febbe-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="febbe-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="febbe-131">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="febbe-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

<span data-ttu-id="febbe-132">Dans le code précédent, seul le gestionnaire avec le schéma « PORTEUR » s’exécute.</span><span class="sxs-lookup"><span data-stu-id="febbe-132">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="febbe-133">Aucune identité basée sur les cookies est ignorées.</span><span class="sxs-lookup"><span data-stu-id="febbe-133">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="febbe-134">Sélectionnez le schéma avec des stratégies</span><span class="sxs-lookup"><span data-stu-id="febbe-134">Selecting the scheme with policies</span></span>

<span data-ttu-id="febbe-135">Si vous souhaitez spécifier les schémas souhaités dans [la stratégie](xref:security/authorization/policies), vous pouvez définir la collection `AuthenticationSchemes` lors de l’ajout de votre stratégie :</span><span class="sxs-lookup"><span data-stu-id="febbe-135">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("Over18", policy =>
    {
        policy.AuthenticationSchemes.Add(JwtBearerDefaults.AuthenticationScheme);
        policy.RequireAuthenticatedUser();
        policy.Requirements.Add(new MinimumAgeRequirement());
    });
});
```

<span data-ttu-id="febbe-136">Dans l’exemple précédent, la stratégie « Over18 » ne s’exécute pas par rapport à l’identité créée par le gestionnaire « Support ».</span><span class="sxs-lookup"><span data-stu-id="febbe-136">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="febbe-137">Utilisez la stratégie en définissant l’attribut `[Authorize]` avec sa propriété `Policy`:</span><span class="sxs-lookup"><span data-stu-id="febbe-137">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```

::: moniker range=">= aspnetcore-2.0"

## <a name="use-multiple-authentication-schemes"></a><span data-ttu-id="febbe-138">Utiliser plusieurs schémas d’authentification</span><span class="sxs-lookup"><span data-stu-id="febbe-138">Use multiple authentication schemes</span></span>

<span data-ttu-id="febbe-139">Certaines applications peuvent devoir prendre en charge plusieurs types d’authentification.</span><span class="sxs-lookup"><span data-stu-id="febbe-139">Some apps may need to support multiple types of authentication.</span></span> <span data-ttu-id="febbe-140">Par exemple, votre application peut authentifier les utilisateurs d’Azure Active Directory et à partir d’une base de données des utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="febbe-140">For example, your app might authenticate users from Azure Active Directory and from a users database.</span></span> <span data-ttu-id="febbe-141">Un autre exemple est une application qui authentifie les utilisateurs à partir d’Active Directory Federation Services et Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="febbe-141">Another example is an app that authenticates users from both Active Directory Federation Services and Azure Active Directory B2C.</span></span> <span data-ttu-id="febbe-142">Dans ce cas, l’application doit accepter un jeton de porteur JWT à partir de plusieurs émetteurs.</span><span class="sxs-lookup"><span data-stu-id="febbe-142">In this case, the app should accept a JWT bearer token from several issuers.</span></span>

<span data-ttu-id="febbe-143">Ajouter tous les schémas d’authentification que vous souhaitez accepter.</span><span class="sxs-lookup"><span data-stu-id="febbe-143">Add all authentication schemes you'd like to accept.</span></span> <span data-ttu-id="febbe-144">Par exemple, le code suivant dans `Startup.ConfigureServices` ajoute deux schémas d’authentification du porteur JWT avec des émetteurs différents :</span><span class="sxs-lookup"><span data-stu-id="febbe-144">For example, the following code in `Startup.ConfigureServices` adds two JWT bearer authentication schemes with different issuers:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
        .AddJwtBearer(options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://localhost:5000/identity/";
        })
        .AddJwtBearer("AzureAD", options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://login.microsoftonline.com/eb971100-6f99-4bdc-8611-1bc8edd7f436/";
        });
}
```

> [!NOTE]
> <span data-ttu-id="febbe-145">Seule l’authentification de porteur JWT est inscrit avec le schéma d’authentification par défaut `JwtBearerDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="febbe-145">Only one JWT bearer authentication is registered with the default authentication scheme `JwtBearerDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="febbe-146">Une authentification supplémentaire doit être enregistré avec un schéma d’authentification unique.</span><span class="sxs-lookup"><span data-stu-id="febbe-146">Additional authentication has to be registered with a unique authentication scheme.</span></span>

<span data-ttu-id="febbe-147">L’étape suivante consiste à mettre à jour la stratégie d’autorisation par défaut pour accepter les deux schémas d’authentification.</span><span class="sxs-lookup"><span data-stu-id="febbe-147">The next step is to update the default authorization policy to accept both authentication schemes.</span></span> <span data-ttu-id="febbe-148">Exemple :</span><span class="sxs-lookup"><span data-stu-id="febbe-148">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthorization(options =>
    {
        var defaultAuthorizationPolicyBuilder = new AuthorizationPolicyBuilder(
            JwtBearerDefaults.AuthenticationScheme,
            "AzureAD");
        defaultAuthorizationPolicyBuilder = 
            defaultAuthorizationPolicyBuilder.RequireAuthenticatedUser();
        options.DefaultPolicy = defaultAuthorizationPolicyBuilder.Build();
    });
}
```

<span data-ttu-id="febbe-149">Comme la substitution de la stratégie d’autorisation par défaut, il est possible d’utiliser le `[Authorize]` attribut dans les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="febbe-149">As the default authorization policy is overridden, it's possible to use the `[Authorize]` attribute in controllers.</span></span> <span data-ttu-id="febbe-150">Puis, le contrôleur accepte les demandes avec le jeton JWT émis par l’émetteur de la première ou deuxième.</span><span class="sxs-lookup"><span data-stu-id="febbe-150">The controller then accepts requests with JWT issued by the first or second issuer.</span></span>

::: moniker-end
