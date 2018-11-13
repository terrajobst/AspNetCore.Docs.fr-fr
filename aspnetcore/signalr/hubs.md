---
title: Utilisation des hubs dans ASP.NET Core SignalR
author: tdykstra
description: Découvrez comment utiliser des hubs dans ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/07/2018
uid: signalr/hubs
ms.openlocfilehash: 0413d354307208726f4252f431ac59526effed08
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51569917"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="57087-103">Utilisation des hubs dans SignalR pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="57087-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="57087-104">Par [Rachel Appel](https://twitter.com/rachelappel) et [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="57087-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="57087-105">[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(comment télécharger)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="57087-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="57087-106">Qu’est-ce qu’un hub SignalR ?</span><span class="sxs-lookup"><span data-stu-id="57087-106">What is a SignalR hub</span></span>

<span data-ttu-id="57087-107">L’API Hubs de SignalR vous permet d’appeler des méthodes sur des clients connectés à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="57087-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="57087-108">Dans le code serveur, vous définissez des méthodes qui sont appelées par le client.</span><span class="sxs-lookup"><span data-stu-id="57087-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="57087-109">Dans le code client, vous définissez des méthodes qui sont appelées à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="57087-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="57087-110">SignalR prend en charge tout ce qui se passe à l’arrière-plan et qui rend possibles les communications client-serveur et serveur-client en temps réel.</span><span class="sxs-lookup"><span data-stu-id="57087-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="57087-111">Configurer les hubs SignalR</span><span class="sxs-lookup"><span data-stu-id="57087-111">Configure SignalR hubs</span></span>

<span data-ttu-id="57087-112">Le middleware SignalR requiert certains services, qui sont configurés en appelant `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="57087-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="57087-113">Quand vous ajoutez des fonctionnalités SignalR à une application ASP.NET Core, configurez les routes SignalR en appelant `app.UseSignalR` dans la méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="57087-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="57087-114">Créer et utiliser les hubs</span><span class="sxs-lookup"><span data-stu-id="57087-114">Create and use hubs</span></span>

<span data-ttu-id="57087-115">Créez un hub en déclarant une classe qui hérite de `Hub`et ajoutez-lui des méthodes publiques.</span><span class="sxs-lookup"><span data-stu-id="57087-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="57087-116">Les clients peuvent appeler des méthodes qui sont définies en tant que `public`.</span><span class="sxs-lookup"><span data-stu-id="57087-116">Clients can call methods that are defined as `public`.</span></span>

```csharp
public class ChatHub : Hub
{
    public Task SendMessage(string user, string message)
    {
        return Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

<span data-ttu-id="57087-117">Vous pouvez spécifier un type de retour et paramètres, y compris les types complexes et les tableaux, comme vous le feriez dans n’importe quelle méthode c#.</span><span class="sxs-lookup"><span data-stu-id="57087-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="57087-118">SignalR gère la sérialisation et désérialisation des objets complexes et des tableaux dans vos paramètres et valeurs de retournés.</span><span class="sxs-lookup"><span data-stu-id="57087-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

> [!NOTE]
> <span data-ttu-id="57087-119">Les concentrateurs sont temporaires :</span><span class="sxs-lookup"><span data-stu-id="57087-119">Hubs are transient:</span></span>
> * <span data-ttu-id="57087-120">Ne pas stocker l’état dans une propriété sur la classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="57087-120">Don't store state in a property on the hub class.</span></span> <span data-ttu-id="57087-121">Chaque appel de méthode de concentrateur est exécutée sur une nouvelle instance de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="57087-121">Every hub method call is executed on a new hub instance.</span></span>  
> * <span data-ttu-id="57087-122">Utilisez `await` lors de l’appel de méthodes asynchrones qui dépendent du hub de rester actif.</span><span class="sxs-lookup"><span data-stu-id="57087-122">Use `await` when calling asynchronous methods that depend on the hub staying alive.</span></span> <span data-ttu-id="57087-123">Par exemple, une méthode telle que `Clients.All.SendAsync(...)` peut échouer si elle est appelée sans `await` et la méthode de concentrateur se termine avant `SendAsync` se termine.</span><span class="sxs-lookup"><span data-stu-id="57087-123">For example, a method such as `Clients.All.SendAsync(...)` can fail if it's called without `await` and the hub method completes before `SendAsync` finishes.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="57087-124">L’objet de contexte</span><span class="sxs-lookup"><span data-stu-id="57087-124">The Context object</span></span>

<span data-ttu-id="57087-125">La classe `Hub` a une propriété `Context` qui contient les propriétés suivantes avec des informations sur la connexion :</span><span class="sxs-lookup"><span data-stu-id="57087-125">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="57087-126">Propriété</span><span class="sxs-lookup"><span data-stu-id="57087-126">Property</span></span> | <span data-ttu-id="57087-127">Description</span><span class="sxs-lookup"><span data-stu-id="57087-127">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="57087-128">Obtient l’ID unique pour la connexion affectée par SignalR.</span><span class="sxs-lookup"><span data-stu-id="57087-128">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="57087-129">Il existe un identifiant de connexion pour chaque connexion.</span><span class="sxs-lookup"><span data-stu-id="57087-129">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="57087-130">Obtient l'[identificateur d’utilisateur](xref:signalr/groups).</span><span class="sxs-lookup"><span data-stu-id="57087-130">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="57087-131">Par défaut, SignalR utilise le `ClaimTypes.NameIdentifier` provenant du `ClaimsPrincipal` associé à la connexion comme identificateur d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="57087-131">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="57087-132">Obtient le `ClaimsPrincipal` associé à l’utilisateur actuel.</span><span class="sxs-lookup"><span data-stu-id="57087-132">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="57087-133">Obtient une collection clé/valeur qui peut être utilisée pour partager des données dans le cadre de cette connexion.</span><span class="sxs-lookup"><span data-stu-id="57087-133">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="57087-134">Données peuvent être stockées dans cette collection et il persistera pour la connexion entre les appels de méthode de concentrateur différents.</span><span class="sxs-lookup"><span data-stu-id="57087-134">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="57087-135">Obtient la collection de fonctionnalités disponibles sur la connexion.</span><span class="sxs-lookup"><span data-stu-id="57087-135">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="57087-136">Pour l’instant, cette collection n’est pas nécessaire dans la plupart des scénarios, donc elle n’est pas encore documentée en détail.</span><span class="sxs-lookup"><span data-stu-id="57087-136">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="57087-137">Obtient un `CancellationToken` qui avertit quand connexion est abandonnée.</span><span class="sxs-lookup"><span data-stu-id="57087-137">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="57087-138">`Hub.Context` contient également les méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="57087-138">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="57087-139">Méthode</span><span class="sxs-lookup"><span data-stu-id="57087-139">Method</span></span> | <span data-ttu-id="57087-140">Description</span><span class="sxs-lookup"><span data-stu-id="57087-140">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="57087-141">Retourne le `HttpContext` pour la connexion, ou `null` si la connexion n’est pas associée à une requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="57087-141">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="57087-142">Pour les connexions HTTP, vous pouvez utiliser cette méthode pour obtenir des informations telles que les en-têtes HTTP et les chaînes de requête.</span><span class="sxs-lookup"><span data-stu-id="57087-142">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="57087-143">Abandonne la connexion.</span><span class="sxs-lookup"><span data-stu-id="57087-143">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="57087-144">L’objet de Clients</span><span class="sxs-lookup"><span data-stu-id="57087-144">The Clients object</span></span>

<span data-ttu-id="57087-145">La classe `Hub` a une propriété `Clients` qui contient les propriétés suivantes pour la communication entre client et serveur :</span><span class="sxs-lookup"><span data-stu-id="57087-145">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="57087-146">Propriété</span><span class="sxs-lookup"><span data-stu-id="57087-146">Property</span></span> | <span data-ttu-id="57087-147">Description</span><span class="sxs-lookup"><span data-stu-id="57087-147">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="57087-148">Appelle une méthode sur tous les clients connectés</span><span class="sxs-lookup"><span data-stu-id="57087-148">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="57087-149">Appelle une méthode sur le client qui a appelé la méthode de hub</span><span class="sxs-lookup"><span data-stu-id="57087-149">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="57087-150">Appelle une méthode sur tous les clients connectés à l’exception du client qui a appelé la méthode</span><span class="sxs-lookup"><span data-stu-id="57087-150">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="57087-151">`Hub.Clients` contient également les méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="57087-151">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="57087-152">Méthode</span><span class="sxs-lookup"><span data-stu-id="57087-152">Method</span></span> | <span data-ttu-id="57087-153">Description</span><span class="sxs-lookup"><span data-stu-id="57087-153">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="57087-154">Appelle une méthode sur tous les clients connectés à l’exception des connexions spécifiées</span><span class="sxs-lookup"><span data-stu-id="57087-154">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="57087-155">Appelle une méthode sur un client connecté spécifique</span><span class="sxs-lookup"><span data-stu-id="57087-155">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="57087-156">Appelle une méthode sur des clients connectés spécifiques</span><span class="sxs-lookup"><span data-stu-id="57087-156">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="57087-157">Appelle une méthode sur toutes les connexions dans le groupe spécifié</span><span class="sxs-lookup"><span data-stu-id="57087-157">Calls a method on all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="57087-158">Appelle une méthode sur toutes les connexions dans le groupe spécifié, à l’exception des connexions spécifiées</span><span class="sxs-lookup"><span data-stu-id="57087-158">Calls a method on all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="57087-159">Appelle une méthode sur plusieurs groupes de connexions</span><span class="sxs-lookup"><span data-stu-id="57087-159">Calls a method on multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="57087-160">Appelle une méthode sur un groupe de connexions, à l’exclusion du client qui a appelé la méthode de concentrateur</span><span class="sxs-lookup"><span data-stu-id="57087-160">Calls a method on a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="57087-161">Appelle une méthode sur toutes les connexions associées à un utilisateur spécifique</span><span class="sxs-lookup"><span data-stu-id="57087-161">Calls a method on all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="57087-162">Appelle une méthode sur toutes les connexions associées aux utilisateurs spécifiés</span><span class="sxs-lookup"><span data-stu-id="57087-162">Calls a method on all connections associated with the specified users</span></span> |

<span data-ttu-id="57087-163">Chaque méthode ou propriété des tableaux précédents retourne un objet avec une méthode `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="57087-163">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="57087-164">Le méthode `SendAsync` vous permet de fournir le nom et les paramètres de la méthode du client à appeler.</span><span class="sxs-lookup"><span data-stu-id="57087-164">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="57087-165">Envoyer des messages aux clients</span><span class="sxs-lookup"><span data-stu-id="57087-165">Send messages to clients</span></span>

<span data-ttu-id="57087-166">Pour effectuer des appels à des clients spécifiques, utilisez les propriétés de l'objet `Clients`.</span><span class="sxs-lookup"><span data-stu-id="57087-166">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="57087-167">Dans l’exemple suivant, il existe trois méthodes de concentrateur :</span><span class="sxs-lookup"><span data-stu-id="57087-167">In the following example, there are three Hub methods:</span></span>

* <span data-ttu-id="57087-168">`SendMessage` envoie un message à tous les clients connectés, à l’aide de `Clients.All`.</span><span class="sxs-lookup"><span data-stu-id="57087-168">`SendMessage` sends a message to all connected clients, using `Clients.All`.</span></span>
* <span data-ttu-id="57087-169">`SendMessageToCaller` envoie un message à l’appelant, à l’aide de `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="57087-169">`SendMessageToCaller` sends a message back to the caller, using `Clients.Caller`.</span></span>
* <span data-ttu-id="57087-170">`SendMessageToGroups` envoie un message à tous les clients dans le `SignalR Users` groupe.</span><span class="sxs-lookup"><span data-stu-id="57087-170">`SendMessageToGroups` sends a message to all clients in the `SignalR Users` group.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?name=HubMethods)]

## <a name="strongly-typed-hubs"></a><span data-ttu-id="57087-171">Hubs fortement typés</span><span class="sxs-lookup"><span data-stu-id="57087-171">Strongly typed hubs</span></span>

<span data-ttu-id="57087-172">Un inconvénient de l’utilisation de `SendAsync` est qu’elle s’appuie sur une chaîne en dur pour spécifier la méthode de client à appeler.</span><span class="sxs-lookup"><span data-stu-id="57087-172">A drawback of using `SendAsync` is that it relies on a magic string to specify the client method to be called.</span></span> <span data-ttu-id="57087-173">Cela laisse le code ouvert à des erreurs d’exécution si le nom de la méthode est mal orthographié ou manquant à partir du client.</span><span class="sxs-lookup"><span data-stu-id="57087-173">This leaves code open to runtime errors if the method name is misspelled or missing from the client.</span></span>

<span data-ttu-id="57087-174">Une alternative à l’utilisation de `SendAsync` est pour typer fortement les `Hub` avec <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span><span class="sxs-lookup"><span data-stu-id="57087-174">An alternative to using `SendAsync` is to strongly type the `Hub` with <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span></span> <span data-ttu-id="57087-175">Dans l’exemple suivant, le `ChatHub` méthodes client ont été extraite et placées dans une interface appelée `IChatClient`.</span><span class="sxs-lookup"><span data-stu-id="57087-175">In the following example, the `ChatHub` client methods have been extracted out into an interface called `IChatClient`.</span></span>  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

<span data-ttu-id="57087-176">Cette interface peut être utilisée pour refactoriser l’exemple précédent `ChatHub` .</span><span class="sxs-lookup"><span data-stu-id="57087-176">This interface can be used to refactor the preceding `ChatHub` example.</span></span>

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

<span data-ttu-id="57087-177">À l’aide de `Hub<IChatClient>` Active la vérification de la compilation des méthodes client.</span><span class="sxs-lookup"><span data-stu-id="57087-177">Using `Hub<IChatClient>` enables compile-time checking of the client methods.</span></span> <span data-ttu-id="57087-178">Cela évite les problèmes provoqués par l’utilisation de chaînes magiques, étant donné que `Hub<T>` permettent uniquement d’accéder aux méthodes définies dans l’interface.</span><span class="sxs-lookup"><span data-stu-id="57087-178">This prevents issues caused by using magic strings, since `Hub<T>` can only provide access to the methods defined in the interface.</span></span>

<span data-ttu-id="57087-179">Utiliser un `Hub<T>` fortement typé désactive la possibilité d’utiliser `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="57087-179">Using a strongly typed `Hub<T>` disables the ability to use `SendAsync`.</span></span>

## <a name="change-the-name-of-a-hub-method"></a><span data-ttu-id="57087-180">Modifier le nom d’une méthode de concentrateur</span><span class="sxs-lookup"><span data-stu-id="57087-180">Change the name of a hub method</span></span>

<span data-ttu-id="57087-181">Par défaut, un nom de méthode de concentrateur de serveur est le nom de la méthode .NET.</span><span class="sxs-lookup"><span data-stu-id="57087-181">By default, a server hub method name is the name of the .NET method.</span></span> <span data-ttu-id="57087-182">Toutefois, vous pouvez utiliser la [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) attribut à modifier cette valeur par défaut et spécifier manuellement un nom pour la méthode.</span><span class="sxs-lookup"><span data-stu-id="57087-182">However, you can use the [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) attribute to change this default and manually specify a name for the method.</span></span> <span data-ttu-id="57087-183">Le client doit utiliser ce nom, au lieu du nom de méthode .NET, lors de l’appel de la méthode.</span><span class="sxs-lookup"><span data-stu-id="57087-183">The client should use this name, instead of the .NET method name, when invoking the method.</span></span>

[!code-csharp[HubMethodName attribute](hubs/sample/hubs/chathub.cs?name=HubMethodName&highlight=1)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="57087-184">Gérer les événements pour une connexion</span><span class="sxs-lookup"><span data-stu-id="57087-184">Handle events for a connection</span></span>

<span data-ttu-id="57087-185">L’API Hubs de SignalR fournit les méthodes virtuelles `OnConnectedAsync` et `OnDisconnectedAsync` pour gérer et suivre les connexions.</span><span class="sxs-lookup"><span data-stu-id="57087-185">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="57087-186">Remplacez la méthode virtuelle `OnConnectedAsync` pour effectuer des actions lorsqu’un client se connecte au hub, comme l’ajouter à un groupe.
</span><span class="sxs-lookup"><span data-stu-id="57087-186">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle connection](hubs/sample/hubs/chathub.cs?name=OnConnectedAsync)]

<span data-ttu-id="57087-187">Remplacer le `OnDisconnectedAsync` une méthode virtuelle pour effectuer des actions lorsqu’un client se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="57087-187">Override the `OnDisconnectedAsync` virtual method to perform actions when a client disconnects.</span></span> <span data-ttu-id="57087-188">Si le client se déconnecte volontairement (en appelant `connection.stop()`, par exemple), le `exception` paramètre sera `null`.</span><span class="sxs-lookup"><span data-stu-id="57087-188">If the client disconnects intentionally (by calling `connection.stop()`, for example), the `exception` parameter will be `null`.</span></span> <span data-ttu-id="57087-189">Toutefois, si le client est déconnecté à cause d’une erreur (par exemple, une panne de réseau), le `exception` paramètre contiendra une exception qui décrit l’échec.</span><span class="sxs-lookup"><span data-stu-id="57087-189">However, if the client is disconnected due to an error (such as a network failure), the `exception` parameter will contain an exception describing the failure.</span></span>

[!code-csharp[Handle disconnection](hubs/sample/hubs/chathub.cs?name=OnDisconnectedAsync)]

## <a name="handle-errors"></a><span data-ttu-id="57087-190">Gérer les erreurs</span><span class="sxs-lookup"><span data-stu-id="57087-190">Handle errors</span></span>

<span data-ttu-id="57087-191">Les exceptions levées dans vos méthodes de hub sont envoyées au client qui a appelé la méthode.</span><span class="sxs-lookup"><span data-stu-id="57087-191">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="57087-192">Sur le client JavaScript, la méthode `invoke` retourne une [promesse JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="57087-192">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="57087-193">Lorsque le client reçoit une erreur avec un gestionnaire attaché à la promesse avec `catch`, elle est appelée et passée en tant qu’objet JavaScript `Error`.
</span><span class="sxs-lookup"><span data-stu-id="57087-193">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

<span data-ttu-id="57087-194">Par défaut, si votre Hub lève une exception, SignalR retourne un message d’erreur générique au client.</span><span class="sxs-lookup"><span data-stu-id="57087-194">By default, if your Hub throws an exception, SignalR returns a generic error message to the client.</span></span> <span data-ttu-id="57087-195">Exemple :</span><span class="sxs-lookup"><span data-stu-id="57087-195">For example:</span></span>

```
Microsoft.AspNetCore.SignalR.HubException: An unexpected error occurred invoking 'MethodName' on the server.
```

<span data-ttu-id="57087-196">Exceptions inattendues contiennent souvent des informations sensibles, telles que le nom d’un serveur de base de données dans une exception déclenchée en cas d’échec de la connexion de base de données.</span><span class="sxs-lookup"><span data-stu-id="57087-196">Unexpected exceptions often contain sensitive information, such as the name of a database server in an exception triggered when the database connection fails.</span></span> <span data-ttu-id="57087-197">SignalR n’expose pas ces messages d’erreur détaillés par défaut par mesure de sécurité.</span><span class="sxs-lookup"><span data-stu-id="57087-197">SignalR doesn't expose these detailed error messages by default as a security measure.</span></span> <span data-ttu-id="57087-198">Consultez le [article considérations de sécurité](xref:signalr/security#exceptions) pour plus d’informations sur la raison pour laquelle les détails de l’exception sont supprimés.</span><span class="sxs-lookup"><span data-stu-id="57087-198">See the [Security considerations article](xref:signalr/security#exceptions) for more information on why exception details are suppressed.</span></span>

<span data-ttu-id="57087-199">Si vous avez une exceptionnelle condition vous *faire* à propager vers le client, vous pouvez utiliser la `HubException` classe.</span><span class="sxs-lookup"><span data-stu-id="57087-199">If you have an exceptional condition you *do* want to propagate to the client, you can use the `HubException` class.</span></span> <span data-ttu-id="57087-200">Si vous levez une `HubException` à partir de votre méthode de concentrateur, SignalR **sera** envoyer l’intégralité du message au client, tels quels.</span><span class="sxs-lookup"><span data-stu-id="57087-200">If you throw a `HubException` from your hub method, SignalR **will** send the entire message to the client, unmodified.</span></span>

[!code-csharp[ThrowHubException](hubs/sample/hubs/chathub.cs?name=ThrowHubException&highlight=3)]

> [!NOTE]
> <span data-ttu-id="57087-201">SignalR envoie uniquement le `Message` propriété de l’exception au client.</span><span class="sxs-lookup"><span data-stu-id="57087-201">SignalR only sends the `Message` property of the exception to the client.</span></span> <span data-ttu-id="57087-202">La trace de pile et d’autres propriétés sur l’exception ne sont pas disponibles pour le client.</span><span class="sxs-lookup"><span data-stu-id="57087-202">The stack trace and other properties on the exception aren't available to the client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="57087-203">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="57087-203">Related resources</span></span>

* [<span data-ttu-id="57087-204">Introduction à ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="57087-204">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="57087-205">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="57087-205">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="57087-206">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="57087-206">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
