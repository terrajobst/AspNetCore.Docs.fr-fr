---
title: Hôte générique .NET
author: guardrex
description: Découvrez l’hôte générique dans .NET, responsable de la gestion du démarrage et de la durée de vie des applications.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/generic-host
ms.openlocfilehash: 15f4a689b2756d2bfb6320ab31f2e8d63af51a09
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="net-generic-host"></a><span data-ttu-id="61b95-103">Hôte générique .NET</span><span class="sxs-lookup"><span data-stu-id="61b95-103">.NET Generic Host</span></span>

<span data-ttu-id="61b95-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="61b95-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="61b95-105">Les applications .NET configurent et lancent un *hôte*.</span><span class="sxs-lookup"><span data-stu-id="61b95-105">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="61b95-106">L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="61b95-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="61b95-107">Cette rubrique traite de l’hôte générique ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), utile pour l’hébergement d’applications qui ne traitent pas les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="61b95-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="61b95-108">Pour en savoir plus sur l’hôte web ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), consultez la rubrique [Hôte web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="61b95-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>

<span data-ttu-id="61b95-109">L’objectif de l’hôte générique est de séparer le pipeline HTTP de l’API d’hôte web pour permettre un plus large éventail de scénarios d’hôte.</span><span class="sxs-lookup"><span data-stu-id="61b95-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="61b95-110">La messagerie, les tâches en arrière-plan et autres charges de travail non-HTTP basées sur l’hôte générique bénéficient de fonctionnalités transversales, comme la configuration, l’injection de dépendances (DI) et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="61b95-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="61b95-111">L’hôte générique est nouveau dans ASP.NET Core 2.1 et n’est pas adapté aux scénarios d’hébergement web.</span><span class="sxs-lookup"><span data-stu-id="61b95-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="61b95-112">Pour les scénarios d’hébergement de web, utilisez l’[hôte web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="61b95-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="61b95-113">L’hôte générique est en cours de développement pour remplacer l’hôte web dans une version ultérieure et servir d’API hôte principale dans les scénarios HTTP et non-HTTP.</span><span class="sxs-lookup"><span data-stu-id="61b95-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="61b95-114">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="61b95-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="61b95-115">Quand vous exécutez l’exemple d’application dans [Visual Studio Code](https://code.visualstudio.com/), utilisez un *terminal externe ou intégré*.</span><span class="sxs-lookup"><span data-stu-id="61b95-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="61b95-116">N’exécutez pas l’exemple dans une `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="61b95-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="61b95-117">Pour définir la console dans Visual Studio Code :</span><span class="sxs-lookup"><span data-stu-id="61b95-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="61b95-118">Ouvrez le fichier *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="61b95-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="61b95-119">Dans la configuration **.NET Core Launch (console)**, recherchez l’entrée **console**.</span><span class="sxs-lookup"><span data-stu-id="61b95-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="61b95-120">Définissez la valeur avec `externalTerminal` ou `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="61b95-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="61b95-121">Introduction</span><span class="sxs-lookup"><span data-stu-id="61b95-121">Introduction</span></span>

<span data-ttu-id="61b95-122">La bibliothèque de l’hôte générique est disponible dans l’[espace de noms Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) et fournie par le [package NuGet Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span><span class="sxs-lookup"><span data-stu-id="61b95-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting NuGet package](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span></span> <span data-ttu-id="61b95-123">Le package `Microsoft.Extensions.Hosting` est inclus dans le métapackage [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="61b95-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) metapackage.</span></span>

<span data-ttu-id="61b95-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) est le point d’entrée pour exécuter le code.</span><span class="sxs-lookup"><span data-stu-id="61b95-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="61b95-125">Chaque implémentation `IHostedService` est exécutée dans l’ordre d’[inscription des services dans ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="61b95-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="61b95-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) est appelé sur chaque `IHostedService` au démarrage de l’hôte, tandis que [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) est appelé dans l’ordre d’inscription inverse quand l’hôte s’arrête normalement.</span><span class="sxs-lookup"><span data-stu-id="61b95-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="61b95-127">Configurer un hôte</span><span class="sxs-lookup"><span data-stu-id="61b95-127">Set up a host</span></span>

<span data-ttu-id="61b95-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) est le principal composant que les applications et les bibliothèques utilisent pour initialiser, générer et exécuter l’hôte :</span><span class="sxs-lookup"><span data-stu-id="61b95-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a><span data-ttu-id="61b95-129">Configuration de l’hôte</span><span class="sxs-lookup"><span data-stu-id="61b95-129">Host configuration</span></span>

<span data-ttu-id="61b95-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) s’appuie sur les approches suivantes pour définir les valeurs de configuration de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="61b95-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="61b95-131">Générateur de configuration</span><span class="sxs-lookup"><span data-stu-id="61b95-131">Configuration builder</span></span>
* <span data-ttu-id="61b95-132">Configuration de méthode d’extension</span><span class="sxs-lookup"><span data-stu-id="61b95-132">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="61b95-133">Générateur de configuration</span><span class="sxs-lookup"><span data-stu-id="61b95-133">Configuration builder</span></span>

<span data-ttu-id="61b95-134">Le générateur de configuration d’hôte est créé en appelant [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) dans l’implémentation [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder).</span><span class="sxs-lookup"><span data-stu-id="61b95-134">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="61b95-135">`ConfigureHostConfiguration` utilise un [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) afin de créer une [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) pour l’hôte.</span><span class="sxs-lookup"><span data-stu-id="61b95-135">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="61b95-136">Le générateur de configuration initialise les [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) pour les utiliser dans le processus de génération de l’application.</span><span class="sxs-lookup"><span data-stu-id="61b95-136">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span> <span data-ttu-id="61b95-137">`ConfigureHostConfiguration` peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="61b95-137">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="61b95-138">L’hôte utilise l’option qui définit une valeur en dernier.</span><span class="sxs-lookup"><span data-stu-id="61b95-138">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="61b95-139">*hostsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="61b95-139">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="61b95-140">Exemple de configuration `HostBuilder` avec `ConfigureHostConfiguration` :</span><span class="sxs-lookup"><span data-stu-id="61b95-140">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="61b95-141">Actuellement, la méthode d’extension [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) n’est pas capable d’analyser une section de configuration retournée par [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (par exemple, `.AddConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="61b95-141">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="61b95-142">La méthode `GetSection` filtre les clés de configuration dans la section demandée, mais laisse le nom de la section dans les clés (par exemple, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="61b95-142">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="61b95-143">La méthode `AddConfiguration` suppose que les clés correspondent aux clés `HostBuilder` (par exemple, `environment`).</span><span class="sxs-lookup"><span data-stu-id="61b95-143">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="61b95-144">La présence du nom de la section dans les clés empêche l’utilisation des valeurs de la section pour configurer l’hôte.</span><span class="sxs-lookup"><span data-stu-id="61b95-144">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="61b95-145">Ce problème sera résolu dans une prochaine mise en production.</span><span class="sxs-lookup"><span data-stu-id="61b95-145">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="61b95-146">Pour obtenir plus d’informations et connaître les solutions de contournement possibles, consultez [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="61b95-146">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="61b95-147">Configuration de méthode d’extension</span><span class="sxs-lookup"><span data-stu-id="61b95-147">Extension method configuration</span></span>

<span data-ttu-id="61b95-148">Les méthodes d’extension sont appelées sur l’implémentation `IHostBuilder` pour configurer la racine de contenu et l’environnement.</span><span class="sxs-lookup"><span data-stu-id="61b95-148">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="content-root"></a><span data-ttu-id="61b95-149">Racine de contenu</span><span class="sxs-lookup"><span data-stu-id="61b95-149">Content Root</span></span>

<span data-ttu-id="61b95-150">Ce paramètre détermine où l’hôte commence la recherche des fichiers de contenu.</span><span class="sxs-lookup"><span data-stu-id="61b95-150">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="61b95-151">**Clé** : contentRoot</span><span class="sxs-lookup"><span data-stu-id="61b95-151">**Key**: contentRoot</span></span>  
<span data-ttu-id="61b95-152">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="61b95-152">**Type**: *string*</span></span>  
<span data-ttu-id="61b95-153">**Valeur par défaut** : dossier où réside l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="61b95-153">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="61b95-154">**Définition avec** : `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="61b95-154">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="61b95-155">**Variable d’environnement** : `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="61b95-155">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="61b95-156">Si le chemin est introuvable, l’hôte ne peut pas démarrer.</span><span class="sxs-lookup"><span data-stu-id="61b95-156">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="61b95-157">Environnement</span><span class="sxs-lookup"><span data-stu-id="61b95-157">Environment</span></span>

<span data-ttu-id="61b95-158">Définit l’[environnement](xref:fundamentals/environments) de l’application.</span><span class="sxs-lookup"><span data-stu-id="61b95-158">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="61b95-159">**Clé** : environment</span><span class="sxs-lookup"><span data-stu-id="61b95-159">**Key**: environment</span></span>  
<span data-ttu-id="61b95-160">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="61b95-160">**Type**: *string*</span></span>  
<span data-ttu-id="61b95-161">**Valeur par défaut** : Production</span><span class="sxs-lookup"><span data-stu-id="61b95-161">**Default**: Production</span></span>  
<span data-ttu-id="61b95-162">**Définition avec** : `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="61b95-162">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="61b95-163">**Variable d’environnement** : `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="61b95-163">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="61b95-164">L’environnement peut être défini à n’importe quelle valeur.</span><span class="sxs-lookup"><span data-stu-id="61b95-164">The environment can be set to any value.</span></span> <span data-ttu-id="61b95-165">Les valeurs définies par le framework sont `Development`, `Staging` et `Production`.</span><span class="sxs-lookup"><span data-stu-id="61b95-165">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="61b95-166">Les valeurs ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="61b95-166">Values aren't case sensitive.</span></span> <span data-ttu-id="61b95-167">Par défaut, *l’environnement* est fourni par la variable d’environnement `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="61b95-167">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="61b95-168">Si vous utilisez [Visual Studio](https://www.visualstudio.com/), les variables d’environnement peuvent être définies dans le fichier *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="61b95-168">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="61b95-169">Pour plus d’informations, consultez [Utiliser plusieurs environnements](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="61b95-169">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="61b95-170">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="61b95-170">ConfigureAppConfiguration</span></span>

<span data-ttu-id="61b95-171">Le générateur de configuration d’application est créé en appelant [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) dans l’implémentation [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder).</span><span class="sxs-lookup"><span data-stu-id="61b95-171">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="61b95-172">`ConfigureAppConfiguration` utilise un [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) afin de créer une [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) pour l’application.</span><span class="sxs-lookup"><span data-stu-id="61b95-172">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="61b95-173">`ConfigureAppConfiguration` peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="61b95-173">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="61b95-174">L’application utilise l’option qui définit une valeur en dernier.</span><span class="sxs-lookup"><span data-stu-id="61b95-174">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="61b95-175">La configuration créée par `ConfigureAppConfiguration` est disponible dans [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) pour les opérations suivantes et dans [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span><span class="sxs-lookup"><span data-stu-id="61b95-175">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="61b95-176">Exemple de configuration d’application avec `ConfigureAppConfiguration` :</span><span class="sxs-lookup"><span data-stu-id="61b95-176">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="61b95-177">*appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="61b95-177">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="61b95-178">*appsettings.Development.json* :</span><span class="sxs-lookup"><span data-stu-id="61b95-178">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="61b95-179">*appsettings.Production.json* :</span><span class="sxs-lookup"><span data-stu-id="61b95-179">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="61b95-180">Actuellement, la méthode d’extension [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) n’est pas capable d’analyser une section de configuration retournée par [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (par exemple, `.AddConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="61b95-180">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="61b95-181">La méthode `GetSection` filtre les clés de configuration dans la section demandée, mais laisse le nom de la section dans les clés (par exemple, `section:Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="61b95-181">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="61b95-182">La méthode `AddConfiguration` attend une correspondance exacte pour les clés de configuration (par exemple, `Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="61b95-182">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="61b95-183">La présence du nom de la section dans les clés empêche l’utilisation des valeurs de la section pour configurer l’application.</span><span class="sxs-lookup"><span data-stu-id="61b95-183">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="61b95-184">Ce problème sera résolu dans une prochaine mise en production.</span><span class="sxs-lookup"><span data-stu-id="61b95-184">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="61b95-185">Pour obtenir plus d’informations et connaître les solutions de contournement possibles, consultez [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="61b95-185">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

## <a name="configureservices"></a><span data-ttu-id="61b95-186">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="61b95-186">ConfigureServices</span></span>

<span data-ttu-id="61b95-187">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) ajoute des services au conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) de l’application.</span><span class="sxs-lookup"><span data-stu-id="61b95-187">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="61b95-188">`ConfigureServices` peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="61b95-188">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="61b95-189">Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="61b95-189">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="61b95-190">Pour plus d’informations, consultez la rubrique [Tâches en arrière-plan avec services hébergés](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="61b95-190">For more information, see the [Background tasks with hosted services](xref:fundamentals/host/hosted-services) topic.</span></span>

<span data-ttu-id="61b95-191">L’[exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) utilise la méthode d’extension `AddHostedService` afin d’ajouter un service pour les événements de durée de vie, `LifetimeEventsHostedService`, et une tâche en arrière-plan chronométrée, `TimedHostedService`, à l’application :</span><span class="sxs-lookup"><span data-stu-id="61b95-191">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="61b95-192">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="61b95-192">ConfigureLogging</span></span>

<span data-ttu-id="61b95-193">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) ajoute un délégué pour configurer le [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder) fourni.</span><span class="sxs-lookup"><span data-stu-id="61b95-193">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="61b95-194">`ConfigureLogging` peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="61b95-194">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="61b95-195">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="61b95-195">UseConsoleLifetime</span></span>

<span data-ttu-id="61b95-196">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) écoute `Ctrl+C`/SIGINT ou SIGTERM et appelle [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) pour démarrer le processus d’arrêt.</span><span class="sxs-lookup"><span data-stu-id="61b95-196">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="61b95-197">`UseConsoleLifetime` déverrouille les extensions telles que [RunAsync](#runasync) et [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="61b95-197">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="61b95-198">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) est préinscrit en tant qu’implémentation de durée de vie par défaut.</span><span class="sxs-lookup"><span data-stu-id="61b95-198">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="61b95-199">La dernière durée de vie inscrite est utilisée.</span><span class="sxs-lookup"><span data-stu-id="61b95-199">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="61b95-200">Configuration du conteneur</span><span class="sxs-lookup"><span data-stu-id="61b95-200">Container configuration</span></span>

<span data-ttu-id="61b95-201">Pour prendre en charge le branchement dans d’autres conteneurs, l’hôte peut accepter un [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span><span class="sxs-lookup"><span data-stu-id="61b95-201">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="61b95-202">L’ajout d’une fabrique ne fait pas partie de l’inscription de conteneur DI mais est plutôt une tâche un intrinsèque à l’hôte utilisée pour créer le conteneur DI concret.</span><span class="sxs-lookup"><span data-stu-id="61b95-202">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="61b95-203">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) remplace la fabrique par défaut utilisée pour créer le fournisseur de services de l’application.</span><span class="sxs-lookup"><span data-stu-id="61b95-203">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="61b95-204">La configuration de conteneur personnalisée est gérée par la méthode [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer).</span><span class="sxs-lookup"><span data-stu-id="61b95-204">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="61b95-205">`ConfigureContainer` offre une expérience fortement typée pour configurer le conteneur sur l’API hôte sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="61b95-205">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="61b95-206">`ConfigureContainer` peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="61b95-206">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="61b95-207">Créer un conteneur de service pour l’application :</span><span class="sxs-lookup"><span data-stu-id="61b95-207">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="61b95-208">Fournir une fabrique de conteneur de service :</span><span class="sxs-lookup"><span data-stu-id="61b95-208">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="61b95-209">Utiliser la fabrique et configurer le conteneur de service personnalisé pour l’application :</span><span class="sxs-lookup"><span data-stu-id="61b95-209">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="61b95-210">Extensibilité</span><span class="sxs-lookup"><span data-stu-id="61b95-210">Extensibility</span></span>

<span data-ttu-id="61b95-211">L’extensibilité de l’hôte est effectuée avec les méthodes d’extension sur `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="61b95-211">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="61b95-212">L’exemple suivant montre comment une méthode d’extension étend une implémentation `IHostBuilder` avec [RabbitMQ](https://www.rabbitmq.com/).</span><span class="sxs-lookup"><span data-stu-id="61b95-212">The following example shows how an extension method extends an `IHostBuilder` implementation with [RabbitMQ](https://www.rabbitmq.com/).</span></span> <span data-ttu-id="61b95-213">La méthode d’extension (ailleurs dans l’application) inscrit un `IHostedService` RabbitMQ :</span><span class="sxs-lookup"><span data-stu-id="61b95-213">The extension method (elsewhere in the app) registers a RabbitMQ `IHostedService`:</span></span>

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a><span data-ttu-id="61b95-214">Gérer l’hôte</span><span class="sxs-lookup"><span data-stu-id="61b95-214">Manage the host</span></span>

<span data-ttu-id="61b95-215">L’implémentation [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) est chargée de démarrer et d’arrêter les implémentations `IHostedService` qui sont inscrites dans le conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="61b95-215">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="61b95-216">Exécuter</span><span class="sxs-lookup"><span data-stu-id="61b95-216">Run</span></span>

<span data-ttu-id="61b95-217">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) exécute l’application et bloque le thread appelant jusqu’à l’arrêt de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="61b95-217">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="61b95-218">RunAsync</span><span class="sxs-lookup"><span data-stu-id="61b95-218">RunAsync</span></span>

<span data-ttu-id="61b95-219">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) exécute l’application et retourne une `Task` qui est effectuée quand le jeton d’annulation ou l’arrêt est déclenché :</span><span class="sxs-lookup"><span data-stu-id="61b95-219">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="61b95-220">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="61b95-220">RunConsoleAsync</span></span>

<span data-ttu-id="61b95-221">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) permet la prise en charge de la console, génère et démarre l’hôte et attend `Ctrl+C`/SIGINT ou SIGTERM pour l’arrêter.</span><span class="sxs-lookup"><span data-stu-id="61b95-221">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        await host.RunConsoleAsync();
    }
}
```

### <a name="start-and-stopasync"></a><span data-ttu-id="61b95-222">Start et StopAsync</span><span class="sxs-lookup"><span data-stu-id="61b95-222">Start and StopAsync</span></span>

<span data-ttu-id="61b95-223">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) démarre l’hôte en mode synchrone.</span><span class="sxs-lookup"><span data-stu-id="61b95-223">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="61b95-224">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) tente d’arrêter l’hôte dans le délai d’attente fourni.</span><span class="sxs-lookup"><span data-stu-id="61b95-224">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="61b95-225">StartAsync et StopAsync</span><span class="sxs-lookup"><span data-stu-id="61b95-225">StartAsync and StopAsync</span></span>

<span data-ttu-id="61b95-226">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) démarre l’application.</span><span class="sxs-lookup"><span data-stu-id="61b95-226">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="61b95-227">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) arrête l’application.</span><span class="sxs-lookup"><span data-stu-id="61b95-227">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="61b95-228">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="61b95-228">WaitForShutdown</span></span>

<span data-ttu-id="61b95-229">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) est déclenché via [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), comme [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (écoute `Ctrl+C`/SIGINT ou SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="61b95-229">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="61b95-230">`WaitForShutdown` appelle [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="61b95-230">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="61b95-231">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="61b95-231">WaitForShutdownAsync</span></span>

<span data-ttu-id="61b95-232">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) retourne une `Task` qui est effectuée quand l’arrêt est déclenché via le jeton fourni et appelle [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="61b95-232">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="external-control"></a><span data-ttu-id="61b95-233">Contrôle externe</span><span class="sxs-lookup"><span data-stu-id="61b95-233">External control</span></span>

<span data-ttu-id="61b95-234">Le contrôle externe de l’hôte peut être effectué à l’aide de méthodes pouvant être appelées de façon externe :</span><span class="sxs-lookup"><span data-stu-id="61b95-234">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="61b95-235">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) est appelé au début de [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), lequel attend qu’il soit fini avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="61b95-235">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="61b95-236">Cela permet de retarder le démarrage jusqu'à ce que celui-ci soit signalé par un événement externe.</span><span class="sxs-lookup"><span data-stu-id="61b95-236">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="61b95-237">Interface IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="61b95-237">IHostingEnvironment interface</span></span>

<span data-ttu-id="61b95-238">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) fournit des informations sur l’environnement d’hébergement de l’application.</span><span class="sxs-lookup"><span data-stu-id="61b95-238">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="61b95-239">Utilisez [l’injection de constructeur](xref:fundamentals/dependency-injection) pour obtenir l’interface `IHostingEnvironment` afin d’utiliser ses propriétés et méthodes d’extension :</span><span class="sxs-lookup"><span data-stu-id="61b95-239">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="61b95-240">Pour plus d’informations, consultez [Utiliser plusieurs environnements](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="61b95-240">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="61b95-241">Interface IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="61b95-241">IApplicationLifetime interface</span></span>

<span data-ttu-id="61b95-242">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) s’utilise pour les activités de post-démarrage et d’arrêt, notamment pour les demandes d’arrêt normal.</span><span class="sxs-lookup"><span data-stu-id="61b95-242">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="61b95-243">Trois propriétés de l’interface sont des jetons d’annulation utilisés pour inscrire les méthodes `Action` qui définissent les événements de démarrage et d’arrêt.</span><span class="sxs-lookup"><span data-stu-id="61b95-243">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="61b95-244">Jeton d’annulation</span><span class="sxs-lookup"><span data-stu-id="61b95-244">Cancellation Token</span></span> | <span data-ttu-id="61b95-245">Événement déclencheur</span><span class="sxs-lookup"><span data-stu-id="61b95-245">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="61b95-246">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="61b95-246">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="61b95-247">L’hôte a complètement démarré.</span><span class="sxs-lookup"><span data-stu-id="61b95-247">The host has fully started.</span></span> |
| [<span data-ttu-id="61b95-248">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="61b95-248">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="61b95-249">L’hôte effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="61b95-249">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="61b95-250">Toutes les requêtes doivent être traitées.</span><span class="sxs-lookup"><span data-stu-id="61b95-250">All requests should be processed.</span></span> <span data-ttu-id="61b95-251">L’arrêt est bloqué tant que cet événement n’est pas terminé.</span><span class="sxs-lookup"><span data-stu-id="61b95-251">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="61b95-252">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="61b95-252">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="61b95-253">L’hôte effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="61b95-253">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="61b95-254">Des requêtes peuvent encore être en cours de traitement.</span><span class="sxs-lookup"><span data-stu-id="61b95-254">Requests may still be processing.</span></span> <span data-ttu-id="61b95-255">L’arrêt est bloqué tant que cet événement n’est pas terminé.</span><span class="sxs-lookup"><span data-stu-id="61b95-255">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="61b95-256">Le constructeur injecte le service `IApplicationLifetime` dans une classe.</span><span class="sxs-lookup"><span data-stu-id="61b95-256">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="61b95-257">L’[exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) utilise l’injection de constructeur dans une classe `LifetimeEventsHostedService` (une implémentation `IHostedService`) pour enregistrer les événements.</span><span class="sxs-lookup"><span data-stu-id="61b95-257">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses contructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="61b95-258">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="61b95-258">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="61b95-259">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requête l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="61b95-259">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="61b95-260">La classe suivante utilise `StopApplication` pour arrêter normalement une application quand la méthode `Shutdown` de la classe est appelée :</span><span class="sxs-lookup"><span data-stu-id="61b95-260">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="61b95-261">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="61b95-261">Additional resources</span></span>

* [<span data-ttu-id="61b95-262">Tâches d’arrière-plan avec services hébergés</span><span class="sxs-lookup"><span data-stu-id="61b95-262">Background tasks with hosted services</span></span>](xref:fundamentals/host/hosted-services)
* [<span data-ttu-id="61b95-263">Hosting repo samples on GitHub</span><span class="sxs-lookup"><span data-stu-id="61b95-263">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
