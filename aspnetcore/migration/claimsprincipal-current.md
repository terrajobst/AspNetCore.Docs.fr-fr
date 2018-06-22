---
title: Migrer à partir de ClaimsPrincipal.Current
author: mjrousos
description: Découvrez comment transférer ClaimsPrincipal.Current pour extraire l’identité de l’utilisateur authentifié actuel et les revendications dans ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 05/04/2018
uid: migration/claimsprincipal-current
ms.openlocfilehash: 477f9fe8f0249bdfc528e1b401f072851f2f0d23
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277184"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>Migrer à partir de ClaimsPrincipal.Current

Dans les projets ASP.NET, il était courant d’utiliser [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) en cours de récupération authentifié l’identité utilisateur et revendications. Dans ASP.NET Core, cette propriété a la valeur n’est plus. Code qui a été selon doit être mis à jour pour obtenir l’identité de l’utilisateur authentifié actuel par un autre moyen.

## <a name="context-specific-data-instead-of-static-data"></a>Données spécifiques au contexte à la place des données statiques

Lorsque vous utilisez ASP.NET Core, les valeurs des deux `ClaimsPrincipal.Current` et `Thread.CurrentPrincipal` ne sont pas définis. Ces propriétés représentent l’état statique, ce qui évite généralement d’ASP.NET Core. Au lieu de cela, architecture de ASP.NET principale consiste à récupérer les dépendances (comme identité de l’utilisateur actuel) à partir de collections de services spécifique au contexte (à l’aide de son [injection de dépendance](xref:fundamentals/dependency-injection) (DI) modèle). Plus, `Thread.CurrentPrincipal` étant thread statique, il n’est pas persistant de modifications dans certains scénarios asynchrones (et `ClaimsPrincipal.Current` appelle simplement `Thread.CurrentPrincipal` par défaut).

Pour comprendre les sortes de thread de problèmes des membres statiques peuvent entraîner dans des scénarios asynchrones, envisagez l’extrait de code suivant :

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

L’exemple de code précédent définit `Thread.CurrentPrincipal` et vérifie sa valeur avant et après avoir en attente d’un appel asynchrone. `Thread.CurrentPrincipal` est spécifique à la *thread* sur lequel elle est définie, et la méthode est susceptible de reprendre l’exécution sur un thread différent après l’instruction await. Par conséquent, `Thread.CurrentPrincipal` est présent, quand il est vérifié en premier, mais a la valeur null après l’appel à `await Task.Yield()`.

Identité de l’utilisateur actuel à partir de la collection de service de l’application DI est plus testable, trop, étant donné que les identités de test peuvent être ajoutées facilement.

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>Récupération de l’utilisateur actuel dans une application ASP.NET Core

Il existe plusieurs options pour la récupération de l’utilisateur authentifié actuel `ClaimsPrincipal` dans ASP.NET Core à la place de `ClaimsPrincipal.Current`:

* **ControllerBase.User**. Contrôleurs MVC peuvent accéder à l’utilisateur authentifié actuel, avec leurs [utilisateur](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) propriété.
* **HttpContext.User**. Composants d’accéder aux actuel `HttpContext` (intergiciel (middleware), par exemple) peut obtenir l’utilisateur actuel `ClaimsPrincipal` de [HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).
* **Transmis à partir de l’appelant**. Bibliothèques sans accéder à `HttpContext` sont souvent appelés à partir de contrôleurs ou les composants d’intergiciel (middleware) et peut avoir l’identité de l’utilisateur actuel est passée comme argument.
* **IHttpContextAccessor**. Le projet ASP.NET en cours de migration vers ASP.NET Core est peut-être trop grand pour passer facilement d’identité de l’utilisateur actuel à tous les emplacements nécessaires. Dans ce cas, [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) peut être utilisé comme solution de contournement. `IHttpContextAccessor` est en mesure d’accéder en cours `HttpContext` (le cas échéant). Une solution à court terme pour obtenir l’identité de l’utilisateur actuel dans le code qui n’a pas encore été mis à jour pour fonctionner avec l’architecture de piloté par les DI d’ASP.NET Core serait :

  * Rendre `IHttpContextAccessor` disponible dans le conteneur DI en appelant [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) dans `Startup.ConfigureServices`.
  * Obtenir une instance de `IHttpContextAccessor` lors du démarrage et les stocker dans une variable statique. L’instance est rendue disponible au code qui a été précédemment récupération de l’utilisateur actuel à partir d’une propriété statique.
  * Récupérer l’utilisateur actuel `ClaimsPrincipal` à l’aide de `HttpContextAccessor.HttpContext?.User`. Si ce code est utilisé en dehors du contexte d’une requête HTTP, le `HttpContext` est null.

La dernière option, à l’aide de `IHttpContextAccessor`, est contraire aux principes de base de ASP.NET (et préférez dépendances injectées dépendances statiques). Envisagez de supprimer la dépendance vis-à-vis de la méthode statique `IHttpContextAccessor` helper. Il peut être un pont utile, cependant, lors de la migration de grandes applications ASP.NET existantes que vous utilisant auparavant `ClaimsPrincipal.Current`.
