---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Créer un nouveau projet ASP.NET MVC | Microsoft Docs
author: microsoft
description: Étape 1 vous montre comment mettre en place de la structure d’application NerdDinner base.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 9f5e0b3f82d113fc72ab4002ec8d06ad8444dceb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374274"
---
<a name="create-a-new-aspnet-mvc-project"></a>Créer un nouveau projet ASP.NET MVC
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit d’étape 1 a gratuit [« « l’application NerdDinner](introducing-the-nerddinner-tutorial.md) qui présente en détail comment créer un petit mais terminé, l’application web à l’aide d’ASP.NET MVC 1.
> 
> Étape 1 vous montre comment mettre en place de la structure d’application NerdDinner base.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre le [mise en route avec MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de musique MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) didacticiels.


## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner étape 1 : Fichier -&gt;nouveau projet

Nous allons commencer notre application NerdDinner en sélectionnant le **fichier -&gt;nouveau projet** élément de menu dans Visual Studio 2008 ou sur le gratuit Visual Web Developer 2008 Express.

Cela fera apparaître la boîte de dialogue « Nouveau projet ». Pour créer une application ASP.NET MVC, nous allons sélectionnez le nœud « Web » sur le côté gauche de la boîte de dialogue, puis choisissez le modèle de projet « Application Web ASP.NET MVC » sur la droite :

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Important : Assurez-vous que vous avez téléchargé et installé ASP.NET MVC - sinon il n’apparaît pas dans la boîte de dialogue Nouveau projet. Vous pouvez utiliser la version 2 de la [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) si elle ne l’avez pas encore installé (ASP.NET MVC est disponible dans la « plate-forme Web -&gt;infrastructures et les Runtimes » section).*

Nous allons nommer le nouveau projet, que nous allons créer « NerdDinner », puis sur le bouton « OK » pour la créer.

Lorsque nous cliquons sur « OK » Visual Studio fait apparaître une boîte de dialogue supplémentaire qui nous demande créer éventuellement un projet de test unitaire pour la nouvelle application également. Ce projet de test unitaire permet de créer des tests automatisés qui vérifient les fonctionnalités et le comportement de notre application (quelque chose que nous allons aborder la tâche plus loin dans ce didacticiel).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

La liste déroulante « Test framework » dans la boîte de dialogue ci-dessus est remplie avec tous les disponible ASP.NET MVC unit test modèles de projet installés sur l’ordinateur. Les versions peuvent être téléchargées pour NUnit, MBUnit et XUnit. L’infrastructure de Test unitaire Visual Studio intégrée est également pris en charge.

*Remarque : L’infrastructure des tests unitaires Visual Studio est uniquement disponible avec Visual Studio 2008 Professional et versions ultérieures. Si vous utilisez Visual Studio 2008 Standard Edition ou Visual Web Developer 2008 Express, vous devrez télécharger et installer les extensions de NUnit, MBUnit ou XUnit pour ASP.NET MVC pour cette boîte de dialogue à afficher. La boîte de dialogue n’affiche pas si il ne sont pas des infrastructures de test installés.*

Nous allons utiliser le nom de « NerdDinner.Tests » par défaut pour le projet de test que nous créons et utiliser l’option de framework « Visual Studio Test unitaire ». Lorsque nous cliquons sur le bouton « OK », Visual Studio crée une solution pour nous avec deux projets à l’intérieur : un pour notre application web et l’autre pour nos tests unitaires :

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>Étude de la structure de répertoire de NerdDinner

Lorsque vous créez une application ASP.NET MVC avec Visual Studio, il ajoute automatiquement un nombre de répertoires et fichiers au projet :

![](create-a-new-aspnet-mvc-project/_static/image4.png)

Les projets ASP.NET MVC par défaut ont six répertoires de niveau supérieur :

| **Répertoire** | **Fonction** |
| --- | --- |
| **/ Contrôleurs** | Emplacement où vous placez les classes de contrôleur qui gèrent les demandes d’URL |
| **/ Modèles** | Emplacement où vous placez des classes qui représentent et manipulent des données |
| **/Views** | Emplacement où vous placez les fichiers de modèle de l’interface utilisateur qui sont responsables de la sortie de rendu |
| **/ Scripts** | Emplacement où vous placez les fichiers de bibliothèque JavaScript et les scripts (.js) |
| **/ Contenu** | Emplacement où vous placez le code CSS et les fichiers image et les autres contenus non-dynamique/non-JavaScript |
| **/ Application\_données** | Lorsque vous stockez des fichiers de données voulez-vous en lecture/écriture. |

ASP.NET MVC ne nécessite pas de cette structure. En fait, les développeurs qui travaillent sur des applications volumineuses généralement partitionne l’application de plusieurs projets pour le rendre plus facile à gérer (par exemple : les classes de modèles de données vont souvent dans un projet de bibliothèque de classes distinct à partir de l’application web). Toutefois, la structure de projet par défaut, offre une convention de répertoire par défaut intéressant que nous pouvons utiliser pour nettoyer nos préoccupations de l’application.

Lorsque nous développons le répertoire /Controllers, vous trouverez que Visual Studio a ajouté deux classes de contrôleur – HomeController et AccountController – par défaut pour le projet :

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Lorsque nous développez le répertoire /Views, nous trouverez trois sous-répertoires – /Home, /Account et /Shared – ainsi que modèle plusieurs fichiers au sein de celles-ci ont été également ajoutés au projet par défaut :

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Lorsque nous étendons la /Content ; et les répertoires du dossier /Scripts, vous trouverez un fichier Site.css qui est utilisé pour définir le style HTML sur le site, ainsi que les bibliothèques JavaScript qui peuvent activer ASP.NET AJAX et jQuery prennent en charge au sein de l’application :

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Lorsque nous développons le projet NerdDinner.Tests, vous trouverez deux classes qui contiennent des tests unitaires pour nos classes de contrôleur :

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Ces fichiers par défaut ajoutés par Visual Studio nous fournissent une structure de base pour une application opérationnelle - avec la page d’accueil, sur la page, les pages de connexion/déconnexion / d’inscription de compte et une page d’erreur non gérée (tous les accès câblé et du fonctionnement prêts à l’emploi).

### <a name="running-the-nerddinner-application"></a>Exécution de l’Application NerdDinner

Nous pouvons exécuter le projet en sélectionnant le **Debug -&gt;démarrer le débogage** ou **Debug -&gt;démarrer sans débogage** des éléments de menu :

![](create-a-new-aspnet-mvc-project/_static/image9.png)

Cette opération lance le serveur Web ASP.NET intégré-qui est fourni avec Visual Studio et exécuter notre application :

![](create-a-new-aspnet-mvc-project/_static/image10.png)

Voici la page d’accueil pour notre nouveau projet (URL : « / ») lorsqu’il s’exécute :

![](create-a-new-aspnet-mvc-project/_static/image11.png)

Cliquez sur l’onglet « About » pour afficher un sur la page (URL : « / Home/About ») :

![](create-a-new-aspnet-mvc-project/_static/image12.png)

Cliquant sur le lien « Ouverture de session » dans l’angle supérieur droit de nous mène à une page de connexion (URL : « / compte / d’ouverture de session »)

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Si nous n’avons pas un compte de connexion que nous pouvons cliquer sur le lien s’inscrire (URL : « / compte/Register ») pour en créer un :

![](create-a-new-aspnet-mvc-project/_static/image14.png)

Le code permettant d’implémenter la maison ci-dessus, à propos et déconnexion / inscrire fonctionnalité a été ajoutée par défaut lorsque nous avons créé notre nouveau projet. Nous allons l’utiliser comme point de départ de notre application.

### <a name="testing-the-nerddinner-application"></a>Test de l’Application NerdDinner

Si nous utilisons le Professional Edition ou une version plus récente de Visual Studio 2008, nous pouvons utiliser la prise en charge de l’IDE Visual Studio du test des unités pour le projet de test :

![](create-a-new-aspnet-mvc-project/_static/image15.png)

En choisissant une des options ci-dessus pour ouvrir le volet « Résultats de Test » dans l’IDE et faites-nous part de l’état de réussite/échec sur les tests 27 unitaires inclus dans notre nouveau projet qui couvrent la fonctionnalité intégrée :

![](create-a-new-aspnet-mvc-project/_static/image16.png)

Plus loin dans ce didacticiel nous en dirons plus sur les tests automatisés et ajouter des tests unitaires supplémentaires qui couvrent les fonctionnalités de l’application que nous implémentons.

### <a name="next-step"></a>Étape suivante

Nous avons à présent une structure d’application de base en place. Nous allons maintenant [créer une base de données pour stocker les données de notre application](create-a-database.md).

> [!div class="step-by-step"]
> [Précédent](introducing-the-nerddinner-tutorial.md)
> [Suivant](create-a-database.md)
