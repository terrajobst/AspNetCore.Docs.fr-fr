---
title: Gérer les utilisateurs et des groupes dans SignalR
author: tdykstra
description: Vue d’ensemble de la gestion des utilisateurs et des groupes dans ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: d3e580dfc42a36762358899892831c8b68f544b0
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207158"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="72a8d-103">Gérer les utilisateurs et des groupes dans SignalR</span><span class="sxs-lookup"><span data-stu-id="72a8d-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="72a8d-104">Par [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="72a8d-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="72a8d-105">SignalR permet d'envoyer des messages à toutes les connexions associées à un utilisateur spécifique, mais aussi à des groupes de connexions nommés.</span><span class="sxs-lookup"><span data-stu-id="72a8d-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="72a8d-106">[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(comment télécharger)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="72a8d-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="72a8d-107">Utilisateurs dans SignalR</span><span class="sxs-lookup"><span data-stu-id="72a8d-107">Users in SignalR</span></span>

<span data-ttu-id="72a8d-108">SignalR vous permet d’envoyer des messages à toutes les connexions associées à un utilisateur spécifique.</span><span class="sxs-lookup"><span data-stu-id="72a8d-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="72a8d-109">Par défaut, SignalR utilise le `ClaimTypes.NameIdentifier` du `ClaimsPrincipal` associé à la connexion comme identificateur d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="72a8d-109">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="72a8d-110">Un utilisateur peut avoir plusieurs connexions à une application SignalR.</span><span class="sxs-lookup"><span data-stu-id="72a8d-110">A single user can have multiple connections to a SignalR app.</span></span> <span data-ttu-id="72a8d-111">Par exemple, il peut être connecté sur son ordinateur de bureau et sur son téléphone.</span><span class="sxs-lookup"><span data-stu-id="72a8d-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="72a8d-112">Chaque appareil a une connexion SignalR distincte, mais ils sont tous associés au même utilisateur.</span><span class="sxs-lookup"><span data-stu-id="72a8d-112">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="72a8d-113">Si un message est envoyé à l’utilisateur, toutes les connexions associées à cet utilisateur reçoivent le message.</span><span class="sxs-lookup"><span data-stu-id="72a8d-113">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="72a8d-114">L’identificateur d’utilisateur pour une connexion est accessible au moyen de la propriété `Context.UserIdentifier` dans votre hub.</span><span class="sxs-lookup"><span data-stu-id="72a8d-114">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in your hub.</span></span>

<span data-ttu-id="72a8d-115">Envoyez un message à un utilisateur spécifique en passant l’identificateur d’utilisateur à la fonction `User` dans la méthode de votre hub, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="72a8d-115">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="72a8d-116">L’identificateur d’utilisateur respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="72a8d-116">The user identifier is case-sensitive.</span></span>

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

<span data-ttu-id="72a8d-117">L’identificateur d’utilisateur peut être personnalisé en créant un `IUserIdProvider`et en l’inscrivant dans `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="72a8d-117">The user identifier can be customized by creating an `IUserIdProvider`, and registering it in `ConfigureServices`.</span></span>

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> <span data-ttu-id="72a8d-118">AddSignalR doit être appelée avant l’inscription de vos services SignalR personnalisées.</span><span class="sxs-lookup"><span data-stu-id="72a8d-118">AddSignalR must be called before registering your custom SignalR services.</span></span>

## <a name="groups-in-signalr"></a><span data-ttu-id="72a8d-119">Groupes dans SignalR</span><span class="sxs-lookup"><span data-stu-id="72a8d-119">Groups in SignalR</span></span>

<span data-ttu-id="72a8d-120">Un groupe est une collection de connexions associées à un nom.</span><span class="sxs-lookup"><span data-stu-id="72a8d-120">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="72a8d-121">Les messages peuvent être envoyés à toutes les connexions dans un groupe.</span><span class="sxs-lookup"><span data-stu-id="72a8d-121">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="72a8d-122">Les groupes sont recommandés pour envoyer des messages à une ou plusieurs connexions, car ils sont gérés par l’application.</span><span class="sxs-lookup"><span data-stu-id="72a8d-122">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="72a8d-123">Une connexion peut appartenir à plusieurs groupes.</span><span class="sxs-lookup"><span data-stu-id="72a8d-123">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="72a8d-124">Les groupes conviennent donc parfaitement à certaines choses, notamment les applications de conversation dans lesquelles chaque salle peut être représentée par un groupe.</span><span class="sxs-lookup"><span data-stu-id="72a8d-124">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="72a8d-125">Les connexions peuvent être ajoutées à des groupes ou supprimées de ceux-ci à l'aide des méthodes `AddToGroupAsync` et `RemoveFromGroupAsync`.</span><span class="sxs-lookup"><span data-stu-id="72a8d-125">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="72a8d-126">L’appartenance au groupe n’est pas conservée quand une connexion se reconnecte.</span><span class="sxs-lookup"><span data-stu-id="72a8d-126">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="72a8d-127">La connexion doit rejoindre le groupe quand elle est rétablie.</span><span class="sxs-lookup"><span data-stu-id="72a8d-127">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="72a8d-128">Il n’est pas possible de compter les membres d’un groupe, car ces informations ne sont pas disponibles si l’application est mise à l'échelle sur plusieurs serveurs.</span><span class="sxs-lookup"><span data-stu-id="72a8d-128">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

> [!NOTE]
> <span data-ttu-id="72a8d-129">Les noms de groupe respectent la casse.</span><span class="sxs-lookup"><span data-stu-id="72a8d-129">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="72a8d-130">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="72a8d-130">Related resources</span></span>

* [<span data-ttu-id="72a8d-131">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="72a8d-131">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="72a8d-132">Hubs</span><span class="sxs-lookup"><span data-stu-id="72a8d-132">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="72a8d-133">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="72a8d-133">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
