---
title: Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core
author: guardrex
description: Découvrez comment implémenter des tâches d’arrière-plan avec des services hébergés dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/25/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: d10a335429752c1a52c1b3619adecc41725a819a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899305"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="19898-103">Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="19898-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="19898-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="19898-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="19898-105">Dans ASP.NET Core, les tâches d’arrière-plan peuvent être implémentées en tant que *services hébergés*.</span><span class="sxs-lookup"><span data-stu-id="19898-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="19898-106">Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="19898-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="19898-107">Cette rubrique contient trois exemples de service hébergé :</span><span class="sxs-lookup"><span data-stu-id="19898-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="19898-108">Tâche d’arrière-plan qui s’exécute sur un minuteur.</span><span class="sxs-lookup"><span data-stu-id="19898-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="19898-109">Service hébergé qui active un [service délimité](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="19898-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="19898-110">Le service délimité peut utiliser l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="19898-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="19898-111">Tâches d’arrière-plan en file d’attente qui s’exécutent séquentiellement.</span><span class="sxs-lookup"><span data-stu-id="19898-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="19898-112">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="19898-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="19898-113">Cet exemple d’application est fourni en deux versions :</span><span class="sxs-lookup"><span data-stu-id="19898-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="19898-114">Hôte web &ndash; L’hôte web est utile pour l’hébergement d’applications web.</span><span class="sxs-lookup"><span data-stu-id="19898-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="19898-115">L’exemple de code présenté dans cette rubrique provient de la version de l’hôte web de l’exemple.</span><span class="sxs-lookup"><span data-stu-id="19898-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="19898-116">Pour plus d’informations, consultez la rubrique [Hôte web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="19898-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="19898-117">Hôte générique &ndash; L’hôte générique est une nouveauté d’ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="19898-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="19898-118">Pour plus d’informations, consultez la rubrique [Hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="19898-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="19898-119">Package</span><span class="sxs-lookup"><span data-stu-id="19898-119">Package</span></span>

<span data-ttu-id="19898-120">Référencer le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ou ajouter une référence de package au package [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting).</span><span class="sxs-lookup"><span data-stu-id="19898-120">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="19898-121">Interface IHostedService</span><span class="sxs-lookup"><span data-stu-id="19898-121">IHostedService interface</span></span>

<span data-ttu-id="19898-122">Les services hébergés implémentent l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="19898-122">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="19898-123">L’interface définit deux méthodes pour les objets qui sont gérés par l’hôte :</span><span class="sxs-lookup"><span data-stu-id="19898-123">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="19898-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash;`StartAsync` contient la logique pour démarrer la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="19898-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="19898-125">Quand [l’hôte web](xref:fundamentals/host/web-host) est utilisé, `StartAsync` est appelé après le démarrage du serveur et le déclenchement [d’IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*).</span><span class="sxs-lookup"><span data-stu-id="19898-125">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="19898-126">Quand [l’hôte générique](xref:fundamentals/host/generic-host) est utilisé, `StartAsync` est appelé avant le déclenchement de `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="19898-126">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="19898-127">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash;, déclenchée quand l’hôte effectue un arrêt normal.</span><span class="sxs-lookup"><span data-stu-id="19898-127">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="19898-128">`StopAsync` contient la logique pour terminer la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="19898-128">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="19898-129">Implémentez <xref:System.IDisposable> et les [finaliseurs (destructeurs)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) pour supprimer toutes les ressources non managées.</span><span class="sxs-lookup"><span data-stu-id="19898-129">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="19898-130">Le jeton d’annulation a un délai d’expiration par défaut de cinq secondes pour indiquer que le processus d’arrêt ne doit plus être normal.</span><span class="sxs-lookup"><span data-stu-id="19898-130">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="19898-131">Quand l’annulation est demandée sur le jeton :</span><span class="sxs-lookup"><span data-stu-id="19898-131">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="19898-132">Les opérations en arrière-plan restantes effectuées par l’application doivent être abandonnées.</span><span class="sxs-lookup"><span data-stu-id="19898-132">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="19898-133">Les méthodes appelées dans `StopAsync` doivent retourner rapidement.</span><span class="sxs-lookup"><span data-stu-id="19898-133">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="19898-134">Cependant, les tâches ne sont pas abandonnées après la demande d’annulation : l’appelant attend que toutes les tâches se terminent.</span><span class="sxs-lookup"><span data-stu-id="19898-134">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="19898-135">Si l’application s’arrête inopinément (par exemple en cas d’échec du processus de l’application), `StopAsync` n’est probablement pas appelée.</span><span class="sxs-lookup"><span data-stu-id="19898-135">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="19898-136">Par conséquent, les méthodes appelées ou les opérations effectuées dans `StopAsync` peuvent ne pas se produire.</span><span class="sxs-lookup"><span data-stu-id="19898-136">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="19898-137">Pour prolonger le délai d’expiration par défaut de cinq secondes, définissez :</span><span class="sxs-lookup"><span data-stu-id="19898-137">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="19898-138"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> quand vous utilisez l’hôte générique.</span><span class="sxs-lookup"><span data-stu-id="19898-138"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="19898-139">Pour plus d'informations, consultez <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="19898-139">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="19898-140">Le paramètre de configuration du délai d’expiration de l’hôte quand vous utilisez l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="19898-140">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="19898-141">Pour plus d'informations, consultez <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="19898-141">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="19898-142">Le service hébergé est activé une seule fois au démarrage de l’application et s’arrête normalement à l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="19898-142">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="19898-143">Si une erreur est levée pendant l’exécution des tâches d’arrière-plan, `Dispose` doit être appelée même si `StopAsync` n’est pas appelée.</span><span class="sxs-lookup"><span data-stu-id="19898-143">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="19898-144">Tâche d’arrière-plan avec minuteur</span><span class="sxs-lookup"><span data-stu-id="19898-144">Timed background tasks</span></span>

<span data-ttu-id="19898-145">Une tâche d’arrière-plan avec minuteur utilise la classe [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="19898-145">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="19898-146">Le minuteur déclenche la méthode `DoWork` de la tâche.</span><span class="sxs-lookup"><span data-stu-id="19898-146">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="19898-147">Le minuteur est désactivé sur `StopAsync` et supprimé quand le conteneur du service est supprimé sur `Dispose` :</span><span class="sxs-lookup"><span data-stu-id="19898-147">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="19898-148">Le service est inscrit dans `Startup.ConfigureServices` avec la méthode d’extension `AddHostedService` :</span><span class="sxs-lookup"><span data-stu-id="19898-148">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="19898-149">Utilisation d’un service délimité dans une tâche d’arrière-plan</span><span class="sxs-lookup"><span data-stu-id="19898-149">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="19898-150">Pour utiliser des [services délimités](xref:fundamentals/dependency-injection#service-lifetimes) au sein d’un `IHostedService`, créez une étendue.</span><span class="sxs-lookup"><span data-stu-id="19898-150">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="19898-151">Par défaut, aucune étendue n’est créée pour un service hébergé.</span><span class="sxs-lookup"><span data-stu-id="19898-151">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="19898-152">Le service des tâches d’arrière-plan délimitées contient la logique de la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="19898-152">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="19898-153">Dans l’exemple suivant, un <xref:Microsoft.Extensions.Logging.ILogger> est injecté dans le service :</span><span class="sxs-lookup"><span data-stu-id="19898-153">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="19898-154">Le service hébergé crée une étendue pour résoudre le service des tâches d’arrière-plan délimitées pour appeler sa méthode `DoWork` :</span><span class="sxs-lookup"><span data-stu-id="19898-154">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="19898-155">Les services sont inscrits dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="19898-155">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="19898-156">L’implémentation `IHostedService` est inscrite avec la méthode d’extension `AddHostedService` :</span><span class="sxs-lookup"><span data-stu-id="19898-156">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="19898-157">Tâches d’arrière-plan en file d’attente</span><span class="sxs-lookup"><span data-stu-id="19898-157">Queued background tasks</span></span>

<span data-ttu-id="19898-158">Une file d’attente de tâches d’arrière-plan est basée sur <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> de .NET 4.x ([dont l’intégration dans ASP.NET Core 3.0 est provisoirement planifiée](https://github.com/aspnet/Hosting/issues/1280)) :</span><span class="sxs-lookup"><span data-stu-id="19898-158">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="19898-159">Dans `QueueHostedService`, les tâches d’arrière-plan dans la file d’attente sont retirées de la file d’attente et exécutées en tant que <xref:Microsoft.Extensions.Hosting.BackgroundService>, qui est une classe de base pour l’implémentation d’une exécution longue de `IHostedService` :</span><span class="sxs-lookup"><span data-stu-id="19898-159">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="19898-160">Les services sont inscrits dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="19898-160">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="19898-161">L’implémentation `IHostedService` est inscrite avec la méthode d’extension `AddHostedService` :</span><span class="sxs-lookup"><span data-stu-id="19898-161">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="19898-162">Dans la classe de modèle de page Index :</span><span class="sxs-lookup"><span data-stu-id="19898-162">In the Index page model class:</span></span>

* <span data-ttu-id="19898-163">`IBackgroundTaskQueue` est injecté dans le constructeur et affecté à `Queue`.</span><span class="sxs-lookup"><span data-stu-id="19898-163">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="19898-164">Un <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> est injecté et affecté à `_serviceScopeFactory`.</span><span class="sxs-lookup"><span data-stu-id="19898-164">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="19898-165">La fabrique est utilisée pour créer des instances de <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, qui servent à créer des services au sein d’une étendue.</span><span class="sxs-lookup"><span data-stu-id="19898-165">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="19898-166">Une étendue est créée de façon à utiliser l’élément `AppDbContext` ([service délimité](xref:fundamentals/dependency-injection#service-lifetimes)) de l’application pour écrire des enregistrements de base de données dans `IBackgroundTaskQueue` (service singleton).</span><span class="sxs-lookup"><span data-stu-id="19898-166">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="19898-167">Quand le bouton **Ajouter une tâche** est sélectionné dans la page Index, la méthode `OnPostAddTask` est exécutée.</span><span class="sxs-lookup"><span data-stu-id="19898-167">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="19898-168">`QueueBackgroundWorkItem` est appelée pour mettre l’élément de travail en file d’attente :</span><span class="sxs-lookup"><span data-stu-id="19898-168">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="19898-169">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="19898-169">Additional resources</span></span>

* [<span data-ttu-id="19898-170">Implémenter des tâches d’arrière-plan dans des microservices avec IHostedService et la classe BackgroundService</span><span class="sxs-lookup"><span data-stu-id="19898-170">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
