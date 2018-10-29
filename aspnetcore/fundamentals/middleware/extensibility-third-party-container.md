---
title: Activation d’un intergiciel (middleware) avec un conteneur tiers dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser un middleware fortement typé avec une activation basée sur une fabrique et un conteneur tiers dans ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/02/2018
uid: fundamentals/middleware/extensibility-third-party-container
ms.openlocfilehash: 6af775c66a1de7f1a4f06a4a639ade20c6493b2a
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206807"
---
# <a name="middleware-activation-with-a-third-party-container-in-aspnet-core"></a>Activation d’un intergiciel (middleware) avec un conteneur tiers dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

Cet article montre comment utiliser [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) et [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) comme point d’extensibilité pour l’activation d’un[middleware](xref:fundamentals/middleware/index) avec un conteneur tiers. Pour une présentation de `IMiddlewareFactory` et de `IMiddleware`, consultez la rubrique [Activation d’un middleware basée sur une fabrique](xref:fundamentals/middleware/extensibility).

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

L’exemple d’application montre l’activation d’un middleware par une implémentation de `IMiddlewareFactory`, `SimpleInjectorMiddlewareFactory`. L’exemple utilise le conteneur d’injection de dépendances [Simple Injector](https://simpleinjector.org).

L’implémentation du middleware de l’exemple enregistre la valeur fournie par un paramètre de la chaîne de requête (`key`). Le middleware utilise un contexte de base de données injecté (un service délimité) pour enregistrer la valeur de la chaîne de requête dans une base de données en mémoire.

> [!NOTE]
> L’exemple d’application utilise [Simple Injector](https://github.com/simpleinjector/SimpleInjector) seulement à des fins de démonstration. Sa simple utilisation ne constitue pas une publicité pour ce produit. Les approches de l’activation des middlewares décrites dans la documentation de Simple Injector et les problèmes GitHub sont recommandées par les développeurs de Simple Injector. Pour plus d’informations, consultez la [documentation de Simple Injector](https://simpleinjector.readthedocs.io/en/latest/index.html) et le [dépôt GitHub de Simple Injector](https://github.com/simpleinjector/SimpleInjector).

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) fournit des méthodes pour créer un middleware.

Dans l’exemple d’application, une fabrique de middleware est implémentée pour créer une instance de `SimpleInjectorActivatedMiddleware`. La fabrique de middleware utilise le conteneur Simple Injector pour résoudre le middleware :

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a>IMiddleware

[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) définit l’intergiciel pour le pipeline des requêtes de l’application.

Middleware activé par une implémentation de `IMiddlewareFactory` (*Middleware/SimpleInjectorActivatedMiddleware.css*) :

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

Une extension est créée pour le middleware (*Middleware/MiddlewareExtensions.cs*) :

[!code-csharp[](extensibility-third-party-container/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

`Startup.ConfigureServices` doit effectuer plusieurs tâches :

* Configurer le conteneur Simple Injector.
* Inscrire la fabrique et le middleware.
* Rendez le contexte de base de données de l’application disponible depuis le conteneur Simple Injector pour une page Razor.

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet1)]

Le middleware est inscrit dans le pipeline de traitement des requêtes, dans `Startup.Configure` :

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet2&highlight=13)]

## <a name="additional-resources"></a>Ressources supplémentaires

* [Intergiciel (middleware)](xref:fundamentals/middleware/index)
* [Activation d’intergiciel (middleware) basée sur une fabrique](xref:fundamentals/middleware/extensibility)
* [Dépôt GitHub de Simple Injector](https://github.com/simpleinjector/SimpleInjector)
* [Documentation de Simple Injector](https://simpleinjector.readthedocs.io/en/latest/index.html)
