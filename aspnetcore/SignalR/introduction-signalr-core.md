---
title: "Introduction à SignalR sur ASP.NET Core"
author: rachelappel
description: "Découvrez comment la bibliothèque ASP.NET Core SignalR simplifie l’ajout de fonctionnalités web en temps réel aux applications."
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/07/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/introduction-signalr-core
ms.openlocfilehash: 3fa70c957b246787d4e457c74f90ad797b3af766
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="introduction-to-signalr"></a>Introduction à SignalR

Par [Rachel Appel](https://twitter.com/rachelappel)

## <a name="what-is-signalr"></a>Nouveautés SignalR

ASP.NET Core SignalR est une bibliothèque qui simplifie l’ajout de fonctionnalités web en temps réel aux applications. Les fonctionnalités web en temps réel permet au code de côté serveur pour le contenu de type push aux clients instantanément.

Candidats pour SignalR :

* Applications nécessitant une haute fréquence des mises à jour à partir du serveur. Les jeux, réseaux sociaux, vote, offre aux enchères, mappages et GPS applications sont des exemples.
* Tableaux de bord et des applications de surveillance. Exemples incluent les tableaux de bord, les mises à jour client instantanées, ou les alertes de voyage.
* Applications de collaboration. Les applications de tableau blanc et logiciel de réunion d’équipe sont des exemples d’applications de collaboration.
* Applications qui nécessitent des notifications. Réseaux sociaux, messagerie, conversation, jeux, alertes de déplacement et de nombreuses autres applications utilisent des notifications.

SignalR fournit une API pour la création du serveur-client [des appels de procédure distante (RPC)](https://wikipedia.org/wiki/Remote_procedure_call). Les appels RPC appellent les fonctions JavaScript sur les clients à partir du code côté serveur .NET Core.

SignalR pour ASP.NET Core :

* Gestion des connexions, gère automatiquement.
* Permet la diffusion des messages à tous les clients connectés simultanément. Par exemple, une salle de conversation.
* Permet d’envoyer des messages à des clients spécifiques ou des groupes de clients.
* Est open source à [GitHub](https://github.com/aspnet/signalr).
* Évolutive.

La connexion entre le client et le serveur est persistante, contrairement à une connexion HTTP.

## <a name="transports"></a>Transports

Résumés SignalR sur un certain nombre de techniques pour créer des applications web en temps réel. [WebSockets](https://tools.ietf.org/html/rfc7118) est le transport optimal, mais d’autres techniques telles que les événements Server-Sent et interrogation longue peuvent être utilisés lors de celles qui ne sont pas disponibles. SignalR détectera automatiquement et initialiser le transport approprié en fonction des fonctionnalités prises en charge sur le serveur et le client.

## <a name="hubs-and-endpoints"></a>Les points de terminaison et les concentrateurs

SignalR utilise des concentrateurs et des points de terminaison pour la communication entre les clients et serveurs. L’API de concentrateurs couvre la plupart des scénarios.

Un concentrateur est un pipeline de haut niveau basé sur l’API de point de terminaison qui permet à votre client et le serveur appeler des méthodes sur eux. SignalR gère la répartition au-delà des limites de machine automatiquement, autorisant les clients à appeler des méthodes sur le serveur en tant que facilement en tant que méthodes locales et vice versa. Concentrateurs permettent de passer fortement typée de paramètres à des méthodes, ce qui permet la liaison de modèle. SignalR fournit deux protocoles de concentrateur intégré : un protocole de texte en fonction de JSON et un protocole binaire basé sur [MessagePack](https://msgpack.org/).  MessagePack crée généralement des messages plus petits que lors de l’utilisation de JSON. Les navigateurs plus anciens doivent prendre en charge [au niveau du XHR 2](https://caniuse.com/#feat=xhr2) pour prendre en charge le protocole MessagePack.

Concentrateurs appellent le code côté client en envoyant des messages en utilisant le transport actif. Les messages contenant le nom et les paramètres de la méthode côté client. Objets envoyés comme paramètres de méthode sont désérialisées en utilisant le protocole configuré. Le client essaie de correspondre au nom à une méthode dans le code côté client. En cas de correspondance, la méthode du client s’exécute en utilisant les données de paramètre désérialisé.

Points de terminaison fournissent une API de type socket brute, ce qui leur permet de lire et écrire à partir du client. Il incombe au développeur de gérer le regroupement, diffusion et autres fonctions. L’API de concentrateurs repose sur la couche de points de terminaison.

Le diagramme suivant montre la relation entre les concentrateurs, les points de terminaison et les clients.

![Mappage de SignalR](introduction-signalr-core/_static/signalr-core-architecture.png)

## <a name="related-resources"></a>Ressources connexes

[Prise en main SignalR pour ASP.NET Core](xref:signalr/get-started-signalr-core)
