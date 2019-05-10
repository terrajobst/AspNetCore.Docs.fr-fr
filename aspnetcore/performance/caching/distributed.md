---
title: Mise en cache dans ASP.NET Core distribuée
author: guardrex
description: Découvrez comment utiliser un cache d’ASP.NET Core est distribué pour améliorer les performances des applications et l’évolutivité, en particulier dans un environnement de batterie de serveurs cloud ou un serveur.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/30/2019
uid: performance/caching/distributed
ms.openlocfilehash: 871e27c068de3a465cedb517418a35a87f992300
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65085835"
---
# <a name="distributed-caching-in-aspnet-core"></a>Mise en cache dans ASP.NET Core distribuée

Par [Luke Latham](https://github.com/guardrex) et [Steve Smith](https://ardalis.com/)

Un cache distribué est un cache partagé par plusieurs serveurs d’applications, généralement gérées comme un service externe pour les serveurs d’application qui y accèdent. Un cache distribué peut améliorer les performances et l’évolutivité d’une application ASP.NET Core, en particulier lorsque l’application est hébergée par un service cloud ou une batterie de serveurs.

Un cache distribué présente plusieurs avantages sur les autres scénarios de mise en cache où les données mises en cache sont stockées sur les serveurs d’applications individuelles.

Lorsque les données mises en cache sont distribuées, les données :

* Est *cohérente* (cohérent) entre les requêtes vers plusieurs serveurs.
* Persiste lors des redémarrages de serveur et des déploiements d’applications.
* N’utilise pas la mémoire locale.

Configuration de cache distribué est spécifique à l’implémentation. Cet article décrit comment configurer SQL Server et les caches distribués Redis. Les implémentations de tiers sont également disponibles, comme [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache sur GitHub](https://github.com/Alachisoft/NCache)). Quel que soit l’implémentation qui est sélectionnée, l’application interagit avec le cache à l’aide de la <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prérequis

::: moniker range=">= aspnetcore-2.2"

Pour utiliser un serveur SQL Server distributed cache, référence le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app) ou ajouter une référence de package à la [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.

Pour utiliser un Redis distributed cache, référence le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app) et ajouter une référence de package à la [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package. Le package Redis n’est pas inclus dans le `Microsoft.AspNetCore.App` du package, que vous devez référencer le package Redis séparément dans votre fichier projet.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Pour utiliser un serveur SQL Server distributed cache, référence le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app) ou ajouter une référence de package à la [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.

Pour utiliser un Redis distributed cache, référence le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app) et ajouter une référence de package à la [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package. Le package Redis n’est pas inclus dans le `Microsoft.AspNetCore.App` du package, que vous devez référencer le package Redis séparément dans votre fichier projet.

::: moniker-end

## <a name="idistributedcache-interface"></a>Interface de IDistributedCache

Le <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface fournit les méthodes suivantes pour manipuler des éléments dans l’implémentation de cache distribué :

* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Accepte une clé de chaîne et récupère un élément mis en cache comme un `byte[]` tableau si trouvé dans le cache.
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Ajoute un élément (comme `byte[]` tableau) dans le cache à l’aide d’une clé de chaîne.
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Actualise un élément dans le cache en fonction de sa clé, la réinitialisation de son délai d’expiration décalée (le cas échéant).
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Supprime un élément de cache en fonction de sa clé de chaîne.

## <a name="establish-distributed-caching-services"></a>Établissez des services de mise en cache distribuées

Inscrire une implémentation de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> dans `Startup.ConfigureServices`. Les implémentations fournis par le Framework décrites dans cette rubrique sont les suivantes :

* [Cache de la mémoire distribuée](#distributed-memory-cache)
* [Cache distribué de SQL Server](#distributed-sql-server-cache)
* [Cache Redis distribué](#distributed-redis-cache)

### <a name="distributed-memory-cache"></a>Cache de la mémoire distribuée

Le Cache distribué de mémoire (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) est une implémentation fournie par le framework de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> qui stocke des éléments dans la mémoire. Le Cache mémoire distribué n’est pas un cache distribué réel. Éléments mis en cache sont stockés par l’instance d’application sur le serveur où l’application est en cours d’exécution.

Le Cache distribué de mémoire est une implémentation utile :

* Dans le développement et les scénarios de test.
* Quand un serveur unique est utilisé dans la consommation de mémoire et de production n’est pas un problème. Implémentation des extraits du Cache mémoire distribué mis en cache le stockage de données. Il permet pour implémenter qu'un véritable distributed solution mise en cache dans le futures if plusieurs nœuds ou une tolérance de panne devient nécessaire.

L’exemple d’application utilise le Cache distribué de mémoire quand l’application est exécutée dans l’environnement de développement dans `Startup.ConfigureServices`:

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

### <a name="distributed-sql-server-cache"></a>Cache distribué de SQL Server

L’implémentation de Cache distribué de SQL Server (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) permet au cache distribué à utiliser une base de données SQL Server comme magasin de sauvegarde. Pour créer une table d’élément mis en cache de SQL Server dans une instance de SQL Server, vous pouvez utiliser le `sql-cache` outil. L’outil crée une table avec le nom et le schéma que vous spécifiez.

Créer une table dans SQL Server en exécutant la commande `sql-cache create` : Fournir l’instance de SQL Server (`Data Source`), base de données (`Initial Catalog`), schéma (par exemple, `dbo`) et le nom de la table (par exemple, `TestCache`) :

```console
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

Un message est consigné pour indiquer que l’outil a réussi :

```console
Table and index were created successfully.
```

La table créée par le `sql-cache` outil présente le schéma suivant :

![Table de Cache de SQL Server](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> Une application doit manipuler les valeurs de cache à l’aide d’une instance de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, et non un <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.

L’exemple d’application implémente <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> dans un environnement non liées au développement dans `Startup.ConfigureServices`:

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

> [!NOTE]
> Un <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (et éventuellement, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> et <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) sont généralement stockés en dehors du contrôle de code source (par exemple, stockés par le [Secret Manager](xref:security/app-secrets) ou dans *appsettings.json* / *appsettings. {ENVIRONMENT} .json* fichiers). La chaîne de connexion peut contenir des informations d’identification qui doivent être conservées en dehors de systèmes de contrôle de source.

### <a name="distributed-redis-cache"></a>Cache Redis distribué

[Redis](https://redis.io/) est un magasin de données en mémoire open source, qui est souvent utilisé comme un cache distribué. Vous pouvez utiliser Redis localement, et vous pouvez configurer un [Cache Redis Azure](https://azure.microsoft.com/services/cache/) pour une application ASP.NET Core hébergées Azure.

::: moniker range=">= aspnetcore-2.2"

Une application configure l’implémentation de cache à l’aide un <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) dans un environnement non liées au développement dans `Startup.ConfigureServices`:

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Une application configure l’implémentation de cache à l’aide un <xref:Microsoft.Extensions.Caching.Redis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>) :

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

::: moniker-end

Pour installer Redis sur votre ordinateur local :

* Installer le [package Chocolatey Redis](https://chocolatey.org/packages/redis-64/).
* Exécutez `redis-server` à partir d’une invite de commandes.

## <a name="use-the-distributed-cache"></a>Utiliser le cache distribué

Pour utiliser le <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> l’interface, demandez une instance de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> à partir de n’importe quel constructeur dans l’application. L’instance est fournie par [l’injection de dépendance (DI)](xref:fundamentals/dependency-injection).

Lorsque l’application démarre, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> est injecté dans `Startup.Configure`. L’heure actuelle est mise en cache à l’aide de <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (pour plus d’informations, consultez [hôte Web : Interface IApplicationLifetime](xref:fundamentals/host/web-host#iapplicationlifetime-interface)) :

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

L’exemple d’application injecte <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> dans le `IndexModel` pour une utilisation par la page d’Index.

Chaque fois que la page d’Index est chargée, le cache est activé pour la durée de mise en cache en `OnGetAsync`. Si l’heure de mise en cache n’a pas expiré, l’heure est affichée. Si 20 secondes se sont écoulées depuis la dernière fois que l’heure de mise en cache a accédé (la dernière fois cette page a été chargée), la page affiche *mis en cache délai expiré*.

Mettre immédiatement à jour l’heure de mise en cache à l’heure actuelle en sélectionnant le **réinitialiser le temps en cache** bouton. Les déclencheurs de bouton la `OnPostResetCachedTime` méthode de gestionnaire.

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> Il est inutile d’utiliser un Singleton ou une durée de vie limitée à un périmètre d'utilisation pour gérer des instances de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> (et ceci au moins pour les implémentations intégrées).
>
> Vous pouvez également créer un <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> de l’instance où vous devrez peut-être un au lieu d’utiliser l’injection de dépendances, mais la création d’une instance dans le code peut rendre votre code plus difficile à tester et ne respecte pas la [principe des dépendances explicites](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).

## <a name="recommendations"></a>Recommandations

Lorsque vous décidez quelle implémentation de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> convient le mieux à votre application, considérez les éléments suivants :

* Infrastructure existante
* Exigences de performances
* Coût
* Expérience de l’équipe

Solutions de mise en cache s’appuient généralement sur le stockage en mémoire pour fournir la récupération rapide des données mises en cache, mais la mémoire est une ressource limitée et coûteux pour développer. Magasin uniquement les données couramment utilisées dans un cache.

En règle générale, un cache Redis fournit un débit plus élevé et une latence plus faible qu’un cache de SQL Server. Toutefois, il est généralement requis pour déterminer les caractéristiques de performances des stratégies de mise en cache de tests de performances.

Lorsque SQL Server est utilisé comme magasin de stockage de cache distribué, utiliser la même base de données pour le cache et stockage de données ordinaire de l’application et de récupération peut affecter négativement les performances des deux. Nous vous recommandons d’utiliser une instance de SQL Server dédiée pour le magasin de stockage de cache distribué.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Redis Cache sur Azure](/azure/azure-cache-for-redis/)
* [Base de données SQL sur Azure](/azure/sql-database/)
* [ASP.NET Core fournisseur IDistributedCache pour NCache dans les batteries de serveurs Web](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache sur GitHub](https://github.com/Alachisoft/NCache))
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
