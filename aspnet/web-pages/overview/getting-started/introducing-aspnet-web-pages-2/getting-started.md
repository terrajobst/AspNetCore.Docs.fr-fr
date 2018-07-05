---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: Présentation des Pages Web ASP.NET - prise en main | Microsoft Docs
author: tfitzmac
description: WebMatrix n’est plus recommandé comme un environnement de développement intégré pour ASP.NET Web Pages. Utilisez Visual Studio ou Visual Studio Code. Ce guide un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 3717fe230f818f251ac5d0378701c28f6b427df8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382070"
---
<a name="introducing-aspnet-web-pages---getting-started"></a>Présentation des Pages Web ASP.NET - mise en route
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > 
> > WebMatrix n’est plus recommandé comme un environnement de développement intégré pour ASP.NET Web Pages. Utilisez [Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) ou [Visual Studio Code](https://code.visualstudio.com/).
> 
> 
> Cette application et conseils vous donne une vue d’ensemble des Pages Web ASP.NET (version 2 ou version ultérieure) et de la syntaxe Razor, une infrastructure légère permettant la création de sites Web dynamiques. Il présente également WebMatrix, un outil de création de pages et des sites.
> 
> **Niveau**: nouvelles pages Web ASP.NET.  
> **Compétences supposés**: HTML, base feuilles de style en cascade (CSS).
> 
> Ce que vous allez apprendre dans le premier didacticiel de l’ensemble :
> 
> - Quelle technologie ASP.NET Web Pages est et elle concerne.
> - WebMatrix nouveautés.
> - Comment installer tous les éléments.
> - Comment créer un site Web à l’aide de WebMatrix.
>   
> 
> Fonctionnalités technologies abordées :
> 
> - Microsoft Web Platform Installer.
> - WebMatrix.
> - *.cshtml* pages
>   
> 
> Mike Pope écrit à l’origine de ce didacticiel. Tom FitzMacken mis à jour pour Microsoft WebMatrix 3.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3


## <a name="what-should-you-know"></a>Ce que vous devez savoir ?

Nous partons du principe que vous êtes familiarisé avec :

- **HTML**. Aucune expertise approfondie n’est nécessaire. Pour expliquer HTML, mais nous avons également ne pas utiliser rien fait de complexe. Nous vous fournirons des liens vers des didacticiels HTML où nous pensons que leur utilité.
- **Feuilles de style en cascade (CSS)**. Identique au code HTML.
- **Idées de base de données**. Si vous avez utilisé une feuille de calcul pour les données et trié et filtré les données, ce qui sont le niveau d’expertise nous partons généralement pour cet ensemble de didacticiels.

Nous supposons également que vous êtes intéressé par apprentissage de la programmation de base. Pages Web ASP.NET utilisent un langage de programmation appelé c#. Vous n’êtes pas obligé d’avoir n’importe quel arrière-plan en programmation, juste un intérêt qu’il contient. Si vous avez déjà rédigé un JavaScript dans une page web avant, vous avez beaucoup d’arrière-plan.

Notez que si vous êtes familiarisé avec la programmation, vous constaterez peut-être que ce didacticiel défini initialement déplace lentement pendant que nous présentons les programmeurs débutants rapidement. À mesure que nous au-delà de la première peu de didacticiels, cependant, il y aura moins de base de programmation pour expliquer et avance le long de choses à un élément plus rapide.

## <a name="what-do-you-need"></a>De quoi avez-vous besoin ?

Voici ce dont vous aurez besoin :

- Un ordinateur qui exécute Windows 8, Windows 7, Windows Server 2008 ou Windows Server 2012.
- Une connexion internet active.
- Privilèges d’administrateur (requis pour le processus d’installation).

## <a name="what-is-aspnet-web-pages"></a>Nouveautés des Pages Web ASP.NET

Les Pages Web ASP.NET est une infrastructure que vous pouvez utiliser pour créer des pages web dynamiques. Une page web HTML est statique ; son contenu est déterminé par le balisage HTML fixe qui se trouve dans la page. Les pages dynamiques telles que celles que vous créez avec ASP.NET Web Pages vous permettent de créer le contenu de la page à la volée, à l’aide de code.

Pages dynamiques vous permettent d’effectuer toutes sortes de choses. Vous pouvez demander à un utilisateur pour l’entrée à l’aide d’un formulaire, puis modifier ce qu’affiche la page ou à quoi il ressemble. Vous pouvez prendre des informations à partir d’un utilisateur, enregistrez-le dans une base de données et puis de le liste ultérieurement. Vous pouvez envoyer des e-mails à partir de votre site. Vous pouvez interagir avec d’autres services sur le web (par exemple, un service de mappage) et produire des pages qui s’intègrent à partir de ces sources d’informations.

## <a name="what-is-webmatrix"></a>Qu’est WebMatrix ?

WebMatrix est un outil qui intègre un éditeur de page web, un utilitaire de base de données, un serveur web pour le test des pages et les fonctionnalités pour la publication de votre site Web sur Internet. WebMatrix est gratuit, et il est facile à installer et facile à utiliser. (Elle fonctionne également pour les pages HTML simples, ainsi que pour d’autres technologies telles que PHP.)

Vous n’avez pas réellement *ont* WebMatrix permet de travailler avec des Pages Web ASP.NET. Vous pouvez créer les pages à l’aide d’un texte éditeur, par exemple et testez les pages à l’aide d’un serveur web auquel vous avez accès à. Toutefois, WebMatrix rend tout très simple, donc ces didacticiels utilisera WebMatrix tout au long.

## <a name="about-these-tutorials"></a>À propos de ces didacticiels

Cet ensemble de didacticiels est une introduction à l’utilisation des Pages Web ASP.NET. Il existe des 9 didacticiels total dans cet ensemble de didacticiels d’introduction. Il s’agit de la partie d’une série de didacticiels jeux qui vous emmène novice de Pages Web ASP.NET à la création de sites Web réels, de qualité professionnelle.

Ce premier didacticiel définie concentrés sur vous montrant les principes fondamentaux de l’utilisation avec ASP.NET Web Pages. Lorsque vous avez terminé, vous pouvez travailler avec des ensembles de didacticiel supplémentaires qui reprennent où celle-ci se termine et Explorer plus en détail les Pages Web.

Nous allons délibérément simples sur des explications détaillées. Et chaque fois que nous montrer quelque chose, pour cet ensemble de didacticiels nous toujours choisir la façon dont nous pensons qu’est plus facile à comprendre. Les jeux de didacticiel ultérieures plus détaillés et vous montrent des approches plus efficaces ou plus souples (également plus amusant). Mais ces didacticiels vous devez d’abord comprendre les principes de base.

L’ensemble de didacticiels que vous venez de démarrer couvre ces fonctionnalités d’ASP.NET Web Pages :

- Introduction et l’obtention de tous les éléments installés. (C’est dans le didacticiel, que vous êtes en train de lire).
- Les principes fondamentaux de la programmation de Pages Web ASP.NET.
- Création d’une base de données.
- Création et traitement d’un formulaire d’entrée utilisateur.
- Ajout, la mise à jour et suppression de données dans la base de données.

## <a name="what-will-you-create"></a>Ce que vous allez créer ?

Ce didacticiel et les conditions suivantes sont axés sur un site Web où vous pouvez répertorier les films que vous le souhaitez. Vous serez en mesure d’entrer des films, de les modifier et de les répertorier. Voici quelques pages que vous créerez dans cet ensemble de didacticiels. La première affiche le film répertoriant page que vous allez créer :

![A fini Movie app affichant une liste de films](getting-started/_static/image1.png)

Et Voici la page qui vous permet d’ajouter de nouvelles informations de film à votre site :

![Application de film terminé affichant la page Ajouter un film](getting-started/_static/image2.png)

Génération de jeux de didacticiel ultérieures sur ce définissez et ajoutez davantage de fonctionnalités, telles que le chargement des images, car les employés à se connecter, envoi de courrier électronique et l’intégration aux réseaux sociaux.

## <a name="see-this-app-running-on-azure"></a>Consultez cette application en cours d’exécution sur Azure

Vous souhaitez voir le site terminé en cours d’exécution en tant qu’une application web en direct ? Vous pouvez déployer une version complète de l’application à votre compte Azure en cliquant simplement sur le bouton suivant.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

Vous avez besoin d’un compte Azure pour déployer cette solution sur Azure. Si vous n’avez pas déjà un compte, vous disposez des options suivantes :

- [Ouvrir un compte Azure gratuit](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -vous bénéficiez de crédits que vous pouvez utiliser pour tester les services Azure payants et même lorsqu’ils sont épuisés, vous pouvez conserver le compte et utiliser les services Azure gratuits.
- [Activer les avantages d’abonnement MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -votre abonnement MSDN vous donne des crédits chaque mois que vous pouvez utiliser pour les services Azure payants.

## <a name="installing-everything"></a>L’installation de tous les éléments

Vous pouvez installer tous les éléments à l’aide de Microsoft Web Platform Installer. En effet, vous installez le programme d’installation et puis l’utiliser pour installer tout le reste.

Pour utiliser les Pages Web, vous devez disposer au minimum Windows XP SP3 est installé, ou Windows Server 2008 ou version ultérieure.

Sur le [page Web Pages](../../../index.md) du site Web ASP.NET, cliquez sur **installer**.

![Affichage du site Web ASP.NET &quot;installer WebMatrix&quot; bouton](getting-started/_static/image3.png)

Vous êtes invité à accepter les termes du contrat de licence et la déclaration de confidentialité avant d’installer WebMatrix.

![Acceptez les termes pour commencer l’installation](getting-started/_static/image4.png)

Cliquez sur **exécuter** pour démarrer l’installation. (Si vous souhaitez enregistrer le programme d’installation, cliquez sur **enregistrer** puis exécutez le programme d’installation à partir du dossier où vous l’avez enregistré.)

![](getting-started/_static/image5.png)

Le programme d’installation de la plateforme Web s’affiche, prêt à installer WebMatrix. Cliquez sur **Installer**.

![](getting-started/_static/image6.png)

Le processus d’installation détermine qu’il doit installer sur votre ordinateur et démarre le processus. Selon exactement ce qui doit être installé, le processus peut prendre de quelques instants à plusieurs minutes. Sélectionnez **J’accepte** pour accepter les termes du contrat de licence.

## <a name="hello-webmatrix"></a>Hello, WebMatrix

Une fois terminé, le processus d’installation peut se lancer WebMatrix automatiquement. Si ce n’est pas, dans Windows, à partir de la **Démarrer** menu, lancez **Microsoft WebMatrix**.

Lorsque vous lancez WebMatrix pour la première fois, vous obtenez une occasion de vous connecter à Microsoft Azure avec votre compte Microsoft. En vous connectant, vous recevrez 10 applications gratuitement sur Internet via Azure. Ces applications web gratuites fournissent un moyen pratique pour tester vos applications. Si vous ne disposez pas d’un compte Azure, mais vous disposez d’un abonnement MSDN, vous pouvez [activer vos avantages d’abonnement MSDN](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604). Sinon, vous pouvez créer un compte d’essai gratuit en quelques minutes. Pour plus d’informations, consultez [version d’évaluation gratuite Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Il est inutile de se connecter dès maintenant continuer avec ce didacticiel. Si vous ne connectez pas maintenant, toujours avoir l’option pour vous connecter ultérieurement. La dernière [rubrique](publishing.md) dans ce didacticiel série explique comment déployer votre site Web dans Azure ; par conséquent, vous devez vous connecter à cette rubrique.

À ce stade, soit vous connecter avec votre compte Microsoft ou le sélectionner **pas maintenant** dans le coin inférieur droit.

![Se connecter](getting-started/_static/image7.png)

Pour commencer, vous allez créer un site Web vide et ajouter une page. Dans un didacticiel plus loin dans ce jeu, vous vous entraînerez avec un des modèles de site Web intégré.

Dans la fenêtre de démarrage, cliquez sur **New**.

![Écran de démarrage de WebMatrix](getting-started/_static/image8.png)

Les modèles sont des fichiers prédéfinis et des pages pour différents types de sites Web. Pour afficher tous les modèles qui sont disponibles par défaut, sélectionnez l’option de la galerie de modèles.

![Sélectionnez la galerie de modèles](getting-started/_static/image9.png)

Dans le **démarrage rapide** fenêtre, sélectionnez **Site vide** à partir de la **ASP.NET** de groupe et nommez le nouveau site « WebPagesMovies ».

![Fenêtre de démarrage rapide de WebMatrix avec le modèle Site vide sélectionné](getting-started/_static/image10.png)

Cliquez sur **Suivant**.

Si vous êtes connecté à votre compte Microsoft, vous aurez la possibilité de créer le site sur Azure. En fonction du nom de votre site, le nom par défaut **WebPagesMovies.azurewebsites.net** est suggéré ; Toutefois, le point d’exclamation indique que ce nom n’est pas disponible sur Windows Azure. Par souci de simplicité, sélectionnez **Skip** pour ignorer la création du site web sur Azure dès maintenant. Plus loin dans cette série, nous publierons le site vers Azure.

![créer un site azure](getting-started/_static/image11.png)

WebMatrix crée et ouvre le site :

![Nouveau site WebPagesMovies ouverts dans WebMatrix](getting-started/_static/image12.png)

En haut, il existe une barre d’outils Accès rapide et un ruban. En bas à gauche, vous voyez le sélecteur d’espace de travail dans lequel vous basculez entre les tâches (**Site**, **fichiers**, **bases de données**, **rapports**). Sur la droite est le volet de contenu pour l’éditeur et les rapports. Et en bas, vous verrez parfois une barre de notification pour les messages.

Vous apprendrez plus sur WebMatrix et ses fonctionnalités, tout au long de ces didacticiels.

## <a name="creating-a-web-page"></a>Création d’une Page Web

Pour vous familiariser avec WebMatrix et ASP.NET Web Pages, vous allez créer une page simple.

Dans le sélecteur d’espace de travail, sélectionnez le **fichiers** espace de travail. Cet espace de travail vous permet de travailler avec les fichiers et dossiers. Le volet gauche affiche la structure de fichiers de votre site. Les modifications de ruban pour afficher les tâches liées aux fichiers.

![Espace de travail de fichier dans WebMatrix](getting-started/_static/image13.png)

Dans le ruban, cliquez sur la flèche sous **New** puis cliquez sur **nouveau fichier**.

![À l’aide de la &quot;New&quot; commande dans le ruban pour créer un nouveau fichier](getting-started/_static/image14.png)

WebMatrix affiche une liste des types de fichiers. Sélectionnez **CSHTML**, puis, dans le **nom** , tapez « HelloWorld ». Une page CSHTML est une page ASP.NET Web Pages.

![Création d’une page CSHTML nommé HelloWorld.cshtml](getting-started/_static/image15.png)

Cliquez sur **OK**.

WebMatrix crée la page et l’ouvre dans l’éditeur.

![La nouvelle page HelloWorld dans l’éditeur WebMatrix](getting-started/_static/image16.png)

Comme vous pouvez le voir, la page contient principalement ordinaire balisage HTML, à l’exception d’un bloc en haut, qui ressemble à ceci :

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

C’est pour l’ajout de code, comme vous le verrez bientôt.

Notez que les différentes parties de la page &mdash; les noms d’éléments, attributs et texte, ainsi que le bloc en haut, sont tous dans différentes couleurs. Il s’agit *la coloration syntaxique*, et cela rend plus facile de tout Effacer. Il est l’une des fonctionnalités qui le rend facile à utiliser avec des pages web dans WebMatrix.

Ajouter du contenu pour le `<head>` et `<body>` éléments tels que dans l’exemple suivant. (Si vous le souhaitez, vous pouvez simplement copiez le bloc suivant et remplacez toute la page existante par le code.)

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

Dans la barre d’outils Accès rapide ou dans le **fichier** menu, cliquez sur **enregistrer**.

![Bouton Enregistrer dans la barre d’outils Accès rapide de WebMatrix](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>Test de la Page

Dans le **fichiers** espace de travail, avec le bouton droit le *HelloWorld.cshtml* page, puis cliquez sur **lancer dans le navigateur**.

![Une page en utilisant le bouton exécuter dans le ruban de WebMatrix en cours d’exécution](getting-started/_static/image18.png)

WebMatrix démarre un serveur web intégré (IIS Express) que vous pouvez utiliser pour tester les pages sur votre ordinateur. (Sans IIS Express dans WebMatrix, vous seriez obligé votre page Publier sur un serveur web à un endroit avant que vous pouvez le tester.) La page s’affiche dans votre navigateur par défaut.

![&quot;Hello World&quot; page en cours d’exécution dans le navigateur](getting-started/_static/image19.png)

Notez que lorsque vous testez une page dans WebMatrix, l’URL dans le navigateur est quelque chose comme `http://localhost:33651/HelloWorld.cshtml.` le nom *localhost* fait référence à un serveur local, ce qui signifie que la page est servie par un serveur web qui se trouve sur votre ordinateur. Comme indiqué, WebMatrix inclut un programme de serveur web nommé IIS Express qui s’exécute lorsque vous lancez une page.

Le nombre après *localhost* (par exemple, *localhost:33651*) fait référence à un *le numéro de port* sur votre ordinateur. Il s’agit du nombre de « canal » par IIS Express pour ce site Web particulier. Le numéro de port est sélectionné au hasard à partir de la plage 1024 et 65 536 lorsque vous créez votre site, et elle est différente pour chaque site que vous créez. (Lorsque vous testez votre propre site, le numéro de port sera certainement un nombre différent que 33561.) En utilisant un port différent pour chaque site Web, IIS Express peuvent conserver droites quel site il parle.

Par la suite lorsque vous publiez votre site sur un serveur web public, vous ne voyez plus *localhost* dans l’URL. À ce stade, vous verrez une URL plus classique comme `http://myhostingsite/mywebsite/HelloWorld.cshtml` ou n’importe quelle page de l’est. Vous allez en savoir plus sur la publication d’un site plus loin dans cette série de didacticiels.

## <a name="adding-some-server-side-code"></a>Ajout de Code côté serveur

Fermez le navigateur et revenir à la page dans WebMatrix.

Ajoutez une ligne au bloc de code afin qu’il ressemble au code suivant :

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

Il s’agit d’un peu de code Razor. Il est probablement clair qu’il obtient la date et heure actuelles et place cette valeur dans un *variable* nommé `currentDateTime`. Vous allez en savoir plus sur la syntaxe Razor dans le didacticiel suivant.

Dans le corps de la page, une fois que le `<p>Hello World!</p>` élément, ajoutez le code suivant :

[!code-html[Main](getting-started/samples/sample4.html)]

Ce code obtient la valeur que vous placez dans le `currentDateTime` variable en haut et l’insère dans le balisage de la page. Le `@` caractère marque le code ASP.NET Web Pages dans la page.

Exécutez à nouveau la page (WebMatrix enregistre les modifications apportées à votre place avant d’exécuter la page). Cette fois que vous voyez la date et l’heure dans la page.

![&quot;Hello World&quot; page en cours d’exécution dans le navigateur avec un affichage de l’heure générée dynamiquement](getting-started/_static/image20.png)

Attendez quelques instants, puis actualisez la page dans le navigateur. L’affichage de la date et l’heure est mise à jour.

Dans le navigateur, examinez la page source. Il se présente comme le balisage suivant :

[!code-html[Main](getting-started/samples/sample5.html)]

Notez que le `@{ }` bloc en haut n’y figure pas. Notez également que l’affichage de date et d’heure affiche une chaîne de caractères réelle (`1/18/2012 2:49:50 PM` ou quelle que soit), et non `@currentDateTime` que vous aviez le *.cshtml* page. Qu’est-il arrivé ici est que, lorsque vous avez exécuté la page, ASP.NET traitées tout le code (très peu de choses dans ce cas) qui a été marqué avec `@`. Le code génère la sortie, et cette sortie a été insérée dans la page.

## <a name="this-is-what-aspnet-web-pages-are-about"></a>C’est ce que sont les Pages Web ASP.NET sur

Lorsque vous lisez que ASP.NET Web Pages produit un contenu web dynamique, ce que vous avez vu ici est l’idée. La page que vous venez de créer contient le même balisage HTML que vous avez vu avant. Il peut également contenir du code qui peut effectuer toutes sortes de tâches. Dans cet exemple, elle le faisait la tâche facile d’obtenir la date et heure actuelles. Comme vous l’avez vu, vous pouvez intercaler code avec le code HTML pour produire une sortie dans la page. Quand un utilisateur demande un *.cshtml* page dans le navigateur, ASP.NET traite la page, alors qu’il est encore entre les mains du serveur web. ASP.NET insère la sortie du code (le cas échéant) dans la page au format HTML. Lorsque le traitement de code est effectué, ASP.NET envoie le résultat au navigateur. Le navigateur reçoit jamais est au format HTML. Voici un diagramme :

![Flux conceptuel de la façon dont ASP.NET génère dynamiquement HTML](getting-started/_static/image21.png)

Le concept est simple, mais le code peut effectuer de nombreuses tâches intéressantes, et il existe plusieurs façons intéressantes dans lequel vous pouvez dynamiquement ajouter du contenu HTML à la page. Et ASP.NET *.cshtml* pages, comme n’importe quelle page HTML, peuvent également inclure du code qui s’exécute dans le navigateur lui-même (code JavaScript et jQuery). Vous explorerez tous ces aspects dans cet ensemble de didacticiels et dans les conditions suivantes.

## <a name="coming-up-next"></a>Prochaine

Dans le didacticiel suivant de cette série, vous explorez un peu plus de programmation ASP.NET Web Pages.

## <a name="additional-resources"></a>Ressources supplémentaires

[Créer un site Web ASP.NET à partir de zéro](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch). Il s’agit d’un didacticiel qui est spécifiquement sur l’utilisation de WebMatrix (pas les Pages Web ASP.NET). Il prendra un peu plus en détail certaines des fonctionnalités supplémentaires de WebMatrix nous n’aborderons pas dans cet ensemble de didacticiels.

> [!div class="step-by-step"]
> [Next](intro-to-web-pages-programming.md)
