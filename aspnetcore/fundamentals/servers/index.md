---
title: Implémentations de serveurs web dans ASP.NET Core
author: guardrex
description: Découvrez les serveurs web Kestrel et HTTP.sys pour ASP.NET Core. Découvrez comment choisir un serveur et quand utiliser un serveur proxy inverse.
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/01/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 965d69dd071ec71d283284d58e6e1a6e78604f90
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861354"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="ad31f-104">Implémentations de serveurs web dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad31f-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="ad31f-105">De [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) et [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="ad31f-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="ad31f-106">Une application ASP.NET Core s’exécute avec une implémentation de serveur HTTP in-process.</span><span class="sxs-lookup"><span data-stu-id="ad31f-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="ad31f-107">L’implémentation du serveur écoute les requêtes HTTP et les expose à l’application sous forme d’ensembles de [fonctionnalités de requêtes](xref:fundamentals/request-features) composées dans un <xref:Microsoft.AspNetCore.Http.HttpContext>.</span><span class="sxs-lookup"><span data-stu-id="ad31f-107">The server implementation listens for HTTP requests and surfaces them to the app as sets of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="ad31f-108">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="ad31f-108">Windows</span></span>](#tab/windows)

<span data-ttu-id="ad31f-109">ASP.NET Core est fourni avec les composants suivants :</span><span class="sxs-lookup"><span data-stu-id="ad31f-109">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="ad31f-110">[Serveur Kestrel](xref:fundamentals/servers/kestrel) : serveur HTTP multiplateforme par défaut.</span><span class="sxs-lookup"><span data-stu-id="ad31f-110">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="ad31f-111">Serveur HTTP IIS (`IISHttpServer`) : implémentation d’un [serveur in-process IIS](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) utilisée avec le [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="ad31f-111">IIS HTTP Server (`IISHttpServer`) is an [IIS in-process server](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) implementation used with the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>
* <span data-ttu-id="ad31f-112">[Serveur HTTP.sys](xref:fundamentals/servers/httpsys) : serveur HTTP exclusivement Windows basé sur le [pilote de noyau HTTP.Sys et l’API Serveur HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="ad31f-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="ad31f-113">HTTP.sys est appelé [WebListener](xref:fundamentals/servers/weblistener) dans ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="ad31f-113">HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="ad31f-114">macOS</span><span class="sxs-lookup"><span data-stu-id="ad31f-114">macOS</span></span>](#tab/macos)

<span data-ttu-id="ad31f-115">ASP.NET Core est fourni avec le [serveur Kestrel](xref:fundamentals/servers/kestrel) qui est le serveur HTTP multiplateforme par défaut.</span><span class="sxs-lookup"><span data-stu-id="ad31f-115">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="ad31f-116">Linux</span><span class="sxs-lookup"><span data-stu-id="ad31f-116">Linux</span></span>](#tab/linux)

<span data-ttu-id="ad31f-117">ASP.NET Core est fourni avec le [serveur Kestrel](xref:fundamentals/servers/kestrel) qui est le serveur HTTP multiplateforme par défaut.</span><span class="sxs-lookup"><span data-stu-id="ad31f-117">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="ad31f-118">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="ad31f-118">Windows</span></span>](#tab/windows)

<span data-ttu-id="ad31f-119">ASP.NET Core est fourni avec les composants suivants :</span><span class="sxs-lookup"><span data-stu-id="ad31f-119">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="ad31f-120">[Serveur Kestrel](xref:fundamentals/servers/kestrel) : serveur HTTP multiplateforme par défaut.</span><span class="sxs-lookup"><span data-stu-id="ad31f-120">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="ad31f-121">[Serveur HTTP.sys](xref:fundamentals/servers/httpsys) : serveur HTTP exclusivement Windows basé sur le [pilote de noyau HTTP.Sys et l’API Serveur HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="ad31f-121">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="ad31f-122">HTTP.sys est appelé [WebListener](xref:fundamentals/servers/weblistener) dans ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="ad31f-122">HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="ad31f-123">macOS</span><span class="sxs-lookup"><span data-stu-id="ad31f-123">macOS</span></span>](#tab/macos)

<span data-ttu-id="ad31f-124">ASP.NET Core est fourni avec le [serveur Kestrel](xref:fundamentals/servers/kestrel) qui est le serveur HTTP multiplateforme par défaut.</span><span class="sxs-lookup"><span data-stu-id="ad31f-124">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="ad31f-125">Linux</span><span class="sxs-lookup"><span data-stu-id="ad31f-125">Linux</span></span>](#tab/linux)

<span data-ttu-id="ad31f-126">ASP.NET Core est fourni avec le [serveur Kestrel](xref:fundamentals/servers/kestrel) qui est le serveur HTTP multiplateforme par défaut.</span><span class="sxs-lookup"><span data-stu-id="ad31f-126">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="ad31f-127">Kestrel</span><span class="sxs-lookup"><span data-stu-id="ad31f-127">Kestrel</span></span>

<span data-ttu-id="ad31f-128">Kestrel est le serveur web par défaut inclus dans les modèles de projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ad31f-128">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ad31f-129">Kestrel peut être utilisé :</span><span class="sxs-lookup"><span data-stu-id="ad31f-129">Kestrel can be used:</span></span>

* <span data-ttu-id="ad31f-130">Par lui même, c’est-à-dire en tant que serveur de périphérie traitant les requêtes en provenance directe d’un réseau, notamment d’Internet.</span><span class="sxs-lookup"><span data-stu-id="ad31f-130">By itself as an edge server processing requests directly from a network, including the Internet.</span></span>
* <span data-ttu-id="ad31f-131">Avec un *serveur proxy inverse*, comme [IIS (Internet Information Services)](https://www.iis.net/), [Nginx](http://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="ad31f-131">With a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="ad31f-132">Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ad31f-132">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![Kestrel communique directement avec Internet sans serveur proxy inverse](kestrel/_static/kestrel-to-internet2.png)

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="ad31f-135">Les deux configurations&mdash;avec ou sans serveur proxy inverse&mdash;sont des configurations d’hébergement valides et prises en charge pour les applications ASP.NET Core versions 2.0 ou ultérieures.</span><span class="sxs-lookup"><span data-stu-id="ad31f-135">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="ad31f-136">Pour plus d’informations, consultez [Quand utiliser Kestrel avec un proxy inverse ?](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="ad31f-136">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ad31f-137">Si l’application accepte seulement les requêtes provenant d’un réseau interne, Kestrel peut être utilisé par lui-même.</span><span class="sxs-lookup"><span data-stu-id="ad31f-137">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![Kestrel communique directement avec le réseau interne](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="ad31f-139">Si l’application est exposée à Internet, Kestrel doit utiliser un *serveur proxy inverse* comme[IIS (Internet Information Services)](https://www.iis.net/), [Nginx](http://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="ad31f-139">If the app is exposed to the Internet, Kestrel must use a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="ad31f-140">Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ad31f-140">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="ad31f-142">La raison la plus importante de l’utilisation d’un proxy inversé pour les déploiements de serveurs de périphérie publics directement exposés à Internet est la sécurité.</span><span class="sxs-lookup"><span data-stu-id="ad31f-142">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="ad31f-143">Les versions 1.x de Kestrel n’incluent pas de fonctionnalités de sécurité suffisantes pour vous défendre contre les attaques provenant d’Internet.</span><span class="sxs-lookup"><span data-stu-id="ad31f-143">The 1.x versions of Kestrel don't include important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="ad31f-144">Ceci inclut, entre autres choses, des délais d’attente appropriés, des limites de taille des requêtes et des limites du nombre de connexions simultanées.</span><span class="sxs-lookup"><span data-stu-id="ad31f-144">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="ad31f-145">Pour plus d’informations, consultez [Quand utiliser Kestrel avec un proxy inverse ?](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="ad31f-145">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

### <a name="iis-with-kestrel"></a><span data-ttu-id="ad31f-146">IIS avec Kestrel</span><span class="sxs-lookup"><span data-stu-id="ad31f-146">IIS with Kestrel</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ad31f-147">Lorsque vous utilisez [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) ou [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), l'application ASP.NET Core s'exécute soit dans le même processus que le processus Worker IIS (modèle d'hébergement *in-process*), soit dans un processus distinct du processus Worker IIS (modèle *out-of-process*).</span><span class="sxs-lookup"><span data-stu-id="ad31f-147">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the ASP.NET Core app either runs in the same process as the IIS worker process (the *in-process* hosting model) or in a process separate from the IIS worker process (the *out-of-process* hosting model).</span></span>

<span data-ttu-id="ad31f-148">Le [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) est un module IIS natif qui gère les requêtes IIS natives entre le serveur HTTP IIS in-process ou le serveur Kestrel out-of-process.</span><span class="sxs-lookup"><span data-stu-id="ad31f-148">The [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) is a native IIS module that handles native IIS requests between either the in-process IIS HTTP Server or the out-of-process Kestrel server.</span></span> <span data-ttu-id="ad31f-149">Pour plus d'informations, consultez <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="ad31f-149">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ad31f-150">Quand vous utilisez [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) ou [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) comme proxy inverse pour ASP.NET Core, l’application ASP.NET Core s’exécute dans un processus distinct du processus de travail IIS.</span><span class="sxs-lookup"><span data-stu-id="ad31f-150">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) as a reverse proxy for ASP.NET Core, the ASP.NET Core app runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="ad31f-151">Dans le processus IIS, le [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) coordonne la relation du proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="ad31f-151">In the IIS process, the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) coordinates the reverse proxy relationship.</span></span> <span data-ttu-id="ad31f-152">Les principales fonctions du module ASP.NET Core consistent à démarrer l’application, à la redémarrer quand elle plante et à lui transférer le trafic HTTP.</span><span class="sxs-lookup"><span data-stu-id="ad31f-152">The primary functions of the ASP.NET Core Module are to start the app, restart the app when it crashes, and forward HTTP traffic to the app.</span></span> <span data-ttu-id="ad31f-153">Pour plus d'informations, consultez <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="ad31f-153">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

### <a name="nginx-with-kestrel"></a><span data-ttu-id="ad31f-154">Nginx avec Kestrel</span><span class="sxs-lookup"><span data-stu-id="ad31f-154">Nginx with Kestrel</span></span>

<span data-ttu-id="ad31f-155">Pour plus d’informations sur l’utilisation de Nginx sur Linux comme serveur proxy inverse pour Kestrel, consultez <xref:host-and-deploy/linux-nginx>.</span><span class="sxs-lookup"><span data-stu-id="ad31f-155">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-nginx>.</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="ad31f-156">Apache avec Kestrel</span><span class="sxs-lookup"><span data-stu-id="ad31f-156">Apache with Kestrel</span></span>

<span data-ttu-id="ad31f-157">Pour plus d’informations sur l’utilisation d’Apache sur Linux comme serveur proxy inverse pour Kestrel, consultez <xref:host-and-deploy/linux-apache>.</span><span class="sxs-lookup"><span data-stu-id="ad31f-157">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-apache>.</span></span>

## <a name="httpsys"></a><span data-ttu-id="ad31f-158">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ad31f-158">HTTP.sys</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ad31f-159">Si vos applications ASP.NET Core sont exécutées sur Windows, HTTP.sys est une alternative à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ad31f-159">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="ad31f-160">Kestrel est généralement recommandé pour de meilleures performances.</span><span class="sxs-lookup"><span data-stu-id="ad31f-160">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="ad31f-161">HTTP.sys peut être utilisé dans les scénarios où l’application est exposée à Internet et où des fonctionnalités requises sont prises en charge par HTTP.sys, mais pas par Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ad31f-161">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="ad31f-162">Pour plus d'informations, consultez <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="ad31f-162">For more information, see <xref:fundamentals/servers/httpsys>.</span></span>

![HTTP.sys communique directement avec Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="ad31f-164">HTTP.sys peut également être utilisé pour les applications qui sont exposées seulement à un réseau interne.</span><span class="sxs-lookup"><span data-stu-id="ad31f-164">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![HTTP.sys communique directement avec le réseau interne](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ad31f-166">HTTP.sys est nommé [WebListener](xref:fundamentals/servers/weblistener) dans ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="ad31f-166">HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span> <span data-ttu-id="ad31f-167">Si les applications ASP.NET Core sont exécutées sur Windows, WebListener est une alternative pour les scénarios où IIS n’est pas disponible pour héberger les applications.</span><span class="sxs-lookup"><span data-stu-id="ad31f-167">If ASP.NET Core apps are run on Windows, WebListener is an alternative for scenarios where IIS isn't available to host apps.</span></span>

![WebListener communique directement avec Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="ad31f-169">WebListener peut également être utilisé à la place de Kestrel pour les applications exposées à un réseau interne, si des fonctionnalités requises sont prises en charge par WebListener, mais pas par Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ad31f-169">WebListener can also be used in place of Kestrel for apps that are only exposed to an internal network, if required capabilities are supported by WebListener but not Kestrel.</span></span> <span data-ttu-id="ad31f-170">Pour plus d’informations sur WebListener, consultez [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="ad31f-170">For information on WebListener, see [WebListener](xref:fundamentals/servers/weblistener).</span></span>

![WebListener communique directement avec le réseau interne](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="ad31f-172">Infrastructure serveur d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad31f-172">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="ad31f-173">Le [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) disponible dans la méthode `Startup.Configure` expose la propriété [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) de type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="ad31f-173">The [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup.Configure` method exposes the [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) property of type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="ad31f-174">Kestrel et HTTP.sys (WebListener dans ASP.NET Core 1.x) n’exposent chacun qu’une seule fonctionnalité, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), mais des implémentations serveur différentes peuvent exposer des fonctionnalités supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="ad31f-174">Kestrel and HTTP.sys (WebListener in ASP.NET Core 1.x) only expose a single feature each, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="ad31f-175">`IServerAddressesFeature` permet de déterminer quel est le port lié à l’implémentation serveur au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="ad31f-175">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="ad31f-176">Serveurs personnalisés</span><span class="sxs-lookup"><span data-stu-id="ad31f-176">Custom servers</span></span>

<span data-ttu-id="ad31f-177">Si les serveurs intégrés ne répondent pas aux spécifications de l’application, vous pouvez créer une implémentation serveur personnalisée.</span><span class="sxs-lookup"><span data-stu-id="ad31f-177">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="ad31f-178">Le [guide OWIN (Open Web Interface pour .NET)](xref:fundamentals/owin) montre comment écrire une implémentation [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) basée sur [Nowin](https://github.com/Bobris/Nowin).</span><span class="sxs-lookup"><span data-stu-id="ad31f-178">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="ad31f-179">Seules les interfaces des fonctionnalités utilisées par l’application nécessitent une implémentation, bien qu’au minimum, [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) et [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) doivent être pris en charge.</span><span class="sxs-lookup"><span data-stu-id="ad31f-179">Only the feature interfaces that the app uses require implementation, though at a minimum [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="ad31f-180">Démarrage du serveur</span><span class="sxs-lookup"><span data-stu-id="ad31f-180">Server startup</span></span>

<span data-ttu-id="ad31f-181">Quand vous utilisez [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio pour Mac](https://www.visualstudio.com/vs/mac/) ou [Visual Studio Code](https://code.visualstudio.com/), le serveur est lancé quand l’application est démarrée par l’IDE.</span><span class="sxs-lookup"><span data-stu-id="ad31f-181">When using [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio for Mac](https://www.visualstudio.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/), the server is launched when the app is started by the Integrated Development Environment (IDE).</span></span> <span data-ttu-id="ad31f-182">Dans Visual Studio sur Windows, des profils de lancement peuvent être utilisés pour démarrer l’application et le serveur avec [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[Module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) ou la console.</span><span class="sxs-lookup"><span data-stu-id="ad31f-182">In Visual Studio on Windows, launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) or the console.</span></span> <span data-ttu-id="ad31f-183">Dans Visual Studio Code, l’application et le serveur sont démarrés par [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), qui active le débogueur CoreCLR.</span><span class="sxs-lookup"><span data-stu-id="ad31f-183">In Visual Studio Code, the app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span> <span data-ttu-id="ad31f-184">Avec Visual Studio pour Mac, l’application et le serveur sont démarrés par le [débogueur Mono Soft-Mode](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span><span class="sxs-lookup"><span data-stu-id="ad31f-184">Using Visual Studio for Mac, the app and server are started by the [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="ad31f-185">Lors du lancement d’une application à partir d’une invite de commandes dans le dossier du projet, [dotnet run](/dotnet/core/tools/dotnet-run) lance l’application et le serveur (Kestrel et HTTP.sys uniquement).</span><span class="sxs-lookup"><span data-stu-id="ad31f-185">When launching an app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="ad31f-186">La configuration est spécifiée par l’option `-c|--configuration`, qui est définie sur `Debug` (par défaut) ou sur `Release`.</span><span class="sxs-lookup"><span data-stu-id="ad31f-186">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="ad31f-187">Si les profils de lancement sont présents dans un fichier *launchSettings.json*, utilisez l’option `--launch-profile <NAME>` pour définir le profil de lancement (par exemple `Development` ou `Production`).</span><span class="sxs-lookup"><span data-stu-id="ad31f-187">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="ad31f-188">Pour plus d’informations, consultez les rubriques [dotnet run](/dotnet/core/tools/dotnet-run) et [Empaquetage de la distribution de .NET Core](/dotnet/core/build/distribution-packaging).</span><span class="sxs-lookup"><span data-stu-id="ad31f-188">For more information, see the [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging) topics.</span></span>

## <a name="http2-support"></a><span data-ttu-id="ad31f-189">Prise en charge de HTTP/2</span><span class="sxs-lookup"><span data-stu-id="ad31f-189">HTTP/2 support</span></span>

<span data-ttu-id="ad31f-190">[HTTP/2](https://httpwg.org/specs/rfc7540.html) est pris en charge avec ASP.NET Core dans les scénarios de déploiement suivants :</span><span class="sxs-lookup"><span data-stu-id="ad31f-190">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="ad31f-191">Kestrel</span><span class="sxs-lookup"><span data-stu-id="ad31f-191">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="ad31f-192">Système d'exploitation</span><span class="sxs-lookup"><span data-stu-id="ad31f-192">Operating system</span></span>
    * <span data-ttu-id="ad31f-193">Windows Server 2016/Windows 10 ou version ultérieure&dagger;</span><span class="sxs-lookup"><span data-stu-id="ad31f-193">Windows Server 2016/Windows 10 or later&dagger;</span></span>
    * <span data-ttu-id="ad31f-194">Linux avec OpenSSL 1.0.2 ou version ultérieure (par exemple, Ubuntu 16.04 ou version ultérieure)</span><span class="sxs-lookup"><span data-stu-id="ad31f-194">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="ad31f-195">HTTP/2 sera pris en charge sur macOS dans une prochaine version.</span><span class="sxs-lookup"><span data-stu-id="ad31f-195">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="ad31f-196">Version cible de .Net Framework : .NET Core 2.2 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="ad31f-196">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="ad31f-197">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ad31f-197">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="ad31f-198">Windows Server 2016/Windows 10 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="ad31f-198">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="ad31f-199">Version cible de .Net Framework : non applicable aux déploiements HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="ad31f-199">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="ad31f-200">IIS (in-process)</span><span class="sxs-lookup"><span data-stu-id="ad31f-200">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="ad31f-201">Windows Server 2016/Windows 10 ou version ultérieure ; IIS 10 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="ad31f-201">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="ad31f-202">Version cible de .Net Framework : .NET Core 2.2 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="ad31f-202">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="ad31f-203">IIS (out-of-process)</span><span class="sxs-lookup"><span data-stu-id="ad31f-203">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="ad31f-204">Windows Server 2016/Windows 10 ou version ultérieure ; IIS 10 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="ad31f-204">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="ad31f-205">Les connexions au serveur périphérique public utilisent HTTP/2, mais la connexion de proxy inverse à Kestrel utilise HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="ad31f-205">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="ad31f-206">Version cible de .Net Framework : non applicable aux déploiements IIS out-of-process.</span><span class="sxs-lookup"><span data-stu-id="ad31f-206">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

<span data-ttu-id="ad31f-207">&dagger;Kestrel propose une prise en charge limitée de HTTP/2 sous Windows Server 2012 R2 et Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="ad31f-207">&dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="ad31f-208">La prise en charge est limitée car la liste des suites de chiffrement TLS prises en charge sur ces systèmes d’exploitation est limitée.</span><span class="sxs-lookup"><span data-stu-id="ad31f-208">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="ad31f-209">Un certificat généré à l’aide d’Elliptic Curve Digital Signature algorithme (ECDSA) peut être requis pour sécuriser les connexions TLS.</span><span class="sxs-lookup"><span data-stu-id="ad31f-209">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="ad31f-210">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ad31f-210">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="ad31f-211">Windows Server 2016/Windows 10 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="ad31f-211">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="ad31f-212">Version cible de .Net Framework : non applicable aux déploiements HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="ad31f-212">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="ad31f-213">IIS (out-of-process)</span><span class="sxs-lookup"><span data-stu-id="ad31f-213">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="ad31f-214">Windows Server 2016/Windows 10 ou version ultérieure ; IIS 10 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="ad31f-214">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="ad31f-215">Les connexions au serveur périphérique public utilisent HTTP/2, mais la connexion de proxy inverse à Kestrel utilise HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="ad31f-215">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="ad31f-216">Version cible de .Net Framework : non applicable aux déploiements IIS out-of-process.</span><span class="sxs-lookup"><span data-stu-id="ad31f-216">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="ad31f-217">Une connexion HTTP/2 doit utiliser la [négociation du protocole de la couche d’application (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) et TLS 1.2 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="ad31f-217">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="ad31f-218">Pour plus d’informations, consultez les rubriques qui se rapportent à vos scénarios de déploiement de serveur.</span><span class="sxs-lookup"><span data-stu-id="ad31f-218">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ad31f-219">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ad31f-219">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <span data-ttu-id="ad31f-220"><xref:fundamentals/servers/httpsys> (pour ASP.NET Core 1.x, consultez<xref:fundamentals/servers/weblistener>)</span><span class="sxs-lookup"><span data-stu-id="ad31f-220"><xref:fundamentals/servers/httpsys> (for ASP.NET Core 1.x, see <xref:fundamentals/servers/weblistener>)</span></span>
