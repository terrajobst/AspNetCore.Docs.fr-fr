---
title: Notions de base d’ASP.NET Core
author: rick-anderson
description: Découvrez les concepts de base permettant de créer des applications ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/31/2019
uid: fundamentals/index
ms.openlocfilehash: a1fed574db0baab391ebb9cfc44664ceddbfa69b
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58809287"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="75984-103">Notions de base d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="75984-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="75984-104">Cet article est une vue d’ensemble des sujets clés pour comprendre comment développer des applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="75984-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="75984-105">Classe Startup</span><span class="sxs-lookup"><span data-stu-id="75984-105">The Startup class</span></span>

<span data-ttu-id="75984-106">La classe `Startup` est l’endroit où :</span><span class="sxs-lookup"><span data-stu-id="75984-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="75984-107">Tous les services requis par l’application sont configurés.</span><span class="sxs-lookup"><span data-stu-id="75984-107">Any services required by the app are configured.</span></span>
* <span data-ttu-id="75984-108">Le pipeline de traitement des requêtes est défini.</span><span class="sxs-lookup"><span data-stu-id="75984-108">The request handling pipeline is defined.</span></span>

* <span data-ttu-id="75984-109">Le code pour configurer (ou *enregistrer*) des services est ajouté à la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="75984-109">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="75984-110">Les *services* sont des composants utilisés par l’application.</span><span class="sxs-lookup"><span data-stu-id="75984-110">*Services* are components that are used by the app.</span></span> <span data-ttu-id="75984-111">Par exemple, un objet de contexte Entity Framework Core est un service.</span><span class="sxs-lookup"><span data-stu-id="75984-111">For example, an Entity Framework Core context object is a service.</span></span>
* <span data-ttu-id="75984-112">Le code pour configurer le pipeline de traitement des requêtes est ajouté à la méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="75984-112">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span> <span data-ttu-id="75984-113">Le pipeline est composé comme une série de composants d’*intergiciel (middleware)*.</span><span class="sxs-lookup"><span data-stu-id="75984-113">The pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="75984-114">Par exemple, un intergiciel (middleware) peut gérer les demandes de fichiers statiques ou rediriger les requêtes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="75984-114">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="75984-115">Chaque intergiciel (middleware) effectue des opérations asynchrones sur `HttpContext`, puis appelle l’intergiciel (middleware) suivant dans le pipeline ou met fin à la requête.</span><span class="sxs-lookup"><span data-stu-id="75984-115">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="75984-116">Voici un exemple de classe `Startup` :</span><span class="sxs-lookup"><span data-stu-id="75984-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="75984-117">Pour plus d'informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="75984-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="75984-118">Injection de dépendances (services)</span><span class="sxs-lookup"><span data-stu-id="75984-118">Dependency injection (services)</span></span>

<span data-ttu-id="75984-119">ASP.NET Core offre une infrastructure d’injection de dépendances (DI) intégrée qui rend disponibles les services configurés aux classes d’une application.</span><span class="sxs-lookup"><span data-stu-id="75984-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="75984-120">Pour obtenir une instance d’un service dans une classe, vous pouvez créer un constructeur avec un paramètre du type requis.</span><span class="sxs-lookup"><span data-stu-id="75984-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="75984-121">Le paramètre peut être le type de service ou une interface.</span><span class="sxs-lookup"><span data-stu-id="75984-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="75984-122">Le système d’injection de dépendances fournit le service lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="75984-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="75984-123">Voici une classe qui utilise l’injection de dépendances pour obtenir un objet de contexte Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="75984-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="75984-124">La ligne en surbrillance est un exemple d’injection de constructeur :</span><span class="sxs-lookup"><span data-stu-id="75984-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="75984-125">Si l’injection de dépendances est intégrée, elle est conçue pour vous permettre d’incorporer un conteneur d’inversion de contrôle (IoC) tiers si vous préférez.</span><span class="sxs-lookup"><span data-stu-id="75984-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="75984-126">Pour plus d'informations, consultez <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="75984-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="75984-127">Intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="75984-127">Middleware</span></span>

<span data-ttu-id="75984-128">Le pipeline de traitement des requêtes est composé comme une série de composants d’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="75984-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="75984-129">Chaque composant effectue des opérations asynchrones sur `HttpContext`, puis appelle l’intergiciel (middleware) suivant dans le pipeline ou met fin à la requête.</span><span class="sxs-lookup"><span data-stu-id="75984-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="75984-130">Par convention, un composant d’intergiciel (middleware) est ajouté au pipeline en appelant sa méthode d’extension `Use...` dans la méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="75984-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="75984-131">Par exemple, pour activer le rendu des fichiers statiques, appelez `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="75984-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="75984-132">Le code en surbrillance dans l’exemple suivant configure le pipeline de traitement des requêtes :</span><span class="sxs-lookup"><span data-stu-id="75984-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="75984-133">ASP.NET Core inclut un ensemble complet de middlewares intégrés, et vous pouvez écrire un middleware personnalisé.</span><span class="sxs-lookup"><span data-stu-id="75984-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="75984-134">Pour plus d'informations, consultez <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="75984-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

<a id="host"/>

## <a name="the-host"></a><span data-ttu-id="75984-135">L’hôte</span><span class="sxs-lookup"><span data-stu-id="75984-135">The host</span></span>

<span data-ttu-id="75984-136">Une application ASP.NET Core génère un *hôte* au démarrage.</span><span class="sxs-lookup"><span data-stu-id="75984-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="75984-137">L’hôte est un objet qui encapsule toutes les ressources de l’application, telles que :</span><span class="sxs-lookup"><span data-stu-id="75984-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="75984-138">Une implémentation serveur HTTP</span><span class="sxs-lookup"><span data-stu-id="75984-138">An HTTP server implementation</span></span>
* <span data-ttu-id="75984-139">Composants d’intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="75984-139">Middleware components</span></span>
* <span data-ttu-id="75984-140">Journalisation</span><span class="sxs-lookup"><span data-stu-id="75984-140">Logging</span></span>
* <span data-ttu-id="75984-141">INJECTION DE DÉPENDANCES</span><span class="sxs-lookup"><span data-stu-id="75984-141">DI</span></span>
* <span data-ttu-id="75984-142">Configuration</span><span class="sxs-lookup"><span data-stu-id="75984-142">Configuration</span></span>

<span data-ttu-id="75984-143">La principale raison d’inclure toutes les ressources interdépendantes de l’application dans un objet est la gestion de la durée de vie : contrôler le démarrage de l’application et l’arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="75984-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="75984-144">Le code pour créer un hôte se trouve dans `Program.Main` et suit le [modèle de conception](https://wikipedia.org/wiki/Builder_pattern).</span><span class="sxs-lookup"><span data-stu-id="75984-144">The code to create a host is in `Program.Main` and follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern).</span></span> <span data-ttu-id="75984-145">Les méthodes sont appelées pour configurer chaque ressource faisant partie de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="75984-145">Methods are called to configure each resource that is part of the host.</span></span> <span data-ttu-id="75984-146">Une méthode de générateur est appelée pour tout rassembler et instancier l’objet hôte.</span><span class="sxs-lookup"><span data-stu-id="75984-146">A builder method is called to pull it all together and instantiate the host object.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="75984-147">Dans ASP.NET Core 3.0 ou version ultérieure, un hôte générique (classe `Host`) ou un hôte web (classe `WebHost`) peut être utilisé dans une application web.</span><span class="sxs-lookup"><span data-stu-id="75984-147">In ASP.NET Core 3.0 or later, Generic Host (`Host` class) or Web Host (`WebHost` class) can be used in a web app.</span></span> <span data-ttu-id="75984-148">L’hôte générique est recommandée, et l’hôte web est disponible pour la compatibilité descendante.</span><span class="sxs-lookup"><span data-stu-id="75984-148">Generic Host is recommended, and Web Host is available for backwards compatibility.</span></span>

<span data-ttu-id="75984-149">Le framework fournit les méthodes `CreateDefaultBuilder` et `ConfigureWebHostDefaults` pour définir un hôte avec des options fréquemment utilisées, telles que :</span><span class="sxs-lookup"><span data-stu-id="75984-149">The framework provides the `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods to set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="75984-150">Utilisez [Kestrel](#servers) en tant que serveur web et activez l’intégration IIS.</span><span class="sxs-lookup"><span data-stu-id="75984-150">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="75984-151">Chargez la configuration à partir de *appsettings.json*, *appsettings.{Environment Name}.json*, des variables d’environnement, des arguments de ligne de commande et d’autres sources de configuration.</span><span class="sxs-lookup"><span data-stu-id="75984-151">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="75984-152">Envoyez la sortie de journalisation aux fournisseurs Console et Debug.</span><span class="sxs-lookup"><span data-stu-id="75984-152">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="75984-153">Voici l’exemple de code qui génère un hôte.</span><span class="sxs-lookup"><span data-stu-id="75984-153">Here's sample code that builds a host.</span></span> <span data-ttu-id="75984-154">Les méthodes qui configurent l’hôte avec les options fréquemment utilisées sont mises en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="75984-154">The methods that set up the host with commonly used options are highlighted:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs?highlight=9-10)]

<span data-ttu-id="75984-155">Pour plus d’informations, consultez <xref:fundamentals/host/generic-host> et <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="75984-155">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="75984-156">ASP.NET Core 2.x utilise l’hôte web (classe `WebHost`) pour les applications web.</span><span class="sxs-lookup"><span data-stu-id="75984-156">ASP.NET Core 2.x uses Web Host (`WebHost` class) for web apps.</span></span> <span data-ttu-id="75984-157">Le framework fournit `CreateDefaultBuilder` pour définir un hôte avec des options fréquemment utilisées, telles que :</span><span class="sxs-lookup"><span data-stu-id="75984-157">The framework provides `CreateDefaultBuilder` to set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="75984-158">Utilisez [Kestrel](#servers) en tant que serveur web et activez l’intégration IIS.</span><span class="sxs-lookup"><span data-stu-id="75984-158">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="75984-159">Chargez la configuration à partir de *appsettings.json*, *appsettings.{Environment Name}.json*, des variables d’environnement, des arguments de ligne de commande et d’autres sources de configuration.</span><span class="sxs-lookup"><span data-stu-id="75984-159">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="75984-160">Envoyez la sortie de journalisation aux fournisseurs Console et Debug.</span><span class="sxs-lookup"><span data-stu-id="75984-160">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="75984-161">Voici l’exemple de code qui génère un hôte :</span><span class="sxs-lookup"><span data-stu-id="75984-161">Here's sample code that builds a host:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs?highlight=9)]

<span data-ttu-id="75984-162">Pour plus d'informations, consultez <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="75984-162">For more information, see <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

### <a name="advanced-host-scenarios"></a><span data-ttu-id="75984-163">Scénarios d’hôte avancés</span><span class="sxs-lookup"><span data-stu-id="75984-163">Advanced host scenarios</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="75984-164">L’hôte générique est disponible pour une utilisation par toutes les applications .NET Core&mdash;pas uniquement les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="75984-164">Generic Host is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="75984-165">L’hôte générique (classe `Host`) permet à d’autres types d’applications d’utiliser des extensions de framework composites telles que la journalisation, l’injection de dépendances, la configuration et la gestion de la durée de vie d’application.</span><span class="sxs-lookup"><span data-stu-id="75984-165">Generic Host (`Host` class) allows other types of apps to use cross-cutting framework extensions, such as logging, DI, configuration, and app lifetime management.</span></span> <span data-ttu-id="75984-166">Pour plus d'informations, consultez <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="75984-166">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="75984-167">L’hôte web est conçu pour inclure une implémentation de serveur HTTP, ce qui n’est pas nécessaire pour d’autres types d’applications .NET.</span><span class="sxs-lookup"><span data-stu-id="75984-167">Web Host is designed to include an HTTP server implementation, which isn't required for other kinds of .NET apps.</span></span> <span data-ttu-id="75984-168">À compter de la version ASP.NET Core 2.1, l’hôte générique (classe `Host`) est disponible pour une utilisation par toutes les applications .NET Core&mdash;pas uniquement les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="75984-168">Starting in ASP.NET Core 2.1, the Generic Host (`Host` class) is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="75984-169">L’hôte générique permet à d’autres types d’applications d’utiliser des extensions de framework composites telles que la journalisation, l’injection de dépendances, la configuration et la gestion de la durée de vie d’application.</span><span class="sxs-lookup"><span data-stu-id="75984-169">Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, DI, configuration, and app lifetime management.</span></span> <span data-ttu-id="75984-170">Pour plus d'informations, consultez <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="75984-170">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

<span data-ttu-id="75984-171">Vous pouvez également utiliser l’hôte pour exécuter des tâches en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="75984-171">You can also use the host to run background tasks.</span></span> <span data-ttu-id="75984-172">Pour plus d'informations, consultez <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="75984-172">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="75984-173">Serveurs</span><span class="sxs-lookup"><span data-stu-id="75984-173">Servers</span></span>

<span data-ttu-id="75984-174">Une application ASP.NET Core utilise une implémentation de serveur HTTP pour écouter les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="75984-174">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="75984-175">Les surfaces de serveur envoient des requêtes à l’application comme un ensemble de [fonctionnalités de requête](xref:fundamentals/request-features) composées dans un `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="75984-175">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="75984-176">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="75984-176">Windows</span></span>](#tab/windows)

<span data-ttu-id="75984-177">ASP.NET Core fournit les implémentations de serveur suivantes :</span><span class="sxs-lookup"><span data-stu-id="75984-177">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="75984-178">*Kestrel* est un serveur web multiplateforme.</span><span class="sxs-lookup"><span data-stu-id="75984-178">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="75984-179">Kestrel est souvent exécuté dans une configuration de proxy inverse à l’aide d’[IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="75984-179">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="75984-180">Dans ASP.NET Core 2.0 ou versions ultérieures, Kestrel peut être exécuté en tant que serveur de périphérie public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="75984-180">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="75984-181">*Le serveur HTTP IIS* est un serveur pour Windows qui utilise IIS.</span><span class="sxs-lookup"><span data-stu-id="75984-181">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="75984-182">Avec ce serveur, l’application ASP.NET Core et IIS s’exécutent dans le même processus.</span><span class="sxs-lookup"><span data-stu-id="75984-182">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="75984-183">*HTTP.sys* est un serveur Windows qui n’est pas utilisé avec IIS.</span><span class="sxs-lookup"><span data-stu-id="75984-183">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="75984-184">macOS</span><span class="sxs-lookup"><span data-stu-id="75984-184">macOS</span></span>](#tab/macos)

<span data-ttu-id="75984-185">ASP.NET Core fournit l’implémentation du serveur multiplateforme *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="75984-185">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="75984-186">Dans ASP.NET Core 2.0 ou versions ultérieures, Kestrel peut être exécuté en tant que serveur de périphérie public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="75984-186">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="75984-187">Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="75984-187">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="75984-188">Linux</span><span class="sxs-lookup"><span data-stu-id="75984-188">Linux</span></span>](#tab/linux)

<span data-ttu-id="75984-189">ASP.NET Core fournit l’implémentation du serveur multiplateforme *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="75984-189">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="75984-190">Dans ASP.NET Core 2.0 ou versions ultérieures, Kestrel peut être exécuté en tant que serveur de périphérie public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="75984-190">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="75984-191">Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="75984-191">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="75984-192">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="75984-192">Windows</span></span>](#tab/windows)

<span data-ttu-id="75984-193">ASP.NET Core fournit les implémentations de serveur suivantes :</span><span class="sxs-lookup"><span data-stu-id="75984-193">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="75984-194">*Kestrel* est un serveur web multiplateforme.</span><span class="sxs-lookup"><span data-stu-id="75984-194">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="75984-195">Kestrel est souvent exécuté dans une configuration de proxy inverse à l’aide d’[IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="75984-195">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="75984-196">Dans ASP.NET Core 2.0 ou versions ultérieures, Kestrel peut être exécuté en tant que serveur de périphérie public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="75984-196">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="75984-197">*HTTP.sys* est un serveur Windows qui n’est pas utilisé avec IIS.</span><span class="sxs-lookup"><span data-stu-id="75984-197">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="75984-198">macOS</span><span class="sxs-lookup"><span data-stu-id="75984-198">macOS</span></span>](#tab/macos)

<span data-ttu-id="75984-199">ASP.NET Core fournit l’implémentation du serveur multiplateforme *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="75984-199">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="75984-200">Dans ASP.NET Core 2.0 ou versions ultérieures, Kestrel peut être exécuté en tant que serveur de périphérie public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="75984-200">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="75984-201">Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="75984-201">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="75984-202">Linux</span><span class="sxs-lookup"><span data-stu-id="75984-202">Linux</span></span>](#tab/linux)

<span data-ttu-id="75984-203">ASP.NET Core fournit l’implémentation du serveur multiplateforme *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="75984-203">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="75984-204">Dans ASP.NET Core 2.0 ou versions ultérieures, Kestrel peut être exécuté en tant que serveur de périphérie public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="75984-204">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="75984-205">Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](http://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="75984-205">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="75984-206">Pour plus d'informations, consultez <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="75984-206">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="75984-207">Configuration</span><span class="sxs-lookup"><span data-stu-id="75984-207">Configuration</span></span>

<span data-ttu-id="75984-208">ASP.NET Core fournit une infrastructure de configuration qui obtient des paramètres en tant que paires nom-valeur à partir d’un ensemble ordonné de fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="75984-208">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="75984-209">Il existe des fournisseurs de configuration intégrés pour une grande variété de sources, tels que des fichiers *.json*, *.xml*, des variables d’environnement et des arguments de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="75984-209">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="75984-210">Vous pouvez également écrire des fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="75984-210">You can also write custom configuration providers.</span></span>

<span data-ttu-id="75984-211">Par exemple, vous pouvez spécifier que la configuration provient de *appsettings.json* et de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="75984-211">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="75984-212">Lorsque la valeur de *ConnectionString* est demandée, l’infrastructure recherche d’abord dans le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="75984-212">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="75984-213">Si la valeur est trouvée dans ce fichier, mais également dans une variable d’environnement, la valeur de la variable d’environnement est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="75984-213">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="75984-214">Pour gérer des données de configuration confidentielles telles que les mots de passe, ASP.NET Core fournit un [outil Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="75984-214">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="75984-215">Pour les secrets de production, nous vous recommandons [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="75984-215">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="75984-216">Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="75984-216">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="75984-217">Options</span><span class="sxs-lookup"><span data-stu-id="75984-217">Options</span></span>

<span data-ttu-id="75984-218">Lorsque cela est possible, ASP.NET Core suit le *modèle d’options* pour stocker et récupérer des valeurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="75984-218">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="75984-219">Le modèle d’options utilise des classes pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="75984-219">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="75984-220">Par exemple, le code suivant définit des options WebSockets :</span><span class="sxs-lookup"><span data-stu-id="75984-220">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="75984-221">Pour plus d'informations, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="75984-221">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="75984-222">Environnements</span><span class="sxs-lookup"><span data-stu-id="75984-222">Environments</span></span>

<span data-ttu-id="75984-223">Les environnements d’exécution, tels que *Développement*, *Mise en lots* et *Production*, sont une notion de premier plan dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="75984-223">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="75984-224">Vous pouvez spécifier l’environnement d’exécution d’une application en définissant la variable d’environnement `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="75984-224">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="75984-225">ASP.NET Core lit la variable d’environnement au démarrage de l’application et stocke la valeur dans une implémentation `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="75984-225">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="75984-226">L’objet d’environnement est disponible partout dans l’application par le biais de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="75984-226">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="75984-227">L’exemple de code suivant de la classe `Startup` configure l’application pour fournir des informations d’erreur détaillées uniquement lorsqu’elle s’exécute en développement :</span><span class="sxs-lookup"><span data-stu-id="75984-227">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="75984-228">Pour plus d'informations, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="75984-228">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="75984-229">Journalisation</span><span class="sxs-lookup"><span data-stu-id="75984-229">Logging</span></span>

<span data-ttu-id="75984-230">ASP.NET Core prend en charge une API de journalisation qui fonctionne avec différents fournisseurs de journalisation tiers et intégrés.</span><span class="sxs-lookup"><span data-stu-id="75984-230">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="75984-231">Les fournisseurs disponibles sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="75984-231">Available providers include the following:</span></span>

* <span data-ttu-id="75984-232">Console</span><span class="sxs-lookup"><span data-stu-id="75984-232">Console</span></span>
* <span data-ttu-id="75984-233">Débogage</span><span class="sxs-lookup"><span data-stu-id="75984-233">Debug</span></span>
* <span data-ttu-id="75984-234">Suivi des événements sur Windows</span><span class="sxs-lookup"><span data-stu-id="75984-234">Event Tracing on Windows</span></span>
* <span data-ttu-id="75984-235">Journal des événements Windows</span><span class="sxs-lookup"><span data-stu-id="75984-235">Windows Event Log</span></span>
* <span data-ttu-id="75984-236">TraceSource</span><span class="sxs-lookup"><span data-stu-id="75984-236">TraceSource</span></span>
* <span data-ttu-id="75984-237">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="75984-237">Azure App Service</span></span>
* <span data-ttu-id="75984-238">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="75984-238">Azure Application Insights</span></span>

<span data-ttu-id="75984-239">Écrivez des journaux à partir de n’importe quel emplacement dans le code d’une application en obtenant un objet `ILogger` à partir de l’injection de dépendances et en appelant les méthodes de journal.</span><span class="sxs-lookup"><span data-stu-id="75984-239">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="75984-240">Voici un exemple de code qui utilise un objet `ILogger`, avec l’injection de constructeur et les appels de méthode de journalisation mis en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="75984-240">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="75984-241">L’interface `ILogger` vous permet de passer un certain nombre de champs au fournisseur de journalisation.</span><span class="sxs-lookup"><span data-stu-id="75984-241">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="75984-242">Les champs sont couramment utilisés pour construire une chaîne de message, mais le fournisseur peut également les envoyer en tant que champs distincts dans un magasin de données.</span><span class="sxs-lookup"><span data-stu-id="75984-242">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="75984-243">Cette fonctionnalité permet aux fournisseurs de journalisation d’implémenter la [journalisation sémantique, également appelée journalisation structurée](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="75984-243">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="75984-244">Pour plus d'informations, consultez <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="75984-244">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="75984-245">Routage</span><span class="sxs-lookup"><span data-stu-id="75984-245">Routing</span></span>

<span data-ttu-id="75984-246">Un *itinéraire* est un modèle d’URL qui est mappé à un gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="75984-246">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="75984-247">Le gestionnaire est généralement une page Razor, une méthode d’action dans un contrôleur MVC, ou un intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="75984-247">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="75984-248">Le routage ASP.NET Core vous permet de contrôler les URL utilisées par votre application.</span><span class="sxs-lookup"><span data-stu-id="75984-248">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="75984-249">Pour plus d'informations, consultez <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="75984-249">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="75984-250">Gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="75984-250">Error handling</span></span>

<span data-ttu-id="75984-251">ASP.NET Core offre des fonctionnalités intégrées pour gérer des erreurs, telles que :</span><span class="sxs-lookup"><span data-stu-id="75984-251">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="75984-252">Une page d’exceptions du développeur</span><span class="sxs-lookup"><span data-stu-id="75984-252">A developer exception page</span></span>
* <span data-ttu-id="75984-253">Pages d’erreurs personnalisées</span><span class="sxs-lookup"><span data-stu-id="75984-253">Custom error pages</span></span>
* <span data-ttu-id="75984-254">Pages de codes d’état statique</span><span class="sxs-lookup"><span data-stu-id="75984-254">Static status code pages</span></span>
* <span data-ttu-id="75984-255">Gestion des exceptions de démarrage</span><span class="sxs-lookup"><span data-stu-id="75984-255">Startup exception handling</span></span>

<span data-ttu-id="75984-256">Pour plus d'informations, consultez <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="75984-256">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="75984-257">Effectuer des requêtes HTTP</span><span class="sxs-lookup"><span data-stu-id="75984-257">Make HTTP requests</span></span>

<span data-ttu-id="75984-258">Une implémentation de `IHttpClientFactory` est disponible pour la création d’instances `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="75984-258">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="75984-259">La fabrique :</span><span class="sxs-lookup"><span data-stu-id="75984-259">The factory:</span></span>

* <span data-ttu-id="75984-260">Fournit un emplacement central pour le nommage et la configuration d’instance de `HttpClient` logiques.</span><span class="sxs-lookup"><span data-stu-id="75984-260">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="75984-261">Par exemple, un client *github* peut être inscrit et configuré pour accéder à GitHub.</span><span class="sxs-lookup"><span data-stu-id="75984-261">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="75984-262">Un client par défaut peut être inscrit à d’autres fins.</span><span class="sxs-lookup"><span data-stu-id="75984-262">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="75984-263">Prend en charge l’inscription et le chaînage de plusieurs gestionnaires de délégation pour créer un pipeline de middlewares pour les requêtes sortantes.</span><span class="sxs-lookup"><span data-stu-id="75984-263">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="75984-264">Ce modèle est similaire au pipeline de middlewares entrants dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="75984-264">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="75984-265">Le modèle fournit un mécanisme permettant de gérer les problèmes transversaux liés aux des requêtes HTTP, notamment la mise en cache, la gestion des erreurs, la sérialisation et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="75984-265">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="75984-266">S’intègre à *Polly*, une bibliothèque tierce populaire pour la gestion des erreurs temporaires.</span><span class="sxs-lookup"><span data-stu-id="75984-266">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="75984-267">Gère le regroupement et la durée de vie des instances de `HttpClientMessageHandler` sous-jacentes pour éviter les problèmes DNS courants qui se produisent lors de la gestion manuelle des durées de vie de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="75984-267">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="75984-268">Ajoute une expérience de journalisation configurable (via `ILogger`) pour toutes les requêtes envoyées via des clients créés par la fabrique.</span><span class="sxs-lookup"><span data-stu-id="75984-268">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="75984-269">Pour plus d'informations, consultez <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="75984-269">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="75984-270">Racine de contenu</span><span class="sxs-lookup"><span data-stu-id="75984-270">Content root</span></span>

<span data-ttu-id="75984-271">La racine de contenu est le chemin de base de tout contenu privé utilisé par l’application, comme les fichiers Razor.</span><span class="sxs-lookup"><span data-stu-id="75984-271">The content root is the base path to any private content used by the app, such as its Razor files.</span></span> <span data-ttu-id="75984-272">Par défaut, la racine de contenu est le chemin de base pour l’exécutable qui héberge l’application.</span><span class="sxs-lookup"><span data-stu-id="75984-272">By default, the content root is the base path for the executable hosting the app.</span></span> <span data-ttu-id="75984-273">Un autre emplacement peut être spécifié lors de la [création de l’hôte](#host).</span><span class="sxs-lookup"><span data-stu-id="75984-273">An alternative location can be specified when [building the host](#host).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="75984-274">Pour plus d’informations, consultez [Racine de contenu](xref:fundamentals/host/generic-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="75984-274">For more information, see [Content root](xref:fundamentals/host/generic-host#content-root).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="75984-275">Pour plus d’informations, consultez [Racine de contenu](xref:fundamentals/host/web-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="75984-275">For more information, see [Content root](xref:fundamentals/host/web-host#content-root).</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="75984-276">Racine web</span><span class="sxs-lookup"><span data-stu-id="75984-276">Web root</span></span>

<span data-ttu-id="75984-277">La racine web (également appelée *webroot*) est le chemin d’accès de base aux ressources statiques publiques, telles que les fichiers CSS, JavaScript et image.</span><span class="sxs-lookup"><span data-stu-id="75984-277">The web root (also known as *webroot*) is the base path to public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="75984-278">Par défaut, l’intergiciel (middleware) des fichiers statiques traite uniquement les fichiers provenant du répertoire racine web (et des sous-répertoires).</span><span class="sxs-lookup"><span data-stu-id="75984-278">The static files middleware will only serve files from the web root directory (and sub-directories) by default.</span></span> <span data-ttu-id="75984-279">Le chemin d’accès par défaut de la racine web est *{Content Root}/wwwroot*, mais un autre emplacement peut être spécifié lors de la [création de l’hôte](#host).</span><span class="sxs-lookup"><span data-stu-id="75984-279">The web root path defaults to *{Content Root}/wwwroot*, but a different location can be specified when [building the host](#host).</span></span>

<span data-ttu-id="75984-280">Dans les fichiers Razor (*.cshtml*), la barre oblique tilde `~/` pointe vers la racine web.</span><span class="sxs-lookup"><span data-stu-id="75984-280">In Razor (*.cshtml*) files, the tilde-slash `~/` points to the web root.</span></span> <span data-ttu-id="75984-281">Les chemins commençant par `~/` sont appelés chemins virtuels.</span><span class="sxs-lookup"><span data-stu-id="75984-281">Paths beginning with `~/` are referred to as virtual paths.</span></span>

<span data-ttu-id="75984-282">Pour plus d'informations, consultez <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="75984-282">For more information, see <xref:fundamentals/static-files>.</span></span>
