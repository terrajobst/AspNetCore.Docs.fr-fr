---
title: Introduction à ASP.NET Core SignalR
author: tdykstra
description: Découvrez comment la bibliothèque ASP.NET Core SignalR simplifie l’ajout de fonctionnalités en temps réel aux applications.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: 2fff24609caf7592bad763a077288990a29617aa
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342547"
---
# <a name="introduction-to-aspnet-core-signalr"></a>Introduction à ASP.NET Core SignalR

## <a name="what-is-signalr"></a>Nouveautés SignalR

ASP.NET Core SignalR est une bibliothèque open source qui simplifie l’ajout de fonctionnalités web en temps réel aux applications. Les fonctionnalités web en temps réel permettent au code côté serveur de transmettre le contenu aux clients instantanément.	

De bons candidats pour SignalR sont :

* Des applications qui nécessitent des mises à jour à haute fréquence à partir du serveur. Des exemples sont des applications de jeux, de réseaux sociaux, de vote, de vente aux enchères, de cartographie et de géolocalisation.
* Des tableaux de bord et des applications de surveillance. Des exemples incluent des tableaux de bord, des mises à jour de ventes instantanées, ou des alertes de voyage.
* Des applications de collaboration. Des applications de tableau blanc et des logiciels de réunion d’équipe sont des exemples d’applications de collaboration.
* Des applications qui nécessitent des notifications. Les applications de réseaux sociaux, e-mail, chat, jeux, d'alertes de voyage et de nombreuses autres applications utilisent des notifications.

SignalR fournit une API pour la création [d'appels de procédure distante (RPC)](https://wikipedia.org/wiki/Remote_procedure_call) client-serveur. Les appels RPC appellent des fonctions JavaScript sur les clients à partir de code côté serveur en .NET Core.

Voici certaines fonctionnalités de SignalR pour ASP.NET Core :

* Gestion automatique des connexions.
* Envoi de messages à tous les clients connectés simultanément. Par exemple, une salle de conversation.
* Envoi de messages à des clients spécifiques ou des groupes de clients.
* Gestion de la montée en charge lors de l'augmentation de trafic.

La source est hébergée dans un [dépôt SignalR sur GitHub](https://github.com/aspnet/signalr).

## <a name="transports"></a>Transports

SignalR prend en charge plusieurs techniques pour la gestion des communications en temps réel :

* [WebSockets](https://tools.ietf.org/html/rfc7118)
* Événements envoyés par le serveur
* Interrogation longue (Long polling)

SignalR choisit automatiquement la meilleure méthode de transport qui se trouve dans les fonctionnalités du serveur et client.

## <a name="hubs"></a>Hubs

SignalR utilise des *hubs* pour communiquer entre les clients et serveurs.

Un hub est un pipeline de haut niveau qui permet à un client et au serveur d'appeler des méthodes entre eux. SignalR gère la distribution au-delà des limites de machine automatiquement, autorisant les clients à appeler des méthodes sur le serveur et vice versa. Vous pouvez passer des paramètres fortement typés aux méthodes, ce qui permet la liaison de modèle. SignalR fournit deux protocoles hub intégrés : un protocole de texte basé sur JSON et un protocole binaire basé sur [MessagePack](https://msgpack.org/).  MessagePack crée généralement des messages plus petits par rapport au format JSON. Les navigateurs plus anciens doivent prendre en charge [niveau XHR 2](https://caniuse.com/#feat=xhr2) pour prendre en charge le protocole MessagePack.

Les hubs appellent le code côté client en envoyant des messages qui contiennent le nom et les paramètres de la méthode côté client. Les objets envoyés comme paramètres de méthode sont désérialisés en utilisant le protocole configuré. Le client essaie de faire correspondre le nom à une méthode dans le code côté client. Lorsque le client trouve une correspondance, il appelle la méthode et lui passe les données de paramètre désérialisées.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Bien démarrer avec SignalR pour ASP.NET Core](xref:tutorials/signalr)
* [Plateformes prises en charge](xref:signalr/supported-platforms)
* [Hubs](xref:signalr/hubs)
* [Client JavaScript](xref:signalr/javascript-client)
