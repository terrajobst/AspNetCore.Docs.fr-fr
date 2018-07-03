---
title: Notions de base d’ASP.NET Core
author: rick-anderson
description: Découvrez les concepts de base permettant de créer des applications ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 07/02/2018
uid: fundamentals/index
ms.openlocfilehash: 33786bf78567a1aa12a1ac97d44d1a596ec4c3be
ms.sourcegitcommit: 08f1a9baa97060da5168840b332c9c0805b5f901
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37144974"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="e849c-103">Notions de base d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e849c-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="e849c-104">Une application ASP.NET Core est une application console qui crée un serveur web dans sa méthode `Main` :</span><span class="sxs-lookup"><span data-stu-id="e849c-104">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e849c-105">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e849c-105">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

<span data-ttu-id="e849c-106">La méthode `Main` appelle `WebHost.CreateDefaultBuilder`, qui suit le modèle de conception builder pour créer un hôte d’application web.</span><span class="sxs-lookup"><span data-stu-id="e849c-106">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="e849c-107">Le builder a des méthodes qui définissent le serveur web (par exemple `UseKestrel`) et la classe de démarrage (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="e849c-107">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="e849c-108">Dans l’exemple précédent, le serveur web [Kestrel](xref:fundamentals/servers/kestrel) est alloué automatiquement.</span><span class="sxs-lookup"><span data-stu-id="e849c-108">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="e849c-109">L’hôte web d’ASP.NET Core tente de s’exécuter sur IIS, s’il est disponible.</span><span class="sxs-lookup"><span data-stu-id="e849c-109">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="e849c-110">D’autres serveurs web, tels que [HTTP.sys](xref:fundamentals/servers/httpsys), peuvent être utilisés via l’appel de la méthode d’extension appropriée.</span><span class="sxs-lookup"><span data-stu-id="e849c-110">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="e849c-111">`UseStartup` est expliqué de manière plus détaillée dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="e849c-111">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="e849c-112">`IWebHostBuilder`, le type de retour de l’appel de `WebHost.CreateDefaultBuilder`, fournit de nombreuses méthodes facultatives.</span><span class="sxs-lookup"><span data-stu-id="e849c-112">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="e849c-113">Certaines de ces méthodes incluent `UseHttpSys` pour l’hébergement de l’application dans HTTP.sys et `UseContentRoot` pour la spécification du répertoire de contenu racine.</span><span class="sxs-lookup"><span data-stu-id="e849c-113">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="e849c-114">Les méthodes `Build` et `Run` génèrent l’objet `IWebHost`, qui héberge l’application et démarre l’écoute des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="e849c-114">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e849c-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e849c-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs)]

<span data-ttu-id="e849c-116">La méthode `Main` utilise `WebHostBuilder`, qui suit le modèle du générateur pour créer un hôte d’application web.</span><span class="sxs-lookup"><span data-stu-id="e849c-116">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="e849c-117">Le builder a des méthodes qui définissent le serveur web (par exemple `UseKestrel`) et la classe de démarrage (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="e849c-117">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="e849c-118">Dans l’exemple précédent, le serveur web [Kestrel](xref:fundamentals/servers/kestrel) est utilisé.</span><span class="sxs-lookup"><span data-stu-id="e849c-118">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="e849c-119">D’autres serveurs web, tels que [WebListener](xref:fundamentals/servers/weblistener), peuvent être utilisés via l’appel de la méthode d’extension appropriée.</span><span class="sxs-lookup"><span data-stu-id="e849c-119">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="e849c-120">`UseStartup` est expliqué de manière plus détaillée dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="e849c-120">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="e849c-121">`WebHostBuilder` fournit de nombreuses méthodes facultatives, notamment `UseIISIntegration` pour l’hébergement dans IIS et IIS Express et `UseContentRoot` pour la spécification du répertoire de contenu racine.</span><span class="sxs-lookup"><span data-stu-id="e849c-121">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="e849c-122">Les méthodes `Build` et `Run` génèrent l’objet `IWebHost`, qui héberge l’application et démarre l’écoute des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="e849c-122">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="e849c-123">Démarrage</span><span class="sxs-lookup"><span data-stu-id="e849c-123">Startup</span></span>

<span data-ttu-id="e849c-124">La méthode `UseStartup` sur `WebHostBuilder` spécifie la classe `Startup` pour votre application :</span><span class="sxs-lookup"><span data-stu-id="e849c-124">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e849c-125">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e849c-125">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e849c-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e849c-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

<span data-ttu-id="e849c-127">La classe `Startup` est l’emplacement où vous définissez le pipeline de traitement des requêtes et où sont configurés les services nécessaires à l’application.</span><span class="sxs-lookup"><span data-stu-id="e849c-127">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="e849c-128">La classe `Startup` doit être publique et contenir les méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="e849c-128">The `Startup` class must be public and contain the following methods:</span></span>

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method
    // to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method
    // to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

<span data-ttu-id="e849c-129">`ConfigureServices` définit les [services](#dependency-injection-services) utilisés par votre application (par exemple, ASP.NET Core MVC, Entity Framework Core et Identity).</span><span class="sxs-lookup"><span data-stu-id="e849c-129">`ConfigureServices` defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="e849c-130">`Configure` définit les [intergiciels (middleware)](xref:fundamentals/middleware/index) du pipeline de requêtes.</span><span class="sxs-lookup"><span data-stu-id="e849c-130">`Configure` defines the [middleware](xref:fundamentals/middleware/index) for the request pipeline.</span></span>

<span data-ttu-id="e849c-131">Pour plus d’informations, consultez [Démarrage de l’application](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="e849c-131">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="content-root"></a><span data-ttu-id="e849c-132">Racine de contenu</span><span class="sxs-lookup"><span data-stu-id="e849c-132">Content root</span></span>

<span data-ttu-id="e849c-133">La racine de contenu est le chemin de base de tout contenu utilisé par l’application, par exemple les vues, les [pages Razor](xref:razor-pages/index) et les composants statiques.</span><span class="sxs-lookup"><span data-stu-id="e849c-133">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:razor-pages/index), and static assets.</span></span> <span data-ttu-id="e849c-134">Par défaut, la racine de contenu est identique au chemin de base de l’application pour l’exécutable qui héberge l’application.</span><span class="sxs-lookup"><span data-stu-id="e849c-134">By default, the content root is the same as application base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="e849c-135">Racine web</span><span class="sxs-lookup"><span data-stu-id="e849c-135">Web root</span></span>

<span data-ttu-id="e849c-136">La racine web d’une application correspond au répertoire de projet qui contient les ressources publiques, statiques, telles que les fichiers CSS, JavaScript et image.</span><span class="sxs-lookup"><span data-stu-id="e849c-136">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="e849c-137">Injection de dépendances (services)</span><span class="sxs-lookup"><span data-stu-id="e849c-137">Dependency injection (services)</span></span>

<span data-ttu-id="e849c-138">Un service est un composant destiné à une consommation courante dans une application.</span><span class="sxs-lookup"><span data-stu-id="e849c-138">A service is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="e849c-139">Les services sont accessibles via l’[injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e849c-139">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e849c-140">ASP.NET Core inclut un conteneur IoC (**I**nversion **o**f **C**ontrol) natif qui prend en charge l’[injection de constructeurs](xref:mvc/controllers/dependency-injection#constructor-injection) par défaut.</span><span class="sxs-lookup"><span data-stu-id="e849c-140">ASP.NET Core includes a native **I**nversion **o**f **C**ontrol (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="e849c-141">Vous pouvez remplacer le conteneur natif par défaut si vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="e849c-141">You can replace the default native container if you wish.</span></span> <span data-ttu-id="e849c-142">L’injection de dépendances n’offre pas seulement un couplage faible, elle permet également d’accéder aux services dans l’ensemble de votre application (par exemple, la [journalisation](xref:fundamentals/logging/index)).</span><span class="sxs-lookup"><span data-stu-id="e849c-142">In addition to its loose coupling benefit, DI makes services available throughout your app (for example, [logging](xref:fundamentals/logging/index)).</span></span>

<span data-ttu-id="e849c-143">Pour plus d’informations, consultez [Injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e849c-143">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="e849c-144">Intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="e849c-144">Middleware</span></span>

<span data-ttu-id="e849c-145">Dans ASP.NET Core, vous composez votre pipeline de requêtes à l’aide d’un [intergiciel (middleware)](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="e849c-145">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="e849c-146">Un intergiciel (middleware) ASP.NET Core suit une logique asynchrone sur `HttpContext`, puis appelle l’intergiciel suivant de la séquence ou met fin directement à la requête.</span><span class="sxs-lookup"><span data-stu-id="e849c-146">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="e849c-147">Un composant intergiciel (middleware) appelé « XYZ » est ajouté via l’appel de la méthode d’extension `UseXYZ` dans la méthode `Configure`.</span><span class="sxs-lookup"><span data-stu-id="e849c-147">A middleware component called "XYZ" is added by invoking an `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="e849c-148">ASP.NET Core comprend un ensemble complet d’intergiciels (middleware) intégrés :</span><span class="sxs-lookup"><span data-stu-id="e849c-148">ASP.NET Core includes a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="e849c-149">Fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="e849c-149">Static files</span></span>](xref:fundamentals/static-files)
* [<span data-ttu-id="e849c-150">Le routage</span><span class="sxs-lookup"><span data-stu-id="e849c-150">Routing</span></span>](xref:fundamentals/routing)
* [<span data-ttu-id="e849c-151">Authentification</span><span class="sxs-lookup"><span data-stu-id="e849c-151">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="e849c-152">Intergiciel (middleware) de compression des réponses</span><span class="sxs-lookup"><span data-stu-id="e849c-152">Response Compression Middleware</span></span>](xref:performance/response-compression)
* [<span data-ttu-id="e849c-153">Intergiciel de réécriture d’URL</span><span class="sxs-lookup"><span data-stu-id="e849c-153">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)

<span data-ttu-id="e849c-154">L’intergiciel (middleware) [OWIN](http://owin.org) est disponible pour les applications ASP.NET Core. Vous pouvez aussi écrire votre propre intergiciel personnalisé.</span><span class="sxs-lookup"><span data-stu-id="e849c-154">[OWIN](http://owin.org)-based middleware is available for ASP.NET Core apps, and you can write your own custom middleware.</span></span>

<span data-ttu-id="e849c-155">Pour plus d’informations, consultez [Intergiciel (middleware)](xref:fundamentals/middleware/index) et [OWIN (Open Web Interface pour .NET)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="e849c-155">For more information, see [Middleware](xref:fundamentals/middleware/index) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="e849c-156">Lancer des requêtes HTTP</span><span class="sxs-lookup"><span data-stu-id="e849c-156">Initiate HTTP requests</span></span>

<span data-ttu-id="e849c-157">Pour plus d’informations sur l’utilisation de `IHttpClientFactory` pour accéder à des instances `HttpClient` afin d’effectuer des requêtes HTTP, consultez [Lancement de requêtes HTTP](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="e849c-157">For information about using `IHttpClientFactory` to access `HttpClient` instances to make HTTP requests, see [Initiate HTTP requests](xref:fundamentals/http-requests).</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="e849c-158">Environnements</span><span class="sxs-lookup"><span data-stu-id="e849c-158">Environments</span></span>

<span data-ttu-id="e849c-159">Les environnements, tels que « Développement » et « Production », sont une notion de premier plan dans ASP.NET Core. Vous pouvez les définir à l’aide de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="e849c-159">Environments, such as "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="e849c-160">Pour plus d’informations, consultez [Utiliser plusieurs environnements](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="e849c-160">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="configuration"></a><span data-ttu-id="e849c-161">Configuration</span><span class="sxs-lookup"><span data-stu-id="e849c-161">Configuration</span></span>

<span data-ttu-id="e849c-162">ASP.NET Core utilise un modèle de configuration basé sur les paires nom-valeur.</span><span class="sxs-lookup"><span data-stu-id="e849c-162">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="e849c-163">Le modèle de configuration n’est pas basé sur `System.Configuration` ou *web.config*. La configuration obtient les paramètres d’une sélection de fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="e849c-163">The configuration model isn't based on `System.Configuration` or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="e849c-164">Les fournisseurs de configuration intégrés prennent en charge une grande variété de formats de fichiers (XML, JSON, INI), ainsi que des variables d’environnement pour activer la configuration basée sur l’environnement.</span><span class="sxs-lookup"><span data-stu-id="e849c-164">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="e849c-165">Vous pouvez également écrire vos propres fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="e849c-165">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="e849c-166">Pour plus d’informations, consultez [Configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="e849c-166">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="logging"></a><span data-ttu-id="e849c-167">Journalisation</span><span class="sxs-lookup"><span data-stu-id="e849c-167">Logging</span></span>

<span data-ttu-id="e849c-168">ASP.NET Core prend en charge une API de journalisation qui fonctionne avec divers fournisseurs de journalisation.</span><span class="sxs-lookup"><span data-stu-id="e849c-168">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="e849c-169">Les fournisseurs intégrés prennent en charge l’envoi de journaux à une ou plusieurs destinations.</span><span class="sxs-lookup"><span data-stu-id="e849c-169">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="e849c-170">Des frameworks de journalisation tiers peuvent être utilisés.</span><span class="sxs-lookup"><span data-stu-id="e849c-170">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="e849c-171">Pour plus d’informations, consultez [Journalisation](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="e849c-171">For more information, see [Logging](xref:fundamentals/logging/index)</span></span>

## <a name="error-handling"></a><span data-ttu-id="e849c-172">Gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="e849c-172">Error handling</span></span>

<span data-ttu-id="e849c-173">ASP.NET Core offre des fonctionnalités intégrées pour la gestion des erreurs dans les applications, notamment une page d’exceptions du développeur, des pages d’erreurs personnalisées, des pages de code d’état statique et la gestion des exceptions de démarrage.</span><span class="sxs-lookup"><span data-stu-id="e849c-173">ASP.NET Core has built-in features for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="e849c-174">Pour plus d’informations, consultez [Guide pratique pour gérer les erreurs](xref:fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="e849c-174">For more information, see [how to handle errors](xref:fundamentals/error-handling).</span></span>

## <a name="routing"></a><span data-ttu-id="e849c-175">Routage</span><span class="sxs-lookup"><span data-stu-id="e849c-175">Routing</span></span>

<span data-ttu-id="e849c-176">ASP.NET Core offre des fonctionnalités pour router les requêtes d’application vers les gestionnaires de routage.</span><span class="sxs-lookup"><span data-stu-id="e849c-176">ASP.NET Core offers features for routing of app requests to route handlers.</span></span>

<span data-ttu-id="e849c-177">Pour plus d’informations, consultez [Routage](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="e849c-177">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="file-providers"></a><span data-ttu-id="e849c-178">Fournisseurs de fichiers</span><span class="sxs-lookup"><span data-stu-id="e849c-178">File providers</span></span>

<span data-ttu-id="e849c-179">ASP.NET Core fournit un accès au système de fichiers en utilisant des fournisseurs de fichiers qui offrent une interface commune permettant de travailler avec des fichiers sur plusieurs plateformes.</span><span class="sxs-lookup"><span data-stu-id="e849c-179">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="e849c-180">Pour plus d’informations, consultez [Fournisseurs de fichiers](xref:fundamentals/file-providers).</span><span class="sxs-lookup"><span data-stu-id="e849c-180">For more information, see [File Providers](xref:fundamentals/file-providers).</span></span>

## <a name="static-files"></a><span data-ttu-id="e849c-181">Fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="e849c-181">Static files</span></span>

<span data-ttu-id="e849c-182">L’intergiciel (middleware) de fichiers statiques prend en charge des fichiers statiques, tels que HTML, CSS, image et JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e849c-182">Static files middleware serves static files, such as HTML, CSS, image, and JavaScript.</span></span>

<span data-ttu-id="e849c-183">Pour plus d’informations, consultez [Fichiers statiques](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="e849c-183">For more information, see [Static files](xref:fundamentals/static-files).</span></span>

## <a name="hosting"></a><span data-ttu-id="e849c-184">Hébergement</span><span class="sxs-lookup"><span data-stu-id="e849c-184">Hosting</span></span>

<span data-ttu-id="e849c-185">Les applications ASP.NET Core configurent et lancent un *hôte*, qui est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="e849c-185">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="e849c-186">Pour plus d’informations, consultez [Héberger dans ASP.NET Core](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="e849c-186">For more information, see [Host in ASP.NET Core](xref:fundamentals/host/index).</span></span>

## <a name="session-and-app-state"></a><span data-ttu-id="e849c-187">État de session et d’application</span><span class="sxs-lookup"><span data-stu-id="e849c-187">Session and app state</span></span>

<span data-ttu-id="e849c-188">ASP.NET Core offre plusieurs approches pour conserver l’état de session et d’application pendant que l’utilisateur parcourt une application web.</span><span class="sxs-lookup"><span data-stu-id="e849c-188">ASP.NET Core offers several approaches to preserve session and app state while the user browses a web app.</span></span>

<span data-ttu-id="e849c-189">Pour plus d’informations, consultez [État de session et d’application](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="e849c-189">For more information, see [Session and app state](xref:fundamentals/app-state).</span></span>

## <a name="servers"></a><span data-ttu-id="e849c-190">Serveurs</span><span class="sxs-lookup"><span data-stu-id="e849c-190">Servers</span></span>

<span data-ttu-id="e849c-191">Le modèle d’hébergement ASP.NET Core n’écoute pas directement les requêtes.</span><span class="sxs-lookup"><span data-stu-id="e849c-191">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="e849c-192">Le modèle d’hébergement s’appuie sur une implémentation de serveur HTTP pour transférer la requête à l’application.</span><span class="sxs-lookup"><span data-stu-id="e849c-192">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="e849c-193">La requête transférée est enveloppée (wrap) dans un ensemble d’objets de fonctionnalités auxquels vous pouvez accéder via des interfaces.</span><span class="sxs-lookup"><span data-stu-id="e849c-193">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="e849c-194">ASP.NET Core inclut un serveur web multiplateforme géré, appelé [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="e849c-194">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="e849c-195">Kestrel s’exécute souvent derrière un serveur web de production comme [IIS](https://www.iis.net/) ou [Nginx](http://nginx.org).</span><span class="sxs-lookup"><span data-stu-id="e849c-195">Kestrel is often run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org).</span></span> <span data-ttu-id="e849c-196">Kestrel peut être exécuté comme serveur edge.</span><span class="sxs-lookup"><span data-stu-id="e849c-196">Kestrel can be run as an edge server.</span></span>

<span data-ttu-id="e849c-197">Pour plus d’informations, consultez [Serveurs](xref:fundamentals/servers/index) et les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="e849c-197">For more information, see [Servers](xref:fundamentals/servers/index) and the following topics:</span></span>

* [<span data-ttu-id="e849c-198">Kestrel</span><span class="sxs-lookup"><span data-stu-id="e849c-198">Kestrel</span></span>](xref:fundamentals/servers/kestrel)
* [<span data-ttu-id="e849c-199">Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e849c-199">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* <span data-ttu-id="e849c-200">[HTTP.sys](xref:fundamentals/servers/httpsys) (anciennement [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="e849c-200">[HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="e849c-201">Globalisation et localisation</span><span class="sxs-lookup"><span data-stu-id="e849c-201">Globalization and localization</span></span>

<span data-ttu-id="e849c-202">La création d’un site web multilingue avec ASP.NET Core vous permet d’atteindre un plus large public.</span><span class="sxs-lookup"><span data-stu-id="e849c-202">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="e849c-203">ASP.NET Core offre des services et des intergiciels (middleware) de traduction dans différentes langues et cultures.</span><span class="sxs-lookup"><span data-stu-id="e849c-203">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="e849c-204">Pour plus d’informations, consultez [Internationalisation et traduction](xref:fundamentals/localization).</span><span class="sxs-lookup"><span data-stu-id="e849c-204">For more information, see [Globalization and localization](xref:fundamentals/localization).</span></span>

## <a name="request-features"></a><span data-ttu-id="e849c-205">Fonctionnalités de requête</span><span class="sxs-lookup"><span data-stu-id="e849c-205">Request features</span></span>

<span data-ttu-id="e849c-206">Les détails d’implémentation d’un serveur web relatifs aux requêtes HTTP et aux réponses sont définis dans les interfaces.</span><span class="sxs-lookup"><span data-stu-id="e849c-206">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="e849c-207">Ces interfaces sont utilisées par les implémentations de serveur et les intergiciels (middleware) pour créer et modifier le pipeline d’hébergement de l’application.</span><span class="sxs-lookup"><span data-stu-id="e849c-207">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="e849c-208">Pour plus d’informations, consultez [Fonctionnalités de requête](xref:fundamentals/request-features).</span><span class="sxs-lookup"><span data-stu-id="e849c-208">For more information, see [Request Features](xref:fundamentals/request-features).</span></span>

## <a name="background-tasks"></a><span data-ttu-id="e849c-209">Tâches en arrière-plan</span><span class="sxs-lookup"><span data-stu-id="e849c-209">Background tasks</span></span>

<span data-ttu-id="e849c-210">Les tâches en arrière-plan sont implémentées en tant que *services hébergés*.</span><span class="sxs-lookup"><span data-stu-id="e849c-210">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="e849c-211">Un service hébergé est une classe avec la logique de tâches en arrière-plan qui implémente l’interface [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="e849c-211">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span>

<span data-ttu-id="e849c-212">Pour plus d’informations, consultez [Tâches en arrière-plan avec services hébergés](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="e849c-212">For more information, see [Background tasks with hosted services](xref:fundamentals/host/hosted-services).</span></span>

## <a name="open-web-interface-for-net-owin"></a><span data-ttu-id="e849c-213">OWIN (Open Web Interface pour .NET)</span><span class="sxs-lookup"><span data-stu-id="e849c-213">Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="e849c-214">ASP.NET Core prend en charge l’interface Open Web pour .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="e849c-214">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="e849c-215">OWIN permet que les applications web soient dissociées des serveurs web.</span><span class="sxs-lookup"><span data-stu-id="e849c-215">OWIN allows web apps to be decoupled from web servers.</span></span>

<span data-ttu-id="e849c-216">Pour plus d’informations, consultez [Interface Open Web pour .NET (OWIN)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="e849c-216">For more information, see [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="websockets"></a><span data-ttu-id="e849c-217">WebSockets</span><span class="sxs-lookup"><span data-stu-id="e849c-217">WebSockets</span></span>

<span data-ttu-id="e849c-218">[WebSocket](https://wikipedia.org/wiki/WebSocket) est un protocole qui autorise des canaux de communication persistants bidirectionnels sur les connexions TCP.</span><span class="sxs-lookup"><span data-stu-id="e849c-218">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="e849c-219">Il est utilisé pour des applications comme le chat, les transactions boursières, les jeux et partout où vous voulez des fonctionnalités en temps réel dans une application web.</span><span class="sxs-lookup"><span data-stu-id="e849c-219">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="e849c-220">ASP.NET Core prend en charge les fonctionnalités de socket web.</span><span class="sxs-lookup"><span data-stu-id="e849c-220">ASP.NET Core supports web socket features.</span></span>

<span data-ttu-id="e849c-221">Pour plus d’informations, consultez [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="e849c-221">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="microsoftaspnetcoreapp-metapackage"></a><span data-ttu-id="e849c-222">Métapackage Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="e849c-222">Microsoft.AspNetCore.App metapackage</span></span>

<span data-ttu-id="e849c-223">Le métapackage [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) simplifie la gestion des packages.</span><span class="sxs-lookup"><span data-stu-id="e849c-223">The [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) metapackage simplifies package management.</span></span> <span data-ttu-id="e849c-224">Pour plus d’informations, consultez [Métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="e849c-224">For more information, see [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end
::: moniker range="= aspnetcore-2.0"
## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="e849c-225">Métapackage Microsoft.AspNetCore.All</span><span class="sxs-lookup"><span data-stu-id="e849c-225">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="e849c-226">Le métapackage [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) pour ASP.NET Core comprend :</span><span class="sxs-lookup"><span data-stu-id="e849c-226">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="e849c-227">Tous les packages pris en charge par l’équipe ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e849c-227">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="e849c-228">Tous les packages pris en charge par Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e849c-228">All supported packages by Entity Framework Core.</span></span>
* <span data-ttu-id="e849c-229">Les dépendances internes et tierces utilisées par ASP.NET Core et Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e849c-229">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="e849c-230">Pour plus d’informations, consultez [Métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="e849c-230">For more information, see [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>
::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="e849c-231">Runtime .NET Core et runtime .NET Framework</span><span class="sxs-lookup"><span data-stu-id="e849c-231">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="e849c-232">Une application ASP.NET Core peut cibler le runtime .NET Core ou .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="e849c-232">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="e849c-233">Pour plus d’informations, consultez [Choix entre .NET Core et .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="e849c-233">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="e849c-234">Choisir entre ASP.NET Core et ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e849c-234">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="e849c-235">Pour plus d’informations sur le choix entre ASP.NET Core et ASP.NET, consultez [Choisir entre ASP.NET Core et ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span><span class="sxs-lookup"><span data-stu-id="e849c-235">For more information on choosing between ASP.NET Core and ASP.NET, see [Choose between ASP.NET Core and ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span></span>
