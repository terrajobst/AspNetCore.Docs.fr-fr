---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: À l’aide de la publication (Postback) automatique avec CascadingDropDown (c#) | Microsoft Docs
author: wenz
description: Le contrôle CascadingDropDown dans AJAX Control Toolkit étend un contrôle DropDownList afin que les modifications dans un DropDownList charges associés à des valeurs dans anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c84951691935d9976f961f65f96fa70633ecbce
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823640"
---
<a name="using-auto-postback-with-cascadingdropdown-c"></a>À l’aide de la publication (Postback) automatique avec CascadingDropDown (c#)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)

> Le contrôle CascadingDropDown dans AJAX Control Toolkit étend un contrôle DropDownList afin que les modifications dans un DropDownList charges associés à des valeurs dans un autre objet DropDownList. Toutefois lorsque vous utilisez le contrôle CascadingDropDown, ASP. Fonctionnalité de AutoPostBack du contrôle de NET DropDownList ne fonctionne pas, étant donné que le chargement de façon asynchrone des données dans la liste génère une publication (postback) (inutile) lui-même. Avec du code JavaScript, vous pouvez éviter cet effet.


## <a name="overview"></a>Vue d'ensemble

Le contrôle CascadingDropDown dans AJAX Control Toolkit étend un contrôle DropDownList afin que les modifications dans un DropDownList charges associés à des valeurs dans un autre objet DropDownList. (Par exemple, une liste fournit une liste des États, et la liste suivante est ensuite remplie de grandes villes dans cet état.) Toutefois lorsque vous utilisez le contrôle CascadingDropDown, ASP. Fonctionnalité de AutoPostBack du contrôle de NET DropDownList ne fonctionne pas, étant donné que le chargement de façon asynchrone des données dans la liste génère une publication (postback) (inutile) lui-même. Avec du code JavaScript, vous pouvez éviter cet effet.

## <a name="steps"></a>Étapes

Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le &lt; `form` &gt; élément) :

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

Ensuite, un contrôle DropDownList est nécessaire :

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

Pour cette liste, un extendeur CascadingDropDown est ajouté, qui fournit des informations de URL et la méthode de service web :

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

L’extendeur CascadingDropDown puis appelle de façon asynchrone un service web avec la signature de méthode suivante :

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

La méthode retourne un tableau de type valeur de CascadingDropDown. Le constructeur du type attend tout d’abord légende de l’entrée liste, puis la valeur (HTML `value` attribut).

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

Chargement de la page dans le navigateur remplira la liste déroulante avec trois fournisseurs, l’autre qui est présélectionnée. En outre, ASP.NET définit la `__doPostBack()` méthode JavaScript. Une fois que la page a été chargée, cet appel JavaScript est ajouté à la liste déroulante, mais uniquement s’il existe éléments qu’il contient. S’il n’existe aucun élément dans la liste, les outils de contrôle est actuellement leur chargement, afin que le code JavaScript utilise un délai d’expiration et tente à nouveau dans une demi-seconde.

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

De cette façon, une publication (postback) est exécutée uniquement lorsqu’il existe en fait des éléments dans la liste et l’utilisateur sélectionne une entrée.


[![Sélection d’un élément de liste entraîne une publication](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)

Sélection d’un élément de liste entraîne une publication ([cliquez pour afficher l’image en taille réelle](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](presetting-list-entries-with-cascadingdropdown-cs.md)
> [Suivant](filling-a-list-using-cascadingdropdown-vb.md)
