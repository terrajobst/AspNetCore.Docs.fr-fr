---
title: Gérer les utilisateurs et groupes dans SignalR
author: rachelappel
description: Vue d’ensemble de gestion ASP.NET Core SignalR utilisateur et de groupe.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: f7d60a906fc238f79c76fd2a4ee693417a348825
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272079"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="aab83-103">Gérer les utilisateurs et groupes dans SignalR</span><span class="sxs-lookup"><span data-stu-id="aab83-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="aab83-104">Par [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="aab83-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="aab83-105">SignalR permet aux messages à envoyer à toutes les connexions associées à un utilisateur spécifique, ainsi que pour les groupes de connexions nommés.</span><span class="sxs-lookup"><span data-stu-id="aab83-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="aab83-106">[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(comment télécharger)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="aab83-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="aab83-107">Utilisateurs de SignalR</span><span class="sxs-lookup"><span data-stu-id="aab83-107">Users in SignalR</span></span>

<span data-ttu-id="aab83-108">SignalR vous permet d’envoyer des messages à toutes les connexions associées à un utilisateur spécifique.</span><span class="sxs-lookup"><span data-stu-id="aab83-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="aab83-109">Par défaut SignalR utilise le `ClaimTypes.NameIdentifier` à partir de la `ClaimsPrincipal` associé à la connexion en tant qu’identificateur d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="aab83-109">By default SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="aab83-110">Un seul utilisateur peut avoir plusieurs connexions à une application SignalR.</span><span class="sxs-lookup"><span data-stu-id="aab83-110">A single user can have multiple connections to a SignalR application.</span></span> <span data-ttu-id="aab83-111">Par exemple, un utilisateur peut être connecté sur leur bureau, ainsi que leur numéro de téléphone.</span><span class="sxs-lookup"><span data-stu-id="aab83-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="aab83-112">Chaque appareil possède une connexion SignalR distincte, mais ils sont tous associés à l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="aab83-112">Each device has a separate SignalR connection, but they are all associated with the same user.</span></span> <span data-ttu-id="aab83-113">Si un message est envoyé à l’utilisateur, toutes les connexions associées à cet utilisateur recevra le message.</span><span class="sxs-lookup"><span data-stu-id="aab83-113">If a message is sent to the user, all of the connections associated with that user will receive the message.</span></span>

<span data-ttu-id="aab83-114">Envoyer un message à un utilisateur spécifique en passant l’identificateur d’utilisateur pour le `User` fonctionner dans votre méthode de concentrateur, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="aab83-114">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="aab83-115">L’identificateur d’utilisateur respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="aab83-115">The user identifier is case-sensitive.</span></span>

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

<span data-ttu-id="aab83-116">L’identificateur d’utilisateur peut être personnalisé en créant un `IUserIdProvider`et de son inscription dans `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="aab83-116">The user identifier can be customized by creating an `IUserIdProvider`, and registering it in `ConfigureServices`.</span></span>

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> <span data-ttu-id="aab83-117">AddSignalR doit être appelé avant l’inscription de vos services SignalR personnalisées.</span><span class="sxs-lookup"><span data-stu-id="aab83-117">AddSignalR must be called before registering your custom SignalR services.</span></span>

## <a name="groups-in-signalr"></a><span data-ttu-id="aab83-118">Groupes dans SignalR</span><span class="sxs-lookup"><span data-stu-id="aab83-118">Groups in SignalR</span></span>

<span data-ttu-id="aab83-119">Un groupe est une collection de connexions associé à un nom.</span><span class="sxs-lookup"><span data-stu-id="aab83-119">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="aab83-120">Les messages peuvent être envoyés à toutes les connexions dans un groupe.</span><span class="sxs-lookup"><span data-stu-id="aab83-120">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="aab83-121">Les groupes sont la méthode recommandée pour l’envoyer à une connexion ou de plusieurs connexions, car les groupes sont gérés par l’application.</span><span class="sxs-lookup"><span data-stu-id="aab83-121">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="aab83-122">Une connexion peut être un membre de plusieurs groupes.</span><span class="sxs-lookup"><span data-stu-id="aab83-122">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="aab83-123">Cela rend les groupes idéale pour quelque chose comme une application de conversation, où chaque pièce peut être représenté en tant que groupe.</span><span class="sxs-lookup"><span data-stu-id="aab83-123">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="aab83-124">Connexions peuvent être ajoutées ou supprimées à partir de groupes via le `AddToGroupAsync` et `RemoveFromGroupAsync` méthodes.</span><span class="sxs-lookup"><span data-stu-id="aab83-124">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="aab83-125">L’appartenance au groupe n’est pas conservé lorsqu’une connexion se reconnecte.</span><span class="sxs-lookup"><span data-stu-id="aab83-125">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="aab83-126">La connexion doit rejoindre le groupe lorsqu’il est rétablie.</span><span class="sxs-lookup"><span data-stu-id="aab83-126">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="aab83-127">Il n’est pas possible de compter les membres d’un groupe, étant donné que ces informations ne sont pas disponibles si l’application est redimensionnée à plusieurs serveurs.</span><span class="sxs-lookup"><span data-stu-id="aab83-127">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

> [!NOTE]
> <span data-ttu-id="aab83-128">Les noms de groupe respectent la casse.</span><span class="sxs-lookup"><span data-stu-id="aab83-128">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="aab83-129">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="aab83-129">Related resources</span></span>

* [<span data-ttu-id="aab83-130">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="aab83-130">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="aab83-131">Hubs</span><span class="sxs-lookup"><span data-stu-id="aab83-131">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="aab83-132">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="aab83-132">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
