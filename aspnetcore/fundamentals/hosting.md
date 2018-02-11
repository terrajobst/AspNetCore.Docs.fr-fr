---
title: "Hébergement dans ASP.NET Core"
author: guardrex
description: "Découvrez l’hôte web dans ASP.NET Core, qui est responsable de la gestion du démarrage et de la durée de vie des applications."
manager: wpickett
ms.author: riande
ms.date: 09/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosting
ms.openlocfilehash: 78209c8d34fa1a2a164ae333d625feca1e145e89
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="hosting-in-aspnet-core"></a>Hébergement dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

Les applications ASP.NET Core configurent et lancent un *hôte*. L’hôte est responsable de la gestion du démarrage et de la durée de vie des applications. Au minimum, l’hôte configure un serveur ainsi qu’un pipeline de traitement des requêtes.

## <a name="setting-up-a-host"></a>Configuration d’un hôte

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Créez un hôte en utilisant une instance de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Cette opération est généralement effectuée au point d’entrée de l’application, à savoir la méthode `Main`. Dans les modèles de projet, `Main` se trouve dans *Program.cs*. Un fichier *Program.cs* standard appelle [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) pour commencer la configuration d’un hôte :

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

`CreateDefaultBuilder` effectue les tâches suivantes :

* Configure [Kestrel](servers/kestrel.md) en tant que serveur web. Pour connaître les options Kestrel par défaut, consultez la [section sur les options Kestrel dans Implémentation du serveur web Kestrel dans ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).
* Définit le chemin retourné par [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) comme racine de contenu.
* Charge la configuration facultative à partir des éléments suivants :
  * *appsettings.json*
  * *appsettings.{Environment}.json*
  * Les [secrets utilisateur](xref:security/app-secrets) quand l’application s’exécute dans l’environnement `Development`
  * Variables d’environnement
  * Arguments de ligne de commande
* Configure la [journalisation](xref:fundamentals/logging/index) des sorties de la console et du débogage. La journalisation inclut les règles de [filtrage de journal](xref:fundamentals/logging/index#log-filtering) qui sont spécifiées dans une section de configuration de la journalisation dans un fichier *appsettings.json* ou *appsettings.{Environment}.json*.
* Active [l’intégration IIS](xref:host-and-deploy/iis/index) quand il est exécuté derrière IIS. Configure le chemin de base et le port écouté par le serveur lors de l’utilisation du [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). Le module crée un proxy inverse entre IIS et Kestrel. Il configure également l’application pour la [capture des erreurs de démarrage](#capture-startup-errors). Pour connaître les options IIS par défaut, consultez la [section sur les options IIS dans Héberger ASP.NET Core sur Windows avec IIS](xref:host-and-deploy/iis/index#iis-options).

La *racine de contenu* détermine l’emplacement où l’hôte recherche les fichiers de contenu, tels que les fichiers de vue MVC. Quand l’application est démarrée à partir du dossier racine du projet, ce dossier est utilisé comme racine de contenu. Il s’agit du dossier par défaut utilisé dans [Visual Studio](https://www.visualstudio.com/) et les [modèles dotnet new](/dotnet/core/tools/dotnet-new).

Pour plus d’informations sur la configuration d’une application, consultez [Configuration dans ASP.NET Core](xref:fundamentals/configuration/index).

> [!NOTE]
> Au lieu d’utiliser la méthode statique `CreateDefaultBuilder`, vous pouvez aussi créer un hôte à partir de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Cette approche est prise en charge dans ASP.NET Core 2.x. Pour plus d’informations, consultez l’onglet ASP.NET Core 1.x.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Créez un hôte en utilisant une instance de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Cette opération est généralement effectuée au point d’entrée de l’application, à savoir la méthode `Main`. Dans les modèles de projet, `Main` se trouve dans *Program.cs* :

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

`WebHostBuilder` nécessite un serveur [ qui implémente IServer](servers/index.md). Les serveurs intégrés sont [Kestrel](servers/kestrel.md) et [HTTP.sys](servers/httpsys.md) (dans les versions antérieures à ASP.NET Core 2.0, HTTP.sys était appelé [WebListener](xref:fundamentals/servers/weblistener)). Dans cet exemple, la [méthode d’extension UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) spécifie le serveur Kestrel.

La *racine de contenu* détermine l’emplacement où l’hôte recherche les fichiers de contenu, tels que les fichiers de vue MVC. La racine de contenu par défaut pour `UseContentRoot` est retournée par [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1). Quand l’application est démarrée à partir du dossier racine du projet, ce dossier est utilisé comme racine de contenu. Il s’agit du dossier par défaut utilisé dans [Visual Studio](https://www.visualstudio.com/) et les [modèles dotnet new](/dotnet/core/tools/dotnet-new).

Pour utiliser IIS comme proxy inverse, appelez [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) dans le cadre de la création de l’hôte. `UseIISIntegration` ne configure pas un *serveur*, contrairement à ce que fait [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1). `UseIISIntegration` configure le chemin de base et le port écouté par le serveur lors de l’utilisation du [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) pour créer un proxy inverse entre Kestrel et IIS. Pour utiliser IIS avec ASP.NET Core, `UseKestrel` et `UseIISIntegration` doivent être spécifiés. `UseIISIntegration` est activé uniquement dans le cadre d’une exécution derrière IIS ou IIS Express. Pour plus d’informations, consultez [Présentation du module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) et [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

Une implémentation minimale qui configure un hôte (et une application ASP.NET Core) consiste à spécifier un serveur et la configuration du pipeline des requêtes de l’application :

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

---

Lors de la configuration d’un hôte, les méthodes [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) et [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) peuvent être fournies. Si une classe `Startup` est spécifiée, elle doit définir une méthode `Configure`. Pour plus d’informations, consultez [Démarrage de l’application dans ASP.NET Core](startup.md). Les appels multiples à `ConfigureServices` s’ajoutent les uns aux autres. Les appels multiples à `Configure` ou `UseStartup` sur `WebHostBuilder` remplacent les paramètres précédents.

## <a name="host-configuration-values"></a>Valeurs de configuration de l’hôte

[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) s’appuie sur les approches suivantes pour définir les valeurs de configuration de l’hôte :

* Configuration du générateur de l’hôte, qui inclut des variables d’environnement au format `ASPNETCORE_{configurationKey}`. Par exemple, `ASPNETCORE_URLS`.
* Méthodes explicites, telles que `CaptureStartupErrors`.
* [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) et la clé associée. Quand une valeur est définie avec `UseSetting`, elle est définie au format chaîne indépendamment du type.

L’hôte utilise l’option qui définit une valeur en dernier. Pour plus d’informations, consultez [Substitution d’une configuration](#overriding-configuration) dans la section suivante.

### <a name="capture-startup-errors"></a>Capture des erreurs de démarrage

Ce paramètre contrôle la capture des erreurs de démarrage.

**Clé** : captureStartupErrors  
**Type** : *bool* (`true` ou `1`)  
**Valeur par défaut** : `false`, ou `true` si l’application s’exécute avec Kestrel derrière IIS.  
**Définition avec** : `CaptureStartupErrors`  
**Variable d’environnement** : `ASPNETCORE_CAPTURESTARTUPERRORS`

Avec la valeur `false`, la survenue d’erreurs au démarrage entraîne la fermeture de l’hôte. Avec la valeur `true`, l’hôte capture les exceptions levées au démarrage et tente de démarrer le serveur.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a>Racine de contenu

Ce paramètre détermine l’emplacement où ASP.NET Core commence la recherche des fichiers de contenu, tels que les vues MVC. 

**Clé** : contentRoot  
**Type** : *string*  
**Valeur par défaut** : dossier où réside l’assembly de l’application.  
**Définition avec** : `UseContentRoot`  
**Variable d’environnement** : `ASPNETCORE_CONTENTROOT`

La racine de contenu est également utilisée comme chemin de base pour le [paramètre de la racine Web](#web-root). Si le chemin est introuvable, l’hôte ne peut pas démarrer.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a>Erreurs détaillées

Détermine si les erreurs détaillées doivent être capturées.

**Clé** : detailedErrors  
**Type** : *bool* (`true` ou `1`)  
**Valeur par défaut** : false  
**Définition avec** : `UseSetting`  
**Variable d’environnement** : `ASPNETCORE_DETAILEDERRORS`

Quand cette fonctionnalité est activée (ou que <a href="#environment">l’environnement</a> est défini sur `Development`), l’application capture les exceptions détaillées.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a>Environnement

Définit l’environnement de l’application.

**Clé** : environment  
**Type** : *string*  
**Valeur par défaut** : Production  
**Définition avec** : `UseEnvironment`  
**Variable d’environnement** : `ASPNETCORE_ENVIRONMENT`

L’environnement peut être défini à n’importe quelle valeur. Les valeurs définies par le framework sont `Development`, `Staging` et `Production`. Les valeurs ne respectent pas la casse. Par défaut, *l’environnement* est fourni par la variable d’environnement `ASPNETCORE_ENVIRONMENT`. Si vous utilisez [Visual Studio](https://www.visualstudio.com/), les variables d’environnement peuvent être définies dans le fichier *launchSettings.json*. Pour plus d’informations, consultez [Utilisation de plusieurs environnements](xref:fundamentals/environments).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a>Assemblys d’hébergement au démarrage

Définit les assemblys d’hébergement au démarrage de l’application.

**Clé** : hostingStartupAssemblies  
**Type** : *string*  
**Valeur par défaut** : une chaîne vide  
**Définition avec** : `UseSetting`  
**Variable d’environnement** : `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`

Chaîne délimitée par des points-virgules qui spécifie les assemblys d’hébergement à charger au démarrage. Cette fonctionnalité est nouvelle dans ASP.NET Core 2.0.

La valeur de configuration par défaut est une chaîne vide, mais les assemblys d’hébergement au démarrage incluent toujours l’assembly de l’application. Quand des assemblys d’hébergement au démarrage sont fournis, ils sont ajoutés à l’assembly de l’application et sont chargés lorsque l’application génère ses services communs au démarrage.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Cette fonctionnalité n’est pas disponible dans ASP.NET Core 1.x.

---

### <a name="prefer-hosting-urls"></a>URL d’hébergement préférées

Indique si l’hôte doit écouter les URL configurées avec `WebHostBuilder` au lieu d’écouter celles configurées avec l’implémentation `IServer`.

**Clé** : preferHostingUrls  
**Type** : *bool* (`true` ou `1`)  
**Valeur par défaut** : true  
**Définition avec** : `PreferHostingUrls`  
**Variable d’environnement** : `ASPNETCORE_PREFERHOSTINGURLS`

Cette fonctionnalité est nouvelle dans ASP.NET Core 2.0.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Cette fonctionnalité n’est pas disponible dans ASP.NET Core 1.x.

---

### <a name="prevent-hosting-startup"></a>Bloquer les assemblys d’hébergement au démarrage

Empêche le chargement automatique des assemblys d’hébergement au démarrage, y compris ceux configurés par l’assembly de l’application. Pour plus d’informations, consultez [Ajouter des fonctionnalités d’application à partir d’un assembly externe avec IHostingStartup](xref:host-and-deploy/ihostingstartup).

**Clé** : preventHostingStartup  
**Type** : *bool* (`true` ou `1`)  
**Valeur par défaut** : false  
**Définition avec** : `UseSetting`  
**Variable d’environnement** : `ASPNETCORE_PREVENTHOSTINGSTARTUP`

Cette fonctionnalité est nouvelle dans ASP.NET Core 2.0.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Cette fonctionnalité n’est pas disponible dans ASP.NET Core 1.x.

---

### <a name="server-urls"></a>URL du serveur

Indique les adresses IP ou les adresses d’hôte avec les ports et protocoles sur lesquels le serveur doit écouter les requêtes.

**Clé** : urls  
**Type** : *string*  
**Valeur par défaut** : http://localhost:5000  
**Définition avec** : `UseUrls`  
**Variable d’environnement** : `ASPNETCORE_URLS`

Liste de préfixes d’URL séparés par des points-virgules (;) correspondant aux URL auxquelles le serveur doit répondre. Par exemple, `http://localhost:123`. Utilisez « \* » pour indiquer que le serveur doit écouter les requêtes sur toutes les adresses IP ou tous les noms d’hôte qui utilisent le port et le protocole spécifiés (par exemple, `http://*:5000`). Le protocole (`http://` ou `https://`) doit être inclus avec chaque URL. Les formats pris en charge varient selon les serveurs.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

Kestrel a sa propre API de configuration de points de terminaison. Pour plus d’informations, consultez [Implémentation du serveur web Kestrel dans ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a>Délai d’arrêt

Spécifie la durée d’attente avant l’arrêt de l’hôte web.

**Clé** : shutdownTimeoutSeconds  
**Type** : *int*  
**Valeur par défaut** : 5  
**Définition avec** : `UseShutdownTimeout`  
**Variable d’environnement** : `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`

La clé accepte une valeur *int* avec `UseSetting` (par exemple, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), mais la méthode d’extension `UseShutdownTimeout` prend une valeur `TimeSpan`. Cette fonctionnalité est nouvelle dans ASP.NET Core 2.0.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Cette fonctionnalité n’est pas disponible dans ASP.NET Core 1.x.

---

### <a name="startup-assembly"></a>Assembly de démarrage

Détermine l’assembly à rechercher pour la classe `Startup`.

**Clé** : startupAssembly  
**Type** : *string*  
**Valeur par défaut** : l’assembly de l’application  
**Définition avec** : `UseStartup`  
**Variable d’environnement** : `ASPNETCORE_STARTUPASSEMBLY`

L’assembly peut être référencé par nom (`string`) ou type (`TStartup`). Si plusieurs méthodes `UseStartup` sont appelées, la dernière prévaut.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

### <a name="web-root"></a>Racine Web

Définit le chemin relatif des ressources statiques de l’application.

**Clé** : webroot  
**Type** : *string*  
**Valeur par défaut** : quand aucune valeur n’est spécifiée, la valeur par défaut est « (Racine de contenu)/wwwroot », si ce chemin existe. Si ce chemin est introuvable, un fournisseur de fichiers no-op est utilisé.  
**Définition avec** : `UseWebRoot`  
**Variable d’environnement** : `ASPNETCORE_WEBROOT`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a>Substitution d’une configuration

Utilisez [Configuration](xref:fundamentals/configuration/index) pour configurer l’hôte. Dans l’exemple suivant, la configuration de l’hôte est facultativement spécifiée dans un fichier *hosting.json*. Une configuration chargée à partir du fichier *hosting.json* peut être remplacée par des arguments de ligne de commande. La configuration intégrée (dans `config`) est utilisée pour configurer l’hôte avec `UseConfiguration`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

*hosting.json* :

```json
{
    urls: "http://*:5005"
}
```

La configuration fournie par `UseUrls` est d’abord remplacée par la configuration du fichier *hosting.json* et ensuite par la configuration des arguments de ligne de commande :

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

*hosting.json* :

```json
{
    urls: "http://*:5005"
}
```

La configuration fournie par `UseUrls` est d’abord remplacée par la configuration du fichier *hosting.json* et ensuite par la configuration des arguments de ligne de commande :

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
> La méthode d’extension [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) ne peut pas analyser une section de configuration retournée par `GetSection` (par exemple, `.UseConfiguration(Configuration.GetSection("section"))`. La méthode `GetSection` filtre les clés de configuration dans la section demandée, mais laisse le nom de la section dans les clés (par exemple, `section:urls`, `section:environment`). La méthode `UseConfiguration` suppose que les clés correspondent aux clés `WebHostBuilder` (par exemple, `urls`, `environment`). La présence du nom de la section dans les clés empêche l’utilisation des valeurs de la section pour configurer l’hôte. Ce problème sera résolu dans une prochaine mise en production. Pour obtenir plus d’informations et connaître les solutions de contournement possibles, consultez [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).

Pour spécifier l’exécution de l’hôte sur une URL particulière, vous pouvez passer la valeur souhaitée à partir d’une invite de commandes au moment de l’exécution de `dotnet run`. L’argument de ligne de commande se substitue à la valeur `urls` fournie par le fichier *hosting.json*, et le serveur écoute le port 8080 :

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a>Démarrage de l’hôte

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**Run**

La méthode `Run` démarre l’application web et bloque le thread appelant jusqu’à l’arrêt de l’hôte :

```csharp
host.Run();
```

**Start**

Appelez la méthode `Start` pour exécuter l’hôte en mode non bloquant :

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Si une liste d’URL est passée à la méthode `Start`, l’hôte écoute les URL spécifiées :

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

L’application peut initialiser et démarrer un nouvel hôte ayant les valeurs par défaut préconfigurées de `CreateDefaultBuilder` en utilisant une méthode d’usage statique. Ces méthodes démarrent le serveur sans sortie de console et, avec [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown), elles attendent un arrêt (Ctrl-C/SIGINT ou SIGTERM) :

**Start(RequestDelegate app)**

Commencez par un `RequestDelegate` :

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Envoyez une requête à `http://localhost:5000` dans le navigateur pour recevoir la réponse « Hello World! » `WaitForShutdown` bloque la requête jusqu’à l’émission d’une commande d’arrêt (Ctrl-C/SIGINT ou SIGTERM). L’application affiche le message `Console.WriteLine` et attend que l’utilisateur appuie sur une touche pour s’arrêter.

**Start(string url, RequestDelegate app)**

Commencez par une URL et `RequestDelegate` :

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Produit le même résultat que **Start(RequestDelegate app)**, sauf que l’application répond sur `http://localhost:8080`.

**Start(Action<IRouteBuilder> routeBuilder)**

Utilisez une instance de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) pour utiliser le middleware de routage :

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

Utilisez les requêtes de navigateur suivantes avec l’exemple :

| Demande                                    | Réponse                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | Hello, Martin!                           |
| `http://localhost:5000/buenosdias/Catrina` | Buenos dias, Catrina!                    |
| `http://localhost:5000/throw/ooops!`       | Lève une exception avec la chaîne « ooops! » |
| `http://localhost:5000/throw`              | Lève une exception avec la chaîne « Uh oh! » |
| `http://localhost:5000/Sante/Kevin`        | Sante, Kevin!                            |
| `http://localhost:5000`                    | Hello World!                             |

`WaitForShutdown` bloque la requête jusqu’à l’émission d’une commande d’arrêt (Ctrl-C/SIGINT ou SIGTERM). L’application affiche le message `Console.WriteLine` et attend que l’utilisateur appuie sur une touche pour s’arrêter.

**Start(string url, Action<IRouteBuilder> routeBuilder)**

Utilisez une URL et une instance de `IRouteBuilder` :

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

Produit le même résultat que **Start(Action<IRouteBuilder> routeBuilder)**, sauf que l’application répond sur `http://localhost:8080`.

**StartWith(Action<IApplicationBuilder> app)**

Fournissez un délégué pour configurer un `IApplicationBuilder` :

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

Envoyez une requête à `http://localhost:5000` dans le navigateur pour recevoir la réponse « Hello World! » `WaitForShutdown` bloque la requête jusqu’à l’émission d’une commande d’arrêt (Ctrl-C/SIGINT ou SIGTERM). L’application affiche le message `Console.WriteLine` et attend que l’utilisateur appuie sur une touche pour s’arrêter.

**StartWith(string url, Action<IApplicationBuilder> app)**

Fournissez une URL et un délégué pour configurer un `IApplicationBuilder` :

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

Produit le même résultat que **StartWith(Action<IApplicationBuilder> app)**, sauf que l’application répond sur `http://localhost:8080`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**Run**

La méthode `Run` démarre l’application web et bloque le thread appelant jusqu’à l’arrêt de l’hôte :

```csharp
host.Run();
```

**Start**

Appelez la méthode `Start` pour exécuter l’hôte en mode non bloquant :

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Si une liste d’URL est passée à la méthode `Start`, l’hôte écoute les URL spécifiées :


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

## <a name="ihostingenvironment-interface"></a>Interface IHostingEnvironment

[L’interface IHostingEnvironment](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) fournit des informations sur l’environnement d’hébergement web de l’application. Utilisez [l’injection de constructeur](xref:fundamentals/dependency-injection) pour obtenir l’interface `IHostingEnvironment` afin d’utiliser ses propriétés et méthodes d’extension :

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

Vous pouvez utiliser une [approche basée sur une convention](xref:fundamentals/environments#startup-conventions) pour configurer l’application au démarrage en fonction de l’environnement. Vous pouvez également injecter `IHostingEnvironment` dans le constructeur `Startup` pour l’utiliser dans `ConfigureServices` :

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
> En plus de la méthode d’extension `IsDevelopment`, `IHostingEnvironment` fournit les méthodes `IsStaging`, `IsProduction` et `IsEnvironment(string environmentName)`. Pour plus d’informations, consultez [Utilisation de plusieurs environnements](xref:fundamentals/environments).

Le service `IHostingEnvironment` peut également être injecté directement dans la méthode `Configure` pour configurer le pipeline de traitement :

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

`IHostingEnvironment` peut être injecté dans la méthode `Invoke` lors de la création d’un [intergiciel (middleware)](xref:fundamentals/middleware#writing-middleware) personnalisé :

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

## <a name="iapplicationlifetime-interface"></a>Interface IApplicationLifetime

[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) s’utilise pour les opérations post-démarrage et arrêt. Trois propriétés de l’interface sont des jetons d’annulation utilisés pour inscrire les méthodes `Action` qui définissent les événements de démarrage et d’arrêt. L’interface fournit aussi la méthode `StopApplication`.

| Jeton d’annulation    | Événement déclencheur |
| --------------------- | --------------------- |
| `ApplicationStarted`  | L’hôte a complètement démarré. |
| `ApplicationStopping` | L’hôte effectue un arrêt approprié. Des requêtes peuvent encore être en cours de traitement. L’arrêt est bloqué tant que cet événement n’est pas terminé. |
| `ApplicationStopped`  | L’hôte effectue un arrêt approprié. Toutes les requêtes doivent être traitées. L’arrêt est bloqué tant que cet événement n’est pas terminé. |

| Méthode            | Action                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | Demande l’arrêt de l’application active. |

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

## <a name="troubleshooting-systemargumentexception"></a>Dépannage de System.ArgumentException

**S’applique à ASP.NET Core 2.0 uniquement**

Vous pouvez créer un hôte en injectant `IStartup` directement dans le conteneur d’injection de dépendances au lieu d’appeler `UseStartup` ou `Configure` :

```csharp
services.AddSingleton<IStartup, Startup>();
```

Dans ce cas, l’erreur suivante peut se produire :

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

Cette erreur se produit, car [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (l’assembly actuel) est requis pour analyser `HostingStartupAttributes`. Si l’application injecte `IStartup` manuellement dans le conteneur d’injection de dépendances, ajoutez l’appel suivant à `WebHostBuilder` en spécifiant le nom de l’assembly :

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

Vous pouvez également ajouter un `Configure` factice à `WebHostBuilder`, qui définit automatiquement la valeur `applicationName`(`ApplicationKey`) :

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

**REMARQUE** : Cela est nécessaire uniquement dans ASP.NET Core 2.0 et si l’application n’appelle pas `UseStartup` ou `Configure`.

Pour plus d’informations, consultez le commentaire [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) et [l’exemple StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).

## <a name="additional-resources"></a>Ressources supplémentaires

* [Héberger sur Windows avec IIS](xref:host-and-deploy/iis/index)
* [Héberger sur Linux avec Nginx](xref:host-and-deploy/linux-nginx)
* [Héberger sur Linux avec Apache](xref:host-and-deploy/linux-apache)
* [Héberger dans un service Windows](xref:host-and-deploy/windows-service)
