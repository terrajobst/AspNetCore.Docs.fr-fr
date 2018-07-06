---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
title: Ajout d’une colonne GridView de cases à cocher (c#) | Microsoft Docs
author: rick-anderson
description: Ce didacticiel explique comment ajouter une colonne de cases à cocher à un contrôle GridView à permettre aux utilisateurs de manière intuitive de la sélection de plusieurs lignes de G....
ms.author: aspnetcontent
ms.date: 03/06/2007
ms.assetid: f63a9443-2db0-4f80-8246-840d3e86c2a3
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: 481ae436ef644bcc4d5a13d060ed87671cfcb4dc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814779"
---
<a name="adding-a-gridview-column-of-checkboxes-c"></a>Ajout d’une colonne GridView de cases à cocher (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_CS.exe) ou [télécharger le PDF](adding-a-gridview-column-of-checkboxes-cs/_static/datatutorial52cs1.pdf)

> Ce didacticiel explique comment ajouter une colonne de cases à cocher à un contrôle GridView à permettre aux utilisateurs de manière intuitive de la sélection de plusieurs lignes du contrôle GridView.


## <a name="introduction"></a>Introduction

Dans le didacticiel précédent, nous avons examiné comment ajouter une colonne de cases d’option dans le contrôle GridView à des fins de sélection d’un enregistrement particulier. Une colonne de cases d’option est une interface utilisateur appropriée lorsque l’utilisateur est limité à la sélection d’au maximum un élément à partir de la grille. Dans certains cas, toutefois, nous souhaiterons autoriser l’utilisateur de choisir un nombre arbitraire d’éléments à partir de la grille. Clients de messagerie basée sur le Web, par exemple, en général, affichent la liste des messages avec une colonne de cases à cocher. L’utilisateur peut sélectionner un nombre arbitraire de messages, puis effectuez une action, telles que le déplacement des e-mails vers un autre dossier ou la suppression.

Dans ce didacticiel, nous verrons comment ajouter une colonne de cases à cocher et déterminer quelles cases à cocher ont été vérifiées lors de la publication. En particulier, nous allons créer un exemple qui reproduit fidèlement l’interface utilisateur du client sur le web. Notre exemple inclut un GridView paginé répertoriant les produits dans le `Products` table de base de données avec une case à cocher de chaque ligne (voir Figure 1). Un bouton Supprimer des produits sélectionnés, cette option supprime ces produits sélectionnés.


[![Chaque ligne de produit inclut une case à cocher](adding-a-gridview-column-of-checkboxes-cs/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image1.png)

**Figure 1**: chaque ligne de produit inclut une case à cocher ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-checkboxes-cs/_static/image2.png))


## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>Étape 1 : Ajout d’un GridView paginé qui répertorie les informations de produit

Avant de nous soucier d’ajout d’une colonne de cases à cocher, permettent de concentrer tout d’abord de s sur la liste des produits dans un GridView qui prend en charge la pagination. Commencez par ouvrir le `CheckBoxField.aspx` page dans le `EnhancedGridView` dossier et faites glisser un GridView à partir de la boîte à outils vers le concepteur, en définissant son `ID` à `Products`. Ensuite, choisissez lier le contrôle GridView à une nouvelle ObjectDataSource nommé `ProductsDataSource`. Configurer l’ObjectDataSource à utiliser le `ProductsBLL` classe, appelant le `GetProducts()` méthode pour retourner les données. Dans la mesure où ce GridView sera en lecture seule, définir les listes déroulantes dans la mise à jour, insertion et supprimer des onglets à (None).


[![Créer un nouveau ObjectDataSource nommé ProductsDataSource](adding-a-gridview-column-of-checkboxes-cs/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image3.png)

**Figure 2**: créer une nouvelle nommée de ObjectDataSource `ProductsDataSource` ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-checkboxes-cs/_static/image4.png))


[![Configurer l’ObjectDataSource pour récupérer des données à l’aide de la méthode GetProducts()](adding-a-gridview-column-of-checkboxes-cs/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image5.png)

**Figure 3**: configurer ObjectDataSource pour récupérer des données à l’aide du `GetProducts()` (méthode) ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-checkboxes-cs/_static/image6.png))


[![Définir les listes déroulantes dans la mise à jour, insertion et supprimer des onglets à (None)](adding-a-gridview-column-of-checkboxes-cs/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image7.png)

**Figure 4**: la valeur est la liste déroulante répertorie dans la mise à jour, insertion et supprimer des onglets (aucun) ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-checkboxes-cs/_static/image8.png))


À l’issue de l’Assistant Configurer la Source de données, Visual Studio crée automatiquement BoundColumns et un CheckBoxColumn pour les champs de données relatives au produit. Comme nous l’avons fait dans le didacticiel précédent, supprimez tout sauf la `ProductName`, `CategoryName`, et `UnitPrice` BoundFields et modifier le `HeaderText` propriétés à catégorie, les produits et les prix. Configurer le `UnitPrice` BoundField afin que sa valeur est mise en forme comme une devise. Configurez également le contrôle GridView pour prendre en charge la pagination en cochant la case Activer la pagination de la balise active.

Permettent de s également ajouter l’interface utilisateur pour la suppression des produits sélectionnés. Ajouter un contrôle bouton sous le contrôle GridView, en définissant son `ID` à `DeleteSelectedProducts` et son `Text` propriété à supprimer les produits sélectionnés. Au lieu de supprimer réellement les produits à partir de la base de données, pour cet exemple nous allons simplement afficher un message indiquant que les produits qui aurait été supprimés. Pour ce faire, ajoutez un contrôle Web Label sous le bouton. Affectez à son ID `DeleteResults`, désactivez out son `Text` propriété et définissez son `Visible` et `EnableViewState` propriétés à `false`.

Après avoir apporté ces modifications, le balisage déclaratif s GridView, ObjectDataSource, bouton et une étiquette doit similaire à ce qui suit :


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample1.aspx)]

Prenez un moment pour afficher la page dans un navigateur (voir Figure 5). À ce stade, vous devez voir le nom, la catégorie et le prix des dix premiers produits.


[![Le nom, la catégorie et le prix des dix premiers produits sont répertoriés.](adding-a-gridview-column-of-checkboxes-cs/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image9.png)

**Figure 5**: le champ nom, catégorie et le prix des dix premiers produits sont répertoriés ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-checkboxes-cs/_static/image10.png))


## <a name="step-2-adding-a-column-of-checkboxes"></a>Étape 2 : Ajout d’une colonne de cases à cocher

Dans la mesure où ASP.NET 2.0 inclut une CheckBoxField, on pourrait penser qu’il peut servir à ajouter une colonne de cases à cocher à un GridView. Malheureusement, qui n’est pas le cas, comme le CheckBoxField est conçu pour fonctionner avec un champ de données booléen. Autrement dit, pour pouvoir utiliser le CheckBoxField, nous devons spécifier le champ de données sous-jacente dont la valeur est consultée pour déterminer si le rendu de la case à cocher est activée. Nous ne pouvons pas utiliser le CheckBoxField à simplement inclure une colonne de cases à cocher désactivée.

Au lieu de cela, nous devons ajouter un TemplateField et ajouter un contrôle de case à cocher Web à ses `ItemTemplate`. Lancez-vous et ajoutez un TemplateField pour le `Products` GridView et définissez-la comme le premier champ (extrême gauche). À partir de la balise active de s GridView, cliquez sur le lien Modifier les modèles et puis faites glisser un contrôle de case à cocher Web à partir de la boîte à outils dans le `ItemTemplate`. Définissez cette case à cocher s `ID` propriété `ProductSelector`.


[![Ajoutez un contrôle de case à cocher Web nommé ProductSelector à ItemTemplate s TemplateField](adding-a-gridview-column-of-checkboxes-cs/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image11.png)

**Figure 6**: ajouter un contrôle de case à cocher Web nommé `ProductSelector` au s TemplateField `ItemTemplate` ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-checkboxes-cs/_static/image12.png))


Avec le contrôle TemplateField et la case à cocher Web ajouté, chaque ligne inclut désormais une case à cocher. Figure 7 illustre cette page, lorsqu’ils sont affichés via un navigateur, une fois le TemplateField et la case à cocher ont été ajoutés.


[![Chaque ligne de produit inclut désormais une case à cocher](adding-a-gridview-column-of-checkboxes-cs/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image13.png)

**Figure 7**: chaque ligne de produit inclut désormais une case à cocher ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-checkboxes-cs/_static/image14.png))


## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>Étape 3 : Déterminer quelles cases à cocher ont été vérifiées lors de la publication

À ce stade, nous avons une colonne de cases à cocher, mais aucun moyen de déterminer quelles cases à cocher ont été vérifiées lors de la publication. Lorsque l’utilisateur clique sur le bouton Supprimer les produits sélectionnés, cependant, nous avons besoin de savoir quelles cases à cocher afin de supprimer ces produits ont été extraits.

Les opérations de mappage GridView [ `Rows` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows.aspx) fournit l’accès aux lignes de données dans le contrôle GridView. Nous pouvons parcourir ces lignes, accéder par programmation le contrôle de case à cocher et recherchez son `Checked` propriété afin de déterminer si la case à cocher a été sélectionnée.

Créer un gestionnaire d’événements pour le `DeleteSelectedProducts` contrôle Web Button s `Click` événement et ajoutez le code suivant :


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample2.cs)]

Le `Rows` propriété retourne une collection de `GridViewRow` instances cette composition les lignes de données de GridView s. Le `foreach` boucle ici énumère cette collection. Pour chaque `GridViewRow` de l’objet, la ligne s case à cocher est accessible par programmation à l’aide de `row.FindControl("controlID")`. Si la case à cocher est activée, la ligne s correspondant `ProductID` valeur est extraite de la `DataKeys` collection. Dans cet exercice, nous avons simplement afficher un message d’information dans le `DeleteResults` étiqueter, bien que dans une application opérationnelle nous d à la place un appel à la `ProductsBLL` classe s `DeleteProduct(productID)` (méthode).

Avec l’ajout de ce gestionnaire d’événements, cliquez sur le bouton Supprimer des produits sélectionnés maintenant pour afficher le `ProductID` s des produits sélectionnés.


[![Un clic sur le bouton Supprimer de produits sélectionnée est la ProductIDs de produits sélectionnée sont répertoriées](adding-a-gridview-column-of-checkboxes-cs/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image15.png)

**Figure 8**: lors de la supprimer sélectionné produits bouton les produits sélectionnés `ProductID` s sont répertoriés ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-checkboxes-cs/_static/image16.png))


## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>Étape 4 : Ajout vérifie toutes les et décochez tous les boutons

Si un utilisateur souhaite supprimer tous les produits sur la page actuelle, ils doivent vérifier chacune des cases à dix cocher. Nous pouvons vous aider à accélérer ce processus en ajoutant un vérifier tous les bouton qui, lorsque vous cliquez dessus, sélectionne toutes les cases à cocher dans la grille. Un bouton désélectionner tout serait tout aussi utile.

Ajoutez deux contrôles bouton à la page, en les plaçant au-dessus de la GridView. Définir le premier s `ID` à `CheckAll` et son `Text` propriété à vérifier tout ; ensemble le second s `ID` à `UncheckAll` et son `Text` propriété décochez tous les.


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample3.aspx)]

Ensuite, créez une méthode dans la classe code-behind nommée `ToggleCheckState(checkState)` qui, lorsqu’elle est appelée, énumère les `Products` GridView s `Rows` collection et définit chaque case à cocher s `Checked` valeur à la propriété des éléments transmis dans *checkState*  paramètre.


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample4.cs)]

Ensuite, créez `Click` gestionnaires d’événements pour le `CheckAll` et `UncheckAll` boutons. Dans `CheckAll` Gestionnaire d’événements s, il vous suffit d’appel `ToggleCheckState(true)`; dans `UncheckAll`, appelez `ToggleCheckState(false)`.


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample5.cs)]

Ce code, en cliquant sur le bouton Vérifier tout entraîne une publication et vérifie toutes les cases à cocher dans le contrôle GridView. De même, en cliquant sur Désélectionner tout Désélectionne toutes les cases à cocher. La figure 9 illustre l’écran une fois que le bouton Vérifier tout a été vérifié.


[![En cliquant sur le contrôle que bouton tous sélectionne toutes les cases à cocher](adding-a-gridview-column-of-checkboxes-cs/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image17.png)

**Figure 9**: en cliquant sur les vérifier tous les bouton sélectionne toutes les cases à cocher ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-checkboxes-cs/_static/image18.png))


> [!NOTE]
> Affichage d’une colonne de cases à cocher, une approche pour sélectionner ou désélectionner toutes les cases à cocher lorsque est via une case à cocher dans la ligne d’en-tête. En outre, en cours vérifie toutes les / décochez toute implémentation nécessite une publication (postback). Les cases à cocher possible checked ou unchecked, toutefois, entièrement par le biais d’un script côté client, offrant ainsi une expérience utilisateur plus active. Pour savoir comment utiliser une case à cocher de la ligne en-tête pour tout et décochez tout en détail, ainsi que d’une discussion sur l’utilisation de techniques de côté client, consultez [vérification toutes les cases à cocher dans un Script côté Client à l’aide de GridView et une case à cocher tous les vérifier](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx).


## <a name="summary"></a>Récapitulatif

Dans les cas où vous avez besoin pour permettre aux utilisateurs de choisir un nombre arbitraire de lignes à partir d’un GridView avant de continuer, l’ajout d’une colonne de cases à cocher est une option. Comme nous l’avons vu dans ce didacticiel, y compris une colonne de cases à cocher dans le contrôle GridView implique l’ajout d’un TemplateField avec un contrôle de case à cocher Web. À l’aide d’un contrôle Web (par opposition à l’injection de balisage directement dans le modèle, comme nous l’avons fait dans le didacticiel précédent) ASP.NET automatiquement se souvient de cases à cocher ont été et n’ont pas été vérifiées via la publication (postback). Nous pouvons également accéder par programme les cases à cocher dans le code pour déterminer si une case à cocher donné est activé, ou pour modifier l’état activé.

Ce didacticiel et la dernière étudié l’ajout d’une colonne de sélecteur de ligne pour le contrôle GridView. Dans notre prochain didacticiel, nous allons examiner comment, avec un peu de travail, nous pouvons ajouter des fonctionnalités insertion au GridView.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](adding-a-gridview-column-of-radio-buttons-cs.md)
> [Suivant](inserting-a-new-record-from-the-gridview-s-footer-cs.md)
