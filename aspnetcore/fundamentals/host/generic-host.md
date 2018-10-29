---
title: Hôte générique .NET
author: guardrex
description: Découvrez l’hôte générique dans .NET, responsable de la gestion du démarrage et de la durée de vie des applications.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: e5f91ed64b7f8402dfe938f0fa8a0d94755d15c6
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207717"
---
# <a name="net-generic-host"></a><span data-ttu-id="37bd8-103">Hôte générique .NET</span><span class="sxs-lookup"><span data-stu-id="37bd8-103">.NET Generic Host</span></span>

<span data-ttu-id="37bd8-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="37bd8-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="37bd8-105">Les applications .NET Core configurent et lancent un *hôte*.</span><span class="sxs-lookup"><span data-stu-id="37bd8-105">.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="37bd8-106">L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="37bd8-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="37bd8-107">Cette rubrique traite de l’hôte générique ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), utile pour l’hébergement d’applications qui ne traitent pas les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="37bd8-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="37bd8-108">Pour en savoir plus sur l’hôte web ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), consultez <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="37bd8-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see <xref:fundamentals/host/web-host>.</span></span>

<span data-ttu-id="37bd8-109">L’objectif de l’hôte générique est de séparer le pipeline HTTP de l’API d’hôte web pour permettre un plus large éventail de scénarios d’hôte.</span><span class="sxs-lookup"><span data-stu-id="37bd8-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="37bd8-110">La messagerie, les tâches en arrière-plan et autres charges de travail non-HTTP basées sur l’hôte générique bénéficient de fonctionnalités transversales, comme la configuration, l’injection de dépendances (DI) et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="37bd8-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="37bd8-111">L’hôte générique est nouveau dans ASP.NET Core 2.1 et n’est pas adapté aux scénarios d’hébergement web.</span><span class="sxs-lookup"><span data-stu-id="37bd8-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="37bd8-112">Pour les scénarios d’hébergement de web, utilisez l’[hôte web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="37bd8-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="37bd8-113">L’hôte générique est en cours de développement pour remplacer l’hôte web dans une version ultérieure et servir d’API hôte principale dans les scénarios HTTP et non-HTTP.</span><span class="sxs-lookup"><span data-stu-id="37bd8-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="37bd8-114">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="37bd8-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="37bd8-115">Quand vous exécutez l’exemple d’application dans [Visual Studio Code](https://code.visualstudio.com/), utilisez un *terminal externe ou intégré*.</span><span class="sxs-lookup"><span data-stu-id="37bd8-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="37bd8-116">N’exécutez pas l’exemple dans une `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="37bd8-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="37bd8-117">Pour définir la console dans Visual Studio Code :</span><span class="sxs-lookup"><span data-stu-id="37bd8-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="37bd8-118">Ouvrez le fichier *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="37bd8-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="37bd8-119">Dans la configuration **.NET Core Launch (console)**, recherchez l’entrée **console**.</span><span class="sxs-lookup"><span data-stu-id="37bd8-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="37bd8-120">Définissez la valeur avec `externalTerminal` ou `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="37bd8-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="37bd8-121">Introduction</span><span class="sxs-lookup"><span data-stu-id="37bd8-121">Introduction</span></span>

<span data-ttu-id="37bd8-122">La bibliothèque de l’hôte générique est disponible dans [l’espace de noms Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) et est fournie par le package [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span><span class="sxs-lookup"><span data-stu-id="37bd8-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="37bd8-123">Le package `Microsoft.Extensions.Hosting` est inclus dans le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 ou ultérieur).</span><span class="sxs-lookup"><span data-stu-id="37bd8-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="37bd8-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) est le point d’entrée pour exécuter le code.</span><span class="sxs-lookup"><span data-stu-id="37bd8-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="37bd8-125">Chaque implémentation `IHostedService` est exécutée dans l’ordre d’[inscription des services dans ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="37bd8-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="37bd8-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) est appelé sur chaque `IHostedService` au démarrage de l’hôte, tandis que [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) est appelé dans l’ordre d’inscription inverse quand l’hôte s’arrête normalement.</span><span class="sxs-lookup"><span data-stu-id="37bd8-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="37bd8-127">Configurer un hôte</span><span class="sxs-lookup"><span data-stu-id="37bd8-127">Set up a host</span></span>

<span data-ttu-id="37bd8-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) est le principal composant que les applications et les bibliothèques utilisent pour initialiser, générer et exécuter l’hôte :</span><span class="sxs-lookup"><span data-stu-id="37bd8-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="default-services"></a><span data-ttu-id="37bd8-129">Services par défaut</span><span class="sxs-lookup"><span data-stu-id="37bd8-129">Default services</span></span>

<span data-ttu-id="37bd8-130">Les services suivants sont inscrits au moment de l’initialisation de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="37bd8-130">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="37bd8-131">[Environnement](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="37bd8-131">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="37bd8-132">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="37bd8-132">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="37bd8-133"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="37bd8-133"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="37bd8-134"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="37bd8-134"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="37bd8-135">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="37bd8-135">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="37bd8-136">[Journalisation](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="37bd8-136">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="37bd8-137">Configuration de l’hôte</span><span class="sxs-lookup"><span data-stu-id="37bd8-137">Host configuration</span></span>

<span data-ttu-id="37bd8-138">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) s’appuie sur les approches suivantes pour définir les valeurs de configuration de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="37bd8-138">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="37bd8-139">Générateur de configuration</span><span class="sxs-lookup"><span data-stu-id="37bd8-139">Configuration builder</span></span>
* <span data-ttu-id="37bd8-140">Configuration de méthode d’extension</span><span class="sxs-lookup"><span data-stu-id="37bd8-140">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="37bd8-141">Générateur de configuration</span><span class="sxs-lookup"><span data-stu-id="37bd8-141">Configuration builder</span></span>

<span data-ttu-id="37bd8-142">Le générateur de configuration d’hôte est créé en appelant [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) dans l’implémentation [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder).</span><span class="sxs-lookup"><span data-stu-id="37bd8-142">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="37bd8-143">`ConfigureHostConfiguration` utilise un [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) afin de créer une [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) pour l’hôte.</span><span class="sxs-lookup"><span data-stu-id="37bd8-143">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="37bd8-144">Le générateur de configuration initialise les [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) pour les utiliser dans le processus de génération de l’application.</span><span class="sxs-lookup"><span data-stu-id="37bd8-144">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span>

<span data-ttu-id="37bd8-145">La configuration des variables d’environnement n’est pas ajoutée par défaut.</span><span class="sxs-lookup"><span data-stu-id="37bd8-145">Environment variable configuration isn't added by default.</span></span> <span data-ttu-id="37bd8-146">Appelez [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) sur le générateur d’hôte pour configurer l’hôte à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="37bd8-146">Call [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) on the host builder to configure the host from environment variables.</span></span> <span data-ttu-id="37bd8-147">`AddEnvironmentVariables` accepte un préfixe facultatif défini par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="37bd8-147">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="37bd8-148">L’exemple d’application utilise un préfixe `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="37bd8-148">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="37bd8-149">Ce préfixe est supprimé à la lecture des variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="37bd8-149">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="37bd8-150">Lorsque l’hôte de l’exemple d’application est configuré, la valeur de variable d’environnement de `PREFIX_ENVIRONMENT` devient la valeur de configuration d’hôte de la clé `environment`.</span><span class="sxs-lookup"><span data-stu-id="37bd8-150">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="37bd8-151">Pendant le développement, lorsque vous utilisez [Visual Studio](https://www.visualstudio.com/) ou que vous lancez une application avec `dotnet run`, vous pouvez définir les variables d’environnement dans le fichier *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="37bd8-151">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="37bd8-152">Dans [Visual Studio Code](https://code.visualstudio.com/), elles peuvent être définies dans le fichier *.vscode/launch.json* au cours du développement.</span><span class="sxs-lookup"><span data-stu-id="37bd8-152">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="37bd8-153">Pour plus d'informations, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="37bd8-153">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="37bd8-154">`ConfigureHostConfiguration` peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="37bd8-154">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="37bd8-155">L’hôte utilise l’option qui définit une valeur en dernier.</span><span class="sxs-lookup"><span data-stu-id="37bd8-155">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="37bd8-156">*hostsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="37bd8-156">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="37bd8-157">Exemple de configuration `HostBuilder` avec `ConfigureHostConfiguration` :</span><span class="sxs-lookup"><span data-stu-id="37bd8-157">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="37bd8-158">Actuellement, la méthode d’extension [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) n’est pas capable d’analyser une section de configuration retournée par [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (par exemple, `.AddConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="37bd8-158">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="37bd8-159">La méthode `GetSection` filtre les clés de configuration dans la section demandée, mais laisse le nom de la section dans les clés (par exemple, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="37bd8-159">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="37bd8-160">La méthode `AddConfiguration` suppose que les clés correspondent aux clés `HostBuilder` (par exemple, `environment`).</span><span class="sxs-lookup"><span data-stu-id="37bd8-160">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="37bd8-161">La présence du nom de la section dans les clés empêche l’utilisation des valeurs de la section pour configurer l’hôte.</span><span class="sxs-lookup"><span data-stu-id="37bd8-161">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="37bd8-162">Ce problème sera résolu dans une prochaine mise en production.</span><span class="sxs-lookup"><span data-stu-id="37bd8-162">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="37bd8-163">Pour obtenir plus d’informations et connaître les solutions de contournement possibles, consultez [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="37bd8-163">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="37bd8-164">Configuration de méthode d’extension</span><span class="sxs-lookup"><span data-stu-id="37bd8-164">Extension method configuration</span></span>

<span data-ttu-id="37bd8-165">Les méthodes d’extension sont appelées sur l’implémentation `IHostBuilder` pour configurer la racine de contenu et l’environnement.</span><span class="sxs-lookup"><span data-stu-id="37bd8-165">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="application-key-name"></a><span data-ttu-id="37bd8-166">Clé d’application (Nom)</span><span class="sxs-lookup"><span data-stu-id="37bd8-166">Application Key (Name)</span></span>

<span data-ttu-id="37bd8-167">La propriété [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) est définie à partir de la configuration de l’hôte pendant la construction de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="37bd8-167">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is set from host configuration during host construction.</span></span> <span data-ttu-id="37bd8-168">Pour définir la valeur explicitement, utilisez [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey) :</span><span class="sxs-lookup"><span data-stu-id="37bd8-168">To set the value explicitly, use the [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey):</span></span>

<span data-ttu-id="37bd8-169">**Clé** : applicationName</span><span class="sxs-lookup"><span data-stu-id="37bd8-169">**Key**: applicationName</span></span>  
<span data-ttu-id="37bd8-170">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="37bd8-170">**Type**: *string*</span></span>  
<span data-ttu-id="37bd8-171">**Par défaut** : nom de l’assembly contenant le point d’entrée de l’application.</span><span class="sxs-lookup"><span data-stu-id="37bd8-171">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="37bd8-172">**Définition avec** : `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="37bd8-172">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="37bd8-173">**Variable d’environnement** : `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` est [facultatif et défini par l’utilisateur](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="37bd8-173">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

```csharp
var host = new HostBuilder()
    .ConfigureAppConfiguration((hostContext, configApp) =>
    {
        hostContext.HostingEnvironment.ApplicationName = "CustomApplicationName";
    })
```

#### <a name="content-root"></a><span data-ttu-id="37bd8-174">Racine de contenu</span><span class="sxs-lookup"><span data-stu-id="37bd8-174">Content Root</span></span>

<span data-ttu-id="37bd8-175">Ce paramètre détermine où l’hôte commence la recherche des fichiers de contenu.</span><span class="sxs-lookup"><span data-stu-id="37bd8-175">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="37bd8-176">**Clé** : contentRoot</span><span class="sxs-lookup"><span data-stu-id="37bd8-176">**Key**: contentRoot</span></span>  
<span data-ttu-id="37bd8-177">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="37bd8-177">**Type**: *string*</span></span>  
<span data-ttu-id="37bd8-178">**Valeur par défaut** : dossier où réside l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="37bd8-178">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="37bd8-179">**Définition avec** : `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="37bd8-179">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="37bd8-180">**Variable d’environnement** : `<PREFIX_>CONTENTROOT` (`<PREFIX_>` est [facultatif et défini par l’utilisateur](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="37bd8-180">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="37bd8-181">Si le chemin est introuvable, l’hôte ne peut pas démarrer.</span><span class="sxs-lookup"><span data-stu-id="37bd8-181">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="37bd8-182">Environnement</span><span class="sxs-lookup"><span data-stu-id="37bd8-182">Environment</span></span>

<span data-ttu-id="37bd8-183">Définit l’[environnement](xref:fundamentals/environments) de l’application.</span><span class="sxs-lookup"><span data-stu-id="37bd8-183">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="37bd8-184">**Clé** : environment</span><span class="sxs-lookup"><span data-stu-id="37bd8-184">**Key**: environment</span></span>  
<span data-ttu-id="37bd8-185">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="37bd8-185">**Type**: *string*</span></span>  
<span data-ttu-id="37bd8-186">**Valeur par défaut** : Production</span><span class="sxs-lookup"><span data-stu-id="37bd8-186">**Default**: Production</span></span>  
<span data-ttu-id="37bd8-187">**Définition avec** : `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="37bd8-187">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="37bd8-188">**Variable d’environnement** : `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` est [facultatif et défini par l’utilisateur](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="37bd8-188">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="37bd8-189">L’environnement peut être défini à n’importe quelle valeur.</span><span class="sxs-lookup"><span data-stu-id="37bd8-189">The environment can be set to any value.</span></span> <span data-ttu-id="37bd8-190">Les valeurs définies par le framework sont `Development`, `Staging` et `Production`.</span><span class="sxs-lookup"><span data-stu-id="37bd8-190">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="37bd8-191">Les valeurs ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="37bd8-191">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="37bd8-192">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="37bd8-192">ConfigureAppConfiguration</span></span>

<span data-ttu-id="37bd8-193">Le générateur de configuration d’application est créé en appelant [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) dans l’implémentation [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder).</span><span class="sxs-lookup"><span data-stu-id="37bd8-193">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="37bd8-194">`ConfigureAppConfiguration` utilise un [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) afin de créer une [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) pour l’application.</span><span class="sxs-lookup"><span data-stu-id="37bd8-194">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="37bd8-195">`ConfigureAppConfiguration` peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="37bd8-195">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="37bd8-196">L’application utilise l’option qui définit une valeur en dernier.</span><span class="sxs-lookup"><span data-stu-id="37bd8-196">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="37bd8-197">La configuration créée par `ConfigureAppConfiguration` est disponible dans [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) pour les opérations suivantes et dans [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span><span class="sxs-lookup"><span data-stu-id="37bd8-197">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="37bd8-198">Exemple de configuration d’application avec `ConfigureAppConfiguration` :</span><span class="sxs-lookup"><span data-stu-id="37bd8-198">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="37bd8-199">*appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="37bd8-199">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="37bd8-200">*appsettings.Development.json* :</span><span class="sxs-lookup"><span data-stu-id="37bd8-200">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="37bd8-201">*appsettings.Production.json* :</span><span class="sxs-lookup"><span data-stu-id="37bd8-201">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="37bd8-202">Actuellement, la méthode d’extension [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) n’est pas capable d’analyser une section de configuration retournée par [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (par exemple, `.AddConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="37bd8-202">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="37bd8-203">La méthode `GetSection` filtre les clés de configuration dans la section demandée, mais laisse le nom de la section dans les clés (par exemple, `section:Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="37bd8-203">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="37bd8-204">La méthode `AddConfiguration` attend une correspondance exacte pour les clés de configuration (par exemple, `Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="37bd8-204">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="37bd8-205">La présence du nom de la section dans les clés empêche l’utilisation des valeurs de la section pour configurer l’application.</span><span class="sxs-lookup"><span data-stu-id="37bd8-205">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="37bd8-206">Ce problème sera résolu dans une prochaine mise en production.</span><span class="sxs-lookup"><span data-stu-id="37bd8-206">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="37bd8-207">Pour obtenir plus d’informations et connaître les solutions de contournement possibles, consultez [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="37bd8-207">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="37bd8-208">Pour déplacer des fichiers de paramètres vers le répertoire de sortie, spécifiez-les en tant qu’[éléments de projet MSBuild](/visualstudio/msbuild/common-msbuild-project-items) dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="37bd8-208">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="37bd8-209">L’exemple d’application déplace ses fichiers de paramètres d’application JSON et *hostsettings.json* avec l’élément **&lt;Content:&gt;** suivant :</span><span class="sxs-lookup"><span data-stu-id="37bd8-209">The sample app moves its JSON app settings files and *hostsettings.json* with the following **&lt;Content:&gt;** item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a><span data-ttu-id="37bd8-210">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="37bd8-210">ConfigureServices</span></span>

<span data-ttu-id="37bd8-211">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) ajoute des services au conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) de l’application.</span><span class="sxs-lookup"><span data-stu-id="37bd8-211">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="37bd8-212">`ConfigureServices` peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="37bd8-212">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="37bd8-213">Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="37bd8-213">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="37bd8-214">Pour plus d'informations, consultez <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="37bd8-214">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="37bd8-215">L’[exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) utilise la méthode d’extension `AddHostedService` afin d’ajouter un service pour les événements de durée de vie, `LifetimeEventsHostedService`, et une tâche en arrière-plan chronométrée, `TimedHostedService`, à l’application :</span><span class="sxs-lookup"><span data-stu-id="37bd8-215">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="37bd8-216">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="37bd8-216">ConfigureLogging</span></span>

<span data-ttu-id="37bd8-217">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) ajoute un délégué pour configurer le [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder) fourni.</span><span class="sxs-lookup"><span data-stu-id="37bd8-217">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="37bd8-218">`ConfigureLogging` peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="37bd8-218">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="37bd8-219">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="37bd8-219">UseConsoleLifetime</span></span>

<span data-ttu-id="37bd8-220">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) écoute `Ctrl+C`/SIGINT ou SIGTERM et appelle [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) pour démarrer le processus d’arrêt.</span><span class="sxs-lookup"><span data-stu-id="37bd8-220">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="37bd8-221">`UseConsoleLifetime` déverrouille les extensions telles que [RunAsync](#runasync) et [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="37bd8-221">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="37bd8-222">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) est préinscrit en tant qu’implémentation de durée de vie par défaut.</span><span class="sxs-lookup"><span data-stu-id="37bd8-222">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="37bd8-223">La dernière durée de vie inscrite est utilisée.</span><span class="sxs-lookup"><span data-stu-id="37bd8-223">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="37bd8-224">Configuration du conteneur</span><span class="sxs-lookup"><span data-stu-id="37bd8-224">Container configuration</span></span>

<span data-ttu-id="37bd8-225">Pour prendre en charge le branchement dans d’autres conteneurs, l’hôte peut accepter un [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span><span class="sxs-lookup"><span data-stu-id="37bd8-225">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="37bd8-226">L’ajout d’une fabrique ne fait pas partie de l’inscription de conteneur DI mais est plutôt une tâche un intrinsèque à l’hôte utilisée pour créer le conteneur DI concret.</span><span class="sxs-lookup"><span data-stu-id="37bd8-226">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="37bd8-227">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) remplace la fabrique par défaut utilisée pour créer le fournisseur de services de l’application.</span><span class="sxs-lookup"><span data-stu-id="37bd8-227">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="37bd8-228">La configuration de conteneur personnalisée est gérée par la méthode [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer).</span><span class="sxs-lookup"><span data-stu-id="37bd8-228">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="37bd8-229">`ConfigureContainer` offre une expérience fortement typée pour configurer le conteneur sur l’API hôte sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="37bd8-229">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="37bd8-230">`ConfigureContainer` peut être appelé plusieurs fois avec des résultats additifs.</span><span class="sxs-lookup"><span data-stu-id="37bd8-230">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="37bd8-231">Créer un conteneur de service pour l’application :</span><span class="sxs-lookup"><span data-stu-id="37bd8-231">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="37bd8-232">Fournir une fabrique de conteneur de service :</span><span class="sxs-lookup"><span data-stu-id="37bd8-232">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="37bd8-233">Utiliser la fabrique et configurer le conteneur de service personnalisé pour l’application :</span><span class="sxs-lookup"><span data-stu-id="37bd8-233">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="37bd8-234">Extensibilité</span><span class="sxs-lookup"><span data-stu-id="37bd8-234">Extensibility</span></span>

<span data-ttu-id="37bd8-235">L’extensibilité de l’hôte est effectuée avec les méthodes d’extension sur `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="37bd8-235">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="37bd8-236">L’exemple suivant montre comment une méthode d’extension étend une implémentation `IHostBuilder` avec l’exemple [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) démontré dans <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="37bd8-236">The following example shows how an extension method extends an `IHostBuilder` implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="37bd8-237">Une application établit la méthode d'extension `UseHostedService` pour inscrire le service hébergé passé dans `T` :</span><span class="sxs-lookup"><span data-stu-id="37bd8-237">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="37bd8-238">Gérer l’hôte</span><span class="sxs-lookup"><span data-stu-id="37bd8-238">Manage the host</span></span>

<span data-ttu-id="37bd8-239">L’implémentation [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) est chargée de démarrer et d’arrêter les implémentations `IHostedService` qui sont inscrites dans le conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="37bd8-239">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="37bd8-240">Exécuter</span><span class="sxs-lookup"><span data-stu-id="37bd8-240">Run</span></span>

<span data-ttu-id="37bd8-241">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) exécute l’application et bloque le thread appelant jusqu’à l’arrêt de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="37bd8-241">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="37bd8-242">RunAsync</span><span class="sxs-lookup"><span data-stu-id="37bd8-242">RunAsync</span></span>

<span data-ttu-id="37bd8-243">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) exécute l’application et retourne une `Task` qui est effectuée quand le jeton d’annulation ou l’arrêt est déclenché :</span><span class="sxs-lookup"><span data-stu-id="37bd8-243">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="37bd8-244">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="37bd8-244">RunConsoleAsync</span></span>

<span data-ttu-id="37bd8-245">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) permet la prise en charge de la console, génère et démarre l’hôte et attend `Ctrl+C`/SIGINT ou SIGTERM pour l’arrêter.</span><span class="sxs-lookup"><span data-stu-id="37bd8-245">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="37bd8-246">Start et StopAsync</span><span class="sxs-lookup"><span data-stu-id="37bd8-246">Start and StopAsync</span></span>

<span data-ttu-id="37bd8-247">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) démarre l’hôte en mode synchrone.</span><span class="sxs-lookup"><span data-stu-id="37bd8-247">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="37bd8-248">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) tente d’arrêter l’hôte dans le délai d’attente fourni.</span><span class="sxs-lookup"><span data-stu-id="37bd8-248">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="37bd8-249">StartAsync et StopAsync</span><span class="sxs-lookup"><span data-stu-id="37bd8-249">StartAsync and StopAsync</span></span>

<span data-ttu-id="37bd8-250">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) démarre l’application.</span><span class="sxs-lookup"><span data-stu-id="37bd8-250">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="37bd8-251">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) arrête l’application.</span><span class="sxs-lookup"><span data-stu-id="37bd8-251">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="37bd8-252">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="37bd8-252">WaitForShutdown</span></span>

<span data-ttu-id="37bd8-253">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) est déclenché via [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), comme [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (écoute `Ctrl+C`/SIGINT ou SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="37bd8-253">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="37bd8-254">`WaitForShutdown` appelle [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="37bd8-254">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="37bd8-255">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="37bd8-255">WaitForShutdownAsync</span></span>

<span data-ttu-id="37bd8-256">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) retourne une `Task` qui est effectuée quand l’arrêt est déclenché via le jeton fourni et appelle [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="37bd8-256">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="external-control"></a><span data-ttu-id="37bd8-257">Contrôle externe</span><span class="sxs-lookup"><span data-stu-id="37bd8-257">External control</span></span>

<span data-ttu-id="37bd8-258">Le contrôle externe de l’hôte peut être effectué à l’aide de méthodes pouvant être appelées de façon externe :</span><span class="sxs-lookup"><span data-stu-id="37bd8-258">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="37bd8-259">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) est appelé au début de [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), lequel attend qu’il soit fini avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="37bd8-259">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="37bd8-260">Cela permet de retarder le démarrage jusqu'à ce que celui-ci soit signalé par un événement externe.</span><span class="sxs-lookup"><span data-stu-id="37bd8-260">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="37bd8-261">Interface IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="37bd8-261">IHostingEnvironment interface</span></span>

<span data-ttu-id="37bd8-262">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) fournit des informations sur l’environnement d’hébergement de l’application.</span><span class="sxs-lookup"><span data-stu-id="37bd8-262">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="37bd8-263">Utilisez [l’injection de constructeur](xref:fundamentals/dependency-injection) pour obtenir l’interface `IHostingEnvironment` afin d’utiliser ses propriétés et méthodes d’extension :</span><span class="sxs-lookup"><span data-stu-id="37bd8-263">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="37bd8-264">Pour plus d'informations, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="37bd8-264">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="37bd8-265">Interface IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="37bd8-265">IApplicationLifetime interface</span></span>

<span data-ttu-id="37bd8-266">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) s’utilise pour les activités de post-démarrage et d’arrêt, notamment pour les demandes d’arrêt normal.</span><span class="sxs-lookup"><span data-stu-id="37bd8-266">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="37bd8-267">Trois propriétés de l’interface sont des jetons d’annulation utilisés pour inscrire les méthodes `Action` qui définissent les événements de démarrage et d’arrêt.</span><span class="sxs-lookup"><span data-stu-id="37bd8-267">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="37bd8-268">Jeton d’annulation</span><span class="sxs-lookup"><span data-stu-id="37bd8-268">Cancellation Token</span></span> | <span data-ttu-id="37bd8-269">Événement déclencheur</span><span class="sxs-lookup"><span data-stu-id="37bd8-269">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="37bd8-270">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="37bd8-270">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="37bd8-271">L’hôte a complètement démarré.</span><span class="sxs-lookup"><span data-stu-id="37bd8-271">The host has fully started.</span></span> |
| [<span data-ttu-id="37bd8-272">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="37bd8-272">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="37bd8-273">L’hôte effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="37bd8-273">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="37bd8-274">Toutes les requêtes doivent être traitées.</span><span class="sxs-lookup"><span data-stu-id="37bd8-274">All requests should be processed.</span></span> <span data-ttu-id="37bd8-275">L’arrêt est bloqué tant que cet événement n’est pas terminé.</span><span class="sxs-lookup"><span data-stu-id="37bd8-275">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="37bd8-276">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="37bd8-276">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="37bd8-277">L’hôte effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="37bd8-277">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="37bd8-278">Des requêtes peuvent encore être en cours de traitement.</span><span class="sxs-lookup"><span data-stu-id="37bd8-278">Requests may still be processing.</span></span> <span data-ttu-id="37bd8-279">L’arrêt est bloqué tant que cet événement n’est pas terminé.</span><span class="sxs-lookup"><span data-stu-id="37bd8-279">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="37bd8-280">Le constructeur injecte le service `IApplicationLifetime` dans une classe.</span><span class="sxs-lookup"><span data-stu-id="37bd8-280">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="37bd8-281">[L’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) utilise l’injection de constructeur dans une classe `LifetimeEventsHostedService` (une implémentation `IHostedService`) pour inscrire les événements.</span><span class="sxs-lookup"><span data-stu-id="37bd8-281">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="37bd8-282">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="37bd8-282">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="37bd8-283">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requête l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="37bd8-283">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="37bd8-284">La classe suivante utilise `StopApplication` pour arrêter normalement une application quand la méthode `Shutdown` de la classe est appelée :</span><span class="sxs-lookup"><span data-stu-id="37bd8-284">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="37bd8-285">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="37bd8-285">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
* [<span data-ttu-id="37bd8-286">Hosting repo samples on GitHub</span><span class="sxs-lookup"><span data-stu-id="37bd8-286">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
