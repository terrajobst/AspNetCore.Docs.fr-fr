---
title: Gérer les utilisateurs et des groupes dans SignalR
author: tdykstra
description: Vue d’ensemble de la gestion des utilisateurs et des groupes dans ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 02db46f090c487a03171de244ff7ad0d5e9de0fa
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758165"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="4df63-103">Gérer les utilisateurs et des groupes dans SignalR</span><span class="sxs-lookup"><span data-stu-id="4df63-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="4df63-104">Par [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="4df63-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="4df63-105">SignalR permet d'envoyer des messages à toutes les connexions associées à un utilisateur spécifique, mais aussi à des groupes de connexions nommés.</span><span class="sxs-lookup"><span data-stu-id="4df63-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="4df63-106">[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(comment télécharger)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="4df63-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="4df63-107">Utilisateurs dans SignalR</span><span class="sxs-lookup"><span data-stu-id="4df63-107">Users in SignalR</span></span>

<span data-ttu-id="4df63-108">SignalR vous permet d’envoyer des messages à toutes les connexions associées à un utilisateur spécifique.</span><span class="sxs-lookup"><span data-stu-id="4df63-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="4df63-109">Par défaut, SignalR utilise le `ClaimTypes.NameIdentifier` du `ClaimsPrincipal` associé à la connexion comme identificateur d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="4df63-109">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="4df63-110">Un utilisateur peut avoir plusieurs connexions à une application SignalR.</span><span class="sxs-lookup"><span data-stu-id="4df63-110">A single user can have multiple connections to a SignalR app.</span></span> <span data-ttu-id="4df63-111">Par exemple, il peut être connecté sur son ordinateur de bureau et sur son téléphone.</span><span class="sxs-lookup"><span data-stu-id="4df63-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="4df63-112">Chaque appareil a une connexion SignalR distincte, mais ils sont tous associés au même utilisateur.</span><span class="sxs-lookup"><span data-stu-id="4df63-112">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="4df63-113">Si un message est envoyé à l’utilisateur, toutes les connexions associées à cet utilisateur reçoivent le message.</span><span class="sxs-lookup"><span data-stu-id="4df63-113">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="4df63-114">L’identificateur d’utilisateur pour une connexion est accessible au moyen de la propriété `Context.UserIdentifier` dans votre hub.</span><span class="sxs-lookup"><span data-stu-id="4df63-114">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in your hub.</span></span>

<span data-ttu-id="4df63-115">Envoyez un message à un utilisateur spécifique en passant l’identificateur d’utilisateur à la fonction `User` dans la méthode de votre hub, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="4df63-115">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="4df63-116">L’identificateur d’utilisateur respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="4df63-116">The user identifier is case-sensitive.</span></span>

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

<span data-ttu-id="4df63-117">L’identificateur d’utilisateur peut être personnalisé en créant un `IUserIdProvider`et en l’inscrivant dans `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4df63-117">The user identifier can be customized by creating an `IUserIdProvider`, and registering it in `ConfigureServices`.</span></span>

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> <span data-ttu-id="4df63-118">AddSignalR doit être appelée avant l’inscription de vos services SignalR personnalisées.</span><span class="sxs-lookup"><span data-stu-id="4df63-118">AddSignalR must be called before registering your custom SignalR services.</span></span>

## <a name="groups-in-signalr"></a><span data-ttu-id="4df63-119">Groupes dans SignalR</span><span class="sxs-lookup"><span data-stu-id="4df63-119">Groups in SignalR</span></span>

<span data-ttu-id="4df63-120">Un groupe est une collection de connexions associées à un nom.</span><span class="sxs-lookup"><span data-stu-id="4df63-120">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="4df63-121">Les messages peuvent être envoyés à toutes les connexions dans un groupe.</span><span class="sxs-lookup"><span data-stu-id="4df63-121">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="4df63-122">Les groupes sont recommandés pour envoyer des messages à une ou plusieurs connexions, car ils sont gérés par l’application.</span><span class="sxs-lookup"><span data-stu-id="4df63-122">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="4df63-123">Une connexion peut appartenir à plusieurs groupes.</span><span class="sxs-lookup"><span data-stu-id="4df63-123">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="4df63-124">Les groupes conviennent donc parfaitement à certaines choses, notamment les applications de conversation dans lesquelles chaque salle peut être représentée par un groupe.</span><span class="sxs-lookup"><span data-stu-id="4df63-124">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="4df63-125">Les connexions peuvent être ajoutées à des groupes ou supprimées de ceux-ci à l'aide des méthodes `AddToGroupAsync` et `RemoveFromGroupAsync`.</span><span class="sxs-lookup"><span data-stu-id="4df63-125">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="4df63-126">L’appartenance au groupe n’est pas conservée quand une connexion se reconnecte.</span><span class="sxs-lookup"><span data-stu-id="4df63-126">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="4df63-127">La connexion doit rejoindre le groupe quand elle est rétablie.</span><span class="sxs-lookup"><span data-stu-id="4df63-127">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="4df63-128">Il n’est pas possible de compter les membres d’un groupe, car ces informations ne sont pas disponibles si l’application est mise à l'échelle sur plusieurs serveurs.</span><span class="sxs-lookup"><span data-stu-id="4df63-128">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

<span data-ttu-id="4df63-129">Pour protéger l’accès aux ressources lors de l’utilisation de groupes, utilisez [authentification et autorisation](xref:signalr/authn-and-authz) fonctionnalités dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4df63-129">To protect access to resources while using groups, use [authentication and authorization](xref:signalr/authn-and-authz) functionality in ASP.NET Core.</span></span> <span data-ttu-id="4df63-130">Si vous ajoutez uniquement des utilisateurs à un groupe lorsque les informations d’identification sont valides pour ce groupe, les messages envoyés à ce groupe passera uniquement aux utilisateurs autorisés.</span><span class="sxs-lookup"><span data-stu-id="4df63-130">If you only add users to a group when the credentials are valid for that group, messages sent to that group will only go to authorized users.</span></span> <span data-ttu-id="4df63-131">Toutefois, les groupes ne sont pas une fonctionnalité de sécurité.</span><span class="sxs-lookup"><span data-stu-id="4df63-131">However, groups are not a security feature.</span></span> <span data-ttu-id="4df63-132">L’authentification par revendications possèdent des fonctionnalités groupes ne le faites pas, telles que l’expiration et la révocation.</span><span class="sxs-lookup"><span data-stu-id="4df63-132">Authentication claims have features that groups do not, such as expiry and revocation.</span></span> <span data-ttu-id="4df63-133">Si l’autorisation d’un utilisateur pour accéder au groupe est révoquée, vous devez manuellement détecte et le supprimer du groupe.</span><span class="sxs-lookup"><span data-stu-id="4df63-133">If a user's permission to access the group is revoked, you have to manually detect that and remove them from the group.</span></span>

> [!NOTE]
> <span data-ttu-id="4df63-134">Les noms de groupe respectent la casse.</span><span class="sxs-lookup"><span data-stu-id="4df63-134">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="4df63-135">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="4df63-135">Related resources</span></span>

* [<span data-ttu-id="4df63-136">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="4df63-136">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="4df63-137">Hubs</span><span class="sxs-lookup"><span data-stu-id="4df63-137">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="4df63-138">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="4df63-138">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
