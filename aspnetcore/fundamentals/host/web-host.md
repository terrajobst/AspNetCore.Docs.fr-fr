---
title: Hôte web ASP.NET Core
author: guardrex
description: Découvrez l’hôte web dans ASP.NET Core, qui est responsable de la gestion du démarrage et de la durée de vie des applications.
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: 94d7e3436b52646166733e9e7334ac533c72b7b8
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346466"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="77758-103">Hôte web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="77758-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="77758-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="77758-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="77758-105">Les applications ASP.NET Core configurent et lancent un *hôte*.</span><span class="sxs-lookup"><span data-stu-id="77758-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="77758-106">L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="77758-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="77758-107">Au minimum, l’hôte configure un serveur ainsi qu’un pipeline de traitement des requêtes.</span><span class="sxs-lookup"><span data-stu-id="77758-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="77758-108">L’hôte peut aussi configurer la journalisation, l’injection de dépendances et la configuration.</span><span class="sxs-lookup"><span data-stu-id="77758-108">The host can also set up logging, dependency injection, and configuration.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="77758-109">Pour obtenir la version 1.1 de cette rubrique, téléchargez [ASP.NET Core Web Host (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="77758-109">For the 1.1 version of this topic, download [ASP.NET Core Web Host (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="77758-110">Cet article traite de l’hôte web ASP.NET Core (<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>), qui est responsable de l’hébergement des applications web.</span><span class="sxs-lookup"><span data-stu-id="77758-110">This article covers the ASP.NET Core Web Host (<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>), which is for hosting web apps.</span></span> <span data-ttu-id="77758-111">Pour obtenir des informations sur l’hôte générique .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), consultez <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="77758-111">For information about the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="77758-112">Cet article traite de l’hôte web ASP.NET Core ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)).</span><span class="sxs-lookup"><span data-stu-id="77758-112">This article covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)).</span></span> <span data-ttu-id="77758-113">Dans ASP.NET Core 3.0, l’hôte générique remplace l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="77758-113">In ASP.NET Core 3.0, Generic Host replaces Web Host.</span></span> <span data-ttu-id="77758-114">Pour plus d’informations, consultez [L’hôte](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="77758-114">For more information, see [The host](xref:fundamentals/index#host).</span></span>

::: moniker-end

## <a name="set-up-a-host"></a><span data-ttu-id="77758-115">Configurer un hôte</span><span class="sxs-lookup"><span data-stu-id="77758-115">Set up a host</span></span>

<span data-ttu-id="77758-116">Créez un hôte en utilisant une instance de [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="77758-116">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="77758-117">Cette opération est généralement effectuée au point d’entrée de l’application, à savoir la méthode `Main`.</span><span class="sxs-lookup"><span data-stu-id="77758-117">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="77758-118">Dans les modèles de projet, `Main` se trouve dans *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="77758-118">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="77758-119">Un fichier *Program.cs* standard appelle [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) pour commencer la configuration d’un hôte :</span><span class="sxs-lookup"><span data-stu-id="77758-119">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="77758-120">`CreateDefaultBuilder` effectue les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="77758-120">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="77758-121">Configure le serveur [Kestrel](xref:fundamentals/servers/kestrel) comme serveur web en utilisant les fournisseurs de configuration d’hébergement de l’application.</span><span class="sxs-lookup"><span data-stu-id="77758-121">Configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server using the app's hosting configuration providers.</span></span> <span data-ttu-id="77758-122">Pour connaître les options par défaut du serveur Kestrel, consultez <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="77758-122">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="77758-123">Définit le chemin retourné par [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) comme racine de contenu.</span><span class="sxs-lookup"><span data-stu-id="77758-123">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="77758-124">Charge la [configuration de l’hôte](#host-configuration-values) à partir de :</span><span class="sxs-lookup"><span data-stu-id="77758-124">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="77758-125">Variables d’environnement comportant le préfixe `ASPNETCORE_` (par exemple, `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="77758-125">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="77758-126">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="77758-126">Command-line arguments.</span></span>
* <span data-ttu-id="77758-127">Charge la configuration de l’application dans l’ordre suivant à partir des éléments ci-après :</span><span class="sxs-lookup"><span data-stu-id="77758-127">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="77758-128">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="77758-128">*appsettings.json*.</span></span>
  * <span data-ttu-id="77758-129">*appsettings.{Environment}.json*</span><span class="sxs-lookup"><span data-stu-id="77758-129">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="77758-130">L’outil [Secret Manager (Gestionnaire de secrets)](xref:security/app-secrets) quand l’application s’exécute dans l’environnement `Development` à l’aide de l’assembly d’entrée.</span><span class="sxs-lookup"><span data-stu-id="77758-130">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="77758-131">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="77758-131">Environment variables.</span></span>
  * <span data-ttu-id="77758-132">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="77758-132">Command-line arguments.</span></span>
* <span data-ttu-id="77758-133">Configure la [journalisation](xref:fundamentals/logging/index) des sorties de la console et du débogage.</span><span class="sxs-lookup"><span data-stu-id="77758-133">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="77758-134">La journalisation inclut les règles de [filtrage de journal](xref:fundamentals/logging/index#log-filtering) qui sont spécifiées dans une section de configuration de la journalisation dans un fichier *appsettings.json* ou *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="77758-134">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="77758-135">Lors de l’exécution derrière IIS avec le [module ASP.NET Core](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` permet l’[intégration IIS](xref:host-and-deploy/iis/index), qui configure l’adresse de base et le port de l’application.</span><span class="sxs-lookup"><span data-stu-id="77758-135">When running behind IIS with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` enables [IIS Integration](xref:host-and-deploy/iis/index), which configures the app's base address and port.</span></span> <span data-ttu-id="77758-136">L’intégration IIS configure également l’application pour la [capture des erreurs de démarrage](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="77758-136">IIS Integration also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="77758-137">Pour connaître les options par défaut d’IIS, consultez <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="77758-137">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="77758-138">Définissez [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) sur `true` si l’environnement de l’application est Développement.</span><span class="sxs-lookup"><span data-stu-id="77758-138">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="77758-139">Pour plus d’informations, consultez [Validation de l’étendue](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="77758-139">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="77758-140">La configuration définie par `CreateDefaultBuilder` peut être remplacée et enrichie par [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging) et les autres méthodes et les méthodes d’extension de [ IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="77758-140">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="77758-141">En voici quelques exemples :</span><span class="sxs-lookup"><span data-stu-id="77758-141">A few examples follow:</span></span>

* <span data-ttu-id="77758-142">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) permet de spécifier une configuration `IConfiguration` supplémentaire pour l’application.</span><span class="sxs-lookup"><span data-stu-id="77758-142">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="77758-143">L’appel `ConfigureAppConfiguration` suivant ajoute un délégué pour inclure la configuration de l’application dans le fichier *appsettings.xml*.</span><span class="sxs-lookup"><span data-stu-id="77758-143">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="77758-144">`ConfigureAppConfiguration` peut être appelé plusieurs fois.</span><span class="sxs-lookup"><span data-stu-id="77758-144">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="77758-145">Notez que cette configuration ne s’applique pas à l’hôte (par exemple, les URL de serveur ou l’environnement).</span><span class="sxs-lookup"><span data-stu-id="77758-145">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="77758-146">Voir la section [Valeurs de configuration de l’hôte](#host-configuration-values).</span><span class="sxs-lookup"><span data-stu-id="77758-146">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="77758-147">L’appel `ConfigureLogging` suivant ajoute un délégué pour configurer le niveau de journalisation minimal ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) sur [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="77758-147">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="77758-148">Ce paramètre remplace les paramètres de *appsettings.Development.json* (`LogLevel.Debug`) et *appsettings.Production.json* (`LogLevel.Error`) configurés par `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="77758-148">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="77758-149">`ConfigureLogging` peut être appelé plusieurs fois.</span><span class="sxs-lookup"><span data-stu-id="77758-149">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="77758-150">L’appel suivant à `ConfigureKestrel` remplace la valeur par défaut de [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) (30 000 000 octets) établie lors de la configuration de Kestrel par `CreateDefaultBuilder` :</span><span class="sxs-lookup"><span data-stu-id="77758-150">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="77758-151">L’appel suivant à [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) remplace la valeur par défaut de [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) (30 000 000 octets) établie lors de la configuration de Kestrel par `CreateDefaultBuilder` :</span><span class="sxs-lookup"><span data-stu-id="77758-151">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

<span data-ttu-id="77758-152">La *racine de contenu* détermine l’emplacement où l’hôte recherche les fichiers de contenu, tels que les fichiers de vue MVC.</span><span class="sxs-lookup"><span data-stu-id="77758-152">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="77758-153">Quand l’application est démarrée à partir du dossier racine du projet, ce dossier est utilisé comme racine de contenu.</span><span class="sxs-lookup"><span data-stu-id="77758-153">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="77758-154">Il s’agit du dossier par défaut utilisé dans [Visual Studio](https://www.visualstudio.com/) et les [modèles dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="77758-154">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="77758-155">Pour plus d’informations sur la configuration d’application, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="77758-155">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="77758-156">Au lieu d’utiliser la méthode statique `CreateDefaultBuilder`, vous pouvez aussi créer un hôte à partir de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Cette approche est prise en charge dans ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="77758-156">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="77758-157">Pour plus d’informations, consultez l’onglet ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="77758-157">For more information, see the ASP.NET Core 1.x tab.</span></span>

<span data-ttu-id="77758-158">Lors de la configuration d’un hôte, les méthodes [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) et [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) peuvent être fournies.</span><span class="sxs-lookup"><span data-stu-id="77758-158">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="77758-159">Si une classe `Startup` est spécifiée, elle doit définir une méthode `Configure`.</span><span class="sxs-lookup"><span data-stu-id="77758-159">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="77758-160">Pour plus d'informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="77758-160">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="77758-161">Les appels multiples à `ConfigureServices` s’ajoutent les uns aux autres.</span><span class="sxs-lookup"><span data-stu-id="77758-161">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="77758-162">Les appels multiples à `Configure` ou `UseStartup` sur `WebHostBuilder` remplacent les paramètres précédents.</span><span class="sxs-lookup"><span data-stu-id="77758-162">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="77758-163">Valeurs de configuration de l’hôte</span><span class="sxs-lookup"><span data-stu-id="77758-163">Host configuration values</span></span>

<span data-ttu-id="77758-164">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) s’appuie sur les approches suivantes pour définir les valeurs de configuration de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="77758-164">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="77758-165">Configuration du générateur de l’hôte, qui inclut des variables d’environnement au format `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="77758-165">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="77758-166">Par exemple, `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="77758-166">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="77758-167">Des extensions comme [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) et [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (voir la section [Remplacer la configuration](#override-configuration)).</span><span class="sxs-lookup"><span data-stu-id="77758-167">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="77758-168">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) et la clé associée.</span><span class="sxs-lookup"><span data-stu-id="77758-168">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="77758-169">Quand une valeur est définie avec `UseSetting`, elle est définie au format chaîne indépendamment du type.</span><span class="sxs-lookup"><span data-stu-id="77758-169">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="77758-170">L’hôte utilise l’option qui définit une valeur en dernier.</span><span class="sxs-lookup"><span data-stu-id="77758-170">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="77758-171">Pour plus d’informations, consultez [Remplacer une configuration](#override-configuration) dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="77758-171">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="77758-172">Clé d’application (Nom)</span><span class="sxs-lookup"><span data-stu-id="77758-172">Application Key (Name)</span></span>

<span data-ttu-id="77758-173">La propriété [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) est définie automatiquement quand [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) ou [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) est appelé pendant la construction de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="77758-173">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="77758-174">La valeur affectée correspond au nom de l’assembly contenant le point d’entrée de l’application.</span><span class="sxs-lookup"><span data-stu-id="77758-174">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="77758-175">Pour définir la valeur explicitement, utilisez [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey) :</span><span class="sxs-lookup"><span data-stu-id="77758-175">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="77758-176">**Clé** : applicationName</span><span class="sxs-lookup"><span data-stu-id="77758-176">**Key**: applicationName</span></span>  
<span data-ttu-id="77758-177">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="77758-177">**Type**: *string*</span></span>  
<span data-ttu-id="77758-178">**Par défaut** : nom de l’assembly contenant le point d’entrée de l’application.</span><span class="sxs-lookup"><span data-stu-id="77758-178">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="77758-179">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="77758-179">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="77758-180">**Variable d’environnement** : `ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="77758-180">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a><span data-ttu-id="77758-181">Capture des erreurs de démarrage</span><span class="sxs-lookup"><span data-stu-id="77758-181">Capture Startup Errors</span></span>

<span data-ttu-id="77758-182">Ce paramètre contrôle la capture des erreurs de démarrage.</span><span class="sxs-lookup"><span data-stu-id="77758-182">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="77758-183">**Clé** : captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="77758-183">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="77758-184">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="77758-184">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="77758-185">**Par défaut** : `false`, ou `true` si l’application s’exécute avec Kestrel derrière IIS.</span><span class="sxs-lookup"><span data-stu-id="77758-185">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="77758-186">**Définition avec** : `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="77758-186">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="77758-187">**Variable d’environnement** : `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="77758-187">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="77758-188">Avec la valeur `false`, la survenue d’erreurs au démarrage entraîne la fermeture de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="77758-188">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="77758-189">Avec la valeur `true`, l’hôte capture les exceptions levées au démarrage et tente de démarrer le serveur.</span><span class="sxs-lookup"><span data-stu-id="77758-189">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a><span data-ttu-id="77758-190">Racine de contenu</span><span class="sxs-lookup"><span data-stu-id="77758-190">Content Root</span></span>

<span data-ttu-id="77758-191">Ce paramètre détermine l’emplacement où ASP.NET Core commence la recherche des fichiers de contenu, tels que les vues MVC.</span><span class="sxs-lookup"><span data-stu-id="77758-191">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="77758-192">**Clé** : contentRoot</span><span class="sxs-lookup"><span data-stu-id="77758-192">**Key**: contentRoot</span></span>  
<span data-ttu-id="77758-193">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="77758-193">**Type**: *string*</span></span>  
<span data-ttu-id="77758-194">**Par défaut** : dossier où réside l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="77758-194">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="77758-195">**Définition avec** : `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="77758-195">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="77758-196">**Variable d’environnement** : `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="77758-196">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="77758-197">La racine de contenu est également utilisée comme chemin de base pour le [paramètre de la racine Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="77758-197">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="77758-198">Si le chemin est introuvable, l’hôte ne peut pas démarrer.</span><span class="sxs-lookup"><span data-stu-id="77758-198">If the path doesn't exist, the host fails to start.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a><span data-ttu-id="77758-199">Erreurs détaillées</span><span class="sxs-lookup"><span data-stu-id="77758-199">Detailed Errors</span></span>

<span data-ttu-id="77758-200">Détermine si les erreurs détaillées doivent être capturées.</span><span class="sxs-lookup"><span data-stu-id="77758-200">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="77758-201">**Clé** : detailedErrors</span><span class="sxs-lookup"><span data-stu-id="77758-201">**Key**: detailedErrors</span></span>  
<span data-ttu-id="77758-202">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="77758-202">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="77758-203">**Valeur par défaut** : false</span><span class="sxs-lookup"><span data-stu-id="77758-203">**Default**: false</span></span>  
<span data-ttu-id="77758-204">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="77758-204">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="77758-205">**Variable d’environnement** : `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="77758-205">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="77758-206">Quand cette fonctionnalité est activée (ou que <a href="#environment">l’environnement</a> est défini sur `Development`), l’application capture les exceptions détaillées.</span><span class="sxs-lookup"><span data-stu-id="77758-206">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a><span data-ttu-id="77758-207">Environnement</span><span class="sxs-lookup"><span data-stu-id="77758-207">Environment</span></span>

<span data-ttu-id="77758-208">Définit l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="77758-208">Sets the app's environment.</span></span>

<span data-ttu-id="77758-209">**Clé** : environment</span><span class="sxs-lookup"><span data-stu-id="77758-209">**Key**: environment</span></span>  
<span data-ttu-id="77758-210">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="77758-210">**Type**: *string*</span></span>  
<span data-ttu-id="77758-211">**Par défaut** : Production</span><span class="sxs-lookup"><span data-stu-id="77758-211">**Default**: Production</span></span>  
<span data-ttu-id="77758-212">**Définition avec** : `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="77758-212">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="77758-213">**Variable d’environnement** : `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="77758-213">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="77758-214">L’environnement peut être défini à n’importe quelle valeur.</span><span class="sxs-lookup"><span data-stu-id="77758-214">The environment can be set to any value.</span></span> <span data-ttu-id="77758-215">Les valeurs définies par le framework sont `Development`, `Staging` et `Production`.</span><span class="sxs-lookup"><span data-stu-id="77758-215">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="77758-216">Les valeurs ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="77758-216">Values aren't case sensitive.</span></span> <span data-ttu-id="77758-217">Par défaut, *l’environnement* est fourni par la variable d’environnement `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="77758-217">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="77758-218">Si vous utilisez [Visual Studio](https://www.visualstudio.com/), les variables d’environnement peuvent être définies dans le fichier *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="77758-218">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="77758-219">Pour plus d'informations, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="77758-219">For more information, see <xref:fundamentals/environments>.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="77758-220">Assemblys d’hébergement au démarrage</span><span class="sxs-lookup"><span data-stu-id="77758-220">Hosting Startup Assemblies</span></span>

<span data-ttu-id="77758-221">Définit les assemblys d’hébergement au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="77758-221">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="77758-222">**Clé** : hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="77758-222">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="77758-223">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="77758-223">**Type**: *string*</span></span>  
<span data-ttu-id="77758-224">**Par défaut** : Chaîne vide</span><span class="sxs-lookup"><span data-stu-id="77758-224">**Default**: Empty string</span></span>  
<span data-ttu-id="77758-225">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="77758-225">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="77758-226">**Variable d’environnement** : `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="77758-226">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="77758-227">Chaîne délimitée par des points-virgules qui spécifie les assemblys d’hébergement à charger au démarrage.</span><span class="sxs-lookup"><span data-stu-id="77758-227">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="77758-228">La valeur de configuration par défaut est une chaîne vide, mais les assemblys d’hébergement au démarrage incluent toujours l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="77758-228">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="77758-229">Quand des assemblys d’hébergement au démarrage sont fournis, ils sont ajoutés à l’assembly de l’application et sont chargés lorsque l’application génère ses services communs au démarrage.</span><span class="sxs-lookup"><span data-stu-id="77758-229">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a><span data-ttu-id="77758-230">Port HTTPS</span><span class="sxs-lookup"><span data-stu-id="77758-230">HTTPS Port</span></span>

<span data-ttu-id="77758-231">Définissez le port de redirection HTTPS.</span><span class="sxs-lookup"><span data-stu-id="77758-231">Set the HTTPS redirect port.</span></span> <span data-ttu-id="77758-232">Utilisé dans [l’application de HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="77758-232">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="77758-233">**Clé** : https_port **Type** : *string*
**Par défaut** : Aucune valeur par défaut n’est définie.</span><span class="sxs-lookup"><span data-stu-id="77758-233">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="77758-234">**Définir avec** : `UseSetting`
**La variable d’environnement**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="77758-234">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="77758-235">Assemblys d’hébergement à exclure au démarrage</span><span class="sxs-lookup"><span data-stu-id="77758-235">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="77758-236">Chaîne délimitée par des points-virgules qui spécifie les assemblys d’hébergement à exclure au démarrage.</span><span class="sxs-lookup"><span data-stu-id="77758-236">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="77758-237">**Clé** : hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="77758-237">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="77758-238">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="77758-238">**Type**: *string*</span></span>  
<span data-ttu-id="77758-239">**Par défaut** : Chaîne vide</span><span class="sxs-lookup"><span data-stu-id="77758-239">**Default**: Empty string</span></span>  
<span data-ttu-id="77758-240">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="77758-240">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="77758-241">**Variable d’environnement** : `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="77758-241">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a><span data-ttu-id="77758-242">URL d’hébergement préférées</span><span class="sxs-lookup"><span data-stu-id="77758-242">Prefer Hosting URLs</span></span>

<span data-ttu-id="77758-243">Indique si l’hôte doit écouter les URL configurées avec `WebHostBuilder` au lieu d’écouter celles configurées avec l’implémentation `IServer`.</span><span class="sxs-lookup"><span data-stu-id="77758-243">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="77758-244">**Clé** : preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="77758-244">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="77758-245">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="77758-245">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="77758-246">**Valeur par défaut** : true</span><span class="sxs-lookup"><span data-stu-id="77758-246">**Default**: true</span></span>  
<span data-ttu-id="77758-247">**Définition avec** : `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="77758-247">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="77758-248">**Variable d’environnement** : `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="77758-248">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a><span data-ttu-id="77758-249">Bloquer les assemblys d’hébergement au démarrage</span><span class="sxs-lookup"><span data-stu-id="77758-249">Prevent Hosting Startup</span></span>

<span data-ttu-id="77758-250">Empêche le chargement automatique des assemblys d’hébergement au démarrage, y compris ceux configurés par l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="77758-250">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="77758-251">Pour plus d'informations, consultez <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="77758-251">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="77758-252">**Clé** : preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="77758-252">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="77758-253">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="77758-253">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="77758-254">**Valeur par défaut** : false</span><span class="sxs-lookup"><span data-stu-id="77758-254">**Default**: false</span></span>  
<span data-ttu-id="77758-255">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="77758-255">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="77758-256">**Variable d’environnement** : `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="77758-256">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a><span data-ttu-id="77758-257">URL du serveur</span><span class="sxs-lookup"><span data-stu-id="77758-257">Server URLs</span></span>

<span data-ttu-id="77758-258">Indique les adresses IP ou les adresses d’hôte avec les ports et protocoles sur lesquels le serveur doit écouter les requêtes.</span><span class="sxs-lookup"><span data-stu-id="77758-258">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="77758-259">**Clé** : urls</span><span class="sxs-lookup"><span data-stu-id="77758-259">**Key**: urls</span></span>  
<span data-ttu-id="77758-260">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="77758-260">**Type**: *string*</span></span>  
<span data-ttu-id="77758-261">**Par défaut** : http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="77758-261">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="77758-262">**Définition avec** : `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="77758-262">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="77758-263">**Variable d’environnement** : `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="77758-263">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="77758-264">Liste de préfixes d’URL séparés par des points-virgules (;) correspondant aux URL auxquelles le serveur doit répondre.</span><span class="sxs-lookup"><span data-stu-id="77758-264">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="77758-265">Par exemple, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="77758-265">For example, `http://localhost:123`.</span></span> <span data-ttu-id="77758-266">Utilisez « \* » pour indiquer que le serveur doit écouter les requêtes sur toutes les adresses IP ou tous les noms d’hôte qui utilisent le port et le protocole spécifiés (par exemple, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="77758-266">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="77758-267">Le protocole (`http://` ou `https://`) doit être inclus avec chaque URL.</span><span class="sxs-lookup"><span data-stu-id="77758-267">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="77758-268">Les formats pris en charge varient selon les serveurs.</span><span class="sxs-lookup"><span data-stu-id="77758-268">Supported formats vary among servers.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="77758-269">Kestrel a sa propre API de configuration de points de terminaison.</span><span class="sxs-lookup"><span data-stu-id="77758-269">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="77758-270">Pour plus d'informations, consultez <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="77758-270">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="77758-271">Délai d’arrêt</span><span class="sxs-lookup"><span data-stu-id="77758-271">Shutdown Timeout</span></span>

<span data-ttu-id="77758-272">Spécifie le délai d’attente avant l’arrêt de l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="77758-272">Specifies the amount of time to wait for Web Host to shut down.</span></span>

<span data-ttu-id="77758-273">**Clé** : shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="77758-273">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="77758-274">**Type** : *int*</span><span class="sxs-lookup"><span data-stu-id="77758-274">**Type**: *int*</span></span>  
<span data-ttu-id="77758-275">**Par défaut** : 5</span><span class="sxs-lookup"><span data-stu-id="77758-275">**Default**: 5</span></span>  
<span data-ttu-id="77758-276">**Définition avec** : `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="77758-276">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="77758-277">**Variable d’environnement** : `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="77758-277">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="77758-278">La clé accepte une valeur *int* avec `UseSetting` (par exemple, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), mais la méthode d’extension [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) prend une valeur [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="77758-278">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="77758-279">Pendant la période du délai d’attente, l’hébergement :</span><span class="sxs-lookup"><span data-stu-id="77758-279">During the timeout period, hosting:</span></span>

* <span data-ttu-id="77758-280">Déclenche [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="77758-280">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="77758-281">Tente d’arrêter les services hébergés, en journalisant les erreurs pour les services qui échouent à s’arrêter.</span><span class="sxs-lookup"><span data-stu-id="77758-281">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="77758-282">Si la période du délai d’attente expire avant l’arrêt de tous les services hébergés, les services actifs restants sont arrêtés quand l’application s’arrête.</span><span class="sxs-lookup"><span data-stu-id="77758-282">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="77758-283">Les services s’arrêtent même s’ils n’ont pas terminé les traitements.</span><span class="sxs-lookup"><span data-stu-id="77758-283">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="77758-284">Si des services nécessitent plus de temps pour s’arrêter, augmentez le délai d’attente.</span><span class="sxs-lookup"><span data-stu-id="77758-284">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a><span data-ttu-id="77758-285">Assembly de démarrage</span><span class="sxs-lookup"><span data-stu-id="77758-285">Startup Assembly</span></span>

<span data-ttu-id="77758-286">Détermine l’assembly à rechercher pour la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="77758-286">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="77758-287">**Clé** : startupAssembly</span><span class="sxs-lookup"><span data-stu-id="77758-287">**Key**: startupAssembly</span></span>  
<span data-ttu-id="77758-288">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="77758-288">**Type**: *string*</span></span>  
<span data-ttu-id="77758-289">**Par défaut** : l’assembly de l’application</span><span class="sxs-lookup"><span data-stu-id="77758-289">**Default**: The app's assembly</span></span>  
<span data-ttu-id="77758-290">**Définition avec** : `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="77758-290">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="77758-291">**Variable d’environnement** : `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="77758-291">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="77758-292">L’assembly peut être référencé par nom (`string`) ou type (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="77758-292">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="77758-293">Si plusieurs méthodes `UseStartup` sont appelées, la dernière prévaut.</span><span class="sxs-lookup"><span data-stu-id="77758-293">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a><span data-ttu-id="77758-294">Racine Web</span><span class="sxs-lookup"><span data-stu-id="77758-294">Web Root</span></span>

<span data-ttu-id="77758-295">Définit le chemin relatif des ressources statiques de l’application.</span><span class="sxs-lookup"><span data-stu-id="77758-295">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="77758-296">**Clé** : webroot</span><span class="sxs-lookup"><span data-stu-id="77758-296">**Key**: webroot</span></span>  
<span data-ttu-id="77758-297">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="77758-297">**Type**: *string*</span></span>  
<span data-ttu-id="77758-298">**Par défaut** : quand aucune valeur n’est spécifiée, la valeur par défaut est « (Racine de contenu)/wwwroot », si ce chemin existe.</span><span class="sxs-lookup"><span data-stu-id="77758-298">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="77758-299">Si ce chemin est introuvable, un fournisseur de fichiers no-op est utilisé.</span><span class="sxs-lookup"><span data-stu-id="77758-299">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="77758-300">**Définition avec** : `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="77758-300">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="77758-301">**Variable d’environnement** : `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="77758-301">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a><span data-ttu-id="77758-302">Remplacer la configuration</span><span class="sxs-lookup"><span data-stu-id="77758-302">Override configuration</span></span>

<span data-ttu-id="77758-303">Utilisez [Configuration](xref:fundamentals/configuration/index) pour configurer l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="77758-303">Use [Configuration](xref:fundamentals/configuration/index) to configure Web Host.</span></span> <span data-ttu-id="77758-304">Dans l’exemple suivant, la configuration de l’hôte est spécifiée de façon facultative dans un fichier *hostsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="77758-304">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="77758-305">Toute configuration chargée à partir du fichier *hostsettings.json* est remplaçable par des arguments de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="77758-305">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="77758-306">La configuration définie (dans `config`) est utilisée pour configurer l’hôte avec [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="77758-306">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="77758-307">La configuration `IWebHostBuilder` est ajoutée à la configuration de l’application, mais l’inverse n’est pas vrai &mdash; `ConfigureAppConfiguration` n’a pas d’incidence sur la configuration `IWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="77758-307">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

<span data-ttu-id="77758-308">La configuration fournie par `UseUrls` est d’abord remplacée par la configuration *hostsettings.json*, puis par la configuration des arguments de ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="77758-308">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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
            });
    }
}
```

<span data-ttu-id="77758-309">*hostsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="77758-309">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> <span data-ttu-id="77758-310">La méthode d’extension [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) ne peut pas analyser une section de configuration retournée par `GetSection` (par exemple, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="77758-310">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="77758-311">La méthode `GetSection` filtre les clés de configuration dans la section demandée, mais laisse le nom de la section dans les clés (par exemple, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="77758-311">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="77758-312">La méthode `UseConfiguration` suppose que les clés correspondent aux clés `WebHostBuilder` (par exemple, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="77758-312">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="77758-313">La présence du nom de la section dans les clés empêche l’utilisation des valeurs de la section pour configurer l’hôte.</span><span class="sxs-lookup"><span data-stu-id="77758-313">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="77758-314">Ce problème sera résolu dans une prochaine mise en production.</span><span class="sxs-lookup"><span data-stu-id="77758-314">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="77758-315">Pour obtenir plus d’informations et connaître les solutions de contournement possibles, consultez [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="77758-315">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="77758-316">`UseConfiguration` copie seulement les clés de la configuration `IConfiguration` fournie vers la configuration du générateur d’hôte.</span><span class="sxs-lookup"><span data-stu-id="77758-316">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="77758-317">Par conséquent, le fait de définir `reloadOnChange: true` pour les fichiers de paramètres XML, JSON et INI n’a aucun effet.</span><span class="sxs-lookup"><span data-stu-id="77758-317">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="77758-318">Pour spécifier l’exécution de l’hôte sur une URL particulière, vous pouvez passer la valeur souhaitée à partir d’une invite de commandes lors de l’exécution de [dotnet run](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="77758-318">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="77758-319">L’argument de ligne de commande remplace la valeur `urls` du fichier *hostsettings.json*, et le serveur écoute le port 8080 :</span><span class="sxs-lookup"><span data-stu-id="77758-319">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="77758-320">Gérer l’hôte</span><span class="sxs-lookup"><span data-stu-id="77758-320">Manage the host</span></span>

<span data-ttu-id="77758-321">**Exécuter**</span><span class="sxs-lookup"><span data-stu-id="77758-321">**Run**</span></span>

<span data-ttu-id="77758-322">La méthode `Run` démarre l’application web et bloque le thread appelant jusqu’à l’arrêt de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="77758-322">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="77758-323">**Démarrer**</span><span class="sxs-lookup"><span data-stu-id="77758-323">**Start**</span></span>

<span data-ttu-id="77758-324">Appelez la méthode `Start` pour exécuter l’hôte en mode non bloquant :</span><span class="sxs-lookup"><span data-stu-id="77758-324">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="77758-325">Si une liste d’URL est passée à la méthode `Start`, l’hôte écoute les URL spécifiées :</span><span class="sxs-lookup"><span data-stu-id="77758-325">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="77758-326">L’application peut initialiser et démarrer un nouvel hôte ayant les valeurs par défaut préconfigurées de `CreateDefaultBuilder` en utilisant une méthode d’usage statique.</span><span class="sxs-lookup"><span data-stu-id="77758-326">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="77758-327">Ces méthodes démarrent le serveur sans sortie de console et, avec [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown), elles attendent un arrêt (Ctrl-C/SIGINT ou SIGTERM) :</span><span class="sxs-lookup"><span data-stu-id="77758-327">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="77758-328">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="77758-328">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="77758-329">Commencez par un `RequestDelegate` :</span><span class="sxs-lookup"><span data-stu-id="77758-329">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="77758-330">Envoyez une requête à `http://localhost:5000` dans le navigateur pour recevoir la réponse « Hello World! »</span><span class="sxs-lookup"><span data-stu-id="77758-330">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="77758-331">`WaitForShutdown` bloque la requête jusqu’à l’émission d’une commande d’arrêt (Ctrl-C/SIGINT ou SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="77758-331">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="77758-332">L’application affiche le message `Console.WriteLine` et attend que l’utilisateur appuie sur une touche pour s’arrêter.</span><span class="sxs-lookup"><span data-stu-id="77758-332">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="77758-333">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="77758-333">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="77758-334">Commencez par une URL et `RequestDelegate` :</span><span class="sxs-lookup"><span data-stu-id="77758-334">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="77758-335">Produit le même résultat que **Start(RequestDelegate app)**, sauf que l’application répond sur `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="77758-335">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="77758-336">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="77758-336">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="77758-337">Utilisez une instance de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) pour utiliser le middleware de routage :</span><span class="sxs-lookup"><span data-stu-id="77758-337">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="77758-338">Utilisez les requêtes de navigateur suivantes avec l’exemple :</span><span class="sxs-lookup"><span data-stu-id="77758-338">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="77758-339">Requête</span><span class="sxs-lookup"><span data-stu-id="77758-339">Request</span></span>                                    | <span data-ttu-id="77758-340">Réponse</span><span class="sxs-lookup"><span data-stu-id="77758-340">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="77758-341">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="77758-341">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="77758-342">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="77758-342">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="77758-343">Lève une exception avec la chaîne « ooops! »</span><span class="sxs-lookup"><span data-stu-id="77758-343">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="77758-344">Lève une exception avec la chaîne « Uh oh! »</span><span class="sxs-lookup"><span data-stu-id="77758-344">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="77758-345">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="77758-345">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="77758-346">Hello World!</span><span class="sxs-lookup"><span data-stu-id="77758-346">Hello World!</span></span>                             |

<span data-ttu-id="77758-347">`WaitForShutdown` bloque la requête jusqu’à l’émission d’une commande d’arrêt (Ctrl-C/SIGINT ou SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="77758-347">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="77758-348">L’application affiche le message `Console.WriteLine` et attend que l’utilisateur appuie sur une touche pour s’arrêter.</span><span class="sxs-lookup"><span data-stu-id="77758-348">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="77758-349">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="77758-349">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="77758-350">Utilisez une URL et une instance de `IRouteBuilder` :</span><span class="sxs-lookup"><span data-stu-id="77758-350">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="77758-351">Produit le même résultat que **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, sauf que l’application répond sur `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="77758-351">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="77758-352">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="77758-352">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="77758-353">Fournissez un délégué pour configurer un `IApplicationBuilder` :</span><span class="sxs-lookup"><span data-stu-id="77758-353">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="77758-354">Envoyez une requête à `http://localhost:5000` dans le navigateur pour recevoir la réponse « Hello World! »</span><span class="sxs-lookup"><span data-stu-id="77758-354">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="77758-355">`WaitForShutdown` bloque la requête jusqu’à l’émission d’une commande d’arrêt (Ctrl-C/SIGINT ou SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="77758-355">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="77758-356">L’application affiche le message `Console.WriteLine` et attend que l’utilisateur appuie sur une touche pour s’arrêter.</span><span class="sxs-lookup"><span data-stu-id="77758-356">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="77758-357">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="77758-357">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="77758-358">Fournissez une URL et un délégué pour configurer un `IApplicationBuilder` :</span><span class="sxs-lookup"><span data-stu-id="77758-358">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="77758-359">Produit le même résultat que **StartWith(Action&lt;IApplicationBuilder&gt; app)**, sauf que l’application répond sur `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="77758-359">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="77758-360">Interface IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="77758-360">IHostingEnvironment interface</span></span>

<span data-ttu-id="77758-361">[L’interface IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) fournit des informations sur l’environnement d’hébergement web de l’application.</span><span class="sxs-lookup"><span data-stu-id="77758-361">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="77758-362">Utilisez [l’injection de constructeur](xref:fundamentals/dependency-injection) pour obtenir l’interface `IHostingEnvironment` afin d’utiliser ses propriétés et méthodes d’extension :</span><span class="sxs-lookup"><span data-stu-id="77758-362">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="77758-363">Vous pouvez utiliser une [approche basée sur une convention](xref:fundamentals/environments#environment-based-startup-class-and-methods) pour configurer l’application au démarrage en fonction de l’environnement.</span><span class="sxs-lookup"><span data-stu-id="77758-363">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="77758-364">Vous pouvez également injecter `IHostingEnvironment` dans le constructeur `Startup` pour l’utiliser dans `ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="77758-364">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="77758-365">En plus de la méthode d’extension `IsDevelopment`, `IHostingEnvironment` fournit les méthodes `IsStaging`, `IsProduction` et `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="77758-365">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="77758-366">Pour plus d'informations, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="77758-366">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="77758-367">Le service `IHostingEnvironment` peut également être injecté directement dans la méthode `Configure` pour configurer le pipeline de traitement :</span><span class="sxs-lookup"><span data-stu-id="77758-367">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the Developer Exception Page
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

<span data-ttu-id="77758-368">`IHostingEnvironment` peut être injecté dans la méthode `Invoke` lors de la création d’un [intergiciel (middleware)](xref:fundamentals/middleware/write) personnalisé :</span><span class="sxs-lookup"><span data-stu-id="77758-368">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/write):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="77758-369">Interface IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="77758-369">IApplicationLifetime interface</span></span>

<span data-ttu-id="77758-370">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) s’utilise pour les opérations post-démarrage et arrêt.</span><span class="sxs-lookup"><span data-stu-id="77758-370">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="77758-371">Trois propriétés de l’interface sont des jetons d’annulation utilisés pour inscrire les méthodes `Action` qui définissent les événements de démarrage et d’arrêt.</span><span class="sxs-lookup"><span data-stu-id="77758-371">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="77758-372">Jeton d’annulation</span><span class="sxs-lookup"><span data-stu-id="77758-372">Cancellation Token</span></span>    | <span data-ttu-id="77758-373">Événement déclencheur</span><span class="sxs-lookup"><span data-stu-id="77758-373">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="77758-374">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="77758-374">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="77758-375">L’hôte a complètement démarré.</span><span class="sxs-lookup"><span data-stu-id="77758-375">The host has fully started.</span></span> |
| [<span data-ttu-id="77758-376">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="77758-376">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="77758-377">L’hôte effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="77758-377">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="77758-378">Toutes les requêtes doivent être traitées.</span><span class="sxs-lookup"><span data-stu-id="77758-378">All requests should be processed.</span></span> <span data-ttu-id="77758-379">L’arrêt est bloqué tant que cet événement n’est pas terminé.</span><span class="sxs-lookup"><span data-stu-id="77758-379">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="77758-380">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="77758-380">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="77758-381">L’hôte effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="77758-381">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="77758-382">Des requêtes peuvent encore être en cours de traitement.</span><span class="sxs-lookup"><span data-stu-id="77758-382">Requests may still be processing.</span></span> <span data-ttu-id="77758-383">L’arrêt est bloqué tant que cet événement n’est pas terminé.</span><span class="sxs-lookup"><span data-stu-id="77758-383">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="77758-384">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requête l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="77758-384">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="77758-385">La classe suivante utilise `StopApplication` pour arrêter normalement une application quand la méthode `Shutdown` de la classe est appelée :</span><span class="sxs-lookup"><span data-stu-id="77758-385">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="77758-386">Validation de l’étendue</span><span class="sxs-lookup"><span data-stu-id="77758-386">Scope validation</span></span>

<span data-ttu-id="77758-387">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) affecte la valeur `true` à [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) si l’environnement de l’application est Développement.</span><span class="sxs-lookup"><span data-stu-id="77758-387">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="77758-388">Quand `ValidateScopes` est défini sur `true`, le fournisseur de services par défaut vérifie que :</span><span class="sxs-lookup"><span data-stu-id="77758-388">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="77758-389">Les services délimités ne sont pas résolus directement ou indirectement à partir du fournisseur de services racine.</span><span class="sxs-lookup"><span data-stu-id="77758-389">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="77758-390">Les services Scoped ne sont pas directement ou indirectement injectés dans des singletons.</span><span class="sxs-lookup"><span data-stu-id="77758-390">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="77758-391">Le fournisseur de services racine est créé quand [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) est appelé.</span><span class="sxs-lookup"><span data-stu-id="77758-391">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="77758-392">La durée de vie du fournisseur de services racine correspond à la durée de vie de l’application/du serveur quand le fournisseur démarre avec l’application et qu’il est supprimé quand l’application s’arrête.</span><span class="sxs-lookup"><span data-stu-id="77758-392">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="77758-393">Les services Scoped sont supprimés par le conteneur qui les a créés.</span><span class="sxs-lookup"><span data-stu-id="77758-393">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="77758-394">Si un service Scoped est créé dans le conteneur racine, la durée de vie du service est promue en singleton, car elle est supprimée par le conteneur racine seulement quand l’application/le serveur est arrêté.</span><span class="sxs-lookup"><span data-stu-id="77758-394">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="77758-395">La validation des étendues du service permet de traiter ces situations quand `BuildServiceProvider` est appelé.</span><span class="sxs-lookup"><span data-stu-id="77758-395">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="77758-396">Pour toujours valider les étendues, notamment dans l’environnement de production, configurez [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) avec [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) sur le créateur d’hôte :</span><span class="sxs-lookup"><span data-stu-id="77758-396">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a><span data-ttu-id="77758-397">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="77758-397">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
