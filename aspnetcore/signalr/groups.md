---
title: Gérer les utilisateurs et groupes dans SignalR
author: tdykstra
description: Vue d’ensemble d’ASP.NET Core SignalR utilisateur et groupe de gestion.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: d54ab2a113345f98e26425a88cad165d67b8d456
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095019"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="95d1c-103">Gérer les utilisateurs et groupes dans SignalR</span><span class="sxs-lookup"><span data-stu-id="95d1c-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="95d1c-104">Par [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="95d1c-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="95d1c-105">SignalR permet des messages à envoyer à toutes les connexions associées à un utilisateur spécifique, mais aussi à des groupes nommés de connexions.</span><span class="sxs-lookup"><span data-stu-id="95d1c-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="95d1c-106">[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(comment télécharger)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="95d1c-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="95d1c-107">Utilisateurs dans SignalR</span><span class="sxs-lookup"><span data-stu-id="95d1c-107">Users in SignalR</span></span>

<span data-ttu-id="95d1c-108">SignalR vous permet d’envoyer des messages à toutes les connexions associées à un utilisateur spécifique.</span><span class="sxs-lookup"><span data-stu-id="95d1c-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="95d1c-109">Par défaut, SignalR utilise le `ClaimTypes.NameIdentifier` à partir de la `ClaimsPrincipal` associé à la connexion comme identificateur d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="95d1c-109">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="95d1c-110">Un seul utilisateur peut avoir plusieurs connexions à une application de SignalR.</span><span class="sxs-lookup"><span data-stu-id="95d1c-110">A single user can have multiple connections to a SignalR app.</span></span> <span data-ttu-id="95d1c-111">Par exemple, un utilisateur peut être connecté sur leur bureau, ainsi que leur téléphone.</span><span class="sxs-lookup"><span data-stu-id="95d1c-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="95d1c-112">Chaque périphérique possède une connexion SignalR distincte, mais ils sont tous associés au même utilisateur.</span><span class="sxs-lookup"><span data-stu-id="95d1c-112">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="95d1c-113">Si un message est envoyé à l’utilisateur, toutes les connexions associées à cet utilisateur le message.</span><span class="sxs-lookup"><span data-stu-id="95d1c-113">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="95d1c-114">L’identificateur d’utilisateur pour une connexion est accessible par le `Context.UserIdentifier` propriété dans votre hub.</span><span class="sxs-lookup"><span data-stu-id="95d1c-114">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in your hub.</span></span>

<span data-ttu-id="95d1c-115">Envoyer un message à un utilisateur spécifique en passant l’identificateur d’utilisateur pour le `User` fonctionner dans votre méthode de concentrateur, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="95d1c-115">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="95d1c-116">L’identificateur d’utilisateur respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="95d1c-116">The user identifier is case-sensitive.</span></span>

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

<span data-ttu-id="95d1c-117">L’identificateur d’utilisateur peut être personnalisé en créant un `IUserIdProvider`et en l’inscrivant dans `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="95d1c-117">The user identifier can be customized by creating an `IUserIdProvider`, and registering it in `ConfigureServices`.</span></span>

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> <span data-ttu-id="95d1c-118">AddSignalR doit être appelée avant l’inscription de vos services SignalR personnalisées.</span><span class="sxs-lookup"><span data-stu-id="95d1c-118">AddSignalR must be called before registering your custom SignalR services.</span></span>

## <a name="groups-in-signalr"></a><span data-ttu-id="95d1c-119">Groupes dans SignalR</span><span class="sxs-lookup"><span data-stu-id="95d1c-119">Groups in SignalR</span></span>

<span data-ttu-id="95d1c-120">Un groupe est une collection de connexions associées à un nom.</span><span class="sxs-lookup"><span data-stu-id="95d1c-120">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="95d1c-121">Messages peuvent être envoyés à toutes les connexions dans un groupe.</span><span class="sxs-lookup"><span data-stu-id="95d1c-121">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="95d1c-122">Les groupes sont la méthode recommandée pour l’envoyer à une connexion ou de plusieurs connexions, car les groupes sont gérés par l’application.</span><span class="sxs-lookup"><span data-stu-id="95d1c-122">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="95d1c-123">Une connexion peut être un membre de plusieurs groupes.</span><span class="sxs-lookup"><span data-stu-id="95d1c-123">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="95d1c-124">Cela, les groupes de parfaits pour quelque chose comme une application de conversation, où chaque pièce peut être représenté en tant que groupe.</span><span class="sxs-lookup"><span data-stu-id="95d1c-124">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="95d1c-125">Connexions peuvent être ajoutées ou supprimées à partir de groupes via le `AddToGroupAsync` et `RemoveFromGroupAsync` méthodes.</span><span class="sxs-lookup"><span data-stu-id="95d1c-125">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="95d1c-126">L’appartenance au groupe n’est pas conservé lorsqu’une connexion se reconnecte.</span><span class="sxs-lookup"><span data-stu-id="95d1c-126">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="95d1c-127">La connexion doit rejoindre le groupe quand elle est rétablie.</span><span class="sxs-lookup"><span data-stu-id="95d1c-127">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="95d1c-128">Il n’est pas possible de compter les membres d’un groupe, étant donné que ces informations ne sont pas disponibles si l’application est à l’échelle vers plusieurs serveurs.</span><span class="sxs-lookup"><span data-stu-id="95d1c-128">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

> [!NOTE]
> <span data-ttu-id="95d1c-129">Les noms de groupe respectent la casse.</span><span class="sxs-lookup"><span data-stu-id="95d1c-129">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="95d1c-130">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="95d1c-130">Related resources</span></span>

* [<span data-ttu-id="95d1c-131">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="95d1c-131">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="95d1c-132">Hubs</span><span class="sxs-lookup"><span data-stu-id="95d1c-132">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="95d1c-133">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="95d1c-133">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
