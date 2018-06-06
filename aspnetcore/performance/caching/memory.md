---
title: Mettre en cache en mémoire dans ASP.NET Core
author: rick-anderson
description: Découvrez comment mettre en cache les données en mémoire dans ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 12/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/memory
ms.openlocfilehash: eca6610caf4e0a654c9a31f89a42e2ac82e94d23
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734482"
---
# <a name="cache-in-memory-in-aspnet-core"></a><span data-ttu-id="1331c-103">Mettre en cache en mémoire dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1331c-103">Cache in-memory in ASP.NET Core</span></span>

<span data-ttu-id="1331c-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), et [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="1331c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="1331c-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1331c-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="1331c-106">Principes fondamentaux de la mise en cache</span><span class="sxs-lookup"><span data-stu-id="1331c-106">Caching basics</span></span>

<span data-ttu-id="1331c-107">La mise en cache peut améliorer considérablement les performances et l’évolutivité d’une application en réduisant le travail requis pour générer le contenu.</span><span class="sxs-lookup"><span data-stu-id="1331c-107">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="1331c-108">Elle est idéale pour les données qui ne sont pas souvent modifiées.</span><span class="sxs-lookup"><span data-stu-id="1331c-108">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="1331c-109">Elle effectue une copie des données, qui sont ainsi renvoyées beaucoup plus rapidement qu’à partir de la source d’origine.</span><span class="sxs-lookup"><span data-stu-id="1331c-109">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="1331c-110">L'application doit être écrite et testée de façon à ne jamais dépendre des données en cache.</span><span class="sxs-lookup"><span data-stu-id="1331c-110">You should write and test your app to never depend on cached data.</span></span>

<span data-ttu-id="1331c-111">ASP.NET Core prend en charge plusieurs caches différents.</span><span class="sxs-lookup"><span data-stu-id="1331c-111">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="1331c-112">Le cache de la plus simple est basé sur le [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), qui représente un cache stocké dans la mémoire du serveur web.</span><span class="sxs-lookup"><span data-stu-id="1331c-112">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="1331c-113">Les applications qui s’exécutent sur une batterie de serveurs de plusieurs serveurs devraient vous assurer que les sessions rémanentes lors de l’utilisation du cache en mémoire.</span><span class="sxs-lookup"><span data-stu-id="1331c-113">Apps which run on a server farm of multiple servers should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="1331c-114">Sessions rémanentes Assurez-vous que les requêtes ultérieures à partir d’un client tous les dirigé vers le même serveur.</span><span class="sxs-lookup"><span data-stu-id="1331c-114">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="1331c-115">Par exemple, les utilisation d’applications Web Azure [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) pour router toutes les demandes ultérieures au même serveur.</span><span class="sxs-lookup"><span data-stu-id="1331c-115">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="1331c-116">Les sessions non persistantes dans une batterie de serveurs Web nécessitent un [cache distribué](distributed.md) pour éviter les problèmes de cohérence du cache. </span><span class="sxs-lookup"><span data-stu-id="1331c-116">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="1331c-117">Pour certaines applications, un cache distribué est est mesure de gérer une montée en charge plus élevée qu’un cache en mémoire.</span><span class="sxs-lookup"><span data-stu-id="1331c-117">For some apps, a distributed cache can support higher scale out than an in-memory cache.</span></span> <span data-ttu-id="1331c-118">Il permet de décharger la mémoire cache vers un processus externe.</span><span class="sxs-lookup"><span data-stu-id="1331c-118">Using a distributed cache offloads the cache memory to an external process.</span></span>

<span data-ttu-id="1331c-119">Le `IMemoryCache` cache supprimez les entrées du cache mémoire insuffisante, sauf si le [cache priorité](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) a la valeur `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="1331c-119">The `IMemoryCache` cache will evict cache entries under memory pressure unless the [cache priority](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) is set to `CacheItemPriority.NeverRemove`.</span></span> <span data-ttu-id="1331c-120">Vous pouvez définir le `CacheItemPriority` pour ajuster la priorité avec laquelle le cache supprime les éléments de sollicitation de la mémoire.</span><span class="sxs-lookup"><span data-stu-id="1331c-120">You can set the `CacheItemPriority` to adjust the priority with which the cache evicts items under memory pressure.</span></span>

<span data-ttu-id="1331c-121">Le cache en mémoire permet de stocker n’importe quel objet ; l’interface de cache distribué est limitée à `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="1331c-121">The in-memory cache can store any object; the distributed cache interface is limited to `byte[]`.</span></span>

## <a name="using-imemorycache"></a><span data-ttu-id="1331c-122">À l’aide de IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="1331c-122">Using IMemoryCache</span></span>

<span data-ttu-id="1331c-123">La mise en cache en mémoire est un *service* qui est référencé à partir de votre application à l’aide de l' [Injection de dépendance](../../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="1331c-123">In-memory caching is a *service* that's referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="1331c-124">Appelez `AddMemoryCache` dans `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1331c-124">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

<span data-ttu-id="1331c-125">Faire appel à l'instance `IMemoryCache` dans le constructeur :</span><span class="sxs-lookup"><span data-stu-id="1331c-125">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1331c-126">`IMemoryCache` nécessite le package NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span><span class="sxs-lookup"><span data-stu-id="1331c-126">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1331c-127">`IMemoryCache` nécessite le package NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), qui est disponible dans le [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="1331c-127">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is avaiable in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="1331c-128">`IMemoryCache` nécessite le package NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), qui est disponible dans le [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1331c-128">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is avaiable in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="1331c-129">Le code suivant utilise [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) pour vérifier si une heure est dans le cache.</span><span class="sxs-lookup"><span data-stu-id="1331c-129">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="1331c-130">Si une heure n’est pas mis en cache, une nouvelle entrée est créée et ajoutée au cache avec [définir](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span><span class="sxs-lookup"><span data-stu-id="1331c-130">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="1331c-131">L’heure actuelle et l’heure de mise en cache s’affichent :</span><span class="sxs-lookup"><span data-stu-id="1331c-131">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="1331c-132">La valeur `DateTime` mise en cache reste dans le cache tant qu'il existe des demandes dans le délai d’expiration (et aucune suppression en raison d’une sollicitation de la mémoire).</span><span class="sxs-lookup"><span data-stu-id="1331c-132">The cached `DateTime` value remains in the cache while there are requests within the timeout period (and no eviction due to memory pressure).</span></span> <span data-ttu-id="1331c-133">L’illustration ci-dessous indique l’heure actuelle et une heure antérieure récupérées du cache :</span><span class="sxs-lookup"><span data-stu-id="1331c-133">The following image shows the current time and an older time retrieved from the cache:</span></span>

![Vue d’index avec deux fois différentes affichées](memory/_static/time.png)

<span data-ttu-id="1331c-135">Le code suivant utilise [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) et [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) en cache des données.</span><span class="sxs-lookup"><span data-stu-id="1331c-135">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span> 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="1331c-136">Le code suivant appelle [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) pour extraire l’heure de mise en cache :</span><span class="sxs-lookup"><span data-stu-id="1331c-136">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="1331c-137">Consultez [les méthodes de IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) et [les méthodes de CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) pour obtenir une description des méthodes du cache.</span><span class="sxs-lookup"><span data-stu-id="1331c-137">See [IMemoryCache methods](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) for a description of the cache methods.</span></span>

## <a name="using-memorycacheentryoptions"></a><span data-ttu-id="1331c-138">Utilisation de MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="1331c-138">Using MemoryCacheEntryOptions</span></span>

<span data-ttu-id="1331c-139">L’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="1331c-139">The following sample:</span></span>

- <span data-ttu-id="1331c-140">Définit l’heure d’expiration absolue.</span><span class="sxs-lookup"><span data-stu-id="1331c-140">Sets the absolute expiration time.</span></span> <span data-ttu-id="1331c-141">Ceci est la durée maximale pendant laquelle l’entrée peut être mise en cache et empêche l’élément de devenir trop périmé quand l’expiration glissante est constamment renouvelée.</span><span class="sxs-lookup"><span data-stu-id="1331c-141">This is the maximum time the entry can be cached and prevents the item from becoming too stale when the sliding expiration is continuously renewed.</span></span>
- <span data-ttu-id="1331c-142">Définit un délai d’expiration glissant.</span><span class="sxs-lookup"><span data-stu-id="1331c-142">Sets a sliding expiration time.</span></span> <span data-ttu-id="1331c-143">Les requêtes qui accèdent à cet élément de mise en cache réinitialisent l’horloge d’expiration glissante.</span><span class="sxs-lookup"><span data-stu-id="1331c-143">Requests that access this cached item will reset the sliding expiration clock.</span></span>
- <span data-ttu-id="1331c-144">Définit la priorité de cache `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="1331c-144">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span>
- <span data-ttu-id="1331c-145">Définit un [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) qui est appelé après la suppression de l’entrée du cache.</span><span class="sxs-lookup"><span data-stu-id="1331c-145">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="1331c-146">Le rappel est exécuté sur un thread différent du code qui supprime l’élément à partir du cache.</span><span class="sxs-lookup"><span data-stu-id="1331c-146">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="cache-dependencies"></a><span data-ttu-id="1331c-147">Dépendances de cache</span><span class="sxs-lookup"><span data-stu-id="1331c-147">Cache dependencies</span></span>

<span data-ttu-id="1331c-148">L’exemple suivant montre comment expirer une entrée de cache si une entrée dépendante expire.</span><span class="sxs-lookup"><span data-stu-id="1331c-148">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="1331c-149">Un `CancellationChangeToken` est ajouté à l’élément mis en cache.</span><span class="sxs-lookup"><span data-stu-id="1331c-149">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="1331c-150">Lorsque `Cancel` est appelée sur le `CancellationTokenSource`, les deux entrées du cache sont supprimées.</span><span class="sxs-lookup"><span data-stu-id="1331c-150">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="1331c-151">Utiliser un `CancellationTokenSource` permet à plusieurs entrées de cache d'être supprimées en tant que groupe.</span><span class="sxs-lookup"><span data-stu-id="1331c-151">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="1331c-152">Avec le modèle `using` dans le code ci-dessus, les entrées de cache créées à l’intérieur du bloc `using` hériteront des déclencheurs et des paramètres d’expiration.</span><span class="sxs-lookup"><span data-stu-id="1331c-152">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="1331c-153">Remarques supplémentaires</span><span class="sxs-lookup"><span data-stu-id="1331c-153">Additional notes</span></span>

- <span data-ttu-id="1331c-154">Lorsque vous utilisez un rappel pour remplir un élément de cache :</span><span class="sxs-lookup"><span data-stu-id="1331c-154">When using a callback to repopulate a cache item:</span></span>

  - <span data-ttu-id="1331c-155">Plusieurs demandes peuvent trouver la valeur de clé mise en cache vide étant donné que le rappel n’est pas terminé.</span><span class="sxs-lookup"><span data-stu-id="1331c-155">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span>
  - <span data-ttu-id="1331c-156">Il peut en résulter que plusieurs threads remplissent l’élément mis en cache.</span><span class="sxs-lookup"><span data-stu-id="1331c-156">This can result in several threads repopulating the cached item.</span></span>

- <span data-ttu-id="1331c-157">Lorsqu’une entrée de cache est utilisée pour en créer une autre, l’enfant copie les jetons d’expiration et les paramètres d’expiration basés sur le temps de l’entrée parente.</span><span class="sxs-lookup"><span data-stu-id="1331c-157">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="1331c-158">L’enfant n’expire par suite à la suppression manuelle ou à la mise à jour de l’entrée parente.</span><span class="sxs-lookup"><span data-stu-id="1331c-158">The child isn't expired by manual removal or updating of the parent entry.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1331c-159">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="1331c-159">Additional resources</span></span>

* [<span data-ttu-id="1331c-160">Utiliser un cache distribué</span><span class="sxs-lookup"><span data-stu-id="1331c-160">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="1331c-161">Détecter les modifications à l’aide de jetons de modification</span><span class="sxs-lookup"><span data-stu-id="1331c-161">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="1331c-162">Mise en cache des réponses</span><span class="sxs-lookup"><span data-stu-id="1331c-162">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="1331c-163">Intergiciel de mise en cache des réponses</span><span class="sxs-lookup"><span data-stu-id="1331c-163">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="1331c-164">Tag Helper de cache</span><span class="sxs-lookup"><span data-stu-id="1331c-164">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="1331c-165">Tag Helper de cache distribué</span><span class="sxs-lookup"><span data-stu-id="1331c-165">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
