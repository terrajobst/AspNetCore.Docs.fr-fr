---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
title: Mise en forme personnalisée en fonction des données (c#) | Microsoft Docs
author: rick-anderson
description: Ajustement du format de la GridView, DetailsView ou FormView basé sur les données liées à l’il est possible de plusieurs façons. Dans ce didacticiel, nous allons l...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 871a4574-f89c-4214-b786-79253ed3653b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 505081890ac0eb7a1f248569fda1b14351119275
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397982"
---
<a name="custom-formatting-based-upon-data-c"></a>Mise en forme personnalisée en fonction des données (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_11_CS.exe) ou [télécharger le PDF](custom-formatting-based-upon-data-cs/_static/datatutorial11cs1.pdf)

> Ajustement du format de la GridView, DetailsView ou FormView basé sur les données liées à l’il est possible de plusieurs façons. Dans ce didacticiel, nous allons examiner comment accomplir lié aux données mise en forme en utilisant les gestionnaires d’événements DataBound et RowDataBound.


## <a name="introduction"></a>Introduction

L’apparence des contrôles GridView, DetailsView et FormView pouvant être personnalisé via une multitude de propriétés associées au style. Propriétés, telles que `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`, et `Height`, entre autres, déterminent l’apparence générale du contrôle rendu. Propriétés, y compris `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`, et d’autres autoriser ces mêmes paramètres de style à appliquer à des sections particulières. De même, ces paramètres de style peuvent être appliqués au niveau du champ.

Dans de nombreux scénarios, les exigences de mise en forme dépendent de la valeur des données affichées. Par exemple, pour attirer l’attention pour out de stock des produits, un rapport répertoriant des informations sur le produit peut définir la couleur d’arrière-plan sur le jaune pour les produits dont la propriété `UnitsInStock` et `UnitsOnOrder` champs sont toutes deux égales à 0. Pour mettre en évidence les produits les plus chers, nous souhaiterons peut-être afficher les prix de ces produits d’évaluation des coûts de plus de 75,00 $ en caractères gras.

Ajustement du format de la GridView, DetailsView ou FormView basé sur les données liées à l’il est possible de plusieurs façons. Dans ce didacticiel, nous allons examiner comment accomplir lié aux données mise en forme à l’aide de la `DataBound` et `RowDataBound` gestionnaires d’événements. Dans le didacticiel suivant, nous allons Explorer une approche alternative.

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>À l’aide du contrôle DetailsView`DataBound`Gestionnaire d’événements

Lorsque les données sont liées à un contrôle DetailsView, à partir d’un contrôle de source de données ou par le biais de par programmation les données du contrôle `DataSource` propriété et en appelant son `DataBind()` (méthode), la séquence suivante d’étapes se produit :

1. Les données contrôle Web `DataBinding` événement est déclenché.
2. Les données sont liées aux données de contrôle Web.
3. Les données contrôle Web `DataBound` événement est déclenché.

Logique personnalisée peut être injectée immédiatement après les étapes 1 et 3 via un gestionnaire d’événements. En créant un gestionnaire d’événements pour le `DataBound` événement, nous pouvons déterminer par programmation les données qui a été lié au contrôle de Web de données et ajuster la mise en forme en fonction des besoins. Pour illustrer cela, nous allons créer un contrôle DetailsView qui répertorie des informations générales sur un produit, mais affichera le `UnitPrice` valeur dans un ***police en gras, italique*** si elle dépasse 75,00 $.

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>Étape 1 : Afficher les informations de produit dans un contrôle DetailsView

Ouvrir le `CustomColors.aspx` page dans le `CustomFormatting` dossier, faites glisser un contrôle DetailsView à partir de la boîte à outils vers le concepteur, définissez son `ID` valeur de propriété à `ExpensiveProductsPriceInBoldItalic`et le lier à un nouveau contrôle ObjectDataSource qui appelle le `ProductsBLL` la classe `GetProducts()` (méthode). Les étapes détaillées pour ce faire sont omis ici par souci de concision, dans la mesure où nous les examiné en détail dans les didacticiels précédents.

Une fois que vous avez lié à ObjectDataSource pour le contrôle DetailsView, prenez un moment pour modifier la liste de champs. J’ai choisi de supprimer le `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, et `Discontinued` BoundFields et renommé et reformaté le BoundFields restantes. J’ai également effacés le `Width` et `Height` paramètres. Étant donné que le contrôle DetailsView affiche un seul enregistrement, nous devons activer la pagination afin de permettre à l’utilisateur final afficher tous les produits. Faire en cochant la case Activer la pagination dans la balise active de DetailsView.


[![La case Activer la pagination dans la balise active de DetailsView](custom-formatting-based-upon-data-cs/_static/image2.png)](custom-formatting-based-upon-data-cs/_static/image1.png)

**Figure 1**: case à cocher Activer la pagination dans la balise active de DetailsView ([cliquez pour afficher l’image en taille réelle](custom-formatting-based-upon-data-cs/_static/image3.png))


Après ces modifications, le balisage de DetailsView sera :


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample1.aspx)]

Prenez un moment pour tester cette page dans votre navigateur.


[![Le contrôle DetailsView affiche un seul produit à la fois](custom-formatting-based-upon-data-cs/_static/image5.png)](custom-formatting-based-upon-data-cs/_static/image4.png)

**Figure 2**: le contrôle DetailsView contrôle affiche un seul produit à la fois ([cliquez pour afficher l’image en taille réelle](custom-formatting-based-upon-data-cs/_static/image6.png))


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Étape 2 : Déterminer par programmation la valeur des données dans le Gestionnaire d’événement DataBound

Pour afficher le prix dans une police en gras, italique pour les produits dont `UnitPrice` valeur dépasse $75,00, nous devons tout d’abord être en mesure de déterminer par programme le `UnitPrice` valeur. Pour le contrôle DetailsView, cela peut être accompli dans le `DataBound` Gestionnaire d’événements. Pour créer l’événement Gestionnaire cliquez sur le contrôle DetailsView dans le concepteur, puis accédez à la fenêtre Propriétés. Appuyez sur F4 pour afficher, si elle n’est pas visible, ou accédez au menu Affichage et sélectionnez l’option de menu de la fenêtre Propriétés. Dans la fenêtre Propriétés, cliquez sur l’icône d’éclair pour répertorier les événements de DetailsView. Ensuite, double-cliquez sur le `DataBound` événement ou tapez le nom de gestionnaire d’événements que vous souhaitez créer.


![Créer un gestionnaire d’événements pour l’événement DataBound](custom-formatting-based-upon-data-cs/_static/image7.png)

**Figure 3**: créer un gestionnaire d’événements pour le `DataBound` événement


Cela sera automatiquement créer le Gestionnaire d’événements et d’accéder à la partie du code dans lequel elle a été ajoutée. À ce stade vous verrez :


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample2.cs)]

Les données liées au contrôle DetailsView est accessible via la `DataItem` propriété. Rappelez-vous que nous effectuons une liaison nos contrôles à un DataTable fortement typée, qui est composé d’une collection d’instances de DataRow fortement typée. Lorsque la table de données est liée à DetailsView, le premier DataRow du DataTable est affecté à la DetailsView `DataItem` propriété. Plus précisément, le `DataItem` est affectée à un `DataRowView` objet. Nous pouvons utiliser le `DataRowView`de `Row` propriété pour accéder à l’objet DataRow sous-jacent, qui est en fait une `ProductsRow` instance. Une fois que nous avons ça `ProductsRow` instance nous pouvons rendre notre décision en examinant simplement les valeurs de propriété de l’objet.

Le code suivant illustre comment déterminer si le `UnitPrice` valeur liée au contrôle DetailsView est supérieure à $75,00 :


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample3.cs)]

> [!NOTE]
> Dans la mesure où `UnitPrice` peut avoir un `NULL` valeur dans la base de données, nous vérifions d’abord vous assurer que nous n’avons pas affaire à un `NULL` valeur avant d’accéder à la `ProductsRow`de `UnitPrice` propriété. Cette vérification est importante, car si nous tentons d’accéder à la `UnitPrice` propriété lorsqu’il a un `NULL` valeur le `ProductsRow` objet lèvera une [StrongTypingException exception](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx).


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>Étape 3 : Mise en forme la valeur de UnitPrice dans le contrôle DetailsView

À ce stade, nous pouvons déterminer si le `UnitPrice` liée à DetailsView a une valeur qui dépasse $75,00, mais nous avons encore pour voir comment ajuster par programmation le contrôle DetailsView de mise en forme en conséquence. Pour modifier la mise en forme d’une ligne entière dans le contrôle DetailsView, accéder par programme à la ligne à l’aide `DetailsViewID.Rows[index]`; pour modifier une cellule particulière, utilisation de l’accès `DetailsViewID.Rows[index].Cells[index]`. Une fois que nous avons une référence à la ligne ou nous pouvons ensuite adapter son apparence en définissant ses propriétés associées au style de cellule.

L’accès à une ligne par programme, vous devez l’index de la ligne, qui démarre à 0. Le `UnitPrice` ligne est la cinquième ligne dans le contrôle DetailsView, en lui attribuant un index de 4 et le rendre accessible par programmation à l’aide de `ExpensiveProductsPriceInBoldItalic.Rows[4]`. À ce stade, nous pourrions avoir du contenu de la ligne entière affiché dans une police en gras, italique, en utilisant le code suivant :


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample4.cs)]

Toutefois, cela rendra *à la fois* l’étiquette (prix) et la valeur en gras et italique. Si nous voulons simplement la valeur en gras et italique nous devons appliquer la mise en forme à la deuxième cellule de la ligne, ce qui peut être effectuée à l’aide de ce qui suit :


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample5.cs)]

Étant donné que nos didacticiels ont jusqu'à présent utilisé des feuilles de style pour maintenir une séparation nette entre le balisage rendu et les informations relatives au style, au lieu de définir les propriétés de style spécifique, comme indiqué ci-dessus nous allons à la place, utilisez une classe CSS. Ouvrez le `Styles.css` feuille de style et ajoutez une nouvelle classe CSS nommée `ExpensivePriceEmphasis` avec la définition suivante :


[!code-css[Main](custom-formatting-based-upon-data-cs/samples/sample6.css)]

Ensuite, dans le `DataBound` Gestionnaire d’événements, définir la cellule `CssClass` propriété `ExpensivePriceEmphasis`. Le code suivant illustre la `DataBound` Gestionnaire d’événements dans son intégralité :


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample7.cs)]

Lors de l’affichage Chai, ce qui coûte moins de 75,00 $, le prix est affiché dans une police normale (voir Figure 4). Toutefois, lors de l’affichage Mishi Kobe Niku, qui a un prix de $97.00, le prix est affiché dans une police en gras, italique (voir Figure 5).


[![Les prix inférieur à $75,00 sont affichés dans une police normale](custom-formatting-based-upon-data-cs/_static/image9.png)](custom-formatting-based-upon-data-cs/_static/image8.png)

**Figure 4**: prix inférieur à $75,00 sont affichés dans une police normale ([cliquez pour afficher l’image en taille réelle](custom-formatting-based-upon-data-cs/_static/image10.png))


[![Les prix des produits chers sont affichés dans un gras, italique police](custom-formatting-based-upon-data-cs/_static/image12.png)](custom-formatting-based-upon-data-cs/_static/image11.png)

**Figure 5**: les prix des produits chers sont affichés dans un gras, italique police ([cliquez pour afficher l’image en taille réelle](custom-formatting-based-upon-data-cs/_static/image13.png))


## <a name="using-the-formview-controlsdataboundevent-handler"></a>À l’aide du contrôle FormView`DataBound`Gestionnaire d’événements

Les étapes permettant de déterminer les données sous-jacentes liées à un FormView sont identiques à celles pour créer un contrôle DetailsView un `DataBound` effectuer un cast du Gestionnaire d’événements, le `DataItem` propriété sur le type d’objet approprié liée au contrôle et déterminer la marche à suivre. FormView et DetailsView diffèrent, toutefois, dans comment apparence de l’interface de leur utilisateur est mise à jour.

Le contrôle FormView ne contient pas de n’importe quel BoundFields et par conséquent ne dispose pas de la `Rows` collection. Au lieu de cela, un FormView est composé de modèles, qui peuvent contenir un mélange de code HTML statique, les contrôles Web et la syntaxe de liaison de données. Ajuster le style d’un FormView généralement implique d’ajuster le style d’un ou plusieurs des contrôles Web au sein des modèles de FormView.

Pour illustrer cela, nous allons utiliser un FormView aux produits de la liste comme dans l’exemple précédent, mais cette fois, nous allons afficher seulement le nom de produit et les unités en stock avec les unités en stock affiché en rouge si elle est inférieure ou égale à 10.

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>Étape 4 : Afficher les informations de produit dans un FormView

Ajoute un FormView à la `CustomColors.aspx` page sous le contrôle DetailsView et un ensemble son `ID` propriété `LowStockedProductsInRed`. Lier le contrôle FormView pour le contrôle ObjectDataSource créé à partir de l’étape précédente. Cela créera un `ItemTemplate`, `EditItemTemplate`, et `InsertItemTemplate` pour le contrôle FormView. Supprimer le `EditItemTemplate` et `InsertItemTemplate` et simplifier le `ItemTemplate` pour inclure uniquement le `ProductName` et `UnitsInStock` des valeurs, chacune d’elles dans leurs propres contrôles Label nommé de manière appropriée. Comme avec le contrôle DetailsView à partir de l’exemple précédent, également la case Activer la pagination dans la balise active du FormView.

Après ces modifications balisage de votre FormView doit ressembler à ce qui suit :


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample8.aspx)]

Notez que le `ItemTemplate` contient :

- **HTML statique** le texte « produit : » et « unités en Stock : » avec le `<br />` et `<b>` éléments.
- **Contrôles Web** les deux contrôles Label, `ProductNameLabel` et `UnitsInStockLabel`.
- **Syntaxe de liaison de données** le `<%# Bind("ProductName") %>` et `<%# Bind("UnitsInStock") %>` syntaxe, ce qui affecte les valeurs de ces champs pour les contrôles étiquette `Text` propriétés.

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Étape 5 : Déterminer par programmation la valeur des données dans le Gestionnaire d’événement DataBound

Avec le balisage de FormView terminé, l’étape suivante consiste à déterminer par programmation si le `UnitsInStock` valeur est inférieure ou égale à 10. Cela s’effectue dans la même manière avec le contrôle FormView telle qu’elle était avec le contrôle DetailsView. Commencez par créer un gestionnaire d’événements pour le FormView `DataBound` événement.


![Créer le Gestionnaire d’événement DataBound](custom-formatting-based-upon-data-cs/_static/image14.png)

**Figure 6**: créer le `DataBound` Gestionnaire d’événements


Dans l’événement Gestionnaire d’effectuer un cast de FormView `DataItem` propriété à un `ProductsRow` de l’instance et de déterminer si le `UnitsInPrice` valeur est telle que nous avons besoin pour l’afficher dans une police rouge.


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample9.cs)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>Étape 6 : Mise en forme le contrôle d’étiquette de UnitsInStockLabel dans ItemTemplate de FormView

L’étape finale consiste à mettre en forme le texte affiché `UnitsInStock` valeur dans une police rouge si la valeur est inférieure ou égal à 10. Pour cela nous devons accéder par programme à la `UnitsInStockLabel` dans contrôler le `ItemTemplate` et définissez ses propriétés de style afin que son texte s’affiche en rouge. Pour accéder à un contrôle Web dans un modèle, utilisez la `FindControl("controlID")` méthode comme suit :


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample10.cs)]

Dans notre exemple, nous souhaitons accéder à une étiquette de contrôle dont `ID` valeur est `UnitsInStockLabel`, donc nous utiliserions :


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample11.cs)]

Une fois que nous avons une référence de programmation pour le contrôle Web, nous pouvons modifier ses propriétés associées au style, en fonction des besoins. Comme avec l’exemple précédent, j’ai créé une classe CSS dans `Styles.css` nommé `LowUnitsInStockEmphasis`. Pour appliquer ce style au contrôle Web Label, définissez son `CssClass` propriété en conséquence.


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample12.cs)]

> [!NOTE]
> La syntaxe de mise en forme d’un modèle d’accès par programme au contrôle Web en utilisant `FindControl("controlID")` et puis en définissant ses propriétés associées au style peut également être utilisé lors de l’utilisation [TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) dans le contrôle DetailsView ou GridView contrôles. Nous allons examiner TemplateField dans notre didacticiel suivant.


Figures 7 montre le contrôle FormView lors de l’affichage d’un produit dont `UnitsInStock` valeur est supérieure à 10, alors que le produit dans la Figure 8 a sa valeur inférieure à 10.


[![Pour les produits avec un suffisamment grandes unités en Stock, personnalisé de non mise en forme est appliquée](custom-formatting-based-upon-data-cs/_static/image16.png)](custom-formatting-based-upon-data-cs/_static/image15.png)

**Figure 7**: pour les produits avec un suffisamment grandes Units In Stock, aucune mise en forme de personnalisé est appliqué ([cliquez pour afficher l’image en taille réelle](custom-formatting-based-upon-data-cs/_static/image17.png))


[![Les unités en Stock nombre est indiqué en rouge pour les produits avec des valeurs de 10 ou moins](custom-formatting-based-upon-data-cs/_static/image19.png)](custom-formatting-based-upon-data-cs/_static/image18.png)

**Figure 8**: les unités dans le numéro de Stock est affichée en rouge pour les produits avec des valeurs de 10 ou moins ([cliquez pour afficher l’image en taille réelle](custom-formatting-based-upon-data-cs/_static/image20.png))


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>Mise en forme avec le GridView`RowDataBound`événement

Précédemment, nous avons examiné la séquence d’étapes, le contrôle DetailsView et FormView contrôle la progression via pendant la liaison de données. Examinons ces étapes qu’une seule fois à nouveau en tant qu’un rappel.

1. Les données contrôle Web `DataBinding` événement est déclenché.
2. Les données sont liées aux données de contrôle Web.
3. Les données contrôle Web `DataBound` événement est déclenché.

Ces trois étapes simples suffisent pour le contrôle DetailsView et FormView parce qu’ils affichent un seul enregistrement. Pour le contrôle GridView, qui affiche *tous les* enregistrements liés à celui-ci (et pas seulement le premier), l’étape 2 est un peu plus complexe.

À l’étape 2, le contrôle GridView énumère la source de données et, pour chaque enregistrement, crée un `GridViewRow` de l’instance et lie l’enregistrement en cours à celle-ci. Pour chaque `GridViewRow` ajouté au GridView, deux événements sont déclenchés :

- **`RowCreated`** se déclenche après la `GridViewRow` a été créé.
- **`RowDataBound`** se déclenche après l’enregistrement en cours a été liée à la `GridViewRow`.

Ensuite, pour le contrôle GridView, liaison de données est décrit plus précisément par la séquence d’étapes suivante :

1. Le contrôle GridView `DataBinding` événement est déclenché.
2. Les données sont liées au GridView.   
  
   Pour chaque enregistrement dans la source de données 

    1. Créer un `GridViewRow` objet
    2. Déclencher la `RowCreated` événement
    3. Lier l’enregistrement à la `GridViewRow`
    4. Déclencher la `RowDataBound` événement
    5. Ajouter le `GridViewRow` à la `Rows` collection
3. Le contrôle GridView `DataBound` événement est déclenché.

Pour personnaliser le format des enregistrements individuels de GridView, ensuite, nous devons créer un gestionnaire d’événements pour le `RowDataBound` événement. Pour illustrer cela, nous allons ajouter un contrôle GridView à la `CustomColors.aspx` page qui répertorie le nom, la catégorie et le prix de chaque produit, la mise en surbrillance les produits dont le prix est inférieur à $10,00 avec une couleur d’arrière-plan jaune.

## <a name="step-7-displaying-product-information-in-a-gridview"></a>Étape 7 : Affichage des informations sur les produits dans un GridView

Ajoutez un GridView sous le contrôle FormView à partir de l’exemple précédent et définissez son `ID` propriété `HighlightCheapProducts`. Étant donné que nous avons déjà un ObjectDataSource qui retourne tous les produits sur la page, lier le contrôle GridView à qui. Enfin, modifiez BoundFields le contrôle GridView pour inclure uniquement leurs noms, catégories et prix. Après ces modifications balisage de la GridView doit ressembler à :


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample13.aspx)]

La figure 9 illustre notre progression jusqu'à présent lorsqu’ils sont affichés via un navigateur.


[![Le contrôle GridView répertorie le nom, la catégorie et le prix de chaque produit](custom-formatting-based-upon-data-cs/_static/image22.png)](custom-formatting-based-upon-data-cs/_static/image21.png)

**Figure 9**: le contrôle GridView répertorie le nom, catégorie et prix pour chaque produit ([cliquez pour afficher l’image en taille réelle](custom-formatting-based-upon-data-cs/_static/image23.png))


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>Étape 8 : Déterminer par programmation la valeur des données dans le Gestionnaire d’événement RowDataBound

Lorsque le `ProductsDataTable` est lié au GridView son `ProductsRow` instances sont énumérés et pour chaque `ProductsRow` un `GridViewRow` est créé. Le `GridViewRow`de `DataItem` propriété est affectée à la particulier `ProductRow`, après laquelle le GridView `RowDataBound` Gestionnaire d’événements est déclenché. Pour déterminer le `UnitPrice` valeur pour chaque produit lié au GridView, puis, nous devons créer un gestionnaire d’événements pour le contrôle GridView `RowDataBound` événement. Dans ce gestionnaire d’événements, nous pouvons examiner la `UnitPrice` valeur actif `GridViewRow` et prendre une décision de mise en forme pour cette ligne.

Ce gestionnaire d’événements peut être créé à l’aide de la même série d’étapes comme avec les FormView et DetailsView.


![Créer un gestionnaire d’événements pour l’événement de RowDataBound du contrôle GridView](custom-formatting-based-upon-data-cs/_static/image24.png)

**Figure 10**: créer un gestionnaire d’événements pour le contrôle GridView `RowDataBound` événement


Création du Gestionnaire d’événements de cette manière entraîne le code suivant à être automatiquement ajoutés à la partie du code de la page ASP.NET :


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample14.cs)]

Lorsque le `RowDataBound` événement est déclenché, le Gestionnaire d’événements est passé en tant que son deuxième paramètre un objet de type `GridViewRowEventArgs`, ce qui a une propriété nommée `Row`. Cette propriété retourne une référence à la `GridViewRow` qui a été liées uniquement les données. Pour accéder à la `ProductsRow` instance liée à la `GridViewRow` , nous utilisons le `DataItem` propriété comme suit :


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample15.cs)]

Lorsque vous travaillez avec le `RowDataBound` Gestionnaire d’événements qu’il est important de garder à l’esprit que le contrôle GridView se compose de différents types de lignes et que cet événement est déclenché pour *tous les* ligne types. Un `GridViewRow`de type peut être déterminé par ses `RowType` propriété et peut prendre l’une des valeurs possibles :

- `DataRow` une ligne qui est liée à un enregistrement de la GridView `DataSource`
- `EmptyDataRow` la ligne affichée si le GridView `DataSource` est vide
- `Footer` la ligne de pied de page ; ci-dessus si le GridView `ShowFooter` propriété est définie sur `true`
- `Header` la ligne d’en-tête ; affiche si la propriété de ShowHeader le contrôle GridView est définie `true` (la valeur par défaut)
- `Pager` pour le contrôle GridView qui implémentent la pagination, la ligne qui affiche l’interface de pagination
- `Separator` ne pas utilisé pour le contrôle GridView, mais par le `RowType` contrôle les propriétés pour les contrôles DataList et Repeater, données de deux contrôles Web que nous aborderons dans les futures didacticiels

Dans la mesure où le `EmptyDataRow`, `Header`, `Footer`, et `Pager` lignes ne sont pas associées à un `DataSource` enregistrements, ils ont toujours un `null` valeur pour leurs `DataItem` propriété. Pour cette raison, avant d’essayer de travailler avec actuel `GridViewRow`de `DataItem` propriété, nous devons Assurez-vous d’abord que nous avons affaire à un `DataRow`. Cela est possible en vérifiant la `GridViewRow`de `RowType` propriété comme suit :


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample16.cs)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>Étape 9 : Mise en surbrillance la ligne jaune lorsque la valeur UnitPrice est inférieur à $10,00

La dernière étape consiste à mettre en évidence par programmation de l’ensemble du `GridViewRow` si le `UnitPrice` valeur de cette ligne est inférieur à $10,00. La syntaxe pour accéder à un contrôle GridView lignes ou cellules est la même façon que le contrôle DetailsView `GridViewID.Rows[index]` pour accéder à la ligne entière, `GridViewID.Rows[index].Cells[index]` pour accéder à une cellule particulière. Toutefois, lorsque le `RowDataBound` Gestionnaire d’événements déclenche à la limite de données `GridViewRow` doit encore être ajouté à la GridView `Rows` collection. Par conséquent, vous ne pouvez pas accéder actuel `GridViewRow` instance à partir de la `RowDataBound` Gestionnaire d’événements à l’aide de la collection de lignes.

Au lieu de `GridViewID.Rows[index]`, nous pouvons référencer actuel `GridViewRow` d’instance dans le `RowDataBound` Gestionnaire d’événements à l’aide `e.Row`. Autrement dit, dans l’ordre pour mettre en surbrillance actuel `GridViewRow` instance à partir de la `RowDataBound` nous utiliserions Gestionnaire d’événements :


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample17.cs)]

Au lieu de définir la `GridViewRow`de `BackColor` propriété directement, conservez-le à l’aide des classes CSS. J’ai créé une classe CSS nommée `AffordablePriceEmphasis` qui définit la couleur d’arrière-plan jaune. Fin du `RowDataBound` Gestionnaire d’événements suit :


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample18.cs)]


[![Les produits les plus abordable sont mis en surbrillance en jaune](custom-formatting-based-upon-data-cs/_static/image26.png)](custom-formatting-based-upon-data-cs/_static/image25.png)

**Figure 11**: produits abordable The Most sont mis en surbrillance en jaune ([cliquez pour afficher l’image en taille réelle](custom-formatting-based-upon-data-cs/_static/image27.png))


## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons vu comment mettre en forme le GridView, DetailsView et FormView basé sur les données liées au contrôle. Pour cela que nous avons créé un gestionnaire d’événements pour le `DataBound` ou `RowDataBound` événements, où les données sous-jacentes a été examinées, ainsi que d’une mise en forme changent, si nécessaire. Pour accéder aux données liées à un contrôle DetailsView ou les FormView, nous utilisons le `DataItem` propriété dans le `DataBound` Gestionnaire d’événements ; pour un GridView, chaque `GridViewRow` l’instance `DataItem` propriété contient les données liées à cette ligne, qui est disponible dans le `RowDataBound` Gestionnaire d’événements.

La syntaxe de réglage par programmation de la mise en forme du contrôle Web de données varie selon le contrôle Web et la façon dont les données à mettre en forme s’affiche. Pour le contrôle DetailsView et GridView contrôles, les lignes et les cellules accessibles par index ordinal. Pour le contrôle FormView, qui utilise des modèles, le `FindControl("controlID")` méthode est couramment utilisée pour localiser un contrôle Web à partir du modèle.

Dans le didacticiel suivant, nous allons examiner comment utiliser les modèles des contrôles GridView et DetailsView. En outre, nous verrons une autre technique pour la personnalisation de la mise en forme en fonction des données sous-jacentes.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été E.R. Gillain, Dennis Patterson et Dan Jagers. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](using-templatefields-in-the-gridview-control-cs.md)
