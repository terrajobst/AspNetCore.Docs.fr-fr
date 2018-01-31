---
title: "Notions de base d’ASP.NET Core"
author: rick-anderson
description: "Découvrez les concepts de base permettant de créer des applications ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: fundamentals/index
ms.openlocfilehash: 946ccc80915c5de60976a98cbbb253cb8dfacaca
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="7950f-103">Notions de base d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7950f-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="7950f-104">Une application ASP.NET Core est une application console qui crée un serveur web dans sa méthode `Main` :</span><span class="sxs-lookup"><span data-stu-id="7950f-104">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7950f-105">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7950f-105">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

<span data-ttu-id="7950f-106">La méthode `Main` appelle `WebHost.CreateDefaultBuilder`, qui suit le modèle du générateur pour créer un hôte d’application web.</span><span class="sxs-lookup"><span data-stu-id="7950f-106">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="7950f-107">Le générateur a des méthodes qui définissent le serveur web (par exemple `UseKestrel`) et la classe de démarrage (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="7950f-107">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="7950f-108">Dans l’exemple précédent, le serveur web [Kestrel](xref:fundamentals/servers/kestrel) est alloué automatiquement.</span><span class="sxs-lookup"><span data-stu-id="7950f-108">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="7950f-109">L’hôte web d’ASP.NET Core tente de s’exécuter sur IIS, s’il est disponible.</span><span class="sxs-lookup"><span data-stu-id="7950f-109">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="7950f-110">D’autres serveurs web, tels que [HTTP.sys](xref:fundamentals/servers/httpsys), peuvent être utilisés via l’appel de la méthode d’extension appropriée.</span><span class="sxs-lookup"><span data-stu-id="7950f-110">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="7950f-111">`UseStartup` est expliqué de manière plus détaillée dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="7950f-111">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="7950f-112">`IWebHostBuilder`, le type de retour de l’appel de `WebHost.CreateDefaultBuilder`, fournit de nombreuses méthodes facultatives.</span><span class="sxs-lookup"><span data-stu-id="7950f-112">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="7950f-113">Certaines de ces méthodes incluent `UseHttpSys` pour l’hébergement de l’application dans HTTP.sys et `UseContentRoot` pour la spécification du répertoire de contenu racine.</span><span class="sxs-lookup"><span data-stu-id="7950f-113">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="7950f-114">Les méthodes `Build` et `Run` génèrent l’objet `IWebHost`, qui héberge l’application et démarre l’écoute des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="7950f-114">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7950f-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7950f-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]

<span data-ttu-id="7950f-116">La méthode `Main` utilise `WebHostBuilder`, qui suit le modèle du générateur pour créer un hôte d’application web.</span><span class="sxs-lookup"><span data-stu-id="7950f-116">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="7950f-117">Le générateur a des méthodes qui définissent le serveur web (par exemple `UseKestrel`) et la classe de démarrage (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="7950f-117">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="7950f-118">Dans l’exemple précédent, le serveur web [Kestrel](xref:fundamentals/servers/kestrel) est utilisé.</span><span class="sxs-lookup"><span data-stu-id="7950f-118">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="7950f-119">D’autres serveurs web, tels que [WebListener](xref:fundamentals/servers/weblistener), peuvent être utilisés via l’appel de la méthode d’extension appropriée.</span><span class="sxs-lookup"><span data-stu-id="7950f-119">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="7950f-120">`UseStartup` est expliqué de manière plus détaillée dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="7950f-120">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="7950f-121">`WebHostBuilder` fournit de nombreuses méthodes facultatives, notamment `UseIISIntegration` pour l’hébergement dans IIS et IIS Express et `UseContentRoot` pour la spécification du répertoire de contenu racine.</span><span class="sxs-lookup"><span data-stu-id="7950f-121">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="7950f-122">Les méthodes `Build` et `Run` génèrent l’objet `IWebHost`, qui héberge l’application et démarre l’écoute des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="7950f-122">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="7950f-123">Démarrage</span><span class="sxs-lookup"><span data-stu-id="7950f-123">Startup</span></span>

<span data-ttu-id="7950f-124">La méthode `UseStartup` sur `WebHostBuilder` spécifie la classe `Startup` pour votre application :</span><span class="sxs-lookup"><span data-stu-id="7950f-124">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7950f-125">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7950f-125">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7950f-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7950f-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

<span data-ttu-id="7950f-127">La classe `Startup` est l’emplacement où vous définissez le pipeline de traitement des requêtes et où sont configurés les services nécessaires à l’application.</span><span class="sxs-lookup"><span data-stu-id="7950f-127">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="7950f-128">La classe `Startup` doit être publique et contenir les méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="7950f-128">The `Startup` class must be public and contain the following methods:</span></span>

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

<span data-ttu-id="7950f-129">`ConfigureServices` définit les [services](#dependency-injection-services) utilisés par votre application (par exemple, ASP.NET Core MVC, Entity Framework Core et Identity).</span><span class="sxs-lookup"><span data-stu-id="7950f-129">`ConfigureServices` defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="7950f-130">`Configure` définit les [intergiciels (middleware)](xref:fundamentals/middleware) du pipeline de requêtes.</span><span class="sxs-lookup"><span data-stu-id="7950f-130">`Configure` defines the [middleware](xref:fundamentals/middleware) for the request pipeline.</span></span>

<span data-ttu-id="7950f-131">Pour plus d’informations, consultez [Démarrage de l’application](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="7950f-131">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="content-root"></a><span data-ttu-id="7950f-132">Racine de contenu</span><span class="sxs-lookup"><span data-stu-id="7950f-132">Content root</span></span>

<span data-ttu-id="7950f-133">La racine de contenu est le chemin de base de tout contenu utilisé par l’application, par exemple les vues, les [pages Razor](xref:mvc/razor-pages/index) et les composants statiques.</span><span class="sxs-lookup"><span data-stu-id="7950f-133">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:mvc/razor-pages/index), and static assets.</span></span> <span data-ttu-id="7950f-134">Par défaut, la racine de contenu est identique au chemin de base de l’application pour l’exécutable qui héberge l’application.</span><span class="sxs-lookup"><span data-stu-id="7950f-134">By default, the content root is the same as application base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="7950f-135">Racine web</span><span class="sxs-lookup"><span data-stu-id="7950f-135">Web root</span></span>

<span data-ttu-id="7950f-136">La racine web d’une application correspond au répertoire de projet qui contient les ressources publiques, statiques, telles que les fichiers CSS, JavaScript et image.</span><span class="sxs-lookup"><span data-stu-id="7950f-136">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="7950f-137">Injection de dépendances (Services)</span><span class="sxs-lookup"><span data-stu-id="7950f-137">Dependency Injection (Services)</span></span>

<span data-ttu-id="7950f-138">Un service est un composant destiné à une consommation courante dans une application.</span><span class="sxs-lookup"><span data-stu-id="7950f-138">A service is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="7950f-139">Les services sont accessibles via l’[injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7950f-139">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="7950f-140">ASP.NET Core inclut un conteneur IoC (**I**nversion **o**f **C**ontrol) natif qui prend en charge l’[injection de constructeurs](xref:mvc/controllers/dependency-injection#constructor-injection) par défaut.</span><span class="sxs-lookup"><span data-stu-id="7950f-140">ASP.NET Core includes a native **I**nversion **o**f **C**ontrol (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="7950f-141">Vous pouvez remplacer le conteneur natif par défaut si vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="7950f-141">You can replace the default native container if you wish.</span></span> <span data-ttu-id="7950f-142">L’injection de dépendances n’offre pas seulement un couplage faible, elle permet également d’accéder aux services dans l’ensemble de votre application (par exemple, la [journalisation](xref:fundamentals/logging/index)).</span><span class="sxs-lookup"><span data-stu-id="7950f-142">In addition to its loose coupling benefit, DI makes services available throughout your app (for example, [logging](xref:fundamentals/logging/index)).</span></span>

<span data-ttu-id="7950f-143">Pour plus d’informations, consultez [Injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7950f-143">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="7950f-144">Intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="7950f-144">Middleware</span></span>

<span data-ttu-id="7950f-145">Dans ASP.NET Core, vous composez votre pipeline de requêtes à l’aide d’un [intergiciel (middleware)](xref:fundamentals/middleware).</span><span class="sxs-lookup"><span data-stu-id="7950f-145">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware).</span></span> <span data-ttu-id="7950f-146">Un intergiciel (middleware) ASP.NET Core suit une logique asynchrone sur `HttpContext`, puis appelle l’intergiciel suivant de la séquence ou met fin directement à la requête.</span><span class="sxs-lookup"><span data-stu-id="7950f-146">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="7950f-147">Un composant intergiciel (middleware) appelé « XYZ » est ajouté via l’appel de la méthode d’extension `UseXYZ` dans la méthode `Configure`.</span><span class="sxs-lookup"><span data-stu-id="7950f-147">A middleware component called "XYZ" is added by invoking an `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="7950f-148">ASP.NET Core est livré avec un vaste ensemble d’intergiciels (middleware) intégrés :</span><span class="sxs-lookup"><span data-stu-id="7950f-148">ASP.NET Core comes with a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="7950f-149">Fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="7950f-149">Static files</span></span>](xref:fundamentals/static-files)
* [<span data-ttu-id="7950f-150">Routage</span><span class="sxs-lookup"><span data-stu-id="7950f-150">Routing</span></span>](xref:fundamentals/routing)
* [<span data-ttu-id="7950f-151">Authentification</span><span class="sxs-lookup"><span data-stu-id="7950f-151">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="7950f-152">Intergiciel (middleware) de compression des réponses</span><span class="sxs-lookup"><span data-stu-id="7950f-152">Response Compression Middleware</span></span>](xref:performance/response-compression)
* [<span data-ttu-id="7950f-153">Intergiciel de réécriture d’URL</span><span class="sxs-lookup"><span data-stu-id="7950f-153">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)

<span data-ttu-id="7950f-154">L’intergiciel (middleware) [OWIN](http://owin.org) est disponible pour les applications ASP.NET Core. Vous pouvez aussi écrire votre propre intergiciel personnalisé.</span><span class="sxs-lookup"><span data-stu-id="7950f-154">[OWIN](http://owin.org)-based middleware is available for ASP.NET Core apps, and you can write your own custom middleware.</span></span>

<span data-ttu-id="7950f-155">Pour plus d’informations, consultez [Intergiciel (middleware)](xref:fundamentals/middleware) et [OWIN (Open Web Interface pour .NET)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="7950f-155">For more information, see [Middleware](xref:fundamentals/middleware) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="environments"></a><span data-ttu-id="7950f-156">Environnements</span><span class="sxs-lookup"><span data-stu-id="7950f-156">Environments</span></span>

<span data-ttu-id="7950f-157">Les environnements, tels que « Développement » et « Production », sont une notion de premier plan dans ASP.NET Core. Vous pouvez les définir à l’aide de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="7950f-157">Environments, such as "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="7950f-158">Pour plus d’informations, consultez [Utilisation de plusieurs environnements](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="7950f-158">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="configuration"></a><span data-ttu-id="7950f-159">Configuration</span><span class="sxs-lookup"><span data-stu-id="7950f-159">Configuration</span></span>

<span data-ttu-id="7950f-160">ASP.NET Core utilise un modèle de configuration basé sur les paires nom-valeur.</span><span class="sxs-lookup"><span data-stu-id="7950f-160">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="7950f-161">Le modèle de configuration n’est pas basé sur `System.Configuration` ou *web.config*. La configuration obtient les paramètres d’une sélection de fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="7950f-161">The configuration model isn't based on `System.Configuration` or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="7950f-162">Les fournisseurs de configuration intégrés prennent en charge une grande variété de formats de fichiers (XML, JSON, INI), ainsi que des variables d’environnement pour activer la configuration basée sur l’environnement.</span><span class="sxs-lookup"><span data-stu-id="7950f-162">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="7950f-163">Vous pouvez également écrire vos propres fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="7950f-163">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="7950f-164">Pour plus d’informations, consultez [Configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="7950f-164">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="logging"></a><span data-ttu-id="7950f-165">Journalisation</span><span class="sxs-lookup"><span data-stu-id="7950f-165">Logging</span></span>

<span data-ttu-id="7950f-166">ASP.NET Core prend en charge une API de journalisation qui fonctionne avec divers fournisseurs de journalisation.</span><span class="sxs-lookup"><span data-stu-id="7950f-166">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="7950f-167">Les fournisseurs intégrés prennent en charge l’envoi de journaux à une ou plusieurs destinations.</span><span class="sxs-lookup"><span data-stu-id="7950f-167">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="7950f-168">Des frameworks de journalisation tiers peuvent être utilisés.</span><span class="sxs-lookup"><span data-stu-id="7950f-168">Third-party logging frameworks can be used.</span></span>

[<span data-ttu-id="7950f-169">Journalisation</span><span class="sxs-lookup"><span data-stu-id="7950f-169">Logging</span></span>](xref:fundamentals/logging/index)

## <a name="error-handling"></a><span data-ttu-id="7950f-170">Gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="7950f-170">Error handling</span></span>

<span data-ttu-id="7950f-171">ASP.NET Core offre des fonctionnalités intégrées pour la gestion des erreurs dans les applications, notamment une page d’exceptions du développeur, des pages d’erreurs personnalisées, des pages de code d’état statique et la gestion des exceptions de démarrage.</span><span class="sxs-lookup"><span data-stu-id="7950f-171">ASP.NET Core has built-in features for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="7950f-172">Pour plus d’informations, consultez [Gestion des erreurs](xref:fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="7950f-172">For more information, see [Error Handling](xref:fundamentals/error-handling).</span></span>

## <a name="routing"></a><span data-ttu-id="7950f-173">Routage</span><span class="sxs-lookup"><span data-stu-id="7950f-173">Routing</span></span>

<span data-ttu-id="7950f-174">ASP.NET Core offre des fonctionnalités pour router les requêtes d’application vers les gestionnaires de routage.</span><span class="sxs-lookup"><span data-stu-id="7950f-174">ASP.NET Core offers features for routing of app requests to route handlers.</span></span>

<span data-ttu-id="7950f-175">Pour plus d’informations, consultez [Routage](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="7950f-175">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="file-providers"></a><span data-ttu-id="7950f-176">Fournisseurs de fichiers</span><span class="sxs-lookup"><span data-stu-id="7950f-176">File providers</span></span>

<span data-ttu-id="7950f-177">ASP.NET Core fournit un accès au système de fichiers en utilisant des fournisseurs de fichiers qui offrent une interface commune permettant de travailler avec des fichiers sur plusieurs plateformes.</span><span class="sxs-lookup"><span data-stu-id="7950f-177">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="7950f-178">Pour plus d’informations, consultez [Fournisseurs de fichiers](xref:fundamentals/file-providers).</span><span class="sxs-lookup"><span data-stu-id="7950f-178">For more information, see [File Providers](xref:fundamentals/file-providers).</span></span>

## <a name="static-files"></a><span data-ttu-id="7950f-179">Fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="7950f-179">Static files</span></span>

<span data-ttu-id="7950f-180">L’intergiciel (middleware) de fichiers statiques prend en charge des fichiers statiques, tels que HTML, CSS, image et JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7950f-180">Static files middleware serves static files, such as HTML, CSS, image, and JavaScript.</span></span>

<span data-ttu-id="7950f-181">Pour plus d’informations, consultez [Utilisation des fichiers statiques](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="7950f-181">For more information, see [Working with static files](xref:fundamentals/static-files).</span></span>

## <a name="hosting"></a><span data-ttu-id="7950f-182">Hébergement</span><span class="sxs-lookup"><span data-stu-id="7950f-182">Hosting</span></span>

<span data-ttu-id="7950f-183">Les applications ASP.NET Core configurent et lancent un *hôte*, qui est responsable de la gestion du démarrage et de la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="7950f-183">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="7950f-184">Pour plus d’informations, consultez [Hébergement](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="7950f-184">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

## <a name="session-and-application-state"></a><span data-ttu-id="7950f-185">État de session et d’application</span><span class="sxs-lookup"><span data-stu-id="7950f-185">Session and application state</span></span>

<span data-ttu-id="7950f-186">L’état de session est une fonctionnalité ASP.NET Core que vous pouvez utiliser pour enregistrer et stocker des données utilisateur pendant que l’utilisateur navigue dans votre application web.</span><span class="sxs-lookup"><span data-stu-id="7950f-186">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span>

<span data-ttu-id="7950f-187">Pour plus d’informations, consultez [État de session et d’application](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="7950f-187">For more information, see [Session and application state](xref:fundamentals/app-state).</span></span>

## <a name="servers"></a><span data-ttu-id="7950f-188">Serveurs</span><span class="sxs-lookup"><span data-stu-id="7950f-188">Servers</span></span>

<span data-ttu-id="7950f-189">Le modèle d’hébergement ASP.NET Core n’écoute pas directement les requêtes.</span><span class="sxs-lookup"><span data-stu-id="7950f-189">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="7950f-190">Le modèle d’hébergement s’appuie sur une implémentation de serveur HTTP pour transférer la requête à l’application.</span><span class="sxs-lookup"><span data-stu-id="7950f-190">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="7950f-191">La requête transférée est enveloppée (wrap) dans un ensemble d’objets de fonctionnalités auxquels vous pouvez accéder via des interfaces.</span><span class="sxs-lookup"><span data-stu-id="7950f-191">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="7950f-192">ASP.NET Core inclut un serveur web multiplateforme géré, appelé [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="7950f-192">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="7950f-193">Kestrel s’exécute souvent derrière un serveur web de production comme [IIS](https://www.iis.net/) ou [Nginx](http://nginx.org).</span><span class="sxs-lookup"><span data-stu-id="7950f-193">Kestrel is often run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org).</span></span> <span data-ttu-id="7950f-194">Kestrel peut être exécuté comme serveur edge.</span><span class="sxs-lookup"><span data-stu-id="7950f-194">Kestrel can be run as an edge server.</span></span>

<span data-ttu-id="7950f-195">Pour plus d’informations, consultez [Serveurs](xref:fundamentals/servers/index) et les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="7950f-195">For more information, see [Servers](xref:fundamentals/servers/index) and the following topics:</span></span>

* [<span data-ttu-id="7950f-196">Kestrel</span><span class="sxs-lookup"><span data-stu-id="7950f-196">Kestrel</span></span>](xref:fundamentals/servers/kestrel)
* [<span data-ttu-id="7950f-197">Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7950f-197">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* <span data-ttu-id="7950f-198">[HTTP.sys](xref:fundamentals/servers/httpsys) (anciennement [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="7950f-198">[HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="7950f-199">Globalisation et localisation</span><span class="sxs-lookup"><span data-stu-id="7950f-199">Globalization and localization</span></span>

<span data-ttu-id="7950f-200">La création d’un site web multilingue avec ASP.NET Core vous permet d’atteindre un plus large public.</span><span class="sxs-lookup"><span data-stu-id="7950f-200">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="7950f-201">ASP.NET Core offre des services et des intergiciels (middleware) de traduction dans différentes langues et cultures.</span><span class="sxs-lookup"><span data-stu-id="7950f-201">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="7950f-202">Pour plus d’informations, consultez [Internationalisation et traduction](xref:fundamentals/localization).</span><span class="sxs-lookup"><span data-stu-id="7950f-202">For more information, see [Globalization and localization](xref:fundamentals/localization).</span></span>

## <a name="request-features"></a><span data-ttu-id="7950f-203">Fonctionnalités de requête</span><span class="sxs-lookup"><span data-stu-id="7950f-203">Request features</span></span>

<span data-ttu-id="7950f-204">Les détails d’implémentation d’un serveur web relatifs aux requêtes HTTP et aux réponses sont définis dans les interfaces.</span><span class="sxs-lookup"><span data-stu-id="7950f-204">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="7950f-205">Ces interfaces sont utilisées par les implémentations de serveur et les intergiciels (middleware) pour créer et modifier le pipeline d’hébergement de l’application.</span><span class="sxs-lookup"><span data-stu-id="7950f-205">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="7950f-206">Pour plus d’informations, consultez [Fonctionnalités de requête](xref:fundamentals/request-features).</span><span class="sxs-lookup"><span data-stu-id="7950f-206">For more information, see [Request Features](xref:fundamentals/request-features).</span></span>

## <a name="open-web-interface-for-net-owin"></a><span data-ttu-id="7950f-207">OWIN (Open Web Interface pour .NET)</span><span class="sxs-lookup"><span data-stu-id="7950f-207">Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="7950f-208">ASP.NET Core prend en charge l’interface Open Web pour .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="7950f-208">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="7950f-209">OWIN permet que les applications web soient dissociées des serveurs web.</span><span class="sxs-lookup"><span data-stu-id="7950f-209">OWIN allows web apps to be decoupled from web servers.</span></span>

<span data-ttu-id="7950f-210">Pour plus d’informations, consultez [Interface Open Web pour .NET (OWIN)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="7950f-210">For more information, see [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="websockets"></a><span data-ttu-id="7950f-211">WebSockets</span><span class="sxs-lookup"><span data-stu-id="7950f-211">WebSockets</span></span>

<span data-ttu-id="7950f-212">[WebSocket](https://wikipedia.org/wiki/WebSocket) est un protocole qui autorise des canaux de communication persistants bidirectionnels sur les connexions TCP.</span><span class="sxs-lookup"><span data-stu-id="7950f-212">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="7950f-213">Il est utilisé pour des applications comme le chat, les transactions boursières, les jeux et partout où vous voulez des fonctionnalités en temps réel dans une application web.</span><span class="sxs-lookup"><span data-stu-id="7950f-213">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="7950f-214">ASP.NET Core prend en charge les fonctionnalités de socket web.</span><span class="sxs-lookup"><span data-stu-id="7950f-214">ASP.NET Core supports web socket features.</span></span>

<span data-ttu-id="7950f-215">Pour plus d’informations, consultez [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="7950f-215">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="7950f-216">Métapackage Microsoft.AspNetCore.All</span><span class="sxs-lookup"><span data-stu-id="7950f-216">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="7950f-217">Le métapackage [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) pour ASP.NET Core comprend :</span><span class="sxs-lookup"><span data-stu-id="7950f-217">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="7950f-218">Tous les packages pris en charge par l’équipe ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7950f-218">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="7950f-219">Tous les packages pris en charge par Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7950f-219">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="7950f-220">Les dépendances internes et tierces utilisées par ASP.NET Core et Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7950f-220">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="7950f-221">Pour plus d’informations, consultez [Métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="7950f-221">For more information, see [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="7950f-222">Runtime .NET Core et runtime .NET Framework</span><span class="sxs-lookup"><span data-stu-id="7950f-222">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="7950f-223">Une application ASP.NET Core peut cibler le runtime .NET Core ou .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7950f-223">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="7950f-224">Pour plus d’informations, consultez [Choix entre .NET Core et .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="7950f-224">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="7950f-225">Choisir entre ASP.NET Core et ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7950f-225">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="7950f-226">Pour plus d’informations sur le choix entre ASP.NET Core et ASP.NET, consultez [Choisir entre ASP.NET Core et ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span><span class="sxs-lookup"><span data-stu-id="7950f-226">For more information on choosing between ASP.NET Core and ASP.NET, see [Choose between ASP.NET Core and ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span></span>
