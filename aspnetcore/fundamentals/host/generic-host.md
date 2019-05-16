---
title: Hôte générique .NET
author: guardrex
description: Découvrez l’hôte générique d’ASP.NET Core, qui est responsable de la gestion du démarrage et de la durée de vie des applications.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/25/2019
uid: fundamentals/host/generic-host
ms.openlocfilehash: d823e2189d21e0566656b7eb8c9164d02e43d0ea
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64889114"
---
# <a name="net-generic-host"></a>Hôte générique .NET

Par [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

Les applications ASP.NET Core configurent et lancent un hôte. L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.

Cet article traite de l’hôte générique .NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>).

L’hôte générique se distingue de l’hôte web en ce sens qu’il sépare le pipeline HTTP de l’API Hôte web pour autoriser un plus large éventail de scénarios d’hôte. La messagerie, les tâches en arrière-plan et autres charges de travail non HTTP peuvent utiliser l’hôte générique et bénéficier de fonctionnalités transversales, comme la configuration, l’injection de dépendances (DI) et la journalisation.

À compter d’ASP.NET Core 3.0, l’hôte générique est recommandé pour les charges de travail HTTP et non-HTTP. Une implémentation de serveur HTTP, si elle est incluse, s’exécute en tant qu’implémentation de <xref:Microsoft.Extensions.Hosting.IHostedService>. <xref:Microsoft.Extensions.Hosting.IHostedService> est une interface qui peut aussi être utilisée pour d’autres charges de travail.

L’hôte web n’est plus recommandé pour les applications web, mais reste disponible à des fins de compatibilité descendante.

> [!NOTE]
> Le reste de cet article n’a pas été mis à jour pour la version 3.0.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Les applications ASP.NET Core configurent et lancent un hôte. L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications.

Cet article traite de l’hôte générique ASP.NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>), qui est utilisé pour les applications qui ne traitent pas les requêtes HTTP.

L’objectif de l’hôte générique est de séparer le pipeline HTTP de l’API Hôte web pour permettre un plus large éventail de scénarios d’hôte. La messagerie, les tâches en arrière-plan et autres charges de travail non HTTP basées sur l’hôte générique bénéficient de fonctionnalités transversales, comme la configuration, l’injection de dépendances (DI) et la journalisation.

L’hôte générique est nouveau dans ASP.NET Core 2.1 et n’est pas adapté aux scénarios d’hébergement web. Pour les scénarios d’hébergement de web, utilisez l’[hôte web](xref:fundamentals/host/web-host). L’hôte générique est appelé à remplacer l’hôte web dans une version ultérieure et à servir d’API hôte principale dans les scénarios HTTP et non-HTTP.

::: moniker-end

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

Quand vous exécutez l’exemple d’application dans [Visual Studio Code](https://code.visualstudio.com/), utilisez un *terminal externe ou intégré*. N’exécutez pas l’exemple dans une `internalConsole`.

Pour définir la console dans Visual Studio Code :

1. Ouvrez le fichier *.vscode/launch.json*.
1. Dans la configuration **.NET Core Launch (console)**, recherchez l’entrée **console**. Définissez la valeur avec `externalTerminal` ou `integratedTerminal`.

## <a name="introduction"></a>Introduction

La bibliothèque de l’hôte générique est disponible dans l’espace de noms <xref:Microsoft.Extensions.Hosting> et est fournie par le package [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/). Le package `Microsoft.Extensions.Hosting` est inclus dans le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 ou ultérieur).

<xref:Microsoft.Extensions.Hosting.IHostedService> est le point d’entrée pour exécuter le code. Chaque implémentation <xref:Microsoft.Extensions.Hosting.IHostedService> est exécutée dans l’ordre d’[inscription des services dans ConfigureServices](#configureservices). <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> est appelé sur chaque <xref:Microsoft.Extensions.Hosting.IHostedService> au démarrage de l’hôte, tandis que <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> est appelé dans l’ordre d’inscription inverse quand l’hôte s’arrête normalement.

## <a name="set-up-a-host"></a>Configurer un hôte

<xref:Microsoft.Extensions.Hosting.IHostBuilder> est le principal composant que les applications et les bibliothèques utilisent pour initialiser, générer et exécuter l’hôte :

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a>Options

<xref:Microsoft.Extensions.Hosting.HostOptions> configure les options pour <xref:Microsoft.Extensions.Hosting.IHost>.

### <a name="shutdown-timeout"></a>Délai d’expiration

<xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> définit le délai d’expiration pour <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>. La valeur par défaut est de cinq secondes.

La configuration de l’option suivante dans `Program.Main` augmente le délai d’expiration par défaut de 5 à 20 secondes :

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

## <a name="default-services"></a>Services par défaut

Les services suivants sont inscrits au moment de l’initialisation de l’hôte :

* [Environnement](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* [Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)
* <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)
* <xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)
* <xref:Microsoft.Extensions.Hosting.IHost>
* [Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)
* [Journalisation](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)

## <a name="host-configuration"></a>Configuration de l’hôte

La création de la configuration d’hôte fait appel aux opérations suivantes :

* Appel de méthodes d’extension sur <xref:Microsoft.Extensions.Hosting.IHostBuilder> pour définir la [racine du contenu](#content-root) et [l’environnement](#environment).
* Lecture de la configuration à partir des fournisseurs de configuration dans <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.

### <a name="extension-methods"></a>Méthodes d’extension

### <a name="application-key-name"></a>Clé d’application (nom)

La propriété [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) est définie à partir de la configuration de l’hôte pendant la construction de l’hôte. Pour définir la valeur explicitement, utilisez [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey) :

**Clé** : applicationName  
**Type** : *string*  
**Par défaut** : nom de l’assembly contenant le point d’entrée de l’application.  
**Définition avec** : `HostBuilderContext.HostingEnvironment.ApplicationName`  
**Variable d’environnement** : `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` est [facultatif et défini par l’utilisateur](#configurehostconfiguration))

### <a name="content-root"></a>Racine de contenu

Ce paramètre détermine où l’hôte commence la recherche des fichiers de contenu.

**Clé** : contentRoot  
**Type** : *string*  
**Par défaut** : dossier où réside l’assembly de l’application.  
**Définition avec** : `UseContentRoot`  
**Variable d’environnement** : `<PREFIX_>CONTENTROOT` (`<PREFIX_>` est [facultatif et défini par l’utilisateur](#configurehostconfiguration))

Si le chemin est introuvable, l’hôte ne peut pas démarrer.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

### <a name="environment"></a>Environnement

Définit l’[environnement](xref:fundamentals/environments) de l’application.

**Clé** : environment  
**Type** : *string*  
**Par défaut** : Production  
**Définition avec** : `UseEnvironment`  
**Variable d’environnement** : `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` est [facultatif et défini par l’utilisateur](#configurehostconfiguration))

L’environnement peut être défini à n’importe quelle valeur. Les valeurs définies par le framework sont `Development`, `Staging` et `Production`. Les valeurs ne respectent pas la casse.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a>ConfigureHostConfiguration

<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> utilise un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> pour créer un <xref:Microsoft.Extensions.Configuration.IConfiguration> pour l’hôte. La configuration d’hôte permet d’initialiser le <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> en vue de son utilisation dans le processus de génération de l’application.

<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> peut être appelé plusieurs fois avec des résultats additifs. L’hôte utilise l’option qui définit une valeur en dernier sur une clé donnée.

La configuration d’hôte est automatiquement acheminée à la configuration de l’application ([ConfigureAppConfiguration](#configureappconfiguration) et le reste de l’application).

Aucun fournisseur n’est inclus par défaut. Vous devez spécifier explicitement les fournisseurs de configuration dont l’application a besoin dans <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, notamment :

* Configuration du fichier (par exemple, à partir d’un fichier *hostsettings.json*).
* Configuration de la variable d’environnement.
* Configuration de l’argument de ligne de commande.
* Tout autre fournisseur de configuration obligatoire.

Pour activer la configuration de fichier de l’hôte, spécifiez le chemin de base de l’application avec `SetBasePath`, suivi d’un appel à l’un des [fournisseurs de configuration de fichier](xref:fundamentals/configuration/index#file-configuration-provider). L’exemple d’application utilise un fichier JSON, *hostsettings.json*, et appelle <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> pour utiliser les paramètres de configuration d’hôte du fichier.

Pour ajouter la [configuration de variable d’environnement](xref:fundamentals/configuration/index#environment-variables-configuration-provider) de l’hôte, appelez <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> sur le générateur d’hôte. `AddEnvironmentVariables` accepte un préfixe facultatif défini par l’utilisateur. L’exemple d’application utilise un préfixe `PREFIX_`. Ce préfixe est supprimé à la lecture des variables d’environnement. Lorsque l’hôte de l’exemple d’application est configuré, la valeur de variable d’environnement de `PREFIX_ENVIRONMENT` devient la valeur de configuration d’hôte de la clé `environment`.

Pendant le développement, lorsque vous utilisez [Visual Studio](https://visualstudio.microsoft.com) ou que vous lancez une application avec `dotnet run`, vous pouvez définir les variables d’environnement dans le fichier *Properties/launchSettings.json*. Dans [Visual Studio Code](https://code.visualstudio.com/), elles peuvent être définies dans le fichier *.vscode/launch.json* au cours du développement. Pour plus d'informations, consultez <xref:fundamentals/environments>.

Pour ajouter la [configuration de ligne de commande](xref:fundamentals/configuration/index#command-line-configuration-provider), appelez <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>. La configuration de ligne de commande est ajoutée en dernier pour permettre aux arguments de ligne de commande de substituer la configuration fournie par les fournisseurs de configuration antérieurs.

*hostsettings.json* :

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

Une configuration supplémentaire peut être fournie à l’aide des clés [applicationName](#application-key-name) et [contentRoot](#content-root).

Exemple de configuration `HostBuilder` avec <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> :

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Pour créer la configuration d’application, appelez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> sur l’implémentation <xref:Microsoft.Extensions.Hosting.IHostBuilder>. <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> utilise un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> pour créer un <xref:Microsoft.Extensions.Configuration.IConfiguration> pour l’application. <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> peut être appelé plusieurs fois avec des résultats additifs. L’application utilise l’option qui définit une valeur en dernier sur une clé donnée. La configuration créée par <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> est disponible dans [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) pour les opérations suivantes et dans <xref:Microsoft.Extensions.Hosting.IHost.Services*>.

La configuration d’application reçoit automatiquement la configuration d’hôte fournie par [ConfigureHostConfiguration](#configurehostconfiguration).

Exemple de configuration d’application avec <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> :

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appsettings.json* :

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appsettings.Development.json* :

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appsettings.Production.json* :

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

Pour déplacer des fichiers de paramètres vers le répertoire de sortie, spécifiez-les en tant qu’[éléments de projet MSBuild](/visualstudio/msbuild/common-msbuild-project-items) dans le fichier projet. L’exemple d’application déplace ses fichiers de paramètres d’application JSON et *hostsettings.json* avec l’élément `<Content>` suivant :

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> Les méthodes d’extension de configuration, telles que <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> et <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*>, nécessitent des packages NuGet supplémentaires, tels que [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) et [ Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables). Si l’application n’utilise pas le [métapaquet Microsoft.AspNetCore.App ](xref:fundamentals/metapackage-app), ces packages doivent être ajoutés au projet en plus du noyau [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration). Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.

## <a name="configureservices"></a>ConfigureServices

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> ajoute des services au conteneur [d’injection de dépendances](xref:fundamentals/dependency-injection) de l’application. <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> peut être appelé plusieurs fois avec des résultats additifs.

Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>. Pour plus d'informations, consultez <xref:fundamentals/host/hosted-services>.

L’[exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) utilise la méthode d’extension `AddHostedService` afin d’ajouter un service pour les événements de durée de vie, `LifetimeEventsHostedService`, et une tâche en arrière-plan chronométrée, `TimedHostedService`, à l’application :

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> ajoute un délégué pour configurer le <xref:Microsoft.Extensions.Logging.ILoggingBuilder> fourni. <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> peut être appelé plusieurs fois avec des résultats additifs.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> écoute Ctrl+C/SIGINT ou SIGTERM et appelle <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> pour démarrer le processus d’arrêt. <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> déverrouille les extensions telles que [RunAsync](#runasync) et [WaitForShutdownAsync](#waitforshutdownasync). <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> est préinscrit comme implémentation de durée de vie par défaut. La dernière durée de vie inscrite est utilisée.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>Configuration du conteneur

Pour prendre en charge le branchement dans d’autres conteneurs, l’hôte peut accepter un <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>. L’ajout d’une fabrique ne fait pas partie de l’inscription de conteneur DI mais est plutôt une tâche un intrinsèque à l’hôte utilisée pour créer le conteneur DI concret. [UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) remplace la fabrique par défaut utilisée pour créer le fournisseur de services de l’application.

La configuration de conteneur personnalisée est gérée par la méthode <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>. <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> offre une expérience fortement typée pour configurer le conteneur sur l’API hôte sous-jacente. <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> peut être appelé plusieurs fois avec des résultats additifs.

Créer un conteneur de service pour l’application :

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

Fournir une fabrique de conteneur de service :

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

Utiliser la fabrique et configurer le conteneur de service personnalisé pour l’application :

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a>Extensibilité

L’extensibilité de l’hôte est effectuée avec les méthodes d’extension sur <xref:Microsoft.Extensions.Hosting.IHostBuilder>. L’exemple suivant montre comment une méthode d’extension étend une implémentation <xref:Microsoft.Extensions.Hosting.IHostBuilder> avec l’exemple [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) démontré dans <xref:fundamentals/host/hosted-services>.

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

Une application établit la méthode d'extension `UseHostedService` pour inscrire le service hébergé passé dans `T` :

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

## <a name="manage-the-host"></a>Gérer l’hôte

L’implémentation <xref:Microsoft.Extensions.Hosting.IHost> est chargée de démarrer et d’arrêter les implémentations <xref:Microsoft.Extensions.Hosting.IHostedService> qui sont inscrites dans le conteneur de service.

### <a name="run"></a>Exécuter

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> exécute l’application et bloque le thread appelant jusqu’à l’arrêt de l’hôte :

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

### <a name="runasync"></a>RunAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> exécute l’application et retourne une <xref:System.Threading.Tasks.Task> qui est effectuée quand le jeton d’annulation ou l’arrêt est déclenché :

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

### <a name="runconsoleasync"></a>RunConsoleAsync

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> permet la prise en charge de la console, génère et démarre l’hôte et attend Ctrl+C/SIGINT ou SIGTERM pour l’arrêter.

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

### <a name="start-and-stopasync"></a>Start et StopAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> démarre l’hôte en mode synchrone.

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> tente d’arrêter l’hôte dans le délai d’attente fourni.

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

### <a name="startasync-and-stopasync"></a>StartAsync et StopAsync

<xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> démarre l’application.

<xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> arrête l’application.

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

### <a name="waitforshutdown"></a>WaitForShutdown

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> est déclenché par le biais de <xref:Microsoft.Extensions.Hosting.IHostLifetime>, par exemple <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (écoute Ctrl+C/SIGINT ou SIGTERM). <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> appelle <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.

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

### <a name="waitforshutdownasync"></a>WaitForShutdownAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> retourne une <xref:System.Threading.Tasks.Task> qui est effectuée quand l’arrêt est déclenché par le biais du jeton fourni et appelle <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.

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

### <a name="external-control"></a>Contrôle externe

Le contrôle externe de l’hôte peut être effectué à l’aide de méthodes pouvant être appelées de façon externe :

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

<xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> est appelé au début de <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, lequel attend qu’il soit fini avant de continuer. Cela permet de retarder le démarrage jusqu'à ce que celui-ci soit signalé par un événement externe.

## <a name="ihostingenvironment-interface"></a>Interface IHostingEnvironment

<xref:Microsoft.Extensions.Hosting.IHostingEnvironment> fournit des informations sur l’environnement d’hébergement de l’application. Utilisez [l’injection de constructeur](xref:fundamentals/dependency-injection) pour obtenir l’interface <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> afin d’utiliser ses propriétés et méthodes d’extension :

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

Pour plus d'informations, consultez <xref:fundamentals/environments>.

## <a name="iapplicationlifetime-interface"></a>Interface IApplicationLifetime

<xref:Microsoft.Extensions.Hosting.IApplicationLifetime> s’utilise pour les activités de post-démarrage et d’arrêt, notamment pour les demandes d’arrêt normal. Trois propriétés de l’interface sont des jetons d’annulation utilisés pour inscrire les méthodes <xref:System.Action> qui définissent les événements de démarrage et d’arrêt.

| Jeton d’annulation | Événement déclencheur |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | L’hôte a complètement démarré. |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | L’hôte effectue un arrêt approprié. Toutes les requêtes doivent être traitées. L’arrêt est bloqué tant que cet événement n’est pas terminé. |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | L’hôte effectue un arrêt approprié. Des requêtes peuvent encore être en cours de traitement. L’arrêt est bloqué tant que cet événement n’est pas terminé. |

Le constructeur injecte le service <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> dans une classe. [L’exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) utilise l’injection de constructeur dans une classe `LifetimeEventsHostedService` (une implémentation <xref:Microsoft.Extensions.Hosting.IHostedService>) pour inscrire les événements.

*LifetimeEventsHostedService.cs*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> demande l’arrêt de l’application. La classe suivante utilise <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> pour arrêter normalement une application quand la méthode `Shutdown` de la classe est appelée :

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

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:fundamentals/host/hosted-services>
