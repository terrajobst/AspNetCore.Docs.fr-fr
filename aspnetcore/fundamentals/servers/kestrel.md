---
title: Implémentation du serveur web Kestrel dans ASP.NET Core
author: guardrex
description: Découvrez plus d’informations sur Kestrel, serveur web multiplateforme pour ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2019
uid: fundamentals/servers/kestrel
ms.openlocfilehash: b1c28f084e67d9cce74258433aa0c884878c2520
ms.sourcegitcommit: dc5b293e08336dc236de66ed1834f7ef78359531
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71011160"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="3f80e-103">Implémentation du serveur web Kestrel dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f80e-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="3f80e-104">Par [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), [Stephen Halter](https://twitter.com/halter73)et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3f80e-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), [Stephen Halter](https://twitter.com/halter73), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3f80e-105">Kestrel est un [serveur web multiplateforme pour ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="3f80e-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="3f80e-106">Kestrel est le serveur web inclus par défaut dans les modèles de projets ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f80e-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="3f80e-107">Kestrel prend en charge les scénarios suivants :</span><span class="sxs-lookup"><span data-stu-id="3f80e-107">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="3f80e-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="3f80e-108">HTTPS</span></span>
* <span data-ttu-id="3f80e-109">Mise à niveau opaque utilisée pour activer les [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="3f80e-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="3f80e-110">Sockets UNIX pour des performances élevées derrière Nginx</span><span class="sxs-lookup"><span data-stu-id="3f80e-110">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="3f80e-111">HTTP/2 (sauf sur macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="3f80e-111">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="3f80e-112">&dagger;HTTP/2 sera pris en charge sur macOS dans une prochaine version.</span><span class="sxs-lookup"><span data-stu-id="3f80e-112">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="3f80e-113">Kestrel est pris en charge sur toutes les plateformes et les versions prises en charge par .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f80e-113">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="3f80e-114">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3f80e-114">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="3f80e-115">Assistance HTTP/2</span><span class="sxs-lookup"><span data-stu-id="3f80e-115">HTTP/2 support</span></span>

<span data-ttu-id="3f80e-116">[HTTP/2](https://httpwg.org/specs/rfc7540.html) est disponible pour les applications ASP.NET Core si les conditions de base suivantes sont remplies :</span><span class="sxs-lookup"><span data-stu-id="3f80e-116">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="3f80e-117">Système d’exploitation&dagger;</span><span class="sxs-lookup"><span data-stu-id="3f80e-117">Operating system&dagger;</span></span>
  * <span data-ttu-id="3f80e-118">Windows Server 2016/Windows 10 ou version ultérieure&Dagger;</span><span class="sxs-lookup"><span data-stu-id="3f80e-118">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="3f80e-119">Linux avec OpenSSL 1.0.2 ou version ultérieure (par exemple, Ubuntu 16.04 ou version ultérieure)</span><span class="sxs-lookup"><span data-stu-id="3f80e-119">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="3f80e-120">Framework cible : .NET Core 2.2 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="3f80e-120">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="3f80e-121">Connexion [ALPN (Application-Layer Protocol Negotiation)](https://tools.ietf.org/html/rfc7301#section-3)</span><span class="sxs-lookup"><span data-stu-id="3f80e-121">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="3f80e-122">TLS 1.2 ou connexion ultérieure</span><span class="sxs-lookup"><span data-stu-id="3f80e-122">TLS 1.2 or later connection</span></span>

<span data-ttu-id="3f80e-123">&dagger;HTTP/2 sera pris en charge sur macOS dans une prochaine version.</span><span class="sxs-lookup"><span data-stu-id="3f80e-123">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="3f80e-124">&Dagger;Kestrel propose une prise en charge limitée de HTTP/2 sous Windows Server 2012 R2 et Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="3f80e-124">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="3f80e-125">La prise en charge est limitée car la liste des suites de chiffrement TLS prises en charge sur ces systèmes d’exploitation est limitée.</span><span class="sxs-lookup"><span data-stu-id="3f80e-125">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="3f80e-126">Un certificat généré à l’aide d’Elliptic Curve Digital Signature algorithme (ECDSA) peut être requis pour sécuriser les connexions TLS.</span><span class="sxs-lookup"><span data-stu-id="3f80e-126">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="3f80e-127">Si une connexion HTTP/2 est établie, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) retourne `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-127">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="3f80e-128">HTTP/2 est désactivé par défaut.</span><span class="sxs-lookup"><span data-stu-id="3f80e-128">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="3f80e-129">Pour plus d’informations sur la configuration, consultez les sections [Options Kestrel](#kestrel-options) et [ListenOptions.Protocols](#listenoptionsprotocols).</span><span class="sxs-lookup"><span data-stu-id="3f80e-129">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="3f80e-130">Quand utiliser Kestrel avec un proxy inverse ?</span><span class="sxs-lookup"><span data-stu-id="3f80e-130">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="3f80e-131">Vous pouvez utiliser Kestrel par lui-même ou avec un *serveur proxy inverse*, comme [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="3f80e-131">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="3f80e-132">Un serveur proxy inverse reçoit les requêtes HTTP en provenance du réseau et les transmet à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f80e-132">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="3f80e-133">Kestrel utilisé comme serveur web edge (accessible sur Internet) :</span><span class="sxs-lookup"><span data-stu-id="3f80e-133">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel communique directement avec Internet sans serveur proxy inverse](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="3f80e-135">Kestrel utilisé dans une configuration de proxy inverse :</span><span class="sxs-lookup"><span data-stu-id="3f80e-135">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="3f80e-137">L’une ou l’autre des configurations, avec ou sans serveur proxy inverse, est une configuration d’hébergement prise en charge.</span><span class="sxs-lookup"><span data-stu-id="3f80e-137">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="3f80e-138">Kestrel, s’il est utilisé comme serveur de périphérie sans serveur proxy inverse, ne prend pas en charge le partage de la même adresse IP et du même port entre plusieurs processus.</span><span class="sxs-lookup"><span data-stu-id="3f80e-138">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="3f80e-139">Quand Kestrel est configuré pour écouter sur un port, il gère tout le trafic pour ce port, quel que soit les en-têtes `Host` des requêtes.</span><span class="sxs-lookup"><span data-stu-id="3f80e-139">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="3f80e-140">Un proxy inverse qui peut partager des ports a la possibilité de transférer des requêtes à Kestrel sur une adresse IP et un port uniques.</span><span class="sxs-lookup"><span data-stu-id="3f80e-140">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="3f80e-141">Même si un serveur proxy inverse n’est pas nécessaire, en utiliser un peut être un bon choix.</span><span class="sxs-lookup"><span data-stu-id="3f80e-141">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="3f80e-142">Un proxy inverse :</span><span class="sxs-lookup"><span data-stu-id="3f80e-142">A reverse proxy:</span></span>

* <span data-ttu-id="3f80e-143">Peut limiter la surface publique exposée des applications qu’il héberge.</span><span class="sxs-lookup"><span data-stu-id="3f80e-143">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="3f80e-144">Fournit une couche supplémentaire de configuration et de défense.</span><span class="sxs-lookup"><span data-stu-id="3f80e-144">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="3f80e-145">Peut mieux s’intégrer à l’infrastructure existante.</span><span class="sxs-lookup"><span data-stu-id="3f80e-145">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="3f80e-146">Simplifie la configuration de l’équilibrage de charge et d’une communication sécurisée (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="3f80e-146">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="3f80e-147">Seul le serveur proxy inverse requiert un certificat X. 509 et ce serveur peut communiquer avec les serveurs de l’application sur le réseau interne à l’aide du protocole HTTP simple.</span><span class="sxs-lookup"><span data-stu-id="3f80e-147">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="3f80e-148">L’hébergement dans une configuration de proxy inverse nécessite le [filtrage d’hôte](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="3f80e-148">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="3f80e-149">Comment utiliser Kestrel dans les applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f80e-149">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="3f80e-150">Les modèles de projet ASP.NET Core utilisent Kestrel par défaut.</span><span class="sxs-lookup"><span data-stu-id="3f80e-150">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="3f80e-151">Dans *Program.cs*, le modèle de code appelle <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>, qui appelle <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="3f80e-151">In *Program.cs*, the template code calls <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="3f80e-152">Pour fournir une configuration supplémentaire après `CreateDefaultBuilder` avoir `ConfigureWebHostDefaults`appelé et `ConfigureKestrel`, utilisez :</span><span class="sxs-lookup"><span data-stu-id="3f80e-152">To provide additional configuration after calling `CreateDefaultBuilder` and `ConfigureWebHostDefaults`, use `ConfigureKestrel`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                // Set properties and call methods on options
            })
            .UseStartup<Startup>();
        });
```

<span data-ttu-id="3f80e-153">Si l’application n’appelle pas `CreateDefaultBuilder` pour configurer l’hôte, appelez <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **avant** d’appeler `ConfigureKestrel` :</span><span class="sxs-lookup"><span data-stu-id="3f80e-153">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = new HostBuilder()
        .UseContentRoot(Directory.GetCurrentDirectory())
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseKestrel(serverOptions =>
            {
                // Set properties and call methods on options
            })
            .UseIISIntegration()
            .UseStartup<Startup>();
        })
        .Build();

    host.Run();
}
```

## <a name="kestrel-options"></a><span data-ttu-id="3f80e-154">Options Kestrel</span><span class="sxs-lookup"><span data-stu-id="3f80e-154">Kestrel options</span></span>

<span data-ttu-id="3f80e-155">Le serveur web Kestrel a des options de configuration de contrainte qui sont particulièrement utiles dans les déploiements exposés à Internet.</span><span class="sxs-lookup"><span data-stu-id="3f80e-155">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="3f80e-156">Définissez des contraintes sur la propriété <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> de la classe <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="3f80e-156">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="3f80e-157">La propriété `Limits` conserve une instance de la classe <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="3f80e-157">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="3f80e-158">Les exemples suivants utilisent l’espace de noms <xref:Microsoft.AspNetCore.Server.Kestrel.Core> :</span><span class="sxs-lookup"><span data-stu-id="3f80e-158">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

### <a name="keep-alive-timeout"></a><span data-ttu-id="3f80e-159">Délai d’expiration toujours actif</span><span class="sxs-lookup"><span data-stu-id="3f80e-159">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="3f80e-160">Obtient ou définit le [délai d’expiration toujours actif](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="3f80e-160">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="3f80e-161">La valeur par défaut est de 2 minutes.</span><span class="sxs-lookup"><span data-stu-id="3f80e-161">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=19-20)]

### <a name="maximum-client-connections"></a><span data-ttu-id="3f80e-162">Nombre maximale de connexions client</span><span class="sxs-lookup"><span data-stu-id="3f80e-162">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="3f80e-163">Le nombre maximal de connexions TCP ouvertes simultanées peut être défini pour l’application entière avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="3f80e-163">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="3f80e-164">Il existe une limite distincte pour les connexions qui ont été mises à niveau à partir de HTTP ou HTTPS vers un autre protocole (par exemple, sur une demande WebSocket).</span><span class="sxs-lookup"><span data-stu-id="3f80e-164">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="3f80e-165">Une fois mise à niveau, une connexion n’est pas prise en compte dans la limite `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-165">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="3f80e-166">Le nombre maximal de connexions est illimité (null) par défaut.</span><span class="sxs-lookup"><span data-stu-id="3f80e-166">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="3f80e-167">Taille maximale du corps de la requête</span><span class="sxs-lookup"><span data-stu-id="3f80e-167">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="3f80e-168">La taille maximale par défaut du corps de la requête est de 30 000 000 octets, soit environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="3f80e-168">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="3f80e-169">Pour remplacer la limite dans une application ASP.NET Core MVC, nous vous recommandons d’utiliser l’attribut <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> sur une méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="3f80e-169">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="3f80e-170">Voici un exemple qui montre comment configurer la contrainte pour l’application sur chaque requête :</span><span class="sxs-lookup"><span data-stu-id="3f80e-170">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="3f80e-171">Remplacer le paramètre sur une demande spécifique dans l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="3f80e-171">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="3f80e-172">Une exception est levée si l’application configure la limite sur une demande après que l’application a commencé à lire la demande.</span><span class="sxs-lookup"><span data-stu-id="3f80e-172">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="3f80e-173">Il existe une propriété `IsReadOnly` qui indique si la propriété `MaxRequestBodySize` est en lecture seule ; si tel est le cas, il est trop tard pour configurer la limite.</span><span class="sxs-lookup"><span data-stu-id="3f80e-173">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="3f80e-174">Quand une application est exécutée [hors processus](xref:host-and-deploy/iis/index#out-of-process-hosting-model) derrière le [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module), la limite de taille du corps de demande de Kestrel est désactivée, car IIS définit déjà la limite.</span><span class="sxs-lookup"><span data-stu-id="3f80e-174">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="3f80e-175">Débit données minimal du corps de la requête</span><span class="sxs-lookup"><span data-stu-id="3f80e-175">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="3f80e-176">Kestrel vérifie à chaque seconde si les données arrivent au débit spécifié en octets/seconde.</span><span class="sxs-lookup"><span data-stu-id="3f80e-176">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="3f80e-177">Si le débit est inférieur au minimum, la connexion expire. La période de grâce est la durée que Kestrel accorde au client pour augmenter sa vitesse de transmission jusqu’à la limite minimale ; pendant cette période, le débit n’est pas vérifié.</span><span class="sxs-lookup"><span data-stu-id="3f80e-177">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="3f80e-178">La période de grâce permet d’éviter la suppression des connexions qui, initialement, envoient des données à une vitesse lente en raison de la lenteur du démarrage de TCP.</span><span class="sxs-lookup"><span data-stu-id="3f80e-178">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="3f80e-179">Le débit minimal par défaut est 240 octets/seconde, avec une période de grâce de 5 secondes.</span><span class="sxs-lookup"><span data-stu-id="3f80e-179">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="3f80e-180">Un débit minimal s’applique également à la réponse.</span><span class="sxs-lookup"><span data-stu-id="3f80e-180">A minimum rate also applies to the response.</span></span> <span data-ttu-id="3f80e-181">Le code pour définir les limites de demande et de réponse est identique à l’exception de `RequestBody` ou `Response` dans les noms de propriété et d’interface.</span><span class="sxs-lookup"><span data-stu-id="3f80e-181">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="3f80e-182">Voici un exemple qui montre comment configurer les débits de données minimaux dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="3f80e-182">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-11)]

<span data-ttu-id="3f80e-183">Remplacer les limites de taux minimum par demande dans l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="3f80e-183">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="3f80e-184">Le <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature>débit référencé dans l’exemple précédent n’est pas présent dans `HttpContext.Features` pour les requêtes HTTP/2, car la modification des limites de débit par requête n’est généralement pas prise en charge pour HTTP/2 (le protocole prend en charge le multiplexage de requête).</span><span class="sxs-lookup"><span data-stu-id="3f80e-184">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> referenced in the prior sample is not present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis is generally not supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="3f80e-185">Toutefois, le <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> est toujours présent `HttpContext.Features` pour les requêtes HTTP/2, car la limite de débit de lecture peut toujours être *désactivée entièrement* sur une base par demande en définissant `IHttpMinRequestBodyDataRateFeature.MinDataRate` sur `null` même pour une requête HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3f80e-185">However, the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> is still present `HttpContext.Features` for HTTP/2 requests, because the read rate limit can still be *disabled entirely* on a per-request basis by setting `IHttpMinRequestBodyDataRateFeature.MinDataRate` to `null` even for an HTTP/2 request.</span></span> <span data-ttu-id="3f80e-186">Une tentative de lecture de `IHttpMinRequestBodyDataRateFeature.MinDataRate` ou une tentative de définition sur une valeur autre que `null` entraîne une levée de `NotSupportedException` selon une requête HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3f80e-186">Attempting to read `IHttpMinRequestBodyDataRateFeature.MinDataRate` or attempting to set it to a value other than `null` will result in a `NotSupportedException` being thrown given an HTTP/2 request.</span></span>

<span data-ttu-id="3f80e-187">Les limites de débit à l’échelle du serveur configurées par le biais de `KestrelServerOptions.Limits` s’appliquent encore aux connexions HTTP/1.x et HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3f80e-187">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="3f80e-188">Délai d’expiration des en-têtes de requête</span><span class="sxs-lookup"><span data-stu-id="3f80e-188">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="3f80e-189">Obtient ou définit le temps maximal passé par le serveur à recevoir des en-têtes de requête.</span><span class="sxs-lookup"><span data-stu-id="3f80e-189">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="3f80e-190">La valeur par défaut est de 30 secondes.</span><span class="sxs-lookup"><span data-stu-id="3f80e-190">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=21-22)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="3f80e-191">Flux de données maximal par connexion</span><span class="sxs-lookup"><span data-stu-id="3f80e-191">Maximum streams per connection</span></span>

<span data-ttu-id="3f80e-192">`Http2.MaxStreamsPerConnection` limite le nombre de flux de requête simultanée par connexion HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3f80e-192">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="3f80e-193">Les flux de données excédentaires sont refusés.</span><span class="sxs-lookup"><span data-stu-id="3f80e-193">Excess streams are refused.</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
});
```

<span data-ttu-id="3f80e-194">La valeur par défaut est 100.</span><span class="sxs-lookup"><span data-stu-id="3f80e-194">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="3f80e-195">Taille de la table d’en-tête</span><span class="sxs-lookup"><span data-stu-id="3f80e-195">Header table size</span></span>

<span data-ttu-id="3f80e-196">Le décodeur HPACK décompresse les en-têtes HTTP pour les connexions HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3f80e-196">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="3f80e-197">`Http2.HeaderTableSize` limite la taille de la table de compression d’en-tête que le décodeur HPACK utilise.</span><span class="sxs-lookup"><span data-stu-id="3f80e-197">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="3f80e-198">La valeur est fournie en octets et doit être supérieure à zéro (0).</span><span class="sxs-lookup"><span data-stu-id="3f80e-198">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.HeaderTableSize = 4096;
});
```

<span data-ttu-id="3f80e-199">La valeur par défaut est 4096.</span><span class="sxs-lookup"><span data-stu-id="3f80e-199">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="3f80e-200">Taille de trame maximale</span><span class="sxs-lookup"><span data-stu-id="3f80e-200">Maximum frame size</span></span>

<span data-ttu-id="3f80e-201">`Http2.MaxFrameSize`indique la taille maximale autorisée d’une charge utile de trame de connexion HTTP/2 reçue ou envoyée par le serveur.</span><span class="sxs-lookup"><span data-stu-id="3f80e-201">`Http2.MaxFrameSize` indicates the maximum allowed size of an HTTP/2 connection frame payload received or sent by the server.</span></span> <span data-ttu-id="3f80e-202">La valeur est fournie en octets et doit être comprise entre 2^14 (16,384) et 2^24-1 (16,777,215).</span><span class="sxs-lookup"><span data-stu-id="3f80e-202">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxFrameSize = 16384;
});
```

<span data-ttu-id="3f80e-203">La valeur par défaut est 2^14 (16,384).</span><span class="sxs-lookup"><span data-stu-id="3f80e-203">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="3f80e-204">Taille maximale d’en-tête de requête</span><span class="sxs-lookup"><span data-stu-id="3f80e-204">Maximum request header size</span></span>

<span data-ttu-id="3f80e-205">`Http2.MaxRequestHeaderFieldSize` indique la taille maximale autorisée en octets des valeurs d’en-tête de requête.</span><span class="sxs-lookup"><span data-stu-id="3f80e-205">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="3f80e-206">Cette limite s’applique à la fois au nom et à la valeur dans leurs représentations compressées et non compressées.</span><span class="sxs-lookup"><span data-stu-id="3f80e-206">This limit applies to both name and value in their compressed and uncompressed representations.</span></span> <span data-ttu-id="3f80e-207">La valeur doit être supérieure à zéro (0).</span><span class="sxs-lookup"><span data-stu-id="3f80e-207">The value must be greater than zero (0).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
});
```

<span data-ttu-id="3f80e-208">La valeur par défaut est 8 192.</span><span class="sxs-lookup"><span data-stu-id="3f80e-208">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="3f80e-209">Taille de fenêtre de connexion initiale</span><span class="sxs-lookup"><span data-stu-id="3f80e-209">Initial connection window size</span></span>

<span data-ttu-id="3f80e-210">`Http2.InitialConnectionWindowSize` indique la quantité maximale de données de corps de requête, en octets, que le serveur met en mémoire tampon à un moment donné pour toutes les requêtes (flux) par connexion.</span><span class="sxs-lookup"><span data-stu-id="3f80e-210">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="3f80e-211">Les requêtes sont également limitées par `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-211">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="3f80e-212">La valeur doit être supérieure ou égale à 65 535 et inférieure à 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="3f80e-212">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
});
```

<span data-ttu-id="3f80e-213">La valeur par défaut est 128 Ko (131 072).</span><span class="sxs-lookup"><span data-stu-id="3f80e-213">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="3f80e-214">Taille de la fenêtre de flux initiale</span><span class="sxs-lookup"><span data-stu-id="3f80e-214">Initial stream window size</span></span>

<span data-ttu-id="3f80e-215">`Http2.InitialStreamWindowSize` indique la quantité maximale de données de corps de requête, en octets, que le serveur met en mémoire tampon à un moment donné par requête (flux).</span><span class="sxs-lookup"><span data-stu-id="3f80e-215">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="3f80e-216">Les requêtes sont également limitées par `Http2.InitialConnectionWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-216">Requests are also limited by `Http2.InitialConnectionWindowSize`.</span></span> <span data-ttu-id="3f80e-217">La valeur doit être supérieure ou égale à 65 535 et inférieure à 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="3f80e-217">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
});
```

<span data-ttu-id="3f80e-218">La valeur par défaut est 96 Ko (98 304).</span><span class="sxs-lookup"><span data-stu-id="3f80e-218">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="3f80e-219">E/S synchrone</span><span class="sxs-lookup"><span data-stu-id="3f80e-219">Synchronous IO</span></span>

<span data-ttu-id="3f80e-220"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> contrôle si l’e/s synchrone est autorisée pour la requête et la réponse.</span><span class="sxs-lookup"><span data-stu-id="3f80e-220"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="3f80e-221">La valeur par défaut est `false`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-221">The default value is `false`.</span></span>

> [!WARNING]
> <span data-ttu-id="3f80e-222">Un grand nombre d’opérations d’e/s synchrones de blocage peut entraîner une privation de pool de thread, ce qui fait que l’application ne répond pas.</span><span class="sxs-lookup"><span data-stu-id="3f80e-222">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="3f80e-223">Activez uniquement `AllowSynchronousIO` lors de l’utilisation d’une bibliothèque qui ne prend pas en charge l’e/s asynchrone.</span><span class="sxs-lookup"><span data-stu-id="3f80e-223">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="3f80e-224">L’exemple suivant active un e/s synchrone :</span><span class="sxs-lookup"><span data-stu-id="3f80e-224">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="3f80e-225">Pour plus d’informations sur les autres options et limites de Kestrel, consultez :</span><span class="sxs-lookup"><span data-stu-id="3f80e-225">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="3f80e-226">Configuration du point de terminaison</span><span class="sxs-lookup"><span data-stu-id="3f80e-226">Endpoint configuration</span></span>

<span data-ttu-id="3f80e-227">Par défaut, ASP.NET Core se lie à :</span><span class="sxs-lookup"><span data-stu-id="3f80e-227">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="3f80e-228">`https://localhost:5001` (quand un certificat de développement local est présent)</span><span class="sxs-lookup"><span data-stu-id="3f80e-228">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="3f80e-229">Spécifiez les URL avec :</span><span class="sxs-lookup"><span data-stu-id="3f80e-229">Specify URLs using the:</span></span>

* <span data-ttu-id="3f80e-230">La variable d’environnement `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-230">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="3f80e-231">L’argument de ligne de commande `--urls`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-231">`--urls` command-line argument.</span></span>
* <span data-ttu-id="3f80e-232">La clé de configuration d’hôte `urls`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-232">`urls` host configuration key.</span></span>
* <span data-ttu-id="3f80e-233">La méthode d’extension `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-233">`UseUrls` extension method.</span></span>

<span data-ttu-id="3f80e-234">La valeur fournie avec ces approches peut être un ou plusieurs points de terminaison HTTP et HTTPS (HTTPS si un certificat par défaut est disponible).</span><span class="sxs-lookup"><span data-stu-id="3f80e-234">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="3f80e-235">Configurez la valeur sous forme de liste délimitée par des points-virgules (par exemple `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="3f80e-235">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="3f80e-236">Pour plus d’informations sur ces approches, voir [URL de serveur](xref:fundamentals/host/web-host#server-urls) et [Remplacer la configuration](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="3f80e-236">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="3f80e-237">Un certificat de développement est créé :</span><span class="sxs-lookup"><span data-stu-id="3f80e-237">A development certificate is created:</span></span>

* <span data-ttu-id="3f80e-238">Quand le [SDK .NET Core](/dotnet/core/sdk) est installé.</span><span class="sxs-lookup"><span data-stu-id="3f80e-238">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="3f80e-239">[L’outil dev-certs](xref:aspnetcore-2.1#https) est utilisé pour créer un certificat.</span><span class="sxs-lookup"><span data-stu-id="3f80e-239">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="3f80e-240">Certains navigateurs requièrent l’octroi d’une autorisation explicite pour approuver le certificat de développement local.</span><span class="sxs-lookup"><span data-stu-id="3f80e-240">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="3f80e-241">Les modèles de projet configurent les applications à exécuter sur HTTPs par défaut et incluent [la redirection https et la prise en charge de HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="3f80e-241">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="3f80e-242">Appelez les méthodes <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> ou <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> sur <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> pour configurer les préfixes et les ports d’URL pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f80e-242">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="3f80e-243">`UseUrls`, l’argument de ligne de commande `--urls`, la clé de configuration d’hôte `urls` et la variable d’environnement `ASPNETCORE_URLS` fonctionnent également, mais ils présentent les limitations indiquées plus loin dans cette section (un certificat par défaut doit être disponible pour la configuration du point de terminaison HTTPS).</span><span class="sxs-lookup"><span data-stu-id="3f80e-243">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="3f80e-244">`KestrelServerOptions`configuré</span><span class="sxs-lookup"><span data-stu-id="3f80e-244">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="3f80e-245">ConfigureEndpointDefaults (action\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="3f80e-245">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="3f80e-246">Spécifie une `Action` de configuration à exécuter pour chaque point de terminaison spécifié.</span><span class="sxs-lookup"><span data-stu-id="3f80e-246">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="3f80e-247">Le fait d’appeler `ConfigureEndpointDefaults` plusieurs fois remplace les `Action`s précédentes par la dernière `Action` spécifiée.</span><span class="sxs-lookup"><span data-stu-id="3f80e-247">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                serverOptions.ConfigureEndpointDefaults(listenOptions =>
                {
                    // Configure endpoint defaults
                });
            })
            .UseStartup<Startup>();
        });
}
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="3f80e-248">ConfigureHttpsDefaults (action\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="3f80e-248">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="3f80e-249">Spécifie une `Action` de configuration à exécuter pour chaque point de terminaison HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3f80e-249">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="3f80e-250">Le fait d’appeler `ConfigureHttpsDefaults` plusieurs fois remplace les `Action`s précédentes par la dernière `Action` spécifiée.</span><span class="sxs-lookup"><span data-stu-id="3f80e-250">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                serverOptions.ConfigureHttpsDefaults(listenOptions =>
                {
                    // certificate is an X509Certificate2
                    listenOptions.ServerCertificate = certificate;
                });
            })
            .UseStartup<Startup>();
        });
}
```

### <a name="configureiconfiguration"></a><span data-ttu-id="3f80e-251">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="3f80e-251">Configure(IConfiguration)</span></span>

<span data-ttu-id="3f80e-252">Crée un chargeur de configuration pour configurer Kestrel, qui prend en entrée une <xref:Microsoft.Extensions.Configuration.IConfiguration>.</span><span class="sxs-lookup"><span data-stu-id="3f80e-252">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="3f80e-253">La configuration doit être limitée à la section de configuration pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f80e-253">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="3f80e-254">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="3f80e-254">ListenOptions.UseHttps</span></span>

<span data-ttu-id="3f80e-255">Configure Kestrel pour l’utilisation de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3f80e-255">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="3f80e-256">Extensions de `ListenOptions.UseHttps` :</span><span class="sxs-lookup"><span data-stu-id="3f80e-256">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="3f80e-257">`UseHttps` &ndash; Configure Kestrel pour l’utilisation de HTTPS avec le certificat par défaut.</span><span class="sxs-lookup"><span data-stu-id="3f80e-257">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="3f80e-258">Lève une exception si aucun certificat par défaut n’est configuré.</span><span class="sxs-lookup"><span data-stu-id="3f80e-258">Throws an exception if no default certificate is configured.</span></span>
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

<span data-ttu-id="3f80e-259">Paramètres de `ListenOptions.UseHttps` :</span><span class="sxs-lookup"><span data-stu-id="3f80e-259">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="3f80e-260">`filename` est le chemin et le nom d’un fichier de certificat, relatif au répertoire qui contient les fichiers de contenu de l’application.</span><span class="sxs-lookup"><span data-stu-id="3f80e-260">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="3f80e-261">`password` est le mot de passe nécessaire pour accéder aux données du certificat X.509.</span><span class="sxs-lookup"><span data-stu-id="3f80e-261">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="3f80e-262">`configureOptions` est une `Action` pour configurer les `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-262">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="3f80e-263">Retourne l'`ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-263">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="3f80e-264">`storeName` est le magasin de certificats à partir duquel charger le certificat.</span><span class="sxs-lookup"><span data-stu-id="3f80e-264">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="3f80e-265">`subject` est le nom du sujet du certificat.</span><span class="sxs-lookup"><span data-stu-id="3f80e-265">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="3f80e-266">`allowInvalid` indique si les certificats non valides doivent être considérés, comme les certificats auto-signés.</span><span class="sxs-lookup"><span data-stu-id="3f80e-266">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="3f80e-267">`location` est l’emplacement du magasin à partir duquel charger le certificat.</span><span class="sxs-lookup"><span data-stu-id="3f80e-267">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="3f80e-268">`serverCertificate` est le certificat X.509.</span><span class="sxs-lookup"><span data-stu-id="3f80e-268">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="3f80e-269">En production, HTTPS doit être explicitement configuré.</span><span class="sxs-lookup"><span data-stu-id="3f80e-269">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="3f80e-270">Au minimum, un certificat par défaut doit être fourni.</span><span class="sxs-lookup"><span data-stu-id="3f80e-270">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="3f80e-271">Configurations prises en charge décrites dans la suite :</span><span class="sxs-lookup"><span data-stu-id="3f80e-271">Supported configurations described next:</span></span>

* <span data-ttu-id="3f80e-272">Pas de configuration</span><span class="sxs-lookup"><span data-stu-id="3f80e-272">No configuration</span></span>
* <span data-ttu-id="3f80e-273">Remplacer le certificat par défaut dans la configuration</span><span class="sxs-lookup"><span data-stu-id="3f80e-273">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="3f80e-274">Changer les valeurs par défaut dans le code</span><span class="sxs-lookup"><span data-stu-id="3f80e-274">Change the defaults in code</span></span>

<span data-ttu-id="3f80e-275">*Pas de configuration*</span><span class="sxs-lookup"><span data-stu-id="3f80e-275">*No configuration*</span></span>

<span data-ttu-id="3f80e-276">Kestrel écoute sur `http://localhost:5000` et sur `https://localhost:5001` (si un certificat par défaut est disponible).</span><span class="sxs-lookup"><span data-stu-id="3f80e-276">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="3f80e-277">*Remplacer le certificat par défaut dans la configuration*</span><span class="sxs-lookup"><span data-stu-id="3f80e-277">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="3f80e-278">Par défaut, `CreateDefaultBuilder` appelle `Configure(context.Configuration.GetSection("Kestrel"))` pour charger la configuration de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f80e-278">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="3f80e-279">Un schéma de configuration des paramètres d’application HTTPS par défaut est disponible pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f80e-279">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="3f80e-280">Configurez plusieurs points de terminaison, notamment les URL et les certificats à utiliser, à partir d’un fichier sur disque ou d’un magasin de certificats.</span><span class="sxs-lookup"><span data-stu-id="3f80e-280">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="3f80e-281">Dans l’exemple de fichier *appsettings.json* suivant :</span><span class="sxs-lookup"><span data-stu-id="3f80e-281">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="3f80e-282">Définissez **AllowInvalid** sur `true` pour permettre l’utilisation de certificats non valides (par exemple des certificats auto-signés).</span><span class="sxs-lookup"><span data-stu-id="3f80e-282">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="3f80e-283">Les points de terminaison HTTPS qui ne spécifient pas de certificat (**HttpsDefaultCert** dans l’exemple qui suit) utilise comme alternative de secours le certificat défini sous **Certificats** > **Par défaut**  ou bien le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="3f80e-283">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "Http": {
        "Url": "http://localhost:5000"
      },

      "HttpsInlineCertFile": {
        "Url": "https://localhost:5001",
        "Certificate": {
          "Path": "<path to .pfx file>",
          "Password": "<certificate password>"
        }
      },

      "HttpsInlineCertStore": {
        "Url": "https://localhost:5002",
        "Certificate": {
          "Subject": "<subject; required>",
          "Store": "<certificate store; required>",
          "Location": "<location; defaults to CurrentUser>",
          "AllowInvalid": "<true or false; defaults to false>"
        }
      },

      "HttpsDefaultCert": {
        "Url": "https://localhost:5003"
      },

      "Https": {
        "Url": "https://*:5004",
        "Certificate": {
          "Path": "<path to .pfx file>",
          "Password": "<certificate password>"
        }
      }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

<span data-ttu-id="3f80e-284">Une alternative à l’utilisation de **Chemin** et de **Mot de passe** pour un nœud de certificat consiste à spécifier le certificat avec des champs du magasin de certificats.</span><span class="sxs-lookup"><span data-stu-id="3f80e-284">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="3f80e-285">Par exemple, le certificat **Certificats** > **Par défaut** peut être spécifié en tant que :</span><span class="sxs-lookup"><span data-stu-id="3f80e-285">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="3f80e-286">Notes de schéma :</span><span class="sxs-lookup"><span data-stu-id="3f80e-286">Schema notes:</span></span>

* <span data-ttu-id="3f80e-287">Les noms des points de terminaison ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="3f80e-287">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="3f80e-288">Par exemple, `HTTPS` et `Https` sont valides.</span><span class="sxs-lookup"><span data-stu-id="3f80e-288">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="3f80e-289">Le paramètre `Url` est obligatoire pour chaque point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="3f80e-289">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="3f80e-290">Le format de ce paramètre est le même que celui du paramètre de configuration `Urls` du plus haut niveau, sauf qu’il est limité à une seule valeur.</span><span class="sxs-lookup"><span data-stu-id="3f80e-290">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="3f80e-291">Ces points de terminaison remplacent ceux qui sont définis dans le paramètre de configuration `Urls` du plus haut niveau configuration, au lieu de s’y ajouter.</span><span class="sxs-lookup"><span data-stu-id="3f80e-291">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="3f80e-292">Les points de terminaison définis dans le code via `Listen` sont cumulatifs avec les points de terminaison définis dans la section de configuration.</span><span class="sxs-lookup"><span data-stu-id="3f80e-292">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="3f80e-293">La section `Certificate` est facultative.</span><span class="sxs-lookup"><span data-stu-id="3f80e-293">The `Certificate` section is optional.</span></span> <span data-ttu-id="3f80e-294">Si la section `Certificate` n’est pas spécifiée, les valeurs par défaut définies dans les scénarios précédents sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="3f80e-294">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="3f80e-295">Si aucune valeur par défaut n’est disponible, le serveur lève une exception et son démarrage échoue.</span><span class="sxs-lookup"><span data-stu-id="3f80e-295">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="3f80e-296">La section `Certificate` prend en charge les certificats **Chemin**&ndash;**Mot de passe** et **Sujet**&ndash;**Magasin**.</span><span class="sxs-lookup"><span data-stu-id="3f80e-296">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="3f80e-297">Vous pouvez définir un nombre quelconque de points de terminaison de cette façon, pour autant qu’ils ne provoquent pas de conflits de port.</span><span class="sxs-lookup"><span data-stu-id="3f80e-297">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="3f80e-298">`options.Configure(context.Configuration.GetSection("{SECTION}"))` retourne un `KestrelConfigurationLoader` avec une méthode `.Endpoint(string name, listenOptions => { })` qui peut être utilisée pour compléter les paramètres d’un point de terminaison configuré :</span><span class="sxs-lookup"><span data-stu-id="3f80e-298">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseKestrel((context, serverOptions) =>
            {
                serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
                    .Endpoint("HTTPS", listenOptions =>
                    {
                        listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
                    });
            });
        });
```

<span data-ttu-id="3f80e-299">`KestrelServerOptions.ConfigurationLoader`peut être directement accessible pour poursuivre l’itération sur le chargeur existant, tel que celui fourni par <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="3f80e-299">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="3f80e-300">La section de configuration pour chaque point de terminaison est disponible sur les options de la méthode `Endpoint`, ce qui permet de lire les paramètres personnalisés.</span><span class="sxs-lookup"><span data-stu-id="3f80e-300">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="3f80e-301">Plusieurs configurations peuvent être chargées en rappelant `options.Configure(context.Configuration.GetSection("{SECTION}"))` avec une autre section.</span><span class="sxs-lookup"><span data-stu-id="3f80e-301">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="3f80e-302">Seule la dernière configuration est utilisée, à moins que `Load` soit explicitement appelé sur les instances précédentes.</span><span class="sxs-lookup"><span data-stu-id="3f80e-302">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="3f80e-303">Le métapackage n’appelle pas `Load` : sa section de configuration par défaut peut donc être remplacée.</span><span class="sxs-lookup"><span data-stu-id="3f80e-303">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="3f80e-304">`KestrelConfigurationLoader` reflète la famille d’API `Listen` de `KestrelServerOptions` sous forme de surcharges de `Endpoint` : le code et les points de terminaison de configuration peuvent donc être configurés au même emplacement.</span><span class="sxs-lookup"><span data-stu-id="3f80e-304">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="3f80e-305">Ces surcharges n’utilisent pas de noms et consomment seulement les paramètres par défaut de la configuration.</span><span class="sxs-lookup"><span data-stu-id="3f80e-305">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="3f80e-306">*Changer les valeurs par défaut dans le code*</span><span class="sxs-lookup"><span data-stu-id="3f80e-306">*Change the defaults in code*</span></span>

<span data-ttu-id="3f80e-307">`ConfigureEndpointDefaults` et `ConfigureHttpsDefaults` peuvent être utilisés pour modifier les paramètres par défaut pour `ListenOptions` et `HttpsConnectionAdapterOptions`, notamment en remplaçant le certificat par défaut spécifié dans le scénario précédent.</span><span class="sxs-lookup"><span data-stu-id="3f80e-307">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="3f80e-308">`ConfigureEndpointDefaults` et `ConfigureHttpsDefaults` doivent être appelés avant que des points de terminaison soient configurés.</span><span class="sxs-lookup"><span data-stu-id="3f80e-308">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                serverOptions.ConfigureEndpointDefaults(listenOptions =>
                {
                    // Configure endpoint defaults
                });

                serverOptions.ConfigureHttpsDefaults(listenOptions =>
                {
                    listenOptions.SslProtocols = SslProtocols.Tls12;
                });
            });
        });
```

<span data-ttu-id="3f80e-309">*Prise en charge de SNI par Kestrel*</span><span class="sxs-lookup"><span data-stu-id="3f80e-309">*Kestrel support for SNI*</span></span>

<span data-ttu-id="3f80e-310">[L’indication du nom de serveur (SNI, Server Name Indication)](https://tools.ietf.org/html/rfc6066#section-3) peut être utilisée pour héberger plusieurs domaines sur la même adresse IP et le même port.</span><span class="sxs-lookup"><span data-stu-id="3f80e-310">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="3f80e-311">Pour que SNI fonctionne, le client envoie le nom d’hôte pour la session sécurisée au serveur pendant la négociation TLS afin que le serveur puisse fournir le certificat correct.</span><span class="sxs-lookup"><span data-stu-id="3f80e-311">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="3f80e-312">Le client utilise le certificat fourni pour la communication chiffrée avec le serveur pendant la session sécurisée qui suit la négociation TLS.</span><span class="sxs-lookup"><span data-stu-id="3f80e-312">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="3f80e-313">Kestrel prend en charge SNI via le rappel de `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-313">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="3f80e-314">Le rappel est appelé une fois par connexion pour permettre à l’application d’inspecter le nom d’hôte et de sélectionner le certificat approprié.</span><span class="sxs-lookup"><span data-stu-id="3f80e-314">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="3f80e-315">La prise en charge de SNI nécessite les points suivants :</span><span class="sxs-lookup"><span data-stu-id="3f80e-315">SNI support requires:</span></span>

* <span data-ttu-id="3f80e-316">Exécution sur la version `netcoreapp2.1` cible de .NET Framework ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="3f80e-316">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="3f80e-317">Sur `net461` ou version ultérieure, le rappel est appelé, `name` mais est toujours `null`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-317">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="3f80e-318">`name` est également `null` si le client ne fournit pas le paramètre du nom d’hôte dans la négociation TLS.</span><span class="sxs-lookup"><span data-stu-id="3f80e-318">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="3f80e-319">Tous les sites web s’exécutent sur la même instance Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f80e-319">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="3f80e-320">Kestrel ne prend pas en charge le partage d’une adresse IP et d’un port entre plusieurs instances sans un proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="3f80e-320">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                serverOptions.ListenAnyIP(5005, listenOptions =>
                {
                    listenOptions.UseHttps(httpsOptions =>
                    {
                        var localhostCert = CertificateLoader.LoadFromStoreCert(
                            "localhost", "My", StoreLocation.CurrentUser,
                            allowInvalid: true);
                        var exampleCert = CertificateLoader.LoadFromStoreCert(
                            "example.com", "My", StoreLocation.CurrentUser,
                            allowInvalid: true);
                        var subExampleCert = CertificateLoader.LoadFromStoreCert(
                            "sub.example.com", "My", StoreLocation.CurrentUser,
                            allowInvalid: true);
                        var certs = new Dictionary<string, X509Certificate2>(
                            StringComparer.OrdinalIgnoreCase);
                        certs["localhost"] = localhostCert;
                        certs["example.com"] = exampleCert;
                        certs["sub.example.com"] = subExampleCert;
    
                        httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                        {
                            if (name != null && certs.TryGetValue(name, out var cert))
                            {
                                return cert;
                            }
    
                            return exampleCert;
                        };
                    });
                });
            })
            .UseStartup<Startup>();
        });
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="3f80e-321">Lier à un socket TCP</span><span class="sxs-lookup"><span data-stu-id="3f80e-321">Bind to a TCP socket</span></span>

<span data-ttu-id="3f80e-322">La méthode <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> se lie à un socket TCP, et une expression lambda options permet la configuration d’un certificat X.509 :</span><span class="sxs-lookup"><span data-stu-id="3f80e-322">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="3f80e-323">L’exemple configure HTTPS pour un point de terminaison avec <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="3f80e-323">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="3f80e-324">Utilisez la même API afin de configurer d’autres paramètres Kestrel pour des points de terminaison spécifiques.</span><span class="sxs-lookup"><span data-stu-id="3f80e-324">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="3f80e-325">Lier à un socket Unix</span><span class="sxs-lookup"><span data-stu-id="3f80e-325">Bind to a Unix socket</span></span>

<span data-ttu-id="3f80e-326">Écoutez sur un socket Unix avec <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> pour améliorer les performances avec Nginx, comme illustré dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="3f80e-326">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="3f80e-327">Port 0</span><span class="sxs-lookup"><span data-stu-id="3f80e-327">Port 0</span></span>

<span data-ttu-id="3f80e-328">Si vous spécifiez le numéro de port `0`, Kestrel se lie dynamiquement à un port disponible.</span><span class="sxs-lookup"><span data-stu-id="3f80e-328">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="3f80e-329">L’exemple suivant montre comment déterminer le port auquel Kestrel se lie à l’exécution :</span><span class="sxs-lookup"><span data-stu-id="3f80e-329">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="3f80e-330">Quand l’application est exécutée, la sortie de la fenêtre de console indique le port dynamique où l’application peut être atteinte :</span><span class="sxs-lookup"><span data-stu-id="3f80e-330">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="3f80e-331">Limites</span><span class="sxs-lookup"><span data-stu-id="3f80e-331">Limitations</span></span>

<span data-ttu-id="3f80e-332">Configurez des points de terminaison avec les approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="3f80e-332">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="3f80e-333">Arguments de ligne de commande `--urls`</span><span class="sxs-lookup"><span data-stu-id="3f80e-333">`--urls` command-line argument</span></span>
* <span data-ttu-id="3f80e-334">La clé de configuration d’hôte `urls`</span><span class="sxs-lookup"><span data-stu-id="3f80e-334">`urls` host configuration key</span></span>
* <span data-ttu-id="3f80e-335">Variable d’environnement `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="3f80e-335">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="3f80e-336">Ces méthodes sont utiles si vous voulez que votre code fonctionne avec des serveurs autres que Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f80e-336">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="3f80e-337">Toutefois, soyez conscient des limitations suivantes :</span><span class="sxs-lookup"><span data-stu-id="3f80e-337">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="3f80e-338">HTTPS ne peut pas être utilisé avec ces approches, sauf si un certificat par défaut est fourni dans la configuration du point de terminaison HTTPS (par exemple avec la configuration de `KestrelServerOptions` ou un fichier de configuration, comme illustré plus haut dans cette rubrique).</span><span class="sxs-lookup"><span data-stu-id="3f80e-338">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="3f80e-339">Quand les deux approches `Listen` et `UseUrls` sont utilisées simultanément, les points de terminaison `Listen` remplacent les points de terminaison `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-339">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="3f80e-340">Configuration de point de terminaison IIS</span><span class="sxs-lookup"><span data-stu-id="3f80e-340">IIS endpoint configuration</span></span>

<span data-ttu-id="3f80e-341">Quand vous utilisez IIS, les liaisons d’URL pour IIS remplacent les liaisons qui sont définies par `Listen` ou par `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-341">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="3f80e-342">Pour plus d’informations, consultez la rubrique [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="3f80e-342">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="3f80e-343">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="3f80e-343">ListenOptions.Protocols</span></span>

<span data-ttu-id="3f80e-344">La propriété `Protocols` établit les protocoles HTTP (`HttpProtocols`) activés sur un point de terminaison de connexion ou pour le serveur.</span><span class="sxs-lookup"><span data-stu-id="3f80e-344">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="3f80e-345">Affectez une valeur à la propriété `Protocols` à partir de l’énumération `HttpProtocols`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-345">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="3f80e-346">Valeur enum `HttpProtocols`</span><span class="sxs-lookup"><span data-stu-id="3f80e-346">`HttpProtocols` enum value</span></span> | <span data-ttu-id="3f80e-347">Protocole de connexion autorisé</span><span class="sxs-lookup"><span data-stu-id="3f80e-347">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="3f80e-348">HTTP/1.1 uniquement.</span><span class="sxs-lookup"><span data-stu-id="3f80e-348">HTTP/1.1 only.</span></span> <span data-ttu-id="3f80e-349">Peut être utilisé avec ou sans TLS.</span><span class="sxs-lookup"><span data-stu-id="3f80e-349">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="3f80e-350">HTTP/2 uniquement.</span><span class="sxs-lookup"><span data-stu-id="3f80e-350">HTTP/2 only.</span></span> <span data-ttu-id="3f80e-351">Peut être utilisé sans TLS, uniquement si le client prend en charge un [mode de connaissance préalable (Prior Knowledge)](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="3f80e-351">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="3f80e-352">HTTP/1.1 et HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3f80e-352">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="3f80e-353">HTTP/2 nécessite que le client sélectionne HTTP/2 dans le protocole de transfert de [négociation de protocole de couche d’application (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) TLS. dans le cas contraire, la connexion par défaut est HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="3f80e-353">HTTP/2 requires the client to select HTTP/2 in the TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="3f80e-354">La valeur `ListenOptions.Protocols` par défaut d’un point `HttpProtocols.Http1AndHttp2`de terminaison est.</span><span class="sxs-lookup"><span data-stu-id="3f80e-354">The default `ListenOptions.Protocols` value for any endpoint is `HttpProtocols.Http1AndHttp2`.</span></span>

<span data-ttu-id="3f80e-355">Restrictions TLS pour HTTP/2 :</span><span class="sxs-lookup"><span data-stu-id="3f80e-355">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="3f80e-356">TLS version 1.2 ou ultérieure</span><span class="sxs-lookup"><span data-stu-id="3f80e-356">TLS version 1.2 or later</span></span>
* <span data-ttu-id="3f80e-357">Renégociation désactivée</span><span class="sxs-lookup"><span data-stu-id="3f80e-357">Renegotiation disabled</span></span>
* <span data-ttu-id="3f80e-358">Compression désactivée</span><span class="sxs-lookup"><span data-stu-id="3f80e-358">Compression disabled</span></span>
* <span data-ttu-id="3f80e-359">Tailles minimales de l’échange de clé éphémère :</span><span class="sxs-lookup"><span data-stu-id="3f80e-359">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="3f80e-360">Diffie-Hellman à courbe elliptique (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span><span class="sxs-lookup"><span data-stu-id="3f80e-360">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="3f80e-361">Diffie-Hellman à champ fini (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2 048 bits minimum</span><span class="sxs-lookup"><span data-stu-id="3f80e-361">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="3f80e-362">Suite de chiffrement non inscrite sur liste rouge</span><span class="sxs-lookup"><span data-stu-id="3f80e-362">Cipher suite not blacklisted</span></span>

<span data-ttu-id="3f80e-363">Prise en charge par défaut de `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; avec la courbe elliptique P-256 &lbrack;`FIPS186`&rbrack;.</span><span class="sxs-lookup"><span data-stu-id="3f80e-363">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="3f80e-364">L’exemple suivant autorise les connexions HTTP/1.1 et HTTP/2 sur le port 8000.</span><span class="sxs-lookup"><span data-stu-id="3f80e-364">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="3f80e-365">Les connexions sont sécurisées par TLS avec un certificat fourni :</span><span class="sxs-lookup"><span data-stu-id="3f80e-365">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="3f80e-366">Utilisez éventuellement l’intergiciel (middleware) de connexion pour filtrer les négociations TLS en fonction de la connexion pour des chiffrements spécifiques :</span><span class="sxs-lookup"><span data-stu-id="3f80e-366">Optionally use Connection Middleware to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

```csharp
// using System.Net;
// using Microsoft.AspNetCore.Connections;

.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.UseConnectionHandler<TlsFilterConnectionHandler>();
    });
});
```

```csharp
using System;
using System.Buffers;
using System.Security.Authentication;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Connections;
using Microsoft.AspNetCore.Connections.Features;

public class TlsFilterConnectionHandler : ConnectionHandler
{
    public override async Task OnConnectedAsync(ConnectionContext connection)
    {
        var tlsFeature = connection.Features.Get<ITlsHandshakeFeature>();

        // Throw NotSupportedException for any cipher algorithm that the app doesn't
        // wish to support. Alternatively, define and compare
        // ITlsHandshakeFeature.CipherAlgorithm to a list of acceptable cipher
        // suites.
        //
        // A ITlsHandshakeFeature.CipherAlgorithm of CipherAlgorithmType.Null
        // indicates that no cipher algorithm supported by Kestrel matches the
        // requested algorithm(s).
        if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
        {
            throw new NotSupportedException("Prohibited cipher: " + tlsFeature.CipherAlgorithm);
        }

        while (true)
        {
            var result = await connection.Transport.Input.ReadAsync();
            var buffer = result.Buffer;

            if (!buffer.IsEmpty)
            {
                await connection.Transport.Output.WriteAsync(buffer.ToArray());
            }
            else if (result.IsCompleted)
            {
                break;
            }

            connection.Transport.Input.AdvanceTo(buffer.End);
        }
    }
}
```

<span data-ttu-id="3f80e-367">*Définir le protocole à partir de la configuration*</span><span class="sxs-lookup"><span data-stu-id="3f80e-367">*Set the protocol from configuration*</span></span>

<span data-ttu-id="3f80e-368">Par défaut, `CreateDefaultBuilder` appelle `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` pour charger la configuration de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f80e-368">`CreateDefaultBuilder` calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="3f80e-369">L’exemple *appSettings. JSON* suivant établit http/1.1 comme protocole de connexion par défaut pour tous les points de terminaison :</span><span class="sxs-lookup"><span data-stu-id="3f80e-369">The following *appsettings.json* example establishes HTTP/1.1 as the default connection protocol for all endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1"
    }
  }
}
```

<span data-ttu-id="3f80e-370">L’exemple *appSettings. JSON* suivant établit le protocole de connexion http/1.1 pour un point de terminaison spécifique :</span><span class="sxs-lookup"><span data-stu-id="3f80e-370">The following *appsettings.json* example establishes the HTTP/1.1 connection protocol for a specific endpoint:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1"
      }
    }
  }
}
```

<span data-ttu-id="3f80e-371">Les protocoles spécifiés dans le code remplacent les valeurs définies par configuration.</span><span class="sxs-lookup"><span data-stu-id="3f80e-371">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="3f80e-372">Configuration du transport</span><span class="sxs-lookup"><span data-stu-id="3f80e-372">Transport configuration</span></span>

<span data-ttu-id="3f80e-373">Pour les projets qui requièrent l’utilisation de<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>Libuv () :</span><span class="sxs-lookup"><span data-stu-id="3f80e-373">For projects that require the use of Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span></span>

* <span data-ttu-id="3f80e-374">Ajoutez une dépendance pour le package [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) au fichier projet de l’application :</span><span class="sxs-lookup"><span data-stu-id="3f80e-374">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

   ```xml
   <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                     Version="{VERSION}" />
   ```

* <span data-ttu-id="3f80e-375">Appelez <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> sur le `IWebHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="3f80e-375">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> on the `IWebHostBuilder`:</span></span>

   ```csharp
   public class Program
   {
       public static void Main(string[] args)
       {
           CreateHostBuilder(args).Build().Run();
       }

       public static IHostBuilder CreateHostBuilder(string[] args) =>
           Host.CreateDefaultBuilder(args)
               .ConfigureWebHostDefaults(webBuilder =>
               {
                   webBuilder.UseLibuv();
                   webBuilder.UseStartup<Startup>();
               });
   }
   ```

### <a name="url-prefixes"></a><span data-ttu-id="3f80e-376">Préfixes d’URL</span><span class="sxs-lookup"><span data-stu-id="3f80e-376">URL prefixes</span></span>

<span data-ttu-id="3f80e-377">Quand vous utilisez `UseUrls`, l’argument de ligne de commande `--urls`, la clé de configuration d’hôte `urls` ou la variable d’environnement `ASPNETCORE_URLS`, les préfixes d’URL peuvent être dans un des formats suivants.</span><span class="sxs-lookup"><span data-stu-id="3f80e-377">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="3f80e-378">Seuls les préfixes d’URL HTTP sont valides.</span><span class="sxs-lookup"><span data-stu-id="3f80e-378">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="3f80e-379">Kestrel ne prend pas en charge HTTPS lors de la configuration de liaisons d’URL avec `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-379">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="3f80e-380">Adresse IPv4 avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="3f80e-380">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="3f80e-381">`0.0.0.0` est un cas spécial qui se lie à toutes les adresses IPv4.</span><span class="sxs-lookup"><span data-stu-id="3f80e-381">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="3f80e-382">Adresse IPv6 avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="3f80e-382">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="3f80e-383">`[::]` est l’équivalent IPv6 de `0.0.0.0` dans IPv4.</span><span class="sxs-lookup"><span data-stu-id="3f80e-383">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="3f80e-384">Nom d’hôte avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="3f80e-384">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="3f80e-385">Les noms d’hôte, `*` et `+` ne sont pas spéciaux.</span><span class="sxs-lookup"><span data-stu-id="3f80e-385">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="3f80e-386">Tout ce qui n’est pas reconnu comme adresse IP valide ou `localhost` se lie à toutes les adresses IP IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="3f80e-386">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="3f80e-387">Pour lier différents noms d’hôte à différentes applications ASP.NET Core sur le même port, utilisez [HTTP.sys](xref:fundamentals/servers/httpsys) ou un serveur proxy inverse, comme IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="3f80e-387">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="3f80e-388">L’hébergement dans une configuration de proxy inverse nécessite le [filtrage d’hôte](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="3f80e-388">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="3f80e-389">Nom `localhost` de l’hôte avec numéro de port ou adresse IP de bouclage avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="3f80e-389">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="3f80e-390">Quand `localhost` est spécifié, Kestrel tente de se lier aux interfaces de bouclage IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="3f80e-390">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="3f80e-391">Si le port demandé est en cours d’utilisation par un autre service sur l’une des interfaces de bouclage, Kestrel ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="3f80e-391">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="3f80e-392">Si l’une des interfaces de bouclage n’est pas disponible pour une raison quelconque (généralement du fait de la non-prise en charge d’IPv6), Kestrel journalise un avertissement.</span><span class="sxs-lookup"><span data-stu-id="3f80e-392">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="3f80e-393">Filtrage d’hôtes</span><span class="sxs-lookup"><span data-stu-id="3f80e-393">Host filtering</span></span>

<span data-ttu-id="3f80e-394">Si Kestrel prend en charge la configuration basée sur les préfixes, comme `http://example.com:5000`, il ignore en grande partie le nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="3f80e-394">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="3f80e-395">L’hôte `localhost` est un cas spécial utilisé pour la liaison à des adresses de bouclage.</span><span class="sxs-lookup"><span data-stu-id="3f80e-395">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="3f80e-396">Tout hôte autre qu’une adresse IP explicite se lie à toutes les adresses IP publiques.</span><span class="sxs-lookup"><span data-stu-id="3f80e-396">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="3f80e-397">Les en-têtes `Host` ne sont pas validés.</span><span class="sxs-lookup"><span data-stu-id="3f80e-397">`Host` headers aren't validated.</span></span>

<span data-ttu-id="3f80e-398">En guise de solution de contournement, utilisez le middleware de filtrage d’hôtes.</span><span class="sxs-lookup"><span data-stu-id="3f80e-398">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="3f80e-399">L’intergiciel (middleware) de filtrage d’hôte est fourni par le package [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) , qui est fourni implicitement pour les applications ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="3f80e-399">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is implicitly provided for ASP.NET Core apps.</span></span> <span data-ttu-id="3f80e-400">L’intergiciel (middleware) est ajouté par <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, qui appelle <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*> :</span><span class="sxs-lookup"><span data-stu-id="3f80e-400">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="3f80e-401">Le middleware de filtrage d’hôtes est désactivé par défaut.</span><span class="sxs-lookup"><span data-stu-id="3f80e-401">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="3f80e-402">Pour activer le middleware, définissez une clé `AllowedHosts` dans *appsettings.json*/*appsettings.\<nom_environnement>.json*.</span><span class="sxs-lookup"><span data-stu-id="3f80e-402">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="3f80e-403">La valeur est une liste délimitée par des points-virgules des noms d’hôte sans numéros de port :</span><span class="sxs-lookup"><span data-stu-id="3f80e-403">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="3f80e-404">*appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="3f80e-404">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="3f80e-405">Le [middleware des en-têtes transférés](xref:host-and-deploy/proxy-load-balancer) a aussi une option <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="3f80e-405">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="3f80e-406">Le middleware des en-têtes transférés et le middleware de filtrage d’hôtes ont des fonctionnalités similaires pour différents scénarios.</span><span class="sxs-lookup"><span data-stu-id="3f80e-406">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="3f80e-407">La définition d’`AllowedHosts` avec le middleware des en-têtes transférés est appropriée quand l’en-tête `Host` n’est pas conservé durant le transfert des requêtes avec un serveur proxy inverse ou un équilibreur de charge.</span><span class="sxs-lookup"><span data-stu-id="3f80e-407">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="3f80e-408">La définition d’`AllowedHosts` avec le middleware de filtrage d’hôtes est appropriée quand Kestrel est utilisé comme serveur de périphérie public ou que l’en-tête `Host` est directement transféré.</span><span class="sxs-lookup"><span data-stu-id="3f80e-408">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="3f80e-409">Pour plus d’informations sur le middleware des en-têtes transférés, consultez <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="3f80e-409">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="3f80e-410">Kestrel est un [serveur web multiplateforme pour ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="3f80e-410">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="3f80e-411">Kestrel est le serveur web inclus par défaut dans les modèles de projets ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f80e-411">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="3f80e-412">Kestrel prend en charge les scénarios suivants :</span><span class="sxs-lookup"><span data-stu-id="3f80e-412">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="3f80e-413">HTTPS</span><span class="sxs-lookup"><span data-stu-id="3f80e-413">HTTPS</span></span>
* <span data-ttu-id="3f80e-414">Mise à niveau opaque utilisée pour activer les [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="3f80e-414">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="3f80e-415">Sockets UNIX pour des performances élevées derrière Nginx</span><span class="sxs-lookup"><span data-stu-id="3f80e-415">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="3f80e-416">HTTP/2 (sauf sur macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="3f80e-416">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="3f80e-417">&dagger;HTTP/2 sera pris en charge sur macOS dans une prochaine version.</span><span class="sxs-lookup"><span data-stu-id="3f80e-417">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="3f80e-418">Kestrel est pris en charge sur toutes les plateformes et les versions prises en charge par .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f80e-418">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="3f80e-419">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3f80e-419">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="3f80e-420">Assistance HTTP/2</span><span class="sxs-lookup"><span data-stu-id="3f80e-420">HTTP/2 support</span></span>

<span data-ttu-id="3f80e-421">[HTTP/2](https://httpwg.org/specs/rfc7540.html) est disponible pour les applications ASP.NET Core si les conditions de base suivantes sont remplies :</span><span class="sxs-lookup"><span data-stu-id="3f80e-421">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="3f80e-422">Système d’exploitation&dagger;</span><span class="sxs-lookup"><span data-stu-id="3f80e-422">Operating system&dagger;</span></span>
  * <span data-ttu-id="3f80e-423">Windows Server 2016/Windows 10 ou version ultérieure&Dagger;</span><span class="sxs-lookup"><span data-stu-id="3f80e-423">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="3f80e-424">Linux avec OpenSSL 1.0.2 ou version ultérieure (par exemple, Ubuntu 16.04 ou version ultérieure)</span><span class="sxs-lookup"><span data-stu-id="3f80e-424">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="3f80e-425">Framework cible : .NET Core 2.2 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="3f80e-425">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="3f80e-426">Connexion [ALPN (Application-Layer Protocol Negotiation)](https://tools.ietf.org/html/rfc7301#section-3)</span><span class="sxs-lookup"><span data-stu-id="3f80e-426">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="3f80e-427">TLS 1.2 ou connexion ultérieure</span><span class="sxs-lookup"><span data-stu-id="3f80e-427">TLS 1.2 or later connection</span></span>

<span data-ttu-id="3f80e-428">&dagger;HTTP/2 sera pris en charge sur macOS dans une prochaine version.</span><span class="sxs-lookup"><span data-stu-id="3f80e-428">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="3f80e-429">&Dagger;Kestrel propose une prise en charge limitée de HTTP/2 sous Windows Server 2012 R2 et Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="3f80e-429">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="3f80e-430">La prise en charge est limitée car la liste des suites de chiffrement TLS prises en charge sur ces systèmes d’exploitation est limitée.</span><span class="sxs-lookup"><span data-stu-id="3f80e-430">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="3f80e-431">Un certificat généré à l’aide d’Elliptic Curve Digital Signature algorithme (ECDSA) peut être requis pour sécuriser les connexions TLS.</span><span class="sxs-lookup"><span data-stu-id="3f80e-431">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="3f80e-432">Si une connexion HTTP/2 est établie, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) retourne `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-432">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="3f80e-433">HTTP/2 est désactivé par défaut.</span><span class="sxs-lookup"><span data-stu-id="3f80e-433">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="3f80e-434">Pour plus d’informations sur la configuration, consultez les sections [Options Kestrel](#kestrel-options) et [ListenOptions.Protocols](#listenoptionsprotocols).</span><span class="sxs-lookup"><span data-stu-id="3f80e-434">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="3f80e-435">Quand utiliser Kestrel avec un proxy inverse ?</span><span class="sxs-lookup"><span data-stu-id="3f80e-435">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="3f80e-436">Vous pouvez utiliser Kestrel par lui-même ou avec un *serveur proxy inverse*, comme [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="3f80e-436">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="3f80e-437">Un serveur proxy inverse reçoit les requêtes HTTP en provenance du réseau et les transmet à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f80e-437">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="3f80e-438">Kestrel utilisé comme serveur web edge (accessible sur Internet) :</span><span class="sxs-lookup"><span data-stu-id="3f80e-438">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel communique directement avec Internet sans serveur proxy inverse](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="3f80e-440">Kestrel utilisé dans une configuration de proxy inverse :</span><span class="sxs-lookup"><span data-stu-id="3f80e-440">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="3f80e-442">L’une ou l’autre des configurations, avec ou sans serveur proxy inverse, est une configuration d’hébergement prise en charge.</span><span class="sxs-lookup"><span data-stu-id="3f80e-442">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="3f80e-443">Kestrel, s’il est utilisé comme serveur de périphérie sans serveur proxy inverse, ne prend pas en charge le partage de la même adresse IP et du même port entre plusieurs processus.</span><span class="sxs-lookup"><span data-stu-id="3f80e-443">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="3f80e-444">Quand Kestrel est configuré pour écouter sur un port, il gère tout le trafic pour ce port, quel que soit les en-têtes `Host` des requêtes.</span><span class="sxs-lookup"><span data-stu-id="3f80e-444">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="3f80e-445">Un proxy inverse qui peut partager des ports a la possibilité de transférer des requêtes à Kestrel sur une adresse IP et un port uniques.</span><span class="sxs-lookup"><span data-stu-id="3f80e-445">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="3f80e-446">Même si un serveur proxy inverse n’est pas nécessaire, en utiliser un peut être un bon choix.</span><span class="sxs-lookup"><span data-stu-id="3f80e-446">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="3f80e-447">Un proxy inverse :</span><span class="sxs-lookup"><span data-stu-id="3f80e-447">A reverse proxy:</span></span>

* <span data-ttu-id="3f80e-448">Peut limiter la surface publique exposée des applications qu’il héberge.</span><span class="sxs-lookup"><span data-stu-id="3f80e-448">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="3f80e-449">Fournit une couche supplémentaire de configuration et de défense.</span><span class="sxs-lookup"><span data-stu-id="3f80e-449">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="3f80e-450">Peut mieux s’intégrer à l’infrastructure existante.</span><span class="sxs-lookup"><span data-stu-id="3f80e-450">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="3f80e-451">Simplifie la configuration de l’équilibrage de charge et d’une communication sécurisée (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="3f80e-451">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="3f80e-452">Seul le serveur proxy inverse requiert un certificat X. 509 et ce serveur peut communiquer avec les serveurs de l’application sur le réseau interne à l’aide du protocole HTTP simple.</span><span class="sxs-lookup"><span data-stu-id="3f80e-452">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="3f80e-453">L’hébergement dans une configuration de proxy inverse nécessite le [filtrage d’hôte](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="3f80e-453">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="3f80e-454">Comment utiliser Kestrel dans les applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f80e-454">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="3f80e-455">Le package [Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) est inclus dans le sous- [package Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="3f80e-455">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="3f80e-456">Les modèles de projet ASP.NET Core utilisent Kestrel par défaut.</span><span class="sxs-lookup"><span data-stu-id="3f80e-456">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="3f80e-457">Dans *Program.cs*, le modèle de code appelle <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, qui appelle <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="3f80e-457">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="3f80e-458">Pour fournir une configuration supplémentaire après l’appel de `CreateDefaultBuilder`, utilisez `ConfigureKestrel` :</span><span class="sxs-lookup"><span data-stu-id="3f80e-458">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="3f80e-459">Si l’application n’appelle pas `CreateDefaultBuilder` pour configurer l’hôte, appelez <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **avant** d’appeler `ConfigureKestrel` :</span><span class="sxs-lookup"><span data-stu-id="3f80e-459">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = new WebHostBuilder()
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseKestrel()
        .UseIISIntegration()
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        })
        .Build();

    host.Run();
}
```

## <a name="kestrel-options"></a><span data-ttu-id="3f80e-460">Options Kestrel</span><span class="sxs-lookup"><span data-stu-id="3f80e-460">Kestrel options</span></span>

<span data-ttu-id="3f80e-461">Le serveur web Kestrel a des options de configuration de contrainte qui sont particulièrement utiles dans les déploiements exposés à Internet.</span><span class="sxs-lookup"><span data-stu-id="3f80e-461">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="3f80e-462">Définissez des contraintes sur la propriété <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> de la classe <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="3f80e-462">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="3f80e-463">La propriété `Limits` conserve une instance de la classe <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="3f80e-463">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="3f80e-464">Les exemples suivants utilisent l’espace de noms <xref:Microsoft.AspNetCore.Server.Kestrel.Core> :</span><span class="sxs-lookup"><span data-stu-id="3f80e-464">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

### <a name="keep-alive-timeout"></a><span data-ttu-id="3f80e-465">Délai d’expiration toujours actif</span><span class="sxs-lookup"><span data-stu-id="3f80e-465">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="3f80e-466">Obtient ou définit le [délai d’expiration toujours actif](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="3f80e-466">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="3f80e-467">La valeur par défaut est de 2 minutes.</span><span class="sxs-lookup"><span data-stu-id="3f80e-467">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=15)]

### <a name="maximum-client-connections"></a><span data-ttu-id="3f80e-468">Nombre maximale de connexions client</span><span class="sxs-lookup"><span data-stu-id="3f80e-468">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="3f80e-469">Le nombre maximal de connexions TCP ouvertes simultanées peut être défini pour l’application entière avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="3f80e-469">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="3f80e-470">Il existe une limite distincte pour les connexions qui ont été mises à niveau à partir de HTTP ou HTTPS vers un autre protocole (par exemple, sur une demande WebSocket).</span><span class="sxs-lookup"><span data-stu-id="3f80e-470">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="3f80e-471">Une fois mise à niveau, une connexion n’est pas prise en compte dans la limite `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-471">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="3f80e-472">Le nombre maximal de connexions est illimité (null) par défaut.</span><span class="sxs-lookup"><span data-stu-id="3f80e-472">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="3f80e-473">Taille maximale du corps de la requête</span><span class="sxs-lookup"><span data-stu-id="3f80e-473">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="3f80e-474">La taille maximale par défaut du corps de la requête est de 30 000 000 octets, soit environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="3f80e-474">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="3f80e-475">Pour remplacer la limite dans une application ASP.NET Core MVC, nous vous recommandons d’utiliser l’attribut <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> sur une méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="3f80e-475">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="3f80e-476">Voici un exemple qui montre comment configurer la contrainte pour l’application sur chaque requête :</span><span class="sxs-lookup"><span data-stu-id="3f80e-476">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="3f80e-477">Remplacer le paramètre sur une demande spécifique dans l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="3f80e-477">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="3f80e-478">Une exception est levée si l’application configure la limite sur une demande après que l’application a commencé à lire la demande.</span><span class="sxs-lookup"><span data-stu-id="3f80e-478">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="3f80e-479">Il existe une propriété `IsReadOnly` qui indique si la propriété `MaxRequestBodySize` est en lecture seule ; si tel est le cas, il est trop tard pour configurer la limite.</span><span class="sxs-lookup"><span data-stu-id="3f80e-479">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="3f80e-480">Quand une application est exécutée [hors processus](xref:host-and-deploy/iis/index#out-of-process-hosting-model) derrière le [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module), la limite de taille du corps de demande de Kestrel est désactivée, car IIS définit déjà la limite.</span><span class="sxs-lookup"><span data-stu-id="3f80e-480">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="3f80e-481">Débit données minimal du corps de la requête</span><span class="sxs-lookup"><span data-stu-id="3f80e-481">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="3f80e-482">Kestrel vérifie à chaque seconde si les données arrivent au débit spécifié en octets/seconde.</span><span class="sxs-lookup"><span data-stu-id="3f80e-482">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="3f80e-483">Si le débit est inférieur au minimum, la connexion expire. La période de grâce est la durée que Kestrel accorde au client pour augmenter sa vitesse de transmission jusqu’à la limite minimale ; pendant cette période, le débit n’est pas vérifié.</span><span class="sxs-lookup"><span data-stu-id="3f80e-483">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="3f80e-484">La période de grâce permet d’éviter la suppression des connexions qui, initialement, envoient des données à une vitesse lente en raison de la lenteur du démarrage de TCP.</span><span class="sxs-lookup"><span data-stu-id="3f80e-484">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="3f80e-485">Le débit minimal par défaut est 240 octets/seconde, avec une période de grâce de 5 secondes.</span><span class="sxs-lookup"><span data-stu-id="3f80e-485">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="3f80e-486">Un débit minimal s’applique également à la réponse.</span><span class="sxs-lookup"><span data-stu-id="3f80e-486">A minimum rate also applies to the response.</span></span> <span data-ttu-id="3f80e-487">Le code pour définir les limites de demande et de réponse est identique à l’exception de `RequestBody` ou `Response` dans les noms de propriété et d’interface.</span><span class="sxs-lookup"><span data-stu-id="3f80e-487">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="3f80e-488">Voici un exemple qui montre comment configurer les débits de données minimaux dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="3f80e-488">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="3f80e-489">Remplacer les limites de taux minimum par demande dans l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="3f80e-489">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="3f80e-490">Aucune des fonctionnalités de débit référencées dans l’exemple précédent n’est présente dans `HttpContext.Features` pour les requêtes HTTP/2, car la modification des limites de débit par requête n’est pas prise en charge pour HTTP/2 (le protocole prend en charge le multiplexage de requête).</span><span class="sxs-lookup"><span data-stu-id="3f80e-490">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="3f80e-491">Les limites de débit à l’échelle du serveur configurées par le biais de `KestrelServerOptions.Limits` s’appliquent encore aux connexions HTTP/1.x et HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3f80e-491">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="3f80e-492">Délai d’expiration des en-têtes de requête</span><span class="sxs-lookup"><span data-stu-id="3f80e-492">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="3f80e-493">Obtient ou définit le temps maximal passé par le serveur à recevoir des en-têtes de requête.</span><span class="sxs-lookup"><span data-stu-id="3f80e-493">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="3f80e-494">La valeur par défaut est de 30 secondes.</span><span class="sxs-lookup"><span data-stu-id="3f80e-494">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=16)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="3f80e-495">Flux de données maximal par connexion</span><span class="sxs-lookup"><span data-stu-id="3f80e-495">Maximum streams per connection</span></span>

<span data-ttu-id="3f80e-496">`Http2.MaxStreamsPerConnection` limite le nombre de flux de requête simultanée par connexion HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3f80e-496">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="3f80e-497">Les flux de données excédentaires sont refusés.</span><span class="sxs-lookup"><span data-stu-id="3f80e-497">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="3f80e-498">La valeur par défaut est 100.</span><span class="sxs-lookup"><span data-stu-id="3f80e-498">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="3f80e-499">Taille de la table d’en-tête</span><span class="sxs-lookup"><span data-stu-id="3f80e-499">Header table size</span></span>

<span data-ttu-id="3f80e-500">Le décodeur HPACK décompresse les en-têtes HTTP pour les connexions HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3f80e-500">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="3f80e-501">`Http2.HeaderTableSize` limite la taille de la table de compression d’en-tête que le décodeur HPACK utilise.</span><span class="sxs-lookup"><span data-stu-id="3f80e-501">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="3f80e-502">La valeur est fournie en octets et doit être supérieure à zéro (0).</span><span class="sxs-lookup"><span data-stu-id="3f80e-502">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="3f80e-503">La valeur par défaut est 4096.</span><span class="sxs-lookup"><span data-stu-id="3f80e-503">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="3f80e-504">Taille de trame maximale</span><span class="sxs-lookup"><span data-stu-id="3f80e-504">Maximum frame size</span></span>

<span data-ttu-id="3f80e-505">`Http2.MaxFrameSize` Indique la taille maximale de charge utile dans la trame de connexion HTTP/2 à recevoir.</span><span class="sxs-lookup"><span data-stu-id="3f80e-505">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="3f80e-506">La valeur est fournie en octets et doit être comprise entre 2^14 (16,384) et 2^24-1 (16,777,215).</span><span class="sxs-lookup"><span data-stu-id="3f80e-506">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="3f80e-507">La valeur par défaut est 2^14 (16,384).</span><span class="sxs-lookup"><span data-stu-id="3f80e-507">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="3f80e-508">Taille maximale d’en-tête de requête</span><span class="sxs-lookup"><span data-stu-id="3f80e-508">Maximum request header size</span></span>

<span data-ttu-id="3f80e-509">`Http2.MaxRequestHeaderFieldSize` indique la taille maximale autorisée en octets des valeurs d’en-tête de requête.</span><span class="sxs-lookup"><span data-stu-id="3f80e-509">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="3f80e-510">Cette limite s’applique au nom et à la valeur dans leurs représentations compressées et non compressées.</span><span class="sxs-lookup"><span data-stu-id="3f80e-510">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="3f80e-511">La valeur doit être supérieure à zéro (0).</span><span class="sxs-lookup"><span data-stu-id="3f80e-511">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="3f80e-512">La valeur par défaut est 8 192.</span><span class="sxs-lookup"><span data-stu-id="3f80e-512">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="3f80e-513">Taille de fenêtre de connexion initiale</span><span class="sxs-lookup"><span data-stu-id="3f80e-513">Initial connection window size</span></span>

<span data-ttu-id="3f80e-514">`Http2.InitialConnectionWindowSize` indique la quantité maximale de données de corps de requête, en octets, que le serveur met en mémoire tampon à un moment donné pour toutes les requêtes (flux) par connexion.</span><span class="sxs-lookup"><span data-stu-id="3f80e-514">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="3f80e-515">Les requêtes sont également limitées par `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-515">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="3f80e-516">La valeur doit être supérieure ou égale à 65 535 et inférieure à 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="3f80e-516">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="3f80e-517">La valeur par défaut est 128 Ko (131 072).</span><span class="sxs-lookup"><span data-stu-id="3f80e-517">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="3f80e-518">Taille de la fenêtre de flux initiale</span><span class="sxs-lookup"><span data-stu-id="3f80e-518">Initial stream window size</span></span>

<span data-ttu-id="3f80e-519">`Http2.InitialStreamWindowSize` indique la quantité maximale de données de corps de requête, en octets, que le serveur met en mémoire tampon à un moment donné par requête (flux).</span><span class="sxs-lookup"><span data-stu-id="3f80e-519">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="3f80e-520">Les requêtes sont également limitées par `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-520">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="3f80e-521">La valeur doit être supérieure ou égale à 65 535 et inférieure à 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="3f80e-521">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="3f80e-522">La valeur par défaut est 96 Ko (98 304).</span><span class="sxs-lookup"><span data-stu-id="3f80e-522">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="3f80e-523">E/S synchrone</span><span class="sxs-lookup"><span data-stu-id="3f80e-523">Synchronous IO</span></span>

<span data-ttu-id="3f80e-524"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> contrôle si l’e/s synchrone est autorisée pour la requête et la réponse.</span><span class="sxs-lookup"><span data-stu-id="3f80e-524"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="3f80e-525">La valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-525">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="3f80e-526">Un grand nombre d’opérations d’e/s synchrones de blocage peut entraîner une privation de pool de thread, ce qui fait que l’application ne répond pas.</span><span class="sxs-lookup"><span data-stu-id="3f80e-526">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="3f80e-527">Activez uniquement `AllowSynchronousIO` lors de l’utilisation d’une bibliothèque qui ne prend pas en charge l’e/s asynchrone.</span><span class="sxs-lookup"><span data-stu-id="3f80e-527">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="3f80e-528">L’exemple suivant active un e/s synchrone :</span><span class="sxs-lookup"><span data-stu-id="3f80e-528">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="3f80e-529">Pour plus d’informations sur les autres options et limites de Kestrel, consultez :</span><span class="sxs-lookup"><span data-stu-id="3f80e-529">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="3f80e-530">Configuration du point de terminaison</span><span class="sxs-lookup"><span data-stu-id="3f80e-530">Endpoint configuration</span></span>

<span data-ttu-id="3f80e-531">Par défaut, ASP.NET Core se lie à :</span><span class="sxs-lookup"><span data-stu-id="3f80e-531">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="3f80e-532">`https://localhost:5001` (quand un certificat de développement local est présent)</span><span class="sxs-lookup"><span data-stu-id="3f80e-532">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="3f80e-533">Spécifiez les URL avec :</span><span class="sxs-lookup"><span data-stu-id="3f80e-533">Specify URLs using the:</span></span>

* <span data-ttu-id="3f80e-534">La variable d’environnement `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-534">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="3f80e-535">L’argument de ligne de commande `--urls`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-535">`--urls` command-line argument.</span></span>
* <span data-ttu-id="3f80e-536">La clé de configuration d’hôte `urls`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-536">`urls` host configuration key.</span></span>
* <span data-ttu-id="3f80e-537">La méthode d’extension `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-537">`UseUrls` extension method.</span></span>

<span data-ttu-id="3f80e-538">La valeur fournie avec ces approches peut être un ou plusieurs points de terminaison HTTP et HTTPS (HTTPS si un certificat par défaut est disponible).</span><span class="sxs-lookup"><span data-stu-id="3f80e-538">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="3f80e-539">Configurez la valeur sous forme de liste délimitée par des points-virgules (par exemple `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="3f80e-539">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="3f80e-540">Pour plus d’informations sur ces approches, voir [URL de serveur](xref:fundamentals/host/web-host#server-urls) et [Remplacer la configuration](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="3f80e-540">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="3f80e-541">Un certificat de développement est créé :</span><span class="sxs-lookup"><span data-stu-id="3f80e-541">A development certificate is created:</span></span>

* <span data-ttu-id="3f80e-542">Quand le [SDK .NET Core](/dotnet/core/sdk) est installé.</span><span class="sxs-lookup"><span data-stu-id="3f80e-542">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="3f80e-543">[L’outil dev-certs](xref:aspnetcore-2.1#https) est utilisé pour créer un certificat.</span><span class="sxs-lookup"><span data-stu-id="3f80e-543">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="3f80e-544">Certains navigateurs requièrent l’octroi d’une autorisation explicite pour approuver le certificat de développement local.</span><span class="sxs-lookup"><span data-stu-id="3f80e-544">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="3f80e-545">Les modèles de projet configurent les applications à exécuter sur HTTPs par défaut et incluent [la redirection https et la prise en charge de HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="3f80e-545">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="3f80e-546">Appelez les méthodes <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> ou <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> sur <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> pour configurer les préfixes et les ports d’URL pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f80e-546">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="3f80e-547">`UseUrls`, l’argument de ligne de commande `--urls`, la clé de configuration d’hôte `urls` et la variable d’environnement `ASPNETCORE_URLS` fonctionnent également, mais ils présentent les limitations indiquées plus loin dans cette section (un certificat par défaut doit être disponible pour la configuration du point de terminaison HTTPS).</span><span class="sxs-lookup"><span data-stu-id="3f80e-547">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="3f80e-548">`KestrelServerOptions`configuré</span><span class="sxs-lookup"><span data-stu-id="3f80e-548">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="3f80e-549">ConfigureEndpointDefaults (action\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="3f80e-549">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="3f80e-550">Spécifie une `Action` de configuration à exécuter pour chaque point de terminaison spécifié.</span><span class="sxs-lookup"><span data-stu-id="3f80e-550">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="3f80e-551">Le fait d’appeler `ConfigureEndpointDefaults` plusieurs fois remplace les `Action`s précédentes par la dernière `Action` spécifiée.</span><span class="sxs-lookup"><span data-stu-id="3f80e-551">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
        });
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="3f80e-552">ConfigureHttpsDefaults (action\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="3f80e-552">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="3f80e-553">Spécifie une `Action` de configuration à exécuter pour chaque point de terminaison HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3f80e-553">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="3f80e-554">Le fait d’appeler `ConfigureHttpsDefaults` plusieurs fois remplace les `Action`s précédentes par la dernière `Action` spécifiée.</span><span class="sxs-lookup"><span data-stu-id="3f80e-554">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                // certificate is an X509Certificate2
                listenOptions.ServerCertificate = certificate;
            });
        });
```

### <a name="configureiconfiguration"></a><span data-ttu-id="3f80e-555">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="3f80e-555">Configure(IConfiguration)</span></span>

<span data-ttu-id="3f80e-556">Crée un chargeur de configuration pour configurer Kestrel, qui prend en entrée une <xref:Microsoft.Extensions.Configuration.IConfiguration>.</span><span class="sxs-lookup"><span data-stu-id="3f80e-556">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="3f80e-557">La configuration doit être limitée à la section de configuration pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f80e-557">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="3f80e-558">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="3f80e-558">ListenOptions.UseHttps</span></span>

<span data-ttu-id="3f80e-559">Configure Kestrel pour l’utilisation de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3f80e-559">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="3f80e-560">Extensions de `ListenOptions.UseHttps` :</span><span class="sxs-lookup"><span data-stu-id="3f80e-560">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="3f80e-561">`UseHttps` &ndash; Configure Kestrel pour l’utilisation de HTTPS avec le certificat par défaut.</span><span class="sxs-lookup"><span data-stu-id="3f80e-561">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="3f80e-562">Lève une exception si aucun certificat par défaut n’est configuré.</span><span class="sxs-lookup"><span data-stu-id="3f80e-562">Throws an exception if no default certificate is configured.</span></span>
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

<span data-ttu-id="3f80e-563">Paramètres de `ListenOptions.UseHttps` :</span><span class="sxs-lookup"><span data-stu-id="3f80e-563">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="3f80e-564">`filename` est le chemin et le nom d’un fichier de certificat, relatif au répertoire qui contient les fichiers de contenu de l’application.</span><span class="sxs-lookup"><span data-stu-id="3f80e-564">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="3f80e-565">`password` est le mot de passe nécessaire pour accéder aux données du certificat X.509.</span><span class="sxs-lookup"><span data-stu-id="3f80e-565">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="3f80e-566">`configureOptions` est une `Action` pour configurer les `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-566">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="3f80e-567">Retourne l'`ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-567">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="3f80e-568">`storeName` est le magasin de certificats à partir duquel charger le certificat.</span><span class="sxs-lookup"><span data-stu-id="3f80e-568">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="3f80e-569">`subject` est le nom du sujet du certificat.</span><span class="sxs-lookup"><span data-stu-id="3f80e-569">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="3f80e-570">`allowInvalid` indique si les certificats non valides doivent être considérés, comme les certificats auto-signés.</span><span class="sxs-lookup"><span data-stu-id="3f80e-570">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="3f80e-571">`location` est l’emplacement du magasin à partir duquel charger le certificat.</span><span class="sxs-lookup"><span data-stu-id="3f80e-571">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="3f80e-572">`serverCertificate` est le certificat X.509.</span><span class="sxs-lookup"><span data-stu-id="3f80e-572">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="3f80e-573">En production, HTTPS doit être explicitement configuré.</span><span class="sxs-lookup"><span data-stu-id="3f80e-573">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="3f80e-574">Au minimum, un certificat par défaut doit être fourni.</span><span class="sxs-lookup"><span data-stu-id="3f80e-574">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="3f80e-575">Configurations prises en charge décrites dans la suite :</span><span class="sxs-lookup"><span data-stu-id="3f80e-575">Supported configurations described next:</span></span>

* <span data-ttu-id="3f80e-576">Pas de configuration</span><span class="sxs-lookup"><span data-stu-id="3f80e-576">No configuration</span></span>
* <span data-ttu-id="3f80e-577">Remplacer le certificat par défaut dans la configuration</span><span class="sxs-lookup"><span data-stu-id="3f80e-577">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="3f80e-578">Changer les valeurs par défaut dans le code</span><span class="sxs-lookup"><span data-stu-id="3f80e-578">Change the defaults in code</span></span>

<span data-ttu-id="3f80e-579">*Pas de configuration*</span><span class="sxs-lookup"><span data-stu-id="3f80e-579">*No configuration*</span></span>

<span data-ttu-id="3f80e-580">Kestrel écoute sur `http://localhost:5000` et sur `https://localhost:5001` (si un certificat par défaut est disponible).</span><span class="sxs-lookup"><span data-stu-id="3f80e-580">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="3f80e-581">*Remplacer le certificat par défaut dans la configuration*</span><span class="sxs-lookup"><span data-stu-id="3f80e-581">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="3f80e-582">Par défaut, `CreateDefaultBuilder` appelle `Configure(context.Configuration.GetSection("Kestrel"))` pour charger la configuration de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f80e-582">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="3f80e-583">Un schéma de configuration des paramètres d’application HTTPS par défaut est disponible pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f80e-583">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="3f80e-584">Configurez plusieurs points de terminaison, notamment les URL et les certificats à utiliser, à partir d’un fichier sur disque ou d’un magasin de certificats.</span><span class="sxs-lookup"><span data-stu-id="3f80e-584">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="3f80e-585">Dans l’exemple de fichier *appsettings.json* suivant :</span><span class="sxs-lookup"><span data-stu-id="3f80e-585">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="3f80e-586">Définissez **AllowInvalid** sur `true` pour permettre l’utilisation de certificats non valides (par exemple des certificats auto-signés).</span><span class="sxs-lookup"><span data-stu-id="3f80e-586">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="3f80e-587">Les points de terminaison HTTPS qui ne spécifient pas de certificat (**HttpsDefaultCert** dans l’exemple qui suit) utilise comme alternative de secours le certificat défini sous **Certificats** > **Par défaut**  ou bien le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="3f80e-587">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "Http": {
        "Url": "http://localhost:5000"
      },

      "HttpsInlineCertFile": {
        "Url": "https://localhost:5001",
        "Certificate": {
          "Path": "<path to .pfx file>",
          "Password": "<certificate password>"
        }
      },

      "HttpsInlineCertStore": {
        "Url": "https://localhost:5002",
        "Certificate": {
          "Subject": "<subject; required>",
          "Store": "<certificate store; required>",
          "Location": "<location; defaults to CurrentUser>",
          "AllowInvalid": "<true or false; defaults to false>"
        }
      },

      "HttpsDefaultCert": {
        "Url": "https://localhost:5003"
      },

      "Https": {
        "Url": "https://*:5004",
        "Certificate": {
          "Path": "<path to .pfx file>",
          "Password": "<certificate password>"
        }
      }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

<span data-ttu-id="3f80e-588">Une alternative à l’utilisation de **Chemin** et de **Mot de passe** pour un nœud de certificat consiste à spécifier le certificat avec des champs du magasin de certificats.</span><span class="sxs-lookup"><span data-stu-id="3f80e-588">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="3f80e-589">Par exemple, le certificat **Certificats** > **Par défaut** peut être spécifié en tant que :</span><span class="sxs-lookup"><span data-stu-id="3f80e-589">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="3f80e-590">Notes de schéma :</span><span class="sxs-lookup"><span data-stu-id="3f80e-590">Schema notes:</span></span>

* <span data-ttu-id="3f80e-591">Les noms des points de terminaison ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="3f80e-591">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="3f80e-592">Par exemple, `HTTPS` et `Https` sont valides.</span><span class="sxs-lookup"><span data-stu-id="3f80e-592">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="3f80e-593">Le paramètre `Url` est obligatoire pour chaque point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="3f80e-593">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="3f80e-594">Le format de ce paramètre est le même que celui du paramètre de configuration `Urls` du plus haut niveau, sauf qu’il est limité à une seule valeur.</span><span class="sxs-lookup"><span data-stu-id="3f80e-594">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="3f80e-595">Ces points de terminaison remplacent ceux qui sont définis dans le paramètre de configuration `Urls` du plus haut niveau configuration, au lieu de s’y ajouter.</span><span class="sxs-lookup"><span data-stu-id="3f80e-595">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="3f80e-596">Les points de terminaison définis dans le code via `Listen` sont cumulatifs avec les points de terminaison définis dans la section de configuration.</span><span class="sxs-lookup"><span data-stu-id="3f80e-596">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="3f80e-597">La section `Certificate` est facultative.</span><span class="sxs-lookup"><span data-stu-id="3f80e-597">The `Certificate` section is optional.</span></span> <span data-ttu-id="3f80e-598">Si la section `Certificate` n’est pas spécifiée, les valeurs par défaut définies dans les scénarios précédents sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="3f80e-598">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="3f80e-599">Si aucune valeur par défaut n’est disponible, le serveur lève une exception et son démarrage échoue.</span><span class="sxs-lookup"><span data-stu-id="3f80e-599">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="3f80e-600">La section `Certificate` prend en charge les certificats **Chemin**&ndash;**Mot de passe** et **Sujet**&ndash;**Magasin**.</span><span class="sxs-lookup"><span data-stu-id="3f80e-600">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="3f80e-601">Vous pouvez définir un nombre quelconque de points de terminaison de cette façon, pour autant qu’ils ne provoquent pas de conflits de port.</span><span class="sxs-lookup"><span data-stu-id="3f80e-601">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="3f80e-602">`options.Configure(context.Configuration.GetSection("{SECTION}"))` retourne un `KestrelConfigurationLoader` avec une méthode `.Endpoint(string name, listenOptions => { })` qui peut être utilisée pour compléter les paramètres d’un point de terminaison configuré :</span><span class="sxs-lookup"><span data-stu-id="3f80e-602">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
                .Endpoint("HTTPS", listenOptions =>
                {
                    listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
                });
        });
```

<span data-ttu-id="3f80e-603">`KestrelServerOptions.ConfigurationLoader`peut être directement accessible pour poursuivre l’itération sur le chargeur existant, tel que celui fourni par <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="3f80e-603">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="3f80e-604">La section de configuration pour chaque point de terminaison est disponible sur les options de la méthode `Endpoint`, ce qui permet de lire les paramètres personnalisés.</span><span class="sxs-lookup"><span data-stu-id="3f80e-604">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="3f80e-605">Plusieurs configurations peuvent être chargées en rappelant `options.Configure(context.Configuration.GetSection("{SECTION}"))` avec une autre section.</span><span class="sxs-lookup"><span data-stu-id="3f80e-605">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="3f80e-606">Seule la dernière configuration est utilisée, à moins que `Load` soit explicitement appelé sur les instances précédentes.</span><span class="sxs-lookup"><span data-stu-id="3f80e-606">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="3f80e-607">Le métapackage n’appelle pas `Load` : sa section de configuration par défaut peut donc être remplacée.</span><span class="sxs-lookup"><span data-stu-id="3f80e-607">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="3f80e-608">`KestrelConfigurationLoader` reflète la famille d’API `Listen` de `KestrelServerOptions` sous forme de surcharges de `Endpoint` : le code et les points de terminaison de configuration peuvent donc être configurés au même emplacement.</span><span class="sxs-lookup"><span data-stu-id="3f80e-608">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="3f80e-609">Ces surcharges n’utilisent pas de noms et consomment seulement les paramètres par défaut de la configuration.</span><span class="sxs-lookup"><span data-stu-id="3f80e-609">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="3f80e-610">*Changer les valeurs par défaut dans le code*</span><span class="sxs-lookup"><span data-stu-id="3f80e-610">*Change the defaults in code*</span></span>

<span data-ttu-id="3f80e-611">`ConfigureEndpointDefaults` et `ConfigureHttpsDefaults` peuvent être utilisés pour modifier les paramètres par défaut pour `ListenOptions` et `HttpsConnectionAdapterOptions`, notamment en remplaçant le certificat par défaut spécifié dans le scénario précédent.</span><span class="sxs-lookup"><span data-stu-id="3f80e-611">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="3f80e-612">`ConfigureEndpointDefaults` et `ConfigureHttpsDefaults` doivent être appelés avant que des points de terminaison soient configurés.</span><span class="sxs-lookup"><span data-stu-id="3f80e-612">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
            
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                listenOptions.SslProtocols = SslProtocols.Tls12;
            });
        });
```

<span data-ttu-id="3f80e-613">*Prise en charge de SNI par Kestrel*</span><span class="sxs-lookup"><span data-stu-id="3f80e-613">*Kestrel support for SNI*</span></span>

<span data-ttu-id="3f80e-614">[L’indication du nom de serveur (SNI, Server Name Indication)](https://tools.ietf.org/html/rfc6066#section-3) peut être utilisée pour héberger plusieurs domaines sur la même adresse IP et le même port.</span><span class="sxs-lookup"><span data-stu-id="3f80e-614">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="3f80e-615">Pour que SNI fonctionne, le client envoie le nom d’hôte pour la session sécurisée au serveur pendant la négociation TLS afin que le serveur puisse fournir le certificat correct.</span><span class="sxs-lookup"><span data-stu-id="3f80e-615">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="3f80e-616">Le client utilise le certificat fourni pour la communication chiffrée avec le serveur pendant la session sécurisée qui suit la négociation TLS.</span><span class="sxs-lookup"><span data-stu-id="3f80e-616">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="3f80e-617">Kestrel prend en charge SNI via le rappel de `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-617">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="3f80e-618">Le rappel est appelé une fois par connexion pour permettre à l’application d’inspecter le nom d’hôte et de sélectionner le certificat approprié.</span><span class="sxs-lookup"><span data-stu-id="3f80e-618">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="3f80e-619">La prise en charge de SNI nécessite les points suivants :</span><span class="sxs-lookup"><span data-stu-id="3f80e-619">SNI support requires:</span></span>

* <span data-ttu-id="3f80e-620">Exécution sur la version `netcoreapp2.1` cible de .NET Framework ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="3f80e-620">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="3f80e-621">Sur `net461` ou version ultérieure, le rappel est appelé, `name` mais est toujours `null`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-621">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="3f80e-622">`name` est également `null` si le client ne fournit pas le paramètre du nom d’hôte dans la négociation TLS.</span><span class="sxs-lookup"><span data-stu-id="3f80e-622">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="3f80e-623">Tous les sites web s’exécutent sur la même instance Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f80e-623">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="3f80e-624">Kestrel ne prend pas en charge le partage d’une adresse IP et d’un port entre plusieurs instances sans un proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="3f80e-624">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ListenAnyIP(5005, listenOptions =>
            {
                listenOptions.UseHttps(httpsOptions =>
                {
                    var localhostCert = CertificateLoader.LoadFromStoreCert(
                        "localhost", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var exampleCert = CertificateLoader.LoadFromStoreCert(
                        "example.com", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var subExampleCert = CertificateLoader.LoadFromStoreCert(
                        "sub.example.com", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
                    certs["localhost"] = localhostCert;
                    certs["example.com"] = exampleCert;
                    certs["sub.example.com"] = subExampleCert;

                    httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                    {
                        if (name != null && certs.TryGetValue(name, out var cert))
                        {
                            return cert;
                        }

                        return exampleCert;
                    };
                });
            });
        });
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="3f80e-625">Lier à un socket TCP</span><span class="sxs-lookup"><span data-stu-id="3f80e-625">Bind to a TCP socket</span></span>

<span data-ttu-id="3f80e-626">La méthode <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> se lie à un socket TCP, et une expression lambda options permet la configuration d’un certificat X.509 :</span><span class="sxs-lookup"><span data-stu-id="3f80e-626">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="3f80e-627">L’exemple configure HTTPS pour un point de terminaison avec <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="3f80e-627">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="3f80e-628">Utilisez la même API afin de configurer d’autres paramètres Kestrel pour des points de terminaison spécifiques.</span><span class="sxs-lookup"><span data-stu-id="3f80e-628">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="3f80e-629">Lier à un socket Unix</span><span class="sxs-lookup"><span data-stu-id="3f80e-629">Bind to a Unix socket</span></span>

<span data-ttu-id="3f80e-630">Écoutez sur un socket Unix avec <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> pour améliorer les performances avec Nginx, comme illustré dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="3f80e-630">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="3f80e-631">Port 0</span><span class="sxs-lookup"><span data-stu-id="3f80e-631">Port 0</span></span>

<span data-ttu-id="3f80e-632">Si vous spécifiez le numéro de port `0`, Kestrel se lie dynamiquement à un port disponible.</span><span class="sxs-lookup"><span data-stu-id="3f80e-632">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="3f80e-633">L’exemple suivant montre comment déterminer le port auquel Kestrel se lie à l’exécution :</span><span class="sxs-lookup"><span data-stu-id="3f80e-633">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="3f80e-634">Quand l’application est exécutée, la sortie de la fenêtre de console indique le port dynamique où l’application peut être atteinte :</span><span class="sxs-lookup"><span data-stu-id="3f80e-634">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="3f80e-635">Limites</span><span class="sxs-lookup"><span data-stu-id="3f80e-635">Limitations</span></span>

<span data-ttu-id="3f80e-636">Configurez des points de terminaison avec les approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="3f80e-636">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="3f80e-637">Arguments de ligne de commande `--urls`</span><span class="sxs-lookup"><span data-stu-id="3f80e-637">`--urls` command-line argument</span></span>
* <span data-ttu-id="3f80e-638">La clé de configuration d’hôte `urls`</span><span class="sxs-lookup"><span data-stu-id="3f80e-638">`urls` host configuration key</span></span>
* <span data-ttu-id="3f80e-639">Variable d’environnement `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="3f80e-639">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="3f80e-640">Ces méthodes sont utiles si vous voulez que votre code fonctionne avec des serveurs autres que Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f80e-640">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="3f80e-641">Toutefois, soyez conscient des limitations suivantes :</span><span class="sxs-lookup"><span data-stu-id="3f80e-641">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="3f80e-642">HTTPS ne peut pas être utilisé avec ces approches, sauf si un certificat par défaut est fourni dans la configuration du point de terminaison HTTPS (par exemple avec la configuration de `KestrelServerOptions` ou un fichier de configuration, comme illustré plus haut dans cette rubrique).</span><span class="sxs-lookup"><span data-stu-id="3f80e-642">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="3f80e-643">Quand les deux approches `Listen` et `UseUrls` sont utilisées simultanément, les points de terminaison `Listen` remplacent les points de terminaison `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-643">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="3f80e-644">Configuration de point de terminaison IIS</span><span class="sxs-lookup"><span data-stu-id="3f80e-644">IIS endpoint configuration</span></span>

<span data-ttu-id="3f80e-645">Quand vous utilisez IIS, les liaisons d’URL pour IIS remplacent les liaisons qui sont définies par `Listen` ou par `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-645">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="3f80e-646">Pour plus d’informations, consultez la rubrique [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="3f80e-646">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="3f80e-647">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="3f80e-647">ListenOptions.Protocols</span></span>

<span data-ttu-id="3f80e-648">La propriété `Protocols` établit les protocoles HTTP (`HttpProtocols`) activés sur un point de terminaison de connexion ou pour le serveur.</span><span class="sxs-lookup"><span data-stu-id="3f80e-648">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="3f80e-649">Affectez une valeur à la propriété `Protocols` à partir de l’énumération `HttpProtocols`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-649">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="3f80e-650">Valeur enum `HttpProtocols`</span><span class="sxs-lookup"><span data-stu-id="3f80e-650">`HttpProtocols` enum value</span></span> | <span data-ttu-id="3f80e-651">Protocole de connexion autorisé</span><span class="sxs-lookup"><span data-stu-id="3f80e-651">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="3f80e-652">HTTP/1.1 uniquement.</span><span class="sxs-lookup"><span data-stu-id="3f80e-652">HTTP/1.1 only.</span></span> <span data-ttu-id="3f80e-653">Peut être utilisé avec ou sans TLS.</span><span class="sxs-lookup"><span data-stu-id="3f80e-653">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="3f80e-654">HTTP/2 uniquement.</span><span class="sxs-lookup"><span data-stu-id="3f80e-654">HTTP/2 only.</span></span> <span data-ttu-id="3f80e-655">Peut être utilisé sans TLS, uniquement si le client prend en charge un [mode de connaissance préalable (Prior Knowledge)](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="3f80e-655">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="3f80e-656">HTTP/1.1 et HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3f80e-656">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="3f80e-657">HTTP/2 requiert une connexion TLS et la [négociation de protocole de couche application (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) ; dans le cas contraire, la connexion par défaut est HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="3f80e-657">HTTP/2 requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="3f80e-658">Le protocole par défaut est HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="3f80e-658">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="3f80e-659">Restrictions TLS pour HTTP/2 :</span><span class="sxs-lookup"><span data-stu-id="3f80e-659">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="3f80e-660">TLS version 1.2 ou ultérieure</span><span class="sxs-lookup"><span data-stu-id="3f80e-660">TLS version 1.2 or later</span></span>
* <span data-ttu-id="3f80e-661">Renégociation désactivée</span><span class="sxs-lookup"><span data-stu-id="3f80e-661">Renegotiation disabled</span></span>
* <span data-ttu-id="3f80e-662">Compression désactivée</span><span class="sxs-lookup"><span data-stu-id="3f80e-662">Compression disabled</span></span>
* <span data-ttu-id="3f80e-663">Tailles minimales de l’échange de clé éphémère :</span><span class="sxs-lookup"><span data-stu-id="3f80e-663">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="3f80e-664">Diffie-Hellman à courbe elliptique (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span><span class="sxs-lookup"><span data-stu-id="3f80e-664">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="3f80e-665">Diffie-Hellman à champ fini (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2 048 bits minimum</span><span class="sxs-lookup"><span data-stu-id="3f80e-665">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="3f80e-666">Suite de chiffrement non inscrite sur liste rouge</span><span class="sxs-lookup"><span data-stu-id="3f80e-666">Cipher suite not blacklisted</span></span>

<span data-ttu-id="3f80e-667">Prise en charge par défaut de `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; avec la courbe elliptique P-256 &lbrack;`FIPS186`&rbrack;.</span><span class="sxs-lookup"><span data-stu-id="3f80e-667">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="3f80e-668">L’exemple suivant autorise les connexions HTTP/1.1 et HTTP/2 sur le port 8000.</span><span class="sxs-lookup"><span data-stu-id="3f80e-668">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="3f80e-669">Les connexions sont sécurisées par TLS avec un certificat fourni :</span><span class="sxs-lookup"><span data-stu-id="3f80e-669">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
.ConfigureKestrel((context, serverOptions) =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="3f80e-670">Le cas échéant, créez une implémentation `IConnectionAdapter` pour filtrer les négociations TLS par connexion pour des chiffrements spécifiques :</span><span class="sxs-lookup"><span data-stu-id="3f80e-670">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

```csharp
.ConfigureKestrel((context, serverOptions) =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.ConnectionAdapters.Add(new TlsFilterAdapter());
    });
});
```

```csharp
private class TlsFilterAdapter : IConnectionAdapter
{
    public bool IsHttps => false;

    public Task<IAdaptedConnection> OnConnectionAsync(ConnectionAdapterContext context)
    {
        var tlsFeature = context.Features.Get<ITlsHandshakeFeature>();

        // Throw NotSupportedException for any cipher algorithm that the app doesn't
        // wish to support. Alternatively, define and compare
        // ITlsHandshakeFeature.CipherAlgorithm to a list of acceptable cipher
        // suites.
        //
        // A ITlsHandshakeFeature.CipherAlgorithm of CipherAlgorithmType.Null
        // indicates that no cipher algorithm supported by Kestrel matches the
        // requested algorithm(s).
        if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
        {
            throw new NotSupportedException("Prohibited cipher: " + tlsFeature.CipherAlgorithm);
        }

        return Task.FromResult<IAdaptedConnection>(new AdaptedConnection(context.ConnectionStream));
    }

    private class AdaptedConnection : IAdaptedConnection
    {
        public AdaptedConnection(Stream adaptedStream)
        {
            ConnectionStream = adaptedStream;
        }

        public Stream ConnectionStream { get; }

        public void Dispose()
        {
        }
    }
}
```

<span data-ttu-id="3f80e-671">*Définir le protocole à partir de la configuration*</span><span class="sxs-lookup"><span data-stu-id="3f80e-671">*Set the protocol from configuration*</span></span>

<span data-ttu-id="3f80e-672">Par défaut, <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> appelle `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` pour charger la configuration de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f80e-672"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="3f80e-673">Dans l’exemple *appsettings.json* suivant, un protocole de connexion par défaut (HTTP/1.1 et HTTP/2) est établi pour l’ensemble des points de terminaison de Kestrel :</span><span class="sxs-lookup"><span data-stu-id="3f80e-673">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="3f80e-674">L’exemple de fichier de configuration suivant établit un protocole de connexion pour un point de terminaison spécifique :</span><span class="sxs-lookup"><span data-stu-id="3f80e-674">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1AndHttp2"
      }
    }
  }
}
```

<span data-ttu-id="3f80e-675">Les protocoles spécifiés dans le code remplacent les valeurs définies par configuration.</span><span class="sxs-lookup"><span data-stu-id="3f80e-675">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="3f80e-676">Configuration du transport</span><span class="sxs-lookup"><span data-stu-id="3f80e-676">Transport configuration</span></span>

<span data-ttu-id="3f80e-677">Dans ASP.NET Core 2.1, le transport par défaut de Kestrel n’est plus basé sur Libuv, mais sur des sockets managés.</span><span class="sxs-lookup"><span data-stu-id="3f80e-677">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="3f80e-678">Il s’agit d’un changement cassant pour les applications ASP.NET Core 2.0 mises à niveau vers la version 2.1 qui appellent <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> et qui dépendent d’un des packages suivants :</span><span class="sxs-lookup"><span data-stu-id="3f80e-678">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="3f80e-679">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (référence de package directe)</span><span class="sxs-lookup"><span data-stu-id="3f80e-679">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="3f80e-680">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="3f80e-680">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="3f80e-681">Pour les projets qui requièrent l’utilisation de Libuv :</span><span class="sxs-lookup"><span data-stu-id="3f80e-681">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="3f80e-682">Ajoutez une dépendance pour le package [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) au fichier projet de l’application :</span><span class="sxs-lookup"><span data-stu-id="3f80e-682">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="3f80e-683">Appelez <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> :</span><span class="sxs-lookup"><span data-stu-id="3f80e-683">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

  ```csharp
  public class Program
  {
      public static void Main(string[] args)
      {
          CreateWebHostBuilder(args).Build().Run();
      }

      public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
          WebHost.CreateDefaultBuilder(args)
              .UseLibuv()
              .UseStartup<Startup>();
  }
  ```

### <a name="url-prefixes"></a><span data-ttu-id="3f80e-684">Préfixes d’URL</span><span class="sxs-lookup"><span data-stu-id="3f80e-684">URL prefixes</span></span>

<span data-ttu-id="3f80e-685">Quand vous utilisez `UseUrls`, l’argument de ligne de commande `--urls`, la clé de configuration d’hôte `urls` ou la variable d’environnement `ASPNETCORE_URLS`, les préfixes d’URL peuvent être dans un des formats suivants.</span><span class="sxs-lookup"><span data-stu-id="3f80e-685">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="3f80e-686">Seuls les préfixes d’URL HTTP sont valides.</span><span class="sxs-lookup"><span data-stu-id="3f80e-686">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="3f80e-687">Kestrel ne prend pas en charge HTTPS lors de la configuration de liaisons d’URL avec `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-687">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="3f80e-688">Adresse IPv4 avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="3f80e-688">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="3f80e-689">`0.0.0.0` est un cas spécial qui se lie à toutes les adresses IPv4.</span><span class="sxs-lookup"><span data-stu-id="3f80e-689">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="3f80e-690">Adresse IPv6 avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="3f80e-690">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="3f80e-691">`[::]` est l’équivalent IPv6 de `0.0.0.0` dans IPv4.</span><span class="sxs-lookup"><span data-stu-id="3f80e-691">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="3f80e-692">Nom d’hôte avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="3f80e-692">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="3f80e-693">Les noms d’hôte, `*` et `+` ne sont pas spéciaux.</span><span class="sxs-lookup"><span data-stu-id="3f80e-693">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="3f80e-694">Tout ce qui n’est pas reconnu comme adresse IP valide ou `localhost` se lie à toutes les adresses IP IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="3f80e-694">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="3f80e-695">Pour lier différents noms d’hôte à différentes applications ASP.NET Core sur le même port, utilisez [HTTP.sys](xref:fundamentals/servers/httpsys) ou un serveur proxy inverse, comme IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="3f80e-695">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="3f80e-696">L’hébergement dans une configuration de proxy inverse nécessite le [filtrage d’hôte](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="3f80e-696">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="3f80e-697">Nom `localhost` de l’hôte avec numéro de port ou adresse IP de bouclage avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="3f80e-697">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="3f80e-698">Quand `localhost` est spécifié, Kestrel tente de se lier aux interfaces de bouclage IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="3f80e-698">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="3f80e-699">Si le port demandé est en cours d’utilisation par un autre service sur l’une des interfaces de bouclage, Kestrel ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="3f80e-699">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="3f80e-700">Si l’une des interfaces de bouclage n’est pas disponible pour une raison quelconque (généralement du fait de la non-prise en charge d’IPv6), Kestrel journalise un avertissement.</span><span class="sxs-lookup"><span data-stu-id="3f80e-700">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="3f80e-701">Filtrage d’hôtes</span><span class="sxs-lookup"><span data-stu-id="3f80e-701">Host filtering</span></span>

<span data-ttu-id="3f80e-702">Si Kestrel prend en charge la configuration basée sur les préfixes, comme `http://example.com:5000`, il ignore en grande partie le nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="3f80e-702">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="3f80e-703">L’hôte `localhost` est un cas spécial utilisé pour la liaison à des adresses de bouclage.</span><span class="sxs-lookup"><span data-stu-id="3f80e-703">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="3f80e-704">Tout hôte autre qu’une adresse IP explicite se lie à toutes les adresses IP publiques.</span><span class="sxs-lookup"><span data-stu-id="3f80e-704">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="3f80e-705">Les en-têtes `Host` ne sont pas validés.</span><span class="sxs-lookup"><span data-stu-id="3f80e-705">`Host` headers aren't validated.</span></span>

<span data-ttu-id="3f80e-706">En guise de solution de contournement, utilisez le middleware de filtrage d’hôtes.</span><span class="sxs-lookup"><span data-stu-id="3f80e-706">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="3f80e-707">L’intergiciel (middleware) de filtrage d’hôte est fourni par le package [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) , qui est inclus dans le de [Microsoft. AspNetCore. App](xref:fundamentals/metapackage-app) (ASP.net Core 2,1 ou 2,2).</span><span class="sxs-lookup"><span data-stu-id="3f80e-707">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="3f80e-708">L’intergiciel (middleware) est ajouté par <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, qui appelle <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*> :</span><span class="sxs-lookup"><span data-stu-id="3f80e-708">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="3f80e-709">Le middleware de filtrage d’hôtes est désactivé par défaut.</span><span class="sxs-lookup"><span data-stu-id="3f80e-709">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="3f80e-710">Pour activer le middleware, définissez une clé `AllowedHosts` dans *appsettings.json*/*appsettings.\<nom_environnement>.json*.</span><span class="sxs-lookup"><span data-stu-id="3f80e-710">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="3f80e-711">La valeur est une liste délimitée par des points-virgules des noms d’hôte sans numéros de port :</span><span class="sxs-lookup"><span data-stu-id="3f80e-711">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="3f80e-712">*appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="3f80e-712">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="3f80e-713">Le [middleware des en-têtes transférés](xref:host-and-deploy/proxy-load-balancer) a aussi une option <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="3f80e-713">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="3f80e-714">Le middleware des en-têtes transférés et le middleware de filtrage d’hôtes ont des fonctionnalités similaires pour différents scénarios.</span><span class="sxs-lookup"><span data-stu-id="3f80e-714">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="3f80e-715">La définition d’`AllowedHosts` avec le middleware des en-têtes transférés est appropriée quand l’en-tête `Host` n’est pas conservé durant le transfert des requêtes avec un serveur proxy inverse ou un équilibreur de charge.</span><span class="sxs-lookup"><span data-stu-id="3f80e-715">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="3f80e-716">La définition d’`AllowedHosts` avec le middleware de filtrage d’hôtes est appropriée quand Kestrel est utilisé comme serveur de périphérie public ou que l’en-tête `Host` est directement transféré.</span><span class="sxs-lookup"><span data-stu-id="3f80e-716">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="3f80e-717">Pour plus d’informations sur le middleware des en-têtes transférés, consultez <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="3f80e-717">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="3f80e-718">Kestrel est un [serveur web multiplateforme pour ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="3f80e-718">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="3f80e-719">Kestrel est le serveur web inclus par défaut dans les modèles de projets ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f80e-719">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="3f80e-720">Kestrel prend en charge les scénarios suivants :</span><span class="sxs-lookup"><span data-stu-id="3f80e-720">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="3f80e-721">HTTPS</span><span class="sxs-lookup"><span data-stu-id="3f80e-721">HTTPS</span></span>
* <span data-ttu-id="3f80e-722">Mise à niveau opaque utilisée pour activer les [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="3f80e-722">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="3f80e-723">Sockets UNIX pour des performances élevées derrière Nginx</span><span class="sxs-lookup"><span data-stu-id="3f80e-723">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="3f80e-724">Kestrel est pris en charge sur toutes les plateformes et les versions prises en charge par .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f80e-724">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="3f80e-725">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3f80e-725">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="3f80e-726">Quand utiliser Kestrel avec un proxy inverse ?</span><span class="sxs-lookup"><span data-stu-id="3f80e-726">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="3f80e-727">Vous pouvez utiliser Kestrel par lui-même ou avec un *serveur proxy inverse*, comme [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="3f80e-727">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="3f80e-728">Un serveur proxy inverse reçoit les requêtes HTTP en provenance du réseau et les transmet à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f80e-728">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="3f80e-729">Kestrel utilisé comme serveur web edge (accessible sur Internet) :</span><span class="sxs-lookup"><span data-stu-id="3f80e-729">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel communique directement avec Internet sans serveur proxy inverse](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="3f80e-731">Kestrel utilisé dans une configuration de proxy inverse :</span><span class="sxs-lookup"><span data-stu-id="3f80e-731">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="3f80e-733">L’une ou l’autre des configurations, avec ou sans serveur proxy inverse, est une configuration d’hébergement prise en charge.</span><span class="sxs-lookup"><span data-stu-id="3f80e-733">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="3f80e-734">Kestrel, s’il est utilisé comme serveur de périphérie sans serveur proxy inverse, ne prend pas en charge le partage de la même adresse IP et du même port entre plusieurs processus.</span><span class="sxs-lookup"><span data-stu-id="3f80e-734">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="3f80e-735">Quand Kestrel est configuré pour écouter sur un port, il gère tout le trafic pour ce port, quel que soit les en-têtes `Host` des requêtes.</span><span class="sxs-lookup"><span data-stu-id="3f80e-735">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="3f80e-736">Un proxy inverse qui peut partager des ports a la possibilité de transférer des requêtes à Kestrel sur une adresse IP et un port uniques.</span><span class="sxs-lookup"><span data-stu-id="3f80e-736">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="3f80e-737">Même si un serveur proxy inverse n’est pas nécessaire, en utiliser un peut être un bon choix.</span><span class="sxs-lookup"><span data-stu-id="3f80e-737">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="3f80e-738">Un proxy inverse :</span><span class="sxs-lookup"><span data-stu-id="3f80e-738">A reverse proxy:</span></span>

* <span data-ttu-id="3f80e-739">Peut limiter la surface publique exposée des applications qu’il héberge.</span><span class="sxs-lookup"><span data-stu-id="3f80e-739">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="3f80e-740">Fournit une couche supplémentaire de configuration et de défense.</span><span class="sxs-lookup"><span data-stu-id="3f80e-740">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="3f80e-741">Peut mieux s’intégrer à l’infrastructure existante.</span><span class="sxs-lookup"><span data-stu-id="3f80e-741">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="3f80e-742">Simplifie la configuration de l’équilibrage de charge et d’une communication sécurisée (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="3f80e-742">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="3f80e-743">Seul le serveur proxy inverse requiert un certificat X. 509 et ce serveur peut communiquer avec les serveurs de l’application sur le réseau interne à l’aide du protocole HTTP simple.</span><span class="sxs-lookup"><span data-stu-id="3f80e-743">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="3f80e-744">L’hébergement dans une configuration de proxy inverse nécessite le [filtrage d’hôte](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="3f80e-744">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="3f80e-745">Comment utiliser Kestrel dans les applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f80e-745">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="3f80e-746">Le package [Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) est inclus dans le sous- [package Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="3f80e-746">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="3f80e-747">Les modèles de projet ASP.NET Core utilisent Kestrel par défaut.</span><span class="sxs-lookup"><span data-stu-id="3f80e-747">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="3f80e-748">Dans *Program.cs*, le modèle de code appelle <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, qui appelle <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="3f80e-748">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

<span data-ttu-id="3f80e-749">Pour fournir une configuration supplémentaire après l’appel de `CreateDefaultBuilder`, appelez <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> :</span><span class="sxs-lookup"><span data-stu-id="3f80e-749">To provide additional configuration after calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            // Set properties and call methods on serverOptions
        });
```

## <a name="kestrel-options"></a><span data-ttu-id="3f80e-750">Options Kestrel</span><span class="sxs-lookup"><span data-stu-id="3f80e-750">Kestrel options</span></span>

<span data-ttu-id="3f80e-751">Le serveur web Kestrel a des options de configuration de contrainte qui sont particulièrement utiles dans les déploiements exposés à Internet.</span><span class="sxs-lookup"><span data-stu-id="3f80e-751">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="3f80e-752">Définissez des contraintes sur la propriété <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> de la classe <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="3f80e-752">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="3f80e-753">La propriété `Limits` conserve une instance de la classe <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="3f80e-753">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="3f80e-754">Les exemples suivants utilisent l’espace de noms <xref:Microsoft.AspNetCore.Server.Kestrel.Core> :</span><span class="sxs-lookup"><span data-stu-id="3f80e-754">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

### <a name="keep-alive-timeout"></a><span data-ttu-id="3f80e-755">Délai d’expiration toujours actif</span><span class="sxs-lookup"><span data-stu-id="3f80e-755">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="3f80e-756">Obtient ou définit le [délai d’expiration toujours actif](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="3f80e-756">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="3f80e-757">La valeur par défaut est de 2 minutes.</span><span class="sxs-lookup"><span data-stu-id="3f80e-757">Defaults to 2 minutes.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
        });
```

### <a name="maximum-client-connections"></a><span data-ttu-id="3f80e-758">Nombre maximale de connexions client</span><span class="sxs-lookup"><span data-stu-id="3f80e-758">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="3f80e-759">Le nombre maximal de connexions TCP ouvertes simultanées peut être défini pour l’application entière avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="3f80e-759">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentConnections = 100;
        });
```

<span data-ttu-id="3f80e-760">Il existe une limite distincte pour les connexions qui ont été mises à niveau à partir de HTTP ou HTTPS vers un autre protocole (par exemple, sur une demande WebSocket).</span><span class="sxs-lookup"><span data-stu-id="3f80e-760">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="3f80e-761">Une fois mise à niveau, une connexion n’est pas prise en compte dans la limite `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-761">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

<span data-ttu-id="3f80e-762">Le nombre maximal de connexions est illimité (null) par défaut.</span><span class="sxs-lookup"><span data-stu-id="3f80e-762">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="3f80e-763">Taille maximale du corps de la requête</span><span class="sxs-lookup"><span data-stu-id="3f80e-763">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="3f80e-764">La taille maximale par défaut du corps de la requête est de 30 000 000 octets, soit environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="3f80e-764">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="3f80e-765">Pour remplacer la limite dans une application ASP.NET Core MVC, nous vous recommandons d’utiliser l’attribut <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> sur une méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="3f80e-765">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="3f80e-766">Voici un exemple qui montre comment configurer la contrainte pour l’application sur chaque requête :</span><span class="sxs-lookup"><span data-stu-id="3f80e-766">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="3f80e-767">Remplacer le paramètre sur une demande spécifique dans l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="3f80e-767">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="3f80e-768">Une exception est levée si l’application configure la limite sur une demande après que l’application a commencé à lire la demande.</span><span class="sxs-lookup"><span data-stu-id="3f80e-768">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="3f80e-769">Il existe une propriété `IsReadOnly` qui indique si la propriété `MaxRequestBodySize` est en lecture seule ; si tel est le cas, il est trop tard pour configurer la limite.</span><span class="sxs-lookup"><span data-stu-id="3f80e-769">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="3f80e-770">Quand une application est exécutée [hors processus](xref:host-and-deploy/iis/index#out-of-process-hosting-model) derrière le [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module), la limite de taille du corps de demande de Kestrel est désactivée, car IIS définit déjà la limite.</span><span class="sxs-lookup"><span data-stu-id="3f80e-770">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="3f80e-771">Débit données minimal du corps de la requête</span><span class="sxs-lookup"><span data-stu-id="3f80e-771">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="3f80e-772">Kestrel vérifie à chaque seconde si les données arrivent au débit spécifié en octets/seconde.</span><span class="sxs-lookup"><span data-stu-id="3f80e-772">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="3f80e-773">Si le débit est inférieur au minimum, la connexion expire. La période de grâce est la durée que Kestrel accorde au client pour augmenter sa vitesse de transmission jusqu’à la limite minimale ; pendant cette période, le débit n’est pas vérifié.</span><span class="sxs-lookup"><span data-stu-id="3f80e-773">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="3f80e-774">La période de grâce permet d’éviter la suppression des connexions qui, initialement, envoient des données à une vitesse lente en raison de la lenteur du démarrage de TCP.</span><span class="sxs-lookup"><span data-stu-id="3f80e-774">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="3f80e-775">Le débit minimal par défaut est 240 octets/seconde, avec une période de grâce de 5 secondes.</span><span class="sxs-lookup"><span data-stu-id="3f80e-775">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="3f80e-776">Un débit minimal s’applique également à la réponse.</span><span class="sxs-lookup"><span data-stu-id="3f80e-776">A minimum rate also applies to the response.</span></span> <span data-ttu-id="3f80e-777">Le code pour définir les limites de demande et de réponse est identique à l’exception de `RequestBody` ou `Response` dans les noms de propriété et d’interface.</span><span class="sxs-lookup"><span data-stu-id="3f80e-777">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="3f80e-778">Voici un exemple qui montre comment configurer les débits de données minimaux dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="3f80e-778">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MinRequestBodyDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
            serverOptions.Limits.MinResponseDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
        });
```

### <a name="request-headers-timeout"></a><span data-ttu-id="3f80e-779">Délai d’expiration des en-têtes de requête</span><span class="sxs-lookup"><span data-stu-id="3f80e-779">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="3f80e-780">Obtient ou définit le temps maximal passé par le serveur à recevoir des en-têtes de requête.</span><span class="sxs-lookup"><span data-stu-id="3f80e-780">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="3f80e-781">La valeur par défaut est de 30 secondes.</span><span class="sxs-lookup"><span data-stu-id="3f80e-781">Defaults to 30 seconds.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(1);
        });
```

### <a name="synchronous-io"></a><span data-ttu-id="3f80e-782">E/S synchrone</span><span class="sxs-lookup"><span data-stu-id="3f80e-782">Synchronous IO</span></span>

<span data-ttu-id="3f80e-783"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> contrôle si l’e/s synchrone est autorisée pour la requête et la réponse.</span><span class="sxs-lookup"><span data-stu-id="3f80e-783"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="3f80e-784">La valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-784">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="3f80e-785">Un grand nombre d’opérations d’e/s synchrones de blocage peut entraîner une privation de pool de thread, ce qui fait que l’application ne répond pas.</span><span class="sxs-lookup"><span data-stu-id="3f80e-785">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="3f80e-786">Activez uniquement `AllowSynchronousIO` lors de l’utilisation d’une bibliothèque qui ne prend pas en charge l’e/s asynchrone.</span><span class="sxs-lookup"><span data-stu-id="3f80e-786">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="3f80e-787">L’exemple suivant désactive un e/s synchrone :</span><span class="sxs-lookup"><span data-stu-id="3f80e-787">The following example disables synchronous IO:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.AllowSynchronousIO = false;
        });
```

<span data-ttu-id="3f80e-788">Pour plus d’informations sur les autres options et limites de Kestrel, consultez :</span><span class="sxs-lookup"><span data-stu-id="3f80e-788">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="3f80e-789">Configuration du point de terminaison</span><span class="sxs-lookup"><span data-stu-id="3f80e-789">Endpoint configuration</span></span>

<span data-ttu-id="3f80e-790">Par défaut, ASP.NET Core se lie à :</span><span class="sxs-lookup"><span data-stu-id="3f80e-790">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="3f80e-791">`https://localhost:5001` (quand un certificat de développement local est présent)</span><span class="sxs-lookup"><span data-stu-id="3f80e-791">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="3f80e-792">Spécifiez les URL avec :</span><span class="sxs-lookup"><span data-stu-id="3f80e-792">Specify URLs using the:</span></span>

* <span data-ttu-id="3f80e-793">La variable d’environnement `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-793">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="3f80e-794">L’argument de ligne de commande `--urls`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-794">`--urls` command-line argument.</span></span>
* <span data-ttu-id="3f80e-795">La clé de configuration d’hôte `urls`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-795">`urls` host configuration key.</span></span>
* <span data-ttu-id="3f80e-796">La méthode d’extension `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-796">`UseUrls` extension method.</span></span>

<span data-ttu-id="3f80e-797">La valeur fournie avec ces approches peut être un ou plusieurs points de terminaison HTTP et HTTPS (HTTPS si un certificat par défaut est disponible).</span><span class="sxs-lookup"><span data-stu-id="3f80e-797">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="3f80e-798">Configurez la valeur sous forme de liste délimitée par des points-virgules (par exemple `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="3f80e-798">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="3f80e-799">Pour plus d’informations sur ces approches, voir [URL de serveur](xref:fundamentals/host/web-host#server-urls) et [Remplacer la configuration](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="3f80e-799">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="3f80e-800">Un certificat de développement est créé :</span><span class="sxs-lookup"><span data-stu-id="3f80e-800">A development certificate is created:</span></span>

* <span data-ttu-id="3f80e-801">Quand le [SDK .NET Core](/dotnet/core/sdk) est installé.</span><span class="sxs-lookup"><span data-stu-id="3f80e-801">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="3f80e-802">[L’outil dev-certs](xref:aspnetcore-2.1#https) est utilisé pour créer un certificat.</span><span class="sxs-lookup"><span data-stu-id="3f80e-802">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="3f80e-803">Certains navigateurs requièrent l’octroi d’une autorisation explicite pour approuver le certificat de développement local.</span><span class="sxs-lookup"><span data-stu-id="3f80e-803">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="3f80e-804">Les modèles de projet configurent les applications à exécuter sur HTTPs par défaut et incluent [la redirection https et la prise en charge de HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="3f80e-804">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="3f80e-805">Appelez les méthodes <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> ou <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> sur <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> pour configurer les préfixes et les ports d’URL pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f80e-805">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="3f80e-806">`UseUrls`, l’argument de ligne de commande `--urls`, la clé de configuration d’hôte `urls` et la variable d’environnement `ASPNETCORE_URLS` fonctionnent également, mais ils présentent les limitations indiquées plus loin dans cette section (un certificat par défaut doit être disponible pour la configuration du point de terminaison HTTPS).</span><span class="sxs-lookup"><span data-stu-id="3f80e-806">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="3f80e-807">`KestrelServerOptions`configuré</span><span class="sxs-lookup"><span data-stu-id="3f80e-807">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="3f80e-808">ConfigureEndpointDefaults (action\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="3f80e-808">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="3f80e-809">Spécifie une `Action` de configuration à exécuter pour chaque point de terminaison spécifié.</span><span class="sxs-lookup"><span data-stu-id="3f80e-809">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="3f80e-810">Le fait d’appeler `ConfigureEndpointDefaults` plusieurs fois remplace les `Action`s précédentes par la dernière `Action` spécifiée.</span><span class="sxs-lookup"><span data-stu-id="3f80e-810">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
        });
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="3f80e-811">ConfigureHttpsDefaults (action\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="3f80e-811">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="3f80e-812">Spécifie une `Action` de configuration à exécuter pour chaque point de terminaison HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3f80e-812">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="3f80e-813">Le fait d’appeler `ConfigureHttpsDefaults` plusieurs fois remplace les `Action`s précédentes par la dernière `Action` spécifiée.</span><span class="sxs-lookup"><span data-stu-id="3f80e-813">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                // certificate is an X509Certificate2
                listenOptions.ServerCertificate = certificate;
            });
        });
```

### <a name="configureiconfiguration"></a><span data-ttu-id="3f80e-814">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="3f80e-814">Configure(IConfiguration)</span></span>

<span data-ttu-id="3f80e-815">Crée un chargeur de configuration pour configurer Kestrel, qui prend en entrée une <xref:Microsoft.Extensions.Configuration.IConfiguration>.</span><span class="sxs-lookup"><span data-stu-id="3f80e-815">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="3f80e-816">La configuration doit être limitée à la section de configuration pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f80e-816">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="3f80e-817">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="3f80e-817">ListenOptions.UseHttps</span></span>

<span data-ttu-id="3f80e-818">Configure Kestrel pour l’utilisation de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3f80e-818">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="3f80e-819">Extensions de `ListenOptions.UseHttps` :</span><span class="sxs-lookup"><span data-stu-id="3f80e-819">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="3f80e-820">`UseHttps` &ndash; Configure Kestrel pour l’utilisation de HTTPS avec le certificat par défaut.</span><span class="sxs-lookup"><span data-stu-id="3f80e-820">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="3f80e-821">Lève une exception si aucun certificat par défaut n’est configuré.</span><span class="sxs-lookup"><span data-stu-id="3f80e-821">Throws an exception if no default certificate is configured.</span></span>
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

<span data-ttu-id="3f80e-822">Paramètres de `ListenOptions.UseHttps` :</span><span class="sxs-lookup"><span data-stu-id="3f80e-822">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="3f80e-823">`filename` est le chemin et le nom d’un fichier de certificat, relatif au répertoire qui contient les fichiers de contenu de l’application.</span><span class="sxs-lookup"><span data-stu-id="3f80e-823">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="3f80e-824">`password` est le mot de passe nécessaire pour accéder aux données du certificat X.509.</span><span class="sxs-lookup"><span data-stu-id="3f80e-824">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="3f80e-825">`configureOptions` est une `Action` pour configurer les `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-825">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="3f80e-826">Retourne l'`ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-826">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="3f80e-827">`storeName` est le magasin de certificats à partir duquel charger le certificat.</span><span class="sxs-lookup"><span data-stu-id="3f80e-827">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="3f80e-828">`subject` est le nom du sujet du certificat.</span><span class="sxs-lookup"><span data-stu-id="3f80e-828">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="3f80e-829">`allowInvalid` indique si les certificats non valides doivent être considérés, comme les certificats auto-signés.</span><span class="sxs-lookup"><span data-stu-id="3f80e-829">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="3f80e-830">`location` est l’emplacement du magasin à partir duquel charger le certificat.</span><span class="sxs-lookup"><span data-stu-id="3f80e-830">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="3f80e-831">`serverCertificate` est le certificat X.509.</span><span class="sxs-lookup"><span data-stu-id="3f80e-831">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="3f80e-832">En production, HTTPS doit être explicitement configuré.</span><span class="sxs-lookup"><span data-stu-id="3f80e-832">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="3f80e-833">Au minimum, un certificat par défaut doit être fourni.</span><span class="sxs-lookup"><span data-stu-id="3f80e-833">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="3f80e-834">Configurations prises en charge décrites dans la suite :</span><span class="sxs-lookup"><span data-stu-id="3f80e-834">Supported configurations described next:</span></span>

* <span data-ttu-id="3f80e-835">Pas de configuration</span><span class="sxs-lookup"><span data-stu-id="3f80e-835">No configuration</span></span>
* <span data-ttu-id="3f80e-836">Remplacer le certificat par défaut dans la configuration</span><span class="sxs-lookup"><span data-stu-id="3f80e-836">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="3f80e-837">Changer les valeurs par défaut dans le code</span><span class="sxs-lookup"><span data-stu-id="3f80e-837">Change the defaults in code</span></span>

<span data-ttu-id="3f80e-838">*Pas de configuration*</span><span class="sxs-lookup"><span data-stu-id="3f80e-838">*No configuration*</span></span>

<span data-ttu-id="3f80e-839">Kestrel écoute sur `http://localhost:5000` et sur `https://localhost:5001` (si un certificat par défaut est disponible).</span><span class="sxs-lookup"><span data-stu-id="3f80e-839">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="3f80e-840">*Remplacer le certificat par défaut dans la configuration*</span><span class="sxs-lookup"><span data-stu-id="3f80e-840">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="3f80e-841">Par défaut, `CreateDefaultBuilder` appelle `Configure(context.Configuration.GetSection("Kestrel"))` pour charger la configuration de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f80e-841">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="3f80e-842">Un schéma de configuration des paramètres d’application HTTPS par défaut est disponible pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f80e-842">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="3f80e-843">Configurez plusieurs points de terminaison, notamment les URL et les certificats à utiliser, à partir d’un fichier sur disque ou d’un magasin de certificats.</span><span class="sxs-lookup"><span data-stu-id="3f80e-843">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="3f80e-844">Dans l’exemple de fichier *appsettings.json* suivant :</span><span class="sxs-lookup"><span data-stu-id="3f80e-844">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="3f80e-845">Définissez **AllowInvalid** sur `true` pour permettre l’utilisation de certificats non valides (par exemple des certificats auto-signés).</span><span class="sxs-lookup"><span data-stu-id="3f80e-845">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="3f80e-846">Les points de terminaison HTTPS qui ne spécifient pas de certificat (**HttpsDefaultCert** dans l’exemple qui suit) utilise comme alternative de secours le certificat défini sous **Certificats** > **Par défaut**  ou bien le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="3f80e-846">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "Http": {
        "Url": "http://localhost:5000"
      },

      "HttpsInlineCertFile": {
        "Url": "https://localhost:5001",
        "Certificate": {
          "Path": "<path to .pfx file>",
          "Password": "<certificate password>"
        }
      },

      "HttpsInlineCertStore": {
        "Url": "https://localhost:5002",
        "Certificate": {
          "Subject": "<subject; required>",
          "Store": "<certificate store; required>",
          "Location": "<location; defaults to CurrentUser>",
          "AllowInvalid": "<true or false; defaults to false>"
        }
      },

      "HttpsDefaultCert": {
        "Url": "https://localhost:5003"
      },

      "Https": {
        "Url": "https://*:5004",
        "Certificate": {
          "Path": "<path to .pfx file>",
          "Password": "<certificate password>"
        }
      }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

<span data-ttu-id="3f80e-847">Une alternative à l’utilisation de **Chemin** et de **Mot de passe** pour un nœud de certificat consiste à spécifier le certificat avec des champs du magasin de certificats.</span><span class="sxs-lookup"><span data-stu-id="3f80e-847">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="3f80e-848">Par exemple, le certificat **Certificats** > **Par défaut** peut être spécifié en tant que :</span><span class="sxs-lookup"><span data-stu-id="3f80e-848">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="3f80e-849">Notes de schéma :</span><span class="sxs-lookup"><span data-stu-id="3f80e-849">Schema notes:</span></span>

* <span data-ttu-id="3f80e-850">Les noms des points de terminaison ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="3f80e-850">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="3f80e-851">Par exemple, `HTTPS` et `Https` sont valides.</span><span class="sxs-lookup"><span data-stu-id="3f80e-851">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="3f80e-852">Le paramètre `Url` est obligatoire pour chaque point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="3f80e-852">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="3f80e-853">Le format de ce paramètre est le même que celui du paramètre de configuration `Urls` du plus haut niveau, sauf qu’il est limité à une seule valeur.</span><span class="sxs-lookup"><span data-stu-id="3f80e-853">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="3f80e-854">Ces points de terminaison remplacent ceux qui sont définis dans le paramètre de configuration `Urls` du plus haut niveau configuration, au lieu de s’y ajouter.</span><span class="sxs-lookup"><span data-stu-id="3f80e-854">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="3f80e-855">Les points de terminaison définis dans le code via `Listen` sont cumulatifs avec les points de terminaison définis dans la section de configuration.</span><span class="sxs-lookup"><span data-stu-id="3f80e-855">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="3f80e-856">La section `Certificate` est facultative.</span><span class="sxs-lookup"><span data-stu-id="3f80e-856">The `Certificate` section is optional.</span></span> <span data-ttu-id="3f80e-857">Si la section `Certificate` n’est pas spécifiée, les valeurs par défaut définies dans les scénarios précédents sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="3f80e-857">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="3f80e-858">Si aucune valeur par défaut n’est disponible, le serveur lève une exception et son démarrage échoue.</span><span class="sxs-lookup"><span data-stu-id="3f80e-858">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="3f80e-859">La section `Certificate` prend en charge les certificats **Chemin**&ndash;**Mot de passe** et **Sujet**&ndash;**Magasin**.</span><span class="sxs-lookup"><span data-stu-id="3f80e-859">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="3f80e-860">Vous pouvez définir un nombre quelconque de points de terminaison de cette façon, pour autant qu’ils ne provoquent pas de conflits de port.</span><span class="sxs-lookup"><span data-stu-id="3f80e-860">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="3f80e-861">`options.Configure(context.Configuration.GetSection("{SECTION}"))` retourne un `KestrelConfigurationLoader` avec une méthode `.Endpoint(string name, listenOptions => { })` qui peut être utilisée pour compléter les paramètres d’un point de terminaison configuré :</span><span class="sxs-lookup"><span data-stu-id="3f80e-861">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
                .Endpoint("HTTPS", listenOptions =>
                {
                    listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
                });
        });
```

<span data-ttu-id="3f80e-862">`KestrelServerOptions.ConfigurationLoader`peut être directement accessible pour poursuivre l’itération sur le chargeur existant, tel que celui fourni par <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="3f80e-862">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="3f80e-863">La section de configuration pour chaque point de terminaison est disponible sur les options de la méthode `Endpoint`, ce qui permet de lire les paramètres personnalisés.</span><span class="sxs-lookup"><span data-stu-id="3f80e-863">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="3f80e-864">Plusieurs configurations peuvent être chargées en rappelant `options.Configure(context.Configuration.GetSection("{SECTION}"))` avec une autre section.</span><span class="sxs-lookup"><span data-stu-id="3f80e-864">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="3f80e-865">Seule la dernière configuration est utilisée, à moins que `Load` soit explicitement appelé sur les instances précédentes.</span><span class="sxs-lookup"><span data-stu-id="3f80e-865">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="3f80e-866">Le métapackage n’appelle pas `Load` : sa section de configuration par défaut peut donc être remplacée.</span><span class="sxs-lookup"><span data-stu-id="3f80e-866">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="3f80e-867">`KestrelConfigurationLoader` reflète la famille d’API `Listen` de `KestrelServerOptions` sous forme de surcharges de `Endpoint` : le code et les points de terminaison de configuration peuvent donc être configurés au même emplacement.</span><span class="sxs-lookup"><span data-stu-id="3f80e-867">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="3f80e-868">Ces surcharges n’utilisent pas de noms et consomment seulement les paramètres par défaut de la configuration.</span><span class="sxs-lookup"><span data-stu-id="3f80e-868">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="3f80e-869">*Changer les valeurs par défaut dans le code*</span><span class="sxs-lookup"><span data-stu-id="3f80e-869">*Change the defaults in code*</span></span>

<span data-ttu-id="3f80e-870">`ConfigureEndpointDefaults` et `ConfigureHttpsDefaults` peuvent être utilisés pour modifier les paramètres par défaut pour `ListenOptions` et `HttpsConnectionAdapterOptions`, notamment en remplaçant le certificat par défaut spécifié dans le scénario précédent.</span><span class="sxs-lookup"><span data-stu-id="3f80e-870">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="3f80e-871">`ConfigureEndpointDefaults` et `ConfigureHttpsDefaults` doivent être appelés avant que des points de terminaison soient configurés.</span><span class="sxs-lookup"><span data-stu-id="3f80e-871">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
            
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                listenOptions.SslProtocols = SslProtocols.Tls12;
            });
        });
```

<span data-ttu-id="3f80e-872">*Prise en charge de SNI par Kestrel*</span><span class="sxs-lookup"><span data-stu-id="3f80e-872">*Kestrel support for SNI*</span></span>

<span data-ttu-id="3f80e-873">[L’indication du nom de serveur (SNI, Server Name Indication)](https://tools.ietf.org/html/rfc6066#section-3) peut être utilisée pour héberger plusieurs domaines sur la même adresse IP et le même port.</span><span class="sxs-lookup"><span data-stu-id="3f80e-873">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="3f80e-874">Pour que SNI fonctionne, le client envoie le nom d’hôte pour la session sécurisée au serveur pendant la négociation TLS afin que le serveur puisse fournir le certificat correct.</span><span class="sxs-lookup"><span data-stu-id="3f80e-874">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="3f80e-875">Le client utilise le certificat fourni pour la communication chiffrée avec le serveur pendant la session sécurisée qui suit la négociation TLS.</span><span class="sxs-lookup"><span data-stu-id="3f80e-875">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="3f80e-876">Kestrel prend en charge SNI via le rappel de `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-876">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="3f80e-877">Le rappel est appelé une fois par connexion pour permettre à l’application d’inspecter le nom d’hôte et de sélectionner le certificat approprié.</span><span class="sxs-lookup"><span data-stu-id="3f80e-877">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="3f80e-878">La prise en charge de SNI nécessite les points suivants :</span><span class="sxs-lookup"><span data-stu-id="3f80e-878">SNI support requires:</span></span>

* <span data-ttu-id="3f80e-879">Exécution sur la version `netcoreapp2.1` cible de .NET Framework ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="3f80e-879">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="3f80e-880">Sur `net461` ou version ultérieure, le rappel est appelé, `name` mais est toujours `null`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-880">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="3f80e-881">`name` est également `null` si le client ne fournit pas le paramètre du nom d’hôte dans la négociation TLS.</span><span class="sxs-lookup"><span data-stu-id="3f80e-881">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="3f80e-882">Tous les sites web s’exécutent sur la même instance Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f80e-882">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="3f80e-883">Kestrel ne prend pas en charge le partage d’une adresse IP et d’un port entre plusieurs instances sans un proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="3f80e-883">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ListenAnyIP(5005, listenOptions =>
            {
                listenOptions.UseHttps(httpsOptions =>
                {
                    var localhostCert = CertificateLoader.LoadFromStoreCert(
                        "localhost", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var exampleCert = CertificateLoader.LoadFromStoreCert(
                        "example.com", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var subExampleCert = CertificateLoader.LoadFromStoreCert(
                        "sub.example.com", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
                    certs["localhost"] = localhostCert;
                    certs["example.com"] = exampleCert;
                    certs["sub.example.com"] = subExampleCert;

                    httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                    {
                        if (name != null && certs.TryGetValue(name, out var cert))
                        {
                            return cert;
                        }

                        return exampleCert;
                    };
                });
            });
        })
        .Build();
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="3f80e-884">Lier à un socket TCP</span><span class="sxs-lookup"><span data-stu-id="3f80e-884">Bind to a TCP socket</span></span>

<span data-ttu-id="3f80e-885">La méthode <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> se lie à un socket TCP, et une expression lambda options permet la configuration d’un certificat X.509 :</span><span class="sxs-lookup"><span data-stu-id="3f80e-885">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Listen(IPAddress.Loopback, 5000);
            serverOptions.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Listen(IPAddress.Loopback, 5000);
            serverOptions.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

<span data-ttu-id="3f80e-886">L’exemple configure HTTPS pour un point de terminaison avec <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="3f80e-886">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="3f80e-887">Utilisez la même API afin de configurer d’autres paramètres Kestrel pour des points de terminaison spécifiques.</span><span class="sxs-lookup"><span data-stu-id="3f80e-887">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="3f80e-888">Lier à un socket Unix</span><span class="sxs-lookup"><span data-stu-id="3f80e-888">Bind to a Unix socket</span></span>

<span data-ttu-id="3f80e-889">Écoutez sur un socket Unix avec <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> pour améliorer les performances avec Nginx, comme illustré dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="3f80e-889">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.ListenUnixSocket("/tmp/kestrel-test.sock");
            serverOptions.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testpassword");
            });
        });
```

### <a name="port-0"></a><span data-ttu-id="3f80e-890">Port 0</span><span class="sxs-lookup"><span data-stu-id="3f80e-890">Port 0</span></span>

<span data-ttu-id="3f80e-891">Si vous spécifiez le numéro de port `0`, Kestrel se lie dynamiquement à un port disponible.</span><span class="sxs-lookup"><span data-stu-id="3f80e-891">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="3f80e-892">L’exemple suivant montre comment déterminer le port auquel Kestrel se lie à l’exécution :</span><span class="sxs-lookup"><span data-stu-id="3f80e-892">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="3f80e-893">Quand l’application est exécutée, la sortie de la fenêtre de console indique le port dynamique où l’application peut être atteinte :</span><span class="sxs-lookup"><span data-stu-id="3f80e-893">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="3f80e-894">Limites</span><span class="sxs-lookup"><span data-stu-id="3f80e-894">Limitations</span></span>

<span data-ttu-id="3f80e-895">Configurez des points de terminaison avec les approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="3f80e-895">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="3f80e-896">Arguments de ligne de commande `--urls`</span><span class="sxs-lookup"><span data-stu-id="3f80e-896">`--urls` command-line argument</span></span>
* <span data-ttu-id="3f80e-897">La clé de configuration d’hôte `urls`</span><span class="sxs-lookup"><span data-stu-id="3f80e-897">`urls` host configuration key</span></span>
* <span data-ttu-id="3f80e-898">Variable d’environnement `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="3f80e-898">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="3f80e-899">Ces méthodes sont utiles si vous voulez que votre code fonctionne avec des serveurs autres que Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f80e-899">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="3f80e-900">Toutefois, soyez conscient des limitations suivantes :</span><span class="sxs-lookup"><span data-stu-id="3f80e-900">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="3f80e-901">HTTPS ne peut pas être utilisé avec ces approches, sauf si un certificat par défaut est fourni dans la configuration du point de terminaison HTTPS (par exemple avec la configuration de `KestrelServerOptions` ou un fichier de configuration, comme illustré plus haut dans cette rubrique).</span><span class="sxs-lookup"><span data-stu-id="3f80e-901">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="3f80e-902">Quand les deux approches `Listen` et `UseUrls` sont utilisées simultanément, les points de terminaison `Listen` remplacent les points de terminaison `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-902">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="3f80e-903">Configuration de point de terminaison IIS</span><span class="sxs-lookup"><span data-stu-id="3f80e-903">IIS endpoint configuration</span></span>

<span data-ttu-id="3f80e-904">Quand vous utilisez IIS, les liaisons d’URL pour IIS remplacent les liaisons qui sont définies par `Listen` ou par `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-904">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="3f80e-905">Pour plus d’informations, consultez la rubrique [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="3f80e-905">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="3f80e-906">Configuration du transport</span><span class="sxs-lookup"><span data-stu-id="3f80e-906">Transport configuration</span></span>

<span data-ttu-id="3f80e-907">Dans ASP.NET Core 2.1, le transport par défaut de Kestrel n’est plus basé sur Libuv, mais sur des sockets managés.</span><span class="sxs-lookup"><span data-stu-id="3f80e-907">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="3f80e-908">Il s’agit d’un changement cassant pour les applications ASP.NET Core 2.0 mises à niveau vers la version 2.1 qui appellent <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> et qui dépendent d’un des packages suivants :</span><span class="sxs-lookup"><span data-stu-id="3f80e-908">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="3f80e-909">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (référence de package directe)</span><span class="sxs-lookup"><span data-stu-id="3f80e-909">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="3f80e-910">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="3f80e-910">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="3f80e-911">Pour les projets qui requièrent l’utilisation de Libuv :</span><span class="sxs-lookup"><span data-stu-id="3f80e-911">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="3f80e-912">Ajoutez une dépendance pour le package [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) au fichier projet de l’application :</span><span class="sxs-lookup"><span data-stu-id="3f80e-912">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="3f80e-913">Appelez <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> :</span><span class="sxs-lookup"><span data-stu-id="3f80e-913">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

  ```csharp
  public class Program
  {
      public static void Main(string[] args)
      {
          CreateWebHostBuilder(args).Build().Run();
      }

      public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
          WebHost.CreateDefaultBuilder(args)
              .UseLibuv()
              .UseStartup<Startup>();
  }
  ```

### <a name="url-prefixes"></a><span data-ttu-id="3f80e-914">Préfixes d’URL</span><span class="sxs-lookup"><span data-stu-id="3f80e-914">URL prefixes</span></span>

<span data-ttu-id="3f80e-915">Quand vous utilisez `UseUrls`, l’argument de ligne de commande `--urls`, la clé de configuration d’hôte `urls` ou la variable d’environnement `ASPNETCORE_URLS`, les préfixes d’URL peuvent être dans un des formats suivants.</span><span class="sxs-lookup"><span data-stu-id="3f80e-915">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="3f80e-916">Seuls les préfixes d’URL HTTP sont valides.</span><span class="sxs-lookup"><span data-stu-id="3f80e-916">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="3f80e-917">Kestrel ne prend pas en charge HTTPS lors de la configuration de liaisons d’URL avec `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f80e-917">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="3f80e-918">Adresse IPv4 avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="3f80e-918">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="3f80e-919">`0.0.0.0` est un cas spécial qui se lie à toutes les adresses IPv4.</span><span class="sxs-lookup"><span data-stu-id="3f80e-919">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="3f80e-920">Adresse IPv6 avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="3f80e-920">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="3f80e-921">`[::]` est l’équivalent IPv6 de `0.0.0.0` dans IPv4.</span><span class="sxs-lookup"><span data-stu-id="3f80e-921">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="3f80e-922">Nom d’hôte avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="3f80e-922">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="3f80e-923">Les noms d’hôte, `*` et `+` ne sont pas spéciaux.</span><span class="sxs-lookup"><span data-stu-id="3f80e-923">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="3f80e-924">Tout ce qui n’est pas reconnu comme adresse IP valide ou `localhost` se lie à toutes les adresses IP IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="3f80e-924">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="3f80e-925">Pour lier différents noms d’hôte à différentes applications ASP.NET Core sur le même port, utilisez [HTTP.sys](xref:fundamentals/servers/httpsys) ou un serveur proxy inverse, comme IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="3f80e-925">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="3f80e-926">L’hébergement dans une configuration de proxy inverse nécessite le [filtrage d’hôte](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="3f80e-926">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="3f80e-927">Nom `localhost` de l’hôte avec numéro de port ou adresse IP de bouclage avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="3f80e-927">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="3f80e-928">Quand `localhost` est spécifié, Kestrel tente de se lier aux interfaces de bouclage IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="3f80e-928">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="3f80e-929">Si le port demandé est en cours d’utilisation par un autre service sur l’une des interfaces de bouclage, Kestrel ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="3f80e-929">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="3f80e-930">Si l’une des interfaces de bouclage n’est pas disponible pour une raison quelconque (généralement du fait de la non-prise en charge d’IPv6), Kestrel journalise un avertissement.</span><span class="sxs-lookup"><span data-stu-id="3f80e-930">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="3f80e-931">Filtrage d’hôtes</span><span class="sxs-lookup"><span data-stu-id="3f80e-931">Host filtering</span></span>

<span data-ttu-id="3f80e-932">Si Kestrel prend en charge la configuration basée sur les préfixes, comme `http://example.com:5000`, il ignore en grande partie le nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="3f80e-932">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="3f80e-933">L’hôte `localhost` est un cas spécial utilisé pour la liaison à des adresses de bouclage.</span><span class="sxs-lookup"><span data-stu-id="3f80e-933">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="3f80e-934">Tout hôte autre qu’une adresse IP explicite se lie à toutes les adresses IP publiques.</span><span class="sxs-lookup"><span data-stu-id="3f80e-934">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="3f80e-935">Les en-têtes `Host` ne sont pas validés.</span><span class="sxs-lookup"><span data-stu-id="3f80e-935">`Host` headers aren't validated.</span></span>

<span data-ttu-id="3f80e-936">En guise de solution de contournement, utilisez le middleware de filtrage d’hôtes.</span><span class="sxs-lookup"><span data-stu-id="3f80e-936">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="3f80e-937">L’intergiciel (middleware) de filtrage d’hôte est fourni par le package [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) , qui est inclus dans le de [Microsoft. AspNetCore. App](xref:fundamentals/metapackage-app) (ASP.net Core 2,1 ou 2,2).</span><span class="sxs-lookup"><span data-stu-id="3f80e-937">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="3f80e-938">L’intergiciel (middleware) est ajouté par <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, qui appelle <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*> :</span><span class="sxs-lookup"><span data-stu-id="3f80e-938">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="3f80e-939">Le middleware de filtrage d’hôtes est désactivé par défaut.</span><span class="sxs-lookup"><span data-stu-id="3f80e-939">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="3f80e-940">Pour activer le middleware, définissez une clé `AllowedHosts` dans *appsettings.json*/*appsettings.\<nom_environnement>.json*.</span><span class="sxs-lookup"><span data-stu-id="3f80e-940">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="3f80e-941">La valeur est une liste délimitée par des points-virgules des noms d’hôte sans numéros de port :</span><span class="sxs-lookup"><span data-stu-id="3f80e-941">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="3f80e-942">*appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="3f80e-942">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="3f80e-943">Le [middleware des en-têtes transférés](xref:host-and-deploy/proxy-load-balancer) a aussi une option <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="3f80e-943">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="3f80e-944">Le middleware des en-têtes transférés et le middleware de filtrage d’hôtes ont des fonctionnalités similaires pour différents scénarios.</span><span class="sxs-lookup"><span data-stu-id="3f80e-944">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="3f80e-945">La définition d’`AllowedHosts` avec le middleware des en-têtes transférés est appropriée quand l’en-tête `Host` n’est pas conservé durant le transfert des requêtes avec un serveur proxy inverse ou un équilibreur de charge.</span><span class="sxs-lookup"><span data-stu-id="3f80e-945">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="3f80e-946">La définition d’`AllowedHosts` avec le middleware de filtrage d’hôtes est appropriée quand Kestrel est utilisé comme serveur de périphérie public ou que l’en-tête `Host` est directement transféré.</span><span class="sxs-lookup"><span data-stu-id="3f80e-946">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="3f80e-947">Pour plus d’informations sur le middleware des en-têtes transférés, consultez <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="3f80e-947">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="3f80e-948">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="3f80e-948">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="3f80e-949">Document RFC 7230 : syntaxe et routage des messages (section 5.4 : Hôte)</span><span class="sxs-lookup"><span data-stu-id="3f80e-949">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
