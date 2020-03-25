---
title: Notions de base d’ASP.NET Core
author: rick-anderson
description: Découvrez les concepts de base permettant de créer des applications ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2020
uid: fundamentals/index
ms.openlocfilehash: 7533242140c31a937f32cc9082d760103347ce25
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219179"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="7b122-103">Notions de base d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7b122-103">ASP.NET Core fundamentals</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7b122-104">Cet article est une vue d’ensemble des sujets clés pour comprendre comment développer des applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7b122-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="7b122-105">Classe Startup</span><span class="sxs-lookup"><span data-stu-id="7b122-105">The Startup class</span></span>

<span data-ttu-id="7b122-106">La classe `Startup` est l’endroit où :</span><span class="sxs-lookup"><span data-stu-id="7b122-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="7b122-107">Les services nécessaires à l’application sont configurés.</span><span class="sxs-lookup"><span data-stu-id="7b122-107">Services required by the app are configured.</span></span>
* <span data-ttu-id="7b122-108">Le pipeline de traitement des requêtes est défini.</span><span class="sxs-lookup"><span data-stu-id="7b122-108">The request handling pipeline is defined.</span></span>

<span data-ttu-id="7b122-109">Les *services* sont des composants utilisés par l’application.</span><span class="sxs-lookup"><span data-stu-id="7b122-109">*Services* are components that are used by the app.</span></span> <span data-ttu-id="7b122-110">Par exemple, un composant de journalisation est un service.</span><span class="sxs-lookup"><span data-stu-id="7b122-110">For example, a logging component is a service.</span></span> <span data-ttu-id="7b122-111">Le code pour configurer (ou *enregistrer*) des services est ajouté à la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7b122-111">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="7b122-112">Le pipeline de traitement des requêtes est composé d’une série de composants d’*intergiciel (middleware)* .</span><span class="sxs-lookup"><span data-stu-id="7b122-112">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="7b122-113">Par exemple, un intergiciel (middleware) peut gérer les demandes de fichiers statiques ou rediriger les requêtes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7b122-113">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="7b122-114">Chaque intergiciel (middleware) effectue des opérations asynchrones sur `HttpContext`, puis appelle l’intergiciel (middleware) suivant dans le pipeline ou met fin à la requête.</span><span class="sxs-lookup"><span data-stu-id="7b122-114">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="7b122-115">Le code pour configurer le pipeline de traitement des requêtes est ajouté à la méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="7b122-115">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="7b122-116">Voici un exemple de classe `Startup` :</span><span class="sxs-lookup"><span data-stu-id="7b122-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="7b122-117">Pour plus d’informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="7b122-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="7b122-118">Injection de dépendances (services)</span><span class="sxs-lookup"><span data-stu-id="7b122-118">Dependency injection (services)</span></span>

<span data-ttu-id="7b122-119">ASP.NET Core offre une infrastructure d’injection de dépendances (DI) intégrée qui rend disponibles les services configurés aux classes d’une application.</span><span class="sxs-lookup"><span data-stu-id="7b122-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="7b122-120">Pour obtenir une instance d’un service dans une classe, vous pouvez créer un constructeur avec un paramètre du type requis.</span><span class="sxs-lookup"><span data-stu-id="7b122-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="7b122-121">Le paramètre peut être le type de service ou une interface.</span><span class="sxs-lookup"><span data-stu-id="7b122-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="7b122-122">Le système d’injection de dépendances fournit le service lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="7b122-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="7b122-123">Voici une classe qui utilise l’injection de dépendances pour obtenir un objet de contexte Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7b122-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="7b122-124">La ligne en surbrillance est un exemple d’injection de constructeur :</span><span class="sxs-lookup"><span data-stu-id="7b122-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="7b122-125">Si l’injection de dépendances est intégrée, elle est conçue pour vous permettre d’incorporer un conteneur d’inversion de contrôle (IoC) tiers si vous préférez.</span><span class="sxs-lookup"><span data-stu-id="7b122-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="7b122-126">Pour plus d’informations, consultez <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="7b122-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="7b122-127">Middlewares</span><span class="sxs-lookup"><span data-stu-id="7b122-127">Middleware</span></span>

<span data-ttu-id="7b122-128">Le pipeline de traitement des requêtes est composé comme une série de composants d’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="7b122-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="7b122-129">Chaque composant effectue des opérations asynchrones sur `HttpContext`, puis appelle l’intergiciel (middleware) suivant dans le pipeline ou met fin à la requête.</span><span class="sxs-lookup"><span data-stu-id="7b122-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="7b122-130">Par convention, un composant d’intergiciel (middleware) est ajouté au pipeline en appelant sa méthode d’extension `Use...` dans la méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="7b122-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="7b122-131">Par exemple, pour activer le rendu des fichiers statiques, appelez `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="7b122-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="7b122-132">Le code en surbrillance dans l’exemple suivant configure le pipeline de traitement des requêtes :</span><span class="sxs-lookup"><span data-stu-id="7b122-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="7b122-133">ASP.NET Core inclut un ensemble complet de middlewares intégrés, et vous pouvez écrire un middleware personnalisé.</span><span class="sxs-lookup"><span data-stu-id="7b122-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="7b122-134">Pour plus d’informations, consultez <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="7b122-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="7b122-135">Host</span><span class="sxs-lookup"><span data-stu-id="7b122-135">Host</span></span>

<span data-ttu-id="7b122-136">Une application ASP.NET Core génère un *hôte* au démarrage.</span><span class="sxs-lookup"><span data-stu-id="7b122-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="7b122-137">L’hôte est un objet qui encapsule toutes les ressources de l’application, telles que :</span><span class="sxs-lookup"><span data-stu-id="7b122-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="7b122-138">Une implémentation serveur HTTP</span><span class="sxs-lookup"><span data-stu-id="7b122-138">An HTTP server implementation</span></span>
* <span data-ttu-id="7b122-139">Composants d’intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="7b122-139">Middleware components</span></span>
* <span data-ttu-id="7b122-140">Journalisation</span><span class="sxs-lookup"><span data-stu-id="7b122-140">Logging</span></span>
* <span data-ttu-id="7b122-141">DI</span><span class="sxs-lookup"><span data-stu-id="7b122-141">DI</span></span>
* <span data-ttu-id="7b122-142">Configuration</span><span class="sxs-lookup"><span data-stu-id="7b122-142">Configuration</span></span>

<span data-ttu-id="7b122-143">La principale raison d’inclure toutes les ressources interdépendantes de l’application dans un objet est la gestion de la durée de vie : contrôler le démarrage de l’application et l’arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="7b122-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="7b122-144">Deux hôtes sont disponibles : l’hôte générique et l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="7b122-144">Two hosts are available: the Generic Host and the Web Host.</span></span> <span data-ttu-id="7b122-145">L’hôte générique est recommandé. L’hôte web est disponible uniquement pour des raisons de compatibilité descendante.</span><span class="sxs-lookup"><span data-stu-id="7b122-145">The Generic Host is recommended, and the Web Host is available only for backwards compatibility.</span></span>

<span data-ttu-id="7b122-146">Le code permettant de créer un hôte se trouve dans `Program.Main` :</span><span class="sxs-lookup"><span data-stu-id="7b122-146">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs)]

<span data-ttu-id="7b122-147">Les méthodes `CreateDefaultBuilder` et `ConfigureWebHostDefaults` permettent de configurer un hôte avec les options fréquemment utilisées, notamment :</span><span class="sxs-lookup"><span data-stu-id="7b122-147">The `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods configure a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="7b122-148">Utilisez [Kestrel](#servers) en tant que serveur web et activez l’intégration IIS.</span><span class="sxs-lookup"><span data-stu-id="7b122-148">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="7b122-149">Chargez la configuration à partir de *appsettings.json*, *appsettings.{Environment Name}.json*, des variables d’environnement, des arguments de ligne de commande et d’autres sources de configuration.</span><span class="sxs-lookup"><span data-stu-id="7b122-149">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="7b122-150">Envoyez la sortie de journalisation aux fournisseurs Console et Debug.</span><span class="sxs-lookup"><span data-stu-id="7b122-150">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="7b122-151">Pour plus d’informations, consultez <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="7b122-151">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="non-web-scenarios"></a><span data-ttu-id="7b122-152">Scénarios non basés sur le web</span><span class="sxs-lookup"><span data-stu-id="7b122-152">Non-web scenarios</span></span>

<span data-ttu-id="7b122-153">L’hôte générique permet à d’autres types d’application d’utiliser des extensions de framework composites, par exemple la journalisation, l’injection de dépendance, la configuration et la gestion de la durée de vie de l’application.</span><span class="sxs-lookup"><span data-stu-id="7b122-153">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="7b122-154">Pour plus d’informations, consultez <xref:fundamentals/host/generic-host> et <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="7b122-154">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="7b122-155">Serveurs</span><span class="sxs-lookup"><span data-stu-id="7b122-155">Servers</span></span>

<span data-ttu-id="7b122-156">Une application ASP.NET Core utilise une implémentation de serveur HTTP pour écouter les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="7b122-156">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="7b122-157">Les surfaces de serveur envoient des requêtes à l’application comme un ensemble de [fonctionnalités de requête](xref:fundamentals/request-features) composées dans un `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="7b122-157">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

# <a name="windows"></a>[<span data-ttu-id="7b122-158">Windows</span><span class="sxs-lookup"><span data-stu-id="7b122-158">Windows</span></span>](#tab/windows)

<span data-ttu-id="7b122-159">ASP.NET Core fournit les implémentations de serveur suivantes :</span><span class="sxs-lookup"><span data-stu-id="7b122-159">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="7b122-160">*Kestrel* est un serveur web multiplateforme.</span><span class="sxs-lookup"><span data-stu-id="7b122-160">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="7b122-161">Kestrel est souvent exécuté dans une configuration de proxy inverse à l’aide d’[IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="7b122-161">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="7b122-162">Dans ASP.NET Core 2.0 ou versions ultérieures, Kestrel peut être exécuté en tant que serveur de périphérie public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="7b122-162">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="7b122-163">Le *serveur http IIS* est un serveur pour Windows qui utilise IIS.</span><span class="sxs-lookup"><span data-stu-id="7b122-163">*IIS HTTP Server* is a server for Windows that uses IIS.</span></span> <span data-ttu-id="7b122-164">Avec ce serveur, l’application ASP.NET Core et IIS s’exécutent dans le même processus.</span><span class="sxs-lookup"><span data-stu-id="7b122-164">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="7b122-165">*HTTP.sys* est un serveur Windows qui n’est pas utilisé avec IIS.</span><span class="sxs-lookup"><span data-stu-id="7b122-165">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="7b122-166">macOS</span><span class="sxs-lookup"><span data-stu-id="7b122-166">macOS</span></span>](#tab/macos)

<span data-ttu-id="7b122-167">ASP.NET Core fournit l’implémentation du serveur multiplateforme *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="7b122-167">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7b122-168">Dans ASP.NET Core 2.0 ou versions ultérieures, Kestrel peut être exécuté en tant que serveur de périphérie public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="7b122-168">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7b122-169">Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="7b122-169">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="7b122-170">Linux</span><span class="sxs-lookup"><span data-stu-id="7b122-170">Linux</span></span>](#tab/linux)

<span data-ttu-id="7b122-171">ASP.NET Core fournit l’implémentation du serveur multiplateforme *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="7b122-171">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7b122-172">Dans ASP.NET Core 2.0 ou versions ultérieures, Kestrel peut être exécuté en tant que serveur de périphérie public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="7b122-172">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7b122-173">Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="7b122-173">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

<span data-ttu-id="7b122-174">Pour plus d’informations, consultez <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="7b122-174">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="7b122-175">Configuration</span><span class="sxs-lookup"><span data-stu-id="7b122-175">Configuration</span></span>

<span data-ttu-id="7b122-176">ASP.NET Core fournit une infrastructure de configuration qui obtient des paramètres en tant que paires nom-valeur à partir d’un ensemble ordonné de fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="7b122-176">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="7b122-177">Il existe des fournisseurs de configuration intégrés pour une grande variété de sources, tels que des fichiers *.json*, *.xml*, des variables d’environnement et des arguments de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="7b122-177">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="7b122-178">Vous pouvez également écrire des fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="7b122-178">You can also write custom configuration providers.</span></span>

<span data-ttu-id="7b122-179">Par exemple, vous pouvez spécifier que la configuration provient de *appsettings.json* et de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="7b122-179">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="7b122-180">Lorsque la valeur de *ConnectionString* est demandée, l’infrastructure recherche d’abord dans le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="7b122-180">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="7b122-181">Si la valeur est trouvée dans ce fichier, mais également dans une variable d’environnement, la valeur de la variable d’environnement est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="7b122-181">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="7b122-182">Pour gérer des données de configuration confidentielles telles que les mots de passe, ASP.NET Core fournit un [outil Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="7b122-182">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="7b122-183">Pour les secrets de production, nous vous recommandons [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="7b122-183">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="7b122-184">Pour plus d’informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="7b122-184">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="7b122-185">Options</span><span class="sxs-lookup"><span data-stu-id="7b122-185">Options</span></span>

<span data-ttu-id="7b122-186">Lorsque cela est possible, ASP.NET Core suit le *modèle d’options* pour stocker et récupérer des valeurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="7b122-186">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="7b122-187">Le modèle d’options utilise des classes pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="7b122-187">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="7b122-188">Par exemple, le code suivant définit des options WebSockets :</span><span class="sxs-lookup"><span data-stu-id="7b122-188">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="7b122-189">Pour plus d’informations, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="7b122-189">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="7b122-190">Environnements</span><span class="sxs-lookup"><span data-stu-id="7b122-190">Environments</span></span>

<span data-ttu-id="7b122-191">Les environnements d’exécution, tels que *Développement*, *Mise en lots* et *Production*, sont une notion de premier plan dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7b122-191">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="7b122-192">Vous pouvez spécifier l’environnement d’exécution d’une application en définissant la variable d’environnement `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="7b122-192">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="7b122-193">ASP.NET Core lit la variable d’environnement au démarrage de l’application et stocke la valeur dans une implémentation `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="7b122-193">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="7b122-194">L’objet d’environnement est disponible partout dans l’application par le biais de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="7b122-194">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="7b122-195">L’exemple de code suivant de la classe `Startup` configure l’application pour fournir des informations d’erreur détaillées uniquement lorsqu’elle s’exécute en développement :</span><span class="sxs-lookup"><span data-stu-id="7b122-195">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="7b122-196">Pour plus d’informations, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="7b122-196">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="7b122-197">Journalisation</span><span class="sxs-lookup"><span data-stu-id="7b122-197">Logging</span></span>

<span data-ttu-id="7b122-198">ASP.NET Core prend en charge une API de journalisation qui fonctionne avec un large éventail de fournisseurs de journalisation intégrés et tiers.</span><span class="sxs-lookup"><span data-stu-id="7b122-198">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="7b122-199">Les fournisseurs disponibles sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="7b122-199">Available providers include the following:</span></span>

* <span data-ttu-id="7b122-200">Console</span><span class="sxs-lookup"><span data-stu-id="7b122-200">Console</span></span>
* <span data-ttu-id="7b122-201">Débogage</span><span class="sxs-lookup"><span data-stu-id="7b122-201">Debug</span></span>
* <span data-ttu-id="7b122-202">Suivi des événements sur Windows</span><span class="sxs-lookup"><span data-stu-id="7b122-202">Event Tracing on Windows</span></span>
* <span data-ttu-id="7b122-203">Journal des événements Windows</span><span class="sxs-lookup"><span data-stu-id="7b122-203">Windows Event Log</span></span>
* <span data-ttu-id="7b122-204">TraceSource</span><span class="sxs-lookup"><span data-stu-id="7b122-204">TraceSource</span></span>
* <span data-ttu-id="7b122-205">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="7b122-205">Azure App Service</span></span>
* <span data-ttu-id="7b122-206">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="7b122-206">Azure Application Insights</span></span>

<span data-ttu-id="7b122-207">Écrivez des journaux à partir de n’importe quel emplacement dans le code d’une application en obtenant un objet `ILogger` à partir de l’injection de dépendances et en appelant les méthodes de journal.</span><span class="sxs-lookup"><span data-stu-id="7b122-207">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="7b122-208">Voici un exemple de code qui utilise un objet `ILogger`, avec l’injection de constructeur et les appels de méthode de journalisation mis en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="7b122-208">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="7b122-209">L’interface `ILogger` vous permet de passer un certain nombre de champs au fournisseur de journalisation.</span><span class="sxs-lookup"><span data-stu-id="7b122-209">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="7b122-210">Les champs sont couramment utilisés pour construire une chaîne de message, mais le fournisseur peut également les envoyer en tant que champs distincts dans un magasin de données.</span><span class="sxs-lookup"><span data-stu-id="7b122-210">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="7b122-211">Cette fonctionnalité permet aux fournisseurs de journalisation d’implémenter la [journalisation sémantique, également appelée journalisation structurée](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="7b122-211">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="7b122-212">Pour plus d’informations, consultez <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="7b122-212">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="7b122-213">Routage</span><span class="sxs-lookup"><span data-stu-id="7b122-213">Routing</span></span>

<span data-ttu-id="7b122-214">Un *itinéraire* est un modèle d’URL qui est mappé à un gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="7b122-214">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="7b122-215">Le gestionnaire est généralement une page Razor, une méthode d’action dans un contrôleur MVC, ou un intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="7b122-215">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="7b122-216">Le routage ASP.NET Core vous permet de contrôler les URL utilisées par votre application.</span><span class="sxs-lookup"><span data-stu-id="7b122-216">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="7b122-217">Pour plus d’informations, consultez <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="7b122-217">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="7b122-218">Gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="7b122-218">Error handling</span></span>

<span data-ttu-id="7b122-219">ASP.NET Core offre des fonctionnalités intégrées pour gérer des erreurs, telles que :</span><span class="sxs-lookup"><span data-stu-id="7b122-219">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="7b122-220">Une page d’exceptions du développeur</span><span class="sxs-lookup"><span data-stu-id="7b122-220">A developer exception page</span></span>
* <span data-ttu-id="7b122-221">Pages d’erreur personnalisées</span><span class="sxs-lookup"><span data-stu-id="7b122-221">Custom error pages</span></span>
* <span data-ttu-id="7b122-222">Pages de codes d’état statique</span><span class="sxs-lookup"><span data-stu-id="7b122-222">Static status code pages</span></span>
* <span data-ttu-id="7b122-223">Gestion des exceptions de démarrage</span><span class="sxs-lookup"><span data-stu-id="7b122-223">Startup exception handling</span></span>

<span data-ttu-id="7b122-224">Pour plus d’informations, consultez <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="7b122-224">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="7b122-225">Effectuer des requêtes HTTP</span><span class="sxs-lookup"><span data-stu-id="7b122-225">Make HTTP requests</span></span>

<span data-ttu-id="7b122-226">Une implémentation de `IHttpClientFactory` est disponible pour la création d’instances `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7b122-226">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="7b122-227">La fabrique :</span><span class="sxs-lookup"><span data-stu-id="7b122-227">The factory:</span></span>

* <span data-ttu-id="7b122-228">Fournit un emplacement central pour le nommage et la configuration d’instance de `HttpClient` logiques.</span><span class="sxs-lookup"><span data-stu-id="7b122-228">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="7b122-229">Par exemple, un client *github* peut être inscrit et configuré pour accéder à GitHub.</span><span class="sxs-lookup"><span data-stu-id="7b122-229">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="7b122-230">Un client par défaut peut être inscrit à d’autres fins.</span><span class="sxs-lookup"><span data-stu-id="7b122-230">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="7b122-231">Prend en charge l’inscription et le chaînage de plusieurs gestionnaires de délégation pour créer un pipeline de middlewares pour les requêtes sortantes.</span><span class="sxs-lookup"><span data-stu-id="7b122-231">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="7b122-232">Ce modèle est similaire au pipeline de middlewares entrants dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7b122-232">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="7b122-233">Le modèle fournit un mécanisme permettant de gérer les problèmes transversaux liés aux des requêtes HTTP, notamment la mise en cache, la gestion des erreurs, la sérialisation et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="7b122-233">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="7b122-234">S’intègre à *Polly*, une bibliothèque tierce populaire pour la gestion des erreurs temporaires.</span><span class="sxs-lookup"><span data-stu-id="7b122-234">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="7b122-235">Gère le regroupement et la durée de vie des instances de `HttpClientMessageHandler` sous-jacentes pour éviter les problèmes DNS courants qui se produisent lors de la gestion manuelle des durées de vie de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7b122-235">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="7b122-236">Ajoute une expérience de journalisation configurable (via `ILogger`) pour toutes les requêtes envoyées via des clients créés par la fabrique.</span><span class="sxs-lookup"><span data-stu-id="7b122-236">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="7b122-237">Pour plus d’informations, consultez <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="7b122-237">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="7b122-238">Racine de contenu</span><span class="sxs-lookup"><span data-stu-id="7b122-238">Content root</span></span>

<span data-ttu-id="7b122-239">La racine du contenu est le chemin d’accès de base à :</span><span class="sxs-lookup"><span data-stu-id="7b122-239">The content root is the base path to the:</span></span>

* <span data-ttu-id="7b122-240">Exécutable hébergeant l’application ( *. exe*).</span><span class="sxs-lookup"><span data-stu-id="7b122-240">Executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="7b122-241">Assemblys compilés qui composent l’application ( *. dll*).</span><span class="sxs-lookup"><span data-stu-id="7b122-241">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="7b122-242">Fichiers de contenu sans code utilisés par l’application, tels que :</span><span class="sxs-lookup"><span data-stu-id="7b122-242">Non-code content files used by the app, such as:</span></span>
  * <span data-ttu-id="7b122-243">Fichiers Razor ( *. cshtml*, *. Razor*)</span><span class="sxs-lookup"><span data-stu-id="7b122-243">Razor files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="7b122-244">Fichiers de configuration ( *. JSON*, *. xml*)</span><span class="sxs-lookup"><span data-stu-id="7b122-244">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="7b122-245">Fichiers de données ( *. db*)</span><span class="sxs-lookup"><span data-stu-id="7b122-245">Data files (*.db*)</span></span>
* <span data-ttu-id="7b122-246">[Racine Web](#web-root), en général le dossier *wwwroot* publié.</span><span class="sxs-lookup"><span data-stu-id="7b122-246">[Web root](#web-root), typically the published *wwwroot* folder.</span></span>

<span data-ttu-id="7b122-247">Lors du développement :</span><span class="sxs-lookup"><span data-stu-id="7b122-247">During development:</span></span>

* <span data-ttu-id="7b122-248">La racine du contenu est par défaut le répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="7b122-248">The content root defaults to the project's root directory.</span></span>
* <span data-ttu-id="7b122-249">Le répertoire racine du projet est utilisé pour créer le :</span><span class="sxs-lookup"><span data-stu-id="7b122-249">The project's root directory is used to create the:</span></span>
  * <span data-ttu-id="7b122-250">Chemin d’accès aux fichiers de contenu sans code de l’application dans le répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="7b122-250">Path to the app's non-code content files in the project's root directory.</span></span>
  * <span data-ttu-id="7b122-251">[Racine Web](#web-root), généralement le dossier *wwwroot* dans le répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="7b122-251">[Web root](#web-root), typically the *wwwroot* folder in the project's root directory.</span></span>

<span data-ttu-id="7b122-252">Vous pouvez spécifier un autre chemin d’accès racine [de contenu lors de la génération de l’hôte](#host).</span><span class="sxs-lookup"><span data-stu-id="7b122-252">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="7b122-253">Pour plus d’informations, consultez <xref:fundamentals/host/generic-host#contentrootpath>.</span><span class="sxs-lookup"><span data-stu-id="7b122-253">For more information, see <xref:fundamentals/host/generic-host#contentrootpath>.</span></span>

## <a name="web-root"></a><span data-ttu-id="7b122-254">Racine web</span><span class="sxs-lookup"><span data-stu-id="7b122-254">Web root</span></span>

<span data-ttu-id="7b122-255">La racine Web est le chemin de base des fichiers de ressources statiques, non-code et publics, tels que :</span><span class="sxs-lookup"><span data-stu-id="7b122-255">The web root is the base path to public, non-code, static resource files, such as:</span></span>

* <span data-ttu-id="7b122-256">Feuilles de style ( *. CSS*)</span><span class="sxs-lookup"><span data-stu-id="7b122-256">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="7b122-257">JavaScript ( *. js*)</span><span class="sxs-lookup"><span data-stu-id="7b122-257">JavaScript (*.js*)</span></span>
* <span data-ttu-id="7b122-258">Images ( *. png*, *. jpg*)</span><span class="sxs-lookup"><span data-stu-id="7b122-258">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="7b122-259">Les fichiers statiques sont pris en charge par défaut uniquement à partir du répertoire racine Web (et des sous-répertoires).</span><span class="sxs-lookup"><span data-stu-id="7b122-259">Static files are only served by default from the web root directory (and sub-directories).</span></span>

<span data-ttu-id="7b122-260">Le chemin d’accès racine Web a comme valeur par défaut *{content root}/wwwroot*, mais une autre racine Web peut être spécifiée lors de [la génération de l’hôte](#host).</span><span class="sxs-lookup"><span data-stu-id="7b122-260">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="7b122-261">Pour plus d’informations, consultez <xref:fundamentals/host/generic-host#webroot>.</span><span class="sxs-lookup"><span data-stu-id="7b122-261">For more information, see <xref:fundamentals/host/generic-host#webroot>.</span></span>

<span data-ttu-id="7b122-262">Empêchez la publication de fichiers dans *wwwroot* avec le [contenu\<> élément de projet](/visualstudio/msbuild/common-msbuild-project-items#content) dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="7b122-262">Prevent publishing files in *wwwroot* with the [\<Content> project item](/visualstudio/msbuild/common-msbuild-project-items#content) in the project file.</span></span> <span data-ttu-id="7b122-263">L’exemple suivant empêche la publication de contenu dans le répertoire et les sous-répertoires *wwwroot/local* :</span><span class="sxs-lookup"><span data-stu-id="7b122-263">The following example prevents publishing content in the *wwwroot/local* directory and sub-directories:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="7b122-264">Pour empêcher la publication de ressources d’identité statiques sur la racine Web, consultez <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span><span class="sxs-lookup"><span data-stu-id="7b122-264">To prevent publishing static Identity assets to the web root, see <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span></span>

<span data-ttu-id="7b122-265">Dans les fichiers Razor ( *. cshtml*), le tilde-slash (`~/`) pointe vers la racine Web.</span><span class="sxs-lookup"><span data-stu-id="7b122-265">In Razor (*.cshtml*) files, the tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="7b122-266">Un chemin d’accès commençant par `~/` est désigné sous le terme de « *chemin d’accès virtuel*».</span><span class="sxs-lookup"><span data-stu-id="7b122-266">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="7b122-267">Pour plus d’informations, consultez <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="7b122-267">For more information, see <xref:fundamentals/static-files>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7b122-268">Cet article est une vue d’ensemble des sujets clés pour comprendre comment développer des applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7b122-268">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="7b122-269">Classe Startup</span><span class="sxs-lookup"><span data-stu-id="7b122-269">The Startup class</span></span>

<span data-ttu-id="7b122-270">La classe `Startup` est l’endroit où :</span><span class="sxs-lookup"><span data-stu-id="7b122-270">The `Startup` class is where:</span></span>

* <span data-ttu-id="7b122-271">Les services nécessaires à l’application sont configurés.</span><span class="sxs-lookup"><span data-stu-id="7b122-271">Services required by the app are configured.</span></span>
* <span data-ttu-id="7b122-272">Le pipeline de traitement des requêtes est défini.</span><span class="sxs-lookup"><span data-stu-id="7b122-272">The request handling pipeline is defined.</span></span>

<span data-ttu-id="7b122-273">Les *services* sont des composants utilisés par l’application.</span><span class="sxs-lookup"><span data-stu-id="7b122-273">*Services* are components that are used by the app.</span></span> <span data-ttu-id="7b122-274">Par exemple, un composant de journalisation est un service.</span><span class="sxs-lookup"><span data-stu-id="7b122-274">For example, a logging component is a service.</span></span> <span data-ttu-id="7b122-275">Le code pour configurer (ou *enregistrer*) des services est ajouté à la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7b122-275">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="7b122-276">Le pipeline de traitement des requêtes est composé d’une série de composants d’*intergiciel (middleware)* .</span><span class="sxs-lookup"><span data-stu-id="7b122-276">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="7b122-277">Par exemple, un intergiciel (middleware) peut gérer les demandes de fichiers statiques ou rediriger les requêtes HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7b122-277">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="7b122-278">Chaque intergiciel (middleware) effectue des opérations asynchrones sur `HttpContext`, puis appelle l’intergiciel (middleware) suivant dans le pipeline ou met fin à la requête.</span><span class="sxs-lookup"><span data-stu-id="7b122-278">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="7b122-279">Le code pour configurer le pipeline de traitement des requêtes est ajouté à la méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="7b122-279">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="7b122-280">Voici un exemple de classe `Startup` :</span><span class="sxs-lookup"><span data-stu-id="7b122-280">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="7b122-281">Pour plus d’informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="7b122-281">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="7b122-282">Injection de dépendances (services)</span><span class="sxs-lookup"><span data-stu-id="7b122-282">Dependency injection (services)</span></span>

<span data-ttu-id="7b122-283">ASP.NET Core offre une infrastructure d’injection de dépendances (DI) intégrée qui rend disponibles les services configurés aux classes d’une application.</span><span class="sxs-lookup"><span data-stu-id="7b122-283">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="7b122-284">Pour obtenir une instance d’un service dans une classe, vous pouvez créer un constructeur avec un paramètre du type requis.</span><span class="sxs-lookup"><span data-stu-id="7b122-284">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="7b122-285">Le paramètre peut être le type de service ou une interface.</span><span class="sxs-lookup"><span data-stu-id="7b122-285">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="7b122-286">Le système d’injection de dépendances fournit le service lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="7b122-286">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="7b122-287">Voici une classe qui utilise l’injection de dépendances pour obtenir un objet de contexte Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7b122-287">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="7b122-288">La ligne en surbrillance est un exemple d’injection de constructeur :</span><span class="sxs-lookup"><span data-stu-id="7b122-288">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="7b122-289">Si l’injection de dépendances est intégrée, elle est conçue pour vous permettre d’incorporer un conteneur d’inversion de contrôle (IoC) tiers si vous préférez.</span><span class="sxs-lookup"><span data-stu-id="7b122-289">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="7b122-290">Pour plus d’informations, consultez <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="7b122-290">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="7b122-291">Middlewares</span><span class="sxs-lookup"><span data-stu-id="7b122-291">Middleware</span></span>

<span data-ttu-id="7b122-292">Le pipeline de traitement des requêtes est composé comme une série de composants d’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="7b122-292">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="7b122-293">Chaque composant effectue des opérations asynchrones sur `HttpContext`, puis appelle l’intergiciel (middleware) suivant dans le pipeline ou met fin à la requête.</span><span class="sxs-lookup"><span data-stu-id="7b122-293">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="7b122-294">Par convention, un composant d’intergiciel (middleware) est ajouté au pipeline en appelant sa méthode d’extension `Use...` dans la méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="7b122-294">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="7b122-295">Par exemple, pour activer le rendu des fichiers statiques, appelez `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="7b122-295">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="7b122-296">Le code en surbrillance dans l’exemple suivant configure le pipeline de traitement des requêtes :</span><span class="sxs-lookup"><span data-stu-id="7b122-296">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="7b122-297">ASP.NET Core inclut un ensemble complet de middlewares intégrés, et vous pouvez écrire un middleware personnalisé.</span><span class="sxs-lookup"><span data-stu-id="7b122-297">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="7b122-298">Pour plus d’informations, consultez <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="7b122-298">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="7b122-299">Host</span><span class="sxs-lookup"><span data-stu-id="7b122-299">Host</span></span>

<span data-ttu-id="7b122-300">Une application ASP.NET Core génère un *hôte* au démarrage.</span><span class="sxs-lookup"><span data-stu-id="7b122-300">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="7b122-301">L’hôte est un objet qui encapsule toutes les ressources de l’application, telles que :</span><span class="sxs-lookup"><span data-stu-id="7b122-301">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="7b122-302">Une implémentation serveur HTTP</span><span class="sxs-lookup"><span data-stu-id="7b122-302">An HTTP server implementation</span></span>
* <span data-ttu-id="7b122-303">Composants d’intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="7b122-303">Middleware components</span></span>
* <span data-ttu-id="7b122-304">Journalisation</span><span class="sxs-lookup"><span data-stu-id="7b122-304">Logging</span></span>
* <span data-ttu-id="7b122-305">DI</span><span class="sxs-lookup"><span data-stu-id="7b122-305">DI</span></span>
* <span data-ttu-id="7b122-306">Configuration</span><span class="sxs-lookup"><span data-stu-id="7b122-306">Configuration</span></span>

<span data-ttu-id="7b122-307">La principale raison d’inclure toutes les ressources interdépendantes de l’application dans un objet est la gestion de la durée de vie : contrôler le démarrage de l’application et l’arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="7b122-307">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="7b122-308">Deux hôtes sont disponibles : l’hôte web et l’hôte générique.</span><span class="sxs-lookup"><span data-stu-id="7b122-308">Two hosts are available: the Web Host and the Generic Host.</span></span> <span data-ttu-id="7b122-309">En ASP.NET Core 2.x, l’hôte générique est destiné uniquement aux scénarios non basés sur le web.</span><span class="sxs-lookup"><span data-stu-id="7b122-309">In ASP.NET Core 2.x, the Generic Host is only for non-web scenarios.</span></span>

<span data-ttu-id="7b122-310">Le code permettant de créer un hôte se trouve dans `Program.Main` :</span><span class="sxs-lookup"><span data-stu-id="7b122-310">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs)]

<span data-ttu-id="7b122-311">La méthode `CreateDefaultBuilder` permet de configurer un hôte avec les options fréquemment utilisées, notamment :</span><span class="sxs-lookup"><span data-stu-id="7b122-311">The `CreateDefaultBuilder` method configures a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="7b122-312">Utilisez [Kestrel](#servers) en tant que serveur web et activez l’intégration IIS.</span><span class="sxs-lookup"><span data-stu-id="7b122-312">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="7b122-313">Chargez la configuration à partir de *appsettings.json*, *appsettings.{Environment Name}.json*, des variables d’environnement, des arguments de ligne de commande et d’autres sources de configuration.</span><span class="sxs-lookup"><span data-stu-id="7b122-313">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="7b122-314">Envoyez la sortie de journalisation aux fournisseurs Console et Debug.</span><span class="sxs-lookup"><span data-stu-id="7b122-314">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="7b122-315">Pour plus d’informations, consultez <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="7b122-315">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="non-web-scenarios"></a><span data-ttu-id="7b122-316">Scénarios non basés sur le web</span><span class="sxs-lookup"><span data-stu-id="7b122-316">Non-web scenarios</span></span>

<span data-ttu-id="7b122-317">L’hôte générique permet à d’autres types d’application d’utiliser des extensions de framework composites, par exemple la journalisation, l’injection de dépendance, la configuration et la gestion de la durée de vie de l’application.</span><span class="sxs-lookup"><span data-stu-id="7b122-317">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="7b122-318">Pour plus d’informations, consultez <xref:fundamentals/host/generic-host> et <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="7b122-318">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="7b122-319">Serveurs</span><span class="sxs-lookup"><span data-stu-id="7b122-319">Servers</span></span>

<span data-ttu-id="7b122-320">Une application ASP.NET Core utilise une implémentation de serveur HTTP pour écouter les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="7b122-320">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="7b122-321">Les surfaces de serveur envoient des requêtes à l’application comme un ensemble de [fonctionnalités de requête](xref:fundamentals/request-features) composées dans un `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="7b122-321">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

# <a name="windows"></a>[<span data-ttu-id="7b122-322">Windows</span><span class="sxs-lookup"><span data-stu-id="7b122-322">Windows</span></span>](#tab/windows)

<span data-ttu-id="7b122-323">ASP.NET Core fournit les implémentations de serveur suivantes :</span><span class="sxs-lookup"><span data-stu-id="7b122-323">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="7b122-324">*Kestrel* est un serveur web multiplateforme.</span><span class="sxs-lookup"><span data-stu-id="7b122-324">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="7b122-325">Kestrel est souvent exécuté dans une configuration de proxy inverse à l’aide d’[IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="7b122-325">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="7b122-326">Kestrel peut être exécuté en tant que serveur Edge public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="7b122-326">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="7b122-327">*Le serveur HTTP IIS* est un serveur pour Windows qui utilise IIS.</span><span class="sxs-lookup"><span data-stu-id="7b122-327">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="7b122-328">Avec ce serveur, l’application ASP.NET Core et IIS s’exécutent dans le même processus.</span><span class="sxs-lookup"><span data-stu-id="7b122-328">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="7b122-329">*HTTP.sys* est un serveur Windows qui n’est pas utilisé avec IIS.</span><span class="sxs-lookup"><span data-stu-id="7b122-329">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="7b122-330">macOS</span><span class="sxs-lookup"><span data-stu-id="7b122-330">macOS</span></span>](#tab/macos)

<span data-ttu-id="7b122-331">ASP.NET Core fournit l’implémentation du serveur multiplateforme *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="7b122-331">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7b122-332">Kestrel peut être exécuté en tant que serveur Edge public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="7b122-332">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7b122-333">Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="7b122-333">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="7b122-334">Linux</span><span class="sxs-lookup"><span data-stu-id="7b122-334">Linux</span></span>](#tab/linux)

<span data-ttu-id="7b122-335">ASP.NET Core fournit l’implémentation du serveur multiplateforme *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="7b122-335">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7b122-336">Kestrel peut être exécuté en tant que serveur Edge public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="7b122-336">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7b122-337">Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="7b122-337">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windows"></a>[<span data-ttu-id="7b122-338">Windows</span><span class="sxs-lookup"><span data-stu-id="7b122-338">Windows</span></span>](#tab/windows)

<span data-ttu-id="7b122-339">ASP.NET Core fournit les implémentations de serveur suivantes :</span><span class="sxs-lookup"><span data-stu-id="7b122-339">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="7b122-340">*Kestrel* est un serveur web multiplateforme.</span><span class="sxs-lookup"><span data-stu-id="7b122-340">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="7b122-341">Kestrel est souvent exécuté dans une configuration de proxy inverse à l’aide d’[IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="7b122-341">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="7b122-342">Kestrel peut être exécuté en tant que serveur Edge public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="7b122-342">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="7b122-343">*HTTP.sys* est un serveur Windows qui n’est pas utilisé avec IIS.</span><span class="sxs-lookup"><span data-stu-id="7b122-343">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="7b122-344">macOS</span><span class="sxs-lookup"><span data-stu-id="7b122-344">macOS</span></span>](#tab/macos)

<span data-ttu-id="7b122-345">ASP.NET Core fournit l’implémentation du serveur multiplateforme *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="7b122-345">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7b122-346">Kestrel peut être exécuté en tant que serveur Edge public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="7b122-346">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7b122-347">Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="7b122-347">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="7b122-348">Linux</span><span class="sxs-lookup"><span data-stu-id="7b122-348">Linux</span></span>](#tab/linux)

<span data-ttu-id="7b122-349">ASP.NET Core fournit l’implémentation du serveur multiplateforme *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="7b122-349">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7b122-350">Kestrel peut être exécuté en tant que serveur Edge public exposé directement à Internet.</span><span class="sxs-lookup"><span data-stu-id="7b122-350">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7b122-351">Kestrel est souvent exécuté dans une configuration de proxy inverse avec [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="7b122-351">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7b122-352">Pour plus d’informations, consultez <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="7b122-352">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="7b122-353">Configuration</span><span class="sxs-lookup"><span data-stu-id="7b122-353">Configuration</span></span>

<span data-ttu-id="7b122-354">ASP.NET Core fournit une infrastructure de configuration qui obtient des paramètres en tant que paires nom-valeur à partir d’un ensemble ordonné de fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="7b122-354">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="7b122-355">Il existe des fournisseurs de configuration intégrés pour une grande variété de sources, tels que des fichiers *.json*, *.xml*, des variables d’environnement et des arguments de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="7b122-355">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="7b122-356">Vous pouvez également écrire des fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="7b122-356">You can also write custom configuration providers.</span></span>

<span data-ttu-id="7b122-357">Par exemple, vous pouvez spécifier que la configuration provient de *appsettings.json* et de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="7b122-357">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="7b122-358">Lorsque la valeur de *ConnectionString* est demandée, l’infrastructure recherche d’abord dans le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="7b122-358">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="7b122-359">Si la valeur est trouvée dans ce fichier, mais également dans une variable d’environnement, la valeur de la variable d’environnement est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="7b122-359">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="7b122-360">Pour gérer des données de configuration confidentielles telles que les mots de passe, ASP.NET Core fournit un [outil Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="7b122-360">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="7b122-361">Pour les secrets de production, nous vous recommandons [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="7b122-361">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="7b122-362">Pour plus d’informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="7b122-362">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="7b122-363">Options</span><span class="sxs-lookup"><span data-stu-id="7b122-363">Options</span></span>

<span data-ttu-id="7b122-364">Lorsque cela est possible, ASP.NET Core suit le *modèle d’options* pour stocker et récupérer des valeurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="7b122-364">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="7b122-365">Le modèle d’options utilise des classes pour représenter les groupes de paramètres associés.</span><span class="sxs-lookup"><span data-stu-id="7b122-365">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="7b122-366">Par exemple, le code suivant définit des options WebSockets :</span><span class="sxs-lookup"><span data-stu-id="7b122-366">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="7b122-367">Pour plus d’informations, consultez <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="7b122-367">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="7b122-368">Environnements</span><span class="sxs-lookup"><span data-stu-id="7b122-368">Environments</span></span>

<span data-ttu-id="7b122-369">Les environnements d’exécution, tels que *Développement*, *Mise en lots* et *Production*, sont une notion de premier plan dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7b122-369">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="7b122-370">Vous pouvez spécifier l’environnement d’exécution d’une application en définissant la variable d’environnement `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="7b122-370">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="7b122-371">ASP.NET Core lit la variable d’environnement au démarrage de l’application et stocke la valeur dans une implémentation `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="7b122-371">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="7b122-372">L’objet d’environnement est disponible partout dans l’application par le biais de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="7b122-372">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="7b122-373">L’exemple de code suivant de la classe `Startup` configure l’application pour fournir des informations d’erreur détaillées uniquement lorsqu’elle s’exécute en développement :</span><span class="sxs-lookup"><span data-stu-id="7b122-373">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="7b122-374">Pour plus d’informations, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="7b122-374">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="7b122-375">Journalisation</span><span class="sxs-lookup"><span data-stu-id="7b122-375">Logging</span></span>

<span data-ttu-id="7b122-376">ASP.NET Core prend en charge une API de journalisation qui fonctionne avec un large éventail de fournisseurs de journalisation intégrés et tiers.</span><span class="sxs-lookup"><span data-stu-id="7b122-376">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="7b122-377">Les fournisseurs disponibles sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="7b122-377">Available providers include the following:</span></span>

* <span data-ttu-id="7b122-378">Console</span><span class="sxs-lookup"><span data-stu-id="7b122-378">Console</span></span>
* <span data-ttu-id="7b122-379">Débogage</span><span class="sxs-lookup"><span data-stu-id="7b122-379">Debug</span></span>
* <span data-ttu-id="7b122-380">Suivi des événements sur Windows</span><span class="sxs-lookup"><span data-stu-id="7b122-380">Event Tracing on Windows</span></span>
* <span data-ttu-id="7b122-381">Journal des événements Windows</span><span class="sxs-lookup"><span data-stu-id="7b122-381">Windows Event Log</span></span>
* <span data-ttu-id="7b122-382">TraceSource</span><span class="sxs-lookup"><span data-stu-id="7b122-382">TraceSource</span></span>
* <span data-ttu-id="7b122-383">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="7b122-383">Azure App Service</span></span>
* <span data-ttu-id="7b122-384">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="7b122-384">Azure Application Insights</span></span>

<span data-ttu-id="7b122-385">Écrivez des journaux à partir de n’importe quel emplacement dans le code d’une application en obtenant un objet `ILogger` à partir de l’injection de dépendances et en appelant les méthodes de journal.</span><span class="sxs-lookup"><span data-stu-id="7b122-385">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="7b122-386">Voici un exemple de code qui utilise un objet `ILogger`, avec l’injection de constructeur et les appels de méthode de journalisation mis en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="7b122-386">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="7b122-387">L’interface `ILogger` vous permet de passer un certain nombre de champs au fournisseur de journalisation.</span><span class="sxs-lookup"><span data-stu-id="7b122-387">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="7b122-388">Les champs sont couramment utilisés pour construire une chaîne de message, mais le fournisseur peut également les envoyer en tant que champs distincts dans un magasin de données.</span><span class="sxs-lookup"><span data-stu-id="7b122-388">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="7b122-389">Cette fonctionnalité permet aux fournisseurs de journalisation d’implémenter la [journalisation sémantique, également appelée journalisation structurée](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="7b122-389">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="7b122-390">Pour plus d’informations, consultez <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="7b122-390">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="7b122-391">Routage</span><span class="sxs-lookup"><span data-stu-id="7b122-391">Routing</span></span>

<span data-ttu-id="7b122-392">Un *itinéraire* est un modèle d’URL qui est mappé à un gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="7b122-392">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="7b122-393">Le gestionnaire est généralement une page Razor, une méthode d’action dans un contrôleur MVC, ou un intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="7b122-393">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="7b122-394">Le routage ASP.NET Core vous permet de contrôler les URL utilisées par votre application.</span><span class="sxs-lookup"><span data-stu-id="7b122-394">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="7b122-395">Pour plus d’informations, consultez <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="7b122-395">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="7b122-396">Gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="7b122-396">Error handling</span></span>

<span data-ttu-id="7b122-397">ASP.NET Core offre des fonctionnalités intégrées pour gérer des erreurs, telles que :</span><span class="sxs-lookup"><span data-stu-id="7b122-397">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="7b122-398">Une page d’exceptions du développeur</span><span class="sxs-lookup"><span data-stu-id="7b122-398">A developer exception page</span></span>
* <span data-ttu-id="7b122-399">Pages d’erreur personnalisées</span><span class="sxs-lookup"><span data-stu-id="7b122-399">Custom error pages</span></span>
* <span data-ttu-id="7b122-400">Pages de codes d’état statique</span><span class="sxs-lookup"><span data-stu-id="7b122-400">Static status code pages</span></span>
* <span data-ttu-id="7b122-401">Gestion des exceptions de démarrage</span><span class="sxs-lookup"><span data-stu-id="7b122-401">Startup exception handling</span></span>

<span data-ttu-id="7b122-402">Pour plus d’informations, consultez <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="7b122-402">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="7b122-403">Effectuer des requêtes HTTP</span><span class="sxs-lookup"><span data-stu-id="7b122-403">Make HTTP requests</span></span>

<span data-ttu-id="7b122-404">Une implémentation de `IHttpClientFactory` est disponible pour la création d’instances `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7b122-404">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="7b122-405">La fabrique :</span><span class="sxs-lookup"><span data-stu-id="7b122-405">The factory:</span></span>

* <span data-ttu-id="7b122-406">Fournit un emplacement central pour le nommage et la configuration d’instance de `HttpClient` logiques.</span><span class="sxs-lookup"><span data-stu-id="7b122-406">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="7b122-407">Par exemple, un client *github* peut être inscrit et configuré pour accéder à GitHub.</span><span class="sxs-lookup"><span data-stu-id="7b122-407">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="7b122-408">Un client par défaut peut être inscrit à d’autres fins.</span><span class="sxs-lookup"><span data-stu-id="7b122-408">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="7b122-409">Prend en charge l’inscription et le chaînage de plusieurs gestionnaires de délégation pour créer un pipeline de middlewares pour les requêtes sortantes.</span><span class="sxs-lookup"><span data-stu-id="7b122-409">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="7b122-410">Ce modèle est similaire au pipeline de middlewares entrants dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7b122-410">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="7b122-411">Le modèle fournit un mécanisme permettant de gérer les problèmes transversaux liés aux des requêtes HTTP, notamment la mise en cache, la gestion des erreurs, la sérialisation et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="7b122-411">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="7b122-412">S’intègre à *Polly*, une bibliothèque tierce populaire pour la gestion des erreurs temporaires.</span><span class="sxs-lookup"><span data-stu-id="7b122-412">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="7b122-413">Gère le regroupement et la durée de vie des instances de `HttpClientMessageHandler` sous-jacentes pour éviter les problèmes DNS courants qui se produisent lors de la gestion manuelle des durées de vie de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7b122-413">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="7b122-414">Ajoute une expérience de journalisation configurable (via `ILogger`) pour toutes les requêtes envoyées via des clients créés par la fabrique.</span><span class="sxs-lookup"><span data-stu-id="7b122-414">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="7b122-415">Pour plus d’informations, consultez <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="7b122-415">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="7b122-416">Racine de contenu</span><span class="sxs-lookup"><span data-stu-id="7b122-416">Content root</span></span>

<span data-ttu-id="7b122-417">La racine du contenu est le chemin d’accès de base à :</span><span class="sxs-lookup"><span data-stu-id="7b122-417">The content root is the base path to the:</span></span>

* <span data-ttu-id="7b122-418">Exécutable hébergeant l’application ( *. exe*).</span><span class="sxs-lookup"><span data-stu-id="7b122-418">Executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="7b122-419">Assemblys compilés qui composent l’application ( *. dll*).</span><span class="sxs-lookup"><span data-stu-id="7b122-419">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="7b122-420">Fichiers de contenu sans code utilisés par l’application, tels que :</span><span class="sxs-lookup"><span data-stu-id="7b122-420">Non-code content files used by the app, such as:</span></span>
  * <span data-ttu-id="7b122-421">Fichiers Razor ( *. cshtml*, *. Razor*)</span><span class="sxs-lookup"><span data-stu-id="7b122-421">Razor files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="7b122-422">Fichiers de configuration ( *. JSON*, *. xml*)</span><span class="sxs-lookup"><span data-stu-id="7b122-422">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="7b122-423">Fichiers de données ( *. db*)</span><span class="sxs-lookup"><span data-stu-id="7b122-423">Data files (*.db*)</span></span>
* <span data-ttu-id="7b122-424">[Racine Web](#web-root), en général le dossier *wwwroot* publié.</span><span class="sxs-lookup"><span data-stu-id="7b122-424">[Web root](#web-root), typically the published *wwwroot* folder.</span></span>

<span data-ttu-id="7b122-425">Lors du développement :</span><span class="sxs-lookup"><span data-stu-id="7b122-425">During development:</span></span>

* <span data-ttu-id="7b122-426">La racine du contenu est par défaut le répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="7b122-426">The content root defaults to the project's root directory.</span></span>
* <span data-ttu-id="7b122-427">Le répertoire racine du projet est utilisé pour créer le :</span><span class="sxs-lookup"><span data-stu-id="7b122-427">The project's root directory is used to create the:</span></span>
  * <span data-ttu-id="7b122-428">Chemin d’accès aux fichiers de contenu sans code de l’application dans le répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="7b122-428">Path to the app's non-code content files in the project's root directory.</span></span>
  * <span data-ttu-id="7b122-429">[Racine Web](#web-root), généralement le dossier *wwwroot* dans le répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="7b122-429">[Web root](#web-root), typically the *wwwroot* folder in the project's root directory.</span></span>

<span data-ttu-id="7b122-430">Vous pouvez spécifier un autre chemin d’accès racine [de contenu lors de la génération de l’hôte](#host).</span><span class="sxs-lookup"><span data-stu-id="7b122-430">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="7b122-431">Pour plus d’informations, consultez <xref:fundamentals/host/web-host#content-root>.</span><span class="sxs-lookup"><span data-stu-id="7b122-431">For more information, see <xref:fundamentals/host/web-host#content-root>.</span></span>

## <a name="web-root"></a><span data-ttu-id="7b122-432">Racine web</span><span class="sxs-lookup"><span data-stu-id="7b122-432">Web root</span></span>

<span data-ttu-id="7b122-433">La racine Web est le chemin de base des fichiers de ressources statiques, non-code et publics, tels que :</span><span class="sxs-lookup"><span data-stu-id="7b122-433">The web root is the base path to public, non-code, static resource files, such as:</span></span>

* <span data-ttu-id="7b122-434">Feuilles de style ( *. CSS*)</span><span class="sxs-lookup"><span data-stu-id="7b122-434">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="7b122-435">JavaScript ( *. js*)</span><span class="sxs-lookup"><span data-stu-id="7b122-435">JavaScript (*.js*)</span></span>
* <span data-ttu-id="7b122-436">Images ( *. png*, *. jpg*)</span><span class="sxs-lookup"><span data-stu-id="7b122-436">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="7b122-437">Les fichiers statiques sont pris en charge par défaut uniquement à partir du répertoire racine Web (et des sous-répertoires).</span><span class="sxs-lookup"><span data-stu-id="7b122-437">Static files are only served by default from the web root directory (and sub-directories).</span></span>

<span data-ttu-id="7b122-438">Le chemin d’accès racine Web a comme valeur par défaut *{content root}/wwwroot*, mais une autre racine Web peut être spécifiée lors de [la génération de l’hôte](#host).</span><span class="sxs-lookup"><span data-stu-id="7b122-438">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="7b122-439">Pour plus d’informations, consultez [Racine web](xref:fundamentals/host/web-host#web-root).</span><span class="sxs-lookup"><span data-stu-id="7b122-439">For more information, see [Web root](xref:fundamentals/host/web-host#web-root).</span></span>

<span data-ttu-id="7b122-440">Empêchez la publication de fichiers dans *wwwroot* avec le [contenu\<> élément de projet](/visualstudio/msbuild/common-msbuild-project-items#content) dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="7b122-440">Prevent publishing files in *wwwroot* with the [\<Content> project item](/visualstudio/msbuild/common-msbuild-project-items#content) in the project file.</span></span> <span data-ttu-id="7b122-441">L’exemple suivant empêche la publication de contenu dans le répertoire et les sous-répertoires *wwwroot/local* :</span><span class="sxs-lookup"><span data-stu-id="7b122-441">The following example prevents publishing content in the *wwwroot/local* directory and sub-directories:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="7b122-442">Dans les fichiers Razor ( *. cshtml*), le tilde-slash (`~/`) pointe vers la racine Web.</span><span class="sxs-lookup"><span data-stu-id="7b122-442">In Razor (*.cshtml*) files, the tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="7b122-443">Un chemin d’accès commençant par `~/` est désigné sous le terme de « *chemin d’accès virtuel*».</span><span class="sxs-lookup"><span data-stu-id="7b122-443">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="7b122-444">Pour plus d’informations, consultez <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="7b122-444">For more information, see <xref:fundamentals/static-files>.</span></span>

::: moniker-end
