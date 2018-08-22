---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: Remplissage dynamique d’un contrôle à l’aide de Code JavaScript (c#) | Microsoft Docs
author: wenz
description: Le contrôle de DynamicPopulate dans ASP.NET AJAX Control Toolkit appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible sur t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: e8b49f41f132cc31ca458ce0af3b74dbb54f225e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830988"
---
<a name="dynamically-populating-a-control-using-javascript-code-c"></a>Remplissage dynamique d’un contrôle à l’aide de Code JavaScript (c#)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)

> Le contrôle DynamicPopulate dans ASP.NET AJAX Control Toolkit appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible dans la page, sans une actualisation de la page. Il est également possible de déclencher le remplissage à l’aide d’un code JavaScript côté client personnalisé.


## <a name="overview"></a>Vue d'ensemble

Le `DynamicPopulate` contrôle dans ASP.NET AJAX Control Toolkit appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible dans la page, sans une actualisation de la page. Il est également possible de déclencher le remplissage à l’aide d’un code JavaScript côté client personnalisé.

## <a name="steps"></a>Étapes

Tout d’abord, vous avez besoin d’un Service Web de ASP.NET qui implémente la méthode doit être appelée par le `DynamicPopulateExtender` contrôle. Le service web implémente la méthode `getDate()` qui attend un argument de type chaîne, appelée `contextKey`, dans la mesure où le `DynamicPopulate` contrôle envoie une information de contexte à chaque appel de service web. Voici le code (fichier `DynamicPopulate.cs.asmx`) qui Récupère la date actuelle dans un des trois formats :

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

Dans l’étape suivante, créer un nouveau site ASP.NET et démarrer avec le contrôle ASP.NET AJAX ScriptManager :

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

Ensuite, ajoutez un contrôle d’étiquette (par exemple en utilisant le contrôle HTML du même nom, ou le `<asp:Label />` contrôle web) qui affiche plus tard le résultat de l’appel de service web.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

Ensuite, incluez un `DynamicPopulateExtender` contrôlent et fournissent des informations sur le service web, contrôle cible, mais pas le nom du contrôle qui déclenche le remplissage de cette opération est effectuée par la suite à l’aide de code JavaScript personnalisé !

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

Maintenant à la partie de JavaScript. Le `$find()` fonction, définie par la bibliothèque ASP.NET AJAX, retourne une référence aux objets côté serveur d’ASP.NET AJAX Control Toolkit comme `DynamicPopulateExtender`. Dans le fichier actuel, `$find("dpe")` retourne une référence à le `DynamicPopulateExtender` contrôle dans la page. Il expose une méthode appelée `populate()` qui déclenche le processus de remplissage dynamique. Le `populate()` méthode nécessite un seul argument : la clé de contexte qui servira d’en tant qu’argument à la `getDate()` méthode web. Ainsi, par exemple, `$find("dpe").populate("format1")` peut remplir l’étiquette avec la date actuelle au format mois-jour-année.

Afin de rendre l’exemple un peu plus souple, l’utilisateur peut désormais choisir entre plusieurs formats de date. Pour chacun d'entre eux, une case s’affiche. Une fois l’utilisateur clique sur un bouton radio, code JavaScript remplit dynamiquement l’étiquette avec le format de date sélectionnée. Voici ces cases :

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

Notez que dans le contexte d’un bouton radio, l’expression JavaScript `this.value` fait référence à la valeur du bouton actuel, qui se trouve être exactement les mêmes informations le `getDate()` méthode peut fonctionner avec.


[![Un clic sur le bouton récupère la date à partir du serveur, dans le format spécifié](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)

Un clic sur le bouton récupère la date à partir du serveur, dans le format spécifié ([cliquez pour afficher l’image en taille réelle](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](dynamically-populating-a-control-cs.md)
> [Suivant](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
