---
title: Hébergement dans ASP.NET Core
author: guardrex
description: Découvrez l’hôte web dans ASP.NET Core, qui est responsable de la gestion du démarrage et de la durée de vie des applications.
manager: wpickett
ms.author: riande
ms.date: 09/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosting
ms.openlocfilehash: 344bf5f0917f4c33d67eeb14176ff2aae3ae75da
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="85b02-103">Hébergement dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="85b02-103">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="85b02-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="85b02-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="85b02-105">Les applications ASP.NET Core configurent et lancent un *hôte*.</span><span class="sxs-lookup"><span data-stu-id="85b02-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="85b02-106">L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="85b02-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="85b02-107">Au minimum, l’hôte configure un serveur ainsi qu’un pipeline de traitement des requêtes.</span><span class="sxs-lookup"><span data-stu-id="85b02-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="85b02-108">Configuration d’un hôte</span><span class="sxs-lookup"><span data-stu-id="85b02-108">Setting up a host</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="85b02-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="85b02-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="85b02-110">Créez un hôte en utilisant une instance de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="85b02-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="85b02-111">Cette opération est généralement effectuée au point d’entrée de l’application, à savoir la méthode `Main`.</span><span class="sxs-lookup"><span data-stu-id="85b02-111">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="85b02-112">Dans les modèles de projet, `Main` se trouve dans *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="85b02-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="85b02-113">Un fichier *Program.cs* standard appelle [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) pour commencer la configuration d’un hôte :</span><span class="sxs-lookup"><span data-stu-id="85b02-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="85b02-114">`CreateDefaultBuilder` effectue les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="85b02-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="85b02-115">Configure [Kestrel](servers/kestrel.md) en tant que serveur web.</span><span class="sxs-lookup"><span data-stu-id="85b02-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="85b02-116">Pour connaître les options Kestrel par défaut, consultez la [section sur les options Kestrel dans Implémentation du serveur web Kestrel dans ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="85b02-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="85b02-117">Définit le chemin retourné par [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) comme racine de contenu.</span><span class="sxs-lookup"><span data-stu-id="85b02-117">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="85b02-118">Charge la configuration facultative à partir des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="85b02-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="85b02-119">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="85b02-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="85b02-120">*appsettings.{Environment}.json*</span><span class="sxs-lookup"><span data-stu-id="85b02-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="85b02-121">Les [secrets utilisateur](xref:security/app-secrets) quand l’application s’exécute dans l’environnement `Development`</span><span class="sxs-lookup"><span data-stu-id="85b02-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="85b02-122">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="85b02-122">Environment variables.</span></span>
  * <span data-ttu-id="85b02-123">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="85b02-123">Command-line arguments.</span></span>
* <span data-ttu-id="85b02-124">Configure la [journalisation](xref:fundamentals/logging/index) des sorties de la console et du débogage.</span><span class="sxs-lookup"><span data-stu-id="85b02-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="85b02-125">La journalisation inclut les règles de [filtrage de journal](xref:fundamentals/logging/index#log-filtering) qui sont spécifiées dans une section de configuration de la journalisation dans un fichier *appsettings.json* ou *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="85b02-125">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="85b02-126">Active [l’intégration IIS](xref:host-and-deploy/iis/index) quand il est exécuté derrière IIS.</span><span class="sxs-lookup"><span data-stu-id="85b02-126">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="85b02-127">Configure le chemin de base et le port écouté par le serveur lors de l’utilisation du [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="85b02-127">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="85b02-128">Le module crée un proxy inverse entre IIS et Kestrel.</span><span class="sxs-lookup"><span data-stu-id="85b02-128">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="85b02-129">Il configure également l’application pour la [capture des erreurs de démarrage](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="85b02-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="85b02-130">Pour connaître les options IIS par défaut, consultez la [section sur les options IIS dans Héberger ASP.NET Core sur Windows avec IIS](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="85b02-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>
* <span data-ttu-id="85b02-131">Définissez [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) sur `true` si l’environnement de l’application est Développement.</span><span class="sxs-lookup"><span data-stu-id="85b02-131">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="85b02-132">Pour plus d’informations, consultez [Validation de l’étendue](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="85b02-132">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="85b02-133">La *racine de contenu* détermine l’emplacement où l’hôte recherche les fichiers de contenu, tels que les fichiers de vue MVC.</span><span class="sxs-lookup"><span data-stu-id="85b02-133">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="85b02-134">Quand l’application est démarrée à partir du dossier racine du projet, ce dossier est utilisé comme racine de contenu.</span><span class="sxs-lookup"><span data-stu-id="85b02-134">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="85b02-135">Il s’agit du dossier par défaut utilisé dans [Visual Studio](https://www.visualstudio.com/) et les [modèles dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="85b02-135">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="85b02-136">Pour plus d’informations sur la configuration d’une application, consultez [Configuration dans ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="85b02-136">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="85b02-137">Au lieu d’utiliser la méthode statique `CreateDefaultBuilder`, vous pouvez aussi créer un hôte à partir de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Cette approche est prise en charge dans ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="85b02-137">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="85b02-138">Pour plus d’informations, consultez l’onglet ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="85b02-138">For more information, see the ASP.NET Core 1.x tab.</span></span>

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="85b02-139">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="85b02-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="85b02-140">Créez un hôte en utilisant une instance de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="85b02-140">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="85b02-141">Cette opération est généralement effectuée au point d’entrée de l’application, à savoir la méthode `Main`.</span><span class="sxs-lookup"><span data-stu-id="85b02-141">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="85b02-142">Dans les modèles de projet, `Main` se trouve dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="85b02-142">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="85b02-143">`WebHostBuilder` nécessite un serveur [ qui implémente IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="85b02-143">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="85b02-144">Les serveurs intégrés sont [Kestrel](servers/kestrel.md) et [HTTP.sys](servers/httpsys.md) (dans les versions antérieures à ASP.NET Core 2.0, HTTP.sys était appelé [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="85b02-144">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="85b02-145">Dans cet exemple, la [méthode d’extension UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) spécifie le serveur Kestrel.</span><span class="sxs-lookup"><span data-stu-id="85b02-145">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="85b02-146">La *racine de contenu* détermine l’emplacement où l’hôte recherche les fichiers de contenu, tels que les fichiers de vue MVC.</span><span class="sxs-lookup"><span data-stu-id="85b02-146">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="85b02-147">La racine de contenu par défaut pour `UseContentRoot` est retournée par [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="85b02-147">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="85b02-148">Quand l’application est démarrée à partir du dossier racine du projet, ce dossier est utilisé comme racine de contenu.</span><span class="sxs-lookup"><span data-stu-id="85b02-148">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="85b02-149">Il s’agit du dossier par défaut utilisé dans [Visual Studio](https://www.visualstudio.com/) et les [modèles dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="85b02-149">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="85b02-150">Pour utiliser IIS comme proxy inverse, appelez [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) dans le cadre de la création de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="85b02-150">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="85b02-151">`UseIISIntegration` ne configure pas un *serveur*, contrairement à ce que fait [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="85b02-151">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="85b02-152">`UseIISIntegration` configure le chemin de base et le port écouté par le serveur lors de l’utilisation du [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) pour créer un proxy inverse entre Kestrel et IIS.</span><span class="sxs-lookup"><span data-stu-id="85b02-152">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="85b02-153">Pour utiliser IIS avec ASP.NET Core, `UseKestrel` et `UseIISIntegration` doivent être spécifiés.</span><span class="sxs-lookup"><span data-stu-id="85b02-153">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="85b02-154">`UseIISIntegration` est activé uniquement dans le cadre d’une exécution derrière IIS ou IIS Express.</span><span class="sxs-lookup"><span data-stu-id="85b02-154">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="85b02-155">Pour plus d’informations, consultez [Présentation du module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) et [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="85b02-155">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="85b02-156">Une implémentation minimale qui configure un hôte (et une application ASP.NET Core) consiste à spécifier un serveur et la configuration du pipeline des requêtes de l’application :</span><span class="sxs-lookup"><span data-stu-id="85b02-156">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

* * *
<span data-ttu-id="85b02-157">Lors de la configuration d’un hôte, les méthodes [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) et [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) peuvent être fournies.</span><span class="sxs-lookup"><span data-stu-id="85b02-157">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="85b02-158">Si une classe `Startup` est spécifiée, elle doit définir une méthode `Configure`.</span><span class="sxs-lookup"><span data-stu-id="85b02-158">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="85b02-159">Pour plus d’informations, consultez [Démarrage de l’application dans ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="85b02-159">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="85b02-160">Les appels multiples à `ConfigureServices` s’ajoutent les uns aux autres.</span><span class="sxs-lookup"><span data-stu-id="85b02-160">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="85b02-161">Les appels multiples à `Configure` ou `UseStartup` sur `WebHostBuilder` remplacent les paramètres précédents.</span><span class="sxs-lookup"><span data-stu-id="85b02-161">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="85b02-162">Valeurs de configuration de l’hôte</span><span class="sxs-lookup"><span data-stu-id="85b02-162">Host configuration values</span></span>

<span data-ttu-id="85b02-163">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) s’appuie sur les approches suivantes pour définir les valeurs de configuration de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="85b02-163">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="85b02-164">Configuration du générateur de l’hôte, qui inclut des variables d’environnement au format `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="85b02-164">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="85b02-165">Par exemple, `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="85b02-165">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="85b02-166">Méthodes explicites, telles que `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="85b02-166">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="85b02-167">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) et la clé associée.</span><span class="sxs-lookup"><span data-stu-id="85b02-167">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="85b02-168">Quand une valeur est définie avec `UseSetting`, elle est définie au format chaîne indépendamment du type.</span><span class="sxs-lookup"><span data-stu-id="85b02-168">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="85b02-169">L’hôte utilise l’option qui définit une valeur en dernier.</span><span class="sxs-lookup"><span data-stu-id="85b02-169">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="85b02-170">Pour plus d’informations, consultez [Substitution d’une configuration](#overriding-configuration) dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="85b02-170">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="85b02-171">Capture des erreurs de démarrage</span><span class="sxs-lookup"><span data-stu-id="85b02-171">Capture Startup Errors</span></span>

<span data-ttu-id="85b02-172">Ce paramètre contrôle la capture des erreurs de démarrage.</span><span class="sxs-lookup"><span data-stu-id="85b02-172">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="85b02-173">**Clé** : captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="85b02-173">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="85b02-174">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="85b02-174">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="85b02-175">**Valeur par défaut** : `false`, ou `true` si l’application s’exécute avec Kestrel derrière IIS.</span><span class="sxs-lookup"><span data-stu-id="85b02-175">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="85b02-176">**Définition avec** : `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="85b02-176">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="85b02-177">**Variable d’environnement** : `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="85b02-177">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="85b02-178">Avec la valeur `false`, la survenue d’erreurs au démarrage entraîne la fermeture de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="85b02-178">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="85b02-179">Avec la valeur `true`, l’hôte capture les exceptions levées au démarrage et tente de démarrer le serveur.</span><span class="sxs-lookup"><span data-stu-id="85b02-179">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="85b02-180">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="85b02-180">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="85b02-181">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="85b02-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="85b02-182">Racine de contenu</span><span class="sxs-lookup"><span data-stu-id="85b02-182">Content Root</span></span>

<span data-ttu-id="85b02-183">Ce paramètre détermine l’emplacement où ASP.NET Core commence la recherche des fichiers de contenu, tels que les vues MVC.</span><span class="sxs-lookup"><span data-stu-id="85b02-183">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="85b02-184">**Clé** : contentRoot</span><span class="sxs-lookup"><span data-stu-id="85b02-184">**Key**: contentRoot</span></span>  
<span data-ttu-id="85b02-185">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="85b02-185">**Type**: *string*</span></span>  
<span data-ttu-id="85b02-186">**Valeur par défaut** : dossier où réside l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="85b02-186">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="85b02-187">**Définition avec** : `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="85b02-187">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="85b02-188">**Variable d’environnement** : `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="85b02-188">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="85b02-189">La racine de contenu est également utilisée comme chemin de base pour le [paramètre de la racine Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="85b02-189">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="85b02-190">Si le chemin est introuvable, l’hôte ne peut pas démarrer.</span><span class="sxs-lookup"><span data-stu-id="85b02-190">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="85b02-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="85b02-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="85b02-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="85b02-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="85b02-193">Erreurs détaillées</span><span class="sxs-lookup"><span data-stu-id="85b02-193">Detailed Errors</span></span>

<span data-ttu-id="85b02-194">Détermine si les erreurs détaillées doivent être capturées.</span><span class="sxs-lookup"><span data-stu-id="85b02-194">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="85b02-195">**Clé** : detailedErrors</span><span class="sxs-lookup"><span data-stu-id="85b02-195">**Key**: detailedErrors</span></span>  
<span data-ttu-id="85b02-196">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="85b02-196">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="85b02-197">**Valeur par défaut** : false</span><span class="sxs-lookup"><span data-stu-id="85b02-197">**Default**: false</span></span>  
<span data-ttu-id="85b02-198">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="85b02-198">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="85b02-199">**Variable d’environnement** : `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="85b02-199">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="85b02-200">Quand cette fonctionnalité est activée (ou que <a href="#environment">l’environnement</a> est défini sur `Development`), l’application capture les exceptions détaillées.</span><span class="sxs-lookup"><span data-stu-id="85b02-200">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="85b02-201">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="85b02-201">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="85b02-202">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="85b02-202">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="85b02-203">Environnement</span><span class="sxs-lookup"><span data-stu-id="85b02-203">Environment</span></span>

<span data-ttu-id="85b02-204">Définit l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="85b02-204">Sets the app's environment.</span></span>

<span data-ttu-id="85b02-205">**Clé** : environment</span><span class="sxs-lookup"><span data-stu-id="85b02-205">**Key**: environment</span></span>  
<span data-ttu-id="85b02-206">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="85b02-206">**Type**: *string*</span></span>  
<span data-ttu-id="85b02-207">**Valeur par défaut** : Production</span><span class="sxs-lookup"><span data-stu-id="85b02-207">**Default**: Production</span></span>  
<span data-ttu-id="85b02-208">**Définition avec** : `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="85b02-208">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="85b02-209">**Variable d’environnement** : `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="85b02-209">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="85b02-210">L’environnement peut être défini à n’importe quelle valeur.</span><span class="sxs-lookup"><span data-stu-id="85b02-210">The environment can be set to any value.</span></span> <span data-ttu-id="85b02-211">Les valeurs définies par le framework sont `Development`, `Staging` et `Production`.</span><span class="sxs-lookup"><span data-stu-id="85b02-211">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="85b02-212">Les valeurs ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="85b02-212">Values aren't case sensitive.</span></span> <span data-ttu-id="85b02-213">Par défaut, *l’environnement* est fourni par la variable d’environnement `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="85b02-213">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="85b02-214">Si vous utilisez [Visual Studio](https://www.visualstudio.com/), les variables d’environnement peuvent être définies dans le fichier *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="85b02-214">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="85b02-215">Pour plus d’informations, consultez [Utiliser plusieurs environnements](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="85b02-215">For more information, see [Work with multiple environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="85b02-216">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="85b02-216">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="85b02-217">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="85b02-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="85b02-218">Assemblys d’hébergement au démarrage</span><span class="sxs-lookup"><span data-stu-id="85b02-218">Hosting Startup Assemblies</span></span>

<span data-ttu-id="85b02-219">Définit les assemblys d’hébergement au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="85b02-219">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="85b02-220">**Clé** : hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="85b02-220">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="85b02-221">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="85b02-221">**Type**: *string*</span></span>  
<span data-ttu-id="85b02-222">**Valeur par défaut** : une chaîne vide</span><span class="sxs-lookup"><span data-stu-id="85b02-222">**Default**: Empty string</span></span>  
<span data-ttu-id="85b02-223">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="85b02-223">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="85b02-224">**Variable d’environnement** : `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="85b02-224">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="85b02-225">Chaîne délimitée par des points-virgules qui spécifie les assemblys d’hébergement à charger au démarrage.</span><span class="sxs-lookup"><span data-stu-id="85b02-225">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="85b02-226">Cette fonctionnalité est nouvelle dans ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="85b02-226">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="85b02-227">La valeur de configuration par défaut est une chaîne vide, mais les assemblys d’hébergement au démarrage incluent toujours l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="85b02-227">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="85b02-228">Quand des assemblys d’hébergement au démarrage sont fournis, ils sont ajoutés à l’assembly de l’application et sont chargés lorsque l’application génère ses services communs au démarrage.</span><span class="sxs-lookup"><span data-stu-id="85b02-228">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="85b02-229">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="85b02-229">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="85b02-230">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="85b02-230">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="85b02-231">Cette fonctionnalité n’est pas disponible dans ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="85b02-231">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="85b02-232">URL d’hébergement préférées</span><span class="sxs-lookup"><span data-stu-id="85b02-232">Prefer Hosting URLs</span></span>

<span data-ttu-id="85b02-233">Indique si l’hôte doit écouter les URL configurées avec `WebHostBuilder` au lieu d’écouter celles configurées avec l’implémentation `IServer`.</span><span class="sxs-lookup"><span data-stu-id="85b02-233">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="85b02-234">**Clé** : preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="85b02-234">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="85b02-235">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="85b02-235">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="85b02-236">**Valeur par défaut** : true</span><span class="sxs-lookup"><span data-stu-id="85b02-236">**Default**: true</span></span>  
<span data-ttu-id="85b02-237">**Définition avec** : `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="85b02-237">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="85b02-238">**Variable d’environnement** : `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="85b02-238">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="85b02-239">Cette fonctionnalité est nouvelle dans ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="85b02-239">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="85b02-240">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="85b02-240">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="85b02-241">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="85b02-241">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="85b02-242">Cette fonctionnalité n’est pas disponible dans ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="85b02-242">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="85b02-243">Bloquer les assemblys d’hébergement au démarrage</span><span class="sxs-lookup"><span data-stu-id="85b02-243">Prevent Hosting Startup</span></span>

<span data-ttu-id="85b02-244">Empêche le chargement automatique des assemblys d’hébergement au démarrage, y compris ceux configurés par l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="85b02-244">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="85b02-245">Pour plus d’informations, consultez [Ajouter des fonctionnalités d’application à l’aide d’une configuration spécifique à une plateforme](xref:host-and-deploy/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="85b02-245">See [Add app features using a platform-specific configuration](xref:host-and-deploy/platform-specific-configuration) for more information.</span></span>

<span data-ttu-id="85b02-246">**Clé** : preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="85b02-246">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="85b02-247">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="85b02-247">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="85b02-248">**Valeur par défaut** : false</span><span class="sxs-lookup"><span data-stu-id="85b02-248">**Default**: false</span></span>  
<span data-ttu-id="85b02-249">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="85b02-249">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="85b02-250">**Variable d’environnement** : `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="85b02-250">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="85b02-251">Cette fonctionnalité est nouvelle dans ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="85b02-251">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="85b02-252">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="85b02-252">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="85b02-253">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="85b02-253">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="85b02-254">Cette fonctionnalité n’est pas disponible dans ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="85b02-254">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="85b02-255">URL du serveur</span><span class="sxs-lookup"><span data-stu-id="85b02-255">Server URLs</span></span>

<span data-ttu-id="85b02-256">Indique les adresses IP ou les adresses d’hôte avec les ports et protocoles sur lesquels le serveur doit écouter les requêtes.</span><span class="sxs-lookup"><span data-stu-id="85b02-256">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="85b02-257">**Clé** : urls</span><span class="sxs-lookup"><span data-stu-id="85b02-257">**Key**: urls</span></span>  
<span data-ttu-id="85b02-258">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="85b02-258">**Type**: *string*</span></span>  
<span data-ttu-id="85b02-259">**Par défaut** : http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="85b02-259">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="85b02-260">**Définition avec** : `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="85b02-260">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="85b02-261">**Variable d’environnement** : `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="85b02-261">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="85b02-262">Liste de préfixes d’URL séparés par des points-virgules (;) correspondant aux URL auxquelles le serveur doit répondre.</span><span class="sxs-lookup"><span data-stu-id="85b02-262">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="85b02-263">Par exemple, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="85b02-263">For example, `http://localhost:123`.</span></span> <span data-ttu-id="85b02-264">Utilisez « \* » pour indiquer que le serveur doit écouter les requêtes sur toutes les adresses IP ou tous les noms d’hôte qui utilisent le port et le protocole spécifiés (par exemple, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="85b02-264">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="85b02-265">Le protocole (`http://` ou `https://`) doit être inclus avec chaque URL.</span><span class="sxs-lookup"><span data-stu-id="85b02-265">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="85b02-266">Les formats pris en charge varient selon les serveurs.</span><span class="sxs-lookup"><span data-stu-id="85b02-266">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="85b02-267">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="85b02-267">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="85b02-268">Kestrel a sa propre API de configuration de points de terminaison.</span><span class="sxs-lookup"><span data-stu-id="85b02-268">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="85b02-269">Pour plus d’informations, consultez [Implémentation du serveur web Kestrel dans ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="85b02-269">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="85b02-270">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="85b02-270">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="85b02-271">Délai d’arrêt</span><span class="sxs-lookup"><span data-stu-id="85b02-271">Shutdown Timeout</span></span>

<span data-ttu-id="85b02-272">Spécifie la durée d’attente avant l’arrêt de l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="85b02-272">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="85b02-273">**Clé** : shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="85b02-273">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="85b02-274">**Type** : *int*</span><span class="sxs-lookup"><span data-stu-id="85b02-274">**Type**: *int*</span></span>  
<span data-ttu-id="85b02-275">**Valeur par défaut** : 5</span><span class="sxs-lookup"><span data-stu-id="85b02-275">**Default**: 5</span></span>  
<span data-ttu-id="85b02-276">**Définition avec** : `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="85b02-276">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="85b02-277">**Variable d’environnement** : `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="85b02-277">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="85b02-278">La clé accepte une valeur *int* avec `UseSetting` (par exemple, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), mais la méthode d’extension [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) prend une valeur [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="85b02-278">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span> <span data-ttu-id="85b02-279">Cette fonctionnalité est nouvelle dans ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="85b02-279">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="85b02-280">Pendant la période du délai d’attente, l’hébergement :</span><span class="sxs-lookup"><span data-stu-id="85b02-280">During the timeout period, hosting:</span></span>

* <span data-ttu-id="85b02-281">Déclenche [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="85b02-281">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="85b02-282">Tente d’arrêter les services hébergés, en journalisant les erreurs pour les services qui échouent à s’arrêter.</span><span class="sxs-lookup"><span data-stu-id="85b02-282">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="85b02-283">Si la période du délai d’attente expire avant l’arrêt de tous les services hébergés, les services actifs restants sont arrêtés quand l’application s’arrête.</span><span class="sxs-lookup"><span data-stu-id="85b02-283">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="85b02-284">Les services s’arrêtent même s’ils n’ont pas terminé les traitements.</span><span class="sxs-lookup"><span data-stu-id="85b02-284">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="85b02-285">Si des services nécessitent plus de temps pour s’arrêter, augmentez le délai d’attente.</span><span class="sxs-lookup"><span data-stu-id="85b02-285">If services require additional time to stop, increase the timeout.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="85b02-286">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="85b02-286">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="85b02-287">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="85b02-287">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="85b02-288">Cette fonctionnalité n’est pas disponible dans ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="85b02-288">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="85b02-289">Assembly de démarrage</span><span class="sxs-lookup"><span data-stu-id="85b02-289">Startup Assembly</span></span>

<span data-ttu-id="85b02-290">Détermine l’assembly à rechercher pour la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="85b02-290">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="85b02-291">**Clé** : startupAssembly</span><span class="sxs-lookup"><span data-stu-id="85b02-291">**Key**: startupAssembly</span></span>  
<span data-ttu-id="85b02-292">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="85b02-292">**Type**: *string*</span></span>  
<span data-ttu-id="85b02-293">**Valeur par défaut** : l’assembly de l’application</span><span class="sxs-lookup"><span data-stu-id="85b02-293">**Default**: The app's assembly</span></span>  
<span data-ttu-id="85b02-294">**Définition avec** : `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="85b02-294">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="85b02-295">**Variable d’environnement** : `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="85b02-295">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="85b02-296">L’assembly peut être référencé par nom (`string`) ou type (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="85b02-296">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="85b02-297">Si plusieurs méthodes `UseStartup` sont appelées, la dernière prévaut.</span><span class="sxs-lookup"><span data-stu-id="85b02-297">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="85b02-298">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="85b02-298">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="85b02-299">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="85b02-299">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
    ...
```

---

### <a name="web-root"></a><span data-ttu-id="85b02-300">Racine Web</span><span class="sxs-lookup"><span data-stu-id="85b02-300">Web Root</span></span>

<span data-ttu-id="85b02-301">Définit le chemin relatif des ressources statiques de l’application.</span><span class="sxs-lookup"><span data-stu-id="85b02-301">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="85b02-302">**Clé** : webroot</span><span class="sxs-lookup"><span data-stu-id="85b02-302">**Key**: webroot</span></span>  
<span data-ttu-id="85b02-303">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="85b02-303">**Type**: *string*</span></span>  
<span data-ttu-id="85b02-304">**Valeur par défaut** : quand aucune valeur n’est spécifiée, la valeur par défaut est « (Racine de contenu)/wwwroot », si ce chemin existe.</span><span class="sxs-lookup"><span data-stu-id="85b02-304">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="85b02-305">Si ce chemin est introuvable, un fournisseur de fichiers no-op est utilisé.</span><span class="sxs-lookup"><span data-stu-id="85b02-305">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="85b02-306">**Définition avec** : `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="85b02-306">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="85b02-307">**Variable d’environnement** : `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="85b02-307">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="85b02-308">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="85b02-308">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="85b02-309">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="85b02-309">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="85b02-310">Substitution d’une configuration</span><span class="sxs-lookup"><span data-stu-id="85b02-310">Overriding configuration</span></span>

<span data-ttu-id="85b02-311">Utilisez [Configuration](xref:fundamentals/configuration/index) pour configurer l’hôte.</span><span class="sxs-lookup"><span data-stu-id="85b02-311">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="85b02-312">Dans l’exemple suivant, la configuration de l’hôte est facultativement spécifiée dans un fichier *hosting.json*.</span><span class="sxs-lookup"><span data-stu-id="85b02-312">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="85b02-313">Une configuration chargée à partir du fichier *hosting.json* peut être remplacée par des arguments de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="85b02-313">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="85b02-314">La configuration intégrée (dans `config`) est utilisée pour configurer l’hôte avec `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="85b02-314">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="85b02-315">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="85b02-315">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="85b02-316">*hosting.json* :</span><span class="sxs-lookup"><span data-stu-id="85b02-316">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="85b02-317">La configuration fournie par `UseUrls` est d’abord remplacée par la configuration du fichier *hosting.json* et ensuite par la configuration des arguments de ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="85b02-317">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();
    }
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="85b02-318">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="85b02-318">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="85b02-319">*hosting.json* :</span><span class="sxs-lookup"><span data-stu-id="85b02-319">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="85b02-320">La configuration fournie par `UseUrls` est d’abord remplacée par la configuration du fichier *hosting.json* et ensuite par la configuration des arguments de ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="85b02-320">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

---

> [!NOTE]
> <span data-ttu-id="85b02-321">La méthode d’extension [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) ne peut pas analyser une section de configuration retournée par `GetSection` (par exemple, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="85b02-321">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="85b02-322">La méthode `GetSection` filtre les clés de configuration dans la section demandée, mais laisse le nom de la section dans les clés (par exemple, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="85b02-322">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="85b02-323">La méthode `UseConfiguration` suppose que les clés correspondent aux clés `WebHostBuilder` (par exemple, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="85b02-323">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="85b02-324">La présence du nom de la section dans les clés empêche l’utilisation des valeurs de la section pour configurer l’hôte.</span><span class="sxs-lookup"><span data-stu-id="85b02-324">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="85b02-325">Ce problème sera résolu dans une prochaine mise en production.</span><span class="sxs-lookup"><span data-stu-id="85b02-325">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="85b02-326">Pour obtenir plus d’informations et connaître les solutions de contournement possibles, consultez [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="85b02-326">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="85b02-327">Pour spécifier l’exécution de l’hôte sur une URL particulière, vous pouvez passer la valeur souhaitée à partir d’une invite de commandes lors de l’exécution de [dotnet run](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="85b02-327">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="85b02-328">L’argument de ligne de commande se substitue à la valeur `urls` fournie par le fichier *hosting.json*, et le serveur écoute le port 8080 :</span><span class="sxs-lookup"><span data-stu-id="85b02-328">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="85b02-329">Démarrage de l’hôte</span><span class="sxs-lookup"><span data-stu-id="85b02-329">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="85b02-330">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="85b02-330">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="85b02-331">**Exécuter**</span><span class="sxs-lookup"><span data-stu-id="85b02-331">**Run**</span></span>

<span data-ttu-id="85b02-332">La méthode `Run` démarre l’application web et bloque le thread appelant jusqu’à l’arrêt de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="85b02-332">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="85b02-333">**Start**</span><span class="sxs-lookup"><span data-stu-id="85b02-333">**Start**</span></span>

<span data-ttu-id="85b02-334">Appelez la méthode `Start` pour exécuter l’hôte en mode non bloquant :</span><span class="sxs-lookup"><span data-stu-id="85b02-334">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="85b02-335">Si une liste d’URL est passée à la méthode `Start`, l’hôte écoute les URL spécifiées :</span><span class="sxs-lookup"><span data-stu-id="85b02-335">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

<span data-ttu-id="85b02-336">L’application peut initialiser et démarrer un nouvel hôte ayant les valeurs par défaut préconfigurées de `CreateDefaultBuilder` en utilisant une méthode d’usage statique.</span><span class="sxs-lookup"><span data-stu-id="85b02-336">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="85b02-337">Ces méthodes démarrent le serveur sans sortie de console et, avec [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown), elles attendent un arrêt (Ctrl-C/SIGINT ou SIGTERM) :</span><span class="sxs-lookup"><span data-stu-id="85b02-337">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="85b02-338">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="85b02-338">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="85b02-339">Commencez par un `RequestDelegate` :</span><span class="sxs-lookup"><span data-stu-id="85b02-339">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="85b02-340">Envoyez une requête à `http://localhost:5000` dans le navigateur pour recevoir la réponse « Hello World! »</span><span class="sxs-lookup"><span data-stu-id="85b02-340">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="85b02-341">`WaitForShutdown` bloque la requête jusqu’à l’émission d’une commande d’arrêt (Ctrl-C/SIGINT ou SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="85b02-341">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="85b02-342">L’application affiche le message `Console.WriteLine` et attend que l’utilisateur appuie sur une touche pour s’arrêter.</span><span class="sxs-lookup"><span data-stu-id="85b02-342">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="85b02-343">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="85b02-343">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="85b02-344">Commencez par une URL et `RequestDelegate` :</span><span class="sxs-lookup"><span data-stu-id="85b02-344">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="85b02-345">Produit le même résultat que **Start(RequestDelegate app)**, sauf que l’application répond sur `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="85b02-345">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="85b02-346">**Start(Action<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="85b02-346">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="85b02-347">Utilisez une instance de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) pour utiliser le middleware de routage :</span><span class="sxs-lookup"><span data-stu-id="85b02-347">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="85b02-348">Utilisez les requêtes de navigateur suivantes avec l’exemple :</span><span class="sxs-lookup"><span data-stu-id="85b02-348">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="85b02-349">Demande</span><span class="sxs-lookup"><span data-stu-id="85b02-349">Request</span></span>                                    | <span data-ttu-id="85b02-350">Réponse</span><span class="sxs-lookup"><span data-stu-id="85b02-350">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="85b02-351">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="85b02-351">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="85b02-352">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="85b02-352">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="85b02-353">Lève une exception avec la chaîne « ooops! »</span><span class="sxs-lookup"><span data-stu-id="85b02-353">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="85b02-354">Lève une exception avec la chaîne « Uh oh! »</span><span class="sxs-lookup"><span data-stu-id="85b02-354">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="85b02-355">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="85b02-355">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="85b02-356">Hello World!</span><span class="sxs-lookup"><span data-stu-id="85b02-356">Hello World!</span></span>                             |

<span data-ttu-id="85b02-357">`WaitForShutdown` bloque la requête jusqu’à l’émission d’une commande d’arrêt (Ctrl-C/SIGINT ou SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="85b02-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="85b02-358">L’application affiche le message `Console.WriteLine` et attend que l’utilisateur appuie sur une touche pour s’arrêter.</span><span class="sxs-lookup"><span data-stu-id="85b02-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="85b02-359">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="85b02-359">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="85b02-360">Utilisez une URL et une instance de `IRouteBuilder` :</span><span class="sxs-lookup"><span data-stu-id="85b02-360">Use a URL and an instance of `IRouteBuilder`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="85b02-361">Produit le même résultat que **Start(Action<IRouteBuilder> routeBuilder)**, sauf que l’application répond sur `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="85b02-361">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="85b02-362">**StartWith(Action<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="85b02-362">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="85b02-363">Fournissez un délégué pour configurer un `IApplicationBuilder` :</span><span class="sxs-lookup"><span data-stu-id="85b02-363">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="85b02-364">Envoyez une requête à `http://localhost:5000` dans le navigateur pour recevoir la réponse « Hello World! »</span><span class="sxs-lookup"><span data-stu-id="85b02-364">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="85b02-365">`WaitForShutdown` bloque la requête jusqu’à l’émission d’une commande d’arrêt (Ctrl-C/SIGINT ou SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="85b02-365">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="85b02-366">L’application affiche le message `Console.WriteLine` et attend que l’utilisateur appuie sur une touche pour s’arrêter.</span><span class="sxs-lookup"><span data-stu-id="85b02-366">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="85b02-367">**StartWith(string url, Action<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="85b02-367">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="85b02-368">Fournissez une URL et un délégué pour configurer un `IApplicationBuilder` :</span><span class="sxs-lookup"><span data-stu-id="85b02-368">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="85b02-369">Produit le même résultat que **StartWith(Action<IApplicationBuilder> app)**, sauf que l’application répond sur `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="85b02-369">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="85b02-370">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="85b02-370">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="85b02-371">**Exécuter**</span><span class="sxs-lookup"><span data-stu-id="85b02-371">**Run**</span></span>

<span data-ttu-id="85b02-372">La méthode `Run` démarre l’application web et bloque le thread appelant jusqu’à l’arrêt de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="85b02-372">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="85b02-373">**Start**</span><span class="sxs-lookup"><span data-stu-id="85b02-373">**Start**</span></span>

<span data-ttu-id="85b02-374">Appelez la méthode `Start` pour exécuter l’hôte en mode non bloquant :</span><span class="sxs-lookup"><span data-stu-id="85b02-374">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="85b02-375">Si une liste d’URL est passée à la méthode `Start`, l’hôte écoute les URL spécifiées :</span><span class="sxs-lookup"><span data-stu-id="85b02-375">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

---

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="85b02-376">Interface IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="85b02-376">IHostingEnvironment interface</span></span>

<span data-ttu-id="85b02-377">[L’interface IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) fournit des informations sur l’environnement d’hébergement web de l’application.</span><span class="sxs-lookup"><span data-stu-id="85b02-377">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="85b02-378">Utilisez [l’injection de constructeur](xref:fundamentals/dependency-injection) pour obtenir l’interface `IHostingEnvironment` afin d’utiliser ses propriétés et méthodes d’extension :</span><span class="sxs-lookup"><span data-stu-id="85b02-378">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

<span data-ttu-id="85b02-379">Vous pouvez utiliser une [approche basée sur une convention](xref:fundamentals/environments#startup-conventions) pour configurer l’application au démarrage en fonction de l’environnement.</span><span class="sxs-lookup"><span data-stu-id="85b02-379">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="85b02-380">Vous pouvez également injecter `IHostingEnvironment` dans le constructeur `Startup` pour l’utiliser dans `ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="85b02-380">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> <span data-ttu-id="85b02-381">En plus de la méthode d’extension `IsDevelopment`, `IHostingEnvironment` fournit les méthodes `IsStaging`, `IsProduction` et `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="85b02-381">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="85b02-382">Pour plus d’informations, consultez [Utiliser plusieurs environnements](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="85b02-382">See [Work with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="85b02-383">Le service `IHostingEnvironment` peut également être injecté directement dans la méthode `Configure` pour configurer le pipeline de traitement :</span><span class="sxs-lookup"><span data-stu-id="85b02-383">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

<span data-ttu-id="85b02-384">`IHostingEnvironment` peut être injecté dans la méthode `Invoke` lors de la création d’un [intergiciel (middleware)](xref:fundamentals/middleware/index#writing-middleware) personnalisé :</span><span class="sxs-lookup"><span data-stu-id="85b02-384">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="85b02-385">Interface IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="85b02-385">IApplicationLifetime interface</span></span>

<span data-ttu-id="85b02-386">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) s’utilise pour les opérations post-démarrage et arrêt.</span><span class="sxs-lookup"><span data-stu-id="85b02-386">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="85b02-387">Trois propriétés de l’interface sont des jetons d’annulation utilisés pour inscrire les méthodes `Action` qui définissent les événements de démarrage et d’arrêt.</span><span class="sxs-lookup"><span data-stu-id="85b02-387">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="85b02-388">Jeton d’annulation</span><span class="sxs-lookup"><span data-stu-id="85b02-388">Cancellation Token</span></span>    | <span data-ttu-id="85b02-389">Événement déclencheur</span><span class="sxs-lookup"><span data-stu-id="85b02-389">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="85b02-390">L’hôte a complètement démarré.</span><span class="sxs-lookup"><span data-stu-id="85b02-390">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="85b02-391">L’hôte effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="85b02-391">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="85b02-392">Des requêtes peuvent encore être en cours de traitement.</span><span class="sxs-lookup"><span data-stu-id="85b02-392">Requests may still be processing.</span></span> <span data-ttu-id="85b02-393">L’arrêt est bloqué tant que cet événement n’est pas terminé.</span><span class="sxs-lookup"><span data-stu-id="85b02-393">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="85b02-394">L’hôte effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="85b02-394">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="85b02-395">Toutes les requêtes doivent être traitées.</span><span class="sxs-lookup"><span data-stu-id="85b02-395">All requests should be processed.</span></span> <span data-ttu-id="85b02-396">L’arrêt est bloqué tant que cet événement n’est pas terminé.</span><span class="sxs-lookup"><span data-stu-id="85b02-396">Shutdown blocks until this event completes.</span></span> |

```csharp
public class Startup 
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime) 
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

<span data-ttu-id="85b02-397">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requête l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="85b02-397">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="85b02-398">La classe suivante utilise `StopApplication` pour arrêter normalement une application lors de l’appel de la méthode `Shutdown` de la classe :</span><span class="sxs-lookup"><span data-stu-id="85b02-398">The following class uses `StopApplication` to gracefully shutdown an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="85b02-399">Validation de l’étendue</span><span class="sxs-lookup"><span data-stu-id="85b02-399">Scope validation</span></span>

<span data-ttu-id="85b02-400">Dans ASP.NET Core 2.0 ou ultérieur, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) définit [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) sur `true` si l’environnement de l’application est Développement.</span><span class="sxs-lookup"><span data-stu-id="85b02-400">In ASP.NET Core 2.0 or later, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="85b02-401">Quand `ValidateScopes` est défini sur `true`, le fournisseur de services par défaut vérifie que :</span><span class="sxs-lookup"><span data-stu-id="85b02-401">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="85b02-402">Les services délimités ne sont pas résolus directement ou indirectement à partir du fournisseur de services racine.</span><span class="sxs-lookup"><span data-stu-id="85b02-402">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="85b02-403">Les services délimités ne sont pas directement ou indirectement injectés dans des singletons.</span><span class="sxs-lookup"><span data-stu-id="85b02-403">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="85b02-404">Le fournisseur de services racine est créé quand [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) est appelé.</span><span class="sxs-lookup"><span data-stu-id="85b02-404">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="85b02-405">La durée de vie du fournisseur de services racine correspond à la durée de vie de l’application/du serveur quand le fournisseur démarre avec l’application et qu’il est supprimé quand l’application s’arrête.</span><span class="sxs-lookup"><span data-stu-id="85b02-405">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="85b02-406">Les services délimités sont supprimés par le conteneur qui les a créés.</span><span class="sxs-lookup"><span data-stu-id="85b02-406">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="85b02-407">Si un service délimité est créé dans le conteneur racine, la durée de vie du service est promue en singleton, car elle est supprimée par le conteneur racine seulement quand l’application/le serveur est arrêté.</span><span class="sxs-lookup"><span data-stu-id="85b02-407">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="85b02-408">La validation des étendues du service permet de traiter ces situations quand `BuildServiceProvider` est appelé.</span><span class="sxs-lookup"><span data-stu-id="85b02-408">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="85b02-409">Pour toujours valider les étendues, notamment dans l’environnement de production, configurez [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) avec [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) sur le créateur d’hôte :</span><span class="sxs-lookup"><span data-stu-id="85b02-409">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="85b02-410">Dépannage de System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="85b02-410">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="85b02-411">**S’applique à ASP.NET Core 2.0 uniquement**</span><span class="sxs-lookup"><span data-stu-id="85b02-411">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="85b02-412">Vous pouvez créer un hôte en injectant `IStartup` directement dans le conteneur d’injection de dépendances au lieu d’appeler `UseStartup` ou `Configure` :</span><span class="sxs-lookup"><span data-stu-id="85b02-412">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="85b02-413">Dans ce cas, l’erreur suivante peut se produire :</span><span class="sxs-lookup"><span data-stu-id="85b02-413">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="85b02-414">Cette erreur se produit, car [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (l’assembly actuel) est requis pour analyser `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="85b02-414">This occurs because the [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="85b02-415">Si l’application injecte `IStartup` manuellement dans le conteneur d’injection de dépendances, ajoutez l’appel suivant à `WebHostBuilder` en spécifiant le nom de l’assembly :</span><span class="sxs-lookup"><span data-stu-id="85b02-415">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="85b02-416">Vous pouvez également ajouter un `Configure` factice à `WebHostBuilder`, qui définit automatiquement la valeur `applicationName`(`ApplicationKey`) :</span><span class="sxs-lookup"><span data-stu-id="85b02-416">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="85b02-417">**REMARQUE** : Cela est nécessaire uniquement dans ASP.NET Core 2.0 et si l’application n’appelle pas `UseStartup` ou `Configure`.</span><span class="sxs-lookup"><span data-stu-id="85b02-417">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="85b02-418">Pour plus d’informations, consultez le commentaire [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) et [l’exemple StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="85b02-418">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="85b02-419">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="85b02-419">Additional resources</span></span>

* [<span data-ttu-id="85b02-420">Héberger sur Windows avec IIS</span><span class="sxs-lookup"><span data-stu-id="85b02-420">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="85b02-421">Héberger sur Linux avec Nginx</span><span class="sxs-lookup"><span data-stu-id="85b02-421">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="85b02-422">Héberger sur Linux avec Apache</span><span class="sxs-lookup"><span data-stu-id="85b02-422">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="85b02-423">Héberger dans un service Windows</span><span class="sxs-lookup"><span data-stu-id="85b02-423">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
