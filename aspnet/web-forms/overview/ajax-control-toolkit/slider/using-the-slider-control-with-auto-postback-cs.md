---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: À l’aide du contrôle Slider avec publication automatique (c#) | Microsoft Docs
author: wenz
description: Le contrôle Slider dans AJAX Control Toolkit fournit un contrôle slider graphique qui peut être contrôlé à l’aide de la souris. Il est possible de rendre la comptabilisation de curseur automatique...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: 4f776d609099c398b4937710487ba51a5efb1876
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823665"
---
<a name="using-the-slider-control-with-auto-postback-c"></a>À l’aide du contrôle Slider avec publication automatique (c#)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)

> Le contrôle Slider dans AJAX Control Toolkit fournit un contrôle slider graphique qui peut être contrôlé à l’aide de la souris. Il est possible d’effectuer l’autopostback curseur une fois sa valeur change.


## <a name="overview"></a>Vue d'ensemble

Le contrôle Slider dans AJAX Control Toolkit fournit un contrôle slider graphique qui peut être contrôlé à l’aide de la souris. Il est possible d’effectuer l’autopostback curseur une fois sa valeur change.

## <a name="steps"></a>Étapes

Afin de rendre le curseur automatiquement publication (postback) à une modification, les deux zones de texte besoin de l’attribut `AutoPostBack="true"`: la zone de texte qui deviendra le curseur lui-même et la zone de texte qui contient la position du curseur. Voici le balisage requis pour ce faire :

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

Le `SliderExtender` contrôle à partir d’ASP.NET AJAX Control Toolkit affecte les fonctionnalités de curseur à deux zones de texte :

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

Un élément label supplémentaires est utilisé ultérieurement pour informer l’utilisateur d’une publication (postback) :

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

Enfin, le `ScriptManager` contrôle d’ASP.NET AJAX charge le code JavaScript nécessaire pour les outils de contrôle fonctionne :

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

Maintenant le curseur est publication ; sur le côté serveur, cet événement peut être intercepté et traité en :

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


[![En déplaçant le curseur déclenche une publication (postback)](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)

En déplaçant le curseur déclenche une publication (postback) ([cliquez pour afficher l’image en taille réelle](using-the-slider-control-with-auto-postback-cs/_static/image3.png))


[![Par la suite, la date de cette modification est écrite dans l’étiquette](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)

Par la suite, la date de cette modification est écrite dans l’étiquette ([cliquez pour afficher l’image en taille réelle](using-the-slider-control-with-auto-postback-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Next](databinding-the-slider-control-cs.md)
