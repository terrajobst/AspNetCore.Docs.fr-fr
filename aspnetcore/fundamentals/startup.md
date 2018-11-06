---
title: Démarrage d’une application dans ASP.NET Core
author: ardalis
description: Explique comment la classe Startup dans ASP.NET Core configure des services et le pipeline de requête de l’application.
ms.author: tdykstra
ms.custom: mvc
ms.date: 4/13/2018
uid: fundamentals/startup
ms.openlocfilehash: 2212344cb3c651714e8c520b096ab0c4eaf5a180
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206454"
---
# <a name="app-startup-in-aspnet-core"></a>Démarrage d’une application dans ASP.NET Core

Par [Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra) et [Luke Latham](https://github.com/guardrex)

La classe `Startup` configure les services et le pipeline de demande de l’application.

[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).

## <a name="the-startup-class"></a>Classe Startup

Les applications ASP.NET Core utilisent une classe `Startup`, appelée `Startup` par convention. La classe `Startup` :

* Peut éventuellement inclure une méthode [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) permettant de configurer les services de l’application.
* Doit inclure une méthode [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) pour créer le pipeline de traitement de demande de l’application.

`ConfigureServices` et `Configure` sont appelées par le runtime au démarrage de l’application :

[!code-csharp[](startup/snapshot_sample/Startup1.cs)]

Spécifiez la classe `Startup` avec la méthode [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) :

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

L’hôte web fournit des services disponibles pour le constructeur de classe `Startup`. L’application ajoute des services supplémentaires à l’aide de `ConfigureServices`. Les services de l’hôte ainsi que ceux de l’application sont alors disponibles dans `Configure` et dans l’ensemble de l’application.

Une utilisation courante de [l’injection de dépendances](xref:fundamentals/dependency-injection) dans la classe `Startup` consiste à injecter :

* [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) pour configurer les services par environnement.
* [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) pour lire la configuration.
* [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) pour créer un enregistreur d’événements dans `Startup.ConfigureServices`.

[!code-csharp[](startup/snapshot_sample/Startup2.cs)]

Une alternative à l’injection de `IHostingEnvironment` consiste à utiliser une approche basée sur les conventions. Quand l’application définit différentes classes `Startup` pour différents environnements (par exemple, `StartupDevelopment`), la classe `Startup` appropriée est sélectionnée au moment de l’exécution. La classe dont le suffixe du nom correspond à l'environnement actuel est prioritaire. Si l’application est exécutée dans l’environnement de développement et comprend à la fois une classe `Startup` et une classe `StartupDevelopment`, la classe `StartupDevelopment` est utilisée. Pour plus d’informations, consultez [Utiliser plusieurs environnements](xref:fundamentals/environments#environment-based-startup-class-and-methods).

Pour en savoir plus sur `WebHostBuilder`, consultez la rubrique [Hébergement](xref:fundamentals/host/index). Pour plus d’informations sur la gestion des erreurs lors du démarrage, consultez [Gestion des exceptions de démarrage](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Méthode ConfigureServices

La méthode [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) est :

* Facultatif
* Appelée par l’hôte web avant la méthode `Configure` pour configurer les services de l’application.
* L’emplacement où les [options de configuration](xref:fundamentals/configuration/index) sont définies par convention.

Le modèle par défaut consiste à appeler toutes les méthodes `Add{Service}`, puis toutes les méthodes `services.Configure{Service}`. Par exemple, consultez [Configurer les services d’identité](xref:security/authentication/identity#pw).

L'ajout de services au conteneur de service les rend disponibles au sein de l’application et dans la méthode `Configure`. Les services sont résolus à l’aide de [l’injection de dépendances](xref:fundamentals/dependency-injection) ou à partir [d’IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).

L’hôte web peut configurer certains services avant que les méthodes `Startup` soient appelées. Vous trouverez plus de détails dans la rubrique [Héberger dans ASP.NET Core](xref:fundamentals/host/index).

Pour les fonctionnalités qui nécessitent une installation substantielle, il existe des méthodes d’extension `Add[Service]` sur [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Une application ASP.NET Core classique inscrit des services pour Entity Framework, Identity et MVC :

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="the-configure-method"></a>Méthode Configure

La méthode [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) est utilisée pour spécifier la façon dont l’application répond aux requêtes HTTP. Le pipeline de requête est configuré en ajoutant des composants [d’intergiciel (middleware)](xref:fundamentals/middleware/index) à une instance [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder). `IApplicationBuilder` est disponible pour la méthode `Configure`, mais elle n’est pas inscrite dans le conteneur de service. L’hébergement crée un `IApplicationBuilder` et le passe directement à `Configure`.

Les [modèles ASP.NET Core](/dotnet/core/tools/dotnet-new) configurent le pipeline avec la prise en charge d’une page d’exception de développeur, de [BrowserLink](http://vswebessentials.com/features/browserlink), des pages d’erreurs, des fichiers statiques et d’ASP.NET Core MVC :

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Chaque méthode d’extension `Use` ajoute un composant d’intergiciel au pipeline de requête. Par exemple, la méthode d’extension `UseMvc` ajoute [le middleware de routage](xref:fundamentals/routing) au pipeline des requêtes et configure [MVC](xref:mvc/overview) comme gestionnaire par défaut.

Chaque composant de middleware du pipeline des requêtes est chargé d’appeler le composant suivant du pipeline ou de court-circuiter la chaîne, si c’est approprié. Si un court-circuit ne se produit pas dans la chaîne de middlewares, chaque middleware a une deuxième occasion de traiter la requête avant qu’elle soit envoyée au client.

Il est également possible de spécifier des services supplémentaires, comme `IHostingEnvironment` et `ILoggerFactory`, dans la signature de méthode. Si tel est le cas, les services supplémentaires sont injectés s’ils sont disponibles.

Pour plus d’informations sur l’utilisation de `IApplicationBuilder` et l’ordre de traitement des middlewares, consultez [Middleware](xref:fundamentals/middleware/index).

## <a name="convenience-methods"></a>Méthodes pratiques

Les méthodes pratiques [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) et [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) peuvent être utilisées au lieu de spécifier une classe `Startup`. Les appels multiples à `ConfigureServices` s’ajoutent les uns aux autres. Les appels multiples à `Configure` utilisent le dernier appel à la méthode.

[!code-csharp[](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="extend-startup-with-startup-filters"></a>Étendre le démarrage avec les filtres de démarrage

Utilisez [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) pour configurer un intergiciel au début ou à la fin du pipeline de l’intergiciel de la méthode [Configure](#the-configure-method) d’une application. `IStartupFilter` est utile pour s’assurer qu’un intergiciel s’exécute avant ou après l’intergiciel ajouté par des bibliothèques au début ou à la fin du pipeline de traitement de requête de l’application.

`IStartupFilter` implémente une méthode unique, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), qui reçoit et retourne un `Action<IApplicationBuilder>`. Un [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) définit une classe pour configurer le pipeline de requête d’une application. Pour plus d’informations, consultez [Créer un pipeline de middlewares avec IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).

Chaque `IStartupFilter` implémente un ou plusieurs middlewares dans le pipeline de requête. Les filtres sont appelés dans l’ordre dans lequel ils ont été ajoutés au conteneur de service. Les filtres peuvent ajouter l’intergiciel avant ou après la transmission du contrôle au filtre suivant. Par conséquent, ils s’ajoutent au début ou à la fin du pipeline de l’application.

L’exemple d’application montre comment inscrire un middleware avec `IStartupFilter`. L’exemple d’application inclut un middleware qui définit une valeur d’options à partir d’un paramètre de chaîne de requête :

[!code-csharp[](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` est configuré dans la classe `RequestSetOptionsStartupFilter` :

[!code-csharp[](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` est inscrit dans le conteneur de service dans [IWebHostBuilder.ConfigureServices](xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.ConfigureServices*) pour illustrer la façon dont le filtre de démarrage augmente `Startup` en dehors de la classe `Startup` :

[!code-csharp[](startup/sample/Program.cs?name=snippet1&highlight=4-5)]

Quand un paramètre de chaîne de requête pour `option` est fourni, l’intergiciel traite l’affectation de valeur avant que l’intergiciel MVC affiche la réponse :

![Fenêtre de navigateur affichant la page d’index rendue. La valeur d’Option rendue est « From Middleware » en fonction de la demande de la page avec le paramètre de chaîne de requête et la valeur d’option définie sur « From Middleware ».](startup/_static/index.png)

L’ordre d’exécution de l’intergiciel est défini par l’ordre des inscriptions d’`IStartupFilter` :

* Plusieurs implémentations d’`IStartupFilter` peuvent interagir avec les mêmes objets. Si l’ordre est important, définissez l’ordre des inscriptions de leurs services `IStartupFilter` pour qu’il corresponde à l’ordre dans lequel leurs intergiciels doivent s’exécuter.
* Les bibliothèques peuvent ajouter des intergiciels avec une ou plusieurs implémentations `IStartupFilter` qui s’exécutent avant ou après l’autre intergiciel d’application inscrit avec `IStartupFilter`. Pour appeler un intergiciel `IStartupFilter` avant un intergiciel ajouté à l’`IStartupFilter` d’une bibliothèque, placez l’inscription du service avant l’ajout de la bibliothèque au conteneur de service. Pour l’appeler par la suite, placez l’inscription du service après l’ajout de la bibliothèque.

## <a name="add-configuration-at-startup-from-an-external-assembly"></a>Ajouter la configuration au démarrage à partir d’un assembly externe

L’implémentation de [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) permet d’ajouter des améliorations à une application au démarrage à partir d’un assembly externe, en dehors de la classe `Startup` de l’application. Pour plus d’informations, consultez [Améliorer une application à partir d’un assembly externe](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:fundamentals/host/index>
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
