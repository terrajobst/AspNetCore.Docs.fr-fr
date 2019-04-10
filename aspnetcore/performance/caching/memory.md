---
title: Mettre en cache en mémoire dans ASP.NET Core
author: rick-anderson
description: Découvrez comment mettre en cache les données en mémoire dans ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/11/2019
uid: performance/caching/memory
ms.openlocfilehash: 6433df36023b79bc679186bee8b0a92371661dbe
ms.sourcegitcommit: 258a97159da206f9009f23fdf6f8fa32f178e50b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59425047"
---
# <a name="cache-in-memory-in-aspnet-core"></a>Mettre en cache en mémoire dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), et [Steve Smith](https://ardalis.com/)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="caching-basics"></a>Principes fondamentaux de la mise en cache

La mise en cache peut améliorer considérablement les performances et l’évolutivité d’une application en réduisant le travail requis pour générer le contenu. Elle est idéale pour les données qui ne sont pas souvent modifiées. Elle effectue une copie des données, qui sont ainsi renvoyées beaucoup plus rapidement qu’à partir de la source d’origine. Les applications doivent être écrites et testées à **jamais** dépendent des données mises en cache.

ASP.NET Core prend en charge plusieurs caches différents. Le cache le plus simple est basé sur le [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), qui représente un cache stocké dans la mémoire du serveur web. Les applications qui s’exécutent sur une batterie de serveurs de plusieurs serveurs devraient vous assurer que les sessions sont bloquées lors de l’utilisation du cache en mémoire. Les sessions rémanentes garantissent que les demandes d’un client vont vers le même serveur. Par exemple, les applications web Azure utilisent [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) pour router toutes les demandes vers le même serveur.

Les sessions non rémanentes dans une batterie de serveurs web nécessitent un [cache distribué](distributed.md) pour éviter les problèmes de cohérence du cache. Pour certaines applications, un cache distribué peut prendre en charge un sclae-out plus important qu'un cache en mémoire. Il permet de décharger la mémoire cache vers un processus externe.

::: moniker range="< aspnetcore-2.0"

Le cache `IMemoryCache` supprime les entrées du cache mémoire insuffisante, sauf si la [priorité de cache](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) est définie à `CacheItemPriority.NeverRemove`. Vous pouvez définir le `CacheItemPriority` pour ajuster la priorité avec laquelle le cache évince des éléments avec une mémoire insuffisante.

::: moniker-end

Le cache en mémoire permet de stocker n’importe quel objet ; l’interface de cache distribué est limitée à `byte[]`. Les éléments de cache de magasin de cache en mémoire et distribuée en tant que paires clé-valeur.

## <a name="systemruntimecachingmemorycache"></a>System.Runtime.Caching/MemoryCache

<xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([Package NuGet](https://www.nuget.org/packages/System.Runtime.Caching/)) peut être utilisé avec :

* .NET standard 2.0 ou version ultérieure.
* N’importe quel [implémentation .NET](/dotnet/standard/net-standard#net-implementation-support) qui cible .NET Standard 2.0 ou version ultérieure. Par exemple, ASP.NET Core 2.0 ou version ultérieure.
* .NET framework 4.5 ou version ultérieure.

[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) / `IMemoryCache` (décrite dans cet article) est recommandée sur `System.Runtime.Caching` / `MemoryCache` , car il est mieux intégrée dans ASP.NET Core. Par exemple, `IMemoryCache` fonctionne en mode natif avec [l’injection de dépendances](xref:fundamentals/dependency-injection) ASP.NET Core.

Utilisez `System.Runtime.Caching` / `MemoryCache` comme un pont de compatibilité lors du portage du code à partir d’ASP.NET 4.x vers ASP.NET Core.

## <a name="cache-guidelines"></a>Recommandations de cache

* Code doit systématiquement posséder une option de secours pour extraire des données et **pas** dépendent d’une valeur mise en cache qui est disponible.
* Le cache utilise une ressource rare, la mémoire. Limiter la croissance de cache :
  * Faire **pas** utiliser entrée externe en tant que clés de cache.
  * Utiliser des délais d’expiration pour limiter la croissance du cache.
  * [Permet de limiter la taille du cache SetSize, la taille et SizeLimit](#use-setsize-size-and-sizelimit-to-limit-cache-size). Le runtime ASP.NET Core ne limite pas la taille du cache en fonction de la sollicitation de la mémoire. Il incombe au développeur de limiter la taille du cache.

## <a name="using-imemorycache"></a>À l’aide de IMemoryCache

La mise en cache en mémoire est un *service* qui est référencé à partir de votre application à l’aide de l'[Injection de dépendance](../../fundamentals/dependency-injection.md). Appelez `AddMemoryCache` dans `ConfigureServices`:

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

Faire appel à l'instance `IMemoryCache` dans le constructeur :

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

`IMemoryCache` nécessite le package NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

`IMemoryCache` nécessite le package NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), qui est disponible dans le [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage).

::: moniker-end

::: moniker range="> aspnetcore-2.0"

`IMemoryCache` nécessite le package NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), qui est disponible dans le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app).

::: moniker-end

Le code suivant utilise [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) pour vérifier si une heure est dans le cache. Si une heure n’est pas mis en cache, une nouvelle entrée est créée et ajoutée au cache avec [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).

[!code-csharp [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

L’heure actuelle et l’heure de mise en cache s’affichent :

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

La mise en cache `DateTime` valeur reste dans le cache, bien qu’il existe des demandes dans le délai imparti. L’illustration ci-dessous indique l’heure actuelle et une heure antérieure récupérées du cache :

![Vue index avec deux heures différentes affichées](memory/_static/time.png)

Le code suivant utilise [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) et [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) pour mettre en cache des données.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Le code suivant appelle [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) pour extraire l’heure de mise en cache :

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*> , <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>, et [obtenir](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) font partie de méthodes d’extension de la [CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) classe qui étend les fonctionnalités de <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>. Consultez [IMemoryCache méthodes](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) et [CacheExtensions méthodes](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) pour obtenir une description des autres méthodes de cache.

## <a name="memorycacheentryoptions"></a>MemoryCacheEntryOptions

L’exemple suivant :

* Définit l’heure d’expiration absolue. Ceci est la durée maximale pendant laquelle l’entrée peut être mise en cache et empêche l’élément de devenir trop périmé quand l’expiration glissante est constamment renouvelée.
* Définit un délai d’expiration glissant. Les requêtes qui accèdent à cet élément de mise en cache réinitialisent l’horloge d’expiration glissante.
* Définit la priorité de cache sur `CacheItemPriority.NeverRemove`.
* Définit un [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) qui est appelé après la suppression de l’entrée du cache. Le rappel est exécuté sur un thread différent du code qui supprime l’élément à partir du cache.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

::: moniker range=">= aspnetcore-2.0"

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a>Utiliser SetSize, Size et SizeLimit pour limiter la taille du cache

Une instance `MemoryCache` peut éventuellement spécifier et appliquer une limite de taille. La limite de taille de mémoire n’a pas d’unité de mesure, car le cache dispose d’aucun mécanisme pour mesurer le nombre d’entrées. Si la limite de taille de mémoire cache est définie, toutes les entrées doivent spécifier taille. Le runtime ASP.NET Core ne limite pas la taille du cache en fonction de la sollicitation de la mémoire. Il incombe au développeur de limiter la taille du cache. La taille spécifiée est exprimé en unités choisies par le développeur.

Exemple :

* Si l’application web met en cache principalement les chaînes, chaque taille d’entrée du cache peut être la longueur de chaîne.
* L’application peut spécifier la taille de toutes les entrées en tant que 1 et la limite de taille est le nombre d’entrées.

Le code suivant crée un [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) de taille fixe sans unité, accessible par [l’injection de dépendances](xref:fundamentals/dependency-injection):

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) n’a pas d’unités. Les entrées mises en cache doivent indiquer la taille dans les unités jugées les plus appropriées si la taille de la mémoire cache a été définie. Tous les utilisateurs d’une instance de cache doivent utiliser le même système d’unité. Une entrée n’est pas mise en cache si la somme des tailles des entrées mises en cache dépasse la valeur spécifiée par `SizeLimit`. Si aucune limite de taille du cache n’est définie, la taille de cache définie sur l’entrée est ignorée.

Le code suivant inscrit `MyMemoryCache` avec le conteneur [d’injection de dépendances](xref:fundamentals/dependency-injection).

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

`MyMemoryCache` est créé comme cache de mémoire indépendant pour les composants qui sont informés de la taille limitée du cache et qui ont la capacité de définir une taille d’entrée de cache en conséquence.

Le code suivant utilise `MyMemoryCache`:

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

La taille de l’entrée de cache peut être définie par [Size](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) ou par la méthode d’extension [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) :

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

::: moniker-end

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
