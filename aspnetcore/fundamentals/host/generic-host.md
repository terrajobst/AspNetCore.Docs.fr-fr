---
title: Hôte générique .NET
author: tdykstra
description: Découvrez l’hôte générique .NET Core, qui est responsable de la gestion du démarrage et de la durée de vie des applications.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: fundamentals/host/generic-host
ms.openlocfilehash: 75af6dc58d31aaad888b14640268bf05c193272d
ms.sourcegitcommit: e54672f5c493258dc449fac5b98faf47eb123b28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71248285"
---
# <a name="net-generic-host"></a><span data-ttu-id="66dbf-103">Hôte générique .NET</span><span class="sxs-lookup"><span data-stu-id="66dbf-103">.NET Generic Host</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="66dbf-104">Cet article présente l’hôte générique .NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>) et fournit des conseils sur son utilisation.</span><span class="sxs-lookup"><span data-stu-id="66dbf-104">This article introduces the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) and provides guidance on how to use it.</span></span>

## <a name="whats-a-host"></a><span data-ttu-id="66dbf-105">Qu’est-ce qu’un hôte ?</span><span class="sxs-lookup"><span data-stu-id="66dbf-105">What's a host?</span></span>

<span data-ttu-id="66dbf-106">Un *hôte* est un objet qui encapsule les ressources de l’application, telles que :</span><span class="sxs-lookup"><span data-stu-id="66dbf-106">A *host* is an object that encapsulates an app's resources, such as:</span></span>

* <span data-ttu-id="66dbf-107">Injection de dépendances (DI)</span><span class="sxs-lookup"><span data-stu-id="66dbf-107">Dependency injection (DI)</span></span>
* <span data-ttu-id="66dbf-108">Journalisation</span><span class="sxs-lookup"><span data-stu-id="66dbf-108">Logging</span></span>
* <span data-ttu-id="66dbf-109">Configuration</span><span class="sxs-lookup"><span data-stu-id="66dbf-109">Configuration</span></span>
* <span data-ttu-id="66dbf-110">Implémentations de `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="66dbf-110">`IHostedService` implementations</span></span>

<span data-ttu-id="66dbf-111">Quand un hôte démarre, il appelle `IHostedService.StartAsync` sur chaque implémentation de <xref:Microsoft.Extensions.Hosting.IHostedService> qu’il trouve dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="66dbf-111">When a host starts, it calls `IHostedService.StartAsync` on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> that it finds in the DI container.</span></span> <span data-ttu-id="66dbf-112">Dans une application web, l’une des implémentations de `IHostedService` est un service web qui démarre une [implémentation de serveur HTTP](xref:fundamentals/index#servers).</span><span class="sxs-lookup"><span data-stu-id="66dbf-112">In a web app, one of the `IHostedService` implementations is a web service that starts an [HTTP server implementation](xref:fundamentals/index#servers).</span></span>

<span data-ttu-id="66dbf-113">La principale raison d’inclure toutes les ressources interdépendantes de l’application dans un objet est la gestion de la durée de vie : contrôler le démarrage de l’application et l’arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="66dbf-113">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="66dbf-114">Dans les versions d’ASP.NET Core antérieures à 3.0, l’[hôte web](xref:fundamentals/host/web-host) est utilisé pour les charges de travail HTTP.</span><span class="sxs-lookup"><span data-stu-id="66dbf-114">In versions of ASP.NET Core earlier than 3.0, the [Web Host](xref:fundamentals/host/web-host) is used for HTTP workloads.</span></span> <span data-ttu-id="66dbf-115">L’hôte web n’est plus recommandé pour les applications web, mais reste disponible uniquement à des fins de compatibilité descendante.</span><span class="sxs-lookup"><span data-stu-id="66dbf-115">The Web Host is no longer recommended for web apps and remains available only for backward compatibility.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="66dbf-116">Configurer un hôte</span><span class="sxs-lookup"><span data-stu-id="66dbf-116">Set up a host</span></span>

<span data-ttu-id="66dbf-117">L’hôte est généralement configuré, généré et exécuté par du code dans la classe `Program`.</span><span class="sxs-lookup"><span data-stu-id="66dbf-117">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="66dbf-118">La méthode `Main` :</span><span class="sxs-lookup"><span data-stu-id="66dbf-118">The `Main` method:</span></span>

* <span data-ttu-id="66dbf-119">Appelle une méthode `CreateHostBuilder` pour créer et configurer un objet builder.</span><span class="sxs-lookup"><span data-stu-id="66dbf-119">Calls a `CreateHostBuilder` method to create and configure a builder object.</span></span>
* <span data-ttu-id="66dbf-120">Appelle les méthodes `Build` et `Run` sur l’objet builder.</span><span class="sxs-lookup"><span data-stu-id="66dbf-120">Calls `Build` and `Run` methods on the builder object.</span></span>

<span data-ttu-id="66dbf-121">Voici le code de *Program.cs* pour une charge de travail non-HTTP, avec une seule implémentation `IHostedService` ajoutée au conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="66dbf-121">Here's *Program.cs* code for a non-HTTP workload, with a single `IHostedService` implementation added to the DI container.</span></span> 

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureServices((hostContext, services) =>
            {
               services.AddHostedService<Worker>();
            });
}
```

<span data-ttu-id="66dbf-122">Pour une charge de travail HTTP, la méthode `Main` est la même, mais `CreateHostBuilder` appelle `ConfigureWebHostDefaults` :</span><span class="sxs-lookup"><span data-stu-id="66dbf-122">For an HTTP workload, the `Main` method is the same but `CreateHostBuilder` calls `ConfigureWebHostDefaults`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="66dbf-123">Si l’application utilise Entity Framework Core, ne changez pas le nom ou la signature de la méthode `CreateHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="66dbf-123">If the app uses Entity Framework Core, don't change the name or signature of the `CreateHostBuilder` method.</span></span> <span data-ttu-id="66dbf-124">Les [outils Entity Framework Core tools](/ef/core/miscellaneous/cli/) s’attendent à trouver une méthode `CreateHostBuilder` qui configure l’hôte sans exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="66dbf-124">The [Entity Framework Core tools](/ef/core/miscellaneous/cli/) expect to find a `CreateHostBuilder` method that configures the host without running the app.</span></span> <span data-ttu-id="66dbf-125">Pour plus d’informations, consultez [Création de DbContext au moment du design](/ef/core/miscellaneous/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="66dbf-125">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

## <a name="default-builder-settings"></a><span data-ttu-id="66dbf-126">Paramètres du générateur par défaut</span><span class="sxs-lookup"><span data-stu-id="66dbf-126">Default builder settings</span></span> 

<span data-ttu-id="66dbf-127">La méthode <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> :</span><span class="sxs-lookup"><span data-stu-id="66dbf-127">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> method:</span></span>

* <span data-ttu-id="66dbf-128">Définit le chemin retourné par <xref:System.IO.Directory.GetCurrentDirectory*> comme racine de contenu.</span><span class="sxs-lookup"><span data-stu-id="66dbf-128">Sets the content root to the path returned by <xref:System.IO.Directory.GetCurrentDirectory*>.</span></span>
* <span data-ttu-id="66dbf-129">Charge la configuration de l’hôte à partir de :</span><span class="sxs-lookup"><span data-stu-id="66dbf-129">Loads host configuration from:</span></span>
  * <span data-ttu-id="66dbf-130">Variables d’environnement comportant le préfixe « DOTNET_ ».</span><span class="sxs-lookup"><span data-stu-id="66dbf-130">Environment variables prefixed with "DOTNET_".</span></span>
  * <span data-ttu-id="66dbf-131">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="66dbf-131">Command-line arguments.</span></span>
* <span data-ttu-id="66dbf-132">Charge la configuration de l’application à partir de :</span><span class="sxs-lookup"><span data-stu-id="66dbf-132">Loads app configuration from:</span></span>
  * <span data-ttu-id="66dbf-133">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="66dbf-133">*appsettings.json*.</span></span>
  * <span data-ttu-id="66dbf-134">*appsettings.{Environment}.json*</span><span class="sxs-lookup"><span data-stu-id="66dbf-134">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="66dbf-135">[Secret Manager](xref:security/app-secrets) quand l’application s’exécute dans l’environnement `Development`.</span><span class="sxs-lookup"><span data-stu-id="66dbf-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="66dbf-136">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="66dbf-136">Environment variables.</span></span>
  * <span data-ttu-id="66dbf-137">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="66dbf-137">Command-line arguments.</span></span>
* <span data-ttu-id="66dbf-138">Ajoute les fournisseurs de [journalisation](xref:fundamentals/logging/index) suivants :</span><span class="sxs-lookup"><span data-stu-id="66dbf-138">Adds the following [logging](xref:fundamentals/logging/index) providers:</span></span>
  * <span data-ttu-id="66dbf-139">Console</span><span class="sxs-lookup"><span data-stu-id="66dbf-139">Console</span></span>
  * <span data-ttu-id="66dbf-140">Débogage</span><span class="sxs-lookup"><span data-stu-id="66dbf-140">Debug</span></span>
  * <span data-ttu-id="66dbf-141">EventSource</span><span class="sxs-lookup"><span data-stu-id="66dbf-141">EventSource</span></span>
  * <span data-ttu-id="66dbf-142">EventLog (uniquement en cas d’exécution sur Windows)</span><span class="sxs-lookup"><span data-stu-id="66dbf-142">EventLog (only when running on Windows)</span></span>
* <span data-ttu-id="66dbf-143">Active la [validation de l’étendue](xref:fundamentals/dependency-injection#scope-validation) et la [validation de dépendances](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) dans un environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="66dbf-143">Enables [scope validation](xref:fundamentals/dependency-injection#scope-validation) and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is Development.</span></span>

<span data-ttu-id="66dbf-144">La méthode `ConfigureWebHostDefaults` :</span><span class="sxs-lookup"><span data-stu-id="66dbf-144">The `ConfigureWebHostDefaults` method:</span></span>

* <span data-ttu-id="66dbf-145">Charge la configuration hôte à partir des variables d’environnement comportant le préfixe « ASPNETCORE_ ».</span><span class="sxs-lookup"><span data-stu-id="66dbf-145">Loads host configuration from environment variables prefixed with "ASPNETCORE_".</span></span>
* <span data-ttu-id="66dbf-146">Définit le serveur [Kestrel](xref:fundamentals/servers/kestrel) comme serveur web et le configure en utilisant les fournisseurs de configuration d’hébergement de l’application.</span><span class="sxs-lookup"><span data-stu-id="66dbf-146">Sets [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and configures it using the app's hosting configuration providers.</span></span> <span data-ttu-id="66dbf-147">Pour connaître les options par défaut du serveur Kestrel, consultez <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-147">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="66dbf-148">Ajoute l’[intergiciel (middleware) Filtrage d’hôtes](xref:fundamentals/servers/kestrel#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="66dbf-148">Adds [Host Filtering middleware](xref:fundamentals/servers/kestrel#host-filtering).</span></span>
* <span data-ttu-id="66dbf-149">Ajoute l’[intergiciel (middleware) En-têtes transférés](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) si ASPNETCORE_FORWARDEDHEADERS_ENABLED est true.</span><span class="sxs-lookup"><span data-stu-id="66dbf-149">Adds [Forwarded Headers middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) if ASPNETCORE_FORWARDEDHEADERS_ENABLED=true.</span></span>
* <span data-ttu-id="66dbf-150">Permet l’intégration d’IIS.</span><span class="sxs-lookup"><span data-stu-id="66dbf-150">Enables IIS integration.</span></span> <span data-ttu-id="66dbf-151">Pour connaître les options par défaut d’IIS, consultez <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-151">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>

<span data-ttu-id="66dbf-152">Les sections [Paramètres pour tous les types d’applications](#settings-for-all-app-types) et [Paramètres pour les applications web](#settings-for-web-apps) figurant plus loin dans cet article montrent comment remplacer les paramètres du générateur par défaut.</span><span class="sxs-lookup"><span data-stu-id="66dbf-152">The [Settings for all app types](#settings-for-all-app-types) and [Settings for web apps](#settings-for-web-apps) sections later in this article show how to override default builder settings.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="66dbf-153">Services fournis par le framework</span><span class="sxs-lookup"><span data-stu-id="66dbf-153">Framework-provided services</span></span>

<span data-ttu-id="66dbf-154">Les services inscrits automatiquement sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="66dbf-154">Services that are registered automatically include the following:</span></span>

* [<span data-ttu-id="66dbf-155">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="66dbf-155">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
* [<span data-ttu-id="66dbf-156">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="66dbf-156">IHostLifetime</span></span>](#ihostlifetime)
* [<span data-ttu-id="66dbf-157">IHostEnvironment / IWebHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="66dbf-157">IHostEnvironment / IWebHostEnvironment</span></span>](#ihostenvironment)

<span data-ttu-id="66dbf-158">Pour plus d’informations sur les services fournis par le <xref:fundamentals/dependency-injection#framework-provided-services>Framework, consultez.</span><span class="sxs-lookup"><span data-stu-id="66dbf-158">For more information on framework-provided services, see <xref:fundamentals/dependency-injection#framework-provided-services>.</span></span>

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="66dbf-159">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="66dbf-159">IHostApplicationLifetime</span></span>

<span data-ttu-id="66dbf-160">Injectez le service <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (anciennement `IApplicationLifetime`) dans n’importe quelle classe pour gérer les tâches post-démarrage et d’arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="66dbf-160">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (formerly `IApplicationLifetime`) service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="66dbf-161">Trois propriétés de l’interface sont des jetons d’annulation utilisés pour inscrire les méthodes du gestionnaire d’événements de démarrage et d’arrêt d’application.</span><span class="sxs-lookup"><span data-stu-id="66dbf-161">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="66dbf-162">L’interface inclut également une méthode `StopApplication`.</span><span class="sxs-lookup"><span data-stu-id="66dbf-162">The interface also includes a `StopApplication` method.</span></span>

<span data-ttu-id="66dbf-163">L’exemple suivant est une implémentation de `IHostedService` qui inscrit les événements `IApplicationLifetime` :</span><span class="sxs-lookup"><span data-stu-id="66dbf-163">The following example is an `IHostedService` implementation that registers the `IApplicationLifetime` events:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a><span data-ttu-id="66dbf-164">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="66dbf-164">IHostLifetime</span></span>

<span data-ttu-id="66dbf-165">L’implémentation de <xref:Microsoft.Extensions.Hosting.IHostLifetime> contrôle quand l’hôte démarre et quand il s’arrête.</span><span class="sxs-lookup"><span data-stu-id="66dbf-165">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="66dbf-166">La dernière implémentation inscrite est utilisée.</span><span class="sxs-lookup"><span data-stu-id="66dbf-166">The last implementation registered is used.</span></span>

<span data-ttu-id="66dbf-167"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> est l’implémentation de `IHostLifetime` par défaut.</span><span class="sxs-lookup"><span data-stu-id="66dbf-167"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="66dbf-168">`ConsoleLifetime`:</span><span class="sxs-lookup"><span data-stu-id="66dbf-168">`ConsoleLifetime`:</span></span>

* <span data-ttu-id="66dbf-169">écoute Ctrl+C/SIGINT ou SIGTERM et appelle <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> pour démarrer le processus d’arrêt.</span><span class="sxs-lookup"><span data-stu-id="66dbf-169">listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span>
* <span data-ttu-id="66dbf-170">Déverrouille les extensions telles que [RunAsync](#runasync) et [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="66dbf-170">Unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="66dbf-171">IHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="66dbf-171">IHostEnvironment</span></span>

<span data-ttu-id="66dbf-172">Injectez le service <xref:Microsoft.Extensions.Hosting.IHostEnvironment> dans une classe pour obtenir des informations sur ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="66dbf-172">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following:</span></span>

* [<span data-ttu-id="66dbf-173">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="66dbf-173">ApplicationName</span></span>](#applicationname)
* [<span data-ttu-id="66dbf-174">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="66dbf-174">EnvironmentName</span></span>](#environmentname)
* [<span data-ttu-id="66dbf-175">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="66dbf-175">ContentRootPath</span></span>](#contentrootpath)

<span data-ttu-id="66dbf-176">Les applications web implémentent l’interface `IWebHostEnvironment`, qui hérite de `IHostEnvironment` et ajoute :</span><span class="sxs-lookup"><span data-stu-id="66dbf-176">Web apps implement the `IWebHostEnvironment` interface, which inherits `IHostEnvironment` and adds:</span></span>

* [<span data-ttu-id="66dbf-177">WebRootPath</span><span class="sxs-lookup"><span data-stu-id="66dbf-177">WebRootPath</span></span>](#webroot)

## <a name="host-configuration"></a><span data-ttu-id="66dbf-178">Configuration de l’hôte</span><span class="sxs-lookup"><span data-stu-id="66dbf-178">Host configuration</span></span>

<span data-ttu-id="66dbf-179">La configuration de l’hôte est utilisée pour les propriétés de l’implémentation de <xref:Microsoft.Extensions.Hosting.IHostEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-179">Host configuration is used for the properties of the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementation.</span></span>

<span data-ttu-id="66dbf-180">La configuration de l’hôte est disponible à partir de [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) dans <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-180">Host configuration is available from [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) inside <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span></span> <span data-ttu-id="66dbf-181">Après `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` est remplacé par la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="66dbf-181">After `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` is replaced with the app config.</span></span>

<span data-ttu-id="66dbf-182">Pour ajouter la configuration d’hôte, appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> sur `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="66dbf-182">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="66dbf-183">`ConfigureHostConfiguration` peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="66dbf-183">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="66dbf-184">L’hôte utilise l’option qui définit une valeur en dernier sur une clé donnée.</span><span class="sxs-lookup"><span data-stu-id="66dbf-184">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="66dbf-185">Le fournisseur de variables d’environnement avec le préfixe `DOTNET_` et les arguments de ligne de commande sont inclus par CreateDefaultBuilder.</span><span class="sxs-lookup"><span data-stu-id="66dbf-185">The environment variable provider with prefix `DOTNET_` and command line args are included by CreateDefaultBuilder.</span></span> <span data-ttu-id="66dbf-186">Pour les applications web, le fournisseur de variables d’environnement avec le préfixe `ASPNETCORE_` est ajouté.</span><span class="sxs-lookup"><span data-stu-id="66dbf-186">For web apps, the environment variable provider with prefix `ASPNETCORE_` is added.</span></span> <span data-ttu-id="66dbf-187">Ce préfixe est supprimé à la lecture des variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="66dbf-187">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="66dbf-188">Par exemple, la valeur de variable d’environnement de `ASPNETCORE_ENVIRONMENT` devient la valeur de configuration d’hôte de la clé `environment`.</span><span class="sxs-lookup"><span data-stu-id="66dbf-188">For example, the environment variable value for `ASPNETCORE_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="66dbf-189">L’exemple suivant crée la configuration d’hôte :</span><span class="sxs-lookup"><span data-stu-id="66dbf-189">The following example creates host configuration:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a><span data-ttu-id="66dbf-190">Configuration de l’application</span><span class="sxs-lookup"><span data-stu-id="66dbf-190">App configuration</span></span>

<span data-ttu-id="66dbf-191">La configuration d’application est créée en appelant <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> sur `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="66dbf-191">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="66dbf-192">`ConfigureAppConfiguration` peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="66dbf-192">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="66dbf-193">L’application utilise l’option qui définit une valeur en dernier sur une clé donnée.</span><span class="sxs-lookup"><span data-stu-id="66dbf-193">The app uses whichever option sets a value last on a given key.</span></span> 

<span data-ttu-id="66dbf-194">La configuration créée par `ConfigureAppConfiguration` est disponible dans [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) pour les opérations suivantes et en tant que service à partir de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="66dbf-194">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="66dbf-195">La configuration d’hôte est également ajoutée à la configuration d’application.</span><span class="sxs-lookup"><span data-stu-id="66dbf-195">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="66dbf-196">Pour plus d’informations, consultez [Configuration dans ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span><span class="sxs-lookup"><span data-stu-id="66dbf-196">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span></span>

## <a name="settings-for-all-app-types"></a><span data-ttu-id="66dbf-197">Paramètres pour tous les types d’applications</span><span class="sxs-lookup"><span data-stu-id="66dbf-197">Settings for all app types</span></span>

<span data-ttu-id="66dbf-198">Cette section liste les paramètres d’hôte qui s’appliquent aux charges de travail HTTP et non-HTTP.</span><span class="sxs-lookup"><span data-stu-id="66dbf-198">This section lists host settings that apply to both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="66dbf-199">Par défaut, les variables d’environnement utilisées pour configurer ces paramètres peuvent avoir un préfixe `DOTNET_` ou `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="66dbf-199">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<!-- In the following sections, two spaces at end of line are used to force line breaks in the rendered page. -->

### <a name="applicationname"></a><span data-ttu-id="66dbf-200">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="66dbf-200">ApplicationName</span></span>

<span data-ttu-id="66dbf-201">La propriété [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) est définie à partir de la configuration d’hôte pendant la construction de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="66dbf-201">The [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span>

<span data-ttu-id="66dbf-202">**Clé** : applicationName</span><span class="sxs-lookup"><span data-stu-id="66dbf-202">**Key**: applicationName</span></span>  
<span data-ttu-id="66dbf-203">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="66dbf-203">**Type**: *string*</span></span>  
<span data-ttu-id="66dbf-204">**Par défaut** : nom de l’assembly contenant le point d’entrée de l’application.</span><span class="sxs-lookup"><span data-stu-id="66dbf-204">**Default**: The name of the assembly that contains the app's entry point.</span></span>
<span data-ttu-id="66dbf-205">**Variable d’environnement** : `<PREFIX_>APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="66dbf-205">**Environment variable**: `<PREFIX_>APPLICATIONNAME`</span></span>

<span data-ttu-id="66dbf-206">Pour définir cette valeur, utilisez la variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="66dbf-206">To set this value, use the environment variable.</span></span> 

### <a name="contentrootpath"></a><span data-ttu-id="66dbf-207">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="66dbf-207">ContentRootPath</span></span>

<span data-ttu-id="66dbf-208">La propriété [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) détermine où l’hôte commence à rechercher les fichiers de contenu.</span><span class="sxs-lookup"><span data-stu-id="66dbf-208">The [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) property determines where the host begins searching for content files.</span></span> <span data-ttu-id="66dbf-209">Si le chemin est introuvable, l’hôte ne peut pas démarrer.</span><span class="sxs-lookup"><span data-stu-id="66dbf-209">If the path doesn't exist, the host fails to start.</span></span>

<span data-ttu-id="66dbf-210">**Clé** : contentRoot</span><span class="sxs-lookup"><span data-stu-id="66dbf-210">**Key**: contentRoot</span></span>  
<span data-ttu-id="66dbf-211">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="66dbf-211">**Type**: *string*</span></span>  
<span data-ttu-id="66dbf-212">**Par défaut** : dossier où réside l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="66dbf-212">**Default**: The folder where the app assembly resides.</span></span>  
<span data-ttu-id="66dbf-213">**Variable d’environnement** : `<PREFIX_>CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="66dbf-213">**Environment variable**: `<PREFIX_>CONTENTROOT`</span></span>

<span data-ttu-id="66dbf-214">Pour définir cette valeur, utilisez la variable d’environnement ou appelez `UseContentRoot` sur `IHostBuilder` :</span><span class="sxs-lookup"><span data-stu-id="66dbf-214">To set this value, use the environment variable or call `UseContentRoot` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

### <a name="environmentname"></a><span data-ttu-id="66dbf-215">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="66dbf-215">EnvironmentName</span></span>

<span data-ttu-id="66dbf-216">La propriété [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) peut être définie avec n’importe quelle valeur.</span><span class="sxs-lookup"><span data-stu-id="66dbf-216">The [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) property can be set to any value.</span></span> <span data-ttu-id="66dbf-217">Les valeurs définies par le framework sont `Development`, `Staging` et `Production`.</span><span class="sxs-lookup"><span data-stu-id="66dbf-217">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="66dbf-218">Les valeurs ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="66dbf-218">Values aren't case sensitive.</span></span>

<span data-ttu-id="66dbf-219">**Clé** : environment</span><span class="sxs-lookup"><span data-stu-id="66dbf-219">**Key**: environment</span></span>  
<span data-ttu-id="66dbf-220">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="66dbf-220">**Type**: *string*</span></span>  
<span data-ttu-id="66dbf-221">**Par défaut** : Production</span><span class="sxs-lookup"><span data-stu-id="66dbf-221">**Default**: Production</span></span>  
<span data-ttu-id="66dbf-222">**Variable d’environnement** : `<PREFIX_>ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="66dbf-222">**Environment variable**: `<PREFIX_>ENVIRONMENT`</span></span>

<span data-ttu-id="66dbf-223">Pour définir cette valeur, utilisez la variable d’environnement ou appelez `UseEnvironment` sur `IHostBuilder` :</span><span class="sxs-lookup"><span data-stu-id="66dbf-223">To set this value, use the environment variable or call `UseEnvironment` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a><span data-ttu-id="66dbf-224">ShutdownTimeout</span><span class="sxs-lookup"><span data-stu-id="66dbf-224">ShutdownTimeout</span></span>

<span data-ttu-id="66dbf-225">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) définit le délai d’expiration pour <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-225">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="66dbf-226">La valeur par défaut est de cinq secondes.</span><span class="sxs-lookup"><span data-stu-id="66dbf-226">The default value is five seconds.</span></span>  <span data-ttu-id="66dbf-227">Pendant la période du délai d’expiration, l’hôte :</span><span class="sxs-lookup"><span data-stu-id="66dbf-227">During the timeout period, the host:</span></span>

* <span data-ttu-id="66dbf-228">Déclenche [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="66dbf-228">Triggers [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="66dbf-229">Tente d’arrêter les services hébergés, en journalisant les erreurs pour les services qui échouent à s’arrêter.</span><span class="sxs-lookup"><span data-stu-id="66dbf-229">Attempts to stop hosted services, logging errors for services that fail to stop.</span></span>

<span data-ttu-id="66dbf-230">Si la période du délai d’attente expire avant l’arrêt de tous les services hébergés, les services actifs restants sont arrêtés quand l’application s’arrête.</span><span class="sxs-lookup"><span data-stu-id="66dbf-230">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="66dbf-231">Les services s’arrêtent même s’ils n’ont pas terminé les traitements.</span><span class="sxs-lookup"><span data-stu-id="66dbf-231">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="66dbf-232">Si des services nécessitent plus de temps pour s’arrêter, augmentez le délai d’attente.</span><span class="sxs-lookup"><span data-stu-id="66dbf-232">If services require additional time to stop, increase the timeout.</span></span>

<span data-ttu-id="66dbf-233">**Clé** : shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="66dbf-233">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="66dbf-234">**Type** : *int*</span><span class="sxs-lookup"><span data-stu-id="66dbf-234">**Type**: *int*</span></span>  
<span data-ttu-id="66dbf-235">**Par défaut** : 5 secondes **Variable d’environnement** : `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="66dbf-235">**Default**: 5 seconds **Environment variable**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="66dbf-236">Pour définir cette valeur, utilisez la variable d’environnement ou configurez `HostOptions`.</span><span class="sxs-lookup"><span data-stu-id="66dbf-236">To set this value, use the environment variable or configure `HostOptions`.</span></span> <span data-ttu-id="66dbf-237">L’exemple suivant définit un délai d’expiration de 20 secondes :</span><span class="sxs-lookup"><span data-stu-id="66dbf-237">The following example sets the timeout to 20 seconds:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

## <a name="settings-for-web-apps"></a><span data-ttu-id="66dbf-238">Paramètres pour les applications web</span><span class="sxs-lookup"><span data-stu-id="66dbf-238">Settings for web apps</span></span>

<span data-ttu-id="66dbf-239">Certains paramètres d’hôte s’appliquent uniquement aux charges de travail HTTP.</span><span class="sxs-lookup"><span data-stu-id="66dbf-239">Some host settings apply only to HTTP workloads.</span></span> <span data-ttu-id="66dbf-240">Par défaut, les variables d’environnement utilisées pour configurer ces paramètres peuvent avoir un préfixe `DOTNET_` ou `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="66dbf-240">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<span data-ttu-id="66dbf-241">Des méthodes d’extension sur `IWebHostBuilder` sont disponibles pour ces paramètres.</span><span class="sxs-lookup"><span data-stu-id="66dbf-241">Extension methods on `IWebHostBuilder` are available for these settings.</span></span> <span data-ttu-id="66dbf-242">Les exemples de code qui montrent comment appeler les méthodes d’extension supposent que `webBuilder` est une instance de `IWebHostBuilder`, comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="66dbf-242">Code samples that show how to call the extension methods assume `webBuilder` is an instance of `IWebHostBuilder`, as in the following example:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a><span data-ttu-id="66dbf-243">CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="66dbf-243">CaptureStartupErrors</span></span>

<span data-ttu-id="66dbf-244">Avec la valeur `false`, la survenue d’erreurs au démarrage entraîne la fermeture de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="66dbf-244">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="66dbf-245">Avec la valeur `true`, l’hôte capture les exceptions levées au démarrage et tente de démarrer le serveur.</span><span class="sxs-lookup"><span data-stu-id="66dbf-245">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

<span data-ttu-id="66dbf-246">**Clé** : captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="66dbf-246">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="66dbf-247">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="66dbf-247">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="66dbf-248">**Par défaut** : `false`, ou `true` si l’application s’exécute avec Kestrel derrière IIS.</span><span class="sxs-lookup"><span data-stu-id="66dbf-248">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="66dbf-249">**Variable d’environnement** : `<PREFIX_>CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="66dbf-249">**Environment variable**: `<PREFIX_>CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="66dbf-250">Pour définir cette valeur, utilisez la configuration ou appelez `CaptureStartupErrors` :</span><span class="sxs-lookup"><span data-stu-id="66dbf-250">To set this value, use configuration or call `CaptureStartupErrors`:</span></span>

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a><span data-ttu-id="66dbf-251">DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="66dbf-251">DetailedErrors</span></span>

<span data-ttu-id="66dbf-252">En cas d’activation ou quand l’environnement est `Development`, l’application capture des erreurs détaillées.</span><span class="sxs-lookup"><span data-stu-id="66dbf-252">When enabled, or when the environment is `Development`, the app captures detailed errors.</span></span>

<span data-ttu-id="66dbf-253">**Clé** : detailedErrors</span><span class="sxs-lookup"><span data-stu-id="66dbf-253">**Key**: detailedErrors</span></span>  
<span data-ttu-id="66dbf-254">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="66dbf-254">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="66dbf-255">**Valeur par défaut** : false</span><span class="sxs-lookup"><span data-stu-id="66dbf-255">**Default**: false</span></span>  
<span data-ttu-id="66dbf-256">**Variable d’environnement** : `<PREFIX_>_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="66dbf-256">**Environment variable**: `<PREFIX_>_DETAILEDERRORS`</span></span>

<span data-ttu-id="66dbf-257">Pour définir cette valeur, utilisez la configuration ou appelez `UseSetting` :</span><span class="sxs-lookup"><span data-stu-id="66dbf-257">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a><span data-ttu-id="66dbf-258">HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="66dbf-258">HostingStartupAssemblies</span></span>

<span data-ttu-id="66dbf-259">Chaîne délimitée par des points-virgules qui spécifie les assemblys d’hébergement à charger au démarrage.</span><span class="sxs-lookup"><span data-stu-id="66dbf-259">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="66dbf-260">La valeur de configuration par défaut est une chaîne vide, mais les assemblys d’hébergement au démarrage incluent toujours l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="66dbf-260">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="66dbf-261">Quand des assemblys d’hébergement au démarrage sont fournis, ils sont ajoutés à l’assembly de l’application et sont chargés lorsque l’application génère ses services communs au démarrage.</span><span class="sxs-lookup"><span data-stu-id="66dbf-261">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

<span data-ttu-id="66dbf-262">**Clé** : hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="66dbf-262">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="66dbf-263">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="66dbf-263">**Type**: *string*</span></span>  
<span data-ttu-id="66dbf-264">**Par défaut** : Chaîne vide</span><span class="sxs-lookup"><span data-stu-id="66dbf-264">**Default**: Empty string</span></span>  
<span data-ttu-id="66dbf-265">**Variable d’environnement** : `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="66dbf-265">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="66dbf-266">Pour définir cette valeur, utilisez la configuration ou appelez `UseSetting` :</span><span class="sxs-lookup"><span data-stu-id="66dbf-266">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a><span data-ttu-id="66dbf-267">HostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="66dbf-267">HostingStartupExcludeAssemblies</span></span>

<span data-ttu-id="66dbf-268">Chaîne délimitée par des points-virgules qui spécifie les assemblys d’hébergement à exclure au démarrage.</span><span class="sxs-lookup"><span data-stu-id="66dbf-268">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="66dbf-269">**Clé** : hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="66dbf-269">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="66dbf-270">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="66dbf-270">**Type**: *string*</span></span>  
<span data-ttu-id="66dbf-271">**Par défaut** : Chaîne vide</span><span class="sxs-lookup"><span data-stu-id="66dbf-271">**Default**: Empty string</span></span>  
<span data-ttu-id="66dbf-272">**Variable d’environnement** : `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="66dbf-272">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="66dbf-273">Pour définir cette valeur, utilisez la configuration ou appelez `UseSetting` :</span><span class="sxs-lookup"><span data-stu-id="66dbf-273">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a><span data-ttu-id="66dbf-274">HTTPS_Port</span><span class="sxs-lookup"><span data-stu-id="66dbf-274">HTTPS_Port</span></span>

<span data-ttu-id="66dbf-275">Port de redirection HTTPS.</span><span class="sxs-lookup"><span data-stu-id="66dbf-275">The HTTPS redirect port.</span></span> <span data-ttu-id="66dbf-276">Utilisé dans [l’application de HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="66dbf-276">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="66dbf-277">**Clé**: https_port</span><span class="sxs-lookup"><span data-stu-id="66dbf-277">**Key**: https_port</span></span>  
<span data-ttu-id="66dbf-278">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="66dbf-278">**Type**: *string*</span></span>  
<span data-ttu-id="66dbf-279">**Par défaut** : Aucune valeur par défaut n’est définie.</span><span class="sxs-lookup"><span data-stu-id="66dbf-279">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="66dbf-280">**Variable d’environnement** : `<PREFIX_>HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="66dbf-280">**Environment variable**: `<PREFIX_>HTTPS_PORT`</span></span>

<span data-ttu-id="66dbf-281">Pour définir cette valeur, utilisez la configuration ou appelez `UseSetting` :</span><span class="sxs-lookup"><span data-stu-id="66dbf-281">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a><span data-ttu-id="66dbf-282">PreferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="66dbf-282">PreferHostingUrls</span></span>

<span data-ttu-id="66dbf-283">Indique si l’hôte doit écouter les URL configurées avec `IWebHostBuilder` au lieu d’écouter celles configurées avec l’implémentation `IServer`.</span><span class="sxs-lookup"><span data-stu-id="66dbf-283">Indicates whether the host should listen on the URLs configured with the `IWebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="66dbf-284">**Clé** : preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="66dbf-284">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="66dbf-285">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="66dbf-285">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="66dbf-286">**Valeur par défaut** : true</span><span class="sxs-lookup"><span data-stu-id="66dbf-286">**Default**: true</span></span>  
<span data-ttu-id="66dbf-287">**Variable d’environnement** : `<PREFIX_>_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="66dbf-287">**Environment variable**: `<PREFIX_>_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="66dbf-288">Pour définir cette valeur, utilisez la variable d’environnement ou appelez `PreferHostingUrls` :</span><span class="sxs-lookup"><span data-stu-id="66dbf-288">To set this value, use the environment variable or call `PreferHostingUrls`:</span></span>

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a><span data-ttu-id="66dbf-289">PreventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="66dbf-289">PreventHostingStartup</span></span>

<span data-ttu-id="66dbf-290">Empêche le chargement automatique des assemblys d’hébergement au démarrage, y compris ceux configurés par l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="66dbf-290">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="66dbf-291">Pour plus d'informations, consultez <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-291">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="66dbf-292">**Clé** : preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="66dbf-292">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="66dbf-293">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="66dbf-293">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="66dbf-294">**Valeur par défaut** : false</span><span class="sxs-lookup"><span data-stu-id="66dbf-294">**Default**: false</span></span>  
<span data-ttu-id="66dbf-295">**Variable d’environnement** : `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="66dbf-295">**Environment variable**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="66dbf-296">Pour définir cette valeur, utilisez la variable d’environnement ou appelez `UseSetting` :</span><span class="sxs-lookup"><span data-stu-id="66dbf-296">To set this value, use the environment variable or call `UseSetting` :</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a><span data-ttu-id="66dbf-297">StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="66dbf-297">StartupAssembly</span></span>

<span data-ttu-id="66dbf-298">Assembly où rechercher la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="66dbf-298">The assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="66dbf-299">**Clé** : startupAssembly</span><span class="sxs-lookup"><span data-stu-id="66dbf-299">**Key**: startupAssembly</span></span>  
<span data-ttu-id="66dbf-300">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="66dbf-300">**Type**: *string*</span></span>  
<span data-ttu-id="66dbf-301">**Par défaut** : l’assembly de l’application</span><span class="sxs-lookup"><span data-stu-id="66dbf-301">**Default**: The app's assembly</span></span>  
<span data-ttu-id="66dbf-302">**Variable d’environnement** : `<PREFIX_>STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="66dbf-302">**Environment variable**: `<PREFIX_>STARTUPASSEMBLY`</span></span>

<span data-ttu-id="66dbf-303">Pour définir cette valeur, utilisez la variable d’environnement ou appelez `UseStartup`.</span><span class="sxs-lookup"><span data-stu-id="66dbf-303">To set this value, use the environment variable or call `UseStartup`.</span></span> <span data-ttu-id="66dbf-304">`UseStartup` peut prendre un nom d’assembly (`string`) ou un type (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="66dbf-304">`UseStartup` can take an assembly name (`string`) or a type (`TStartup`).</span></span> <span data-ttu-id="66dbf-305">Si plusieurs méthodes `UseStartup` sont appelées, la dernière prévaut.</span><span class="sxs-lookup"><span data-stu-id="66dbf-305">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a><span data-ttu-id="66dbf-306">URL</span><span class="sxs-lookup"><span data-stu-id="66dbf-306">URLs</span></span>

<span data-ttu-id="66dbf-307">Liste délimitée par des points-virgules d’adresses IP ou d’adresses d’hôte avec les ports et protocoles sur lesquels le serveur doit écouter les requêtes.</span><span class="sxs-lookup"><span data-stu-id="66dbf-307">A semicolon-delimited list of IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span> <span data-ttu-id="66dbf-308">Par exemple, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="66dbf-308">For example, `http://localhost:123`.</span></span> <span data-ttu-id="66dbf-309">Utilisez « \* » pour indiquer que le serveur doit écouter les requêtes sur toutes les adresses IP ou tous les noms d’hôte qui utilisent le port et le protocole spécifiés (par exemple, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="66dbf-309">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="66dbf-310">Le protocole (`http://` ou `https://`) doit être inclus avec chaque URL.</span><span class="sxs-lookup"><span data-stu-id="66dbf-310">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="66dbf-311">Les formats pris en charge varient selon les serveurs.</span><span class="sxs-lookup"><span data-stu-id="66dbf-311">Supported formats vary among servers.</span></span>

<span data-ttu-id="66dbf-312">**Clé** : urls</span><span class="sxs-lookup"><span data-stu-id="66dbf-312">**Key**: urls</span></span>  
<span data-ttu-id="66dbf-313">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="66dbf-313">**Type**: *string*</span></span>  
<span data-ttu-id="66dbf-314">**Par défaut**: `http://localhost:5000` et`https://localhost:5001`</span><span class="sxs-lookup"><span data-stu-id="66dbf-314">**Default**: `http://localhost:5000` and `https://localhost:5001`</span></span>  
<span data-ttu-id="66dbf-315">**Variable d’environnement** : `<PREFIX_>URLS`</span><span class="sxs-lookup"><span data-stu-id="66dbf-315">**Environment variable**: `<PREFIX_>URLS`</span></span>

<span data-ttu-id="66dbf-316">Pour définir cette valeur, utilisez la variable d’environnement ou appelez `UseUrls` :</span><span class="sxs-lookup"><span data-stu-id="66dbf-316">To set this value, use the environment variable or call `UseUrls`:</span></span>

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

<span data-ttu-id="66dbf-317">Kestrel a sa propre API de configuration de points de terminaison.</span><span class="sxs-lookup"><span data-stu-id="66dbf-317">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="66dbf-318">Pour plus d'informations, consultez <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-318">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="webroot"></a><span data-ttu-id="66dbf-319">WebRoot</span><span class="sxs-lookup"><span data-stu-id="66dbf-319">WebRoot</span></span>

<span data-ttu-id="66dbf-320">Chemin relatif des ressources statiques de l’application.</span><span class="sxs-lookup"><span data-stu-id="66dbf-320">The relative path to the app's static assets.</span></span>

<span data-ttu-id="66dbf-321">**Clé** : webroot</span><span class="sxs-lookup"><span data-stu-id="66dbf-321">**Key**: webroot</span></span>  
<span data-ttu-id="66dbf-322">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="66dbf-322">**Type**: *string*</span></span>  
<span data-ttu-id="66dbf-323">**Par défaut** : *(Racine de contenu)/wwwroot*, si le chemin d’accès existe.</span><span class="sxs-lookup"><span data-stu-id="66dbf-323">**Default**: *(Content Root)/wwwroot*, if the path exists.</span></span> <span data-ttu-id="66dbf-324">Si ce chemin n’existe pas, un fournisseur de fichiers no-op est utilisé.</span><span class="sxs-lookup"><span data-stu-id="66dbf-324">If the path doesn't exist, a no-op file provider is used.</span></span>  
<span data-ttu-id="66dbf-325">**Variable d’environnement** : `<PREFIX_>WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="66dbf-325">**Environment variable**: `<PREFIX_>WEBROOT`</span></span>

<span data-ttu-id="66dbf-326">Pour définir cette valeur, utilisez la variable d’environnement ou appelez `UseWebRoot` :</span><span class="sxs-lookup"><span data-stu-id="66dbf-326">To set this value, use the environment variable or call `UseWebRoot`:</span></span>

```csharp
webBuilder.UseWebRoot("public");
```

## <a name="manage-the-host-lifetime"></a><span data-ttu-id="66dbf-327">Gérer la durée de vie de l’hôte</span><span class="sxs-lookup"><span data-stu-id="66dbf-327">Manage the host lifetime</span></span>

<span data-ttu-id="66dbf-328">Appelez des méthodes sur l’implémentation de <xref:Microsoft.Extensions.Hosting.IHost> pour démarrer et arrêter l’application.</span><span class="sxs-lookup"><span data-stu-id="66dbf-328">Call methods on the built <xref:Microsoft.Extensions.Hosting.IHost> implementation to start and stop the app.</span></span> <span data-ttu-id="66dbf-329">Ces méthodes affectent toutes les implémentations de <xref:Microsoft.Extensions.Hosting.IHostedService> qui sont inscrites dans le conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="66dbf-329">These methods affect all  <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="66dbf-330">Exécuter</span><span class="sxs-lookup"><span data-stu-id="66dbf-330">Run</span></span>

<span data-ttu-id="66dbf-331"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> exécute l’application et bloque le thread appelant jusqu’à l’arrêt de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="66dbf-331"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down.</span></span>

### <a name="runasync"></a><span data-ttu-id="66dbf-332">RunAsync</span><span class="sxs-lookup"><span data-stu-id="66dbf-332">RunAsync</span></span>

<span data-ttu-id="66dbf-333"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> exécute l’application et retourne un <xref:System.Threading.Tasks.Task> qui est effectué quand le jeton d’annulation ou l’arrêt est déclenché.</span><span class="sxs-lookup"><span data-stu-id="66dbf-333"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span>

### <a name="runconsoleasync"></a><span data-ttu-id="66dbf-334">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="66dbf-334">RunConsoleAsync</span></span>

<span data-ttu-id="66dbf-335"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> permet la prise en charge de la console, génère et démarre l’hôte et attend Ctrl+C/SIGINT ou SIGTERM pour l’arrêter.</span><span class="sxs-lookup"><span data-stu-id="66dbf-335"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

### <a name="start"></a><span data-ttu-id="66dbf-336">Start</span><span class="sxs-lookup"><span data-stu-id="66dbf-336">Start</span></span>

<span data-ttu-id="66dbf-337"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> démarre l’hôte en mode synchrone.</span><span class="sxs-lookup"><span data-stu-id="66dbf-337"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

### <a name="startasync"></a><span data-ttu-id="66dbf-338">StartAsync</span><span class="sxs-lookup"><span data-stu-id="66dbf-338">StartAsync</span></span>

<span data-ttu-id="66dbf-339"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> démarre l’hôte et retourne un <xref:System.Threading.Tasks.Task> qui est effectué quand le jeton d’annulation ou l’arrêt est déclenché.</span><span class="sxs-lookup"><span data-stu-id="66dbf-339"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the host and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span> 

<span data-ttu-id="66dbf-340"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> est appelé au début de `StartAsync`, lequel attend qu’il soit fini avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="66dbf-340"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of `StartAsync`, which waits until it's complete before continuing.</span></span> <span data-ttu-id="66dbf-341">Cela permet de retarder le démarrage jusqu'à ce que celui-ci soit signalé par un événement externe.</span><span class="sxs-lookup"><span data-stu-id="66dbf-341">This can be used to delay startup until signaled by an external event.</span></span>

### <a name="stopasync"></a><span data-ttu-id="66dbf-342">StopAsync</span><span class="sxs-lookup"><span data-stu-id="66dbf-342">StopAsync</span></span>

<span data-ttu-id="66dbf-343"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> tente d’arrêter l’hôte dans le délai d’attente fourni.</span><span class="sxs-lookup"><span data-stu-id="66dbf-343"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

### <a name="waitforshutdown"></a><span data-ttu-id="66dbf-344">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="66dbf-344">WaitForShutdown</span></span>

<span data-ttu-id="66dbf-345"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> bloque le thread appelant jusqu’à ce qu’un arrêt soit déclenché par IHostLifetime, par exemple par le biais de Ctrl+C/SIGINT ou de SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="66dbf-345"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocks the calling thread until shutdown is triggered by the IHostLifetime, such as via Ctrl+C/SIGINT or SIGTERM.</span></span>

### <a name="waitforshutdownasync"></a><span data-ttu-id="66dbf-346">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="66dbf-346">WaitForShutdownAsync</span></span>

<span data-ttu-id="66dbf-347"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> retourne une <xref:System.Threading.Tasks.Task> qui est effectuée quand l’arrêt est déclenché par le biais du jeton fourni et appelle <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-347"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

### <a name="external-control"></a><span data-ttu-id="66dbf-348">Contrôle externe</span><span class="sxs-lookup"><span data-stu-id="66dbf-348">External control</span></span>

<span data-ttu-id="66dbf-349">Le contrôle direct de la durée de vie de l’hôte peut être effectué à l’aide de méthodes pouvant être appelées de façon externe :</span><span class="sxs-lookup"><span data-stu-id="66dbf-349">Direct control of the host lifetime can be achieved using methods that can be called externally:</span></span>

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="66dbf-350">Les applications ASP.NET Core configurent et lancent un hôte.</span><span class="sxs-lookup"><span data-stu-id="66dbf-350">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="66dbf-351">L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="66dbf-351">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="66dbf-352">Cet article traite de l’hôte générique ASP.NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>), qui est utilisé pour les applications qui ne traitent pas les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="66dbf-352">This article covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is used for apps that don't process HTTP requests.</span></span>

<span data-ttu-id="66dbf-353">L’objectif de l’hôte générique est de séparer le pipeline HTTP de l’API Hôte web pour permettre un plus large éventail de scénarios d’hôte.</span><span class="sxs-lookup"><span data-stu-id="66dbf-353">The purpose of Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="66dbf-354">La messagerie, les tâches en arrière-plan et autres charges de travail non HTTP basées sur l’hôte générique bénéficient de fonctionnalités transversales, comme la configuration, l’injection de dépendances (DI) et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="66dbf-354">Messaging, background tasks, and other non-HTTP workloads based on Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="66dbf-355">L’hôte générique est nouveau dans ASP.NET Core 2.1 et n’est pas adapté aux scénarios d’hébergement web.</span><span class="sxs-lookup"><span data-stu-id="66dbf-355">Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="66dbf-356">Pour les scénarios d’hébergement de web, utilisez l’[hôte web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="66dbf-356">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="66dbf-357">L’hôte générique est appelé à remplacer l’hôte web dans une version ultérieure et à servir d’API hôte principale dans les scénarios HTTP et non-HTTP.</span><span class="sxs-lookup"><span data-stu-id="66dbf-357">Generic Host will replace Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="66dbf-358">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="66dbf-358">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="66dbf-359">Quand vous exécutez l’exemple d’application dans [Visual Studio Code](https://code.visualstudio.com/), utilisez un *terminal externe ou intégré*.</span><span class="sxs-lookup"><span data-stu-id="66dbf-359">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="66dbf-360">N’exécutez pas l’exemple dans une `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="66dbf-360">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="66dbf-361">Pour définir la console dans Visual Studio Code :</span><span class="sxs-lookup"><span data-stu-id="66dbf-361">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="66dbf-362">Ouvrez le fichier *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="66dbf-362">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="66dbf-363">Dans la configuration **.NET Core Launch (console)** , recherchez l’entrée **console**.</span><span class="sxs-lookup"><span data-stu-id="66dbf-363">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="66dbf-364">Définissez la valeur avec `externalTerminal` ou `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="66dbf-364">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="66dbf-365">Introduction</span><span class="sxs-lookup"><span data-stu-id="66dbf-365">Introduction</span></span>

<span data-ttu-id="66dbf-366">La bibliothèque de l’hôte générique est disponible dans l’espace de noms <xref:Microsoft.Extensions.Hosting> et est fournie par le package [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span><span class="sxs-lookup"><span data-stu-id="66dbf-366">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="66dbf-367">Le package `Microsoft.Extensions.Hosting` est inclus dans le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 ou ultérieur).</span><span class="sxs-lookup"><span data-stu-id="66dbf-367">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="66dbf-368"><xref:Microsoft.Extensions.Hosting.IHostedService> est le point d’entrée pour exécuter le code.</span><span class="sxs-lookup"><span data-stu-id="66dbf-368"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="66dbf-369">Chaque implémentation <xref:Microsoft.Extensions.Hosting.IHostedService> est exécutée dans l’ordre d’[inscription des services dans ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="66dbf-369">Each <xref:Microsoft.Extensions.Hosting.IHostedService> implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="66dbf-370"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> est appelé sur chaque <xref:Microsoft.Extensions.Hosting.IHostedService> au démarrage de l’hôte, tandis que <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> est appelé dans l’ordre d’inscription inverse quand l’hôte s’arrête normalement.</span><span class="sxs-lookup"><span data-stu-id="66dbf-370"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each <xref:Microsoft.Extensions.Hosting.IHostedService> when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="66dbf-371">Configurer un hôte</span><span class="sxs-lookup"><span data-stu-id="66dbf-371">Set up a host</span></span>

<span data-ttu-id="66dbf-372"><xref:Microsoft.Extensions.Hosting.IHostBuilder> est le principal composant que les applications et les bibliothèques utilisent pour initialiser, générer et exécuter l’hôte :</span><span class="sxs-lookup"><span data-stu-id="66dbf-372"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="66dbf-373">Options</span><span class="sxs-lookup"><span data-stu-id="66dbf-373">Options</span></span>

<span data-ttu-id="66dbf-374"><xref:Microsoft.Extensions.Hosting.HostOptions> configure les options pour <xref:Microsoft.Extensions.Hosting.IHost>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-374"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="66dbf-375">Délai d’expiration</span><span class="sxs-lookup"><span data-stu-id="66dbf-375">Shutdown timeout</span></span>

<span data-ttu-id="66dbf-376"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> définit le délai d’expiration pour <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-376"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="66dbf-377">La valeur par défaut est de cinq secondes.</span><span class="sxs-lookup"><span data-stu-id="66dbf-377">The default value is five seconds.</span></span>

<span data-ttu-id="66dbf-378">La configuration de l’option suivante dans `Program.Main` augmente le délai d’expiration par défaut de 5 à 20 secondes :</span><span class="sxs-lookup"><span data-stu-id="66dbf-378">The following option configuration in `Program.Main` increases the default five second shutdown timeout to 20 seconds:</span></span>

```csharp
var host = new HostBuilder()
    .ConfigureServices((hostContext, services) =>
    {
        services.Configure<HostOptions>(option =>
        {
            option.ShutdownTimeout = System.TimeSpan.FromSeconds(20);
        });
    })
    .Build();
```

## <a name="default-services"></a><span data-ttu-id="66dbf-379">Services par défaut</span><span class="sxs-lookup"><span data-stu-id="66dbf-379">Default services</span></span>

<span data-ttu-id="66dbf-380">Les services suivants sont inscrits au moment de l’initialisation de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="66dbf-380">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="66dbf-381">[Environnement](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="66dbf-381">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="66dbf-382">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="66dbf-382">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="66dbf-383"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="66dbf-383"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="66dbf-384"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="66dbf-384"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="66dbf-385">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="66dbf-385">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="66dbf-386">[Journalisation](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="66dbf-386">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="66dbf-387">Configuration de l’hôte</span><span class="sxs-lookup"><span data-stu-id="66dbf-387">Host configuration</span></span>

<span data-ttu-id="66dbf-388">La création de la configuration d’hôte fait appel aux opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="66dbf-388">Host configuration is created by:</span></span>

* <span data-ttu-id="66dbf-389">Appel de méthodes d’extension sur <xref:Microsoft.Extensions.Hosting.IHostBuilder> pour définir la [racine du contenu](#content-root) et [l’environnement](#environment).</span><span class="sxs-lookup"><span data-stu-id="66dbf-389">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="66dbf-390">Lecture de la configuration à partir des fournisseurs de configuration dans <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-390">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="66dbf-391">Méthodes d’extension</span><span class="sxs-lookup"><span data-stu-id="66dbf-391">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="66dbf-392">Clé d’application (nom)</span><span class="sxs-lookup"><span data-stu-id="66dbf-392">Application key (name)</span></span>

<span data-ttu-id="66dbf-393">La propriété [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) est définie à partir de la configuration de l’hôte pendant la construction de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="66dbf-393">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="66dbf-394">Pour définir la valeur explicitement, utilisez [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey) :</span><span class="sxs-lookup"><span data-stu-id="66dbf-394">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="66dbf-395">**Clé** : applicationName</span><span class="sxs-lookup"><span data-stu-id="66dbf-395">**Key**: applicationName</span></span>  
<span data-ttu-id="66dbf-396">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="66dbf-396">**Type**: *string*</span></span>  
<span data-ttu-id="66dbf-397">**Par défaut** : nom de l’assembly contenant le point d’entrée de l’application.</span><span class="sxs-lookup"><span data-stu-id="66dbf-397">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="66dbf-398">**Définition avec** : `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="66dbf-398">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="66dbf-399">**Variable d’environnement** : `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` est [facultatif et défini par l’utilisateur](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="66dbf-399">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="66dbf-400">Racine de contenu</span><span class="sxs-lookup"><span data-stu-id="66dbf-400">Content root</span></span>

<span data-ttu-id="66dbf-401">Ce paramètre détermine où l’hôte commence la recherche des fichiers de contenu.</span><span class="sxs-lookup"><span data-stu-id="66dbf-401">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="66dbf-402">**Clé** : contentRoot</span><span class="sxs-lookup"><span data-stu-id="66dbf-402">**Key**: contentRoot</span></span>  
<span data-ttu-id="66dbf-403">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="66dbf-403">**Type**: *string*</span></span>  
<span data-ttu-id="66dbf-404">**Par défaut** : dossier où réside l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="66dbf-404">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="66dbf-405">**Définition avec** : `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="66dbf-405">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="66dbf-406">**Variable d’environnement** : `<PREFIX_>CONTENTROOT` (`<PREFIX_>` est [facultatif et défini par l’utilisateur](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="66dbf-406">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="66dbf-407">Si le chemin est introuvable, l’hôte ne peut pas démarrer.</span><span class="sxs-lookup"><span data-stu-id="66dbf-407">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

### <a name="environment"></a><span data-ttu-id="66dbf-408">Environnement</span><span class="sxs-lookup"><span data-stu-id="66dbf-408">Environment</span></span>

<span data-ttu-id="66dbf-409">Définit l’[environnement](xref:fundamentals/environments) de l’application.</span><span class="sxs-lookup"><span data-stu-id="66dbf-409">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="66dbf-410">**Clé** : environment</span><span class="sxs-lookup"><span data-stu-id="66dbf-410">**Key**: environment</span></span>  
<span data-ttu-id="66dbf-411">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="66dbf-411">**Type**: *string*</span></span>  
<span data-ttu-id="66dbf-412">**Par défaut** : Production</span><span class="sxs-lookup"><span data-stu-id="66dbf-412">**Default**: Production</span></span>  
<span data-ttu-id="66dbf-413">**Définition avec** : `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="66dbf-413">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="66dbf-414">**Variable d’environnement** : `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` est [facultatif et défini par l’utilisateur](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="66dbf-414">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="66dbf-415">L’environnement peut être défini à n’importe quelle valeur.</span><span class="sxs-lookup"><span data-stu-id="66dbf-415">The environment can be set to any value.</span></span> <span data-ttu-id="66dbf-416">Les valeurs définies par le framework sont `Development`, `Staging` et `Production`.</span><span class="sxs-lookup"><span data-stu-id="66dbf-416">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="66dbf-417">Les valeurs ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="66dbf-417">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="66dbf-418">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="66dbf-418">ConfigureHostConfiguration</span></span>

<span data-ttu-id="66dbf-419"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> utilise un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> pour créer un <xref:Microsoft.Extensions.Configuration.IConfiguration> pour l’hôte.</span><span class="sxs-lookup"><span data-stu-id="66dbf-419"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="66dbf-420">La configuration d’hôte permet d’initialiser le <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> en vue de son utilisation dans le processus de génération de l’application.</span><span class="sxs-lookup"><span data-stu-id="66dbf-420">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="66dbf-421"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="66dbf-421"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="66dbf-422">L’hôte utilise l’option qui définit une valeur en dernier sur une clé donnée.</span><span class="sxs-lookup"><span data-stu-id="66dbf-422">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="66dbf-423">Aucun fournisseur n’est inclus par défaut.</span><span class="sxs-lookup"><span data-stu-id="66dbf-423">No providers are included by default.</span></span> <span data-ttu-id="66dbf-424">Vous devez spécifier explicitement les fournisseurs de configuration dont l’application a besoin dans <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, notamment :</span><span class="sxs-lookup"><span data-stu-id="66dbf-424">You must explicitly specify whatever configuration providers the app requires in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, including:</span></span>

* <span data-ttu-id="66dbf-425">Configuration du fichier (par exemple, à partir d’un fichier *hostsettings.json*).</span><span class="sxs-lookup"><span data-stu-id="66dbf-425">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="66dbf-426">Configuration de la variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="66dbf-426">Environment variable configuration.</span></span>
* <span data-ttu-id="66dbf-427">Configuration de l’argument de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="66dbf-427">Command-line argument configuration.</span></span>
* <span data-ttu-id="66dbf-428">Tout autre fournisseur de configuration obligatoire.</span><span class="sxs-lookup"><span data-stu-id="66dbf-428">Any other required configuration providers.</span></span>

<span data-ttu-id="66dbf-429">Pour activer la configuration de fichier de l’hôte, spécifiez le chemin de base de l’application avec `SetBasePath`, suivi d’un appel à l’un des [fournisseurs de configuration de fichier](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="66dbf-429">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="66dbf-430">L’exemple d’application utilise un fichier JSON, *hostsettings.json*, et appelle <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> pour utiliser les paramètres de configuration d’hôte du fichier.</span><span class="sxs-lookup"><span data-stu-id="66dbf-430">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="66dbf-431">Pour ajouter la [configuration de variable d’environnement](xref:fundamentals/configuration/index#environment-variables-configuration-provider) de l’hôte, appelez <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> sur le générateur d’hôte.</span><span class="sxs-lookup"><span data-stu-id="66dbf-431">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="66dbf-432">`AddEnvironmentVariables` accepte un préfixe facultatif défini par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="66dbf-432">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="66dbf-433">L’exemple d’application utilise un préfixe `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="66dbf-433">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="66dbf-434">Ce préfixe est supprimé à la lecture des variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="66dbf-434">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="66dbf-435">Lorsque l’hôte de l’exemple d’application est configuré, la valeur de variable d’environnement de `PREFIX_ENVIRONMENT` devient la valeur de configuration d’hôte de la clé `environment`.</span><span class="sxs-lookup"><span data-stu-id="66dbf-435">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="66dbf-436">Pendant le développement, lorsque vous utilisez [Visual Studio](https://visualstudio.microsoft.com) ou que vous lancez une application avec `dotnet run`, vous pouvez définir les variables d’environnement dans le fichier *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="66dbf-436">During development when using [Visual Studio](https://visualstudio.microsoft.com) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="66dbf-437">Dans [Visual Studio Code](https://code.visualstudio.com/), elles peuvent être définies dans le fichier *.vscode/launch.json* au cours du développement.</span><span class="sxs-lookup"><span data-stu-id="66dbf-437">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="66dbf-438">Pour plus d'informations, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-438">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="66dbf-439">Pour ajouter la [configuration de ligne de commande](xref:fundamentals/configuration/index#command-line-configuration-provider), appelez <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-439">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="66dbf-440">La configuration de ligne de commande est ajoutée en dernier pour permettre aux arguments de ligne de commande de substituer la configuration fournie par les fournisseurs de configuration antérieurs.</span><span class="sxs-lookup"><span data-stu-id="66dbf-440">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="66dbf-441">*hostsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="66dbf-441">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="66dbf-442">Une configuration supplémentaire peut être fournie à l’aide des clés [applicationName](#application-key-name) et [contentRoot](#content-root).</span><span class="sxs-lookup"><span data-stu-id="66dbf-442">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="66dbf-443">Exemple de configuration `HostBuilder` avec <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> :</span><span class="sxs-lookup"><span data-stu-id="66dbf-443">Example `HostBuilder` configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="66dbf-444">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="66dbf-444">ConfigureAppConfiguration</span></span>

<span data-ttu-id="66dbf-445">Pour créer la configuration d’application, appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> sur l’implémentation <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-445">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="66dbf-446"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> utilise un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> pour créer un <xref:Microsoft.Extensions.Configuration.IConfiguration> pour l’application.</span><span class="sxs-lookup"><span data-stu-id="66dbf-446"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="66dbf-447"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="66dbf-447"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="66dbf-448">L’application utilise l’option qui définit une valeur en dernier sur une clé donnée.</span><span class="sxs-lookup"><span data-stu-id="66dbf-448">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="66dbf-449">La configuration créée par <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> est disponible dans [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) pour les opérations suivantes et dans <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-449">The configuration created by <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="66dbf-450">La configuration d’application reçoit automatiquement la configuration d’hôte fournie par [ConfigureHostConfiguration](#configurehostconfiguration).</span><span class="sxs-lookup"><span data-stu-id="66dbf-450">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="66dbf-451">Exemple de configuration d’application avec <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> :</span><span class="sxs-lookup"><span data-stu-id="66dbf-451">Example app configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="66dbf-452">*appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="66dbf-452">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="66dbf-453">*appsettings.Development.json* :</span><span class="sxs-lookup"><span data-stu-id="66dbf-453">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="66dbf-454">*appsettings.Production.json* :</span><span class="sxs-lookup"><span data-stu-id="66dbf-454">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="66dbf-455">Pour déplacer des fichiers de paramètres vers le répertoire de sortie, spécifiez-les en tant qu’[éléments de projet MSBuild](/visualstudio/msbuild/common-msbuild-project-items) dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="66dbf-455">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="66dbf-456">L’exemple d’application déplace ses fichiers de paramètres d’application JSON et *hostsettings.json* avec l’élément `<Content>` suivant :</span><span class="sxs-lookup"><span data-stu-id="66dbf-456">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> <span data-ttu-id="66dbf-457">Les méthodes d’extension de configuration, telles que <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> et <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*>, nécessitent des packages NuGet supplémentaires, tels que [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) et [ Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span><span class="sxs-lookup"><span data-stu-id="66dbf-457">Configuration extension methods, such as <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> and <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> require additional NuGet packages, such as [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) and [Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span></span> <span data-ttu-id="66dbf-458">Si l’application n’utilise pas le [métapaquet Microsoft.AspNetCore.App ](xref:fundamentals/metapackage-app), ces packages doivent être ajoutés au projet en plus du noyau [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration).</span><span class="sxs-lookup"><span data-stu-id="66dbf-458">Unless the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), these packages must be added to the project in addition to the core [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) package.</span></span> <span data-ttu-id="66dbf-459">Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-459">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="configureservices"></a><span data-ttu-id="66dbf-460">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="66dbf-460">ConfigureServices</span></span>

<span data-ttu-id="66dbf-461"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> ajoute des services au conteneur [d’injection de dépendances](xref:fundamentals/dependency-injection) de l’application.</span><span class="sxs-lookup"><span data-stu-id="66dbf-461"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="66dbf-462"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="66dbf-462"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="66dbf-463">Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-463">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="66dbf-464">Pour plus d'informations, consultez <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-464">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="66dbf-465">L’[exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) utilise la méthode d’extension `AddHostedService` afin d’ajouter un service pour les événements de durée de vie, `LifetimeEventsHostedService`, et une tâche en arrière-plan chronométrée, `TimedHostedService`, à l’application :</span><span class="sxs-lookup"><span data-stu-id="66dbf-465">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="66dbf-466">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="66dbf-466">ConfigureLogging</span></span>

<span data-ttu-id="66dbf-467"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> ajoute un délégué pour configurer le <xref:Microsoft.Extensions.Logging.ILoggingBuilder> fourni.</span><span class="sxs-lookup"><span data-stu-id="66dbf-467"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="66dbf-468"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="66dbf-468"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="66dbf-469">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="66dbf-469">UseConsoleLifetime</span></span>

<span data-ttu-id="66dbf-470"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> écoute Ctrl+C/SIGINT ou SIGTERM et appelle <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> pour démarrer le processus d’arrêt.</span><span class="sxs-lookup"><span data-stu-id="66dbf-470"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="66dbf-471"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> déverrouille les extensions telles que [RunAsync](#runasync) et [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="66dbf-471"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="66dbf-472"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> est préinscrit comme implémentation de durée de vie par défaut.</span><span class="sxs-lookup"><span data-stu-id="66dbf-472"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="66dbf-473">La dernière durée de vie inscrite est utilisée.</span><span class="sxs-lookup"><span data-stu-id="66dbf-473">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="66dbf-474">Configuration du conteneur</span><span class="sxs-lookup"><span data-stu-id="66dbf-474">Container configuration</span></span>

<span data-ttu-id="66dbf-475">Pour prendre en charge le branchement dans d’autres conteneurs, l’hôte peut accepter un <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-475">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span></span> <span data-ttu-id="66dbf-476">L’ajout d’une fabrique ne fait pas partie de l’inscription de conteneur DI mais est plutôt une tâche un intrinsèque à l’hôte utilisée pour créer le conteneur DI concret.</span><span class="sxs-lookup"><span data-stu-id="66dbf-476">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="66dbf-477">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) remplace la fabrique par défaut utilisée pour créer le fournisseur de services de l’application.</span><span class="sxs-lookup"><span data-stu-id="66dbf-477">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="66dbf-478">La configuration de conteneur personnalisée est gérée par la méthode <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-478">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="66dbf-479"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> offre une expérience fortement typée pour configurer le conteneur sur l’API hôte sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="66dbf-479"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="66dbf-480"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="66dbf-480"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="66dbf-481">Créer un conteneur de service pour l’application :</span><span class="sxs-lookup"><span data-stu-id="66dbf-481">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="66dbf-482">Fournir une fabrique de conteneur de service :</span><span class="sxs-lookup"><span data-stu-id="66dbf-482">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="66dbf-483">Utiliser la fabrique et configurer le conteneur de service personnalisé pour l’application :</span><span class="sxs-lookup"><span data-stu-id="66dbf-483">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="66dbf-484">Extensibilité</span><span class="sxs-lookup"><span data-stu-id="66dbf-484">Extensibility</span></span>

<span data-ttu-id="66dbf-485">L’extensibilité de l’hôte est effectuée avec les méthodes d’extension sur <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-485">Host extensibility is performed with extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span></span> <span data-ttu-id="66dbf-486">L’exemple suivant montre comment une méthode d’extension étend une implémentation <xref:Microsoft.Extensions.Hosting.IHostBuilder> avec l’exemple [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) démontré dans <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-486">The following example shows how an extension method extends an <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="66dbf-487">Une application établit la méthode d'extension `UseHostedService` pour inscrire le service hébergé passé dans `T` :</span><span class="sxs-lookup"><span data-stu-id="66dbf-487">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

```csharp
using System;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

public static class Extensions
{
    public static IHostBuilder UseHostedService<T>(this IHostBuilder hostBuilder)
        where T : class, IHostedService, IDisposable
    {
        return hostBuilder.ConfigureServices(services =>
            services.AddHostedService<T>());
    }
}
```

## <a name="manage-the-host"></a><span data-ttu-id="66dbf-488">Gérer l’hôte</span><span class="sxs-lookup"><span data-stu-id="66dbf-488">Manage the host</span></span>

<span data-ttu-id="66dbf-489">L’implémentation <xref:Microsoft.Extensions.Hosting.IHost> est chargée de démarrer et d’arrêter les implémentations <xref:Microsoft.Extensions.Hosting.IHostedService> qui sont inscrites dans le conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="66dbf-489">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="66dbf-490">Exécuter</span><span class="sxs-lookup"><span data-stu-id="66dbf-490">Run</span></span>

<span data-ttu-id="66dbf-491"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> exécute l’application et bloque le thread appelant jusqu’à l’arrêt de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="66dbf-491"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        host.Run();
    }
}
```

### <a name="runasync"></a><span data-ttu-id="66dbf-492">RunAsync</span><span class="sxs-lookup"><span data-stu-id="66dbf-492">RunAsync</span></span>

<span data-ttu-id="66dbf-493"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> exécute l’application et retourne une <xref:System.Threading.Tasks.Task> qui est effectuée quand le jeton d’annulation ou l’arrêt est déclenché :</span><span class="sxs-lookup"><span data-stu-id="66dbf-493"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        await host.RunAsync();
    }
}
```

### <a name="runconsoleasync"></a><span data-ttu-id="66dbf-494">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="66dbf-494">RunConsoleAsync</span></span>

<span data-ttu-id="66dbf-495"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> permet la prise en charge de la console, génère et démarre l’hôte et attend Ctrl+C/SIGINT ou SIGTERM pour l’arrêter.</span><span class="sxs-lookup"><span data-stu-id="66dbf-495"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var hostBuilder = new HostBuilder();

        await hostBuilder.RunConsoleAsync();
    }
}
```

### <a name="start-and-stopasync"></a><span data-ttu-id="66dbf-496">Start et StopAsync</span><span class="sxs-lookup"><span data-stu-id="66dbf-496">Start and StopAsync</span></span>

<span data-ttu-id="66dbf-497"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> démarre l’hôte en mode synchrone.</span><span class="sxs-lookup"><span data-stu-id="66dbf-497"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="66dbf-498"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> tente d’arrêter l’hôte dans le délai d’attente fourni.</span><span class="sxs-lookup"><span data-stu-id="66dbf-498"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            await host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

### <a name="startasync-and-stopasync"></a><span data-ttu-id="66dbf-499">StartAsync et StopAsync</span><span class="sxs-lookup"><span data-stu-id="66dbf-499">StartAsync and StopAsync</span></span>

<span data-ttu-id="66dbf-500"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> démarre l’application.</span><span class="sxs-lookup"><span data-stu-id="66dbf-500"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="66dbf-501"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> arrête l’application.</span><span class="sxs-lookup"><span data-stu-id="66dbf-501"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.StopAsync();
        }
    }
}
```

### <a name="waitforshutdown"></a><span data-ttu-id="66dbf-502">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="66dbf-502">WaitForShutdown</span></span>

<span data-ttu-id="66dbf-503"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> est déclenché par le biais de <xref:Microsoft.Extensions.Hosting.IHostLifetime>, par exemple <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (écoute Ctrl+C/SIGINT ou SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="66dbf-503"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (listens for Ctrl+C/SIGINT or SIGTERM).</span></span> <span data-ttu-id="66dbf-504"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> appelle <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-504"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            host.WaitForShutdown();
        }
    }
}
```

### <a name="waitforshutdownasync"></a><span data-ttu-id="66dbf-505">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="66dbf-505">WaitForShutdownAsync</span></span>

<span data-ttu-id="66dbf-506"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> retourne une <xref:System.Threading.Tasks.Task> qui est effectuée quand l’arrêt est déclenché par le biais du jeton fourni et appelle <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-506"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.WaitForShutdownAsync();
        }

    }
}
```

### <a name="external-control"></a><span data-ttu-id="66dbf-507">Contrôle externe</span><span class="sxs-lookup"><span data-stu-id="66dbf-507">External control</span></span>

<span data-ttu-id="66dbf-508">Le contrôle externe de l’hôte peut être effectué à l’aide de méthodes pouvant être appelées de façon externe :</span><span class="sxs-lookup"><span data-stu-id="66dbf-508">External control of the host can be achieved using methods that can be called externally:</span></span>

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

<span data-ttu-id="66dbf-509"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> est appelé au début de <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, lequel attend qu’il soit fini avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="66dbf-509"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="66dbf-510">Cela permet de retarder le démarrage jusqu'à ce que celui-ci soit signalé par un événement externe.</span><span class="sxs-lookup"><span data-stu-id="66dbf-510">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="66dbf-511">Interface IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="66dbf-511">IHostingEnvironment interface</span></span>

<span data-ttu-id="66dbf-512"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> fournit des informations sur l’environnement d’hébergement de l’application.</span><span class="sxs-lookup"><span data-stu-id="66dbf-512"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="66dbf-513">Utilisez [l’injection de constructeur](xref:fundamentals/dependency-injection) pour obtenir l’interface <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> afin d’utiliser ses propriétés et méthodes d’extension :</span><span class="sxs-lookup"><span data-stu-id="66dbf-513">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> in order to use its properties and extension methods:</span></span>

```csharp
public class MyClass
{
    private readonly IHostingEnvironment _env;

    public MyClass(IHostingEnvironment env)
    {
        _env = env;
    }

    public void DoSomething()
    {
        var environmentName = _env.EnvironmentName;
    }
}
```

<span data-ttu-id="66dbf-514">Pour plus d'informations, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="66dbf-514">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="66dbf-515">Interface IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="66dbf-515">IApplicationLifetime interface</span></span>

<span data-ttu-id="66dbf-516"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> s’utilise pour les activités de post-démarrage et d’arrêt, notamment pour les demandes d’arrêt normal.</span><span class="sxs-lookup"><span data-stu-id="66dbf-516"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="66dbf-517">Trois propriétés de l’interface sont des jetons d’annulation utilisés pour inscrire les méthodes <xref:System.Action> qui définissent les événements de démarrage et d’arrêt.</span><span class="sxs-lookup"><span data-stu-id="66dbf-517">Three properties on the interface are cancellation tokens used to register <xref:System.Action> methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="66dbf-518">Jeton d’annulation</span><span class="sxs-lookup"><span data-stu-id="66dbf-518">Cancellation Token</span></span> | <span data-ttu-id="66dbf-519">Événement déclencheur</span><span class="sxs-lookup"><span data-stu-id="66dbf-519">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="66dbf-520">L’hôte a complètement démarré.</span><span class="sxs-lookup"><span data-stu-id="66dbf-520">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="66dbf-521">L’hôte effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="66dbf-521">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="66dbf-522">Toutes les requêtes doivent être traitées.</span><span class="sxs-lookup"><span data-stu-id="66dbf-522">All requests should be processed.</span></span> <span data-ttu-id="66dbf-523">L’arrêt est bloqué tant que cet événement n’est pas terminé.</span><span class="sxs-lookup"><span data-stu-id="66dbf-523">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="66dbf-524">L’hôte effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="66dbf-524">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="66dbf-525">Des requêtes peuvent encore être en cours de traitement.</span><span class="sxs-lookup"><span data-stu-id="66dbf-525">Requests may still be processing.</span></span> <span data-ttu-id="66dbf-526">L’arrêt est bloqué tant que cet événement n’est pas terminé.</span><span class="sxs-lookup"><span data-stu-id="66dbf-526">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="66dbf-527">Le constructeur injecte le service <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> dans une classe.</span><span class="sxs-lookup"><span data-stu-id="66dbf-527">Constructor-inject the <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> service into any class.</span></span> <span data-ttu-id="66dbf-528">[L’exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) utilise l’injection de constructeur dans une classe `LifetimeEventsHostedService` (une implémentation <xref:Microsoft.Extensions.Hosting.IHostedService>) pour inscrire les événements.</span><span class="sxs-lookup"><span data-stu-id="66dbf-528">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an <xref:Microsoft.Extensions.Hosting.IHostedService> implementation) to register the events.</span></span>

<span data-ttu-id="66dbf-529">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="66dbf-529">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="66dbf-530"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> demande l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="66dbf-530"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="66dbf-531">La classe suivante utilise <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> pour arrêter normalement une application quand la méthode `Shutdown` de la classe est appelée :</span><span class="sxs-lookup"><span data-stu-id="66dbf-531">The following class uses <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="66dbf-532">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="66dbf-532">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
