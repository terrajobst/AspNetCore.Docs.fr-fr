---
uid: mvc/overview/getting-started/introduction/getting-started
title: Bien démarrer avec ASP.NET MVC 5 | Microsoft Docs
author: Rick-Anderson
ms.author: riande
ms.date: 10/04/2018
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 2cc9364b815cae0207fc59784303c6a0906f1b94
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578443"
---
<a name="getting-started-with-aspnet-mvc-5"></a>Mise en route avec ASP.NET MVC 5
====================
par [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [consider RP](../../../../includes/razor.md)]

Ce didacticiel vous enseigne les principes fondamentaux de la création d’une application web ASP.NET MVC 5 avec [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Le code source finale pour le didacticiel se trouve sur [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie).

Ce didacticiel a été écrit par [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter : [ @shanselman ](https://twitter.com/shanselman) ) , et [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )

Vous avez besoin d’un compte Azure pour déployer cette application dans Azure :

- Vous pouvez [ouvrir un compte Azure gratuit](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -vous bénéficiez de crédits que vous pouvez utiliser pour tester les services Azure payants et même lorsqu’ils sont épuisés, vous pouvez conserver le compte et utiliser les services Azure gratuits.
- Vous pouvez [activer les avantages d’abonnement MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -votre abonnement MSDN vous donne des crédits chaque mois que vous pouvez utiliser pour les services Azure payants.

## <a name="get-started"></a>Prise en main

Commencez par [l’installation de Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Ensuite, ouvrez Visual Studio.

Visual Studio est un environnement de développement intégré ou IDE. Tout comme vous utilisez Microsoft Word pour écrire des documents, vous utiliserez un IDE pour créer des applications. Dans Visual Studio, il existe une liste dans la partie inférieure affiche les différentes options disponibles pour vous. Il existe également un menu qui fournit une autre façon d’effectuer des tâches dans l’IDE. Par exemple, au lieu de sélectionner **nouveau projet** sur le **page de démarrage**, vous pouvez utiliser la barre de menus et sélectionnez **fichier** > **denouveauprojet**.

![](getting-started/_static/image1.png)

## <a name="create-your-first-app"></a>Créer votre première application

Sur le **page de démarrage**, sélectionnez **nouveau projet**. Dans le **nouveau projet** boîte de dialogue, sélectionnez le **Visual C#** catégorie sur la gauche, puis **Web**, puis sélectionnez le **Application Web ASP.NET (.NET Framework)**  modèle de projet. Nommez votre projet « MvcMovie », puis choisissez **OK**.

![](getting-started/_static/image2.png)

Dans le **nouvelle Application Web ASP.NET** boîte de dialogue, choisissez **MVC** , puis **OK**.

![](getting-started/_static/image3.png)

Visual Studio a utilisé un modèle par défaut pour le projet ASP.NET MVC que vous venez de créer, afin que vous ayez une application opérationnelle sans rien faire dès maintenant ! Il s’agit d’un simple « Hello World ! » projet et il l’un bon point de départ de votre application.

![](getting-started/_static/image4.png)

Appuyez sur **F5** pour démarrer le débogage. Quand vous appuyez sur **F5**, Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute votre application web. Ensuite, Visual Studio lance un navigateur et ouvre la page d’accueil de l’application. Notez que la barre d’adresses du navigateur indique `localhost:port#` et non quelque chose comme `example.com`. C’est parce que `localhost` pointe toujours vers votre ordinateur local, qui dans ce cas s’exécute l’application que vous venez de créer. Lorsque Visual Studio s’exécute à un projet web, un port aléatoire est utilisé pour le serveur web. Dans l’image ci-dessous, le numéro de port est 1234. Lorsque vous exécutez l’application, vous verrez un numéro de port différent.

![](getting-started/_static/image5.png)

Dès ce modèle par défaut vous donne `Home`, `Contact`, et `About` pages. L’image ci-dessus n’affiche pas le **accueil**, **sur**, et **Contact** des liens. Selon la taille de la fenêtre du navigateur, vous devrez peut-être cliquer sur l’icône de navigation pour afficher ces liens.

![](getting-started/_static/image6.png)

L’application fournit également la prise en charge pour vous inscrire et connectez-vous. L’étape suivante consiste à modifier le fonctionnement de cette application et en savoir un peu sur ASP.NET MVC. Fermez l’application ASP.NET MVC et nous allons modifier du code.

Pour obtenir la liste de didacticiels actuels, consultez [MVC recommandé articles](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Consultez cette application en cours d’exécution sur Azure

Vous souhaitez voir le site terminé en cours d’exécution en tant qu’une application web en direct ? Vous pouvez déployer une version complète de l’application à votre compte Azure en cliquant simplement sur le bouton suivant.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Vous avez besoin d’un compte Azure pour déployer cette solution sur Azure. Si vous ne disposez pas d’un compte, utilisez une des options suivantes pour en créer un :

- [Ouvrir un compte Azure gratuit](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -vous bénéficiez de crédits que vous pouvez utiliser pour tester les services Azure payants et même lorsqu’ils sont épuisés, vous pouvez conserver le compte et utiliser les services Azure gratuits.
- [Activer les avantages d’abonnement Visual Studio](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers) -votre abonnement Visual Studio vous donne des crédits chaque mois que vous pouvez utiliser pour les services Azure payants.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
