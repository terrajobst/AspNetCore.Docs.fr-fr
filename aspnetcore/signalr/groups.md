---
title: Gérer les utilisateurs et groupes dans SignalR
author: rachelappel
description: Vue d’ensemble d’ASP.NET Core SignalR utilisateur et groupe de gestion.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 3e5e310c84bc3ed5790d5b67a917bd54162ea163
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402523"
---
# <a name="manage-users-and-groups-in-signalr"></a>Gérer les utilisateurs et groupes dans SignalR

Par [Brennan Conroy](https://github.com/BrennanConroy)

SignalR permet des messages à envoyer à toutes les connexions associées à un utilisateur spécifique, mais aussi à des groupes nommés de connexions.

[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(comment télécharger)](xref:tutorials/index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>Utilisateurs dans SignalR

SignalR vous permet d’envoyer des messages à toutes les connexions associées à un utilisateur spécifique. Par défaut, SignalR utilise le `ClaimTypes.NameIdentifier` à partir de la `ClaimsPrincipal` associé à la connexion comme identificateur d’utilisateur. Un seul utilisateur peut avoir plusieurs connexions à une application de SignalR. Par exemple, un utilisateur peut être connecté sur leur bureau, ainsi que leur téléphone. Chaque périphérique possède une connexion SignalR distincte, mais ils sont tous associés au même utilisateur. Si un message est envoyé à l’utilisateur, toutes les connexions associées à cet utilisateur le message. L’identificateur d’utilisateur pour une connexion est accessible par le `Context.UserIdentifier` propriété dans votre hub.

Envoyer un message à un utilisateur spécifique en passant l’identificateur d’utilisateur pour le `User` fonctionner dans votre méthode de concentrateur, comme indiqué dans l’exemple suivant :

> [!NOTE]
> L’identificateur d’utilisateur respecte la casse.

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

L’identificateur d’utilisateur peut être personnalisé en créant un `IUserIdProvider`et en l’inscrivant dans `ConfigureServices`.

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> AddSignalR doit être appelée avant l’inscription de vos services SignalR personnalisées.

## <a name="groups-in-signalr"></a>Groupes dans SignalR

Un groupe est une collection de connexions associées à un nom. Messages peuvent être envoyés à toutes les connexions dans un groupe. Les groupes sont la méthode recommandée pour l’envoyer à une connexion ou de plusieurs connexions, car les groupes sont gérés par l’application. Une connexion peut être un membre de plusieurs groupes. Cela, les groupes de parfaits pour quelque chose comme une application de conversation, où chaque pièce peut être représenté en tant que groupe. Connexions peuvent être ajoutées ou supprimées à partir de groupes via le `AddToGroupAsync` et `RemoveFromGroupAsync` méthodes.

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

L’appartenance au groupe n’est pas conservé lorsqu’une connexion se reconnecte. La connexion doit rejoindre le groupe quand elle est rétablie. Il n’est pas possible de compter les membres d’un groupe, étant donné que ces informations ne sont pas disponibles si l’application est à l’échelle vers plusieurs serveurs.

> [!NOTE]
> Les noms de groupe respectent la casse.

## <a name="related-resources"></a>Ressources connexes

* [Bien démarrer](xref:tutorials/signalr)
* [Hubs](xref:signalr/hubs)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
