---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Changement d’une Animation à l’aide de Code côté Client (c#) | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. L’animation peut également...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 4bf480401e244661e2c316adcde3cbde647a6dc3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393072"
---
<a name="changing-an-animation-using-client-side-code-c"></a>Changement d’une Animation à l’aide de Code côté Client (c#)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)

> Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. L’animation peut également être modifiée à l’aide d’un code JavaScript côté client personnalisé.


## <a name="overview"></a>Vue d'ensemble

Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. L’animation peut également être modifiée à l’aide d’un code JavaScript côté client personnalisé.

## <a name="steps"></a>Étapes

Tout d’abord inclure le `ScriptManager` dans la page ; ensuite, la bibliothèque AJAX ASP.NET est chargée, ce qui permet d’utiliser les outils de contrôle :

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

L’animation sera appliquée à un volet de texte qui ressemble à ceci :

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

Dans la classe CSS associée pour le panneau, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau :

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

L’animation proprement dite est lancée par un bouton HTML :

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

Notez qu’il existe aucune `<Animations>` nœud dans le `AnimationExtender` contrôle. Code JavaScript personnalisé est utilisé pour fournir les animations à utiliser avec le contrôle.

Comme avec l’API du serveur de `AnimationExtender`, il n’existe aucun moyen facile pour assigner une animation pour l’extendeur encore. Toutefois l’extendeur expose plusieurs méthodes pour lire et écrire des animations inscrit avec divers événements (`OnClick`, `OnLoad`, et ainsi de suite). Voici quelques exemples :

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

Le format de la valeur de retour de la `get_*()` fonctions et le format de l’argument pour le `set_*()` functions est une chaîne JSON, en fournissant une représentation d’objet de ce que serait le balisage XML. Actuellement, il n’existe aucun moyen de passer un objet dans, mais il est possible de lire un objet à partir d’une animation donnée (`get_OnXXXBehavior()` méthodes).

Voici une chaîne JSON (sans les guillemets de délimitation et bien formatées) représentant une animation déclenchée par le bouton, mais animer le volet en redimensionnant et en atténuant progressivement en même temps :

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

Le code JavaScript suivant assigne cette descripting JSON pour le `OnClick` animation de l’extendeur actuel et l’exécute :

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


[![L’animation s’exécute immédiatement, sans un clic de souris (et avec très peu de balisage)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)

L’animation s’exécute immédiatement, sans un clic de souris (et avec très peu de balisage) ([cliquez pour afficher l’image en taille réelle](changing-an-animation-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](executing-animations-using-client-side-code-cs.md)
> [Suivant](animating-an-updatepanel-control-cs.md)
