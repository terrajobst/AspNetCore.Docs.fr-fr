---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
title: Utilisation de TemplateField dans le contrôle GridView (VB) | Microsoft Docs
author: rick-anderson
description: Pour fournir la flexibilité, le contrôle GridView offre le TemplateField contenu, qui effectue le rendu à l’aide d’un modèle. Un modèle peut inclure une combinaison de code HTML statique, des contrôles Web, et...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: a92cd6ed-609a-4e40-ad23-004b54afd436
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 6090c97e12736fb83938e9da5991d19ed50463b6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837483"
---
<a name="using-templatefields-in-the-gridview-control-vb"></a>Utilisation de TemplateField dans le contrôle GridView (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_12_VB.exe) ou [télécharger le PDF](using-templatefields-in-the-gridview-control-vb/_static/datatutorial12vb1.pdf)

> Pour fournir la flexibilité, le contrôle GridView offre le TemplateField contenu, qui effectue le rendu à l’aide d’un modèle. Un modèle peut inclure une combinaison de code HTML statique, contrôles Web et syntaxe de liaison de données. Dans ce didacticiel, nous allons examiner comment utiliser le TemplateField pour atteindre un degré de personnalisation avec le contrôle GridView supérieur.


## <a name="introduction"></a>Introduction

Le contrôle GridView se compose d’un ensemble de champs qui indiquent quelles propriétés à partir de la `DataSource` doivent être incluses dans la sortie rendue, ainsi que la façon dont les données seront affichera. Le type de champ la plus simple est BoundField, qui affiche une valeur de données sous forme de texte. Autres types de champs affichent les données à l’aide d’autres éléments HTML. Le CheckBoxField, par exemple, rendu sous la forme d’une case à cocher dont l’état activé dépend de la valeur d’un champ de données spécifié ; l’ImageField restitue une image dont la source image est basée sur un champ de données spécifié. Liens hypertexte et boutons dont l’état varie selon une valeur de champ de données sous-jacente peut être rendu à l’aide des types de champs HyperLinkField et ButtonField.

Alors que les types de champs CheckBoxField, ImageField, HyperLinkField et ButtonField permettent une autre vue des données, ils sont toujours assez limités en ce qui concerne la mise en forme. Un CheckBoxField peut uniquement afficher une case à cocher unique, tandis qu’un ImageField peut uniquement afficher une image unique. Que se passe-t-il si un champ particulier doit afficher un texte, une case à cocher, *et* une image, tout en fonction des valeurs de champ de données différentes ? Ou bien, que se passe-t-il si nous souhaitions afficher les données à l’aide d’un contrôle Web autre que la case à cocher, une Image, un lien hypertexte ou un bouton ? En outre, le BoundField limite son affichage à un seul champ de données. Que se passe-t-il si nous souhaitons afficher deux ou plusieurs valeurs de champ de données dans une seule colonne de GridView ?

Pour prendre en charge ce niveau de flexibilité, le contrôle GridView offre TemplateField, qui est restitué par un *modèle*. Un modèle peut inclure une combinaison de code HTML statique, contrôles Web et syntaxe de liaison de données. En outre, le TemplateField contenu a un large éventail de modèles qui peut être utilisé pour personnaliser le rendu des situations différentes. Par exemple, le `ItemTemplate` est utilisé par défaut pour rendre la cellule pour chaque ligne, mais la `EditItemTemplate` modèle peut être utilisé pour personnaliser l’interface lors de la modification des données.

Dans ce didacticiel, nous allons examiner comment utiliser le TemplateField pour atteindre un degré de personnalisation avec le contrôle GridView supérieur. Dans le [didacticiel précédent](custom-formatting-based-upon-data-vb.md) nous avons vu comment personnaliser le formatage basé sur les données sous-jacentes à l’aide du `DataBound` et `RowDataBound` gestionnaires d’événements. Une autre façon de personnaliser le formatage basé sur les données sous-jacentes est par l’appel de méthodes de formatage d’au sein d’un modèle. Nous allons examiner cette technique dans ce didacticiel.

Pour ce didacticiel, nous allons utiliser TemplateField pour personnaliser l’apparence d’une liste d’employés. Plus précisément, nous allons répertorier tous les employés, mais affiche l’employé prénoms et noms dans une colonne, leur date d’embauche dans un contrôle calendrier et une colonne d’état qui indique le nombre de jours qu’ils ont jusqu'à présent été employées dans la société.


[![Trois TemplateField est utilisés pour personnaliser l’affichage](using-templatefields-in-the-gridview-control-vb/_static/image2.png)](using-templatefields-in-the-gridview-control-vb/_static/image1.png)

**Figure 1**: TemplateField trois sont utilisées pour personnaliser l’affichage ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-vb/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-gridview"></a>Étape 1 : Liaison des données pour le contrôle GridView

Dans les scénarios où vous devez utiliser TemplateField pour personnaliser l’apparence des États, je trouve plus facile de commencer en créant un contrôle GridView qui contient juste BoundFields tout d’abord, puis pour ajouter des nouvelles TemplateField ou convertir le BoundFields existant pour TemplateField en fonction des besoins. Par conséquent, nous allons démarrer ce didacticiel en ajoutant un GridView à la page via le concepteur et en liant à un ObjectDataSource qui retourne la liste des employés. Ces étapes créera un GridView avec BoundFields pour chacun des champs employee.

Ouvrez le `GridViewTemplateField.aspx` page et faites glisser un GridView à partir de la boîte à outils vers le concepteur. À partir de la balise active le contrôle GridView choisir d’ajouter un nouveau contrôle ObjectDataSource qui appelle le `EmployeesBLL` la classe `GetEmployees()` (méthode).


[![Ajouter un nouveau contrôle ObjectDataSource qui appelle la méthode GetEmployees()](using-templatefields-in-the-gridview-control-vb/_static/image5.png)](using-templatefields-in-the-gridview-control-vb/_static/image4.png)

**Figure 2**: ajouter un nouveau contrôle ObjectDataSource ce Invoke le `GetEmployees()` (méthode) ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-vb/_static/image6.png))


Le contrôle GridView de liaison de cette manière ajoute automatiquement un BoundField pour chacune des propriétés de l’employé : `EmployeeID`, `LastName`, `FirstName`, `Title`, `HireDate`, `ReportsTo`, et `Country`. Pour ce rapport nous allons pas se préoccuper affichant le `EmployeeID`, `ReportsTo`, ou `Country` propriétés. Pour supprimer ces BoundFields, vous pouvez :

- Utilisez la boîte de dialogue champs, cliquez sur le lien Modifier les colonnes à partir de la balise active le contrôle GridView pour afficher cette boîte de dialogue. Ensuite, sélectionnez les BoundFields à partir de l’angle inférieur gauche de liste et cliquez sur le X rouge bouton pour supprimer le BoundField.
- Modifier la syntaxe déclarative du contrôle GridView manuellement à partir de la vue de Source, supprimez le `<asp:BoundField>` élément pour le BoundField que vous souhaitez supprimer.

Après avoir supprimé le `EmployeeID`, `ReportsTo`, et `Country` BoundFields, balisage de votre contrôle GridView doit ressembler à :


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample1.aspx)]

Prenez un moment pour consulter notre progression dans un navigateur. À ce stade, vous devez voir une table avec un enregistrement de chaque employé et quatre colonnes : une pour l’employé nom, un pour son prénom, un titre et un pour leur date d’embauche.


[![LastName, FirstName, titre et HireDate les champs sont affichés pour chaque employé](using-templatefields-in-the-gridview-control-vb/_static/image8.png)](using-templatefields-in-the-gridview-control-vb/_static/image7.png)

**Figure 3**: le `LastName`, `FirstName`, `Title`, et `HireDate` champs sont affichés pour chaque employé ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-vb/_static/image9.png))


## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>Étape 2 : Afficher le prénom et le nom dans une seule colonne

Actuellement, chaque employé du premier et dernier noms sont affichés dans une colonne distincte. Il peut être intéressant de les combiner en une seule colonne à la place. Pour ce faire, nous devons utiliser TemplateField. Nous pouvons soit ajouter un nouveau TemplateField, ajouter le balisage nécessaire et la syntaxe de liaison de données, puis supprimez le `FirstName` et `LastName` BoundFields, ou nous pouvons convertir les `FirstName` BoundField en TemplateField, modifier le TemplateField contenu à inclure le `LastName` valeur, puis supprimez le `LastName` BoundField.

Les deux approches net le même résultat, mais personnellement, j’aime convertir BoundFields TemplateField lorsque cela est possible, car la conversion ajoute automatiquement un `ItemTemplate` et `EditItemTemplate` avec les contrôles Web et la syntaxe de liaison de données afin de reproduire l’apparence et les fonctionnalités de la BoundField. L’avantage est que nous allons devoir faire moins avec le TemplateField contenu comme le processus de conversion ont fera partie du travail pour nous.

Pour convertir un BoundField existant en TemplateField, cliquez sur le lien Modifier les colonnes à partir de la balise active le contrôle GridView, afficher la boîte de dialogue champs. Sélectionnez le BoundField convertir à partir de la liste dans l’angle inférieur gauche, puis cliquez sur le lien « Convertir ce champ en TemplateField » dans l’angle inférieur droit.


[![Convertir un BoundField en TemplateField à partir de la boîte de dialogue champs](using-templatefields-in-the-gridview-control-vb/_static/image11.png)](using-templatefields-in-the-gridview-control-vb/_static/image10.png)

**Figure 4**: convertir un BoundField en TemplateField de contenu à partir de la boîte de dialogue champs ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-vb/_static/image12.png))


Continuez et convertir le `FirstName` BoundField en TemplateField. Après cette modification il n’existe aucune différence perceptive dans le concepteur. Il s’agit, car conversion le BoundField en TemplateField crée un TemplateField qui tient à jour l’apparence de la BoundField. Malgré l’existence d’aucune différence visuelle à ce stade dans le concepteur, ce processus de conversion a remplacé la syntaxe déclarative de la BoundField - `<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />` - avec la syntaxe TemplateField suivante :


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample2.aspx)]

Comme vous pouvez le voir, le TemplateField contenu se compose des deux modèles un `ItemTemplate` qui a une étiquette dont `Text` propriété est définie sur la valeur de la `FirstName` champ de données et un `EditItemTemplate` avec une zone de texte du contrôle dont `Text` est également définie pour le `FirstName` champ de données. La syntaxe de liaison de données - `<%# Bind("fieldName") %>` -indique que le champ de données *`fieldName`* est lié à la propriété de contrôle Web spécifiée.

Pour ajouter le `LastName` valeur à cette TemplateField, nous devons ajouter un autre contrôle Web de l’étiquette de champ de données le `ItemTemplate` et lier sa `Text` propriété `LastName`. Cela peut être accompli manuellement ou via le concepteur. Pour cela à la main, ajoutez simplement la syntaxe déclarative appropriée pour le `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample3.aspx)]

Pour l’ajouter via le concepteur, cliquez sur le lien Modifier les modèles à partir de la balise active le contrôle GridView. Ceci affichera une interface de modification de modèle du contrôle GridView. Dans cette balise active de l’interface est une liste des modèles dans le contrôle GridView. Étant donné que nous avons un TemplateField à ce stade, seuls les modèles répertoriés dans la liste déroulante sont ces modèles pour la `FirstName` TemplateField avec le `EmptyDataTemplate` et `PagerTemplate`. Le `EmptyDataTemplate` modèle, si spécifié, est utilisé pour restituer la sortie du contrôle GridView s’il en existe aucun résultat dans les données liées au GridView ; le `PagerTemplate`, si spécifiée, est utilisé pour restituer l’interface de pagination pour un GridView qui prend en charge la pagination.


[![Modèles le contrôle GridView peuvent être modifiées via le Concepteur](using-templatefields-in-the-gridview-control-vb/_static/image14.png)](using-templatefields-in-the-gridview-control-vb/_static/image13.png)

**Figure 5**: peut être modifié via le concepteur le contrôle GridView de modèles ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-vb/_static/image15.png))


Affiche également le `LastName` dans le `FirstName` TemplateField faites glisser le contrôle d’étiquette de la boîte à outils dans le `FirstName` de TemplateField `ItemTemplate` dans le contrôle GridView de modification de modèle interface.


[![Ajouter un contrôle Web d’étiquette à ItemTemplate de la FirstName TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image17.png)](using-templatefields-in-the-gridview-control-vb/_static/image16.png)

**Figure 6**: ajouter un contrôle étiquette à la `FirstName` ItemTemplate de TemplateField ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-vb/_static/image18.png))


À ce stade le contrôle d’étiquette Web ajouté à la TemplateField a son `Text` propriété définie sur « Label ». Nous devons modifier cela pour que cette propriété est liée à la valeur de la `LastName` à la place du champ de données. Pour accomplir ce clic sur la balise active du contrôle d’étiquette et choisissez l’option Modifier les DataBindings.


[![Choisissez l’Option modifier les DataBindings à partir de la balise active de l’étiquette](using-templatefields-in-the-gridview-control-vb/_static/image20.png)](using-templatefields-in-the-gridview-control-vb/_static/image19.png)

**Figure 7**: choisissez l’Option DataBindings modifier à partir de la balise active de l’étiquette ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-vb/_static/image21.png))


Cela fera apparaître la boîte de dialogue DataBindings. À partir de là, vous pouvez sélectionner la propriété de participer dans la liaison de données à partir de la liste sur la gauche, puis choisissez le champ pour lier les données à partir de la liste déroulante de droite. Choisissez le `Text` propriété à partir de la gauche et le `LastName` champ de droite et cliquez sur OK.


[![Lier la propriété de texte au champ de données de LastName](using-templatefields-in-the-gridview-control-vb/_static/image23.png)](using-templatefields-in-the-gridview-control-vb/_static/image22.png)

**Figure 8**: lier le `Text` propriété le `LastName` champ de données ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-vb/_static/image24.png))


> [!NOTE]
> La boîte de dialogue DataBindings vous permet d’indiquer si vous souhaitez effectuer la liaison de données bidirectionnelle. Si vous laissez cette option est désactivée, la syntaxe de liaison de données `<%# Eval("LastName")%>` sera utilisé à la place de `<%# Bind("LastName")%>`. Chacune de ces approches convient parfaitement pour ce didacticiel. Liaison de données bidirectionnelle devient important lors de l’insertion et la modification des données. Pour simplement afficher les données, toutefois, chacune de ces approches fonctionne tout aussi bien. Nous allons aborder la liaison de données bidirectionnelle en détail dans les didacticiels futures.


Prenez un moment pour afficher cette page via un navigateur. Comme vous pouvez le voir, le contrôle GridView inclut toujours les quatre colonnes ; Toutefois, le `FirstName` colonne répertorie maintenant *à la fois* le `FirstName` et `LastName` valeurs de champ de données.


[![Valeurs à le FirstName et LastName sont affichés dans une seule colonne](using-templatefields-in-the-gridview-control-vb/_static/image26.png)](using-templatefields-in-the-gridview-control-vb/_static/image25.png)

**Figure 9**: à la fois le `FirstName` et `LastName` valeurs figurent dans une seule colonne ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-vb/_static/image27.png))


Pour effectuer cette première étape, vous devez supprimer le `LastName` BoundField et renommer le `FirstName` de TemplateField `HeaderText` propriété sur « Name ». Après ces modifications balisage déclaratif de GridView doit ressembler à ce qui suit :


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample4.aspx)]


[![Premier de chaque employé et les noms sont affichés dans une colonne](using-templatefields-in-the-gridview-control-vb/_static/image29.png)](using-templatefields-in-the-gridview-control-vb/_static/image28.png)

**Figure 10**: prénoms et noms de chaque employé sont affichés dans une colonne ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-vb/_static/image30.png))


## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>Étape 3 : Utilisation du contrôle de calendrier à afficher le`HiredDate`champ

Affichage d’une valeur de champ de données sous forme de texte dans un GridView est aussi simple que d’utiliser un BoundField. Pour certains scénarios, cependant, les données sont mieux exprimées à l’aide d’un contrôle Web particulier plutôt que du texte uniquement. Ce type de personnalisation de l’affichage de données est possible avec TemplateField. Par exemple, plutôt que d’afficher la date d’embauche sous forme de texte, nous avons pu montrer un calendrier (à l’aide de [le contrôle Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) avec leur date d’embauche mis en surbrillance.

Pour ce faire, commencez par convertir la `HiredDate` BoundField en TemplateField. Simplement, accédez à la balise active le contrôle GridView et cliquez sur le lien Modifier les colonnes, afficher la boîte de dialogue champs. Sélectionnez le `HiredDate` BoundField et cliquez sur « convertissent ce champ en TemplateField. »


[![Convertir le HiredDate BoundField en TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image32.png)](using-templatefields-in-the-gridview-control-vb/_static/image31.png)

**Figure 11**: convertir la `HiredDate` BoundField dans un TemplateField ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-vb/_static/image33.png))


Comme nous l’avons vu à l’étape 2, cela remplacera le BoundField TemplateField contenant un `ItemTemplate` et `EditItemTemplate` avec une étiquette et une zone de texte dont `Text` propriétés sont liées à la `HiredDate` valeur à l’aide de la syntaxe de liaison de données `<%# Bind("HiredDate")%>`.

Pour remplacer le texte avec un contrôle de calendrier, modifier le modèle en supprimant l’étiquette et en ajoutant un contrôle calendrier. Dans le concepteur, sélectionnez Modifier les modèles à partir de la balise active le contrôle GridView et choisissez le `HireDate` de TemplateField `ItemTemplate` dans la liste déroulante. Ensuite, supprimez le contrôle d’étiquette et faites glisser un contrôle de calendrier à partir de la boîte à outils dans l’interface de modification de modèle.


[![Ajouter un contrôle de calendrier à la HireDate ItemTemplate de TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image35.png)](using-templatefields-in-the-gridview-control-vb/_static/image34.png)

**Figure 12**: ajouter un contrôle de calendrier à la `HireDate` de TemplateField `ItemTemplate` ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-vb/_static/image36.png))


À ce stade, chaque ligne dans le contrôle GridView contiendra un contrôle calendrier dans son `HiredDate` TemplateField. Néanmoins, l’employé du réel `HiredDate` valeur n’est pas définie n’importe où dans le contrôle de calendrier, à l’origine de chaque contrôle de calendrier par défaut à la présentation de la date et le mois en cours. Pour résoudre ce problème, nous devons affecter chaque employé `HiredDate` pour le contrôle Calendar [SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx) et [VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx) propriétés.

À partir de la balise active du contrôle de calendrier, choisissez Modifier les DataBindings. Ensuite, liez les deux `SelectedDate` et `VisibleDate` propriétés pour le `HiredDate` champ de données.


[![Lier la propriété SelectedDate et VisibleDate propriétés au champ de données de HiredDate](using-templatefields-in-the-gridview-control-vb/_static/image38.png)](using-templatefields-in-the-gridview-control-vb/_static/image37.png)

**Figure 13**: lier le `SelectedDate` et `VisibleDate` propriétés pour le `HiredDate` champ de données ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-vb/_static/image39.png))


> [!NOTE]
> Date sélectionnée du contrôle de calendrier ne doit pas nécessairement être visible. Par exemple, un calendrier peut avoir le 1er août<sup>st</sup>, 1999 comme la date sélectionnée, mais être montrant le mois en cours et l’année. Les dates sélectionnées et visibles sont spécifiées par le contrôle Calendar `SelectedDate` et `VisibleDate` propriétés. Dans la mesure où nous voulons pour sélectionner les deux éléments de l’employé `HiredDate` et assurez-vous qu’elle est affichée nous devons lier ces deux propriétés pour le `HireDate` champ de données.


Lorsque vous affichez la page dans un navigateur, le calendrier affiche le mois de date d’embauche de l’employé maintenant et sélectionne cette date.


[![HiredDate l’employé est indiqué dans le contrôle Calendar](using-templatefields-in-the-gridview-control-vb/_static/image41.png)](using-templatefields-in-the-gridview-control-vb/_static/image40.png)

**Figure 14**: l’employé `HiredDate` est indiqué dans le contrôle calendrier ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-vb/_static/image42.png))


> [!NOTE]
> Contrairement à tous les exemples que nous avons vu jusqu'à présent, pour ce didacticiel que nous l’avons fait *pas* définir `EnableViewState` propriété `False` pour ce GridView. La raison de cette décision est car en cliquant sur les dates du contrôle Calendar entraîne une publication (postback), en définissant la date sélectionnée du calendrier à la date cliquée simplement. Si l’état d’affichage du contrôle GridView est désactivé, toutefois, sur chaque publication (postback) les données de GridView sont de nouveau liées à sa source de données sous-jacente, ce qui provoque la date sélectionnée du calendrier à définir *retour* à l’employé `HireDate`, remplacement la date choisie par l’utilisateur.


Pour ce didacticiel il s’agit une discussion plus discutable étant donné que l’utilisateur n’est pas en mesure de mettre à jour de l’employé `HireDate`. Il serait probablement préférable de configurer le contrôle de calendrier afin que ses dates ne sont pas sélectionnables. Malgré tout, ce didacticiel montre que, dans certains cas, l’état d’affichage doit être activée afin de fournir certaines fonctionnalités.

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>Étape 4 : Montrant le nombre de jours pendant lesquels l’employé a travaillé pour l’entreprise

Jusqu'à présent, nous avons vu deux applications de TemplateFields :

- Combinaison de deux ou plusieurs valeurs de champ de données en une seule colonne, et
- Exprimer une valeur de champ de données à l’aide d’un contrôle Web au lieu de texte

Une troisième utilisation de TemplateField est dans l’affichage des métadonnées concernant le GridView données sous-jacentes. En plus d’afficher les dates d’embauche des employés, par exemple, nous pouvons également avoir une colonne qui affiche le nombre de jours total elles ont été lors de la tâche.

Encore une autre utilisation de TemplateField se scénarios lorsque les données sous-jacentes doivent s’afficher différemment dans le rapport de la page web que dans le format, il est stocké dans la base de données. Imaginez que le `Employees` table comportait un `Gender` champ stockés le caractère `M` ou `F` pour indiquer le sexe de l’employé. Lors de l’affichage de ces informations dans une page web que nous voulons afficher le sexe, en tant que « Masculin » ou « Féminin », plutôt que de simplement « M » ou « F ».

Ces deux scénarios peuvent être gérés en créant un *mise en forme de la méthode* dans la classe de code-behind de la page ASP.NET (ou dans une bibliothèque de classes distincte, implémentée comme un `Shared` méthode) qui est appelé à partir du modèle. Une telle méthode de mise en forme est appelée à partir du modèle à l’aide de la même syntaxe de liaison de données vue précédemment. La méthode de mise en forme peut prendre dans n’importe quel nombre de paramètres, mais il doit renvoyer une chaîne. Cette chaîne retournée est le code HTML qui est injecté dans le modèle.

Pour illustrer ce concept, nous allons augmenter notre didacticiel pour afficher une colonne qui répertorie le nombre total de jours pendant lesquels qu'un employé travaille sur la tâche. Cette méthode de mise en forme prendra un `Northwind.EmployeesRow` de l’objet et retourner le nombre de jours pendant lesquels l’employé a été utilisé en tant que chaîne. Cette méthode peut être ajoutée à la classe de code-behind de la page ASP.NET, mais *doit* être marqué comme `Protected` ou `Public` afin d’être accessible à partir du modèle.


[!code-vb[Main](using-templatefields-in-the-gridview-control-vb/samples/sample5.vb)]

Dans la mesure où le `HiredDate` champ peut contenir `NULL` nous devons tout d’abord nous assurer que la valeur n’est pas de valeurs de base de données `NULL` avant de poursuivre le calcul. Si le `HiredDate` valeur est `NULL`, nous renvoyons simplement la chaîne « Unknown » ; si elle n’est pas `NULL`, nous calculons la différence entre l’heure actuelle et la `HiredDate` valeur et retourner le nombre de jours.

Pour utiliser cette méthode, nous devons appeler à partir d’un TemplateField contenu dans le contrôle GridView à l’aide de la syntaxe de liaison de données. Commencez par ajouter un nouveau TemplateField au GridView en cliquant sur le lien Modifier les colonnes dans la balise active le contrôle GridView et en ajoutant un TemplateField nouvelle.


[![Ajouter un nouveau TemplateField au GridView](using-templatefields-in-the-gridview-control-vb/_static/image44.png)](using-templatefields-in-the-gridview-control-vb/_static/image43.png)

**Figure 15**: ajouter un nouveau TemplateField au GridView ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-vb/_static/image45.png))


Définir cette nouvelle TemplateField contenu `HeaderText` propriété à « Jours sur du travail » et ses `ItemStyle`de `HorizontalAlign` propriété `Center`. Pour appeler le `DisplayDaysOnJob` (méthode) à partir du modèle, ajoutez un `ItemTemplate` et utilisez la syntaxe de liaison de données suivante :


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample6.aspx)]

`Container.DataItem` Retourne un `DataRowView` objet correspondant à la `DataSource` enregistrement lié à la `GridViewRow`. Son `Row` propriété retourne fortement typées `Northwind.EmployeesRow`, qui est transmis à la `DisplayDaysOnJob` (méthode). Cette syntaxe de liaison de données peut apparaître directement dans le `ItemTemplate` (comme indiqué dans la syntaxe déclarative ci-dessous) ou peut être affectée à la `Text` propriété d’un contrôle Web Label.

> [!NOTE]
> Ou bien, au lieu de passer un `EmployeesRow` instance, nous aurions pu simplement transmettre le `HireDate` à l’aide de la valeur `<%# DisplayDaysOnJob(Eval("HireDate")) %>`. Toutefois, le `Eval` méthode retourne un `Object`, de sorte que nous devons modifier notre `DisplayDaysOnJob` signature de méthode pour accepter un paramètre d’entrée de type `Object`, à la place. Nous ne pouvons pas aveuglément converti le `Eval("HireDate")` appel à un `DateTime` , car le `HireDate` colonne dans le `Employees` table peut contenir `NULL` valeurs. Par conséquent, nous devons l’accepter un `Object` comme paramètre d’entrée pour le `DisplayDaysOnJob` (méthode), vérifiez si elle avait une base de données `NULL` valeur (qui peut être accompli en utilisant `Convert.IsDBNull(objectToCheck)`) et puis agissez en conséquence.


En raison de ces subtilités, j’ai opté pour passer dans l’ensemble `EmployeesRow` instance. Dans le didacticiel suivant, nous allons voir un exemple de l’ajustement plus pour l’utilisation de la `Eval("columnName")` syntaxe pour la transmission d’un paramètre d’entrée dans une méthode de mise en forme.

L’exemple suivant montre la syntaxe déclarative pour notre GridView après le TemplateField contenu a été ajouté et le `DisplayDaysOnJob` méthode appelée à partir de la `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample7.aspx)]

Figure 16 illustre la fin du didacticiel, lorsqu’ils sont affichés via un navigateur.


[![Le nombre de jours de que l’employé travaille sur le travail s’affiche.](using-templatefields-in-the-gridview-control-vb/_static/image47.png)](using-templatefields-in-the-gridview-control-vb/_static/image46.png)

**Figure 16**: le nombre de jours de l’employé a été lors de la tâche s’affiche ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-gridview-control-vb/_static/image48.png))


## <a name="summary"></a>Récapitulatif

Le TemplateField contenu dans le contrôle GridView permet un degré plus élevé de flexibilité dans l’affichage des données que celui offert par les autres contrôles de champ. TemplateField est idéales pour les situations où :

- Plusieurs champs de données doivent être affichés dans une colonne GridView
- Les données sont mieux exprimées à l’aide d’un contrôle Web plutôt que texte brut
- La sortie varie selon les données sous-jacentes, telles que l’affichage des métadonnées ou de reformater les données

En plus de la personnalisation de l’affichage de données, TemplateField est également utilisés pour personnaliser les interfaces utilisateur utilisés pour l’édition et insertion de données, comme nous allons le voir dans les futures didacticiels.

Les deux didacticiels continuent à Explorer les modèles, en commençant par un coup de œil à l’utilisation de TemplateField dans un contrôle DetailsView. Ensuite, nous vont se tourner vers le contrôle FormView, qui utilise des modèles à la place de champs pour fournir une plus grande flexibilité dans la disposition et la structure des données.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Dan Jagers. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](custom-formatting-based-upon-data-vb.md)
> [Suivant](using-templatefields-in-the-detailsview-control-vb.md)
