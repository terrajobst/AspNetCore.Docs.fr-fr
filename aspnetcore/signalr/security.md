---
title: Considérations de sécurité dans ASP.NET Core SignalR
author: rachelappel
description: Découvrez comment utiliser l’authentification et autorisation dans ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/security
ms.openlocfilehash: eff4542b88f24dd6c1c0675f56874e368d441fdd
ms.sourcegitcommit: 32626efaa7316c9b283c96be6516e637d548c5e5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39028481"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="56d43-103">Considérations de sécurité dans ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="56d43-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="56d43-104">Par [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="56d43-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

## <a name="overview"></a><span data-ttu-id="56d43-105">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="56d43-105">Overview</span></span>

<span data-ttu-id="56d43-106">SignalR fournit un nombre de protections de sécurité par défaut.</span><span class="sxs-lookup"><span data-stu-id="56d43-106">SignalR provides a number of security protections by default.</span></span> <span data-ttu-id="56d43-107">Il est important de comprendre comment configurer ces protections.</span><span class="sxs-lookup"><span data-stu-id="56d43-107">It's important to understand how to configure these protections.</span></span>

### <a name="cross-origin-resource-sharing"></a><span data-ttu-id="56d43-108">Partage des ressources cross-origin</span><span class="sxs-lookup"><span data-stu-id="56d43-108">Cross-origin resource sharing</span></span>

<span data-ttu-id="56d43-109">[Ressources cross-origin CORS (partage)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) peut être utilisé pour autoriser les connexions SignalR cross-origine dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="56d43-109">[Cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="56d43-110">Si votre code JavaScript est hébergé sur un nom de domaine différent à partir de votre application SignalR, vous devez activer le [intergiciel (middleware) ASP.NET Core CORS](xref:security/cors) afin de permettre la connexion.</span><span class="sxs-lookup"><span data-stu-id="56d43-110">If your JavaScript code is hosted on a different domain name from your SignalR app, you have to enable the [ASP.NET Core CORS middleware](xref:security/cors) in order to allow the connection.</span></span> <span data-ttu-id="56d43-111">En règle générale, autoriser les demandes cross-origin uniquement à partir de domaines que vous contrôlez.</span><span class="sxs-lookup"><span data-stu-id="56d43-111">In general, allow cross-origin requests only from domains you control.</span></span> <span data-ttu-id="56d43-112">Par exemple, si votre site est hébergé sur `http://www.example.com` et votre application SignalR est hébergée sur `http://signalr.example.com`, vous devez configurer CORS dans votre application de SignalR pour autoriser uniquement l’origine `www.example.com`.</span><span class="sxs-lookup"><span data-stu-id="56d43-112">For example, if your site is hosted on `http://www.example.com` and your SignalR app is hosted on `http://signalr.example.com`, you should configure CORS in your SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="56d43-113">Pour plus d’informations sur la configuration de CORS, consultez [la documentation sur ASP.NET Core CORS](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="56d43-113">For more information on configuring CORS, see [the documentation on ASP.NET Core CORS](xref:security/cors).</span></span> <span data-ttu-id="56d43-114">SignalR requiert que les stratégies CORS suivantes afin de fonctionner correctement :</span><span class="sxs-lookup"><span data-stu-id="56d43-114">SignalR requires the following CORS policies in order to operate correctly:</span></span>

* <span data-ttu-id="56d43-115">La stratégie doit autoriser les origines spécifiques que vous escomptez ou autoriser toute origine (non recommandé).</span><span class="sxs-lookup"><span data-stu-id="56d43-115">The policy must allow the specific origins you expect, or allow any origin (not recommended).</span></span>
* <span data-ttu-id="56d43-116">Méthodes HTTP `GET` et `POST` doivent être autorisés.</span><span class="sxs-lookup"><span data-stu-id="56d43-116">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="56d43-117">Informations d’identification doivent être activées, même lorsque vous n’utilisez pas l’authentification.</span><span class="sxs-lookup"><span data-stu-id="56d43-117">Credentials must be enabled, even when you aren't using authentication.</span></span>

<span data-ttu-id="56d43-118">Par exemple, la stratégie CORS suivante permet à un client de navigateur de SignalR hébergé sur `http://example.com` pour accéder à votre application SignalR :</span><span class="sxs-lookup"><span data-stu-id="56d43-118">For example, the following CORS policy allows a SignalR browser client hosted on `http://example.com` to access your SignalR app:</span></span>

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
> <span data-ttu-id="56d43-119">SignalR n’est pas compatible avec la fonctionnalité CORS intégrée dans Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="56d43-119">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

### <a name="access-token-logging"></a><span data-ttu-id="56d43-120">Connexion de jeton d’accès</span><span class="sxs-lookup"><span data-stu-id="56d43-120">Access token logging</span></span>

<span data-ttu-id="56d43-121">Lorsque vous utilisez des WebSockets ou les événements, le navigateur client envoie le jeton d’accès dans la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="56d43-121">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="56d43-122">Il s’agit généralement aussi sécurisé qu’à l’aide de la norme `Authorization` en-tête, mais de nombreux serveurs web connecter à l’URL pour chaque demande, y compris la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="56d43-122">This is generally as secure as using the standard `Authorization` header, however many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="56d43-123">Cela signifie que le jeton d’accès peut être inclus dans les journaux.</span><span class="sxs-lookup"><span data-stu-id="56d43-123">This means the access token may be included in logs.</span></span> <span data-ttu-id="56d43-124">Examinez les paramètres de journalisation du serveur web pour éviter l’enregistrement de ces informations.</span><span class="sxs-lookup"><span data-stu-id="56d43-124">Consider reviewing the web server's logging settings to avoid logging this information.</span></span>

### <a name="exceptions"></a><span data-ttu-id="56d43-125">Exceptions</span><span class="sxs-lookup"><span data-stu-id="56d43-125">Exceptions</span></span>

<span data-ttu-id="56d43-126">Messages d’exception sont généralement considérés comme des données sensibles qui ne doivent pas être révélées à un client.</span><span class="sxs-lookup"><span data-stu-id="56d43-126">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="56d43-127">Par défaut, les détails d’une exception levée par une méthode de concentrateur au client n’envoie pas de SignalR.</span><span class="sxs-lookup"><span data-stu-id="56d43-127">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="56d43-128">Au lieu de cela, le client reçoit un message générique indiquant qu'une erreur s’est produite.</span><span class="sxs-lookup"><span data-stu-id="56d43-128">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="56d43-129">Vous pouvez substituer ce comportement en définissant le [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options) paramètre.</span><span class="sxs-lookup"><span data-stu-id="56d43-129">You can override this behavior by setting the [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options) setting.</span></span>

### <a name="buffer-management"></a><span data-ttu-id="56d43-130">Gestion des tampons</span><span class="sxs-lookup"><span data-stu-id="56d43-130">Buffer management</span></span>

<span data-ttu-id="56d43-131">SignalR utilise des mémoires tampons de chaque connexion afin de gérer les messages entrants et sortants.</span><span class="sxs-lookup"><span data-stu-id="56d43-131">SignalR uses per-connection buffers in order to manage incoming and outgoing messages.</span></span> <span data-ttu-id="56d43-132">Par défaut, SignalR limite ces mémoires tampons à 32 Ko.</span><span class="sxs-lookup"><span data-stu-id="56d43-132">By default, SignalR limits these buffers to 32KB.</span></span> <span data-ttu-id="56d43-133">Cela signifie que le plus grand message possible qu'un client ou le serveur peut envoyer est de 32 Ko.</span><span class="sxs-lookup"><span data-stu-id="56d43-133">This means the largest possible message a client or server can send is 32KB.</span></span> <span data-ttu-id="56d43-134">Cela signifie également la quantité maximale de mémoire consommée par une connexion pour les messages est 32 Ko.</span><span class="sxs-lookup"><span data-stu-id="56d43-134">This also means the maximum amount of memory consumed by a connection for messages is 32KB.</span></span> <span data-ttu-id="56d43-135">Si vous connaissez que vos messages sont toujours plus petits que cette limite, vous pouvez réduire cette taille pour empêcher un client capable d’envoyer un message plus volumineux et de forcer le serveur à allouer de la mémoire pour l’accepter.</span><span class="sxs-lookup"><span data-stu-id="56d43-135">If you know your messages are always smaller than this limit, you can reduce this size to prevent a client from being able to send a larger message and force the server to allocate memory to accept it.</span></span> <span data-ttu-id="56d43-136">De même, si vous connaissez que vos messages sont supérieures à cette limite, vous pouvez l’augmenter.</span><span class="sxs-lookup"><span data-stu-id="56d43-136">Similarly, if you know your messages are larger than this limit, you can increase it.</span></span> <span data-ttu-id="56d43-137">Toutefois, n’oubliez pas que si vous augmentez cette limite, que le client est en mesure de provoquer le serveur d’allocation de mémoire supplémentaire et peut réduire le nombre de connexions simultanées que votre application puisse les traiter.</span><span class="sxs-lookup"><span data-stu-id="56d43-137">However, be aware that increasing this limit means that the client is able to cause the server to allocate additional memory and may reduce the number of concurrent connections your app can handle.</span></span>

<span data-ttu-id="56d43-138">Il existe des limites distincts pour les messages entrants et sortants, les deux peuvent être configurées sur le [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) objet configuré dans `MapHub`:</span><span class="sxs-lookup"><span data-stu-id="56d43-138">There are separate limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="56d43-139">`ApplicationMaxBufferSize` représente le nombre maximal d’octets à partir du client qui les mémoires tampons de serveur.</span><span class="sxs-lookup"><span data-stu-id="56d43-139">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="56d43-140">Si le client tente d’envoyer un message dépasse cette limite, la connexion peut être fermée.</span><span class="sxs-lookup"><span data-stu-id="56d43-140">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="56d43-141">`TransportMaxBufferSize` représente le nombre maximal d’octets que le serveur peut envoyer.</span><span class="sxs-lookup"><span data-stu-id="56d43-141">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="56d43-142">Si le serveur tente d’envoyer un message (inclut les valeurs de retour des méthodes de concentrateur) dépasse cette limite, une exception sera levée.</span><span class="sxs-lookup"><span data-stu-id="56d43-142">If the server attempts to send a message (includes return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="56d43-143">Définition de la limite `0` permet de désactiver complètement la limite.</span><span class="sxs-lookup"><span data-stu-id="56d43-143">Setting the limit to `0` disables the limit entirely.</span></span> <span data-ttu-id="56d43-144">Toutefois, cela doit être fait avec une extrême prudence.</span><span class="sxs-lookup"><span data-stu-id="56d43-144">However, this should be done with extreme caution.</span></span> <span data-ttu-id="56d43-145">Suppression d’une limite permet à un client envoyer un message de n’importe quelle taille.</span><span class="sxs-lookup"><span data-stu-id="56d43-145">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="56d43-146">Il peut être utilisé par un client malveillant de provoquer l’excès de mémoire à allouer, ce qui pourrait réduire considérablement le nombre de connexions simultanées que votre application peut prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="56d43-146">This could be used by a malicious client to cause excess memory to be allocated, which could dramatically reduce the number of concurrent connections your app can support.</span></span>
