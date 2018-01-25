---
title: "Implémentation du serveur web kestrel ASP.NET Core"
author: tdykstra
description: "Introduit Kestrel, le serveur web d’inter-plateformes pour ASP.NET Core selon libuv."
ms.author: tdykstra
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/kestrel
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3e2b28f15e47789ac89213e57396060ee356ee33
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="b9efb-103">Introduction à l’implémentation du serveur web Kestrel ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b9efb-103">Introduction to Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="b9efb-104">Par [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), et [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="b9efb-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="b9efb-105">Kestrel est un inter-plateforme [serveur web pour ASP.NET Core](index.md) selon [libuv](https://github.com/libuv/libuv), une bibliothèque d’e/s asynchrones inter-plateformes.</span><span class="sxs-lookup"><span data-stu-id="b9efb-105">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="b9efb-106">Kestrel est le serveur web qui est inclus par défaut dans les modèles de projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b9efb-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="b9efb-107">Kestrel prend en charge les fonctionnalités suivantes :</span><span class="sxs-lookup"><span data-stu-id="b9efb-107">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="b9efb-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="b9efb-108">HTTPS</span></span>
  * <span data-ttu-id="b9efb-109">Permet d’activer de mise à niveau opaque [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="b9efb-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="b9efb-110">Sockets UNIX pour des performances élevées derrière Nginx</span><span class="sxs-lookup"><span data-stu-id="b9efb-110">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="b9efb-111">Kestrel est pris en charge sur toutes les plateformes et les versions qui prend en charge de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b9efb-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b9efb-112">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b9efb-112">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b9efb-113">[Afficher ou télécharger l’exemple de code pour 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b9efb-113">[View or download sample code for 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b9efb-114">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b9efb-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b9efb-115">[Afficher ou télécharger l’exemple de code pour 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b9efb-115">[View or download sample code for 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="b9efb-116">Quand utiliser Kestrel avec un proxy inverse</span><span class="sxs-lookup"><span data-stu-id="b9efb-116">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b9efb-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b9efb-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b9efb-118">Vous pouvez utiliser uniquement Kestrel ou l’associer à un *serveur proxy inverse*, par exemple IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="b9efb-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="b9efb-119">Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel après un traitement préliminaire.</span><span class="sxs-lookup"><span data-stu-id="b9efb-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel communique directement avec Internet sans serveur proxy inverse](kestrel/_static/kestrel-to-internet2.png)

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="b9efb-122">L’une ou l’autre des configurations, &mdash;avec ou sans serveur proxy inverse&mdash;, peut également être utilisée si Kestrel est exposé uniquement à un réseau interne.</span><span class="sxs-lookup"><span data-stu-id="b9efb-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b9efb-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b9efb-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b9efb-124">Si votre application accepte uniquement les requêtes d’un réseau interne, vous pouvez utiliser Kestrel de manière autonome.</span><span class="sxs-lookup"><span data-stu-id="b9efb-124">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel communique directement avec votre réseau interne](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="b9efb-126">Si vous exposez votre application à Internet, vous devez utiliser IIS, Nginx ou Apache en tant que *serveur proxy inverse*.</span><span class="sxs-lookup"><span data-stu-id="b9efb-126">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="b9efb-127">Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel après un traitement préliminaire.</span><span class="sxs-lookup"><span data-stu-id="b9efb-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="b9efb-129">Un proxy inverse est requis pour les déploiements de bord (exposés au trafic Internet) pour des raisons de sécurité.</span><span class="sxs-lookup"><span data-stu-id="b9efb-129">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="b9efb-130">Les versions 1.x de Kestrel n’ont pas un ensemble complet de mesures de défense contre les attaques.</span><span class="sxs-lookup"><span data-stu-id="b9efb-130">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="b9efb-131">Cela inclut, mais n’est pas limité à des délais d’attente appropriée, les limites de taille et limite de connexions simultanées.</span><span class="sxs-lookup"><span data-stu-id="b9efb-131">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="b9efb-132">Un scénario qui nécessite un proxy inverse est lorsque vous possédez plusieurs applications qui partagent la même adresse IP et le même port en cours d’exécution sur un seul serveur.</span><span class="sxs-lookup"><span data-stu-id="b9efb-132">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="b9efb-133">Cela ne fonctionne pas avec Kestrel directement Kestrel ne gère pas l’adresse IP et un port entre plusieurs processus même de partage.</span><span class="sxs-lookup"><span data-stu-id="b9efb-133">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="b9efb-134">Lorsque vous configurez Kestrel à écouter un port, il gère tout le trafic pour ce port, quelle que soit l’en-tête d’hôte.</span><span class="sxs-lookup"><span data-stu-id="b9efb-134">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="b9efb-135">Un proxy inverse qui permettre partager des ports doit ensuite transmettre à Kestrel sur une adresse IP et un port unique.</span><span class="sxs-lookup"><span data-stu-id="b9efb-135">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="b9efb-136">Même si un serveur proxy inverse n’est pas nécessaire, à l’aide d’un peut être un bon choix pour d’autres raisons :</span><span class="sxs-lookup"><span data-stu-id="b9efb-136">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="b9efb-137">Il peut limiter votre surface exposée.</span><span class="sxs-lookup"><span data-stu-id="b9efb-137">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="b9efb-138">Il fournit une couche supplémentaire facultatif de la configuration et de la défense.</span><span class="sxs-lookup"><span data-stu-id="b9efb-138">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="b9efb-139">Il peut mieux intégrer à l’infrastructure existante.</span><span class="sxs-lookup"><span data-stu-id="b9efb-139">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="b9efb-140">Elle simplifie la configuration SSL et l’équilibrage de charge.</span><span class="sxs-lookup"><span data-stu-id="b9efb-140">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="b9efb-141">Seulement votre serveur proxy inverse requiert un certificat SSL, et ce serveur peut communiquer avec vos serveurs d’applications sur le réseau interne à l’aide de HTTP brut.</span><span class="sxs-lookup"><span data-stu-id="b9efb-141">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="b9efb-142">Comment utiliser Kestrel dans les applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b9efb-142">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b9efb-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b9efb-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b9efb-144">Le [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package est inclus dans le [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="b9efb-144">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="b9efb-145">Modèles de projet ASP.NET Core utilisent Kestrel par défaut.</span><span class="sxs-lookup"><span data-stu-id="b9efb-145">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="b9efb-146">Dans *Program.cs*, le modèle de code appelle `CreateDefaultBuilder`, qui appelle [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="b9efb-146">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="b9efb-147">Si vous avez besoin configurer les options de Kestrel, appelez `UseKestrel` dans *Program.cs* comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="b9efb-147">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b9efb-148">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b9efb-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b9efb-149">Installer le [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="b9efb-149">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="b9efb-150">Appelez le [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) méthode d’extension sur `WebHostBuilder` dans votre `Main` méthode, en spécifiant les [options de Kestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) dont vous avez besoin, comme indiqué dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="b9efb-150">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="b9efb-151">Options de kestrel</span><span class="sxs-lookup"><span data-stu-id="b9efb-151">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b9efb-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b9efb-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b9efb-153">Le serveur web Kestrel a des options de configuration de contrainte sont particulièrement utiles dans les déploiements exposés à Internet.</span><span class="sxs-lookup"><span data-stu-id="b9efb-153">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="b9efb-154">Voici quelques-unes des limites que vous pouvez définir :</span><span class="sxs-lookup"><span data-stu-id="b9efb-154">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="b9efb-155">Nombre maximale de connexions client</span><span class="sxs-lookup"><span data-stu-id="b9efb-155">Maximum client connections</span></span>
- <span data-ttu-id="b9efb-156">Taille maximale du corps de la requête</span><span class="sxs-lookup"><span data-stu-id="b9efb-156">Maximum request body size</span></span>
- <span data-ttu-id="b9efb-157">Débit données minimal du corps de la requête</span><span class="sxs-lookup"><span data-stu-id="b9efb-157">Minimum request body data rate</span></span>

<span data-ttu-id="b9efb-158">Vous définissez ces contraintes et autres dans le `Limits` propriété de la [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) classe.</span><span class="sxs-lookup"><span data-stu-id="b9efb-158">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="b9efb-159">Le `Limits` propriété conserve une instance de la [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) classe.</span><span class="sxs-lookup"><span data-stu-id="b9efb-159">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="b9efb-160">**Connexions clientes maximale**</span><span class="sxs-lookup"><span data-stu-id="b9efb-160">**Maximum client connections**</span></span>

<span data-ttu-id="b9efb-161">Le nombre maximal de connexions TCP simultanées et ouvertes peut être défini pour l’application entière avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b9efb-161">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="b9efb-162">Il existe une limite distincte pour les connexions qui ont été mis à niveau à partir de HTTP ou HTTPS vers un autre protocole (par exemple, sur une demande WebSocket).</span><span class="sxs-lookup"><span data-stu-id="b9efb-162">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="b9efb-163">Une fois une connexion est mise à niveau, il n’est pas compté dans le `MaxConcurrentConnections` limite.</span><span class="sxs-lookup"><span data-stu-id="b9efb-163">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="b9efb-164">Le nombre maximal de connexions est illimité (null) par défaut.</span><span class="sxs-lookup"><span data-stu-id="b9efb-164">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="b9efb-165">**Taille du corps de demande maximale**</span><span class="sxs-lookup"><span data-stu-id="b9efb-165">**Maximum request body size**</span></span>

<span data-ttu-id="b9efb-166">La taille du corps de demande maximale par défaut est 30 000 000 octets, qui est d’environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="b9efb-166">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="b9efb-167">La méthode recommandée pour remplacer la limite dans une application ASP.NET MVC de base consiste à utiliser le [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribut sur une méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="b9efb-167">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="b9efb-168">Voici un exemple qui montre comment configurer la contrainte pour l’application entière, chaque demande :</span><span class="sxs-lookup"><span data-stu-id="b9efb-168">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="b9efb-169">Vous pouvez remplacer le paramètre sur une demande spécifique dans l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="b9efb-169">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="b9efb-170">Une exception est levée si vous essayez de configurer la limite sur une demande une fois que l’application a démarré la lecture de la demande.</span><span class="sxs-lookup"><span data-stu-id="b9efb-170">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="b9efb-171">Il existe un `IsReadOnly` propriété qui indique si le `MaxRequestBodySize` propriété est en lecture seule, ce qui signifie qu’il est trop tard pour configurer la limite.</span><span class="sxs-lookup"><span data-stu-id="b9efb-171">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="b9efb-172">**Taux de données de corps de demande minimale**</span><span class="sxs-lookup"><span data-stu-id="b9efb-172">**Minimum request body data rate**</span></span>

<span data-ttu-id="b9efb-173">Kestrel vérifie chaque seconde si les données arrivent à la vitesse spécifiée en octets/seconde.</span><span class="sxs-lookup"><span data-stu-id="b9efb-173">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="b9efb-174">Si le taux est inférieur au minimum, la délai de connexion. La période de grâce est la quantité de temps que Kestrel donne le client à augmenter sa vitesse de transmission jusqu'à la limite minimale ; pendant ce temps, la vitesse n’est pas vérifiée.</span><span class="sxs-lookup"><span data-stu-id="b9efb-174">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="b9efb-175">La période de grâce permet d’éviter la suppression des connexions qui initialement envoient des données à une vitesse lente en raison de démarrage lent TCP.</span><span class="sxs-lookup"><span data-stu-id="b9efb-175">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="b9efb-176">La fréquence minimale par défaut est 240 octets/seconde, avec une période de grâce de 5 secondes.</span><span class="sxs-lookup"><span data-stu-id="b9efb-176">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="b9efb-177">Un taux minimum s’applique également à la réponse.</span><span class="sxs-lookup"><span data-stu-id="b9efb-177">A minimum rate also applies to the response.</span></span> <span data-ttu-id="b9efb-178">Le code pour définir la limite de demandes et de la limite de la réponse est identique à l’exception des `RequestBody` ou `Response` dans les noms de propriété et d’interface.</span><span class="sxs-lookup"><span data-stu-id="b9efb-178">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="b9efb-179">Voici un exemple qui montre comment configurer les taux de données minimal dans *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="b9efb-179">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="b9efb-180">Vous pouvez configurer les taux de demande dans l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="b9efb-180">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="b9efb-181">Pour plus d’informations sur les autres options Kestrel, consultez les classes suivantes :</span><span class="sxs-lookup"><span data-stu-id="b9efb-181">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="b9efb-182">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="b9efb-182">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="b9efb-183">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="b9efb-183">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="b9efb-184">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="b9efb-184">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b9efb-185">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b9efb-185">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b9efb-186">Pour plus d’informations sur les options de Kestrel, consultez [KestrelServerOptions classe](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="b9efb-186">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="b9efb-187">Configuration de point de terminaison</span><span class="sxs-lookup"><span data-stu-id="b9efb-187">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b9efb-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b9efb-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b9efb-189">Par défaut, ASP.NET Core lie à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b9efb-189">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="b9efb-190">Vous configurez des préfixes d’URL et ports pour Kestrel d’écouter en appelant `Listen` ou `ListenUnixSocket` méthodes sur `KestrelServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="b9efb-190">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="b9efb-191">(`UseUrls`, le `urls` argument de ligne de commande et la variable d’environnement ASPNETCORE_URLS fonctionne également mais ils présentent les limitations de noter [plus loin dans cet article](#useurls-limitations).)</span><span class="sxs-lookup"><span data-stu-id="b9efb-191">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="b9efb-192">**Lier à un socket TCP**</span><span class="sxs-lookup"><span data-stu-id="b9efb-192">**Bind to a TCP socket**</span></span>

<span data-ttu-id="b9efb-193">Le `Listen` méthode lie à un socket TCP, et une expression lambda options vous permet de configurer un certificat SSL :</span><span class="sxs-lookup"><span data-stu-id="b9efb-193">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="b9efb-194">Notez la façon dont cet exemple configure le SSL pour un point de terminaison particulier à l’aide de [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="b9efb-194">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="b9efb-195">Vous pouvez utiliser la même API pour configurer les autres paramètres Kestrel des points de terminaison particuliers.</span><span class="sxs-lookup"><span data-stu-id="b9efb-195">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

<span data-ttu-id="b9efb-196">**Lier à un socket Unix**</span><span class="sxs-lookup"><span data-stu-id="b9efb-196">**Bind to a Unix socket**</span></span>

<span data-ttu-id="b9efb-197">Vous pouvez écouter sur un socket Unix pour améliorer les performances avec Nginx, comme illustré dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="b9efb-197">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="b9efb-198">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="b9efb-198">**Port 0**</span></span>

<span data-ttu-id="b9efb-199">Si vous spécifiez le numéro de port 0, Kestrel lie dynamiquement un port disponible.</span><span class="sxs-lookup"><span data-stu-id="b9efb-199">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="b9efb-200">L’exemple suivant montre comment déterminer le port Kestrel effectivement lié à l’exécution :</span><span class="sxs-lookup"><span data-stu-id="b9efb-200">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="b9efb-201">**Limitations de UseUrls**</span><span class="sxs-lookup"><span data-stu-id="b9efb-201">**UseUrls limitations**</span></span>

<span data-ttu-id="b9efb-202">Vous pouvez configurer des points de terminaison en appelant le `UseUrls` méthode ou à l’aide de la `urls` argument de ligne de commande ou la variable d’environnement ASPNETCORE_URLS.</span><span class="sxs-lookup"><span data-stu-id="b9efb-202">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="b9efb-203">Ces méthodes sont utiles si vous souhaitez que votre code fonctionne avec les serveurs autres que Kestrel.</span><span class="sxs-lookup"><span data-stu-id="b9efb-203">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="b9efb-204">Toutefois, tenez compte de ces limitations :</span><span class="sxs-lookup"><span data-stu-id="b9efb-204">However, be aware of these limitations:</span></span>

* <span data-ttu-id="b9efb-205">Vous ne pouvez pas utiliser SSL avec ces méthodes.</span><span class="sxs-lookup"><span data-stu-id="b9efb-205">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="b9efb-206">Si vous utilisez à la fois le `Listen` (méthode) et `UseUrls`, le `Listen` substituer des points de terminaison le `UseUrls` points de terminaison.</span><span class="sxs-lookup"><span data-stu-id="b9efb-206">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="b9efb-207">**Configuration de point de terminaison pour IIS**</span><span class="sxs-lookup"><span data-stu-id="b9efb-207">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="b9efb-208">Si vous utilisez IIS, les liaisons d’URL pour IIS remplacent les liaisons que vous définissez en appelant `Listen` ou `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="b9efb-208">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="b9efb-209">Pour plus d’informations, consultez [Introduction au Module de base ASP.NET](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="b9efb-209">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b9efb-210">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b9efb-210">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b9efb-211">Par défaut, ASP.NET Core lie à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b9efb-211">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="b9efb-212">Vous pouvez configurer des préfixes d’URL et ports pour Kestrel à écouter à l’aide de la `UseUrls` méthode d’extension, le `urls` argument de ligne de commande ou dans le système de configuration ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b9efb-212">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="b9efb-213">Pour plus d’informations sur ces méthodes, consultez [hébergement](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="b9efb-213">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="b9efb-214">Pour plus d’informations sur le fonctionne de la liaison d’URL lorsque vous utilisez IIS comme un proxy inverse, consultez [ASP.NET Core Module](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="b9efb-214">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="b9efb-215">Préfixes d’URL</span><span class="sxs-lookup"><span data-stu-id="b9efb-215">URL prefixes</span></span>

<span data-ttu-id="b9efb-216">Si vous appelez `UseUrls` ou utilisez le `urls` argument de ligne de commande ou la variable d’environnement ASPNETCORE_URLS, les préfixes d’URL peuvent être l’un des formats suivants.</span><span class="sxs-lookup"><span data-stu-id="b9efb-216">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b9efb-217">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b9efb-217">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b9efb-218">Seuls les préfixes d’URL HTTP sont valides ; Kestrel ne prend en charge SSL lorsque vous configurez les liaisons de l’URL à l’aide de `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="b9efb-218">Only HTTP URL prefixes are valid; Kestrel doesn't support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="b9efb-219">Adresse IPv4 avec le numéro de port</span><span class="sxs-lookup"><span data-stu-id="b9efb-219">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="b9efb-220">0.0.0.0 est un cas spécial qui est lié à toutes les adresses IPv4.</span><span class="sxs-lookup"><span data-stu-id="b9efb-220">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="b9efb-221">Adresse IPv6 avec le numéro de port</span><span class="sxs-lookup"><span data-stu-id="b9efb-221">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="b9efb-222">[ :] équivaut à IPv6 IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="b9efb-222">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="b9efb-223">Nom d’hôte avec le numéro de port</span><span class="sxs-lookup"><span data-stu-id="b9efb-223">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="b9efb-224">Héberger des noms, \*, et +, ne sont pas spécifiques.</span><span class="sxs-lookup"><span data-stu-id="b9efb-224">Host names, \*, and +, are not special.</span></span> <span data-ttu-id="b9efb-225">Tout ce qui n’est pas une adresse IP ou le « localhost » reconnu est lié à tous les IPv4 et IPv6 des adresses IP.</span><span class="sxs-lookup"><span data-stu-id="b9efb-225">Anything that's not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="b9efb-226">Si vous devez lier des noms d’hôte différents pour les applications ASP.NET Core différentes sur le même port, utilisez [HTTP.sys](httpsys.md) ou un serveur proxy inverse, comme IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="b9efb-226">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="b9efb-227">Nom de « Localhost » avec IP de bouclage ou de numéro de port avec le numéro de port</span><span class="sxs-lookup"><span data-stu-id="b9efb-227">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="b9efb-228">Lorsque `localhost` est spécifié, Kestrel essaie de lier aux interfaces de bouclage IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="b9efb-228">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="b9efb-229">Si le port demandé est en cours d’utilisation par un autre service sur l’interface de bouclage, Kestrel ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="b9efb-229">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="b9efb-230">Si l’interface de bouclage est indisponible pour une raison quelconque (la plupart des couramment car IPv6 n’est pas pris en charge), Kestrel consigne un avertissement.</span><span class="sxs-lookup"><span data-stu-id="b9efb-230">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b9efb-231">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b9efb-231">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="b9efb-232">Adresse IPv4 avec le numéro de port</span><span class="sxs-lookup"><span data-stu-id="b9efb-232">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="b9efb-233">0.0.0.0 est un cas spécial qui est lié à toutes les adresses IPv4.</span><span class="sxs-lookup"><span data-stu-id="b9efb-233">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="b9efb-234">Adresse IPv6 avec le numéro de port</span><span class="sxs-lookup"><span data-stu-id="b9efb-234">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="b9efb-235">[ :] équivaut à IPv6 IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="b9efb-235">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="b9efb-236">Nom d’hôte avec le numéro de port</span><span class="sxs-lookup"><span data-stu-id="b9efb-236">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="b9efb-237">Les noms, d’hôte \*, et + ne sont pas spéciales.</span><span class="sxs-lookup"><span data-stu-id="b9efb-237">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="b9efb-238">Tout ce qui n’est pas une adresse IP ou le « localhost » reconnu est liée à tous les IPv4 et IPv6 des adresses IP.</span><span class="sxs-lookup"><span data-stu-id="b9efb-238">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="b9efb-239">Si vous devez lier des noms d’hôte différents pour les applications ASP.NET Core différentes sur le même port, utilisez [WebListener](weblistener.md) ou un serveur proxy inverse, comme IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="b9efb-239">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="b9efb-240">Nom de « Localhost » avec IP de bouclage ou de numéro de port avec le numéro de port</span><span class="sxs-lookup"><span data-stu-id="b9efb-240">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="b9efb-241">Lorsque `localhost` est spécifié, Kestrel essaie de lier aux interfaces de bouclage IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="b9efb-241">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="b9efb-242">Si le port demandé est en cours d’utilisation par un autre service sur l’interface de bouclage, Kestrel ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="b9efb-242">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="b9efb-243">Si l’interface de bouclage est indisponible pour une raison quelconque (la plupart des couramment car IPv6 n’est pas pris en charge), Kestrel consigne un avertissement.</span><span class="sxs-lookup"><span data-stu-id="b9efb-243">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="b9efb-244">Socket d’UNIX</span><span class="sxs-lookup"><span data-stu-id="b9efb-244">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="b9efb-245">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="b9efb-245">**Port 0**</span></span>

<span data-ttu-id="b9efb-246">Si vous spécifiez le numéro de port 0, Kestrel lie dynamiquement un port disponible.</span><span class="sxs-lookup"><span data-stu-id="b9efb-246">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="b9efb-247">Liaison de port 0 est autorisée pour n’importe quel nom d’hôte ou l’adresse IP à l’exception de `localhost` nom.</span><span class="sxs-lookup"><span data-stu-id="b9efb-247">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="b9efb-248">L’exemple suivant montre comment déterminer le port Kestrel effectivement lié à l’exécution :</span><span class="sxs-lookup"><span data-stu-id="b9efb-248">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="b9efb-249">**Préfixes d’URL pour le protocole SSL**</span><span class="sxs-lookup"><span data-stu-id="b9efb-249">**URL prefixes for SSL**</span></span>

<span data-ttu-id="b9efb-250">Veillez à inclure les préfixes d’URL avec `https:` si vous appelez le `UseHttps` méthode d’extension, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="b9efb-250">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

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
> <span data-ttu-id="b9efb-251">HTTPS et HTTP ne peut pas être hébergé sur le même port.</span><span class="sxs-lookup"><span data-stu-id="b9efb-251">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a><span data-ttu-id="b9efb-252">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="b9efb-252">Next steps</span></span>

<span data-ttu-id="b9efb-253">Pour plus d'informations, reportez-vous aux ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="b9efb-253">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b9efb-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b9efb-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="b9efb-255">Exemple d’application pour 2.x</span><span class="sxs-lookup"><span data-stu-id="b9efb-255">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="b9efb-256">Code source de kestrel</span><span class="sxs-lookup"><span data-stu-id="b9efb-256">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b9efb-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b9efb-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="b9efb-258">Exemple d’application pour 1.x</span><span class="sxs-lookup"><span data-stu-id="b9efb-258">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="b9efb-259">Code source de kestrel</span><span class="sxs-lookup"><span data-stu-id="b9efb-259">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---
