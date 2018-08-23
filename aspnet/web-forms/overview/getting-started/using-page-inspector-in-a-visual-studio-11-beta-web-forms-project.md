---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: À l’aide de l’inspecteur de Page pour Visual Studio 2012 dans ASP.NET Web Forms | Microsoft Docs
author: rick-anderson
description: L’inspecteur de page pour Visual Studio 2012 est un outil de développement web avec un navigateur intégré. Sélectionnez n’importe quel élément dans le navigateur intégré et l’inspecteur de Page...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: d2c377f8466f8f324b75ce60860aa00c11bc0ffe
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838403"
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>À l’aide de l’inspecteur de Page pour Visual Studio 2012 dans Web Forms ASP.NET
====================
par Tim Ammann

> L’inspecteur de page pour Visual Studio 2012 est un outil de développement web avec un navigateur intégré. Sélectionnez n’importe quel élément dans le navigateur intégré et l’inspecteur de Page instantanément met en surbrillance l’élément source et CSS. Vous pouvez parcourir n’importe quelle page dans votre application, rapidement rechercher les sources de balisage rendu et utiliser les outils de navigateur directement dans l’environnement Visual Studio.
> 
> Ce didacticiel shwos comment activer le Mode d’Inspection rapidement rechercher et modifier les règles CSS et du texte dans votre projet web. Ce didacticiel utilise un projet d’Application Web Forms, mais vous pouvez également utiliser l’inspecteur de Page pour les projets de Site Web et [MVC](https://go.microsoft.com/?linkid=9802002) applications.
> 
> Le didacticiel comporte les sections suivantes :
> 
> [Composants requis](#_1_prerequisites)
> 
> [Créer une Application Web](#_2_creating_a)
> 
> [Utiliser l’inspecteur de Page pour afficher l’Application](#_3_using_page)
> 
> [Activer le Mode d’Inspection](#_4_inspection_mode)
> 
> [Utiliser l’inspecteur de Page pour apporter des modifications au balisage](#_5_using_page)
> 
> [Mode d’inspection et de la fenêtre HTML](#_6_inspection_mode)
> 
> [Aperçu des modifications dans la fenêtre Styles CSS](#_7_previewing_css)
> 
> [Synchronisation automatique CSS](#css_auto_sync)
> 
> [À l’aide du sélecteur de couleurs CSS](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Prérequis

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) ou [Visual Studio Express 2012 pour le Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Pour obtenir la dernière version de l’inspecteur de Page, utilisez [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) pour installer le kit SDK Azure pour .NET 2.0.


Inspecteur de page est fourni avec les outils de développement Web de Microsoft. La dernière version est 1.3. Pour vérifier quelle version vous avez, exécutez Visual Studio, puis sélectionnez **propos de Microsoft Visual Studio** à partir de la **aide** menu.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Créer une Application Web

Tout d’abord, vous allez créer une application web que vous utiliserez avec l’inspecteur de Page. Dans Visual Studio, choisissez **fichier** &gt; **nouveau projet**. Sur la gauche, développez **Visual C#**, sélectionnez **Web**, puis sélectionnez **Application ASP.NET Web Forms**.

![Nouvelle Application Web Forms](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

Cliquez sur **OK**.

L’application s’ouvre dans **Source** vue.

![Nouvelle Application Web Forms en mode Source](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

Maintenant que vous avez une application fonctionne avec, vous pouvez utiliser l’inspecteur de Page pour examiner et modifiez-le.

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>Utiliser l’inspecteur de Page pour afficher l’Application

Ensuite, vous allez afficher l’application avec l’inspecteur de Page. Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis choisissez **afficher dans l’inspecteur de Page**.

![Afficher dans l’inspecteur de Page](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

Par défaut, lors de l’inspecteur de Page se lance pour la première fois, elle est ancrée sous forme de fenêtre étroit sur le côté gauche de l’environnement Visual Studio. Laisser ancrée sur le côté gauche et le définir sur une largeur qui est à l’aise pour vous, ou l’ancre dans les zones de l’outil sur le haut, bas ou à droite :

![Positionne l’inspecteur de page d’accueil](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

Si vous détacher de la fenêtre de l’inspecteur de Page, vous pouvez placer il hors de Visual Studio, ou même sur un deuxième écran si vous en avez pas. Toutefois, dans l’ordre à ALT + TAB entre l’inspecteur de Page et de Visual Studio lorsque la fenêtre d’inspecteur de Page est non attachée, accédez à **outils** &gt; **Options** &gt;  **Environnement** &gt; **onglets et Windows**, puis, sous **onglet bien**, désactivez la case à cocher appelée **fenêtres d’outils flottante restent toujours affichés en haut de la fenêtre principale**:

![Décochez la case de windows outil flottante ALT + TAB entre Visual Studio et la fenêtre d’inspecteur de Page non ancrée](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

Le volet supérieur de la fenêtre de l’inspecteur de Page affiche la page actuelle dans une fenêtre de navigateur. Le volet inférieur affiche la page dans le balisage HTML sur la gauche, et certains onglets à droite qui vous permettent d’inspecter les différents aspects de la page. Le volet inférieur est similaire à la [outils de développement F12](https://msdn.microsoft.com/ie/aa740478) dans Internet Explorer. (Toutefois, contrairement aux outils de développement, vous pouvez utiliser l’inspecteur de Page directement dans Visual Studio.)

![Inspecteur de page](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

Dans ce didacticiel, vous allez utiliser le volet navigateur de l’inspecteur de Page et le **HTML** et **Styles** onglets pour vous aider à rapidement naviguer et apporter des modifications à l’application.

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>Activer le Mode d’Inspection

Ensuite, vous verrez comment le Mode d’Inspection de l’inspecteur de Page fonctionne. Dans la fenêtre Inspecteur de Page, cliquez sur le **inspecter** bouton.

![Inspecter l’élément](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

Pour afficher le mode d’inspection en action, déplacez votre souris sur différentes parties de la page dans la fenêtre de navigateur de l’inspecteur de Page. Comme vous le faites, le pointeur de la souris passe à un signe plus volumineux, et la mise en surbrillance de l’élément en dessous :

![Survole div.content-wrapper](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

Lorsque vous déplacez le pointeur de la souris, notez que

- Le contenu de **Source** afficher change pour afficher le balisage correspondant à l’élément sélectionné dans la page. Le balisage approprié est mis en surbrillance. Si la source est dans un autre fichier, ce fichier est ouvert en mode Source avec le balisage pertinents mis en surbrillance.

- Le balisage affiché dans le **HTML** onglet dans l’inspecteur de Page change également pour correspondre à l’élément sélectionné dans la page. Dans le **HTML** onglet, le balisage approprié est indiqué.

- Le **Styles** onglet affiche les règles CSS relatives à la sélection actuelle.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Utiliser l’inspecteur de Page pour apporter des modifications au balisage

Vous voyez à présent comment vous pouvez utiliser l’inspecteur de Page pour rechercher et d’apporter des modifications au balisage ou de texte dont l’emplacement n’est peut-être pas immédiatement évident.

Placer l’inspecteur de Page en Mode d’Inspection et puis faites défiler vers le bas de la page d’accueil.

Dès que vous entrez la zone de pied de page, l’inspecteur de Page ouvre le *Site.Master* fichier de disposition dans **Source** vue dans un onglet temporaire à droite des autres onglets et met en surbrillance de la section du maître page que vous avez avez sélectionné. Cela vous montre comment l’inspecteur de Page peuvent rechercher et afficher le contenu sur une page qui proviennent en fait un autre fichier que celui que vous avez ouvert à l’origine.

![Met en évidence de pied de page en Mode d’Inspection](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

Dans la fenêtre de navigateur de l’inspecteur de Page, déplacez votre pointeur de la souris sur la ligne avec les informations de copyright <a id="a"> </a>Notez.

Dans le *Site.Master* page, la ligne correspondante est mise en surbrillance.

![Ligne de pied de page copyright mis en surbrillance](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

Ajouter du texte à la fin de la ligne dans le *Site.Master* fichier.

&lt;p&gt;&amp;copier ; &lt;% : DateTime.Now.Year %&gt; -mon Rocks d’Application ASP.NET !&lt; /p&gt;

Maintenant, appuyez sur Ctrl + Alt + Entrée ou cliquez sur la barre de mise à jour pour afficher les résultats dans la fenêtre de navigateur de l’inspecteur de Page.

![Mon Rocks Application ASP.NET !](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

Vous considériez sans doute que le pied de page a été sur le *Default.aspx* page, mais il est avéré pour être dans la page de disposition principale et l’inspecteur de Page trouvé pour vous.

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Mode d’inspection et de la fenêtre HTML

Ensuite, vous aurez un rapide coup de œil à la fenêtre HTML et comment il mappe les éléments pour vous.

Placez l’inspecteur de Page en Mode d’Inspection.

![Inspecter l’élément](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

Cliquez sur la partie supérieure de la page indiquant que « votre logo ici ». Vous examinez un élément particulier plus en détail, afin de l’affichage dans la fenêtre du navigateur ne change plus lorsque vous déplacez le pointeur de la souris.

Maintenant déplacer le pointeur de la souris vers le **HTML** fenêtre. Lorsque vous déplacez le pointeur de la souris, l’inspecteur de Page décrit l’élément dans le **HTML** fenêtre et met en surbrillance l’élément correspondant dans la fenêtre du navigateur.

![Fenêtre HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

Comme auparavant, l’inspecteur de Page ouvre le *Site.Master* fichier pour vous dans un onglet temporaire. Cliquez sur l’onglet Site.Master, et le balisage correspondant est mis en surbrillance dans le &lt;en-tête&gt; section :

![Balisage en surbrillance](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Aperçu des modifications dans la fenêtre Styles CSS

Ensuite, vous verrez comment vous pouvez utiliser l’inspecteur de Page **Styles** fenêtre pour afficher un aperçu des modifications apportées à CSS.

Cliquez sur le **inspecter** bouton à placer l’inspecteur de Page en Mode d’Inspection.

Dans la fenêtre de navigateur de l’inspecteur de Page, placez le pointeur de la souris sur la section « Page d’accueil » jusqu'à ce que le **div.content-wrapper** étiquette s’affiche.

![Vous placez le curseur sur les éléments](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

Cliquez sur une seule fois dans la section div.content-wrapper, et puis déplacez le pointeur de la souris vers le **Styles** fenêtre. Sous le sélecteur de classe wrapper de .content .featured, désactivez et activez la case à cocher pour la propriété de couleur d’arrière-plan.

![Couleur d’arrière-plan clair](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

Notez comment la modification affiche un aperçu d’instantanément dans la fenêtre de navigateur de l’inspecteur de Page.

Sélectionnez la case à cocher à nouveau, puis double-cliquez sur la valeur de propriété et remplacez-le par `red`. La modification s’affiche immédiatement :

![Couleur d’arrière-plan rouge](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

Le **Styles** rend fenêtre facilement tester et de prévisualiser les CSS qui change avant de valider les modifications apportées au style de feuille de lui-même.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Synchronisation automatique CSS

> [!NOTE]
> Cette fonctionnalité nécessite la version 1.3 de l’inspecteur de Page.


La fonctionnalité de synchronisation automatique CSS vous permet de modifier directement un fichier CSS et voir les modifications immédiatement dans le navigateur de l’inspecteur de Page.

Cliquez sur **inspecter** à placer l’inspecteur de Page en Mode d’Inspection.

Dans le navigateur de l’inspecteur de Page, placez le pointeur de la souris sur la section « Page d’accueil » jusqu'à ce que le **div.content-wrapper** étiquette s’affiche. Cliquez une fois pour sélectionner cet élément.

Le **styles** fenêtre affiche toutes les règles CSS pour cet élément. Faites défiler jusqu'à find .featured .content-wrapper du sélecteur de classe. Cliquez sur « .featured .content-wrapper ». Inspecteur de page ouvre le fichier CSS qui définit ce style (Site.css) et met en évidence le style CSS correspondant.

![Fichier CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

Modifiez maintenant la valeur de `background-color` sur « rouge ». La modification s’affiche immédiatement dans le navigateur de l’inspecteur de Page.

![Navigateur de l’inspecteur de page](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>À l’aide du sélecteur de couleurs CSS

Ensuite, vous allez apprendre à utiliser l’inspecteur de Page pour rapidement rechercher et modifier le code CSS pour le texte mis en surbrillance dans l’application par défaut. Dans cet exemple, vous avez décidé que vous ne la surbrillance bleue et souhaitez remplacer par une autre couleur.

Cliquez sur le **inspecter** bouton.

![Inspecter l’élément](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

Dans la fenêtre de navigateur de l’inspecteur de Page, placez le pointeur de la souris sur la mise en surbrillance « vidéos, didacticiels et exemples » texte afin que le code CSS « marquer « étiquette s’affiche.

![Vous placez le curseur sur l’élément de la marque](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

Cliquez sur le texte pour le sélectionner. Le sélecteur de mark CSS correspondant s’affiche en bas de la **Styles** fenêtre.

![Sélecteur de marque dans la fenêtre Styles](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

Cliquez sur le sélecteur d’interrogation. Cette opération ouvre le *Site.css* fichier pour l’application web. Cliquez sur l’onglet Site.css et les CSS correspondants pour le sélecteur est mis en surbrillance :

![Sélecteur de marque dans la feuille de style](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

Sélectionnez et supprimez la ligne avec la propriété de couleur d’arrière-plan.

Vous allez maintenant utiliser le nouveau sélecteur de couleurs CSS de 2012 Visual Studio pour choisir une nouvelle couleur pour le **marquer** propriété de couleur d’arrière-plan.

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>À l’aide du sélecteur de couleurs CSS de Visual Studio 2012

L’éditeur CSS dans Visual Studio 2012 a un sélecteur de couleurs qui permet de facilement choisir et insérer des couleurs. Il a une barre de couleurs simple et un sélecteur « pop-down » qui offre un contrôle plus précis.

Le sélecteur de couleurs inclut une palette de couleurs standard prend en charge les noms de couleurs standard, les codes de hachage, les couleurs RGB, RGBA, HSL et HSLA et gère une liste des couleurs que vous avez utilisé le plus récemment dans le document.

Sur la ligne où la propriété de couleur d’arrière-plan a été, tapez « bc » et appuyez sur la flèche vers le bas une fois.

Lorsque vous tapez le premier caractère de chaque mot dans une propriété séparées par des traits d’union comme « couleur d’arrière-plan », IntelliSense filtre la liste pour afficher uniquement les propriétés qui correspondent à :

![Valeurs IntelliSense filtré](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

Tapez maintenant un signe deux-points. Lorsque vous le faites, le nom de propriété de couleur d’arrière-plan complet est inséré. Type **#** ou **RVB (**, et la barre de sélecteur de couleurs s’affiche :

![La barre de sélecteur de couleurs CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

Pour observer le fonctionne de la barre de sélecteur de couleurs, cliquez sur ses couleurs avec le pointeur de la souris, ou appuyez sur la touche de direction bas et puis utilisez les touches de direction gauche et droite pour parcourir les couleurs. Lorsque vous visitez une couleur, la valeur correspondante pour la propriété de couleur d’arrière-plan est affiché en aperçu :

![valeur de propriété de couleur d’arrière-plan aperçu](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

À ce stade, vous pouvez appuyer sur ENTRÉE pour sélectionner la valeur, puis un point-virgule ( ;) pour terminer l’entrée CSS. Pour l’instant, passez à la section suivante afin que vous puissiez voir le fonctionnement de la couleur sélecteur contextuel déroulante.

#### <a name="using-the-color-picker-pop-down"></a>À l’aide du sélecteur de couleur Pop en bas

Lors de la barre de couleurs n’a pas la couleur exacte que vous recherchez, vous pouvez utiliser le sélecteur de couleurs contextuelle déroulante.

Pour l’ouvrir, cliquez sur le double chevron à l’extrémité droite de la barre de couleurs, ou appuyez sur la flèche vers le bas une ou deux fois sur le clavier.

![Sélecteur de couleur CSS Pop-bas](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

Cliquez sur une couleur dans la barre verticale sur la droite. Voici un dégradé de cette couleur dans la fenêtre principale. Choisissez une couleur directement à partir de la barre verticale en appuyant sur entrée, ou cliquez sur n’importe quel point dans la fenêtre principale de choisir avec une précision supérieure.

S’il est une couleur à l’écran que vous souhaitez utiliser (il n’a pas à l’intérieur de l’interface utilisateur de Visual Studio), vous pouvez capturer sa valeur à l’aide de l’outil Pipette dans le coin inférieur droit.

Vous pouvez également modifier l’opacité d’une couleur en déplaçant le curseur en bas du sélecteur de couleurs. Procédant ainsi, les modifications de valeurs pour différentes valeurs RVBA de couleur, car le format RVBA peut représenter l’opacité.

Une fois que vous avez choisi une couleur, appuyez sur entrée, puis tapez un point-virgule pour terminer l’entrée de la couleur d’arrière-plan dans le *Site.css* fichier.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>La barre de mise à jour d’inspecteur de Page

Inspecteur de page détecte immédiatement la modification apportée à la *Site.css* fichier (ou à n’importe quel fichier dans l’application) et affiche une alerte dans une barre de mise à jour.

![Barre de mise à jour](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

Pour enregistrer tous vos fichiers et actualisez le navigateur de l’inspecteur de Page, appuyez sur Ctrl + Alt + Entrée ou cliquez sur la barre de mise à jour. La modification de la couleur de surbrillance apparaît dans le navigateur :

![Couleur de surbrillance modifié](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>Notez que vous avez actualisé aisément le navigateur de l’inspecteur de Page directement à partir de l’environnement Visual Studio. À l’aide de l’inspecteur de Page au lieu d’un navigateur externe vous permet de rester dans l’éditeur lorsque vous développez vos applications web.

## <a name="see-also"></a>Voir aussi

[Présentation de l’inspecteur de Page](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (vidéo Channel 9)

[Messages d’erreur de l’inspecteur de page](https://go.microsoft.com/?linkid=9813062) (MSDN)
