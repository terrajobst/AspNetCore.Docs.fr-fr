---
uid: signalr/overview/getting-started/supported-platforms
title: Plateformes prises en charge | Microsoft Docs
author: pfletcher
description: Cet article décrit les clients et les serveurs sont pris en charge par SignalR.
ms.author: aspnetcontent
ms.date: 04/18/2018
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: 5d77db71c5c6b0c297756921b5b7cb79add03998
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805809"
---
<a name="supported-platforms"></a>Plateformes prises en charge
====================
par [Patrick Fletcher](https://github.com/pfletcher)

> Cet article décrit les clients et les serveurs sont pris en charge par SignalR. 
> 
> ## <a name="questions-and-comments"></a>Questions et commentaires
> 
> Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer dans les commentaires en bas de la page. Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).


SignalR est pris en charge dans diverses configurations de serveurs et clients. En outre, chaque option de transport possède un ensemble d’exigences de son propre ; Si la configuration système requise pour un transport n’est pas disponible, SignalR basculent normalement d’autres transports. Pour plus d’informations sur les transports prenant en charge SignalR, consultez [Transports et les solutions de secours](introduction-to-signalr.md#transports).

## <a name="server-system-requirements"></a>Configuration requise du système serveur

Le composant de serveur SignalR peut être hébergé sur une variété de configurations de serveur. Cette section décrit les versions prises en charge de systèmes d’exploitation, .NET framework, Internet Information Server et autres composants.

### <a name="supported-server-operating-systems"></a>Systèmes d'exploitation serveurs pris en charge

Le composant de serveur SignalR peut être hébergé dans les systèmes d’exploitation serveur ou client suivants. Notez que, pour SignalR à utiliser les WebSockets, Windows Server 2012, Windows Server 2016 ou Windows 8 est obligatoire (WebSocket peut être utilisé sur les Sites Web Windows Azure, tant que la version du .NET framework du site a vers la version 4.5, et Web Sockets est activée dans le site Page de configuration).

- Windows Server 2016
- Windows Server 2012
- Windows Server 2008 r2
- Windows 10
- Windows 8
- Windows 7
- Windows Azure

### <a name="supported-server-net-framework-version"></a>Version du .NET Framework serveur pris en charge

SignalR 2 est uniquement pris en charge sur .NET Framework 4.5. Consultez le [mises à jour recommandées](#updates) section des mises à jour qui améliorent la fiabilité, de compatibilité, de stabilité et de performances.

### <a name="supported-server-iis-versions"></a>Versions d’IIS de serveur pris en charge

Lorsque SignalR est hébergé dans IIS, les versions suivantes sont prises en charge. Notez que si un système d’exploitation de client est utilisé, comme pour le développement (Windows 8 ou Windows 7), les versions complètes des services IIS ou Cassini ne doivent pas être utilisées, dans la mesure où il y aura une limite de 10 connexions simultanées imposée, qui sera atteinte très rapidement car les connexions sont temporaires, fréquemment rétablie et sont pas supprimés immédiatement sur les plus utilisés. IIS Express doit être utilisé sur les systèmes d’exploitation clients.

Notez également que pour SignalR à utiliser WebSocket, IIS 8 ou IIS 8 Express doit être utilisé, le serveur doit être à l’aide de Windows 8, Windows Server 2012 ou version ultérieure, et WebSocket doit être activée dans IIS. Pour plus d’informations sur l’activation de WebSocket dans IIS, consultez [prise en charge du protocole WebSocket IIS 8.0](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).

- IIS 8 ou IIS 8 Express.
- IIS 7 et 7.5. Prise en charge de [URL sans extension](https://support.microsoft.com/kb/980368) est requis.
- IIS doit être en cours d’exécution en mode intégré ; en mode classique n’est pas pris en charge. Des retards de message de trente secondes peuvent s’afficher si IIS est exécuté en mode classique en utilisant le transport les événements.
- L’application d’hébergement doit être en cours d’exécution en mode confiance totale.

## <a name="client-system-requirements"></a>Configuration requise du client

SignalR peut être utilisé dans un large éventail de plateformes clientes. Cette section décrit la configuration système requise pour l’utilisation de SignalR dans les navigateurs web, les applications de bureau Windows, les applications Silverlight et les appareils mobiles.

### <a name="web-browsers"></a>Navigateurs Web

SignalR peut être utilisé dans un large éventail de navigateurs web, mais en règle générale, seuls les deux dernières versions sont prises en charge.

Les applications qui utilisent SignalR dans les navigateurs doivent utiliser jQuery 1.6.4 ou une version ultérieure majeure (par exemple, 1.7.2, 1.8.2 ou 1.9.1).

SignalR peut être utilisé dans les navigateurs suivants :

- Versions de Microsoft Internet Explorer 8, 9, 10 et 11. Moderne, bureau et les versions mobiles sont pris en charge.
- Mozilla Firefox : version actuelle - 1, versions de Windows et Mac.
- Google Chrome : version actuelle - 1, versions de Windows et Mac.
- Safari : version actuelle - 1, versions de Mac et iOS.
- Opera : version actuelle - 1, Windows uniquement.
- Navigateur Android

En plus de nécessiter certains navigateurs, les différents transports que SignalR utilise ont des exigences de leurs propres. Les transports suivants sont pris en charge les configurations suivantes :

<a id="browser"></a>

**Configuration requise de Transport du navigateur Web**

| Transport | Internet Explorer | Chrome (Windows ou iOS) | Firefox | Safari (OS x ou iOS) | Android |
| --- | --- | --- | --- | --- | --- |
| WebSockets | 10+ | en cours - 1 | en cours - 1 | en cours - 1 | N/A |
| Événements de serveur a été envoyé | N/A | en cours - 1 | en cours - 1 | en cours - 1 | N/A |
| ForeverFrame | 8+ | N/A | N/A | N/A | 4.1 |
| Interrogation longue | 8+ | en cours - 1 | en cours - 1 | en cours - 1 | 4.1 |

\*: 6 + requis pour toutes les fonctionnalités.

#### <a name="unsupported-browsers"></a>Navigateurs non pris en charge

Tandis que SignalR *peut* s’exécutent sans problèmes majeurs dans les versions antérieures du navigateur, nous ne testez pas activement SignalR dans les et généralement ne pas corriger les bogues qui peuvent apparaître dans les.

### <a name="windows-desktop-and-silverlight-applications"></a>Windows Applications de bureau et Silverlight

Outre l’exécution dans un navigateur web, SignalR peut être hébergé dans les applications Silverlight ou de client de Windows autonome. Les applications Windows Desktop et de Silverlight SignalR présentent les exigences système suivantes.

- Applications à l’aide de .NET 4 sont pris en charge sur Windows XP SP3 ou version ultérieure.
- Applications à l’aide de .NET Framework 4.5 sont pris en charge sur Windows Vista ou version ultérieure.

Outre le système d’exploitation et la configuration requise du .NET framework, les transports disponibles pour SignalR ont des exigences de leurs propres. Les transports suivants sont pris en charge les configurations suivantes :

**Bureau de Windows en matière de Transport de Silverlight**

| Transport | Application .NET | Silverlight |
| --- | --- | --- |
| Web Sockets | Windows 8 + et .NET 4.5 + | N/A |
| Forever Frame | N/A | N/A |
| Événements de serveur a été envoyé | .NET 4 + | 5+ |
| Interrogation longue | .NET 4 + | 5+ |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a>Windows Store et les Applications Windows Phone

SignalR peut être utilisé dans les applications du Windows Store et les applications Windows Phone 8. Les transports suivants sont pris en charge les configurations suivantes :

**Windows Store et les exigences de Transport de Windows Phone**

| Transport | Windows Store / .NET | Windows Store / JavaScript | Windows Phone / Internet Explorer | Windows Phone / .NET |
| --- | --- | --- | --- | --- |
| WebSockets | N/A | Win8 + | 8+ | N/A |
| Forever Frame | N/A | Win8 + | 7.5+ | N/A |
| Événements de serveur a été envoyé | Win8 + | N/A | N/A | 8+ |
| Interrogation longue | Win8 + | Win8 + | 7.5+ | 8+ |

<a id="updates"></a>

## <a name="recommended-updates"></a>Mises à jour recommandées

Les mises à jour suivantes sont recommandées pour les serveurs de SignalR :

- Une mise à jour pour .NET Framework 4.5 est disponible [ici](https://support.microsoft.com/kb/2750149).
- Microsoft publie régulièrement des correctifs QFE pour ASP.NET. Elles doivent être appliquées comme étant disponibles.
