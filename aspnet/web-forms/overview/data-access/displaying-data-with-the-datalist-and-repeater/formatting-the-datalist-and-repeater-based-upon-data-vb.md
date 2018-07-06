---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
title: Mise en forme les contrôles DataList et Repeater en fonction des données (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous détaillerons à obtenir des exemples de la façon dont nous mettre en forme l’apparence des contrôles DataList et Repeater, à l’aide des fonctions de mise en forme avec...
ms.author: aspnetcontent
ms.date: 09/13/2006
ms.assetid: e2f401ae-37bb-4b19-aa97-d6b385d40f88
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 428438b2bae062c09d13c002f4729c3c394975a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807708"
---
<a name="formatting-the-datalist-and-repeater-based-upon-data-vb"></a>Mise en forme les contrôles DataList et Repeater en fonction des données (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_VB.exe) ou [télécharger le PDF](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/datatutorial30vb1.pdf)

> Dans ce didacticiel nous allons parcourir des exemples de la façon dont nous mettre en forme l’apparence des contrôles DataList et Repeater, à l’aide des fonctions de mise en forme au sein des modèles ou en gérant l’événement lié aux données.


## <a name="introduction"></a>Introduction

Comme nous l’avons vu dans le didacticiel précédent, le contrôle DataList offre un nombre de propriétés de style qui affectent son apparence. En particulier, nous avons vu comment affecter des classes CSS par défaut pour le contrôle DataList s `HeaderStyle`, `ItemStyle`, `AlternatingItemStyle`, et `SelectedItemStyle` propriétés. Outre ces quatre propriétés, le contrôle DataList comprend d’autres propriétés associées au style, tel que `Font`, `ForeColor`, `BackColor`, et `BorderWidth`, pour citer que quelques-uns. Le contrôle Repeater ne contienne pas toutes les propriétés associées au style. Toutes ces paramètres de style doivent être effectuées directement dans le balisage dans les modèles de s Repeater.

Souvent, cependant, comment les données doivent être mis en forme dépend des données lui-même. Par exemple, lors de l’énumération des produits nous souhaitions afficher les informations de produit dans une couleur de police gris clair si elle n’est plus disponible, ou nous pouvons choisir de mettre en surbrillance le `UnitsInStock` valeur si elle est égale à zéro. Comme nous l’avons vu dans les didacticiels précédents, le GridView, DetailsView et FormView offrent deux façons distinctes pour mettre en forme leur apparence en fonction de leurs données :

- **Le `DataBound` événement** créer un gestionnaire d’événements approprié `DataBound` événement, ce qui se déclenche après que les données a été liées à chaque élément (pour le contrôle GridView c’était le `RowDataBound` événement ; pour les contrôles DataList et Repeater doivent impérativement le `ItemDataBound`événement). Dans ce cas, gestionnaire, les données simplement liées peut être examiné et prendre des décisions de mise en forme. Nous avons examiné cette technique dans le [mise en forme en fonction lors de données personnalisées](../custom-formatting/custom-formatting-based-upon-data-vb.md) didacticiel.
- **Mise en forme de fonctions dans les modèles** lors de l’utilisation de TemplateField dans les contrôles DetailsView ou GridView, ou un modèle dans le contrôle FormView, nous pouvons ajouter une fonction de mise en forme à la classe code-behind de s de page ASP.NET, la couche de logique métier ou l’un autre bibliothèque de classes qui est accessible à partir de l’application web. Cette fonction de mise en forme peut accepter un nombre arbitraire de paramètres d’entrée, mais il doit retourner le code HTML à restituer dans le modèle. Fonctions de mise en forme ont été tout d’abord examinées dans le [à l’aide de TemplateFields dans le contrôle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) didacticiel.

Deux de ces techniques de mise en forme sont disponibles avec les contrôles DataList et Repeater. Dans ce didacticiel, nous allez parcourir les exemples d’utilisation de ces deux techniques pour les deux contrôles.

## <a name="using-theitemdataboundevent-handler"></a>À l’aide de la`ItemDataBound`Gestionnaire d’événements

Lorsque les données sont liées à un contrôle DataList, à partir d’un contrôle de source de données ou par le biais de par programmation les données au contrôle s `DataSource` propriété et en appelant son `DataBind()` (méthode), le contrôle DataList s `DataBinding` événement est déclenché, la source de données énumérée, et chaque enregistrement de données est lié à la DataList. Pour chaque enregistrement dans la source de données, le contrôle DataList crée un [ `DataListItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.aspx) de l’objet qui est ensuite lié à l’enregistrement actif. Pendant ce processus, le contrôle DataList déclenche deux événements :

- **`ItemCreated`** se déclenche après la `DataListItem` a été créé.
- **`ItemDataBound`** se déclenche après l’enregistrement en cours a été liée à la `DataListItem`

Les étapes suivantes décrivent le processus de liaison de données pour le contrôle DataList.

1. Le contrôle DataList s [ `DataBinding` événement](https://msdn.microsoft.com/library/system.web.ui.control.databinding.aspx) se déclenche
2. Les données sont liées à la DataList  
  
   Pour chaque enregistrement dans la source de données 

    1. Créer un `DataListItem` objet
    2. Déclencher la [ `ItemCreated` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. Lier l’enregistrement à la `DataListItem`
    4. Déclencher la [ `ItemDataBound` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. Ajouter le `DataListItem` à la `Items` collection

Lors de la liaison de données au contrôle Repeater, elle concerne toute exactement la même séquence d’étapes. La seule différence est qu’au lieu de `DataListItem` utilise des instances en cours de création, le contrôle Repeater [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s.

> [!NOTE]
> Le lecteur astucieux avez peut-être remarqué une légère anomalie entre la séquence d’étapes qui se produira lorsque les contrôles DataList et Repeater liés aux données et lorsque le contrôle GridView est lié aux données. À la fin de la fin du processus de liaison de données, le contrôle GridView déclenche le `DataBound` événement ; Toutefois, le contrôle Repeater ni DataList ont un tel événement. Il s’agit, car les contrôles DataList et Repeater ont été créés dans le délai d’exécution ASP.NET 1.x, avant que le modèle de gestionnaire d’événements pré- et post-niveau était devenu commun.


Comme avec le contrôle GridView, une option de mise en forme en fonction des données consiste à créer un gestionnaire d’événements pour le `ItemDataBound` événement. Ce gestionnaire d’événements aurait inspecter les données qui avaient simplement été liées à la `DataListItem` ou `RepeaterItem` et affectent la mise en forme du contrôle en fonction des besoins.

Pour le contrôle DataList, mise en forme pour l’élément entier peut être implémentée à l’aide de la `DataListItem` s associées au style propriétés, notamment la norme `Font`, `ForeColor`, `BackColor`, `CssClass`, et ainsi de suite. Pour affecter la mise en forme de contrôles Web particuliers dans le modèle de s DataList, nous devons accéder par programmation et de modifier le style de ces contrôles Web. Nous avons vu comment accomplir cette arrière dans le *mise en forme en fonction lors de données personnalisées* didacticiel. Comme le contrôle Repeater, le `RepeaterItem` classe n’a pas de propriétés associées au style ; par conséquent, toutes les modifications associées au style apportées à un `RepeaterItem` dans le `ItemDataBound` Gestionnaire d’événements doit être effectué en accédant par programme et de contrôles Web au sein de la mise à jour le modèle.

Dans la mesure où le `ItemDataBound` technique de mise en forme pour les contrôles DataList et Repeater sont virtuellement identiques, notre exemple aborde l’utilisation du contrôle DataList.

## <a name="step-1-displaying-product-information-in-the-datalist"></a>Étape 1 : Afficher des informations sur les produits dans le contrôle DataList

Avant de nous soucier de la mise en forme, s permettent de créer une page qui utilise un contrôle DataList pour afficher des informations de produit. Dans le [didacticiel précédent](displaying-data-with-the-datalist-and-repeater-controls-vb.md) que nous avons créé un contrôle DataList dont `ItemTemplate` affiche chaque nom de produit s, la catégorie, la fournisseur, la quantité par unité et le prix. Laissez s Répétez cette fonctionnalité ici dans ce didacticiel. Pour ce faire, vous pouvez soit recréer le contrôle DataList et son ObjectDataSource à partir de zéro, ou vous pouvez copier sur ces contrôles à partir de la page créée dans le didacticiel précédent (`Basics.aspx`) et les coller dans la page pour ce didacticiel (`Formatting.aspx`).

Une fois que vous avez répliqué la fonctionnalité des contrôles DataList et ObjectDataSource depuis `Basics.aspx` dans `Formatting.aspx`, prenez un moment pour modifier le contrôle DataList s `ID` propriété à partir de `DataList1` à plus descriptif `ItemDataBoundFormattingExample`. Ensuite, affichez le contrôle DataList dans un navigateur. Comme le montre la Figure 1, la seule différence de mise en forme entre chaque produit est que fait alterner la couleur d’arrière-plan.


[![Les produits sont répertoriés dans le contrôle DataList](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image1.png)

**Figure 1**: les produits sont répertoriés dans le contrôle DataList ([cliquez pour afficher l’image en taille réelle](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image3.png))


Pour ce didacticiel, permettent de mettre en forme le contrôle DataList telles que tous les produits dont le prix est inférieur à $20,00 aura à la fois son nom et le prix unitaire affiché en surbrillance jaune s.

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>Étape 2 : Déterminer par programmation la valeur des données dans le Gestionnaire d’événement ItemDataBound

Étant donné qu’uniquement les produits dont le prix sous $ 20,00 par la mise en forme personnalisée appliquée, nous devons être en mesure de déterminer le prix de s de chaque produit. Lors de la liaison de données à un contrôle DataList, le contrôle DataList énumère les enregistrements dans sa source de données et, pour chaque enregistrement, crée un `DataListItem` instance, la liaison de l’enregistrement de source de données à le `DataListItem`. Une fois l’enregistrement particulier s des données a été liées à actuel `DataListItem` objet, le contrôle DataList s `ItemDataBound` événement est déclenché. Nous pouvons créer un gestionnaire d’événements pour cet événement pour inspecter les valeurs de données pour le cours `DataListItem` et, en fonction de ces valeurs, effectuez les modifications de mise en forme nécessaires.

Créer un `ItemDataBound` événement pour le contrôle DataList et ajoutez le code suivant :


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample1.vb)]

Alors que le concept et la sémantique derrière le contrôle DataList s `ItemDataBound` Gestionnaire d’événements sont les mêmes que celles utilisées par les opérations de mappage GridView `RowDataBound` Gestionnaire d’événements dans le *mise en forme en fonction lors de données personnalisées* didacticiel, la syntaxe est différente. légèrement. Lorsque le `ItemDataBound` événement est déclenché, le `DataListItem` simplement liée aux données est passée dans le Gestionnaire d’événements correspondant par le biais de `e.Item` (au lieu de `e.Row`, comme avec les opérations de mappage GridView `RowDataBound` Gestionnaire d’événements). Le contrôle DataList s `ItemDataBound` Gestionnaire d’événements se déclenche pour *chaque* ligne ajoutée à la DataList, y compris les lignes d’en-tête, des lignes de pied de page et des lignes du séparateur. Toutefois, les informations de produit sont associées uniquement aux lignes de données. Par conséquent, lorsque vous utilisez le `ItemDataBound` événement pour inspecter les données lié à la DataList, nous devons tout d’abord vous assurer que nous re fonctionne avec un élément de données. Cela est possible en vérifiant la `DataListItem` s [ `ItemType` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx), qui peut avoir une des [huit valeurs suivants](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

Les deux `Item` et `AlternatingItem``DataListItem` des éléments de données de composition le s DataList s. En supposant que nous re fonctionne avec un `Item` ou `AlternatingItem`, nous avons accès réel `ProductsRow` instance qui était liée à l’actuel `DataListItem`. Le `DataListItem` s [ `DataItem` propriété](https://msdn.microsoft.com/system.web.ui.webcontrols.datalistitem.dataitem.aspx) contient une référence à la `DataRowView` de l’objet, dont la propriété `Row` propriété fournit une référence au véritable `ProductsRow` objet.

Ensuite, nous vérifions le `ProductsRow` instance s `UnitPrice` propriété. Depuis la table Products s `UnitPrice` champ permet `NULL` valeurs, avant de tenter d’accéder à la `UnitPrice` propriété nous devons tout d’abord vérifier s’il a un `NULL` valeur à l’aide de la `IsUnitPriceNull()` (méthode). Si le `UnitPrice` valeur n’est pas `NULL`, nous devez vérifier si elle s inférieur à $20,00. S’il s’agit en effet sous 20,00 $, nous devons appliquer la mise en forme personnalisée.

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>Étape 3 : Mise en surbrillance le nom du produit s et le prix

Une fois que nous savons que les prix d’un produit s est inférieur à $20,00, reste qu’à mettre en évidence son nom et le prix. Pour ce faire, nous devons tout d’abord faire référence par programme les contrôles Label dans le `ItemTemplate` qui affichent le nom de produit s et le prix. Ensuite, nous devons leur faire afficher un arrière-plan jaune. Ces informations de mise en forme peuvent être appliquées en modifiant directement les étiquettes `BackColor` propriétés (`LabelID.BackColor = Color.Yellow`) ; dans l’idéal, cependant, tous les aspects liés à l’affichage doivent être exprimés via des feuilles de style en cascade. En fait, nous avons déjà une feuille de style qui fournit la mise en forme souhaitée défini dans `Styles.css`  -  `AffordablePriceEmphasis`, qui a été créé et décrits dans le *mise en forme en fonction lors de données personnalisées* didacticiel.

Pour appliquer la mise en forme, il suffit de définir les deux contrôles Web Label `CssClass` propriétés à `AffordablePriceEmphasis`, comme illustré dans le code suivant :


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample2.vb)]

Avec le `ItemDataBound` terminée de gestionnaire d’événements, de revoir le `Formatting.aspx` page dans un navigateur. Comme le montre la Figure 2, ces produits dont le prix sous $20,00 ont leur nom et le prix mis en surbrillance.


[![Ces produits inférieur 20,00 $ sont mises en surbrillance](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image4.png)

**Figure 2**: ceux produits inférieur à $20,00 sont mis en surbrillance ([cliquez pour afficher l’image en taille réelle](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image6.png))


> [!NOTE]
> Étant donné que le contrôle DataList est restitué sous la forme d’un élément HTML `<table>`, ses `DataListItem` instances possèdent des propriétés associées au style qui peuvent être définies pour appliquer un style spécifique à l’élément entier. Par exemple, si nous voulions mettre en surbrillance le *entière* élément jaune lorsque son prix était inférieur à $20,00, nous pourrions avoir remplacé le code qui a référencé les étiquettes et définir leur `CssClass` propriétés avec la ligne de code suivante : `e.Item.CssClass = "AffordablePriceEmphasis"` (voir Figure 3).


Le `RepeaterItem` s qui composent le contrôle Repeater, toutefois, don t offrent ces propriétés au niveau du style. Par conséquent, l’application mise en forme personnalisée pour le contrôle Repeater nécessite l’application de propriétés de style pour les contrôles Web dans les modèles Repeater s, comme nous l’avons fait dans la Figure 2.


[![L’élément de produit entier est mis en surbrillance pour les produits sous 20,00 $](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image7.png)

**Figure 3**: l’élément de produit entier est mis en surbrillance pour les produits sous $20,00 ([cliquez pour afficher l’image en taille réelle](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image9.png))


## <a name="using-formatting-functions-from-within-the-template"></a>Utilisation de fonctions de mise en forme à partir de dans le modèle

Dans le *à l’aide de TemplateFields dans le contrôle GridView* didacticiel, nous avons vu comment utiliser une fonction de mise en forme au sein d’un GridView TemplateField pour appliquer une mise en forme personnalisées basées sur les données liées aux lignes s GridView. Une fonction de mise en forme est une méthode qui peut être appelée à partir d’un modèle et retourne le code HTML pour être émis à la place. Fonctions de mise en forme peut résider dans la classe code-behind de pages ASP.NET ou peuvent être centralisées dans des fichiers de classe dans le `App_Code` dossier ou dans un projet de bibliothèque de classes distinct. Déplacement de la fonction de mise en forme en dehors de la classe de code-behind ASP.NET page s est idéal si vous prévoyez d’utiliser la même fonction de mise en forme dans plusieurs pages ASP.NET ou dans d’autres applications web ASP.NET.

Pour illustrer les fonctions de mise en forme, s permettent de disposer des informations produit incluent le texte [DISCONTINUED] en regard du nom de produit s si elle s abandonné. En outre, let s ont if jaune en surbrillance prix il s inférieur à $20,00 (comme nous l’avons fait le `ItemDataBound` exemple de gestionnaire d’événements) ; si le prix est 20,00 $ ou s plus élevé, vous permettent pas d’afficher le prix réel, mais le texte, veuillez appeler à la place de devis. Figure 4 montre une capture d’écran des liste avec des ces règles de mise en forme des produits.


[![Pour les produits coûteuse, le prix est remplacé par le texte, appelez de devis](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image10.png)

**Figure 4**: pour Expensive Products, le prix est remplacé par le texte, appelez de devis ([cliquez pour afficher l’image en taille réelle](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image12.png))


## <a name="step-1-create-the-formatting-functions"></a>Étape 1 : Créer les fonctions de mise en forme

Pour cet exemple nous avons besoin de deux fonctions de mise en forme, qui affiche le nom du produit ainsi que le texte [DISCONTINUED], si nécessaire et un autre qui affiche soit un prix en surbrillance si elle s inférieure 20,00 $ ou le texte, appelez de devis sinon. Laisser s créer ces fonctions dans la classe code-behind de pages ASP.NET et nommez-les `DisplayProductNameAndDiscontinuedStatus` et `DisplayPrice`. Les deux méthodes doivent retourner le code HTML à restituer sous forme de chaîne et les deux doivent être marqués `Protected` (ou `Public`) afin d’être appelé à partir de la partie de la syntaxe déclarative s de page ASP.NET. Le code pour ces deux méthodes suit :


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample3.vb)]

Notez que le `DisplayProductNameAndDiscontinuedStatus` méthode accepte les valeurs de la `productName` et `discontinued` champs de données en tant que valeurs scalaires, alors que le `DisplayPrice` méthode accepte un `ProductsRow` instance (au lieu d’un `unitPrice` valeur scalaire). Chacune de ces approches fonctionnera ; Toutefois, si la fonction de mise en forme fonctionne avec des valeurs scalaires qui peuvent contenir de base de données `NULL` valeurs (telles que `UnitPrice`; ni `ProductName` ni `Discontinued` autoriser `NULL` valeurs), doit être particulièrement vigilant dans la gestion de ces entrées scalaires.

En particulier, le paramètre d’entrée doit être de type `Object` dans la mesure où la valeur entrante peut être un `DBNull` instance au lieu du type de données attendu. En outre, il doit être vérifié pour déterminer si la valeur entrante est une base de données `NULL` valeur. Autrement dit, si nous voulions le `DisplayPrice` méthode accepter le prix comme une valeur scalaire, nous d obligé d’utiliser le code suivant :


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample4.vb)]

Notez que le `unitPrice` paramètre d’entrée est de type `Object` et que l’instruction conditionnelle a été modifiée pour déterminer si `unitPrice` est `DBNull` ou non. En outre, depuis le `unitPrice` paramètre d’entrée est transmis comme un `Object`, elle doit être convertie en valeur décimale.

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>Étape 2 : Appel de la fonction de mise en forme à partir de l’ItemTemplate DataList s

Avec les fonctions de mise en forme ajoutées à la classe code-behind ASP.NET page s, il reste qu’à appeler ces fonctions à partir du contrôle DataList s de mise en forme `ItemTemplate`. Pour appeler une fonction de mise en forme à partir d’un modèle, placez l’appel de fonction au sein de la syntaxe de liaison de données :


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample5.aspx)]

Dans le contrôle DataList s `ItemTemplate` le `ProductNameLabel` contrôle Web Label affiche actuellement le nom du produit s, affectez son `Text` propriété le résultat de `<%# Eval("ProductName") %>`. Afin qu’il affiche le nom et le texte [DISCONTINUED], si nécessaire, mettre à jour la syntaxe déclarative afin qu’il assigne à la place la `Text` propriété la valeur de la `DisplayProductNameAndDiscontinuedStatus` (méthode). Ce cas, nous devons passer dans le nom de produit s et les valeurs supprimées à l’aide de la `Eval("columnName")` syntaxe. `Eval` Retourne une valeur de type `Object`, mais la `DisplayProductNameAndDiscontinuedStatus` méthode attend des paramètres d’entrée de type `String` et `Boolean`; par conséquent, nous devons convertir les valeurs retournées par la `Eval` méthode aux types de paramètre d’entrée attendu, comme suit :


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample6.aspx)]

Pour afficher le prix, nous pouvons simplement définir la `UnitPriceLabel` étiquette s `Text` propriété à la valeur retournée par la `DisplayPrice` méthode, comme nous l’avez fait pour l’affichage du nom de produit s et [supprimé] texte. Toutefois, au lieu de passer le `UnitPrice` comme paramètre d’entrée scalaire, nous passons à la place dans l’ensemble `ProductsRow` instance :


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample7.aspx)]

Avec les appels aux fonctions de mise en forme en place, prenez un moment pour consulter notre progression dans un navigateur. Votre écran doit ressembler à la Figure 5, avec les produits interrompus, y compris le texte [DISCONTINUED] et les produits d’évaluation des coûts de plus de $ 20,00 par avoir leur prix remplacé par le texte, l’appel de devis.


[![Pour les produits coûteuse, le prix est remplacé par le texte, appelez de devis](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image13.png)

**Figure 5**: pour Expensive Products, le prix est remplacé par le texte, appelez de devis ([cliquez pour afficher l’image en taille réelle](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image15.png))


## <a name="summary"></a>Récapitulatif

Mise en forme le contenu d’un contrôle DataList ou Repeater basées sur les données peut être accomplie à l’aide de deux techniques. La première technique consiste à créer un gestionnaire d’événements pour le `ItemDataBound` événement, ce qui se déclenche que chaque enregistrement dans la source de données est lié à un nouveau `DataListItem` ou `RepeaterItem`. Dans le `ItemDataBound` Gestionnaire d’événements, les données actuelles de l’élément s peuvent être examinées et ensuite mise en forme peut être appliqué au contenu du modèle ou, pour `DataListItem` s, à l’élément lui-même.

Vous pouvez également la mise en forme personnalisée peut être réalisée via la mise en forme de fonctions. Une fonction de mise en forme est une méthode qui peut être appelée à partir du contrôle DataList ou modèles s Repeater qui retourne le code HTML pour émettre à la place. Souvent, le code HTML retourné par une fonction de mise en forme est déterminé par les valeurs en cours de liaison à l’élément actuel. Ces valeurs peuvent être passées à la fonction de mise en forme, comme des valeurs scalaires ou en passant l’objet entier qui est lié à l’élément (comme le `ProductsRow` instance).

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Yaakov Ellis, Randy Schmidt et Liz Shulok. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
> [Suivant](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
