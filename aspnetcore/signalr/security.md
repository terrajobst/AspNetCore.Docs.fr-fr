---
title: Considérations de sécurité dans ASP.NET Core SignalR
author: tdykstra
description: Découvrez comment utiliser l’authentification et les autorisations dans ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 10/17/2018
uid: signalr/security
ms.openlocfilehash: be1dd24c40327d9a0d8f91bf75300128d3d52725
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225367"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="3d2e3-103">Considérations de sécurité dans ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="3d2e3-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="3d2e3-104">Par [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="3d2e3-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="3d2e3-105">Cet article fournit des informations sur la sécurisation de SignalR.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-105">This article provides information on securing SignalR.</span></span>

## <a name="cross-origin-resource-sharing"></a><span data-ttu-id="3d2e3-106">Partage des ressources cross-origin</span><span class="sxs-lookup"><span data-stu-id="3d2e3-106">Cross-origin resource sharing</span></span>

<span data-ttu-id="3d2e3-107">[Ressources cross-origin CORS (partage)](https://www.w3.org/TR/cors/) peut être utilisé pour autoriser les connexions SignalR cross-origine dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-107">[Cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="3d2e3-108">Si le code JavaScript est hébergé sur un domaine différent de l’application SignalR, [intergiciel (middleware) CORS](xref:security/cors) doit être activée pour autoriser le code JavaScript pour se connecter à l’application de SignalR.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-108">If JavaScript code is hosted on a different domain from the SignalR app, [CORS middleware](xref:security/cors) must be enabled to allow the JavaScript to connect to the SignalR app.</span></span> <span data-ttu-id="3d2e3-109">Autoriser les demandes cross-origin uniquement à partir de domaines de que confiance ou un contrôle.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-109">Allow cross-origin requests only from domains you trust or control.</span></span> <span data-ttu-id="3d2e3-110">Exemple :</span><span class="sxs-lookup"><span data-stu-id="3d2e3-110">For example:</span></span>

* <span data-ttu-id="3d2e3-111">Votre site est hébergé sur `http://www.example.com`</span><span class="sxs-lookup"><span data-stu-id="3d2e3-111">Your site is hosted on `http://www.example.com`</span></span>
* <span data-ttu-id="3d2e3-112">Votre application SignalR est hébergée sur `http://signalr.example.com`</span><span class="sxs-lookup"><span data-stu-id="3d2e3-112">Your SignalR app is hosted on `http://signalr.example.com`</span></span>

<span data-ttu-id="3d2e3-113">CORS doit être configuré dans l’application de SignalR pour autoriser uniquement l’origine `www.example.com`.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-113">CORS should be configured in the SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="3d2e3-114">Pour plus d’informations sur la configuration de CORS, consultez [activer de demandes de Cross-Origin (CORS)](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="3d2e3-114">For more information on configuring CORS, see [Enable Cross-Origin Requests (CORS)](xref:security/cors).</span></span> <span data-ttu-id="3d2e3-115">SignalR **requiert** les stratégies CORS suivantes :</span><span class="sxs-lookup"><span data-stu-id="3d2e3-115">SignalR **requires** the following CORS policies:</span></span>

* <span data-ttu-id="3d2e3-116">Autoriser les origines attendus spécifiques.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-116">Allow the specific expected origins.</span></span> <span data-ttu-id="3d2e3-117">Autoriser toute origine est possible mais est **pas** sécurisée ou recommandée.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-117">Allowing any origin is possible but is **not** secure or recommended.</span></span>
* <span data-ttu-id="3d2e3-118">Méthodes HTTP `GET` et `POST` doivent être autorisés.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-118">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="3d2e3-119">Informations d’identification doivent être activées, même lorsque l’authentification n’est pas utilisée.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-119">Credentials must be enabled, even when authentication is not used.</span></span>

<span data-ttu-id="3d2e3-120">Par exemple, la stratégie CORS suivante permet à un client de navigateur de SignalR hébergé sur `http://example.com` pour accéder à l’application de SignalR hébergée sur `http://signalr.example.com`:</span><span class="sxs-lookup"><span data-stu-id="3d2e3-120">For example, the following CORS policy allows a SignalR browser client hosted on `http://example.com` to access the SignalR app hosted on `http://signalr.example.com`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

> [!NOTE]
> <span data-ttu-id="3d2e3-121">SignalR n’est pas compatible avec la fonctionnalité CORS intégrée dans Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-121">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="3d2e3-122">Restriction de l’origine de WebSocket</span><span class="sxs-lookup"><span data-stu-id="3d2e3-122">WebSocket Origin Restriction</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="3d2e3-123">Les protections fournies par CORS ne s’appliquent pas aux WebSockets.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-123">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="3d2e3-124">Pour la restriction de l’origine sur WebSockets, lisez [restriction de l’origine WebSockets](xref:fundamentals/websockets#websocket-origin-restriction).</span><span class="sxs-lookup"><span data-stu-id="3d2e3-124">For origin restriction on WebSockets, read [WebSockets origin restriction](xref:fundamentals/websockets#websocket-origin-restriction).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="3d2e3-125">Les protections fournies par CORS ne s’appliquent pas aux WebSockets.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-125">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="3d2e3-126">Navigateurs **pas**:</span><span class="sxs-lookup"><span data-stu-id="3d2e3-126">Browsers do **not**:</span></span>

* <span data-ttu-id="3d2e3-127">Effectuer des demandes de contrôle préliminaire CORS.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-127">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="3d2e3-128">Respecter les restrictions spécifiées dans `Access-Control` en-têtes pour effectuer des demandes de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-128">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="3d2e3-129">Toutefois, les navigateurs envoient le `Origin` en-tête lors de l’émission des demandes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-129">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="3d2e3-130">Applications doivent être configurées pour valider ces en-têtes pour s’assurer qu’uniquement les WebSockets provenant les origines attendus sont autorisées.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-130">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="3d2e3-131">Dans ASP.NET Core 2.1 et versions ultérieures, validation de l’en-tête peut être obtenue à l’aide d’un intergiciel (middleware) personnalisé placé **avant `UseSignalR`et l’intergiciel d’authentification** dans `Configure`:</span><span class="sxs-lookup"><span data-stu-id="3d2e3-131">In ASP.NET Core 2.1 and later, header validation can be achieved using a custom middleware placed **before `UseSignalR`, and authentication middleware** in `Configure`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="3d2e3-132">Le `Origin` en-tête est contrôlé par le client et, comme le `Referer` en-tête, peut être fausses.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-132">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="3d2e3-133">Ces en-têtes doivent **pas** être utilisé comme mécanisme d’authentification.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-133">These headers should **not** be used as an authentication mechanism.</span></span>

::: moniker-end

## <a name="access-token-logging"></a><span data-ttu-id="3d2e3-134">Connexion de jeton d’accès</span><span class="sxs-lookup"><span data-stu-id="3d2e3-134">Access token logging</span></span>

<span data-ttu-id="3d2e3-135">Lorsque vous utilisez des WebSockets ou les événements, le navigateur client envoie le jeton d’accès dans la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-135">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="3d2e3-136">Reçoit le jeton d’accès via la chaîne de requête est généralement aussi sécurisé qu’à l’aide de la norme `Authorization` en-tête.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-136">Receiving the access token via query string is generally as secure as using the standard `Authorization` header.</span></span> <span data-ttu-id="3d2e3-137">Toutefois, plusieurs serveurs web connecter à l’URL pour chaque demande, y compris la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-137">However, many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="3d2e3-138">Journalisation de l’URL peut enregistrer le jeton d’accès.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-138">Logging the URLs may log the access token.</span></span> <span data-ttu-id="3d2e3-139">Une bonne pratique consiste à définir des paramètres de journalisation du serveur pour empêcher les jetons d’accès de journalisation le web.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-139">A best practice is to set the web server's logging settings to prevent logging access tokens.</span></span>

## <a name="exceptions"></a><span data-ttu-id="3d2e3-140">Exceptions</span><span class="sxs-lookup"><span data-stu-id="3d2e3-140">Exceptions</span></span>

<span data-ttu-id="3d2e3-141">Messages d’exception sont généralement considérés comme des données sensibles qui ne doivent pas être révélées à un client.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-141">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="3d2e3-142">Par défaut, les détails d’une exception levée par une méthode de concentrateur au client n’envoie pas de SignalR.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-142">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="3d2e3-143">Au lieu de cela, le client reçoit un message générique indiquant qu'une erreur s’est produite.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-143">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="3d2e3-144">Remise de messages d’exception pour le client peut être remplacée (par exemple dans le développement ou test) par [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options).</span><span class="sxs-lookup"><span data-stu-id="3d2e3-144">Exception message delivery to the client can be overridden (for example in development or test) with [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options).</span></span> <span data-ttu-id="3d2e3-145">Messages d’exception ne doivent pas être exposées au client dans les applications de production.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-145">Exception messages should not be exposed to the client in production apps.</span></span>

## <a name="buffer-management"></a><span data-ttu-id="3d2e3-146">Gestion des tampons</span><span class="sxs-lookup"><span data-stu-id="3d2e3-146">Buffer management</span></span>

<span data-ttu-id="3d2e3-147">SignalR utilise des mémoires tampons de chaque connexion pour gérer les messages entrants et sortants.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-147">SignalR uses per-connection buffers to manage incoming and outgoing messages.</span></span> <span data-ttu-id="3d2e3-148">Par défaut, SignalR limite ces mémoires tampons à 32 Ko.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-148">By default, SignalR limits these buffers to 32 KB.</span></span> <span data-ttu-id="3d2e3-149">Un client ou le serveur peut envoyer message maximale est de 32 Ko.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-149">The largest message a client or server can send is 32 KB.</span></span> <span data-ttu-id="3d2e3-150">La mémoire maximale utilisée par une connexion pour les messages est de 32 Ko.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-150">The maximum memory consumed by a connection for messages is 32 KB.</span></span> <span data-ttu-id="3d2e3-151">Si vos messages sont toujours inférieures à 32 Ko, vous pouvez réduire la limite, dont :</span><span class="sxs-lookup"><span data-stu-id="3d2e3-151">If your messages are always smaller than 32 KB, you can reduce the limit, which:</span></span>

* <span data-ttu-id="3d2e3-152">Empêche un client d’être en mesure d’envoyer un message plus volumineux.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-152">Prevents a client from being able to send a larger message.</span></span>
* <span data-ttu-id="3d2e3-153">Le serveur devra jamais allouer des tampons de grande taille pour accepter les messages.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-153">The server will never need to allocate large buffers to accept messages.</span></span>

<span data-ttu-id="3d2e3-154">Si vos messages sont supérieur à 32 Ko, vous pouvez augmenter la limite.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-154">If your messages are larger than 32 KB, you can increase the limit.</span></span> <span data-ttu-id="3d2e3-155">Si vous augmentez cette limite :</span><span class="sxs-lookup"><span data-stu-id="3d2e3-155">Increasing this limit means:</span></span>

* <span data-ttu-id="3d2e3-156">Le client peut entraîner le serveur d’allouer des mémoires tampons volumineuses.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-156">The client can cause the server to allocate large memory buffers.</span></span>
* <span data-ttu-id="3d2e3-157">Allocation de serveur de tampons de grande taille peut réduire le nombre de connexions simultanées.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-157">Server allocation of large buffers may reduce the number of concurrent connections.</span></span>

<span data-ttu-id="3d2e3-158">Il existe des limites pour les messages entrants et sortants, les deux peuvent être configurées sur le [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) objet configuré dans `MapHub`:</span><span class="sxs-lookup"><span data-stu-id="3d2e3-158">There are limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="3d2e3-159">`ApplicationMaxBufferSize` représente le nombre maximal d’octets à partir du client qui les mémoires tampons de serveur.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-159">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="3d2e3-160">Si le client tente d’envoyer un message dépasse cette limite, la connexion peut être fermée.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-160">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="3d2e3-161">`TransportMaxBufferSize` représente le nombre maximal d’octets que le serveur peut envoyer.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-161">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="3d2e3-162">Si le serveur tente d’envoyer un message (y compris les valeurs de retour des méthodes de concentrateur) dépasse cette limite, une exception sera levée.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-162">If the server attempts to send a message (including return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="3d2e3-163">Définition de la limite `0` désactive la limite.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-163">Setting the limit to `0` disables the limit.</span></span> <span data-ttu-id="3d2e3-164">Suppression d’une limite permet à un client envoyer un message de n’importe quelle taille.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-164">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="3d2e3-165">Clients malveillants envoi de messages volumineux peuvent entraîner de trop de mémoire à allouer.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-165">Malicious clients sending large messages can cause excess memory to be allocated.</span></span> <span data-ttu-id="3d2e3-166">Utilisation excessive de la mémoire peut réduire considérablement le nombre de connexions simultanées.</span><span class="sxs-lookup"><span data-stu-id="3d2e3-166">Excess memory usage can significantly reduce the number of concurrent connections.</span></span>
