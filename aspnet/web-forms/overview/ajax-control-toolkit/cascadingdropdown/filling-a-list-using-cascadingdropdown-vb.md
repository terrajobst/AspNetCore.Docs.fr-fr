---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: Remplissage d’une liste avec CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: Le contrôle CascadingDropDown dans AJAX Control Toolkit étend un contrôle DropDownList afin que les modifications dans un DropDownList charges associés à des valeurs dans anoth...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 4a2e2440c0b93fc3945c466a2c60c6560c9e57a4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834175"
---
<a name="filling-a-list-using-cascadingdropdown-vb"></a>Remplissage d’une liste avec CascadingDropDown (VB)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)

> Le contrôle CascadingDropDown dans AJAX Control Toolkit étend un contrôle DropDownList afin que les modifications dans un DropDownList charges associés à des valeurs dans un autre objet DropDownList. (Par exemple, une liste fournit une liste des États, et la liste suivante est ensuite remplie de grandes villes dans cet état.) La première difficulté à résoudre est réellement remplir une liste déroulante à l’aide de ce contrôle.


## <a name="overview"></a>Vue d'ensemble

Le contrôle CascadingDropDown dans AJAX Control Toolkit étend un contrôle DropDownList afin que les modifications dans un DropDownList charges associés à des valeurs dans un autre objet DropDownList. (Par exemple, une liste fournit une liste des États, et la liste suivante est ensuite remplie de grandes villes dans cet état.) La première difficulté à résoudre est réellement remplir une liste déroulante à l’aide de ce contrôle.

## <a name="steps"></a>Étapes

Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le `<form>` élément) :

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

Ensuite, un contrôle DropDownList est nécessaire :

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

Pour cette liste, un extendeur CascadingDropDown est ajouté. Il envoie une demande asynchrone à un service web qui renvoie ensuite une liste d’entrées à afficher dans la liste. Pour ce faire, les attributs de CascadingDropDown suivants doivent être définies :

- `ServicePath`: L’URL d’un service web fournissant les entrées de liste
- `ServiceMethod`: Méthode web fournir les entrées de liste
- `TargetControlID`: ID de la liste déroulante
- `Category`: Les informations de catégorie qui sont soumises à la méthode web lorsqu’elle est appelée
- `PromptText`: Texte qui s’affichée lorsque le chargement asynchrone des données de liste à partir du serveur

Voici le balisage pour le `CascadingDropDown` élément. La seule différence entre c# et VB est le nom du service web associé :

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

Le code JavaScript en provenance de la `CascadingDropDown` extendeur appelle une méthode de service web avec la signature suivante :

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

Par conséquent, l’aspect important est que la méthode doit retourner un tableau de type `CascadingDropDownNameValue` (défini par ASP.NET AJAX Control Toolkit). Dans le `CascadingDropDownNameValue` constructeur, le premier texte d’entrée de la liste, puis sa valeur doivent être fournies, tout comme `<option value="VALUE">NAME</option>` dans HTML. Voici quelques exemples de données :

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

Chargement de la page dans le navigateur déclenche la liste à remplir avec les trois fournisseurs.


[![La liste est remplie automatiquement](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)

La liste est remplie automatiquement ([cliquez pour afficher l’image en taille réelle](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](using-auto-postback-with-cascadingdropdown-cs.md)
> [Suivant](using-cascadingdropdown-with-a-database-vb.md)
