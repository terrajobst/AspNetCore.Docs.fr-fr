---
title: Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core
author: guardrex
description: Découvrez comment implémenter des tâches d’arrière-plan avec des services hébergés dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/19/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: da3c2679005714a3d82de94cf3bc3c809aa3500d
ms.sourcegitcommit: 8157e5a351f49aeef3769f7d38b787b4386aad5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74239730"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="05b5b-103">Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="05b5b-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="05b5b-104">By [Luke Latham](https://github.com/guardrex) and [Jeow Li Huan](https://github.com/huan086)</span><span class="sxs-lookup"><span data-stu-id="05b5b-104">By [Luke Latham](https://github.com/guardrex) and [Jeow Li Huan](https://github.com/huan086)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="05b5b-105">Dans ASP.NET Core, les tâches d’arrière-plan peuvent être implémentées en tant que *services hébergés*.</span><span class="sxs-lookup"><span data-stu-id="05b5b-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="05b5b-106">Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="05b5b-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="05b5b-107">Cette rubrique contient trois exemples de service hébergé :</span><span class="sxs-lookup"><span data-stu-id="05b5b-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="05b5b-108">Tâche d’arrière-plan qui s’exécute sur un minuteur.</span><span class="sxs-lookup"><span data-stu-id="05b5b-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="05b5b-109">Service hébergé qui active un [service délimité](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="05b5b-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="05b5b-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="05b5b-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="05b5b-111">Tâches d’arrière-plan en file d’attente qui s’exécutent séquentiellement.</span><span class="sxs-lookup"><span data-stu-id="05b5b-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="05b5b-112">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="05b5b-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="05b5b-113">Modèle Service Worker</span><span class="sxs-lookup"><span data-stu-id="05b5b-113">Worker Service template</span></span>

<span data-ttu-id="05b5b-114">Le modèle Service Worker ASP.NET Core fournit un point de départ pour l’écriture d’applications de service durables.</span><span class="sxs-lookup"><span data-stu-id="05b5b-114">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="05b5b-115">An app created from the Worker Service template specifies the Worker SDK in its project file:</span><span class="sxs-lookup"><span data-stu-id="05b5b-115">An app created from the Worker Service template specifies the Worker SDK in its project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

<span data-ttu-id="05b5b-116">Pour utiliser le modèle en tant que base d’une application de services hébergés :</span><span class="sxs-lookup"><span data-stu-id="05b5b-116">To use the template as a basis for a hosted services app:</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

## <a name="package"></a><span data-ttu-id="05b5b-117">Package</span><span class="sxs-lookup"><span data-stu-id="05b5b-117">Package</span></span>

<span data-ttu-id="05b5b-118">An app based on the Worker Service template uses the `Microsoft.NET.Sdk.Worker` SDK and has an explicit package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span><span class="sxs-lookup"><span data-stu-id="05b5b-118">An app based on the Worker Service template uses the `Microsoft.NET.Sdk.Worker` SDK and has an explicit package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span> <span data-ttu-id="05b5b-119">For example, see the sample app's project file (*BackgroundTasksSample.csproj*).</span><span class="sxs-lookup"><span data-stu-id="05b5b-119">For example, see the sample app's project file (*BackgroundTasksSample.csproj*).</span></span>

<span data-ttu-id="05b5b-120">For web apps that use the `Microsoft.NET.Sdk.Web` SDK, the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is referenced implicitly from the shared framework.</span><span class="sxs-lookup"><span data-stu-id="05b5b-120">For web apps that use the `Microsoft.NET.Sdk.Web` SDK, the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is referenced implicitly from the shared framework.</span></span> <span data-ttu-id="05b5b-121">An explicit package reference in the app's project file isn't required.</span><span class="sxs-lookup"><span data-stu-id="05b5b-121">An explicit package reference in the app's project file isn't required.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="05b5b-122">Interface IHostedService</span><span class="sxs-lookup"><span data-stu-id="05b5b-122">IHostedService interface</span></span>

<span data-ttu-id="05b5b-123">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span><span class="sxs-lookup"><span data-stu-id="05b5b-123">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="05b5b-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash;`StartAsync` contient la logique pour démarrer la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="05b5b-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="05b5b-125">`StartAsync` is called *before*:</span><span class="sxs-lookup"><span data-stu-id="05b5b-125">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="05b5b-126">The app's request processing pipeline is configured (`Startup.Configure`).</span><span class="sxs-lookup"><span data-stu-id="05b5b-126">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="05b5b-127">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span><span class="sxs-lookup"><span data-stu-id="05b5b-127">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="05b5b-128">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span><span class="sxs-lookup"><span data-stu-id="05b5b-128">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="05b5b-129">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="05b5b-129">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="05b5b-130">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash;, déclenchée quand l’hôte effectue un arrêt normal.</span><span class="sxs-lookup"><span data-stu-id="05b5b-130">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="05b5b-131">`StopAsync` contient la logique pour terminer la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="05b5b-131">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="05b5b-132">Implémentez <xref:System.IDisposable> et les [finaliseurs (destructeurs)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) pour supprimer toutes les ressources non managées.</span><span class="sxs-lookup"><span data-stu-id="05b5b-132">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="05b5b-133">Le jeton d’annulation a un délai d’expiration par défaut de cinq secondes pour indiquer que le processus d’arrêt ne doit plus être normal.</span><span class="sxs-lookup"><span data-stu-id="05b5b-133">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="05b5b-134">Quand l’annulation est demandée sur le jeton :</span><span class="sxs-lookup"><span data-stu-id="05b5b-134">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="05b5b-135">Les opérations en arrière-plan restantes effectuées par l’application doivent être abandonnées.</span><span class="sxs-lookup"><span data-stu-id="05b5b-135">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="05b5b-136">Les méthodes appelées dans `StopAsync` doivent retourner rapidement.</span><span class="sxs-lookup"><span data-stu-id="05b5b-136">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="05b5b-137">Cependant, les tâches ne sont pas abandonnées après la demande d’annulation : l’appelant attend que toutes les tâches se terminent.</span><span class="sxs-lookup"><span data-stu-id="05b5b-137">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="05b5b-138">Si l’application s’arrête inopinément (par exemple en cas d’échec du processus de l’application), `StopAsync` n’est probablement pas appelée.</span><span class="sxs-lookup"><span data-stu-id="05b5b-138">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="05b5b-139">Par conséquent, les méthodes appelées ou les opérations effectuées dans `StopAsync` peuvent ne pas se produire.</span><span class="sxs-lookup"><span data-stu-id="05b5b-139">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="05b5b-140">Pour prolonger le délai d’expiration par défaut de cinq secondes, définissez :</span><span class="sxs-lookup"><span data-stu-id="05b5b-140">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="05b5b-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> quand vous utilisez l’hôte générique.</span><span class="sxs-lookup"><span data-stu-id="05b5b-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="05b5b-142">Pour plus d'informations, consultez <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="05b5b-142">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="05b5b-143">Le paramètre de configuration du délai d’expiration de l’hôte quand vous utilisez l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="05b5b-143">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="05b5b-144">Pour plus d'informations, consultez <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="05b5b-144">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="05b5b-145">Le service hébergé est activé une seule fois au démarrage de l’application et s’arrête normalement à l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="05b5b-145">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="05b5b-146">Si une erreur est levée pendant l’exécution des tâches d’arrière-plan, `Dispose` doit être appelée même si `StopAsync` n’est pas appelée.</span><span class="sxs-lookup"><span data-stu-id="05b5b-146">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice-base-class"></a><span data-ttu-id="05b5b-147">BackgroundService base class</span><span class="sxs-lookup"><span data-stu-id="05b5b-147">BackgroundService base class</span></span>

<span data-ttu-id="05b5b-148"><xref:Microsoft.Extensions.Hosting.BackgroundService> is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="05b5b-148"><xref:Microsoft.Extensions.Hosting.BackgroundService> is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span>

<span data-ttu-id="05b5b-149">[ExecuteAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.ExecuteAsync*) is called to run the background service.</span><span class="sxs-lookup"><span data-stu-id="05b5b-149">[ExecuteAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.ExecuteAsync*) is called to run the background service.</span></span> <span data-ttu-id="05b5b-150">The implementation returns a <xref:System.Threading.Tasks.Task> that represents the entire lifetime of the background service.</span><span class="sxs-lookup"><span data-stu-id="05b5b-150">The implementation returns a <xref:System.Threading.Tasks.Task> that represents the entire lifetime of the background service.</span></span> <span data-ttu-id="05b5b-151">No further services are started until [ExecuteAsync becomes asynchronous](https://github.com/aspnet/Extensions/issues/2149), such as by calling `await`.</span><span class="sxs-lookup"><span data-stu-id="05b5b-151">No further services are started until [ExecuteAsync becomes asynchronous](https://github.com/aspnet/Extensions/issues/2149), such as by calling `await`.</span></span> <span data-ttu-id="05b5b-152">Avoid performing long, blocking initialization work in `ExecuteAsync`.</span><span class="sxs-lookup"><span data-stu-id="05b5b-152">Avoid performing long, blocking initialization work in `ExecuteAsync`.</span></span> <span data-ttu-id="05b5b-153">The host blocks in [StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.StopAsync*) waiting for `ExecuteAsync` to complete.</span><span class="sxs-lookup"><span data-stu-id="05b5b-153">The host blocks in [StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.StopAsync*) waiting for `ExecuteAsync` to complete.</span></span>

<span data-ttu-id="05b5b-154">The cancellation token is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span><span class="sxs-lookup"><span data-stu-id="05b5b-154">The cancellation token is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span> <span data-ttu-id="05b5b-155">Your implementation of `ExecuteAsync` should finish promptly when the cancellation token is fired in order to gracefully shut down the service.</span><span class="sxs-lookup"><span data-stu-id="05b5b-155">Your implementation of `ExecuteAsync` should finish promptly when the cancellation token is fired in order to gracefully shut down the service.</span></span> <span data-ttu-id="05b5b-156">Otherwise, the service ungracefully shuts down at the shutdown timeout.</span><span class="sxs-lookup"><span data-stu-id="05b5b-156">Otherwise, the service ungracefully shuts down at the shutdown timeout.</span></span> <span data-ttu-id="05b5b-157">For more information, see the [IHostedService interface](#ihostedservice-interface) section.</span><span class="sxs-lookup"><span data-stu-id="05b5b-157">For more information, see the [IHostedService interface](#ihostedservice-interface) section.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="05b5b-158">Tâche d’arrière-plan avec minuteur</span><span class="sxs-lookup"><span data-stu-id="05b5b-158">Timed background tasks</span></span>

<span data-ttu-id="05b5b-159">Une tâche d’arrière-plan avec minuteur utilise la classe [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="05b5b-159">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="05b5b-160">Le minuteur déclenche la méthode `DoWork` de la tâche.</span><span class="sxs-lookup"><span data-stu-id="05b5b-160">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="05b5b-161">Le minuteur est désactivé sur `StopAsync` et supprimé quand le conteneur du service est supprimé sur `Dispose` :</span><span class="sxs-lookup"><span data-stu-id="05b5b-161">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="05b5b-162">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span><span class="sxs-lookup"><span data-stu-id="05b5b-162">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="05b5b-163">Utilisation d’un service délimité dans une tâche d’arrière-plan</span><span class="sxs-lookup"><span data-stu-id="05b5b-163">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="05b5b-164">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a [BackgroundService](#backgroundservice-base-class), create a scope.</span><span class="sxs-lookup"><span data-stu-id="05b5b-164">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a [BackgroundService](#backgroundservice-base-class), create a scope.</span></span> <span data-ttu-id="05b5b-165">Par défaut, aucune étendue n’est créée pour un service hébergé.</span><span class="sxs-lookup"><span data-stu-id="05b5b-165">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="05b5b-166">Le service des tâches d’arrière-plan délimitées contient la logique de la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="05b5b-166">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="05b5b-167">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="05b5b-167">In the following example:</span></span>

* <span data-ttu-id="05b5b-168">The service is asynchronous.</span><span class="sxs-lookup"><span data-stu-id="05b5b-168">The service is asynchronous.</span></span> <span data-ttu-id="05b5b-169">La méthode `DoWork` retourne un `Task`.</span><span class="sxs-lookup"><span data-stu-id="05b5b-169">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="05b5b-170">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span><span class="sxs-lookup"><span data-stu-id="05b5b-170">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="05b5b-171">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span><span class="sxs-lookup"><span data-stu-id="05b5b-171">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="05b5b-172">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span><span class="sxs-lookup"><span data-stu-id="05b5b-172">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="05b5b-173">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span><span class="sxs-lookup"><span data-stu-id="05b5b-173">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="05b5b-174">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="05b5b-174">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="05b5b-175">The hosted service is registered with the `AddHostedService` extension method:</span><span class="sxs-lookup"><span data-stu-id="05b5b-175">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="05b5b-176">Tâches d’arrière-plan en file d’attente</span><span class="sxs-lookup"><span data-stu-id="05b5b-176">Queued background tasks</span></span>

<span data-ttu-id="05b5b-177">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="05b5b-177">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="05b5b-178">In the following `QueueHostedService` example:</span><span class="sxs-lookup"><span data-stu-id="05b5b-178">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="05b5b-179">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span><span class="sxs-lookup"><span data-stu-id="05b5b-179">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="05b5b-180">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span><span class="sxs-lookup"><span data-stu-id="05b5b-180">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>
* <span data-ttu-id="05b5b-181">Work items are awaited before the service stops in `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="05b5b-181">Work items are awaited before the service stops in `StopAsync`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28-29,33)]

<span data-ttu-id="05b5b-182">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span><span class="sxs-lookup"><span data-stu-id="05b5b-182">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="05b5b-183">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span><span class="sxs-lookup"><span data-stu-id="05b5b-183">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="05b5b-184">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span><span class="sxs-lookup"><span data-stu-id="05b5b-184">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>
* <span data-ttu-id="05b5b-185">The work item simulates a long-running background task:</span><span class="sxs-lookup"><span data-stu-id="05b5b-185">The work item simulates a long-running background task:</span></span>
  * <span data-ttu-id="05b5b-186">Three 5-second delays are executed (`Task.Delay`).</span><span class="sxs-lookup"><span data-stu-id="05b5b-186">Three 5-second delays are executed (`Task.Delay`).</span></span>
  * <span data-ttu-id="05b5b-187">A `try-catch` statement traps <xref:System.OperationCanceledException> if the task is cancelled.</span><span class="sxs-lookup"><span data-stu-id="05b5b-187">A `try-catch` statement traps <xref:System.OperationCanceledException> if the task is cancelled.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="05b5b-188">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="05b5b-188">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="05b5b-189">The hosted service is registered with the `AddHostedService` extension method:</span><span class="sxs-lookup"><span data-stu-id="05b5b-189">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="05b5b-190">Dans ASP.NET Core, les tâches d’arrière-plan peuvent être implémentées en tant que *services hébergés*.</span><span class="sxs-lookup"><span data-stu-id="05b5b-190">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="05b5b-191">Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="05b5b-191">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="05b5b-192">Cette rubrique contient trois exemples de service hébergé :</span><span class="sxs-lookup"><span data-stu-id="05b5b-192">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="05b5b-193">Tâche d’arrière-plan qui s’exécute sur un minuteur.</span><span class="sxs-lookup"><span data-stu-id="05b5b-193">Background task that runs on a timer.</span></span>
* <span data-ttu-id="05b5b-194">Service hébergé qui active un [service délimité](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="05b5b-194">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="05b5b-195">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="05b5b-195">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="05b5b-196">Tâches d’arrière-plan en file d’attente qui s’exécutent séquentiellement.</span><span class="sxs-lookup"><span data-stu-id="05b5b-196">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="05b5b-197">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="05b5b-197">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="05b5b-198">Package</span><span class="sxs-lookup"><span data-stu-id="05b5b-198">Package</span></span>

<span data-ttu-id="05b5b-199">Référencer le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ou ajouter une référence de package au package [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting).</span><span class="sxs-lookup"><span data-stu-id="05b5b-199">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="05b5b-200">Interface IHostedService</span><span class="sxs-lookup"><span data-stu-id="05b5b-200">IHostedService interface</span></span>

<span data-ttu-id="05b5b-201">Les services hébergés implémentent l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="05b5b-201">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="05b5b-202">L’interface définit deux méthodes pour les objets qui sont gérés par l’hôte :</span><span class="sxs-lookup"><span data-stu-id="05b5b-202">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="05b5b-203">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash;`StartAsync` contient la logique pour démarrer la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="05b5b-203">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="05b5b-204">Quand [l’hôte web](xref:fundamentals/host/web-host) est utilisé, `StartAsync` est appelé après le démarrage du serveur et le déclenchement [d’IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*).</span><span class="sxs-lookup"><span data-stu-id="05b5b-204">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="05b5b-205">Quand [l’hôte générique](xref:fundamentals/host/generic-host) est utilisé, `StartAsync` est appelé avant le déclenchement de `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="05b5b-205">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="05b5b-206">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash;, déclenchée quand l’hôte effectue un arrêt normal.</span><span class="sxs-lookup"><span data-stu-id="05b5b-206">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="05b5b-207">`StopAsync` contient la logique pour terminer la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="05b5b-207">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="05b5b-208">Implémentez <xref:System.IDisposable> et les [finaliseurs (destructeurs)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) pour supprimer toutes les ressources non managées.</span><span class="sxs-lookup"><span data-stu-id="05b5b-208">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="05b5b-209">Le jeton d’annulation a un délai d’expiration par défaut de cinq secondes pour indiquer que le processus d’arrêt ne doit plus être normal.</span><span class="sxs-lookup"><span data-stu-id="05b5b-209">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="05b5b-210">Quand l’annulation est demandée sur le jeton :</span><span class="sxs-lookup"><span data-stu-id="05b5b-210">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="05b5b-211">Les opérations en arrière-plan restantes effectuées par l’application doivent être abandonnées.</span><span class="sxs-lookup"><span data-stu-id="05b5b-211">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="05b5b-212">Les méthodes appelées dans `StopAsync` doivent retourner rapidement.</span><span class="sxs-lookup"><span data-stu-id="05b5b-212">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="05b5b-213">Cependant, les tâches ne sont pas abandonnées après la demande d’annulation : l’appelant attend que toutes les tâches se terminent.</span><span class="sxs-lookup"><span data-stu-id="05b5b-213">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="05b5b-214">Si l’application s’arrête inopinément (par exemple en cas d’échec du processus de l’application), `StopAsync` n’est probablement pas appelée.</span><span class="sxs-lookup"><span data-stu-id="05b5b-214">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="05b5b-215">Par conséquent, les méthodes appelées ou les opérations effectuées dans `StopAsync` peuvent ne pas se produire.</span><span class="sxs-lookup"><span data-stu-id="05b5b-215">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="05b5b-216">Pour prolonger le délai d’expiration par défaut de cinq secondes, définissez :</span><span class="sxs-lookup"><span data-stu-id="05b5b-216">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="05b5b-217"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> quand vous utilisez l’hôte générique.</span><span class="sxs-lookup"><span data-stu-id="05b5b-217"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="05b5b-218">Pour plus d'informations, consultez <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="05b5b-218">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="05b5b-219">Le paramètre de configuration du délai d’expiration de l’hôte quand vous utilisez l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="05b5b-219">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="05b5b-220">Pour plus d'informations, consultez <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="05b5b-220">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="05b5b-221">Le service hébergé est activé une seule fois au démarrage de l’application et s’arrête normalement à l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="05b5b-221">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="05b5b-222">Si une erreur est levée pendant l’exécution des tâches d’arrière-plan, `Dispose` doit être appelée même si `StopAsync` n’est pas appelée.</span><span class="sxs-lookup"><span data-stu-id="05b5b-222">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="05b5b-223">Tâche d’arrière-plan avec minuteur</span><span class="sxs-lookup"><span data-stu-id="05b5b-223">Timed background tasks</span></span>

<span data-ttu-id="05b5b-224">Une tâche d’arrière-plan avec minuteur utilise la classe [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="05b5b-224">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="05b5b-225">Le minuteur déclenche la méthode `DoWork` de la tâche.</span><span class="sxs-lookup"><span data-stu-id="05b5b-225">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="05b5b-226">Le minuteur est désactivé sur `StopAsync` et supprimé quand le conteneur du service est supprimé sur `Dispose` :</span><span class="sxs-lookup"><span data-stu-id="05b5b-226">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="05b5b-227">Le service est inscrit dans `Startup.ConfigureServices` avec la méthode d’extension `AddHostedService` :</span><span class="sxs-lookup"><span data-stu-id="05b5b-227">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="05b5b-228">Utilisation d’un service délimité dans une tâche d’arrière-plan</span><span class="sxs-lookup"><span data-stu-id="05b5b-228">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="05b5b-229">Pour utiliser des [services délimités](xref:fundamentals/dependency-injection#service-lifetimes) au sein d’un `IHostedService`, créez une étendue.</span><span class="sxs-lookup"><span data-stu-id="05b5b-229">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="05b5b-230">Par défaut, aucune étendue n’est créée pour un service hébergé.</span><span class="sxs-lookup"><span data-stu-id="05b5b-230">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="05b5b-231">Le service des tâches d’arrière-plan délimitées contient la logique de la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="05b5b-231">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="05b5b-232">Dans l’exemple suivant, un <xref:Microsoft.Extensions.Logging.ILogger> est injecté dans le service :</span><span class="sxs-lookup"><span data-stu-id="05b5b-232">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="05b5b-233">Le service hébergé crée une étendue pour résoudre le service des tâches d’arrière-plan délimitées pour appeler sa méthode `DoWork` :</span><span class="sxs-lookup"><span data-stu-id="05b5b-233">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="05b5b-234">Les services sont inscrits dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="05b5b-234">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="05b5b-235">L’implémentation `IHostedService` est inscrite avec la méthode d’extension `AddHostedService` :</span><span class="sxs-lookup"><span data-stu-id="05b5b-235">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="05b5b-236">Tâches d’arrière-plan en file d’attente</span><span class="sxs-lookup"><span data-stu-id="05b5b-236">Queued background tasks</span></span>

<span data-ttu-id="05b5b-237">A background task queue is based on the .NET Framework 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="05b5b-237">A background task queue is based on the .NET Framework 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="05b5b-238">Dans `QueueHostedService`, les tâches d'arrière-plan dans la file d'attente sont retirées de la file d'attente et exécutées en tant que [BackgroundService](#backgroundservice-base-class), qui est une classe de base pour l’implémentation d’une exécution longue `IHostedService` :</span><span class="sxs-lookup"><span data-stu-id="05b5b-238">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a [BackgroundService](#backgroundservice-base-class), which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="05b5b-239">Les services sont inscrits dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="05b5b-239">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="05b5b-240">L’implémentation `IHostedService` est inscrite avec la méthode d’extension `AddHostedService` :</span><span class="sxs-lookup"><span data-stu-id="05b5b-240">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="05b5b-241">Dans la classe de modèle de page Index :</span><span class="sxs-lookup"><span data-stu-id="05b5b-241">In the Index page model class:</span></span>

* <span data-ttu-id="05b5b-242">`IBackgroundTaskQueue` est injecté dans le constructeur et affecté à `Queue`.</span><span class="sxs-lookup"><span data-stu-id="05b5b-242">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="05b5b-243">Un <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> est injecté et affecté à `_serviceScopeFactory`.</span><span class="sxs-lookup"><span data-stu-id="05b5b-243">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="05b5b-244">La fabrique est utilisée pour créer des instances de <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, qui servent à créer des services au sein d’une étendue.</span><span class="sxs-lookup"><span data-stu-id="05b5b-244">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="05b5b-245">Une étendue est créée de façon à utiliser l’élément `AppDbContext` ([service délimité](xref:fundamentals/dependency-injection#service-lifetimes)) de l’application pour écrire des enregistrements de base de données dans `IBackgroundTaskQueue` (service singleton).</span><span class="sxs-lookup"><span data-stu-id="05b5b-245">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="05b5b-246">Quand le bouton **Ajouter une tâche** est sélectionné dans la page Index, la méthode `OnPostAddTask` est exécutée.</span><span class="sxs-lookup"><span data-stu-id="05b5b-246">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="05b5b-247">`QueueBackgroundWorkItem` is called to enqueue a work item:</span><span class="sxs-lookup"><span data-stu-id="05b5b-247">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="05b5b-248">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="05b5b-248">Additional resources</span></span>

* [<span data-ttu-id="05b5b-249">Implémenter des tâches d’arrière-plan dans des microservices avec IHostedService et la classe BackgroundService</span><span class="sxs-lookup"><span data-stu-id="05b5b-249">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
