---
title: Notions de base d’ASP.NET Core
author: rick-anderson
description: Découvrez les concepts de base permettant de créer des applications ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/06/2019
uid: fundamentals/index
ms.openlocfilehash: a56beebd796448705c7b84f47699e9739f451419
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099232"
---
# <a name="aspnet-core-fundamentals"></a>Notions de base d’ASP.NET Core

Une application ASP.NET Core est une application console qui crée un serveur web dans sa méthode `Program.Main`. La méthode `Main` est le *point d’entrée managé* de l’application :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

L’hôte .NET Core :

* Charge le [runtime .NET Core](https://github.com/dotnet/coreclr).
* Utilise le premier argument de ligne de commande comme chemin du fichier binaire managé qui contient le point d’entrée (`Main`) et commence l’exécution de code.

La méthode `Main` appelle [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), qui suit le [modèle de conception builder](https://wikipedia.org/wiki/Builder_pattern) pour créer un hôte web. Le builder a des méthodes qui définissent un serveur web (par exemple, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) et la classe de démarrage (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>). Dans l’exemple précédent, le serveur web [Kestrel](xref:fundamentals/servers/kestrel) est alloué automatiquement. L’hôte web d’ASP.NET Core tente de s’exécuter sur [IIS (Internet Information Services)](https://www.iis.net/), s’il est disponible. D’autres serveurs web, tels que [HTTP.sys](xref:fundamentals/servers/httpsys), peuvent être utilisés via l’appel de la méthode d’extension appropriée. `UseStartup` est expliqué de manière plus détaillée dans la section [Démarrage](#startup).

<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, le type de retour de l’appel de `WebHost.CreateDefaultBuilder`, fournit de nombreuses méthodes facultatives. Certaines de ces méthodes incluent `UseHttpSys` pour l’hébergement de l’application dans HTTP.sys et <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> pour la spécification du répertoire de contenu racine. Les méthodes <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> et <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> génèrent l’objet <xref:Microsoft.AspNetCore.Hosting.IWebHost>, qui héberge l’application et démarre l’écoute des requêtes HTTP.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

L’hôte .NET Core :

* Charge le [runtime .NET Core](https://github.com/dotnet/coreclr).
* Utilise le premier argument de ligne de commande comme chemin du fichier binaire managé qui contient le point d’entrée (`Main`) et commence l’exécution de code.

La méthode `Main` utilise <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, qui suit le [modèle de conception builder](https://wikipedia.org/wiki/Builder_pattern) pour créer un hôte d’application web. Le builder a des méthodes qui définissent le serveur web (par exemple <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) et la classe de démarrage (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>). Dans l’exemple précédent, le serveur web [Kestrel](xref:fundamentals/servers/kestrel) est utilisé. D’autres serveurs web, tels que [HTTP.sys](xref:fundamentals/servers/httpsys), peuvent être utilisés via l’appel de la méthode d’extension appropriée. `UseStartup` est expliqué de manière plus détaillée dans la section [Démarrage](#startup).

`WebHostBuilder` fournit de nombreuses méthodes facultatives, notamment <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> pour l’hébergement dans IIS et IIS Express et <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> pour la spécification du répertoire de contenu racine. Les méthodes <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> et <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> génèrent l’objet <xref:Microsoft.AspNetCore.Hosting.IWebHost>, qui héberge l’application et démarre l’écoute des requêtes HTTP.

::: moniker-end

## <a name="startup"></a>Démarrage

La méthode `UseStartup` sur `WebHostBuilder` spécifie la classe `Startup` pour votre application :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

C’est dans la classe `Startup` que sont configurés les services nécessaires à l’application et qu’est défini le pipeline de traitement des requêtes. La classe `Startup`, qui doit être publique, contient en général les méthodes suivantes. `Startup.ConfigureServices` est facultatif.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> définit les [services](#dependency-injection-services) utilisés par votre application (par exemple, ASP.NET Core MVC, Entity Framework Core et Identity). <xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> définit les [intergiciels (middlewares)](xref:fundamentals/middleware/index) appelés dans le pipeline de requête.

Pour plus d'informations, consultez <xref:fundamentals/startup>.

## <a name="content-root"></a>Racine de contenu

La racine de contenu est le chemin de base de tout contenu utilisé par l’application, tel que les [pages Razor](xref:razor-pages/index), les vues MVC et les composants statiques. Par défaut, la racine de contenu est identique au chemin de base de l’application pour l’exécutable qui héberge l’application.

## <a name="web-root-webroot"></a>Racine web (webroot)

La racine web d’une application correspond au répertoire de projet qui contient les ressources publiques statiques, telles que les fichiers CSS, JavaScript et image. Par défaut, *wwwroot* est la racine web.

Pour les fichiers Razor (*.cshtml*), la barre oblique tilde `~/` pointe vers la racine web. Les chemins commençant par `~/` sont appelés chemins virtuels.

## <a name="dependency-injection-services"></a>Injection de dépendances (services)

Un *service* est un composant destiné à une consommation courante dans une application. Les services sont accessibles via l’[injection de dépendances](xref:fundamentals/dependency-injection). ASP.NET Core inclut un conteneur IoC (Inversion of Control) natif qui prend en charge l’[injection de constructeurs](xref:mvc/controllers/dependency-injection#constructor-injection) par défaut. Vous pouvez remplacer le conteneur par défaut si vous le souhaitez. L’injection de dépendances n’offre pas seulement un [couplage faible](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), elle permet également d’accéder à des services (tels que la [journalisation](xref:fundamentals/logging/index)) dans l’ensemble de votre application.

Pour plus d'informations, consultez <xref:fundamentals/dependency-injection>.

## <a name="middleware"></a>Intergiciel (middleware)

Dans ASP.NET Core, vous composez votre pipeline de requêtes à l’aide d’un [intergiciel (middleware)](xref:fundamentals/middleware/index). Un middleware ASP.NET Core effectue des opérations asynchrones sur `HttpContext`, puis appelle le middleware suivant dans le pipeline ou met fin à la requête.

Par convention, un composant de middleware appelé « XYZ » est ajouté au pipeline en appelant la méthode d’extension `UseXYZ` dans la méthode `Configure`.

ASP.NET Core inclut un ensemble complet de middlewares intégrés, et vous pouvez écrire votre propre middleware personnalisé. [Open Web Interface pour .NET (OWIN)](xref:fundamentals/owin), qui permet aux applications web d’être dissociées des serveurs web, est pris en charge dans les applications ASP.NET Core.

Pour plus d’informations, consultez <xref:fundamentals/middleware/index> et <xref:fundamentals/owin>.

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a>Lancer des requêtes HTTP

<xref:System.Net.Http.IHttpClientFactory> est disponible pour accéder aux instances <xref:System.Net.Http.HttpClient> afin d’effectuer des requêtes HTTP.

Pour plus d'informations, consultez <xref:fundamentals/http-requests>.

::: moniker-end

## <a name="environments"></a>Environnements

Les environnements, tels que *Développement* et *Production*, sont une notion de premier plan dans ASP.NET Core. Vous pouvez les définir à l’aide d’une variable d’environnement, d’un fichier de paramètres et d’un argument de ligne de commande.

Pour plus d'informations, consultez <xref:fundamentals/environments>.

## <a name="hosting"></a>Hébergement

Les applications ASP.NET Core configurent et lancent un *hôte*, qui est responsable de la gestion du démarrage et de la durée de vie des applications.

Pour plus d'informations, consultez <xref:fundamentals/host/index>.

## <a name="servers"></a>Serveurs

Le modèle d’hébergement ASP.NET Core n’écoute pas directement les requêtes. Le modèle d’hébergement s’appuie sur une implémentation de serveur HTTP pour transférer la requête à l’application.

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Fenêtres](#tab/windows)

ASP.NET Core fournit les implémentations de serveur suivantes :

* Serveur [Kestrel](xref:fundamentals/servers/kestrel) : serveur web multiplateforme géré. Kestrel est souvent exécuté dans une configuration de proxy inverse à l’aide d’[IIS](https://www.iis.net/). Kestrel peut également être exécuté en tant que serveur périphérique public exposé directement à Internet dans ASP.NET Core 2.0 ou version ultérieure.
* Serveur IIS HTTP (`IISHttpServer`) est un [serveur in-process](xref:fundamentals/servers/index#in-process-hosting-model) pour IIS.
* Serveur [HTTP.sys](xref:fundamentals/servers/httpsys) : serveur web pour ASP.NET Core sur Windows.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core utilise l’implémentation du serveur [Kestrel](xref:fundamentals/servers/kestrel). Kestrel est un serveur web multiplateforme géré. Kestrel peut également être exécuté en tant que serveur périphérique public exposé directement à Internet dans ASP.NET Core 2.0 ou version ultérieure.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core utilise l’implémentation du serveur [Kestrel](xref:fundamentals/servers/kestrel). Kestrel est un serveur web multiplateforme géré. Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](http://nginx.org) ou [Apache](https://httpd.apache.org/). Kestrel peut également être exécuté en tant que serveur périphérique public exposé directement à Internet dans ASP.NET Core 2.0 ou version ultérieure.

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Fenêtres](#tab/windows)

ASP.NET Core fournit les implémentations de serveur suivantes :

* Serveur [Kestrel](xref:fundamentals/servers/kestrel) : serveur web multiplateforme géré. Kestrel est souvent exécuté dans une configuration de proxy inverse à l’aide d’[IIS](https://www.iis.net/). Kestrel peut également être exécuté en tant que serveur périphérique public exposé directement à Internet dans ASP.NET Core 2.0 ou version ultérieure.
* Serveur [HTTP.sys](xref:fundamentals/servers/httpsys) : serveur web pour ASP.NET Core sur Windows.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core utilise l’implémentation du serveur [Kestrel](xref:fundamentals/servers/kestrel). Kestrel est un serveur web multiplateforme géré. Kestrel peut également être exécuté en tant que serveur périphérique public exposé directement à Internet dans ASP.NET Core 2.0 ou version ultérieure.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core utilise l’implémentation du serveur [Kestrel](xref:fundamentals/servers/kestrel). Kestrel est un serveur web multiplateforme géré. Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](http://nginx.org) ou [Apache](https://httpd.apache.org/). Kestrel peut également être exécuté en tant que serveur périphérique public exposé directement à Internet dans ASP.NET Core 2.0 ou version ultérieure.

---

::: moniker-end

Pour plus d'informations, consultez <xref:fundamentals/servers/index>.

## <a name="configuration"></a>Configuration

ASP.NET Core utilise un modèle de configuration basé sur les paires nom-valeur. Le modèle de configuration n’est pas basé sur <xref:System.Configuration> ou *web.config*. La configuration obtient les paramètres d’une sélection de fournisseurs de configuration. Les fournisseurs de configuration intégrés prennent en charge une grande variété de formats de fichiers (XML, JSON, INI), de variables d’environnement et d’arguments de ligne de commande. Vous pouvez également écrire vos propres fournisseurs de configuration.

Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.

## <a name="logging"></a>Journalisation

ASP.NET Core prend en charge une API de journalisation qui fonctionne avec divers fournisseurs de journalisation. Les fournisseurs intégrés prennent en charge l’envoi de journaux à une ou plusieurs destinations. Des frameworks de journalisation tiers peuvent être utilisés.

Pour plus d'informations, consultez <xref:fundamentals/logging/index>.

## <a name="error-handling"></a>Gestion des erreurs

ASP.NET Core offre des scénarios intégrées pour la gestion des erreurs dans les applications, notamment une page d’exceptions du développeur, des pages d’erreurs personnalisées, des pages de code d’état statique et la gestion des exceptions de démarrage.

Pour plus d'informations, consultez <xref:fundamentals/error-handling>.

## <a name="routing"></a>Routage

ASP.NET Core offre des scénarios pour router les requêtes d’application vers les gestionnaires de routage.

Pour plus d'informations, consultez <xref:fundamentals/routing>.

## <a name="background-tasks"></a>Tâches en arrière-plan

Les tâches en arrière-plan sont implémentées en tant que *services hébergés*. Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>.

Pour plus d'informations, consultez <xref:fundamentals/host/hosted-services>.

## <a name="access-httpcontext"></a>Accéder à HttpContext

`HttpContext` est disponible automatiquement lors du traitement des requêtes avec Razor Pages et MVC. Dans les cas où `HttpContext` n’est pas disponible, vous pouvez accéder à `HttpContext` par le biais de l’interface <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> et de son implémentation par défaut, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.

Pour plus d'informations, consultez <xref:fundamentals/httpcontext>.
