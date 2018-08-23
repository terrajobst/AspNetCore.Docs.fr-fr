---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
title: Maître/détail filtrage entre les deux Pages (c#) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons comment séparer un rapport maître/détail sur deux pages. Dans la page 'master', nous utilisons un contrôle Repeater pour afficher une liste de categ...
ms.author: riande
ms.date: 10/30/2010
ms.assetid: 68b8c023-92fa-4df6-9563-1764e16e4b04
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 6d4f38201eaa25097dcba589f35506204d3f73a3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838419"
---
<a name="masterdetail-filtering-across-two-pages-c"></a>Maître/détail filtrage entre les deux Pages (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_CS.exe) ou [télécharger le PDF](master-detail-filtering-acess-two-pages-datalist-cs/_static/datatutorial34cs1.pdf)

> Dans ce didacticiel, nous allons comment séparer un rapport maître/détail sur deux pages. Dans la page « maître », nous utilisons un contrôle Repeater pour afficher une liste de catégories qui, quand vous cliquez sur, dirige l’utilisateur à la page « Détails » où une DataList des deux colonnes affiche les produits appartenant à la catégorie sélectionnée.


## <a name="introduction"></a>Introduction

Dans le [didacticiel précédent](master-detail-filtering-with-a-dropdownlist-datalist-cs.md) nous avons vu comment afficher les rapports maître/détail dans une seule page web à l’aide de DropDownList pour afficher les enregistrements « maîtres » et un contrôle DataList pour afficher les « détails ». Un autre modèle commun utilisé pour les rapports maître/détail est d’avoir les enregistrements principaux sur une page web et les détails sur un autre. Dans l’ancien [filtrage de maître/détail sur deux Pages](../masterdetail/master-detail-filtering-across-two-pages-cs.md) didacticiel, nous avons examiné ce modèle à l’aide d’un GridView pour afficher tous les fournisseurs dans le système. Ce GridView inclus un HyperLinkField, ce qui est restitué sous la forme d’un lien vers une deuxième page, en transmettant le `SupplierID` dans la chaîne de requête. La deuxième page permet de répertorier les produits fournis par le fournisseur sélectionné un GridView.

Ces rapports de deux pages maître/détail peuvent être accomplies à l’aide des contrôles DataList et Repeater. La seule différence est que le contrôle DataList, ni le Repeater prend en charge le contrôle HyperLinkField. Au lieu de cela, nous devons ajouter un contrôle de lien hypertexte Web ou un élément d’ancrage HTML (`<a>`) dans le contrôle `ItemTemplate`. Du lien hypertexte `NavigateUrl` propriété ou le point d’ancrage `href` attribut peut ensuite être personnalisé à l’aide des approches déclaratives ou par programmation.

Dans ce didacticiel, nous allons découvrir un exemple qui répertorie les catégories dans une liste à puces sur une page à l’aide d’un contrôle Repeater. Chaque élément de liste inclura les nom et une description, la catégorie avec le nom de catégorie affiché sous la forme d’un lien vers une deuxième page. En cliquant sur ce lien sera tous l’utilisateur à la deuxième page, où un contrôle DataList affiche les produits qui appartiennent à la catégorie sélectionnée.

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>Étape 1 : Afficher les catégories dans une liste à puces

La première étape de création d’un rapport maître/détail consiste à démarrer en affichant les enregistrements « maîtres ». Par conséquent, notre première tâche consiste à afficher les catégories dans la page « maître ». Ouvrez le `CategoryListMaster.aspx` page dans le `DataListRepeaterFiltering` dossier, ajoutez un contrôle Repeater et, à partir de la balise active, choisir d’ajouter un nouveau ObjectDataSource. Configurer le nouveau ObjectDataSource afin qu’elle accède à ses données à partir de la `CategoriesBLL` la classe `GetCategories` (méthode) (voir Figure 1).


[![Configurer pour utiliser méthode la classe CategoriesBLL GetCategories ObjectDataSource](master-detail-filtering-acess-two-pages-datalist-cs/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image1.png)

**Figure 1**: configurer ObjectDataSource à utiliser le `CategoriesBLL` la classe `GetCategories` (méthode) ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-acess-two-pages-datalist-cs/_static/image3.png))


Ensuite, définissez les modèles de répéteur tel qu’il affiche chaque nom de catégorie et la description sous la forme d’un élément dans une liste à puces. Nous allons pas encore soucier de disposer chaque catégorie de lien vers la page de détails. Voici le balisage déclaratif pour le Repeater et ObjectDataSource :

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample1.aspx)]

Avec ce balisage complet, prenez un moment pour consulter notre progression via un navigateur. Comme le montre la Figure 2, le contrôle Repeater est rendu sous la forme d’une liste à puces montrant le nom et la description de chaque catégorie.


[![Chaque catégorie est affichée comme un élément de liste à puces](master-detail-filtering-acess-two-pages-datalist-cs/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image4.png)

**Figure 2**: chaque catégorie est affichée comme un élément de liste à puces ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-acess-two-pages-datalist-cs/_static/image6.png))


## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>Étape 2 : Activer le nom de catégorie dans un lien vers la Page de détails

Pour autoriser un utilisateur afficher les informations de « détails » pour une catégorie donnée, nous devons ajouter un lien à chaque liste à puces d’élément qui, quand vous cliquez sur, dirige l’utilisateur à la deuxième page (`ProductsForCategoryDetails.aspx`). Cette deuxième page puis affichera les produits pour la catégorie sélectionnée à l’aide d’un contrôle DataList. Afin de déterminer la catégorie dont le lien de l’utilisateur a cliqué, nous devons transmettre la catégorie utilisateur a cliqué dessue `CategoryID` à la deuxième page via un mécanisme. La façon la plus simple, plus simple pour transférer des données scalaires à partir d’une page à l’autre est via la chaîne de requête, qui est l’option que nous allons utiliser dans ce didacticiel. En particulier, le `ProductsForCategoryDetails.aspx` page attendra sélectionné *`categoryID`* valeur doit être passé à un champ de chaîne de requête nommé `CategoryID`. Par exemple, pour afficher les produits pour la catégorie boissons, qui a un `CategoryID` 1, un utilisateur visite `ProductsForCategoryDetails.aspx?CategoryID=1`.

Pour créer un lien hypertexte pour chaque élément de liste à puces dans le répéteur, nous devons ajouter un contrôle de lien hypertexte Web ou un élément d’ancrage HTML (`<a>`) pour le `ItemTemplate`. Dans les scénarios où le lien hypertexte est affichent le même pour chaque ligne, chacune de ces approches est suffisante. Pour des répéteurs, je préfère à l’aide de l’élément d’ancrage. Pour utiliser l’élément d’ancrage, mettez à jour ItemTemplate de répéteur :

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample2.aspx)]

Notez que le `CategoryID` peut être injectée directement dans l’élément d’ancrage `href` attribut ; Toutefois, pour veillez donc à délimiter la `href` valeur de l’attribut avec les apostrophes (et notez les guillemets) depuis le `Eval` (méthode) dans le `href` attribut délimite sa chaîne (`"CategoryID"`) avec des guillemets. Vous pouvez également un contrôle de lien hypertexte Web peut servir à la place :

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample3.aspx)]

Notez comment la partie statique de l’URL : `ProductsForCategoryDetails.aspx?CategoryID` , est ajouté au résultat de `Eval("CategoryID")` directement dans la syntaxe de liaison de données à l’aide de la concaténation de chaînes.

L’un des avantages de l’utilisation du contrôle de lien hypertexte sont qu’il est par programmation accessible à partir du Repeater `ItemDataBound` Gestionnaire d’événements, si nécessaire. Par exemple, vous souhaiterez peut-être afficher le nom de catégorie sous forme de texte plutôt que sous forme de lien pour les catégories avec aucun produit associé. Cette vérification puisse être effectuée par programme dans le `ItemDataBound` Gestionnaire d’événements ; pour les catégories sans aucune associé produits, le lien hypertexte `NavigateUrl` propriété peut être définie sur une chaîne vide, ce qui entraîne ce nom de catégorie particulière rendu au format texte brut (plutôt que sous forme de lien). Faire référence à la [mise en forme les contrôles DataList et Repeater sur données](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md) didacticiel pour plus d’informations sur la mise en forme les contrôles DataList et du Repeater contenu basé sur la logique de programmation via la `ItemDataBound` Gestionnaire d’événements.

Si vous suivez, n’hésitez pas à utiliser l’élément d’ancrage ou une approche de contrôle de lien hypertexte dans votre page. Quelle que soit l’approche, lorsque vous affichez la page via un navigateur chaque nom de catégorie doit être restitué sous forme de lien à `ProductsForCategoryDetails.aspx`, en passant l’applicable `CategoryID` valeur (voir Figure 3).


[![Les noms de catégorie maintenant lier à ProductsForCategoryDetails.aspx](master-detail-filtering-acess-two-pages-datalist-cs/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image7.png)

**Figure 3**: la catégorie noms maintenant lien vers `ProductsForCategoryDetails.aspx` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-acess-two-pages-datalist-cs/_static/image9.png))


## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>Étape 3 : Répertorier les produits qui appartiennent à la catégorie sélectionnée

Avec le `CategoryListMaster.aspx` page terminée, nous sommes prêts à porter notre attention vers l’implémentation de la page « Détails », `ProductsForCategoryDetails.aspx`. Ouvrir cette page, faites glisser un contrôle DataList à partir de la boîte à outils vers le concepteur et définissez son `ID` propriété `ProductsInCategory`. Ensuite, choisissez à partir de la balise active du contrôle DataList ajouter un nouveau ObjectDataSource à la page, en nommant `ProductsInCategoryDataSource`. Configurez-le de sorte qu’elle appelle le `ProductsBLL` la classe `GetProductsByCategoryID(categoryID)` méthode ; définir la liste déroulante répertorie dans les onglets INSERT, UPDATE et DELETE (None).


[![Configurer pour utiliser GetProductsByCategoryID(categoryID) méthode la classe ProductsBLL ObjectDataSource](master-detail-filtering-acess-two-pages-datalist-cs/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image10.png)

**Figure 4**: configurer ObjectDataSource à utiliser le `ProductsBLL` la classe `GetProductsByCategoryID(categoryID)` (méthode) ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-acess-two-pages-datalist-cs/_static/image12.png))


Dans la mesure où le `GetProductsByCategoryID(categoryID)` méthode accepte un paramètre d’entrée (*`categoryID`*), l’Assistant de choisir la Source de données nous offre une opportunité pour spécifier la source du paramètre. Définissez la source de paramètre de chaîne de requête à l’aide de la QueryStringField `CategoryID`.


[![Utilisez la CategoryID de champ de chaîne de requête en tant que Source du paramètre](master-detail-filtering-acess-two-pages-datalist-cs/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image13.png)

**Figure 5**: utilisez le Querystring Field `CategoryID` en tant que Source du paramètre ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-acess-two-pages-datalist-cs/_static/image15.png))


Comme nous l’avons vu dans les didacticiels précédents, après la fin de l’Assistant de choisir la Source de données, Visual Studio crée automatiquement un `ItemTemplate` pour le contrôle DataList qui répertorie chaque nom de champ de données et la valeur. Remplacez ce modèle avec l’une qui répertorie uniquement le produit nom, fournisseur et prix. En outre, définissez la DataList `RepeatColumns` propriété à 2. Après ces modifications, vos contrôles DataList et l’ObjectDataSource balisage déclaratif doit ressembler à ce qui suit :

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample4.aspx)]

Pour afficher cette page en action, démarrez à partir de la `CategoryListMaster.aspx` page ; ensuite, cliquez sur un lien dans la liste à puces des catégories. Cela vous dirigera vers `ProductsForCategoryDetails.aspx`, en passant le long de le `CategoryID` via la chaîne de requête. Le `ProductsInCategoryDataSource` ObjectDataSource dans `ProductsForCategoryDetails.aspx` ensuite obtenir uniquement les produits de la catégorie spécifiée et les afficher dans le contrôle DataList, qui affiche les deux produits par ligne. La figure 6 présente une capture d’écran de `ProductsForCategoryDetails.aspx` lorsque vous affichez les boissons.


[![Les boissons sont affichés, deux par ligne](master-detail-filtering-acess-two-pages-datalist-cs/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image16.png)

**Figure 6**: les boissons sont affichés, deux par ligne ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-acess-two-pages-datalist-cs/_static/image18.png))


## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>Étape 4 : Affichage des informations de catégorie sur ProductsForCategoryDetails.aspx

Lorsqu’un utilisateur clique sur une catégorie dans `CategoryListMaster.aspx`, il est dirigé vers `ProductsForCategoryDetails.aspx` et affiche les produits qui appartiennent à la catégorie sélectionnée. Toutefois, dans `ProductsForCategoryDetails.aspx` il n’y a aucune signaux visuels quant à quelle catégorie a été sélectionné. Un utilisateur destiné à cliquer sur les boissons, mais les Condiments accidentellement cliqué dessus, qui n’a aucun moyen de rendre compte de son erreur lorsqu’ils atteignent leur `ProductsForCategoryDetails.aspx`. Pour pallier ce problème, nous pouvons afficher des informations sur la catégorie sélectionnée, son nom et sa description, en haut de la `ProductsForCategoryDetails.aspx` page.

Pour ce faire, ajoutez un FormView au-dessus du contrôle Repeater dans `ProductsForCategoryDetails.aspx`. Ensuite, ajoutez un nouveau ObjectDataSource à la page à partir de la balise active du FormView nommé `CategoryDataSource` et configurez-le pour utiliser le `CategoriesBLL` la classe `GetCategoryByCategoryID(categoryID)` (méthode).


[![Accéder aux informations sur la catégorie par le biais GetCategoryByCategoryID(categoryID) (méthode de la classe CategoriesBLL)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image19.png)

**Figure 7**: accéder aux informations sur la catégorie via la `CategoriesBLL` la classe `GetCategoryByCategoryID(categoryID)` (méthode) ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-acess-two-pages-datalist-cs/_static/image21.png))


Comme avec la `ProductsInCategoryDataSource` ObjectDataSource ajouté à l’étape 3, le `CategoryDataSource`d’Assistant Configurer la Source de données nous demande une source pour le `GetCategoryByCategoryID(categoryID)` paramètre d’entrée (méthode). Utiliser les mêmes paramètres que précédemment, la définition de la source de paramètre de chaîne de requête et la valeur de QueryStringField à `CategoryID` (voir la Figure 5).

À l’issue de l’Assistant, Visual Studio crée automatiquement un `ItemTemplate`, `EditItemTemplate`, et `InsertItemTemplate` pour le contrôle FormView. Étant donné que nous fournissons une interface en lecture seule, n’hésitez pas à supprimer le `EditItemTemplate` et `InsertItemTemplate`. En outre, n’hésitez pas à personnaliser le FormView `ItemTemplate`. Après avoir supprimé les modèles superflus et personnalisation ItemTemplate, FormView et de l’ObjectDataSource balisage déclaratif doit ressembler à ce qui suit :

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample5.aspx)]

La figure 8 illustre une capture lors de l’affichage de cette page via un navigateur.

> [!NOTE]
> Outre le contrôle FormView, j’ai également ajouté un contrôle de lien hypertexte ci-dessus FormView qui dirige l’utilisateur à la liste des catégories (`CategoryListMaster.aspx`). N’hésitez pas à placer ce lien ailleurs ou à ne pas l’utiliser.


[![Les informations de catégorie sont maintenant affiché en haut de la Page](master-detail-filtering-acess-two-pages-datalist-cs/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image22.png)

**Figure 8**: les informations de catégorie est maintenant affiché en haut de la Page ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-acess-two-pages-datalist-cs/_static/image24.png))


## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>Étape 5 : Affichage d’un Message si aucun produit n’appartiennent à la catégorie sélectionnée

Le `CategoryListMaster.aspx` page répertorie toutes les catégories dans le système, quel que soit s’il existe des associé des produits. Si un utilisateur clique sur une catégorie avec aucun produits associés, le contrôle DataList dans `ProductsForCategoryDetails.aspx` ne sera pas affiché, comme sa source de données n’aura pas tous les éléments. Comme nous l’avons vu dans les didacticiels passées, le contrôle GridView fournit un `EmptyDataText` propriété qui peut être utilisée pour spécifier un message texte à afficher si aucun enregistrement n’est dans sa source de données. Malheureusement, ni le contrôle DataList ni le Repeater a une telle propriété.

Pour afficher un message informant l’utilisateur qu’aucun produit correspondant pour la catégorie sélectionnée, nous devons ajouter une étiquette de contrôle à la page dont `Text` le message à afficher dans le cas où aucun produit n’est assigné à la propriété. Nous devons définir par programmation son `Visible` propriété selon ou non le contrôle DataList contienne tous les éléments.

Pour ce faire, commencez par ajouter une étiquette sous le contrôle DataList. Définissez ses `ID` propriété `NoProductsMessage` et son `Text` propriété à « Il n’y aucun produit pour la catégorie sélectionnée... » Ensuite, nous devons définir par programmation de cette étiquette `Visible` propriété selon ou non des données a été liées à la `ProductsInCategory` DataList. Cette attribution doit être effectuée une fois que les données a été liées à la DataList. Pour le GridView, DetailsView et FormView, nous pourrions créer un gestionnaire d’événements pour le contrôle `DataBound` événement, ce qui se déclenche après la liaison de données est terminée. Toutefois, le contrôle DataList, ni le Repeater a un `DataBound` événements disponibles.

Pour cet exemple particulier, nous pouvons attribuer l’étiquette `Visible` propriété dans le `Page_Load` Gestionnaire d’événements, dans la mesure où les données seront affectées à la DataList avant que la page `Load` événement. Toutefois, cette approche fonctionnerait pas dans le cas général, comme les données à partir de l’ObjectDataSource peuvent être liées à la DataList plus loin dans le cycle de vie de la page. Par exemple, si les données affichées sont basées sur la valeur dans un autre contrôle, comme il est lors de l’affichage d’un rapport maître/détail à l’aide d’un contrôle DropDownList pour contenir les enregistrements « maîtres », les données ne peuvent pas liées une nouvelle pour le contrôle Web de données jusqu'à ce que le `PreRender` à l’étape dans le cycle de vie de la page.

Une solution qui fonctionne pour tous les cas consiste à affecter la `Visible` propriété `False` dans la DataList `ItemDataBound` (ou `ItemCreated`) lors de la liaison d’un type d’élément de gestionnaire d’événements `Item` ou `AlternatingItem`. Dans ce cas nous savons qu’il existe des données au moins un élément dans la source de données et par conséquent peut masquer la `NoProductsMessage` étiquette. En plus de ce gestionnaire d’événements, nous avons également besoin d’un gestionnaire d’événements pour la DataList `DataBinding` événement, où nous initialisons l’étiquette `Visible` propriété `True`. Dans la mesure où le `DataBinding` événement est déclenché avant le `ItemDataBound` d’événements, l’étiquette `Visible` propriété sera initialement définie `True`; s’il existe des éléments de données, toutefois, elle sera définie `False`. Le code suivant implémente cette logique :

[!code-csharp[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample6.cs)]

Toutes les catégories dans la base de données Northwind sont associés à un ou plusieurs produits. Pour tester cette fonctionnalité, j’ai ajusté manuellement la base de données Northwind pour ce didacticiel, la réaffectation de tous les produits de la catégorie de produit (`CategoryID` = 7) à la catégorie Seafood (`CategoryID` = 8). Cela est possible à partir de l’Explorateur de serveurs en choisissant nouvelle requête et en utilisant la commande suivante `UPDATE` instruction :

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample7.sql)]

Après la mise à jour la base de données en conséquence, revenez à la `CategoryListMaster.aspx` page et cliquez sur le lien du produit. Dans la mesure où il ne sont plus tous les produits appartenant à la catégorie de produit, vous devez voir le message « Il n’y aucun produit pour la catégorie sélectionnée... », comme illustré à la Figure 9.


[![Un Message s’affiche s’il existe non produits appartenant à la catégorie sélectionnée](master-detail-filtering-acess-two-pages-datalist-cs/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image25.png)

**Figure 9**: un Message s’affiche s’il existe non produits appartenant à la catégorie sélectionnée ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-acess-two-pages-datalist-cs/_static/image27.png))


## <a name="summary"></a>Récapitulatif

Les rapports maître/détail peuvent afficher les enregistrements maître et détail sur une seule page, dans de nombreux sites Web qu’ils sont séparés et répartis entre deux pages web. Dans ce didacticiel, nous avons vu comment implémenter ce rapport maître/détail en donnant les catégories répertoriées dans une liste à puces à l’aide d’un répéteur dans la page web « maître » et les produits associés, répertoriés dans la page « Détails ». Chaque élément de liste dans la page web maître contenait un lien vers la page de détails passé le long de la ligne `CategoryID` valeur.

Dans la page Détails de la récupération de ces produits pour le fournisseur spécifié était réalisée via la `ProductsBLL` la classe `GetProductsByCategoryID(categoryID)` (méthode). Le *`categoryID`* valeur du paramètre a été spécifiée de façon déclarative à l’aide de la `CategoryID` valeur de chaîne de requête en tant que le paramètre source. Nous avons également étudié comment afficher les détails de catégorie dans la page de détails à l’aide d’un FormView et comment afficher un message si aucun produit appartenant à la catégorie sélectionnée.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements particuliers à...

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Zack Jones et Liz Shulok. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](master-detail-filtering-with-a-dropdownlist-datalist-cs.md)
> [Suivant](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
