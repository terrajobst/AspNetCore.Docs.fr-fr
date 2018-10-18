---
title: Considérations de sécurité dans ASP.NET Core SignalR
author: tdykstra
description: Découvrez comment utiliser l’authentification et autorisation dans ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/security
ms.openlocfilehash: 98b5eb7be87920aacf7a941f76ff652ae7905303
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391256"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="bca28-103">Considérations de sécurité dans ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="bca28-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="bca28-104">Par [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="bca28-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

## <a name="overview"></a><span data-ttu-id="bca28-105">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="bca28-105">Overview</span></span>

<span data-ttu-id="bca28-106">SignalR fournit un nombre de protections de sécurité par défaut.</span><span class="sxs-lookup"><span data-stu-id="bca28-106">SignalR provides a number of security protections by default.</span></span> <span data-ttu-id="bca28-107">Il est important de comprendre comment configurer ces protections.</span><span class="sxs-lookup"><span data-stu-id="bca28-107">It's important to understand how to configure these protections.</span></span>

### <a name="cross-origin-resource-sharing"></a><span data-ttu-id="bca28-108">Partage des ressources cross-origin</span><span class="sxs-lookup"><span data-stu-id="bca28-108">Cross-origin resource sharing</span></span>

<span data-ttu-id="bca28-109">[Ressources cross-origin CORS (partage)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) peut être utilisé pour autoriser les connexions SignalR cross-origine dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="bca28-109">[Cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="bca28-110">Si votre code JavaScript est hébergé sur un nom de domaine différent à partir de votre application SignalR, vous devez activer le [intergiciel (middleware) ASP.NET Core CORS](xref:security/cors) afin de permettre la connexion.</span><span class="sxs-lookup"><span data-stu-id="bca28-110">If your JavaScript code is hosted on a different domain name from your SignalR app, you have to enable the [ASP.NET Core CORS middleware](xref:security/cors) in order to allow the connection.</span></span> <span data-ttu-id="bca28-111">En règle générale, autoriser les demandes cross-origin uniquement à partir de domaines que vous contrôlez.</span><span class="sxs-lookup"><span data-stu-id="bca28-111">In general, allow cross-origin requests only from domains you control.</span></span> <span data-ttu-id="bca28-112">Par exemple, si votre site est hébergé sur `http://www.example.com` et votre application SignalR est hébergée sur `http://signalr.example.com`, vous devez configurer CORS dans votre application de SignalR pour autoriser uniquement l’origine `www.example.com`.</span><span class="sxs-lookup"><span data-stu-id="bca28-112">For example, if your site is hosted on `http://www.example.com` and your SignalR app is hosted on `http://signalr.example.com`, you should configure CORS in your SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="bca28-113">Pour plus d’informations sur la configuration de CORS, consultez [la documentation sur ASP.NET Core CORS](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="bca28-113">For more information on configuring CORS, see [the documentation on ASP.NET Core CORS](xref:security/cors).</span></span> <span data-ttu-id="bca28-114">SignalR requiert que les stratégies CORS suivantes afin de fonctionner correctement :</span><span class="sxs-lookup"><span data-stu-id="bca28-114">SignalR requires the following CORS policies in order to operate correctly:</span></span>

* <span data-ttu-id="bca28-115">La stratégie doit autoriser les origines spécifiques que vous escomptez ou autoriser toute origine (non recommandé).</span><span class="sxs-lookup"><span data-stu-id="bca28-115">The policy must allow the specific origins you expect, or allow any origin (not recommended).</span></span>
* <span data-ttu-id="bca28-116">Méthodes HTTP `GET` et `POST` doivent être autorisés.</span><span class="sxs-lookup"><span data-stu-id="bca28-116">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="bca28-117">Informations d’identification doivent être activées, même lorsque vous n’utilisez pas l’authentification.</span><span class="sxs-lookup"><span data-stu-id="bca28-117">Credentials must be enabled, even when you aren't using authentication.</span></span>

<span data-ttu-id="bca28-118">Par exemple, la stratégie CORS suivante permet à un client de navigateur de SignalR hébergé sur `http://example.com` pour accéder à votre application SignalR :</span><span class="sxs-lookup"><span data-stu-id="bca28-118">For example, the following CORS policy allows a SignalR browser client hosted on `http://example.com` to access your SignalR app:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder => {
        builder.WithOrigins("http://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> <span data-ttu-id="bca28-119">SignalR n’est pas compatible avec la fonctionnalité CORS intégrée dans Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="bca28-119">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

### <a name="websocket-origin-restriction"></a><span data-ttu-id="bca28-120">Restriction de l’origine de WebSocket</span><span class="sxs-lookup"><span data-stu-id="bca28-120">WebSocket Origin Restriction</span></span>

<span data-ttu-id="bca28-121">Les protections fournies par CORS ne s’appliquent pas aux WebSockets.</span><span class="sxs-lookup"><span data-stu-id="bca28-121">The protections provided by CORS do not apply to WebSockets.</span></span> <span data-ttu-id="bca28-122">Navigateurs n’effectuent pas de demandes de contrôle préliminaire CORS, ni qu’ils respectent les restrictions spécifiées dans `Access-Control` en-têtes pour effectuer des demandes de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="bca28-122">Browsers do not perform CORS pre-flight requests, nor do they respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span> <span data-ttu-id="bca28-123">Toutefois, les navigateurs envoient le `Origin` en-tête lors de l’émission des demandes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="bca28-123">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="bca28-124">Vous devez configurer votre application pour valider ces en-têtes afin de garantir que seul WebSockets provenant les origines souhaitées sont autorisés.</span><span class="sxs-lookup"><span data-stu-id="bca28-124">You should configure your application to validate these headers in order to ensure that only WebSockets coming from the origins you expect are allowed.</span></span>

<span data-ttu-id="bca28-125">Dans ASP.NET Core 2.1, cela peut être effectuée à l’aide d’un intergiciel (middleware) personnalisé, vous pouvez placer **ci-dessus `UseSignalR`et tout intergiciel d’authentification** dans votre `Configure` méthode :</span><span class="sxs-lookup"><span data-stu-id="bca28-125">In ASP.NET Core 2.1, this can be achieved using a custom middleware you can place **above `UseSignalR`, and any authentication middleware** in your `Configure` method:</span></span>

```csharp
// In your Startup class, add a static field listing the allowed Origin values:
private static readonly HashSet<string> _allowedOrigins = new HashSet<string>()
{
    // Add allowed origins here. For example:
    "http://www.mysite.com",
    "http://mysite.com",
};

// In your Configure method:
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Validate Origin header on WebSocket requests to prevent unexpected cross-site WebSocket requests
    app.Use((context, next) =>
    {
        // Check for a WebSocket request.
        if(string.Equals(context.Request.Headers["Upgrade"], "websocket"))
        {
            var origin = context.Request.Headers["Origin"];

            // If there is no origin header, or if the origin header doesn't match an allowed value:
            if(string.IsNullOrEmpty(origin) && !_allowedOrigins.Contains(origin))
            {
                // The origin is not allowed, reject the request
                context.Response.StatusCode = StatusCodes.Status400BadRequest;
                return Task.CompletedTask;
            }
        }

        // The request is not a WebSocket request or is a valid Origin, so let it continue
        return next();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> <span data-ttu-id="bca28-126">Le `Origin` en-tête est entièrement contrôlé par le client et, comme le `Referer` en-tête, peut être fausses.</span><span class="sxs-lookup"><span data-stu-id="bca28-126">The `Origin` header is completely controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="bca28-127">Ces en-têtes ne doivent jamais être utilisés comme mécanisme d’authentification.</span><span class="sxs-lookup"><span data-stu-id="bca28-127">These headers should never be used as an authentication mechanism.</span></span>

### <a name="access-token-logging"></a><span data-ttu-id="bca28-128">Connexion de jeton d’accès</span><span class="sxs-lookup"><span data-stu-id="bca28-128">Access token logging</span></span>

<span data-ttu-id="bca28-129">Lorsque vous utilisez des WebSockets ou les événements, le navigateur client envoie le jeton d’accès dans la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="bca28-129">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="bca28-130">Il s’agit généralement aussi sécurisé qu’à l’aide de la norme `Authorization` en-tête, mais de nombreux serveurs web connecter à l’URL pour chaque demande, y compris la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="bca28-130">This is generally as secure as using the standard `Authorization` header, however many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="bca28-131">Cela signifie que le jeton d’accès peut être inclus dans les journaux.</span><span class="sxs-lookup"><span data-stu-id="bca28-131">This means the access token may be included in logs.</span></span> <span data-ttu-id="bca28-132">Examinez les paramètres de journalisation du serveur web pour éviter l’enregistrement de ces informations.</span><span class="sxs-lookup"><span data-stu-id="bca28-132">Consider reviewing the web server's logging settings to avoid logging this information.</span></span>

### <a name="exceptions"></a><span data-ttu-id="bca28-133">Exceptions</span><span class="sxs-lookup"><span data-stu-id="bca28-133">Exceptions</span></span>

<span data-ttu-id="bca28-134">Messages d’exception sont généralement considérés comme des données sensibles qui ne doivent pas être révélées à un client.</span><span class="sxs-lookup"><span data-stu-id="bca28-134">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="bca28-135">Par défaut, les détails d’une exception levée par une méthode de concentrateur au client n’envoie pas de SignalR.</span><span class="sxs-lookup"><span data-stu-id="bca28-135">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="bca28-136">Au lieu de cela, le client reçoit un message générique indiquant qu'une erreur s’est produite.</span><span class="sxs-lookup"><span data-stu-id="bca28-136">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="bca28-137">Vous pouvez substituer ce comportement en définissant le [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options) paramètre.</span><span class="sxs-lookup"><span data-stu-id="bca28-137">You can override this behavior by setting the [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options) setting.</span></span>

### <a name="buffer-management"></a><span data-ttu-id="bca28-138">Gestion des tampons</span><span class="sxs-lookup"><span data-stu-id="bca28-138">Buffer management</span></span>

<span data-ttu-id="bca28-139">SignalR utilise des mémoires tampons de chaque connexion afin de gérer les messages entrants et sortants.</span><span class="sxs-lookup"><span data-stu-id="bca28-139">SignalR uses per-connection buffers in order to manage incoming and outgoing messages.</span></span> <span data-ttu-id="bca28-140">Par défaut, SignalR limite ces mémoires tampons à 32 Ko.</span><span class="sxs-lookup"><span data-stu-id="bca28-140">By default, SignalR limits these buffers to 32KB.</span></span> <span data-ttu-id="bca28-141">Cela signifie que le plus grand message possible qu'un client ou le serveur peut envoyer est de 32 Ko.</span><span class="sxs-lookup"><span data-stu-id="bca28-141">This means the largest possible message a client or server can send is 32KB.</span></span> <span data-ttu-id="bca28-142">Cela signifie également la quantité maximale de mémoire consommée par une connexion pour les messages est 32 Ko.</span><span class="sxs-lookup"><span data-stu-id="bca28-142">This also means the maximum amount of memory consumed by a connection for messages is 32KB.</span></span> <span data-ttu-id="bca28-143">Si vous connaissez que vos messages sont toujours plus petits que cette limite, vous pouvez réduire cette taille pour empêcher un client capable d’envoyer un message plus volumineux et de forcer le serveur à allouer de la mémoire pour l’accepter.</span><span class="sxs-lookup"><span data-stu-id="bca28-143">If you know your messages are always smaller than this limit, you can reduce this size to prevent a client from being able to send a larger message and force the server to allocate memory to accept it.</span></span> <span data-ttu-id="bca28-144">De même, si vous connaissez que vos messages sont supérieures à cette limite, vous pouvez l’augmenter.</span><span class="sxs-lookup"><span data-stu-id="bca28-144">Similarly, if you know your messages are larger than this limit, you can increase it.</span></span> <span data-ttu-id="bca28-145">Toutefois, n’oubliez pas que si vous augmentez cette limite, que le client est en mesure de provoquer le serveur d’allocation de mémoire supplémentaire et peut réduire le nombre de connexions simultanées que votre application puisse les traiter.</span><span class="sxs-lookup"><span data-stu-id="bca28-145">However, be aware that increasing this limit means that the client is able to cause the server to allocate additional memory and may reduce the number of concurrent connections your app can handle.</span></span>

<span data-ttu-id="bca28-146">Il existe des limites distincts pour les messages entrants et sortants, les deux peuvent être configurées sur le [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) objet configuré dans `MapHub`:</span><span class="sxs-lookup"><span data-stu-id="bca28-146">There are separate limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="bca28-147">`ApplicationMaxBufferSize` représente le nombre maximal d’octets à partir du client qui les mémoires tampons de serveur.</span><span class="sxs-lookup"><span data-stu-id="bca28-147">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="bca28-148">Si le client tente d’envoyer un message dépasse cette limite, la connexion peut être fermée.</span><span class="sxs-lookup"><span data-stu-id="bca28-148">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="bca28-149">`TransportMaxBufferSize` représente le nombre maximal d’octets que le serveur peut envoyer.</span><span class="sxs-lookup"><span data-stu-id="bca28-149">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="bca28-150">Si le serveur tente d’envoyer un message (inclut les valeurs de retour des méthodes de concentrateur) dépasse cette limite, une exception sera levée.</span><span class="sxs-lookup"><span data-stu-id="bca28-150">If the server attempts to send a message (includes return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="bca28-151">Définition de la limite `0` permet de désactiver complètement la limite.</span><span class="sxs-lookup"><span data-stu-id="bca28-151">Setting the limit to `0` disables the limit entirely.</span></span> <span data-ttu-id="bca28-152">Toutefois, cela doit être fait avec une extrême prudence.</span><span class="sxs-lookup"><span data-stu-id="bca28-152">However, this should be done with extreme caution.</span></span> <span data-ttu-id="bca28-153">Suppression d’une limite permet à un client envoyer un message de n’importe quelle taille.</span><span class="sxs-lookup"><span data-stu-id="bca28-153">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="bca28-154">Il peut être utilisé par un client malveillant de provoquer l’excès de mémoire à allouer, ce qui pourrait réduire considérablement le nombre de connexions simultanées que votre application peut prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="bca28-154">This could be used by a malicious client to cause excess memory to be allocated, which could dramatically reduce the number of concurrent connections your app can support.</span></span>
