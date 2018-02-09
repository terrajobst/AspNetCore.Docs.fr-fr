---
title: "Démarrage de l’application dans ASP.NET Core"
author: ardalis
description: "Découvrez comment la classe de démarrage dans ASP.NET Core configure les services et le pipeline de demande de l’application."
ms.author: tdykstra
manager: wpickett
ms.custom: mvc
ms.date: 12/08/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: 81d76c39b7890e2d4ab86252cb0a343e3bb7359a
ms.sourcegitcommit: 83b5a4715fd25e4eb6f7c8427c0ef03850a7fa07
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2018
---
# <a name="application-startup-in-aspnet-core"></a>Démarrage de l’application dans ASP.NET Core

Par [Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra), et [Luke Latham](https://github.com/guardrex)

La classe `Startup` configure les services et le pipeline de demande de l’application.

## <a name="the-startup-class"></a>Classe de démarrage.

Les applications ASP.NET Core utilisent une classe `Startup`, qui est nommé `Startup` par convention. La classe `Startup` :

* Peut éventuellement inclure une méthode [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) permettant de configurer les services de l’application.
* Doit inclure une méthode [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) pour créer le pipeline de traitement de demande de l’application.

`ConfigureServices`et `Configure` sont appelées par le runtime au démarrage de l’application :

[!code-csharp[Main](startup/snapshot_sample/Startup1.cs)]

Spécifiez la classe `Startup` avec le [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt; ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) méthode :

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

Le constructeur de la classe `Startup` accepte les dépendances définies par l’hôte. Une utilisation courante de l'[injection de dépendance](xref:fundamentals/dependency-injection) dans la classe `Startup` consiste à injecter :

* [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) pour configurer les services par environnement.
* [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) pour configurer l’application lors du démarrage.

[!code-csharp[Main](startup/snapshot_sample/Startup2.cs)]

Une alternative à l’injection de `IHostingEnvironment` consiste à utiliser une approche basée sur les conventions. L’application peut définir des classes `Startup` distinctes pour différents environnements (par exemple, `StartupDevelopment`), et la classe de démarrage appropriée est sélectionnée lors de l’exécution. La classe dont le suffixe de nom correspond à l’environnement actuel est priorisée. Si l’application est exécutée dans l’environnement de développement et comprend à la fois une classe `Startup` et une classe `StartupDevelopment`, la classe `StartupDevelopment` est utilisée. Pour plus d’informations, consultez [Utilisation de plusieurs environnements](xref:fundamentals/environments#startup-conventions).

Pour en savoir plus sur `WebHostBuilder`, consultez la rubrique [hébergement](xref:fundamentals/hosting). Pour plus d’informations sur la gestion des erreurs lors du démarrage, consultez [la gestion des exceptions de démarrage](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>La méthode ConfigureServices

La méthode [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) est :

* Facultatif.
* Appelée par l’hôte web avant du `Configure` méthode permettant de configurer les services de l’application.
* Où [les options de configuration](xref:fundamentals/configuration/index) sont définis par convention.

L'ajout de services au conteneur de services les rend disponibles au sein de l’application et dans la méthode `Configure`. Les services ne sont pas résolus via l'[injection de dépendance](xref:fundamentals/dependency-injection) ou à partir de [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).

L’hôte web peut configurer certains services avant que les méthodes`Startup` soient appelées. Les détails sont disponibles dans la rubrique [hébergement](xref:fundamentals/hosting). 

Pour les fonctionnalités qui nécessitent le programme d’installation substantiel, il y a les méthodes d’extension `Add[Service]` sur [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Une application web typique inscrit des services pour Entity Framework, d’identité et MVC :

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="services-available-in-startup"></a>Services disponibles dans Startup

L’hôte web fournit des services qui sont disponibles pour le constructeur de classe `Startup`. L’application ajoute des services supplémentaires via `ConfigureServices`. Les services de l’hôte et l’application sont ensuite disponibles dans `Configure` et dans l’ensemble de l’application.

## <a name="the-configure-method"></a>Méthode Configure

La méthode [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) est utilisée pour spécifier la manière dont l’application répond aux demandes HTTP. Le pipeline de requête est configuré en ajoutant les composants de l'[intergiciel (middleware)](xref:fundamentals/middleware) à une instance [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder). `IApplicationBuilder`est disponible pour la méthode `Configure`, mais il n’est pas enregistré dans le conteneur de service. L'hébergement crée un `IApplicationBuilder` et le passe directement à `Configure` ([source de référence](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).

[Les modèles ASP.NET Core](/dotnet/core/tools/dotnet-new) configurent le pipeline avec une prise en charge d'une page d’exception de développeur, [BrowserLink](http://vswebessentials.com/features/browserlink), des pages d’erreurs, les fichiers statiques et ASP.NET MVC :

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Chaque méthode d’extension `Use` ajoute un composant d’intergiciel (middleware) au pipeline de demande. Par exemple, la méthode d’extension `UseMvc` ajoute l'[intergiciel (middleware) de routage](xref:fundamentals/routing) au pipeline de demande et configure [MVC](xref:mvc/overview) en tant que gestionnaire par défaut.

Les services supplémentaires, tels que `IHostingEnvironment` et `ILoggerFactory`, peuvent également être spécifiées dans la signature de méthode. Si spécifiés, les services supplémentaires sont injectées si ils sont disponibles.

Pour plus d’informations sur l’utilisation de `IApplicationBuilder`, consultez [Intergiciel (middleware)](xref:fundamentals/middleware).

## <a name="convenience-methods"></a>Méthodes d’usage

Les méthodes pratiques [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) et [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) peuvent être utilisés au lieu de spécifier une classe `Startup`. Des appels multiples à `ConfigureServices` s'ajoutent les uns aux autres. Des appels multiples à `Configure` utilisent le dernier appel de méthode.

[!code-csharp[Main](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="startup-filters"></a>Filtres de démarrage

Utilisez [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) pour configurer un intergiciel (middleware) au début ou à la fin de l’application du pipeline d’intergiciel (middleware) [Configure](#the-configure-method). `IStartupFilter`est utile pour s’assurer qu’un intergiciel (middleware) s’exécute avant ou après l’intergiciel (middleware) ajouté par des bibliothèques au début ou à la fin du pipeline de traitement de demande de l’application.

`IStartupFilter` implémente une méthode unique, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), qui reçoit et renvoie un `Action<IApplicationBuilder>`. Un [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) définit une classe pour configurer le pipeline de demande d’une application. Pour plus d’informations, consultez [création d’un pipeline de l’intergiciel (middleware) avec IApplicationBuilder](xref:fundamentals/middleware#creating-a-middleware-pipeline-with-iapplicationbuilder).

Chaque `IStartupFilter` implémente un ou plusieurs middlewares dans le pipeline de demande. Les filtres sont appelés dans l’ordre où ils ont été ajoutés au conteneur de service. Les filtres peuvent ajouter un intergiciel (middleware) avant ou après le transfert de contrôle pour le filtre suivant, par conséquent, ils s'ajoutent au début ou à la fin du pipeline d’application.

L'[exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([Comment télécharger](xref:tutorials/index#how-to-download-a-sample)) montre comment inscrire un intergiciel (middleware) avec `IStartupFilter`. L’exemple d’application inclut un intergiciel (middleware) qui définit une valeur d’options à partir d’un paramètre de chaîne de requête :

[!code-csharp[Main](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

Le `RequestSetOptionsMiddleware` est configuré dans la classe `RequestSetOptionsStartupFilter` :

[!code-csharp[Main](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

Le `IStartupFilter` est enregistré dans le conteneur de service dans `ConfigureServices`:

[!code-csharp[Main](startup/sample/Startup.cs?name=snippet1&highlight=3)]

Lorsqu’un paramètre de chaîne de requête pour `option` est fourni, l’intergiciel (middleware) traite l’affectation de la valeur avant que l’intergiciel (middleware) MVC affiche la réponse :

![Fenêtre de navigateur affichant la page d’Index rendue. La valeur de l’Option est rendue telle que 'D’intergiciel (middleware)' en fonction de la demande de la page avec la valeur de l’option est définie sur « D’intergiciel (middleware) » et le paramètre de chaîne de requête.](startup/_static/index.png)

L’ordre d’exécution de l'Intergiciel (middleware) est défini par l’ordre des inscriptions de `IStartupFilter` :

* Plusieurs implémentations `IStartupFilter` peuvent interagir avec les mêmes objets. Si l’ordre est important, ordonner leurs inscriptions de service `IStartupFilter` pour faire correspondre avec l’ordre dans lequel les middlewares doivent exécuter.
* Les bibliothèques peuvent ajouter des intergiciels (middleware) avec un ou plusieurs implémentations `IStartupFilter` qui s’exécutent avant ou après l’autre intergiciel (middleware) d'application inscrit avec `IStartupFilter`. Pour appeler un `IStartupFilter` intergiciel (middleware) avant un intergiciel (middleware) ajouté à une bibliothèque `IStartupFilter`, placez l’inscription du service avant que la bibliothèque soit ajoutée au conteneur de service. Pour l'appeler par la suite, placez l’inscription du service après avoir ajouté la bibliothèque.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Hébergement](xref:fundamentals/hosting)
* [Utilisation de plusieurs environnements](xref:fundamentals/environments)
* [Intergiciel (middleware)](xref:fundamentals/middleware)
* [Journalisation](xref:fundamentals/logging/index)
* [Configuration](xref:fundamentals/configuration/index)
* [Classe de StartupLoader : méthode FindStartupType (source de référence)](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
