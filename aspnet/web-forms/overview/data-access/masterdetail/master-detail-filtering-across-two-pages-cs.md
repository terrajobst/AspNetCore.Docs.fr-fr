---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
title: Maître/détail filtrage entre les deux Pages (c#) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons implémenter ce modèle à l’aide d’un GridView pour répertorier les fournisseurs dans la base de données. Chaque ligne du fournisseur dans le contrôle GridView contiendra une vue...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 552d2d50-fe73-4153-9a7f-2b379bec4625
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 48102f64b1ec832774d9b41258503937dae4f4d5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373118"
---
<a name="masterdetail-filtering-across-two-pages-c"></a>Maître/détail filtrage entre les deux Pages (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_9_CS.exe) ou [télécharger le PDF](master-detail-filtering-across-two-pages-cs/_static/datatutorial09cs1.pdf)

> Dans ce didacticiel, nous allons implémenter ce modèle à l’aide d’un GridView pour répertorier les fournisseurs dans la base de données. Chaque ligne du fournisseur dans le contrôle GridView contiendra un lien Afficher les produits que, quand vous cliquez sur, dirige l’utilisateur vers une page séparée qui répertorie ces produits pour le fournisseur sélectionné.


## <a name="introduction"></a>Introduction

Dans les deux didacticiels précédents, nous avons vu comment [afficher les rapports maître/détail dans une seule page web à l’aide de DropDownList](master-detail-filtering-with-a-dropdownlist-cs.md) à [afficher les enregistrements « maîtres » et un contrôle GridView ou DetailsView](master-detail-filtering-with-two-dropdownlists-cs.md) pour afficher le » plus d’informations. » Un autre modèle commun utilisé pour les rapports maître/détails consiste à disposer les enregistrements maîtres sur une page web et les détails affichés sur un autre. Un site Web forum, comme [les Forums ASP.NET](https://forums.asp.net/), constitue un excellent exemple de ce modèle dans la pratique. Les Forums ASP.NET sont composées d’une variété de forums mise en route, Web Forms, les contrôles de présentation de données et ainsi de suite. Chaque forum est composée de nombreux threads et chaque thread est composé d’un nombre de publications. Sur la page d’accueil des Forums ASP.NET, les forums sont répertoriés. En cliquant sur un forum vous emmène à un `ShowForum.aspx` page qui répertorie tous les threads du forum. De même, sur un thread vous amène à `ShowPost.aspx`, qui affiche les publications pour le thread qui a été cliqué.

Dans ce didacticiel, nous allons implémenter ce modèle à l’aide d’un GridView pour répertorier les fournisseurs dans la base de données. Chaque ligne du fournisseur dans le contrôle GridView contiendra un lien Afficher les produits que, quand vous cliquez sur, dirige l’utilisateur vers une page séparée qui répertorie ces produits pour le fournisseur sélectionné.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>Étape 1 : Ajout de`SupplierListMaster.aspx`et`ProductsForSupplierDetails.aspx`des Pages à la`Filtering`dossier

Lors de la définition de la mise en page dans le troisième didacticiel nous avons ajouté un nombre de pages « starter » dans le `BasicReporting`, `Filtering`, et `CustomFormatting` dossiers. Toutefois, nous n’avons pas ajouté une page de démarrage pour ce didacticiel à ce moment-là, par conséquent, prenez un moment pour ajouter deux nouvelles pages à la `Filtering` dossier : `SupplierListMaster.aspx` et `ProductsForSupplierDetails.aspx`. `SupplierListMaster.aspx` Répertorie les enregistrements « maîtres » (fournisseurs) lors de la `ProductsForSupplierDetails.aspx` affichera les produits pour le fournisseur sélectionné.

Lorsque la création de ces deux nouvelles pages veillez à les associer avec le `Site.master` page maître.


![Ajoutez les Pages de ProductsForSupplierDetails.aspx SupplierListMaster.aspx dans le dossier de filtrage](master-detail-filtering-across-two-pages-cs/_static/image1.png)

**Figure 1**: ajouter la `SupplierListMaster.aspx` et `ProductsForSupplierDetails.aspx` des Pages à la `Filtering` dossier


En outre, lorsque vous ajoutez de nouvelles pages au projet, veillez à mettre à jour le fichier de mappage de site, `Web.sitemap`, en conséquence. Pour ce didacticiel simplement ajouter la `SupplierListMaster.aspx` page pour le plan de site à l’aide du contenu XML suivant en tant qu’enfant de rapports de filtrage `<siteMapNode>` élément :


[!code-xml[Main](master-detail-filtering-across-two-pages-cs/samples/sample1.xml)]

> [!NOTE]
> Vous pouvez aider à automatiser le processus de mise à jour le fichier de mappage de site lors de l’ajout d’ASP.NET de nouvelles pages à l’aide de [K. Scott Allen](http://odetocode.com/Blogs/scott/)de gratuites Visual Studio [Macro de mappage de Site](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx).


## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>Étape 2 : Afficher la liste des fournisseurs dans`SupplierListMaster.aspx`

Avec le `SupplierListMaster.aspx` et `ProductsForSupplierDetails.aspx` pages créées, l’étape suivante consiste à créer le contrôle GridView des fournisseurs dans `SupplierListMaster.aspx`. Ajouter un GridView à la page et la lier à un nouveau ObjectDataSource. Cette ObjectDataSource doit utiliser le `SuppliersBLL` la classe `GetSuppliers()` méthode pour retourner tous les fournisseurs.


[![Sélectionnez la classe SuppliersBLL](master-detail-filtering-across-two-pages-cs/_static/image3.png)](master-detail-filtering-across-two-pages-cs/_static/image2.png)

**Figure 2**: sélectionnez le `SuppliersBLL` classe ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image4.png))


[![Configurer pour utiliser la méthode GetSuppliers() ObjectDataSource](master-detail-filtering-across-two-pages-cs/_static/image6.png)](master-detail-filtering-across-two-pages-cs/_static/image5.png)

**Figure 3**: configurer ObjectDataSource à utiliser le `GetSuppliers()` (méthode) ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image7.png))


Nous devons inclure un lien intitulé d’afficher les produits dans chaque ligne GridView qui, lorsque vous cliquez dessus, dirige l’utilisateur vers `ProductsForSupplierDetails.aspx` en passant de la ligne sélectionnée `SupplierID` valeur via la chaîne de requête. Par exemple, si l’utilisateur clique sur le lien Afficher les produits pour le fournisseur de Tokyo Traders (qui a un `SupplierID` égale à 4), ils doivent être envoyés à `ProductsForSupplierDetails.aspx?SupplierID=4`.

Pour ce faire, ajoutez un [HyperLinkField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx) au GridView, qui ajoute un lien hypertexte pour chaque ligne GridView. Démarrez en cliquant sur le lien Modifier les colonnes à partir de la balise active le contrôle GridView. Ensuite, sélectionnez le HyperLinkField dans la liste en haut à gauche et cliquez sur Ajouter pour inclure la HyperLinkField dans la liste de champs du contrôle GridView.


[![Ajouter un HyperLinkField au GridView](master-detail-filtering-across-two-pages-cs/_static/image9.png)](master-detail-filtering-across-two-pages-cs/_static/image8.png)

**Figure 4**: ajouter un HyperLinkField au GridView ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image10.png))


Le HyperLinkField peut être configuré pour utiliser le même texte ou URL valeurs le lien dans chaque ligne GridView ou pouvez baser ces valeurs sur les valeurs de données liés à chaque ligne particulière. Pour spécifier un mappage statique valeur sur toutes les lignes utilisent le HyperLinkField `Text` ou `NavigateUrl` propriétés. Dans la mesure où nous voulons le texte du lien vers la même pour toutes les lignes, définissez le HyperLinkField `Text` propriété pour afficher les produits.


[![Définir la propriété de texte de la HyperLinkField pour afficher les produits](master-detail-filtering-across-two-pages-cs/_static/image12.png)](master-detail-filtering-across-two-pages-cs/_static/image11.png)

**Figure 5**: définir le HyperLinkField `Text` propriété pour afficher les produits ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image13.png))


Pour définir le texte ou les valeurs d’URL doit être basé sur les données sous-jacentes liées à la ligne de GridView, spécifiez le texte des champs de données ou de valeurs d’URL doivent être extraite dans la `DataTextField` ou `DataNavigateUrlFields` propriétés. `DataTextField` peut uniquement être définie sur un seul champ de données ; `DataNavigateUrlFields`, toutefois, peut être définie à une liste délimitée par des virgules des champs de données. Nous devons fréquemment le texte ou l’URL sur une combinaison de valeur de champ de données de la ligne actuelle et des balises statique de base. Dans ce didacticiel, par exemple, nous voulons l’URL des liens de la HyperLinkField `ProductsForSupplierDetails.aspx?SupplierID=supplierID`, où *`supplierID`* est la ligne de chaque contrôle GridView `SupplierID` valeur. Notez que nous avons besoin statiques et pilotés par les données de valeurs ici : le `ProductsForSupplierDetails.aspx?SupplierID=` partie de l’URL du lien est statique, tandis que le *`supplierID`* partie est orientée données car sa valeur est de chaque ligne propre `SupplierID` valeur.

Pour indiquer une combinaison de valeurs statiques et pilotés par les données, utilisez le `DataTextFormatString` et `DataNavigateUrlFormatString` propriétés. Dans ces propriétés, entrez le balisage statique en fonction des besoins, puis utilisez le marqueur `{0}` où vous souhaitez que la valeur du champ spécifié dans le `DataTextField` ou `DataNavigateUrlFields` propriétés doivent apparaître. Si le `DataNavigateUrlFields` propriété possède plusieurs utilisation spécifiée champs `{0}` où vous souhaitez que la première valeur du champ insérée, `{1}` pour la deuxième valeur de champ et ainsi de suite.

Appliquer cela à notre didacticiel, nous devons définir la `DataNavigateUrlFields` propriété `SupplierID`, puisque c’est le champ de données dont nous avons besoin pour personnaliser sur une ligne par ligne, la valeur et le `DataNavigateUrlFormatString` propriété `ProductsForSupplierDetails.aspx?SupplierID={0}`.


[![Configurer le HyperLinkField pour inclure l’URL du lien approprié en fonction de la SupplierID](master-detail-filtering-across-two-pages-cs/_static/image15.png)](master-detail-filtering-across-two-pages-cs/_static/image14.png)

**Figure 6**: configurer le HyperLinkField pour inclure le bon lien URL en fonction lors de la `SupplierID` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image16.png))


Après avoir ajouté le HyperLinkField, n’hésitez pas à personnaliser et réorganiser les champs du contrôle GridView. Le balisage suivant montre le contrôle GridView, une fois que j’ai apporté des personnalisations au niveau du champ secondaires.


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample2.aspx)]

Prenez un moment pour afficher la `SupplierListMaster.aspx` page via un navigateur. Comme le montre la Figure 7, la page répertorie actuellement tous les fournisseurs, notamment un lien Afficher les produits. En cliquant sur Afficher les produits lien vous mènera à `ProductsForSupplierDetails.aspx`, en passant le long du fournisseur `SupplierID` dans la chaîne de requête.


[![Chaque ligne du fournisseur contient un lien de produits de vue](master-detail-filtering-across-two-pages-cs/_static/image18.png)](master-detail-filtering-across-two-pages-cs/_static/image17.png)

**Figure 7**: chaque ligne du fournisseur contient un lien de produits de vue ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image19.png))


## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>Étape 3 : Répertorier les produits du fournisseur dans`ProductsForSupplierDetails.aspx`

À ce stade le `SupplierListMaster.aspx` page envoie aux utilisateurs de `ProductsForSupplierDetails.aspx`, en passant par le fournisseur sélectionné `SupplierID` dans la chaîne de requête. Étape finale du didacticiel consiste à afficher les produits dans un GridView dans `ProductsForSupplierDetails.aspx` dont `SupplierID` est égale à la `SupplierID` passé dans la chaîne de requête. Pour accomplir ce guide de démarrage en ajoutant un contrôle GridView à la `ProductsForSupplierDetails.aspx` page, à l’aide d’un nouveau contrôle ObjectDataSource nommé `ProductsBySupplierDataSource` qui appelle le `GetProductsBySupplierID(supplierID)` méthode à partir de la `ProductsBLL` classe.


[![Ajouter un nouveau ObjectDataSource nommé ProductsBySupplierDataSource](master-detail-filtering-across-two-pages-cs/_static/image21.png)](master-detail-filtering-across-two-pages-cs/_static/image20.png)

**Figure 8**: ajouter une nouvelle nommée de ObjectDataSource `ProductsBySupplierDataSource` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image22.png))


[![Sélectionnez la classe ProductsBLL](master-detail-filtering-across-two-pages-cs/_static/image24.png)](master-detail-filtering-across-two-pages-cs/_static/image23.png)

**Figure 9**: sélectionnez le `ProductsBLL` classe ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image25.png))


[![Avez ObjectDataSource appeler la méthode GetProductsBySupplierID(supplierID)](master-detail-filtering-across-two-pages-cs/_static/image27.png)](master-detail-filtering-across-two-pages-cs/_static/image26.png)

**Figure 10**: que l’ObjectDataSource appelle le `GetProductsBySupplierID(supplierID)` (méthode) ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image28.png))


L’étape finale de l’Assistant Configurer la Source de données nous demande de fournir la source de la `GetProductsBySupplierID(supplierID)` la méthode *`supplierID`* paramètre. Pour utiliser la valeur de chaîne de requête, définissez la source de paramètre de chaîne de requête et entrez le nom de la valeur de chaîne de requête à utiliser dans la zone de texte QueryStringField (`SupplierID`).


[![Remplir la valeur du paramètre à partir de la valeur de chaîne de requête SupplierID supplierID](master-detail-filtering-across-two-pages-cs/_static/image30.png)](master-detail-filtering-across-two-pages-cs/_static/image29.png)

**Figure 11**: remplir le *`supplierID`* valeur du paramètre à partir de la `SupplierID` valeur Querystring ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image31.png))


C’est tout cela ! La figure 12 illustre le `ProductsForSupplierDetails.aspx` lors de navigation en cliquant sur le lien de Tokyo Traders à partir de la page `SupplierListMaster.aspx`.


[![Les produits fournis par Tokyo Traders sont affichés.](master-detail-filtering-across-two-pages-cs/_static/image33.png)](master-detail-filtering-across-two-pages-cs/_static/image32.png)

**Figure 12**: les produits fournis par Tokyo Traders sont affichés ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image34.png))


## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Affichage des informations de fournisseur dans`ProductsForSupplierDetails.aspx`

Comme le montre la Figure 12, le `ProductsForSupplierDetails.aspx` page répertorie simplement les produits qui sont fournis par le `SupplierID` spécifié dans la chaîne de requête. Une personne envoyées directement à cette page, toutefois, pas sait que la Figure 12 est montrant les produits de Tokyo Traders. Pour résoudre ce problème, nous pouvons afficher des informations sur le fournisseur dans cette page.

Commencez par ajouter un FormView ci-dessus les produits GridView. Créer un contrôle ObjectDataSource nommé `SuppliersDataSource` qui appelle le `SuppliersBLL` la classe `GetSupplierBySupplierID(supplierID)` (méthode).


[![Sélectionnez la classe SuppliersBLL](master-detail-filtering-across-two-pages-cs/_static/image36.png)](master-detail-filtering-across-two-pages-cs/_static/image35.png)

**Figure 13**: sélectionnez le `SuppliersBLL` classe ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image37.png))


[![Avez ObjectDataSource appeler la méthode GetSupplierBySupplierID(supplierID)](master-detail-filtering-across-two-pages-cs/_static/image39.png)](master-detail-filtering-across-two-pages-cs/_static/image38.png)

**Figure 14**: que l’ObjectDataSource appelle le `GetSupplierBySupplierID(supplierID)` (méthode) ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image40.png))


Comme avec la `ProductsBySupplierDataSource`, ont le *`supplierID`* paramètre reçoit la valeur de la `SupplierID` valeur de chaîne de requête.


[![Remplir la valeur du paramètre à partir de la valeur de chaîne de requête SupplierID supplierID](master-detail-filtering-across-two-pages-cs/_static/image42.png)](master-detail-filtering-across-two-pages-cs/_static/image41.png)

**Figure 15**: remplir le *`supplierID`* valeur du paramètre à partir de la `SupplierID` valeur Querystring ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image43.png))


Lorsque vous liez le contrôle FormView à ObjectDataSource en mode Design, Visual Studio crée automatiquement le FormView `ItemTemplate`, `InsertItemTemplate`, et `EditItemTemplate` avec les contrôles Web Label et TextBox pour chacun des champs de données retournés par le ObjectDataSource. Dans la mesure où nous voulons simplement afficher le fournisseur d’informations hésitez pas à supprimer le `InsertItemTemplate` et `EditItemTemplate`. Ensuite, modifiez le modèle ItemTemplate afin qu’il affiche le nom de la société du fournisseur dans un `<h3>` élément et l’adresse, ville, pays et numéro de téléphone sous le nom de société. Vous pouvez également définir manuellement le FormView `DataSourceID` et créer le `ItemTemplate` balisage, comme nous l’avons fait dans la «[affichant les données avec ObjectDataSource le](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)« didacticiel.

Après ces modifications balisage déclaratif de FormView doit ressembler à ce qui suit :


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample3.aspx)]

Figure 16 montre une capture d’écran de la `ProductsForSupplierDetails.aspx` page une fois que les informations de fournisseur détaillées ci-dessus a été incluses.


[![La liste des produits comprend un résumé sur le fournisseur](master-detail-filtering-across-two-pages-cs/_static/image45.png)](master-detail-filtering-across-two-pages-cs/_static/image44.png)

**Figure 16**: la liste des produits inclut un résumé sur le fournisseur ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image46.png))


## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>Application de la dernière touche pour la`ProductsForSupplierDetails.aspx`l’interface utilisateur

Afin d’améliorer l’utilisateur expérience pour ce rapport, il existe quelques ajouts, nous devons aider à apporter à la `ProductsForSupplierDetails.aspx` page. Actuellement la seule façon d’un utilisateur peut accéder à partir de la `ProductsForSupplierDetails.aspx` page sur la liste des fournisseurs consiste à cliquer sur le bouton précédent de leur navigateur. Nous allons ajouter un contrôle de lien hypertexte pour le `ProductsForSupplierDetails.aspx` page qui revient à `SupplierListMaster.aspx`, offrant ainsi un autre moyen pour l’utilisateur revenir à la liste principale.


[![Ajouter un contrôle de lien hypertexte pour rétablir l’utilisateur SupplierListMaster.aspx](master-detail-filtering-across-two-pages-cs/_static/image48.png)](master-detail-filtering-across-two-pages-cs/_static/image47.png)

**Figure 17**: ajouter un contrôle de lien hypertexte pour reprendre l’utilisateur à `SupplierListMaster.aspx` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image49.png))


Si l’utilisateur clique sur le lien Afficher les produits pour un fournisseur qui n’a pas tous les produits, les `ProductsBySupplierDataSource` ObjectDataSource dans `ProductsForSupplierDetails.aspx` ne retourne aucun résultat. Le contrôle GridView lié à ObjectDataSource ne s’afficheront pas tout balisage résultant dans une zone vide dans la page dans le navigateur de l’utilisateur. Pour indiquer clairement à l’utilisateur qu’il n’y a aucun produit associée au fournisseur sélectionné, nous pouvons définir le GridView `EmptyDataText` propriété au message, nous voulons affichées lorsque cette situation se présente. J’ai défini cette propriété sur « Il n’y aucun produit fourni par ce fournisseur »

Par défaut, tous les fournisseurs dans la base de données Northwind fournissent au moins un produit. Toutefois, pour ce didacticiel j’ai modifié manuellement le `Products` afin que le fournisseur Escargots Nouveaux n’est plus associé avec des produits de la table. Figure 18 montre la page de détails pour les Nouveaux Escargots après que cette modification a été apportée.


[![Les utilisateurs sont informés que le fournisseur ne fournit pas tous les produits](master-detail-filtering-across-two-pages-cs/_static/image51.png)](master-detail-filtering-across-two-pages-cs/_static/image50.png)

**Figure 18**: les utilisateurs sont informés que le fournisseur ne fournit pas tous les produits ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image52.png))


## <a name="summary"></a>Récapitulatif

Les rapports maître/détail peuvent afficher les enregistrements maître et détail sur une seule page, dans de nombreux sites Web qu’ils sont séparés et répartis entre deux pages web. Dans ce didacticiel, nous avons vu comment implémenter ce rapport maître/détail en donnant les fournisseurs répertoriés dans un GridView dans la page web « maître » et les produits associés, répertoriés dans la page « Détails ». Chaque ligne du fournisseur dans la page web maître contenait un lien vers la page de détails passé le long de la ligne `SupplierID` valeur. Ces liens spécifiques à la ligne peuvent être facilement ajoutés à l’aide du contrôle GridView HyperLinkField.

Dans la page Détails de récupération de ces produits pour le fournisseur spécifié était obtenue en appelant le `ProductsBLL` la classe `GetProductsBySupplierID(supplierID)` (méthode). Le *`supplierID`* valeur du paramètre a été spécifiée de façon déclarative à l’aide de la chaîne de requête en tant que le paramètre source. Nous avons également étudié comment afficher les détails du fournisseur dans la page de détails à l’aide d’un FormView.

Notre [didacticiel suivant](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) est le dernier sur les rapports maître/détail. Nous examinerons comment afficher une liste de produits dans un GridView où chaque ligne comporte un bouton Select. En cliquant sur le bouton Sélectionner affichera les détails du produit dans un contrôle DetailsView sur la même page.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Hilton Giesenow. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](master-detail-filtering-with-two-dropdownlists-cs.md)
> [Suivant](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
