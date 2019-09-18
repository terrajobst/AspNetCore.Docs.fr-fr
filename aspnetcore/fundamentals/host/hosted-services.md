---
title: Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core
author: guardrex
description: Découvrez comment implémenter des tâches d’arrière-plan avec des services hébergés dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/03/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 1db3ee1a9bcc0d41edf24df55bcd8d54fb0e9724
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081782"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

Dans ASP.NET Core, les tâches d’arrière-plan peuvent être implémentées en tant que *services hébergés*. Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>. Cette rubrique contient trois exemples de service hébergé :

* Tâche d’arrière-plan qui s’exécute sur un minuteur.
* Service hébergé qui active un [service délimité](xref:fundamentals/dependency-injection#service-lifetimes). Le service délimité peut utiliser l’injection de dépendances.
* Tâches d’arrière-plan en file d’attente qui s’exécutent séquentiellement.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

Cet exemple d’application est fourni en deux versions :

* Hôte web &ndash; L’hôte web est utile pour l’hébergement d’applications web. L’exemple de code présenté dans cette rubrique provient de la version de l’hôte web de l’exemple. Pour plus d’informations, consultez la rubrique [Hôte web](xref:fundamentals/host/web-host).
* Hôte générique &ndash; L’hôte générique est une nouveauté d’ASP.NET Core 2.1. Pour plus d’informations, consultez la rubrique [Hôte générique](xref:fundamentals/host/generic-host).

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a>Modèle Service Worker

Le modèle Service Worker ASP.NET Core fournit un point de départ pour l’écriture d’applications de service durables. Pour utiliser le modèle en tant que base d’une application de services hébergés :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Créer un nouveau projet.
1. Sélectionnez **Nouvelle application web ASP.NET Core**. Sélectionnez **Suivant**.
1. Indiquez un nom de projet dans le champ **Nom du projet**, ou acceptez le nom de projet par défaut. Sélectionnez **Créer**.
1. Dans la boîte de dialogue **Créer une application web ASP.NET Core**, vérifiez que **.NET Core** et **ASP.NET Core 3.0** sont sélectionnés.
1. Sélectionnez le modèle **Service Worker**. Sélectionnez **Créer**.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Utilisez le modèle Service Worker (`worker`) avec la commande [dotnet new](/dotnet/core/tools/dotnet-new) à partir d’un interpréteur de commandes. Dans l’exemple suivant, une application Service Worker est créée et se nomme `ContosoWorkerService`. Un dossier pour l’application `ContosoWorkerService` est créé automatiquement durant l’exécution de la commande.

```dotnetcli
dotnet new worker -o ContosoWorkerService
```

---

::: moniker-end

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

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

Le service est inscrit dans `Startup.ConfigureServices` avec la méthode d’extension `AddHostedService` :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Utilisation d’un service délimité dans une tâche d’arrière-plan

Pour utiliser des [services délimités](xref:fundamentals/dependency-injection#service-lifetimes) au sein d’un `IHostedService`, créez une étendue. Par défaut, aucune étendue n’est créée pour un service hébergé.

Le service des tâches d’arrière-plan délimitées contient la logique de la tâche d’arrière-plan. Dans l’exemple suivant, un <xref:Microsoft.Extensions.Logging.ILogger> est injecté dans le service :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

Le service hébergé crée une étendue pour résoudre le service des tâches d’arrière-plan délimitées pour appeler sa méthode `DoWork` :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

Les services sont inscrits dans `Startup.ConfigureServices`. L’implémentation `IHostedService` est inscrite avec la méthode d’extension `AddHostedService` :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Tâches d’arrière-plan en file d’attente

Une file d’attente de tâches d’arrière-plan est basée sur <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> de .NET 4.x ([dont l’intégration dans ASP.NET Core 3.0 est provisoirement planifiée](https://github.com/aspnet/Hosting/issues/1280)) :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

Dans `QueueHostedService`, les tâches d’arrière-plan dans la file d’attente sont retirées de la file d’attente et exécutées en tant que <xref:Microsoft.Extensions.Hosting.BackgroundService>, qui est une classe de base pour l’implémentation d’une exécution longue de `IHostedService` :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

Les services sont inscrits dans `Startup.ConfigureServices`. L’implémentation `IHostedService` est inscrite avec la méthode d’extension `AddHostedService` :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

Dans la classe de modèle de page Index :

* `IBackgroundTaskQueue` est injecté dans le constructeur et affecté à `Queue`.
* Un <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> est injecté et affecté à `_serviceScopeFactory`. La fabrique est utilisée pour créer des instances de <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, qui servent à créer des services au sein d’une étendue. Une étendue est créée de façon à utiliser l’élément `AppDbContext` ([service délimité](xref:fundamentals/dependency-injection#service-lifetimes)) de l’application pour écrire des enregistrements de base de données dans `IBackgroundTaskQueue` (service singleton).

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

Quand le bouton **Ajouter une tâche** est sélectionné dans la page Index, la méthode `OnPostAddTask` est exécutée. `QueueBackgroundWorkItem` est appelée pour mettre l’élément de travail en file d’attente :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a>Ressources supplémentaires

* [Implémenter des tâches d’arrière-plan dans des microservices avec IHostedService et la classe BackgroundService](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
