---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: Mappage des utilisateurs de SignalR pour les connexions SignalR 1.x | Documents Microsoft
author: pfletcher
description: Cette rubrique montre comment conserver les informations sur les utilisateurs et leurs connexions.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 896bf4142ce090e39ed5697ff053cd56728318ed
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a><span data-ttu-id="c2ee0-103">Mappage des utilisateurs de SignalR pour les connexions SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="c2ee0-103">Mapping SignalR Users to Connections in SignalR 1.x</span></span>
====================
<span data-ttu-id="c2ee0-104">par [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c2ee0-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="c2ee0-105">Cette rubrique montre comment conserver les informations sur les utilisateurs et leurs connexions.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-105">This topic shows how to retain information about users and their connections.</span></span>


## <a name="introduction"></a><span data-ttu-id="c2ee0-106">Introduction</span><span class="sxs-lookup"><span data-stu-id="c2ee0-106">Introduction</span></span>

<span data-ttu-id="c2ee0-107">Chaque client qui se connecte à un concentrateur passe un id de connexion unique. Vous pouvez récupérer cette valeur dans le `Context.ConnectionId` propriété du contexte du concentrateur.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-107">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="c2ee0-108">Si votre application doit associer un utilisateur à l’id de connexion et de conserver ce mappage, vous pouvez utiliser une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="c2ee0-108">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- <span data-ttu-id="c2ee0-109">[Stockage en mémoire](#inmemory), par exemple un dictionnaire</span><span class="sxs-lookup"><span data-stu-id="c2ee0-109">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="c2ee0-110">Groupe SignalR pour chaque utilisateur</span><span class="sxs-lookup"><span data-stu-id="c2ee0-110">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="c2ee0-111">[Stockage permanent, externe](#database), tel qu’une table de base de données ou le stockage de table Windows Azure</span><span class="sxs-lookup"><span data-stu-id="c2ee0-111">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="c2ee0-112">Chacune de ces implémentations est indiqué dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-112">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="c2ee0-113">Vous utilisez la `OnConnected`, `OnDisconnected`, et `OnReconnected` méthodes de la `Hub` classe pour effectuer le suivi de l’état de connexion utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-113">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="c2ee0-114">La meilleure approche pour votre application dépend de :</span><span class="sxs-lookup"><span data-stu-id="c2ee0-114">The best approach for your application depends on:</span></span>

- <span data-ttu-id="c2ee0-115">Le nombre de serveurs web qui héberge votre application.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-115">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="c2ee0-116">Si vous devez obtenir la liste des utilisateurs actuellement connectés.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-116">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="c2ee0-117">S’il faut conserver les informations de groupe et utilisateur lorsque l’application ou le serveur redémarre.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-117">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="c2ee0-118">Indique si la latence de l’appel à un serveur externe est un problème.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-118">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="c2ee0-119">Le tableau suivant indique quelle approche fonctionne pour ces considérations.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-119">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="c2ee0-120">Plusieurs serveurs</span><span class="sxs-lookup"><span data-stu-id="c2ee0-120">More than one server</span></span> | <span data-ttu-id="c2ee0-121">Obtenir la liste des utilisateurs actuellement connectés.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-121">Get list of currently connected users</span></span> | <span data-ttu-id="c2ee0-122">Conserver les informations d’après le redémarrage</span><span class="sxs-lookup"><span data-stu-id="c2ee0-122">Persist information after restarts</span></span> | <span data-ttu-id="c2ee0-123">Performances optimales</span><span class="sxs-lookup"><span data-stu-id="c2ee0-123">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="c2ee0-124">En mémoire</span><span class="sxs-lookup"><span data-stu-id="c2ee0-124">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="c2ee0-125">Groupes d’utilisateur unique</span><span class="sxs-lookup"><span data-stu-id="c2ee0-125">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="c2ee0-126">Permanentes, externe</span><span class="sxs-lookup"><span data-stu-id="c2ee0-126">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="c2ee0-127">Stockage en mémoire</span><span class="sxs-lookup"><span data-stu-id="c2ee0-127">In-memory storage</span></span>

<span data-ttu-id="c2ee0-128">Les exemples suivants montrent comment conserver les informations de connexion et d’utilisateur dans un dictionnaire qui est stocké en mémoire.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-128">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="c2ee0-129">Le dictionnaire utilise un `HashSet` pour stocker l’id de connexion. À tout moment, un utilisateur peut avoir plusieurs connexions à l’application de SignalR.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-129">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="c2ee0-130">Par exemple, un utilisateur qui est connecté via plusieurs appareils ou plusieurs onglets de navigateur aurait plus d’un id de connexion.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-130">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="c2ee0-131">Si l’application s’arrête, toutes les informations sont perdues, mais il est nouveau rempli que les utilisateurs de nouveau établissent les connexions.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-131">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="c2ee0-132">Stockage en mémoire ne fonctionne pas si votre environnement inclut plusieurs serveurs web, parce que chaque serveur n’aurait une collection distincte de connexions.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-132">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="c2ee0-133">Le premier exemple montre une classe qui gère le mappage des utilisateurs pour les connexions.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-133">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="c2ee0-134">La clé pour le HashSet sera le nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-134">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="c2ee0-135">L’exemple suivant montre comment utiliser la classe de mappage de connexion d’un concentrateur.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-135">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="c2ee0-136">L’instance de la classe est stockée dans un nom de variable `_connections`.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-136">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="c2ee0-137">Groupes d’utilisateur unique</span><span class="sxs-lookup"><span data-stu-id="c2ee0-137">Single-user groups</span></span>

<span data-ttu-id="c2ee0-138">Vous pouvez créer un groupe pour chaque utilisateur et puis envoyez un message à ce groupe lorsque vous souhaitez atteindre seul cet utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-138">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="c2ee0-139">Le nom de chaque groupe est le nom de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-139">The name of each group is the name of the user.</span></span> <span data-ttu-id="c2ee0-140">Si un utilisateur a plusieurs connexions, chaque id de connexion est ajouté au groupe de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-140">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="c2ee0-141">Vous devez supprimer pas manuellement l’utilisateur à partir du groupe lorsque l’utilisateur se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-141">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="c2ee0-142">Cette action est effectuée automatiquement par l’infrastructure SignalR.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-142">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="c2ee0-143">L’exemple suivant montre comment implémenter des groupes d’utilisateur unique.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-143">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="c2ee0-144">Stockage permanent, externe</span><span class="sxs-lookup"><span data-stu-id="c2ee0-144">Permanent, external storage</span></span>

<span data-ttu-id="c2ee0-145">Cette rubrique montre comment utiliser une base de données ou le stockage de table Windows Azure pour stocker les informations de connexion.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-145">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="c2ee0-146">Cette approche fonctionne lorsque vous avez plusieurs serveurs web, car chaque serveur web peut interagir avec le même référentiel de données.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-146">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="c2ee0-147">Si vos serveurs web arrêter le travail ou le redémarrage de l’application, le `OnDisconnected` méthode n’est pas appelée.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-147">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="c2ee0-148">Par conséquent, il est possible que votre référentiel de données aura des enregistrements pour les ID de connexion qui ne sont plus valides.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-148">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="c2ee0-149">Pour nettoyer ces enregistrements orphelins, vous pouvez souhaiter annuler toute connexion qui a été créée en dehors d’un laps de temps qui s’applique à votre application.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-149">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="c2ee0-150">Les exemples de cette section incluent une valeur pour le suivi des modifications lorsque la connexion a été créée, mais n’affichent pas comment nettoyer les anciens enregistrements, car vous souhaiterez faire en tant que processus en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-150">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="c2ee0-151">Base de données</span><span class="sxs-lookup"><span data-stu-id="c2ee0-151">Database</span></span>

<span data-ttu-id="c2ee0-152">Les exemples suivants montrent comment conserver les informations de connexion et d’utilisateur dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-152">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="c2ee0-153">Vous pouvez utiliser n’importe quel technologie d’accès aux données ; Toutefois, l’exemple ci-dessous montre comment définir des modèles à l’aide d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="c2ee0-154">Ces modèles d’entité correspondent aux champs et des tables de base de données.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="c2ee0-155">Votre structure de données peut varier considérablement selon les besoins de votre application.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-155">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="c2ee0-156">Le premier exemple montre comment définir une entité d’utilisateur qui peut être associée à de nombreuses entités de connexion.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-156">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="c2ee0-157">Puis, dans le concentrateur, vous pouvez suivre l’état de chaque connexion avec le code ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-157">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a><span data-ttu-id="c2ee0-158">Stockage de table Windows Azure</span><span class="sxs-lookup"><span data-stu-id="c2ee0-158">Azure table storage</span></span>

<span data-ttu-id="c2ee0-159">L’exemple de stockage de table Azure suivant est semblable à l’exemple de base de données.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-159">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="c2ee0-160">Il n’inclut pas toutes les informations que vous devrez commencer avec le Service de stockage de Table Azure.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-160">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="c2ee0-161">Pour plus d’informations, consultez [comment utiliser le stockage de Table à partir de .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="c2ee0-161">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="c2ee0-162">L’exemple suivant montre une entité de table pour stocker les informations de connexion.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-162">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="c2ee0-163">Il partitionne les données par nom d’utilisateur et identifie chaque entité par l’id de connexion pour un utilisateur peut avoir plusieurs connexions à tout moment.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-163">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<span data-ttu-id="c2ee0-164">Dans le concentrateur, vous suivre l’état de chaque connexion d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c2ee0-164">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
