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
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="e3301-103">Notions de base d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e3301-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="e3301-104">Une application ASP.NET Core est une application console qui crée un serveur web dans sa méthode `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="e3301-104">An ASP.NET Core app is a console app that creates a web server in its `Program.Main` method.</span></span> <span data-ttu-id="e3301-105">La méthode `Main` est le *point d’entrée managé* de l’application :</span><span class="sxs-lookup"><span data-stu-id="e3301-105">The `Main` method is the app's *managed entry point*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

<span data-ttu-id="e3301-106">L’hôte .NET Core :</span><span class="sxs-lookup"><span data-stu-id="e3301-106">The .NET Core Host:</span></span>

* <span data-ttu-id="e3301-107">Charge le [runtime .NET Core](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="e3301-107">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="e3301-108">Utilise le premier argument de ligne de commande comme chemin du fichier binaire managé qui contient le point d’entrée (`Main`) et commence l’exécution de code.</span><span class="sxs-lookup"><span data-stu-id="e3301-108">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="e3301-109">La méthode `Main` appelle [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), qui suit le [modèle de conception builder](https://wikipedia.org/wiki/Builder_pattern) pour créer un hôte web.</span><span class="sxs-lookup"><span data-stu-id="e3301-109">The `Main` method invokes [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web host.</span></span> <span data-ttu-id="e3301-110">Le builder a des méthodes qui définissent un serveur web (par exemple, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) et la classe de démarrage (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="e3301-110">The builder has methods that define a web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="e3301-111">Dans l’exemple précédent, le serveur web [Kestrel](xref:fundamentals/servers/kestrel) est alloué automatiquement.</span><span class="sxs-lookup"><span data-stu-id="e3301-111">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="e3301-112">L’hôte web d’ASP.NET Core tente de s’exécuter sur [IIS (Internet Information Services)](https://www.iis.net/), s’il est disponible.</span><span class="sxs-lookup"><span data-stu-id="e3301-112">ASP.NET Core's web host attempts to run on [Internet Information Services (IIS)](https://www.iis.net/), if available.</span></span> <span data-ttu-id="e3301-113">D’autres serveurs web, tels que [HTTP.sys](xref:fundamentals/servers/httpsys), peuvent être utilisés via l’appel de la méthode d’extension appropriée.</span><span class="sxs-lookup"><span data-stu-id="e3301-113">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="e3301-114">`UseStartup` est expliqué de manière plus détaillée dans la section [Démarrage](#startup).</span><span class="sxs-lookup"><span data-stu-id="e3301-114">`UseStartup` is explained further in the [Startup](#startup) section.</span></span>

<span data-ttu-id="e3301-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, le type de retour de l’appel de `WebHost.CreateDefaultBuilder`, fournit de nombreuses méthodes facultatives.</span><span class="sxs-lookup"><span data-stu-id="e3301-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="e3301-116">Certaines de ces méthodes incluent `UseHttpSys` pour l’hébergement de l’application dans HTTP.sys et <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> pour la spécification du répertoire de contenu racine.</span><span class="sxs-lookup"><span data-stu-id="e3301-116">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="e3301-117">Les méthodes <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> et <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> génèrent l’objet <xref:Microsoft.AspNetCore.Hosting.IWebHost>, qui héberge l’application et démarre l’écoute des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="e3301-117">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

<span data-ttu-id="e3301-118">L’hôte .NET Core :</span><span class="sxs-lookup"><span data-stu-id="e3301-118">The .NET Core Host:</span></span>

* <span data-ttu-id="e3301-119">Charge le [runtime .NET Core](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="e3301-119">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="e3301-120">Utilise le premier argument de ligne de commande comme chemin du fichier binaire managé qui contient le point d’entrée (`Main`) et commence l’exécution de code.</span><span class="sxs-lookup"><span data-stu-id="e3301-120">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="e3301-121">La méthode `Main` utilise <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, qui suit le [modèle de conception builder](https://wikipedia.org/wiki/Builder_pattern) pour créer un hôte d’application web.</span><span class="sxs-lookup"><span data-stu-id="e3301-121">The `Main` method uses <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web app host.</span></span> <span data-ttu-id="e3301-122">Le builder a des méthodes qui définissent le serveur web (par exemple <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) et la classe de démarrage (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="e3301-122">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="e3301-123">Dans l’exemple précédent, le serveur web [Kestrel](xref:fundamentals/servers/kestrel) est utilisé.</span><span class="sxs-lookup"><span data-stu-id="e3301-123">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="e3301-124">D’autres serveurs web, tels que [HTTP.sys](xref:fundamentals/servers/httpsys), peuvent être utilisés via l’appel de la méthode d’extension appropriée.</span><span class="sxs-lookup"><span data-stu-id="e3301-124">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="e3301-125">`UseStartup` est expliqué de manière plus détaillée dans la section [Démarrage](#startup).</span><span class="sxs-lookup"><span data-stu-id="e3301-125">`UseStartup` is explained further in the [Startup](#startup) section.</span></span>

<span data-ttu-id="e3301-126">`WebHostBuilder` fournit de nombreuses méthodes facultatives, notamment <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> pour l’hébergement dans IIS et IIS Express et <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> pour la spécification du répertoire de contenu racine.</span><span class="sxs-lookup"><span data-stu-id="e3301-126">`WebHostBuilder` provides many optional methods, including <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> for hosting in IIS and IIS Express and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="e3301-127">Les méthodes <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> et <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> génèrent l’objet <xref:Microsoft.AspNetCore.Hosting.IWebHost>, qui héberge l’application et démarre l’écoute des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="e3301-127">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

## <a name="startup"></a><span data-ttu-id="e3301-128">Démarrage</span><span class="sxs-lookup"><span data-stu-id="e3301-128">Startup</span></span>

<span data-ttu-id="e3301-129">La méthode `UseStartup` sur `WebHostBuilder` spécifie la classe `Startup` pour votre application :</span><span class="sxs-lookup"><span data-stu-id="e3301-129">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

<span data-ttu-id="e3301-130">C’est dans la classe `Startup` que sont configurés les services nécessaires à l’application et qu’est défini le pipeline de traitement des requêtes.</span><span class="sxs-lookup"><span data-stu-id="e3301-130">The `Startup` class is where any services required by the app are configured and the request handling pipeline is defined.</span></span> <span data-ttu-id="e3301-131">La classe `Startup`, qui doit être publique, contient en général les méthodes suivantes.</span><span class="sxs-lookup"><span data-stu-id="e3301-131">The `Startup` class must be public and usually contains the following methods.</span></span> <span data-ttu-id="e3301-132">`Startup.ConfigureServices` est facultatif.</span><span class="sxs-lookup"><span data-stu-id="e3301-132">`Startup.ConfigureServices` is optional.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<span data-ttu-id="e3301-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> définit les [services](#dependency-injection-services) utilisés par votre application (par exemple, ASP.NET Core MVC, Entity Framework Core et Identity).</span><span class="sxs-lookup"><span data-stu-id="e3301-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="e3301-134"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> définit les [intergiciels (middlewares)](xref:fundamentals/middleware/index) appelés dans le pipeline de requête.</span><span class="sxs-lookup"><span data-stu-id="e3301-134"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> defines the [middleware](xref:fundamentals/middleware/index) called in the request pipeline.</span></span>

<span data-ttu-id="e3301-135">Pour plus d'informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="e3301-135">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="content-root"></a><span data-ttu-id="e3301-136">Racine de contenu</span><span class="sxs-lookup"><span data-stu-id="e3301-136">Content root</span></span>

<span data-ttu-id="e3301-137">La racine de contenu est le chemin de base de tout contenu utilisé par l’application, tel que les [pages Razor](xref:razor-pages/index), les vues MVC et les composants statiques.</span><span class="sxs-lookup"><span data-stu-id="e3301-137">The content root is the base path to any content used by the app, such as [Razor Pages](xref:razor-pages/index), MVC views, and static assets.</span></span> <span data-ttu-id="e3301-138">Par défaut, la racine de contenu est identique au chemin de base de l’application pour l’exécutable qui héberge l’application.</span><span class="sxs-lookup"><span data-stu-id="e3301-138">By default, the content root is the same location as the app base path for the executable hosting the app.</span></span>

## <a name="web-root-webroot"></a><span data-ttu-id="e3301-139">Racine web (webroot)</span><span class="sxs-lookup"><span data-stu-id="e3301-139">Web root (webroot)</span></span>

<span data-ttu-id="e3301-140">La racine web d’une application correspond au répertoire de projet qui contient les ressources publiques statiques, telles que les fichiers CSS, JavaScript et image.</span><span class="sxs-lookup"><span data-stu-id="e3301-140">The webroot of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="e3301-141">Par défaut, *wwwroot* est la racine web.</span><span class="sxs-lookup"><span data-stu-id="e3301-141">By default, *wwwroot* is the webroot.</span></span>

<span data-ttu-id="e3301-142">Pour les fichiers Razor (*.cshtml*), la barre oblique tilde `~/` pointe vers la racine web.</span><span class="sxs-lookup"><span data-stu-id="e3301-142">For Razor (*.cshtml*) files, the tilde-slash  `~/` points to the webroot.</span></span> <span data-ttu-id="e3301-143">Les chemins commençant par `~/` sont appelés chemins virtuels.</span><span class="sxs-lookup"><span data-stu-id="e3301-143">Paths beginning with `~/` are referred to as virtual paths.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="e3301-144">Injection de dépendances (services)</span><span class="sxs-lookup"><span data-stu-id="e3301-144">Dependency injection (services)</span></span>

<span data-ttu-id="e3301-145">Un *service* est un composant destiné à une consommation courante dans une application.</span><span class="sxs-lookup"><span data-stu-id="e3301-145">A *service* is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="e3301-146">Les services sont accessibles via l’[injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e3301-146">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e3301-147">ASP.NET Core inclut un conteneur IoC (Inversion of Control) natif qui prend en charge l’[injection de constructeurs](xref:mvc/controllers/dependency-injection#constructor-injection) par défaut.</span><span class="sxs-lookup"><span data-stu-id="e3301-147">ASP.NET Core includes a native Inversion of Control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="e3301-148">Vous pouvez remplacer le conteneur par défaut si vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="e3301-148">You can replace the default container if you wish.</span></span> <span data-ttu-id="e3301-149">L’injection de dépendances n’offre pas seulement un [couplage faible](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), elle permet également d’accéder à des services (tels que la [journalisation](xref:fundamentals/logging/index)) dans l’ensemble de votre application.</span><span class="sxs-lookup"><span data-stu-id="e3301-149">In addition to its [loose coupling benefit](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI makes services, such as [logging](xref:fundamentals/logging/index), available throughout your app.</span></span>

<span data-ttu-id="e3301-150">Pour plus d'informations, consultez <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="e3301-150">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="e3301-151">Intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="e3301-151">Middleware</span></span>

<span data-ttu-id="e3301-152">Dans ASP.NET Core, vous composez votre pipeline de requêtes à l’aide d’un [intergiciel (middleware)](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="e3301-152">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="e3301-153">Un middleware ASP.NET Core effectue des opérations asynchrones sur `HttpContext`, puis appelle le middleware suivant dans le pipeline ou met fin à la requête.</span><span class="sxs-lookup"><span data-stu-id="e3301-153">ASP.NET Core middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="e3301-154">Par convention, un composant de middleware appelé « XYZ » est ajouté au pipeline en appelant la méthode d’extension `UseXYZ` dans la méthode `Configure`.</span><span class="sxs-lookup"><span data-stu-id="e3301-154">By convention, a middleware component called "XYZ" is added to the pipeline by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="e3301-155">ASP.NET Core inclut un ensemble complet de middlewares intégrés, et vous pouvez écrire votre propre middleware personnalisé.</span><span class="sxs-lookup"><span data-stu-id="e3301-155">ASP.NET Core includes a rich set of built-in middleware, and you can write your own custom middleware.</span></span> <span data-ttu-id="e3301-156">[Open Web Interface pour .NET (OWIN)](xref:fundamentals/owin), qui permet aux applications web d’être dissociées des serveurs web, est pris en charge dans les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e3301-156">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), which allows web apps to be decoupled from web servers, is supported in ASP.NET Core apps.</span></span>

<span data-ttu-id="e3301-157">Pour plus d’informations, consultez <xref:fundamentals/middleware/index> et <xref:fundamentals/owin>.</span><span class="sxs-lookup"><span data-stu-id="e3301-157">For more information, see <xref:fundamentals/middleware/index> and <xref:fundamentals/owin>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="e3301-158">Lancer des requêtes HTTP</span><span class="sxs-lookup"><span data-stu-id="e3301-158">Initiate HTTP requests</span></span>

<span data-ttu-id="e3301-159"><xref:System.Net.Http.IHttpClientFactory> est disponible pour accéder aux instances <xref:System.Net.Http.HttpClient> afin d’effectuer des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="e3301-159"><xref:System.Net.Http.IHttpClientFactory> is available to access <xref:System.Net.Http.HttpClient> instances to make HTTP requests.</span></span>

<span data-ttu-id="e3301-160">Pour plus d'informations, consultez <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="e3301-160">For more information, see <xref:fundamentals/http-requests>.</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="e3301-161">Environnements</span><span class="sxs-lookup"><span data-stu-id="e3301-161">Environments</span></span>

<span data-ttu-id="e3301-162">Les environnements, tels que *Développement* et *Production*, sont une notion de premier plan dans ASP.NET Core. Vous pouvez les définir à l’aide d’une variable d’environnement, d’un fichier de paramètres et d’un argument de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="e3301-162">Environments, such as *Development* and *Production*, are a first-class notion in ASP.NET Core and can be set using an environment variable, settings file, and command-line argument.</span></span>

<span data-ttu-id="e3301-163">Pour plus d'informations, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="e3301-163">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="hosting"></a><span data-ttu-id="e3301-164">Hébergement</span><span class="sxs-lookup"><span data-stu-id="e3301-164">Hosting</span></span>

<span data-ttu-id="e3301-165">Les applications ASP.NET Core configurent et lancent un *hôte*, qui est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="e3301-165">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="e3301-166">Pour plus d'informations, consultez <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="e3301-166">For more information, see <xref:fundamentals/host/index>.</span></span>

## <a name="servers"></a><span data-ttu-id="e3301-167">Serveurs</span><span class="sxs-lookup"><span data-stu-id="e3301-167">Servers</span></span>

<span data-ttu-id="e3301-168">Le modèle d’hébergement ASP.NET Core n’écoute pas directement les requêtes.</span><span class="sxs-lookup"><span data-stu-id="e3301-168">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="e3301-169">Le modèle d’hébergement s’appuie sur une implémentation de serveur HTTP pour transférer la requête à l’application.</span><span class="sxs-lookup"><span data-stu-id="e3301-169">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="e3301-170">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="e3301-170">Windows</span></span>](#tab/windows)

<span data-ttu-id="e3301-171">ASP.NET Core fournit les implémentations de serveur suivantes :</span><span class="sxs-lookup"><span data-stu-id="e3301-171">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="e3301-172">Serveur [Kestrel](xref:fundamentals/servers/kestrel) : serveur web multiplateforme géré.</span><span class="sxs-lookup"><span data-stu-id="e3301-172">[Kestrel](xref:fundamentals/servers/kestrel) server is a managed, cross-platform web server.</span></span> <span data-ttu-id="e3301-173">Kestrel est souvent exécuté dans une configuration de proxy inverse à l’aide d’[IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="e3301-173">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="e3301-174">Kestrel peut également être exécuté en tant que serveur périphérique public exposé directement à Internet dans ASP.NET Core 2.0 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="e3301-174">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="e3301-175">Serveur IIS HTTP (`IISHttpServer`) est un [serveur in-process](xref:fundamentals/servers/index#in-process-hosting-model) pour IIS.</span><span class="sxs-lookup"><span data-stu-id="e3301-175">IIS HTTP Server (`IISHttpServer`) is an [in-process server](xref:fundamentals/servers/index#in-process-hosting-model) for IIS.</span></span>
* <span data-ttu-id="e3301-176">Serveur [HTTP.sys](xref:fundamentals/servers/httpsys) : serveur web pour ASP.NET Core sur Windows.</span><span class="sxs-lookup"><span data-stu-id="e3301-176">[HTTP.sys](xref:fundamentals/servers/httpsys) server is a web server for ASP.NET Core on Windows.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="e3301-177">macOS</span><span class="sxs-lookup"><span data-stu-id="e3301-177">macOS</span></span>](#tab/macos)

<span data-ttu-id="e3301-178">ASP.NET Core utilise l’implémentation du serveur [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="e3301-178">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="e3301-179">Kestrel est un serveur web multiplateforme géré.</span><span class="sxs-lookup"><span data-stu-id="e3301-179">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="e3301-180">Kestrel peut également être exécuté en tant que serveur périphérique public exposé directement à Internet dans ASP.NET Core 2.0 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="e3301-180">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="e3301-181">Linux</span><span class="sxs-lookup"><span data-stu-id="e3301-181">Linux</span></span>](#tab/linux)

<span data-ttu-id="e3301-182">ASP.NET Core utilise l’implémentation du serveur [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="e3301-182">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="e3301-183">Kestrel est un serveur web multiplateforme géré.</span><span class="sxs-lookup"><span data-stu-id="e3301-183">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="e3301-184">Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](http://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="e3301-184">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="e3301-185">Kestrel peut également être exécuté en tant que serveur périphérique public exposé directement à Internet dans ASP.NET Core 2.0 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="e3301-185">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="e3301-186">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="e3301-186">Windows</span></span>](#tab/windows)

<span data-ttu-id="e3301-187">ASP.NET Core fournit les implémentations de serveur suivantes :</span><span class="sxs-lookup"><span data-stu-id="e3301-187">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="e3301-188">Serveur [Kestrel](xref:fundamentals/servers/kestrel) : serveur web multiplateforme géré.</span><span class="sxs-lookup"><span data-stu-id="e3301-188">[Kestrel](xref:fundamentals/servers/kestrel) server is a managed, cross-platform web server.</span></span> <span data-ttu-id="e3301-189">Kestrel est souvent exécuté dans une configuration de proxy inverse à l’aide d’[IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="e3301-189">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="e3301-190">Kestrel peut également être exécuté en tant que serveur périphérique public exposé directement à Internet dans ASP.NET Core 2.0 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="e3301-190">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="e3301-191">Serveur [HTTP.sys](xref:fundamentals/servers/httpsys) : serveur web pour ASP.NET Core sur Windows.</span><span class="sxs-lookup"><span data-stu-id="e3301-191">[HTTP.sys](xref:fundamentals/servers/httpsys) server is a web server for ASP.NET Core on Windows.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="e3301-192">macOS</span><span class="sxs-lookup"><span data-stu-id="e3301-192">macOS</span></span>](#tab/macos)

<span data-ttu-id="e3301-193">ASP.NET Core utilise l’implémentation du serveur [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="e3301-193">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="e3301-194">Kestrel est un serveur web multiplateforme géré.</span><span class="sxs-lookup"><span data-stu-id="e3301-194">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="e3301-195">Kestrel peut également être exécuté en tant que serveur périphérique public exposé directement à Internet dans ASP.NET Core 2.0 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="e3301-195">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="e3301-196">Linux</span><span class="sxs-lookup"><span data-stu-id="e3301-196">Linux</span></span>](#tab/linux)

<span data-ttu-id="e3301-197">ASP.NET Core utilise l’implémentation du serveur [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="e3301-197">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="e3301-198">Kestrel est un serveur web multiplateforme géré.</span><span class="sxs-lookup"><span data-stu-id="e3301-198">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="e3301-199">Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](http://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="e3301-199">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="e3301-200">Kestrel peut également être exécuté en tant que serveur périphérique public exposé directement à Internet dans ASP.NET Core 2.0 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="e3301-200">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

---

::: moniker-end

<span data-ttu-id="e3301-201">Pour plus d'informations, consultez <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="e3301-201">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="e3301-202">Configuration</span><span class="sxs-lookup"><span data-stu-id="e3301-202">Configuration</span></span>

<span data-ttu-id="e3301-203">ASP.NET Core utilise un modèle de configuration basé sur les paires nom-valeur.</span><span class="sxs-lookup"><span data-stu-id="e3301-203">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="e3301-204">Le modèle de configuration n’est pas basé sur <xref:System.Configuration> ou *web.config*. La configuration obtient les paramètres d’une sélection de fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="e3301-204">The configuration model isn't based on <xref:System.Configuration> or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="e3301-205">Les fournisseurs de configuration intégrés prennent en charge une grande variété de formats de fichiers (XML, JSON, INI), de variables d’environnement et d’arguments de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="e3301-205">The built-in configuration providers support a variety of file formats (XML, JSON, INI), environment variables, and command-line arguments.</span></span> <span data-ttu-id="e3301-206">Vous pouvez également écrire vos propres fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="e3301-206">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="e3301-207">Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="e3301-207">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="logging"></a><span data-ttu-id="e3301-208">Journalisation</span><span class="sxs-lookup"><span data-stu-id="e3301-208">Logging</span></span>

<span data-ttu-id="e3301-209">ASP.NET Core prend en charge une API de journalisation qui fonctionne avec divers fournisseurs de journalisation.</span><span class="sxs-lookup"><span data-stu-id="e3301-209">ASP.NET Core supports a Logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="e3301-210">Les fournisseurs intégrés prennent en charge l’envoi de journaux à une ou plusieurs destinations.</span><span class="sxs-lookup"><span data-stu-id="e3301-210">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="e3301-211">Des frameworks de journalisation tiers peuvent être utilisés.</span><span class="sxs-lookup"><span data-stu-id="e3301-211">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="e3301-212">Pour plus d'informations, consultez <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="e3301-212">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="e3301-213">Gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="e3301-213">Error handling</span></span>

<span data-ttu-id="e3301-214">ASP.NET Core offre des scénarios intégrées pour la gestion des erreurs dans les applications, notamment une page d’exceptions du développeur, des pages d’erreurs personnalisées, des pages de code d’état statique et la gestion des exceptions de démarrage.</span><span class="sxs-lookup"><span data-stu-id="e3301-214">ASP.NET Core has built-in scenarios for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="e3301-215">Pour plus d'informations, consultez <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="e3301-215">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="routing"></a><span data-ttu-id="e3301-216">Routage</span><span class="sxs-lookup"><span data-stu-id="e3301-216">Routing</span></span>

<span data-ttu-id="e3301-217">ASP.NET Core offre des scénarios pour router les requêtes d’application vers les gestionnaires de routage.</span><span class="sxs-lookup"><span data-stu-id="e3301-217">ASP.NET Core offers scenarios for routing of app requests to route handlers.</span></span>

<span data-ttu-id="e3301-218">Pour plus d'informations, consultez <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="e3301-218">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="background-tasks"></a><span data-ttu-id="e3301-219">Tâches en arrière-plan</span><span class="sxs-lookup"><span data-stu-id="e3301-219">Background tasks</span></span>

<span data-ttu-id="e3301-220">Les tâches en arrière-plan sont implémentées en tant que *services hébergés*.</span><span class="sxs-lookup"><span data-stu-id="e3301-220">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="e3301-221">Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="e3301-221">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span>

<span data-ttu-id="e3301-222">Pour plus d'informations, consultez <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="e3301-222">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="e3301-223">Accéder à HttpContext</span><span class="sxs-lookup"><span data-stu-id="e3301-223">Access HttpContext</span></span>

<span data-ttu-id="e3301-224">`HttpContext` est disponible automatiquement lors du traitement des requêtes avec Razor Pages et MVC.</span><span class="sxs-lookup"><span data-stu-id="e3301-224">`HttpContext` is automatically available when processing requests with Razor Pages and MVC.</span></span> <span data-ttu-id="e3301-225">Dans les cas où `HttpContext` n’est pas disponible, vous pouvez accéder à `HttpContext` par le biais de l’interface <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> et de son implémentation par défaut, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span><span class="sxs-lookup"><span data-stu-id="e3301-225">In circumstances where `HttpContext` isn't readily available, you can access the `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span>

<span data-ttu-id="e3301-226">Pour plus d'informations, consultez <xref:fundamentals/httpcontext>.</span><span class="sxs-lookup"><span data-stu-id="e3301-226">For more information, see <xref:fundamentals/httpcontext>.</span></span>
