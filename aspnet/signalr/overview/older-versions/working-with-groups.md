---
uid: signalr/overview/older-versions/working-with-groups
title: Utilisation de groupes dans SignalR 1.x | Microsoft Docs
author: pfletcher
description: Cette rubrique décrit comment conserver les informations d’appartenance de groupe avec l’API de concentrateur.
ms.author: riande
ms.date: 10/21/2013
ms.assetid: 22929efd-68c9-4609-b76d-f8ba42fda01e
msc.legacyurl: /signalr/overview/older-versions/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 5096e7a5eb8704762a21eeb88a128d92e75ede60
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287486"
---
<a name="working-with-groups-in-signalr-1x"></a>Utilisation de groupes dans SignalR 1.x
====================
par [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Cette rubrique décrit comment ajouter des utilisateurs aux groupes et de conserver les informations d’appartenance au groupe.


## <a name="overview"></a>Vue d'ensemble

Groupes dans SignalR fournissent une méthode pour diffuser des messages à des sous-ensembles spécifiés de clients connectés. Un groupe peut avoir n’importe quel nombre de clients, et un client peut être un membre de n’importe quel nombre de groupes. Vous n’êtes pas obligé de créer explicitement des groupes. En effet, un groupe est automatiquement créé la première fois que vous spécifiez son nom dans un appel à Groups.Add, et il est supprimé lorsque vous supprimez la dernière connexion de l’appartenance à elle. Pour une introduction à l’aide de groupes, consultez [comment gérer l’appartenance au groupe à partir de la classe de concentrateur](index.md) dans l’API Hubs - Guide du serveur.

Il n’existe aucune API pour obtenir une liste d’appartenance de groupe ou une liste de groupes. SignalR envoie des messages vers des clients et des groupes basés sur un modèle pub/sub et le serveur ne conserve pas les listes de groupes ou des appartenances aux groupes. Cela permet de maximiser l’évolutivité, car chaque fois que vous ajoutez un nœud à une batterie de serveurs web, n’importe quel état SignalR tient à jour doit être propagée vers le nouveau nœud.

Lorsque vous ajoutez un utilisateur à un groupe en utilisant le `Groups.Add` (méthode), l’utilisateur reçoit les messages dirigés vers ce groupe pour la durée de la connexion actuelle, mais l’appartenance de ce groupe n’est pas conservé au-delà de la connexion actuelle. Si vous souhaitez conserver en permanence d’informations sur les groupes et l’appartenance au groupe, vous devez stocker ces données dans un référentiel comme une base de données ou le stockage table Azure. Ensuite, chaque fois qu'un utilisateur se connecte à votre application, vous récupérer à partir du référentiel, les groupes auxquels appartient l’utilisateur et ajoutez manuellement cet utilisateur à ces groupes.

Lors de la reconnexion après une interruption temporaire, l’utilisateur nouveau joint automatiquement les groupes précédemment affecté. Réintégration automatiquement un groupe s’applique uniquement lors de la reconnexion, ne pas lors de l’établissement d’une nouvelle connexion. Un jeton de signature numérique est passé à partir du client qui contient la liste des groupes précédemment affectés. Si vous souhaitez vérifier si l’utilisateur appartient aux groupes requis, vous pouvez remplacer le comportement par défaut.

Cette rubrique comporte les sections suivantes :

- [Ajout et suppression d’utilisateurs](#add)
- [Appel des membres d’un groupe](#call)
- [Appartenance au groupe de stockage dans une base de données](#storedatabase)
- [Appartenance au groupe de stockage dans le stockage table Azure](#storeazuretable)
- [Vérification de l’appartenance au groupe lors de la reconnexion](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a>Ajout et suppression d’utilisateurs

Pour ajouter ou supprimer des utilisateurs dans un groupe, vous appelez le [ajouter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) ou [supprimer](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) méthodes et passez dans l’id de connexion de l’utilisateur et le nom du groupe en tant que paramètres. Vous n’avez pas besoin de supprimer manuellement un utilisateur à partir d’un groupe lorsque la connexion se termine.

L’exemple suivant montre le `Groups.Add` et `Groups.Remove` méthodes utilisées dans les méthodes de concentrateur.

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

Le `Groups.Add` et `Groups.Remove` méthodes exécuter de façon asynchrone.

Si vous souhaitez ajouter un client à un groupe et d’envoyer immédiatement un message au client en utilisant le groupe, vous devez vous assurer que la méthode Groups.Add se termine en premier. Les exemples de code suivants montrent comment pour ce faire, un à l’aide de code qui fonctionne dans .NET 4.5 et l’autre à l’aide de code qui fonctionne dans .NET 4.

#### <a name="asynchronous-net-45-example"></a>Asynchrone .NET 4.5 exemple

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

#### <a name="asynchronous-net-4-example"></a>Asynchrone .NET 4 exemple

[!code-csharp[Main](working-with-groups/samples/sample3.cs?highlight=3-4)]

En règle générale, vous ne devez pas inclure `await` lors de l’appel le `Groups.Remove` (méthode), car l’id de connexion que vous cherchez à supprimer peut ne plus être disponible. Dans ce cas, `TaskCanceledException` est levée après l’expiration de la demande. Si votre application doit garantir que l’utilisateur a été supprimé du groupe avant d’envoyer un message au groupe, vous pouvez ajouter `await` avant Groups.Remove et catch puis le `TaskCanceledException` exception qui peut être levée.

<a id="call"></a>

## <a name="calling-members-of-a-group"></a>Appel des membres d’un groupe

Vous pouvez envoyer des messages à tous les membres d’un groupe ou seuls les membres spécifiés du groupe, comme indiqué dans les exemples suivants.

- **Tous les** connecté les clients dans un groupe spécifié. 

    [!code-css[Main](working-with-groups/samples/sample4.css)]
- Tous les clients dans un groupe spécifié connectés **à l’exception des clients spécifiés**, identifié par l’ID de connexion. 

    [!code-csharp[Main](working-with-groups/samples/sample5.cs)]
- Tous les clients dans un groupe spécifié connectés **à l’exception du client appelant**. 

    [!code-css[Main](working-with-groups/samples/sample6.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a>Appartenance au groupe de stockage dans une base de données

Les exemples suivants montrent comment conserver les informations du groupe et utilisateur dans une base de données. Vous pouvez utiliser toute technologie d’accès aux données ; Toutefois, l’exemple ci-dessous montre comment définir des modèles à l’aide d’Entity Framework. Ces modèles d’entité correspondent aux champs et aux tables de base de données. Votre structure de données peut varier considérablement selon les besoins de votre application. Cet exemple inclut une classe nommée `ConversationRoom` qui serait unique d’une application qui permet aux utilisateurs de joindre des conversations des sujets différents, tels que sports ou jardinage. Cet exemple inclut également une classe pour les connexions. La classe de connexion n’est pas absolument nécessaire pour le suivi de l’appartenance au groupe, mais fait souvent partie d’une solution robuste pour le suivi des utilisateurs.

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

Ensuite, dans le hub, vous pouvez récupérer les informations de groupe et utilisateur de la base de données et ajouter manuellement l’utilisateur aux groupes appropriés. L’exemple n’inclut pas de code pour le suivi des connexions utilisateur. Dans cet exemple, le `await` mot clé n’est pas appliqué avant `Groups.Add` , car un message n’est pas immédiatement envoyé aux membres du groupe. Si vous souhaitez envoyer un message à tous les membres du groupe immédiatement après l’ajout du nouveau membre, vous pouvez appliquer le `await` mot clé pour vous assurer que l’opération asynchrone est terminée.

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a>Appartenance au groupe de stockage dans le stockage table Azure

L’utilisation du stockage de table Azure pour stocker les informations utilisateur et de groupe est similaire à l’utilisation d’une base de données. L’exemple suivant montre une entité de table qui stocke le nom d’utilisateur et le nom de groupe.

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

Dans le hub, vous récupérer les groupes affectés lorsque l’utilisateur se connecte.

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a>Vérification de l’appartenance au groupe lors de la reconnexion

Par défaut, SignalR nouveau affecte automatiquement un utilisateur aux groupes appropriés lors de la reconnexion à partir d’une interruption temporaire, par exemple quand une connexion est supprimée et rétablie avant l’expiration de la connexion. Informations sur les groupes de l’utilisateur sont passées dans un jeton lors de la reconnexion, et ce jeton est vérifié sur le serveur. Pour plus d’informations sur le processus de vérification de réintégration des utilisateurs aux groupes, consultez [réintégration des groupes lors de la reconnexion](index.md).

En général, vous devez utiliser le comportement par défaut de réintégration des groupes sur se reconnecter automatiquement. SignalR groupes ne sont pas conçus comme mécanisme de sécurité pour restreindre l’accès aux données sensibles. Toutefois, si votre application doit vérifier l’appartenance au groupe de l’utilisateur lors de la reconnexion, vous pouvez remplacer le comportement par défaut. Modification du comportement par défaut, un fardeau peut ajouter à votre base de données, car l’appartenance au groupe de l’utilisateur doit être récupérée pour chaque reconnexion plutôt que simplement lorsque l’utilisateur se connecte.

Si vous devez vérifier l’appartenance au groupe sur se reconnecter, créez un nouveau module de pipeline de concentrateur qui retourne une liste de groupes affectés, comme indiqué ci-dessous.

[!code-csharp[Main](working-with-groups/samples/sample11.cs)]

Ensuite, ajoutez ce module au pipeline de concentrateur, comme indiqué ci-dessous.

[!code-csharp[Main](working-with-groups/samples/sample12.cs?highlight=10)]
