---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
title: Une vue d’ensemble de la modification et suppression de données dans le contrôle DataList (VB) | Microsoft Docs
author: rick-anderson
description: Tandis que le contrôle DataList ne dispose pas de modification intégrée et la suppression de fonctionnalités, que vous dans ce didacticiel, nous allons voir comment créer un contrôle DataList qui prend en charge la modification et suppression o...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 9410a23c-9697-4f07-bd71-e62b0ceac655
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: b1c7834e67f7682f82ecd0b2b5140260d104aecc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838619"
---
<a name="an-overview-of-editing-and-deleting-data-in-the-datalist-vb"></a>Une vue d’ensemble de la modification et suppression de données dans le contrôle DataList (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_VB.exe) ou [télécharger le PDF](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/datatutorial36vb1.pdf)

> Tandis que le contrôle DataList ne dispose pas de modification intégrée et la suppression de fonctionnalités, que vous dans ce didacticiel, nous allons voir comment créer un contrôle DataList qui prend en charge la modification et suppression de ses données sous-jacentes.


## <a name="introduction"></a>Introduction

Dans le [une vue d’ensemble d’insertion, mise à jour et suppression des données](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) didacticiel, nous avons vu comment insérer, mettre à jour et supprimer des données à l’aide de l’architecture d’application, un ObjectDataSource et GridView, DetailsView et FormView contrôles. Implémentation des interfaces de modification de données simple avec ObjectDataSource et ces trois données Web, était un composant logiciel enfichable et impliqué cochant simplement une case à cocher à partir d’une balise active. Aucun code n’est nécessaire pour être écrites.

Malheureusement, le contrôle DataList ne dispose pas intégrés, modification et suppression de fonctionnalités inhérentes dans le contrôle GridView. Cette fonctionnalité manquante est due en partie au fait que le contrôle DataList est un relic depuis la version précédente d’ASP.NET, lorsque les contrôles de source de données déclarative et pages de modification de données sans code n’étaient pas disponibles. Tandis que le contrôle DataList dans ASP.NET 2.0 n’offre pas les mêmes prêts à l’emploi des fonctionnalités de modification de données en tant que le contrôle GridView, nous pouvons utiliser ASP.NET 1.x techniques pour inclure ce type de fonctionnalité. Cette approche nécessite un peu de code, mais, comme nous le verrons dans ce didacticiel, le contrôle DataList a certaines propriétés et événements en place afin de faciliter ce processus.

Dans ce didacticiel, nous allons voir comment créer un contrôle DataList qui prend en charge la modification et suppression de ses données sous-jacentes. Didacticiels futures examinerons plus avancés de modification et suppression des scénarios, y compris la validation de champ d’entrée, normalement de gestion des exceptions déclenchées à partir de l’accès aux données ou couches de logique métier et ainsi de suite.

> [!NOTE]
> Comme le contrôle DataList, le contrôle Repeater ne dispose pas hors de la fonctionnalité de zone d’insertion, de mise à jour ou de suppression. Bien que ces fonctionnalités peuvent être ajoutées, le contrôle DataList inclut propriétés et événements introuvable dans le répéteur qui simplifient l’ajout de ces fonctionnalités. Par conséquent, ce didacticiel et futurs qui examinent la modification et suppression concentrerons strictement sur le contrôle DataList.


## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>Étape 1 : Créer les Pages Web Didacticiels Édition et la suppression

Avant de commencer expliquant comment mettre à jour et supprimer des données à partir d’un contrôle DataList, permettent de s tout d’abord prendre un moment pour créer les pages ASP.NET dans notre projet de site Web que nous avons besoin pour ce didacticiel et celles plusieurs suivants. Commencez par ajouter un nouveau dossier nommé `EditDeleteDataList`. Ensuite, ajoutez les pages ASP.NET suivantes à ce dossier, en veillant à associer chaque page avec le `Site.master` page maître :

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Ajouter les Pages ASP.NET pour les didacticiels](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image1.png)

**Figure 1**: ajoutez les Pages ASP.NET pour les didacticiels


Comme dans les autres dossiers, `Default.aspx` dans le `EditDeleteDataList` dossier répertorie les didacticiels dans sa section. N’oubliez pas que le `SectionLevelTutorialListing.ascx` contrôle utilisateur fournit cette fonctionnalité. Par conséquent, ajoutez ce contrôle utilisateur à `Default.aspx` en le faisant glisser à partir de l’Explorateur de solutions sur la page s en mode Création.


[![Ajouter le contrôle utilisateur de SectionLevelTutorialListing.ascx à Default.aspx](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image2.png)

**Figure 2**: ajouter la `SectionLevelTutorialListing.ascx` contrôle utilisateur à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image4.png))


Enfin, ajoutez les pages en tant qu’entrées pour le `Web.sitemap` fichier. Plus précisément, ajoutez le balisage suivant après les rapports maître/détail avec les contrôles DataList et Repeater `<siteMapNode>`:


[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample1.xml)]

Après la mise à jour `Web.sitemap`, prenez un moment pour afficher le site Web de didacticiels via un navigateur. Le menu de gauche inclut maintenant des éléments pour le contrôle DataList de modification et suppression des didacticiels.


![Le plan de Site inclut maintenant des entrées pour le contrôle DataList de modification et suppression des didacticiels](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image5.png)

**Figure 3**: le plan de Site inclut maintenant des entrées pour le contrôle DataList de modification et suppression des didacticiels


## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>Étape 2 : Examen des Techniques pour la mise à jour et suppression de données

Modification et suppression de données avec le contrôle GridView sont si facile, car, dans les coulisses, les contrôles GridView et ObjectDataSource fonctionnent de concert. Comme indiqué dans le [examinant les événements associés insertion, mise à jour et suppression](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) (didacticiel), un clic sur un bouton de mise à jour de ligne s, le contrôle GridView attribue automatiquement ses champs qui a utilisé la liaison de données bidirectionnelle pour les `UpdateParameters` collection de son ObjectDataSource, puis appelle ce s ObjectDataSource `Update()` (méthode).

Malheureusement, le contrôle DataList ne fournit pas une de ces fonctionnalités intégrées. Il est de notre responsabilité pour vous assurer que les valeurs de l’utilisateur s sont affectées pour les paramètres de s ObjectDataSource et que son `Update()` méthode est appelée. Pour faciliter cette tâche, le contrôle DataList fournit les propriétés et les événements suivants :

- **Le [ `DataKeyField` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**  lors de la mise à jour ou la suppression, nous devons être en mesure d’identifier de manière unique chaque élément dans le contrôle DataList. Définissez cette propriété sur le champ de clé primaire des données affichées. Cela remplira le contrôle DataList s [ `DataKeys` collection](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx) avec la valeur `DataKeyField` valeur pour chaque élément DataList.
- **Le [ `EditCommand` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**  se déclenche quand un bouton, LinkButton ou ImageButton dont `CommandName` propriété est définie sur un clic sur Modifier.
- **Le [ `CancelCommand` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**  se déclenche quand un bouton, LinkButton ou ImageButton dont `CommandName` propriété est définie sur un clic sur Annuler.
- **Le [ `UpdateCommand` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**  se déclenche quand un bouton, LinkButton ou ImageButton dont `CommandName` propriété est définie sur utilisateur clique sur mise à jour.
- **Le [ `DeleteCommand` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**  se déclenche quand un bouton, LinkButton ou ImageButton dont `CommandName` propriété est définie sur un clic sur Supprimer.

À l’aide de ces propriétés et des événements, il existe quatre approches que nous pouvons utiliser pour mettre à jour et supprimer des données dans le contrôle DataList :

1. **À l’aide d’ASP.NET 1.x Techniques** DataList existait avant ASP.NET 2.0 et ObjectDataSources et a été en mesure de mettre à jour et supprimer des données entièrement par le biais de moyens de programmation. Cette technique fossés ObjectDataSource complètement et nécessite que nous lier les données pour le contrôle DataList directement à partir de la couche de logique métier, à la fois dans la récupération des données pour afficher et lorsque la mise à jour ou suppression d’un enregistrement.
2. **À l’aide d’un seul contrôle ObjectDataSource sur la Page de sélection, de mise à jour et de suppression** tandis que le contrôle DataList ne dispose pas de modification inhérente de GridView s et de suppression de capacités, s il y a aucune raison que nous pouvons t n’ajoutez-les dans nous-mêmes. Avec cette approche, nous utiliser un ObjectDataSource comme dans les exemples de GridView, mais devez créer un gestionnaire d’événements pour le contrôle DataList s `UpdateCommand` événement où nous définissons les paramètres de s ObjectDataSource et appellent son `Update()` (méthode).
3. **À l’aide d’un contrôle ObjectDataSource pour la sélection, mais mise à jour et suppression directement par rapport à la couche BLL** lorsque vous utilisez l’option 2, nous devons écrire un peu de code dans le `UpdateCommand` événement, affectez les valeurs de paramètre et ainsi de suite. Au lieu de cela, nous pouvons conserver à l’aide d’ObjectDataSource pour la sélection, mais effectuer les appels de la mise à jour et suppression directement par rapport à la couche BLL (par exemple, avec option 1). À mon avis, la mise à jour des données en créant une interface directement avec la couche BLL entraîne un code plus lisible que l’affectation de l’ObjectDataSource s `UpdateParameters` et en appelant son `Update()` (méthode).
4. **À l’aide d’un moyen déclaratif via plusieurs ObjectDataSources** précédentes trois approches tous les nécessitent un peu de code. Si vous d plutôt continuer à utiliser en tant que la syntaxe déclarative autant que possible, la dernière possibilité consiste à inclure plusieurs ObjectDataSources sur la page. Première ObjectDataSource récupère les données à partir de la couche BLL et le lie à la DataList. Pour la mise à jour, un autre ObjectDataSource est ajouté, mais directement dans le contrôle DataList s `EditItemTemplate`. Pour inclure la prise en charge de suppression, encore une autre ObjectDataSource devaient être apportés dans la `ItemTemplate`. Avec cette approche, ces incorporé utilisation ObjectDataSource `ControlParameters` pour lier de façon déclarative les paramètres de s ObjectDataSource aux contrôles d’entrée utilisateur (au lieu d’avoir à les spécifier par programme dans le contrôle DataList s `UpdateCommand` Gestionnaire d’événements). Cette approche nécessite encore un peu de code, nous devons appeler les opérations de mappage ObjectDataSource embedded `Update()` ou `Delete()` commande mais nécessite beaucoup moins d’avec les autres trois approches. L’inconvénient est que les ObjectDataSources plusieurs encombrent la page, se départir à partir de la lisibilité globale.

Si forcé à toujours utiliser une de ces approches, d choisir option 1, car elle fournit la plus grande souplesse et étant donné que le contrôle DataList a été conçu pour prendre en charge ce modèle. Tandis que le contrôle DataList a été étendu pour fonctionner avec les contrôles de source de données ASP.NET 2.0, il n’a pas de tous les points d’extensibilité ou des fonctionnalités des données ASP.NET 2.0 officielles des contrôles Web (le GridView, DetailsView et FormView). Options de 2 à 4 ne sont pas sans mérite, cependant.

Cela future modification et suppression des didacticiels utilisera un ObjectDataSource pour récupérer les données pour afficher et de diriger les appels à la couche BLL à mettre à jour et supprimer des données (option 3).

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>Étape 3 : Ajout du contrôle DataList et la configuration de son ObjectDataSource

Dans ce didacticiel, nous allons créer un contrôle DataList qui affiche des informations de produit et, pour chaque produit, fournit à l’utilisateur la possibilité de modifier le nom et le prix et pour supprimer le produit complètement. En particulier, nous récupérera les enregistrements à afficher à l’aide d’ObjectDataSource, mais effectuer la mise à jour et supprimer des actions en créant une interface directement avec la couche BLL. Avant de nous soucier de mise en œuvre les fonctionnalités de modification et de suppression pour le contrôle DataList, laisser s tout d’abord la page pour afficher les produits dans une interface en lecture seule. Dans la mesure où ve examiné ces étapes dans les didacticiels précédents, je vais poursuivre les rapidement.

Commencez par ouvrir le `Basics.aspx` page dans le `EditDeleteDataList` dossier et, à partir de la vue conception, ajoutez un contrôle DataList à la page. Ensuite, à partir de la balise active de s DataList, créez un nouveau ObjectDataSource. Étant donné que nous travaillons en collaboration avec les données de produit, configurez-le pour utiliser le `ProductsBLL` classe. Pour récupérer *tous les* produits, choisissez le `GetProducts()` méthode dans l’onglet sélection.


[![Configurer pour utiliser la classe ProductsBLL ObjectDataSource](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image6.png)

**Figure 4**: configurer ObjectDataSource à utiliser le `ProductsBLL` classe ([cliquez pour afficher l’image en taille réelle](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image8.png))


[![Retourner les informations de produit à l’aide de la méthode GetProducts()](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image9.png)

**Figure 5**: retourner les informations de produit en utilisant le `GetProducts()` (méthode) ([cliquez pour afficher l’image en taille réelle](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image11.png))


Le contrôle DataList, tels que le contrôle GridView, n’est pas conçu pour l’insertion de nouvelles données ; Par conséquent, sélectionnez (aucun) option dans la liste déroulante dans l’onglet Insertion. Choisissez également (aucune) pour les onglets de la mise à jour et suppression depuis les mises à jour et suppressions se fera par programmation via la couche BLL.


[![Vérifiez que les listes déroulantes dans le s ObjectDataSource insertion, mise à jour et supprimer des onglets sont définis à (None)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image12.png)

**Figure 6**: Vérifiez que la liste déroulante répertorie ObjectDataSource s insertion, mise à jour, et supprimer des onglets sont définis à (None) ([cliquez pour afficher l’image en taille réelle](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image14.png))


Après avoir configuré l’ObjectDataSource, cliquez sur Terminer, retourner au concepteur. Comme nous ve vu dans ces exemples, lorsque vous terminez la configuration ObjectDataSource, Visual Studio automatiquement crée un `ItemTemplate` pour l’objet DropDownList, affichage de chacun des champs de données. Remplacez ceci `ItemTemplate` avec celui qui affiche uniquement le nom de produit s et le prix. En outre, définissez le `RepeatColumns` propriété à 2.

> [!NOTE]
> Comme indiqué dans le *vue d’ensemble de l’insertion, mise à jour et suppression des données* (didacticiel), lors de la modification des données à l’aide de l’ObjectDataSource notre architecture requiert que nous supprimons le `OldValuesParameterFormatString` propriété à partir de l’ObjectDataSource s balisage déclaratif (ou le réinitialiser à sa valeur par défaut, `{0}`). Dans ce didacticiel, toutefois, nous utilisons l’ObjectDataSource uniquement pour récupérer des données. Par conséquent, nous n’avez pas besoin de modifier les opérations de mappage ObjectDataSource `OldValuesParameterFormatString` valeur de propriété (bien qu’il ne fait pas de mal pour faire).


Après avoir remplacé la valeur par défaut DataList `ItemTemplate` avec une, le balisage déclaratif sur votre page doit ressembler à ce qui suit :


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample2.aspx)]

Prenez un moment pour consulter notre progression via un navigateur. Comme le montre la Figure 7, le contrôle DataList affiche le produit nom et le prix unitaire pour chaque produit dans deux colonnes.


[![Les noms de produits et les prix sont affichés dans un contrôle DataList de deux colonnes](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image15.png)

**Figure 7**: les noms de produits et les prix sont affichés dans un contrôle DataList de deux colonnes ([cliquez pour afficher l’image en taille réelle](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image17.png))


> [!NOTE]
> Le contrôle DataList a un nombre de propriétés qui sont requis pour le processus de mise à jour et suppression, et ces valeurs sont stockées dans l’état d’affichage. Par conséquent, lors de la création d’un contrôle DataList qui prend en charge la modification ou la suppression des données, il est essentiel que l’état d’affichage DataList s soit activé.  
>   
>  Le lecteur astucieux suggérait que nous avons pu désactiver l’état d’affichage lors de la création modifiable des contrôles GridView, DetailsViews et FormViews. Il s’agit, car les contrôles Web ASP.NET 2.0 peuvent inclure *état du contrôle*, qui est état rendu persistant entre les postbacks comme état d’affichage, mais essentielles présumé.


Désactiver l’affichage de l’état dans le contrôle GridView simplement omet les informations d’état simple, mais conserve l’état du contrôle (qui inclut l’état nécessaire pour la modification et suppression). Le contrôle DataList, ayant été créé dans le délai d’exécution ASP.NET 1.x, n’utilise pas l’état du contrôle et doit donc état d’affichage est activé. Consultez [vs d’état du contrôle. État d’affichage](https://msdn.microsoft.com/library/1whwt1k7.aspx) pour plus d’informations sur l’objectif de l’état du contrôle et la façon dont il diffère de l’état d’affichage.

## <a name="step-4-adding-an-editing-user-interface"></a>Étape 4 : Ajout d’une Interface utilisateur de modification

Le contrôle GridView se compose d’une collection de champs (BoundFields CheckBoxFields, TemplateFields et ainsi de suite). Ces champs peuvent ajuster leur balisage en fonction de leur mode de rendu. Par exemple, en mode lecture seule, un BoundField affiche sa valeur de champ de données sous forme de texte ; en mode édition, il restitue une zone de texte de Web contrôle dont `Text` propriété est affectée à la valeur de champ de données.

Le contrôle DataList, restitue quant à eux, ses éléments à l’aide de modèles. Éléments en lecture seule sont rendus à l’aide de la `ItemTemplate` tandis que les éléments en mode d’édition sont rendus la `EditItemTemplate`. À ce stade, notre DataList a uniquement un `ItemTemplate`. Pour prendre en charge la fonctionnalité d’édition au niveau élément nous devons ajouter un `EditItemTemplate` qui contient le balisage à afficher pour l’élément modifiable. Pour ce didacticiel, nous allons utiliser des contrôles Web de la zone de texte pour l’édition du produit s nom et le prix unitaire.

Le `EditItemTemplate` peuvent être créés soit de manière déclarative ou par le biais du concepteur (en sélectionnant l’option Modifier les modèles à partir de la balise active DataList s). Pour utiliser l’option Modifier les modèles, cliquez d’abord sur le lien Modifier les modèles dans la balise active, puis le `EditItemTemplate` élément dans la liste déroulante.


[![Opter pour travailler avec DataList s EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image18.png)

**Figure 8**: opter pour travailler avec le contrôle DataList s `EditItemTemplate` ([cliquez pour afficher l’image en taille réelle](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image20.png))


Ensuite, tapez dans nom du produit : et prix : puis faites glisser deux contrôles TextBox à partir de la boîte à outils dans le `EditItemTemplate` interface sur le concepteur. Définir les zones de texte `ID` propriétés à `ProductName` et `UnitPrice`.


[![Ajouter une zone de texte pour le nom de produit s et le prix](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image21.png)

**Figure 9**: ajouter une zone de texte pour le nom de produit et un prix ([cliquez pour afficher l’image en taille réelle](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image23.png))


Nous avons besoin lier les valeurs de champ de données produit correspondant à la `Text` propriétés des deux zones de texte. Dans les balises actives de zones de texte, cliquez sur le lien Modifier les DataBindings, puis associez le champ de données appropriées avec la `Text` propriété, comme illustré dans la Figure 10.

> [!NOTE]
> Lors de la liaison la `UnitPrice` champ de données au prix de zone de texte s `Text` champ, vous pouvez mettez-la en forme comme une valeur monétaire (`{0:C}`), un nombre Général (`{0:N}`), ou laissez-le sans mise en forme.


![Lier le ProductName et les champs de données de prix unitaire pour les propriétés de texte des zones de texte](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image24.png)

**Figure 10**: lier le `ProductName` et `UnitPrice` des champs de données pour le `Text` propriétés des zones de texte


Notez en quoi la boîte de dialogue Modifier les DataBindings dans la Figure 10 *pas* incluent la case à cocher de liaison de données bidirectionnelle est présent lors de la modification d’un TemplateField dans le contrôle GridView ou DetailsView, ou un modèle dans le contrôle FormView. La fonctionnalité de liaison de données bidirectionnelle autorisés la valeur entrée dans le contrôle Web d’entrée soit attribué automatiquement au s ObjectDataSource correspondant `InsertParameters` ou `UpdateParameters` pendant l’insertion ou la mise à jour des données. Le contrôle DataList ne prend pas en charge la liaison de données bidirectionnelle nous verrons plus tard dans ce didacticiel, une fois l’utilisateur effectue sa change et est prêt à mettre à jour les données, nous devons accéder par programmation à ces zones de texte `Text` propriétés et leurs valeurs à passer le appropriée `UpdateProduct` méthode dans la `ProductsBLL` classe.

Enfin, nous devons ajouter la mise à jour et annuler des boutons à la `EditItemTemplate`. Comme nous l’avons vu dans la [maître/détail à l’aide d’une liste à puces des enregistrements maîtres avec une DataList des détails](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) (didacticiel), quand un bouton, LinkButton ou ImageButton dont `CommandName` propriété a la valeur est activé à partir de dans un répéteur ou d’un contrôle DataList, le S Repeater ou DataList `ItemCommand` événement est déclenché. Pour le contrôle DataList, si le `CommandName` propriété est définie sur une certaine valeur, un événement supplémentaire peut être levée ainsi. Spéciale `CommandName` valeurs de propriétés incluent, entre autres :

- Annuler déclenche le `CancelCommand` événement
- Modifier déclenche le `EditCommand` événement
- Mettre à jour déclenche le `UpdateCommand` événement

N’oubliez pas que ces événements sont déclenchés *outre* le `ItemCommand` événement.

Ajouter à la `EditItemTemplate` deux contrôles bouton, un dont `CommandName` est défini sur la mise à jour et les autres s défini sur Annuler. Après avoir ajouté ces deux contrôles bouton Web le concepteur doit ressembler à ce qui suit :


[![Ajouter la mise à jour et annuler des boutons à EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image25.png)

**Figure 11**: ajouter la mise à jour et les boutons Annuler le `EditItemTemplate` ([cliquez pour afficher l’image en taille réelle](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image27.png))


Avec le `EditItemTemplate` complète votre balisage déclaratif DataList s doit ressembler à ce qui suit :


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>Étape 5 : Ajout de l’infrastructure nécessaire pour entrer en Mode Édition

À ce stade notre DataList a une interface de modification définie par le biais de son `EditItemTemplate`; Toutefois, cet emplacement s actuellement aucun moyen pour un utilisateur accédant à notre page pour indiquer qu’il souhaite modifier une information de produit s. Nous devons ajouter un bouton Modifier pour chaque produit que, lorsque vous cliquez dessus, affiche ce DataList élément en mode édition. Commencez par ajouter un bouton Modifier pour le `ItemTemplate`, soit par le biais du concepteur ou de façon déclarative. Veillez à définir le bouton Modifier s `CommandName` propriété à modifier.

Une fois que vous avez ajouté ce bouton Modifier, prenez un moment pour afficher la page via un navigateur. Ainsi, chaque liste de produits doit inclure un bouton Modifier.


[![Ajouter la mise à jour et annuler des boutons à EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image28.png)

**Figure 12**: ajouter la mise à jour et les boutons Annuler le `EditItemTemplate` ([cliquez pour afficher l’image en taille réelle](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image30.png))


En cliquant sur le bouton provoque une publication (postback), contrairement à *pas* mettre le produit listing en mode édition. Pour rendre le produit modifiables, nous devons :

1. Définir le contrôle DataList s [ `EditItemIndex` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) à l’index de la `DataListItem` dont modifier simplement bouton.
2. Relier les données pour le contrôle DataList. Lorsque le contrôle DataList est restitué, la `DataListItem` dont `ItemIndex` correspond à la DataList s `EditItemIndex` s’affiche à l’aide de son `EditItemTemplate`.

Depuis le contrôle DataList s `EditCommand` événement est déclenché lorsque l’utilisateur clique sur le bouton Modifier, créer un `EditCommand` Gestionnaire d’événements par le code suivant :


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample4.vb)]

Le `EditCommand` est transmis au gestionnaire d’événements dans un objet de type `DataListCommandEventArgs` en tant que son deuxième paramètre d’entrée, qui inclut une référence à la `DataListItem` dont bouton Modifier (`e.Item`). Le Gestionnaire d’événements définit tout d’abord le contrôle DataList s `EditItemIndex` à la `ItemIndex` de la liste modifiable `DataListItem` , puis relie les données pour le contrôle DataList en appelant le contrôle DataList s `DataBind()` (méthode).

Après avoir ajouté ce gestionnaire d’événements, visitez la page dans un navigateur. En cliquant sur le bouton Modifier maintenant rend l’utilisateur a cliqué dessus produit modifiable (voir Figure 13).


[![En cliquant sur le fait de bouton Modifier le produit modifiable](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image31.png)

**Figure 13**: en cliquant sur le bouton Modifier permet du modifier de produit ([cliquez pour afficher l’image en taille réelle](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image33.png))


## <a name="step-6-saving-the-user-s-changes"></a>Étape 6 : L’enregistrement des modifications s utilisateur

En cliquant sur le produit modifié s mise à jour ou les boutons Annuler n’a aucun effet à ce stade ; Pour ajouter cette fonctionnalité, nous devons créer des gestionnaires d’événements pour le contrôle DataList s `UpdateCommand` et `CancelCommand` événements. Commencez par créer le `CancelCommand` Gestionnaire d’événements, qui doit être exécutée lorsque le bouton Cancel produit modifié s et il chargé de retourner le contrôle DataList à son état avant modification.

Pour que le contrôle DataList restituer tous ses éléments dans le mode lecture seule, nous devons :

1. Définir le contrôle DataList s [ `EditItemIndex` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) à l’index d’un inexistante `DataListItem` index. `-1` est un choix sûr, car le `DataListItem` index commencent à `0`.
2. Relier les données pour le contrôle DataList. Depuis non `DataListItem` `ItemIndex` es correspondent à la DataList s `EditItemIndex`, le contrôle DataList entière sera rendu dans un mode en lecture seule.

Ces étapes peuvent être effectuées avec le code de gestionnaire d’événements suivantes :


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample5.vb)]

Ainsi, en cliquant sur le retourne de bouton Annuler le contrôle DataList à son état préalable édition.

Est le dernier gestionnaire d’événements nous devons effectuer la `UpdateCommand` Gestionnaire d’événements. Ce gestionnaire d’événements doit :

1. Accès par programme le nom de produit entré par l’utilisateur et prix, ainsi que le produit modifié s `ProductID`.
2. Lancer le processus de mise à jour en appelant approprié `UpdateProduct` surcharge dans le `ProductsBLL` classe.
3. Définir le contrôle DataList s [ `EditItemIndex` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) à l’index d’un inexistante `DataListItem` index. `-1` est un choix sûr, car le `DataListItem` index commencent à `0`.
4. Relier les données pour le contrôle DataList. Depuis non `DataListItem` `ItemIndex` es correspondent à la DataList s `EditItemIndex`, le contrôle DataList entière sera rendu dans un mode en lecture seule.

Les étapes 1 et 2 sont responsables de l’enregistrement des modifications de s ; de l’utilisateur les étapes 3 et 4 retournent le contrôle DataList à son état préalable modification une fois que les modifications ont été enregistrées et sont identiques aux étapes effectuées dans le `CancelCommand` Gestionnaire d’événements.

Pour obtenir le nom du produit mis à jour et le prix, nous devons utiliser le `FindControl` méthode par programmation la référence Web de la zone de texte des contrôles dans le `EditItemTemplate`. Nous devons également obtenir ce produit modifié s `ProductID` valeur. Lorsque nous avons initialement lié ObjectDataSource pour le contrôle DataList, Visual Studio attribué le contrôle DataList s `DataKeyField` propriété la valeur de clé primaire à partir de la source de données (`ProductID`). Cette valeur peut ensuite être récupérée à partir du contrôle DataList s `DataKeys` collection. Prenez un moment pour vous assurer que le `DataKeyField` propriété est en effet la valeur `ProductID`.

Le code suivant implémente les quatre étapes :


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample6.vb)]

Le Gestionnaire d’événements démarre en lisant le produit modifié s `ProductID` à partir de la `DataKeys` collection. Ensuite, les deux zones de texte dans le `EditItemTemplate` sont référencés et leurs `Text` propriétés stockées dans des variables locales, `productNameValue` et `unitPriceValue`. Nous utilisons le `Decimal.Parse()` méthode pour lire la valeur de la `UnitPrice` zone de texte afin qu’en cas de la valeur entrée a un symbole de devise, il peut toujours être correctement converti en un `Decimal` valeur.

> [!NOTE]
> Les valeurs à partir de la `ProductName` et `UnitPrice` zones de texte sont uniquement affectées aux variables productNameValue et unitPriceValue si les propriétés de texte des zones de texte ont une valeur spécifiée. Sinon, une valeur de `Nothing` est utilisé pour les variables, ce qui a pour effet de mettre à jour les données avec une base de données `NULL` valeur. Autrement dit, notre code traite convertit des chaînes à la base de données vides `NULL` valeurs, qui est le comportement par défaut de l’interface de modification dans les contrôles GridView, DetailsView et FormView.


Après avoir lu les valeurs, le `ProductsBLL` classe s `UpdateProduct` est appelée, en passant le nom de produit s, prix, et `ProductID`. Le Gestionnaire d’événements se termine en renvoyant le contrôle DataList à son état préalable édition à l’aide de la même logique exactement comme dans le `CancelCommand` Gestionnaire d’événements.

Avec le `EditCommand`, `CancelCommand`, et `UpdateCommand` terminer des gestionnaires d’événements, un visiteur peut modifier le nom et le prix d’un produit. Du 14 au 16 chiffres montrant ce flux de travail en action.


[![Lorsque la première visite de la Page, tous les produits sont en Mode lecture seule](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image34.png)

**Figure 14**: lors de la première visite la Page, tous les produits sont en Mode lecture seule ([cliquez pour afficher l’image en taille réelle](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image36.png))


[![Pour mettre à jour un s nom ou le prix du produit, cliquez sur le bouton Modifier](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image37.png)

**Figure 15**: pour mettre à jour d’un nom de produit ou le prix, cliquez sur le bouton Modifier ([cliquez pour afficher l’image en taille réelle](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image39.png))


[![Après avoir modifié la valeur, cliquez sur la mise à jour pour retourner au Mode en lecture seule](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image40.png)

**Figure 16**: après la modification de la valeur, cliquez sur la mise à jour pour retourner au Mode en lecture seule ([cliquez pour afficher l’image en taille réelle](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image42.png))


## <a name="step-7-adding-delete-capabilities"></a>Étape 7 : Ajout de fonctionnalités de suppression

Les étapes pour ajouter des fonctionnalités de suppression à un contrôle DataList sont similaires à celles de l’ajout de fonctionnalités d’édition. En bref, nous devons ajouter un bouton Supprimer pour le `ItemTemplate` qui, lorsque vous cliquez sur :

1. Lit dans le produit correspondant s `ProductID` via la `DataKeys` collection.
2. Exécute la suppression en appelant le `ProductsBLL` classe s `DeleteProduct` (méthode).
3. Relie les données du contrôle DataList.

S permettent de commencer par ajouter un bouton Supprimer pour le `ItemTemplate`.

Lorsque vous cliquez sur, un bouton dont `CommandName` est mise à jour, modifier ou Annuler déclenche le contrôle DataList s `ItemCommand` événement ainsi que d’un événement supplémentaire (par exemple, lors de l’utilisation de modifier le `EditCommand` événement est également déclenché). De même, n’importe quel bouton, un LinkButton ou ImageButton dans le contrôle DataList dont `CommandName` propriété est définie sur les causes de supprimer le `DeleteCommand` l’événement (avec `ItemCommand`).

Ajouter un bouton Supprimer en regard du bouton Modifier dans le `ItemTemplate`, ce qui affecte ses `CommandName` propriété à supprimer. Après avoir ajouté ce contrôle bouton votre DataList s `ItemTemplate` syntaxe déclarative doit ressembler à :


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample7.aspx)]

Ensuite, créez un gestionnaire d’événements pour le contrôle DataList s `DeleteCommand` événement, en utilisant le code suivant :


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample8.vb)]

En cliquant sur le bouton Supprimer entraîne une publication et déclenche le contrôle DataList s `DeleteCommand` événement. Dans le Gestionnaire d’événements, l’utilisateur a cliqué dessu produit s `ProductID` valeur est accessible à partir du `DataKeys` collection. Ensuite, le produit est supprimé en appelant le `ProductsBLL` classe s `DeleteProduct` (méthode).

Après la suppression du produit, il s important que nous lier de nouveau les données pour le contrôle DataList (`DataList1.DataBind()`), sinon le contrôle DataList continue d’afficher le produit qui a été simplement supprimé.

## <a name="summary"></a>Récapitulatif

Bien que le contrôle DataList ne possède pas le point et cliquez sur la modification et suppression de prise en charge apprécié par le contrôle GridView, avec un court peu de code il peut être améliorée pour inclure ces fonctionnalités. Dans ce didacticiel, nous avons vu comment créer une liste de deux colonnes de produits qui peut être supprimée et dont le nom et prix peuvent être modifiés. Ajout de modification et suppression de prise en charge est une question de, notamment les contrôles Web appropriés dans le `ItemTemplate` et `EditItemTemplate`, créer les gestionnaires d’événements correspondants, lire les valeurs de clé primaires et entré par l’utilisateur et interagissant avec l’entreprise Couche de logique.

Alors que nous avons ajouté les modifications de base et la suppression de fonctionnalités pour le contrôle DataList, il ne dispose pas des fonctionnalités plus avancées. Par exemple, il n’existe aucune validation de champ d’entrée - si un utilisateur entre un prix de trop coûteux, une exception sera levée par `Decimal.Parse` lorsque vous tentez de convertir trop coûteuses dans un `Decimal`. De même, s’il existe un problème de mise à jour les données à la logique métier ou des couches d’accès aux données à l’utilisateur s’affiche avec l’écran d’erreur standard. Sans une forme quelconque de confirmation sur le bouton Supprimer, la suppression accidentelle d’un produit est trop probable.

Dans les futures rencontrer des didacticiels, nous verrons comment améliorer l’édition utilisateur.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Zack Jones, Ken Pespisa et Randy Schmidt. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](customizing-the-datalist-s-editing-interface-cs.md)
> [Suivant](performing-batch-updates-vb.md)
