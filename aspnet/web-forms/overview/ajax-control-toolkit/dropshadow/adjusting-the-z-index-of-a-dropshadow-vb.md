---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: Ajustement de l’Index-Z d’un DropShadow (VB) | Microsoft Docs
author: wenz
description: Le contrôle DropShadow dans AJAX Control Toolkit étend un panneau avec une ombre portée. Toutefois cette ombre parfois est en conflit avec d’autres contrôles, pour le programme d’insta...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: 78697f51a09dfaad315255efa23120d4c456bfea
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829542"
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a>Ajustement de l’Index-Z d’un DropShadow (VB)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)

> Le contrôle DropShadow dans AJAX Control Toolkit étend un panneau avec une ombre portée. Toutefois cette ombre parfois entre en conflit avec d’autres contrôles, par exemple le contrôle de Menu ASP.NET. Quand une entrée de menu s’affiche, il apparaît derrière l’ombre portée.


## <a name="overview"></a>Vue d'ensemble

Le contrôle DropShadow dans AJAX Control Toolkit étend un panneau avec une ombre portée. Toutefois cette ombre parfois entre en conflit avec d’autres contrôles, par exemple le contrôle de Menu ASP.NET. Quand une entrée de menu s’affiche, il apparaît derrière l’ombre portée.

## <a name="steps"></a>Étapes

Le code commence avec le panneau proprement dit, contenant suffisamment de texte afin que le panneau contient suffisamment de texte pour l’effet soit visible :

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

Un autre panneau est placé directement avant le `panelShadow` Panneau de configuration. Il contient un menu avec l’orientation horizontale afin que les entrées de menu s’affiche au-dessus (ou plutôt : sous) le `dropShadow` Panneau de configuration) :

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

Ensuite, le `DropShadowExtender` est ajouté pour étendre le `panelShadow` panneau avec un effet d’ombre portée :

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

Enfin, ASP.NET AJAX `ScriptManager` contrôle permet de la boîte à outils de contrôle fonctionne :

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

Lorsque vous exécutez ce script, les entrées de menu apparaissent sous le panneau de configuration. Toutefois le menu utilise la classe CSS `panel` où vous devez simplement définir deux choses pour rendre les éléments apparaissent devant l’autre panneau :

- Positionnement relatif
- Un index-z positif

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

Ensuite, le `DropShadowExtender` contrôle n’est pas en conflit plus avec le contrôle de Menu.


[![Avant : L’entrée de menu n’est pas visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)

Avant : L’entrée de menu n’est pas visible ([cliquez pour afficher l’image en taille réelle](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))


[![Après : L’entrée de menu s’affiche](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)

Après : L’entrée de menu s’affiche ([cliquez pour afficher l’image en taille réelle](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Précédent](manipulating-dropshadow-properties-from-client-code-cs.md)
> [Suivant](manipulating-dropshadow-properties-from-client-code-vb.md)
