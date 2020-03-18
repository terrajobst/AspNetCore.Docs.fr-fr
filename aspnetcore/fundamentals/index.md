---
title: Notions de base d’ASP.NET Core
author: rick-anderson
description: Découvrez les concepts de base permettant de créer des applications ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2020
uid: fundamentals/index
ms.openlocfilehash: a16a2fbb4ad2a79f96b6646ffdc359619d361a25
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434315"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="bc773-103">Notions de base d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bc773-103">ASP.NET Core fundamentals</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="bc773-104">Cet article est une vue d’ensemble des sujets clés pour comprendre comment développer des applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bc773-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="bc773-105">Classe Startup</span><span class="sxs-lookup"><span data-stu-id="bc773-105">The Startup class</span></span>

<span data-ttu-id="bc773-106">La classe `Startup` est l’endroit où :</span><span class="sxs-lookup"><span data-stu-id="bc773-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="bc773-107">Les services nécessaires à l’application sont configurés.</span><span class="sxs-lookup"><span data-stu-id="bc773-107">Services required by the app are configured.</span></span>
* <span data-ttu-id="bc773-108">Le pipeline de traitement des requêtes est défini.</span><span class="sxs-lookup"><span data-stu-id="bc773-108">The request handling pipeline is defined.</span></span>

<span data-ttu-id="bc773-109">Les *services* sont des composants utilisés par l’application.</span><span class="sxs-lookup"><span data-stu-id="bc773-109">*Services* are components that are used by the app.</span></span> <span data-ttu-id="bc773-110">Par exemple, un composant de journalisation est un service.</span><span class="sxs-lookup"><span data-stu-id="bc773-110">For example, a logging component is a service.</span></span> <span data-ttu-id="bc773-111">Le code pour configurer (ou *enregistrer*) des services est ajouté à la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="bc773-111">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="bc773-112">Le pipeline de traitement des requêtes est composé d’une série de composants d’*intergiciel (middleware)* .</span><span class="sxs-lookup"><span data-stu-id="bc773-112">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="bc773-113">Par exemple, un intergiciel (middleware) peut gérer les demandes de fichiers statiques ou rediriger les requêtes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bc773-113">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="bc773-114">Chaque intergiciel (middleware) effectue des opérations asynchrones sur `HttpContext`, puis appelle l’intergiciel (middleware) suivant dans le pipeline ou met fin à la requête.</span><span class="sxs-lookup"><span data-stu-id="bc773-114">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="bc773-115">Le code pour configurer le pipeline de traitement des requêtes est ajouté à la méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="bc773-115">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="bc773-116">Voici un exemple de classe `Startup` :</span><span class="sxs-lookup"><span data-stu-id="bc773-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="bc773-117">Pour plus d’informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="bc773-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="bc773-118">Injection de dépendances (services)</span><span class="sxs-lookup"><span data-stu-id="bc773-118">Dependency injection (services)</span></span>

<span data-ttu-id="bc773-119">ASP.NET Core offre une infrastructure d’injection de dépendances (DI) intégrée qui rend disponibles les services configurés aux classes d’une application.</span><span class="sxs-lookup"><span data-stu-id="bc773-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="bc773-120">Pour obtenir une instance d’un service dans une classe, vous pouvez créer un constructeur avec un paramètre du type requis.</span><span class="sxs-lookup"><span data-stu-id="bc773-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="bc773-121">Le paramètre peut être le type de service ou une interface.</span><span class="sxs-lookup"><span data-stu-id="bc773-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="bc773-122">Le système d’injection de dépendances fournit le service lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="bc773-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="bc773-123">Voici une classe qui utilise l’injection de dépendances pour obtenir un objet de contexte Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="bc773-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="bc773-124">La ligne en surbrillance est un exemple d’injection de constructeur :</span><span class="sxs-lookup"><span data-stu-id="bc773-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="bc773-125">Si l’injection de dépendances est intégrée, elle est conçue pour vous permettre d’incorporer un conteneur d’inversion de contrôle (IoC) tiers si vous préférez.</span><span class="sxs-lookup"><span data-stu-id="bc773-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="bc773-126">Pour plus d’informations, consultez <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="bc773-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="bc773-127">Middlewares</span><span class="sxs-lookup"><span data-stu-id="bc773-127">Middleware</span></span>

<span data-ttu-id="bc773-128">Le pipeline de traitement des requêtes est composé comme une série de composants d’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="bc773-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="bc773-129">Chaque composant effectue des opérations asynchrones sur `HttpContext`, puis appelle l’intergiciel (middleware) suivant dans le pipeline ou met fin à la requête.</span><span class="sxs-lookup"><span data-stu-id="bc773-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="bc773-130">Par convention, un composant d’intergiciel (middleware) est ajouté au pipeline en appelant sa méthode d’extension `Use...` dans la méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="bc773-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="bc773-131">Par exemple, pour activer le rendu des fichiers statiques, appelez `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="bc773-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="bc773-132">Le code en surbrillance dans l’exemple suivant configure le pipeline de traitement des requêtes :</span><span class="sxs-lookup"><span data-stu-id="bc773-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="bc773-133">ASP.NET Core inclut un ensemble complet de middlewares intégrés, et vous pouvez écrire un middleware personnalisé.</span><span class="sxs-lookup"><span data-stu-id="bc773-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="bc773-134">Pour plus d’informations, consultez <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="bc773-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="bc773-135">Host</span><span class="sxs-lookup"><span data-stu-id="bc773-135">Host</span></span>

<span data-ttu-id="bc773-136">Une application ASP.NET Core génère un *hôte* au démarrage.</span><span class="sxs-lookup"><span data-stu-id="bc773-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="bc773-137">L’hôte est un objet qui encapsule toutes les ressources de l’application, telles que :</span><span class="sxs-lookup"><span data-stu-id="bc773-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="bc773-138">Une implémentation serveur HTTP</span><span class="sxs-lookup"><span data-stu-id="bc773-138">An HTTP server implementation</span></span>
* <span data-ttu-id="bc773-139">Composants d’intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="bc773-139">Middleware components</span></span>
* <span data-ttu-id="bc773-140">Journalisation</span><span class="sxs-lookup"><span data-stu-id="bc773-140">Logging</span></span>
* <span data-ttu-id="bc773-141">DI</span><span class="sxs-lookup"><span data-stu-id="bc773-141">DI</span></span>
* <span data-ttu-id="bc773-142">Configuration</span><span class="sxs-lookup"><span data-stu-id="bc773-142">Configuration</span></span>

<span data-ttu-id="bc773-143">La principale raison d’inclure toutes les ressources interdépendantes de l’application dans un objet est la gestion de la durée de vie : contrôler le démarrage de l’application et l’arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="bc773-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="bc773-144">Deux hôtes sont disponibles : l’hôte générique et l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="bc773-144">Two hosts are available: the Generic Host and the Web Host.</span></span> <span data-ttu-id="bc773-145">L’hôte générique est recommandé. L’hôte web est disponible uniquement pour des raisons de compatibilité descendante.</span><span class="sxs-lookup"><span data-stu-id="bc773-145">The Generic Host is recommended, and the Web Host is available only for backwards compatibility.</span></span>

<span data-ttu-id="bc773-146">Le code permettant de créer un hôte se trouve dans `Program.Main` :</span><span class="sxs-lookup"><span data-stu-id="bc773-146">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs)]

<span data-ttu-id="bc773-147">Les méthodes `CreateDefaultBuilder` et `ConfigureWebHostDefaults` permettent de configurer un hôte avec les options fréquemment utilisées, notamment :</span><span class="sxs-lookup"><span data-stu-id="bc773-147">The `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods configure a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="bc773-148">Utilisez [Kestrel](#servers) en tant que serveur web et activez l’intégration IIS.</span><span class="sxs-lookup"><span data-stu-id="bc773-148">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="bc773-149">Chargez la configuration à partir de *appsettings.json*, *appsettings.{Environment Name}.json*, des variables d’environnement, des arguments de ligne de commande et d’autres sources de configuration.</span><span class="sxs-lookup"><span data-stu-id="bc773-149">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="bc773-150">Envoyez la sortie de journalisation aux fournisseurs Console et Debug.</span><span class="sxs-lookup"><span data-stu-id="bc773-150">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="bc773-151">Pour plus d’informations, consultez <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="bc773-151">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="non-web-scenarios"></a><span data-ttu-id="bc773-152">Scénarios non basés sur le web</span><span class="sxs-lookup"><span data-stu-id="bc773-152">Non-web scenarios</span></span>

<span data-ttu-id="bc773-153">L’hôte générique permet à d’autres types d’application d’utiliser des extensions de framework composites, par exemple la journalisation, l’injection de dépendance, la configuration et la gestion de la durée de vie de l’application.</span><span class="sxs-lookup"><span data-stu-id="bc773-153">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="bc773-154">Pour plus d'informations, consultez les rubriques <xref:fundamentals/host/generic-host> et <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="bc773-154">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="bc773-155">Serveurs</span><span class="sxs-lookup"><span data-stu-id="bc773-155">Servers</span></span>

<span data-ttu-id="bc773-156">Une application ASP.NET Core utilise une implémentation de serveur HTTP pour écouter les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="bc773-156">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="bc773-157">Les surfaces de serveur envoient des requêtes à l’application comme un ensemble de [fonctionnalités de requête](xref:fundamentals/request-features) composées dans un `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="bc773-157">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

# <a name="windows"></a>[<span data-ttu-id="bc773-158">Windows</span><span class="sxs-lookup"><span data-stu-id="bc773-158">Windows</span></span>](#tab/windows)

<span data-ttu-id="bc773-159">ASP.NET Core fournit les implémentations de serveur suivantes :</span><span class="sxs-lookup"><span data-stu-id="bc773-159">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="bc773-160">*Kestrel* est un serveur web multiplateforme.</span><span class="sxs-lookup"><span data-stu-id="bc773-160">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="bc773-161">Kestrel est souvent exécuté dans une configuration de proxy inverse à l’aide d’[IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="bc773-161">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="bc773-162">Dans ASP.NET Core 2.0 ou versions ultérieures, Kestrel peut être exécuté en tant que serveur de périphérie public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="bc773-162">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="bc773-163">*Le serveur HTTP IIS* est un serveur pour Windows qui utilise IIS.</span><span class="sxs-lookup"><span data-stu-id="bc773-163">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="bc773-164">Avec ce serveur, l’application ASP.NET Core et IIS s’exécutent dans le même processus.</span><span class="sxs-lookup"><span data-stu-id="bc773-164">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="bc773-165">*HTTP.sys* est un serveur Windows qui n’est pas utilisé avec IIS.</span><span class="sxs-lookup"><span data-stu-id="bc773-165">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="bc773-166">macOS</span><span class="sxs-lookup"><span data-stu-id="bc773-166">macOS</span></span>](#tab/macos)

<span data-ttu-id="bc773-167">ASP.NET Core fournit l’implémentation du serveur multiplateforme *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="bc773-167">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="bc773-168">Dans ASP.NET Core 2.0 ou versions ultérieures, Kestrel peut être exécuté en tant que serveur de périphérie public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="bc773-168">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="bc773-169">Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="bc773-169">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="bc773-170">Linux</span><span class="sxs-lookup"><span data-stu-id="bc773-170">Linux</span></span>](#tab/linux)

<span data-ttu-id="bc773-171">ASP.NET Core fournit l’implémentation du serveur multiplateforme *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="bc773-171">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="bc773-172">Dans ASP.NET Core 2.0 ou versions ultérieures, Kestrel peut être exécuté en tant que serveur de périphérie public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="bc773-172">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="bc773-173">Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="bc773-173">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

<span data-ttu-id="bc773-174">Pour plus d’informations, consultez <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="bc773-174">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="bc773-175">Configuration</span><span class="sxs-lookup"><span data-stu-id="bc773-175">Configuration</span></span>

<span data-ttu-id="bc773-176">ASP.NET Core fournit une infrastructure de configuration qui obtient des paramètres en tant que paires nom-valeur à partir d’un ensemble ordonné de fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="bc773-176">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="bc773-177">Il existe des fournisseurs de configuration intégrés pour une grande variété de sources, tels que des fichiers *.json*, *.xml*, des variables d’environnement et des arguments de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="bc773-177">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="bc773-178">Vous pouvez également écrire des fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="bc773-178">You can also write custom configuration providers.</span></span>

<span data-ttu-id="bc773-179">Par exemple, vous pouvez spécifier que la configuration provient de *appsettings.json* et de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="bc773-179">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="bc773-180">Lorsque la valeur de *ConnectionString* est demandée, l’infrastructure recherche d’abord dans le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bc773-180">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="bc773-181">Si la valeur est trouvée dans ce fichier, mais également dans une variable d’environnement, la valeur de la variable d’environnement est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="bc773-181">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="bc773-182">Pour gérer des données de configuration confidentielles telles que les mots de passe, ASP.NET Core fournit un [outil Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="bc773-182">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="bc773-183">Pour les secrets de production, nous vous recommandons [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="bc773-183">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="bc773-184">Pour plus d’informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="bc773-184">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="bc773-185">Options</span><span class="sxs-lookup"><span data-stu-id="bc773-185">Options</span></span>

<span data-ttu-id="bc773-186">Lorsque cela est possible, ASP.NET Core suit le *modèle d’options* pour stocker et récupérer des valeurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="bc773-186">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="bc773-187">Le modèle d’options utilise des classes pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="bc773-187">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="bc773-188">Par exemple, le code suivant définit des options WebSockets :</span><span class="sxs-lookup"><span data-stu-id="bc773-188">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="bc773-189">Pour plus d’informations, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="bc773-189">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="bc773-190">de développement</span><span class="sxs-lookup"><span data-stu-id="bc773-190">Environments</span></span>

<span data-ttu-id="bc773-191">Les environnements d’exécution, tels que *Développement*, *Mise en lots* et *Production*, sont une notion de premier plan dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bc773-191">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="bc773-192">Vous pouvez spécifier l’environnement d’exécution d’une application en définissant la variable d’environnement `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="bc773-192">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="bc773-193">ASP.NET Core lit la variable d’environnement au démarrage de l’application et stocke la valeur dans une implémentation `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="bc773-193">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="bc773-194">L’objet d’environnement est disponible partout dans l’application par le biais de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="bc773-194">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="bc773-195">L’exemple de code suivant de la classe `Startup` configure l’application pour fournir des informations d’erreur détaillées uniquement lorsqu’elle s’exécute en développement :</span><span class="sxs-lookup"><span data-stu-id="bc773-195">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="bc773-196">Pour plus d’informations, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="bc773-196">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="bc773-197">Journalisation</span><span class="sxs-lookup"><span data-stu-id="bc773-197">Logging</span></span>

<span data-ttu-id="bc773-198">ASP.NET Core prend en charge une API de journalisation qui fonctionne avec un large éventail de fournisseurs de journalisation intégrés et tiers.</span><span class="sxs-lookup"><span data-stu-id="bc773-198">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="bc773-199">Les fournisseurs disponibles sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="bc773-199">Available providers include the following:</span></span>

* <span data-ttu-id="bc773-200">Console</span><span class="sxs-lookup"><span data-stu-id="bc773-200">Console</span></span>
* <span data-ttu-id="bc773-201">Débogage</span><span class="sxs-lookup"><span data-stu-id="bc773-201">Debug</span></span>
* <span data-ttu-id="bc773-202">Suivi des événements sur Windows</span><span class="sxs-lookup"><span data-stu-id="bc773-202">Event Tracing on Windows</span></span>
* <span data-ttu-id="bc773-203">Journal des événements Windows</span><span class="sxs-lookup"><span data-stu-id="bc773-203">Windows Event Log</span></span>
* <span data-ttu-id="bc773-204">TraceSource</span><span class="sxs-lookup"><span data-stu-id="bc773-204">TraceSource</span></span>
* <span data-ttu-id="bc773-205">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="bc773-205">Azure App Service</span></span>
* <span data-ttu-id="bc773-206">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="bc773-206">Azure Application Insights</span></span>

<span data-ttu-id="bc773-207">Écrivez des journaux à partir de n’importe quel emplacement dans le code d’une application en obtenant un objet `ILogger` à partir de l’injection de dépendances et en appelant les méthodes de journal.</span><span class="sxs-lookup"><span data-stu-id="bc773-207">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="bc773-208">Voici un exemple de code qui utilise un objet `ILogger`, avec l’injection de constructeur et les appels de méthode de journalisation mis en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="bc773-208">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="bc773-209">L’interface `ILogger` vous permet de passer un certain nombre de champs au fournisseur de journalisation.</span><span class="sxs-lookup"><span data-stu-id="bc773-209">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="bc773-210">Les champs sont couramment utilisés pour construire une chaîne de message, mais le fournisseur peut également les envoyer en tant que champs distincts dans un magasin de données.</span><span class="sxs-lookup"><span data-stu-id="bc773-210">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="bc773-211">Cette fonctionnalité permet aux fournisseurs de journalisation d’implémenter la [journalisation sémantique, également appelée journalisation structurée](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="bc773-211">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="bc773-212">Pour plus d’informations, consultez <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="bc773-212">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="bc773-213">Routage</span><span class="sxs-lookup"><span data-stu-id="bc773-213">Routing</span></span>

<span data-ttu-id="bc773-214">Un *itinéraire* est un modèle d’URL qui est mappé à un gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="bc773-214">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="bc773-215">Le gestionnaire est généralement une page Razor, une méthode d’action dans un contrôleur MVC, ou un intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="bc773-215">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="bc773-216">Le routage ASP.NET Core vous permet de contrôler les URL utilisées par votre application.</span><span class="sxs-lookup"><span data-stu-id="bc773-216">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="bc773-217">Pour plus d’informations, consultez <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="bc773-217">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="bc773-218">Gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="bc773-218">Error handling</span></span>

<span data-ttu-id="bc773-219">ASP.NET Core offre des fonctionnalités intégrées pour gérer des erreurs, telles que :</span><span class="sxs-lookup"><span data-stu-id="bc773-219">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="bc773-220">Une page d’exceptions du développeur</span><span class="sxs-lookup"><span data-stu-id="bc773-220">A developer exception page</span></span>
* <span data-ttu-id="bc773-221">Pages d’erreur personnalisées</span><span class="sxs-lookup"><span data-stu-id="bc773-221">Custom error pages</span></span>
* <span data-ttu-id="bc773-222">Pages de codes d’état statique</span><span class="sxs-lookup"><span data-stu-id="bc773-222">Static status code pages</span></span>
* <span data-ttu-id="bc773-223">Gestion des exceptions de démarrage</span><span class="sxs-lookup"><span data-stu-id="bc773-223">Startup exception handling</span></span>

<span data-ttu-id="bc773-224">Pour plus d’informations, consultez <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="bc773-224">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="bc773-225">Effectuer des requêtes HTTP</span><span class="sxs-lookup"><span data-stu-id="bc773-225">Make HTTP requests</span></span>

<span data-ttu-id="bc773-226">Une implémentation de `IHttpClientFactory` est disponible pour la création d’instances `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="bc773-226">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="bc773-227">La fabrique :</span><span class="sxs-lookup"><span data-stu-id="bc773-227">The factory:</span></span>

* <span data-ttu-id="bc773-228">Fournit un emplacement central pour le nommage et la configuration d’instance de `HttpClient` logiques.</span><span class="sxs-lookup"><span data-stu-id="bc773-228">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="bc773-229">Par exemple, un client *github* peut être inscrit et configuré pour accéder à GitHub.</span><span class="sxs-lookup"><span data-stu-id="bc773-229">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="bc773-230">Un client par défaut peut être inscrit à d’autres fins.</span><span class="sxs-lookup"><span data-stu-id="bc773-230">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="bc773-231">Prend en charge l’inscription et le chaînage de plusieurs gestionnaires de délégation pour créer un pipeline de middlewares pour les requêtes sortantes.</span><span class="sxs-lookup"><span data-stu-id="bc773-231">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="bc773-232">Ce modèle est similaire au pipeline de middlewares entrants dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bc773-232">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="bc773-233">Le modèle fournit un mécanisme permettant de gérer les problèmes transversaux liés aux des requêtes HTTP, notamment la mise en cache, la gestion des erreurs, la sérialisation et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="bc773-233">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="bc773-234">S’intègre à *Polly*, une bibliothèque tierce populaire pour la gestion des erreurs temporaires.</span><span class="sxs-lookup"><span data-stu-id="bc773-234">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="bc773-235">Gère le regroupement et la durée de vie des instances de `HttpClientMessageHandler` sous-jacentes pour éviter les problèmes DNS courants qui se produisent lors de la gestion manuelle des durées de vie de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="bc773-235">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="bc773-236">Ajoute une expérience de journalisation configurable (via `ILogger`) pour toutes les requêtes envoyées via des clients créés par la fabrique.</span><span class="sxs-lookup"><span data-stu-id="bc773-236">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="bc773-237">Pour plus d’informations, consultez <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="bc773-237">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="bc773-238">Racine de contenu</span><span class="sxs-lookup"><span data-stu-id="bc773-238">Content root</span></span>

<span data-ttu-id="bc773-239">La racine du contenu est le chemin d’accès de base à :</span><span class="sxs-lookup"><span data-stu-id="bc773-239">The content root is the base path to the:</span></span>

* <span data-ttu-id="bc773-240">Exécutable hébergeant l’application ( *. exe*).</span><span class="sxs-lookup"><span data-stu-id="bc773-240">Executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="bc773-241">Assemblys compilés qui composent l’application ( *. dll*).</span><span class="sxs-lookup"><span data-stu-id="bc773-241">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="bc773-242">Fichiers de contenu sans code utilisés par l’application, tels que :</span><span class="sxs-lookup"><span data-stu-id="bc773-242">Non-code content files used by the app, such as:</span></span>
  * <span data-ttu-id="bc773-243">Fichiers Razor ( *. cshtml*, *. Razor*)</span><span class="sxs-lookup"><span data-stu-id="bc773-243">Razor files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="bc773-244">Fichiers de configuration ( *. JSON*, *. xml*)</span><span class="sxs-lookup"><span data-stu-id="bc773-244">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="bc773-245">Fichiers de données ( *. db*)</span><span class="sxs-lookup"><span data-stu-id="bc773-245">Data files (*.db*)</span></span>
* <span data-ttu-id="bc773-246">[Racine Web](#web-root), en général le dossier *wwwroot* publié.</span><span class="sxs-lookup"><span data-stu-id="bc773-246">[Web root](#web-root), typically the published *wwwroot* folder.</span></span>

<span data-ttu-id="bc773-247">Lors du développement :</span><span class="sxs-lookup"><span data-stu-id="bc773-247">During development:</span></span>

* <span data-ttu-id="bc773-248">La racine du contenu est par défaut le répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="bc773-248">The content root defaults to the project's root directory.</span></span>
* <span data-ttu-id="bc773-249">Le répertoire racine du projet est utilisé pour créer le :</span><span class="sxs-lookup"><span data-stu-id="bc773-249">The project's root directory is used to create the:</span></span>
  * <span data-ttu-id="bc773-250">Chemin d’accès aux fichiers de contenu sans code de l’application dans le répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="bc773-250">Path to the app's non-code content files in the project's root directory.</span></span>
  * <span data-ttu-id="bc773-251">[Racine Web](#web-root), généralement le dossier *wwwroot* dans le répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="bc773-251">[Web root](#web-root), typically the *wwwroot* folder in the project's root directory.</span></span>

<span data-ttu-id="bc773-252">Vous pouvez spécifier un autre chemin d’accès racine [de contenu lors de la génération de l’hôte](#host).</span><span class="sxs-lookup"><span data-stu-id="bc773-252">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="bc773-253">Pour plus d’informations, consultez <xref:fundamentals/host/generic-host#contentrootpath>.</span><span class="sxs-lookup"><span data-stu-id="bc773-253">For more information, see <xref:fundamentals/host/generic-host#contentrootpath>.</span></span>

## <a name="web-root"></a><span data-ttu-id="bc773-254">Racine web</span><span class="sxs-lookup"><span data-stu-id="bc773-254">Web root</span></span>

<span data-ttu-id="bc773-255">La racine Web est le chemin de base des fichiers de ressources statiques, non-code et publics, tels que :</span><span class="sxs-lookup"><span data-stu-id="bc773-255">The web root is the base path to public, non-code, static resource files, such as:</span></span>

* <span data-ttu-id="bc773-256">Feuilles de style ( *. CSS*)</span><span class="sxs-lookup"><span data-stu-id="bc773-256">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="bc773-257">JavaScript ( *. js*)</span><span class="sxs-lookup"><span data-stu-id="bc773-257">JavaScript (*.js*)</span></span>
* <span data-ttu-id="bc773-258">Images ( *. png*, *. jpg*)</span><span class="sxs-lookup"><span data-stu-id="bc773-258">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="bc773-259">Les fichiers statiques sont pris en charge par défaut uniquement à partir du répertoire racine Web (et des sous-répertoires).</span><span class="sxs-lookup"><span data-stu-id="bc773-259">Static files are only served by default from the web root directory (and sub-directories).</span></span>

<span data-ttu-id="bc773-260">Le chemin d’accès racine Web a comme valeur par défaut *{content root}/wwwroot*, mais une autre racine Web peut être spécifiée lors de [la génération de l’hôte](#host).</span><span class="sxs-lookup"><span data-stu-id="bc773-260">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="bc773-261">Pour plus d’informations, consultez <xref:fundamentals/host/generic-host#webroot>.</span><span class="sxs-lookup"><span data-stu-id="bc773-261">For more information, see <xref:fundamentals/host/generic-host#webroot>.</span></span>

<span data-ttu-id="bc773-262">Empêchez la publication de fichiers dans *wwwroot* avec le [contenu\<> élément de projet](/visualstudio/msbuild/common-msbuild-project-items#content) dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="bc773-262">Prevent publishing files in *wwwroot* with the [\<Content> project item](/visualstudio/msbuild/common-msbuild-project-items#content) in the project file.</span></span> <span data-ttu-id="bc773-263">L’exemple suivant empêche la publication de contenu dans le répertoire et les sous-répertoires *wwwroot/local* :</span><span class="sxs-lookup"><span data-stu-id="bc773-263">The following example prevents publishing content in the *wwwroot/local* directory and sub-directories:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="bc773-264">Pour empêcher la publication de ressources d’identité statiques sur la racine Web, consultez <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span><span class="sxs-lookup"><span data-stu-id="bc773-264">To prevent publishing static Identity assets to the web root, see <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span></span>

<span data-ttu-id="bc773-265">Dans les fichiers Razor ( *. cshtml*), le tilde-slash (`~/`) pointe vers la racine Web.</span><span class="sxs-lookup"><span data-stu-id="bc773-265">In Razor (*.cshtml*) files, the tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="bc773-266">Un chemin d’accès commençant par `~/` est désigné sous le terme de « *chemin d’accès virtuel*».</span><span class="sxs-lookup"><span data-stu-id="bc773-266">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="bc773-267">Pour plus d’informations, consultez <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="bc773-267">For more information, see <xref:fundamentals/static-files>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="bc773-268">Cet article est une vue d’ensemble des sujets clés pour comprendre comment développer des applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bc773-268">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="bc773-269">Classe Startup</span><span class="sxs-lookup"><span data-stu-id="bc773-269">The Startup class</span></span>

<span data-ttu-id="bc773-270">La classe `Startup` est l’endroit où :</span><span class="sxs-lookup"><span data-stu-id="bc773-270">The `Startup` class is where:</span></span>

* <span data-ttu-id="bc773-271">Les services nécessaires à l’application sont configurés.</span><span class="sxs-lookup"><span data-stu-id="bc773-271">Services required by the app are configured.</span></span>
* <span data-ttu-id="bc773-272">Le pipeline de traitement des requêtes est défini.</span><span class="sxs-lookup"><span data-stu-id="bc773-272">The request handling pipeline is defined.</span></span>

<span data-ttu-id="bc773-273">Les *services* sont des composants utilisés par l’application.</span><span class="sxs-lookup"><span data-stu-id="bc773-273">*Services* are components that are used by the app.</span></span> <span data-ttu-id="bc773-274">Par exemple, un composant de journalisation est un service.</span><span class="sxs-lookup"><span data-stu-id="bc773-274">For example, a logging component is a service.</span></span> <span data-ttu-id="bc773-275">Le code pour configurer (ou *enregistrer*) des services est ajouté à la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="bc773-275">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="bc773-276">Le pipeline de traitement des requêtes est composé d’une série de composants d’*intergiciel (middleware)* .</span><span class="sxs-lookup"><span data-stu-id="bc773-276">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="bc773-277">Par exemple, un intergiciel (middleware) peut gérer les demandes de fichiers statiques ou rediriger les requêtes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bc773-277">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="bc773-278">Chaque intergiciel (middleware) effectue des opérations asynchrones sur `HttpContext`, puis appelle l’intergiciel (middleware) suivant dans le pipeline ou met fin à la requête.</span><span class="sxs-lookup"><span data-stu-id="bc773-278">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="bc773-279">Le code pour configurer le pipeline de traitement des requêtes est ajouté à la méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="bc773-279">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="bc773-280">Voici un exemple de classe `Startup` :</span><span class="sxs-lookup"><span data-stu-id="bc773-280">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="bc773-281">Pour plus d’informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="bc773-281">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="bc773-282">Injection de dépendances (services)</span><span class="sxs-lookup"><span data-stu-id="bc773-282">Dependency injection (services)</span></span>

<span data-ttu-id="bc773-283">ASP.NET Core offre une infrastructure d’injection de dépendances (DI) intégrée qui rend disponibles les services configurés aux classes d’une application.</span><span class="sxs-lookup"><span data-stu-id="bc773-283">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="bc773-284">Pour obtenir une instance d’un service dans une classe, vous pouvez créer un constructeur avec un paramètre du type requis.</span><span class="sxs-lookup"><span data-stu-id="bc773-284">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="bc773-285">Le paramètre peut être le type de service ou une interface.</span><span class="sxs-lookup"><span data-stu-id="bc773-285">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="bc773-286">Le système d’injection de dépendances fournit le service lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="bc773-286">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="bc773-287">Voici une classe qui utilise l’injection de dépendances pour obtenir un objet de contexte Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="bc773-287">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="bc773-288">La ligne en surbrillance est un exemple d’injection de constructeur :</span><span class="sxs-lookup"><span data-stu-id="bc773-288">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="bc773-289">Si l’injection de dépendances est intégrée, elle est conçue pour vous permettre d’incorporer un conteneur d’inversion de contrôle (IoC) tiers si vous préférez.</span><span class="sxs-lookup"><span data-stu-id="bc773-289">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="bc773-290">Pour plus d’informations, consultez <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="bc773-290">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="bc773-291">Middlewares</span><span class="sxs-lookup"><span data-stu-id="bc773-291">Middleware</span></span>

<span data-ttu-id="bc773-292">Le pipeline de traitement des requêtes est composé comme une série de composants d’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="bc773-292">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="bc773-293">Chaque composant effectue des opérations asynchrones sur `HttpContext`, puis appelle l’intergiciel (middleware) suivant dans le pipeline ou met fin à la requête.</span><span class="sxs-lookup"><span data-stu-id="bc773-293">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="bc773-294">Par convention, un composant d’intergiciel (middleware) est ajouté au pipeline en appelant sa méthode d’extension `Use...` dans la méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="bc773-294">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="bc773-295">Par exemple, pour activer le rendu des fichiers statiques, appelez `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="bc773-295">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="bc773-296">Le code en surbrillance dans l’exemple suivant configure le pipeline de traitement des requêtes :</span><span class="sxs-lookup"><span data-stu-id="bc773-296">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="bc773-297">ASP.NET Core inclut un ensemble complet de middlewares intégrés, et vous pouvez écrire un middleware personnalisé.</span><span class="sxs-lookup"><span data-stu-id="bc773-297">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="bc773-298">Pour plus d’informations, consultez <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="bc773-298">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="bc773-299">Host</span><span class="sxs-lookup"><span data-stu-id="bc773-299">Host</span></span>

<span data-ttu-id="bc773-300">Une application ASP.NET Core génère un *hôte* au démarrage.</span><span class="sxs-lookup"><span data-stu-id="bc773-300">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="bc773-301">L’hôte est un objet qui encapsule toutes les ressources de l’application, telles que :</span><span class="sxs-lookup"><span data-stu-id="bc773-301">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="bc773-302">Une implémentation serveur HTTP</span><span class="sxs-lookup"><span data-stu-id="bc773-302">An HTTP server implementation</span></span>
* <span data-ttu-id="bc773-303">Composants d’intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="bc773-303">Middleware components</span></span>
* <span data-ttu-id="bc773-304">Journalisation</span><span class="sxs-lookup"><span data-stu-id="bc773-304">Logging</span></span>
* <span data-ttu-id="bc773-305">DI</span><span class="sxs-lookup"><span data-stu-id="bc773-305">DI</span></span>
* <span data-ttu-id="bc773-306">Configuration</span><span class="sxs-lookup"><span data-stu-id="bc773-306">Configuration</span></span>

<span data-ttu-id="bc773-307">La principale raison d’inclure toutes les ressources interdépendantes de l’application dans un objet est la gestion de la durée de vie : contrôler le démarrage de l’application et l’arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="bc773-307">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="bc773-308">Deux hôtes sont disponibles : l’hôte web et l’hôte générique.</span><span class="sxs-lookup"><span data-stu-id="bc773-308">Two hosts are available: the Web Host and the Generic Host.</span></span> <span data-ttu-id="bc773-309">En ASP.NET Core 2.x, l’hôte générique est destiné uniquement aux scénarios non basés sur le web.</span><span class="sxs-lookup"><span data-stu-id="bc773-309">In ASP.NET Core 2.x, the Generic Host is only for non-web scenarios.</span></span>

<span data-ttu-id="bc773-310">Le code permettant de créer un hôte se trouve dans `Program.Main` :</span><span class="sxs-lookup"><span data-stu-id="bc773-310">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs)]

<span data-ttu-id="bc773-311">La méthode `CreateDefaultBuilder` permet de configurer un hôte avec les options fréquemment utilisées, notamment :</span><span class="sxs-lookup"><span data-stu-id="bc773-311">The `CreateDefaultBuilder` method configures a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="bc773-312">Utilisez [Kestrel](#servers) en tant que serveur web et activez l’intégration IIS.</span><span class="sxs-lookup"><span data-stu-id="bc773-312">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="bc773-313">Chargez la configuration à partir de *appsettings.json*, *appsettings.{Environment Name}.json*, des variables d’environnement, des arguments de ligne de commande et d’autres sources de configuration.</span><span class="sxs-lookup"><span data-stu-id="bc773-313">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="bc773-314">Envoyez la sortie de journalisation aux fournisseurs Console et Debug.</span><span class="sxs-lookup"><span data-stu-id="bc773-314">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="bc773-315">Pour plus d’informations, consultez <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="bc773-315">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="non-web-scenarios"></a><span data-ttu-id="bc773-316">Scénarios non basés sur le web</span><span class="sxs-lookup"><span data-stu-id="bc773-316">Non-web scenarios</span></span>

<span data-ttu-id="bc773-317">L’hôte générique permet à d’autres types d’application d’utiliser des extensions de framework composites, par exemple la journalisation, l’injection de dépendance, la configuration et la gestion de la durée de vie de l’application.</span><span class="sxs-lookup"><span data-stu-id="bc773-317">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="bc773-318">Pour plus d'informations, consultez les rubriques <xref:fundamentals/host/generic-host> et <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="bc773-318">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="bc773-319">Serveurs</span><span class="sxs-lookup"><span data-stu-id="bc773-319">Servers</span></span>

<span data-ttu-id="bc773-320">Une application ASP.NET Core utilise une implémentation de serveur HTTP pour écouter les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="bc773-320">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="bc773-321">Les surfaces de serveur envoient des requêtes à l’application comme un ensemble de [fonctionnalités de requête](xref:fundamentals/request-features) composées dans un `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="bc773-321">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

# <a name="windows"></a>[<span data-ttu-id="bc773-322">Windows</span><span class="sxs-lookup"><span data-stu-id="bc773-322">Windows</span></span>](#tab/windows)

<span data-ttu-id="bc773-323">ASP.NET Core fournit les implémentations de serveur suivantes :</span><span class="sxs-lookup"><span data-stu-id="bc773-323">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="bc773-324">*Kestrel* est un serveur web multiplateforme.</span><span class="sxs-lookup"><span data-stu-id="bc773-324">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="bc773-325">Kestrel est souvent exécuté dans une configuration de proxy inverse à l’aide d’[IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="bc773-325">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="bc773-326">Kestrel peut être exécuté en tant que serveur Edge public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="bc773-326">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="bc773-327">*Le serveur HTTP IIS* est un serveur pour Windows qui utilise IIS.</span><span class="sxs-lookup"><span data-stu-id="bc773-327">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="bc773-328">Avec ce serveur, l’application ASP.NET Core et IIS s’exécutent dans le même processus.</span><span class="sxs-lookup"><span data-stu-id="bc773-328">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="bc773-329">*HTTP.sys* est un serveur Windows qui n’est pas utilisé avec IIS.</span><span class="sxs-lookup"><span data-stu-id="bc773-329">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="bc773-330">macOS</span><span class="sxs-lookup"><span data-stu-id="bc773-330">macOS</span></span>](#tab/macos)

<span data-ttu-id="bc773-331">ASP.NET Core fournit l’implémentation du serveur multiplateforme *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="bc773-331">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="bc773-332">Kestrel peut être exécuté en tant que serveur Edge public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="bc773-332">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="bc773-333">Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="bc773-333">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="bc773-334">Linux</span><span class="sxs-lookup"><span data-stu-id="bc773-334">Linux</span></span>](#tab/linux)

<span data-ttu-id="bc773-335">ASP.NET Core fournit l’implémentation du serveur multiplateforme *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="bc773-335">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="bc773-336">Kestrel peut être exécuté en tant que serveur Edge public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="bc773-336">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="bc773-337">Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="bc773-337">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windows"></a>[<span data-ttu-id="bc773-338">Windows</span><span class="sxs-lookup"><span data-stu-id="bc773-338">Windows</span></span>](#tab/windows)

<span data-ttu-id="bc773-339">ASP.NET Core fournit les implémentations de serveur suivantes :</span><span class="sxs-lookup"><span data-stu-id="bc773-339">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="bc773-340">*Kestrel* est un serveur web multiplateforme.</span><span class="sxs-lookup"><span data-stu-id="bc773-340">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="bc773-341">Kestrel est souvent exécuté dans une configuration de proxy inverse à l’aide d’[IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="bc773-341">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="bc773-342">Kestrel peut être exécuté en tant que serveur Edge public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="bc773-342">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="bc773-343">*HTTP.sys* est un serveur Windows qui n’est pas utilisé avec IIS.</span><span class="sxs-lookup"><span data-stu-id="bc773-343">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="bc773-344">macOS</span><span class="sxs-lookup"><span data-stu-id="bc773-344">macOS</span></span>](#tab/macos)

<span data-ttu-id="bc773-345">ASP.NET Core fournit l’implémentation du serveur multiplateforme *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="bc773-345">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="bc773-346">Kestrel peut être exécuté en tant que serveur Edge public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="bc773-346">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="bc773-347">Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="bc773-347">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="bc773-348">Linux</span><span class="sxs-lookup"><span data-stu-id="bc773-348">Linux</span></span>](#tab/linux)

<span data-ttu-id="bc773-349">ASP.NET Core fournit l’implémentation du serveur multiplateforme *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="bc773-349">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="bc773-350">Kestrel peut être exécuté en tant que serveur Edge public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="bc773-350">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="bc773-351">Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="bc773-351">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="bc773-352">Pour plus d’informations, consultez <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="bc773-352">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="bc773-353">Configuration</span><span class="sxs-lookup"><span data-stu-id="bc773-353">Configuration</span></span>

<span data-ttu-id="bc773-354">ASP.NET Core fournit une infrastructure de configuration qui obtient des paramètres en tant que paires nom-valeur à partir d’un ensemble ordonné de fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="bc773-354">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="bc773-355">Il existe des fournisseurs de configuration intégrés pour une grande variété de sources, tels que des fichiers *.json*, *.xml*, des variables d’environnement et des arguments de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="bc773-355">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="bc773-356">Vous pouvez également écrire des fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="bc773-356">You can also write custom configuration providers.</span></span>

<span data-ttu-id="bc773-357">Par exemple, vous pouvez spécifier que la configuration provient de *appsettings.json* et de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="bc773-357">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="bc773-358">Lorsque la valeur de *ConnectionString* est demandée, l’infrastructure recherche d’abord dans le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bc773-358">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="bc773-359">Si la valeur est trouvée dans ce fichier, mais également dans une variable d’environnement, la valeur de la variable d’environnement est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="bc773-359">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="bc773-360">Pour gérer des données de configuration confidentielles telles que les mots de passe, ASP.NET Core fournit un [outil Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="bc773-360">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="bc773-361">Pour les secrets de production, nous vous recommandons [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="bc773-361">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="bc773-362">Pour plus d’informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="bc773-362">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="bc773-363">Options</span><span class="sxs-lookup"><span data-stu-id="bc773-363">Options</span></span>

<span data-ttu-id="bc773-364">Lorsque cela est possible, ASP.NET Core suit le *modèle d’options* pour stocker et récupérer des valeurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="bc773-364">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="bc773-365">Le modèle d’options utilise des classes pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="bc773-365">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="bc773-366">Par exemple, le code suivant définit des options WebSockets :</span><span class="sxs-lookup"><span data-stu-id="bc773-366">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="bc773-367">Pour plus d’informations, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="bc773-367">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="bc773-368">de développement</span><span class="sxs-lookup"><span data-stu-id="bc773-368">Environments</span></span>

<span data-ttu-id="bc773-369">Les environnements d’exécution, tels que *Développement*, *Mise en lots* et *Production*, sont une notion de premier plan dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bc773-369">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="bc773-370">Vous pouvez spécifier l’environnement d’exécution d’une application en définissant la variable d’environnement `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="bc773-370">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="bc773-371">ASP.NET Core lit la variable d’environnement au démarrage de l’application et stocke la valeur dans une implémentation `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="bc773-371">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="bc773-372">L’objet d’environnement est disponible partout dans l’application par le biais de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="bc773-372">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="bc773-373">L’exemple de code suivant de la classe `Startup` configure l’application pour fournir des informations d’erreur détaillées uniquement lorsqu’elle s’exécute en développement :</span><span class="sxs-lookup"><span data-stu-id="bc773-373">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="bc773-374">Pour plus d’informations, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="bc773-374">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="bc773-375">Journalisation</span><span class="sxs-lookup"><span data-stu-id="bc773-375">Logging</span></span>

<span data-ttu-id="bc773-376">ASP.NET Core prend en charge une API de journalisation qui fonctionne avec un large éventail de fournisseurs de journalisation intégrés et tiers.</span><span class="sxs-lookup"><span data-stu-id="bc773-376">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="bc773-377">Les fournisseurs disponibles sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="bc773-377">Available providers include the following:</span></span>

* <span data-ttu-id="bc773-378">Console</span><span class="sxs-lookup"><span data-stu-id="bc773-378">Console</span></span>
* <span data-ttu-id="bc773-379">Débogage</span><span class="sxs-lookup"><span data-stu-id="bc773-379">Debug</span></span>
* <span data-ttu-id="bc773-380">Suivi des événements sur Windows</span><span class="sxs-lookup"><span data-stu-id="bc773-380">Event Tracing on Windows</span></span>
* <span data-ttu-id="bc773-381">Journal des événements Windows</span><span class="sxs-lookup"><span data-stu-id="bc773-381">Windows Event Log</span></span>
* <span data-ttu-id="bc773-382">TraceSource</span><span class="sxs-lookup"><span data-stu-id="bc773-382">TraceSource</span></span>
* <span data-ttu-id="bc773-383">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="bc773-383">Azure App Service</span></span>
* <span data-ttu-id="bc773-384">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="bc773-384">Azure Application Insights</span></span>

<span data-ttu-id="bc773-385">Écrivez des journaux à partir de n’importe quel emplacement dans le code d’une application en obtenant un objet `ILogger` à partir de l’injection de dépendances et en appelant les méthodes de journal.</span><span class="sxs-lookup"><span data-stu-id="bc773-385">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="bc773-386">Voici un exemple de code qui utilise un objet `ILogger`, avec l’injection de constructeur et les appels de méthode de journalisation mis en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="bc773-386">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="bc773-387">L’interface `ILogger` vous permet de passer un certain nombre de champs au fournisseur de journalisation.</span><span class="sxs-lookup"><span data-stu-id="bc773-387">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="bc773-388">Les champs sont couramment utilisés pour construire une chaîne de message, mais le fournisseur peut également les envoyer en tant que champs distincts dans un magasin de données.</span><span class="sxs-lookup"><span data-stu-id="bc773-388">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="bc773-389">Cette fonctionnalité permet aux fournisseurs de journalisation d’implémenter la [journalisation sémantique, également appelée journalisation structurée](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="bc773-389">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="bc773-390">Pour plus d’informations, consultez <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="bc773-390">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="bc773-391">Routage</span><span class="sxs-lookup"><span data-stu-id="bc773-391">Routing</span></span>

<span data-ttu-id="bc773-392">Un *itinéraire* est un modèle d’URL qui est mappé à un gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="bc773-392">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="bc773-393">Le gestionnaire est généralement une page Razor, une méthode d’action dans un contrôleur MVC, ou un intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="bc773-393">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="bc773-394">Le routage ASP.NET Core vous permet de contrôler les URL utilisées par votre application.</span><span class="sxs-lookup"><span data-stu-id="bc773-394">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="bc773-395">Pour plus d’informations, consultez <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="bc773-395">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="bc773-396">Gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="bc773-396">Error handling</span></span>

<span data-ttu-id="bc773-397">ASP.NET Core offre des fonctionnalités intégrées pour gérer des erreurs, telles que :</span><span class="sxs-lookup"><span data-stu-id="bc773-397">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="bc773-398">Une page d’exceptions du développeur</span><span class="sxs-lookup"><span data-stu-id="bc773-398">A developer exception page</span></span>
* <span data-ttu-id="bc773-399">Pages d’erreur personnalisées</span><span class="sxs-lookup"><span data-stu-id="bc773-399">Custom error pages</span></span>
* <span data-ttu-id="bc773-400">Pages de codes d’état statique</span><span class="sxs-lookup"><span data-stu-id="bc773-400">Static status code pages</span></span>
* <span data-ttu-id="bc773-401">Gestion des exceptions de démarrage</span><span class="sxs-lookup"><span data-stu-id="bc773-401">Startup exception handling</span></span>

<span data-ttu-id="bc773-402">Pour plus d’informations, consultez <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="bc773-402">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="bc773-403">Effectuer des requêtes HTTP</span><span class="sxs-lookup"><span data-stu-id="bc773-403">Make HTTP requests</span></span>

<span data-ttu-id="bc773-404">Une implémentation de `IHttpClientFactory` est disponible pour la création d’instances `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="bc773-404">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="bc773-405">La fabrique :</span><span class="sxs-lookup"><span data-stu-id="bc773-405">The factory:</span></span>

* <span data-ttu-id="bc773-406">Fournit un emplacement central pour le nommage et la configuration d’instance de `HttpClient` logiques.</span><span class="sxs-lookup"><span data-stu-id="bc773-406">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="bc773-407">Par exemple, un client *github* peut être inscrit et configuré pour accéder à GitHub.</span><span class="sxs-lookup"><span data-stu-id="bc773-407">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="bc773-408">Un client par défaut peut être inscrit à d’autres fins.</span><span class="sxs-lookup"><span data-stu-id="bc773-408">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="bc773-409">Prend en charge l’inscription et le chaînage de plusieurs gestionnaires de délégation pour créer un pipeline de middlewares pour les requêtes sortantes.</span><span class="sxs-lookup"><span data-stu-id="bc773-409">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="bc773-410">Ce modèle est similaire au pipeline de middlewares entrants dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bc773-410">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="bc773-411">Le modèle fournit un mécanisme permettant de gérer les problèmes transversaux liés aux des requêtes HTTP, notamment la mise en cache, la gestion des erreurs, la sérialisation et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="bc773-411">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="bc773-412">S’intègre à *Polly*, une bibliothèque tierce populaire pour la gestion des erreurs temporaires.</span><span class="sxs-lookup"><span data-stu-id="bc773-412">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="bc773-413">Gère le regroupement et la durée de vie des instances de `HttpClientMessageHandler` sous-jacentes pour éviter les problèmes DNS courants qui se produisent lors de la gestion manuelle des durées de vie de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="bc773-413">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="bc773-414">Ajoute une expérience de journalisation configurable (via `ILogger`) pour toutes les requêtes envoyées via des clients créés par la fabrique.</span><span class="sxs-lookup"><span data-stu-id="bc773-414">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="bc773-415">Pour plus d’informations, consultez <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="bc773-415">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="bc773-416">Racine de contenu</span><span class="sxs-lookup"><span data-stu-id="bc773-416">Content root</span></span>

<span data-ttu-id="bc773-417">La racine du contenu est le chemin d’accès de base à :</span><span class="sxs-lookup"><span data-stu-id="bc773-417">The content root is the base path to the:</span></span>

* <span data-ttu-id="bc773-418">Exécutable hébergeant l’application ( *. exe*).</span><span class="sxs-lookup"><span data-stu-id="bc773-418">Executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="bc773-419">Assemblys compilés qui composent l’application ( *. dll*).</span><span class="sxs-lookup"><span data-stu-id="bc773-419">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="bc773-420">Fichiers de contenu sans code utilisés par l’application, tels que :</span><span class="sxs-lookup"><span data-stu-id="bc773-420">Non-code content files used by the app, such as:</span></span>
  * <span data-ttu-id="bc773-421">Fichiers Razor ( *. cshtml*, *. Razor*)</span><span class="sxs-lookup"><span data-stu-id="bc773-421">Razor files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="bc773-422">Fichiers de configuration ( *. JSON*, *. xml*)</span><span class="sxs-lookup"><span data-stu-id="bc773-422">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="bc773-423">Fichiers de données ( *. db*)</span><span class="sxs-lookup"><span data-stu-id="bc773-423">Data files (*.db*)</span></span>
* <span data-ttu-id="bc773-424">[Racine Web](#web-root), en général le dossier *wwwroot* publié.</span><span class="sxs-lookup"><span data-stu-id="bc773-424">[Web root](#web-root), typically the published *wwwroot* folder.</span></span>

<span data-ttu-id="bc773-425">Lors du développement :</span><span class="sxs-lookup"><span data-stu-id="bc773-425">During development:</span></span>

* <span data-ttu-id="bc773-426">La racine du contenu est par défaut le répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="bc773-426">The content root defaults to the project's root directory.</span></span>
* <span data-ttu-id="bc773-427">Le répertoire racine du projet est utilisé pour créer le :</span><span class="sxs-lookup"><span data-stu-id="bc773-427">The project's root directory is used to create the:</span></span>
  * <span data-ttu-id="bc773-428">Chemin d’accès aux fichiers de contenu sans code de l’application dans le répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="bc773-428">Path to the app's non-code content files in the project's root directory.</span></span>
  * <span data-ttu-id="bc773-429">[Racine Web](#web-root), généralement le dossier *wwwroot* dans le répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="bc773-429">[Web root](#web-root), typically the *wwwroot* folder in the project's root directory.</span></span>

<span data-ttu-id="bc773-430">Vous pouvez spécifier un autre chemin d’accès racine [de contenu lors de la génération de l’hôte](#host).</span><span class="sxs-lookup"><span data-stu-id="bc773-430">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="bc773-431">Pour plus d’informations, consultez <xref:fundamentals/host/web-host#content-root>.</span><span class="sxs-lookup"><span data-stu-id="bc773-431">For more information, see <xref:fundamentals/host/web-host#content-root>.</span></span>

## <a name="web-root"></a><span data-ttu-id="bc773-432">Racine web</span><span class="sxs-lookup"><span data-stu-id="bc773-432">Web root</span></span>

<span data-ttu-id="bc773-433">La racine Web est le chemin de base des fichiers de ressources statiques, non-code et publics, tels que :</span><span class="sxs-lookup"><span data-stu-id="bc773-433">The web root is the base path to public, non-code, static resource files, such as:</span></span>

* <span data-ttu-id="bc773-434">Feuilles de style ( *. CSS*)</span><span class="sxs-lookup"><span data-stu-id="bc773-434">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="bc773-435">JavaScript ( *. js*)</span><span class="sxs-lookup"><span data-stu-id="bc773-435">JavaScript (*.js*)</span></span>
* <span data-ttu-id="bc773-436">Images ( *. png*, *. jpg*)</span><span class="sxs-lookup"><span data-stu-id="bc773-436">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="bc773-437">Les fichiers statiques sont pris en charge par défaut uniquement à partir du répertoire racine Web (et des sous-répertoires).</span><span class="sxs-lookup"><span data-stu-id="bc773-437">Static files are only served by default from the web root directory (and sub-directories).</span></span>

<span data-ttu-id="bc773-438">Le chemin d’accès racine Web a comme valeur par défaut *{content root}/wwwroot*, mais une autre racine Web peut être spécifiée lors de [la génération de l’hôte](#host).</span><span class="sxs-lookup"><span data-stu-id="bc773-438">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="bc773-439">Pour plus d’informations, consultez [Racine web](xref:fundamentals/host/web-host#web-root).</span><span class="sxs-lookup"><span data-stu-id="bc773-439">For more information, see [Web root](xref:fundamentals/host/web-host#web-root).</span></span>

<span data-ttu-id="bc773-440">Empêchez la publication de fichiers dans *wwwroot* avec le [contenu\<> élément de projet](/visualstudio/msbuild/common-msbuild-project-items#content) dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="bc773-440">Prevent publishing files in *wwwroot* with the [\<Content> project item](/visualstudio/msbuild/common-msbuild-project-items#content) in the project file.</span></span> <span data-ttu-id="bc773-441">L’exemple suivant empêche la publication de contenu dans le répertoire et les sous-répertoires *wwwroot/local* :</span><span class="sxs-lookup"><span data-stu-id="bc773-441">The following example prevents publishing content in the *wwwroot/local* directory and sub-directories:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="bc773-442">Dans les fichiers Razor ( *. cshtml*), le tilde-slash (`~/`) pointe vers la racine Web.</span><span class="sxs-lookup"><span data-stu-id="bc773-442">In Razor (*.cshtml*) files, the tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="bc773-443">Un chemin d’accès commençant par `~/` est désigné sous le terme de « *chemin d’accès virtuel*».</span><span class="sxs-lookup"><span data-stu-id="bc773-443">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="bc773-444">Pour plus d’informations, consultez <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="bc773-444">For more information, see <xref:fundamentals/static-files>.</span></span>

::: moniker-end