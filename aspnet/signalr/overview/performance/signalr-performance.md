---
uid: signalr/overview/performance/signalr-performance
title: Performances de SignalR | Microsoft Docs
author: pfletcher
description: Performances de SignalR
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: ae19493c46ae9670bd200529f73b74b0c3f4db00
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834024"
---
<a name="signalr-performance"></a>Performances de SignalR
====================
par [Patrick Fletcher](https://github.com/pfletcher)

> Cette rubrique décrit comment concevoir pour, mesurer et améliorer les performances dans une application de SignalR.
> 
> ## <a name="software-versions-used-in-this-topic"></a>Versions des logiciels utilisées dans cette rubrique
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR version 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Versions précédentes de cette rubrique
> 
> Pour plus d’informations sur les versions antérieures de SignalR, consultez [les Versions antérieures de SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Questions et commentaires
> 
> Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer dans les commentaires en bas de la page. Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).


Pour obtenir une présentation récente sur les performances de SignalR et de la mise à l’échelle, consultez [mise à l’échelle du Web en temps réel avec ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).

Cette rubrique contient les sections suivantes :

- [Considérations de conception](#design)
- [Réglage des performances de votre serveur SignalR](#tuning)
- [Résolution des problèmes de performances](#troubleshooting)
- [À l’aide de compteurs de performances de SignalR](#perfcounters)
- [À l’aide d’autres compteurs de performances](#othercounters)
- [Autres ressources](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>Considérations de conception

Cette section décrit les modèles qui peuvent être implémentées lors de la conception d’une application de SignalR, pour vous assurer que les performances ne sont pas en cours freinées par générer un trafic réseau inutile.

### <a name="throttling-message-frequency"></a>La limitation de fréquence de message

Même dans une application qui envoie des messages à une fréquence élevée (par exemple, une application de jeu en temps réel), la plupart des applications n’avez pas besoin envoyer plusieurs messages par seconde. Pour réduire la quantité de trafic qui génère de chaque client, une boucle de messages peut être implémentée que les files d’attente et envoie les messages ne dépasse pas fréquemment un taux fixe (autrement dit, jusqu'à un certain nombre de messages envoyés par seconde, s’il existe des messages pendant cette période dans onstruction à envoyer). Pour un exemple d’application qui limite les messages à un certain taux (à partir de client et serveur), consultez [en temps réel haute fréquence avec SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).

### <a name="reducing-message-size"></a>Réduire la taille de message

Vous pouvez réduire la taille d’un message SignalR en réduisant la taille de vos objets sérialisés. Dans le code serveur, si vous envoyez un objet qui contient les propriétés que vous n’avez pas besoin d’être transmis, empêcher ces propriétés en cours de sérialisation à l’aide de la `JsonIgnore` attribut. Les noms des propriétés sont également stockés dans le message. les noms des propriétés peuvent être abrégées à l’aide de la `JsonProperty` attribut. L’exemple de code suivant montre comment exclure une propriété d’être envoyé au client et raccourcir les noms de propriété :

**Code de serveur .NET qui explique l’attribut JsonIgnore à exclure des données envoyées au client et l’attribut JsonProperty pour réduire la taille de message**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

Afin de conserver la lisibilité / la facilité de gestion dans le code client, les noms de propriété abrégée peuvent être des noms remappés à convivial une fois que le message est reçu. L’exemple de code suivant montre une façon possible de remappage des noms abrégées à plus longues, en définissant un contrat de message (mappage) et à l’aide de la `reMap` fonction à appliquer le contrat à la classe de message optimisée :

**Le code JavaScript côté client qui remappe abrégés des noms de propriété aux noms contrôlable de visu**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

Les noms peuvent être abrégées dans les messages à partir du client au serveur, à l’aide de la même méthode.

En réduisant l’encombrement de mémoire (autrement dit, la quantité de mémoire utilisée pour le message) du message objet peut également améliorer les performances. Par exemple, si la plage complète d’un `int` n’est pas nécessaire, un `short` ou `byte` peut être utilisé à la place.

Dans la mesure où les messages sont stockés dans le bus de messages dans la mémoire du serveur, ce qui réduit la taille des messages peut également résoudre les problèmes de mémoire de serveur.

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>Réglage des performances de votre serveur SignalR

Les paramètres de configuration suivants peuvent être utilisés pour paramétrer votre serveur pour optimiser les performances dans une application de SignalR. Pour obtenir des informations générales sur la façon d’améliorer les performances dans une application ASP.NET, consultez [amélioration des performances ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).

**Paramètres de configuration de SignalR**

- **DefaultMessageBufferSize**: par défaut, SignalR conserve 1 000 messages en mémoire par hub par connexion. Si les messages volumineux sont utilisées, cela peut créer des problèmes de mémoire qui peuvent être résolus en réduisant cette valeur. Ce paramètre peut être défini dans le `Application_Start` Gestionnaire d’événements dans une application ASP.NET ou dans le `Configuration` méthode d’une classe de démarrage OWIN dans une application auto-hébergée. L’exemple suivant montre comment réduire cette valeur afin de réduire l’encombrement mémoire de votre application afin de réduire la quantité de mémoire du serveur utilisé :

    **Code de serveur .NET dans Startup.cs pour diminuer la taille de mémoire tampon de message par défaut**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**Paramètres de configuration IIS**

- **Nombre maximal de demandes simultanées par application**: augmentation du nombre de IIS simultanées demandes augmente les ressources serveur disponibles au traitement des requêtes. La valeur par défaut est 5000 ; Pour augmenter ce paramètre, exécutez les commandes suivantes dans une invite de commandes avec élévation de privilèges :

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- **ApplicationPool QueueLength**: il s’agit du nombre maximal de demandes que Http.sys files d’attente du pool d’applications. Lorsque la file d’attente est pleine, les nouvelles demandes de recevoir une réponse de « Service indisponible » 503. La valeur par défaut est 1000.

    Raccourcir la longueur de file d’attente pour le processus de travail dans le pool d’applications hébergeant votre application sera économiser les ressources de mémoire. Pour plus d’informations, consultez [gestion, paramétrage et configuration des Pools d’applications](https://technet.microsoft.com/library/cc745955.aspx).

**Paramètres de configuration ASP.NET**

Cette section inclut des paramètres de configuration qui peuvent être définies dans le `aspnet.config` fichier. Ce fichier se trouve dans un des deux emplacements, en fonction de la plateforme :

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

Paramètres de ASP.NET qui peuvent améliorer les performances de SignalR sont les suivants :

- **Nombre maximal de requêtes simultané par UC**: augmentation de ce paramètre peut atténuer les goulets d’étranglement. Pour augmenter ce paramètre, ajoutez le paramètre de configuration suivant à la `aspnet.config` fichier :

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **Limite de la file d’attente des requêtes**: lorsque le nombre total de connexions dépasse le `maxConcurrentRequestsPerCPU` définissant, ASP.NET commence à l’aide d’une file d’attente des demandes de limitation. Pour augmenter la taille de la file d’attente, vous pouvez augmenter la `requestQueueLimit` paramètre. Pour ce faire, ajoutez le paramètre de configuration suivant à la `processModel` nœud `config/machine.config` (plutôt que `aspnet.config`) :

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>Résolution des problèmes de performances

Cette section décrit les façons de rechercher les goulots d’étranglement dans votre application.

### <a name="verifying-that-websocket-is-being-used"></a>Vérification que WebSocket est utilisé

Bien que SignalR permettre utiliser une variété de transports pour la communication entre client et serveur, WebSocket offre un avantage de performances significatifs et doit être utilisé si le client et le serveur la prend en charge. Pour déterminer si votre client et le serveur de répondre à la configuration requise pour le WebSocket, consultez [Transports et les solutions de secours](../getting-started/introduction-to-signalr.md#transports). Pour déterminer que le transport est utilisé dans votre application, vous pouvez utiliser les outils de développement du navigateur et examinez les journaux pour vérifier que le transport est utilisé pour la connexion. Pour plus d’informations sur l’utilisation des outils de développement de navigateur dans Internet Explorer et Chrome, consultez [Transports et les solutions de secours](../getting-started/introduction-to-signalr.md#transports).

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>À l’aide de compteurs de performances de SignalR

Cette section décrit comment activer et utiliser des compteurs de performances de SignalR, trouvé dans le `Microsoft.AspNet.SignalR.Utils` package.

### <a name="installing-signalrexe"></a>L’installation signalr.exe

Compteurs de performances peuvent être ajoutés au serveur à l’aide d’un utilitaire appelé SignalR.exe. Pour installer cet utilitaire, procédez comme suit :

1. Dans votre application Visual Studio, sélectionnez **outils**, **Library Package Manager**, **gérer les Packages NuGet pour la Solution...**
2. Recherchez **signalr.utils**et sélectionnez Installer.

    ![](signalr-performance/_static/image1.png)
3. Acceptez le contrat de licence pour installer le package.
4. SignalR.exe sera installé à `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.

### <a name="installing-performance-counters-with-signalrexe"></a>Installation des compteurs de performances avec SignalR.exe

Pour installer les compteurs de performance SignalR, exécutez SignalR.exe dans une invite de commandes avec élévation de privilèges avec le paramètre suivant :

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

Pour supprimer les compteurs de performance SignalR, exécutez SignalR.exe dans une invite de commandes avec élévation de privilèges avec le paramètre suivant :

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>Compteurs de performances de SignalR

Le package d’utilitaires installe des compteurs de performances suivants. Les compteurs « Total » mesurent le nombre d’événements dans la mesure où le dernier pool d’applications ou le serveur de redémarrer.

**Métriques de connexion**

Les métriques suivantes mesurent les événements de durée de vie de connexion qui se produisent. Pour plus d’informations, consultez [compréhension et gestion des événements de durée de vie de connexion](../guide-to-the-api/handling-connection-lifetime-events.md).

- **Connexions connectées**
- **Connexions reconnectées**
- **Connexions déconnectées**
- **Connexions en cours**

**Métriques de message**

Les métriques suivantes mesurent le trafic des messages généré par SignalR.

- **Nombre Total de Messages de connexion reçus**
- **Nombre Total de Messages de connexion envoyés**
- **Connexion de Messages reçus/s**
- **Connexion de Messages envoyés/s**

**Métriques de bus de messages**

Les métriques suivantes mesurent le trafic via le bus de messages SignalR interne, la file d’attente dans laquelle tous les messages de SignalR entrants et sortants sont placés. Un message est **publié** quand il est envoyé ou de diffusion. Un **abonné** dans ce contexte est un abonnement sur le bus de messages ; il doit être égal au nombre de clients plus le serveur lui-même. Un **travail alloué** est un composant qui envoie des données à des connexions actives ; un **travail occupé** est celui qui envoie un message activement.

- **Total reçu des Messages de Bus de messages**
- **Bus de messages Messages reçus/s**
- **Bus de messages Messages publiés Total**
- **Bus de messages Messages publiés par seconde**
- **Message Bus abonnés actuel**
- **Total des abonnés Bus de messages**
- **Les abonnés de Bus de messages/s**
- **Bus de messages allouée travailleurs**
- **Threads de travail occupés de Bus de messages**
- **Actuel de rubriques de Bus de messages**

**Mesures d’erreur**

Les métriques suivantes mesurent les erreurs générées par le trafic des messages SignalR. **Résolution du Hub** erreurs se produisent quand un hub ou une méthode de concentrateur ne peut pas être résolu. **Appel de concentrateur** erreurs sont des exceptions levées lors de l’appel d’une méthode de concentrateur. **Transport** erreurs sont des erreurs de connexion levées pendant une requête ou réponse HTTP.

- **Erreurs : Total de toutes les**
- **Erreurs : All/s**
- **Erreurs : Total de résolution Hub**
- **Erreurs : Résolution de concentrateur par seconde**
- **Erreurs : Total d’appel de concentrateur**
- **Erreurs : Appel de concentrateur par seconde**
- **Erreurs : Total de Transport**
- **Erreurs : Transport/s**

<a id="scaleout_metrics"></a>

**Métriques de montée en puissance parallèle**

Les métriques suivantes mesurent le trafic et les erreurs générées par le fournisseur de montée en puissance parallèle. Un **Stream** dans ce contexte est une unité d’échelle utilisée par le fournisseur de montée en puissance parallèle ; il s’agit une table si SQL Server est utilisé, une rubrique si Service Bus est utilisé et un abonnement si Redis est utilisé. Chaque flux garantit ordonnée opérations de lecture et écriture ; un seul flux étant un goulot d’étranglement potentiel de mise à l’échelle, le nombre de flux peut être augmenté pour aider à réduire ce goulot d’étranglement. Si plusieurs flux sont utilisés, SignalR répartit automatiquement les messages (partition) entre ces flux d’une façon qui garantit que les messages envoyés à partir d’une connexion donnée sont dans l’ordre.

Le [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) paramètre détermine la longueur de la file d’envoi de montée en puissance parallèle gérée par SignalR. Lui attribuant une valeur supérieure à 0 place tous les messages dans une file d’attente d’envoi à envoyer à la fois pour le fond de panier de messagerie configuré. Si la taille de la file d’attente dépasse la longueur configurée, les appels suivants à envoyer va échouent immédiatement avec un [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) jusqu'à ce que le nombre de messages dans la file d’attente est inférieure à celle du paramètre à nouveau. File d’attente est désactivée par défaut, car les fonds de panier implémentées ont généralement leurs propres queuing ou le contrôle de flux en place. Dans le cas de SQL Server, le regroupement de connexions limite efficacement le nombre d’envois passe à tout moment.

Par défaut, qu’un seul flux est utilisé pour SQL Server et Redis, cinq flux sont utilisés pour Service Bus et file d’attente est désactivé, mais ces paramètres peuvent être modifiés par la configuration sur SQL Server et Service Bus :

**Code de serveur .NET pour la configuration de longueur de nombre et de la file d’attente de la table de fond de panier de SQL Server**

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

**Code de serveur .NET pour la configuration de longueur de nombre et de la file d’attente de rubrique pour Service Bus de fond de panier**

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

Un **mise en mémoire tampon** stream est celui qui a entré un état d’erreur ; lorsque le flux est dans l’état par défaut, tous les messages envoyés au fond de panier échouera immédiatement jusqu'à ce que le flux est défaillante n’est plus. Le **longueur de file d’attente d’envoi** est le nombre de messages qui ont été validées mais pas encore été envoyées.

- **Bus de messages de montée en puissance parallèle des Messages reçus/s**
- **Total des flux de montée en puissance parallèle**
- **Montée en puissance parallèle diffuse Open**
- **Mise en mémoire tampon de flux de montée en puissance parallèle**
- **Total des erreurs de montée en puissance parallèle**
- **Erreurs de montée en puissance parallèle par seconde**
- **Longueur de file d’attente d’envoi de montée en puissance parallèle**

Pour plus d’informations sur la mesurant des ces compteurs, consultez [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>À l’aide d’autres compteurs de performances

Les compteurs de performances suivants peuvent également être utiles pour surveiller les performances de votre application.

**Mémoire**

- Mémoire CLR .NET\\Nb. d’octets dans tous les segments de mémoire (pour w3wp)

**ASP.NET**

- ASP. net\requêtes en cours
- ASP.NET\Queued
- ASP.NET\Rejected

**Processeur**

- Temps de processeur Information\Processor

**TCP/IP**

- TCPv6/connexions établies
- TCPv4/connexions établies

**Service Web**

- Connexions Web service web\connexions en cours
- Connexions de Service\Maximum Web

**Thread**

- .NET CLR de verrous et Threads\\# de Threads actuels logiques
- .NET CLR de verrous et Threads\\# de Threads actuels physiques

<a id="otherresources"></a>

## <a name="other-resources"></a>Autres ressources

Pour plus d’informations sur la surveillance et réglage des performances de ASP.NET, consultez les rubriques suivantes :

- [Vue d’ensemble des performances ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)
- [Utilisation de Thread ASP.NET sur IIS 7.5, IIS 7.0 et IIS 6.0](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;applicationPool&gt; , élément (paramètres Web)](https://msdn.microsoft.com/library/dd560842.aspx)
