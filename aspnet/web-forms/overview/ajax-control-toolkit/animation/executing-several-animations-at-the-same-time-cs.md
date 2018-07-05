---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
title: Exécution de plusieurs Animations en même temps (c#) | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il permet d’exécuter la chute...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 219149e1-3ee9-4b79-8fe4-7433f6b7d15b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
msc.type: authoredcontent
ms.openlocfilehash: 017a37533ab055ad149cf0de3a5892ae92741b84
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816848"
---
<a name="executing-several-animations-at-the-same-time-c"></a>Exécution de plusieurs Animations en même temps (c#)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)

> Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il permet d’exécuter plusieurs animations de manière parallèle.


## <a name="overview"></a>Vue d'ensemble

Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il permet d’exécuter plusieurs animations de manière parallèle.

## <a name="steps"></a>Étapes

Tout d’abord inclure le `ScriptManager` dans la page ; ensuite, la bibliothèque AJAX ASP.NET est chargée, ce qui permet d’utiliser les outils de contrôle :

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample1.aspx)]

L’animation sera appliquée à un volet de texte qui ressemble à ceci :

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample2.aspx)]

Dans la classe CSS associée pour le panneau, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau :

[!code-css[Main](executing-several-animations-at-the-same-time-cs/samples/sample3.css)]

Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server"`:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample4.aspx)]

Dans le `<Animations>` nœud, utilisez `<OnLoad>` pour exécuter les animations, une fois que la page a été entièrement chargée. En règle générale, `<OnLoad>` accepte uniquement une animation. Le framework d’Animation vous permet de joindre plusieurs animations en un seul via le `<Parallel>` élément. Toutes les animations dans `<Parallel>` sont exécutées en même temps.

Voici l’un balisage possible pour le `AnimationExtender` contrôle, atténuant progressivement et redimensionner le panneau en même temps :

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample5.aspx)]

Et effectivement : lorsque vous exécutez ce script, le panneau s’affiche, puis redimensionne (plus que threefolding sa largeur et sa halfing sa hauteur), puis disparaît via en même temps.


[![Le panneau est fondu et redimensionnement (y compris son contenu, grâce à moteur de rendu du navigateur)](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)

Le panneau est fondu et redimensionnement (y compris son contenu, grâce à moteur de rendu du navigateur) ([cliquez pour afficher l’image en taille réelle](executing-several-animations-at-the-same-time-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](adding-animation-to-a-control-cs.md)
> [Suivant](executing-several-animations-after-each-other-cs.md)
