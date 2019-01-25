---
title: Introduction à ASP.NET Core SignalR
author: bradygaster
description: Découvrez comment la bibliothèque ASP.NET Core SignalR simplifie l’ajout de fonctionnalités en temps réel aux applications.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: 673efafce60dfa46cb99f9537fda2bca42bf9822
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836361"
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="566b1-103">Introduction à ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="566b1-103">Introduction to ASP.NET Core SignalR</span></span>

## <a name="what-is-signalr"></a><span data-ttu-id="566b1-104">Nouveautés SignalR</span><span class="sxs-lookup"><span data-stu-id="566b1-104">What is SignalR?</span></span>

<span data-ttu-id="566b1-105">ASP.NET Core SignalR est une bibliothèque open source qui simplifie l’ajout de fonctionnalités web en temps réel aux applications.</span><span class="sxs-lookup"><span data-stu-id="566b1-105">ASP.NET Core SignalR is an open-source library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="566b1-106">Les fonctionnalités web en temps réel permettent au code côté serveur de transmettre le contenu aux clients instantanément.</span><span class="sxs-lookup"><span data-stu-id="566b1-106">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="566b1-107">De bons candidats pour SignalR sont :</span><span class="sxs-lookup"><span data-stu-id="566b1-107">Good candidates for SignalR:</span></span>

* <span data-ttu-id="566b1-108">Des applications qui nécessitent des mises à jour à haute fréquence à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="566b1-108">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="566b1-109">Des exemples sont des applications de jeux, de réseaux sociaux, de vote, de vente aux enchères, de cartographie et de géolocalisation.</span><span class="sxs-lookup"><span data-stu-id="566b1-109">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="566b1-110">Des tableaux de bord et des applications de surveillance.</span><span class="sxs-lookup"><span data-stu-id="566b1-110">Dashboards and monitoring apps.</span></span> <span data-ttu-id="566b1-111">Des exemples incluent des tableaux de bord, des mises à jour de ventes instantanées, ou des alertes de voyage.</span><span class="sxs-lookup"><span data-stu-id="566b1-111">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="566b1-112">Des applications de collaboration.</span><span class="sxs-lookup"><span data-stu-id="566b1-112">Collaborative apps.</span></span> <span data-ttu-id="566b1-113">Des applications de tableau blanc et des logiciels de réunion d’équipe sont des exemples d’applications de collaboration.</span><span class="sxs-lookup"><span data-stu-id="566b1-113">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="566b1-114">Des applications qui nécessitent des notifications.</span><span class="sxs-lookup"><span data-stu-id="566b1-114">Apps that require notifications.</span></span> <span data-ttu-id="566b1-115">Les applications de réseaux sociaux, e-mail, chat, jeux, d'alertes de voyage et de nombreuses autres applications utilisent des notifications.</span><span class="sxs-lookup"><span data-stu-id="566b1-115">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="566b1-116">SignalR fournit une API pour la création [d'appels de procédure distante (RPC)](https://wikipedia.org/wiki/Remote_procedure_call) client-serveur.</span><span class="sxs-lookup"><span data-stu-id="566b1-116">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="566b1-117">Les appels RPC appellent des fonctions JavaScript sur les clients à partir de code côté serveur en .NET Core.</span><span class="sxs-lookup"><span data-stu-id="566b1-117">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="566b1-118">Voici certaines fonctionnalités de SignalR pour ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="566b1-118">Here are some features of SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="566b1-119">Gestion automatique des connexions.</span><span class="sxs-lookup"><span data-stu-id="566b1-119">Handles connection management automatically.</span></span>
* <span data-ttu-id="566b1-120">Envoi de messages à tous les clients connectés simultanément.</span><span class="sxs-lookup"><span data-stu-id="566b1-120">Sends messages to all connected clients simultaneously.</span></span> <span data-ttu-id="566b1-121">Par exemple, une salle de conversation.</span><span class="sxs-lookup"><span data-stu-id="566b1-121">For example, a chat room.</span></span>
* <span data-ttu-id="566b1-122">Envoi de messages à des clients spécifiques ou des groupes de clients.</span><span class="sxs-lookup"><span data-stu-id="566b1-122">Sends messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="566b1-123">Gestion de la montée en charge lors de l'augmentation de trafic.</span><span class="sxs-lookup"><span data-stu-id="566b1-123">Scales to handle increasing traffic.</span></span>

<span data-ttu-id="566b1-124">La source est hébergée dans un [dépôt SignalR sur GitHub](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).</span><span class="sxs-lookup"><span data-stu-id="566b1-124">The source is hosted in a [SignalR repository on GitHub](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).</span></span>

## <a name="transports"></a><span data-ttu-id="566b1-125">Transports</span><span class="sxs-lookup"><span data-stu-id="566b1-125">Transports</span></span>

<span data-ttu-id="566b1-126">SignalR prend en charge plusieurs techniques pour la gestion des communications en temps réel :</span><span class="sxs-lookup"><span data-stu-id="566b1-126">SignalR supports several techniques for handling real-time communications:</span></span>

* [<span data-ttu-id="566b1-127">WebSockets</span><span class="sxs-lookup"><span data-stu-id="566b1-127">WebSockets</span></span>](https://tools.ietf.org/html/rfc7118)
* <span data-ttu-id="566b1-128">Événements envoyés par le serveur</span><span class="sxs-lookup"><span data-stu-id="566b1-128">Server-Sent Events</span></span>
* <span data-ttu-id="566b1-129">Interrogation longue (Long polling)</span><span class="sxs-lookup"><span data-stu-id="566b1-129">Long Polling</span></span>

<span data-ttu-id="566b1-130">SignalR choisit automatiquement la meilleure méthode de transport qui se trouve dans les fonctionnalités du serveur et client.</span><span class="sxs-lookup"><span data-stu-id="566b1-130">SignalR automatically chooses the best transport method that is within the capabilities of the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="566b1-131">Hubs</span><span class="sxs-lookup"><span data-stu-id="566b1-131">Hubs</span></span>

<span data-ttu-id="566b1-132">SignalR utilise des *hubs* pour communiquer entre les clients et serveurs.</span><span class="sxs-lookup"><span data-stu-id="566b1-132">SignalR uses *hubs* to communicate between clients and servers.</span></span>

<span data-ttu-id="566b1-133">Un hub est un pipeline de haut niveau qui permet à un client et au serveur d'appeler des méthodes entre eux.</span><span class="sxs-lookup"><span data-stu-id="566b1-133">A hub is a high-level pipeline that allows a client and server to call methods on each other.</span></span> <span data-ttu-id="566b1-134">SignalR gère la distribution au-delà des limites de machine automatiquement, autorisant les clients à appeler des méthodes sur le serveur et vice versa.</span><span class="sxs-lookup"><span data-stu-id="566b1-134">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server and vice versa.</span></span> <span data-ttu-id="566b1-135">Vous pouvez passer des paramètres fortement typés aux méthodes, ce qui permet la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="566b1-135">You can pass strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="566b1-136">SignalR fournit deux protocoles hub intégrés : un protocole de texte basé sur JSON et un protocole binaire basé sur [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="566b1-136">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="566b1-137">MessagePack crée généralement des messages plus petits par rapport au format JSON.</span><span class="sxs-lookup"><span data-stu-id="566b1-137">MessagePack generally creates smaller messages compared to JSON.</span></span> <span data-ttu-id="566b1-138">Les navigateurs plus anciens doivent prendre en charge [niveau XHR 2](https://caniuse.com/#feat=xhr2) pour prendre en charge le protocole MessagePack.</span><span class="sxs-lookup"><span data-stu-id="566b1-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="566b1-139">Les hubs appellent le code côté client en envoyant des messages qui contiennent le nom et les paramètres de la méthode côté client.</span><span class="sxs-lookup"><span data-stu-id="566b1-139">Hubs call client-side code by sending messages that contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="566b1-140">Les objets envoyés comme paramètres de méthode sont désérialisés en utilisant le protocole configuré.</span><span class="sxs-lookup"><span data-stu-id="566b1-140">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="566b1-141">Le client essaie de faire correspondre le nom à une méthode dans le code côté client.</span><span class="sxs-lookup"><span data-stu-id="566b1-141">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="566b1-142">Lorsque le client trouve une correspondance, il appelle la méthode et lui passe les données de paramètre désérialisées.</span><span class="sxs-lookup"><span data-stu-id="566b1-142">When the client finds a match, it calls the method and passes to it the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="566b1-143">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="566b1-143">Additional resources</span></span>

* [<span data-ttu-id="566b1-144">Bien démarrer avec SignalR pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="566b1-144">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="566b1-145">Plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="566b1-145">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="566b1-146">Hubs</span><span class="sxs-lookup"><span data-stu-id="566b1-146">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="566b1-147">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="566b1-147">JavaScript client</span></span>](xref:signalr/javascript-client)
