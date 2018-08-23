---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: Introduction à ASP.NET MVC 3 (VB) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est en cours...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 92439a6c75e8e106d0c8c99b672dcfb1effd513f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832208"
---
<a name="intro-to-aspnet-mvc-3-vb"></a>Introduction à ASP.NET MVC 3 (VB)
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

> Ce didacticiel vous apprend les notions de base de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio. Avant de commencer, assurez-vous que vous avez installé les composants requis listés ci-dessous. Vous pouvez installer tous les en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les conditions préalables à l’aide des liens suivants :
> 
> - [Prérequis pour le Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Mettre à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + outils prennent en charge)
> 
> Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les conditions préalables en cliquant sur le lien suivant : [configuration requise de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un projet de Visual Web Developer avec le code source VB.NET est disponible pour accompagner cette rubrique. [Téléchargez la version VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si vous préférez c#, basculez vers le [c# version](../cs/intro-to-aspnet-mvc-3.md) de ce didacticiel.


Ce didacticiel vous apprend les notions de base de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio. Avant de commencer, assurez-vous que vous avez installé les composants requis listés ci-dessous. Vous pouvez installer tous les en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les conditions préalables à l’aide des liens suivants :

- [Prérequis pour le Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [Mettre à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + outils prennent en charge)

Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les conditions préalables en cliquant sur le lien suivant : [configuration requise de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Un projet de Visual Web Developer avec code source Visual Basic est disponible pour accompagner cette rubrique. [Téléchargez la version VB ici](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824). Si vous préférez CSharp, basculez vers le [CSharp version](../cs/intro-to-aspnet-mvc-3.md) de ce didacticiel.

## <a name="what-youll-build"></a>Ce que vous allez générer

Vous allez implémenter une application de liste de film simple qui prend en charge la création, la modification et la liste de films à partir d’une base de données. Voici deux captures d’écran de l’application que vous allez générer. Il comprend une page qui affiche une liste de films à partir d’une base de données :

[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)

L’application vous permet également ajouter, modifier et supprimer des films, mais aussi voir des détails sur les modifications individuelles. Tous les scénarios de saisie de données incluent la validation pour vous assurer que les données stockées dans la base de données sont correctes.

[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="skills-youll-learn"></a>Vous allez apprendre des compétences

Voici ce que vous allez apprendre :

- Comment créer un nouveau projet ASP.NET MVC
- Comment créer une nouvelle base de données à l’aide d’Entity Framework code first
- La création d’ASP.NET MVC contrôleurs et des vues
- Comment récupérer et afficher des données
- Comment modifier des données et activer la validation de données

## <a name="getting-started"></a>Prise en main

Commencez par exécuter Visual Web Developer 2010 Express (« VWD » pour faire plus court) et sélectionnez **nouveau projet** à partir de la **Démarrer** page.

Visual Web Developer est un environnement de développement intégré ou IDE. Tout comme vous utilisez Microsoft Word pour écrire des documents, vous utiliserez un IDE pour créer des applications. Dans Visual Web Developer, il existe une barre d’outils en haut montrant les différentes options disponibles pour vous. Il existe également un menu qui fournit une autre façon d’effectuer des tâches dans l’IDE. (Par exemple, au lieu de sélectionner **nouveau projet** à partir de la **Démarrer** page, vous pouvez utiliser le menu et sélectionnez **fichier** &gt; **denouveauprojet**.)

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a>Créer votre première Application

Vous pouvez créer des applications à l’aide de votre choix de Visual Basic ou Visual c# comme langage de programmation. Pour ce didacticiel, sélectionnez Visual Basic sur la gauche, puis **Application Web de ASP.NET MVC 3**. Nommez votre projet « MvcMovie », puis **OK**.

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

Dans le **nouveau projet ASP.NET MVC 3** boîte de dialogue, sélectionnez **Application Internet**. Laissez **Razor** en tant que le moteur d’affichage par défaut.

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

Cliquez sur **OK**. Visual Web Developer a utilisé un modèle par défaut pour le projet ASP.NET MVC que vous venez de créer, afin que vous ayez une application opérationnelle sans rien faire dès maintenant ! Il s’agit d’un simple « Hello World ! » projet et il l’un bon point de départ de votre application.

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

Dans le menu **Déboguer**, sélectionnez **Démarrer le débogage**.

![](intro-to-aspnet-mvc-3/_static/image11.png)

Notez que le raccourci clavier pour démarrer le débogage F5.

F5 provoque Visual Web Developer démarrer un serveur web de développement et d’exécuter votre application web. VWD lance un navigateur, puis ouvre la page d’accueil de l’application. Notez que la barre d’adresses du navigateur indique `localhost` et non quelque chose comme `example.com`. C’est parce que `localhost` pointe toujours vers votre ordinateur local, qui dans ce cas s’exécute l’application que vous venez de créer. Lorsque VWD s’exécute à un projet web, un port aléatoire est utilisé pour le projet. Dans l’image ci-dessous, le numéro de port aléatoire est 43246. Votre projet sera probablement utiliser un autre numéro de port.

![](intro-to-aspnet-mvc-3/_static/image12.png)

Prêt à l’emploi ce modèle par défaut donne vous deux pages à visiter et une page de connexion de base. Nous allons modifier le fonctionnement de cette application et en savoir un peu sur ASP.NET MVC dans le processus. Fermez votre navigateur et nous allons modifier du code.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
