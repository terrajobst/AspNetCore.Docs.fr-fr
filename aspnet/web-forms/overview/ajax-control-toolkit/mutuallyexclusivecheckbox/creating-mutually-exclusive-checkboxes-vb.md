---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: Création de cases à cocher mutuellement exclusives (VB) | Microsoft Docs
author: wenz
description: 'Lorsque seul un ensemble d’options peuvent être sélectionnés, les cases d’option sont généralement utilisées. Il y a cependant un inconvénient, : une fois sélectionnée, une case d’option dans un groupe...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: fd338b622779b64dd59f9cf6f3e2365ef5cb3ffb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823659"
---
<a name="creating-mutually-exclusive-checkboxes-vb"></a>Création de cases à cocher mutuellement exclusives (VB)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)

> Lorsque seul un ensemble d’options peuvent être sélectionnés, les cases d’option sont généralement utilisées. Il y a cependant un inconvénient, : une fois qu’une case d’option dans un groupe est sélectionnée, il n’est pas possible de désactiver toutes les cases d’option. Cases à cocher peut être désactivées à tout moment, toutefois, ne sont pas mutuellement exclusives. Ce didacticiel offre le meilleur des deux approches : cases à cocher qui s’excluent mutuellement.


## <a name="overview"></a>Vue d'ensemble

Lorsque seul un ensemble d’options peuvent être sélectionnés, les cases d’option sont généralement utilisées. Il y a cependant un inconvénient, : une fois qu’une case d’option dans un groupe est sélectionnée, il n’est pas possible de désactiver toutes les cases d’option. Cases à cocher peut être désactivées à tout moment, toutefois, ne sont pas mutuellement exclusives. Ce didacticiel offre le meilleur des deux approches : cases à cocher qui s’excluent mutuellement.

## <a name="steps"></a>Étapes

ASP.NET AJAX Control Toolkit contient les extendeurs MutuallyExclusiveCheckBox. Cela permet aux programmeurs d’affecter n’importe quel case à cocher à un nom de groupe (`Key` attribut). À partir de toutes les cases à cocher dans le même groupe, seul l’un peut être sélectionné à la fois.

Commençons par placer les deux cases à cocher sur une page ASP.NET. Il peut y avoir plus, mais deux d'entre eux suffit pour illustrer le principe :

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

Pour les deux cases à cocher, vous devez placer un contrôle MutuallyExclusiveCheckBoxExtender sur la page. Les deux attributs de clé doivent ont la même valeur, tout comme la valeur d’attributs des éléments de bouton de case d’option HTML doivent être identiques à indiquer le groupe qu'auquel ils appartiennent. La propriété TargetControlID de l’extendeur pointe vers l’ID de la case à cocher.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

Enfin, inclure ASP.NET AJAX `ScriptManager` qui est requis par tous les éléments d’ASP.NET AJAX Control Toolkit :

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

Enregistrez et exécutez la page : vous pouvez vérifier et décochez les deux cases à cocher, toutefois à aucun moment peut les deux cases à cocher être vérifiées.


[![Case à cocher qu’une seule peut être vérifiée à la fois](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)

Case à cocher qu’une seule peut être vérifiée à la fois ([cliquez pour afficher l’image en taille réelle](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](creating-mutually-exclusive-checkboxes-cs.md)
