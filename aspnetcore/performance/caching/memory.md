---
title: Mettre en cache en mémoire dans ASP.NET Core
author: rick-anderson
description: Découvrez comment mettre en cache les données en mémoire dans ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 8/22/2019
uid: performance/caching/memory
ms.openlocfilehash: 23bbca5ded51d504a04415ced99ad3a6094fff6e
ms.sourcegitcommit: 41f2c1a6b316e6e368a4fd27a8b18d157cef91e1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886453"
---
# <a name="cache-in-memory-in-aspnet-core"></a>Mettre en cache en mémoire dans ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

Par [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo)et [Steve Smith](https://ardalis.com/)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="caching-basics"></a>Bases de la mise en cache

La mise en cache peut améliorer considérablement les performances et l’évolutivité d’une application en réduisant le travail requis pour générer le contenu. La mise en cache fonctionne mieux avec les données qui changent rarement **et** qui sont coûteuses à générer. La mise en cache crée une copie des données qui peuvent être retournées beaucoup plus rapidement qu’à partir de la source. Les applications doivent être écrites et testées pour **ne** pas dépendre des données mises en cache.

ASP.NET Core prend en charge plusieurs caches différents. Le cache le plus simple est basé sur [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache). `IMemoryCache`représente un cache stocké dans la mémoire du serveur Web. Les applications qui s’exécutent sur une batterie de serveurs (plusieurs serveurs) doivent s’assurer que les sessions sont permanentes lors de l’utilisation du cache en mémoire. Les sessions rémanentes garantissent que les demandes d’un client vont vers le même serveur. Par exemple, les applications web Azure utilisent [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) pour router toutes les demandes vers le même serveur.

Les sessions non rémanentes dans une batterie de serveurs web nécessitent un [cache distribué](distributed.md) pour éviter les problèmes de cohérence du cache. Pour certaines applications, un cache distribué peut prendre en charge un sclae-out plus important qu'un cache en mémoire. Il permet de décharger la mémoire cache vers un processus externe.

Le cache en mémoire peut stocker n’importe quel objet. L’interface du cache distribué est limitée `byte[]`à. Les éléments du cache de stockage du cache distribué et en mémoire sont des paires clé-valeur.

## <a name="systemruntimecachingmemorycache"></a>System.Runtime.Caching/MemoryCache

<xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache>([Package NuGet](https://www.nuget.org/packages/System.Runtime.Caching/)) peut être utilisé avec:

* .NET Standard 2,0 ou version ultérieure.
* Toute [implémentation .net](/dotnet/standard/net-standard#net-implementation-support) qui cible .NET standard 2,0 ou version ultérieure. Par exemple, ASP.NET Core 2,0 ou version ultérieure.
* .NET Framework 4,5 ou version ultérieure.

[Microsoft. extensions. Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) / `IMemoryCache` (décrit dans `System.Runtime.Caching` / `MemoryCache` cet article) est recommandé, car il est mieux intégré à ASP.net core. Par exemple, `IMemoryCache` fonctionne en mode natif avec [l’injection de dépendances](xref:fundamentals/dependency-injection) ASP.NET Core.

À `System.Runtime.Caching` utiliser / entantquepontdecompatibilitélorsduPortageducodedeASP.net4.xversASP.netcore.`MemoryCache`

## <a name="cache-guidelines"></a>Instructions du cache

* Le code doit toujours avoir une option de secours pour extraire les données et **ne pas** dépendre d’une valeur mise en cache disponible.
* Le cache utilise une ressource rare, la mémoire. Limiter la croissance du cache:
  * N’utilisez **pas** d’entrée externe comme clés de cache.
  * Utilisez des expirations pour limiter la croissance du cache.
  * [Utilisez les paramètres, size et SizeLimit pour limiter la taille du cache](#use-setsize-size-and-sizelimit-to-limit-cache-size). Le runtime ASP.NET Core ne limite **pas** la taille du cache en fonction de la sollicitation de la mémoire. C’est au développeur de limiter la taille du cache.

## <a name="use-imemorycache"></a>Utiliser IMemoryCache

> [!WARNING]
> L’utilisation d’un cache mémoire *partagé* à partir d’une `Size`injection de `SizeLimit` [dépendance](xref:fundamentals/dependency-injection) et l’appel `SetSize`de, ou pour limiter la taille du cache peuvent entraîner l’échec de l’application. Quand une limite de taille est définie sur un cache, toutes les entrées doivent spécifier une taille lors de leur ajout. Cela peut entraîner des problèmes, car les développeurs n’ont peut-être pas un contrôle total sur ce qui utilise le cache partagé. Par exemple, Entity Framework Core utilise le cache partagé et ne spécifie pas de taille. Si une application définit une limite de taille de cache et utilise EF Core, l’application lève `InvalidOperationException`une exception.
> Lorsque vous `SetSize`utilisez `Size`, ou `SizeLimit` pour limiter le cache, créez un singleton de cache pour la mise en cache. Pour plus d’informations et pour obtenir un exemple, consultez utiliser la configuration, la [taille et SizeLimit pour limiter la taille du cache](#use-setsize-size-and-sizelimit-to-limit-cache-size).

La mise en cache en mémoire est un *service* référencé à partir d’une application à l’aide de l' [injection](xref:fundamentals/dependency-injection)de dépendances. Faire appel à l'instance `IMemoryCache` dans le constructeur :

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_ctor)]

Le code suivant utilise [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) pour vérifier si une heure se trouve dans le cache. Si une heure n’est pas mis en cache, une nouvelle entrée est créée et ajoutée au cache avec [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).

[! code-CSharp [] (Memory/3.0 Sample/WebCacheSample/CacheKeys. cs) [](memory/3.0sample/WebCacheSample/CacheKeys.cs)]

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet1)]

L’heure actuelle et l’heure de mise en cache s’affichent :

[!code-cshtml[](memory/3.0sample/WebCacheSample/Views/Home/Cache.cshtml)]

La `DateTime` valeur mise en cache reste dans le cache pendant qu’il y a des requêtes dans le délai imparti.

Le code suivant utilise [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) et [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) pour mettre en cache des données.

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Le code suivant appelle [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) pour extraire l’heure de mise en cache :

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_gct)]

Le code suivant obtient ou crée un élément mis en cache avec une expiration absolue:

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet99)]

Un ensemble d’éléments mis en cache avec expiration décalée est menacé pour devenir obsolète, car il n’y a aucune limite à son expiration. Utilisez une expiration absolue avec une expiration décalée pour garantir que l’élément mis en cache ne deviendra pas plus obsolète que l’expiration absolue. Lorsque l’expiration absolue est combinée avec le glissement, l’expiration absolue définit une limite supérieure à la durée pendant laquelle l’élément peut être mis en cache. Contrairement à l’heure d’expiration absolue uniquement, si l’élément n’est pas demandé dans le cache au cours de l’intervalle d’expiration décalé, l’élément est supprimé du cache. Lorsque l’expiration absolue et décalée est spécifiée, les expirations sont logiquement associées.

Le code suivant obtient ou crée un élément mis en cache avec une expiration décalée et absolue:

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet9)]

Le code précédent garantit que les données ne seront pas mises en cache plus longtemps que l’heure absolue.

<xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*>, <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*> <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions> <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>et <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.Get*> sont des méthodes d’extension qui font partie de la classe qui étend la fonctionnalité de. Pour obtenir une description des autres méthodes de cache, consultez [méthodes IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) et [méthodes CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) .

## <a name="memorycacheentryoptions"></a>MemoryCacheEntryOptions

L’exemple suivant:

* Définit un délai d’expiration glissant. Les requêtes qui accèdent à cet élément de mise en cache réinitialisent l’horloge d’expiration glissante.
* Définit la priorité du cache sur [CacheItemPriority. NeverRemove](xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove).
* Définit un [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) qui est appelé après la suppression de l’entrée du cache. Le rappel est exécuté sur un thread différent du code qui supprime l’élément à partir du cache.

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a>Utiliser SetSize, Size et SizeLimit pour limiter la taille du cache

Une instance `MemoryCache` peut éventuellement spécifier et appliquer une limite de taille. La limite de taille de mémoire n’a pas d’unité de mesure, car le cache dispose d’aucun mécanisme pour mesurer le nombre d’entrées. Si la limite de taille de la mémoire cache est définie, toutes les entrées doivent spécifier la taille. Le runtime ASP.NET Core ne limite pas la taille du cache en fonction de la sollicitation de la mémoire. C’est au développeur de limiter la taille du cache. La taille spécifiée est exprimé en unités choisies par le développeur.

Exemple :

* Si l’application web met en cache principalement les chaînes, chaque taille d’entrée du cache peut être la longueur de chaîne.
* L’application peut spécifier la taille de toutes les entrées en tant que 1, et la limite de taille est le nombre d’entrées.

Si <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheOptions.SizeLimit> n’est pas défini, le cache croît sans limite. Le runtime ASP.NET Core ne supprime pas le cache lorsque la mémoire système est insuffisante. Les applications sont en grande partie conçues pour:

* Limiter la croissance du cache.
* Appelez <xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Compact*> ou<xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Remove*> lorsque la mémoire disponible est limitée:

Le code suivant crée une taille <xref:Microsoft.Extensions.Caching.Memory.MemoryCache> fixe sans unité accessible par [injection de dépendances](xref:fundamentals/dependency-injection):

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

`SizeLimit`n’a pas d’unités. Les entrées mises en cache doivent indiquer la taille dans les unités jugées les plus appropriées si la taille de la mémoire cache a été définie. Tous les utilisateurs d’une instance de cache doivent utiliser le même système d’unité. Une entrée n’est pas mise en cache si la somme des tailles des entrées mises en cache dépasse la valeur spécifiée par `SizeLimit`. Si aucune limite de taille du cache n’est définie, la taille de cache définie sur l’entrée est ignorée.

Le code suivant inscrit `MyMemoryCache` avec le conteneur [d’injection de dépendances](xref:fundamentals/dependency-injection).

[!code-csharp[](memory/3.0sample/RPcache/Startup.cs?name=snippet)]

`MyMemoryCache` est créé comme cache de mémoire indépendant pour les composants qui sont informés de la taille limitée du cache et qui ont la capacité de définir une taille d’entrée de cache en conséquence.

Le code suivant utilise `MyMemoryCache`:

[!code-csharp[](memory/3.0sample/RPcache/Pages/SetSize.cshtml.cs?name=snippet)]

La taille de l’entrée de cache peut être définie <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.Size> par ou <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.SetSize*> par les méthodes d’extension:

[!code-csharp[](memory/3.0sample/RPcache/Pages/SetSize.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

### <a name="memorycachecompact"></a>MemoryCache. compact

`MemoryCache.Compact`tente de supprimer le pourcentage spécifié du cache dans l’ordre suivant:

* Tous les éléments arrivés à expiration.
* Éléments par priorité. Les éléments de priorité la plus basse sont supprimés en premier.
* Objets utilisés le moins récemment.
* Éléments avec l’expiration absolue la plus ancienne.
* Éléments avec l’expiration décalée la plus ancienne.

Les éléments épinglés <xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove> avec priorité ne sont jamais supprimés. Le code suivant supprime un élément de cache et `Compact`appelle:

[!code-csharp[](memory/3.0sample/RPcache/Pages/TestCache.cshtml.cs?name=snippet3)]

Pour plus d’informations, consultez [source compact sur GitHub](https://github.com/aspnet/Extensions/blob/v3.0.0-preview8.19405.4/src/Caching/Memory/src/MemoryCache.cs#L382-L393) .

## <a name="cache-dependencies"></a>Dépendances de cache

L’exemple suivant montre comment expirer une entrée de cache si une entrée dépendante expire. Un `CancellationChangeToken` est ajouté à l’élément mis en cache. Lorsque `Cancel` est appelée sur le `CancellationTokenSource`, les deux entrées du cache sont supprimées.

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_ed)]

Utiliser un `CancellationTokenSource` permet à plusieurs entrées de cache d'être supprimées en tant que groupe. Avec le modèle `using` dans le code ci-dessus, les entrées de cache créées à l’intérieur du bloc `using` hériteront des déclencheurs et des paramètres d’expiration.

## <a name="additional-notes"></a>Remarques supplémentaires

* L’expiration ne se produit pas en arrière-plan. Il n’existe pas de minuteur qui analyse activement le cache à la recherche d’éléments arrivés à expiration. Toute activité sur le cache (`Get`, `Set`, `Remove`) peut déclencher une analyse en arrière-plan des éléments arrivés à expiration. Un minuteur sur `CancellationTokenSource` (`CancelAfter`) supprimerait également l’entrée et déclenchera une analyse des éléments arrivés à expiration. Par exemple, au lieu `SetAbsoluteExpiration(TimeSpan.FromHours(1))`d’utiliser, utilisez `CancellationTokenSource.CancelAfter(TimeSpan.FromHours(1))` pour le jeton inscrit. Quand ce jeton est activé, il supprime immédiatement l’entrée et déclenche les rappels d’éviction. Pour plus d’informations, consultez [ce problème GitHub](https://github.com/aspnet/Caching/issues/248).
* Lorsque vous utilisez un rappel pour remplir un élément de cache :

  * Plusieurs demandes peuvent trouver la valeur de clé mise en cache vide étant donné que le rappel n’est pas terminé.
  * Il peut en résulter que plusieurs threads remplissent l’élément mis en cache.

* Lorsqu’une entrée de cache est utilisée pour en créer une autre, l’enfant copie les jetons d’expiration et les paramètres d’expiration basés sur le temps de l’entrée parente. L’enfant n’est pas expiré par la suppression manuelle ou à la mise à jour de l’entrée parente.

* Utilisez <xref:Microsoft.Extensions.Caching.Memory.ICacheEntry.PostEvictionCallbacks> pour définir les rappels qui seront déclenchés après que l’entrée du cache a été supprimée du cache.
* Pour la plupart des `IMemoryCache` applications, est activé. Par exemple, l' `AddMvc`appel `AddControllersWithViews`de `AddRazorPages`, `AddMvcCore().AddRazorViewEngine`,,, et `Add{Service}` de nombreuses `ConfigureServices`autres méthodes `IMemoryCache`dans, active. Pour les applications qui n’appellent pas l’une des `Add{Service}` méthodes précédentes, il peut être nécessaire d' <xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddMemoryCache*> appeler `ConfigureServices`dans.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<!-- This is the 2.1 version -->
Par [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo)et [Steve Smith](https://ardalis.com/)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="caching-basics"></a>Bases de la mise en cache

La mise en cache peut améliorer considérablement les performances et l’évolutivité d’une application en réduisant le travail requis pour générer le contenu. Elle est idéale pour les données qui ne sont pas souvent modifiées. Elle effectue une copie des données, qui sont ainsi renvoyées beaucoup plus rapidement qu’à partir de la source d’origine. Le code doit être écrit et testé pour **ne** pas dépendre des données mises en cache.

ASP.NET Core prend en charge plusieurs caches différents. Le cache le plus simple est basé sur le [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), qui représente un cache stocké dans la mémoire du serveur web. Les applications qui s’exécutent sur une batterie de serveurs (plusieurs serveurs) doivent s’assurer que les sessions sont permanentes lors de l’utilisation du cache en mémoire. Les sessions rémanentes garantissent que les demandes ultérieures provenant d’un client sont toutes dirigées vers le même serveur. Par exemple, Azure Web Apps utilise [application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (arr) pour acheminer toutes les demandes d’un agent utilisateur vers le même serveur.

Les sessions non rémanentes dans une batterie de serveurs web nécessitent un [cache distribué](distributed.md) pour éviter les problèmes de cohérence du cache. Pour certaines applications, un cache distribué peut prendre en charge un sclae-out plus important qu'un cache en mémoire. Il permet de décharger la mémoire cache vers un processus externe.

Le cache en mémoire peut stocker n’importe quel objet. L’interface du cache distribué est limitée `byte[]`à. Les éléments du cache de stockage du cache distribué et en mémoire sont des paires clé-valeur.

## <a name="systemruntimecachingmemorycache"></a>System.Runtime.Caching/MemoryCache

<xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache>([Package NuGet](https://www.nuget.org/packages/System.Runtime.Caching/)) peut être utilisé avec:

* .NET Standard 2,0 ou version ultérieure.
* Toute [implémentation .net](/dotnet/standard/net-standard#net-implementation-support) qui cible .NET standard 2,0 ou version ultérieure. Par exemple, ASP.NET Core 2,0 ou version ultérieure.
* .NET Framework 4,5 ou version ultérieure.

[Microsoft. extensions. Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) / `IMemoryCache` (décrit dans `System.Runtime.Caching` / `MemoryCache` cet article) est recommandé, car il est mieux intégré à ASP.net core. Par exemple, `IMemoryCache` fonctionne en mode natif avec [l’injection de dépendances](xref:fundamentals/dependency-injection) ASP.NET Core.

À `System.Runtime.Caching` utiliser / entantquepontdecompatibilitélorsduPortageducodedeASP.net4.xversASP.netcore.`MemoryCache`

## <a name="cache-guidelines"></a>Instructions du cache

* Le code doit toujours avoir une option de secours pour extraire les données et **ne pas** dépendre d’une valeur mise en cache disponible.
* Le cache utilise une ressource rare, la mémoire. Limiter la croissance du cache:
  * N’utilisez **pas** d’entrée externe comme clés de cache.
  * Utilisez des expirations pour limiter la croissance du cache.
  * [Utilisez les paramètres, size et SizeLimit pour limiter la taille du cache](#use-setsize-size-and-sizelimit-to-limit-cache-size). Le runtime ASP.NET Core ne limite pas la taille du cache en fonction de la sollicitation de la mémoire. C’est au développeur de limiter la taille du cache.

## <a name="using-imemorycache"></a>Utilisation de IMemoryCache

> [!WARNING]
> L’utilisation d’un cache mémoire *partagé* à partir d’une `Size`injection de `SizeLimit` [dépendance](xref:fundamentals/dependency-injection) et l’appel `SetSize`de, ou pour limiter la taille du cache peuvent entraîner l’échec de l’application. Quand une limite de taille est définie sur un cache, toutes les entrées doivent spécifier une taille lors de leur ajout. Cela peut entraîner des problèmes, car les développeurs n’ont peut-être pas un contrôle total sur ce qui utilise le cache partagé. Par exemple, Entity Framework Core utilise le cache partagé et ne spécifie pas de taille. Si une application définit une limite de taille de cache et utilise EF Core, l’application lève `InvalidOperationException`une exception.
> Lorsque vous `SetSize`utilisez `Size`, ou `SizeLimit` pour limiter le cache, créez un singleton de cache pour la mise en cache. Pour plus d’informations et pour obtenir un exemple, consultez utiliser la configuration, la [taille et SizeLimit pour limiter la taille du cache](#use-setsize-size-and-sizelimit-to-limit-cache-size).

La mise en cache en mémoire est un *service* qui est référencé à partir de votre application à l’aide de l'[Injection de dépendance](../../fundamentals/dependency-injection.md). Appelez `AddMemoryCache` dans `ConfigureServices`:

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

Faire appel à l'instance `IMemoryCache` dans le constructeur :

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

`IMemoryCache`requiert le package NuGet [Microsoft. extensions. Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), qui est disponible dans le [produit Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).

Le code suivant utilise [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) pour vérifier si une heure se trouve dans le cache. Si une heure n’est pas mis en cache, une nouvelle entrée est créée et ajoutée au cache avec [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).

[! code-CSharp [] (mémoire/exemple/WEBCACHE/CacheKeys. cs) [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

L’heure actuelle et l’heure de mise en cache s’affichent :

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

La `DateTime` valeur mise en cache reste dans le cache pendant qu’il y a des requêtes dans le délai imparti. L’illustration ci-dessous indique l’heure actuelle et une heure antérieure récupérées du cache :

![Vue d’index avec deux heures différentes affichées](memory/_static/time.png)

Le code suivant utilise [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) et [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) pour mettre en cache des données.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Le code suivant appelle [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) pour extraire l’heure de mise en cache :

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*>, <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*> et [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) sont des méthodes d’extension qui font partie de la classe [CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) qui étend la fonctionnalité de <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>. Pour obtenir une description des autres méthodes de cache, consultez [méthodes IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) et [méthodes CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) .

## <a name="memorycacheentryoptions"></a>MemoryCacheEntryOptions

L’exemple suivant:

* Définit un délai d’expiration glissant. Les requêtes qui accèdent à cet élément de mise en cache réinitialisent l’horloge d’expiration glissante.
* Affecte à la priorité de `CacheItemPriority.NeverRemove`cache la valeur.
* Définit un [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) qui est appelé après la suppression de l’entrée du cache. Le rappel est exécuté sur un thread différent du code qui supprime l’élément à partir du cache.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a>Utiliser SetSize, Size et SizeLimit pour limiter la taille du cache

Une instance `MemoryCache` peut éventuellement spécifier et appliquer une limite de taille. La limite de taille de mémoire n’a pas d’unité de mesure, car le cache dispose d’aucun mécanisme pour mesurer le nombre d’entrées. Si la limite de taille de la mémoire cache est définie, toutes les entrées doivent spécifier la taille. Le runtime ASP.NET Core ne limite pas la taille du cache en fonction de la sollicitation de la mémoire. C’est au développeur de limiter la taille du cache. La taille spécifiée est exprimé en unités choisies par le développeur.

Exemple :

* Si l’application web met en cache principalement les chaînes, chaque taille d’entrée du cache peut être la longueur de chaîne.
* L’application peut spécifier la taille de toutes les entrées en tant que 1, et la limite de taille est le nombre d’entrées.

Si <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheOptions.SizeLimit> n’est pas défini, le cache croît sans limite. Le runtime ASP.NET Core ne supprime pas le cache lorsque la mémoire système est insuffisante. Les applications sont en grande partie conçues pour:

* Limiter la croissance du cache.
* Appelez <xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Compact*> ou<xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Remove*> lorsque la mémoire disponible est limitée:

Le code suivant crée une taille <xref:Microsoft.Extensions.Caching.Memory.MemoryCache> fixe sans unité accessible par [injection de dépendances](xref:fundamentals/dependency-injection):

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

`SizeLimit`n’a pas d’unités. Les entrées mises en cache doivent indiquer la taille dans les unités jugées les plus appropriées si la taille de la mémoire cache a été définie. Tous les utilisateurs d’une instance de cache doivent utiliser le même système d’unité. Une entrée n’est pas mise en cache si la somme des tailles des entrées mises en cache dépasse la valeur spécifiée par `SizeLimit`. Si aucune limite de taille du cache n’est définie, la taille de cache définie sur l’entrée est ignorée.

Le code suivant inscrit `MyMemoryCache` avec le conteneur [d’injection de dépendances](xref:fundamentals/dependency-injection).

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

`MyMemoryCache` est créé comme cache de mémoire indépendant pour les composants qui sont informés de la taille limitée du cache et qui ont la capacité de définir une taille d’entrée de cache en conséquence.

Le code suivant utilise `MyMemoryCache`:

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

La taille de l’entrée de cache peut être définie par [Size](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) ou par la méthode d’extension [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) :

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

### <a name="memorycachecompact"></a>MemoryCache. compact

`MemoryCache.Compact`tente de supprimer le pourcentage spécifié du cache dans l’ordre suivant:

* Tous les éléments arrivés à expiration.
* Éléments par priorité. Les éléments de priorité la plus basse sont supprimés en premier.
* Objets utilisés le moins récemment.
* Éléments avec l’expiration absolue la plus ancienne.
* Éléments avec l’expiration décalée la plus ancienne.

Les éléments épinglés <xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove> avec priorité ne sont jamais supprimés.

[!code-csharp[](memory/3.0sample/RPcache/Pages/TestCache.cshtml.cs?name=snippet3)]

Pour plus d’informations, consultez [source compact sur GitHub](https://github.com/aspnet/Extensions/blob/v3.0.0-preview8.19405.4/src/Caching/Memory/src/MemoryCache.cs#L382-L393) .

## <a name="cache-dependencies"></a>Dépendances de cache

L’exemple suivant montre comment expirer une entrée de cache si une entrée dépendante expire. Un `CancellationChangeToken` est ajouté à l’élément mis en cache. Lorsque `Cancel` est appelée sur le `CancellationTokenSource`, les deux entrées du cache sont supprimées.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

Utiliser un `CancellationTokenSource` permet à plusieurs entrées de cache d'être supprimées en tant que groupe. Avec le modèle `using` dans le code ci-dessus, les entrées de cache créées à l’intérieur du bloc `using` hériteront des déclencheurs et des paramètres d’expiration.

## <a name="additional-notes"></a>Remarques supplémentaires

* Lorsque vous utilisez un rappel pour remplir un élément de cache :

  * Plusieurs demandes peuvent trouver la valeur de clé mise en cache vide étant donné que le rappel n’est pas terminé.
  * Il peut en résulter que plusieurs threads remplissent l’élément mis en cache.

* Lorsqu’une entrée de cache est utilisée pour en créer une autre, l’enfant copie les jetons d’expiration et les paramètres d’expiration basés sur le temps de l’entrée parente. L’enfant n’est pas expiré par la suppression manuelle ou à la mise à jour de l’entrée parente.

* Utilisez [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) pour définir que les rappels sont déclenchés une fois que l’entrée de cache est supprimée du cache.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

::: moniker-end
