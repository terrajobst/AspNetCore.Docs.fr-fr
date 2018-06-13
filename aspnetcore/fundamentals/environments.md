---
title: Utiliser plusieurs environnements dans ASP.NET Core
author: rick-anderson
description: Découvrez dans quelle mesure ASP.NET Core permet de contrôler le comportement d’une application entre différents environnements.
manager: wpickett
ms.author: riande
ms.date: 12/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/environments
ms.openlocfilehash: 2c8441db527203aeea516073dae3bc335c335565
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33840955"
---
# <a name="use-multiple-environments-in-aspnet-core"></a>Utiliser plusieurs environnements dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core prend en charge la définition du comportement d’une application au moment de l’exécution avec des variables d’environnement.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>Environnements

ASP.NET Core lit la variable d’environnement `ASPNETCORE_ENVIRONMENT` au démarrage de l’application et stocke cette valeur dans [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName). `ASPNETCORE_ENVIRONMENT` peut être définie sur n’importe quelle valeur, mais [trois valeurs](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) sont prises en charge par le framework : [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0) et [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0). Si `ASPNETCORE_ENVIRONMENT` n’est pas définie, elle prend par défaut la valeur `Production`.

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

Le code précédent :

* Appelle [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) et [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quand `ASPNETCORE_ENVIRONMENT` a la valeur `Development`.
* Appelle [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quand `ASPNETCORE_ENVIRONMENT` est définie sur l’une des valeurs :

    * `Staging`
    * `Production`
    * `Staging_2`

Le [Tag helper d’environnement](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) utilise la valeur de la propriété `IHostingEnvironment.EnvironmentName` pour inclure ou exclure du balisage dans l’élément :

[!code-html[](environments/sample/WebApp1/Pages/About.cshtml)]

Remarque : Sur Windows et macOS, les valeurs et les variables d’environnement ne respectent pas la casse. Les valeurs et les variables d’environnement Linux **respectent la casse** par défaut.

### <a name="development"></a>Développement

L’environnement de développement peut activer des fonctionnalités qui ne doivent pas être exposées en production. Par exemple, les modèles ASP.NET Core activent la [page d’exception de développeur](xref:fundamentals/error-handling#the-developer-exception-page) dans l’environnement de développement.

L’environnement de développement de l’ordinateur local peut être défini dans le fichier *Properties\launchSettings.json* du projet. Les valeurs d’environnement définies dans *launchSettings.json* remplacent les valeurs définies dans l’environnement système.

Le code JSON suivant montre trois profils à partir d’un fichier *launchSettings.json* :

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

::: moniker range=">= aspnetcore-2.1"
> [!NOTE]
> La propriété `applicationUrl` dans *launchSettings.json* peut spécifier une liste d’URL de serveur. Utilisez un point-virgule entre les URL de la liste :
>
> ```json
> "WebApplication1": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```
::: moniker-end

Quand l’application est lancée avec [dotnet run](/dotnet/core/tools/dotnet-run), le premier profil avec `"commandName": "Project"` est utilisé. La valeur de `commandName` spécifie le serveur web à lancer. `commandName` peut avoir l’une des valeurs suivantes :

* IIS Express
* IIS
* Project (qui lance Kestrel)

Quand une application est lancée avec [dotnet run](/dotnet/core/tools/dotnet-run) :

* *launchSettings.json* est lu, s’il est disponible. Les paramètres `environmentVariables` dans *launchSettings.json* remplacent les variables d’environnement.
* L’environnement d’hébergement s’affiche.


La sortie suivante montre une application démarrée avec [dotnet run](/dotnet/core/tools/dotnet-run) :
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

L’onglet **Déboguer** de Visual Studio fournit une interface graphique utilisateur qui permet de modifier le fichier *launchSettings.json* :

![Propriétés de projet, définition des variables d’environnement](environments/_static/project-properties-debug.png)

Les modifications apportées aux profils de projet peuvent ne prendre effet qu’une fois le serveur web redémarré. Kestrel doit être redémarré pour qu’il puisse détecter les modifications apportées à son environnement.

>[!WARNING]
> *launchSettings.json* ne doit pas stocker de secrets. Vous pouvez utiliser [l’outil Secret Manager](xref:security/app-secrets) afin de stocker des secrets pour le développement local.

### <a name="production"></a>Production

Vous devez configurer l’environnement de production pour optimiser la sécurité, les performances et la robustesse de l’application. Voici quelques paramètres courants qui diffèrent du développement :

* La mise en cache.
* Les ressources côté client sont groupées, réduites et éventuellement servies à partir d’un CDN.
* Les Pages d’erreur de diagnostic sont désactivées.
* Les pages d’erreur conviviales sont activées.
* La journalisation et la surveillance de la production sont activées. Par exemple, [Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="setting-the-environment"></a>Définition de l’environnement

Il est souvent utile de définir un environnement spécifique à des fins de test. Si l’environnement n’est pas défini, il prend par défaut la valeur `Production` qui désactive la plupart des fonctionnalités de débogage.

La méthode de configuration de l’environnement dépend du système d’exploitation.

### <a name="azure"></a>Azure

Pour Azure App Service :

* Sélectionnez le panneau **Paramètres de l’application**.
* Ajoutez la clé et la valeur dans **Paramètres de l’application**.


### <a name="windows"></a>Windows
Pour définir `ASPNETCORE_ENVIRONMENT` pour la session actuelle, si l’application est démarrée avec [dotnet run](/dotnet/core/tools/dotnet-run), les commandes suivantes sont utilisées :

**Ligne de commande**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Ces commandes prennent effet uniquement pour la fenêtre active. Quand la fenêtre est fermée, le paramètre ASPNETCORE_ENVIRONMENT rétablit la valeur de machine ou le paramètre par défaut. Pour définir la valeur globalement à l’ouverture de Windows, ouvrez le **Panneau de configuration** > **Système** > **Paramètres système avancés** et ajoutez ou modifiez la valeur `ASPNETCORE_ENVIRONMENT`.

![Propriétés système avancées](environments/_static/systemsetting_environment.png)

![Variable d’environnement ASPNET Core](environments/_static/windows_aspnetcore_environment.png)


**web.config**

Consultez la section *Définition des variables d’environnement* de la rubrique [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

**Par pool d’applications IIS**

Pour définir des variables d’environnement pour des applications qui s’exécutent dans des pools d’applications isolés (prise en charge sur IIS 10.0+), consultez la section *Commande AppCmd.exe* de la rubrique [Variables d’environnement \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).

### <a name="macos"></a>macOS
Vous pouvez définir l’environnement actuel pour macOS en ligne à l’exécution de l’application ;

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
ou vous pouvez utiliser `export` pour le définir avant d’exécuter l’application.

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
Les variables d’environnement de niveau machine sont définies dans le fichier *.bashrc* ou *.bash_profile*. Modifiez le fichier à l’aide de n’importe quel éditeur de texte et ajoutez l’instruction suivante.

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux
Pour les versions Linux, utilisez la commande `export` depuis la ligne de commande pour les paramètres de variable basés sur la session, et le fichier *bash_profile* pour les paramètres d’environnement de niveau machine.

### <a name="configuration-by-environment"></a>Configuration par environnement

Pour plus d’informations, consultez [Configuration par environnement](xref:fundamentals/configuration/index#configuration-by-environment).

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a>Classe et méthodes Startup en fonction de l’environnement

Quand une application ASP.NET Core démarre, la [classe Startup](xref:fundamentals/startup) amorce l’application. Si une classe `Startup{EnvironmentName}` existe, elle est appelée pour cet `EnvironmentName` :

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

Remarque : Appeler [WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) remplace les sections de configuration.

[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) et [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) prennent en charge des versions d’environnement spécifiques de la forme `Configure{EnvironmentName}` et `Configure{EnvironmentName}Services` :

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a>Ressources supplémentaires

* [Démarrage d’une application](xref:fundamentals/startup)
* [Configuration](xref:fundamentals/configuration/index)
* [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
