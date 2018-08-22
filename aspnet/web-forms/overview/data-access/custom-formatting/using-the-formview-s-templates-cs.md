---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
title: À l’aide de modèles de FormView (c#) | Microsoft Docs
author: rick-anderson
description: Contrairement à DetailsView, FormView n’est pas composé de champs. Au lieu de cela, le contrôle FormView est restitué à l’aide de modèles. Dans ce didacticiel, nous allons examiner à l’aide de la F....
ms.author: riande
ms.date: 03/31/2010
ms.assetid: d3f062af-88cf-426d-af44-e41f32c41672
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
msc.type: authoredcontent
ms.openlocfilehash: 1a7cf17d8cbd0a5a17a387b9a70336a1b06efde7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824293"
---
<a name="using-the-formviews-templates-c"></a>À l’aide de modèles de FormView (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_14_CS.exe) ou [télécharger le PDF](using-the-formview-s-templates-cs/_static/datatutorial14cs1.pdf)

> Contrairement à DetailsView, FormView n’est pas composé de champs. Au lieu de cela, le contrôle FormView est restitué à l’aide de modèles. Dans ce didacticiel, nous allons examiner utilisant le contrôle FormView pour présenter un affichage moins rigid de données.


## <a name="introduction"></a>Introduction

Dans les deux derniers didacticiels, nous avons vu comment personnaliser les sorties de contrôles GridView et DetailsView à l’aide de TemplateFields. TemplateField permettre le contenu d’un champ spécifique être hautement personnalisées, mais au final GridView et DetailsView ont une apparence bruts au lieu de cela, la grille. Pour de nombreux scénarios, une mise en page de grille est idéale, mais parfois un affichage plus fluid, moins rigid est nécessaire. Lors de l’affichage d’un enregistrement unique, telle une mise en page fluide est possible en utilisant le contrôle FormView.

Contrairement à DetailsView, FormView n’est pas composé de champs. Impossible d’ajouter un BoundField ou un TemplateField pour un FormView. Au lieu de cela, le contrôle FormView est restitué à l’aide de modèles. Considérer le FormView comme un contrôle DetailsView qui contient un TemplateField unique. Le contrôle FormView prend en charge les modèles suivants :

- `ItemTemplate` utilisé pour restituer l’enregistrement particulier affiché dans le contrôle FormView
- `HeaderTemplate` permet de spécifier une ligne d’en-tête facultatif
- `FooterTemplate` permet de spécifier une ligne de pied de page facultatif
- `EmptyDataTemplate` Lorsque le FormView `DataSource` ne dispose pas de tous les enregistrements, le `EmptyDataTemplate` est utilisé à la place de la `ItemTemplate` pour restituer le balisage du contrôle
- `PagerTemplate` peut être utilisé pour personnaliser l’interface de pagination pour FormViews ayant la pagination est activée
- `EditItemTemplate` / `InsertItemTemplate` utilisé pour personnaliser l’interface d’édition ou insertion interface pour FormViews qui prennent en charge ce type de fonctionnalité

Dans ce didacticiel, nous allons examiner utilisant le contrôle FormView pour présenter un affichage moins rigid de produits. Au lieu d’avoir des champs pour le nom, catégorie, fournisseur et d’ainsi de suite, le contrôle FormView `ItemTemplate` affiche ces valeurs à l’aide d’une combinaison d’un élément d’en-tête et un `<table>` (voir Figure 1).


[![Le contrôle FormView découpe de la disposition de grille illustrée dans le contrôle DetailsView](using-the-formview-s-templates-cs/_static/image2.png)](using-the-formview-s-templates-cs/_static/image1.png)

**Figure 1**: le contrôle FormView quitte la disposition Grid-Like vu dans le contrôle DetailsView ([cliquez pour afficher l’image en taille réelle](using-the-formview-s-templates-cs/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-formview"></a>Étape 1 : Liaison des données pour le contrôle FormView

Ouvrez le `FormView.aspx` page et faites glisser un FormView à partir de la boîte à outils vers le concepteur. Lorsque vous ajoutez le contrôle FormView il apparaît comme une zone grise, en nous demandant qu’un `ItemTemplate` est nécessaire.


[![Le contrôle FormView ne peut pas être restitué dans le concepteur jusqu'à ce qu’un modèle ItemTemplate est fourni](using-the-formview-s-templates-cs/_static/image5.png)](using-the-formview-s-templates-cs/_static/image4.png)

**Figure 2**: The de FormView ne peut pas être restitué dans le concepteur jusqu'à ce qu’un `ItemTemplate` est fourni ([cliquez pour afficher l’image en taille réelle](using-the-formview-s-templates-cs/_static/image6.png))


Le `ItemTemplate` peuvent être créés manuellement (via la syntaxe déclarative) ou peut être créé automatiquement par le contrôle FormView de liaison à un contrôle de source de données via le concepteur. Cette statistique créée automatiquement `ItemTemplate` contient du code HTML que répertorie le nom de chaque champ et une étiquette de contrôle dont `Text` propriété est liée à la valeur du champ. Cette approche également auto-crée un `InsertItemTemplate` et `EditItemTemplate`, qui sont remplies avec les contrôles d’entrée pour chacun des champs de données retournés par le contrôle de source de données.

Si vous souhaitez créer automatiquement le modèle, à partir de la balise active du FormView ajouter un nouveau contrôle ObjectDataSource qui appelle le `ProductsBLL` la classe `GetProducts()` (méthode). Cela créera un FormView avec un `ItemTemplate`, `InsertItemTemplate`, et `EditItemTemplate`. À partir de la vue de Source, supprimez le `InsertItemTemplate` et `EditItemTemplate` dans la mesure où nous ne sommes pas intéressés par la création d’un FormView prend en charge l’édition ou insertion encore. Ensuite, désactivez out le balisage dans le `ItemTemplate` afin que nous avons vierge pour travailler à partir de.

Si vous créeriez au lieu de cela les `ItemTemplate` manuellement, vous pouvez ajouter et configurer l’ObjectDataSource en le faisant glisser à partir de la boîte à outils vers le concepteur. Toutefois, ne définissez pas source de données de FormView à partir du concepteur. Au lieu de cela, accédez à la vue de Source et définir manuellement le FormView `DataSourceID` propriété le `ID` valeur de l’ObjectDataSource. Ensuite, ajoutez manuellement le `ItemTemplate`.

Quelle que soit quelle approche vous décidé de prendre, à ce stade balisage déclaratif de votre FormView doit ressembler :


[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample1.aspx)]

Prenez un moment pour vérifier la case à cocher Activer la pagination dans la balise active du FormView ; Cela ajoutera le `AllowPaging="True"` attribut syntaxe déclarative FormView. En outre, définissez le `EnableViewState` False à la propriété.

## <a name="step-2-defining-theitemtemplates-markup"></a>Étape 2 : Définir le`ItemTemplate`du balisage

Avec le contrôle FormView liée au contrôle ObjectDataSource et configuré pour prendre en charge la pagination, nous sommes prêts spécifier le contenu pour le `ItemTemplate`. Pour ce didacticiel, nous allons avoir le nom du produit affiché dans un `<h3>` titre. Ensuite, nous allons utiliser HTML `<table>` pour afficher les propriétés restantes de produit dans une table de quatre colonnes où les premier et troisième colonnes répertorient les noms de propriété et la seconde et la quatrième liste de leurs valeurs.

Ce balisage peut être entré dans via l’interface de modification de modèle de FormView dans le concepteur ou manuellement via la syntaxe déclarative. Lorsque vous travaillez avec des modèles généralement est-elle plus rapide de travailler directement avec la syntaxe déclarative, mais n’hésitez pas à utiliser la technique qui vous convient le mieux.

Le balisage suivant montre le balisage déclaratif FormView après le `ItemTemplate`de structure a été effectuée :


[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample2.aspx)]

Notez que la syntaxe de liaison de données - `<%# Eval("ProductName") %>`, par exemple peut être injecté directement dans la sortie du modèle. Autrement dit, elle ne doive pas être affectée à un contrôle Label `Text` propriété. Par exemple, nous avons le `ProductName` valeur affichée dans un `<h3>` à l’aide de l’élément `<h3><%# Eval("ProductName") %></h3>`, qui, pour le produit Chai s’affiche en tant que `<h3>Chai</h3>`.

Le `ProductPropertyLabel` et `ProductPropertyValue` classes CSS sont utilisées pour spécifier le style des noms de propriété de produit et des valeurs dans le `<table>`. Ces classes CSS sont définies dans `Styles.css` , entraînant des noms de propriété être en gras et aligné à droite et ajouter un droit de remplissage pour les valeurs de propriété.

Dans la mesure où aucun CheckBoxFields ne sont disponibles avec le contrôle FormView, afin d’afficher le `Discontinued` valeur comme une case à cocher, nous devons ajouter notre propre contrôle de case à cocher. Le `Enabled` propriété est définie sur False, ce qui en lecture seule et la case à cocher `Checked` propriété est liée à la valeur de la `Discontinued` champ de données.

Avec la `ItemTemplate` terminée, les informations de produit s’affichent de manière beaucoup plus fluide. Comparez la sortie de DetailsView à partir du dernier didacticiel (Figure 3) avec la sortie générée par le contrôle FormView dans ce didacticiel (Figure 4).


[![La sortie de DetailsView rigides](using-the-formview-s-templates-cs/_static/image8.png)](using-the-formview-s-templates-cs/_static/image7.png)

**Figure 3**: la sortie de DetailsView rigide ([cliquez pour afficher l’image en taille réelle](using-the-formview-s-templates-cs/_static/image9.png))


[![La sortie de FormView FLUIDE](using-the-formview-s-templates-cs/_static/image11.png)](using-the-formview-s-templates-cs/_static/image10.png)

**Figure 4**: sortie FormView fluide ([cliquez pour afficher l’image en taille réelle](using-the-formview-s-templates-cs/_static/image12.png))


## <a name="summary"></a>Récapitulatif

Alors que les contrôles GridView et DetailsView peuvent avoir leur sortie personnalisée à l’aide de TemplateFields, les deux présentent toujours leurs données dans un format de type grille, bruts. Dans les cas lorsqu’un enregistrement unique doit être indiqué à l’aide d’une disposition moins rigide, FormView constitue le choix idéal. Comme le contrôle DetailsView, FormView restitue un enregistrement unique à partir de son `DataSource`, mais contrairement à DetailsView se compose simplement de modèles et ne prend pas en charge les champs.

Comme nous l’avons vu dans ce didacticiel, le contrôle FormView permet une présentation plus souple lors de l’affichage d’un seul enregistrement. Dans les futures les didacticiels, nous allons examiner les contrôles DataList et Repeater, qui fournissent le même niveau de flexibilité que le FormsView, mais sont en mesure d’afficher plusieurs enregistrements (par exemple, le contrôle GridView).

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été E.R. Gillain. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](using-templatefields-in-the-detailsview-control-cs.md)
> [Suivant](displaying-summary-information-in-the-gridview-s-footer-cs.md)
