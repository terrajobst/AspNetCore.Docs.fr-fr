---
title: Mise en cache distribuée dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser un cache ASP.NET Core distribué pour améliorer les performances et l’évolutivité des applications, en particulier dans un environnement de batterie de serveurs ou de Cloud.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
uid: performance/caching/distributed
ms.openlocfilehash: 3e039a26505aed1bcc0299880039760fef19fd67
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727239"
---
# <a name="distributed-caching-in-aspnet-core"></a><span data-ttu-id="7d363-103">Mise en cache distribuée dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d363-103">Distributed caching in ASP.NET Core</span></span>

<span data-ttu-id="7d363-104">Par [Luke Latham](https://github.com/guardrex), [mohsin Nasir](https://github.com/mohsinnasir)et [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="7d363-104">By [Luke Latham](https://github.com/guardrex), [Mohsin Nasir](https://github.com/mohsinnasir), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="7d363-105">Un cache distribué est un cache partagé par plusieurs serveurs d’applications, généralement géré comme un service externe pour les serveurs d’applications qui y accèdent.</span><span class="sxs-lookup"><span data-stu-id="7d363-105">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="7d363-106">Un cache distribué peut améliorer les performances et l’extensibilité d’une application ASP.NET Core, en particulier lorsque l’application est hébergée par un service Cloud ou une batterie de serveurs.</span><span class="sxs-lookup"><span data-stu-id="7d363-106">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="7d363-107">Un cache distribué présente plusieurs avantages par rapport à d’autres scénarios de mise en cache où les données mises en cache sont stockées sur des serveurs d’applications individuels.</span><span class="sxs-lookup"><span data-stu-id="7d363-107">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="7d363-108">Lorsque les données mises en cache sont distribuées, les données :</span><span class="sxs-lookup"><span data-stu-id="7d363-108">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="7d363-109">Est *cohérente* (cohérente) entre les demandes adressées à plusieurs serveurs.</span><span class="sxs-lookup"><span data-stu-id="7d363-109">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="7d363-110">Survit aux redémarrages du serveur et aux déploiements d’applications.</span><span class="sxs-lookup"><span data-stu-id="7d363-110">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="7d363-111">N’utilise pas la mémoire locale.</span><span class="sxs-lookup"><span data-stu-id="7d363-111">Doesn't use local memory.</span></span>

<span data-ttu-id="7d363-112">La configuration du cache distribué est spécifique à l’implémentation.</span><span class="sxs-lookup"><span data-stu-id="7d363-112">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="7d363-113">Cet article explique comment configurer des caches distribués SQL Server et Redims.</span><span class="sxs-lookup"><span data-stu-id="7d363-113">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="7d363-114">Des implémentations tierces sont également disponibles, telles que [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache sur GitHub](https://github.com/Alachisoft/NCache)).</span><span class="sxs-lookup"><span data-stu-id="7d363-114">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="7d363-115">Quelle que soit l’implémentation choisie, l’application interagit avec le cache à l’aide de l’interface <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>.</span><span class="sxs-lookup"><span data-stu-id="7d363-115">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="7d363-116">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7d363-116">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7d363-117">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="7d363-117">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7d363-118">Pour utiliser un SQL Server cache distribué, ajoutez une référence de package au package [Microsoft. extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="7d363-118">To use a SQL Server distributed cache, add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="7d363-119">Pour utiliser un cache redistribuable ReDim, ajoutez une référence de package au package [Microsoft. extensions. Caching. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) .</span><span class="sxs-lookup"><span data-stu-id="7d363-119">To use a Redis distributed cache, add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span>

<span data-ttu-id="7d363-120">Pour utiliser le cache distribué NCache, ajoutez une référence de package au package [NCache. Microsoft. extensions. Caching. Open source](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) .</span><span class="sxs-lookup"><span data-stu-id="7d363-120">To use NCache distributed cache, add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="7d363-121">Pour utiliser un SQL Server cache distribué, référencez le [AspNetCore. app. app](xref:fundamentals/metapackage-app) ou ajoutez une référence de package au package [Microsoft. extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="7d363-121">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="7d363-122">Pour utiliser un cache distribué Redims, référencez le package [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) et ajoutez une référence de package au package [Microsoft. extensions. Caching. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) .</span><span class="sxs-lookup"><span data-stu-id="7d363-122">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span> <span data-ttu-id="7d363-123">Le package Redims n’est pas inclus dans le package `Microsoft.AspNetCore.App`. vous devez donc référencer le package Redims séparément dans votre fichier projet.</span><span class="sxs-lookup"><span data-stu-id="7d363-123">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

<span data-ttu-id="7d363-124">Pour utiliser le cache distribué NCache, référencez le logiciel [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) et ajoutez une référence de package au package [NCache. Microsoft. extensions. Caching. Open source](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) .</span><span class="sxs-lookup"><span data-stu-id="7d363-124">To use NCache distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span> <span data-ttu-id="7d363-125">Le package NCache n’est pas inclus dans le package `Microsoft.AspNetCore.App`. vous devez donc référencer le package NCache séparément dans votre fichier projet.</span><span class="sxs-lookup"><span data-stu-id="7d363-125">The NCache package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the NCache package separately in your project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7d363-126">Pour utiliser un SQL Server cache distribué, référencez le [AspNetCore. app. app](xref:fundamentals/metapackage-app) ou ajoutez une référence de package au package [Microsoft. extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="7d363-126">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="7d363-127">Pour utiliser un cache redistribuable ReDim, référencez le package [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) et ajoutez une référence de package au package [Microsoft. extensions. Caching. redims](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) .</span><span class="sxs-lookup"><span data-stu-id="7d363-127">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span> <span data-ttu-id="7d363-128">Le package Redims n’est pas inclus dans le package `Microsoft.AspNetCore.App`. vous devez donc référencer le package Redims séparément dans votre fichier projet.</span><span class="sxs-lookup"><span data-stu-id="7d363-128">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

<span data-ttu-id="7d363-129">Pour utiliser le cache distribué NCache, référencez le logiciel [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) et ajoutez une référence de package au package [NCache. Microsoft. extensions. Caching. Open source](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) .</span><span class="sxs-lookup"><span data-stu-id="7d363-129">To use NCache distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span> <span data-ttu-id="7d363-130">Le package NCache n’est pas inclus dans le package `Microsoft.AspNetCore.App`. vous devez donc référencer le package NCache séparément dans votre fichier projet.</span><span class="sxs-lookup"><span data-stu-id="7d363-130">The NCache package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the NCache package separately in your project file.</span></span>

::: moniker-end

## <a name="idistributedcache-interface"></a><span data-ttu-id="7d363-131">Interface IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="7d363-131">IDistributedCache interface</span></span>

<span data-ttu-id="7d363-132">L’interface <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> fournit les méthodes suivantes pour manipuler des éléments dans l’implémentation de cache distribué :</span><span class="sxs-lookup"><span data-stu-id="7d363-132">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="7d363-133"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; accepte une clé de chaîne et récupère un élément mis en cache en tant que tableau de `byte[]`, s’il est trouvé dans le cache.</span><span class="sxs-lookup"><span data-stu-id="7d363-133"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="7d363-134"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; ajoute un élément (en tant que tableau de `byte[]`) au cache à l’aide d’une clé de chaîne.</span><span class="sxs-lookup"><span data-stu-id="7d363-134"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="7d363-135"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; actualise un élément dans le cache en fonction de sa clé, en réinitialisant son délai d’expiration décalé (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="7d363-135"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="7d363-136"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; supprime un élément de cache en fonction de sa clé de chaîne.</span><span class="sxs-lookup"><span data-stu-id="7d363-136"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="7d363-137">Établir des services de mise en cache distribuée</span><span class="sxs-lookup"><span data-stu-id="7d363-137">Establish distributed caching services</span></span>

<span data-ttu-id="7d363-138">Inscrire une implémentation de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7d363-138">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="7d363-139">Les implémentations fournies par le Framework décrites dans cette rubrique sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="7d363-139">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="7d363-140">Cache mémoire distribuée</span><span class="sxs-lookup"><span data-stu-id="7d363-140">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="7d363-141">Cache de SQL Server distribué</span><span class="sxs-lookup"><span data-stu-id="7d363-141">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="7d363-142">Cache Redims distribué</span><span class="sxs-lookup"><span data-stu-id="7d363-142">Distributed Redis cache</span></span>](#distributed-redis-cache)
* [<span data-ttu-id="7d363-143">Cache NCache distribué</span><span class="sxs-lookup"><span data-stu-id="7d363-143">Distributed NCache cache</span></span>](#distributed-ncache-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="7d363-144">Cache mémoire distribuée</span><span class="sxs-lookup"><span data-stu-id="7d363-144">Distributed Memory Cache</span></span>

<span data-ttu-id="7d363-145">Le cache mémoire distribuée (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) est une implémentation fournie par le Framework d' <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> qui stocke des éléments en mémoire.</span><span class="sxs-lookup"><span data-stu-id="7d363-145">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> that stores items in memory.</span></span> <span data-ttu-id="7d363-146">Le cache mémoire distribuée n’est pas un cache distribué réel.</span><span class="sxs-lookup"><span data-stu-id="7d363-146">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="7d363-147">Les éléments mis en cache sont stockés par l’instance de l’application sur le serveur sur lequel l’application s’exécute.</span><span class="sxs-lookup"><span data-stu-id="7d363-147">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="7d363-148">Le cache mémoire distribuée est une implémentation utile :</span><span class="sxs-lookup"><span data-stu-id="7d363-148">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="7d363-149">Dans les scénarios de développement et de test.</span><span class="sxs-lookup"><span data-stu-id="7d363-149">In development and testing scenarios.</span></span>
* <span data-ttu-id="7d363-150">Lorsqu’un serveur unique est utilisé en production et que la consommation de mémoire n’est pas un problème.</span><span class="sxs-lookup"><span data-stu-id="7d363-150">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="7d363-151">L’implémentation du cache de mémoire distribuée résume le stockage des données mises en cache.</span><span class="sxs-lookup"><span data-stu-id="7d363-151">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="7d363-152">Il permet d’implémenter une solution de mise en cache distribuée réelle à l’avenir si plusieurs nœuds ou une tolérance de panne sont nécessaires.</span><span class="sxs-lookup"><span data-stu-id="7d363-152">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="7d363-153">L’exemple d’application utilise le cache de mémoire distribuée lorsque l’application est exécutée dans l’environnement de développement dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7d363-153">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="7d363-154">Cache de SQL Server distribué</span><span class="sxs-lookup"><span data-stu-id="7d363-154">Distributed SQL Server Cache</span></span>

<span data-ttu-id="7d363-155">L’implémentation du cache SQL Server distribué (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) permet au cache distribué d’utiliser une base de données SQL Server comme magasin de stockage.</span><span class="sxs-lookup"><span data-stu-id="7d363-155">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="7d363-156">Pour créer un SQL Server table d’éléments mis en cache dans une instance de SQL Server, vous pouvez utiliser l’outil `sql-cache`.</span><span class="sxs-lookup"><span data-stu-id="7d363-156">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="7d363-157">L’outil crée une table avec le nom et le schéma que vous spécifiez.</span><span class="sxs-lookup"><span data-stu-id="7d363-157">The tool creates a table with the name and schema that you specify.</span></span>

<span data-ttu-id="7d363-158">Créez une table dans SQL Server en exécutant la commande `sql-cache create`.</span><span class="sxs-lookup"><span data-stu-id="7d363-158">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="7d363-159">Indiquez l’instance de SQL Server (`Data Source`), la base de données (`Initial Catalog`), le schéma (par exemple, `dbo`) et le nom de la table (par exemple, `TestCache`) :</span><span class="sxs-lookup"><span data-stu-id="7d363-159">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```dotnetcli
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="7d363-160">Un message est consigné pour indiquer la réussite de l’outil :</span><span class="sxs-lookup"><span data-stu-id="7d363-160">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="7d363-161">La table créée par l’outil `sql-cache` a le schéma suivant :</span><span class="sxs-lookup"><span data-stu-id="7d363-161">The table created by the `sql-cache` tool has the following schema:</span></span>

![Table de cache SqlServer](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="7d363-163">Une application doit manipuler les valeurs de cache à l’aide d’une instance de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, et non d’une <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span><span class="sxs-lookup"><span data-stu-id="7d363-163">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="7d363-164">L’exemple d’application implémente <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> dans un environnement de non-développement dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7d363-164">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="7d363-165">Une <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (et éventuellement, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> et <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) sont généralement stockées en dehors du contrôle de code source (par exemple, stockées par le [Gestionnaire de secret](xref:security/app-secrets) ou dans *appsettings. JSON*/*appSettings. { Fichiers ENVIRONMENT}. JSON* ).</span><span class="sxs-lookup"><span data-stu-id="7d363-165">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{ENVIRONMENT}.json* files).</span></span> <span data-ttu-id="7d363-166">La chaîne de connexion peut contenir des informations d’identification qui doivent être conservées hors des systèmes de contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="7d363-166">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="7d363-167">Cache Redims distribué</span><span class="sxs-lookup"><span data-stu-id="7d363-167">Distributed Redis Cache</span></span>

<span data-ttu-id="7d363-168">[Redims](https://redis.io/) est un magasin de données en mémoire Open source, qui est souvent utilisé comme un cache distribué.</span><span class="sxs-lookup"><span data-stu-id="7d363-168">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="7d363-169">Vous pouvez utiliser des ReDim localement, et vous pouvez configurer un [cache redims Azure](https://azure.microsoft.com/services/cache/) pour une application ASP.net Core hébergée sur Azure.</span><span class="sxs-lookup"><span data-stu-id="7d363-169">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7d363-170">Une application configure l’implémentation du cache à l’aide d’une instance de <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) dans un environnement de non-développement dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7d363-170">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="7d363-171">Une application configure l’implémentation du cache à l’aide d’une instance de <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) dans un environnement de non-développement dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7d363-171">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7d363-172">Une application configure l’implémentation du cache à l’aide d’une instance de <xref:Microsoft.Extensions.Caching.Redis.RedisCache> (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>) :</span><span class="sxs-lookup"><span data-stu-id="7d363-172">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.Redis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span></span>

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

::: moniker-end

<span data-ttu-id="7d363-173">Pour installer les éléments ReDim sur votre ordinateur local :</span><span class="sxs-lookup"><span data-stu-id="7d363-173">To install Redis on your local machine:</span></span>

1. <span data-ttu-id="7d363-174">Installez le [package redims en chocolat](https://chocolatey.org/packages/redis-64/).</span><span class="sxs-lookup"><span data-stu-id="7d363-174">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
1. <span data-ttu-id="7d363-175">Exécutez `redis-server` à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="7d363-175">Run `redis-server` from a command prompt.</span></span>

### <a name="distributed-ncache-cache"></a><span data-ttu-id="7d363-176">Cache NCache distribué</span><span class="sxs-lookup"><span data-stu-id="7d363-176">Distributed NCache Cache</span></span>

<span data-ttu-id="7d363-177">[NCache](https://github.com/Alachisoft/NCache) est un cache distribué en mémoire Open source développé en mode natif dans .NET et .net core.</span><span class="sxs-lookup"><span data-stu-id="7d363-177">[NCache](https://github.com/Alachisoft/NCache) is an open source in-memory distributed cache developed natively in .NET and .NET Core.</span></span> <span data-ttu-id="7d363-178">NCache fonctionne localement et est configuré en tant que cluster de cache distribué pour une application ASP.NET Core s’exécutant dans Azure ou sur d’autres plateformes d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="7d363-178">NCache works both locally and configured as a distributed cache cluster for an ASP.NET Core app running in Azure or on other hosting platforms.</span></span>

<span data-ttu-id="7d363-179">Pour installer et configurer NCache sur votre ordinateur local, consultez le [Guide de prise en main NCache pour Windows](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).</span><span class="sxs-lookup"><span data-stu-id="7d363-179">To install and configure NCache on your local machine, see [NCache Getting Started Guide for Windows](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).</span></span>

<span data-ttu-id="7d363-180">Pour configurer NCache :</span><span class="sxs-lookup"><span data-stu-id="7d363-180">To configure NCache:</span></span>

1. <span data-ttu-id="7d363-181">Installez [NuGet Open source de NCache](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/).</span><span class="sxs-lookup"><span data-stu-id="7d363-181">Install [NCache open source NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/).</span></span>
1. <span data-ttu-id="7d363-182">Configurez le cluster de cache dans [client. ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html).</span><span class="sxs-lookup"><span data-stu-id="7d363-182">Configure the cache cluster in [client.ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html).</span></span>
1. <span data-ttu-id="7d363-183">Ajoutez le code suivant à `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="7d363-183">Add the following code to `Startup.ConfigureServices`:</span></span>

   ```csharp
   services.AddNCacheDistributedCache(configuration =>    
   {        
       configuration.CacheName = "demoClusteredCache";
       configuration.EnableLogs = true;
       configuration.ExceptionsEnabled = true;
   });
   ```

## <a name="use-the-distributed-cache"></a><span data-ttu-id="7d363-184">Utiliser le cache distribué</span><span class="sxs-lookup"><span data-stu-id="7d363-184">Use the distributed cache</span></span>

<span data-ttu-id="7d363-185">Pour utiliser l’interface <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, demandez une instance de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> à partir de n’importe quel constructeur dans l’application.</span><span class="sxs-lookup"><span data-stu-id="7d363-185">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> from any constructor in the app.</span></span> <span data-ttu-id="7d363-186">L’instance est fournie par l' [injection de dépendances (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7d363-186">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7d363-187">Au démarrage de l’exemple d’application, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> est injecté dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="7d363-187">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="7d363-188">L’heure actuelle est mise en cache à l’aide de <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (pour plus d’informations, consultez [hôte générique : IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)) :</span><span class="sxs-lookup"><span data-stu-id="7d363-188">The current time is cached using <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (for more information, see [Generic Host: IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)):</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7d363-189">Au démarrage de l’exemple d’application, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> est injecté dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="7d363-189">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="7d363-190">L’heure actuelle est mise en cache à l’aide de <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (pour plus d’informations, consultez [hôte Web : IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)) :</span><span class="sxs-lookup"><span data-stu-id="7d363-190">The current time is cached using <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (for more information, see [Web Host: IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

<span data-ttu-id="7d363-191">L’exemple d’application injecte <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> dans le `IndexModel` pour une utilisation par la page d’index.</span><span class="sxs-lookup"><span data-stu-id="7d363-191">The sample app injects <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="7d363-192">Chaque fois que la page d’index est chargée, la durée de mise en cache est vérifiée dans `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="7d363-192">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="7d363-193">Si l’heure mise en cache n’a pas expiré, l’heure est affichée.</span><span class="sxs-lookup"><span data-stu-id="7d363-193">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="7d363-194">Si 20 secondes se sont écoulées depuis le dernier accès à l’heure de mise en cache (la dernière fois que cette page a été chargée), la page affiche le *temps mis en cache expiré*.</span><span class="sxs-lookup"><span data-stu-id="7d363-194">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="7d363-195">Mettez immédiatement à jour l’heure de mise en cache à l’heure actuelle en sélectionnant le bouton **réinitialisation du temps mis en cache** .</span><span class="sxs-lookup"><span data-stu-id="7d363-195">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="7d363-196">Le bouton déclenche la méthode du gestionnaire de `OnPostResetCachedTime`.</span><span class="sxs-lookup"><span data-stu-id="7d363-196">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="7d363-197">Il n’est pas nécessaire d’utiliser un singleton ou une durée de vie limitée pour <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (au moins pour les implémentations intégrées).</span><span class="sxs-lookup"><span data-stu-id="7d363-197">There's no need to use a Singleton or Scoped lifetime for <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="7d363-198">Vous pouvez également créer une instance de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> à chaque fois que vous en aurez besoin au lieu d’utiliser l’injection de dépendances, mais la création d’une instance dans du code peut rendre votre code plus difficile à tester et violer le [principe des dépendances explicites](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span><span class="sxs-lookup"><span data-stu-id="7d363-198">You can also create an <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="7d363-199">Recommandations</span><span class="sxs-lookup"><span data-stu-id="7d363-199">Recommendations</span></span>

<span data-ttu-id="7d363-200">Lorsque vous décidez de l’implémentation de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> la mieux adaptée à votre application, tenez compte des points suivants :</span><span class="sxs-lookup"><span data-stu-id="7d363-200">When deciding which implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is best for your app, consider the following:</span></span>

* <span data-ttu-id="7d363-201">Infrastructure existante</span><span class="sxs-lookup"><span data-stu-id="7d363-201">Existing infrastructure</span></span>
* <span data-ttu-id="7d363-202">Exigences en matière de performances</span><span class="sxs-lookup"><span data-stu-id="7d363-202">Performance requirements</span></span>
* <span data-ttu-id="7d363-203">Coût</span><span class="sxs-lookup"><span data-stu-id="7d363-203">Cost</span></span>
* <span data-ttu-id="7d363-204">Expérience de l’équipe</span><span class="sxs-lookup"><span data-stu-id="7d363-204">Team experience</span></span>

<span data-ttu-id="7d363-205">Les solutions de mise en cache s’appuient généralement sur le stockage en mémoire pour fournir une récupération rapide des données mises en cache, mais la mémoire est une ressource limitée et coûteuse à développer.</span><span class="sxs-lookup"><span data-stu-id="7d363-205">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="7d363-206">Stocke uniquement les données couramment utilisées dans un cache.</span><span class="sxs-lookup"><span data-stu-id="7d363-206">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="7d363-207">En règle générale, un cache Redims fournit un débit plus élevé et une latence inférieure à celle d’un cache de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7d363-207">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="7d363-208">Toutefois, l’évaluation est généralement requise pour déterminer les caractéristiques de performances des stratégies de mise en cache.</span><span class="sxs-lookup"><span data-stu-id="7d363-208">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="7d363-209">Lorsque SQL Server est utilisé en tant que magasin de stockage de cache distribué, l’utilisation de la même base de données pour le cache et du stockage et de la récupération de données ordinaires de l’application peut avoir un impact négatif sur les performances des deux.</span><span class="sxs-lookup"><span data-stu-id="7d363-209">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="7d363-210">Nous vous recommandons d’utiliser une instance de SQL Server dédiée pour le magasin de stockage de cache distribué.</span><span class="sxs-lookup"><span data-stu-id="7d363-210">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7d363-211">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="7d363-211">Additional resources</span></span>

* [<span data-ttu-id="7d363-212">Cache redims sur Azure</span><span class="sxs-lookup"><span data-stu-id="7d363-212">Redis Cache on Azure</span></span>](/azure/azure-cache-for-redis/)
* [<span data-ttu-id="7d363-213">SQL Database sur Azure</span><span class="sxs-lookup"><span data-stu-id="7d363-213">SQL Database on Azure</span></span>](/azure/sql-database/)
* <span data-ttu-id="7d363-214">[ASP.net Core fournisseur IDistributedCache pour NCache dans les batteries de serveurs Web](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache sur GitHub](https://github.com/Alachisoft/NCache))</span><span class="sxs-lookup"><span data-stu-id="7d363-214">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
