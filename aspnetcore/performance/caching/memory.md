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
# <a name="cache-in-memory-in-aspnet-core"></a>Mettre en cache en mémoire dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), et [Steve Smith](https://ardalis.com/)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="caching-basics"></a>Principes fondamentaux de la mise en cache

La mise en cache peut améliorer considérablement les performances et l’évolutivité d’une application en réduisant le travail requis pour générer le contenu. Elle est idéale pour les données qui ne sont pas souvent modifiées. Elle effectue une copie des données, qui sont ainsi renvoyées beaucoup plus rapidement qu’à partir de la source d’origine. L'application doit être écrite et testée de façon à ne jamais dépendre des données en cache.

ASP.NET Core prend en charge plusieurs caches différents. Le cache de la plus simple est basé sur le [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), qui représente un cache stocké dans la mémoire du serveur web. Les applications qui s’exécutent sur une batterie de serveurs de plusieurs serveurs devraient vous assurer que les sessions rémanentes lors de l’utilisation du cache en mémoire. Sessions rémanentes Assurez-vous que les requêtes ultérieures à partir d’un client tous les dirigé vers le même serveur. Par exemple, les utilisation d’applications Web Azure [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) pour router toutes les demandes ultérieures au même serveur.

Les sessions non persistantes dans une batterie de serveurs Web nécessitent un [cache distribué](distributed.md) pour éviter les problèmes de cohérence du cache.  Pour certaines applications, un cache distribué est est mesure de gérer une montée en charge plus élevée qu’un cache en mémoire. Il permet de décharger la mémoire cache vers un processus externe.

Le `IMemoryCache` cache supprimez les entrées du cache mémoire insuffisante, sauf si le [cache priorité](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) a la valeur `CacheItemPriority.NeverRemove`. Vous pouvez définir le `CacheItemPriority` pour ajuster la priorité avec laquelle le cache supprime les éléments de sollicitation de la mémoire.

Le cache en mémoire permet de stocker n’importe quel objet ; l’interface de cache distribué est limitée à `byte[]`.

## <a name="using-imemorycache"></a>À l’aide de IMemoryCache

La mise en cache en mémoire est un *service* qui est référencé à partir de votre application à l’aide de l' [Injection de dépendance](../../fundamentals/dependency-injection.md). Appelez `AddMemoryCache` dans `ConfigureServices`:

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

Faire appel à l'instance `IMemoryCache` dans le constructeur :

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

`IMemoryCache` nécessite le package NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

`IMemoryCache` nécessite le package NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), qui est disponible dans le [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).

::: moniker-end

::: moniker range="> aspnetcore-2.0"

`IMemoryCache` nécessite le package NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), qui est disponible dans le [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).

::: moniker-end

Le code suivant utilise [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) pour vérifier si une heure est dans le cache. Si une heure n’est pas mis en cache, une nouvelle entrée est créée et ajoutée au cache avec [définir](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

L’heure actuelle et l’heure de mise en cache s’affichent :

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

La valeur `DateTime` mise en cache reste dans le cache tant qu'il existe des demandes dans le délai d’expiration (et aucune suppression en raison d’une sollicitation de la mémoire). L’illustration ci-dessous indique l’heure actuelle et une heure antérieure récupérées du cache :

![Vue d’index avec deux fois différentes affichées](memory/_static/time.png)

Le code suivant utilise [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) et [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) en cache des données. 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Le code suivant appelle [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) pour extraire l’heure de mise en cache :

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

Consultez [les méthodes de IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) et [les méthodes de CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) pour obtenir une description des méthodes du cache.

## <a name="using-memorycacheentryoptions"></a>Utilisation de MemoryCacheEntryOptions

L’exemple suivant :

- Définit l’heure d’expiration absolue. Ceci est la durée maximale pendant laquelle l’entrée peut être mise en cache et empêche l’élément de devenir trop périmé quand l’expiration glissante est constamment renouvelée.
- Définit un délai d’expiration glissant. Les requêtes qui accèdent à cet élément de mise en cache réinitialisent l’horloge d’expiration glissante.
- Définit la priorité de cache `CacheItemPriority.NeverRemove`.
- Définit un [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) qui est appelé après la suppression de l’entrée du cache. Le rappel est exécuté sur un thread différent du code qui supprime l’élément à partir du cache.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="cache-dependencies"></a>Dépendances de cache

L’exemple suivant montre comment expirer une entrée de cache si une entrée dépendante expire. Un `CancellationChangeToken` est ajouté à l’élément mis en cache. Lorsque `Cancel` est appelée sur le `CancellationTokenSource`, les deux entrées du cache sont supprimées.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

Utiliser un `CancellationTokenSource` permet à plusieurs entrées de cache d'être supprimées en tant que groupe. Avec le modèle `using` dans le code ci-dessus, les entrées de cache créées à l’intérieur du bloc `using` hériteront des déclencheurs et des paramètres d’expiration.

## <a name="additional-notes"></a>Remarques supplémentaires

- Lorsque vous utilisez un rappel pour remplir un élément de cache :

  - Plusieurs demandes peuvent trouver la valeur de clé mise en cache vide étant donné que le rappel n’est pas terminé.
  - Il peut en résulter que plusieurs threads remplissent l’élément mis en cache.

- Lorsqu’une entrée de cache est utilisée pour en créer une autre, l’enfant copie les jetons d’expiration et les paramètres d’expiration basés sur le temps de l’entrée parente. L’enfant n’expire par suite à la suppression manuelle ou à la mise à jour de l’entrée parente.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Utiliser un cache distribué](xref:performance/caching/distributed)
* [Détecter les modifications à l’aide de jetons de modification](xref:fundamentals/primitives/change-tokens)
* [Mise en cache des réponses](xref:performance/caching/response)
* [Intergiciel de mise en cache des réponses](xref:performance/caching/middleware)
* [Tag Helper de cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Tag Helper de cache distribué](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
