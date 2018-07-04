---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
title: Réduction et développement d’un panneau à partir de JavaScript (VB) | Microsoft Docs
author: wenz
description: Le contrôle CollapsiblePanel dans ASP.NET AJAX Control Toolkit étend un panneau et lui fournit la fonctionnalité pour réduire son contenu et le développer un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 298789b4-2964-49f5-a0a8-d4dbeb9ff2c2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 917fa7a276e95d8f2259239698e6f9904f9abbf7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37361928"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-vb"></a>Réduction et développement d’un panneau à partir de JavaScript (VB)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)

> Le contrôle CollapsiblePanel dans ASP.NET AJAX Control Toolkit étend un panneau et lui fournit la fonctionnalité pour réduire son contenu et le développer à nouveau. Ces deux actions peuvent également être déclenchées à partir de code JavaScript personnalisé.


## <a name="overview"></a>Vue d'ensemble

Le contrôle CollapsiblePanel dans ASP.NET AJAX Control Toolkit étend un panneau et lui fournit la fonctionnalité pour réduire son contenu et le développer à nouveau. Ces deux actions peuvent également être déclenchées à partir de code JavaScript personnalisé.

## <a name="steps"></a>Étapes

Tout d’abord, créez une page ASP.NET et inclure le `ScriptManager` dans celui `<form>` élément. Cela charge la bibliothèque ASP.NET AJAX, ce qui est requis par les outils de contrôle :

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample1.aspx)]

Ensuite, créez un panneau avec du texte, afin de pouvoir l’effet de réduction/développement peut être :

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample2.aspx)]

Comme vous pouvez le voir, le panneau de configuration fait référence à une classe CSS qui est indiquée ici (et fondamentalement définit une couleur d’arrière-plan et la largeur du panneau) :

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample3.css)]

Le `CollapsiblePanelExtender` contrôle requiert le `TargetControlID` attribut afin que le Kit de ressources sache quel panneau pour réduire ou développer à la demande :

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample4.aspx)]

Malheureusement, l’extendeur actuellement n’expose pas une API spécifique pour réduire ou de développer le panneau de configuration, mais certaines méthodes non documentées effectuera. Tout d’abord, ajoutez trois boutons HTML à la page qui déclenchera le JavaScript côté client pour réduire ou développer le contenu du panneau :

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample5.aspx)]

Dans le code JavaScript côté client (Démarrer avec `<script type="text/javascript">`), la `$find()` méthode doit être utilisée pour accéder à la `CollapsiblePanelExtender`. `$find("cpe")` Retourne une référence à celui-ci. Depuis lors, des méthodes spécifiques permettent de résoudre la tâche en cours.

La méthode d’ouverture (développement) le panneau de configuration est appelé `_doOpen()`; le code suivant implémente la `doOpen()` fonction appelée lorsque l’utilisateur clique sur le premier bouton :

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample6.js)]

Pour une fermeture ou réduire le panneau de configuration, le `_doClose()` méthode doit être exécutée. Par conséquent, lorsque l’utilisateur clique sur le deuxième bouton, le code JavaScript suivant est appelé :

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample7.js)]

Le troisième bouton bascule l’état du panneau : à partir de réduits à développée et vice versa. Le `CollapsiblePanelExtender` expose le `toggle()` méthode qui est exactement ce que : inverse l’état du panneau. Cependant, il est également une autre approche (qui est utilisé en interne par le `toggle()` (méthode)) : le `get_Collapsed()` méthode de la `CollapsiblePanelExtender()` nous indique si le panneau est réduit ou non. Selon la valeur de retour de cette fonction, le panneau est ensuite soit développé (`_doOpen()` méthode) ou réduit (`_doClose()`) méthode :

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample8.js)]


[![Le troisième bouton modifie l’état du panneau : à partir de réduits à développée et précédent](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)

Le troisième bouton modifie l’état du panneau : à partir de réduits à développée et précédent ([cliquez pour afficher l’image en taille réelle](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](collapsing-and-expanding-a-panel-from-javascript-cs.md)
