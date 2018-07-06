---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: Création d’un contrôle Rating (c#) | Microsoft Docs
author: wenz
description: De nombreux sites Web, à partir de commerce électronique aux sites de Communauté, offrent aux utilisateurs d’articles de taux ou éléments. Cela nécessite généralement des efforts de codage, mais nous avons le...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 67c66d2a28a247493a0b1ea67e15935c27af80ae
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830952"
---
<a name="creating-a-rating-control-c"></a>Création d’un contrôle Rating (c#)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)

> De nombreux sites Web, à partir de commerce électronique aux sites de Communauté, offrent aux utilisateurs d’articles de taux ou éléments. Cela nécessite généralement des efforts de codage, mais nous avons les outils de contrôle à notre disposition.


## <a name="overview"></a>Vue d'ensemble

De nombreux sites Web, à partir de commerce électronique aux sites de Communauté, offrent aux utilisateurs d’articles de taux ou éléments. Cela nécessite généralement des efforts de codage, mais nous avons les outils de contrôle à notre disposition.

## <a name="steps"></a>Étapes

Tout d’abord, vous avez besoin (au moins) deux types d’images : une pour rempli les élément de contrôle d’accès et l’autre pour un élément de contrôle d’accès vide. Un élément de contrôle d’accès est généralement une étoile ou un sourire. Pour ce scénario, vous trouvez trois fichiers, smiley.png et empty.png et émoticône-done.png dans le cadre des téléchargements de code source pour ce didacticiel.

Ensuite, créez un fichier ASP.NET et commencez par ajouter un `ScriptManager` contrôle :

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

Ensuite, ajoutez le `Rating` contrôle à partir d’ASP.NET AJAX Control Toolkit. Les attributs suivants doivent être définies pour cet exemple :

- `CurrentRating` l’évaluation initiale à utiliser
- `MaxRating` la classification maximale
- `EmptyStarCssClass` la classe CSS à utiliser lorsqu’un élément d’évaluation (étoiles) est vide
- `FilledStarCssClass` la classe CSS à utiliser lors du remplissage un élément d’évaluation (étoiles)
- `StarCssClass` la classe CSS à utiliser pour un stat visible
- `WaitingStarCssClass` la classe CSS à utiliser pendant un nombre d’étoiles est envoyé sur le serveur

Et Voici le balisage qui crée un contrôle rating avec cinq éléments (smileys) dont aucun n’est remplie initialement :

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

Les trois classes CSS référencés devant maintenant afficher les fichiers image appropriée, qui est facile à réaliser à l’aide de CSS :

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

Assurez-vous que vous fournissez la largeur et la hauteur des trois images, sinon l’affichage peut sembler un peu tordue.

Enfin, le résultat de l’évaluation doit être affiché à l’utilisateur (ou, au moins enregistré dans une base de données). Par conséquent, ajoutez une étiquette pour la sortie d’un message texte et un bouton Envoyer le formulaire de contrôle d’accès au serveur de publication :

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

Dans le code côté serveur, accéder au contrôle d’évaluation par le biais de son `ID` , puis accédez à son `CurrentRating` propriété qui est le nombre d’éléments de contrôle d’accès sélectionné, dans notre exemple, une valeur comprise entre 0 et 5.

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

Enregistrez la page et chargez-le dans votre navigateur. Lorsque vous pointez sur les éléments de contrôle d’accès (initialement vide), un effet de JavaScript se produit : les modifications de contrôle d’accès. Lorsque vous cliquez sur l’ensemble des étoiles, l’évaluation actuelle est conservée. Enfin, lorsque vous envoyez le formulaire, le code côté serveur génère le classement sélectionné.


[![Création d’un système d’évaluation avec un minimum de code](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)

Création d’un système d’évaluation avec un minimum de code ([cliquez pour afficher l’image en taille réelle](creating-a-rating-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](creating-a-rating-control-vb.md)
