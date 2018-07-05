---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
title: Personnalisation de l’Interface de Modification de données (c#) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons examiner comment personnaliser l’interface d’un GridView modifiable, en remplaçant la zone de texte standard et contrôle de case à cocher avec secondaire...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 22e99600-8d18-4a94-a20e-a3a62bb63798
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 1f8f08fc78ef5cf6d6878daf6b00d6556f585762
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383715"
---
<a name="customizing-the-data-modification-interface-c"></a>Personnalisation de l’Interface de Modification de données (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_CS.exe) ou [télécharger le PDF](customizing-the-data-modification-interface-cs/_static/datatutorial20cs1.pdf)

> Dans ce didacticiel, nous allons examiner comment personnaliser l’interface d’un GridView modifiable, en remplaçant la zone de texte standard et contrôle de case à cocher avec d’autres contrôles Web d’entrée.


## <a name="introduction"></a>Introduction

Le BoundFields et CheckBoxFields utilisés par les contrôles GridView et DetailsView simplifier le processus de modification des données en raison de leur capacité à rendre les interfaces en lecture seule, modifiables et pouvant être insérés. Ces interfaces peuvent être rendus sans avoir besoin d’ajouter de balisage déclaratif supplémentaire ou de code. Toutefois, les interfaces le BoundField et du CheckBoxField ne disposent pas les possibilités de personnalisation fréquemment utilisées dans les scénarios réels. Pour personnaliser l’interface modifiable ou insérable dans un GridView ou DetailsView un que nous devons plutôt utiliser TemplateField.

Dans le [didacticiel précédent](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) nous avons vu comment personnaliser les interfaces de modification de données en ajoutant des contrôles Web de validation. Dans ce didacticiel, nous allons examiner comment personnaliser les contrôles pour le Web des collection de données réelles, en remplaçant le BoundField et zone de texte standard du CheckBoxField et les contrôles CheckBox avec d’autres contrôles Web d’entrée. En particulier, nous allons créer un GridView modifiable qui permet d’un produit nom, catégorie, fournisseur et état abandonné à mettre à jour. Lorsque vous modifiez une ligne particulière, les champs de catégorie et le fournisseur affichera comme DropDownList, contenant l’ensemble de catégories disponibles et les fournisseurs sélectionnables. En outre, nous allons remplacer la valeur par défaut de la CheckBoxField case à cocher avec un contrôle RadioButtonList qui propose deux options : « Actif » et « Supprimées ».


[![Interface de modification du contrôle GridView inclut DropDownList et cases d’option](customizing-the-data-modification-interface-cs/_static/image2.png)](customizing-the-data-modification-interface-cs/_static/image1.png)

**Figure 1**: modification DropDownList d’inclut Interface et des cases d’option du GridView ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-cs/_static/image3.png))


## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>Étape 1 : Création approprié`UpdateProduct`de surcharge

Dans ce didacticiel, nous allons créer un GridView modifiable qui autorise la modification de nom d’un produit, catégorie, fournisseur et supprimées d’état. Par conséquent, nous avons besoin un `UpdateProduct` surcharge qui accepte cinq paramètres d’entrée les valeurs de ces quatre produit ainsi que les `ProductID`. Comme dans notre surcharges précédentes, ce qui une permettra :

1. Récupérer les informations de produit de la base de données spécifié `ProductID`,
2. Mise à jour le `ProductName`, `CategoryID`, `SupplierID`, et `Discontinued` champs, et
3. Envoyer la demande de mise à jour à la couche DAL par le biais du TableAdapter `Update()` (méthode).

Par souci de concision, pour cette surcharge particulière, j’ai omis la vérification de la règle métier qui garantit un produit qui est marqué comme supprimée n’est pas le seul produit offert par son fournisseur. Vous pouvez ajouter si vous préférez, ou, dans l’idéal, refactorisez la logique à une méthode distincte.

Le code suivant montre la nouvelle `UpdateProduct` surcharge dans le `ProductsBLL` classe :


[!code-csharp[Main](customizing-the-data-modification-interface-cs/samples/sample1.cs)]

## <a name="step-2-crafting-the-editable-gridview"></a>Étape 2 : Élaboration GridView modifiable

Avec le `UpdateProduct` surcharge ajoutée, nous sommes prêts à créer notre GridView modifiable. Ouvrez le `CustomizedUI.aspx` page dans le `EditInsertDelete` dossier et ajouter un contrôle GridView au concepteur. Ensuite, créez un nouveau ObjectDataSource à partir de la balise active le contrôle GridView. Configurer ObjectDataSource pour récupérer des informations de produit via le `ProductBLL` la classe `GetProducts()` (méthode) et mettre à jour les données de produit à l’aide la `UpdateProduct` surcharge que nous venons de créer. Dans les onglets INSERT et DELETE, sélectionnez (aucun) dans les listes déroulantes.


[![Configurer l’ObjectDataSource pour utiliser la surcharge UpdateProduct venez de créer](customizing-the-data-modification-interface-cs/_static/image5.png)](customizing-the-data-modification-interface-cs/_static/image4.png)

**Figure 2**: configurer ObjectDataSource à utiliser le `UpdateProduct` surcharger venez de créer ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-cs/_static/image6.png))


Comme nous l’avons vu dans les didacticiels de modification de données, la syntaxe déclarative pour ObjectDataSource créé par Visual Studio assigne le `OldValuesParameterFormatString` propriété `original_{0}`. Ceci, bien sûr, ne fonctionne pas avec notre couche de logique métier dans la mesure où nos méthodes n’attendent l’original `ProductID` valeur à passer dans. Par conséquent, comme nous l’avons fait dans les didacticiels précédents, prenez un moment pour supprimer cette attribution de propriété à partir de la syntaxe déclarative ou, au lieu de cela, la valeur est la valeur de cette propriété `{0}`.

Après cette modification, le balisage déclaratif de l’ObjectDataSource doit se présenter comme suit :


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample2.aspx)]

Notez que le `OldValuesParameterFormatString` propriété a été supprimée et qu’il existe un `Parameter` dans le `UpdateParameters` collection pour chacun des paramètres d’entrée attendus par notre `UpdateProduct` de surcharge.

Pendant la configuration de l’ObjectDataSource pour mettre à jour uniquement un sous-ensemble des valeurs de produit, le contrôle GridView affiche actuellement *tous les* des champs de produit. Prenez un moment pour modifier le contrôle GridView afin que :

- Elle inclut uniquement le `ProductName`, `SupplierName`, `CategoryName` BoundFields et `Discontinued` CheckBoxField
- Le `CategoryName` et `SupplierName` champs apparaissent avant (à gauche de) la `Discontinued` CheckBoxField
- Le `CategoryName` et `SupplierName` 'BoundFields `HeaderText` propriété est définie sur « Catégorie » et « Fournisseur », respectivement
- Prise en charge de l’édition est activée (case à cocher Activer la modification dans la balise active le contrôle GridView)

Après ces modifications, le concepteur doit ressembler à la Figure 3, avec la syntaxe déclarative du contrôle GridView indiquée ci-dessous.


[![Supprimer les champs inutiles de GridView](customizing-the-data-modification-interface-cs/_static/image8.png)](customizing-the-data-modification-interface-cs/_static/image7.png)

**Figure 3**: supprimer les champs inutiles de la GridView ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-cs/_static/image9.png))


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample3.aspx)]

À ce stade le comportement en lecture seule du contrôle GridView est terminée. Lorsque vous affichez les données, chaque produit est restitué sous la forme d’une ligne dans le contrôle GridView, montrant le nom du produit, catégorie, fournisseur et supprimées d’état.


[![Le contrôle GridView en lecture seule Interface est terminée.](customizing-the-data-modification-interface-cs/_static/image11.png)](customizing-the-data-modification-interface-cs/_static/image10.png)

**Figure 4**: le contrôle GridView en lecture seule Interface est terminée ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-cs/_static/image12.png))


> [!NOTE]
> Comme indiqué dans [didacticiels une vue d’ensemble d’insertion, mise à jour et suppression des données](an-overview-of-inserting-updating-and-deleting-data-cs.md), il est extrêmement important que le contrôle GridView état d’affichage s être activée (le comportement par défaut). Si vous définissez le s GridView `EnableViewState` propriété `false`, vous courez le risque de permettre aux utilisateurs simultanés involontairement suppression ou modification d’enregistrements. Consultez [Avertissement : problème d’accès concurrentiel avec ASP.NET 2.0 GridViews/DetailsView/FormViews cette modification de la prise en charge et/ou la suppression et la dont l’état d’affichage est désactivé](http://scottonwriting.net/sowblog/posts/10054.aspx) pour plus d’informations.


## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>Étape 3 : Utiliser un DropDownList pour la catégorie et la modification des Interfaces de fournisseur

N’oubliez pas que le `ProductsRow` objet contient `CategoryID`, `CategoryName`, `SupplierID`, et `SupplierName` propriétés qui fournissent les valeurs d’ID de clé étrangère réelles dans le `Products` de base de données de table et le correspondantes `Name` les valeurs dans le `Categories` et `Suppliers` tables. Le `ProductRow`de `CategoryID` et `SupplierID` peuvent tous deux être lues et écrites à, tandis que le `CategoryName` et `SupplierName` propriétés sont marquées en lecture seule.

En raison de l’état en lecture seule de la `CategoryName` et `SupplierName` propriétés, le correspondantes BoundFields ont eu leur `ReadOnly` propriété définie sur `true`, empêchant la modification lorsqu’une ligne est modifiée ces valeurs. Pendant que nous pouvons définir le `ReadOnly` propriété `false`, rendu le `CategoryName` et `SupplierName` BoundFields comme des zones de texte lors de l’édition, une telle approche entraîne une exception lorsque l’utilisateur tente de mettre à jour le produit dans la mesure où il existe aucune `UpdateProduct` surcharge dans `CategoryName` et `SupplierName` entrées. En fait, nous ne souhaitons pas créer une telle surcharge pour deux raisons :

- Le `Products` table ne contient pas `SupplierName` ou `CategoryName` champs, mais `SupplierID` et `CategoryID`. Par conséquent, nous voulons notre méthode à passer ces valeurs d’ID particuliers, pas les valeurs des tables de leur choix.
- Obliger l’utilisateur à taper le nom du fournisseur ou de la catégorie est loin d’être idéal, car il nécessite l’utilisateur connaisse les catégories disponibles et les fournisseurs et leurs orthographes correctes.

Les champs de catégorie et le fournisseur doivent afficher la catégorie et les noms de fournisseurs en mode en lecture seule (comme il le fait maintenant) et une liste déroulante d’options applicables lorsqu’en cours de modification. À l’aide d’une liste déroulante, l’utilisateur final peut voir rapidement quelles catégories et les fournisseurs sont disponibles pour le choix entre et peuvent plus facilement effectuer sa sélection.

Pour fournir ce comportement, nous devons convertir les `SupplierName` et `CategoryName` BoundFields dans TemplateField dont `ItemTemplate` émet le `SupplierName` et `CategoryName` valeurs et dont la propriété `EditItemTemplate` utilise un contrôle DropDownList pour la liste le catégories disponibles et les fournisseurs.

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>Ajout de la`Categories`et`Suppliers`DropDownList

Commencez par convertir la `SupplierName` et `CategoryName` BoundFields dans TemplateField par : en cliquant sur le lien Modifier les colonnes à partir de la balise active le contrôle GridView ; en sélectionnant le BoundField dans la liste en bas à gauche ; et en cliquant sur le « convertir ce champ en un Lien de TemplateField ». Le processus de conversion créera un TemplateField avec à la fois un `ItemTemplate` et un `EditItemTemplate`, comme illustré dans la syntaxe déclarative ci-dessous :


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample4.aspx)]

Dans la mesure où le BoundField a été marquée en lecture seule, à la fois le `ItemTemplate` et `EditItemTemplate` contiennent un Web Label contrôle dont `Text` propriété est liée au champ de données applicable (`CategoryName`, dans la syntaxe ci-dessus). Nous devons modifier la `EditItemTemplate`, en remplaçant le contrôle Web Label avec un contrôle DropDownList.

Comme nous l’avons vu dans les didacticiels précédents, le modèle peut être modifié via le concepteur ou directement à partir de la syntaxe déclarative. Pour le modifier via le concepteur, cliquez sur le lien Modifier les modèles à partir de la balise active le contrôle GridView et choisir de travailler avec le champ de catégorie `EditItemTemplate`. Supprimer le contrôle Web Label et remplacez-le par un contrôle DropDownList, en définissant la propriété d’ID de l’objet DropDownList sur `Categories`.


[![Supprimer la zone de texte et ajouter un contrôle DropDownList à EditItemTemplate](customizing-the-data-modification-interface-cs/_static/image14.png)](customizing-the-data-modification-interface-cs/_static/image13.png)

**Figure 5**: supprimer la zone de texte et ajouter un contrôle DropDownList pour la `EditItemTemplate` ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-cs/_static/image15.png))


Nous devons ensuite remplir la liste DropDownList avec les catégories disponibles. Cliquez sur le lien de choisir la Source de données à partir de la balise active de l’objet DropDownList et choisir de créer un nouveau ObjectDataSource nommé `CategoriesDataSource`.


[![Créer un contrôle ObjectDataSource nommé CategoriesDataSource](customizing-the-data-modification-interface-cs/_static/image17.png)](customizing-the-data-modification-interface-cs/_static/image16.png)

**Figure 6**: créer un nouveau contrôle ObjectDataSource nommé `CategoriesDataSource` ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-cs/_static/image18.png))


Pour que cette ObjectDataSource retourner toutes les catégories, liez-le à la `CategoriesBLL` la classe `GetCategories()` (méthode).


[![Lier l’ObjectDataSource à GetCategories() (méthode de la CategoriesBLL)](customizing-the-data-modification-interface-cs/_static/image20.png)](customizing-the-data-modification-interface-cs/_static/image19.png)

**Figure 7**: lier ObjectDataSource pour le `CategoriesBLL`de `GetCategories()` (méthode) ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-cs/_static/image21.png))


Enfin, configurez les paramètres de l’objet DropDownList telles que la `CategoryName` champ est affiché dans chaque DropDownList `ListItem` avec la `CategoryID` utilisée comme valeur de champ.


[![Avoir le champ CategoryName affiché et la CategoryID utilisée comme valeur](customizing-the-data-modification-interface-cs/_static/image23.png)](customizing-the-data-modification-interface-cs/_static/image22.png)

**Figure 8**: ont le `CategoryName` champ affiché et la `CategoryID` utilisée comme valeur ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-cs/_static/image24.png))


Une fois ces modifications le balisage déclaratif pour la `EditItemTemplate` dans le `CategoryName` TemplateField inclura un DropDownList et ObjectDataSource :


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> L’objet DropDownList dans le `EditItemTemplate` doit avoir son état d’affichage est activé. Nous allons bientôt ajouter la syntaxe de liaison de données à la syntaxe déclarative de la liste DropDownList et commandes de la liaison de données telles que `Eval()` et `Bind()` peuvent apparaître uniquement dans les contrôles dont l’état d’affichage est activé.


Répétez ces étapes pour ajouter un contrôle DropDownList nommé `Suppliers` à la `SupplierName` de TemplateField `EditItemTemplate`. Ce qui impliquera l’ajout d’un contrôle DropDownList pour la `EditItemTemplate` et la création d’un autre ObjectDataSource. Le `Suppliers` ObjectDataSource de DropDownList, toutefois, doit être configurée pour appeler le `SuppliersBLL` la classe `GetSuppliers()` (méthode). En outre, configurez le `Suppliers` DropDownList pour afficher le `CompanyName` champ et utilisez le `SupplierID` champ comme valeur pour son `ListItem` s.

Après avoir ajouté le DropDownList pour les deux `EditItemTemplate` s, chargez la page dans un navigateur et cliquez sur le bouton Modifier pour le produit de Cajun Seasoning du Chef Anton. Comme le montre la Figure 9, colonnes de catégorie et le fournisseur du produit sont rendus sous forme de listes déroulantes contenant les catégories disponibles et les fournisseurs sélectionnables. Toutefois, notez que le *premier* éléments dans les deux listes déroulantes sont sélectionnées par défaut (boissons pour la catégorie) et liquides exotiques en tant que fournisseur, bien que Cajun Seasoning de Chef Anton est un Condiment fournie par la Nouvelle-Orléans Cajun Divertissement.


[![Le premier élément dans la liste déroulante répertorie est sélectionné par défaut](customizing-the-data-modification-interface-cs/_static/image26.png)](customizing-the-data-modification-interface-cs/_static/image25.png)

**Figure 9**: le premier élément dans la liste déroulante répertorie est sélectionné par défaut ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-cs/_static/image27.png))


En outre, si vous cliquez sur mise à jour, vous découvrirez que le produit `CategoryID` et `SupplierID` ont la valeur `NULL`. Ces deux éléments non désirés comportements sont dû le DropDownList dans le `EditItemTemplate` s ne sont pas liés à des champs de données à partir des données de produit sous-jacent.

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>Le DropDownList pour la liaison la`CategoryID`et`SupplierID`des champs de données

Afin de disposer de catégorie et le fournisseur du produit modifié les listes déroulantes défini pour les valeurs appropriées et que ces valeurs renvoyées à la couche de logique métier `UpdateProduct` méthode lorsque vous cliquez sur la mise à jour, nous devons lier DropDownList `SelectedValue` propriétés à la `CategoryID` et `SupplierID` des champs de données à l’aide de la liaison de données bidirectionnelle. Pour y parvenir avec le `Categories` DropDownList, vous pouvez ajouter `SelectedValue='<%# Bind("CategoryID") %>'` directement à la syntaxe déclarative.

Ou bien, vous pouvez définir databindings de la liste DropDownList en modifiant le modèle par le biais du concepteur et en cliquant sur le lien Modifier les DataBindings à partir de la balise active de la liste DropDownList. Ensuite, indiquer que le `SelectedValue` propriété doit être liée à la `CategoryID` champ à l’aide de la liaison de données bidirectionnelle (voir Figure 10). Répétez le processus déclaratif ou concepteur pour lier le `SupplierID` de champ de données pour le `Suppliers` DropDownList.


[![Lier la CategoryID SelectedValue propriété de la liste DropDownList à l’aide de la liaison de données bidirectionnelle](customizing-the-data-modification-interface-cs/_static/image29.png)](customizing-the-data-modification-interface-cs/_static/image28.png)

**Figure 10**: lier le `CategoryID` le DropDownList `SelectedValue` propriété à l’aide de bidirectionnel de liaison de données ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-cs/_static/image30.png))


Une fois que les liaisons ont été appliqués à la `SelectedValue` des propriétés de la deux DropDownList, colonnes de catégorie et le fournisseur du produit modifié par défaut pour les valeurs actuelles du produit. Lorsque vous cliquez sur la mise à jour, le `CategoryID` et `SupplierID` recevront des valeurs de l’élément de liste déroulante sélectionnée pour le `UpdateProduct` (méthode). La figure 11 illustre le didacticiel après ont ajouté les instructions de liaison de données ; Notez la façon dont les éléments de liste déroulante sélectionnée pour Cajun Seasoning du Chef Anton sont correctement Condiment et Nouvelle-Orléans Cajun divertissement.


[![Catégorie actuelle et les valeurs du fournisseur du produit modifié sont sélectionnées par défaut](customizing-the-data-modification-interface-cs/_static/image32.png)](customizing-the-data-modification-interface-cs/_static/image31.png)

**Figure 11**: la modification du produit catégorie actuelle et valeurs du fournisseur sont sélectionnées par défaut ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-cs/_static/image33.png))


## <a name="handlingnullvalues"></a>Gestion des`NULL`valeurs

Le `CategoryID` et `SupplierID` colonnes dans le `Products` table peut être `NULL`, encore le DropDownList dans le `EditItemTemplate` s n’incluez pas un élément de liste pour représenter un `NULL` valeur. Cela a deux conséquences :

- Utilisateur ne peut pas utiliser notre interface pour modifier la catégorie d’un produit ou le fournisseur à partir d’une non -`NULL` valeur à un `NULL` un
- Si un produit possède un `NULL` `CategoryID` ou `SupplierID`, en cliquant sur le bouton Modifier provoquera une exception. Il s’agit, car le `NULL` valeur retournée par `CategoryID` (ou `SupplierID`) dans le `Bind()` instruction n’est pas mappé à une valeur dans la liste DropDownList (DropDownList lève une exception lors de son `SelectedValue` propriété est définie sur une valeur *pas* dans sa collection d’éléments de liste).

Pour prendre en charge `NULL` `CategoryID` et `SupplierID` valeurs, nous devons ajouter un autre `ListItem` à chaque DropDownList pour représenter le `NULL` valeur. Dans le [filtrage de maître/détail avec un DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) didacticiel, nous avons vu comment ajouter un autre `ListItem` à un DropDownList lié aux données, qui impliquait définissant la DropDownList `AppendDataBoundItems` propriété `true` et Ajout manuel supplémentaire `ListItem`. Dans ce didacticiel précédent, toutefois, nous avons ajouté un `ListItem` avec un `Value` de `-1`. Toutefois, la logique de liaison de données dans ASP.NET, convertit automatiquement une chaîne vide à un `NULL` valeur et inversement a. Par conséquent, pour ce didacticiel nous voulons le `ListItem`de `Value` soit une chaîne vide.

Commencez par définir les deux DropDownLists' `AppendDataBoundItems` propriété `true`. Ensuite, ajoutez le `NULL` `ListItem` en ajoutant le code suivant `<asp:ListItem>` élément à chaque DropDownList afin que le balisage déclaratif ressemble, tels que :


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample6.aspx)]

J’ai choisi d’utiliser « (None) » en tant que la valeur de texte pour ce `ListItem`, mais vous pouvez modifier pour qu’il soit également une chaîne vide si vous le souhaitez.

> [!NOTE]
> Comme nous l’avons vu dans la *filtrage de maître/détail avec un DropDownList* didacticiel, `ListItem` s peut être ajouté à un contrôle DropDownList via le concepteur en cliquant sur la DropDownList `Items` propriété dans la fenêtre Propriétés (qui Affiche le `ListItem` éditeur de collections). Toutefois, veillez à ajouter le `NULL` `ListItem` pour ce didacticiel via la syntaxe déclarative. Si vous utilisez le `ListItem` éditeur de collections, la syntaxe déclarative générée sera omettre la `Value` paramètre complètement lorsque affecté à une chaîne vide, en créant un balisage déclaratif telles que : `<asp:ListItem>(None)</asp:ListItem>`. Bien que cela peut vous paraître inoffensif, la valeur manquante provoque la liste DropDownList à utiliser le `Text` valeur de propriété à la place. Qui signifie que si cela `NULL` `ListItem` est sélectionnée, la valeur « (None) » sera tentée à assigner à la `CategoryID`, ce qui entraînera une exception. En définissant explicitement `Value=""`, un `NULL` valeur sera affectée à `CategoryID` lorsque le `NULL` `ListItem` est sélectionné.


Répétez ces étapes pour l’objet DropDownList de fournisseurs.

Avec cette supplémentaires `ListItem`, l’interface de modification peut affecter maintenant `NULL` les valeurs d’un produit `CategoryID` et `SupplierID` champs, comme indiqué dans la Figure 12.


[![Choisissez (aucun) pour affecter une valeur NULL pour la catégorie ou le fournisseur du produit](customizing-the-data-modification-interface-cs/_static/image35.png)](customizing-the-data-modification-interface-cs/_static/image34.png)

**Figure 12**: (None) choisi d’affecter un `NULL` valeur pour la catégorie ou le fournisseur du produit ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-cs/_static/image36.png))


## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>Étape 4 : À l’aide de cases d’option pour l’état abandonné

Actuellement les produits `Discontinued` champ de données est exprimé à l’aide d’un CheckBoxField, qui restitue une case à cocher désactivée pour les lignes en lecture seule et une case à cocher est activée pour la ligne en cours de modification. Bien que cette interface utilisateur s’avère souvent appropriée, nous pouvons le personnaliser si nécessaire à l’aide d’un TemplateField. Pour ce didacticiel, nous allons modifier le CheckBoxField en TemplateField qui utilise un contrôle RadioButtonList avec deux options « Actives » et « Supprimées » à partir de laquelle l’utilisateur peut spécifier le produit `Discontinued` valeur.

Commencez par convertir la `Discontinued` CheckBoxField en TemplateField, ce qui créera un TemplateField avec un `ItemTemplate` et `EditItemTemplate`. Les deux modèles incluent une case à cocher avec son `Checked` propriété liée à la `Discontinued` de champ de données, la seule différence entre les deux qui sont le `ItemTemplate`de la case à cocher `Enabled` propriété est définie sur `false`.

Remplacez la case à cocher à la fois dans le `ItemTemplate` et `EditItemTemplate` avec un contrôle RadioButtonList, définissant les deux RadioButtonList `ID` propriétés à `DiscontinuedChoice`. Ensuite, indiquer le RadioButtonList doit contenant chacune deux cases d’option, un seul étiqueté « actif » avec la valeur « False » et bouton intitulé « Supprimées » avec la valeur « True ». Pour y parvenir vous pouvez soit entrer le `<asp:ListItem>` éléments directement par le biais de la syntaxe déclarative ou utilisez le `ListItem` éditeur de collections à partir du concepteur. La figure 13 montre la `ListItem` éditeur de Collection une fois que les deux cases d’option options du bouton ont été spécifiés.


[![Ajouter](customizing-the-data-modification-interface-cs/_static/image38.png)](customizing-the-data-modification-interface-cs/_static/image37.png)

**Figure 13**: ajouter « Actif » et « Discontinued » Options à RadioButtonList ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-cs/_static/image39.png))


Depuis le RadioButtonList dans le `ItemTemplate` ne doit pas être modifiée, définir son `Enabled` propriété `false`, laissant le `Enabled` propriété `true` (la valeur par défaut) pour RadioButtonList dans le `EditItemTemplate`. Cela rendra les cases d’option dans la ligne non modifiée en lecture seule, mais permet à l’utilisateur de modifier les valeurs de case d’option pour la ligne modifiée.

Nous devons encore affecter les contrôles RadioButtonList `SelectedValue` afin que le bouton radio approprié est sélectionné en fonction du produit `Discontinued` champ de données. Comme avec DropDownList examinée plus haut dans ce didacticiel, cette syntaxe de liaison de données peut être ajoutée directement dans le balisage déclaratif ou via le lien Modifier les DataBindings dans les balises actives des RadioButtonList.

Après avoir ajouté les deux RadioButtonList et de leur configuration, le `Discontinued` balisage déclaratif de TemplateField doit ressembler à :


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample7.aspx)]

Avec ces modifications, la `Discontinued` colonne a été transformée à partir d’une liste de cases à cocher pour une liste de paires de bouton radio (voir Figure 14). Lorsque vous modifiez un produit, le bouton radio approprié est sélectionné et l’état du produit supprimées peut être mis à jour en cochant la case autres en cliquant sur la mise à jour.


[![Les cases à cocher abandonnés ont été remplacés par des paires de bouton Radio](customizing-the-data-modification-interface-cs/_static/image41.png)](customizing-the-data-modification-interface-cs/_static/image40.png)

**Figure 14**: l’abandonné cases à cocher ont été remplacés par des paires de bouton Radio ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-cs/_static/image42.png))


> [!NOTE]
> Dans la mesure où le `Discontinued` colonne dans le `Products` base de données ne peut pas avoir `NULL` valeurs, nous n’avez pas besoin à vous soucier de capture `NULL` informations dans l’interface. If, toutefois, `Discontinued` colonne peut contenir `NULL` valeurs que nous voulons ajouter une troisième case d’option à la liste avec ses `Value` définie sur une chaîne vide (`Value=""`), tout comme avec la catégorie et le fournisseur DropDownList.


## <a name="summary"></a>Récapitulatif

Bien que le BoundField et CheckBoxField restituent automatiquement des interfaces en lecture seule, éditions et insertion, ils n’ont pas la possibilité de personnalisation. Souvent, cependant, nous allons devoir personnaliser la modification ou l’insertion d’interface, en ajoutant éventuellement les contrôles de validation (comme nous l’avons vu dans le didacticiel précédent) ou en personnalisant l’interface utilisateur de collecte de données (comme nous l’avons vu dans ce didacticiel). Personnalisation de l’interface avec un TemplateField peut être résumée dans les étapes suivantes :

1. Ajouter TemplateField ou convertir un BoundField existant ou du CheckBoxField en TemplateField
2. Augmenter l’interface en fonction des besoins
3. Lier les champs de données approprié pour les contrôles Web qui vient d’être ajoutés à l’aide de la liaison de données bidirectionnelle

Outre les contrôles Web ASP.NET intégrés, vous pouvez également personnaliser les modèles de TemplateField avec les contrôles serveur personnalisés, compilé et les contrôles utilisateur.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)
> [Suivant](implementing-optimistic-concurrency-cs.md)
