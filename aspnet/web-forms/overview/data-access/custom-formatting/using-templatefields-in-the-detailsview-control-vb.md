---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
title: Utilisation de TemplateField dans le contrôle DetailsView (VB) | Microsoft Docs
author: rick-anderson
description: Les mêmes fonctionnalités TemplateField disponibles avec le contrôle GridView sont également disponibles avec le contrôle DetailsView. Dans ce didacticiel, nous affichons un produit...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 0b91d5f8-127d-4f6a-b204-f2e2b35ef703
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 1c18e8be25369d6e7d1703e71b80e75adb9f15fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828291"
---
<a name="using-templatefields-in-the-detailsview-control-vb"></a>Utilisation de TemplateField dans le contrôle DetailsView (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_13_VB.exe) ou [télécharger le PDF](using-templatefields-in-the-detailsview-control-vb/_static/datatutorial13vb1.pdf)

> Les mêmes fonctionnalités TemplateField disponibles avec le contrôle GridView sont également disponibles avec le contrôle DetailsView. Dans ce didacticiel, nous affichons un produit à la fois à l’aide d’un contrôle DetailsView contenant TemplateField.


## <a name="introduction"></a>Introduction

Le TemplateField contenu offre un degré plus élevé de flexibilité dans les données de rendu que le BoundField, CheckBoxField, HyperLinkField et autres contrôles de champ de données. Dans le [didacticiel précédent](using-templatefields-in-the-gridview-control-vb.md) nous avons examiné à l’aide du TemplateField contenu dans un contrôle GridView à :

- Afficher plusieurs valeurs de champ de données dans une colonne. Plus précisément, les deux le `FirstName` et `LastName` champs ont été combinés en une seule colonne de GridView.
- Utilisez un autre contrôle Web pour exprimer une valeur de champ de données. Nous avons vu comment afficher le `HiredDate` valeur à l’aide d’un contrôle calendrier.
- Afficher les informations d’état basées sur les données sous-jacentes. Bien que le `Employees` table ne contient pas une colonne qui retourne le nombre de jours pendant lesquels un employé travaille sur la tâche, nous avons pu afficher ces informations dans l’exemple de GridView dans le didacticiel précédent à l’aide d’un TemplateField et la méthode de mise en forme.

Les mêmes fonctionnalités TemplateField disponibles avec le contrôle GridView sont également disponibles avec le contrôle DetailsView. Dans ce didacticiel, nous affichons un produit à la fois à l’aide d’un contrôle DetailsView qui contient deux TemplateFields. Le TemplateField contenu première combinera le `UnitPrice`, `UnitsInStock`, et `UnitsOnOrder` des champs de données dans une ligne de DetailsView. Le TemplateField contenu deuxième affichera la valeur de la `Discontinued` champ, mais utilisera une méthode de mise en forme pour afficher « Oui » si `Discontinued` est `True`et « NO » dans le cas contraire.


[![Deux TemplateField est utilisés pour personnaliser l’affichage](using-templatefields-in-the-detailsview-control-vb/_static/image2.png)](using-templatefields-in-the-detailsview-control-vb/_static/image1.png)

**Figure 1**: TemplateField deux sont utilisés pour personnaliser l’affichage ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-detailsview-control-vb/_static/image3.png))


C’est parti !

## <a name="step-1-binding-the-data-to-the-detailsview"></a>Étape 1 : Liaison des données pour le contrôle DetailsView

Comme indiqué dans le didacticiel précédent, lorsque vous travaillez avec TemplateField, il est souvent plus facile de commencer par créer le contrôle DetailsView qui contient juste BoundFields puis ajoutez TemplateField nouvelle ou convertir le BoundFields existant TemplateField comme nécessaire . Ainsi, vous pouvez commencer ce didacticiel en ajoutant un contrôle DetailsView à la page via le concepteur et en liant à un ObjectDataSource qui retourne la liste des produits. Ces étapes crée un contrôle DetailsView avec BoundFields pour chacun des champs de valeur non booléenne du produit et un CheckBoxField pour ce champ valeur booléenne (indisponible).

Ouvrez le `DetailsViewTemplateField.aspx` page et faites glisser un contrôle DetailsView à partir de la boîte à outils vers le concepteur. À partir de la balise active de DetailsView choisir d’ajouter un nouveau contrôle ObjectDataSource qui appelle le `ProductsBLL` la classe `GetProducts()` (méthode).


[![Ajouter un nouveau contrôle ObjectDataSource qui appelle la méthode GetProducts()](using-templatefields-in-the-detailsview-control-vb/_static/image5.png)](using-templatefields-in-the-detailsview-control-vb/_static/image4.png)

**Figure 2**: ajouter un nouveau contrôle ObjectDataSource ce Invoke le `GetProducts()` (méthode) ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-detailsview-control-vb/_static/image6.png))


Pour ce rapport, supprimez le `ProductID`, `SupplierID`, `CategoryID`, et `ReorderLevel` BoundFields. Ensuite, réorganiser les BoundFields afin que le `CategoryName` et `SupplierName` BoundFields apparaître immédiatement après le `ProductName` BoundField. N’hésitez pas à ajuster le `HeaderText` propriétés et les propriétés de mise en forme pour le BoundFields comme vous le souhaitez. Comme avec le contrôle GridView, ces modifications au niveau BoundField peuvent être effectuées via la boîte de dialogue champs (accessible en cliquant sur le lien Modifier les champs dans la balise active de DetailsView) ou via la syntaxe déclarative. Enfin, effacez le DetailsView `Height` et `Width` les valeurs de propriété afin de permettre le contrôle DetailsView contrôler pour développer selon les données affichées et cochez la case Activer la pagination dans la balise active.

Après avoir apporté ces modifications, balisage déclaratif de votre contrôle DetailsView doit ressembler à ce qui suit :


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample1.aspx)]

Prenez un moment pour afficher la page via un navigateur. À ce stade devrait un seul produit répertorié (Chai) avec des lignes représentant le nom du produit, catégorie, fournisseur, prix, unités en stock, unités commandées et son état abandonné.


[![Détails du produit sont affichés à l’aide d’une série de BoundFields](using-templatefields-in-the-detailsview-control-vb/_static/image8.png)](using-templatefields-in-the-detailsview-control-vb/_static/image7.png)

**Figure 3**: détails du produit le sont affichés à l’aide d’une série de BoundFields ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-detailsview-control-vb/_static/image9.png))


## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>Étape 2 : Combinant le prix, les unités en Stock et les unités sur l’ordre dans une ligne

Le contrôle DetailsView comporte une ligne pour le `UnitPrice`, `UnitsInStock`, et `UnitsOnOrder` champs. Nous pouvons combiner ces champs de données en une seule ligne avec un TemplateField en ajoutant un nouveau TemplateField ou en convertissant un existants `UnitPrice`, `UnitsInStock`, et `UnitsOnOrder` BoundFields en TemplateField. Alors que je préfère personnellement conversion BoundFields existant, entraînez-vous en ajoutant un nouveau TemplateField.

Commencez par cliquer sur le lien Modifier les champs dans la balise active de DetailsView pour afficher la boîte de dialogue champs. Ensuite, ajoutez un nouveau TemplateField et définissez son `HeaderText` à la propriété « Prix et inventaire » et déplacer le TemplateField contenu nouveau afin qu’il est positionné au-dessus de la `UnitPrice` BoundField.


[![Ajouter un nouveau TemplateField pour le contrôle DetailsView](using-templatefields-in-the-detailsview-control-vb/_static/image11.png)](using-templatefields-in-the-detailsview-control-vb/_static/image10.png)

**Figure 4**: ajouter un nouveau TemplateField pour le contrôle DetailsView ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-detailsview-control-vb/_static/image12.png))


Étant donné que cette nouvelle TemplateField contiendra les valeurs actuellement affichées dans le `UnitPrice`, `UnitsInStock`, et `UnitsOnOrder` BoundFields, nous allons les supprimer.

La dernière tâche pour cette étape consiste à définir le `ItemTemplate` balisage pour le prix et l’inventaire TemplateField, qui peut être effectuée par le biais de DetailsView modification de modèle interface dans le concepteur ou manuellement via la syntaxe déclarative du contrôle. Comme avec le contrôle GridView, interface de modification de modèle de DetailsView est accessible en cliquant sur le lien Modifier les modèles dans la balise active. À partir de là, vous pouvez sélectionner le modèle pour modifier dans la liste déroulante, puis ajoutez tous les contrôles Web à partir de la boîte à outils.

Pour ce didacticiel, commencez par ajouter un contrôle Label et le prix du stock TemplateField `ItemTemplate`. Ensuite, cliquez sur le lien Modifier les DataBindings à partir de la balise active du contrôle Web Label et lier le `Text` propriété le `UnitPrice` champ.


[![Lier la propriété de texte de l’étiquette au champ de données de prix unitaire](using-templatefields-in-the-detailsview-control-vb/_static/image14.png)](using-templatefields-in-the-detailsview-control-vb/_static/image13.png)

**Figure 5**: lier l’étiquette `Text` propriété le `UnitPrice` champ de données ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-detailsview-control-vb/_static/image15.png))


## <a name="formatting-the-price-as-a-currency"></a>Le prix de la mise en forme comme une devise

Ainsi, le contrôle Web Label prix et inventaire TemplateField seront affichent désormais simplement le prix pour le produit sélectionné. Figure 6 présente une capture d’écran de notre progression jusqu'à présent lorsqu’ils sont affichés via un navigateur.


[![Le prix et l’inventaire TemplateField indique le prix](using-templatefields-in-the-detailsview-control-vb/_static/image17.png)](using-templatefields-in-the-detailsview-control-vb/_static/image16.png)

**Figure 6**: le prix et inventaire TemplateField indique le prix ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-detailsview-control-vb/_static/image18.png))


Notez que le prix du produit n’est pas mis en forme comme une devise. Avec un BoundField, mise en forme est possible en définissant le `HtmlEncode` propriété `False` et `DataFormatString` propriété `{0:formatSpecifier}`. Pour un TemplateField, toutefois, les instructions de mise en forme doivent être spécifiées dans la syntaxe de liaison de données ou via l’utilisation d’une méthode de mise en forme défini quelque part dans le code de l’application (comme dans la classe de code-behind de la page ASP.NET).

Pour spécifier la mise en forme pour la syntaxe de liaison de données utilisée dans le contrôle Web Label, retourner à la boîte de dialogue DataBindings en cliquant sur le lien Modifier les DataBindings à partir de la balise active de l’étiquette. Vous pouvez taper les instructions de mise en forme directement dans la liste déroulante Format ou sélectionnez une des chaînes de format définie. Comme avec la BoundField `DataFormatString` propriété, la mise en forme est spécifié à l’aide de `{0:formatSpecifier}`.

Pour le `UnitPrice` champ Utilisation de la mise en forme de devise spécifiée en sélectionnant la valeur approprié dans la liste déroulante ou en tapant dans `{0:C}` manuellement.


[![Mettre en forme le prix sous forme de devise](using-templatefields-in-the-detailsview-control-vb/_static/image20.png)](using-templatefields-in-the-detailsview-control-vb/_static/image19.png)

**Figure 7**: mettre en forme le prix sous forme de devise ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-detailsview-control-vb/_static/image21.png))


De façon déclarative, la spécification de mise en forme est indiquée en tant que deuxième paramètre dans le `Bind` ou `Eval` méthodes. Les paramètres simplement effectuées via le résultat du concepteur dans l’expression de liaison de données suivante dans le balisage déclaratif :


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>Ajouter les champs de données restants pour le TemplateField contenu

À ce stade nous avons affiché et mis en forme le `UnitPrice` données champ dans le prix et l’inventaire TemplateField, mais vous devez toujours afficher le `UnitsInStock` et `UnitsOnOrder` champs. Nous allons les afficher sur une ligne au-dessous du prix et entre parenthèses. À partir de l’interface de modification de modèle dans le concepteur, ce balisage peut être ajouté en plaçant votre curseur dans le modèle et en tapant simplement dans le texte à afficher. Vous pouvez également, ce balisage peut être entré directement dans la syntaxe déclarative.

Ajouter le balisage statique, les contrôles Web de l’étiquette et syntaxe de liaison de données afin que le prix et l’inventaire TemplateField affiche des informations de prix et d’inventaire comme suit :

*prix unitaire*  
(**En Stock / de commande :** *UnitsInStock* / *UnitsOnOrder*)

Après avoir effectué cette tâche balisage déclaratif de votre DetailsView doit ressembler à ce qui suit :


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample3.aspx)]

Avec ces modifications, nous avons consolidé les informations de prix et de stock en une seule ligne de DetailsView.


[![Le prix et les informations d’inventaire s’affiche dans une ligne unique](using-templatefields-in-the-detailsview-control-vb/_static/image23.png)](using-templatefields-in-the-detailsview-control-vb/_static/image22.png)

**Figure 8**: le prix et les informations d’inventaire s’affiche dans une ligne unique ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-detailsview-control-vb/_static/image24.png))


## <a name="step-3-customizing-the-discontinued-field-information"></a>Étape 3 : Personnaliser les informations de champ éponyme a

Le `Products` la table `Discontinued` colonne est une valeur de bit qui indique si le produit a été abandonné. Lorsque vous liez un contrôle DetailsView (ou GridView) à un contrôle de source de données, les champs de valeur booléenne, tels que `Discontinued`, sont implémentés en tant que CheckBoxFields tandis que les champs de valeur non booléenne, tels que `ProductID`, `ProductName`, et ainsi de suite, sont implémentées en tant que BoundFields. Le CheckBoxField rendu sous la forme d’une case à cocher désactivée qui est activée si la valeur du champ de données est True et elle est désactivée dans le cas contraire.

Au lieu d’afficher le CheckBoxField, nous souhaiterons affiche à la place texte indiquant que le produit n’est plus disponible ou non. Pour effectuer cette opération le CheckBoxField dans le contrôle DetailsView et en renforçant les ajoutez ensuite un BoundField dont `DataField` propriété a été définie sur `Discontinued`. Prenez un moment pour ce faire. Après cette modification DetailsView affiche le texte « True » pour les produits interrompus et « False » pour les produits qui sont toujours actifs.


[![Les chaînes de valeur True et False sont utilisés pour afficher l’état abandonné](using-templatefields-in-the-detailsview-control-vb/_static/image26.png)](using-templatefields-in-the-detailsview-control-vb/_static/image25.png)

**Figure 9**: les chaînes True et False sont utilisés pour afficher l’état Abandonné ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-detailsview-control-vb/_static/image27.png))


Imaginez que nous ne voulions les chaînes « True » ou « False » à utiliser, mais « Oui » et « Non », à la place. Cette personnalisation peut être effectuée à l’aide d’un TemplateField et une méthode de mise en forme. Une méthode de mise en forme peut prendre n’importe quel nombre de paramètres d’entrée, mais doit retourner le code HTML (sous forme de chaîne) pour injecter dans le modèle.

Ajoutez une méthode de mise en forme à la `DetailsViewTemplateField.aspx` classe code-behind de la page nommée `DisplayDiscontinuedAsYESorNO` qui accepte un `Northwind.ProductsRow` objet comme paramètre d’entrée et retourne une chaîne. Comme indiqué dans le didacticiel précédent, cette méthode *doit* être marqué comme `Protected` ou `Public` afin d’être accessible à partir du modèle.


[!code-vb[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample4.vb)]

Cette méthode vérifie le paramètre d’entrée (`discontinued`) et retourne « Oui » s’il s’agit `True`, « Non » dans le cas contraire.

> [!NOTE]
> Dans la méthode de mise en forme examinée dans le rappel de didacticiel précédent qui nous étions en passant dans un champ de données peut contenir `NULL` s et par conséquent nécessaire pour vérifier si l’employé `HiredDate` valeur de la propriété avait une base de données `NULL` valeur avant l’accès à la `EmployeesRow`de `HiredDate` propriété. Cette vérification n’est pas nécessaire ici, car le `Discontinued` colonne ne peut pas contenir de base de données `NULL` valeurs affectées. En outre, cela est la raison pour laquelle la méthode peut accepter une valeur booléenne d’entrée de paramètre au lieu d’accepter un `ProductsRow` instance ou un paramètre de type `Object`.


Cette méthode de mise en forme terminée, ne reste qu’à appeler à partir de la TemplateField `ItemTemplate`. Pour créer le TemplateField contenu soit supprimer la `Discontinued` BoundField et ajouter un nouveau TemplateField ou de convertir le `Discontinued` BoundField en TemplateField. Puis, dans la vue de balisage déclaratif, modifiez le TemplateField contenu afin qu’il contienne uniquement un modèle ItemTemplate qui appelle le `DisplayDiscontinuedAsYESorNO` méthode, en passant la valeur de l’actuel `ProductRow` l’instance `Discontinued` propriété. Vous pouvez y accéder le `Eval` (méthode). Plus précisément, le balisage du TemplateField contenu doit ressembler à :


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample5.aspx)]

Cela entraîne le `DisplayDiscontinuedAsYESorNO` méthode à appeler lors du rendu de DetailsView, en passant le `ProductRow` l’instance `Discontinued` valeur. Dans la mesure où le `Eval` méthode retourne une valeur de type `Object`, mais la `DisplayDiscontinuedAsYESorNO` méthode attend un paramètre d’entrée de type `Boolean`, nous forçons le `Eval` méthodes retournent la valeur à `Boolean`. Le `DisplayDiscontinuedAsYESorNO` méthode retournera ensuite « Oui » ou « Non » en fonction de la valeur qu’il reçoit. La valeur retournée est ce qui est affiché dans ce contrôle DetailsView (voir Figure 10) de la ligne.


[![Les valeurs Yes ou NO sont maintenant affichés dans la ligne de fin de série](using-templatefields-in-the-detailsview-control-vb/_static/image29.png)](using-templatefields-in-the-detailsview-control-vb/_static/image28.png)

**Figure 10**: valeurs Oui ou non sont maintenant affichés dans la ligne de fin de série ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-detailsview-control-vb/_static/image30.png))


## <a name="summary"></a>Récapitulatif

Le TemplateField contenu dans le contrôle DetailsView permet d’obtenir un degré plus élevé de flexibilité dans l’affichage des données qu’est disponible avec les autres contrôles de champ et sont idéales pour les situations où :

- Plusieurs champs de données doivent être affichés dans une colonne GridView
- Les données sont mieux exprimées à l’aide d’un contrôle Web plutôt que texte brut
- La sortie varie selon les données sous-jacentes, telles que l’affichage des métadonnées ou de reformater les données

Alors que TemplateField permettre une plus grande flexibilité dans le rendu des données sous-jacentes de DetailsView, la sortie de DetailsView semble toujours un peu bruts comme chaque champ est restitué sous la forme d’une ligne dans le code HTML `<table>`.

Le contrôle FormView offre une plus grande souplesse dans la configuration de la sortie rendue. Le contrôle FormView ne contient-elle pas de champs, mais plutôt qu’une série de modèles (`ItemTemplate`, `EditItemTemplate`, `HeaderTemplate`, et ainsi de suite). Nous verrons comment utiliser le contrôle FormView pour obtenir un meilleur contrôle de la disposition de l’affichage dans notre didacticiel suivant.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Dan Jagers. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](using-templatefields-in-the-gridview-control-vb.md)
> [Suivant](using-the-formview-s-templates-vb.md)
