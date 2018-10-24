---
title: Notions de base d’ASP.NET Core
author: rick-anderson
description: Découvrez les concepts de base permettant de créer des applications ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/20/2018
uid: fundamentals/index
ms.openlocfilehash: 83dfb5707700da01c45bae3c0c00e67ca397d402
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325469"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="b05f0-103">Notions de base d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b05f0-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="b05f0-104">Une application ASP.NET Core est une application console qui crée un serveur web dans sa méthode `Main` :</span><span class="sxs-lookup"><span data-stu-id="b05f0-104">An ASP.NET Core app is a console app that creates a web server in its `Main` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

<span data-ttu-id="b05f0-105">La méthode `Main` appelle [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), qui suit le [modèle de conception builder](https://wikipedia.org/wiki/Builder_pattern) pour créer un hôte web.</span><span class="sxs-lookup"><span data-stu-id="b05f0-105">The `Main` method invokes [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web host.</span></span> <span data-ttu-id="b05f0-106">Le builder a des méthodes qui définissent le serveur web (par exemple <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) et la classe de démarrage (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="b05f0-106">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="b05f0-107">Dans l’exemple précédent, le serveur web [Kestrel](xref:fundamentals/servers/kestrel) est alloué automatiquement.</span><span class="sxs-lookup"><span data-stu-id="b05f0-107">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="b05f0-108">L’hôte web d’ASP.NET Core tente de s’exécuter sur IIS, s’il est disponible.</span><span class="sxs-lookup"><span data-stu-id="b05f0-108">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="b05f0-109">D’autres serveurs web, tels que [HTTP.sys](xref:fundamentals/servers/httpsys), peuvent être utilisés via l’appel de la méthode d’extension appropriée.</span><span class="sxs-lookup"><span data-stu-id="b05f0-109">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="b05f0-110">`UseStartup` est expliqué de manière plus détaillée dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="b05f0-110">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="b05f0-111"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, le type de retour de l’appel de `WebHost.CreateDefaultBuilder`, fournit de nombreuses méthodes facultatives.</span><span class="sxs-lookup"><span data-stu-id="b05f0-111"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="b05f0-112">Certaines de ces méthodes incluent `UseHttpSys` pour l’hébergement de l’application dans HTTP.sys et <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> pour la spécification du répertoire de contenu racine.</span><span class="sxs-lookup"><span data-stu-id="b05f0-112">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="b05f0-113">Les méthodes <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> et <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> génèrent l’objet <xref:Microsoft.AspNetCore.Hosting.IWebHost>, qui héberge l’application et démarre l’écoute des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b05f0-113">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

<span data-ttu-id="b05f0-114">La méthode `Main` utilise <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, qui suit le [modèle de conception builder](https://wikipedia.org/wiki/Builder_pattern) pour créer un hôte d’application web.</span><span class="sxs-lookup"><span data-stu-id="b05f0-114">The `Main` method uses <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web app host.</span></span> <span data-ttu-id="b05f0-115">Le builder a des méthodes qui définissent le serveur web (par exemple <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) et la classe de démarrage (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="b05f0-115">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="b05f0-116">Dans l’exemple précédent, le serveur web [Kestrel](xref:fundamentals/servers/kestrel) est utilisé.</span><span class="sxs-lookup"><span data-stu-id="b05f0-116">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="b05f0-117">D’autres serveurs web, tels que [WebListener](xref:fundamentals/servers/weblistener), peuvent être utilisés via l’appel de la méthode d’extension appropriée.</span><span class="sxs-lookup"><span data-stu-id="b05f0-117">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="b05f0-118">`UseStartup` est expliqué de manière plus détaillée dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="b05f0-118">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="b05f0-119">`WebHostBuilder` fournit de nombreuses méthodes facultatives, notamment <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> pour l’hébergement dans IIS et IIS Express et <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> pour la spécification du répertoire de contenu racine.</span><span class="sxs-lookup"><span data-stu-id="b05f0-119">`WebHostBuilder` provides many optional methods, including <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> for hosting in IIS and IIS Express and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="b05f0-120">Les méthodes <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> et <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> génèrent l’objet <xref:Microsoft.AspNetCore.Hosting.IWebHost>, qui héberge l’application et démarre l’écoute des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b05f0-120">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

## <a name="startup"></a><span data-ttu-id="b05f0-121">Démarrage</span><span class="sxs-lookup"><span data-stu-id="b05f0-121">Startup</span></span>

<span data-ttu-id="b05f0-122">La méthode `UseStartup` sur `WebHostBuilder` spécifie la classe `Startup` pour votre application :</span><span class="sxs-lookup"><span data-stu-id="b05f0-122">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

<span data-ttu-id="b05f0-123">La classe `Startup` est l’emplacement où vous définissez le pipeline de traitement des requêtes et où sont configurés les services nécessaires à l’application.</span><span class="sxs-lookup"><span data-stu-id="b05f0-123">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="b05f0-124">La classe `Startup` doit être publique et contenir les méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="b05f0-124">The `Startup` class must be public and contain the following methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<span data-ttu-id="b05f0-125"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> définit les [services](#dependency-injection-services) utilisés par votre application (par exemple, ASP.NET Core MVC, Entity Framework Core et Identity).</span><span class="sxs-lookup"><span data-stu-id="b05f0-125"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="b05f0-126"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> définit les [intergiciels (middlewares)](xref:fundamentals/middleware/index) appelés dans le pipeline de requête.</span><span class="sxs-lookup"><span data-stu-id="b05f0-126"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> defines the [middleware](xref:fundamentals/middleware/index) called in the request pipeline.</span></span>

<span data-ttu-id="b05f0-127">Pour plus d'informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-127">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="content-root"></a><span data-ttu-id="b05f0-128">Racine de contenu</span><span class="sxs-lookup"><span data-stu-id="b05f0-128">Content root</span></span>

<span data-ttu-id="b05f0-129">La racine de contenu est le chemin de base de tout contenu utilisé par l’application, tel que les [pages Razor](xref:razor-pages/index), les vues MVC et les composants statiques.</span><span class="sxs-lookup"><span data-stu-id="b05f0-129">The content root is the base path to any content used by the app, such as [Razor Pages](xref:razor-pages/index), MVC views, and static assets.</span></span> <span data-ttu-id="b05f0-130">Par défaut, la racine de contenu est identique au chemin de base de l’application pour l’exécutable qui héberge l’application.</span><span class="sxs-lookup"><span data-stu-id="b05f0-130">By default, the content root is the same location as the app base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="b05f0-131">Racine web</span><span class="sxs-lookup"><span data-stu-id="b05f0-131">Web root</span></span>

<span data-ttu-id="b05f0-132">La racine web d’une application correspond au répertoire de projet qui contient les ressources publiques, statiques, telles que les fichiers CSS, JavaScript et image.</span><span class="sxs-lookup"><span data-stu-id="b05f0-132">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="b05f0-133">Injection de dépendances (services)</span><span class="sxs-lookup"><span data-stu-id="b05f0-133">Dependency injection (services)</span></span>

<span data-ttu-id="b05f0-134">Un *service* est un composant destiné à une consommation courante dans une application.</span><span class="sxs-lookup"><span data-stu-id="b05f0-134">A *service* is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="b05f0-135">Les services sont accessibles via l’[injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b05f0-135">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="b05f0-136">ASP.NET Core inclut un conteneur IoC (Inversion of Control) natif qui prend en charge l’[injection de constructeurs](xref:mvc/controllers/dependency-injection#constructor-injection) par défaut.</span><span class="sxs-lookup"><span data-stu-id="b05f0-136">ASP.NET Core includes a native Inversion of Control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="b05f0-137">Vous pouvez remplacer le conteneur par défaut si vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="b05f0-137">You can replace the default container if you wish.</span></span> <span data-ttu-id="b05f0-138">L’injection de dépendances n’offre pas seulement un [couplage faible](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), elle permet également d’accéder à des services (tels que la [journalisation](xref:fundamentals/logging/index)) dans l’ensemble de votre application.</span><span class="sxs-lookup"><span data-stu-id="b05f0-138">In addition to its [loose coupling benefit](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI makes services, such as [logging](xref:fundamentals/logging/index), available throughout your app.</span></span>

<span data-ttu-id="b05f0-139">Pour plus d'informations, consultez <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-139">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="b05f0-140">Intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="b05f0-140">Middleware</span></span>

<span data-ttu-id="b05f0-141">Dans ASP.NET Core, vous composez votre pipeline de requêtes à l’aide d’un [intergiciel (middleware)](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="b05f0-141">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="b05f0-142">Un middleware ASP.NET Core effectue des opérations asynchrones sur `HttpContext`, puis appelle le middleware suivant dans le pipeline ou met fin à la requête.</span><span class="sxs-lookup"><span data-stu-id="b05f0-142">ASP.NET Core middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="b05f0-143">Par convention, un composant de middleware appelé « XYZ » est ajouté au pipeline en appelant la méthode d’extension `UseXYZ` dans la méthode `Configure`.</span><span class="sxs-lookup"><span data-stu-id="b05f0-143">By convention, a middleware component called "XYZ" is added to the pipeline by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="b05f0-144">ASP.NET Core inclut un ensemble complet de middlewares intégrés, et vous pouvez écrire votre propre middleware personnalisé.</span><span class="sxs-lookup"><span data-stu-id="b05f0-144">ASP.NET Core includes a rich set of built-in middleware, and you can write your own custom middleware.</span></span> <span data-ttu-id="b05f0-145">[Open Web Interface pour .NET (OWIN)](xref:fundamentals/owin), qui permet aux applications web d’être dissociées des serveurs web, est pris en charge dans les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b05f0-145">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), which allows web apps to be decoupled from web servers, is supported in ASP.NET Core apps.</span></span>

<span data-ttu-id="b05f0-146">Pour plus d’informations, consultez <xref:fundamentals/middleware/index> et <xref:fundamentals/owin>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-146">For more information, see <xref:fundamentals/middleware/index> and <xref:fundamentals/owin>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="b05f0-147">Lancer des requêtes HTTP</span><span class="sxs-lookup"><span data-stu-id="b05f0-147">Initiate HTTP requests</span></span>

<span data-ttu-id="b05f0-148"><xref:System.Net.Http.IHttpClientFactory> est disponible pour accéder aux instances <xref:System.Net.Http.HttpClient> afin d’effectuer des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b05f0-148"><xref:System.Net.Http.IHttpClientFactory> is available to access <xref:System.Net.Http.HttpClient> instances to make HTTP requests.</span></span>

<span data-ttu-id="b05f0-149">Pour plus d'informations, consultez <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-149">For more information, see <xref:fundamentals/http-requests>.</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="b05f0-150">Environnements</span><span class="sxs-lookup"><span data-stu-id="b05f0-150">Environments</span></span>

<span data-ttu-id="b05f0-151">Les environnements, tels que *Développement* et *Production*, sont une notion de premier plan dans ASP.NET Core. Vous pouvez les définir à l’aide d’une variable d’environnement, d’un fichier de paramètres et d’un argument de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="b05f0-151">Environments, such as *Development* and *Production*, are a first-class notion in ASP.NET Core and can be set using an environment variable, settings file, and command-line argument.</span></span>

<span data-ttu-id="b05f0-152">Pour plus d'informations, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-152">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="hosting"></a><span data-ttu-id="b05f0-153">Hébergement</span><span class="sxs-lookup"><span data-stu-id="b05f0-153">Hosting</span></span>

<span data-ttu-id="b05f0-154">Les applications ASP.NET Core configurent et lancent un *hôte*, qui est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="b05f0-154">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="b05f0-155">Pour plus d'informations, consultez <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-155">For more information, see <xref:fundamentals/host/index>.</span></span>

## <a name="servers"></a><span data-ttu-id="b05f0-156">Serveurs</span><span class="sxs-lookup"><span data-stu-id="b05f0-156">Servers</span></span>

<span data-ttu-id="b05f0-157">Le modèle d’hébergement ASP.NET Core n’écoute pas directement les requêtes.</span><span class="sxs-lookup"><span data-stu-id="b05f0-157">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="b05f0-158">Le modèle d’hébergement s’appuie sur une implémentation de serveur HTTP pour transférer la requête à l’application.</span><span class="sxs-lookup"><span data-stu-id="b05f0-158">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="b05f0-159">La requête transférée est enveloppée (wrap) dans un ensemble d’objets de fonctionnalités auxquels vous pouvez accéder via des interfaces.</span><span class="sxs-lookup"><span data-stu-id="b05f0-159">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="b05f0-160">ASP.NET Core inclut un serveur web multiplateforme géré, appelé [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="b05f0-160">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="b05f0-161">Kestrel est généralement exécuté derrière un serveur web de production, tel qu’[IIS](https://www.iis.net/) ou [Nginx](http://nginx.org) dans une configuration de proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="b05f0-161">Kestrel is commonly run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org) in a reverse proxy configuration.</span></span> <span data-ttu-id="b05f0-162">Kestrel peut également être exécuté en tant que serveur périphérique public exposé directement à Internet dans ASP.NET Core 2.0 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="b05f0-162">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

<span data-ttu-id="b05f0-163">Pour plus d'informations, consultez <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-163">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="b05f0-164">Configuration</span><span class="sxs-lookup"><span data-stu-id="b05f0-164">Configuration</span></span>

<span data-ttu-id="b05f0-165">ASP.NET Core utilise un modèle de configuration basé sur les paires nom-valeur.</span><span class="sxs-lookup"><span data-stu-id="b05f0-165">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="b05f0-166">Le modèle de configuration n’est pas basé sur <xref:System.Configuration> ou *web.config*. La configuration obtient les paramètres d’une sélection de fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="b05f0-166">The configuration model isn't based on <xref:System.Configuration> or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="b05f0-167">Les fournisseurs de configuration intégrés prennent en charge une grande variété de formats de fichiers (XML, JSON, INI), de variables d’environnement et d’arguments de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="b05f0-167">The built-in configuration providers support a variety of file formats (XML, JSON, INI), environment variables, and command-line arguments.</span></span> <span data-ttu-id="b05f0-168">Vous pouvez également écrire vos propres fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="b05f0-168">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="b05f0-169">Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-169">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="logging"></a><span data-ttu-id="b05f0-170">Journalisation</span><span class="sxs-lookup"><span data-stu-id="b05f0-170">Logging</span></span>

<span data-ttu-id="b05f0-171">ASP.NET Core prend en charge une API de journalisation qui fonctionne avec divers fournisseurs de journalisation.</span><span class="sxs-lookup"><span data-stu-id="b05f0-171">ASP.NET Core supports a Logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="b05f0-172">Les fournisseurs intégrés prennent en charge l’envoi de journaux à une ou plusieurs destinations.</span><span class="sxs-lookup"><span data-stu-id="b05f0-172">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="b05f0-173">Des frameworks de journalisation tiers peuvent être utilisés.</span><span class="sxs-lookup"><span data-stu-id="b05f0-173">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="b05f0-174">Pour plus d'informations, consultez <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-174">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="b05f0-175">Gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="b05f0-175">Error handling</span></span>

<span data-ttu-id="b05f0-176">ASP.NET Core offre des scénarios intégrées pour la gestion des erreurs dans les applications, notamment une page d’exceptions du développeur, des pages d’erreurs personnalisées, des pages de code d’état statique et la gestion des exceptions de démarrage.</span><span class="sxs-lookup"><span data-stu-id="b05f0-176">ASP.NET Core has built-in scenarios for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="b05f0-177">Pour plus d'informations, consultez <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-177">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="routing"></a><span data-ttu-id="b05f0-178">Routage</span><span class="sxs-lookup"><span data-stu-id="b05f0-178">Routing</span></span>

<span data-ttu-id="b05f0-179">ASP.NET Core offre des scénarios pour router les requêtes d’application vers les gestionnaires de routage.</span><span class="sxs-lookup"><span data-stu-id="b05f0-179">ASP.NET Core offers scenarios for routing of app requests to route handlers.</span></span>

<span data-ttu-id="b05f0-180">Pour plus d'informations, consultez <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-180">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="file-providers"></a><span data-ttu-id="b05f0-181">Fournisseurs de fichiers</span><span class="sxs-lookup"><span data-stu-id="b05f0-181">File Providers</span></span>

<span data-ttu-id="b05f0-182">ASP.NET Core fournit un accès au système de fichiers en utilisant des fournisseurs de fichiers qui offrent une interface commune permettant de travailler avec des fichiers sur plusieurs plateformes.</span><span class="sxs-lookup"><span data-stu-id="b05f0-182">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="b05f0-183">Pour plus d'informations, consultez <xref:fundamentals/file-providers>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-183">For more information, see <xref:fundamentals/file-providers>.</span></span>

## <a name="static-files"></a><span data-ttu-id="b05f0-184">Fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="b05f0-184">Static files</span></span>

<span data-ttu-id="b05f0-185">Le middleware Fichiers statiques prend en charge des fichiers statiques, tels que les fichiers HTML, CSS, image et JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b05f0-185">Static Files Middleware serves static files, such as HTML, CSS, image, and JavaScript files.</span></span>

<span data-ttu-id="b05f0-186">Pour plus d'informations, consultez <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-186">For more information, see <xref:fundamentals/static-files>.</span></span>

## <a name="session-and-app-state"></a><span data-ttu-id="b05f0-187">État de session et d’application</span><span class="sxs-lookup"><span data-stu-id="b05f0-187">Session and app state</span></span>

<span data-ttu-id="b05f0-188">ASP.NET Core offre plusieurs approches pour conserver l’état de session et d’application pendant qu’un utilisateur parcourt une application web.</span><span class="sxs-lookup"><span data-stu-id="b05f0-188">ASP.NET Core offers several approaches to preserve session and app state while a user browses a web app.</span></span>

<span data-ttu-id="b05f0-189">Pour plus d'informations, consultez <xref:fundamentals/app-state>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-189">For more information, see <xref:fundamentals/app-state>.</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="b05f0-190">Globalisation et localisation</span><span class="sxs-lookup"><span data-stu-id="b05f0-190">Globalization and localization</span></span>

<span data-ttu-id="b05f0-191">La création d’un site web multilingue avec ASP.NET Core vous permet d’atteindre un plus large public.</span><span class="sxs-lookup"><span data-stu-id="b05f0-191">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="b05f0-192">ASP.NET Core fournit des services et des middlewares pour localiser du contenu dans différentes langues et cultures.</span><span class="sxs-lookup"><span data-stu-id="b05f0-192">ASP.NET Core provides services and middleware for localizing content into different languages and cultures.</span></span>

<span data-ttu-id="b05f0-193">Pour plus d'informations, consultez <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-193">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="request-features"></a><span data-ttu-id="b05f0-194">Fonctionnalités de requête</span><span class="sxs-lookup"><span data-stu-id="b05f0-194">Request features</span></span>

<span data-ttu-id="b05f0-195">Les détails d’implémentation d’un serveur web relatifs aux requêtes HTTP et aux réponses sont définis dans les interfaces.</span><span class="sxs-lookup"><span data-stu-id="b05f0-195">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="b05f0-196">Ces interfaces sont utilisées par les implémentations de serveur et les intergiciels (middleware) pour créer et modifier le pipeline d’hébergement de l’application.</span><span class="sxs-lookup"><span data-stu-id="b05f0-196">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="b05f0-197">Pour plus d'informations, consultez <xref:fundamentals/request-features>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-197">For more information, see <xref:fundamentals/request-features>.</span></span>

## <a name="background-tasks"></a><span data-ttu-id="b05f0-198">Tâches en arrière-plan</span><span class="sxs-lookup"><span data-stu-id="b05f0-198">Background tasks</span></span>

<span data-ttu-id="b05f0-199">Les tâches en arrière-plan sont implémentées en tant que *services hébergés*.</span><span class="sxs-lookup"><span data-stu-id="b05f0-199">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="b05f0-200">Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-200">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span>

<span data-ttu-id="b05f0-201">Pour plus d'informations, consultez <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-201">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="b05f0-202">Accéder à HttpContext</span><span class="sxs-lookup"><span data-stu-id="b05f0-202">Access HttpContext</span></span>

<span data-ttu-id="b05f0-203">`HttpContext` est disponible automatiquement lors du traitement des requêtes avec Razor Pages et MVC.</span><span class="sxs-lookup"><span data-stu-id="b05f0-203">`HttpContext` is automatically available when processing requests with Razor Pages and MVC.</span></span> <span data-ttu-id="b05f0-204">Dans les cas où `HttpContext` n’est pas disponible, vous pouvez accéder à `HttpContext` par le biais de l’interface <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> et de son implémentation par défaut, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-204">In circumstances where `HttpContext` isn't readily available, you can access the `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span>

<span data-ttu-id="b05f0-205">Pour plus d'informations, consultez <xref:fundamentals/httpcontext>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-205">For more information, see <xref:fundamentals/httpcontext>.</span></span>

## <a name="websockets"></a><span data-ttu-id="b05f0-206">WebSockets</span><span class="sxs-lookup"><span data-stu-id="b05f0-206">WebSockets</span></span>

<span data-ttu-id="b05f0-207">[WebSocket](https://wikipedia.org/wiki/WebSocket) est un protocole qui autorise des canaux de communication persistants bidirectionnels sur les connexions TCP.</span><span class="sxs-lookup"><span data-stu-id="b05f0-207">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="b05f0-208">Il est utilisé pour des applications comme le chat, les transactions boursières, les jeux et partout où vous voulez des fonctionnalités en temps réel dans une application web.</span><span class="sxs-lookup"><span data-stu-id="b05f0-208">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="b05f0-209">ASP.NET Core prend en charge des scénarios de socket web.</span><span class="sxs-lookup"><span data-stu-id="b05f0-209">ASP.NET Core supports web socket scenarios.</span></span>

<span data-ttu-id="b05f0-210">Pour plus d'informations, consultez <xref:fundamentals/websockets>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-210">For more information, see <xref:fundamentals/websockets>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="microsoftaspnetcoreapp-metapackage"></a><span data-ttu-id="b05f0-211">Métapackage Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="b05f0-211">Microsoft.AspNetCore.App metapackage</span></span>

<span data-ttu-id="b05f0-212">Le métapackage [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) simplifie la gestion des packages.</span><span class="sxs-lookup"><span data-stu-id="b05f0-212">The [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) metapackage simplifies package management.</span></span>

<span data-ttu-id="b05f0-213">Pour plus d'informations, consultez <xref:fundamentals/metapackage-app>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-213">For more information, see <xref:fundamentals/metapackage-app>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="b05f0-214">Métapackage Microsoft.AspNetCore.All</span><span class="sxs-lookup"><span data-stu-id="b05f0-214">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="b05f0-215">Le métapackage [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) pour ASP.NET Core comprend :</span><span class="sxs-lookup"><span data-stu-id="b05f0-215">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="b05f0-216">Tous les packages pris en charge par l’équipe ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b05f0-216">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="b05f0-217">Tous les packages pris en charge par Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b05f0-217">All supported packages by Entity Framework Core.</span></span>
* <span data-ttu-id="b05f0-218">Les dépendances internes et tierces utilisées par ASP.NET Core et Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b05f0-218">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="b05f0-219">Pour plus d'informations, consultez <xref:fundamentals/metapackage>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-219">For more information, see <xref:fundamentals/metapackage>.</span></span>

::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="b05f0-220">Runtime .NET Core et runtime .NET Framework</span><span class="sxs-lookup"><span data-stu-id="b05f0-220">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="b05f0-221">Une application ASP.NET Core peut cibler le runtime .NET Core ou .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b05f0-221">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="b05f0-222">Pour plus d’informations, consultez [Choix entre .NET Core et .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="b05f0-222">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="b05f0-223">Choisir entre ASP.NET Core et ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b05f0-223">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="b05f0-224">Pour plus d’informations sur le choix entre ASP.NET Core et ASP.NET, consultez <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.</span><span class="sxs-lookup"><span data-stu-id="b05f0-224">For more information on choosing between ASP.NET Core and ASP.NET, see <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.</span></span>
