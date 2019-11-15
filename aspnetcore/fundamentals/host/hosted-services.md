---
title: Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core
author: guardrex
description: Découvrez comment implémenter des tâches d’arrière-plan avec des services hébergés dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/14/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 0fdf503e4a5f6f73d5488261707180cfb5967492
ms.sourcegitcommit: 231780c8d7848943e5e9fd55e93f437f7e5a371d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2019
ms.locfileid: "74115940"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="08be8-103">Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="08be8-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="08be8-104">Par [Luke Latham](https://github.com/guardrex) et [Jeow Li Huan](https://github.com/huan086)</span><span class="sxs-lookup"><span data-stu-id="08be8-104">By [Luke Latham](https://github.com/guardrex) and [Jeow Li Huan](https://github.com/huan086)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="08be8-105">Dans ASP.NET Core, les tâches d’arrière-plan peuvent être implémentées en tant que *services hébergés*.</span><span class="sxs-lookup"><span data-stu-id="08be8-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="08be8-106">Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="08be8-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="08be8-107">Cette rubrique contient trois exemples de service hébergé :</span><span class="sxs-lookup"><span data-stu-id="08be8-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="08be8-108">Tâche d’arrière-plan qui s’exécute sur un minuteur.</span><span class="sxs-lookup"><span data-stu-id="08be8-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="08be8-109">Service hébergé qui active un [service délimité](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="08be8-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="08be8-110">Le service étendu peut utiliser l' [injection de dépendances (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="08be8-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="08be8-111">Tâches d’arrière-plan en file d’attente qui s’exécutent séquentiellement.</span><span class="sxs-lookup"><span data-stu-id="08be8-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="08be8-112">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="08be8-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="08be8-113">Modèle Service Worker</span><span class="sxs-lookup"><span data-stu-id="08be8-113">Worker Service template</span></span>

<span data-ttu-id="08be8-114">Le modèle Service Worker ASP.NET Core fournit un point de départ pour l’écriture d’applications de service durables.</span><span class="sxs-lookup"><span data-stu-id="08be8-114">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="08be8-115">Une application créée à partir du modèle de service Worker spécifie le kit de développement logiciel (SDK) Worker dans son fichier projet :</span><span class="sxs-lookup"><span data-stu-id="08be8-115">An app created from the Worker Service template specifies the Worker SDK in its project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

<span data-ttu-id="08be8-116">Pour utiliser le modèle en tant que base d’une application de services hébergés :</span><span class="sxs-lookup"><span data-stu-id="08be8-116">To use the template as a basis for a hosted services app:</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

## <a name="package"></a><span data-ttu-id="08be8-117">Package</span><span class="sxs-lookup"><span data-stu-id="08be8-117">Package</span></span>

<span data-ttu-id="08be8-118">Une application basée sur le modèle de service worker utilise le kit de développement logiciel (SDK) `Microsoft.NET.Sdk.Worker` et possède une référence de package explicite au package [Microsoft. extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) .</span><span class="sxs-lookup"><span data-stu-id="08be8-118">An app based on the Worker Service template uses the `Microsoft.NET.Sdk.Worker` SDK and has an explicit package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span> <span data-ttu-id="08be8-119">Par exemple, consultez le fichier projet de l’exemple d’application (*BackgroundTasksSample. csproj*).</span><span class="sxs-lookup"><span data-stu-id="08be8-119">For example, see the sample app's project file (*BackgroundTasksSample.csproj*).</span></span>

<span data-ttu-id="08be8-120">Pour les applications Web qui utilisent le kit de développement logiciel (SDK) `Microsoft.NET.Sdk.Web`, le package [Microsoft. extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) est référencé implicitement à partir de l’infrastructure partagée.</span><span class="sxs-lookup"><span data-stu-id="08be8-120">For web apps that use the `Microsoft.NET.Sdk.Web` SDK, the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is referenced implicitly from the shared framework.</span></span> <span data-ttu-id="08be8-121">Une référence de package explicite dans le fichier projet de l’application n’est pas obligatoire.</span><span class="sxs-lookup"><span data-stu-id="08be8-121">An explicit package reference in the app's project file isn't required.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="08be8-122">Interface IHostedService</span><span class="sxs-lookup"><span data-stu-id="08be8-122">IHostedService interface</span></span>

<span data-ttu-id="08be8-123">L’interface <xref:Microsoft.Extensions.Hosting.IHostedService> définit deux méthodes pour les objets gérés par l’hôte :</span><span class="sxs-lookup"><span data-stu-id="08be8-123">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="08be8-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash;`StartAsync` contient la logique pour démarrer la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="08be8-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="08be8-125">`StartAsync` est appelé *avant*:</span><span class="sxs-lookup"><span data-stu-id="08be8-125">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="08be8-126">Le pipeline de traitement des demandes de l’application est configuré (`Startup.Configure`).</span><span class="sxs-lookup"><span data-stu-id="08be8-126">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="08be8-127">Le serveur est démarré et [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) est déclenché.</span><span class="sxs-lookup"><span data-stu-id="08be8-127">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="08be8-128">Le comportement par défaut peut être modifié de sorte que le `StartAsync` du service hébergé s’exécute après la configuration du pipeline de l’application et que `ApplicationStarted` soit appelé.</span><span class="sxs-lookup"><span data-stu-id="08be8-128">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="08be8-129">Pour modifier le comportement par défaut, ajoutez le service hébergé (`VideosWatcher` dans l’exemple suivant) après l’appel de `ConfigureWebHostDefaults` :</span><span class="sxs-lookup"><span data-stu-id="08be8-129">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="08be8-130">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash;, déclenchée quand l’hôte effectue un arrêt normal.</span><span class="sxs-lookup"><span data-stu-id="08be8-130">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="08be8-131">`StopAsync` contient la logique pour terminer la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="08be8-131">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="08be8-132">Implémentez <xref:System.IDisposable> et les [finaliseurs (destructeurs)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) pour supprimer toutes les ressources non managées.</span><span class="sxs-lookup"><span data-stu-id="08be8-132">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="08be8-133">Le jeton d’annulation a un délai d’expiration par défaut de cinq secondes pour indiquer que le processus d’arrêt ne doit plus être normal.</span><span class="sxs-lookup"><span data-stu-id="08be8-133">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="08be8-134">Quand l’annulation est demandée sur le jeton :</span><span class="sxs-lookup"><span data-stu-id="08be8-134">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="08be8-135">Les opérations en arrière-plan restantes effectuées par l’application doivent être abandonnées.</span><span class="sxs-lookup"><span data-stu-id="08be8-135">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="08be8-136">Les méthodes appelées dans `StopAsync` doivent retourner rapidement.</span><span class="sxs-lookup"><span data-stu-id="08be8-136">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="08be8-137">Cependant, les tâches ne sont pas abandonnées après la demande d’annulation : l’appelant attend que toutes les tâches se terminent.</span><span class="sxs-lookup"><span data-stu-id="08be8-137">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="08be8-138">Si l’application s’arrête inopinément (par exemple en cas d’échec du processus de l’application), `StopAsync` n’est probablement pas appelée.</span><span class="sxs-lookup"><span data-stu-id="08be8-138">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="08be8-139">Par conséquent, les méthodes appelées ou les opérations effectuées dans `StopAsync` peuvent ne pas se produire.</span><span class="sxs-lookup"><span data-stu-id="08be8-139">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="08be8-140">Pour prolonger le délai d’expiration par défaut de cinq secondes, définissez :</span><span class="sxs-lookup"><span data-stu-id="08be8-140">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="08be8-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> quand vous utilisez l’hôte générique.</span><span class="sxs-lookup"><span data-stu-id="08be8-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="08be8-142">Pour plus d'informations, consultez <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="08be8-142">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="08be8-143">Le paramètre de configuration du délai d’expiration de l’hôte quand vous utilisez l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="08be8-143">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="08be8-144">Pour plus d'informations, consultez <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="08be8-144">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="08be8-145">Le service hébergé est activé une seule fois au démarrage de l’application et s’arrête normalement à l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="08be8-145">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="08be8-146">Si une erreur est levée pendant l’exécution des tâches d’arrière-plan, `Dispose` doit être appelée même si `StopAsync` n’est pas appelée.</span><span class="sxs-lookup"><span data-stu-id="08be8-146">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice"></a><span data-ttu-id="08be8-147">BackgroundService</span><span class="sxs-lookup"><span data-stu-id="08be8-147">BackgroundService</span></span>

<span data-ttu-id="08be8-148">`BackgroundService` est une classe de base pour l’implémentation d’une longue <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="08be8-148">`BackgroundService` is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="08be8-149">`BackgroundService` fournit la méthode abstraite `ExecuteAsync(CancellationToken stoppingToken)` pour contenir la logique du service.</span><span class="sxs-lookup"><span data-stu-id="08be8-149">`BackgroundService` provides the `ExecuteAsync(CancellationToken stoppingToken)` abstract method to contain the service's logic.</span></span> <span data-ttu-id="08be8-150">La `stoppingToken` est déclenchée lors de l’appel de [IHostedService. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) .</span><span class="sxs-lookup"><span data-stu-id="08be8-150">The `stoppingToken` is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span> <span data-ttu-id="08be8-151">L’implémentation de cette méthode retourne un `Task` qui représente l’intégralité de la durée de vie du service d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="08be8-151">The implementation of this method returns a `Task` that represents the entire lifetime of the background service.</span></span>

<span data-ttu-id="08be8-152">En outre, *vous pouvez éventuellement* remplacer les méthodes définies sur `IHostedService` pour exécuter le code de démarrage et d’arrêt de votre service :</span><span class="sxs-lookup"><span data-stu-id="08be8-152">In addition, *optionally* override the methods defined on `IHostedService` to run startup and shutdown code for your service:</span></span>

* <span data-ttu-id="08be8-153">`StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync` est appelé lorsque l’hôte d’application effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="08be8-153">`StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync` is called when the application host is performing a graceful shutdown.</span></span> <span data-ttu-id="08be8-154">Le `cancellationToken` est signalé lorsque l’hôte décide de mettre fin au service de force.</span><span class="sxs-lookup"><span data-stu-id="08be8-154">The `cancellationToken` is signalled when the host decides to forcibly terminate the service.</span></span> <span data-ttu-id="08be8-155">Si cette méthode est substituée, vous **devez** appeler (et `await`) la méthode de la classe de base pour vous assurer que le service s’arrête correctement.</span><span class="sxs-lookup"><span data-stu-id="08be8-155">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service shuts down properly.</span></span>
* <span data-ttu-id="08be8-156">`StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` est appelé pour démarrer le service d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="08be8-156">`StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` is called to start the background service.</span></span> <span data-ttu-id="08be8-157">Le `cancellationToken` est signalé si le processus de démarrage est interrompu.</span><span class="sxs-lookup"><span data-stu-id="08be8-157">The `cancellationToken` is signalled if the startup process is interrupted.</span></span> <span data-ttu-id="08be8-158">L’implémentation retourne un `Task` qui représente le processus de démarrage pour le service.</span><span class="sxs-lookup"><span data-stu-id="08be8-158">The implementation returns a `Task` that represents the startup process for the service.</span></span> <span data-ttu-id="08be8-159">Aucun autre service n’est démarré tant que cette `Task` n’est pas terminée.</span><span class="sxs-lookup"><span data-stu-id="08be8-159">No further services are started until this `Task` completes.</span></span> <span data-ttu-id="08be8-160">Si cette méthode est substituée, vous **devez** appeler (et `await`) la méthode de la classe de base pour vous assurer que le service démarre correctement.</span><span class="sxs-lookup"><span data-stu-id="08be8-160">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service starts properly.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="08be8-161">Tâche d’arrière-plan avec minuteur</span><span class="sxs-lookup"><span data-stu-id="08be8-161">Timed background tasks</span></span>

<span data-ttu-id="08be8-162">Une tâche d’arrière-plan avec minuteur utilise la classe [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="08be8-162">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="08be8-163">Le minuteur déclenche la méthode `DoWork` de la tâche.</span><span class="sxs-lookup"><span data-stu-id="08be8-163">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="08be8-164">Le minuteur est désactivé sur `StopAsync` et supprimé quand le conteneur du service est supprimé sur `Dispose` :</span><span class="sxs-lookup"><span data-stu-id="08be8-164">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="08be8-165">Le service est inscrit dans `IHostBuilder.ConfigureServices` (*Program.cs*) avec la méthode d’extension `AddHostedService` :</span><span class="sxs-lookup"><span data-stu-id="08be8-165">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="08be8-166">Utilisation d’un service délimité dans une tâche d’arrière-plan</span><span class="sxs-lookup"><span data-stu-id="08be8-166">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="08be8-167">Pour utiliser les [services délimités](xref:fundamentals/dependency-injection#service-lifetimes) dans un `BackgroundService`, créez une étendue.</span><span class="sxs-lookup"><span data-stu-id="08be8-167">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a `BackgroundService`, create a scope.</span></span> <span data-ttu-id="08be8-168">Par défaut, aucune étendue n’est créée pour un service hébergé.</span><span class="sxs-lookup"><span data-stu-id="08be8-168">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="08be8-169">Le service des tâches d’arrière-plan délimitées contient la logique de la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="08be8-169">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="08be8-170">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="08be8-170">In the following example:</span></span>

* <span data-ttu-id="08be8-171">Le service est asynchrone.</span><span class="sxs-lookup"><span data-stu-id="08be8-171">The service is asynchronous.</span></span> <span data-ttu-id="08be8-172">La méthode `DoWork` retourne un `Task`.</span><span class="sxs-lookup"><span data-stu-id="08be8-172">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="08be8-173">À des fins de démonstration, un délai de dix secondes est attendu dans la méthode `DoWork`.</span><span class="sxs-lookup"><span data-stu-id="08be8-173">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="08be8-174">Une <xref:Microsoft.Extensions.Logging.ILogger> est injectée dans le service.</span><span class="sxs-lookup"><span data-stu-id="08be8-174">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="08be8-175">Le service hébergé crée une étendue pour résoudre le service de tâche d’arrière-plan étendu afin d’appeler sa méthode `DoWork`.</span><span class="sxs-lookup"><span data-stu-id="08be8-175">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="08be8-176">`DoWork` retourne une `Task`, qui est attendue dans `ExecuteAsync` :</span><span class="sxs-lookup"><span data-stu-id="08be8-176">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="08be8-177">Les services sont inscrits dans `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="08be8-177">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="08be8-178">Le service hébergé est inscrit avec la méthode d’extension `AddHostedService` :</span><span class="sxs-lookup"><span data-stu-id="08be8-178">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="08be8-179">Tâches d’arrière-plan en file d’attente</span><span class="sxs-lookup"><span data-stu-id="08be8-179">Queued background tasks</span></span>

<span data-ttu-id="08be8-180">Une file d’attente de tâches en arrière-plan est basée sur le .NET 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([prévu provisoirement pour être intégré pour ASP.net Core](https://github.com/aspnet/Hosting/issues/1280)) :</span><span class="sxs-lookup"><span data-stu-id="08be8-180">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="08be8-181">Dans l’exemple suivant `QueueHostedService` :</span><span class="sxs-lookup"><span data-stu-id="08be8-181">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="08be8-182">La méthode `BackgroundProcessing` retourne une `Task`, qui est attendue dans `ExecuteAsync`.</span><span class="sxs-lookup"><span data-stu-id="08be8-182">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="08be8-183">Les tâches en arrière-plan de la file d’attente sont déplacées en file d’attente et exécutées dans `BackgroundProcessing`.</span><span class="sxs-lookup"><span data-stu-id="08be8-183">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>
* <span data-ttu-id="08be8-184">Les éléments de travail sont attendus avant que le service ne s’arrête dans `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="08be8-184">Work items are awaited before the service stops in `StopAsync`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28-29,33)]

<span data-ttu-id="08be8-185">Un service `MonitorLoop` gère les tâches de mise en file d’attente pour le service hébergé chaque fois que la clé `w` est sélectionnée sur un appareil d’entrée :</span><span class="sxs-lookup"><span data-stu-id="08be8-185">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="08be8-186">La `IBackgroundTaskQueue` est injectée dans le service `MonitorLoop`.</span><span class="sxs-lookup"><span data-stu-id="08be8-186">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="08be8-187">`IBackgroundTaskQueue.QueueBackgroundWorkItem` est appelé pour empiler un élément de travail.</span><span class="sxs-lookup"><span data-stu-id="08be8-187">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>
* <span data-ttu-id="08be8-188">L’élément de travail simule une tâche en arrière-plan de longue durée :</span><span class="sxs-lookup"><span data-stu-id="08be8-188">The work item simulates a long-running background task:</span></span>
  * <span data-ttu-id="08be8-189">Trois retards de 5 secondes sont exécutés (`Task.Delay`).</span><span class="sxs-lookup"><span data-stu-id="08be8-189">Three 5-second delays are executed (`Task.Delay`).</span></span>
  * <span data-ttu-id="08be8-190">Une instruction `try-catch` intercepte <xref:System.OperationCanceledException> si la tâche est annulée.</span><span class="sxs-lookup"><span data-stu-id="08be8-190">A `try-catch` statement traps <xref:System.OperationCanceledException> if the task is cancelled.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="08be8-191">Les services sont inscrits dans `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="08be8-191">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="08be8-192">Le service hébergé est inscrit avec la méthode d’extension `AddHostedService` :</span><span class="sxs-lookup"><span data-stu-id="08be8-192">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="08be8-193">Dans ASP.NET Core, les tâches d’arrière-plan peuvent être implémentées en tant que *services hébergés*.</span><span class="sxs-lookup"><span data-stu-id="08be8-193">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="08be8-194">Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="08be8-194">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="08be8-195">Cette rubrique contient trois exemples de service hébergé :</span><span class="sxs-lookup"><span data-stu-id="08be8-195">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="08be8-196">Tâche d’arrière-plan qui s’exécute sur un minuteur.</span><span class="sxs-lookup"><span data-stu-id="08be8-196">Background task that runs on a timer.</span></span>
* <span data-ttu-id="08be8-197">Service hébergé qui active un [service délimité](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="08be8-197">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="08be8-198">Le service étendu peut utiliser l' [injection de dépendances (di)](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="08be8-198">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="08be8-199">Tâches d’arrière-plan en file d’attente qui s’exécutent séquentiellement.</span><span class="sxs-lookup"><span data-stu-id="08be8-199">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="08be8-200">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="08be8-200">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="08be8-201">Package</span><span class="sxs-lookup"><span data-stu-id="08be8-201">Package</span></span>

<span data-ttu-id="08be8-202">Référencer le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ou ajouter une référence de package au package [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting).</span><span class="sxs-lookup"><span data-stu-id="08be8-202">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="08be8-203">Interface IHostedService</span><span class="sxs-lookup"><span data-stu-id="08be8-203">IHostedService interface</span></span>

<span data-ttu-id="08be8-204">Les services hébergés implémentent l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="08be8-204">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="08be8-205">L’interface définit deux méthodes pour les objets qui sont gérés par l’hôte :</span><span class="sxs-lookup"><span data-stu-id="08be8-205">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="08be8-206">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash;`StartAsync` contient la logique pour démarrer la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="08be8-206">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="08be8-207">Quand [l’hôte web](xref:fundamentals/host/web-host) est utilisé, `StartAsync` est appelé après le démarrage du serveur et le déclenchement [d’IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*).</span><span class="sxs-lookup"><span data-stu-id="08be8-207">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="08be8-208">Quand [l’hôte générique](xref:fundamentals/host/generic-host) est utilisé, `StartAsync` est appelé avant le déclenchement de `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="08be8-208">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="08be8-209">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash;, déclenchée quand l’hôte effectue un arrêt normal.</span><span class="sxs-lookup"><span data-stu-id="08be8-209">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="08be8-210">`StopAsync` contient la logique pour terminer la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="08be8-210">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="08be8-211">Implémentez <xref:System.IDisposable> et les [finaliseurs (destructeurs)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) pour supprimer toutes les ressources non managées.</span><span class="sxs-lookup"><span data-stu-id="08be8-211">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="08be8-212">Le jeton d’annulation a un délai d’expiration par défaut de cinq secondes pour indiquer que le processus d’arrêt ne doit plus être normal.</span><span class="sxs-lookup"><span data-stu-id="08be8-212">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="08be8-213">Quand l’annulation est demandée sur le jeton :</span><span class="sxs-lookup"><span data-stu-id="08be8-213">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="08be8-214">Les opérations en arrière-plan restantes effectuées par l’application doivent être abandonnées.</span><span class="sxs-lookup"><span data-stu-id="08be8-214">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="08be8-215">Les méthodes appelées dans `StopAsync` doivent retourner rapidement.</span><span class="sxs-lookup"><span data-stu-id="08be8-215">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="08be8-216">Cependant, les tâches ne sont pas abandonnées après la demande d’annulation : l’appelant attend que toutes les tâches se terminent.</span><span class="sxs-lookup"><span data-stu-id="08be8-216">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="08be8-217">Si l’application s’arrête inopinément (par exemple en cas d’échec du processus de l’application), `StopAsync` n’est probablement pas appelée.</span><span class="sxs-lookup"><span data-stu-id="08be8-217">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="08be8-218">Par conséquent, les méthodes appelées ou les opérations effectuées dans `StopAsync` peuvent ne pas se produire.</span><span class="sxs-lookup"><span data-stu-id="08be8-218">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="08be8-219">Pour prolonger le délai d’expiration par défaut de cinq secondes, définissez :</span><span class="sxs-lookup"><span data-stu-id="08be8-219">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="08be8-220"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> quand vous utilisez l’hôte générique.</span><span class="sxs-lookup"><span data-stu-id="08be8-220"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="08be8-221">Pour plus d'informations, consultez <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="08be8-221">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="08be8-222">Le paramètre de configuration du délai d’expiration de l’hôte quand vous utilisez l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="08be8-222">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="08be8-223">Pour plus d'informations, consultez <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="08be8-223">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="08be8-224">Le service hébergé est activé une seule fois au démarrage de l’application et s’arrête normalement à l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="08be8-224">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="08be8-225">Si une erreur est levée pendant l’exécution des tâches d’arrière-plan, `Dispose` doit être appelée même si `StopAsync` n’est pas appelée.</span><span class="sxs-lookup"><span data-stu-id="08be8-225">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="08be8-226">Tâche d’arrière-plan avec minuteur</span><span class="sxs-lookup"><span data-stu-id="08be8-226">Timed background tasks</span></span>

<span data-ttu-id="08be8-227">Une tâche d’arrière-plan avec minuteur utilise la classe [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="08be8-227">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="08be8-228">Le minuteur déclenche la méthode `DoWork` de la tâche.</span><span class="sxs-lookup"><span data-stu-id="08be8-228">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="08be8-229">Le minuteur est désactivé sur `StopAsync` et supprimé quand le conteneur du service est supprimé sur `Dispose` :</span><span class="sxs-lookup"><span data-stu-id="08be8-229">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="08be8-230">Le service est inscrit dans `Startup.ConfigureServices` avec la méthode d’extension `AddHostedService` :</span><span class="sxs-lookup"><span data-stu-id="08be8-230">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="08be8-231">Utilisation d’un service délimité dans une tâche d’arrière-plan</span><span class="sxs-lookup"><span data-stu-id="08be8-231">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="08be8-232">Pour utiliser des [services délimités](xref:fundamentals/dependency-injection#service-lifetimes) au sein d’un `IHostedService`, créez une étendue.</span><span class="sxs-lookup"><span data-stu-id="08be8-232">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="08be8-233">Par défaut, aucune étendue n’est créée pour un service hébergé.</span><span class="sxs-lookup"><span data-stu-id="08be8-233">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="08be8-234">Le service des tâches d’arrière-plan délimitées contient la logique de la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="08be8-234">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="08be8-235">Dans l’exemple suivant, un <xref:Microsoft.Extensions.Logging.ILogger> est injecté dans le service :</span><span class="sxs-lookup"><span data-stu-id="08be8-235">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="08be8-236">Le service hébergé crée une étendue pour résoudre le service des tâches d’arrière-plan délimitées pour appeler sa méthode `DoWork` :</span><span class="sxs-lookup"><span data-stu-id="08be8-236">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="08be8-237">Les services sont inscrits dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="08be8-237">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="08be8-238">L’implémentation `IHostedService` est inscrite avec la méthode d’extension `AddHostedService` :</span><span class="sxs-lookup"><span data-stu-id="08be8-238">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="08be8-239">Tâches d’arrière-plan en file d’attente</span><span class="sxs-lookup"><span data-stu-id="08be8-239">Queued background tasks</span></span>

<span data-ttu-id="08be8-240">Une file d’attente de tâches en arrière-plan est basée sur le .NET Framework 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([prévu provisoirement pour être intégré pour ASP.net Core](https://github.com/aspnet/Hosting/issues/1280)) :</span><span class="sxs-lookup"><span data-stu-id="08be8-240">A background task queue is based on the .NET Framework 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="08be8-241">Dans `QueueHostedService`, les tâches d’arrière-plan dans la file d’attente sont retirées de la file d’attente et exécutées en tant que <xref:Microsoft.Extensions.Hosting.BackgroundService>, qui est une classe de base pour l’implémentation d’une exécution longue de `IHostedService` :</span><span class="sxs-lookup"><span data-stu-id="08be8-241">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="08be8-242">Les services sont inscrits dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="08be8-242">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="08be8-243">L’implémentation `IHostedService` est inscrite avec la méthode d’extension `AddHostedService` :</span><span class="sxs-lookup"><span data-stu-id="08be8-243">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="08be8-244">Dans la classe de modèle de page Index :</span><span class="sxs-lookup"><span data-stu-id="08be8-244">In the Index page model class:</span></span>

* <span data-ttu-id="08be8-245">`IBackgroundTaskQueue` est injecté dans le constructeur et affecté à `Queue`.</span><span class="sxs-lookup"><span data-stu-id="08be8-245">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="08be8-246">Un <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> est injecté et affecté à `_serviceScopeFactory`.</span><span class="sxs-lookup"><span data-stu-id="08be8-246">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="08be8-247">La fabrique est utilisée pour créer des instances de <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, qui servent à créer des services au sein d’une étendue.</span><span class="sxs-lookup"><span data-stu-id="08be8-247">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="08be8-248">Une étendue est créée de façon à utiliser l’élément `AppDbContext` ([service délimité](xref:fundamentals/dependency-injection#service-lifetimes)) de l’application pour écrire des enregistrements de base de données dans `IBackgroundTaskQueue` (service singleton).</span><span class="sxs-lookup"><span data-stu-id="08be8-248">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="08be8-249">Quand le bouton **Ajouter une tâche** est sélectionné dans la page Index, la méthode `OnPostAddTask` est exécutée.</span><span class="sxs-lookup"><span data-stu-id="08be8-249">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="08be8-250">`QueueBackgroundWorkItem` est appelé pour empiler un élément de travail :</span><span class="sxs-lookup"><span data-stu-id="08be8-250">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="08be8-251">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="08be8-251">Additional resources</span></span>

* [<span data-ttu-id="08be8-252">Implémenter des tâches d’arrière-plan dans des microservices avec IHostedService et la classe BackgroundService</span><span class="sxs-lookup"><span data-stu-id="08be8-252">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
