---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
title: Maître/détail filtrage avec une DropDownList (c#) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous expliquons comment afficher les rapports maître/détail dans une seule page web à l’aide de DropDownList pour afficher les enregistrements de 'master' et un contrôle DataList pour fficher sous...
ms.author: aspnetcontent
ms.date: 07/18/2007
ms.assetid: 07fa47ae-e491-4a2f-b265-d342b9ddef46
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: ed2631e49786c81075099cca6941d98ba3b67e37
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818250"
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Maître/détail filtrage avec une DropDownList (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_CS.exe) ou [télécharger le PDF](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/datatutorial33cs1.pdf)

> Dans ce didacticiel, nous expliquons comment afficher les rapports maître/détail dans une seule page web à l’aide de DropDownList pour afficher les enregistrements « maîtres » et un contrôle DataList pour afficher les « détails ».


## <a name="introduction"></a>Introduction

Le rapport maître/détail, que nous avons créée tout d’abord à l’aide d’un GridView dans l’ancien [filtrage de maître/détail avec un DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) début du didacticiel, en affichant un jeu d’enregistrements « maîtres ». L’utilisateur peut puis accédez à un des principaux, affichant ainsi que master détails de l’enregistrement «. » Les rapports maître/détail sont un choix idéal pour visualiser les relations un-à-plusieurs et l’affichage des informations détaillées à partir d’en particulier les tables « large » (ceux qui ont un grand nombre de colonnes). Nous avons étudié comment implémenter les rapports maître/détail à l’aide des contrôles GridView et DetailsView dans les didacticiels précédents. Dans ce didacticiel et les deux suivants, nous allons réexaminer ces concepts, mais le focus sur l’utilisation de contrôles DataList et Repeater contrôle à la place.

Dans ce didacticiel, nous allons examiner à l’aide d’un contrôle DropDownList pour contenir les enregistrements « maîtres », avec les enregistrements « details » affichés dans un contrôle DataList.

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>Étape 1 : Ajout de Pages Web didacticiel maître/détail

Avant de commencer ce didacticiel, nous allons tout d’abord prendre un moment pour ajouter le dossier et les pages ASP.NET que nous avons besoin pour ce didacticiel et les deux suivants affaire rapports maître/détail avec les contrôles DataList et Repeater. Commencez par créer un nouveau dossier dans le projet nommé `DataListRepeaterFiltering`. Ensuite, ajoutez les pages ASP.NET cinq suivantes à ce dossier, présentant toutes les configurer pour utiliser la page maître `Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![Créez un dossier DataListRepeaterFiltering et ajouter les Pages ASP.NET didacticiel](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image1.png)

**Figure 1**: créer un `DataListRepeaterFiltering` dossier et ajouter les Pages ASP.NET didacticiel


Ensuite, ouvrez le `Default.aspx` page et faites glisser le `SectionLevelTutorialListing.ascx` contrôle utilisateur à partir de la `UserControls` dossier sur l’aire de conception. Ce contrôle utilisateur, que nous avons créée dans le [Pages maîtres et Navigation du Site](../introduction/master-pages-and-site-navigation-cs.md) didacticiel, énumère le plan du site et affiche les didacticiels à partir de la section en cours dans une liste à puces.


[![Ajouter le contrôle utilisateur de SectionLevelTutorialListing.ascx à Default.aspx](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image2.png)

**Figure 2**: ajouter la `SectionLevelTutorialListing.ascx` contrôle utilisateur à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image4.png))


Afin de disposer de l’affichage de liste à puces les didacticiels maître/détail que nous allons créer, nous avons besoin de les ajouter au plan du site. Ouvrez le `Web.sitemap` fichier, puis ajoutez le balisage suivant après la balise de nœud de carte de site « Affichage des données avec la DataList et Repeater » :

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample1.xml)]


![Mettre à jour le plan du Site pour inclure les nouvelles Pages ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image5.png)

**Figure 3**: mettre à jour le plan du Site pour inclure les nouvelles Pages ASP.NET


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>Étape 2 : Afficher les catégories dans un contrôle DropDownList

Notre rapport maître/détail répertorie les catégories dans un contrôle DropDownList, avec les produits de l’élément de liste sélectionné affichées plus loin dans la page dans un contrôle DataList. La première tâche préalable des États-Unis, est ensuite, pour que les catégories affichées dans un contrôle DropDownList. Commencez par ouvrir le `FilterByDropDownList.aspx` page dans le `DataListRepeaterFiltering` dossier et faites glisser un contrôle DropDownList à partir de la boîte à outils vers le Concepteur de la page. Ensuite, définissez la DropDownList `ID` propriété `Categories`. Cliquez sur le lien de choisir la Source de données à partir de la balise active de l’objet DropDownList et créez un nouveau ObjectDataSource nommé `CategoriesDataSource`.


[![Ajouter un nouveau ObjectDataSource nommé CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image6.png)

**Figure 4**: ajouter une nouvelle nommée de ObjectDataSource `CategoriesDataSource` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image8.png))


Configurer le nouveau ObjectDataSource telle qu’elle appelle le `CategoriesBLL` la classe `GetCategories()` (méthode). Après avoir configuré l’ObjectDataSource, nous devons encore spécifier quel champ de source de données doit être affiché dans la liste DropDownList et un doit être associé en tant que la valeur pour chaque élément de liste. Avoir le `CategoryName` champ en tant que l’affichage et `CategoryID` comme valeur pour chaque élément de liste.


[![Ont l’affichage DropDownList le champ nom de catégorie et utilisez CategoryID comme valeur](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image9.png)

**Figure 5**: affiche la liste DropDownList le `CategoryName` champ et utilisez `CategoryID` comme valeur ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image11.png))


À ce stade, nous avons un contrôle DropDownList qui est rempli avec les enregistrements à partir de la `Categories` table (toutes accompli dans environ six secondes). Figure 6 illustre notre progression jusqu'à présent lorsqu’ils sont affichés via un navigateur.


[![Une liste déroulante répertorie les catégories des actifs](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image12.png)

**Figure 6**: une liste déroulante répertorie les catégories des actifs ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image14.png))


## <a name="step-2-adding-the-products-datalist"></a>Étape 2 : Ajout du contrôle DataList de produits

La dernière étape de notre rapport maître/détail consiste à répertorier les produits associés à la catégorie sélectionnée. Pour ce faire, ajoutez un contrôle DataList à la page et créer un nouveau ObjectDataSource nommé `ProductsByCategoryDataSource`. Avoir le `ProductsByCategoryDataSource` contrôle récupérer ses données à partir de la `ProductsBLL` la classe `GetProductsByCategoryID(categoryID)` (méthode). Dans la mesure où ce rapport maître/détail est en lecture seule, choisissez (aucun) option dans les onglets INSERT, UPDATE et DELETE.


[![Sélectionnez la méthode GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image15.png)

**Figure 7**: sélectionnez le `GetProductsByCategoryID(categoryID)` (méthode) ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image17.png))


Après avoir cliqué sur Suivant, l’Assistant ObjectDataSource nous demande la source de la valeur pour le `GetProductsByCategoryID(categoryID)` la méthode *`categoryID`* paramètre. Pour utiliser la valeur de l’élément sélectionné `categories` DropDownList élément définie la source de paramètre au contrôle et le ControlID à `Categories`.


[![Définir la paramètre categoryID sur la valeur de l’objet DropDownList de catégories](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image18.png)

**Figure 8**: définir le *`categoryID`* paramètre à la valeur de la `Categories` DropDownList ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image20.png))


À la fin de l’Assistant Configurer la Source de données, Visual Studio génère automatiquement un `ItemTemplate` pour le contrôle DataList qui affiche le nom et la valeur de chaque champ de données. Nous allons améliorer le contrôle DataList à utiliser à la place un `ItemTemplate` qui affiche simplement le nom du produit, catégorie, fournisseur, quantité par unité et le prix avec un `SeparatorTemplate` qui injecte un `<hr>` élément entre chaque élément. Je vais utiliser le `ItemTemplate` à partir d’un exemple dans le [affichage des données avec les contrôles DataList et Repeater](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs.md) didacticiel, mais vous pouvez utiliser le balisage de modèle, vous trouvez plus attrayante.

Après avoir apporté ces modifications, votre DataList et le balisage de son ObjectDataSource doivent ressembler à ce qui suit :

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample2.aspx)]

Prenez un moment pour consulter notre progression dans un navigateur. Lors de la première visite la page, les produits appartenant à la catégorie sélectionnée (boissons) sont affichées (comme indiqué dans la Figure 9), mais la modification de la liste DropDownList ne met à jour les données. Il s’agit, car une publication (postback) doit survenir pour que le contrôle DataList mettre à jour. Pour y parvenir nous pouvez soit définie la DropDownList `AutoPostBack` propriété `true` ou ajouter un contrôle bouton Web à la page. Pour ce didacticiel, j’ai opté pour définir la DropDownList `AutoPostBack` propriété `true`.

Les figures 9 et 10 montrent le rapport maître/détail en action.


[![Lors de la première visite la Page, les produits de boissons sont affichés.](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image21.png)

**Figure 9**: lors de la première visite la Page, les produits de boissons sont affichés ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image23.png))


[![Sélection d’un nouveau produit (produit) automatiquement provoque une publication (postback), la mise à jour le contrôle DataList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image24.png)

**Figure 10**: sélection d’un nouveau produit (produit) automatiquement provoque une publication (postback), la mise à jour le contrôle DataList ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image26.png))


## <a name="adding-a----choose-a-category----list-item"></a>Ajout d’un élément de liste «--Choisir une catégorie-- »

Lors de la visite de tout d’abord le `FilterByDropDownList.aspx` page les catégories premier élément de liste du DropDownList (boissons) est sélectionné par défaut, montrant les produits de boissons dans le contrôle DataList. Dans le *filtrage de maître/détail avec un DropDownList* didacticiel, nous avons ajouté une option «--choisir une catégorie-- » à la liste DropDownList qui a été sélectionnée par défaut et que, lorsque sélectionné, affiche *tous les* de la produits dans la base de données. Une telle approche a été facile à gérer la liste des produits dans un GridView, car chaque ligne de produit prenait une petite quantité d’espace d’écran. Avec le contrôle DataList, toutefois, les informations de chaque produit consomme un quantité segment plus grand de l’écran. Nous allons toujours ajouter une option «--choisir une catégorie-- » et l’avez sélectionnée par défaut, mais au lieu de sorte qu’il affiche tous les produits lorsque sélectionné, nous allons configurer pour qu’il n’affiche aucun produit.

Pour ajouter un nouvel élément de liste à l’objet DropDownList, accédez à la fenêtre Propriétés, puis cliquez sur le bouton de sélection dans le `Items` propriété. Ajouter un nouvel élément de liste avec la `Text` «--choisir une catégorie-- » et le `Value` `0`.


![Ajouter un](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image27.png)

**Figure 11**: ajouter un élément de liste «--Choisir une catégorie-- »


Vous pouvez également ajouter l’élément de liste en ajoutant le balisage suivant à la liste DropDownList :

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample3.aspx)]

En outre, nous devons définir le contrôle DropDownList `AppendDataBoundItems` à `true` , car si elle est définie sur `false` (la valeur par défaut), lorsque les catégories sont liés à la liste DropDownList à partir de l’ObjectDataSource elles remplacent toute liste ajoutés manuellement éléments.


![Définissez la propriété AppendDataBoundItems sur True](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image28.png)

**Figure 12**: définir le `AppendDataBoundItems` True à la propriété


La raison pour laquelle nous avons choisi la valeur `0` pour obtenir la liste «--Choisir une catégorie-- » élément est car aucune catégorie n’existe dans le système avec la valeur `0`, par conséquent, aucun enregistrement de produit n’est retournées lorsque l’élément de liste «--Choisir une catégorie-- » est sélectionné. Pour vérifier cela, prenez un moment pour visiter la page via un navigateur. Comme la Figure 13 montre, quand initialement affichant la page l’élément de liste «--Choisir une catégorie-- » est sélectionné et qu’aucun produit n’est affichés.


[![Lorsque le](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image29.png)

**Figure 13**: lorsque l’élément de liste «--Choisir une catégorie-- » est sélectionnée, les produits non sont affichés ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image31.png))


Si vous afficherait plutôt *tous les* des produits lorsque l’option «--choisir une catégorie-- » est sélectionnée, utilisez la valeur `-1` à la place. Le lecteur astucieux vous en rappeler ce serveur dans le *filtrage de maître/détail avec un DropDownList* didacticiel nous mis à jour le `ProductsBLL` la classe de `GetProductsByCategoryID(categoryID)` méthode afin que si un *`categoryID`* valeur de `-1` a été passé, produit tous les enregistrements retournés.

## <a name="summary"></a>Récapitulatif

Lors de l’affichage des données liées de manière hiérarchique, il permet souvent de présenter les données à l’aide de rapports maître/détail, à partir de laquelle l’utilisateur peut démarrer feuilletant les données à partir du haut de la hiérarchie et afficher les détails. Dans ce didacticiel, nous avons examiné la création d’un rapport maître/détail simple montrant les produits d’une catégorie sélectionnée. Cela a été accompli en utilisant un DropDownList pour la liste des catégories et un contrôle DataList pour les produits appartenant à la catégorie sélectionnée.

Dans le didacticiel suivant, nous allons examiner en séparant les enregistrements maître et détail sur deux pages. Dans la première page, une liste d’enregistrements « maîtres » s’affichera, avec un lien pour afficher les détails. En cliquant sur le lien sera tous l’utilisateur à la deuxième page, qui affiche les détails de l’enregistrement principal sélectionné.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements particuliers à...

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Randy Schmidt. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](master-detail-filtering-acess-two-pages-datalist-cs.md)
