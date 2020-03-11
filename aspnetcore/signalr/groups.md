---
title: Gérer les utilisateurs et les groupes dans SignalR
author: bradygaster
description: Vue d’ensemble de la gestion des utilisateurs et des groupes ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/groups
ms.openlocfilehash: 7e8c85dcbc46daa68988374f499f19a9d2cead47
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665864"
---
# <a name="manage-users-and-groups-in-opno-locsignalr"></a>Gérer les utilisateurs et les groupes dans SignalR

Par [Brennan Conroy](https://github.com/BrennanConroy)

SignalR permet d’envoyer des messages à toutes les connexions associées à un utilisateur spécifique, ainsi qu’à des groupes nommés de connexions.

[Afficher ou télécharger l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/groups/sample/) [(procédure de téléchargement)](xref:index#how-to-download-a-sample)

## <a name="users-in-opno-locsignalr"></a>Utilisateurs dans SignalR

SignalR vous permet d’envoyer des messages à toutes les connexions associées à un utilisateur spécifique. Par défaut, SignalR utilise le `ClaimTypes.NameIdentifier` à partir de la `ClaimsPrincipal` associée à la connexion en tant qu’identificateur de l’utilisateur. Un seul utilisateur peut avoir plusieurs connexions à une application SignalR. Par exemple, il peut être connecté sur son ordinateur de bureau et sur son téléphone. Chaque appareil dispose d’une connexion SignalR distincte, mais elles sont toutes associées au même utilisateur. Si un message est envoyé à l’utilisateur, toutes les connexions associées à cet utilisateur reçoivent le message. L’identificateur d’utilisateur pour une connexion est accessible à l’aide de la propriété `Context.UserIdentifier` de votre Hub.

Envoyez un message à un utilisateur spécifique en passant l’identificateur d’utilisateur à la fonction `User` dans votre méthode de concentrateur, comme indiqué dans l’exemple suivant :

> [!NOTE]
> L’identificateur d’utilisateur respecte la casse.

[!code-csharp[Configure service](groups/sample/hubs/chathub.cs?range=29-32)]

## <a name="groups-in-opno-locsignalr"></a>Groupes dans SignalR

Un groupe est une collection de connexions associées à un nom. Les messages peuvent être envoyés à toutes les connexions dans un groupe. Les groupes sont recommandés pour envoyer des messages à une ou plusieurs connexions, car ils sont gérés par l’application. Une connexion peut appartenir à plusieurs groupes. Les groupes conviennent donc parfaitement à certaines choses, notamment les applications de conversation dans lesquelles chaque salle peut être représentée par un groupe. Les connexions peuvent être ajoutées ou supprimées des groupes via les méthodes `AddToGroupAsync` et `RemoveFromGroupAsync`.

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

L’appartenance au groupe n’est pas conservée quand une connexion se reconnecte. La connexion doit rejoindre le groupe lorsqu’elle est rétablie. Il n’est pas possible de compter les membres d’un groupe, car ces informations ne sont pas disponibles si l’application est mise à l'échelle sur plusieurs serveurs.

Pour protéger l’accès aux ressources lors de l’utilisation de groupes, utilisez la fonctionnalité d' [authentification et d’autorisation](xref:signalr/authn-and-authz) dans ASP.net core. Si vous ajoutez uniquement des utilisateurs à un groupe lorsque les informations d’identification sont valides pour ce groupe, les messages envoyés à ce groupe sont uniquement dirigés vers les utilisateurs autorisés. Toutefois, les groupes ne sont pas une fonctionnalité de sécurité. Les revendications d’authentification ont des fonctionnalités que les groupes n’ont pas, telles que l’expiration et la révocation. Si l’autorisation d’un utilisateur d’accéder au groupe est révoquée, vous devez la détecter manuellement et la supprimer du groupe.

> [!NOTE]
> Les noms de groupes respectent la casse.

## <a name="related-resources"></a>Ressources associées

* [Prise en main](xref:tutorials/signalr)
* [Hubs](xref:signalr/hubs)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
