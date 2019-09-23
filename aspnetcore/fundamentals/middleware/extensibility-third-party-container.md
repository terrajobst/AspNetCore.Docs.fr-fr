---
title: Activation d’un intergiciel (middleware) avec un conteneur tiers dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser un middleware fortement typé avec une activation basée sur une fabrique et un conteneur tiers dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/22/2019
uid: fundamentals/middleware/extensibility-third-party-container
ms.openlocfilehash: e54a2bd366457fa2d898b7ee26e95021aec5389b
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187090"
---
# <a name="middleware-activation-with-a-third-party-container-in-aspnet-core"></a><span data-ttu-id="35cda-103">Activation d’un intergiciel (middleware) avec un conteneur tiers dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="35cda-103">Middleware activation with a third-party container in ASP.NET Core</span></span>

<span data-ttu-id="35cda-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="35cda-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="35cda-105">Cet article montre comment utiliser <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> et <xref:Microsoft.AspNetCore.Http.IMiddleware> comme point d’extensibilité pour l’activation d’un [middleware](xref:fundamentals/middleware/index) avec un conteneur tiers.</span><span class="sxs-lookup"><span data-stu-id="35cda-105">This article demonstrates how to use <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> and <xref:Microsoft.AspNetCore.Http.IMiddleware> as an extensibility point for [middleware](xref:fundamentals/middleware/index) activation with a third-party container.</span></span> <span data-ttu-id="35cda-106">Pour obtenir des informations sur `IMiddlewareFactory` et `IMiddleware`, consultez <xref:fundamentals/middleware/extensibility>.</span><span class="sxs-lookup"><span data-stu-id="35cda-106">For introductory information on `IMiddlewareFactory` and `IMiddleware`, see <xref:fundamentals/middleware/extensibility>.</span></span>

<span data-ttu-id="35cda-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="35cda-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="35cda-108">L’exemple d’application montre l’activation d’un middleware par une implémentation de `IMiddlewareFactory`, `SimpleInjectorMiddlewareFactory`.</span><span class="sxs-lookup"><span data-stu-id="35cda-108">The sample app demonstrates middleware activation by an `IMiddlewareFactory` implementation, `SimpleInjectorMiddlewareFactory`.</span></span> <span data-ttu-id="35cda-109">L’exemple utilise le conteneur d’injection de dépendances [Simple Injector](https://simpleinjector.org).</span><span class="sxs-lookup"><span data-stu-id="35cda-109">The sample uses the [Simple Injector](https://simpleinjector.org) dependency injection (DI) container.</span></span>

<span data-ttu-id="35cda-110">L’implémentation du middleware de l’exemple enregistre la valeur fournie par un paramètre de la chaîne de requête (`key`).</span><span class="sxs-lookup"><span data-stu-id="35cda-110">The sample's middleware implementation records the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="35cda-111">Le middleware utilise un contexte de base de données injecté (un service délimité) pour enregistrer la valeur de la chaîne de requête dans une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="35cda-111">The middleware uses an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

> [!NOTE]
> <span data-ttu-id="35cda-112">L’exemple d’application utilise [Simple Injector](https://github.com/simpleinjector/SimpleInjector) seulement à des fins de démonstration.</span><span class="sxs-lookup"><span data-stu-id="35cda-112">The sample app uses [Simple Injector](https://github.com/simpleinjector/SimpleInjector) purely for demonstration purposes.</span></span> <span data-ttu-id="35cda-113">Sa simple utilisation ne constitue pas une publicité pour ce produit.</span><span class="sxs-lookup"><span data-stu-id="35cda-113">Use of Simple Injector isn't an endorsement.</span></span> <span data-ttu-id="35cda-114">Les approches de l’activation des middlewares décrites dans la documentation de Simple Injector et les problèmes GitHub sont recommandées par les développeurs de Simple Injector.</span><span class="sxs-lookup"><span data-stu-id="35cda-114">Middleware activation approaches described in the Simple Injector documentation and GitHub issues are recommended by the maintainers of Simple Injector.</span></span> <span data-ttu-id="35cda-115">Pour plus d’informations, consultez la [documentation de Simple Injector](https://simpleinjector.readthedocs.io/en/latest/index.html) et le [dépôt GitHub de Simple Injector](https://github.com/simpleinjector/SimpleInjector).</span><span class="sxs-lookup"><span data-stu-id="35cda-115">For more information, see the [Simple Injector documentation](https://simpleinjector.readthedocs.io/en/latest/index.html) and [Simple Injector GitHub repository](https://github.com/simpleinjector/SimpleInjector).</span></span>

## <a name="imiddlewarefactory"></a><span data-ttu-id="35cda-116">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="35cda-116">IMiddlewareFactory</span></span>

<span data-ttu-id="35cda-117"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> fournit des méthodes pour créer un middleware.</span><span class="sxs-lookup"><span data-stu-id="35cda-117"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> provides methods to create middleware.</span></span>

<span data-ttu-id="35cda-118">Dans l’exemple d’application, une fabrique de middleware est implémentée pour créer une instance de `SimpleInjectorActivatedMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="35cda-118">In the sample app, a middleware factory is implemented to create an `SimpleInjectorActivatedMiddleware` instance.</span></span> <span data-ttu-id="35cda-119">La fabrique de middleware utilise le conteneur Simple Injector pour résoudre le middleware :</span><span class="sxs-lookup"><span data-stu-id="35cda-119">The middleware factory uses the Simple Injector container to resolve the middleware:</span></span>

[!code-csharp[](extensibility-third-party-container/samples/3.x/SampleApp/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a><span data-ttu-id="35cda-120">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="35cda-120">IMiddleware</span></span>

<span data-ttu-id="35cda-121"><xref:Microsoft.AspNetCore.Http.IMiddleware> définit le middleware pour le pipeline des requêtes de l’application.</span><span class="sxs-lookup"><span data-stu-id="35cda-121"><xref:Microsoft.AspNetCore.Http.IMiddleware> defines middleware for the app's request pipeline.</span></span>

<span data-ttu-id="35cda-122">Middleware activé par une implémentation de `IMiddlewareFactory` (*Middleware/SimpleInjectorActivatedMiddleware.css*) :</span><span class="sxs-lookup"><span data-stu-id="35cda-122">Middleware activated by an `IMiddlewareFactory` implementation (*Middleware/SimpleInjectorActivatedMiddleware.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/samples/3.x/SampleApp/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="35cda-123">Une extension est créée pour le middleware (*Middleware/MiddlewareExtensions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="35cda-123">An extension is created for the middleware (*Middleware/MiddlewareExtensions.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/samples/3.x/SampleApp/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="35cda-124">`Startup.ConfigureServices` doit effectuer plusieurs tâches :</span><span class="sxs-lookup"><span data-stu-id="35cda-124">`Startup.ConfigureServices` must perform several tasks:</span></span>

* <span data-ttu-id="35cda-125">Configurer le conteneur Simple Injector.</span><span class="sxs-lookup"><span data-stu-id="35cda-125">Set up the Simple Injector container.</span></span>
* <span data-ttu-id="35cda-126">Inscrire la fabrique et le middleware.</span><span class="sxs-lookup"><span data-stu-id="35cda-126">Register the factory and middleware.</span></span>
* <span data-ttu-id="35cda-127">Rendez le contexte de base de données de l’application disponible depuis le conteneur Simple Injector.</span><span class="sxs-lookup"><span data-stu-id="35cda-127">Make the app's database context available from the Simple Injector container.</span></span>

[!code-csharp[](extensibility-third-party-container/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="35cda-128">Le middleware est inscrit dans le pipeline de traitement des requêtes, dans `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="35cda-128">The middleware is registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility-third-party-container/samples/3.x/SampleApp/Startup.cs?name=snippet2&highlight=12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="35cda-129">Cet article montre comment utiliser <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> et <xref:Microsoft.AspNetCore.Http.IMiddleware> comme point d’extensibilité pour l’activation d’un [middleware](xref:fundamentals/middleware/index) avec un conteneur tiers.</span><span class="sxs-lookup"><span data-stu-id="35cda-129">This article demonstrates how to use <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> and <xref:Microsoft.AspNetCore.Http.IMiddleware> as an extensibility point for [middleware](xref:fundamentals/middleware/index) activation with a third-party container.</span></span> <span data-ttu-id="35cda-130">Pour obtenir des informations sur `IMiddlewareFactory` et `IMiddleware`, consultez <xref:fundamentals/middleware/extensibility>.</span><span class="sxs-lookup"><span data-stu-id="35cda-130">For introductory information on `IMiddlewareFactory` and `IMiddleware`, see <xref:fundamentals/middleware/extensibility>.</span></span>

<span data-ttu-id="35cda-131">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="35cda-131">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="35cda-132">L’exemple d’application montre l’activation d’un middleware par une implémentation de `IMiddlewareFactory`, `SimpleInjectorMiddlewareFactory`.</span><span class="sxs-lookup"><span data-stu-id="35cda-132">The sample app demonstrates middleware activation by an `IMiddlewareFactory` implementation, `SimpleInjectorMiddlewareFactory`.</span></span> <span data-ttu-id="35cda-133">L’exemple utilise le conteneur d’injection de dépendances [Simple Injector](https://simpleinjector.org).</span><span class="sxs-lookup"><span data-stu-id="35cda-133">The sample uses the [Simple Injector](https://simpleinjector.org) dependency injection (DI) container.</span></span>

<span data-ttu-id="35cda-134">L’implémentation du middleware de l’exemple enregistre la valeur fournie par un paramètre de la chaîne de requête (`key`).</span><span class="sxs-lookup"><span data-stu-id="35cda-134">The sample's middleware implementation records the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="35cda-135">Le middleware utilise un contexte de base de données injecté (un service délimité) pour enregistrer la valeur de la chaîne de requête dans une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="35cda-135">The middleware uses an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

> [!NOTE]
> <span data-ttu-id="35cda-136">L’exemple d’application utilise [Simple Injector](https://github.com/simpleinjector/SimpleInjector) seulement à des fins de démonstration.</span><span class="sxs-lookup"><span data-stu-id="35cda-136">The sample app uses [Simple Injector](https://github.com/simpleinjector/SimpleInjector) purely for demonstration purposes.</span></span> <span data-ttu-id="35cda-137">Sa simple utilisation ne constitue pas une publicité pour ce produit.</span><span class="sxs-lookup"><span data-stu-id="35cda-137">Use of Simple Injector isn't an endorsement.</span></span> <span data-ttu-id="35cda-138">Les approches de l’activation des middlewares décrites dans la documentation de Simple Injector et les problèmes GitHub sont recommandées par les développeurs de Simple Injector.</span><span class="sxs-lookup"><span data-stu-id="35cda-138">Middleware activation approaches described in the Simple Injector documentation and GitHub issues are recommended by the maintainers of Simple Injector.</span></span> <span data-ttu-id="35cda-139">Pour plus d’informations, consultez la [documentation de Simple Injector](https://simpleinjector.readthedocs.io/en/latest/index.html) et le [dépôt GitHub de Simple Injector](https://github.com/simpleinjector/SimpleInjector).</span><span class="sxs-lookup"><span data-stu-id="35cda-139">For more information, see the [Simple Injector documentation](https://simpleinjector.readthedocs.io/en/latest/index.html) and [Simple Injector GitHub repository](https://github.com/simpleinjector/SimpleInjector).</span></span>

## <a name="imiddlewarefactory"></a><span data-ttu-id="35cda-140">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="35cda-140">IMiddlewareFactory</span></span>

<span data-ttu-id="35cda-141"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> fournit des méthodes pour créer un middleware.</span><span class="sxs-lookup"><span data-stu-id="35cda-141"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> provides methods to create middleware.</span></span>

<span data-ttu-id="35cda-142">Dans l’exemple d’application, une fabrique de middleware est implémentée pour créer une instance de `SimpleInjectorActivatedMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="35cda-142">In the sample app, a middleware factory is implemented to create an `SimpleInjectorActivatedMiddleware` instance.</span></span> <span data-ttu-id="35cda-143">La fabrique de middleware utilise le conteneur Simple Injector pour résoudre le middleware :</span><span class="sxs-lookup"><span data-stu-id="35cda-143">The middleware factory uses the Simple Injector container to resolve the middleware:</span></span>

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a><span data-ttu-id="35cda-144">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="35cda-144">IMiddleware</span></span>

<span data-ttu-id="35cda-145"><xref:Microsoft.AspNetCore.Http.IMiddleware> définit le middleware pour le pipeline des requêtes de l’application.</span><span class="sxs-lookup"><span data-stu-id="35cda-145"><xref:Microsoft.AspNetCore.Http.IMiddleware> defines middleware for the app's request pipeline.</span></span>

<span data-ttu-id="35cda-146">Middleware activé par une implémentation de `IMiddlewareFactory` (*Middleware/SimpleInjectorActivatedMiddleware.css*) :</span><span class="sxs-lookup"><span data-stu-id="35cda-146">Middleware activated by an `IMiddlewareFactory` implementation (*Middleware/SimpleInjectorActivatedMiddleware.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="35cda-147">Une extension est créée pour le middleware (*Middleware/MiddlewareExtensions.cs*) :</span><span class="sxs-lookup"><span data-stu-id="35cda-147">An extension is created for the middleware (*Middleware/MiddlewareExtensions.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="35cda-148">`Startup.ConfigureServices` doit effectuer plusieurs tâches :</span><span class="sxs-lookup"><span data-stu-id="35cda-148">`Startup.ConfigureServices` must perform several tasks:</span></span>

* <span data-ttu-id="35cda-149">Configurer le conteneur Simple Injector.</span><span class="sxs-lookup"><span data-stu-id="35cda-149">Set up the Simple Injector container.</span></span>
* <span data-ttu-id="35cda-150">Inscrire la fabrique et le middleware.</span><span class="sxs-lookup"><span data-stu-id="35cda-150">Register the factory and middleware.</span></span>
* <span data-ttu-id="35cda-151">Rendez le contexte de base de données de l’application disponible depuis le conteneur Simple Injector.</span><span class="sxs-lookup"><span data-stu-id="35cda-151">Make the app's database context available from the Simple Injector container.</span></span>

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="35cda-152">Le middleware est inscrit dans le pipeline de traitement des requêtes, dans `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="35cda-152">The middleware is registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Startup.cs?name=snippet2&highlight=12)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="35cda-153">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="35cda-153">Additional resources</span></span>

* [<span data-ttu-id="35cda-154">Intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="35cda-154">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="35cda-155">Activation d’intergiciel (middleware) basée sur une fabrique</span><span class="sxs-lookup"><span data-stu-id="35cda-155">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="35cda-156">Dépôt GitHub de Simple Injector</span><span class="sxs-lookup"><span data-stu-id="35cda-156">Simple Injector GitHub repository</span></span>](https://github.com/simpleinjector/SimpleInjector)
* [<span data-ttu-id="35cda-157">Documentation de Simple Injector</span><span class="sxs-lookup"><span data-stu-id="35cda-157">Simple Injector documentation</span></span>](https://simpleinjector.readthedocs.io/en/latest/index.html)
