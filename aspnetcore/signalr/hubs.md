---
title: Utilisation des hubs dans ASP.NET Core SignalR
author: rachelappel
description: En savoir plus sur l’utilisation des hubs dans ASP.NET Core SignalR.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/01/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: 495aa156dd5e4641d688d7b16df1e5814c9607f4
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34819082"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>Utilisation des hubs dans SignalR pour ASP.NET Core

Par [Rachel Appel](https://twitter.com/rachelappel) et [Kevin Griffin](https://twitter.com/1kevgriff)

[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(comment télécharger)](xref:tutorials/index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>Qu’est-ce qu’un hub SignalR ?

L’API Hubs de SignalR vous permet d’appeler des méthodes sur des clients connectés à partir du serveur. Dans le code serveur, vous définissez des méthodes qui sont appelées par le client. Dans le code client, vous définissez des méthodes qui sont appelées à partir du serveur. SignalR prend en charge tout ce qui se passe à l’arrière-plan et qui rend possibles les communications client-serveur et serveur-client en temps réel.

## <a name="configure-signalr-hubs"></a>Configurer les concentrateurs SignalR

L’intergiciel (middleware) SignalR requiert certains services, qui sont configurés en appelant `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

Quand vous ajoutez des fonctionnalités SignalR à une application ASP.NET Core, configurez les routes SignalR en appelant `app.UseSignalR` dans la méthode `Startup.Configure`.

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a>Créer et utiliser les hubs

Créez un hub en déclarant une classe qui hérite de `Hub`et ajoutez-lui des méthodes publiques. Les clients peuvent appeler des méthodes qui sont définies en tant que `public`.

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

Vous pouvez spécifier un type de retour et des paramètres, y compris les types complexes et les tableaux, comme vous le feriez dans n’importe quelle méthode c#. SignalR gère la sérialisation et désérialisation d’objets complexes et les tableaux dans vos paramètres et les valeurs de retour.

## <a name="the-clients-object"></a>L’objet de Clients

Chaque instance de la classe `Hub` a une propriété nommée `Clients` qui contient les membres suivants pour la communication entre le serveur et le client :


| Propriété | Description |
| ------ | ----------- |
| `All` | Appelle une méthode sur tous les clients connectés |
| `Caller` | Appelle une méthode sur le client qui a appelé la méthode de hub |
| `Others` | Appelle une méthode sur tous les clients connectés à l’exception du client qui a appelé la méthode |


En outre, `Hub.Clients` contient les méthodes suivantes :

| Méthode | Description |
| ------ | ----------- |
| `AllExcept` | Appelle une méthode sur tous les clients connectés à l’exception des connexions spécifiées |
| `Client` | Appelle une méthode sur un client connecté spécifique |
| `Clients` | Appelle une méthode sur les clients connectés spécifiques |
| `Group` | Appelle une méthode à toutes les connexions dans le groupe spécifié  |
| `GroupExcept` | Appelle une méthode à toutes les connexions dans le groupe spécifié, à l’exception des connexions spécifiées |
| `Groups` | Appelle une méthode à plusieurs groupes de connexions  |
| `OthersInGroup` | Appelle une méthode sur un groupe de connexions, à l’exclusion du client qui a appelé la méthode de hub  |
| `User` | Appelle une méthode à toutes les connexions associées à un utilisateur spécifique |
| `Users` | Appelle une méthode à toutes les connexions associées aux utilisateurs spécifiés |

Chaque méthode ou propriété des tableaux précédents retourne un objet avec une méthode `SendAsync`. Le méthode `SendAsync` vous permet de fournir le nom et les paramètres de la méthode du client à appeler.

## <a name="send-messages-to-clients"></a>Envoyer des messages aux clients

Pour effectuer des appels à des clients spécifiques, utilisez les propriétés de l'objet `Clients`. Dans l’exemple suivant, la méthode `SendMessageToCaller` illustre l’envoi d’un message à la connexion qui a appelé la méthode de hub. Le méthode `SendMessageToGroups` envoie un message pour les groupes stockés dans une `List` nommée `groups`.

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a>Gérer les événements pour une connexion

L’API Hubs de SignalR fournit les méthodes virtuelles `OnConnectedAsync` et `OnDisconnectedAsync` pour gérer et suivre les connexions. Remplacez la méthode virtuelle `OnConnectedAsync` pour effectuer des actions lorsqu’un client se connecte au hub, comme l’ajouter à un groupe.


[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a>Gérer les erreurs

Les exceptions levées dans vos méthodes de hub sont envoyées au client qui a appelé la méthode. Sur le client JavaScript, la méthode `invoke` retourne une [promesse JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). Lorsque le client reçoit une erreur avec un gestionnaire attaché à la promesse avec `catch`, elle est appelée et passée en tant qu’objet JavaScript `Error`.


[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a>Ressources connexes

* [Présentation d’ASP.NET Core SignalR](xref:signalr/introduction)
* [Client JavaScript](xref:signalr/javascript-client)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
