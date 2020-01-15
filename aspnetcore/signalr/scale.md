---
title: ASP.NET Core SignalR l’hébergement et la mise à l’échelle de la production
author: bradygaster
description: Découvrez comment éviter les problèmes de performances et de mise à l’échelle dans les applications qui utilisent ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/28/2018
no-loc:
- SignalR
uid: signalr/scale
ms.openlocfilehash: 8e7b7596fcfe2d6b7150fe1ab09a7ab1dc4a2e47
ms.sourcegitcommit: 2388c2a7334ce66b6be3ffbab06dd7923df18f60
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75952122"
---
# <a name="aspnet-core-opno-locsignalr-hosting-and-scaling"></a>ASP.NET Core SignalR l’hébergement et la mise à l’échelle

Par [Andrew Stanton-infirmière](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster)et [Tom Dykstra](https://github.com/tdykstra),

Cet article explique les considérations relatives à l’hébergement et à la mise à l’échelle pour les applications à fort trafic qui utilisent ASP.NET Core SignalR.

## <a name="sticky-sessions"></a>Sessions rémanentes

SignalR requiert que toutes les requêtes HTTP pour une connexion spécifique soient gérées par le même processus serveur. Lorsque SignalR s’exécute sur une batterie de serveurs (plusieurs serveurs), vous devez utiliser des « sessions rémanentes ». Les « sessions rémanentes » sont également appelées « affinité de session » par certains équilibrages de charge. Azure App Service utilise [application Request Routing](https://docs.microsoft.com/iis/extensions/planning-for-arr/application-request-routing-version-2-overview) (arr) pour acheminer les demandes. L’activation du paramètre « affinité ARR » dans votre Azure App Service permet d’activer les « sessions rémanentes ». Les sessions rémanentes ne sont pas nécessaires dans les cas suivants :

1. En cas d’hébergement sur un seul serveur, dans un seul processus.
1. Lors de l’utilisation du service Azure SignalR.
1. Lorsque tous les clients sont configurés pour utiliser **uniquement** les WebSockets, **et** que le [paramètre SkipNegotiation](xref:signalr/configuration#configure-additional-options) est activé dans la configuration du client.

Dans toutes les autres circonstances (y compris lorsque le fond de panier ReDim est utilisé), l’environnement du serveur doit être configuré pour les sessions rémanentes.

Pour obtenir des conseils sur la configuration de Azure App Service pour SignalR, consultez <xref:signalr/publish-to-azure-web-app>.

## <a name="tcp-connection-resources"></a>Ressources de connexion TCP

Le nombre de connexions TCP simultanées qu’un serveur Web peut prendre en charge est limité. Les clients HTTP standard utilisent des connexions *éphémères* . Ces connexions peuvent être fermées lorsque le client devient inactif et rouvert ultérieurement. En revanche, une connexion SignalR est *persistante*. les connexions SignalR restent ouvertes même lorsque le client devient inactif. Dans une application à trafic élevé qui dessert de nombreux clients, ces connexions persistantes peuvent entraîner l’atteinte du nombre maximal de connexions des serveurs.

Les connexions persistantes consomment également de la mémoire supplémentaire pour effectuer le suivi de chaque connexion.

L’utilisation intensive des ressources liées à la connexion par SignalR peut affecter les autres applications Web qui sont hébergées sur le même serveur. Lorsque SignalR s’ouvre et contient les dernières connexions TCP disponibles, les autres applications Web sur le même serveur n’ont pas non plus de connexions disponibles.

Si un serveur est à court de connexions, vous verrez des erreurs de socket aléatoires et des erreurs de réinitialisation de la connexion. Par exemple :

```
An attempt was made to access a socket in a way forbidden by its access permissions...
```

Pour empêcher SignalR utilisation des ressources de provoquer des erreurs dans d’autres applications Web, exécutez SignalR sur des serveurs différents de ceux des autres applications Web.

Pour empêcher SignalR utilisation des ressources de provoquer des erreurs dans une application SignalR, augmentez la taille des instances pour limiter le nombre de connexions qu’un serveur doit gérer.

## <a name="scale-out"></a>Monter en charge

Une application qui utilise SignalR doit effectuer le suivi de toutes ses connexions, ce qui crée des problèmes pour une batterie de serveurs. Ajoutez un serveur et il obtient les nouvelles connexions que les autres serveurs ne connaissent pas. Par exemple, SignalR sur chaque serveur dans le diagramme suivant n’a pas connaissance des connexions sur les autres serveurs. Lorsque SignalR sur l’un des serveurs souhaite envoyer un message à tous les clients, le message est envoyé uniquement aux clients connectés à ce serveur.

![Mise à l’échelle [ ! Opérationnel. NO-LOC (Signalr)] sans fond de panier](scale/_static/scale-no-backplane.png)

Les options permettant de résoudre ce problème sont le [service Azure SignalR](#azure-signalr-service) et le [fond de panier ReDim](#redis-backplane).

## <a name="azure-opno-locsignalr-service"></a>Service SignalR Azure

Le service de SignalR Azure est un proxy plutôt qu’un fond de panier. Chaque fois qu’un client initie une connexion au serveur, le client est redirigé pour se connecter au service. Ce processus est illustré dans le diagramme suivant :

![Établissement d’une connexion à Azure [ ! Opérationnel. Service NO-LOC (Signalr)]](scale/_static/azure-signalr-service-one-connection.png)

Le résultat est que le service gère toutes les connexions clientes, alors que chaque serveur n’a besoin que d’un petit nombre constant de connexions au service, comme indiqué dans le diagramme suivant :

![Clients connectés au service, serveurs connectés au service](scale/_static/azure-signalr-service-multiple-connections.png)

Cette approche de la montée en puissance parallèle présente plusieurs avantages par rapport au fond de panier ReDim :

* Les sessions rémanentes, également appelées « [affinité du client](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity)», ne sont pas nécessaires, car les clients sont immédiatement redirigés vers le Service Azure SignalR lorsqu’ils se connectent.
* Une application SignalR peut monter en charge en fonction du nombre de messages envoyés, tandis que le service Azure SignalR est automatiquement mis à l’échelle pour gérer un nombre quelconque de connexions. Par exemple, il peut y avoir des milliers de clients, mais si seuls quelques messages par seconde sont envoyés, l’application SignalR n’a pas besoin d’effectuer une montée en charge sur plusieurs serveurs simplement pour gérer les connexions elles-mêmes.
* Une application SignalR n’utilise pas beaucoup plus de ressources de connexion qu’une application Web sans SignalR.

Pour ces raisons, nous recommandons le service Azure SignalR pour toutes les applications ASP.NET Core SignalR hébergées sur Azure, y compris les App Service, les machines virtuelles et les conteneurs.

Pour plus d’informations, consultez la [documentation du service Azure SignalR](/azure/azure-signalr/signalr-overview).

## <a name="redis-backplane"></a>Fond de panier Redis

[Redims](https://redis.io/) est un magasin de valeurs de clé en mémoire qui prend en charge un système de messagerie avec un modèle de publication/abonnement. Le SignalR le fond de panier ReDim utilise la fonctionnalité Pub/Sub pour transférer des messages vers d’autres serveurs. Lorsqu’un client établit une connexion, les informations de connexion sont transmises au fond de panier. Lorsqu’un serveur souhaite envoyer un message à tous les clients, il envoie au fond de panier. Le fond de panier connaît tous les clients connectés et les serveurs sur lesquels ils se trouvent. Elle envoie le message à tous les clients par le biais de leurs serveurs respectifs. Ce processus est illustré dans le diagramme suivant :

![Le fond de panier ReDim, message envoyé d’un serveur à tous les clients](scale/_static/redis-backplane.png)

Le fond de panier ReDim est l’approche recommandée pour la montée en puissance parallèle pour les applications hébergées dans votre propre infrastructure. En cas de latence de connexion importante entre votre centre de données et un centre de données Azure, le service Azure SignalR peut ne pas être une option pratique pour les applications locales avec des exigences de faible latence ou de débit élevé.

Les avantages du service Azure SignalR indiqués plus haut sont les inconvénients du fond de panier ReDim :

* Des sessions rémanentes, également appelées « [affinité du client](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity)», sont nécessaires, sauf lorsque les **deux** conditions suivantes sont réunies :
  * Tous les clients sont configurés pour utiliser **uniquement** les WebSockets.
  * Le [paramètre SkipNegotiation](xref:signalr/configuration#configure-additional-options) est activé dans la configuration du client. 
   Une fois qu’une connexion est établie sur un serveur, la connexion doit rester sur ce serveur.
* Une application SignalR doit monter en charge en fonction du nombre de clients, même si peu de messages sont envoyés.
* Une application SignalR utilise beaucoup plus de ressources de connexion qu’une application Web sans SignalR.

## <a name="iis-limitations-on-windows-client-os"></a>Limitations IIS sur le système d’exploitation client Windows

Windows 10 et Windows 8. x sont des systèmes d’exploitation clients. IIS sur les systèmes d’exploitation clients est limité à 10 connexions simultanées. les connexions de SignalRsont les suivantes :

* Transitoire et fréquemment rétabli.
* **N’est pas** supprimé immédiatement lorsqu’il n’est plus utilisé.

Les conditions précédentes permettent d’atteindre la limite de 10 connexions sur un système d’exploitation client. Lorsqu’un système d’exploitation client est utilisé pour le développement, nous vous recommandons les opérations suivantes :

* Évitez les services Internet.
* Utilisez Kestrel ou IIS Express comme cibles de déploiement.

## <a name="next-steps"></a>Étapes suivantes :

Pour plus d'informations, voir les ressources suivantes :

* [Documentation Azure SignalR service](/azure/azure-signalr/signalr-overview)
* [Configurer un backplane ReDim](xref:signalr/redis-backplane)
