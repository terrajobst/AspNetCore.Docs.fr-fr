---
title: Utilisation des hubs dans ASP.NET Core SignalR
author: tdykstra
description: Découvrez comment utiliser des hubs dans ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/12/2018
uid: signalr/hubs
ms.openlocfilehash: 17e3ee23967bc1097a3121b3e3e5b58cebe3887d
ms.sourcegitcommit: a742b55e4b8276a48b8b4394784554fecd883c84
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538360"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>Utilisation des hubs dans SignalR pour ASP.NET Core

Par [Rachel Appel](https://twitter.com/rachelappel) et [Kevin Griffin](https://twitter.com/1kevgriff)

[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(comment télécharger)](xref:tutorials/index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>Qu’est-ce qu’un hub SignalR ?

L’API Hubs de SignalR vous permet d’appeler des méthodes sur des clients connectés à partir du serveur. Dans le code serveur, vous définissez des méthodes qui sont appelées par le client. Dans le code client, vous définissez des méthodes qui sont appelées à partir du serveur. SignalR prend en charge tout ce qui se passe à l’arrière-plan et qui rend possibles les communications client-serveur et serveur-client en temps réel.

## <a name="configure-signalr-hubs"></a>Configurer les concentrateurs SignalR

Le middleware SignalR requiert certains services, qui sont configurés en appelant `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

Quand vous ajoutez des fonctionnalités SignalR à une application ASP.NET Core, configurez les routes SignalR en appelant `app.UseSignalR` dans la méthode `Startup.Configure`.

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a>Créer et utiliser les hubs

Créez un hub en déclarant une classe qui hérite de `Hub`et ajoutez-lui des méthodes publiques. Les clients peuvent appeler des méthodes qui sont définies en tant que `public`.

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

Vous pouvez spécifier un type de retour et paramètres, y compris les types complexes et les tableaux, comme vous le feriez dans n’importe quelle méthode c#. SignalR gère la sérialisation et désérialisation des objets complexes et des tableaux dans vos paramètres et valeurs de retournés.

## <a name="the-context-object"></a>L’objet de contexte

Le `Hub` classe a un `Context` propriété qui contient les propriétés suivantes avec des informations sur la connexion :

| Propriété | Description |
| ------ | ----------- |
| `ConnectionId` | Obtient l’ID unique pour la connexion affectée par SignalR. Il existe un identifiant de connexion pour chaque connexion.|
| `UserIdentifier` | Obtient le [identificateur d’utilisateur](xref:signalr/groups). Par défaut, SignalR utilise le `ClaimTypes.NameIdentifier` à partir de la `ClaimsPrincipal` associé à la connexion comme identificateur d’utilisateur. |
| `User` | Obtient le `ClaimsPrincipal` associé à l’utilisateur actuel. |
| `Items` | Obtient une collection clé/valeur qui peut être utilisée pour partager des données dans le cadre de cette connexion. Données peuvent être stockées dans cette collection et il persistera pour la connexion entre les appels de méthode de concentrateur différents. |
| `Features` | Obtient la collection de fonctionnalités disponibles sur la connexion. Pour l’instant, cette collection n’est pas nécessaire dans la plupart des scénarios, donc il n’est pas encore documentée en détail. |
| `ConnectionAborted` | Obtient un `CancellationToken` qui avertit de la connexion est abandonnée. |

`Hub.Context` contient également les méthodes suivantes :

| Méthode | Description |
| ------ | ----------- |
| `GetHttpContext` | Retourne le `HttpContext` pour la connexion, ou `null` si la connexion n’est pas associée à une requête HTTP. Pour les connexions HTTP, vous pouvez utiliser cette méthode pour obtenir des informations telles que les en-têtes HTTP et les chaînes de requête. |
| `Abort` | Abandonne la connexion. |

## <a name="the-clients-object"></a>L’objet de Clients

Le `Hub` classe a un `Clients` propriété qui contient les propriétés suivantes pour la communication entre client et serveur :

| Propriété | Description |
| ------ | ----------- |
| `All` | Appelle une méthode sur tous les clients connectés |
| `Caller` | Appelle une méthode sur le client qui a appelé la méthode de hub |
| `Others` | Appelle une méthode sur tous les clients connectés à l’exception du client qui a appelé la méthode |


`Hub.Clients` contient également les méthodes suivantes :

| Méthode | Description |
| ------ | ----------- |
| `AllExcept` | Appelle une méthode sur tous les clients connectés à l’exception des connexions spécifiées |
| `Client` | Appelle une méthode sur un client connecté spécifique |
| `Clients` | Appelle une méthode sur des clients connectés spécifiques |
| `Group` | Appelle une méthode à toutes les connexions dans le groupe spécifié  |
| `GroupExcept` | Appelle une méthode à toutes les connexions dans le groupe spécifié, à l’exception des connexions spécifiées |
| `Groups` | Appelle une méthode à plusieurs groupes de connexions  |
| `OthersInGroup` | Appelle une méthode sur un groupe de connexions, à l’exclusion du client qui a appelé la méthode de hub  |
| `User` | Appelle une méthode pour toutes les connexions associées à un utilisateur spécifique |
| `Users` | Appelle une méthode pour toutes les connexions associées aux utilisateurs spécifiés |

Chaque méthode ou propriété des tableaux précédents retourne un objet avec une méthode `SendAsync`. Le méthode `SendAsync` vous permet de fournir le nom et les paramètres de la méthode du client à appeler.

## <a name="send-messages-to-clients"></a>Envoyer des messages aux clients

Pour effectuer des appels à des clients spécifiques, utilisez les propriétés de l'objet `Clients`. Dans l’exemple suivant, la méthode `SendMessageToCaller` illustre l’envoi d’un message à la connexion qui a appelé la méthode de hub. Le méthode `SendMessageToGroups` envoie un message pour les groupes stockés dans une `List` nommée `groups`.

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="strongly-typed-hubs"></a>Hubs fortement typés

Un inconvénient de l’utilisation de `SendAsync` est qu’elle s’appuie sur une chaîne magique pour spécifier la méthode de client à appeler. Cela laisse ouvrir du code pour les erreurs d’exécution si le nom de la méthode est mal orthographié ou manquant à partir du client.

Une alternative à l’utilisation de `SendAsync` est pour typer fortement les `Hub` avec <xref:Microsoft.AspNetCore.SignalR.Hub`1>. Dans l’exemple suivant, le `ChatHub` méthodes client ont été extraite et placées dans une interface appelée `IChatClient`.  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

Cette interface peut être utilisée pour refactoriser l’exemple précédent `ChatHub` exemple.

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

À l’aide de `Hub<IChatClient>` Active la vérification de la compilation des méthodes client. Cela évite les problèmes provoqués par l’utilisation de chaînes magiques, étant donné que `Hub<T>` permettent uniquement d’accéder aux méthodes définies dans l’interface.

À l’aide de fortement typé `Hub<T>` désactive la possibilité d’utiliser `SendAsync`.

## <a name="handle-events-for-a-connection"></a>Gérer les événements pour une connexion

L’API Hubs de SignalR fournit les méthodes virtuelles `OnConnectedAsync` et `OnDisconnectedAsync` pour gérer et suivre les connexions. Remplacez la méthode virtuelle `OnConnectedAsync` pour effectuer des actions lorsqu’un client se connecte au hub, comme l’ajouter à un groupe.


[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a>Gérer les erreurs

Les exceptions levées dans vos méthodes de hub sont envoyées au client qui a appelé la méthode. Sur le client JavaScript, la méthode `invoke` retourne une [promesse JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). Lorsque le client reçoit une erreur avec un gestionnaire attaché à la promesse avec `catch`, elle est appelée et passée en tant qu’objet JavaScript `Error`.


[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a>Ressources connexes

* [Introduction à ASP.NET Core SignalR](xref:signalr/introduction)
* [Client JavaScript](xref:signalr/javascript-client)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
