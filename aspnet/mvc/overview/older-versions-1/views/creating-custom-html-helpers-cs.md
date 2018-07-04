---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Création de Helpers HTML personnalisés (c#) | Microsoft Docs
author: microsoft
description: L’objectif de ce didacticiel consiste à montrer comment vous pouvez créer des programmes d’assistance HTML personnalisé que vous pouvez utiliser dans vos vues MVC. En tirant parti du programme d’assistance HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 1e5bb247f52162aba02e0d5775bced73f76d2081
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365170"
---
<a name="creating-custom-html-helpers-c"></a>Création de Helpers HTML personnalisés (c#)
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> L’objectif de ce didacticiel consiste à montrer comment vous pouvez créer des programmes d’assistance HTML personnalisé que vous pouvez utiliser dans vos vues MVC. En tirant parti des programmes d’assistance HTML, vous pouvez réduire la quantité de frappe fastidieux de balises HTML que vous devez effectuer pour créer une page HTML standard.


L’objectif de ce didacticiel consiste à montrer comment vous pouvez créer des programmes d’assistance HTML personnalisé que vous pouvez utiliser dans vos vues MVC. En tirant parti des programmes d’assistance HTML, vous pouvez réduire la quantité de frappe fastidieux de balises HTML que vous devez effectuer pour créer une page HTML standard.

Dans la première partie de ce didacticiel, je décris certaines des programmes d’assistance HTML existant inclus avec l’infrastructure ASP.NET MVC. Ensuite, je décris les deux méthodes de création de programmes d’assistance HTML personnalisée : j’explique comment créer des programmes d’assistance HTML personnalisée en créant une méthode statique et en créant une méthode d’extension.

## <a name="understanding-html-helpers"></a>Présentation des programmes d’assistance HTML

Une application d’assistance HTML est simplement une méthode qui retourne une chaîne. La chaîne peut représenter n’importe quel type de contenu que vous souhaitez. Par exemple, vous pouvez utiliser des programmes d’assistance HTML pour restituer des balises HTML standard comme HTML `<input>` et `<img>` balises. Vous pouvez également utiliser assistances HTML à afficher le contenu comme un contrôle onglet ou une table HTML de base de données plus complexe.

L’infrastructure ASP.NET MVC inclut l’ensemble des programmes d’assistance HTML standard (cela n’est pas une liste complète) suivantes :

- Html.ActionLink()
- Html.BeginForm()
- Html.CheckBox()
- Html.DropDownList()
- Html.EndForm()
- Html.Hidden()
- Html.ListBox()
- Html.Password()
- Html.RadioButton()
- Html.TextArea()
- Html.TextBox()

Par exemple, considérez le formulaire dans le Listing 1. Ce formulaire est restitué à l’aide de deux des programmes d’assistance HTML standard (voir Figure 1). Ce formulaire utilise la `Html.BeginForm()` et `Html.TextBox()` les méthodes d’assistance pour restituer un formulaire HTML simple.


[![Page rendue avec des programmes d’assistance HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)

**Figure 01**: Page rendue avec des programmes d’assistance HTML ([cliquez pour afficher l’image en taille réelle](creating-custom-html-helpers-cs/_static/image3.png))


**Liste 1 : `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

La méthode d’assistance Html.BeginForm() est utilisée pour créer le code HTML ouvrant et fermant `<form>` balises. Notez que le `Html.BeginForm()` méthode est appelée au sein d’un à l’aide de déclaration. L’instruction using assure que le `<form>` balise se ferme à la fin de l’à l’aide de bloc.

Si vous préférez, au lieu de créer un à l’aide de bloc, vous pouvez appeler la méthode d’assistance Html.EndForm() pour fermer la `<form>` balise. Utiliser l’approche de création d’ouverture et fermeture `<form>` balise qui semble la plus intuitive.

Le `Html.TextBox()` méthodes d’assistance sont utilisés dans le Listing 1 pour le rendu HTML `<input>` balises. Si vous sélectionnez Afficher la source dans votre navigateur, vous verrez la source HTML dans le Listing 2. Notez que la source contient des balises HTML standard.

> [!IMPORTANT]
> Notez que le `Html.TextBox()`-HTML Helper est rendue avec `<%= %>` balises au lieu de `<% %>` balises. Si vous n’incluez pas le signe égal, rien n’est rendu dans le navigateur.

L’infrastructure ASP.NET MVC contient un petit ensemble de programmes d’assistance. Vous devrez très probablement, étendre l’infrastructure MVC avec des programmes d’assistance HTML personnalisée. Dans le reste de ce didacticiel, vous allez les deux méthodes de création de programmes d’assistance HTML personnalisée.

**Listing 2 : `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a>Création de Helpers HTML avec des méthodes statiques

Pour créer un nouveau programme d’assistance HTML, le plus simple consiste à créer une méthode statique qui retourne une chaîne. Par exemple, imaginez que vous décidez de créer un nouveau programme d’assistance HTML qui assure un rendu HTML `<label>` balise. Vous pouvez utiliser la classe dans la liste 2 pour restituer un `<label>` .

**Listing 2 : `Helpers\LabelHelper.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

Il n’a rien de spécial à la classe dans le Listing 2. Le `Label()` méthode retourne simplement une chaîne.

La vue Index modifiée dans la liste 3 utilise le `LabelHelper` pour le rendu HTML `<label>` balises. Notez que la vue inclut une `<%@ imports %>` directive qui importe le `Application1.Helpers` espace de noms.

**Listing 2 : `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Création de Helpers HTML avec les méthodes d’Extension

Si vous souhaitez créer des programmes d’assistance HTML qui fonctionnent comme les programmes d’assistance HTML standard inclus dans l’infrastructure ASP.NET MVC, vous devez créer des méthodes d’extension. Méthodes d’extension permettent d’ajouter de nouvelles méthodes à une classe existante. Lorsque vous créez une méthode d’assistance HTML, vous ajoutez de nouvelles méthodes à la classe HtmlHelper représentée par la propriété de Html d’un affichage.

La classe dans le Listing 3 ajoute une méthode d’extension pour le `HtmlHelper` classe nommée `Label()`. Il existe plusieurs choses que vous devriez remarquer sur cette classe. Tout d’abord, notez que la classe est une classe statique. Vous devez définir une méthode d’extension avec une classe statique.

En second lieu, notez que le premier paramètre de la `Label()` méthode est précédée par le mot clé `this`. Le premier paramètre d’une méthode d’extension indique la classe qui étend la méthode d’extension.

**Liste 3 : `Helpers\LabelExtensions.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

Après avoir créé une méthode d’extension et que vous générez votre application avec succès, la méthode d’extension s’affiche dans Intellisense dans Visual Studio comme tous les autres méthodes d’une classe (voir Figure 2). La seule différence est qu’extension méthodes apparaissent avec un symbole spécial en regard (il s’agit d’une icône d’une flèche vers le bas).


[![À l’aide de la méthode d’extension Html.Label()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)

**Figure 02**: à l’aide de la méthode d’extension Html.Label() ([cliquez pour afficher l’image en taille réelle](creating-custom-html-helpers-cs/_static/image6.png))


La vue Index modifiée sur la liste 4 utilise la méthode d’extension Html.Label() pour restituer tous ses `<label>` balises.

**Liste 4 – `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez appris de deux méthodes de création de programmes d’assistance HTML personnalisée. Tout d’abord, vous avez appris à créer un personnalisé `Label()` programme d’assistance HTML en créant une méthode statique qui retourne une chaîne. Ensuite, vous avez appris à créer un personnalisé `Label()` méthode d’assistance HTML en créant une méthode d’extension sur la `HtmlHelper` classe.

Dans ce didacticiel, je me suis concentré sur la création d’une méthode d’assistance HTML extrêmement simple. Notez qu’une application d’assistance HTML peuvent être aussi complexe que vous le souhaitez. Vous pouvez créer des programmes d’assistance HTML qui restituent contenu riche, tels que les vues de l’arborescence, des menus ou des tables de base de données.

> [!div class="step-by-step"]
> [Précédent](asp-net-mvc-views-overview-cs.md)
> [Suivant](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
