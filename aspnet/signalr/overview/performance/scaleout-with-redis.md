---
uid: signalr/overview/performance/scaleout-with-redis
title: Montée en puissance parallèle de SignalR avec Redis | Microsoft Docs
author: MikeWasson
description: Versions des logiciels utilisés dans cette rubrique Visual Studio 2013, .NET 4.5 SignalR les versions précédentes de la version 2 de cette rubrique pour plus d’informations sur les versions antérieures de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: e08e0fdcd0df65bea6802838a8ed55d6b86ce1a3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381953"
---
<a name="signalr-scaleout-with-redis"></a>Montée en puissance parallèle de SignalR avec Redis
====================
par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

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


Dans ce didacticiel, vous allez utiliser [Redis](http://redis.io/) pour distribuer les messages sur une application de SignalR est déployée sur deux instances distinctes d’IIS.

Redis est un magasin de clé-valeur en mémoire. Il prend également en charge un système de messagerie avec un modèle de publication/abonnement. Le fond de panier SignalR Redis utilise la fonctionnalité de pub/sub pour transférer des messages vers d’autres serveurs.

![](scaleout-with-redis/_static/image1.png)

Pour ce didacticiel, vous allez utiliser les trois serveurs :

- Deux serveurs exécutant Windows, que vous utiliserez pour déployer une application de SignalR.
- Un serveur exécutant Linux, que vous utiliserez pour exécuter Redis. Pour les captures d’écran dans ce didacticiel, j’ai utilisé Ubuntu 12.04 TLS.

Si vous n’avez trois serveurs physiques à utiliser, vous pouvez créer des machines virtuelles sur Hyper-V. Une autre option consiste à créer des machines virtuelles sur Azure.

Bien que ce didacticiel utilise l’implémentation de Redis officielle, il existe également un [Windows port de Redis](https://github.com/MSOpenTech/redis) à partir de MSOpenTech. Le programme d’installation et de configuration sont différentes, mais sinon les étapes sont identiques.

> [!NOTE] 
> 
> Montée en puissance parallèle de SignalR avec Redis ne prend pas en charge les clusters Redis.


## <a name="overview"></a>Vue d'ensemble

Avant de passer au didacticiel détaillé, Voici un aperçu rapide de la procédure à suivre.

1. Installez Redis et démarrer le serveur Redis.
2. Ajoutez ces packages NuGet à votre application : 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.Redis](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. Créez une application de SignalR.
4. Ajoutez le code suivant à Startup.cs pour configurer le fond de panier : 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>Ubuntu sur Hyper-V

À l’aide de Windows Hyper-V, vous pouvez facilement créer une VM Ubuntu sur Windows Server.

Téléchargez le fichier ISO Ubuntu de [ http://www.ubuntu.com ](http://www.ubuntu.com/).

Dans Hyper-V, ajoutez une nouvelle machine virtuelle. Dans le **connecter un disque dur virtuel** étape, sélectionnez **créer un disque dur virtuel**.

![](scaleout-with-redis/_static/image2.png)

Dans le **Options d’Installation** étape, sélectionnez **fichier Image (.iso)**, cliquez sur **Parcourir**, puis accédez à l’installation d’Ubuntu ISO.

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>Installez Redis

Suivez les étapes de [ http://redis.io/download ](http://redis.io/download) pour télécharger et générer Redis.

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

Cette opération génère les fichiers binaires de Redis le `src` directory.

Par défaut, Redis ne nécessite pas un mot de passe. Pour définir un mot de passe, modifier le `redis.conf` fichier, ce qui se trouve dans le répertoire racine du code source. (Faites une copie de sauvegarde du fichier avant de le modifier !) Ajoutez la directive suivante à `redis.conf`:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

Démarrer maintenant le serveur Redis :

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

Ouvrir le port 6379, qui est le port par défaut Redis écoute. (Vous pouvez modifier le numéro de port dans le fichier de configuration.)

## <a name="create-the-signalr-application"></a>Créer l’Application de SignalR

Créez une application de SignalR en suivant une de ces didacticiels :

- [Bien démarrer avec SignalR 2.0](../getting-started/tutorial-getting-started-with-signalr.md)
- [Bien démarrer avec SignalR 2.0 et MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

Ensuite, nous allons modifier l’application de conversation pour prendre en charge la montée en puissance parallèle avec Redis. Tout d’abord, ajoutez le package NuGet de SignalR.Redis à votre projet. Dans Visual Studio, à partir de la **outils** menu, sélectionnez **Library Package Manager**, puis sélectionnez **Console du Gestionnaire de Package**. Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

Ensuite, ouvrez le fichier Startup.cs. Ajoutez le code suivant à la **Configuration** méthode :

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- « serveur » est le nom du serveur qui est en cours d’exécution Redis.
- *port* est le numéro de port
- « password » est le mot de passe que vous avez défini dans le fichier redis.conf.
- « AppName » est n’importe quelle chaîne. SignalR crée un canal de pub/sub Redis portant ce nom.

Exemple :

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>Déployer et exécuter l’Application

Préparez vos instances de Windows Server pour déployer l’application de SignalR.

Ajouter le rôle IIS. Inclut des fonctionnalités de « Développement d’applications », y compris le protocole WebSocket.

![](scaleout-with-redis/_static/image5.png)

Incluent également le Service de gestion (répertorié sous « Outils de gestion »).

![](scaleout-with-redis/_static/image6.png)

**Installer Web Deploy 3.0.** Lorsque vous exécutez le Gestionnaire des services Internet, il vous invite à installer Microsoft Web Platform, ou vous pouvez [télécharger l’intstaller](https://go.microsoft.com/fwlink/?LinkId=255386). Dans le programme d’installation de la plateforme, recherchez de Web Deploy et installer Web Deploy 3.0

![](scaleout-with-redis/_static/image7.png)

Vérifiez que le Service de gestion Web est en cours d’exécution. Si ce n’est pas le cas, démarrez le service. (Si vous ne voyez pas Service de gestion Web dans la liste des services de Windows, assurez-vous que vous avez installé le Service de gestion lorsque vous avez ajouté le rôle IIS.)

Par défaut, le Service de gestion Web écoute sur le port TCP 8172. Dans le pare-feu Windows, créez une règle de trafic entrant pour autoriser le trafic TCP sur le port 8172. Pour plus d’informations, consultez [configuration des règles de pare-feu](https://technet.microsoft.com/library/dd448559(WS.10).aspx). (Si vous hébergez les machines virtuelles sur Azure, vous pouvez le faire directement dans le portail Azure. Consultez [procédure pour configurer des points de terminaison sur une Machine virtuelle](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)

Vous êtes maintenant prêt à déployer le projet de Visual Studio à partir de votre ordinateur de développement sur le serveur. Dans l’Explorateur de solutions, avec le bouton droit de la solution, puis cliquez sur **publier**.

Pour plus de documentation sur le déploiement web, consultez [plan de contenu de déploiement Web pour Visual Studio et ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).

Si vous déployez l’application à deux serveurs, vous pouvez ouvrir chaque instance dans une fenêtre de navigateur distincte et voir qu’ils reçoivent des messages SignalR à partir de l’autre. (Bien sûr, dans un environnement de production, les deux serveurs passait souvent derrière un équilibreur de charge.)

![](scaleout-with-redis/_static/image8.png)

Si vous êtes impatient de voir les messages qui sont envoyés avec Redis, vous pouvez utiliser la **redis-cli** client, qui s’installe avec Redis.

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
