---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
title: Utilisation de TextBoxWatermark avec des contrôles de Validation (VB) | Microsoft Docs
author: wenz
description: Le contrôle TextBoxWatermark dans AJAX Control Toolkit étend une zone de texte afin qu’un texte est affiché dans la zone. Lorsqu’un utilisateur clique dans la zone, il je...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e6c2cb98-f745-4bc8-973a-813879c8a891
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 636a9d00b4f699536d2851d3bac5f657c272c80a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838087"
---
<a name="using-textboxwatermark-with-validation-controls-vb"></a>Utilisation de TextBoxWatermark avec des contrôles de Validation (VB)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)

> Le contrôle TextBoxWatermark dans AJAX Control Toolkit étend une zone de texte afin qu’un texte est affiché dans la zone. Lorsqu’un utilisateur clique dans la zone, elle est vidée. Si l’utilisateur laisse la zone sans saisie de texte, le texte prérempli réapparaît. Cela peut entrer en conflit avec des contrôles de Validation ASP.NET sur la même page, mais ces problèmes peuvent être surmontés.


## <a name="overview"></a>Vue d'ensemble

Le `TextBoxWatermark` contrôle dans AJAX Control Toolkit étend une zone de texte afin qu’un texte est affiché dans la zone. Lorsqu’un utilisateur clique dans la zone, elle est vidée. Si l’utilisateur laisse la zone sans saisie de texte, le texte prérempli réapparaît. Cela peut entrer en conflit avec des contrôles de Validation ASP.NET sur la même page, mais ces problèmes peuvent être surmontés.

## <a name="steps"></a>Étapes

La configuration de base de l’exemple est le suivant : un `TextBox` contrôle est lui appliquer un filigrane à l’aide un `TextBoxWatermarkExtender` contrôle. Un bouton déclenche une publication (postback) et est utilisé ultérieurement pour déclencher les contrôles de validation sur la page. En outre, un `ScriptManager` contrôle est requis pour initialiser d’ASP.NET AJAX :

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample1.aspx)]

Ajoutez maintenant un `RequiredFieldValidator` contrôle qui vérifie si le champ n’est texte lorsque le formulaire est envoyé. Le `InitialValue` propriété du validateur doit être définie sur la même valeur que celle qui est utilisée dans le `TextBoxWatermarkExtender` contrôle : quand le formulaire est envoyé, la valeur d’une zone de texte inchangé est la valeur de filigrane qu’il contient :

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample2.aspx)]

Toutefois, il existe un problème avec cette approche : si le client désactive JavaScript, le champ de texte n’est pas donc prérempli avec le texte de filigrane, les `RequiredFieldValidator` ne déclenche pas d’un message d’erreur. Par conséquent, une seconde `RequiredFieldValidator` contrôle est requise qui vérifie une zone de texte vide (en omettant la `InitialValue` attribut).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample3.aspx)]

Étant donné que les deux validateurs utilisent `Display` = `"Dynamic"`, l’utilisateur final ne peut pas distinguer à partir de l’apparence visuelle parmi les deux validateurs a été déclenché ; au lieu de cela, il semblerait qu’un seul d'entre eux.

Enfin, ajoutez du code côté serveur pour le texte dans le champ de sortie si aucun validateur n’émis un message d’erreur :

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample4.aspx)]


[![Le programme de validation se plaint qu’il n’existe aucun texte dans le champ](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)

Le programme de validation se plaint qu’il n’existe aucun texte dans le champ ([cliquez pour afficher l’image en taille réelle](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](using-textboxwatermark-in-a-formview-vb.md)
