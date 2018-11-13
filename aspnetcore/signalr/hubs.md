---
title: Utilisation des hubs dans ASP.NET Core SignalR
author: tdykstra
description: Découvrez comment utiliser des hubs dans ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/07/2018
uid: signalr/hubs
ms.openlocfilehash: 0413d354307208726f4252f431ac59526effed08
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51569917"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>Utilisation des hubs dans SignalR pour ASP.NET Core

Par [Rachel Appel](https://twitter.com/rachelappel) et [Kevin Griffin](https://twitter.com/1kevgriff)

[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(comment télécharger)](xref:index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>Qu’est-ce qu’un hub SignalR ?

L’API Hubs de SignalR vous permet d’appeler des méthodes sur des clients connectés à partir du serveur. Dans le code serveur, vous définissez des méthodes qui sont appelées par le client. Dans le code client, vous définissez des méthodes qui sont appelées à partir du serveur. SignalR prend en charge tout ce qui se passe à l’arrière-plan et qui rend possibles les communications client-serveur et serveur-client en temps réel.

## <a name="configure-signalr-hubs"></a>Configurer les hubs SignalR

Le middleware SignalR requiert certains services, qui sont configurés en appelant `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

Quand vous ajoutez des fonctionnalités SignalR à une application ASP.NET Core, configurez les routes SignalR en appelant `app.UseSignalR` dans la méthode `Startup.Configure`.

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a>Créer et utiliser les hubs

Créez un hub en déclarant une classe qui hérite de `Hub`et ajoutez-lui des méthodes publiques. Les clients peuvent appeler des méthodes qui sont définies en tant que `public`.

```csharp
public class ChatHub : Hub
{
    public Task SendMessage(string user, string message)
    {
        return Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

Vous pouvez spécifier un type de retour et paramètres, y compris les types complexes et les tableaux, comme vous le feriez dans n’importe quelle méthode c#. SignalR gère la sérialisation et désérialisation des objets complexes et des tableaux dans vos paramètres et valeurs de retournés.

> [!NOTE]
> Les concentrateurs sont temporaires :
> * Ne pas stocker l’état dans une propriété sur la classe de concentrateur. Chaque appel de méthode de concentrateur est exécutée sur une nouvelle instance de concentrateur.  
> * Utilisez `await` lors de l’appel de méthodes asynchrones qui dépendent du hub de rester actif. Par exemple, une méthode telle que `Clients.All.SendAsync(...)` peut échouer si elle est appelée sans `await` et la méthode de concentrateur se termine avant `SendAsync` se termine.

## <a name="the-context-object"></a>L’objet de contexte

La classe `Hub` a une propriété `Context` qui contient les propriétés suivantes avec des informations sur la connexion :

| Propriété | Description |
| ------ | ----------- |
| `ConnectionId` | Obtient l’ID unique pour la connexion affectée par SignalR. Il existe un identifiant de connexion pour chaque connexion.|
| `UserIdentifier` | Obtient l'[identificateur d’utilisateur](xref:signalr/groups). Par défaut, SignalR utilise le `ClaimTypes.NameIdentifier` provenant du `ClaimsPrincipal` associé à la connexion comme identificateur d’utilisateur. |
| `User` | Obtient le `ClaimsPrincipal` associé à l’utilisateur actuel. |
| `Items` | Obtient une collection clé/valeur qui peut être utilisée pour partager des données dans le cadre de cette connexion. Données peuvent être stockées dans cette collection et il persistera pour la connexion entre les appels de méthode de concentrateur différents. |
| `Features` | Obtient la collection de fonctionnalités disponibles sur la connexion. Pour l’instant, cette collection n’est pas nécessaire dans la plupart des scénarios, donc elle n’est pas encore documentée en détail. |
| `ConnectionAborted` | Obtient un `CancellationToken` qui avertit quand connexion est abandonnée. |

`Hub.Context` contient également les méthodes suivantes :

| Méthode | Description |
| ------ | ----------- |
| `GetHttpContext` | Retourne le `HttpContext` pour la connexion, ou `null` si la connexion n’est pas associée à une requête HTTP. Pour les connexions HTTP, vous pouvez utiliser cette méthode pour obtenir des informations telles que les en-têtes HTTP et les chaînes de requête. |
| `Abort` | Abandonne la connexion. |

## <a name="the-clients-object"></a>L’objet de Clients

La classe `Hub` a une propriété `Clients` qui contient les propriétés suivantes pour la communication entre client et serveur :

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
| `Group` | Appelle une méthode sur toutes les connexions dans le groupe spécifié  |
| `GroupExcept` | Appelle une méthode sur toutes les connexions dans le groupe spécifié, à l’exception des connexions spécifiées |
| `Groups` | Appelle une méthode sur plusieurs groupes de connexions  |
| `OthersInGroup` | Appelle une méthode sur un groupe de connexions, à l’exclusion du client qui a appelé la méthode de concentrateur  |
| `User` | Appelle une méthode sur toutes les connexions associées à un utilisateur spécifique |
| `Users` | Appelle une méthode sur toutes les connexions associées aux utilisateurs spécifiés |

Chaque méthode ou propriété des tableaux précédents retourne un objet avec une méthode `SendAsync`. Le méthode `SendAsync` vous permet de fournir le nom et les paramètres de la méthode du client à appeler.

## <a name="send-messages-to-clients"></a>Envoyer des messages aux clients

Pour effectuer des appels à des clients spécifiques, utilisez les propriétés de l'objet `Clients`. Dans l’exemple suivant, il existe trois méthodes de concentrateur :

* `SendMessage` envoie un message à tous les clients connectés, à l’aide de `Clients.All`.
* `SendMessageToCaller` envoie un message à l’appelant, à l’aide de `Clients.Caller`.
* `SendMessageToGroups` envoie un message à tous les clients dans le `SignalR Users` groupe.

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?name=HubMethods)]

## <a name="strongly-typed-hubs"></a>Hubs fortement typés

Un inconvénient de l’utilisation de `SendAsync` est qu’elle s’appuie sur une chaîne en dur pour spécifier la méthode de client à appeler. Cela laisse le code ouvert à des erreurs d’exécution si le nom de la méthode est mal orthographié ou manquant à partir du client.

Une alternative à l’utilisation de `SendAsync` est pour typer fortement les `Hub` avec <xref:Microsoft.AspNetCore.SignalR.Hub`1>. Dans l’exemple suivant, le `ChatHub` méthodes client ont été extraite et placées dans une interface appelée `IChatClient`.  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

Cette interface peut être utilisée pour refactoriser l’exemple précédent `ChatHub` .

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

À l’aide de `Hub<IChatClient>` Active la vérification de la compilation des méthodes client. Cela évite les problèmes provoqués par l’utilisation de chaînes magiques, étant donné que `Hub<T>` permettent uniquement d’accéder aux méthodes définies dans l’interface.

Utiliser un `Hub<T>` fortement typé désactive la possibilité d’utiliser `SendAsync`.

## <a name="change-the-name-of-a-hub-method"></a>Modifier le nom d’une méthode de concentrateur

Par défaut, un nom de méthode de concentrateur de serveur est le nom de la méthode .NET. Toutefois, vous pouvez utiliser la [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) attribut à modifier cette valeur par défaut et spécifier manuellement un nom pour la méthode. Le client doit utiliser ce nom, au lieu du nom de méthode .NET, lors de l’appel de la méthode.

[!code-csharp[HubMethodName attribute](hubs/sample/hubs/chathub.cs?name=HubMethodName&highlight=1)]

## <a name="handle-events-for-a-connection"></a>Gérer les événements pour une connexion

L’API Hubs de SignalR fournit les méthodes virtuelles `OnConnectedAsync` et `OnDisconnectedAsync` pour gérer et suivre les connexions. Remplacez la méthode virtuelle `OnConnectedAsync` pour effectuer des actions lorsqu’un client se connecte au hub, comme l’ajouter à un groupe.


[!code-csharp[Handle connection](hubs/sample/hubs/chathub.cs?name=OnConnectedAsync)]

Remplacer le `OnDisconnectedAsync` une méthode virtuelle pour effectuer des actions lorsqu’un client se déconnecte. Si le client se déconnecte volontairement (en appelant `connection.stop()`, par exemple), le `exception` paramètre sera `null`. Toutefois, si le client est déconnecté à cause d’une erreur (par exemple, une panne de réseau), le `exception` paramètre contiendra une exception qui décrit l’échec.

[!code-csharp[Handle disconnection](hubs/sample/hubs/chathub.cs?name=OnDisconnectedAsync)]

## <a name="handle-errors"></a>Gérer les erreurs

Les exceptions levées dans vos méthodes de hub sont envoyées au client qui a appelé la méthode. Sur le client JavaScript, la méthode `invoke` retourne une [promesse JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). Lorsque le client reçoit une erreur avec un gestionnaire attaché à la promesse avec `catch`, elle est appelée et passée en tant qu’objet JavaScript `Error`.


[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

Par défaut, si votre Hub lève une exception, SignalR retourne un message d’erreur générique au client. Exemple :

```
Microsoft.AspNetCore.SignalR.HubException: An unexpected error occurred invoking 'MethodName' on the server.
```

Exceptions inattendues contiennent souvent des informations sensibles, telles que le nom d’un serveur de base de données dans une exception déclenchée en cas d’échec de la connexion de base de données. SignalR n’expose pas ces messages d’erreur détaillés par défaut par mesure de sécurité. Consultez le [article considérations de sécurité](xref:signalr/security#exceptions) pour plus d’informations sur la raison pour laquelle les détails de l’exception sont supprimés.

Si vous avez une exceptionnelle condition vous *faire* à propager vers le client, vous pouvez utiliser la `HubException` classe. Si vous levez une `HubException` à partir de votre méthode de concentrateur, SignalR **sera** envoyer l’intégralité du message au client, tels quels.

[!code-csharp[ThrowHubException](hubs/sample/hubs/chathub.cs?name=ThrowHubException&highlight=3)]

> [!NOTE]
> SignalR envoie uniquement le `Message` propriété de l’exception au client. La trace de pile et d’autres propriétés sur l’exception ne sont pas disponibles pour le client.

## <a name="related-resources"></a>Ressources connexes

* [Introduction à ASP.NET Core SignalR](xref:signalr/introduction)
* [Client JavaScript](xref:signalr/javascript-client)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
