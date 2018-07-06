---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Introduction à ASP.NET MVC | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC. Créer une application web simple qui lit et écrit à partir d’une base de données.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: ee39cf78835767d73bd9a1d4c9a2c4ca75aca5fa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808861"
---
<a name="intro-to-aspnet-mvc"></a>Introduction à ASP.NET MVC
====================
par [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Une version mise à jour si ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) à l’aide de [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Le nouveau didacticiel utilise ASP.NET MVC 5, qui fournit de nombreuses améliorations de ce didacticiel.
> 
> 
> Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC. Vous allez créer une application web simple qui lit et écrit à partir d’une base de données. Visitez le [centre d’apprentissage ASP.NET MVC](../../../index.md) pour rechercher d’autres ASP.NET MVC didacticiels et exemples.


Assurons-nous que notre première Application Web ASP.NET MVC à l’aide [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/). Nous allons rendre une petite application de liste de films qui sera créons et liste de films.

## <a name="what-youll-build"></a>Ce que vous allez générer

Voici deux captures d’écran de l’application que vous allez générer. Vous aurez une table simple de films avec diverses colonnes.

[![Liste de films - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

Et vous aurez un formulaire de création afin de pouvoir ajouter à la liste de films.

[![Créer un film - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>Vous allez apprendre des compétences

Ce didacticiel vous apprend les notions de base de la création d’une Application de Web ASP.NET MVC à l’aide de Visual Studio. Vous apprendrez à :

- Comment créer un nouveau projet ASP.NET MVC
- Comment créer une nouvelle base de données avec SQL Server
- Comment créer des vues et contrôleurs de MVC ASP.NET
- Comment récupérer et afficher des données
- Comment modifier des données et activer la validation de données
- Comment mettre à jour le schéma de base de données

## <a name="get-started"></a>Bien démarrer

Commencez par exécuter Visual Web Developer 2010 Express (je l’appellerai « VWD » par la suite) et sélectionnez Nouveau projet à partir de l’écran d’accueil.

Visual Web Developer est un IDE ou l’environnement de développement intégré. Tout comme vous utilisez Microsoft Word pour écrire des documents, vous utiliserez un IDE pour créer des applications. Il existe une barre d’outils en haut montrant les différentes options disponibles pour vous, ainsi que du menu vous pouvez également avoir utilisé pour sélectionner le fichier | Nouveau projet.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>Créer votre première Application

Vous pouvez créer des applications à l’aide de Visual Basic ou Visual c#. Pour l’instant, sélectionnez Visual c# sur la gauche, puis choisissez « Application Web de ASP.NET MVC 2. » Nommez votre projet « Movies » et cliquez sur OK.

[![Nouveau projet](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

Dans la partie droite est l’Explorateur de solutions affichant tous les fichiers et dossiers dans votre application. La fenêtre big au milieu est où vous modifiez votre code et la plupart de votre temps. Visual Studio a utilisé un modèle par défaut pour le projet ASP.NET MVC que vous venez de créer, afin que vous ayez une application opérationnelle sans rien faire dès maintenant ! Il s’agit d’un simple « Hello World ! projet et que c’est un bon point de départ pour notre application.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

Sélectionnez le bouton « lecture » à la barre d’outils.

![Démarrer le débogage](getting-started-with-mvc-part1/_static/image11.png)

Il s’agit d’une flèche verte pointant vers la droite qui compilera votre programme et démarrer votre application dans un navigateur web.

*Remarque : Vous pouvez à la place appuyez sur F5 de votre clavier, ou sélectionnez débogage -&gt;démarrer le débogage à partir du menu « Debug ».*

Ainsi, Visual Web Developer démarrer un serveur web de développement et d’exécuter notre application web (il n’existe aucune configuration ou les étapes manuelles requises pour activer cette option). Puis il lance un navigateur et configurez-le pour parcourir la page d’accueil de l’application. Ci-dessous, notez que la barre d’adresses du navigateur indique « localhost » et pas quelque chose comme example.com. C’est parce que localhost pointe toujours vers votre propre ordinateur local - qui dans ce cas s’exécute l’application que nous venez de créer.

[![Page d’accueil](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

Prêt à l’emploi ce modèle par défaut donne vous deux pages à visiter et une page de connexion de base. Nous allons modifier le fonctionnement de cette application et en savoir un peu sur ASP.NET MVC dans le processus. Fermez votre navigateur et vous permet de modifier du code.

> [!div class="step-by-step"]
> [Next](getting-started-with-mvc-part2.md)
