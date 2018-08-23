---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: Exécution d’Animations avec Code côté Client (c#) | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. L’exécution de l’animation...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 5dc5c0b49a3530988bf42d6d632a061622f0a217
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836163"
---
<a name="executing-animations-using-client-side-code-c"></a>Animations en cours d’exécution à l’aide de Code côté Client (c#)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)

> Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. L’exécution de l’animation peut également être déclenchée à l’aide d’un code JavaScript côté client personnalisé.


## <a name="overview"></a>Vue d'ensemble

Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. L’exécution de l’animation peut également être déclenchée à l’aide d’un code JavaScript côté client personnalisé.

## <a name="steps"></a>Étapes

Tout d’abord inclure le `ScriptManager` dans la page ; ensuite, la bibliothèque AJAX ASP.NET est chargée, ce qui permet d’utiliser les outils de contrôle :

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

L’animation sera appliquée à un volet de texte qui ressemble à ceci :

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

Dans la classe CSS associée pour le panneau, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau :

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

Dans le `<Animations>` nœud, utilisez `<OnClick>` pour exécuter les animations une fois l’utilisateur clique sur le panneau de configuration. Ajoutez deux animations doit être exécuté en :

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

Fins de démonstration, cette animation (et les autres animations créées à l’aide de la boîte à outils de contrôle) sont exécutées à l’aide de code JavaScript, une fois que la page s’exécute. Tout d’abord, nous devons accéder à la `AnimationExtender` contrôle. La bibliothèque ASP.NET AJAX fournit le `$find()` (fonction) pour cette tâche :

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

Le `AnimationExtender` contrôle expose une API riche, y compris les méthodes avec des noms identiques aux gestionnaires d’événements utilisés dans le balisage XML : `OnClick()`, `OnLoad()`, et ainsi de suite. Par exemple, un appel de la `OnClick()` méthode exécute l’animation dans le `<OnClick>` élément de la `AnimationExtender` contrôle :

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

Voici le code JavaScript côté client complet qui émule le, cliquez sur le panneau de configuration une fois que la page a été entièrement chargée Notez que le `pageLoad()` nom de la fonction est utilisé, qui est appelée par ASP.NET AJAX une fois la page et JavaScript bibliothèques ont été inclus chargé.

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]


[![L’animation s’exécute immédiatement, sans un clic de souris](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)

L’animation s’exécute immédiatement, sans un clic de souris ([cliquez pour afficher l’image en taille réelle](executing-animations-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](modifying-animations-from-the-server-side-cs.md)
> [Suivant](changing-an-animation-using-client-side-code-cs.md)
