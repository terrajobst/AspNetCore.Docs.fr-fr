---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
title: Ajout de contrôles de Validation à la modification des Interfaces et d’insertion (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons voir combien il est facile pour ajouter des contrôles de validation aux EditItemTemplate et InsertItemTemplate d’un contrôle Web, des données pour fournir une plus...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: e3d7028a-7a22-4a4f-babe-d53afc41c0e2
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
msc.type: authoredcontent
ms.openlocfilehash: d06408717bdf5e7446597ae4330ffb32cf943e7f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829239"
---
<a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-vb"></a>Ajout de contrôles de Validation à la modification des Interfaces et d’insertion (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_VB.exe) ou [télécharger le PDF](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/datatutorial19vb1.pdf)

> Dans ce didacticiel, nous allons voir combien il est facile pour ajouter des contrôles de validation au EditItemTemplate et InsertItemTemplate d’un contrôle Web, des données pour fournir une interface utilisateur plus infaillible.


## <a name="introduction"></a>Introduction

Les contrôles GridView et DetailsView dans les exemples que nous avons exploré au cours des trois didacticiels ont tous été composés de BoundFields et CheckBoxFields (les types de champs ajoutés automatiquement par Visual Studio lors de la liaison d’un GridView ou un contrôle DetailsView à une source de données contrôler via la balise active). Lorsque vous modifiez une ligne dans un GridView ou d’un contrôle DetailsView, ces BoundFields qui ne sont pas en lecture seule sont convertis en zones de texte, à partir de laquelle l’utilisateur final peut modifier les données existantes. De même, lorsque insérer un nouveau enregistrer dans un contrôle DetailsView, ces BoundFields dont `InsertVisible` propriété est définie sur `True` (la valeur par défaut) sont rendus sous forme de zones de texte vide, dans laquelle l’utilisateur peut fournir des valeurs de champ du nouvel enregistrement. De même, CheckBoxFields, qui sont désactivées dans l’interface standard, en lecture seule, sont convertis en cases à cocher est activée dans la modification des interfaces et d’insertion.

Si la valeur par défaut, modification et l’insertion des interfaces pour la BoundField et CheckBoxField peut être utile, l’interface n’a pas une forme quelconque de validation. Si un utilisateur commet une erreur données entrée - par exemple en omettant le `ProductName` champ ou en entrant une valeur non valide pour `UnitsInStock` (par exemple, -50) est une exception sera levée depuis dans les profondeurs de l’architecture d’application. Bien que cette exception peut être gérée normalement comme illustré dans le [didacticiel précédent](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md), dans l’idéal, la modification ou l’insertion de l’interface utilisateur inclut des contrôles de validation pour empêcher un utilisateur d’entrer ces données non valides dans le tout d’abord placer.

Afin de fournir une édition personnalisée ou une interface d’insertion, nous devons remplacer le BoundField ou CheckBoxField avec TemplateField. TemplateField, qui ont été le sujet de discussion dans le [à l’aide de TemplateFields dans le contrôle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) et [à l’aide de TemplateFields dans le contrôle DetailsView](../custom-formatting/using-templatefields-in-the-detailsview-control-vb.md) didacticiels, peut se composer de plusieurs modèles de définition de séparent les interfaces pour les États des lignes différentes. Le TemplateField `ItemTemplate` est utilisé pour lors du rendu des champs en lecture seule ou des lignes dans les contrôles DetailsView ou GridView, tandis que le `EditItemTemplate` et `InsertItemTemplate` indiquent les interfaces à utiliser pour la modification et l’insertion des modes, respectivement.

Dans ce didacticiel, nous allons voir combien il est facile d’ajouter des contrôles de validation pour le TemplateField `EditItemTemplate` et `InsertItemTemplate` pour fournir une interface utilisateur plus infaillible. Plus précisément, ce didacticiel prend l’exemple créé dans le [examinant les événements associés insertion, mise à jour et suppression](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) didacticiel et renforce la modification et l’insertion d’interfaces pour inclure la validation appropriée.

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-vbmd"></a>Étape 1 : Réplication de l’exemple à partir de[examen des événements associés de l’insertion, de la mise à jour et de suppression](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)

Dans le [examinant les événements associés insertion, mise à jour et suppression](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) didacticiel, nous avons créé une page qui liste les noms et les prix des produits dans un GridView modifiable. En outre, la page inclus un contrôle DetailsView dont `DefaultMode` propriété a été définie sur `Insert`, il est donc toujours rendu en mode insertion. À partir de ce contrôle DetailsView, l’utilisateur peut entrer le nom et le prix d’un nouveau produit et cliquez sur Insertion, l’avez ajouté au système (voir Figure 1).


[![L’exemple précédent permet aux utilisateurs d’ajouter de nouveaux produits et de modifier des relations existantes](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image1.png)

**Figure 1**: le précédent exemple permet aux utilisateurs à ajouter de nouveaux produits et de modifier certains d'entre eux ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image3.png))


Notre objectif pour ce didacticiel consiste à augmenter le DetailsView et un GridView pour fournir des contrôles de validation. En particulier, notre logique de validation sera :

- Exiger que le nom lors de l’insertion ou modification d’un produit
- Exiger que le prix être fournie lors de l’insertion d’un enregistrement. Lorsque vous modifiez un enregistrement, nous nécessitera toujours un prix, mais utilise la logique de programmation dans le GridView `RowUpdating` Gestionnaire d’événements déjà présent dans le tutoriel précédent
- Vérifiez que la valeur entrée pour le prix est un format monétaire valide

Avant que nous pouvons examiner l’exemple précédent pour inclure la validation en décuplant le volume, nous devons tout d’abord répliquer l’exemple à partir de la `DataModificationEvents.aspx` page à la page pour ce didacticiel, `UIValidation.aspx`. Pour cela nous devons recopiez les deux le `DataModificationEvents.aspx` balisage déclaratif de la page et son code source. Un premier temps copier le balisage déclaratif en effectuant les étapes suivantes :

1. Ouvrez le `DataModificationEvents.aspx` page dans Visual Studio
2. Accédez à balisage déclaratif de la page (cliquez sur le bouton de la Source en bas de la page)
3. Copiez le texte dans le `<asp:Content>` et `</asp:Content>` les balises (lignes 3 à 44), comme illustré dans la Figure 2.


[![Copiez le texte au sein de la &lt;asp : Content&gt; contrôle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image4.png)

**Figure 2**: copiez le texte au sein de la `<asp:Content>` contrôle ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image6.png))


1. Ouvrez le `UIValidation.aspx` page
2. Accédez à balisage déclaratif de la page
3. Collez le texte dans le `<asp:Content>` contrôle.

Pour copier le code source, ouvrez le `DataModificationEvents.aspx.vb` page puis copiez simplement le texte *dans* la `EditInsertDelete_DataModificationEvents` classe. Copiez les trois gestionnaires d’événements (`Page_Load`, `GridView1_RowUpdating`, et `ObjectDataSource1_Inserting`), contrairement à **pas** copier la déclaration de classe. Coller le texte copié *dans* le `EditInsertDelete_UIValidation` classe dans `UIValidation.aspx.vb`.

Après avoir déplacé le contenu et le code à partir de `DataModificationEvents.aspx` à `UIValidation.aspx`, prenez un moment pour tester votre progression dans un navigateur. Vous devriez voir la même sortie et rencontrez les mêmes fonctionnalités dans chacune de ces deux pages (vous référer à la Figure 1 pour la capture d’écran `DataModificationEvents.aspx` en action).

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>Étape 2 : Conversion de la BoundFields en TemplateField

Pour ajouter des contrôles de validation aux interfaces d’éditions et insertion, les BoundFields utilisés par les contrôles DetailsView et GridView doivent être converties en TemplateField. Pour ce faire, cliquez sur les liens de modifier les colonnes et modifier les champs dans les contrôles GridView et de DetailsView balises actives, respectivement. Là, sélectionnez chacune du BoundFields et cliquez sur le lien « Convertir ce champ en TemplateField ».


[![Chacun des BoundFields le de DetailsView et de GridView convertir en TemplateField](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image7.png)

**Figure 3**: convertir chacun du de DetailsView et de GridView BoundFields dans TemplateFields ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image9.png))


Conversion d’un BoundField en TemplateField via la boîte de dialogue champs génère TemplateField présentant les mêmes interfaces en lecture seule, éditions et insertion comme le BoundField lui-même. Le balisage suivant montre la syntaxe déclarative pour le `ProductName` champ dans le contrôle DetailsView après qu’il a été converti en TemplateField :

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample1.aspx)]

Notez que cette TemplateField avait trois modèles automatiquement créés `ItemTemplate`, `EditItemTemplate`, et `InsertItemTemplate`. Le `ItemTemplate` affiche une valeur de champ de données unique (`ProductName`) à l’aide d’un contrôle Web Label, tandis que le `EditItemTemplate` et `InsertItemTemplate` présenter la valeur de champ de données dans un contrôle TextBox Web qui associe le champ de données à la zone de texte `Text` propriété à l’aide de la liaison de données bidirectionnelle. Étant donné que nous utilisons uniquement le contrôle DetailsView dans cette page pour l’insertion, vous pouvez supprimer le `ItemTemplate` et `EditItemTemplate` à partir de la deux TemplateField, bien qu’il n’existe aucun risque à les laisser.

Dans la mesure où le contrôle GridView ne prend pas en charge intégrés insertion des fonctionnalités de DetailsView, de convertir le GridView `ProductName` champ en TemplateField génère uniquement un `ItemTemplate` et `EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample2.aspx)]

En cliquant sur la « convertir ce champ en TemplateField », Visual Studio a créé un TemplateField dont modèles imitent l’interface utilisateur de la BoundField converti. Vous pouvez le vérifier en consultant cette page via un navigateur. Vous trouverez que l’apparence et le comportement de la TemplateField est identique à l’expérience lorsque BoundFields étaient utilisées à la place.

> [!NOTE]
> N’hésitez pas à personnaliser les interfaces de modifications dans les modèles en fonction des besoins. Par exemple, nous souhaiterons peut-être avoir la zone de texte dans le `UnitPrice` TemplateField restitué sous la forme d’une zone de texte plus petits que la `ProductName` zone de texte. Pour ce faire, vous pouvez définir la zone de texte `Columns` propriété une valeur appropriée ou fournir une largeur absolue via le `Width` propriété. Dans le didacticiel suivant, nous allons voir comment personnaliser entièrement l’interface de modification en remplaçant la zone de texte avec une entrée de données alternatif contrôle Web.


## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>Étape 3 : Ajout de contrôles de Validation pour le contrôle GridView`EditItemTemplate` s

Lors de la construction des formulaires de saisie de données, il est important que les utilisateurs entrent tous les champs obligatoires et que toutes les entrées fournies ne sont autorisées, correctement mise en forme des valeurs. Pour garantir que les entrées d’un utilisateur sont valides, ASP.NET fournit cinq contrôles de validation intégrées qui sont conçus pour être utilisés pour valider la valeur d’un contrôle d’entrée unique :

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) garantit qu’une valeur a été fournie.
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) valide une valeur par rapport à une autre valeur de contrôle Web ou une valeur constante ou garantit que le format de la valeur est légal pour un type de données spécifié
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) garantit qu’une valeur est dans une plage de valeurs
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) valide une valeur par rapport à un [expression régulière](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) valide une valeur par rapport à une méthode personnalisée, définie par l’utilisateur

Pour plus d’informations sur ces cinq contrôles, consultez le [section contrôles de Validation](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx) de la [didacticiels de démarrage rapide ASP.NET](https://asp.net/QuickStart/aspnet/).

Dans notre didacticiel, nous devons utiliser un contrôle RequiredFieldValidator dans les contrôle GridView et DetailsView `ProductName` TemplateField et un contrôle RequiredFieldValidator dans le DetailsView `UnitPrice` TemplateField. En outre, nous devons ajouter un CompareValidator pour les deux contrôles `UnitPrice` TemplateField qui garantit que le prix entré a une valeur supérieure ou égale à 0 et qu’il est présenté dans un format monétaire valide.

> [!NOTE]
> Alors que ASP.NET 1.x avait ces mêmes contrôles de validation de cinq, ASP.NET 2.0 a ajouté un certain nombre d’améliorations, la principale deux en cours d’un script côté client prend en charge les navigateurs autres qu’Internet Explorer et la possibilité de contrôles de validation de partition sur une page dans groupes de validation. Pour plus d’informations sur les nouvelles fonctionnalités de contrôle de validation de 2.0, reportez-vous à [dissection les contrôles de Validation dans ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Commençons par ajouter les contrôles de validation nécessaire à la `EditItemTemplate` s dans TemplateField le contrôle GridView. Pour ce faire, cliquez sur le lien Modifier les modèles à partir de la balise active du contrôle GridView pour afficher l’interface de modification de modèle. À ce stade, vous pouvez sélectionner le modèle à modifier dans la liste déroulante. Dans la mesure où nous voulons augmenter l’interface de modification, nous devons ajouter des contrôles de validation à la `ProductName` et `UnitPrice`de `EditItemTemplate` s.


[![Nous avons besoin étendre le ProductName et EditItemTemplates de UnitPrice](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image10.png)

**Figure 4**: nous devons étendre la `ProductName` et `UnitPrice`de `EditItemTemplate` s ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image12.png))


Dans le `ProductName` `EditItemTemplate`, ajoutez un contrôle RequiredFieldValidator en le faisant glisser à partir de la boîte à outils dans l’interface de modification de modèle, plaçant après la zone de texte.


[![Ajouter un contrôle RequiredFieldValidator à ProductName EditItemTemplate](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image13.png)

**Figure 5**: ajouter un contrôle RequiredFieldValidator à la `ProductName` `EditItemTemplate` ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image15.png))


Tous les contrôles de validation fonctionnent en validant l’entrée d’un contrôle Web ASP.NET unique. Par conséquent, nous devons indiquer que le contrôle RequiredFieldValidator que nous venons d’ajouter doit valider par rapport à la zone de texte dans le `EditItemTemplate`; cela s’effectue en définissant le contrôle de validation [propriété ControlToValidate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) à la `ID` du contrôle Web approprié. La zone de texte a actuellement plutôt apercevoir `ID` de `TextBox1`, mais nous allons lui donner un nom plus approprié. Cliquez sur la zone de texte dans le modèle et ensuite, à partir de la fenêtre Propriétés, changez le `ID` de `TextBox1` à `EditProductName`.


[![Modifier les ID de la zone de texte à EditProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image16.png)

**Figure 6**: modifier la zone de texte `ID` à `EditProductName` ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image18.png))


Ensuite, définissez le RequiredFieldValidator `ControlToValidate` propriété `EditProductName`. Enfin, définissez la [propriété ErrorMessage](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) à « Vous devez fournir le nom du produit » et le [propriété Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) à «\*». Le `Text` valeur de propriété, s’il est fourni, est le texte affiché par le contrôle de validation si la validation échoue. Le `ErrorMessage` valeur de propriété, qui est requis, est utilisé par le contrôle ValidationSummary ; si le `Text` valeur de propriété est omise, la `ErrorMessage` valeur de propriété est également le texte affiché par le contrôle de validation d’entrée non valide.

Après avoir défini ces trois propriétés de RequiredFieldValidator, votre écran doit ressembler à la Figure 7.


[![Définir le RequiredFieldValidator ControlToValidate, ErrorMessage et les propriétés de texte](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image19.png)

**Figure 7**: définir le RequiredFieldValidator `ControlToValidate`, `ErrorMessage`, et `Text` propriétés ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image21.png))


Avec le contrôle RequiredFieldValidator ajouté à la `ProductName` `EditItemTemplate`, que ne reste à ajouter la validation nécessaire à la `UnitPrice` `EditItemTemplate`. Étant donné que nous avons décidé que, pour cette page, le `UnitPrice` est facultatif quand modifier un enregistrement, nous n’avez pas besoin ajouter un contrôle RequiredFieldValidator. Nous avons, toutefois, doivent-ils ajouter un CompareValidator pour vous assurer que le `UnitPrice`, s’il est fourni, est correctement mis en forme comme une devise et est supérieur ou égal à 0.

Avant d’ajouter le contrôle CompareValidator pour les `UnitPrice` `EditItemTemplate`, nous allons tout d’abord modifier les ID du contrôle Web de la zone de texte à partir de `TextBox2` à `EditUnitPrice`. Après avoir apporté cette modification, ajoutez le contrôle CompareValidator, définissant son `ControlToValidate` propriété `EditUnitPrice`, ses `ErrorMessage` propriété à « le prix doit être supérieur ou égal à zéro et ne peut pas inclure le symbole monétaire » et son `Text` propriété "\*".

Pour indiquer que le `UnitPrice` valeur doit être supérieure ou égale à 0, définissez la CompareValidator [propriété Operator](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) à `GreaterThanEqual`, ses [propriété ValueToCompare](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) à « 0 » et ses [ Propriété de type](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) à `Currency`. La syntaxe déclarative suivante indique le `UnitPrice` de TemplateField `EditItemTemplate` après ces modifications ont été apportées :

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample3.aspx)]

Après avoir apporté ces modifications, ouvrez la page dans un navigateur. Si vous tentez d’omettre le nom ou entrez une valeur de prix non valide lors de la modification d’un produit, un astérisque apparaît en regard de la zone de texte. Comme le montre la Figure 8, une valeur de prix qui inclut le symbole monétaire tels que 19,95 $ est considéré comme non valide. Le CompareValidator `Currency` `Type` permettant de séparateurs de chiffres (par exemple, des virgules ou des points, selon les paramètres de culture) et un signe de début plus ou moins, contrairement à *pas* autoriser un symbole monétaire. Ce comportement peut perplex utilisateurs comme l’interface de modification affiche actuellement le `UnitPrice` en utilisant le format de devise.

> [!NOTE]
> N’oubliez pas que dans le *événements associés à l’insertion, mise à jour et suppression* didacticiel, nous définissons le BoundField `DataFormatString` propriété `{0:c}` pour mettre en forme comme une devise. En outre, nous définissons le `ApplyFormatInEditMode` true à la propriété, à l’origine de la GridView d’édition interface à mettre en forme le `UnitPrice` sous forme de devise. Lors de la conversion du BoundField en TemplateField, Visual Studio noter ces paramètres et mise en forme de la zone de texte `Text` propriété sous forme de devise à l’aide de la syntaxe de liaison de données `<%# Bind("UnitPrice", "{0:c}") %>`.


[![Un astérisque apparaît en regard des zones de texte avec une entrée non valide](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image22.png)

**Figure 8**: un astérisque apparaît à côté de zones de texte avec une entrée non valide ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image24.png))


Tandis que le fonctionnement de la validation en tant que-est, l’utilisateur doit supprimer manuellement le symbole monétaire lorsque vous modifiez un enregistrement qui n’est pas acceptable. Pour résoudre ce problème, nous avons trois options :

1. Configurer le `EditItemTemplate` afin que le `UnitPrice` valeur n’est pas mise en forme comme une devise.
2. Autoriser l’utilisateur à entrer un symbole monétaire en supprimant les CompareValidator et en la remplaçant par une RegularExpressionValidator vérifie correctement pour une valeur monétaire mise en forme correctement. Le problème ici est que l’expression régulière pour valider une valeur de devise n’est pas jolie et nécessiterait une écriture de code si nous souhaitons intégrer des paramètres de culture.
3. Supprimer le contrôle de validation complètement et s’appuient sur la logique de validation côté serveur dans le GridView `RowUpdating` Gestionnaire d’événements.

Revenons avec option #1 pour cet exercice. Actuellement le `UnitPrice` est mis en forme comme une devise en raison de l’expression de liaison de données pour la zone de texte dans le `EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`. Remplacez l’instruction de liaison par `Bind("UnitPrice", "{0:n2}")`, qui met en forme le résultat sous la forme un nombre avec deux chiffres de précision. Cela est possible directement par le biais de la syntaxe déclarative ou en cliquant sur le lien Modifier les DataBindings à partir de la `EditUnitPrice` zone de texte dans le `UnitPrice` de TemplateField `EditItemTemplate` (voir les Figures 9 et 10).


[![Cliquez sur le lien de modifier les DataBindings de la zone de texte](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image25.png)

**Figure 9**: cliquez sur le lien de modifier les DataBindings de la zone de texte ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image27.png))


[![Spécifiez le spécificateur de Format dans l’instruction de liaison](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image28.png)

**Figure 10**: spécifiez le spécificateur de Format dans le `Bind` instruction ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image30.png))


Avec cette modification, le prix de mise en forme dans l’interface de modification inclut des virgules comme séparateur de groupe et un point comme séparateur décimal, mais laisse de côté le symbole monétaire.

> [!NOTE]
> Le `UnitPrice` `EditItemTemplate` n’inclut pas un contrôle RequiredFieldValidator, ce qui permet la publication (postback) à surgir et la logique de mise à jour de commencer. Toutefois, le `RowUpdating` Gestionnaire d’événements copiée à partir de la *examinant les événements associés insertion, mise à jour et suppression* didacticiel inclut une vérification de programmation qui garantit que le `UnitPrice` est fourni. N’hésitez pas à supprimer cette logique, laissez ce champ dans-est, ou ajoutez un contrôle RequiredFieldValidator à la `UnitPrice` `EditItemTemplate`.


## <a name="step-4-summarizing-data-entry-problems"></a>Étape 4 : Synthèse des problèmes de saisie de données

Outre les contrôles de validation de cinq, ASP.NET propose le [contrôle ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), qui affiche le `ErrorMessage` s de ces contrôles de validation qui a détecté des données non valides. Ces données peuvent figurer sous forme de texte sur la page web ou via un messagebox modale, côté client. Nous allons améliorer ce didacticiel pour inclure un messagebox côté client résumant les problèmes de validation.

Pour ce faire, faites glisser un contrôle ValidationSummary à partir de la boîte à outils vers le concepteur. L’emplacement du contrôle de Validation n’a pas grande importance étant donné que nous allons configurer pour afficher uniquement le résumé en tant qu’un élément messagebox. Après avoir ajouté le contrôle, définissez son [ShowSummary, propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) à `False` et son [à la propriété ShowMessageBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) à `True`. Ainsi, les erreurs de validation sont résumées dans un messagebox côté client.


[![Les erreurs de Validation sont résumés dans Messagebox côté Client](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image31.png)

**Figure 11**: les erreurs de Validation sont résumés dans Messagebox côté Client ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image33.png))


## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>Étape 5 : Ajouter les contrôles de Validation pour le DetailsView`InsertItemTemplate`

Il reste pour ce didacticiel qu’à ajouter les contrôles de validation à l’interface de l’insertion de DetailsView. Le processus d’ajout de contrôles de validation pour les modèles de DetailsView est identique à celui examiné à l’étape 3 ; Par conséquent, nous allons breeze via la tâche dans cette étape. Comme nous l’avons fait avec le GridView `EditItemTemplate` s, je vous encourage à renommer le `ID` s des zones de texte à partir de l’apercevoir `TextBox1` et `TextBox2` à `InsertProductName` et `InsertUnitPrice`.

Ajoutez un contrôle RequiredFieldValidator à la `ProductName` `InsertItemTemplate`. Définir le `ControlToValidate` à la `ID` de la zone de texte dans le modèle, son `Text` propriété »\*» et ses `ErrorMessage` propriété à « Vous devez fournir le nom du produit ».

Dans la mesure où le `UnitPrice` est requis pour cette page lors de l’ajout d’un nouvel enregistrement, ajoutez un contrôle RequiredFieldValidator à la `UnitPrice` `InsertItemTemplate`, ce qui affecte ses `ControlToValidate`, `Text`, et `ErrorMessage` propriétés en conséquence. Enfin, ajoutez un CompareValidator pour le `UnitPrice` `InsertItemTemplate` configuration, son `ControlToValidate`, `Text`, `ErrorMessage`, `Type`, `Operator`, et `ValueToCompare` propriétés comme nous l’avons fait avec le `UnitPrice`de CompareValidator dans le GridView `EditItemTemplate`.

Après avoir ajouté ces contrôles de validation, un nouveau produit Impossible d’ajouter au système si son nom n’est pas fourni ou si son prix est un nombre négatif ou illégalement mis en forme.


[![La logique de validation a été ajoutée à l’Interface d’insertion de DetailsView](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image34.png)

**Figure 12**: logique de Validation a été ajoutée à l’Interface d’insertion de DetailsView ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image36.png))


## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>Étape 6 : Partitionnement les contrôles de Validation des groupes de Validation

Notre page se compose de deux jeux logiquement disparates de contrôles de validation : ceux qui correspondent au GridView d’édition interface et celles qui correspondent au contrôle DetailsView de l’insertion d’interface. Par défaut, lors d’une publication *tous les* des contrôles de validation sur la page sont vérifiées. Toutefois, lorsque vous modifiez un enregistrement nous ne voulons pas les contrôles de validation de l’interface de DetailsView insertion à valider. Figure 13 illustre notre dilemme actuel quand un utilisateur modifie un produit avec des valeurs parfaitement légales, mise à jour en cliquant sur entraîne une erreur de validation, car les valeurs de nom et le prix de l’interface d’insertion sont vides.


[![La mise à jour d’un produit entraîne des contrôles de Validation d’insertion de l’Interface à incendie](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image37.png)

**Figure 13**: la mise à jour d’un produit entraîne des contrôles de Validation de l’Interface insertion à déclencher ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image39.png))


Les contrôles de validation dans ASP.NET 2.0 peuvent être partitionnés en groupes de contrôle via leur `ValidationGroup` propriété. Pour associer un ensemble de contrôles de validation dans un groupe, il suffit de définir leurs `ValidationGroup` à la même valeur de propriété. Pour notre didacticiel, définissez la `ValidationGroup` propriétés des contrôles de validation dans TemplateField le contrôle GridView à `EditValidationControls` et `ValidationGroup` propriétés de TemplateField de DetailsView à `InsertValidationControls`. Ces modifications peuvent être effectuées directement dans le balisage déclaratif ou via la fenêtre Propriétés lors de l’utilisation du concepteur modifier l’interface de modèle.

En plus de la validation des contrôles, le bouton et liés à un bouton dans ASP.NET 2.0 incluent également un `ValidationGroup` propriété. Les validateurs d’un groupe de validation sont vérifiés pour validité uniquement lorsqu’une publication (postback) est induit par un bouton qui a le même `ValidationGroup` paramètre de propriété. Par exemple, dans l’ordre pour un bouton Insérer de DetailsView déclencher le `InsertValidationControls` groupe de validation que nous devons définir la CommandField `ValidationGroup` propriété `InsertValidationControls` (voir Figure 14). En outre, définissez le GridView de CommandField `ValidationGroup` propriété `EditValidationControls`.


[![Propriété de ValidationGroup de CommandField à InsertValidationControls de l’ensemble le contrôle DetailsView](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image40.png)

**Figure 14**: définir le DetailsView de CommandField `ValidationGroup` propriété `InsertValidationControls` ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image42.png))


Après ces modifications, le contrôle DetailsView de GridView TemplateField et CommandFields doit ressembler à ce qui suit :

Le DetailsView TemplateField et CommandField

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample4.aspx)]

Le contrôle GridView CommandField et TemplateField

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample5.aspx)]

À ce stade les contrôles de validation de modification spécifiques déclenchent uniquement lorsque le contrôle GridView mise à jour bouton et l’incendie de contrôles de validation d’insertion spécifiques uniquement lors de DetailsView insertion du bouton, résolution du problème mises en évidence par la Figure 13. Toutefois, avec cette modification notre contrôle ValidationSummary n’affiche plus lors de la saisie des données non valides. Le contrôle ValidationSummary contient également un `ValidationGroup` propriété et affiche uniquement les informations résumées pour les contrôles de validation dans son groupe de validation. Par conséquent, nous devons disposer de deux contrôles de validation dans cette page, un pour le `InsertValidationControls` groupe de validation et l’autre pour `EditValidationControls`.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample6.aspx)]

Avec cet ajout notre didacticiel est terminé !

## <a name="summary"></a>Récapitulatif

BoundFields peuvent contribuer à la fois une interface d’insertion et d’édition, l’interface n’est pas personnalisable. En général, nous souhaitons ajouter des contrôles de validation à la modification et l’insertion d’interface pour vous assurer que l’utilisateur entre les entrées requises dans un format juridique. Pour ce faire, nous devons convertir les BoundFields en TemplateField et ajouter les contrôles de validation pour les modèles appropriés. Dans ce didacticiel, nous avons étendu l’exemple à partir de la *examinant les événements associés insertion, mise à jour et suppression* didacticiel en ajoutant des contrôles de validation pour les deux le contrôle DetailsView de l’insertion d’interface et le contrôle GridView interface de modification. En outre, nous avons vu comment afficher les informations de résumé de validation à l’aide du contrôle ValidationSummary et comment partitionner les contrôles de validation sur la page groupes de contrôle distinctes.

Comme nous l’avons vu dans ce didacticiel, TemplateField autorise les interfaces d’éditions et insertion être augmentés pour inclure les contrôles de validation. TemplateField peut également être étendu pour inclure les contrôles Web d’entrée supplémentaires, l’activation de la zone de texte à remplacer par un contrôle Web plus adapté. Dans notre didacticiel suivant, nous allons voir comment remplacer le contrôle TextBox avec un contrôle DropDownList lié aux données, ce qui est idéal lors de la modification d’une clé étrangère (comme `CategoryID` ou `SupplierID` dans le `Products` table).

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Liz Shulok et Zack Jones. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
> [Suivant](customizing-the-data-modification-interface-vb.md)
