---
title: Utiliser plusieurs environnements dans ASP.NET Core
author: rick-anderson
description: Découvrez comment contrôler le comportement de l’application dans différents environnements dans les applications ASP.NET Core.
ms.author: riande
ms.date: 07/03/2018
uid: fundamentals/environments
ms.openlocfilehash: 8983a0ce81beb16d68c799d30bfbfce6e7b693b1
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433946"
---
# <a name="use-multiple-environments-in-aspnet-core"></a>Utiliser plusieurs environnements dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core configure le comportement de l’application en fonction de l’environnement d’exécution à l’aide d’une variable d’environnement.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>Environnements

ASP.NET Core lit la variable d’environnement `ASPNETCORE_ENVIRONMENT` au démarrage de l’application et stocke la valeur dans [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname). Vous pouvez affecter n’importe quelle valeur à `ASPNETCORE_ENVIRONMENT`, mais [trois valeurs](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) sont prises en charge par le framework : [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging) et [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production). Si `ASPNETCORE_ENVIRONMENT` n’est pas définie, la valeur par défaut est `Production`.

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

Le code précédent :

* Appelle [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) et [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) quand `ASPNETCORE_ENVIRONMENT` a la valeur `Development`.
* Appelle [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) quand `ASPNETCORE_ENVIRONMENT` est définie sur l’une des valeurs :

    * `Staging`
    * `Production`
    * `Staging_2`

Le [Tag helper d’environnement](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) utilise la valeur de la propriété `IHostingEnvironment.EnvironmentName` pour inclure ou exclure le balisage dans l’élément :

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

Sur Windows et macOS, les valeurs et les variables d’environnement ne respectent pas la casse. Les valeurs et les variables d’environnement Linux **respectent la casse** par défaut.

### <a name="development"></a>Développement

L’environnement de développement peut activer des fonctionnalités qui ne doivent pas être exposées en production. Par exemple, les modèles ASP.NET Core activent la [page d’exception de développeur](xref:fundamentals/error-handling#the-developer-exception-page) dans l’environnement de développement.

L’environnement de développement de l’ordinateur local peut être défini dans le fichier *Properties\launchSettings.json* du projet. Les valeurs d’environnement définies dans *launchSettings.json* remplacent les valeurs définies dans l’environnement système.

Le code JSON suivant montre trois profils à partir d’un fichier *launchSettings.json* :

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:54339/",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      }
    },
    "EnvironmentsSample": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:54340/"
    },
    "Kestrel Staging": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:51997/"
    }
  }
}
```

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> La propriété `applicationUrl` dans *launchSettings.json* peut spécifier une liste d’URL de serveur. Utilisez un point-virgule entre les URL de la liste :
>
> ```json
> "EnvironmentsSample": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```

::: moniker-end

Quand l’application est lancée avec [dotnet run](/dotnet/core/tools/dotnet-run), le premier profil avec `"commandName": "Project"` est utilisé. La valeur de `commandName` spécifie le serveur web à lancer. `commandName` peut avoir l’une des valeurs suivantes :

* IIS Express
* IIS
* Project (qui lance Kestrel)

Quand une application est lancée avec [dotnet run](/dotnet/core/tools/dotnet-run) :

* *launchSettings.json* est lu, s’il est disponible. Les paramètres `environmentVariables` dans *launchSettings.json* remplacent les variables d’environnement.
* L’environnement d’hébergement s’affiche.

La sortie suivante montre une application démarrée avec [dotnet run](/dotnet/core/tools/dotnet-run) :

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

L’onglet **Déboguer** des propriétés de projet Visual Studio fournit une interface graphique utilisateur qui permet de modifier le fichier *launchSettings.json* :

![Propriétés de projet, définition des variables d’environnement](environments/_static/project-properties-debug.png)

Les modifications apportées aux profils de projet peuvent ne prendre effet qu’une fois le serveur web redémarré. Vous devez redémarrer Kestrel pour qu’il puisse détecter les modifications apportées à son environnement.

> [!WARNING]
> *launchSettings.json* ne doit pas stocker de secrets. Vous pouvez utiliser [l’outil Secret Manager](xref:security/app-secrets) afin de stocker des secrets pour le développement local.

Quand vous utilisez [Visual Studio Code](https://code.visualstudio.com/), les variables d’environnement peuvent être définies dans le fichier *.vscode/launch.json*. L’exemple suivant définit `Development` comme environnement :

```json
{
   "version": "0.2.0",
   "configurations": [
        {
            "name": ".NET Core Launch (web)",

            ... additional VS Code configuration settings ...

            "env": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    ]
}
```

Un fichier *.vscode/launch.json* dans le projet n’est pas lu au démarrage de l’application avec `dotnet run` de la même façon que *Properties/launchSettings.json*. Lors du lancement d’une application en cours de développement qui n’a pas de fichier *launchSettings.json*, vous devez définir l’environnement avec une variable d’environnement ou un argument de ligne de commande pour la commande `dotnet run`.

### <a name="production"></a>Production

Vous devez configurer l’environnement de production pour optimiser la sécurité, les performances et la robustesse de l’application. Voici quelques paramètres courants qui diffèrent du développement :

* La mise en cache.
* Les ressources côté client sont groupées, réduites et éventuellement servies à partir d’un CDN.
* Les Pages d’erreur de diagnostic sont désactivées.
* Les pages d’erreur conviviales sont activées.
* La journalisation et la surveillance de la production sont activées. Par exemple, [Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="set-the-environment"></a>Définir l’environnement

Il est souvent utile de définir un environnement spécifique à des fins de test. Si l’environnement n’est pas défini, il prend par défaut la valeur `Production`, ce qui désactive la plupart des fonctionnalités de débogage. La méthode de configuration de l’environnement dépend du système d’exploitation.

### <a name="azure-app-service"></a>Azure App Service

Pour définir l’environnement dans [Azure App Service](https://azure.microsoft.com/services/app-service/), effectuez les étapes suivantes :

1. Sélectionnez l’application dans le panneau **App Services**.
1. Dans le groupe **PARAMÈTRES**, sélectionnez le panneau **Paramètres de l’application**.
1. Dans la zone **Paramètres d’application**, sélectionnez **Ajouter un nouveau paramètre**.
1. Pour **Entrer un nom**, spécifiez `ASPNETCORE_ENVIRONMENT`. Pour **Entrer une valeur**, spécifiez l’environnement (par exemple `Staging`).
1. Cochez la case **Paramètre d’emplacement** si vous souhaitez que le paramètre d’environnement reste avec l’emplacement actuel quand des emplacements de déploiement sont permutés. Pour plus d’informations, consultez [Documentation Azure : Quels sont les paramètres qui sont permutés ?](/azure/app-service/web-sites-staged-publishing).
1. Sélectionnez **Enregistrer** en haut du panneau.

Azure App Service redémarre automatiquement l’application après qu’un paramètre d’application (variable d’environnement) est ajouté, changé ou supprimé dans le portail Azure.

### <a name="windows"></a>Windows

Pour définir `ASPNETCORE_ENVIRONMENT` pour la session actuelle quand l’application est démarrée avec [dotnet run](/dotnet/core/tools/dotnet-run), les commandes suivantes sont utilisées :

**Invite de commandes**

```console
set ASPNETCORE_ENVIRONMENT=Development
```

**PowerShell**

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Ces commandes prennent effet uniquement pour la fenêtre active. Quand la fenêtre est fermée, le paramètre `ASPNETCORE_ENVIRONMENT` reprend la valeur de machine ou la valeur par défaut. Pour définir la valeur globalement dans Windows, ouvrez le **Panneau de configuration** > **Système** > **Paramètres système avancés** et ajoutez ou modifiez la valeur `ASPNETCORE_ENVIRONMENT` :

![Propriétés système avancées](environments/_static/systemsetting_environment.png)

![Variable d’environnement ASPNET Core](environments/_static/windows_aspnetcore_environment.png)

**web.config**

Consultez la section *Définition des variables d’environnement* de la rubrique [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

**Par pool d’applications IIS**

Pour définir des variables d’environnement pour des applications qui s’exécutent dans des pools d’applications isolés (prise en charge sur IIS 10.0+), consultez la section *Commande AppCmd.exe* de la rubrique [Variables d’environnement &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).

### <a name="macos"></a>macOS

Vous pouvez définir l’environnement actuel pour macOS en ligne durant l’exécution de l’application :

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

Vous pouvez également définir l’environnement avec `export` avant d’exécuter l’application :

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

Les variables d’environnement de niveau machine sont définies dans le fichier *.bashrc* ou *.bash_profile*. Modifiez le fichier à l’aide d’un éditeur de texte. Ajoutez l’instruction suivante :

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux

Pour les versions Linux, exécutez la commande `export` à une invite de commandes pour les paramètres de variable basés sur la session, et le fichier *bash_profile* pour les paramètres d’environnement de niveau machine.

### <a name="configuration-by-environment"></a>Configuration par environnement

Pour plus d’informations, consultez [Configuration par environnement](xref:fundamentals/configuration/index#configuration-by-environment).

## <a name="environment-based-startup-class-and-methods"></a>Classe et méthodes Startup en fonction de l’environnement

Quand une application ASP.NET Core démarre, la [classe Startup](xref:fundamentals/startup) amorce l’application. Si une classe `Startup{EnvironmentName}` existe, elle est appelée pour cet `EnvironmentName` :

[!code-csharp[](environments/sample/EnvironmentsSample/StartupDev.cs?name=snippet&highlight=1)]

[WebHostBuilder.UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) remplace les sections de configuration.

[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) et [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) prennent en charge les versions propres à l’environnement de la forme `Configure{EnvironmentName}` et `Configure{EnvironmentName}Services` :

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,51)]

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
* [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
