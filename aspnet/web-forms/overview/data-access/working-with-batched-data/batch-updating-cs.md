---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-cs
title: Traitement par lots de la mise à jour (c#) | Microsoft Docs
author: rick-anderson
description: Découvrez comment mettre à jour plusieurs enregistrements de base de données en une seule opération. Dans la couche d’Interface utilisateur, nous créons un GridView où chaque ligne est modifiable. Dans les données...
ms.author: aspnetcontent
ms.date: 06/26/2007
ms.assetid: 4e849bcc-c557-4bc3-937e-f7453ee87265
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-cs
msc.type: authoredcontent
ms.openlocfilehash: 0cb55689ac3a6dc36ed18459ecece82ff0b073b7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842190"
---
<a name="batch-updating-c"></a>Traitement par lots de la mise à jour (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_CS.zip) ou [télécharger le PDF](batch-updating-cs/_static/datatutorial64cs1.pdf)

> Découvrez comment mettre à jour plusieurs enregistrements de base de données en une seule opération. Dans la couche d’Interface utilisateur, nous créons un GridView où chaque ligne est modifiable. Dans la couche d’accès aux données nous encapsuler toutes les opérations de mise à jour dans une transaction pour vous assurer que toutes les mises à jour réussissent ou que toutes les mises à jour sont restaurées.


## <a name="introduction"></a>Introduction

Dans le [didacticiel précédent](wrapping-database-modifications-within-a-transaction-cs.md) nous avons vu comment étendre la couche d’accès aux données pour ajouter la prise en charge des transactions de base de données. Les transactions de base de données garantissent qu’une série d’instructions de modification de données sera considérée comme une seule opération atomique, ce qui garantit que toutes les modifications échoue ou réussiront tous. Avec cette couche DAL fonctionnalités de bas niveau disparaître, nous re prêts à porter notre attention à la création d’interfaces de modification de données par lots.

Dans ce didacticiel, nous allons créer un GridView où chaque ligne est modifiable (voir Figure 1). Dans la mesure où chaque ligne n’est rendue dans son interface de modification, cet emplacement s aucun nécessaire pour une colonne de la modifier, mettre à jour et les boutons d’annulation. Au lieu de cela, il existe deux boutons de mettre à jour des produits sur la page qui, lorsque vous cliquez dessus, énumérer les lignes de GridView et mettre à jour de la base de données.


[![Chaque ligne dans le contrôle GridView est modifiable](batch-updating-cs/_static/image1.gif)](batch-updating-cs/_static/image1.png)

**Figure 1**: chaque ligne dans le contrôle GridView est modifiable ([cliquez pour afficher l’image en taille réelle](batch-updating-cs/_static/image2.png))


Laissez s commencer !

> [!NOTE]
> Dans le [effectuant des mises à jour par lots](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) didacticiel, nous avons créé une modification de l’interface à l’aide du contrôle DataList. Ce didacticiel diffère du précédent dans qui est utilise un GridView et la mise à jour par lots est effectuée dans l’étendue d’une transaction. Après avoir terminé ce didacticiel je vous encourage à revenir au didacticiel antérieures et mettre à jour pour utiliser les fonctionnalités liées aux transactions de base de données ajoutées dans le didacticiel précédent.


## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>Examiner les étapes pour effectuer toutes les lignes de GridView modifiable

Comme indiqué dans le [une vue d’ensemble d’insertion, mise à jour et suppression des données](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) didacticiel, le contrôle GridView offre une prise en charge intégrée pour la modification de ses données sous-jacentes sur une ligne par ligne. En interne, le contrôle GridView notes quelle ligne est modifiable via son [ `EditIndex` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx). Comme le contrôle GridView est lié à sa source de données, il vérifie chaque ligne pour voir si l’index de la ligne est égale à la valeur de `EditIndex`. Dans ce cas, cette ligne s champs sont rendus à l’aide de la modification de leurs interfaces. Pour BoundFields, l’interface de modification est une zone de texte dont `Text` propriété est affectée à la valeur du champ de données spécifié par le s BoundField `DataField` propriété. Pour TemplateFields, le `EditItemTemplate` est utilisé à la place de la `ItemTemplate`.

Rappelez-vous que l’édition du workflow démarre lorsqu’un utilisateur clique sur un bouton de modification de ligne s. Cela entraîne une publication, définit les opérations de mappage GridView `EditIndex` propriété à l’index de ligne utilisateur a cliqué dessus s et Reliaisons les données à la grille. Quand une ligne s bouton Cancel, lors de la publication la `EditIndex` est défini sur une valeur de `-1` avant les données à la grille de reliaison. Étant donné que les lignes de s GridView démarrer l’indexation à zéro, le paramètre `EditIndex` à `-1` a pour effet d’afficher le contrôle GridView en mode lecture seule.

Le `EditIndex` propriété fonctionne bien pour modification par ligne, mais n’est pas conçue pour l’édition de lot. Pour rendre le contrôle GridView entière modifiables, que nous devons disposer à chaque ligne de rendus à l’aide de son interface de modification. Le moyen le plus simple pour y parvenir consiste à créer dans lequel chaque champ modifiable est implémenté comme un TemplateField avec son interface de modification est défini dans le `ItemTemplate`.

Le prochain plusieurs étapes nous créerons un GridView complètement modifiable. À l’étape 1 nous commençons par créer le contrôle GridView et son ObjectDataSource et convertir son BoundFields et du CheckBoxField TemplateField. Dans les étapes 2 et 3, nous allons déplacer les interfaces de modifications à partir de la TemplateFields `EditItemTemplate` s pour leurs `ItemTemplate` s.

## <a name="step-1-displaying-product-information"></a>Étape 1 : Afficher des informations de produit

Avant de nous soucier de la création d’un GridView où des lignes sont modifiables, permettent de s commencent par simplement afficher les informations de produit. Ouvrez le `BatchUpdate.aspx` page dans le `BatchData` dossier et faites glisser un GridView à partir de la boîte à outils vers le concepteur. Définir les opérations de mappage GridView `ID` à `ProductsGrid` et, à partir de sa balise active, choisir de lier à un nouveau ObjectDataSource nommé `ProductsDataSource`. Configurer l’ObjectDataSource pour récupérer ses données à partir de la `ProductsBLL` classe s `GetProducts` (méthode).


[![Configurer pour utiliser la classe ProductsBLL ObjectDataSource](batch-updating-cs/_static/image2.gif)](batch-updating-cs/_static/image3.png)

**Figure 2**: configurer ObjectDataSource à utiliser le `ProductsBLL` classe ([cliquez pour afficher l’image en taille réelle](batch-updating-cs/_static/image4.png))


[![Récupérer les données de produit à l’aide de la méthode GetProducts](batch-updating-cs/_static/image3.gif)](batch-updating-cs/_static/image5.png)

**Figure 3**: récupérer les données de produit à l’aide de la `GetProducts` (méthode) ([cliquez pour afficher l’image en taille réelle](batch-updating-cs/_static/image6.png))


Comme le contrôle GridView, les fonctionnalités de modification ObjectDataSource s sont conçues pour fonctionner sur une ligne par ligne. Pour mettre à jour un jeu d’enregistrements, nous allons devoir écrire un peu de code dans la classe code-behind de pages ASP.NET qui regroupe les données et la transmet à la couche BLL. Par conséquent, définir les listes déroulantes dans le s ObjectDataSource onglets UPDATE, INSERT et DELETE (None). Cliquez sur Terminer pour terminer l’Assistant.


[![Définir les listes déroulantes dans la mise à jour, insertion et supprimer des onglets à (None)](batch-updating-cs/_static/image4.gif)](batch-updating-cs/_static/image7.png)

**Figure 4**: la valeur est la liste déroulante répertorie dans la mise à jour, insertion et supprimer des onglets (aucun) ([cliquez pour afficher l’image en taille réelle](batch-updating-cs/_static/image8.png))


À l’issue de l’Assistant Configurer la Source de données, le balisage déclaratif de s ObjectDataSource doit ressembler à ce qui suit :


[!code-aspx[Main](batch-updating-cs/samples/sample1.aspx)]

Fin de l’Assistant Configurer la Source de données entraîne également Visual Studio créer BoundFields et un CheckBoxField pour les champs de données de produit dans le contrôle GridView. Pour ce didacticiel, permettent d’autoriser uniquement l’utilisateur afficher et modifier le nom du produit s, catégorie, prix et état abandonné s. Supprimer tout sauf la `ProductName`, `CategoryName`, `UnitPrice`, et `Discontinued` champs et renommez le `HeaderText` des trois premières propriétés de champs à catégorie, les produits et les prix, respectivement. Finalement, activez les cases à cocher Activer la pagination et activer le tri dans la balise active de s GridView.

À ce stade le contrôle GridView présente trois BoundFields (`ProductName`, `CategoryName`, et `UnitPrice`) et un CheckBoxField (`Discontinued`). Nous avons besoin convertir ces quatre champs TemplateField, puis déplacez l’interface de modification à partir de la s TemplateField `EditItemTemplate` à son `ItemTemplate`.

> [!NOTE]
> Nous avons exploré création et personnalisation de TemplateFields dans le [personnalisation de l’Interface de Modification de données](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) didacticiel. Nous allons étudier les étapes de conversion de la BoundFields et CheckBoxField en TemplateField et définir leur modification des interfaces dans leurs `ItemTemplate` s, mais si vous êtes bloqué ou avez besoin de rafraîchir la mémoire, don t hésitez pas à vous référer à ce didacticiel antérieures.


À partir de la balise active de s GridView, cliquez sur le lien Modifier les colonnes pour ouvrir la boîte de dialogue champs. Ensuite, sélectionnez chaque champ et cliquez sur la convertir ce champ en TemplateField lien.


![Convertir le BoundFields existant et CheckBoxField TemplateField](batch-updating-cs/_static/image5.gif)

**Figure 5**: convertir le BoundFields existant et CheckBoxField TemplateField


Maintenant que chaque champ est un TemplateField, nous vous êtes prêt à passer à la modification d’interface à partir de la `EditItemTemplate` s pour le `ItemTemplate` s.

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>Étape 2 : Création de la`ProductName`,`UnitPrice`, et`Discontinued`modification des Interfaces

Création de la `ProductName`, `UnitPrice`, et `Discontinued` modification des interfaces sont la rubrique de cette étape et sont relativement simple, tel que chaque interface est déjà défini dans le s TemplateField `EditItemTemplate`. Création de la `CategoryName` modification d’interface est un peu plus complexe, car nous devons créer un contrôle DropDownList des catégories applicables. Cela `CategoryName` modification d’interface est abordé à l’étape 3.

S permettent de démarrer avec le `ProductName` TemplateField. Cliquez sur le lien Modifier les modèles à partir de la balise active de s GridView et explorez pour le `ProductName` TemplateField s `EditItemTemplate`. Sélectionnez la zone de texte, copiez-le dans le Presse-papiers, puis collez-le dans le `ProductName` TemplateField s `ItemTemplate`. Modifier la zone de texte s `ID` propriété `ProductName`.

Ensuite, ajoutez un contrôle RequiredFieldValidator à la `ItemTemplate` pour vous assurer que l’utilisateur fournit une valeur pour chaque nom de produit s. Définir le `ControlToValidate` propriété ProductName, le `ErrorMessage` propriété, vous devez fournir le nom du produit. et le `Text` propriété \*. Après avoir apporté ces ajouts à la `ItemTemplate`, votre écran doit ressembler à la Figure 6.


[![Le présent TemplateField ProductName inclut une zone de texte et un contrôle RequiredFieldValidator](batch-updating-cs/_static/image6.gif)](batch-updating-cs/_static/image9.png)

**Figure 6**: le `ProductName` TemplateField inclut désormais une zone de texte et un contrôle RequiredFieldValidator ([cliquez pour afficher l’image en taille réelle](batch-updating-cs/_static/image10.png))


Pour le `UnitPrice` modification d’interface, commencez par copie de la zone de texte à partir de la `EditItemTemplate` à la `ItemTemplate`. Ensuite, placez un $ devant la zone de texte et un ensemble son `ID` propriété UnitPrice et son `Columns` propriété à 8.

Ajoutez également un CompareValidator pour les `UnitPrice` s `ItemTemplate` pour vous assurer que la valeur entrée par l’utilisateur est une valeur de devise valide supérieure ou égale à 0,00 $. Définir les opérations de mappage du programme de validation `ControlToValidate` propriété UnitPrice, son `ErrorMessage` propriété, vous devez entrer une valeur de devise valide. Veuillez omettre les symboles de devise., son `Text` propriété \*, ses `Type` propriété `Currency`, ses `Operator` propriété `GreaterThanEqual`et son `ValueToCompare` 0 à la propriété.


[![Ajouter un CompareValidator pour garantir le prix entré est une valeur de devise Non négative](batch-updating-cs/_static/image7.gif)](batch-updating-cs/_static/image11.png)

**Figure 7**: ajouter un CompareValidator pour garantir le prix entré est une valeur de devise Non négative ([cliquez pour afficher l’image en taille réelle](batch-updating-cs/_static/image12.png))


Pour le `Discontinued` TemplateField, vous pouvez utiliser la case à cocher déjà définie dans le `ItemTemplate`. Il suffit de définir son `ID` à abandonné et son `Enabled` propriété `true`.

## <a name="step-3-creating-thecategorynameediting-interface"></a>Étape 3 : Création de la`CategoryName`modification d’Interface

L’interface de modification dans le `CategoryName` TemplateField s `EditItemTemplate` contient une zone de texte qui affiche la valeur de la `CategoryName` champ de données. Nous avons besoin de le remplacer par un contrôle DropDownList qui répertorie les catégories possibles.

> [!NOTE]
> Le [personnalisation de l’Interface de Modification de données](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) didacticiel contient une discussion plus approfondie et complète sur la personnalisation d’un modèle pour inclure un contrôle DropDownList par opposition à une zone de texte. Bien que les étapes décrites ici sont terminées, elles sont présentées tersely. Pour une présentation plus détaillée à créer et configurer les catégories DropDownList, faire référence à la [personnalisation de l’Interface de Modification de données](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) didacticiel.


Faites glisser un contrôle DropDownList à partir de la boîte à outils vers le `CategoryName` TemplateField s `ItemTemplate`, ce qui affecte ses `ID` à `Categories`. À ce stade nous généralement définirait la source de données s DropDownList via sa balise active, création d’un nouveau ObjectDataSource. Toutefois, cela ajoutera ObjectDataSource dans le `ItemTemplate`, ce qui entraînera une instance de l’ObjectDataSource créée pour chaque ligne GridView. Au lieu de cela, permettre de s ObjectDataSource en dehors de la s GridView TemplateField. Fin de la modification de modèle et faites glisser un ObjectDataSource à partir de la boîte à outils vers le concepteur sous la `ProductsDataSource` ObjectDataSource. Nommez le nouveau ObjectDataSource `CategoriesDataSource` et configurez-le pour utiliser le `CategoriesBLL` classe s `GetCategories` (méthode).


[![Configurer pour utiliser la classe CategoriesBLL ObjectDataSource](batch-updating-cs/_static/image8.gif)](batch-updating-cs/_static/image13.png)

**Figure 8**: configurer ObjectDataSource à utiliser le `CategoriesBLL` clas. ([cliquez pour afficher l’image en taille réelle](batch-updating-cs/_static/image14.png))


[![Récupérer les données de catégorie à l’aide de la méthode GetCategories](batch-updating-cs/_static/image9.gif)](batch-updating-cs/_static/image15.png)

**Figure 9**: récupérer les données de catégorie à l’aide de la `GetCategories` (méthode) ([cliquez pour afficher l’image en taille réelle](batch-updating-cs/_static/image16.png))


Dans la mesure où cette ObjectDataSource est utilisé simplement pour récupérer des données, définissez les listes déroulantes dans les onglets UPDATE et DELETE (None). Cliquez sur Terminer pour terminer l’Assistant.


[![Définir les listes déroulantes dans la mise à jour et supprimer les tabulations à (None)](batch-updating-cs/_static/image10.gif)](batch-updating-cs/_static/image17.png)

**Figure 10**: la valeur est la liste déroulante répertorie dans la mise à jour et supprimer des onglets (aucun) ([cliquez pour afficher l’image en taille réelle](batch-updating-cs/_static/image18.png))


Après la fin de l’Assistant, le `CategoriesDataSource` balisage déclaratif s doit ressembler à ce qui suit :


[!code-aspx[Main](batch-updating-cs/samples/sample2.aspx)]

Avec le `CategoriesDataSource` créé et configuré, revenir à la `CategoryName` TemplateField s `ItemTemplate` et, à partir de la balise active de s DropDownList, cliquez sur le lien de choisir la Source de données. Dans l’Assistant Configuration de Source de données, sélectionnez le `CategoriesDataSource` option dans la première liste déroulante et choisissez les `CategoryName` utilisé pour l’affichage et `CategoryID` comme valeur.


[![Lier l’objet DropDownList pour la CategoriesDataSource](batch-updating-cs/_static/image11.gif)](batch-updating-cs/_static/image19.png)

**Figure 11**: lier l’objet DropDownList pour la `CategoriesDataSource` ([cliquez pour afficher l’image en taille réelle](batch-updating-cs/_static/image20.png))


À ce stade le `Categories` DropDownList répertorie toutes les catégories, mais il ne pas encore automatiquement sélectionne la catégorie appropriée pour le produit lié à la ligne GridView. Pour cela nous devons définir la `Categories` DropDownList s `SelectedValue` au produit s `CategoryID` valeur. Cliquez sur le lien Modifier les DataBindings DropDownList s smart tag et associer le `SelectedValue` propriété avec le `CategoryID` de champ de données comme indiqué dans la Figure 12.


![Lier la valeur de CategoryID produit s à la propriété SelectedValue de s DropDownList](batch-updating-cs/_static/image12.gif)

**Figure 12**: lier le produit s `CategoryID` valeur au s DropDownList `SelectedValue` propriété


Une dernière reste de problème : si le produit n’a un `CategoryID` valeur spécifiée puis l’instruction de liaison de données sur `SelectedValue` provoquera une exception. Il s’agit, car l’objet DropDownList contient uniquement les éléments pour les catégories et n’offre pas une option pour les produits qui ont un `NULL` valeur pour la base de données `CategoryID`. Pour résoudre ce problème, définissez le s DropDownList `AppendDataBoundItems` propriété `true` et ajouter un nouvel élément à la liste DropDownList, en omettant le `Value` propriété à partir de la syntaxe déclarative. Autrement dit, assurez-vous que le `Categories` syntaxe déclarative des s DropDownList ressemble à ceci :


[!code-aspx[Main](batch-updating-cs/samples/sample3.aspx)]

Notez comment la `<asp:ListItem Value="">` --sélectionner 1--présente ses `Value` attribut défini explicitement sur une chaîne vide. Faire référence à la [personnalisation de l’Interface de Modification de données](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) didacticiel pour une discussion plus détaillée sur la raison pour laquelle cet élément DropDownList supplémentaire est nécessaire pour gérer le `NULL` cas et pourquoi affectation de la `Value` propriété à une chaîne vide est essentielle.

> [!NOTE]
> Il existe une performance et évolutivité problème potentiel ici qui est intéressant de mentionner. Dans la mesure où chaque ligne comporte un DropDownList qui utilise le `CategoriesDataSource` comme source de données, le `CategoriesBLL` classe s `GetCategories` méthode sera appelée *n* visitent de fois par page, où *n* est le nombre de lignes dans le contrôle GridView. Ces *n* appelle à `GetCategories` entraîne *n* requêtes à la base de données. Cet impact sur la base de données peut être atténué en mettant en cache les catégories retournés dans un cache par requête ou via la couche de mise en cache à l’aide d’une mise en cache de la dépendance ou à une expiration très courte temporelle de SQL. Pour plus d’informations sur la demande par la mise en cache option, consultez [ `HttpContext.Items` un Store de Cache par requête](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx).


## <a name="step-4-completing-the-editing-interface"></a>Étape 4 : Fin de l’Interface de modification

Nous sauter quelques modifications apportées aux modèles s GridView sans marquer de pause pour afficher notre progression. Prenez un moment pour consulter notre progression via un navigateur. Comme le montre la Figure 13, chaque ligne est restitué à l’aide de son `ItemTemplate`, qui contient le s cellule modification d’interface.


[![Chaque ligne GridView est modifiable](batch-updating-cs/_static/image13.gif)](batch-updating-cs/_static/image21.png)

**Figure 13**: chaque ligne GridView est modifiable ([cliquez pour afficher l’image en taille réelle](batch-updating-cs/_static/image22.png))


Il existe quelques problèmes mineurs mise en forme qui nous devrions prendre en charge à ce stade. Tout d’abord, notez que le `UnitPrice` valeur contient quatre décimales. Pour résoudre ce problème, revenez à la `UnitPrice` TemplateField s `ItemTemplate` et, à partir de la balise active s de zone de texte, cliquez sur le lien Modifier les DataBindings. Ensuite, spécifiez que le `Text` propriété doit être sous forme de nombre.


![Mettre en forme la propriété de texte sous forme de nombre](batch-updating-cs/_static/image14.gif)

**Figure 14**: Format le `Text` propriété sous forme de nombre


En second lieu, let s centre de la case à cocher dans la `Discontinued` colonne (plutôt que de sorte qu’il est aligné à gauche). Cliquez sur Modifier les colonnes à partir de la balise active de s GridView et sélectionnez le `Discontinued` TemplateField dans la liste des champs dans le coin inférieur gauche. Explorez `ItemStyle` et définir le `HorizontalAlign` propriété Center, comme illustré dans la Figure 15.


![Centre de la case à cocher supprimée](batch-updating-cs/_static/image15.gif)

**Figure 15**: centre la `Discontinued` case à cocher


Ensuite, ajoutez un contrôle ValidationSummary à la page et définissez son `ShowMessageBox` propriété `true` et son `ShowSummary` propriété `false`. Également ajouter que le bouton Web des contrôles qui, lorsque vous cliquez sur, met à jour l’utilisateur change de s. Plus précisément, ajoutez deux contrôles de bouton Web, un au-dessus de la GridView et celui ci-dessous, en définissant les deux contrôles `Text` propriétés aux produits de mise à jour.

Depuis le s GridView modification d’interface est définie dans ses TemplateField `ItemTemplate` s, le `EditItemTemplate` s sont superflus et peuvent être supprimées.

Une fois que ce qui précède mentionné modifications de mise en forme, ajouter les contrôles de bouton et suppression superflu `EditItemTemplate` s, votre syntaxe déclarative s de page doit ressembler à ce qui suit :


[!code-aspx[Main](batch-updating-cs/samples/sample4.aspx)]

Figure 16 illustre cette page lorsqu’ils sont affichés via un navigateur une fois que les contrôles bouton Web ont été ajoutées et les modifications de mise en forme.


[![La Page maintenant inclut deux boutons de produits de mise à jour](batch-updating-cs/_static/image16.gif)](batch-updating-cs/_static/image23.png)

**Figure 16**: la Page maintenant inclut deux mises à jour produits boutons ([cliquez pour afficher l’image en taille réelle](batch-updating-cs/_static/image24.png))


## <a name="step-5-updating-the-products"></a>Étape 5 : Mise à jour des produits

Lorsqu’un utilisateur visite cette page, ils apportent leurs modifications et puis cliquez sur une des deux boutons de produits de mise à jour. À ce stade, nous avons besoin d’une certaine manière enregistrer les valeurs entrées par l’utilisateur pour chaque ligne dans un `ProductsDataTable` d’instance et puis passez-le à une méthode de couche de logique métier qui transmet ensuite qui `ProductsDataTable` instance à la couche DAL s `UpdateWithTransaction` (méthode). Le `UpdateWithTransaction` (méthode), que nous avons créée dans le [didacticiel précédent](wrapping-database-modifications-within-a-transaction-cs.md), garantit que le lot de modifications sera mis à jour comme une opération atomique.

Créez une méthode nommée `BatchUpdate` dans `BatchUpdate.aspx.cs` et ajoutez le code suivant :


[!code-csharp[Main](batch-updating-cs/samples/sample5.cs)]

Cette méthode commence par l’obtention de tous les produits dans un `ProductsDataTable` via un appel à la couche BLL s `GetProducts` (méthode). Il énumère ensuite les `ProductGrid` GridView s [ `Rows` collection](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx). Le `Rows` collection contient un [ `GridViewRow` instance](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx) pour chaque ligne affichée dans le contrôle GridView. Étant donné que nous vous montrons au plus dix lignes par page, les opérations de mappage GridView `Rows` collection aura pas plus de dix éléments.

Pour chaque ligne le `ProductID` est retiré de la `DataKeys` approprié et collection `ProductsRow` est sélectionné dans le `ProductsDataTable`. Les quatre contrôles d’entrée TemplateField sont référencés par programmation et leurs valeurs assignées à la `ProductsRow` s propriétés de l’instance. Après chaque GridView les valeurs de ligne s ont été utilisées pour mettre à jour le `ProductsDataTable`, il s passé à la couche BLL s `UpdateWithTransaction` méthode qui, comme nous l’avons vu dans le didacticiel précédent, appelle simplement vers le bas dans la couche DAL s `UpdateWithTransaction` (méthode).

L’algorithme de mise à jour de lot utilisé pour ce didacticiel met à jour chaque ligne dans le `ProductsDataTable` qui correspond à une ligne dans le contrôle GridView, indépendamment de si les informations de produit s a été modifiées. Alors que ce blind met généralement à jour ne sont pas toujours un problème de performances, elles peuvent entraîner des enregistrements superflus si vous faites l’audit change à la table de base de données. Dans le [effectuant des mises à jour par lots](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) didacticiel, nous avons exploré un lot de mise à jour d’interface avec le contrôle DataList et ajouté du code uniquement mise à jour les enregistrements qui ont été réellement modifiés par l’utilisateur. N’hésitez pas à utiliser les techniques de [effectuant des mises à jour par lots](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) pour mettre à jour le code dans ce didacticiel, si vous le souhaitez.

> [!NOTE]
> Lors de la liaison de la source de données pour le contrôle GridView via sa balise active, Visual Studio affecte automatiquement les valeurs de clé primaire données source s au GridView s `DataKeyNames` propriété. Si vous n’avez pas lié à ObjectDataSource au GridView via la balise active de GridView s comme indiqué à l’étape 1, vous devez définir manuellement le s GridView `DataKeyNames` propriété ProductID afin d’accéder à la `ProductID` valeur pour chaque ligne via le `DataKeys` collection.


Le code utilisé dans `BatchUpdate` est similaire à celle utilisée dans la couche BLL s `UpdateProduct` méthodes, la principale différence est que, dans le `UpdateProduct` méthodes qu’un seul `ProductRow` instance est récupérée à partir de l’architecture. Le code qui affecte les propriétés de la `ProductRow` est le même entre le `UpdateProducts` méthodes et le code dans le `foreach` boucle dans la `BatchUpdate`, comme est le modèle global.

Pour suivre ce didacticiel, nous devons disposer le `BatchUpdate` méthode appelée lorsque vous cliquez sur un des boutons de produits de mise à jour. Créer des gestionnaires d’événements pour le `Click` événements de ces deux contrôles de bouton et ajoutez le code suivant dans les gestionnaires d’événements :


[!code-csharp[Main](batch-updating-cs/samples/sample6.cs)]

Tout d’abord un appel est fait pour `BatchUpdate`. Ensuite, le `ClientScript property` est utilisé pour injecter du code JavaScript qui affiche un messagebox qui lit les produits ont été mis à jour.

Prenez une minute pour tester ce code. Visitez `BatchUpdate.aspx` via un navigateur, modifier un nombre de lignes, puis cliquez sur un des boutons de produits de mise à jour. En supposant qu’il n’y a aucune erreur de validation d’entrée, vous devez voir une messagebox qui lit que les produits ont été mis à jour. Pour vérifier la cohérence de la mise à jour, vous pouvez envisager un aléatoire `CHECK` contrainte, comme un qui n’autorise pas `UnitPrice` valeurs de 1234.56 en. Ensuite, dans `BatchUpdate.aspx`, modifier un nombre d’enregistrements, en veillant à définir un des produit s `UnitPrice` valeur à la valeur interdite (1234.56 en). Cela doit entraîner une erreur lors de l’en cliquant sur les produits de mise à jour avec les autres modifications au cours de cette opération de traitement par lots restaurée à leurs valeurs d’origine.

## <a name="an-alternativebatchupdatemethod"></a>Une Alternative`BatchUpdate`(méthode)

Le `BatchUpdate` méthode on exagère un peu examiné récupère *tous les* des produits à partir de la couche BLL s `GetProducts` méthode puis met à jour uniquement les enregistrements qui s’affichent dans le contrôle GridView. Cette approche est idéale si le contrôle GridView n’utilise pas la pagination, mais le cas échéant, il peut être des centaines, des milliers ou des dizaines de milliers de produits, mais seulement dix lignes dans le contrôle GridView. Dans ce cas, l’obtention de tous les produits à partir de la base de données uniquement pour modifier les 10 d'entre eux est loin d’être idéal.

Pour ces types de situations, envisagez d’utiliser les éléments suivants `BatchUpdateAlternate` méthode à la place :


[!code-csharp[Main](batch-updating-cs/samples/sample7.cs)]

`BatchMethodAlternate` démarre en créant un nouveau vide `ProductsDataTable` nommé `products`. Il puis parcourt le s GridView `Rows` collection et, pour chaque ligne Obtient les informations de produit spécifique à l’aide de la couche BLL s `GetProductByProductID(productID)` (méthode). Récupérées `ProductsRow` instance a ses propriétés mis à jour dans la même manière que `BatchUpdate`, mais après la mise à jour de la ligne, il est importé dans le `products``ProductsDataTable` via le s DataTable [ `ImportRow(DataRow)` méthode](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx).

Après le `foreach` boucle se termine, `products` contient un `ProductsRow` instance pour chaque ligne dans le contrôle GridView. Étant donné que chaque du `ProductsRow` instances ont été ajoutées à la `products` (au lieu de mise à jour), si nous transmettons à aveuglément le `UpdateWithTransaction` (méthode) le `ProductsTableAdatper` tente d’insérer les enregistrements de la base de données. Au lieu de cela, nous devons spécifier que chacune de ces lignes ont été modifiée (ne pas ajouté).

Cela est possible en ajoutant une nouvelle méthode à la couche BLL nommée `UpdateProductsWithTransaction`. `UpdateProductsWithTransaction`, jeux illustré ci-dessous, le `RowState` de chacune de la `ProductsRow` instances dans le `ProductsDataTable` à `Modified` , puis transmet le `ProductsDataTable` à la couche DAL s `UpdateWithTransaction` (méthode).


[!code-csharp[Main](batch-updating-cs/samples/sample8.cs)]

## <a name="summary"></a>Récapitulatif

Le contrôle GridView fournit des fonctionnalités de modification de ligne intégré, mais ne dispose pas de prise en charge pour la création d’interfaces entièrement modifiables. Comme nous l’avons vu dans ce didacticiel, ces interfaces sont possibles, mais nécessitent un peu de travail. Pour créer un GridView où chaque ligne est modifiable, nous devons convertir les champs de s GridView en TemplateField et définir l’interface de modification dans le `ItemTemplate` s. En outre, mettre à jour tous les - type de contrôle de bouton Web doit être ajouté à la page, distincte de GridView. Ces boutons `Click` gestionnaires d’événements doivent énumérer le s GridView `Rows` collecte, stocke les modifications dans un `ProductsDataTable`et transmettre les informations mises à jour dans la méthode appropriée de la couche BLL.

Dans le didacticiel suivant, nous allons voir comment créer une interface pour la suppression par lots. En particulier, chaque ligne GridView inclut une case à cocher et à la place de tout mettre à jour-tapez boutons, nous aurons des boutons de supprimer les lignes sélectionnées.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Teresa Murphy et David Suru. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](wrapping-database-modifications-within-a-transaction-cs.md)
> [Suivant](batch-deleting-cs.md)
