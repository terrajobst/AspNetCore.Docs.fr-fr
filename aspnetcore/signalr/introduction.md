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
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662917"
---
# <a name="introduction-to-aspnet-core-opno-locsignalr"></a>Présentation de ASP.NET Core SignalR

## <a name="what-is-opno-locsignalr"></a>Qu’est-ce que SignalR?

ASP.NET Core SignalR est une bibliothèque open source qui simplifie l’ajout de fonctionnalités Web en temps réel aux applications. Les fonctionnalités web en temps réel permettent au code côté serveur de transmettre le contenu aux clients instantanément.

Bons candidats pour SignalR:

* Les applications ayant besoin de mises à jour fréquentes auprès du serveur. Exemples : jeux, réseaux sociaux, scrutin, enchères, cartes et applications GPS.
* Les tableaux de bord et les applications de monitoring. Exemples : tableaux de bord des entreprises, mises à jour instantanées des ventes et alertes de voyage.
* Les applications de collaboration. Exemples : applications de tableau blanc et logiciels de réunion d’équipe.
* Les applications qui envoient des notifications. Exemples : réseaux sociaux, messagerie, conversation instantanée, jeux, alertes de voyage, etc.

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
