---
title: Utilisation des hubs dans ASP.NET Core SignalR
author: rachelappel
description: En savoir plus sur l’utilisation des hubs dans ASP.NET Core SignalR.
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: 73397ba6951f2752653575920bdf739439874366
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/03/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>Utilisation des hubs dans SignalR pour ASP.NET Core

Par [Rachel Appel](https://twitter.com/rachelappel) et [Kevin Griffin](https://twitter.com/1kevgriff)

## <a name="what-is-a-signalr-hub"></a>Qu’est un concentrateur SignalR

L’API de concentrateurs SignalR vous permet d’appeler des méthodes sur les clients connectés à partir du serveur. Dans le code serveur, définir des méthodes qui sont appelées par le client. Dans le code client, définir des méthodes qui sont appelées à partir du serveur. SignalR prend en charge de tous les éléments en arrière-plan qui permet des communications client-serveur et client-server en temps réel.

## <a name="configure-signalr-hubs"></a>Configurer les concentrateurs SignalR

L’intergiciel (middleware) SignalR requiert certains services, qui sont configurés en appelant `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=35)]

Lorsque vous ajoutez des fonctionnalités de SignalR pour une application ASP.NET Core, configurer les itinéraires SignalR en appelant `app.UseSignalR` dans le `Startup.Configure` (méthode).

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=55-58)]

## <a name="create-and-use-hubs"></a>Créer et utiliser les concentrateurs

Créer un hub en déclarant une classe qui hérite de `Hub`et lui ajouter des méthodes publiques. Les clients peuvent appeler des méthodes qui sont définies en tant que `public`.

[!code-csharp[Create and use hubs](hubs/sample/chathub.cs?range=10-13)]

Vous pouvez spécifier un type de retour et des paramètres, y compris les types complexes et les tableaux, comme vous le feriez dans n’importe quelle méthode c#. SignalR gère la sérialisation et désérialisation d’objets complexes et les tableaux dans vos paramètres et les valeurs de retour.

## <a name="the-clients-object"></a>L’objet de Clients

Chaque instance de la `Hub` classe a une propriété nommée `Clients` qui contient les membres suivants pour la communication entre le serveur et le client :

| Propriété | Description |
| ------ | ----------- |
| `All` | Appelle une méthode sur tous les clients connectés |
| `Caller` | Appelle une méthode sur le client qui a appelé la méthode de concentrateur |
| `Others` | Appelle une méthode sur tous les clients connectés à l’exception du client qui a appelé la méthode |

En outre, la `Hub` classe contient les méthodes suivantes :

| Méthode | Description |
| ------ | ----------- |
| `AllExcept` | Appelle une méthode sur tous les clients connectés à l’exception des connexions spécifiées |
| `Client` | Appelle une méthode sur un client connecté spécifique |
| `Clients` | Appelle une méthode sur les clients connectés spécifiques |
| `Group` | Envoie un message à toutes les connexions dans le groupe spécifié  |
| `GroupExcept` | Envoie un message à toutes les connexions dans le groupe spécifié, à l’exception des connexions spécifiées |
| `Groups` | Envoie un message à plusieurs groupes de connexions  |
| `OthersInGroup` | Envoie un message à un groupe de connexions, à l’exclusion du client qui a appelé la méthode de concentrateur  |
| `User` | Envoie un message à toutes les connexions associées à un utilisateur spécifique |
| `Users` | Envoie un message à toutes les connexions associées aux utilisateurs spécifiés |

Chaque méthode ou propriété dans les tables précédentes retourne un objet avec un `SendAsync` (méthode). Le `SendAsync` méthode vous permet de fournir le nom et les paramètres de la méthode du client à appeler.

## <a name="send-messages-to-clients"></a>Envoyer des messages aux clients

Pour effectuer des appels à des clients spécifiques, utilisez les propriétés de la `Clients` objet. Dans l’exemple suivant dans l’exemple suivant, la `SendMessageToCaller` méthode illustre l’envoi d’un message à la connexion qui a appelé la méthode de concentrateur. Le `SendMessageToGroups` méthode envoie un message pour les groupes stockés dans un `List` nommé `groups`.

[!code-csharp[Send messages](hubs/sample/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a>Gérer les événements pour une connexion

L’API de concentrateurs SignalR fournit le `OnConnectedAsync` et `OnDisconnectedAsync` méthodes virtuelles pour gérer et suivre les connexions. Remplacer la `OnConnectedAsync` méthode virtuelle pour effectuer des actions lorsqu’un client se connecte au concentrateur, telles que l’ajouter à un groupe.

[!code-csharp[Handle events](hubs/sample/chathub.cs?range=26-30)]

## <a name="handle-errors"></a>Gérer les erreurs

Les exceptions levées dans vos méthodes de concentrateur sont envoyées au client qui a appelé la méthode. Sur le client JavaScript, le `invoke` méthode retourne un [méthode JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). Lorsque le client reçoit une erreur avec un gestionnaire attaché l’à l’aide de la promesse `catch`, elle a appelée et passée comme un code JavaScript `Error` objet.

[!code-javascript[Error](hubs/sample/chat.js?range=20)]
[!code-javascript[Error](hubs/sample/chat.js?range=16-18)]

## <a name="related-resources"></a>Ressources connexes

[Présentation d’ASP.NET Core SignalR](xref:signalr/introduction)