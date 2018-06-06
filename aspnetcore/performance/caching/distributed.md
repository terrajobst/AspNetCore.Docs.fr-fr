---
title: Travailler avec un cache distribué dans ASP.NET Core
author: ardalis
description: Découvrez comment utiliser la mise en cache distribué afin d’améliorer les performances et l'évolutivité des applications ASP.NET Core, en particulier lorsqu'elles sont hébergées dans un environnement cloud ou dans une batterie de serveurs.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/distributed
ms.openlocfilehash: 6c595572641604d241c0c8f702d4f392afe34f71
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734456"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a><span data-ttu-id="5105a-103">Travailler avec un cache distribué dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5105a-103">Work with a distributed cache in ASP.NET Core</span></span>

<span data-ttu-id="5105a-104">Par [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="5105a-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="5105a-105">Les caches distribués peuvent améliorer les performances et la scalabilité des applications ASP.NET Core, en particulier lorsqu'ils sont hébergés dans un environnement cloud ou dans une ferme de serveurs.</span><span class="sxs-lookup"><span data-stu-id="5105a-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in a cloud or server farm environment.</span></span> <span data-ttu-id="5105a-106">Cet article explique comment utiliser les abstractions et les implémentations de cache distribué intégrées dans ASP.NET Core.
</span><span class="sxs-lookup"><span data-stu-id="5105a-106">This article explains how to work with ASP.NET Core's built-in distributed cache abstractions and implementations.</span></span>

<span data-ttu-id="5105a-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5105a-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="5105a-108">Qu’est un cache distribué</span><span class="sxs-lookup"><span data-stu-id="5105a-108">What is a distributed cache</span></span>

<span data-ttu-id="5105a-109">Un cache distribué est partagé par plusieurs serveurs d’application (consultez [principes de base du Cache](memory.md#caching-basics)).</span><span class="sxs-lookup"><span data-stu-id="5105a-109">A distributed cache is shared by multiple app servers (see [Cache Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="5105a-110">Les informations contenues dans le cache ne sont pas isolées dans la mémoire de chaque serveur de site web, mais les données mises en cache sont disponibles pour tous les serveurs de l’application.</span><span class="sxs-lookup"><span data-stu-id="5105a-110">The information in the cache isn't stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="5105a-111">Cela présente plusieurs avantages :</span><span class="sxs-lookup"><span data-stu-id="5105a-111">This provides several advantages:</span></span>

1. <span data-ttu-id="5105a-112">Les données mises en cache sont cohérentes sur tous les serveurs web.</span><span class="sxs-lookup"><span data-stu-id="5105a-112">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="5105a-113">Les utilisateurs ne voient pas des résultats différents selon le serveur web qui gère leur demande</span><span class="sxs-lookup"><span data-stu-id="5105a-113">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="5105a-114">Les données mises en cache survivent au redémarrage du serveur web et aux déploiements.</span><span class="sxs-lookup"><span data-stu-id="5105a-114">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="5105a-115">Chaque serveur web peut individuellement être supprimé ou ajouté sans impact sur le cache.</span><span class="sxs-lookup"><span data-stu-id="5105a-115">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="5105a-116">Il y a moins de requêtes effectuées sur la source de données (que s'il y avait plusieurs caches en mémoire ou pas de mise en cache du tout).</span><span class="sxs-lookup"><span data-stu-id="5105a-116">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="5105a-117">Si vous utilisez un cache distribué de SQL Server, certains de ces avantages sont réels uniquement si une instance de base de données distincte de celle pour les données de l'application est utilisée pour le cache.</span><span class="sxs-lookup"><span data-stu-id="5105a-117">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="5105a-118">Comme n’importe quel type de cache, un cache distribué peut considérablement améliorer la réactivité d’une application, car en général, il est beaucoup plus rapide de récupérer les données à partir d'un cache qu’à partir d’une base de données relationnelle (ou d'un service web).</span><span class="sxs-lookup"><span data-stu-id="5105a-118">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="5105a-119">La configuration d'un cache est spécifique à l’implémentation.</span><span class="sxs-lookup"><span data-stu-id="5105a-119">Cache configuration is implementation specific.</span></span> <span data-ttu-id="5105a-120">Cet article décrit comment configurer des caches distribués Redis et SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5105a-120">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="5105a-121">Indépendamment de l’implémentation sélectionnée, l’application interagit avec le cache à l’aide d’une interface commune `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="5105a-121">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="5105a-122">L’Interface IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="5105a-122">The IDistributedCache Interface</span></span>

<span data-ttu-id="5105a-123">L'interface `IDistributedCache` inclut des méthodes synchrones et asynchrones.</span><span class="sxs-lookup"><span data-stu-id="5105a-123">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="5105a-124">L’interface permet que des éléments soient ajoutés, récupérés ou supprimés à partir de l’implémentation de cache distribué.</span><span class="sxs-lookup"><span data-stu-id="5105a-124">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="5105a-125">L'interface `IDistributedCache` comprend les méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="5105a-125">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="5105a-126">**Get, GetAsync**</span><span class="sxs-lookup"><span data-stu-id="5105a-126">**Get, GetAsync**</span></span>

<span data-ttu-id="5105a-127">Prend une clé de type string et récupère un élément mis en cache sous la forme d'un `byte[]` si trouvé dans le cache.</span><span class="sxs-lookup"><span data-stu-id="5105a-127">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="5105a-128">**Set, SetAsync**</span><span class="sxs-lookup"><span data-stu-id="5105a-128">**Set, SetAsync**</span></span>

<span data-ttu-id="5105a-129">Ajoute un élément (comme `byte[]`) dans le cache à l’aide d’une clé de type string.</span><span class="sxs-lookup"><span data-stu-id="5105a-129">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="5105a-130">**Actualisation, RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="5105a-130">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="5105a-131">Actualise un élément dans le cache en fonction de sa clé, la réinitialisation de son délai d’expiration est décalée (si défini).</span><span class="sxs-lookup"><span data-stu-id="5105a-131">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="5105a-132">**Supprimer, RemoveAsync**</span><span class="sxs-lookup"><span data-stu-id="5105a-132">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="5105a-133">Supprime une entrée de cache en fonction de sa clé.</span><span class="sxs-lookup"><span data-stu-id="5105a-133">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="5105a-134">Pour utiliser l'interface `IDistributedCache` :</span><span class="sxs-lookup"><span data-stu-id="5105a-134">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="5105a-135">Ajouter les packages NuGet nécessaires à votre fichier projet.</span><span class="sxs-lookup"><span data-stu-id="5105a-135">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="5105a-136">Configurer l’implémentation spécifique de `IDistributedCache` dans la méthode `ConfigureServices` de votre classe `Startup` et l’ajouter au conteneur.</span><span class="sxs-lookup"><span data-stu-id="5105a-136">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="5105a-137">À partir de l'[intergiciel (middleware)](xref:fundamentals/middleware/index) de l’application ou des classes des contrôleurs MVC, demandez une instance de `IDistributedCache` à partir du constructeur.</span><span class="sxs-lookup"><span data-stu-id="5105a-137">From the app's [Middleware](xref:fundamentals/middleware/index) or MVC controller classes, request an instance of `IDistributedCache` from the constructor.</span></span> <span data-ttu-id="5105a-138">L’instance sera fournie par l'[Injection de dépendance](../../fundamentals/dependency-injection.md) (DI).</span><span class="sxs-lookup"><span data-stu-id="5105a-138">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="5105a-139">Il est inutile d’utiliser un Singleton ou une durée de vie limitée à un périmètre d'utilisation pour gérer des instances de `IDistributedCache` (et ceci au moins pour les implémentations intégrées).</span><span class="sxs-lookup"><span data-stu-id="5105a-139">There's no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="5105a-140">Vous pouvez également créer explicitement une instance à chaque fois que vous en avez besoin (au lieu d’utiliser l'[Injection de dépendance](../../fundamentals/dependency-injection.md)), mais cela peut rendre votre code plus difficile à tester et ne respecte pas le [principe de dépendances explicites](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="5105a-140">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="5105a-141">L’exemple suivant montre comment utiliser une instance de `IDistributedCache` dans un composant d’intergiciel (middleware) simple :</span><span class="sxs-lookup"><span data-stu-id="5105a-141">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

<span data-ttu-id="5105a-142">Dans le code ci-dessus, la valeur mise en cache est lue, mais jamais écrite.</span><span class="sxs-lookup"><span data-stu-id="5105a-142">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="5105a-143">Dans cet exemple, la valeur est définie uniquement lorsqu’un serveur démarre et elle ne change pas.</span><span class="sxs-lookup"><span data-stu-id="5105a-143">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="5105a-144">Dans un scénario multiserveur, le serveur démarré le plus récemment remplace toutes les valeurs précédentes qui ont été définies par d’autres serveurs.</span><span class="sxs-lookup"><span data-stu-id="5105a-144">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="5105a-145">Les méthodes `Get` et `Set` utilisent le type `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="5105a-145">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="5105a-146">Par conséquent, la valeur de type string doit être convertie à l’aide des méthodes `Encoding.UTF8.GetString` (pour `Get`) et `Encoding.UTF8.GetBytes` (pour `Set`).</span><span class="sxs-lookup"><span data-stu-id="5105a-146">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="5105a-147">Le code suivant du fichier *Startup.cs* affiche la valeur :</span><span class="sxs-lookup"><span data-stu-id="5105a-147">The following code from *Startup.cs* shows the value being set:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

> [!NOTE]
> <span data-ttu-id="5105a-148">Étant donné que `IDistributedCache` est configuré dans la méthode `ConfigureServices`, elle est disponible pour la méthode `Configure` en tant que paramètre. Afin de pouvoir injecter sous forme de dépendance l'instance configurée, ajoutez-la en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="5105a-148">Since `IDistributedCache` is configured in the `ConfigureServices` method, it's available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="5105a-149">Pour permettre à l’instance configurée être fourni via DI, ajoutez-le en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="5105a-149">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="5105a-150">Utiliser un cache distribué Redis</span><span class="sxs-lookup"><span data-stu-id="5105a-150">Using a Redis distributed cache</span></span>

<span data-ttu-id="5105a-151">[Redis](https://redis.io/) est un magasin de données en mémoire open source, qui est souvent utilisé comme un cache distribué.</span><span class="sxs-lookup"><span data-stu-id="5105a-151">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="5105a-152">Vous pouvez l’utiliser localement, et vous pouvez configurer un [Cache Redis Azure](https://azure.microsoft.com/services/cache/) pour vos applications ASP.NET Core hébergées sur Azure.</span><span class="sxs-lookup"><span data-stu-id="5105a-152">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="5105a-153">Votre application ASP.NET Core configure l’implémentation de cache via une instance de type `RedisDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="5105a-153">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="5105a-154">Vous configurez l’implémentation de Redis dans `ConfigureServices` et y accédez dans le code de votre application en demandant une instance de `IDistributedCache` (voir le code ci-dessus).</span><span class="sxs-lookup"><span data-stu-id="5105a-154">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="5105a-155">Dans l’exemple de code, une implémentation de type `RedisCache` est utilisée lorsque le serveur est configuré pour un environnement `Staging`.</span><span class="sxs-lookup"><span data-stu-id="5105a-155">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="5105a-156">Par conséquent, la méthode `ConfigureStagingServices` configure le `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="5105a-156">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="5105a-157">Pour installer Redis sur votre ordinateur local, installez le package chocolatey [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) et exécutez `redis-server` à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="5105a-157">To install Redis on your local machine, install the chocolatey package [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="5105a-158">Utiliser un cache distribué SQL Server</span><span class="sxs-lookup"><span data-stu-id="5105a-158">Using a SQL Server distributed cache</span></span>

<span data-ttu-id="5105a-159">L’implémentation proposée par SqlServerCache permet l'utilisation d'une base de données SQL Server comme magasin de sauvegarde du cache distribué.</span><span class="sxs-lookup"><span data-stu-id="5105a-159">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="5105a-160">Pour créer une table SQL Server, vous pouvez utiliser l'outil sql-cache, l’outil crée une table avec le nom et le schéma que vous spécifiez.</span><span class="sxs-lookup"><span data-stu-id="5105a-160">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="5105a-161">Ajouter `SqlConfig.Tools` à la `<ItemGroup>` élément du fichier de projet et exécuter `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="5105a-161">Add `SqlConfig.Tools` to the `<ItemGroup>` element of the project file and run `dotnet restore`.</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

<span data-ttu-id="5105a-162">Test SqlConfig.Tools en exécutant la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="5105a-162">Test SqlConfig.Tools by running the following command:</span></span>

```console
dotnet sql-cache create --help
```

<span data-ttu-id="5105a-163">SqlConfig.Tools affiche l’utilisation, options et l’aide de la commande.</span><span class="sxs-lookup"><span data-stu-id="5105a-163">SqlConfig.Tools displays usage, options, and command help.</span></span>

<span data-ttu-id="5105a-164">Créer une table dans SQL Server en exécutant le `sql-cache create` commande :</span><span class="sxs-lookup"><span data-stu-id="5105a-164">Create a table in SQL Server by running the `sql-cache create` command :</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

<span data-ttu-id="5105a-165">Le tableau présente le schéma suivant :</span><span class="sxs-lookup"><span data-stu-id="5105a-165">The created table has the following schema:</span></span>

![Table de Cache de SQL Server](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="5105a-167">Comme toutes les implémentations de cache, votre application doit obtenir et définir des valeurs de cache à l’aide d’une instance de `IDistributedCache`, et non de `SqlServerCache`. </span><span class="sxs-lookup"><span data-stu-id="5105a-167">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="5105a-168">L’exemple implémente `SqlServerCache` dans l’environnement de Production (par conséquent, il est configuré dans `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="5105a-168">The sample implements `SqlServerCache` in the Production environment (so it's configured in `ConfigureProductionServices`).</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> <span data-ttu-id="5105a-169">`ConnectionString` (et, éventuellement, `SchemaName` et `TableName`) doit généralement être stocké en dehors du contrôle de code source (par exemple, usersecrets.), car il peut contenir des informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="5105a-169">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="5105a-170">Recommandations</span><span class="sxs-lookup"><span data-stu-id="5105a-170">Recommendations</span></span>

<span data-ttu-id="5105a-171">Lorsque vous décidez quelle implémentation de `IDistributedCache` est adaptée à votre application, choisissez entre Redis et SQL Server en fonction de votre infrastructure existante et votre environnement, de vos exigences de performances et l'expérience de votre équipe.</span><span class="sxs-lookup"><span data-stu-id="5105a-171">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="5105a-172">Si votre équipe est plus familière avec Redis, Redis constitue un excellent choix.</span><span class="sxs-lookup"><span data-stu-id="5105a-172">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="5105a-173">Si votre équipe préfère SQL Server, vous pouvez être certain qu’elle appréciera également l’implémentation SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5105a-173">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="5105a-174">Notez qu’une solution de mise en cache traditionnelle stocke les données en mémoire qui permet la récupération rapide des données.</span><span class="sxs-lookup"><span data-stu-id="5105a-174">Note that a traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="5105a-175">Vous devez stocker les données couramment utilisées dans un cache et stocker la totalité des données dans un magasin persistant de back-end comme SQL Server ou le Stockage Azure.</span><span class="sxs-lookup"><span data-stu-id="5105a-175">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="5105a-176">Un cache redis est une solution de mise en cache qui vous donne un débit élevé et une faible latence par rapport au Cache SQL.</span><span class="sxs-lookup"><span data-stu-id="5105a-176">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5105a-177">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="5105a-177">Additional resources</span></span>

* [<span data-ttu-id="5105a-178">Cache dans Azure redis</span><span class="sxs-lookup"><span data-stu-id="5105a-178">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="5105a-179">Base de données SQL Azure</span><span class="sxs-lookup"><span data-stu-id="5105a-179">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* [<span data-ttu-id="5105a-180">Mettre en cache en mémoire</span><span class="sxs-lookup"><span data-stu-id="5105a-180">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="5105a-181">Détecter les modifications à l’aide de jetons de modification</span><span class="sxs-lookup"><span data-stu-id="5105a-181">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="5105a-182">Mise en cache des réponses</span><span class="sxs-lookup"><span data-stu-id="5105a-182">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="5105a-183">Intergiciel de mise en cache des réponses</span><span class="sxs-lookup"><span data-stu-id="5105a-183">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="5105a-184">Tag Helper de cache</span><span class="sxs-lookup"><span data-stu-id="5105a-184">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="5105a-185">Tag Helper de cache distribué</span><span class="sxs-lookup"><span data-stu-id="5105a-185">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
