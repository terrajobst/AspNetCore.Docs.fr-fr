---
title: Contrôles d’intégrité dans ASP.NET Core
author: guardrex
description: Découvrez comment configurer des contrôles d’intégrité pour l’infrastructure ASP.NET Core, comme des applications ou des bases de données.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 09/27/2019
uid: host-and-deploy/health-checks
ms.openlocfilehash: e4b2a577815335078f7e0c9128144a514e42a6c3
ms.sourcegitcommit: 5d25a7f22c50ca6fdd0f8ecd8e525822e1b35b7a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/28/2019
ms.locfileid: "71482049"
---
# <a name="health-checks-in-aspnet-core"></a><span data-ttu-id="89698-103">Contrôles d’intégrité dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="89698-103">Health checks in ASP.NET Core</span></span>

<span data-ttu-id="89698-104">Par [Luke Latham](https://github.com/guardrex) et [Glenn Condron](https://github.com/glennc)</span><span class="sxs-lookup"><span data-stu-id="89698-104">By [Luke Latham](https://github.com/guardrex) and [Glenn Condron](https://github.com/glennc)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="89698-105">ASP.NET Core offre un intergiciel et des bibliothèques de contrôle d’intégrité pour signaler l’intégrité des composants d’infrastructure d’application.</span><span class="sxs-lookup"><span data-stu-id="89698-105">ASP.NET Core offers Health Checks Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="89698-106">Les contrôles d’intégrité sont exposés par une application comme des points de terminaison HTTP.</span><span class="sxs-lookup"><span data-stu-id="89698-106">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="89698-107">Les points de terminaison de vérification d’intégrité peuvent être configurés pour de nombreux scénarios de supervision en temps réel :</span><span class="sxs-lookup"><span data-stu-id="89698-107">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="89698-108">Les sondes d’intégrité peuvent être utilisées par les orchestrateurs de conteneurs et les équilibreurs de charge afin de vérifier l’état d’une application.</span><span class="sxs-lookup"><span data-stu-id="89698-108">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="89698-109">Par exemple, un orchestrateur de conteneurs peut répondre à un résultat de non intégrité en arrêtant le déploiement ou en redémarrant un conteneur.</span><span class="sxs-lookup"><span data-stu-id="89698-109">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="89698-110">Face à une application non saine, l’équilibreur de charge peut réagir en redirigeant le trafic vers une instance saine.</span><span class="sxs-lookup"><span data-stu-id="89698-110">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="89698-111">L’utilisation de la mémoire, des disques et des autres ressources de serveur physique peut être supervisée dans le cadre d’un contrôle d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="89698-111">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="89698-112">Les contrôles d’intégrité peuvent tester les dépendances d’une application, telles que les bases de données et les points de terminaison de service externes, dans le but de vérifier leur disponibilité et leur bon fonctionnement.</span><span class="sxs-lookup"><span data-stu-id="89698-112">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="89698-113">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="89698-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="89698-114">L’exemple d’application comprend des exemples pour les scénarios décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="89698-114">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="89698-115">Pour exécuter l’exemple d’application selon un scénario donné, utilisez la commande [dotnet run](/dotnet/core/tools/dotnet-run) dans un interpréteur de commandes, à partir du dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="89698-115">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="89698-116">Pour plus d’informations sur l’utilisation de l’exemple d’application, consultez le fichier *README.md* de l’exemple d’application ainsi que les descriptions de scénarios de cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="89698-116">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="89698-117">Prérequis</span><span class="sxs-lookup"><span data-stu-id="89698-117">Prerequisites</span></span>

<span data-ttu-id="89698-118">Les contrôles d’intégrité sont généralement utilisés avec un service de supervision externe ou un orchestrateur de conteneurs pour vérifier l’état d’une application.</span><span class="sxs-lookup"><span data-stu-id="89698-118">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="89698-119">Avant d’ajouter des contrôles d’intégrité à une application, vous devez décider du système de supervision à utiliser.</span><span class="sxs-lookup"><span data-stu-id="89698-119">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="89698-120">Le système de supervision détermine les types de contrôles d’intégrité qui doivent être créés ainsi que la configuration de leurs points de terminaison.</span><span class="sxs-lookup"><span data-stu-id="89698-120">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="89698-121">Ajoutez une référence de package au package [Microsoft. AspNetCore. Diagnostics. HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) .</span><span class="sxs-lookup"><span data-stu-id="89698-121">Add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span> <span data-ttu-id="89698-122">Pour effectuer des contrôles d’intégrité à l’aide de Entity Framework Core, ajoutez une référence de package au package [Microsoft. extensions. Diagnostics. HealthChecks. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore) .</span><span class="sxs-lookup"><span data-stu-id="89698-122">To perform health checks using Entity Framework Core, add a package reference to the [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore) package.</span></span>

<span data-ttu-id="89698-123">L’exemple d’application fournit du code de démarrage pour illustrer les contrôles d’intégrité de plusieurs scénarios.</span><span class="sxs-lookup"><span data-stu-id="89698-123">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="89698-124">Le scénario [probe de la base de données](#database-probe) vérifie l’intégrité d’une connexion de base de données à l’aide d’[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span><span class="sxs-lookup"><span data-stu-id="89698-124">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="89698-125">Le scénario [Sondage DbContext](#entity-framework-core-dbcontext-probe) vérifie une base de données à l’aide d’un `DbContext` EF Core.</span><span class="sxs-lookup"><span data-stu-id="89698-125">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="89698-126">Pour explorer les scénarios de base de données, l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="89698-126">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="89698-127">Crée une base de données et fournit sa chaîne de connexion dans le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="89698-127">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="89698-128">Possède les références de package suivantes dans son fichier de projet :</span><span class="sxs-lookup"><span data-stu-id="89698-128">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="89698-129">AspNetCore.HealthChecks.SqlServer</span><span class="sxs-lookup"><span data-stu-id="89698-129">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="89698-130">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="89698-130">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="89698-131">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) n’est pas géré ou pris en charge par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="89698-131">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="89698-132">Un autre scénario de contrôle d’intégrité montre comment filtrer des contrôles d’intégrité selon un port de gestion.</span><span class="sxs-lookup"><span data-stu-id="89698-132">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="89698-133">L’exemple d’application vous oblige à créer un fichier *Properties/launchSettings.json* comprenant l’URL de gestion et le port de gestion.</span><span class="sxs-lookup"><span data-stu-id="89698-133">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="89698-134">Pour plus d’informations, consultez la section [Filtrer par port](#filter-by-port).</span><span class="sxs-lookup"><span data-stu-id="89698-134">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="89698-135">Sondage d’intégrité de base</span><span class="sxs-lookup"><span data-stu-id="89698-135">Basic health probe</span></span>

<span data-ttu-id="89698-136">Pour de nombreuses applications, un sondage d’intégrité de base qui signale la disponibilité d’une application pour le traitement des requêtes (*liveness*) suffit à découvrir l’état de l’application.</span><span class="sxs-lookup"><span data-stu-id="89698-136">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="89698-137">La configuration de base inscrit les services de contrôle d’intégrité et appelle l’intergiciel (middleware) des contrôles d’intégrité pour répondre à un point de terminaison d’URL avec une réponse d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="89698-137">The basic configuration registers health check services and calls the Health Checks Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="89698-138">Par défaut, aucun contrôle d’intégrité n’est inscrit pour tester les dépendances ou le sous-système.</span><span class="sxs-lookup"><span data-stu-id="89698-138">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="89698-139">L’application est considérée comme saine si elle est capable de répondre à l’URL de point de terminaison de contrôle d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="89698-139">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="89698-140">L’enregistreur de réponse par défaut écrit l’état (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) sous forme de texte en clair qu’il renvoie au client, indiquant si l’état est [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) ou [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus).</span><span class="sxs-lookup"><span data-stu-id="89698-140">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="89698-141">Pour inscrire les services de contrôle d’intégrité, utilisez <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="89698-141">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="89698-142">Créez un point de terminaison de contrôle `MapHealthChecks` d' `Startup.Configure`intégrité en appelant dans.</span><span class="sxs-lookup"><span data-stu-id="89698-142">Create a health check endpoint by calling `MapHealthChecks` in `Startup.Configure`.</span></span>

<span data-ttu-id="89698-143">Dans l’exemple d’application, le point de terminaison de contrôle d’intégrité est créé au niveau de `/health` (*BasicStartup.cs*) :</span><span class="sxs-lookup"><span data-stu-id="89698-143">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

```csharp
public class BasicStartup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddHealthChecks();
    }

    public void Configure(IApplicationBuilder app)
    {
        app.UseRouting();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapHealthChecks("/health");
        });
    }
}
```

<span data-ttu-id="89698-144">Pour exécuter le scénario de configuration de base à l’aide de l’exemple d’application, exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="89698-144">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="89698-145">Exemple Docker</span><span class="sxs-lookup"><span data-stu-id="89698-145">Docker example</span></span>

<span data-ttu-id="89698-146">[Docker](xref:host-and-deploy/docker/index) fournit une directive `HEALTHCHECK` intégrée qui peut être utilisée pour vérifier l’état d’une application utilisant la configuration de contrôle d’intégrité de base :</span><span class="sxs-lookup"><span data-stu-id="89698-146">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="89698-147">Créer des contrôles d’intégrité</span><span class="sxs-lookup"><span data-stu-id="89698-147">Create health checks</span></span>

<span data-ttu-id="89698-148">Les contrôles d’intégrité sont créés via l’implémentation de l’interface <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck>.</span><span class="sxs-lookup"><span data-stu-id="89698-148">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="89698-149">La méthode <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> retourne un <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> qui indique l’état d’intégrité comme étant `Healthy`, `Degraded` ou `Unhealthy`.</span><span class="sxs-lookup"><span data-stu-id="89698-149">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="89698-150">Le résultat est écrit sous la forme de texte en clair avec un code d’état configurable (la configuration est décrite dans la section [Options de contrôle d’intégrité](#health-check-options)).</span><span class="sxs-lookup"><span data-stu-id="89698-150">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="89698-151"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> peut également retourner des paires clé-valeur facultatives.</span><span class="sxs-lookup"><span data-stu-id="89698-151"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

<span data-ttu-id="89698-152">La classe `ExampleHealthCheck` suivante illustre la disposition d’un contrôle d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="89698-152">The following `ExampleHealthCheck` class demonstrates the layout of a health check.</span></span> <span data-ttu-id="89698-153">La logique des contrôles d’intégrité est placée `CheckHealthAsync` dans la méthode.</span><span class="sxs-lookup"><span data-stu-id="89698-153">The health checks logic is placed in the `CheckHealthAsync` method.</span></span> <span data-ttu-id="89698-154">L’exemple suivant définit une variable factice, `healthCheckResultHealthy`, sur `true`.</span><span class="sxs-lookup"><span data-stu-id="89698-154">The following example sets a dummy variable, `healthCheckResultHealthy`, to `true`.</span></span> <span data-ttu-id="89698-155">Si la valeur de `healthCheckResultHealthy` est définie sur `false`, l’état [HealthCheckResult. inhealth](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) est retourné.</span><span class="sxs-lookup"><span data-stu-id="89698-155">If the value of `healthCheckResultHealthy` is set to `false`, the [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) status is returned.</span></span>

```csharp
public class ExampleHealthCheck : IHealthCheck
{
    public Task<HealthCheckResult> CheckHealthAsync(
        HealthCheckContext context,
        CancellationToken cancellationToken = default(CancellationToken))
    {
        var healthCheckResultHealthy = true;

        if (healthCheckResultHealthy)
        {
            return Task.FromResult(
                HealthCheckResult.Healthy("A healthy result."));
        }

        return Task.FromResult(
            HealthCheckResult.Unhealthy("An unhealthy result."));
    }
}
```

## <a name="register-health-check-services"></a><span data-ttu-id="89698-156">Inscrire les services de contrôle d’intégrité</span><span class="sxs-lookup"><span data-stu-id="89698-156">Register health check services</span></span>

<span data-ttu-id="89698-157">Le `ExampleHealthCheck` type est ajouté aux services de contrôle d' <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> intégrité `Startup.ConfigureServices`avec dans :</span><span class="sxs-lookup"><span data-stu-id="89698-157">The `ExampleHealthCheck` type is added to health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

<span data-ttu-id="89698-158">La surcharge <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> de l’exemple suivant définit l’état d’échec (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) de manière à être signalé lorsque le contrôle d’intégrité signale un échec.</span><span class="sxs-lookup"><span data-stu-id="89698-158">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="89698-159">Si l’état d’échec est défini sur `null` (par défaut), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) est signalé.</span><span class="sxs-lookup"><span data-stu-id="89698-159">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="89698-160">Cette surcharge est utile pour les créateurs de bibliothèque, dans les cas où l’état d’échec indiqué par la bibliothèque est appliqué par l’application lorsqu’un échec est signalé par le contrôle d’intégrité, si l’implémentation de ce dernier respecte le paramètre.</span><span class="sxs-lookup"><span data-stu-id="89698-160">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="89698-161">Des *étiquettes* peuvent être utilisées pour filtrer les contrôles d’intégrité (cette procédure est décrite plus loin dans la section [Filtrer les contrôles d’intégrité](#filter-health-checks)).</span><span class="sxs-lookup"><span data-stu-id="89698-161">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="89698-162"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> peut également exécuter une fonction lambda.</span><span class="sxs-lookup"><span data-stu-id="89698-162"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="89698-163">Dans l’exemple suivant, le nom du contrôle d’intégrité est spécifié en tant que `Example`, et la vérification retourne toujours un état sain :</span><span class="sxs-lookup"><span data-stu-id="89698-163">In the following example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

## <a name="use-health-checks-routing"></a><span data-ttu-id="89698-164">Utiliser le routage des contrôles d’intégrité</span><span class="sxs-lookup"><span data-stu-id="89698-164">Use Health Checks Routing</span></span>

<span data-ttu-id="89698-165">Dans `Startup.Configure`, appelez `MapHealthChecks` sur le générateur de points de terminaison avec l’URL de point de terminaison ou le chemin d’accès relatif :</span><span class="sxs-lookup"><span data-stu-id="89698-165">In `Startup.Configure`, call `MapHealthChecks` on the endpoint builder with the endpoint URL or relative path:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

### <a name="require-host"></a><span data-ttu-id="89698-166">Exiger un hôte</span><span class="sxs-lookup"><span data-stu-id="89698-166">Require host</span></span>

<span data-ttu-id="89698-167">Appelez `RequireHost` pour spécifier un ou plusieurs hôtes autorisés pour le point de terminaison de contrôle d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="89698-167">Call `RequireHost` to specify one or more permitted hosts for the health check endpoint.</span></span> <span data-ttu-id="89698-168">Les hôtes doivent être au format Unicode plutôt que Punycode et peuvent inclure un port.</span><span class="sxs-lookup"><span data-stu-id="89698-168">Hosts should be Unicode rather than punycode and may include a port.</span></span> <span data-ttu-id="89698-169">Si une collection n’est pas fournie, tous les hôtes sont acceptés.</span><span class="sxs-lookup"><span data-stu-id="89698-169">If a collection isn't supplied, any host is accepted.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireHost("www.contoso.com:5001");
});
```

<span data-ttu-id="89698-170">Pour plus d’informations, consultez la section [Filtrer par port](#filter-by-port).</span><span class="sxs-lookup"><span data-stu-id="89698-170">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

### <a name="require-authorization"></a><span data-ttu-id="89698-171">Exiger une autorisation</span><span class="sxs-lookup"><span data-stu-id="89698-171">Require authorization</span></span>

<span data-ttu-id="89698-172">Appelez `RequireAuthorization` pour exécuter l’intergiciel (middleware) d’autorisation sur le point de terminaison de demande de contrôle d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="89698-172">Call `RequireAuthorization` to run Authorization Middleware on the health check request endpoint.</span></span> <span data-ttu-id="89698-173">Une `RequireAuthorization` surcharge accepte une ou plusieurs stratégies d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="89698-173">A `RequireAuthorization` overload accepts one or more authorization policies.</span></span> <span data-ttu-id="89698-174">Si aucune stratégie n’est fournie, la stratégie d’autorisation par défaut est utilisée.</span><span class="sxs-lookup"><span data-stu-id="89698-174">If a policy isn't provided, the default authorization policy is used.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireAuthorization();
});
```

### <a name="enable-cross-origin-requests-cors"></a><span data-ttu-id="89698-175">Activer les requêtes d’origines différentes</span><span class="sxs-lookup"><span data-stu-id="89698-175">Enable Cross-Origin Requests (CORS)</span></span>

<span data-ttu-id="89698-176">Bien que l’exécution manuelle de contrôles d’intégrité à partir d’un navigateur ne soit pas un scénario d’utilisation courant `RequireCors` , l’intergiciel (middleware) cors peut être activé en appelant sur des points de terminaison de contrôle d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="89698-176">Although performing health checks manually from a browser isn't a common use scenario, CORS Middleware can be enabled by calling `RequireCors` on health checks endpoints.</span></span> <span data-ttu-id="89698-177">Une `RequireCors` surcharge accepte un délégué de générateur de stratégie`CorsPolicyBuilder`cors () ou un nom de stratégie.</span><span class="sxs-lookup"><span data-stu-id="89698-177">A `RequireCors` overload accepts a CORS policy builder delegate (`CorsPolicyBuilder`) or a policy name.</span></span> <span data-ttu-id="89698-178">Si aucune stratégie n’est fournie, la stratégie CORS par défaut est utilisée.</span><span class="sxs-lookup"><span data-stu-id="89698-178">If a policy isn't provided, the default CORS policy is used.</span></span> <span data-ttu-id="89698-179">Pour plus d'informations, consultez <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="89698-179">For more information, see <xref:security/cors>.</span></span>

## <a name="health-check-options"></a><span data-ttu-id="89698-180">Options de contrôle d’intégrité</span><span class="sxs-lookup"><span data-stu-id="89698-180">Health check options</span></span>

<span data-ttu-id="89698-181"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> permet de personnaliser le comportement du contrôle d’intégrité :</span><span class="sxs-lookup"><span data-stu-id="89698-181"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="89698-182">Filtrer les contrôles d’intégrité</span><span class="sxs-lookup"><span data-stu-id="89698-182">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="89698-183">Personnaliser le code d’état HTTP</span><span class="sxs-lookup"><span data-stu-id="89698-183">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="89698-184">Supprimer les en-têtes de cache</span><span class="sxs-lookup"><span data-stu-id="89698-184">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="89698-185">Personnaliser la sortie</span><span class="sxs-lookup"><span data-stu-id="89698-185">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="89698-186">Filtrer les contrôles d’intégrité</span><span class="sxs-lookup"><span data-stu-id="89698-186">Filter health checks</span></span>

<span data-ttu-id="89698-187">Par défaut, l’intergiciel (middleware) des contrôles d’intégrité exécute toutes les vérifications d’intégrité inscrites.</span><span class="sxs-lookup"><span data-stu-id="89698-187">By default, Health Checks Middleware runs all registered health checks.</span></span> <span data-ttu-id="89698-188">Pour exécuter un sous-ensemble de contrôles d’intégrité, fournissez une fonction qui retourne une valeur booléenne à l’option <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate>.</span><span class="sxs-lookup"><span data-stu-id="89698-188">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="89698-189">Dans l’exemple suivant, le contrôle d’intégrité `Bar` est filtré en fonction de son étiquette (`bar_tag`) dans l’instruction conditionnelle de la fonction, où `true` est retourné uniquement si la propriété <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> du contrôle d’intégrité correspond à `foo_tag` ou à `baz_tag` :</span><span class="sxs-lookup"><span data-stu-id="89698-189">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

<span data-ttu-id="89698-190">Dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="89698-190">In `Startup.ConfigureServices`:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Foo", () =>
        HealthCheckResult.Healthy("Foo is OK!"), tags: new[] { "foo_tag" })
    .AddCheck("Bar", () =>
        HealthCheckResult.Unhealthy("Bar is unhealthy!"), tags: new[] { "bar_tag" })
    .AddCheck("Baz", () =>
        HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
```

<span data-ttu-id="89698-191">Dans `Startup.Configure`, le `Predicate` filtre le contrôle d’intégrité « bar ».</span><span class="sxs-lookup"><span data-stu-id="89698-191">In `Startup.Configure`, the `Predicate` filters out the 'Bar' health check.</span></span> <span data-ttu-id="89698-192">Seules les opérations foo et Baz sont exécutées.</span><span class="sxs-lookup"><span data-stu-id="89698-192">Only Foo and Baz execute.:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("foo_tag") ||
            check.Tags.Contains("baz_tag")
    });
});
```

### <a name="customize-the-http-status-code"></a><span data-ttu-id="89698-193">Personnaliser le code d’état HTTP</span><span class="sxs-lookup"><span data-stu-id="89698-193">Customize the HTTP status code</span></span>

<span data-ttu-id="89698-194">Utilisez <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> pour personnaliser le mappage de l’état d’intégrité aux codes d’état HTTP.</span><span class="sxs-lookup"><span data-stu-id="89698-194">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="89698-195">Les affectations <xref:Microsoft.AspNetCore.Http.StatusCodes> suivantes sont les valeurs par défaut utilisées par le middleware.</span><span class="sxs-lookup"><span data-stu-id="89698-195">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="89698-196">Vous pouvez modifier les valeurs de code d’état selon vos besoins.</span><span class="sxs-lookup"><span data-stu-id="89698-196">Change the status code values to meet your requirements.</span></span>

<span data-ttu-id="89698-197">Dans `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="89698-197">In `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResultStatusCodes =
        {
            [HealthStatus.Healthy] = StatusCodes.Status200OK,
            [HealthStatus.Degraded] = StatusCodes.Status200OK,
            [HealthStatus.Unhealthy] = StatusCodes.Status503ServiceUnavailable
        }
    });
});
```

### <a name="suppress-cache-headers"></a><span data-ttu-id="89698-198">Supprimer les en-têtes de cache</span><span class="sxs-lookup"><span data-stu-id="89698-198">Suppress cache headers</span></span>

<span data-ttu-id="89698-199"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses>contrôle si l’intergiciel (middleware) de contrôles d’intégrité ajoute des en-têtes HTTP à une réponse de sondage pour empêcher la mise en cache des réponses.</span><span class="sxs-lookup"><span data-stu-id="89698-199"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Checks Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="89698-200">Si la valeur est `false` (par défaut), le middleware définit ou substitue les en-têtes `Cache-Control`, `Expires` et `Pragma` afin d’empêcher la mise en cache de la réponse.</span><span class="sxs-lookup"><span data-stu-id="89698-200">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="89698-201">Si la valeur est `true`, le middleware ne modifie pas les en-têtes de cache de la réponse.</span><span class="sxs-lookup"><span data-stu-id="89698-201">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

<span data-ttu-id="89698-202">Dans `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="89698-202">In `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        AllowCachingResponses = false
    });
});
```

### <a name="customize-output"></a><span data-ttu-id="89698-203">Personnaliser la sortie</span><span class="sxs-lookup"><span data-stu-id="89698-203">Customize output</span></span>

<span data-ttu-id="89698-204">L’option <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> obtient ou définit un délégué utilisé pour écrire la réponse.</span><span class="sxs-lookup"><span data-stu-id="89698-204">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span>

<span data-ttu-id="89698-205">Dans `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="89698-205">In `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
});
```

<span data-ttu-id="89698-206">Le délégué par défaut écrit une réponse minimale constituée de texte en clair, avec la valeur de chaîne [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span><span class="sxs-lookup"><span data-stu-id="89698-206">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span> <span data-ttu-id="89698-207">Le délégué personnalisé suivant, `WriteResponse`, génère une réponse JSON personnalisée :</span><span class="sxs-lookup"><span data-stu-id="89698-207">The following custom delegate, `WriteResponse`, outputs a custom JSON response:</span></span>

```csharp
private static Task WriteResponse(HttpContext httpContext, HealthReport result)
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

<span data-ttu-id="89698-208">Le système de contrôle d’intégrité ne fournit pas de prise en charge intégrée des formats de retour JSON complexes, car le format est spécifique à votre choix de système de surveillance.</span><span class="sxs-lookup"><span data-stu-id="89698-208">The health checks system doesn't provide built-in support for complex JSON return formats because the format is specific to your choice of monitoring system.</span></span> <span data-ttu-id="89698-209">N’hésitez pas à `JObject` personnaliser le dans l’exemple précédent si nécessaire pour répondre à vos besoins.</span><span class="sxs-lookup"><span data-stu-id="89698-209">Feel free to customize the `JObject` in the preceding example as necessary to meet your needs.</span></span>

## <a name="database-probe"></a><span data-ttu-id="89698-210">Sondage de base de données</span><span class="sxs-lookup"><span data-stu-id="89698-210">Database probe</span></span>

<span data-ttu-id="89698-211">Un contrôle d’intégrité peut spécifier une requête de base de données à exécuter en tant que test booléen pour indiquer si la base de données répond normalement.</span><span class="sxs-lookup"><span data-stu-id="89698-211">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="89698-212">L’exemple d’application utilise [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), une bibliothèque de vérification d’intégrité pour les applications ASP.NET Core, pour effectuer une vérification d’intégrité sur une base de données SQL Server.</span><span class="sxs-lookup"><span data-stu-id="89698-212">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="89698-213">`AspNetCore.Diagnostics.HealthChecks` exécute une demande `SELECT 1` sur la base de données pour confirmer que la connexion à la base de données est saine.</span><span class="sxs-lookup"><span data-stu-id="89698-213">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="89698-214">Lorsque vous vérifiez une connexion de base de données à l’aide d’une requête, choisissez une requête qui est retournée rapidement.</span><span class="sxs-lookup"><span data-stu-id="89698-214">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="89698-215">L’utilisation d’une requête risque néanmoins d’entraîner une surcharge de la base de données et d’en diminuer les performances.</span><span class="sxs-lookup"><span data-stu-id="89698-215">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="89698-216">Dans la plupart des cas, il n’est pas nécessaire d’utiliser une requête de test.</span><span class="sxs-lookup"><span data-stu-id="89698-216">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="89698-217">Il suffit simplement d’établir une connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="89698-217">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="89698-218">Si vous avez besoin d’exécuter une requête, choisissez une requête SELECT simple, telle que `SELECT 1`.</span><span class="sxs-lookup"><span data-stu-id="89698-218">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="89698-219">Inclure une référence de package à [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="89698-219">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="89698-220">Fournissez une chaîne de connexion de base de données valide dans le fichier *appsettings.json* de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="89698-220">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="89698-221">L’application utilise une base de données SQL Server nommée `HealthCheckSample` :</span><span class="sxs-lookup"><span data-stu-id="89698-221">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/3.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="89698-222">Pour inscrire les services de contrôle d’intégrité, utilisez <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="89698-222">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="89698-223">L’exemple d’application appelle la méthode `AddSqlServer` avec la chaîne de connexion de la base de données (*DbHealthStartup.cs*) :</span><span class="sxs-lookup"><span data-stu-id="89698-223">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="89698-224">Pour créer un point de terminaison de contrôle `MapHealthChecks` d' `Startup.Configure`intégrité, appelez dans :</span><span class="sxs-lookup"><span data-stu-id="89698-224">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

<span data-ttu-id="89698-225">Pour exécuter le scénario de sondage d’une base de données à l’aide de l’exemple d’application, exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="89698-225">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="89698-226">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) n’est pas géré ou pris en charge par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="89698-226">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="89698-227">Sondage DbContext Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="89698-227">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="89698-228">La vérification `DbContext` vérifie que l’application peut communiquer avec la base de données configurée pour un `DbContext` EF Core.</span><span class="sxs-lookup"><span data-stu-id="89698-228">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="89698-229">La vérification `DbContext` est prise en charge dans les applications qui :</span><span class="sxs-lookup"><span data-stu-id="89698-229">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="89698-230">Utilisent [Entity Framework (EF) Core](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="89698-230">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="89698-231">Incluent une référence de package à [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span><span class="sxs-lookup"><span data-stu-id="89698-231">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="89698-232">`AddDbContextCheck<TContext>` inscrit un contrôle d’intégrité pour un `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="89698-232">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="89698-233">Le `DbContext` est fourni en tant que `TContext` à la méthode.</span><span class="sxs-lookup"><span data-stu-id="89698-233">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="89698-234">Une surcharge est disponible pour configurer l’état d’échec, les étiquettes et une requête de test personnalisée.</span><span class="sxs-lookup"><span data-stu-id="89698-234">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="89698-235">Par défaut :</span><span class="sxs-lookup"><span data-stu-id="89698-235">By default:</span></span>

* <span data-ttu-id="89698-236">`DbContextHealthCheck` appelle la méthode `CanConnectAsync` d’EF Core.</span><span class="sxs-lookup"><span data-stu-id="89698-236">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="89698-237">Vous pouvez choisir quelle opération doit être exécutée lors du contrôle d’intégrité à l’aide des surcharges de la méthode `AddDbContextCheck`.</span><span class="sxs-lookup"><span data-stu-id="89698-237">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="89698-238">Le nom du contrôle d’intégrité correspond à celui du type `TContext`.</span><span class="sxs-lookup"><span data-stu-id="89698-238">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="89698-239">Dans l’exemple d’application `AppDbContext` , est fourni `AddDbContextCheck` à et enregistré en tant que `Startup.ConfigureServices` service dans (*DbContextHealthStartup.cs*) :</span><span class="sxs-lookup"><span data-stu-id="89698-239">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="89698-240">Pour créer un point de terminaison de contrôle `MapHealthChecks` d' `Startup.Configure`intégrité, appelez dans :</span><span class="sxs-lookup"><span data-stu-id="89698-240">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

<span data-ttu-id="89698-241">Pour exécuter le scénario de sondage `DbContext` à l’aide de l’exemple d’application, vérifiez que la base de données spécifiée par le la chaîne de connexion ne se trouve pas déjà dans l’instance SQL Server.</span><span class="sxs-lookup"><span data-stu-id="89698-241">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="89698-242">Si la base de données existe, supprimez-la.</span><span class="sxs-lookup"><span data-stu-id="89698-242">If the database exists, delete it.</span></span>

<span data-ttu-id="89698-243">Exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="89698-243">Execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario dbcontext
```

<span data-ttu-id="89698-244">Une fois que l’application est en cours d’exécution, vérifiez l’état d’intégrité en envoyant une requête au point de terminaison `/health` dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="89698-244">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="89698-245">La base de données et `AppDbContext` n’existent pas, donc l’application fournit la réponse suivante :</span><span class="sxs-lookup"><span data-stu-id="89698-245">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="89698-246">Déclenchez l’exemple d’application pour créer la base de données.</span><span class="sxs-lookup"><span data-stu-id="89698-246">Trigger the sample app to create the database.</span></span> <span data-ttu-id="89698-247">Envoyez une requête à `/createdatabase`.</span><span class="sxs-lookup"><span data-stu-id="89698-247">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="89698-248">L’application répond :</span><span class="sxs-lookup"><span data-stu-id="89698-248">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="89698-249">Envoyez une requête au point de terminaison `/health`.</span><span class="sxs-lookup"><span data-stu-id="89698-249">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="89698-250">La base de données et le contexte existent, donc l’application répond :</span><span class="sxs-lookup"><span data-stu-id="89698-250">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="89698-251">Déclenchez l’exemple d’application pour supprimer la base de données.</span><span class="sxs-lookup"><span data-stu-id="89698-251">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="89698-252">Envoyez une requête à `/deletedatabase`.</span><span class="sxs-lookup"><span data-stu-id="89698-252">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="89698-253">L’application répond :</span><span class="sxs-lookup"><span data-stu-id="89698-253">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="89698-254">Envoyez une requête au point de terminaison `/health`.</span><span class="sxs-lookup"><span data-stu-id="89698-254">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="89698-255">L’application fournit une réponse non intégrité :</span><span class="sxs-lookup"><span data-stu-id="89698-255">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="89698-256">Séparer les sondages probe readiness et probe liveness</span><span class="sxs-lookup"><span data-stu-id="89698-256">Separate readiness and liveness probes</span></span>

<span data-ttu-id="89698-257">Dans certains scénarios d’hébergement, une paire de contrôles d’intégrité est utilisée pour distinguer deux états d’application :</span><span class="sxs-lookup"><span data-stu-id="89698-257">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="89698-258">L’application fonctionne mais n’est pas encore prête à recevoir des requêtes.</span><span class="sxs-lookup"><span data-stu-id="89698-258">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="89698-259">Il s’agit de l’état de *readiness* qui indique que l’application est prête.</span><span class="sxs-lookup"><span data-stu-id="89698-259">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="89698-260">L’application fonctionne et répond aux requêtes.</span><span class="sxs-lookup"><span data-stu-id="89698-260">The app is functioning and responding to requests.</span></span> <span data-ttu-id="89698-261">Il s’agit de l’état de *liveness* qui indique que l’application est active.</span><span class="sxs-lookup"><span data-stu-id="89698-261">This state is the app's *liveness*.</span></span>

<span data-ttu-id="89698-262">En général, la vérification qui permet de savoir si une application est prête implique un ensemble de vérifications plus complet permettant de déterminer si tous les sous-systèmes et toutes les ressources de l’application sont disponibles, ce qui est un processus assez long.</span><span class="sxs-lookup"><span data-stu-id="89698-262">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="89698-263">La vérification qui permet de savoir si une application est active est simple et rapide, et vise à déterminer si l’application est disponible pour traiter les requêtes.</span><span class="sxs-lookup"><span data-stu-id="89698-263">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="89698-264">Une fois que l’application est considérée comme prête, il est inutile de recommencer le test. En effet, le seul test nécessaire ensuite est celui visant à vérifier que l’application est active.</span><span class="sxs-lookup"><span data-stu-id="89698-264">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="89698-265">L’exemple d’application contient un contrôle d’intégrité permettant de signaler l’achèvement d’une tâche de démarrage de longue durée dans un [service hébergé](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="89698-265">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="89698-266">`StartupHostedServiceHealthCheck` expose la propriété `StartupTaskCompleted`, que le service hébergé peut définir sur `true` lorsque sa tâche de longue durée est terminée (*StartupHostedServiceHealthCheck.cs*) :</span><span class="sxs-lookup"><span data-stu-id="89698-266">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="89698-267">La tâche de longue durée en arrière-plan est démarrée par un [service hébergé](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span><span class="sxs-lookup"><span data-stu-id="89698-267">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="89698-268">À la fin de la tâche, `StartupHostedServiceHealthCheck.StartupTaskCompleted` est défini sur `true` :</span><span class="sxs-lookup"><span data-stu-id="89698-268">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="89698-269">Le contrôle d’intégrité est inscrit auprès de <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> dans `Startup.ConfigureServices` en même temps que le service hébergé.</span><span class="sxs-lookup"><span data-stu-id="89698-269">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="89698-270">Étant donné que le service hébergé doit définir la propriété sur le contrôle d’intégrité, le contrôle d’intégrité est également inscrit dans le conteneur du service (*LivenessProbeStartup.cs*) :</span><span class="sxs-lookup"><span data-stu-id="89698-270">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="89698-271">Un point de terminaison de contrôle d’intégrité `MapHealthChecks` est `Startup.Configure`créé en appelant dans.</span><span class="sxs-lookup"><span data-stu-id="89698-271">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`.</span></span> <span data-ttu-id="89698-272">Dans l’exemple d’application, les points de terminaison de contrôle d’intégrité sont créés à l’adresse :</span><span class="sxs-lookup"><span data-stu-id="89698-272">In the sample app, the health check endpoints are created at:</span></span>

* <span data-ttu-id="89698-273">`/health/ready`pour la vérification de la disponibilité.</span><span class="sxs-lookup"><span data-stu-id="89698-273">`/health/ready` for the readiness check.</span></span> <span data-ttu-id="89698-274">Le test qui permet de vérifier si l’application est prête filtre les contrôles d’intégrité pour n’afficher que celui dont l’étiquette est `ready`.</span><span class="sxs-lookup"><span data-stu-id="89698-274">The readiness check filters health checks to the health check with the `ready` tag.</span></span>
* <span data-ttu-id="89698-275">`/health/live`pour la vérification de l’activité.</span><span class="sxs-lookup"><span data-stu-id="89698-275">`/health/live` for the liveness check.</span></span> <span data-ttu-id="89698-276">La vérification d' `StartupHostedServiceHealthCheck` activité filtre le en retournant `false` dans le [prédicat HealthCheckOptions.](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (pour plus d’informations, consultez [contrôles d’intégrité de filtre](#filter-health-checks)).</span><span class="sxs-lookup"><span data-stu-id="89698-276">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks))</span></span>

<span data-ttu-id="89698-277">Dans l’exemple de code suivant :</span><span class="sxs-lookup"><span data-stu-id="89698-277">In the following example code:</span></span>

* <span data-ttu-id="89698-278">La vérification de disponibilité utilise toutes les vérifications enregistrées avec la balise « Ready ».</span><span class="sxs-lookup"><span data-stu-id="89698-278">The readiness check uses all registered checks with the 'ready' tag.</span></span>
* <span data-ttu-id="89698-279">Le `Predicate` exclut toutes les vérifications et retourne un 200-OK.</span><span class="sxs-lookup"><span data-stu-id="89698-279">The `Predicate` excludes all checks and return a 200-Ok.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health/ready", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("ready"),
    });

    endpoints.MapHealthChecks("/health/live", new HealthCheckOptions()
    {
        Predicate = (_) => false
    });
}
```

<span data-ttu-id="89698-280">Pour exécuter le scénario de configuration readiness/liveness à l’aide de l’exemple d’application, exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="89698-280">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario liveness
```

<span data-ttu-id="89698-281">Dans un navigateur, accédez à `/health/ready` plusieurs fois jusqu’à ce que 15 secondes soient écoulées.</span><span class="sxs-lookup"><span data-stu-id="89698-281">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="89698-282">Le contrôle d’intégrité signale *Unhealthy* pendant les 15 premières secondes.</span><span class="sxs-lookup"><span data-stu-id="89698-282">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="89698-283">Après 15 secondes, le point de terminaison signale *Healthy*, ce qui reflète l’achèvement de la tâche de longue durée exécutée par le service hébergé.</span><span class="sxs-lookup"><span data-stu-id="89698-283">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="89698-284">Cet exemple crée également un éditeur de vérification de l’intégrité (implémentation <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>) qui exécute la première vérification de disponibilité avec un délai de deux secondes.</span><span class="sxs-lookup"><span data-stu-id="89698-284">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="89698-285">Pour plus d’informations, consultez la section [Éditeur de vérification de l’intégrité](#health-check-publisher).</span><span class="sxs-lookup"><span data-stu-id="89698-285">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="89698-286">Exemple Kubernetes</span><span class="sxs-lookup"><span data-stu-id="89698-286">Kubernetes example</span></span>

<span data-ttu-id="89698-287">Dans un environnement tel que [Kubernetes](https://kubernetes.io/), il peut être utile d’utiliser séparément le test permettant de savoir si la l’application est prête et celui visant à savoir si l’application est active.</span><span class="sxs-lookup"><span data-stu-id="89698-287">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="89698-288">Dans Kubernetes, une application peut devoir effectuer une tâche de démarrage de longue durée avant d’accepter des requêtes, telles qu’un test de la disponibilité de la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="89698-288">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="89698-289">L’utilisation séparée des deux tests permet à l’orchestrateur de faire la distinction entre une application qui fonctionne mais qui n’est pas encore prête, et une application qui n’a pas pu démarrer.</span><span class="sxs-lookup"><span data-stu-id="89698-289">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="89698-290">Pour plus d’informations sur les sondages probe readiness et probe liveness dans Kubernetes, consultez [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) dans la documentation Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="89698-290">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="89698-291">L’exemple suivant montre une configuration probe readiness Kubernetes :</span><span class="sxs-lookup"><span data-stu-id="89698-291">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="89698-292">Sonde basée sur les métriques avec un enregistreur de réponse personnalisé</span><span class="sxs-lookup"><span data-stu-id="89698-292">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="89698-293">L’exemple d’application montre un contrôle d’intégrité de mémoire avec un enregistreur de réponse personnalisé.</span><span class="sxs-lookup"><span data-stu-id="89698-293">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="89698-294">`MemoryHealthCheck` signale un état détérioré si l’application utilise plus de mémoire que le seuil défini (1 Go dans l’exemple d’application).</span><span class="sxs-lookup"><span data-stu-id="89698-294">`MemoryHealthCheck` reports a degraded status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="89698-295"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> inclut des informations de récupérateur de mémoire pour l’application (*MemoryHealthCheck.cs*) :</span><span class="sxs-lookup"><span data-stu-id="89698-295">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="89698-296">Pour inscrire les services de contrôle d’intégrité, utilisez <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="89698-296">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="89698-297">Au lieu d’activer le contrôle d’intégrité en le passant à <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, `MemoryHealthCheck` est inscrit en tant que service.</span><span class="sxs-lookup"><span data-stu-id="89698-297">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="89698-298">Tous les services <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> inscrits sont disponibles pour les services de contrôle d’intégrité et le middleware.</span><span class="sxs-lookup"><span data-stu-id="89698-298">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="89698-299">Nous vous recommandons d’inscrire les services de contrôle d’intégrité en tant que services Singleton.</span><span class="sxs-lookup"><span data-stu-id="89698-299">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="89698-300">Dans l’exemple d’application (*CustomWriterStartup.cs*) :</span><span class="sxs-lookup"><span data-stu-id="89698-300">In the sample app (*CustomWriterStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="89698-301">Un point de terminaison de contrôle d’intégrité `MapHealthChecks` est `Startup.Configure`créé en appelant dans.</span><span class="sxs-lookup"><span data-stu-id="89698-301">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`.</span></span> <span data-ttu-id="89698-302">Un délégué `WriteResponse` est fourni à la propriété `ResponseWriter` pour produire une réponse JSON personnalisée lorsque le contrôle d’intégrité est exécuté :</span><span class="sxs-lookup"><span data-stu-id="89698-302">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
}
```

<span data-ttu-id="89698-303">La méthode `WriteResponse` formate le `CompositeHealthCheckResult` en un objet JSON et génère une sortie JSON pour la réponse du contrôle d’intégrité :</span><span class="sxs-lookup"><span data-stu-id="89698-303">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="89698-304">Pour exécuter le sondage basé sur les métriques avec l’enregistreur de réponse personnalisé à l’aide de l’exemple d’application, exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="89698-304">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="89698-305">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) inclut des scénarios de vérification de l’intégrité basés sur des métriques, notamment des vérifications du stockage sur disque et de la vivacité maximale des valeurs.</span><span class="sxs-lookup"><span data-stu-id="89698-305">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="89698-306">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) n’est pas géré ou pris en charge par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="89698-306">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="89698-307">Filtrer par port</span><span class="sxs-lookup"><span data-stu-id="89698-307">Filter by port</span></span>

<span data-ttu-id="89698-308">Appelez `RequireHost` avec un modèle d’URL qui spécifie un port pour restreindre les demandes de contrôle d’intégrité au port spécifié. `MapHealthChecks`</span><span class="sxs-lookup"><span data-stu-id="89698-308">Call `RequireHost` on `MapHealthChecks` with a URL pattern that specifies a port to restrict health check requests to the port specified.</span></span> <span data-ttu-id="89698-309">Ceci est généralement utilisé dans les environnements de conteneurs pour exposer un port aux services de supervision.</span><span class="sxs-lookup"><span data-stu-id="89698-309">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="89698-310">L’exemple d’application configure le port à l’aide du [fournisseur de configuration des variables d’environnement](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="89698-310">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="89698-311">Le port est défini dans le fichier *launchSettings.json*, puis transmis au fournisseur de configuration via une variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="89698-311">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="89698-312">Vous devez également configurer le serveur pour écouter les requêtes qui arrivent au port de gestion.</span><span class="sxs-lookup"><span data-stu-id="89698-312">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="89698-313">Pour utiliser l’exemple d’application dans le but d’illustrer la configuration du port de gestion, créez le fichier *launchSettings.json* dans un dossier *Propriétés*.</span><span class="sxs-lookup"><span data-stu-id="89698-313">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="89698-314">Le fichier *Properties/launchSettings. JSON* suivant dans l’exemple d’application n’est pas inclus dans les fichiers projet de l’exemple d’application et doit être créé manuellement :</span><span class="sxs-lookup"><span data-stu-id="89698-314">The following *Properties/launchSettings.json* file in the sample app isn't included in the sample app's project files and must be created manually:</span></span>

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

<span data-ttu-id="89698-315">Pour inscrire les services de contrôle d’intégrité, utilisez <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="89698-315">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="89698-316">Créez un point de terminaison de contrôle `MapHealthChecks` d' `Startup.Configure`intégrité en appelant dans.</span><span class="sxs-lookup"><span data-stu-id="89698-316">Create a health check endpoint by calling `MapHealthChecks` in `Startup.Configure`.</span></span>

<span data-ttu-id="89698-317">Dans l’exemple d’application, un appel `RequireHost` à sur le point `Startup.Configure` de terminaison dans spécifie le port de gestion de la configuration :</span><span class="sxs-lookup"><span data-stu-id="89698-317">In the sample app, a call to `RequireHost` on the endpoint in `Startup.Configure` specifies the management port from configuration:</span></span>

```csharp
endpoints.MapHealthChecks("/health")
    .RequireHost($"*:{Configuration["ManagementPort"]}");
```

<span data-ttu-id="89698-318">Les points de terminaison sont créés dans l’exemple `Startup.Configure`d’application dans.</span><span class="sxs-lookup"><span data-stu-id="89698-318">Endpoints are created in the sample app in `Startup.Configure`.</span></span> <span data-ttu-id="89698-319">Dans l’exemple de code suivant :</span><span class="sxs-lookup"><span data-stu-id="89698-319">In the following example code:</span></span>

* <span data-ttu-id="89698-320">La vérification de disponibilité utilise toutes les vérifications enregistrées avec la balise « Ready ».</span><span class="sxs-lookup"><span data-stu-id="89698-320">The readiness check uses all registered checks with the 'ready' tag.</span></span>
* <span data-ttu-id="89698-321">Le `Predicate` exclut toutes les vérifications et retourne un 200-OK.</span><span class="sxs-lookup"><span data-stu-id="89698-321">The `Predicate` excludes all checks and return a 200-Ok.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health/ready", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("ready"),
    });

    endpoints.MapHealthChecks("/health/live", new HealthCheckOptions()
    {
        Predicate = (_) => false
    });
}
```

> [!NOTE]
> <span data-ttu-id="89698-322">Vous pouvez éviter de créer le fichier *launchSettings. JSON* dans l’exemple d’application en définissant le port de gestion explicitement dans le code.</span><span class="sxs-lookup"><span data-stu-id="89698-322">You can avoid creating the *launchSettings.json* file in the sample app by setting the management port explicitly in code.</span></span> <span data-ttu-id="89698-323">Dans *Program.cs* où <xref:Microsoft.Extensions.Hosting.HostBuilder> est créé, ajoutez un appel à <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*> et fournissez le point de terminaison du port de gestion de l’application.</span><span class="sxs-lookup"><span data-stu-id="89698-323">In *Program.cs* where the <xref:Microsoft.Extensions.Hosting.HostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*> and provide the app's management port endpoint.</span></span> <span data-ttu-id="89698-324">Dans `Configure` *ManagementPortStartup.cs*, spécifiez le port de gestion `RequireHost`avec :</span><span class="sxs-lookup"><span data-stu-id="89698-324">In `Configure` of *ManagementPortStartup.cs*, specify the management port with `RequireHost`:</span></span>
>
> <span data-ttu-id="89698-325">*Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="89698-325">*Program.cs*:</span></span>
>
> ```csharp
> return new HostBuilder()
>     .ConfigureWebHostDefaults(webBuilder =>
>     {
>         webBuilder.UseKestrel()
>             .ConfigureKestrel(serverOptions =>
>             {
>                 serverOptions.ListenAnyIP(5001);
>             })
>             .UseStartup(startupType);
>     })
>     .Build();
> ```
>
> <span data-ttu-id="89698-326">*ManagementPortStartup.cs* :</span><span class="sxs-lookup"><span data-stu-id="89698-326">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseEndpoints(endpoints =>
> {
>     endpoints.MapHealthChecks("/health").RequireHost("*:5001");
> });
> ```

<span data-ttu-id="89698-327">Pour exécuter le scénario de configuration du port de gestion à l’aide de l’exemple d’application, exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="89698-327">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="89698-328">Distribuer une bibliothèque de contrôle d’intégrité</span><span class="sxs-lookup"><span data-stu-id="89698-328">Distribute a health check library</span></span>

<span data-ttu-id="89698-329">Pour distribuer une bibliothèque comme un contrôle d’intégrité :</span><span class="sxs-lookup"><span data-stu-id="89698-329">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="89698-330">Écrivez un contrôle d’intégrité qui implémente l’interface <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> comme une classe autonome.</span><span class="sxs-lookup"><span data-stu-id="89698-330">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="89698-331">La classe peut utiliser une [injection de dépendance](xref:fundamentals/dependency-injection), une activation de type et des [options nommées](xref:fundamentals/configuration/options) pour accéder aux données de configuration.</span><span class="sxs-lookup"><span data-stu-id="89698-331">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   <span data-ttu-id="89698-332">Dans la logique des contrôles d' `CheckHealthAsync`intégrité de :</span><span class="sxs-lookup"><span data-stu-id="89698-332">In the health checks logic of `CheckHealthAsync`:</span></span>

   * <span data-ttu-id="89698-333">`data1`et `data2` sont utilisés dans la méthode pour exécuter la logique de contrôle d’intégrité de la sonde.</span><span class="sxs-lookup"><span data-stu-id="89698-333">`data1` and `data2` are used in the method to run the probe's health check logic.</span></span>
   * <span data-ttu-id="89698-334">`AccessViolationException`est géré.</span><span class="sxs-lookup"><span data-stu-id="89698-334">`AccessViolationException` is handled.</span></span>

   <span data-ttu-id="89698-335">Lorsqu’un <xref:System.AccessViolationException> se produit <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> , est retourné avec le <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> pour permettre aux utilisateurs de configurer l’état d’échec des contrôles d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="89698-335">When an <xref:System.AccessViolationException> occurs, the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> is returned with the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> to allow users to configure the health checks failure status.</span></span>

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

1. <span data-ttu-id="89698-336">Écrivez une méthode d’extension avec des paramètres que l’application consommatrice appelle dans sa méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="89698-336">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="89698-337">Dans l’exemple qui suit, prenons la signature de méthode de contrôle d’intégrité suivante :</span><span class="sxs-lookup"><span data-stu-id="89698-337">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="89698-338">La signature précédente indique que `ExampleHealthCheck` nécessite des données supplémentaires pour traiter la logique de sondage du contrôle d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="89698-338">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="89698-339">Les données sont fournies au délégué qui est utilisé pour créer l’instance de contrôle d’intégrité lorsque le contrôle d’intégrité est inscrit avec une méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="89698-339">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="89698-340">Dans l’exemple qui suit, l’appelant spécifie les éléments facultatifs suivants :</span><span class="sxs-lookup"><span data-stu-id="89698-340">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="89698-341">Nom du contrôle d’intégrité (`name`).</span><span class="sxs-lookup"><span data-stu-id="89698-341">health check name (`name`).</span></span> <span data-ttu-id="89698-342">Si `null`, `example_health_check` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="89698-342">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="89698-343">Point de données de chaîne du contrôle d’intégrité (`data1`).</span><span class="sxs-lookup"><span data-stu-id="89698-343">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="89698-344">Point de données Integer du contrôle d’intégrité (`data2`).</span><span class="sxs-lookup"><span data-stu-id="89698-344">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="89698-345">Si `null`, `1` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="89698-345">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="89698-346">État d’échec (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span><span class="sxs-lookup"><span data-stu-id="89698-346">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="89698-347">La valeur par défaut est `null`.</span><span class="sxs-lookup"><span data-stu-id="89698-347">The default is `null`.</span></span> <span data-ttu-id="89698-348">Si `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) est signalé pour un état d’échec.</span><span class="sxs-lookup"><span data-stu-id="89698-348">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="89698-349">Étiquettes (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="89698-349">tags (`IEnumerable<string>`).</span></span>

   ```csharp
   using System.Collections.Generic;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   public static class ExampleHealthCheckBuilderExtensions
   {
       const string DefaultName = "example_health_check";

       public static IHealthChecksBuilder AddExampleHealthCheck(
           this IHealthChecksBuilder builder,
           string name = default,
           string data1,
           int data2 = 1,
           HealthStatus? failureStatus = default,
           IEnumerable<string> tags = default)
       {
           return builder.Add(new HealthCheckRegistration(
               name ?? DefaultName,
               sp => new ExampleHealthCheck(data1, data2),
               failureStatus,
               tags));
       }
   }
   ```

## <a name="health-check-publisher"></a><span data-ttu-id="89698-350">Serveur de publication des contrôles d’intégrité</span><span class="sxs-lookup"><span data-stu-id="89698-350">Health Check Publisher</span></span>

<span data-ttu-id="89698-351">Quand un <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> est ajouté au conteneur du service, le système de contrôle d’intégrité exécute régulièrement vos contrôles d’intégrité et appelle `PublishAsync` avec le résultat.</span><span class="sxs-lookup"><span data-stu-id="89698-351">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="89698-352">C’est utile dans un scénario impliquant un système de supervision basé sur les envois (push) et nécessitant que chaque processus appelle le système de supervision régulièrement afin de déterminer l’état d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="89698-352">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="89698-353">L’interface <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> comprend une seule méthode :</span><span class="sxs-lookup"><span data-stu-id="89698-353">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="89698-354"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> vous autorise à définir :</span><span class="sxs-lookup"><span data-stu-id="89698-354"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="89698-355"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; Le retard initial appliqué après le démarrage de l’application avant d’exécuter les instances <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="89698-355"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="89698-356">Le retard est appliqué à une seule fois au démarrage et ne s’applique pas aux itérations ultérieures.</span><span class="sxs-lookup"><span data-stu-id="89698-356">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="89698-357">La valeur par défaut est de cinq secondes.</span><span class="sxs-lookup"><span data-stu-id="89698-357">The default value is five seconds.</span></span>
* <span data-ttu-id="89698-358"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; La période d’exécution de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="89698-358"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="89698-359">La valeur par défaut est de 30 secondes.</span><span class="sxs-lookup"><span data-stu-id="89698-359">The default value is 30 seconds.</span></span>
* <span data-ttu-id="89698-360"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; Si <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> est `null` (valeur par défaut), le service d’éditeur de vérification de l’intégrité exécute toutes les vérifications d’intégrité inscrites.</span><span class="sxs-lookup"><span data-stu-id="89698-360"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="89698-361">Pour exécuter un sous-ensemble de contrôles d’intégrité, fournissez une fonction qui filtre l’ensemble de vérifications.</span><span class="sxs-lookup"><span data-stu-id="89698-361">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="89698-362">Le prédicat est évalué à chaque période.</span><span class="sxs-lookup"><span data-stu-id="89698-362">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="89698-363"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; Le délai d’attente pour l’exécution des vérifications d’intégrité pour toutes les instances <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="89698-363"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="89698-364">Utilisez <xref:System.Threading.Timeout.InfiniteTimeSpan> pour une exécution sans délai d’attente.</span><span class="sxs-lookup"><span data-stu-id="89698-364">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="89698-365">La valeur par défaut est de 30 secondes.</span><span class="sxs-lookup"><span data-stu-id="89698-365">The default value is 30 seconds.</span></span>

<span data-ttu-id="89698-366">Dans l’exemple d’application, `ReadinessPublisher` est une implémentation <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="89698-366">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="89698-367">L’état du contrôle d’intégrité est journalisé pour chaque vérification au niveau du journal :</span><span class="sxs-lookup"><span data-stu-id="89698-367">The health check status is logged for each check at a log level of:</span></span>

* <span data-ttu-id="89698-368">Information (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) si l’état des contrôles d' <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>intégrité est.</span><span class="sxs-lookup"><span data-stu-id="89698-368">Information (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) if the health checks status is <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span></span>
* <span data-ttu-id="89698-369">Error (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) si l’état <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> est ou <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span><span class="sxs-lookup"><span data-stu-id="89698-369">Error (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) if the status is either <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> or <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=18-27)]

<span data-ttu-id="89698-370">Dans l’exemple d’application `LivenessProbeStartup`, la vérification de la disponibilité `StartupHostedService` a un délai de démarrage de deux secondes et exécute la vérification toutes les 30 secondes.</span><span class="sxs-lookup"><span data-stu-id="89698-370">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="89698-371">Pour activer l’implémentation <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>, l’exemple enregistre `ReadinessPublisher` comme un service singleton dans le conteneur [d’injection de dépendance (DI)](xref:fundamentals/dependency-injection) :</span><span class="sxs-lookup"><span data-stu-id="89698-371">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

> [!NOTE]
> <span data-ttu-id="89698-372">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) inclut les serveurs de publication pour plusieurs systèmes, notamment [Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="89698-372">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="89698-373">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) n’est pas géré ou pris en charge par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="89698-373">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="89698-374">Restreindre les vérifications d’intégrité avec MapWhen</span><span class="sxs-lookup"><span data-stu-id="89698-374">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="89698-375">Utilisez <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> pour créer une branche conditionnelle du pipeline des demandes pour les points de terminaison de contrôle d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="89698-375">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="89698-376">Dans l’exemple suivant, `MapWhen` branche le pipeline de demande pour activer l’intergiciel (middleware) des contrôles d’intégrité si `api/HealthCheck` une requête d’extraction est reçue pour le point de terminaison :</span><span class="sxs-lookup"><span data-stu-id="89698-376">In the following example, `MapWhen` branches the request pipeline to activate Health Checks Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

```csharp
app.MapWhen(
    context => context.Request.Method == HttpMethod.Get.Method && 
        context.Request.Path.StartsWith("/api/HealthCheck"),
    builder => builder.UseHealthChecks());

app.UseEndpoints(endpoints =>
{
    endpoints.MapRazorPages();
});
```

<span data-ttu-id="89698-377">Pour plus d'informations, consultez <xref:fundamentals/middleware/index#use-run-and-map>.</span><span class="sxs-lookup"><span data-stu-id="89698-377">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="89698-378">ASP.NET Core offre un intergiciel et des bibliothèques de contrôle d’intégrité pour signaler l’intégrité des composants d’infrastructure d’application.</span><span class="sxs-lookup"><span data-stu-id="89698-378">ASP.NET Core offers Health Checks Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="89698-379">Les contrôles d’intégrité sont exposés par une application comme des points de terminaison HTTP.</span><span class="sxs-lookup"><span data-stu-id="89698-379">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="89698-380">Les points de terminaison de vérification d’intégrité peuvent être configurés pour de nombreux scénarios de supervision en temps réel :</span><span class="sxs-lookup"><span data-stu-id="89698-380">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="89698-381">Les sondes d’intégrité peuvent être utilisées par les orchestrateurs de conteneurs et les équilibreurs de charge afin de vérifier l’état d’une application.</span><span class="sxs-lookup"><span data-stu-id="89698-381">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="89698-382">Par exemple, un orchestrateur de conteneurs peut répondre à un résultat de non intégrité en arrêtant le déploiement ou en redémarrant un conteneur.</span><span class="sxs-lookup"><span data-stu-id="89698-382">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="89698-383">Face à une application non saine, l’équilibreur de charge peut réagir en redirigeant le trafic vers une instance saine.</span><span class="sxs-lookup"><span data-stu-id="89698-383">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="89698-384">L’utilisation de la mémoire, des disques et des autres ressources de serveur physique peut être supervisée dans le cadre d’un contrôle d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="89698-384">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="89698-385">Les contrôles d’intégrité peuvent tester les dépendances d’une application, telles que les bases de données et les points de terminaison de service externes, dans le but de vérifier leur disponibilité et leur bon fonctionnement.</span><span class="sxs-lookup"><span data-stu-id="89698-385">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="89698-386">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="89698-386">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="89698-387">L’exemple d’application comprend des exemples pour les scénarios décrits dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="89698-387">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="89698-388">Pour exécuter l’exemple d’application selon un scénario donné, utilisez la commande [dotnet run](/dotnet/core/tools/dotnet-run) dans un interpréteur de commandes, à partir du dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="89698-388">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="89698-389">Pour plus d’informations sur l’utilisation de l’exemple d’application, consultez le fichier *README.md* de l’exemple d’application ainsi que les descriptions de scénarios de cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="89698-389">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="89698-390">Prérequis</span><span class="sxs-lookup"><span data-stu-id="89698-390">Prerequisites</span></span>

<span data-ttu-id="89698-391">Les contrôles d’intégrité sont généralement utilisés avec un service de supervision externe ou un orchestrateur de conteneurs pour vérifier l’état d’une application.</span><span class="sxs-lookup"><span data-stu-id="89698-391">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="89698-392">Avant d’ajouter des contrôles d’intégrité à une application, vous devez décider du système de supervision à utiliser.</span><span class="sxs-lookup"><span data-stu-id="89698-392">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="89698-393">Le système de supervision détermine les types de contrôles d’intégrité qui doivent être créés ainsi que la configuration de leurs points de terminaison.</span><span class="sxs-lookup"><span data-stu-id="89698-393">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="89698-394">Référencez le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ou ajoutez une référence de package au package [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks).</span><span class="sxs-lookup"><span data-stu-id="89698-394">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span>

<span data-ttu-id="89698-395">L’exemple d’application fournit du code de démarrage pour illustrer les contrôles d’intégrité de plusieurs scénarios.</span><span class="sxs-lookup"><span data-stu-id="89698-395">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="89698-396">Le scénario [probe de la base de données](#database-probe) vérifie l’intégrité d’une connexion de base de données à l’aide d’[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span><span class="sxs-lookup"><span data-stu-id="89698-396">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="89698-397">Le scénario [Sondage DbContext](#entity-framework-core-dbcontext-probe) vérifie une base de données à l’aide d’un `DbContext` EF Core.</span><span class="sxs-lookup"><span data-stu-id="89698-397">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="89698-398">Pour explorer les scénarios de base de données, l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="89698-398">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="89698-399">Crée une base de données et fournit sa chaîne de connexion dans le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="89698-399">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="89698-400">Possède les références de package suivantes dans son fichier de projet :</span><span class="sxs-lookup"><span data-stu-id="89698-400">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="89698-401">AspNetCore.HealthChecks.SqlServer</span><span class="sxs-lookup"><span data-stu-id="89698-401">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="89698-402">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="89698-402">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="89698-403">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) n’est pas géré ou pris en charge par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="89698-403">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="89698-404">Un autre scénario de contrôle d’intégrité montre comment filtrer des contrôles d’intégrité selon un port de gestion.</span><span class="sxs-lookup"><span data-stu-id="89698-404">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="89698-405">L’exemple d’application vous oblige à créer un fichier *Properties/launchSettings.json* comprenant l’URL de gestion et le port de gestion.</span><span class="sxs-lookup"><span data-stu-id="89698-405">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="89698-406">Pour plus d’informations, consultez la section [Filtrer par port](#filter-by-port).</span><span class="sxs-lookup"><span data-stu-id="89698-406">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="89698-407">Sondage d’intégrité de base</span><span class="sxs-lookup"><span data-stu-id="89698-407">Basic health probe</span></span>

<span data-ttu-id="89698-408">Pour de nombreuses applications, un sondage d’intégrité de base qui signale la disponibilité d’une application pour le traitement des requêtes (*liveness*) suffit à découvrir l’état de l’application.</span><span class="sxs-lookup"><span data-stu-id="89698-408">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="89698-409">La configuration de base inscrit les services de contrôle d’intégrité et appelle l’intergiciel (middleware) des contrôles d’intégrité pour répondre à un point de terminaison d’URL avec une réponse d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="89698-409">The basic configuration registers health check services and calls the Health Checks Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="89698-410">Par défaut, aucun contrôle d’intégrité n’est inscrit pour tester les dépendances ou le sous-système.</span><span class="sxs-lookup"><span data-stu-id="89698-410">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="89698-411">L’application est considérée comme saine si elle est capable de répondre à l’URL de point de terminaison de contrôle d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="89698-411">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="89698-412">L’enregistreur de réponse par défaut écrit l’état (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) sous forme de texte en clair qu’il renvoie au client, indiquant si l’état est [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) ou [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus).</span><span class="sxs-lookup"><span data-stu-id="89698-412">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="89698-413">Pour inscrire les services de contrôle d’intégrité, utilisez <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="89698-413">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="89698-414">Ajoutez un point de terminaison pour l’intergiciel de <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> contrôles d’intégrité avec dans le `Startup.Configure`pipeline de traitement des requêtes de.</span><span class="sxs-lookup"><span data-stu-id="89698-414">Add an endpoint for Health Checks Middleware with <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the request processing pipeline of `Startup.Configure`.</span></span>

<span data-ttu-id="89698-415">Dans l’exemple d’application, le point de terminaison de contrôle d’intégrité est créé au niveau de `/health` (*BasicStartup.cs*) :</span><span class="sxs-lookup"><span data-stu-id="89698-415">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

```csharp
public class BasicStartup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddHealthChecks();
    }

    public void Configure(IApplicationBuilder app)
    {
        app.UseHealthChecks("/health");
    }
}
```

<span data-ttu-id="89698-416">Pour exécuter le scénario de configuration de base à l’aide de l’exemple d’application, exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="89698-416">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="89698-417">Exemple Docker</span><span class="sxs-lookup"><span data-stu-id="89698-417">Docker example</span></span>

<span data-ttu-id="89698-418">[Docker](xref:host-and-deploy/docker/index) fournit une directive `HEALTHCHECK` intégrée qui peut être utilisée pour vérifier l’état d’une application utilisant la configuration de contrôle d’intégrité de base :</span><span class="sxs-lookup"><span data-stu-id="89698-418">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="89698-419">Créer des contrôles d’intégrité</span><span class="sxs-lookup"><span data-stu-id="89698-419">Create health checks</span></span>

<span data-ttu-id="89698-420">Les contrôles d’intégrité sont créés via l’implémentation de l’interface <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck>.</span><span class="sxs-lookup"><span data-stu-id="89698-420">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="89698-421">La méthode <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> retourne un <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> qui indique l’état d’intégrité comme étant `Healthy`, `Degraded` ou `Unhealthy`.</span><span class="sxs-lookup"><span data-stu-id="89698-421">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="89698-422">Le résultat est écrit sous la forme de texte en clair avec un code d’état configurable (la configuration est décrite dans la section [Options de contrôle d’intégrité](#health-check-options)).</span><span class="sxs-lookup"><span data-stu-id="89698-422">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="89698-423"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> peut également retourner des paires clé-valeur facultatives.</span><span class="sxs-lookup"><span data-stu-id="89698-423"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

### <a name="example-health-check"></a><span data-ttu-id="89698-424">Exemple de contrôle d’intégrité</span><span class="sxs-lookup"><span data-stu-id="89698-424">Example health check</span></span>

<span data-ttu-id="89698-425">La classe `ExampleHealthCheck` suivante illustre la disposition d’un contrôle d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="89698-425">The following `ExampleHealthCheck` class demonstrates the layout of a health check.</span></span> <span data-ttu-id="89698-426">La logique des contrôles d’intégrité est placée `CheckHealthAsync` dans la méthode.</span><span class="sxs-lookup"><span data-stu-id="89698-426">The health checks logic is placed in the `CheckHealthAsync` method.</span></span> <span data-ttu-id="89698-427">L’exemple suivant définit une variable factice, `healthCheckResultHealthy`, sur `true`.</span><span class="sxs-lookup"><span data-stu-id="89698-427">The following example sets a dummy variable, `healthCheckResultHealthy`, to `true`.</span></span> <span data-ttu-id="89698-428">Si la valeur de `healthCheckResultHealthy` est définie sur `false`, l’état [HealthCheckResult. inhealth](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) est retourné.</span><span class="sxs-lookup"><span data-stu-id="89698-428">If the value of `healthCheckResultHealthy` is set to `false`, the [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) status is returned.</span></span>

```csharp
public class ExampleHealthCheck : IHealthCheck
{
    public Task<HealthCheckResult> CheckHealthAsync(
        HealthCheckContext context,
        CancellationToken cancellationToken = default(CancellationToken))
    {
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

### <a name="register-health-check-services"></a><span data-ttu-id="89698-429">Inscrire les services de contrôle d’intégrité</span><span class="sxs-lookup"><span data-stu-id="89698-429">Register health check services</span></span>

<span data-ttu-id="89698-430">Le `ExampleHealthCheck` type est ajouté aux services de contrôle d' `Startup.ConfigureServices` intégrité <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>dans avec :</span><span class="sxs-lookup"><span data-stu-id="89698-430">The `ExampleHealthCheck` type is added to health check services in `Startup.ConfigureServices` with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

<span data-ttu-id="89698-431">La surcharge <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> de l’exemple suivant définit l’état d’échec (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) de manière à être signalé lorsque le contrôle d’intégrité signale un échec.</span><span class="sxs-lookup"><span data-stu-id="89698-431">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="89698-432">Si l’état d’échec est défini sur `null` (par défaut), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) est signalé.</span><span class="sxs-lookup"><span data-stu-id="89698-432">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="89698-433">Cette surcharge est utile pour les créateurs de bibliothèque, dans les cas où l’état d’échec indiqué par la bibliothèque est appliqué par l’application lorsqu’un échec est signalé par le contrôle d’intégrité, si l’implémentation de ce dernier respecte le paramètre.</span><span class="sxs-lookup"><span data-stu-id="89698-433">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="89698-434">Des *étiquettes* peuvent être utilisées pour filtrer les contrôles d’intégrité (cette procédure est décrite plus loin dans la section [Filtrer les contrôles d’intégrité](#filter-health-checks)).</span><span class="sxs-lookup"><span data-stu-id="89698-434">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="89698-435"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> peut également exécuter une fonction lambda.</span><span class="sxs-lookup"><span data-stu-id="89698-435"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="89698-436">Dans l’exemple `Startup.ConfigureServices` suivant, le nom du contrôle d’intégrité est `Example` spécifié comme et le contrôle retourne toujours un état sain :</span><span class="sxs-lookup"><span data-stu-id="89698-436">In the following `Startup.ConfigureServices` example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

### <a name="use-health-checks-middleware"></a><span data-ttu-id="89698-437">Utiliser Health Checks Middleware</span><span class="sxs-lookup"><span data-stu-id="89698-437">Use Health Checks Middleware</span></span>

<span data-ttu-id="89698-438">Dans `Startup.Configure`, appelez <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> dans le pipeline de traitement avec l’URL de point de terminaison ou le chemin relatif :</span><span class="sxs-lookup"><span data-stu-id="89698-438">In `Startup.Configure`, call <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the processing pipeline with the endpoint URL or relative path:</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="89698-439">Si les contrôles d’intégrité doivent écouter un port en particulier, utilisez une surcharge de <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> pour définir le port (cette procédure est décrite plus loin dans la section [Filtrer par port](#filter-by-port)) :</span><span class="sxs-lookup"><span data-stu-id="89698-439">If the health checks should listen on a specific port, use an overload of <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> to set the port (described further in the [Filter by port](#filter-by-port) section):</span></span>

```csharp
app.UseHealthChecks("/health", port: 8000);
```

## <a name="health-check-options"></a><span data-ttu-id="89698-440">Options de contrôle d’intégrité</span><span class="sxs-lookup"><span data-stu-id="89698-440">Health check options</span></span>

<span data-ttu-id="89698-441"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> permet de personnaliser le comportement du contrôle d’intégrité :</span><span class="sxs-lookup"><span data-stu-id="89698-441"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="89698-442">Filtrer les contrôles d’intégrité</span><span class="sxs-lookup"><span data-stu-id="89698-442">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="89698-443">Personnaliser le code d’état HTTP</span><span class="sxs-lookup"><span data-stu-id="89698-443">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="89698-444">Supprimer les en-têtes de cache</span><span class="sxs-lookup"><span data-stu-id="89698-444">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="89698-445">Personnaliser la sortie</span><span class="sxs-lookup"><span data-stu-id="89698-445">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="89698-446">Filtrer les contrôles d’intégrité</span><span class="sxs-lookup"><span data-stu-id="89698-446">Filter health checks</span></span>

<span data-ttu-id="89698-447">Par défaut, l’intergiciel (middleware) des contrôles d’intégrité exécute toutes les vérifications d’intégrité inscrites.</span><span class="sxs-lookup"><span data-stu-id="89698-447">By default, Health Checks Middleware runs all registered health checks.</span></span> <span data-ttu-id="89698-448">Pour exécuter un sous-ensemble de contrôles d’intégrité, fournissez une fonction qui retourne une valeur booléenne à l’option <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate>.</span><span class="sxs-lookup"><span data-stu-id="89698-448">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="89698-449">Dans l’exemple suivant, le contrôle d’intégrité `Bar` est filtré en fonction de son étiquette (`bar_tag`) dans l’instruction conditionnelle de la fonction, où `true` est retourné uniquement si la propriété <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> du contrôle d’intégrité correspond à `foo_tag` ou à `baz_tag` :</span><span class="sxs-lookup"><span data-stu-id="89698-449">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

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
            HealthCheckResult.Unhealthy("Bar is unhealthy!"), 
                tags: new[] { "bar_tag" })
        .AddCheck("Baz", () =>
            HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
}

public void Configure(IApplicationBuilder app)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("foo_tag") ||
            check.Tags.Contains("baz_tag")
    });
}
```

### <a name="customize-the-http-status-code"></a><span data-ttu-id="89698-450">Personnaliser le code d’état HTTP</span><span class="sxs-lookup"><span data-stu-id="89698-450">Customize the HTTP status code</span></span>

<span data-ttu-id="89698-451">Utilisez <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> pour personnaliser le mappage de l’état d’intégrité aux codes d’état HTTP.</span><span class="sxs-lookup"><span data-stu-id="89698-451">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="89698-452">Les affectations <xref:Microsoft.AspNetCore.Http.StatusCodes> suivantes sont les valeurs par défaut utilisées par le middleware.</span><span class="sxs-lookup"><span data-stu-id="89698-452">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="89698-453">Vous pouvez modifier les valeurs de code d’état selon vos besoins.</span><span class="sxs-lookup"><span data-stu-id="89698-453">Change the status code values to meet your requirements.</span></span>

<span data-ttu-id="89698-454">Dans `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="89698-454">In `Startup.Configure`:</span></span>

```csharp
//using Microsoft.AspNetCore.Diagnostics.HealthChecks;
//using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    ResultStatusCodes =
    {
        [HealthStatus.Healthy] = StatusCodes.Status200OK,
        [HealthStatus.Degraded] = StatusCodes.Status200OK,
        [HealthStatus.Unhealthy] = StatusCodes.Status503ServiceUnavailable
    }
});
```

### <a name="suppress-cache-headers"></a><span data-ttu-id="89698-455">Supprimer les en-têtes de cache</span><span class="sxs-lookup"><span data-stu-id="89698-455">Suppress cache headers</span></span>

<span data-ttu-id="89698-456"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses>contrôle si l’intergiciel (middleware) de contrôles d’intégrité ajoute des en-têtes HTTP à une réponse de sondage pour empêcher la mise en cache des réponses.</span><span class="sxs-lookup"><span data-stu-id="89698-456"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Checks Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="89698-457">Si la valeur est `false` (par défaut), le middleware définit ou substitue les en-têtes `Cache-Control`, `Expires` et `Pragma` afin d’empêcher la mise en cache de la réponse.</span><span class="sxs-lookup"><span data-stu-id="89698-457">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="89698-458">Si la valeur est `true`, le middleware ne modifie pas les en-têtes de cache de la réponse.</span><span class="sxs-lookup"><span data-stu-id="89698-458">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

<span data-ttu-id="89698-459">Dans `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="89698-459">In `Startup.Configure`:</span></span>

```csharp
//using Microsoft.AspNetCore.Diagnostics.HealthChecks;
//using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    AllowCachingResponses = false
});
```

### <a name="customize-output"></a><span data-ttu-id="89698-460">Personnaliser la sortie</span><span class="sxs-lookup"><span data-stu-id="89698-460">Customize output</span></span>

<span data-ttu-id="89698-461">L’option <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> obtient ou définit un délégué utilisé pour écrire la réponse.</span><span class="sxs-lookup"><span data-stu-id="89698-461">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span> <span data-ttu-id="89698-462">Le délégué par défaut écrit une réponse minimale constituée de texte en clair, avec la valeur de chaîne [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span><span class="sxs-lookup"><span data-stu-id="89698-462">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span>

<span data-ttu-id="89698-463">Dans `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="89698-463">In `Startup.Configure`:</span></span>

```csharp
// using Microsoft.AspNetCore.Diagnostics.HealthChecks;
// using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    ResponseWriter = WriteResponse
});
```

<span data-ttu-id="89698-464">Le délégué par défaut écrit une réponse minimale constituée de texte en clair, avec la valeur de chaîne [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span><span class="sxs-lookup"><span data-stu-id="89698-464">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span> <span data-ttu-id="89698-465">Le délégué personnalisé suivant, `WriteResponse`, génère une réponse JSON personnalisée :</span><span class="sxs-lookup"><span data-stu-id="89698-465">The following custom delegate, `WriteResponse`, outputs a custom JSON response:</span></span>

```csharp
private static Task WriteResponse(HttpContext httpContext, HealthReport result)
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

<span data-ttu-id="89698-466">Le système de contrôle d’intégrité ne fournit pas de prise en charge intégrée des formats de retour JSON complexes, car le format est spécifique à votre choix de système de surveillance.</span><span class="sxs-lookup"><span data-stu-id="89698-466">The health checks system doesn't provide built-in support for complex JSON return formats because the format is specific to your choice of monitoring system.</span></span> <span data-ttu-id="89698-467">N’hésitez pas à `JObject` personnaliser le dans l’exemple précédent si nécessaire pour répondre à vos besoins.</span><span class="sxs-lookup"><span data-stu-id="89698-467">Feel free to customize the `JObject` in the preceding example as necessary to meet your needs.</span></span>

## <a name="database-probe"></a><span data-ttu-id="89698-468">Sondage de base de données</span><span class="sxs-lookup"><span data-stu-id="89698-468">Database probe</span></span>

<span data-ttu-id="89698-469">Un contrôle d’intégrité peut spécifier une requête de base de données à exécuter en tant que test booléen pour indiquer si la base de données répond normalement.</span><span class="sxs-lookup"><span data-stu-id="89698-469">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="89698-470">L’exemple d’application utilise [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), une bibliothèque de vérification d’intégrité pour les applications ASP.NET Core, pour effectuer une vérification d’intégrité sur une base de données SQL Server.</span><span class="sxs-lookup"><span data-stu-id="89698-470">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="89698-471">`AspNetCore.Diagnostics.HealthChecks` exécute une demande `SELECT 1` sur la base de données pour confirmer que la connexion à la base de données est saine.</span><span class="sxs-lookup"><span data-stu-id="89698-471">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="89698-472">Lorsque vous vérifiez une connexion de base de données à l’aide d’une requête, choisissez une requête qui est retournée rapidement.</span><span class="sxs-lookup"><span data-stu-id="89698-472">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="89698-473">L’utilisation d’une requête risque néanmoins d’entraîner une surcharge de la base de données et d’en diminuer les performances.</span><span class="sxs-lookup"><span data-stu-id="89698-473">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="89698-474">Dans la plupart des cas, il n’est pas nécessaire d’utiliser une requête de test.</span><span class="sxs-lookup"><span data-stu-id="89698-474">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="89698-475">Il suffit simplement d’établir une connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="89698-475">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="89698-476">Si vous avez besoin d’exécuter une requête, choisissez une requête SELECT simple, telle que `SELECT 1`.</span><span class="sxs-lookup"><span data-stu-id="89698-476">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="89698-477">Inclure une référence de package à [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="89698-477">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="89698-478">Fournissez une chaîne de connexion de base de données valide dans le fichier *appsettings.json* de l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="89698-478">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="89698-479">L’application utilise une base de données SQL Server nommée `HealthCheckSample` :</span><span class="sxs-lookup"><span data-stu-id="89698-479">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="89698-480">Pour inscrire les services de contrôle d’intégrité, utilisez <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="89698-480">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="89698-481">L’exemple d’application appelle la méthode `AddSqlServer` avec la chaîne de connexion de la base de données (*DbHealthStartup.cs*) :</span><span class="sxs-lookup"><span data-stu-id="89698-481">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="89698-482">Call Health Checks middleware dans le pipeline de traitement `Startup.Configure`des applications dans :</span><span class="sxs-lookup"><span data-stu-id="89698-482">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`:</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="89698-483">Pour exécuter le scénario de sondage d’une base de données à l’aide de l’exemple d’application, exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="89698-483">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="89698-484">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) n’est pas géré ou pris en charge par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="89698-484">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="89698-485">Sondage DbContext Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="89698-485">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="89698-486">La vérification `DbContext` vérifie que l’application peut communiquer avec la base de données configurée pour un `DbContext` EF Core.</span><span class="sxs-lookup"><span data-stu-id="89698-486">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="89698-487">La vérification `DbContext` est prise en charge dans les applications qui :</span><span class="sxs-lookup"><span data-stu-id="89698-487">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="89698-488">Utilisent [Entity Framework (EF) Core](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="89698-488">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="89698-489">Incluent une référence de package à [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span><span class="sxs-lookup"><span data-stu-id="89698-489">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="89698-490">`AddDbContextCheck<TContext>` inscrit un contrôle d’intégrité pour un `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="89698-490">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="89698-491">Le `DbContext` est fourni en tant que `TContext` à la méthode.</span><span class="sxs-lookup"><span data-stu-id="89698-491">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="89698-492">Une surcharge est disponible pour configurer l’état d’échec, les étiquettes et une requête de test personnalisée.</span><span class="sxs-lookup"><span data-stu-id="89698-492">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="89698-493">Par défaut :</span><span class="sxs-lookup"><span data-stu-id="89698-493">By default:</span></span>

* <span data-ttu-id="89698-494">`DbContextHealthCheck` appelle la méthode `CanConnectAsync` d’EF Core.</span><span class="sxs-lookup"><span data-stu-id="89698-494">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="89698-495">Vous pouvez choisir quelle opération doit être exécutée lors du contrôle d’intégrité à l’aide des surcharges de la méthode `AddDbContextCheck`.</span><span class="sxs-lookup"><span data-stu-id="89698-495">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="89698-496">Le nom du contrôle d’intégrité correspond à celui du type `TContext`.</span><span class="sxs-lookup"><span data-stu-id="89698-496">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="89698-497">Dans l’exemple d’application `AppDbContext` , est fourni `AddDbContextCheck` à et enregistré en tant que `Startup.ConfigureServices` service dans (*DbContextHealthStartup.cs*) :</span><span class="sxs-lookup"><span data-stu-id="89698-497">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="89698-498">Dans l’exemple d’application `UseHealthChecks` , ajoute l’intergiciel de contrôles d' `Startup.Configure`intégrité dans.</span><span class="sxs-lookup"><span data-stu-id="89698-498">In the sample app, `UseHealthChecks` adds the Health Checks Middleware in `Startup.Configure`.</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="89698-499">Pour exécuter le scénario de sondage `DbContext` à l’aide de l’exemple d’application, vérifiez que la base de données spécifiée par le la chaîne de connexion ne se trouve pas déjà dans l’instance SQL Server.</span><span class="sxs-lookup"><span data-stu-id="89698-499">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="89698-500">Si la base de données existe, supprimez-la.</span><span class="sxs-lookup"><span data-stu-id="89698-500">If the database exists, delete it.</span></span>

<span data-ttu-id="89698-501">Exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="89698-501">Execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario dbcontext
```

<span data-ttu-id="89698-502">Une fois que l’application est en cours d’exécution, vérifiez l’état d’intégrité en envoyant une requête au point de terminaison `/health` dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="89698-502">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="89698-503">La base de données et `AppDbContext` n’existent pas, donc l’application fournit la réponse suivante :</span><span class="sxs-lookup"><span data-stu-id="89698-503">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="89698-504">Déclenchez l’exemple d’application pour créer la base de données.</span><span class="sxs-lookup"><span data-stu-id="89698-504">Trigger the sample app to create the database.</span></span> <span data-ttu-id="89698-505">Envoyez une requête à `/createdatabase`.</span><span class="sxs-lookup"><span data-stu-id="89698-505">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="89698-506">L’application répond :</span><span class="sxs-lookup"><span data-stu-id="89698-506">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="89698-507">Envoyez une requête au point de terminaison `/health`.</span><span class="sxs-lookup"><span data-stu-id="89698-507">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="89698-508">La base de données et le contexte existent, donc l’application répond :</span><span class="sxs-lookup"><span data-stu-id="89698-508">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="89698-509">Déclenchez l’exemple d’application pour supprimer la base de données.</span><span class="sxs-lookup"><span data-stu-id="89698-509">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="89698-510">Envoyez une requête à `/deletedatabase`.</span><span class="sxs-lookup"><span data-stu-id="89698-510">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="89698-511">L’application répond :</span><span class="sxs-lookup"><span data-stu-id="89698-511">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="89698-512">Envoyez une requête au point de terminaison `/health`.</span><span class="sxs-lookup"><span data-stu-id="89698-512">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="89698-513">L’application fournit une réponse non intégrité :</span><span class="sxs-lookup"><span data-stu-id="89698-513">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="89698-514">Séparer les sondages probe readiness et probe liveness</span><span class="sxs-lookup"><span data-stu-id="89698-514">Separate readiness and liveness probes</span></span>

<span data-ttu-id="89698-515">Dans certains scénarios d’hébergement, une paire de contrôles d’intégrité est utilisée pour distinguer deux états d’application :</span><span class="sxs-lookup"><span data-stu-id="89698-515">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="89698-516">L’application fonctionne mais n’est pas encore prête à recevoir des requêtes.</span><span class="sxs-lookup"><span data-stu-id="89698-516">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="89698-517">Il s’agit de l’état de *readiness* qui indique que l’application est prête.</span><span class="sxs-lookup"><span data-stu-id="89698-517">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="89698-518">L’application fonctionne et répond aux requêtes.</span><span class="sxs-lookup"><span data-stu-id="89698-518">The app is functioning and responding to requests.</span></span> <span data-ttu-id="89698-519">Il s’agit de l’état de *liveness* qui indique que l’application est active.</span><span class="sxs-lookup"><span data-stu-id="89698-519">This state is the app's *liveness*.</span></span>

<span data-ttu-id="89698-520">En général, la vérification qui permet de savoir si une application est prête implique un ensemble de vérifications plus complet permettant de déterminer si tous les sous-systèmes et toutes les ressources de l’application sont disponibles, ce qui est un processus assez long.</span><span class="sxs-lookup"><span data-stu-id="89698-520">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="89698-521">La vérification qui permet de savoir si une application est active est simple et rapide, et vise à déterminer si l’application est disponible pour traiter les requêtes.</span><span class="sxs-lookup"><span data-stu-id="89698-521">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="89698-522">Une fois que l’application est considérée comme prête, il est inutile de recommencer le test. En effet, le seul test nécessaire ensuite est celui visant à vérifier que l’application est active.</span><span class="sxs-lookup"><span data-stu-id="89698-522">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="89698-523">L’exemple d’application contient un contrôle d’intégrité permettant de signaler l’achèvement d’une tâche de démarrage de longue durée dans un [service hébergé](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="89698-523">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="89698-524">`StartupHostedServiceHealthCheck` expose la propriété `StartupTaskCompleted`, que le service hébergé peut définir sur `true` lorsque sa tâche de longue durée est terminée (*StartupHostedServiceHealthCheck.cs*) :</span><span class="sxs-lookup"><span data-stu-id="89698-524">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="89698-525">La tâche de longue durée en arrière-plan est démarrée par un [service hébergé](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span><span class="sxs-lookup"><span data-stu-id="89698-525">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="89698-526">À la fin de la tâche, `StartupHostedServiceHealthCheck.StartupTaskCompleted` est défini sur `true` :</span><span class="sxs-lookup"><span data-stu-id="89698-526">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="89698-527">Le contrôle d’intégrité est inscrit auprès de <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> dans `Startup.ConfigureServices` en même temps que le service hébergé.</span><span class="sxs-lookup"><span data-stu-id="89698-527">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="89698-528">Étant donné que le service hébergé doit définir la propriété sur le contrôle d’intégrité, le contrôle d’intégrité est également inscrit dans le conteneur du service (*LivenessProbeStartup.cs*) :</span><span class="sxs-lookup"><span data-stu-id="89698-528">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="89698-529">Call Health Checks middleware dans le pipeline de traitement `Startup.Configure`des applications dans.</span><span class="sxs-lookup"><span data-stu-id="89698-529">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="89698-530">Dans l’exemple d’application, les points de terminaison de contrôle d’intégrité sont créés au niveau de `/health/ready` pour vérifier si l’application est prête, et au niveau de `/health/live` pour vérifier si l’application est active.</span><span class="sxs-lookup"><span data-stu-id="89698-530">In the sample app, the health check endpoints are created at `/health/ready` for the readiness check and `/health/live` for the liveness check.</span></span> <span data-ttu-id="89698-531">Le test qui permet de vérifier si l’application est prête filtre les contrôles d’intégrité pour n’afficher que celui dont l’étiquette est `ready`.</span><span class="sxs-lookup"><span data-stu-id="89698-531">The readiness check filters health checks to the health check with the `ready` tag.</span></span> <span data-ttu-id="89698-532">Le test qui permet de vérifier si l’application est active exclut le `StartupHostedServiceHealthCheck` en retournant `false` dans [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (pour plus d’informations, consultez [Filtrer les contrôles d’intégrité](#filter-health-checks)) :</span><span class="sxs-lookup"><span data-stu-id="89698-532">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks)):</span></span>

```csharp
app.UseHealthChecks("/health/ready", new HealthCheckOptions()
{
    Predicate = (check) => check.Tags.Contains("ready"), 
});

app.UseHealthChecks("/health/live", new HealthCheckOptions()
{
    Predicate = (_) => false
});
```

<span data-ttu-id="89698-533">Pour exécuter le scénario de configuration readiness/liveness à l’aide de l’exemple d’application, exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="89698-533">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario liveness
```

<span data-ttu-id="89698-534">Dans un navigateur, accédez à `/health/ready` plusieurs fois jusqu’à ce que 15 secondes soient écoulées.</span><span class="sxs-lookup"><span data-stu-id="89698-534">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="89698-535">Le contrôle d’intégrité signale *Unhealthy* pendant les 15 premières secondes.</span><span class="sxs-lookup"><span data-stu-id="89698-535">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="89698-536">Après 15 secondes, le point de terminaison signale *Healthy*, ce qui reflète l’achèvement de la tâche de longue durée exécutée par le service hébergé.</span><span class="sxs-lookup"><span data-stu-id="89698-536">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="89698-537">Cet exemple crée également un éditeur de vérification de l’intégrité (implémentation <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>) qui exécute la première vérification de disponibilité avec un délai de deux secondes.</span><span class="sxs-lookup"><span data-stu-id="89698-537">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="89698-538">Pour plus d’informations, consultez la section [Éditeur de vérification de l’intégrité](#health-check-publisher).</span><span class="sxs-lookup"><span data-stu-id="89698-538">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="89698-539">Exemple Kubernetes</span><span class="sxs-lookup"><span data-stu-id="89698-539">Kubernetes example</span></span>

<span data-ttu-id="89698-540">Dans un environnement tel que [Kubernetes](https://kubernetes.io/), il peut être utile d’utiliser séparément le test permettant de savoir si la l’application est prête et celui visant à savoir si l’application est active.</span><span class="sxs-lookup"><span data-stu-id="89698-540">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="89698-541">Dans Kubernetes, une application peut devoir effectuer une tâche de démarrage de longue durée avant d’accepter des requêtes, telles qu’un test de la disponibilité de la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="89698-541">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="89698-542">L’utilisation séparée des deux tests permet à l’orchestrateur de faire la distinction entre une application qui fonctionne mais qui n’est pas encore prête, et une application qui n’a pas pu démarrer.</span><span class="sxs-lookup"><span data-stu-id="89698-542">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="89698-543">Pour plus d’informations sur les sondages probe readiness et probe liveness dans Kubernetes, consultez [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) dans la documentation Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="89698-543">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="89698-544">L’exemple suivant montre une configuration probe readiness Kubernetes :</span><span class="sxs-lookup"><span data-stu-id="89698-544">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="89698-545">Sonde basée sur les métriques avec un enregistreur de réponse personnalisé</span><span class="sxs-lookup"><span data-stu-id="89698-545">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="89698-546">L’exemple d’application montre un contrôle d’intégrité de mémoire avec un enregistreur de réponse personnalisé.</span><span class="sxs-lookup"><span data-stu-id="89698-546">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="89698-547">`MemoryHealthCheck`signale un État non sain si l’application utilise plus d’un seuil de mémoire donné (1 Go dans l’exemple d’application).</span><span class="sxs-lookup"><span data-stu-id="89698-547">`MemoryHealthCheck` reports an unhealthy status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="89698-548"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> inclut des informations de récupérateur de mémoire pour l’application (*MemoryHealthCheck.cs*) :</span><span class="sxs-lookup"><span data-stu-id="89698-548">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="89698-549">Pour inscrire les services de contrôle d’intégrité, utilisez <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="89698-549">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="89698-550">Au lieu d’activer le contrôle d’intégrité en le passant à <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, `MemoryHealthCheck` est inscrit en tant que service.</span><span class="sxs-lookup"><span data-stu-id="89698-550">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="89698-551">Tous les services <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> inscrits sont disponibles pour les services de contrôle d’intégrité et le middleware.</span><span class="sxs-lookup"><span data-stu-id="89698-551">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="89698-552">Nous vous recommandons d’inscrire les services de contrôle d’intégrité en tant que services Singleton.</span><span class="sxs-lookup"><span data-stu-id="89698-552">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="89698-553">Dans l’exemple d’application (*CustomWriterStartup.cs*) :</span><span class="sxs-lookup"><span data-stu-id="89698-553">In the sample app (*CustomWriterStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="89698-554">Call Health Checks middleware dans le pipeline de traitement `Startup.Configure`des applications dans.</span><span class="sxs-lookup"><span data-stu-id="89698-554">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="89698-555">Un délégué `WriteResponse` est fourni à la propriété `ResponseWriter` pour produire une réponse JSON personnalisée lorsque le contrôle d’intégrité est exécuté :</span><span class="sxs-lookup"><span data-stu-id="89698-555">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // This custom writer formats the detailed status as JSON.
        ResponseWriter = WriteResponse
    });
}
```

<span data-ttu-id="89698-556">La méthode `WriteResponse` formate le `CompositeHealthCheckResult` en un objet JSON et génère une sortie JSON pour la réponse du contrôle d’intégrité :</span><span class="sxs-lookup"><span data-stu-id="89698-556">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="89698-557">Pour exécuter le sondage basé sur les métriques avec l’enregistreur de réponse personnalisé à l’aide de l’exemple d’application, exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="89698-557">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="89698-558">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) inclut des scénarios de vérification de l’intégrité basés sur des métriques, notamment des vérifications du stockage sur disque et de la vivacité maximale des valeurs.</span><span class="sxs-lookup"><span data-stu-id="89698-558">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="89698-559">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) n’est pas géré ou pris en charge par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="89698-559">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="89698-560">Filtrer par port</span><span class="sxs-lookup"><span data-stu-id="89698-560">Filter by port</span></span>

<span data-ttu-id="89698-561">Si vous appelez <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> avec un port, les requêtes de contrôle d’intégrité seront limitées au port spécifié.</span><span class="sxs-lookup"><span data-stu-id="89698-561">Calling <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> with a port restricts health check requests to the port specified.</span></span> <span data-ttu-id="89698-562">Ceci est généralement utilisé dans les environnements de conteneurs pour exposer un port aux services de supervision.</span><span class="sxs-lookup"><span data-stu-id="89698-562">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="89698-563">L’exemple d’application configure le port à l’aide du [fournisseur de configuration des variables d’environnement](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="89698-563">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="89698-564">Le port est défini dans le fichier *launchSettings.json*, puis transmis au fournisseur de configuration via une variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="89698-564">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="89698-565">Vous devez également configurer le serveur pour écouter les requêtes qui arrivent au port de gestion.</span><span class="sxs-lookup"><span data-stu-id="89698-565">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="89698-566">Pour utiliser l’exemple d’application dans le but d’illustrer la configuration du port de gestion, créez le fichier *launchSettings.json* dans un dossier *Propriétés*.</span><span class="sxs-lookup"><span data-stu-id="89698-566">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="89698-567">Le fichier *Properties/launchSettings. JSON* suivant dans l’exemple d’application n’est pas inclus dans les fichiers projet de l’exemple d’application et doit être créé manuellement :</span><span class="sxs-lookup"><span data-stu-id="89698-567">The following *Properties/launchSettings.json* file in the sample app isn't included in the sample app's project files and must be created manually:</span></span>

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

<span data-ttu-id="89698-568">Pour inscrire les services de contrôle d’intégrité, utilisez <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="89698-568">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="89698-569">L’appel à <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> spécifie le port de gestion (*ManagementPortStartup.cs*) :</span><span class="sxs-lookup"><span data-stu-id="89698-569">The call to <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> specifies the management port (*ManagementPortStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=17)]

> [!NOTE]
> <span data-ttu-id="89698-570">Vous pouvez éviter de créer le fichier *launchSettings.json* dans l’exemple d’application en définissant explicitement les URL et le port de gestion dans le code.</span><span class="sxs-lookup"><span data-stu-id="89698-570">You can avoid creating the *launchSettings.json* file in the sample app by setting the URLs and management port explicitly in code.</span></span> <span data-ttu-id="89698-571">Dans *Program.cs* où <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> est créé, ajoutez un appel à <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> et fournissez le point de terminaison de réponse normal et le point de terminaison de port de gestion de l’application.</span><span class="sxs-lookup"><span data-stu-id="89698-571">In *Program.cs* where the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> and provide the app's normal response endpoint and the management port endpoint.</span></span> <span data-ttu-id="89698-572">Dans *ManagementPortStartup.cs* où <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> est appelé, spécifiez le port de gestion explicitement.</span><span class="sxs-lookup"><span data-stu-id="89698-572">In *ManagementPortStartup.cs* where <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> is called, specify the management port explicitly.</span></span>
>
> <span data-ttu-id="89698-573">*Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="89698-573">*Program.cs*:</span></span>
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
> <span data-ttu-id="89698-574">*ManagementPortStartup.cs* :</span><span class="sxs-lookup"><span data-stu-id="89698-574">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

<span data-ttu-id="89698-575">Pour exécuter le scénario de configuration du port de gestion à l’aide de l’exemple d’application, exécutez la commande suivante à partir du dossier du projet, dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="89698-575">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="89698-576">Distribuer une bibliothèque de contrôle d’intégrité</span><span class="sxs-lookup"><span data-stu-id="89698-576">Distribute a health check library</span></span>

<span data-ttu-id="89698-577">Pour distribuer une bibliothèque comme un contrôle d’intégrité :</span><span class="sxs-lookup"><span data-stu-id="89698-577">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="89698-578">Écrivez un contrôle d’intégrité qui implémente l’interface <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> comme une classe autonome.</span><span class="sxs-lookup"><span data-stu-id="89698-578">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="89698-579">La classe peut utiliser une [injection de dépendance](xref:fundamentals/dependency-injection), une activation de type et des [options nommées](xref:fundamentals/configuration/options) pour accéder aux données de configuration.</span><span class="sxs-lookup"><span data-stu-id="89698-579">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   <span data-ttu-id="89698-580">Dans la logique des contrôles d' `CheckHealthAsync`intégrité de :</span><span class="sxs-lookup"><span data-stu-id="89698-580">In the health checks logic of `CheckHealthAsync`:</span></span>

   * <span data-ttu-id="89698-581">`data1`et `data2` sont utilisés dans la méthode pour exécuter la logique de contrôle d’intégrité de la sonde.</span><span class="sxs-lookup"><span data-stu-id="89698-581">`data1` and `data2` are used in the method to run the probe's health check logic.</span></span>
   * <span data-ttu-id="89698-582">`AccessViolationException`est géré.</span><span class="sxs-lookup"><span data-stu-id="89698-582">`AccessViolationException` is handled.</span></span>

   <span data-ttu-id="89698-583">Lorsqu’un <xref:System.AccessViolationException> se produit <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> , est retourné avec le <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> pour permettre aux utilisateurs de configurer l’état d’échec des contrôles d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="89698-583">When an <xref:System.AccessViolationException> occurs, the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> is returned with the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> to allow users to configure the health checks failure status.</span></span>

   ```csharp
   using System;
   using System.Threading;
   using System.Threading.Tasks;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

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
   ```

1. <span data-ttu-id="89698-584">Écrivez une méthode d’extension avec des paramètres que l’application consommatrice appelle dans sa méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="89698-584">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="89698-585">Dans l’exemple qui suit, prenons la signature de méthode de contrôle d’intégrité suivante :</span><span class="sxs-lookup"><span data-stu-id="89698-585">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="89698-586">La signature précédente indique que `ExampleHealthCheck` nécessite des données supplémentaires pour traiter la logique de sondage du contrôle d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="89698-586">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="89698-587">Les données sont fournies au délégué qui est utilisé pour créer l’instance de contrôle d’intégrité lorsque le contrôle d’intégrité est inscrit avec une méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="89698-587">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="89698-588">Dans l’exemple qui suit, l’appelant spécifie les éléments facultatifs suivants :</span><span class="sxs-lookup"><span data-stu-id="89698-588">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="89698-589">Nom du contrôle d’intégrité (`name`).</span><span class="sxs-lookup"><span data-stu-id="89698-589">health check name (`name`).</span></span> <span data-ttu-id="89698-590">Si `null`, `example_health_check` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="89698-590">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="89698-591">Point de données de chaîne du contrôle d’intégrité (`data1`).</span><span class="sxs-lookup"><span data-stu-id="89698-591">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="89698-592">Point de données Integer du contrôle d’intégrité (`data2`).</span><span class="sxs-lookup"><span data-stu-id="89698-592">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="89698-593">Si `null`, `1` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="89698-593">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="89698-594">État d’échec (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span><span class="sxs-lookup"><span data-stu-id="89698-594">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="89698-595">La valeur par défaut est `null`.</span><span class="sxs-lookup"><span data-stu-id="89698-595">The default is `null`.</span></span> <span data-ttu-id="89698-596">Si `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) est signalé pour un état d’échec.</span><span class="sxs-lookup"><span data-stu-id="89698-596">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="89698-597">Étiquettes (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="89698-597">tags (`IEnumerable<string>`).</span></span>

   ```csharp
   using System.Collections.Generic;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   public static class ExampleHealthCheckBuilderExtensions
   {
       const string DefaultName = "example_health_check";

       public static IHealthChecksBuilder AddExampleHealthCheck(
           this IHealthChecksBuilder builder,
           string name = default,
           string data1,
           int data2 = 1,
           HealthStatus? failureStatus = default,
           IEnumerable<string> tags = default)
       {
           return builder.Add(new HealthCheckRegistration(
               name ?? DefaultName,
               sp => new ExampleHealthCheck(data1, data2),
               failureStatus,
               tags));
       }
   }
   ```

## <a name="health-check-publisher"></a><span data-ttu-id="89698-598">Serveur de publication des contrôles d’intégrité</span><span class="sxs-lookup"><span data-stu-id="89698-598">Health Check Publisher</span></span>

<span data-ttu-id="89698-599">Quand un <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> est ajouté au conteneur du service, le système de contrôle d’intégrité exécute régulièrement vos contrôles d’intégrité et appelle `PublishAsync` avec le résultat.</span><span class="sxs-lookup"><span data-stu-id="89698-599">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="89698-600">C’est utile dans un scénario impliquant un système de supervision basé sur les envois (push) et nécessitant que chaque processus appelle le système de supervision régulièrement afin de déterminer l’état d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="89698-600">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="89698-601">L’interface <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> comprend une seule méthode :</span><span class="sxs-lookup"><span data-stu-id="89698-601">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="89698-602"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> vous autorise à définir :</span><span class="sxs-lookup"><span data-stu-id="89698-602"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="89698-603"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; Le retard initial appliqué après le démarrage de l’application avant d’exécuter les instances <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="89698-603"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="89698-604">Le retard est appliqué à une seule fois au démarrage et ne s’applique pas aux itérations ultérieures.</span><span class="sxs-lookup"><span data-stu-id="89698-604">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="89698-605">La valeur par défaut est de cinq secondes.</span><span class="sxs-lookup"><span data-stu-id="89698-605">The default value is five seconds.</span></span>
* <span data-ttu-id="89698-606"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; La période d’exécution de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="89698-606"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="89698-607">La valeur par défaut est de 30 secondes.</span><span class="sxs-lookup"><span data-stu-id="89698-607">The default value is 30 seconds.</span></span>
* <span data-ttu-id="89698-608"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; Si <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> est `null` (valeur par défaut), le service d’éditeur de vérification de l’intégrité exécute toutes les vérifications d’intégrité inscrites.</span><span class="sxs-lookup"><span data-stu-id="89698-608"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="89698-609">Pour exécuter un sous-ensemble de contrôles d’intégrité, fournissez une fonction qui filtre l’ensemble de vérifications.</span><span class="sxs-lookup"><span data-stu-id="89698-609">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="89698-610">Le prédicat est évalué à chaque période.</span><span class="sxs-lookup"><span data-stu-id="89698-610">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="89698-611"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; Le délai d’attente pour l’exécution des vérifications d’intégrité pour toutes les instances <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="89698-611"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="89698-612">Utilisez <xref:System.Threading.Timeout.InfiniteTimeSpan> pour une exécution sans délai d’attente.</span><span class="sxs-lookup"><span data-stu-id="89698-612">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="89698-613">La valeur par défaut est de 30 secondes.</span><span class="sxs-lookup"><span data-stu-id="89698-613">The default value is 30 seconds.</span></span>

> [!WARNING]
> <span data-ttu-id="89698-614">Dans la version ASP.NET Core 2.2, le paramètre <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> n’est pas respecté par l’implémentation <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> ; il définit la valeur de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span><span class="sxs-lookup"><span data-stu-id="89698-614">In the ASP.NET Core 2.2 release, setting <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> isn't honored by the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation; it sets the value of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span></span> <span data-ttu-id="89698-615">Ce problème a été résolu dans ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="89698-615">This issue has been addressed in ASP.NET Core 3.0.</span></span>

<span data-ttu-id="89698-616">Dans l’exemple d’application, `ReadinessPublisher` est une implémentation <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="89698-616">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="89698-617">L’état du contrôle d’intégrité est journalisé pour chaque vérification comme suit :</span><span class="sxs-lookup"><span data-stu-id="89698-617">The health check status is logged for each check as either:</span></span>

* <span data-ttu-id="89698-618">Information (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) si l’état des contrôles d' <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>intégrité est.</span><span class="sxs-lookup"><span data-stu-id="89698-618">Information (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) if the health checks status is <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span></span>
* <span data-ttu-id="89698-619">Error (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) si l’état <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> est ou <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span><span class="sxs-lookup"><span data-stu-id="89698-619">Error (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) if the status is either <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> or <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=18-27)]

<span data-ttu-id="89698-620">Dans l’exemple d’application `LivenessProbeStartup`, la vérification de la disponibilité `StartupHostedService` a un délai de démarrage de deux secondes et exécute la vérification toutes les 30 secondes.</span><span class="sxs-lookup"><span data-stu-id="89698-620">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="89698-621">Pour activer l’implémentation <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>, l’exemple enregistre `ReadinessPublisher` comme un service singleton dans le conteneur [d’injection de dépendance (DI)](xref:fundamentals/dependency-injection) :</span><span class="sxs-lookup"><span data-stu-id="89698-621">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices&highlight=12-17,28)]

> [!NOTE]
> <span data-ttu-id="89698-622">La solution de contournement suivante autorise l’ajout d’une instance <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> au conteneur de service lorsqu’un ou plusieurs autres services hébergés ont déjà été ajoutés à l’application.</span><span class="sxs-lookup"><span data-stu-id="89698-622">The following workaround permits adding an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instance to the service container when one or more other hosted services have already been added to the app.</span></span> <span data-ttu-id="89698-623">Cette solution de contournement n’est pas nécessaire dans ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="89698-623">This workaround won't isn't required in ASP.NET Core 3.0.</span></span>
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

> [!NOTE]
> <span data-ttu-id="89698-624">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) inclut les serveurs de publication pour plusieurs systèmes, notamment [Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="89698-624">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="89698-625">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) n’est pas géré ou pris en charge par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="89698-625">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="89698-626">Restreindre les vérifications d’intégrité avec MapWhen</span><span class="sxs-lookup"><span data-stu-id="89698-626">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="89698-627">Utilisez <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> pour créer une branche conditionnelle du pipeline des demandes pour les points de terminaison de contrôle d’intégrité.</span><span class="sxs-lookup"><span data-stu-id="89698-627">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="89698-628">Dans l’exemple suivant, `MapWhen` branche le pipeline de demande pour activer l’intergiciel (middleware) des contrôles d’intégrité si `api/HealthCheck` une requête d’extraction est reçue pour le point de terminaison :</span><span class="sxs-lookup"><span data-stu-id="89698-628">In the following example, `MapWhen` branches the request pipeline to activate Health Checks Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

```csharp
app.MapWhen(
    context => context.Request.Method == HttpMethod.Get.Method && 
        context.Request.Path.StartsWith("/api/HealthCheck"),
    builder => builder.UseHealthChecks());

app.UseMvc();
```

<span data-ttu-id="89698-629">Pour plus d'informations, consultez <xref:fundamentals/middleware/index#use-run-and-map>.</span><span class="sxs-lookup"><span data-stu-id="89698-629">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>

::: moniker-end
