---
title: Notions de base d’ASP.NET Core
author: rick-anderson
description: Découvrez les concepts de base permettant de créer des applications ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: fundamentals/index
ms.openlocfilehash: 11dc6336ae7667038983c967f28232bef325f5bb
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637766"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="e63e8-103">Notions de base d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e63e8-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="e63e8-104">Une application ASP.NET Core est une application console qui crée un serveur web dans sa méthode `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="e63e8-104">An ASP.NET Core app is a console app that creates a web server in its `Program.Main` method.</span></span> <span data-ttu-id="e63e8-105">La méthode `Main` est le *point d’entrée managé* de l’application :</span><span class="sxs-lookup"><span data-stu-id="e63e8-105">The `Main` method is the app's *managed entry point*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

<span data-ttu-id="e63e8-106">L’hôte .NET Core :</span><span class="sxs-lookup"><span data-stu-id="e63e8-106">The .NET Core Host:</span></span>

* <span data-ttu-id="e63e8-107">Charge le [runtime .NET Core](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="e63e8-107">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="e63e8-108">Utilise le premier argument de ligne de commande comme chemin du fichier binaire managé qui contient le point d’entrée (`Main`) et commence l’exécution de code.</span><span class="sxs-lookup"><span data-stu-id="e63e8-108">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="e63e8-109">La méthode `Main` appelle [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), qui suit le [modèle de conception builder](https://wikipedia.org/wiki/Builder_pattern) pour créer un hôte web.</span><span class="sxs-lookup"><span data-stu-id="e63e8-109">The `Main` method invokes [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web host.</span></span> <span data-ttu-id="e63e8-110">Le builder a des méthodes qui définissent un serveur web (par exemple, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) et la classe de démarrage (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="e63e8-110">The builder has methods that define a web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="e63e8-111">Dans l’exemple précédent, le serveur web [Kestrel](xref:fundamentals/servers/kestrel) est alloué automatiquement.</span><span class="sxs-lookup"><span data-stu-id="e63e8-111">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="e63e8-112">L’hôte web d’ASP.NET Core tente de s’exécuter sur [IIS (Internet Information Services)](https://www.iis.net/), s’il est disponible.</span><span class="sxs-lookup"><span data-stu-id="e63e8-112">ASP.NET Core's web host attempts to run on [Internet Information Services (IIS)](https://www.iis.net/), if available.</span></span> <span data-ttu-id="e63e8-113">D’autres serveurs web, tels que [HTTP.sys](xref:fundamentals/servers/httpsys), peuvent être utilisés via l’appel de la méthode d’extension appropriée.</span><span class="sxs-lookup"><span data-stu-id="e63e8-113">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="e63e8-114">`UseStartup` est expliqué de manière plus détaillée dans la section [Démarrage](#startup).</span><span class="sxs-lookup"><span data-stu-id="e63e8-114">`UseStartup` is explained further in the [Startup](#startup) section.</span></span>

<span data-ttu-id="e63e8-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, le type de retour de l’appel de `WebHost.CreateDefaultBuilder`, fournit de nombreuses méthodes facultatives.</span><span class="sxs-lookup"><span data-stu-id="e63e8-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="e63e8-116">Certaines de ces méthodes incluent `UseHttpSys` pour l’hébergement de l’application dans HTTP.sys et <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> pour la spécification du répertoire de contenu racine.</span><span class="sxs-lookup"><span data-stu-id="e63e8-116">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="e63e8-117">Les méthodes <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> et <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> génèrent l’objet <xref:Microsoft.AspNetCore.Hosting.IWebHost>, qui héberge l’application et démarre l’écoute des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="e63e8-117">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

<span data-ttu-id="e63e8-118">L’hôte .NET Core :</span><span class="sxs-lookup"><span data-stu-id="e63e8-118">The .NET Core Host:</span></span>

* <span data-ttu-id="e63e8-119">Charge le [runtime .NET Core](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="e63e8-119">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="e63e8-120">Utilise le premier argument de ligne de commande comme chemin du fichier binaire managé qui contient le point d’entrée (`Main`) et commence l’exécution de code.</span><span class="sxs-lookup"><span data-stu-id="e63e8-120">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="e63e8-121">La méthode `Main` utilise <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, qui suit le [modèle de conception builder](https://wikipedia.org/wiki/Builder_pattern) pour créer un hôte d’application web.</span><span class="sxs-lookup"><span data-stu-id="e63e8-121">The `Main` method uses <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web app host.</span></span> <span data-ttu-id="e63e8-122">Le builder a des méthodes qui définissent le serveur web (par exemple <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) et la classe de démarrage (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="e63e8-122">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="e63e8-123">Dans l’exemple précédent, le serveur web [Kestrel](xref:fundamentals/servers/kestrel) est utilisé.</span><span class="sxs-lookup"><span data-stu-id="e63e8-123">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="e63e8-124">D’autres serveurs web, tels que [HTTP.sys](xref:fundamentals/servers/httpsys), peuvent être utilisés via l’appel de la méthode d’extension appropriée.</span><span class="sxs-lookup"><span data-stu-id="e63e8-124">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="e63e8-125">`UseStartup` est expliqué de manière plus détaillée dans la section [Démarrage](#startup).</span><span class="sxs-lookup"><span data-stu-id="e63e8-125">`UseStartup` is explained further in the [Startup](#startup) section.</span></span>

<span data-ttu-id="e63e8-126">`WebHostBuilder` fournit de nombreuses méthodes facultatives, notamment <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> pour l’hébergement dans IIS et IIS Express et <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> pour la spécification du répertoire de contenu racine.</span><span class="sxs-lookup"><span data-stu-id="e63e8-126">`WebHostBuilder` provides many optional methods, including <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> for hosting in IIS and IIS Express and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="e63e8-127">Les méthodes <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> et <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> génèrent l’objet <xref:Microsoft.AspNetCore.Hosting.IWebHost>, qui héberge l’application et démarre l’écoute des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="e63e8-127">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

## <a name="startup"></a><span data-ttu-id="e63e8-128">Démarrage</span><span class="sxs-lookup"><span data-stu-id="e63e8-128">Startup</span></span>

<span data-ttu-id="e63e8-129">La méthode `UseStartup` sur `WebHostBuilder` spécifie la classe `Startup` pour votre application :</span><span class="sxs-lookup"><span data-stu-id="e63e8-129">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

<span data-ttu-id="e63e8-130">La classe `Startup` est l’emplacement où vous définissez le pipeline de traitement des requêtes et où sont configurés les services nécessaires à l’application.</span><span class="sxs-lookup"><span data-stu-id="e63e8-130">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="e63e8-131">La classe `Startup` doit être publique et contenir les méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="e63e8-131">The `Startup` class must be public and contain the following methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<span data-ttu-id="e63e8-132"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> définit les [services](#dependency-injection-services) utilisés par votre application (par exemple, ASP.NET Core MVC, Entity Framework Core et Identity).</span><span class="sxs-lookup"><span data-stu-id="e63e8-132"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="e63e8-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> définit les [intergiciels (middlewares)](xref:fundamentals/middleware/index) appelés dans le pipeline de requête.</span><span class="sxs-lookup"><span data-stu-id="e63e8-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> defines the [middleware](xref:fundamentals/middleware/index) called in the request pipeline.</span></span>

<span data-ttu-id="e63e8-134">Pour plus d'informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="e63e8-134">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="content-root"></a><span data-ttu-id="e63e8-135">Racine de contenu</span><span class="sxs-lookup"><span data-stu-id="e63e8-135">Content root</span></span>

<span data-ttu-id="e63e8-136">La racine de contenu est le chemin de base de tout contenu utilisé par l’application, tel que les [pages Razor](xref:razor-pages/index), les vues MVC et les composants statiques.</span><span class="sxs-lookup"><span data-stu-id="e63e8-136">The content root is the base path to any content used by the app, such as [Razor Pages](xref:razor-pages/index), MVC views, and static assets.</span></span> <span data-ttu-id="e63e8-137">Par défaut, la racine de contenu est identique au chemin de base de l’application pour l’exécutable qui héberge l’application.</span><span class="sxs-lookup"><span data-stu-id="e63e8-137">By default, the content root is the same location as the app base path for the executable hosting the app.</span></span>

## <a name="web-root-webroot"></a><span data-ttu-id="e63e8-138">Racine web (webroot)</span><span class="sxs-lookup"><span data-stu-id="e63e8-138">Web root (webroot)</span></span>

<span data-ttu-id="e63e8-139">La racine web d’une application correspond au répertoire de projet qui contient les ressources publiques statiques, telles que les fichiers CSS, JavaScript et image.</span><span class="sxs-lookup"><span data-stu-id="e63e8-139">The webroot of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="e63e8-140">Par défaut, *wwwroot* est la racine web.</span><span class="sxs-lookup"><span data-stu-id="e63e8-140">By default, *wwwroot* is the webroot.</span></span>

<span data-ttu-id="e63e8-141">Pour les fichiers Razor (*.cshtml*), la barre oblique tilde `~/` pointe vers la racine web.</span><span class="sxs-lookup"><span data-stu-id="e63e8-141">For Razor (*.cshtml*) files, the tilde-slash  `~/` points to the webroot.</span></span> <span data-ttu-id="e63e8-142">Les chemins commençant par `~/` sont appelés chemins virtuels.</span><span class="sxs-lookup"><span data-stu-id="e63e8-142">Paths beginning with `~/` are referred to as virtual paths.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="e63e8-143">Injection de dépendances (services)</span><span class="sxs-lookup"><span data-stu-id="e63e8-143">Dependency injection (services)</span></span>

<span data-ttu-id="e63e8-144">Un *service* est un composant destiné à une consommation courante dans une application.</span><span class="sxs-lookup"><span data-stu-id="e63e8-144">A *service* is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="e63e8-145">Les services sont accessibles via l’[injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e63e8-145">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e63e8-146">ASP.NET Core inclut un conteneur IoC (Inversion of Control) natif qui prend en charge l’[injection de constructeurs](xref:mvc/controllers/dependency-injection#constructor-injection) par défaut.</span><span class="sxs-lookup"><span data-stu-id="e63e8-146">ASP.NET Core includes a native Inversion of Control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="e63e8-147">Vous pouvez remplacer le conteneur par défaut si vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="e63e8-147">You can replace the default container if you wish.</span></span> <span data-ttu-id="e63e8-148">L’injection de dépendances n’offre pas seulement un [couplage faible](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), elle permet également d’accéder à des services (tels que la [journalisation](xref:fundamentals/logging/index)) dans l’ensemble de votre application.</span><span class="sxs-lookup"><span data-stu-id="e63e8-148">In addition to its [loose coupling benefit](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI makes services, such as [logging](xref:fundamentals/logging/index), available throughout your app.</span></span>

<span data-ttu-id="e63e8-149">Pour plus d'informations, consultez <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="e63e8-149">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="e63e8-150">Intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="e63e8-150">Middleware</span></span>

<span data-ttu-id="e63e8-151">Dans ASP.NET Core, vous composez votre pipeline de requêtes à l’aide d’un [intergiciel (middleware)](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="e63e8-151">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="e63e8-152">Un middleware ASP.NET Core effectue des opérations asynchrones sur `HttpContext`, puis appelle le middleware suivant dans le pipeline ou met fin à la requête.</span><span class="sxs-lookup"><span data-stu-id="e63e8-152">ASP.NET Core middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="e63e8-153">Par convention, un composant de middleware appelé « XYZ » est ajouté au pipeline en appelant la méthode d’extension `UseXYZ` dans la méthode `Configure`.</span><span class="sxs-lookup"><span data-stu-id="e63e8-153">By convention, a middleware component called "XYZ" is added to the pipeline by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="e63e8-154">ASP.NET Core inclut un ensemble complet de middlewares intégrés, et vous pouvez écrire votre propre middleware personnalisé.</span><span class="sxs-lookup"><span data-stu-id="e63e8-154">ASP.NET Core includes a rich set of built-in middleware, and you can write your own custom middleware.</span></span> <span data-ttu-id="e63e8-155">[Open Web Interface pour .NET (OWIN)](xref:fundamentals/owin), qui permet aux applications web d’être dissociées des serveurs web, est pris en charge dans les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e63e8-155">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), which allows web apps to be decoupled from web servers, is supported in ASP.NET Core apps.</span></span>

<span data-ttu-id="e63e8-156">Pour plus d’informations, consultez <xref:fundamentals/middleware/index> et <xref:fundamentals/owin>.</span><span class="sxs-lookup"><span data-stu-id="e63e8-156">For more information, see <xref:fundamentals/middleware/index> and <xref:fundamentals/owin>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="e63e8-157">Lancer des requêtes HTTP</span><span class="sxs-lookup"><span data-stu-id="e63e8-157">Initiate HTTP requests</span></span>

<span data-ttu-id="e63e8-158"><xref:System.Net.Http.IHttpClientFactory> est disponible pour accéder aux instances <xref:System.Net.Http.HttpClient> afin d’effectuer des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="e63e8-158"><xref:System.Net.Http.IHttpClientFactory> is available to access <xref:System.Net.Http.HttpClient> instances to make HTTP requests.</span></span>

<span data-ttu-id="e63e8-159">Pour plus d'informations, consultez <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="e63e8-159">For more information, see <xref:fundamentals/http-requests>.</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="e63e8-160">Environnements</span><span class="sxs-lookup"><span data-stu-id="e63e8-160">Environments</span></span>

<span data-ttu-id="e63e8-161">Les environnements, tels que *Développement* et *Production*, sont une notion de premier plan dans ASP.NET Core. Vous pouvez les définir à l’aide d’une variable d’environnement, d’un fichier de paramètres et d’un argument de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="e63e8-161">Environments, such as *Development* and *Production*, are a first-class notion in ASP.NET Core and can be set using an environment variable, settings file, and command-line argument.</span></span>

<span data-ttu-id="e63e8-162">Pour plus d'informations, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="e63e8-162">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="hosting"></a><span data-ttu-id="e63e8-163">Hébergement</span><span class="sxs-lookup"><span data-stu-id="e63e8-163">Hosting</span></span>

<span data-ttu-id="e63e8-164">Les applications ASP.NET Core configurent et lancent un *hôte*, qui est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="e63e8-164">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="e63e8-165">Pour plus d'informations, consultez <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="e63e8-165">For more information, see <xref:fundamentals/host/index>.</span></span>

## <a name="servers"></a><span data-ttu-id="e63e8-166">Serveurs</span><span class="sxs-lookup"><span data-stu-id="e63e8-166">Servers</span></span>

<span data-ttu-id="e63e8-167">Le modèle d’hébergement ASP.NET Core n’écoute pas directement les requêtes.</span><span class="sxs-lookup"><span data-stu-id="e63e8-167">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="e63e8-168">Le modèle d’hébergement s’appuie sur une implémentation de serveur HTTP pour transférer la requête à l’application.</span><span class="sxs-lookup"><span data-stu-id="e63e8-168">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="e63e8-169">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="e63e8-169">Windows</span></span>](#tab/windows)

<span data-ttu-id="e63e8-170">ASP.NET Core fournit les implémentations de serveur suivantes :</span><span class="sxs-lookup"><span data-stu-id="e63e8-170">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="e63e8-171">Serveur [Kestrel](xref:fundamentals/servers/kestrel) : serveur web multiplateforme géré.</span><span class="sxs-lookup"><span data-stu-id="e63e8-171">[Kestrel](xref:fundamentals/servers/kestrel) server is a managed, cross-platform web server.</span></span> <span data-ttu-id="e63e8-172">Kestrel est souvent exécuté dans une configuration de proxy inverse à l’aide d’[IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="e63e8-172">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="e63e8-173">Kestrel peut également être exécuté en tant que serveur périphérique public exposé directement à Internet dans ASP.NET Core 2.0 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="e63e8-173">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="e63e8-174">Serveur IIS HTTP (`IISHttpServer`) est un [serveur in-process](xref:fundamentals/servers/index#in-process-hosting-model) pour IIS.</span><span class="sxs-lookup"><span data-stu-id="e63e8-174">IIS HTTP Server (`IISHttpServer`) is an [in-process server](xref:fundamentals/servers/index#in-process-hosting-model) for IIS.</span></span>
* <span data-ttu-id="e63e8-175">Serveur [HTTP.sys](xref:fundamentals/servers/httpsys) : serveur web pour ASP.NET Core sur Windows.</span><span class="sxs-lookup"><span data-stu-id="e63e8-175">[HTTP.sys](xref:fundamentals/servers/httpsys) server is a web server for ASP.NET Core on Windows.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="e63e8-176">macOS</span><span class="sxs-lookup"><span data-stu-id="e63e8-176">macOS</span></span>](#tab/macos)

<span data-ttu-id="e63e8-177">ASP.NET Core utilise l’implémentation du serveur [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="e63e8-177">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="e63e8-178">Kestrel est un serveur web multiplateforme géré.</span><span class="sxs-lookup"><span data-stu-id="e63e8-178">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="e63e8-179">Kestrel peut également être exécuté en tant que serveur périphérique public exposé directement à Internet dans ASP.NET Core 2.0 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="e63e8-179">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="e63e8-180">Linux</span><span class="sxs-lookup"><span data-stu-id="e63e8-180">Linux</span></span>](#tab/linux)

<span data-ttu-id="e63e8-181">ASP.NET Core utilise l’implémentation du serveur [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="e63e8-181">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="e63e8-182">Kestrel est un serveur web multiplateforme géré.</span><span class="sxs-lookup"><span data-stu-id="e63e8-182">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="e63e8-183">Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](http://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="e63e8-183">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="e63e8-184">Kestrel peut également être exécuté en tant que serveur périphérique public exposé directement à Internet dans ASP.NET Core 2.0 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="e63e8-184">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="e63e8-185">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="e63e8-185">Windows</span></span>](#tab/windows)

<span data-ttu-id="e63e8-186">ASP.NET Core fournit les implémentations de serveur suivantes :</span><span class="sxs-lookup"><span data-stu-id="e63e8-186">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="e63e8-187">Serveur [Kestrel](xref:fundamentals/servers/kestrel) : serveur web multiplateforme géré.</span><span class="sxs-lookup"><span data-stu-id="e63e8-187">[Kestrel](xref:fundamentals/servers/kestrel) server is a managed, cross-platform web server.</span></span> <span data-ttu-id="e63e8-188">Kestrel est souvent exécuté dans une configuration de proxy inverse à l’aide d’[IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="e63e8-188">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="e63e8-189">Kestrel peut également être exécuté en tant que serveur périphérique public exposé directement à Internet dans ASP.NET Core 2.0 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="e63e8-189">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="e63e8-190">Serveur [HTTP.sys](xref:fundamentals/servers/httpsys) : serveur web pour ASP.NET Core sur Windows.</span><span class="sxs-lookup"><span data-stu-id="e63e8-190">[HTTP.sys](xref:fundamentals/servers/httpsys) server is a web server for ASP.NET Core on Windows.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="e63e8-191">macOS</span><span class="sxs-lookup"><span data-stu-id="e63e8-191">macOS</span></span>](#tab/macos)

<span data-ttu-id="e63e8-192">ASP.NET Core utilise l’implémentation du serveur [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="e63e8-192">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="e63e8-193">Kestrel est un serveur web multiplateforme géré.</span><span class="sxs-lookup"><span data-stu-id="e63e8-193">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="e63e8-194">Kestrel peut également être exécuté en tant que serveur périphérique public exposé directement à Internet dans ASP.NET Core 2.0 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="e63e8-194">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="e63e8-195">Linux</span><span class="sxs-lookup"><span data-stu-id="e63e8-195">Linux</span></span>](#tab/linux)

<span data-ttu-id="e63e8-196">ASP.NET Core utilise l’implémentation du serveur [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="e63e8-196">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="e63e8-197">Kestrel est un serveur web multiplateforme géré.</span><span class="sxs-lookup"><span data-stu-id="e63e8-197">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="e63e8-198">Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](http://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="e63e8-198">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="e63e8-199">Kestrel peut également être exécuté en tant que serveur périphérique public exposé directement à Internet dans ASP.NET Core 2.0 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="e63e8-199">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

---

::: moniker-end

<span data-ttu-id="e63e8-200">Pour plus d'informations, consultez <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="e63e8-200">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="e63e8-201">Configuration</span><span class="sxs-lookup"><span data-stu-id="e63e8-201">Configuration</span></span>

<span data-ttu-id="e63e8-202">ASP.NET Core utilise un modèle de configuration basé sur les paires nom-valeur.</span><span class="sxs-lookup"><span data-stu-id="e63e8-202">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="e63e8-203">Le modèle de configuration n’est pas basé sur <xref:System.Configuration> ou *web.config*. La configuration obtient les paramètres d’une sélection de fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="e63e8-203">The configuration model isn't based on <xref:System.Configuration> or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="e63e8-204">Les fournisseurs de configuration intégrés prennent en charge une grande variété de formats de fichiers (XML, JSON, INI), de variables d’environnement et d’arguments de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="e63e8-204">The built-in configuration providers support a variety of file formats (XML, JSON, INI), environment variables, and command-line arguments.</span></span> <span data-ttu-id="e63e8-205">Vous pouvez également écrire vos propres fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="e63e8-205">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="e63e8-206">Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="e63e8-206">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="logging"></a><span data-ttu-id="e63e8-207">Journalisation</span><span class="sxs-lookup"><span data-stu-id="e63e8-207">Logging</span></span>

<span data-ttu-id="e63e8-208">ASP.NET Core prend en charge une API de journalisation qui fonctionne avec divers fournisseurs de journalisation.</span><span class="sxs-lookup"><span data-stu-id="e63e8-208">ASP.NET Core supports a Logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="e63e8-209">Les fournisseurs intégrés prennent en charge l’envoi de journaux à une ou plusieurs destinations.</span><span class="sxs-lookup"><span data-stu-id="e63e8-209">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="e63e8-210">Des frameworks de journalisation tiers peuvent être utilisés.</span><span class="sxs-lookup"><span data-stu-id="e63e8-210">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="e63e8-211">Pour plus d'informations, consultez <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="e63e8-211">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="e63e8-212">Gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="e63e8-212">Error handling</span></span>

<span data-ttu-id="e63e8-213">ASP.NET Core offre des scénarios intégrées pour la gestion des erreurs dans les applications, notamment une page d’exceptions du développeur, des pages d’erreurs personnalisées, des pages de code d’état statique et la gestion des exceptions de démarrage.</span><span class="sxs-lookup"><span data-stu-id="e63e8-213">ASP.NET Core has built-in scenarios for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="e63e8-214">Pour plus d'informations, consultez <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="e63e8-214">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="routing"></a><span data-ttu-id="e63e8-215">Routage</span><span class="sxs-lookup"><span data-stu-id="e63e8-215">Routing</span></span>

<span data-ttu-id="e63e8-216">ASP.NET Core offre des scénarios pour router les requêtes d’application vers les gestionnaires de routage.</span><span class="sxs-lookup"><span data-stu-id="e63e8-216">ASP.NET Core offers scenarios for routing of app requests to route handlers.</span></span>

<span data-ttu-id="e63e8-217">Pour plus d'informations, consultez <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="e63e8-217">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="background-tasks"></a><span data-ttu-id="e63e8-218">Tâches en arrière-plan</span><span class="sxs-lookup"><span data-stu-id="e63e8-218">Background tasks</span></span>

<span data-ttu-id="e63e8-219">Les tâches en arrière-plan sont implémentées en tant que *services hébergés*.</span><span class="sxs-lookup"><span data-stu-id="e63e8-219">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="e63e8-220">Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="e63e8-220">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span>

<span data-ttu-id="e63e8-221">Pour plus d'informations, consultez <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="e63e8-221">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="e63e8-222">Accéder à HttpContext</span><span class="sxs-lookup"><span data-stu-id="e63e8-222">Access HttpContext</span></span>

<span data-ttu-id="e63e8-223">`HttpContext` est disponible automatiquement lors du traitement des requêtes avec Razor Pages et MVC.</span><span class="sxs-lookup"><span data-stu-id="e63e8-223">`HttpContext` is automatically available when processing requests with Razor Pages and MVC.</span></span> <span data-ttu-id="e63e8-224">Dans les cas où `HttpContext` n’est pas disponible, vous pouvez accéder à `HttpContext` par le biais de l’interface <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> et de son implémentation par défaut, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span><span class="sxs-lookup"><span data-stu-id="e63e8-224">In circumstances where `HttpContext` isn't readily available, you can access the `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span>

<span data-ttu-id="e63e8-225">Pour plus d'informations, consultez <xref:fundamentals/httpcontext>.</span><span class="sxs-lookup"><span data-stu-id="e63e8-225">For more information, see <xref:fundamentals/httpcontext>.</span></span>
