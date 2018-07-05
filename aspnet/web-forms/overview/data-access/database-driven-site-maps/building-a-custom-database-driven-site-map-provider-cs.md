---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
title: Création d’un fournisseur de plan de Site personnalisé piloté sur la base de données (c#) | Microsoft Docs
author: rick-anderson
description: Le fournisseur de plan de site par défaut dans ASP.NET 2.0 récupère ses données à partir d’un fichier XML statique. Tandis que le fournisseur basé sur XML est adapté à de nombreuses petites et moyennes-siz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 04b7591d-106f-4f05-87e9-d416cb65a8a6
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
msc.type: authoredcontent
ms.openlocfilehash: 1160711911d53dcf889ced1a8422c2dfcf72b240
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368694"
---
<a name="building-a-custom-database-driven-site-map-provider-c"></a>Création d’un fournisseur de plan de Site personnalisé piloté sur la base de données (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_CS.zip) ou [télécharger le PDF](building-a-custom-database-driven-site-map-provider-cs/_static/datatutorial62cs1.pdf)

> Le fournisseur de plan de site par défaut dans ASP.NET 2.0 récupère ses données à partir d’un fichier XML statique. Tandis que le fournisseur basé sur XML est adapté à de nombreux sites Web de petites et moyennes, les applications Web plus importantes nécessitent un plan de site plus dynamique. Dans ce didacticiel, que nous allons créer un fournisseur de plan de site personnalisé qui récupère ses données à partir de la couche de logique métier, qui à son tour récupère les données à partir de la base de données.


## <a name="introduction"></a>Introduction

ASP.NET 2.0 caractéristique de carte de site s permet à un développeur de page définir un plan de site web application s dans un support permanent, comme dans un fichier XML. Une fois défini, les données de plan de site sont accessible par programme via le [ `SiteMap` classe](https://msdn.microsoft.com/library/system.web.sitemap.aspx) dans le [ `System.Web` espace de noms](https://msdn.microsoft.com/library/system.web.aspx) ou via un large éventail de navigation Web contrôles, tels que le Contrôles SiteMapPath, un Menu et TreeView. Le système de mappage de site utilise le [modèle de fournisseur](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) afin que les implémentations de sérialisation de plan de site différent peuvent être créées et connectées à une application web. Le fournisseur de plan de site par défaut qui est fourni avec ASP.NET 2.0 conserve la structure de plan de site dans un fichier XML. Dans le [Pages maîtres et Navigation dans les sites](../introduction/master-pages-and-site-navigation-cs.md) didacticiel, nous avons créé un fichier nommé `Web.sitemap` qui contenues de cette structure et avoir été mise à jour son XML avec chaque nouvelle section du didacticiel.

Le fournisseur de plan de site basé sur XML par défaut fonctionne bien si la structure de s de plan de site est relativement statique, comme pour ces didacticiels. Toutefois, dans de nombreux scénarios, un plan de site plus dynamique est nécessaire. Envisagez le plan de site illustré à la Figure 1, où chaque catégorie et chaque produit s’affichent sous forme de sections dans la structure du site Web s. Avec ce plan de site, visitez la page web correspondant au nœud racine peut répertorier toutes les catégories, tandis que de visiter une page web de catégorie particulière s répertorie ce produit est s catégorie et affichage d’une page web de produit particulier s serait afficher ce produit s Détails.


[![Les catégories et les produits composition la Structure du plan s Site](building-a-custom-database-driven-site-map-provider-cs/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image1.png)

**Figure 1**: les catégories et produits composition la Structure de s plan du Site ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image2.png))


Bien que cette structure en fonction de catégorie et de produit peut être codée en dur dans le `Web.sitemap` fichier, le fichier doit mettre à jour chaque fois qu’une catégorie ou un produit a été ajouté, supprimé ou renommé. Par conséquent, la maintenance de plan de site est considérablement simplifiée si sa structure a été récupérée à partir de la base de données ou, dans l’idéal, à partir de la couche de logique métier de l’architecture d’application s. De cette façon, comme les produits et les catégories ont été ajoutés, renommés ou supprimés, le plan de site est automatiquement mise à jour pour refléter ces modifications.

Étant donné que la sérialisation de carte de site de s ASP.NET 2.0 est construite sur le modèle de fournisseur, nous pouvons créer notre propre fournisseur de plan de site personnalisé qui extrait ses données à partir d’un magasin de données alternatifs, tels que la base de données ou l’architecture. Dans ce didacticiel, nous allons créer un fournisseur personnalisé qui récupère ses données à partir de la couche BLL. Laissez s commencer !

> [!NOTE]
> Le fournisseur de plan de site personnalisé créé dans ce didacticiel est étroitement lié au modèle d’application s architecture et données. Jeff Prosise s [stocker les plans de Site dans SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) et [le fournisseur de plan de Site SQL ve vous attendiez](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx) articles examiner une approche généralisée au stockage des données de plan de site dans SQL Server.


## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>Étape 1 : Créer les Pages Web de Site personnalisé carte fournisseur

Avant de commencer la création d’un fournisseur de plan de site personnalisé, permettent de s tout d’abord ajouter les pages ASP.NET que nous avons besoin pour ce didacticiel. Commencez par ajouter un nouveau dossier nommé `SiteMapProvider`. Ensuite, ajoutez les pages ASP.NET suivantes à ce dossier, en veillant à associer chaque page avec le `Site.master` page maître :

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

Ajoutez également un `CustomProviders` sous-dossier pour le `App_Code` dossier.


![Ajouter les Pages ASP.NET pour les didacticiels liée au fournisseur de plan de Site](building-a-custom-database-driven-site-map-provider-cs/_static/image2.gif)

**Figure 2**: ajoutez les Pages ASP.NET pour le Site mappent didacticiels liée au fournisseur


Dans la mesure où il n'existe qu’un didacticiel pour cette section, nous n’avez pas besoin `Default.aspx` pour répertorier les didacticiels de section s. Au lieu de cela, `Default.aspx` affichera les catégories dans un contrôle GridView. Nous les aborderons cela à l’étape 2.

Ensuite, mettez à jour `Web.sitemap` à inclure une référence à la `Default.aspx` page. Plus précisément, ajoutez le balisage suivant après la mise en cache `<siteMapNode>`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample1.xml)]

Après la mise à jour `Web.sitemap`, prenez un moment pour afficher le site Web de didacticiels via un navigateur. Le menu de gauche inclut désormais un élément pour le didacticiel de fournisseur de carte de site unique.


![Le plan de Site comprend désormais une entrée pour le didacticiel de fournisseur de plan de Site](building-a-custom-database-driven-site-map-provider-cs/_static/image3.gif)

**Figure 3**: le plan de Site comprend désormais une entrée pour le didacticiel de fournisseur de plan de Site


Ce didacticiel s principal objectif est d’illustrer la création d’un fournisseur de plan de site personnalisé et la configuration d’une application web pour utiliser ce fournisseur. En particulier, nous allons créer un fournisseur qui retourne un plan de site qui inclut un nœud racine, ainsi que d’un nœud pour chaque catégorie et chaque produit, comme illustré dans la Figure 1. En général, chaque nœud dans le plan de site peut spécifier une URL. Pour notre plan de site, l’URL racine du nœud s sera `~/SiteMapProvider/Default.aspx`, qui répertorie toutes les catégories dans la base de données. Chaque nœud de catégorie dans le plan de site aura une URL qui pointe vers `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`, qui répertorie tous les produits spécifié *categoryID*. Enfin, chaque nœud de plan de site de produit pointera vers `~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`, qui affiche les détails de produit spécifique s.

Pour démarrer nous devons créer la `Default.aspx`, `ProductsByCategory.aspx`, et `ProductDetails.aspx` pages. Ces pages sont terminées dans les étapes 2, 3 et 4, respectivement. Étant donné que le sujet de ce didacticiel est de fournisseurs de plan de site, et étant donné que les derniers didacticiels ont couvert création ces sortes de plusieurs page maître/détail rapports, nous sera Dépêchez-vous via les étapes 2 à 4. Si vous ne savez plus sur la création de rapports maître/détail qui s’étendent sur plusieurs pages, revenir à la [filtrage de maître/détail sur deux Pages](../masterdetail/master-detail-filtering-across-two-pages-cs.md) didacticiel.

## <a name="step-2-displaying-a-list-of-categories"></a>Étape 2 : Afficher une liste de catégories

Ouvrir le `Default.aspx` page dans le `SiteMapProvider` dossier et faites glisser un GridView à partir de la boîte à outils vers le concepteur, en définissant son `ID` à `Categories`. À partir de la balise active de s GridView, liez-le à une nouvelle ObjectDataSource nommé `CategoriesDataSource` et configurez-la afin qu’il récupère ses données à l’aide de la `CategoriesBLL` classe s `GetCategories` (méthode). Dans la mesure où ce GridView affiche les catégories uniquement et ne fournit pas de fonctionnalités de modification de données, définissez les listes déroulantes dans la mise à jour, insertion et supprimer des onglets à (None).


[![Configurer l’ObjectDataSource pour retourner des catégories à l’aide de la méthode GetCategories](building-a-custom-database-driven-site-map-provider-cs/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image3.png)

**Figure 4**: configurer ObjectDataSource à retourner de catégories à l’aide du `GetCategories` (méthode) ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image4.png))


[![Définir les listes déroulantes dans la mise à jour, insertion et supprimer des onglets à (None)](building-a-custom-database-driven-site-map-provider-cs/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image5.png)

**Figure 5**: la valeur est la liste déroulante répertorie dans la mise à jour, insertion et supprimer des onglets (aucun) ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image6.png))


À l’issue de l’Assistant Configurer la Source de données, Visual Studio ajoute un BoundField pour `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, et `BrochurePath`. Modifier le contrôle GridView pour qu’il contienne uniquement les `CategoryName` et `Description` BoundFields et mettre à jour le `CategoryName` BoundField s `HeaderText` propriété à la catégorie.

Ensuite, ajoutez un HyperLinkField et placez-le donc qu’il s le champ le plus à gauche. Définir le `DataNavigateUrlFields` propriété `CategoryID` et `DataNavigateUrlFormatString` propriété `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`. Définir le `Text` propriété pour afficher les produits.


![Ajouter un HyperLinkField au GridView catégories](building-a-custom-database-driven-site-map-provider-cs/_static/image6.gif)

**Figure 6**: ajouter un HyperLinkField à la `Categories` GridView


Après avoir créé les ObjectDataSource et personnaliser les champs de s GridView, le balisage déclaratif de deux contrôles se présente comme suit :


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample2.aspx)]

La figure 7 illustre `Default.aspx` lorsqu’ils sont affichés via un navigateur. Une catégorie s afficher les produits lien vous amène à `ProductsByCategory.aspx?CategoryID=categoryID`, ce qui nous allons créer à l’étape 3.


[![Chaque catégorie est répertorié avec un lien de produits d’affichage](building-a-custom-database-driven-site-map-provider-cs/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image7.png)

**Figure 7**: chaque catégorie est répertorié avec un lien de produits de vue ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image8.png))


## <a name="step-3-listing-the-selected-category-s-products"></a>Étape 3 : Répertorier les produits de s catégorie sélectionnée

Ouvrez le `ProductsByCategory.aspx` page et ajoutez un GridView, nommez-le `ProductsByCategory`. À partir de sa balise active, lier le contrôle GridView à une nouvelle ObjectDataSource nommé `ProductsByCategoryDataSource`. Configurer l’ObjectDataSource à utiliser le `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` (méthode) et définissez la liste déroulante répertorie à (None) dans les onglets UPDATE, INSERT et DELETE.


[![Utilisez la méthode de GetProductsByCategoryID(categoryID) ProductsBLL classe s](building-a-custom-database-driven-site-map-provider-cs/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image9.png)

**Figure 8**: utilisez le `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` (méthode) ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image10.png))


L’étape finale de l’Assistant Configurer la Source de données vous invite à entrer pour une source de paramètre pour *categoryID*. Dans la mesure où ces informations sont passées via le champ de chaîne de requête `CategoryID`, sélectionnez la chaîne de requête dans la liste déroulante et entrez CategoryID dans la zone de texte QueryStringField, comme illustré à la Figure 9. Cliquez sur Terminer pour terminer l’Assistant.


[![Utilisez le champ de chaîne de requête de CategoryID pour la paramètre categoryID](building-a-custom-database-driven-site-map-provider-cs/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image11.png)

**Figure 9**: utilisez le `CategoryID` Querystring Field pour le *categoryID* paramètre ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image12.png))


À l’issue de l’Assistant, Visual Studio ajoutera BoundFields correspondante et un CheckBoxField au GridView pour les champs de données de produit. Supprimer tout sauf la `ProductName`, `UnitPrice`, et `SupplierName` BoundFields. Personnaliser ces trois BoundFields `HeaderText` propriétés pour lire des produits, les prix et les fournisseurs, respectivement. Format du `UnitPrice` BoundField sous forme de devise.

Ensuite, ajoutez un HyperLinkField et déplacez-le vers la position la plus à gauche. Définir son `Text` propriété pour afficher des détails, son `DataNavigateUrlFields` propriété `ProductID`et son `DataNavigateUrlFormatString` propriété `~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`.


![Ajouter un HyperLinkField de détails de vue qui pointe vers ProductDetails.aspx](building-a-custom-database-driven-site-map-provider-cs/_static/image10.gif)

**Figure 10**: ajouter une vue Détails HyperLinkField qui pointe vers `ProductDetails.aspx`


Après avoir apporté ces personnalisations, le balisage déclaratif s GridView et ObjectDataSource doit ressembler à ce qui suit :


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample3.aspx)]

Retourner à l’affichage `Default.aspx` via un navigateur et d’un clic sur les produits de la vue de lier le correspondant aux boissons. Ceci vous dirigera vers `ProductsByCategory.aspx?CategoryID=1`, affichant les noms, les prix et les fournisseurs des produits dans la base de données Northwind qui appartiennent à la catégorie des boissons (voir Figure 11). N’hésitez pas à améliorer cette page pour inclure un lien pour retourner les utilisateurs à la page de liste de catégorie (`Default.aspx`) et un contrôle DetailsView ou FormView qui affiche le nom de la catégorie sélectionnée s et la description.


[![Les noms des boissons, les prix et les fournisseurs sont affichés.](building-a-custom-database-driven-site-map-provider-cs/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image13.png)

**Figure 11**: les noms de boissons, les prix et les fournisseurs sont affichés ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image14.png))


## <a name="step-4-showing-a-product-s-details"></a>Étape 4 : Montrant un s Product Details

La dernière page, `ProductDetails.aspx`, affiche les détails des produits sélectionnés. Ouvrez `ProductDetails.aspx` et faites glisser un contrôle DetailsView à partir de la boîte à outils vers le concepteur. Définir le contrôle DetailsView s `ID` propriété `ProductInfo` et d’effacer les sa `Height` et `Width` les valeurs de propriété. À partir de sa balise active, lier le contrôle DetailsView à une nouvelle ObjectDataSource nommé `ProductDataSource`, configuration ObjectDataSource afin d’extraire ses données à partir de la `ProductsBLL` classe s `GetProductByProductID(productID)` (méthode). Comme avec les pages web créées dans les étapes 2 et 3, définir les listes déroulantes dans la mise à jour, insertion et supprimer des onglets à (None).


[![Configurer pour utiliser la méthode GetProductByProductID(productID) ObjectDataSource](building-a-custom-database-driven-site-map-provider-cs/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.png)

**Figure 12**: configurer ObjectDataSource à utiliser le `GetProductByProductID(productID)` (méthode) ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image16.png))


La dernière étape de l’Assistant Configurer la Source de données vous invite à entrer pour la source de la *productID* paramètre. Dans la mesure où ces données sont affichées via le champ de chaîne de requête `ProductID`, la valeur de la liste déroulante chaîne de requête et de la zone de texte QueryStringField ProductID. Enfin, cliquez sur le bouton Terminer pour terminer l’Assistant.


[![Configurer le paramètre pour extraire sa valeur à partir du champ de chaîne de requête ProductID productID](building-a-custom-database-driven-site-map-provider-cs/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image17.png)

**Figure 13**: configurer le *productID* paramètre pour extraire sa valeur à partir de la `ProductID` Querystring Field ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image18.png))


À l’issue de l’Assistant Configurer la Source de données, Visual Studio créera BoundFields correspondante et un CheckBoxField dans le contrôle DetailsView pour les champs de données de produit. Supprimer le `ProductID`, `SupplierID`, et `CategoryID` BoundFields et configurer les champs restants, comme vous le souhaitez. Après un certain nombre de configurations esthétiques, mon balisage déclaratif s DetailsView et ObjectDataSource ressemblait à ce qui suit :


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample4.aspx)]

Pour tester cette page, revenez à `Default.aspx` , puis cliquez sur Afficher les produits pour la catégorie des boissons. Dans la liste de produits de boissons, cliquez sur le lien Afficher les détails pour Chai thé. Ceci vous dirigera vers `ProductDetails.aspx?ProductID=1`, qui montre un s Chai thé détails (voir Figure 14).


[![Thé Chai s fournisseur, catégorie, prix et autres informations s’affiche.](building-a-custom-database-driven-site-map-provider-cs/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image19.png)

**Figure 14**: thé Chai s fournisseur, catégorie, prix et autres informations s’affiche ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image20.png))


## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>Étape 5 : Présentation du fonctionnement interne d’un fournisseur de plan de Site

Le plan de site est représenté dans la mémoire du serveur s web comme une collection de `SiteMapNode` instances qui forment une hiérarchie. Il doit y avoir exactement une seule racine, tous les nœuds non racine doivent avoir exactement un nœud parent et tous les nœuds peuvent avoir un nombre arbitraire d’enfants. Chaque `SiteMapNode` objet représente une section dans la structure du site Web s ; ces sections comportent généralement une page web correspondante. Par conséquent, le [ `SiteMapNode` classe](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) possède des propriétés comme `Title`, `Url`, et `Description`, qui fournissent des informations de la section la `SiteMapNode` représente. Il existe également un `Key` propriété qui identifie de façon unique chaque `SiteMapNode` dans la hiérarchie, ainsi que les propriétés utilisées pour établir cette hiérarchie `ChildNodes`, `ParentNode`, `NextSibling`, `PreviousSibling`, et ainsi de suite.

Figure 15 illustre la structure du plan général du site à partir de la Figure 1, mais avec les détails d’implémentation décrit en détail.


[![Chaque noeud SiteMapNode a des propriétés telles que titre, Url, clé et ainsi de suite](building-a-custom-database-driven-site-map-provider-cs/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.gif)

**Figure 15**: chaque `SiteMapNode` a des propriétés telles que `Title`, `Url`, `Key`, et ainsi de suite ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image17.gif))


Le plan de site est accessible via la [ `SiteMap` classe](https://msdn.microsoft.com/library/system.web.sitemap.aspx) dans le [ `System.Web` espace de noms](https://msdn.microsoft.com/library/system.web.aspx). Cette classe s `RootNode` propriété retourne la racine du site carte s `SiteMapNode` instance ; `CurrentNode` retourne le `SiteMapNode` dont `Url` propriété correspond à l’URL de la page actuellement demandée. Cette classe est utilisée en interne par les contrôles ASP.NET 2.0 s navigation Web.

Lorsque le `SiteMap` des propriétés de la classe s sont accessibles, il doit sérialiser la structure de plan de site à partir d’un support persistant dans la mémoire. Toutefois, la logique de sérialisation de carte de site n'est pas codé en dur dans le `SiteMap` classe. Au lieu de cela, lors de l’exécution le `SiteMap` classe détermine quel plan de site *fournisseur* à utiliser pour la sérialisation. Par défaut, le [ `XmlSiteMapProvider` classe](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx) est utilisé, qui lit la structure du plan s site à partir d’un fichier XML correctement mis en forme. Toutefois, avec un peu de travail, nous pouvons créer notre propre fournisseur de plan de site personnalisé.

Tous les fournisseurs de plan de site doivent être dérivés de la [ `SiteMapProvider` classe](https://msdn.microsoft.com/library/system.web.sitemapprovider.aspx), qui inclut les méthodes essentielles et les propriétés nécessaires pour le site mapper des fournisseurs, mais omet de nombreux détails de l’implémentation. Une deuxième classe, [ `StaticSiteMapProvider` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.aspx), étend la `SiteMapProvider` classe et contient une implémentation plus robuste de la fonctionnalité requise. En interne, le `StaticSiteMapProvider` stocke la `SiteMapNode` mappent les instances du site dans un `Hashtable` et fournit des méthodes telles que `AddNode(child, parent)`, `RemoveNode(siteMapNode),` et `Clear()` qui ajoutent et suppriment `SiteMapNode` s pour le texte interne `Hashtable`. `XmlSiteMapProvider` est dérivé de `StaticSiteMapProvider`.

Lorsque la création d’un fournisseur de plan de site personnalisé qui étend `StaticSiteMapProvider`, il existe deux méthodes abstraites qui doivent être remplacées : [ `BuildSiteMap` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.buildsitemap.aspx) et [ `GetRootNodeCore` ](https://msdn.microsoft.com/library/system.web.sitemapprovider.getrootnodecore.aspx). `BuildSiteMap`, comme son nom l’indique, est responsable du chargement de la structure de plan de site à partir d’un stockage persistant et il construction en mémoire. `GetRootNodeCore` Renvoie le nœud racine dans le plan du site.

Avant une web application peut utiliser un fournisseur de plan de site qu'il doit être inscrit dans la configuration d’application s. Par défaut, le `XmlSiteMapProvider` classe est inscrite à l’aide du nom `AspNetXmlSiteMapProvider`. Pour inscrire des fournisseurs de plan de site supplémentaires, ajoutez le balisage suivant à `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample5.xml)]

Le *nom* valeur assigne un nom explicite pour le fournisseur lors de la *type* Spécifie le nom de type qualifié complet du fournisseur de plan de site. Nous allons explorer des valeurs concrètes pour la *nom* et *type* valeurs à l’étape 7, après nous ve créé notre fournisseur de plan de site personnalisé.

La classe de fournisseur de plan de site est instanciée de la première fois, il est accessible à partir de la `SiteMap` classe et reste en mémoire pour la durée de vie de l’application web. Étant donné qu’une seule instance du fournisseur de plan de site qui peut-être être appelée à partir de multiples, simultanées aux visiteurs, il est impératif que les méthodes du fournisseur s être *thread-safe*.

Pour des raisons de performances et évolutivité, il s important que nous mettons en cache le site en mémoire mapper la structure et retourner cette mise en cache structure plutôt que de recréer chaque fois que le `BuildSiteMap` méthode est appelée. `BuildSiteMap` peut être appelée plusieurs fois par demande de page par utilisateur, selon les contrôles de navigation en cours d’utilisation sur la page et la profondeur de la structure de plan de site. Dans tous les cas, si nous ne pas mettre en cache la structure de plan de site dans `BuildSiteMap` puis chaque fois qu’elle est appelée que nous devons récupérer de nouveau les informations de produit et la catégorie de l’architecture (ce qui entraînerait une requête à la base de données). Comme expliqué dans les mise en cache les didacticiels précédents, données mises en cache peuvent devenir obsolètes. Pour remédier à ce problème, nous pouvons utiliser le temps - ou les expirations de dans le cache basé sur la dépendance SQL.

> [!NOTE]
> Un fournisseur de plan de site peut également remplacer le [ `Initialize` méthode](https://msdn.microsoft.com/library/system.web.sitemapprovider.initialize.aspx). `Initialize` est appelé lorsque le fournisseur de plan de site est instancié et est transmis à tous les attributs personnalisés affectés au fournisseur dans `Web.config` dans le `<add>` élément tel que : `<add name="name" type="type" customAttribute="value" />`. Il est utile si vous souhaitez permettre à un développeur de page spécifier les paramètres liée au fournisseur de carte de site différents sans avoir à modifier le code du fournisseur s. Par exemple, si nous étions lire les données de catégorie et produits directement à partir de la base de données, par opposition à l’architecture, nous d probablement souhaitez permettent au développeur de page de spécifier la chaîne de connexion de base de données via `Web.config` au lieu d’utiliser un incrustées valeur dans le code du fournisseur s. Le fournisseur de plan de site personnalisé que nous allons créer à l’étape 6 ne remplace pas cette `Initialize` (méthode). Pour obtenir un exemple d’utilisation de la `Initialize` (méthode), reportez-vous à [Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [stocker les plans de Site dans SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) article.


## <a name="step-6-creating-the-custom-site-map-provider"></a>Étape 6 : Créer le fournisseur de plan de Site personnalisé

Pour créer un fournisseur de plan de site personnalisé qui génère le plan de site à partir des catégories et des produits dans la base de données Northwind, nous devons créer une classe qui étend `StaticSiteMapProvider`. À l’étape 1, j’ai demandé vous permet d’ajouter un `CustomProviders` dossier dans le `App_Code` dossier - ajouter une nouvelle classe dans ce dossier nommé `NorthwindSiteMapProvider`. Ajoutez le code suivant à la classe `NorthwindSiteMapProvider` :


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample6.cs)]

Permettent de commencer par Explorer cette classe s s `BuildSiteMap` (méthode), qui commence par un [ `lock` instruction](https://msdn.microsoft.com/library/c5kehkcz.aspx). La `lock` instruction autorise uniquement un seul thread à la fois à entrer, ainsi accès à son code de sérialisation et empêche l’exécution pas à pas sur eux s moins d’une vingtaine de deux threads simultanés.

Niveau de la classe `SiteMapNode` variable `root` est utilisé pour mettre en cache de la structure de plan de site. Lorsque le plan de site est créée pour la première fois, ou pour la première fois, une fois que les données sous-jacentes a été modifiées, `root` sera `null` et la structure de plan de site sera construite. Le nœud racine de la carte s site est affecté à `root` pendant la construction processus afin que la prochaine fois que cette méthode est appelé, `root` ne sera pas `null`. Par conséquent, tant que `root` n’est pas `null` la structure de plan de site s’affichera à l’appelant sans avoir à le recréer.

Si la racine est `null`, la structure de plan de site est créée à partir des informations produit et la catégorie. Le mappage de site est créé en créant le `SiteMapNode` instances et former ensuite la hiérarchie via des appels à la `StaticSiteMapProvider` classe s `AddNode` (méthode). `AddNode` effectue la comptabilité interne, en stockant l’assorties `SiteMapNode` instances dans un `Hashtable`. Avant de commencer la construction de la hiérarchie, nous commençons en appelant le `Clear` (méthode), ce qui efface les éléments à partir de la liste interne `Hashtable`. Ensuite, le `ProductsBLL` classe s `GetProducts` méthode et résultant `ProductsDataTable` sont stockés dans des variables locales.

La construction d’un mappage s site commence par créer le nœud racine et en l’assignant à `root`. La surcharge de la [ `SiteMapNode` constructeur de s](https://msdn.microsoft.com/library/system.web.sitemapnode.sitemapnode.aspx) utilisé ici et tout au long de ce `BuildSiteMap` est passé les informations suivantes :

- Une référence au fournisseur de plan de site (`this`).
- Le `SiteMapNode` s `Key`. Cela nécessaire la valeur doit être unique pour chaque `SiteMapNode`.
- Le `SiteMapNode` s `Url`. `Url` est facultatif, mais s’il est fourni, chacun `SiteMapNode` s `Url` valeur doit être unique.
- Le `SiteMapNode` s `Title`, ce qui est nécessaire.

Le `AddNode(root)` appel de méthode ajoute la `SiteMapNode` `root` au plan du site comme racine. Ensuite, chaque `ProductRow` dans le `ProductsDataTable` est énuméré. S’il existe déjà un `SiteMapNode` pour la catégorie de produit s actuelle, il est référencé. Sinon, un nouveau `SiteMapNode` pour la catégorie est créée et ajoutée en tant qu’enfant de le `SiteMapNode``root` via la `AddNode(categoryNode, root)` appel de méthode. Après la catégorie appropriée `SiteMapNode` nœud a été trouvé ou créé, un `SiteMapNode` est créé pour le produit actuel et ajouté en tant qu’enfant de la catégorie `SiteMapNode` via `AddNode(productNode, categoryNode)`. Notez que la catégorie `SiteMapNode` s `Url` valeur de propriété est `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID` tandis que le produit `SiteMapNode` s `Url` est affectée à `~/SiteMapNode/ProductDetails.aspx?ProductID=productID`.

> [!NOTE]
> Possédant une base de données `NULL` valeur pour leurs `CategoryID` sont regroupés sous une catégorie `SiteMapNode` dont `Title` propriété est définie sur None et dont la propriété `Url` propriété est définie sur une chaîne vide. J’ai décidé de définir `Url` sur une chaîne vide depuis le `ProductBLL` classe s `GetProductsByCategory(categoryID)` méthode ne dispose pas actuellement la fonctionnalité permettant de retourner uniquement les produits avec une `NULL` `CategoryID` valeur. En outre, je souhaitais démontrer comment les contrôles de navigation restituent un `SiteMapNode` auquel il manque une valeur pour son `Url` propriété. Je vous encourage à étendre ce didacticiel afin qu’aucune `SiteMapNode` s `Url` propriété pointe vers `ProductsByCategory.aspx`, encore affiche uniquement les produits avec `NULL` `CategoryID` valeurs.


Après avoir construit le plan du site, un objet arbitraire est ajouté au cache de données à l’aide d’une dépendance de cache SQL sur le `Categories` et `Products` tables via une `AggregateCacheDependency` objet. Nous avons explorés à l’aide de dépendances de cache SQL dans le didacticiel précédent, *à l’aide des dépendances de Cache SQL*. Le fournisseur de plan de site personnalisé, cependant, utilise une surcharge du cache de données s `Insert` méthode que nous avons ve encore à Explorer. Cette surcharge accepte comme paramètre d’entrée finale un délégué qui est appelé lorsque l’objet est supprimé du cache. Plus précisément, nous passons dans un nouveau [ `CacheItemRemovedCallback` déléguer](https://msdn.microsoft.com/library/system.web.caching.cacheitemremovedcallback.aspx) qui pointe vers le `OnSiteMapChanged` méthode définies plus bas dans la `NorthwindSiteMapProvider` classe.

> [!NOTE]
> La représentation en mémoire du plan de site est mis en cache via la variable de niveau classe `root`. Étant donné qu’une seule instance de la classe de fournisseur de plan de site personnalisé, et étant donné que cette instance est partagée entre tous les threads dans l’application web, cette variable de la classe sert un cache. Le `BuildSiteMap` méthode utilise également le cache de données, mais uniquement comme un moyen de recevoir une notification lorsque les données dans la base de données sous-jacent le `Categories` ou `Products` modifications des tables. Notez que la valeur placée dans le cache de données est simplement la date et heure actuelles. Les données de plan de site réel sont *pas* placés dans le cache de données.


Le `BuildSiteMap` méthode se termine en renvoyant le nœud racine du plan de site.

Les méthodes restantes sont assez simples. `GetRootNodeCore` est chargée de retourner le nœud racine. Dans la mesure où `BuildSiteMap` retourne la racine, `GetRootNodeCore` renvoie simplement `BuildSiteMap` s retourner de valeur. Le `OnSiteMapChanged` méthode jeux `root` à `null` lorsque l’élément de cache est supprimé. Avec racine retrouvent `null`, la prochaine fois `BuildSiteMap` est appelé, la structure de plan de site va être régénérée. Enfin, le `CachedDate` propriété retourne la valeur de date et d’heure stockée dans le cache de données, si cette valeur existe. Cette propriété peut être utilisée par un développeur de pages pour déterminer quand les données de plan de site a été dernière mise en cache.

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>Étape 7 : Inscrire le`NorthwindSiteMapProvider`

Dans l’ordre de notre application web à utiliser le `NorthwindSiteMapProvider` fournisseur de plan de site créé à l’étape 6, nous avons besoin pour l’inscrire dans le `<siteMap>` section de `Web.config`. Plus précisément, ajoutez le balisage suivant dans le `<system.web>` élément dans `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample7.xml)]

Ce balisage effectue deux opérations : tout d’abord, cela indique que l’intégrée `AspNetXmlSiteMapProvider` est le fournisseur de plan de site par défaut ; Deuxièmement, elle enregistre le fournisseur de plan de site personnalisé créé à l’étape 6 avec le nom convivial Northwind.

> [!NOTE]
> Pour les fournisseurs de plan de site situés dans l’application s `App_Code` dossier, la valeur de la `type` attribut est simplement le nom de classe. Vous pouvez également, le fournisseur de plan de site personnalisé pourrait ont été créé dans un projet de bibliothèque de classes distinct avec l’assembly compilé est placé dans le s d’application web `/Bin` directory. Dans ce cas, le `type` valeur d’attribut serait *Namespace*. *ClassName*, *AssemblyName* .


Après la mise à jour `Web.config`, prenez un moment pour afficher n’importe quelle page dans les didacticiels dans un navigateur. Notez que l’interface de navigation de gauche affiche toujours les sections et didacticiels défini dans `Web.sitemap`. Il s’agit, car nous avons laissé `AspNetXmlSiteMapProvider` en tant que le fournisseur par défaut. Pour créer un élément d’interface utilisateur navigation qui utilise le `NorthwindSiteMapProvider`, nous allons devoir spécifier explicitement que le fournisseur de plan de site de Northwind doit être utilisé. Nous verrons comment procéder à l’étape 8.

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>Étape 8 : Affichage des informations de plan de Site à l’aide du fournisseur de plan de Site personnalisé

Avec le site personnalisé fournisseur de plan créés et enregistrés dans `Web.config`, nous vous êtes prêt à ajouter des contrôles de navigation à la `Default.aspx`, `ProductsByCategory.aspx`, et `ProductDetails.aspx` des pages dans le `SiteMapProvider` dossier. Commencez par ouvrir le `Default.aspx` page et faites glisser un `SiteMapPath` à partir de la boîte à outils vers le concepteur. Le contrôle SiteMapPath se trouve dans la section de Navigation de la boîte à outils.


[![Ajouter un SiteMapPath à Default.aspx](building-a-custom-database-driven-site-map-provider-cs/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image18.gif)

**Figure 16**: ajouter un SiteMapPath à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image20.gif))


Le contrôle SiteMapPath affiche un fil d’Ariane, indiquant l’emplacement de s de page actuel dans le plan du site. Nous avons ajouté un SiteMapPath vers le haut de la page maître dans le *Pages maîtres et Navigation du Site* didacticiel.

Prenez un moment pour afficher cette page via un navigateur. Le contrôle SiteMapPath ajouté à la Figure 16 utilise le fournisseur de plan de site par défaut, extraction de ses données à partir de `Web.sitemap`. Par conséquent, la barre de navigation affiche accueil &gt; personnaliser le plan du Site, tout comme la barre de navigation dans le coin supérieur droit.


[![Le plan de site utilise le fournisseur de plan de Site par défaut](building-a-custom-database-driven-site-map-provider-cs/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.gif)

**Figure 17**: le plan de site utilise le fournisseur de plan de Site par défaut ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image23.gif))


Pour que le contrôle SiteMapPath ajouté à la Figure 16 à utiliser le fournisseur de plan de site personnalisé nous avons créé à l’étape 6, définissez son [ `SiteMapProvider` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx) à Northwind, le nom que nous avons attribué à la `NorthwindSiteMapProvider` dans `Web.config`. Malheureusement, le Concepteur continue d’utiliser le fournisseur de plan de site par défaut, mais si vous visitez la page via un navigateur après avoir apporté cette modification de propriété vous verrez que la barre de navigation utilise désormais le fournisseur de plan de site personnalisé.


[![La barre de navigation utilise désormais le NorthwindSiteMapProvider de fournisseur de carte de Site personnalisé](building-a-custom-database-driven-site-map-provider-cs/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image24.gif)

**Figure 18**: la barre de navigation utilise désormais le fournisseur de plan de Site personnalisé `NorthwindSiteMapProvider` ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image26.gif))


Le contrôle SiteMapPath affiche une interface utilisateur plus fonctionnelle dans le `ProductsByCategory.aspx` et `ProductDetails.aspx` pages. Ajouter un SiteMapPath à ces pages, en définissant le `SiteMapProvider` propriété dans les deux à Northwind. À partir de `Default.aspx` cliquez sur le lien Afficher les produits correspondant aux boissons, puis sur le lien Afficher les détails pour Chai thé. Comme le montre la Figure 19, la barre de navigation inclut la section de la carte de site en cours (Tea Martin) et ses ancêtres : Beverages et toutes les catégories.


[![La barre de navigation utilise désormais le NorthwindSiteMapProvider de fournisseur de carte de Site personnalisé](building-a-custom-database-driven-site-map-provider-cs/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.png)

**Figure 19**: la barre de navigation utilise désormais le fournisseur de plan de Site personnalisé `NorthwindSiteMapProvider` ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image22.png))


Autres éléments d’interface utilisateur navigation peuvent être utilisés en plus le contrôle SiteMapPath, tels que les contrôles Menu et TreeView. Le `Default.aspx`, `ProductsByCategory.aspx`, et `ProductDetails.aspx` dans le téléchargement de ce didacticiel, par exemple, les pages incluent toutes les contrôles de Menu (voir la Figure 20). Consultez [s examinant ASP.NET 2.0 les fonctionnalités de Navigation de Site](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) et le [à l’aide de contrôles de Navigation du Site](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx) section de la [guides de démarrage rapide ASP.NET 2.0](https://quickstarts.asp.net/QuickStartv20/aspnet/) pour examiner plus approfondie la contrôles de navigation et de système de mappage de site dans ASP.NET 2.0.


[![Le contrôle de Menu répertorie chacune des catégories et des produits](building-a-custom-database-driven-site-map-provider-cs/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image28.gif)

**Figure 20**: le Menu contrôle répertorie chaque des catégories et des produits ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image30.gif))


Comme mentionné précédemment dans ce didacticiel, la structure de plan de site est accessible par programme via le `SiteMap` classe. Le code suivant retourne la racine `SiteMapNode` du fournisseur par défaut :


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample8.cs)]

Dans la mesure où le `AspNetXmlSiteMapProvider` est le fournisseur par défaut pour notre application, le code ci-dessus renvoie le nœud racine défini dans `Web.sitemap`. Pour faire référence à un fournisseur de plan de site autre que la valeur par défaut, utilisez le `SiteMap` classe s [ `Providers` propriété](https://msdn.microsoft.com/library/system.web.sitemap.providers.aspx) comme suit :


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample9.cs)]

Où *nom* est le nom du fournisseur de plan de site personnalisé (Northwind, pour notre application web).

Pour accéder à un membre spécifique à un fournisseur de plan de site, utilisez `SiteMap.Providers["name"]` pour récupérer l’instance du fournisseur et puis effectuer un cast vers le type approprié. Par exemple, pour afficher le `NorthwindSiteMapProvider` s `CachedDate` propriété dans une page ASP.NET, utilisez le code suivant :


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample10.cs)]

> [!NOTE]
> Veillez à tester la fonctionnalité de dépendance de cache SQL. Après vous rendant sur le `Default.aspx`, `ProductsByCategory.aspx`, et `ProductDetails.aspx` pages, accédez à un des didacticiels dans l’édition, insertion et suppression de section et modifier le nom d’une catégorie ou un produit. Puis revenez à une des pages dans le `SiteMapProvider` dossier. En supposant que suffisamment de temps est écoulé pour le mécanisme d’interrogation de noter la modification apportée à la base de données sous-jacente, le plan de site doit être mis à jour pour afficher le nouveau produit ou le nom de catégorie.


## <a name="summary"></a>Récapitulatif

ASP.NET 2.0 inclut des fonctions de mappage de site s un `SiteMap` (classe), un nombre de fonctionnalités de navigation Web de contrôles, et un fournisseur de plan de site par défaut qui attend les informations de plan de site rendues persistantes dans un fichier XML. Pour pouvoir utiliser les informations de plan de site à partir d’une autre source, par exemple à partir d’une base de données, l’architecture d’application s ou un service Web à distance, que nous devons créer un fournisseur de plan de site personnalisé. Cela implique la création d’une classe qui dérive directement ou indirectement, à partir de la `SiteMapProvider` classe.

Dans ce didacticiel, nous avons vu comment créer un fournisseur de plan de site personnalisé qui selon le plan du site les informations de produit et la catégorie éliminées de l’architecture d’application. Notre fournisseur étendu la `StaticSiteMapProvider` de classe et de création impliquée un `BuildSiteMap` méthode qui extrait les données, construits de la hiérarchie de plan de site et mis en cache de la structure qui en résulte dans une variable de niveau classe. Nous avons utilisé une dépendance de cache SQL avec une fonction de rappel pour invalider la mise en cache lorsque structure sous-jacent `Categories` ou `Products` de données est modifiée.

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Stockage des plans de Site dans SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) et [le fournisseur de plan de Site SQL ve vous attendiez](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [Modèle de fournisseur s un coup de œil à ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [Le Kit de ressources de fournisseur](https://msdn.microsoft.com/asp.net/aa336558.aspx)
- [Examen d’ASP.NET 2.0 fonctionnalités de Navigation du Site s](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Dave Gardner, Zack Jones, Teresa Murphy et Bernadette Leigh. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](building-a-custom-database-driven-site-map-provider-vb.md)
