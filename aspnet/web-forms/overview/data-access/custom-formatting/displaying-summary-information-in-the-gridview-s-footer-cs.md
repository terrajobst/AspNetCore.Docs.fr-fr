---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
title: Affichage des informations récapitulatives dans le pied de page du GridView (c#) | Microsoft Docs
author: rick-anderson
description: Informations de résumé sont souvent affichées en bas du rapport dans une ligne de résumé. Le contrôle GridView peut inclure une ligne de pied de page dans des cellules dont nous pouvons pr...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: d50edc31-9286-4c6a-8635-be09e72752a4
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: 76df8ea925f4485b52090723b2f0a37b25f7e684
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826566"
---
<a name="displaying-summary-information-in-the-gridviews-footer-c"></a>Affichage des informations récapitulatives dans le pied de page du GridView (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_15_CS.exe) ou [télécharger le PDF](displaying-summary-information-in-the-gridview-s-footer-cs/_static/datatutorial15cs1.pdf)

> Informations de résumé sont souvent affichées en bas du rapport dans une ligne de résumé. Le contrôle GridView peut inclure une ligne de pied de page dans des cellules dont nous pouvons par programmation injecter des données d’agrégation. Dans ce didacticiel, nous allons voir comment afficher les données agrégées dans cette ligne de pied de page.


## <a name="introduction"></a>Introduction

Outre l’affichage de chacun des prix de produits, les unités en stock, les unités sur l’ordre et les niveaux de réorganisation, un utilisateur peut également être intéressé par des informations d’agrégation, telles que le prix moyen, le nombre total d’unités en stock et ainsi de suite. Ces informations de résumé sont souvent affichées en bas du rapport dans une ligne de résumé. Le contrôle GridView peut inclure une ligne de pied de page dans des cellules dont nous pouvons par programmation injecter des données d’agrégation.

Cette tâche propose trois défis :

1. Configurer le contrôle GridView pour afficher sa ligne de pied de page
2. Déterminer les données de synthèse ; Autrement dit, comment nous calculons le prix moyen ou le total des unités en stock ?
3. Injecter des données de synthèse dans les cellules appropriées de la ligne de pied de page

Dans ce didacticiel, nous allons voir comment relever ces défis. Plus précisément, nous allons créer une page qui répertorie les catégories dans une liste déroulante avec les produits de la catégorie sélectionnée affichés dans un GridView. Le contrôle GridView inclura une ligne de pied de page qui affiche le prix moyen et le nombre total d’unités en stock et sur l’ordre pour les produits de cette catégorie.


[![Informations de résumé s’affiche dans la ligne de pied de page du GridView](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image1.png)

**Figure 1**: informations de résumé s’affiche dans la ligne de pied de page du GridView ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image3.png))


Ce didacticiel, avec sa catégorie à interface maître/détail de produits, s’appuie sur les concepts abordés dans l’ancien [filtrage de maître/détail avec un DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) didacticiel. Si vous n’avez pas encore travaillé dans le didacticiel antérieur, veuillez le faire avant de poursuivre celle-ci.

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>Étape 1 : Ajout de la Categories DropDownList et produits GridView

Avant concernant nous-mêmes avec l’ajout d’informations de synthèse au pied de page du GridView, nous allons tout d’abord simplement créer le rapport maître/détail. Une fois que nous avons terminé cette première étape, nous allons examiner comment inclure des données de synthèse.

Commencez par ouvrir le `SummaryDataInFooter.aspx` page dans le `CustomFormatting` dossier. Ajoutez un contrôle DropDownList et définissez son `ID` à `Categories`. Ensuite, cliquez sur le lien de choisir la Source de données à partir de la balise active de l’objet DropDownList et choisir d’ajouter un nouveau ObjectDataSource nommé `CategoriesDataSource` qui appelle le `CategoriesBLL` la classe `GetCategories()` (méthode).


[![Ajouter un nouveau ObjectDataSource nommé CategoriesDataSource](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image4.png)

**Figure 2**: ajouter une nouvelle nommée de ObjectDataSource `CategoriesDataSource` ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image6.png))


[![Avez ObjectDataSource appeler GetCategories() (méthode de la classe CategoriesBLL)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image7.png)

**Figure 3**: que l’ObjectDataSource appelle le `CategoriesBLL` la classe `GetCategories()` (méthode) ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image9.png))


Après avoir configuré l’ObjectDataSource, l’Assistant retourne contribué à la Configuration de Source de données de l’objet DropDownList Assistant à partir de laquelle nous devons spécifier quelle valeur de champ de données doit être affiché et lequel doit correspondre à la valeur de l’objet DropDownList `ListItem` s. Avoir le `CategoryName` champ affiché et l’utilisation du `CategoryID` comme valeur.


[![Utiliser le nom de catégorie et les champs de CategoryID en tant que le texte et la valeur pour les ListItems,](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image10.png)

**Figure 4**: utilisez le `CategoryName` et `CategoryID` champs comme le `Text` et `Value` pour le `ListItem` s, respectivement ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image12.png))


À ce stade, nous avons un DropDownList (`Categories`) qui répertorie les catégories dans le système. Nous devons maintenant ajouter un GridView qui répertorie les produits qui appartiennent à la catégorie sélectionnée. Avant cela, cependant, prenez un moment pour la case à cocher Activer AutoPostBack dans la balise active de la liste DropDownList. Comme indiqué dans le *filtrage de maître/détail avec un DropDownList* (didacticiel), en définissant la DropDownList `AutoPostBack` propriété `true` la page sera être republiée chaque fois que la valeur de DropDownList est modifiée. Cela entraîne le contrôle GridView à être actualisé, montrant les produits pour la catégorie qui vient d’être sélectionnée. Si le `AutoPostBack` propriété est définie sur `false` (la valeur par défaut), la modification de la catégorie n’entraînera pas une publication (postback) et par conséquent ne mettre à jour les produits listés.


[![La case Activer AutoPostBack dans la balise active de la liste DropDownList](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image13.png)

**Figure 5**: case à cocher Activer AutoPostBack dans la balise active de l’objet DropDownList ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image15.png))


Ajouter un contrôle GridView à la page pour afficher les produits pour la catégorie sélectionnée. Définir le GridView `ID` à `ProductsInCategory` et la lier à un nouveau ObjectDataSource nommé `ProductsInCategoryDataSource`.


[![Ajouter un nouveau ObjectDataSource nommé ProductsInCategoryDataSource](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image16.png)

**Figure 6**: ajouter une nouvelle nommée de ObjectDataSource `ProductsInCategoryDataSource` ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image18.png))


Configurer l’ObjectDataSource afin qu’elle appelle le `ProductsBLL` la classe `GetProductsByCategoryID(categoryID)` (méthode).


[![Avez ObjectDataSource appeler la méthode GetProductsByCategoryID(categoryID)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image19.png)

**Figure 7**: que l’ObjectDataSource appelle le `GetProductsByCategoryID(categoryID)` (méthode) ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image21.png))


Dans la mesure où le `GetProductsByCategoryID(categoryID)` méthode prend un paramètre d’entrée, à l’étape finale de l’Assistant, nous pouvons spécifier la source de la valeur du paramètre. Pour afficher ces produits à partir de la catégorie sélectionnée, ont le paramètre extrait le `Categories` DropDownList.


[![Obtenir la valeur du paramètre categoryID dans la liste de catégories sélectionné DropDownList](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image22.png)

**Figure 8**: obtenir le *`categoryID`* valeur du paramètre dans la liste de catégories sélectionné DropDownList ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image24.png))


Après la fin de l’Assistant GridView aura un BoundField pour chacune des propriétés de produit. Nous allons nettoyer ces BoundFields afin que seul le `ProductName`, `UnitPrice`, `UnitsInStock`, et `UnitsOnOrder` BoundFields sont affichés. N’hésitez pas à ajouter tous les paramètres au niveau du champ à la BoundFields restantes (telles que la mise en forme le `UnitPrice` sous forme de devise). Après avoir apporté ces modifications, balisage déclaratif de GridView doit ressembler à ce qui suit :


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample1.aspx)]

À ce stade, nous avons un rapport maître/détail entièrement fonctionnel qui affiche le nom, le prix unitaire, unités en stock et des unités de commande pour les produits qui appartiennent à la catégorie sélectionnée.


[![Obtenir la valeur du paramètre categoryID dans la liste de catégories sélectionné DropDownList](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image25.png)

**Figure 9**: obtenir le *`categoryID`* valeur du paramètre dans la liste de catégories sélectionné DropDownList ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image27.png))


## <a name="step-2-displaying-a-footer-in-the-gridview"></a>Étape 2 : Afficher un pied de page dans le contrôle GridView

Le contrôle GridView peut afficher un en-tête et le pied de page de ligne. Ces lignes sont affichées en fonction des valeurs de la `ShowHeader` et `ShowFooter` propriétés, respectivement, avec `ShowHeader` pris par défaut `true` et `ShowFooter` à `false`. Pour inclure un pied de page dans le contrôle GridView simplement définie son `ShowFooter` propriété `true`.


[![ShowFooter propriété le contrôle GridView la valeur true](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image28.png)

**Figure 10**: définir le GridView `ShowFooter` propriété `true` ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image30.png))


La ligne de pied de page comporte une cellule pour chacun des champs définis dans le contrôle GridView ; Toutefois, ces cellules sont vides par défaut. Prenez un moment pour consulter notre progression dans un navigateur. Avec le `ShowFooter` propriété maintenant la valeur `true`, le contrôle GridView inclut une ligne de pied de page vide.


[![Le GridView comprend désormais une ligne de pied de page](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image31.png)

**Figure 11**: le contrôle GridView inclut désormais une ligne de pied de page ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image33.png))


La ligne de pied de page dans la Figure 11 ne se distinguent, car elle a un arrière-plan blanc. Nous allons créer un `FooterStyle` de classe CSS dans `Styles.css` qui spécifie un arrière-plan rouge foncé, puis configurez le `GridView.skin` fichier d’apparence dans le `DataWebControls` thème pour affecter cette classe CSS pour le contrôle GridView `FooterStyle`de `CssClass` propriété. Si vous souhaitez rafraîchir vos connaissances sur les apparences et thèmes, revenir à la [affichant les données avec ObjectDataSource the](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) didacticiel.

Commencez par ajouter la classe CSS suivante à `Styles.css`:


[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample2.css)]

Le `FooterStyle` classe CSS est similaire dans un style à la `HeaderStyle` classe, bien que le `HeaderStyle`de couleur d’arrière-plan est subtilité plus sombre et son texte s’affiche dans une police en gras. En outre, le texte du pied de page est aligné à droite alors que le texte de l’en-tête est centré.

Ensuite, pour associer cette classe CSS à pied de page de chaque contrôle GridView, ouvrez le `GridView.skin` de fichiers dans le `DataWebControls` thème et définissez le `FooterStyle`de `CssClass` propriété. Après cet ajout balisage du fichier doit ressembler à :


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample3.aspx)]

Comme la capture d’écran ci-dessous montre, cette modification rend le pied de page de ressortir plus clairement.


[![Ligne de pied de page du GridView a maintenant une couleur d’arrière-plan tirent](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image34.png)

**Figure 12**: ligne de pied de page du GridView a maintenant une couleur d’arrière-plan tirent ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image36.png))


## <a name="step-3-computing-the-summary-data"></a>Étape 3 : Calcul des données de synthèse

Avec pied de page du GridView affiché, le défi suivant nos consiste à calculer les données de synthèse. Il existe deux façons pour calculer ces informations d’agrégation :

1. Via une requête SQL, nous aurions pu émettre une requête supplémentaire à la base de données pour calculer les données de synthèse pour une catégorie particulière. SQL inclut un nombre de fonctions d’agrégation avec un `GROUP BY` clause pour spécifier les données sur laquelle les données doivent être résumées. La requête SQL suivante serait ramener les informations nécessaires :  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample4.sql)]

    Bien sûr vous ne voudriez d’émettre cette requête directement à partir de la `SummaryDataInFooter.aspx` page, mais plutôt par la création d’une méthode dans le `ProductsTableAdapter` et `ProductsBLL`.
2. Ces informations de calcul comme il est ajouté au GridView comme indiqué dans [mise en forme en fonction lors de données personnalisées](custom-formatting-based-upon-data-cs.md) de (didacticiel), le contrôle GridView `RowDataBound` Gestionnaire d’événements est déclenché pour chaque ligne ajoutée au GridView après son été lié aux données. En créant un gestionnaire d’événements pour cet événement, nous pouvons conserver en cours d’exécution total des valeurs que nous voulons à agréger. Après la dernière ligne de données a été liée au GridView que nous avons les totaux et les informations nécessaires pour calculer la moyenne.

J’utilise généralement la deuxième approche car elle enregistre un voyage à la base de données et l’effort nécessaire pour implémenter les fonctionnalités de synthèse dans la couche d’accès aux données et la couche de logique métier, mais chacune de ces approches suffirait. Pour ce didacticiel nous allons utiliser la deuxième option et de suivre le total en cours d’exécution à l’aide du `RowDataBound` Gestionnaire d’événements.

Créer un `RowDataBound` Gestionnaire d’événements pour le contrôle GridView en sélectionnant le contrôle GridView dans le concepteur, en cliquant sur l’icône d’éclair à partir de la fenêtre Propriétés et en double-cliquant sur le `RowDataBound` événement. Cela créera un nouveau gestionnaire d’événements nommé `ProductsInCategory_RowDataBound` dans la `SummaryDataInFooter.aspx` classe code-behind de la page.


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample5.cs)]

Pour gérer un cumul que nous devons définir des variables en dehors de la portée du Gestionnaire d’événements. Créez les quatre variables au niveau des pages suivantes :

- `_totalUnitPrice`, de type `decimal`
- `_totalNonNullUnitPriceCount`, de type `int`
- `_totalUnitsInStock`, de type `int`
- `_totalUnitsOnOrder`, de type `int`

Ensuite, écrivez le code pour incrémenter ces trois variables pour chaque ligne de données a rencontré dans le `RowDataBound` Gestionnaire d’événements.


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample6.cs)]

Le `RowDataBound` Gestionnaire d’événements démarre en veillant à ce que nous avons affaire à un DataRow. Une fois qu’est établie, le `Northwind.ProductsRow` instance qui a été liée seulement à la `GridViewRow` dans l’objet `e.Row` est stocké dans la variable `product`. Ensuite, en cours d’exécution variables total sont incrémentées par les valeurs correspondantes du produit en cours (en supposant qu’ils ne contiennent pas une base de données `NULL` valeur). Nous assurer le suivi de l’exécution de ces deux `UnitPrice` total et le nombre de non -`NULL` `UnitPrice` enregistre, car le prix moyen est le quotient de ces deux nombres.

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>Étape 4 : Afficher les données de synthèse dans le pied de page

Avec les données de synthèse totalisées, la dernière étape consiste à afficher dans la ligne de pied de page du GridView. Cette tâche, trop, peut être réalisée par programme via le `RowDataBound` Gestionnaire d’événements. N’oubliez pas que le `RowDataBound` Gestionnaire d’événements se déclenche pour *chaque* ligne qui est lié au GridView, y compris la ligne de pied de page. Par conséquent, nous pouvons augmenter notre gestionnaire d’événements pour afficher les données dans la ligne de pied de page en utilisant le code suivant :


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample7.cs)]

Dans la mesure où la ligne de pied de page est ajoutée au GridView une fois que toutes les lignes de données ont été ajoutées, nous pouvons être convaincus qu’au moment où nous sommes prêts à afficher les données de synthèse dans le pied de page que les calculs en cours d’exécution total seront terminées. La dernière étape, est ensuite, pour définir ces valeurs dans les cellules du pied de page.

Pour afficher le texte dans une cellule de pied de page particulière, utilisez `e.Row.Cells[index].Text = value`, où le `Cells` indexation démarre à 0. Le code suivant calcule le prix moyen (le prix total divisé par le nombre de produits) et l’affiche, ainsi que le nombre total d’unités en stock et unités commandées dans les cellules de pied de page approprié du contrôle GridView.


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample8.cs)]

Figure 13 montre le rapport une fois ce code a été ajouté. Notez comment la `ToString("c")` entraîne les prix moyen des informations de résumé à mettre en forme comme une devise.


[![Ligne de pied de page du GridView a maintenant une couleur d’arrière-plan tirent](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image37.png)

**Figure 13**: ligne de pied de page du GridView a maintenant une couleur d’arrière-plan tirent ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image39.png))


## <a name="summary"></a>Récapitulatif

Afficher les données résumées est une exigence courante de rapport et le contrôle GridView rend facile d’inclure ces informations dans sa ligne de pied de page. La ligne de pied de page s’affiche lorsque le GridView `ShowFooter` propriété est définie sur `true` et peut avoir le texte dans ses cellules définies par programme via le `RowDataBound` Gestionnaire d’événements. Les données de synthèse de l’informatique soit faire en interrogeant de nouveau la base de données ou à l’aide de code dans la classe de code-behind de la page ASP.NET pour les données de synthèse de calcul par programmation.

Ce didacticiel conclut notre examen de la mise en forme personnalisée avec les contrôles GridView, DetailsView et FormView. Le didacticiel suivant lance notre exploration de l’insertion, la mise à jour et suppression des données à l’aide de ces mêmes contrôles.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](using-the-formview-s-templates-cs.md)
> [Suivant](custom-formatting-based-upon-data-vb.md)
