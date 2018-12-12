---
title: Hôte générique .NET
author: guardrex
description: Découvrez l’hôte générique dans .NET, responsable de la gestion du démarrage et de la durée de vie des applications.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: 4d435984d8169b558ab026ef8541c90f7a2a96b9
ms.sourcegitcommit: 0fc89b80bb1952852ecbcf3c5c156459b02a6ceb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52618153"
---
# <a name="net-generic-host"></a><span data-ttu-id="9f42a-103">Hôte générique .NET</span><span class="sxs-lookup"><span data-stu-id="9f42a-103">.NET Generic Host</span></span>

<span data-ttu-id="9f42a-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9f42a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9f42a-105">Les applications .NET Core configurent et lancent un *hôte*.</span><span class="sxs-lookup"><span data-stu-id="9f42a-105">.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="9f42a-106">L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="9f42a-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="9f42a-107">Cette rubrique traite de l’hôte générique ASP.NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>), utile pour l’hébergement d’applications qui ne traitent pas les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="9f42a-107">This topic covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="9f42a-108">Pour en savoir plus sur l’hôte web (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>), consultez <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="9f42a-108">For coverage of the Web Host (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>), see <xref:fundamentals/host/web-host>.</span></span>

<span data-ttu-id="9f42a-109">L’objectif de l’hôte générique est de séparer le pipeline HTTP de l’API d’hôte web pour permettre un plus large éventail de scénarios d’hôte.</span><span class="sxs-lookup"><span data-stu-id="9f42a-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="9f42a-110">La messagerie, les tâches en arrière-plan et autres charges de travail non-HTTP basées sur l’hôte générique bénéficient de fonctionnalités transversales, comme la configuration, l’injection de dépendances (DI) et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="9f42a-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="9f42a-111">L’hôte générique est nouveau dans ASP.NET Core 2.1 et n’est pas adapté aux scénarios d’hébergement web.</span><span class="sxs-lookup"><span data-stu-id="9f42a-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="9f42a-112">Pour les scénarios d’hébergement de web, utilisez l’[hôte web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="9f42a-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="9f42a-113">L’hôte générique est en cours de développement pour remplacer l’hôte web dans une version ultérieure et servir d’API hôte principale dans les scénarios HTTP et non-HTTP.</span><span class="sxs-lookup"><span data-stu-id="9f42a-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="9f42a-114">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9f42a-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9f42a-115">Quand vous exécutez l’exemple d’application dans [Visual Studio Code](https://code.visualstudio.com/), utilisez un *terminal externe ou intégré*.</span><span class="sxs-lookup"><span data-stu-id="9f42a-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="9f42a-116">N’exécutez pas l’exemple dans une `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="9f42a-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="9f42a-117">Pour définir la console dans Visual Studio Code :</span><span class="sxs-lookup"><span data-stu-id="9f42a-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="9f42a-118">Ouvrez le fichier *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="9f42a-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="9f42a-119">Dans la configuration **.NET Core Launch (console)**, recherchez l’entrée **console**.</span><span class="sxs-lookup"><span data-stu-id="9f42a-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="9f42a-120">Définissez la valeur avec `externalTerminal` ou `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="9f42a-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="9f42a-121">Introduction</span><span class="sxs-lookup"><span data-stu-id="9f42a-121">Introduction</span></span>

<span data-ttu-id="9f42a-122">La bibliothèque de l’hôte générique est disponible dans l’espace de noms <xref:Microsoft.Extensions.Hosting> et est fournie par le package [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span><span class="sxs-lookup"><span data-stu-id="9f42a-122">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="9f42a-123">Le package `Microsoft.Extensions.Hosting` est inclus dans le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 ou ultérieur).</span><span class="sxs-lookup"><span data-stu-id="9f42a-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="9f42a-124"><xref:Microsoft.Extensions.Hosting.IHostedService> est le point d’entrée pour exécuter le code.</span><span class="sxs-lookup"><span data-stu-id="9f42a-124"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="9f42a-125">Chaque implémentation `IHostedService` est exécutée dans l’ordre d’[inscription des services dans ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="9f42a-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="9f42a-126"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> est appelé sur chaque `IHostedService` au démarrage de l’hôte, tandis que <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> est appelé dans l’ordre d’inscription inverse quand l’hôte s’arrête normalement.</span><span class="sxs-lookup"><span data-stu-id="9f42a-126"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each `IHostedService` when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="9f42a-127">Configurer un hôte</span><span class="sxs-lookup"><span data-stu-id="9f42a-127">Set up a host</span></span>

<span data-ttu-id="9f42a-128"><xref:Microsoft.Extensions.Hosting.IHostBuilder> est le principal composant que les applications et les bibliothèques utilisent pour initialiser, générer et exécuter l’hôte :</span><span class="sxs-lookup"><span data-stu-id="9f42a-128"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="9f42a-129">Options</span><span class="sxs-lookup"><span data-stu-id="9f42a-129">Options</span></span>

<span data-ttu-id="9f42a-130"><xref:Microsoft.Extensions.Hosting.HostOptions> configure les options pour <xref:Microsoft.Extensions.Hosting.IHost>.</span><span class="sxs-lookup"><span data-stu-id="9f42a-130"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="9f42a-131">Délai d’expiration</span><span class="sxs-lookup"><span data-stu-id="9f42a-131">Shutdown timeout</span></span>

<span data-ttu-id="9f42a-132"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> définit le délai d’expiration pour <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="9f42a-132"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="9f42a-133">La valeur par défaut est de cinq secondes.</span><span class="sxs-lookup"><span data-stu-id="9f42a-133">The default value is five seconds.</span></span>

<span data-ttu-id="9f42a-134">La configuration de l’option suivante dans `Program.Main` augmente le délai d’expiration par défaut de 5 à 20 secondes :</span><span class="sxs-lookup"><span data-stu-id="9f42a-134">The following option configuration in `Program.Main` increases the default five second shutdown timeout to 20 seconds:</span></span>

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

## <a name="default-services"></a><span data-ttu-id="9f42a-135">Services par défaut</span><span class="sxs-lookup"><span data-stu-id="9f42a-135">Default services</span></span>

<span data-ttu-id="9f42a-136">Les services suivants sont inscrits au moment de l’initialisation de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="9f42a-136">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="9f42a-137">[Environnement](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="9f42a-137">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="9f42a-138">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="9f42a-138">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="9f42a-139"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="9f42a-139"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="9f42a-140"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="9f42a-140"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="9f42a-141">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="9f42a-141">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="9f42a-142">[Journalisation](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="9f42a-142">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="9f42a-143">Configuration de l’hôte</span><span class="sxs-lookup"><span data-stu-id="9f42a-143">Host configuration</span></span>

<span data-ttu-id="9f42a-144">La création de la configuration d’hôte fait appel aux opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="9f42a-144">Host configuration is created by:</span></span>

* <span data-ttu-id="9f42a-145">Appel de méthodes d’extension sur <xref:Microsoft.Extensions.Hosting.IHostBuilder> pour définir la [racine du contenu](#content-root) et [l’environnement](#environment).</span><span class="sxs-lookup"><span data-stu-id="9f42a-145">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="9f42a-146">Lecture de la configuration à partir des fournisseurs de configuration dans <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="9f42a-146">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="9f42a-147">Méthodes d’extension</span><span class="sxs-lookup"><span data-stu-id="9f42a-147">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="9f42a-148">Clé d’application (nom)</span><span class="sxs-lookup"><span data-stu-id="9f42a-148">Application key (name)</span></span>

<span data-ttu-id="9f42a-149">La propriété [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) est définie à partir de la configuration de l’hôte pendant la construction de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="9f42a-149">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="9f42a-150">Pour définir la valeur explicitement, utilisez [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey) :</span><span class="sxs-lookup"><span data-stu-id="9f42a-150">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="9f42a-151">**Clé** : applicationName</span><span class="sxs-lookup"><span data-stu-id="9f42a-151">**Key**: applicationName</span></span>  
<span data-ttu-id="9f42a-152">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="9f42a-152">**Type**: *string*</span></span>  
<span data-ttu-id="9f42a-153">**Par défaut** : nom de l’assembly contenant le point d’entrée de l’application.</span><span class="sxs-lookup"><span data-stu-id="9f42a-153">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="9f42a-154">**Définition avec** : `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="9f42a-154">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="9f42a-155">**Variable d’environnement** : `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` est [facultatif et défini par l’utilisateur](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="9f42a-155">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="9f42a-156">Racine de contenu</span><span class="sxs-lookup"><span data-stu-id="9f42a-156">Content root</span></span>

<span data-ttu-id="9f42a-157">Ce paramètre détermine où l’hôte commence la recherche des fichiers de contenu.</span><span class="sxs-lookup"><span data-stu-id="9f42a-157">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="9f42a-158">**Clé** : contentRoot</span><span class="sxs-lookup"><span data-stu-id="9f42a-158">**Key**: contentRoot</span></span>  
<span data-ttu-id="9f42a-159">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="9f42a-159">**Type**: *string*</span></span>  
<span data-ttu-id="9f42a-160">**Valeur par défaut** : dossier où réside l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="9f42a-160">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="9f42a-161">**Définition avec** : `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="9f42a-161">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="9f42a-162">**Variable d’environnement** : `<PREFIX_>CONTENTROOT` (`<PREFIX_>` est [facultatif et défini par l’utilisateur](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="9f42a-162">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="9f42a-163">Si le chemin est introuvable, l’hôte ne peut pas démarrer.</span><span class="sxs-lookup"><span data-stu-id="9f42a-163">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

### <a name="environment"></a><span data-ttu-id="9f42a-164">Environnement</span><span class="sxs-lookup"><span data-stu-id="9f42a-164">Environment</span></span>

<span data-ttu-id="9f42a-165">Définit l’[environnement](xref:fundamentals/environments) de l’application.</span><span class="sxs-lookup"><span data-stu-id="9f42a-165">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="9f42a-166">**Clé** : environment</span><span class="sxs-lookup"><span data-stu-id="9f42a-166">**Key**: environment</span></span>  
<span data-ttu-id="9f42a-167">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="9f42a-167">**Type**: *string*</span></span>  
<span data-ttu-id="9f42a-168">**Valeur par défaut** : Production</span><span class="sxs-lookup"><span data-stu-id="9f42a-168">**Default**: Production</span></span>  
<span data-ttu-id="9f42a-169">**Définition avec** : `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="9f42a-169">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="9f42a-170">**Variable d’environnement** : `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` est [facultatif et défini par l’utilisateur](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="9f42a-170">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="9f42a-171">L’environnement peut être défini à n’importe quelle valeur.</span><span class="sxs-lookup"><span data-stu-id="9f42a-171">The environment can be set to any value.</span></span> <span data-ttu-id="9f42a-172">Les valeurs définies par le framework sont `Development`, `Staging` et `Production`.</span><span class="sxs-lookup"><span data-stu-id="9f42a-172">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="9f42a-173">Les valeurs ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="9f42a-173">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="9f42a-174">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="9f42a-174">ConfigureHostConfiguration</span></span>

<span data-ttu-id="9f42a-175">`ConfigureHostConfiguration` utilise un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> pour créer un <xref:Microsoft.Extensions.Configuration.IConfiguration> pour l’hôte.</span><span class="sxs-lookup"><span data-stu-id="9f42a-175">`ConfigureHostConfiguration` uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="9f42a-176">La configuration d’hôte permet d’initialiser le <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> en vue de son utilisation dans le processus de génération de l’application.</span><span class="sxs-lookup"><span data-stu-id="9f42a-176">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="9f42a-177">`ConfigureHostConfiguration` peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="9f42a-177">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="9f42a-178">L’hôte utilise l’option qui définit une valeur en dernier sur une clé donnée.</span><span class="sxs-lookup"><span data-stu-id="9f42a-178">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="9f42a-179">La configuration d’hôte est automatiquement acheminée à la configuration de l’application ([ConfigureAppConfiguration](#configureappconfiguration) et le reste de l’application).</span><span class="sxs-lookup"><span data-stu-id="9f42a-179">Host configuration automatically flows to app configuration ([ConfigureAppConfiguration](#configureappconfiguration) and the rest of the app).</span></span>

<span data-ttu-id="9f42a-180">Aucun fournisseur n’est inclus par défaut.</span><span class="sxs-lookup"><span data-stu-id="9f42a-180">No providers are included by default.</span></span> <span data-ttu-id="9f42a-181">Vous devez spécifier explicitement les fournisseurs de configuration dont l’application a besoin dans `ConfigureHostConfiguration`, notamment :</span><span class="sxs-lookup"><span data-stu-id="9f42a-181">You must explicitly specify whatever configuration providers the app requires in `ConfigureHostConfiguration`, including:</span></span>

* <span data-ttu-id="9f42a-182">Configuration du fichier (par exemple, à partir d’un fichier *hostsettings.json*).</span><span class="sxs-lookup"><span data-stu-id="9f42a-182">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="9f42a-183">Configuration de la variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="9f42a-183">Environment variable configuration.</span></span>
* <span data-ttu-id="9f42a-184">Configuration de l’argument de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="9f42a-184">Command-line argument configuration.</span></span>
* <span data-ttu-id="9f42a-185">Tout autre fournisseur de configuration obligatoire.</span><span class="sxs-lookup"><span data-stu-id="9f42a-185">Any other required configuration providers.</span></span>

<span data-ttu-id="9f42a-186">Pour activer la configuration de fichier de l’hôte, spécifiez le chemin de base de l’application avec `SetBasePath`, suivi d’un appel à l’un des [fournisseurs de configuration de fichier](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="9f42a-186">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="9f42a-187">L’exemple d’application utilise un fichier JSON, *hostsettings.json*, et appelle <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> pour utiliser les paramètres de configuration d’hôte du fichier.</span><span class="sxs-lookup"><span data-stu-id="9f42a-187">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="9f42a-188">Pour ajouter la [configuration de variable d’environnement](xref:fundamentals/configuration/index#environment-variables-configuration-provider) de l’hôte, appelez <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> sur le générateur d’hôte.</span><span class="sxs-lookup"><span data-stu-id="9f42a-188">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="9f42a-189">`AddEnvironmentVariables` accepte un préfixe facultatif défini par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="9f42a-189">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="9f42a-190">L’exemple d’application utilise un préfixe `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="9f42a-190">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="9f42a-191">Ce préfixe est supprimé à la lecture des variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="9f42a-191">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="9f42a-192">Lorsque l’hôte de l’exemple d’application est configuré, la valeur de variable d’environnement de `PREFIX_ENVIRONMENT` devient la valeur de configuration d’hôte de la clé `environment`.</span><span class="sxs-lookup"><span data-stu-id="9f42a-192">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="9f42a-193">Pendant le développement, lorsque vous utilisez [Visual Studio](https://www.visualstudio.com/) ou que vous lancez une application avec `dotnet run`, vous pouvez définir les variables d’environnement dans le fichier *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9f42a-193">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="9f42a-194">Dans [Visual Studio Code](https://code.visualstudio.com/), elles peuvent être définies dans le fichier *.vscode/launch.json* au cours du développement.</span><span class="sxs-lookup"><span data-stu-id="9f42a-194">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="9f42a-195">Pour plus d'informations, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="9f42a-195">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="9f42a-196">Pour ajouter la [configuration de ligne de commande](xref:fundamentals/configuration/index#command-line-configuration-provider), appelez <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="9f42a-196">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="9f42a-197">La configuration de ligne de commande est ajoutée en dernier pour permettre aux arguments de ligne de commande de substituer la configuration fournie par les fournisseurs de configuration antérieurs.</span><span class="sxs-lookup"><span data-stu-id="9f42a-197">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="9f42a-198">*hostsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="9f42a-198">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="9f42a-199">Une configuration supplémentaire peut être fournie à l’aide des clés [applicationName](#application-key-name) et [contentRoot](#content-root).</span><span class="sxs-lookup"><span data-stu-id="9f42a-199">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="9f42a-200">Exemple de configuration `HostBuilder` avec `ConfigureHostConfiguration` :</span><span class="sxs-lookup"><span data-stu-id="9f42a-200">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="9f42a-201">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="9f42a-201">ConfigureAppConfiguration</span></span>

<span data-ttu-id="9f42a-202">Pour créer la configuration d’application, appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> sur l’implémentation <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="9f42a-202">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="9f42a-203">`ConfigureAppConfiguration` utilise un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> pour créer un <xref:Microsoft.Extensions.Configuration.IConfiguration> pour l’application.</span><span class="sxs-lookup"><span data-stu-id="9f42a-203">`ConfigureAppConfiguration` uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="9f42a-204">`ConfigureAppConfiguration` peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="9f42a-204">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="9f42a-205">L’application utilise l’option qui définit une valeur en dernier sur une clé donnée.</span><span class="sxs-lookup"><span data-stu-id="9f42a-205">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="9f42a-206">La configuration créée par `ConfigureAppConfiguration` est disponible dans [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) pour les opérations suivantes et dans <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span><span class="sxs-lookup"><span data-stu-id="9f42a-206">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="9f42a-207">La configuration d’application reçoit automatiquement la configuration d’hôte fournie par [ConfigureHostConfiguration](#configurehostconfiguration).</span><span class="sxs-lookup"><span data-stu-id="9f42a-207">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="9f42a-208">Exemple de configuration d’application avec `ConfigureAppConfiguration` :</span><span class="sxs-lookup"><span data-stu-id="9f42a-208">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="9f42a-209">*appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="9f42a-209">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="9f42a-210">*appsettings.Development.json* :</span><span class="sxs-lookup"><span data-stu-id="9f42a-210">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="9f42a-211">*appsettings.Production.json* :</span><span class="sxs-lookup"><span data-stu-id="9f42a-211">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="9f42a-212">Pour déplacer des fichiers de paramètres vers le répertoire de sortie, spécifiez-les en tant qu’[éléments de projet MSBuild](/visualstudio/msbuild/common-msbuild-project-items) dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="9f42a-212">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="9f42a-213">L’exemple d’application déplace ses fichiers de paramètres d’application JSON et *hostsettings.json* avec l’élément `<Content>` suivant :</span><span class="sxs-lookup"><span data-stu-id="9f42a-213">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a><span data-ttu-id="9f42a-214">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="9f42a-214">ConfigureServices</span></span>

<span data-ttu-id="9f42a-215"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> ajoute des services au conteneur [d’injection de dépendances](xref:fundamentals/dependency-injection) de l’application.</span><span class="sxs-lookup"><span data-stu-id="9f42a-215"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="9f42a-216">`ConfigureServices` peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="9f42a-216">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="9f42a-217">Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="9f42a-217">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="9f42a-218">Pour plus d'informations, consultez <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="9f42a-218">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="9f42a-219">L’[exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) utilise la méthode d’extension `AddHostedService` afin d’ajouter un service pour les événements de durée de vie, `LifetimeEventsHostedService`, et une tâche en arrière-plan chronométrée, `TimedHostedService`, à l’application :</span><span class="sxs-lookup"><span data-stu-id="9f42a-219">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="9f42a-220">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="9f42a-220">ConfigureLogging</span></span>

<span data-ttu-id="9f42a-221"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> ajoute un délégué pour configurer le <xref:Microsoft.Extensions.Logging.ILoggingBuilder> fourni.</span><span class="sxs-lookup"><span data-stu-id="9f42a-221"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="9f42a-222">`ConfigureLogging` peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="9f42a-222">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="9f42a-223">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="9f42a-223">UseConsoleLifetime</span></span>

<span data-ttu-id="9f42a-224"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> écoute `Ctrl+C`/SIGINT ou SIGTERM et appelle <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> pour démarrer le processus d’arrêt.</span><span class="sxs-lookup"><span data-stu-id="9f42a-224"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for `Ctrl+C`/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="9f42a-225">`UseConsoleLifetime` déverrouille les extensions telles que [RunAsync](#runasync) et [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="9f42a-225">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="9f42a-226"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> est préinscrit comme implémentation de durée de vie par défaut.</span><span class="sxs-lookup"><span data-stu-id="9f42a-226"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="9f42a-227">La dernière durée de vie inscrite est utilisée.</span><span class="sxs-lookup"><span data-stu-id="9f42a-227">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="9f42a-228">Configuration du conteneur</span><span class="sxs-lookup"><span data-stu-id="9f42a-228">Container configuration</span></span>

<span data-ttu-id="9f42a-229">Pour prendre en charge le branchement dans d’autres conteneurs, l’hôte peut accepter un <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory`1>.</span><span class="sxs-lookup"><span data-stu-id="9f42a-229">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory`1>.</span></span> <span data-ttu-id="9f42a-230">L’ajout d’une fabrique ne fait pas partie de l’inscription de conteneur DI mais est plutôt une tâche un intrinsèque à l’hôte utilisée pour créer le conteneur DI concret.</span><span class="sxs-lookup"><span data-stu-id="9f42a-230">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="9f42a-231">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) remplace la fabrique par défaut utilisée pour créer le fournisseur de services de l’application.</span><span class="sxs-lookup"><span data-stu-id="9f42a-231">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="9f42a-232">La configuration de conteneur personnalisée est gérée par la méthode <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>.</span><span class="sxs-lookup"><span data-stu-id="9f42a-232">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="9f42a-233">`ConfigureContainer` offre une expérience fortement typée pour configurer le conteneur sur l’API hôte sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="9f42a-233">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="9f42a-234">`ConfigureContainer` peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="9f42a-234">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="9f42a-235">Créer un conteneur de service pour l’application :</span><span class="sxs-lookup"><span data-stu-id="9f42a-235">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="9f42a-236">Fournir une fabrique de conteneur de service :</span><span class="sxs-lookup"><span data-stu-id="9f42a-236">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="9f42a-237">Utiliser la fabrique et configurer le conteneur de service personnalisé pour l’application :</span><span class="sxs-lookup"><span data-stu-id="9f42a-237">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="9f42a-238">Extensibilité</span><span class="sxs-lookup"><span data-stu-id="9f42a-238">Extensibility</span></span>

<span data-ttu-id="9f42a-239">L’extensibilité de l’hôte est effectuée avec les méthodes d’extension sur `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9f42a-239">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="9f42a-240">L’exemple suivant montre comment une méthode d’extension étend une implémentation `IHostBuilder` avec l’exemple [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) démontré dans <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="9f42a-240">The following example shows how an extension method extends an `IHostBuilder` implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="9f42a-241">Une application établit la méthode d'extension `UseHostedService` pour inscrire le service hébergé passé dans `T` :</span><span class="sxs-lookup"><span data-stu-id="9f42a-241">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="9f42a-242">Gérer l’hôte</span><span class="sxs-lookup"><span data-stu-id="9f42a-242">Manage the host</span></span>

<span data-ttu-id="9f42a-243">L’implémentation <xref:Microsoft.Extensions.Hosting.IHost> est chargée de démarrer et d’arrêter les implémentations `IHostedService` qui sont inscrites dans le conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="9f42a-243">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="9f42a-244">Exécuter</span><span class="sxs-lookup"><span data-stu-id="9f42a-244">Run</span></span>

<span data-ttu-id="9f42a-245"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> exécute l’application et bloque le thread appelant jusqu’à l’arrêt de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="9f42a-245"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="9f42a-246">RunAsync</span><span class="sxs-lookup"><span data-stu-id="9f42a-246">RunAsync</span></span>

<span data-ttu-id="9f42a-247"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> exécute l’application et retourne une `Task` qui est effectuée quand le jeton d’annulation ou l’arrêt est déclenché :</span><span class="sxs-lookup"><span data-stu-id="9f42a-247"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="9f42a-248">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="9f42a-248">RunConsoleAsync</span></span>

<span data-ttu-id="9f42a-249"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> permet la prise en charge de la console, génère et démarre l’hôte et attend `Ctrl+C`/SIGINT ou SIGTERM pour l’arrêter.</span><span class="sxs-lookup"><span data-stu-id="9f42a-249"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="9f42a-250">Start et StopAsync</span><span class="sxs-lookup"><span data-stu-id="9f42a-250">Start and StopAsync</span></span>

<span data-ttu-id="9f42a-251"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> démarre l’hôte en mode synchrone.</span><span class="sxs-lookup"><span data-stu-id="9f42a-251"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="9f42a-252"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> tente d’arrêter l’hôte dans le délai d’attente fourni.</span><span class="sxs-lookup"><span data-stu-id="9f42a-252"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="9f42a-253">StartAsync et StopAsync</span><span class="sxs-lookup"><span data-stu-id="9f42a-253">StartAsync and StopAsync</span></span>

<span data-ttu-id="9f42a-254"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> démarre l’application.</span><span class="sxs-lookup"><span data-stu-id="9f42a-254"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="9f42a-255"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> arrête l’application.</span><span class="sxs-lookup"><span data-stu-id="9f42a-255"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="9f42a-256">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="9f42a-256">WaitForShutdown</span></span>

<span data-ttu-id="9f42a-257"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> est déclenché par le biais de <xref:Microsoft.Extensions.Hosting.IHostLifetime>, par exemple <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (écoute `Ctrl+C`/SIGINT ou SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="9f42a-257"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="9f42a-258">`WaitForShutdown` appelle <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="9f42a-258">`WaitForShutdown` calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="9f42a-259">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="9f42a-259">WaitForShutdownAsync</span></span>

<span data-ttu-id="9f42a-260"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> retourne une `Task` qui est effectuée quand l’arrêt est déclenché par le biais du jeton fourni et appelle <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="9f42a-260"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a `Task` that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="external-control"></a><span data-ttu-id="9f42a-261">Contrôle externe</span><span class="sxs-lookup"><span data-stu-id="9f42a-261">External control</span></span>

<span data-ttu-id="9f42a-262">Le contrôle externe de l’hôte peut être effectué à l’aide de méthodes pouvant être appelées de façon externe :</span><span class="sxs-lookup"><span data-stu-id="9f42a-262">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="9f42a-263"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> est appelé au début de <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, lequel attend qu’il soit fini avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="9f42a-263"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="9f42a-264">Cela permet de retarder le démarrage jusqu'à ce que celui-ci soit signalé par un événement externe.</span><span class="sxs-lookup"><span data-stu-id="9f42a-264">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="9f42a-265">Interface IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="9f42a-265">IHostingEnvironment interface</span></span>

<span data-ttu-id="9f42a-266"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> fournit des informations sur l’environnement d’hébergement de l’application.</span><span class="sxs-lookup"><span data-stu-id="9f42a-266"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="9f42a-267">Utilisez [l’injection de constructeur](xref:fundamentals/dependency-injection) pour obtenir l’interface `IHostingEnvironment` afin d’utiliser ses propriétés et méthodes d’extension :</span><span class="sxs-lookup"><span data-stu-id="9f42a-267">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="9f42a-268">Pour plus d'informations, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="9f42a-268">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="9f42a-269">Interface IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="9f42a-269">IApplicationLifetime interface</span></span>

<span data-ttu-id="9f42a-270"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> s’utilise pour les activités de post-démarrage et d’arrêt, notamment pour les demandes d’arrêt normal.</span><span class="sxs-lookup"><span data-stu-id="9f42a-270"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="9f42a-271">Trois propriétés de l’interface sont des jetons d’annulation utilisés pour inscrire les méthodes `Action` qui définissent les événements de démarrage et d’arrêt.</span><span class="sxs-lookup"><span data-stu-id="9f42a-271">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="9f42a-272">Jeton d’annulation</span><span class="sxs-lookup"><span data-stu-id="9f42a-272">Cancellation Token</span></span> | <span data-ttu-id="9f42a-273">Événement déclencheur</span><span class="sxs-lookup"><span data-stu-id="9f42a-273">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="9f42a-274">L’hôte a complètement démarré.</span><span class="sxs-lookup"><span data-stu-id="9f42a-274">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="9f42a-275">L’hôte effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="9f42a-275">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="9f42a-276">Toutes les requêtes doivent être traitées.</span><span class="sxs-lookup"><span data-stu-id="9f42a-276">All requests should be processed.</span></span> <span data-ttu-id="9f42a-277">L’arrêt est bloqué tant que cet événement n’est pas terminé.</span><span class="sxs-lookup"><span data-stu-id="9f42a-277">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="9f42a-278">L’hôte effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="9f42a-278">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="9f42a-279">Des requêtes peuvent encore être en cours de traitement.</span><span class="sxs-lookup"><span data-stu-id="9f42a-279">Requests may still be processing.</span></span> <span data-ttu-id="9f42a-280">L’arrêt est bloqué tant que cet événement n’est pas terminé.</span><span class="sxs-lookup"><span data-stu-id="9f42a-280">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="9f42a-281">Le constructeur injecte le service `IApplicationLifetime` dans une classe.</span><span class="sxs-lookup"><span data-stu-id="9f42a-281">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="9f42a-282">[L’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) utilise l’injection de constructeur dans une classe `LifetimeEventsHostedService` (une implémentation `IHostedService`) pour inscrire les événements.</span><span class="sxs-lookup"><span data-stu-id="9f42a-282">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="9f42a-283">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="9f42a-283">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="9f42a-284"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> demande l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="9f42a-284"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="9f42a-285">La classe suivante utilise `StopApplication` pour arrêter normalement une application quand la méthode `Shutdown` de la classe est appelée :</span><span class="sxs-lookup"><span data-stu-id="9f42a-285">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="9f42a-286">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9f42a-286">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
* [<span data-ttu-id="9f42a-287">Hosting repo samples on GitHub</span><span class="sxs-lookup"><span data-stu-id="9f42a-287">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
