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
ms.locfileid: "30072696"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a>Autoriser avec un modèle spécifique dans ASP.NET Core

Dans certains scénarios, tels que des Applications à Page unique (SPA), il est courant d’utiliser plusieurs méthodes d’authentification. Par exemple, l’application peut utiliser l’authentification par cookie pour vous connecter et l’authentification du support JSON pour les demandes de JavaScript. Dans certains cas, l’application peut avoir plusieurs instances d’un gestionnaire d’authentification. Par exemple, deux gestionnaires de cookie où un premier contient une identité de base et l’autre est créé lorsqu’une authentification multifacteur (MFA) a été déclenchée. L’authentification Multifacteur peut être déclenchée, car l’utilisateur a demandé une opération qui requiert une sécurité supplémentaire.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Un schéma d’authentification est appelé lorsque le service d’authentification est configuré lors de l’authentification. Par exemple :

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

Dans le code précédent, les deux gestionnaires d’authentification ont été ajoutés : une pour les cookies et l’autre pour le support.

>[!NOTE]
>Les spécifications de schéma par défaut entraînent la propriété `HttpContext.User` définie pour cette identité. Si ce comportement n’est pas souhaité, désactivez-le en appelant le formulaire sans paramètre de `AddAuthentication`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Les schémas d’authentification sont nommés lors de l’authentification middlewares et sont configurés lors de l’authentification. Par exemple :

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

Dans le code précédent, deux middlewares d’authentification ont été ajoutés : un pour les cookies et l’autre pour le support.

>[!NOTE]
>Les spécifications de schéma par défaut entraînent la propriété `HttpContext.User` définie pour cette identité. Si ce comportement n’est pas souhaité, désactivez-le en définissant la propriété `AuthenticationOptions.AutomaticAuthenticate` à `false`.

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>Sélection du schéma avec l’attribut Authorize

Dans la phase d’autorisation, l’application indique le gestionnaire à utiliser. Sélectionnez le gestionnaire avec lequel l’application autorise en passant une liste délimitée par des virgules des schémas d’authentification à `[Authorize]`. Le `[Authorize]` attribut spécifie le schéma d’authentification ou les schémas à utiliser que par défaut soit configuré. Par exemple :

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

Dans l’exemple précédent, le cookie et le support de gestionnaires s’exécuteront et ont la possibilité de créer et d'ajouter une identité pour l’utilisateur actuel. En spécifiant un schéma unique uniquement, le gestionnaire correspondant s’exécute.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

Dans le code précédent, seul le gestionnaire avec le schéma « Support » s’exécute. Toutes les identités basées sur les cookies sont ignorées.

## <a name="selecting-the-scheme-with-policies"></a>Sélection du schéma avec des stratégies

Si vous souhaitez spécifier les schémas souhaités dans [la stratégie](xref:security/authorization/policies), vous pouvez définir la collection `AuthenticationSchemes` lors de l’ajout de votre stratégie :

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

Dans l’exemple précédent, la stratégie « Over18 » ne s’exécute pas par rapport à l’identité créée par le gestionnaire « Support ». Utilisez la stratégie en définissant l’attribut `[Authorize]` avec sa propriété `Policy`:

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
