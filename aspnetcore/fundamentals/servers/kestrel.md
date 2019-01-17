---
title: Implémentation du serveur web Kestrel dans ASP.NET Core
author: guardrex
description: Découvrez plus d’informations sur Kestrel, serveur web multiplateforme pour ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/11/2019
uid: fundamentals/servers/kestrel
ms.openlocfilehash: a85403468d64b35ac5b6754139f78a12ad3fc386
ms.sourcegitcommit: ec71fd5a988f927ae301813aae5ff764feb3bb6a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/12/2019
ms.locfileid: "54249527"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="23c60-103">Implémentation du serveur web Kestrel dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="23c60-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="23c60-104">Par [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) et [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="23c60-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="23c60-105">Pour obtenir la version 1.1 de cette rubrique, téléchargez [Kestrel web server implementation in ASP.NET Core (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Kestrel_1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="23c60-105">For the 1.1 version of this topic, download [Kestrel web server implementation in ASP.NET Core (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Kestrel_1.1.pdf).</span></span>

::: moniker-end

<span data-ttu-id="23c60-106">Kestrel est un [serveur web multiplateforme pour ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="23c60-106">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="23c60-107">Kestrel est le serveur web inclus par défaut dans les modèles de projets ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="23c60-107">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="23c60-108">Kestrel prend en charge les scénarios suivants :</span><span class="sxs-lookup"><span data-stu-id="23c60-108">Kestrel supports the following scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="23c60-109">HTTPS</span><span class="sxs-lookup"><span data-stu-id="23c60-109">HTTPS</span></span>
* <span data-ttu-id="23c60-110">Mise à niveau opaque utilisée pour activer les [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="23c60-110">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="23c60-111">Sockets UNIX pour des performances élevées derrière Nginx</span><span class="sxs-lookup"><span data-stu-id="23c60-111">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="23c60-112">HTTP/2 (sauf sur macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="23c60-112">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="23c60-113">&dagger;HTTP/2 sera pris en charge sur macOS dans une prochaine version.</span><span class="sxs-lookup"><span data-stu-id="23c60-113">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="23c60-114">HTTPS</span><span class="sxs-lookup"><span data-stu-id="23c60-114">HTTPS</span></span>
* <span data-ttu-id="23c60-115">Mise à niveau opaque utilisée pour activer les [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="23c60-115">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="23c60-116">Sockets UNIX pour des performances élevées derrière Nginx</span><span class="sxs-lookup"><span data-stu-id="23c60-116">Unix sockets for high performance behind Nginx</span></span>

::: moniker-end

<span data-ttu-id="23c60-117">Kestrel est pris en charge sur toutes les plateformes et les versions prises en charge par .NET Core.</span><span class="sxs-lookup"><span data-stu-id="23c60-117">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="23c60-118">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="23c60-118">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="http2-support"></a><span data-ttu-id="23c60-119">Prise en charge de HTTP/2</span><span class="sxs-lookup"><span data-stu-id="23c60-119">HTTP/2 support</span></span>

<span data-ttu-id="23c60-120">[HTTP/2](https://httpwg.org/specs/rfc7540.html) est disponible pour les applications ASP.NET Core si les conditions de base suivantes sont remplies :</span><span class="sxs-lookup"><span data-stu-id="23c60-120">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="23c60-121">Système d’exploitation&dagger;</span><span class="sxs-lookup"><span data-stu-id="23c60-121">Operating system&dagger;</span></span>
  * <span data-ttu-id="23c60-122">Windows Server 2016/Windows 10 ou version ultérieure&Dagger;</span><span class="sxs-lookup"><span data-stu-id="23c60-122">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="23c60-123">Linux avec OpenSSL 1.0.2 ou version ultérieure (par exemple, Ubuntu 16.04 ou version ultérieure)</span><span class="sxs-lookup"><span data-stu-id="23c60-123">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="23c60-124">Framework cible : .NET Core 2.2 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="23c60-124">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="23c60-125">Connexion [ALPN (Application-Layer Protocol Negotiation)](https://tools.ietf.org/html/rfc7301#section-3)</span><span class="sxs-lookup"><span data-stu-id="23c60-125">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="23c60-126">TLS 1.2 ou connexion ultérieure</span><span class="sxs-lookup"><span data-stu-id="23c60-126">TLS 1.2 or later connection</span></span>

<span data-ttu-id="23c60-127">&dagger;HTTP/2 sera pris en charge sur macOS dans une prochaine version.</span><span class="sxs-lookup"><span data-stu-id="23c60-127">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="23c60-128">&Dagger;Kestrel propose une prise en charge limitée de HTTP/2 sous Windows Server 2012 R2 et Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="23c60-128">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="23c60-129">La prise en charge est limitée car la liste des suites de chiffrement TLS prises en charge sur ces systèmes d’exploitation est limitée.</span><span class="sxs-lookup"><span data-stu-id="23c60-129">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="23c60-130">Un certificat généré à l’aide d’Elliptic Curve Digital Signature algorithme (ECDSA) peut être requis pour sécuriser les connexions TLS.</span><span class="sxs-lookup"><span data-stu-id="23c60-130">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="23c60-131">Si une connexion HTTP/2 est établie, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) retourne `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="23c60-131">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="23c60-132">HTTP/2 est désactivé par défaut.</span><span class="sxs-lookup"><span data-stu-id="23c60-132">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="23c60-133">Pour plus d’informations sur la configuration, consultez les sections [Options Kestrel](#kestrel-options) et [ListenOptions.Protocols](#listenoptionsprotocols).</span><span class="sxs-lookup"><span data-stu-id="23c60-133">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

::: moniker-end

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="23c60-134">Quand utiliser Kestrel avec un proxy inverse ?</span><span class="sxs-lookup"><span data-stu-id="23c60-134">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="23c60-135">Vous pouvez utiliser Kestrel par lui-même ou avec un *serveur proxy inverse*, comme [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="23c60-135">You can use Kestrel by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="23c60-136">Un serveur proxy inverse reçoit les requêtes HTTP en provenance du réseau et les transmet à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="23c60-136">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="23c60-137">Kestrel utilisé comme serveur web edge (accessible sur Internet) :</span><span class="sxs-lookup"><span data-stu-id="23c60-137">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel communique directement avec Internet sans serveur proxy inverse](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="23c60-139">Kestrel utilisé dans une configuration de proxy inverse :</span><span class="sxs-lookup"><span data-stu-id="23c60-139">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="23c60-141">Les deux configurations, avec ou sans serveur proxy inverse, sont des configurations d’hébergement prises en charge pour les applications ASP.NET Core 2.1 ou ultérieur qui reçoivent des requêtes d’Internet.</span><span class="sxs-lookup"><span data-stu-id="23c60-141">Either configuration&mdash;with or without a reverse proxy server&mdash;is a supported hosting configuration for ASP.NET Core 2.1 or later apps that receive requests from the Internet.</span></span>

<span data-ttu-id="23c60-142">Kestrel, s’il est utilisé comme serveur de périphérie sans serveur proxy inverse, ne prend pas en charge le partage de la même adresse IP et du même port entre plusieurs processus.</span><span class="sxs-lookup"><span data-stu-id="23c60-142">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="23c60-143">Quand Kestrel est configuré pour écouter sur un port, il gère tout le trafic pour ce port, quel que soit les en-têtes `Host` des requêtes.</span><span class="sxs-lookup"><span data-stu-id="23c60-143">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="23c60-144">Un proxy inverse qui peut partager des ports a la possibilité de transférer des requêtes à Kestrel sur une adresse IP et un port uniques.</span><span class="sxs-lookup"><span data-stu-id="23c60-144">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="23c60-145">Même si un serveur proxy inverse n’est pas nécessaire, en utiliser un peut être un bon choix.</span><span class="sxs-lookup"><span data-stu-id="23c60-145">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="23c60-146">Un proxy inverse :</span><span class="sxs-lookup"><span data-stu-id="23c60-146">A reverse proxy:</span></span>

* <span data-ttu-id="23c60-147">Peut limiter la surface publique exposée des applications qu’il héberge.</span><span class="sxs-lookup"><span data-stu-id="23c60-147">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="23c60-148">Fournit une couche supplémentaire de configuration et de défense.</span><span class="sxs-lookup"><span data-stu-id="23c60-148">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="23c60-149">Peut mieux s’intégrer à l’infrastructure existante.</span><span class="sxs-lookup"><span data-stu-id="23c60-149">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="23c60-150">Simplifie la configuration de l’équilibrage de charge et d’une communication sécurisée (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="23c60-150">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="23c60-151">Seul le serveur proxy inverse nécessite un certificat X.509 : ce serveur peut donc communiquer avec vos serveurs d’applications sur le réseau interne avec du HTTP normal.</span><span class="sxs-lookup"><span data-stu-id="23c60-151">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with your app servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="23c60-152">L’hébergement dans une configuration de proxy inverse nécessite le [filtrage d’hôte](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="23c60-152">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="23c60-153">Comment utiliser Kestrel dans les applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="23c60-153">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="23c60-154">Le package [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) est inclus dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 ou version ultérieure).</span><span class="sxs-lookup"><span data-stu-id="23c60-154">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="23c60-155">Les modèles de projet ASP.NET Core utilisent Kestrel par défaut.</span><span class="sxs-lookup"><span data-stu-id="23c60-155">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="23c60-156">Dans *Program.cs*, le code du modèle appelle [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), qui appelle [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="23c60-156">In *Program.cs*, the template code calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="23c60-157">Pour fournir une configuration supplémentaire après l’appel de `CreateDefaultBuilder`, utilisez `ConfigureKestrel` :</span><span class="sxs-lookup"><span data-stu-id="23c60-157">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Set properties and call methods on options
        });
```

<span data-ttu-id="23c60-158">Si l’application n’appelle pas `CreateDefaultBuilder` pour configurer l’hôte, appelez <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **avant** d’appeler `ConfigureKestrel` :</span><span class="sxs-lookup"><span data-stu-id="23c60-158">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = new WebHostBuilder()
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseKestrel()
        .UseIISIntegration()
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Set properties and call methods on options
        })
        .Build();

    host.Run();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="23c60-159">Pour fournir une configuration supplémentaire après l’appel de `CreateDefaultBuilder`, appelez [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) :</span><span class="sxs-lookup"><span data-stu-id="23c60-159">To provide additional configuration after calling `CreateDefaultBuilder`, call [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel):</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            // Set properties and call methods on options
        });
```

::: moniker-end

## <a name="kestrel-options"></a><span data-ttu-id="23c60-160">Options Kestrel</span><span class="sxs-lookup"><span data-stu-id="23c60-160">Kestrel options</span></span>

<span data-ttu-id="23c60-161">Le serveur web Kestrel a des options de configuration de contrainte qui sont particulièrement utiles dans les déploiements exposés à Internet.</span><span class="sxs-lookup"><span data-stu-id="23c60-161">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="23c60-162">Définissez ces contraintes sur la propriété [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) de la classe [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="23c60-162">Set constraints on the [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) property of the [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) class.</span></span> <span data-ttu-id="23c60-163">La propriété `Limits` conserve une instance de la classe [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits).</span><span class="sxs-lookup"><span data-stu-id="23c60-163">The `Limits` property holds an instance of the [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) class.</span></span>

### <a name="maximum-client-connections"></a><span data-ttu-id="23c60-164">Nombre maximale de connexions client</span><span class="sxs-lookup"><span data-stu-id="23c60-164">Maximum client connections</span></span>

[<span data-ttu-id="23c60-165">MaxConcurrentConnections</span><span class="sxs-lookup"><span data-stu-id="23c60-165">MaxConcurrentConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[<span data-ttu-id="23c60-166">MaxConcurrentUpgradedConnections</span><span class="sxs-lookup"><span data-stu-id="23c60-166">MaxConcurrentUpgradedConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

<span data-ttu-id="23c60-167">Le nombre maximal de connexions TCP ouvertes simultanées peut être défini pour l’application entière avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="23c60-167">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentConnections = 100;
        });
```

::: moniker-end

<span data-ttu-id="23c60-168">Il existe une limite distincte pour les connexions qui ont été mises à niveau à partir de HTTP ou HTTPS vers un autre protocole (par exemple, sur une demande WebSocket).</span><span class="sxs-lookup"><span data-stu-id="23c60-168">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="23c60-169">Une fois mise à niveau, une connexion n’est pas prise en compte dans la limite `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="23c60-169">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

::: moniker-end

<span data-ttu-id="23c60-170">Le nombre maximal de connexions est illimité (null) par défaut.</span><span class="sxs-lookup"><span data-stu-id="23c60-170">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="23c60-171">Taille maximale du corps de la requête</span><span class="sxs-lookup"><span data-stu-id="23c60-171">Maximum request body size</span></span>

[<span data-ttu-id="23c60-172">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="23c60-172">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

<span data-ttu-id="23c60-173">La taille maximale par défaut du corps de la requête est de 30 000 000 octets, soit environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="23c60-173">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="23c60-174">Pour remplacer la limite dans une application ASP.NET Core MVC, nous vous recommandons d’utiliser l’attribut [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) sur une méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="23c60-174">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="23c60-175">Voici un exemple qui montre comment configurer la contrainte pour l’application sur chaque requête :</span><span class="sxs-lookup"><span data-stu-id="23c60-175">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="23c60-176">Vous pouvez remplacer le paramètre sur une demande spécifique dans l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="23c60-176">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

::: moniker-end

<span data-ttu-id="23c60-177">Une exception est levée vous tentez de configurer la limite sur une requête une fois que l’application a commencé à la lire.</span><span class="sxs-lookup"><span data-stu-id="23c60-177">An exception is thrown if you attempt to configure the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="23c60-178">Il existe une propriété `IsReadOnly` qui indique si la propriété `MaxRequestBodySize` est en lecture seule ; si tel est le cas, il est trop tard pour configurer la limite.</span><span class="sxs-lookup"><span data-stu-id="23c60-178">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="23c60-179">Débit données minimal du corps de la requête</span><span class="sxs-lookup"><span data-stu-id="23c60-179">Minimum request body data rate</span></span>

[<span data-ttu-id="23c60-180">MinRequestBodyDataRate</span><span class="sxs-lookup"><span data-stu-id="23c60-180">MinRequestBodyDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[<span data-ttu-id="23c60-181">MinResponseDataRate</span><span class="sxs-lookup"><span data-stu-id="23c60-181">MinResponseDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

<span data-ttu-id="23c60-182">Kestrel vérifie à chaque seconde si les données arrivent au débit spécifié en octets/seconde.</span><span class="sxs-lookup"><span data-stu-id="23c60-182">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="23c60-183">Si le débit est inférieur au minimum, la connexion expire. La période de grâce est la durée que Kestrel accorde au client pour augmenter sa vitesse de transmission jusqu’à la limite minimale ; pendant cette période, le débit n’est pas vérifié.</span><span class="sxs-lookup"><span data-stu-id="23c60-183">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="23c60-184">La période de grâce permet d’éviter la suppression des connexions qui, initialement, envoient des données à une vitesse lente en raison de la lenteur du démarrage de TCP.</span><span class="sxs-lookup"><span data-stu-id="23c60-184">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="23c60-185">Le débit minimal par défaut est 240 octets/seconde, avec une période de grâce de 5 secondes.</span><span class="sxs-lookup"><span data-stu-id="23c60-185">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="23c60-186">Un débit minimal s’applique également à la réponse.</span><span class="sxs-lookup"><span data-stu-id="23c60-186">A minimum rate also applies to the response.</span></span> <span data-ttu-id="23c60-187">Le code pour définir les limites de demande et de réponse est identique à l’exception de `RequestBody` ou `Response` dans les noms de propriété et d’interface.</span><span class="sxs-lookup"><span data-stu-id="23c60-187">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="23c60-188">Voici un exemple qui montre comment configurer les débits de données minimaux dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="23c60-188">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MinRequestBodyDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
            options.Limits.MinResponseDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
        });
```

::: moniker-end

<span data-ttu-id="23c60-189">Vous pouvez remplacer les limites de débit minimal par requête dans l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="23c60-189">You can override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="23c60-190">Aucune des fonctionnalités de débit référencées dans l’exemple précédent n’est présente dans `HttpContext.Features` pour les requêtes HTTP/2, car la modification des limites de débit par requête n’est pas prise en charge pour HTTP/2 (le protocole prend en charge le multiplexage de requête).</span><span class="sxs-lookup"><span data-stu-id="23c60-190">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="23c60-191">Les limites de débit à l’échelle du serveur configurées par le biais de `KestrelServerOptions.Limits` s’appliquent encore aux connexions HTTP/1.x et HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="23c60-191">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="23c60-192">Flux de données maximal par connexion</span><span class="sxs-lookup"><span data-stu-id="23c60-192">Maximum streams per connection</span></span>

<span data-ttu-id="23c60-193">`Http2.MaxStreamsPerConnection` limite le nombre de flux de requête simultanée par connexion HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="23c60-193">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="23c60-194">Les flux de données excédentaires sont refusés.</span><span class="sxs-lookup"><span data-stu-id="23c60-194">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="23c60-195">La valeur par défaut est 100.</span><span class="sxs-lookup"><span data-stu-id="23c60-195">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="23c60-196">Taille de la table d’en-tête</span><span class="sxs-lookup"><span data-stu-id="23c60-196">Header table size</span></span>

<span data-ttu-id="23c60-197">Le décodeur HPACK décompresse les en-têtes HTTP pour les connexions HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="23c60-197">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="23c60-198">`Http2.HeaderTableSize` limite la taille de la table de compression d’en-tête que le décodeur HPACK utilise.</span><span class="sxs-lookup"><span data-stu-id="23c60-198">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="23c60-199">La valeur est fournie en octets et doit être supérieure à zéro (0).</span><span class="sxs-lookup"><span data-stu-id="23c60-199">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="23c60-200">La valeur par défaut est 4096.</span><span class="sxs-lookup"><span data-stu-id="23c60-200">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="23c60-201">Taille de trame maximale</span><span class="sxs-lookup"><span data-stu-id="23c60-201">Maximum frame size</span></span>

<span data-ttu-id="23c60-202">`Http2.MaxFrameSize` Indique la taille maximale de charge utile dans la trame de connexion HTTP/2 à recevoir.</span><span class="sxs-lookup"><span data-stu-id="23c60-202">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="23c60-203">La valeur est fournie en octets et doit être comprise entre 2^14 (16,384) et 2^24-1 (16,777,215).</span><span class="sxs-lookup"><span data-stu-id="23c60-203">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="23c60-204">La valeur par défaut est 2^14 (16,384).</span><span class="sxs-lookup"><span data-stu-id="23c60-204">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="23c60-205">Taille maximale d’en-tête de requête</span><span class="sxs-lookup"><span data-stu-id="23c60-205">Maximum request header size</span></span>

<span data-ttu-id="23c60-206">`Http2.MaxRequestHeaderFieldSize` indique la taille maximale autorisée en octets des valeurs d’en-tête de requête.</span><span class="sxs-lookup"><span data-stu-id="23c60-206">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="23c60-207">Cette limite s’applique au nom et à la valeur dans leurs représentations compressées et non compressées.</span><span class="sxs-lookup"><span data-stu-id="23c60-207">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="23c60-208">La valeur doit être supérieure à zéro (0).</span><span class="sxs-lookup"><span data-stu-id="23c60-208">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="23c60-209">La valeur par défaut est 8 192.</span><span class="sxs-lookup"><span data-stu-id="23c60-209">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="23c60-210">Taille de fenêtre de connexion initiale</span><span class="sxs-lookup"><span data-stu-id="23c60-210">Initial connection window size</span></span>

<span data-ttu-id="23c60-211">`Http2.InitialConnectionWindowSize` indique la quantité maximale de données de corps de requête, en octets, que le serveur met en mémoire tampon à un moment donné pour toutes les requêtes (flux) par connexion.</span><span class="sxs-lookup"><span data-stu-id="23c60-211">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="23c60-212">Les requêtes sont également limitées par `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="23c60-212">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="23c60-213">La valeur doit être supérieure ou égale à 65 535 et inférieure à 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="23c60-213">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="23c60-214">La valeur par défaut est 128 Ko (131 072).</span><span class="sxs-lookup"><span data-stu-id="23c60-214">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="23c60-215">Taille de la fenêtre de flux initiale</span><span class="sxs-lookup"><span data-stu-id="23c60-215">Initial stream window size</span></span>

<span data-ttu-id="23c60-216">`Http2.InitialStreamWindowSize` indique la quantité maximale de données de corps de requête, en octets, que le serveur met en mémoire tampon à un moment donné par requête (flux).</span><span class="sxs-lookup"><span data-stu-id="23c60-216">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="23c60-217">Les requêtes sont également limitées par `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="23c60-217">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="23c60-218">La valeur doit être supérieure ou égale à 65 535 et inférieure à 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="23c60-218">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="23c60-219">La valeur par défaut est 96 Ko (98 304).</span><span class="sxs-lookup"><span data-stu-id="23c60-219">The default value is 96 KB (98,304).</span></span>

::: moniker-end

<span data-ttu-id="23c60-220">Pour plus d’informations sur les autres options et limites de Kestrel, consultez :</span><span class="sxs-lookup"><span data-stu-id="23c60-220">For information about other Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="23c60-221">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="23c60-221">KestrelServerOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [<span data-ttu-id="23c60-222">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="23c60-222">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [<span data-ttu-id="23c60-223">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="23c60-223">ListenOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

## <a name="endpoint-configuration"></a><span data-ttu-id="23c60-224">Configuration de point de terminaison</span><span class="sxs-lookup"><span data-stu-id="23c60-224">Endpoint configuration</span></span>

<span data-ttu-id="23c60-225">Par défaut, ASP.NET Core se lie à :</span><span class="sxs-lookup"><span data-stu-id="23c60-225">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="23c60-226">`https://localhost:5001` (quand un certificat de développement local est présent)</span><span class="sxs-lookup"><span data-stu-id="23c60-226">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="23c60-227">Un certificat de développement est créé :</span><span class="sxs-lookup"><span data-stu-id="23c60-227">A development certificate is created:</span></span>

* <span data-ttu-id="23c60-228">Quand le [SDK .NET Core](/dotnet/core/sdk) est installé.</span><span class="sxs-lookup"><span data-stu-id="23c60-228">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="23c60-229">[L’outil dev-certs](xref:aspnetcore-2.1#https) est utilisé pour créer un certificat.</span><span class="sxs-lookup"><span data-stu-id="23c60-229">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="23c60-230">Vous devez accorder à certains navigateurs l’autorisation explicite d’approuver le certificat de développement local.</span><span class="sxs-lookup"><span data-stu-id="23c60-230">Some browsers require that you grant explicit permission to the browser to trust the local development certificate.</span></span>

<span data-ttu-id="23c60-231">Les modèles de projet ASP.NET Core 2.1 et ultérieur configurent les applications pour une exécution par défaut sur HTTPS et pour l’inclusion de [la redirection HTTPS et de la prise en charge de HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="23c60-231">ASP.NET Core 2.1 and later project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="23c60-232">Appelez les méthodes [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) ou [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) sur [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) pour configurer les préfixes d’URL et les ports pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="23c60-232">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="23c60-233">`UseUrls`, l’argument de ligne de commande `--urls`, la clé de configuration d’hôte `urls` et la variable d’environnement `ASPNETCORE_URLS` fonctionnent également, mais ils présentent les limitations indiquées plus loin dans cette section (un certificat par défaut doit être disponible pour la configuration du point de terminaison HTTPS).</span><span class="sxs-lookup"><span data-stu-id="23c60-233">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="23c60-234">Configuration de `KestrelServerOptions` pour ASP.NET Core 2.1 :</span><span class="sxs-lookup"><span data-stu-id="23c60-234">ASP.NET Core 2.1 `KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionltlistenoptionsgt"></a><span data-ttu-id="23c60-235">ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)</span><span class="sxs-lookup"><span data-stu-id="23c60-235">ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)</span></span>

<span data-ttu-id="23c60-236">Spécifie une `Action` de configuration à exécuter pour chaque point de terminaison spécifié.</span><span class="sxs-lookup"><span data-stu-id="23c60-236">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="23c60-237">Le fait d’appeler `ConfigureEndpointDefaults` plusieurs fois remplace les `Action`s précédentes par la dernière `Action` spécifiée.</span><span class="sxs-lookup"><span data-stu-id="23c60-237">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

### <a name="configurehttpsdefaultsactionlthttpsconnectionadapteroptionsgt"></a><span data-ttu-id="23c60-238">ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)</span><span class="sxs-lookup"><span data-stu-id="23c60-238">ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)</span></span>

<span data-ttu-id="23c60-239">Spécifie une `Action` de configuration à exécuter pour chaque point de terminaison HTTPS.</span><span class="sxs-lookup"><span data-stu-id="23c60-239">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="23c60-240">Le fait d’appeler `ConfigureHttpsDefaults` plusieurs fois remplace les `Action`s précédentes par la dernière `Action` spécifiée.</span><span class="sxs-lookup"><span data-stu-id="23c60-240">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

### <a name="configureiconfiguration"></a><span data-ttu-id="23c60-241">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="23c60-241">Configure(IConfiguration)</span></span>

<span data-ttu-id="23c60-242">Crée un chargeur de configuration pour configurer Kestrel, qui prend en entrée une [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration).</span><span class="sxs-lookup"><span data-stu-id="23c60-242">Creates a configuration loader for setting up Kestrel that takes an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) as input.</span></span> <span data-ttu-id="23c60-243">La configuration doit être limitée à la section de configuration pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="23c60-243">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="23c60-244">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="23c60-244">ListenOptions.UseHttps</span></span>

<span data-ttu-id="23c60-245">Configure Kestrel pour l’utilisation de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="23c60-245">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="23c60-246">Extensions de `ListenOptions.UseHttps` :</span><span class="sxs-lookup"><span data-stu-id="23c60-246">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="23c60-247">`UseHttps` &ndash; Configure Kestrel pour l’utilisation de HTTPS avec le certificat par défaut.</span><span class="sxs-lookup"><span data-stu-id="23c60-247">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="23c60-248">Lève une exception si aucun certificat par défaut n’est configuré.</span><span class="sxs-lookup"><span data-stu-id="23c60-248">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="23c60-249">Paramètres de `ListenOptions.UseHttps` :</span><span class="sxs-lookup"><span data-stu-id="23c60-249">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="23c60-250">`filename` est le chemin et le nom d’un fichier de certificat, relatif au répertoire qui contient les fichiers de contenu de l’application.</span><span class="sxs-lookup"><span data-stu-id="23c60-250">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="23c60-251">`password` est le mot de passe nécessaire pour accéder aux données du certificat X.509.</span><span class="sxs-lookup"><span data-stu-id="23c60-251">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="23c60-252">`configureOptions` est une `Action` pour configurer les `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="23c60-252">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="23c60-253">Retourne l'`ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="23c60-253">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="23c60-254">`storeName` est le magasin de certificats à partir duquel charger le certificat.</span><span class="sxs-lookup"><span data-stu-id="23c60-254">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="23c60-255">`subject` est le nom du sujet du certificat.</span><span class="sxs-lookup"><span data-stu-id="23c60-255">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="23c60-256">`allowInvalid` indique si les certificats non valides doivent être considérés, comme les certificats auto-signés.</span><span class="sxs-lookup"><span data-stu-id="23c60-256">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="23c60-257">`location` est l’emplacement du magasin à partir duquel charger le certificat.</span><span class="sxs-lookup"><span data-stu-id="23c60-257">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="23c60-258">`serverCertificate` est le certificat X.509.</span><span class="sxs-lookup"><span data-stu-id="23c60-258">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="23c60-259">En production, HTTPS doit être explicitement configuré.</span><span class="sxs-lookup"><span data-stu-id="23c60-259">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="23c60-260">Au minimum, un certificat par défaut doit être fourni.</span><span class="sxs-lookup"><span data-stu-id="23c60-260">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="23c60-261">Configurations prises en charge décrites dans la suite :</span><span class="sxs-lookup"><span data-stu-id="23c60-261">Supported configurations described next:</span></span>

* <span data-ttu-id="23c60-262">Pas de configuration</span><span class="sxs-lookup"><span data-stu-id="23c60-262">No configuration</span></span>
* <span data-ttu-id="23c60-263">Remplacer le certificat par défaut dans la configuration</span><span class="sxs-lookup"><span data-stu-id="23c60-263">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="23c60-264">Changer les valeurs par défaut dans le code</span><span class="sxs-lookup"><span data-stu-id="23c60-264">Change the defaults in code</span></span>

<span data-ttu-id="23c60-265">*Pas de configuration*</span><span class="sxs-lookup"><span data-stu-id="23c60-265">*No configuration*</span></span>

<span data-ttu-id="23c60-266">Kestrel écoute sur `http://localhost:5000` et sur `https://localhost:5001` (si un certificat par défaut est disponible).</span><span class="sxs-lookup"><span data-stu-id="23c60-266">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<span data-ttu-id="23c60-267">Spécifiez les URL avec :</span><span class="sxs-lookup"><span data-stu-id="23c60-267">Specify URLs using the:</span></span>

* <span data-ttu-id="23c60-268">La variable d’environnement `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="23c60-268">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="23c60-269">L’argument de ligne de commande `--urls`.</span><span class="sxs-lookup"><span data-stu-id="23c60-269">`--urls` command-line argument.</span></span>
* <span data-ttu-id="23c60-270">La clé de configuration d’hôte `urls`.</span><span class="sxs-lookup"><span data-stu-id="23c60-270">`urls` host configuration key.</span></span>
* <span data-ttu-id="23c60-271">La méthode d’extension `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="23c60-271">`UseUrls` extension method.</span></span>

<span data-ttu-id="23c60-272">Pour plus d’informations, consultez [URL de serveur](xref:fundamentals/host/web-host#server-urls) et [Remplacer la configuration](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="23c60-272">For more information, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="23c60-273">La valeur fournie avec ces approches peut être un ou plusieurs points de terminaison HTTP et HTTPS (HTTPS si un certificat par défaut est disponible).</span><span class="sxs-lookup"><span data-stu-id="23c60-273">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="23c60-274">Configurez la valeur sous forme de liste délimitée par des points-virgules (par exemple `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="23c60-274">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="23c60-275">*Remplacer le certificat par défaut dans la configuration*</span><span class="sxs-lookup"><span data-stu-id="23c60-275">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="23c60-276">Par défaut, [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) appelle `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` pour charger la configuration de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="23c60-276">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="23c60-277">Un schéma de configuration des paramètres d’application HTTPS par défaut est disponible pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="23c60-277">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="23c60-278">Configurez plusieurs points de terminaison, notamment les URL et les certificats à utiliser, à partir d’un fichier sur disque ou d’un magasin de certificats.</span><span class="sxs-lookup"><span data-stu-id="23c60-278">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="23c60-279">Dans l’exemple de fichier *appsettings.json* suivant :</span><span class="sxs-lookup"><span data-stu-id="23c60-279">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="23c60-280">Définissez **AllowInvalid** sur `true` pour permettre l’utilisation de certificats non valides (par exemple des certificats auto-signés).</span><span class="sxs-lookup"><span data-stu-id="23c60-280">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="23c60-281">Tout point de terminaison HTTPS qui ne spécifie pas de certificat (**HttpsDefaultCert** dans l’exemple qui suit) revient au certificat défini sous **Certificats** > **Par défaut** ou au certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="23c60-281">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
"Kestrel": {
  "EndPoints": {
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
        "Store": "<certificate store; defaults to My>",
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

<span data-ttu-id="23c60-282">Une alternative à l’utilisation de **Chemin** et de **Mot de passe** pour un nœud de certificat consiste à spécifier le certificat avec des champs du magasin de certificats.</span><span class="sxs-lookup"><span data-stu-id="23c60-282">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="23c60-283">Par exemple, le certificat **Certificats** > **Par défaut** peut être spécifié en tant que :</span><span class="sxs-lookup"><span data-stu-id="23c60-283">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="23c60-284">Notes de schéma :</span><span class="sxs-lookup"><span data-stu-id="23c60-284">Schema notes:</span></span>

* <span data-ttu-id="23c60-285">Les noms des points de terminaison ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="23c60-285">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="23c60-286">Par exemple, `HTTPS` et `Https` sont valides.</span><span class="sxs-lookup"><span data-stu-id="23c60-286">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="23c60-287">Le paramètre `Url` est obligatoire pour chaque point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="23c60-287">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="23c60-288">Le format de ce paramètre est le même que celui du paramètre de configuration `Urls` du plus haut niveau, sauf qu’il est limité à une seule valeur.</span><span class="sxs-lookup"><span data-stu-id="23c60-288">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="23c60-289">Ces points de terminaison remplacent ceux qui sont définis dans le paramètre de configuration `Urls` du plus haut niveau configuration, au lieu de s’y ajouter.</span><span class="sxs-lookup"><span data-stu-id="23c60-289">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="23c60-290">Les points de terminaison définis dans le code via `Listen` sont cumulatifs avec les points de terminaison définis dans la section de configuration.</span><span class="sxs-lookup"><span data-stu-id="23c60-290">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="23c60-291">La section `Certificate` est facultative.</span><span class="sxs-lookup"><span data-stu-id="23c60-291">The `Certificate` section is optional.</span></span> <span data-ttu-id="23c60-292">Si la section `Certificate` n’est pas spécifiée, les valeurs par défaut définies dans les scénarios précédents sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="23c60-292">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="23c60-293">Si aucune valeur par défaut n’est disponible, le serveur lève une exception et son démarrage échoue.</span><span class="sxs-lookup"><span data-stu-id="23c60-293">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="23c60-294">La section `Certificate` prend en charge les certificats **Chemin**&ndash;**Mot de passe** et **Sujet**&ndash;**Magasin**.</span><span class="sxs-lookup"><span data-stu-id="23c60-294">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="23c60-295">Vous pouvez définir un nombre quelconque de points de terminaison de cette façon, pour autant qu’ils ne provoquent pas de conflits de port.</span><span class="sxs-lookup"><span data-stu-id="23c60-295">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="23c60-296">`options.Configure(context.Configuration.GetSection("Kestrel"))` retourne un `KestrelConfigurationLoader` avec une méthode `.Endpoint(string name, options => { })` qui peut être utilisée pour compléter les paramètres d’un point de terminaison configuré :</span><span class="sxs-lookup"><span data-stu-id="23c60-296">`options.Configure(context.Configuration.GetSection("Kestrel"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, options => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

  ```csharp
  options.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  <span data-ttu-id="23c60-297">Vous pouvez également accéder directement à `KestrelServerOptions.ConfigurationLoader` pour continuer à boucler sur le chargeur existant, comme celui fourni par [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="23c60-297">You can also directly access `KestrelServerOptions.ConfigurationLoader` to keep iterating on the existing loader, such as the one provided by [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

* <span data-ttu-id="23c60-298">La section de configuration pour chaque point de terminaison est disponible sur les options de la méthode `Endpoint`, ce qui permet de lire les paramètres personnalisés.</span><span class="sxs-lookup"><span data-stu-id="23c60-298">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="23c60-299">Plusieurs configurations peuvent être chargées en rappelant `options.Configure(context.Configuration.GetSection("Kestrel"))` avec une autre section.</span><span class="sxs-lookup"><span data-stu-id="23c60-299">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("Kestrel"))` again with another section.</span></span> <span data-ttu-id="23c60-300">Seule la dernière configuration est utilisée, à moins que `Load` soit explicitement appelé sur les instances précédentes.</span><span class="sxs-lookup"><span data-stu-id="23c60-300">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="23c60-301">Le métapackage n’appelle pas `Load` : sa section de configuration par défaut peut donc être remplacée.</span><span class="sxs-lookup"><span data-stu-id="23c60-301">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="23c60-302">`KestrelConfigurationLoader` reflète la famille d’API `Listen` de `KestrelServerOptions` sous forme de surcharges de `Endpoint` : le code et les points de terminaison de configuration peuvent donc être configurés au même emplacement.</span><span class="sxs-lookup"><span data-stu-id="23c60-302">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="23c60-303">Ces surcharges n’utilisent pas de noms et consomment seulement les paramètres par défaut de la configuration.</span><span class="sxs-lookup"><span data-stu-id="23c60-303">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="23c60-304">*Changer les valeurs par défaut dans le code*</span><span class="sxs-lookup"><span data-stu-id="23c60-304">*Change the defaults in code*</span></span>

<span data-ttu-id="23c60-305">`ConfigureEndpointDefaults` et `ConfigureHttpsDefaults` peuvent être utilisés pour modifier les paramètres par défaut pour `ListenOptions` et `HttpsConnectionAdapterOptions`, notamment en remplaçant le certificat par défaut spécifié dans le scénario précédent.</span><span class="sxs-lookup"><span data-stu-id="23c60-305">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="23c60-306">`ConfigureEndpointDefaults` et `ConfigureHttpsDefaults` doivent être appelés avant que des points de terminaison soient configurés.</span><span class="sxs-lookup"><span data-stu-id="23c60-306">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
options.ConfigureEndpointDefaults(opt =>
{
    opt.NoDelay = true;
});

options.ConfigureHttpsDefaults(httpsOptions =>
{
    httpsOptions.SslProtocols = SslProtocols.Tls12;
});
```

<span data-ttu-id="23c60-307">*Prise en charge de SNI par Kestrel*</span><span class="sxs-lookup"><span data-stu-id="23c60-307">*Kestrel support for SNI*</span></span>

<span data-ttu-id="23c60-308">[L’indication du nom de serveur (SNI, Server Name Indication)](https://tools.ietf.org/html/rfc6066#section-3) peut être utilisée pour héberger plusieurs domaines sur la même adresse IP et le même port.</span><span class="sxs-lookup"><span data-stu-id="23c60-308">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="23c60-309">Pour que SNI fonctionne, le client envoie le nom d’hôte pour la session sécurisée au serveur pendant la négociation TLS afin que le serveur puisse fournir le certificat correct.</span><span class="sxs-lookup"><span data-stu-id="23c60-309">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="23c60-310">Le client utilise le certificat fourni pour la communication chiffrée avec le serveur pendant la session sécurisée qui suit la négociation TLS.</span><span class="sxs-lookup"><span data-stu-id="23c60-310">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="23c60-311">Kestrel prend en charge SNI via le rappel de `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="23c60-311">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="23c60-312">Le rappel est appelé une fois par connexion pour permettre à l’application d’inspecter le nom d’hôte et de sélectionner le certificat approprié.</span><span class="sxs-lookup"><span data-stu-id="23c60-312">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="23c60-313">La prise en charge de SNI nécessite les points suivants :</span><span class="sxs-lookup"><span data-stu-id="23c60-313">SNI support requires:</span></span>

* <span data-ttu-id="23c60-314">Exécution sur le framework cible `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="23c60-314">Running on target framework `netcoreapp2.1`.</span></span> <span data-ttu-id="23c60-315">Sur `netcoreapp2.0` et `net461`, le rappel est appelé, mais `name` est toujours `null`.</span><span class="sxs-lookup"><span data-stu-id="23c60-315">On `netcoreapp2.0` and `net461`, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="23c60-316">`name` est également `null` si le client ne fournit pas le paramètre du nom d’hôte dans la négociation TLS.</span><span class="sxs-lookup"><span data-stu-id="23c60-316">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="23c60-317">Tous les sites web s’exécutent sur la même instance Kestrel.</span><span class="sxs-lookup"><span data-stu-id="23c60-317">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="23c60-318">Kestrel ne prend pas en charge le partage d’une adresse IP et d’un port entre plusieurs instances sans un proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="23c60-318">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.ListenAnyIP(5005, listenOptions =>
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

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, options) =>
        {
            options.ListenAnyIP(5005, listenOptions =>
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

::: moniker-end

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="23c60-319">Lier à un socket TCP</span><span class="sxs-lookup"><span data-stu-id="23c60-319">Bind to a TCP socket</span></span>

<span data-ttu-id="23c60-320">La méthode [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) se lie à un socket TCP, et une expression lambda options permet la configuration d’un certificat X.509 :</span><span class="sxs-lookup"><span data-stu-id="23c60-320">The [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Listen(IPAddress.Loopback, 5000);
            options.Listen(IPAddress.Loopback, 5001, listenOptions =>
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
        .UseKestrel(options =>
        {
            options.Listen(IPAddress.Loopback, 5000);
            options.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

::: moniker-end

<span data-ttu-id="23c60-321">L’exemple configure HTTPS pour un point de terminaison avec [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span><span class="sxs-lookup"><span data-stu-id="23c60-321">The example configures HTTPS for an endpoint with [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span></span> <span data-ttu-id="23c60-322">Utilisez la même API afin de configurer d’autres paramètres Kestrel pour des points de terminaison spécifiques.</span><span class="sxs-lookup"><span data-stu-id="23c60-322">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="23c60-323">Lier à un socket Unix</span><span class="sxs-lookup"><span data-stu-id="23c60-323">Bind to a Unix socket</span></span>

<span data-ttu-id="23c60-324">Écoutez sur un socket Unix avec [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) pour améliorer les performances avec Nginx, comme illustré dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="23c60-324">Listen on a Unix socket with [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) for improved performance with Nginx, as shown in this example:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.ListenUnixSocket("/tmp/kestrel-test.sock");
            options.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testpassword");
            });
        });
```

::: moniker-end

### <a name="port-0"></a><span data-ttu-id="23c60-325">Port 0</span><span class="sxs-lookup"><span data-stu-id="23c60-325">Port 0</span></span>

<span data-ttu-id="23c60-326">Si vous spécifiez le numéro de port `0`, Kestrel se lie dynamiquement à un port disponible.</span><span class="sxs-lookup"><span data-stu-id="23c60-326">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="23c60-327">L’exemple suivant montre comment déterminer le port auquel Kestrel se lie à l’exécution :</span><span class="sxs-lookup"><span data-stu-id="23c60-327">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="23c60-328">Quand l’application est exécutée, la sortie de la fenêtre de console indique le port dynamique où l’application peut être atteinte :</span><span class="sxs-lookup"><span data-stu-id="23c60-328">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="23c60-329">Limitations</span><span class="sxs-lookup"><span data-stu-id="23c60-329">Limitations</span></span>

<span data-ttu-id="23c60-330">Configurez des points de terminaison avec les approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="23c60-330">Configure endpoints with the following approaches:</span></span>

* [<span data-ttu-id="23c60-331">UseUrls</span><span class="sxs-lookup"><span data-stu-id="23c60-331">UseUrls</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* <span data-ttu-id="23c60-332">Arguments de ligne de commande `--urls`</span><span class="sxs-lookup"><span data-stu-id="23c60-332">`--urls` command-line argument</span></span>
* <span data-ttu-id="23c60-333">La clé de configuration d’hôte `urls`</span><span class="sxs-lookup"><span data-stu-id="23c60-333">`urls` host configuration key</span></span>
* <span data-ttu-id="23c60-334">Variable d’environnement `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="23c60-334">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="23c60-335">Ces méthodes sont utiles si vous voulez que votre code fonctionne avec des serveurs autres que Kestrel.</span><span class="sxs-lookup"><span data-stu-id="23c60-335">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="23c60-336">Toutefois, soyez conscient des limitations suivantes :</span><span class="sxs-lookup"><span data-stu-id="23c60-336">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="23c60-337">HTTPS ne peut pas être utilisé avec ces approches, sauf si un certificat par défaut est fourni dans la configuration du point de terminaison HTTPS (par exemple avec la configuration de `KestrelServerOptions` ou un fichier de configuration, comme illustré plus haut dans cette rubrique).</span><span class="sxs-lookup"><span data-stu-id="23c60-337">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="23c60-338">Quand les deux approches `Listen` et `UseUrls` sont utilisées simultanément, les points de terminaison `Listen` remplacent les points de terminaison `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="23c60-338">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="23c60-339">Configuration de point de terminaison IIS</span><span class="sxs-lookup"><span data-stu-id="23c60-339">IIS endpoint configuration</span></span>

<span data-ttu-id="23c60-340">Quand vous utilisez IIS, les liaisons d’URL pour IIS remplacent les liaisons qui sont définies par `Listen` ou par `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="23c60-340">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="23c60-341">Pour plus d’informations, consultez la rubrique [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="23c60-341">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="listenoptionsprotocols"></a><span data-ttu-id="23c60-342">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="23c60-342">ListenOptions.Protocols</span></span>

<span data-ttu-id="23c60-343">La propriété `Protocols` établit les protocoles HTTP (`HttpProtocols`) activés sur un point de terminaison de connexion ou pour le serveur.</span><span class="sxs-lookup"><span data-stu-id="23c60-343">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="23c60-344">Affectez une valeur à la propriété `Protocols` à partir de l’énumération `HttpProtocols`.</span><span class="sxs-lookup"><span data-stu-id="23c60-344">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="23c60-345">Valeur enum `HttpProtocols`</span><span class="sxs-lookup"><span data-stu-id="23c60-345">`HttpProtocols` enum value</span></span> | <span data-ttu-id="23c60-346">Protocole de connexion autorisé</span><span class="sxs-lookup"><span data-stu-id="23c60-346">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="23c60-347">HTTP/1.1 uniquement.</span><span class="sxs-lookup"><span data-stu-id="23c60-347">HTTP/1.1 only.</span></span> <span data-ttu-id="23c60-348">Peut être utilisé avec ou sans TLS.</span><span class="sxs-lookup"><span data-stu-id="23c60-348">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="23c60-349">HTTP/2 uniquement.</span><span class="sxs-lookup"><span data-stu-id="23c60-349">HTTP/2 only.</span></span> <span data-ttu-id="23c60-350">Principalement utilisé avec TLS.</span><span class="sxs-lookup"><span data-stu-id="23c60-350">Primarily used with TLS.</span></span> <span data-ttu-id="23c60-351">Peut être utilisé sans TLS, uniquement si le client prend en charge un [mode de connaissance préalable (Prior Knowledge)](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="23c60-351">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="23c60-352">HTTP/1.1 et HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="23c60-352">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="23c60-353">Nécessite une connexion TLS et [ALPN (Application-Layer Protocol Negotiation)](https://tools.ietf.org/html/rfc7301#section-3) pour négocier HTTP/2 ; sinon, la connexion par défaut est HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="23c60-353">Requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection to negotiate HTTP/2; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="23c60-354">Le protocole par défaut est HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="23c60-354">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="23c60-355">Restrictions TLS pour HTTP/2 :</span><span class="sxs-lookup"><span data-stu-id="23c60-355">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="23c60-356">TLS version 1.2 ou ultérieure</span><span class="sxs-lookup"><span data-stu-id="23c60-356">TLS version 1.2 or later</span></span>
* <span data-ttu-id="23c60-357">Renégociation désactivée</span><span class="sxs-lookup"><span data-stu-id="23c60-357">Renegotiation disabled</span></span>
* <span data-ttu-id="23c60-358">Compression désactivée</span><span class="sxs-lookup"><span data-stu-id="23c60-358">Compression disabled</span></span>
* <span data-ttu-id="23c60-359">Tailles minimales de l’échange de clé éphémère :</span><span class="sxs-lookup"><span data-stu-id="23c60-359">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="23c60-360">Diffie-Hellman à courbe elliptique (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span><span class="sxs-lookup"><span data-stu-id="23c60-360">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="23c60-361">Diffie-Hellman à champ fini (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2 048 bits minimum</span><span class="sxs-lookup"><span data-stu-id="23c60-361">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="23c60-362">Suite de chiffrement non inscrite sur liste rouge</span><span class="sxs-lookup"><span data-stu-id="23c60-362">Cipher suite not blacklisted</span></span>

<span data-ttu-id="23c60-363">Prise en charge par défaut de `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; avec la courbe elliptique P-256 &lbrack;`FIPS186`&rbrack;.</span><span class="sxs-lookup"><span data-stu-id="23c60-363">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="23c60-364">L’exemple suivant autorise les connexions HTTP/1.1 et HTTP/2 sur le port 8000.</span><span class="sxs-lookup"><span data-stu-id="23c60-364">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="23c60-365">Les connexions sont sécurisées par TLS avec un certificat fourni :</span><span class="sxs-lookup"><span data-stu-id="23c60-365">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
            {
                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

<span data-ttu-id="23c60-366">Le cas échéant, créez une implémentation `IConnectionAdapter` pour filtrer les négociations TLS par connexion pour des chiffrements spécifiques :</span><span class="sxs-lookup"><span data-stu-id="23c60-366">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
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

        // Throw NotSupportedException for any cipher algorithm that you don't 
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

<span data-ttu-id="23c60-367">*Définir le protocole à partir de la configuration*</span><span class="sxs-lookup"><span data-stu-id="23c60-367">*Set the protocol from configuration*</span></span>

<span data-ttu-id="23c60-368">Par défaut, [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) appelle `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` pour charger la configuration de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="23c60-368">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="23c60-369">Dans l’exemple *appsettings.json* suivant, un protocole de connexion par défaut (HTTP/1.1 et HTTP/2) est établi pour l’ensemble des points de terminaison de Kestrel :</span><span class="sxs-lookup"><span data-stu-id="23c60-369">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndPointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="23c60-370">L’exemple de fichier de configuration suivant établit un protocole de connexion pour un point de terminaison spécifique :</span><span class="sxs-lookup"><span data-stu-id="23c60-370">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

```json
{
  "Kestrel": {
    "EndPoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1AndHttp2"
      }
    }
  }
}
```

<span data-ttu-id="23c60-371">Les protocoles spécifiés dans le code remplacent les valeurs définies par configuration.</span><span class="sxs-lookup"><span data-stu-id="23c60-371">Protocols specified in code override values set by configuration.</span></span>

::: moniker-end

## <a name="transport-configuration"></a><span data-ttu-id="23c60-372">Configuration du transport</span><span class="sxs-lookup"><span data-stu-id="23c60-372">Transport configuration</span></span>

<span data-ttu-id="23c60-373">Dans ASP.NET Core 2.1, le transport par défaut de Kestrel n’est plus basé sur Libuv, mais sur des sockets managés.</span><span class="sxs-lookup"><span data-stu-id="23c60-373">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="23c60-374">Il s’agit d’un changement cassant pour les applications ASP.NET Core 2.0 mises à niveau vers la version 2.1 qui appellent [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) et qui dépendent d’un des packages suivants :</span><span class="sxs-lookup"><span data-stu-id="23c60-374">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) and depend on either of the following packages:</span></span>

* <span data-ttu-id="23c60-375">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (référence de package directe)</span><span class="sxs-lookup"><span data-stu-id="23c60-375">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="23c60-376">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="23c60-376">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="23c60-377">Pour les projets ASP.NET Core 2.1 ou ultérieur qui utilisent le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) et nécessitent l’utilisation de Libuv :</span><span class="sxs-lookup"><span data-stu-id="23c60-377">For ASP.NET Core 2.1 or later projects that use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and require the use of Libuv:</span></span>

* <span data-ttu-id="23c60-378">Ajoutez une dépendance pour le package [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) au fichier projet de l’application :</span><span class="sxs-lookup"><span data-stu-id="23c60-378">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="<LATEST_VERSION>" />
    ```

* <span data-ttu-id="23c60-379">Appelez [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) :</span><span class="sxs-lookup"><span data-stu-id="23c60-379">Call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="23c60-380">Préfixes d’URL</span><span class="sxs-lookup"><span data-stu-id="23c60-380">URL prefixes</span></span>

<span data-ttu-id="23c60-381">Quand vous utilisez `UseUrls`, l’argument de ligne de commande `--urls`, la clé de configuration d’hôte `urls` ou la variable d’environnement `ASPNETCORE_URLS`, les préfixes d’URL peuvent être dans un des formats suivants.</span><span class="sxs-lookup"><span data-stu-id="23c60-381">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="23c60-382">Seuls les préfixes d’URL HTTP sont valides.</span><span class="sxs-lookup"><span data-stu-id="23c60-382">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="23c60-383">Kestrel ne prend pas en charge HTTPS lors de la configuration de liaisons d’URL avec `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="23c60-383">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="23c60-384">Adresse IPv4 avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="23c60-384">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="23c60-385">`0.0.0.0` est un cas spécial qui se lie à toutes les adresses IPv4.</span><span class="sxs-lookup"><span data-stu-id="23c60-385">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="23c60-386">Adresse IPv6 avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="23c60-386">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="23c60-387">`[::]` est l’équivalent IPv6 de `0.0.0.0` dans IPv4.</span><span class="sxs-lookup"><span data-stu-id="23c60-387">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="23c60-388">Nom d’hôte avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="23c60-388">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="23c60-389">Les noms d’hôte, `*` et `+` ne sont pas spéciaux.</span><span class="sxs-lookup"><span data-stu-id="23c60-389">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="23c60-390">Tout ce qui n’est pas reconnu comme adresse IP valide ou `localhost` se lie à toutes les adresses IP IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="23c60-390">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="23c60-391">Pour lier différents noms d’hôte à différentes applications ASP.NET Core sur le même port, utilisez [HTTP.sys](xref:fundamentals/servers/httpsys) ou un serveur proxy inverse, comme IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="23c60-391">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="23c60-392">L’hébergement dans une configuration de proxy inverse nécessite le [filtrage d’hôte](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="23c60-392">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="23c60-393">Nom `localhost` de l’hôte avec numéro de port ou adresse IP de bouclage avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="23c60-393">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="23c60-394">Quand `localhost` est spécifié, Kestrel tente de se lier aux interfaces de bouclage IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="23c60-394">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="23c60-395">Si le port demandé est en cours d’utilisation par un autre service sur l’une des interfaces de bouclage, Kestrel ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="23c60-395">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="23c60-396">Si l’une des interfaces de bouclage n’est pas disponible pour une raison quelconque (généralement du fait de la non-prise en charge d’IPv6), Kestrel journalise un avertissement.</span><span class="sxs-lookup"><span data-stu-id="23c60-396">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="23c60-397">Filtrage d’hôtes</span><span class="sxs-lookup"><span data-stu-id="23c60-397">Host filtering</span></span>

<span data-ttu-id="23c60-398">Si Kestrel prend en charge la configuration basée sur les préfixes, comme `http://example.com:5000`, il ignore en grande partie le nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="23c60-398">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="23c60-399">L’hôte `localhost` est un cas spécial utilisé pour la liaison à des adresses de bouclage.</span><span class="sxs-lookup"><span data-stu-id="23c60-399">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="23c60-400">Tout hôte autre qu’une adresse IP explicite se lie à toutes les adresses IP publiques.</span><span class="sxs-lookup"><span data-stu-id="23c60-400">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="23c60-401">Les en-têtes `Host` ne sont pas validés.</span><span class="sxs-lookup"><span data-stu-id="23c60-401">`Host` headers aren't validated.</span></span>

<span data-ttu-id="23c60-402">En guise de solution de contournement, utilisez le middleware de filtrage d’hôtes.</span><span class="sxs-lookup"><span data-stu-id="23c60-402">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="23c60-403">Le middleware de filtrage d’hôtes est fourni par le package [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering), qui est inclus dans le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 ou version ultérieure).</span><span class="sxs-lookup"><span data-stu-id="23c60-403">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="23c60-404">Le middleware est ajouté par [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), qui appelle [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering) :</span><span class="sxs-lookup"><span data-stu-id="23c60-404">The middleware is added by [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="23c60-405">Le middleware de filtrage d’hôtes est désactivé par défaut.</span><span class="sxs-lookup"><span data-stu-id="23c60-405">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="23c60-406">Pour activer le middleware, définissez une clé `AllowedHosts` dans *appsettings.json*/*appsettings.\<nom_environnement>.json*.</span><span class="sxs-lookup"><span data-stu-id="23c60-406">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="23c60-407">La valeur est une liste délimitée par des points-virgules des noms d’hôte sans numéros de port :</span><span class="sxs-lookup"><span data-stu-id="23c60-407">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="23c60-408">*appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="23c60-408">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="23c60-409">Le [middleware des en-têtes transférés ](xref:host-and-deploy/proxy-load-balancer) a également une option [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts).</span><span class="sxs-lookup"><span data-stu-id="23c60-409">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) option.</span></span> <span data-ttu-id="23c60-410">Le middleware des en-têtes transférés et le middleware de filtrage d’hôtes ont des fonctionnalités similaires pour différents scénarios.</span><span class="sxs-lookup"><span data-stu-id="23c60-410">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="23c60-411">La définition d’`AllowedHosts` avec le middleware des en-têtes transférés est appropriée quand l’en-tête `Host` n’est pas conservé durant le transfert des requêtes avec un serveur proxy inverse ou un équilibreur de charge.</span><span class="sxs-lookup"><span data-stu-id="23c60-411">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="23c60-412">La définition d’`AllowedHosts` avec le middleware de filtrage d’hôtes est appropriée quand Kestrel est utilisé comme serveur de périphérie public ou que l’en-tête `Host` est directement transféré.</span><span class="sxs-lookup"><span data-stu-id="23c60-412">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="23c60-413">Pour plus d’informations sur le middleware des en-têtes transférés, consultez <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="23c60-413">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="23c60-414">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="23c60-414">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="23c60-415">Code source Kestrel</span><span class="sxs-lookup"><span data-stu-id="23c60-415">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)
* [<span data-ttu-id="23c60-416">Document RFC 7230 : syntaxe et routage des messages (section 5.4 : Hôte)</span><span class="sxs-lookup"><span data-stu-id="23c60-416">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
