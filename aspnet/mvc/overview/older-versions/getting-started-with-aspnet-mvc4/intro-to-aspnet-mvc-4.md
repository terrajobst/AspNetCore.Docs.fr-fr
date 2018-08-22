---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: Introduction à ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Une version mise à jour si ce didacticiel est disponible ici à l’aide de Visual Studio 2013. Le nouveau didacticiel utilise ASP.NET MVC 5, qui fournit de nombreuses améliorations de t...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 6442477c22c124f782642b770f9394873a988e85
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824920"
---
<a name="intro-to-aspnet-mvc-4"></a>Introduction à ASP.NET MVC 4
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

> Une version mise à jour si ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) à l’aide de [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Le nouveau didacticiel utilise ASP.NET MVC 5, qui fournit de nombreuses améliorations de ce didacticiel.
> 
> Ce didacticiel vous apprend les notions de base de la création d’une application Web ASP.NET MVC 4 à l’aide de Microsoft [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) ou Visual Web Developer 2010 Express Service Pack 1. Visual Studio 2012 est recommandée, vous n’aurez pas à installer quoi que ce soit pour suivre ce didacticiel. Si vous utilisez Visual Studio 2010, vous devez installer les composants ci-après. Vous pouvez installer tous les en cliquant sur les liens suivants :
> 
> - [Prérequis pour le Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Programme d’installation WPI pour ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [Base de données locale](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
> 
> Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez le [programme d’installation WPI pour ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) et : [configuration requise de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
> 
> Un projet de Visual Web Developer avec code source c# est disponible pour accompagner cette rubrique. [Téléchargez la version c#](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).
> 
> Dans le didacticiel, vous exécutez l’application dans Visual Studio. Vous pouvez également rendre l’application disponible sur Internet en la déployant sur un fournisseur d’hébergement. Microsoft propose d’hébergement web gratuit pour jusqu'à 10 sites web dans un [compte d’évaluation de Windows Azure gratuit](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Pour plus d’informations sur la façon de déployer un projet web Visual Studio sur un Site Web de Windows Azure, consultez [créer et déployer un site web ASP.NET et la base de données SQL avec Visual Studio](https://docs.microsoft.com/dotnet/azure/). Ce didacticiel montre également comment utiliser des Migrations Entity Framework Code First pour déployer votre base de données SQL Server sur Windows Azure SQL Database (anciennement SQL Azure).
> 
> Ce didacticiel a été rédigé par Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="what-youll-build"></a>Ce que vous allez générer

> [!NOTE]
> Une version mise à jour si ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) à l’aide de [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Le nouveau didacticiel utilise ASP.NET MVC 5, qui fournit de nombreuses améliorations de ce didacticiel.


Vous allez implémenter une application de liste de film simple qui prend en charge la création, la modification, la recherche et la liste de films à partir d’une base de données. Voici deux captures d’écran de l’application que vous allez générer. Il comprend une page qui affiche une liste de films à partir d’une base de données :

![](intro-to-aspnet-mvc-4/_static/image1.png)

L’application vous permet également ajouter, modifier et supprimer des films, mais aussi voir des détails sur les modifications individuelles. Tous les scénarios de saisie de données incluent la validation pour vous assurer que les données stockées dans la base de données sont correctes.

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>Prise en main

Commencez par exécuter Visual Studio Express 2012 ou Visual Web Developer 2010 Express. La plupart des captures d’écran de cette utilisation de la série Visual Studio Express 2012, mais vous pouvez suivre ce didacticiel avec Visual Studio 2010 SP1, de Visual Studio 2012, de Visual Studio Express 2012 ou de Visual Web Developer 2010 Express. Sélectionnez **nouveau projet** à partir de la **Démarrer** page.

Visual Studio est un environnement de développement intégré ou IDE. Tout comme vous utilisez Microsoft Word pour écrire des documents, vous utiliserez un IDE pour créer des applications. Dans Visual Studio, il existe une barre d’outils en haut montrant les différentes options disponibles pour vous. Il existe également un menu qui fournit une autre façon d’effectuer des tâches dans l’IDE. (Par exemple, au lieu de sélectionner **nouveau projet** à partir de la **Démarrer** page, vous pouvez utiliser le menu et sélectionnez **fichier** &gt; **denouveauprojet**.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>Créer votre première Application

Vous pouvez créer des applications à l’aide de Visual Basic ou Visual c# comme langage de programmation. Sélectionnez Visual c# sur la gauche, puis sélectionnez **ASP.NET MVC 4 Web Application**. Nommez votre projet &quot;MvcMovie&quot; puis cliquez sur **OK**.

![](intro-to-aspnet-mvc-4/_static/image4.png)

Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue, sélectionnez **Application Internet**. Laissez **Razor** en tant que le moteur d’affichage par défaut.

![](intro-to-aspnet-mvc-4/_static/image5.png)

Cliquez sur **OK**. Visual Studio a utilisé un modèle par défaut pour le projet ASP.NET MVC que vous venez de créer, afin que vous ayez une application opérationnelle sans rien faire dès maintenant ! Il s’agit d’une simple &quot;Hello World !&quot; projet et un bon point de départ de votre application.

![](intro-to-aspnet-mvc-4/_static/image6.png)

Dans le menu **Déboguer**, sélectionnez **Démarrer le débogage**.

![](intro-to-aspnet-mvc-4/_static/image7.png)

Notez que le raccourci clavier pour démarrer le débogage F5.

F5 provoque Visual Studio pour démarrer IIS Express et exécuter votre application web. Ensuite, Visual Studio lance un navigateur et ouvre la page d’accueil de l’application. Notez que la barre d’adresses du navigateur indique `localhost` et non quelque chose comme `example.com`. C’est parce que `localhost` pointe toujours vers votre ordinateur local, qui dans ce cas s’exécute l’application que vous venez de créer. Lorsque Visual Studio s’exécute à un projet web, un port aléatoire est utilisé pour le serveur web. Dans l’image ci-dessous, le numéro de port est 41788. Lorsque vous exécutez l’application, vous verrez probablement un autre numéro de port.

![](intro-to-aspnet-mvc-4/_static/image8.png)

Dès ce modèle par défaut vous donne les pages Accueil, Contact et sur. Il prend en charge pour vous inscrire et connecter et vous lie à Facebook et Twitter. L’étape suivante consiste à modifier le fonctionnement de cette application et en savoir un peu sur ASP.NET MVC. Fermez votre navigateur et nous allons modifier du code.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
