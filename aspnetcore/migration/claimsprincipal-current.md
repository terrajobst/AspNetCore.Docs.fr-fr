---
title: Migrer à partir de ClaimsPrincipal.Current
author: mjrousos
description: Apprenez à migrer à partir de ClaimsPrincipal.Current pour récupérer les identités et les revendications dans ASP.NET Core de l’utilisateur authentifié actuel.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/26/2019
uid: migration/claimsprincipal-current
ms.openlocfilehash: 526cc3cf3a58a656e2a1b162cfaccacc7694dc51
ms.sourcegitcommit: 687ffb15ebe65379f75c84739ea851d5a0d788b7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488640"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>Migrer à partir de ClaimsPrincipal.Current

Dans les projets ASP.NET 4.x, il était courant d’utiliser [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) pour récupérer le cours authentifié l’identité utilisateur et revendications. Dans ASP.NET Core, cette propriété est n’est plus définie. Code qui a été selon doit être mis à jour pour obtenir l’identité de l’utilisateur authentifié actuel via un autre moyen.

## <a name="context-specific-data-instead-of-static-data"></a>Données spécifiques à un contexte au lieu des données statiques

Lorsque vous utilisez ASP.NET Core, les valeurs des deux `ClaimsPrincipal.Current` et `Thread.CurrentPrincipal` ne sont pas définies. Ces propriétés représentent l’état statique, ce qui évite généralement d’ASP.NET Core. Au lieu de cela, architecture de d’ASP.NET Core consiste à récupérer des dépendances (telles que l’identité de l’utilisateur actuel) à partir de collections de services spécifique au contexte (à l’aide de son [l’injection de dépendances](xref:fundamentals/dependency-injection) (DI) modèle). Quel est le plus, `Thread.CurrentPrincipal` étant thread statique, il n’est pas persistant de modifications dans certains scénarios asynchrones (et `ClaimsPrincipal.Current` appelle simplement `Thread.CurrentPrincipal` par défaut).

Pour comprendre les sortes de thread de problèmes des membres statiques peuvent conduire à des scénarios asynchrones, envisagez l’extrait de code suivant :

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

L’exemple de code précédent définit `Thread.CurrentPrincipal` et vérifie sa valeur avant et après en attente d’un appel asynchrone. `Thread.CurrentPrincipal` est spécifique à la *thread* sur lequel elle est définie, et la méthode est susceptible de reprendre l’exécution sur un thread différent après l’instruction await. Par conséquent, `Thread.CurrentPrincipal` est présent quand elle est tout d’abord vérifié mais qu’a la valeur null après l’appel à `await Task.Yield()`.

Identité de l’utilisateur actuel à partir de la collection de service de l’injection de dépendances de l’application est plus faciles à tester, trop, étant donné que les identités de test peuvent être facilement injectées.

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>Récupérer l’utilisateur actuel dans une application ASP.NET Core

Il existe plusieurs options pour la récupération de l’utilisateur authentifié actuel `ClaimsPrincipal` dans ASP.NET Core à la place de `ClaimsPrincipal.Current`:

* **ControllerBase.User**. Contrôleurs MVC peuvent accéder à l’utilisateur authentifié actuel avec leurs [utilisateur](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) propriété.
* **HttpContext.User**. Composants avec accès aux cours `HttpContext` (intergiciel (middleware), par exemple) peut obtenir l’utilisateur actuel `ClaimsPrincipal` de [HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).
* **Passé à partir de l’appelant**. Bibliothèques sans accès aux cours `HttpContext` sont souvent appelés à partir des contrôleurs ou des composants d’intergiciel (middleware) et peut avoir l’identité de l’utilisateur actuel passée en tant qu’argument.
* **IHttpContextAccessor**. Le projet en cours de migration vers ASP.NET Core peut-être être trop importants pour passer facilement d’identité de l’utilisateur actuel à tous les emplacements nécessaires. Dans ce cas, [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) peut être utilisé comme solution de contournement. `IHttpContextAccessor` est en mesure d’accéder en cours `HttpContext` (le cas échéant). Une solution à court terme pour obtenir l’identité de l’utilisateur actuel dans le code qui n’a pas encore été mis à jour pour fonctionner avec une architecture pilotée par l’injection de dépendances d’ASP.NET Core serait :

  * Rendre `IHttpContextAccessor` disponible dans le conteneur d’injection de dépendance en appelant [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) dans `Startup.ConfigureServices`.
  * Obtenir une instance de `IHttpContextAccessor` lors du démarrage et le stocker dans une variable statique. L’instance est rendue disponible au code qui récupérait précédemment l’utilisateur actuel à partir d’une propriété statique.
  * Récupérer l’utilisateur actuel `ClaimsPrincipal` à l’aide de `HttpContextAccessor.HttpContext?.User`. Si ce code est utilisé en dehors du contexte d’une requête HTTP, le `HttpContext` a la valeur null.

La dernière option, à l’aide un `IHttpContextAccessor` instance stockées dans une variable statique, est contraire aux principes de ASP.NET Core (préférant injectés dépendances aux dépendances statiques). Envisagez éventuellement récupérer `IHttpContextAccessor` instances à partir de l’injection de dépendances à la place. Une application d’assistance statique peut être un pont utile, cependant, lors de la migration de grandes applications ASP.NET existantes qui ont été précédemment à l’aide `ClaimsPrincipal.Current`.
