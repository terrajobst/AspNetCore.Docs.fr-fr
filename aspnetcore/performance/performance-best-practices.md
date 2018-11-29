---
title: ASP.NET Core performances meilleures pratiques
author: mjrousos
description: Conseils pour améliorer les performances dans les applications ASP.NET Core et éviter les problèmes de performances courants.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 11/29/2018
uid: performance/performance-best-practices
ms.openlocfilehash: 9f3ed97bf4d4eb371ff5ae3874234b44745cc4ca
ms.sourcegitcommit: 0fc89b80bb1952852ecbcf3c5c156459b02a6ceb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52618114"
---
# <a name="aspnet-core-performance-best-practices"></a>ASP.NET Core performances meilleures pratiques

Par [Mike Rousos](https://github.com/mjrousos)

Cette rubrique fournit des instructions pour des performances meilleures pratiques avec ASP.NET Core.

<a name="hot"></a>
<!-- TODO review hot code paths is jargon that won't MT (machine translate) and is not well defined for native speakers. --> Dans ce document, un chemin d’accès du code à chaud est défini comme un chemin d’accès de code qui est fréquemment appelé et où une grande partie de la durée d’exécution se produit. Chemins d’accès du code à chaud limitent généralement l’application scale-out et performances.

## <a name="cache-aggressively"></a>Mettre en cache agressive

La mise en cache est abordée en plusieurs parties de ce document. Pour plus d'informations, consultez <xref:performance/caching/response>.

## <a name="avoid-blocking-calls"></a>Éviter de bloquer les appels

Les applications ASP.NET Core doivent être conçues pour traiter de nombreuses requêtes simultanément. Les API asynchrones permettent un petit pool de threads pour traiter des milliers de demandes simultanées par ne pas en attente sur le blocage d’appels. Au lieu d’attendre sur une tâche synchrone longue se termine, le thread peut travailler sur une autre demande.

Un problème de performances courants dans les applications ASP.NET Core bloque les appels qui peut être asynchrones. Nombre d’appels blocage synchrone mène à [privation de Thread de Pool](https://blogs.msdn.microsoft.com/vancem/2018/10/16/diagnosing-net-core-threadpool-starvation-with-perfview-why-my-service-is-not-saturating-all-cores-or-seems-to-stall/) et affecter les temps de réponse.

**Ne le faites pas**:

* Bloquer l’exécution asynchrone en appelant [Task.Wait](/dotnet/api/system.threading.tasks.task.wait) ou [Task.Result](/dotnet/api/system.threading.tasks.task-1.result).
* Acquérir des verrous dans les chemins de code commune. Les applications ASP.NET Core sont plus performantes lorsque conçu pour exécuter du code en parallèle.

**Faire**:

* Rendre [à chaud des chemins de code](#hot) asynchrone.
* Appeler l’accès aux données et les opérations longues API de façon asynchrone.
* Rendre le contrôleur/Razor actions sur la Page asynchrone. La pile d’appel entière doit être asynchrone pour tirer parti de [async/await](/dotnet/csharp/programming-guide/concepts/async/) modèles.

Un profileur comme [PerfView](https://github.com/Microsoft/perfview) peut être utilisé pour rechercher des threads fréquemment ajoutés à la [du Pool de threads](/windows/desktop/procthread/thread-pool). Le `Microsoft-Windows-DotNETRuntime/ThreadPoolWorkerThread/Start` événement indique un thread qui est ajouté au pool de threads. <!--  For more information, see [async guidance docs](TBD-Link_To_Davifowl_Doc  -->

## <a name="minimize-large-object-allocations"></a>Réduire les allocations d’objets volumineux

<!-- TODO review Bill - replaced original .NET language below with .NET Core since this targets .NET Core --> Le [RÉCUPÉRATEUR de mémoire .NET Core](/dotnet/standard/garbage-collection/) gère l’allocation et libération de mémoire automatiquement dans les applications ASP.NET Core. Garbage collection automatique signifie généralement que les développeurs n’ont à se soucier de comment ou quand la mémoire est libérée. Toutefois, nettoyage des objets non référencés du temps processeur, donc les développeurs doivent réduire l’allocation d’objets dans [à chaud des chemins de code](#hot). Le garbage collection est particulièrement chères sur des objets volumineux (> 85 Ko). Les objets volumineux sont stockés sur le [tas d’objets volumineux](/dotnet/standard/garbage-collection/large-object-heap) et nécessitent un intégral (génération 2) le garbage collection pour nettoyer. Contrairement à la génération 0 et les collections de génération 1, une collection de génération 2 nécessite l’exécution d’application doit être temporairement suspendue. Allocation de fréquente et une désallocation des objets volumineux peuvent entraîner des performances incohérent.

Recommandations :

* **Faire** envisager la mise en cache des objets volumineux qui sont fréquemment utilisées. La mise en cache des objets volumineux empêche les allocations coûteuses.
* **Faire** pool de mémoires tampons en utilisant un [ `ArrayPool<T>` ](/dotnet/api/system.buffers.arraypool-1) pour stocker des tableaux volumineux.
* **Ne le faites pas** allouer nombreux, de courte durée de vie des objets volumineux sur [à chaud des chemins de code](#hot).

Problèmes de mémoire, comme l’exemple précédent peut être diagnostiqué en examinant les statistiques des garbage collection (GC) dans [PerfView](https://github.com/Microsoft/perfview) et examen :

* Temps de pause de garbage collection.
* Quel pourcentage du temps processeur consacré au garbage collection.
* Combien de garbage collection sont la génération 0, 1 et 2.

Pour plus d’informations, consultez [le Garbage Collection et performances](/dotnet/standard/garbage-collection/performance).

## <a name="optimize-data-access"></a>Optimiser l’accès aux données

Interactions avec un magasin de données ou d’autres services à distance sont souvent la partie la plus lente d’une application ASP.NET Core. Lire et écrire les données efficacement sont essentiel pour de bonnes performances.

Recommandations :

* **Faire** appeler toutes les API d’accès de données de façon asynchrone.
* **Ne le faites pas** récupérer plus de données que nécessaire. Écrire des requêtes pour retourner uniquement les données qui sont nécessaires pour la requête HTTP actuelle.
* **Faire** envisager la mise en cache fréquemment accédé à des données récupérées à partir d’une base de données ou d’un service distant s’il est acceptable pour les données soient légèrement obsolètes. Selon le scénario, vous pouvez utiliser un [MemoryCache](xref:performance/caching/memory) ou un [DistributedCache](xref:performance/caching/distributed). Pour plus d'informations, consultez <xref:performance/caching/response>.
* Réduire les allers-retours réseau. L’objectif est de récupérer toutes les données qui seront nécessaires dans un seul appel plutôt que plusieurs appels.
* **Faire** utiliser [pas de suivi des requêtes](/ef/core/querying/tracking#no-tracking-queries) dans Entity Framework Core lors de l’accès aux données en lecture seule. EF Core peut retourner les résultats de pas de suivi des requêtes plus efficacement.
* **Faire** filtre et des requêtes d’agrégation LINQ (avec `.Where`, `.Select`, ou `.Sum` instructions, par exemple) afin que le filtrage est effectué par la base de données.
* **Faire** envisager que EF Core résout certains opérateurs de requête sur le client, ce qui peut entraîner l’exécution de requête inefficace. Pour plus d’informations, consultez [problèmes de performances de Client d’évaluation](/ef/core/querying/client-eval#client-evaluation-performance-issues)
* **Ne le faites pas** utiliser des requêtes de projection sur les collections, ce qui peuvent entraîner l’exécution de « N + 1 » des requêtes SQL. Pour plus d’informations, consultez [optimisation des sous-requêtes corrélées](/ef/core/what-is-new/ef-core-2.1#optimization-of-correlated-subqueries).

Consultez [EF hautes performances](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries) pour les approches qui peuvent améliorer les performances dans les applications à grande échelle :

* [Le regroupement DbContext](/ef/core/what-is-new/ef-core-2.0#dbcontext-pooling)
* [Requêtes compilées explicitement](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries)

Nous vous recommandons de que vous mesurez l’impact des approches hautes performances précédents avant de valider votre base de code. La complexité supplémentaire des requêtes compilées ne justifie pas l’amélioration des performances.

Requête problèmes peuvent être détectés en examinant le temps passé à l’accès aux données avec [Application Insights](/azure/application-insights/app-insights-overview) ou avec des outils de profilage. La plupart des bases de données également à disposition des statistiques concernant les requêtes fréquemment exécutées.

## <a name="pool-http-connections-with-httpclientfactory"></a>Connexions de pool HTTP avec HttpClientFactory

Bien que [HttpClient](/dotnet/api/system.net.http.httpclient?view=netstandard-2.0) implémente le `IDisposable` interface, il est destiné à être réutilisé. Fermé `HttpClient` instances laisser les sockets ouverts dans le `TIME_WAIT` état pendant une courte période de temps. Par conséquent, si un chemin d’accès du code qui crée et supprime `HttpClient` objets est fréquemment utilisée, l’application peut épuiser sockets disponibles. [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) a été introduit dans ASP.NET Core 2.1 en tant que solution à ce problème. Il gère le regroupement de connexions HTTP afin d’optimiser les performances et la fiabilité.

Recommandations :

* **Ne le faites pas** créer et supprimer des `HttpClient` directement les instances.
* **Faire** utiliser [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) pour récupérer `HttpClient` instances. Pour plus d’informations, consultez [HttpClientFactory d’utilisation pour implémenter des requêtes HTTP résilientes](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests).

## <a name="keep-common-code-paths-fast"></a>Chemins de code courantes rapide

Vous voulez tout votre code pour être rapide, mais les chemins de code fréquemment appelées sont les plus importantes pour optimiser :

* Composants d’intergiciel (middleware) dans le pipeline de traitement de demande de l’application, en particulier l’intergiciel (middleware) exécutez tôt dans le pipeline. Ces composants ont un impact important sur les performances.
* Code qui est exécuté pour chaque requête ou plusieurs fois par demande. Par exemple, la journalisation personnalisée, les gestionnaires d’autorisation ou l’initialisation des services temporaires.

Recommandations :

* **Ne le faites pas** utiliser des composants d’intergiciel (middleware) personnalisé avec des tâches longues.
* **Faire** utiliser les outils de profilage des performances (tels que [les outils de Diagnostic de Visual Studio](/visualstudio/profiling/profiling-feature-tour) ou [PerfView](https://github.com/Microsoft/perfview)) pour identifier les [à chaud des chemins de code](#hot).

## <a name="complete-long-running-tasks-outside-of-http-requests"></a>Effectuer de longues tâches en dehors des requêtes HTTP

La plupart des demandes à une application ASP.NET Core peuvent être gérées par un contrôleur ou un modèle de page appelant les services nécessaires et renvoyer une réponse HTTP. Pour certaines requêtes qui impliquent des tâches longues, il est préférable rendre le processus de demande-réponse asynchrone.

Recommandations :

* **Ne le faites pas** attendre des tâches longues à effectuer dans le cadre du traitement des requêtes HTTP normal.
* **Faire** prendre en compte la gestion des requêtes longues avec [services d’arrière-plan](/aspnet/core/fundamentals/host/hosted-services) ou en dehors du processus avec un [Azure Function](/azure/azure-functions/). Fin de travail out-of-process est particulièrement utile pour les tâches sollicitant beaucoup le processeur.
* **Faire** utiliser les options de communication en temps réel telles que [SignalR](xref:signalr/introduction) pour communiquer avec les clients de façon asynchrone.

## <a name="minify-client-assets"></a>Réduction des ressources du client

Les applications ASP.NET Core avec les serveurs frontaux complexes servent généralement de nombreux JavaScript, CSS ou les fichiers image. Performances des demandes de charge initiale peut être améliorée par :

* Regroupement, qui combine plusieurs fichiers en une seule.
* Minimisation, ce qui réduit la taille des fichiers par.

Recommandations :

* **Faire** utiliser d’ASP.NET Core [prise en charge intégrée](xref:client-side/bundling-and-minification) pour le regroupement et minimisation des ressources du client.
* **Faire** envisager d’autres outils tiers tels que [Gulp](uid:client-side/bundling-and-minification#consume-bundleconfigjson-from-gulp) ou [Webpack](https://webpack.js.org/) pour la gestion des ressources client plus complexe.

## <a name="use-the-latest-aspnet-core-release"></a>Utilisez la dernière version d’ASP.NET Core

Chaque nouvelle version d’ASP.NET inclut des améliorations de performances. Optimisations dans .NET Core et ASP.NET Core signifient que les versions plus récentes seront plus performantes que les versions antérieures. Par exemple, .NET Core 2.1 prise en charge pour les expressions régulières compilées et bénéficié de [ `Span<T>` ](https://msdn.microsoft.com/magazine/mt814808.aspx). ASP.NET Core 2.2 ajouté prise en charge HTTP/2. Si les performances sont une priorité, envisagez la mise à niveau vers la version la plus récente d’ASP.NET Core.

<!-- TODO review link and taking advantage of new [performance features](#TBD)
Maybe skip this TBD link as each version will have perf improvements -->

## <a name="minimize-exceptions"></a>Réduire les exceptions

Les exceptions doivent être rares. Levée et l’interception des exceptions sont lent par rapport à d’autres modèles de flux de code. Pour cette raison, les exceptions ne doivent pas être utilisées pour contrôler le flux normal du programme.

Recommandations :

* **Ne le faites pas** utilisation lever ou intercepter des exceptions comme un moyen de flux normal du programme, en particulier dans les chemins d’accès du code à chaud.
* **Faire** inclure une logique dans l’application pour détecter et gérer des conditions qui provoquent une exception.
* **Faire** lever ou intercepter des exceptions pour des conditions inhabituelles ou inattendues.

Outils de diagnostic d’application (par exemple, Application Insights) peuvent aider à identifier les exceptions courantes dans une application qui peut affecter les performances.