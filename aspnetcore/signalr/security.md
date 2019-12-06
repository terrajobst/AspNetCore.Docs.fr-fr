---
title: Considérations relatives à la sécurité dans ASP.NET Core SignalR
author: bradygaster
description: Découvrez comment utiliser l’authentification et l’autorisation dans ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- SignalR
uid: signalr/security
ms.openlocfilehash: f443fe0fbaaa1facd09edc0878c048772895ecff
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881180"
---
# <a name="security-considerations-in-aspnet-core-opno-locsignalr"></a><span data-ttu-id="5d5c3-103">Considérations relatives à la sécurité dans ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="5d5c3-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="5d5c3-104">Par [Andrew Stanton-infirmière](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="5d5c3-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="5d5c3-105">Cet article fournit des informations sur la sécurisation des SignalR.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-105">This article provides information on securing SignalR.</span></span>

## <a name="cross-origin-resource-sharing"></a><span data-ttu-id="5d5c3-106">Partage de ressources cross-origin</span><span class="sxs-lookup"><span data-stu-id="5d5c3-106">Cross-origin resource sharing</span></span>

<span data-ttu-id="5d5c3-107">Le [partage des ressources Cross-Origin (cors)](https://www.w3.org/TR/cors/) peut être utilisé pour autoriser les connexions de SignalR Cross-Origin dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-107">[Cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="5d5c3-108">Si le code JavaScript est hébergé sur un domaine différent de l’application SignalR, l' [intergiciel (middleware) cors](xref:security/cors) doit être activé pour permettre à JavaScript de se connecter à l’application SignalR.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-108">If JavaScript code is hosted on a different domain from the SignalR app, [CORS middleware](xref:security/cors) must be enabled to allow the JavaScript to connect to the SignalR app.</span></span> <span data-ttu-id="5d5c3-109">Autorisez les requêtes Cross-Origin uniquement à partir des domaines que vous approuvez ou contrôlez.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-109">Allow cross-origin requests only from domains you trust or control.</span></span> <span data-ttu-id="5d5c3-110">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="5d5c3-110">For example:</span></span>

* <span data-ttu-id="5d5c3-111">Votre site est hébergé sur `http://www.example.com`</span><span class="sxs-lookup"><span data-stu-id="5d5c3-111">Your site is hosted on `http://www.example.com`</span></span>
* <span data-ttu-id="5d5c3-112">Votre application SignalR est hébergée sur `http://signalr.example.com`</span><span class="sxs-lookup"><span data-stu-id="5d5c3-112">Your SignalR app is hosted on `http://signalr.example.com`</span></span>

<span data-ttu-id="5d5c3-113">CORS doit être configuré dans l’application SignalR pour autoriser uniquement le `www.example.com`d’origine.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-113">CORS should be configured in the SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="5d5c3-114">Pour plus d’informations sur la configuration de CORS, consultez [activer les demandes Cross-Origin (cors)](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="5d5c3-114">For more information on configuring CORS, see [Enable Cross-Origin Requests (CORS)](xref:security/cors).</span></span> SignalR<span data-ttu-id="5d5c3-115"> **requiert** les stratégies cors suivantes :</span><span class="sxs-lookup"><span data-stu-id="5d5c3-115"> **requires** the following CORS policies:</span></span>

* <span data-ttu-id="5d5c3-116">Autorisez les origines attendues spécifiques.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-116">Allow the specific expected origins.</span></span> <span data-ttu-id="5d5c3-117">L’autorisation d’une origine est possible, mais elle n’est **pas** sécurisée ou recommandée.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-117">Allowing any origin is possible but is **not** secure or recommended.</span></span>
* <span data-ttu-id="5d5c3-118">Les méthodes HTTP `GET` et `POST` doivent être autorisées.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-118">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="5d5c3-119">Les informations d’identification doivent être activées, même lorsque l’authentification n’est pas utilisée.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-119">Credentials must be enabled, even when authentication is not used.</span></span>

<span data-ttu-id="5d5c3-120">Par exemple, la stratégie CORS suivante permet à un client de navigateur SignalR hébergé sur `https://example.com` d’accéder à l’application SignalR hébergée sur `https://signalr.example.com`:</span><span class="sxs-lookup"><span data-stu-id="5d5c3-120">For example, the following CORS policy allows a SignalR browser client hosted on `https://example.com` to access the SignalR app hosted on `https://signalr.example.com`:</span></span>

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
> SignalR<span data-ttu-id="5d5c3-121"> n’est pas compatible avec la fonctionnalité CORS intégrée dans Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-121"> is not compatible with the built-in CORS feature in Azure App Service.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="5d5c3-122">Restriction d’origine WebSocket</span><span class="sxs-lookup"><span data-stu-id="5d5c3-122">WebSocket Origin Restriction</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="5d5c3-123">Les protections fournies par CORS ne s’appliquent pas aux WebSockets.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-123">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="5d5c3-124">Pour une restriction d’origine sur les WebSockets, consultez [restriction d’origine des WebSockets](xref:fundamentals/websockets#websocket-origin-restriction).</span><span class="sxs-lookup"><span data-stu-id="5d5c3-124">For origin restriction on WebSockets, read [WebSockets origin restriction](xref:fundamentals/websockets#websocket-origin-restriction).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="5d5c3-125">Les protections fournies par CORS ne s’appliquent pas aux WebSockets.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-125">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="5d5c3-126">Les navigateurs **:**</span><span class="sxs-lookup"><span data-stu-id="5d5c3-126">Browsers do **not**:</span></span>

* <span data-ttu-id="5d5c3-127">n’effectuent pas de requêtes préalables CORS ;</span><span class="sxs-lookup"><span data-stu-id="5d5c3-127">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="5d5c3-128">respectent les restrictions spécifiées dans les en-têtes `Access-Control` quand ils effectuent des requêtes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-128">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="5d5c3-129">Toutefois, les navigateurs envoient l’en-tête `Origin` au moment de l’émission des requêtes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-129">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="5d5c3-130">Les applications doivent être configurées de manière à valider ces en-têtes, le but étant de vérifier que seuls les WebSockets provenant des origines attendues sont autorisés.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-130">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="5d5c3-131">Dans ASP.NET Core 2,1 et versions ultérieures, la validation d’en-tête peut être obtenue à l’aide d’un intergiciel (middleware) personnalisé placé **avant `UseSignalR`et de l’intergiciel (middleware) d’authentification** dans `Configure`:</span><span class="sxs-lookup"><span data-stu-id="5d5c3-131">In ASP.NET Core 2.1 and later, header validation can be achieved using a custom middleware placed **before `UseSignalR`, and authentication middleware** in `Configure`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="5d5c3-132">L’en-tête `Origin` est contrôlé par le client et, comme l’en-tête `Referer`, peut être falsifié.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-132">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="5d5c3-133">Ces en-têtes ne doivent **pas** être utilisés en tant que mécanisme d’authentification.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-133">These headers should **not** be used as an authentication mechanism.</span></span>

::: moniker-end

## <a name="access-token-logging"></a><span data-ttu-id="5d5c3-134">Journalisation des jetons d’accès</span><span class="sxs-lookup"><span data-stu-id="5d5c3-134">Access token logging</span></span>

<span data-ttu-id="5d5c3-135">Lorsque vous utilisez des WebSockets ou des événements envoyés par le serveur, le client du navigateur envoie le jeton d’accès dans la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-135">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="5d5c3-136">La réception du jeton d’accès via la chaîne de requête est généralement aussi sécurisée que l’utilisation de l’en-tête `Authorization` standard.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-136">Receiving the access token via query string is generally as secure as using the standard `Authorization` header.</span></span> <span data-ttu-id="5d5c3-137">Vous devez toujours utiliser le protocole HTTPs pour garantir une connexion sécurisée de bout en bout entre le client et le serveur.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-137">You should always use HTTPS to ensure a secure end-to-end connection between the client and the server.</span></span> <span data-ttu-id="5d5c3-138">De nombreux serveurs Web consignent l’URL pour chaque demande, y compris la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-138">Many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="5d5c3-139">La journalisation des URL peut enregistrer le jeton d’accès.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-139">Logging the URLs may log the access token.</span></span> <span data-ttu-id="5d5c3-140">ASP.NET Core enregistre l’URL pour chaque demande par défaut, qui inclut la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-140">ASP.NET Core logs the URL for each request by default, which will include the query string.</span></span> <span data-ttu-id="5d5c3-141">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="5d5c3-141">For example:</span></span>

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

<span data-ttu-id="5d5c3-142">Si vous avez des doutes quant à la journalisation de ces données avec les journaux de votre serveur, vous pouvez désactiver cette journalisation en configurant l’enregistreur d' `Microsoft.AspNetCore.Hosting` au niveau `Warning` ou au-dessus (ces messages sont écrits au niveau `Info`).</span><span class="sxs-lookup"><span data-stu-id="5d5c3-142">If you have concerns about logging this data with your server logs, you can disable this logging entirely by configuring the `Microsoft.AspNetCore.Hosting` logger to the `Warning` level or above (these messages are written at `Info` level).</span></span> <span data-ttu-id="5d5c3-143">Pour plus d’informations, consultez la documentation relative au [filtrage des journaux](xref:fundamentals/logging/index#log-filtering) .</span><span class="sxs-lookup"><span data-stu-id="5d5c3-143">See the documentation on [Log Filtering](xref:fundamentals/logging/index#log-filtering) for more information.</span></span> <span data-ttu-id="5d5c3-144">Si vous souhaitez toujours enregistrer certaines informations de demande, vous pouvez [écrire un intergiciel (middleware](xref:fundamentals/middleware/write) ) pour enregistrer les données dont vous avez besoin et filtrer les `access_token` valeur de chaîne de requête (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="5d5c3-144">If you still want to log certain request information, you can [write a middleware](xref:fundamentals/middleware/write) to log the data you require and filter out the `access_token` query string value (if present).</span></span>

## <a name="exceptions"></a><span data-ttu-id="5d5c3-145">Exceptions</span><span class="sxs-lookup"><span data-stu-id="5d5c3-145">Exceptions</span></span>

<span data-ttu-id="5d5c3-146">Les messages d’exception sont généralement considérés comme des données sensibles qui ne doivent pas être révélées à un client.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-146">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="5d5c3-147">Par défaut, SignalR n’envoie pas les détails d’une exception levée par une méthode de concentrateur au client.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-147">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="5d5c3-148">Au lieu de cela, le client reçoit un message générique indiquant qu’une erreur s’est produite.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-148">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="5d5c3-149">La remise de message d’exception au client peut être remplacée (par exemple, dans le développement ou le test) par [EnableDetailedErrors](xref:signalr/configuration#configure-server-options).</span><span class="sxs-lookup"><span data-stu-id="5d5c3-149">Exception message delivery to the client can be overridden (for example in development or test) with [EnableDetailedErrors](xref:signalr/configuration#configure-server-options).</span></span> <span data-ttu-id="5d5c3-150">Les messages d’exception ne doivent pas être exposés au client dans les applications de production.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-150">Exception messages should not be exposed to the client in production apps.</span></span>

## <a name="buffer-management"></a><span data-ttu-id="5d5c3-151">Gestion des tampons</span><span class="sxs-lookup"><span data-stu-id="5d5c3-151">Buffer management</span></span>

SignalR<span data-ttu-id="5d5c3-152"> utilise des tampons par connexion pour gérer les messages entrants et sortants.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-152"> uses per-connection buffers to manage incoming and outgoing messages.</span></span> <span data-ttu-id="5d5c3-153">Par défaut, SignalR limite ces mémoires tampon à 32 Ko.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-153">By default, SignalR limits these buffers to 32 KB.</span></span> <span data-ttu-id="5d5c3-154">Le plus grand message qu’un client ou un serveur peut envoyer est de 32 Ko.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-154">The largest message a client or server can send is 32 KB.</span></span> <span data-ttu-id="5d5c3-155">La mémoire maximale consommée par une connexion pour les messages est de 32 Ko.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-155">The maximum memory consumed by a connection for messages is 32 KB.</span></span> <span data-ttu-id="5d5c3-156">Si vos messages sont toujours inférieurs à 32 Ko, vous pouvez réduire la limite, qui :</span><span class="sxs-lookup"><span data-stu-id="5d5c3-156">If your messages are always smaller than 32 KB, you can reduce the limit, which:</span></span>

* <span data-ttu-id="5d5c3-157">Empêche un client d’envoyer un message plus volumineux.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-157">Prevents a client from being able to send a larger message.</span></span>
* <span data-ttu-id="5d5c3-158">Le serveur n’a jamais besoin d’allouer de grandes mémoires tampons pour accepter des messages.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-158">The server will never need to allocate large buffers to accept messages.</span></span>

<span data-ttu-id="5d5c3-159">Si vos messages sont supérieurs à 32 Ko, vous pouvez augmenter la limite.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-159">If your messages are larger than 32 KB, you can increase the limit.</span></span> <span data-ttu-id="5d5c3-160">L’amélioration de cette limite signifie :</span><span class="sxs-lookup"><span data-stu-id="5d5c3-160">Increasing this limit means:</span></span>

* <span data-ttu-id="5d5c3-161">Le client peut forcer le serveur à allouer de grandes mémoires tampons de mémoire.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-161">The client can cause the server to allocate large memory buffers.</span></span>
* <span data-ttu-id="5d5c3-162">L’allocation de serveurs de mémoires tampons de grande taille peut réduire le nombre de connexions simultanées.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-162">Server allocation of large buffers may reduce the number of concurrent connections.</span></span>

<span data-ttu-id="5d5c3-163">Il existe des limites pour les messages entrants et sortants, qui peuvent être configurés sur l’objet [HttpConnectionDispatcherOptions](xref:signalr/configuration#configure-server-options) configuré dans `MapHub`:</span><span class="sxs-lookup"><span data-stu-id="5d5c3-163">There are limits for incoming and outgoing messages, both can be configured on the [HttpConnectionDispatcherOptions](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="5d5c3-164">`ApplicationMaxBufferSize` représente le nombre maximal d’octets du client que le serveur met en mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-164">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="5d5c3-165">Si le client tente d’envoyer un message d’une taille supérieure à cette limite, la connexion peut être fermée.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-165">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="5d5c3-166">`TransportMaxBufferSize` représente le nombre maximal d’octets que le serveur peut envoyer.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-166">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="5d5c3-167">Si le serveur tente d’envoyer un message (y compris les valeurs de retour des méthodes de concentrateur) supérieures à cette limite, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-167">If the server attempts to send a message (including return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="5d5c3-168">Définir la limite sur `0` désactive la limite.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-168">Setting the limit to `0` disables the limit.</span></span> <span data-ttu-id="5d5c3-169">La suppression de la limite permet à un client d’envoyer un message de toute taille.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-169">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="5d5c3-170">Les clients malveillants qui envoient des messages volumineux peuvent entraîner l’allocation de mémoire supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-170">Malicious clients sending large messages can cause excess memory to be allocated.</span></span> <span data-ttu-id="5d5c3-171">L’utilisation excessive de la mémoire peut réduire de manière significative le nombre de connexions simultanées.</span><span class="sxs-lookup"><span data-stu-id="5d5c3-171">Excess memory usage can significantly reduce the number of concurrent connections.</span></span>
