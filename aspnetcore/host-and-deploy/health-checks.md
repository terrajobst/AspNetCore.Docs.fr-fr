---
title: Contrôles d’intégrité dans ASP.NET Core
author: guardrex
description: Découvrez comment configurer des contrôles d’intégrité pour l’infrastructure ASP.NET Core, comme des applications ou des bases de données.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 07/11/2019
uid: host-and-deploy/health-checks
ms.openlocfilehash: 43b6c3b55170eaf3a989d0f2779edac5290df823
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67855908"
---
# <a name="health-checks-in-aspnet-core"></a><span data-ttu-id="07764-103">Contrôles d’intégrité dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="07764-103">Health checks in ASP.NET Core</span></span>

<span data-ttu-id="07764-104">Par [Luke Latham](https://github.com/guardrex) et [Glenn Condron](https://github.com/glennc)</span><span class="sxs-lookup"><span data-stu-id="07764-104">By [Luke Latham](https://github.com/guardrex) and [Glenn Condron](https://github.com/glennc)</span></span>

<span data-ttu-id="07764-105">ASP.NET Core met à disposition Health Check Middleware ainsi que des bibliothèques afin de créer des rapports sur l’intégrité des composants de l’infrastructure d’application.</span><span class="sxs-lookup"><span data-stu-id="07764-105">ASP.NET Core offers Health Check Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="07764-106">Les contrôles d’intégrité sont exposés par une application comme des points de terminaison HTTP.</span><span class="sxs-lookup"><span data-stu-id="07764-106">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="07764-107">Les points de terminaison de vérification d’intégrité peuvent être configurés pour de nombreux scénarios de supervision en temps réel :</span><span class="sxs-lookup"><span data-stu-id="07764-107">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="07764-108">Les sondes d’intégrité peuvent être utilisées par les orchestrateurs de conteneurs et les équilibreurs de charge afin de vérifier l’état d’une application.</span><span class="sxs-lookup"><span data-stu-id="07764-108">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="07764-109">Par exemple, un orchestrateur de conteneurs peut répondre à un résultat de non intégrité en arrêtant le déploiement ou en redémarrant un conteneur.</span><span class="sxs-lookup"><span data-stu-id="07764-109">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="07764-110">Face à une application non saine, l’équilibreur de charge peut réagir en redirigeant le trafic vers une instance saine.</span><span class="sxs-lookup"><span data-stu-id="07764-110">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="07764-111">L’utilisation de la mémoire, des disques et des autres ressources de serveur physique peut être supervisée dans le cadre d’un contrôle d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="07764-111">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="07764-112">Les contrôles d’intégrité peuvent tester les dépendances d’une application, telles que les bases de données et les points de terminaison de service externes, dans le but de vérifier leur disponibilité et leur bon fonctionnement.</span><span class="sxs-lookup"><span data-stu-id="07764-112">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="07764-113">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="07764-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="07764-114">L’exemple d’application comprend des exemples pour les scénarios décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="07764-114">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="07764-115">Pour exécuter l’exemple d’application selon un scénario donné, utilisez la commande [dotnet run](/dotnet/core/tools/dotnet-run) dans un interpréteur de commandes, à partir du dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="07764-115">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="07764-116">Pour plus d’informations sur l’utilisation de l’exemple d’application, consultez le fichier *README.md* de l’exemple d’application ainsi que les descriptions de scénarios de cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="07764-116">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="07764-117">Prérequis</span><span class="sxs-lookup"><span data-stu-id="07764-117">Prerequisites</span></span>

<span data-ttu-id="07764-118">Les contrôles d’intégrité sont généralement utilisés avec un service de supervision externe ou un orchestrateur de conteneurs pour vérifier l’état d’une application.</span><span class="sxs-lookup"><span data-stu-id="07764-118">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="07764-119">Avant d’ajouter des contrôles d’intégrité à une application, vous devez décider du système de supervision à utiliser.</span><span class="sxs-lookup"><span data-stu-id="07764-119">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="07764-120">Le système de supervision détermine les types de contrôles d’intégrité qui doivent être créés ainsi que la configuration de leurs points de terminaison.</span><span class="sxs-lookup"><span data-stu-id="07764-120">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="07764-121">Référencez le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ou ajoutez une référence de package au package [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks).</span><span class="sxs-lookup"><span data-stu-id="07764-121">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span>

<span data-ttu-id="07764-122">L’exemple d’application fournit du code de démarrage pour illustrer les contrôles d’intégrité de plusieurs scénarios.</span><span class="sxs-lookup"><span data-stu-id="07764-122">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="07764-123">Le scénario [probe de la base de données](#database-probe) vérifie l’intégrité d’une connexion de base de données à l’aide d’[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span><span class="sxs-lookup"><span data-stu-id="07764-123">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="07764-124">Le scénario [Sondage DbContext](#entity-framework-core-dbcontext-probe) vérifie une base de données à l’aide d’un `DbContext` EF Core.</span><span class="sxs-lookup"><span data-stu-id="07764-124">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="07764-125">Pour explorer les scénarios de base de données, l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="07764-125">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="07764-126">Crée une base de données et fournit sa chaîne de connexion dans le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="07764-126">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="07764-127">Possède les références de package suivantes dans son fichier de projet :</span><span class="sxs-lookup"><span data-stu-id="07764-127">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="07764-128">AspNetCore.HealthChecks.SqlServer</span><span class="sxs-lookup"><span data-stu-id="07764-128">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="07764-129">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="07764-129">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="07764-130">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) est un port de [BeatPulse](https://github.com/xabaril/beatpulse) et n’est ni tenu à jour ni pris en charge par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="07764-130">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="07764-131">Un autre scénario de contrôle d’intégrité montre comment filtrer des contrôles d’intégrité selon un port de gestion.</span><span class="sxs-lookup"><span data-stu-id="07764-131">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="07764-132">L’exemple d’application vous oblige à créer un fichier *Properties/launchSettings.json* comprenant l’URL de gestion et le port de gestion.</span><span class="sxs-lookup"><span data-stu-id="07764-132">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="07764-133">Pour plus d’informations, consultez la section [Filtrer par port](#filter-by-port).</span><span class="sxs-lookup"><span data-stu-id="07764-133">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="07764-134">Sondage d’intégrité de base</span><span class="sxs-lookup"><span data-stu-id="07764-134">Basic health probe</span></span>

<span data-ttu-id="07764-135">Pour de nombreuses applications, un sondage d’intégrité de base qui signale la disponibilité d’une application pour le traitement des requêtes (*liveness*) suffit à découvrir l’état de l’application.</span><span class="sxs-lookup"><span data-stu-id="07764-135">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="07764-136">La configuration de base inscrit les services de contrôle d’intégrité, puis appelle Health Check Middleware pour répondre à un point de terminaison d’URL avec une réponse d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="07764-136">The basic configuration registers health check services and calls the Health Check Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="07764-137">Par défaut, aucun contrôle d’intégrité n’est inscrit pour tester les dépendances ou le sous-système.</span><span class="sxs-lookup"><span data-stu-id="07764-137">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="07764-138">L’application est considérée comme saine si elle est capable de répondre à l’URL de point de terminaison de contrôle d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="07764-138">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="07764-139">L’enregistreur de réponse par défaut écrit l’état (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) sous forme de texte en clair qu’il renvoie au client, indiquant si l’état est [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) ou [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus).</span><span class="sxs-lookup"><span data-stu-id="07764-139">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="07764-140">Pour inscrire les services de contrôle d’intégrité, utilisez <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="07764-140">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="07764-141">Ajoutez Health Check Middleware avec <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> dans le pipeline de traitement des requêtes de `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="07764-141">Add Health Check Middleware with <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the request processing pipeline of `Startup.Configure`.</span></span>

<span data-ttu-id="07764-142">Dans l’exemple d’application, le point de terminaison de contrôle d’intégrité est créé au niveau de `/health` (*BasicStartup.cs*) :</span><span class="sxs-lookup"><span data-stu-id="07764-142">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/BasicStartup.cs?name=snippet1&highlight=5,10)]

<span data-ttu-id="07764-143">Pour exécuter le scénario de configuration de base à l’aide de l’exemple d’application, exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="07764-143">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="07764-144">Exemple Docker</span><span class="sxs-lookup"><span data-stu-id="07764-144">Docker example</span></span>

<span data-ttu-id="07764-145">[Docker](xref:host-and-deploy/docker/index) fournit une directive `HEALTHCHECK` intégrée qui peut être utilisée pour vérifier l’état d’une application utilisant la configuration de contrôle d’intégrité de base :</span><span class="sxs-lookup"><span data-stu-id="07764-145">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="07764-146">Créer des contrôles d’intégrité</span><span class="sxs-lookup"><span data-stu-id="07764-146">Create health checks</span></span>

<span data-ttu-id="07764-147">Les contrôles d’intégrité sont créés via l’implémentation de l’interface <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck>.</span><span class="sxs-lookup"><span data-stu-id="07764-147">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="07764-148">La méthode <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> retourne un `Task<` <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> `>` qui indique l’état d’intégrité comme étant `Healthy`, `Degraded` ou `Unhealthy`.</span><span class="sxs-lookup"><span data-stu-id="07764-148">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a `Task<` <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> `>` that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="07764-149">Le résultat est écrit sous la forme de texte en clair avec un code d’état configurable (la configuration est décrite dans la section [Options de contrôle d’intégrité](#health-check-options)).</span><span class="sxs-lookup"><span data-stu-id="07764-149">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="07764-150"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> peut également retourner des paires clé-valeur facultatives.</span><span class="sxs-lookup"><span data-stu-id="07764-150"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

### <a name="example-health-check"></a><span data-ttu-id="07764-151">Exemple de contrôle d’intégrité</span><span class="sxs-lookup"><span data-stu-id="07764-151">Example health check</span></span>

<span data-ttu-id="07764-152">La classe `ExampleHealthCheck` suivante montre la disposition d’un contrôle d’intégrité :</span><span class="sxs-lookup"><span data-stu-id="07764-152">The following `ExampleHealthCheck` class demonstrates the layout of a health check:</span></span>

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

### <a name="register-health-check-services"></a><span data-ttu-id="07764-153">Inscrire les services de contrôle d’intégrité</span><span class="sxs-lookup"><span data-stu-id="07764-153">Register health check services</span></span>

<span data-ttu-id="07764-154">Le type `ExampleHealthCheck` est ajouté aux services de contrôle d’intégrité avec <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> :</span><span class="sxs-lookup"><span data-stu-id="07764-154">The `ExampleHealthCheck` type is added to health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck<ExampleHealthCheck>("example_health_check");
}
```

<span data-ttu-id="07764-155">La surcharge <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> de l’exemple suivant définit l’état d’échec (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) de manière à être signalé lorsque le contrôle d’intégrité signale un échec.</span><span class="sxs-lookup"><span data-stu-id="07764-155">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="07764-156">Si l’état d’échec est défini sur `null` (par défaut), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) est signalé.</span><span class="sxs-lookup"><span data-stu-id="07764-156">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="07764-157">Cette surcharge est utile pour les créateurs de bibliothèque, dans les cas où l’état d’échec indiqué par la bibliothèque est appliqué par l’application lorsqu’un échec est signalé par le contrôle d’intégrité, si l’implémentation de ce dernier respecte le paramètre.</span><span class="sxs-lookup"><span data-stu-id="07764-157">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="07764-158">Des *étiquettes* peuvent être utilisées pour filtrer les contrôles d’intégrité (cette procédure est décrite plus loin dans la section [Filtrer les contrôles d’intégrité](#filter-health-checks)).</span><span class="sxs-lookup"><span data-stu-id="07764-158">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="07764-159"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> peut également exécuter une fonction lambda.</span><span class="sxs-lookup"><span data-stu-id="07764-159"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="07764-160">Dans l’exemple suivant, le nom du contrôle d’intégrité est spécifié en tant que `Example`, et la vérification retourne toujours un état sain :</span><span class="sxs-lookup"><span data-stu-id="07764-160">In the following example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck("Example", () =>
            HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" })
}
```

### <a name="use-health-checks-middleware"></a><span data-ttu-id="07764-161">Utiliser Health Checks Middleware</span><span class="sxs-lookup"><span data-stu-id="07764-161">Use Health Checks Middleware</span></span>

<span data-ttu-id="07764-162">Dans `Startup.Configure`, appelez <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> dans le pipeline de traitement avec l’URL de point de terminaison ou le chemin relatif :</span><span class="sxs-lookup"><span data-stu-id="07764-162">In `Startup.Configure`, call <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the processing pipeline with the endpoint URL or relative path:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health");
}
```

<span data-ttu-id="07764-163">Si les contrôles d’intégrité doivent écouter un port en particulier, utilisez une surcharge de <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> pour définir le port (cette procédure est décrite plus loin dans la section [Filtrer par port](#filter-by-port)) :</span><span class="sxs-lookup"><span data-stu-id="07764-163">If the health checks should listen on a specific port, use an overload of <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> to set the port (described further in the [Filter by port](#filter-by-port) section):</span></span>

```csharp
app.UseHealthChecks("/health", port: 8000);
```

<span data-ttu-id="07764-164">Health Checks Middleware est un *intergiciel (middleware) terminal* présent dans le pipeline de traitement des requêtes de l’application.</span><span class="sxs-lookup"><span data-stu-id="07764-164">Health Checks Middleware is a *terminal middleware* in the app's request processing pipeline.</span></span> <span data-ttu-id="07764-165">Le premier point de terminaison de contrôle d’intégrité a trouvé une correspondance exacte à l’URL de requête et court-circuite le reste du pipeline du middleware.</span><span class="sxs-lookup"><span data-stu-id="07764-165">The first health check endpoint encountered that's an exact match to the request URL executes and short-circuits the rest of the middleware pipeline.</span></span> <span data-ttu-id="07764-166">Lorsqu’un court-circuit se produit, aucun autre middleware n’est exécuté après le contrôle d’intégrité mis en correspondance.</span><span class="sxs-lookup"><span data-stu-id="07764-166">When short-circuiting occurs, no middleware following the matched health check executes.</span></span>

## <a name="health-check-options"></a><span data-ttu-id="07764-167">Options de contrôle d’intégrité</span><span class="sxs-lookup"><span data-stu-id="07764-167">Health check options</span></span>

<span data-ttu-id="07764-168"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> permet de personnaliser le comportement du contrôle d’intégrité :</span><span class="sxs-lookup"><span data-stu-id="07764-168"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="07764-169">Filtrer les contrôles d’intégrité</span><span class="sxs-lookup"><span data-stu-id="07764-169">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="07764-170">Personnaliser le code d’état HTTP</span><span class="sxs-lookup"><span data-stu-id="07764-170">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="07764-171">Supprimer les en-têtes de cache</span><span class="sxs-lookup"><span data-stu-id="07764-171">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="07764-172">Personnaliser la sortie</span><span class="sxs-lookup"><span data-stu-id="07764-172">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="07764-173">Filtrer les contrôles d’intégrité</span><span class="sxs-lookup"><span data-stu-id="07764-173">Filter health checks</span></span>

<span data-ttu-id="07764-174">Par défaut, Health Check Middleware exécute tous les contrôles d’intégrité inscrits.</span><span class="sxs-lookup"><span data-stu-id="07764-174">By default, Health Check Middleware runs all registered health checks.</span></span> <span data-ttu-id="07764-175">Pour exécuter un sous-ensemble de contrôles d’intégrité, fournissez une fonction qui retourne une valeur booléenne à l’option <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate>.</span><span class="sxs-lookup"><span data-stu-id="07764-175">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="07764-176">Dans l’exemple suivant, le contrôle d’intégrité `Bar` est filtré en fonction de son étiquette (`bar_tag`) dans l’instruction conditionnelle de la fonction, où `true` est retourné uniquement si la propriété <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> du contrôle d’intégrité correspond à `foo_tag` ou à `baz_tag` :</span><span class="sxs-lookup"><span data-stu-id="07764-176">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

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

### <a name="customize-the-http-status-code"></a><span data-ttu-id="07764-177">Personnaliser le code d’état HTTP</span><span class="sxs-lookup"><span data-stu-id="07764-177">Customize the HTTP status code</span></span>

<span data-ttu-id="07764-178">Utilisez <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> pour personnaliser le mappage de l’état d’intégrité aux codes d’état HTTP.</span><span class="sxs-lookup"><span data-stu-id="07764-178">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="07764-179">Les affectations <xref:Microsoft.AspNetCore.Http.StatusCodes> suivantes sont les valeurs par défaut utilisées par le middleware.</span><span class="sxs-lookup"><span data-stu-id="07764-179">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="07764-180">Vous pouvez modifier les valeurs de code d’état selon vos besoins.</span><span class="sxs-lookup"><span data-stu-id="07764-180">Change the status code values to meet your requirements.</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // The following StatusCodes are the default assignments for
        // the HealthStatus properties.
        ResultStatusCodes =
        {
            [HealthStatus.Healthy] = StatusCodes.Status200OK,
            [HealthStatus.Degraded] = StatusCodes.Status200OK,
            [HealthStatus.Unhealthy] = StatusCodes.Status503ServiceUnavailable
        }
    });
}
```

### <a name="suppress-cache-headers"></a><span data-ttu-id="07764-181">Supprimer les en-têtes de cache</span><span class="sxs-lookup"><span data-stu-id="07764-181">Suppress cache headers</span></span>

<span data-ttu-id="07764-182"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> contrôle si Health Check Middleware ajoute des en-têtes HTTP à une réponse de sonde pour empêcher la mise en cache de la réponse.</span><span class="sxs-lookup"><span data-stu-id="07764-182"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Check Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="07764-183">Si la valeur est `false` (par défaut), le middleware définit ou substitue les en-têtes `Cache-Control`, `Expires` et `Pragma` afin d’empêcher la mise en cache de la réponse.</span><span class="sxs-lookup"><span data-stu-id="07764-183">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="07764-184">Si la valeur est `true`, le middleware ne modifie pas les en-têtes de cache de la réponse.</span><span class="sxs-lookup"><span data-stu-id="07764-184">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

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

### <a name="customize-output"></a><span data-ttu-id="07764-185">Personnaliser la sortie</span><span class="sxs-lookup"><span data-stu-id="07764-185">Customize output</span></span>

<span data-ttu-id="07764-186">L’option <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> obtient ou définit un délégué utilisé pour écrire la réponse.</span><span class="sxs-lookup"><span data-stu-id="07764-186">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span> <span data-ttu-id="07764-187">Le délégué par défaut écrit une réponse minimale constituée de texte en clair, avec la valeur de chaîne [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span><span class="sxs-lookup"><span data-stu-id="07764-187">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span>

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

## <a name="database-probe"></a><span data-ttu-id="07764-188">Sondage de base de données</span><span class="sxs-lookup"><span data-stu-id="07764-188">Database probe</span></span>

<span data-ttu-id="07764-189">Un contrôle d’intégrité peut spécifier une requête de base de données à exécuter en tant que test booléen pour indiquer si la base de données répond normalement.</span><span class="sxs-lookup"><span data-stu-id="07764-189">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="07764-190">L’exemple d’application utilise [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), une bibliothèque de vérification d’intégrité pour les applications ASP.NET Core, pour effectuer une vérification d’intégrité sur une base de données SQL Server.</span><span class="sxs-lookup"><span data-stu-id="07764-190">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="07764-191">`AspNetCore.Diagnostics.HealthChecks` exécute une demande `SELECT 1` sur la base de données pour confirmer que la connexion à la base de données est saine.</span><span class="sxs-lookup"><span data-stu-id="07764-191">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="07764-192">Lorsque vous vérifiez une connexion de base de données à l’aide d’une requête, choisissez une requête qui est retournée rapidement.</span><span class="sxs-lookup"><span data-stu-id="07764-192">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="07764-193">L’utilisation d’une requête risque néanmoins d’entraîner une surcharge de la base de données et d’en diminuer les performances.</span><span class="sxs-lookup"><span data-stu-id="07764-193">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="07764-194">Dans la plupart des cas, il n’est pas nécessaire d’utiliser une requête de test.</span><span class="sxs-lookup"><span data-stu-id="07764-194">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="07764-195">Il suffit simplement d’établir une connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="07764-195">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="07764-196">Si vous avez besoin d’exécuter une requête, choisissez une requête SELECT simple, telle que `SELECT 1`.</span><span class="sxs-lookup"><span data-stu-id="07764-196">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="07764-197">Inclure une référence de package à [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="07764-197">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="07764-198">Fournissez une chaîne de connexion de base de données valide dans le fichier *appsettings.json* de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="07764-198">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="07764-199">L’application utilise une base de données SQL Server nommée `HealthCheckSample` :</span><span class="sxs-lookup"><span data-stu-id="07764-199">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="07764-200">Pour inscrire les services de contrôle d’intégrité, utilisez <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="07764-200">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="07764-201">L’exemple d’application appelle la méthode `AddSqlServer` avec la chaîne de connexion de la base de données (*DbHealthStartup.cs*) :</span><span class="sxs-lookup"><span data-stu-id="07764-201">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="07764-202">Appelez Health Check Middleware dans le pipeline de traitement d’application de `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="07764-202">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_Configure)]

<span data-ttu-id="07764-203">Pour exécuter le scénario de sondage d’une base de données à l’aide de l’exemple d’application, exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="07764-203">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="07764-204">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) est un port de [BeatPulse](https://github.com/xabaril/beatpulse) et n’est ni tenu à jour ni pris en charge par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="07764-204">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="07764-205">Sondage DbContext Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="07764-205">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="07764-206">La vérification `DbContext` vérifie que l’application peut communiquer avec la base de données configurée pour un `DbContext` EF Core.</span><span class="sxs-lookup"><span data-stu-id="07764-206">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="07764-207">La vérification `DbContext` est prise en charge dans les applications qui :</span><span class="sxs-lookup"><span data-stu-id="07764-207">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="07764-208">Utilisent [Entity Framework (EF) Core](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="07764-208">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="07764-209">Incluent une référence de package à [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span><span class="sxs-lookup"><span data-stu-id="07764-209">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="07764-210">`AddDbContextCheck<TContext>` inscrit un contrôle d’intégrité pour un `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="07764-210">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="07764-211">Le `DbContext` est fourni en tant que `TContext` à la méthode.</span><span class="sxs-lookup"><span data-stu-id="07764-211">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="07764-212">Une surcharge est disponible pour configurer l’état d’échec, les étiquettes et une requête de test personnalisée.</span><span class="sxs-lookup"><span data-stu-id="07764-212">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="07764-213">Par défaut :</span><span class="sxs-lookup"><span data-stu-id="07764-213">By default:</span></span>

* <span data-ttu-id="07764-214">`DbContextHealthCheck` appelle la méthode `CanConnectAsync` d’EF Core.</span><span class="sxs-lookup"><span data-stu-id="07764-214">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="07764-215">Vous pouvez choisir quelle opération doit être exécutée lors du contrôle d’intégrité à l’aide des surcharges de la méthode `AddDbContextCheck`.</span><span class="sxs-lookup"><span data-stu-id="07764-215">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="07764-216">Le nom du contrôle d’intégrité correspond à celui du type `TContext`.</span><span class="sxs-lookup"><span data-stu-id="07764-216">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="07764-217">Dans l’exemple d’application, `AppDbContext` est fourni à `AddDbContextCheck`, puis est inscrit en tant que service dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="07764-217">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="07764-218">*DbContextHealthStartup.cs* :</span><span class="sxs-lookup"><span data-stu-id="07764-218">*DbContextHealthStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="07764-219">Dans l’exemple d’application, `UseHealthChecks` ajoute Health Check Middleware dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="07764-219">In the sample app, `UseHealthChecks` adds the Health Check Middleware in `Startup.Configure`.</span></span>

<span data-ttu-id="07764-220">*DbContextHealthStartup.cs* :</span><span class="sxs-lookup"><span data-stu-id="07764-220">*DbContextHealthStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_Configure)]

<span data-ttu-id="07764-221">Pour exécuter le scénario de sondage `DbContext` à l’aide de l’exemple d’application, vérifiez que la base de données spécifiée par le la chaîne de connexion ne se trouve pas déjà dans l’instance SQL Server.</span><span class="sxs-lookup"><span data-stu-id="07764-221">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="07764-222">Si la base de données existe, supprimez-la.</span><span class="sxs-lookup"><span data-stu-id="07764-222">If the database exists, delete it.</span></span>

<span data-ttu-id="07764-223">Exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="07764-223">Execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario dbcontext
```

<span data-ttu-id="07764-224">Une fois que l’application est en cours d’exécution, vérifiez l’état d’intégrité en envoyant une requête au point de terminaison `/health` dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="07764-224">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="07764-225">La base de données et `AppDbContext` n’existent pas, donc l’application fournit la réponse suivante :</span><span class="sxs-lookup"><span data-stu-id="07764-225">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="07764-226">Déclenchez l’exemple d’application pour créer la base de données.</span><span class="sxs-lookup"><span data-stu-id="07764-226">Trigger the sample app to create the database.</span></span> <span data-ttu-id="07764-227">Envoyez une requête à `/createdatabase`.</span><span class="sxs-lookup"><span data-stu-id="07764-227">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="07764-228">L’application répond :</span><span class="sxs-lookup"><span data-stu-id="07764-228">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="07764-229">Envoyez une requête au point de terminaison `/health`.</span><span class="sxs-lookup"><span data-stu-id="07764-229">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="07764-230">La base de données et le contexte existent, donc l’application répond :</span><span class="sxs-lookup"><span data-stu-id="07764-230">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="07764-231">Déclenchez l’exemple d’application pour supprimer la base de données.</span><span class="sxs-lookup"><span data-stu-id="07764-231">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="07764-232">Envoyez une requête à `/deletedatabase`.</span><span class="sxs-lookup"><span data-stu-id="07764-232">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="07764-233">L’application répond :</span><span class="sxs-lookup"><span data-stu-id="07764-233">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="07764-234">Envoyez une requête au point de terminaison `/health`.</span><span class="sxs-lookup"><span data-stu-id="07764-234">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="07764-235">L’application fournit une réponse non intégrité :</span><span class="sxs-lookup"><span data-stu-id="07764-235">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="07764-236">Séparer les sondages probe readiness et probe liveness</span><span class="sxs-lookup"><span data-stu-id="07764-236">Separate readiness and liveness probes</span></span>

<span data-ttu-id="07764-237">Dans certains scénarios d’hébergement, une paire de contrôles d’intégrité est utilisée pour distinguer deux états d’application :</span><span class="sxs-lookup"><span data-stu-id="07764-237">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="07764-238">L’application fonctionne mais n’est pas encore prête à recevoir des requêtes.</span><span class="sxs-lookup"><span data-stu-id="07764-238">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="07764-239">Il s’agit de l’état de *readiness* qui indique que l’application est prête.</span><span class="sxs-lookup"><span data-stu-id="07764-239">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="07764-240">L’application fonctionne et répond aux requêtes.</span><span class="sxs-lookup"><span data-stu-id="07764-240">The app is functioning and responding to requests.</span></span> <span data-ttu-id="07764-241">Il s’agit de l’état de *liveness* qui indique que l’application est active.</span><span class="sxs-lookup"><span data-stu-id="07764-241">This state is the app's *liveness*.</span></span>

<span data-ttu-id="07764-242">En général, la vérification qui permet de savoir si une application est prête implique un ensemble de vérifications plus complet permettant de déterminer si tous les sous-systèmes et toutes les ressources de l’application sont disponibles, ce qui est un processus assez long.</span><span class="sxs-lookup"><span data-stu-id="07764-242">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="07764-243">La vérification qui permet de savoir si une application est active est simple et rapide, et vise à déterminer si l’application est disponible pour traiter les requêtes.</span><span class="sxs-lookup"><span data-stu-id="07764-243">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="07764-244">Une fois que l’application est considérée comme prête, il est inutile de recommencer le test. En effet, le seul test nécessaire ensuite est celui visant à vérifier que l’application est active.</span><span class="sxs-lookup"><span data-stu-id="07764-244">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="07764-245">L’exemple d’application contient un contrôle d’intégrité permettant de signaler l’achèvement d’une tâche de démarrage de longue durée dans un [service hébergé](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="07764-245">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="07764-246">`StartupHostedServiceHealthCheck` expose la propriété `StartupTaskCompleted`, que le service hébergé peut définir sur `true` lorsque sa tâche de longue durée est terminée (*StartupHostedServiceHealthCheck.cs*) :</span><span class="sxs-lookup"><span data-stu-id="07764-246">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="07764-247">La tâche de longue durée en arrière-plan est démarrée par un [service hébergé](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span><span class="sxs-lookup"><span data-stu-id="07764-247">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="07764-248">À la fin de la tâche, `StartupHostedServiceHealthCheck.StartupTaskCompleted` est défini sur `true` :</span><span class="sxs-lookup"><span data-stu-id="07764-248">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="07764-249">Le contrôle d’intégrité est inscrit auprès de <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> dans `Startup.ConfigureServices` en même temps que le service hébergé.</span><span class="sxs-lookup"><span data-stu-id="07764-249">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="07764-250">Étant donné que le service hébergé doit définir la propriété sur le contrôle d’intégrité, le contrôle d’intégrité est également inscrit dans le conteneur du service (*LivenessProbeStartup.cs*) :</span><span class="sxs-lookup"><span data-stu-id="07764-250">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="07764-251">Appelez Health Check Middleware dans le pipeline de traitement d’application de `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="07764-251">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="07764-252">Dans l’exemple d’application, les points de terminaison de contrôle d’intégrité sont créés au niveau de `/health/ready` pour vérifier si l’application est prête, et au niveau de `/health/live` pour vérifier si l’application est active.</span><span class="sxs-lookup"><span data-stu-id="07764-252">In the sample app, the health check endpoints are created at `/health/ready` for the readiness check and `/health/live` for the liveness check.</span></span> <span data-ttu-id="07764-253">Le test qui permet de vérifier si l’application est prête filtre les contrôles d’intégrité pour n’afficher que celui dont l’étiquette est `ready`.</span><span class="sxs-lookup"><span data-stu-id="07764-253">The readiness check filters health checks to the health check with the `ready` tag.</span></span> <span data-ttu-id="07764-254">Le test qui permet de vérifier si l’application est active exclut le `StartupHostedServiceHealthCheck` en retournant `false` dans [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (pour plus d’informations, consultez [Filtrer les contrôles d’intégrité](#filter-health-checks)) :</span><span class="sxs-lookup"><span data-stu-id="07764-254">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks)):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_Configure)]

<span data-ttu-id="07764-255">Pour exécuter le scénario de configuration readiness/liveness à l’aide de l’exemple d’application, exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="07764-255">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario liveness
```

<span data-ttu-id="07764-256">Dans un navigateur, accédez à `/health/ready` plusieurs fois jusqu’à ce que 15 secondes soient écoulées.</span><span class="sxs-lookup"><span data-stu-id="07764-256">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="07764-257">Le contrôle d’intégrité signale *Unhealthy* pendant les 15 premières secondes.</span><span class="sxs-lookup"><span data-stu-id="07764-257">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="07764-258">Après 15 secondes, le point de terminaison signale *Healthy*, ce qui reflète l’achèvement de la tâche de longue durée exécutée par le service hébergé.</span><span class="sxs-lookup"><span data-stu-id="07764-258">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="07764-259">Cet exemple crée également un éditeur de vérification de l’intégrité (implémentation <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>) qui exécute la première vérification de disponibilité avec un délai de deux secondes.</span><span class="sxs-lookup"><span data-stu-id="07764-259">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="07764-260">Pour plus d’informations, consultez la section [Éditeur de vérification de l’intégrité](#health-check-publisher).</span><span class="sxs-lookup"><span data-stu-id="07764-260">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="07764-261">Exemple Kubernetes</span><span class="sxs-lookup"><span data-stu-id="07764-261">Kubernetes example</span></span>

<span data-ttu-id="07764-262">Dans un environnement tel que [Kubernetes](https://kubernetes.io/), il peut être utile d’utiliser séparément le test permettant de savoir si la l’application est prête et celui visant à savoir si l’application est active.</span><span class="sxs-lookup"><span data-stu-id="07764-262">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="07764-263">Dans Kubernetes, une application peut devoir effectuer une tâche de démarrage de longue durée avant d’accepter des requêtes, telles qu’un test de la disponibilité de la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="07764-263">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="07764-264">L’utilisation séparée des deux tests permet à l’orchestrateur de faire la distinction entre une application qui fonctionne mais qui n’est pas encore prête, et une application qui n’a pas pu démarrer.</span><span class="sxs-lookup"><span data-stu-id="07764-264">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="07764-265">Pour plus d’informations sur les sondages probe readiness et probe liveness dans Kubernetes, consultez [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) dans la documentation Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="07764-265">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="07764-266">L’exemple suivant montre une configuration probe readiness Kubernetes :</span><span class="sxs-lookup"><span data-stu-id="07764-266">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="07764-267">Sonde basée sur les métriques avec un enregistreur de réponse personnalisé</span><span class="sxs-lookup"><span data-stu-id="07764-267">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="07764-268">L’exemple d’application montre un contrôle d’intégrité de mémoire avec un enregistreur de réponse personnalisé.</span><span class="sxs-lookup"><span data-stu-id="07764-268">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="07764-269">`MemoryHealthCheck` signale un état détérioré si l’application utilise plus de mémoire que le seuil défini (1 Go dans l’exemple d’application).</span><span class="sxs-lookup"><span data-stu-id="07764-269">`MemoryHealthCheck` reports a degraded status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="07764-270"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> inclut des informations de récupérateur de mémoire pour l’application (*MemoryHealthCheck.cs*) :</span><span class="sxs-lookup"><span data-stu-id="07764-270">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="07764-271">Pour inscrire les services de contrôle d’intégrité, utilisez <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="07764-271">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="07764-272">Au lieu d’activer le contrôle d’intégrité en le passant à <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, `MemoryHealthCheck` est inscrit en tant que service.</span><span class="sxs-lookup"><span data-stu-id="07764-272">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="07764-273">Tous les services <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> inscrits sont disponibles pour les services de contrôle d’intégrité et le middleware.</span><span class="sxs-lookup"><span data-stu-id="07764-273">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="07764-274">Nous vous recommandons d’inscrire les services de contrôle d’intégrité en tant que services Singleton.</span><span class="sxs-lookup"><span data-stu-id="07764-274">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="07764-275">*CustomWriterStartup.cs* :</span><span class="sxs-lookup"><span data-stu-id="07764-275">*CustomWriterStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="07764-276">Appelez Health Check Middleware dans le pipeline de traitement d’application de `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="07764-276">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="07764-277">Un délégué `WriteResponse` est fourni à la propriété `ResponseWriter` pour produire une réponse JSON personnalisée lorsque le contrôle d’intégrité est exécuté :</span><span class="sxs-lookup"><span data-stu-id="07764-277">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_Configure&highlight=6)]

<span data-ttu-id="07764-278">La méthode `WriteResponse` formate le `CompositeHealthCheckResult` en un objet JSON et génère une sortie JSON pour la réponse du contrôle d’intégrité :</span><span class="sxs-lookup"><span data-stu-id="07764-278">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="07764-279">Pour exécuter le sondage basé sur les métriques avec l’enregistreur de réponse personnalisé à l’aide de l’exemple d’application, exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="07764-279">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="07764-280">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) inclut des scénarios de vérification de l’intégrité basés sur des métriques, notamment des vérifications du stockage sur disque et de la vivacité maximale des valeurs.</span><span class="sxs-lookup"><span data-stu-id="07764-280">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="07764-281">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) est un port de [BeatPulse](https://github.com/xabaril/beatpulse) et n’est ni tenu à jour ni pris en charge par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="07764-281">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="07764-282">Filtrer par port</span><span class="sxs-lookup"><span data-stu-id="07764-282">Filter by port</span></span>

<span data-ttu-id="07764-283">Si vous appelez <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> avec un port, les requêtes de contrôle d’intégrité seront limitées au port spécifié.</span><span class="sxs-lookup"><span data-stu-id="07764-283">Calling <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> with a port restricts health check requests to the port specified.</span></span> <span data-ttu-id="07764-284">Ceci est généralement utilisé dans les environnements de conteneurs pour exposer un port aux services de supervision.</span><span class="sxs-lookup"><span data-stu-id="07764-284">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="07764-285">L’exemple d’application configure le port à l’aide du [fournisseur de configuration des variables d’environnement](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="07764-285">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="07764-286">Le port est défini dans le fichier *launchSettings.json*, puis transmis au fournisseur de configuration via une variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="07764-286">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="07764-287">Vous devez également configurer le serveur pour écouter les requêtes qui arrivent au port de gestion.</span><span class="sxs-lookup"><span data-stu-id="07764-287">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="07764-288">Pour utiliser l’exemple d’application dans le but d’illustrer la configuration du port de gestion, créez le fichier *launchSettings.json* dans un dossier *Propriétés*.</span><span class="sxs-lookup"><span data-stu-id="07764-288">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="07764-289">Le fichier *launchSettings.json* qui suit n’est pas inclus dans les fichiers projet de l’exemple d’application, et doit donc être créé manuellement.</span><span class="sxs-lookup"><span data-stu-id="07764-289">The following *launchSettings.json* file isn't included in the sample app's project files and must be created manually.</span></span>

<span data-ttu-id="07764-290">*Properties/launchSettings.json* :</span><span class="sxs-lookup"><span data-stu-id="07764-290">*Properties/launchSettings.json*:</span></span>

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

<span data-ttu-id="07764-291">Pour inscrire les services de contrôle d’intégrité, utilisez <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="07764-291">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="07764-292">L’appel à <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> spécifie le port de gestion (*ManagementPortStartup.cs*) :</span><span class="sxs-lookup"><span data-stu-id="07764-292">The call to <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> specifies the management port (*ManagementPortStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=12,18)]

> [!NOTE]
> <span data-ttu-id="07764-293">Vous pouvez éviter de créer le fichier *launchSettings.json* dans l’exemple d’application en définissant explicitement les URL et le port de gestion dans le code.</span><span class="sxs-lookup"><span data-stu-id="07764-293">You can avoid creating the *launchSettings.json* file in the sample app by setting the URLs and management port explicitly in code.</span></span> <span data-ttu-id="07764-294">Dans *Program.cs* où <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> est créé, ajoutez un appel à <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> et fournissez le point de terminaison de réponse normal et le point de terminaison de port de gestion de l’application.</span><span class="sxs-lookup"><span data-stu-id="07764-294">In *Program.cs* where the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> and provide the app's normal response endpoint and the management port endpoint.</span></span> <span data-ttu-id="07764-295">Dans *ManagementPortStartup.cs* où <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> est appelé, spécifiez le port de gestion explicitement.</span><span class="sxs-lookup"><span data-stu-id="07764-295">In *ManagementPortStartup.cs* where <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> is called, specify the management port explicitly.</span></span>
>
> <span data-ttu-id="07764-296">*Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="07764-296">*Program.cs*:</span></span>
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
> <span data-ttu-id="07764-297">*ManagementPortStartup.cs* :</span><span class="sxs-lookup"><span data-stu-id="07764-297">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

<span data-ttu-id="07764-298">Pour exécuter le scénario de configuration du port de gestion à l’aide de l’exemple d’application, exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="07764-298">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="07764-299">Distribuer une bibliothèque de contrôle d’intégrité</span><span class="sxs-lookup"><span data-stu-id="07764-299">Distribute a health check library</span></span>

<span data-ttu-id="07764-300">Pour distribuer une bibliothèque comme un contrôle d’intégrité :</span><span class="sxs-lookup"><span data-stu-id="07764-300">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="07764-301">Écrivez un contrôle d’intégrité qui implémente l’interface <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> comme une classe autonome.</span><span class="sxs-lookup"><span data-stu-id="07764-301">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="07764-302">La classe peut utiliser une [injection de dépendance](xref:fundamentals/dependency-injection), une activation de type et des [options nommées](xref:fundamentals/configuration/options) pour accéder aux données de configuration.</span><span class="sxs-lookup"><span data-stu-id="07764-302">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

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

1. <span data-ttu-id="07764-303">Écrivez une méthode d’extension avec des paramètres que l’application consommatrice appelle dans sa méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="07764-303">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="07764-304">Dans l’exemple qui suit, prenons la signature de méthode de contrôle d’intégrité suivante :</span><span class="sxs-lookup"><span data-stu-id="07764-304">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="07764-305">La signature précédente indique que `ExampleHealthCheck` nécessite des données supplémentaires pour traiter la logique de sondage du contrôle d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="07764-305">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="07764-306">Les données sont fournies au délégué qui est utilisé pour créer l’instance de contrôle d’intégrité lorsque le contrôle d’intégrité est inscrit avec une méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="07764-306">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="07764-307">Dans l’exemple qui suit, l’appelant spécifie les éléments facultatifs suivants :</span><span class="sxs-lookup"><span data-stu-id="07764-307">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="07764-308">Nom du contrôle d’intégrité (`name`).</span><span class="sxs-lookup"><span data-stu-id="07764-308">health check name (`name`).</span></span> <span data-ttu-id="07764-309">Si `null`, `example_health_check` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="07764-309">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="07764-310">Point de données de chaîne du contrôle d’intégrité (`data1`).</span><span class="sxs-lookup"><span data-stu-id="07764-310">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="07764-311">Point de données Integer du contrôle d’intégrité (`data2`).</span><span class="sxs-lookup"><span data-stu-id="07764-311">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="07764-312">Si `null`, `1` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="07764-312">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="07764-313">État d’échec (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span><span class="sxs-lookup"><span data-stu-id="07764-313">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="07764-314">La valeur par défaut est `null`.</span><span class="sxs-lookup"><span data-stu-id="07764-314">The default is `null`.</span></span> <span data-ttu-id="07764-315">Si `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) est signalé pour un état d’échec.</span><span class="sxs-lookup"><span data-stu-id="07764-315">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="07764-316">Étiquettes (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="07764-316">tags (`IEnumerable<string>`).</span></span>

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

## <a name="health-check-publisher"></a><span data-ttu-id="07764-317">Serveur de publication des contrôles d’intégrité</span><span class="sxs-lookup"><span data-stu-id="07764-317">Health Check Publisher</span></span>

<span data-ttu-id="07764-318">Quand un <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> est ajouté au conteneur du service, le système de contrôle d’intégrité exécute régulièrement vos contrôles d’intégrité et appelle `PublishAsync` avec le résultat.</span><span class="sxs-lookup"><span data-stu-id="07764-318">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="07764-319">C’est utile dans un scénario impliquant un système de supervision basé sur les envois (push) et nécessitant que chaque processus appelle le système de supervision régulièrement afin de déterminer l’état d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="07764-319">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="07764-320">L’interface <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> comprend une seule méthode :</span><span class="sxs-lookup"><span data-stu-id="07764-320">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="07764-321"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> vous autorise à définir :</span><span class="sxs-lookup"><span data-stu-id="07764-321"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="07764-322"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; Le retard initial appliqué après le démarrage de l’application avant d’exécuter les instances <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="07764-322"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="07764-323">Le retard est appliqué à une seule fois au démarrage et ne s’applique pas aux itérations ultérieures.</span><span class="sxs-lookup"><span data-stu-id="07764-323">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="07764-324">La valeur par défaut est de cinq secondes.</span><span class="sxs-lookup"><span data-stu-id="07764-324">The default value is five seconds.</span></span>
* <span data-ttu-id="07764-325"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; La période d’exécution de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="07764-325"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="07764-326">La valeur par défaut est de 30 secondes.</span><span class="sxs-lookup"><span data-stu-id="07764-326">The default value is 30 seconds.</span></span>
* <span data-ttu-id="07764-327"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; Si <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> est `null` (valeur par défaut), le service d’éditeur de vérification de l’intégrité exécute toutes les vérifications d’intégrité inscrites.</span><span class="sxs-lookup"><span data-stu-id="07764-327"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="07764-328">Pour exécuter un sous-ensemble de contrôles d’intégrité, fournissez une fonction qui filtre l’ensemble de vérifications.</span><span class="sxs-lookup"><span data-stu-id="07764-328">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="07764-329">Le prédicat est évalué à chaque période.</span><span class="sxs-lookup"><span data-stu-id="07764-329">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="07764-330"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; Le délai d’attente pour l’exécution des vérifications d’intégrité pour toutes les instances <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="07764-330"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="07764-331">Utilisez <xref:System.Threading.Timeout.InfiniteTimeSpan> pour une exécution sans délai d’attente.</span><span class="sxs-lookup"><span data-stu-id="07764-331">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="07764-332">La valeur par défaut est de 30 secondes.</span><span class="sxs-lookup"><span data-stu-id="07764-332">The default value is 30 seconds.</span></span>

::: moniker range="= aspnetcore-2.2"

> [!WARNING]
> <span data-ttu-id="07764-333">Dans la version ASP.NET Core 2.2, le paramètre <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> n’est pas respecté par l’implémentation <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> ; il définit la valeur de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span><span class="sxs-lookup"><span data-stu-id="07764-333">In the ASP.NET Core 2.2 release, setting <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> isn't honored by the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation; it sets the value of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span></span> <span data-ttu-id="07764-334">Ce problème sera corrigé dans ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="07764-334">This issue will be fixed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="07764-335">Pour plus d’informations, consultez [HealthCheckPublisherOptions.Period définit la valeur de. Delay](https://github.com/aspnet/Extensions/issues/1041).</span><span class="sxs-lookup"><span data-stu-id="07764-335">For more information, see [HealthCheckPublisherOptions.Period sets the value of .Delay](https://github.com/aspnet/Extensions/issues/1041).</span></span>

::: moniker-end

<span data-ttu-id="07764-336">Dans l’exemple d’application, `ReadinessPublisher` est une implémentation <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="07764-336">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="07764-337">L’état de la vérification d’intégrité est enregistré dans `Entries` et consigné pour chaque vérification :</span><span class="sxs-lookup"><span data-stu-id="07764-337">The health check status is recorded in `Entries` and logged for each check:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=20,22-23)]

<span data-ttu-id="07764-338">Dans l’exemple d’application `LivenessProbeStartup`, la vérification de la disponibilité `StartupHostedService` a un délai de démarrage de deux secondes et exécute la vérification toutes les 30 secondes.</span><span class="sxs-lookup"><span data-stu-id="07764-338">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="07764-339">Pour activer l’implémentation <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>, l’exemple enregistre `ReadinessPublisher` comme un service singleton dans le conteneur [d’injection de dépendance (DI)](xref:fundamentals/dependency-injection) :</span><span class="sxs-lookup"><span data-stu-id="07764-339">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices&highlight=12-17,28)]

::: moniker range="= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="07764-340">La solution de contournement suivante autorise l’ajout d’une instance <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> au conteneur de service lorsqu’un ou plusieurs autres services hébergés ont déjà été ajoutés à l’application.</span><span class="sxs-lookup"><span data-stu-id="07764-340">The following workaround permits adding an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instance to the service container when one or more other hosted services have already been added to the app.</span></span> <span data-ttu-id="07764-341">Cette solution de contournement n’est pas nécessaire avec ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="07764-341">This workaround won't be required with the release of ASP.NET Core 3.0.</span></span> <span data-ttu-id="07764-342">Pour plus d’informations, consultez https://github.com/aspnet/Extensions/issues/639.</span><span class="sxs-lookup"><span data-stu-id="07764-342">For more information, see: https://github.com/aspnet/Extensions/issues/639.</span></span>
>
> ```csharp
> private const string HealthCheckServiceAssembly =
>     "Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherHostedService";
>
> services.TryAddEnumerable(
>     ServiceDescriptor.Singleton(typeof(IHostedService),
>         typeof(HealthCheckPublisherOptions).Assembly
>             .GetType(HealthCheckServiceAssembly)));
> ```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="07764-343">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) inclut les serveurs de publication pour plusieurs systèmes, notamment [Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="07764-343">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="07764-344">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) est un port de [BeatPulse](https://github.com/xabaril/beatpulse) et n’est ni tenu à jour ni pris en charge par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="07764-344">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="07764-345">Restreindre les vérifications d’intégrité avec MapWhen</span><span class="sxs-lookup"><span data-stu-id="07764-345">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="07764-346">Utilisez <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> pour créer une branche conditionnelle du pipeline des demandes pour les points de terminaison de contrôle d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="07764-346">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="07764-347">Dans l’exemple suivant, `MapWhen` crée une branche du pipeline des demandes pour activer l’intergiciel (middleware) de contrôle d’intégrité si une demande GET est reçue pour le point de terminaison `api/HealthCheck` :</span><span class="sxs-lookup"><span data-stu-id="07764-347">In the following example, `MapWhen` branches the request pipeline to activate Health Check Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

```csharp
app.MapWhen(
    context => context.Request.Method == HttpMethod.Get.Method && 
        context.Request.Path.StartsWith("/api/HealthCheck"),
    builder => builder.UseHealthChecks());

app.UseMvc();
```

<span data-ttu-id="07764-348">Pour plus d’informations, consultez <xref:fundamentals/middleware/index#use-run-and-map>.</span><span class="sxs-lookup"><span data-stu-id="07764-348">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>
