---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
title: Filtrage avec une DropDownList (VB) maître/détail | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons voir comment afficher les enregistrements maîtres dans un contrôle DropDownList et les détails de l’élément de liste sélectionné dans un GridView.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: ea44717e-ab2e-46cd-a692-e4a9c0de194c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
msc.type: authoredcontent
ms.openlocfilehash: d15348812abf88fa35485c0d3415b2683225b6b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386693"
---
<a name="masterdetail-filtering-with-a-dropdownlist-vb"></a>Filtrage avec une DropDownList (VB) maître/détail
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_7_VB.exe) ou [télécharger le PDF](master-detail-filtering-with-a-dropdownlist-vb/_static/datatutorial07vb1.pdf)

> Dans ce didacticiel, nous allons voir comment afficher les enregistrements maîtres dans un contrôle DropDownList et les détails de l’élément de liste sélectionné dans un GridView.


## <a name="introduction"></a>Introduction

Un type commun de rapport est la *maître/détail rapport*, dans laquelle commence le rapport en affichant un jeu d’enregistrements « maîtres ». L’utilisateur peut puis accédez à un des principaux, affichant ainsi que master détails de l’enregistrement «. » Maître/détail signale est un choix idéal pour visualiser les relations un-à-plusieurs, tel qu’un rapport affichant toutes les catégories, puis en autorisant un utilisateur de sélectionner une catégorie particulière et afficher ses produits associés. En outre, les rapports maître/détail sont utiles pour afficher des informations détaillées à partir des tables en particulier « larges » (ceux qui ont un grand nombre de colonnes). Par exemple, le niveau « maître » d’un rapport maître/détail peut-être afficher simplement le nom et l’unité de prix du produit des produits dans la base de données et exploration d’un produit particulier affiche les champs de produits supplémentaires (catégorie, fournisseur, quantité par unité, et ainsi de suite).

Il existe plusieurs façons avec laquelle un rapport maître/détail peut être implémenté. Cet aspect ainsi que les trois didacticiels, nous allons examiner divers rapports maître/détail. Dans ce didacticiel, nous allons voir comment afficher les enregistrements maîtres dans un [contrôle DropDownList](https://msdn.microsoft.com/library/dtx91y0z.aspx) et les détails de l’élément de liste sélectionné dans un GridView. En particulier, rapport maître/détail de ce didacticiel va répertorier les informations de catégorie et de produit.

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>Étape 1 : Afficher les catégories dans un contrôle DropDownList

Notre rapport maître/détail répertorie les catégories dans un contrôle DropDownList, avec les produits de l’élément de liste sélectionné affichées plus loin dans la page dans un GridView. La première tâche préalable des États-Unis, est ensuite, pour que les catégories affichées dans un contrôle DropDownList. Ouvrez le `FilterByDropDownList.aspx` page dans le `Filtering` dossier, faites glisser sur un contrôle DropDownList de la boîte à outils vers le Concepteur de la page et définissez son `ID` propriété `Categories`. Ensuite, cliquez sur le lien de choisir la Source de données à partir de la balise active de la liste DropDownList. Ceci affichera l’Assistant Configuration de Source de données.


[![Spécifiez la Source de données de la liste DropDownList](master-detail-filtering-with-a-dropdownlist-vb/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image1.png)

**Figure 1**: spécifiez la Source de données de l’objet DropDownList ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image3.png))


Choisissez d’ajouter un nouveau ObjectDataSource nommé `CategoriesDataSource` qui appelle le `CategoriesBLL` la classe `GetCategories()` (méthode).


[![Ajouter un nouveau ObjectDataSource nommé CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-vb/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image4.png)

**Figure 2**: ajouter une nouvelle nommée de ObjectDataSource `CategoriesDataSource` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image6.png))


[![Choisissez d’utiliser la classe CategoriesBLL](master-detail-filtering-with-a-dropdownlist-vb/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image7.png)

**Figure 3**: choisissez d’utiliser le `CategoriesBLL` classe ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image9.png))


[![Configurer pour utiliser la méthode GetCategories() ObjectDataSource](master-detail-filtering-with-a-dropdownlist-vb/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image10.png)

**Figure 4**: configurer ObjectDataSource à utiliser le `GetCategories()` (méthode) ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image12.png))


Après avoir configuré l’ObjectDataSource, nous avons besoin pour spécifier quel champ de source de données doit s’afficher dans DropDownList et qui, une doit être associée en tant que la valeur de l’élément de liste. Avoir le `CategoryName` champ en tant que l’affichage et `CategoryID` comme valeur pour chaque élément de liste.


[![Ont l’affichage DropDownList le champ nom de catégorie et utilisez CategoryID comme valeur](master-detail-filtering-with-a-dropdownlist-vb/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image13.png)

**Figure 5**: affiche la liste DropDownList le `CategoryName` champ et utilisez `CategoryID` comme valeur ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image15.png))


À ce stade, nous avons un contrôle DropDownList qui est rempli avec les enregistrements à partir de la `Categories` table (toutes accompli dans environ six secondes). Figure 6 illustre notre progression jusqu'à présent lorsqu’ils sont affichés via un navigateur.


[![Une liste déroulante répertorie les catégories des actifs](master-detail-filtering-with-a-dropdownlist-vb/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image16.png)

**Figure 6**: une liste déroulante répertorie les catégories des actifs ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image18.png))


## <a name="step-2-adding-the-products-gridview"></a>Étape 2 : Ajout de produits GridView

Cette dernière étape dans notre rapport maître/détail consiste à répertorier les produits associés à la catégorie sélectionnée. Pour ce faire, ajoutez un GridView à la page et créer un nouveau ObjectDataSource nommé `productsDataSource`. Ont le `productsDataSource` contrôle sélectionnons ses données à partir de la `ProductsBLL` la classe `GetProductsByCategoryID(categoryID)` (méthode).


[![Sélectionnez la méthode GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-vb/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image19.png)

**Figure 7**: sélectionnez le `GetProductsByCategoryID(categoryID)` (méthode) ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image21.png))


Après avoir choisi cette méthode, l’Assistant ObjectDataSource nous demande la valeur de la méthode *`categoryID`* paramètre. Pour utiliser la valeur de l’élément sélectionné `categories` DropDownList élément définie la source de paramètre au contrôle et le ControlID à `Categories`.


[![Définir la paramètre categoryID sur la valeur de l’objet DropDownList de catégories](master-detail-filtering-with-a-dropdownlist-vb/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image22.png)

**Figure 8**: définir le *`categoryID`* paramètre à la valeur de la `Categories` DropDownList ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image24.png))


Prenez un moment pour consulter notre progression dans un navigateur. Lors de la première visite la page, ces produits font partie de la catégorie sélectionnée (boissons) sont affichées (comme indiqué dans la Figure 9), mais la modification de la liste DropDownList ne met à jour les données. Il s’agit, car une publication (postback) doit survenir pour que le contrôle GridView à mettre à jour. Pour ce faire, nous avons deux options (qui ne nécessite l’écriture de code) :

- **Définir les catégories DropDownList**[propriété AutoPostBack](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**sur True.** (Vous pouvez parvenir en sélectionnant l’option Activer AutoPostBack dans la balise active de la liste DropDownList.) Cela déclenchera une publication (postback) lorsque vous sélectionnez la DropDownList élément est modifié par l’utilisateur. Par conséquent, lorsque l’utilisateur sélectionne une nouvelle catégorie dans la liste DropDownList résulte une publication (postback) et le contrôle GridView sera actualisée avec les produits de la catégorie qui vient d’être sélectionnée. (Il s’agit de l’approche que j’ai utilisé dans ce didacticiel.)
- **Ajouter un contrôle bouton en regard de la liste DropDownList.** Définissez ses `Text` propriété pour l’actualisation ou quelque chose de similaire. Avec cette approche, l’utilisateur devra sélectionner une nouvelle catégorie et puis cliquez sur le bouton. En cliquant sur le bouton pour provoquer une publication (postback) et mettre à jour le contrôle GridView pour répertorier les produits de la catégorie sélectionnée.

Les figures 9 et 10 montrent le rapport maître/détail en action.


[![Lors de la première visite la Page, les produits de boissons sont affichés.](master-detail-filtering-with-a-dropdownlist-vb/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image25.png)

**Figure 9**: lors de la première visite la Page, les produits de boissons sont affichés ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image27.png))


[![Sélection d’un nouveau produit (produit) automatiquement provoque une publication (postback), la mise à jour le contrôle GridView](master-detail-filtering-with-a-dropdownlist-vb/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image28.png)

**Figure 10**: sélection d’un nouveau produit (produit) automatiquement provoque une publication (postback), la mise à jour le contrôle GridView ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image30.png))


## <a name="adding-a----choose-a-category----list-item"></a>Ajout d’un élément de liste «--Choisir une catégorie-- »

Lors de la visite de tout d’abord le `FilterByDropDownList.aspx` page les catégories premier élément de liste du DropDownList (boissons) est sélectionné par défaut, montrant les produits de boissons dans le contrôle GridView. Non montrant les produits de la première catégorie, que nous souhaiterons peut-être d’avoir à la place un élément DropDownList sélectionné qui dit quelque chose comme «--choisir une catégorie-- ».

Pour ajouter un nouvel élément de liste à l’objet DropDownList, accédez à la fenêtre Propriétés, puis cliquez sur le bouton de sélection dans le `Items` propriété. Ajouter un nouvel élément de liste avec la `Text` «--choisir une catégorie-- » et le `Value` `-1`.


[![Ajouter un--choisir une catégorie, élément de liste](master-detail-filtering-with-a-dropdownlist-vb/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image31.png)

**Figure 11**: ajouter un--choisir une catégorie, élément de liste ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image33.png))


Vous pouvez également ajouter l’élément de liste en ajoutant le balisage suivant à la liste DropDownList :


[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample1.aspx)]

En outre, nous devons définir le contrôle DropDownList `AppendDataBoundItems` sur True, car lorsque les catégories sont liés à la liste DropDownList à partir de l’ObjectDataSource elles remplaceront tous les éléments ajoutés manuellement la liste si `AppendDataBoundItems` n’est pas True.


![Définissez la propriété AppendDataBoundItems sur True](master-detail-filtering-with-a-dropdownlist-vb/_static/image34.png)

**Figure 12**: définir le `AppendDataBoundItems` True à la propriété


Après ces modifications, lors de la visite de tout d’abord la page de l’option «--choisir une catégorie-- » est sélectionnée et aucun produit n’est affichés.


[![Sur le chargement de Page Initial aucun produit n’est affichés](master-detail-filtering-with-a-dropdownlist-vb/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image35.png)

**Figure 13**: sur l’initiale Page charge non produits sont affichés ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image37.png))


Aucun produit n’est affichés lorsque car l’élément de liste «--Choisir une catégorie-- » est sélectionnée est parce que sa valeur est `-1` et il n’y aucun produit dans la base de données avec un `CategoryID` de `-1`. S’il s’agit du comportement voulu, puis que vous avez terminé à ce stade ! Si, toutefois, vous souhaitez afficher *tous les* des catégories lorsque l’élément de liste «--Choisir une catégorie-- » est sélectionné, revenir à la `ProductsBLL` classe et de personnaliser le `GetProductsByCategoryID(categoryID)` méthode afin qu’elle appelle le `GetProducts()` (méthode) si passé dans *`categoryID`* paramètre est inférieur à zéro :


[!code-vb[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample2.vb)]

La technique utilisée ici est similaire à l’approche que nous avons utilisé pour afficher tous les fournisseurs dans le [paramètres déclaratifs](../basic-reporting/declarative-parameters-cs.md) (didacticiel), bien que dans cet exemple, nous utilisons la valeur `-1` pour indiquer que tous les enregistrements doivent être récupéré par opposition à `Nothing`. Il s’agit, car le *`categoryID`* paramètre de la `GetProductsByCategoryID(categoryID)` méthode attend comme valeur d’entier passés, tandis que dans le didacticiel de paramètres déclaratifs nous étions en passant un paramètre d’entrée de chaîne.

La figure 14 montre la capture d’écran `FilterByDropDownList.aspx` lorsque l’option «--choisir une catégorie-- » est sélectionnée. Ici, tous les produits sont affichés par défaut, et l’utilisateur peut limiter l’affichage en choisissant une catégorie spécifique.


[![Tous les produits sont maintenant répertoriés par défaut](master-detail-filtering-with-a-dropdownlist-vb/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image38.png)

**Figure 14**: tous les produits sont maintenant répertoriés par défaut ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image40.png))


## <a name="summary"></a>Récapitulatif

Lors de l’affichage des données liées de manière hiérarchique, il permet souvent de présenter les données à l’aide de rapports maître/détail, à partir de laquelle l’utilisateur peut démarrer feuilletant les données à partir du haut de la hiérarchie et afficher les détails. Dans ce didacticiel, nous avons examiné la création d’un rapport maître/détail simple montrant les produits d’une catégorie sélectionnée. Cela a été accompli en utilisant un DropDownList pour la liste des catégories et un GridView pour les produits appartenant à la catégorie sélectionnée.

Dans le [didacticiel suivant](master-detail-filtering-with-two-dropdownlists-vb.md) nous allons l’une étape interface de DropDownList en outre, à l’aide de deux DropDownList.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
> [Suivant](master-detail-filtering-with-two-dropdownlists-vb.md)
