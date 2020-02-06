---
title: Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core
author: guardrex
description: Découvrez comment implémenter des tâches d’arrière-plan avec des services hébergés dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/05/2020
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 6a88e56afc4fb1b4f673c362f83d948eda84b930
ms.sourcegitcommit: bd896935e91236e03241f75e6534ad6debcecbbf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77044885"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex) et [Jeow Li Huan](https://github.com/huan086)

::: moniker range=">= aspnetcore-3.0"

Dans ASP.NET Core, les tâches d’arrière-plan peuvent être implémentées en tant que *services hébergés*. Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>. Cette rubrique contient trois exemples de service hébergé :

* Tâche d’arrière-plan qui s’exécute sur un minuteur.
* Service hébergé qui active un [service délimité](xref:fundamentals/dependency-injection#service-lifetimes). Le service étendu peut utiliser l' [injection de dépendances (di)](xref:fundamentals/dependency-injection).
* Tâches d’arrière-plan en file d’attente qui s’exécutent séquentiellement.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="worker-service-template"></a>Modèle Service Worker

Le modèle Service Worker ASP.NET Core fournit un point de départ pour l’écriture d’applications de service durables. Une application créée à partir du modèle de service Worker spécifie le kit de développement logiciel (SDK) Worker dans son fichier projet :

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

Pour utiliser le modèle en tant que base d’une application de services hébergés :

[!INCLUDE[](~/includes/worker-template-instructions.md)]

## <a name="package"></a>Package

Une application basée sur le modèle de service worker utilise le kit de développement logiciel (SDK) `Microsoft.NET.Sdk.Worker` et possède une référence de package explicite au package [Microsoft. extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) . Par exemple, consultez le fichier projet de l’exemple d’application (*BackgroundTasksSample. csproj*).

Pour les applications Web qui utilisent le kit de développement logiciel (SDK) `Microsoft.NET.Sdk.Web`, le package [Microsoft. extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) est référencé implicitement à partir de l’infrastructure partagée. Une référence de package explicite dans le fichier projet de l’application n’est pas obligatoire.

## <a name="ihostedservice-interface"></a>Interface IHostedService

L’interface <xref:Microsoft.Extensions.Hosting.IHostedService> définit deux méthodes pour les objets gérés par l’hôte :

* [StartAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contient la logique de démarrage de la tâche en arrière-plan. `StartAsync` est appelé *avant*:

  * Le pipeline de traitement des demandes de l’application est configuré (`Startup.Configure`).
  * Le serveur est démarré et [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) est déclenché.

  Le comportement par défaut peut être modifié de sorte que le `StartAsync` du service hébergé s’exécute une fois que le pipeline de l’application a été configuré et que `ApplicationStarted` est appelé. Pour modifier le comportement par défaut, ajoutez le service hébergé (`VideosWatcher` dans l’exemple suivant) après avoir appelé `ConfigureWebHostDefaults`:

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

* [StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; déclenché lorsque l’ordinateur hôte effectue un arrêt approprié. `StopAsync` contient la logique pour terminer la tâche d’arrière-plan. Implémentez <xref:System.IDisposable> et les [finaliseurs (destructeurs)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) pour supprimer toutes les ressources non managées.

  Le jeton d’annulation a un délai d’expiration par défaut de cinq secondes pour indiquer que le processus d’arrêt ne doit plus être normal. Quand l’annulation est demandée sur le jeton :

  * Les opérations en arrière-plan restantes effectuées par l’application doivent être abandonnées.
  * Les méthodes appelées dans `StopAsync` doivent retourner rapidement.

  Cependant, les tâches ne sont pas abandonnées après la demande d’annulation : l’appelant attend que toutes les tâches se terminent.

  Si l’application s’arrête inopinément (par exemple en cas d’échec du processus de l’application), `StopAsync` n’est probablement pas appelée. Par conséquent, les méthodes appelées ou les opérations effectuées dans `StopAsync` peuvent ne pas se produire.

  Pour prolonger le délai d’expiration par défaut de cinq secondes, définissez :

  * <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> quand vous utilisez l’hôte générique. Pour plus d’informations, consultez <xref:fundamentals/host/generic-host#shutdown-timeout>.
  * Le paramètre de configuration du délai d’expiration de l’hôte quand vous utilisez l’hôte web. Pour plus d’informations, consultez <xref:fundamentals/host/web-host#shutdown-timeout>.

Le service hébergé est activé une seule fois au démarrage de l’application et s’arrête normalement à l’arrêt de l’application. Si une erreur est levée pendant l’exécution des tâches d’arrière-plan, `Dispose` doit être appelée même si `StopAsync` n’est pas appelée.

## <a name="backgroundservice-base-class"></a>Classe de base BackgroundService

<xref:Microsoft.Extensions.Hosting.BackgroundService> est une classe de base pour l’implémentation d’un <xref:Microsoft.Extensions.Hosting.IHostedService>à long terme.

[ExecuteAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.ExecuteAsync*) est appelé pour exécuter le service d’arrière-plan. L’implémentation retourne un <xref:System.Threading.Tasks.Task> qui représente toute la durée de vie du service d’arrière-plan. Aucun autre service n’est démarré tant que [ExecuteAsync n’est pas asynchrone](https://github.com/dotnet/extensions/issues/2149), par exemple en appelant `await`. Évitez d’effectuer des opérations d’initialisation de blocage longues dans `ExecuteAsync`. L’hôte bloque dans [StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.StopAsync*) en attente de la fin de `ExecuteAsync`.

Le jeton d’annulation est déclenché lors de l’appel de [IHostedService. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) . Votre implémentation de `ExecuteAsync` doit se terminer rapidement lorsque le jeton d’annulation est déclenché afin d’arrêter correctement le service. Dans le cas contraire, le service s’arrête de manière inappropriée au délai d’attente de l’arrêt. Pour plus d’informations, consultez la section de l' [interface IHostedService](#ihostedservice-interface) .

## <a name="timed-background-tasks"></a>Tâche d’arrière-plan avec minuteur

Une tâche d’arrière-plan avec minuteur utilise la classe [System.Threading.Timer](xref:System.Threading.Timer). Le minuteur déclenche la méthode `DoWork` de la tâche. Le minuteur est désactivé sur `StopAsync` et supprimé quand le conteneur du service est supprimé sur `Dispose` :

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-17,34,41)]

L' <xref:System.Threading.Timer> n’attend pas que les exécutions précédentes de `DoWork` se terminent. l’approche indiquée peut donc ne pas convenir à chaque scénario. Avec [verrouillage. l’incrément](xref:System.Threading.Interlocked.Increment*) est utilisé pour incrémenter le compteur d’exécution sous la forme d’une opération atomique, ce qui garantit que plusieurs threads ne sont pas mis à jour `executionCount` simultanément.

Le service est inscrit dans `IHostBuilder.ConfigureServices` (*Program.cs*) avec la méthode d’extension `AddHostedService` :

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Utilisation d’un service délimité dans une tâche d’arrière-plan

Pour utiliser les [services délimités](xref:fundamentals/dependency-injection#service-lifetimes) dans un [BackgroundService](#backgroundservice-base-class), créez une étendue. Par défaut, aucune étendue n’est créée pour un service hébergé.

Le service des tâches d’arrière-plan délimitées contient la logique de la tâche d’arrière-plan. Dans l’exemple suivant :

* Le service est asynchrone. La méthode `DoWork` retourne un `Task`. À des fins de démonstration, un délai de dix secondes est attendu dans la méthode `DoWork`.
* Une <xref:Microsoft.Extensions.Logging.ILogger> est injectée dans le service.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

Le service hébergé crée une étendue pour résoudre le service de tâche d’arrière-plan étendu afin d’appeler sa méthode `DoWork`. `DoWork` retourne un `Task`, qui est attendu dans `ExecuteAsync`:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

Les services sont inscrits dans `IHostBuilder.ConfigureServices` (*Program.cs*). Le service hébergé est inscrit avec la méthode d’extension `AddHostedService` :

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Tâches d’arrière-plan en file d’attente

Une file d’attente de tâches en arrière-plan est basée sur la <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> .NET 4. x ([planifiée provisoirement pour ASP.net Core](https://github.com/aspnet/Hosting/issues/1280)) :

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

Dans l’exemple de `QueueHostedService` suivant :

* La méthode `BackgroundProcessing` retourne un `Task`, qui est attendu dans `ExecuteAsync`.
* Les tâches en arrière-plan de la file d’attente sont déplacées en file d’attente et exécutées dans `BackgroundProcessing`.
* Les éléments de travail sont attendus avant que le service ne s’arrête dans `StopAsync`.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28-29,33)]

Un service `MonitorLoop` gère les tâches de mise en file d’attente pour le service hébergé chaque fois que la clé `w` est sélectionnée sur un appareil d’entrée :

* Le `IBackgroundTaskQueue` est injecté dans le service `MonitorLoop`.
* `IBackgroundTaskQueue.QueueBackgroundWorkItem` est appelé pour empiler un élément de travail.
* L’élément de travail simule une tâche en arrière-plan de longue durée :
  * Trois retards de 5 secondes sont exécutés (`Task.Delay`).
  * Une instruction `try-catch` intercepte <xref:System.OperationCanceledException> si la tâche est annulée.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

Les services sont inscrits dans `IHostBuilder.ConfigureServices` (*Program.cs*). Le service hébergé est inscrit avec la méthode d’extension `AddHostedService` :

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Dans ASP.NET Core, les tâches d’arrière-plan peuvent être implémentées en tant que *services hébergés*. Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>. Cette rubrique contient trois exemples de service hébergé :

* Tâche d’arrière-plan qui s’exécute sur un minuteur.
* Service hébergé qui active un [service délimité](xref:fundamentals/dependency-injection#service-lifetimes). Le service étendu peut utiliser l' [injection de dépendances (di)](xref:fundamentals/dependency-injection)
* Tâches d’arrière-plan en file d’attente qui s’exécutent séquentiellement.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="package"></a>Package

Référencer le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ou ajouter une référence de package au package [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting).

## <a name="ihostedservice-interface"></a>Interface IHostedService

Les services hébergés implémentent l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>. L’interface définit deux méthodes pour les objets qui sont gérés par l’hôte :

* [StartAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contient la logique de démarrage de la tâche en arrière-plan. Quand [l’hôte web](xref:fundamentals/host/web-host) est utilisé, `StartAsync` est appelé après le démarrage du serveur et le déclenchement [d’IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*). Quand [l’hôte générique](xref:fundamentals/host/generic-host) est utilisé, `StartAsync` est appelé avant le déclenchement de `ApplicationStarted`.

* [StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; déclenché lorsque l’ordinateur hôte effectue un arrêt approprié. `StopAsync` contient la logique pour terminer la tâche d’arrière-plan. Implémentez <xref:System.IDisposable> et les [finaliseurs (destructeurs)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) pour supprimer toutes les ressources non managées.

  Le jeton d’annulation a un délai d’expiration par défaut de cinq secondes pour indiquer que le processus d’arrêt ne doit plus être normal. Quand l’annulation est demandée sur le jeton :

  * Les opérations en arrière-plan restantes effectuées par l’application doivent être abandonnées.
  * Les méthodes appelées dans `StopAsync` doivent retourner rapidement.

  Cependant, les tâches ne sont pas abandonnées après la demande d’annulation : l’appelant attend que toutes les tâches se terminent.

  Si l’application s’arrête inopinément (par exemple en cas d’échec du processus de l’application), `StopAsync` n’est probablement pas appelée. Par conséquent, les méthodes appelées ou les opérations effectuées dans `StopAsync` peuvent ne pas se produire.

  Pour prolonger le délai d’expiration par défaut de cinq secondes, définissez :

  * <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> quand vous utilisez l’hôte générique. Pour plus d’informations, consultez <xref:fundamentals/host/generic-host#shutdown-timeout>.
  * Le paramètre de configuration du délai d’expiration de l’hôte quand vous utilisez l’hôte web. Pour plus d’informations, consultez <xref:fundamentals/host/web-host#shutdown-timeout>.

Le service hébergé est activé une seule fois au démarrage de l’application et s’arrête normalement à l’arrêt de l’application. Si une erreur est levée pendant l’exécution des tâches d’arrière-plan, `Dispose` doit être appelée même si `StopAsync` n’est pas appelée.

## <a name="timed-background-tasks"></a>Tâche d’arrière-plan avec minuteur

Une tâche d’arrière-plan avec minuteur utilise la classe [System.Threading.Timer](xref:System.Threading.Timer). Le minuteur déclenche la méthode `DoWork` de la tâche. Le minuteur est désactivé sur `StopAsync` et supprimé quand le conteneur du service est supprimé sur `Dispose` :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

L' <xref:System.Threading.Timer> n’attend pas que les exécutions précédentes de `DoWork` se terminent. l’approche indiquée peut donc ne pas convenir à chaque scénario.

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

Une file d’attente de tâches en arrière-plan est basée sur le .NET Framework 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([prévu provisoirement pour être intégré pour ASP.net Core](https://github.com/aspnet/Hosting/issues/1280)) :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

Dans `QueueHostedService`, les tâches d'arrière-plan dans la file d'attente sont retirées de la file d'attente et exécutées en tant que [BackgroundService](#backgroundservice-base-class), qui est une classe de base pour l’implémentation d’une exécution longue `IHostedService` :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

Les services sont inscrits dans `Startup.ConfigureServices`. L’implémentation `IHostedService` est inscrite avec la méthode d’extension `AddHostedService` :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

Dans la classe de modèle de page Index :

* `IBackgroundTaskQueue` est injecté dans le constructeur et affecté à `Queue`.
* Un <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> est injecté et affecté à `_serviceScopeFactory`. La fabrique est utilisée pour créer des instances de <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, qui servent à créer des services au sein d’une étendue. Une étendue est créée de façon à utiliser l’élément `AppDbContext` ([service délimité](xref:fundamentals/dependency-injection#service-lifetimes)) de l’application pour écrire des enregistrements de base de données dans `IBackgroundTaskQueue` (service singleton).

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

Quand le bouton **Ajouter une tâche** est sélectionné dans la page Index, la méthode `OnPostAddTask` est exécutée. `QueueBackgroundWorkItem` est appelé pour empiler un élément de travail :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a>Ressources supplémentaires

* [Implémenter des tâches d’arrière-plan dans des microservices avec IHostedService et la classe BackgroundService](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [Exécuter des tâches en arrière-plan avec les tâches Web dans Azure App Service](/azure/app-service/webjobs-create)
* <xref:System.Threading.Timer>
