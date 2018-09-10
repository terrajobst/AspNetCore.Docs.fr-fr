---
title: Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core
author: guardrex
description: Découvrez comment implémenter des tâches d’arrière-plan avec des services hébergés dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
uid: fundamentals/host/hosted-services
ms.openlocfilehash: cc8f7fa00436a847ab1d1ba0976fb5e3899576ee
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312126"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

Dans ASP.NET Core, les tâches d’arrière-plan peuvent être implémentées en tant que *services hébergés*. Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice). Cette rubrique contient trois exemples de service hébergé :

* Tâche d’arrière-plan qui s’exécute sur un minuteur.
* Service hébergé qui active un service délimité. Le service délimité peut utiliser l’injection de dépendances.
* Tâches d’arrière-plan en file d’attente qui s’exécutent séquentiellement.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

Cet exemple d’application est fourni en deux versions :

* Hôte web &ndash; L’hôte web est utile pour l’hébergement d’applications web. L’exemple de code présenté dans cette rubrique provient de la version de l’hôte web de l’exemple. Pour plus d’informations, consultez la rubrique [Hôte web](xref:fundamentals/host/web-host).
* Hôte générique &ndash; L’hôte générique est une nouveauté d’ASP.NET Core 2.1. Pour plus d’informations, consultez la rubrique [Hôte générique](xref:fundamentals/host/generic-host).

## <a name="ihostedservice-interface"></a>Interface IHostedService

Les services hébergés implémentent l’interface [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice). L’interface définit deux méthodes pour les objets qui sont gérés par l’hôte :

* [StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - `StartAsync` contient la logique pour démarrer la tâche d’arrière-plan. Quand [l’hôte web](xref:fundamentals/host/web-host) est utilisé, `StartAsync` est appelé après le démarrage du serveur et le déclenchement [d’IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted). Quand [l’hôte générique](xref:fundamentals/host/generic-host) est utilisé, `StartAsync` est appelé avant le déclenchement de `ApplicationStarted`.

* [StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync), déclenchée quand l’hôte effectue un arrêt approprié. `StopAsync` contient la logique pour mettre fin à la tâche d’arrière-plan et pour supprimer les ressources non managées. Si l’application s’arrête inopinément (par exemple en cas d’échec du processus de l’application), `StopAsync` n’est probablement pas appelée.

Le service hébergé est activé une seule fois au démarrage de l’application et s’arrête de façon appropriée à l’arrêt de l’application. Quand [IDisposable](/dotnet/api/system.idisposable) est implémentée, les ressources peuvent être supprimées quand le conteneur du service est supprimé. Si une erreur est levée pendant l’exécution des tâches d’arrière-plan, `Dispose` doit être appelée même si `StopAsync` n’est pas appelée.

## <a name="timed-background-tasks"></a>Tâche d’arrière-plan avec minuteur

Une tâche d’arrière-plan avec minuteur utilise la classe [System.Threading.Timer](/dotnet/api/system.threading.timer). Le minuteur déclenche la méthode `DoWork` de la tâche. Le minuteur est désactivé sur `StopAsync` et supprimé quand le conteneur du service est supprimé sur `Dispose` :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

Le service est inscrit dans `Startup.ConfigureServices` avec la méthode d’extension `AddHostedService` :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Utilisation d’un service délimité dans une tâche d’arrière-plan

Pour utiliser des services délimités au sein d’un `IHostedService`, créez une étendue. Par défaut, aucune étendue n’est créée pour un service hébergé.

Le service des tâches d’arrière-plan délimitées contient la logique de la tâche d’arrière-plan. Dans l’exemple suivant, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) est injecté dans le service :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

Le service hébergé crée une étendue pour résoudre le service des tâches d’arrière-plan délimitées pour appeler sa méthode `DoWork` :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

Les services sont inscrits dans `Startup.ConfigureServices`. L’implémentation `IHostedService` est inscrite avec la méthode d’extension `AddHostedService` :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Tâches d’arrière-plan en file d’attente

Une file d’attente de tâches d’arrière-plan est basée sur [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) de .NET 4.x ([dont l’intégration dans ASP.NET Core 3.0 est provisoirement planifiée](https://github.com/aspnet/Hosting/issues/1280)) :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

Dans `QueueHostedService`, les tâches d'arrière-plan dans la file d'attente sont retirées de la file d'attente et exécutées en tant que [BackgroundService](/dotnet/api/microsoft.extensions.hosting.backgroundservice), qui est une classe de base pour l’implémentation d’une exécution longue `IHostedService` :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=16,20)]

Les services sont inscrits dans `Startup.ConfigureServices`. L’implémentation `IHostedService` est inscrite avec la méthode d’extension `AddHostedService` :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

Dans la classe de modèle de page Index, la `IBackgroundTaskQueue` est injectée dans le constructeur et affectée à `Queue` :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

Quand le bouton **Ajouter une tâche** est sélectionné dans la page Index, la méthode `OnPostAddTask` est exécutée. `QueueBackgroundWorkItem` est appelée pour mettre l’élément de travail en file d’attente :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a>Ressources supplémentaires

* [Implémenter des tâches d’arrière-plan dans des microservices avec IHostedService et la classe BackgroundService](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [System.Threading.Timer](/dotnet/api/system.threading.timer)
