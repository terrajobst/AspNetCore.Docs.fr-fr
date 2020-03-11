---
title: Présentation de ASP.NET Core SignalR
author: bradygaster
description: Découvrez comment la bibliothèque de SignalR ASP.NET Core simplifie l’ajout de fonctionnalités en temps réel aux applications.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/27/2019
no-loc:
- SignalR
uid: signalr/introduction
ms.openlocfilehash: 635431abf9263c2dff261aea47e6f8324061763f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662917"
---
# <a name="introduction-to-aspnet-core-opno-locsignalr"></a><span data-ttu-id="dc37d-103">Présentation de ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="dc37d-103">Introduction to ASP.NET Core SignalR</span></span>

## <a name="what-is-opno-locsignalr"></a><span data-ttu-id="dc37d-104">Qu’est-ce que SignalR?</span><span class="sxs-lookup"><span data-stu-id="dc37d-104">What is SignalR?</span></span>

<span data-ttu-id="dc37d-105">ASP.NET Core SignalR est une bibliothèque open source qui simplifie l’ajout de fonctionnalités Web en temps réel aux applications.</span><span class="sxs-lookup"><span data-stu-id="dc37d-105">ASP.NET Core SignalR is an open-source library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="dc37d-106">Les fonctionnalités web en temps réel permettent au code côté serveur de transmettre le contenu aux clients instantanément.</span><span class="sxs-lookup"><span data-stu-id="dc37d-106">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="dc37d-107">Bons candidats pour SignalR:</span><span class="sxs-lookup"><span data-stu-id="dc37d-107">Good candidates for SignalR:</span></span>

* <span data-ttu-id="dc37d-108">Les applications ayant besoin de mises à jour fréquentes auprès du serveur.</span><span class="sxs-lookup"><span data-stu-id="dc37d-108">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="dc37d-109">Exemples : jeux, réseaux sociaux, scrutin, enchères, cartes et applications GPS.</span><span class="sxs-lookup"><span data-stu-id="dc37d-109">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="dc37d-110">Les tableaux de bord et les applications de monitoring.</span><span class="sxs-lookup"><span data-stu-id="dc37d-110">Dashboards and monitoring apps.</span></span> <span data-ttu-id="dc37d-111">Exemples : tableaux de bord des entreprises, mises à jour instantanées des ventes et alertes de voyage.</span><span class="sxs-lookup"><span data-stu-id="dc37d-111">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="dc37d-112">Les applications de collaboration.</span><span class="sxs-lookup"><span data-stu-id="dc37d-112">Collaborative apps.</span></span> <span data-ttu-id="dc37d-113">Exemples : applications de tableau blanc et logiciels de réunion d’équipe.</span><span class="sxs-lookup"><span data-stu-id="dc37d-113">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="dc37d-114">Les applications qui envoient des notifications.</span><span class="sxs-lookup"><span data-stu-id="dc37d-114">Apps that require notifications.</span></span> <span data-ttu-id="dc37d-115">Exemples : réseaux sociaux, messagerie, conversation instantanée, jeux, alertes de voyage, etc.</span><span class="sxs-lookup"><span data-stu-id="dc37d-115">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

SignalR<span data-ttu-id="dc37d-116"> fournit une API pour la création d' [appels de procédure distante (RPC)](https://wikipedia.org/wiki/Remote_procedure_call)de serveur à client.</span><span class="sxs-lookup"><span data-stu-id="dc37d-116"> provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="dc37d-117">Les appels RPC appellent des fonctions JavaScript sur les clients à partir de code côté serveur en .NET Core.</span><span class="sxs-lookup"><span data-stu-id="dc37d-117">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="dc37d-118">Voici certaines fonctionnalités de SignalR pour ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="dc37d-118">Here are some features of SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="dc37d-119">Gestion automatique des connexions.</span><span class="sxs-lookup"><span data-stu-id="dc37d-119">Handles connection management automatically.</span></span>
* <span data-ttu-id="dc37d-120">Envoi de messages à tous les clients connectés simultanément.</span><span class="sxs-lookup"><span data-stu-id="dc37d-120">Sends messages to all connected clients simultaneously.</span></span> <span data-ttu-id="dc37d-121">Par exemple, une salle de conversation.</span><span class="sxs-lookup"><span data-stu-id="dc37d-121">For example, a chat room.</span></span>
* <span data-ttu-id="dc37d-122">Envoi de messages à des clients spécifiques ou des groupes de clients.</span><span class="sxs-lookup"><span data-stu-id="dc37d-122">Sends messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="dc37d-123">Gestion de la montée en charge lors de l'augmentation de trafic.</span><span class="sxs-lookup"><span data-stu-id="dc37d-123">Scales to handle increasing traffic.</span></span>

<span data-ttu-id="dc37d-124">La source est hébergée dans un [référentielSignalR sur GitHub](https://github.com/dotnet/AspNetCore/tree/master/src/SignalR).</span><span class="sxs-lookup"><span data-stu-id="dc37d-124">The source is hosted in a [SignalR repository on GitHub](https://github.com/dotnet/AspNetCore/tree/master/src/SignalR).</span></span>

## <a name="transports"></a><span data-ttu-id="dc37d-125">Transports</span><span class="sxs-lookup"><span data-stu-id="dc37d-125">Transports</span></span>

SignalR<span data-ttu-id="dc37d-126"> prend en charge les techniques suivantes pour gérer les communications en temps réel (par ordre de secours normal) :</span><span class="sxs-lookup"><span data-stu-id="dc37d-126"> supports the following techniques for handling real-time communication (in order of graceful fallback):</span></span>

* [<span data-ttu-id="dc37d-127">WebSockets</span><span class="sxs-lookup"><span data-stu-id="dc37d-127">WebSockets</span></span>](https://tools.ietf.org/html/rfc7118)
* <span data-ttu-id="dc37d-128">Événements envoyés par le serveur</span><span class="sxs-lookup"><span data-stu-id="dc37d-128">Server-Sent Events</span></span>
* <span data-ttu-id="dc37d-129">Interrogation longue (Long polling)</span><span class="sxs-lookup"><span data-stu-id="dc37d-129">Long Polling</span></span>

SignalR<span data-ttu-id="dc37d-130"> choisit automatiquement la meilleure méthode de transport parmi les capacités du serveur et du client.</span><span class="sxs-lookup"><span data-stu-id="dc37d-130"> automatically chooses the best transport method that is within the capabilities of the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="dc37d-131">Hubs</span><span class="sxs-lookup"><span data-stu-id="dc37d-131">Hubs</span></span>

SignalR<span data-ttu-id="dc37d-132"> utilise des *hubs* pour la communication entre les clients et les serveurs.</span><span class="sxs-lookup"><span data-stu-id="dc37d-132"> uses *hubs* to communicate between clients and servers.</span></span>

<span data-ttu-id="dc37d-133">Un hub est un pipeline de haut niveau qui permet à un client et au serveur d'appeler des méthodes entre eux.</span><span class="sxs-lookup"><span data-stu-id="dc37d-133">A hub is a high-level pipeline that allows a client and server to call methods on each other.</span></span> SignalR<span data-ttu-id="dc37d-134"> gère automatiquement la distribution à travers les limites de l’ordinateur, ce qui permet aux clients d’appeler des méthodes sur le serveur et vice versa.</span><span class="sxs-lookup"><span data-stu-id="dc37d-134"> handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server and vice versa.</span></span> <span data-ttu-id="dc37d-135">Vous pouvez passer des paramètres fortement typés à des méthodes, ce qui active la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="dc37d-135">You can pass strongly-typed parameters to methods, which enables model binding.</span></span> SignalR<span data-ttu-id="dc37d-136"> fournit deux protocoles Hub intégrés : un protocole texte basé sur JSON et un protocole binaire basé sur [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="dc37d-136"> provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="dc37d-137">MessagePack crée généralement des messages plus petits par rapport à JSON.</span><span class="sxs-lookup"><span data-stu-id="dc37d-137">MessagePack generally creates smaller messages compared to JSON.</span></span> <span data-ttu-id="dc37d-138">Les anciens navigateurs doivent prendre en charge le [niveau 2 de XHR](https://caniuse.com/#feat=xhr2) pour fournir la prise en charge du protocole MessagePack.</span><span class="sxs-lookup"><span data-stu-id="dc37d-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="dc37d-139">Les hubs appellent le code côté client en envoyant des messages qui contiennent le nom et les paramètres de la méthode côté client.</span><span class="sxs-lookup"><span data-stu-id="dc37d-139">Hubs call client-side code by sending messages that contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="dc37d-140">Les objets envoyés comme paramètres de méthode sont désérialisés en utilisant le protocole configuré.</span><span class="sxs-lookup"><span data-stu-id="dc37d-140">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="dc37d-141">Le client essaie de faire correspondre le nom à une méthode dans le code côté client.</span><span class="sxs-lookup"><span data-stu-id="dc37d-141">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="dc37d-142">Lorsque le client trouve une correspondance, il appelle la méthode et lui passe les données de paramètre désérialisées.</span><span class="sxs-lookup"><span data-stu-id="dc37d-142">When the client finds a match, it calls the method and passes to it the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dc37d-143">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="dc37d-143">Additional resources</span></span>

* <span data-ttu-id="dc37d-144">[Prise en main de SignalR pour ASP.NET Core](xref:tutorials/signalr)</span><span class="sxs-lookup"><span data-stu-id="dc37d-144">[Get started with SignalR for ASP.NET Core](xref:tutorials/signalr)</span></span>
* [<span data-ttu-id="dc37d-145">Plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="dc37d-145">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="dc37d-146">Hubs</span><span class="sxs-lookup"><span data-stu-id="dc37d-146">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="dc37d-147">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="dc37d-147">JavaScript client</span></span>](xref:signalr/javascript-client)
