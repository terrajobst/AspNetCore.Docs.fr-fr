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
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="9f261-103">Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9f261-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="9f261-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9f261-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9f261-105">Dans ASP.NET Core, les tâches d’arrière-plan peuvent être implémentées en tant que *services hébergés*.</span><span class="sxs-lookup"><span data-stu-id="9f261-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="9f261-106">Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="9f261-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="9f261-107">Cette rubrique contient trois exemples de service hébergé :</span><span class="sxs-lookup"><span data-stu-id="9f261-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="9f261-108">Tâche d’arrière-plan qui s’exécute sur un minuteur.</span><span class="sxs-lookup"><span data-stu-id="9f261-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="9f261-109">Service hébergé qui active un [service délimité](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="9f261-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="9f261-110">Le service étendu peut utiliser l' [injection de dépendances (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="9f261-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="9f261-111">Tâches d’arrière-plan en file d’attente qui s’exécutent séquentiellement.</span><span class="sxs-lookup"><span data-stu-id="9f261-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="9f261-112">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9f261-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9f261-113">Cet exemple d’application est fourni en deux versions :</span><span class="sxs-lookup"><span data-stu-id="9f261-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="9f261-114">Hôte web &ndash; L’hôte web est utile pour l’hébergement d’applications web.</span><span class="sxs-lookup"><span data-stu-id="9f261-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="9f261-115">L’exemple de code présenté dans cette rubrique provient de la version de l’hôte web de l’exemple.</span><span class="sxs-lookup"><span data-stu-id="9f261-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="9f261-116">Pour plus d’informations, consultez la rubrique [Hôte web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="9f261-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="9f261-117">Hôte générique &ndash; L’hôte générique est une nouveauté d’ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="9f261-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="9f261-118">Pour plus d’informations, consultez la rubrique [Hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="9f261-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="9f261-119">Modèle Service Worker</span><span class="sxs-lookup"><span data-stu-id="9f261-119">Worker Service template</span></span>

<span data-ttu-id="9f261-120">Le modèle Service Worker ASP.NET Core fournit un point de départ pour l’écriture d’applications de service durables.</span><span class="sxs-lookup"><span data-stu-id="9f261-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="9f261-121">Pour utiliser le modèle en tant que base d’une application de services hébergés :</span><span class="sxs-lookup"><span data-stu-id="9f261-121">To use the template as a basis for a hosted services app:</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

---

## <a name="package"></a><span data-ttu-id="9f261-122">Package</span><span class="sxs-lookup"><span data-stu-id="9f261-122">Package</span></span>

<span data-ttu-id="9f261-123">Une référence de package au package [Microsoft. extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) est ajoutée implicitement pour les applications ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="9f261-123">A package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is added implicitly for ASP.NET Core apps.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="9f261-124">Interface IHostedService</span><span class="sxs-lookup"><span data-stu-id="9f261-124">IHostedService interface</span></span>

<span data-ttu-id="9f261-125">L' <xref:Microsoft.Extensions.Hosting.IHostedService> interface définit deux méthodes pour les objets gérés par l’hôte :</span><span class="sxs-lookup"><span data-stu-id="9f261-125">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="9f261-126">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash;`StartAsync` contient la logique pour démarrer la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="9f261-126">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="9f261-127">`StartAsync`est appelé *avant*:</span><span class="sxs-lookup"><span data-stu-id="9f261-127">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="9f261-128">Le pipeline de traitement des demandes de l'`Startup.Configure`application est configuré ().</span><span class="sxs-lookup"><span data-stu-id="9f261-128">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="9f261-129">Le serveur est démarré et [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) est déclenché.</span><span class="sxs-lookup"><span data-stu-id="9f261-129">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="9f261-130">Le comportement par défaut peut être modifié de `StartAsync` sorte que le service hébergé s’exécute après que le pipeline de l’application a été configuré et `ApplicationStarted` qu’il est appelé.</span><span class="sxs-lookup"><span data-stu-id="9f261-130">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="9f261-131">Pour modifier le comportement par défaut, ajoutez le service hébergé`VideosWatcher` (dans l’exemple suivant) après `ConfigureWebHostDefaults`l’appel de :</span><span class="sxs-lookup"><span data-stu-id="9f261-131">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="9f261-132">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash;, déclenchée quand l’hôte effectue un arrêt normal.</span><span class="sxs-lookup"><span data-stu-id="9f261-132">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="9f261-133">`StopAsync` contient la logique pour terminer la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="9f261-133">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="9f261-134">Implémentez <xref:System.IDisposable> et les [finaliseurs (destructeurs)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) pour supprimer toutes les ressources non managées.</span><span class="sxs-lookup"><span data-stu-id="9f261-134">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="9f261-135">Le jeton d’annulation a un délai d’expiration par défaut de cinq secondes pour indiquer que le processus d’arrêt ne doit plus être normal.</span><span class="sxs-lookup"><span data-stu-id="9f261-135">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="9f261-136">Quand l’annulation est demandée sur le jeton :</span><span class="sxs-lookup"><span data-stu-id="9f261-136">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="9f261-137">Les opérations en arrière-plan restantes effectuées par l’application doivent être abandonnées.</span><span class="sxs-lookup"><span data-stu-id="9f261-137">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="9f261-138">Les méthodes appelées dans `StopAsync` doivent retourner rapidement.</span><span class="sxs-lookup"><span data-stu-id="9f261-138">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="9f261-139">Cependant, les tâches ne sont pas abandonnées après la demande d’annulation : l’appelant attend que toutes les tâches se terminent.</span><span class="sxs-lookup"><span data-stu-id="9f261-139">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="9f261-140">Si l’application s’arrête inopinément (par exemple en cas d’échec du processus de l’application), `StopAsync` n’est probablement pas appelée.</span><span class="sxs-lookup"><span data-stu-id="9f261-140">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="9f261-141">Par conséquent, les méthodes appelées ou les opérations effectuées dans `StopAsync` peuvent ne pas se produire.</span><span class="sxs-lookup"><span data-stu-id="9f261-141">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="9f261-142">Pour prolonger le délai d’expiration par défaut de cinq secondes, définissez :</span><span class="sxs-lookup"><span data-stu-id="9f261-142">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="9f261-143"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> quand vous utilisez l’hôte générique.</span><span class="sxs-lookup"><span data-stu-id="9f261-143"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="9f261-144">Pour plus d'informations, consultez <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="9f261-144">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="9f261-145">Le paramètre de configuration du délai d’expiration de l’hôte quand vous utilisez l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="9f261-145">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="9f261-146">Pour plus d'informations, consultez <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="9f261-146">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="9f261-147">Le service hébergé est activé une seule fois au démarrage de l’application et s’arrête normalement à l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="9f261-147">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="9f261-148">Si une erreur est levée pendant l’exécution des tâches d’arrière-plan, `Dispose` doit être appelée même si `StopAsync` n’est pas appelée.</span><span class="sxs-lookup"><span data-stu-id="9f261-148">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice"></a><span data-ttu-id="9f261-149">BackgroundService</span><span class="sxs-lookup"><span data-stu-id="9f261-149">BackgroundService</span></span>

<span data-ttu-id="9f261-150">`BackgroundService`est une classe de base pour l’implémentation d' <xref:Microsoft.Extensions.Hosting.IHostedService>une exécution longue.</span><span class="sxs-lookup"><span data-stu-id="9f261-150">`BackgroundService` is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="9f261-151">`BackgroundService`fournit la `ExecuteAsync(CancellationToken stoppingToken)` méthode abstraite pour contenir la logique du service.</span><span class="sxs-lookup"><span data-stu-id="9f261-151">`BackgroundService` provides the `ExecuteAsync(CancellationToken stoppingToken)` abstract method to contain the service's logic.</span></span> <span data-ttu-id="9f261-152">La `stoppingToken` est déclenchée lorsque [IHostedService. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) est appelé.</span><span class="sxs-lookup"><span data-stu-id="9f261-152">The `stoppingToken` is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span> <span data-ttu-id="9f261-153">L’implémentation de cette méthode retourne un `Task` qui représente la durée de vie totale du service d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="9f261-153">The implementation of this method returns a `Task` that represents the entire lifetime of the background service.</span></span>

<span data-ttu-id="9f261-154">En outre, *vous pouvez* également substituer les méthodes définies `IHostedService` sur pour exécuter le code de démarrage et d’arrêt de votre service :</span><span class="sxs-lookup"><span data-stu-id="9f261-154">In addition, *optionally* override the methods defined on `IHostedService` to run startup and shutdown code for your service:</span></span>

* <span data-ttu-id="9f261-155">`StopAsync(CancellationToken cancellationToken)`&ndash; estappelélorsquel’hôted’application`StopAsync` effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="9f261-155">`StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync` is called when the application host is performing a graceful shutdown.</span></span> <span data-ttu-id="9f261-156">Le `cancellationToken` est signalé lorsque l’hôte décide de forcer l’arrêt du service.</span><span class="sxs-lookup"><span data-stu-id="9f261-156">The `cancellationToken` is signalled when the host decides to forcibly terminate the service.</span></span> <span data-ttu-id="9f261-157">Si cette méthode est substituée, vous **devez** appeler (et `await`) la méthode de classe de base pour vérifier que le service s’arrête correctement.</span><span class="sxs-lookup"><span data-stu-id="9f261-157">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service shuts down properly.</span></span>
* <span data-ttu-id="9f261-158">`StartAsync(CancellationToken cancellationToken)`&ndash; estappelépourdémarrerleservice`StartAsync` d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="9f261-158">`StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` is called to start the background service.</span></span> <span data-ttu-id="9f261-159">Le `cancellationToken` est signalé si le processus de démarrage est interrompu.</span><span class="sxs-lookup"><span data-stu-id="9f261-159">The `cancellationToken` is signalled if the startup process is interrupted.</span></span> <span data-ttu-id="9f261-160">L’implémentation retourne un `Task` qui représente le processus de démarrage pour le service.</span><span class="sxs-lookup"><span data-stu-id="9f261-160">The implementation returns a `Task` that represents the startup process for the service.</span></span> <span data-ttu-id="9f261-161">Aucun autre service n’est démarré jusqu' `Task` à ce que cela se termine.</span><span class="sxs-lookup"><span data-stu-id="9f261-161">No further services are started until this `Task` completes.</span></span> <span data-ttu-id="9f261-162">Si cette méthode est substituée, vous **devez** appeler (et `await`) la méthode de classe de base pour vous assurer que le service démarre correctement.</span><span class="sxs-lookup"><span data-stu-id="9f261-162">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service starts properly.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="9f261-163">Tâche d’arrière-plan avec minuteur</span><span class="sxs-lookup"><span data-stu-id="9f261-163">Timed background tasks</span></span>

<span data-ttu-id="9f261-164">Une tâche d’arrière-plan avec minuteur utilise la classe [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="9f261-164">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="9f261-165">Le minuteur déclenche la méthode `DoWork` de la tâche.</span><span class="sxs-lookup"><span data-stu-id="9f261-165">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="9f261-166">Le minuteur est désactivé sur `StopAsync` et supprimé quand le conteneur du service est supprimé sur `Dispose` :</span><span class="sxs-lookup"><span data-stu-id="9f261-166">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="9f261-167">Le service est inscrit dans `IHostBuilder.ConfigureServices` (*Program.cs*) avec la `AddHostedService` méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="9f261-167">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="9f261-168">Utilisation d’un service délimité dans une tâche d’arrière-plan</span><span class="sxs-lookup"><span data-stu-id="9f261-168">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="9f261-169">Pour utiliser les [services délimités](xref:fundamentals/dependency-injection#service-lifetimes) dans un `BackgroundService`, créez une étendue.</span><span class="sxs-lookup"><span data-stu-id="9f261-169">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a `BackgroundService`, create a scope.</span></span> <span data-ttu-id="9f261-170">Par défaut, aucune étendue n’est créée pour un service hébergé.</span><span class="sxs-lookup"><span data-stu-id="9f261-170">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="9f261-171">Le service des tâches d’arrière-plan délimitées contient la logique de la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="9f261-171">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="9f261-172">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="9f261-172">In the following example:</span></span>

* <span data-ttu-id="9f261-173">Le service est asynchrone.</span><span class="sxs-lookup"><span data-stu-id="9f261-173">The service is asynchronous.</span></span> <span data-ttu-id="9f261-174">La méthode `DoWork` retourne un `Task`.</span><span class="sxs-lookup"><span data-stu-id="9f261-174">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="9f261-175">À des fins de démonstration, un délai de dix secondes est attendu dans `DoWork` la méthode.</span><span class="sxs-lookup"><span data-stu-id="9f261-175">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="9f261-176">Un <xref:Microsoft.Extensions.Logging.ILogger> est injecté dans le service.</span><span class="sxs-lookup"><span data-stu-id="9f261-176">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="9f261-177">Le service hébergé crée une étendue pour résoudre le service de tâche d’arrière-plan étendu `DoWork` afin d’appeler sa méthode.</span><span class="sxs-lookup"><span data-stu-id="9f261-177">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="9f261-178">`DoWork`retourne un `Task`, qui est attendu dans `ExecuteAsync`:</span><span class="sxs-lookup"><span data-stu-id="9f261-178">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="9f261-179">Les services sont inscrits dans `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="9f261-179">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="9f261-180">Le service hébergé est inscrit avec la `AddHostedService` méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="9f261-180">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="9f261-181">Tâches d’arrière-plan en file d’attente</span><span class="sxs-lookup"><span data-stu-id="9f261-181">Queued background tasks</span></span>

<span data-ttu-id="9f261-182">Une file d’attente de tâches en arrière-plan est basée <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> sur le .net 4. x ([prévu provisoirement pour être intégré pour ASP.net Core](https://github.com/aspnet/Hosting/issues/1280)) :</span><span class="sxs-lookup"><span data-stu-id="9f261-182">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="9f261-183">Dans l’exemple `QueueHostedService` suivant :</span><span class="sxs-lookup"><span data-stu-id="9f261-183">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="9f261-184">La `BackgroundProcessing` méthode retourne un `Task`, qui est attendu dans `ExecuteAsync`.</span><span class="sxs-lookup"><span data-stu-id="9f261-184">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="9f261-185">Les tâches en arrière-plan de la file d’attente sont `BackgroundProcessing`déplacées en file d’attente et exécutées dans.</span><span class="sxs-lookup"><span data-stu-id="9f261-185">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28,39-40,44)]

<span data-ttu-id="9f261-186">Un `MonitorLoop` service gère les tâches de mise en file d’attente pour le `w` service hébergé chaque fois que la clé est sélectionnée sur un appareil d’entrée :</span><span class="sxs-lookup"><span data-stu-id="9f261-186">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="9f261-187">Est injecté dans le `MonitorLoop` service. `IBackgroundTaskQueue`</span><span class="sxs-lookup"><span data-stu-id="9f261-187">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="9f261-188">`IBackgroundTaskQueue.QueueBackgroundWorkItem`est appelé pour empiler un élément de travail.</span><span class="sxs-lookup"><span data-stu-id="9f261-188">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="9f261-189">Les services sont inscrits dans `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="9f261-189">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="9f261-190">Le service hébergé est inscrit avec la `AddHostedService` méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="9f261-190">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9f261-191">Dans ASP.NET Core, les tâches d’arrière-plan peuvent être implémentées en tant que *services hébergés*.</span><span class="sxs-lookup"><span data-stu-id="9f261-191">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="9f261-192">Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="9f261-192">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="9f261-193">Cette rubrique contient trois exemples de service hébergé :</span><span class="sxs-lookup"><span data-stu-id="9f261-193">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="9f261-194">Tâche d’arrière-plan qui s’exécute sur un minuteur.</span><span class="sxs-lookup"><span data-stu-id="9f261-194">Background task that runs on a timer.</span></span>
* <span data-ttu-id="9f261-195">Service hébergé qui active un [service délimité](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="9f261-195">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="9f261-196">Le service étendu peut utiliser l' [injection de dépendances (di)](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="9f261-196">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="9f261-197">Tâches d’arrière-plan en file d’attente qui s’exécutent séquentiellement.</span><span class="sxs-lookup"><span data-stu-id="9f261-197">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="9f261-198">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9f261-198">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9f261-199">Cet exemple d’application est fourni en deux versions :</span><span class="sxs-lookup"><span data-stu-id="9f261-199">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="9f261-200">Hôte web &ndash; L’hôte web est utile pour l’hébergement d’applications web.</span><span class="sxs-lookup"><span data-stu-id="9f261-200">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="9f261-201">L’exemple de code présenté dans cette rubrique provient de la version de l’hôte web de l’exemple.</span><span class="sxs-lookup"><span data-stu-id="9f261-201">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="9f261-202">Pour plus d’informations, consultez la rubrique [Hôte web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="9f261-202">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="9f261-203">Hôte générique &ndash; L’hôte générique est une nouveauté d’ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="9f261-203">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="9f261-204">Pour plus d’informations, consultez la rubrique [Hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="9f261-204">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="9f261-205">Package</span><span class="sxs-lookup"><span data-stu-id="9f261-205">Package</span></span>

<span data-ttu-id="9f261-206">Référencer le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ou ajouter une référence de package au package [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting).</span><span class="sxs-lookup"><span data-stu-id="9f261-206">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="9f261-207">Interface IHostedService</span><span class="sxs-lookup"><span data-stu-id="9f261-207">IHostedService interface</span></span>

<span data-ttu-id="9f261-208">Les services hébergés implémentent l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="9f261-208">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="9f261-209">L’interface définit deux méthodes pour les objets qui sont gérés par l’hôte :</span><span class="sxs-lookup"><span data-stu-id="9f261-209">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="9f261-210">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash;`StartAsync` contient la logique pour démarrer la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="9f261-210">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="9f261-211">Quand [l’hôte web](xref:fundamentals/host/web-host) est utilisé, `StartAsync` est appelé après le démarrage du serveur et le déclenchement [d’IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*).</span><span class="sxs-lookup"><span data-stu-id="9f261-211">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="9f261-212">Quand [l’hôte générique](xref:fundamentals/host/generic-host) est utilisé, `StartAsync` est appelé avant le déclenchement de `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="9f261-212">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="9f261-213">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash;, déclenchée quand l’hôte effectue un arrêt normal.</span><span class="sxs-lookup"><span data-stu-id="9f261-213">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="9f261-214">`StopAsync` contient la logique pour terminer la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="9f261-214">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="9f261-215">Implémentez <xref:System.IDisposable> et les [finaliseurs (destructeurs)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) pour supprimer toutes les ressources non managées.</span><span class="sxs-lookup"><span data-stu-id="9f261-215">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="9f261-216">Le jeton d’annulation a un délai d’expiration par défaut de cinq secondes pour indiquer que le processus d’arrêt ne doit plus être normal.</span><span class="sxs-lookup"><span data-stu-id="9f261-216">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="9f261-217">Quand l’annulation est demandée sur le jeton :</span><span class="sxs-lookup"><span data-stu-id="9f261-217">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="9f261-218">Les opérations en arrière-plan restantes effectuées par l’application doivent être abandonnées.</span><span class="sxs-lookup"><span data-stu-id="9f261-218">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="9f261-219">Les méthodes appelées dans `StopAsync` doivent retourner rapidement.</span><span class="sxs-lookup"><span data-stu-id="9f261-219">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="9f261-220">Cependant, les tâches ne sont pas abandonnées après la demande d’annulation : l’appelant attend que toutes les tâches se terminent.</span><span class="sxs-lookup"><span data-stu-id="9f261-220">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="9f261-221">Si l’application s’arrête inopinément (par exemple en cas d’échec du processus de l’application), `StopAsync` n’est probablement pas appelée.</span><span class="sxs-lookup"><span data-stu-id="9f261-221">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="9f261-222">Par conséquent, les méthodes appelées ou les opérations effectuées dans `StopAsync` peuvent ne pas se produire.</span><span class="sxs-lookup"><span data-stu-id="9f261-222">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="9f261-223">Pour prolonger le délai d’expiration par défaut de cinq secondes, définissez :</span><span class="sxs-lookup"><span data-stu-id="9f261-223">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="9f261-224"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> quand vous utilisez l’hôte générique.</span><span class="sxs-lookup"><span data-stu-id="9f261-224"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="9f261-225">Pour plus d'informations, consultez <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="9f261-225">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="9f261-226">Le paramètre de configuration du délai d’expiration de l’hôte quand vous utilisez l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="9f261-226">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="9f261-227">Pour plus d'informations, consultez <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="9f261-227">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="9f261-228">Le service hébergé est activé une seule fois au démarrage de l’application et s’arrête normalement à l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="9f261-228">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="9f261-229">Si une erreur est levée pendant l’exécution des tâches d’arrière-plan, `Dispose` doit être appelée même si `StopAsync` n’est pas appelée.</span><span class="sxs-lookup"><span data-stu-id="9f261-229">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="9f261-230">Tâche d’arrière-plan avec minuteur</span><span class="sxs-lookup"><span data-stu-id="9f261-230">Timed background tasks</span></span>

<span data-ttu-id="9f261-231">Une tâche d’arrière-plan avec minuteur utilise la classe [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="9f261-231">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="9f261-232">Le minuteur déclenche la méthode `DoWork` de la tâche.</span><span class="sxs-lookup"><span data-stu-id="9f261-232">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="9f261-233">Le minuteur est désactivé sur `StopAsync` et supprimé quand le conteneur du service est supprimé sur `Dispose` :</span><span class="sxs-lookup"><span data-stu-id="9f261-233">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="9f261-234">Le service est inscrit dans `Startup.ConfigureServices` avec la méthode d’extension `AddHostedService` :</span><span class="sxs-lookup"><span data-stu-id="9f261-234">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="9f261-235">Utilisation d’un service délimité dans une tâche d’arrière-plan</span><span class="sxs-lookup"><span data-stu-id="9f261-235">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="9f261-236">Pour utiliser des [services délimités](xref:fundamentals/dependency-injection#service-lifetimes) au sein d’un `IHostedService`, créez une étendue.</span><span class="sxs-lookup"><span data-stu-id="9f261-236">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="9f261-237">Par défaut, aucune étendue n’est créée pour un service hébergé.</span><span class="sxs-lookup"><span data-stu-id="9f261-237">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="9f261-238">Le service des tâches d’arrière-plan délimitées contient la logique de la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="9f261-238">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="9f261-239">Dans l’exemple suivant, un <xref:Microsoft.Extensions.Logging.ILogger> est injecté dans le service :</span><span class="sxs-lookup"><span data-stu-id="9f261-239">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="9f261-240">Le service hébergé crée une étendue pour résoudre le service des tâches d’arrière-plan délimitées pour appeler sa méthode `DoWork` :</span><span class="sxs-lookup"><span data-stu-id="9f261-240">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="9f261-241">Les services sont inscrits dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9f261-241">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9f261-242">L’implémentation `IHostedService` est inscrite avec la méthode d’extension `AddHostedService` :</span><span class="sxs-lookup"><span data-stu-id="9f261-242">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="9f261-243">Tâches d’arrière-plan en file d’attente</span><span class="sxs-lookup"><span data-stu-id="9f261-243">Queued background tasks</span></span>

<span data-ttu-id="9f261-244">Une file d’attente de tâches en arrière-plan est basée <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> sur le .net 4. x ([prévu provisoirement pour être intégré pour ASP.net Core](https://github.com/aspnet/Hosting/issues/1280)) :</span><span class="sxs-lookup"><span data-stu-id="9f261-244">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="9f261-245">Dans `QueueHostedService`, les tâches d’arrière-plan dans la file d’attente sont retirées de la file d’attente et exécutées en tant que <xref:Microsoft.Extensions.Hosting.BackgroundService>, qui est une classe de base pour l’implémentation d’une exécution longue de `IHostedService` :</span><span class="sxs-lookup"><span data-stu-id="9f261-245">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="9f261-246">Les services sont inscrits dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9f261-246">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9f261-247">L’implémentation `IHostedService` est inscrite avec la méthode d’extension `AddHostedService` :</span><span class="sxs-lookup"><span data-stu-id="9f261-247">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="9f261-248">Dans la classe de modèle de page Index :</span><span class="sxs-lookup"><span data-stu-id="9f261-248">In the Index page model class:</span></span>

* <span data-ttu-id="9f261-249">`IBackgroundTaskQueue` est injecté dans le constructeur et affecté à `Queue`.</span><span class="sxs-lookup"><span data-stu-id="9f261-249">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="9f261-250">Un <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> est injecté et affecté à `_serviceScopeFactory`.</span><span class="sxs-lookup"><span data-stu-id="9f261-250">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="9f261-251">La fabrique est utilisée pour créer des instances de <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, qui servent à créer des services au sein d’une étendue.</span><span class="sxs-lookup"><span data-stu-id="9f261-251">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="9f261-252">Une étendue est créée de façon à utiliser l’élément `AppDbContext` ([service délimité](xref:fundamentals/dependency-injection#service-lifetimes)) de l’application pour écrire des enregistrements de base de données dans `IBackgroundTaskQueue` (service singleton).</span><span class="sxs-lookup"><span data-stu-id="9f261-252">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="9f261-253">Quand le bouton **Ajouter une tâche** est sélectionné dans la page Index, la méthode `OnPostAddTask` est exécutée.</span><span class="sxs-lookup"><span data-stu-id="9f261-253">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="9f261-254">`QueueBackgroundWorkItem`est appelé pour empiler un élément de travail :</span><span class="sxs-lookup"><span data-stu-id="9f261-254">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="9f261-255">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9f261-255">Additional resources</span></span>

* [<span data-ttu-id="9f261-256">Implémenter des tâches d’arrière-plan dans des microservices avec IHostedService et la classe BackgroundService</span><span class="sxs-lookup"><span data-stu-id="9f261-256">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
