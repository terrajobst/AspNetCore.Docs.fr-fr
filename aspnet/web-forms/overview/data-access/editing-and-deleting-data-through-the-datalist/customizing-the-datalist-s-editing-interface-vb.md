---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
title: Personnalisation du contrôle DataList d’édition Interface (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons créer une interface plus riche de modification pour le contrôle DataList, qui inclut DropDownList et une case à cocher.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 718628e2-224c-455f-b33a-a41efd48d5a0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 4ba5f532d3098eee269d97f7a317dfde7ad51680
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386655"
---
<a name="customizing-the-datalists-editing-interface-vb"></a>Personnalisation de l’Interface de modification du contrôle DataList (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_VB.exe) ou [télécharger le PDF](customizing-the-datalist-s-editing-interface-vb/_static/datatutorial40vb1.pdf)

> Dans ce didacticiel, nous allons créer une interface plus riche de modification pour le contrôle DataList, qui inclut DropDownList et une case à cocher.


## <a name="introduction"></a>Introduction

Le balisage et les contrôles Web dans le contrôle DataList s `EditItemTemplate` définir son interface modifiable. Dans tous les exemples de DataList modifiables nous ve examiné jusqu’ici, le texte modifiable interface a été composée de contrôles Web de la zone de texte. Dans le [didacticiel précédent](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md) nous avons amélioré l’expérience utilisateur de l’heure de modification en ajoutant des contrôles de validation.

Le `EditItemTemplate` peut également être étendue pour inclure les contrôles Web autre que la zone de texte, comme DropDownList, RadioButtonList, calendriers et ainsi de suite. Comme avec les zones de texte, lors de la personnalisation de l’interface de modification pour inclure d’autres contrôles Web, emploient les étapes suivantes :

1. Ajouter le contrôle Web à le `EditItemTemplate`.
2. Utilisez la syntaxe de liaison de données pour affecter la valeur de champ de données correspondant à la propriété appropriée.
3. Dans le `UpdateCommand` contrôler la valeur de gestionnaire d’événements, accéder par programme le Web et transmettez-le à la méthode appropriée de la couche BLL.

Dans ce didacticiel, nous allons créer une interface plus riche de modification pour le contrôle DataList, qui inclut DropDownList et une case à cocher. En particulier, nous allons créer un contrôle DataList qui affiche des informations de produit et autorise le nom de produit s, fournisseur, catégorie et état abandonné à mettre à jour (voir Figure 1).


[![L’Interface de modification inclut une zone de texte, deux DropDownList et une case à cocher](customizing-the-datalist-s-editing-interface-vb/_static/image2.png)](customizing-the-datalist-s-editing-interface-vb/_static/image1.png)

**Figure 1**: l’Interface de modification inclut une case à cocher, une zone de texte et deux DropDownList ([cliquez pour afficher l’image en taille réelle](customizing-the-datalist-s-editing-interface-vb/_static/image3.png))


## <a name="step-1-displaying-product-information"></a>Étape 1 : Afficher des informations de produit

Avant de pouvoir créer l’interface modifiable de s DataList, nous devons d’abord générer l’interface en lecture seule. Commencez par ouvrir le `CustomizedUI.aspx` page à partir de la `EditDeleteDataList` dossier et, à partir du concepteur, ajoutez un contrôle DataList à la page, définissant son `ID` propriété `Products`. À partir de la balise active de s DataList, créez un nouveau ObjectDataSource. Nommez cette nouvelle ObjectDataSource `ProductsDataSource` et configurez-le pour récupérer des données à partir de la `ProductsBLL` classe s `GetProducts` (méthode). Comme avec les didacticiels DataList modifiables précédents, nous mettrons à jour les informations de s produit modifié en accédant directement à la couche de logique métier. En conséquence, définissez les listes déroulantes dans la mise à jour, insertion et supprimer des onglets à (None).


[![Définir les listes déroulantes UPDATE, INSERT et DELETE onglets à (None)](customizing-the-datalist-s-editing-interface-vb/_static/image5.png)](customizing-the-datalist-s-editing-interface-vb/_static/image4.png)

**Figure 2**: la valeur est la mise à jour, insertion et supprimer des onglets liste déroulante répertorie (aucun) ([cliquez pour afficher l’image en taille réelle](customizing-the-datalist-s-editing-interface-vb/_static/image6.png))


Après avoir configuré l’ObjectDataSource, Visual Studio crée une valeur par défaut `ItemTemplate` pour le contrôle DataList qui répertorie le nom et la valeur pour chacun des champs de données retournées. Modifier le `ItemTemplate` afin que le modèle indique le nom de produit dans un `<h4>` élément ainsi que le nom de catégorie, nom du fournisseur, prix et état abandonné. En outre, ajoutez un bouton Modifier, s’assurer que ses `CommandName` propriété est définie sur Modifier. Le balisage déclaratif pour mon `ItemTemplate` suit :


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

Le balisage ci-dessus présente les informations de produit en utilisant un &lt;h4&gt; titre pour le nom du produit s et quatre colonnes `<table>` pour les champs restants. Le `ProductPropertyLabel` et `ProductPropertyValue` classes CSS, définies dans `Styles.css`, ont été abordés dans les didacticiels précédents. Figure 3 illustre notre progression lorsqu’ils sont affichés via un navigateur.


[![Le nom du fournisseur, catégorie, état abandonné et prix de chaque produit s’affiche.](customizing-the-datalist-s-editing-interface-vb/_static/image8.png)](customizing-the-datalist-s-editing-interface-vb/_static/image7.png)

**Figure 3**: le nom du fournisseur, catégorie, état abandonné et prix de chaque produit s’affiche ([cliquez pour afficher l’image en taille réelle](customizing-the-datalist-s-editing-interface-vb/_static/image9.png))


## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>Étape 2 : Ajout des contrôles Web à l’Interface de modification

La première étape de génération personnalisée DataList interface de modification consiste à ajouter des contrôles Web nécessaires à la `EditItemTemplate`. En particulier, nous avons besoin d’un contrôle DropDownList pour la catégorie, un autre pour le fournisseur et une case à cocher pour l’état abandonné. Étant donné que le prix du produit s n’est pas modifiable dans cet exemple, nous pouvons continuer à afficher à l’aide d’un contrôle Web Label.

Pour personnaliser l’interface de modification, cliquez sur le lien Modifier les modèles à partir de la balise active DataList s et choisissez la `EditItemTemplate` option dans la liste déroulante. Ajouter un contrôle DropDownList pour la `EditItemTemplate` et définissez son `ID` à `Categories`.


[![Ajouter un contrôle DropDownList pour les catégories](customizing-the-datalist-s-editing-interface-vb/_static/image11.png)](customizing-the-datalist-s-editing-interface-vb/_static/image10.png)

**Figure 4**: ajouter un contrôle DropDownList pour les catégories ([cliquez pour afficher l’image en taille réelle](customizing-the-datalist-s-editing-interface-vb/_static/image12.png))


Ensuite, à partir de la balise active de s DropDownList, sélectionnez l’option de choisir la Source de données et créer un nouveau ObjectDataSource nommé `CategoriesDataSource`. Configurer cette ObjectDataSource à utiliser le `CategoriesBLL` classe s `GetCategories()` (méthode) (voir Figure 5). Ensuite, les opérations de mappage DropDownList Assistant de Configuration de Source de données vous invite à entrer pour les champs de données à utiliser pour chaque `ListItem` s `Text` et `Value` propriétés. Affiche la liste DropDownList le `CategoryName` champ de données et utilisez le `CategoryID` comme valeur, comme illustré dans la Figure 6.


[![Créer un nouveau ObjectDataSource nommé CategoriesDataSource](customizing-the-datalist-s-editing-interface-vb/_static/image14.png)](customizing-the-datalist-s-editing-interface-vb/_static/image13.png)

**Figure 5**: créer une nouvelle nommée de ObjectDataSource `CategoriesDataSource` ([cliquez pour afficher l’image en taille réelle](customizing-the-datalist-s-editing-interface-vb/_static/image15.png))


[![Configurer l’affichage de s DropDownList et que la valeur des champs](customizing-the-datalist-s-editing-interface-vb/_static/image17.png)](customizing-the-datalist-s-editing-interface-vb/_static/image16.png)

**Figure 6**: configurer les champs de valeur et un DropDownList s affichage ([cliquez pour afficher l’image en taille réelle](customizing-the-datalist-s-editing-interface-vb/_static/image18.png))


Répétez cette série d’étapes pour créer un contrôle DropDownList pour les fournisseurs. Définir le `ID` pour cette DropDownList pour `Suppliers` et nommez son ObjectDataSource `SuppliersDataSource`.

Après avoir ajouté les deux DropDownList, ajouter une case à cocher pour l’état abandonné et une zone de texte Nom de produit s. Définir le `ID` s pour la case à cocher et zone de texte pour `Discontinued` et `ProductName`, respectivement. Ajoutez un contrôle RequiredFieldValidator pour vous assurer que l’utilisateur fournit une valeur pour le nom du produit s.

Enfin, ajoutez les boutons de la mise à jour et Annuler. N’oubliez pas que, pour ces deux boutons, il est impératif que leurs `CommandName` propriétés sont définies pour mettre à jour et Annuler, respectivement.

N’hésitez pas à disposer de l’interface de modification comme vous le souhaitez. Je ve choisi d’utiliser les mêmes quatre colonnes `<table>` illustre la disposition de l’interface en lecture seule, en tant que la syntaxe déclarative suivante et la capture d’écran :


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample2.aspx)]


[![L’Interface de modification est définie comme l’Interface en lecture seule](customizing-the-datalist-s-editing-interface-vb/_static/image20.png)](customizing-the-datalist-s-editing-interface-vb/_static/image19.png)

**Figure 7**: l’Interface de modification est définie comme l’Interface en lecture seule ([cliquez pour afficher l’image en taille réelle](customizing-the-datalist-s-editing-interface-vb/_static/image21.png))


## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>Étape 3 : Créer les gestionnaires d’événements de CancelCommand EditCommand

Actuellement, il n’existe aucune syntaxe de liaison de données dans le `EditItemTemplate` (à l’exception de la `UnitPriceLabel`, qui a été copié mot pour mot à partir de la `ItemTemplate`). Nous allons ajouter momentanément la syntaxe de liaison de données, mais laissez première s créer les gestionnaires d’événements pour le contrôle DataList s `EditCommand` et `CancelCommand` événements. N’oubliez pas que la responsabilité de la `EditCommand` Gestionnaire d’événements consiste à rendre l’interface de modification pour l’élément DataList dont bouton Modifier, tandis que le `CancelCommand` travail de s est de retourner le contrôle DataList à son état avant modification.

Créez ces deux gestionnaires d’événements et demandez-leur d’utiliser le code suivant :


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample3.vb)]

Avec ces deux gestionnaires d’événements en place, en cliquant sur le bouton Modifier affiche l’interface de modification et en cliquant sur le bouton Annuler retourne l’élément modifié en mode en lecture seule. La figure 8 illustre le contrôle DataList une fois que le bouton Modifier a été cliqué pour Chef Anton s Gumbo Mix. Dans la mesure où ve encore pour ajouter une syntaxe de liaison de données à l’interface de modification, le `ProductName` zone de texte est vide, le `Discontinued` case à cocher désactivée et les premiers éléments sélectionnés à partir de la `Categories` et `Suppliers` DropDownList.


[![En cliquant sur le bouton Edition affiche l’Interface de modification](customizing-the-datalist-s-editing-interface-vb/_static/image23.png)](customizing-the-datalist-s-editing-interface-vb/_static/image22.png)

**Figure 8**: en cliquant sur le bouton Modifier affiche l’Interface de modification ([cliquez pour afficher l’image en taille réelle](customizing-the-datalist-s-editing-interface-vb/_static/image24.png))


## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>Étape 4 : Ajout de la syntaxe de liaison de données à l’Interface de modification

Pour afficher les valeurs de s produit actuel dans l’interface de modification, nous devons utiliser la syntaxe de liaison de données pour affecter les valeurs de champ de données pour les valeurs de contrôle Web appropriées. La syntaxe de liaison de données peut être appliquée via le concepteur en accédant à l’écran Modifier les modèles et en sélectionnant le lien Modifier les DataBindings à partir du Web de contrôle des balises actives. Vous pouvez également la syntaxe de liaison de données peut être ajoutée directement au balisage déclaratif.

Affecter le `ProductName` valeur au champ de données le `ProductName` s de la zone de texte `Text` propriété, le `CategoryID` et `SupplierID` valeurs de champ de données le `Categories` et `Suppliers` DropDownList `SelectedValue` propriétés et le `Discontinued` données champ valeur pour le `Discontinued` case à cocher s `Checked` propriété. Après avoir apporté ces modifications, par le biais du concepteur ou directement le balisage déclaratif, visitez la page via un navigateur et cliquez sur le bouton Modifier pour Chef Anton s Gumbo Mix. Comme le montre la Figure 9, la syntaxe de liaison de données a ajouté les valeurs actuelles dans la zone de texte, DropDownList, case à cocher.


[![En cliquant sur le bouton Edition affiche l’Interface de modification](customizing-the-datalist-s-editing-interface-vb/_static/image26.png)](customizing-the-datalist-s-editing-interface-vb/_static/image25.png)

**Figure 9**: en cliquant sur le bouton Modifier affiche l’Interface de modification ([cliquez pour afficher l’image en taille réelle](customizing-the-datalist-s-editing-interface-vb/_static/image27.png))


## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>Étape 5 : Enregistrer les modifications de s utilisateur dans le Gestionnaire d’événements de UpdateCommand

Lorsque l’utilisateur modifie un produit et clique sur le bouton de mise à jour, une publication (postback) se produit et le contrôle DataList s `UpdateCommand` événement est déclenché. Dans l’événement gestionnaire, que nous devons lire les valeurs des contrôles Web dans le `EditItemTemplate` et l’interface avec la couche BLL à mettre à jour le produit dans la base de données. Comme nous ve vu dans les didacticiels précédents, le `ProductID` du produit mis à jour est accessible via la `DataKeys` collection. Les champs entré par l’utilisateur sont accessibles en référençant par programmation les contrôles Web à l’aide de `FindControl("controlID")`, comme illustré dans le code suivant :


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample4.vb)]

Le code commence par le Conseil le `Page.IsValid` propriété pour garantir que tous les contrôles de validation sur la page sont valides. Si `Page.IsValid` est `True`, le produit modifié s `ProductID` valeur est lue à partir de la `DataKeys` collecte et l’entrée de données que les contrôles Web dans le `EditItemTemplate` sont référencés par programmation. Ensuite, les valeurs à partir de ces contrôles Web sont lues dans des variables qui sont ensuite transmis dans approprié `UpdateProduct` de surcharge. Après la mise à jour les données, le contrôle DataList est rétabli à son état avant modification.

> [!NOTE]
> Je ve omis les exceptions logique ajoutée à la [BLL - gestion et les Exceptions au niveau de la couche DAL](handling-bll-and-dal-level-exceptions-vb.md) didacticiel afin de conserver le code et cet exemple s’est concentré. En guise d’exercice, ajoutez cette fonctionnalité après avoir terminé ce didacticiel.


## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>Étape 6 : Gestion des CategoryID NULL et les valeurs SupplierID

Permet à la base de données Northwind pour `NULL` valeurs pour le `Products` table s `CategoryID` et `SupplierID` colonnes. Toutefois, notre édition t de n interface adapter `NULL` valeurs. Si nous tentons de modifier un produit qui a un `NULL` valeur pour une sa `CategoryID` ou `SupplierID` colonnes, nous obtenons un `ArgumentOutOfRangeException` avec un message d’erreur similaire à : *« Categories » a un SelectedValue qui n’est pas valide, car c’est le cas n’existe pas dans la liste des éléments.* En outre, s’il y a actuellement aucun moyen de modifier une catégorie de produit s ou d’un fournisseur ne valeur non -`NULL` valeur à un `NULL` une.

Pour prendre en charge `NULL` valeurs pour la catégorie et le fournisseur DropDownList, nous devons ajouter un autre `ListItem`. Je ve choisi d’utiliser (aucun), comme le `Text` valeur pour ce `ListItem`, mais vous pouvez le modifier à quelque chose d’autre (par exemple, une chaîne vide) si vous d comme. Enfin, n’oubliez pas de définir le DropDownList `AppendDataBoundItems` à `True`; si vous oubliez de le faire, les catégories et fournisseurs liés à la liste DropDownList remplacera le-ajoutés de façon statique `ListItem`.

Après avoir apporté ces modifications, le balisage DropDownList dans le contrôle DataList s `EditItemTemplate` doit ressembler à ce qui suit :


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> Statique `ListItem` s peuvent être ajoutés à un contrôle DropDownList via le concepteur ou directement par le biais de la syntaxe déclarative. Lorsque vous ajoutez un élément DropDownList pour représenter une base de données `NULL` valeur, veillez à ajouter la `ListItem` via la syntaxe déclarative. Si vous utilisez le `ListItem` éditeur de collections dans le concepteur, la syntaxe déclarative générée sera omettre la `Value` paramètre complètement lorsque affecté à une chaîne vide, en créant un balisage déclaratif telles que : `<asp:ListItem>(None)</asp:ListItem>`. Bien que cela peut vous paraître inoffensif, manquants `Value` provoque la liste DropDownList à utiliser le `Text` valeur de propriété à la place. Qui signifie que si cela `NULL` `ListItem` est sélectionnée, la valeur (aucun) sera tentée à affecter au champ de données de produit (`CategoryID` ou `SupplierID`, dans ce didacticiel), ce qui entraînera une exception. En définissant explicitement `Value=""`, un `NULL` valeur sera affectée au produit du champ de données lorsque le `NULL` `ListItem` est sélectionné.


Prenez un moment pour consulter notre progression via un navigateur. Lorsque vous modifiez un produit, notez que le `Categories` et `Suppliers` DropDownList les deux avoir la valeur (aucune) option au début de la liste DropDownList.


[![Les catégories et les fournisseurs DropDownList comprennent un (aucun) Option](customizing-the-datalist-s-editing-interface-vb/_static/image29.png)](customizing-the-datalist-s-editing-interface-vb/_static/image28.png)

**Figure 10**: le `Categories` et `Suppliers` DropDownList comprennent un (aucun) Option ([cliquez pour afficher l’image en taille réelle](customizing-the-datalist-s-editing-interface-vb/_static/image30.png))


Pour enregistrer (aucun) option comme une base de données `NULL` valeur, nous devons revenir à la `UpdateCommand` Gestionnaire d’événements. Modifier le `categoryIDValue` et `supplierIDValue` variables nullables entiers et les affecter une valeur autre que `Nothing` uniquement si les opérations de mappage DropDownList `SelectedValue` n’est pas une chaîne vide :


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample6.vb)]

Avec cette modification, la valeur `Nothing` sera passé dans le `UpdateProduct` méthode BLL si l’utilisateur sélectionné (aucun) option à partir des listes de liste déroulante, qui correspond à un `NULL` valeur de base de données.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons vu comment créer une interface d’édition DataList plus complexe qui inclut trois contrôles Web d’entrée différents une zone de texte, deux DropDownList et une case à cocher, ainsi que des contrôles de validation. Lorsque vous créez l’interface de modification, les étapes sont identiques pour tous les contrôles Web utilisés : commencez par ajouter les contrôles Web pour le contrôle DataList s `EditItemTemplate`; utiliser la syntaxe de liaison de données pour affecter les valeurs de champ de données correspondantes avec le site Web approprié Propriétés du contrôle ; puis, dans le `UpdateCommand` Gestionnaire d’événements, accéder par programmation les contrôles Web et leurs propriétés appropriées, en passant de leurs valeurs dans la couche BLL.

Lorsque vous créez une interface de modification, si elle s composé des zones de texte ou une collection de contrôles Web différents, veillez à gérer correctement la base de données `NULL` valeurs. Pour la comptabilité de `NULL` s, il est impératif que vous n’est pas uniquement correctement afficher existant `NULL` valeur dans l’interface de modification, mais également que vous offrent un moyen pour le marquage d’une valeur en tant que `NULL`. Pour DropDownList dans DataList, cela signifie généralement que l’Ajout statique `ListItem` dont `Value` propriété est explicitement définie sur une chaîne vide (`Value=""`) et l’ajout d’un peu de code pour le `UpdateCommand` Gestionnaire d’événements pour déterminer ou non le `NULL``ListItem` a été sélectionné.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Dennis Patterson, David Suru et Randy Schmidt. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
