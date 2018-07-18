---
title: Introduction à ASP.NET Core SignalR
author: tdykstra
description: Découvrez comment la bibliothèque ASP.NET Core SignalR simplifie l’ajout de fonctionnalités en temps réel aux applications.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: bc6f25c3f35e7fb0c2c68220697f2e0fdc6a9958
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095387"
---
# <a name="introduction-to-aspnet-core-signalr"></a>Introduction à ASP.NET Core SignalR

Par [Rachel Appel](https://twitter.com/rachelappel)

## <a name="what-is-signalr"></a>Nouveautés SignalR

ASP.NET Core SignalR est une bibliothèque qui simplifie l’ajout de fonctionnalités web en temps réel aux applications. Les fonctionnalités web en temps réel permet au code côté serveur pour transmettre le contenu aux clients instantanément.

Bons candidats pour SignalR :

* Applications qui nécessitent des mises à jour de la fréquence élevée à partir du serveur. Exemples sont des jeux, réseaux sociaux, de vote, vente aux enchères, mappages et les applications GPS.
* Tableaux de bord et des applications de surveillance. Exemples incluent des tableaux de bord, les ventes mises à jour instantanées, ou d’alertes voyagent impossible.
* Applications de collaboration. Les applications de tableau blanc et les logiciels de réunion d’équipe sont des exemples d’applications de collaboration.
* Applications qui nécessitent des notifications. Réseaux sociaux, e-mail, chat, jeux, des alertes de voyage et de nombreuses autres applications utilisent des notifications.

SignalR fournit une API pour la création de client-serveur [des appels de procédure distante (RPC)](https://wikipedia.org/wiki/Remote_procedure_call). Les appels RPC appellent des fonctions JavaScript sur les clients à partir du code côté serveur .NET Core.

SignalR pour ASP.NET Core :

* Gestion des connexions, gère automatiquement.
* Permet la diffusion des messages à tous les clients connectés simultanément. Par exemple, une salle de conversation.
* Permet d’envoyer des messages à des clients spécifiques ou des groupes de clients.
* Est open source à [GitHub](https://github.com/aspnet/signalr).
* Évolutif.

La connexion entre le client et le serveur est persistante, contrairement à une connexion HTTP.

## <a name="transports"></a>Transports

Résumés de SignalR sur un certain nombre de techniques pour créer des applications web en temps réel. [WebSockets](https://tools.ietf.org/html/rfc7118) est le transport optimal, mais d’autres techniques telles que les événements et d’interrogation longue peuvent être utilisées lorsque celles qui ne sont pas disponibles. SignalR détectera automatiquement et initialiser le transport approprié selon les fonctionnalités prises en charge sur le serveur et le client.

## <a name="hubs"></a>Hubs

SignalR utilise des hubs pour communiquer entre les clients et serveurs.

Un concentrateur est un pipeline de haut niveau qui permet à votre client et le serveur pour appeler des méthodes sur eux. SignalR gère la distribution au-delà des limites de machine automatiquement, autorisant les clients à appeler des méthodes sur le serveur en tant que facilement en tant que méthodes locales et vice versa. Hubs permettent de transmettre des paramètres fortement typés aux méthodes, ce qui permet la liaison de modèle. SignalR fournit deux protocoles hub intégrés : un protocole de texte basé sur JSON et un protocole binaire basé sur [MessagePack](https://msgpack.org/).  MessagePack crée généralement des messages plus petits que lors de l’utilisation de JSON. Les navigateurs plus anciens doivent prendre en charge [niveau XHR 2](https://caniuse.com/#feat=xhr2) pour prendre en charge le protocole MessagePack.

Hubs appellent le code côté client en envoyant des messages en utilisant le transport actif. Les messages contiennent le nom et les paramètres de la méthode côté client. Objets envoyés comme paramètres de méthode sont désérialisés en utilisant le protocole configuré. Le client essaie de correspondre au nom à une méthode dans le code côté client. En cas de correspondance, la méthode du client s’exécute en utilisant les données de paramètre désérialisées.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Bien démarrer avec SignalR pour ASP.NET Core](xref:tutorials/signalr)
* [Plateformes prises en charge](xref:signalr/supported-platforms)
* [Hubs](xref:signalr/hubs)
* [Client JavaScript](xref:signalr/javascript-client)
