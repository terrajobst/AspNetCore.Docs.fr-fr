---
title: Hôte générique .NET
author: guardrex
description: Découvrez l’hôte générique dans .NET, responsable de la gestion du démarrage et de la durée de vie des applications.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: 0924e2764958911dc1711d5427f6dd58e8873739
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477603"
---
# <a name="net-generic-host"></a>Hôte générique .NET

Par [Luke Latham](https://github.com/guardrex)

Les applications .NET Core configurent et lancent un *hôte*. L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications. Cette rubrique traite de l’hôte générique ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), utile pour l’hébergement d’applications qui ne traitent pas les requêtes HTTP. Pour en savoir plus sur l’hôte web ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), consultez <xref:fundamentals/host/web-host>.

L’objectif de l’hôte générique est de séparer le pipeline HTTP de l’API d’hôte web pour permettre un plus large éventail de scénarios d’hôte. La messagerie, les tâches en arrière-plan et autres charges de travail non-HTTP basées sur l’hôte générique bénéficient de fonctionnalités transversales, comme la configuration, l’injection de dépendances (DI) et la journalisation.

L’hôte générique est nouveau dans ASP.NET Core 2.1 et n’est pas adapté aux scénarios d’hébergement web. Pour les scénarios d’hébergement de web, utilisez l’[hôte web](xref:fundamentals/host/web-host). L’hôte générique est en cours de développement pour remplacer l’hôte web dans une version ultérieure et servir d’API hôte principale dans les scénarios HTTP et non-HTTP.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

Quand vous exécutez l’exemple d’application dans [Visual Studio Code](https://code.visualstudio.com/), utilisez un *terminal externe ou intégré*. N’exécutez pas l’exemple dans une `internalConsole`.

Pour définir la console dans Visual Studio Code :

1. Ouvrez le fichier *.vscode/launch.json*.
1. Dans la configuration **.NET Core Launch (console)**, recherchez l’entrée **console**. Définissez la valeur avec `externalTerminal` ou `integratedTerminal`.

## <a name="introduction"></a>Introduction

La bibliothèque de l’hôte générique est disponible dans [l’espace de noms Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) et est fournie par le package [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/). Le package `Microsoft.Extensions.Hosting` est inclus dans le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 ou ultérieur).

[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) est le point d’entrée pour exécuter le code. Chaque implémentation `IHostedService` est exécutée dans l’ordre d’[inscription des services dans ConfigureServices](#configureservices). [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) est appelé sur chaque `IHostedService` au démarrage de l’hôte, tandis que [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) est appelé dans l’ordre d’inscription inverse quand l’hôte s’arrête normalement.

## <a name="set-up-a-host"></a>Configurer un hôte

[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) est le principal composant que les applications et les bibliothèques utilisent pour initialiser, générer et exécuter l’hôte :

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

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

[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) s’appuie sur les approches suivantes pour définir les valeurs de configuration de l’hôte :

* Générateur de configuration
* Configuration de méthode d’extension

### <a name="configuration-builder"></a>Générateur de configuration

Le générateur de configuration d’hôte est créé en appelant [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) dans l’implémentation [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder). `ConfigureHostConfiguration` utilise un [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) afin de créer une [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) pour l’hôte. Le générateur de configuration initialise les [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) pour les utiliser dans le processus de génération de l’application.

La configuration des variables d’environnement n’est pas ajoutée par défaut. Appelez [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) sur le générateur d’hôte pour configurer l’hôte à partir de variables d’environnement. `AddEnvironmentVariables` accepte un préfixe facultatif défini par l’utilisateur. L’exemple d’application utilise un préfixe `PREFIX_`. Ce préfixe est supprimé à la lecture des variables d’environnement. Lorsque l’hôte de l’exemple d’application est configuré, la valeur de variable d’environnement de `PREFIX_ENVIRONMENT` devient la valeur de configuration d’hôte de la clé `environment`.

Pendant le développement, lorsque vous utilisez [Visual Studio](https://www.visualstudio.com/) ou que vous lancez une application avec `dotnet run`, vous pouvez définir les variables d’environnement dans le fichier *Properties/launchSettings.json*. Dans [Visual Studio Code](https://code.visualstudio.com/), elles peuvent être définies dans le fichier *.vscode/launch.json* au cours du développement. Pour plus d'informations, consultez <xref:fundamentals/environments>.

`ConfigureHostConfiguration` peut être appelé plusieurs fois avec des résultats additifs. L’hôte utilise l’option qui définit une valeur en dernier.

*hostsettings.json* :

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

Exemple de configuration `HostBuilder` avec `ConfigureHostConfiguration` :

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> Actuellement, la méthode d’extension [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) n’est pas capable d’analyser une section de configuration retournée par [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (par exemple, `.AddConfiguration(Configuration.GetSection("section"))`. La méthode `GetSection` filtre les clés de configuration dans la section demandée, mais laisse le nom de la section dans les clés (par exemple, `section:environment`). La méthode `AddConfiguration` suppose que les clés correspondent aux clés `HostBuilder` (par exemple, `environment`). La présence du nom de la section dans les clés empêche l’utilisation des valeurs de la section pour configurer l’hôte. Ce problème sera résolu dans une prochaine mise en production. Pour obtenir plus d’informations et connaître les solutions de contournement possibles, consultez [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).

### <a name="extension-method-configuration"></a>Configuration de méthode d’extension

Les méthodes d’extension sont appelées sur l’implémentation `IHostBuilder` pour configurer la racine de contenu et l’environnement.

#### <a name="application-key-name"></a>Clé d’application (Nom)

La propriété [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) est définie à partir de la configuration de l’hôte pendant la construction de l’hôte. Pour définir la valeur explicitement, utilisez [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey) :

**Clé** : applicationName  
**Type** : *string*  
**Par défaut** : nom de l’assembly contenant le point d’entrée de l’application.  
**Définition avec** : `HostBuilderContext.HostingEnvironment.ApplicationName`  
**Variable d’environnement** : `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` est [facultatif et défini par l’utilisateur](#configuration-builder))

```csharp
var host = new HostBuilder()
    .ConfigureAppConfiguration((hostContext, configApp) =>
    {
        hostContext.HostingEnvironment.ApplicationName = "CustomApplicationName";
    })
```

#### <a name="content-root"></a>Racine de contenu

Ce paramètre détermine où l’hôte commence la recherche des fichiers de contenu.

**Clé** : contentRoot  
**Type** : *string*  
**Valeur par défaut** : dossier où réside l’assembly de l’application.  
**Définition avec** : `UseContentRoot`  
**Variable d’environnement** : `<PREFIX_>CONTENTROOT` (`<PREFIX_>` est [facultatif et défini par l’utilisateur](#configuration-builder))

Si le chemin est introuvable, l’hôte ne peut pas démarrer.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a>Environnement

Définit l’[environnement](xref:fundamentals/environments) de l’application.

**Clé** : environment  
**Type** : *string*  
**Valeur par défaut** : Production  
**Définition avec** : `UseEnvironment`  
**Variable d’environnement** : `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` est [facultatif et défini par l’utilisateur](#configuration-builder))

L’environnement peut être défini à n’importe quelle valeur. Les valeurs définies par le framework sont `Development`, `Staging` et `Production`. Les valeurs ne respectent pas la casse.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Le générateur de configuration d’application est créé en appelant [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) dans l’implémentation [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder). `ConfigureAppConfiguration` utilise un [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) afin de créer une [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) pour l’application. `ConfigureAppConfiguration` peut être appelé plusieurs fois avec des résultats additifs. L’application utilise l’option qui définit une valeur en dernier. La configuration créée par `ConfigureAppConfiguration` est disponible dans [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) pour les opérations suivantes et dans [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).

Exemple de configuration d’application avec `ConfigureAppConfiguration` :

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appsettings.json* :

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appsettings.Development.json* :

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appsettings.Production.json* :

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> Actuellement, la méthode d’extension [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) n’est pas capable d’analyser une section de configuration retournée par [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (par exemple, `.AddConfiguration(Configuration.GetSection("section"))`. La méthode `GetSection` filtre les clés de configuration dans la section demandée, mais laisse le nom de la section dans les clés (par exemple, `section:Logging:LogLevel:Default`). La méthode `AddConfiguration` attend une correspondance exacte pour les clés de configuration (par exemple, `Logging:LogLevel:Default`). La présence du nom de la section dans les clés empêche l’utilisation des valeurs de la section pour configurer l’application. Ce problème sera résolu dans une prochaine mise en production. Pour obtenir plus d’informations et connaître les solutions de contournement possibles, consultez [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).

Pour déplacer des fichiers de paramètres vers le répertoire de sortie, spécifiez-les en tant qu’[éléments de projet MSBuild](/visualstudio/msbuild/common-msbuild-project-items) dans le fichier projet. L’exemple d’application déplace ses fichiers de paramètres d’application JSON et *hostsettings.json* avec l’élément **&lt;Content:&gt;** suivant :

```xml
<ItemGroup>
  <Content Include="**\*.json" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a>ConfigureServices

[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) ajoute des services au conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) de l’application. `ConfigureServices` peut être appelé plusieurs fois avec des résultats additifs.

Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice). Pour plus d'informations, consultez <xref:fundamentals/host/hosted-services>.

L’[exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) utilise la méthode d’extension `AddHostedService` afin d’ajouter un service pour les événements de durée de vie, `LifetimeEventsHostedService`, et une tâche en arrière-plan chronométrée, `TimedHostedService`, à l’application :

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) ajoute un délégué pour configurer le [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder) fourni. `ConfigureLogging` peut être appelé plusieurs fois avec des résultats additifs.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) écoute `Ctrl+C`/SIGINT ou SIGTERM et appelle [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) pour démarrer le processus d’arrêt. `UseConsoleLifetime` déverrouille les extensions telles que [RunAsync](#runasync) et [WaitForShutdownAsync](#waitforshutdownasync). [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) est préinscrit en tant qu’implémentation de durée de vie par défaut. La dernière durée de vie inscrite est utilisée.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>Configuration du conteneur

Pour prendre en charge le branchement dans d’autres conteneurs, l’hôte peut accepter un [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1). L’ajout d’une fabrique ne fait pas partie de l’inscription de conteneur DI mais est plutôt une tâche un intrinsèque à l’hôte utilisée pour créer le conteneur DI concret. [UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) remplace la fabrique par défaut utilisée pour créer le fournisseur de services de l’application.

La configuration de conteneur personnalisée est gérée par la méthode [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer). `ConfigureContainer` offre une expérience fortement typée pour configurer le conteneur sur l’API hôte sous-jacente. `ConfigureContainer` peut être appelé plusieurs fois avec des résultats additifs.

Créer un conteneur de service pour l’application :

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

Fournir une fabrique de conteneur de service :

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

Utiliser la fabrique et configurer le conteneur de service personnalisé pour l’application :

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a>Extensibilité

L’extensibilité de l’hôte est effectuée avec les méthodes d’extension sur `IHostBuilder`. L’exemple suivant montre comment une méthode d’extension étend une implémentation `IHostBuilder` avec l’exemple [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) démontré dans <xref:fundamentals/host/hosted-services>.

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

L’implémentation [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) est chargée de démarrer et d’arrêter les implémentations `IHostedService` qui sont inscrites dans le conteneur de service.

### <a name="run"></a>Exécuter

[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) exécute l’application et bloque le thread appelant jusqu’à l’arrêt de l’hôte :

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

[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) exécute l’application et retourne une `Task` qui est effectuée quand le jeton d’annulation ou l’arrêt est déclenché :

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

[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) permet la prise en charge de la console, génère et démarre l’hôte et attend `Ctrl+C`/SIGINT ou SIGTERM pour l’arrêter.

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

[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) démarre l’hôte en mode synchrone.

[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) tente d’arrêter l’hôte dans le délai d’attente fourni.

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

[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) démarre l’application.

[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) arrête l’application.

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

[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) est déclenché via [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), comme [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (écoute `Ctrl+C`/SIGINT ou SIGTERM). `WaitForShutdown` appelle [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).

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

[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) retourne une `Task` qui est effectuée quand l’arrêt est déclenché via le jeton fourni et appelle [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).

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

[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) est appelé au début de [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), lequel attend qu’il soit fini avant de continuer. Cela permet de retarder le démarrage jusqu'à ce que celui-ci soit signalé par un événement externe.

## <a name="ihostingenvironment-interface"></a>Interface IHostingEnvironment

[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) fournit des informations sur l’environnement d’hébergement de l’application. Utilisez [l’injection de constructeur](xref:fundamentals/dependency-injection) pour obtenir l’interface `IHostingEnvironment` afin d’utiliser ses propriétés et méthodes d’extension :

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

[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) s’utilise pour les activités de post-démarrage et d’arrêt, notamment pour les demandes d’arrêt normal. Trois propriétés de l’interface sont des jetons d’annulation utilisés pour inscrire les méthodes `Action` qui définissent les événements de démarrage et d’arrêt.

| Jeton d’annulation | Événement déclencheur |
| ------------------ | --------------------- |
| [ApplicationStarted](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | L’hôte a complètement démarré. |
| [ApplicationStopped](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | L’hôte effectue un arrêt approprié. Toutes les requêtes doivent être traitées. L’arrêt est bloqué tant que cet événement n’est pas terminé. |
| [ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | L’hôte effectue un arrêt approprié. Des requêtes peuvent encore être en cours de traitement. L’arrêt est bloqué tant que cet événement n’est pas terminé. |

Le constructeur injecte le service `IApplicationLifetime` dans une classe. [L’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) utilise l’injection de constructeur dans une classe `LifetimeEventsHostedService` (une implémentation `IHostedService`) pour inscrire les événements.

*LifetimeEventsHostedService.cs*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requête l’arrêt de l’application. La classe suivante utilise `StopApplication` pour arrêter normalement une application quand la méthode `Shutdown` de la classe est appelée :

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
* [Hosting repo samples on GitHub](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
