---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
title: À l’aide de l’extendeur de contrôle ColorPicker (c#) | Microsoft Docs
author: microsoft
description: ColorPicker est un extendeur ASP.NET AJAX qui fournit des fonctionnalités de sélection de couleur côté client avec l’interface utilisateur dans un contrôle de fenêtre contextuelle. Il peut être associé à n’importe quel ASP.NET...
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: 0d86a1e7-a910-4ab2-b85c-7a9ea6906c39
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 308f56ecc1ff76ebdaea26e127a06687fcbcaba6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837585"
---
<a name="using-the-colorpicker-control-extender-c"></a>À l’aide de l’extendeur de contrôle ColorPicker (c#)
====================
par [Microsoft](https://github.com/microsoft)

> ColorPicker est un extendeur ASP.NET AJAX qui fournit des fonctionnalités de sélection de couleur côté client avec l’interface utilisateur dans un contrôle de fenêtre contextuelle. Il peut être associé à n’importe quel contrôle de zone de texte de ASP.NET. Il.


L’objectif de ce didacticiel est d’expliquer comment vous pouvez utiliser l’extendeur du contrôle ColorPicker de boîte à outils de contrôle AJAX. L’extendeur de contrôle ColorPicker affiche une boîte de dialogue contextuelle qui vous permet de sélectionner une couleur. Le composant ColorPicker est utile lorsque vous souhaitez fournir une interface utilisateur intuitive pour un utilisateur de choisir une couleur.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Extension d’un contrôle de zone de texte avec l’extendeur de contrôle ColorPicker

Par exemple, imaginez que vous souhaitez créer un site Web qui permet aux visiteurs de créer des cartes de visite personnalisées. Les visiteurs peuvent entrer le texte pour une carte de visite et choisir la couleur. La page ASP.NET dans le Listing 1 contienne deux contrôles TextBox nommés txtCardText et txtCardColor. Lorsque vous envoyez le formulaire, les valeurs sélectionnées sont affichées (voir Figure 1).


[![Formulaire simple pour la création d’une carte de visite](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)

**Figure 01**: formulaire Simple pour la création d’une carte de visite ([cliquez pour afficher l’image en taille réelle](using-the-colorpicker-control-extender-cs/_static/image2.png))


**Liste 1 - CreateCard.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

Le formulaire de liste 1 fonctionne, mais elle ne fournit pas une expérience utilisateur satisfaisante. L’utilisateur doit taper une couleur dans la zone de texte. Si l’utilisateur veut une couleur spécialisée - par exemple, simplement la droite nuance de vert de pea - puis l’utilisateur doit déterminer le code de couleur HTML sans l’aide.

Vous pouvez utiliser l’extendeur du contrôle ColorPicker pour créer une meilleure expérience utilisateur. Le composant ColorPicker affiche une boîte de dialogue couleur lorsque vous déplacez le focus à un contrôle de zone de texte (voir Figure 2).


[![L’extendeur de contrôle ColorPicker](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)

**Figure 02**: l’extendeur de contrôle ColorPicker ([cliquez pour afficher l’image en taille réelle](using-the-colorpicker-control-extender-cs/_static/image4.png))


Vous devez suivre deux étapes pour utiliser l’extendeur du contrôle ColorPicker avec le formulaire dans la liste 1 :

1. Ajouter un contrôle ScriptManager à la page
2. Ajoutez l’extendeur de contrôle ColorPicker à la page

Avant de pouvoir utiliser le composant ColorPicker, vous devez ajouter un ScriptManager à votre page. Pour ajouter le ScriptManager, vous pouvez juste en dessous ouverture côté serveur &lt;formulaire&gt; balise. Vous pouvez faire glisser le ScriptManager sur la page à partir de la boîte à outils (ScriptManager se trouve sous l’onglet Extensions AJAX). Vous pouvez également taper la balise suivante dans la vue de Source sous la balise de formulaire côté serveur d’ouverture :

&lt;ASP : ScriptManager ID = « ScriptManager1 » runat = « server » /&gt;

Le moyen le plus simple pour ajouter l’extendeur de contrôle ColorPicker à la page est en mode Design. Si vous pointez votre souris sur la zone de texte txtCardColor, une option de tâche guidée apparaît le permet de vous permet d’ajouter un extendeur (voir Figure 3). Si vous sélectionnez cette option, l’Assistant de l’extendeur s’affiche (voir Figure 4).


[![Ajout d’un extendeur](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)

**Figure 03**: ajout d’un extendeur ([cliquez pour afficher l’image en taille réelle](using-the-colorpicker-control-extender-cs/_static/image6.png))


[![Sélection d’un extendeur de contrôle avec l’Assistant d’extendeur](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)

**Figure 04**: sélection d’un extendeur de contrôle avec l’Assistant d’extendeur ([cliquez pour afficher l’image en taille réelle](using-the-colorpicker-control-extender-cs/_static/image8.png))


Vous pouvez choisir l’extendeur ColorPicker pour étendre la zone de texte txtCardColor avec l’extendeur ColorPicker. Cliquez sur OK pour fermer la boîte de dialogue.

Après avoir apporté ces modifications, la source de la page ressemble à la liste 2.

Listing 2 - CreateCard.aspx (avec ColorPicker)

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

Notez que la page contienne désormais un contrôle ColorPickerExtender qui s’affiche directement sous le contrôle de zone de texte txtCardColor. Le contrôle ColorPickerExtender étend le contrôle txtCardColor afin qu’il affiche une boîte de dialogue de sélecteur de couleur.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>À l’aide d’un bouton pour lancer la boîte de dialogue de sélecteur de couleurs

L’extendeur ColorPicker prend en charge les propriétés suivantes :

- PopupButtonId - l’ID d’un bouton sur la page qui provoque la boîte de dialogue du sélecteur de couleur d’apparaissent.
- PopupPosition - la position, par rapport au contrôle cible, de la boîte de dialogue de sélecteur de couleur. Les valeurs possibles sont absolues, Center, BottomLeft, BottomRight, TopLeft, droit, droit et gauche (la valeur par défaut est BottomLeft).
- SampleControlId - l’ID d’un contrôle qui affiche la couleur sélectionnée.
- SelectedColor - couleur initiale sélectionnée par le composant ColorPicker.

Vous pouvez utiliser ces propriétés pour personnaliser le mode d’affichage de la boîte de dialogue de sélecteur de couleur et la façon dont la couleur sélectionnée s’affiche. La page dans le Listing 3 illustre comment vous pouvez utiliser plusieurs de ces propriétés.

**Liste 3 - CreateCardButton.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

La page dans le Listing 3 inclut un choisir la couleur du bouton (voir Figure 5). Lorsque vous cliquez sur ce bouton, la boîte de dialogue de sélecteur de couleur s’affiche au-dessus de la zone de texte. Si vous sélectionnez une couleur dans la boîte de dialogue la couleur sélectionnée s’affiche en tant que la couleur d’arrière-plan de la lblSample contrôle Label.

La propriété PopupButtonID de ColorPicker est utilisée pour associer le bouton Choisir la couleur de l’extendeur ColorPicker. Lorsque vous fournissez une valeur pour la propriété PopupButtonID, la boîte de dialogue de sélecteur de couleurs n’apparaît plus quand le contrôle cible a le focus. Vous devez cliquer sur le bouton pour afficher la boîte de dialogue.

La propriété SampleControlID est utilisée pour associer un contrôle qui affiche la couleur sélectionnée avec le composant ColorPicker. Le composant ColorPicker modifie la couleur d’arrière-plan de ce contrôle pour la couleur actuellement sélectionnée.


[![Affichage de la boîte de dialogue du sélecteur de couleur avec un bouton](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)

**Figure 05**: affichage de la boîte de dialogue du sélecteur de couleur avec un bouton ([cliquez pour afficher l’image en taille réelle](using-the-colorpicker-control-extender-cs/_static/image10.png))


## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez appris à utiliser l’extendeur du contrôle ColorPicker pour afficher une boîte de dialogue Sélecteur de couleurs contextuelle. Tout d’abord, nous avons examiné comment vous pouvez afficher la boîte de dialogue lorsque le focus est déplacé à un contrôle de zone de texte. Ensuite, vous avez appris à créer un bouton qui affiche la boîte de dialogue de sélecteur de couleur lorsque le bouton est activé.

> [!div class="step-by-step"]
> [Next](using-the-colorpicker-control-extender-vb.md)
