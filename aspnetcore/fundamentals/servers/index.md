---
title: Implémentations de serveurs web dans ASP.NET Core
author: rick-anderson
description: Découvrez les serveurs web Kestrel et HTTP.sys pour ASP.NET Core. Découvrez comment choisir un serveur et quand utiliser un serveur proxy inverse.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 161ab3fdf48e58d8c9af991dc5531e46d9c5adff
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325859"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="3c4e0-104">Implémentations de serveurs web dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3c4e0-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="3c4e0-105">De [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) et [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="3c4e0-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="3c4e0-106">Une application ASP.NET Core s’exécute avec une implémentation de serveur HTTP in-process.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="3c4e0-107">L’implémentation du serveur écoute les requêtes HTTP et les expose à l’application sous forme d’ensembles de [fonctionnalités de requêtes](xref:fundamentals/request-features) composées dans un <xref:Microsoft.AspNetCore.Http.HttpContext>.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-107">The server implementation listens for HTTP requests and surfaces them to the app as sets of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

<span data-ttu-id="3c4e0-108">ASP.NET Core est livré avec trois implémentations de serveurs :</span><span class="sxs-lookup"><span data-stu-id="3c4e0-108">ASP.NET Core ships three server implementations:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="3c4e0-109">[Kestrel](xref:fundamentals/servers/kestrel) est le serveur HTTP multiplateforme par défaut pour ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-109">[Kestrel](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server for ASP.NET Core.</span></span>
* <span data-ttu-id="3c4e0-110">`IISHttpServer` est utilisé avec le [modèle d’hébergement in-process](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) et le [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) sur Windows.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-110">`IISHttpServer` is used with the [in-process hosting model](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) on Windows.</span></span>
* <span data-ttu-id="3c4e0-111">[HTTP.sys](xref:fundamentals/servers/httpsys) est un serveur HTTP exclusivement Windows basé sur le [pilote de noyau HTTP.Sys et l’API Serveur HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="3c4e0-111">[HTTP.sys](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="3c4e0-112">(HTTP.sys est appelé [WebListener](xref:fundamentals/servers/weblistener) dans ASP.NET Core 1.x.)</span><span class="sxs-lookup"><span data-stu-id="3c4e0-112">(HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="3c4e0-113">[Kestrel](xref:fundamentals/servers/kestrel) est le serveur HTTP multiplateforme par défaut pour ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-113">[Kestrel](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server for ASP.NET Core.</span></span>
* <span data-ttu-id="3c4e0-114">[HTTP.sys](xref:fundamentals/servers/httpsys) est un serveur HTTP exclusivement Windows basé sur le [pilote de noyau HTTP.Sys et l’API Serveur HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="3c4e0-114">[HTTP.sys](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="3c4e0-115">(HTTP.sys est appelé [WebListener](xref:fundamentals/servers/weblistener) dans ASP.NET Core 1.x.)</span><span class="sxs-lookup"><span data-stu-id="3c4e0-115">(HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span></span>

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="3c4e0-116">Kestrel</span><span class="sxs-lookup"><span data-stu-id="3c4e0-116">Kestrel</span></span>

<span data-ttu-id="3c4e0-117">Kestrel est le serveur web par défaut inclus dans les modèles de projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-117">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3c4e0-118">Vous pouvez utiliser Kestrel par lui-même ou l’associer à un *serveur proxy inverse*, comme IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-118">Kestrel can be used by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="3c4e0-119">Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel après un traitement préliminaire.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel communique directement avec Internet sans serveur proxy inverse](kestrel/_static/kestrel-to-internet2.png)

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="3c4e0-122">Les deux configurations&mdash;avec ou sans serveur proxy inverse&mdash;sont des configurations d’hébergement valides et prises en charge pour les applications ASP.NET Core versions 2.0 ou ultérieures.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-122">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="3c4e0-123">Pour plus d’informations, consultez [Quand utiliser Kestrel avec un proxy inverse ?](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="3c4e0-123">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3c4e0-124">Si l’application accepte seulement les requêtes provenant d’un réseau interne, Kestrel peut être utilisé par lui-même.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-124">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![Kestrel communique directement avec le réseau interne](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="3c4e0-126">Si l’application est exposée à Internet, Kestrel doit utiliser IIS, Nginx ou Apache comme *serveur proxy inverse*.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-126">If the app is exposed to the Internet, Kestrel must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="3c4e0-127">Un serveur proxy inverse reçoit les requêtes HTTP provenant d’Internet et les transfère à Kestrel après un traitement préliminaire, comme illustré dans le diagramme suivant :</span><span class="sxs-lookup"><span data-stu-id="3c4e0-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram:</span></span>

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="3c4e0-129">La raison la plus importante de l’utilisation d’un proxy inversé pour les déploiements de serveurs de périphérie publics directement exposés à Internet est la sécurité.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-129">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="3c4e0-130">Les versions 1.x de Kestrel n’ont pas de fonctionnalités de sécurité suffisantes pour vous défendre contre les attaques provenant d’Internet.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-130">The 1.x versions of Kestrel don't have important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="3c4e0-131">Ceci inclut, entre autres choses, des délais d’attente appropriés, des limites de taille des requêtes et des limites du nombre de connexions simultanées.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-131">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="3c4e0-132">Pour plus d’informations, consultez [Quand utiliser Kestrel avec un proxy inverse ?](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="3c4e0-132">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

<span data-ttu-id="3c4e0-133">Vous ne pouvez pas utiliser IIS, Nginx ou Apache sans Kestrel ou sans une [implémentation de serveur personnalisée](#custom-servers).</span><span class="sxs-lookup"><span data-stu-id="3c4e0-133">IIS, Nginx, and Apache can't be used without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="3c4e0-134">ASP.NET Core a été conçu pour s’exécuter dans son propre processus et se comporter de manière cohérente entre les plateformes.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-134">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="3c4e0-135">IIS, Nginx et Apache imposent leur propre procédure de démarrage et leur environnement.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-135">IIS, Nginx, and Apache dictate their own startup procedure and environment.</span></span> <span data-ttu-id="3c4e0-136">Pour utiliser ces technologies serveur directement, ASP.NET Core doit s’adapter aux spécifications de chaque serveur.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-136">To use these server technologies directly, ASP.NET Core would need to adapt to the requirements of each server.</span></span> <span data-ttu-id="3c4e0-137">Avec une implémentation de serveur web comme Kestrel, ASP.NET Core peut contrôler le processus de démarrage et l’environnement quand il est hébergé sur des technologies serveur différentes.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-137">Using a web server implementation, such as Kestrel, ASP.NET Core has control over the startup process and environment when hosted on different server technologies.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="3c4e0-138">IIS avec Kestrel</span><span class="sxs-lookup"><span data-stu-id="3c4e0-138">IIS with Kestrel</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="3c4e0-139">Lorsque vous utilisez [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) ou [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), l'application ASP.NET Core s'exécute soit dans le même processus que le processus Worker IIS (modèle d'hébergement *in-process*), soit dans un processus distinct du processus Worker IIS (modèle *out-of-process*).</span><span class="sxs-lookup"><span data-stu-id="3c4e0-139">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the ASP.NET Core app either runs in the same process as the IIS worker process (the *in-process* hosting model) or in a process separate from the IIS worker process (the *out-of-process* hosting model).</span></span>

<span data-ttu-id="3c4e0-140">Le [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) est un module IIS natif qui gère les requêtes IIS natives entre le serveur Http IIS in-process ou le serveur Kestrel out-of-process.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-140">The [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) is a native IIS module that handles native IIS requests between either the in-process IIS Http Server or the out-of-process Kestrel server.</span></span> <span data-ttu-id="3c4e0-141">Pour plus d'informations, consultez <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-141">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="3c4e0-142">Quand vous utilisez [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) ou [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) comme proxy inverse pour ASP.NET Core, l’application ASP.NET Core s’exécute dans un processus distinct du processus de travail IIS.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-142">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) as a reverse proxy for ASP.NET Core, the ASP.NET Core app runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="3c4e0-143">Dans le processus IIS, le [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) coordonne la relation du proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-143">In the IIS process, the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) coordinates the reverse proxy relationship.</span></span> <span data-ttu-id="3c4e0-144">Les principales fonctions du module ASP.NET Core consistent à démarrer l’application ASP.NET Core, à la redémarrer quand elle se bloque et à lui transférer le trafic HTTP.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-144">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core app, restart the app when it crashes, and forward HTTP traffic to the app.</span></span> <span data-ttu-id="3c4e0-145">Pour plus d'informations, consultez <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-145">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

### <a name="nginx-with-kestrel"></a><span data-ttu-id="3c4e0-146">Nginx avec Kestrel</span><span class="sxs-lookup"><span data-stu-id="3c4e0-146">Nginx with Kestrel</span></span>

<span data-ttu-id="3c4e0-147">Pour plus d’informations sur l’utilisation de Nginx sur Linux comme serveur proxy inverse pour Kestrel, consultez [Héberger sur Linux avec Nginx](xref:host-and-deploy/linux-nginx).</span><span class="sxs-lookup"><span data-stu-id="3c4e0-147">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="3c4e0-148">Apache avec Kestrel</span><span class="sxs-lookup"><span data-stu-id="3c4e0-148">Apache with Kestrel</span></span>

<span data-ttu-id="3c4e0-149">Pour plus d’informations sur l’utilisation d’Apache sur Linux comme serveur proxy inverse pour Kestrel, consultez [Héberger sur Linux avec Apache](xref:host-and-deploy/linux-apache).</span><span class="sxs-lookup"><span data-stu-id="3c4e0-149">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Apache](xref:host-and-deploy/linux-apache).</span></span>

## <a name="httpsys"></a><span data-ttu-id="3c4e0-150">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="3c4e0-150">HTTP.sys</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3c4e0-151">Si vos applications ASP.NET Core sont exécutées sur Windows, HTTP.sys est une alternative à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-151">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="3c4e0-152">Kestrel est généralement recommandé pour de meilleures performances.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-152">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="3c4e0-153">HTTP.sys peut être utilisé dans les scénarios où l’application est exposée à Internet et où des fonctionnalités requises sont prises en charge par HTTP.sys, mais pas par Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-153">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="3c4e0-154">Pour plus d’informations sur HTTP.sys, consultez [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="3c4e0-154">For information on HTTP.sys, see [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

![HTTP.sys communique directement avec Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="3c4e0-156">HTTP.sys peut également être utilisé pour les applications qui sont exposées seulement à un réseau interne.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-156">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![HTTP.sys communique directement avec le réseau interne](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3c4e0-158">HTTP.sys est nommé [WebListener](xref:fundamentals/servers/weblistener) dans ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-158">HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span> <span data-ttu-id="3c4e0-159">Si les applications ASP.NET Core sont exécutées sur Windows, WebListener est une alternative pour les scénarios où IIS n’est pas disponible pour héberger les applications.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-159">If ASP.NET Core apps are run on Windows, WebListener is an alternative for scenarios where IIS isn't available to host apps.</span></span>

![WebListener communique directement avec Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="3c4e0-161">WebListener peut également être utilisé à la place de Kestrel pour les applications exposées à un réseau interne, si des fonctionnalités requises sont prises en charge par WebListener, mais pas par Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-161">WebListener can also be used in place of Kestrel for apps that are only exposed to an internal network, if required capabilities are supported by WebListener but not Kestrel.</span></span> <span data-ttu-id="3c4e0-162">Pour plus d’informations sur WebListener, consultez [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="3c4e0-162">For information on WebListener, see [WebListener](xref:fundamentals/servers/weblistener).</span></span>

![WebListener communique directement avec le réseau interne](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="3c4e0-164">Infrastructure serveur d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3c4e0-164">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="3c4e0-165">Le [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) disponible dans la méthode `Startup.Configure` expose la propriété [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) de type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="3c4e0-165">The [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup.Configure` method exposes the [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) property of type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="3c4e0-166">Kestrel et HTTP.sys (WebListener dans ASP.NET Core 1.x) n’exposent chacun qu’une seule fonctionnalité, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), mais des implémentations serveur différentes peuvent exposer des fonctionnalités supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-166">Kestrel and HTTP.sys (WebListener in ASP.NET Core 1.x) only expose a single feature each, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="3c4e0-167">`IServerAddressesFeature` permet de déterminer quel est le port lié à l’implémentation serveur au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-167">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="3c4e0-168">Serveurs personnalisés</span><span class="sxs-lookup"><span data-stu-id="3c4e0-168">Custom servers</span></span>

<span data-ttu-id="3c4e0-169">Si les serveurs intégrés ne répondent pas aux spécifications de l’application, vous pouvez créer une implémentation serveur personnalisée.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-169">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="3c4e0-170">Le [guide OWIN (Open Web Interface pour .NET)](xref:fundamentals/owin) montre comment écrire une implémentation [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) basée sur [Nowin](https://github.com/Bobris/Nowin).</span><span class="sxs-lookup"><span data-stu-id="3c4e0-170">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="3c4e0-171">Seules les interfaces des fonctionnalités utilisées par l’application nécessitent une implémentation, bien qu’au minimum, [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) et [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) doivent être pris en charge.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-171">Only the feature interfaces that the app uses require implementation, though at a minimum [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="3c4e0-172">Démarrage du serveur</span><span class="sxs-lookup"><span data-stu-id="3c4e0-172">Server startup</span></span>

<span data-ttu-id="3c4e0-173">Quand vous utilisez [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio pour Mac](https://www.visualstudio.com/vs/mac/) ou [Visual Studio Code](https://code.visualstudio.com/), le serveur est lancé quand l’application est démarrée par l’IDE.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-173">When using [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio for Mac](https://www.visualstudio.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/), the server is launched when the app is started by the Integrated Development Environment (IDE).</span></span> <span data-ttu-id="3c4e0-174">Dans Visual Studio sur Windows, des profils de lancement peuvent être utilisés pour démarrer l’application et le serveur avec [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[Module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) ou la console.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-174">In Visual Studio on Windows, launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) or the console.</span></span> <span data-ttu-id="3c4e0-175">Dans Visual Studio Code, l’application et le serveur sont démarrés par [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), qui active le débogueur CoreCLR.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-175">In Visual Studio Code, the app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span> <span data-ttu-id="3c4e0-176">Avec Visual Studio pour Mac, l’application et le serveur sont démarrés par le [débogueur Mono Soft-Mode](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span><span class="sxs-lookup"><span data-stu-id="3c4e0-176">Using Visual Studio for Mac, the app and server are started by the [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="3c4e0-177">Lors du lancement d’une application à partir d’une invite de commandes dans le dossier du projet, [dotnet run](/dotnet/core/tools/dotnet-run) lance l’application et le serveur (Kestrel et HTTP.sys uniquement).</span><span class="sxs-lookup"><span data-stu-id="3c4e0-177">When launching an app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="3c4e0-178">La configuration est spécifiée par l’option `-c|--configuration`, qui est définie sur `Debug` (par défaut) ou sur `Release`.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-178">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="3c4e0-179">Si les profils de lancement sont présents dans un fichier *launchSettings.json*, utilisez l’option `--launch-profile <NAME>` pour définir le profil de lancement (par exemple `Development` ou `Production`).</span><span class="sxs-lookup"><span data-stu-id="3c4e0-179">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="3c4e0-180">Pour plus d’informations, consultez les rubriques [dotnet run](/dotnet/core/tools/dotnet-run) et [Empaquetage de la distribution de .NET Core](/dotnet/core/build/distribution-packaging).</span><span class="sxs-lookup"><span data-stu-id="3c4e0-180">For more information, see the [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging) topics.</span></span>

## <a name="http2-support"></a><span data-ttu-id="3c4e0-181">Prise en charge de HTTP/2</span><span class="sxs-lookup"><span data-stu-id="3c4e0-181">HTTP/2 support</span></span>

<span data-ttu-id="3c4e0-182">[HTTP/2](https://httpwg.org/specs/rfc7540.html) est pris en charge avec ASP.NET Core dans les scénarios de déploiement suivants :</span><span class="sxs-lookup"><span data-stu-id="3c4e0-182">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="3c4e0-183">Kestrel</span><span class="sxs-lookup"><span data-stu-id="3c4e0-183">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="3c4e0-184">Système d'exploitation</span><span class="sxs-lookup"><span data-stu-id="3c4e0-184">Operating system</span></span>
    * <span data-ttu-id="3c4e0-185">Windows Server 2012 R2/Windows 8.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="3c4e0-185">Windows Server 2012 R2/Windows 8.1 or later</span></span>
    * <span data-ttu-id="3c4e0-186">Linux avec OpenSSL 1.0.2 ou version ultérieure (par exemple,Ubuntu 16.04 ou version ultérieure)</span><span class="sxs-lookup"><span data-stu-id="3c4e0-186">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="3c4e0-187">HTTP/2 sera pris en charge sur macOS dans une prochaine version.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-187">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="3c4e0-188">Version cible de .Net Framework : .NET Core 2.2 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="3c4e0-188">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="3c4e0-189">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="3c4e0-189">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="3c4e0-190">Windows Server 2016/Windows 10 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="3c4e0-190">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="3c4e0-191">Version cible de .Net Framework : non applicable aux déploiements HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-191">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="3c4e0-192">IIS (in-process)</span><span class="sxs-lookup"><span data-stu-id="3c4e0-192">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="3c4e0-193">Windows Server 2016/Windows 10 ou version ultérieure ; IIS 10 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="3c4e0-193">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="3c4e0-194">Version cible de .Net Framework : .NET Core 2.2 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="3c4e0-194">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="3c4e0-195">IIS (out-of-process)</span><span class="sxs-lookup"><span data-stu-id="3c4e0-195">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="3c4e0-196">Windows Server 2016/Windows 10 ou version ultérieure ; IIS 10 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="3c4e0-196">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="3c4e0-197">Les connexions au serveur périphérique public utilisent HTTP/2, mais la connexion de proxy inverse à Kestrel utilise HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-197">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="3c4e0-198">Version cible de .Net Framework : non applicable aux déploiements IIS out-of-process.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-198">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="3c4e0-199">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="3c4e0-199">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="3c4e0-200">Windows Server 2016/Windows 10 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="3c4e0-200">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="3c4e0-201">Version cible de .Net Framework : non applicable aux déploiements HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-201">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="3c4e0-202">IIS (out-of-process)</span><span class="sxs-lookup"><span data-stu-id="3c4e0-202">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="3c4e0-203">Windows Server 2016/Windows 10 ou version ultérieure ; IIS 10 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="3c4e0-203">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="3c4e0-204">Les connexions au serveur périphérique public utilisent HTTP/2, mais la connexion de proxy inverse à Kestrel utilise HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-204">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="3c4e0-205">Version cible de .Net Framework : non applicable aux déploiements IIS out-of-process.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-205">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="3c4e0-206">Une connexion HTTP/2 doit utiliser la [négociation du protocole de la couche d’application (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) et TLS 1.2 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-206">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="3c4e0-207">Pour plus d’informations, consultez les rubriques qui se rapportent à vos scénarios de déploiement de serveur.</span><span class="sxs-lookup"><span data-stu-id="3c4e0-207">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3c4e0-208">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="3c4e0-208">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <span data-ttu-id="3c4e0-209"><xref:fundamentals/servers/httpsys> (pour ASP.NET Core 1.x, consultez<xref:fundamentals/servers/weblistener>)</span><span class="sxs-lookup"><span data-stu-id="3c4e0-209"><xref:fundamentals/servers/httpsys> (for ASP.NET Core 1.x, see <xref:fundamentals/servers/weblistener>)</span></span>
