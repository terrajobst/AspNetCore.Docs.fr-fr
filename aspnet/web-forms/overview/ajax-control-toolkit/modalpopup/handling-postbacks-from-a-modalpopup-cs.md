---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Gestion des publications (postback) à partir d’un ModalPopup (c#) | Microsoft Docs
author: wenz
description: Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client. Une attention particulière doit entreprendre lorsqu’un pos...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 0f27d28b9851f8e26c207a6bc495e98ad70577a2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838120"
---
<a name="handling-postbacks-from-a-modalpopup-c"></a>Gestion des publications (postback) à partir d’un ModalPopup (c#)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)

> Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client. Une attention particulière doit prendre lorsqu’une publication (postback) est créé à partir de la fenêtre contextuelle.


## <a name="overview"></a>Vue d'ensemble

Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client. Une attention particulière doit prendre lorsqu’une publication (postback) est créé à partir de la fenêtre contextuelle.

## <a name="steps"></a>Étapes

Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le `<form>` élément) :

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

Ensuite, ajoutez un panneau qui sert de la fenêtre contextuelle modale. Là, l’utilisateur peut entrer un nom et une adresse de messagerie. Un bouton est utilisé pour fermer la fenêtre contextuelle et enregistrer les informations. Notez que le `OnClick` attribut a la valeur afin qu’une publication (postback) se produit lorsqu’un clic sur ce bouton :

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

La page elle-même se compose de deux étiquettes exactement les mêmes informations : nom et adresse de messagerie. Un bouton est utilisé pour déclencher la fenêtre contextuelle modale :

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

Pour que la fenêtre contextuelle s’affiche, ajoutez le `ModalPopupExtender` contrôle. Définir le `PopupControlID` ID du panneau par d’attribut et `TargetControlID` à l’ID du bouton :

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

Maintenant chaque fois que le `Save` dans la fenêtre contextuelle modale avec le bouton est cliqué, côté serveur `SaveData()` méthode est exécutée. Là, vous pouvez enregistrer les données entrées dans un magasin de données. Par souci de simplicité, les nouvelles données sont tout simplement sorties dans l’étiquette :

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

En outre, les contrôles de zone de texte dans la fenêtre contextuelle modale doivent être remplis avec le nom et le courrier électronique. Toutefois cela est nécessaire uniquement lorsque aucune publication (postback) se produit. En l’absence d’une publication (postback), la fonctionnalité de viewstate ASP.NET remplira automatiquement les zones de texte avec les valeurs appropriées.

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]


[![La fenêtre contextuelle modale entraîne une publication](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)

La fenêtre contextuelle modale entraîne une publication ([cliquez pour afficher l’image en taille réelle](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](using-modalpopup-with-a-repeater-control-cs.md)
> [Suivant](positioning-a-modalpopup-cs.md)
