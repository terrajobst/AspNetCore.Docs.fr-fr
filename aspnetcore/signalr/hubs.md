---
title: Utilisation des hubs dans ASP.NET Core SignalR
author: rachelappel
description: En savoir plus sur l’utilisation des hubs dans ASP.NET Core SignalR.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/01/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: 495aa156dd5e4641d688d7b16df1e5814c9607f4
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34819082"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="735d6-103">Utilisation des hubs dans SignalR pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="735d6-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="735d6-104">Par [Rachel Appel](https://twitter.com/rachelappel) et [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="735d6-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="735d6-105">[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(comment télécharger)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="735d6-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="735d6-106">Qu’est un concentrateur SignalR</span><span class="sxs-lookup"><span data-stu-id="735d6-106">What is a SignalR hub</span></span>

<span data-ttu-id="735d6-107">L’API de concentrateurs SignalR vous permet d’appeler des méthodes sur les clients connectés à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="735d6-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="735d6-108">Dans le code serveur, définir des méthodes qui sont appelées par le client.</span><span class="sxs-lookup"><span data-stu-id="735d6-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="735d6-109">Dans le code client, définir des méthodes qui sont appelées à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="735d6-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="735d6-110">SignalR prend en charge de tous les éléments en arrière-plan qui permet des communications client-serveur et client-server en temps réel.</span><span class="sxs-lookup"><span data-stu-id="735d6-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="735d6-111">Configurer les concentrateurs SignalR</span><span class="sxs-lookup"><span data-stu-id="735d6-111">Configure SignalR hubs</span></span>

<span data-ttu-id="735d6-112">L’intergiciel (middleware) SignalR requiert certains services, qui sont configurés en appelant `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="735d6-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="735d6-113">Lorsque vous ajoutez des fonctionnalités de SignalR pour une application ASP.NET Core, configurer les itinéraires SignalR en appelant `app.UseSignalR` dans le `Startup.Configure` (méthode).</span><span class="sxs-lookup"><span data-stu-id="735d6-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="735d6-114">Créer et utiliser les concentrateurs</span><span class="sxs-lookup"><span data-stu-id="735d6-114">Create and use hubs</span></span>

<span data-ttu-id="735d6-115">Créer un hub en déclarant une classe qui hérite de `Hub`et lui ajouter des méthodes publiques.</span><span class="sxs-lookup"><span data-stu-id="735d6-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="735d6-116">Les clients peuvent appeler des méthodes qui sont définies en tant que `public`.</span><span class="sxs-lookup"><span data-stu-id="735d6-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="735d6-117">Vous pouvez spécifier un type de retour et des paramètres, y compris les types complexes et les tableaux, comme vous le feriez dans n’importe quelle méthode c#.</span><span class="sxs-lookup"><span data-stu-id="735d6-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="735d6-118">SignalR gère la sérialisation et désérialisation d’objets complexes et les tableaux dans vos paramètres et les valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="735d6-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-clients-object"></a><span data-ttu-id="735d6-119">L’objet de Clients</span><span class="sxs-lookup"><span data-stu-id="735d6-119">The Clients object</span></span>

<span data-ttu-id="735d6-120">Chaque instance de la `Hub` classe a une propriété nommée `Clients` qui contient les membres suivants pour la communication entre le serveur et le client :</span><span class="sxs-lookup"><span data-stu-id="735d6-120">Each instance of the `Hub` class has a property named `Clients` that contains the following members for communication between server and client:</span></span>

| <span data-ttu-id="735d6-121">Propriété</span><span class="sxs-lookup"><span data-stu-id="735d6-121">Property</span></span> | <span data-ttu-id="735d6-122">Description</span><span class="sxs-lookup"><span data-stu-id="735d6-122">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="735d6-123">Appelle une méthode sur tous les clients connectés</span><span class="sxs-lookup"><span data-stu-id="735d6-123">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="735d6-124">Appelle une méthode sur le client qui a appelé la méthode de concentrateur</span><span class="sxs-lookup"><span data-stu-id="735d6-124">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="735d6-125">Appelle une méthode sur tous les clients connectés à l’exception du client qui a appelé la méthode</span><span class="sxs-lookup"><span data-stu-id="735d6-125">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="735d6-126">En outre, `Hub.Clients` contient les méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="735d6-126">Additionally, `Hub.Clients` contains the following methods:</span></span>

| <span data-ttu-id="735d6-127">Méthode</span><span class="sxs-lookup"><span data-stu-id="735d6-127">Method</span></span> | <span data-ttu-id="735d6-128">Description</span><span class="sxs-lookup"><span data-stu-id="735d6-128">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="735d6-129">Appelle une méthode sur tous les clients connectés à l’exception des connexions spécifiées</span><span class="sxs-lookup"><span data-stu-id="735d6-129">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="735d6-130">Appelle une méthode sur un client connecté spécifique</span><span class="sxs-lookup"><span data-stu-id="735d6-130">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="735d6-131">Appelle une méthode sur les clients connectés spécifiques</span><span class="sxs-lookup"><span data-stu-id="735d6-131">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="735d6-132">Appelle une méthode à toutes les connexions dans le groupe spécifié</span><span class="sxs-lookup"><span data-stu-id="735d6-132">Calls a method to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="735d6-133">Appelle une méthode à toutes les connexions dans le groupe spécifié, à l’exception des connexions spécifiées</span><span class="sxs-lookup"><span data-stu-id="735d6-133">Calls a method to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="735d6-134">Appelle une méthode à plusieurs groupes de connexions</span><span class="sxs-lookup"><span data-stu-id="735d6-134">Calls a method to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="735d6-135">Appelle une méthode à un groupe de connexions, à l’exclusion du client qui a appelé la méthode de concentrateur</span><span class="sxs-lookup"><span data-stu-id="735d6-135">Calls a method to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="735d6-136">Appelle une méthode à toutes les connexions associées à un utilisateur spécifique</span><span class="sxs-lookup"><span data-stu-id="735d6-136">Calls a method to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="735d6-137">Appelle une méthode à toutes les connexions associées aux utilisateurs spécifiés</span><span class="sxs-lookup"><span data-stu-id="735d6-137">Calls a method to all connections associated with the specified users</span></span> |

<span data-ttu-id="735d6-138">Chaque méthode ou propriété dans les tables précédentes retourne un objet avec un `SendAsync` (méthode).</span><span class="sxs-lookup"><span data-stu-id="735d6-138">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="735d6-139">Le `SendAsync` méthode vous permet de fournir le nom et les paramètres de la méthode du client à appeler.</span><span class="sxs-lookup"><span data-stu-id="735d6-139">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="735d6-140">Envoyer des messages aux clients</span><span class="sxs-lookup"><span data-stu-id="735d6-140">Send messages to clients</span></span>

<span data-ttu-id="735d6-141">Pour effectuer des appels à des clients spécifiques, utilisez les propriétés de la `Clients` objet.</span><span class="sxs-lookup"><span data-stu-id="735d6-141">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="735d6-142">Dans l’exemple suivant, la `SendMessageToCaller` méthode illustre l’envoi d’un message à la connexion qui a appelé la méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="735d6-142">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="735d6-143">Le `SendMessageToGroups` méthode envoie un message pour les groupes stockés dans un `List` nommé `groups`.</span><span class="sxs-lookup"><span data-stu-id="735d6-143">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="735d6-144">Gérer les événements pour une connexion</span><span class="sxs-lookup"><span data-stu-id="735d6-144">Handle events for a connection</span></span>

<span data-ttu-id="735d6-145">L’API de concentrateurs SignalR fournit le `OnConnectedAsync` et `OnDisconnectedAsync` méthodes virtuelles pour gérer et suivre les connexions.</span><span class="sxs-lookup"><span data-stu-id="735d6-145">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="735d6-146">Remplacer la `OnConnectedAsync` méthode virtuelle pour effectuer des actions lorsqu’un client se connecte au concentrateur, telles que l’ajouter à un groupe.</span><span class="sxs-lookup"><span data-stu-id="735d6-146">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="735d6-147">Gérer les erreurs</span><span class="sxs-lookup"><span data-stu-id="735d6-147">Handle errors</span></span>

<span data-ttu-id="735d6-148">Les exceptions levées dans vos méthodes de concentrateur sont envoyées au client qui a appelé la méthode.</span><span class="sxs-lookup"><span data-stu-id="735d6-148">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="735d6-149">Sur le client JavaScript, le `invoke` méthode retourne un [méthode JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="735d6-149">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="735d6-150">Lorsque le client reçoit une erreur avec un gestionnaire attaché l’à l’aide de la promesse `catch`, elle a appelée et passée comme un code JavaScript `Error` objet.</span><span class="sxs-lookup"><span data-stu-id="735d6-150">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="735d6-151">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="735d6-151">Related resources</span></span>

* [<span data-ttu-id="735d6-152">Présentation d’ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="735d6-152">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="735d6-153">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="735d6-153">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="735d6-154">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="735d6-154">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
