---
title: Migrer à partir de ClaimsPrincipal. Current
author: mjrousos
description: Découvrez comment migrer à partir de ClaimsPrincipal. Current pour récupérer l’identité et les revendications de l’utilisateur authentifié actuel dans ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/26/2019
uid: migration/claimsprincipal-current
ms.openlocfilehash: f7472f5b851d3869da3d26b881e276ce4ca004fb
ms.sourcegitcommit: 231780c8d7848943e5e9fd55e93f437f7e5a371d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2019
ms.locfileid: "74115967"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>Migrer à partir de ClaimsPrincipal. Current

Dans les projets ASP.NET 4. x, il était courant d’utiliser [ClaimsPrincipal. Current](/dotnet/api/system.security.claims.claimsprincipal.current) pour récupérer l’identité et les revendications de l’utilisateur authentifié actuel. Dans ASP.NET Core, cette propriété n’est plus définie. Le code qui dépend de lui doit être mis à jour pour obtenir l’identité de l’utilisateur authentifié actuel par le biais d’un autre moyen.

## <a name="context-specific-data-instead-of-static-data"></a>Données spécifiques au contexte au lieu de données statiques

Lorsque vous utilisez ASP.NET Core, les valeurs des `ClaimsPrincipal.Current` et `Thread.CurrentPrincipal` ne sont pas définies. Ces propriétés représentent l’état statique, ce qui ASP.NET Core en général évite. Au lieu de cela, l’architecture de ASP.NET Core consiste à récupérer les dépendances (telles que l’identité de l’utilisateur actuel) à partir de collections de services spécifiques au contexte (à l’aide de son modèle d' [injection de dépendances](xref:fundamentals/dependency-injection) ). De plus, `Thread.CurrentPrincipal` est thread static, il peut donc ne pas conserver les modifications dans certains scénarios asynchrones (et `ClaimsPrincipal.Current` appelle simplement `Thread.CurrentPrincipal` par défaut).

Pour comprendre le genre de problèmes que les membres statiques de thread peuvent entraîner dans les scénarios asynchrones, examinez l’extrait de code suivant :

```csharp
// Create a ClaimsPrincipal and set Thread.CurrentPrincipal
var identity = new ClaimsIdentity();
identity.AddClaim(new Claim(ClaimTypes.Name, "User1"));
Thread.CurrentPrincipal = new ClaimsPrincipal(identity);

// Check the current user
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");

// For the method to complete asynchronously
await Task.Yield();

// Check the current user after
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");
```

L’exemple de code précédent définit `Thread.CurrentPrincipal` et vérifie sa valeur avant et après l’attente d’un appel asynchrone. `Thread.CurrentPrincipal` est spécifique au *thread* sur lequel il est défini, et la méthode est susceptible de reprendre l’exécution sur un thread différent après l’expression await. Par conséquent, `Thread.CurrentPrincipal` est présent lorsqu’il est vérifié pour la première fois, mais il a la valeur null après l’appel à `await Task.Yield()`.

L’obtention de l’identité de l’utilisateur actuel à partir de la collection d’injection de services de l’application est également plus testable, car les identités de test peuvent être facilement injectées.

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>Récupérer l’utilisateur actuel dans une application ASP.NET Core

Il existe plusieurs options pour récupérer le `ClaimsPrincipal` de l’utilisateur authentifié actuel dans ASP.NET Core à la place des `ClaimsPrincipal.Current`:

* **ControllerBase. utilisateur**. Les contrôleurs MVC peuvent accéder à l’utilisateur authentifié actuel avec leur propriété [User](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) .
* **HttpContext. User**. Les composants ayant accès au `HttpContext` actuel (middleware, par exemple) peuvent obtenir le `ClaimsPrincipal` de l’utilisateur actuel de [HttpContext. User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).
* **Passé à partir de l’appelant**. Les bibliothèques sans accès au `HttpContext` actuel sont souvent appelées à partir de contrôleurs ou de composants de middleware et peuvent avoir l’identité de l’utilisateur actuel passée comme argument.
* **IHttpContextAccessor**. Le projet en cours de migration vers ASP.NET Core peut être trop volumineux pour transmettre facilement l’identité de l’utilisateur actuel à tous les emplacements nécessaires. Dans ce cas, [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) peut être utilisé comme solution de contournement. `IHttpContextAccessor` est en mesure d’accéder au `HttpContext` actuel (s’il en existe un). Si DI est utilisé, consultez <xref:fundamentals/httpcontext>. Une solution à bref terme pour obtenir l’identité de l’utilisateur actuel dans du code qui n’a pas encore été mis à jour pour fonctionner avec l’architecture de l’injection de ASP.NET Core est :

  * Rendez `IHttpContextAccessor` disponible dans le conteneur DI en appelant [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) dans `Startup.ConfigureServices`.
  * Obtenir une instance de `IHttpContextAccessor` lors du démarrage et la stocker dans une variable statique. L’instance est mise à la disposition du code qui récupérait précédemment l’utilisateur actuel à partir d’une propriété statique.
  * Récupérez l' `ClaimsPrincipal` de l’utilisateur actuel à l’aide de `HttpContextAccessor.HttpContext?.User`. Si ce code est utilisé en dehors du contexte d’une requête HTTP, le `HttpContext` a la valeur null.

L’option finale, à l’aide d’une instance de `IHttpContextAccessor` stockée dans une variable statique, est contraire aux principes de ASP.NET Core (en préférant les dépendances injectées aux dépendances statiques). Envisagez de récupérer des instances de `IHttpContextAccessor` à partir d’une injection de dépendance à la place. Toutefois, une application auxiliaire statique peut être utile lors de la migration d’applications ASP.NET volumineuses existantes qui utilisaient précédemment `ClaimsPrincipal.Current`.
