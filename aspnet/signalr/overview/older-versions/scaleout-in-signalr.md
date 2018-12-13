---
uid: signalr/overview/older-versions/scaleout-in-signalr
title: Introduction à la montée en puissance parallèle dans SignalR 1.x | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 04/29/2013
ms.assetid: 3fd9f11c-799b-4001-bd60-1e70cfc61c19
msc.legacyurl: /signalr/overview/older-versions/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: fffa424ea4b62a54b9df48aaa409541ab5d1608f
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287586"
---
<a name="introduction-to-scaleout-in-signalr-1x"></a>Introduction à la montée en puissance parallèle dans SignalR 1.x
====================
par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

En règle générale, il existe deux façons de mettre à l’échelle une application web : *monter en puissance* et *monter en charge*.

- Monter en puissance implique l’utilisation d’un plus grand serveur (ou une plus grande machine virtuelle) avec plus de RAM, UC, etc.
- Monter en charge signifie ajouter davantage de serveurs pour gérer la charge.

Le problème de montée en puissance est vite appuyé sur une limite de la taille de l’ordinateur. En outre, vous devez monter en charge. Toutefois, lorsque vous faites évoluer, les clients peuvent obtenir acheminés vers différents serveurs. Un client qui est connecté à un seul serveur ne recevra pas les messages envoyés à partir d’un autre serveur.

![](scaleout-in-signalr/_static/image1.png)

Une solution consiste à transférer les messages entre les serveurs, à l’aide d’un composant appelé un *fond de panier*. Avec un fond de panier est activée, chaque instance d’application envoie des messages au fond de panier, et le fond de panier les transfère vers les autres instances de l’application. (En électronique, un fond de panier est un groupe de connecteurs parallèles. Par analogie, un fond de panier SignalR connecte plusieurs serveurs.)

![](scaleout-in-signalr/_static/image2.png)

SignalR fournit actuellement trois fonds de panier :

- **Azure Service Bus**. Service Bus est une infrastructure de messagerie qui permet aux composants envoyer des messages d’une manière faiblement couplée.
- **Redis**. Redis est un magasin de clé-valeur en mémoire. Redis prend en charge un modèle de publication/abonnement (« pub/sub ») pour envoyer des messages.
- **SQL Server**. Le fond de panier de SQL Server écrit les messages dans les tables SQL. Le fond de panier utilise Service Broker de messagerie efficace. Toutefois, elle fonctionne également si Service Broker n’est pas activé.

Si vous déployez votre application sur Azure, envisagez d’utiliser l’infrastructure d’intégration Azure Service Bus. Si vous déployez sur votre propre batterie de serveurs, envisagez de SQL Server ou des fonds de panier de Redis.

Les rubriques suivantes contiennent des didacticiels pas à pas pour chaque fond de panier :

- [Montée en puissance parallèle de SignalR avec Azure Service Bus](scaleout-with-windows-azure-service-bus.md)
- [Montée en puissance parallèle de SignalR avec Redis](scaleout-with-redis.md)
- [Montée en puissance parallèle de SignalR avec SQL Server](scaleout-with-sql-server.md)

## <a name="implementation"></a>Implémentation

Dans SignalR, chaque message est envoyé via un bus de messages. Un bus de messages implémente le [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interface, qui fournit une abstraction de publication/abonnement. Les fonds de panier fonctionnent en remplaçant la valeur par défaut **IMessageBus** avec un bus conçu ce fond de panier. Par exemple, le bus de messages pour Redis est [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), et il utilise le cache Redis [pub/sub](http://redis.io/topics/pubsub) mécanisme pour envoyer et recevoir des messages.

Chaque instance de serveur se connecte à l’infrastructure d’intégration via le bus. Lorsqu’un message est envoyé, il est placé dans le fond de panier, et le fond de panier envoie à chaque serveur. Lorsqu’un serveur reçoit un message du fond de panier, il place le message dans sa mémoire cache locale. Le serveur remet ensuite les messages aux clients à partir de sa mémoire cache locale.

Pour chaque connexion client, progression du client dans la lecture du flux de message est suivie à l’aide d’un curseur. (Un curseur représente une position dans le flux de message.) Si un client se déconnecte, puis se reconnecte, il demande le bus de messages reçus après la valeur du curseur du client. La même chose se produit lorsqu’une connexion utilise [interrogation longue](../getting-started/introduction-to-signalr.md#transports). Après l’achèvement d’une requête d’interrogation longue, le client ouvre une nouvelle connexion et vous demande des messages reçus après le curseur.

Le fonctionnement du mécanisme curseur même si un client est acheminé vers un autre serveur sur se reconnecter. Le fond de panier tient compte de tous les serveurs, et peu importe le serveur auquel le client se connecte.

## <a name="limitations"></a>Limitations

À l’aide d’un fond de panier, le débit de message maximale est inférieur à ce que c’est lorsque les clients communiquent directement avec un seul nœud serveur. C’est parce que le fond de panier transfère tous les messages à tous les nœuds, donc le fond de panier peut devenir un goulot d’étranglement. Si cette limitation est un problème dépend de l’application. Par exemple, voici quelques scénarios classiques de SignalR :

- [Diffusion de serveur](tutorial-server-broadcast-with-aspnet-signalr.md) (par exemple, les cotations boursières) : Fonds de panier fonctionnent bien pour ce scénario, étant donné que le serveur contrôle la fréquence à laquelle les messages sont envoyés.
- [Client à](tutorial-getting-started-with-signalr.md) (par exemple, chat) : Dans ce scénario, le fond de panier peut être un goulot d’étranglement si le nombre de messages évolue avec le nombre de clients ; Autrement dit, si le taux de messages augmente proportionnellement à sa plus de clients joindre.
- [En temps réel haute fréquence](tutorial-high-frequency-realtime-with-signalr.md) (par exemple, des jeux en temps réel) : Fond de panier n’est pas recommandée pour ce scénario.

## <a name="enabling-tracing-for-signalr-scaleout"></a>Activation du suivi pour la montée en puissance parallèle de SignalR

Pour activer le suivi pour les fonds de panier, ajoutez les sections suivantes au fichier web.config, sous la racine **configuration** élément :

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
