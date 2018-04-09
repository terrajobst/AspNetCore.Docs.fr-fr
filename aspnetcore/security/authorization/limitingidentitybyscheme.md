---
title: Autoriser avec un modèle spécifique dans ASP.NET Core
author: rick-anderson
description: Cet article explique comment limiter l’identité à un schéma spécifique lorsque vous travaillez avec plusieurs méthodes d’authentification.
manager: wpickett
ms.author: riande
ms.date: 10/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 81a01d7de8221fcb3bf90a108d9df6633ca2b696
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a><span data-ttu-id="6dbb4-103">Autoriser avec un modèle spécifique dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6dbb4-103">Authorize with a specific scheme in ASP.NET Core</span></span>

<span data-ttu-id="6dbb4-104">Dans certains scénarios, tels que des Applications à Page unique (SPA), il est courant d’utiliser plusieurs méthodes d’authentification.</span><span class="sxs-lookup"><span data-stu-id="6dbb4-104">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="6dbb4-105">Par exemple, l’application peut utiliser l’authentification par cookie pour vous connecter et l’authentification du support JSON pour les demandes de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6dbb4-105">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="6dbb4-106">Dans certains cas, l’application peut avoir plusieurs instances d’un gestionnaire d’authentification.</span><span class="sxs-lookup"><span data-stu-id="6dbb4-106">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="6dbb4-107">Par exemple, deux gestionnaires de cookie où un premier contient une identité de base et l’autre est créé lorsqu’une authentification multifacteur (MFA) a été déclenchée.</span><span class="sxs-lookup"><span data-stu-id="6dbb4-107">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="6dbb4-108">L’authentification Multifacteur peut être déclenchée, car l’utilisateur a demandé une opération qui requiert une sécurité supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="6dbb4-108">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6dbb4-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6dbb4-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6dbb4-110">Un schéma d’authentification est appelé lorsque le service d’authentification est configuré lors de l’authentification.</span><span class="sxs-lookup"><span data-stu-id="6dbb4-110">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="6dbb4-111">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="6dbb4-111">For example:</span></span>

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

<span data-ttu-id="6dbb4-112">Dans le code précédent, les deux gestionnaires d’authentification ont été ajoutés : une pour les cookies et l’autre pour le support.</span><span class="sxs-lookup"><span data-stu-id="6dbb4-112">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="6dbb4-113">Les spécifications de schéma par défaut entraînent la propriété `HttpContext.User` définie pour cette identité.</span><span class="sxs-lookup"><span data-stu-id="6dbb4-113">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="6dbb4-114">Si ce comportement n’est pas souhaité, désactivez-le en appelant le formulaire sans paramètre de `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="6dbb4-114">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6dbb4-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6dbb4-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6dbb4-116">Les schémas d’authentification sont nommés lors de l’authentification middlewares et sont configurés lors de l’authentification.</span><span class="sxs-lookup"><span data-stu-id="6dbb4-116">Authentication schemes are named when authentication middlewares are configured during authentication.</span></span> <span data-ttu-id="6dbb4-117">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="6dbb4-117">For example:</span></span>

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

<span data-ttu-id="6dbb4-118">Dans le code précédent, deux middlewares d’authentification ont été ajoutés : un pour les cookies et l’autre pour le support.</span><span class="sxs-lookup"><span data-stu-id="6dbb4-118">In the preceding code, two authentication middlewares have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="6dbb4-119">Les spécifications de schéma par défaut entraînent la propriété `HttpContext.User` définie pour cette identité.</span><span class="sxs-lookup"><span data-stu-id="6dbb4-119">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="6dbb4-120">Si ce comportement n’est pas souhaité, désactivez-le en définissant la propriété `AuthenticationOptions.AutomaticAuthenticate` à `false`.</span><span class="sxs-lookup"><span data-stu-id="6dbb4-120">If that behavior isn't desired, disable it by setting the `AuthenticationOptions.AutomaticAuthenticate` property to `false`.</span></span>

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="6dbb4-121">Sélection du schéma avec l’attribut Authorize</span><span class="sxs-lookup"><span data-stu-id="6dbb4-121">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="6dbb4-122">Dans la phase d’autorisation, l’application indique le gestionnaire à utiliser.</span><span class="sxs-lookup"><span data-stu-id="6dbb4-122">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="6dbb4-123">Sélectionnez le gestionnaire avec lequel l’application autorise en passant une liste délimitée par des virgules des schémas d’authentification à `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="6dbb4-123">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="6dbb4-124">Le `[Authorize]` attribut spécifie le schéma d’authentification ou les schémas à utiliser que par défaut soit configuré.</span><span class="sxs-lookup"><span data-stu-id="6dbb4-124">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="6dbb4-125">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="6dbb4-125">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6dbb4-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6dbb4-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6dbb4-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6dbb4-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="6dbb4-128">Dans l’exemple précédent, le cookie et le support de gestionnaires s’exécuteront et ont la possibilité de créer et d'ajouter une identité pour l’utilisateur actuel.</span><span class="sxs-lookup"><span data-stu-id="6dbb4-128">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="6dbb4-129">En spécifiant un schéma unique uniquement, le gestionnaire correspondant s’exécute.</span><span class="sxs-lookup"><span data-stu-id="6dbb4-129">By specifying a single scheme only, the corresponding handler runs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6dbb4-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6dbb4-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6dbb4-131">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6dbb4-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

<span data-ttu-id="6dbb4-132">Dans le code précédent, seul le gestionnaire avec le schéma « Support » s’exécute.</span><span class="sxs-lookup"><span data-stu-id="6dbb4-132">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="6dbb4-133">Toutes les identités basées sur les cookies sont ignorées.</span><span class="sxs-lookup"><span data-stu-id="6dbb4-133">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="6dbb4-134">Sélection du schéma avec des stratégies</span><span class="sxs-lookup"><span data-stu-id="6dbb4-134">Selecting the scheme with policies</span></span>

<span data-ttu-id="6dbb4-135">Si vous souhaitez spécifier les schémas souhaités dans [la stratégie](xref:security/authorization/policies), vous pouvez définir la collection `AuthenticationSchemes` lors de l’ajout de votre stratégie :</span><span class="sxs-lookup"><span data-stu-id="6dbb4-135">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

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

<span data-ttu-id="6dbb4-136">Dans l’exemple précédent, la stratégie « Over18 » ne s’exécute pas par rapport à l’identité créée par le gestionnaire « Support ».</span><span class="sxs-lookup"><span data-stu-id="6dbb4-136">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="6dbb4-137">Utilisez la stratégie en définissant l’attribut `[Authorize]` avec sa propriété `Policy`:</span><span class="sxs-lookup"><span data-stu-id="6dbb4-137">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
