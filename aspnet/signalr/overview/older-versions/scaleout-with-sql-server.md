---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: Montée en puissance parallèle de SignalR avec SQL Server (SignalR 1.x) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 05/01/2013
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 682aa837ed991cbf5d78dcb304e2c1bce905c52c
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287530"
---
<a name="signalr-scaleout-with-sql-server-signalr-1x"></a>Montée en puissance parallèle de SignalR avec SQL Server (SignalR 1.x)
====================
par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

Dans ce didacticiel, vous allez utiliser SQL Server pour distribuer les messages sur une application de SignalR est déployée dans deux instances distinctes d’IIS. Vous pouvez également exécuter ce didacticiel sur un ordinateur de test unique, mais pour obtenir l’effet, vous devez déployer l’application de SignalR à deux ou plusieurs serveurs. Vous devez également installer SQL Server sur l’un des serveurs ou sur un serveur dédié distinct. Une autre option consiste à exécuter le didacticiel à l’aide de machines virtuelles sur Azure.

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a>Prérequis

Microsoft SQL Server 2005 ou version ultérieure. Le fond de panier prend en charge les éditions de bureau et serveur de SQL Server. Il ne prend pas en charge SQL Server Compact Edition ou la base de données SQL Azure. (Si votre application est hébergée sur Azure, envisagez le fond de panier de Service Bus à la place.)

## <a name="overview"></a>Vue d'ensemble

Avant de passer au didacticiel détaillé, Voici un aperçu rapide de la procédure à suivre.

1. Créer une nouvelle base de données vide. Le fond de panier créera les tables nécessaires dans cette base de données.
2. Ajoutez ces packages NuGet à votre application : 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.SqlServer](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. Créez une application de SignalR.
4. Ajoutez le code suivant à global.asax de façon à configurer le fond de panier : 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a>Configurer la base de données

Décider si l’application utilisera l’authentification Windows ou l’authentification SQL Server pour accéder à la base de données. Tous les cas, assurez-vous que l’utilisateur de base de données dispose des autorisations pour vous connecter, créer des schémas et créer des tables.

Créer une base de données pour le fond de panier à utiliser. Vous pouvez donner à la base de données n’importe quel nom. Vous n’avez pas besoin de créer des tables dans la base de données ; le fond de panier créera les tables nécessaires.

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a>Activer Service Broker

Il est recommandé d’activer Service Broker pour la base de données de fond de panier. Service Broker fournit une prise en charge native pour la messagerie et files d’attente dans SQL Server, ce qui permet de recevoir des mises à jour plus efficacement le fond de panier. (Toutefois, le fond de panier fonctionne également sans Service Broker.)

Pour vérifier si le Service Broker est activée, interrogez la **est\_broker\_activé** colonne dans le **sys.databases** vue de catalogue.

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

Pour activer Service Broker, utilisez la requête SQL suivante :

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> Si cette requête s’affiche en interblocage, vérifiez qu’il n’existe aucune application connectée à la base de données.

Si vous avez activé le suivi, les traces montrera également si Service Broker est activé.

## <a name="create-a-signalr-application"></a>Créer une Application de SignalR

Créez une application de SignalR en suivant une de ces didacticiels :

- [Bien démarrer avec SignalR](../getting-started/tutorial-getting-started-with-signalr.md)
- [Bien démarrer avec SignalR et MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md)

Ensuite, nous allons modifier l’application de conversation pour prendre en charge la montée en puissance parallèle avec SQL Server. Tout d’abord, ajoutez le package NuGet de SignalR.SqlServer à votre projet. Dans Visual Studio, à partir de la **outils** menu, sélectionnez **Gestionnaire de Package NuGet**, puis sélectionnez **Console du Gestionnaire de Package**. Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

Ensuite, ouvrez le fichier Global.asax. Ajoutez le code suivant à la **Application\_Démarrer** méthode :

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a>Déployer et exécuter l’Application

Préparez vos instances de Windows Server pour déployer l’application de SignalR.

Ajouter le rôle IIS. Inclut des fonctionnalités de « Développement d’applications », y compris le protocole WebSocket.

![](scaleout-with-sql-server/_static/image4.png)

Incluent également le Service de gestion (répertorié sous « Outils de gestion »).

![](scaleout-with-sql-server/_static/image5.png)

**Installer Web Deploy 3.0.** Lorsque vous exécutez le Gestionnaire des services Internet, il vous invite à installer Microsoft Web Platform, ou vous pouvez [télécharger l’intstaller](https://go.microsoft.com/fwlink/?LinkId=255386). Dans le programme d’installation de la plateforme, recherchez de Web Deploy et installer Web Deploy 3.0

![](scaleout-with-sql-server/_static/image6.png)

Vérifiez que le Service de gestion Web est en cours d’exécution. Si ce n’est pas le cas, démarrez le service. (Si vous ne voyez pas Service de gestion Web dans la liste des services de Windows, assurez-vous que vous avez installé le Service de gestion lorsque vous avez ajouté le rôle IIS.)

Enfin, ouvrir le port 8172 pour TCP. C’est le port qui utilise l’outil Web Deploy.

Vous êtes maintenant prêt à déployer le projet de Visual Studio à partir de votre ordinateur de développement sur le serveur. Dans l’Explorateur de solutions, avec le bouton droit de la solution, puis cliquez sur **publier**.

Pour plus de documentation sur le déploiement web, consultez [plan de contenu de déploiement Web pour Visual Studio et ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).

Si vous déployez l’application à deux serveurs, vous pouvez ouvrir chaque instance dans une fenêtre de navigateur distincte et voir qu’ils reçoivent des messages SignalR à partir de l’autre. (Bien sûr, dans un environnement de production, les deux serveurs passait souvent derrière un équilibreur de charge.)

![](scaleout-with-sql-server/_static/image7.png)

Après avoir exécuté l’application, vous pouvez voir que SignalR a automatiquement créé des tables dans la base de données :

![](scaleout-with-sql-server/_static/image8.png)

SignalR gère les tables. Tant que votre application est déployée, ne pas supprimer des lignes, modifier la table et ainsi de suite.
