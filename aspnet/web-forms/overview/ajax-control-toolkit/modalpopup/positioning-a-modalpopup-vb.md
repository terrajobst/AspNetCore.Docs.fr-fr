---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: Positionnement d’un ModalPopup (VB) | Microsoft Docs
author: wenz
description: Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client. Toutefois le contrôle n’offre pas un...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: e36d22b5c2f503ed849f373153e263bdd9546452
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821759"
---
<a name="positioning-a-modalpopup-vb"></a>Positionnement d’un ModalPopup (VB)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)

> Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client. Toutefois le contrôle n’offre pas une fonctionnalité intégrée pour positionner la fenêtre contextuelle.


## <a name="overview"></a>Vue d'ensemble

Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client. Toutefois le contrôle n’offre pas une fonctionnalité intégrée pour positionner la fenêtre contextuelle.

## <a name="steps"></a>Étapes

Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager`. contrôle doit être placé n’importe où sur la page (mais dans les limites du `<form>` élément) :

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

Ensuite, ajoutez un panneau qui sert de la fenêtre contextuelle modale. Un bouton est utilisé pour fermer la fenêtre contextuelle :

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

Chaque fois que la fenêtre contextuelle s’affiche, il doit être positionné sur un lieu donné dans la page. Pour cette tâche, une fonction de JavaScript côté client est créée. Il essaie tout d’abord accéder au panneau. Si elle réussit, la position du panneau est définie à l’aide de CSS et JavaScript (modification de la position de la fenêtre contextuelle à sera). Toutefois, le `ModalPopupExtender` contrôle tente également de positionner la fenêtre contextuelle. Par conséquent, le code JavaScript positionne à plusieurs reprises de la fenêtre contextuelle, chaque dixième de seconde.

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

Comme vous pouvez le voir, la valeur de retour de la `setTimeout()` méthode JavaScript est enregistré dans une variable globale. Cela permet d’arrêter le positionnement répétées de la fenêtre contextuelle à la demande, à l’aide de la `clearTimeout()` méthode :

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

Maintenant, tout ce qui reste à faire consiste à appeler ces fonctions dès que possible le navigateur. Le `movePanel()` fonction JavaScript doit être appelée lorsque le bouton qui déclenche le panneau :

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

Et le `stopMoving()` fonction qu’intervient la fermeture de la fenêtre contextuelle peut être déclenchée dans le `ModalPopupExtender` contrôle :

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


[![La fenêtre contextuelle modale s’affiche à la position désignée](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)

La fenêtre contextuelle modale s’affiche à la position désignée ([cliquez pour afficher l’image en taille réelle](positioning-a-modalpopup-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](handling-postbacks-from-a-modalpopup-vb.md)
