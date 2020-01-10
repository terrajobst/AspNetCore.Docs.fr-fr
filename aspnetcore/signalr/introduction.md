---
title: Présentation de ASP.NET Core SignalR
author: bradygaster
description: Découvrez comment la bibliothèque de SignalR ASP.NET Core simplifie l’ajout de fonctionnalités en temps réel aux applications.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/27/2019
no-loc:
- SignalR
uid: signalr/introduction
ms.openlocfilehash: 635431abf9263c2dff261aea47e6f8324061763f
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75829281"
---
# <a name="introduction-to-aspnet-core-opno-locsignalr"></a>Présentation de ASP.NET Core SignalR

## <a name="what-is-opno-locsignalr"></a>Qu’est-ce qu’SignalR ?

ASP.NET Core SignalR est une bibliothèque open source qui simplifie l’ajout de fonctionnalités Web en temps réel aux applications. Les fonctionnalités web en temps réel permettent au code côté serveur de transmettre le contenu aux clients instantanément.

Bons candidats pour SignalR:

* Des applications qui nécessitent des mises à jour à haute fréquence à partir du serveur. Des exemples sont des applications de jeux, de réseaux sociaux, de vote, de vente aux enchères, de cartographie et de géolocalisation.
* Des tableaux de bord et des applications de surveillance. Des exemples incluent des tableaux de bord, des mises à jour de ventes instantanées, ou des alertes de voyage.
* Des applications de collaboration. Des applications de tableau blanc et des logiciels de réunion d’équipe sont des exemples d’applications de collaboration.
* Des applications qui nécessitent des notifications. Les applications de réseaux sociaux, e-mail, chat, jeux, d'alertes de voyage et de nombreuses autres applications utilisent des notifications.

SignalR fournit une API pour la création d' [appels de procédure distante (RPC)](https://wikipedia.org/wiki/Remote_procedure_call)de serveur à client. Les appels RPC appellent des fonctions JavaScript sur les clients à partir de code côté serveur en .NET Core.

Voici certaines fonctionnalités de SignalR pour ASP.NET Core :

* Gestion automatique des connexions.
* Envoi de messages à tous les clients connectés simultanément. Par exemple, une salle de conversation.
* Envoi de messages à des clients spécifiques ou des groupes de clients.
* Gestion de la montée en charge lors de l'augmentation de trafic.

La source est hébergée dans un [référentielSignalR sur GitHub](https://github.com/dotnet/AspNetCore/tree/master/src/SignalR).

## <a name="transports"></a>Transports

SignalR prend en charge les techniques suivantes pour gérer les communications en temps réel (par ordre de secours normal) :

* [WebSockets](https://tools.ietf.org/html/rfc7118)
* Événements envoyés par le serveur
* Interrogation longue (Long polling)

SignalR choisit automatiquement la meilleure méthode de transport parmi les capacités du serveur et du client.

## <a name="hubs"></a>Hubs

SignalR utilise des *hubs* pour la communication entre les clients et les serveurs.

Un hub est un pipeline de haut niveau qui permet à un client et au serveur d'appeler des méthodes entre eux. SignalR gère automatiquement la distribution à travers les limites de l’ordinateur, ce qui permet aux clients d’appeler des méthodes sur le serveur et vice versa. Vous pouvez passer des paramètres fortement typés à des méthodes, ce qui active la liaison de modèle. SignalR fournit deux protocoles Hub intégrés : un protocole texte basé sur JSON et un protocole binaire basé sur [MessagePack](https://msgpack.org/).  MessagePack crée généralement des messages plus petits par rapport à JSON. Les anciens navigateurs doivent prendre en charge le [niveau 2 de XHR](https://caniuse.com/#feat=xhr2) pour fournir la prise en charge du protocole MessagePack.

Les hubs appellent le code côté client en envoyant des messages qui contiennent le nom et les paramètres de la méthode côté client. Les objets envoyés comme paramètres de méthode sont désérialisés en utilisant le protocole configuré. Le client essaie de faire correspondre le nom à une méthode dans le code côté client. Lorsque le client trouve une correspondance, il appelle la méthode et lui passe les données de paramètre désérialisées.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Prise en main de SignalR pour ASP.NET Core](xref:tutorials/signalr)
* [Plateformes prises en charge](xref:signalr/supported-platforms)
* [Hubs](xref:signalr/hubs)
* [Client JavaScript](xref:signalr/javascript-client)
