---
title: Mise en cache distribuée dans ASP.NET Core
author: guardrex
description: Découvrez comment utiliser un cache ASP.NET Core distribué pour améliorer les performances et l’évolutivité des applications, en particulier dans un environnement de batterie de serveurs ou de Cloud.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/27/2019
uid: performance/caching/distributed
ms.openlocfilehash: 8417463038bcdc0f77852bec3c3bb8a618153009
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773851"
---
# <a name="distributed-caching-in-aspnet-core"></a><span data-ttu-id="e7671-103">Mise en cache distribuée dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7671-103">Distributed caching in ASP.NET Core</span></span>

<span data-ttu-id="e7671-104">Par [Luke Latham](https://github.com/guardrex) et [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e7671-104">By [Luke Latham](https://github.com/guardrex) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e7671-105">Un cache distribué est un cache partagé par plusieurs serveurs d’applications, généralement géré comme un service externe pour les serveurs d’applications qui y accèdent.</span><span class="sxs-lookup"><span data-stu-id="e7671-105">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="e7671-106">Un cache distribué peut améliorer les performances et l’extensibilité d’une application ASP.NET Core, en particulier lorsque l’application est hébergée par un service Cloud ou une batterie de serveurs.</span><span class="sxs-lookup"><span data-stu-id="e7671-106">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="e7671-107">Un cache distribué présente plusieurs avantages par rapport à d’autres scénarios de mise en cache où les données mises en cache sont stockées sur des serveurs d’applications individuels.</span><span class="sxs-lookup"><span data-stu-id="e7671-107">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="e7671-108">Lorsque les données mises en cache sont distribuées, les données :</span><span class="sxs-lookup"><span data-stu-id="e7671-108">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="e7671-109">Est *cohérente* (cohérente) entre les demandes adressées à plusieurs serveurs.</span><span class="sxs-lookup"><span data-stu-id="e7671-109">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="e7671-110">Survit aux redémarrages du serveur et aux déploiements d’applications.</span><span class="sxs-lookup"><span data-stu-id="e7671-110">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="e7671-111">N’utilise pas la mémoire locale.</span><span class="sxs-lookup"><span data-stu-id="e7671-111">Doesn't use local memory.</span></span>

<span data-ttu-id="e7671-112">La configuration du cache distribué est spécifique à l’implémentation.</span><span class="sxs-lookup"><span data-stu-id="e7671-112">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="e7671-113">Cet article explique comment configurer des caches distribués SQL Server et Redims.</span><span class="sxs-lookup"><span data-stu-id="e7671-113">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="e7671-114">Des implémentations tierces sont également disponibles, telles que [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache sur GitHub](https://github.com/Alachisoft/NCache)).</span><span class="sxs-lookup"><span data-stu-id="e7671-114">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="e7671-115">Quelle que soit l’implémentation sélectionnée, l’application interagit avec le cache à l' <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> aide de l’interface.</span><span class="sxs-lookup"><span data-stu-id="e7671-115">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="e7671-116">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e7671-116">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e7671-117">Prérequis</span><span class="sxs-lookup"><span data-stu-id="e7671-117">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e7671-118">Pour utiliser un SQL Server cache distribué, ajoutez une référence de package au package [Microsoft. extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="e7671-118">To use a SQL Server distributed cache, add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="e7671-119">Pour utiliser un cache redistribuable ReDim, ajoutez une référence de package au package [Microsoft. extensions. Caching. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) .</span><span class="sxs-lookup"><span data-stu-id="e7671-119">To use a Redis distributed cache, add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="e7671-120">Pour utiliser un SQL Server cache distribué, référencez le [AspNetCore. app. app](xref:fundamentals/metapackage-app) ou ajoutez une référence de package au package [Microsoft. extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="e7671-120">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="e7671-121">Pour utiliser un cache distribué Redims, référencez le package [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) et ajoutez une référence de package au package [Microsoft. extensions. Caching. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) .</span><span class="sxs-lookup"><span data-stu-id="e7671-121">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span> <span data-ttu-id="e7671-122">Le package redims n’étant pas inclus `Microsoft.AspNetCore.App` dans le package, vous devez référencer le package redims séparément dans votre fichier projet.</span><span class="sxs-lookup"><span data-stu-id="e7671-122">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="e7671-123">Pour utiliser un SQL Server cache distribué, référencez le [AspNetCore. app. app](xref:fundamentals/metapackage-app) ou ajoutez une référence de package au package [Microsoft. extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="e7671-123">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="e7671-124">Pour utiliser un cache redistribuable ReDim, référencez le package [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) et ajoutez une référence de package au package [Microsoft. extensions. Caching. redims](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) .</span><span class="sxs-lookup"><span data-stu-id="e7671-124">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span> <span data-ttu-id="e7671-125">Le package redims n’étant pas inclus `Microsoft.AspNetCore.App` dans le package, vous devez référencer le package redims séparément dans votre fichier projet.</span><span class="sxs-lookup"><span data-stu-id="e7671-125">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

::: moniker-end

## <a name="idistributedcache-interface"></a><span data-ttu-id="e7671-126">Interface IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="e7671-126">IDistributedCache interface</span></span>

<span data-ttu-id="e7671-127">L' <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface fournit les méthodes suivantes pour manipuler des éléments dans l’implémentation de cache distribué :</span><span class="sxs-lookup"><span data-stu-id="e7671-127">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="e7671-128"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> `byte[]` Accepte une clé de chaîne et récupère un élément mis en cache en tant que tableau s’il est trouvé dans le cache. &ndash;</span><span class="sxs-lookup"><span data-stu-id="e7671-128"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="e7671-129"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> `byte[]` Ajoute un élément (en tant que tableau) au cache à l’aide d’une clé de chaîne. &ndash;</span><span class="sxs-lookup"><span data-stu-id="e7671-129"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="e7671-130"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> ,&ndash; Actualise un élément dans le cache en fonction de sa clé, en réinitialisant son délai d’expiration décalé (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="e7671-130"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="e7671-131"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> ,&ndash; Supprime un élément de cache en fonction de sa clé de chaîne.</span><span class="sxs-lookup"><span data-stu-id="e7671-131"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="e7671-132">Établir des services de mise en cache distribuée</span><span class="sxs-lookup"><span data-stu-id="e7671-132">Establish distributed caching services</span></span>

<span data-ttu-id="e7671-133">Inscrire une implémentation de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e7671-133">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e7671-134">Les implémentations fournies par le Framework décrites dans cette rubrique sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="e7671-134">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="e7671-135">Cache mémoire distribuée</span><span class="sxs-lookup"><span data-stu-id="e7671-135">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="e7671-136">Cache de SQL Server distribué</span><span class="sxs-lookup"><span data-stu-id="e7671-136">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="e7671-137">Cache Redims distribué</span><span class="sxs-lookup"><span data-stu-id="e7671-137">Distributed Redis cache</span></span>](#distributed-redis-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="e7671-138">Cache mémoire distribuée</span><span class="sxs-lookup"><span data-stu-id="e7671-138">Distributed Memory Cache</span></span>

<span data-ttu-id="e7671-139">Le cache mémoire distribuée<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>() est une implémentation fournie par le <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> Framework de qui stocke des éléments en mémoire.</span><span class="sxs-lookup"><span data-stu-id="e7671-139">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> that stores items in memory.</span></span> <span data-ttu-id="e7671-140">Le cache mémoire distribuée n’est pas un cache distribué réel.</span><span class="sxs-lookup"><span data-stu-id="e7671-140">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="e7671-141">Les éléments mis en cache sont stockés par l’instance de l’application sur le serveur sur lequel l’application s’exécute.</span><span class="sxs-lookup"><span data-stu-id="e7671-141">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="e7671-142">Le cache mémoire distribuée est une implémentation utile :</span><span class="sxs-lookup"><span data-stu-id="e7671-142">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="e7671-143">Dans les scénarios de développement et de test.</span><span class="sxs-lookup"><span data-stu-id="e7671-143">In development and testing scenarios.</span></span>
* <span data-ttu-id="e7671-144">Lorsqu’un serveur unique est utilisé en production et que la consommation de mémoire n’est pas un problème.</span><span class="sxs-lookup"><span data-stu-id="e7671-144">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="e7671-145">L’implémentation du cache de mémoire distribuée résume le stockage des données mises en cache.</span><span class="sxs-lookup"><span data-stu-id="e7671-145">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="e7671-146">Il permet d’implémenter une solution de mise en cache distribuée réelle à l’avenir si plusieurs nœuds ou une tolérance de panne sont nécessaires.</span><span class="sxs-lookup"><span data-stu-id="e7671-146">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="e7671-147">L’exemple d’application utilise le cache de mémoire distribuée lorsque l’application est exécutée dans l’environnement `Startup.ConfigureServices`de développement dans :</span><span class="sxs-lookup"><span data-stu-id="e7671-147">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="e7671-148">Cache de SQL Server distribué</span><span class="sxs-lookup"><span data-stu-id="e7671-148">Distributed SQL Server Cache</span></span>

<span data-ttu-id="e7671-149">L’implémentation du cache SQL Server distribué<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>() permet au cache distribué d’utiliser une base de données SQL Server comme magasin de stockage.</span><span class="sxs-lookup"><span data-stu-id="e7671-149">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="e7671-150">Pour créer un SQL Server table d’éléments mis en cache dans une instance de SQL Server, vous `sql-cache` pouvez utiliser l’outil.</span><span class="sxs-lookup"><span data-stu-id="e7671-150">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="e7671-151">L’outil crée une table avec le nom et le schéma que vous spécifiez.</span><span class="sxs-lookup"><span data-stu-id="e7671-151">The tool creates a table with the name and schema that you specify.</span></span>

<span data-ttu-id="e7671-152">Créer une table dans SQL Server en exécutant la commande `sql-cache create` :</span><span class="sxs-lookup"><span data-stu-id="e7671-152">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="e7671-153">Indiquez l’instance de SQL Server`Data Source`(), la`Initial Catalog`base de données (), le `dbo`schéma (par exemple,) et le nom `TestCache`de la table (par exemple,) :</span><span class="sxs-lookup"><span data-stu-id="e7671-153">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="e7671-154">Un message est consigné pour indiquer la réussite de l’outil :</span><span class="sxs-lookup"><span data-stu-id="e7671-154">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="e7671-155">La table créée par l' `sql-cache` outil a le schéma suivant :</span><span class="sxs-lookup"><span data-stu-id="e7671-155">The table created by the `sql-cache` tool has the following schema:</span></span>

![Table de cache SqlServer](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="e7671-157">Une application doit manipuler les valeurs de cache à l' <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>aide d’une <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>instance de, et non d’un.</span><span class="sxs-lookup"><span data-stu-id="e7671-157">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="e7671-158">L’exemple d’application implémente <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> dans un environnement de non-développement dans : `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="e7671-158">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="e7671-159"><xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> /(Et éventuellement et<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) sont généralement stockés en dehors du contrôle de code source (par exemple, stocké par le [Gestionnaire de secret](xref:security/app-secrets) ou dans *appSettings. JSON*appSettings. { <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*>  *Fichiers ENVIRONMENT}. JSON* ).</span><span class="sxs-lookup"><span data-stu-id="e7671-159">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{ENVIRONMENT}.json* files).</span></span> <span data-ttu-id="e7671-160">La chaîne de connexion peut contenir des informations d’identification qui doivent être conservées hors des systèmes de contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="e7671-160">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="e7671-161">Cache Redims distribué</span><span class="sxs-lookup"><span data-stu-id="e7671-161">Distributed Redis Cache</span></span>

<span data-ttu-id="e7671-162">[Redis](https://redis.io/) est un magasin de données en mémoire open source, qui est souvent utilisé comme un cache distribué.</span><span class="sxs-lookup"><span data-stu-id="e7671-162">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="e7671-163">Vous pouvez utiliser des ReDim localement, et vous pouvez configurer un [cache redims Azure](https://azure.microsoft.com/services/cache/) pour une application ASP.net Core hébergée sur Azure.</span><span class="sxs-lookup"><span data-stu-id="e7671-163">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e7671-164">Une application configure l’implémentation du cache à l' <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> aide d'<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>une instance () dans un environnement de `Startup.ConfigureServices`non-développement dans :</span><span class="sxs-lookup"><span data-stu-id="e7671-164">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="e7671-165">Une application configure l’implémentation du cache à l' <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> aide d'<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>une instance () dans un environnement de `Startup.ConfigureServices`non-développement dans :</span><span class="sxs-lookup"><span data-stu-id="e7671-165">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="e7671-166">Une application configure l’implémentation du cache à l' <xref:Microsoft.Extensions.Caching.Redis.RedisCache> aide d'<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>une instance () :</span><span class="sxs-lookup"><span data-stu-id="e7671-166">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.Redis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span></span>

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

::: moniker-end

<span data-ttu-id="e7671-167">Pour installer les éléments ReDim sur votre ordinateur local :</span><span class="sxs-lookup"><span data-stu-id="e7671-167">To install Redis on your local machine:</span></span>

* <span data-ttu-id="e7671-168">Installez le [package redims en chocolat](https://chocolatey.org/packages/redis-64/).</span><span class="sxs-lookup"><span data-stu-id="e7671-168">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
* <span data-ttu-id="e7671-169">Exécutez `redis-server` à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="e7671-169">Run `redis-server` from a command prompt.</span></span>

## <a name="use-the-distributed-cache"></a><span data-ttu-id="e7671-170">Utiliser le cache distribué</span><span class="sxs-lookup"><span data-stu-id="e7671-170">Use the distributed cache</span></span>

<span data-ttu-id="e7671-171">Pour utiliser l' <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, demandez une instance de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> à partir de n’importe quel constructeur de l’application.</span><span class="sxs-lookup"><span data-stu-id="e7671-171">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> from any constructor in the app.</span></span> <span data-ttu-id="e7671-172">L’instance est fournie par l' [injection de dépendances (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e7671-172">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e7671-173">Lorsque l’exemple d’application démarre <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> , est injecté dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="e7671-173">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="e7671-174">L’heure actuelle est mise en cache <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> à l’aide de (pour [plus d’informations, consultez hôte générique : IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)) :</span><span class="sxs-lookup"><span data-stu-id="e7671-174">The current time is cached using <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (for more information, see [Generic Host: IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)):</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e7671-175">Lorsque l’exemple d’application démarre <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> , est injecté dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="e7671-175">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="e7671-176">L’heure actuelle est mise en cache <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> à l’aide de (pour [plus d’informations, consultez hôte Web : Interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)IApplicationLifetime) :</span><span class="sxs-lookup"><span data-stu-id="e7671-176">The current time is cached using <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (for more information, see [Web Host: IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

<span data-ttu-id="e7671-177">L’exemple d’application injecte <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> dans le `IndexModel` pour une utilisation par la page d’index.</span><span class="sxs-lookup"><span data-stu-id="e7671-177">The sample app injects <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="e7671-178">Chaque fois que la page d’index est chargée, la durée de mise en cache est vérifiée dans `OnGetAsync`le cache.</span><span class="sxs-lookup"><span data-stu-id="e7671-178">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="e7671-179">Si l’heure mise en cache n’a pas expiré, l’heure est affichée.</span><span class="sxs-lookup"><span data-stu-id="e7671-179">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="e7671-180">Si 20 secondes se sont écoulées depuis le dernier accès à l’heure de mise en cache (la dernière fois que cette page a été chargée), la page affiche le *temps mis en cache expiré*.</span><span class="sxs-lookup"><span data-stu-id="e7671-180">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="e7671-181">Mettez immédiatement à jour l’heure de mise en cache à l’heure actuelle en sélectionnant le bouton **réinitialisation du temps mis en cache** .</span><span class="sxs-lookup"><span data-stu-id="e7671-181">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="e7671-182">Le bouton déclenche la méthode `OnPostResetCachedTime` de gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="e7671-182">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="e7671-183">Il est inutile d’utiliser un Singleton ou une durée de vie limitée à un périmètre d'utilisation pour gérer des instances de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> (et ceci au moins pour les implémentations intégrées).</span><span class="sxs-lookup"><span data-stu-id="e7671-183">There's no need to use a Singleton or Scoped lifetime for <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="e7671-184">Vous pouvez également créer une <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance de chaque fois que vous en aurez besoin au lieu d’utiliser l’injection de dépendances, mais la création d’une instance dans du code peut compliquer le test et violer le [principe des dépendances explicites](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span><span class="sxs-lookup"><span data-stu-id="e7671-184">You can also create an <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="e7671-185">Recommandations</span><span class="sxs-lookup"><span data-stu-id="e7671-185">Recommendations</span></span>

<span data-ttu-id="e7671-186">Lorsque vous décidez de l' <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> implémentation qui convient le mieux à votre application, tenez compte des points suivants :</span><span class="sxs-lookup"><span data-stu-id="e7671-186">When deciding which implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is best for your app, consider the following:</span></span>

* <span data-ttu-id="e7671-187">Infrastructure existante</span><span class="sxs-lookup"><span data-stu-id="e7671-187">Existing infrastructure</span></span>
* <span data-ttu-id="e7671-188">Exigences en matière de performances</span><span class="sxs-lookup"><span data-stu-id="e7671-188">Performance requirements</span></span>
* <span data-ttu-id="e7671-189">Coût</span><span class="sxs-lookup"><span data-stu-id="e7671-189">Cost</span></span>
* <span data-ttu-id="e7671-190">Expérience de l’équipe</span><span class="sxs-lookup"><span data-stu-id="e7671-190">Team experience</span></span>

<span data-ttu-id="e7671-191">Les solutions de mise en cache s’appuient généralement sur le stockage en mémoire pour fournir une récupération rapide des données mises en cache, mais la mémoire est une ressource limitée et coûteuse à développer.</span><span class="sxs-lookup"><span data-stu-id="e7671-191">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="e7671-192">Stocke uniquement les données couramment utilisées dans un cache.</span><span class="sxs-lookup"><span data-stu-id="e7671-192">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="e7671-193">En règle générale, un cache Redims fournit un débit plus élevé et une latence inférieure à celle d’un cache de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e7671-193">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="e7671-194">Toutefois, l’évaluation est généralement requise pour déterminer les caractéristiques de performances des stratégies de mise en cache.</span><span class="sxs-lookup"><span data-stu-id="e7671-194">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="e7671-195">Lorsque SQL Server est utilisé en tant que magasin de stockage de cache distribué, l’utilisation de la même base de données pour le cache et du stockage et de la récupération de données ordinaires de l’application peut avoir un impact négatif sur les performances des deux.</span><span class="sxs-lookup"><span data-stu-id="e7671-195">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="e7671-196">Nous vous recommandons d’utiliser une instance de SQL Server dédiée pour le magasin de stockage de cache distribué.</span><span class="sxs-lookup"><span data-stu-id="e7671-196">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e7671-197">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e7671-197">Additional resources</span></span>

* [<span data-ttu-id="e7671-198">Cache redims sur Azure</span><span class="sxs-lookup"><span data-stu-id="e7671-198">Redis Cache on Azure</span></span>](/azure/azure-cache-for-redis/)
* [<span data-ttu-id="e7671-199">SQL Database sur Azure</span><span class="sxs-lookup"><span data-stu-id="e7671-199">SQL Database on Azure</span></span>](/azure/sql-database/)
* <span data-ttu-id="e7671-200">[ASP.net Core fournisseur IDistributedCache pour NCache dans les batteries de serveurs Web](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache sur GitHub](https://github.com/Alachisoft/NCache))</span><span class="sxs-lookup"><span data-stu-id="e7671-200">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
