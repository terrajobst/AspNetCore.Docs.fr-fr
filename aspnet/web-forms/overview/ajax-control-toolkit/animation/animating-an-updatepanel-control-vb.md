---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Animation d’un contrôle UpdatePanel (VB) | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Pour le contenu d’un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 797ee37eb440bed261403aa0e1b68f38d3cd8ef9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833836"
---
<a name="animating-an-updatepanel-control-vb"></a>Animation d’un contrôle UpdatePanel (VB)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)

> Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Pour le contenu d’un UpdatePanel, il existe un extendeur spéciaux qui s’appuie fortement sur l’infrastructure d’animation : UpdatePanelAnimation. Ce didacticiel montre comment configurer ce type d’une animation pour un UpdatePanel.


## <a name="overview"></a>Vue d'ensemble

Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Pour le contenu d’un `UpdatePanel`, un extendeur spécial existe qui s’appuie fortement sur l’infrastructure d’animation : `UpdatePanelAnimation`. Ce didacticiel montre comment configurer ce type d’une animation pour un `UpdatePanel`.

## <a name="steps"></a>Étapes

La première étape consiste comme d’habitude à inclure le `ScriptManager` dans la page afin que la bibliothèque ASP.NET AJAX est chargée et les outils de contrôle peut être utilisé :

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

L’animation dans ce scénario sera appliquée à un ASP.NET `Wizard` contrôle web résidant dans un `UpdatePanel`. Trois étapes (arbitraires) offrent suffisamment déclenchent des publications :

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

Le balisage nécessaire pour le `UpdatePanelAnimationExtender` contrôle est assez semblable à celui utilisé pour le `AnimationExtender`. Dans le `TargetControlID` attribut nous fournissons le `ID` de la `UpdatePanel` pour animer ; dans le `UpdatePanelAnimationExtender` contrôle, le `<Animations>` élément contient le balisage XML de l’animation. Toutefois, il existe une différence : la quantité d’événements et gestionnaires d’événements est limitée par rapport à `AnimationExtender`. Pour `UpdatePanels`, seuls deux d'entre elles existe :

- `<OnUpdated>` Lorsque le contrôle UpdatePanel a été mis à jour
- `<OnUpdating>` Lorsque le contrôle UpdatePanel démarre la mise à jour

Dans ce scénario, le nouveau contenu de la `UpdatePanel` (après la publication (postback)) est apparition en fondu. Voici le balisage nécessaire pour ce faire :

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

Maintenant chaque fois qu’une publication (postback) se produit dans le contrôle UpdatePanel, le nouveau contenu du panneau Fondu léger.


[![L’étape suivante de l’Assistant est fondu](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)

L’étape suivante de l’Assistant est fondu ([cliquez pour afficher l’image en taille réelle](animating-an-updatepanel-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](changing-an-animation-using-client-side-code-vb.md)
> [Suivant](dynamically-controlling-updatepanel-animations-vb.md)
