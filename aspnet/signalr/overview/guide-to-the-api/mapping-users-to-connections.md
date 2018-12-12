---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Mappage des utilisateurs SignalR aux connexions | Microsoft Docs
author: Rick-Anderson
description: Cette rubrique montre comment conserver des informations sur les utilisateurs et de leurs connexions. Patrick Fletcher a permis à écrire cette rubrique. Versions des logiciels utilisées dans cette rubrique...
ms.author: riande
ms.date: 12/30/2014
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: f24d3a4828bc0863f8213e53f054aa7914edffad
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287760"
---
<a name="mapping-signalr-users-to-connections"></a>Mappage des utilisateurs SignalR aux connexions
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Cette rubrique montre comment conserver des informations sur les utilisateurs et de leurs connexions.
>
> Patrick Fletcher a permis à écrire cette rubrique.
>
> ## <a name="software-versions-used-in-this-topic"></a>Versions des logiciels utilisées dans cette rubrique
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR version 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versions précédentes de cette rubrique
>
> Pour plus d’informations sur les versions antérieures de SignalR, consultez [les Versions antérieures de SignalR](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Questions et commentaires
>
> Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer dans les commentaires en bas de la page. Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).

## <a name="introduction"></a>Introduction

Chaque client qui se connecte à un concentrateur transmet un id de connexion unique. Vous pouvez récupérer cette valeur dans le `Context.ConnectionId` propriété de contexte du concentrateur. Si votre application a besoin mapper un utilisateur à l’id de connexion et de conserver ce mappage, vous pouvez utiliser une des opérations suivantes :

- [Le fournisseur d’ID utilisateur (SignalR 2)](#IUserIdProvider)
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
| Fournisseur de nom d’utilisateur | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| En mémoire |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| Groupes d’utilisateur unique | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| Permanente, externe | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a>Fournisseur de IUserID

Cette fonctionnalité permet aux utilisateurs de spécifier quel est le nom d’utilisateur basé sur un IRequest via une nouvelle interface IUserIdProvider.

**Le IUserIdProvider**

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

Par défaut, il y aura une implémentation qui utilise l’utilisateur `IPrincipal.Identity.Name` comme nom d’utilisateur. Pour changer cela, enregistrer votre implémentation de `IUserIdProvider` avec l’hôte global au démarrage de votre application :

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

À partir d’au sein d’un concentrateur, vous serez en mesure d’envoyer des messages à ces utilisateurs via l’API suivante :

**Envoi d’un message à un utilisateur spécifique**

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Stockage in-memory

Les exemples suivants montrent comment conserver la connexion et les informations utilisateur dans un dictionnaire qui est stocké en mémoire. Le dictionnaire utilise un `HashSet` pour stocker l’id de connexion. À tout moment un utilisateur peut avoir plusieurs connexions à l’application de SignalR. Par exemple, un utilisateur qui est connecté via plusieurs appareils ou de plusieurs onglets de navigateur aurait plus d’un id de connexion.

Si l’application s’arrête, toutes les informations sont perdues, mais il sera nouveau rempli quand les utilisateurs rétablissement leurs connexions. Stockage en mémoire ne fonctionne pas si votre environnement comprend plusieurs serveurs web parce que chaque serveur aurait une collection distincte de connexions.

Le premier exemple montre une classe qui gère le mappage des utilisateurs pour les connexions. La clé pour le HashSet sera le nom d’utilisateur.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

L’exemple suivant montre comment utiliser la classe de mappage de connexion d’un concentrateur. L’instance de la classe est stockée dans un nom de variable `_connections`.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Groupes d’utilisateur unique

Vous pouvez créer un groupe pour chaque utilisateur et ensuite envoyer un message à ce groupe lorsque vous souhaitez atteindre seul cet utilisateur. Le nom de chaque groupe est le nom de l’utilisateur. Si un utilisateur a plusieurs connexions, chaque id de connexion est ajouté au groupe de l’utilisateur.

Vous devez supprimer pas manuellement l’utilisateur à partir du groupe lorsque l’utilisateur se déconnecte. Cette action est effectuée automatiquement par l’infrastructure SignalR.

L’exemple suivant montre comment implémenter des groupes d’utilisateur unique.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Stockage permanent, externe

Cette rubrique montre comment utiliser une base de données ou le stockage table Azure pour stocker les informations de connexion. Cette approche fonctionne lorsque vous avez plusieurs serveurs web, car chaque serveur web peut interagir avec le même référentiel de données. Si vos serveurs web arrêter le travail ou le redémarrage de l’application, le `OnDisconnected` méthode n’est pas appelée. Par conséquent, il est possible que votre référentiel de données auront des enregistrements pour les identifiants de connexion qui ne sont plus valides. Pour nettoyer ces enregistrements orphelins, vous pouvez souhaiter invalider toute connexion qui a été créée en dehors d’un laps de temps qui s’applique à votre application. Les exemples dans cette section incluent une valeur pour le suivi lorsque la connexion a été créée, mais ne s’affichent pas comment nettoyer les anciens enregistrements, car vous pouvez souhaiter le faire en tant que processus en arrière-plan.

### <a name="database"></a>Base de données

Les exemples suivants montrent comment conserver la connexion et les informations utilisateur dans une base de données. Vous pouvez utiliser toute technologie d’accès aux données ; Toutefois, l’exemple ci-dessous montre comment définir des modèles à l’aide d’Entity Framework. Ces modèles d’entité correspondent aux champs et aux tables de base de données. Votre structure de données peut varier considérablement selon les besoins de votre application.

Le premier exemple montre comment définir une entité d’utilisateur qui peut être associée à de nombreuses entités de connexion.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

Ensuite, à partir du hub, vous pouvez suivre l’état de chaque connexion avec le code indiqué ci-dessous.

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a>Stockage table Azure

L’exemple de stockage table Azure suivant est similaire à l’exemple de base de données. Il n’inclut pas toutes les informations que vous devrez bien démarrer avec le Service de stockage de Table Azure. Pour plus d’informations, consultez [comment utiliser le stockage de Table à partir de .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

L’exemple suivant montre une entité de table pour stocker les informations de connexion. Il partitionne les données par nom d’utilisateur et identifie chaque entité par l’id de connexion pour un utilisateur peut avoir plusieurs connexions à tout moment.

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

Dans le hub, vous suivre l’état de chaque connexion d’utilisateur.

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
