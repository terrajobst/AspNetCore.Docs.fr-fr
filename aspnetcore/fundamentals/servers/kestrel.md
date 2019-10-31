---
title: Implémentation du serveur web Kestrel dans ASP.NET Core
author: guardrex
description: Découvrez plus d’informations sur Kestrel, serveur web multiplateforme pour ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/29/2019
uid: fundamentals/servers/kestrel
ms.openlocfilehash: beaf6ac49359adfdc2dc24221eab04cc853646a9
ms.sourcegitcommit: de0fc77487a4d342bcc30965ec5c142d10d22c03
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73143439"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="cb935-103">Implémentation du serveur web Kestrel dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cb935-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="cb935-104">Par [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), [Stephen Halter](https://twitter.com/halter73)et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="cb935-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), [Stephen Halter](https://twitter.com/halter73), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cb935-105">Kestrel est un [serveur web multiplateforme pour ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="cb935-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="cb935-106">Kestrel est le serveur web inclus par défaut dans les modèles de projets ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cb935-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="cb935-107">Kestrel prend en charge les scénarios suivants :</span><span class="sxs-lookup"><span data-stu-id="cb935-107">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="cb935-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="cb935-108">HTTPS</span></span>
* <span data-ttu-id="cb935-109">Mise à niveau opaque utilisée pour activer les [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="cb935-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="cb935-110">Sockets UNIX pour des performances élevées derrière Nginx</span><span class="sxs-lookup"><span data-stu-id="cb935-110">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="cb935-111">HTTP/2 (sauf sur macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="cb935-111">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="cb935-112">&dagger;HTTP/2 sera pris en charge sur macOS dans une prochaine version.</span><span class="sxs-lookup"><span data-stu-id="cb935-112">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="cb935-113">Kestrel est pris en charge sur toutes les plateformes et les versions prises en charge par .NET Core.</span><span class="sxs-lookup"><span data-stu-id="cb935-113">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="cb935-114">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cb935-114">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="cb935-115">Prise en charge de HTTP/2</span><span class="sxs-lookup"><span data-stu-id="cb935-115">HTTP/2 support</span></span>

<span data-ttu-id="cb935-116">[HTTP/2](https://httpwg.org/specs/rfc7540.html) est disponible pour les applications ASP.NET Core si les conditions de base suivantes sont remplies :</span><span class="sxs-lookup"><span data-stu-id="cb935-116">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="cb935-117">Système d’exploitation&dagger;</span><span class="sxs-lookup"><span data-stu-id="cb935-117">Operating system&dagger;</span></span>
  * <span data-ttu-id="cb935-118">Windows Server 2016/Windows 10 ou version ultérieure&Dagger;</span><span class="sxs-lookup"><span data-stu-id="cb935-118">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="cb935-119">Linux avec OpenSSL 1.0.2 ou version ultérieure (par exemple,Ubuntu 16.04 ou version ultérieure)</span><span class="sxs-lookup"><span data-stu-id="cb935-119">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="cb935-120">Version cible de .Net Framework : .NET Core 2.2 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="cb935-120">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="cb935-121">Connexion [ALPN (Application-Layer Protocol Negotiation)](https://tools.ietf.org/html/rfc7301#section-3)</span><span class="sxs-lookup"><span data-stu-id="cb935-121">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="cb935-122">TLS 1.2 ou connexion ultérieure</span><span class="sxs-lookup"><span data-stu-id="cb935-122">TLS 1.2 or later connection</span></span>

<span data-ttu-id="cb935-123">&dagger;HTTP/2 sera pris en charge sur macOS dans une prochaine version.</span><span class="sxs-lookup"><span data-stu-id="cb935-123">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="cb935-124">&Dagger;Kestrel propose une prise en charge limitée de HTTP/2 sous Windows Server 2012 R2 et Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="cb935-124">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="cb935-125">La prise en charge est limitée car la liste des suites de chiffrement TLS prises en charge sur ces systèmes d’exploitation est limitée.</span><span class="sxs-lookup"><span data-stu-id="cb935-125">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="cb935-126">Un certificat généré à l’aide d’Elliptic Curve Digital Signature algorithme (ECDSA) peut être requis pour sécuriser les connexions TLS.</span><span class="sxs-lookup"><span data-stu-id="cb935-126">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="cb935-127">Si une connexion HTTP/2 est établie, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) retourne `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="cb935-127">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="cb935-128">HTTP/2 est désactivé par défaut.</span><span class="sxs-lookup"><span data-stu-id="cb935-128">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="cb935-129">Pour plus d’informations sur la configuration, consultez les sections [Options Kestrel](#kestrel-options) et [ListenOptions.Protocols](#listenoptionsprotocols).</span><span class="sxs-lookup"><span data-stu-id="cb935-129">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="cb935-130">Quand utiliser Kestrel avec un proxy inverse ?</span><span class="sxs-lookup"><span data-stu-id="cb935-130">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="cb935-131">Vous pouvez utiliser Kestrel par lui-même ou avec un *serveur proxy inverse*, comme [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="cb935-131">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="cb935-132">Un serveur proxy inverse reçoit les requêtes HTTP en provenance du réseau et les transmet à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-132">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="cb935-133">Kestrel utilisé comme serveur web edge (accessible sur Internet) :</span><span class="sxs-lookup"><span data-stu-id="cb935-133">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel communique directement avec Internet sans serveur proxy inverse](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="cb935-135">Kestrel utilisé dans une configuration de proxy inverse :</span><span class="sxs-lookup"><span data-stu-id="cb935-135">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="cb935-137">L’une ou l’autre des configurations, avec ou sans serveur proxy inverse, est une configuration d’hébergement prise en charge.</span><span class="sxs-lookup"><span data-stu-id="cb935-137">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="cb935-138">Kestrel, s’il est utilisé comme serveur de périphérie sans serveur proxy inverse, ne prend pas en charge le partage de la même adresse IP et du même port entre plusieurs processus.</span><span class="sxs-lookup"><span data-stu-id="cb935-138">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="cb935-139">Quand Kestrel est configuré pour écouter sur un port, il gère tout le trafic pour ce port, quel que soit les en-têtes `Host` des requêtes.</span><span class="sxs-lookup"><span data-stu-id="cb935-139">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="cb935-140">Un proxy inverse qui peut partager des ports a la possibilité de transférer des requêtes à Kestrel sur une adresse IP et un port uniques.</span><span class="sxs-lookup"><span data-stu-id="cb935-140">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="cb935-141">Même si un serveur proxy inverse n’est pas nécessaire, en utiliser un peut être un bon choix.</span><span class="sxs-lookup"><span data-stu-id="cb935-141">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="cb935-142">Un proxy inverse :</span><span class="sxs-lookup"><span data-stu-id="cb935-142">A reverse proxy:</span></span>

* <span data-ttu-id="cb935-143">Peut limiter la surface publique exposée des applications qu’il héberge.</span><span class="sxs-lookup"><span data-stu-id="cb935-143">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="cb935-144">Fournit une couche supplémentaire de configuration et de défense.</span><span class="sxs-lookup"><span data-stu-id="cb935-144">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="cb935-145">Peut mieux s’intégrer à l’infrastructure existante.</span><span class="sxs-lookup"><span data-stu-id="cb935-145">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="cb935-146">Simplifie la configuration de l’équilibrage de charge et d’une communication sécurisée (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="cb935-146">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="cb935-147">Seul le serveur proxy inverse requiert un certificat X. 509 et ce serveur peut communiquer avec les serveurs de l’application sur le réseau interne à l’aide du protocole HTTP simple.</span><span class="sxs-lookup"><span data-stu-id="cb935-147">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="cb935-148">L’hébergement dans une configuration de proxy inverse nécessite le [filtrage d’hôte](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="cb935-148">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="cb935-149">Comment utiliser Kestrel dans les applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cb935-149">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="cb935-150">Les modèles de projet ASP.NET Core utilisent Kestrel par défaut.</span><span class="sxs-lookup"><span data-stu-id="cb935-150">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="cb935-151">Dans *Program.cs*, le modèle de code appelle <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>, qui appelle <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="cb935-151">In *Program.cs*, the template code calls <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="cb935-152">Pour plus d’informations sur la `CreateDefaultBuilder` et la génération de l’hôte, consultez les sections configurer les paramètres de l' *ordinateur hôte* et du *générateur par défaut* de <xref:fundamentals/host/generic-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="cb935-152">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* and *Default builder settings* sections of <xref:fundamentals/host/generic-host#set-up-a-host>.</span></span>

<span data-ttu-id="cb935-153">Pour fournir une configuration supplémentaire après l’appel de `CreateDefaultBuilder` et `ConfigureWebHostDefaults`, utilisez `ConfigureKestrel` :</span><span class="sxs-lookup"><span data-stu-id="cb935-153">To provide additional configuration after calling `CreateDefaultBuilder` and `ConfigureWebHostDefaults`, use `ConfigureKestrel`:</span></span>

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

<span data-ttu-id="cb935-154">Si l’application n’appelle pas `CreateDefaultBuilder` pour configurer l’hôte, appelez <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **avant** d’appeler `ConfigureKestrel` :</span><span class="sxs-lookup"><span data-stu-id="cb935-154">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="cb935-155">Options Kestrel</span><span class="sxs-lookup"><span data-stu-id="cb935-155">Kestrel options</span></span>

<span data-ttu-id="cb935-156">Le serveur web Kestrel a des options de configuration de contrainte qui sont particulièrement utiles dans les déploiements exposés à Internet.</span><span class="sxs-lookup"><span data-stu-id="cb935-156">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="cb935-157">Définissez des contraintes sur la propriété <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> de la classe <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="cb935-157">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="cb935-158">La propriété `Limits` conserve une instance de la classe <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="cb935-158">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="cb935-159">Les exemples suivants utilisent l’espace de noms <xref:Microsoft.AspNetCore.Server.Kestrel.Core> :</span><span class="sxs-lookup"><span data-stu-id="cb935-159">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="cb935-160">Les options Kestrel, qui sont configurées dans C# le code des exemples suivants, peuvent également être définies à l’aide d’un fournisseur de [configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="cb935-160">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="cb935-161">Par exemple, le fournisseur de configuration de fichier peut charger la configuration Kestrel à partir d’un fichier *appSettings. JSON* ou *appSettings. { Fichier Environment}. JSON* :</span><span class="sxs-lookup"><span data-stu-id="cb935-161">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

```json
{
  "Kestrel": {
    "Limits": {
      "MaxConcurrentConnections": 100,
      "MaxConcurrentUpgradedConnections": 100
    },
    "DisableStringReuse": true
  }
}
```

<span data-ttu-id="cb935-162">Utilisez l' **une** des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="cb935-162">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="cb935-163">Configurer Kestrel dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="cb935-163">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="cb935-164">Injecte une instance de `IConfiguration` dans la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="cb935-164">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="cb935-165">L’exemple suivant suppose que la configuration injectée est assignée à la propriété `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="cb935-165">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="cb935-166">Dans `Startup.ConfigureServices`, chargez la section `Kestrel` de la configuration dans la configuration de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-166">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="cb935-167">Configurez Kestrel lors de la génération de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="cb935-167">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="cb935-168">Dans *Program.cs*, chargez la section `Kestrel` de la configuration dans la configuration de Kestrel :</span><span class="sxs-lookup"><span data-stu-id="cb935-168">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

  ```csharp
  // using Microsoft.Extensions.DependencyInjection;

  public static IHostBuilder CreateHostBuilder(string[] args) =>
      Host.CreateDefaultBuilder(args)
          .ConfigureServices((context, services) =>
          {
              services.Configure<KestrelServerOptions>(
                  context.Configuration.GetSection("Kestrel"));
          })
          .ConfigureWebHostDefaults(webBuilder =>
          {
              webBuilder.UseStartup<Startup>();
          });
  ```

<span data-ttu-id="cb935-169">Les deux approches précédentes fonctionnent avec n’importe quel [fournisseur de configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="cb935-169">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="cb935-170">Délai d’expiration toujours actif</span><span class="sxs-lookup"><span data-stu-id="cb935-170">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="cb935-171">Obtient ou définit le [délai d’expiration toujours actif](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="cb935-171">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="cb935-172">La valeur par défaut est de 2 minutes.</span><span class="sxs-lookup"><span data-stu-id="cb935-172">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=19-20)]

### <a name="maximum-client-connections"></a><span data-ttu-id="cb935-173">Nombre maximale de connexions client</span><span class="sxs-lookup"><span data-stu-id="cb935-173">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="cb935-174">Le nombre maximal de connexions TCP ouvertes simultanées peut être défini pour l’application entière avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="cb935-174">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="cb935-175">Il existe une limite distincte pour les connexions qui ont été mises à niveau à partir de HTTP ou HTTPS vers un autre protocole (par exemple, sur une demande WebSocket).</span><span class="sxs-lookup"><span data-stu-id="cb935-175">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="cb935-176">Une fois mise à niveau, une connexion n’est pas prise en compte dans la limite `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="cb935-176">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="cb935-177">Le nombre maximal de connexions est illimité (null) par défaut.</span><span class="sxs-lookup"><span data-stu-id="cb935-177">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="cb935-178">Taille maximale du corps de la requête</span><span class="sxs-lookup"><span data-stu-id="cb935-178">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="cb935-179">La taille maximale par défaut du corps de la requête est de 30 000 000 octets, soit environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="cb935-179">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="cb935-180">Pour remplacer la limite dans une application ASP.NET Core MVC, nous vous recommandons d’utiliser l’attribut <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> sur une méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="cb935-180">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="cb935-181">Voici un exemple qui montre comment configurer la contrainte pour l’application sur chaque requête :</span><span class="sxs-lookup"><span data-stu-id="cb935-181">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="cb935-182">Remplacer le paramètre sur une demande spécifique dans l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="cb935-182">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="cb935-183">Une exception est levée si l’application configure la limite sur une demande après que l’application a commencé à lire la demande.</span><span class="sxs-lookup"><span data-stu-id="cb935-183">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="cb935-184">Il existe une propriété `IsReadOnly` qui indique si la propriété `MaxRequestBodySize` est en lecture seule ; si tel est le cas, il est trop tard pour configurer la limite.</span><span class="sxs-lookup"><span data-stu-id="cb935-184">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="cb935-185">Quand une application est exécutée [hors processus](xref:host-and-deploy/iis/index#out-of-process-hosting-model) derrière le [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module), la limite de taille du corps de demande de Kestrel est désactivée, car IIS définit déjà la limite.</span><span class="sxs-lookup"><span data-stu-id="cb935-185">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="cb935-186">Débit données minimal du corps de la requête</span><span class="sxs-lookup"><span data-stu-id="cb935-186">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="cb935-187">Kestrel vérifie à chaque seconde si les données arrivent au débit spécifié en octets/seconde.</span><span class="sxs-lookup"><span data-stu-id="cb935-187">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="cb935-188">Si le taux chute au-dessous de la valeur minimale, le délai d’attente de la connexion est dépassé. La période de grâce correspond à la durée pendant laquelle Kestrel permet au client d’augmenter son taux d’envoi jusqu’à la valeur minimale. la vitesse n’est pas vérifiée pendant cette période.</span><span class="sxs-lookup"><span data-stu-id="cb935-188">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="cb935-189">La période de grâce permet d’éviter la suppression des connexions qui, initialement, envoient des données à une vitesse lente en raison de la lenteur du démarrage de TCP.</span><span class="sxs-lookup"><span data-stu-id="cb935-189">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="cb935-190">Le débit minimal par défaut est 240 octets/seconde, avec une période de grâce de 5 secondes.</span><span class="sxs-lookup"><span data-stu-id="cb935-190">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="cb935-191">Un débit minimal s’applique également à la réponse.</span><span class="sxs-lookup"><span data-stu-id="cb935-191">A minimum rate also applies to the response.</span></span> <span data-ttu-id="cb935-192">Le code pour définir les limites de demande et de réponse est identique à l’exception de `RequestBody` ou `Response` dans les noms de propriété et d’interface.</span><span class="sxs-lookup"><span data-stu-id="cb935-192">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="cb935-193">Voici un exemple qui montre comment configurer les débits de données minimaux dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="cb935-193">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-11)]

<span data-ttu-id="cb935-194">Remplacer les limites de taux minimum par demande dans l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="cb935-194">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="cb935-195">Le <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature>débit référencé dans l’exemple précédent n’est pas présent dans `HttpContext.Features` pour les requêtes HTTP/2, car la modification des limites de débit par requête n’est généralement pas prise en charge pour HTTP/2 (le protocole prend en charge le multiplexage de requête).</span><span class="sxs-lookup"><span data-stu-id="cb935-195">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> referenced in the prior sample is not present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis is generally not supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="cb935-196">Toutefois, le <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> est toujours présent `HttpContext.Features` pour les requêtes HTTP/2, car la limite de débit de lecture peut toujours être *désactivée entièrement* sur une base par demande en définissant `IHttpMinRequestBodyDataRateFeature.MinDataRate` sur `null` même pour une requête HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="cb935-196">However, the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> is still present `HttpContext.Features` for HTTP/2 requests, because the read rate limit can still be *disabled entirely* on a per-request basis by setting `IHttpMinRequestBodyDataRateFeature.MinDataRate` to `null` even for an HTTP/2 request.</span></span> <span data-ttu-id="cb935-197">Une tentative de lecture de `IHttpMinRequestBodyDataRateFeature.MinDataRate` ou une tentative de définition sur une valeur autre que `null` entraîne une levée de `NotSupportedException` selon une requête HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="cb935-197">Attempting to read `IHttpMinRequestBodyDataRateFeature.MinDataRate` or attempting to set it to a value other than `null` will result in a `NotSupportedException` being thrown given an HTTP/2 request.</span></span>

<span data-ttu-id="cb935-198">Les limites de débit à l’échelle du serveur configurées par le biais de `KestrelServerOptions.Limits` s’appliquent encore aux connexions HTTP/1.x et HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="cb935-198">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="cb935-199">Délai d’expiration des en-têtes de requête</span><span class="sxs-lookup"><span data-stu-id="cb935-199">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="cb935-200">Obtient ou définit le temps maximal passé par le serveur à recevoir des en-têtes de requête.</span><span class="sxs-lookup"><span data-stu-id="cb935-200">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="cb935-201">La valeur par défaut est de 30 secondes.</span><span class="sxs-lookup"><span data-stu-id="cb935-201">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=21-22)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="cb935-202">Flux de données maximal par connexion</span><span class="sxs-lookup"><span data-stu-id="cb935-202">Maximum streams per connection</span></span>

<span data-ttu-id="cb935-203">`Http2.MaxStreamsPerConnection` limite le nombre de flux de requête simultanée par connexion HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="cb935-203">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="cb935-204">Les flux de données excédentaires sont refusés.</span><span class="sxs-lookup"><span data-stu-id="cb935-204">Excess streams are refused.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
});
```

<span data-ttu-id="cb935-205">La valeur par défaut est 100.</span><span class="sxs-lookup"><span data-stu-id="cb935-205">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="cb935-206">Taille de la table d’en-tête</span><span class="sxs-lookup"><span data-stu-id="cb935-206">Header table size</span></span>

<span data-ttu-id="cb935-207">Le décodeur HPACK décompresse les en-têtes HTTP pour les connexions HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="cb935-207">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="cb935-208">`Http2.HeaderTableSize` limite la taille de la table de compression d’en-tête que le décodeur HPACK utilise.</span><span class="sxs-lookup"><span data-stu-id="cb935-208">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="cb935-209">La valeur est fournie en octets et doit être supérieure à zéro (0).</span><span class="sxs-lookup"><span data-stu-id="cb935-209">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.HeaderTableSize = 4096;
});
```

<span data-ttu-id="cb935-210">La valeur par défaut est 4096.</span><span class="sxs-lookup"><span data-stu-id="cb935-210">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="cb935-211">Taille de trame maximale</span><span class="sxs-lookup"><span data-stu-id="cb935-211">Maximum frame size</span></span>

<span data-ttu-id="cb935-212">`Http2.MaxFrameSize` indique la taille maximale autorisée d’une charge utile de trame de connexion HTTP/2 reçue ou envoyée par le serveur.</span><span class="sxs-lookup"><span data-stu-id="cb935-212">`Http2.MaxFrameSize` indicates the maximum allowed size of an HTTP/2 connection frame payload received or sent by the server.</span></span> <span data-ttu-id="cb935-213">La valeur est fournie en octets et doit être comprise entre 2^14 (16,384) et 2^24-1 (16,777,215).</span><span class="sxs-lookup"><span data-stu-id="cb935-213">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxFrameSize = 16384;
});
```

<span data-ttu-id="cb935-214">La valeur par défaut est 2^14 (16,384).</span><span class="sxs-lookup"><span data-stu-id="cb935-214">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="cb935-215">Taille maximale d’en-tête de requête</span><span class="sxs-lookup"><span data-stu-id="cb935-215">Maximum request header size</span></span>

<span data-ttu-id="cb935-216">`Http2.MaxRequestHeaderFieldSize` indique la taille maximale autorisée en octets des valeurs d’en-tête de requête.</span><span class="sxs-lookup"><span data-stu-id="cb935-216">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="cb935-217">Cette limite s’applique à la fois au nom et à la valeur dans leurs représentations compressées et non compressées.</span><span class="sxs-lookup"><span data-stu-id="cb935-217">This limit applies to both name and value in their compressed and uncompressed representations.</span></span> <span data-ttu-id="cb935-218">La valeur doit être supérieure à zéro (0).</span><span class="sxs-lookup"><span data-stu-id="cb935-218">The value must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
});
```

<span data-ttu-id="cb935-219">La valeur par défaut est 8 192.</span><span class="sxs-lookup"><span data-stu-id="cb935-219">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="cb935-220">Taille de fenêtre de connexion initiale</span><span class="sxs-lookup"><span data-stu-id="cb935-220">Initial connection window size</span></span>

<span data-ttu-id="cb935-221">`Http2.InitialConnectionWindowSize` indique la quantité maximale de données de corps de requête, en octets, que le serveur met en mémoire tampon à un moment donné pour toutes les requêtes (flux) par connexion.</span><span class="sxs-lookup"><span data-stu-id="cb935-221">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="cb935-222">Les requêtes sont également limitées par `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="cb935-222">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="cb935-223">La valeur doit être supérieure ou égale à 65 535 et inférieure à 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="cb935-223">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
});
```

<span data-ttu-id="cb935-224">La valeur par défaut est 128 Ko (131 072).</span><span class="sxs-lookup"><span data-stu-id="cb935-224">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="cb935-225">Taille de la fenêtre de flux initiale</span><span class="sxs-lookup"><span data-stu-id="cb935-225">Initial stream window size</span></span>

<span data-ttu-id="cb935-226">`Http2.InitialStreamWindowSize` indique la quantité maximale de données de corps de requête, en octets, que le serveur met en mémoire tampon à un moment donné par requête (flux).</span><span class="sxs-lookup"><span data-stu-id="cb935-226">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="cb935-227">Les requêtes sont également limitées par `Http2.InitialConnectionWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="cb935-227">Requests are also limited by `Http2.InitialConnectionWindowSize`.</span></span> <span data-ttu-id="cb935-228">La valeur doit être supérieure ou égale à 65 535 et inférieure à 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="cb935-228">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
});
```

<span data-ttu-id="cb935-229">La valeur par défaut est 96 Ko (98 304).</span><span class="sxs-lookup"><span data-stu-id="cb935-229">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="cb935-230">E/S synchrone</span><span class="sxs-lookup"><span data-stu-id="cb935-230">Synchronous IO</span></span>

<span data-ttu-id="cb935-231"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> contrôle si l’e/s synchrone est autorisée pour la requête et la réponse.</span><span class="sxs-lookup"><span data-stu-id="cb935-231"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="cb935-232">La valeur par défaut est `false`.</span><span class="sxs-lookup"><span data-stu-id="cb935-232">The default value is `false`.</span></span>

> [!WARNING]
> <span data-ttu-id="cb935-233">Un grand nombre d’opérations d’e/s synchrones de blocage peut entraîner une privation de pool de thread, ce qui fait que l’application ne répond pas.</span><span class="sxs-lookup"><span data-stu-id="cb935-233">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="cb935-234">Activez uniquement `AllowSynchronousIO` lors de l’utilisation d’une bibliothèque qui ne prend pas en charge l’e/s asynchrone.</span><span class="sxs-lookup"><span data-stu-id="cb935-234">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="cb935-235">L’exemple suivant active un e/s synchrone :</span><span class="sxs-lookup"><span data-stu-id="cb935-235">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="cb935-236">Pour plus d’informations sur les autres options et limites de Kestrel, consultez :</span><span class="sxs-lookup"><span data-stu-id="cb935-236">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="cb935-237">Configuration de point de terminaison</span><span class="sxs-lookup"><span data-stu-id="cb935-237">Endpoint configuration</span></span>

<span data-ttu-id="cb935-238">Par défaut, ASP.NET Core se lie à :</span><span class="sxs-lookup"><span data-stu-id="cb935-238">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="cb935-239">`https://localhost:5001` (quand un certificat de développement local est présent)</span><span class="sxs-lookup"><span data-stu-id="cb935-239">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="cb935-240">Spécifiez les URL avec :</span><span class="sxs-lookup"><span data-stu-id="cb935-240">Specify URLs using the:</span></span>

* <span data-ttu-id="cb935-241">La variable d’environnement `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="cb935-241">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="cb935-242">L’argument de ligne de commande `--urls`.</span><span class="sxs-lookup"><span data-stu-id="cb935-242">`--urls` command-line argument.</span></span>
* <span data-ttu-id="cb935-243">La clé de configuration d’hôte `urls`.</span><span class="sxs-lookup"><span data-stu-id="cb935-243">`urls` host configuration key.</span></span>
* <span data-ttu-id="cb935-244">La méthode d’extension `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="cb935-244">`UseUrls` extension method.</span></span>

<span data-ttu-id="cb935-245">La valeur fournie avec ces approches peut être un ou plusieurs points de terminaison HTTP et HTTPS (HTTPS si un certificat par défaut est disponible).</span><span class="sxs-lookup"><span data-stu-id="cb935-245">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="cb935-246">Configurez la valeur sous forme de liste délimitée par des points-virgules (par exemple `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="cb935-246">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="cb935-247">Pour plus d’informations sur ces approches, voir [URL de serveur](xref:fundamentals/host/web-host#server-urls) et [Remplacer la configuration](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="cb935-247">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="cb935-248">Un certificat de développement est créé :</span><span class="sxs-lookup"><span data-stu-id="cb935-248">A development certificate is created:</span></span>

* <span data-ttu-id="cb935-249">Quand le [SDK .NET Core](/dotnet/core/sdk) est installé.</span><span class="sxs-lookup"><span data-stu-id="cb935-249">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="cb935-250">[L’outil dev-certs](xref:aspnetcore-2.1#https) est utilisé pour créer un certificat.</span><span class="sxs-lookup"><span data-stu-id="cb935-250">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="cb935-251">Certains navigateurs requièrent l’octroi d’une autorisation explicite pour approuver le certificat de développement local.</span><span class="sxs-lookup"><span data-stu-id="cb935-251">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="cb935-252">Les modèles de projet configurent les applications à exécuter sur HTTPs par défaut et incluent [la redirection https et la prise en charge de HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="cb935-252">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="cb935-253">Appelez les méthodes <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> ou <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> sur <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> pour configurer les préfixes et les ports d’URL pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-253">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="cb935-254">`UseUrls`, l’argument de ligne de commande `--urls`, la clé de configuration d’hôte `urls` et la variable d’environnement `ASPNETCORE_URLS` fonctionnent également, mais ils présentent les limitations indiquées plus loin dans cette section (un certificat par défaut doit être disponible pour la configuration du point de terminaison HTTPS).</span><span class="sxs-lookup"><span data-stu-id="cb935-254">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="cb935-255">configuration de `KestrelServerOptions` :</span><span class="sxs-lookup"><span data-stu-id="cb935-255">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="cb935-256">ConfigureEndpointDefaults (action\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="cb935-256">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="cb935-257">Spécifie une `Action` de configuration à exécuter pour chaque point de terminaison spécifié.</span><span class="sxs-lookup"><span data-stu-id="cb935-257">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="cb935-258">Le fait d’appeler `ConfigureEndpointDefaults` plusieurs fois remplace les `Action`s précédentes par la dernière `Action` spécifiée.</span><span class="sxs-lookup"><span data-stu-id="cb935-258">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureEndpointDefaults(listenOptions =>
    {
        // Configure endpoint defaults
    });
});
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="cb935-259">ConfigureHttpsDefaults (action\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="cb935-259">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="cb935-260">Spécifie une `Action` de configuration à exécuter pour chaque point de terminaison HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cb935-260">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="cb935-261">Le fait d’appeler `ConfigureHttpsDefaults` plusieurs fois remplace les `Action`s précédentes par la dernière `Action` spécifiée.</span><span class="sxs-lookup"><span data-stu-id="cb935-261">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureHttpsDefaults(listenOptions =>
    {
        // certificate is an X509Certificate2
        listenOptions.ServerCertificate = certificate;
    });
});
```

### <a name="configureiconfiguration"></a><span data-ttu-id="cb935-262">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="cb935-262">Configure(IConfiguration)</span></span>

<span data-ttu-id="cb935-263">Crée un chargeur de configuration pour configurer Kestrel, qui prend en entrée une <xref:Microsoft.Extensions.Configuration.IConfiguration>.</span><span class="sxs-lookup"><span data-stu-id="cb935-263">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="cb935-264">La configuration doit être limitée à la section de configuration pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-264">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="cb935-265">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="cb935-265">ListenOptions.UseHttps</span></span>

<span data-ttu-id="cb935-266">Configure Kestrel pour l’utilisation de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cb935-266">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="cb935-267">Extensions de `ListenOptions.UseHttps` :</span><span class="sxs-lookup"><span data-stu-id="cb935-267">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="cb935-268">`UseHttps` &ndash; Configure Kestrel pour l’utilisation de HTTPS avec le certificat par défaut.</span><span class="sxs-lookup"><span data-stu-id="cb935-268">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="cb935-269">Lève une exception si aucun certificat par défaut n’est configuré.</span><span class="sxs-lookup"><span data-stu-id="cb935-269">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="cb935-270">Paramètres de `ListenOptions.UseHttps` :</span><span class="sxs-lookup"><span data-stu-id="cb935-270">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="cb935-271">`filename` est le chemin et le nom d’un fichier de certificat, relatif au répertoire qui contient les fichiers de contenu de l’application.</span><span class="sxs-lookup"><span data-stu-id="cb935-271">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="cb935-272">`password` est le mot de passe nécessaire pour accéder aux données du certificat X.509.</span><span class="sxs-lookup"><span data-stu-id="cb935-272">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="cb935-273">`configureOptions` est une `Action` pour configurer les `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="cb935-273">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="cb935-274">Retourne l'`ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="cb935-274">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="cb935-275">`storeName` est le magasin de certificats à partir duquel charger le certificat.</span><span class="sxs-lookup"><span data-stu-id="cb935-275">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="cb935-276">`subject` est le nom du sujet du certificat.</span><span class="sxs-lookup"><span data-stu-id="cb935-276">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="cb935-277">`allowInvalid` indique si les certificats non valides doivent être considérés, comme les certificats auto-signés.</span><span class="sxs-lookup"><span data-stu-id="cb935-277">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="cb935-278">`location` est l’emplacement du magasin à partir duquel charger le certificat.</span><span class="sxs-lookup"><span data-stu-id="cb935-278">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="cb935-279">`serverCertificate` est le certificat X.509.</span><span class="sxs-lookup"><span data-stu-id="cb935-279">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="cb935-280">En production, HTTPS doit être explicitement configuré.</span><span class="sxs-lookup"><span data-stu-id="cb935-280">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="cb935-281">Au minimum, un certificat par défaut doit être fourni.</span><span class="sxs-lookup"><span data-stu-id="cb935-281">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="cb935-282">Configurations prises en charge décrites dans la suite :</span><span class="sxs-lookup"><span data-stu-id="cb935-282">Supported configurations described next:</span></span>

* <span data-ttu-id="cb935-283">Pas de configuration</span><span class="sxs-lookup"><span data-stu-id="cb935-283">No configuration</span></span>
* <span data-ttu-id="cb935-284">Remplacer le certificat par défaut dans la configuration</span><span class="sxs-lookup"><span data-stu-id="cb935-284">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="cb935-285">Changer les valeurs par défaut dans le code</span><span class="sxs-lookup"><span data-stu-id="cb935-285">Change the defaults in code</span></span>

<span data-ttu-id="cb935-286">*Pas de configuration*</span><span class="sxs-lookup"><span data-stu-id="cb935-286">*No configuration*</span></span>

<span data-ttu-id="cb935-287">Kestrel écoute sur `http://localhost:5000` et sur `https://localhost:5001` (si un certificat par défaut est disponible).</span><span class="sxs-lookup"><span data-stu-id="cb935-287">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="cb935-288">*Remplacer le certificat par défaut dans la configuration*</span><span class="sxs-lookup"><span data-stu-id="cb935-288">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="cb935-289">Par défaut, `CreateDefaultBuilder` appelle `Configure(context.Configuration.GetSection("Kestrel"))` pour charger la configuration de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-289">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="cb935-290">Un schéma de configuration des paramètres d’application HTTPS par défaut est disponible pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-290">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="cb935-291">Configurez plusieurs points de terminaison, notamment les URL et les certificats à utiliser, à partir d’un fichier sur disque ou d’un magasin de certificats.</span><span class="sxs-lookup"><span data-stu-id="cb935-291">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="cb935-292">Dans l’exemple de fichier *appsettings.json* suivant :</span><span class="sxs-lookup"><span data-stu-id="cb935-292">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="cb935-293">Définissez **AllowInvalid** sur `true` pour permettre l’utilisation de certificats non valides (par exemple des certificats auto-signés).</span><span class="sxs-lookup"><span data-stu-id="cb935-293">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="cb935-294">Les points de terminaison HTTPS qui ne spécifient pas de certificat (**HttpsDefaultCert** dans l’exemple qui suit) utilise comme alternative de secours le certificat défini sous **Certificats** > **Par défaut**  ou bien le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="cb935-294">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="cb935-295">Une alternative à l’utilisation de **Chemin** et de **Mot de passe** pour un nœud de certificat consiste à spécifier le certificat avec des champs du magasin de certificats.</span><span class="sxs-lookup"><span data-stu-id="cb935-295">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="cb935-296">Par exemple, le certificat **Certificats** > **Par défaut** peut être spécifié en tant que :</span><span class="sxs-lookup"><span data-stu-id="cb935-296">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="cb935-297">Notes de schéma :</span><span class="sxs-lookup"><span data-stu-id="cb935-297">Schema notes:</span></span>

* <span data-ttu-id="cb935-298">Les noms des points de terminaison ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="cb935-298">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="cb935-299">Par exemple, `HTTPS` et `Https` sont valides.</span><span class="sxs-lookup"><span data-stu-id="cb935-299">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="cb935-300">Le paramètre `Url` est obligatoire pour chaque point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="cb935-300">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="cb935-301">Le format de ce paramètre est le même que celui du paramètre de configuration `Urls` du plus haut niveau, sauf qu’il est limité à une seule valeur.</span><span class="sxs-lookup"><span data-stu-id="cb935-301">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="cb935-302">Ces points de terminaison remplacent ceux qui sont définis dans le paramètre de configuration `Urls` du plus haut niveau configuration, au lieu de s’y ajouter.</span><span class="sxs-lookup"><span data-stu-id="cb935-302">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="cb935-303">Les points de terminaison définis dans le code via `Listen` sont cumulatifs avec les points de terminaison définis dans la section de configuration.</span><span class="sxs-lookup"><span data-stu-id="cb935-303">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="cb935-304">La section `Certificate` est facultative.</span><span class="sxs-lookup"><span data-stu-id="cb935-304">The `Certificate` section is optional.</span></span> <span data-ttu-id="cb935-305">Si la section `Certificate` n’est pas spécifiée, les valeurs par défaut définies dans les scénarios précédents sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="cb935-305">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="cb935-306">Si aucune valeur par défaut n’est disponible, le serveur lève une exception et son démarrage échoue.</span><span class="sxs-lookup"><span data-stu-id="cb935-306">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="cb935-307">La section `Certificate` prend en charge les certificats **Chemin**&ndash;**Mot de passe** et **Sujet**&ndash;**Magasin**.</span><span class="sxs-lookup"><span data-stu-id="cb935-307">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="cb935-308">Vous pouvez définir un nombre quelconque de points de terminaison de cette façon, pour autant qu’ils ne provoquent pas de conflits de port.</span><span class="sxs-lookup"><span data-stu-id="cb935-308">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="cb935-309">`options.Configure(context.Configuration.GetSection("{SECTION}"))` retourne un `KestrelConfigurationLoader` avec une méthode `.Endpoint(string name, listenOptions => { })` qui peut être utilisée pour compléter les paramètres d’un point de terminaison configuré :</span><span class="sxs-lookup"><span data-stu-id="cb935-309">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
webBuilder.UseKestrel((context, serverOptions) =>
{
    serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
        .Endpoint("HTTPS", listenOptions =>
        {
            listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
        });
});
```

<span data-ttu-id="cb935-310">`KestrelServerOptions.ConfigurationLoader` est accessible directement pour poursuivre l’itération sur le chargeur existant, tel que celui fourni par <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="cb935-310">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="cb935-311">La section de configuration pour chaque point de terminaison est disponible sur les options de la méthode `Endpoint` afin que les paramètres personnalisés puissent être lus.</span><span class="sxs-lookup"><span data-stu-id="cb935-311">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="cb935-312">Plusieurs configurations peuvent être chargées en rappelant `options.Configure(context.Configuration.GetSection("{SECTION}"))` avec une autre section.</span><span class="sxs-lookup"><span data-stu-id="cb935-312">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="cb935-313">Seule la dernière configuration est utilisée, à moins que `Load` soit explicitement appelé sur les instances précédentes.</span><span class="sxs-lookup"><span data-stu-id="cb935-313">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="cb935-314">Le métapackage n’appelle pas `Load` : sa section de configuration par défaut peut donc être remplacée.</span><span class="sxs-lookup"><span data-stu-id="cb935-314">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="cb935-315">`KestrelConfigurationLoader` reflète la famille d’API `Listen` de `KestrelServerOptions` sous forme de surcharges de `Endpoint` : le code et les points de terminaison de configuration peuvent donc être configurés au même emplacement.</span><span class="sxs-lookup"><span data-stu-id="cb935-315">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="cb935-316">Ces surcharges n’utilisent pas de noms et consomment seulement les paramètres par défaut de la configuration.</span><span class="sxs-lookup"><span data-stu-id="cb935-316">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="cb935-317">*Changer les valeurs par défaut dans le code*</span><span class="sxs-lookup"><span data-stu-id="cb935-317">*Change the defaults in code*</span></span>

<span data-ttu-id="cb935-318">`ConfigureEndpointDefaults` et `ConfigureHttpsDefaults` peuvent être utilisés pour modifier les paramètres par défaut pour `ListenOptions` et `HttpsConnectionAdapterOptions`, notamment en remplaçant le certificat par défaut spécifié dans le scénario précédent.</span><span class="sxs-lookup"><span data-stu-id="cb935-318">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="cb935-319">`ConfigureEndpointDefaults` et `ConfigureHttpsDefaults` doivent être appelés avant que des points de terminaison soient configurés.</span><span class="sxs-lookup"><span data-stu-id="cb935-319">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
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
```

<span data-ttu-id="cb935-320">*Prise en charge de SNI par Kestrel*</span><span class="sxs-lookup"><span data-stu-id="cb935-320">*Kestrel support for SNI*</span></span>

<span data-ttu-id="cb935-321">[L’indication du nom de serveur (SNI, Server Name Indication)](https://tools.ietf.org/html/rfc6066#section-3) peut être utilisée pour héberger plusieurs domaines sur la même adresse IP et le même port.</span><span class="sxs-lookup"><span data-stu-id="cb935-321">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="cb935-322">Pour que SNI fonctionne, le client envoie le nom d’hôte pour la session sécurisée au serveur pendant la négociation TLS afin que le serveur puisse fournir le certificat correct.</span><span class="sxs-lookup"><span data-stu-id="cb935-322">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="cb935-323">Le client utilise le certificat fourni pour la communication chiffrée avec le serveur pendant la session sécurisée qui suit la négociation TLS.</span><span class="sxs-lookup"><span data-stu-id="cb935-323">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="cb935-324">Kestrel prend en charge SNI via le rappel de `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="cb935-324">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="cb935-325">Le rappel est appelé une fois par connexion pour permettre à l’application d’inspecter le nom d’hôte et de sélectionner le certificat approprié.</span><span class="sxs-lookup"><span data-stu-id="cb935-325">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="cb935-326">La prise en charge de SNI nécessite les points suivants :</span><span class="sxs-lookup"><span data-stu-id="cb935-326">SNI support requires:</span></span>

* <span data-ttu-id="cb935-327">Exécution sur la version cible de .NET Framework `netcoreapp2.1` ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="cb935-327">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="cb935-328">Sur `net461` ou version ultérieure, le rappel est appelé, mais la `name` est toujours `null`.</span><span class="sxs-lookup"><span data-stu-id="cb935-328">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="cb935-329">`name` est également `null` si le client ne fournit pas le paramètre du nom d’hôte dans la négociation TLS.</span><span class="sxs-lookup"><span data-stu-id="cb935-329">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="cb935-330">Tous les sites web s’exécutent sur la même instance Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-330">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="cb935-331">Kestrel ne prend pas en charge le partage d’une adresse IP et d’un port entre plusieurs instances sans un proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="cb935-331">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
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
});
```

### <a name="connection-logging"></a><span data-ttu-id="cb935-332">Journalisation des connexions</span><span class="sxs-lookup"><span data-stu-id="cb935-332">Connection logging</span></span>

<span data-ttu-id="cb935-333">Appelez <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> pour émettre des journaux au niveau du débogage pour la communication au niveau des octets sur une connexion.</span><span class="sxs-lookup"><span data-stu-id="cb935-333">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="cb935-334">La journalisation des connexions est utile pour résoudre les problèmes de communication de bas niveau, par exemple lors du chiffrement TLS et derrière les proxies.</span><span class="sxs-lookup"><span data-stu-id="cb935-334">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="cb935-335">Si `UseConnectionLogging` est placé avant `UseHttps`, le trafic chiffré est journalisé.</span><span class="sxs-lookup"><span data-stu-id="cb935-335">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="cb935-336">Si `UseConnectionLogging` est placé après `UseHttps`, le trafic déchiffré est journalisé.</span><span class="sxs-lookup"><span data-stu-id="cb935-336">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="cb935-337">Lier à un socket TCP</span><span class="sxs-lookup"><span data-stu-id="cb935-337">Bind to a TCP socket</span></span>

<span data-ttu-id="cb935-338">La méthode <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> se lie à un socket TCP, et une expression lambda options permet la configuration d’un certificat X.509 :</span><span class="sxs-lookup"><span data-stu-id="cb935-338">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="cb935-339">L’exemple configure HTTPS pour un point de terminaison avec <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="cb935-339">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="cb935-340">Utilisez la même API afin de configurer d’autres paramètres Kestrel pour des points de terminaison spécifiques.</span><span class="sxs-lookup"><span data-stu-id="cb935-340">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="cb935-341">Lier à un socket Unix</span><span class="sxs-lookup"><span data-stu-id="cb935-341">Bind to a Unix socket</span></span>

<span data-ttu-id="cb935-342">Écoutez sur un socket Unix avec <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> pour améliorer les performances avec Nginx, comme illustré dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="cb935-342">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="cb935-343">Port 0</span><span class="sxs-lookup"><span data-stu-id="cb935-343">Port 0</span></span>

<span data-ttu-id="cb935-344">Si vous spécifiez le numéro de port `0`, Kestrel se lie dynamiquement à un port disponible.</span><span class="sxs-lookup"><span data-stu-id="cb935-344">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="cb935-345">L’exemple suivant montre comment déterminer le port auquel Kestrel se lie à l’exécution :</span><span class="sxs-lookup"><span data-stu-id="cb935-345">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="cb935-346">Quand l’application est exécutée, la sortie de la fenêtre de console indique le port dynamique où l’application peut être atteinte :</span><span class="sxs-lookup"><span data-stu-id="cb935-346">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="cb935-347">Limitations</span><span class="sxs-lookup"><span data-stu-id="cb935-347">Limitations</span></span>

<span data-ttu-id="cb935-348">Configurez des points de terminaison avec les approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="cb935-348">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="cb935-349">Arguments de ligne de commande `--urls`</span><span class="sxs-lookup"><span data-stu-id="cb935-349">`--urls` command-line argument</span></span>
* <span data-ttu-id="cb935-350">La clé de configuration d’hôte `urls`</span><span class="sxs-lookup"><span data-stu-id="cb935-350">`urls` host configuration key</span></span>
* <span data-ttu-id="cb935-351">Variable d’environnement `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="cb935-351">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="cb935-352">Ces méthodes sont utiles si vous voulez que votre code fonctionne avec des serveurs autres que Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-352">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="cb935-353">Toutefois, soyez conscient des limitations suivantes :</span><span class="sxs-lookup"><span data-stu-id="cb935-353">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="cb935-354">HTTPS ne peut pas être utilisé avec ces approches, sauf si un certificat par défaut est fourni dans la configuration du point de terminaison HTTPS (par exemple avec la configuration de `KestrelServerOptions` ou un fichier de configuration, comme illustré plus haut dans cette rubrique).</span><span class="sxs-lookup"><span data-stu-id="cb935-354">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="cb935-355">Quand les deux approches `Listen` et `UseUrls` sont utilisées simultanément, les points de terminaison `Listen` remplacent les points de terminaison `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="cb935-355">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="cb935-356">Configuration de point de terminaison IIS</span><span class="sxs-lookup"><span data-stu-id="cb935-356">IIS endpoint configuration</span></span>

<span data-ttu-id="cb935-357">Quand vous utilisez IIS, les liaisons d’URL pour IIS remplacent les liaisons qui sont définies par `Listen` ou par `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="cb935-357">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="cb935-358">Pour plus d’informations, consultez la rubrique [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="cb935-358">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="cb935-359">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="cb935-359">ListenOptions.Protocols</span></span>

<span data-ttu-id="cb935-360">La propriété `Protocols` établit les protocoles HTTP (`HttpProtocols`) activés sur un point de terminaison de connexion ou pour le serveur.</span><span class="sxs-lookup"><span data-stu-id="cb935-360">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="cb935-361">Affectez une valeur à la propriété `Protocols` à partir de l’énumération `HttpProtocols`.</span><span class="sxs-lookup"><span data-stu-id="cb935-361">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="cb935-362">Valeur enum `HttpProtocols`</span><span class="sxs-lookup"><span data-stu-id="cb935-362">`HttpProtocols` enum value</span></span> | <span data-ttu-id="cb935-363">Protocole de connexion autorisé</span><span class="sxs-lookup"><span data-stu-id="cb935-363">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="cb935-364">HTTP/1.1 uniquement.</span><span class="sxs-lookup"><span data-stu-id="cb935-364">HTTP/1.1 only.</span></span> <span data-ttu-id="cb935-365">Peut être utilisé avec ou sans TLS.</span><span class="sxs-lookup"><span data-stu-id="cb935-365">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="cb935-366">HTTP/2 uniquement.</span><span class="sxs-lookup"><span data-stu-id="cb935-366">HTTP/2 only.</span></span> <span data-ttu-id="cb935-367">Peut être utilisé sans TLS, uniquement si le client prend en charge un [mode de connaissance préalable (Prior Knowledge)](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="cb935-367">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="cb935-368">HTTP/1.1 et HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="cb935-368">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="cb935-369">HTTP/2 nécessite que le client sélectionne HTTP/2 dans le protocole de transfert de [négociation de protocole de couche d’application (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) TLS. dans le cas contraire, la connexion par défaut est HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="cb935-369">HTTP/2 requires the client to select HTTP/2 in the TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="cb935-370">La valeur par défaut `ListenOptions.Protocols` pour un point de terminaison est `HttpProtocols.Http1AndHttp2`.</span><span class="sxs-lookup"><span data-stu-id="cb935-370">The default `ListenOptions.Protocols` value for any endpoint is `HttpProtocols.Http1AndHttp2`.</span></span>

<span data-ttu-id="cb935-371">Restrictions TLS pour HTTP/2 :</span><span class="sxs-lookup"><span data-stu-id="cb935-371">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="cb935-372">TLS version 1.2 ou ultérieure</span><span class="sxs-lookup"><span data-stu-id="cb935-372">TLS version 1.2 or later</span></span>
* <span data-ttu-id="cb935-373">Renégociation désactivée</span><span class="sxs-lookup"><span data-stu-id="cb935-373">Renegotiation disabled</span></span>
* <span data-ttu-id="cb935-374">Compression désactivée</span><span class="sxs-lookup"><span data-stu-id="cb935-374">Compression disabled</span></span>
* <span data-ttu-id="cb935-375">Tailles minimales de l’échange de clé éphémère :</span><span class="sxs-lookup"><span data-stu-id="cb935-375">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="cb935-376">Diffie-Hellman à courbe elliptique (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span><span class="sxs-lookup"><span data-stu-id="cb935-376">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="cb935-377">Diffie-Hellman à champ fini (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2 048 bits minimum</span><span class="sxs-lookup"><span data-stu-id="cb935-377">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="cb935-378">Suite de chiffrement non inscrite sur liste rouge</span><span class="sxs-lookup"><span data-stu-id="cb935-378">Cipher suite not blacklisted</span></span>

<span data-ttu-id="cb935-379">Prise en charge par défaut de `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; avec la courbe elliptique P-256 &lbrack;`FIPS186`&rbrack;.</span><span class="sxs-lookup"><span data-stu-id="cb935-379">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="cb935-380">L’exemple suivant autorise les connexions HTTP/1.1 et HTTP/2 sur le port 8000.</span><span class="sxs-lookup"><span data-stu-id="cb935-380">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="cb935-381">Les connexions sont sécurisées par TLS avec un certificat fourni :</span><span class="sxs-lookup"><span data-stu-id="cb935-381">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="cb935-382">Utilisez l’intergiciel de connexion pour filtrer les négociations TLS en fonction de la connexion pour des chiffrements spécifiques, si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="cb935-382">Use Connection Middleware to filter TLS handshakes on a per-connection basis for specific ciphers if required.</span></span>

<span data-ttu-id="cb935-383">L’exemple suivant lève <xref:System.NotSupportedException> pour tous les algorithmes de chiffrement que l’application ne prend pas en charge.</span><span class="sxs-lookup"><span data-stu-id="cb935-383">The following example throws <xref:System.NotSupportedException> for any cipher algorithm that the app doesn't support.</span></span> <span data-ttu-id="cb935-384">Vous pouvez également définir et comparer [ITlsHandshakeFeature. CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) à une liste de suites de chiffrement acceptables.</span><span class="sxs-lookup"><span data-stu-id="cb935-384">Alternatively, define and compare [ITlsHandshakeFeature.CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) to a list of acceptable cipher suites.</span></span>

<span data-ttu-id="cb935-385">Aucun chiffrement n’est utilisé avec un algorithme de chiffrement [CipherAlgorithmType. null](xref:System.Security.Authentication.CipherAlgorithmType) .</span><span class="sxs-lookup"><span data-stu-id="cb935-385">No encryption is used with a [CipherAlgorithmType.Null](xref:System.Security.Authentication.CipherAlgorithmType) cipher algorithm.</span></span>

```csharp
// using System.Net;
// using Microsoft.AspNetCore.Connections;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.UseTlsFilter();
    });
});
```

```csharp
using System;
using System.Security.Authentication;
using Microsoft.AspNetCore.Connections.Features;

namespace Microsoft.AspNetCore.Connections
{
    public static class TlsFilterConnectionMiddlewareExtensions
    {
        public static IConnectionBuilder UseTlsFilter(
            this IConnectionBuilder builder)
        {
            return builder.Use((connection, next) =>
            {
                var tlsFeature = connection.Features.Get<ITlsHandshakeFeature>();

                if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
                {
                    throw new NotSupportedException("Prohibited cipher: " +
                        tlsFeature.CipherAlgorithm);
                }

                return next();
            });
        }
    }
}
```

<span data-ttu-id="cb935-386">Le filtrage des connexions peut également être configuré via une expression lambda <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> :</span><span class="sxs-lookup"><span data-stu-id="cb935-386">Connection filtering can also be configured via an <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda:</span></span>

```csharp
// using System;
// using System.Net;
// using System.Security.Authentication;
// using Microsoft.AspNetCore.Connections;
// using Microsoft.AspNetCore.Connections.Features;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.Use((context, next) =>
        {
            var tlsFeature = context.Features.Get<ITlsHandshakeFeature>();

            if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
            {
                throw new NotSupportedException(
                    $"Prohibited cipher: {tlsFeature.CipherAlgorithm}");
            }

            return next();
        });
    });
});
```

<span data-ttu-id="cb935-387">Sur Linux, <xref:System.Net.Security.CipherSuitesPolicy> peut être utilisé pour filtrer les négociations TLS en fonction de la connexion :</span><span class="sxs-lookup"><span data-stu-id="cb935-387">On Linux, <xref:System.Net.Security.CipherSuitesPolicy> can be used to filter TLS handshakes on a per-connection basis:</span></span>

```csharp
// using System.Net.Security;
// using Microsoft.AspNetCore.Hosting;
// using Microsoft.AspNetCore.Server.Kestrel.Core;
// using Microsoft.Extensions.DependencyInjection;
// using Microsoft.Extensions.Hosting;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureHttpsDefaults(listenOptions =>
    {
        listenOptions.OnAuthenticate = (context, sslOptions) =>
        {
            sslOptions.CipherSuitesPolicy = new CipherSuitesPolicy(
                new[]
                {
                    TlsCipherSuite.TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256,
                    TlsCipherSuite.TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384,
                    // ...
                });
        };
    });
});
```

<span data-ttu-id="cb935-388">*Définir le protocole à partir de la configuration*</span><span class="sxs-lookup"><span data-stu-id="cb935-388">*Set the protocol from configuration*</span></span>

<span data-ttu-id="cb935-389">Par défaut, `CreateDefaultBuilder` appelle `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` pour charger la configuration de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-389">`CreateDefaultBuilder` calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="cb935-390">L’exemple *appSettings. JSON* suivant établit http/1.1 comme protocole de connexion par défaut pour tous les points de terminaison :</span><span class="sxs-lookup"><span data-stu-id="cb935-390">The following *appsettings.json* example establishes HTTP/1.1 as the default connection protocol for all endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1"
    }
  }
}
```

<span data-ttu-id="cb935-391">L’exemple *appSettings. JSON* suivant établit le protocole de connexion http/1.1 pour un point de terminaison spécifique :</span><span class="sxs-lookup"><span data-stu-id="cb935-391">The following *appsettings.json* example establishes the HTTP/1.1 connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="cb935-392">Les protocoles spécifiés dans le code remplacent les valeurs définies par configuration.</span><span class="sxs-lookup"><span data-stu-id="cb935-392">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="cb935-393">Configuration du transport</span><span class="sxs-lookup"><span data-stu-id="cb935-393">Transport configuration</span></span>

<span data-ttu-id="cb935-394">Pour les projets qui requièrent l’utilisation de Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>) :</span><span class="sxs-lookup"><span data-stu-id="cb935-394">For projects that require the use of Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span></span>

* <span data-ttu-id="cb935-395">Ajoutez une dépendance pour le package [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) au fichier projet de l’application :</span><span class="sxs-lookup"><span data-stu-id="cb935-395">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

   ```xml
   <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                     Version="{VERSION}" />
   ```

* <span data-ttu-id="cb935-396">Appelez <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> sur la `IWebHostBuilder` :</span><span class="sxs-lookup"><span data-stu-id="cb935-396">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> on the `IWebHostBuilder`:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="cb935-397">Préfixes d’URL</span><span class="sxs-lookup"><span data-stu-id="cb935-397">URL prefixes</span></span>

<span data-ttu-id="cb935-398">Quand vous utilisez `UseUrls`, l’argument de ligne de commande `--urls`, la clé de configuration d’hôte `urls` ou la variable d’environnement `ASPNETCORE_URLS`, les préfixes d’URL peuvent être dans un des formats suivants.</span><span class="sxs-lookup"><span data-stu-id="cb935-398">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="cb935-399">Seuls les préfixes d’URL HTTP sont valides.</span><span class="sxs-lookup"><span data-stu-id="cb935-399">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="cb935-400">Kestrel ne prend pas en charge HTTPS lors de la configuration de liaisons d’URL avec `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="cb935-400">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="cb935-401">Adresse IPv4 avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="cb935-401">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="cb935-402">`0.0.0.0` est un cas spécial qui se lie à toutes les adresses IPv4.</span><span class="sxs-lookup"><span data-stu-id="cb935-402">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="cb935-403">Adresse IPv6 avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="cb935-403">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="cb935-404">`[::]` est l’équivalent IPv6 de `0.0.0.0` dans IPv4.</span><span class="sxs-lookup"><span data-stu-id="cb935-404">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="cb935-405">Nom d’hôte avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="cb935-405">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="cb935-406">Les noms d’hôte, `*` et `+` ne sont pas spéciaux.</span><span class="sxs-lookup"><span data-stu-id="cb935-406">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="cb935-407">Tout ce qui n’est pas reconnu comme adresse IP valide ou `localhost` se lie à toutes les adresses IP IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="cb935-407">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="cb935-408">Pour lier différents noms d’hôte à différentes applications ASP.NET Core sur le même port, utilisez [HTTP.sys](xref:fundamentals/servers/httpsys) ou un serveur proxy inverse, comme IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="cb935-408">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="cb935-409">L’hébergement dans une configuration de proxy inverse nécessite le [filtrage d’hôte](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="cb935-409">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="cb935-410">Nom `localhost` de l’hôte avec numéro de port ou adresse IP de bouclage avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="cb935-410">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="cb935-411">Quand `localhost` est spécifié, Kestrel tente de se lier aux interfaces de bouclage IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="cb935-411">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="cb935-412">Si le port demandé est en cours d’utilisation par un autre service sur l’une des interfaces de bouclage, Kestrel ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="cb935-412">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="cb935-413">Si l’une des interfaces de bouclage n’est pas disponible pour une raison quelconque (généralement du fait de la non-prise en charge d’IPv6), Kestrel journalise un avertissement.</span><span class="sxs-lookup"><span data-stu-id="cb935-413">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="cb935-414">Filtrage d’hôtes</span><span class="sxs-lookup"><span data-stu-id="cb935-414">Host filtering</span></span>

<span data-ttu-id="cb935-415">Si Kestrel prend en charge la configuration basée sur les préfixes, comme `http://example.com:5000`, il ignore en grande partie le nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="cb935-415">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="cb935-416">L’hôte `localhost` est un cas spécial utilisé pour la liaison à des adresses de bouclage.</span><span class="sxs-lookup"><span data-stu-id="cb935-416">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="cb935-417">Tout hôte autre qu’une adresse IP explicite se lie à toutes les adresses IP publiques.</span><span class="sxs-lookup"><span data-stu-id="cb935-417">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="cb935-418">Les en-têtes `Host` ne sont pas validés.</span><span class="sxs-lookup"><span data-stu-id="cb935-418">`Host` headers aren't validated.</span></span>

<span data-ttu-id="cb935-419">En guise de solution de contournement, utilisez le middleware de filtrage d’hôtes.</span><span class="sxs-lookup"><span data-stu-id="cb935-419">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="cb935-420">L’intergiciel (middleware) de filtrage d’hôte est fourni par le package [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) , qui est fourni implicitement pour les applications ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="cb935-420">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is implicitly provided for ASP.NET Core apps.</span></span> <span data-ttu-id="cb935-421">L’intergiciel (middleware) est ajouté par <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, qui appelle <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*> :</span><span class="sxs-lookup"><span data-stu-id="cb935-421">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="cb935-422">Le middleware de filtrage d’hôtes est désactivé par défaut.</span><span class="sxs-lookup"><span data-stu-id="cb935-422">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="cb935-423">Pour activer le middleware, définissez une clé `AllowedHosts` dans *appsettings.json*/*appsettings.\<nom_environnement>.json*.</span><span class="sxs-lookup"><span data-stu-id="cb935-423">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="cb935-424">La valeur est une liste délimitée par des points-virgules des noms d’hôte sans numéros de port :</span><span class="sxs-lookup"><span data-stu-id="cb935-424">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="cb935-425">*appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="cb935-425">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="cb935-426">Le [middleware des en-têtes transférés](xref:host-and-deploy/proxy-load-balancer) a aussi une option <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="cb935-426">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="cb935-427">Le middleware des en-têtes transférés et le middleware de filtrage d’hôtes ont des fonctionnalités similaires pour différents scénarios.</span><span class="sxs-lookup"><span data-stu-id="cb935-427">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="cb935-428">La définition d’`AllowedHosts` avec le middleware des en-têtes transférés est appropriée quand l’en-tête `Host` n’est pas conservé durant le transfert des requêtes avec un serveur proxy inverse ou un équilibreur de charge.</span><span class="sxs-lookup"><span data-stu-id="cb935-428">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="cb935-429">La définition d’`AllowedHosts` avec le middleware de filtrage d’hôtes est appropriée quand Kestrel est utilisé comme serveur de périphérie public ou que l’en-tête `Host` est directement transféré.</span><span class="sxs-lookup"><span data-stu-id="cb935-429">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="cb935-430">Pour plus d’informations sur le middleware des en-têtes transférés, consultez <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="cb935-430">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="cb935-431">Kestrel est un [serveur web multiplateforme pour ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="cb935-431">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="cb935-432">Kestrel est le serveur web inclus par défaut dans les modèles de projets ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cb935-432">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="cb935-433">Kestrel prend en charge les scénarios suivants :</span><span class="sxs-lookup"><span data-stu-id="cb935-433">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="cb935-434">HTTPS</span><span class="sxs-lookup"><span data-stu-id="cb935-434">HTTPS</span></span>
* <span data-ttu-id="cb935-435">Mise à niveau opaque utilisée pour activer les [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="cb935-435">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="cb935-436">Sockets UNIX pour des performances élevées derrière Nginx</span><span class="sxs-lookup"><span data-stu-id="cb935-436">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="cb935-437">HTTP/2 (sauf sur macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="cb935-437">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="cb935-438">&dagger;HTTP/2 sera pris en charge sur macOS dans une prochaine version.</span><span class="sxs-lookup"><span data-stu-id="cb935-438">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="cb935-439">Kestrel est pris en charge sur toutes les plateformes et les versions prises en charge par .NET Core.</span><span class="sxs-lookup"><span data-stu-id="cb935-439">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="cb935-440">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cb935-440">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="cb935-441">Prise en charge de HTTP/2</span><span class="sxs-lookup"><span data-stu-id="cb935-441">HTTP/2 support</span></span>

<span data-ttu-id="cb935-442">[HTTP/2](https://httpwg.org/specs/rfc7540.html) est disponible pour les applications ASP.NET Core si les conditions de base suivantes sont remplies :</span><span class="sxs-lookup"><span data-stu-id="cb935-442">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="cb935-443">Système d’exploitation&dagger;</span><span class="sxs-lookup"><span data-stu-id="cb935-443">Operating system&dagger;</span></span>
  * <span data-ttu-id="cb935-444">Windows Server 2016/Windows 10 ou version ultérieure&Dagger;</span><span class="sxs-lookup"><span data-stu-id="cb935-444">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="cb935-445">Linux avec OpenSSL 1.0.2 ou version ultérieure (par exemple,Ubuntu 16.04 ou version ultérieure)</span><span class="sxs-lookup"><span data-stu-id="cb935-445">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="cb935-446">Version cible de .Net Framework : .NET Core 2.2 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="cb935-446">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="cb935-447">Connexion [ALPN (Application-Layer Protocol Negotiation)](https://tools.ietf.org/html/rfc7301#section-3)</span><span class="sxs-lookup"><span data-stu-id="cb935-447">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="cb935-448">TLS 1.2 ou connexion ultérieure</span><span class="sxs-lookup"><span data-stu-id="cb935-448">TLS 1.2 or later connection</span></span>

<span data-ttu-id="cb935-449">&dagger;HTTP/2 sera pris en charge sur macOS dans une prochaine version.</span><span class="sxs-lookup"><span data-stu-id="cb935-449">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="cb935-450">&Dagger;Kestrel propose une prise en charge limitée de HTTP/2 sous Windows Server 2012 R2 et Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="cb935-450">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="cb935-451">La prise en charge est limitée car la liste des suites de chiffrement TLS prises en charge sur ces systèmes d’exploitation est limitée.</span><span class="sxs-lookup"><span data-stu-id="cb935-451">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="cb935-452">Un certificat généré à l’aide d’Elliptic Curve Digital Signature algorithme (ECDSA) peut être requis pour sécuriser les connexions TLS.</span><span class="sxs-lookup"><span data-stu-id="cb935-452">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="cb935-453">Si une connexion HTTP/2 est établie, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) retourne `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="cb935-453">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="cb935-454">HTTP/2 est désactivé par défaut.</span><span class="sxs-lookup"><span data-stu-id="cb935-454">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="cb935-455">Pour plus d’informations sur la configuration, consultez les sections [Options Kestrel](#kestrel-options) et [ListenOptions.Protocols](#listenoptionsprotocols).</span><span class="sxs-lookup"><span data-stu-id="cb935-455">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="cb935-456">Quand utiliser Kestrel avec un proxy inverse ?</span><span class="sxs-lookup"><span data-stu-id="cb935-456">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="cb935-457">Vous pouvez utiliser Kestrel par lui-même ou avec un *serveur proxy inverse*, comme [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="cb935-457">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="cb935-458">Un serveur proxy inverse reçoit les requêtes HTTP en provenance du réseau et les transmet à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-458">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="cb935-459">Kestrel utilisé comme serveur web edge (accessible sur Internet) :</span><span class="sxs-lookup"><span data-stu-id="cb935-459">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel communique directement avec Internet sans serveur proxy inverse](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="cb935-461">Kestrel utilisé dans une configuration de proxy inverse :</span><span class="sxs-lookup"><span data-stu-id="cb935-461">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="cb935-463">L’une ou l’autre des configurations, avec ou sans serveur proxy inverse, est une configuration d’hébergement prise en charge.</span><span class="sxs-lookup"><span data-stu-id="cb935-463">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="cb935-464">Kestrel, s’il est utilisé comme serveur de périphérie sans serveur proxy inverse, ne prend pas en charge le partage de la même adresse IP et du même port entre plusieurs processus.</span><span class="sxs-lookup"><span data-stu-id="cb935-464">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="cb935-465">Quand Kestrel est configuré pour écouter sur un port, il gère tout le trafic pour ce port, quel que soit les en-têtes `Host` des requêtes.</span><span class="sxs-lookup"><span data-stu-id="cb935-465">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="cb935-466">Un proxy inverse qui peut partager des ports a la possibilité de transférer des requêtes à Kestrel sur une adresse IP et un port uniques.</span><span class="sxs-lookup"><span data-stu-id="cb935-466">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="cb935-467">Même si un serveur proxy inverse n’est pas nécessaire, en utiliser un peut être un bon choix.</span><span class="sxs-lookup"><span data-stu-id="cb935-467">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="cb935-468">Un proxy inverse :</span><span class="sxs-lookup"><span data-stu-id="cb935-468">A reverse proxy:</span></span>

* <span data-ttu-id="cb935-469">Peut limiter la surface publique exposée des applications qu’il héberge.</span><span class="sxs-lookup"><span data-stu-id="cb935-469">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="cb935-470">Fournit une couche supplémentaire de configuration et de défense.</span><span class="sxs-lookup"><span data-stu-id="cb935-470">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="cb935-471">Peut mieux s’intégrer à l’infrastructure existante.</span><span class="sxs-lookup"><span data-stu-id="cb935-471">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="cb935-472">Simplifie la configuration de l’équilibrage de charge et d’une communication sécurisée (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="cb935-472">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="cb935-473">Seul le serveur proxy inverse requiert un certificat X. 509 et ce serveur peut communiquer avec les serveurs de l’application sur le réseau interne à l’aide du protocole HTTP simple.</span><span class="sxs-lookup"><span data-stu-id="cb935-473">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="cb935-474">L’hébergement dans une configuration de proxy inverse nécessite le [filtrage d’hôte](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="cb935-474">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="cb935-475">Comment utiliser Kestrel dans les applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cb935-475">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="cb935-476">Le package [Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) est inclus dans le sous- [package Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="cb935-476">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="cb935-477">Les modèles de projet ASP.NET Core utilisent Kestrel par défaut.</span><span class="sxs-lookup"><span data-stu-id="cb935-477">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="cb935-478">Dans *Program.cs*, le modèle de code appelle <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, qui appelle <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="cb935-478">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="cb935-479">Pour plus d’informations sur la `CreateDefaultBuilder` et la génération de l’hôte, consultez la section *configurer un hôte* de <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="cb935-479">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

<span data-ttu-id="cb935-480">Pour fournir une configuration supplémentaire après l’appel de `CreateDefaultBuilder`, utilisez `ConfigureKestrel` :</span><span class="sxs-lookup"><span data-stu-id="cb935-480">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="cb935-481">Si l’application n’appelle pas `CreateDefaultBuilder` pour configurer l’hôte, appelez <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **avant** d’appeler `ConfigureKestrel` :</span><span class="sxs-lookup"><span data-stu-id="cb935-481">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="cb935-482">Options Kestrel</span><span class="sxs-lookup"><span data-stu-id="cb935-482">Kestrel options</span></span>

<span data-ttu-id="cb935-483">Le serveur web Kestrel a des options de configuration de contrainte qui sont particulièrement utiles dans les déploiements exposés à Internet.</span><span class="sxs-lookup"><span data-stu-id="cb935-483">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="cb935-484">Définissez des contraintes sur la propriété <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> de la classe <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="cb935-484">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="cb935-485">La propriété `Limits` conserve une instance de la classe <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="cb935-485">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="cb935-486">Les exemples suivants utilisent l’espace de noms <xref:Microsoft.AspNetCore.Server.Kestrel.Core> :</span><span class="sxs-lookup"><span data-stu-id="cb935-486">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="cb935-487">Les options Kestrel, qui sont configurées dans C# le code des exemples suivants, peuvent également être définies à l’aide d’un fournisseur de [configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="cb935-487">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="cb935-488">Par exemple, le fournisseur de configuration de fichier peut charger la configuration Kestrel à partir d’un fichier *appSettings. JSON* ou *appSettings. { Fichier Environment}. JSON* :</span><span class="sxs-lookup"><span data-stu-id="cb935-488">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

```json
{
  "Kestrel": {
    "Limits": {
      "MaxConcurrentConnections": 100,
      "MaxConcurrentUpgradedConnections": 100
    }
  }
}
```

<span data-ttu-id="cb935-489">Utilisez l' **une** des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="cb935-489">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="cb935-490">Configurer Kestrel dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="cb935-490">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="cb935-491">Injecte une instance de `IConfiguration` dans la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="cb935-491">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="cb935-492">L’exemple suivant suppose que la configuration injectée est assignée à la propriété `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="cb935-492">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="cb935-493">Dans `Startup.ConfigureServices`, chargez la section `Kestrel` de la configuration dans la configuration de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-493">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="cb935-494">Configurez Kestrel lors de la génération de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="cb935-494">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="cb935-495">Dans *Program.cs*, chargez la section `Kestrel` de la configuration dans la configuration de Kestrel :</span><span class="sxs-lookup"><span data-stu-id="cb935-495">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

  ```csharp
  // using Microsoft.Extensions.DependencyInjection;

  public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
      WebHost.CreateDefaultBuilder(args)
          .ConfigureServices((context, services) =>
          {
              services.Configure<KestrelServerOptions>(
                  context.Configuration.GetSection("Kestrel"));
          })
          .UseStartup<Startup>();
  ```

<span data-ttu-id="cb935-496">Les deux approches précédentes fonctionnent avec n’importe quel [fournisseur de configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="cb935-496">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="cb935-497">Délai d’expiration toujours actif</span><span class="sxs-lookup"><span data-stu-id="cb935-497">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="cb935-498">Obtient ou définit le [délai d’expiration toujours actif](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="cb935-498">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="cb935-499">La valeur par défaut est de 2 minutes.</span><span class="sxs-lookup"><span data-stu-id="cb935-499">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=15)]

### <a name="maximum-client-connections"></a><span data-ttu-id="cb935-500">Nombre maximale de connexions client</span><span class="sxs-lookup"><span data-stu-id="cb935-500">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="cb935-501">Le nombre maximal de connexions TCP ouvertes simultanées peut être défini pour l’application entière avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="cb935-501">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="cb935-502">Il existe une limite distincte pour les connexions qui ont été mises à niveau à partir de HTTP ou HTTPS vers un autre protocole (par exemple, sur une demande WebSocket).</span><span class="sxs-lookup"><span data-stu-id="cb935-502">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="cb935-503">Une fois mise à niveau, une connexion n’est pas prise en compte dans la limite `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="cb935-503">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="cb935-504">Le nombre maximal de connexions est illimité (null) par défaut.</span><span class="sxs-lookup"><span data-stu-id="cb935-504">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="cb935-505">Taille maximale du corps de la requête</span><span class="sxs-lookup"><span data-stu-id="cb935-505">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="cb935-506">La taille maximale par défaut du corps de la requête est de 30 000 000 octets, soit environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="cb935-506">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="cb935-507">Pour remplacer la limite dans une application ASP.NET Core MVC, nous vous recommandons d’utiliser l’attribut <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> sur une méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="cb935-507">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="cb935-508">Voici un exemple qui montre comment configurer la contrainte pour l’application sur chaque requête :</span><span class="sxs-lookup"><span data-stu-id="cb935-508">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="cb935-509">Remplacer le paramètre sur une demande spécifique dans l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="cb935-509">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="cb935-510">Une exception est levée si l’application configure la limite sur une demande après que l’application a commencé à lire la demande.</span><span class="sxs-lookup"><span data-stu-id="cb935-510">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="cb935-511">Il existe une propriété `IsReadOnly` qui indique si la propriété `MaxRequestBodySize` est en lecture seule ; si tel est le cas, il est trop tard pour configurer la limite.</span><span class="sxs-lookup"><span data-stu-id="cb935-511">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="cb935-512">Quand une application est exécutée [hors processus](xref:host-and-deploy/iis/index#out-of-process-hosting-model) derrière le [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module), la limite de taille du corps de demande de Kestrel est désactivée, car IIS définit déjà la limite.</span><span class="sxs-lookup"><span data-stu-id="cb935-512">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="cb935-513">Débit données minimal du corps de la requête</span><span class="sxs-lookup"><span data-stu-id="cb935-513">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="cb935-514">Kestrel vérifie à chaque seconde si les données arrivent au débit spécifié en octets/seconde.</span><span class="sxs-lookup"><span data-stu-id="cb935-514">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="cb935-515">Si le taux chute au-dessous de la valeur minimale, le délai d’attente de la connexion est dépassé. La période de grâce correspond à la durée pendant laquelle Kestrel permet au client d’augmenter son taux d’envoi jusqu’à la valeur minimale. la vitesse n’est pas vérifiée pendant cette période.</span><span class="sxs-lookup"><span data-stu-id="cb935-515">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="cb935-516">La période de grâce permet d’éviter la suppression des connexions qui, initialement, envoient des données à une vitesse lente en raison de la lenteur du démarrage de TCP.</span><span class="sxs-lookup"><span data-stu-id="cb935-516">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="cb935-517">Le débit minimal par défaut est 240 octets/seconde, avec une période de grâce de 5 secondes.</span><span class="sxs-lookup"><span data-stu-id="cb935-517">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="cb935-518">Un débit minimal s’applique également à la réponse.</span><span class="sxs-lookup"><span data-stu-id="cb935-518">A minimum rate also applies to the response.</span></span> <span data-ttu-id="cb935-519">Le code pour définir les limites de demande et de réponse est identique à l’exception de `RequestBody` ou `Response` dans les noms de propriété et d’interface.</span><span class="sxs-lookup"><span data-stu-id="cb935-519">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="cb935-520">Voici un exemple qui montre comment configurer les débits de données minimaux dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="cb935-520">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="cb935-521">Remplacer les limites de taux minimum par demande dans l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="cb935-521">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="cb935-522">Aucune des fonctionnalités de débit référencées dans l’exemple précédent n’est présente dans `HttpContext.Features` pour les requêtes HTTP/2, car la modification des limites de débit par requête n’est pas prise en charge pour HTTP/2 (le protocole prend en charge le multiplexage de requête).</span><span class="sxs-lookup"><span data-stu-id="cb935-522">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="cb935-523">Les limites de débit à l’échelle du serveur configurées par le biais de `KestrelServerOptions.Limits` s’appliquent encore aux connexions HTTP/1.x et HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="cb935-523">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="cb935-524">Délai d’expiration des en-têtes de requête</span><span class="sxs-lookup"><span data-stu-id="cb935-524">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="cb935-525">Obtient ou définit le temps maximal passé par le serveur à recevoir des en-têtes de requête.</span><span class="sxs-lookup"><span data-stu-id="cb935-525">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="cb935-526">La valeur par défaut est de 30 secondes.</span><span class="sxs-lookup"><span data-stu-id="cb935-526">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=16)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="cb935-527">Flux de données maximal par connexion</span><span class="sxs-lookup"><span data-stu-id="cb935-527">Maximum streams per connection</span></span>

<span data-ttu-id="cb935-528">`Http2.MaxStreamsPerConnection` limite le nombre de flux de requête simultanée par connexion HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="cb935-528">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="cb935-529">Les flux de données excédentaires sont refusés.</span><span class="sxs-lookup"><span data-stu-id="cb935-529">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="cb935-530">La valeur par défaut est 100.</span><span class="sxs-lookup"><span data-stu-id="cb935-530">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="cb935-531">Taille de la table d’en-tête</span><span class="sxs-lookup"><span data-stu-id="cb935-531">Header table size</span></span>

<span data-ttu-id="cb935-532">Le décodeur HPACK décompresse les en-têtes HTTP pour les connexions HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="cb935-532">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="cb935-533">`Http2.HeaderTableSize` limite la taille de la table de compression d’en-tête que le décodeur HPACK utilise.</span><span class="sxs-lookup"><span data-stu-id="cb935-533">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="cb935-534">La valeur est fournie en octets et doit être supérieure à zéro (0).</span><span class="sxs-lookup"><span data-stu-id="cb935-534">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="cb935-535">La valeur par défaut est 4096.</span><span class="sxs-lookup"><span data-stu-id="cb935-535">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="cb935-536">Taille de trame maximale</span><span class="sxs-lookup"><span data-stu-id="cb935-536">Maximum frame size</span></span>

<span data-ttu-id="cb935-537">`Http2.MaxFrameSize` Indique la taille maximale de charge utile dans la trame de connexion HTTP/2 à recevoir.</span><span class="sxs-lookup"><span data-stu-id="cb935-537">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="cb935-538">La valeur est fournie en octets et doit être comprise entre 2^14 (16,384) et 2^24-1 (16,777,215).</span><span class="sxs-lookup"><span data-stu-id="cb935-538">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="cb935-539">La valeur par défaut est 2^14 (16,384).</span><span class="sxs-lookup"><span data-stu-id="cb935-539">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="cb935-540">Taille maximale d’en-tête de requête</span><span class="sxs-lookup"><span data-stu-id="cb935-540">Maximum request header size</span></span>

<span data-ttu-id="cb935-541">`Http2.MaxRequestHeaderFieldSize` indique la taille maximale autorisée en octets des valeurs d’en-tête de requête.</span><span class="sxs-lookup"><span data-stu-id="cb935-541">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="cb935-542">Cette limite s’applique au nom et à la valeur dans leurs représentations compressées et non compressées.</span><span class="sxs-lookup"><span data-stu-id="cb935-542">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="cb935-543">La valeur doit être supérieure à zéro (0).</span><span class="sxs-lookup"><span data-stu-id="cb935-543">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="cb935-544">La valeur par défaut est 8 192.</span><span class="sxs-lookup"><span data-stu-id="cb935-544">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="cb935-545">Taille de fenêtre de connexion initiale</span><span class="sxs-lookup"><span data-stu-id="cb935-545">Initial connection window size</span></span>

<span data-ttu-id="cb935-546">`Http2.InitialConnectionWindowSize` indique la quantité maximale de données de corps de requête, en octets, que le serveur met en mémoire tampon à un moment donné pour toutes les requêtes (flux) par connexion.</span><span class="sxs-lookup"><span data-stu-id="cb935-546">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="cb935-547">Les requêtes sont également limitées par `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="cb935-547">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="cb935-548">La valeur doit être supérieure ou égale à 65 535 et inférieure à 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="cb935-548">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="cb935-549">La valeur par défaut est 128 Ko (131 072).</span><span class="sxs-lookup"><span data-stu-id="cb935-549">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="cb935-550">Taille de la fenêtre de flux initiale</span><span class="sxs-lookup"><span data-stu-id="cb935-550">Initial stream window size</span></span>

<span data-ttu-id="cb935-551">`Http2.InitialStreamWindowSize` indique la quantité maximale de données de corps de requête, en octets, que le serveur met en mémoire tampon à un moment donné par requête (flux).</span><span class="sxs-lookup"><span data-stu-id="cb935-551">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="cb935-552">Les requêtes sont également limitées par `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="cb935-552">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="cb935-553">La valeur doit être supérieure ou égale à 65 535 et inférieure à 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="cb935-553">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="cb935-554">La valeur par défaut est 96 Ko (98 304).</span><span class="sxs-lookup"><span data-stu-id="cb935-554">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="cb935-555">E/S synchrone</span><span class="sxs-lookup"><span data-stu-id="cb935-555">Synchronous IO</span></span>

<span data-ttu-id="cb935-556"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> contrôle si l’e/s synchrone est autorisée pour la requête et la réponse.</span><span class="sxs-lookup"><span data-stu-id="cb935-556"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="cb935-557">La valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="cb935-557">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="cb935-558">Un grand nombre d’opérations d’e/s synchrones de blocage peut entraîner une privation de pool de thread, ce qui fait que l’application ne répond pas.</span><span class="sxs-lookup"><span data-stu-id="cb935-558">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="cb935-559">Activez uniquement `AllowSynchronousIO` lors de l’utilisation d’une bibliothèque qui ne prend pas en charge l’e/s asynchrone.</span><span class="sxs-lookup"><span data-stu-id="cb935-559">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="cb935-560">L’exemple suivant active un e/s synchrone :</span><span class="sxs-lookup"><span data-stu-id="cb935-560">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="cb935-561">Pour plus d’informations sur les autres options et limites de Kestrel, consultez :</span><span class="sxs-lookup"><span data-stu-id="cb935-561">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="cb935-562">Configuration de point de terminaison</span><span class="sxs-lookup"><span data-stu-id="cb935-562">Endpoint configuration</span></span>

<span data-ttu-id="cb935-563">Par défaut, ASP.NET Core se lie à :</span><span class="sxs-lookup"><span data-stu-id="cb935-563">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="cb935-564">`https://localhost:5001` (quand un certificat de développement local est présent)</span><span class="sxs-lookup"><span data-stu-id="cb935-564">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="cb935-565">Spécifiez les URL avec :</span><span class="sxs-lookup"><span data-stu-id="cb935-565">Specify URLs using the:</span></span>

* <span data-ttu-id="cb935-566">La variable d’environnement `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="cb935-566">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="cb935-567">L’argument de ligne de commande `--urls`.</span><span class="sxs-lookup"><span data-stu-id="cb935-567">`--urls` command-line argument.</span></span>
* <span data-ttu-id="cb935-568">La clé de configuration d’hôte `urls`.</span><span class="sxs-lookup"><span data-stu-id="cb935-568">`urls` host configuration key.</span></span>
* <span data-ttu-id="cb935-569">La méthode d’extension `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="cb935-569">`UseUrls` extension method.</span></span>

<span data-ttu-id="cb935-570">La valeur fournie avec ces approches peut être un ou plusieurs points de terminaison HTTP et HTTPS (HTTPS si un certificat par défaut est disponible).</span><span class="sxs-lookup"><span data-stu-id="cb935-570">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="cb935-571">Configurez la valeur sous forme de liste délimitée par des points-virgules (par exemple `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="cb935-571">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="cb935-572">Pour plus d’informations sur ces approches, voir [URL de serveur](xref:fundamentals/host/web-host#server-urls) et [Remplacer la configuration](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="cb935-572">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="cb935-573">Un certificat de développement est créé :</span><span class="sxs-lookup"><span data-stu-id="cb935-573">A development certificate is created:</span></span>

* <span data-ttu-id="cb935-574">Quand le [SDK .NET Core](/dotnet/core/sdk) est installé.</span><span class="sxs-lookup"><span data-stu-id="cb935-574">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="cb935-575">[L’outil dev-certs](xref:aspnetcore-2.1#https) est utilisé pour créer un certificat.</span><span class="sxs-lookup"><span data-stu-id="cb935-575">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="cb935-576">Certains navigateurs requièrent l’octroi d’une autorisation explicite pour approuver le certificat de développement local.</span><span class="sxs-lookup"><span data-stu-id="cb935-576">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="cb935-577">Les modèles de projet configurent les applications à exécuter sur HTTPs par défaut et incluent [la redirection https et la prise en charge de HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="cb935-577">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="cb935-578">Appelez les méthodes <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> ou <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> sur <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> pour configurer les préfixes et les ports d’URL pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-578">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="cb935-579">`UseUrls`, l’argument de ligne de commande `--urls`, la clé de configuration d’hôte `urls` et la variable d’environnement `ASPNETCORE_URLS` fonctionnent également, mais ils présentent les limitations indiquées plus loin dans cette section (un certificat par défaut doit être disponible pour la configuration du point de terminaison HTTPS).</span><span class="sxs-lookup"><span data-stu-id="cb935-579">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="cb935-580">configuration de `KestrelServerOptions` :</span><span class="sxs-lookup"><span data-stu-id="cb935-580">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="cb935-581">ConfigureEndpointDefaults (action\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="cb935-581">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="cb935-582">Spécifie une `Action` de configuration à exécuter pour chaque point de terminaison spécifié.</span><span class="sxs-lookup"><span data-stu-id="cb935-582">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="cb935-583">Le fait d’appeler `ConfigureEndpointDefaults` plusieurs fois remplace les `Action`s précédentes par la dernière `Action` spécifiée.</span><span class="sxs-lookup"><span data-stu-id="cb935-583">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="cb935-584">ConfigureHttpsDefaults (action\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="cb935-584">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="cb935-585">Spécifie une `Action` de configuration à exécuter pour chaque point de terminaison HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cb935-585">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="cb935-586">Le fait d’appeler `ConfigureHttpsDefaults` plusieurs fois remplace les `Action`s précédentes par la dernière `Action` spécifiée.</span><span class="sxs-lookup"><span data-stu-id="cb935-586">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configureiconfiguration"></a><span data-ttu-id="cb935-587">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="cb935-587">Configure(IConfiguration)</span></span>

<span data-ttu-id="cb935-588">Crée un chargeur de configuration pour configurer Kestrel, qui prend en entrée une <xref:Microsoft.Extensions.Configuration.IConfiguration>.</span><span class="sxs-lookup"><span data-stu-id="cb935-588">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="cb935-589">La configuration doit être limitée à la section de configuration pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-589">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="cb935-590">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="cb935-590">ListenOptions.UseHttps</span></span>

<span data-ttu-id="cb935-591">Configure Kestrel pour l’utilisation de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cb935-591">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="cb935-592">Extensions de `ListenOptions.UseHttps` :</span><span class="sxs-lookup"><span data-stu-id="cb935-592">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="cb935-593">`UseHttps` &ndash; Configure Kestrel pour l’utilisation de HTTPS avec le certificat par défaut.</span><span class="sxs-lookup"><span data-stu-id="cb935-593">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="cb935-594">Lève une exception si aucun certificat par défaut n’est configuré.</span><span class="sxs-lookup"><span data-stu-id="cb935-594">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="cb935-595">Paramètres de `ListenOptions.UseHttps` :</span><span class="sxs-lookup"><span data-stu-id="cb935-595">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="cb935-596">`filename` est le chemin et le nom d’un fichier de certificat, relatif au répertoire qui contient les fichiers de contenu de l’application.</span><span class="sxs-lookup"><span data-stu-id="cb935-596">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="cb935-597">`password` est le mot de passe nécessaire pour accéder aux données du certificat X.509.</span><span class="sxs-lookup"><span data-stu-id="cb935-597">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="cb935-598">`configureOptions` est une `Action` pour configurer les `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="cb935-598">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="cb935-599">Retourne l'`ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="cb935-599">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="cb935-600">`storeName` est le magasin de certificats à partir duquel charger le certificat.</span><span class="sxs-lookup"><span data-stu-id="cb935-600">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="cb935-601">`subject` est le nom du sujet du certificat.</span><span class="sxs-lookup"><span data-stu-id="cb935-601">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="cb935-602">`allowInvalid` indique si les certificats non valides doivent être considérés, comme les certificats auto-signés.</span><span class="sxs-lookup"><span data-stu-id="cb935-602">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="cb935-603">`location` est l’emplacement du magasin à partir duquel charger le certificat.</span><span class="sxs-lookup"><span data-stu-id="cb935-603">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="cb935-604">`serverCertificate` est le certificat X.509.</span><span class="sxs-lookup"><span data-stu-id="cb935-604">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="cb935-605">En production, HTTPS doit être explicitement configuré.</span><span class="sxs-lookup"><span data-stu-id="cb935-605">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="cb935-606">Au minimum, un certificat par défaut doit être fourni.</span><span class="sxs-lookup"><span data-stu-id="cb935-606">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="cb935-607">Configurations prises en charge décrites dans la suite :</span><span class="sxs-lookup"><span data-stu-id="cb935-607">Supported configurations described next:</span></span>

* <span data-ttu-id="cb935-608">Pas de configuration</span><span class="sxs-lookup"><span data-stu-id="cb935-608">No configuration</span></span>
* <span data-ttu-id="cb935-609">Remplacer le certificat par défaut dans la configuration</span><span class="sxs-lookup"><span data-stu-id="cb935-609">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="cb935-610">Changer les valeurs par défaut dans le code</span><span class="sxs-lookup"><span data-stu-id="cb935-610">Change the defaults in code</span></span>

<span data-ttu-id="cb935-611">*Pas de configuration*</span><span class="sxs-lookup"><span data-stu-id="cb935-611">*No configuration*</span></span>

<span data-ttu-id="cb935-612">Kestrel écoute sur `http://localhost:5000` et sur `https://localhost:5001` (si un certificat par défaut est disponible).</span><span class="sxs-lookup"><span data-stu-id="cb935-612">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="cb935-613">*Remplacer le certificat par défaut dans la configuration*</span><span class="sxs-lookup"><span data-stu-id="cb935-613">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="cb935-614">Par défaut, `CreateDefaultBuilder` appelle `Configure(context.Configuration.GetSection("Kestrel"))` pour charger la configuration de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-614">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="cb935-615">Un schéma de configuration des paramètres d’application HTTPS par défaut est disponible pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-615">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="cb935-616">Configurez plusieurs points de terminaison, notamment les URL et les certificats à utiliser, à partir d’un fichier sur disque ou d’un magasin de certificats.</span><span class="sxs-lookup"><span data-stu-id="cb935-616">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="cb935-617">Dans l’exemple de fichier *appsettings.json* suivant :</span><span class="sxs-lookup"><span data-stu-id="cb935-617">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="cb935-618">Définissez **AllowInvalid** sur `true` pour permettre l’utilisation de certificats non valides (par exemple des certificats auto-signés).</span><span class="sxs-lookup"><span data-stu-id="cb935-618">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="cb935-619">Les points de terminaison HTTPS qui ne spécifient pas de certificat (**HttpsDefaultCert** dans l’exemple qui suit) utilise comme alternative de secours le certificat défini sous **Certificats** > **Par défaut**  ou bien le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="cb935-619">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="cb935-620">Une alternative à l’utilisation de **Chemin** et de **Mot de passe** pour un nœud de certificat consiste à spécifier le certificat avec des champs du magasin de certificats.</span><span class="sxs-lookup"><span data-stu-id="cb935-620">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="cb935-621">Par exemple, le certificat **Certificats** > **Par défaut** peut être spécifié en tant que :</span><span class="sxs-lookup"><span data-stu-id="cb935-621">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="cb935-622">Notes de schéma :</span><span class="sxs-lookup"><span data-stu-id="cb935-622">Schema notes:</span></span>

* <span data-ttu-id="cb935-623">Les noms des points de terminaison ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="cb935-623">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="cb935-624">Par exemple, `HTTPS` et `Https` sont valides.</span><span class="sxs-lookup"><span data-stu-id="cb935-624">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="cb935-625">Le paramètre `Url` est obligatoire pour chaque point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="cb935-625">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="cb935-626">Le format de ce paramètre est le même que celui du paramètre de configuration `Urls` du plus haut niveau, sauf qu’il est limité à une seule valeur.</span><span class="sxs-lookup"><span data-stu-id="cb935-626">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="cb935-627">Ces points de terminaison remplacent ceux qui sont définis dans le paramètre de configuration `Urls` du plus haut niveau configuration, au lieu de s’y ajouter.</span><span class="sxs-lookup"><span data-stu-id="cb935-627">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="cb935-628">Les points de terminaison définis dans le code via `Listen` sont cumulatifs avec les points de terminaison définis dans la section de configuration.</span><span class="sxs-lookup"><span data-stu-id="cb935-628">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="cb935-629">La section `Certificate` est facultative.</span><span class="sxs-lookup"><span data-stu-id="cb935-629">The `Certificate` section is optional.</span></span> <span data-ttu-id="cb935-630">Si la section `Certificate` n’est pas spécifiée, les valeurs par défaut définies dans les scénarios précédents sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="cb935-630">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="cb935-631">Si aucune valeur par défaut n’est disponible, le serveur lève une exception et son démarrage échoue.</span><span class="sxs-lookup"><span data-stu-id="cb935-631">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="cb935-632">La section `Certificate` prend en charge les certificats **Chemin**&ndash;**Mot de passe** et **Sujet**&ndash;**Magasin**.</span><span class="sxs-lookup"><span data-stu-id="cb935-632">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="cb935-633">Vous pouvez définir un nombre quelconque de points de terminaison de cette façon, pour autant qu’ils ne provoquent pas de conflits de port.</span><span class="sxs-lookup"><span data-stu-id="cb935-633">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="cb935-634">`options.Configure(context.Configuration.GetSection("{SECTION}"))` retourne un `KestrelConfigurationLoader` avec une méthode `.Endpoint(string name, listenOptions => { })` qui peut être utilisée pour compléter les paramètres d’un point de terminaison configuré :</span><span class="sxs-lookup"><span data-stu-id="cb935-634">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="cb935-635">`KestrelServerOptions.ConfigurationLoader` est accessible directement pour poursuivre l’itération sur le chargeur existant, tel que celui fourni par <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="cb935-635">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="cb935-636">La section de configuration pour chaque point de terminaison est disponible sur les options de la méthode `Endpoint` afin que les paramètres personnalisés puissent être lus.</span><span class="sxs-lookup"><span data-stu-id="cb935-636">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="cb935-637">Plusieurs configurations peuvent être chargées en rappelant `options.Configure(context.Configuration.GetSection("{SECTION}"))` avec une autre section.</span><span class="sxs-lookup"><span data-stu-id="cb935-637">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="cb935-638">Seule la dernière configuration est utilisée, à moins que `Load` soit explicitement appelé sur les instances précédentes.</span><span class="sxs-lookup"><span data-stu-id="cb935-638">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="cb935-639">Le métapackage n’appelle pas `Load` : sa section de configuration par défaut peut donc être remplacée.</span><span class="sxs-lookup"><span data-stu-id="cb935-639">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="cb935-640">`KestrelConfigurationLoader` reflète la famille d’API `Listen` de `KestrelServerOptions` sous forme de surcharges de `Endpoint` : le code et les points de terminaison de configuration peuvent donc être configurés au même emplacement.</span><span class="sxs-lookup"><span data-stu-id="cb935-640">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="cb935-641">Ces surcharges n’utilisent pas de noms et consomment seulement les paramètres par défaut de la configuration.</span><span class="sxs-lookup"><span data-stu-id="cb935-641">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="cb935-642">*Changer les valeurs par défaut dans le code*</span><span class="sxs-lookup"><span data-stu-id="cb935-642">*Change the defaults in code*</span></span>

<span data-ttu-id="cb935-643">`ConfigureEndpointDefaults` et `ConfigureHttpsDefaults` peuvent être utilisés pour modifier les paramètres par défaut pour `ListenOptions` et `HttpsConnectionAdapterOptions`, notamment en remplaçant le certificat par défaut spécifié dans le scénario précédent.</span><span class="sxs-lookup"><span data-stu-id="cb935-643">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="cb935-644">`ConfigureEndpointDefaults` et `ConfigureHttpsDefaults` doivent être appelés avant que des points de terminaison soient configurés.</span><span class="sxs-lookup"><span data-stu-id="cb935-644">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="cb935-645">*Prise en charge de SNI par Kestrel*</span><span class="sxs-lookup"><span data-stu-id="cb935-645">*Kestrel support for SNI*</span></span>

<span data-ttu-id="cb935-646">[L’indication du nom de serveur (SNI, Server Name Indication)](https://tools.ietf.org/html/rfc6066#section-3) peut être utilisée pour héberger plusieurs domaines sur la même adresse IP et le même port.</span><span class="sxs-lookup"><span data-stu-id="cb935-646">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="cb935-647">Pour que SNI fonctionne, le client envoie le nom d’hôte pour la session sécurisée au serveur pendant la négociation TLS afin que le serveur puisse fournir le certificat correct.</span><span class="sxs-lookup"><span data-stu-id="cb935-647">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="cb935-648">Le client utilise le certificat fourni pour la communication chiffrée avec le serveur pendant la session sécurisée qui suit la négociation TLS.</span><span class="sxs-lookup"><span data-stu-id="cb935-648">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="cb935-649">Kestrel prend en charge SNI via le rappel de `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="cb935-649">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="cb935-650">Le rappel est appelé une fois par connexion pour permettre à l’application d’inspecter le nom d’hôte et de sélectionner le certificat approprié.</span><span class="sxs-lookup"><span data-stu-id="cb935-650">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="cb935-651">La prise en charge de SNI nécessite les points suivants :</span><span class="sxs-lookup"><span data-stu-id="cb935-651">SNI support requires:</span></span>

* <span data-ttu-id="cb935-652">Exécution sur la version cible de .NET Framework `netcoreapp2.1` ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="cb935-652">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="cb935-653">Sur `net461` ou version ultérieure, le rappel est appelé, mais la `name` est toujours `null`.</span><span class="sxs-lookup"><span data-stu-id="cb935-653">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="cb935-654">`name` est également `null` si le client ne fournit pas le paramètre du nom d’hôte dans la négociation TLS.</span><span class="sxs-lookup"><span data-stu-id="cb935-654">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="cb935-655">Tous les sites web s’exécutent sur la même instance Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-655">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="cb935-656">Kestrel ne prend pas en charge le partage d’une adresse IP et d’un port entre plusieurs instances sans un proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="cb935-656">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="cb935-657">Journalisation des connexions</span><span class="sxs-lookup"><span data-stu-id="cb935-657">Connection logging</span></span>

<span data-ttu-id="cb935-658">Appelez <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> pour émettre des journaux au niveau du débogage pour la communication au niveau des octets sur une connexion.</span><span class="sxs-lookup"><span data-stu-id="cb935-658">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="cb935-659">La journalisation des connexions est utile pour résoudre les problèmes de communication de bas niveau, par exemple lors du chiffrement TLS et derrière les proxies.</span><span class="sxs-lookup"><span data-stu-id="cb935-659">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="cb935-660">Si `UseConnectionLogging` est placé avant `UseHttps`, le trafic chiffré est journalisé.</span><span class="sxs-lookup"><span data-stu-id="cb935-660">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="cb935-661">Si `UseConnectionLogging` est placé après `UseHttps`, le trafic déchiffré est journalisé.</span><span class="sxs-lookup"><span data-stu-id="cb935-661">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="cb935-662">Lier à un socket TCP</span><span class="sxs-lookup"><span data-stu-id="cb935-662">Bind to a TCP socket</span></span>

<span data-ttu-id="cb935-663">La méthode <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> se lie à un socket TCP, et une expression lambda options permet la configuration d’un certificat X.509 :</span><span class="sxs-lookup"><span data-stu-id="cb935-663">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="cb935-664">L’exemple configure HTTPS pour un point de terminaison avec <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="cb935-664">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="cb935-665">Utilisez la même API afin de configurer d’autres paramètres Kestrel pour des points de terminaison spécifiques.</span><span class="sxs-lookup"><span data-stu-id="cb935-665">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="cb935-666">Lier à un socket Unix</span><span class="sxs-lookup"><span data-stu-id="cb935-666">Bind to a Unix socket</span></span>

<span data-ttu-id="cb935-667">Écoutez sur un socket Unix avec <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> pour améliorer les performances avec Nginx, comme illustré dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="cb935-667">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="cb935-668">Port 0</span><span class="sxs-lookup"><span data-stu-id="cb935-668">Port 0</span></span>

<span data-ttu-id="cb935-669">Si vous spécifiez le numéro de port `0`, Kestrel se lie dynamiquement à un port disponible.</span><span class="sxs-lookup"><span data-stu-id="cb935-669">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="cb935-670">L’exemple suivant montre comment déterminer le port auquel Kestrel se lie à l’exécution :</span><span class="sxs-lookup"><span data-stu-id="cb935-670">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="cb935-671">Quand l’application est exécutée, la sortie de la fenêtre de console indique le port dynamique où l’application peut être atteinte :</span><span class="sxs-lookup"><span data-stu-id="cb935-671">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="cb935-672">Limitations</span><span class="sxs-lookup"><span data-stu-id="cb935-672">Limitations</span></span>

<span data-ttu-id="cb935-673">Configurez des points de terminaison avec les approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="cb935-673">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="cb935-674">Arguments de ligne de commande `--urls`</span><span class="sxs-lookup"><span data-stu-id="cb935-674">`--urls` command-line argument</span></span>
* <span data-ttu-id="cb935-675">La clé de configuration d’hôte `urls`</span><span class="sxs-lookup"><span data-stu-id="cb935-675">`urls` host configuration key</span></span>
* <span data-ttu-id="cb935-676">Variable d’environnement `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="cb935-676">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="cb935-677">Ces méthodes sont utiles si vous voulez que votre code fonctionne avec des serveurs autres que Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-677">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="cb935-678">Toutefois, soyez conscient des limitations suivantes :</span><span class="sxs-lookup"><span data-stu-id="cb935-678">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="cb935-679">HTTPS ne peut pas être utilisé avec ces approches, sauf si un certificat par défaut est fourni dans la configuration du point de terminaison HTTPS (par exemple avec la configuration de `KestrelServerOptions` ou un fichier de configuration, comme illustré plus haut dans cette rubrique).</span><span class="sxs-lookup"><span data-stu-id="cb935-679">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="cb935-680">Quand les deux approches `Listen` et `UseUrls` sont utilisées simultanément, les points de terminaison `Listen` remplacent les points de terminaison `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="cb935-680">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="cb935-681">Configuration de point de terminaison IIS</span><span class="sxs-lookup"><span data-stu-id="cb935-681">IIS endpoint configuration</span></span>

<span data-ttu-id="cb935-682">Quand vous utilisez IIS, les liaisons d’URL pour IIS remplacent les liaisons qui sont définies par `Listen` ou par `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="cb935-682">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="cb935-683">Pour plus d’informations, consultez la rubrique [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="cb935-683">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="cb935-684">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="cb935-684">ListenOptions.Protocols</span></span>

<span data-ttu-id="cb935-685">La propriété `Protocols` établit les protocoles HTTP (`HttpProtocols`) activés sur un point de terminaison de connexion ou pour le serveur.</span><span class="sxs-lookup"><span data-stu-id="cb935-685">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="cb935-686">Affectez une valeur à la propriété `Protocols` à partir de l’énumération `HttpProtocols`.</span><span class="sxs-lookup"><span data-stu-id="cb935-686">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="cb935-687">Valeur enum `HttpProtocols`</span><span class="sxs-lookup"><span data-stu-id="cb935-687">`HttpProtocols` enum value</span></span> | <span data-ttu-id="cb935-688">Protocole de connexion autorisé</span><span class="sxs-lookup"><span data-stu-id="cb935-688">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="cb935-689">HTTP/1.1 uniquement.</span><span class="sxs-lookup"><span data-stu-id="cb935-689">HTTP/1.1 only.</span></span> <span data-ttu-id="cb935-690">Peut être utilisé avec ou sans TLS.</span><span class="sxs-lookup"><span data-stu-id="cb935-690">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="cb935-691">HTTP/2 uniquement.</span><span class="sxs-lookup"><span data-stu-id="cb935-691">HTTP/2 only.</span></span> <span data-ttu-id="cb935-692">Peut être utilisé sans TLS, uniquement si le client prend en charge un [mode de connaissance préalable (Prior Knowledge)](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="cb935-692">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="cb935-693">HTTP/1.1 et HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="cb935-693">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="cb935-694">HTTP/2 requiert une connexion TLS et la [négociation de protocole de couche application (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) ; dans le cas contraire, la connexion par défaut est HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="cb935-694">HTTP/2 requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="cb935-695">Le protocole par défaut est HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="cb935-695">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="cb935-696">Restrictions TLS pour HTTP/2 :</span><span class="sxs-lookup"><span data-stu-id="cb935-696">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="cb935-697">TLS version 1.2 ou ultérieure</span><span class="sxs-lookup"><span data-stu-id="cb935-697">TLS version 1.2 or later</span></span>
* <span data-ttu-id="cb935-698">Renégociation désactivée</span><span class="sxs-lookup"><span data-stu-id="cb935-698">Renegotiation disabled</span></span>
* <span data-ttu-id="cb935-699">Compression désactivée</span><span class="sxs-lookup"><span data-stu-id="cb935-699">Compression disabled</span></span>
* <span data-ttu-id="cb935-700">Tailles minimales de l’échange de clé éphémère :</span><span class="sxs-lookup"><span data-stu-id="cb935-700">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="cb935-701">Diffie-Hellman à courbe elliptique (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span><span class="sxs-lookup"><span data-stu-id="cb935-701">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="cb935-702">Diffie-Hellman à champ fini (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2 048 bits minimum</span><span class="sxs-lookup"><span data-stu-id="cb935-702">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="cb935-703">Suite de chiffrement non inscrite sur liste rouge</span><span class="sxs-lookup"><span data-stu-id="cb935-703">Cipher suite not blacklisted</span></span>

<span data-ttu-id="cb935-704">Prise en charge par défaut de `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; avec la courbe elliptique P-256 &lbrack;`FIPS186`&rbrack;.</span><span class="sxs-lookup"><span data-stu-id="cb935-704">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="cb935-705">L’exemple suivant autorise les connexions HTTP/1.1 et HTTP/2 sur le port 8000.</span><span class="sxs-lookup"><span data-stu-id="cb935-705">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="cb935-706">Les connexions sont sécurisées par TLS avec un certificat fourni :</span><span class="sxs-lookup"><span data-stu-id="cb935-706">Connections are secured by TLS with a supplied certificate:</span></span>

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

<span data-ttu-id="cb935-707">Le cas échéant, créez une implémentation `IConnectionAdapter` pour filtrer les négociations TLS par connexion pour des chiffrements spécifiques :</span><span class="sxs-lookup"><span data-stu-id="cb935-707">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

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
        // No encryption is used with a CipherAlgorithmType.Null cipher algorithm.
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

<span data-ttu-id="cb935-708">*Définir le protocole à partir de la configuration*</span><span class="sxs-lookup"><span data-stu-id="cb935-708">*Set the protocol from configuration*</span></span>

<span data-ttu-id="cb935-709">Par défaut, <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> appelle `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` pour charger la configuration de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-709"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="cb935-710">Dans l’exemple *appsettings.json* suivant, un protocole de connexion par défaut (HTTP/1.1 et HTTP/2) est établi pour l’ensemble des points de terminaison de Kestrel :</span><span class="sxs-lookup"><span data-stu-id="cb935-710">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="cb935-711">L’exemple de fichier de configuration suivant établit un protocole de connexion pour un point de terminaison spécifique :</span><span class="sxs-lookup"><span data-stu-id="cb935-711">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="cb935-712">Les protocoles spécifiés dans le code remplacent les valeurs définies par configuration.</span><span class="sxs-lookup"><span data-stu-id="cb935-712">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="cb935-713">Configuration du transport</span><span class="sxs-lookup"><span data-stu-id="cb935-713">Transport configuration</span></span>

<span data-ttu-id="cb935-714">Dans ASP.NET Core 2.1, le transport par défaut de Kestrel n’est plus basé sur Libuv, mais sur des sockets managés.</span><span class="sxs-lookup"><span data-stu-id="cb935-714">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="cb935-715">Il s’agit d’un changement cassant pour les applications ASP.NET Core 2.0 mises à niveau vers la version 2.1 qui appellent <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> et qui dépendent d’un des packages suivants :</span><span class="sxs-lookup"><span data-stu-id="cb935-715">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="cb935-716">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (référence de package directe)</span><span class="sxs-lookup"><span data-stu-id="cb935-716">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="cb935-717">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="cb935-717">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="cb935-718">Pour les projets qui requièrent l’utilisation de Libuv :</span><span class="sxs-lookup"><span data-stu-id="cb935-718">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="cb935-719">Ajoutez une dépendance pour le package [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) au fichier projet de l’application :</span><span class="sxs-lookup"><span data-stu-id="cb935-719">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="cb935-720">Appelez <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> :</span><span class="sxs-lookup"><span data-stu-id="cb935-720">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="cb935-721">Préfixes d’URL</span><span class="sxs-lookup"><span data-stu-id="cb935-721">URL prefixes</span></span>

<span data-ttu-id="cb935-722">Quand vous utilisez `UseUrls`, l’argument de ligne de commande `--urls`, la clé de configuration d’hôte `urls` ou la variable d’environnement `ASPNETCORE_URLS`, les préfixes d’URL peuvent être dans un des formats suivants.</span><span class="sxs-lookup"><span data-stu-id="cb935-722">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="cb935-723">Seuls les préfixes d’URL HTTP sont valides.</span><span class="sxs-lookup"><span data-stu-id="cb935-723">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="cb935-724">Kestrel ne prend pas en charge HTTPS lors de la configuration de liaisons d’URL avec `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="cb935-724">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="cb935-725">Adresse IPv4 avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="cb935-725">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="cb935-726">`0.0.0.0` est un cas spécial qui se lie à toutes les adresses IPv4.</span><span class="sxs-lookup"><span data-stu-id="cb935-726">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="cb935-727">Adresse IPv6 avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="cb935-727">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="cb935-728">`[::]` est l’équivalent IPv6 de `0.0.0.0` dans IPv4.</span><span class="sxs-lookup"><span data-stu-id="cb935-728">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="cb935-729">Nom d’hôte avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="cb935-729">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="cb935-730">Les noms d’hôte, `*` et `+` ne sont pas spéciaux.</span><span class="sxs-lookup"><span data-stu-id="cb935-730">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="cb935-731">Tout ce qui n’est pas reconnu comme adresse IP valide ou `localhost` se lie à toutes les adresses IP IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="cb935-731">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="cb935-732">Pour lier différents noms d’hôte à différentes applications ASP.NET Core sur le même port, utilisez [HTTP.sys](xref:fundamentals/servers/httpsys) ou un serveur proxy inverse, comme IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="cb935-732">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="cb935-733">L’hébergement dans une configuration de proxy inverse nécessite le [filtrage d’hôte](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="cb935-733">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="cb935-734">Nom `localhost` de l’hôte avec numéro de port ou adresse IP de bouclage avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="cb935-734">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="cb935-735">Quand `localhost` est spécifié, Kestrel tente de se lier aux interfaces de bouclage IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="cb935-735">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="cb935-736">Si le port demandé est en cours d’utilisation par un autre service sur l’une des interfaces de bouclage, Kestrel ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="cb935-736">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="cb935-737">Si l’une des interfaces de bouclage n’est pas disponible pour une raison quelconque (généralement du fait de la non-prise en charge d’IPv6), Kestrel journalise un avertissement.</span><span class="sxs-lookup"><span data-stu-id="cb935-737">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="cb935-738">Filtrage d’hôtes</span><span class="sxs-lookup"><span data-stu-id="cb935-738">Host filtering</span></span>

<span data-ttu-id="cb935-739">Si Kestrel prend en charge la configuration basée sur les préfixes, comme `http://example.com:5000`, il ignore en grande partie le nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="cb935-739">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="cb935-740">L’hôte `localhost` est un cas spécial utilisé pour la liaison à des adresses de bouclage.</span><span class="sxs-lookup"><span data-stu-id="cb935-740">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="cb935-741">Tout hôte autre qu’une adresse IP explicite se lie à toutes les adresses IP publiques.</span><span class="sxs-lookup"><span data-stu-id="cb935-741">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="cb935-742">Les en-têtes `Host` ne sont pas validés.</span><span class="sxs-lookup"><span data-stu-id="cb935-742">`Host` headers aren't validated.</span></span>

<span data-ttu-id="cb935-743">En guise de solution de contournement, utilisez le middleware de filtrage d’hôtes.</span><span class="sxs-lookup"><span data-stu-id="cb935-743">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="cb935-744">L’intergiciel (middleware) de filtrage d’hôte est fourni par le package [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) , qui est inclus dans le de [Microsoft. AspNetCore. App](xref:fundamentals/metapackage-app) (ASP.net Core 2,1 ou 2,2).</span><span class="sxs-lookup"><span data-stu-id="cb935-744">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="cb935-745">L’intergiciel (middleware) est ajouté par <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, qui appelle <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*> :</span><span class="sxs-lookup"><span data-stu-id="cb935-745">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="cb935-746">Le middleware de filtrage d’hôtes est désactivé par défaut.</span><span class="sxs-lookup"><span data-stu-id="cb935-746">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="cb935-747">Pour activer le middleware, définissez une clé `AllowedHosts` dans *appsettings.json*/*appsettings.\<nom_environnement>.json*.</span><span class="sxs-lookup"><span data-stu-id="cb935-747">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="cb935-748">La valeur est une liste délimitée par des points-virgules des noms d’hôte sans numéros de port :</span><span class="sxs-lookup"><span data-stu-id="cb935-748">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="cb935-749">*appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="cb935-749">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="cb935-750">Le [middleware des en-têtes transférés](xref:host-and-deploy/proxy-load-balancer) a aussi une option <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="cb935-750">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="cb935-751">Le middleware des en-têtes transférés et le middleware de filtrage d’hôtes ont des fonctionnalités similaires pour différents scénarios.</span><span class="sxs-lookup"><span data-stu-id="cb935-751">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="cb935-752">La définition d’`AllowedHosts` avec le middleware des en-têtes transférés est appropriée quand l’en-tête `Host` n’est pas conservé durant le transfert des requêtes avec un serveur proxy inverse ou un équilibreur de charge.</span><span class="sxs-lookup"><span data-stu-id="cb935-752">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="cb935-753">La définition d’`AllowedHosts` avec le middleware de filtrage d’hôtes est appropriée quand Kestrel est utilisé comme serveur de périphérie public ou que l’en-tête `Host` est directement transféré.</span><span class="sxs-lookup"><span data-stu-id="cb935-753">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="cb935-754">Pour plus d’informations sur le middleware des en-têtes transférés, consultez <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="cb935-754">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="cb935-755">Kestrel est un [serveur web multiplateforme pour ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="cb935-755">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="cb935-756">Kestrel est le serveur web inclus par défaut dans les modèles de projets ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cb935-756">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="cb935-757">Kestrel prend en charge les scénarios suivants :</span><span class="sxs-lookup"><span data-stu-id="cb935-757">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="cb935-758">HTTPS</span><span class="sxs-lookup"><span data-stu-id="cb935-758">HTTPS</span></span>
* <span data-ttu-id="cb935-759">Mise à niveau opaque utilisée pour activer les [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="cb935-759">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="cb935-760">Sockets UNIX pour des performances élevées derrière Nginx</span><span class="sxs-lookup"><span data-stu-id="cb935-760">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="cb935-761">Kestrel est pris en charge sur toutes les plateformes et les versions prises en charge par .NET Core.</span><span class="sxs-lookup"><span data-stu-id="cb935-761">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="cb935-762">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cb935-762">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="cb935-763">Quand utiliser Kestrel avec un proxy inverse ?</span><span class="sxs-lookup"><span data-stu-id="cb935-763">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="cb935-764">Vous pouvez utiliser Kestrel par lui-même ou avec un *serveur proxy inverse*, comme [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="cb935-764">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="cb935-765">Un serveur proxy inverse reçoit les requêtes HTTP en provenance du réseau et les transmet à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-765">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="cb935-766">Kestrel utilisé comme serveur web edge (accessible sur Internet) :</span><span class="sxs-lookup"><span data-stu-id="cb935-766">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel communique directement avec Internet sans serveur proxy inverse](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="cb935-768">Kestrel utilisé dans une configuration de proxy inverse :</span><span class="sxs-lookup"><span data-stu-id="cb935-768">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="cb935-770">L’une ou l’autre des configurations, avec ou sans serveur proxy inverse, est une configuration d’hébergement prise en charge.</span><span class="sxs-lookup"><span data-stu-id="cb935-770">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="cb935-771">Kestrel, s’il est utilisé comme serveur de périphérie sans serveur proxy inverse, ne prend pas en charge le partage de la même adresse IP et du même port entre plusieurs processus.</span><span class="sxs-lookup"><span data-stu-id="cb935-771">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="cb935-772">Quand Kestrel est configuré pour écouter sur un port, il gère tout le trafic pour ce port, quel que soit les en-têtes `Host` des requêtes.</span><span class="sxs-lookup"><span data-stu-id="cb935-772">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="cb935-773">Un proxy inverse qui peut partager des ports a la possibilité de transférer des requêtes à Kestrel sur une adresse IP et un port uniques.</span><span class="sxs-lookup"><span data-stu-id="cb935-773">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="cb935-774">Même si un serveur proxy inverse n’est pas nécessaire, en utiliser un peut être un bon choix.</span><span class="sxs-lookup"><span data-stu-id="cb935-774">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="cb935-775">Un proxy inverse :</span><span class="sxs-lookup"><span data-stu-id="cb935-775">A reverse proxy:</span></span>

* <span data-ttu-id="cb935-776">Peut limiter la surface publique exposée des applications qu’il héberge.</span><span class="sxs-lookup"><span data-stu-id="cb935-776">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="cb935-777">Fournit une couche supplémentaire de configuration et de défense.</span><span class="sxs-lookup"><span data-stu-id="cb935-777">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="cb935-778">Peut mieux s’intégrer à l’infrastructure existante.</span><span class="sxs-lookup"><span data-stu-id="cb935-778">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="cb935-779">Simplifie la configuration de l’équilibrage de charge et d’une communication sécurisée (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="cb935-779">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="cb935-780">Seul le serveur proxy inverse requiert un certificat X. 509 et ce serveur peut communiquer avec les serveurs de l’application sur le réseau interne à l’aide du protocole HTTP simple.</span><span class="sxs-lookup"><span data-stu-id="cb935-780">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="cb935-781">L’hébergement dans une configuration de proxy inverse nécessite le [filtrage d’hôte](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="cb935-781">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="cb935-782">Comment utiliser Kestrel dans les applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cb935-782">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="cb935-783">Le package [Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) est inclus dans le sous- [package Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="cb935-783">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="cb935-784">Les modèles de projet ASP.NET Core utilisent Kestrel par défaut.</span><span class="sxs-lookup"><span data-stu-id="cb935-784">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="cb935-785">Dans *Program.cs*, le modèle de code appelle <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, qui appelle <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="cb935-785">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

<span data-ttu-id="cb935-786">Pour fournir une configuration supplémentaire après l’appel de `CreateDefaultBuilder`, appelez <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> :</span><span class="sxs-lookup"><span data-stu-id="cb935-786">To provide additional configuration after calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="cb935-787">Pour plus d’informations sur la `CreateDefaultBuilder` et la génération de l’hôte, consultez la section *configurer un hôte* de <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="cb935-787">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="kestrel-options"></a><span data-ttu-id="cb935-788">Options Kestrel</span><span class="sxs-lookup"><span data-stu-id="cb935-788">Kestrel options</span></span>

<span data-ttu-id="cb935-789">Le serveur web Kestrel a des options de configuration de contrainte qui sont particulièrement utiles dans les déploiements exposés à Internet.</span><span class="sxs-lookup"><span data-stu-id="cb935-789">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="cb935-790">Définissez des contraintes sur la propriété <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> de la classe <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="cb935-790">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="cb935-791">La propriété `Limits` conserve une instance de la classe <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="cb935-791">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="cb935-792">Les exemples suivants utilisent l’espace de noms <xref:Microsoft.AspNetCore.Server.Kestrel.Core> :</span><span class="sxs-lookup"><span data-stu-id="cb935-792">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="cb935-793">Les options Kestrel, qui sont configurées dans C# le code des exemples suivants, peuvent également être définies à l’aide d’un fournisseur de [configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="cb935-793">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="cb935-794">Par exemple, le fournisseur de configuration de fichier peut charger la configuration Kestrel à partir d’un fichier *appSettings. JSON* ou *appSettings. { Fichier Environment}. JSON* :</span><span class="sxs-lookup"><span data-stu-id="cb935-794">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

```json
{
  "Kestrel": {
    "Limits": {
      "MaxConcurrentConnections": 100,
      "MaxConcurrentUpgradedConnections": 100
    }
  }
}
```

<span data-ttu-id="cb935-795">Utilisez l' **une** des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="cb935-795">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="cb935-796">Configurer Kestrel dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="cb935-796">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="cb935-797">Injecte une instance de `IConfiguration` dans la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="cb935-797">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="cb935-798">L’exemple suivant suppose que la configuration injectée est assignée à la propriété `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="cb935-798">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="cb935-799">Dans `Startup.ConfigureServices`, chargez la section `Kestrel` de la configuration dans la configuration de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-799">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="cb935-800">Configurez Kestrel lors de la génération de l’hôte :</span><span class="sxs-lookup"><span data-stu-id="cb935-800">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="cb935-801">Dans *Program.cs*, chargez la section `Kestrel` de la configuration dans la configuration de Kestrel :</span><span class="sxs-lookup"><span data-stu-id="cb935-801">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

  ```csharp
  // using Microsoft.Extensions.DependencyInjection;

  public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
      WebHost.CreateDefaultBuilder(args)
          .ConfigureServices((context, services) =>
          {
              services.Configure<KestrelServerOptions>(
                  context.Configuration.GetSection("Kestrel"));
          })
          .UseStartup<Startup>();
  ```

<span data-ttu-id="cb935-802">Les deux approches précédentes fonctionnent avec n’importe quel [fournisseur de configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="cb935-802">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="cb935-803">Délai d’expiration toujours actif</span><span class="sxs-lookup"><span data-stu-id="cb935-803">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="cb935-804">Obtient ou définit le [délai d’expiration toujours actif](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="cb935-804">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="cb935-805">La valeur par défaut est de 2 minutes.</span><span class="sxs-lookup"><span data-stu-id="cb935-805">Defaults to 2 minutes.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
        });
```

### <a name="maximum-client-connections"></a><span data-ttu-id="cb935-806">Nombre maximale de connexions client</span><span class="sxs-lookup"><span data-stu-id="cb935-806">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="cb935-807">Le nombre maximal de connexions TCP ouvertes simultanées peut être défini pour l’application entière avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="cb935-807">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentConnections = 100;
        });
```

<span data-ttu-id="cb935-808">Il existe une limite distincte pour les connexions qui ont été mises à niveau à partir de HTTP ou HTTPS vers un autre protocole (par exemple, sur une demande WebSocket).</span><span class="sxs-lookup"><span data-stu-id="cb935-808">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="cb935-809">Une fois mise à niveau, une connexion n’est pas prise en compte dans la limite `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="cb935-809">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

<span data-ttu-id="cb935-810">Le nombre maximal de connexions est illimité (null) par défaut.</span><span class="sxs-lookup"><span data-stu-id="cb935-810">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="cb935-811">Taille maximale du corps de la requête</span><span class="sxs-lookup"><span data-stu-id="cb935-811">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="cb935-812">La taille maximale par défaut du corps de la requête est de 30 000 000 octets, soit environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="cb935-812">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="cb935-813">Pour remplacer la limite dans une application ASP.NET Core MVC, nous vous recommandons d’utiliser l’attribut <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> sur une méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="cb935-813">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="cb935-814">Voici un exemple qui montre comment configurer la contrainte pour l’application sur chaque requête :</span><span class="sxs-lookup"><span data-stu-id="cb935-814">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="cb935-815">Remplacer le paramètre sur une demande spécifique dans l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="cb935-815">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="cb935-816">Une exception est levée si l’application configure la limite sur une demande après que l’application a commencé à lire la demande.</span><span class="sxs-lookup"><span data-stu-id="cb935-816">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="cb935-817">Il existe une propriété `IsReadOnly` qui indique si la propriété `MaxRequestBodySize` est en lecture seule ; si tel est le cas, il est trop tard pour configurer la limite.</span><span class="sxs-lookup"><span data-stu-id="cb935-817">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="cb935-818">Quand une application est exécutée [hors processus](xref:host-and-deploy/iis/index#out-of-process-hosting-model) derrière le [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module), la limite de taille du corps de demande de Kestrel est désactivée, car IIS définit déjà la limite.</span><span class="sxs-lookup"><span data-stu-id="cb935-818">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="cb935-819">Débit données minimal du corps de la requête</span><span class="sxs-lookup"><span data-stu-id="cb935-819">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="cb935-820">Kestrel vérifie à chaque seconde si les données arrivent au débit spécifié en octets/seconde.</span><span class="sxs-lookup"><span data-stu-id="cb935-820">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="cb935-821">Si le taux chute au-dessous de la valeur minimale, le délai d’attente de la connexion est dépassé. La période de grâce correspond à la durée pendant laquelle Kestrel permet au client d’augmenter son taux d’envoi jusqu’à la valeur minimale. la vitesse n’est pas vérifiée pendant cette période.</span><span class="sxs-lookup"><span data-stu-id="cb935-821">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="cb935-822">La période de grâce permet d’éviter la suppression des connexions qui, initialement, envoient des données à une vitesse lente en raison de la lenteur du démarrage de TCP.</span><span class="sxs-lookup"><span data-stu-id="cb935-822">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="cb935-823">Le débit minimal par défaut est 240 octets/seconde, avec une période de grâce de 5 secondes.</span><span class="sxs-lookup"><span data-stu-id="cb935-823">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="cb935-824">Un débit minimal s’applique également à la réponse.</span><span class="sxs-lookup"><span data-stu-id="cb935-824">A minimum rate also applies to the response.</span></span> <span data-ttu-id="cb935-825">Le code pour définir les limites de demande et de réponse est identique à l’exception de `RequestBody` ou `Response` dans les noms de propriété et d’interface.</span><span class="sxs-lookup"><span data-stu-id="cb935-825">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="cb935-826">Voici un exemple qui montre comment configurer les débits de données minimaux dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="cb935-826">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

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

### <a name="request-headers-timeout"></a><span data-ttu-id="cb935-827">Délai d’expiration des en-têtes de requête</span><span class="sxs-lookup"><span data-stu-id="cb935-827">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="cb935-828">Obtient ou définit le temps maximal passé par le serveur à recevoir des en-têtes de requête.</span><span class="sxs-lookup"><span data-stu-id="cb935-828">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="cb935-829">La valeur par défaut est de 30 secondes.</span><span class="sxs-lookup"><span data-stu-id="cb935-829">Defaults to 30 seconds.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(1);
        });
```

### <a name="synchronous-io"></a><span data-ttu-id="cb935-830">E/S synchrone</span><span class="sxs-lookup"><span data-stu-id="cb935-830">Synchronous IO</span></span>

<span data-ttu-id="cb935-831"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> contrôle si l’e/s synchrone est autorisée pour la requête et la réponse.</span><span class="sxs-lookup"><span data-stu-id="cb935-831"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="cb935-832">La valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="cb935-832">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="cb935-833">Un grand nombre d’opérations d’e/s synchrones de blocage peut entraîner une privation de pool de thread, ce qui fait que l’application ne répond pas.</span><span class="sxs-lookup"><span data-stu-id="cb935-833">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="cb935-834">Activez uniquement `AllowSynchronousIO` lors de l’utilisation d’une bibliothèque qui ne prend pas en charge l’e/s asynchrone.</span><span class="sxs-lookup"><span data-stu-id="cb935-834">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="cb935-835">L’exemple suivant désactive un e/s synchrone :</span><span class="sxs-lookup"><span data-stu-id="cb935-835">The following example disables synchronous IO:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.AllowSynchronousIO = false;
        });
```

<span data-ttu-id="cb935-836">Pour plus d’informations sur les autres options et limites de Kestrel, consultez :</span><span class="sxs-lookup"><span data-stu-id="cb935-836">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="cb935-837">Configuration de point de terminaison</span><span class="sxs-lookup"><span data-stu-id="cb935-837">Endpoint configuration</span></span>

<span data-ttu-id="cb935-838">Par défaut, ASP.NET Core se lie à :</span><span class="sxs-lookup"><span data-stu-id="cb935-838">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="cb935-839">`https://localhost:5001` (quand un certificat de développement local est présent)</span><span class="sxs-lookup"><span data-stu-id="cb935-839">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="cb935-840">Spécifiez les URL avec :</span><span class="sxs-lookup"><span data-stu-id="cb935-840">Specify URLs using the:</span></span>

* <span data-ttu-id="cb935-841">La variable d’environnement `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="cb935-841">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="cb935-842">L’argument de ligne de commande `--urls`.</span><span class="sxs-lookup"><span data-stu-id="cb935-842">`--urls` command-line argument.</span></span>
* <span data-ttu-id="cb935-843">La clé de configuration d’hôte `urls`.</span><span class="sxs-lookup"><span data-stu-id="cb935-843">`urls` host configuration key.</span></span>
* <span data-ttu-id="cb935-844">La méthode d’extension `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="cb935-844">`UseUrls` extension method.</span></span>

<span data-ttu-id="cb935-845">La valeur fournie avec ces approches peut être un ou plusieurs points de terminaison HTTP et HTTPS (HTTPS si un certificat par défaut est disponible).</span><span class="sxs-lookup"><span data-stu-id="cb935-845">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="cb935-846">Configurez la valeur sous forme de liste délimitée par des points-virgules (par exemple `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="cb935-846">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="cb935-847">Pour plus d’informations sur ces approches, voir [URL de serveur](xref:fundamentals/host/web-host#server-urls) et [Remplacer la configuration](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="cb935-847">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="cb935-848">Un certificat de développement est créé :</span><span class="sxs-lookup"><span data-stu-id="cb935-848">A development certificate is created:</span></span>

* <span data-ttu-id="cb935-849">Quand le [SDK .NET Core](/dotnet/core/sdk) est installé.</span><span class="sxs-lookup"><span data-stu-id="cb935-849">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="cb935-850">[L’outil dev-certs](xref:aspnetcore-2.1#https) est utilisé pour créer un certificat.</span><span class="sxs-lookup"><span data-stu-id="cb935-850">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="cb935-851">Certains navigateurs requièrent l’octroi d’une autorisation explicite pour approuver le certificat de développement local.</span><span class="sxs-lookup"><span data-stu-id="cb935-851">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="cb935-852">Les modèles de projet configurent les applications à exécuter sur HTTPs par défaut et incluent [la redirection https et la prise en charge de HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="cb935-852">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="cb935-853">Appelez les méthodes <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> ou <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> sur <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> pour configurer les préfixes et les ports d’URL pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-853">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="cb935-854">`UseUrls`, l’argument de ligne de commande `--urls`, la clé de configuration d’hôte `urls` et la variable d’environnement `ASPNETCORE_URLS` fonctionnent également, mais ils présentent les limitations indiquées plus loin dans cette section (un certificat par défaut doit être disponible pour la configuration du point de terminaison HTTPS).</span><span class="sxs-lookup"><span data-stu-id="cb935-854">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="cb935-855">configuration de `KestrelServerOptions` :</span><span class="sxs-lookup"><span data-stu-id="cb935-855">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="cb935-856">ConfigureEndpointDefaults (action\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="cb935-856">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="cb935-857">Spécifie une `Action` de configuration à exécuter pour chaque point de terminaison spécifié.</span><span class="sxs-lookup"><span data-stu-id="cb935-857">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="cb935-858">Le fait d’appeler `ConfigureEndpointDefaults` plusieurs fois remplace les `Action`s précédentes par la dernière `Action` spécifiée.</span><span class="sxs-lookup"><span data-stu-id="cb935-858">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="cb935-859">ConfigureHttpsDefaults (action\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="cb935-859">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="cb935-860">Spécifie une `Action` de configuration à exécuter pour chaque point de terminaison HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cb935-860">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="cb935-861">Le fait d’appeler `ConfigureHttpsDefaults` plusieurs fois remplace les `Action`s précédentes par la dernière `Action` spécifiée.</span><span class="sxs-lookup"><span data-stu-id="cb935-861">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configureiconfiguration"></a><span data-ttu-id="cb935-862">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="cb935-862">Configure(IConfiguration)</span></span>

<span data-ttu-id="cb935-863">Crée un chargeur de configuration pour configurer Kestrel, qui prend en entrée une <xref:Microsoft.Extensions.Configuration.IConfiguration>.</span><span class="sxs-lookup"><span data-stu-id="cb935-863">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="cb935-864">La configuration doit être limitée à la section de configuration pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-864">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="cb935-865">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="cb935-865">ListenOptions.UseHttps</span></span>

<span data-ttu-id="cb935-866">Configure Kestrel pour l’utilisation de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cb935-866">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="cb935-867">Extensions de `ListenOptions.UseHttps` :</span><span class="sxs-lookup"><span data-stu-id="cb935-867">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="cb935-868">`UseHttps` &ndash; Configure Kestrel pour l’utilisation de HTTPS avec le certificat par défaut.</span><span class="sxs-lookup"><span data-stu-id="cb935-868">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="cb935-869">Lève une exception si aucun certificat par défaut n’est configuré.</span><span class="sxs-lookup"><span data-stu-id="cb935-869">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="cb935-870">Paramètres de `ListenOptions.UseHttps` :</span><span class="sxs-lookup"><span data-stu-id="cb935-870">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="cb935-871">`filename` est le chemin et le nom d’un fichier de certificat, relatif au répertoire qui contient les fichiers de contenu de l’application.</span><span class="sxs-lookup"><span data-stu-id="cb935-871">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="cb935-872">`password` est le mot de passe nécessaire pour accéder aux données du certificat X.509.</span><span class="sxs-lookup"><span data-stu-id="cb935-872">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="cb935-873">`configureOptions` est une `Action` pour configurer les `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="cb935-873">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="cb935-874">Retourne l'`ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="cb935-874">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="cb935-875">`storeName` est le magasin de certificats à partir duquel charger le certificat.</span><span class="sxs-lookup"><span data-stu-id="cb935-875">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="cb935-876">`subject` est le nom du sujet du certificat.</span><span class="sxs-lookup"><span data-stu-id="cb935-876">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="cb935-877">`allowInvalid` indique si les certificats non valides doivent être considérés, comme les certificats auto-signés.</span><span class="sxs-lookup"><span data-stu-id="cb935-877">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="cb935-878">`location` est l’emplacement du magasin à partir duquel charger le certificat.</span><span class="sxs-lookup"><span data-stu-id="cb935-878">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="cb935-879">`serverCertificate` est le certificat X.509.</span><span class="sxs-lookup"><span data-stu-id="cb935-879">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="cb935-880">En production, HTTPS doit être explicitement configuré.</span><span class="sxs-lookup"><span data-stu-id="cb935-880">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="cb935-881">Au minimum, un certificat par défaut doit être fourni.</span><span class="sxs-lookup"><span data-stu-id="cb935-881">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="cb935-882">Configurations prises en charge décrites dans la suite :</span><span class="sxs-lookup"><span data-stu-id="cb935-882">Supported configurations described next:</span></span>

* <span data-ttu-id="cb935-883">Pas de configuration</span><span class="sxs-lookup"><span data-stu-id="cb935-883">No configuration</span></span>
* <span data-ttu-id="cb935-884">Remplacer le certificat par défaut dans la configuration</span><span class="sxs-lookup"><span data-stu-id="cb935-884">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="cb935-885">Changer les valeurs par défaut dans le code</span><span class="sxs-lookup"><span data-stu-id="cb935-885">Change the defaults in code</span></span>

<span data-ttu-id="cb935-886">*Pas de configuration*</span><span class="sxs-lookup"><span data-stu-id="cb935-886">*No configuration*</span></span>

<span data-ttu-id="cb935-887">Kestrel écoute sur `http://localhost:5000` et sur `https://localhost:5001` (si un certificat par défaut est disponible).</span><span class="sxs-lookup"><span data-stu-id="cb935-887">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="cb935-888">*Remplacer le certificat par défaut dans la configuration*</span><span class="sxs-lookup"><span data-stu-id="cb935-888">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="cb935-889">Par défaut, `CreateDefaultBuilder` appelle `Configure(context.Configuration.GetSection("Kestrel"))` pour charger la configuration de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-889">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="cb935-890">Un schéma de configuration des paramètres d’application HTTPS par défaut est disponible pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-890">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="cb935-891">Configurez plusieurs points de terminaison, notamment les URL et les certificats à utiliser, à partir d’un fichier sur disque ou d’un magasin de certificats.</span><span class="sxs-lookup"><span data-stu-id="cb935-891">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="cb935-892">Dans l’exemple de fichier *appsettings.json* suivant :</span><span class="sxs-lookup"><span data-stu-id="cb935-892">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="cb935-893">Définissez **AllowInvalid** sur `true` pour permettre l’utilisation de certificats non valides (par exemple des certificats auto-signés).</span><span class="sxs-lookup"><span data-stu-id="cb935-893">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="cb935-894">Les points de terminaison HTTPS qui ne spécifient pas de certificat (**HttpsDefaultCert** dans l’exemple qui suit) utilise comme alternative de secours le certificat défini sous **Certificats** > **Par défaut**  ou bien le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="cb935-894">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="cb935-895">Une alternative à l’utilisation de **Chemin** et de **Mot de passe** pour un nœud de certificat consiste à spécifier le certificat avec des champs du magasin de certificats.</span><span class="sxs-lookup"><span data-stu-id="cb935-895">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="cb935-896">Par exemple, le certificat **Certificats** > **Par défaut** peut être spécifié en tant que :</span><span class="sxs-lookup"><span data-stu-id="cb935-896">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="cb935-897">Notes de schéma :</span><span class="sxs-lookup"><span data-stu-id="cb935-897">Schema notes:</span></span>

* <span data-ttu-id="cb935-898">Les noms des points de terminaison ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="cb935-898">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="cb935-899">Par exemple, `HTTPS` et `Https` sont valides.</span><span class="sxs-lookup"><span data-stu-id="cb935-899">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="cb935-900">Le paramètre `Url` est obligatoire pour chaque point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="cb935-900">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="cb935-901">Le format de ce paramètre est le même que celui du paramètre de configuration `Urls` du plus haut niveau, sauf qu’il est limité à une seule valeur.</span><span class="sxs-lookup"><span data-stu-id="cb935-901">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="cb935-902">Ces points de terminaison remplacent ceux qui sont définis dans le paramètre de configuration `Urls` du plus haut niveau configuration, au lieu de s’y ajouter.</span><span class="sxs-lookup"><span data-stu-id="cb935-902">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="cb935-903">Les points de terminaison définis dans le code via `Listen` sont cumulatifs avec les points de terminaison définis dans la section de configuration.</span><span class="sxs-lookup"><span data-stu-id="cb935-903">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="cb935-904">La section `Certificate` est facultative.</span><span class="sxs-lookup"><span data-stu-id="cb935-904">The `Certificate` section is optional.</span></span> <span data-ttu-id="cb935-905">Si la section `Certificate` n’est pas spécifiée, les valeurs par défaut définies dans les scénarios précédents sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="cb935-905">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="cb935-906">Si aucune valeur par défaut n’est disponible, le serveur lève une exception et son démarrage échoue.</span><span class="sxs-lookup"><span data-stu-id="cb935-906">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="cb935-907">La section `Certificate` prend en charge les certificats **Chemin**&ndash;**Mot de passe** et **Sujet**&ndash;**Magasin**.</span><span class="sxs-lookup"><span data-stu-id="cb935-907">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="cb935-908">Vous pouvez définir un nombre quelconque de points de terminaison de cette façon, pour autant qu’ils ne provoquent pas de conflits de port.</span><span class="sxs-lookup"><span data-stu-id="cb935-908">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="cb935-909">`options.Configure(context.Configuration.GetSection("{SECTION}"))` retourne un `KestrelConfigurationLoader` avec une méthode `.Endpoint(string name, listenOptions => { })` qui peut être utilisée pour compléter les paramètres d’un point de terminaison configuré :</span><span class="sxs-lookup"><span data-stu-id="cb935-909">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="cb935-910">`KestrelServerOptions.ConfigurationLoader` est accessible directement pour poursuivre l’itération sur le chargeur existant, tel que celui fourni par <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="cb935-910">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="cb935-911">La section de configuration pour chaque point de terminaison est disponible sur les options de la méthode `Endpoint` afin que les paramètres personnalisés puissent être lus.</span><span class="sxs-lookup"><span data-stu-id="cb935-911">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="cb935-912">Plusieurs configurations peuvent être chargées en rappelant `options.Configure(context.Configuration.GetSection("{SECTION}"))` avec une autre section.</span><span class="sxs-lookup"><span data-stu-id="cb935-912">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="cb935-913">Seule la dernière configuration est utilisée, à moins que `Load` soit explicitement appelé sur les instances précédentes.</span><span class="sxs-lookup"><span data-stu-id="cb935-913">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="cb935-914">Le métapackage n’appelle pas `Load` : sa section de configuration par défaut peut donc être remplacée.</span><span class="sxs-lookup"><span data-stu-id="cb935-914">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="cb935-915">`KestrelConfigurationLoader` reflète la famille d’API `Listen` de `KestrelServerOptions` sous forme de surcharges de `Endpoint` : le code et les points de terminaison de configuration peuvent donc être configurés au même emplacement.</span><span class="sxs-lookup"><span data-stu-id="cb935-915">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="cb935-916">Ces surcharges n’utilisent pas de noms et consomment seulement les paramètres par défaut de la configuration.</span><span class="sxs-lookup"><span data-stu-id="cb935-916">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="cb935-917">*Changer les valeurs par défaut dans le code*</span><span class="sxs-lookup"><span data-stu-id="cb935-917">*Change the defaults in code*</span></span>

<span data-ttu-id="cb935-918">`ConfigureEndpointDefaults` et `ConfigureHttpsDefaults` peuvent être utilisés pour modifier les paramètres par défaut pour `ListenOptions` et `HttpsConnectionAdapterOptions`, notamment en remplaçant le certificat par défaut spécifié dans le scénario précédent.</span><span class="sxs-lookup"><span data-stu-id="cb935-918">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="cb935-919">`ConfigureEndpointDefaults` et `ConfigureHttpsDefaults` doivent être appelés avant que des points de terminaison soient configurés.</span><span class="sxs-lookup"><span data-stu-id="cb935-919">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="cb935-920">*Prise en charge de SNI par Kestrel*</span><span class="sxs-lookup"><span data-stu-id="cb935-920">*Kestrel support for SNI*</span></span>

<span data-ttu-id="cb935-921">[L’indication du nom de serveur (SNI, Server Name Indication)](https://tools.ietf.org/html/rfc6066#section-3) peut être utilisée pour héberger plusieurs domaines sur la même adresse IP et le même port.</span><span class="sxs-lookup"><span data-stu-id="cb935-921">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="cb935-922">Pour que SNI fonctionne, le client envoie le nom d’hôte pour la session sécurisée au serveur pendant la négociation TLS afin que le serveur puisse fournir le certificat correct.</span><span class="sxs-lookup"><span data-stu-id="cb935-922">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="cb935-923">Le client utilise le certificat fourni pour la communication chiffrée avec le serveur pendant la session sécurisée qui suit la négociation TLS.</span><span class="sxs-lookup"><span data-stu-id="cb935-923">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="cb935-924">Kestrel prend en charge SNI via le rappel de `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="cb935-924">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="cb935-925">Le rappel est appelé une fois par connexion pour permettre à l’application d’inspecter le nom d’hôte et de sélectionner le certificat approprié.</span><span class="sxs-lookup"><span data-stu-id="cb935-925">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="cb935-926">La prise en charge de SNI nécessite les points suivants :</span><span class="sxs-lookup"><span data-stu-id="cb935-926">SNI support requires:</span></span>

* <span data-ttu-id="cb935-927">Exécution sur la version cible de .NET Framework `netcoreapp2.1` ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="cb935-927">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="cb935-928">Sur `net461` ou version ultérieure, le rappel est appelé, mais la `name` est toujours `null`.</span><span class="sxs-lookup"><span data-stu-id="cb935-928">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="cb935-929">`name` est également `null` si le client ne fournit pas le paramètre du nom d’hôte dans la négociation TLS.</span><span class="sxs-lookup"><span data-stu-id="cb935-929">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="cb935-930">Tous les sites web s’exécutent sur la même instance Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-930">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="cb935-931">Kestrel ne prend pas en charge le partage d’une adresse IP et d’un port entre plusieurs instances sans un proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="cb935-931">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="cb935-932">Journalisation des connexions</span><span class="sxs-lookup"><span data-stu-id="cb935-932">Connection logging</span></span>

<span data-ttu-id="cb935-933">Appelez <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> pour émettre des journaux au niveau du débogage pour la communication au niveau des octets sur une connexion.</span><span class="sxs-lookup"><span data-stu-id="cb935-933">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="cb935-934">La journalisation des connexions est utile pour résoudre les problèmes de communication de bas niveau, par exemple lors du chiffrement TLS et derrière les proxies.</span><span class="sxs-lookup"><span data-stu-id="cb935-934">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="cb935-935">Si `UseConnectionLogging` est placé avant `UseHttps`, le trafic chiffré est journalisé.</span><span class="sxs-lookup"><span data-stu-id="cb935-935">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="cb935-936">Si `UseConnectionLogging` est placé après `UseHttps`, le trafic déchiffré est journalisé.</span><span class="sxs-lookup"><span data-stu-id="cb935-936">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="cb935-937">Lier à un socket TCP</span><span class="sxs-lookup"><span data-stu-id="cb935-937">Bind to a TCP socket</span></span>

<span data-ttu-id="cb935-938">La méthode <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> se lie à un socket TCP, et une expression lambda options permet la configuration d’un certificat X.509 :</span><span class="sxs-lookup"><span data-stu-id="cb935-938">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

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

<span data-ttu-id="cb935-939">L’exemple configure HTTPS pour un point de terminaison avec <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="cb935-939">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="cb935-940">Utilisez la même API afin de configurer d’autres paramètres Kestrel pour des points de terminaison spécifiques.</span><span class="sxs-lookup"><span data-stu-id="cb935-940">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="cb935-941">Lier à un socket Unix</span><span class="sxs-lookup"><span data-stu-id="cb935-941">Bind to a Unix socket</span></span>

<span data-ttu-id="cb935-942">Écoutez sur un socket Unix avec <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> pour améliorer les performances avec Nginx, comme illustré dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="cb935-942">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

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

### <a name="port-0"></a><span data-ttu-id="cb935-943">Port 0</span><span class="sxs-lookup"><span data-stu-id="cb935-943">Port 0</span></span>

<span data-ttu-id="cb935-944">Si vous spécifiez le numéro de port `0`, Kestrel se lie dynamiquement à un port disponible.</span><span class="sxs-lookup"><span data-stu-id="cb935-944">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="cb935-945">L’exemple suivant montre comment déterminer le port auquel Kestrel se lie à l’exécution :</span><span class="sxs-lookup"><span data-stu-id="cb935-945">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="cb935-946">Quand l’application est exécutée, la sortie de la fenêtre de console indique le port dynamique où l’application peut être atteinte :</span><span class="sxs-lookup"><span data-stu-id="cb935-946">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="cb935-947">Limitations</span><span class="sxs-lookup"><span data-stu-id="cb935-947">Limitations</span></span>

<span data-ttu-id="cb935-948">Configurez des points de terminaison avec les approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="cb935-948">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="cb935-949">Arguments de ligne de commande `--urls`</span><span class="sxs-lookup"><span data-stu-id="cb935-949">`--urls` command-line argument</span></span>
* <span data-ttu-id="cb935-950">La clé de configuration d’hôte `urls`</span><span class="sxs-lookup"><span data-stu-id="cb935-950">`urls` host configuration key</span></span>
* <span data-ttu-id="cb935-951">Variable d’environnement `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="cb935-951">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="cb935-952">Ces méthodes sont utiles si vous voulez que votre code fonctionne avec des serveurs autres que Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cb935-952">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="cb935-953">Toutefois, soyez conscient des limitations suivantes :</span><span class="sxs-lookup"><span data-stu-id="cb935-953">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="cb935-954">HTTPS ne peut pas être utilisé avec ces approches, sauf si un certificat par défaut est fourni dans la configuration du point de terminaison HTTPS (par exemple avec la configuration de `KestrelServerOptions` ou un fichier de configuration, comme illustré plus haut dans cette rubrique).</span><span class="sxs-lookup"><span data-stu-id="cb935-954">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="cb935-955">Quand les deux approches `Listen` et `UseUrls` sont utilisées simultanément, les points de terminaison `Listen` remplacent les points de terminaison `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="cb935-955">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="cb935-956">Configuration de point de terminaison IIS</span><span class="sxs-lookup"><span data-stu-id="cb935-956">IIS endpoint configuration</span></span>

<span data-ttu-id="cb935-957">Quand vous utilisez IIS, les liaisons d’URL pour IIS remplacent les liaisons qui sont définies par `Listen` ou par `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="cb935-957">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="cb935-958">Pour plus d’informations, consultez la rubrique [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="cb935-958">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="cb935-959">Configuration du transport</span><span class="sxs-lookup"><span data-stu-id="cb935-959">Transport configuration</span></span>

<span data-ttu-id="cb935-960">Dans ASP.NET Core 2.1, le transport par défaut de Kestrel n’est plus basé sur Libuv, mais sur des sockets managés.</span><span class="sxs-lookup"><span data-stu-id="cb935-960">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="cb935-961">Il s’agit d’un changement cassant pour les applications ASP.NET Core 2.0 mises à niveau vers la version 2.1 qui appellent <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> et qui dépendent d’un des packages suivants :</span><span class="sxs-lookup"><span data-stu-id="cb935-961">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="cb935-962">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (référence de package directe)</span><span class="sxs-lookup"><span data-stu-id="cb935-962">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="cb935-963">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="cb935-963">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="cb935-964">Pour les projets qui requièrent l’utilisation de Libuv :</span><span class="sxs-lookup"><span data-stu-id="cb935-964">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="cb935-965">Ajoutez une dépendance pour le package [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) au fichier projet de l’application :</span><span class="sxs-lookup"><span data-stu-id="cb935-965">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="cb935-966">Appelez <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> :</span><span class="sxs-lookup"><span data-stu-id="cb935-966">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="cb935-967">Préfixes d’URL</span><span class="sxs-lookup"><span data-stu-id="cb935-967">URL prefixes</span></span>

<span data-ttu-id="cb935-968">Quand vous utilisez `UseUrls`, l’argument de ligne de commande `--urls`, la clé de configuration d’hôte `urls` ou la variable d’environnement `ASPNETCORE_URLS`, les préfixes d’URL peuvent être dans un des formats suivants.</span><span class="sxs-lookup"><span data-stu-id="cb935-968">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="cb935-969">Seuls les préfixes d’URL HTTP sont valides.</span><span class="sxs-lookup"><span data-stu-id="cb935-969">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="cb935-970">Kestrel ne prend pas en charge HTTPS lors de la configuration de liaisons d’URL avec `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="cb935-970">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="cb935-971">Adresse IPv4 avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="cb935-971">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="cb935-972">`0.0.0.0` est un cas spécial qui se lie à toutes les adresses IPv4.</span><span class="sxs-lookup"><span data-stu-id="cb935-972">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="cb935-973">Adresse IPv6 avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="cb935-973">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="cb935-974">`[::]` est l’équivalent IPv6 de `0.0.0.0` dans IPv4.</span><span class="sxs-lookup"><span data-stu-id="cb935-974">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="cb935-975">Nom d’hôte avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="cb935-975">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="cb935-976">Les noms d’hôte, `*` et `+` ne sont pas spéciaux.</span><span class="sxs-lookup"><span data-stu-id="cb935-976">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="cb935-977">Tout ce qui n’est pas reconnu comme adresse IP valide ou `localhost` se lie à toutes les adresses IP IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="cb935-977">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="cb935-978">Pour lier différents noms d’hôte à différentes applications ASP.NET Core sur le même port, utilisez [HTTP.sys](xref:fundamentals/servers/httpsys) ou un serveur proxy inverse, comme IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="cb935-978">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="cb935-979">L’hébergement dans une configuration de proxy inverse nécessite le [filtrage d’hôte](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="cb935-979">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="cb935-980">Nom `localhost` de l’hôte avec numéro de port ou adresse IP de bouclage avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="cb935-980">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="cb935-981">Quand `localhost` est spécifié, Kestrel tente de se lier aux interfaces de bouclage IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="cb935-981">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="cb935-982">Si le port demandé est en cours d’utilisation par un autre service sur l’une des interfaces de bouclage, Kestrel ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="cb935-982">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="cb935-983">Si l’une des interfaces de bouclage n’est pas disponible pour une raison quelconque (généralement du fait de la non-prise en charge d’IPv6), Kestrel journalise un avertissement.</span><span class="sxs-lookup"><span data-stu-id="cb935-983">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="cb935-984">Filtrage d’hôtes</span><span class="sxs-lookup"><span data-stu-id="cb935-984">Host filtering</span></span>

<span data-ttu-id="cb935-985">Si Kestrel prend en charge la configuration basée sur les préfixes, comme `http://example.com:5000`, il ignore en grande partie le nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="cb935-985">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="cb935-986">L’hôte `localhost` est un cas spécial utilisé pour la liaison à des adresses de bouclage.</span><span class="sxs-lookup"><span data-stu-id="cb935-986">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="cb935-987">Tout hôte autre qu’une adresse IP explicite se lie à toutes les adresses IP publiques.</span><span class="sxs-lookup"><span data-stu-id="cb935-987">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="cb935-988">Les en-têtes `Host` ne sont pas validés.</span><span class="sxs-lookup"><span data-stu-id="cb935-988">`Host` headers aren't validated.</span></span>

<span data-ttu-id="cb935-989">En guise de solution de contournement, utilisez le middleware de filtrage d’hôtes.</span><span class="sxs-lookup"><span data-stu-id="cb935-989">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="cb935-990">L’intergiciel (middleware) de filtrage d’hôte est fourni par le package [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) , qui est inclus dans le de [Microsoft. AspNetCore. App](xref:fundamentals/metapackage-app) (ASP.net Core 2,1 ou 2,2).</span><span class="sxs-lookup"><span data-stu-id="cb935-990">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="cb935-991">L’intergiciel (middleware) est ajouté par <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, qui appelle <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*> :</span><span class="sxs-lookup"><span data-stu-id="cb935-991">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="cb935-992">Le middleware de filtrage d’hôtes est désactivé par défaut.</span><span class="sxs-lookup"><span data-stu-id="cb935-992">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="cb935-993">Pour activer le middleware, définissez une clé `AllowedHosts` dans *appsettings.json*/*appsettings.\<nom_environnement>.json*.</span><span class="sxs-lookup"><span data-stu-id="cb935-993">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="cb935-994">La valeur est une liste délimitée par des points-virgules des noms d’hôte sans numéros de port :</span><span class="sxs-lookup"><span data-stu-id="cb935-994">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="cb935-995">*appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="cb935-995">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="cb935-996">Le [middleware des en-têtes transférés](xref:host-and-deploy/proxy-load-balancer) a aussi une option <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="cb935-996">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="cb935-997">Le middleware des en-têtes transférés et le middleware de filtrage d’hôtes ont des fonctionnalités similaires pour différents scénarios.</span><span class="sxs-lookup"><span data-stu-id="cb935-997">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="cb935-998">La définition d’`AllowedHosts` avec le middleware des en-têtes transférés est appropriée quand l’en-tête `Host` n’est pas conservé durant le transfert des requêtes avec un serveur proxy inverse ou un équilibreur de charge.</span><span class="sxs-lookup"><span data-stu-id="cb935-998">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="cb935-999">La définition d’`AllowedHosts` avec le middleware de filtrage d’hôtes est appropriée quand Kestrel est utilisé comme serveur de périphérie public ou que l’en-tête `Host` est directement transféré.</span><span class="sxs-lookup"><span data-stu-id="cb935-999">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="cb935-1000">Pour plus d’informations sur le middleware des en-têtes transférés, consultez <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="cb935-1000">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="cb935-1001">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="cb935-1001">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="cb935-1002">Document RFC 7230 : syntaxe et routage des messages (section 5.4 : Hôte)</span><span class="sxs-lookup"><span data-stu-id="cb935-1002">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
