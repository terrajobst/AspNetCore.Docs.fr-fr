---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: Exécution de plusieurs Animations après l’autre (c#) | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il permet d’exécuter la chute...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: 60fb7dacfe59eaafef71fcc9cde61b7840624343
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836848"
---
<a name="executing-several-animations-after-each-other-c"></a>Exécution de plusieurs Animations après l’autre (c#)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)

> Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il permet d’exécuter plusieurs animations un après l’autre.


Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il permet d’exécuter plusieurs animations un après l’autre.

## <a name="steps"></a>Étapes

Tout d’abord inclure le `ScriptManager` dans la page ; ensuite, la bibliothèque AJAX ASP.NET est chargée, ce qui permet d’utiliser les outils de contrôle :

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

L’animation sera appliquée à un volet de texte qui ressemble à ceci :

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

Dans la classe CSS associée pour le panneau, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau :

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

Dans le `<Animations>` nœud, utilisez `<OnLoad>` pour exécuter les animations, une fois que la page a été entièrement chargée. En règle générale, `<OnLoad>` accepte uniquement une animation. Le framework d’Animation vous permet de joindre plusieurs animations en un seul via le `<Sequence>` élément. Toutes les animations dans `<Sequence>` sont exécutées une après l’autre. Voici l’un balisage possible pour le `AnimationExtender` contrôle, tout d’abord effectuer le panneau plus large et puis diminuant leur hauteur :

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

Lorsque vous exécutez ce script, le panneau première obtient ensuite petite et plus large.


[![Tout d’abord la largeur est augmentée.](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)

Tout d’abord la largeur est augmentée ([cliquez pour afficher l’image en taille réelle](executing-several-animations-after-each-other-cs/_static/image3.png))


[![Puis diminue la hauteur](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)

Puis diminue la hauteur ([cliquez pour afficher l’image en taille réelle](executing-several-animations-after-each-other-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Précédent](executing-several-animations-at-the-same-time-cs.md)
> [Suivant](animation-depending-on-a-condition-cs.md)
