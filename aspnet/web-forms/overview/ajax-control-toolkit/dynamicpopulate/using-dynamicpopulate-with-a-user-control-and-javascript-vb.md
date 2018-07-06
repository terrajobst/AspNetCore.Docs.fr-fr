---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: Utilisation de DynamicPopulate avec un contrôle utilisateur et le JavaScript (VB) | Microsoft Docs
author: wenz
description: Le contrôle de DynamicPopulate dans ASP.NET AJAX Control Toolkit appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible sur t...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: c2fe7fbe0d57c6cf64fe31607215c920e67ed736
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841313"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a>Utilisation de DynamicPopulate avec un contrôle utilisateur et le JavaScript (VB)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)

> Le contrôle DynamicPopulate dans ASP.NET AJAX Control Toolkit appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible dans la page, sans une actualisation de la page. Il est également possible de déclencher le remplissage à l’aide d’un code JavaScript côté client personnalisé. Toutefois, une attention particulière doit être effectuée lorsque l’extendeur se trouve dans un contrôle utilisateur.


## <a name="overview"></a>Vue d'ensemble

Le `DynamicPopulate` contrôle dans ASP.NET AJAX Control Toolkit appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible dans la page, sans une actualisation de la page. Il est également possible de déclencher le remplissage à l’aide d’un code JavaScript côté client personnalisé. Toutefois, une attention particulière doit être effectuée lorsque l’extendeur se trouve dans un contrôle utilisateur.

## <a name="steps"></a>Étapes

Tout d’abord, vous avez besoin d’un Service Web de ASP.NET qui implémente la méthode doit être appelée par le `DynamicPopulateExtender` contrôle. Le service web implémente la méthode `getDate()` qui attend un argument de type chaîne, appelée `contextKey`, dans la mesure où le `DynamicPopulate` contrôle envoie une information de contexte à chaque appel de service web. Voici le code (fichiers `DynamicPopulate.vb.asmx`) qui Récupère la date actuelle dans un des trois formats :

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

Dans l’étape suivante, créez un nouveau contrôle utilisateur (`.ascx` fichier), il est signalé par la déclaration suivante dans sa première ligne :

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

Un &lt; `label` &gt; élément doit être utilisé pour afficher les données provenant du serveur.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

Également dans le fichier de contrôle utilisateur, nous allons utiliser trois boutons radio, chacun d’eux représentant une des trois formats de date possibles prises en charge par le service web. Lorsque l’utilisateur clique sur l’un des boutons radio, le navigateur exécute le code JavaScript qui ressemble à ceci :

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

Ce code accède à la `DynamicPopulateExtender` (ne vous inquiétez pas sur l’ID étrange encore, ce point sera abordé plus tard) et déclenche le remplissage dynamique des données. Dans le contexte de la case actuel, `this.value` fait référence à sa valeur soit `format1`, `format2` ou `format3` exactement ce qu’attend la méthode web.

La seule chose que manquant encore dans le contrôle utilisateur est le `DynamicPopulateExtender` contrôle qui lie les boutons radio au service web.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

Là encore, vous pouvez noter l’ID étrange utilisé dans le contrôle : `mcd1$myDate` au lieu de `myDate`. Auparavant, le code JavaScript utilisé `mcd1_dpe1` pour accéder à la `DynamicPopulateExtender` au lieu de `dpe1`. Cette stratégie d’affectation de noms est un besoin particulier lorsque vous utilisez `DynamicPopulateExtender` au sein d’un contrôle utilisateur. En outre, vous devez incorporer le contrôle utilisateur de manière spécifique pour que tout fonctionne. Créez une page ASP.NET et inscrire un préfixe de balise pour le contrôle utilisateur que vous venez d’implémenter :

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

Incluez ensuite ASP.NET AJAX `ScriptManager` contrôle sur la nouvelle page :

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

Enfin, ajoutez le contrôle utilisateur à la page. Vous devez uniquement définir son `ID` attribut (et `runat="server"`, bien sûr), mais vous devez également lui attribuer un nom spécifique : `mcd1` puisqu’il s’agit le préfixe utilisé dans le contrôle utilisateur pour accéder à l’aide de JavaScript.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

Et voilà ! La page se comporte comme prévu : un utilisateur clique sur un des boutons radio, le contrôle dans la boîte à outils appelle le service web et affiche la date actuelle au format souhaité.


[![Les boutons radio résident dans un contrôle utilisateur](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)

Les boutons radio résident dans un contrôle utilisateur ([cliquez pour afficher l’image en taille réelle](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](dynamically-populating-a-control-using-javascript-code-vb.md)
