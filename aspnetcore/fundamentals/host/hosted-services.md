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
uid: fundamentals/host/hosted-services
ms.openlocfilehash: ace7fc8622864099b7c0e36e4a914de340d4d4e9
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="ab2dc-103">Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ab2dc-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="ab2dc-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ab2dc-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ab2dc-105">Dans ASP.NET Core, les tâches d’arrière-plan peuvent être implémentées en tant que *services hébergés*.</span><span class="sxs-lookup"><span data-stu-id="ab2dc-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="ab2dc-106">Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="ab2dc-106">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="ab2dc-107">Cette rubrique contient trois exemples de service hébergé :</span><span class="sxs-lookup"><span data-stu-id="ab2dc-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="ab2dc-108">Tâche d’arrière-plan qui s’exécute sur un minuteur.</span><span class="sxs-lookup"><span data-stu-id="ab2dc-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="ab2dc-109">Service hébergé qui active un service délimité.</span><span class="sxs-lookup"><span data-stu-id="ab2dc-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="ab2dc-110">Le service délimité peut utiliser l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="ab2dc-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="ab2dc-111">Tâches d’arrière-plan en file d’attente qui s’exécutent séquentiellement.</span><span class="sxs-lookup"><span data-stu-id="ab2dc-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="ab2dc-112">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ab2dc-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="ab2dc-113">Interface IHostedService</span><span class="sxs-lookup"><span data-stu-id="ab2dc-113">IHostedService interface</span></span>

<span data-ttu-id="ab2dc-114">Les services hébergés implémentent l’interface [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="ab2dc-114">Hosted services implement the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="ab2dc-115">L’interface définit deux méthodes pour les objets qui sont gérés par l’hôte :</span><span class="sxs-lookup"><span data-stu-id="ab2dc-115">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="ab2dc-116">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync), appelée une fois que le serveur a démarré et que [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) est déclenchée.</span><span class="sxs-lookup"><span data-stu-id="ab2dc-116">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - Called after the server has started and [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) is triggered.</span></span> <span data-ttu-id="ab2dc-117">`StartAsync` contient la logique pour démarrer la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="ab2dc-117">`StartAsync` contains the logic to start the background task.</span></span>

* <span data-ttu-id="ab2dc-118">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync), déclenchée quand l’hôte effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="ab2dc-118">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="ab2dc-119">`StopAsync` contient la logique pour mettre fin à la tâche d’arrière-plan et pour supprimer les ressources non managées.</span><span class="sxs-lookup"><span data-stu-id="ab2dc-119">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="ab2dc-120">Si l’application s’arrête inopinément (par exemple en cas d’échec du processus de l’application), `StopAsync` n’est probablement pas appelée.</span><span class="sxs-lookup"><span data-stu-id="ab2dc-120">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="ab2dc-121">Le service hébergé est un singleton qui est activé une seule fois au démarrage de l’application et qui s’arrête de façon appropriée lors de l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="ab2dc-121">The hosted service is a singleton that's activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="ab2dc-122">Quand [IDisposable](/dotnet/api/system.idisposable) est implémentée, les ressources peuvent être supprimées quand le conteneur du service est supprimé.</span><span class="sxs-lookup"><span data-stu-id="ab2dc-122">When [IDisposable](/dotnet/api/system.idisposable) is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="ab2dc-123">Si une erreur est levée pendant l’exécution des tâches d’arrière-plan, `Dispose` doit être appelée même si `StopAsync` n’est pas appelée.</span><span class="sxs-lookup"><span data-stu-id="ab2dc-123">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="ab2dc-124">Tâche d’arrière-plan avec minuteur</span><span class="sxs-lookup"><span data-stu-id="ab2dc-124">Timed background tasks</span></span>

<span data-ttu-id="ab2dc-125">Une tâche d’arrière-plan avec minuteur utilise la classe [System.Threading.Timer](/dotnet/api/system.threading.timer).</span><span class="sxs-lookup"><span data-stu-id="ab2dc-125">A timed background task makes use of the [System.Threading.Timer](/dotnet/api/system.threading.timer) class.</span></span> <span data-ttu-id="ab2dc-126">Le minuteur déclenche la méthode `DoWork` de la tâche.</span><span class="sxs-lookup"><span data-stu-id="ab2dc-126">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="ab2dc-127">Le minuteur est désactivé sur `StopAsync` et supprimé quand le conteneur du service est supprimé sur `Dispose` :</span><span class="sxs-lookup"><span data-stu-id="ab2dc-127">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="ab2dc-128">Le service est inscrit dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="ab2dc-128">The service is registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="ab2dc-129">Utilisation d’un service délimité dans une tâche d’arrière-plan</span><span class="sxs-lookup"><span data-stu-id="ab2dc-129">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="ab2dc-130">Pour utiliser des services délimités au sein d’un `IHostedService`, créez une étendue.</span><span class="sxs-lookup"><span data-stu-id="ab2dc-130">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="ab2dc-131">Par défaut, aucune étendue n’est créée pour un service hébergé.</span><span class="sxs-lookup"><span data-stu-id="ab2dc-131">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="ab2dc-132">Le service des tâches d’arrière-plan délimitées contient la logique de la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="ab2dc-132">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="ab2dc-133">Dans l’exemple suivant, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) est injecté dans le service :</span><span class="sxs-lookup"><span data-stu-id="ab2dc-133">In the following example, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="ab2dc-134">Le service hébergé crée une étendue pour résoudre le service des tâches d’arrière-plan délimitées pour appeler sa méthode `DoWork` :</span><span class="sxs-lookup"><span data-stu-id="ab2dc-134">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="ab2dc-135">Les services sont inscrits dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="ab2dc-135">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="ab2dc-136">Tâches d’arrière-plan en file d’attente</span><span class="sxs-lookup"><span data-stu-id="ab2dc-136">Queued background tasks</span></span>

<span data-ttu-id="ab2dc-137">Une file d’attente de tâches d’arrière-plan est basée sur [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) de .NET 4.x ([dont l’intégration dans ASP.NET Core 2.2 est provisoirement planifiée](https://github.com/aspnet/Hosting/issues/1280)) :</span><span class="sxs-lookup"><span data-stu-id="ab2dc-137">A background task queue is based on the .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([tentatively scheduled to be built-in for ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="ab2dc-138">Dans `QueueHostedService`, les tâches d’arrière-plan (`workItem`) de la file d’attente sont sorties de la file et exécutées :</span><span class="sxs-lookup"><span data-stu-id="ab2dc-138">In `QueueHostedService`, background tasks (`workItem`) in the queue are dequeued and executed:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

<span data-ttu-id="ab2dc-139">Les services sont inscrits dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="ab2dc-139">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet3)]

<span data-ttu-id="ab2dc-140">Dans la classe de modèle de page Index, la `IBackgroundTaskQueue` est injectée dans le constructeur et affectée à `Queue` :</span><span class="sxs-lookup"><span data-stu-id="ab2dc-140">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="ab2dc-141">Quand le bouton **Ajouter une tâche** est sélectionné dans la page Index, la méthode `OnPostAddTask` est exécutée.</span><span class="sxs-lookup"><span data-stu-id="ab2dc-141">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="ab2dc-142">`QueueBackgroundWorkItem` est appelée pour mettre l’élément de travail en file d’attente :</span><span class="sxs-lookup"><span data-stu-id="ab2dc-142">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="ab2dc-143">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ab2dc-143">Additional resources</span></span>

* [<span data-ttu-id="ab2dc-144">Implémenter des tâches d’arrière-plan dans des microservices avec IHostedService et la classe BackgroundService</span><span class="sxs-lookup"><span data-stu-id="ab2dc-144">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="ab2dc-145">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="ab2dc-145">System.Threading.Timer</span></span>](/dotnet/api/system.threading.timer)
