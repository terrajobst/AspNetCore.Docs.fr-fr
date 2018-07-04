---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Mappage des utilisateurs SignalR aux connexions | Microsoft Docs
author: tfitzmac
description: Cette rubrique montre comment conserver des informations sur les utilisateurs et de leurs connexions. Patrick Fletcher a permis à écrire cette rubrique. Versions des logiciels utilisées dans cette rubrique...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/30/2014
ms.topic: article
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: bd7c0cd9a645ab5b65c5c1446b51ea1646e43799
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391530"
---
<a name="mapping-signalr-users-to-connections"></a><span data-ttu-id="d90fd-105">Mappage des utilisateurs SignalR aux connexions</span><span class="sxs-lookup"><span data-stu-id="d90fd-105">Mapping SignalR Users to Connections</span></span>
====================
<span data-ttu-id="d90fd-106">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d90fd-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d90fd-107">Cette rubrique montre comment conserver des informations sur les utilisateurs et de leurs connexions.</span><span class="sxs-lookup"><span data-stu-id="d90fd-107">This topic shows how to retain information about users and their connections.</span></span>
> 
> <span data-ttu-id="d90fd-108">Patrick Fletcher a permis à écrire cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="d90fd-108">Patrick Fletcher helped write this topic.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="d90fd-109">Versions des logiciels utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="d90fd-109">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="d90fd-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d90fd-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="d90fd-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="d90fd-111">.NET 4.5</span></span>
> - <span data-ttu-id="d90fd-112">SignalR version 2</span><span class="sxs-lookup"><span data-stu-id="d90fd-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="d90fd-113">Versions précédentes de cette rubrique</span><span class="sxs-lookup"><span data-stu-id="d90fd-113">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="d90fd-114">Pour plus d’informations sur les versions antérieures de SignalR, consultez [les Versions antérieures de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="d90fd-114">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="d90fd-115">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="d90fd-115">Questions and comments</span></span>
> 
> <span data-ttu-id="d90fd-116">Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="d90fd-116">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="d90fd-117">Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="d90fd-117">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="introduction"></a><span data-ttu-id="d90fd-118">Introduction</span><span class="sxs-lookup"><span data-stu-id="d90fd-118">Introduction</span></span>

<span data-ttu-id="d90fd-119">Chaque client qui se connecte à un concentrateur transmet un id de connexion unique. Vous pouvez récupérer cette valeur dans le `Context.ConnectionId` propriété de contexte du concentrateur.</span><span class="sxs-lookup"><span data-stu-id="d90fd-119">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="d90fd-120">Si votre application a besoin mapper un utilisateur à l’id de connexion et de conserver ce mappage, vous pouvez utiliser une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="d90fd-120">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- [<span data-ttu-id="d90fd-121">Le fournisseur d’ID utilisateur (SignalR 2)</span><span class="sxs-lookup"><span data-stu-id="d90fd-121">The User ID Provider (SignalR 2)</span></span>](#IUserIdProvider)
- <span data-ttu-id="d90fd-122">[Stockage in-memory](#inmemory), par exemple un dictionnaire</span><span class="sxs-lookup"><span data-stu-id="d90fd-122">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="d90fd-123">Groupe de SignalR pour chaque utilisateur</span><span class="sxs-lookup"><span data-stu-id="d90fd-123">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="d90fd-124">[Stockage permanent, externe](#database), comme une table de base de données ou d’un stockage table Azure</span><span class="sxs-lookup"><span data-stu-id="d90fd-124">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="d90fd-125">Chacune de ces implémentations est indiqué dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="d90fd-125">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="d90fd-126">Vous utilisez le `OnConnected`, `OnDisconnected`, et `OnReconnected` méthodes de la `Hub` classe pour effectuer le suivi de l’état de connexion utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d90fd-126">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="d90fd-127">La meilleure approche pour votre application dépend de :</span><span class="sxs-lookup"><span data-stu-id="d90fd-127">The best approach for your application depends on:</span></span>

- <span data-ttu-id="d90fd-128">Le nombre de serveurs web hébergeant votre application.</span><span class="sxs-lookup"><span data-stu-id="d90fd-128">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="d90fd-129">S’il faut obtenir la liste des utilisateurs actuellement connectés.</span><span class="sxs-lookup"><span data-stu-id="d90fd-129">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="d90fd-130">S’il faut conserver les informations de groupe et utilisateur lorsque l’application ou le serveur redémarre.</span><span class="sxs-lookup"><span data-stu-id="d90fd-130">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="d90fd-131">Indique si la latence de l’appel d’un serveur externe est un problème.</span><span class="sxs-lookup"><span data-stu-id="d90fd-131">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="d90fd-132">Le tableau suivant présente l’approche qui fonctionne pour ces considérations.</span><span class="sxs-lookup"><span data-stu-id="d90fd-132">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="d90fd-133">Plusieurs serveurs</span><span class="sxs-lookup"><span data-stu-id="d90fd-133">More than one server</span></span> | <span data-ttu-id="d90fd-134">Obtenir la liste des utilisateurs actuellement connectés</span><span class="sxs-lookup"><span data-stu-id="d90fd-134">Get list of currently connected users</span></span> | <span data-ttu-id="d90fd-135">Conserver les informations après le redémarrage</span><span class="sxs-lookup"><span data-stu-id="d90fd-135">Persist information after restarts</span></span> | <span data-ttu-id="d90fd-136">Performances optimales</span><span class="sxs-lookup"><span data-stu-id="d90fd-136">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="d90fd-137">Fournisseur de nom d’utilisateur</span><span class="sxs-lookup"><span data-stu-id="d90fd-137">UserID Provider</span></span> | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="d90fd-138">En mémoire</span><span class="sxs-lookup"><span data-stu-id="d90fd-138">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="d90fd-139">Groupes d’utilisateur unique</span><span class="sxs-lookup"><span data-stu-id="d90fd-139">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| <span data-ttu-id="d90fd-140">Permanente, externe</span><span class="sxs-lookup"><span data-stu-id="d90fd-140">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a><span data-ttu-id="d90fd-141">Fournisseur de IUserID</span><span class="sxs-lookup"><span data-stu-id="d90fd-141">IUserID provider</span></span>

<span data-ttu-id="d90fd-142">Cette fonctionnalité permet aux utilisateurs de spécifier quel est le nom d’utilisateur basé sur un IRequest via une nouvelle interface IUserIdProvider.</span><span class="sxs-lookup"><span data-stu-id="d90fd-142">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider.</span></span>

<span data-ttu-id="d90fd-143">**Le IUserIdProvider**</span><span class="sxs-lookup"><span data-stu-id="d90fd-143">**The IUserIdProvider**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="d90fd-144">Par défaut, il y aura une implémentation qui utilise l’utilisateur `IPrincipal.Identity.Name` comme nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d90fd-144">By default, there will be an implementation that uses the user's `IPrincipal.Identity.Name` as the user name.</span></span> <span data-ttu-id="d90fd-145">Pour changer cela, enregistrer votre implémentation de `IUserIdProvider` avec l’hôte global au démarrage de votre application :</span><span class="sxs-lookup"><span data-stu-id="d90fd-145">To change this, register your implementation of `IUserIdProvider` with the global host when your application starts:</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<span data-ttu-id="d90fd-146">À partir d’au sein d’un concentrateur, vous serez en mesure d’envoyer des messages à ces utilisateurs via l’API suivante :</span><span class="sxs-lookup"><span data-stu-id="d90fd-146">From within a hub, you'll be able to send messages to these users via the following API:</span></span>

<span data-ttu-id="d90fd-147">**Envoi d’un message à un utilisateur spécifique**</span><span class="sxs-lookup"><span data-stu-id="d90fd-147">**Sending a message to a specific user**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="d90fd-148">Stockage in-memory</span><span class="sxs-lookup"><span data-stu-id="d90fd-148">In-memory storage</span></span>

<span data-ttu-id="d90fd-149">Les exemples suivants montrent comment conserver la connexion et les informations utilisateur dans un dictionnaire qui est stocké en mémoire.</span><span class="sxs-lookup"><span data-stu-id="d90fd-149">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="d90fd-150">Le dictionnaire utilise un `HashSet` pour stocker l’id de connexion. À tout moment un utilisateur peut avoir plusieurs connexions à l’application de SignalR.</span><span class="sxs-lookup"><span data-stu-id="d90fd-150">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="d90fd-151">Par exemple, un utilisateur qui est connecté via plusieurs appareils ou de plusieurs onglets de navigateur aurait plus d’un id de connexion.</span><span class="sxs-lookup"><span data-stu-id="d90fd-151">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="d90fd-152">Si l’application s’arrête, toutes les informations sont perdues, mais il sera nouveau rempli quand les utilisateurs rétablissement leurs connexions.</span><span class="sxs-lookup"><span data-stu-id="d90fd-152">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="d90fd-153">Stockage en mémoire ne fonctionne pas si votre environnement comprend plusieurs serveurs web parce que chaque serveur aurait une collection distincte de connexions.</span><span class="sxs-lookup"><span data-stu-id="d90fd-153">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="d90fd-154">Le premier exemple montre une classe qui gère le mappage des utilisateurs pour les connexions.</span><span class="sxs-lookup"><span data-stu-id="d90fd-154">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="d90fd-155">La clé pour le HashSet sera le nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d90fd-155">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="d90fd-156">L’exemple suivant montre comment utiliser la classe de mappage de connexion d’un concentrateur.</span><span class="sxs-lookup"><span data-stu-id="d90fd-156">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="d90fd-157">L’instance de la classe est stockée dans un nom de variable `_connections`.</span><span class="sxs-lookup"><span data-stu-id="d90fd-157">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="d90fd-158">Groupes d’utilisateur unique</span><span class="sxs-lookup"><span data-stu-id="d90fd-158">Single-user groups</span></span>

<span data-ttu-id="d90fd-159">Vous pouvez créer un groupe pour chaque utilisateur et ensuite envoyer un message à ce groupe lorsque vous souhaitez atteindre seul cet utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d90fd-159">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="d90fd-160">Le nom de chaque groupe est le nom de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d90fd-160">The name of each group is the name of the user.</span></span> <span data-ttu-id="d90fd-161">Si un utilisateur a plusieurs connexions, chaque id de connexion est ajouté au groupe de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d90fd-161">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="d90fd-162">Vous devez supprimer pas manuellement l’utilisateur à partir du groupe lorsque l’utilisateur se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="d90fd-162">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="d90fd-163">Cette action est effectuée automatiquement par l’infrastructure SignalR.</span><span class="sxs-lookup"><span data-stu-id="d90fd-163">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="d90fd-164">L’exemple suivant montre comment implémenter des groupes d’utilisateur unique.</span><span class="sxs-lookup"><span data-stu-id="d90fd-164">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="d90fd-165">Stockage permanent, externe</span><span class="sxs-lookup"><span data-stu-id="d90fd-165">Permanent, external storage</span></span>

<span data-ttu-id="d90fd-166">Cette rubrique montre comment utiliser une base de données ou le stockage table Azure pour stocker les informations de connexion.</span><span class="sxs-lookup"><span data-stu-id="d90fd-166">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="d90fd-167">Cette approche fonctionne lorsque vous avez plusieurs serveurs web, car chaque serveur web peut interagir avec le même référentiel de données.</span><span class="sxs-lookup"><span data-stu-id="d90fd-167">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="d90fd-168">Si vos serveurs web arrêter le travail ou le redémarrage de l’application, le `OnDisconnected` méthode n’est pas appelée.</span><span class="sxs-lookup"><span data-stu-id="d90fd-168">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="d90fd-169">Par conséquent, il est possible que votre référentiel de données auront des enregistrements pour les identifiants de connexion qui ne sont plus valides.</span><span class="sxs-lookup"><span data-stu-id="d90fd-169">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="d90fd-170">Pour nettoyer ces enregistrements orphelins, vous pouvez souhaiter invalider toute connexion qui a été créée en dehors d’un laps de temps qui s’applique à votre application.</span><span class="sxs-lookup"><span data-stu-id="d90fd-170">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="d90fd-171">Les exemples dans cette section incluent une valeur pour le suivi lorsque la connexion a été créée, mais ne s’affichent pas comment nettoyer les anciens enregistrements, car vous pouvez souhaiter le faire en tant que processus en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="d90fd-171">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="d90fd-172">Base de données</span><span class="sxs-lookup"><span data-stu-id="d90fd-172">Database</span></span>

<span data-ttu-id="d90fd-173">Les exemples suivants montrent comment conserver la connexion et les informations utilisateur dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="d90fd-173">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="d90fd-174">Vous pouvez utiliser toute technologie d’accès aux données ; Toutefois, l’exemple ci-dessous montre comment définir des modèles à l’aide d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d90fd-174">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="d90fd-175">Ces modèles d’entité correspondent aux champs et aux tables de base de données.</span><span class="sxs-lookup"><span data-stu-id="d90fd-175">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="d90fd-176">Votre structure de données peut varier considérablement selon les besoins de votre application.</span><span class="sxs-lookup"><span data-stu-id="d90fd-176">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="d90fd-177">Le premier exemple montre comment définir une entité d’utilisateur qui peut être associée à de nombreuses entités de connexion.</span><span class="sxs-lookup"><span data-stu-id="d90fd-177">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

<span data-ttu-id="d90fd-178">Ensuite, à partir du hub, vous pouvez suivre l’état de chaque connexion avec le code indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="d90fd-178">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a><span data-ttu-id="d90fd-179">Stockage table Azure</span><span class="sxs-lookup"><span data-stu-id="d90fd-179">Azure table storage</span></span>

<span data-ttu-id="d90fd-180">L’exemple de stockage table Azure suivant est similaire à l’exemple de base de données.</span><span class="sxs-lookup"><span data-stu-id="d90fd-180">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="d90fd-181">Il n’inclut pas toutes les informations que vous devrez bien démarrer avec le Service de stockage de Table Azure.</span><span class="sxs-lookup"><span data-stu-id="d90fd-181">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="d90fd-182">Pour plus d’informations, consultez [comment utiliser le stockage de Table à partir de .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="d90fd-182">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="d90fd-183">L’exemple suivant montre une entité de table pour stocker les informations de connexion.</span><span class="sxs-lookup"><span data-stu-id="d90fd-183">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="d90fd-184">Il partitionne les données par nom d’utilisateur et identifie chaque entité par l’id de connexion pour un utilisateur peut avoir plusieurs connexions à tout moment.</span><span class="sxs-lookup"><span data-stu-id="d90fd-184">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

<span data-ttu-id="d90fd-185">Dans le hub, vous suivre l’état de chaque connexion d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d90fd-185">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
