---
title: Accéder à HttpContext dans ASP.NET Core
author: coderandhiker
description: Découvrez comment accéder à HttpContext dans ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/27/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: 373c036e0839ce51259e23f8503fbe4691b48751
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64886514"
---
# <a name="access-httpcontext-in-aspnet-core"></a>Accéder à HttpContext dans ASP.NET Core

Les applications ASP.NET Core accèdent à `HttpContext` via l’interface [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) et son implémentation par défaut [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor). Il est seulement nécessaire d’utiliser `IHttpContextAccessor` quand vous avez besoin d’accéder à `HttpContext` à l’intérieur d’un service.

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a>Utiliser HttpContext à partir de Razor Pages

[PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) de Razor Pages expose la propriété [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) :

```csharp
public class AboutModel : PageModel
{
    public string Message { get; set; }

    public void OnGet()
    {
        Message = HttpContext.Request.PathBase;
    }
}
```

::: moniker-end

## <a name="use-httpcontext-from-a-razor-view"></a>Utiliser HttpContext à partir d’une vue Razor

Les vues Razor exposent `HttpContext` directement par l’intermédiaire d’une propriété [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) de la vue. L’exemple suivant récupère le nom d’utilisateur actif dans une application Intranet en utilisant l’authentification Windows :

```cshtml
@{
    var username = Context.User.Identity.Name;
}
```

## <a name="use-httpcontext-from-a-controller"></a>Utiliser HttpContext à partir d’un contrôleur

Les contrôleurs exposent la propriété [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) :

```csharp
public class HomeController : Controller
{
    public IActionResult About()
    {
        var pathBase = HttpContext.Request.PathBase;
        // Do something with the PathBase.

        return View();
    }
}
```

## <a name="use-httpcontext-from-middleware"></a>Utiliser HttpContext à partir d’un intergiciel

Lorsque vous travaillez avec des composants d’intergiciel personnalisés, `HttpContext` est transmis à la méthode `Invoke` ou `InvokeAsync` et accessible lorsque l’intergiciel est configuré :

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a>Utiliser HttpContext à partir de composants personnalisés

Pour les autres infrastructures et les composants personnalisés qui requièrent l’accès à `HttpContext`, l’approche recommandée consiste à inscrire une dépendance à l’aide du conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) intégré. Le conteneur d’injection de dépendances fournit le `IHttpContextAccessor` à toutes les classes qui le déclarent en tant que dépendance dans leurs constructeurs.

::: moniker range=">= aspnetcore-2.1"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc()
         .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
     services.AddHttpContextAccessor();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc();
     services.AddSingleton<IHttpContextAccessor, HttpContextAccessor>();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

Dans l’exemple suivant :

* `UserRepository` déclare sa dépendance par rapport à `IHttpContextAccessor`.
* Il y a dépendance lorsque l’instance de dépendances résout la chaîne de dépendance et crée une instance de `UserRepository`.

```csharp
public class UserRepository : IUserRepository
{
    private readonly IHttpContextAccessor _httpContextAccessor;

    public UserRepository(IHttpContextAccessor httpContextAccessor)
    {
        _httpContextAccessor = httpContextAccessor;
    }

    public void LogCurrentUser()
    {
        var username = _httpContextAccessor.HttpContext.User.Identity.Name;
        service.LogAccessRequest(username);
    }
}
```

## <a name="httpcontext-access-from-a-background-thread"></a>Accès de HttpContext à partir d’un thread en arrière-plan

`HttpContext` n’est pas thread‑safe. La lecture ou l’écriture de propriétés de `HttpContext` en dehors du traitement d’une requête peut entraîner une exception `NullReferenceException`.

> [!NOTE]
> L’utilisation de `HttpContext` en dehors du traitement d’une requête entraîne souvent une exception `NullReferenceException`. Si votre application génère des `NullReferenceException` sporadiques, examinez les parties du code qui démarrent le traitement en arrière-plan ou qui continue le traitement après qu’une requête a abouti. Recherchez des erreurs comme une méthode de contrôleur qui serait définie en tant que `async void`.

Pour effectuer un travail en arrière-plan en toute sécurité avec des données `HttpContext` :

* Copiez les données nécessaires pendant le traitement de la requête.
* Transmettez les données copiées à une tâche en arrière-plan.

Pour éviter un code unsafe, ne transmettez jamais `HttpContext` dans une méthode qui effectue un travail en arrière-plan. Transmettez plutôt les données nécessaires.

```csharp
public class EmailController
{
    public ActionResult SendEmail(string email)
    {
        var correlationId = HttpContext.Request.Headers["x-correlation-id"].ToString();

        // Starts sending an email, but doesn't wait for it to complete
        _ = SendEmailCore(correlationId);
        return View();
    }

    private async Task SendEmailCore(string correlationId)
    {
        // send the email
    }
}
