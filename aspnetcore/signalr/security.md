---
title: Considérations relatives à la sécurité dans ASP.NET Core SignalR
author: bradygaster
description: Découvrez comment utiliser l’authentification et l’autorisation dans ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- SignalR
uid: signalr/security
ms.openlocfilehash: 4b27d9abb36938ed8161ff0d3535204e3fa68765
ms.sourcegitcommit: f259889044d1fc0f0c7e3882df0008157ced4915
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294706"
---
# <a name="security-considerations-in-aspnet-core-opno-locsignalr"></a><span data-ttu-id="49ae8-103">Considérations relatives à la sécurité dans ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="49ae8-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="49ae8-104">Par [Andrew Stanton-infirmière](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="49ae8-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="49ae8-105">Cet article fournit des informations sur la sécurisation des SignalR.</span><span class="sxs-lookup"><span data-stu-id="49ae8-105">This article provides information on securing SignalR.</span></span>

## <a name="cross-origin-resource-sharing"></a><span data-ttu-id="49ae8-106">Partage de ressources cross-origin</span><span class="sxs-lookup"><span data-stu-id="49ae8-106">Cross-origin resource sharing</span></span>

<span data-ttu-id="49ae8-107">Le [partage des ressources Cross-Origin (cors)](https://www.w3.org/TR/cors/) peut être utilisé pour autoriser les connexions de SignalR Cross-Origin dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="49ae8-107">[Cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="49ae8-108">Si le code JavaScript est hébergé sur un domaine différent de l’application SignalR, l' [intergiciel (middleware) cors](xref:security/cors) doit être activé pour permettre à JavaScript de se connecter à l’application SignalR.</span><span class="sxs-lookup"><span data-stu-id="49ae8-108">If JavaScript code is hosted on a different domain from the SignalR app, [CORS middleware](xref:security/cors) must be enabled to allow the JavaScript to connect to the SignalR app.</span></span> <span data-ttu-id="49ae8-109">Autorisez les requêtes Cross-Origin uniquement à partir des domaines que vous approuvez ou contrôlez.</span><span class="sxs-lookup"><span data-stu-id="49ae8-109">Allow cross-origin requests only from domains you trust or control.</span></span> <span data-ttu-id="49ae8-110">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="49ae8-110">For example:</span></span>

* <span data-ttu-id="49ae8-111">Votre site est hébergé sur `http://www.example.com`</span><span class="sxs-lookup"><span data-stu-id="49ae8-111">Your site is hosted on `http://www.example.com`</span></span>
* <span data-ttu-id="49ae8-112">Votre application SignalR est hébergée sur `http://signalr.example.com`</span><span class="sxs-lookup"><span data-stu-id="49ae8-112">Your SignalR app is hosted on `http://signalr.example.com`</span></span>

<span data-ttu-id="49ae8-113">CORS doit être configuré dans l’application SignalR pour autoriser uniquement le `www.example.com`d’origine.</span><span class="sxs-lookup"><span data-stu-id="49ae8-113">CORS should be configured in the SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="49ae8-114">Pour plus d’informations sur la configuration de CORS, consultez [activer les demandes Cross-Origin (cors)](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="49ae8-114">For more information on configuring CORS, see [Enable Cross-Origin Requests (CORS)](xref:security/cors).</span></span> SignalR<span data-ttu-id="49ae8-115"> **requiert** les stratégies cors suivantes :</span><span class="sxs-lookup"><span data-stu-id="49ae8-115"> **requires** the following CORS policies:</span></span>

* <span data-ttu-id="49ae8-116">Autorisez les origines attendues spécifiques.</span><span class="sxs-lookup"><span data-stu-id="49ae8-116">Allow the specific expected origins.</span></span> <span data-ttu-id="49ae8-117">L’autorisation d’une origine est possible, mais elle n’est **pas** sécurisée ou recommandée.</span><span class="sxs-lookup"><span data-stu-id="49ae8-117">Allowing any origin is possible but is **not** secure or recommended.</span></span>
* <span data-ttu-id="49ae8-118">Les méthodes HTTP `GET` et `POST` doivent être autorisées.</span><span class="sxs-lookup"><span data-stu-id="49ae8-118">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="49ae8-119">Les informations d’identification doivent être autorisées afin que les sessions rémanentes basées sur des cookies fonctionnent correctement.</span><span class="sxs-lookup"><span data-stu-id="49ae8-119">Credentials must be allowed in order for cookie-based sticky sessions to work correctly.</span></span> <span data-ttu-id="49ae8-120">Ils doivent être activés même lorsque l’authentification n’est pas utilisée.</span><span class="sxs-lookup"><span data-stu-id="49ae8-120">They must be enabled even when authentication isn't used.</span></span>

<!--
::: moniker range=">= aspnetcore-5.0"  // Moniker here just to make sure this doesn't get missed in the 5.0 version update.
However, in 5.0 we have provided an option in the TypeScript client to not use credentials.
The not to use credentials option should only be used when you know 100% that credentials like Cookies are not needed in your app (cookies are used by azure app service when using multiple servers)

For more info, see https://github.com/aspnet/AspNetCore.Docs/issues/16003
.-->

<span data-ttu-id="49ae8-121">Par exemple, la stratégie CORS suivante permet à un client de navigateur SignalR hébergé sur `https://example.com` d’accéder à l’application SignalR hébergée sur `https://signalr.example.com`:</span><span class="sxs-lookup"><span data-stu-id="49ae8-121">For example, the following CORS policy allows a SignalR browser client hosted on `https://example.com` to access the SignalR app hosted on `https://signalr.example.com`:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder =>
    {
        builder.WithOrigins("https://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<ChatHub>("/chatHub");
    });

    // ... other middleware ...
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

::: moniker-end

> [!NOTE]
> SignalR<span data-ttu-id="49ae8-122"> n’est pas compatible avec la fonctionnalité CORS intégrée dans Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="49ae8-122"> is not compatible with the built-in CORS feature in Azure App Service.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="49ae8-123">Restriction d’origine WebSocket</span><span class="sxs-lookup"><span data-stu-id="49ae8-123">WebSocket Origin Restriction</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="49ae8-124">Les protections fournies par CORS ne s’appliquent pas aux WebSockets.</span><span class="sxs-lookup"><span data-stu-id="49ae8-124">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="49ae8-125">Pour une restriction d’origine sur les WebSockets, consultez [restriction d’origine des WebSockets](xref:fundamentals/websockets#websocket-origin-restriction).</span><span class="sxs-lookup"><span data-stu-id="49ae8-125">For origin restriction on WebSockets, read [WebSockets origin restriction](xref:fundamentals/websockets#websocket-origin-restriction).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="49ae8-126">Les protections fournies par CORS ne s’appliquent pas aux WebSockets.</span><span class="sxs-lookup"><span data-stu-id="49ae8-126">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="49ae8-127">Les navigateurs **:**</span><span class="sxs-lookup"><span data-stu-id="49ae8-127">Browsers do **not**:</span></span>

* <span data-ttu-id="49ae8-128">n’effectuent pas de requêtes préalables CORS ;</span><span class="sxs-lookup"><span data-stu-id="49ae8-128">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="49ae8-129">respectent les restrictions spécifiées dans les en-têtes `Access-Control` quand ils effectuent des requêtes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="49ae8-129">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="49ae8-130">Toutefois, les navigateurs envoient l’en-tête `Origin` au moment de l’émission des requêtes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="49ae8-130">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="49ae8-131">Les applications doivent être configurées de manière à valider ces en-têtes, le but étant de vérifier que seuls les WebSockets provenant des origines attendues sont autorisés.</span><span class="sxs-lookup"><span data-stu-id="49ae8-131">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="49ae8-132">Dans ASP.NET Core 2,1 et versions ultérieures, la validation d’en-tête peut être obtenue à l’aide d’un intergiciel (middleware) personnalisé placé **avant `UseSignalR`et de l’intergiciel (middleware) d’authentification** dans `Configure`:</span><span class="sxs-lookup"><span data-stu-id="49ae8-132">In ASP.NET Core 2.1 and later, header validation can be achieved using a custom middleware placed **before `UseSignalR`, and authentication middleware** in `Configure`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="49ae8-133">L’en-tête `Origin` est contrôlé par le client et, comme l’en-tête `Referer`, peut être falsifié.</span><span class="sxs-lookup"><span data-stu-id="49ae8-133">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="49ae8-134">Ces en-têtes ne doivent **pas** être utilisés en tant que mécanisme d’authentification.</span><span class="sxs-lookup"><span data-stu-id="49ae8-134">These headers should **not** be used as an authentication mechanism.</span></span>

::: moniker-end

## <a name="connectionid"></a><span data-ttu-id="49ae8-135">ConnectionId</span><span class="sxs-lookup"><span data-stu-id="49ae8-135">ConnectionId</span></span>

<span data-ttu-id="49ae8-136">L’exposition de `ConnectionId` peut entraîner un emprunt d’identité malveillant si la version du client ou du serveur SignalR est ASP.NET Core 2,2 ou une version antérieure.</span><span class="sxs-lookup"><span data-stu-id="49ae8-136">Exposing `ConnectionId` can lead to malicious impersonation if the SignalR server or client version is ASP.NET Core 2.2 or earlier.</span></span> <span data-ttu-id="49ae8-137">Si le serveur SignalR et la version du client sont ASP.NET Core 3,0 ou version ultérieure, le `ConnectionToken` plutôt que le `ConnectionId` doit être gardé secret.</span><span class="sxs-lookup"><span data-stu-id="49ae8-137">If the SignalR server and client version are ASP.NET Core 3.0 or later, the `ConnectionToken` rather than the `ConnectionId` must be kept secret.</span></span> <span data-ttu-id="49ae8-138">L' `ConnectionToken` n’est volontairement pas exposée dans une API.</span><span class="sxs-lookup"><span data-stu-id="49ae8-138">The `ConnectionToken` is purposely not exposed in any API.</span></span>  <span data-ttu-id="49ae8-139">Il peut être difficile de s’assurer que les anciens clients SignalR ne se connectent pas au serveur. par conséquent, même si votre version de SignalR Server est ASP.NET Core 3,0 ou une version ultérieure, le `ConnectionId` ne doit pas être exposé.</span><span class="sxs-lookup"><span data-stu-id="49ae8-139">It can be difficult to ensure that older SignalR clients aren't connecting to the server, so even if your SignalR server version is ASP.NET Core 3.0 or later, the `ConnectionId` shouldn't be exposed.</span></span>

## <a name="access-token-logging"></a><span data-ttu-id="49ae8-140">Journalisation des jetons d’accès</span><span class="sxs-lookup"><span data-stu-id="49ae8-140">Access token logging</span></span>

<span data-ttu-id="49ae8-141">Lorsque vous utilisez des WebSockets ou des événements envoyés par le serveur, le client du navigateur envoie le jeton d’accès dans la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="49ae8-141">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="49ae8-142">La réception du jeton d’accès via la chaîne de requête est généralement sécurisée à l’aide de l’en-tête `Authorization` standard.</span><span class="sxs-lookup"><span data-stu-id="49ae8-142">Receiving the access token via query string is generally secure as using the standard `Authorization` header.</span></span> <span data-ttu-id="49ae8-143">Utilisez toujours le protocole HTTPs pour garantir une connexion sécurisée de bout en bout entre le client et le serveur.</span><span class="sxs-lookup"><span data-stu-id="49ae8-143">Always use HTTPS to ensure a secure end-to-end connection between the client and the server.</span></span> <span data-ttu-id="49ae8-144">De nombreux serveurs Web consignent l’URL pour chaque demande, y compris la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="49ae8-144">Many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="49ae8-145">La journalisation des URL peut enregistrer le jeton d’accès.</span><span class="sxs-lookup"><span data-stu-id="49ae8-145">Logging the URLs may log the access token.</span></span> <span data-ttu-id="49ae8-146">ASP.NET Core enregistre l’URL pour chaque demande par défaut, qui inclut la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="49ae8-146">ASP.NET Core logs the URL for each request by default, which will include the query string.</span></span> <span data-ttu-id="49ae8-147">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="49ae8-147">For example:</span></span>

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

<span data-ttu-id="49ae8-148">Si vous avez des doutes quant à la journalisation de ces données avec les journaux de votre serveur, vous pouvez désactiver cette journalisation en configurant l’enregistreur d' `Microsoft.AspNetCore.Hosting` au niveau `Warning` ou au-dessus (ces messages sont écrits au niveau `Info`).</span><span class="sxs-lookup"><span data-stu-id="49ae8-148">If you have concerns about logging this data with your server logs, you can disable this logging entirely by configuring the `Microsoft.AspNetCore.Hosting` logger to the `Warning` level or above (these messages are written at `Info` level).</span></span> <span data-ttu-id="49ae8-149">Pour plus d’informations, consultez [filtrage des journaux](xref:fundamentals/logging/index#log-filtering) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="49ae8-149">For more information, see [Log Filtering](xref:fundamentals/logging/index#log-filtering) for more information.</span></span> <span data-ttu-id="49ae8-150">Si vous souhaitez toujours enregistrer certaines informations de demande, vous pouvez [écrire un intergiciel (middleware](xref:fundamentals/middleware/write) ) pour enregistrer les données dont vous avez besoin et filtrer les `access_token` valeur de chaîne de requête (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="49ae8-150">If you still want to log certain request information, you can [write a middleware](xref:fundamentals/middleware/write) to log the data you require and filter out the `access_token` query string value (if present).</span></span>

## <a name="exceptions"></a><span data-ttu-id="49ae8-151">Exceptions</span><span class="sxs-lookup"><span data-stu-id="49ae8-151">Exceptions</span></span>

<span data-ttu-id="49ae8-152">Les messages d’exception sont généralement considérés comme des données sensibles qui ne doivent pas être révélées à un client.</span><span class="sxs-lookup"><span data-stu-id="49ae8-152">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="49ae8-153">Par défaut, SignalR n’envoie pas les détails d’une exception levée par une méthode de concentrateur au client.</span><span class="sxs-lookup"><span data-stu-id="49ae8-153">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="49ae8-154">Au lieu de cela, le client reçoit un message générique indiquant qu’une erreur s’est produite.</span><span class="sxs-lookup"><span data-stu-id="49ae8-154">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="49ae8-155">La remise de message d’exception au client peut être remplacée (par exemple, dans le développement ou le test) par [EnableDetailedErrors](xref:signalr/configuration#configure-server-options).</span><span class="sxs-lookup"><span data-stu-id="49ae8-155">Exception message delivery to the client can be overridden (for example in development or test) with [EnableDetailedErrors](xref:signalr/configuration#configure-server-options).</span></span> <span data-ttu-id="49ae8-156">Les messages d’exception ne doivent pas être exposés au client dans les applications de production.</span><span class="sxs-lookup"><span data-stu-id="49ae8-156">Exception messages should not be exposed to the client in production apps.</span></span>

## <a name="buffer-management"></a><span data-ttu-id="49ae8-157">Gestion des tampons</span><span class="sxs-lookup"><span data-stu-id="49ae8-157">Buffer management</span></span>

SignalR<span data-ttu-id="49ae8-158"> utilise des tampons par connexion pour gérer les messages entrants et sortants.</span><span class="sxs-lookup"><span data-stu-id="49ae8-158"> uses per-connection buffers to manage incoming and outgoing messages.</span></span> <span data-ttu-id="49ae8-159">Par défaut, SignalR limite ces mémoires tampon à 32 Ko.</span><span class="sxs-lookup"><span data-stu-id="49ae8-159">By default, SignalR limits these buffers to 32 KB.</span></span> <span data-ttu-id="49ae8-160">Le plus grand message qu’un client ou un serveur peut envoyer est de 32 Ko.</span><span class="sxs-lookup"><span data-stu-id="49ae8-160">The largest message a client or server can send is 32 KB.</span></span> <span data-ttu-id="49ae8-161">La mémoire maximale consommée par une connexion pour les messages est de 32 Ko.</span><span class="sxs-lookup"><span data-stu-id="49ae8-161">The maximum memory consumed by a connection for messages is 32 KB.</span></span> <span data-ttu-id="49ae8-162">Si vos messages sont toujours inférieurs à 32 Ko, vous pouvez réduire la limite, qui :</span><span class="sxs-lookup"><span data-stu-id="49ae8-162">If your messages are always smaller than 32 KB, you can reduce the limit, which:</span></span>

* <span data-ttu-id="49ae8-163">Empêche un client d’envoyer un message plus volumineux.</span><span class="sxs-lookup"><span data-stu-id="49ae8-163">Prevents a client from being able to send a larger message.</span></span>
* <span data-ttu-id="49ae8-164">Le serveur n’a jamais besoin d’allouer de grandes mémoires tampons pour accepter des messages.</span><span class="sxs-lookup"><span data-stu-id="49ae8-164">The server will never need to allocate large buffers to accept messages.</span></span>

<span data-ttu-id="49ae8-165">Si vos messages sont supérieurs à 32 Ko, vous pouvez augmenter la limite.</span><span class="sxs-lookup"><span data-stu-id="49ae8-165">If your messages are larger than 32 KB, you can increase the limit.</span></span> <span data-ttu-id="49ae8-166">L’amélioration de cette limite signifie :</span><span class="sxs-lookup"><span data-stu-id="49ae8-166">Increasing this limit means:</span></span>

* <span data-ttu-id="49ae8-167">Le client peut forcer le serveur à allouer de grandes mémoires tampons de mémoire.</span><span class="sxs-lookup"><span data-stu-id="49ae8-167">The client can cause the server to allocate large memory buffers.</span></span>
* <span data-ttu-id="49ae8-168">L’allocation de serveurs de mémoires tampons de grande taille peut réduire le nombre de connexions simultanées.</span><span class="sxs-lookup"><span data-stu-id="49ae8-168">Server allocation of large buffers may reduce the number of concurrent connections.</span></span>

<span data-ttu-id="49ae8-169">Il existe des limites pour les messages entrants et sortants, qui peuvent être configurés sur l’objet [HttpConnectionDispatcherOptions](xref:signalr/configuration#configure-server-options) configuré dans `MapHub`:</span><span class="sxs-lookup"><span data-stu-id="49ae8-169">There are limits for incoming and outgoing messages, both can be configured on the [HttpConnectionDispatcherOptions](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="49ae8-170">`ApplicationMaxBufferSize` représente le nombre maximal d’octets du client que le serveur met en mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="49ae8-170">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="49ae8-171">Si le client tente d’envoyer un message d’une taille supérieure à cette limite, la connexion peut être fermée.</span><span class="sxs-lookup"><span data-stu-id="49ae8-171">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="49ae8-172">`TransportMaxBufferSize` représente le nombre maximal d’octets que le serveur peut envoyer.</span><span class="sxs-lookup"><span data-stu-id="49ae8-172">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="49ae8-173">Si le serveur tente d’envoyer un message (y compris les valeurs de retour des méthodes de concentrateur) supérieures à cette limite, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="49ae8-173">If the server attempts to send a message (including return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="49ae8-174">Définir la limite sur `0` désactive la limite.</span><span class="sxs-lookup"><span data-stu-id="49ae8-174">Setting the limit to `0` disables the limit.</span></span> <span data-ttu-id="49ae8-175">La suppression de la limite permet à un client d’envoyer un message de toute taille.</span><span class="sxs-lookup"><span data-stu-id="49ae8-175">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="49ae8-176">Les clients malveillants qui envoient des messages volumineux peuvent entraîner l’allocation de mémoire supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="49ae8-176">Malicious clients sending large messages can cause excess memory to be allocated.</span></span> <span data-ttu-id="49ae8-177">L’utilisation excessive de la mémoire peut réduire de manière significative le nombre de connexions simultanées.</span><span class="sxs-lookup"><span data-stu-id="49ae8-177">Excess memory usage can significantly reduce the number of concurrent connections.</span></span>
