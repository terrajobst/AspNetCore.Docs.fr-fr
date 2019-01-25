---
title: Hébergement d’ASP.NET Core SignalR production et de mise à l’échelle
author: bradygaster
description: Découvrez comment éviter les performances et la mise à l’échelle des problèmes dans les applications qui utilisent ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/scale
ms.openlocfilehash: 4ac4509acc89d0091a3757c7cfbc9981614f29ad
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836920"
---
# <a name="aspnet-core-signalr-hosting-and-scaling"></a>Hébergement de ASP.NET Core SignalR et de mise à l’échelle

Par [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), et [Nowak](https://github.com/tdykstra),

Cet article explique l’hébergement et de mise à l’échelle des considérations pour les applications à trafic élevé qui utilisent ASP.NET Core SignalR.

## <a name="tcp-connection-resources"></a>Ressources de connexion TCP

Le nombre de connexions TCP simultanées prenant en charge un serveur web est limité. Les clients HTTP standards utilisent *éphémère* connexions. Ces connexions peuvent être fermées lorsque le client devient inactif et rouverts ultérieurement. En revanche, une connexion SignalR est *persistant*. Les connexions SignalR restent ouverte, même lorsque le client est inactif. Dans une application à trafic élevé qui sert de nombreux clients, ces connexions persistantes peuvent entraîner des serveurs atteint son nombre maximal de connexions.

Connexions persistantes également consomment mémoire supplémentaire, pour effectuer le suivi de chaque connexion.

Une utilisation intensive des ressources liées à la connexion par SignalR peut affecter d’autres applications web qui sont hébergées sur le même serveur. Lorsque SignalR s’ouvre et le maintient les connexions TCP disponibles dernière, autres applications web sur le même serveur n’ont également aucun davantage de connexions à leur disposition.

Si un serveur manque de connexions, vous verrez les erreurs de socket aléatoire et erreurs de réinitialisation de connexion. Exemple :

```
An attempt was made to access a socket in a way forbidden by its access permissions...
```

Pour éviter l’utilisation des ressources SignalR et provoquent des erreurs dans d’autres applications web, exécutez SignalR sur différents serveurs à vos autres applications web.

Pour éviter l’utilisation des ressources SignalR et provoquent des erreurs dans une application de SignalR, monter en charge pour limiter le nombre de connexions à qu'un serveur doit gérer.

## <a name="scale-out"></a>Monter en charge

Une application qui utilise SignalR doit effectuer le suivi de ses connexions, ce qui crée des problèmes pour une batterie de serveurs. Ajouter un serveur, et il obtient de nouvelles connexions qui ne savent pas sur les autres serveurs. Par exemple, les SignalR sur chaque serveur dans le diagramme suivant n’a pas connaissance des connexions sur les autres serveurs. Lorsqu’il souhaite SignalR sur l’un des serveurs envoyer un message à tous les clients, le message passe uniquement pour les clients connectés à ce serveur.

![Mise à l’échelle de SignalR sans fond de panier](scale/_static/scale-no-backplane.png)

Les options de résolution de ce problème sont les [Service Azure SignalR](#azure-signalr-service) et [Redis fond de panier](#redis-backplane).

## <a name="azure-signalr-service"></a>Service Azure SignalR

Le Service Azure SignalR est un proxy plutôt que sur un fond de panier. Chaque fois qu’un client établit une connexion au serveur, le client est redirigé pour vous connecter au service. Ce processus est illustré dans le diagramme suivant :

![L’établissement d’une connexion au Service Azure SignalR](scale/_static/azure-signalr-service-one-connection.png)

Le résultat est que le service gère toutes les connexions client, tandis que chaque serveur doit uniquement un petit nombre constant de connexions au service, comme illustré dans le diagramme suivant :

![Clients connectés au service, les serveurs connectés au service](scale/_static/azure-signalr-service-multiple-connections.png)

Cette approche de montée en puissance présente plusieurs avantages par rapport à l’alternative de fond de panier de Redis :

* Sessions rémanentes, également appelées [affinité du client](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity), n’est pas nécessaire, car les clients sont immédiatement redirigés vers le Service Azure SignalR lorsqu’ils se connectent.
* Une application peut monter en charge de SignalR en fonction du nombre de messages envoyés, tandis que le Service Azure SignalR évolue automatiquement pour gérer n’importe quel nombre de connexions. Par exemple, il peut y avoir des milliers de clients, mais si seules quelques messages par seconde sont envoyées, l’application SignalR ne devra pas évoluer vers plusieurs serveurs pour gérer les connexions elles-mêmes.
* Une application de SignalR n’utiliser beaucoup plus de ressources de connexion à une application web sans SignalR.

Pour ces raisons, nous vous recommandons le Service Azure SignalR pour toutes les applications ASP.NET Core SignalR hébergées sur Azure, y compris App Service, les machines virtuelles et les conteneurs.

Pour plus d’informations, consultez le [documentation de Service Azure SignalR](/azure/azure-signalr/signalr-overview).

## <a name="redis-backplane"></a>Fond de panier de redis

[Redis](https://redis.io/) est un magasin clé-valeur en mémoire qui prend en charge un système de messagerie avec un modèle de publication/abonnement. Le fond de panier SignalR Redis utilise la fonctionnalité de pub/sub pour transférer des messages vers d’autres serveurs. Lorsqu’un client établit une connexion, les informations de connexion sont passées au fond de panier. Lorsqu’un serveur souhaite envoyer un message à tous les clients, il envoie au fond de panier. Le fond de panier connaît tous les clients connectés et les serveurs, ils sont sur. Il envoie le message à tous les clients par le biais de leurs serveurs respectifs. Ce processus est illustré dans le diagramme suivant :

![Fond de panier, message envoyé à partir d’un serveur à tous les clients de redis](scale/_static/redis-backplane.png)

Le fond de panier de Redis est l’approche de montée en puissance recommandée pour les applications hébergées sur votre propre infrastructure. Service Azure SignalR n’est pas une option pratique pour la production avec les applications locales en raison de la latence de la connexion entre votre centre de données et un centre de données Azure.

Les avantages du Service Azure SignalR indiqués précédemment sont inconvénients pour le fond de panier de Redis :

* Sessions rémanentes, également appelées [affinité du client](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity), est requis. Une fois qu’une connexion est initiée sur un serveur, la connexion doit rester sur ce serveur.
* Une application de SignalR doit évoluer en fonction du nombre de clients même si certains messages sont envoyés.
* Une application de SignalR utilise beaucoup plus de ressources de connexion à une application web sans SignalR.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d'informations, reportez-vous aux ressources suivantes :

* [Documentation SignalR Service Azure](/azure/azure-signalr/signalr-overview)
* [Configurer un fond de panier de Redis](xref:signalr/redis-backplane)
