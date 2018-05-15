---
title: Activation d’un intergiciel (middleware) avec un conteneur tiers dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser un middleware fortement typé avec une activation basée sur une fabrique et un conteneur tiers dans ASP.NET Core.
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 02/02/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility-third-party-container
ms.openlocfilehash: b6fd02d1863efe571bafe1344fb34939883e6cb5
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/02/2018
---
# <a name="middleware-activation-with-a-third-party-container-in-aspnet-core"></a><span data-ttu-id="66085-103">Activation d’un intergiciel (middleware) avec un conteneur tiers dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="66085-103">Middleware activation with a third-party container in ASP.NET Core</span></span>

<span data-ttu-id="66085-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="66085-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="66085-105">Cet article montre comment utiliser [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) et [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) comme point d’extensibilité pour l’activation d’un[middleware](xref:fundamentals/middleware/index) avec un conteneur tiers.</span><span class="sxs-lookup"><span data-stu-id="66085-105">This article demonstrates how to use [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) and [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) as an extensibility point for [middleware](xref:fundamentals/middleware/index) activation with a third-party container.</span></span> <span data-ttu-id="66085-106">Pour une présentation de `IMiddlewareFactory` et de `IMiddleware`, consultez la rubrique [Activation d’un middleware basée sur une fabrique](xref:fundamentals/middleware/extensibility).</span><span class="sxs-lookup"><span data-stu-id="66085-106">For introductory information on `IMiddlewareFactory` and `IMiddleware`, see the [Factory-based middleware activation](xref:fundamentals/middleware/extensibility) topic.</span></span>

<span data-ttu-id="66085-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="66085-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="66085-108">L’exemple d’application montre l’activation d’un middleware par une implémentation de `IMiddlewareFactory`, `SimpleInjectorMiddlewareFactory`.</span><span class="sxs-lookup"><span data-stu-id="66085-108">The sample app demonstrates middleware activation by an `IMiddlewareFactory` implementation, `SimpleInjectorMiddlewareFactory`.</span></span> <span data-ttu-id="66085-109">L’exemple utilise le conteneur d’injection de dépendances [Simple Injector](https://simpleinjector.org).</span><span class="sxs-lookup"><span data-stu-id="66085-109">The sample uses the [Simple Injector](https://simpleinjector.org) dependency injection (DI) container.</span></span>

<span data-ttu-id="66085-110">L’implémentation du middleware de l’exemple enregistre la valeur fournie par un paramètre de la chaîne de requête (`key`).</span><span class="sxs-lookup"><span data-stu-id="66085-110">The sample's middleware implementation records the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="66085-111">Le middleware utilise un contexte de base de données injecté (un service délimité) pour enregistrer la valeur de la chaîne de requête dans une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="66085-111">The middleware uses an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

> [!NOTE]
> <span data-ttu-id="66085-112">L’exemple d’application utilise [Simple Injector](https://github.com/simpleinjector/SimpleInjector) seulement à des fins de démonstration.</span><span class="sxs-lookup"><span data-stu-id="66085-112">The sample app uses [Simple Injector](https://github.com/simpleinjector/SimpleInjector) purely for demonstration purposes.</span></span> <span data-ttu-id="66085-113">Sa simple utilisation ne constitue pas une publicité pour ce produit.</span><span class="sxs-lookup"><span data-stu-id="66085-113">Use of Simple Injector isn't an endorsement.</span></span> <span data-ttu-id="66085-114">Les approches de l’activation des middlewares décrites dans la documentation de Simple Injector et les problèmes GitHub sont recommandées par les développeurs de Simple Injector.</span><span class="sxs-lookup"><span data-stu-id="66085-114">Middleware activation approaches described in the Simple Injector documentation and GitHub issues are recommended by the maintainers of Simple Injector.</span></span> <span data-ttu-id="66085-115">Pour plus d’informations, consultez la [documentation de Simple Injector](https://simpleinjector.readthedocs.io/en/latest/index.html) et le [dépôt GitHub de Simple Injector](https://github.com/simpleinjector/SimpleInjector).</span><span class="sxs-lookup"><span data-stu-id="66085-115">For more information, see the [Simple Injector documentation](https://simpleinjector.readthedocs.io/en/latest/index.html) and [Simple Injector GitHub repository](https://github.com/simpleinjector/SimpleInjector).</span></span>

## <a name="imiddlewarefactory"></a><span data-ttu-id="66085-116">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="66085-116">IMiddlewareFactory</span></span>

<span data-ttu-id="66085-117">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) fournit des méthodes pour créer un middleware.</span><span class="sxs-lookup"><span data-stu-id="66085-117">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) provides methods to create middleware.</span></span>

<span data-ttu-id="66085-118">Dans l’exemple d’application, une fabrique de middleware est implémentée pour créer une instance de `SimpleInjectorActivatedMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="66085-118">In the sample app, a middleware factory is implemented to create an `SimpleInjectorActivatedMiddleware` instance.</span></span> <span data-ttu-id="66085-119">La fabrique de middleware utilise le conteneur Simple Injector pour résoudre le middleware :</span><span class="sxs-lookup"><span data-stu-id="66085-119">The middleware factory uses the Simple Injector container to resolve the middleware:</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a><span data-ttu-id="66085-120">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="66085-120">IMiddleware</span></span>

<span data-ttu-id="66085-121">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) définit l’intergiciel pour le pipeline des requêtes de l’application.</span><span class="sxs-lookup"><span data-stu-id="66085-121">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) defines middleware for the app's request pipeline.</span></span>

<span data-ttu-id="66085-122">Middleware activé par une implémentation de `IMiddlewareFactory` (*Middleware/SimpleInjectorActivatedMiddleware.css*) :</span><span class="sxs-lookup"><span data-stu-id="66085-122">Middleware activated by an `IMiddlewareFactory` implementation (*Middleware/SimpleInjectorActivatedMiddleware.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="66085-123">Une extension est créée pour le middleware (*Middleware/MiddlewareExtensions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="66085-123">An extension is created for the middleware (*Middleware/MiddlewareExtensions.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="66085-124">`Startup.ConfigureServices` doit effectuer plusieurs tâches :</span><span class="sxs-lookup"><span data-stu-id="66085-124">`Startup.ConfigureServices` must perform several tasks:</span></span>

* <span data-ttu-id="66085-125">Configurer le conteneur Simple Injector.</span><span class="sxs-lookup"><span data-stu-id="66085-125">Set up the Simple Injector container.</span></span>
* <span data-ttu-id="66085-126">Inscrire la fabrique et le middleware.</span><span class="sxs-lookup"><span data-stu-id="66085-126">Register the factory and middleware.</span></span>
* <span data-ttu-id="66085-127">Rendez le contexte de base de données de l’application disponible depuis le conteneur Simple Injector pour une page Razor.</span><span class="sxs-lookup"><span data-stu-id="66085-127">Make the app's database context available from the Simple Injector container for a Razor Page.</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="66085-128">Le middleware est inscrit dans le pipeline de traitement des requêtes, dans `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="66085-128">The middleware is registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet2&highlight=12)]

## <a name="additional-resources"></a><span data-ttu-id="66085-129">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="66085-129">Additional resources</span></span>

* [<span data-ttu-id="66085-130">Intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="66085-130">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="66085-131">Activation d’intergiciel (middleware) basée sur une fabrique</span><span class="sxs-lookup"><span data-stu-id="66085-131">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="66085-132">Dépôt GitHub de Simple Injector</span><span class="sxs-lookup"><span data-stu-id="66085-132">Simple Injector GitHub repository</span></span>](https://github.com/simpleinjector/SimpleInjector)
* [<span data-ttu-id="66085-133">Documentation de Simple Injector</span><span class="sxs-lookup"><span data-stu-id="66085-133">Simple Injector documentation</span></span>](https://simpleinjector.readthedocs.io/en/latest/index.html)
