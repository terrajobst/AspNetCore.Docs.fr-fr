---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: Gestion des publications (postback) à partir d’un contrôle Popup sans un UpdatePanel (VB) | Microsoft Docs
author: wenz
description: L’extendeur PopupControl dans les outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé. Lorsqu’une publication (postback) se produit en unités de recherche...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 6dcb2b09279ec6400465f79fadc2a1b6c72f8f07
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830189"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a>Gestion des publications (postback) à partir d’un contrôle Popup sans un UpdatePanel (VB)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)

> L’extendeur PopupControl dans les outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé. Lorsqu’une publication (postback) se produit dans un tel panneau et il existe plusieurs panneaux sur la page, il est difficile de déterminer le panneau qui a été cliqué.


## <a name="overview"></a>Vue d'ensemble

L’extendeur PopupControl dans les outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé. Lorsqu’une publication (postback) se produit dans un tel panneau et il existe plusieurs panneaux sur la page, il est difficile de déterminer le panneau qui a été cliqué.

## <a name="steps"></a>Étapes

Lorsque vous utilisez un `PopupControl` avec une publication (postback), mais sans avoir un `UpdatePanel` dans la page, les outils de contrôle n’offre pas un moyen de déterminer quel élément client a déclenché la fenêtre contextuelle qui à son tour a provoqué la publication (postback). Toutefois, une petite astuce fournit une solution de contournement pour ce scénario.

Tout d’abord, voici la configuration de base : deux zones de texte qui tous deux de déclenchent la fenêtre contextuelle même, un calendrier. Deux `PopupControlExtenders` rassembler les zones de texte et la fenêtre contextuelle.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

L’idée de base consiste à ajouter un champ de formulaire masqué dans le &lt; `form` &gt; élément qui contient la zone de texte qui a lancé la fenêtre contextuelle :

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

Lorsque la page est chargée, le code JavaScript ajoute un gestionnaire d’événements pour les deux zones de texte : chaque fois que l’utilisateur clique sur une zone de texte, son nom est écrit dans le champ de formulaire masqué :

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

Dans le code côté serveur, la valeur du champ masqué doit être lues. Dans la mesure où les champs de formulaire masqués sont faciles à manipuler, une approche de la liste verte pour valider la valeur masquée est requise. Une fois que la zone de texte correct a été identifiée, la date dans le calendrier est écrit dedans.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]


[![Le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)

Le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte ([cliquez pour afficher l’image en taille réelle](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))


[![En cliquant sur une date de la place dans la zone de texte](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)

En cliquant sur une date de la place dans la zone de texte ([cliquez pour afficher l’image en taille réelle](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Précédent](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
