---
title: Utilisation des hubs dans ASP.NET Core SignalR
author: tdykstra
description: Découvrez comment utiliser des hubs dans ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/12/2018
uid: signalr/hubs
ms.openlocfilehash: 17e3ee23967bc1097a3121b3e3e5b58cebe3887d
ms.sourcegitcommit: a742b55e4b8276a48b8b4394784554fecd883c84
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538360"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="88ec0-103">Utilisation des hubs dans SignalR pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="88ec0-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="88ec0-104">Par [Rachel Appel](https://twitter.com/rachelappel) et [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="88ec0-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="88ec0-105">[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(comment télécharger)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="88ec0-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="88ec0-106">Qu’est-ce qu’un hub SignalR ?</span><span class="sxs-lookup"><span data-stu-id="88ec0-106">What is a SignalR hub</span></span>

<span data-ttu-id="88ec0-107">L’API Hubs de SignalR vous permet d’appeler des méthodes sur des clients connectés à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="88ec0-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="88ec0-108">Dans le code serveur, vous définissez des méthodes qui sont appelées par le client.</span><span class="sxs-lookup"><span data-stu-id="88ec0-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="88ec0-109">Dans le code client, vous définissez des méthodes qui sont appelées à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="88ec0-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="88ec0-110">SignalR prend en charge tout ce qui se passe à l’arrière-plan et qui rend possibles les communications client-serveur et serveur-client en temps réel.</span><span class="sxs-lookup"><span data-stu-id="88ec0-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="88ec0-111">Configurer les concentrateurs SignalR</span><span class="sxs-lookup"><span data-stu-id="88ec0-111">Configure SignalR hubs</span></span>

<span data-ttu-id="88ec0-112">Le middleware SignalR requiert certains services, qui sont configurés en appelant `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="88ec0-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="88ec0-113">Quand vous ajoutez des fonctionnalités SignalR à une application ASP.NET Core, configurez les routes SignalR en appelant `app.UseSignalR` dans la méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="88ec0-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="88ec0-114">Créer et utiliser les hubs</span><span class="sxs-lookup"><span data-stu-id="88ec0-114">Create and use hubs</span></span>

<span data-ttu-id="88ec0-115">Créez un hub en déclarant une classe qui hérite de `Hub`et ajoutez-lui des méthodes publiques.</span><span class="sxs-lookup"><span data-stu-id="88ec0-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="88ec0-116">Les clients peuvent appeler des méthodes qui sont définies en tant que `public`.</span><span class="sxs-lookup"><span data-stu-id="88ec0-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="88ec0-117">Vous pouvez spécifier un type de retour et paramètres, y compris les types complexes et les tableaux, comme vous le feriez dans n’importe quelle méthode c#.</span><span class="sxs-lookup"><span data-stu-id="88ec0-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="88ec0-118">SignalR gère la sérialisation et désérialisation des objets complexes et des tableaux dans vos paramètres et valeurs de retournés.</span><span class="sxs-lookup"><span data-stu-id="88ec0-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="88ec0-119">L’objet de contexte</span><span class="sxs-lookup"><span data-stu-id="88ec0-119">The Context object</span></span>

<span data-ttu-id="88ec0-120">Le `Hub` classe a un `Context` propriété qui contient les propriétés suivantes avec des informations sur la connexion :</span><span class="sxs-lookup"><span data-stu-id="88ec0-120">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="88ec0-121">Propriété</span><span class="sxs-lookup"><span data-stu-id="88ec0-121">Property</span></span> | <span data-ttu-id="88ec0-122">Description</span><span class="sxs-lookup"><span data-stu-id="88ec0-122">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="88ec0-123">Obtient l’ID unique pour la connexion affectée par SignalR.</span><span class="sxs-lookup"><span data-stu-id="88ec0-123">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="88ec0-124">Il existe un identifiant de connexion pour chaque connexion.</span><span class="sxs-lookup"><span data-stu-id="88ec0-124">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="88ec0-125">Obtient le [identificateur d’utilisateur](xref:signalr/groups).</span><span class="sxs-lookup"><span data-stu-id="88ec0-125">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="88ec0-126">Par défaut, SignalR utilise le `ClaimTypes.NameIdentifier` à partir de la `ClaimsPrincipal` associé à la connexion comme identificateur d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="88ec0-126">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="88ec0-127">Obtient le `ClaimsPrincipal` associé à l’utilisateur actuel.</span><span class="sxs-lookup"><span data-stu-id="88ec0-127">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="88ec0-128">Obtient une collection clé/valeur qui peut être utilisée pour partager des données dans le cadre de cette connexion.</span><span class="sxs-lookup"><span data-stu-id="88ec0-128">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="88ec0-129">Données peuvent être stockées dans cette collection et il persistera pour la connexion entre les appels de méthode de concentrateur différents.</span><span class="sxs-lookup"><span data-stu-id="88ec0-129">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="88ec0-130">Obtient la collection de fonctionnalités disponibles sur la connexion.</span><span class="sxs-lookup"><span data-stu-id="88ec0-130">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="88ec0-131">Pour l’instant, cette collection n’est pas nécessaire dans la plupart des scénarios, donc il n’est pas encore documentée en détail.</span><span class="sxs-lookup"><span data-stu-id="88ec0-131">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="88ec0-132">Obtient un `CancellationToken` qui avertit de la connexion est abandonnée.</span><span class="sxs-lookup"><span data-stu-id="88ec0-132">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="88ec0-133">`Hub.Context` contient également les méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="88ec0-133">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="88ec0-134">Méthode</span><span class="sxs-lookup"><span data-stu-id="88ec0-134">Method</span></span> | <span data-ttu-id="88ec0-135">Description</span><span class="sxs-lookup"><span data-stu-id="88ec0-135">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="88ec0-136">Retourne le `HttpContext` pour la connexion, ou `null` si la connexion n’est pas associée à une requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="88ec0-136">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="88ec0-137">Pour les connexions HTTP, vous pouvez utiliser cette méthode pour obtenir des informations telles que les en-têtes HTTP et les chaînes de requête.</span><span class="sxs-lookup"><span data-stu-id="88ec0-137">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="88ec0-138">Abandonne la connexion.</span><span class="sxs-lookup"><span data-stu-id="88ec0-138">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="88ec0-139">L’objet de Clients</span><span class="sxs-lookup"><span data-stu-id="88ec0-139">The Clients object</span></span>

<span data-ttu-id="88ec0-140">Le `Hub` classe a un `Clients` propriété qui contient les propriétés suivantes pour la communication entre client et serveur :</span><span class="sxs-lookup"><span data-stu-id="88ec0-140">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="88ec0-141">Propriété</span><span class="sxs-lookup"><span data-stu-id="88ec0-141">Property</span></span> | <span data-ttu-id="88ec0-142">Description</span><span class="sxs-lookup"><span data-stu-id="88ec0-142">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="88ec0-143">Appelle une méthode sur tous les clients connectés</span><span class="sxs-lookup"><span data-stu-id="88ec0-143">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="88ec0-144">Appelle une méthode sur le client qui a appelé la méthode de hub</span><span class="sxs-lookup"><span data-stu-id="88ec0-144">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="88ec0-145">Appelle une méthode sur tous les clients connectés à l’exception du client qui a appelé la méthode</span><span class="sxs-lookup"><span data-stu-id="88ec0-145">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="88ec0-146">`Hub.Clients` contient également les méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="88ec0-146">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="88ec0-147">Méthode</span><span class="sxs-lookup"><span data-stu-id="88ec0-147">Method</span></span> | <span data-ttu-id="88ec0-148">Description</span><span class="sxs-lookup"><span data-stu-id="88ec0-148">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="88ec0-149">Appelle une méthode sur tous les clients connectés à l’exception des connexions spécifiées</span><span class="sxs-lookup"><span data-stu-id="88ec0-149">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="88ec0-150">Appelle une méthode sur un client connecté spécifique</span><span class="sxs-lookup"><span data-stu-id="88ec0-150">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="88ec0-151">Appelle une méthode sur des clients connectés spécifiques</span><span class="sxs-lookup"><span data-stu-id="88ec0-151">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="88ec0-152">Appelle une méthode à toutes les connexions dans le groupe spécifié</span><span class="sxs-lookup"><span data-stu-id="88ec0-152">Calls a method to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="88ec0-153">Appelle une méthode à toutes les connexions dans le groupe spécifié, à l’exception des connexions spécifiées</span><span class="sxs-lookup"><span data-stu-id="88ec0-153">Calls a method to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="88ec0-154">Appelle une méthode à plusieurs groupes de connexions</span><span class="sxs-lookup"><span data-stu-id="88ec0-154">Calls a method to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="88ec0-155">Appelle une méthode sur un groupe de connexions, à l’exclusion du client qui a appelé la méthode de hub</span><span class="sxs-lookup"><span data-stu-id="88ec0-155">Calls a method to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="88ec0-156">Appelle une méthode pour toutes les connexions associées à un utilisateur spécifique</span><span class="sxs-lookup"><span data-stu-id="88ec0-156">Calls a method to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="88ec0-157">Appelle une méthode pour toutes les connexions associées aux utilisateurs spécifiés</span><span class="sxs-lookup"><span data-stu-id="88ec0-157">Calls a method to all connections associated with the specified users</span></span> |

<span data-ttu-id="88ec0-158">Chaque méthode ou propriété des tableaux précédents retourne un objet avec une méthode `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="88ec0-158">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="88ec0-159">Le méthode `SendAsync` vous permet de fournir le nom et les paramètres de la méthode du client à appeler.</span><span class="sxs-lookup"><span data-stu-id="88ec0-159">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="88ec0-160">Envoyer des messages aux clients</span><span class="sxs-lookup"><span data-stu-id="88ec0-160">Send messages to clients</span></span>

<span data-ttu-id="88ec0-161">Pour effectuer des appels à des clients spécifiques, utilisez les propriétés de l'objet `Clients`.</span><span class="sxs-lookup"><span data-stu-id="88ec0-161">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="88ec0-162">Dans l’exemple suivant, la méthode `SendMessageToCaller` illustre l’envoi d’un message à la connexion qui a appelé la méthode de hub.</span><span class="sxs-lookup"><span data-stu-id="88ec0-162">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="88ec0-163">Le méthode `SendMessageToGroups` envoie un message pour les groupes stockés dans une `List` nommée `groups`.</span><span class="sxs-lookup"><span data-stu-id="88ec0-163">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="strongly-typed-hubs"></a><span data-ttu-id="88ec0-164">Hubs fortement typés</span><span class="sxs-lookup"><span data-stu-id="88ec0-164">Strongly typed hubs</span></span>

<span data-ttu-id="88ec0-165">Un inconvénient de l’utilisation de `SendAsync` est qu’elle s’appuie sur une chaîne magique pour spécifier la méthode de client à appeler.</span><span class="sxs-lookup"><span data-stu-id="88ec0-165">A drawback of using `SendAsync` is that it relies on a magic string to specify the client method to be called.</span></span> <span data-ttu-id="88ec0-166">Cela laisse ouvrir du code pour les erreurs d’exécution si le nom de la méthode est mal orthographié ou manquant à partir du client.</span><span class="sxs-lookup"><span data-stu-id="88ec0-166">This leaves code open to runtime errors if the method name is misspelled or missing from the client.</span></span>

<span data-ttu-id="88ec0-167">Une alternative à l’utilisation de `SendAsync` est pour typer fortement les `Hub` avec <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span><span class="sxs-lookup"><span data-stu-id="88ec0-167">An alternative to using `SendAsync` is to strongly type the `Hub` with <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span></span> <span data-ttu-id="88ec0-168">Dans l’exemple suivant, le `ChatHub` méthodes client ont été extraite et placées dans une interface appelée `IChatClient`.</span><span class="sxs-lookup"><span data-stu-id="88ec0-168">In the following example, the `ChatHub` client methods have been extracted out into an interface called `IChatClient`.</span></span>  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

<span data-ttu-id="88ec0-169">Cette interface peut être utilisée pour refactoriser l’exemple précédent `ChatHub` exemple.</span><span class="sxs-lookup"><span data-stu-id="88ec0-169">This interface can be used to refactor the preceding `ChatHub` example.</span></span>

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

<span data-ttu-id="88ec0-170">À l’aide de `Hub<IChatClient>` Active la vérification de la compilation des méthodes client.</span><span class="sxs-lookup"><span data-stu-id="88ec0-170">Using `Hub<IChatClient>` enables compile-time checking of the client methods.</span></span> <span data-ttu-id="88ec0-171">Cela évite les problèmes provoqués par l’utilisation de chaînes magiques, étant donné que `Hub<T>` permettent uniquement d’accéder aux méthodes définies dans l’interface.</span><span class="sxs-lookup"><span data-stu-id="88ec0-171">This prevents issues caused by using magic strings, since `Hub<T>` can only provide access to the methods defined in the interface.</span></span>

<span data-ttu-id="88ec0-172">À l’aide de fortement typé `Hub<T>` désactive la possibilité d’utiliser `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="88ec0-172">Using a strongly typed `Hub<T>` disables the ability to use `SendAsync`.</span></span>

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="88ec0-173">Gérer les événements pour une connexion</span><span class="sxs-lookup"><span data-stu-id="88ec0-173">Handle events for a connection</span></span>

<span data-ttu-id="88ec0-174">L’API Hubs de SignalR fournit les méthodes virtuelles `OnConnectedAsync` et `OnDisconnectedAsync` pour gérer et suivre les connexions.</span><span class="sxs-lookup"><span data-stu-id="88ec0-174">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="88ec0-175">Remplacez la méthode virtuelle `OnConnectedAsync` pour effectuer des actions lorsqu’un client se connecte au hub, comme l’ajouter à un groupe.
</span><span class="sxs-lookup"><span data-stu-id="88ec0-175">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="88ec0-176">Gérer les erreurs</span><span class="sxs-lookup"><span data-stu-id="88ec0-176">Handle errors</span></span>

<span data-ttu-id="88ec0-177">Les exceptions levées dans vos méthodes de hub sont envoyées au client qui a appelé la méthode.</span><span class="sxs-lookup"><span data-stu-id="88ec0-177">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="88ec0-178">Sur le client JavaScript, la méthode `invoke` retourne une [promesse JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="88ec0-178">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="88ec0-179">Lorsque le client reçoit une erreur avec un gestionnaire attaché à la promesse avec `catch`, elle est appelée et passée en tant qu’objet JavaScript `Error`.
</span><span class="sxs-lookup"><span data-stu-id="88ec0-179">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="88ec0-180">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="88ec0-180">Related resources</span></span>

* [<span data-ttu-id="88ec0-181">Introduction à ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="88ec0-181">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="88ec0-182">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="88ec0-182">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="88ec0-183">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="88ec0-183">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
