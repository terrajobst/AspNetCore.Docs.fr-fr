---
title: Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core
author: guardrex
description: Découvrez comment implémenter des tâches d’arrière-plan avec des services hébergés dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/18/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 8df86b10d7ba853edb3265df0e02eabbf8a2c058
ms.sourcegitcommit: fa61d882be9d0c48bd681f2efcb97e05522051d0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71205714"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="a5f7b-103">Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a5f7b-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="a5f7b-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a5f7b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a5f7b-105">Dans ASP.NET Core, les tâches d’arrière-plan peuvent être implémentées en tant que *services hébergés*.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="a5f7b-106">Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="a5f7b-107">Cette rubrique contient trois exemples de service hébergé :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="a5f7b-108">Tâche d’arrière-plan qui s’exécute sur un minuteur.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="a5f7b-109">Service hébergé qui active un [service délimité](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="a5f7b-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="a5f7b-110">Le service étendu peut utiliser l' [injection de dépendances (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a5f7b-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="a5f7b-111">Tâches d’arrière-plan en file d’attente qui s’exécutent séquentiellement.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="a5f7b-112">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a5f7b-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a5f7b-113">Cet exemple d’application est fourni en deux versions :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="a5f7b-114">Hôte web &ndash; L’hôte web est utile pour l’hébergement d’applications web.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="a5f7b-115">L’exemple de code présenté dans cette rubrique provient de la version de l’hôte web de l’exemple.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="a5f7b-116">Pour plus d’informations, consultez la rubrique [Hôte web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="a5f7b-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="a5f7b-117">Hôte générique &ndash; L’hôte générique est une nouveauté d’ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="a5f7b-118">Pour plus d’informations, consultez la rubrique [Hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="a5f7b-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="a5f7b-119">Modèle Service Worker</span><span class="sxs-lookup"><span data-stu-id="a5f7b-119">Worker Service template</span></span>

<span data-ttu-id="a5f7b-120">Le modèle Service Worker ASP.NET Core fournit un point de départ pour l’écriture d’applications de service durables.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="a5f7b-121">Pour utiliser le modèle en tant que base d’une application de services hébergés :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-121">To use the template as a basis for a hosted services app:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a5f7b-122">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a5f7b-122">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="a5f7b-123">Créer un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-123">Create a new project.</span></span>
1. <span data-ttu-id="a5f7b-124">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-124">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="a5f7b-125">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-125">Select **Next**.</span></span>
1. <span data-ttu-id="a5f7b-126">Indiquez un nom de projet dans le champ **Nom du projet**, ou acceptez le nom de projet par défaut.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-126">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="a5f7b-127">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-127">Select **Create**.</span></span>
1. <span data-ttu-id="a5f7b-128">Dans la boîte de dialogue **Créer une application web ASP.NET Core**, vérifiez que **.NET Core** et **ASP.NET Core 3.0** sont sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-128">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="a5f7b-129">Sélectionnez le modèle **Service Worker**.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-129">Select the **Worker Service** template.</span></span> <span data-ttu-id="a5f7b-130">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-130">Select **Create**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a5f7b-131">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="a5f7b-131">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="a5f7b-132">Créer un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-132">Create a new project.</span></span>
1. <span data-ttu-id="a5f7b-133">Sélectionnez **application** sous **.net Core** dans la barre latérale.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-133">Select **App** under **.NET Core** in the sidebar.</span></span>
1. <span data-ttu-id="a5f7b-134">Sélectionnez **Worker** sous **ASP.net Core**.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-134">Select **Worker** under **ASP.NET Core**.</span></span> <span data-ttu-id="a5f7b-135">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-135">Select **Next**.</span></span>
1. <span data-ttu-id="a5f7b-136">Sélectionnez **.net Core 3,0** pour la version cible de .NET **Framework**.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-136">Select **.NET Core 3.0** for the **Target Framework**.</span></span> <span data-ttu-id="a5f7b-137">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-137">Select **Next**.</span></span>
1. <span data-ttu-id="a5f7b-138">Indiquez un nom dans le champ **nom du projet** .</span><span class="sxs-lookup"><span data-stu-id="a5f7b-138">Provide a name in the **Project Name** field.</span></span> <span data-ttu-id="a5f7b-139">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-139">Select **Create**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a5f7b-140">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="a5f7b-140">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a5f7b-141">Utilisez le modèle Service Worker (`worker`) avec la commande [dotnet new](/dotnet/core/tools/dotnet-new) à partir d’un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-141">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="a5f7b-142">Dans l’exemple suivant, une application Service Worker est créée et se nomme `ContosoWorker`.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-142">In the following example, a Worker Service app is created named `ContosoWorker`.</span></span> <span data-ttu-id="a5f7b-143">Un dossier pour l’application `ContosoWorker` est créé automatiquement durant l’exécution de la commande.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-143">A folder for the `ContosoWorker` app is created automatically when the command is executed.</span></span>

```dotnetcli
dotnet new worker -o ContosoWorker
```

---

## <a name="package"></a><span data-ttu-id="a5f7b-144">Package</span><span class="sxs-lookup"><span data-stu-id="a5f7b-144">Package</span></span>

<span data-ttu-id="a5f7b-145">Une référence de package au package [Microsoft. extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) est ajoutée implicitement pour les applications ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-145">A package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is added implicitly for ASP.NET Core apps.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="a5f7b-146">Interface IHostedService</span><span class="sxs-lookup"><span data-stu-id="a5f7b-146">IHostedService interface</span></span>

<span data-ttu-id="a5f7b-147">L' <xref:Microsoft.Extensions.Hosting.IHostedService> interface définit deux méthodes pour les objets gérés par l’hôte :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-147">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="a5f7b-148">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash;`StartAsync` contient la logique pour démarrer la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-148">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="a5f7b-149">`StartAsync`est appelé *avant*:</span><span class="sxs-lookup"><span data-stu-id="a5f7b-149">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="a5f7b-150">Le pipeline de traitement des demandes de l'`Startup.Configure`application est configuré ().</span><span class="sxs-lookup"><span data-stu-id="a5f7b-150">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="a5f7b-151">Le serveur est démarré et [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) est déclenché.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-151">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="a5f7b-152">Le comportement par défaut peut être modifié de `StartAsync` sorte que le service hébergé s’exécute après que le pipeline de l’application a été configuré et `ApplicationStarted` qu’il est appelé.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-152">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="a5f7b-153">Pour modifier le comportement par défaut, ajoutez le service hébergé`VideosWatcher` (dans l’exemple suivant) après `ConfigureWebHostDefaults`l’appel de :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-153">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="a5f7b-154">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash;, déclenchée quand l’hôte effectue un arrêt normal.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-154">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="a5f7b-155">`StopAsync` contient la logique pour terminer la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-155">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="a5f7b-156">Implémentez <xref:System.IDisposable> et les [finaliseurs (destructeurs)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) pour supprimer toutes les ressources non managées.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-156">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="a5f7b-157">Le jeton d’annulation a un délai d’expiration par défaut de cinq secondes pour indiquer que le processus d’arrêt ne doit plus être normal.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-157">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="a5f7b-158">Quand l’annulation est demandée sur le jeton :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-158">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="a5f7b-159">Les opérations en arrière-plan restantes effectuées par l’application doivent être abandonnées.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-159">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="a5f7b-160">Les méthodes appelées dans `StopAsync` doivent retourner rapidement.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-160">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="a5f7b-161">Cependant, les tâches ne sont pas abandonnées après la demande d’annulation : l’appelant attend que toutes les tâches se terminent.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-161">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="a5f7b-162">Si l’application s’arrête inopinément (par exemple en cas d’échec du processus de l’application), `StopAsync` n’est probablement pas appelée.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-162">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="a5f7b-163">Par conséquent, les méthodes appelées ou les opérations effectuées dans `StopAsync` peuvent ne pas se produire.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-163">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="a5f7b-164">Pour prolonger le délai d’expiration par défaut de cinq secondes, définissez :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-164">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="a5f7b-165"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> quand vous utilisez l’hôte générique.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-165"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="a5f7b-166">Pour plus d'informations, consultez <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-166">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="a5f7b-167">Le paramètre de configuration du délai d’expiration de l’hôte quand vous utilisez l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-167">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="a5f7b-168">Pour plus d'informations, consultez <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-168">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="a5f7b-169">Le service hébergé est activé une seule fois au démarrage de l’application et s’arrête normalement à l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-169">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="a5f7b-170">Si une erreur est levée pendant l’exécution des tâches d’arrière-plan, `Dispose` doit être appelée même si `StopAsync` n’est pas appelée.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-170">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice"></a><span data-ttu-id="a5f7b-171">BackgroundService</span><span class="sxs-lookup"><span data-stu-id="a5f7b-171">BackgroundService</span></span>

<span data-ttu-id="a5f7b-172">`BackgroundService`est une classe de base pour l’implémentation d' <xref:Microsoft.Extensions.Hosting.IHostedService>une exécution longue.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-172">`BackgroundService` is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="a5f7b-173">`BackgroundService`définit deux méthodes pour les opérations d’arrière-plan :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-173">`BackgroundService` defines two methods for background operations:</span></span>

* <span data-ttu-id="a5f7b-174">`ExecuteAsync(CancellationToken stoppingToken)`&ndash; Appelélorsque<xref:Microsoft.Extensions.Hosting.IHostedService> démarre. `ExecuteAsync`</span><span class="sxs-lookup"><span data-stu-id="a5f7b-174">`ExecuteAsync(CancellationToken stoppingToken)` &ndash; `ExecuteAsync` Called when the <xref:Microsoft.Extensions.Hosting.IHostedService> starts.</span></span> <span data-ttu-id="a5f7b-175">L’implémentation doit retourner un `Task` qui représente la durée de vie des opérations de longue durée effectuées.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-175">The implementation should return a `Task` that represents the lifetime of the long running operations performed.</span></span> <span data-ttu-id="a5f7b-176">Déclenché `stoppingToken` lors de l’appel de [IHostedService. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) .</span><span class="sxs-lookup"><span data-stu-id="a5f7b-176">The `stoppingToken` Triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span>
* <span data-ttu-id="a5f7b-177">`StopAsync(CancellationToken stoppingToken)`&ndash; estdéclenchélorsquel’hôted’application`StopAsync` effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-177">`StopAsync(CancellationToken stoppingToken)` &ndash; `StopAsync` is triggered when the application host is performing a graceful shutdown.</span></span> <span data-ttu-id="a5f7b-178">`stoppingToken` Indique que le processus d’arrêt ne doit plus être normal.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-178">The `stoppingToken` indicates that the shutdown process should no longer be graceful.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="a5f7b-179">Tâche d’arrière-plan avec minuteur</span><span class="sxs-lookup"><span data-stu-id="a5f7b-179">Timed background tasks</span></span>

<span data-ttu-id="a5f7b-180">Une tâche d’arrière-plan avec minuteur utilise la classe [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="a5f7b-180">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="a5f7b-181">Le minuteur déclenche la méthode `DoWork` de la tâche.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-181">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="a5f7b-182">Le minuteur est désactivé sur `StopAsync` et supprimé quand le conteneur du service est supprimé sur `Dispose` :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-182">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="a5f7b-183">Le service est inscrit dans `IHostBuilder.ConfigureServices` (*Program.cs*) avec la `AddHostedService` méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-183">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="a5f7b-184">Utilisation d’un service délimité dans une tâche d’arrière-plan</span><span class="sxs-lookup"><span data-stu-id="a5f7b-184">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="a5f7b-185">Pour utiliser les [services délimités](xref:fundamentals/dependency-injection#service-lifetimes) dans un `BackgroundService`, créez une étendue.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-185">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a `BackgroundService`, create a scope.</span></span> <span data-ttu-id="a5f7b-186">Par défaut, aucune étendue n’est créée pour un service hébergé.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-186">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="a5f7b-187">Le service des tâches d’arrière-plan délimitées contient la logique de la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-187">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="a5f7b-188">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-188">In the following example:</span></span>

* <span data-ttu-id="a5f7b-189">Le service est asynchrone.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-189">The service is asynchronous.</span></span> <span data-ttu-id="a5f7b-190">La méthode `DoWork` retourne un `Task`.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-190">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="a5f7b-191">À des fins de démonstration, un délai de dix secondes est attendu dans `DoWork` la méthode.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-191">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="a5f7b-192">Un <xref:Microsoft.Extensions.Logging.ILogger> est injecté dans le service.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-192">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="a5f7b-193">Le service hébergé crée une étendue pour résoudre le service de tâche d’arrière-plan étendu `DoWork` afin d’appeler sa méthode.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-193">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="a5f7b-194">`DoWork`retourne un `Task`, qui est attendu dans `ExecuteAsync`:</span><span class="sxs-lookup"><span data-stu-id="a5f7b-194">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="a5f7b-195">Les services sont inscrits dans `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="a5f7b-195">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="a5f7b-196">Le service hébergé est inscrit avec la `AddHostedService` méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-196">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="a5f7b-197">Tâches d’arrière-plan en file d’attente</span><span class="sxs-lookup"><span data-stu-id="a5f7b-197">Queued background tasks</span></span>

<span data-ttu-id="a5f7b-198">Une file d’attente de tâches en arrière-plan est basée <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> sur le .net 4. x ([prévu provisoirement pour être intégré pour ASP.net Core](https://github.com/aspnet/Hosting/issues/1280)) :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-198">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="a5f7b-199">Dans l’exemple `QueueHostedService` suivant :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-199">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="a5f7b-200">La `BackgroundProcessing` méthode retourne un `Task`, qui est attendu dans `ExecuteAsync`.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-200">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="a5f7b-201">Les tâches en arrière-plan de la file d’attente sont `BackgroundProcessing`déplacées en file d’attente et exécutées dans.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-201">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28,39-40,44)]

<span data-ttu-id="a5f7b-202">Un `MonitorLoop` service gère les tâches de mise en file d’attente pour le `w` service hébergé chaque fois que la clé est sélectionnée sur un appareil d’entrée :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-202">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="a5f7b-203">Est injecté dans le `MonitorLoop` service. `IBackgroundTaskQueue`</span><span class="sxs-lookup"><span data-stu-id="a5f7b-203">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="a5f7b-204">`IBackgroundTaskQueue.QueueBackgroundWorkItem`est appelé pour empiler un élément de travail.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-204">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="a5f7b-205">Les services sont inscrits dans `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="a5f7b-205">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="a5f7b-206">Le service hébergé est inscrit avec la `AddHostedService` méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-206">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a5f7b-207">Dans ASP.NET Core, les tâches d’arrière-plan peuvent être implémentées en tant que *services hébergés*.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-207">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="a5f7b-208">Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-208">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="a5f7b-209">Cette rubrique contient trois exemples de service hébergé :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-209">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="a5f7b-210">Tâche d’arrière-plan qui s’exécute sur un minuteur.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-210">Background task that runs on a timer.</span></span>
* <span data-ttu-id="a5f7b-211">Service hébergé qui active un [service délimité](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="a5f7b-211">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="a5f7b-212">Le service étendu peut utiliser l' [injection de dépendances (di)](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="a5f7b-212">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="a5f7b-213">Tâches d’arrière-plan en file d’attente qui s’exécutent séquentiellement.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-213">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="a5f7b-214">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a5f7b-214">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a5f7b-215">Cet exemple d’application est fourni en deux versions :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-215">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="a5f7b-216">Hôte web &ndash; L’hôte web est utile pour l’hébergement d’applications web.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-216">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="a5f7b-217">L’exemple de code présenté dans cette rubrique provient de la version de l’hôte web de l’exemple.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-217">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="a5f7b-218">Pour plus d’informations, consultez la rubrique [Hôte web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="a5f7b-218">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="a5f7b-219">Hôte générique &ndash; L’hôte générique est une nouveauté d’ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-219">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="a5f7b-220">Pour plus d’informations, consultez la rubrique [Hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="a5f7b-220">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="a5f7b-221">Package</span><span class="sxs-lookup"><span data-stu-id="a5f7b-221">Package</span></span>

<span data-ttu-id="a5f7b-222">Référencer le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ou ajouter une référence de package au package [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting).</span><span class="sxs-lookup"><span data-stu-id="a5f7b-222">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="a5f7b-223">Interface IHostedService</span><span class="sxs-lookup"><span data-stu-id="a5f7b-223">IHostedService interface</span></span>

<span data-ttu-id="a5f7b-224">Les services hébergés implémentent l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-224">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="a5f7b-225">L’interface définit deux méthodes pour les objets qui sont gérés par l’hôte :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-225">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="a5f7b-226">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash;`StartAsync` contient la logique pour démarrer la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-226">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="a5f7b-227">Quand [l’hôte web](xref:fundamentals/host/web-host) est utilisé, `StartAsync` est appelé après le démarrage du serveur et le déclenchement [d’IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*).</span><span class="sxs-lookup"><span data-stu-id="a5f7b-227">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="a5f7b-228">Quand [l’hôte générique](xref:fundamentals/host/generic-host) est utilisé, `StartAsync` est appelé avant le déclenchement de `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-228">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="a5f7b-229">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash;, déclenchée quand l’hôte effectue un arrêt normal.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-229">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="a5f7b-230">`StopAsync` contient la logique pour terminer la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-230">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="a5f7b-231">Implémentez <xref:System.IDisposable> et les [finaliseurs (destructeurs)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) pour supprimer toutes les ressources non managées.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-231">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="a5f7b-232">Le jeton d’annulation a un délai d’expiration par défaut de cinq secondes pour indiquer que le processus d’arrêt ne doit plus être normal.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-232">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="a5f7b-233">Quand l’annulation est demandée sur le jeton :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-233">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="a5f7b-234">Les opérations en arrière-plan restantes effectuées par l’application doivent être abandonnées.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-234">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="a5f7b-235">Les méthodes appelées dans `StopAsync` doivent retourner rapidement.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-235">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="a5f7b-236">Cependant, les tâches ne sont pas abandonnées après la demande d’annulation : l’appelant attend que toutes les tâches se terminent.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-236">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="a5f7b-237">Si l’application s’arrête inopinément (par exemple en cas d’échec du processus de l’application), `StopAsync` n’est probablement pas appelée.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-237">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="a5f7b-238">Par conséquent, les méthodes appelées ou les opérations effectuées dans `StopAsync` peuvent ne pas se produire.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-238">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="a5f7b-239">Pour prolonger le délai d’expiration par défaut de cinq secondes, définissez :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-239">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="a5f7b-240"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> quand vous utilisez l’hôte générique.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-240"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="a5f7b-241">Pour plus d'informations, consultez <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-241">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="a5f7b-242">Le paramètre de configuration du délai d’expiration de l’hôte quand vous utilisez l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-242">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="a5f7b-243">Pour plus d'informations, consultez <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-243">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="a5f7b-244">Le service hébergé est activé une seule fois au démarrage de l’application et s’arrête normalement à l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-244">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="a5f7b-245">Si une erreur est levée pendant l’exécution des tâches d’arrière-plan, `Dispose` doit être appelée même si `StopAsync` n’est pas appelée.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-245">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="a5f7b-246">Tâche d’arrière-plan avec minuteur</span><span class="sxs-lookup"><span data-stu-id="a5f7b-246">Timed background tasks</span></span>

<span data-ttu-id="a5f7b-247">Une tâche d’arrière-plan avec minuteur utilise la classe [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="a5f7b-247">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="a5f7b-248">Le minuteur déclenche la méthode `DoWork` de la tâche.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-248">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="a5f7b-249">Le minuteur est désactivé sur `StopAsync` et supprimé quand le conteneur du service est supprimé sur `Dispose` :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-249">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="a5f7b-250">Le service est inscrit dans `Startup.ConfigureServices` avec la méthode d’extension `AddHostedService` :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-250">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="a5f7b-251">Utilisation d’un service délimité dans une tâche d’arrière-plan</span><span class="sxs-lookup"><span data-stu-id="a5f7b-251">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="a5f7b-252">Pour utiliser des [services délimités](xref:fundamentals/dependency-injection#service-lifetimes) au sein d’un `IHostedService`, créez une étendue.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-252">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="a5f7b-253">Par défaut, aucune étendue n’est créée pour un service hébergé.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-253">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="a5f7b-254">Le service des tâches d’arrière-plan délimitées contient la logique de la tâche d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-254">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="a5f7b-255">Dans l’exemple suivant, un <xref:Microsoft.Extensions.Logging.ILogger> est injecté dans le service :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-255">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="a5f7b-256">Le service hébergé crée une étendue pour résoudre le service des tâches d’arrière-plan délimitées pour appeler sa méthode `DoWork` :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-256">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="a5f7b-257">Les services sont inscrits dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-257">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a5f7b-258">L’implémentation `IHostedService` est inscrite avec la méthode d’extension `AddHostedService` :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-258">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="a5f7b-259">Tâches d’arrière-plan en file d’attente</span><span class="sxs-lookup"><span data-stu-id="a5f7b-259">Queued background tasks</span></span>

<span data-ttu-id="a5f7b-260">Une file d’attente de tâches en arrière-plan est basée <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> sur le .net 4. x ([prévu provisoirement pour être intégré pour ASP.net Core](https://github.com/aspnet/Hosting/issues/1280)) :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-260">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="a5f7b-261">Dans `QueueHostedService`, les tâches d’arrière-plan dans la file d’attente sont retirées de la file d’attente et exécutées en tant que <xref:Microsoft.Extensions.Hosting.BackgroundService>, qui est une classe de base pour l’implémentation d’une exécution longue de `IHostedService` :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-261">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="a5f7b-262">Les services sont inscrits dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-262">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a5f7b-263">L’implémentation `IHostedService` est inscrite avec la méthode d’extension `AddHostedService` :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-263">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="a5f7b-264">Dans la classe de modèle de page Index :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-264">In the Index page model class:</span></span>

* <span data-ttu-id="a5f7b-265">`IBackgroundTaskQueue` est injecté dans le constructeur et affecté à `Queue`.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-265">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="a5f7b-266">Un <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> est injecté et affecté à `_serviceScopeFactory`.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-266">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="a5f7b-267">La fabrique est utilisée pour créer des instances de <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, qui servent à créer des services au sein d’une étendue.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-267">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="a5f7b-268">Une étendue est créée de façon à utiliser l’élément `AppDbContext` ([service délimité](xref:fundamentals/dependency-injection#service-lifetimes)) de l’application pour écrire des enregistrements de base de données dans `IBackgroundTaskQueue` (service singleton).</span><span class="sxs-lookup"><span data-stu-id="a5f7b-268">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="a5f7b-269">Quand le bouton **Ajouter une tâche** est sélectionné dans la page Index, la méthode `OnPostAddTask` est exécutée.</span><span class="sxs-lookup"><span data-stu-id="a5f7b-269">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="a5f7b-270">`QueueBackgroundWorkItem`est appelé pour empiler un élément de travail :</span><span class="sxs-lookup"><span data-stu-id="a5f7b-270">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="a5f7b-271">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="a5f7b-271">Additional resources</span></span>

* [<span data-ttu-id="a5f7b-272">Implémenter des tâches d’arrière-plan dans des microservices avec IHostedService et la classe BackgroundService</span><span class="sxs-lookup"><span data-stu-id="a5f7b-272">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
