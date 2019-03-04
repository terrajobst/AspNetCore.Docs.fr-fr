---
title: Démarrage d’une application dans ASP.NET Core
author: tdykstra
description: Découvrez comment la classe Startup en ASP.NET Core configure les services et le pipeline de requête de l’application.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/17/2019
uid: fundamentals/startup
ms.openlocfilehash: cfd0a57d5d0b60862b017a170b6d5cbddf56f15a
ms.sourcegitcommit: b3894b65e313570e97a2ab78b8addd22f427cac8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56744181"
---
# <a name="app-startup-in-aspnet-core"></a>Démarrage d’une application dans ASP.NET Core

Par [Tom Dykstra](https://github.com/tdykstra), [Luke Latham](https://github.com/guardrex) et [Steve Smith](https://ardalis.com)

La classe `Startup` configure les services et le pipeline de demande de l’application.

## <a name="the-startup-class"></a>Classe Startup

Les applications ASP.NET Core utilisent une classe `Startup`, appelée `Startup` par convention. La classe `Startup` :

* Inclut éventuellement une méthode <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> pour configurer les *services* de l’application. Un service est un composant réutilisable qui fournit une fonctionnalité d’application. Les services sont configurés &mdash;ou *inscrits*&mdash;dans `ConfigureServices` et consommés dans l’application via l’[injection de dépendances](xref:fundamentals/dependency-injection) ou <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.
* Inclut une méthode <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> pour créer le pipeline de traitement des requêtes de l’application.

`ConfigureServices` et `Configure` sont appelées par le runtime au démarrage de l’application :

[!code-csharp[](startup/sample_snapshot/Startup1.cs?highlight=4,10)]

La classe `Startup` est spécifiée pour l’application quand son[hôte](xref:fundamentals/index#host) est généré. L’hôte de l’application est généré quand `Build` est appelé sur le générateur de l’hôte dans la classe `Program`. La classe `Startup` est généralement spécifiée via l’appel de la méthode [ WebHostBuilderExtensions.UseStartup \<TStartup>](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) sur le générateur de l’hôte :

[!code-csharp[](startup/sample_snapshot/Program3.cs?name=snippet_Program&highlight=10)]

L’hôte fournit des services accessibles au constructeur de classe `Startup`. L’application ajoute des services supplémentaires à l’aide de `ConfigureServices`. Les services de l’hôte ainsi que ceux de l’application sont alors disponibles dans `Configure` et dans l’ensemble de l’application.

Une utilisation courante de [l’injection de dépendances](xref:fundamentals/dependency-injection) dans la classe `Startup` consiste à injecter :

* <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> pour configurer les services en fonction de l’environnement.
* <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> pour lire la configuration.
* <xref:Microsoft.Extensions.Logging.ILoggerFactory> pour créer un enregistreur d’événements dans `Startup.ConfigureServices`.

[!code-csharp[](startup/sample_snapshot/Startup2.cs?highlight=7-8)]

Une alternative à l’injection de `IHostingEnvironment` consiste à utiliser une approche basée sur les conventions. Quand l’application définit différentes classes `Startup` pour différents environnements (par exemple, `StartupDevelopment`), la classe `Startup` appropriée est sélectionnée au moment de l’exécution. La classe dont le suffixe du nom correspond à l'environnement actuel est prioritaire. Si l’application est exécutée dans l’environnement de développement et comprend à la fois une classe `Startup` et une classe `StartupDevelopment`, la classe `StartupDevelopment` est utilisée. Pour plus d’informations, consultez [Utiliser plusieurs environnements](xref:fundamentals/environments#environment-based-startup-class-and-methods).

Pour en savoir plus sur l’hôte, consultez [L’hôte](xref:fundamentals/index#host). Pour plus d’informations sur la gestion des erreurs lors du démarrage, consultez [Gestion des exceptions de démarrage](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Méthode ConfigureServices

La méthode <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> est :

* Optionnel.
* Appelée par l’hôte avant la méthode `Configure` pour configurer les services de l’application
* L’emplacement où les [options de configuration](xref:fundamentals/configuration/index) sont définies par convention.

Le modèle par défaut consiste à appeler toutes les méthodes `Add{Service}`, puis toutes les méthodes `services.Configure{Service}`. Par exemple, consultez [Configurer les services d’identité](xref:security/authentication/identity#pw).

L’hôte peut configurer certains services avant l’appel des méthodes `Startup`. Pour plus d’informations, consultez [L’hôte](xref:fundamentals/index#host).

Pour les fonctionnalités qui nécessitent une configuration importante, il existe des méthodes d’extension `Add{Service}` pour <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>. Une application ASP.NET Core classique inscrit des services pour Entity Framework, Identity et MVC :

[!code-csharp[](startup/sample_snapshot/Startup3.cs?highlight=4,7,11)]

L'ajout de services au conteneur de service les rend disponibles au sein de l’application et dans la méthode `Configure`. Les services sont résolus via l’[injection de dépendances](xref:fundamentals/dependency-injection) ou à partir de <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.

## <a name="the-configure-method"></a>Méthode Configure

La méthode <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> permet de spécifier le mode de réponse de l’application aux requêtes HTTP. Vous configurez le pipeline de requête en ajoutant des composants d’[intergiciel (middleware)](xref:fundamentals/middleware/index) à une instance de <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>. `IApplicationBuilder` est disponible pour la méthode `Configure`, mais elle n’est pas inscrite dans le conteneur de service. L’hébergement crée un `IApplicationBuilder` et le passe directement à `Configure`.

Les [modèles ASP.NET Core](/dotnet/core/tools/dotnet-new) configurent le pipeline pour permettre la prise en charge des éléments suivants :

* [Page d’exceptions du développeur](xref:fundamentals/error-handling#the-developer-exception-page)
* [Gestionnaire d’exceptions](xref:fundamentals/error-handling#configure-a-custom-exception-handling-page)
* [HSTS (HTTP Strict Transport Security)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [Redirection HTTPS](xref:security/enforcing-ssl)
* [Fichiers statiques](xref:fundamentals/static-files)
* [RGPD (règlement général sur la protection des données)](xref:security/gdpr)
* ASP.NET Core [MVC](xref:mvc/overview) et [Razor Pages](xref:razor-pages/index)

[!code-csharp[](startup/sample_snapshot/Startup4.cs)]

Chaque méthode d’extension `Use` ajoute un ou plusieurs composants de middleware au pipeline de requête. Par exemple, la méthode d’extension `UseMvc` ajoute le [middleware de routage](xref:fundamentals/routing) au pipeline de requête et configure [MVC](xref:mvc/overview) en tant que gestionnaire par défaut.

Chaque composant de middleware du pipeline des requêtes est chargé d’appeler le composant suivant du pipeline ou de court-circuiter la chaîne, si c’est approprié. Si un court-circuit ne se produit pas dans la chaîne de middlewares, chaque middleware a une deuxième occasion de traiter la requête avant qu’elle soit envoyée au client.

Vous pouvez spécifier des services supplémentaires, par exemple `IHostingEnvironment` et `ILoggerFactory`, dans la signature de méthode `Configure`. Si tel est le cas, les services supplémentaires sont injectés s’ils sont disponibles.

Pour plus d’informations sur l’utilisation de `IApplicationBuilder` et sur l’ordre de traitement des middlewares, consultez <xref:fundamentals/middleware/index>.

## <a name="convenience-methods"></a>Méthodes pratiques

Pour configurer les services et le pipeline de traitement de requête sans utiliser de classe `Startup`, appelez les méthodes pratiques `ConfigureServices` et `Configure` sur le générateur de l’hôte. Les appels multiples à `ConfigureServices` s’ajoutent les uns aux autres. S’il existe plusieurs appels de méthode `Configure`, le dernier appel de `Configure` est utilisé.

[!code-csharp[](startup/sample_snapshot/Program1.cs?highlight=18,22)]

## <a name="extend-startup-with-startup-filters"></a>Étendre le démarrage avec les filtres de démarrage

Utilisez <xref:Microsoft.AspNetCore.Hosting.IStartupFilter> pour configurer le middleware au début ou à la fin du pipeline de middleware de la méthode [Configure](#the-configure-method) d’une application. `IStartupFilter` est utile pour s’assurer qu’un intergiciel s’exécute avant ou après l’intergiciel ajouté par des bibliothèques au début ou à la fin du pipeline de traitement de requête de l’application.

`IStartupFilter` implémente une seule méthode, <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, qui reçoit et retourne `Action<IApplicationBuilder>`. <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> définit une classe pour configurer le pipeline de requête d’une application. Pour plus d’informations, consultez [Créer un pipeline de middlewares avec IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).

Chaque `IStartupFilter` implémente un ou plusieurs intergiciels dans le pipeline de requête. Les filtres sont appelés dans l’ordre dans lequel ils ont été ajoutés au conteneur de service. Les filtres peuvent ajouter l’intergiciel avant ou après la transmission du contrôle au filtre suivant. Par conséquent, ils s’ajoutent au début ou à la fin du pipeline de l’application.

L’exemple suivant montre comment inscrire un middleware auprès de `IStartupFilter`.

Le middleware `RequestSetOptionsMiddleware` définit une valeur d’options à partir d’un paramètre de chaîne de requête :

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsMiddleware.cs?name=snippet1&highlight=21)]

`RequestSetOptionsMiddleware` est configuré dans la classe `RequestSetOptionsStartupFilter` :

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` est inscrit dans le conteneur de service de <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> et augmente `Startup` à partir de l’extérieur de la classe `Startup` :

[!code-csharp[](startup/sample_snapshot/Program2.cs?name=snippet1&highlight=4-5)]

Quand un paramètre de chaîne de requête pour `option` est fourni, l’intergiciel traite l’affectation de valeur avant que l’intergiciel MVC affiche la réponse :

![Fenêtre de navigateur affichant la page d’index rendue. La valeur d’Option rendue est « From Middleware » en fonction de la demande de la page avec le paramètre de chaîne de requête et la valeur d’option définie sur « From Middleware ».](startup/_static/index.png)

L’ordre d’exécution de l’intergiciel est défini par l’ordre des inscriptions d’`IStartupFilter` :

* Plusieurs implémentations d’`IStartupFilter` peuvent interagir avec les mêmes objets. Si l’ordre est important, définissez l’ordre des inscriptions de leurs services `IStartupFilter` pour qu’il corresponde à l’ordre dans lequel leurs intergiciels doivent s’exécuter.
* Les bibliothèques peuvent ajouter des intergiciels avec une ou plusieurs implémentations `IStartupFilter` qui s’exécutent avant ou après l’autre intergiciel d’application inscrit avec `IStartupFilter`. Pour appeler un intergiciel `IStartupFilter` avant un intergiciel ajouté à l’`IStartupFilter` d’une bibliothèque, placez l’inscription du service avant l’ajout de la bibliothèque au conteneur de service. Pour l’appeler par la suite, placez l’inscription du service après l’ajout de la bibliothèque.

## <a name="add-configuration-at-startup-from-an-external-assembly"></a>Ajouter la configuration au démarrage à partir d’un assembly externe

Une implémentation de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permet d’ajouter des améliorations à une application au démarrage à partir d’un assembly externe, en dehors de la classe `Startup` de l’application. Pour plus d'informations, consultez <xref:fundamentals/configuration/platform-specific-configuration>.

## <a name="additional-resources"></a>Ressources supplémentaires

* [L’hôte](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
