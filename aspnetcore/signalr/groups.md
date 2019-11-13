---
title: Gérer les utilisateurs et les groupes dans SignalR
author: bradygaster
description: Vue d’ensemble de la gestion des utilisateurs et des groupes ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/groups
ms.openlocfilehash: 59e90042ecbaf936602643bbdc3965e036426b26
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963810"
---
# <a name="manage-users-and-groups-in-opno-locsignalr"></a><span data-ttu-id="9dd22-103">Gérer les utilisateurs et les groupes dans SignalR</span><span class="sxs-lookup"><span data-stu-id="9dd22-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="9dd22-104">Par [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="9dd22-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

SignalR<span data-ttu-id="9dd22-105"> permet d’envoyer des messages à toutes les connexions associées à un utilisateur spécifique, ainsi qu’à des groupes nommés de connexions.</span><span class="sxs-lookup"><span data-stu-id="9dd22-105"> allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="9dd22-106">[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/groups/sample/) [(procédure de téléchargement)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="9dd22-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="users-in-opno-locsignalr"></a><span data-ttu-id="9dd22-107">Utilisateurs dans SignalR</span><span class="sxs-lookup"><span data-stu-id="9dd22-107">Users in SignalR</span></span>

SignalR<span data-ttu-id="9dd22-108"> vous permet d’envoyer des messages à toutes les connexions associées à un utilisateur spécifique.</span><span class="sxs-lookup"><span data-stu-id="9dd22-108"> allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="9dd22-109">Par défaut, SignalR utilise le `ClaimTypes.NameIdentifier` à partir de la `ClaimsPrincipal` associée à la connexion en tant qu’identificateur de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="9dd22-109">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="9dd22-110">Un seul utilisateur peut avoir plusieurs connexions à une application SignalR.</span><span class="sxs-lookup"><span data-stu-id="9dd22-110">A single user can have multiple connections to a SignalR app.</span></span> <span data-ttu-id="9dd22-111">Par exemple, un utilisateur peut être connecté sur son bureau, ainsi que sur son téléphone.</span><span class="sxs-lookup"><span data-stu-id="9dd22-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="9dd22-112">Chaque appareil dispose d’une connexion SignalR distincte, mais elles sont toutes associées au même utilisateur.</span><span class="sxs-lookup"><span data-stu-id="9dd22-112">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="9dd22-113">Si un message est envoyé à l’utilisateur, toutes les connexions associées à cet utilisateur reçoivent le message.</span><span class="sxs-lookup"><span data-stu-id="9dd22-113">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="9dd22-114">L’identificateur d’utilisateur pour une connexion est accessible à l’aide de la propriété `Context.UserIdentifier` de votre Hub.</span><span class="sxs-lookup"><span data-stu-id="9dd22-114">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in your hub.</span></span>

<span data-ttu-id="9dd22-115">Envoyez un message à un utilisateur spécifique en passant l’identificateur d’utilisateur à la fonction `User` dans votre méthode de concentrateur, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="9dd22-115">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="9dd22-116">L’identificateur d’utilisateur respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="9dd22-116">The user identifier is case-sensitive.</span></span>

[!code-csharp[Configure service](groups/sample/hubs/chathub.cs?range=29-32)]

## <a name="groups-in-opno-locsignalr"></a><span data-ttu-id="9dd22-117">Groupes dans SignalR</span><span class="sxs-lookup"><span data-stu-id="9dd22-117">Groups in SignalR</span></span>

<span data-ttu-id="9dd22-118">Un groupe est une collection de connexions associées à un nom.</span><span class="sxs-lookup"><span data-stu-id="9dd22-118">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="9dd22-119">Les messages peuvent être envoyés à toutes les connexions d’un groupe.</span><span class="sxs-lookup"><span data-stu-id="9dd22-119">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="9dd22-120">Les groupes sont la méthode recommandée pour l’envoi à une connexion ou à plusieurs connexions, car les groupes sont gérés par l’application.</span><span class="sxs-lookup"><span data-stu-id="9dd22-120">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="9dd22-121">Une connexion peut être membre de plusieurs groupes.</span><span class="sxs-lookup"><span data-stu-id="9dd22-121">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="9dd22-122">Cela rend les groupes idéaux pour une application de conversation, où chaque pièce peut être représentée en tant que groupe.</span><span class="sxs-lookup"><span data-stu-id="9dd22-122">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="9dd22-123">Les connexions peuvent être ajoutées ou supprimées des groupes via les méthodes `AddToGroupAsync` et `RemoveFromGroupAsync`.</span><span class="sxs-lookup"><span data-stu-id="9dd22-123">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="9dd22-124">L’appartenance au groupe n’est pas conservée lorsqu’une connexion se reconnecte.</span><span class="sxs-lookup"><span data-stu-id="9dd22-124">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="9dd22-125">La connexion doit rejoindre le groupe lorsqu’elle est rétablie.</span><span class="sxs-lookup"><span data-stu-id="9dd22-125">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="9dd22-126">Il n’est pas possible de compter les membres d’un groupe, car ces informations ne sont pas disponibles si l’application est mise à l’échelle sur plusieurs serveurs.</span><span class="sxs-lookup"><span data-stu-id="9dd22-126">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

<span data-ttu-id="9dd22-127">Pour protéger l’accès aux ressources lors de l’utilisation de groupes, utilisez la fonctionnalité d' [authentification et d’autorisation](xref:signalr/authn-and-authz) dans ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="9dd22-127">To protect access to resources while using groups, use [authentication and authorization](xref:signalr/authn-and-authz) functionality in ASP.NET Core.</span></span> <span data-ttu-id="9dd22-128">Si vous ajoutez uniquement des utilisateurs à un groupe lorsque les informations d’identification sont valides pour ce groupe, les messages envoyés à ce groupe sont uniquement dirigés vers les utilisateurs autorisés.</span><span class="sxs-lookup"><span data-stu-id="9dd22-128">If you only add users to a group when the credentials are valid for that group, messages sent to that group will only go to authorized users.</span></span> <span data-ttu-id="9dd22-129">Toutefois, les groupes ne sont pas une fonctionnalité de sécurité.</span><span class="sxs-lookup"><span data-stu-id="9dd22-129">However, groups are not a security feature.</span></span> <span data-ttu-id="9dd22-130">Les revendications d’authentification ont des fonctionnalités que les groupes n’ont pas, telles que l’expiration et la révocation.</span><span class="sxs-lookup"><span data-stu-id="9dd22-130">Authentication claims have features that groups do not, such as expiry and revocation.</span></span> <span data-ttu-id="9dd22-131">Si l’autorisation d’un utilisateur d’accéder au groupe est révoquée, vous devez la détecter manuellement et la supprimer du groupe.</span><span class="sxs-lookup"><span data-stu-id="9dd22-131">If a user's permission to access the group is revoked, you have to manually detect that and remove them from the group.</span></span>

> [!NOTE]
> <span data-ttu-id="9dd22-132">Les noms de groupes respectent la casse.</span><span class="sxs-lookup"><span data-stu-id="9dd22-132">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="9dd22-133">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="9dd22-133">Related resources</span></span>

* [<span data-ttu-id="9dd22-134">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="9dd22-134">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="9dd22-135">Hubs</span><span class="sxs-lookup"><span data-stu-id="9dd22-135">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="9dd22-136">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="9dd22-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
