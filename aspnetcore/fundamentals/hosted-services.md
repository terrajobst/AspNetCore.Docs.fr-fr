---
title: Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core
author: guardrex
description: Découvrez comment implémenter des tâches d’arrière-plan avec des services hébergés dans ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosted-services
ms.openlocfilehash: 89e595fb6ef38745d7377fdaaf1780c64320efe3
ms.sourcegitcommit: d43c84c4c80527c85e49d53691b293669557a79d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/20/2018
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

Dans ASP.NET Core, les tâches d’arrière-plan peuvent être implémentées en tant que *services hébergés*. Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice). Cette rubrique contient trois exemples de service hébergé :

* Tâche d’arrière-plan qui s’exécute sur un minuteur.
* Service hébergé qui active un service délimité. Le service délimité peut utiliser l’injection de dépendances.
* Tâches d’arrière-plan en file d’attente qui s’exécutent séquentiellement.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/hosted-services/samples/2.x) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="ihostedservice-interface"></a>Interface IHostedService

Les services hébergés implémentent l’interface [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice). L’interface définit deux méthodes pour les objets qui sont gérés par l’hôte :

* [StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync), appelée une fois que le serveur a démarré et que [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) est déclenchée. `StartAsync` contient la logique pour démarrer la tâche d’arrière-plan.

* [StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync), déclenchée quand l’hôte effectue un arrêt approprié. `StopAsync` contient la logique pour mettre fin à la tâche d’arrière-plan et pour supprimer les ressources non managées. Si l’application s’arrête inopinément (par exemple en cas d’échec du processus de l’application), `StopAsync` n’est probablement pas appelée.

Le service hébergé est un singleton qui est activé une seule fois au démarrage de l’application et qui s’arrête de façon appropriée lors de l’arrêt de l’application. Quand [IDisposable](/dotnet/api/system.idisposable) est implémentée, les ressources peuvent être supprimées quand le conteneur du service est supprimé. Si une erreur est levée pendant l’exécution des tâches d’arrière-plan, `Dispose` doit être appelée même si `StopAsync` n’est pas appelée.

## <a name="timed-background-tasks"></a>Tâche d’arrière-plan avec minuteur

Une tâche d’arrière-plan avec minuteur utilise la classe [System.Threading.Timer](/dotnet/api/system.threading.timer). Le minuteur déclenche la méthode `DoWork` de la tâche. Le minuteur est désactivé sur `StopAsync` et supprimé quand le conteneur du service est supprimé sur `Dispose` :

[!code-csharp[](hosted-services/samples/2.x/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

Le service est inscrit dans `Startup.ConfigureServices` :

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Utilisation d’un service délimité dans une tâche d’arrière-plan

Pour utiliser des services délimités au sein d’un `IHostedService`, créez une étendue. Par défaut, aucune étendue n’est créée pour un service hébergé.

Le service des tâches d’arrière-plan délimitées contient la logique de la tâche d’arrière-plan. Dans l’exemple suivant, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) est injecté dans le service :

[!code-csharp[](hosted-services/samples/2.x/Services/ScopedProcessingService.cs?name=snippet1)]

Le service hébergé crée une étendue pour résoudre le service des tâches d’arrière-plan délimitées pour appeler sa méthode `DoWork` :

[!code-csharp[](hosted-services/samples/2.x/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

Les services sont inscrits dans `Startup.ConfigureServices` :

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Tâches d’arrière-plan en file d’attente

Une file d’attente de tâches d’arrière-plan est basée sur [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) de .NET 4.x ([dont l’intégration dans ASP.NET Core 2.2 est provisoirement planifiée](https://github.com/aspnet/Hosting/issues/1280)) :

[!code-csharp[](hosted-services/samples/2.x/Services/BackgroundTaskQueue.cs?name=snippet1)]

Dans `QueueHostedService`, les tâches d’arrière-plan (`workItem`) de la file d’attente sont sorties de la file et exécutées :

[!code-csharp[](hosted-services/samples/2.x/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

Les services sont inscrits dans `Startup.ConfigureServices` :

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet3)]

Dans la classe de modèle de page Index, la `IBackgroundTaskQueue` est injectée dans le constructeur et affectée à `Queue` :

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet1)]

Quand le bouton **Ajouter une tâche** est sélectionné dans la page Index, la méthode `OnPostAddTask` est exécutée. `QueueBackgroundWorkItem` est appelée pour mettre l’élément de travail en file d’attente :

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a>Ressources supplémentaires

* [Implémenter des tâches d’arrière-plan dans des microservices avec IHostedService et la classe BackgroundService](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [System.Threading.Timer](/dotnet/api/system.threading.timer)
