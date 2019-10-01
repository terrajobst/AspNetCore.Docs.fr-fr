---
title: Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core
author: guardrex
description: Découvrez comment implémenter des tâches d’arrière-plan avec des services hébergés dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/26/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 0eaa3a62370c1e413840bb65f597dc664adafc38
ms.sourcegitcommit: fe88748b762525cb490f7e39089a4760f6a73a24
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71688101"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

Dans ASP.NET Core, les tâches d’arrière-plan peuvent être implémentées en tant que *services hébergés*. Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>. Cette rubrique contient trois exemples de service hébergé :

* Tâche d’arrière-plan qui s’exécute sur un minuteur.
* Service hébergé qui active un [service délimité](xref:fundamentals/dependency-injection#service-lifetimes). Le service étendu peut utiliser l' [injection de dépendances (di)](xref:fundamentals/dependency-injection).
* Tâches d’arrière-plan en file d’attente qui s’exécutent séquentiellement.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

Cet exemple d’application est fourni en deux versions :

* Hôte web &ndash; L’hôte web est utile pour l’hébergement d’applications web. L’exemple de code présenté dans cette rubrique provient de la version de l’hôte web de l’exemple. Pour plus d’informations, consultez la rubrique [Hôte web](xref:fundamentals/host/web-host).
* Hôte générique &ndash; L’hôte générique est une nouveauté d’ASP.NET Core 2.1. Pour plus d’informations, consultez la rubrique [Hôte générique](xref:fundamentals/host/generic-host).

## <a name="worker-service-template"></a>Modèle Service Worker

Le modèle Service Worker ASP.NET Core fournit un point de départ pour l’écriture d’applications de service durables. Pour utiliser le modèle en tant que base d’une application de services hébergés :

[!INCLUDE[](~/includes/worker-template-instructions.md)]

---

## <a name="package"></a>Package

Une référence de package au package [Microsoft. extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) est ajoutée implicitement pour les applications ASP.net core.

## <a name="ihostedservice-interface"></a>Interface IHostedService

L' <xref:Microsoft.Extensions.Hosting.IHostedService> interface définit deux méthodes pour les objets gérés par l’hôte :

* [StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash;`StartAsync` contient la logique pour démarrer la tâche d’arrière-plan. `StartAsync`est appelé *avant*:

  * Le pipeline de traitement des demandes de l'`Startup.Configure`application est configuré ().
  * Le serveur est démarré et [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) est déclenché.

  Le comportement par défaut peut être modifié de `StartAsync` sorte que le service hébergé s’exécute après que le pipeline de l’application a été configuré et `ApplicationStarted` qu’il est appelé. Pour modifier le comportement par défaut, ajoutez le service hébergé`VideosWatcher` (dans l’exemple suivant) après `ConfigureWebHostDefaults`l’appel de :

  ```csharp
  using Microsoft.AspNetCore.Hosting;
  using Microsoft.Extensions.DependencyInjection;
  using Microsoft.Extensions.Hosting;

  public class Program
  {
      public static void Main(string[] args)
      {
          CreateHostBuilder(args).Build().Run();
      }

      public static IHostBuilder CreateHostBuilder(string[] args) =>
          Host.CreateDefaultBuilder(args)
              .ConfigureWebHostDefaults(webBuilder =>
              {
                  webBuilder.UseStartup<Startup>();
              })
              .ConfigureServices(services =>
              {
                  services.AddHostedService<VideosWatcher>();
              });
  }
  ```

* [StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash;, déclenchée quand l’hôte effectue un arrêt normal. `StopAsync` contient la logique pour terminer la tâche d’arrière-plan. Implémentez <xref:System.IDisposable> et les [finaliseurs (destructeurs)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) pour supprimer toutes les ressources non managées.

  Le jeton d’annulation a un délai d’expiration par défaut de cinq secondes pour indiquer que le processus d’arrêt ne doit plus être normal. Quand l’annulation est demandée sur le jeton :

  * Les opérations en arrière-plan restantes effectuées par l’application doivent être abandonnées.
  * Les méthodes appelées dans `StopAsync` doivent retourner rapidement.

  Cependant, les tâches ne sont pas abandonnées après la demande d’annulation : l’appelant attend que toutes les tâches se terminent.

  Si l’application s’arrête inopinément (par exemple en cas d’échec du processus de l’application), `StopAsync` n’est probablement pas appelée. Par conséquent, les méthodes appelées ou les opérations effectuées dans `StopAsync` peuvent ne pas se produire.

  Pour prolonger le délai d’expiration par défaut de cinq secondes, définissez :

  * <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> quand vous utilisez l’hôte générique. Pour plus d'informations, consultez <xref:fundamentals/host/generic-host#shutdown-timeout>.
  * Le paramètre de configuration du délai d’expiration de l’hôte quand vous utilisez l’hôte web. Pour plus d'informations, consultez <xref:fundamentals/host/web-host#shutdown-timeout>.

Le service hébergé est activé une seule fois au démarrage de l’application et s’arrête normalement à l’arrêt de l’application. Si une erreur est levée pendant l’exécution des tâches d’arrière-plan, `Dispose` doit être appelée même si `StopAsync` n’est pas appelée.

## <a name="backgroundservice"></a>BackgroundService

`BackgroundService`est une classe de base pour l’implémentation d' <xref:Microsoft.Extensions.Hosting.IHostedService>une exécution longue. `BackgroundService`fournit la `ExecuteAsync(CancellationToken stoppingToken)` méthode abstraite pour contenir la logique du service. La `stoppingToken` est déclenchée lorsque [IHostedService. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) est appelé. L’implémentation de cette méthode retourne un `Task` qui représente la durée de vie totale du service d’arrière-plan.

En outre, *vous pouvez* également substituer les méthodes définies `IHostedService` sur pour exécuter le code de démarrage et d’arrêt de votre service :

* `StopAsync(CancellationToken cancellationToken)`&ndash; estappelélorsquel’hôted’application`StopAsync` effectue un arrêt approprié. Le `cancellationToken` est signalé lorsque l’hôte décide de forcer l’arrêt du service. Si cette méthode est substituée, vous **devez** appeler (et `await`) la méthode de classe de base pour vérifier que le service s’arrête correctement.
* `StartAsync(CancellationToken cancellationToken)`&ndash; estappelépourdémarrerleservice`StartAsync` d’arrière-plan. Le `cancellationToken` est signalé si le processus de démarrage est interrompu. L’implémentation retourne un `Task` qui représente le processus de démarrage pour le service. Aucun autre service n’est démarré jusqu' `Task` à ce que cela se termine. Si cette méthode est substituée, vous **devez** appeler (et `await`) la méthode de classe de base pour vous assurer que le service démarre correctement.

## <a name="timed-background-tasks"></a>Tâche d’arrière-plan avec minuteur

Une tâche d’arrière-plan avec minuteur utilise la classe [System.Threading.Timer](xref:System.Threading.Timer). Le minuteur déclenche la méthode `DoWork` de la tâche. Le minuteur est désactivé sur `StopAsync` et supprimé quand le conteneur du service est supprimé sur `Dispose` :

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

Le service est inscrit dans `IHostBuilder.ConfigureServices` (*Program.cs*) avec la `AddHostedService` méthode d’extension :

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Utilisation d’un service délimité dans une tâche d’arrière-plan

Pour utiliser les [services délimités](xref:fundamentals/dependency-injection#service-lifetimes) dans un `BackgroundService`, créez une étendue. Par défaut, aucune étendue n’est créée pour un service hébergé.

Le service des tâches d’arrière-plan délimitées contient la logique de la tâche d’arrière-plan. Dans l’exemple suivant :

* Le service est asynchrone. La méthode `DoWork` retourne un `Task`. À des fins de démonstration, un délai de dix secondes est attendu dans `DoWork` la méthode.
* Un <xref:Microsoft.Extensions.Logging.ILogger> est injecté dans le service.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

Le service hébergé crée une étendue pour résoudre le service de tâche d’arrière-plan étendu `DoWork` afin d’appeler sa méthode. `DoWork`retourne un `Task`, qui est attendu dans `ExecuteAsync`:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

Les services sont inscrits dans `IHostBuilder.ConfigureServices` (*Program.cs*). Le service hébergé est inscrit avec la `AddHostedService` méthode d’extension :

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Tâches d’arrière-plan en file d’attente

Une file d’attente de tâches en arrière-plan est basée <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> sur le .net 4. x ([prévu provisoirement pour être intégré pour ASP.net Core](https://github.com/aspnet/Hosting/issues/1280)) :

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

Dans l’exemple `QueueHostedService` suivant :

* La `BackgroundProcessing` méthode retourne un `Task`, qui est attendu dans `ExecuteAsync`.
* Les tâches en arrière-plan de la file d’attente sont `BackgroundProcessing`déplacées en file d’attente et exécutées dans.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28,39-40,44)]

Un `MonitorLoop` service gère les tâches de mise en file d’attente pour le `w` service hébergé chaque fois que la clé est sélectionnée sur un appareil d’entrée :

* Est injecté dans le `MonitorLoop` service. `IBackgroundTaskQueue`
* `IBackgroundTaskQueue.QueueBackgroundWorkItem`est appelé pour empiler un élément de travail.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

Les services sont inscrits dans `IHostBuilder.ConfigureServices` (*Program.cs*). Le service hébergé est inscrit avec la `AddHostedService` méthode d’extension :

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Dans ASP.NET Core, les tâches d’arrière-plan peuvent être implémentées en tant que *services hébergés*. Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>. Cette rubrique contient trois exemples de service hébergé :

* Tâche d’arrière-plan qui s’exécute sur un minuteur.
* Service hébergé qui active un [service délimité](xref:fundamentals/dependency-injection#service-lifetimes). Le service étendu peut utiliser l' [injection de dépendances (di)](xref:fundamentals/dependency-injection)
* Tâches d’arrière-plan en file d’attente qui s’exécutent séquentiellement.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

Cet exemple d’application est fourni en deux versions :

* Hôte web &ndash; L’hôte web est utile pour l’hébergement d’applications web. L’exemple de code présenté dans cette rubrique provient de la version de l’hôte web de l’exemple. Pour plus d’informations, consultez la rubrique [Hôte web](xref:fundamentals/host/web-host).
* Hôte générique &ndash; L’hôte générique est une nouveauté d’ASP.NET Core 2.1. Pour plus d’informations, consultez la rubrique [Hôte générique](xref:fundamentals/host/generic-host).

## <a name="package"></a>Package

Référencer le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ou ajouter une référence de package au package [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting).

## <a name="ihostedservice-interface"></a>Interface IHostedService

Les services hébergés implémentent l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>. L’interface définit deux méthodes pour les objets qui sont gérés par l’hôte :

* [StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash;`StartAsync` contient la logique pour démarrer la tâche d’arrière-plan. Quand [l’hôte web](xref:fundamentals/host/web-host) est utilisé, `StartAsync` est appelé après le démarrage du serveur et le déclenchement [d’IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*). Quand [l’hôte générique](xref:fundamentals/host/generic-host) est utilisé, `StartAsync` est appelé avant le déclenchement de `ApplicationStarted`.

* [StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash;, déclenchée quand l’hôte effectue un arrêt normal. `StopAsync` contient la logique pour terminer la tâche d’arrière-plan. Implémentez <xref:System.IDisposable> et les [finaliseurs (destructeurs)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) pour supprimer toutes les ressources non managées.

  Le jeton d’annulation a un délai d’expiration par défaut de cinq secondes pour indiquer que le processus d’arrêt ne doit plus être normal. Quand l’annulation est demandée sur le jeton :

  * Les opérations en arrière-plan restantes effectuées par l’application doivent être abandonnées.
  * Les méthodes appelées dans `StopAsync` doivent retourner rapidement.

  Cependant, les tâches ne sont pas abandonnées après la demande d’annulation : l’appelant attend que toutes les tâches se terminent.

  Si l’application s’arrête inopinément (par exemple en cas d’échec du processus de l’application), `StopAsync` n’est probablement pas appelée. Par conséquent, les méthodes appelées ou les opérations effectuées dans `StopAsync` peuvent ne pas se produire.

  Pour prolonger le délai d’expiration par défaut de cinq secondes, définissez :

  * <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> quand vous utilisez l’hôte générique. Pour plus d'informations, consultez <xref:fundamentals/host/generic-host#shutdown-timeout>.
  * Le paramètre de configuration du délai d’expiration de l’hôte quand vous utilisez l’hôte web. Pour plus d'informations, consultez <xref:fundamentals/host/web-host#shutdown-timeout>.

Le service hébergé est activé une seule fois au démarrage de l’application et s’arrête normalement à l’arrêt de l’application. Si une erreur est levée pendant l’exécution des tâches d’arrière-plan, `Dispose` doit être appelée même si `StopAsync` n’est pas appelée.

## <a name="timed-background-tasks"></a>Tâche d’arrière-plan avec minuteur

Une tâche d’arrière-plan avec minuteur utilise la classe [System.Threading.Timer](xref:System.Threading.Timer). Le minuteur déclenche la méthode `DoWork` de la tâche. Le minuteur est désactivé sur `StopAsync` et supprimé quand le conteneur du service est supprimé sur `Dispose` :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

Le service est inscrit dans `Startup.ConfigureServices` avec la méthode d’extension `AddHostedService` :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Utilisation d’un service délimité dans une tâche d’arrière-plan

Pour utiliser des [services délimités](xref:fundamentals/dependency-injection#service-lifetimes) au sein d’un `IHostedService`, créez une étendue. Par défaut, aucune étendue n’est créée pour un service hébergé.

Le service des tâches d’arrière-plan délimitées contient la logique de la tâche d’arrière-plan. Dans l’exemple suivant, un <xref:Microsoft.Extensions.Logging.ILogger> est injecté dans le service :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

Le service hébergé crée une étendue pour résoudre le service des tâches d’arrière-plan délimitées pour appeler sa méthode `DoWork` :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

Les services sont inscrits dans `Startup.ConfigureServices`. L’implémentation `IHostedService` est inscrite avec la méthode d’extension `AddHostedService` :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Tâches d’arrière-plan en file d’attente

Une file d’attente de tâches en arrière-plan est basée <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> sur le .net 4. x ([prévu provisoirement pour être intégré pour ASP.net Core](https://github.com/aspnet/Hosting/issues/1280)) :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

Dans `QueueHostedService`, les tâches d’arrière-plan dans la file d’attente sont retirées de la file d’attente et exécutées en tant que <xref:Microsoft.Extensions.Hosting.BackgroundService>, qui est une classe de base pour l’implémentation d’une exécution longue de `IHostedService` :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

Les services sont inscrits dans `Startup.ConfigureServices`. L’implémentation `IHostedService` est inscrite avec la méthode d’extension `AddHostedService` :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

Dans la classe de modèle de page Index :

* `IBackgroundTaskQueue` est injecté dans le constructeur et affecté à `Queue`.
* Un <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> est injecté et affecté à `_serviceScopeFactory`. La fabrique est utilisée pour créer des instances de <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, qui servent à créer des services au sein d’une étendue. Une étendue est créée de façon à utiliser l’élément `AppDbContext` ([service délimité](xref:fundamentals/dependency-injection#service-lifetimes)) de l’application pour écrire des enregistrements de base de données dans `IBackgroundTaskQueue` (service singleton).

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

Quand le bouton **Ajouter une tâche** est sélectionné dans la page Index, la méthode `OnPostAddTask` est exécutée. `QueueBackgroundWorkItem`est appelé pour empiler un élément de travail :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a>Ressources supplémentaires

* [Implémenter des tâches d’arrière-plan dans des microservices avec IHostedService et la classe BackgroundService](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
