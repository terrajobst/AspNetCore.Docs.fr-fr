---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: Création d’un numérique haut/bas contrôle avec un Backend de Service Web (VB) | Microsoft Docs
author: wenz
description: Au lieu de laisser un utilisateur de taper une valeur dans une case à cocher, contrôle (qui existe sur Windows et d’autres systèmes d’exploitation) haut/bas numérique peut s’avérer plus que c...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: bebf70093dacb81331c009c6642c2f2d649b8567
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805371"
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a>Création d’un contrôle haut/bas numérique avec un Backend de Service Web (VB)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)

> Au lieu de laisser un utilisateur de taper une valeur dans une case à cocher, une valeur numérique haut/bas contrôle (qui existe sur Windows et d’autres systèmes d’exploitation) peut s’avérer plus à l’aise. Par défaut, le contrôle NumericUpDown toujours augmente ou diminue d’une valeur de 1, mais un service web s’avère plus de souplesse.


## <a name="overview"></a>Vue d'ensemble

Au lieu de laisser un utilisateur de taper une valeur dans une case à cocher, une valeur numérique haut/bas contrôle (qui existe sur Windows et d’autres systèmes d’exploitation) peut s’avérer plus à l’aise. Par défaut, le `NumericUpDown` contrôle toujours augmente ou diminue d’une valeur de 1, mais un service web s’avère plus de souplesse.

## <a name="steps"></a>Étapes

ASP.NET AJAX Control Toolkit contient le `NumericUpDown` extendeur qui ajoute automatiquement les deux boutons à une zone de texte : une pour augmenter sa valeur, un pour réduire ce. Toutefois le contrôle prend également en charge un appel de service web (ou l’appel de méthode de page). Chaque fois que le haut ou vers le bas du bouton est activé, le code JavaScript de code se connecte au serveur web et exécute une méthode il. La signature de méthode est celle qui suit :

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

Le `current` argument est la valeur actuelle dans la zone de texte ; le `tag` attribut est des données de contexte supplémentaire qui peuvent être définies en tant que propriété de la `NumericUpDown` extendeur (mais n’est pas obligatoire).

Pour cet exemple, numérique contrôle NumericUpDown doit autoriser uniquement les valeurs qui sont des puissances de deux : 1, 2, 4, 8, 16, 32, 64 et ainsi de suite. Par conséquent, la méthode exécutée lorsque l’utilisateur souhaite augmenter la valeur doit double l’ancienne valeur ; l’autre méthode doit diviser la valeur par deux. Voici donc le service web complète :

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

Enfin, créez une nouvelle page ASP.NET. Comme d’habitude, vous devez un `ScriptManager` contrôle, un `TextBox` contrôle et un `NumericUpDownExtender` contrôle. Pour ce dernier, vous devez fournir les informations de service web :

- `ServiceDownMethod` nom de la bas méthode web ou page (méthode)
- `ServiceDownPath` chemin d’accès au service web avec la méthode de service vers le bas ; omettre si vous utilisez une méthode de page
- `ServiceUpMethod` partie du nom de méthode web ou de page (méthode)
- `ServiceUpPath` chemin d’accès au service web avec la méthode de service opérationnel ; omettre si vous utilisez une méthode de page

Voici le balisage complet pour la page :

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

Si vous exécutez la page, notez la façon dont la valeur dans la zone de texte double toujours lorsque vous cliquez sur le bouton du haut, êtes réduit de moitié lorsque vous cliquez sur le bouton du bas.


[![Seuls des chiffres qui sont une puissance de 2 apparaissent](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)

Seuls des chiffres qui sont une puissance de 2 apparaissent ([cliquez pour afficher l’image en taille réelle](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
