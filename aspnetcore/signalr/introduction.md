---
title: Présentation de ASP.NET Core SignalR
author: bradygaster
description: Découvrez comment la bibliothèque de SignalR ASP.NET Core simplifie l’ajout de fonctionnalités en temps réel aux applications.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/introduction
ms.openlocfilehash: 7108d9f223db78937dd1203a1cb4b890006b20ec
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963940"
---
# <a name="introduction-to-aspnet-core-opno-locsignalr"></a>Présentation de ASP.NET Core SignalR

## <a name="what-is-opno-locsignalr"></a>Qu’est-ce que SignalR?

ASP.NET Core SignalR est une bibliothèque open source qui simplifie l’ajout de fonctionnalités Web en temps réel aux applications. La fonctionnalité Web en temps réel permet au code côté serveur de transmettre instantanément du contenu aux clients.

Bons candidats pour SignalR:

* Applications qui requièrent des mises à jour à fréquence élevée à partir du serveur. Les exemples sont les jeux, les réseaux sociaux, les votes, les enchères, les cartes et les applications GPS.
* Tableaux de bord et applications de surveillance. Les exemples incluent des tableaux de bord d’entreprise, des mises à jour de ventes instantanées ou des alertes de voyage.
* Applications collaboratives. Les applications de tableau blanc et le logiciel de réunion d’équipe sont des exemples d’applications de collaboration.
* Applications qui requièrent des notifications. Les réseaux sociaux, la messagerie électronique, la conversation, les jeux, les alertes de voyage et beaucoup d’autres applications utilisent des notifications.

SignalR fournit une API pour la création d' [appels de procédure distante (RPC)](https://wikipedia.org/wiki/Remote_procedure_call)de serveur à client. Les RPC appellent des fonctions JavaScript sur les clients à partir du code .NET Core côté serveur.

Voici certaines fonctionnalités de SignalR pour ASP.NET Core :

* Gère automatiquement la gestion des connexions.
* Envoie des messages à tous les clients connectés simultanément. Par exemple, une salle de conversation.
* Envoie des messages à des clients ou groupes de clients spécifiques.
* Met à l’échelle pour gérer le trafic qui augmente.

La source est hébergée dans un [référentielSignalR sur GitHub](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).

## <a name="transports"></a>Transports

SignalR prend en charge plusieurs techniques pour gérer les communications en temps réel :

* [WebSockets](https://tools.ietf.org/html/rfc7118)
* Événements envoyés par le serveur
* Interrogation longue

SignalR choisit automatiquement la meilleure méthode de transport parmi les capacités du serveur et du client.

## <a name="hubs"></a>Hubs

SignalR utilise des *hubs* pour la communication entre les clients et les serveurs.

Un Hub est un pipeline de haut niveau qui permet à un client et un serveur d’appeler des méthodes les unes sur les autres. SignalR gère automatiquement la distribution à travers les limites de l’ordinateur, ce qui permet aux clients d’appeler des méthodes sur le serveur et vice versa. Vous pouvez passer des paramètres fortement typés à des méthodes, ce qui active la liaison de modèle. SignalR fournit deux protocoles Hub intégrés : un protocole texte basé sur JSON et un protocole binaire basé sur [MessagePack](https://msgpack.org/).  MessagePack crée généralement des messages plus petits par rapport à JSON. Les anciens navigateurs doivent prendre en charge le [niveau 2 de XHR](https://caniuse.com/#feat=xhr2) pour fournir la prise en charge du protocole MessagePack.

Les hubs appellent le code côté client en envoyant des messages qui contiennent le nom et les paramètres de la méthode côté client. Les objets envoyés en tant que paramètres de méthode sont désérialisés à l’aide du protocole configuré. Le client tente de faire correspondre le nom à une méthode dans le code côté client. Lorsque le client trouve une correspondance, il appelle la méthode et lui passe les données de paramètre désérialisées.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Prise en main de SignalR pour ASP.NET Core](xref:tutorials/signalr)
* [Plateformes prises en charge](xref:signalr/supported-platforms)
* [Hubs](xref:signalr/hubs)
* [Client JavaScript](xref:signalr/javascript-client)
