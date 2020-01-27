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
# <a name="distributed-caching-in-aspnet-core"></a>Mise en cache distribuée dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex), [mohsin Nasir](https://github.com/mohsinnasir)et [Steve Smith](https://ardalis.com/)

Un cache distribué est un cache partagé par plusieurs serveurs d’applications, généralement géré comme un service externe pour les serveurs d’applications qui y accèdent. Un cache distribué peut améliorer les performances et l’extensibilité d’une application ASP.NET Core, en particulier lorsque l’application est hébergée par un service Cloud ou une batterie de serveurs.

Un cache distribué présente plusieurs avantages par rapport à d’autres scénarios de mise en cache où les données mises en cache sont stockées sur des serveurs d’applications individuels.

Lorsque les données mises en cache sont distribuées, les données :

* Est *cohérente* (cohérente) entre les demandes adressées à plusieurs serveurs.
* Survit aux redémarrages du serveur et aux déploiements d’applications.
* N’utilise pas la mémoire locale.

La configuration du cache distribué est spécifique à l’implémentation. Cet article explique comment configurer des caches distribués SQL Server et Redims. Des implémentations tierces sont également disponibles, telles que [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache sur GitHub](https://github.com/Alachisoft/NCache)). Quelle que soit l’implémentation choisie, l’application interagit avec le cache à l’aide de l’interface <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisites

::: moniker range=">= aspnetcore-3.0"

Pour utiliser un SQL Server cache distribué, ajoutez une référence de package au package [Microsoft. extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .

Pour utiliser un cache redistribuable ReDim, ajoutez une référence de package au package [Microsoft. extensions. Caching. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) .

Pour utiliser le cache distribué NCache, ajoutez une référence de package au package [NCache. Microsoft. extensions. Caching. Open source](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) .

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Pour utiliser un SQL Server cache distribué, référencez le [AspNetCore. app. app](xref:fundamentals/metapackage-app) ou ajoutez une référence de package au package [Microsoft. extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .

Pour utiliser un cache distribué Redims, référencez le package [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) et ajoutez une référence de package au package [Microsoft. extensions. Caching. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) . Le package Redims n’est pas inclus dans le package `Microsoft.AspNetCore.App`. vous devez donc référencer le package Redims séparément dans votre fichier projet.

Pour utiliser le cache distribué NCache, référencez le logiciel [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) et ajoutez une référence de package au package [NCache. Microsoft. extensions. Caching. Open source](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) . Le package NCache n’est pas inclus dans le package `Microsoft.AspNetCore.App`. vous devez donc référencer le package NCache séparément dans votre fichier projet.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Pour utiliser un SQL Server cache distribué, référencez le [AspNetCore. app. app](xref:fundamentals/metapackage-app) ou ajoutez une référence de package au package [Microsoft. extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .

Pour utiliser un cache redistribuable ReDim, référencez le package [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) et ajoutez une référence de package au package [Microsoft. extensions. Caching. redims](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) . Le package Redims n’est pas inclus dans le package `Microsoft.AspNetCore.App`. vous devez donc référencer le package Redims séparément dans votre fichier projet.

Pour utiliser le cache distribué NCache, référencez le logiciel [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) et ajoutez une référence de package au package [NCache. Microsoft. extensions. Caching. Open source](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) . Le package NCache n’est pas inclus dans le package `Microsoft.AspNetCore.App`. vous devez donc référencer le package NCache séparément dans votre fichier projet.

::: moniker-end

## <a name="idistributedcache-interface"></a>Interface IDistributedCache

L’interface <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> fournit les méthodes suivantes pour manipuler des éléments dans l’implémentation de cache distribué :

* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; accepte une clé de chaîne et récupère un élément mis en cache en tant que tableau de `byte[]`, s’il est trouvé dans le cache.
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; ajoute un élément (en tant que tableau de `byte[]`) au cache à l’aide d’une clé de chaîne.
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; actualise un élément dans le cache en fonction de sa clé, en réinitialisant son délai d’expiration décalé (le cas échéant).
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; supprime un élément de cache en fonction de sa clé de chaîne.

## <a name="establish-distributed-caching-services"></a>Établir des services de mise en cache distribuée

Inscrire une implémentation de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> dans `Startup.ConfigureServices`. Les implémentations fournies par le Framework décrites dans cette rubrique sont les suivantes :

* [Cache mémoire distribuée](#distributed-memory-cache)
* [Cache de SQL Server distribué](#distributed-sql-server-cache)
* [Cache Redims distribué](#distributed-redis-cache)
* [Cache NCache distribué](#distributed-ncache-cache)

### <a name="distributed-memory-cache"></a>Cache mémoire distribuée

Le cache mémoire distribuée (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) est une implémentation fournie par le Framework d' <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> qui stocke des éléments en mémoire. Le cache mémoire distribuée n’est pas un cache distribué réel. Les éléments mis en cache sont stockés par l’instance de l’application sur le serveur sur lequel l’application s’exécute.

Le cache mémoire distribuée est une implémentation utile :

* Dans les scénarios de développement et de test.
* Lorsqu’un serveur unique est utilisé en production et que la consommation de mémoire n’est pas un problème. L’implémentation du cache de mémoire distribuée résume le stockage des données mises en cache. Il permet d’implémenter une solution de mise en cache distribuée réelle à l’avenir si plusieurs nœuds ou une tolérance de panne sont nécessaires.

L’exemple d’application utilise le cache de mémoire distribuée lorsque l’application est exécutée dans l’environnement de développement dans `Startup.ConfigureServices`:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

### <a name="distributed-sql-server-cache"></a>Cache de SQL Server distribué

L’implémentation du cache SQL Server distribué (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) permet au cache distribué d’utiliser une base de données SQL Server comme magasin de stockage. Pour créer un SQL Server table d’éléments mis en cache dans une instance de SQL Server, vous pouvez utiliser l’outil `sql-cache`. L’outil crée une table avec le nom et le schéma que vous spécifiez.

Créer une table dans SQL Server en exécutant la commande `sql-cache create` : Indiquez l’instance de SQL Server (`Data Source`), la base de données (`Initial Catalog`), le schéma (par exemple, `dbo`) et le nom de la table (par exemple, `TestCache`) :

```dotnetcli
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

Un message est consigné pour indiquer la réussite de l’outil :

```console
Table and index were created successfully.
```

La table créée par l’outil `sql-cache` a le schéma suivant :

![Table de cache SqlServer](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> Une application doit manipuler les valeurs de cache à l’aide d’une instance de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, et non d’une <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.

L’exemple d’application implémente <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> dans un environnement de non-développement dans `Startup.ConfigureServices`:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

> [!NOTE]
> Une <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (et éventuellement, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> et <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) sont généralement stockées en dehors du contrôle de code source (par exemple, stockées par le [Gestionnaire de secret](xref:security/app-secrets) ou dans *appsettings. JSON*/*appSettings. { Fichiers ENVIRONMENT}. JSON* ). La chaîne de connexion peut contenir des informations d’identification qui doivent être conservées hors des systèmes de contrôle de code source.

### <a name="distributed-redis-cache"></a>Cache Redims distribué

[Redis](https://redis.io/) est un magasin de données en mémoire open source, qui est souvent utilisé comme un cache distribué. Vous pouvez utiliser des ReDim localement, et vous pouvez configurer un [cache redims Azure](https://azure.microsoft.com/services/cache/) pour une application ASP.net Core hébergée sur Azure.

::: moniker range=">= aspnetcore-3.0"

Une application configure l’implémentation du cache à l’aide d’une instance de <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) dans un environnement de non-développement dans `Startup.ConfigureServices`:

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Une application configure l’implémentation du cache à l’aide d’une instance de <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) dans un environnement de non-développement dans `Startup.ConfigureServices`:

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Une application configure l’implémentation du cache à l’aide d’une instance de <xref:Microsoft.Extensions.Caching.Redis.RedisCache> (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>) :

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

::: moniker-end

Pour installer les éléments ReDim sur votre ordinateur local :

1. Installez le [package redims en chocolat](https://chocolatey.org/packages/redis-64/).
1. Exécutez `redis-server` à partir d’une invite de commandes.

### <a name="distributed-ncache-cache"></a>Cache NCache distribué

[NCache](https://github.com/Alachisoft/NCache) est un cache distribué en mémoire Open source développé en mode natif dans .NET et .net core. NCache fonctionne localement et est configuré en tant que cluster de cache distribué pour une application ASP.NET Core s’exécutant dans Azure ou sur d’autres plateformes d’hébergement.

Pour installer et configurer NCache sur votre ordinateur local, consultez le [Guide de prise en main NCache pour Windows](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).

Pour configurer NCache :

1. Installez [NuGet Open source de NCache](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/).
1. Configurez le cluster de cache dans [client. ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html).
1. Ajoutez le code suivant à `Startup.ConfigureServices` :

   ```csharp
   services.AddNCacheDistributedCache(configuration =>    
   {        
       configuration.CacheName = "demoClusteredCache";
       configuration.EnableLogs = true;
       configuration.ExceptionsEnabled = true;
   });
   ```

## <a name="use-the-distributed-cache"></a>Utiliser le cache distribué

Pour utiliser l’interface <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, demandez une instance de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> à partir de n’importe quel constructeur dans l’application. L’instance est fournie par l' [injection de dépendances (di)](xref:fundamentals/dependency-injection).

::: moniker range=">= aspnetcore-3.0"

Au démarrage de l’exemple d’application, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> est injecté dans `Startup.Configure`. L’heure actuelle est mise en cache à l’aide de <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (pour plus d’informations, consultez [hôte générique : IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)) :

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Au démarrage de l’exemple d’application, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> est injecté dans `Startup.Configure`. L’heure actuelle est mise en cache à l’aide de <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (pour plus d’informations, consultez [hôte Web : IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)) :

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

L’exemple d’application injecte <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> dans le `IndexModel` pour une utilisation par la page d’index.

Chaque fois que la page d’index est chargée, la durée de mise en cache est vérifiée dans `OnGetAsync`. Si l’heure mise en cache n’a pas expiré, l’heure est affichée. Si 20 secondes se sont écoulées depuis le dernier accès à l’heure de mise en cache (la dernière fois que cette page a été chargée), la page affiche le *temps mis en cache expiré*.

Mettez immédiatement à jour l’heure de mise en cache à l’heure actuelle en sélectionnant le bouton **réinitialisation du temps mis en cache** . Le bouton déclenche la méthode du gestionnaire de `OnPostResetCachedTime`.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

> [!NOTE]
> Il est inutile d’utiliser un Singleton ou une durée de vie limitée à un périmètre d'utilisation pour gérer des instances de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> (et ceci au moins pour les implémentations intégrées).
>
> Vous pouvez également créer une instance de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> à chaque fois que vous en aurez besoin au lieu d’utiliser l’injection de dépendances, mais la création d’une instance dans du code peut rendre votre code plus difficile à tester et violer le [principe des dépendances explicites](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).

## <a name="recommendations"></a>Recommandations

Lorsque vous décidez de l’implémentation de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> la mieux adaptée à votre application, tenez compte des points suivants :

* Infrastructure existante
* Exigences en matière de performances
* Cost
* Expérience de l’équipe

Les solutions de mise en cache s’appuient généralement sur le stockage en mémoire pour fournir une récupération rapide des données mises en cache, mais la mémoire est une ressource limitée et coûteuse à développer. Stocke uniquement les données couramment utilisées dans un cache.

En règle générale, un cache Redims fournit un débit plus élevé et une latence inférieure à celle d’un cache de SQL Server. Toutefois, l’évaluation est généralement requise pour déterminer les caractéristiques de performances des stratégies de mise en cache.

Lorsque SQL Server est utilisé en tant que magasin de stockage de cache distribué, l’utilisation de la même base de données pour le cache et du stockage et de la récupération de données ordinaires de l’application peut avoir un impact négatif sur les performances des deux. Nous vous recommandons d’utiliser une instance de SQL Server dédiée pour le magasin de stockage de cache distribué.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Cache redims sur Azure](/azure/azure-cache-for-redis/)
* [SQL Database sur Azure](/azure/sql-database/)
* [ASP.net Core fournisseur IDistributedCache pour NCache dans les batteries de serveurs Web](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache sur GitHub](https://github.com/Alachisoft/NCache))
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
