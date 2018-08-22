---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
title: Une vue d’ensemble de l’insertion, la mise à jour et suppression de données (c#) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons voir comment mapper un ObjectDataSource Insert(), Update(), et des classes de méthodes Delete() aux méthodes de la couche BLL, ainsi que la configuration...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: b651dc58-93c7-4f83-a74e-3b99f6d60848
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 82f1127b01c211a2af91623d4df7ca10dcad6d8a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835855"
---
<a name="an-overview-of-inserting-updating-and-deleting-data-c"></a>Une vue d’ensemble de l’insertion, la mise à jour et suppression de données (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_CS.exe) ou [télécharger le PDF](an-overview-of-inserting-updating-and-deleting-data-cs/_static/datatutorial16cs1.pdf)

> Dans ce didacticiel, nous verrons comment mapper un ObjectDataSource Insert(), Update(), et des classes de méthodes Delete() aux méthodes de la couche BLL, ainsi que la façon de configurer les contrôles GridView, DetailsView et FormView afin de fournir des fonctionnalités de modification de données.


## <a name="introduction"></a>Introduction

Sur les dernières plusieurs didacticiels, nous avons examiné comment afficher des données dans une page ASP.NET avec les contrôles GridView, DetailsView et FormView. Ces contrôles fonctionnent avec les données fournies pour les. En général, ces contrôles accéder aux données via l’utilisation d’un contrôle de source de données, tels que ObjectDataSource. Nous avons vu comment ObjectDataSource agit comme un proxy entre la page ASP.NET et les données sous-jacentes. Quand un GridView doit afficher des données, il appelle son ObjectDataSource `Select()` (méthode), qui à son tour appelle une méthode à partir de notre couche BLL (Business Logic), qui appelle une méthode dans appropriée l’accès de la couche données (DAL) TableAdapter, qui à son tour envoie un `SELECT` requête à la base de données Northwind.

N’oubliez pas que, lorsque nous avons créé les TableAdapters dans la couche DAL dans [notre premier didacticiel](../introduction/creating-a-data-access-layer-cs.md), Visual Studio ajouté automatiquement les méthodes d’insertion, la mise à jour, et suppression de données sous-jacent de base de données de la table. En outre, dans [création d’une couche de logique métier](../introduction/creating-a-business-logic-layer-cs.md) nous avons conçu des méthodes dans la couche BLL ayant appelé dans ces méthodes de modification de la couche DAL de données.

Outre ses `Select()` méthode, ObjectDataSource a `Insert()`, `Update()`, et `Delete()` méthodes. Comme le `Select()` (méthode), ces trois méthodes peuvent être mappées aux méthodes dans un objet sous-jacent. Quand est configuré pour insérer, mettre à jour ou supprimer des données, les contrôles GridView, DetailsView et FormView fournissent une interface utilisateur de modification des données sous-jacentes. Cette interface utilisateur appelle le `Insert()`, `Update()`, et `Delete()` méthodes de l’ObjectDataSource, puis appellent l’objet sous-jacent associé à des méthodes (voir Figure 1).


[![L’ObjectDataSource Insert(), Update() et les méthodes Delete() servent d’un Proxy dans la couche BLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image1.png)

**Figure 1**: The ObjectDataSource `Insert()`, `Update()`, et `Delete()` méthodes servir en tant que Proxy dans la couche BLL ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image3.png))


Dans ce didacticiel, nous allons voir comment mapper l’ObjectDataSource `Insert()`, `Update()`, et `Delete()` méthodes aux méthodes des classes dans la couche BLL, ainsi que comment configurer les contrôles GridView, DetailsView et FormView afin de fournir la modification des données fonctionnalités.

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>Étape 1 : Création de l’insertion, mise à jour et supprimer des Pages Web didacticiels

Avant de commencer expliquant comment insérer, mettre à jour et supprimer des données, prenons tout d’abord un moment pour créer les pages ASP.NET dans notre projet de site Web que nous avons besoin pour ce didacticiel et celles plusieurs suivants. Commencez par ajouter un nouveau dossier nommé `EditInsertDelete`. Ensuite, ajoutez les pages ASP.NET suivantes à ce dossier, en veillant à associer chaque page avec le `Site.master` page maître :

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Ajouter les Pages ASP.NET pour les didacticiels liés à la Modification de données](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image4.png)

**Figure 2**: ajoutez les Pages ASP.NET pour les didacticiels liés à la Modification de données


Comme dans les autres dossiers, `Default.aspx` dans le `EditInsertDelete` dossier répertorie les didacticiels dans sa section. N’oubliez pas que le `SectionLevelTutorialListing.ascx` contrôle utilisateur fournit cette fonctionnalité. Par conséquent, ajoutez ce contrôle utilisateur à `Default.aspx` en le faisant glisser à partir de l’Explorateur de solutions sur le mode de création de la page.


[![Ajouter le contrôle utilisateur de SectionLevelTutorialListing.ascx à Default.aspx](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image5.png)

**Figure 3**: ajouter la `SectionLevelTutorialListing.ascx` contrôle utilisateur à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image7.png))


Enfin, ajoutez les pages en tant qu’entrées pour le `Web.sitemap` fichier. Plus précisément, ajoutez le balisage suivant après la mise en forme personnalisée `<siteMapNode>`:


[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample1.xml)]

Après la mise à jour `Web.sitemap`, prenez un moment pour afficher le site Web de didacticiels via un navigateur. Le menu de gauche inclut désormais les éléments de la modification, insertion et suppression des didacticiels.


![Le plan de Site inclut maintenant des entrées pour la modification, insertion et suppression des didacticiels](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image8.png)

**Figure 4**: le plan de Site inclut maintenant des entrées pour la modification, insertion et suppression des didacticiels


## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>Étape 2 : Ajout et configuration du contrôle ObjectDataSource

Depuis le GridView, DetailsView et FormView chacun diffèrent par leurs fonctionnalités de modification de données et la disposition, nous allons examiner chacun d’eux individuellement. Au lieu que chaque contrôle à l’aide de son propre ObjectDataSource, toutefois, nous allons simplement créer une ObjectDataSource unique tous les exemples de contrôle trois peuvent partager.

Ouvrez le `Basics.aspx` page, faites glisser un ObjectDataSource à partir de la boîte à outils vers le concepteur et cliquez sur le lien configurer la Source de données à partir de sa balise active. Dans la mesure où le `ProductsBLL` est la seule classe de couche de logique métier qui fournit la modification, d’insertion et de suppression des méthodes, configurent ObjectDataSource pour utiliser cette classe.


[![Configurer pour utiliser la classe ProductsBLL ObjectDataSource](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image9.png)

**Figure 5**: configurer ObjectDataSource à utiliser le `ProductsBLL` classe ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image11.png))


Dans l’écran suivant, nous pouvons spécifier quelles méthodes de la `ProductsBLL` classe sont mappés à l’ObjectDataSource `Select()`, `Insert()`, `Update()`, et `Delete()` en sélectionnant l’onglet approprié, la méthode dans la liste déroulante. Figure 6, qui doit paraître familière à ce stade, mappe l’ObjectDataSource `Select()` méthode à la `ProductsBLL` la classe `GetProducts()` (méthode). Le `Insert()`, `Update()`, et `Delete()` méthodes peuvent être configurées en sélectionnant l’onglet approprié dans la liste située dans la partie supérieure.


[![Avez ObjectDataSource retourner tous les produits](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image12.png)

**Figure 6**: ont l’ObjectDataSource retourner tous les produits ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image14.png))


Onglets de figures 7, 8 et 9 montrent l’ObjectDataSource UPDATE, INSERT et DELETE. Configurer ces onglets afin que le `Insert()`, `Update()`, et `Delete()` méthodes appellent le `ProductsBLL` la classe `UpdateProduct`, `AddProduct`, et `DeleteProduct` méthodes, respectivement.


[![Mapper Update() méthode l’ObjectDataSource à méthode UpdateProduct de la classe ProductBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image15.png)

**Figure 7**: mapper l’ObjectDataSource `Update()` méthode à la `ProductBLL` la classe `UpdateProduct` (méthode) ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image17.png))


[![Mapper Insert() méthode l’ObjectDataSource à méthode de AddProduct de la classe ProductBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image18.png)

**Figure 8**: mapper l’ObjectDataSource `Insert()` méthode à la `ProductBLL` ajouter de la classe `Product` (méthode) ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image20.png))


[![Mapper Delete() méthode l’ObjectDataSource à DeleteProduct (méthode de la classe ProductBLL)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image21.png)

**Figure 9**: mapper l’ObjectDataSource `Delete()` méthode à la `ProductBLL` la classe `DeleteProduct` (méthode) ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image23.png))


Vous avez peut-être remarqué que les listes déroulantes dans les onglets UPDATE, INSERT et DELETE avaient déjà ces méthodes sélectionnées. C’est grâce à notre utilisation de la `DataObjectMethodAttribute` qui décore les méthodes de `ProducstBLL`. Par exemple, la méthode DeleteProduct a la signature suivante :


[!code-csharp[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample2.cs)]

Le `DataObjectMethodAttribute` attribut indique l’objectif de chaque méthode qu’il s’agisse de sélection, insertion, mise à jour, ou la suppression et si elle est la valeur par défaut. Si vous omettez ces attributs lors de la création de vos classes de la couche BLL, vous aurez besoin afin de sélectionner manuellement les méthodes à partir de la mise à jour, insérer et supprimer des onglets.

Après avoir vérifié que le texte approprié `ProductsBLL` méthodes sont mappés à l’ObjectDataSource `Insert()`, `Update()`, et `Delete()` méthodes, cliquez sur Terminer pour terminer l’Assistant.

## <a name="examining-the-objectdatasources-markup"></a>Examiner le balisage de l’ObjectDataSource

Après avoir configuré l’ObjectDataSource via son Assistant, passez à la vue de Source à examiner le balisage déclaratif généré. Le `<asp:ObjectDataSource>` balise spécifie l’objet sous-jacent et les méthodes à appeler. En outre, il existe `DeleteParameters`, `UpdateParameters`, et `InsertParameters` qui correspondent aux paramètres d’entrée pour le `ProductsBLL` la classe `AddProduct`, `UpdateProduct`, et `DeleteProduct` méthodes :


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample3.aspx)]

ObjectDataSource inclut un paramètre pour chacun des paramètres d’entrée pour ses méthodes associées, comme une liste de `SelectParameter` s est présent quand ObjectDataSource est configuré pour appeler une méthode de sélection qui attend un paramètre d’entrée (par exemple `GetProductsByCategoryID(categoryID)`). Nous verrons bientôt, les valeurs pour ces `DeleteParameters`, `UpdateParameters`, et `InsertParameters` sont définies automatiquement par le GridView, DetailsView et FormView avant d’appeler l’ObjectDataSource `Insert()`, `Update()`, ou `Delete()` méthode. Ces valeurs peuvent également être définies par programme en fonction des besoins, comme nous le verrons dans un futur didacticiel.

Un effet secondaire de l’utilisation de l’Assistant pour configurer à ObjectDataSource est que Visual Studio définit le [propriété OldValuesParameterFormatString](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx) à `original_{0}`. Cette valeur de propriété est utilisée pour inclure les valeurs d’origine des données en cours de modification et est utile dans deux scénarios :

- Si, lorsque vous modifiez un enregistrement, les utilisateurs sont en mesure de modifier la valeur de clé primaire. Dans ce cas, la nouvelle valeur de clé primaire et la valeur de clé primaire d’origine doivent être fournis afin que l’enregistrement avec la valeur de clé primaire d’origine puisse être trouvée et dispose de sa valeur mis à jour en conséquence.
- Lorsque vous utilisez l’accès concurrentiel optimiste. Optimiste d’accès concurrentiel est une technique pour vous assurer que deux utilisateurs simultanés ne pas écraser les modifications par un autre et la rubrique pour un futur didacticiel.

Le `OldValuesParameterFormatString` propriété indique le nom des paramètres d’entrée dans la mise à jour de l’objet sous-jacent et les méthodes de suppression pour les valeurs d’origine. Nous allons aborder cette propriété et son usage en détail lorsque nous explorons l’accès concurrentiel optimiste. J’ai copiez-la à présent, toutefois, étant donné que les méthodes de notre BLL n’attendent pas les valeurs d’origine et par conséquent, il est important que nous supprimons cette propriété. En laissant le `OldValuesParameterFormatString` propriété une valeur autre que la valeur par défaut (`{0}`) provoque une erreur lorsqu’un contrôle Web de données tente d’appeler l’ObjectDataSource `Update()` ou `Delete()` méthodes car ObjectDataSource Essayez de passer à la fois dans le `UpdateParameters` ou `DeleteParameters` spécifié, ainsi que les paramètres de valeur d’origine.

Si ce n’est pas très clair à partir de là, ne vous inquiétez pas, nous allons examiner cette propriété et son utilité dans un futur didacticiel. Pour l’instant, soyez certain supprimez cette déclaration de propriété entièrement à partir de la syntaxe déclarative ou définissez la valeur sur la valeur par défaut ({0}).

> [!NOTE]
> Si vous désactivez simplement le `OldValuesParameterFormatString` valeur de propriété à partir de la fenêtre Propriétés en mode Design, la propriété existe toujours dans la syntaxe déclarative, mais être définie sur une chaîne vide. Ceci, malheureusement, toujours entraîne le problème évoqué ci-dessus. Par conséquent, supprimez la propriété complètement à partir de la syntaxe déclarative ou, à partir de la fenêtre Propriétés, définissez la valeur sur la valeur par défaut, `{0}`.


## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>Étape 3 : Ajout d’un contrôle Web de données et sa configuration pour la Modification de données

Une fois que ObjectDataSource a été ajouté à la page et configuré, nous sommes prêts à ajouter des données des contrôles Web à la page pour afficher les données et de fournir un moyen pour l’utilisateur final pour le modifier. Nous allons examiner le GridView, DetailsView et FormView séparément, comme ces contrôles Web de données diffèrent par leurs fonctionnalités de modification de données et la configuration.

Comme nous allons voir dans le reste de cet article, ajout d’une modification très basique, insertion et suppression de prise en charge via le contrôle GridView, DetailsView, et le contrôle FormView est vraiment aussi simple que de vérifier quelques cases à cocher. Il existe de nombreuses subtilités et des cases de bord dans le monde réel qui rendent fournissant ces fonctionnalités plus complexe que simplement pointez et cliquez sur. Ce didacticiel, toutefois, est consacré exclusivement prouvant les fonctionnalités de modification de données simple. Didacticiels futures examine les problèmes qui seront manifesteront dans un paramètre du monde réel.

## <a name="deleting-data-from-the-gridview"></a>Suppression de données dans le contrôle GridView

Nous allons faire glisser un GridView à partir de la boîte à outils vers le concepteur. Ensuite, liez ObjectDataSource au GridView en le sélectionnant dans la liste déroulante dans la balise active le contrôle GridView. À ce stade balisage déclaratif de GridView sera :


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample4.aspx)]

Lier le contrôle GridView à ObjectDataSource via sa balise active présente deux avantages :

- BoundFields et CheckBoxFields sont automatiquement créés pour chacun des champs retournés par l’ObjectDataSource. En outre, les propriétés de la BoundField et du CheckBoxField sont définies en fonction sur les métadonnées du champ sous-jacent. Par exemple, le `ProductID`, `CategoryName`, et `SupplierName` champs sont marqués comme étant en lecture seule dans le `ProductsDataTable` et par conséquent ne doit pas être mis à jour lors de la modification. Pour prendre en compte cette, ces BoundFields [propriétés en lecture seule](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx) sont définies sur `true`.
- Le [propriété DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx) est affectée aux ou les champs clés primaires de l’objet sous-jacent. Ceci est essentiel quand à l’aide de la GridView pour modifier ou supprimer des données, comme cette propriété indique le champ (ou un ensemble de champs) unique qui identifie chaque enregistrement. Pour plus d’informations sur la `DataKeyNames` propriété, vous référer à la [maître/détail utilisant un GridView maître avec un DetailView détail](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) didacticiel.

Tandis que le contrôle GridView peut être lié à ObjectDataSource via la fenêtre Propriétés ou de la syntaxe déclarative, cela vous oblige à ajouter manuellement le BoundField approprié et `DataKeyNames` balisage.

Le contrôle GridView fournit la prise en charge intégrée pour la modification au niveau des lignes et la suppression. Configuration d’un GridView pour prendre en charge la suppression ajoute une colonne de boutons de suppression. Lorsque l’utilisateur final clique sur le bouton Supprimer pour une ligne particulière, s’ensuit une publication (postback) et le contrôle GridView effectue les étapes suivantes :

1. L’ObjectDataSource `DeleteParameters` ou les valeurs sont affectées.
2. L’ObjectDataSource `Delete()` méthode est appelée, la suppression de l’enregistrement spécifié
3. Le contrôle GridView se lie au ObjectDataSource en appelant son `Select()` (méthode)

Les valeurs affectées à la `DeleteParameters` sont les valeurs de la `DataKeyNames` ou les champs de la ligne dont bouton Supprimer l’utilisateur a cliqué. Par conséquent, il est essentiel que d’un GridView `DataKeyNames` propriété être définie correctement. S’il est manquant, le `DeleteParameters` recevront un `null` valeur à l’étape 1, ce qui à son tour pas entraînera un supprimé des enregistrements à l’étape 2.

> [!NOTE]
> Le `DataKeys` collection est stockée dans l’état du contrôle GridView s, ce qui signifie que le `DataKeys` valeurs seront mémorisés sur la publication (postback), même si l’état d’affichage GridView s a été désactivé. Toutefois, il est très important que l’état d’affichage reste activée pour les contrôles GridView qui prennent en charge la modification ou la suppression (le comportement par défaut). Si vous définissez le s GridView `EnableViewState` propriété `false`, la modification et suppression de comportement fonctionnent pour un seul utilisateur, mais s’il existe des utilisateurs simultanés, la suppression des données, il est possible que ces utilisateurs simultanés peuvent accidentellement supprimer ou modifier des enregistrements qu’ils fonctionné t avez l’intention. Consultez mon billet de blog, [Avertissement : problème d’accès concurrentiel avec ASP.NET 2.0 GridViews/DetailsView/FormViews cette modification de la prise en charge et/ou la suppression et la dont l’état d’affichage est désactivé](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx), pour plus d’informations.


Cet avertissement même s’applique également aux DetailsViews et FormViews.

Pour ajouter des fonctionnalités de suppression sur un GridView, simplement accéder à sa balise active et activez la case à cocher Activer la suppression.


![Vérifiez l’activer la suppression de case à cocher](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image24.png)

**Figure 10**: Vérifiez l’activer la suppression de case à cocher


La vérification de la case à cocher Activer la suppression de la balise active ajoute un CommandField au GridView. Le CommandField restitue une colonne dans le contrôle GridView avec des boutons permettant d’effectuer une ou plusieurs des tâches suivantes : sélectionner un enregistrement, la modification d’un enregistrement et la suppression d’un enregistrement. Nous avons précédemment vu la CommandField en action sans crainte des enregistrements dans la [maître/détail utilisant un GridView maître avec un DetailView détail](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) didacticiel.

Le CommandField contient un nombre de `ShowXButton` propriétés qui indiquent quelle série de boutons est affichés dans le CommandField. En cochant la case Activer la suppression un CommandField dont `ShowDeleteButton` est propriété `true` a été ajouté à la collection de colonnes du contrôle GridView.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample5.aspx)]

À ce stade, croyez-le ou non, nous avons terminé avec l’ajout d’une prise en charge la suppression au GridView ! Comme le montre la Figure 11, lorsque cette page via un navigateur, une colonne de boutons de suppression se trouve.


[![Le CommandField ajoute une colonne de boutons de suppression](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image25.png)

**Figure 11**: le CommandField ajoute une colonne de boutons Supprimer ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image27.png))


Si vous aviez créé ce didacticiel à partir de zéro sur votre propre, lorsque vous testez cette page en cliquant sur le bouton Supprimer lève une exception. Poursuivez votre lecture pour en savoir plus sur pourquoi ces exceptions ont été générées et comment les résoudre.

> [!NOTE]
> Si vous programmez à l’aide du téléchargement qui accompagne ce didacticiel, ces problèmes ont déjà été pris en compte. Toutefois, je vous encourage à lire les détails ci-dessous pour vous aider à identifier les problèmes qui peuvent survenir et les solutions de contournement appropriées.


Si, lorsque vous tentez de supprimer un produit, vous obtenez une exception dont le message est similaire à «*ObjectDataSource 'ObjectDataSource1' n’a pas trouvé une méthode non générique 'DeleteProduct' qui possède des paramètres : productID, d’origine\_ ProductID*, « vous probablement oublié de supprimer le `OldValuesParameterFormatString` propriété à partir de l’ObjectDataSource. Avec le `OldValuesParameterFormatString` propriété spécifiée, ObjectDataSource tente de passer à la fois dans `productID` et `original_ProductID` paramètres d’entrée de la `DeleteProduct` (méthode). `DeleteProduct`, toutefois, il accepte uniquement un paramètre d’entrée unique, par conséquent, l’exception. Suppression de la `OldValuesParameterFormatString` propriété (ou la valeur `{0}`) indique à ObjectDataSource au lieu d’essayer de transmettre le paramètre d’entrée d’origine.


[![Assurez-vous que la propriété OldValuesParameterFormatString a été effacée](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image28.png)

**Figure 12**: Vérifiez que le `OldValuesParameterFormatString` propriété a été effacé Out ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image30.png))


Même si vous aviez supprimé le `OldValuesParameterFormatString` propriété, vous en obtiendrez une exception lorsque vous tentez de supprimer un produit avec le message : «*l’instruction DELETE est en conflit avec la contrainte de référence ' FK\_ordre\_détails \_Produits*. » La base de données Northwind contient une contrainte de clé étrangère entre la `Order Details` et `Products` table, ce qui signifie qu’un produit ne peut pas être supprimé du système s’il existe un ou plusieurs enregistrements pour lui dans le `Order Details` table. Dans la mesure où chaque produit dans la base de données Northwind est associé au moins un enregistrement `Order Details`, nous ne pouvons pas supprimer tous les produits jusqu'à ce que nous supprimons tout d’abord les enregistrements de détails de commande associée du produit.


[![Une contrainte de clé étrangère interdit la suppression de produits](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image31.png)

**Figure 13**: une contrainte de clé étrangère interdit la suppression de produits ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image33.png))


Dans notre didacticiel, nous allons simplement supprimer tous les enregistrements à partir de la `Order Details` table. Dans une application réelle, nous devons soit :

- Écran d’un autre pour gérer les informations de détails de commande
- Augmenter la `DeleteProduct` d’inclure la logique pour supprimer les détails de la commande du produit spécifié (méthode)
- Modifier la requête SQL utilisée par le TableAdapter à inclure la suppression des détails de commande du produit spécifié

Nous allons simplement supprimer tous les enregistrements à partir de la `Order Details` table contourner les mesures de la contrainte de clé étrangère. Accédez à l’Explorateur de serveurs dans Visual Studio, avec le bouton droit sur le `NORTHWND.MDF` nœud, puis choisissez la nouvelle requête. Puis, dans la fenêtre de requête, exécutez l’instruction SQL suivante : `DELETE FROM [Order Details]`


[![Supprimez tous les enregistrements à partir de la Table Order Details](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image34.png)

**Figure 14**: supprimer tous les enregistrements à partir de la `Order Details` Table ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image36.png))


Après éliminant le `Order Details` table en cliquant sur le bouton Supprimer supprimera le produit sans erreur. Si vous cliquez sur le bouton de suppression ne supprime pas le produit, vérifiez que le contrôle GridView `DataKeyNames` propriété est définie sur le champ de clé primaire (`ProductID`).

> [!NOTE]
> Lorsque vous cliquez sur le bouton Supprimer s’ensuit une publication (postback) et l’enregistrement est supprimé. Cela peut être dangereux, car il est facile d’accidentellement cliquez sur le bouton de suppression de la ligne incorrecte. Dans un futur didacticiel, nous allons voir comment ajouter une confirmation côté client lors de la suppression d’un enregistrement.


## <a name="editing-data-with-the-gridview"></a>Modification des données avec le contrôle GridView

En même temps que la suppression, le contrôle GridView fournit également une prise en charge intégrée de modification au niveau des lignes. Configuration d’un GridView pour prendre en charge la modification ajoute une colonne de boutons d’édition. Du point de vue de l’utilisateur final, en cliquant sur les causes de bouton de modification d’une ligne ligne devienne modifiable, transformer les cellules en zones de texte contenant les valeurs existantes et en remplaçant le bouton Modifier avec mise à jour et les boutons d’annulation. Après avoir apporté les modifications souhaitées, l’utilisateur final pouvez cliquer sur le bouton de mise à jour pour valider les modifications ou le bouton Annuler pour les ignorer. Dans les deux cas, après avoir cliqué sur mise à jour ou annuler le GridView retourne à son état avant modification.

À partir de notre point de vue en tant que le développeur de pages, lorsque l’utilisateur final clique sur le bouton Modifier pour une ligne particulière, s’ensuit une publication (postback) et le contrôle GridView effectue les étapes suivantes :

1. Le contrôle GridView `EditItemIndex` propriété est affectée à l’index de la ligne dont bouton Modifier
2. Le contrôle GridView se lie au ObjectDataSource en appelant son `Select()` (méthode)
3. L’index de ligne qui correspond à la `EditItemIndex` est restitué en « mode de modification ». Dans ce mode, le bouton Modifier est remplacé par les boutons Annuler et de mise à jour et BoundFields dont `ReadOnly` propriétés ont la valeur False (valeur par défaut) sont rendus en tant que contrôles de zone de texte Web dont la propriété `Text` propriétés sont attribuées aux valeurs de champs de données.

À ce stade, le balisage est retourné au navigateur, ce qui permet de l’utilisateur final d’apporter des modifications pour les données de la ligne. Lorsque l’utilisateur clique sur le bouton de mise à jour, une publication (postback) se produit et le contrôle GridView effectue les étapes suivantes :

1. L’ObjectDataSource `UpdateParameters` ou les valeurs sont affectées les valeurs entrées par l’utilisateur final dans l’interface de modification du contrôle GridView
2. L’ObjectDataSource `Update()` méthode est appelée, la mise à jour de l’enregistrement spécifié
3. Le contrôle GridView se lie au ObjectDataSource en appelant son `Select()` (méthode)

Les valeurs de clé primaires affectées à la `UpdateParameters` à l’étape 1 proviennent les valeurs spécifiées dans le `DataKeyNames` propriété, tandis que les valeurs de clé non primaire sont fournis à partir du texte dans les contrôles Web de la zone de texte pour la ligne modifiée. Comme avec la suppression, il est essentiel que d’un GridView `DataKeyNames` propriété être définie correctement. S’il est manquant, le `UpdateParameters` ont une valeur de clé primaire une `null` valeur à l’étape 1, ce qui à son tour pas entraîne une mise à jour des enregistrements à l’étape 2.

Fonctionnalités d’édition peuvent être activée en vérifiant simplement la case à cocher Activer la modification dans la balise active le contrôle GridView.


![Vérifiez l’activer la case à cocher de modification](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image37.png)

**Figure 15**: Vérifiez l’activer la case à cocher de modification


Vérification de la case à cocher Activer la modification ajoutera un CommandField (si nécessaire) et définissez son `ShowEditButton` propriété `true`. Comme nous l’avons vu précédemment, le CommandField contient un nombre de `ShowXButton` propriétés qui indiquent quelle série de boutons est affichés dans le CommandField. La vérification de la case à cocher Activer la modification ajoute le `ShowEditButton` propriété le CommandField existant :


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample6.aspx)]

C’est tout concerne l’ajout de la prise en charge rudimentaire de modification. Comme Figure16 montre, l’interface de modification est plutôt brute chaque BoundField dont `ReadOnly` propriété est définie sur `false` (la valeur par défaut) est restitué sous la forme d’une zone de texte. Cela inclut les champs comme `CategoryID` et `SupplierID`, qui sont des clés à d’autres tables.


[![Bouton de modification en cliquant sur Chai s affiche la ligne en Mode Édition](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image38.png)

**Figure 16**: s Chai en cliquant sur bouton Modifier affiche la ligne dans le Mode d’édition ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image40.png))


En plus de demander aux utilisateurs de modifier directement les valeurs de clé étrangère, interface de l’interface modification ne dispose pas de plusieurs manières :

- Si l’utilisateur entre un `CategoryID` ou `SupplierID` qui n’existe pas dans la base de données, le `UPDATE` violeront la contrainte de clé étrangère, à l’origine d’une exception à lever.
- Aucune validation n’inclut pas l’interface de modification. Si vous ne fournissez pas une valeur requise (tel que `ProductName`), ou entrez une valeur de chaîne dans laquelle une valeur numérique est attendue (par exemple, entrez « Trop ! » dans la `UnitPrice` zone de texte), une exception sera levée. Un futur didacticiel examine comment ajouter des contrôles de validation à l’interface utilisateur de modification.
- Actuellement, *tous les* des champs de produits qui ne sont pas en lecture seule doivent être inclus dans le contrôle GridView. Si vous deviez supprimer un champ de GridView, par exemple `UnitPrice`, lors de la mise à jour les données, le contrôle GridView ne définirait pas le `UnitPrice` `UpdateParameters` valeur, ce qui pourrait être modifié l’enregistrement de base de données `UnitPrice` à un `NULL` valeur. De même, si un champ obligatoire, tel que `ProductName`, est supprimé à partir de la GridView, la mise à jour échoue avec le même «*colonne « ProductName » n’autorise pas les valeurs null*« exception mentionné ci-dessus.
- La mise en forme interface édition laisse beaucoup à désirer. Le `UnitPrice` s’affiche avec quatre décimales. Dans l’idéal, le `CategoryID` et `SupplierID` valeurs contiendrait DropDownList qui répertorient les catégories et les fournisseurs dans le système.

Il s’agit de tous les défauts que nous aurons au quotidien pour l’instant, mais n’y être traités dans les didacticiels futures.

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>Insertion, la modification et suppression de données avec le contrôle DetailsView

Comme nous l’avons vu dans les didacticiels précédents, le contrôle DetailsView affiche un enregistrement à la fois et, comme le contrôle GridView, permettant à la modification et suppression de l’enregistrement actuellement affiché. Expérience à la fois l’utilisateur final avec la modification et suppression des éléments à partir d’un contrôle DetailsView et le flux de travail à partir du côté ASP.NET est identique à celle du contrôle GridView. Où le contrôle DetailsView diffère de la GridView est qu’il fournit également une prise en charge l’insertion intégrée.

Pour illustrer les fonctionnalités de modification de données du contrôle GridView, commencez par ajouter un contrôle DetailsView pour le `Basics.aspx` page au-dessus de la GridView existant et liez-le au ObjectDataSource existant via la balise active de DetailsView. Ensuite, désactivez out de DetailsView `Height` et `Width` propriétés et l’option Activer la pagination de la balise active. Pour activer la modification, insertion et suppression de la prise en charge, simplement cocher les cases à cocher Activer la modification, activer l’insertion et activer la suppression de la balise active.


![Configurer le contrôle DetailsView au support technique de modification, d’insertion et de suppression](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image41.png)

**Figure 17**: configurer le contrôle DetailsView au support technique de modification, d’insertion et de suppression


Comme avec le contrôle GridView, ajout de modification, insertion ou la suppression de prise en charge ajoute un CommandField au contrôle DetailsView, comme le montre la syntaxe déclarative suivant :


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample7.aspx)]

Notez que pour le contrôle DetailsView le CommandField apparaît à la fin de la collection de colonnes par défaut. Dans la mesure où les champs de DetailsView sont rendus sous forme de lignes, le CommandField apparaît sous la forme d’une ligne avec l’instruction Insert, modifier et supprimer des boutons au bas de DetailsView.


[![Configurer le contrôle DetailsView au support technique de modification, d’insertion et de suppression](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image42.png)

**Figure 18**: configurer le contrôle DetailsView à la prise en charge de modification, insertion et suppression ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image44.png))


En cliquant sur le bouton Supprimer démarre la même séquence d’événements comme avec le contrôle GridView : une publication (postback) ; suivi par le contrôle DetailsView remplissant son ObjectDataSource `DeleteParameters` selon le `DataKeyNames` valeurs ; et terminé par un appel son ObjectDataSource `Delete()` (méthode), ce qui supprime réellement le produit à partir de la base de données. Modification dans le contrôle DetailsView fonctionne également de manière identique à celle du contrôle GridView.

Pour l’insertion, l’utilisateur final est présenté avec un nouveau bouton qui, lorsque vous cliquez dessus, restitue le contrôle DetailsView en « mode d’insertion ». Avec le « mode insertion » le nouveau bouton est remplacé par les boutons Insérer et annuler et uniquement ces BoundFields dont `InsertVisible` propriété est définie sur `true` (la valeur par défaut) sont affichés. Ces champs de données identifiés en tant que champs de l’incrémentation automatique, tel que `ProductID`, ont leurs [InsertVisible propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx) défini sur `false` lorsque vous liez le contrôle DetailsView à la source de données via la balise active.

Lors de la liaison d’une source de données à un contrôle DetailsView via la balise active, Visual Studio définit le `InsertVisible` propriété `false` uniquement pour les champs de l’incrémentation automatique. Les champs en lecture seule, tels que `CategoryName` et `SupplierName`, apparaît dans l’interface utilisateur de « mode insertion », sauf si leur `InsertVisible` propriété est définie explicitement sur `false`. Prenez un moment pour définir ces deux champs `InsertVisible` propriétés à `false`, soit via la syntaxe déclarative de DetailsView ou modifier les champs lien dans la balise active. Figure 19 montre l’attribution du `InsertVisible` propriétés à `false` en cliquant sur Modifier les champs lien.


[![Northwind Traders propose désormais Acme thé](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image45.png)

**Figure 19**: Northwind Traders maintenant offre Acme thé ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image47.png))


Après avoir défini le `InsertVisible` propriétés, affichez le `Basics.aspx` page dans un navigateur et cliquez sur le bouton Nouveau. La figure 20 montre le contrôle DetailsView lors de l’ajout d’une nouvelle boisson, thé Acme, à notre gamme de produits.


[![Northwind Traders propose désormais Acme thé](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image48.png)

**Figure 20**: Northwind Traders maintenant offre Acme thé ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image50.png))


Après avoir saisi les détails pour Acme thé et en cliquant sur le bouton Insérer, s’ensuit une publication (postback) et le nouvel enregistrement est ajouté à la `Products` table de base de données. Dans la mesure où cette DetailsView répertorie les produits dans l’ordre d’apparition dans la table de base de données, nous devons de page au dernier produit afin de voir le nouveau produit.


[![Détails pour Acme thé](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image51.png)

**Figure 21**: détails pour Acme thé ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image53.png))


> [!NOTE]
> Le DetailsView [CurrentMode propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx) indique l’interface qui est affichée et peut prendre l’une des valeurs suivantes : `Edit`, `Insert`, ou `ReadOnly`. Le [DefaultMode propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx) indique le mode de DetailsView reprend après une modification ou insérer est terminé et qu’il est utile pour afficher un contrôle DetailsView qui est définitivement en mode édition ou le mode d’insertion.


Le point et cliquez sur Insertion et la modification des fonctionnalités de DetailsView d’être affectées par les mêmes limitations que le contrôle GridView : l’utilisateur doit entrer existant `CategoryID` et `SupplierID` valeurs dans une zone de texte ; les ne dispose pas de l’interface de logique de validation ; tous les champs de produit qui n’autorisent pas `NULL` valeurs ou vous n’avez pas une valeur par défaut valeur spécifiée au niveau de la base de données doit être inclus dans l’interface de l’insertion et ainsi de suite.

Les techniques que nous allons examiner d’étendre et améliorer le contrôle GridView interface de modification dans les futures articles peuvent être appliquées pour le contrôle DetailsView modification des interfaces et d’insertion également.

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>À l’aide de FormView pour une Interface utilisateur de Modification des données plus Flexible

Le contrôle FormView offre une prise en charge intégrée pour l’insertion, la modification et suppression de données, mais, car elle utilise des modèles au lieu de champs il n’existe aucun emplacement où ajouter le BoundFields ou le CommandField utilisés par les contrôles GridView et DetailsView pour fournir les données interface de modification. Au lieu de cela, cette interface, les contrôles Web pour la collecte utilisateur d’entrée lors de l’ajout d’un nouvel élément ou modifier un existant, ainsi que le nouveau, modifier, supprimer, insérer, mettre à jour et les boutons d’annulation doit être ajoutée manuellement aux modèles appropriés. Heureusement, Visual Studio crée automatiquement l’interface nécessaire lors de la liaison FormView à une source de données via la liste déroulante dans sa balise active.

Pour illustrer ces techniques, commencez par ajouter un FormView à la `Basics.aspx` page et, à partir de la balise active du FormView, liez-le à ObjectDataSource déjà créé. Cela générera un `EditItemTemplate`, `InsertItemTemplate`, et `ItemTemplate` pour le contrôle FormView avec les contrôles Web de la zone de texte pour la collecte d’entrée et les contrôles Web de bouton de l’utilisateur pour la nouveau, modifier, supprimer, insérer, mettre à jour et les boutons d’annulation. En outre, FormView `DataKeyNames` propriété est définie sur le champ de clé primaire (`ProductID`) de l’objet retourné par l’ObjectDataSource. Enfin, vérifiez l’option Activer la pagination dans la balise active du FormView.

L’exemple suivant montre le balisage déclaratif pour FormView `ItemTemplate` une fois que le contrôle FormView a été lié à ObjectDataSource. Par défaut, chaque champ de produit de valeur non booléenne est lié à la `Text` propriété d’un contrôle Web Label lors de chaque champ de valeur booléenne (`Discontinued`) est lié à la `Checked` propriété d’un contrôle de case à cocher Web désactivé. Dans l’ordre pour les boutons Nouveau, Edit et Delete déclencher certains comportements du FormView d’un clic, il est impératif que leurs `CommandName` valeurs `New`, `Edit`, et `Delete`, respectivement.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample8.aspx)]

La figure 22 montre le FormView `ItemTemplate` lorsqu’ils sont affichés via un navigateur. Chaque champ de produit est répertorié avec les boutons Nouveau, Edit et Delete en bas.


[![L’ItemTemplate de FormView de défaut répertorie chaque champ de produit, ainsi que de nouveau, modifier et supprimer des boutons](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image54.png)

**Figure 22**: le défaut FormView `ItemTemplate` répertorie chaque produit champ sur un nouveau, modifier et supprimer des boutons ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image56.png))


Comme avec les contrôles GridView et DetailsView, en cliquant sur le bouton Supprimer ou n’importe quel bouton, LinkButton ou ImageButton dont `CommandName` propriété est définie sur les causes de supprimer une publication (postback), remplit l’ObjectDataSource `DeleteParameters` selon le FormView `DataKeyNames`valeur et appelle l’ObjectDataSource `Delete()` (méthode).

Lorsque l’utilisateur clique sur le bouton Modifier s’ensuit une publication (postback) et les données sont de nouveau liées à la `EditItemTemplate`, qui est responsable du rendu de l’interface de modification. Cette interface inclut les contrôles Web pour la modification des données, ainsi que les boutons de la mise à jour et Annuler. La valeur par défaut `EditItemTemplate` généré par Visual Studio contient une étiquette pour tous les champs incrémentation automatique (`ProductID`), une zone de texte pour chaque champ de valeur non booléenne et une case à cocher pour chaque champ de valeur booléenne. Ce comportement est très similaire à la BoundFields généré automatiquement dans les contrôles GridView et DetailsView.

> [!NOTE]
> Un petit problème de la génération FormView automatique de la `EditItemTemplate` est qu’elle s’affiche à Web de la zone de texte pour les champs qui sont en lecture seule, telles que les contrôles `CategoryName` et `SupplierName`. Nous verrons comment tenir compte de ce peu de temps.


Contrôle de la zone de texte dans le `EditItemTemplate` ont leur `Text` propriété liée à la valeur de leur champ de données correspondant à l’aide *liaison de données bidirectionnelle*. Liaison de données bidirectionnelle, dénoté par `<%# Bind("dataField") %>`, effectue la liaison de données à la fois lors de la liaison de données au modèle et lors du remplissage des paramètres de l’ObjectDataSource d’insertion ou modification d’enregistrements. Autrement dit, lorsque l’utilisateur clique sur le bouton Modifier à partir de la `ItemTemplate`, le `Bind()` méthode retourne la valeur de champ de données spécifié. Une fois que l’utilisateur modifie leur et clique sur mise à jour, les valeurs publiée qui correspondent aux champs de données spécifiés à l’aide `Bind()` sont appliquées à l’ObjectDataSource `UpdateParameters`. Ou bien, liaison de données unidirectionnelle, dénoté par `<%# Eval("dataField") %>`, uniquement récupère les valeurs de champ de données lors de la liaison de données au modèle l’et *pas* retourner les valeurs entrées par l’utilisateur aux paramètres de la source de données de publication (postback).

Le balisage déclaratif suivant montre le FormView `EditItemTemplate`. Notez que le `Bind()` méthode est utilisée dans la syntaxe de liaison de données ici et qui ont les contrôles de mise à jour et Web de bouton Annuler leur `CommandName` propriétés définies en conséquence.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample9.aspx)]

Notre `EditItemTemplate`, à ce point, provoquera une exception levée si nous tentons de l’utiliser. Le problème est que le `CategoryName` et `SupplierName` les champs sont rendus en tant que contrôles de zone de texte Web dans le `EditItemTemplate`. Nous devons soit modifier ces zones de texte en étiquettes ou les supprimer complètement. Nous allons simplement les supprimer entièrement à partir de la `EditItemTemplate`.

Figure 23 montre le FormView dans un navigateur une fois que le bouton Modifier a été cliqué pour Tran. Notez que le `SupplierName` et `CategoryName` champs affichés dans le `ItemTemplate` ne sont plus présents, comme nous vient d’être supprimée à partir du `EditItemTemplate`. Lorsque l’utilisateur clique sur le bouton de mise à jour le contrôle FormView se poursuit dans la même séquence d’étapes que les contrôles GridView et DetailsView.


[![Par défaut EditItemTemplate affiche chaque champ modifiable produit comme une zone de texte ou une case à cocher](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image57.png)

**Figure 23**: par défaut, le `EditItemTemplate` montre chaque produit champ modifiable en tant que zone de texte ou case à cocher ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image59.png))


Lorsque le bouton Insérer un clic sur le FormView `ItemTemplate` s’ensuit une publication (postback). Toutefois, aucune donnée n’est liée à FormView, car un nouvel enregistrement est ajouté. Le `InsertItemTemplate` interface inclut les contrôles Web pour ajouter un nouvel enregistrement, ainsi que les boutons Insérer et Annuler. La valeur par défaut `InsertItemTemplate` généré par Visual Studio contient une zone de texte pour chaque champ de valeur non booléenne et une case à cocher pour chaque champ de valeur booléenne, semblable à générées automatiquement `EditItemTemplate`de l’interface. Les contrôles de zone de texte ont leur `Text` propriété liée à la valeur de leur champ de données correspondant à l’aide de la liaison de données bidirectionnelle.

Le balisage déclaratif suivant montre le FormView `InsertItemTemplate`. Notez que le `Bind()` méthode est utilisée dans la syntaxe de liaison de données ici et qui ont les contrôles d’insertion et Web de bouton Annuler leur `CommandName` propriétés définies en conséquence.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample10.aspx)]

Il existe une subtilité avec la génération FormView automatique de la `InsertItemTemplate`. Plus précisément, les contrôles Web de la zone de texte sont créés même pour les champs qui sont en lecture seule, telles que `CategoryName` et `SupplierName`. Comme avec la `EditItemTemplate`, nous devons supprimer ces zones de texte à partir de la `InsertItemTemplate`.

Figure 24 montre le contrôle FormView dans un navigateur lorsque vous ajoutez un nouveau produit, Acme café. Notez que le `SupplierName` et `CategoryName` champs affichés dans le `ItemTemplate` ne sont plus présents, comme nous vient d’être supprimée. Clic sur le bouton Insérer le continue FormView via la même séquence d’étapes que le contrôle DetailsView, ajouter un nouvel enregistrement à la `Products` table. La figure 25 illustre les détails du produit Acme Coffee dans le contrôle FormView après que qu’il a été inséré.


[![L’élément InsertItemTemplate dicte FormView insertion d’Interface](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image60.png)

**Figure 24**: le `InsertItemTemplate` détermine l’Interface d’insertion de FormView ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image62.png))


[![Les détails pour le nouveau produit, café Acme, sont affichés dans le contrôle FormView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image63.png)

**Figure 25**: détails pour le nouveau produit, café Acme, sont affichés dans le contrôle FormView ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image65.png))


En les répartissant en lecture seule, par édition et insertion des interfaces dans les trois modèles distincts, le contrôle FormView permet un meilleur niveau de contrôle sur ces interfaces que le contrôle DetailsView et GridView.

> [!NOTE]
> Comme le contrôle DetailsView, FormView `CurrentMode` propriété indique l’interface qui est affichée et son `DefaultMode` propriété indique le mode le retourne FormView à après une modification ou d’insertion a été effectuée.


## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons examiné les principes fondamentaux de l’insertion, la modification et la suppression des données à l’aide de la GridView, DetailsView et FormView. Trois de ces contrôles fournissent un certain niveau de fonctionnalités de modification de données intégré qui peut être utilisée sans écrire une seule ligne de code dans la page ASP.NET grâce aux contrôles data Web et de l’ObjectDataSource. Toutefois, le simple pointez et cliquez sur les techniques rendu assez fragiles et interface utilisateur de modification des données naïve. Pour fournir la validation, injecter des valeurs par programmation, gérer correctement les exceptions, personnaliser l’interface utilisateur, et ainsi de suite, nous allons devoir s’appuient sur une multitude de techniques qui nous reviendrons sur les didacticiels plusieurs suivants.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Next](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
