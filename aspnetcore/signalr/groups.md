---
title: Gérer les utilisateurs et des groupes dans SignalR
author: bradygaster
description: Vue d’ensemble de la gestion des utilisateurs et des groupes dans ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 180f8b4551eea39cc340bf1d250f4575cb5f71ed
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087431"
---
# <a name="manage-users-and-groups-in-signalr"></a>Gérer les utilisateurs et des groupes dans SignalR

Par [Brennan Conroy](https://github.com/BrennanConroy)

SignalR permet d'envoyer des messages à toutes les connexions associées à un utilisateur spécifique, mais aussi à des groupes de connexions nommés.

[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/groups/sample/) [(comment télécharger)](xref:index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>Utilisateurs dans SignalR

SignalR vous permet d’envoyer des messages à toutes les connexions associées à un utilisateur spécifique. Par défaut, SignalR utilise le `ClaimTypes.NameIdentifier` du `ClaimsPrincipal` associé à la connexion comme identificateur d’utilisateur. Un utilisateur peut avoir plusieurs connexions à une application SignalR. Par exemple, il peut être connecté sur son ordinateur de bureau et sur son téléphone. Chaque appareil a une connexion SignalR distincte, mais ils sont tous associés au même utilisateur. Si un message est envoyé à l’utilisateur, toutes les connexions associées à cet utilisateur reçoivent le message. L’identificateur d’utilisateur pour une connexion est accessible au moyen de la propriété `Context.UserIdentifier` dans votre hub.

Envoyez un message à un utilisateur spécifique en passant l’identificateur d’utilisateur à la fonction `User` dans la méthode de votre hub, comme indiqué dans l’exemple suivant :

> [!NOTE]
> L’identificateur d’utilisateur respecte la casse.

[!code-csharp[Configure service](groups/sample/hubs/chathub.cs?range=29-32)]

## <a name="groups-in-signalr"></a>Groupes dans SignalR

Un groupe est une collection de connexions associées à un nom. Les messages peuvent être envoyés à toutes les connexions dans un groupe. Les groupes sont recommandés pour envoyer des messages à une ou plusieurs connexions, car ils sont gérés par l’application. Une connexion peut appartenir à plusieurs groupes. Les groupes conviennent donc parfaitement à certaines choses, notamment les applications de conversation dans lesquelles chaque salle peut être représentée par un groupe. Les connexions peuvent être ajoutées à des groupes ou supprimées de ceux-ci à l'aide des méthodes `AddToGroupAsync` et `RemoveFromGroupAsync`.

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

L’appartenance au groupe n’est pas conservée quand une connexion se reconnecte. La connexion doit rejoindre le groupe quand elle est rétablie. Il n’est pas possible de compter les membres d’un groupe, car ces informations ne sont pas disponibles si l’application est mise à l'échelle sur plusieurs serveurs.

Pour protéger l’accès aux ressources lors de l’utilisation de groupes, utilisez [authentification et autorisation](xref:signalr/authn-and-authz) fonctionnalités dans ASP.NET Core. Si vous ajoutez uniquement des utilisateurs à un groupe lorsque les informations d’identification sont valides pour ce groupe, les messages envoyés à ce groupe passera uniquement aux utilisateurs autorisés. Toutefois, les groupes ne sont pas une fonctionnalité de sécurité. L’authentification par revendications possèdent des fonctionnalités groupes ne le faites pas, telles que l’expiration et la révocation. Si l’autorisation d’un utilisateur pour accéder au groupe est révoquée, vous devez manuellement détecte et le supprimer du groupe.

> [!NOTE]
> Les noms de groupe respectent la casse.

## <a name="related-resources"></a>Ressources connexes

* [Bien démarrer](xref:tutorials/signalr)
* [Hubs](xref:signalr/hubs)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
