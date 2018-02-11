---
title: "Implémentation du serveur web Kestrel dans ASP.NET Core"
author: tdykstra
description: "Présente Kestrel, serveur web multiplateforme pour ASP.NET Core sur libuv."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: bfe7644891296c7c3485c9a870d90951ba87e9e8
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="7309a-103">Présentation de l’implémentation du serveur web Kestrel dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7309a-103">Introduction to Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="7309a-104">Par [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) et [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="7309a-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="7309a-105">[Kestrel](index.md) est un serveur web multiplateforme pour ASP.NET Core basé sur [libuv](https://github.com/libuv/libuv), bibliothèque d’E/S asynchrones multiplateforme.</span><span class="sxs-lookup"><span data-stu-id="7309a-105">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="7309a-106">Kestrel est le serveur web inclus par défaut dans les modèles de projets ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7309a-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="7309a-107">Kestrel prend en charge les fonctionnalités suivantes :</span><span class="sxs-lookup"><span data-stu-id="7309a-107">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="7309a-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="7309a-108">HTTPS</span></span>
  * <span data-ttu-id="7309a-109">Mise à niveau opaque utilisée pour activer les [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="7309a-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="7309a-110">Sockets UNIX pour des performances élevées derrière Nginx</span><span class="sxs-lookup"><span data-stu-id="7309a-110">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="7309a-111">Kestrel est pris en charge sur toutes les plateformes et les versions prises en charge par .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7309a-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7309a-112">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7309a-112">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7309a-113">[Affichez ou téléchargez l’exemple de code pour 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7309a-113">[View or download sample code for 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7309a-114">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7309a-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7309a-115">[Affichez ou téléchargez l’exemple de code pour 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7309a-115">[View or download sample code for 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="7309a-116">Quand utiliser Kestrel avec un proxy inverse ?</span><span class="sxs-lookup"><span data-stu-id="7309a-116">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7309a-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7309a-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7309a-118">Vous pouvez utiliser uniquement Kestrel ou l’associer à un *serveur proxy inverse*, par exemple IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="7309a-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="7309a-119">Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel après un traitement préliminaire.</span><span class="sxs-lookup"><span data-stu-id="7309a-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel communique directement avec Internet sans serveur proxy inverse](kestrel/_static/kestrel-to-internet2.png)

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="7309a-122">L’une ou l’autre des configurations, &mdash;avec ou sans serveur proxy inverse&mdash;, peut également être utilisée si Kestrel est exposé uniquement à un réseau interne.</span><span class="sxs-lookup"><span data-stu-id="7309a-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7309a-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7309a-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7309a-124">Si votre application accepte uniquement les requêtes d’un réseau interne, vous pouvez utiliser Kestrel de manière autonome.</span><span class="sxs-lookup"><span data-stu-id="7309a-124">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel communique directement avec votre réseau interne](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="7309a-126">Si vous exposez votre application à Internet, vous devez utiliser IIS, Nginx ou Apache en tant que *serveur proxy inverse*.</span><span class="sxs-lookup"><span data-stu-id="7309a-126">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="7309a-127">Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel après un traitement préliminaire.</span><span class="sxs-lookup"><span data-stu-id="7309a-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="7309a-129">Un proxy inverse est requis pour les déploiements Edge (exposés au trafic en provenance d’Internet) pour des raisons de sécurité.</span><span class="sxs-lookup"><span data-stu-id="7309a-129">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="7309a-130">Les versions 1.x de Kestrel n’ont pas un ensemble complet de mesures de défense contre les attaques.</span><span class="sxs-lookup"><span data-stu-id="7309a-130">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="7309a-131">Cela inclut, entre autres choses, les délais d’attente appropriés, les limites de taille et les limites de connexions simultanées.</span><span class="sxs-lookup"><span data-stu-id="7309a-131">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="7309a-132">Par exemple, un proxy inverse est nécessaire si plusieurs applications qui partagent les mêmes adresse IP et port s’exécutent sur un seul serveur.</span><span class="sxs-lookup"><span data-stu-id="7309a-132">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="7309a-133">Cette configuration ne fonctionne pas avec Kestrel directement, car ce dernier ne prend pas en charge le partage des mêmes adresse IP et port entre plusieurs processus.</span><span class="sxs-lookup"><span data-stu-id="7309a-133">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="7309a-134">Quand vous configurez Kestrel pour qu’il écoute sur un port, il gère tout le trafic pour ce port, quel que soit l’en-tête d’hôte.</span><span class="sxs-lookup"><span data-stu-id="7309a-134">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="7309a-135">Un proxy inverse qui peut partager des ports doit ensuite transférer les données à Kestrel sur une adresse IP et un port uniques.</span><span class="sxs-lookup"><span data-stu-id="7309a-135">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="7309a-136">Même si un serveur proxy inverse n’est pas nécessaire, en utiliser un peut être un bon choix pour d’autres raisons :</span><span class="sxs-lookup"><span data-stu-id="7309a-136">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="7309a-137">Il peut limiter votre surface d’exposition.</span><span class="sxs-lookup"><span data-stu-id="7309a-137">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="7309a-138">Il fournit une couche de configuration et de défense supplémentaire facultative.</span><span class="sxs-lookup"><span data-stu-id="7309a-138">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="7309a-139">Il peut mieux s’intégrer à l’infrastructure existante.</span><span class="sxs-lookup"><span data-stu-id="7309a-139">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="7309a-140">Il simplifie l’équilibrage de charge la configuration SSL.</span><span class="sxs-lookup"><span data-stu-id="7309a-140">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="7309a-141">Seul votre serveur proxy inverse requiert un certificat SSL ; ce serveur peut donc communiquer avec vos serveurs d’applications sur le réseau interne à l’aide de requêtes HTTP normales.</span><span class="sxs-lookup"><span data-stu-id="7309a-141">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="7309a-142">Comment utiliser Kestrel dans les applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7309a-142">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7309a-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7309a-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7309a-144">Le package [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) est inclus dans le [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="7309a-144">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="7309a-145">Les modèles de projet ASP.NET Core utilisent Kestrel par défaut.</span><span class="sxs-lookup"><span data-stu-id="7309a-145">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="7309a-146">Dans *Program.cs*, le modèle de code appelle `CreateDefaultBuilder`, qui appelle [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="7309a-146">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="7309a-147">Si vous avez besoin de configurer des options Kestrel, appelez `UseKestrel` dans *Program.cs*, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="7309a-147">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7309a-148">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7309a-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7309a-149">Installez le package NuGet [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).</span><span class="sxs-lookup"><span data-stu-id="7309a-149">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="7309a-150">Appelez la méthode d’extension [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) sur `WebHostBuilder` dans votre méthode `Main`, en spécifiant les [options Kestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) dont vous avez besoin, comme indiqué dans la prochaine section.</span><span class="sxs-lookup"><span data-stu-id="7309a-150">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="7309a-151">Options Kestrel</span><span class="sxs-lookup"><span data-stu-id="7309a-151">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7309a-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7309a-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7309a-153">Le serveur web Kestrel a des options de configuration de contrainte qui sont particulièrement utiles dans les déploiements exposés à Internet.</span><span class="sxs-lookup"><span data-stu-id="7309a-153">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="7309a-154">Voici quelques-unes des limites que vous pouvez définir :</span><span class="sxs-lookup"><span data-stu-id="7309a-154">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="7309a-155">Nombre maximale de connexions client</span><span class="sxs-lookup"><span data-stu-id="7309a-155">Maximum client connections</span></span>
- <span data-ttu-id="7309a-156">Taille maximale du corps de la requête</span><span class="sxs-lookup"><span data-stu-id="7309a-156">Maximum request body size</span></span>
- <span data-ttu-id="7309a-157">Débit données minimal du corps de la requête</span><span class="sxs-lookup"><span data-stu-id="7309a-157">Minimum request body data rate</span></span>

<span data-ttu-id="7309a-158">Vous définissez ces contraintes et d’autres dans la propriété `Limits` de la classe [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="7309a-158">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="7309a-159">La propriété `Limits` conserve une instance de la classe [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs).</span><span class="sxs-lookup"><span data-stu-id="7309a-159">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="7309a-160">**Nombre maximal de connexions client**</span><span class="sxs-lookup"><span data-stu-id="7309a-160">**Maximum client connections**</span></span>

<span data-ttu-id="7309a-161">Le nombre maximal de connexions TCP ouvertes simultanées peut être défini pour l’application entière avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="7309a-161">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="7309a-162">Il existe une limite distincte pour les connexions qui ont été mises à niveau à partir de HTTP ou HTTPS vers un autre protocole (par exemple, sur une demande WebSocket).</span><span class="sxs-lookup"><span data-stu-id="7309a-162">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="7309a-163">Une fois mise à niveau, une connexion n’est pas prise en compte dans la limite `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="7309a-163">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="7309a-164">Le nombre maximal de connexions est illimité (null) par défaut.</span><span class="sxs-lookup"><span data-stu-id="7309a-164">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="7309a-165">**Taille maximale du corps de la demande**</span><span class="sxs-lookup"><span data-stu-id="7309a-165">**Maximum request body size**</span></span>

<span data-ttu-id="7309a-166">La taille maximale par défaut du corps de la demande est de 30 000 000 octets, soit environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="7309a-166">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="7309a-167">Pour remplacer la limite dans une application MVC ASP.NET Core, nous vous recommandons d’utiliser l’attribut [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) sur une méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="7309a-167">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="7309a-168">Voici un exemple qui montre comment configurer la contrainte pour l’application entière et chaque demande :</span><span class="sxs-lookup"><span data-stu-id="7309a-168">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="7309a-169">Vous pouvez remplacer le paramètre sur une demande spécifique dans l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="7309a-169">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="7309a-170">Une exception est levée si vous essayez de configurer la limite sur une demande dont l’application a commencé la lecture.</span><span class="sxs-lookup"><span data-stu-id="7309a-170">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="7309a-171">Il existe une propriété `IsReadOnly` qui indique si la propriété `MaxRequestBodySize` est en lecture seule ; si tel est le cas, il est trop tard pour configurer la limite.</span><span class="sxs-lookup"><span data-stu-id="7309a-171">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="7309a-172">**Débit données minimal du corps de la demande**</span><span class="sxs-lookup"><span data-stu-id="7309a-172">**Minimum request body data rate**</span></span>

<span data-ttu-id="7309a-173">Kestrel vérifie à chaque seconde si les données arrivent au débit spécifié en octets/seconde.</span><span class="sxs-lookup"><span data-stu-id="7309a-173">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="7309a-174">Si le débit est inférieur au minimum, la connexion expire. La période de grâce est la durée que Kestrel accorde au client pour augmenter sa vitesse de transmission jusqu’à la limite minimale ; pendant cette période, le débit n’est pas vérifié.</span><span class="sxs-lookup"><span data-stu-id="7309a-174">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="7309a-175">La période de grâce permet d’éviter la suppression des connexions qui, initialement, envoient des données à une vitesse lente en raison de la lenteur du démarrage de TCP.</span><span class="sxs-lookup"><span data-stu-id="7309a-175">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="7309a-176">Le débit minimal par défaut est 240 octets/seconde, avec une période de grâce de 5 secondes.</span><span class="sxs-lookup"><span data-stu-id="7309a-176">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="7309a-177">Un débit minimal s’applique également à la réponse.</span><span class="sxs-lookup"><span data-stu-id="7309a-177">A minimum rate also applies to the response.</span></span> <span data-ttu-id="7309a-178">Le code pour définir les limites de demande et de réponse est identique à l’exception de `RequestBody` ou `Response` dans les noms de propriété et d’interface.</span><span class="sxs-lookup"><span data-stu-id="7309a-178">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="7309a-179">Voici un exemple qui montre comment configurer les débits de données minimaux dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="7309a-179">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="7309a-180">Vous pouvez configurer les débits par demande dans l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="7309a-180">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="7309a-181">Pour plus d’informations sur les autres options Kestrel, consultez les classes suivantes :</span><span class="sxs-lookup"><span data-stu-id="7309a-181">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="7309a-182">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="7309a-182">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="7309a-183">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="7309a-183">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="7309a-184">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="7309a-184">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7309a-185">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7309a-185">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7309a-186">Pour plus d’informations sur les options de , consultez [Classe KestrelServerOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="7309a-186">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="7309a-187">Configuration de point de terminaison</span><span class="sxs-lookup"><span data-stu-id="7309a-187">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7309a-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7309a-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7309a-189">Par défaut, ASP.NET Core est lié à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="7309a-189">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="7309a-190">Vous configurez des préfixes d’URL et des ports d’écoute pour Kestrel en appelant les méthodes `Listen` ou `ListenUnixSocket` sur `KestrelServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="7309a-190">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="7309a-191">(`UseUrls`, l’argument de ligne de commande `urls` et la variable d’environnement ASPNETCORE_URLS fonctionnent également, mais ils présentent les limitations indiquées [plus loin dans cet article](#useurls-limitations).)</span><span class="sxs-lookup"><span data-stu-id="7309a-191">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="7309a-192">**Lier à un socket TCP**</span><span class="sxs-lookup"><span data-stu-id="7309a-192">**Bind to a TCP socket**</span></span>

<span data-ttu-id="7309a-193">La méthode `Listen` lie à un socket TCP, et une expression lambda options vous permet de configurer un certificat SSL :</span><span class="sxs-lookup"><span data-stu-id="7309a-193">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="7309a-194">Notez la façon dont cet exemple configure SSL pour un point de terminaison particulier à l’aide de [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="7309a-194">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="7309a-195">Vous pouvez utiliser la même API afin de configurer d’autres paramètres Kestrel pour des points de terminaison particuliers.</span><span class="sxs-lookup"><span data-stu-id="7309a-195">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

<span data-ttu-id="7309a-196">**Lier à un socket Unix**</span><span class="sxs-lookup"><span data-stu-id="7309a-196">**Bind to a Unix socket**</span></span>

<span data-ttu-id="7309a-197">Vous pouvez écouter sur un socket Unix pour améliorer les performances avec Nginx, comme illustré dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="7309a-197">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="7309a-198">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="7309a-198">**Port 0**</span></span>

<span data-ttu-id="7309a-199">Si vous spécifiez le numéro de port 0, Kestrel lie dynamiquement à un port disponible.</span><span class="sxs-lookup"><span data-stu-id="7309a-199">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="7309a-200">L’exemple suivant montre comment déterminer le port auquel Kestrel a effectué la liaison à l’exécution :</span><span class="sxs-lookup"><span data-stu-id="7309a-200">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="7309a-201">**Limitations de UseUrls**</span><span class="sxs-lookup"><span data-stu-id="7309a-201">**UseUrls limitations**</span></span>

<span data-ttu-id="7309a-202">Vous pouvez configurer des points de terminaison en appelant la méthode `UseUrls` ou à l’aide de l’argument de ligne de commande `urls` ou de la variable d’environnement ASPNETCORE_URLS.</span><span class="sxs-lookup"><span data-stu-id="7309a-202">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="7309a-203">Ces méthodes sont utiles si vous souhaitez que votre code fonctionne avec les serveurs autres que Kestrel.</span><span class="sxs-lookup"><span data-stu-id="7309a-203">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="7309a-204">Toutefois, tenez compte des limitations suivantes :</span><span class="sxs-lookup"><span data-stu-id="7309a-204">However, be aware of these limitations:</span></span>

* <span data-ttu-id="7309a-205">Vous ne pouvez pas utiliser SSL avec ces méthodes.</span><span class="sxs-lookup"><span data-stu-id="7309a-205">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="7309a-206">Si vous utilisez à la fois la méthode `Listen` et `UseUrls`, les points de terminaison `Listen` remplacent les points de terminaison `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="7309a-206">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="7309a-207">**Configuration de point de terminaison pour IIS**</span><span class="sxs-lookup"><span data-stu-id="7309a-207">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="7309a-208">Si vous utilisez IIS, les liaisons d’URL pour IIS remplacent les liaisons que vous définissez en appelant `Listen` ou `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="7309a-208">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="7309a-209">Pour plus d’informations, consultez [Introduction au module ASP.NET Core](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="7309a-209">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7309a-210">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7309a-210">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7309a-211">Par défaut, ASP.NET Core est lié à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="7309a-211">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="7309a-212">Vous pouvez configurer les préfixes d’URL et les ports d’écoute de Kestrel en utilisant la méthode d’extension `UseUrls`, l’argument de ligne de commande `urls` ou le système de configuration ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7309a-212">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="7309a-213">Pour plus d’informations sur ces méthodes, consultez [Hébergement](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="7309a-213">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="7309a-214">Pour plus d’informations sur le fonctionnement de la liaison d’URL quand vous utilisez IIS comme proxy inverse, consultez [Module ASP.NET Core](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="7309a-214">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="7309a-215">Préfixes d’URL</span><span class="sxs-lookup"><span data-stu-id="7309a-215">URL prefixes</span></span>

<span data-ttu-id="7309a-216">Si vous appelez `UseUrls` ou utilisez l’argument de ligne de commande `urls` ou la variable d’environnement ASPNETCORE_URLS, les préfixes d’URL peuvent être dans l’un des formats suivants.</span><span class="sxs-lookup"><span data-stu-id="7309a-216">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7309a-217">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7309a-217">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7309a-218">Seuls les préfixes d’URL HTTP sont valides ; Kestrel ne prend pas en charge SSL quand vous configurez les liaisons d’URL à l’aide de `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="7309a-218">Only HTTP URL prefixes are valid; Kestrel doesn't support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="7309a-219">Adresse IPv4 avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="7309a-219">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="7309a-220">0.0.0.0 est un cas spécial qui est lié à toutes les adresses IPv4.</span><span class="sxs-lookup"><span data-stu-id="7309a-220">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="7309a-221">Adresse IPv6 avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="7309a-221">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="7309a-222">[::] est l’équivalent IPv6 de IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="7309a-222">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="7309a-223">Nom d’hôte avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="7309a-223">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="7309a-224">Les noms d’hôte, \* et + ne sont pas spéciaux.</span><span class="sxs-lookup"><span data-stu-id="7309a-224">Host names, \*, and +, are not special.</span></span> <span data-ttu-id="7309a-225">Tout ce qui n’est pas une adresse IP reconnue ou « localhost » est lié à toutes les adresses IP IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="7309a-225">Anything that's not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="7309a-226">Si vous devez lier différents noms d’hôte à différentes applications ASP.NET Core sur le même port, utilisez [HTTP.sys](httpsys.md) ou un serveur proxy inverse, comme IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="7309a-226">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="7309a-227">Nom «localhost » avec numéro de port ou adresse IP de bouclage avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="7309a-227">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="7309a-228">Quand `localhost` est spécifié, Kestrel essaie de lier aux interfaces de bouclage IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="7309a-228">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="7309a-229">Si le port demandé est en cours d’utilisation par un autre service sur l’une des interfaces de bouclage, Kestrel ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="7309a-229">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="7309a-230">Si l’une des interfaces de bouclage n’est pas disponible pour une raison quelconque (généralement du fait de la non-prise en charge d’IPv6), Kestrel journalise un avertissement.</span><span class="sxs-lookup"><span data-stu-id="7309a-230">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7309a-231">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7309a-231">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="7309a-232">Adresse IPv4 avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="7309a-232">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="7309a-233">0.0.0.0 est un cas spécial qui est lié à toutes les adresses IPv4.</span><span class="sxs-lookup"><span data-stu-id="7309a-233">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="7309a-234">Adresse IPv6 avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="7309a-234">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="7309a-235">[::] est l’équivalent IPv6 de IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="7309a-235">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="7309a-236">Nom d’hôte avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="7309a-236">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="7309a-237">Les noms d’hôte, \* et + ne sont pas spéciaux.</span><span class="sxs-lookup"><span data-stu-id="7309a-237">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="7309a-238">Tout ce qui n’est pas une adresse IP reconnue ou « localhost » est lié à toutes les adresses IP IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="7309a-238">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="7309a-239">Si vous devez lier différents noms d’hôte à différentes applications ASP.NET Core sur le même port, utilisez [WebListener](weblistener.md) ou un serveur proxy inverse, comme IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="7309a-239">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="7309a-240">Nom «localhost » avec numéro de port ou adresse IP de bouclage avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="7309a-240">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="7309a-241">Quand `localhost` est spécifié, Kestrel essaie de lier aux interfaces de bouclage IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="7309a-241">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="7309a-242">Si le port demandé est en cours d’utilisation par un autre service sur l’une des interfaces de bouclage, Kestrel ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="7309a-242">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="7309a-243">Si l’une des interfaces de bouclage n’est pas disponible pour une raison quelconque (généralement du fait de la non-prise en charge d’IPv6), Kestrel journalise un avertissement.</span><span class="sxs-lookup"><span data-stu-id="7309a-243">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="7309a-244">Socket UNIX</span><span class="sxs-lookup"><span data-stu-id="7309a-244">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="7309a-245">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="7309a-245">**Port 0**</span></span>

<span data-ttu-id="7309a-246">Si vous spécifiez le numéro de port 0, Kestrel lie dynamiquement à un port disponible.</span><span class="sxs-lookup"><span data-stu-id="7309a-246">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="7309a-247">La liaison au port 0 est autorisée pour n’importe quel nom d’hôte ou adresse IP à l’exception du nom `localhost`.</span><span class="sxs-lookup"><span data-stu-id="7309a-247">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="7309a-248">L’exemple suivant montre comment déterminer le port auquel Kestrel a effectué la liaison à l’exécution :</span><span class="sxs-lookup"><span data-stu-id="7309a-248">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="7309a-249">**Préfixes d’URL pour SSL**</span><span class="sxs-lookup"><span data-stu-id="7309a-249">**URL prefixes for SSL**</span></span>

<span data-ttu-id="7309a-250">Veillez à inclure les préfixes d’URL avec `https:` si vous appelez la méthode d’extension `UseHttps`, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="7309a-250">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

```csharp
var host = new WebHostBuilder() 
    .UseKestrel(options => 
    { 
        options.UseHttps("testCert.pfx", "testPassword"); 
    }) 
   .UseUrls("http://localhost:5000", "https://localhost:5001") 
   .UseContentRoot(Directory.GetCurrentDirectory()) 
   .UseStartup<Startup>() 
   .Build(); 
```

> [!NOTE]
> <span data-ttu-id="7309a-251">HTTPS et HTTP ne peuvent pas être hébergés sur le même port.</span><span class="sxs-lookup"><span data-stu-id="7309a-251">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a><span data-ttu-id="7309a-252">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="7309a-252">Next steps</span></span>

<span data-ttu-id="7309a-253">Pour plus d'informations, reportez-vous aux ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="7309a-253">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7309a-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7309a-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="7309a-255">Exemple d’application pour 2.x</span><span class="sxs-lookup"><span data-stu-id="7309a-255">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="7309a-256">Code source Kestrel</span><span class="sxs-lookup"><span data-stu-id="7309a-256">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7309a-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7309a-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="7309a-258">Exemple d’application pour 1.x</span><span class="sxs-lookup"><span data-stu-id="7309a-258">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="7309a-259">Code source Kestrel</span><span class="sxs-lookup"><span data-stu-id="7309a-259">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---
