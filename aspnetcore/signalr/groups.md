---
title: Gérer les utilisateurs et des groupes dans SignalR
author: tdykstra
description: Vue d’ensemble de la gestion des utilisateurs et des groupes dans ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: d3e580dfc42a36762358899892831c8b68f544b0
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207158"
---
# <a name="manage-users-and-groups-in-signalr"></a>Gérer les utilisateurs et des groupes dans SignalR

Par [Brennan Conroy](https://github.com/BrennanConroy)

SignalR permet d'envoyer des messages à toutes les connexions associées à un utilisateur spécifique, mais aussi à des groupes de connexions nommés.

[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(comment télécharger)](xref:index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>Utilisateurs dans SignalR

SignalR vous permet d’envoyer des messages à toutes les connexions associées à un utilisateur spécifique. Par défaut, SignalR utilise le `ClaimTypes.NameIdentifier` du `ClaimsPrincipal` associé à la connexion comme identificateur d’utilisateur. Un utilisateur peut avoir plusieurs connexions à une application SignalR. Par exemple, il peut être connecté sur son ordinateur de bureau et sur son téléphone. Chaque appareil a une connexion SignalR distincte, mais ils sont tous associés au même utilisateur. Si un message est envoyé à l’utilisateur, toutes les connexions associées à cet utilisateur reçoivent le message. L’identificateur d’utilisateur pour une connexion est accessible au moyen de la propriété `Context.UserIdentifier` dans votre hub.

Envoyez un message à un utilisateur spécifique en passant l’identificateur d’utilisateur à la fonction `User` dans la méthode de votre hub, comme indiqué dans l’exemple suivant :

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

Un groupe est une collection de connexions associées à un nom. Les messages peuvent être envoyés à toutes les connexions dans un groupe. Les groupes sont recommandés pour envoyer des messages à une ou plusieurs connexions, car ils sont gérés par l’application. Une connexion peut appartenir à plusieurs groupes. Les groupes conviennent donc parfaitement à certaines choses, notamment les applications de conversation dans lesquelles chaque salle peut être représentée par un groupe. Les connexions peuvent être ajoutées à des groupes ou supprimées de ceux-ci à l'aide des méthodes `AddToGroupAsync` et `RemoveFromGroupAsync`.

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

L’appartenance au groupe n’est pas conservée quand une connexion se reconnecte. La connexion doit rejoindre le groupe quand elle est rétablie. Il n’est pas possible de compter les membres d’un groupe, car ces informations ne sont pas disponibles si l’application est mise à l'échelle sur plusieurs serveurs.

> [!NOTE]
> Les noms de groupe respectent la casse.

## <a name="related-resources"></a>Ressources connexes

* [Bien démarrer](xref:tutorials/signalr)
* [Hubs](xref:signalr/hubs)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
