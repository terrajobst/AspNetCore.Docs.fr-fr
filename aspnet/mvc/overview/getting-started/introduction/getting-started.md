---
uid: mvc/overview/getting-started/introduction/getting-started
title: Bien démarrer avec ASP.NET MVC 5 | Microsoft Docs
author: Rick-Anderson
description: 'Remarque : Une version mise à jour de ce didacticiel est disponible ici à l’aide de Visual Studio 2015. Le nouveau didacticiel utilise ASP.NET Core MVC 6, qui fournit de nombreuses improvem...'
ms.author: riande
ms.date: 05/28/2015
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 8417be945e16ed99e655f628134c9190429f1d67
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837727"
---
<a name="getting-started-with-aspnet-mvc-5"></a>Bien démarrer avec ASP.NET MVC 5
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 Ce didacticiel vous apprend les notions de base de la création d’une application web ASP.NET MVC 5 avec [Visual Studio 2017](https://www.visualstudio.com/). Dernière Source pour le didacticiel situé sur [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)


 Ce didacticiel a été écrit par [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter : [ @shanselman ](https://twitter.com/shanselman) ) , et [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )

 Vous avez besoin d’un compte Azure pour déployer cette application dans Azure :

 - Vous pouvez [ouvrir un compte Azure gratuit](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -vous bénéficiez de crédits que vous pouvez utiliser pour tester les services Azure payants et même lorsqu’ils sont épuisés, vous pouvez conserver le compte et utiliser les services Azure gratuits.
 - Vous pouvez [activer les avantages d’abonnement MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -votre abonnement MSDN vous donne des crédits chaque mois que vous pouvez utiliser pour les services Azure payants.


## <a name="getting-started"></a>Prise en main

Démarrez en installant et en cours d’exécution [Visual Studio 2017](https://www.visualstudio.com/).

Visual Studio est un environnement de développement intégré ou IDE. Tout comme vous utilisez Microsoft Word pour écrire des documents, vous utiliserez un IDE pour créer des applications. Dans Visual Studio, il existe une liste dans la partie inférieure affiche les différentes options disponibles pour vous. Il existe également un menu qui fournit une autre façon d’effectuer des tâches dans l’IDE. (Par exemple, au lieu de sélectionner **nouveau projet** à partir de la **Démarrer** page, vous pouvez utiliser le menu et sélectionnez **fichier** &gt; **denouveauprojet**.)


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a>Créer votre première Application

Cliquez sur **nouveau projet**, puis sélectionnez Visual c# sur la gauche, puis **Web** , puis sélectionnez **Application Web ASP.NET (.NET Framework)**. Nommez votre projet « MvcMovie », puis **OK**.

![](getting-started/_static/image2.png)

Dans le **nouveau projet ASP.NET** boîte de dialogue, cliquez sur **MVC** puis cliquez sur **OK**.

![](getting-started/_static/image3.png)

Visual Studio a utilisé un modèle par défaut pour le projet ASP.NET MVC que vous venez de créer, afin que vous ayez une application opérationnelle sans rien faire dès maintenant ! Il s’agit d’un simple « Hello World ! » projet et il l’un bon point de départ de votre application.

![](getting-started/_static/image4.png)

Cliquez sur F5 pour démarrer le débogage. F5 provoque Visual Studio Démarrer [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) et exécuter votre application web. Ensuite, Visual Studio lance un navigateur et ouvre la page d’accueil de l’application. Notez que la barre d’adresses du navigateur indique `localhost:port#` et non quelque chose comme `example.com`. C’est parce que `localhost` pointe toujours vers votre ordinateur local, qui dans ce cas s’exécute l’application que vous venez de créer. Lorsque Visual Studio s’exécute à un projet web, un port aléatoire est utilisé pour le serveur web. Dans l’image ci-dessous, le numéro de port est 1234. Lorsque vous exécutez l’application, vous verrez un numéro de port différent.

![](getting-started/_static/image5.png)

Dès ce modèle par défaut vous donne les pages Accueil, Contact et sur. L’image ci-dessus n’affiche pas le **accueil**, **sur** et **Contact** des liens. Selon la taille de la fenêtre du navigateur, vous devrez peut-être cliquer sur l’icône de navigation pour afficher ces liens.

![](getting-started/_static/image6.png)  

L’application fournit également la prise en charge pour vous inscrire et connectez-vous. L’étape suivante consiste à modifier le fonctionnement de cette application et en savoir un peu sur ASP.NET MVC. Fermez l’application ASP.NET MVC et nous allons modifier du code.

Pour obtenir la liste de didacticiels actuels, consultez [MVC recommandé articles](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Consultez cette application en cours d’exécution sur Azure

Vous souhaitez voir le site terminé en cours d’exécution en tant qu’une application web en direct ? Vous pouvez déployer une version complète de l’application à votre compte Azure en cliquant simplement sur le bouton suivant.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Vous avez besoin d’un compte Azure pour déployer cette solution sur Azure. Si vous n’avez pas déjà un compte, vous disposez des options suivantes :

- [Ouvrir un compte Azure gratuit](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -vous bénéficiez de crédits que vous pouvez utiliser pour tester les services Azure payants et même lorsqu’ils sont épuisés, vous pouvez conserver le compte et utiliser les services Azure gratuits.
- [Activer les avantages d’abonnement MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -votre abonnement MSDN vous donne des crédits chaque mois que vous pouvez utiliser pour les services Azure payants.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
