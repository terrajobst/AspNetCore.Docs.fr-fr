---
title: Hôte web ASP.NET Core
author: guardrex
description: Découvrez l’hôte web dans ASP.NET Core, qui est responsable de la gestion du démarrage et de la durée de vie des applications.
ms.author: riande
ms.custom: mvc
ms.date: 09/01/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: 7440ab26534840b190a346614f645860fc2b7d78
ms.sourcegitcommit: 7211ae2dd702f67d36365831c490d6178c9a46c8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44089897"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="0b0ed-103">Hôte web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b0ed-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="0b0ed-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0b0ed-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0b0ed-105">Les applications ASP.NET Core configurent et lancent un *hôte*.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="0b0ed-106">L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="0b0ed-107">Au minimum, l’hôte configure un serveur ainsi qu’un pipeline de traitement des requêtes.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="0b0ed-108">Cette rubrique traite de l’hôte web ASP.NET Core ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), qui est utile pour l’hébergement d’applications web.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-108">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="0b0ed-109">Pour découvrir l’hôte générique .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), consultez <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-109">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="0b0ed-110">Configurer un hôte</span><span class="sxs-lookup"><span data-stu-id="0b0ed-110">Set up a host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0b0ed-111">Créez un hôte en utilisant une instance de [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-111">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="0b0ed-112">Cette opération est généralement effectuée au point d’entrée de l’application, à savoir la méthode `Main`.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-112">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="0b0ed-113">Dans les modèles de projet, `Main` se trouve dans *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="0b0ed-114">Un fichier *Program.cs* standard appelle [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) pour commencer la configuration d’un hôte :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

<span data-ttu-id="0b0ed-115">`CreateDefaultBuilder` effectue les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="0b0ed-116">Configure [Kestrel](xref:fundamentals/servers/kestrel) en tant que serveur web et configure le serveur à l’aide des fournisseurs de configuration d’hébergement de l’application.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-116">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="0b0ed-117">Pour connaître les options par défaut de Kestrel, consultez <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-117">For the Kestrel default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="0b0ed-118">Définit le chemin retourné par [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) comme racine de contenu.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-118">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="0b0ed-119">Charge la [configuration de l’hôte](#host-configuration-values) à partir de :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-119">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="0b0ed-120">Variables d’environnement comportant le préfixe `ASPNETCORE_` (par exemple, `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-120">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="0b0ed-121">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="0b0ed-121">Command-line arguments.</span></span>
* <span data-ttu-id="0b0ed-122">Charge la configuration de l’application à partir de :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-122">Loads app configuration from:</span></span>
  * <span data-ttu-id="0b0ed-123">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="0b0ed-123">*appsettings.json*.</span></span>
  * <span data-ttu-id="0b0ed-124">*appsettings.{Environment}.json*</span><span class="sxs-lookup"><span data-stu-id="0b0ed-124">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="0b0ed-125">Les [secrets utilisateur](xref:security/app-secrets) quand l’application s’exécute dans l’environnement `Development` à l’aide de l’assembly d’entrée</span><span class="sxs-lookup"><span data-stu-id="0b0ed-125">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="0b0ed-126">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-126">Environment variables.</span></span>
  * <span data-ttu-id="0b0ed-127">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="0b0ed-127">Command-line arguments.</span></span>
* <span data-ttu-id="0b0ed-128">Configure la [journalisation](xref:fundamentals/logging/index) des sorties de la console et du débogage.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-128">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="0b0ed-129">La journalisation inclut les règles de [filtrage de journal](xref:fundamentals/logging/index#log-filtering) qui sont spécifiées dans une section de configuration de la journalisation dans un fichier *appsettings.json* ou *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-129">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="0b0ed-130">Active [l’intégration IIS](xref:host-and-deploy/iis/index) quand il est exécuté derrière IIS.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-130">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="0b0ed-131">Configure le chemin de base et le port écouté par le serveur lors de l’utilisation du [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-131">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="0b0ed-132">Le module crée un proxy inverse entre IIS et Kestrel.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-132">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="0b0ed-133">Il configure également l’application pour la [capture des erreurs de démarrage](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-133">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="0b0ed-134">Pour connaître les options par défaut d’IIS, consultez <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-134">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="0b0ed-135">Définissez [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) sur `true` si l’environnement de l’application est Développement.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-135">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="0b0ed-136">Pour plus d’informations, consultez [Validation de l’étendue](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-136">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="0b0ed-137">La configuration définie par `CreateDefaultBuilder` peut être remplacée et enrichie par [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging) et les autres méthodes et les méthodes d’extension de [ IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-137">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="0b0ed-138">En voici quelques exemples :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-138">A few examples follow:</span></span>

* <span data-ttu-id="0b0ed-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) permet de spécifier une configuration `IConfiguration` supplémentaire pour l’application.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="0b0ed-140">L’appel `ConfigureAppConfiguration` suivant ajoute un délégué pour inclure la configuration de l’application dans le fichier *appsettings.xml*.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-140">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="0b0ed-141">`ConfigureAppConfiguration` peut être appelé plusieurs fois.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-141">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="0b0ed-142">Notez que cette configuration ne s’applique pas à l’hôte (par exemple, les URL de serveur ou l’environnement).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-142">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="0b0ed-143">Voir la section [Valeurs de configuration de l’hôte](#host-configuration-values).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-143">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="0b0ed-144">L’appel `ConfigureLogging` suivant ajoute un délégué pour configurer le niveau de journalisation minimal ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) sur [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-144">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="0b0ed-145">Ce paramètre remplace les paramètres de *appsettings.Development.json* (`LogLevel.Debug`) et *appsettings.Production.json* (`LogLevel.Error`) configurés par `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-145">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="0b0ed-146">`ConfigureLogging` peut être appelé plusieurs fois.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-146">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="0b0ed-147">L’appel suivant à `ConfigureKestrel` remplace la valeur par défaut de [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) (30 000 000 octets) établie lors de la configuration de Kestrel par `CreateDefaultBuilder` :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-147">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

* <span data-ttu-id="0b0ed-148">L’appel suivant à [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) remplace la valeur par défaut de [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) (30 000 000 octets) établie lors de la configuration de Kestrel par `CreateDefaultBuilder` :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-148">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0b0ed-149">La *racine de contenu* détermine l’emplacement où l’hôte recherche les fichiers de contenu, tels que les fichiers de vue MVC.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-149">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="0b0ed-150">Quand l’application est démarrée à partir du dossier racine du projet, ce dossier est utilisé comme racine de contenu.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-150">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="0b0ed-151">Il s’agit du dossier par défaut utilisé dans [Visual Studio](https://www.visualstudio.com/) et les [modèles dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-151">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="0b0ed-152">Pour plus d’informations sur la configuration d’application, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-152">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="0b0ed-153">Au lieu d’utiliser la méthode statique `CreateDefaultBuilder`, vous pouvez aussi créer un hôte à partir de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Cette approche est prise en charge dans ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-153">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="0b0ed-154">Pour plus d’informations, consultez l’onglet ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-154">For more information, see the ASP.NET Core 1.x tab.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0b0ed-155">Créez un hôte en utilisant une instance de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-155">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="0b0ed-156">Cette opération est généralement effectuée au point d’entrée de l’application, à savoir la méthode `Main`.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-156">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="0b0ed-157">Dans les modèles de projet, `Main` se trouve dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-157">In the project templates, `Main` is located in *Program.cs*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>()
            .Build();
}
```

<span data-ttu-id="0b0ed-158">`WebHostBuilder` nécessite un serveur [ qui implémente IServer](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-158">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="0b0ed-159">Les serveurs intégrés sont [Kestrel](xref:fundamentals/servers/kestrel) et [HTTP.sys](xref:fundamentals/servers/httpsys) (dans les versions antérieures à ASP.NET Core 2.0, HTTP.sys était appelé [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-159">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="0b0ed-160">Dans cet exemple, la [méthode d’extension UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) spécifie le serveur Kestrel.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-160">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="0b0ed-161">La *racine de contenu* détermine l’emplacement où l’hôte recherche les fichiers de contenu, tels que les fichiers de vue MVC.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-161">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="0b0ed-162">La racine de contenu par défaut pour `UseContentRoot` est retournée par [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-162">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="0b0ed-163">Quand l’application est démarrée à partir du dossier racine du projet, ce dossier est utilisé comme racine de contenu.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-163">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="0b0ed-164">Il s’agit du dossier par défaut utilisé dans [Visual Studio](https://www.visualstudio.com/) et les [modèles dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-164">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="0b0ed-165">Pour utiliser IIS comme proxy inverse, appelez [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) dans le cadre de la création de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-165">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="0b0ed-166">`UseIISIntegration` ne configure pas un *serveur*, contrairement à ce que fait [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-166">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="0b0ed-167">`UseIISIntegration` configure le chemin de base et le port écouté par le serveur lors de l’utilisation du [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) pour créer un proxy inverse entre Kestrel et IIS.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-167">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="0b0ed-168">Pour utiliser IIS avec ASP.NET Core, `UseKestrel` et `UseIISIntegration` doivent être spécifiés.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-168">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="0b0ed-169">`UseIISIntegration` est activé uniquement dans le cadre d’une exécution derrière IIS ou IIS Express.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-169">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="0b0ed-170">Pour plus d’informations, consultez <xref:fundamentals/servers/aspnet-core-module> et <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-170">For more information, see <xref:fundamentals/servers/aspnet-core-module> and <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="0b0ed-171">Une implémentation minimale qui configure un hôte (et une application ASP.NET Core) consiste à spécifier un serveur et la configuration du pipeline des requêtes de l’application :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-171">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

::: moniker-end

<span data-ttu-id="0b0ed-172">Lors de la configuration d’un hôte, les méthodes [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) et [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) peuvent être fournies.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-172">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="0b0ed-173">Si une classe `Startup` est spécifiée, elle doit définir une méthode `Configure`.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-173">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="0b0ed-174">Pour plus d'informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-174">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="0b0ed-175">Les appels multiples à `ConfigureServices` s’ajoutent les uns aux autres.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-175">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="0b0ed-176">Les appels multiples à `Configure` ou `UseStartup` sur `WebHostBuilder` remplacent les paramètres précédents.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-176">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="0b0ed-177">Valeurs de configuration de l’hôte</span><span class="sxs-lookup"><span data-stu-id="0b0ed-177">Host configuration values</span></span>

<span data-ttu-id="0b0ed-178">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) s’appuie sur les approches suivantes pour définir les valeurs de configuration de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-178">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="0b0ed-179">Configuration du générateur de l’hôte, qui inclut des variables d’environnement au format `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-179">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="0b0ed-180">Par exemple, `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-180">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="0b0ed-181">Des extensions comme [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) et [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (voir la section [Remplacer la configuration](#override-configuration)).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-181">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="0b0ed-182">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) et la clé associée.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-182">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="0b0ed-183">Quand une valeur est définie avec `UseSetting`, elle est définie au format chaîne indépendamment du type.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-183">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="0b0ed-184">L’hôte utilise l’option qui définit une valeur en dernier.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-184">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="0b0ed-185">Pour plus d’informations, consultez [Remplacer une configuration](#override-configuration) dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-185">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="0b0ed-186">Clé d’application (Nom)</span><span class="sxs-lookup"><span data-stu-id="0b0ed-186">Application Key (Name)</span></span>

<span data-ttu-id="0b0ed-187">La propriété [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) est définie automatiquement quand [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) ou [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) est appelé pendant la construction de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-187">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="0b0ed-188">La valeur affectée correspond au nom de l’assembly contenant le point d’entrée de l’application.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-188">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="0b0ed-189">Pour définir la valeur explicitement, utilisez [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey) :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-189">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="0b0ed-190">**Clé** : applicationName</span><span class="sxs-lookup"><span data-stu-id="0b0ed-190">**Key**: applicationName</span></span>  
<span data-ttu-id="0b0ed-191">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="0b0ed-191">**Type**: *string*</span></span>  
<span data-ttu-id="0b0ed-192">**Par défaut** : nom de l’assembly contenant le point d’entrée de l’application.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-192">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="0b0ed-193">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-193">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="0b0ed-194">**Variable d’environnement** : `ASPNETCORE_APPLICATIONKEY`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-194">**Environment variable**: `ASPNETCORE_APPLICATIONKEY`</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```csharp
var host = new WebHostBuilder()
    .UseSetting("applicationName", "CustomApplicationName")
```

::: moniker-end

### <a name="capture-startup-errors"></a><span data-ttu-id="0b0ed-195">Capture des erreurs de démarrage</span><span class="sxs-lookup"><span data-stu-id="0b0ed-195">Capture Startup Errors</span></span>

<span data-ttu-id="0b0ed-196">Ce paramètre contrôle la capture des erreurs de démarrage.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-196">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="0b0ed-197">**Clé** : captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="0b0ed-197">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="0b0ed-198">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="0b0ed-198">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="0b0ed-199">**Valeur par défaut** : `false`, ou `true` si l’application s’exécute avec Kestrel derrière IIS.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-199">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="0b0ed-200">**Définition avec** : `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-200">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="0b0ed-201">**Variable d’environnement** : `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-201">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="0b0ed-202">Avec la valeur `false`, la survenue d’erreurs au démarrage entraîne la fermeture de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-202">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="0b0ed-203">Avec la valeur `true`, l’hôte capture les exceptions levées au démarrage et tente de démarrer le serveur.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-203">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

::: moniker-end

### <a name="content-root"></a><span data-ttu-id="0b0ed-204">Racine de contenu</span><span class="sxs-lookup"><span data-stu-id="0b0ed-204">Content Root</span></span>

<span data-ttu-id="0b0ed-205">Ce paramètre détermine l’emplacement où ASP.NET Core commence la recherche des fichiers de contenu, tels que les vues MVC.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-205">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="0b0ed-206">**Clé** : contentRoot</span><span class="sxs-lookup"><span data-stu-id="0b0ed-206">**Key**: contentRoot</span></span>  
<span data-ttu-id="0b0ed-207">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="0b0ed-207">**Type**: *string*</span></span>  
<span data-ttu-id="0b0ed-208">**Valeur par défaut** : dossier où réside l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-208">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="0b0ed-209">**Définition avec** : `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-209">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="0b0ed-210">**Variable d’environnement** : `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-210">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="0b0ed-211">La racine de contenu est également utilisée comme chemin de base pour le [paramètre de la racine Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-211">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="0b0ed-212">Si le chemin est introuvable, l’hôte ne peut pas démarrer.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-212">If the path doesn't exist, the host fails to start.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

### <a name="detailed-errors"></a><span data-ttu-id="0b0ed-213">Erreurs détaillées</span><span class="sxs-lookup"><span data-stu-id="0b0ed-213">Detailed Errors</span></span>

<span data-ttu-id="0b0ed-214">Détermine si les erreurs détaillées doivent être capturées.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-214">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="0b0ed-215">**Clé** : detailedErrors</span><span class="sxs-lookup"><span data-stu-id="0b0ed-215">**Key**: detailedErrors</span></span>  
<span data-ttu-id="0b0ed-216">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="0b0ed-216">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="0b0ed-217">**Valeur par défaut** : false</span><span class="sxs-lookup"><span data-stu-id="0b0ed-217">**Default**: false</span></span>  
<span data-ttu-id="0b0ed-218">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-218">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="0b0ed-219">**Variable d’environnement** : `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-219">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="0b0ed-220">Quand cette fonctionnalité est activée (ou que <a href="#environment">l’environnement</a> est défini sur `Development`), l’application capture les exceptions détaillées.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-220">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

### <a name="environment"></a><span data-ttu-id="0b0ed-221">Environnement</span><span class="sxs-lookup"><span data-stu-id="0b0ed-221">Environment</span></span>

<span data-ttu-id="0b0ed-222">Définit l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-222">Sets the app's environment.</span></span>

<span data-ttu-id="0b0ed-223">**Clé** : environment</span><span class="sxs-lookup"><span data-stu-id="0b0ed-223">**Key**: environment</span></span>  
<span data-ttu-id="0b0ed-224">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="0b0ed-224">**Type**: *string*</span></span>  
<span data-ttu-id="0b0ed-225">**Valeur par défaut** : Production</span><span class="sxs-lookup"><span data-stu-id="0b0ed-225">**Default**: Production</span></span>  
<span data-ttu-id="0b0ed-226">**Définition avec** : `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-226">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="0b0ed-227">**Variable d’environnement** : `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-227">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="0b0ed-228">L’environnement peut être défini à n’importe quelle valeur.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-228">The environment can be set to any value.</span></span> <span data-ttu-id="0b0ed-229">Les valeurs définies par le framework sont `Development`, `Staging` et `Production`.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-229">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="0b0ed-230">Les valeurs ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-230">Values aren't case sensitive.</span></span> <span data-ttu-id="0b0ed-231">Par défaut, *l’environnement* est fourni par la variable d’environnement `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-231">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="0b0ed-232">Si vous utilisez [Visual Studio](https://www.visualstudio.com/), les variables d’environnement peuvent être définies dans le fichier *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-232">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="0b0ed-233">Pour plus d'informations, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-233">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="0b0ed-234">Assemblys d’hébergement au démarrage</span><span class="sxs-lookup"><span data-stu-id="0b0ed-234">Hosting Startup Assemblies</span></span>

<span data-ttu-id="0b0ed-235">Définit les assemblys d’hébergement au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-235">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="0b0ed-236">**Clé** : hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="0b0ed-236">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="0b0ed-237">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="0b0ed-237">**Type**: *string*</span></span>  
<span data-ttu-id="0b0ed-238">**Valeur par défaut** : une chaîne vide</span><span class="sxs-lookup"><span data-stu-id="0b0ed-238">**Default**: Empty string</span></span>  
<span data-ttu-id="0b0ed-239">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-239">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="0b0ed-240">**Variable d’environnement** : `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-240">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="0b0ed-241">Chaîne délimitée par des points-virgules qui spécifie les assemblys d’hébergement à charger au démarrage.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-241">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="0b0ed-242">La valeur de configuration par défaut est une chaîne vide, mais les assemblys d’hébergement au démarrage incluent toujours l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-242">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="0b0ed-243">Quand des assemblys d’hébergement au démarrage sont fournis, ils sont ajoutés à l’assembly de l’application et sont chargés lorsque l’application génère ses services communs au démarrage.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-243">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="https-port"></a><span data-ttu-id="0b0ed-244">Port HTTPS</span><span class="sxs-lookup"><span data-stu-id="0b0ed-244">HTTPS Port</span></span>

<span data-ttu-id="0b0ed-245">Définissez le port de redirection HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-245">Set the HTTPS redirect port.</span></span> <span data-ttu-id="0b0ed-246">Utilisé dans [l’application de HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-246">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="0b0ed-247">**Clé** : https_port **Type** : *chaîne*
**Valeur par défaut** : une valeur par défaut n’est pas définie.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-247">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="0b0ed-248">**Définition avec** : `UseSetting`
**Variable d’environnement** : `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-248">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="0b0ed-249">Assemblys d’hébergement à exclure au démarrage</span><span class="sxs-lookup"><span data-stu-id="0b0ed-249">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="0b0ed-250">DESCRIPTION</span><span class="sxs-lookup"><span data-stu-id="0b0ed-250">DESCRIPTION</span></span>

<span data-ttu-id="0b0ed-251">**Clé** : hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="0b0ed-251">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="0b0ed-252">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="0b0ed-252">**Type**: *string*</span></span>  
<span data-ttu-id="0b0ed-253">**Valeur par défaut** : une chaîne vide</span><span class="sxs-lookup"><span data-stu-id="0b0ed-253">**Default**: Empty string</span></span>  
<span data-ttu-id="0b0ed-254">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-254">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="0b0ed-255">**Variable d’environnement** : `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-255">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="0b0ed-256">Chaîne délimitée par des points-virgules qui spécifie les assemblys d’hébergement à exclure au démarrage.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-256">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prefer-hosting-urls"></a><span data-ttu-id="0b0ed-257">URL d’hébergement préférées</span><span class="sxs-lookup"><span data-stu-id="0b0ed-257">Prefer Hosting URLs</span></span>

<span data-ttu-id="0b0ed-258">Indique si l’hôte doit écouter les URL configurées avec `WebHostBuilder` au lieu d’écouter celles configurées avec l’implémentation `IServer`.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-258">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="0b0ed-259">**Clé** : preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="0b0ed-259">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="0b0ed-260">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="0b0ed-260">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="0b0ed-261">**Valeur par défaut** : true</span><span class="sxs-lookup"><span data-stu-id="0b0ed-261">**Default**: true</span></span>  
<span data-ttu-id="0b0ed-262">**Définition avec** : `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-262">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="0b0ed-263">**Variable d’environnement** : `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-263">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prevent-hosting-startup"></a><span data-ttu-id="0b0ed-264">Bloquer les assemblys d’hébergement au démarrage</span><span class="sxs-lookup"><span data-stu-id="0b0ed-264">Prevent Hosting Startup</span></span>

<span data-ttu-id="0b0ed-265">Empêche le chargement automatique des assemblys d’hébergement au démarrage, y compris ceux configurés par l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-265">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="0b0ed-266">Pour plus d'informations, consultez <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-266">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="0b0ed-267">**Clé** : preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="0b0ed-267">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="0b0ed-268">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="0b0ed-268">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="0b0ed-269">**Valeur par défaut** : false</span><span class="sxs-lookup"><span data-stu-id="0b0ed-269">**Default**: false</span></span>  
<span data-ttu-id="0b0ed-270">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-270">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="0b0ed-271">**Variable d’environnement** : `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-271">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

::: moniker-end

### <a name="server-urls"></a><span data-ttu-id="0b0ed-272">URL du serveur</span><span class="sxs-lookup"><span data-stu-id="0b0ed-272">Server URLs</span></span>

<span data-ttu-id="0b0ed-273">Indique les adresses IP ou les adresses d’hôte avec les ports et protocoles sur lesquels le serveur doit écouter les requêtes.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-273">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="0b0ed-274">**Clé** : urls</span><span class="sxs-lookup"><span data-stu-id="0b0ed-274">**Key**: urls</span></span>  
<span data-ttu-id="0b0ed-275">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="0b0ed-275">**Type**: *string*</span></span>  
<span data-ttu-id="0b0ed-276">**Par défaut** : http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="0b0ed-276">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="0b0ed-277">**Définition avec** : `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-277">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="0b0ed-278">**Variable d’environnement** : `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-278">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="0b0ed-279">Liste de préfixes d’URL séparés par des points-virgules (;) correspondant aux URL auxquelles le serveur doit répondre.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-279">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="0b0ed-280">Par exemple, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-280">For example, `http://localhost:123`.</span></span> <span data-ttu-id="0b0ed-281">Utilisez « \* » pour indiquer que le serveur doit écouter les requêtes sur toutes les adresses IP ou tous les noms d’hôte qui utilisent le port et le protocole spécifiés (par exemple, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-281">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="0b0ed-282">Le protocole (`http://` ou `https://`) doit être inclus avec chaque URL.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-282">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="0b0ed-283">Les formats pris en charge varient selon les serveurs.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-283">Supported formats vary between servers.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="0b0ed-284">Kestrel a sa propre API de configuration de points de terminaison.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-284">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="0b0ed-285">Pour plus d'informations, consultez <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-285">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="shutdown-timeout"></a><span data-ttu-id="0b0ed-286">Délai d’arrêt</span><span class="sxs-lookup"><span data-stu-id="0b0ed-286">Shutdown Timeout</span></span>

<span data-ttu-id="0b0ed-287">Spécifie la durée d’attente avant l’arrêt de l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-287">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="0b0ed-288">**Clé** : shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="0b0ed-288">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="0b0ed-289">**Type** : *int*</span><span class="sxs-lookup"><span data-stu-id="0b0ed-289">**Type**: *int*</span></span>  
<span data-ttu-id="0b0ed-290">**Valeur par défaut** : 5</span><span class="sxs-lookup"><span data-stu-id="0b0ed-290">**Default**: 5</span></span>  
<span data-ttu-id="0b0ed-291">**Définition avec** : `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-291">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="0b0ed-292">**Variable d’environnement** : `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-292">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="0b0ed-293">La clé accepte une valeur *int* avec `UseSetting` (par exemple, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), mais la méthode d’extension [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) prend une valeur [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-293">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="0b0ed-294">Pendant la période du délai d’attente, l’hébergement :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-294">During the timeout period, hosting:</span></span>

* <span data-ttu-id="0b0ed-295">Déclenche [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-295">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="0b0ed-296">Tente d’arrêter les services hébergés, en journalisant les erreurs pour les services qui échouent à s’arrêter.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-296">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="0b0ed-297">Si la période du délai d’attente expire avant l’arrêt de tous les services hébergés, les services actifs restants sont arrêtés quand l’application s’arrête.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-297">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="0b0ed-298">Les services s’arrêtent même s’ils n’ont pas terminé les traitements.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-298">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="0b0ed-299">Si des services nécessitent plus de temps pour s’arrêter, augmentez le délai d’attente.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-299">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

::: moniker-end

### <a name="startup-assembly"></a><span data-ttu-id="0b0ed-300">Assembly de démarrage</span><span class="sxs-lookup"><span data-stu-id="0b0ed-300">Startup Assembly</span></span>

<span data-ttu-id="0b0ed-301">Détermine l’assembly à rechercher pour la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-301">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="0b0ed-302">**Clé** : startupAssembly</span><span class="sxs-lookup"><span data-stu-id="0b0ed-302">**Key**: startupAssembly</span></span>  
<span data-ttu-id="0b0ed-303">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="0b0ed-303">**Type**: *string*</span></span>  
<span data-ttu-id="0b0ed-304">**Valeur par défaut** : l’assembly de l’application</span><span class="sxs-lookup"><span data-stu-id="0b0ed-304">**Default**: The app's assembly</span></span>  
<span data-ttu-id="0b0ed-305">**Définition avec** : `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-305">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="0b0ed-306">**Variable d’environnement** : `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-306">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="0b0ed-307">L’assembly peut être référencé par nom (`string`) ou type (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-307">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="0b0ed-308">Si plusieurs méthodes `UseStartup` sont appelées, la dernière prévaut.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-308">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

::: moniker-end

### <a name="web-root"></a><span data-ttu-id="0b0ed-309">Racine Web</span><span class="sxs-lookup"><span data-stu-id="0b0ed-309">Web Root</span></span>

<span data-ttu-id="0b0ed-310">Définit le chemin relatif des ressources statiques de l’application.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-310">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="0b0ed-311">**Clé** : webroot</span><span class="sxs-lookup"><span data-stu-id="0b0ed-311">**Key**: webroot</span></span>  
<span data-ttu-id="0b0ed-312">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="0b0ed-312">**Type**: *string*</span></span>  
<span data-ttu-id="0b0ed-313">**Valeur par défaut** : quand aucune valeur n’est spécifiée, la valeur par défaut est « (Racine de contenu)/wwwroot », si ce chemin existe.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-313">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="0b0ed-314">Si ce chemin est introuvable, un fournisseur de fichiers no-op est utilisé.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-314">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="0b0ed-315">**Définition avec** : `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-315">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="0b0ed-316">**Variable d’environnement** : `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="0b0ed-316">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

::: moniker-end

## <a name="override-configuration"></a><span data-ttu-id="0b0ed-317">Remplacer la configuration</span><span class="sxs-lookup"><span data-stu-id="0b0ed-317">Override configuration</span></span>

<span data-ttu-id="0b0ed-318">Utilisez [Configuration](xref:fundamentals/configuration/index) pour configurer l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-318">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="0b0ed-319">Dans l’exemple suivant, la configuration de l’hôte est spécifiée de façon facultative dans un fichier *hostsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-319">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="0b0ed-320">Toute configuration chargée à partir du fichier *hostsettings.json* est remplaçable par des arguments de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-320">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="0b0ed-321">La configuration définie (dans `config`) est utilisée pour configurer l’hôte avec [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-321">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="0b0ed-322">La configuration `IWebHostBuilder` est ajoutée à la configuration de l’application, mais l’inverse n’est pas vrai &mdash; `ConfigureAppConfiguration` n’a pas d’incidence sur la configuration `IWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-322">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0b0ed-323">La configuration fournie par `UseUrls` est d’abord remplacée par la configuration *hostsettings.json*, puis par la configuration des arguments de ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-323">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
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

<span data-ttu-id="0b0ed-324">*hostsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-324">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0b0ed-325">La configuration fournie par `UseUrls` est d’abord remplacée par la configuration *hostsettings.json*, puis par la configuration des arguments de ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-325">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
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

<span data-ttu-id="0b0ed-326">*hostsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-326">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="0b0ed-327">La méthode d’extension [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) ne peut pas analyser une section de configuration retournée par `GetSection` (par exemple, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-327">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="0b0ed-328">La méthode `GetSection` filtre les clés de configuration dans la section demandée, mais laisse le nom de la section dans les clés (par exemple, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-328">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="0b0ed-329">La méthode `UseConfiguration` suppose que les clés correspondent aux clés `WebHostBuilder` (par exemple, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-329">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="0b0ed-330">La présence du nom de la section dans les clés empêche l’utilisation des valeurs de la section pour configurer l’hôte.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-330">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="0b0ed-331">Ce problème sera résolu dans une prochaine mise en production.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-331">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="0b0ed-332">Pour obtenir plus d’informations et connaître les solutions de contournement possibles, consultez [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-332">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="0b0ed-333">`UseConfiguration` copie seulement les clés de la configuration `IConfiguration` fournie vers la configuration du générateur d’hôte.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-333">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="0b0ed-334">Par conséquent, le fait de définir `reloadOnChange: true` pour les fichiers de paramètres XML, JSON et INI n’a aucun effet.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-334">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="0b0ed-335">Pour spécifier l’exécution de l’hôte sur une URL particulière, vous pouvez passer la valeur souhaitée à partir d’une invite de commandes lors de l’exécution de [dotnet run](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-335">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="0b0ed-336">L’argument de ligne de commande remplace la valeur `urls` du fichier *hostsettings.json*, et le serveur écoute le port 8080 :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-336">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="0b0ed-337">Gérer l’hôte</span><span class="sxs-lookup"><span data-stu-id="0b0ed-337">Manage the host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0b0ed-338">**Exécuter**</span><span class="sxs-lookup"><span data-stu-id="0b0ed-338">**Run**</span></span>

<span data-ttu-id="0b0ed-339">La méthode `Run` démarre l’application web et bloque le thread appelant jusqu’à l’arrêt de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-339">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="0b0ed-340">**Start**</span><span class="sxs-lookup"><span data-stu-id="0b0ed-340">**Start**</span></span>

<span data-ttu-id="0b0ed-341">Appelez la méthode `Start` pour exécuter l’hôte en mode non bloquant :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-341">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="0b0ed-342">Si une liste d’URL est passée à la méthode `Start`, l’hôte écoute les URL spécifiées :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-342">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="0b0ed-343">L’application peut initialiser et démarrer un nouvel hôte ayant les valeurs par défaut préconfigurées de `CreateDefaultBuilder` en utilisant une méthode d’usage statique.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-343">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="0b0ed-344">Ces méthodes démarrent le serveur sans sortie de console et, avec [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown), elles attendent un arrêt (Ctrl-C/SIGINT ou SIGTERM) :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-344">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="0b0ed-345">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="0b0ed-345">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="0b0ed-346">Commencez par un `RequestDelegate` :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-346">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="0b0ed-347">Envoyez une requête à `http://localhost:5000` dans le navigateur pour recevoir la réponse « Hello World! »</span><span class="sxs-lookup"><span data-stu-id="0b0ed-347">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="0b0ed-348">`WaitForShutdown` bloque la requête jusqu’à l’émission d’une commande d’arrêt (Ctrl-C/SIGINT ou SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-348">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="0b0ed-349">L’application affiche le message `Console.WriteLine` et attend que l’utilisateur appuie sur une touche pour s’arrêter.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-349">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="0b0ed-350">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="0b0ed-350">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="0b0ed-351">Commencez par une URL et `RequestDelegate` :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-351">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="0b0ed-352">Produit le même résultat que **Start(RequestDelegate app)**, sauf que l’application répond sur `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-352">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="0b0ed-353">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="0b0ed-353">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="0b0ed-354">Utilisez une instance de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) pour utiliser le middleware de routage :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-354">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="0b0ed-355">Utilisez les requêtes de navigateur suivantes avec l’exemple :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-355">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="0b0ed-356">Demande</span><span class="sxs-lookup"><span data-stu-id="0b0ed-356">Request</span></span>                                    | <span data-ttu-id="0b0ed-357">Réponse</span><span class="sxs-lookup"><span data-stu-id="0b0ed-357">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="0b0ed-358">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="0b0ed-358">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="0b0ed-359">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="0b0ed-359">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="0b0ed-360">Lève une exception avec la chaîne « ooops! »</span><span class="sxs-lookup"><span data-stu-id="0b0ed-360">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="0b0ed-361">Lève une exception avec la chaîne « Uh oh! »</span><span class="sxs-lookup"><span data-stu-id="0b0ed-361">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="0b0ed-362">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="0b0ed-362">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="0b0ed-363">Hello World!</span><span class="sxs-lookup"><span data-stu-id="0b0ed-363">Hello World!</span></span>                             |

<span data-ttu-id="0b0ed-364">`WaitForShutdown` bloque la requête jusqu’à l’émission d’une commande d’arrêt (Ctrl-C/SIGINT ou SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-364">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="0b0ed-365">L’application affiche le message `Console.WriteLine` et attend que l’utilisateur appuie sur une touche pour s’arrêter.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-365">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="0b0ed-366">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="0b0ed-366">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="0b0ed-367">Utilisez une URL et une instance de `IRouteBuilder` :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-367">Use a URL and an instance of `IRouteBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="0b0ed-368">Produit le même résultat que **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, sauf que l’application répond sur `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-368">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="0b0ed-369">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="0b0ed-369">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="0b0ed-370">Fournissez un délégué pour configurer un `IApplicationBuilder` :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-370">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="0b0ed-371">Envoyez une requête à `http://localhost:5000` dans le navigateur pour recevoir la réponse « Hello World! »</span><span class="sxs-lookup"><span data-stu-id="0b0ed-371">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="0b0ed-372">`WaitForShutdown` bloque la requête jusqu’à l’émission d’une commande d’arrêt (Ctrl-C/SIGINT ou SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-372">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="0b0ed-373">L’application affiche le message `Console.WriteLine` et attend que l’utilisateur appuie sur une touche pour s’arrêter.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-373">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="0b0ed-374">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="0b0ed-374">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="0b0ed-375">Fournissez une URL et un délégué pour configurer un `IApplicationBuilder` :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-375">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="0b0ed-376">Produit le même résultat que **StartWith(Action&lt;IApplicationBuilder&gt; app)**, sauf que l’application répond sur `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-376">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0b0ed-377">**Exécuter**</span><span class="sxs-lookup"><span data-stu-id="0b0ed-377">**Run**</span></span>

<span data-ttu-id="0b0ed-378">La méthode `Run` démarre l’application web et bloque le thread appelant jusqu’à l’arrêt de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-378">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="0b0ed-379">**Start**</span><span class="sxs-lookup"><span data-stu-id="0b0ed-379">**Start**</span></span>

<span data-ttu-id="0b0ed-380">Appelez la méthode `Start` pour exécuter l’hôte en mode non bloquant :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-380">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="0b0ed-381">Si une liste d’URL est passée à la méthode `Start`, l’hôte écoute les URL spécifiées :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-381">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

::: moniker-end

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="0b0ed-382">Interface IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="0b0ed-382">IHostingEnvironment interface</span></span>

<span data-ttu-id="0b0ed-383">[L’interface IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) fournit des informations sur l’environnement d’hébergement web de l’application.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-383">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="0b0ed-384">Utilisez [l’injection de constructeur](xref:fundamentals/dependency-injection) pour obtenir l’interface `IHostingEnvironment` afin d’utiliser ses propriétés et méthodes d’extension :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-384">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="0b0ed-385">Vous pouvez utiliser une [approche basée sur une convention](xref:fundamentals/environments#environment-based-startup-class-and-methods) pour configurer l’application au démarrage en fonction de l’environnement.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-385">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="0b0ed-386">Vous pouvez également injecter `IHostingEnvironment` dans le constructeur `Startup` pour l’utiliser dans `ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-386">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="0b0ed-387">En plus de la méthode d’extension `IsDevelopment`, `IHostingEnvironment` fournit les méthodes `IsStaging`, `IsProduction` et `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-387">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="0b0ed-388">Pour plus d'informations, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-388">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="0b0ed-389">Le service `IHostingEnvironment` peut également être injecté directement dans la méthode `Configure` pour configurer le pipeline de traitement :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-389">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="0b0ed-390">`IHostingEnvironment` peut être injecté dans la méthode `Invoke` lors de la création d’un [intergiciel (middleware)](xref:fundamentals/middleware/index#write-middleware) personnalisé :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-390">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#write-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="0b0ed-391">Interface IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="0b0ed-391">IApplicationLifetime interface</span></span>

<span data-ttu-id="0b0ed-392">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) s’utilise pour les opérations post-démarrage et arrêt.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-392">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="0b0ed-393">Trois propriétés de l’interface sont des jetons d’annulation utilisés pour inscrire les méthodes `Action` qui définissent les événements de démarrage et d’arrêt.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-393">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="0b0ed-394">Jeton d’annulation</span><span class="sxs-lookup"><span data-stu-id="0b0ed-394">Cancellation Token</span></span>    | <span data-ttu-id="0b0ed-395">Événement déclencheur</span><span class="sxs-lookup"><span data-stu-id="0b0ed-395">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="0b0ed-396">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="0b0ed-396">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="0b0ed-397">L’hôte a complètement démarré.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-397">The host has fully started.</span></span> |
| [<span data-ttu-id="0b0ed-398">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="0b0ed-398">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="0b0ed-399">L’hôte effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-399">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="0b0ed-400">Toutes les requêtes doivent être traitées.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-400">All requests should be processed.</span></span> <span data-ttu-id="0b0ed-401">L’arrêt est bloqué tant que cet événement n’est pas terminé.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-401">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="0b0ed-402">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="0b0ed-402">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="0b0ed-403">L’hôte effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-403">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="0b0ed-404">Des requêtes peuvent encore être en cours de traitement.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-404">Requests may still be processing.</span></span> <span data-ttu-id="0b0ed-405">L’arrêt est bloqué tant que cet événement n’est pas terminé.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-405">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="0b0ed-406">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requête l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-406">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="0b0ed-407">La classe suivante utilise `StopApplication` pour arrêter normalement une application quand la méthode `Shutdown` de la classe est appelée :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-407">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="0b0ed-408">Validation de l’étendue</span><span class="sxs-lookup"><span data-stu-id="0b0ed-408">Scope validation</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0b0ed-409">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) affecte la valeur `true` à [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) si l’environnement de l’application est Développement.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-409">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

::: moniker-end

<span data-ttu-id="0b0ed-410">Quand `ValidateScopes` est défini sur `true`, le fournisseur de services par défaut vérifie que :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-410">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="0b0ed-411">Les services délimités ne sont pas résolus directement ou indirectement à partir du fournisseur de services racine.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-411">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="0b0ed-412">Les services Scoped ne sont pas directement ou indirectement injectés dans des singletons.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-412">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="0b0ed-413">Le fournisseur de services racine est créé quand [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) est appelé.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-413">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="0b0ed-414">La durée de vie du fournisseur de services racine correspond à la durée de vie de l’application/du serveur quand le fournisseur démarre avec l’application et qu’il est supprimé quand l’application s’arrête.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-414">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="0b0ed-415">Les services Scoped sont supprimés par le conteneur qui les a créés.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-415">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="0b0ed-416">Si un service Scoped est créé dans le conteneur racine, la durée de vie du service est promue en singleton, car elle est supprimée par le conteneur racine seulement quand l’application/le serveur est arrêté.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-416">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="0b0ed-417">La validation des étendues du service permet de traiter ces situations quand `BuildServiceProvider` est appelé.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-417">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="0b0ed-418">Pour toujours valider les étendues, notamment dans l’environnement de production, configurez [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) avec [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) sur le créateur d’hôte :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-418">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

::: moniker range="= aspnetcore-2.0"

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="0b0ed-419">Dépannage de System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="0b0ed-419">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="0b0ed-420">**Ce qui suit s’applique uniquement aux applications ASP.NET Core 2.0 quand l’application n’appelle pas `UseStartup` ou `Configure`.**</span><span class="sxs-lookup"><span data-stu-id="0b0ed-420">**The following only applies to ASP.NET Core 2.0 apps when the app doesn't call `UseStartup` or `Configure`.**</span></span>

<span data-ttu-id="0b0ed-421">Vous pouvez créer un hôte en injectant `IStartup` directement dans le conteneur d’injection de dépendances au lieu d’appeler `UseStartup` ou `Configure` :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-421">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="0b0ed-422">Dans ce cas, l’erreur suivante peut se produire :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-422">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="0b0ed-423">Cette erreur se produit car le nom de l’application (le nom de l’assembly actuel) est nécessaire pour analyser `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="0b0ed-423">This occurs because the app name (the name of the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="0b0ed-424">Si l’application injecte `IStartup` manuellement dans le conteneur d’injection de dépendances, ajoutez l’appel suivant à `WebHostBuilder` en spécifiant le nom de l’assembly :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-424">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "AssemblyName")
```

<span data-ttu-id="0b0ed-425">Vous pouvez également ajouter un `Configure` factice à `WebHostBuilder`, qui définit automatiquement le nom de l’application :</span><span class="sxs-lookup"><span data-stu-id="0b0ed-425">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the app name automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
```

<span data-ttu-id="0b0ed-426">Pour plus d’informations, consultez le commentaire [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) et [l’exemple StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="0b0ed-426">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="0b0ed-427">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="0b0ed-427">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
