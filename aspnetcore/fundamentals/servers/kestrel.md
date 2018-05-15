---
title: Implémentation du serveur web Kestrel dans ASP.NET Core
author: tdykstra
description: Découvrez plus d’informations sur Kestrel, le serveur web multiplateforme pour ASP.NET Core basé sur libuv.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 1709d26f5dfe40d178da70c286d328982f2c39a0
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="06a15-103">Implémentation du serveur web Kestrel dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="06a15-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="06a15-104">Par [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) et [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="06a15-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="06a15-105">[Kestrel](xref:fundamentals/servers/index) est un serveur web multiplateforme pour ASP.NET Core basé sur [libuv](https://github.com/libuv/libuv), bibliothèque d’E/S asynchrones multiplateforme.</span><span class="sxs-lookup"><span data-stu-id="06a15-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="06a15-106">Kestrel est le serveur web inclus par défaut dans les modèles de projets ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="06a15-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="06a15-107">Kestrel prend en charge les fonctionnalités suivantes :</span><span class="sxs-lookup"><span data-stu-id="06a15-107">Kestrel supports the following features:</span></span>

* <span data-ttu-id="06a15-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="06a15-108">HTTPS</span></span>
* <span data-ttu-id="06a15-109">Mise à niveau opaque utilisée pour activer les [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="06a15-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="06a15-110">Sockets UNIX pour des performances élevées derrière Nginx</span><span class="sxs-lookup"><span data-stu-id="06a15-110">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="06a15-111">Kestrel est pris en charge sur toutes les plateformes et les versions prises en charge par .NET Core.</span><span class="sxs-lookup"><span data-stu-id="06a15-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="06a15-112">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="06a15-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="06a15-113">Quand utiliser Kestrel avec un proxy inverse ?</span><span class="sxs-lookup"><span data-stu-id="06a15-113">When to use Kestrel with a reverse proxy</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="06a15-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="06a15-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="06a15-115">Vous pouvez utiliser uniquement Kestrel ou l’associer à un *serveur proxy inverse*, par exemple IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="06a15-115">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="06a15-116">Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel après un traitement préliminaire.</span><span class="sxs-lookup"><span data-stu-id="06a15-116">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel communique directement avec Internet sans serveur proxy inverse](kestrel/_static/kestrel-to-internet2.png)

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="06a15-119">Nous vous recommandons d’utiliser Kestrel avec un serveur proxy inverse, sauf si Kestrel est exposé seulement à un réseau interne.</span><span class="sxs-lookup"><span data-stu-id="06a15-119">We recommend using Kestrel with a reverse proxy server unless Kestrel is only exposed to an internal network.</span></span>

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="06a15-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="06a15-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="06a15-121">Si une application accepte seulement les requêtes provenant d’un réseau interne, Kestrel peut être utilisé directement comme serveur de l’application.</span><span class="sxs-lookup"><span data-stu-id="06a15-121">If an app accepts requests only from an internal network, Kestrel can be used directly as the app's server.</span></span>

![Kestrel communique directement avec votre réseau interne](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="06a15-123">Si vous exposez votre application à Internet, vous devez utiliser IIS, Nginx ou Apache comme *serveur proxy inverse*.</span><span class="sxs-lookup"><span data-stu-id="06a15-123">If you expose your app to the Internet, use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="06a15-124">Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel après un traitement préliminaire.</span><span class="sxs-lookup"><span data-stu-id="06a15-124">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="06a15-126">Un proxy inverse est requis pour les déploiements Edge (exposés au trafic en provenance d’Internet) pour des raisons de sécurité.</span><span class="sxs-lookup"><span data-stu-id="06a15-126">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="06a15-127">Les versions 1.x de Kestrel ne disposent pas d’un ensemble complet de défenses contre les attaques, comme des délais d’expiration, des limites de taille et des limites du nombre de connexions simultanées.</span><span class="sxs-lookup"><span data-stu-id="06a15-127">The 1.x versions of Kestrel don't have a full complement of defenses against attacks, such as appropriate timeouts, size limits, and concurrent connection limits.</span></span>

* * *

<span data-ttu-id="06a15-128">Il existe un scénario de proxy inverse quand plusieurs applications partageant la même adresse IP et le même port s’exécutent sur un même serveur.</span><span class="sxs-lookup"><span data-stu-id="06a15-128">A reverse proxy scenario exists when there are multiple apps that share the same IP and port running on a single server.</span></span> <span data-ttu-id="06a15-129">Kestrel ne prend pas en charge ce scénario, car le partage de la même adresse IP et du même port entre plusieurs processus n’est pas pris en charge par Kestrel.</span><span class="sxs-lookup"><span data-stu-id="06a15-129">Kestrel doesn't support this scenario because Kestrel doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="06a15-130">Quand Kestrel est configuré pour écouter sur un port, il gère tout le trafic pour ce port, quel que soit l’en-tête de l’hôte des requêtes.</span><span class="sxs-lookup"><span data-stu-id="06a15-130">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' host header.</span></span> <span data-ttu-id="06a15-131">Un proxy inverse qui peut partager des ports a la possibilité de transférer des requêtes à Kestrel sur une adresse IP et un port uniques.</span><span class="sxs-lookup"><span data-stu-id="06a15-131">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="06a15-132">Même si un serveur proxy inverse n’est pas nécessaire, en utiliser un peut être un bon choix :</span><span class="sxs-lookup"><span data-stu-id="06a15-132">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice:</span></span>

* <span data-ttu-id="06a15-133">Il peut limiter la surface publique exposée des applications qu’il héberge.</span><span class="sxs-lookup"><span data-stu-id="06a15-133">It can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="06a15-134">Il fournit une couche supplémentaire de configuration et de défense.</span><span class="sxs-lookup"><span data-stu-id="06a15-134">It provides an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="06a15-135">Il peut mieux s’intégrer à l’infrastructure existante.</span><span class="sxs-lookup"><span data-stu-id="06a15-135">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="06a15-136">Il simplifie l’équilibrage de charge et la configuration de SSL.</span><span class="sxs-lookup"><span data-stu-id="06a15-136">It simplifies load balancing and SSL configuration.</span></span> <span data-ttu-id="06a15-137">Seul le serveur proxy inverse nécessite un certificat SSL : ce serveur peut donc communiquer avec vos serveurs d’applications sur le réseau interne avec du HTTP normal.</span><span class="sxs-lookup"><span data-stu-id="06a15-137">Only the reverse proxy server requires an SSL certificate, and that server can communicate with your app servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="06a15-138">Si vous n’utilisez pas un proxy inverse avec le filtrage d’hôtes activé, le [filtrage d’hôtes](#host-filtering) doit être activé.</span><span class="sxs-lookup"><span data-stu-id="06a15-138">If not using a reverse proxy with host filtering enabled, [host filtering](#host-filtering) must be enabled.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="06a15-139">Comment utiliser Kestrel dans les applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="06a15-139">How to use Kestrel in ASP.NET Core apps</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="06a15-140">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="06a15-140">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="06a15-141">Le package [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) est inclus dans le [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="06a15-141">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="06a15-142">Les modèles de projet ASP.NET Core utilisent Kestrel par défaut.</span><span class="sxs-lookup"><span data-stu-id="06a15-142">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="06a15-143">Dans *Program.cs*, le code du modèle appelle [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), qui appelle [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="06a15-143">In *Program.cs*, the template code calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="06a15-144">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="06a15-144">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="06a15-145">Installez le package NuGet [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).</span><span class="sxs-lookup"><span data-stu-id="06a15-145">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="06a15-146">Appelez la méthode d’extension [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) sur [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) dans la méthode `Main`, en spécifiant les [options Kestrel](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) dont vous avez besoin, comme indiqué dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="06a15-146">Call the [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) in the `Main` method, specifying any [Kestrel options](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) required, as shown in the next section.</span></span>

[!code-csharp[](kestrel/samples/1.x/Program.cs?name=snippet_Main&highlight=13-19)]

* * *

### <a name="kestrel-options"></a><span data-ttu-id="06a15-147">Options Kestrel</span><span class="sxs-lookup"><span data-stu-id="06a15-147">Kestrel options</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="06a15-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="06a15-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="06a15-149">Le serveur web Kestrel a des options de configuration de contrainte qui sont particulièrement utiles dans les déploiements exposés à Internet.</span><span class="sxs-lookup"><span data-stu-id="06a15-149">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="06a15-150">Vous pouvez personnaliser quelques limites importantes :</span><span class="sxs-lookup"><span data-stu-id="06a15-150">A few important limits that can be customized:</span></span>

* <span data-ttu-id="06a15-151">Nombre maximale de connexions client</span><span class="sxs-lookup"><span data-stu-id="06a15-151">Maximum client connections</span></span>
* <span data-ttu-id="06a15-152">Taille maximale du corps de la requête</span><span class="sxs-lookup"><span data-stu-id="06a15-152">Maximum request body size</span></span>
* <span data-ttu-id="06a15-153">Débit données minimal du corps de la requête</span><span class="sxs-lookup"><span data-stu-id="06a15-153">Minimum request body data rate</span></span>

<span data-ttu-id="06a15-154">Définissez ces limites et d’autres contraintes sur la propriété [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) de la classe [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="06a15-154">Set these and other constraints on the [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) property of the [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) class.</span></span> <span data-ttu-id="06a15-155">La propriété `Limits` conserve une instance de la classe [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits).</span><span class="sxs-lookup"><span data-stu-id="06a15-155">The `Limits` property holds an instance of the [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) class.</span></span>

<span data-ttu-id="06a15-156">**Nombre maximal de connexions client**</span><span class="sxs-lookup"><span data-stu-id="06a15-156">**Maximum client connections**</span></span>

[<span data-ttu-id="06a15-157">MaxConcurrentConnections</span><span class="sxs-lookup"><span data-stu-id="06a15-157">MaxConcurrentConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[<span data-ttu-id="06a15-158">MaxConcurrentUpgradedConnections</span><span class="sxs-lookup"><span data-stu-id="06a15-158">MaxConcurrentUpgradedConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

<span data-ttu-id="06a15-159">Le nombre maximal de connexions TCP ouvertes simultanées peut être défini pour l’application entière avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="06a15-159">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="06a15-160">Il existe une limite distincte pour les connexions qui ont été mises à niveau à partir de HTTP ou HTTPS vers un autre protocole (par exemple, sur une demande WebSocket).</span><span class="sxs-lookup"><span data-stu-id="06a15-160">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="06a15-161">Une fois mise à niveau, une connexion n’est pas prise en compte dans la limite `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="06a15-161">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

<span data-ttu-id="06a15-162">Le nombre maximal de connexions est illimité (null) par défaut.</span><span class="sxs-lookup"><span data-stu-id="06a15-162">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="06a15-163">**Taille maximale du corps de la demande**</span><span class="sxs-lookup"><span data-stu-id="06a15-163">**Maximum request body size**</span></span>

[<span data-ttu-id="06a15-164">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="06a15-164">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

<span data-ttu-id="06a15-165">La taille maximale par défaut du corps de la requête est de 30 000 000 octets, soit environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="06a15-165">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="06a15-166">Pour remplacer la limite dans une application ASP.NET Core MVC, nous vous recommandons d’utiliser l’attribut [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) sur une méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="06a15-166">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="06a15-167">Voici un exemple qui montre comment configurer la contrainte pour l’application sur chaque requête :</span><span class="sxs-lookup"><span data-stu-id="06a15-167">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="06a15-168">Vous pouvez remplacer le paramètre sur une demande spécifique dans l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="06a15-168">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="06a15-169">Une exception est levée vous tentez de configurer la limite sur une requête une fois que l’application a commencé à la lire.</span><span class="sxs-lookup"><span data-stu-id="06a15-169">An exception is thrown if you attempt to configure the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="06a15-170">Il existe une propriété `IsReadOnly` qui indique si la propriété `MaxRequestBodySize` est en lecture seule ; si tel est le cas, il est trop tard pour configurer la limite.</span><span class="sxs-lookup"><span data-stu-id="06a15-170">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="06a15-171">**Débit données minimal du corps de la demande**</span><span class="sxs-lookup"><span data-stu-id="06a15-171">**Minimum request body data rate**</span></span>

[<span data-ttu-id="06a15-172">MinRequestBodyDataRate</span><span class="sxs-lookup"><span data-stu-id="06a15-172">MinRequestBodyDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[<span data-ttu-id="06a15-173">MinResponseDataRate</span><span class="sxs-lookup"><span data-stu-id="06a15-173">MinResponseDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

<span data-ttu-id="06a15-174">Kestrel vérifie à chaque seconde si les données arrivent au débit spécifié en octets/seconde.</span><span class="sxs-lookup"><span data-stu-id="06a15-174">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="06a15-175">Si le débit est inférieur au minimum, la connexion expire. La période de grâce est la durée que Kestrel accorde au client pour augmenter sa vitesse de transmission jusqu’à la limite minimale ; pendant cette période, le débit n’est pas vérifié.</span><span class="sxs-lookup"><span data-stu-id="06a15-175">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="06a15-176">La période de grâce permet d’éviter la suppression des connexions qui, initialement, envoient des données à une vitesse lente en raison de la lenteur du démarrage de TCP.</span><span class="sxs-lookup"><span data-stu-id="06a15-176">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="06a15-177">Le débit minimal par défaut est 240 octets/seconde, avec une période de grâce de 5 secondes.</span><span class="sxs-lookup"><span data-stu-id="06a15-177">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="06a15-178">Un débit minimal s’applique également à la réponse.</span><span class="sxs-lookup"><span data-stu-id="06a15-178">A minimum rate also applies to the response.</span></span> <span data-ttu-id="06a15-179">Le code pour définir les limites de demande et de réponse est identique à l’exception de `RequestBody` ou `Response` dans les noms de propriété et d’interface.</span><span class="sxs-lookup"><span data-stu-id="06a15-179">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="06a15-180">Voici un exemple qui montre comment configurer les débits de données minimaux dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="06a15-180">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="06a15-181">Vous pouvez configurer les débits par demande dans l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="06a15-181">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="06a15-182">Pour plus d’informations sur les autres options et limites de Kestrel, consultez :</span><span class="sxs-lookup"><span data-stu-id="06a15-182">For information about other Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="06a15-183">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="06a15-183">KestrelServerOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [<span data-ttu-id="06a15-184">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="06a15-184">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [<span data-ttu-id="06a15-185">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="06a15-185">ListenOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="06a15-186">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="06a15-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="06a15-187">Pour plus d’informations sur les options et limites de Kestrel, consultez :</span><span class="sxs-lookup"><span data-stu-id="06a15-187">For information about Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="06a15-188">KestrelServerOptions, classe</span><span class="sxs-lookup"><span data-stu-id="06a15-188">KestrelServerOptions class</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [<span data-ttu-id="06a15-189">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="06a15-189">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

* * *

### <a name="endpoint-configuration"></a><span data-ttu-id="06a15-190">Configuration de point de terminaison</span><span class="sxs-lookup"><span data-stu-id="06a15-190">Endpoint configuration</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="06a15-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="06a15-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="06a15-192">Par défaut, ASP.NET Core est lié à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="06a15-192">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="06a15-193">Appelez les méthodes [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) ou [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) sur [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) pour configurer les préfixes d’URL et les ports pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="06a15-193">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span> <span data-ttu-id="06a15-194">`UseUrls`, l’argument de ligne de commande `--urls`, la clé de configuration d’hôte `urls` et la variable d’environnement `ASPNETCORE_URLS` fonctionnent également, mais ils présentent les limitations indiquées plus loin dans cette section.</span><span class="sxs-lookup"><span data-stu-id="06a15-194">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section.</span></span>

<span data-ttu-id="06a15-195">La clé de configuration d’hôte `urls` doit provenir de la configuration de l’hôte, et non pas de la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="06a15-195">The `urls` host configuration key must come from the host configuration, not the app configuration.</span></span> <span data-ttu-id="06a15-196">L’ajout d’une clé et d’une valeur de `urls` à *appsettings.json* n’affecte pas la configuration de l’hôte, car l’hôte est complètement initialisé au moment où la configuration est lue à partir du fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="06a15-196">Adding a `urls` key and value to *appsettings.json* doesn't affect host configuration because the host is completely initialized by the time the configuration is read from the configuration file.</span></span> <span data-ttu-id="06a15-197">Cependant, une clé `urls` dans *appsettings.json* peut être utilisée avec [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) sur le créateur d’hôte pour configurer l’hôte :</span><span class="sxs-lookup"><span data-stu-id="06a15-197">However, a `urls` key in *appsettings.json* can be used with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) on the host builder to configure the host:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appSettings.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseKestrel()
    .UseConfiguration(config)
    .UseContentRoot(Directory.GetCurrentDirectory())
    .UseStartup<Startup>()
    .Build();
```
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!INCLUDE[](~/includes/2.1.md)]

<span data-ttu-id="06a15-199">Par défaut, ASP.NET Core se lie à :</span><span class="sxs-lookup"><span data-stu-id="06a15-199">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="06a15-200">`https://localhost:5001` (quand un certificat de développement local est présent)</span><span class="sxs-lookup"><span data-stu-id="06a15-200">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="06a15-201">Un certificat de développement est créé :</span><span class="sxs-lookup"><span data-stu-id="06a15-201">A development certificate is created:</span></span>

* <span data-ttu-id="06a15-202">Quand le [SDK .NET Core](/dotnet/core/sdk) est installé.</span><span class="sxs-lookup"><span data-stu-id="06a15-202">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="06a15-203">[L’outil dev-certs](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-dev-certs) est utilisé pour créer un certificat.</span><span class="sxs-lookup"><span data-stu-id="06a15-203">The [dev-certs tool](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-dev-certs) is used to create a certificate.</span></span>

<span data-ttu-id="06a15-204">Vous devez accorder à certains navigateurs l’autorisation explicite d’approuver le certificat de développement local.</span><span class="sxs-lookup"><span data-stu-id="06a15-204">Some browsers require that you grant explicit permission to the browser to trust the local development certificate.</span></span>

<span data-ttu-id="06a15-205">Les modèles de projet ASP.NET Core 2.1 et ultérieur configurent les applications pour une exécution par défaut sur HTTPS et pour l’inclusion de [la redirection HTTPS et de la prise en charge de HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="06a15-205">ASP.NET Core 2.1 and later project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="06a15-206">Appelez les méthodes [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) ou [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) sur [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) pour configurer les préfixes d’URL et les ports pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="06a15-206">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="06a15-207">`UseUrls`, l’argument de ligne de commande `--urls`, la clé de configuration d’hôte `urls` et la variable d’environnement `ASPNETCORE_URLS` fonctionnent également, mais ils présentent les limitations indiquées plus loin dans cette section (un certificat par défaut doit être disponible pour la configuration du point de terminaison HTTPS).</span><span class="sxs-lookup"><span data-stu-id="06a15-207">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="06a15-208">Configuration de `KestrelServerOptions` pour ASP.NET Core 2.1 :</span><span class="sxs-lookup"><span data-stu-id="06a15-208">ASP.NET Core 2.1 `KestrelServerOptions` configuration:</span></span>

<span data-ttu-id="06a15-209">**ConfigureEndpointDefaults(Action<ListenOptions>)**</span><span class="sxs-lookup"><span data-stu-id="06a15-209">**ConfigureEndpointDefaults(Action<ListenOptions>)**</span></span>  
<span data-ttu-id="06a15-210">Spécifie une `Action` de configuration à exécuter pour chaque point de terminaison spécifié.</span><span class="sxs-lookup"><span data-stu-id="06a15-210">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="06a15-211">Le fait d’appeler `ConfigureEndpointDefaults` plusieurs fois remplace les `Action`s précédentes par la dernière `Action` spécifiée.</span><span class="sxs-lookup"><span data-stu-id="06a15-211">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="06a15-212">**ConfigureHttpsDefaults(Action<HttpsConnectionAdapterOptions>)**</span><span class="sxs-lookup"><span data-stu-id="06a15-212">**ConfigureHttpsDefaults(Action<HttpsConnectionAdapterOptions>)**</span></span>  
<span data-ttu-id="06a15-213">Spécifie une `Action` de configuration à exécuter pour chaque point de terminaison HTTPS.</span><span class="sxs-lookup"><span data-stu-id="06a15-213">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="06a15-214">Le fait d’appeler `ConfigureHttpsDefaults` plusieurs fois remplace les `Action`s précédentes par la dernière `Action` spécifiée.</span><span class="sxs-lookup"><span data-stu-id="06a15-214">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="06a15-215">**Configure(IConfiguration)**</span><span class="sxs-lookup"><span data-stu-id="06a15-215">**Configure(IConfiguration)**</span></span>  
<span data-ttu-id="06a15-216">Crée un chargeur de configuration pour configurer Kestrel, qui prend en entrée une [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration).</span><span class="sxs-lookup"><span data-stu-id="06a15-216">Creates a configuration loader for setting up Kestrel that takes an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) as input.</span></span> <span data-ttu-id="06a15-217">La configuration doit être limitée à la section de configuration pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="06a15-217">The configuration must be scoped to the configuration section for Kestrel.</span></span>

<span data-ttu-id="06a15-218">**ListenOptions.UseHttps**</span><span class="sxs-lookup"><span data-stu-id="06a15-218">**ListenOptions.UseHttps**</span></span>  
<span data-ttu-id="06a15-219">Configure Kestrel pour l’utilisation de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="06a15-219">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="06a15-220">Extensions de `ListenOptions.UseHttps` :</span><span class="sxs-lookup"><span data-stu-id="06a15-220">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="06a15-221">`UseHttps` &ndash; Configure Kestrel pour l’utilisation de HTTPS avec le certificat par défaut.</span><span class="sxs-lookup"><span data-stu-id="06a15-221">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="06a15-222">Lève une exception si aucun certificat par défaut n’est configuré.</span><span class="sxs-lookup"><span data-stu-id="06a15-222">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="06a15-223">Paramètres de `ListenOptions.UseHttps` :</span><span class="sxs-lookup"><span data-stu-id="06a15-223">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="06a15-224">`filename` est le chemin et le nom d’un fichier de certificat, relatif au répertoire qui contient les fichiers de contenu de l’application.</span><span class="sxs-lookup"><span data-stu-id="06a15-224">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="06a15-225">`password` est le mot de passe nécessaire pour accéder aux données du certificat X.509.</span><span class="sxs-lookup"><span data-stu-id="06a15-225">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="06a15-226">`configureOptions` est une `Action` pour configurer les `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="06a15-226">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="06a15-227">Retourne l'`ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="06a15-227">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="06a15-228">`storeName` est le magasin de certificats à partir duquel charger le certificat.</span><span class="sxs-lookup"><span data-stu-id="06a15-228">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="06a15-229">`subject` est le nom du sujet du certificat.</span><span class="sxs-lookup"><span data-stu-id="06a15-229">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="06a15-230">`allowInvalid` indique si les certificats non valides doivent être considérés, comme les certificats auto-signés.</span><span class="sxs-lookup"><span data-stu-id="06a15-230">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="06a15-231">`location` est l’emplacement du magasin à partir duquel charger le certificat.</span><span class="sxs-lookup"><span data-stu-id="06a15-231">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="06a15-232">`serverCertificate` est le certificat X.509.</span><span class="sxs-lookup"><span data-stu-id="06a15-232">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="06a15-233">En production, HTTPS doit être explicitement configuré.</span><span class="sxs-lookup"><span data-stu-id="06a15-233">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="06a15-234">Au minimum, un certificat par défaut doit être fourni.</span><span class="sxs-lookup"><span data-stu-id="06a15-234">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="06a15-235">Configurations prises en charge décrites dans la suite :</span><span class="sxs-lookup"><span data-stu-id="06a15-235">Supported configurations described next:</span></span>

* <span data-ttu-id="06a15-236">Pas de configuration</span><span class="sxs-lookup"><span data-stu-id="06a15-236">No configuration</span></span>
* <span data-ttu-id="06a15-237">Remplacer le certificat par défaut dans la configuration</span><span class="sxs-lookup"><span data-stu-id="06a15-237">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="06a15-238">Changer les valeurs par défaut dans le code</span><span class="sxs-lookup"><span data-stu-id="06a15-238">Change the defaults in code</span></span>

<span data-ttu-id="06a15-239">*Pas de configuration*</span><span class="sxs-lookup"><span data-stu-id="06a15-239">*No configuration*</span></span>

<span data-ttu-id="06a15-240">Kestrel écoute sur `http://localhost:5000` et sur `https://localhost:5001` (si un certificat par défaut est disponible).</span><span class="sxs-lookup"><span data-stu-id="06a15-240">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<span data-ttu-id="06a15-241">Spécifiez les URL avec :</span><span class="sxs-lookup"><span data-stu-id="06a15-241">Specify URLs using the:</span></span>

* <span data-ttu-id="06a15-242">La variable d’environnement `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="06a15-242">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="06a15-243">L’argument de ligne de commande `--urls`.</span><span class="sxs-lookup"><span data-stu-id="06a15-243">`--urls` command-line argument.</span></span>
* <span data-ttu-id="06a15-244">La clé de configuration d’hôte `urls`.</span><span class="sxs-lookup"><span data-stu-id="06a15-244">`urls` host configuration key.</span></span>
* <span data-ttu-id="06a15-245">La méthode d’extension `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="06a15-245">`UseUrls` extension method.</span></span>

<span data-ttu-id="06a15-246">Pour plus d’informations, consultez [URL de serveur](xref:fundamentals/hosting#server-urls) et [Remplacement de la configuration](xref:fundamentals/hosting#overriding-configuration).</span><span class="sxs-lookup"><span data-stu-id="06a15-246">For more information, see [Server URLs](xref:fundamentals/hosting#server-urls) and [Overriding configuration](xref:fundamentals/hosting#overriding-configuration).</span></span>

<span data-ttu-id="06a15-247">La valeur fournie avec ces approches peut être un ou plusieurs points de terminaison HTTP et HTTPS (HTTPS si un certificat par défaut est disponible).</span><span class="sxs-lookup"><span data-stu-id="06a15-247">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="06a15-248">Configurez la valeur sous forme de liste délimitée par des points-virgules (par exemple `"Urls": "http://localhost:8000;http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="06a15-248">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="06a15-249">*Remplacer le certificat par défaut dans la configuration*</span><span class="sxs-lookup"><span data-stu-id="06a15-249">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="06a15-250">Par défaut, [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) appelle `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` pour charger la configuration de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="06a15-250">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="06a15-251">Un schéma de configuration des paramètres d’application HTTPS par défaut est disponible pour Kestrel.</span><span class="sxs-lookup"><span data-stu-id="06a15-251">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="06a15-252">Configurez plusieurs points de terminaison, notamment les URL et les certificats à utiliser, à partir d’un fichier sur disque ou d’un magasin de certificats.</span><span class="sxs-lookup"><span data-stu-id="06a15-252">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="06a15-253">Dans l’exemple de fichier *appsettings.json* suivant :</span><span class="sxs-lookup"><span data-stu-id="06a15-253">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="06a15-254">Définissez **AllowInvalid** sur `true` pour permettre l’utilisation de certificats non valides (par exemple des certificats auto-signés).</span><span class="sxs-lookup"><span data-stu-id="06a15-254">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="06a15-255">Les points de terminaison HTTPS qui ne spécifient pas de certificat (**HttpsDefaultCert** dans l’exemple qui suit) utilise comme alternative de secours le certificat défini sous **Certificats** > **Par défaut**  ou bien le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="06a15-255">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="06a15-256">Une alternative à l’utilisation de **Chemin** et de **Mot de passe** pour un nœud de certificat consiste à spécifier le certificat avec des champs du magasin de certificats.</span><span class="sxs-lookup"><span data-stu-id="06a15-256">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="06a15-257">Par exemple, le certificat **Certificats** > **Par défaut** peut être spécifié en tant que :</span><span class="sxs-lookup"><span data-stu-id="06a15-257">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="06a15-258">Notes de schéma :</span><span class="sxs-lookup"><span data-stu-id="06a15-258">Schema notes:</span></span>

* <span data-ttu-id="06a15-259">Les noms des points de terminaison ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="06a15-259">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="06a15-260">Par exemple, `HTTPS` et `Https` sont valides.</span><span class="sxs-lookup"><span data-stu-id="06a15-260">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="06a15-261">Le paramètre `Url` est obligatoire pour chaque point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="06a15-261">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="06a15-262">Le format de ce paramètre est le même que celui du paramètre de configuration `Urls` du plus haut niveau, sauf qu’il est limité à une seule valeur.</span><span class="sxs-lookup"><span data-stu-id="06a15-262">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="06a15-263">Ces points de terminaison remplacent ceux qui sont définis dans le paramètre de configuration `Urls` du plus haut niveau configuration, au lieu de s’y ajouter.</span><span class="sxs-lookup"><span data-stu-id="06a15-263">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="06a15-264">Les points de terminaison définis dans le code via `Listen` sont cumulatifs avec les points de terminaison définis dans la section de configuration.</span><span class="sxs-lookup"><span data-stu-id="06a15-264">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="06a15-265">La section `Certificate` est facultative.</span><span class="sxs-lookup"><span data-stu-id="06a15-265">The `Certificate` section is optional.</span></span> <span data-ttu-id="06a15-266">Si la section `Certificate` n’est pas spécifiée, les valeurs par défaut définies dans les scénarios précédents sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="06a15-266">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="06a15-267">Si aucune valeur par défaut n’est disponible, le serveur lève une exception et son démarrage échoue.</span><span class="sxs-lookup"><span data-stu-id="06a15-267">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="06a15-268">La section `Certificate` prend en charge les certificats **Chemin**&ndash;**Mot de passe** et **Sujet**&ndash;**Magasin**.</span><span class="sxs-lookup"><span data-stu-id="06a15-268">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="06a15-269">Vous pouvez définir un nombre quelconque de points de terminaison de cette façon, pour autant qu’ils ne provoquent pas de conflits de port.</span><span class="sxs-lookup"><span data-stu-id="06a15-269">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="06a15-270">`serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` retourne un `KestrelConfigurationLoader` avec une méthode `.Endpoint(string name, options => { })` qui peut être utilisée pour compléter les paramètres d’un point de terminaison configuré :</span><span class="sxs-lookup"><span data-stu-id="06a15-270">`serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, options => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

  ```csharp
  serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  <span data-ttu-id="06a15-271">Vous pouvez également accéder directement à `KestrelServerOptions.ConfigurationLoader` pour continuer à boucler sur le chargeur existant, comme celui fourni par `WebHost.CreatedDeafaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="06a15-271">You can also directly access `KestrelServerOptions.ConfigurationLoader` to keep iterating on the existing loader, such as the one provided by `WebHost.CreatedDeafaultBuilder`.</span></span>

* <span data-ttu-id="06a15-272">La section de configuration pour chaque point de terminaison est disponible sur les options de la méthode `Endpoint`, ce qui permet de lire les paramètres personnalisés.</span><span class="sxs-lookup"><span data-stu-id="06a15-272">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="06a15-273">Plusieurs configurations peuvent être chargées en rappelant `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` avec une autre section.</span><span class="sxs-lookup"><span data-stu-id="06a15-273">Multiple configurations may be loaded by calling `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` again with another section.</span></span> <span data-ttu-id="06a15-274">Seule la dernière configuration est utilisée, à moins que `Load` soit explicitement appelé sur les instances précédentes.</span><span class="sxs-lookup"><span data-stu-id="06a15-274">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="06a15-275">Le métapackage n’appelle pas `Load` : sa section de configuration par défaut peut donc être remplacée.</span><span class="sxs-lookup"><span data-stu-id="06a15-275">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="06a15-276">`KestrelConfigurationLoader` reflète la famille d’API `Listen` de `KestrelServerOptions` sous forme de surcharges de `Endpoint` : le code et les points de terminaison de configuration peuvent donc être configurés au même emplacement.</span><span class="sxs-lookup"><span data-stu-id="06a15-276">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="06a15-277">Ces surcharges n’utilisent pas de noms et consomment seulement les paramètres par défaut de la configuration.</span><span class="sxs-lookup"><span data-stu-id="06a15-277">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="06a15-278">*Changer les valeurs par défaut dans le code*</span><span class="sxs-lookup"><span data-stu-id="06a15-278">*Change the defaults in code*</span></span>

<span data-ttu-id="06a15-279">`ConfigureEndpointDefaults` et `ConfigureHttpsDefaults` peuvent être utilisés pour modifier les paramètres par défaut pour `ListenOptions` et `HttpsConnectionAdapterOptions`, notamment en remplaçant le certificat par défaut spécifié dans le scénario précédent.</span><span class="sxs-lookup"><span data-stu-id="06a15-279">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="06a15-280">`ConfigureEndpointDefaults` et `ConfigureHttpsDefaults` doivent être appelés avant que des points de terminaison soient configurés.</span><span class="sxs-lookup"><span data-stu-id="06a15-280">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="06a15-281">*Prise en charge de SNI par Kestrel*</span><span class="sxs-lookup"><span data-stu-id="06a15-281">*Kestrel support for SNI*</span></span>

<span data-ttu-id="06a15-282">[L’indication du nom de serveur (SNI, Server Name Indication)](https://tools.ietf.org/html/rfc6066#section-3) peut être utilisée pour héberger plusieurs domaines sur la même adresse IP et le même port.</span><span class="sxs-lookup"><span data-stu-id="06a15-282">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="06a15-283">Pour que SNI fonctionne, le client envoie le nom d’hôte pour la session sécurisée au serveur pendant la négociation TLS afin que le serveur puisse fournir le certificat correct.</span><span class="sxs-lookup"><span data-stu-id="06a15-283">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="06a15-284">Le client utilise le certificat fourni pour la communication chiffrée avec le serveur pendant la session sécurisée qui suit la négociation TLS.</span><span class="sxs-lookup"><span data-stu-id="06a15-284">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="06a15-285">Kestrel prend en charge SNI via le rappel de `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="06a15-285">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="06a15-286">Le rappel est appelé une fois par connexion pour permettre à l’application d’inspecter le nom d’hôte et de sélectionner le certificat approprié.</span><span class="sxs-lookup"><span data-stu-id="06a15-286">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="06a15-287">La prise en charge de SNI nécessite une exécution sur le framework cible `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="06a15-287">SNI support requires running on target framework `netcoreapp2.1`.</span></span> <span data-ttu-id="06a15-288">Sur `netcoreapp2.0` et `net461`, le rappel est appelé, mais `name` est toujours `null`.</span><span class="sxs-lookup"><span data-stu-id="06a15-288">On `netcoreapp2.0` and `net461`, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="06a15-289">`name` est également `null` si le client ne fournit pas le paramètre du nom d’hôte dans la négociation TLS.</span><span class="sxs-lookup"><span data-stu-id="06a15-289">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>

```csharp
WebHost.CreateDefaultBuilder()
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
                var certs = new Dictionary(StringComparer.OrdinalIgnoreCase);
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

<span data-ttu-id="06a15-290">**Lier à un socket TCP**</span><span class="sxs-lookup"><span data-stu-id="06a15-290">**Bind to a TCP socket**</span></span>

<span data-ttu-id="06a15-291">La méthode [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) se lie à un socket TCP, et une expression lambda options permet la configuration d’un certificat SSL :</span><span class="sxs-lookup"><span data-stu-id="06a15-291">The [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) method binds to a TCP socket, and an options lambda permits SSL certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="06a15-292">Notez la façon dont cet exemple configure SSL pour un point de terminaison particulier à l’aide de [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span><span class="sxs-lookup"><span data-stu-id="06a15-292">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span></span> <span data-ttu-id="06a15-293">Utilisez la même API afin de configurer d’autres paramètres Kestrel pour des points de terminaison spécifiques.</span><span class="sxs-lookup"><span data-stu-id="06a15-293">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

<span data-ttu-id="06a15-294">**Lier à un socket Unix**</span><span class="sxs-lookup"><span data-stu-id="06a15-294">**Bind to a Unix socket**</span></span>

<span data-ttu-id="06a15-295">Écoutez sur un socket Unix avec [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) pour améliorer les performances avec Nginx, comme illustré dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="06a15-295">Listen on a Unix socket with [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="06a15-296">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="06a15-296">**Port 0**</span></span>

<span data-ttu-id="06a15-297">Si vous spécifiez le numéro de port `0`, Kestrel se lie dynamiquement à un port disponible.</span><span class="sxs-lookup"><span data-stu-id="06a15-297">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="06a15-298">L’exemple suivant montre comment déterminer le port auquel Kestrel se lie à l’exécution :</span><span class="sxs-lookup"><span data-stu-id="06a15-298">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Port0&highlight=3)]

<span data-ttu-id="06a15-299">Quand l’application est exécutée, la sortie de la fenêtre de console indique le port dynamique où l’application peut être atteinte :</span><span class="sxs-lookup"><span data-stu-id="06a15-299">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Now listening on: http://127.0.0.1:48508
```

<span data-ttu-id="06a15-300">**UseUrls, argument de ligne de commande --urls, clé de configuration d’hôte urls et limitations de la variable d’environnement ASPNETCORE_URLS**</span><span class="sxs-lookup"><span data-stu-id="06a15-300">**UseUrls, --urls command-line argument, urls host configuration key, and ASPNETCORE_URLS environment variable limitations**</span></span>

<span data-ttu-id="06a15-301">Configurez des points de terminaison avec les approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="06a15-301">Configure endpoints with the following approaches:</span></span>

* [<span data-ttu-id="06a15-302">UseUrls</span><span class="sxs-lookup"><span data-stu-id="06a15-302">UseUrls</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* <span data-ttu-id="06a15-303">Arguments de ligne de commande `--urls`</span><span class="sxs-lookup"><span data-stu-id="06a15-303">`--urls` command-line argument</span></span>
* <span data-ttu-id="06a15-304">Clé de configuration d’hôte `urls`.</span><span class="sxs-lookup"><span data-stu-id="06a15-304">`urls` host configuration key</span></span>
* <span data-ttu-id="06a15-305">Variable d’environnement `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="06a15-305">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="06a15-306">Ces méthodes sont utiles si vous voulez que votre code fonctionne avec des serveurs autres que Kestrel.</span><span class="sxs-lookup"><span data-stu-id="06a15-306">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="06a15-307">Toutefois, tenez compte des limitations suivantes :</span><span class="sxs-lookup"><span data-stu-id="06a15-307">However, be aware of these limitations:</span></span>

* <span data-ttu-id="06a15-308">SSL ne peut pas être utilisé avec ces approches, sauf si un certificat par défaut est fourni dans la configuration du point de terminaison HTTPS (par exemple avec la configuration de `KestrelServerOptions` ou un fichier de configuration, comme illustré plus haut dans cette rubrique).</span><span class="sxs-lookup"><span data-stu-id="06a15-308">SSL can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="06a15-309">Quand les deux approches `Listen` et `UseUrls` sont utilisées simultanément, les points de terminaison `Listen` remplacent les points de terminaison `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="06a15-309">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="06a15-310">**Configuration des points de terminaison IIS**</span><span class="sxs-lookup"><span data-stu-id="06a15-310">**IIS endpoint configuration**</span></span>

<span data-ttu-id="06a15-311">Quand vous utilisez IIS, les liaisons d’URL pour IIS remplacent les liaisons qui sont définies par `Listen` ou par `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="06a15-311">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="06a15-312">Pour plus d’informations, consultez la rubrique [Module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="06a15-312">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="06a15-313">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="06a15-313">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="06a15-314">Par défaut, ASP.NET Core est lié à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="06a15-314">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="06a15-315">Configurez les préfixes d’URL et les ports pour Kestrel avec :</span><span class="sxs-lookup"><span data-stu-id="06a15-315">Configure URL prefixes and ports for Kestrel using:</span></span>

* <span data-ttu-id="06a15-316">La méthode d’extension [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1)</span><span class="sxs-lookup"><span data-stu-id="06a15-316">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) extension method</span></span>
* <span data-ttu-id="06a15-317">Arguments de ligne de commande `--urls`</span><span class="sxs-lookup"><span data-stu-id="06a15-317">`--urls` command-line argument</span></span>
* <span data-ttu-id="06a15-318">La clé de configuration d’hôte `urls`</span><span class="sxs-lookup"><span data-stu-id="06a15-318">`urls` host configuration key</span></span>
* <span data-ttu-id="06a15-319">Le système de configuration d’ASP.NET Core, notamment la variable d’environnement `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="06a15-319">ASP.NET Core configuration system, including `ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="06a15-320">Pour plus d’informations sur ces méthodes, consultez [Hébergement](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="06a15-320">For more information on these methods, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="06a15-321">**Configuration des points de terminaison IIS**</span><span class="sxs-lookup"><span data-stu-id="06a15-321">**IIS endpoint configuration**</span></span>

<span data-ttu-id="06a15-322">Quand vous utilisez IIS, les liaisons d’URL pour IIS remplacent les liaisons définies par `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="06a15-322">When using IIS, the URL bindings for IIS override bindings set by `UseUrls`.</span></span> <span data-ttu-id="06a15-323">Pour plus d’informations, consultez la rubrique [Module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="06a15-323">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

* * *

### <a name="url-prefixes"></a><span data-ttu-id="06a15-324">Préfixes d’URL</span><span class="sxs-lookup"><span data-stu-id="06a15-324">URL prefixes</span></span>

<span data-ttu-id="06a15-325">Quand vous utilisez `UseUrls`, l’argument de ligne de commande `--urls`, la clé de configuration d’hôte `urls` ou la variable d’environnement `ASPNETCORE_URLS`, les préfixes d’URL peuvent être dans un des formats suivants.</span><span class="sxs-lookup"><span data-stu-id="06a15-325">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="06a15-326">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="06a15-326">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="06a15-327">Seuls les préfixes d’URL HTTP sont valides.</span><span class="sxs-lookup"><span data-stu-id="06a15-327">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="06a15-328">Kestrel ne prend pas en charge SSL lors de la configuration de liaisons d’URL avec `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="06a15-328">Kestrel doesn't support SSL when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="06a15-329">Adresse IPv4 avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="06a15-329">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="06a15-330">`0.0.0.0` est un cas spécial qui se lie à toutes les adresses IPv4.</span><span class="sxs-lookup"><span data-stu-id="06a15-330">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="06a15-331">Adresse IPv6 avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="06a15-331">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="06a15-332">`[::]` est l’équivalent IPv6 de `0.0.0.0` dans IPv4.</span><span class="sxs-lookup"><span data-stu-id="06a15-332">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>


* <span data-ttu-id="06a15-333">Nom d’hôte avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="06a15-333">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="06a15-334">Les noms d’hôte, `*` et `+` ne sont pas spéciaux.</span><span class="sxs-lookup"><span data-stu-id="06a15-334">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="06a15-335">Tout ce qui n’est pas reconnu comme adresse IP valide ou `localhost` se lie à toutes les adresses IP IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="06a15-335">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="06a15-336">Pour lier différents noms d’hôte à différentes applications ASP.NET Core sur le même port, utilisez [HTTP.sys](xref:fundamentals/servers/httpsys) ou un serveur proxy inverse, comme IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="06a15-336">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="06a15-337">Si vous n’utilisez pas un proxy inverse avec le filtrage d’hôtes activé, activez le [filtrage d’hôtes](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="06a15-337">If not using a reverse proxy with host filtering enabled, enable [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="06a15-338">Nom `localhost` de l’hôte avec numéro de port ou adresse IP de bouclage avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="06a15-338">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="06a15-339">Quand `localhost` est spécifié, Kestrel tente de se lier aux interfaces de bouclage IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="06a15-339">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="06a15-340">Si le port demandé est en cours d’utilisation par un autre service sur l’une des interfaces de bouclage, Kestrel ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="06a15-340">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="06a15-341">Si l’une des interfaces de bouclage n’est pas disponible pour une raison quelconque (généralement du fait de la non-prise en charge d’IPv6), Kestrel journalise un avertissement.</span><span class="sxs-lookup"><span data-stu-id="06a15-341">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="06a15-342">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="06a15-342">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="06a15-343">Adresse IPv4 avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="06a15-343">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="06a15-344">`0.0.0.0` est un cas spécial qui se lie à toutes les adresses IPv4.</span><span class="sxs-lookup"><span data-stu-id="06a15-344">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="06a15-345">Adresse IPv6 avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="06a15-345">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  <span data-ttu-id="06a15-346">`[::]` est l’équivalent IPv6 de `0.0.0.0` dans IPv4.</span><span class="sxs-lookup"><span data-stu-id="06a15-346">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="06a15-347">Nom d’hôte avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="06a15-347">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="06a15-348">Les noms d’hôte, `*` et `+` ne sont pas spéciaux.</span><span class="sxs-lookup"><span data-stu-id="06a15-348">Host names, `*`, and `+` aren't special.</span></span> <span data-ttu-id="06a15-349">Tout ce qui n’est pas une adresse IP reconnue ou `localhost` est lié à toutes les adresses IP IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="06a15-349">Anything that isn't a recognized IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="06a15-350">Pour lier différents noms d’hôte à différentes applications ASP.NET Core sur le même port, utilisez [WebListener](xref:fundamentals/servers/weblistener) ou un serveur proxy inverse, comme IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="06a15-350">To bind different host names to different ASP.NET Core apps on the same port, use [WebListener](xref:fundamentals/servers/weblistener) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="06a15-351">Nom `localhost` de l’hôte avec numéro de port ou adresse IP de bouclage avec numéro de port</span><span class="sxs-lookup"><span data-stu-id="06a15-351">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="06a15-352">Quand `localhost` est spécifié, Kestrel tente de se lier aux interfaces de bouclage IPv4 et IPv6.</span><span class="sxs-lookup"><span data-stu-id="06a15-352">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="06a15-353">Si le port demandé est en cours d’utilisation par un autre service sur l’une des interfaces de bouclage, Kestrel ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="06a15-353">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="06a15-354">Si l’une des interfaces de bouclage n’est pas disponible pour une raison quelconque (généralement du fait de la non-prise en charge d’IPv6), Kestrel journalise un avertissement.</span><span class="sxs-lookup"><span data-stu-id="06a15-354">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

* <span data-ttu-id="06a15-355">Socket UNIX</span><span class="sxs-lookup"><span data-stu-id="06a15-355">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock
  ```

<span data-ttu-id="06a15-356">**Port 0**</span><span class="sxs-lookup"><span data-stu-id="06a15-356">**Port 0**</span></span>

<span data-ttu-id="06a15-357">Si vous spécifiez le numéro de port `0`, Kestrel se lie dynamiquement à un port disponible.</span><span class="sxs-lookup"><span data-stu-id="06a15-357">When the port number is `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="06a15-358">La liaison au port `0` est autorisée pour n’importe quel nom d’hôte ou adresse IP, excepté pour `localhost`.</span><span class="sxs-lookup"><span data-stu-id="06a15-358">Binding to port `0` is allowed for any host name or IP except for `localhost`.</span></span>

<span data-ttu-id="06a15-359">Quand l’application est exécutée, la sortie de la fenêtre de console indique le port dynamique où l’application peut être atteinte :</span><span class="sxs-lookup"><span data-stu-id="06a15-359">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Now listening on: http://127.0.0.1:48508
```

<span data-ttu-id="06a15-360">**Préfixes d’URL pour SSL**</span><span class="sxs-lookup"><span data-stu-id="06a15-360">**URL prefixes for SSL**</span></span>

<span data-ttu-id="06a15-361">Si vous appelez la méthode d’extension `UseHttps`, veillez à inclure les préfixes d’URL avec `https:` :</span><span class="sxs-lookup"><span data-stu-id="06a15-361">If calling the `UseHttps` extension method, be sure to include URL prefixes with `https:`:</span></span>

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
> <span data-ttu-id="06a15-362">HTTPS et HTTP ne peuvent pas être hébergés sur le même port.</span><span class="sxs-lookup"><span data-stu-id="06a15-362">HTTPS and HTTP can't be hosted on the same port.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

* * *

## <a name="host-filtering"></a><span data-ttu-id="06a15-363">Filtrage d’hôtes</span><span class="sxs-lookup"><span data-stu-id="06a15-363">Host filtering</span></span>

<span data-ttu-id="06a15-364">Si Kestrel prend en charge la configuration basée sur les préfixes, comme `http://example.com:5000`, il ignore en grande partie le nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="06a15-364">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="06a15-365">L’hôte `localhost` est un cas spécial utilisé pour la liaison à des adresses de bouclage.</span><span class="sxs-lookup"><span data-stu-id="06a15-365">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="06a15-366">Tout hôte autre qu’une adresse IP explicite se lie à toutes les adresses IP publiques.</span><span class="sxs-lookup"><span data-stu-id="06a15-366">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="06a15-367">Aucune de ces informations n’est utilisée pour valider les en-têtes `Host` des requêtes.</span><span class="sxs-lookup"><span data-stu-id="06a15-367">None of this information is used to validate request `Host` headers.</span></span>

<span data-ttu-id="06a15-368">Il y a deux manières d’y remédier :</span><span class="sxs-lookup"><span data-stu-id="06a15-368">There are two workarounds:</span></span>

* <span data-ttu-id="06a15-369">Un hôte derrière un proxy inverse avec filtrage des en-têtes d’hôte.</span><span class="sxs-lookup"><span data-stu-id="06a15-369">Host behind a reverse proxy with host header filtering.</span></span> <span data-ttu-id="06a15-370">Il s’agissait du seul scénario pris en charge pour Kestrel dans ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="06a15-370">This was the only supported scenario for Kestrel in ASP.NET Core 1.x.</span></span>
* <span data-ttu-id="06a15-371">Utilisez un middleware pour filtrer les requêtes d’après l’en-tête `Host`.</span><span class="sxs-lookup"><span data-stu-id="06a15-371">Use a middleware to filter requests by the `Host` header.</span></span> <span data-ttu-id="06a15-372">Voici un exemple de middleware :</span><span class="sxs-lookup"><span data-stu-id="06a15-372">A sample middleware follows:</span></span>

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Primitives;
using Microsoft.Net.Http.Headers;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

// A normal middleware would provide an options type, config binding, extension methods, etc..
// This intentionally does all of the work inside of the middleware so it can be
// easily copy-pasted into docs and other projects.
public class HostFilteringMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IList<string> _hosts;
    private readonly ILogger<HostFilteringMiddleware> _logger;

    public HostFilteringMiddleware(RequestDelegate next, IConfiguration config, ILogger<HostFilteringMiddleware> logger)
    {
        if (config == null)
        {
            throw new ArgumentNullException(nameof(config));
        }

        _next = next ?? throw new ArgumentNullException(nameof(next));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));

        // A semicolon separated list of host names without the port numbers.
        // IPv6 addresses must use the bounding brackets and be in their normalized form.
        _hosts = config["AllowedHosts"]?.Split(new[] { ';' }, StringSplitOptions.RemoveEmptyEntries);
        if (_hosts == null || _hosts.Count == 0)
        {
            throw new InvalidOperationException("No configuration entry found for AllowedHosts.");
        }
    }

    public Task Invoke(HttpContext context)
    {
        if (!ValidateHost(context))
        {
            context.Response.StatusCode = 400;
            _logger.LogDebug("Request rejected due to incorrect Host header.");
            return Task.CompletedTask;
        }

        return _next(context);
    }

    // This does not duplicate format validations that are expected to be performed by the host.
    private bool ValidateHost(HttpContext context)
    {
        StringSegment host = context.Request.Headers[HeaderNames.Host].ToString().Trim();

        if (StringSegment.IsNullOrEmpty(host))
        {
            // Http/1.0 does not require the Host header.
            // Http/1.1 requires the header but the value may be empty.
            return true;
        }

        // Drop the port

        var colonIndex = host.LastIndexOf(':');

        // IPv6 special case
        if (host.StartsWith("[", StringComparison.Ordinal))
        {
            var endBracketIndex = host.IndexOf(']');
            if (endBracketIndex < 0)
            {
                // Invalid format
                return false;
            }
            if (colonIndex < endBracketIndex)
            {
                // No port, just the IPv6 Host
                colonIndex = -1;
            }
        }

        if (colonIndex > 0)
        {
            host = host.Subsegment(0, colonIndex);
        }

        foreach (var allowedHost in _hosts)
        {
            if (StringSegment.Equals(allowedHost, host, StringComparison.OrdinalIgnoreCase))
            {
                return true;
            }

            // Sub-domain wildcards: *.example.com
            if (allowedHost.StartsWith("*.", StringComparison.Ordinal) && host.Length >= allowedHost.Length)
            {
                // .example.com
                var allowedRoot = new StringSegment(allowedHost, 1, allowedHost.Length - 1);

                var hostRoot = host.Subsegment(host.Length - allowedRoot.Length, allowedRoot.Length);
                if (hostRoot.Equals(allowedRoot, StringComparison.OrdinalIgnoreCase))
                {
                    return true;
                }
            }
        }

        return false;
    }
}
```

<span data-ttu-id="06a15-373">Inscrivez le `HostFilteringMiddleware` précédent dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="06a15-373">Register the preceding `HostFilteringMiddleware` in `Startup.Configure`.</span></span> <span data-ttu-id="06a15-374">Notez que [l’ordre d’inscription des middlewares](xref:fundamentals/middleware/index#ordering) est important.</span><span class="sxs-lookup"><span data-stu-id="06a15-374">Note that the [ordering of middleware registration](xref:fundamentals/middleware/index#ordering) is important.</span></span> <span data-ttu-id="06a15-375">L’inscription doit se produire immédiatement après l’inscription du middleware de diagnostic (par exemple `app.UseExceptionHandler`).</span><span class="sxs-lookup"><span data-stu-id="06a15-375">Registration should occur immediately after Diagnostic Middleware registration (for example, `app.UseExceptionHandler`).</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseBrowserLink();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseMiddleware<HostFilteringMiddleware>();

    app.UseMvcWithDefaultRoute();
}
```

<span data-ttu-id="06a15-376">Le middleware précédent attend une clé `AllowedHosts` dans *appsettings.\<nom_environnement>.json*.</span><span class="sxs-lookup"><span data-stu-id="06a15-376">The preceding middleware expects an `AllowedHosts` key in *appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="06a15-377">La valeur de cette clé est une liste délimitée par des points-virgules des noms d’hôte sans les numéros de port.</span><span class="sxs-lookup"><span data-stu-id="06a15-377">This key's value is a semicolon-delimited list of host names without the port numbers.</span></span> <span data-ttu-id="06a15-378">Ajoutez la paire clé-valeur `AllowedHosts` dans *appsettings.Production.json* :</span><span class="sxs-lookup"><span data-stu-id="06a15-378">Include the `AllowedHosts` key-value pair in *appsettings.Production.json*:</span></span>

```json
{
  "AllowedHosts": "example.com"
}
```

<span data-ttu-id="06a15-379">*appsettings.Development.json* (fichier de configuration de localhost) :</span><span class="sxs-lookup"><span data-stu-id="06a15-379">*appsettings.Development.json* (localhost configuration file):</span></span>

```json
{
  "AllowedHosts": "localhost"
}
```

## <a name="additional-resources"></a><span data-ttu-id="06a15-380">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="06a15-380">Additional resources</span></span>

* [<span data-ttu-id="06a15-381">Appliquer HTTPS</span><span class="sxs-lookup"><span data-stu-id="06a15-381">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="06a15-382">Code source Kestrel</span><span class="sxs-lookup"><span data-stu-id="06a15-382">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)
