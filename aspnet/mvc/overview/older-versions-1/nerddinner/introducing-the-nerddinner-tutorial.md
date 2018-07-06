---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Introduction au didacticiel NerdDinner | Microsoft Docs
author: shanselman
description: La meilleure façon d’apprendre une nouvelle infrastructure consiste à créer quelque chose avec lui. Ce didacticiel vous montre comment créer une application légère, mais complète, à l’aide de ASP.NE...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 9188df1eca7f488502640bc17d5034f93f4ac82c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805108"
---
<a name="introducing-the-nerddinner-tutorial"></a>Introduction au didacticiel NerdDinner
====================
par [Scott Hanselman](https://github.com/shanselman)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> La meilleure façon d’apprendre une nouvelle infrastructure consiste à créer quelque chose avec lui. Ce didacticiel vous montre comment créer un petit mais terminé, l’application à l’aide d’ASP.NET MVC 1 et présente certains des principaux concepts derrière lui.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre le [mise en route avec MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de musique MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) didacticiels.


## <a name="nerddinner-tutorial"></a>Didacticiel NerdDinner

La meilleure façon d’apprendre une nouvelle infrastructure consiste à créer quelque chose avec lui. Ce didacticiel vous montre comment créer un petit mais terminé, l’application à l’aide d’ASP.NET MVC et présente certains des principaux concepts derrière lui.

L’application que nous allons générer est appelée « NerdDinner ». NerdDinner offre un moyen facile pour les personnes rechercher et organiser dîners en ligne :

![](introducing-the-nerddinner-tutorial/_static/image1.png)

NerdDinner permet à des utilisateurs inscrits créer, modifier et supprimer dîners. Il applique un ensemble cohérent de validation et les règles métier dans toute l’application :

![](introducing-the-nerddinner-tutorial/_static/image2.png)

Visiteurs peuvent utiliser une carte basée sur AJAX pour rechercher des prochains dîners qui se trouve près d’eux :

![](introducing-the-nerddinner-tutorial/_static/image3.png)

Un clic sur un dîner mène à une page de détails où ils peuvent en savoir plus à ce sujet :

![](introducing-the-nerddinner-tutorial/_static/image4.png)

Si elles sont intéressé par le dîner qu’ils puissent se connecter ou s’inscrire sur le site :

![](introducing-the-nerddinner-tutorial/_static/image5.png)

Ils peuvent puis cliquez sur un lien RSVP basée sur AJAX pour assister à l’événement :

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>Implémentation de NerdDinner

Nous allons commencer notre application NerdDinner en utilisant le fichier -&gt;commande Nouveau projet dans Visual Studio pour créer un tout nouveau projet ASP.NET MVC. Nous allons ajouter puis par incréments de fonctionnalités. Tout au long du processus, nous aborderons :

1. [Comment créer un nouveau projet ASP.NET MVC](# "créer un nouveau projet ASP.NET MVC")
2. [Comment créer une base de données](# "créer une base de données")
3. [Comment créer un modèle avec des validations de règles métier](# "créer un modèle avec des Validations de règles d’entreprise")
4. [Comment utiliser des contrôleurs et des vues pour implémenter une interface utilisateur liste/détails](# "utiliser les contrôleurs et les vues pour implémenter une interface utilisateur liste/détails")
5. [Comment fournir CRUD (créer, lire, mettre à jour, supprimer) les formulaires de données prise en charge de l’entrée](# "fournir CRUD (Create, Read, Update, Delete) données formulaire entrée prend en charge")
6. [Comment utiliser ViewData et implémenter des classes ViewModel](# "utiliser un ViewData et implémenter des Classes ViewModel")
7. [Comment réutiliser l’interface utilisateur à l’aide de pages maîtres et des vues partielles](# "réutiliser d’interface utilisateur à l’aide des Pages maîtres et des vues partielles")
8. [Comment implémenter la pagination des données efficace](# "implémenter de données efficace la pagination")
9. [Comment sécuriser des applications à l’aide de l’authentification et l’autorisation](# "sécurisé Applications à l’aide de l’authentification et autorisation")
10. [Comment utiliser AJAX pour fournir des mises à jour dynamiques](# "utiliser AJAX pour fournir des mises à jour dynamiques")
11. [Comment utiliser AJAX pour implémenter des scénarios de mappage](# "utiliser AJAX pour implémenter les scénarios de mappage")
12. [Comment activer les tests d’unités automatisés](# "activer le test unitaire automatisé")

Vous pouvez créer votre propre copie de NerdDinner à partir de zéro à la fin de chaque étape nous procédure pas à pas dans ce chapitre. Vous pouvez également télécharger une version complète du code source ici : [NerdDinner sur GitHub](https://github.com/AspNetMVPSamples/NerdDinner). Vous pouvez également éventuellement également [télécharger une version PDF de ce didacticiel](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) si vous souhaitez lire le didacticiel en mode hors connexion.

Vous pouvez utiliser Visual Studio 2008 ou sur le gratuit Visual Web Developer 2008 Express pour générer l’application. Vous pouvez utiliser SQL Server ou la version gratuite SQL Server Express pour la base de données.

Vous pouvez installer ASP.NET MVC, Visual Web Developer 2008 Express et SQL Server Express (gratuit) ce à l’aide de la version 2 de la [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>Maintenant nous allons commencer...

Maintenant que nous avons couvert NerdDinner What ' s, nous allons notre manches et écrire du code.

Nous allons commencer à l’aide de fichier -&gt;nouveau projet dans Visual Studio pour créer l’application NerdDinner.

> [!div class="step-by-step"]
> [Next](create-a-new-aspnet-mvc-project.md)
