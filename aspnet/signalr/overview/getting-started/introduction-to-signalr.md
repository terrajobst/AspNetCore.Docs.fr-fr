---
uid: signalr/overview/getting-started/introduction-to-signalr
title: Introduction à SignalR | Microsoft Docs
author: pfletcher
description: Cet article décrit les nouveautés de SignalR et certaines solutions qu’il a été conçu pour créer.
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: d103573fb31bb3b08d054cbf65ff906bd5d151d3
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912798"
---
<a name="introduction-to-signalr"></a>Introduction à SignalR
====================

Une version mise à jour de ce didacticiel est disponible [ici](/aspnet/core/tutorials/signalr) à l’aide de la dernière version de Visual Studio. Utilise le nouveau didacticiel [ASP.NET Core](/aspnet/core/), qui offre de nombreuses améliorations au fil de ce didacticiel.

par [Patrick Fletcher](https://github.com/pfletcher)

> Cet article décrit les nouveautés de SignalR et certaines solutions qu’il a été conçu pour créer. 
> 
> ## <a name="questions-and-comments"></a>Questions et commentaires
> 
> Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer dans les commentaires en bas de la page. Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr).


## <a name="what-is-signalr"></a>Nouveautés SignalR

ASP.NET SignalR est une bibliothèque pour les développeurs ASP.NET qui simplifie le processus d’ajout de fonctionnalités web en temps réel aux applications. Les fonctionnalités web en temps réel sont la possibilité de push de code serveur contenu aux clients connectés instantanément dès que possible, plutôt que le serveur attende qu’un client demander de nouvelles données.

SignalR peut être utilisé pour ajouter une forme quelconque de fonctionnalités web « en temps réel » à votre application ASP.NET. Tandis que la conversation est souvent utilisée comme un exemple, vous pouvez effectuer beaucoup plus. Chaque fois qu’un utilisateur actualise une page web pour voir les nouvelles données ou la page implémente [interrogation longue](http://en.wikipedia.org/wiki/Push_technology#Long_polling) pour récupérer les nouvelles données, il constitue un candidat pour l’utilisation de SignalR. Exemples de tableaux de bord et la surveillance des applications, des applications de collaboration (par exemple, l’Édition simultanée de documents), travail mises à jour de progression et les formulaires en temps réel.

SignalR permet également de complètement nouveaux types d’applications web qui nécessitent des mises à jour de la fréquence élevée à partir du serveur, par exemple, les jeux en temps réel.

SignalR fournit une API simple pour créer des appels de procédure distante du serveur-client (RPC) qui appellent des fonctions JavaScript dans le client (et autres plateformes client) à partir du code .NET côté serveur. SignalR inclut également des API pour la gestion des connexions (par exemple, vous connecter et déconnecter les événements) et le regroupement de connexions.

![Appeler des méthodes avec SignalR](introduction-to-signalr/_static/image1.png)

SignalR gère automatiquement la gestion des connexions et vous permet de messages de diffusion à tous les clients connectés simultanément, comme une salle de conversation. Vous pouvez également envoyer des messages à des clients spécifiques. La connexion entre le client et le serveur est persistante, contrairement à une connexion HTTP classique, qui est rétablie pour chaque communication.

SignalR prend en charge la fonctionnalité « server push », dans lequel le code serveur peut faire appel au code client dans le navigateur en utilisant les appels de procédure distante (RPC), plutôt que le modèle de requête-réponse courantes sur le web dès aujourd'hui.

Les applications SignalR peuvent monter en charge à des milliers de clients à l’aide de Service Bus, SQL Server ou [Redis](http://redis.io).

SignalR est open source, accessibles via [GitHub](https://github.com/signalr).

## <a name="signalr-and-websocket"></a>SignalR et WebSocket

SignalR utilise le nouveau transport WebSocket lorsqu’elles sont disponibles et revient aux transports plus anciens lorsque cela est nécessaire. Bien que vous pourriez certainement rédiger votre application à l’aide de WebSocket directement, à l’aide de moyens de SignalR un grand nombre de fonctionnalités supplémentaires, que vous devez implémenter est déjà fait pour vous. Plus important encore, cela signifie que vous pouvez coder votre application pour tirer parti de WebSocket sans avoir à vous soucier de la création d’un chemin d’accès de code distinct pour les clients plus anciens. SignalR vous protège également d’avoir à vous soucier des mises à jour de WebSocket, étant donné que SignalR est mis à jour pour prendre en charge les modifications dans le transport sous-jacent, fournissant une interface cohérente entre les versions de WebSocket à votre application.

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>Transports et les solutions de secours

SignalR est une abstraction par rapport à des transports qui sont nécessaires pour effectuer un travail en temps réel entre client et serveur. Une connexion SignalR démarre en tant que HTTP et est ensuite promue pour une connexion WebSocket s’il est disponible. WebSocket est le transport idéal pour SignalR, dans la mesure où il rend l’utilisation la plus efficace de mémoire du serveur, a la plus faible latence et présente les caractéristiques plus sous-jacent (par exemple, la communication en duplex intégral entre client et serveur), mais il a également les plus strictes configuration requise : WebSocket demande au serveur d’être à l’aide de Windows Server 2012 ou Windows 8 et .NET Framework 4.5. Si ces conditions ne sont pas remplies, SignalR va tenter d’utiliser les autres transports pour ses connexions.

### <a name="html-5-transports"></a>Transports de HTML 5

Ces transports dépendent de la prise en charge de [HTML 5](http://en.wikipedia.org/wiki/HTML5). Si le navigateur client ne prend pas en charge la norme HTML 5, transports antérieures seront utilisés.

- **WebSocket** (si le le serveur et le navigateur indiquent qu’ils peuvent prendre en charge de Websocket). WebSocket est le seul transport qui établit une connexion permanente, bidirectionnelle true entre client et serveur. Toutefois, WebSocket a également les exigences les plus strictes ; Il est entièrement pris en charge uniquement dans les dernières versions de Microsoft Internet Explorer, Google Chrome et Mozilla Firefox et a uniquement une implémentation partielle dans d’autres navigateurs tels que Opera et Safari.
- **Événements de serveur envoyé**, également connu en tant que source d’événement (si le navigateur prend en charge serveur envoyé événements, qui est en fait tous les navigateurs à l’exception d’Internet Explorer.)

### <a name="comet-transports"></a>Transports Comet

Les transports suivants sont basés sur le [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) modèle d’application web, dans laquelle un navigateur ou un autre client maintient une demande HTTP maintenus à long, le serveur peut utiliser pour transmettre des données au client sans le client en particulier il demande.

- **Forever Frame** (pour Internet Explorer uniquement). Forever Frame crée un IFrame masqué qui effectue une demande à un point de terminaison sur le serveur qui ne se termine pas. Le serveur envoie ensuite en permanence script au client qui est exécuté immédiatement, fournissant une connexion en temps réel à sens unique à partir du serveur au client. La connexion client au serveur utilise une connexion distincte à partir du serveur pour la connexion du client, et comme une requête HTTP standard, une nouvelle connexion est créée pour chaque élément de données qui doivent être envoyé.
- **AJAX interrogation longue**. Interrogation longue ne crée pas une connexion permanente, mais au lieu de cela interroge le serveur avec une demande reste ouverte jusqu'à ce que le serveur répond, le moment où la connexion se ferme, et une nouvelle connexion est immédiatement demandée. Cela risque d’entraîner un temps de latence tandis que réinitialise la connexion.

Pour plus d’informations sur les transports sont pris en charge les configurations, consultez [plateformes prises en charge](supported-platforms.md).

### <a name="transport-selection-process"></a>Processus de sélection de transport

La liste suivante présente les étapes que SignalR utilise pour déterminer le transport à utiliser.

1. Si le navigateur est Internet Explorer 8 ou version antérieure, l’interrogation longue est utilisée.
2. Si JSONP est configuré (autrement dit, le `jsonp` paramètre est défini sur `true` au démarrage de la connexion), l’interrogation longue est utilisé.
3. Si une connexion entre domaines est apportée (autrement dit, si le point de terminaison SignalR n’est pas dans le même domaine que la page d’hébergement), WebSocket est utilisé si les critères suivants sont satisfaits :

   - Le client prend en charge CORS (Cross-Origin Resource Sharing). Pour plus d’informations sur lequel les clients prennent en charge CORS, consultez [CORS au caniuse.com](http://www.caniuse.com/CORS).
   - Le client prend en charge de WebSocket
   - Le serveur prend en charge de WebSocket

     Si un des critères suivants ne sont pas remplie, l’interrogation longue servira. Pour plus d’informations sur les connexions entre domaines, consultez [comment établir une connexion entre domaines](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).
4. Si JSONP n’est pas configuré et que la connexion n’est pas inter-domaines, WebSocket est utilisé si le client et le serveur la prend en charge.
5. Si le client ou le serveur ne prennent pas en charge WebSocket, les événements envoyés du serveur est utilisé s’il est disponible.
6. Si les événements envoyés du serveur n’est pas disponible, Forever Frame est tentée.
7. Si Forever Frame échoue, l’interrogation longue est utilisé.

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>Surveillance des transports

Vous pouvez déterminer que le transport à l’aide de votre application en activant la journalisation sur votre hub et en ouvrant la fenêtre de console dans votre navigateur.

Pour activer la journalisation des événements de votre concentrateur dans un navigateur, ajoutez la commande suivante à votre application cliente :

`$.connection.hub.logging = true;`

- Dans Internet Explorer, ouvrez les outils de développement en appuyant sur F12 et cliquez sur l’onglet de la Console.

    ![Console dans Microsoft Internet Explorer](introduction-to-signalr/_static/image2.png)
- Dans Chrome, ouvrez la console en appuyant sur Ctrl + Maj + J.

    ![Console Google Chrome](introduction-to-signalr/_static/image3.png)

Ouverture de la console et la journalisation est activée, vous serez en mesure de voir quel protocole de transport est utilisé par SignalR.

![Dans Internet Explorer affichant le transport WebSocket de la console](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>Spécification d’un transport

Négocier un transport prend une certaine quantité de temps et de client/serveur ressources. Si les fonctionnalités de client sont connues, puis un transport peut être spécifié lors de la connexion client est démarrée. L’extrait de code suivant illustre le démarrage d’une connexion en utilisant le transport d’interrogation longue Ajax, que vous utiliseriez s’il était connu que le client ne prenaient pas en charge un autre protocole :

`connection.start({ transport: 'longPolling' });`

Vous pouvez spécifier un ordre de secours si vous souhaitez un client pour essayer les transports spécifiques dans l’ordre. L’extrait de code suivant montre durant la tentative de WebSocket et cas d’échec, directement à l’interrogation longue.

`connection.start({ transport: ['webSockets','longPolling'] });`

Les constantes de chaîne pour la spécification des transports sont définis comme suit :

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>Connexions et les Hubs

L’API SignalR contient deux modèles pour la communication entre les clients et serveurs : les concentrateurs et les connexions persistantes.

Une connexion représente un simple point de terminaison pour l’envoi de messages unique-recipient, groupées ou de diffusion. Permet des API de connexion persistante (représenté par la classe PersistentConnection dans le code .NET) au développeur un accès direct au protocole de communication de bas niveau qui expose de SignalR. À l’aide du modèle de communication de connexions seront familière aux développeurs qui ont utilisé les API basées sur les connexions telles que Windows Communication Foundation.

Un concentrateur est un pipeline plus haut niveau basé sur l’API de connexion qui permet à votre client et le serveur pour appeler des méthodes sur eux directement. SignalR gère la distribution au-delà des limites de la machine comme par magie, autorisant les clients à appeler des méthodes sur le serveur en tant que facilement en tant que méthodes locales et vice versa. L’utilisation du modèle de communication Hubs seront familière aux développeurs qui ont utilisé les API telles que .NET Remoting d’invocation à distance. À l’aide d’un Hub vous permet également de transmettre des paramètres fortement typés aux méthodes, l’activation de la liaison de modèle.

### <a name="architecture-diagram"></a>Diagramme d’architecture

Le diagramme suivant montre la relation entre les concentrateurs, des connexions persistantes et les technologies sous-jacentes utilisées pour les transports.

![Diagramme d’Architecture SignalR montrant des API, les transports et les clients](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>Fonctionnement des Hubs

Lorsque le code côté serveur appelle une méthode sur le client, un paquet est envoyé via le transport actif qui contient le nom et les paramètres de la méthode à appeler (lorsqu’un objet est envoyé comme paramètre de méthode, il est sérialisé à l’aide de JSON). Le client fait ensuite correspond le nom de méthode pour les méthodes définies dans le code côté client. S’il existe une correspondance, la méthode du client s’exécutera en utilisant les données de paramètre désérialisées.

L’appel de méthode peut être surveillé à l’aide des outils tels que [Fiddler.](http://fiddler2.com/) L’illustration suivante montre un appel de méthode envoyé à partir d’un serveur de SignalR pour un client de navigateur web dans le volet des journaux de Fiddler. L’appel de méthode est envoyé à partir d’un hub appelé `MoveShapeHub`, et la méthode appelée est appelée `updateShape`.

![Affichage du journal de Fiddler montrant le trafic de SignalR](introduction-to-signalr/_static/image6.png)

Dans cet exemple, le nom de hub est identifié avec le `H` paramètre ; la méthode nom est identifié avec le `M` paramètre et les données envoyées à la méthode est identifié par le `A` paramètre. L’application qui a généré ce message est créée dans le [en temps réel haute fréquence](tutorial-high-frequency-realtime-with-signalr.md) didacticiel.

### <a name="choosing-a-communication-model"></a>Choix d’un modèle de communication

La plupart des applications doivent utiliser l’API de Hubs. L’API de connexions peut être utilisée dans les circonstances suivantes :

- Le format des besoins réels message envoyé à être spécifié.
- Le développeur souhaite travailler avec un modèle de messagerie et de distribution plutôt que d’un modèle d’appel distant.
- Une application existante qui utilise un modèle de messagerie est portée pour utiliser SignalR.
