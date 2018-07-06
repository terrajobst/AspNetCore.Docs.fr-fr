---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: Mappage des utilisateurs SignalR aux connexions dans SignalR 1.x | Microsoft Docs
author: pfletcher
description: Cette rubrique montre comment conserver des informations sur les utilisateurs et de leurs connexions.
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 02ee9468ae4198af47226cdd5c22243f16e20da4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818111"
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a>Mappage des utilisateurs SignalR aux connexions dans SignalR 1.x
====================
par [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> Cette rubrique montre comment conserver des informations sur les utilisateurs et de leurs connexions.


## <a name="introduction"></a>Introduction

Chaque client qui se connecte à un concentrateur transmet un id de connexion unique. Vous pouvez récupérer cette valeur dans le `Context.ConnectionId` propriété de contexte du concentrateur. Si votre application a besoin mapper un utilisateur à l’id de connexion et de conserver ce mappage, vous pouvez utiliser une des opérations suivantes :

- [Stockage in-memory](#inmemory), par exemple un dictionnaire
- [Groupe de SignalR pour chaque utilisateur](#groups)
- [Stockage permanent, externe](#database), comme une table de base de données ou d’un stockage table Azure

Chacune de ces implémentations est indiqué dans cette rubrique. Vous utilisez le `OnConnected`, `OnDisconnected`, et `OnReconnected` méthodes de la `Hub` classe pour effectuer le suivi de l’état de connexion utilisateur.

La meilleure approche pour votre application dépend de :

- Le nombre de serveurs web hébergeant votre application.
- S’il faut obtenir la liste des utilisateurs actuellement connectés.
- S’il faut conserver les informations de groupe et utilisateur lorsque l’application ou le serveur redémarre.
- Indique si la latence de l’appel d’un serveur externe est un problème.

Le tableau suivant présente l’approche qui fonctionne pour ces considérations.

|  | Plusieurs serveurs | Obtenir la liste des utilisateurs actuellement connectés | Conserver les informations après le redémarrage | Performances optimales |
| --- | --- | --- | --- | --- |
| En mémoire |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| Groupes d’utilisateur unique | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| Permanente, externe | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Stockage in-memory

Les exemples suivants montrent comment conserver la connexion et les informations utilisateur dans un dictionnaire qui est stocké en mémoire. Le dictionnaire utilise un `HashSet` pour stocker l’id de connexion. À tout moment un utilisateur peut avoir plusieurs connexions à l’application de SignalR. Par exemple, un utilisateur qui est connecté via plusieurs appareils ou de plusieurs onglets de navigateur aurait plus d’un id de connexion.

Si l’application s’arrête, toutes les informations sont perdues, mais il sera nouveau rempli quand les utilisateurs rétablissement leurs connexions. Stockage en mémoire ne fonctionne pas si votre environnement comprend plusieurs serveurs web parce que chaque serveur aurait une collection distincte de connexions.

Le premier exemple montre une classe qui gère le mappage des utilisateurs pour les connexions. La clé pour le HashSet sera le nom d’utilisateur.

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

L’exemple suivant montre comment utiliser la classe de mappage de connexion d’un concentrateur. L’instance de la classe est stockée dans un nom de variable `_connections`.

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Groupes d’utilisateur unique

Vous pouvez créer un groupe pour chaque utilisateur et ensuite envoyer un message à ce groupe lorsque vous souhaitez atteindre seul cet utilisateur. Le nom de chaque groupe est le nom de l’utilisateur. Si un utilisateur a plusieurs connexions, chaque id de connexion est ajouté au groupe de l’utilisateur.

Vous devez supprimer pas manuellement l’utilisateur à partir du groupe lorsque l’utilisateur se déconnecte. Cette action est effectuée automatiquement par l’infrastructure SignalR.

L’exemple suivant montre comment implémenter des groupes d’utilisateur unique.

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Stockage permanent, externe

Cette rubrique montre comment utiliser une base de données ou le stockage table Azure pour stocker les informations de connexion. Cette approche fonctionne lorsque vous avez plusieurs serveurs web, car chaque serveur web peut interagir avec le même référentiel de données. Si vos serveurs web arrêter le travail ou le redémarrage de l’application, le `OnDisconnected` méthode n’est pas appelée. Par conséquent, il est possible que votre référentiel de données auront des enregistrements pour les identifiants de connexion qui ne sont plus valides. Pour nettoyer ces enregistrements orphelins, vous pouvez souhaiter invalider toute connexion qui a été créée en dehors d’un laps de temps qui s’applique à votre application. Les exemples dans cette section incluent une valeur pour le suivi lorsque la connexion a été créée, mais ne s’affichent pas comment nettoyer les anciens enregistrements, car vous pouvez souhaiter le faire en tant que processus en arrière-plan.

### <a name="database"></a>Base de données

Les exemples suivants montrent comment conserver la connexion et les informations utilisateur dans une base de données. Vous pouvez utiliser toute technologie d’accès aux données ; Toutefois, l’exemple ci-dessous montre comment définir des modèles à l’aide d’Entity Framework. Ces modèles d’entité correspondent aux champs et aux tables de base de données. Votre structure de données peut varier considérablement selon les besoins de votre application.

Le premier exemple montre comment définir une entité d’utilisateur qui peut être associée à de nombreuses entités de connexion.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

Ensuite, à partir du hub, vous pouvez suivre l’état de chaque connexion avec le code indiqué ci-dessous.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a>Stockage table Azure

L’exemple de stockage table Azure suivant est similaire à l’exemple de base de données. Il n’inclut pas toutes les informations que vous devrez bien démarrer avec le Service de stockage de Table Azure. Pour plus d’informations, consultez [comment utiliser le stockage de Table à partir de .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

L’exemple suivant montre une entité de table pour stocker les informations de connexion. Il partitionne les données par nom d’utilisateur et identifie chaque entité par l’id de connexion pour un utilisateur peut avoir plusieurs connexions à tout moment.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

Dans le hub, vous suivre l’état de chaque connexion d’utilisateur.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
