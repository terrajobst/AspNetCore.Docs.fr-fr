---
title: Hôte web ASP.NET Core
author: guardrex
description: Découvrez l’hôte web dans ASP.NET Core, qui est responsable de la gestion du démarrage et de la durée de vie des applications.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: fundamentals/host/web-host
ms.openlocfilehash: 48f3b664d901bdfb27cdf9e798fa60c0587d1def
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610289"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="52342-103">Hôte web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="52342-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="52342-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="52342-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="52342-105">Les applications ASP.NET Core configurent et lancent un *hôte*.</span><span class="sxs-lookup"><span data-stu-id="52342-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="52342-106">L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="52342-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="52342-107">Au minimum, l’hôte configure un serveur ainsi qu’un pipeline de traitement des requêtes.</span><span class="sxs-lookup"><span data-stu-id="52342-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="52342-108">L’hôte peut aussi configurer la journalisation, l’injection de dépendances et la configuration.</span><span class="sxs-lookup"><span data-stu-id="52342-108">The host can also set up logging, dependency injection, and configuration.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="52342-109">Pour obtenir la version 1.1 de cette rubrique, téléchargez [ASP.NET Core Web Host (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="52342-109">For the 1.1 version of this topic, download [ASP.NET Core Web Host (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="52342-110">Cet article traite de l’hôte web ASP.NET Core (<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>), qui est responsable de l’hébergement des applications web.</span><span class="sxs-lookup"><span data-stu-id="52342-110">This article covers the ASP.NET Core Web Host (<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>), which is for hosting web apps.</span></span> <span data-ttu-id="52342-111">Pour obtenir des informations sur l’hôte générique .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), consultez <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="52342-111">For information about the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="52342-112">Cet article traite de l’hôte web ASP.NET Core ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)).</span><span class="sxs-lookup"><span data-stu-id="52342-112">This article covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)).</span></span> <span data-ttu-id="52342-113">Dans ASP.NET Core 3.0, l’hôte générique remplace l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="52342-113">In ASP.NET Core 3.0, Generic Host replaces Web Host.</span></span> <span data-ttu-id="52342-114">Pour plus d’informations, consultez [L’hôte](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="52342-114">For more information, see [The host](xref:fundamentals/index#host).</span></span>

::: moniker-end

## <a name="set-up-a-host"></a><span data-ttu-id="52342-115">Configurer un hôte</span><span class="sxs-lookup"><span data-stu-id="52342-115">Set up a host</span></span>

<span data-ttu-id="52342-116">Créez un hôte en utilisant une instance de [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="52342-116">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="52342-117">Cette opération est généralement effectuée au point d’entrée de l’application, à savoir la méthode `Main`.</span><span class="sxs-lookup"><span data-stu-id="52342-117">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="52342-118">Le nom de la méthode du générateur, `CreateWebHostBuilder`, est un nom spécial qui identifie la méthode du générateur auprès des composants externes, par exemple [Entity Framework](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="52342-118">The builder method name, `CreateWebHostBuilder`, is special name that identifies the builder method to external components, such as [Entity Framework](/ef/core/).</span></span>

<span data-ttu-id="52342-119">Dans les modèles de projet, `Main` se trouve dans *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="52342-119">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="52342-120">Une application standard appelle [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) pour lancer la configuration d’un hôte :</span><span class="sxs-lookup"><span data-stu-id="52342-120">A typical app calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="52342-121">`CreateDefaultBuilder` effectue les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="52342-121">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="52342-122">Configure le serveur [Kestrel](xref:fundamentals/servers/kestrel) comme serveur web en utilisant les fournisseurs de configuration d’hébergement de l’application.</span><span class="sxs-lookup"><span data-stu-id="52342-122">Configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server using the app's hosting configuration providers.</span></span> <span data-ttu-id="52342-123">Pour connaître les options par défaut du serveur Kestrel, consultez <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="52342-123">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="52342-124">Définit le chemin retourné par [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) comme racine de contenu.</span><span class="sxs-lookup"><span data-stu-id="52342-124">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="52342-125">Charge la [configuration de l’hôte](#host-configuration-values) à partir de :</span><span class="sxs-lookup"><span data-stu-id="52342-125">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="52342-126">Variables d’environnement comportant le préfixe `ASPNETCORE_` (par exemple, `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="52342-126">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="52342-127">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="52342-127">Command-line arguments.</span></span>
* <span data-ttu-id="52342-128">Charge la configuration de l’application dans l’ordre suivant à partir des éléments ci-après :</span><span class="sxs-lookup"><span data-stu-id="52342-128">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="52342-129">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="52342-129">*appsettings.json*.</span></span>
  * <span data-ttu-id="52342-130">*appsettings.{Environment}.json*</span><span class="sxs-lookup"><span data-stu-id="52342-130">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="52342-131">L’outil [Secret Manager (Gestionnaire de secrets)](xref:security/app-secrets) quand l’application s’exécute dans l’environnement `Development` à l’aide de l’assembly d’entrée.</span><span class="sxs-lookup"><span data-stu-id="52342-131">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="52342-132">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="52342-132">Environment variables.</span></span>
  * <span data-ttu-id="52342-133">Arguments de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="52342-133">Command-line arguments.</span></span>
* <span data-ttu-id="52342-134">Configure la [journalisation](xref:fundamentals/logging/index) des sorties de la console et du débogage.</span><span class="sxs-lookup"><span data-stu-id="52342-134">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="52342-135">La journalisation inclut les règles de [filtrage de journal](xref:fundamentals/logging/index#log-filtering) qui sont spécifiées dans une section de configuration de la journalisation dans un fichier *appsettings.json* ou *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="52342-135">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="52342-136">Lors de l’exécution derrière IIS avec le [module ASP.NET Core](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` permet l’[intégration IIS](xref:host-and-deploy/iis/index), qui configure l’adresse de base et le port de l’application.</span><span class="sxs-lookup"><span data-stu-id="52342-136">When running behind IIS with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` enables [IIS Integration](xref:host-and-deploy/iis/index), which configures the app's base address and port.</span></span> <span data-ttu-id="52342-137">L’intégration IIS configure également l’application pour la [capture des erreurs de démarrage](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="52342-137">IIS Integration also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="52342-138">Pour connaître les options par défaut d’IIS, consultez <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="52342-138">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="52342-139">Définissez [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) sur `true` si l’environnement de l’application est Développement.</span><span class="sxs-lookup"><span data-stu-id="52342-139">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="52342-140">Pour plus d’informations, consultez [Validation de l’étendue](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="52342-140">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="52342-141">La configuration définie par `CreateDefaultBuilder` peut être remplacée et enrichie par [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging) et les autres méthodes et les méthodes d’extension de [ IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="52342-141">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="52342-142">En voici quelques exemples :</span><span class="sxs-lookup"><span data-stu-id="52342-142">A few examples follow:</span></span>

* <span data-ttu-id="52342-143">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) permet de spécifier une configuration `IConfiguration` supplémentaire pour l’application.</span><span class="sxs-lookup"><span data-stu-id="52342-143">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="52342-144">L’appel `ConfigureAppConfiguration` suivant ajoute un délégué pour inclure la configuration de l’application dans le fichier *appsettings.xml*.</span><span class="sxs-lookup"><span data-stu-id="52342-144">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="52342-145">`ConfigureAppConfiguration` peut être appelé plusieurs fois.</span><span class="sxs-lookup"><span data-stu-id="52342-145">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="52342-146">Notez que cette configuration ne s’applique pas à l’hôte (par exemple, les URL de serveur ou l’environnement).</span><span class="sxs-lookup"><span data-stu-id="52342-146">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="52342-147">Voir la section [Valeurs de configuration de l’hôte](#host-configuration-values).</span><span class="sxs-lookup"><span data-stu-id="52342-147">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="52342-148">L’appel `ConfigureLogging` suivant ajoute un délégué pour configurer le niveau de journalisation minimal ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) sur [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="52342-148">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="52342-149">Ce paramètre remplace les paramètres de *appsettings.Development.json* (`LogLevel.Debug`) et *appsettings.Production.json* (`LogLevel.Error`) configurés par `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="52342-149">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="52342-150">`ConfigureLogging` peut être appelé plusieurs fois.</span><span class="sxs-lookup"><span data-stu-id="52342-150">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="52342-151">L’appel suivant à `ConfigureKestrel` remplace la valeur par défaut de [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) (30 000 000 octets) établie lors de la configuration de Kestrel par `CreateDefaultBuilder` :</span><span class="sxs-lookup"><span data-stu-id="52342-151">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="52342-152">L’appel suivant à [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) remplace la valeur par défaut de [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) (30 000 000 octets) établie lors de la configuration de Kestrel par `CreateDefaultBuilder` :</span><span class="sxs-lookup"><span data-stu-id="52342-152">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

<span data-ttu-id="52342-153">La *racine de contenu* détermine l’emplacement où l’hôte recherche les fichiers de contenu, tels que les fichiers de vue MVC.</span><span class="sxs-lookup"><span data-stu-id="52342-153">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="52342-154">Quand l’application est démarrée à partir du dossier racine du projet, ce dossier est utilisé comme racine de contenu.</span><span class="sxs-lookup"><span data-stu-id="52342-154">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="52342-155">Il s’agit du dossier par défaut utilisé dans [Visual Studio](https://visualstudio.microsoft.com) et les [modèles dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="52342-155">This is the default used in [Visual Studio](https://visualstudio.microsoft.com) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="52342-156">Pour plus d’informations sur la configuration d’application, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="52342-156">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="52342-157">Au lieu d’utiliser la méthode statique `CreateDefaultBuilder`, vous pouvez aussi créer un hôte à partir de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Cette approche est prise en charge dans ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="52342-157">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="52342-158">Pour plus d’informations, consultez l’onglet ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="52342-158">For more information, see the ASP.NET Core 1.x tab.</span></span>

<span data-ttu-id="52342-159">Lors de la configuration d’un hôte, les méthodes [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) et [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) peuvent être fournies.</span><span class="sxs-lookup"><span data-stu-id="52342-159">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="52342-160">Si une classe `Startup` est spécifiée, elle doit définir une méthode `Configure`.</span><span class="sxs-lookup"><span data-stu-id="52342-160">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="52342-161">Pour plus d'informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="52342-161">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="52342-162">Les appels multiples à `ConfigureServices` s’ajoutent les uns aux autres.</span><span class="sxs-lookup"><span data-stu-id="52342-162">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="52342-163">Les appels multiples à `Configure` ou `UseStartup` sur `WebHostBuilder` remplacent les paramètres précédents.</span><span class="sxs-lookup"><span data-stu-id="52342-163">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="52342-164">Valeurs de configuration de l’hôte</span><span class="sxs-lookup"><span data-stu-id="52342-164">Host configuration values</span></span>

<span data-ttu-id="52342-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) s’appuie sur les approches suivantes pour définir les valeurs de configuration de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="52342-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="52342-166">Configuration du générateur de l’hôte, qui inclut des variables d’environnement au format `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="52342-166">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="52342-167">Par exemple, `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="52342-167">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="52342-168">Des extensions comme [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) et [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (voir la section [Remplacer la configuration](#override-configuration)).</span><span class="sxs-lookup"><span data-stu-id="52342-168">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="52342-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) et la clé associée.</span><span class="sxs-lookup"><span data-stu-id="52342-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="52342-170">Quand une valeur est définie avec `UseSetting`, elle est définie au format chaîne indépendamment du type.</span><span class="sxs-lookup"><span data-stu-id="52342-170">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="52342-171">L’hôte utilise l’option qui définit une valeur en dernier.</span><span class="sxs-lookup"><span data-stu-id="52342-171">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="52342-172">Pour plus d’informations, consultez [Remplacer une configuration](#override-configuration) dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="52342-172">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="52342-173">Clé d’application (Nom)</span><span class="sxs-lookup"><span data-stu-id="52342-173">Application Key (Name)</span></span>

<span data-ttu-id="52342-174">La propriété [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) est définie automatiquement quand [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) ou [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) est appelé pendant la construction de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="52342-174">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="52342-175">La valeur affectée correspond au nom de l’assembly contenant le point d’entrée de l’application.</span><span class="sxs-lookup"><span data-stu-id="52342-175">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="52342-176">Pour définir la valeur explicitement, utilisez [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey) :</span><span class="sxs-lookup"><span data-stu-id="52342-176">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="52342-177">**Clé** : applicationName</span><span class="sxs-lookup"><span data-stu-id="52342-177">**Key**: applicationName</span></span>  
<span data-ttu-id="52342-178">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="52342-178">**Type**: *string*</span></span>  
<span data-ttu-id="52342-179">**Par défaut** : nom de l’assembly contenant le point d’entrée de l’application.</span><span class="sxs-lookup"><span data-stu-id="52342-179">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="52342-180">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="52342-180">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="52342-181">**Variable d’environnement** : `ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="52342-181">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a><span data-ttu-id="52342-182">Capture des erreurs de démarrage</span><span class="sxs-lookup"><span data-stu-id="52342-182">Capture Startup Errors</span></span>

<span data-ttu-id="52342-183">Ce paramètre contrôle la capture des erreurs de démarrage.</span><span class="sxs-lookup"><span data-stu-id="52342-183">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="52342-184">**Clé** : captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="52342-184">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="52342-185">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="52342-185">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="52342-186">**Par défaut** : `false`, ou `true` si l’application s’exécute avec Kestrel derrière IIS.</span><span class="sxs-lookup"><span data-stu-id="52342-186">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="52342-187">**Définition avec** : `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="52342-187">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="52342-188">**Variable d’environnement** : `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="52342-188">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="52342-189">Avec la valeur `false`, la survenue d’erreurs au démarrage entraîne la fermeture de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="52342-189">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="52342-190">Avec la valeur `true`, l’hôte capture les exceptions levées au démarrage et tente de démarrer le serveur.</span><span class="sxs-lookup"><span data-stu-id="52342-190">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a><span data-ttu-id="52342-191">Racine de contenu</span><span class="sxs-lookup"><span data-stu-id="52342-191">Content Root</span></span>

<span data-ttu-id="52342-192">Ce paramètre détermine l’emplacement où ASP.NET Core commence la recherche des fichiers de contenu, tels que les vues MVC.</span><span class="sxs-lookup"><span data-stu-id="52342-192">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="52342-193">**Clé** : contentRoot</span><span class="sxs-lookup"><span data-stu-id="52342-193">**Key**: contentRoot</span></span>  
<span data-ttu-id="52342-194">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="52342-194">**Type**: *string*</span></span>  
<span data-ttu-id="52342-195">**Par défaut** : dossier où réside l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="52342-195">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="52342-196">**Définition avec** : `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="52342-196">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="52342-197">**Variable d’environnement** : `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="52342-197">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="52342-198">La racine de contenu est également utilisée comme chemin de base pour le [paramètre de la racine Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="52342-198">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="52342-199">Si le chemin est introuvable, l’hôte ne peut pas démarrer.</span><span class="sxs-lookup"><span data-stu-id="52342-199">If the path doesn't exist, the host fails to start.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a><span data-ttu-id="52342-200">Erreurs détaillées</span><span class="sxs-lookup"><span data-stu-id="52342-200">Detailed Errors</span></span>

<span data-ttu-id="52342-201">Détermine si les erreurs détaillées doivent être capturées.</span><span class="sxs-lookup"><span data-stu-id="52342-201">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="52342-202">**Clé** : detailedErrors</span><span class="sxs-lookup"><span data-stu-id="52342-202">**Key**: detailedErrors</span></span>  
<span data-ttu-id="52342-203">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="52342-203">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="52342-204">**Valeur par défaut** : false</span><span class="sxs-lookup"><span data-stu-id="52342-204">**Default**: false</span></span>  
<span data-ttu-id="52342-205">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="52342-205">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="52342-206">**Variable d’environnement** : `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="52342-206">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="52342-207">Quand cette fonctionnalité est activée (ou que <a href="#environment">l’environnement</a> est défini sur `Development`), l’application capture les exceptions détaillées.</span><span class="sxs-lookup"><span data-stu-id="52342-207">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a><span data-ttu-id="52342-208">Environnement</span><span class="sxs-lookup"><span data-stu-id="52342-208">Environment</span></span>

<span data-ttu-id="52342-209">Définit l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="52342-209">Sets the app's environment.</span></span>

<span data-ttu-id="52342-210">**Clé** : environment</span><span class="sxs-lookup"><span data-stu-id="52342-210">**Key**: environment</span></span>  
<span data-ttu-id="52342-211">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="52342-211">**Type**: *string*</span></span>  
<span data-ttu-id="52342-212">**Par défaut** : Production</span><span class="sxs-lookup"><span data-stu-id="52342-212">**Default**: Production</span></span>  
<span data-ttu-id="52342-213">**Définition avec** : `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="52342-213">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="52342-214">**Variable d’environnement** : `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="52342-214">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="52342-215">L’environnement peut être défini à n’importe quelle valeur.</span><span class="sxs-lookup"><span data-stu-id="52342-215">The environment can be set to any value.</span></span> <span data-ttu-id="52342-216">Les valeurs définies par le framework sont `Development`, `Staging` et `Production`.</span><span class="sxs-lookup"><span data-stu-id="52342-216">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="52342-217">Les valeurs ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="52342-217">Values aren't case sensitive.</span></span> <span data-ttu-id="52342-218">Par défaut, *l’environnement* est fourni par la variable d’environnement `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="52342-218">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="52342-219">Si vous utilisez [Visual Studio](https://visualstudio.microsoft.com), les variables d’environnement peuvent être définies dans le fichier *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="52342-219">When using [Visual Studio](https://visualstudio.microsoft.com), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="52342-220">Pour plus d'informations, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="52342-220">For more information, see <xref:fundamentals/environments>.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="52342-221">Assemblys d’hébergement au démarrage</span><span class="sxs-lookup"><span data-stu-id="52342-221">Hosting Startup Assemblies</span></span>

<span data-ttu-id="52342-222">Définit les assemblys d’hébergement au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="52342-222">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="52342-223">**Clé** : hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="52342-223">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="52342-224">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="52342-224">**Type**: *string*</span></span>  
<span data-ttu-id="52342-225">**Par défaut** : Chaîne vide</span><span class="sxs-lookup"><span data-stu-id="52342-225">**Default**: Empty string</span></span>  
<span data-ttu-id="52342-226">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="52342-226">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="52342-227">**Variable d’environnement** : `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="52342-227">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="52342-228">Chaîne délimitée par des points-virgules qui spécifie les assemblys d’hébergement à charger au démarrage.</span><span class="sxs-lookup"><span data-stu-id="52342-228">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="52342-229">La valeur de configuration par défaut est une chaîne vide, mais les assemblys d’hébergement au démarrage incluent toujours l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="52342-229">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="52342-230">Quand des assemblys d’hébergement au démarrage sont fournis, ils sont ajoutés à l’assembly de l’application et sont chargés lorsque l’application génère ses services communs au démarrage.</span><span class="sxs-lookup"><span data-stu-id="52342-230">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a><span data-ttu-id="52342-231">Port HTTPS</span><span class="sxs-lookup"><span data-stu-id="52342-231">HTTPS Port</span></span>

<span data-ttu-id="52342-232">Définissez le port de redirection HTTPS.</span><span class="sxs-lookup"><span data-stu-id="52342-232">Set the HTTPS redirect port.</span></span> <span data-ttu-id="52342-233">Utilisé dans [l’application de HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="52342-233">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="52342-234">**Clé** : https_port **Type** : *string*
**Par défaut** : Aucune valeur par défaut n’est définie.</span><span class="sxs-lookup"><span data-stu-id="52342-234">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="52342-235">**Définir avec** : `UseSetting`
**La variable d’environnement**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="52342-235">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="52342-236">Assemblys d’hébergement à exclure au démarrage</span><span class="sxs-lookup"><span data-stu-id="52342-236">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="52342-237">Chaîne délimitée par des points-virgules qui spécifie les assemblys d’hébergement à exclure au démarrage.</span><span class="sxs-lookup"><span data-stu-id="52342-237">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="52342-238">**Clé** : hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="52342-238">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="52342-239">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="52342-239">**Type**: *string*</span></span>  
<span data-ttu-id="52342-240">**Par défaut** : Chaîne vide</span><span class="sxs-lookup"><span data-stu-id="52342-240">**Default**: Empty string</span></span>  
<span data-ttu-id="52342-241">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="52342-241">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="52342-242">**Variable d’environnement** : `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="52342-242">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a><span data-ttu-id="52342-243">URL d’hébergement préférées</span><span class="sxs-lookup"><span data-stu-id="52342-243">Prefer Hosting URLs</span></span>

<span data-ttu-id="52342-244">Indique si l’hôte doit écouter les URL configurées avec `WebHostBuilder` au lieu d’écouter celles configurées avec l’implémentation `IServer`.</span><span class="sxs-lookup"><span data-stu-id="52342-244">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="52342-245">**Clé** : preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="52342-245">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="52342-246">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="52342-246">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="52342-247">**Valeur par défaut** : true</span><span class="sxs-lookup"><span data-stu-id="52342-247">**Default**: true</span></span>  
<span data-ttu-id="52342-248">**Définition avec** : `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="52342-248">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="52342-249">**Variable d’environnement** : `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="52342-249">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a><span data-ttu-id="52342-250">Bloquer les assemblys d’hébergement au démarrage</span><span class="sxs-lookup"><span data-stu-id="52342-250">Prevent Hosting Startup</span></span>

<span data-ttu-id="52342-251">Empêche le chargement automatique des assemblys d’hébergement au démarrage, y compris ceux configurés par l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="52342-251">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="52342-252">Pour plus d'informations, consultez <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="52342-252">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="52342-253">**Clé** : preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="52342-253">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="52342-254">**Type** : *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="52342-254">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="52342-255">**Valeur par défaut** : false</span><span class="sxs-lookup"><span data-stu-id="52342-255">**Default**: false</span></span>  
<span data-ttu-id="52342-256">**Définition avec** : `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="52342-256">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="52342-257">**Variable d’environnement** : `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="52342-257">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a><span data-ttu-id="52342-258">URL du serveur</span><span class="sxs-lookup"><span data-stu-id="52342-258">Server URLs</span></span>

<span data-ttu-id="52342-259">Indique les adresses IP ou les adresses d’hôte avec les ports et protocoles sur lesquels le serveur doit écouter les requêtes.</span><span class="sxs-lookup"><span data-stu-id="52342-259">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="52342-260">**Clé** : urls</span><span class="sxs-lookup"><span data-stu-id="52342-260">**Key**: urls</span></span>  
<span data-ttu-id="52342-261">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="52342-261">**Type**: *string*</span></span>  
<span data-ttu-id="52342-262">**Par défaut** : http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="52342-262">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="52342-263">**Définition avec** : `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="52342-263">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="52342-264">**Variable d’environnement** : `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="52342-264">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="52342-265">Liste de préfixes d’URL séparés par des points-virgules (;) correspondant aux URL auxquelles le serveur doit répondre.</span><span class="sxs-lookup"><span data-stu-id="52342-265">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="52342-266">Par exemple, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="52342-266">For example, `http://localhost:123`.</span></span> <span data-ttu-id="52342-267">Utilisez « \* » pour indiquer que le serveur doit écouter les requêtes sur toutes les adresses IP ou tous les noms d’hôte qui utilisent le port et le protocole spécifiés (par exemple, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="52342-267">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="52342-268">Le protocole (`http://` ou `https://`) doit être inclus avec chaque URL.</span><span class="sxs-lookup"><span data-stu-id="52342-268">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="52342-269">Les formats pris en charge varient selon les serveurs.</span><span class="sxs-lookup"><span data-stu-id="52342-269">Supported formats vary among servers.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="52342-270">Kestrel a sa propre API de configuration de points de terminaison.</span><span class="sxs-lookup"><span data-stu-id="52342-270">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="52342-271">Pour plus d'informations, consultez <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="52342-271">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="52342-272">Délai d’arrêt</span><span class="sxs-lookup"><span data-stu-id="52342-272">Shutdown Timeout</span></span>

<span data-ttu-id="52342-273">Spécifie le délai d’attente avant l’arrêt de l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="52342-273">Specifies the amount of time to wait for Web Host to shut down.</span></span>

<span data-ttu-id="52342-274">**Clé** : shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="52342-274">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="52342-275">**Type** : *int*</span><span class="sxs-lookup"><span data-stu-id="52342-275">**Type**: *int*</span></span>  
<span data-ttu-id="52342-276">**Par défaut** : 5</span><span class="sxs-lookup"><span data-stu-id="52342-276">**Default**: 5</span></span>  
<span data-ttu-id="52342-277">**Définition avec** : `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="52342-277">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="52342-278">**Variable d’environnement** : `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="52342-278">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="52342-279">La clé accepte une valeur *int* avec `UseSetting` (par exemple, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), mais la méthode d’extension [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) prend une valeur [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="52342-279">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="52342-280">Pendant la période du délai d’attente, l’hébergement :</span><span class="sxs-lookup"><span data-stu-id="52342-280">During the timeout period, hosting:</span></span>

* <span data-ttu-id="52342-281">Déclenche [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="52342-281">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="52342-282">Tente d’arrêter les services hébergés, en journalisant les erreurs pour les services qui échouent à s’arrêter.</span><span class="sxs-lookup"><span data-stu-id="52342-282">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="52342-283">Si la période du délai d’attente expire avant l’arrêt de tous les services hébergés, les services actifs restants sont arrêtés quand l’application s’arrête.</span><span class="sxs-lookup"><span data-stu-id="52342-283">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="52342-284">Les services s’arrêtent même s’ils n’ont pas terminé les traitements.</span><span class="sxs-lookup"><span data-stu-id="52342-284">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="52342-285">Si des services nécessitent plus de temps pour s’arrêter, augmentez le délai d’attente.</span><span class="sxs-lookup"><span data-stu-id="52342-285">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a><span data-ttu-id="52342-286">Assembly de démarrage</span><span class="sxs-lookup"><span data-stu-id="52342-286">Startup Assembly</span></span>

<span data-ttu-id="52342-287">Détermine l’assembly à rechercher pour la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="52342-287">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="52342-288">**Clé** : startupAssembly</span><span class="sxs-lookup"><span data-stu-id="52342-288">**Key**: startupAssembly</span></span>  
<span data-ttu-id="52342-289">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="52342-289">**Type**: *string*</span></span>  
<span data-ttu-id="52342-290">**Par défaut** : l’assembly de l’application</span><span class="sxs-lookup"><span data-stu-id="52342-290">**Default**: The app's assembly</span></span>  
<span data-ttu-id="52342-291">**Définition avec** : `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="52342-291">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="52342-292">**Variable d’environnement** : `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="52342-292">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="52342-293">L’assembly peut être référencé par nom (`string`) ou type (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="52342-293">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="52342-294">Si plusieurs méthodes `UseStartup` sont appelées, la dernière prévaut.</span><span class="sxs-lookup"><span data-stu-id="52342-294">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a><span data-ttu-id="52342-295">Racine Web</span><span class="sxs-lookup"><span data-stu-id="52342-295">Web Root</span></span>

<span data-ttu-id="52342-296">Définit le chemin relatif des ressources statiques de l’application.</span><span class="sxs-lookup"><span data-stu-id="52342-296">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="52342-297">**Clé** : webroot</span><span class="sxs-lookup"><span data-stu-id="52342-297">**Key**: webroot</span></span>  
<span data-ttu-id="52342-298">**Type** : *string*</span><span class="sxs-lookup"><span data-stu-id="52342-298">**Type**: *string*</span></span>  
<span data-ttu-id="52342-299">**Par défaut** : quand aucune valeur n’est spécifiée, la valeur par défaut est « (Racine de contenu)/wwwroot », si ce chemin existe.</span><span class="sxs-lookup"><span data-stu-id="52342-299">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="52342-300">Si ce chemin est introuvable, un fournisseur de fichiers no-op est utilisé.</span><span class="sxs-lookup"><span data-stu-id="52342-300">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="52342-301">**Définition avec** : `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="52342-301">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="52342-302">**Variable d’environnement** : `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="52342-302">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a><span data-ttu-id="52342-303">Remplacer la configuration</span><span class="sxs-lookup"><span data-stu-id="52342-303">Override configuration</span></span>

<span data-ttu-id="52342-304">Utilisez [Configuration](xref:fundamentals/configuration/index) pour configurer l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="52342-304">Use [Configuration](xref:fundamentals/configuration/index) to configure Web Host.</span></span> <span data-ttu-id="52342-305">Dans l’exemple suivant, la configuration de l’hôte est spécifiée de façon facultative dans un fichier *hostsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="52342-305">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="52342-306">Toute configuration chargée à partir du fichier *hostsettings.json* est remplaçable par des arguments de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="52342-306">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="52342-307">La configuration définie (dans `config`) est utilisée pour configurer l’hôte avec [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="52342-307">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="52342-308">La configuration `IWebHostBuilder` est ajoutée à la configuration de l’application, mais l’inverse n’est pas vrai &mdash; `ConfigureAppConfiguration` n’a pas d’incidence sur la configuration `IWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="52342-308">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

<span data-ttu-id="52342-309">La configuration fournie par `UseUrls` est d’abord remplacée par la configuration *hostsettings.json*, puis par la configuration des arguments de ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="52342-309">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="52342-310">*hostsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="52342-310">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> <span data-ttu-id="52342-311">La méthode d’extension [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) ne peut pas analyser une section de configuration retournée par `GetSection` (par exemple, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="52342-311">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="52342-312">La méthode `GetSection` filtre les clés de configuration dans la section demandée, mais laisse le nom de la section dans les clés (par exemple, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="52342-312">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="52342-313">La méthode `UseConfiguration` suppose que les clés correspondent aux clés `WebHostBuilder` (par exemple, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="52342-313">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="52342-314">La présence du nom de la section dans les clés empêche l’utilisation des valeurs de la section pour configurer l’hôte.</span><span class="sxs-lookup"><span data-stu-id="52342-314">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="52342-315">Ce problème sera résolu dans une prochaine mise en production.</span><span class="sxs-lookup"><span data-stu-id="52342-315">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="52342-316">Pour obtenir plus d’informations et connaître les solutions de contournement possibles, consultez [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="52342-316">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="52342-317">`UseConfiguration` copie seulement les clés de la configuration `IConfiguration` fournie vers la configuration du générateur d’hôte.</span><span class="sxs-lookup"><span data-stu-id="52342-317">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="52342-318">Par conséquent, le fait de définir `reloadOnChange: true` pour les fichiers de paramètres XML, JSON et INI n’a aucun effet.</span><span class="sxs-lookup"><span data-stu-id="52342-318">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="52342-319">Pour spécifier l’exécution de l’hôte sur une URL particulière, vous pouvez passer la valeur souhaitée à partir d’une invite de commandes lors de l’exécution de [dotnet run](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="52342-319">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="52342-320">L’argument de ligne de commande remplace la valeur `urls` du fichier *hostsettings.json*, et le serveur écoute le port 8080 :</span><span class="sxs-lookup"><span data-stu-id="52342-320">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="52342-321">Gérer l’hôte</span><span class="sxs-lookup"><span data-stu-id="52342-321">Manage the host</span></span>

<span data-ttu-id="52342-322">**Exécuter**</span><span class="sxs-lookup"><span data-stu-id="52342-322">**Run**</span></span>

<span data-ttu-id="52342-323">La méthode `Run` démarre l’application web et bloque le thread appelant jusqu’à l’arrêt de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="52342-323">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="52342-324">**Démarrer**</span><span class="sxs-lookup"><span data-stu-id="52342-324">**Start**</span></span>

<span data-ttu-id="52342-325">Appelez la méthode `Start` pour exécuter l’hôte en mode non bloquant :</span><span class="sxs-lookup"><span data-stu-id="52342-325">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="52342-326">Si une liste d’URL est passée à la méthode `Start`, l’hôte écoute les URL spécifiées :</span><span class="sxs-lookup"><span data-stu-id="52342-326">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="52342-327">L’application peut initialiser et démarrer un nouvel hôte ayant les valeurs par défaut préconfigurées de `CreateDefaultBuilder` en utilisant une méthode d’usage statique.</span><span class="sxs-lookup"><span data-stu-id="52342-327">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="52342-328">Ces méthodes démarrent le serveur sans sortie de console et, avec [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown), elles attendent un arrêt (Ctrl-C/SIGINT ou SIGTERM) :</span><span class="sxs-lookup"><span data-stu-id="52342-328">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="52342-329">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="52342-329">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="52342-330">Commencez par un `RequestDelegate` :</span><span class="sxs-lookup"><span data-stu-id="52342-330">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="52342-331">Envoyez une requête à `http://localhost:5000` dans le navigateur pour recevoir la réponse « Hello World! »</span><span class="sxs-lookup"><span data-stu-id="52342-331">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="52342-332">`WaitForShutdown` bloque la requête jusqu’à l’émission d’une commande d’arrêt (Ctrl-C/SIGINT ou SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="52342-332">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="52342-333">L’application affiche le message `Console.WriteLine` et attend que l’utilisateur appuie sur une touche pour s’arrêter.</span><span class="sxs-lookup"><span data-stu-id="52342-333">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="52342-334">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="52342-334">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="52342-335">Commencez par une URL et `RequestDelegate` :</span><span class="sxs-lookup"><span data-stu-id="52342-335">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="52342-336">Produit le même résultat que **Start(RequestDelegate app)**, sauf que l’application répond sur `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="52342-336">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="52342-337">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="52342-337">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="52342-338">Utilisez une instance de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) pour utiliser le middleware de routage :</span><span class="sxs-lookup"><span data-stu-id="52342-338">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="52342-339">Utilisez les requêtes de navigateur suivantes avec l’exemple :</span><span class="sxs-lookup"><span data-stu-id="52342-339">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="52342-340">Requête</span><span class="sxs-lookup"><span data-stu-id="52342-340">Request</span></span>                                    | <span data-ttu-id="52342-341">Réponse</span><span class="sxs-lookup"><span data-stu-id="52342-341">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="52342-342">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="52342-342">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="52342-343">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="52342-343">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="52342-344">Lève une exception avec la chaîne « ooops! »</span><span class="sxs-lookup"><span data-stu-id="52342-344">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="52342-345">Lève une exception avec la chaîne « Uh oh! »</span><span class="sxs-lookup"><span data-stu-id="52342-345">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="52342-346">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="52342-346">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="52342-347">Hello World!</span><span class="sxs-lookup"><span data-stu-id="52342-347">Hello World!</span></span>                             |

<span data-ttu-id="52342-348">`WaitForShutdown` bloque la requête jusqu’à l’émission d’une commande d’arrêt (Ctrl-C/SIGINT ou SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="52342-348">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="52342-349">L’application affiche le message `Console.WriteLine` et attend que l’utilisateur appuie sur une touche pour s’arrêter.</span><span class="sxs-lookup"><span data-stu-id="52342-349">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="52342-350">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="52342-350">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="52342-351">Utilisez une URL et une instance de `IRouteBuilder` :</span><span class="sxs-lookup"><span data-stu-id="52342-351">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="52342-352">Produit le même résultat que **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, sauf que l’application répond sur `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="52342-352">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="52342-353">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="52342-353">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="52342-354">Fournissez un délégué pour configurer un `IApplicationBuilder` :</span><span class="sxs-lookup"><span data-stu-id="52342-354">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="52342-355">Envoyez une requête à `http://localhost:5000` dans le navigateur pour recevoir la réponse « Hello World! »</span><span class="sxs-lookup"><span data-stu-id="52342-355">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="52342-356">`WaitForShutdown` bloque la requête jusqu’à l’émission d’une commande d’arrêt (Ctrl-C/SIGINT ou SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="52342-356">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="52342-357">L’application affiche le message `Console.WriteLine` et attend que l’utilisateur appuie sur une touche pour s’arrêter.</span><span class="sxs-lookup"><span data-stu-id="52342-357">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="52342-358">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="52342-358">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="52342-359">Fournissez une URL et un délégué pour configurer un `IApplicationBuilder` :</span><span class="sxs-lookup"><span data-stu-id="52342-359">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="52342-360">Produit le même résultat que **StartWith(Action&lt;IApplicationBuilder&gt; app)**, sauf que l’application répond sur `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="52342-360">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="52342-361">Interface IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="52342-361">IHostingEnvironment interface</span></span>

<span data-ttu-id="52342-362">[L’interface IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) fournit des informations sur l’environnement d’hébergement web de l’application.</span><span class="sxs-lookup"><span data-stu-id="52342-362">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="52342-363">Utilisez [l’injection de constructeur](xref:fundamentals/dependency-injection) pour obtenir l’interface `IHostingEnvironment` afin d’utiliser ses propriétés et méthodes d’extension :</span><span class="sxs-lookup"><span data-stu-id="52342-363">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="52342-364">Vous pouvez utiliser une [approche basée sur une convention](xref:fundamentals/environments#environment-based-startup-class-and-methods) pour configurer l’application au démarrage en fonction de l’environnement.</span><span class="sxs-lookup"><span data-stu-id="52342-364">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="52342-365">Vous pouvez également injecter `IHostingEnvironment` dans le constructeur `Startup` pour l’utiliser dans `ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="52342-365">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="52342-366">En plus de la méthode d’extension `IsDevelopment`, `IHostingEnvironment` fournit les méthodes `IsStaging`, `IsProduction` et `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="52342-366">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="52342-367">Pour plus d'informations, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="52342-367">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="52342-368">Le service `IHostingEnvironment` peut également être injecté directement dans la méthode `Configure` pour configurer le pipeline de traitement :</span><span class="sxs-lookup"><span data-stu-id="52342-368">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="52342-369">`IHostingEnvironment` peut être injecté dans la méthode `Invoke` lors de la création d’un [intergiciel (middleware)](xref:fundamentals/middleware/write) personnalisé :</span><span class="sxs-lookup"><span data-stu-id="52342-369">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/write):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="52342-370">Interface IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="52342-370">IApplicationLifetime interface</span></span>

<span data-ttu-id="52342-371">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) s’utilise pour les opérations post-démarrage et arrêt.</span><span class="sxs-lookup"><span data-stu-id="52342-371">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="52342-372">Trois propriétés de l’interface sont des jetons d’annulation utilisés pour inscrire les méthodes `Action` qui définissent les événements de démarrage et d’arrêt.</span><span class="sxs-lookup"><span data-stu-id="52342-372">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="52342-373">Jeton d’annulation</span><span class="sxs-lookup"><span data-stu-id="52342-373">Cancellation Token</span></span>    | <span data-ttu-id="52342-374">Événement déclencheur</span><span class="sxs-lookup"><span data-stu-id="52342-374">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="52342-375">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="52342-375">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="52342-376">L’hôte a complètement démarré.</span><span class="sxs-lookup"><span data-stu-id="52342-376">The host has fully started.</span></span> |
| [<span data-ttu-id="52342-377">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="52342-377">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="52342-378">L’hôte effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="52342-378">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="52342-379">Toutes les requêtes doivent être traitées.</span><span class="sxs-lookup"><span data-stu-id="52342-379">All requests should be processed.</span></span> <span data-ttu-id="52342-380">L’arrêt est bloqué tant que cet événement n’est pas terminé.</span><span class="sxs-lookup"><span data-stu-id="52342-380">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="52342-381">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="52342-381">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="52342-382">L’hôte effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="52342-382">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="52342-383">Des requêtes peuvent encore être en cours de traitement.</span><span class="sxs-lookup"><span data-stu-id="52342-383">Requests may still be processing.</span></span> <span data-ttu-id="52342-384">L’arrêt est bloqué tant que cet événement n’est pas terminé.</span><span class="sxs-lookup"><span data-stu-id="52342-384">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="52342-385">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requête l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="52342-385">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="52342-386">La classe suivante utilise `StopApplication` pour arrêter normalement une application quand la méthode `Shutdown` de la classe est appelée :</span><span class="sxs-lookup"><span data-stu-id="52342-386">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="52342-387">Validation de l’étendue</span><span class="sxs-lookup"><span data-stu-id="52342-387">Scope validation</span></span>

<span data-ttu-id="52342-388">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) affecte la valeur `true` à [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) si l’environnement de l’application est Développement.</span><span class="sxs-lookup"><span data-stu-id="52342-388">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="52342-389">Quand `ValidateScopes` est défini sur `true`, le fournisseur de services par défaut vérifie que :</span><span class="sxs-lookup"><span data-stu-id="52342-389">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="52342-390">Les services délimités ne sont pas résolus directement ou indirectement à partir du fournisseur de services racine.</span><span class="sxs-lookup"><span data-stu-id="52342-390">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="52342-391">Les services Scoped ne sont pas directement ou indirectement injectés dans des singletons.</span><span class="sxs-lookup"><span data-stu-id="52342-391">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="52342-392">Le fournisseur de services racine est créé quand [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) est appelé.</span><span class="sxs-lookup"><span data-stu-id="52342-392">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="52342-393">La durée de vie du fournisseur de services racine correspond à la durée de vie de l’application/du serveur quand le fournisseur démarre avec l’application et qu’il est supprimé quand l’application s’arrête.</span><span class="sxs-lookup"><span data-stu-id="52342-393">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="52342-394">Les services Scoped sont supprimés par le conteneur qui les a créés.</span><span class="sxs-lookup"><span data-stu-id="52342-394">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="52342-395">Si un service Scoped est créé dans le conteneur racine, la durée de vie du service est promue en singleton, car elle est supprimée par le conteneur racine seulement quand l’application/le serveur est arrêté.</span><span class="sxs-lookup"><span data-stu-id="52342-395">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="52342-396">La validation des étendues du service permet de traiter ces situations quand `BuildServiceProvider` est appelé.</span><span class="sxs-lookup"><span data-stu-id="52342-396">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="52342-397">Pour toujours valider les étendues, notamment dans l’environnement de production, configurez [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) avec [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) sur le créateur d’hôte :</span><span class="sxs-lookup"><span data-stu-id="52342-397">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a><span data-ttu-id="52342-398">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="52342-398">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
