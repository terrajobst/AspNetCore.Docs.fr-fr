---
title: Autorisation avec un schéma spécifique dans ASP.NET Core
author: rick-anderson
description: Cet article explique comment limiter l’identité à un schéma spécifique lors de l’utilisation de plusieurs méthodes d’authentification.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 11/08/2019
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 38da80519b9d5d097c24d38b5a37503174629fc4
ms.sourcegitcommit: 4818385c3cfe0805e15138a2c1785b62deeaab90
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/09/2019
ms.locfileid: "73896972"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a><span data-ttu-id="245c2-103">Autorisation avec un schéma spécifique dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="245c2-103">Authorize with a specific scheme in ASP.NET Core</span></span>

<span data-ttu-id="245c2-104">Dans certains scénarios, tels que les applications à page unique (SPAs), il est courant d’utiliser plusieurs méthodes d’authentification.</span><span class="sxs-lookup"><span data-stu-id="245c2-104">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="245c2-105">Par exemple, l’application peut utiliser l’authentification basée sur les cookies pour se connecter et l’authentification du porteur JWT pour les demandes JavaScript.</span><span class="sxs-lookup"><span data-stu-id="245c2-105">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="245c2-106">Dans certains cas, l’application peut avoir plusieurs instances d’un gestionnaire d’authentification.</span><span class="sxs-lookup"><span data-stu-id="245c2-106">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="245c2-107">Par exemple, deux gestionnaires de cookies où l’un contient une identité de base et l’autre est créé lorsqu’une authentification multifacteur (MFA) a été déclenchée.</span><span class="sxs-lookup"><span data-stu-id="245c2-107">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="245c2-108">L’authentification multifacteur peut être déclenchée, car l’utilisateur a demandé une opération nécessitant une sécurité supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="245c2-108">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

<span data-ttu-id="245c2-109">Un schéma d’authentification est nommé lorsque le service d’authentification est configuré pendant l’authentification.</span><span class="sxs-lookup"><span data-stu-id="245c2-109">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="245c2-110">Exemple :</span><span class="sxs-lookup"><span data-stu-id="245c2-110">For example:</span></span>

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

<span data-ttu-id="245c2-111">Dans le code précédent, deux gestionnaires d’authentification ont été ajoutés : un pour les cookies et un pour le porteur.</span><span class="sxs-lookup"><span data-stu-id="245c2-111">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="245c2-112">La spécification du schéma par défaut entraîne la définition de la propriété `HttpContext.User` sur cette identité.</span><span class="sxs-lookup"><span data-stu-id="245c2-112">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="245c2-113">Si ce comportement n’est pas souhaité, désactivez-le en appelant la forme sans paramètre de `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="245c2-113">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="245c2-114">Sélection du schéma avec l’attribut Authorize</span><span class="sxs-lookup"><span data-stu-id="245c2-114">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="245c2-115">Au point d’autorisation, l’application indique le gestionnaire à utiliser.</span><span class="sxs-lookup"><span data-stu-id="245c2-115">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="245c2-116">Sélectionnez le gestionnaire avec lequel l’application autorisera en passant une liste de schémas d’authentification délimités par des virgules à `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="245c2-116">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="245c2-117">L’attribut `[Authorize]` spécifie le ou les schémas d’authentification à utiliser, qu’une valeur par défaut soit configurée ou non.</span><span class="sxs-lookup"><span data-stu-id="245c2-117">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="245c2-118">Exemple :</span><span class="sxs-lookup"><span data-stu-id="245c2-118">For example:</span></span>

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

<span data-ttu-id="245c2-119">Dans l’exemple précédent, les gestionnaires de cookies et de porteur s’exécutent et ont la possibilité de créer et d’ajouter une identité pour l’utilisateur actuel.</span><span class="sxs-lookup"><span data-stu-id="245c2-119">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="245c2-120">En spécifiant un seul schéma, le gestionnaire correspondant s’exécute.</span><span class="sxs-lookup"><span data-stu-id="245c2-120">By specifying a single scheme only, the corresponding handler runs.</span></span>

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

<span data-ttu-id="245c2-121">Dans le code précédent, seul le gestionnaire avec le schéma « Bearer » s’exécute.</span><span class="sxs-lookup"><span data-stu-id="245c2-121">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="245c2-122">Toutes les identités basées sur les cookies sont ignorées.</span><span class="sxs-lookup"><span data-stu-id="245c2-122">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="245c2-123">Sélection du schéma avec des stratégies</span><span class="sxs-lookup"><span data-stu-id="245c2-123">Selecting the scheme with policies</span></span>

<span data-ttu-id="245c2-124">Si vous préférez spécifier les schémas souhaités dans la [stratégie](xref:security/authorization/policies), vous pouvez définir le regroupement `AuthenticationSchemes` lors de l’ajout de votre stratégie :</span><span class="sxs-lookup"><span data-stu-id="245c2-124">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

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

<span data-ttu-id="245c2-125">Dans l’exemple précédent, la stratégie « 18 ans » s’exécute uniquement sur l’identité créée par le gestionnaire « porteur ».</span><span class="sxs-lookup"><span data-stu-id="245c2-125">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="245c2-126">Utilisez la stratégie en définissant la propriété `Policy` de l’attribut `[Authorize]` :</span><span class="sxs-lookup"><span data-stu-id="245c2-126">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```

::: moniker range=">= aspnetcore-2.0"

## <a name="use-multiple-authentication-schemes"></a><span data-ttu-id="245c2-127">Utiliser plusieurs schémas d’authentification</span><span class="sxs-lookup"><span data-stu-id="245c2-127">Use multiple authentication schemes</span></span>

<span data-ttu-id="245c2-128">Certaines applications peuvent avoir besoin de prendre en charge plusieurs types d’authentification.</span><span class="sxs-lookup"><span data-stu-id="245c2-128">Some apps may need to support multiple types of authentication.</span></span> <span data-ttu-id="245c2-129">Par exemple, votre application peut authentifier les utilisateurs à partir de Azure Active Directory et à partir d’une base de données utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="245c2-129">For example, your app might authenticate users from Azure Active Directory and from a users database.</span></span> <span data-ttu-id="245c2-130">Un autre exemple est une application qui authentifie les utilisateurs à la fois Services ADFS et Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="245c2-130">Another example is an app that authenticates users from both Active Directory Federation Services and Azure Active Directory B2C.</span></span> <span data-ttu-id="245c2-131">Dans ce cas, l’application doit accepter un jeton de porteur JWT de plusieurs émetteurs.</span><span class="sxs-lookup"><span data-stu-id="245c2-131">In this case, the app should accept a JWT bearer token from several issuers.</span></span>

<span data-ttu-id="245c2-132">Ajoutez tous les schémas d’authentification que vous souhaitez accepter.</span><span class="sxs-lookup"><span data-stu-id="245c2-132">Add all authentication schemes you'd like to accept.</span></span> <span data-ttu-id="245c2-133">Par exemple, le code suivant dans `Startup.ConfigureServices` ajoute deux schémas d’authentification du porteur JWT avec différents émetteurs :</span><span class="sxs-lookup"><span data-stu-id="245c2-133">For example, the following code in `Startup.ConfigureServices` adds two JWT bearer authentication schemes with different issuers:</span></span>

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
> <span data-ttu-id="245c2-134">Une seule authentification du porteur JWT est enregistrée avec le schéma d’authentification par défaut `JwtBearerDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="245c2-134">Only one JWT bearer authentication is registered with the default authentication scheme `JwtBearerDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="245c2-135">Une authentification supplémentaire doit être inscrite avec un schéma d’authentification unique.</span><span class="sxs-lookup"><span data-stu-id="245c2-135">Additional authentication has to be registered with a unique authentication scheme.</span></span>

<span data-ttu-id="245c2-136">L’étape suivante consiste à mettre à jour la stratégie d’autorisation par défaut pour accepter les deux schémas d’authentification.</span><span class="sxs-lookup"><span data-stu-id="245c2-136">The next step is to update the default authorization policy to accept both authentication schemes.</span></span> <span data-ttu-id="245c2-137">Exemple :</span><span class="sxs-lookup"><span data-stu-id="245c2-137">For example:</span></span>

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

<span data-ttu-id="245c2-138">Étant donné que la stratégie d’autorisation par défaut est remplacée, il est possible d’utiliser l’attribut `[Authorize]` dans les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="245c2-138">As the default authorization policy is overridden, it's possible to use the `[Authorize]` attribute in controllers.</span></span> <span data-ttu-id="245c2-139">Le contrôleur accepte ensuite les demandes avec le jeton JWT émis par le premier ou le deuxième émetteur.</span><span class="sxs-lookup"><span data-stu-id="245c2-139">The controller then accepts requests with JWT issued by the first or second issuer.</span></span>

::: moniker-end
