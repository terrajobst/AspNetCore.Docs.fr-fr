---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs
title: Limitation des fonctionnalités de Modification de données basé sur l’utilisateur (c#) | Microsoft Docs
author: rick-anderson
description: Dans une application web qui permet aux utilisateurs de modifier des données, différents comptes d’utilisateur peuvent avoir des privilèges de modification de données différents. Dans ce didacticiel, nous allons examiner comment t...
ms.author: aspnetcontent
ms.date: 07/17/2006
ms.assetid: 2b251c82-77cf-4e36-baa9-b648eddaa394
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs
msc.type: authoredcontent
ms.openlocfilehash: d011f57834ff27efd888a3f66342a7d0a2d70d8c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811391"
---
<a name="limiting-data-modification-functionality-based-on-the-user-c"></a>Limitation des fonctionnalités de Modification de données en fonction de l’utilisateur (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_CS.exe) ou [télécharger le PDF](limiting-data-modification-functionality-based-on-the-user-cs/_static/datatutorial23cs1.pdf)

> Dans une application web qui permet aux utilisateurs de modifier des données, différents comptes d’utilisateur peuvent avoir des privilèges de modification de données différents. Dans ce didacticiel, nous allons examiner comment ajuster dynamiquement les fonctionnalités de modification de données basées sur l’utilisateur visite.


## <a name="introduction"></a>Introduction

Un nombre d’applications web prennent en charge les comptes d’utilisateur et fournir des différentes options, des rapports et des fonctionnalités en fonction de l’utilisateur connecté. Par exemple, avec nos didacticiels nous pourrions permettre aux utilisateurs des sociétés de fournisseur pour ouvrir une session sur les site et mise à jour des informations générales sur leurs produits - leur nom et la quantité par unité, peut-être - ainsi que des informations relatives aux fournisseurs, tels que le nom de leur société, adresse, les informations de la personne à contacter s et ainsi de suite. En outre, nous pourrions inclure certains comptes d’utilisateur pour les personnes à partir de notre entreprise afin qu’ils peuvent se connecter et mettre à jour des informations sur les produits tels que les unités en stock, niveau de réapprovisionnement et ainsi de suite. Notre application web peut autoriser également les utilisateurs anonymes à visiter (personnes qui n’ont pas été consignés sur), mais limiterait à afficher uniquement les données. Avec un tel utilisateur compte système en place, nous aimerions les données des contrôles Web dans nos pages ASP.NET pour offrir l’insertion, la modification et la suppression des privilèges appropriés pour l’utilisateur actuellement connecté.

Dans ce didacticiel, nous allons examiner comment ajuster dynamiquement les fonctionnalités de modification de données basées sur l’utilisateur visite. En particulier, nous allons créer une page qui affiche les informations de fournisseurs dans un contrôle DetailsView modifiable, ainsi que d’un GridView qui répertorie les produits fournis par le fournisseur. Si l’utilisateur accédant à la page est de notre entreprise, ils peuvent : afficher des informations de fournisseur s ; modifier leur adresse ; et modifier les informations pour tous les produits fournis par le fournisseur. Si, toutefois, l’utilisateur provient d’une entreprise particulière, ils peuvent uniquement afficher et modifier leurs propres informations d’adresse et ne peuvent modifier leurs produits qui n’ont pas été marquées comme supprimées.


[![Un utilisateur à partir de notre entreprise peut modifier un fournisseur des informations s](limiting-data-modification-functionality-based-on-the-user-cs/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image1.png)

**Figure 1**: un utilisateur à partir de notre entreprise peut modifier n’importe quel fournisseur s informations ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image3.png))


[![Un utilisateur à partir d’un fournisseur particulier peuvent uniquement afficher et le modifiez leurs informations](limiting-data-modification-functionality-based-on-the-user-cs/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image4.png)

**Figure 2**: un utilisateur à partir d’un particulier fournisseur peut uniquement afficher et modifier leurs informations ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image6.png))


Laissez s commencer !

> [!NOTE]
> ASP.NET 2.0 système d’appartenance s fournit une plate-forme standardisée et extensible pour la création, la gestion et la validation des comptes d’utilisateur. Dans la mesure où un examen du système d’appartenance est dépasse le cadre de ces didacticiels, ce didacticiel à la place « fakes » l’appartenance en autorisant les visiteurs anonymes à choisir s’ils sont d’un fournisseur particulier ou à partir de notre entreprise. Pour plus d’informations sur l’appartenance, reportez-vous à mon [s examinant ASP.NET 2.0 d’appartenance, les rôles et profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx) série d’articles.


## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>Étape 1 : Autoriser l’utilisateur à spécifier leurs droits d’accès

Dans une application web du monde réel, les informations de compte d’utilisateur s inclurait si l’entreprise a collaboré de notre entreprise ou d’un fournisseur particulier, et ces informations pourraient être accessibles par programme à partir de nos pages ASP.NET une fois que l’utilisateur a ouvert une session le site. Ces informations peuvent être capturées via ASP.NET 2.0 rôles du système, en tant qu’informations de compte au niveau de l’utilisateur via le système de profil, ou via certains moyens personnalisés.

Étant donné que l’objectif de ce didacticiel est de montrer à ajuster les fonctionnalités de modification de données en fonction de l’utilisateur connecté et qu’il n’est pas destiné à l’appartenance de s showcase ASP.NET 2.0, les rôles et les systèmes de profil, nous allons utiliser un mécanisme très simplement pour déterminer le fonctionnalités de l’utilisateur accédant à la page - DropDownList à partir de laquelle l’utilisateur peut indiquer s’ils doivent pouvoir afficher et modifier les informations des fournisseurs ou, vous pouvez également ce que les informations de fournisseur particulier s’ils peuvent afficher et modifier. Si l’utilisateur indique qu’elle peut afficher et modifier toutes les informations de fournisseur (la valeur par défaut), elle peut parcourir tous les fournisseurs, modifier les informations d’adresse fournisseur s et modifiez le nom et la quantité par unité pour tous les produits fournis par le fournisseur sélectionné. Si l’utilisateur indique qu’elle peut uniquement afficher et modifier un fournisseur, toutefois, puis elle peut uniquement afficher les détails et les produits pour qu’un fournisseur et peut uniquement mettre à jour le nom et la quantité par les informations relatives aux unités de ces produits sont *pas* abandonné.

Notre première étape dans ce didacticiel, est ensuite, pour créer cette DropDownList et la remplir avec les fournisseurs dans le système. Ouvrez le `UserLevelAccess.aspx` page dans le `EditInsertDelete` dossier, ajouter un contrôle DropDownList dont `ID` propriété est définie sur `Suppliers`et lier cette DropDownList à un nouveau ObjectDataSource nommé `AllSuppliersDataSource`.


[![Créer un nouveau ObjectDataSource nommé AllSuppliersDataSource](limiting-data-modification-functionality-based-on-the-user-cs/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image7.png)

**Figure 3**: créer une nouvelle nommée de ObjectDataSource `AllSuppliersDataSource` ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image9.png))


Étant donné que nous voulons cette DropDownList pour inclure tous les fournisseurs, configurer ObjectDataSource pour appeler le `SuppliersBLL` classe s `GetSuppliers()` (méthode). Vérifiez également que les opérations de mappage ObjectDataSource `Update()` méthode est mappée à la `SuppliersBLL` classe s `UpdateSupplierAddress` méthode, comme ce ObjectDataSource sera également être utilisée par le contrôle DetailsView nous ajouterons à l’étape 2.

À l’issue de l’Assistant ObjectDataSource, effectuez les étapes en configurant le `Suppliers` DropDownList telle qu’elle affiche le `CompanyName` champ de données et utilise le `SupplierID` champ de données comme valeur pour chaque `ListItem`.


[![Configurer la liste de fournisseurs DropDownList pour utiliser le CompanyName et les champs de données de SupplierID](limiting-data-modification-functionality-based-on-the-user-cs/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image10.png)

**Figure 4**: configurer le `Suppliers` DropDownList à utiliser le `CompanyName` et `SupplierID` des champs de données ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image12.png))


À ce stade, l’objet DropDownList répertorie les noms de société des fournisseurs dans la base de données. Toutefois, nous devons également inclure une option « Afficher/modifier tous les fournisseurs » à la liste DropDownList. Pour ce faire, affectez la `Suppliers` DropDownList s `AppendDataBoundItems` propriété `true` , puis ajoutez un `ListItem` dont `Text` propriété est « Afficher/modifier tous les fournisseurs » et dont la valeur est `-1`. Il peut être ajouté directement via le balisage déclaratif ou via le concepteur en accédant à la fenêtre Propriétés et en cliquant sur le bouton de sélection dans le s DropDownList `Items` propriété.

> [!NOTE]
> Faire référence à la [ *filtrage de maître/détail avec un DropDownList* ](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) didacticiel pour une étude plus détaillée sur l’ajout d’un élément sélectionner tout pour un databound DropDownList.


Après le `AppendDataBoundItems` propriété a été définie et la `ListItem` ajouté, le balisage déclaratif de s DropDownList doit ressembler à :


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample1.aspx)]

La figure 5 illustre une capture d’écran de notre progression en cours, lorsqu’ils sont affichés via un navigateur.


[![La liste de fournisseurs DropDownList contient un diaporama tous les ListItem, Plus un pour chaque fournisseur](limiting-data-modification-functionality-based-on-the-user-cs/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image13.png)

**Figure 5**: le `Suppliers` DropDownList contient afficher tout `ListItem`, Plus un pour chaque fournisseur ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image15.png))


Étant donné que nous souhaitons mettre à jour l’interface utilisateur immédiatement une fois que l’utilisateur a changé sa sélection, définir le `Suppliers` DropDownList s `AutoPostBack` propriété `true`. À l’étape 2, nous allons créer un contrôle DetailsView qui affiche les informations pour les fournisseurs en fonction de la sélection de DropDownList. Ensuite, à l’étape 3, nous allons créer un gestionnaire d’événements pour cette s DropDownList `SelectedIndexChanged` événement, dans lequel nous allons ajouter le code qui lie des informations sur les fournisseur approprié pour le contrôle DetailsView basé sur le fournisseur sélectionné.

## <a name="step-2-adding-a-detailsview-control"></a>Étape 2 : Ajout d’un contrôle DetailsView

Permettent un contrôle DetailsView permet d’afficher des informations relatives aux fournisseurs de s. Pour l’utilisateur qui peut afficher et modifier tous les fournisseurs, le contrôle DetailsView prendra en charge la pagination, permettant à l’utilisateur de parcourir la fournisseur d’informations d’un enregistrement à la fois. Si l’utilisateur travaille pour un fournisseur particulier, toutefois, le contrôle DetailsView affichera uniquement ce fournisseur d’informations s et pas une interface de pagination. Dans les deux cas, le contrôle DetailsView doit autoriser l’utilisateur à modifier le fournisseur s adresse, ville et des champs de pays.

Ajouter un contrôle DetailsView à la page sous le `Suppliers` DropDownList, définissez son `ID` propriété `SupplierDetails`, puis liez-le à le `AllSuppliersDataSource` ObjectDataSource créé à l’étape précédente. Ensuite, cochez les cases à cocher Activer la pagination et activer la modification de la balise active de DetailsView s.

> [!NOTE]
> Si ne pas voir une option Activer la modification dans le s DetailsView smart marquez-la s, car vous n’avez pas mappé les s ObjectDataSource `Update()` méthode à la `SuppliersBLL` classe s `UpdateSupplierAddress` (méthode). Prenez un moment pour revenir en arrière et modifier cette configuration, après laquelle l’option Activer la modification doit apparaître dans la balise active de DetailsView s.


Dans la mesure où le `SuppliersBLL` classe s `UpdateSupplierAddress` méthode accepte uniquement les quatre paramètres - `supplierID`, `address`, `city`, et `country` -modifier le s DetailsView BoundFields afin que le `CompanyName` et `Phone` BoundFields sont en lecture seule. Par ailleurs, supprimez le `SupplierID` BoundField complètement. Enfin, le `AllSuppliersDataSource` ObjectDataSource a actuellement ses `OldValuesParameterFormatString` propriété définie sur `original_{0}`. Prenez un moment pour supprimer ce paramètre de propriété complètement de la syntaxe déclarative ou affectez-lui la valeur par défaut, `{0}`.

Après avoir configuré le `SupplierDetails` DetailsView et `AllSuppliersDataSource` ObjectDataSource, nous aurons le balisage déclaratif suivant :


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample2.aspx)]

À ce stade, le contrôle DetailsView peut être contacté via et les informations d’adresse fournisseur sélectionné s peuvent être mis à jour, quelle que soit la sélection effectuée dans le `Suppliers` DropDownList (voir Figure 6).


[![Les fournisseurs d’informations peuvent être affichées et mis à jour de son adresse](limiting-data-modification-functionality-based-on-the-user-cs/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image16.png)

**Figure 6**: Any fournisseurs d’informations peuvent être affichés et son adresse mise à jour ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image18.png))


## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>Étape 3 : Afficher uniquement les informations de s fournisseur sélectionné

Notre page affiche les informations pour tous les fournisseurs indépendamment de si un fournisseur a été sélectionné à partir de la `Suppliers` DropDownList. Afin d’afficher uniquement les informations de fournisseur pour le fournisseur sélectionné, nous devons ajouter un autre ObjectDataSource à notre page, qui Récupère des informations sur un fournisseur particulier.

Ajouter un nouveau ObjectDataSource à la page, nommez-le `SingleSupplierDataSource`. À partir de sa balise active, cliquez sur le lien configurer la Source de données et de sorte qu’il utilise le `SuppliersBLL` classe s `GetSupplierBySupplierID(supplierID)` (méthode). Comme avec la `AllSuppliersDataSource` ObjectDataSource, ont le `SingleSupplierDataSource` ObjectDataSource s `Update()` méthode mappée à la `SuppliersBLL` classe s `UpdateSupplierAddress` (méthode).


[![Configurer pour utiliser la méthode GetSupplierBySupplierID(supplierID) SingleSupplierDataSource ObjectDataSource](limiting-data-modification-functionality-based-on-the-user-cs/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image19.png)

**Figure 7**: configurer le `SingleSupplierDataSource` ObjectDataSource à utiliser le `GetSupplierBySupplierID(supplierID)` (méthode) ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image21.png))


Ensuite, nous re invité à spécifier la source de paramètre pour le `GetSupplierBySupplierID(supplierID)` méthode s `supplierID` paramètre d’entrée. Étant donné que nous voulons afficher les informations pour le fournisseur sélectionné dans la liste DropDownList, utilisez le `Suppliers` DropDownList s `SelectedValue` propriété en tant que le paramètre source.


[![Utiliser la liste de fournisseurs DropDownList en tant que Source du paramètre supplierID](limiting-data-modification-functionality-based-on-the-user-cs/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image22.png)

**Figure 8**: utilisez le `Suppliers` DropDownList en tant que le `supplierID` Source du paramètre ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image24.png))


Même avec cette deuxième ObjectDataSource ajouté, le contrôle DetailsView est actuellement configuré pour toujours utiliser le `AllSuppliersDataSource` ObjectDataSource. Nous devons ajouter la logique pour ajuster la source de données utilisée par le contrôle DetailsView selon le `Suppliers` DropDownList élément sélectionné. Pour ce faire, créez un `SelectedIndexChanged` Gestionnaire d’événements pour l’objet DropDownList de fournisseurs. Celui-ci peut aisément être créé en double-cliquant sur l’objet DropDownList dans le concepteur. Ce gestionnaire d’événements doit déterminer quelle source de données à utiliser et devez réassocier les données pour le contrôle DetailsView. Cela s’effectue par le code suivant :


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample3.cs)]

Le Gestionnaire d’événements commence par déterminer si l’option « Afficher/modifier tous les fournisseurs » a été sélectionnée. S’il s’agissait, il définit le `SupplierDetails` DetailsView s `DataSourceID` à `AllSuppliersDataSource` et renvoie l’utilisateur vers le premier enregistrement dans le jeu de fournisseurs en définissant le `PageIndex` 0 à la propriété. Si, toutefois, l’utilisateur a sélectionné un fournisseur dans la liste DropDownList, le contrôle DetailsView s `DataSourceID` est affectée à `SingleSuppliersDataSource`. Quelles que soient les données source est utilisée, le `SuppliersDetails` mode est restauré vers le mode en lecture seule et les données sont de nouveau liées au contrôle DetailsView par un appel à la `SuppliersDetails` contrôle s `DataBind()` (méthode).

Avec ce gestionnaire d’événements en place, le contrôle DetailsView affiche maintenant le fournisseur sélectionné, sauf si l’option « Afficher/modifier tous les fournisseurs » a été sélectionnée, auquel cas tous les fournisseurs peuvent être affichés via l’interface de pagination. La figure 9 illustre la page avec l’option « Afficher/modifier tous les fournisseurs » sélectionnée ; Notez que l’interface de pagination est présent, permettant à l’utilisateur à consulter et mettre à jour de n’importe quel fournisseur. Figure 10 montre la page avec le fournisseur de Ma Maison sélectionné. Seules les informations de s Ma Maison sont affichable et modifiable dans ce cas.


[![Toutes les informations de fournisseurs peuvent être affichées et modifiées](limiting-data-modification-functionality-based-on-the-user-cs/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image25.png)

**Figure 9**: tous les fournisseurs les informations peuvent être consultées et modifiées ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image27.png))


[![Uniquement les informations du fournisseur sélectionné s peuvent être affichées et modifiées](limiting-data-modification-functionality-based-on-the-user-cs/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image28.png)

**Figure 10**: seules les opérations de mappage sélectionné un fournisseur d’informations peuvent être affichage et modifiée ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image30.png))


> [!NOTE]
> Pour ce didacticiel, le DropDownList et DetailsView contrôlent s `EnableViewState` doit être définie sur `true` (la valeur par défaut), car les opérations de mappage DropDownList `SelectedIndex` et les opérations de mappage DetailsView `DataSourceID` modifications apportées aux propriétés s doit être mémorisées entre les postbacks.


## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>Étape 4 : Répertorier les fournisseurs de produits dans un GridView modifiable

Avec le contrôle DetailsView terminé, notre étape suivante consiste à inclure un GridView modifiable qui répertorie les produits fournis par le fournisseur sélectionné. Ce GridView doit autoriser les modifications seulement le `ProductName` et `QuantityPerUnit` champs. En outre, si l’utilisateur accédant à la page provient d’un fournisseur particulier, il ne doit autoriser les mises à jour sur les produits qui sont *pas* abandonné. Pour cela nous devons tout d’abord ajouter une surcharge de la `ProductsBLL` classe s `UpdateProducts` méthode qui accepte uniquement le `ProductID`, `ProductName`, et `QuantityPerUnit` champs en tant qu’entrées. Nous ve a présenté ce processus au préalable dans nombreux didacticiels, je donc s regardez tout simplement ici, le code qui doit être ajouté à `ProductsBLL`:


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample4.cs)]

Avec cette surcharge créée, nous vous êtes prêt à ajouter le contrôle GridView et son ObjectDataSource associé. Ajouter un nouveau GridView à la page, définissez son `ID` propriété `ProductsBySupplier`et configurez-le pour utiliser un nouveau ObjectDataSource nommé `ProductsBySupplierDataSource`. Étant donné que nous voulons ce GridView pour répertorier les produits par le fournisseur sélectionné, utilisez le `ProductsBLL` classe s `GetProductsBySupplierID(supplierID)` (méthode). Également mapper la `Update()` méthode vers le nouveau `UpdateProduct` surcharge que nous venons de créer.


[![Configurer l’ObjectDataSource pour utiliser la surcharge UpdateProduct venez de créer](limiting-data-modification-functionality-based-on-the-user-cs/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image31.png)

**Figure 11**: configurer ObjectDataSource à utiliser le `UpdateProduct` surcharger venez de créer ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image33.png))


Nous re vous y êtes invité à sélectionner la source de paramètre pour le `GetProductsBySupplierID(supplierID)` méthode s `supplierID` paramètre d’entrée. Étant donné que nous voulons afficher les produits pour le fournisseur sélectionné dans le contrôle DetailsView, utilisez le `SuppliersDetails` contrôle DetailsView s `SelectedValue` propriété en tant que le paramètre source.


[![Utilisez la propriété SelectedValue de s SuppliersDetails DetailsView comme Source de paramètre](limiting-data-modification-functionality-based-on-the-user-cs/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image34.png)

**Figure 12**: utilisez le `SuppliersDetails` DetailsView s `SelectedValue` propriété en tant que la Source de paramètre ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image36.png))


Retour au GridView, supprimez tous les champs à l’exception de GridView `ProductName`, `QuantityPerUnit`, et `Discontinued`, marquage le `Discontinued` CheckBoxField en lecture seule. En outre, cochez la case Activer la modification de la balise active de s GridView. Après ont apporté ces modifications, le balisage déclaratif pour les contrôles GridView et ObjectDataSource doit ressembler à ce qui suit :


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample5.aspx)]

Comme avec notre ObjectDataSources précédente, cette un s `OldValuesParameterFormatString` propriété est définie sur `original_{0}`, ce qui entraîne des problèmes lorsque vous tentez de mettre à jour d’un nom de produit s ou de la quantité par unité. Supprimer cette propriété à partir de la syntaxe déclarative complètement ou définissez-le sur sa valeur par défaut, `{0}`.

Avec cette configuration terminée, notre page répertorie maintenant les produits fournis par le fournisseur sélectionné dans le contrôle GridView (voir Figure 13). Actuellement *n’importe quel* nom de produit s ou de la quantité par unité peut être mis à jour. Toutefois, nous devons mettre à jour notre logique de page afin que cette fonctionnalité est interdite pour les produits interrompus pour les utilisateurs associés à un fournisseur particulier. Nous aborderons ce dernier à l’étape 5.


[![Les produits fournis par le fournisseur sélectionné sont affichés.](limiting-data-modification-functionality-based-on-the-user-cs/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image37.png)

**Figure 13**: les produits fournis par le fournisseur sélectionné sont affichés ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image39.png))


> [!NOTE]
> Avec l’ajout de ce GridView modifiable le `Suppliers` DropDownList s `SelectedIndexChanged` Gestionnaire d’événements doit être mis à jour pour retourner le contrôle GridView à un état en lecture seule. Sinon, si un autre fournisseur est sélectionné au cours de la modification des informations de produit, l’index correspondant dans le contrôle GridView pour le nouveau fournisseur peuvent également être modifiable. Pour éviter ce problème, il suffit de définir les opérations de mappage GridView `EditIndex` propriété `-1` dans le `SelectedIndexChanged` Gestionnaire d’événements.


En outre, rappelez-vous qu’il est important que le contrôle GridView état d’affichage s être activée (le comportement par défaut). Si vous définissez le s GridView `EnableViewState` propriété `false`, vous courez le risque de permettre aux utilisateurs simultanés involontairement suppression ou modification d’enregistrements. Consultez [Avertissement : problème d’accès concurrentiel avec ASP.NET 2.0 GridViews/DetailsView/FormViews cette modification de la prise en charge et/ou la suppression et la dont l’état d’affichage est désactivé](http://scottonwriting.net/sowblog/posts/10054.aspx) pour plus d’informations.

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>Étape 5 : Interdire l’édition pour abandonné produits quand afficher/modifier tous les fournisseurs n’est pas sélectionnée

Bien que le `ProductsBySupplier` GridView est entièrement fonctionnel, il actuellement accorde un trop grand nombre d’accès aux utilisateurs qui sont d’un fournisseur particulier. Par nos règles d’entreprise, ces utilisateurs ne doivent pas être en mesure de mettre à jour de produits interrompus. Pour appliquer cette recommandation, nous pouvons masquer (ou désactiver) le bouton Modifier dans les lignes de GridView avec les produits interrompus lorsque la page est visitée par un utilisateur à partir d’un fournisseur.

Créer un gestionnaire d’événements pour les opérations de mappage GridView `RowDataBound` événement. Dans ce gestionnaire d’événements, nous avons besoin déterminer si l’utilisateur est associé à un fournisseur, qui, pour ce didacticiel, peut être déterminé en vérifiant le s fournisseurs DropDownList `SelectedValue` propriété - si elle est s quelque chose d’autre que -1, puis l’utilisateur associé à un fournisseur particulier. Dans ce cas, nous devons ensuite déterminer si le produit n’est plus disponible. Nous pouvons récupérer une référence au véritable `ProductRow` instance liée à la ligne GridView via le `e.Row.DataItem` propriété, comme indiqué dans le [ *affichant des informations de résumé dans le pied de page/s GridView* ](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) didacticiel. Si le produit n’est plus disponible, nous pouvons récupérer une référence de programmation pour le bouton Modifier dans le s GridView CommandField utilisant les techniques présentées dans le didacticiel précédent, [ *Ajout côté Client Confirmation lors de la suppression* ](adding-client-side-confirmation-when-deleting-cs.md). Une fois que nous avons une référence, nous pouvons ensuite masquer ou désactiver le bouton.


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample6.cs)]

Avec cet événement gestionnaire en place, lors de la visite de cette page en tant qu’utilisateur à partir d’un fournisseur particulier ces produits sont supprimés ne sont pas modifiables, comme le bouton Modifier est masqué pour ces produits. Par exemple, Chef Anton s Gumbo Mix est un produit supprimée pour le fournisseur de la Nouvelle-Orléans Cajun divertissement. Lorsque vous visitez la page pour ce fournisseur particulier, le bouton Modifier pour ce produit est masqué (voir Figure 14). Toutefois, lors de la visite à l’aide de « Afficher/modifier tous les fournisseurs », le bouton Modifier est disponible (voir Figure 15).


[![Pour les utilisateurs de fournisseur spécifique, le bouton Modifier pour Chef Anton s Gumbo Mix est masqué](limiting-data-modification-functionality-based-on-the-user-cs/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image40.png)

**Figure 14**: pour fournisseurs spécifiques aux utilisateurs le bouton Modifier pour Chef Anton s Gumbo Mix est masqué ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image42.png))


[![Pour afficher ou modifier tous les utilisateurs de fournisseurs, le bouton Modifier pour Chef Anton s Gumbo Mix est affiché](limiting-data-modification-functionality-based-on-the-user-cs/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image43.png)

**Figure 15**: pour afficher ou modifier tous les fournisseurs les utilisateurs, le bouton Modifier pour s Chef Anton Gumbo Mix s’affiche ([cliquez pour afficher l’image en taille réelle](limiting-data-modification-functionality-based-on-the-user-cs/_static/image45.png))


## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>Vérification des droits d’accès dans la couche de logique métier

Dans ce didacticiel ASP.NET page gère toute la logique en ce qui concerne les informations que l’utilisateur peut voir et les produits qu’il peut mettre à jour. Dans l’idéal, cette logique serait également présente au niveau de la couche de logique métier. Par exemple, le `SuppliersBLL` classe s `GetSuppliers()` (méthode) (qui retourne tous les fournisseurs) peut inclure une vérification pour vous assurer que l’utilisateur actuellement connecté est *pas* associé à un fournisseur spécifique. De même, la `UpdateSupplierAddress` méthode peut inclure une vérification pour vous assurer que l’utilisateur actuellement connecté soit travaillé pour notre entreprise (et donc peut mettre à jour toutes les informations d’adresse de fournisseurs) ou est associé avec le fournisseur dont les données sont en cours de mise à jour.

Je n’incluent pas ces contrôles de la couche BLL ici, car dans notre didacticiel, les droits d’utilisateur s sont déterminées par un contrôle DropDownList sur la page, les classes de la couche BLL ne peut pas accéder. Lorsque vous utilisez le système d’appartenance ou un des schémas d’authentification hors de la zone fournies par ASP.NET (par exemple, l’authentification Windows), actuellement connecté sur utilisateur s informations et des informations sur les rôles sont accessibles à partir de la couche BLL, rendant ce type d’accès droits vérifie possible à la présentation et les couches de la couche BLL.

## <a name="summary"></a>Récapitulatif

La plupart des sites qui fournissent des comptes d’utilisateurs ont besoin personnaliser l’interface de modification de données en fonction de l’utilisateur connecté. Les utilisateurs administratifs peuvent être en mesure de supprimer et modifier n’importe quel enregistrement, alors que les utilisateurs non-administrateurs peuvent être limitées avec uniquement la mise à jour ou la suppression des enregistrements, ils ont créés eux-mêmes. Quel que soit le scénario peut être, les contrôles Web, ObjectDataSource, données et classes de la couche de logique métier peuvent être étendues pour ajouter ou de refuser certaines fonctionnalités en fonction de l’utilisateur connecté. Dans ce didacticiel, nous avons vu comment limiter les données visibles et modifiables selon si l’utilisateur a été associé à un fournisseur particulier ou s’ils ont travaillé de notre entreprise.

Ce didacticiel conclut notre examen de l’insertion, la mise à jour et suppression de données à l’aide des contrôles GridView, DetailsView et FormView. À partir de l’étape suivante du didacticiel, nous allons porter notre attention à l’ajout de pagination et tri de prise en charge.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](adding-client-side-confirmation-when-deleting-cs.md)
> [Suivant](an-overview-of-inserting-updating-and-deleting-data-vb.md)
