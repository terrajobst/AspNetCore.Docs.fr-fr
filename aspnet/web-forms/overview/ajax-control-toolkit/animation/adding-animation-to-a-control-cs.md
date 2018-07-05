---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Ajout d’une Animation à un contrôle (c#) | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Ce didacticiel montre comment...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 80c982e041af2d0b9ee789665613ced0311dfcc9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396570"
---
<a name="adding-animation-to-a-control-c"></a>Ajout d’une Animation à un contrôle (c#)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)

> Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Ce didacticiel montre comment configurer une animation de ce type.


## <a name="overview"></a>Vue d'ensemble

Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Ce didacticiel montre comment configurer une animation de ce type.

## <a name="steps"></a>Étapes

La première étape consiste comme d’habitude à inclure le `ScriptManager` dans la page afin que la bibliothèque ASP.NET AJAX est chargée et les outils de contrôle peut être utilisé :

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

L’animation dans ce scénario sera appliquée à un volet de texte qui ressemble à ceci :

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

La classe CSS associée pour le panneau définit une couleur d’arrière-plan et une largeur :

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

Ensuite, nous devons le `AnimationExtender`. Après avoir entré un `ID` et habituelles `runat="server"`, le `TargetControlID` attribut doit être défini pour le contrôle pour animer dans notre cas, le panneau de configuration :

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

L’animation entière est appliquée de manière déclarative, à l’aide d’une syntaxe XML, malheureusement actuellement pas entièrement pris en charge par IntelliSense de Visual Studio. Le nœud racine est `<Animations>;` au sein de ce nœud, plusieurs événements sont autorisées qui déterminent quand l’animation prennent place :

- `OnClick` (clic de souris)
- `OnHoverOut` (lorsque la souris quitte un contrôle)
- `OnHoverOver` (lorsque la souris pointe sur un contrôle, l’arrêt du `OnHoverOut` animation)
- `OnLoad` (lorsque la page a été chargée)
- `OnMouseOut` (lorsque la souris quitte un contrôle)
- `OnMouseOver` (lorsque la souris pointe sur un contrôle, ne pas l’arrêt du `OnMouseOut` animation)

Le framework est fourni avec un ensemble d’animations, chacun d’eux représenté par son propre élément XML. Voici une sélection :

- `<Color>` (modification d’une couleur)
- `<FadeIn>` (fondu dans)
- `<FadeOut>` (fondu)
- `<Property>` (modification de propriété d’un contrôle)
- `<Pulse>` (impulsions)
- `<Resize>` (modification de la taille)
- `<Scale>` (proportionnellement à la taille de la variation)

Dans cet exemple, le panneau est disparition en fondu. L’animation prend 1,5 secondes (`Duration` attribut), affichage des 24 images (étapes d’animation) par seconde (`Fps` attributs). Voici le balisage complet pour le `AnimationExtender` contrôle :

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

Lorsque vous exécutez ce script, le panneau s’affiche et fondu en quelques secondes et un demi.


[![Le panneau est fondu](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)

Le panneau est fondu ([cliquez pour afficher l’image en taille réelle](adding-animation-to-a-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](executing-several-animations-at-the-same-time-cs.md)
