---
title: Fonctionnalités du client SignalR
author: bradygaster
description: Découvrez les fonctionnalités prises en charge par les différents clients ASP.NET Core Signalr.
monikerRange: '>= aspnetcore-3.0'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 2d6759a5484c37aee6db3d22b3127414231605ae
ms.sourcegitcommit: 14b25156e34c82ed0495b4aff5776ac5b1950b5e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71301189"
---
# <a name="aspnet-core-signalr-client-features"></a><span data-ttu-id="9fbe6-103">Fonctionnalités du client ASP.NET Core Signalr</span><span class="sxs-lookup"><span data-stu-id="9fbe6-103">ASP.NET Core SignalR client features</span></span>

## <a name="feature-distribution"></a><span data-ttu-id="9fbe6-104">Distribution des fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="9fbe6-104">Feature distribution</span></span>

<span data-ttu-id="9fbe6-105">Le tableau ci-dessous présente les fonctionnalités et la prise en charge des clients qui offrent une prise en charge en temps réel.</span><span class="sxs-lookup"><span data-stu-id="9fbe6-105">The table below shows the features and support for the clients that offer real-time support.</span></span>

| <span data-ttu-id="9fbe6-106">Fonctionnalité</span><span class="sxs-lookup"><span data-stu-id="9fbe6-106">Feature</span></span> | <span data-ttu-id="9fbe6-107">.NET</span><span class="sxs-lookup"><span data-stu-id="9fbe6-107">.NET</span></span> | <span data-ttu-id="9fbe6-108">JavaScript</span><span class="sxs-lookup"><span data-stu-id="9fbe6-108">JavaScript</span></span> | <span data-ttu-id="9fbe6-109">Java</span><span class="sxs-lookup"><span data-stu-id="9fbe6-109">Java</span></span> |
| ---- | :-: | :-: | :-: |
| <span data-ttu-id="9fbe6-110">Prise en charge du service Azure Signalr</span><span class="sxs-lookup"><span data-stu-id="9fbe6-110">Azure SignalR Service Support</span></span> |<span data-ttu-id="9fbe6-111">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-111">✔</span></span>|<span data-ttu-id="9fbe6-112">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-112">✔</span></span>|<span data-ttu-id="9fbe6-113">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-113">✔</span></span>|
| [<span data-ttu-id="9fbe6-114">Streaming de serveur à client</span><span class="sxs-lookup"><span data-stu-id="9fbe6-114">Server-to-client Streaming</span></span>](xref:signalr/streaming)          |<span data-ttu-id="9fbe6-115">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-115">✔</span></span>|<span data-ttu-id="9fbe6-116">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-116">✔</span></span>|<span data-ttu-id="9fbe6-117">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-117">✔</span></span>|
| [<span data-ttu-id="9fbe6-118">Streaming client à serveur</span><span class="sxs-lookup"><span data-stu-id="9fbe6-118">Client-to-server Streaming</span></span>](xref:signalr/streaming)          |<span data-ttu-id="9fbe6-119">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-119">✔</span></span>|<span data-ttu-id="9fbe6-120">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-120">✔</span></span>|<span data-ttu-id="9fbe6-121">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-121">✔</span></span>|
| <span data-ttu-id="9fbe6-122">Reconnexion automatique ([.net](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))</span><span class="sxs-lookup"><span data-stu-id="9fbe6-122">Automatic Reconnection ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))</span></span>          |<span data-ttu-id="9fbe6-123">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-123">✔</span></span>|<span data-ttu-id="9fbe6-124">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-124">✔</span></span>| |
| <span data-ttu-id="9fbe6-125">Transport WebSockets</span><span class="sxs-lookup"><span data-stu-id="9fbe6-125">WebSockets Transport</span></span> |<span data-ttu-id="9fbe6-126">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-126">✔</span></span>|<span data-ttu-id="9fbe6-127">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-127">✔</span></span>|<span data-ttu-id="9fbe6-128">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-128">✔</span></span>|
| <span data-ttu-id="9fbe6-129">Transport des événements envoyés par le serveur</span><span class="sxs-lookup"><span data-stu-id="9fbe6-129">Server-Sent Events Transport</span></span> |<span data-ttu-id="9fbe6-130">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-130">✔</span></span>|<span data-ttu-id="9fbe6-131">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-131">✔</span></span>| |
| <span data-ttu-id="9fbe6-132">Transport d’interrogation longue</span><span class="sxs-lookup"><span data-stu-id="9fbe6-132">Long Polling Transport</span></span> |<span data-ttu-id="9fbe6-133">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-133">✔</span></span>|<span data-ttu-id="9fbe6-134">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-134">✔</span></span>|<span data-ttu-id="9fbe6-135">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-135">✔</span></span>|
| <span data-ttu-id="9fbe6-136">Protocole JSON Hub</span><span class="sxs-lookup"><span data-stu-id="9fbe6-136">JSON Hub Protocol</span></span> |<span data-ttu-id="9fbe6-137">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-137">✔</span></span>|<span data-ttu-id="9fbe6-138">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-138">✔</span></span>|<span data-ttu-id="9fbe6-139">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-139">✔</span></span>|
| <span data-ttu-id="9fbe6-140">Protocole MessagePack Hub</span><span class="sxs-lookup"><span data-stu-id="9fbe6-140">MessagePack Hub Protocol</span></span> |<span data-ttu-id="9fbe6-141">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-141">✔</span></span>|<span data-ttu-id="9fbe6-142">✔</span><span class="sxs-lookup"><span data-stu-id="9fbe6-142">✔</span></span>| |

<span data-ttu-id="9fbe6-143">La prise en charge de la reconnexion automatique dans le client Java est suivie dans notre suivi des [problèmes](https://github.com/aspnet/AspNetCore/issues/8711).</span><span class="sxs-lookup"><span data-stu-id="9fbe6-143">Support for automatic reconnect in the Java client is tracked in [our issue tracker](https://github.com/aspnet/AspNetCore/issues/8711).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9fbe6-144">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9fbe6-144">Additional resources</span></span>

* [<span data-ttu-id="9fbe6-145">Prise en main de Signalr pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9fbe6-145">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="9fbe6-146">Plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="9fbe6-146">Supported platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="9fbe6-147">Hubs</span><span class="sxs-lookup"><span data-stu-id="9fbe6-147">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="9fbe6-148">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="9fbe6-148">JavaScript client</span></span>](xref:signalr/javascript-client)
