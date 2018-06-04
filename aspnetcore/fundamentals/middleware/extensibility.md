---
title: Activation d’un intergiciel (middleware) basé sur une fabrique dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser un middleware fortement typé avec une implémentation de l’activation basée sur une fabrique dans ASP.NET Core.
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 01/29/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 8cec5b3b498f5a23463d8c3cd5901e14f22f6eab
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729114"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a>Activation d’un intergiciel (middleware) basé sur une fabrique dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) est un point d’extensibilité pour l’activation d’un [middleware](xref:fundamentals/middleware/index).

Les méthodes d’extension de `UseMiddleware` vérifient si le type inscrit d’un middleware implémente `IMiddleware`. Si c’est le cas, l’instance de `IMiddlewareFactory` inscrite dans le conteneur est utilisée pour résoudre l’implémentation de `IMiddleware`, au lieu de la logique d’activation de middleware basée sur une convention. Le middleware est inscrit comme service délimité ou temporaire dans le conteneur de service de l’application.

Avantages :

* Activation par demande (injection de services délimités)
* Typage fort du middleware

`IMiddleware` est activé par demande : des services délimités peuvent ainsi être injectés dans le constructeur du middleware.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

L’exemple d’application montre un middleware activé par :

* Convention. Pour plus d’informations sur l’activation conventionnelle de middleware, consultez la rubrique [Intergiciel (middleware)](xref:fundamentals/middleware/index).
* Une implémentation de [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware). La [classe MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) par défaut active le middleware.

Les implémentations de middleware fonctionnent de façon identique et enregistrent la valeur fournie par un paramètre de la chaîne de requête (`key`). Les middlewares utilisent un contexte de base de données injecté (un service délimité) pour enregistrer la valeur de la chaîne de requête dans une base de données en mémoire.

## <a name="imiddleware"></a>IMiddleware

[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) définit l’intergiciel pour le pipeline des requêtes de l’application. La méthode [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) gère les requêtes et retourne un élément `Task` qui représente l’exécution du middleware.

Middleware activé par convention :

[!code-csharp[](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

Middleware activé par `MiddlewareFactory` :

[!code-csharp[](extensibility/sample/Middleware/IMiddlewareMiddleware.cs?name=snippet1)]

Des extensions sont créées pour les middleware :

[!code-csharp[](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

Il n’est pas possible de passer des objets au middleware activé par fabrique avec `UseMiddleware` :

```csharp
public static IApplicationBuilder UseIMiddlewareMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<IMiddlewareMiddleware>(option);
}
```

Le middleware activé par fabrique est ajouté au conteneur intégré dans *Startup.cs* :

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet1&highlight=12)]

Les deux middlewares sont inscrits dans le pipeline de traitement des requêtes dans `Configure` :

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet2&highlight=13-14)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) fournit des méthodes pour créer un middleware. L’implémentation de la fabrique de middlewares est inscrite dans le conteneur comme service délimité.

L’implémentation par défaut de `IMiddlewareFactory`, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), se trouve dans le package [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) ([source de référence](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)).

## <a name="additional-resources"></a>Ressources supplémentaires

* [Intergiciel (middleware)](xref:fundamentals/middleware/index)
* [Activation d’intergiciel (middleware) avec un conteneur tiers](xref:fundamentals/middleware/extensibility-third-party-container)
