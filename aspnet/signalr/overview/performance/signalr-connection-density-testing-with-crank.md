---
uid: signalr/overview/performance/signalr-connection-density-testing-with-crank
title: La densité des connexions SignalR avec Crank de test | Microsoft Docs
author: Rick-Anderson
description: Densité des connexions SignalR avec Crank de test
ms.author: riande
ms.date: 02/22/2015
ms.assetid: 148d9ca7-1af1-44b6-a9fb-91e261b9b463
msc.legacyurl: /signalr/overview/performance/signalr-connection-density-testing-with-crank
msc.type: authoredcontent
ms.openlocfilehash: 556accb1bcc18e9e4d1f813a87fc6f4b67bda088
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021480"
---
<a name="signalr-connection-density-testing-with-crank"></a>Densité des connexions SignalR avec Crank de test
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article décrit comment utiliser l’outil de manivelle pour tester une application avec plusieurs clients simulés.


Une fois que votre application s’exécute dans son environnement d’hébergement (Azure un rôle, IIS, web ou auto-hébergé à l’aide d’Owin), vous pouvez tester la réponse de l’application à un niveau élevé de la densité des connexions à l’aide de l’outil manivelle. L’environnement d’hébergement peut être un serveur Internet Information Services (IIS), un hôte Owin ou un rôle web Azure. (Remarque : les compteurs de performances ne sont pas disponibles sur Azure App Service Web Apps, donc vous ne serez pas en mesure d’obtenir des données de performances à partir d’un test de densité de connexion.)

Densité de connexion fait référence au nombre de connexions TCP simultanées qui peuvent être établies sur un serveur. Chaque connexion TCP entraîne une surcharge de sa propre et ouverture d’un grand nombre de connexions inactives parviendrez à créer un goulot d’étranglement de mémoire.

[SignalR codebase](https://github.com/signalr/signalr) inclut un outil de test de charge appelé **Crank**. Vous trouverez la dernière version de manivelle dans [la branche Dev](https://github.com/SignalR/signalr/tree/dev) sur GitHub. Vous pouvez télécharger un fichier Zip archive de la branche Dev de SignalR codebase [ici](https://github.com/SignalR/SignalR/archive/dev.zip).

Crank peut servir à entièrement saturer la mémoire du serveur afin de calculer le nombre total de connexions inactives que possibles sur le matériel serveur. Ou bien, vous pouvez également utiliser manivelle pour le test de charge du serveur sous une certaine quantité de pression de mémoire, en identifiant les connexions jusqu'à ce qu’un nombre spécifique ou un seuil de mémoire spécifique est atteint.

Lorsque vous testez, il est important d’utiliser à distance ou les clients afin d’éviter toute concurrence pour les ressources (par exemple, les connexions TCP et la mémoire). Surveiller les clients pour vous assurer qu’ils n’atteignent pas les goulots d’étranglement qui peuvent empêcher le serveur d’atteindre sa capacité totale (processeur ou mémoire). Vous devrez peut-être augmenter le nombre de clients afin de charger entièrement le serveur.

### <a name="running-a-connection-density-test"></a>Exécution d’un Test de densité de connexion

Cette section décrit les étapes nécessaires pour exécuter un test de densité de connexion sur une application de SignalR.

1. Télécharger et générer les [codebase de la branche Dev de SignalR](https://github.com/SignalR/SignalR/archive/dev.zip). Dans une invite de commandes, accédez à &lt;répertoire du projet&gt;\src\Microsoft.AspNet.SignalR.Crank\bin\debug.
2. Déployer votre application dans son environnement d’hébergement prévue. Prenez note du point de terminaison par votre application ; par exemple, dans l’application créée dans le [didacticiel mise en route](../getting-started/tutorial-getting-started-with-signalr.md), le point de terminaison `http://<yourhost>:8080/signalr`.
3. Installer [les compteurs de performances de SignalR](signalr-performance.md#perfcounters) sur le serveur. Si votre application s’exécute sur Azure, consultez [à l’aide des compteurs de performances de SignalR dans un rôle Web Azure](using-signalr-performance-counters-in-an-azure-web-role.md).

Une fois que vous avez téléchargé et créé la base de code et installé les compteurs de performances sur votre ordinateur hôte, l’outil de ligne de commande manivelle se trouve dans le `src\Microsoft.AspNet.SignalR.Crank\bin\Debug` dossier.

Les options disponibles pour l’outil manivelle incluent :

- **/?** : Affiche l’écran d’aide. Les options disponibles sont également affichées si les **Url** paramètre est omis.
- **/ Url**: l’URL pour les connexions SignalR. Ce paramètre est obligatoire. Pour une application de SignalR en utilisant le mappage par défaut, le chemin d’accès se termine par « / signalr ».
- **/ Transport**: le nom du transport utilisé. La valeur par défaut est `auto`, qui sélectionne le meilleur protocole disponible. Les options incluent `WebSockets`, `ServerSentEvents`, et `LongPolling` (`ForeverFrame` n’est pas une option pour manivelle, depuis le client .NET au lieu d’Internet Explorer est utilisé). Pour plus d’informations sur la façon dont SignalR sélectionne les transports, consultez [Transports et les solutions de secours](../getting-started/introduction-to-signalr.md#transports).
- **/ BatchSize**: le nombre de clients ajoutés dans chaque lot. La valeur par défaut est 50.
- **/ ConnectInterval**: l’intervalle en millisecondes entre l’ajout de connexions. La valeur par défaut est 500.
- **/ Connexions**: le nombre de connexions utilisé pour l’application de test de charge. La valeur par défaut est 100 000.
- **/ ConnectTimeout**: le délai d’attente en secondes avant l’abandon du test. La valeur par défaut est 300.
- **MinServerMBytes**: les mégaoctets minimale du serveur à atteindre. La valeur par défaut est 500.
- **SendBytes**: la taille de la charge utile envoyée au serveur en octets. La valeur par défaut est 0.
- **SendInterval**: le délai en millisecondes entre les messages au serveur. La valeur par défaut est 500.
- **SendTimeout**: le délai d’expiration en millisecondes pour les messages au serveur. La valeur par défaut est 300.
- **ControllerUrl**: l’Url où un seul client hébergera un contrôleur central. La valeur par défaut est null (aucun hub de contrôleur). Le hub de contrôleur est lancé au démarrage de la session manivelle ; aucune autre contact entre le contrôleur et manivelle est fait.
- **NumClients**: le nombre de clients simulés pour vous connecter à l’application. La valeur par défaut est un.
- **Fichier journal**: le nom de fichier pour le fichier journal pour la série de tests. La valeur par défaut est `crank.csv`.
- **SampleInterval**: la durée en millisecondes entre les échantillons de compteurs de performances. La valeur par défaut est 1000.
- **SignalRInstance**: le nom d’instance des compteurs de performances sur le serveur. La valeur par défaut consiste à utiliser l’état de connexion du client.

### <a name="example"></a>Exemple

La commande suivante teste un site nommé `pfsignalr` sur Azure qui héberge une application sur le port 8080 avec un hub nommé « ControllerHub », à l’aide de 100 connexions.

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`
