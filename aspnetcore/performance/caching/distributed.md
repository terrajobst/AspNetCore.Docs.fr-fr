---
title: Travailler avec un cache distribué dans ASP.NET Core
author: ardalis
description: Découvrez comment utiliser la mise en cache distribué afin d’améliorer les performances et l'évolutivité des applications ASP.NET Core, en particulier lorsqu'elles sont hébergées dans un environnement cloud ou dans une batterie de serveurs.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
uid: performance/caching/distributed
ms.openlocfilehash: 5ddc3a6927652f773ab38f93db1e222c5a1900b3
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077697"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a>Travailler avec un cache distribué dans ASP.NET Core

Par [Steve Smith](https://ardalis.com/)

Les caches distribués peuvent améliorer les performances et la scalabilité des applications ASP.NET Core, en particulier lorsqu'ils sont hébergés dans un environnement cloud ou dans une ferme de serveurs. Cet article explique comment utiliser les abstractions et les implémentations de cache distribué intégrées dans ASP.NET Core.


[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-distributed-cache"></a>Qu’est un cache distribué

Un cache distribué est partagé par plusieurs serveurs d’application (consultez [principes de base du Cache](memory.md#caching-basics)). Les informations contenues dans le cache ne sont pas isolées dans la mémoire de chaque serveur de site web, mais les données mises en cache sont disponibles pour tous les serveurs de l’application. Cela présente plusieurs avantages :

1. Les données mises en cache sont cohérentes sur tous les serveurs web. Les utilisateurs ne voient pas des résultats différents selon le serveur web qui gère leur demande

2. Les données mises en cache survivent au redémarrage du serveur web et aux déploiements. Chaque serveur web peut individuellement être supprimé ou ajouté sans impact sur le cache.

3. Il y a moins de requêtes effectuées sur la source de données (que s'il y avait plusieurs caches en mémoire ou pas de mise en cache du tout).

> [!NOTE]
> Si vous utilisez un cache distribué de SQL Server, certains de ces avantages sont réels uniquement si une instance de base de données distincte de celle pour les données de l'application est utilisée pour le cache.

Comme n’importe quel type de cache, un cache distribué peut considérablement améliorer la réactivité d’une application, car en général, il est beaucoup plus rapide de récupérer les données à partir d'un cache qu’à partir d’une base de données relationnelle (ou d'un service web).

La configuration d'un cache est spécifique à l’implémentation. Cet article décrit comment configurer des caches distribués Redis et SQL Server. Indépendamment de l’implémentation sélectionnée, l’application interagit avec le cache à l’aide d’une interface commune `IDistributedCache`.

## <a name="the-idistributedcache-interface"></a>L’Interface IDistributedCache

L'interface `IDistributedCache` inclut des méthodes synchrones et asynchrones. L’interface permet que des éléments soient ajoutés, récupérés ou supprimés à partir de l’implémentation de cache distribué. L'interface `IDistributedCache` comprend les méthodes suivantes :

**Get, GetAsync**

Prend une clé de type string et récupère un élément mis en cache sous la forme d'un `byte[]` si trouvé dans le cache.

**Set, SetAsync**

Ajoute un élément (comme `byte[]`) dans le cache à l’aide d’une clé de type string.

**Actualisation, RefreshAsync**

Actualise un élément dans le cache en fonction de sa clé, la réinitialisation de son délai d’expiration est décalée (si défini).

**Supprimer, RemoveAsync**

Supprime une entrée de cache en fonction de sa clé.

Pour utiliser l'interface `IDistributedCache` :

   1. Ajouter les packages NuGet nécessaires à votre fichier projet.

   2. Configurer l’implémentation spécifique de `IDistributedCache` dans la méthode `ConfigureServices` de votre classe `Startup` et l’ajouter au conteneur.

   3. À partir de l'[intergiciel (middleware)](xref:fundamentals/middleware/index) de l’application ou des classes des contrôleurs MVC, demandez une instance de `IDistributedCache` à partir du constructeur. L’instance sera fournie par l'[Injection de dépendance](../../fundamentals/dependency-injection.md) (DI).

> [!NOTE]
> Il est inutile d’utiliser un Singleton ou une durée de vie limitée à un périmètre d'utilisation pour gérer des instances de `IDistributedCache` (et ceci au moins pour les implémentations intégrées). Vous pouvez également créer explicitement une instance à chaque fois que vous en avez besoin (au lieu d’utiliser l'[Injection de dépendance](../../fundamentals/dependency-injection.md)), mais cela peut rendre votre code plus difficile à tester et ne respecte pas le [principe de dépendances explicites](http://deviq.com/explicit-dependencies-principle/).

L’exemple suivant montre comment utiliser une instance de `IDistributedCache` dans un composant d’intergiciel (middleware) simple :

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

Dans le code ci-dessus, la valeur mise en cache est lue, mais jamais écrite. Dans cet exemple, la valeur est définie uniquement lorsqu’un serveur démarre et elle ne change pas. Dans un scénario multiserveur, le serveur démarré le plus récemment remplace toutes les valeurs précédentes qui ont été définies par d’autres serveurs. Les méthodes `Get` et `Set` utilisent le type `byte[]`. Par conséquent, la valeur de type string doit être convertie à l’aide des méthodes `Encoding.UTF8.GetString` (pour `Get`) et `Encoding.UTF8.GetBytes` (pour `Set`).

Le code suivant du fichier *Startup.cs* affiche la valeur :

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

> [!NOTE]
> Étant donné que `IDistributedCache` est configuré dans la méthode `ConfigureServices`, elle est disponible pour la méthode `Configure` en tant que paramètre. Afin de pouvoir injecter sous forme de dépendance l'instance configurée, ajoutez-la en tant que paramètre. Pour permettre à l’instance configurée être fourni via DI, ajoutez-le en tant que paramètre.

## <a name="using-a-redis-distributed-cache"></a>Utiliser un cache distribué Redis

[Redis](https://redis.io/) est un magasin de données en mémoire open source, qui est souvent utilisé comme un cache distribué. Vous pouvez l’utiliser localement, et vous pouvez configurer un [Cache Redis Azure](https://azure.microsoft.com/services/cache/) pour vos applications ASP.NET Core hébergées sur Azure. Votre application ASP.NET Core configure l’implémentation de cache via une instance de type `RedisDistributedCache`.

Vous configurez l’implémentation de Redis dans `ConfigureServices` et y accédez dans le code de votre application en demandant une instance de `IDistributedCache` (voir le code ci-dessus).

Dans l’exemple de code, une implémentation de type `RedisCache` est utilisée lorsque le serveur est configuré pour un environnement `Staging`. Par conséquent, la méthode `ConfigureStagingServices` configure le `RedisCache`:

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

> [!NOTE]
> Pour installer Redis sur votre ordinateur local, installez le package chocolatey [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) et exécutez `redis-server` à partir d’une invite de commandes.

## <a name="using-a-sql-server-distributed-cache"></a>Utiliser un cache distribué SQL Server

L’implémentation proposée par SqlServerCache permet l'utilisation d'une base de données SQL Server comme magasin de sauvegarde du cache distribué. Pour créer une table SQL Server, vous pouvez utiliser l'outil sql-cache, l’outil crée une table avec le nom et le schéma que vous spécifiez.

::: moniker range="< aspnetcore-2.1"

Ajouter `SqlConfig.Tools` à la `<ItemGroup>` élément du fichier de projet et exécuter `dotnet restore`.

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

Test SqlConfig.Tools en exécutant la commande suivante :

```console
dotnet sql-cache create --help
```

SqlConfig.Tools affiche l’utilisation, options et l’aide de la commande.

Créer une table dans SQL Server en exécutant le `sql-cache create` commande :

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

Le tableau présente le schéma suivant :

![Table de Cache de SQL Server](distributed/_static/SqlServerCacheTable.png)

Comme toutes les implémentations de cache, votre application doit obtenir et définir des valeurs de cache à l’aide d’une instance de `IDistributedCache`, et non de `SqlServerCache`.  L’exemple implémente `SqlServerCache` dans l’environnement de Production (par conséquent, il est configuré dans `ConfigureProductionServices`).

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> `ConnectionString` (et, éventuellement, `SchemaName` et `TableName`) doit généralement être stocké en dehors du contrôle de code source (par exemple, usersecrets.), car il peut contenir des informations d’identification.

## <a name="recommendations"></a>Recommandations

Lorsque vous décidez quelle implémentation de `IDistributedCache` est adaptée à votre application, choisissez entre Redis et SQL Server en fonction de votre infrastructure existante et votre environnement, de vos exigences de performances et l'expérience de votre équipe. Si votre équipe est plus familière avec Redis, Redis constitue un excellent choix. Si votre équipe préfère SQL Server, vous pouvez être certain qu’elle appréciera également l’implémentation SQL Server. Notez qu’une solution de mise en cache traditionnelle stocke les données en mémoire qui permet la récupération rapide des données. Vous devez stocker les données couramment utilisées dans un cache et stocker la totalité des données dans un magasin persistant de back-end comme SQL Server ou le Stockage Azure. Un cache redis est une solution de mise en cache qui vous donne un débit élevé et une faible latence par rapport au Cache SQL.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Cache dans Azure redis](https://azure.microsoft.com/documentation/services/redis-cache/)
* [Base de données SQL Azure](https://azure.microsoft.com/documentation/services/sql-database/)
* [Mettre en cache en mémoire](xref:performance/caching/memory)
* [Détecter les modifications à l’aide de jetons de modification](xref:fundamentals/primitives/change-tokens)
* [Mise en cache des réponses](xref:performance/caching/response)
* [Intergiciel de mise en cache des réponses](xref:performance/caching/middleware)
* [Tag Helper de cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Tag Helper de cache distribué](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
