---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: Utilisation de groupes dans SignalR | Documents Microsoft
author: pfletcher
description: Cette rubrique décrit comment conserver les informations d’appartenance au groupe avec l’API de concentrateur.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 11f5be1ac4e74b692f0db3daac971a2c9d74a64c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042220"
---
<a name="working-with-groups-in-signalr"></a><span data-ttu-id="9d71d-103">Utilisation de groupes dans SignalR</span><span class="sxs-lookup"><span data-stu-id="9d71d-103">Working with Groups in SignalR</span></span>
====================
<span data-ttu-id="9d71d-104">par [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="9d71d-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="9d71d-105">Cette rubrique décrit comment ajouter des utilisateurs aux groupes et de conserver les informations d’appartenance au groupe.</span><span class="sxs-lookup"><span data-stu-id="9d71d-105">This topic describes how to add users to groups and persist group membership information.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="9d71d-106">Versions du logiciel utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="9d71d-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="9d71d-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="9d71d-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="9d71d-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="9d71d-108">.NET 4.5</span></span>
> - <span data-ttu-id="9d71d-109">SignalR version 2</span><span class="sxs-lookup"><span data-stu-id="9d71d-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="9d71d-110">Versions précédentes de cette rubrique</span><span class="sxs-lookup"><span data-stu-id="9d71d-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="9d71d-111">Pour plus d’informations sur les versions antérieures de SignalR, consultez [SignalR des Versions antérieures](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="9d71d-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="9d71d-112">Questions et des commentaires</span><span class="sxs-lookup"><span data-stu-id="9d71d-112">Questions and comments</span></span>
> 
> <span data-ttu-id="9d71d-113">Veuillez laisser des commentaires sur la façon dont vous avez aimé ce didacticiel et nous pouvons améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="9d71d-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="9d71d-114">Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les valider pour le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="9d71d-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="9d71d-115">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="9d71d-115">Overview</span></span>

<span data-ttu-id="9d71d-116">Groupes dans SignalR fournissent une méthode pour diffuser des messages à des sous-ensembles spécifiés de clients connectés.</span><span class="sxs-lookup"><span data-stu-id="9d71d-116">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="9d71d-117">Un groupe peut avoir n’importe quel nombre de clients, et un client peut être un membre de n’importe quel nombre de groupes.</span><span class="sxs-lookup"><span data-stu-id="9d71d-117">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="9d71d-118">Vous n’êtes pas obligé de créer explicitement des groupes.</span><span class="sxs-lookup"><span data-stu-id="9d71d-118">You don't have to explicitly create groups.</span></span> <span data-ttu-id="9d71d-119">En effet, un groupe est créé automatiquement la première fois que vous spécifiez son nom dans un appel à Groups.Add, et il est supprimé lorsque vous supprimez la dernière connexion de l’appartenance à elle.</span><span class="sxs-lookup"><span data-stu-id="9d71d-119">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="9d71d-120">Pour une introduction à l’aide de groupes, consultez [comment gérer l’appartenance au groupe à partir de la classe de concentrateur](hubs-api-guide-server.md#groupsfromhub) dans l’API de concentrateurs - Guide de serveur.</span><span class="sxs-lookup"><span data-stu-id="9d71d-120">For an introduction to using groups, see [How to manage group membership from the Hub class](hubs-api-guide-server.md#groupsfromhub) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="9d71d-121">Il n’existe aucune API pour obtenir une liste de l’appartenance au groupe ou une liste de groupes.</span><span class="sxs-lookup"><span data-stu-id="9d71d-121">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="9d71d-122">SignalR envoie des messages pour les clients et les groupes basés sur un modèle pub/sub, et le serveur ne conserve pas les listes de groupes ou des appartenances aux groupes.</span><span class="sxs-lookup"><span data-stu-id="9d71d-122">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="9d71d-123">Cela permet d’optimiser l’évolutivité, car chaque fois que vous ajoutez un nœud à une batterie de serveurs web, tout état SignalR tient à jour doit être propagée vers le nouveau nœud.</span><span class="sxs-lookup"><span data-stu-id="9d71d-123">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="9d71d-124">Lorsque vous ajoutez un utilisateur à un groupe à l’aide de la `Groups.Add` méthode, l’utilisateur reçoit les messages vers ce groupe pour la durée de la connexion en cours, mais l’appartenance de ce groupe n’est pas rendue persistante au-delà de la connexion actuelle.</span><span class="sxs-lookup"><span data-stu-id="9d71d-124">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="9d71d-125">Si vous souhaitez conserver les informations sur les groupes et l’appartenance au groupe, vous devez stocker ces données dans un référentiel tel qu’une base de données ou le stockage de table Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="9d71d-125">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="9d71d-126">Ensuite, chaque fois qu'un utilisateur se connecte à votre application, vous récupérer à partir du référentiel, les groupes auxquels appartient l’utilisateur et ajouter manuellement cet utilisateur à ces groupes.</span><span class="sxs-lookup"><span data-stu-id="9d71d-126">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="9d71d-127">Lors de la reconnexion après une interruption temporaire, l’utilisateur nouveau joint automatiquement les groupes précédemment affectés.</span><span class="sxs-lookup"><span data-stu-id="9d71d-127">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="9d71d-128">Automatiquement rejoint un groupe s’applique uniquement lors de la reconnexion, ne pas lors de l’établissement d’une nouvelle connexion.</span><span class="sxs-lookup"><span data-stu-id="9d71d-128">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="9d71d-129">Un jeton de signature numérique est passé à partir du client qui contient la liste des groupes précédemment attribués.</span><span class="sxs-lookup"><span data-stu-id="9d71d-129">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="9d71d-130">Si vous souhaitez vérifier si l’utilisateur appartient aux groupes requis, vous pouvez remplacer le comportement par défaut.</span><span class="sxs-lookup"><span data-stu-id="9d71d-130">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="9d71d-131">Cette rubrique comporte les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="9d71d-131">This topic includes the following sections:</span></span>

- [<span data-ttu-id="9d71d-132">Ajout et suppression d’utilisateurs</span><span class="sxs-lookup"><span data-stu-id="9d71d-132">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="9d71d-133">Appel des membres d’un groupe</span><span class="sxs-lookup"><span data-stu-id="9d71d-133">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="9d71d-134">Appartenance au groupe de stockage dans une base de données</span><span class="sxs-lookup"><span data-stu-id="9d71d-134">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="9d71d-135">Appartenance au groupe de stockage dans le stockage table Azure</span><span class="sxs-lookup"><span data-stu-id="9d71d-135">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="9d71d-136">Vérification de l’appartenance au groupe lors de la reconnexion</span><span class="sxs-lookup"><span data-stu-id="9d71d-136">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="9d71d-137">Ajout et suppression d’utilisateurs</span><span class="sxs-lookup"><span data-stu-id="9d71d-137">Adding and removing users</span></span>

<span data-ttu-id="9d71d-138">Pour ajouter ou supprimer des utilisateurs dans un groupe, vous appelez le [ajouter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) ou [supprimer](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) méthodes et passez les id de connexion de l’utilisateur et le nom du groupe en tant que paramètres.</span><span class="sxs-lookup"><span data-stu-id="9d71d-138">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="9d71d-139">Vous n’avez pas besoin de supprimer manuellement un utilisateur à partir d’un groupe lors de la connexion se termine.</span><span class="sxs-lookup"><span data-stu-id="9d71d-139">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="9d71d-140">L’exemple suivant illustre la `Groups.Add` et `Groups.Remove` méthodes utilisées dans les méthodes de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="9d71d-140">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="9d71d-141">Le `Groups.Add` et `Groups.Remove` méthodes exécutent de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="9d71d-141">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="9d71d-142">Si vous souhaitez ajouter un client à un groupe et d’envoyer immédiatement un message au client en utilisant le groupe, vous devez vous assurer que la méthode Groups.Add se termine en premier.</span><span class="sxs-lookup"><span data-stu-id="9d71d-142">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="9d71d-143">Les exemples de code suivants montrent comment procéder.</span><span class="sxs-lookup"><span data-stu-id="9d71d-143">The following code examples show how to do that.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

<span data-ttu-id="9d71d-144">En règle générale, vous ne devez pas inclure `await` lors de l’appel du `Groups.Remove` (méthode), car l’id de connexion que vous essayez de supprimer ne soit plus disponible.</span><span class="sxs-lookup"><span data-stu-id="9d71d-144">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="9d71d-145">Dans ce cas, `TaskCanceledException` est levée après l’expiration de la demande. Si votre application doit garantir que l’utilisateur a été supprimé du groupe avant d’envoyer un message au groupe, vous pouvez ajouter `await` avant Groups.Remove et catch puis le `TaskCanceledException` exception peut être levée.</span><span class="sxs-lookup"><span data-stu-id="9d71d-145">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before Groups.Remove, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="9d71d-146">Appel des membres d’un groupe</span><span class="sxs-lookup"><span data-stu-id="9d71d-146">Calling members of a group</span></span>

<span data-ttu-id="9d71d-147">Vous pouvez envoyer des messages à tous les membres d’un groupe ou seuls les membres spécifiés du groupe, comme indiqué dans les exemples suivants.</span><span class="sxs-lookup"><span data-stu-id="9d71d-147">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="9d71d-148">**Tous les** connecté les clients dans un groupe spécifié.</span><span class="sxs-lookup"><span data-stu-id="9d71d-148">**All** connected clients in a specified group.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- <span data-ttu-id="9d71d-149">Tous les clients dans un groupe spécifié connectés **à l’exception des clients spécifiés**, identifié par l’ID de connexion.</span><span class="sxs-lookup"><span data-stu-id="9d71d-149">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span> 

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- <span data-ttu-id="9d71d-150">Tous les clients dans un groupe spécifié connectés **sauf le client appelant**.</span><span class="sxs-lookup"><span data-stu-id="9d71d-150">All connected clients in a specified group **except the calling client**.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="9d71d-151">Appartenance au groupe de stockage dans une base de données</span><span class="sxs-lookup"><span data-stu-id="9d71d-151">Storing group membership in a database</span></span>

<span data-ttu-id="9d71d-152">Les exemples suivants montrent comment conserver les informations utilisateur et de groupe dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="9d71d-152">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="9d71d-153">Vous pouvez utiliser n’importe quel technologie d’accès aux données ; Toutefois, l’exemple ci-dessous montre comment définir des modèles à l’aide d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9d71d-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="9d71d-154">Ces modèles d’entité correspondent aux champs et des tables de base de données.</span><span class="sxs-lookup"><span data-stu-id="9d71d-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="9d71d-155">Votre structure de données peut varier considérablement selon les besoins de votre application.</span><span class="sxs-lookup"><span data-stu-id="9d71d-155">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="9d71d-156">Cet exemple inclut une classe nommée `ConversationRoom` qui est unique à une application qui permet aux utilisateurs de joindre des conversations sur des sujets tels que sports ou jardinage.</span><span class="sxs-lookup"><span data-stu-id="9d71d-156">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="9d71d-157">Cet exemple inclut également une classe pour les connexions.</span><span class="sxs-lookup"><span data-stu-id="9d71d-157">This example also includes a class for the connections.</span></span> <span data-ttu-id="9d71d-158">La classe de connexion n’est pas absolument requise pour le suivi de l’appartenance au groupe, mais fait souvent partie d’une solution fiable pour le suivi des utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="9d71d-158">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

<span data-ttu-id="9d71d-159">Puis, dans le concentrateur, vous pouvez récupérer les informations de groupe et d’utilisateur de la base de données et ajouter manuellement l’utilisateur aux groupes appropriés.</span><span class="sxs-lookup"><span data-stu-id="9d71d-159">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="9d71d-160">L’exemple n’inclut pas de code pour le suivi des connexions utilisateur.</span><span class="sxs-lookup"><span data-stu-id="9d71d-160">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="9d71d-161">Dans cet exemple, le `await` mot clé n’est pas appliqué avant `Groups.Add` , car un message n’est pas immédiatement envoyé aux membres du groupe.</span><span class="sxs-lookup"><span data-stu-id="9d71d-161">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="9d71d-162">Si vous souhaitez envoyer un message à tous les membres du groupe immédiatement après l’ajout du nouveau membre, il serez que vous souhaitez appliquer la `await` (mot clé) pour vous assurer que l’opération asynchrone est terminée.</span><span class="sxs-lookup"><span data-stu-id="9d71d-162">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="9d71d-163">Appartenance au groupe de stockage dans le stockage table Azure</span><span class="sxs-lookup"><span data-stu-id="9d71d-163">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="9d71d-164">L’utilisation du stockage de table Windows Azure pour stocker les informations utilisateur et de groupe est semblable à l’utilisation d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="9d71d-164">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="9d71d-165">L’exemple suivant montre une entité de table qui stocke le nom d’utilisateur et le nom de groupe.</span><span class="sxs-lookup"><span data-stu-id="9d71d-165">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<span data-ttu-id="9d71d-166">Dans le concentrateur, vous récupérez les groupes qui sont affectés lorsque l’utilisateur se connecte.</span><span class="sxs-lookup"><span data-stu-id="9d71d-166">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="9d71d-167">Vérification de l’appartenance au groupe lors de la reconnexion</span><span class="sxs-lookup"><span data-stu-id="9d71d-167">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="9d71d-168">Par défaut, SignalR nouveau attribue automatiquement un utilisateur aux groupes appropriés lors de la reconnexion à partir d’une interruption temporaire, tels que lorsqu’une connexion est supprimée ou rétablie avant l’expiration de la connexion. Informations sur les groupes de l’utilisateur sont passées dans un jeton lors de la reconnexion, et ce jeton est vérifié sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="9d71d-168">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="9d71d-169">Pour plus d’informations sur le processus de vérification pour la réintégration des utilisateurs à des groupes, consultez [réintégration des groupes lors de la reconnexion](../security/introduction-to-security.md#rejoingroup).</span><span class="sxs-lookup"><span data-stu-id="9d71d-169">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](../security/introduction-to-security.md#rejoingroup).</span></span>

<span data-ttu-id="9d71d-170">En général, vous devez utiliser le comportement par défaut de la réintégration des groupes sur se reconnecter automatiquement.</span><span class="sxs-lookup"><span data-stu-id="9d71d-170">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="9d71d-171">SignalR groupes ne sont pas conçus comme mécanisme de sécurité pour restreindre l’accès aux données sensibles.</span><span class="sxs-lookup"><span data-stu-id="9d71d-171">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="9d71d-172">Toutefois, si votre application doit vérifier l’appartenance d’un utilisateur lors de la reconnexion, vous pouvez remplacer le comportement par défaut.</span><span class="sxs-lookup"><span data-stu-id="9d71d-172">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="9d71d-173">Modifier le comportement par défaut, une charge peut ajouter à votre base de données car l’appartenance au groupe de l’utilisateur doit être récupérée pour chaque reconnexion plutôt que simplement lorsque l’utilisateur se connecte.</span><span class="sxs-lookup"><span data-stu-id="9d71d-173">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="9d71d-174">Si vous devez vérifier l’appartenance au groupe sur se reconnecter, créez un nouveau module de pipeline de concentrateur qui retourne une liste de groupes qui sont affectés, comme illustré ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="9d71d-174">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<span data-ttu-id="9d71d-175">Ajoutez ensuite ce module dans le pipeline de concentrateur, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="9d71d-175">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
