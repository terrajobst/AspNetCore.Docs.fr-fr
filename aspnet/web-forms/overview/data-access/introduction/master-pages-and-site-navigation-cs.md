---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
title: Pages maîtres et Navigation de Site (c#) | Microsoft Docs
author: rick-anderson
description: L’une des caractéristiques communes des sites Web conviviales sont qu’ils disposent d’un schéma de mise en page et de navigation de page cohérente, à l’échelle du site. Ce didacticiel aborde la façon y...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 5aee8202-a4e3-4aa9-8a95-cd5d156cea4c
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
msc.type: authoredcontent
ms.openlocfilehash: d9ae2fb5a79817053a260e7d0f85992a011f471b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835066"
---
<a name="master-pages-and-site-navigation-c"></a>Pages maîtres et Navigation de Site (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_3_CS.exe) ou [télécharger le PDF](master-pages-and-site-navigation-cs/_static/datatutorial03cs1.pdf)

> L’une des caractéristiques communes des sites Web conviviales sont qu’ils disposent d’un schéma de mise en page et de navigation de page cohérente, à l’échelle du site. Ce didacticiel examine comment vous pouvez créer une apparence cohérente sur toutes les pages qui peuvent être facilement mis à jour.


## <a name="introduction"></a>Introduction

L’une des caractéristiques communes des sites Web conviviales sont qu’ils disposent d’un schéma de mise en page et de navigation de page cohérente, à l’échelle du site. ASP.NET 2.0 introduit deux nouvelles fonctionnalités qui simplifient considérablement le mettant en oeuvre les deux une page de l’échelle du site mise en page et navigation : pages maîtres et navigation sur le site. Pages maîtres permettent aux développeurs de créer un modèle à l’échelle du site avec les zones modifiables désignés. Ce modèle peut ensuite être appliqué aux pages ASP.NET dans le site. Ces pages ASP.NET doivent uniquement fournir du contenu pour la page maître du spécifié zones modifiables toutes les autres marques dans la page maître sont identique sur toutes les pages ASP.NET qui utilisent la page maître. Ce modèle permet aux développeurs de définir et de centraliser une disposition de la page de l’échelle du site, ce qui le rend plus facile de créer une apparence cohérente sur toutes les pages qui peuvent être facilement mis à jour.

Le [système de navigation de site](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) fournit un mécanisme pour les développeurs de pages définir un plan de site et une API pour ce plan de site à être interrogées par programmation. Les nouveaux contrôles de navigation Web que le Menu, TreeView et SiteMapPath facilitent restituent tout ou partie du plan de site dans un élément d’interface utilisateur navigation courantes. Nous allons utiliser le fournisseur de navigation de site par défaut, ce qui signifie que notre plan de site sera défini dans un fichier au format XML.

Pour illustrer ces concepts et faciliter l’utilisation de notre site Web de didacticiels, nous allons passer cette leçon définition d’une mise en page de l’échelle du site, la mise en œuvre un plan de site et l’ajout de l’interface utilisateur de navigation. À la fin de ce didacticiel, nous aurons une conception soignées du site Web pour la création de nos pages web didacticiel.


[![Le résultat final de ce didacticiel](master-pages-and-site-navigation-cs/_static/image2.png)](master-pages-and-site-navigation-cs/_static/image1.png)

**Figure 1**: le résultat de ce didacticiel complet ([cliquez pour afficher l’image en taille réelle](master-pages-and-site-navigation-cs/_static/image3.png))


## <a name="step-1-creating-the-master-page"></a>Étape 1 : Création de la Page maître

La première étape consiste à créer la page maître du site. Actuellement, notre site Web est constitué par le DataSet typé (`Northwind.xsd`, dans le `App_Code` dossier), les classes de la couche BLL (`ProductsBLL.cs`, `CategoriesBLL.cs`, et ainsi de suite, tous dans le `App_Code` dossier), la base de données (`NORTHWND.MDF`, dans le `App_Data` dossier), le fichier de configuration (`Web.config`) et un fichier de feuille de style CSS (`Styles.css`). J’ai éliminé les pages et les fichiers de démonstration à l’aide de la couche DAL et la couche BLL à partir des deux premiers didacticiels dans la mesure où nous sera réexamen des ces exemples plus en détail dans les didacticiels futures.


![Les fichiers dans notre projet.](master-pages-and-site-navigation-cs/_static/image4.png)

**Figure 2**: les fichiers dans notre projet.


Pour créer une page maître, avec le bouton droit sur le nom du projet dans l’Explorateur de solutions et choisissez Ajouter un nouvel élément. Sélectionnez le type de Page maître à partir de la liste des modèles, puis nommez-le `Site.master`.


[![Ajouter une nouvelle Page maître au site Web](master-pages-and-site-navigation-cs/_static/image6.png)](master-pages-and-site-navigation-cs/_static/image5.png)

**Figure 3**: ajouter une nouvelle Page maître au site Web ([cliquez pour afficher l’image en taille réelle](master-pages-and-site-navigation-cs/_static/image7.png))


Définir la disposition de la page de l’échelle du site ici dans la page maître. Vous pouvez utiliser le mode Design et ajouter quelque vous avez besoin des contrôles de disposition ou Web, ou vous pouvez ajouter manuellement le balisage manuellement dans la vue de Source. Dans ma page maître, j’utilise [des feuilles de style en cascade](http://www.w3schools.com/css/default.asp) de positionnement et de styles avec les paramètres de CSS définis dans le fichier externe `Style.css`. Bien que vous ne pouvez pas indiquer à partir du balisage indiqué ci-dessous, les règles CSS sont définies telles que le volet de navigation `<div>`du contenu est positionné de façon absolue afin qu’il apparaît à gauche et a une largeur fixe de 200 pixels.

Site.master


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample1.aspx)]

Une page maître définit la disposition de page statiques et les régions qui peuvent être modifiées par les pages ASP.NET qui utilisent la page maître. Ces zones de contenu modifiables sont indiquées par le contrôle ContentPlaceHolder, qui peut être consulté dans le contenu `<div>`. Notre page maître a un ContentPlaceHolder unique (`MainContent`), mais la page maître peut avoir plusieurs ContentPlaceHolders.

Avec le balisage ci-dessus, basculer vers la vue de conception montre mise en page de la page maître. Toutes les pages ASP.NET qui utilisent cette page maître aura cette mise en page uniforme, avec la possibilité de spécifier le balisage pour le `MainContent` région.


[![La Page maître, lorsqu’ils sont affichés via la vue de conception](master-pages-and-site-navigation-cs/_static/image9.png)](master-pages-and-site-navigation-cs/_static/image8.png)

**Figure 4**: la Page maître, lorsque affichés via le mode création ([cliquez pour afficher l’image en taille réelle](master-pages-and-site-navigation-cs/_static/image10.png))


## <a name="step-2-adding-a-homepage-to-the-website"></a>Étape 2 : Ajout d’une page d’accueil pour le site Web

Avec la page maître définie, nous sommes prêts à ajouter les pages ASP.NET pour le site Web. Commençons par ajouter `Default.aspx`, page d’accueil de notre site Web. Avec le bouton droit sur le nom du projet dans l’Explorateur de solutions et choisissez Ajouter un nouvel élément. Choisissez l’option de formulaire Web à partir de la liste des modèles et le nom du fichier `Default.aspx`. En outre, la case « Sélectionner la page maître ».


[![Ajoutez un nouveau formulaire Web, la vérification de la case à cocher Sélectionner la page maître](master-pages-and-site-navigation-cs/_static/image12.png)](master-pages-and-site-navigation-cs/_static/image11.png)

**Figure 5**: ajouter un nouveau formulaire Web, la vérification de la case à cocher Sélectionner la page maître ([cliquez pour afficher l’image en taille réelle](master-pages-and-site-navigation-cs/_static/image13.png))


Après avoir cliqué sur le bouton OK, nous allons invités à choisir quelle page maître cette nouvelle page ASP.NET doit utiliser. Bien que vous pouvez avoir plusieurs pages maîtres dans votre projet, nous n'avons qu’un seul.


[![Choisissez la Page maître que cette Page ASP.NET doit utiliser.](master-pages-and-site-navigation-cs/_static/image15.png)](master-pages-and-site-navigation-cs/_static/image14.png)

**Figure 6**: cliquez sur cette Page ASP.NET doit utilisent la Page maître ([cliquez pour afficher l’image en taille réelle](master-pages-and-site-navigation-cs/_static/image16.png))


Après avoir sélectionné la page maître, les nouvelles pages ASP.NET contient le balisage suivant :

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample2.aspx)]

Dans le `@Page` la directive il est une référence au fichier de page maître utilisé (`MasterPageFile="~/Site.master"`), et l’ASP.NET du balisage de page contient un contrôle de contenu pour chacun des contrôles ContentPlaceHolder définis dans la page maître, avec le contrôle `ContentPlaceHolderID` le contenu du mappage le contrôle à un ContentPlaceHolder spécifique. Le contrôle de contenu est l’endroit où vous placez le balisage vous souhaitez voir apparaître dans ContentPlaceHolder correspondant. Définir le `@Page` de directive `Title` attribut à l’accueil et ajouter du contenu accueillant au contrôle de contenu :

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample3.aspx)]

Le `Title` d’attribut dans le `@Page` directive nous permet de définir le titre de la page à partir de la page ASP.NET, même si le `<title>` élément est défini dans la page maître. Nous pouvons également définir le titre par programmation, à l’aide de `Page.Title`. Notez également que les références de la page maître pour les feuilles de style (tels que `Style.css`) sont automatiquement mis à jour afin qu’ils fonctionnent dans n’importe quelle page ASP.NET, quel que soit le répertoire la page ASP.NET dans par rapport à la page maître.

Basculer vers la vue de conception que nous pouvons voir l’aspect de notre page dans un navigateur. Notez que, dans la conception, afficher, de la page ASP.NET que seules les régions modifiables de contenu sont modifiables, le balisage non ContentPlaceHolder défini dans la page maître est grisée.


[![Le mode de création de la Page ASP.NET montre les deux régions modifiables et Non modifiables](master-pages-and-site-navigation-cs/_static/image18.png)](master-pages-and-site-navigation-cs/_static/image17.png)

**Figure 7**: la vue de conception pour l’ASP.NET Page montre à la fois le modifiable et Non-éditable régions ([cliquez pour afficher l’image en taille réelle](master-pages-and-site-navigation-cs/_static/image19.png))


Lorsque le `Default.aspx` pages visitées par un navigateur, le moteur ASP.NET fusionne automatiquement le contenu de page maître de la page et ASP. NET de contenu et restitue le contenu fusionné dans le code HTML final qui est envoyé au navigateur demandeur. Lorsque le contenu de la page maître est mis à jour, toutes les pages ASP.NET qui utilisent cette page maître auront leur contenu refusionnée avec la nouvelle page maître contenue la prochaine fois qu’ils sont demandés. En bref, le modèle de page maître permet une seule page modèle de disposition pour être défini (la page maître) dont les modifications sont immédiatement répercutées dans l’ensemble du site.

## <a name="adding-additional-aspnet-pages-to-the-website"></a>Ajout de Pages ASP.NET supplémentaires sur le site Web

Prenons un moment pour ajouter des stubs de page ASP.NET supplémentaires au site qui contiendra finalement les démonstrations de création de rapports différents. Il y aura plus de 35 démonstrations au total, par conséquent, au lieu de créer toutes les pages de stub, nous allons juste créer les premiers. Dans la mesure où il y aura également de nombreuses catégories de démonstrations, les démonstrations ajouter un dossier pour les catégories pour mieux gérer. Pour l’instant, ajoutez les trois dossiers suivants :

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

Enfin, ajoutez de nouveaux fichiers comme indiqué dans l’Explorateur de solutions dans la Figure 8. Lorsque vous ajoutez chaque fichier, pensez à vérifier la case à cocher « Sélectionner la page maître ».


![Ajoutez les fichiers suivants](master-pages-and-site-navigation-cs/_static/image20.png)

**Figure 8**: ajouter les fichiers suivants


## <a name="step-2-creating-a-site-map"></a>Étape 2 : Création d’un plan de Site

Un des défis de la gestion d’un site Web composé de plus de quelques pages fournit un moyen simple pour les visiteurs de naviguer dans le site. Pour commencer, la structure de navigation du site doit être définie. Ensuite, cette structure doit être traduite en éléments d’interface utilisateur navigable, tels que des menus ou barres de navigation. Enfin, ce processus doit être maintenu et mis à jour nouvelles pages sont ajoutés au site et à ceux qui existent déjà supprimé. Avant ASP.NET 2.0, les développeurs étaient sur leurs propres pour la création de structure de navigation du site, gérer et traduire en éléments d’interface utilisateur navigable. Avec ASP.NET 2.0, cependant, les développeurs peuvent utiliser très flexible généré dans le système de navigation de site.

Le système de navigation de site ASP.NET 2.0 fournit un moyen pour un développeur pour définir un plan de site et ensuite accéder à ces informations via une API par programme. ASP.NET est livré avec un fournisseur de plan de site qui attend des données de plan de site à stocker dans un fichier XML mis en forme d’une façon particulière. Mais, étant donné que le système de navigation de site est basé le [modèle de fournisseur](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) il peut être étendu pour prendre en charge d’autres méthodes pour sérialiser les informations de plan de site. Article de Jeff Prosise, [le Site carte fournisseur vous avez déjà été en attente pour SQL](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx) montre comment créer un fournisseur de plan de site qui stocke le plan du site dans une base de données SQL Server ; une autre option consiste à créer [un fournisseur de plan de site basé sur la structure de système de fichiers](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx).

Pour ce didacticiel, toutefois, utilisons le fournisseur de plan de site par défaut qui est fourni avec ASP.NET 2.0. Pour créer le plan de site, simplement avec le bouton droit sur le nom du projet dans l’Explorateur de solutions, choisissez Ajouter un nouvel élément, choisissez l’option de plan de Site. Conservez le nom `Web.sitemap` et cliquez sur le bouton Ajouter.


[![Ajouter un plan de Site à votre projet](master-pages-and-site-navigation-cs/_static/image22.png)](master-pages-and-site-navigation-cs/_static/image21.png)

**Figure 9**: ajouter un plan de Site à votre projet ([cliquez pour afficher l’image en taille réelle](master-pages-and-site-navigation-cs/_static/image23.png))


Le fichier de mappage de site est un fichier XML. Notez que Visual Studio fournit IntelliSense pour la structure de plan de site. Le fichier de mappage de site doit avoir le `<siteMap>` nœud en tant que son nœud racine, qui doit contenir précisément l’un `<siteMapNode>` élément enfant. Ce premier `<siteMapNode>` élément peut contenir un nombre arbitraire de descendant `<siteMapNode>` éléments.

Définir le plan du site pour imiter la structure de système de fichiers. Autrement dit, ajoutez un `<siteMapNode>` pour chacun des trois dossiers, les enfants `<siteMapNode>` éléments pour chacune des pages ASP.NET dans ces dossiers, comme suit :

Web.sitemap


[!code-xml[Main](master-pages-and-site-navigation-cs/samples/sample4.xml)]

Le plan de site définit la structure de navigation du site Web, qui est une hiérarchie qui décrit les différentes sections du site. Chaque `<siteMapNode>` élément `Web.sitemap` représente une section de la structure de navigation du site.


[![Le plan de Site représente une Structure de navigation hiérarchique](master-pages-and-site-navigation-cs/_static/image25.png)](master-pages-and-site-navigation-cs/_static/image24.png)

**Figure 10**: le plan de Site représente une Structure de navigation hiérarchique ([cliquez pour afficher l’image en taille réelle](master-pages-and-site-navigation-cs/_static/image26.png))


ASP.NET expose la structure de la carte de site par le biais du .NET Framework [SiteMap classe](https://msdn.microsoft.com/library/system.web.sitemap.aspx). Cette classe a un `CurrentNode` propriété, qui retourne des informations sur la section que l’utilisateur visite actuellement ; le `RootNode` propriété retourne la racine du plan de site (accueil, dans notre plan de site). À la fois le `CurrentNode` et `RootNode` propriétés retour [SiteMapNode](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) instances qui ont des propriétés telles que `ParentNode`, `ChildNodes`, `NextSibling`, `PreviousSibling`, et ainsi de suite, qui permettent le plan du site hiérarchie à traiter.

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>Étape 3 : Afficher un Menu basé sur le plan du Site

Accès aux données dans ASP.NET 2.0 peut être accompli par programme, comme dans ASP.NET 1.x, ou de façon déclarative, via le nouveau [contrôles de source de données](https://msdn.microsoft.com/library/ms227679.aspx). Il existe plusieurs contrôles de source de données intégrés tels que le contrôle SqlDataSource, pour accéder aux données de base de données relationnelle, le contrôle ObjectDataSource, pour accéder aux données à partir des classes et d’autres. Vous pouvez même créer vos propres [contrôles de source de données personnalisées](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/dnvs05/html/DataSourceCon1.asp).

Les contrôles de source de données servent en tant que proxy entre votre page ASP.NET et les données sous-jacentes. Pour afficher les données récupérées d’un contrôle de source de données, nous en général, ajoutez un autre contrôle Web à la page et liez-le au contrôle de source de données. Pour lier un contrôle Web à un contrôle de source de données, il suffit de définir le contrôle Web `DataSourceID` propriété la valeur du contrôle de source de données `ID` propriété.

Pour aider à travailler avec les données de la carte de site, ASP.NET inclut le contrôle SiteMapDataSource, ce qui nous permet de lier un contrôle Web sur le plan du site de notre site Web. Deux contrôles TreeView et Menu sont couramment utilisés pour fournir une interface utilisateur de navigation. Pour lier les données de plan de site à une de ces deux contrôles, ajoutez simplement un SiteMapDataSource pour la page avec un contrôle TreeView ou Menu contrôle dont `DataSourceID` propriété est définie en conséquence. Par exemple, nous pourrions ajouter un contrôle de Menu à la page maître à l’aide de la balise suivante :


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample5.aspx)]

Pour obtenir un meilleur niveau de contrôle sur le code HTML émis, nous pouvons lier le contrôle SiteMapDataSource pour le contrôle Repeater, comme suit :


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample6.aspx)]

Le contrôle SiteMapDataSource retourne le niveau de hiérarchie celui de carte de site à la fois, en commençant par le nœud de plan de site racine (accueil, dans notre plan de site), puis le niveau suivant (rapports de base, le filtrage des rapports et personnaliser la mise en forme) et ainsi de suite. Lors de la liaison SiteMapDataSource pour un répéteur, elle énumère le premier niveau retourné et instancie les `ItemTemplate` pour chaque `SiteMapNode` instance dans ce premier niveau. Pour accéder à une propriété particulière de la `SiteMapNode`, nous pouvons utiliser `Eval(propertyName)`, c'est-à-dire comment nous obtenons chacun `SiteMapNode`de `Url` et `Title` propriétés pour le contrôle de lien hypertexte.

L’exemple de répéteur ci-dessus s’affiche le balisage suivant :


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample7.html)]

Ces nœuds de plan de site (rapports de base, le filtrage des rapports et personnaliser la mise en forme) constituent le *deuxième* au niveau du plan du site en cours de rendu, pas la première. Il s’agit, car le SiteMapDataSource `ShowStartingNode` propriété est définie sur False, à l’origine SiteMapDataSource à contourner le nœud de plan de site racine et à commencer à la place en retournant le deuxième niveau dans la hiérarchie de plan de site.

Pour afficher les enfants pour les rapports de base, filtrage des rapports et personnaliser la mise en forme `SiteMapNode` s, nous pouvons ajouter un autre Repeater pour le contrôle Repeater initial `ItemTemplate`. Cet deuxième Repeater sera lié à la `SiteMapNode` l’instance `ChildNodes` propriété, comme suit :


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample8.aspx)]

Ces deux répéteurs entraînent le balisage suivant (certaines balises a été supprimé par souci de concision) :


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample9.html)]

À l’aide de CSS définit le style choisi à partir de [Rachel Andrew](http://www.rachelandrew.co.uk/)du livre [le CSS anthologie : conseils essentiels 101, astuces, &amp; Hacks](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance), la `<ul>` et `<li>` éléments sont présentent telles que le balisage génère la sortie de visual suivante :


![Un Menu composé de deux répéteurs et du code CSS](master-pages-and-site-navigation-cs/_static/image27.png)

**Figure 11**: un Menu composé de deux répéteurs et à certaines CSS


Ce menu est dans la page maître et lié à la carte de site définie dans `Web.sitemap`, ce qui signifie que toute modification apportée à la carte de site est immédiatement répercutée sur toutes les pages qui utilisent le `Site.master` page maître.

## <a name="disabling-viewstate"></a>La désactivation de ViewState

Tous les contrôles ASP.NET peuvent conservent également leur état dans le [l’état d’affichage](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/), ce qui est sérialisé comme un champ de formulaire masqué dans le code HTML restitué. État d’affichage est utilisé par les contrôles de se souvenir de leur état modifiée par programmation entre les postbacks, telles que les données liées à un contrôle Web de données. Alors que l’état d’affichage d’informations pour être mémorisées entre les postbacks le permet, il augmente la taille de la balise qui doit être envoyée au client et peut entraîner un encombrement page graves si ce n’est pas étroitement surveillés. Contrôles Web de données en particulier le contrôle GridView sont particulièrement connus pour l’ajout des dizaines de kilo-octets supplémentaires de balisage à une page. Cette augmentation peut être négligeable pour les utilisateurs à large bande ou intranet, état d’affichage peut ajouter plusieurs secondes à l’aller-retour pour les utilisateurs d’accès à distance.

Pour voir l’impact de l’état d’affichage, visitez une page dans un navigateur, puis ensuite afficher la source envoyée par la page web (dans Internet Explorer, accédez au menu Affichage et choisissez l’option de Source). Vous pouvez également activer [le traçage des pages](https://msdn.microsoft.com/library/sfbfw58f.aspx) pour voir l’allocation d’état de vue utilisée par chacun des contrôles sur la page. Afficher les informations d’état sont sérialisées dans un champ de formulaire masqué nommé `__VIEWSTATE`, situé dans un `<div>` élément situé juste après l’ouverture `<form>` balise. État d’affichage est conservé uniquement lorsqu’il existe un formulaire Web utilisé ; Si votre page ASP.NET n’inclut pas un `<form runat="server">` dans sa syntaxe déclarative, il n’y aura un `__VIEWSTATE` champ de formulaire masqué dans le balisage rendu.

Le `__VIEWSTATE` champ de formulaire généré par la page maître ajoute environ 1 800 octets pour le balisage généré de la page. Cet encombrement supplémentaire est principalement dû au contrôle Repeater, telles que le contenu du contrôle SiteMapDataSource est conservée à l’état d’affichage. Pendant un 1 800 octets supplémentaires peut ne pas sembler une grande partie enthousiasmantes, lors de l’utilisation d’un GridView avec de nombreux champs et enregistrements, l’état d’affichage peut augmenter facilement par un facteur de 10 ou plus.

État d’affichage peut être désactivée au niveau de la page ou le contrôle en définissant le `EnableViewState` propriété `false`, ce qui en réduisant la taille du balisage restitué. Depuis l’état d’affichage pour un Web contrôle conserve les données liées aux données de contrôle Web entre des publications (postback) de données, lors de la désactivation de l’état d’affichage pour les données de contrôle Web les données doivent être liées à chaque publication (postback). Dans la version d’ASP.NET 1.x est descendue en cette responsabilité sur les épaules du développeur de page ; avec ASP.NET 2.0, toutefois, les contrôles Web de données seront de nouveau lié à leur contrôle de source de données sur chaque publication (postback) si nécessaire.

Pour réduire l’état d’affichage page Nous allons définie le contrôle Repeater `EnableViewState` propriété `false`. Cela est possible via la fenêtre Propriétés dans le concepteur ou de façon déclarative dans la vue de Source. Après avoir apporté cette modification balisage déclaratif de répéteur doit ressembler à :


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample10.aspx)]

Une fois cette modification, la page rendu affichage taille de l’état est réduit à simple 52 octets, une économie de 97 % dans l’affichage taille de l’état ! Dans les didacticiels de cette série, nous allons désactiver l’état d’affichage des données des contrôles Web par défaut afin de réduire la taille du balisage restitué. Dans la plupart des exemples de la `EnableViewState` propriété sera définie `false` et fait sans mention. La seule fois affichage état sera abordé se trouve dans les scénarios où il doit être activé dans l’ordre pour les données Web contrôle à fournir ses fonctionnalités attendues.

## <a name="step-4-adding-breadcrumb-navigation"></a>Étape 4 : Ajout d’arborescence de Navigation

Pour terminer la page maître, nous allons ajouter un élément de l’interface utilisateur de navigation de fil d’Ariane à chaque page. La barre de navigation affiche rapidement les utilisateurs leur position actuelle dans la hiérarchie du site. Ajout d’un fil d’Ariane dans ASP.NET 2.0 vous suffit simplement d’ajouter un contrôle SiteMapPath à la page ; Aucun code n’est nécessaire.

Pour notre site, ajouter ce contrôle à l’en-tête `<div>`:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample11.aspx)]

La barre de navigation montre la page actuelle de l’utilisateur visite dans la hiérarchie de plan de site, ainsi que ce site « ancêtres, » du nœud de la carte jusqu'à la racine (accueil, dans notre plan de site).


![La barre de navigation affiche la Page actuelle et ses ancêtres sur le Site de mappent de la hiérarchie](master-pages-and-site-navigation-cs/_static/image28.png)

**Figure 12**: la barre de navigation affiche la Page actuelle et ses ancêtres sur le Site de mappent de la hiérarchie


## <a name="step-5-adding-the-default-page-for-each-section"></a>Étape 5 : Ajout de la Page par défaut pour chaque Section

Les didacticiels dans notre site sont décomposent en différentes catégories de rapports de base, de filtrage, de mise en forme personnalisée, et ainsi de suite avec un dossier pour chaque catégorie et les didacticiels correspondants que les pages ASP.NET dans ce dossier. En outre, chaque dossier contient un `Default.aspx` page. Pour cette page par défaut, s’affichent tous les didacticiels pour la section en cours. Autrement dit, pour le `Default.aspx` dans le `BasicReporting` dossier auraient des liens vers `SimpleDisplay.aspx`, `DeclarativeParams.aspx`, et `ProgrammaticParams.aspx`. Ici, là encore, nous pouvons utiliser le `SiteMap` classe et un contrôle Web pour afficher ces informations selon le plan du site de données définis dans `Web.sitemap`.

Nous allons afficher une liste non triée à l’aide d’un répéteur, mais cette fois que nous affichons le titre et la description des didacticiels. Étant donné que le balisage et le code pour accomplir cela va doivent être répétées pour chaque `Default.aspx` page, nous pouvons encapsuler cette logique de l’interface utilisateur dans un [contrôle utilisateur](https://msdn.microsoft.com/library/y6wb1a0e.aspx). Créez un dossier dans le site Web appelé `UserControls` et ajoutez à cela un nouvel élément de type contrôle utilisateur Web nommé `SectionLevelTutorialListing.ascx`et ajoutez le balisage suivant :


[![Ajouter un nouveau contrôle utilisateur de Web dans le dossier UserControls](master-pages-and-site-navigation-cs/_static/image30.png)](master-pages-and-site-navigation-cs/_static/image29.png)

**Figure 13**: ajouter un nouveau contrôle utilisateur de Web pour le `UserControls` dossier ([cliquez pour afficher l’image en taille réelle](master-pages-and-site-navigation-cs/_static/image31.png))


SectionLevelTutorialListing.ascx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample12.aspx)]

SectionLevelTutorialListing.ascx.cs


[!code-csharp[Main](master-pages-and-site-navigation-cs/samples/sample13.cs)]

Dans l’exemple précédent de répéteur nous lié le `SiteMap` données pour le contrôle Repeater déclarative ; le `SectionLevelTutorialListing` contrôle utilisateur, toutefois, le fait par programmation. Dans le `Page_Load` Gestionnaire d’événements, une vérification est effectuée pour vous assurer que cette page s URL mappe à un nœud dans le plan du site. Si ce contrôle utilisateur est utilisé dans une page qui n’a pas un correspondant `<siteMapNode>` entrée, `SiteMap.CurrentNode` retournera `null` et aucune donnée ne sera liée à la répétition. En supposant que nous avons un `CurrentNode`, nous lions son `ChildNodes` collection pour le contrôle Repeater. Étant donné que notre plan de site est configuré tel que le `Default.aspx` page dans chaque section est le nœud parent de tous les didacticiels dans cette section, ce code affiche des liens vers et les descriptions de tous les didacticiels de la section, comme illustré dans la capture d’écran ci-dessous.

Une fois ce Repeater a été créé, ouvrez le `Default.aspx` pages dans chacun des dossiers, accédez à la vue de conception et faites simplement glisser le contrôle utilisateur à partir de l’Explorateur de solutions vers l’aire de conception où vous souhaitez voir apparaître la liste de didacticiels.


[![Le contrôle de l’utilisateur a été ajouté à Default.aspx](master-pages-and-site-navigation-cs/_static/image33.png)](master-pages-and-site-navigation-cs/_static/image32.png)

**Figure 14**: le contrôle utilisateur a été ajouté à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](master-pages-and-site-navigation-cs/_static/image34.png))


[![Les didacticiels de Reporting base sont répertoriés.](master-pages-and-site-navigation-cs/_static/image36.png)](master-pages-and-site-navigation-cs/_static/image35.png)

**Figure 15**: la base des didacticiels Reporting sont répertoriés ([cliquez pour afficher l’image en taille réelle](master-pages-and-site-navigation-cs/_static/image37.png))


## <a name="summary"></a>Récapitulatif

Avec le plan de site défini et la page maître terminée, nous obtenons un schéma de mise en page et de navigation de page cohérente pour nos didacticiels relatifs aux données. Quel que soit le nombre de pages nous ajoutons à notre site, la mise à jour les informations de navigation de site ou de mise en page de l’échelle du site est un processus simple et rapide en raison de ces informations est centralisées. Plus précisément, les informations de mise en page sont définies dans la page maître `Site.master` et le site mapper dans `Web.sitemap`. Nous n’avions pas besoin d’écrire *tout* de code pour atteindre ce mécanisme de mise en page et de navigation de page de l’échelle du site, et nous conservons complète WYSIWYG prise en charge de concepteur dans Visual Studio.

À l’issue de la couche d’accès aux données et la couche de logique métier et avoir une navigation de page cohérente mise en page et de site définie, nous sommes prêts à commencer à Explorer les modèles courants de création de rapports. Dans le [suivant](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) trois didacticiels nous examinerons les tâches de création de rapports base affichant les données récupérées à partir de la couche BLL dans le contrôle GridView, DetailsView, et le contrôle FormView.

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Vue d’ensemble des Pages maître ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Pages maître dans ASP.NET 2.0](http://odetocode.com/Articles/419.aspx)
- [Modèles de conception 2.0 ASP.NET](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [Vue d’ensemble de Navigation de Site ASP.NET](https://msdn.microsoft.com/library/e468hxky.aspx)
- [Examen d’ASP.NET 2.0 de Navigation de Site](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Fonctionnalités de Navigation de Site 2.0 ASP.NET](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [État d’affichage ASP.NET description](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/viewstate.asp)
- [Comment : activer le traçage d’une Page ASP.NET](https://msdn.microsoft.com/library/94c55d08%28VS.80%29.aspx)
- [Contrôles utilisateur ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Liz Shulok, Dennis Patterson et Hilton Giesenow. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](creating-a-business-logic-layer-cs.md)
> [Suivant](creating-a-data-access-layer-vb.md)
