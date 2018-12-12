---
uid: signalr/overview/older-versions/handling-connection-lifetime-events
title: Compréhension et gestion des événements de durée de vie de connexion dans SignalR 1.x | Microsoft Docs
author: pfletcher
description: Cet article décrit comment utiliser les événements exposés par l’API Hubs.
ms.author: riande
ms.date: 06/05/2013
ms.assetid: e608e263-264d-448b-b0eb-6eeb77713b22
msc.legacyurl: /signalr/overview/older-versions/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: f965c38e18c442268f9bb1d7ffb5e98a135efade
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287675"
---
<a name="understanding-and-handling-connection-lifetime-events-in-signalr-1x"></a>Compréhension et gestion des événements de durée de vie de connexion dans SignalR 1.x
====================
par [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Cet article présente les événements de connexion et déconnexion reconnexion SignalR que vous pouvez gérer et les paramètres de délai d’expiration et keepalive que vous pouvez configurer.
> 
> Cet article suppose que vous connaissez déjà des événements de durée de vie de SignalR et de connexion. Pour une introduction à SignalR, consultez [SignalR - vue d’ensemble - mise en route](index.md). Pour obtenir la liste des événements de durée de vie de connexion, consultez les ressources suivantes :
> 
> - [Comment gérer les événements de durée de vie de connexion dans la classe de concentrateur](index.md)
> - [Comment gérer les événements de durée de vie de connexion dans les clients JavaScript](index.md)
> - [Comment gérer les événements de durée de vie de connexion dans les clients .NET](index.md)


## <a name="overview"></a>Vue d'ensemble

Cet article contient les sections suivantes :

- [Scénarios et la terminologie de durée de vie de connexion](#terminology)

    - [Les connexions SignalR, les connexions de transport et les connexions physiques](#signalrvstransport)
    - [Scénarios de déconnexion de transport](#transportdisconnect)
    - [Scénarios de déconnexion de client](#clientdisconnect)
    - [Scénarios de déconnexion de serveur](#serverdisconnect)
- [Paramètres de délai d’expiration et keepalive](#timeoutkeepalive)

    - [ConnectionTimeout](#connectiontimeout)
    - [DisconnectTimeout](#disconnecttimeout)
    - [KeepAlive](#keepalive)
    - [Comment modifier les paramètres de délai d’expiration et keepalive](#changetimeout)
- [Comment informer l’utilisateur de déconnexions](#notifydisconnect)
- [Comment reconnecter en continu](#continuousreconnect)
- [Comment se déconnecter d’un client dans le code serveur](#disconnectclientfromserver)

Liens vers des rubriques de référence de l’API sont à la version de .NET 4.5 de l’API. Si vous utilisez .NET 4, consultez [la version de .NET 4 des rubriques API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>Scénarios et la terminologie de durée de vie de connexion

Le `OnReconnected` Gestionnaire d’événements dans un concentrateur SignalR peut exécuter directement après `OnConnected` mais pas après `OnDisconnected` pour un client donné. Vous pouvez avoir une reconnexion sans une déconnexion parce qu’il existe plusieurs façons dans lequel le mot « connexion » est utilisé dans SignalR.

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>Les connexions SignalR, les connexions de transport et les connexions physiques

Cet article fait la distinction entre *les connexions SignalR*, *connexions de transport*, et *connexions physiques*:

- **Connexion SignalR** fait référence à une relation logique entre un client et une URL de serveur géré par l’API SignalR et identifiée par un ID de connexion. Les données relatives à cette relation sont gérées par SignalR et sont utilisées pour établir une connexion de transport. Les extrémités de relation et SignalR supprime les données lorsque le client appelle le `Stop` méthode ou une limite de délai d’expiration est atteint pendant la tentative de SignalR rétablir une connexion de transport perdu.
- **Connexion de transport** fait référence à une relation logique entre un client et un serveur géré par une des quatre API transport : WebSockets, événements de serveur a été envoyé, forever frame ou d’interrogation longue. SignalR utilise le transport API pour créer une connexion de transport, et l’API de transport dépend de l’existence d’une connexion réseau physique pour créer la connexion de transport. La connexion de transport se termine lorsque l’arrête SignalR ou lorsque le transport API détecte que la connexion physique est rompue.
- **Connexion physique** fait référence aux liens de réseau physique--fils, signaux sans fil, routeurs, etc., qui facilite la communication entre un ordinateur client et un ordinateur serveur. La connexion physique doit être présente afin d’établir une connexion de transport, et une connexion de transport doit être établie afin d’établir une connexion SignalR. Toutefois, avec rupture de la connexion physique ne toujours immédiatement fin à la connexion de transport ou de la connexion SignalR, comme expliqué plus loin dans cette rubrique.

Dans le diagramme suivant, la connexion de SignalR est représentée par l’API des concentrateurs et de la couche de PersistentConnection API SignalR, la connexion de transport est représentée par la couche de Transports et la connexion physique est représentée par les lignes entre le serveur et les clients.

![Diagramme d’architecture de SignalR](handling-connection-lifetime-events/_static/image1.png)

Lorsque vous appelez le `Start` méthode dans un client SignalR, vous fournissez le code client SignalR avec toutes les informations nécessaires afin d’établir une connexion à un serveur physique. Le code client SignalR utilise ces informations pour effectuer une demande HTTP et établir une connexion physique qui utilise l’une des méthodes de transport de quatre. Si l’échec de la connexion de transport ou le serveur ne parvient pas, la connexion SignalR ne disparaît immédiatement, car le client a toujours les informations nécessaires rétablir automatiquement une nouvelle connexion de transport à la même URL de SignalR. Dans ce scénario, aucune intervention de l’application de l’utilisateur n’est impliquée, et lorsque le code de client SignalR établit une nouvelle connexion de transport, il ne démarre pas une nouvelle connexion de SignalR. La continuité de la connexion de SignalR est reflétée dans le fait que l’ID de connexion, qui est créé lorsque vous appelez le `Start` (méthode), ne change pas.

Le `OnReconnected` Gestionnaire d’événements sur le Hub s’exécute lorsqu’une connexion de transport est automatiquement rétablie après avoir été perdues. Le `OnDisconnected` Gestionnaire d’événements s’exécute à la fin d’une connexion SignalR. Une connexion SignalR peut se terminer par une des manières suivantes :

- Si le client appelle le `Stop` (méthode), un message d’arrêt est envoyé au serveur et client et le serveur interrompt la connexion SignalR immédiatement.
- Une fois la connectivité entre le client et le serveur est perdue, le client tente de se reconnecter et le serveur attend que le client se reconnecter. Si les tentatives de reconnexion échouent et le délai de déconnexion se termine, client et serveur mettre fin à la connexion de SignalR. Le client cesse toute tentative de reconnexion, et le serveur supprime sa représentation sous forme de la connexion de SignalR.
- Si le client cesse de s’exécuter sans avoir l’occasion de vous appeler le `Stop` (méthode), le serveur attend que le client pour vous reconnecter, puis met fin à la connexion SignalR après le délai de déconnexion.
- Si le serveur s’arrête en cours d’exécution, le client tente de se reconnecter (recréer la connexion de transport), puis met fin à la connexion SignalR après le délai de déconnexion.

Quand il n’y a aucun problème de connexion, et l’application de l’utilisateur termine la connexion SignalR en appelant le `Stop` (méthode), la connexion de SignalR et la connexion de transport commencer et se terminer en même temps. Les sections suivantes décrivent plus en détail les autres scénarios.

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>Scénarios de déconnexion de transport

Connexions physiques peuvent être lentes ou il peut y avoir des interruptions de connectivité. En fonction de facteurs tels que la longueur de l’interruption, la connexion de transport peut-être être supprimée. SignalR puis tente de rétablir la connexion de transport. Parfois, la connexion de transport API détecte l’interruption et supprime la connexion de transport, et SignalR découvre immédiatement que la connexion est perdue. Dans d’autres scénarios, la connexion de transport API ni SignalR est informé immédiatement que la connectivité a été perdue. Pour tous les transports, à l’exception d’interrogation longue, le client SignalR utilise une fonction appelée *keepalive* dont vous recherchez une perte de connectivité que l’API de transport ne peut pas détecter. Pour plus d’informations sur la durée pendant laquelle les connexions d’interrogation, consultez [les paramètres de délai d’expiration et keepalive](#timeoutkeepalive) plus loin dans cette rubrique.

Lorsqu’une connexion est inactive, le serveur envoie régulièrement un paquet persistant au client. À compter de la date de que cet article est en cours d’écriture, la fréquence par défaut est toutes les 10 secondes. En écoutant pour ces paquets, les clients peuvent indiquer s’il existe un problème de connexion. Si un paquet persistant n’est pas reçu lorsque attendu, après un bref instant le client suppose qu’il n’y a des problèmes de connexion telles que la lenteur ou des interruptions. Si le keepalive est toujours pas reçu après une durée plus longue, le client suppose que la connexion a été supprimée, et il commence la tentative de reconnexion.

Le diagramme suivant illustre les événements de client et serveur qui sont déclenchés dans un scénario classique lorsqu’il existe des problèmes avec la connexion physique qui ne sont pas immédiatement reconnus par le transport API. Le diagramme s’applique aux circonstances suivantes :

- Le transport est WebSockets, forever frame ou événements de serveur a été envoyé.
- Il existe différentes périodes d’interruption de la connexion réseau physique.
- L’API de transport ne devient-elle pas prenant en charge de l’interruption, afin de SignalR s’appuie sur les fonctionnalités de keepalive les détecter.

![Déconnexions de transport](handling-connection-lifetime-events/_static/image2.png)

Si le client passe en mode de reconnexion, mais ne peut pas établir une connexion de transport dans la limite de délai d’attente de déconnexion, le serveur met fin à la connexion de SignalR. Lorsque cela se produit, le serveur exécute le Hub `OnDisconnected` (méthode) et les files d’attente d’un message de déconnexion à envoyer au client dans le cas où le client gère pour vous connecter plus tard. Si le client puis se reconnecte, il reçoit la commande de déconnexion et appelle le `Stop` (méthode). Dans ce scénario, `OnReconnected` n’est pas exécuté lorsque le client se reconnecte, et `OnDisconnected` n’est pas exécuté lorsque le client appelle `Stop`. Le diagramme suivant illustre ce scénario.

![Interruptions de transport - délai d’attente du serveur](handling-connection-lifetime-events/_static/image3.png)

Les événements de durée de vie de connexion SignalR qui peuvent être déclenchés sur le client sont les suivantes :

- `ConnectionSlow` événement de client.

    Déclenché lorsqu’une proportion prédéfinie de la période de délai d’expiration de keepalive est écoulé depuis le dernier message ou keepalive ping a été reçu. Le délai d’avertissement par défaut keepalive est 2/3 du délai d’expiration de keepalive. Le délai d’expiration de keepalive étant 20 secondes, l’avertissement se produit dans environ 13 secondes.

    Par défaut, le serveur envoie les pings de keepalive toutes les 10 secondes, et le client vérifie les pings keepalive sur toutes les 2 secondes (un tiers de la différence entre la valeur de délai d’expiration de keepalive et la valeur d’avertissement de délai d’attente keepalive).

    Si le transport API reconnaît une déconnexion, SignalR peut être informé de la déconnexion avant que le délai d’avertissement keepalive passe. Dans ce cas, le `ConnectionSlow` n'aurait pas été déclenché et SignalR irait directement à la `Reconnecting` événement.
- `Reconnecting` événement de client.

    Déclenché lorsque l’API de transport détecte que la connexion est perdue, ou (b) le délai d’attente keepalive s’est écoulé depuis le dernier message ou keepalive ping a été reçu. Le code de client SignalR commence la tentative de reconnexion. Vous pouvez gérer cet événement si vous souhaitez que votre application exécute une action lorsqu’une connexion de transport est perdue. Le délai de conservation par défaut est actuellement de 20 secondes.

    Si votre code client tente d’appeler une méthode de concentrateur pendant que SignalR est en mode de reconnexion, SignalR va tenter d’envoyer la commande. La plupart du temps, telles que les tentatives échouent, mais dans certains cas, il peuvent réussir. Pour les événements de serveur a été envoyé, indéfiniment le frame et durée pendant laquelle les transports d’interrogation, SignalR utilise deux canaux de communication, le client utilise pour envoyer des messages et l’autre qu’elle utilise pour recevoir des messages. Le canal utilisé pour la réception est celui qui est ouverte en permanence, et c’est celui qui est fermé lorsque la connexion physique est interrompue. Le canal utilisé pour l’envoi reste disponible, par conséquent, si la connectivité physique est rétablie, un appel de méthode à partir du client au serveur peut réussi avant que le canal de réception est rétabli. La valeur de retour n’est pas reçue jusqu'à ce que SignalR rouvre le canal utilisé pour la réception.
- `Reconnected` événement de client.

    Déclenché lorsque la connexion de transport est rétablie. Le `OnReconnected` s’exécute le Gestionnaire d’événements dans le Hub.
- `Closed` événement de client (`disconnected` événements dans JavaScript).

    Déclenché lorsque le délai de déconnexion expire pendant que le code de client SignalR tente de se reconnecter après avoir perdu la connexion de transport. La valeur par défaut déconnecter délai d’expiration est de 30 secondes. (Cet événement est également déclenché lorsque la connexion se termine parce que le `Stop` méthode est appelée.)

Les interruptions de connexion de transport qui ne sont pas détectées par l’API de transport et de ne pas différer la réception des pings keepalive à partir du serveur plus longtemps que le délai d’avertissement keepalive ne peuvent pas entraîner toute connexion déclenchement des événements de durée de vie.

Certains environnements réseau délibérément fermer les connexions inactives, et une autre fonction des paquets keepalive consiste à résoudre ce problème en vous permettant de que ces réseaux savoir qu’une connexion de SignalR est en cours d’utilisation. Dans les cas extrêmes la fréquence par défaut de keepalive pings sans réponse peut-être pas suffisant pour empêcher les connexions fermées. Dans ce cas, vous pouvez configurer les pings de keepalive doit être envoyé plus souvent. Pour plus d’informations, consultez [les paramètres de délai d’expiration et keepalive](#timeoutkeepalive) plus loin dans cette rubrique.

> [!NOTE] 
> 
> [!IMPORTANT]
> La séquence d’événements décrites ici n’est pas garantie. SignalR effectue chaque tentative pour déclencher des événements de durée de vie de connexion de manière prévisible en fonction de ce schéma, mais il existe de nombreuses variations des événements de réseau et dans lequel les infrastructures de communications sous-jacentes telles que les API de transport les gérer de nombreuses façons. Par exemple, le `Reconnected` événement ne peut pas être déclenché lorsque le client se reconnecte, ou le `OnConnected` gestionnaire sur le serveur peut s’exécuter lorsque la tentative pour établir une connexion échoue. Cette rubrique décrit uniquement les effets qui seraient normalement produits par certains des circonstances normales.


<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>Scénarios de déconnexion de client

Dans un navigateur client, le code client SignalR qui gère une connexion SignalR s’exécute dans le contexte de JavaScript d’une page web. Qui a raison pour laquelle la connexion SignalR a à la fin lorsque vous accédez à partir d’une page à l’autre et ce de pourquoi vous avez plusieurs connexions avec plusieurs ID de connexion si vous vous connectez à partir de plusieurs onglets ou fenêtres du navigateur. Lorsque l’utilisateur ferme une fenêtre de navigateur ou un onglet, ou navigue vers une nouvelle page ou actualise la page, la connexion de SignalR se termine immédiatement, car le code client SignalR gère cet événement de navigateur pour vous et appelle le `Stop` (méthode). Dans ces scénarios, ou dans n’importe quelle plateforme client lorsque votre application appelle la `Stop` (méthode), le `OnDisconnected` Gestionnaire d’événements s’exécute immédiatement sur le serveur et le client déclenche le `Closed` événement (l’événement est nommé `disconnected` dans JavaScript).

Si une application cliente ou l’ordinateur sur lequel il est exécuté sur tombe en panne ou en veille (par exemple, lorsque l’utilisateur ferme l’ordinateur portable), le serveur n’est pas informé de ce qui est arrivé. Pour autant que déjà connus du serveur, la perte du client peut être en raison d’une interruption de connectivité et le client essaie de se reconnecter. Par conséquent, dans ces scénarios, le serveur attend pour donner au client une occasion de vous reconnecter, et `OnDisconnected` ne s’exécute pas jusqu'à l’expiration de la période de délai d’attente de déconnexion (environ 30 secondes par défaut). Le diagramme suivant illustre ce scénario.

![Échec de l’ordinateur client](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>Scénarios de déconnexion de serveur

Lorsqu’un serveur est mis hors connexion, il redémarre, échoue, le domaine d’application recycle, etc.--le résultat peut être semblable à une perte de connexion, ou l’API de transport et SignalR peuvent savoir immédiatement que le serveur a disparu, SignalR pourrait commencer tente de se reconnecter sans déclencher la `ConnectionSlow` événement. Si le client passe en mode de reconnexion et si le serveur récupère ou redémarrages ou un nouveau serveur est mis en ligne avant l’expiration de la période de délai d’attente de déconnexion, le client se reconnectera au serveur restauré ou nouveau. Dans ce cas, la connexion SignalR continue sur le client et le `Reconnected` événement est déclenché. Sur le premier serveur, `OnDisconnected` n’est jamais exécutée et sur le nouveau serveur, `OnReconnected` est exécuté même si `OnConnected` n’a jamais été exécutée pour ce client sur ce serveur avant. (L’effet est le même si le client se reconnecte au serveur même après un recyclage de domaine d’application ou de redémarrage, car lorsque le serveur redémarre n’a aucune mémoire de l’activité de connexion précédentes). Le diagramme suivant part du principe que l’API de transport est informé de la perte de la connexion immédiatement, par conséquent, le `ConnectionSlow` événement n’est pas déclenché.

![Reconnexion et défaillance du serveur](handling-connection-lifetime-events/_static/image5.png)

Si un serveur n’est pas disponible dans le délai de déconnexion, la connexion SignalR se termine. Dans ce scénario, le `Closed` événement (`disconnected` dans les clients JavaScript) est déclenché sur le client mais `OnDisconnected` n’est jamais appelée sur le serveur. Le diagramme suivant part du principe que le transport API ne devient-elle pas conscient de la connexion est perdue, donc il est détecté par la fonctionnalité de keepalive SignalR et `ConnectionSlow` événement est déclenché.

![Délai d’expiration et de défaillance du serveur](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>Paramètres de délai d’expiration et keepalive

La valeur par défaut `ConnectionTimeout`, `DisconnectTimeout`, et `KeepAlive` les valeurs appropriées pour la plupart des scénarios, mais peut être modifiées si votre environnement comporte des besoins particuliers. Par exemple, si votre environnement réseau ferme les connexions sont inactives pendant 5 secondes, vous devrez peut-être diminuer la valeur de keepalive.

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

Ce paramètre représente la quantité de délai de maintien d’une connexion de transport ouvert et attend une réponse avant de fermer et ouvrir une nouvelle connexion. La valeur par défaut est de 110 secondes.

Ce paramètre s’applique uniquement lorsque les fonctionnalités de keepalive sont désactivée, auquel s’applique normalement qu’à long transport d’interrogation. Le diagramme suivant illustre l’effet de ce paramètre sur une longue durée d’interrogation de connexion de transport.

![Durée pendant laquelle la connexion de transport d’interrogation](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>DisconnectTimeout

Ce paramètre représente la quantité de temps d’attente après une connexion de transport est perdue avant de déclencher le `Disconnected` événement. La valeur par défaut est de 30 secondes. Lorsque vous définissez `DisconnectTimeout`, `KeepAlive` est automatiquement défini sur 1/3 de la `DisconnectTimeout` valeur.

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

Ce paramètre représente le délai d’attente avant l’envoi d’un paquet persistant sur une connexion inactive. La valeur par défaut est 10 secondes. Cette valeur ne doit pas être plus de 1/3 de la `DisconnectTimeout` valeur.

Si vous souhaitez définir les deux `DisconnectTimeout` et `KeepAlive`, affectez la valeur `KeepAlive` après `DisconnectTimeout`. Sinon, votre `KeepAlive` paramètre sera remplacée lorsque `DisconnectTimeout` définit automatiquement `KeepAlive` à 1/3 de la valeur de délai d’attente.

Si vous souhaitez désactiver la fonctionnalité de keepalive, définissez `KeepAlive` avec la valeur null. Fonctionnalités de KeepAlive sont automatiquement désactivée pour le long d’interrogation de transport.

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>Comment modifier les paramètres de délai d’expiration et keepalive

Pour modifier les valeurs par défaut pour ces paramètres, définissez-les `Application_Start` dans votre *Global.asax* de fichiers, comme indiqué dans l’exemple suivant. Les valeurs indiquées dans l’exemple de code sont le mêmes que les valeurs par défaut.

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>Comment informer l’utilisateur de déconnexions

Dans certaines applications, vous souhaiterez afficher un message à l’utilisateur lorsqu’il existe des problèmes de connectivité. Vous avez plusieurs options sur la manière et le moment pour ce faire. Les exemples de code suivants sont pour un client JavaScript à l’aide du proxy généré.

- Gérer le `connectionSlow` événement pour afficher un message dès que SignalR tient compte des problèmes de connexion, avant d’entrer dans le mode de reconnexion.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- Gérer le `reconnecting` événement pour afficher un message lorsque SignalR connaît une déconnexion et va passer à la reconnexion de mode.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- Gérer le `disconnected` événement pour afficher un message lorsqu’une tentative de reconnexion a expiré. Dans ce scénario, la seule façon de rétablir une connexion avec le serveur est de redémarrer la connexion de SignalR en appelant le `Start` (méthode), qui crée un nouvel ID de connexion. L’exemple de code suivant utilise un indicateur pour vous assurer que vous émettez la notification uniquement après un délai d’attente qui se reconnectent, pas après une fin normale à la connexion de SignalR provoquée en appelant le `Stop` (méthode).

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>Comment reconnecter en continu

Dans certaines applications, vous souhaiterez automatiquement de rétablir une connexion une fois qu’il a été perdu et de la tentative de reconnexion a expiré. Pour ce faire, vous pouvez appeler la `Start` méthode à partir de votre `Closed` Gestionnaire d’événements (`disconnected` Gestionnaire d’événements sur les clients JavaScript). Vous souhaiterez peut-être attendre un certain temps avant d’appeler `Start` afin d’éviter cette opération trop fréquemment lorsque le serveur ou la connexion physique ne sont pas disponibles. L’exemple de code suivant est pour un client JavaScript à l’aide du proxy généré.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

Un problème potentiel à retenir dans les clients mobiles est que les tentatives de reconnexion continue lorsque le serveur ou la connexion physique n’est pas disponible peut entraîner une décharge la batterie inutiles.

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>Comment se déconnecter d’un client dans le code serveur

SignalR version 1.1.1 n’a pas d’un API de serveur intégré pour la déconnexion des clients. Il existe [plans pour ajouter cette fonctionnalité à l’avenir](https://github.com/SignalR/SignalR/issues/2101). Dans la version actuelle de SignalR, le plus simple pour vous déconnecter d’un client à partir du serveur consiste à implémenter une méthode de déconnexion sur le client et appeler cette méthode à partir du serveur. L’exemple de code suivant montre une méthode de déconnexion pour un client JavaScript à l’aide du proxy généré.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> Sécurité - ni cette méthode pour la déconnexion des clients ni l’API intégrée proposée répondront le scénario de piratés clients qui exécutent un code malveillant, dans la mesure où les clients Impossible de vous reconnecter ou le code piraté peut supprimer la `stopClient` méthode ou modification ce qu’il fait. L’emplacement approprié pour implémenter la protection par déni de service (DOS) avec état est pas dans l’infrastructure ou de la couche du serveur, mais plutôt dans infrastructure front-end.
