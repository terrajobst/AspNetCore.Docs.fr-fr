---
title: Mise en cache dans ASP.NET Core distribuée
author: guardrex
description: Découvrez comment utiliser un cache d’ASP.NET Core est distribué pour améliorer les performances des applications et l’évolutivité, en particulier dans un environnement de batterie de serveurs cloud ou un serveur.
ms.author: riande
ms.custom: mvc
ms.date: 10/19/2018
uid: performance/caching/distributed
ms.openlocfilehash: d80cde372535aa04604ce0cd5a731a1448515093
ms.sourcegitcommit: 4a6bbe84db24c2f3dd2de065de418fde952c8d40
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50253006"
---
# <a name="distributed-caching-in-aspnet-core"></a><span data-ttu-id="ae29d-103">Mise en cache dans ASP.NET Core distribuée</span><span class="sxs-lookup"><span data-stu-id="ae29d-103">Distributed caching in ASP.NET Core</span></span>

<span data-ttu-id="ae29d-104">Par [Steve Smith](https://ardalis.com/) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ae29d-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ae29d-105">Un cache distribué est un cache partagé par plusieurs serveurs d’applications, généralement gérées comme un service externe pour les serveurs d’application qui y accèdent.</span><span class="sxs-lookup"><span data-stu-id="ae29d-105">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="ae29d-106">Un cache distribué peut améliorer les performances et l’évolutivité d’une application ASP.NET Core, en particulier lorsque l’application est hébergée par un service cloud ou une batterie de serveurs.</span><span class="sxs-lookup"><span data-stu-id="ae29d-106">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="ae29d-107">Un cache distribué présente plusieurs avantages sur les autres scénarios de mise en cache où les données mises en cache sont stockées sur les serveurs d’applications individuelles.</span><span class="sxs-lookup"><span data-stu-id="ae29d-107">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="ae29d-108">Lorsque les données mises en cache sont distribuées, les données :</span><span class="sxs-lookup"><span data-stu-id="ae29d-108">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="ae29d-109">Est *cohérente* (cohérent) entre les requêtes vers plusieurs serveurs.</span><span class="sxs-lookup"><span data-stu-id="ae29d-109">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="ae29d-110">Persiste lors des redémarrages de serveur et des déploiements d’applications.</span><span class="sxs-lookup"><span data-stu-id="ae29d-110">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="ae29d-111">N’utilise pas la mémoire locale.</span><span class="sxs-lookup"><span data-stu-id="ae29d-111">Doesn't use local memory.</span></span>

<span data-ttu-id="ae29d-112">Configuration de cache distribué est spécifique à l’implémentation.</span><span class="sxs-lookup"><span data-stu-id="ae29d-112">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="ae29d-113">Cet article décrit comment configurer SQL Server et les caches distribués Redis.</span><span class="sxs-lookup"><span data-stu-id="ae29d-113">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="ae29d-114">Les implémentations de tiers sont également disponibles, comme [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache sur GitHub](https://github.com/Alachisoft/NCache)).</span><span class="sxs-lookup"><span data-stu-id="ae29d-114">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="ae29d-115">Quel que soit l’implémentation qui est sélectionnée, l’application interagit avec le cache à l’aide de la <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span><span class="sxs-lookup"><span data-stu-id="ae29d-115">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="ae29d-116">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ae29d-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ae29d-117">Prérequis</span><span class="sxs-lookup"><span data-stu-id="ae29d-117">Prerequisites</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ae29d-118">Pour utiliser un serveur SQL Server distributed cache, référence le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app) ou ajouter une référence de package à la [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span><span class="sxs-lookup"><span data-stu-id="ae29d-118">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="ae29d-119">Pour utiliser un Redis distributed cache, référence le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app) et ajouter une référence de package à la [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span><span class="sxs-lookup"><span data-stu-id="ae29d-119">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span> <span data-ttu-id="ae29d-120">Le package Redis n’est pas inclus dans le `Microsoft.AspNetCore.App` du package, que vous devez référencer le package Redis séparément dans votre fichier projet.</span><span class="sxs-lookup"><span data-stu-id="ae29d-120">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="ae29d-121">Pour utiliser un serveur SQL Server distributed cache, référence le [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ou ajouter une référence de package à la [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span><span class="sxs-lookup"><span data-stu-id="ae29d-121">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="ae29d-122">Pour utiliser un Redis distributed cache, référence le [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ou ajouter une référence de package à la [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span><span class="sxs-lookup"><span data-stu-id="ae29d-122">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span> <span data-ttu-id="ae29d-123">Le package Redis est inclus dans `Microsoft.AspNetCore.All` du package, vous n’avez pas besoin de référencer le package Redis séparément dans votre fichier projet.</span><span class="sxs-lookup"><span data-stu-id="ae29d-123">The Redis package is included in `Microsoft.AspNetCore.All` package, so you don't need to reference the Redis package separately in your project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ae29d-124">Pour utiliser un serveur SQL Server de cache distribué, ajoutez une référence de package pour le [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span><span class="sxs-lookup"><span data-stu-id="ae29d-124">To use a SQL Server distributed cache, add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="ae29d-125">Pour utiliser un Redis cache distribué, ajoutez une référence de package pour le [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span><span class="sxs-lookup"><span data-stu-id="ae29d-125">To use a Redis distributed cache, add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span>

::: moniker-end

## <a name="idistributedcache-interface"></a><span data-ttu-id="ae29d-126">Interface de IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="ae29d-126">IDistributedCache interface</span></span>

<span data-ttu-id="ae29d-127">Le <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface fournit les méthodes suivantes pour manipuler des éléments dans l’implémentation de cache distribué :</span><span class="sxs-lookup"><span data-stu-id="ae29d-127">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="ae29d-128"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Accepte une clé de chaîne et récupère un élément mis en cache comme un `byte[]` tableau si trouvé dans le cache.</span><span class="sxs-lookup"><span data-stu-id="ae29d-128"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="ae29d-129"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Ajoute un élément (comme `byte[]` tableau) dans le cache à l’aide d’une clé de chaîne.</span><span class="sxs-lookup"><span data-stu-id="ae29d-129"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="ae29d-130"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Actualise un élément dans le cache en fonction de sa clé, la réinitialisation de son délai d’expiration décalée (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="ae29d-130"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="ae29d-131"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Supprime un élément de cache en fonction de sa clé de chaîne.</span><span class="sxs-lookup"><span data-stu-id="ae29d-131"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="ae29d-132">Établissez des services de mise en cache distribuées</span><span class="sxs-lookup"><span data-stu-id="ae29d-132">Establish distributed caching services</span></span>

<span data-ttu-id="ae29d-133">Inscrire une implémentation de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ae29d-133">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="ae29d-134">Les implémentations fournis par le Framework décrites dans cette rubrique sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="ae29d-134">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="ae29d-135">Cache de la mémoire distribuée</span><span class="sxs-lookup"><span data-stu-id="ae29d-135">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="ae29d-136">Cache distribué de SQL Server</span><span class="sxs-lookup"><span data-stu-id="ae29d-136">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="ae29d-137">Cache Redis distribué</span><span class="sxs-lookup"><span data-stu-id="ae29d-137">Distributed Redis cache</span></span>](#distributed-redis-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="ae29d-138">Cache de la mémoire distribuée</span><span class="sxs-lookup"><span data-stu-id="ae29d-138">Distributed Memory Cache</span></span>

<span data-ttu-id="ae29d-139">Le Cache distribué de mémoire (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) est une implémentation fournie par le framework de `IDistributedCache` qui stocke des éléments dans la mémoire.</span><span class="sxs-lookup"><span data-stu-id="ae29d-139">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of `IDistributedCache` that stores items in memory.</span></span> <span data-ttu-id="ae29d-140">Le Cache mémoire distribué n’est pas un cache distribué réel.</span><span class="sxs-lookup"><span data-stu-id="ae29d-140">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="ae29d-141">Éléments mis en cache sont stockés par l’instance d’application sur le serveur où l’application est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="ae29d-141">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="ae29d-142">Le Cache distribué de mémoire est une implémentation utile :</span><span class="sxs-lookup"><span data-stu-id="ae29d-142">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="ae29d-143">Dans le développement et les scénarios de test.</span><span class="sxs-lookup"><span data-stu-id="ae29d-143">In development and testing scenarios.</span></span>
* <span data-ttu-id="ae29d-144">Quand un serveur unique est utilisé dans la consommation de mémoire et de production n’est pas un problème.</span><span class="sxs-lookup"><span data-stu-id="ae29d-144">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="ae29d-145">Implémentation des extraits du Cache mémoire distribué mis en cache le stockage de données.</span><span class="sxs-lookup"><span data-stu-id="ae29d-145">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="ae29d-146">Il permet pour implémenter qu'un véritable distributed solution mise en cache dans le futures if plusieurs nœuds ou une tolérance de panne devient nécessaire.</span><span class="sxs-lookup"><span data-stu-id="ae29d-146">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="ae29d-147">L’exemple d’application utilise le Cache distribué de mémoire quand l’application est exécutée dans l’environnement de développement :</span><span class="sxs-lookup"><span data-stu-id="ae29d-147">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="ae29d-148">Cache distribué de SQL Server</span><span class="sxs-lookup"><span data-stu-id="ae29d-148">Distributed SQL Server Cache</span></span>

<span data-ttu-id="ae29d-149">L’implémentation de Cache distribué de SQL Server (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) permet au cache distribué à utiliser une base de données SQL Server comme magasin de sauvegarde.</span><span class="sxs-lookup"><span data-stu-id="ae29d-149">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="ae29d-150">Pour créer une table d’élément mis en cache de SQL Server dans une instance de SQL Server, vous pouvez utiliser le `sql-cache` outil.</span><span class="sxs-lookup"><span data-stu-id="ae29d-150">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="ae29d-151">L’outil crée une table avec le nom et le schéma que vous spécifiez.</span><span class="sxs-lookup"><span data-stu-id="ae29d-151">The tool creates a table with the name and schema that you specify.</span></span>

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="ae29d-152">Ajouter `SqlConfig.Tools` à la `<ItemGroup>` élément du fichier projet et exécutez `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="ae29d-152">Add `SqlConfig.Tools` to the `<ItemGroup>` element of the project file and run `dotnet restore`.</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools"
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

<span data-ttu-id="ae29d-153">Créer une table dans SQL Server en exécutant la `sql-cache create` commande.</span><span class="sxs-lookup"><span data-stu-id="ae29d-153">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="ae29d-154">Fournir l’instance de SQL Server (`Data Source`), base de données (`Initial Catalog`), schéma (par exemple, `dbo`) et le nom de la table (par exemple, `TestCache`) :</span><span class="sxs-lookup"><span data-stu-id="ae29d-154">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="ae29d-155">Un message est consigné pour indiquer que l’outil a réussi :</span><span class="sxs-lookup"><span data-stu-id="ae29d-155">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="ae29d-156">La table créée par le `sql-cache` outil présente le schéma suivant :</span><span class="sxs-lookup"><span data-stu-id="ae29d-156">The table created by the `sql-cache` tool has the following schema:</span></span>

![Table de Cache de SQL Server](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="ae29d-158">Une application doit manipuler les valeurs de cache à l’aide d’une instance de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, et non un <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span><span class="sxs-lookup"><span data-stu-id="ae29d-158">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="ae29d-159">L’exemple d’application implémente <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> dans un environnement non-développement :</span><span class="sxs-lookup"><span data-stu-id="ae29d-159">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_ConfigureServices&highlight=9-15)]

> [!NOTE]
> <span data-ttu-id="ae29d-160">Un <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (et éventuellement, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> et <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) sont généralement stockés en dehors du contrôle de code source (par exemple, stockés par le [Secret Manager](xref:security/app-secrets) ou dans *appsettings.json* / *appsettings. {Environment} .json* fichiers).</span><span class="sxs-lookup"><span data-stu-id="ae29d-160">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{Environment}.json* files).</span></span> <span data-ttu-id="ae29d-161">La chaîne de connexion peut contenir des informations d’identification qui doivent être conservées en dehors de systèmes de contrôle de source.</span><span class="sxs-lookup"><span data-stu-id="ae29d-161">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="ae29d-162">Cache Redis distribué</span><span class="sxs-lookup"><span data-stu-id="ae29d-162">Distributed Redis Cache</span></span>

<span data-ttu-id="ae29d-163">[Redis](https://redis.io/) est un magasin de données en mémoire open source, qui est souvent utilisé comme un cache distribué.</span><span class="sxs-lookup"><span data-stu-id="ae29d-163">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="ae29d-164">Vous pouvez utiliser Redis localement, et vous pouvez configurer un [Cache Redis Azure](https://azure.microsoft.com/services/cache/) pour une application ASP.NET Core hébergées Azure.</span><span class="sxs-lookup"><span data-stu-id="ae29d-164">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span> <span data-ttu-id="ae29d-165">Une application configure l’implémentation de cache à l’aide un <xref:Microsoft.Extensions.Caching.Redis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>) :</span><span class="sxs-lookup"><span data-stu-id="ae29d-165">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.Redis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span></span>

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

<span data-ttu-id="ae29d-166">Pour installer Redis sur votre ordinateur local :</span><span class="sxs-lookup"><span data-stu-id="ae29d-166">To install Redis on your local machine:</span></span>

* <span data-ttu-id="ae29d-167">Installer le [package Chocolatey Redis](https://chocolatey.org/packages/redis-64/).</span><span class="sxs-lookup"><span data-stu-id="ae29d-167">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
* <span data-ttu-id="ae29d-168">Exécutez `redis-server` à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="ae29d-168">Run `redis-server` from a command prompt.</span></span>

## <a name="use-the-distributed-cache"></a><span data-ttu-id="ae29d-169">Utiliser le cache distribué</span><span class="sxs-lookup"><span data-stu-id="ae29d-169">Use the distributed cache</span></span>

<span data-ttu-id="ae29d-170">Pour utiliser le <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> l’interface, demandez une instance de `IDistributedCache` à partir de n’importe quel constructeur dans l’application.</span><span class="sxs-lookup"><span data-stu-id="ae29d-170">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of `IDistributedCache` from any constructor in the app.</span></span> <span data-ttu-id="ae29d-171">L’instance est fournie par [l’injection de dépendance (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ae29d-171">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="ae29d-172">Lorsque l’application démarre, `IDistributedCache` est injecté dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="ae29d-172">When the app starts, `IDistributedCache` is injected into `Startup.Configure`.</span></span> <span data-ttu-id="ae29d-173">L’heure actuelle est mise en cache à l’aide de <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (pour plus d’informations, consultez [hôte Web : interface IApplicationLifetime](xref:fundamentals/host/web-host#iapplicationlifetime-interface)) :</span><span class="sxs-lookup"><span data-stu-id="ae29d-173">The current time is cached using <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (for more information, see [Web Host: IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

<span data-ttu-id="ae29d-174">L’exemple d’application injecte `IDistributedCache` dans le `IndexModel` pour une utilisation par la page d’Index.</span><span class="sxs-lookup"><span data-stu-id="ae29d-174">The sample app injects `IDistributedCache` into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="ae29d-175">Chaque fois que la page d’Index est chargée, le cache est activé pour la durée de mise en cache en `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="ae29d-175">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="ae29d-176">Si l’heure de mise en cache n’a pas expiré, l’heure est affichée.</span><span class="sxs-lookup"><span data-stu-id="ae29d-176">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="ae29d-177">Si 20 secondes se sont écoulées depuis la dernière fois que l’heure de mise en cache a accédé (la dernière fois cette page a été chargée), la page affiche *mis en cache délai expiré*.</span><span class="sxs-lookup"><span data-stu-id="ae29d-177">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="ae29d-178">Mettre immédiatement à jour l’heure de mise en cache à l’heure actuelle en sélectionnant le **réinitialiser le temps en cache** bouton.</span><span class="sxs-lookup"><span data-stu-id="ae29d-178">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="ae29d-179">Les déclencheurs de bouton la `OnPostResetCachedTime` méthode de gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="ae29d-179">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> <span data-ttu-id="ae29d-180">Il est inutile d’utiliser un Singleton ou une durée de vie limitée à un périmètre d'utilisation pour gérer des instances de `IDistributedCache` (et ceci au moins pour les implémentations intégrées).</span><span class="sxs-lookup"><span data-stu-id="ae29d-180">There's no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="ae29d-181">Vous pouvez également créer un `IDistributedCache` de l’instance où vous devrez peut-être un au lieu d’utiliser l’injection de dépendances, mais la création d’une instance dans le code peut rendre votre code plus difficile à tester et ne respecte pas la [principe des dépendances explicites](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span><span class="sxs-lookup"><span data-stu-id="ae29d-181">You can also create an `IDistributedCache` instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="ae29d-182">Recommandations</span><span class="sxs-lookup"><span data-stu-id="ae29d-182">Recommendations</span></span>

<span data-ttu-id="ae29d-183">Lorsque vous décidez quelle implémentation de `IDistributedCache` convient le mieux à votre application, considérez les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="ae29d-183">When deciding which implementation of `IDistributedCache` is best for your app, consider the following:</span></span>

* <span data-ttu-id="ae29d-184">Infrastructure existante</span><span class="sxs-lookup"><span data-stu-id="ae29d-184">Existing infrastructure</span></span>
* <span data-ttu-id="ae29d-185">Exigences de performances</span><span class="sxs-lookup"><span data-stu-id="ae29d-185">Performance requirements</span></span>
* <span data-ttu-id="ae29d-186">Coût</span><span class="sxs-lookup"><span data-stu-id="ae29d-186">Cost</span></span>
* <span data-ttu-id="ae29d-187">Expérience de l’équipe</span><span class="sxs-lookup"><span data-stu-id="ae29d-187">Team experience</span></span>

<span data-ttu-id="ae29d-188">Solutions de mise en cache s’appuient généralement sur le stockage en mémoire pour fournir la récupération rapide des données mises en cache, mais la mémoire est une ressource limitée et coûteux pour développer.</span><span class="sxs-lookup"><span data-stu-id="ae29d-188">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="ae29d-189">Magasin uniquement les données couramment utilisées dans un cache.</span><span class="sxs-lookup"><span data-stu-id="ae29d-189">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="ae29d-190">En règle générale, un cache Redis fournit un débit plus élevé et une latence plus faible qu’un cache de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ae29d-190">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="ae29d-191">Toutefois, il est généralement requis pour déterminer les caractéristiques de performances des stratégies de mise en cache de tests de performances.</span><span class="sxs-lookup"><span data-stu-id="ae29d-191">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="ae29d-192">Lorsque SQL Server est utilisé comme magasin de stockage de cache distribué, utiliser la même base de données pour le cache et stockage de données ordinaire de l’application et de récupération peut affecter négativement les performances des deux.</span><span class="sxs-lookup"><span data-stu-id="ae29d-192">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="ae29d-193">Nous vous recommandons d’utiliser une instance de SQL Server dédiée pour le magasin de stockage de cache distribué.</span><span class="sxs-lookup"><span data-stu-id="ae29d-193">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ae29d-194">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ae29d-194">Additional resources</span></span>

* [<span data-ttu-id="ae29d-195">Redis Cache sur Azure</span><span class="sxs-lookup"><span data-stu-id="ae29d-195">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="ae29d-196">Base de données SQL sur Azure</span><span class="sxs-lookup"><span data-stu-id="ae29d-196">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* <span data-ttu-id="ae29d-197">[ASP.NET Core fournisseur IDistributedCache pour NCache dans les batteries de serveurs Web](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache sur GitHub](https://github.com/Alachisoft/NCache))</span><span class="sxs-lookup"><span data-stu-id="ae29d-197">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
