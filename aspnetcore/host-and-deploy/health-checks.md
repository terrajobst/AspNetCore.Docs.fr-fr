---
title: Contrôles d’intégrité dans ASP.NET Core
author: guardrex
description: Découvrez comment configurer des contrôles d’intégrité pour l’infrastructure ASP.NET Core, comme des applications ou des bases de données.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 12/03/2018
uid: host-and-deploy/health-checks
ms.openlocfilehash: d8fd43d9d689396cf30ca371763cdf7ac9423c77
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862594"
---
# <a name="health-checks-in-aspnet-core"></a><span data-ttu-id="32a0f-103">Contrôles d’intégrité dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="32a0f-103">Health checks in ASP.NET Core</span></span>

<span data-ttu-id="32a0f-104">Par [Luke Latham](https://github.com/guardrex) et [Glenn Condron](https://github.com/glennc)</span><span class="sxs-lookup"><span data-stu-id="32a0f-104">By [Luke Latham](https://github.com/guardrex) and [Glenn Condron](https://github.com/glennc)</span></span>

<span data-ttu-id="32a0f-105">ASP.NET Core met à disposition Health Check Middleware ainsi que des bibliothèques afin de créer des rapports sur l’intégrité des composants de l’infrastructure d’application.</span><span class="sxs-lookup"><span data-stu-id="32a0f-105">ASP.NET Core offers Health Check Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="32a0f-106">Les contrôles d’intégrité sont exposés par une application comme des points de terminaison HTTP.</span><span class="sxs-lookup"><span data-stu-id="32a0f-106">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="32a0f-107">Les points de terminaison de vérification d’intégrité peuvent être configurés pour de nombreux scénarios de supervision en temps réel :</span><span class="sxs-lookup"><span data-stu-id="32a0f-107">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="32a0f-108">Les sondes d’intégrité peuvent être utilisées par les orchestrateurs de conteneurs et les équilibreurs de charge afin de vérifier l’état d’une application.</span><span class="sxs-lookup"><span data-stu-id="32a0f-108">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="32a0f-109">Par exemple, un orchestrateur de conteneurs peut répondre à un résultat de non intégrité en arrêtant le déploiement ou en redémarrant un conteneur.</span><span class="sxs-lookup"><span data-stu-id="32a0f-109">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="32a0f-110">Face à une application non saine, l’équilibreur de charge peut réagir en redirigeant le trafic vers une instance saine.</span><span class="sxs-lookup"><span data-stu-id="32a0f-110">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="32a0f-111">L’utilisation de la mémoire, des disques et des autres ressources de serveur physique peut être supervisée dans le cadre d’un contrôle d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="32a0f-111">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="32a0f-112">Les contrôles d’intégrité peuvent tester les dépendances d’une application, telles que les bases de données et les points de terminaison de service externes, dans le but de vérifier leur disponibilité et leur bon fonctionnement.</span><span class="sxs-lookup"><span data-stu-id="32a0f-112">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="32a0f-113">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="32a0f-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="32a0f-114">L’exemple d’application comprend des exemples pour les scénarios décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="32a0f-114">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="32a0f-115">Pour exécuter l’exemple d’application selon un scénario donné, utilisez la commande [dotnet run](/dotnet/core/tools/dotnet-run) dans un interpréteur de commandes, à partir du dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="32a0f-115">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="32a0f-116">Pour plus d’informations sur l’utilisation de l’exemple d’application, consultez le fichier *README.md* de l’exemple d’application ainsi que les descriptions de scénarios de cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="32a0f-116">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="32a0f-117">Prérequis</span><span class="sxs-lookup"><span data-stu-id="32a0f-117">Prerequisites</span></span>

<span data-ttu-id="32a0f-118">Les contrôles d’intégrité sont généralement utilisés avec un service de supervision externe ou un orchestrateur de conteneurs pour vérifier l’état d’une application.</span><span class="sxs-lookup"><span data-stu-id="32a0f-118">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="32a0f-119">Avant d’ajouter des contrôles d’intégrité à une application, vous devez décider du système de supervision à utiliser.</span><span class="sxs-lookup"><span data-stu-id="32a0f-119">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="32a0f-120">Le système de supervision détermine les types de contrôles d’intégrité qui doivent être créés ainsi que la configuration de leurs points de terminaison.</span><span class="sxs-lookup"><span data-stu-id="32a0f-120">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="32a0f-121">Référencez le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ou ajoutez une référence de package au package [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks).</span><span class="sxs-lookup"><span data-stu-id="32a0f-121">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span>

<span data-ttu-id="32a0f-122">L’exemple d’application fournit du code de démarrage pour illustrer les contrôles d’intégrité de plusieurs scénarios.</span><span class="sxs-lookup"><span data-stu-id="32a0f-122">The sample app provides start-up code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="32a0f-123">Le scénario [Sondage de base de données](#database-probe) sonde l’intégrité d’une connexion de base de données à l’aide de [BeatPulse](https://github.com/Xabaril/BeatPulse).</span><span class="sxs-lookup"><span data-stu-id="32a0f-123">The [database probe](#database-probe) scenario probes the health of a database connection using [BeatPulse](https://github.com/Xabaril/BeatPulse).</span></span> <span data-ttu-id="32a0f-124">Le scénario [Sondage DbContext](#entity-framework-core-dbcontext-probe) sonde une base de données à l’aide d’un `DbContext` EF Core.</span><span class="sxs-lookup"><span data-stu-id="32a0f-124">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario probes a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="32a0f-125">Pour explorer les scénarios de base de données à l’aide de l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="32a0f-125">To explore the database scenarios using the sample app:</span></span>

* <span data-ttu-id="32a0f-126">Créez une base de données et fournissez sa chaîne de connexion dans le fichier *appsettings.json* de l’application.</span><span class="sxs-lookup"><span data-stu-id="32a0f-126">Create a database and provide its connection string in the *appsettings.json* file of the app.</span></span>
* <span data-ttu-id="32a0f-127">Ajoutez une référence de package à [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="32a0f-127">Add a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

> [!NOTE]
> <span data-ttu-id="32a0f-128">[BeatPulse](https://github.com/Xabaril/BeatPulse) n’est pas géré ni pris en charge par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="32a0f-128">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="32a0f-129">Un autre scénario de contrôle d’intégrité montre comment filtrer des contrôles d’intégrité selon un port de gestion.</span><span class="sxs-lookup"><span data-stu-id="32a0f-129">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="32a0f-130">L’exemple d’application vous oblige à créer un fichier *Properties/launchSettings.json* comprenant l’URL de gestion et le port de gestion.</span><span class="sxs-lookup"><span data-stu-id="32a0f-130">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="32a0f-131">Pour plus d’informations, consultez la section [Filtrer par port](#filter-by-port).</span><span class="sxs-lookup"><span data-stu-id="32a0f-131">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="32a0f-132">Sondage d’intégrité de base</span><span class="sxs-lookup"><span data-stu-id="32a0f-132">Basic health probe</span></span>

<span data-ttu-id="32a0f-133">Pour de nombreuses applications, un sondage d’intégrité de base qui signale la disponibilité d’une application pour le traitement des requêtes (*liveness*) suffit à découvrir l’état de l’application.</span><span class="sxs-lookup"><span data-stu-id="32a0f-133">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="32a0f-134">La configuration de base inscrit les services de contrôle d’intégrité, puis appelle Health Check Middleware pour répondre à un point de terminaison d’URL avec une réponse d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="32a0f-134">The basic configuration registers health check services and calls the Health Check Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="32a0f-135">Par défaut, aucun contrôle d’intégrité n’est inscrit pour tester les dépendances ou le sous-système.</span><span class="sxs-lookup"><span data-stu-id="32a0f-135">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="32a0f-136">L’application est considérée comme saine si elle est capable de répondre à l’URL de point de terminaison de contrôle d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="32a0f-136">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="32a0f-137">L’enregistreur de réponse par défaut écrit l’état (`HealthCheckStatus`) sous forme de texte en clair qu’il renvoie au client, indiquant si l’état est `HealthCheckResult.Healthy` ou `HealthCheckResult.Unhealthy`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-137">The default response writer writes the status (`HealthCheckStatus`) as a plaintext response back to the client, indicating either a `HealthCheckResult.Healthy` or `HealthCheckResult.Unhealthy` status.</span></span>

<span data-ttu-id="32a0f-138">Pour inscrire les services de contrôle d’intégrité, utilisez `AddHealthChecks` dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-138">Register health check services with `AddHealthChecks` in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="32a0f-139">Ajoutez Health Check Middleware avec `UseHealthChecks` dans le pipeline de traitement des requêtes de `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-139">Add Health Check Middleware with `UseHealthChecks` in the request processing pipeline of `Startup.Configure`.</span></span>

<span data-ttu-id="32a0f-140">Dans l’exemple d’application, le point de terminaison de contrôle d’intégrité est créé au niveau de `/health` (*BasicStartup.cs*) :</span><span class="sxs-lookup"><span data-stu-id="32a0f-140">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/BasicStartup.cs?name=snippet1&highlight=5,10)]

<span data-ttu-id="32a0f-141">Pour exécuter le scénario de configuration de base à l’aide de l’exemple d’application, exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="32a0f-141">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="32a0f-142">Exemple Docker</span><span class="sxs-lookup"><span data-stu-id="32a0f-142">Docker example</span></span>

<span data-ttu-id="32a0f-143">[Docker](xref:host-and-deploy/docker/index) fournit une directive `HEALTHCHECK` intégrée qui peut être utilisée pour vérifier l’état d’une application utilisant la configuration de contrôle d’intégrité de base :</span><span class="sxs-lookup"><span data-stu-id="32a0f-143">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="32a0f-144">Créer des contrôles d’intégrité</span><span class="sxs-lookup"><span data-stu-id="32a0f-144">Create health checks</span></span>

<span data-ttu-id="32a0f-145">Les contrôles d’intégrité sont créés via l’implémentation de l’interface `IHealthCheck`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-145">Health checks are created by implementing the `IHealthCheck` interface.</span></span> <span data-ttu-id="32a0f-146">La méthode `IHealthCheck.CheckHealthAsync` retourne un `Task<HealthCheckResult>` qui indique l’état d’intégrité comme étant `Healthy`, `Degraded` ou `Unhealthy`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-146">The `IHealthCheck.CheckHealthAsync` method returns a `Task<HealthCheckResult>` that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="32a0f-147">Le résultat est écrit sous la forme de texte en clair avec un code d’état configurable (la configuration est décrite dans la section [Options de contrôle d’intégrité](#health-check-options)).</span><span class="sxs-lookup"><span data-stu-id="32a0f-147">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="32a0f-148">`HealthCheckResult` peut également retourner des paires clé-valeur facultatives.</span><span class="sxs-lookup"><span data-stu-id="32a0f-148">`HealthCheckResult` can also return optional key-value pairs.</span></span>

### <a name="example-health-check"></a><span data-ttu-id="32a0f-149">Exemple de contrôle d’intégrité</span><span class="sxs-lookup"><span data-stu-id="32a0f-149">Example health check</span></span>

<span data-ttu-id="32a0f-150">La classe `ExampleHealthCheck` suivante montre la disposition d’un contrôle d’intégrité :</span><span class="sxs-lookup"><span data-stu-id="32a0f-150">The following `ExampleHealthCheck` class demonstrates the layout of a health check:</span></span>

```csharp
public class ExampleHealthCheck : IHealthCheck
{
    public ExampleHealthCheck()
    {
        // Use dependency injection (DI) to supply any required services to the
        // health check.
    }

    public Task<HealthCheckResult> CheckHealthAsync(
        HealthCheckContext context, 
        CancellationToken cancellationToken = default(CancellationToken))
    {
        // Execute health check logic here. This example sets a dummy
        // variable to true.
        var healthCheckResultHealthy = true;

        if (healthCheckResultHealthy)
        {
            return Task.FromResult(
                HealthCheckResult.Healthy("The check indicates a healthy result."));
        }

        return Task.FromResult(
            HealthCheckResult.Unhealthy("The check indicates an unhealthy result."));
    }
}
```

### <a name="register-health-check-services"></a><span data-ttu-id="32a0f-151">Inscrire les services de contrôle d’intégrité</span><span class="sxs-lookup"><span data-stu-id="32a0f-151">Register health check services</span></span>

<span data-ttu-id="32a0f-152">Le type `ExampleHealthCheck` est ajouté aux services de contrôle d’intégrité avec `AddCheck` :</span><span class="sxs-lookup"><span data-stu-id="32a0f-152">The `ExampleHealthCheck` type is added to health check services with `AddCheck`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck<ExampleHealthCheck>("example_health_check");
}
```

<span data-ttu-id="32a0f-153">La surcharge `AddCheck` de l’exemple suivant définit l’état d’échec (`HealthStatus`) de manière à être signalé lorsque le contrôle d’intégrité signale un échec.</span><span class="sxs-lookup"><span data-stu-id="32a0f-153">The `AddCheck` overload shown in the following example sets the failure status (`HealthStatus`) to report when the health check reports a failure.</span></span> <span data-ttu-id="32a0f-154">Si l’état d’échec est défini sur `null` (par défaut), `HealthStatus.Unhealthy` est signalé.</span><span class="sxs-lookup"><span data-stu-id="32a0f-154">If the failure status is set to `null` (default), `HealthStatus.Unhealthy` is reported.</span></span> <span data-ttu-id="32a0f-155">Cette surcharge est utile pour les créateurs de bibliothèque, dans les cas où l’état d’échec indiqué par la bibliothèque est appliqué par l’application lorsqu’un échec est signalé par le contrôle d’intégrité, si l’implémentation de ce dernier respecte le paramètre.</span><span class="sxs-lookup"><span data-stu-id="32a0f-155">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="32a0f-156">Des *étiquettes* peuvent être utilisées pour filtrer les contrôles d’intégrité (cette procédure est décrite plus loin dans la section [Filtrer les contrôles d’intégrité](#filter-health-checks)).</span><span class="sxs-lookup"><span data-stu-id="32a0f-156">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check", 
        failureStatus: HealthStatus.Degraded, 
        tags: new[] { "example" });
```

<span data-ttu-id="32a0f-157">`AddCheck` peut également exécuter une fonction lambda.</span><span class="sxs-lookup"><span data-stu-id="32a0f-157">`AddCheck` can also execute a lambda function.</span></span> <span data-ttu-id="32a0f-158">Dans l’exemple suivant, le nom du contrôle d’intégrité est spécifié en tant que `Example`, et la vérification retourne toujours un état sain :</span><span class="sxs-lookup"><span data-stu-id="32a0f-158">In the following example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck("Example", () => 
            HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" })
}
```

### <a name="use-health-checks-middleware"></a><span data-ttu-id="32a0f-159">Utiliser Health Checks Middleware</span><span class="sxs-lookup"><span data-stu-id="32a0f-159">Use Health Checks Middleware</span></span>

<span data-ttu-id="32a0f-160">Dans `Startup.Configure`, appelez `UseHealthChecks` dans le pipeline de traitement avec l’URL de point de terminaison ou le chemin relatif :</span><span class="sxs-lookup"><span data-stu-id="32a0f-160">In `Startup.Configure`, call `UseHealthChecks` in the processing pipeline with the endpoint URL or relative path:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health");
}
```

<span data-ttu-id="32a0f-161">Si les contrôles d’intégrité doivent écouter un port en particulier, utilisez une surcharge de `UseHealthChecks` pour définir le port (cette procédure est décrite plus loin dans la section [Filtrer par port](#filter-by-port)) :</span><span class="sxs-lookup"><span data-stu-id="32a0f-161">If the health checks should listen on a specific port, use an overload of `UseHealthChecks` to set the port (described further in the [Filter by port](#filter-by-port) section):</span></span>

```csharp
app.UseHealthChecks("/health", port: 8000);
```

<span data-ttu-id="32a0f-162">Health Checks Middleware est un *intergiciel (middleware) terminal* présent dans le pipeline de traitement des requêtes de l’application.</span><span class="sxs-lookup"><span data-stu-id="32a0f-162">Health Checks Middleware is a *terminal middleware* in the app's request processing pipeline.</span></span> <span data-ttu-id="32a0f-163">Le premier point de terminaison de contrôle d’intégrité a trouvé une correspondance exacte à l’URL de requête et court-circuite le reste du pipeline du middleware.</span><span class="sxs-lookup"><span data-stu-id="32a0f-163">The first health check endpoint encountered that's an exact match to the request URL executes and short-circuits the rest of the middleware pipeline.</span></span> <span data-ttu-id="32a0f-164">Lorsqu’un court-circuit se produit, aucun autre middleware n’est exécuté après le contrôle d’intégrité mis en correspondance.</span><span class="sxs-lookup"><span data-stu-id="32a0f-164">When short-circuiting occurs, no middleware following the matched health check executes.</span></span>

## <a name="health-check-options"></a><span data-ttu-id="32a0f-165">Options de contrôle d’intégrité</span><span class="sxs-lookup"><span data-stu-id="32a0f-165">Health check options</span></span>

<span data-ttu-id="32a0f-166">`HealthCheckOptions` permet de personnaliser le comportement du contrôle d’intégrité :</span><span class="sxs-lookup"><span data-stu-id="32a0f-166">`HealthCheckOptions` provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="32a0f-167">Filtrer les contrôles d’intégrité</span><span class="sxs-lookup"><span data-stu-id="32a0f-167">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="32a0f-168">Personnaliser le code d’état HTTP</span><span class="sxs-lookup"><span data-stu-id="32a0f-168">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="32a0f-169">Supprimer les en-têtes de cache</span><span class="sxs-lookup"><span data-stu-id="32a0f-169">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="32a0f-170">Personnaliser la sortie</span><span class="sxs-lookup"><span data-stu-id="32a0f-170">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="32a0f-171">Filtrer les contrôles d’intégrité</span><span class="sxs-lookup"><span data-stu-id="32a0f-171">Filter health checks</span></span>

<span data-ttu-id="32a0f-172">Par défaut, Health Check Middleware exécute tous les contrôles d’intégrité inscrits.</span><span class="sxs-lookup"><span data-stu-id="32a0f-172">By default, Health Check Middleware runs all registered health checks.</span></span> <span data-ttu-id="32a0f-173">Pour exécuter un sous-ensemble de contrôles d’intégrité, fournissez une fonction qui retourne une valeur booléenne à l’option `Predicate`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-173">To run a subset of health checks, provide a function that returns a boolean to the `Predicate` option.</span></span> <span data-ttu-id="32a0f-174">Dans l’exemple suivant, le contrôle d’intégrité `Bar` est filtré en fonction de son étiquette (`bar_tag`) dans l’instruction conditionnelle de la fonction, où `true` est retourné uniquement si la propriété `Tag` du contrôle d’intégrité correspond à `foo_tag` ou à `baz_tag` :</span><span class="sxs-lookup"><span data-stu-id="32a0f-174">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's `Tag` property matches `foo_tag` or `baz_tag`:</span></span>

```csharp
using System.Threading.Tasks;
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck("Foo", () => 
            HealthCheckResult.Healthy("Foo is OK!"), tags: new[] { "foo_tag" })
        .AddCheck("Bar", () => 
            HealthCheckResult.Unhealthy("Bar is unhealthy!"), tags: new[] { "bar_tag" })
        .AddCheck("Baz", () => 
            HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // Filter out the 'Bar' health check. Only Foo and Baz execute.
        Predicate = (check) => check.Tags.Contains("foo_tag") || 
            check.Tags.Contains("baz_tag")
    });
}
```

### <a name="customize-the-http-status-code"></a><span data-ttu-id="32a0f-175">Personnaliser le code d’état HTTP</span><span class="sxs-lookup"><span data-stu-id="32a0f-175">Customize the HTTP status code</span></span>

<span data-ttu-id="32a0f-176">Utilisez `ResultStatusCodes` pour personnaliser le mappage de l’état d’intégrité aux codes d’état HTTP.</span><span class="sxs-lookup"><span data-stu-id="32a0f-176">Use `ResultStatusCodes` to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="32a0f-177">Les affectations `StatusCode` suivantes sont les valeurs par défaut utilisées par le middleware.</span><span class="sxs-lookup"><span data-stu-id="32a0f-177">The following `StatusCode` assignments are the default values used by the middleware.</span></span> <span data-ttu-id="32a0f-178">Vous pouvez modifier les valeurs de code d’état selon vos besoins.</span><span class="sxs-lookup"><span data-stu-id="32a0f-178">Change the status code values to meet your requirements.</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // The following StatusCodes are the default assignments for
        // the HealthCheckStatus properties.
        ResultStatusCodes =
        {
            [HealthCheckStatus.Healthy] = StatusCodes.Status200OK,
            [HealthCheckStatus.Degraded] = StatusCodes.Status200OK,
            [HealthCheckStatus.Unhealthy] = StatusCodes.Status503ServiceUnavailable
        }
    });
}
```

### <a name="suppress-cache-headers"></a><span data-ttu-id="32a0f-179">Supprimer les en-têtes de cache</span><span class="sxs-lookup"><span data-stu-id="32a0f-179">Suppress cache headers</span></span>

<span data-ttu-id="32a0f-180">`AllowCachingResponses` contrôle si Health Check Middleware ajoute des en-têtes HTTP à une réponse de sonde pour empêcher la mise en cache de la réponse.</span><span class="sxs-lookup"><span data-stu-id="32a0f-180">`AllowCachingResponses` controls whether the Health Check Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="32a0f-181">Si la valeur est `false` (par défaut), le middleware définit ou substitue les en-têtes `Cache-Control`, `Expires` et `Pragma` afin d’empêcher la mise en cache de la réponse.</span><span class="sxs-lookup"><span data-stu-id="32a0f-181">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="32a0f-182">Si la valeur est `true`, le middleware ne modifie pas les en-têtes de cache de la réponse.</span><span class="sxs-lookup"><span data-stu-id="32a0f-182">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // The default value is false.
        AllowCachingResponses = false
    });
}
```

### <a name="customize-output"></a><span data-ttu-id="32a0f-183">Personnaliser la sortie</span><span class="sxs-lookup"><span data-stu-id="32a0f-183">Customize output</span></span>

<span data-ttu-id="32a0f-184">L’option `ResponseWriter` obtient ou définit un délégué utilisé pour écrire la réponse.</span><span class="sxs-lookup"><span data-stu-id="32a0f-184">The `ResponseWriter` option gets or sets a delegate used to write the response.</span></span> <span data-ttu-id="32a0f-185">Le délégué par défaut écrit une réponse minimale constituée de texte en clair, avec la valeur de chaîne `HealthReport.Status`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-185">The default delegate writes a minimal plaintext response with the string value of `HealthReport.Status`.</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // WriteResponse is a delegate used to write the response.
        ResponseWriter = WriteResponse
    });
}

private static Task WriteResponse(HttpContext httpContext, 
    HealthReport result)
{
    httpContext.Response.ContentType = "application/json";

    var json = new JObject(
        new JProperty("status", result.Status.ToString()),
        new JProperty("results", new JObject(result.Entries.Select(pair =>
            new JProperty(pair.Key, new JObject(
                new JProperty("status", pair.Value.Status.ToString()),
                new JProperty("description", pair.Value.Description),
                new JProperty("data", new JObject(pair.Value.Data.Select(
                    p => new JProperty(p.Key, p.Value))))))))));
    return httpContext.Response.WriteAsync(
        json.ToString(Formatting.Indented));
}
```

## <a name="database-probe"></a><span data-ttu-id="32a0f-186">Sondage de base de données</span><span class="sxs-lookup"><span data-stu-id="32a0f-186">Database probe</span></span>

<span data-ttu-id="32a0f-187">Un contrôle d’intégrité peut spécifier une requête de base de données à exécuter en tant que test booléen pour indiquer si la base de données répond normalement.</span><span class="sxs-lookup"><span data-stu-id="32a0f-187">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="32a0f-188">Pour exécuter un contrôle d’intégrité sur une base de données SQL Server, l’exemple d’application utilise [BeatPulse](https://github.com/Xabaril/BeatPulse), qui est une bibliothèque de contrôle d’intégrité pour les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="32a0f-188">The sample app uses [BeatPulse](https://github.com/Xabaril/BeatPulse), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="32a0f-189">BeatPulse exécute une requête `SELECT 1` sur la base de données pour vérifier que la connexion à la base de données est saine.</span><span class="sxs-lookup"><span data-stu-id="32a0f-189">BeatPulse executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="32a0f-190">Lorsque vous vérifiez une connexion de base de données à l’aide d’une requête, choisissez une requête qui est retournée rapidement.</span><span class="sxs-lookup"><span data-stu-id="32a0f-190">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="32a0f-191">L’utilisation d’une requête risque néanmoins d’entraîner une surcharge de la base de données et d’en diminuer les performances.</span><span class="sxs-lookup"><span data-stu-id="32a0f-191">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="32a0f-192">Dans la plupart des cas, il n’est pas nécessaire d’utiliser une requête de test.</span><span class="sxs-lookup"><span data-stu-id="32a0f-192">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="32a0f-193">Il suffit simplement d’établir une connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="32a0f-193">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="32a0f-194">Si vous avez besoin d’exécuter une requête, choisissez une requête SELECT simple, telle que `SELECT 1`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-194">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="32a0f-195">Pour utiliser la bibliothèque BeatPulse, ajoutez une référence de package à [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="32a0f-195">In order to use the BeatPulse library, include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="32a0f-196">Fournissez une chaîne de connexion de base de données valide dans le fichier *appsettings.json* de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="32a0f-196">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="32a0f-197">L’application utilise une base de données SQL Server nommée `HealthCheckSample` :</span><span class="sxs-lookup"><span data-stu-id="32a0f-197">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="32a0f-198">Pour inscrire les services de contrôle d’intégrité, utilisez `AddHealthChecks` dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-198">Register health check services with `AddHealthChecks` in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="32a0f-199">L’exemple d’application appelle la méthode `AddSqlServer` de BeatPulse avec la chaîne de connexion de la base de données (*DbHealthStartup.cs*) :</span><span class="sxs-lookup"><span data-stu-id="32a0f-199">The sample app calls BeatPulse's `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="32a0f-200">Appelez Health Check Middleware dans le pipeline de traitement d’application de `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="32a0f-200">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_Configure)]

<span data-ttu-id="32a0f-201">Pour exécuter le scénario de sondage d’une base de données à l’aide de l’exemple d’application, exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="32a0f-201">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="32a0f-202">[BeatPulse](https://github.com/Xabaril/BeatPulse) n’est pas géré ni pris en charge par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="32a0f-202">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="32a0f-203">Sondage DbContext Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="32a0f-203">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="32a0f-204">La vérification `DbContext` est prise en charge dans les applications qui utilisent [Entity Framework (EF) Core](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="32a0f-204">The `DbContext` check is supported in apps that use [Entity Framework (EF) Core](/ef/core/).</span></span> <span data-ttu-id="32a0f-205">Elle vérifie que l’application peut communiquer avec la base de données configurée pour un `DbContext` EF Core.</span><span class="sxs-lookup"><span data-stu-id="32a0f-205">This check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="32a0f-206">Par défaut, `DbContextHealthCheck` appelle la méthode `CanConnectAsync` d’EF Core.</span><span class="sxs-lookup"><span data-stu-id="32a0f-206">By default, the `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="32a0f-207">Vous pouvez choisir quelle opération doit être exécutée lors du contrôle d’intégrité à l’aide des surcharges de la méthode `AddDbContextCheck`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-207">You can customize what operation is run when checking health using overloads of the `AddDbContextCheck` method.</span></span>

<span data-ttu-id="32a0f-208">`AddDbContextCheck<TContext>` inscrit un contrôle d’intégrité pour un `DbContext` (`TContext`).</span><span class="sxs-lookup"><span data-stu-id="32a0f-208">`AddDbContextCheck<TContext>` registers a health check for a `DbContext` (`TContext`).</span></span> <span data-ttu-id="32a0f-209">Par défaut, le nom du contrôle d’intégrité correspond à celui du type `TContext`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-209">By default, the name of the health check is the name of the `TContext` type.</span></span> <span data-ttu-id="32a0f-210">Une surcharge est disponible pour configurer l’état d’échec, les étiquettes et une requête de test personnalisée.</span><span class="sxs-lookup"><span data-stu-id="32a0f-210">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="32a0f-211">Dans l’exemple d’application, `AppDbContext` est fourni à `AddDbContextCheck`, puis est inscrit en tant que service dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-211">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="32a0f-212">*DbContextHealthStartup.cs* :</span><span class="sxs-lookup"><span data-stu-id="32a0f-212">*DbContextHealthStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="32a0f-213">Dans l’exemple d’application, `UseHealthChecks` ajoute Health Check Middleware dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-213">In the sample app, `UseHealthChecks` adds the Health Check Middleware in `Startup.Configure`.</span></span>

<span data-ttu-id="32a0f-214">*DbContextHealthStartup.cs* :</span><span class="sxs-lookup"><span data-stu-id="32a0f-214">*DbContextHealthStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_Configure)]

<span data-ttu-id="32a0f-215">Pour exécuter le scénario de sondage `DbContext` à l’aide de l’exemple d’application, vérifiez que la base de données spécifiée par le la chaîne de connexion ne se trouve pas déjà dans l’instance SQL Server.</span><span class="sxs-lookup"><span data-stu-id="32a0f-215">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="32a0f-216">Si la base de données existe, supprimez-la.</span><span class="sxs-lookup"><span data-stu-id="32a0f-216">If the database exists, delete it.</span></span>

<span data-ttu-id="32a0f-217">Exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="32a0f-217">Execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario dbcontext
```

<span data-ttu-id="32a0f-218">Une fois que l’application est en cours d’exécution, vérifiez l’état d’intégrité en envoyant une requête au point de terminaison `/health` dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="32a0f-218">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="32a0f-219">La base de données et `AppDbContext` n’existent pas, donc l’application fournit la réponse suivante :</span><span class="sxs-lookup"><span data-stu-id="32a0f-219">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="32a0f-220">Déclenchez l’exemple d’application pour créer la base de données.</span><span class="sxs-lookup"><span data-stu-id="32a0f-220">Trigger the sample app to create the database.</span></span> <span data-ttu-id="32a0f-221">Envoyez une requête à `/createdatabase`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-221">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="32a0f-222">L’application répond :</span><span class="sxs-lookup"><span data-stu-id="32a0f-222">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="32a0f-223">Envoyez une requête au point de terminaison `/health`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-223">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="32a0f-224">La base de données et le contexte existent, donc l’application répond :</span><span class="sxs-lookup"><span data-stu-id="32a0f-224">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="32a0f-225">Déclenchez l’exemple d’application pour supprimer la base de données.</span><span class="sxs-lookup"><span data-stu-id="32a0f-225">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="32a0f-226">Envoyez une requête à `/deletedatabase`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-226">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="32a0f-227">L’application répond :</span><span class="sxs-lookup"><span data-stu-id="32a0f-227">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="32a0f-228">Envoyez une requête au point de terminaison `/health`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-228">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="32a0f-229">L’application fournit une réponse non intégrité :</span><span class="sxs-lookup"><span data-stu-id="32a0f-229">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="32a0f-230">Séparer les sondages probe readiness et probe liveness</span><span class="sxs-lookup"><span data-stu-id="32a0f-230">Separate readiness and liveness probes</span></span>

<span data-ttu-id="32a0f-231">Dans certains scénarios d’hébergement, une paire de contrôles d’intégrité est utilisée pour distinguer deux états d’application :</span><span class="sxs-lookup"><span data-stu-id="32a0f-231">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="32a0f-232">L’application fonctionne mais n’est pas encore prête à recevoir des requêtes.</span><span class="sxs-lookup"><span data-stu-id="32a0f-232">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="32a0f-233">Il s’agit de l’état de *readiness* qui indique que l’application est prête.</span><span class="sxs-lookup"><span data-stu-id="32a0f-233">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="32a0f-234">L’application fonctionne et répond aux requêtes.</span><span class="sxs-lookup"><span data-stu-id="32a0f-234">The app is functioning and responding to requests.</span></span> <span data-ttu-id="32a0f-235">Il s’agit de l’état de *liveness* qui indique que l’application est active.</span><span class="sxs-lookup"><span data-stu-id="32a0f-235">This state is the app's *liveness*.</span></span>

<span data-ttu-id="32a0f-236">En général, la vérification qui permet de savoir si une application est prête implique un ensemble de vérifications plus complet permettant de déterminer si tous les sous-systèmes et toutes les ressources de l’application sont disponibles, ce qui est un processus assez long.</span><span class="sxs-lookup"><span data-stu-id="32a0f-236">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="32a0f-237">La vérification qui permet de savoir si une application est active est simple et rapide, et vise à déterminer si l’application est disponible pour traiter les requêtes.</span><span class="sxs-lookup"><span data-stu-id="32a0f-237">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="32a0f-238">Une fois que l’application est considérée comme prête, il est inutile de recommencer le test. En effet, le seul test nécessaire ensuite est celui visant à vérifier que l’application est active.</span><span class="sxs-lookup"><span data-stu-id="32a0f-238">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="32a0f-239">L’exemple d’application contient un contrôle d’intégrité permettant de signaler l’achèvement d’une tâche de démarrage de longue durée dans un [service hébergé](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="32a0f-239">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="32a0f-240">`StartupHostedServiceHealthCheck` expose la propriété `StartupTaskCompleted`, que le service hébergé peut définir sur `true` lorsque sa tâche de longue durée est terminée (*StartupHostedServiceHealthCheck.cs*) :</span><span class="sxs-lookup"><span data-stu-id="32a0f-240">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=5)]

<span data-ttu-id="32a0f-241">La tâche de longue durée en arrière-plan est démarrée par un [service hébergé](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span><span class="sxs-lookup"><span data-stu-id="32a0f-241">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="32a0f-242">À la fin de la tâche, `StartupHostedServiceHealthCheck.StartupTaskCompleted` est défini sur `true` :</span><span class="sxs-lookup"><span data-stu-id="32a0f-242">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="32a0f-243">Le contrôle d’intégrité est inscrit auprès de `AddCheck` dans `Startup.ConfigureServices` en même temps que le service hébergé.</span><span class="sxs-lookup"><span data-stu-id="32a0f-243">The health check is registered with `AddCheck` in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="32a0f-244">Étant donné que le service hébergé doit définir la propriété sur le contrôle d’intégrité, le contrôle d’intégrité est également inscrit dans le conteneur du service (*LivenessProbeStartup.cs*) :</span><span class="sxs-lookup"><span data-stu-id="32a0f-244">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="32a0f-245">Appelez Health Check Middleware dans le pipeline de traitement d’application de `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-245">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="32a0f-246">Dans l’exemple d’application, les points de terminaison de contrôle d’intégrité sont créés au niveau de `/health/ready` pour vérifier si l’application est prête, et au niveau de `/health/live` pour vérifier si l’application est active.</span><span class="sxs-lookup"><span data-stu-id="32a0f-246">In the sample app, the health check endpoints are created at `/health/ready` for the readiness check and `/health/live` for the liveness check.</span></span> <span data-ttu-id="32a0f-247">Le test qui permet de vérifier si l’application est prête filtre les contrôles d’intégrité pour n’afficher que celui dont l’étiquette est `ready`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-247">The readiness check filters health checks to the health check with the `ready` tag.</span></span> <span data-ttu-id="32a0f-248">Le test qui permet de vérifier si l’application est active exclut le `StartupHostedServiceHealthCheck` en retournant `false` dans `HealthCheckOptions.Predicate` (pour plus d’informations, consultez [Filtrer les contrôles d’intégrité](#filter-health-checks)) :</span><span class="sxs-lookup"><span data-stu-id="32a0f-248">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the `HealthCheckOptions.Predicate` (for more information, see [Filter health checks](#filter-health-checks)):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_Configure)]

<span data-ttu-id="32a0f-249">Pour exécuter le scénario de configuration readiness/liveness à l’aide de l’exemple d’application, exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="32a0f-249">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario liveness
```

<span data-ttu-id="32a0f-250">Dans un navigateur, accédez à `/health/ready` plusieurs fois jusqu’à ce que 15 secondes soient écoulées.</span><span class="sxs-lookup"><span data-stu-id="32a0f-250">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="32a0f-251">Le contrôle d’intégrité signale `Unhealthy` pendant les 15 premières secondes.</span><span class="sxs-lookup"><span data-stu-id="32a0f-251">The health check reports `Unhealthy` for the first 15 seconds.</span></span> <span data-ttu-id="32a0f-252">Après 15 secondes, le point de terminaison signale `Healthy`, ce qui reflète l’achèvement de la tâche de longue durée exécutée par le service hébergé.</span><span class="sxs-lookup"><span data-stu-id="32a0f-252">After 15 seconds, the endpoint reports `Healthy`, which reflects the completion of the long-running task by the hosted service.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="32a0f-253">Exemple Kubernetes</span><span class="sxs-lookup"><span data-stu-id="32a0f-253">Kubernetes example</span></span>

<span data-ttu-id="32a0f-254">Dans un environnement tel que [Kubernetes](https://kubernetes.io/), il peut être utile d’utiliser séparément le test permettant de savoir si la l’application est prête et celui visant à savoir si l’application est active.</span><span class="sxs-lookup"><span data-stu-id="32a0f-254">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="32a0f-255">Dans Kubernetes, une application peut devoir effectuer une tâche de démarrage de longue durée avant d’accepter des requêtes, telles qu’un test de la disponibilité de la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="32a0f-255">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="32a0f-256">L’utilisation séparée des deux tests permet à l’orchestrateur de faire la distinction entre une application qui fonctionne mais qui n’est pas encore prête, et une application qui n’a pas pu démarrer.</span><span class="sxs-lookup"><span data-stu-id="32a0f-256">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="32a0f-257">Pour plus d’informations sur les sondages probe readiness et probe liveness dans Kubernetes, consultez [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) dans la documentation Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="32a0f-257">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="32a0f-258">L’exemple suivant montre une configuration probe readiness Kubernetes :</span><span class="sxs-lookup"><span data-stu-id="32a0f-258">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

```
spec:
  template:
  spec:
    readinessProbe:
      # an http probe
      httpGet:
        path: /health/ready
        port: 80
      # length of time to wait for a pod to initialize
      # after pod startup, before applying health checking
      initialDelaySeconds: 30
      timeoutSeconds: 1
    ports:
      - containerPort: 80
```

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="32a0f-259">Sonde basée sur les métriques avec un enregistreur de réponse personnalisé</span><span class="sxs-lookup"><span data-stu-id="32a0f-259">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="32a0f-260">L’exemple d’application montre un contrôle d’intégrité de mémoire avec un enregistreur de réponse personnalisé.</span><span class="sxs-lookup"><span data-stu-id="32a0f-260">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="32a0f-261">`MemoryHealthCheck` signale un état détérioré si l’application utilise plus de mémoire que le seuil défini (1 Go dans l’exemple d’application).</span><span class="sxs-lookup"><span data-stu-id="32a0f-261">`MemoryHealthCheck` reports a degraded status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="32a0f-262">`HealthCheckResult` inclut des informations de récupérateur de mémoire pour l’application (*MemoryHealthCheck.cs*) :</span><span class="sxs-lookup"><span data-stu-id="32a0f-262">The `HealthCheckResult` includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="32a0f-263">Pour inscrire les services de contrôle d’intégrité, utilisez `AddHealthChecks` dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-263">Register health check services with `AddHealthChecks` in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="32a0f-264">Au lieu d’activer le contrôle d’intégrité en le passant à `AddCheck`, `MemoryHealthCheck` est inscrit en tant que service.</span><span class="sxs-lookup"><span data-stu-id="32a0f-264">Instead of enabling the health check by passing it to `AddCheck`, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="32a0f-265">Tous les services `IHealthCheck` inscrits sont disponibles pour les services de contrôle d’intégrité et le middleware.</span><span class="sxs-lookup"><span data-stu-id="32a0f-265">All `IHealthCheck` registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="32a0f-266">Nous vous recommandons d’inscrire les services de contrôle d’intégrité en tant que services Singleton.</span><span class="sxs-lookup"><span data-stu-id="32a0f-266">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="32a0f-267">*CustomWriterStartup.cs* :</span><span class="sxs-lookup"><span data-stu-id="32a0f-267">*CustomWriterStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="32a0f-268">Appelez Health Check Middleware dans le pipeline de traitement d’application de `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-268">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="32a0f-269">Un délégué `WriteResponse` est fourni à la propriété `ResponseWriter` pour produire une réponse JSON personnalisée lorsque le contrôle d’intégrité est exécuté :</span><span class="sxs-lookup"><span data-stu-id="32a0f-269">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_Configure&highlight=6)]

<span data-ttu-id="32a0f-270">La méthode `WriteResponse` formate le `CompositeHealthCheckResult` en un objet JSON et génère une sortie JSON pour la réponse du contrôle d’intégrité :</span><span class="sxs-lookup"><span data-stu-id="32a0f-270">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="32a0f-271">Pour exécuter le sondage basé sur les métriques avec l’enregistreur de réponse personnalisé à l’aide de l’exemple d’application, exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="32a0f-271">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="32a0f-272">[BeatPulse](https://github.com/Xabaril/BeatPulse) inclut des scénarios de contrôle d’intégrité basé sur les métriques, y compris la vérification du stockage sur disque et la vérification de l’activité de l’application en fonction d’une valeur maximale.</span><span class="sxs-lookup"><span data-stu-id="32a0f-272">[BeatPulse](https://github.com/Xabaril/BeatPulse) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="32a0f-273">[BeatPulse](https://github.com/Xabaril/BeatPulse) n’est pas géré ni pris en charge par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="32a0f-273">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="32a0f-274">Filtrer par port</span><span class="sxs-lookup"><span data-stu-id="32a0f-274">Filter by port</span></span>

<span data-ttu-id="32a0f-275">Si vous appelez `UseHealthChecks` avec un port, les requêtes de contrôle d’intégrité seront limitées au port spécifié.</span><span class="sxs-lookup"><span data-stu-id="32a0f-275">Calling `UseHealthChecks` with a port restricts health check requests to the port specified.</span></span> <span data-ttu-id="32a0f-276">Ceci est généralement utilisé dans les environnements de conteneurs pour exposer un port aux services de supervision.</span><span class="sxs-lookup"><span data-stu-id="32a0f-276">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="32a0f-277">L’exemple d’application configure le port à l’aide du [fournisseur de configuration des variables d’environnement](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="32a0f-277">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="32a0f-278">Le port est défini dans le fichier *launchSettings.json*, puis transmis au fournisseur de configuration via une variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="32a0f-278">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="32a0f-279">Vous devez également configurer le serveur pour écouter les requêtes qui arrivent au port de gestion.</span><span class="sxs-lookup"><span data-stu-id="32a0f-279">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="32a0f-280">Pour utiliser l’exemple d’application dans le but d’illustrer la configuration du port de gestion, créez le fichier *launchSettings.json* dans un dossier *Propriétés*.</span><span class="sxs-lookup"><span data-stu-id="32a0f-280">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="32a0f-281">Le fichier *launchSettings.json* qui suit n’est pas inclus dans les fichiers projet de l’exemple d’application, et doit donc être créé manuellement.</span><span class="sxs-lookup"><span data-stu-id="32a0f-281">The following *launchSettings.json* file isn't included in the sample app's project files and must be created manually.</span></span>

<span data-ttu-id="32a0f-282">*Properties/launchSettings.json* :</span><span class="sxs-lookup"><span data-stu-id="32a0f-282">*Properties/launchSettings.json*:</span></span>

```json
{
  "profiles": {
    "SampleApp": {
      "commandName": "Project",
      "commandLineArgs": "",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development",
        "ASPNETCORE_URLS": "http://localhost:5000/;http://localhost:5001/",
        "ASPNETCORE_MANAGEMENTPORT": "5001"
      },
      "applicationUrl": "http://localhost:5000/"
    }
  }
}
```

<span data-ttu-id="32a0f-283">Pour inscrire les services de contrôle d’intégrité, utilisez `AddHealthChecks` dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-283">Register health check services with `AddHealthChecks` in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="32a0f-284">L’appel à `UseHealthChecks` spécifie le port de gestion (*ManagementPortStartup.cs*) :</span><span class="sxs-lookup"><span data-stu-id="32a0f-284">The call to `UseHealthChecks` specifies the management port (*ManagementPortStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=12,18)]

> [!NOTE]
> <span data-ttu-id="32a0f-285">Vous pouvez éviter de créer le fichier *launchSettings.json* dans l’exemple d’application en définissant explicitement les URL et le port de gestion dans le code.</span><span class="sxs-lookup"><span data-stu-id="32a0f-285">You can avoid creating the *launchSettings.json* file in the sample app by setting the URLs and management port explicitly in code.</span></span> <span data-ttu-id="32a0f-286">Dans *Program.cs* où `WebHostBuilder` est créé, ajoutez un appel à `UseUrls` et fournissez le point de terminaison de réponse normal et le point de terminaison de port de gestion de l’application.</span><span class="sxs-lookup"><span data-stu-id="32a0f-286">In *Program.cs* where the `WebHostBuilder` is created, add a call to `UseUrls` and provide the app's normal response endpoint and the management port endpoint.</span></span> <span data-ttu-id="32a0f-287">Dans *ManagementPortStartup.cs* où `UseHealthChecks` est appelé, spécifiez le port de gestion explicitement.</span><span class="sxs-lookup"><span data-stu-id="32a0f-287">In *ManagementPortStartup.cs* where `UseHealthChecks` is called, specify the management port explicitly.</span></span>
>
> <span data-ttu-id="32a0f-288">*Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="32a0f-288">*Program.cs*:</span></span>
>
> ```csharp
> return new WebHostBuilder()
>     .UseConfiguration(config)
>     .UseUrls("http://localhost:5000/;http://localhost:5001/")
>     .ConfigureLogging(builder =>
>     {
>         builder.SetMinimumLevel(LogLevel.Trace);
>         builder.AddConfiguration(config);
>         builder.AddConsole();
>     })
>     .UseKestrel()
>     .UseStartup(startupType)
>     .Build();
> ```
>
> <span data-ttu-id="32a0f-289">*ManagementPortStartup.cs* :</span><span class="sxs-lookup"><span data-stu-id="32a0f-289">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

<span data-ttu-id="32a0f-290">Pour exécuter le scénario de configuration du port de gestion à l’aide de l’exemple d’application, exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="32a0f-290">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="32a0f-291">Distribuer une bibliothèque de contrôle d’intégrité</span><span class="sxs-lookup"><span data-stu-id="32a0f-291">Distribute a health check library</span></span>

<span data-ttu-id="32a0f-292">Pour distribuer une bibliothèque comme un contrôle d’intégrité :</span><span class="sxs-lookup"><span data-stu-id="32a0f-292">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="32a0f-293">Écrivez un contrôle d’intégrité qui implémente l’interface `IHealthCheck` comme une classe autonome.</span><span class="sxs-lookup"><span data-stu-id="32a0f-293">Write a health check that implements the `IHealthCheck` interface as a standalone class.</span></span> <span data-ttu-id="32a0f-294">La classe peut utiliser une [injection de dépendance](xref:fundamentals/dependency-injection), une activation de type et des [options nommées](xref:fundamentals/configuration/options) pour accéder aux données de configuration.</span><span class="sxs-lookup"><span data-stu-id="32a0f-294">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   ```csharp
   using System;
   using System.Threading;
   using System.Threading.Tasks;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   namespace SampleApp
   {
       public class ExampleHealthCheck : IHealthCheck
       {
           private readonly string _data1;
           private readonly int? _data2;

           public ExampleHealthCheck(string data1, int? data2)
           {
               _data1 = data1 ?? throw new ArgumentNullException(nameof(data1));
               _data2 = data2 ?? throw new ArgumentNullException(nameof(data2));
           }

           public async Task<HealthCheckResult> CheckHealthAsync(
               HealthCheckContext context, CancellationToken cancellationToken)
           {
               try
               {
                   // Health check logic
                   //
                   // data1 and data2 are used in the method to
                   // run the probe's health check logic.

                   // Assume that it's possible for this health check
                   // to throw an AccessViolationException.

                   return HealthCheckResult.Healthy();
               }
               catch (AccessViolationException ex)
               {
                   return new HealthCheckResult(
                       context.Registration.FailureStatus,
                       description: "An access violation occurred during the check.",
                       exception: ex,
                       data: null);
               }
           }
       }
   }
   ```

1. <span data-ttu-id="32a0f-295">Écrivez une méthode d’extension avec des paramètres que l’application consommatrice appelle dans sa méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-295">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="32a0f-296">Dans l’exemple qui suit, prenons la signature de méthode de contrôle d’intégrité suivante :</span><span class="sxs-lookup"><span data-stu-id="32a0f-296">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="32a0f-297">La signature précédente indique que `ExampleHealthCheck` nécessite des données supplémentaires pour traiter la logique de sondage du contrôle d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="32a0f-297">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="32a0f-298">Les données sont fournies au délégué qui est utilisé pour créer l’instance de contrôle d’intégrité lorsque le contrôle d’intégrité est inscrit avec une méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="32a0f-298">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="32a0f-299">Dans l’exemple qui suit, l’appelant spécifie les éléments facultatifs suivants :</span><span class="sxs-lookup"><span data-stu-id="32a0f-299">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="32a0f-300">Nom du contrôle d’intégrité (`name`).</span><span class="sxs-lookup"><span data-stu-id="32a0f-300">health check name (`name`).</span></span> <span data-ttu-id="32a0f-301">Si `null`, `example_health_check` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="32a0f-301">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="32a0f-302">Point de données de chaîne du contrôle d’intégrité (`data1`).</span><span class="sxs-lookup"><span data-stu-id="32a0f-302">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="32a0f-303">Point de données Integer du contrôle d’intégrité (`data2`).</span><span class="sxs-lookup"><span data-stu-id="32a0f-303">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="32a0f-304">Si `null`, `1` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="32a0f-304">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="32a0f-305">État d’échec (`HealthStatus`).</span><span class="sxs-lookup"><span data-stu-id="32a0f-305">failure status (`HealthStatus`).</span></span> <span data-ttu-id="32a0f-306">La valeur par défaut est `null`.</span><span class="sxs-lookup"><span data-stu-id="32a0f-306">The default is `null`.</span></span> <span data-ttu-id="32a0f-307">Si `null`, `HealthStatus.Unhealthy` est signalé pour un état d’échec.</span><span class="sxs-lookup"><span data-stu-id="32a0f-307">If `null`, `HealthStatus.Unhealthy` is reported for a failure status.</span></span>
   * <span data-ttu-id="32a0f-308">Étiquettes (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="32a0f-308">tags (`IEnumerable<string>`).</span></span>

   ```csharp
   using System.Collections.Generic;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   public static class ExampleHealthCheckBuilderExtensions
   {
       const string NAME = "example_health_check";

       public static IHealthChecksBuilder AddExampleHealthCheck(
           this IHealthChecksBuilder builder, 
           string name = default, 
           string data1, 
           int data2 = 1, 
           HealthStatus? failureStatus = default, 
           IEnumerable<string> tags = default)
       {
           return builder.Add(new HealthCheckRegistration(
               name ?? NAME,
               sp => new ExampleHealthCheck(data1, data2),
               failureStatus,
               tags));
       }
   }
   ```

## <a name="health-check-publisher"></a><span data-ttu-id="32a0f-309">Serveur de publication des contrôles d’intégrité</span><span class="sxs-lookup"><span data-stu-id="32a0f-309">Health Check Publisher</span></span>

<span data-ttu-id="32a0f-310">Quand un `IHealthCheckPublisher` est ajouté au conteneur du service, le système de contrôle d’intégrité exécute régulièrement vos contrôles d’intégrité et appelle `PublishAsync` avec le résultat.</span><span class="sxs-lookup"><span data-stu-id="32a0f-310">When an `IHealthCheckPublisher` is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="32a0f-311">C’est utile dans un scénario impliquant un système de supervision basé sur les envois (push) et nécessitant que chaque processus appelle le système de supervision régulièrement afin de déterminer l’état d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="32a0f-311">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="32a0f-312">L’interface `IHealthCheckPublisher` comprend une seule méthode :</span><span class="sxs-lookup"><span data-stu-id="32a0f-312">The `IHealthCheckPublisher` interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

> [!NOTE]
> <span data-ttu-id="32a0f-313">[BeatPulse](https://github.com/Xabaril/BeatPulse) inclut les serveurs de publication de plusieurs systèmes, y compris [Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="32a0f-313">[BeatPulse](https://github.com/Xabaril/BeatPulse) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="32a0f-314">[BeatPulse](https://github.com/Xabaril/BeatPulse) n’est pas géré ni pris en charge par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="32a0f-314">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>
