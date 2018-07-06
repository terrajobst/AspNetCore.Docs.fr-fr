---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-cs
title: Insertion d’un nouvel enregistrement à partir du pied de page du GridView (c#) | Microsoft Docs
author: rick-anderson
description: Tandis que le contrôle GridView ne fournit pas de prise en charge intégrée pour l’insertion d’un nouvel enregistrement de données, ce didacticiel montre comment augmenter le contrôle GridView à inclure un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 49545652-98af-46ba-9dbc-9ab529805d9b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: 3f402d98a12c7da10d97744bbbd33e204275e93c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381849"
---
<a name="inserting-a-new-record-from-the-gridviews-footer-c"></a>Insertion d’un nouvel enregistrement à partir du pied de page du GridView (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_CS.exe) ou [télécharger le PDF](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/datatutorial53cs1.pdf)

> Tandis que le contrôle GridView ne fournit pas de prise en charge intégrée pour l’insertion d’un nouvel enregistrement de données, ce didacticiel montre comment augmenter le contrôle GridView pour inclure une interface d’insertion.


## <a name="introduction"></a>Introduction

Comme indiqué dans le [une vue d’ensemble d’insertion, mise à jour et suppression des données](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) didacticiel, les contrôles GridView, DetailsView et FormView Web incluent des fonctionnalités de modification de données intégrés. Lorsqu’il est utilisé avec les contrôles de source de données déclarative, ces trois contrôles Web rapidement et facilement configurables pour modifier des données - et dans les scénarios sans avoir à écrire une seule ligne de code. Malheureusement, uniquement les contrôles DetailsView et FormView fournissent intégré l’insertion, la modification et la suppression de fonctionnalités. Le contrôle GridView offre uniquement la modification et suppression de prise en charge. Toutefois, avec un peu graisse de coude, nous pouvons augmenter le contrôle GridView pour inclure une interface d’insertion.

Lors de l’ajout de fonctionnalités d’insertion pour le contrôle GridView, nous sommes doit décider sur comment les nouveaux enregistrements sera ajoutée, création de l’interface d’insertion et écrire le code pour insérer le nouvel enregistrement. Dans ce didacticiel, que nous examinerons d’ajout de l’interface d’insertion pour le pied de page s GridView de lignes (voir Figure 1). La cellule de pied de page pour chaque colonne inclut les données appropriées collection élément d’interface utilisateur (une zone de texte pour le nom du produit s, un DropDownList pour le fournisseur et ainsi de suite). Nous avons également besoin d’une colonne pour un ajout bouton qui, d’un clic, entraîne une publication (postback) et insérer un nouvel enregistrement dans le `Products` table en utilisant les valeurs fournies dans la ligne de pied de page.


[![La ligne de pied de page fournit une Interface pour l’ajout de nouveaux produits](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image1.png)

**Figure 1**: la ligne de pied de page fournit une Interface pour l’ajout de nouveaux produits ([cliquez pour afficher l’image en taille réelle](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image2.png))


## <a name="step-1-displaying-product-information-in-a-gridview"></a>Étape 1 : Affichage des informations sur les produits dans un GridView

Avant de nous nous-mêmes concernent à la création de l’interface d’insertion dans le pied de page s GridView, permettent de concentrer tout d’abord de s sur l’ajout d’un GridView à la page qui répertorie les produits dans la base de données. Commencez par ouvrir le `InsertThroughFooter.aspx` page dans le `EnhancedGridView` dossier et faites glisser un GridView à partir de la boîte à outils vers le concepteur, en définissant le s GridView `ID` propriété `Products`. Ensuite, utilisez la balise active de s GridView pour lier à un nouveau ObjectDataSource nommé `ProductsDataSource`.


[![Créer un nouveau ObjectDataSource nommé ProductsDataSource](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image3.png)

**Figure 2**: créer une nouvelle nommée de ObjectDataSource `ProductsDataSource` ([cliquez pour afficher l’image en taille réelle](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image4.png))


Configurer l’ObjectDataSource à utiliser le `ProductsBLL` classe s `GetProducts()` méthode pour récupérer des informations de produit. Pour ce didacticiel, vous permettre s strictement sur l’ajout de fonctionnalités de l’insertion et ne vous inquiétez pas sur la modification et suppression. Par conséquent, assurez-vous que la liste déroulante dans l’onglet Insertion a la valeur `AddProduct()` et que les listes déroulantes dans les onglets UPDATE et DELETE sont définis sur (aucun).


[![Mapper la méthode AddProduct à la méthode de Insert() s ObjectDataSource](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image5.png)

**Figure 3**: carte le `AddProduct` méthode au s ObjectDataSource `Insert()` (méthode) ([cliquez pour afficher l’image en taille réelle](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image6.png))


[![Définir les listes déroulantes de mise à jour et supprimer les tabulations à (None)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image7.png)

**Figure 4**: la valeur est la mise à jour et supprimer des onglets liste déroulante répertorie (aucun) ([cliquez pour afficher l’image en taille réelle](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image8.png))


À l’issue de l’Assistant Configurer la Source de données de s ObjectDataSource, Visual Studio ajoute automatiquement les champs pour le contrôle GridView pour chacun des champs de données correspondante. Pour l’instant, laissez tous les champs ajoutés par Visual Studio. Plus loin dans ce didacticiel, nous allons revenir et supprimer certains des champs dont les valeurs n t doivent être spécifiés lors de l’ajout d’un nouvel enregistrement.

Dans la mesure où il existe près de 80 produits dans la base de données, un utilisateur sera peut-être faire défiler jusqu’au bas de la page web afin d’ajouter un nouvel enregistrement. Par conséquent, vous permettent à s pour activer la pagination rendre l’interface insertion plus visible et accessible. Pour activer la pagination, simplement cocher la case à cocher Activer la pagination de la balise active de s GridView.

À ce stade, le balisage déclaratif s GridView et ObjectDataSource doit ressembler à ce qui suit :


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample1.aspx)]


[![Tous les champs de données de produit sont affichés dans un GridView de réserve paginée](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image9.png)

**Figure 5**: tous les champs de données de produit sont affichés dans un GridView paginée ([cliquez pour afficher l’image en taille réelle](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image10.png))


## <a name="step-2-adding-a-footer-row"></a>Étape 2 : Ajout d’une ligne de pied de page

Ainsi que son en-tête et les lignes de données, le contrôle GridView inclut une ligne de pied de page. Les lignes d’en-tête et pied de page sont affichées en fonction des valeurs de la s GridView [ `ShowHeader` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showheader.aspx) et [ `ShowFooter` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showfooter.aspx) propriétés. Pour afficher la ligne de pied de page, il suffit de définir la `ShowFooter` propriété `true`. Comme le montre la Figure 6, définissant le `ShowFooter` propriété `true` ajoute une ligne de pied de page à la grille.


[![Pour afficher la ligne de pied de page, la valeur ShowFooter True](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image11.png)

**Figure 6**: pour afficher la ligne de pied de page, définissez `ShowFooter` à `True` ([cliquez pour afficher l’image en taille réelle](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image12.png))


Notez que la ligne de pied de page a une couleur d’arrière-plan rouge foncé. Cela est dû le DataWebControls thème, nous avons créé et appliqué à toutes les pages dans le [affichant les données avec ObjectDataSource the](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) didacticiel. Plus précisément, le `GridView.skin` fichier configure le `FooterStyle` propriété telle qu’elle utilise le `FooterStyle` classe CSS. Le `FooterStyle` classe est définie dans `Styles.css` comme suit :


[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample2.css)]

> [!NOTE]
> Nous ve exploré à l’aide de la ligne de pied de page du GridView s dans les didacticiels précédents. Si nécessaire, vous référer à la [affichant des informations de résumé dans le pied de page du GridView](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) didacticiel pour un petit rappel.


Après avoir défini le `ShowFooter` propriété `true`, prenez un moment pour afficher la sortie dans un navigateur. Actuellement, t n de ligne de pied de page contiennent tout texte ou les contrôles Web. À l’étape 3, nous allons modifier le pied de page pour chaque champ GridView pour qu’elle inclue l’interface d’insertion appropriée.


[![La ligne de pied de page vide est affichée au-dessus de la pagination contrôles d’Interface](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image13.png)

**Figure 7**: la ligne vide est affichée au-dessus de la pagination contrôles d’Interface ([cliquez pour afficher l’image en taille réelle](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image14.png))


## <a name="step-3-customizing-the-footer-row"></a>Étape 3 : Personnalisation de la ligne de pied de page

Dans le [à l’aide de TemplateFields dans le contrôle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) didacticiel, nous avons vu comment considérablement personnaliser l’affichage d’une colonne GridView utilisation de TemplateField (par opposition à BoundFields ou CheckBoxFields) ; dans [ Personnalisation de l’Interface de Modification de données](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) nous avons étudié l’utilisation de TemplateField pour personnaliser l’interface de modification dans un GridView. Rappel que TemplateField est composé d’un nombre de modèles qui définit la combinaison de balisage, les contrôles Web et la syntaxe de liaison de données utilisé pour certains types de lignes. Le `ItemTemplate`, par exemple, spécifie le modèle utilisé pour les lignes en lecture seule, tandis que le `EditItemTemplate` définit le modèle pour la ligne modifiable.

Avec le `ItemTemplate` et `EditItemTemplate`, le TemplateField contenu inclut également un `FooterTemplate` qui spécifie le contenu de la ligne de pied de page. Par conséquent, nous pouvons ajouter les contrôles Web nécessaires pour chaque champ s insertion d’interface dans le `FooterTemplate`. Pour démarrer, de convertir tous les champs dans le contrôle GridView TemplateField. Cela est possible cliquant sur le lien Modifier les colonnes dans le GridView s balise active, en sélectionnant chaque champ dans l’angle inférieur gauche et en cliquant sur la convertir ce champ en TemplateField lien.


![Convertir chaque champ en TemplateField](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image8.gif)

**Figure 8**: convertir chaque champ en TemplateField


En sélectionnant le convertir ce champ en TemplateField transforme le type de champ actuel dans un TemplateField équivalente. Par exemple, chaque BoundField est remplacé par un TemplateField avec un `ItemTemplate` qui contient une étiquette qui affiche le champ de données correspondant et une `EditItemTemplate` qui affiche le champ de données dans une zone de texte. Le `ProductName` BoundField a été converti dans le balisage de TemplateField suivant :


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample3.aspx)]

De même, le `Discontinued` CheckBoxField a été converti en TemplateField dont `ItemTemplate` et `EditItemTemplate` contiennent un contrôle de case à cocher Web (avec le `ItemTemplate` s case à cocher désactivée). En lecture seule `ProductID` BoundField a été converti en TemplateField avec un contrôle étiquette à la fois dans le `ItemTemplate` et `EditItemTemplate`. En bref, champ en TemplateField conversion d’un GridView existant est un moyen simple et rapide pour basculer vers le TemplateField contenu plus personnalisable sans perdre la fonctionnalité de champ s existante.

Depuis le contrôle GridView nous re fonctionne avec n t prise en charge la modification, vous pouvez supprimer le `EditItemTemplate` à partir de chaque TemplateField, en laissant seulement le `ItemTemplate`. Après cette opération, votre balisage déclaratif de GridView s doit ressembler à ce qui suit :


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample4.aspx)]

Maintenant que chaque champ GridView a été converti en un TemplateField, nous pouvons saisir l’interface insertion appropriée dans chaque champ s `FooterTemplate`. Certains champs n’aura pas d’une interface d’insertion (`ProductID`, par exemple) ; d’autres peuvent varier dans les contrôles Web permet de collecter les nouvelles informations de produit s.

Pour créer l’interface de modification, cliquez sur le lien Modifier les modèles à partir de la balise active de s GridView. Ensuite, dans la liste déroulante, sélectionnez le champ approprié s `FooterTemplate` et faites glisser le contrôle approprié à partir de la boîte à outils vers le concepteur.


[![Ajouter l’Interface insertion appropriée à chaque FooterTemplate s de champ](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image15.png)

**Figure 9**: ajouter l’Interface appropriée d’insertion pour chaque champ s `FooterTemplate` ([cliquez pour afficher l’image en taille réelle](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image16.png))


La liste à puces suivante énumère les champs de GridView, en spécifiant l’interface insertion à ajouter :

- `ProductID` Aucun.
- `ProductName` Ajoutez une zone de texte et définissez son `ID` à `NewProductName`. Ajoutez un contrôle RequiredFieldValidator pour vous assurer que l’utilisateur entre une valeur pour le nouveau nom de produit s.
- `SupplierID` Aucun.
- `CategoryID` Aucun.
- `QuantityPerUnit` Ajouter une zone de texte, en définissant son `ID` à `NewQuantityPerUnit`.
- `UnitPrice` Ajouter une zone de texte nommé `NewUnitPrice` et un CompareValidator qui garantit que la valeur entrée est une valeur monétaire supérieure ou égale à zéro.
- `UnitsInStock` utiliser une zone de texte dont `ID` est défini sur `NewUnitsInStock`. Inclure un CompareValidator qui garantit que la valeur entrée est une valeur entière supérieure ou égale à zéro.
- `UnitsOnOrder` utiliser une zone de texte dont `ID` est défini sur `NewUnitsOnOrder`. Inclure un CompareValidator qui garantit que la valeur entrée est une valeur entière supérieure ou égale à zéro.
- `ReorderLevel` utiliser une zone de texte dont `ID` est défini sur `NewReorderLevel`. Inclure un CompareValidator qui garantit que la valeur entrée est une valeur entière supérieure ou égale à zéro.
- `Discontinued` Ajouter une case à cocher, en définissant son `ID` à `NewDiscontinued`.
- `CategoryName` Ajoutez un contrôle DropDownList et définissez son `ID` à `NewCategoryID`. Lier à un nouveau ObjectDataSource nommé `CategoriesDataSource` et configurez-le pour utiliser le `CategoriesBLL` classe s `GetCategories()` (méthode). Avoir le s DropDownList `ListItem` affichage de s la `CategoryName` données champ, à l’aide de la `CategoryID` champ de données en tant que leurs valeurs.
- `SupplierName` Ajoutez un contrôle DropDownList et définissez son `ID` à `NewSupplierID`. Lier à un nouveau ObjectDataSource nommé `SuppliersDataSource` et configurez-le pour utiliser le `SuppliersBLL` classe s `GetSuppliers()` (méthode). Avoir le s DropDownList `ListItem` affichage de s la `CompanyName` données champ, à l’aide de la `SupplierID` champ de données en tant que leurs valeurs.

Pour chacun des contrôles de validation, effacer le `ForeColor` propriété afin que le `FooterStyle` couleur de premier plan blanc CSS classe s sera utilisé à la place de la valeur par défaut est rouge. Utiliser également le `ErrorMessage` propriété pour une description détaillée, la valeur, mais le `Text` propriété sur un astérisque. Pour éviter que le texte du contrôle s validation à l’origine de l’interface d’insertion à la ligne sur deux lignes, définissez la `FooterStyle` s `Wrap` propriété sur false pour chacun de la `FooterTemplate` qui utilisent un contrôle de validation. Enfin, ajoutez un contrôle ValidationSummary sous les contrôles GridView et ensemble son `ShowMessageBox` propriété `true` et son `ShowSummary` propriété `false`.

Lorsque vous ajoutez un nouveau produit, nous devons fournir le `CategoryID` et `SupplierID`. Ces informations sont capturées par le biais du DropDownList dans les cellules de pied de page pour le `CategoryName` et `SupplierName` champs. J’ai choisi d’utiliser ces champs, par opposition à la `CategoryID` et `SupplierID` TemplateField, car les lignes de l’utilisateur dans la grille s données est probablement plus intéressé par voir les noms de catégorie et le fournisseur plutôt que leurs valeurs d’identificateur. Dans la mesure où le `CategoryID` et `SupplierID` valeurs sont maintenant capturées dans le `CategoryName` et `SupplierName` interfaces insertion de champ s, nous pouvons supprimer le `CategoryID` et `SupplierID` TemplateFields dans le contrôle GridView.

De même, le `ProductID` n’est pas utilisé lors de l’ajout d’un nouveau produit, par conséquent, le `ProductID` TemplateField peut être également supprimé. Toutefois, laisser s laisser le `ProductID` champ dans la grille. Outre les zones de texte, DropDownList, les cases à cocher et les contrôles de validation qui composent l’interface d’insertion, nous aurons également besoin d’un ajout bouton qui, lorsque vous cliquez dessus, exécute la logique pour ajouter le nouveau produit à la base de données. À l’étape 4, nous allons inclure un bouton Add dans l’interface d’insertion dans le `ProductID` TemplateField s `FooterTemplate`.

N’hésitez pas à améliorer l’apparence des différents champs GridView. Par exemple, vous souhaiterez peut-être mettre en forme le `UnitPrice` Aligner à droite les valeurs sous forme de devise, la `UnitsInStock`, `UnitsOnOrder`, et `ReorderLevel` champs et mise à jour le `HeaderText` valeurs pour le TemplateField.

Après avoir créé la multitude de l’insertion des interfaces dans les `FooterTemplate` s, suppression de la `SupplierID`, et `CategoryID` TemplateField et améliorer l’esthétique de la grille par le biais de mise en forme et l’alignement le TemplateField, votre s GridView déclarative balisage doit ressembler à ce qui suit :


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample5.aspx)]

Lorsqu’ils sont affichés via un navigateur, la ligne de pied de page du GridView s inclut désormais la fin de l’interface insertion (voir Figure 10). À ce stade, l’insertion interface n’inclure un moyen de l’utilisateur indiquer que s she entré les données pour le nouveau produit et qu’il souhaite insérer un nouvel enregistrement dans la base de données. En outre, nous ve encore pour traiter la façon dont les données entrées dans le pied de page seront traduit dans un nouvel enregistrement dans le `Products` base de données. À l’étape 4, nous allons étudier comment inclure un bouton d’ajout à l’interface d’insertion et comment exécuter du code sur publication (postback) quand il s cliqué. Étape 5 montre comment insérer un nouvel enregistrement en utilisant les données dans le pied de page.


[![Le pied de page du GridView fournit une Interface pour ajouter un nouvel enregistrement](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image17.png)

**Figure 10**: le pied de page du GridView fournit une Interface pour ajouter un nouvel enregistrement ([cliquez pour afficher l’image en taille réelle](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image18.png))


## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>Étape 4 : Y compris un bouton Ajouter dans l’Interface d’insertion

Nous devons inclure un bouton Ajouter quelque part dans l’interface d’insertion dans la mesure où les s de ligne de pied de page insertion interface actuellement ne possède pas les moyens de l’utilisateur indiquer qu’ils ont terminé d’entrer les nouvelles informations de produit s. Cela peut être placée dans un des existant `FooterTemplate` s ou nous pouvons ajouter une nouvelle colonne à la grille à cet effet. Pour ce didacticiel, s permettent de placer le bouton Ajouter dans le `ProductID` TemplateField s `FooterTemplate`.

À partir du concepteur, cliquez sur le lien Modifier les modèles dans la balise active de s GridView, puis choisissez le `ProductID` champ s `FooterTemplate` dans la liste déroulante. Ajouter un contrôle bouton Web (ou un LinkButton ou ImageButton, si vous préférez) pour le paramètre de modèle, son ID `AddProduct`, ses `CommandName` INSERT et son `Text` propriété à ajouter comme indiqué dans la Figure 11.


[![Placez le bouton Ajouter dans FooterTemplate s ProductID TemplateField](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image19.png)

**Figure 11**: Place le bouton Ajouter dans le `ProductID` TemplateField s `FooterTemplate` ([cliquez pour afficher l’image en taille réelle](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image20.png))


Une fois que vous avez déjà inclus le bouton Ajouter, testez la page dans un navigateur. Notez que lorsque vous cliquez sur le bouton Ajouter avec des données non valides dans l’interface d’insertion, la publication (postback) est court-circuitée bref et le contrôle ValidationSummary indique les données non valides (voir Figure 12). Avec les données entrées appropriés, en cliquant sur le bouton Ajouter provoque une publication (postback). Aucun enregistrement n’est ajouté à la base de données, toutefois. Nous devrons écrire un peu de code pour effectuer l’insertion.


[![Le bouton Ajouter publication (postback) est court-circuitée courte s’il existe des données non valides dans l’Interface d’insertion](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image21.png)

**Figure 12**: le bouton Ajouter s publication (postback) est court-circuitée courte s’il existe des données non valides dans l’Interface d’insertion ([cliquez pour afficher l’image en taille réelle](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image22.png))


> [!NOTE]
> Les contrôles de validation dans l’interface d’insertion non affectés à un groupe de validation. Cela fonctionne bien tant que l’interface d’insertion est le jeu uniquement des contrôles de validation sur la page. Si, toutefois, il existe d’autres contrôles de validation sur la page (tels que les contrôles de validation dans l’interface de modification de grille s), les contrôles de validation de l’insertion de l’interface et ajouter le bouton s `ValidationGroup` propriétés doivent être attribuées à la même valeur de manière à associer ces contrôles à un groupe de validation particulier. Consultez [dissection les contrôles de Validation dans ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx) pour plus d’informations sur les contrôles de validation et les boutons sur une page de partitionnement des groupes de validation.


## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>Étape 5 : Insertion d’un nouvel enregistrement dans le`Products`Table

Lorsque vous utilisez les fonctionnalités d’édition intégrées du contrôle GridView, le contrôle GridView gère automatiquement tout le travail nécessaire pour effectuer la mise à jour. En particulier, lorsque le bouton de mise à jour, il copie les valeurs entrées à partir de l’interface de modification aux paramètres dans le s ObjectDataSource `UpdateParameters` collection et intervienne désactiver la mise à jour en appelant le s ObjectDataSource `Update()` (méthode). Dans la mesure où le contrôle GridView ne fournit pas ces fonctionnalités intégrées pour l’insertion, nous devons implémenter le code qui appelle les opérations de mappage ObjectDataSource `Insert()` (méthode) et copie les valeurs à partir de l’insertion de l’interface pour les opérations de mappage ObjectDataSource `InsertParameters` collection .

Cette logique d’insertion doit être exécutée une fois que le bouton Ajouter a été cliqué. Comme indiqué dans le [Ajout et réponse à des boutons dans une GridView](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs.md) (didacticiel), chaque fois qu’un bouton, LinkButton ou ImageButton dans une GridView est sélectionné, les opérations de mappage GridView `RowCommand` événement se déclenche lors de la publication. Cet événement se déclenche si le bouton, LinkButton ou ImageButton a été ajouté explicitement telles que le bouton Ajouter dans la ligne de pied de page ou si elle a été automatiquement ajoutée par le contrôle GridView (telles que le type LinkButton en haut de chaque colonne quand activer le tri est sélectionné, ou le Type LinkButton dans l’interface de pagination lorsque vous activez la pagination est activée).

Par conséquent, pour répondre à l’utilisateur clique sur le bouton Ajouter, nous devons créer un gestionnaire d’événements pour les opérations de mappage GridView `RowCommand` événement. Dans la mesure où cet événement se déclenche chaque fois que *n’importe quel* bouton, LinkButton ou ImageButton dans le contrôle GridView est activé, il s vital que nous progressons uniquement avec la logique d’insertion si le `CommandName` propriété passé dans les mappages de gestionnaire d’événements pour le `CommandName` valeur du bouton Ajouter (insertion). En outre, nous devons également de continuer seulement si les contrôles de validation signalent des données valides. Pour ce faire, créez un gestionnaire d’événements pour le `RowCommand` événement avec le code suivant :


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample6.cs)]

> [!NOTE]
> Vous vous demandez peut-être pourquoi le Gestionnaire d’événements dérange vérifiant la `Page.IsValid` propriété. Après tout, gagné / t de la publication (postback) être supprimés si des données non valides sont fournies dans l’interface insertion ? Cette hypothèse est correcte en tant que l’utilisateur n’a pas désactivé JavaScript ou a pris des mesures pour contourner la logique de validation côté client. En bref, un ne doit jamais dépendre strictement validation côté client ; une vérification côté serveur pour la validité doit toujours être effectuée avant de travailler avec les données.


À l’étape 1, nous avons créé le `ProductsDataSource` ObjectDataSource telles que son `Insert()` méthode est mappée à la `ProductsBLL` classe s `AddProduct` (méthode). Pour insérer le nouvel enregistrement dans le `Products` table, nous pouvons simplement appeler les opérations de mappage ObjectDataSource `Insert()` méthode :


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample7.cs)]

Maintenant que le `Insert()` méthode a été appelée, tout ce qui reste consiste à copier les valeurs à partir de l’interface d’insertion pour les paramètres passés à la `ProductsBLL` classe s `AddProduct` (méthode). Comme nous l’avons vu dans la [examinant les événements associés insertion, mise à jour et suppression](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) didacticiel, cela peut être accompli via le s ObjectDataSource `Inserting` événement. Dans le `Inserting` événement que nous devons référencer par programme les contrôles à partir de la `Products` pied de page s GridView de lignes et d’affecter leurs valeurs à le `e.InputParameters` collection. Si l’utilisateur n’indique pas une valeur telle que de laisser le `ReorderLevel` vide de zone de texte nous devons spécifier que la valeur insérée dans la base de données doit être `NULL`. Dans la mesure où le `AddProducts` méthode accepte les types nullable pour les champs de base de données nullable, simplement utiliser un type nullable et définissez sa valeur sur `null` dans le cas où l’entrée d’utilisateur est omise.


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample8.cs)]

Avec le `Inserting` Gestionnaire d’événements terminée, les nouveaux enregistrements peuvent être ajoutés à la `Products` table de base de données par le biais de la ligne de pied de page s GridView. Continuons et essayez d’ajouter plusieurs nouveaux produits.

## <a name="enhancing-and-customizing-the-add-operation"></a>Amélioration et la personnalisation de l’opération d’ajout

Actuellement, en cliquant sur le bouton Ajouter ajoute un nouvel enregistrement à la table de base de données mais ne fournit pas une forme quelconque de rétroaction visuelle que l’enregistrement a été ajoutée. Dans l’idéal, une zone d’alerte Web Label contrôle ou côté client signalerait l’utilisateur qui leur insertion est terminée avec succès. Je laisse cela en guise d’exercice pour le lecteur.

Le contrôle GridView utilisé dans ce didacticiel ne s’applique pas n’importe quel ordre de tri pour les produits listés, ni de l’utilisateur final trier les données. Par conséquent, les enregistrements sont classés comme elles se trouvent dans la base de données par leur champ de clé primaire. Étant donné que chaque nouvel enregistrement a un `ProductID` valeur supérieure à la dernière, chaque fois qu’un nouveau produit est ajouté, il a été ajouté à la fin de la grille. Par conséquent, vous souhaiterez envoyer automatiquement l’utilisateur vers la dernière page du contrôle GridView après avoir ajouté un nouvel enregistrement. Cela est possible en ajoutant la ligne de code suivante après l’appel à `ProductsDataSource.Insert()` dans le `RowCommand` Gestionnaire d’événements pour indiquer que l’utilisateur doit être envoyés à la dernière page après la liaison des données au GridView :


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample9.cs)]

`SendUserToLastPage` est une variable booléenne au niveau des pages qui est initialement affectée à un `false`. Dans le s GridView `DataBound` Gestionnaire d’événements, si `SendUserToLastPage` a la valeur false, le `PageIndex` propriété est mise à jour pour envoyer l’utilisateur vers la dernière page.


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample10.cs)]

La raison pour laquelle le `PageIndex` propriété est définie dans le `DataBound` Gestionnaire d’événements (par opposition à la `RowCommand` Gestionnaire d’événements) est, car lorsque la `RowCommand` Gestionnaire d’événements se déclenche nous ve encore pour ajouter le nouvel enregistrement à la `Products` table de base de données. Par conséquent, dans le `RowCommand` Gestionnaire d’événements au dernier index de page (`PageCount - 1`) représente le dernier index de page *avant* le nouveau produit a été ajouté. Pour la majorité des produits en cours d’ajout, le dernier index de page est le même après avoir ajouté le nouveau produit. Mais lorsque le produit ajouté un nouvel index dernière page, si nous mettre à jour correctement la `PageIndex` dans le `RowCommand` Gestionnaire d’événements est alors être dirigés vers le deuxième à la dernière page (le dernier index de page avant d’ajouter le nouveau produit) par opposition à la dernière page nouvelle i ndex. Dans la mesure où le `DataBound` Gestionnaire d’événements est déclenché une fois que le nouveau produit a été ajouté et les données reliées à la grille, en définissant le `PageIndex` propriété il nous savons que nous re bien le dernier index de page corrects.

Enfin, le contrôle GridView utilisé dans ce didacticiel est assez long en raison du nombre de champs qui doivent être collectées pour l’ajout d’un nouveau produit. En raison de cette largeur, une disposition verticale de DetailsView s peut être préférée. Les opérations de mappage GridView largeur globale pourrait être réduite en collectant les entrées moins. Peut-être nous n t devoir collecter le `UnitsOnOrder`, `UnitsInStock`, et `ReorderLevel` champs lors de l’ajout d’un nouveau produit, auquel cas ces champs pu être supprimés de la GridView.

Pour ajuster les données collectées, nous pouvons utiliser une des deux approches :

- Continuer à utiliser le `AddProduct` méthode qui attend des valeurs pour le `UnitsOnOrder`, `UnitsInStock`, et `ReorderLevel` champs. Dans le `Inserting` Gestionnaire d’événements, fournir codées en dur, par défaut des valeurs à utiliser pour ces entrées qui ont été supprimées de l’interface d’insertion.
- Créer une nouvelle surcharge de la `AddProduct` méthode dans le `ProductsBLL` classe qui n’accepte pas les entrées pour le `UnitsOnOrder`, `UnitsInStock`, et `ReorderLevel` champs. Ensuite, dans la page ASP.NET, configurez l’ObjectDataSource pour utiliser cette nouvelle surcharge.

Soit l’option fonctionnera tout aussi également. Dans au-delà de didacticiels nous avons utilisé cette dernière option, la création de plusieurs surcharges pour les `ProductsBLL` classe s `UpdateProduct` (méthode).

## <a name="summary"></a>Récapitulatif

Le contrôle GridView ne dispose pas de fonctionnalités intégrées d’insertion trouvées dans le contrôle DetailsView et FormView, mais avec un peu d’effort, une interface d’insertion peut être ajoutée à la ligne de pied de page. Pour afficher la ligne de pied de page dans un GridView simplement définie son `ShowFooter` propriété `true`. Le contenu de ligne de pied de page peut être personnalisé pour chaque champ en convertissant le champ en TemplateField et ajout de l’insertion de l’interface pour le `FooterTemplate`. Comme nous l’avons vu dans ce didacticiel, le `FooterTemplate` peut contenir des boutons, zones de texte, DropDownList, les cases à cocher, les contrôles de source de données pour le remplissage de piloté par les données des contrôles Web (par exemple, DropDownList) et les contrôles de validation. En même temps que les contrôles pour la collecte de l’entrée utilisateur s, un bouton Ajouter, LinkButton ou ImageButton est nécessaire.

Lorsque vous cliquez sur le bouton Ajouter, les opérations de mappage ObjectDataSource `Insert()` méthode est appelée pour démarrer le workflow d’insertion. ObjectDataSource appelle ensuite la méthode d’insertion configuré (le `ProductsBLL` classe s `AddProduct` (méthode), dans ce didacticiel). Nous devons également copier les valeurs à partir de la s GridView insertion d’interface pour les opérations de mappage ObjectDataSource `InsertParameters` collection avant la méthode d’insertion invoquée. Cela est possible en référençant par programmation les contrôles de Web interface insertion dans le s ObjectDataSource `Inserting` Gestionnaire d’événements.

Ce didacticiel terminé notre examen des techniques permettant d’améliorer l’apparence de s GridView. L’ensemble suivant de didacticiels examine comment travailler avec des données binaires telles que des images, fichiers PDF, des documents Word et ainsi de suite et les contrôles Web de données.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Bernadette Leigh. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](adding-a-gridview-column-of-checkboxes-cs.md)
> [Suivant](adding-a-gridview-column-of-radio-buttons-vb.md)
