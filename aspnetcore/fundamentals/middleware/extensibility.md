---
title: Activation d’un intergiciel (middleware) basé sur une fabrique dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser un middleware fortement typé avec une implémentation de l’activation basée sur une fabrique dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/22/2019
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 17018d2dd20ed7b26bd0aa1095fa720a73f77261
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71186944"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a><span data-ttu-id="69681-103">Activation d’un intergiciel (middleware) basé sur une fabrique dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="69681-103">Factory-based middleware activation in ASP.NET Core</span></span>

<span data-ttu-id="69681-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="69681-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="69681-105"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> est un point d’extensibilité pour l’activation d’un [middleware (intergiciel)](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="69681-105"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="69681-106">Les méthodes d’extension de <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> vérifient si le type inscrit d’un middleware implémente <xref:Microsoft.AspNetCore.Http.IMiddleware>.</span><span class="sxs-lookup"><span data-stu-id="69681-106"><xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> extension methods check if a middleware's registered type implements <xref:Microsoft.AspNetCore.Http.IMiddleware>.</span></span> <span data-ttu-id="69681-107">Si c’est le cas, l’instance de <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> inscrite dans le conteneur est utilisée pour résoudre l’implémentation de <xref:Microsoft.AspNetCore.Http.IMiddleware>, au lieu de la logique d’activation de middleware basée sur une convention.</span><span class="sxs-lookup"><span data-stu-id="69681-107">If it does, the <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> instance registered in the container is used to resolve the <xref:Microsoft.AspNetCore.Http.IMiddleware> implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="69681-108">Le middleware est inscrit comme [service délimité ou temporaire](xref:fundamentals/dependency-injection#service-lifetimes) dans le conteneur de service de l’application.</span><span class="sxs-lookup"><span data-stu-id="69681-108">The middleware is registered as a [scoped or transient service](xref:fundamentals/dependency-injection#service-lifetimes) in the app's service container.</span></span>

<span data-ttu-id="69681-109">Avantages :</span><span class="sxs-lookup"><span data-stu-id="69681-109">Benefits:</span></span>

* <span data-ttu-id="69681-110">Activation par requête de client (injection de services délimités)</span><span class="sxs-lookup"><span data-stu-id="69681-110">Activation per client request (injection of scoped services)</span></span>
* <span data-ttu-id="69681-111">Typage fort du middleware</span><span class="sxs-lookup"><span data-stu-id="69681-111">Strong typing of middleware</span></span>

<span data-ttu-id="69681-112"><xref:Microsoft.AspNetCore.Http.IMiddleware> est activé par requête de client (connexion) : des services délimités peuvent ainsi être injectés dans le constructeur du middleware.</span><span class="sxs-lookup"><span data-stu-id="69681-112"><xref:Microsoft.AspNetCore.Http.IMiddleware> is activated per client request (connection), so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="69681-113">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="69681-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="imiddleware"></a><span data-ttu-id="69681-114">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="69681-114">IMiddleware</span></span>

<span data-ttu-id="69681-115"><xref:Microsoft.AspNetCore.Http.IMiddleware> définit le middleware pour le pipeline des requêtes de l’application.</span><span class="sxs-lookup"><span data-stu-id="69681-115"><xref:Microsoft.AspNetCore.Http.IMiddleware> defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="69681-116">La méthode [InvokeAsync(HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) gère les requêtes et retourne un élément <xref:System.Threading.Tasks.Task> qui représente l’exécution du middleware.</span><span class="sxs-lookup"><span data-stu-id="69681-116">The [InvokeAsync(HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) method handles requests and returns a <xref:System.Threading.Tasks.Task> that represents the execution of the middleware.</span></span>

<span data-ttu-id="69681-117">Middleware activé par convention :</span><span class="sxs-lookup"><span data-stu-id="69681-117">Middleware activated by convention:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="69681-118">Middleware activé par <xref:Microsoft.AspNetCore.Http.MiddlewareFactory> :</span><span class="sxs-lookup"><span data-stu-id="69681-118">Middleware activated by <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="69681-119">Des extensions sont créées pour les middleware :</span><span class="sxs-lookup"><span data-stu-id="69681-119">Extensions are created for the middlewares:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="69681-120">Il n’est pas possible de passer des objets au middleware activé par fabrique avec <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> :</span><span class="sxs-lookup"><span data-stu-id="69681-120">It isn't possible to pass objects to the factory-activated middleware with <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>:</span></span>

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

<span data-ttu-id="69681-121">Le middleware activé par fabrique est ajouté au conteneur intégré dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="69681-121">The factory-activated middleware is added to the built-in container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="69681-122">Les deux middlewares sont inscrits dans le pipeline de traitement des requêtes dans `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="69681-122">Both middlewares are registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet2&highlight=12-13)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="69681-123">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="69681-123">IMiddlewareFactory</span></span>

<span data-ttu-id="69681-124"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> fournit des méthodes pour créer un middleware.</span><span class="sxs-lookup"><span data-stu-id="69681-124"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> provides methods to create middleware.</span></span> <span data-ttu-id="69681-125">L’implémentation de la fabrique de middlewares est inscrite dans le conteneur comme service délimité.</span><span class="sxs-lookup"><span data-stu-id="69681-125">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="69681-126">L’implémentation par défaut de <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>, <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, se trouve dans le package [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/).</span><span class="sxs-lookup"><span data-stu-id="69681-126">The default <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> implementation, <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="69681-127"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> est un point d’extensibilité pour l’activation d’un [middleware (intergiciel)](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="69681-127"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="69681-128">Les méthodes d’extension de <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> vérifient si le type inscrit d’un middleware implémente <xref:Microsoft.AspNetCore.Http.IMiddleware>.</span><span class="sxs-lookup"><span data-stu-id="69681-128"><xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> extension methods check if a middleware's registered type implements <xref:Microsoft.AspNetCore.Http.IMiddleware>.</span></span> <span data-ttu-id="69681-129">Si c’est le cas, l’instance de <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> inscrite dans le conteneur est utilisée pour résoudre l’implémentation de <xref:Microsoft.AspNetCore.Http.IMiddleware>, au lieu de la logique d’activation de middleware basée sur une convention.</span><span class="sxs-lookup"><span data-stu-id="69681-129">If it does, the <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> instance registered in the container is used to resolve the <xref:Microsoft.AspNetCore.Http.IMiddleware> implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="69681-130">Le middleware est inscrit comme [service délimité ou temporaire](xref:fundamentals/dependency-injection#service-lifetimes) dans le conteneur de service de l’application.</span><span class="sxs-lookup"><span data-stu-id="69681-130">The middleware is registered as a [scoped or transient service](xref:fundamentals/dependency-injection#service-lifetimes) in the app's service container.</span></span>

<span data-ttu-id="69681-131">Avantages :</span><span class="sxs-lookup"><span data-stu-id="69681-131">Benefits:</span></span>

* <span data-ttu-id="69681-132">Activation par requête de client (injection de services délimités)</span><span class="sxs-lookup"><span data-stu-id="69681-132">Activation per client request (injection of scoped services)</span></span>
* <span data-ttu-id="69681-133">Typage fort du middleware</span><span class="sxs-lookup"><span data-stu-id="69681-133">Strong typing of middleware</span></span>

<span data-ttu-id="69681-134"><xref:Microsoft.AspNetCore.Http.IMiddleware> est activé par requête de client (connexion) : des services délimités peuvent ainsi être injectés dans le constructeur du middleware.</span><span class="sxs-lookup"><span data-stu-id="69681-134"><xref:Microsoft.AspNetCore.Http.IMiddleware> is activated per client request (connection), so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="69681-135">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="69681-135">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="imiddleware"></a><span data-ttu-id="69681-136">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="69681-136">IMiddleware</span></span>

<span data-ttu-id="69681-137"><xref:Microsoft.AspNetCore.Http.IMiddleware> définit le middleware pour le pipeline des requêtes de l’application.</span><span class="sxs-lookup"><span data-stu-id="69681-137"><xref:Microsoft.AspNetCore.Http.IMiddleware> defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="69681-138">La méthode [InvokeAsync(HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) gère les requêtes et retourne un élément <xref:System.Threading.Tasks.Task> qui représente l’exécution du middleware.</span><span class="sxs-lookup"><span data-stu-id="69681-138">The [InvokeAsync(HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) method handles requests and returns a <xref:System.Threading.Tasks.Task> that represents the execution of the middleware.</span></span>

<span data-ttu-id="69681-139">Middleware activé par convention :</span><span class="sxs-lookup"><span data-stu-id="69681-139">Middleware activated by convention:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="69681-140">Middleware activé par <xref:Microsoft.AspNetCore.Http.MiddlewareFactory> :</span><span class="sxs-lookup"><span data-stu-id="69681-140">Middleware activated by <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="69681-141">Des extensions sont créées pour les middleware :</span><span class="sxs-lookup"><span data-stu-id="69681-141">Extensions are created for the middlewares:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="69681-142">Il n’est pas possible de passer des objets au middleware activé par fabrique avec <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> :</span><span class="sxs-lookup"><span data-stu-id="69681-142">It isn't possible to pass objects to the factory-activated middleware with <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>:</span></span>

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

<span data-ttu-id="69681-143">Le middleware activé par fabrique est ajouté au conteneur intégré dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="69681-143">The factory-activated middleware is added to the built-in container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="69681-144">Les deux middlewares sont inscrits dans le pipeline de traitement des requêtes dans `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="69681-144">Both middlewares are registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet2&highlight=13-14)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="69681-145">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="69681-145">IMiddlewareFactory</span></span>

<span data-ttu-id="69681-146"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> fournit des méthodes pour créer un middleware.</span><span class="sxs-lookup"><span data-stu-id="69681-146"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> provides methods to create middleware.</span></span> <span data-ttu-id="69681-147">L’implémentation de la fabrique de middlewares est inscrite dans le conteneur comme service délimité.</span><span class="sxs-lookup"><span data-stu-id="69681-147">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="69681-148">L’implémentation par défaut de <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>, <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, se trouve dans le package [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/).</span><span class="sxs-lookup"><span data-stu-id="69681-148">The default <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> implementation, <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="69681-149">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="69681-149">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* <xref:fundamentals/middleware/extensibility-third-party-container>
