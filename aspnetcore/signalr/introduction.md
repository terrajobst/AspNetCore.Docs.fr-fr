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
ms.openlocfilehash: e84dd0d086cbfc80a80bc10baa33979da9b5d137
ms.sourcegitcommit: 3b6b0a54b20dc99b0c8c5978400c60adf431072f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74717232"
---
# <a name="introduction-to-aspnet-core-opno-locsignalr"></a><span data-ttu-id="0ed96-103">Présentation de ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="0ed96-103">Introduction to ASP.NET Core SignalR</span></span>

## <a name="what-is-opno-locsignalr"></a><span data-ttu-id="0ed96-104">Qu’est-ce que SignalR?</span><span class="sxs-lookup"><span data-stu-id="0ed96-104">What is SignalR?</span></span>

<span data-ttu-id="0ed96-105">ASP.NET Core SignalR est une bibliothèque open source qui simplifie l’ajout de fonctionnalités Web en temps réel aux applications.</span><span class="sxs-lookup"><span data-stu-id="0ed96-105">ASP.NET Core SignalR is an open-source library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="0ed96-106">La fonctionnalité Web en temps réel permet au code côté serveur de transmettre instantanément du contenu aux clients.</span><span class="sxs-lookup"><span data-stu-id="0ed96-106">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="0ed96-107">Bons candidats pour SignalR:</span><span class="sxs-lookup"><span data-stu-id="0ed96-107">Good candidates for SignalR:</span></span>

* <span data-ttu-id="0ed96-108">Applications qui requièrent des mises à jour à fréquence élevée à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="0ed96-108">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="0ed96-109">Les exemples sont les jeux, les réseaux sociaux, les votes, les enchères, les cartes et les applications GPS.</span><span class="sxs-lookup"><span data-stu-id="0ed96-109">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="0ed96-110">Tableaux de bord et applications de surveillance.</span><span class="sxs-lookup"><span data-stu-id="0ed96-110">Dashboards and monitoring apps.</span></span> <span data-ttu-id="0ed96-111">Les exemples incluent des tableaux de bord d’entreprise, des mises à jour de ventes instantanées ou des alertes de voyage.</span><span class="sxs-lookup"><span data-stu-id="0ed96-111">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="0ed96-112">Applications collaboratives.</span><span class="sxs-lookup"><span data-stu-id="0ed96-112">Collaborative apps.</span></span> <span data-ttu-id="0ed96-113">Les applications de tableau blanc et le logiciel de réunion d’équipe sont des exemples d’applications de collaboration.</span><span class="sxs-lookup"><span data-stu-id="0ed96-113">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="0ed96-114">Applications qui requièrent des notifications.</span><span class="sxs-lookup"><span data-stu-id="0ed96-114">Apps that require notifications.</span></span> <span data-ttu-id="0ed96-115">Les réseaux sociaux, la messagerie électronique, la conversation, les jeux, les alertes de voyage et beaucoup d’autres applications utilisent des notifications.</span><span class="sxs-lookup"><span data-stu-id="0ed96-115">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

SignalR<span data-ttu-id="0ed96-116"> fournit une API pour la création d' [appels de procédure distante (RPC)](https://wikipedia.org/wiki/Remote_procedure_call)de serveur à client.</span><span class="sxs-lookup"><span data-stu-id="0ed96-116"> provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="0ed96-117">Les RPC appellent des fonctions JavaScript sur les clients à partir du code .NET Core côté serveur.</span><span class="sxs-lookup"><span data-stu-id="0ed96-117">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="0ed96-118">Voici certaines fonctionnalités de SignalR pour ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="0ed96-118">Here are some features of SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="0ed96-119">Gère automatiquement la gestion des connexions.</span><span class="sxs-lookup"><span data-stu-id="0ed96-119">Handles connection management automatically.</span></span>
* <span data-ttu-id="0ed96-120">Envoie des messages à tous les clients connectés simultanément.</span><span class="sxs-lookup"><span data-stu-id="0ed96-120">Sends messages to all connected clients simultaneously.</span></span> <span data-ttu-id="0ed96-121">Par exemple, une salle de conversation.</span><span class="sxs-lookup"><span data-stu-id="0ed96-121">For example, a chat room.</span></span>
* <span data-ttu-id="0ed96-122">Envoie des messages à des clients ou groupes de clients spécifiques.</span><span class="sxs-lookup"><span data-stu-id="0ed96-122">Sends messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="0ed96-123">Met à l’échelle pour gérer le trafic qui augmente.</span><span class="sxs-lookup"><span data-stu-id="0ed96-123">Scales to handle increasing traffic.</span></span>

<span data-ttu-id="0ed96-124">La source est hébergée dans un [référentielSignalR sur GitHub](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).</span><span class="sxs-lookup"><span data-stu-id="0ed96-124">The source is hosted in a [SignalR repository on GitHub](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).</span></span>

## <a name="transports"></a><span data-ttu-id="0ed96-125">Transports</span><span class="sxs-lookup"><span data-stu-id="0ed96-125">Transports</span></span>

SignalR<span data-ttu-id="0ed96-126"> prend en charge les techniques suivantes pour gérer les communications en temps réel (par ordre de secours normal) :</span><span class="sxs-lookup"><span data-stu-id="0ed96-126"> supports the following techniques for handling real-time communication (in order of graceful fallback):</span></span>

* [<span data-ttu-id="0ed96-127">WebSockets</span><span class="sxs-lookup"><span data-stu-id="0ed96-127">WebSockets</span></span>](https://tools.ietf.org/html/rfc7118)
* <span data-ttu-id="0ed96-128">Événements envoyés par le serveur</span><span class="sxs-lookup"><span data-stu-id="0ed96-128">Server-Sent Events</span></span>
* <span data-ttu-id="0ed96-129">Interrogation longue</span><span class="sxs-lookup"><span data-stu-id="0ed96-129">Long Polling</span></span>

SignalR<span data-ttu-id="0ed96-130"> choisit automatiquement la meilleure méthode de transport parmi les capacités du serveur et du client.</span><span class="sxs-lookup"><span data-stu-id="0ed96-130"> automatically chooses the best transport method that is within the capabilities of the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="0ed96-131">Hubs</span><span class="sxs-lookup"><span data-stu-id="0ed96-131">Hubs</span></span>

SignalR<span data-ttu-id="0ed96-132"> utilise des *hubs* pour la communication entre les clients et les serveurs.</span><span class="sxs-lookup"><span data-stu-id="0ed96-132"> uses *hubs* to communicate between clients and servers.</span></span>

<span data-ttu-id="0ed96-133">Un Hub est un pipeline de haut niveau qui permet à un client et un serveur d’appeler des méthodes les unes sur les autres.</span><span class="sxs-lookup"><span data-stu-id="0ed96-133">A hub is a high-level pipeline that allows a client and server to call methods on each other.</span></span> SignalR<span data-ttu-id="0ed96-134"> gère automatiquement la distribution à travers les limites de l’ordinateur, ce qui permet aux clients d’appeler des méthodes sur le serveur et vice versa.</span><span class="sxs-lookup"><span data-stu-id="0ed96-134"> handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server and vice versa.</span></span> <span data-ttu-id="0ed96-135">Vous pouvez passer des paramètres fortement typés à des méthodes, ce qui active la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="0ed96-135">You can pass strongly-typed parameters to methods, which enables model binding.</span></span> SignalR<span data-ttu-id="0ed96-136"> fournit deux protocoles Hub intégrés : un protocole texte basé sur JSON et un protocole binaire basé sur [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="0ed96-136"> provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="0ed96-137">MessagePack crée généralement des messages plus petits par rapport à JSON.</span><span class="sxs-lookup"><span data-stu-id="0ed96-137">MessagePack generally creates smaller messages compared to JSON.</span></span> <span data-ttu-id="0ed96-138">Les anciens navigateurs doivent prendre en charge le [niveau 2 de XHR](https://caniuse.com/#feat=xhr2) pour fournir la prise en charge du protocole MessagePack.</span><span class="sxs-lookup"><span data-stu-id="0ed96-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="0ed96-139">Les hubs appellent le code côté client en envoyant des messages qui contiennent le nom et les paramètres de la méthode côté client.</span><span class="sxs-lookup"><span data-stu-id="0ed96-139">Hubs call client-side code by sending messages that contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="0ed96-140">Les objets envoyés en tant que paramètres de méthode sont désérialisés à l’aide du protocole configuré.</span><span class="sxs-lookup"><span data-stu-id="0ed96-140">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="0ed96-141">Le client tente de faire correspondre le nom à une méthode dans le code côté client.</span><span class="sxs-lookup"><span data-stu-id="0ed96-141">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="0ed96-142">Lorsque le client trouve une correspondance, il appelle la méthode et lui passe les données de paramètre désérialisées.</span><span class="sxs-lookup"><span data-stu-id="0ed96-142">When the client finds a match, it calls the method and passes to it the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0ed96-143">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="0ed96-143">Additional resources</span></span>

* <span data-ttu-id="0ed96-144">[Prise en main de SignalR pour ASP.NET Core](xref:tutorials/signalr)</span><span class="sxs-lookup"><span data-stu-id="0ed96-144">[Get started with SignalR for ASP.NET Core](xref:tutorials/signalr)</span></span>
* [<span data-ttu-id="0ed96-145">Plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="0ed96-145">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="0ed96-146">Hubs</span><span class="sxs-lookup"><span data-stu-id="0ed96-146">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="0ed96-147">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="0ed96-147">JavaScript client</span></span>](xref:signalr/javascript-client)
