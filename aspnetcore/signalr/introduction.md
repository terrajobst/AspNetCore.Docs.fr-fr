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

ASP.NET Core SignalR est une bibliothèque open source qui simplifie l’ajout de fonctionnalités web en temps réel aux applications. Les fonctionnalités web en temps réel permet au code côté serveur pour transmettre le contenu aux clients instantanément.

Bons candidats pour SignalR :

* Applications qui nécessitent des mises à jour de la fréquence élevée à partir du serveur. Exemples sont des jeux, réseaux sociaux, de vote, vente aux enchères, mappages et les applications GPS.
* Tableaux de bord et des applications de surveillance. Exemples incluent des tableaux de bord, les ventes mises à jour instantanées, ou d’alertes voyagent impossible.
* Applications de collaboration. Les applications de tableau blanc et les logiciels de réunion d’équipe sont des exemples d’applications de collaboration.
* Applications qui nécessitent des notifications. Réseaux sociaux, e-mail, chat, jeux, des alertes de voyage et de nombreuses autres applications utilisent des notifications.

SignalR fournit une API pour la création de client-serveur [des appels de procédure distante (RPC)](https://wikipedia.org/wiki/Remote_procedure_call). Les appels RPC appellent des fonctions JavaScript sur les clients à partir du code côté serveur .NET Core.

Voici certaines fonctionnalités de SignalR pour ASP.NET Core :

* Gestion des connexions, gère automatiquement.
* Envoie des messages à tous les clients connectés simultanément. Par exemple, une salle de conversation.
* Envoie des messages à des clients spécifiques ou des groupes de clients.
* Échelles à gérer un trafic croissant.

La source est hébergée dans un [dépôt SignalR sur GitHub](https://github.com/aspnet/signalr).

## <a name="transports"></a>Transports

SignalR prend en charge plusieurs techniques pour la gestion des communications en temps réel :

* [WebSockets](https://tools.ietf.org/html/rfc7118)
* Événements de serveur a été envoyé
* Interrogation longue

SignalR choisit automatiquement la meilleure méthode de transport qui se trouve dans les fonctionnalités du serveur et client.

## <a name="hubs"></a>Hubs

SignalR utilise *hubs* pour communiquer entre les clients et serveurs.

Un concentrateur est un pipeline de haut niveau qui permet à un client et le serveur pour appeler des méthodes sur eux. SignalR gère la distribution au-delà des limites de machine automatiquement, autorisant les clients à appeler des méthodes sur le serveur et vice versa. Vous pouvez passer des paramètres fortement typés aux méthodes, ce qui permet la liaison de modèle. SignalR fournit deux protocoles hub intégrés : un protocole de texte basé sur JSON et un protocole binaire basé sur [MessagePack](https://msgpack.org/).  MessagePack crée généralement des messages plus petits par rapport au format JSON. Les navigateurs plus anciens doivent prendre en charge [niveau XHR 2](https://caniuse.com/#feat=xhr2) pour prendre en charge le protocole MessagePack.

Hubs appellent le code côté client en envoyant des messages qui contiennent le nom et les paramètres de la méthode côté client. Objets envoyés comme paramètres de méthode sont désérialisés en utilisant le protocole configuré. Le client essaie de correspondre au nom à une méthode dans le code côté client. Lorsque le client trouve une correspondance, il appelle la méthode et lui passe les données de paramètre désérialisées.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Bien démarrer avec SignalR pour ASP.NET Core](xref:tutorials/signalr)
* [Plateformes prises en charge](xref:signalr/supported-platforms)
* [Hubs](xref:signalr/hubs)
* [Client JavaScript](xref:signalr/javascript-client)
