---
title: Utilisation des hubs dans ASP.NET Core SignalR
author: rachelappel
description: En savoir plus sur l’utilisation des hubs dans ASP.NET Core SignalR.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 03/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: 7da0c4832b1aa6a844172bf751a46b280a02f37a
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="f0236-103">Utilisation des hubs dans SignalR pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f0236-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="f0236-104">Par [Rachel Appel](https://twitter.com/rachelappel) et [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="f0236-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="f0236-105">Qu’est un concentrateur SignalR</span><span class="sxs-lookup"><span data-stu-id="f0236-105">What is a SignalR hub</span></span>

<span data-ttu-id="f0236-106">L’API de concentrateurs SignalR vous permet d’appeler des méthodes sur les clients connectés à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="f0236-106">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="f0236-107">Dans le code serveur, définir des méthodes qui sont appelées par le client.</span><span class="sxs-lookup"><span data-stu-id="f0236-107">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="f0236-108">Dans le code client, définir des méthodes qui sont appelées à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="f0236-108">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="f0236-109">SignalR prend en charge de tous les éléments en arrière-plan qui permet des communications client-serveur et client-server en temps réel.</span><span class="sxs-lookup"><span data-stu-id="f0236-109">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="f0236-110">Configurer les concentrateurs SignalR</span><span class="sxs-lookup"><span data-stu-id="f0236-110">Configure SignalR hubs</span></span>

<span data-ttu-id="f0236-111">L’intergiciel (middleware) SignalR requiert certains services, qui sont configurés en appelant `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="f0236-111">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=35)]

<span data-ttu-id="f0236-112">Lorsque vous ajoutez des fonctionnalités de SignalR pour une application ASP.NET Core, configurer les itinéraires SignalR en appelant `app.UseSignalR` dans le `Startup.Configure` (méthode).</span><span class="sxs-lookup"><span data-stu-id="f0236-112">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=55-58)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="f0236-113">Créer et utiliser les concentrateurs</span><span class="sxs-lookup"><span data-stu-id="f0236-113">Create and use hubs</span></span>

<span data-ttu-id="f0236-114">Créer un hub en déclarant une classe qui hérite de `Hub`et lui ajouter des méthodes publiques.</span><span class="sxs-lookup"><span data-stu-id="f0236-114">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="f0236-115">Les clients peuvent appeler des méthodes qui sont définies en tant que `public`.</span><span class="sxs-lookup"><span data-stu-id="f0236-115">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/chathub.cs?range=10-13)]

<span data-ttu-id="f0236-116">Vous pouvez spécifier un type de retour et des paramètres, y compris les types complexes et les tableaux, comme vous le feriez dans n’importe quelle méthode c#.</span><span class="sxs-lookup"><span data-stu-id="f0236-116">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="f0236-117">SignalR gère la sérialisation et désérialisation d’objets complexes et les tableaux dans vos paramètres et les valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="f0236-117">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-clients-object"></a><span data-ttu-id="f0236-118">L’objet de Clients</span><span class="sxs-lookup"><span data-stu-id="f0236-118">The Clients object</span></span>

<span data-ttu-id="f0236-119">Chaque instance de la `Hub` classe a une propriété nommée `Clients` qui contient les membres suivants pour la communication entre le serveur et le client :</span><span class="sxs-lookup"><span data-stu-id="f0236-119">Each instance of the `Hub` class has a property named `Clients` that contains the following members for communication between server and client:</span></span>

| <span data-ttu-id="f0236-120">Propriété</span><span class="sxs-lookup"><span data-stu-id="f0236-120">Property</span></span> | <span data-ttu-id="f0236-121">Description</span><span class="sxs-lookup"><span data-stu-id="f0236-121">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="f0236-122">Appelle une méthode sur tous les clients connectés</span><span class="sxs-lookup"><span data-stu-id="f0236-122">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="f0236-123">Appelle une méthode sur le client qui a appelé la méthode de concentrateur</span><span class="sxs-lookup"><span data-stu-id="f0236-123">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="f0236-124">Appelle une méthode sur tous les clients connectés à l’exception du client qui a appelé la méthode</span><span class="sxs-lookup"><span data-stu-id="f0236-124">Calls a method on all connected clients except the client that invoked the method</span></span> |

<span data-ttu-id="f0236-125">En outre, la `Hub` classe contient les méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="f0236-125">Additionally, the `Hub` class contains the following methods:</span></span>

| <span data-ttu-id="f0236-126">Méthode</span><span class="sxs-lookup"><span data-stu-id="f0236-126">Method</span></span> | <span data-ttu-id="f0236-127">Description</span><span class="sxs-lookup"><span data-stu-id="f0236-127">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="f0236-128">Appelle une méthode sur tous les clients connectés à l’exception des connexions spécifiées</span><span class="sxs-lookup"><span data-stu-id="f0236-128">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="f0236-129">Appelle une méthode sur un client connecté spécifique</span><span class="sxs-lookup"><span data-stu-id="f0236-129">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="f0236-130">Appelle une méthode sur les clients connectés spécifiques</span><span class="sxs-lookup"><span data-stu-id="f0236-130">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="f0236-131">Envoie un message à toutes les connexions dans le groupe spécifié</span><span class="sxs-lookup"><span data-stu-id="f0236-131">Sends a message to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="f0236-132">Envoie un message à toutes les connexions dans le groupe spécifié, à l’exception des connexions spécifiées</span><span class="sxs-lookup"><span data-stu-id="f0236-132">Sends a message to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="f0236-133">Envoie un message à plusieurs groupes de connexions</span><span class="sxs-lookup"><span data-stu-id="f0236-133">Sends a message to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="f0236-134">Envoie un message à un groupe de connexions, à l’exclusion du client qui a appelé la méthode de concentrateur</span><span class="sxs-lookup"><span data-stu-id="f0236-134">Sends a message to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="f0236-135">Envoie un message à toutes les connexions associées à un utilisateur spécifique</span><span class="sxs-lookup"><span data-stu-id="f0236-135">Sends a message to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="f0236-136">Envoie un message à toutes les connexions associées aux utilisateurs spécifiés</span><span class="sxs-lookup"><span data-stu-id="f0236-136">Sends a message to all connections associated with the specified users</span></span> |

<span data-ttu-id="f0236-137">Chaque méthode ou propriété dans les tables précédentes retourne un objet avec un `SendAsync` (méthode).</span><span class="sxs-lookup"><span data-stu-id="f0236-137">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="f0236-138">Le `SendAsync` méthode vous permet de fournir le nom et les paramètres de la méthode du client à appeler.</span><span class="sxs-lookup"><span data-stu-id="f0236-138">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="f0236-139">Envoyer des messages aux clients</span><span class="sxs-lookup"><span data-stu-id="f0236-139">Send messages to clients</span></span>

<span data-ttu-id="f0236-140">Pour effectuer des appels à des clients spécifiques, utilisez les propriétés de la `Clients` objet.</span><span class="sxs-lookup"><span data-stu-id="f0236-140">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="f0236-141">Dans l’exemple suivant dans l’exemple suivant, la `SendMessageToCaller` méthode illustre l’envoi d’un message à la connexion qui a appelé la méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="f0236-141">In the following In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="f0236-142">Le `SendMessageToGroups` méthode envoie un message pour les groupes stockés dans un `List` nommé `groups`.</span><span class="sxs-lookup"><span data-stu-id="f0236-142">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="f0236-143">Gérer les événements pour une connexion</span><span class="sxs-lookup"><span data-stu-id="f0236-143">Handle events for a connection</span></span>

<span data-ttu-id="f0236-144">L’API de concentrateurs SignalR fournit le `OnConnectedAsync` et `OnDisconnectedAsync` méthodes virtuelles pour gérer et suivre les connexions.</span><span class="sxs-lookup"><span data-stu-id="f0236-144">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="f0236-145">Remplacer la `OnConnectedAsync` méthode virtuelle pour effectuer des actions lorsqu’un client se connecte au concentrateur, telles que l’ajouter à un groupe.</span><span class="sxs-lookup"><span data-stu-id="f0236-145">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/chathub.cs?range=26-30)]

## <a name="handle-errors"></a><span data-ttu-id="f0236-146">Gérer les erreurs</span><span class="sxs-lookup"><span data-stu-id="f0236-146">Handle errors</span></span>

<span data-ttu-id="f0236-147">Les exceptions levées dans vos méthodes de concentrateur sont envoyées au client qui a appelé la méthode.</span><span class="sxs-lookup"><span data-stu-id="f0236-147">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="f0236-148">Sur le client JavaScript, le `invoke` méthode retourne un [méthode JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="f0236-148">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="f0236-149">Lorsque le client reçoit une erreur avec un gestionnaire attaché l’à l’aide de la promesse `catch`, elle a appelée et passée comme un code JavaScript `Error` objet.</span><span class="sxs-lookup"><span data-stu-id="f0236-149">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/chat.js?range=20)]
[!code-javascript[Error](hubs/sample/chat.js?range=16-18)]

## <a name="related-resources"></a><span data-ttu-id="f0236-150">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="f0236-150">Related resources</span></span>

[<span data-ttu-id="f0236-151">Présentation d’ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="f0236-151">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)