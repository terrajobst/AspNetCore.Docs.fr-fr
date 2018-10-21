---
title: Travailler avec un cache distribué dans ASP.NET Core
author: ardalis
description: Découvrez comment utiliser la mise en cache distribué afin d’améliorer les performances et l'évolutivité des applications ASP.NET Core, en particulier lorsqu'elles sont hébergées dans un environnement cloud ou dans une batterie de serveurs.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
uid: performance/caching/distributed
ms.openlocfilehash: 85da734f3ae7bcf0936888edfb6ac91d4362eef2
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477474"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a><span data-ttu-id="d3d4f-103">Travailler avec un cache distribué dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d3d4f-103">Work with a distributed cache in ASP.NET Core</span></span>

<span data-ttu-id="d3d4f-104">Par [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="d3d4f-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="d3d4f-105">Les caches distribués peuvent améliorer les performances et l’évolutivité des applications ASP.NET Core, en particulier lorsque hébergé dans le cloud ou une batterie de serveurs.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in the cloud or a server farm.</span></span>

<span data-ttu-id="d3d4f-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d3d4f-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="d3d4f-107">Qu’est un cache distribué</span><span class="sxs-lookup"><span data-stu-id="d3d4f-107">What is a distributed cache</span></span>

<span data-ttu-id="d3d4f-108">Un cache distribué est partagé par plusieurs serveurs d’applications (consultez [principes fondamentaux du Cache](memory.md#caching-basics)).</span><span class="sxs-lookup"><span data-stu-id="d3d4f-108">A distributed cache is shared by multiple app servers (see [Cache Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="d3d4f-109">Les informations contenues dans le cache ne sont pas isolées dans la mémoire de chaque serveur de site web, mais les données mises en cache sont disponibles pour tous les serveurs de l’application.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-109">The information in the cache isn't stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="d3d4f-110">Cela présente plusieurs avantages :</span><span class="sxs-lookup"><span data-stu-id="d3d4f-110">This provides several advantages:</span></span>

1. <span data-ttu-id="d3d4f-111">Les données mises en cache sont cohérentes sur tous les serveurs web.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-111">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="d3d4f-112">Les utilisateurs ne voient pas des résultats différents selon le serveur web qui gère leur demande</span><span class="sxs-lookup"><span data-stu-id="d3d4f-112">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="d3d4f-113">Les données mises en cache survivent au redémarrage du serveur web et aux déploiements.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-113">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="d3d4f-114">Chaque serveur web peut individuellement être supprimé ou ajouté sans impact sur le cache.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-114">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="d3d4f-115">Il y a moins de requêtes effectuées sur la source de données (que s'il y avait plusieurs caches en mémoire ou pas de mise en cache du tout).</span><span class="sxs-lookup"><span data-stu-id="d3d4f-115">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="d3d4f-116">Si vous utilisez un cache distribué de SQL Server, certains de ces avantages sont réels uniquement si une instance de base de données distincte de celle pour les données de l'application est utilisée pour le cache.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-116">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="d3d4f-117">Comme n’importe quel type de cache, un cache distribué peut considérablement améliorer la réactivité d’une application, car en général, il est beaucoup plus rapide de récupérer les données à partir d'un cache qu’à partir d’une base de données relationnelle (ou d'un service web).</span><span class="sxs-lookup"><span data-stu-id="d3d4f-117">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="d3d4f-118">La configuration d'un cache est spécifique à l’implémentation.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-118">Cache configuration is implementation specific.</span></span> <span data-ttu-id="d3d4f-119">Cet article décrit comment configurer des caches distribués Redis et SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-119">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="d3d4f-120">Indépendamment de l’implémentation sélectionnée, l’application interagit avec le cache à l’aide d’une interface commune `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-120">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="d3d4f-121">L’Interface IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="d3d4f-121">The IDistributedCache Interface</span></span>

<span data-ttu-id="d3d4f-122">L'interface `IDistributedCache` inclut des méthodes synchrones et asynchrones.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-122">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="d3d4f-123">L’interface permet que des éléments soient ajoutés, récupérés ou supprimés à partir de l’implémentation de cache distribué.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-123">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="d3d4f-124">L'interface `IDistributedCache` comprend les méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="d3d4f-124">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="d3d4f-125">**Get, GetAsync**</span><span class="sxs-lookup"><span data-stu-id="d3d4f-125">**Get, GetAsync**</span></span>

<span data-ttu-id="d3d4f-126">Prend une clé de type string et récupère un élément mis en cache sous la forme d'un `byte[]` si trouvé dans le cache.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-126">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="d3d4f-127">**Set, SetAsync**</span><span class="sxs-lookup"><span data-stu-id="d3d4f-127">**Set, SetAsync**</span></span>

<span data-ttu-id="d3d4f-128">Ajoute un élément (comme `byte[]`) dans le cache à l’aide d’une clé de type string.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-128">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="d3d4f-129">**Actualisation, RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="d3d4f-129">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="d3d4f-130">Actualise un élément dans le cache en fonction de sa clé, la réinitialisation de son délai d’expiration est décalée (si défini).</span><span class="sxs-lookup"><span data-stu-id="d3d4f-130">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="d3d4f-131">**Remove, RemoveAsync**</span><span class="sxs-lookup"><span data-stu-id="d3d4f-131">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="d3d4f-132">Supprime une entrée de cache en fonction de sa clé.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-132">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="d3d4f-133">Pour utiliser l'interface `IDistributedCache` :</span><span class="sxs-lookup"><span data-stu-id="d3d4f-133">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="d3d4f-134">Ajoutez les packages NuGet nécessaires à votre fichier projet.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-134">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="d3d4f-135">Configurer l’implémentation spécifique de `IDistributedCache` dans la méthode `ConfigureServices` de votre classe `Startup` et l’ajouter au conteneur.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-135">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="d3d4f-136">À partir de l'[intergiciel (middleware)](xref:fundamentals/middleware/index) de l’application ou des classes des contrôleurs MVC, demandez une instance de `IDistributedCache` à partir du constructeur.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-136">From the app's [Middleware](xref:fundamentals/middleware/index) or MVC controller classes, request an instance of `IDistributedCache` from the constructor.</span></span> <span data-ttu-id="d3d4f-137">L’instance sera fournie par l'[Injection de dépendance](../../fundamentals/dependency-injection.md) (DI).</span><span class="sxs-lookup"><span data-stu-id="d3d4f-137">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="d3d4f-138">Il est inutile d’utiliser un Singleton ou une durée de vie limitée à un périmètre d'utilisation pour gérer des instances de `IDistributedCache` (et ceci au moins pour les implémentations intégrées).</span><span class="sxs-lookup"><span data-stu-id="d3d4f-138">There's no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="d3d4f-139">Vous pouvez également créer explicitement une instance à chaque fois que vous en avez besoin (au lieu d’utiliser l'[Injection de dépendance](../../fundamentals/dependency-injection.md)), mais cela peut rendre votre code plus difficile à tester et ne respecte pas le [principe de dépendances explicites](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="d3d4f-139">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="d3d4f-140">L’exemple suivant montre comment utiliser une instance de `IDistributedCache` dans un composant d’intergiciel (middleware) simple :</span><span class="sxs-lookup"><span data-stu-id="d3d4f-140">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

<span data-ttu-id="d3d4f-141">Dans le code ci-dessus, la valeur mise en cache est lue, mais jamais écrite.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-141">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="d3d4f-142">Dans cet exemple, la valeur est définie uniquement lorsqu’un serveur démarre et elle ne change pas.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-142">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="d3d4f-143">Dans un scénario multiserveur, le serveur démarré le plus récemment remplace toutes les valeurs précédentes qui ont été définies par d’autres serveurs.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-143">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="d3d4f-144">Les méthodes `Get` et `Set` utilisent le type `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-144">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="d3d4f-145">Par conséquent, la valeur de type string doit être convertie à l’aide des méthodes `Encoding.UTF8.GetString` (pour `Get`) et `Encoding.UTF8.GetBytes` (pour `Set`).</span><span class="sxs-lookup"><span data-stu-id="d3d4f-145">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="d3d4f-146">Le code suivant du fichier *Startup.cs* affiche la valeur :</span><span class="sxs-lookup"><span data-stu-id="d3d4f-146">The following code from *Startup.cs* shows the value being set:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

<span data-ttu-id="d3d4f-147">Étant donné que `IDistributedCache` est configuré dans la méthode `ConfigureServices`, elle est disponible pour la méthode `Configure` en tant que paramètre. Afin de pouvoir injecter sous forme de dépendance l'instance configurée, ajoutez-la en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-147">Since `IDistributedCache` is configured in the `ConfigureServices` method, it's available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="d3d4f-148">Ajouter en tant que paramètre permettra l’instance configurée être fourni via l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-148">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="d3d4f-149">Utiliser un cache distribué Redis</span><span class="sxs-lookup"><span data-stu-id="d3d4f-149">Using a Redis distributed cache</span></span>

<span data-ttu-id="d3d4f-150">[Redis](https://redis.io/) est un magasin de données en mémoire open source, qui est souvent utilisé comme un cache distribué.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-150">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="d3d4f-151">Vous pouvez l’utiliser localement, et vous pouvez configurer un [Cache Redis Azure](https://azure.microsoft.com/services/cache/) pour vos applications ASP.NET Core hébergées sur Azure.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-151">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="d3d4f-152">Votre application ASP.NET Core configure l’implémentation de cache via une instance de type `RedisDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-152">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="d3d4f-153">Le cache Redis requiert [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis/)</span><span class="sxs-lookup"><span data-stu-id="d3d4f-153">The Redis cache requires [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis/)</span></span>

<span data-ttu-id="d3d4f-154">Vous configurez l’implémentation de Redis dans `ConfigureServices` et y accédez dans le code de votre application en demandant une instance de `IDistributedCache` (voir le code ci-dessus).</span><span class="sxs-lookup"><span data-stu-id="d3d4f-154">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="d3d4f-155">Dans l’exemple de code, une implémentation de type `RedisCache` est utilisée lorsque le serveur est configuré pour un environnement `Staging`.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-155">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="d3d4f-156">Par conséquent, la méthode `ConfigureStagingServices` configure le `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="d3d4f-156">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

<span data-ttu-id="d3d4f-157">Pour installer Redis sur votre ordinateur local, installez le package chocolatey [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) et exécutez `redis-server` à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-157">To install Redis on your local machine, install the chocolatey package [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="d3d4f-158">Utiliser un cache distribué SQL Server</span><span class="sxs-lookup"><span data-stu-id="d3d4f-158">Using a SQL Server distributed cache</span></span>

<span data-ttu-id="d3d4f-159">L’implémentation proposée par SqlServerCache permet l'utilisation d'une base de données SQL Server comme magasin de sauvegarde du cache distribué.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-159">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="d3d4f-160">Pour créer une table SQL Server, vous pouvez utiliser l'outil sql-cache, l’outil crée une table avec le nom et le schéma que vous spécifiez.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-160">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="d3d4f-161">Ajouter `SqlConfig.Tools` à la `<ItemGroup>` élément du fichier projet et exécutez `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-161">Add `SqlConfig.Tools` to the `<ItemGroup>` element of the project file and run `dotnet restore`.</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

<span data-ttu-id="d3d4f-162">Testez SqlConfig.Tools en exécutant la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="d3d4f-162">Test SqlConfig.Tools by running the following command:</span></span>

```console
dotnet sql-cache create --help
```

<span data-ttu-id="d3d4f-163">SqlConfig.Tools affiche l’utilisation, options et l’aide de la commande.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-163">SqlConfig.Tools displays usage, options, and command help.</span></span>

<span data-ttu-id="d3d4f-164">Créer une table dans SQL Server en exécutant la `sql-cache create` commande :</span><span class="sxs-lookup"><span data-stu-id="d3d4f-164">Create a table in SQL Server by running the `sql-cache create` command :</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

<span data-ttu-id="d3d4f-165">La table créée présente le schéma suivant :</span><span class="sxs-lookup"><span data-stu-id="d3d4f-165">The created table has the following schema:</span></span>

![Table de Cache de SQL Server](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="d3d4f-167">Comme toutes les implémentations de cache, votre application doit obtenir et définir des valeurs de cache à l’aide d’une instance de `IDistributedCache`, et non de `SqlServerCache`. </span><span class="sxs-lookup"><span data-stu-id="d3d4f-167">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="d3d4f-168">L’exemple implémente `SqlServerCache` dans l’environnement de Production (par conséquent, il est configuré dans `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="d3d4f-168">The sample implements `SqlServerCache` in the Production environment (so it's configured in `ConfigureProductionServices`).</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> <span data-ttu-id="d3d4f-169">`ConnectionString` (et, éventuellement, `SchemaName` et `TableName`) doit généralement être stocké en dehors du contrôle de code source (par exemple, usersecrets.), car il peut contenir des informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-169">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="d3d4f-170">Recommandations</span><span class="sxs-lookup"><span data-stu-id="d3d4f-170">Recommendations</span></span>

<span data-ttu-id="d3d4f-171">Lorsque vous décidez quelle implémentation de `IDistributedCache` est adaptée à votre application, choisissez entre Redis et SQL Server en fonction de votre infrastructure existante et votre environnement, de vos exigences de performances et l'expérience de votre équipe.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-171">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="d3d4f-172">Si votre équipe est plus familière avec Redis, Redis constitue un excellent choix.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-172">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="d3d4f-173">Si votre équipe préfère SQL Server, vous pouvez être certain qu’elle appréciera également l’implémentation SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-173">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="d3d4f-174">Notez qu’une solution de mise en cache traditionnelle stocke les données en mémoire qui permet la récupération rapide des données.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-174">Note that a traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="d3d4f-175">Vous devez stocker les données couramment utilisées dans un cache et stocker la totalité des données dans un magasin persistant de back-end comme SQL Server ou le Stockage Azure.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-175">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="d3d4f-176">Un cache redis est une solution de mise en cache qui vous donne un débit élevé et une faible latence par rapport au Cache SQL.</span><span class="sxs-lookup"><span data-stu-id="d3d4f-176">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d3d4f-177">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d3d4f-177">Additional resources</span></span>

* [<span data-ttu-id="d3d4f-178">Redis Cache sur Azure</span><span class="sxs-lookup"><span data-stu-id="d3d4f-178">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="d3d4f-179">Base de données SQL sur Azure</span><span class="sxs-lookup"><span data-stu-id="d3d4f-179">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
