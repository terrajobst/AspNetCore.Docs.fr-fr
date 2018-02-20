---
title: "Démarrage d’une application dans ASP.NET Core"
author: ardalis
description: "Découvrez comment la classe Startup dans ASP.NET Core configure des services et le pipeline de requête de l’application."
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/08/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/startup
ms.openlocfilehash: c324918b33af82b619bb2251f32308e4a57c27e5
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2018
---
# <a name="application-startup-in-aspnet-core"></a>Démarrage d’une application dans ASP.NET Core

Par [Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra) et [Luke Latham](https://github.com/guardrex)

La classe `Startup` configure les services et le pipeline de demande de l’application.

## <a name="the-startup-class"></a>Classe Startup

Les applications ASP.NET Core utilisent une classe de `Startup`, qui est nommée `Startup` par convention. La classe `Startup` :

* Peut éventuellement inclure une méthode [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) permettant de configurer les services de l’application.
* Doit inclure une méthode [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) pour créer le pipeline de traitement de demande de l’application.

`ConfigureServices` et `Configure` sont appelées par le runtime au démarrage de l’application :

[!code-csharp[Main](startup/snapshot_sample/Startup1.cs)]

Spécifiez la classe `Startup` avec la méthode [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) :

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

Le constructeur de la classe `Startup` accepte les dépendances définies par l’hôte. Une utilisation courante de [l’injection de dépendances](xref:fundamentals/dependency-injection) dans la classe `Startup` consiste à injecter :

* [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) pour configurer les services par environnement.
* [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) pour configurer l’application au démarrage.

[!code-csharp[Main](startup/snapshot_sample/Startup2.cs)]

Une alternative à l’injection de `IHostingEnvironment` consiste à utiliser une approche basée sur les conventions. L’application peut définir différentes classes `Startup` pour différents environnements (par exemple, `StartupDevelopment`), et la classe de démarrage appropriée est sélectionnée lors de l’exécution. La classe dont le suffixe du nom correspond à l'environnement actuel est prioritaire. Si l’application est exécutée dans l’environnement de développement et comprend à la fois une classe `Startup` et une classe `StartupDevelopment`, la classe `StartupDevelopment` est utilisée. Pour plus d’informations, consultez [Utilisation de plusieurs environnements](xref:fundamentals/environments#startup-conventions).

Pour en savoir plus sur `WebHostBuilder`, consultez la rubrique [Hébergement](xref:fundamentals/hosting). Pour plus d’informations sur la gestion des erreurs lors du démarrage, consultez [Gestion des exceptions de démarrage](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Méthode ConfigureServices

La méthode [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) est :

* Facultative.
* Appelée par l’hôte web avant la méthode `Configure` pour configurer les services de l’application.
* L’emplacement où les [options de configuration](xref:fundamentals/configuration/index) sont définies par convention.

L'ajout de services au conteneur de service les rend disponibles au sein de l’application et dans la méthode `Configure`. Les services ne sont pas résolus via [l’injection de dépendances](xref:fundamentals/dependency-injection) ou à partir de [d’IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).

L’hôte web peut configurer certains services avant que les méthodes `Startup` soient appelées. Vous trouverez plus de détails dans la rubrique [Hébergement](xref:fundamentals/hosting). 

Pour les fonctionnalités qui nécessitent une installation substantielle, il existe des méthodes d’extension `Add[Service]` sur [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Une application web classique inscrit des services pour Entity Framework, Identity et MVC :

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="services-available-in-startup"></a>Services disponibles dans Startup

L’hôte web fournit des services disponibles pour le constructeur de classe `Startup`. L’application ajoute des services supplémentaires à l’aide de `ConfigureServices`. Les services de l’hôte ainsi que ceux de l’application sont alors disponibles dans `Configure` et dans l’ensemble de l’application.

## <a name="the-configure-method"></a>Méthode Configure

La méthode [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) est utilisée pour spécifier la façon dont l’application répond aux requêtes HTTP. Le pipeline de requête est configuré en ajoutant des composants [d’intergiciel (middleware)](xref:fundamentals/middleware/index) à une instance [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder). `IApplicationBuilder` est disponible pour la méthode `Configure`, mais elle n’est pas inscrite dans le conteneur de service. L’hébergement crée un `IApplicationBuilder` et le passe directement à `Configure` ([source de référence](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).

Les [modèles ASP.NET Core](/dotnet/core/tools/dotnet-new) configurent le pipeline avec la prise en charge d’une page d’exception de développeur, de [BrowserLink](http://vswebessentials.com/features/browserlink), des pages d’erreurs, des fichiers statiques et d’ASP.NET MVC :

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Chaque méthode d’extension `Use` ajoute un composant d’intergiciel au pipeline de requête. Par exemple, la méthode d’extension `UseMvc` ajoute [l’intergiciel (middleware) de routage](xref:fundamentals/routing) au pipeline de requête et configure [MVC](xref:mvc/overview) comme gestionnaire par défaut.

Il est également possible de spécifier des services supplémentaires, comme `IHostingEnvironment` et `ILoggerFactory`, dans la signature de méthode. Si tel est le cas, les services supplémentaires sont injectés s’ils sont disponibles.

Pour plus d’informations sur l’utilisation d’`IApplicationBuilder`, consultez [Intergiciel (middleware)](xref:fundamentals/middleware/index).

## <a name="convenience-methods"></a>Méthodes pratiques

Les méthodes pratiques [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) et [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) peuvent être utilisées au lieu de spécifier une classe `Startup`. Les appels multiples à `ConfigureServices` s’ajoutent les uns aux autres. Les appels multiples à `Configure` utilisent le dernier appel à la méthode.

[!code-csharp[Main](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="startup-filters"></a>Filtres Startup

Utilisez [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) pour configurer un intergiciel au début ou à la fin du pipeline de l’intergiciel de la méthode [Configure](#the-configure-method) d’une application. `IStartupFilter` est utile pour s’assurer qu’un intergiciel s’exécute avant ou après l’intergiciel ajouté par des bibliothèques au début ou à la fin du pipeline de traitement de requête de l’application.

`IStartupFilter` implémente une méthode unique, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), qui reçoit et retourne un `Action<IApplicationBuilder>`. Un [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) définit une classe pour configurer le pipeline de requête d’une application. Pour plus d’informations, consultez [Création d’un pipeline d’intergiciel (middleware) avec IApplicationBuilder](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder).

Chaque `IStartupFilter` implémente un ou plusieurs intergiciels dans le pipeline de requête. Les filtres sont appelés dans l’ordre dans lequel ils ont été ajoutés au conteneur de service. Les filtres peuvent ajouter l’intergiciel avant ou après la transmission du contrôle au filtre suivant. Par conséquent, ils s’ajoutent au début ou à la fin du pipeline de l’application.

[L’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample)) montre comment inscrire un intergiciel avec `IStartupFilter`. L’exemple d’application inclut un intergiciel qui définit une valeur d’options à partir d’un paramètre de chaîne de requête :

[!code-csharp[Main](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` est configuré dans la classe `RequestSetOptionsStartupFilter` :

[!code-csharp[Main](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` est inscrit dans le conteneur de service dans `ConfigureServices` :

[!code-csharp[Main](startup/sample/Startup.cs?name=snippet1&highlight=3)]

Quand un paramètre de chaîne de requête pour `option` est fourni, l’intergiciel traite l’affectation de valeur avant que l’intergiciel MVC affiche la réponse :

![Fenêtre de navigateur affichant la page d’index rendue. La valeur d’Option rendue est « From Middleware » en fonction de la demande de la page avec le paramètre de chaîne de requête et la valeur d’option définie sur « From Middleware ».](startup/_static/index.png)

L’ordre d’exécution de l’intergiciel est défini par l’ordre des inscriptions d’`IStartupFilter` :

* Plusieurs implémentations d’`IStartupFilter` peuvent interagir avec les mêmes objets. Si l’ordre est important, définissez l’ordre des inscriptions de leurs services `IStartupFilter` pour qu’il corresponde à l’ordre dans lequel leurs intergiciels doivent s’exécuter.
* Les bibliothèques peuvent ajouter des intergiciels avec une ou plusieurs implémentations `IStartupFilter` qui s’exécutent avant ou après l’autre intergiciel d’application inscrit avec `IStartupFilter`. Pour appeler un intergiciel `IStartupFilter` avant un intergiciel ajouté à l’`IStartupFilter` d’une bibliothèque, placez l’inscription du service avant l’ajout de la bibliothèque au conteneur de service. Pour l’appeler par la suite, placez l’inscription du service après l’ajout de la bibliothèque.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Hébergement d’applications WPF](xref:fundamentals/hosting)
* [Utilisation de plusieurs environnements](xref:fundamentals/environments)
* [Intergiciel (middleware)](xref:fundamentals/middleware/index)
* [Journalisation](xref:fundamentals/logging/index)
* [Configuration](xref:fundamentals/configuration/index)
* [Classe StartupLoader : méthode FindStartupType (source de référence)](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
