---
title: Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core
author: guardrex
description: Découvrez comment implémenter des tâches d’arrière-plan avec des services hébergés dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 9c38b1e1d429498bcd59f780e3d3fe1a50eae32d
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47860925"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="054c6-103">Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="054c6-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="054c6-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="054c6-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="054c6-105">Dans ASP.NET Core, les tâches d’arrière-plan peuvent être implémentées en tant que *services hébergés*.</span><span class="sxs-lookup"><span data-stu-id="054c6-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="054c6-106">Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="054c6-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="054c6-107">Cette rubrique contient trois exemples de service hébergé :</span><span class="sxs-lookup"><span data-stu-id="054c6-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="054c6-108">Tâche d’arrière-plan qui s’exécute sur un minuteur.</span><span class="sxs-lookup"><span data-stu-id="054c6-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="054c6-109">Service hébergé qui active un service délimité.</span><span class="sxs-lookup"><span data-stu-id="054c6-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="054c6-110">Le service délimité peut utiliser l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="054c6-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="054c6-111">Tâches d’arrière-plan en file d’attente qui s’exécutent séquentiellement.</span><span class="sxs-lookup"><span data-stu-id="054c6-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="054c6-112">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="054c6-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="054c6-113">Cet exemple d’application est fourni en deux versions :</span><span class="sxs-lookup"><span data-stu-id="054c6-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="054c6-114">Hôte web &ndash; L’hôte web est utile pour l’hébergement d’applications web.</span><span class="sxs-lookup"><span data-stu-id="054c6-114">Web Host &ndash; The Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="054c6-115">L’exemple de code présenté dans cette rubrique provient de la version de l’hôte web de l’exemple.</span><span class="sxs-lookup"><span data-stu-id="054c6-115">The example code shown in this topic is from the Web Host version of the sample.</span></span> <span data-ttu-id="054c6-116">Pour plus d’informations, consultez la rubrique [Hôte web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="054c6-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="054c6-117">Hôte générique &ndash; L’hôte générique est une nouveauté d’ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="054c6-117">Generic Host &ndash; The Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="054c6-118">Pour plus d’informations, consultez la rubrique [Hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="054c6-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="054c6-119">Package</span><span class="sxs-lookup"><span data-stu-id="054c6-119">Package</span></span>

<span data-ttu-id="054c6-120">Référencer le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ou ajouter une référence de package au package [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting).</span><span class="sxs-lookup"><span data-stu-id="054c6-120">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="054c6-121">Interface IHostedService</span><span class="sxs-lookup"><span data-stu-id="054c6-121">IHostedService interface</span></span>

<span data-ttu-id="054c6-122">Les services hébergés implémentent l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="054c6-122">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="054c6-123">L’interface définit deux méthodes pour les objets qui sont gérés par l’hôte :</span><span class="sxs-lookup"><span data-stu-id="054c6-123">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="054c6-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) - `StartAsync` contient la logique pour démarrer la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="054c6-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) - `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="054c6-125">Quand [l’hôte web](xref:fundamentals/host/web-host) est utilisé, `StartAsync` est appelé après le démarrage du serveur et le déclenchement [d’IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*).</span><span class="sxs-lookup"><span data-stu-id="054c6-125">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="054c6-126">Quand [l’hôte générique](xref:fundamentals/host/generic-host) est utilisé, `StartAsync` est appelé avant le déclenchement de `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="054c6-126">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="054c6-127">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*), déclenchée quand l’hôte effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="054c6-127">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="054c6-128">`StopAsync` contient la logique pour mettre fin à la tâche d’arrière-plan et pour supprimer les ressources non managées.</span><span class="sxs-lookup"><span data-stu-id="054c6-128">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="054c6-129">Si l’application s’arrête inopinément (par exemple en cas d’échec du processus de l’application), `StopAsync` n’est probablement pas appelée.</span><span class="sxs-lookup"><span data-stu-id="054c6-129">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="054c6-130">Le service hébergé est activé une seule fois au démarrage de l’application et s’arrête de façon appropriée à l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="054c6-130">The hosted service is activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="054c6-131">Lorsque <xref:System.IDisposable> est implémenté, les ressources peuvent être supprimées quand le conteneur du service est supprimé.</span><span class="sxs-lookup"><span data-stu-id="054c6-131">When <xref:System.IDisposable> is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="054c6-132">Si une erreur est levée pendant l’exécution des tâches d’arrière-plan, `Dispose` doit être appelée même si `StopAsync` n’est pas appelée.</span><span class="sxs-lookup"><span data-stu-id="054c6-132">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="054c6-133">Tâche d’arrière-plan avec minuteur</span><span class="sxs-lookup"><span data-stu-id="054c6-133">Timed background tasks</span></span>

<span data-ttu-id="054c6-134">Une tâche d’arrière-plan avec minuteur utilise la classe [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="054c6-134">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="054c6-135">Le minuteur déclenche la méthode `DoWork` de la tâche.</span><span class="sxs-lookup"><span data-stu-id="054c6-135">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="054c6-136">Le minuteur est désactivé sur `StopAsync` et supprimé quand le conteneur du service est supprimé sur `Dispose` :</span><span class="sxs-lookup"><span data-stu-id="054c6-136">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="054c6-137">Le service est inscrit dans `Startup.ConfigureServices` avec la méthode d’extension `AddHostedService` :</span><span class="sxs-lookup"><span data-stu-id="054c6-137">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="054c6-138">Utilisation d’un service délimité dans une tâche d’arrière-plan</span><span class="sxs-lookup"><span data-stu-id="054c6-138">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="054c6-139">Pour utiliser des services délimités au sein d’un `IHostedService`, créez une étendue.</span><span class="sxs-lookup"><span data-stu-id="054c6-139">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="054c6-140">Par défaut, aucune étendue n’est créée pour un service hébergé.</span><span class="sxs-lookup"><span data-stu-id="054c6-140">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="054c6-141">Le service des tâches d’arrière-plan délimitées contient la logique de la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="054c6-141">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="054c6-142">Dans l’exemple suivant, un <xref:Microsoft.Extensions.Logging.ILogger> est injecté dans le service :</span><span class="sxs-lookup"><span data-stu-id="054c6-142">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="054c6-143">Le service hébergé crée une étendue pour résoudre le service des tâches d’arrière-plan délimitées pour appeler sa méthode `DoWork` :</span><span class="sxs-lookup"><span data-stu-id="054c6-143">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="054c6-144">Les services sont inscrits dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="054c6-144">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="054c6-145">L’implémentation `IHostedService` est inscrite avec la méthode d’extension `AddHostedService` :</span><span class="sxs-lookup"><span data-stu-id="054c6-145">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="054c6-146">Tâches d’arrière-plan en file d’attente</span><span class="sxs-lookup"><span data-stu-id="054c6-146">Queued background tasks</span></span>

<span data-ttu-id="054c6-147">Une file d’attente de tâches d’arrière-plan est basée sur <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> de .NET 4.x ([dont l’intégration dans ASP.NET Core 3.0 est provisoirement planifiée](https://github.com/aspnet/Hosting/issues/1280)) :</span><span class="sxs-lookup"><span data-stu-id="054c6-147">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="054c6-148">Dans `QueueHostedService`, les tâches d’arrière-plan dans la file d’attente sont retirées de la file d’attente et exécutées en tant que <xref:Microsoft.Extensions.Hosting.BackgroundService>, qui est une classe de base pour l’implémentation d’une exécution longue de `IHostedService` :</span><span class="sxs-lookup"><span data-stu-id="054c6-148">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="054c6-149">Les services sont inscrits dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="054c6-149">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="054c6-150">L’implémentation `IHostedService` est inscrite avec la méthode d’extension `AddHostedService` :</span><span class="sxs-lookup"><span data-stu-id="054c6-150">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="054c6-151">Dans la classe de modèle de page Index, la `IBackgroundTaskQueue` est injectée dans le constructeur et affectée à `Queue` :</span><span class="sxs-lookup"><span data-stu-id="054c6-151">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="054c6-152">Quand le bouton **Ajouter une tâche** est sélectionné dans la page Index, la méthode `OnPostAddTask` est exécutée.</span><span class="sxs-lookup"><span data-stu-id="054c6-152">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="054c6-153">`QueueBackgroundWorkItem` est appelée pour mettre l’élément de travail en file d’attente :</span><span class="sxs-lookup"><span data-stu-id="054c6-153">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="054c6-154">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="054c6-154">Additional resources</span></span>

* [<span data-ttu-id="054c6-155">Implémenter des tâches d’arrière-plan dans des microservices avec IHostedService et la classe BackgroundService</span><span class="sxs-lookup"><span data-stu-id="054c6-155">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="054c6-156">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="054c6-156">System.Threading.Timer</span></span>](xref:System.Threading.Timer)
