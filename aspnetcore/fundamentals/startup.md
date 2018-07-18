---
title: Démarrage d’une application dans ASP.NET Core
author: ardalis
description: Découvrez comment la classe Startup dans ASP.NET Core configure des services et le pipeline de requête de l’application.
ms.author: tdykstra
ms.custom: mvc
ms.date: 4/13/2018
uid: fundamentals/startup
ms.openlocfilehash: 285d74c0d12e3aca4d8c33d39467dfda02712993
ms.sourcegitcommit: e12f45ddcbe99102a74d4077df27d6c0ebba49c1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/15/2018
ms.locfileid: "39063258"
---
# <a name="application-startup-in-aspnet-core"></a>Démarrage d’une application dans ASP.NET Core

Par [Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra) et [Luke Latham](https://github.com/guardrex)

La classe `Startup` configure des services et le pipeline de requête de l’application.

## <a name="the-startup-class"></a>Classe Startup

Les applications ASP.NET Core utilisent une classe `Startup`, appelée `Startup` par convention. La classe `Startup` :

* Peut éventuellement inclure une méthode [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) permettant de configurer les services de l’application.
* Doit inclure une méthode [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) pour créer le pipeline de traitement de demande de l’application.

`ConfigureServices` et `Configure` sont appelées par le runtime au démarrage de l’application :

[!code-csharp[](startup/snapshot_sample/Startup1.cs)]

Spécifiez la classe `Startup` avec la méthode [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) :

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

Le constructeur de la classe `Startup` accepte les dépendances définies par l’hôte. Une utilisation courante de [l’injection de dépendances](xref:fundamentals/dependency-injection) dans la classe `Startup` consiste à injecter :

* [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) pour configurer des services par environnement.
* [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) pour configurer l’application au démarrage.

[!code-csharp[](startup/snapshot_sample/Startup2.cs)]

Une alternative à l’injection de `IHostingEnvironment` consiste à utiliser une approche basée sur les conventions. L’application peut définir différentes classes `Startup` pour différents environnements (par exemple, `StartupDevelopment`), et la classe `Startup` appropriée est sélectionnée au moment de l’exécution. La classe dont le suffixe du nom correspond à l'environnement actuel est prioritaire. Si l’application est exécutée dans l’environnement de développement et comprend à la fois une classe `Startup` et une classe `StartupDevelopment`, la classe `StartupDevelopment` est utilisée. Pour plus d’informations, consultez [Utiliser plusieurs environnements](xref:fundamentals/environments#environment-based-startup-class-and-methods).

Pour en savoir plus sur `WebHostBuilder`, consultez la rubrique [Hébergement](xref:fundamentals/host/index). Pour plus d’informations sur la gestion des erreurs lors du démarrage, consultez [Gestion des exceptions de démarrage](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Méthode ConfigureServices

La méthode [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) est :

* Facultatif
* Appelée par l’hôte web avant la méthode `Configure` pour configurer les services de l’application.
* L’emplacement où les [options de configuration](xref:fundamentals/configuration/index) sont définies par convention.

L’ajout de services au conteneur de service les rend disponibles dans l’application et dans la méthode `Configure`. Les services sont résolus à l’aide de [l’injection de dépendances](xref:fundamentals/dependency-injection) ou à partir [d’IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).

L’hôte web peut configurer certains services avant que les méthodes `Startup` soient appelées. Vous trouverez plus de détails dans la rubrique [Héberger dans ASP.NET Core](xref:fundamentals/host/index).

Pour les fonctionnalités qui nécessitent une installation substantielle, il existe des méthodes d’extension `Add[Service]` sur [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Une application web classique inscrit des services pour Entity Framework, Identity et MVC :

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

::: moniker range=">= aspnetcore-2.1"

<a name="setcompatibilityversion"></a>

### <a name="setcompatibilityversion-for-aspnet-core-mvc"></a>SetCompatibilityVersion pour ASP.NET Core MVC 

La méthode `SetCompatibilityVersion` permet à une application d’accepter ou de refuser les changements de comportement potentiellement cassants qui ont été introduits dans ASP.NET MVC Core 2.1+. Ces changements de comportement potentiellement cassants concernent en général la façon dont le sous-système MVC se comporte et la façon dont **votre code** est appelé par le runtime. En acceptant, vous obtenez le comportement le plus récent et le comportement à long terme d’ASP.NET Core.

Le code suivant définit le mode de compatibilité sur ASP.NET Core 2.1 :

[!code-csharp[Main](startup/sampleCompatibility/Startup.cs?name=snippet1)]

Nous vous recommandons de tester votre application avec la version la plus récente (`CompatibilityVersion.Version_2_1`). Nous pensons que la plupart des applications ne connaîtront pas de changements de comportement cassants avec la version la plus récente. 

Les applications qui appellent `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` sont protégées contre les changements de comportement potentiellement cassants qui ont été introduits dans ASP.NET Core 2.1 MVC et dans les versions 2.x ultérieures. Cette protection :

* Ne s’applique pas à tous les changements de 2.1 et ultérieur. Elle est destinée aux changements de comportement potentiellement cassants du runtime ASP.NET Core dans le sous-système MVC.
* Ne s’étend pas à la version majeure suivante.

La compatibilité par défaut pour les applications ASP.NET Core 2.1 et les versions 2.x ultérieures qui n’appellent **pas** `SetCompatibilityVersion` est la compatibilité 2.0. Autrement dit, ne pas appeler `SetCompatibilityVersion` revient à appeler `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.

Le code suivant définit le mode de compatibilité sur ASP.NET Core 2.1, sauf pour les comportements suivants :

* [AllowCombiningAuthorizeFilters](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)
* [InputFormatterExceptionPolicy](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)

[!code-csharp[Main](startup/sampleCompatibility/Startup2.cs?name=snippet1)]

Pour les applications qui rencontrent des changements de comportement cassants, l’utilisation des commutateurs de compatibilité appropriés :

* Vous permet d’utiliser la dernière version et de refuser des changements de comportement cassants spécifiques.
* Vous laisse le temps de mettre à jour votre application pour qu’elle fonctionne avec les derniers changements.

Les commentaires dans la source de la classe [MvcOptions](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) expliquent clairement ce qui a changé et pourquoi les changements représentent une amélioration pour la plupart des utilisateurs.

À une date ultérieure, il y aura une [version d’ASP.NET Core 3.0](https://github.com/aspnet/Home/wiki/Roadmap). Les comportements anciens pris en charge par les commutateurs de compatibilité seront supprimés dans la version 3.0. Nous pensons que ce sont des changements positifs, qui vont bénéficier à presque tous les utilisateurs. Comme nous introduisons ces changements maintenant, la plupart des applications peuvent en profiter tout de suite. Pour les autres applications, les développeurs ont du temps pour les mettre à jour.

::: moniker-end

## <a name="services-available-in-startup"></a>Services disponibles dans Startup

L’hôte web fournit des services disponibles pour le constructeur de classe `Startup`. L’application ajoute des services supplémentaires à l’aide de `ConfigureServices`. Les services de l’hôte ainsi que ceux de l’application sont alors disponibles dans `Configure` et dans l’ensemble de l’application.

## <a name="the-configure-method"></a>Méthode Configure

La méthode [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) est utilisée pour spécifier la façon dont l’application répond aux requêtes HTTP. Le pipeline de requête est configuré en ajoutant des composants [d’intergiciel (middleware)](xref:fundamentals/middleware/index) à une instance [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder). `IApplicationBuilder` est disponible pour la méthode `Configure`, mais elle n’est pas inscrite dans le conteneur de service. L’hébergement crée un `IApplicationBuilder` et le passe directement à `Configure` ([source de référence](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).

Les [modèles ASP.NET Core](/dotnet/core/tools/dotnet-new) configurent le pipeline avec la prise en charge d’une page d’exception de développeur, de [BrowserLink](http://vswebessentials.com/features/browserlink), des pages d’erreurs, des fichiers statiques et d’ASP.NET MVC :

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Chaque méthode d’extension `Use` ajoute un composant d’intergiciel au pipeline de requête. Par exemple, la méthode d’extension `UseMvc` ajoute [le middleware de routage](xref:fundamentals/routing) au pipeline des requêtes et configure [MVC](xref:mvc/overview) comme gestionnaire par défaut.

Chaque composant de middleware du pipeline des requêtes est chargé d’appeler le composant suivant du pipeline ou de court-circuiter la chaîne, si c’est approprié. Si un court-circuit ne se produit pas dans la chaîne de middlewares, chaque middleware a une deuxième occasion de traiter la requête avant qu’elle soit envoyée au client.

Il est également possible de spécifier des services supplémentaires, comme `IHostingEnvironment` et `ILoggerFactory`, dans la signature de méthode. Si tel est le cas, les services supplémentaires sont injectés s’ils sont disponibles.

Pour plus d’informations sur l’utilisation de `IApplicationBuilder` et l’ordre de traitement des middlewares, consultez [Middleware](xref:fundamentals/middleware/index).

## <a name="convenience-methods"></a>Méthodes pratiques

Les méthodes pratiques [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) et [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) peuvent être utilisées au lieu de spécifier une classe `Startup`. Les appels multiples à `ConfigureServices` s’ajoutent les uns aux autres. Les appels multiples à `Configure` utilisent le dernier appel à la méthode.

[!code-csharp[](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="startup-filters"></a>Filtres Startup

Utilisez [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) pour configurer un intergiciel au début ou à la fin du pipeline de l’intergiciel de la méthode [Configure](#the-configure-method) d’une application. `IStartupFilter` est utile pour s’assurer qu’un intergiciel s’exécute avant ou après l’intergiciel ajouté par des bibliothèques au début ou à la fin du pipeline de traitement de requête de l’application.

`IStartupFilter` implémente une méthode unique, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), qui reçoit et retourne un `Action<IApplicationBuilder>`. Un [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) définit une classe pour configurer le pipeline de requête d’une application. Pour plus d’informations, consultez [Créer un pipeline de middlewares avec IApplicationBuilder](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder).

Chaque `IStartupFilter` implémente un ou plusieurs intergiciels dans le pipeline de requête. Les filtres sont appelés dans l’ordre dans lequel ils ont été ajoutés au conteneur de service. Les filtres peuvent ajouter l’intergiciel avant ou après la transmission du contrôle au filtre suivant. Par conséquent, ils s’ajoutent au début ou à la fin du pipeline de l’application.

[L’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample)) montre comment inscrire un intergiciel avec `IStartupFilter`. L’exemple d’application inclut un intergiciel qui définit une valeur d’options à partir d’un paramètre de chaîne de requête :

[!code-csharp[](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` est configuré dans la classe `RequestSetOptionsStartupFilter` :

[!code-csharp[](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` est inscrit dans le conteneur de service dans `ConfigureServices` :

[!code-csharp[](startup/sample/Startup.cs?name=snippet1&highlight=3)]

Quand un paramètre de chaîne de requête pour `option` est fourni, l’intergiciel traite l’affectation de valeur avant que l’intergiciel MVC affiche la réponse :

![Fenêtre de navigateur affichant la page d’index rendue. La valeur d’Option rendue est « From Middleware » en fonction de la demande de la page avec le paramètre de chaîne de requête et la valeur d’option définie sur « From Middleware ».](startup/_static/index.png)

L’ordre d’exécution de l’intergiciel est défini par l’ordre des inscriptions d’`IStartupFilter` :

* Plusieurs implémentations d’`IStartupFilter` peuvent interagir avec les mêmes objets. Si l’ordre est important, définissez l’ordre des inscriptions de leurs services `IStartupFilter` pour qu’il corresponde à l’ordre dans lequel leurs intergiciels doivent s’exécuter.
* Les bibliothèques peuvent ajouter des intergiciels avec une ou plusieurs implémentations `IStartupFilter` qui s’exécutent avant ou après l’autre intergiciel d’application inscrit avec `IStartupFilter`. Pour appeler un intergiciel `IStartupFilter` avant un intergiciel ajouté à l’`IStartupFilter` d’une bibliothèque, placez l’inscription du service avant l’ajout de la bibliothèque au conteneur de service. Pour l’appeler par la suite, placez l’inscription du service après l’ajout de la bibliothèque.

## <a name="adding-configuration-at-startup-from-an-external-assembly"></a>Ajout de la configuration au démarrage à partir d’un assembly externe

L’implémentation de [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) permet d’ajouter des améliorations à une application au démarrage à partir d’un assembly externe, en dehors de la classe `Startup` de l’application. Pour plus d’informations, consultez [Améliorer une application à partir d’un assembly externe](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="additional-resources"></a>Ressources supplémentaires

* [Hébergement](xref:fundamentals/host/index)
* [Utiliser plusieurs environnements](xref:fundamentals/environments)
* [Intergiciel (middleware)](xref:fundamentals/middleware/index)
* [Journalisation](xref:fundamentals/logging/index)
* [Configuration](xref:fundamentals/configuration/index)
* [Classe StartupLoader : méthode FindStartupType (source de référence)](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
