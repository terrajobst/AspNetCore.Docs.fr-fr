---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
title: Pagination des données de rapport dans un contrôle DataList ou Repeater (VB) | Microsoft Docs
author: rick-anderson
description: Lors du Repeater ni DataList offre la pagination automatique ou prise en charge de tri, ce didacticiel montre comment ajouter la prise en charge la pagination pour le contrôle DataList ou Repeater...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: bbd6b7f7-b98a-48b4-93f3-341d6a4f53c0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: da3c851a8752d4ef5c210a6d8fe552412ecbc9b0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385390"
---
<a name="paging-report-data-in-a-datalist-or-repeater-control-vb"></a>Pagination des données de rapport dans un contrôle DataList ou Repeater (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_VB.exe) ou [télécharger le PDF](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial44vb1.pdf)

> Bien que Repeater ni DataList offre automatique la pagination ou trier la prise en charge, ce didacticiel montre comment ajouter la prise en charge la pagination à la DataList ou Repeater, ce qui permet d’interfaces d’affichage pour la pagination et les données beaucoup plus flexible.


## <a name="introduction"></a>Introduction

Pagination et tri sont deux fonctionnalités très courantes lors de l’affichage des données dans une application en ligne. Par exemple, lors de la recherche pour la documentation d’ASP.NET à une librairie en ligne, il peut y avoir des centaines d’ouvrages, mais le rapport affichant les résultats de recherche répertorie les correspondances seulement dix par page. En outre, les résultats peuvent être triées par titre, prix, nombre de pages, nom de l’auteur et ainsi de suite. Comme expliqué dans la [la pagination et tri des données de rapport](../paging-and-sorting/paging-and-sorting-report-data-vb.md) didacticiel, les contrôles GridView, DetailsView et FormView fournissent tous prise en charge de la pagination intégrée peut être activée au battement d’une case à cocher. Le contrôle GridView inclut également la prise en charge de tri.

Malheureusement, Repeater ni DataList offrent la pagination automatique ou le tri de prise en charge. Dans ce didacticiel, nous allons examiner comment ajouter la prise en charge la pagination pour le contrôle DataList ou Repeater. Manuellement, nous devons créer l’interface de pagination, afficher la page d’enregistrements appropriée et n’oubliez pas de la page qui est visitée entre les postbacks. Bien que cela prend plus de temps et de code qu’avec le GridView, DetailsView ou FormView, les contrôles DataList et Repeater autorisent pour la pagination et les données beaucoup plus flexible interfaces d’affichage.

> [!NOTE]
> Ce didacticiel se concentre exclusivement sur la pagination. Dans le didacticiel suivant, nous allons porter notre attention à l’ajout de fonctionnalités de tri.


## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Étape 1 : Ajout de la pagination et tri des Pages Web didacticiels

Avant de commencer ce didacticiel, laissez s tout d’abord prendre un moment pour ajouter les pages ASP.NET que nous aurons besoin pour ce didacticiel et la suivante. Commencez par créer un nouveau dossier dans le projet nommé `PagingSortingDataListRepeater`. Ensuite, ajoutez les pages ASP.NET cinq suivantes à ce dossier, présentant toutes les configurer pour utiliser la page maître `Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`


![Créez un dossier PagingSortingDataListRepeater et ajouter les Pages ASP.NET didacticiel](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Figure 1**: créer un `PagingSortingDataListRepeater` dossier et ajouter les Pages ASP.NET didacticiel


Ensuite, ouvrez le `Default.aspx` page et faites glisser le `SectionLevelTutorialListing.ascx` contrôle utilisateur à partir de la `UserControls` dossier sur l’aire de conception. Ce contrôle utilisateur, que nous avons créée dans le [Pages maîtres et Navigation du Site](../introduction/master-pages-and-site-navigation-vb.md) didacticiel, énumère le plan du site et affiche ces didacticiels dans la section en cours dans une liste à puces.


[![Ajouter le contrôle utilisateur de SectionLevelTutorialListing.ascx à Default.aspx](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)

**Figure 2**: ajouter la `SectionLevelTutorialListing.ascx` contrôle utilisateur à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image4.png))


Pour afficher la pagination et tri des didacticiels, que nous allons créer la liste à puces, nous avons besoin de les ajouter au plan du site. Ouvrez le `Web.sitemap` fichier, puis ajoutez le balisage suivant après la modification et suppression avec le balisage de nœud de plan de site DataList :


[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample1.xml)]


![Mettre à jour le plan du Site pour inclure les nouvelles Pages ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)

**Figure 3**: mettre à jour le plan du Site pour inclure les nouvelles Pages ASP.NET


## <a name="a-review-of-paging"></a>Une revue de pagination

Dans les didacticiels précédents, nous avons vu comment parcourir les données dans les contrôles GridView, DetailsView et FormView. Ces trois contrôles offrent une forme simple de pagination appelée *la pagination par défaut* qui peut être implémentée en activant simplement l’option Activer la pagination dans la balise active de contrôle s. Avec la pagination par défaut, chaque fois qu’une page de données est demandée sur la première page visitez quand l’utilisateur navigue vers une autre page de données de GridView, DetailsView, ou contrôle FormView ré-demande *tous les* des données à partir de la ObjectDataSource. Il puis captures out le jeu d’enregistrements à afficher l’index de la page demandée et du nombre d’enregistrements à afficher par page. Nous avons abordé en détail dans la pagination par défaut le [la pagination et tri des données de rapport](../paging-and-sorting/paging-and-sorting-report-data-vb.md) didacticiel.

Dans la mesure où la pagination par défaut demande nouveau tous les enregistrements pour chaque page, il n’est pas pratique lors de la pagination via suffisamment grandes quantités de données. Par exemple, imaginez la pagination de 50 000 enregistrements avec une taille de page de 10. Chaque fois que l’utilisateur se déplace vers une nouvelle page, tous les enregistrements de 50 000 doivent être récupérées à partir de la base de données, même si seuls dix d'entre eux est affichés.

*La pagination personnalisée* résout les problèmes de performances de la pagination par défaut par s’emparer uniquement le sous-ensemble précis d’enregistrements à afficher sur la page demandée. Lorsque vous implémentez la pagination personnalisée, nous devons écrire la requête SQL qui retournera efficacement simplement le jeu d’enregistrements approprié. Nous avons vu comment créer une telle requête à l’aide de SQL Server 2005 s nouvelle [ `ROW_NUMBER()` mot clé](http://www.4guysfromrolla.com/webtech/010406-1.shtml) dans le [efficacement la pagination par le biais d’importants volumes de données](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) didacticiel.

Pour implémenter la pagination par défaut dans les contrôles DataList ou Repeater, nous pouvons utiliser le [ `PagedDataSource` classe](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.aspx) comme un wrapper autour du `ProductsDataTable` dont le contenu est en cours de pagination. Le `PagedDataSource` classe a un `DataSource` propriété qui peut être affectée à n’importe quel objet énumérable et [ `PageSize` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx) et [ `CurrentPageIndex` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx) propriétés qui indiquent le nombre d’enregistrements à Afficher par page et l’index de page actuel. Une fois que ces propriétés ont été définies, le `PagedDataSource` peut être utilisé comme source de données de n’importe quel contrôle Web de données. Le `PagedDataSource`, lorsqu’elle est énumérée, retournent seulement un sous-ensemble approprié d’enregistrements de son interne `DataSource` selon le `PageSize` et `CurrentPageIndex` propriétés. Figure 4 illustre les fonctionnalités de la `PagedDataSource` classe.


![Le PagedDataSource encapsule un objet énumérable avec une Interface paginable](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image6.png)

**Figure 4**: le `PagedDataSource` encapsule un objet énumérable avec une Interface paginable


Le `PagedDataSource` objet peut être créé et configuré directement à partir de la couche de logique métier et lié à un contrôle DataList ou le Repeater via un ObjectDataSource, ou peut être créé et configuré directement dans la classe code-behind de pages ASP.NET. Si cette dernière approche est utilisée, nous devons renoncer à l’aide de l’ObjectDataSource et lie les données paginées au DataList ou Repeater par programmation.

Le `PagedDataSource` objet a également des propriétés pour prendre en charge la pagination personnalisée. Toutefois, nous pouvons contourner à l’aide un `PagedDataSource` pour la pagination personnalisée, car nous avons déjà des méthodes BLL le `ProductsBLL` classe conçue pour la pagination personnalisée qui retournent les enregistrements précis à afficher.

Dans ce didacticiel, nous examinerons l’implémentation de la pagination par défaut dans un contrôle DataList en ajoutant une nouvelle méthode à la `ProductsBLL` classe renvoyant configurée de manière appropriée `PagedDataSource` objet. Dans le didacticiel suivant, nous allons voir comment utiliser la pagination personnalisée.

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>Étape 2 : Ajout d’une méthode de la pagination par défaut dans la couche de logique métier

Le `ProductsBLL` classe a actuellement une méthode pour retourner toutes les informations de produit `GetProducts()` et un pour retourner un sous-ensemble particulier de produits à un index de départ `GetProductsPaged(startRowIndex, maximumRows)`. Avec la pagination par défaut, le GridView, DetailsView et FormView le contrôle toutes les utilisations du `GetProducts()` méthode pour récupérer tous les produits, mais j’utilise ensuite un `PagedDataSource` en interne pour afficher uniquement le sous-ensemble correct d’enregistrements. Pour répliquer cette fonctionnalité avec les contrôles DataList et Repeater, nous pouvons créer une nouvelle méthode dans la couche BLL qui imite ce comportement.

Ajoutez une méthode à la `ProductsBLL` classe nommée `GetProductsAsPagedDataSource` qui accepte deux paramètres d’entrée entière :

- `pageIndex` l’index de la page à afficher, indexé à zéro, et
- `pageSize` le nombre d’enregistrements à afficher par page.

`GetProductsAsPagedDataSource` commence par récupérer *tous les* des enregistrements à partir de `GetProducts()`. Il crée ensuite un `PagedDataSource` objet, en définissant son `CurrentPageIndex` et `PageSize` propriétés aux valeurs du passé en `pageIndex` et `pageSize` paramètres. La méthode se termine en renvoyant celle-ci a été configurée `PagedDataSource`:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>Étape 3 : Affichage des informations sur les produits dans un contrôle DataList à l’aide de la pagination par défaut

Avec le `GetProductsAsPagedDataSource` méthode ajoutée à la `ProductsBLL` (classe), nous pouvons créer un contrôle DataList ou le Repeater qui fournit la pagination par défaut. Commencez par ouvrir le `Paging.aspx` page dans le `PagingSortingDataListRepeater` dossier et faites glisser un contrôle DataList à partir de la boîte à outils vers le concepteur, en définissant le contrôle DataList s `ID` propriété `ProductsDefaultPaging`. À partir de la balise active DataList s, créez un nouveau ObjectDataSource nommé `ProductsDefaultPagingDataSource` et configurez-la afin qu’il récupère des données à l’aide du `GetProductsAsPagedDataSource` (méthode).


[![Créer un ObjectDataSource et configurez-le pour utiliser le () GetProductsAsPagedDataSource (méthode)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Figure 5**: créer un ObjectDataSource et configurez-le pour utiliser le `GetProductsAsPagedDataSource` `()` (méthode) ([cliquez pour afficher l’image en taille réelle](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))


Définir les listes déroulantes dans la mise à jour, insertion et supprimer des onglets à (None).


[![Définir les listes de liste déroulante dans la mise à jour, insertion et supprimer des onglets à (None)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Figure 6**: définir les listes de liste déroulante dans la mise à jour, insertion et supprimer des onglets à (None) ([cliquez pour afficher l’image en taille réelle](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))


Dans la mesure où le `GetProductsAsPagedDataSource` méthode nécessite deux paramètres d’entrée, l’Assistant nous demande la source de ces valeurs de paramètre.

Les index de page et les valeurs de taille de page doivent être mémorisés entre les postbacks. Ils peuvent être stockées dans l’état d’affichage, rendues persistantes dans la chaîne de requête, stockées dans des variables de session ou conservés à l’aide d’une autre technique. Pour ce didacticiel, nous allons utiliser la chaîne de requête, ce qui a l’avantage de permettre une page particulière de données à être marqué d’un signet.

En particulier, utilisez le pageIndex des champs de chaîne de requête et pageSize pour le `pageIndex` et `pageSize` paramètres, respectivement (voir la Figure 7). Prenez un moment pour définir les valeurs par défaut pour ces paramètres, comme les valeurs de chaîne de requête a gagné t être présent lorsqu’un utilisateur visite tout d’abord cette page. Pour `pageIndex`, la valeur par défaut la valeur est 0 (ce qui affiche la première page de données) et `pageSize` valeur par défaut de s à 4.


[![Utiliser la chaîne de requête comme Source pour les paramètres pageIndex et pageSize](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Figure 7**: utiliser la chaîne de requête comme Source pour le `pageIndex` et `pageSize` paramètres ([cliquez pour afficher l’image en taille réelle](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image15.png))


Après avoir configuré l’ObjectDataSource, Visual Studio crée automatiquement un `ItemTemplate` pour le contrôle DataList. Personnaliser le `ItemTemplate` afin que seuls le nom de produit s, catégorie et le fournisseur sont affichés. Également définir le contrôle DataList s `RepeatColumns` propriété à 2, son `Width` à 100 % et sa `ItemStyle` s `Width` à 50 %. Ces paramètres de la largeur fournira un espacement identique pour les deux colonnes.

Après avoir apporté ces modifications, le balisage de s contrôles DataList et ObjectDataSource doit ressembler à ce qui suit :


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

> [!NOTE]
> Étant donné que nous n’effectuent pas une mise à jour ou supprimer des fonctionnalités dans ce didacticiel, vous pouvez désactiver l’état d’affichage DataList s pour réduire la taille de la page rendue.


Lors de l’initialement visite cette page via un navigateur, ni le `pageIndex` ni `pageSize` paramètres querystring sont fournis. Par conséquent, les valeurs par défaut de 0 et 4 sont utilisés. Comme le montre la Figure 8, cela entraîne un contrôle DataList qui affiche les quatre premiers produits.


[![Les quatre premiers produits sont répertoriés.](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image16.png)

**Figure 8**: le premier quatre produits sont répertoriés ([cliquez pour afficher l’image en taille réelle](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image18.png))


Sans une interface de pagination, là s actuellement non simple signifie pour un utilisateur de naviguer vers la deuxième page de données. Nous allons créer une interface de pagination à l’étape 4. Pour l’instant, cependant, la pagination peut uniquement faire directement en spécifiant les critères de la pagination dans la chaîne de requête. Par exemple, pour afficher la deuxième page, modifiez l’URL dans la barre d’adresse de navigateur s à partir de `Paging.aspx` à `Paging.aspx?pageIndex=2` et appuyez sur ENTRÉE. Cela entraîne la deuxième page de données à afficher (voir Figure 9).


[![La deuxième Page de données s’affiche.](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image19.png)

**Figure 9**: la deuxième Page de données s’affiche ([cliquez pour afficher l’image en taille réelle](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image21.png))


## <a name="step-4-creating-the-paging-interface"></a>Étape 4 : Création de l’Interface de pagination

Il existe une variété de différentes interfaces de pagination qui peut être implémentée. Les contrôles GridView, DetailsView et FormView fournissent quatre différentes interfaces de choisir entre :

- **Ensuite, précédente** les utilisateurs peuvent déplacer une page à la fois, à l’une de suivante ou précédente.
- **Suivant, précédent, premier, dernier** en plus des boutons suivant et précédent, cette interface inclut des premier et dernier boutons de déplacement vers la toute première ou dernière page.
- **Numérique** répertorie les numéros de page dans l’interface de pagination, qui permet à un utilisateur accéder rapidement à une page particulière.
- **Numérique, tout d’abord, dernière** outre les numéros de page numérique, propose les boutons de déplacement vers la toute première ou dernière page.

Pour DataList et Repeater, nous sommes responsables pour le choix d’une interface de pagination et de son implémentation. Cela implique la création de contrôles Web nécessaires dans la page et en affichant la page demandée lors de l’utilisateur clique sur un bouton d’interface de pagination particulier. En outre, certains contrôles d’interface de pagination peuvent doit être désactivé. Par exemple, lorsque vous affichez la première page de données à l’aide de l’autre, précédent, tout d’abord, dernière interface, le premier et le précédent boutons sont désactivées.

Pour ce didacticiel, let s, utilisez le suivant, précédent, tout d’abord, dernière interface. Ajoutez quatre contrôles Web de bouton à la page et définissez leurs `ID` s à `FirstPage`, `PrevPage`, `NextPage`, et `LastPage`. Définir le `Text` propriétés à &lt; &lt; tout d’abord, &lt; préc., en regard &gt;et le dernier &gt; &gt; .


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample4.aspx)]

Ensuite, créez un `Click` Gestionnaire d’événements pour chacun de ces boutons. Dans un instant, nous allons ajouter le code nécessaire pour afficher la page demandée.

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>Mémoriser le nombre Total d’enregistrements en cours de pagination via

Quel que soit l’interface de pagination sélectionnée, nous devons de calcul et n’oubliez pas le nombre total d’enregistrements en cours de pagination via. Le nombre total de lignes (conjointement avec la taille de page) détermine le nombre total de pages de données sont averti par radiomessagerie, qui détermine les contrôles d’interface de pagination sont ajoutés ou sont activés. Dans le suivant, précédent, première, dernière interface que nous créons, le nombre de pages est utilisé de deux manières :

- Pour déterminer si nous consultons la dernière page, auquel cas les boutons suivant et dernier sont désactivées.
- Si l’utilisateur clique sur le bouton dernière, que nous devons les tous à la dernière page, dont l’index est une inférieure à la page compte.

Le nombre de pages est calculé comme le plafond du nombre total de lignes divisée par la taille de page. Par exemple, si nous avons la pagination par le biais des 79 enregistrements avec quatre enregistrements par page, puis le nombre de pages est de 20 (le plafond de 79 / 4). Si nous utilisons l’interface de pagination numérique, ces informations nous informe concernant le nombre de boutons de page numériques à afficher ; Si notre interface de pagination inclut les boutons suivant ou la dernière, le nombre de pages est utilisé pour déterminer quand désactiver les boutons suivant ou dernier.

Si l’interface de pagination inclut un bouton en dernier, il est impératif que le nombre total d’enregistrements en cours de pagination via être mémorisées entre les postbacks afin que lorsque l’utilisateur clique sur le bouton de dernière, nous pouvons déterminer le dernier index de page. Pour ce faire, créez un `TotalRowCount` propriété dans la classe code-behind de page s ASP.NET qui conserve sa valeur à l’état d’affichage :


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

En plus de `TotalRowCount`, prenez une minute pour créer des propriétés de niveau de la page en lecture seule pour accéder facilement à l’index de page, la taille de la page et du nombre de pages :


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample6.vb)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>Détermination du nombre Total d’enregistrements en cours de pagination via

Le `PagedDataSource` objet retourné à partir de l’ObjectDataSource s `Select()` méthode a dedans *tous les* des enregistrements produit, même si uniquement un sous-ensemble d'entre eux sont affichés dans le contrôle DataList. Le `PagedDataSource` s [ `Count` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.count.aspx) retourne uniquement le nombre d’éléments qui s’affichera dans le contrôle DataList ; le [ `DataSourceCount` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx) retourne le nombre total d’éléments dans le `PagedDataSource`. Par conséquent, nous devons affecter ASP.NET page s `TotalRowCount` propriété la valeur de la `PagedDataSource` s `DataSourceCount` propriété.

Pour ce faire, créez un gestionnaire d’événements pour les opérations de mappage ObjectDataSource `Selected` événement. Dans le `Selected` Gestionnaire d’événements, nous avons accès à la valeur de retour de l’ObjectDataSource s `Select()` méthode dans ce cas, le `PagedDataSource`.


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

## <a name="displaying-the-requested-page-of-data"></a>Affichage de la Page demandée de données

Lorsque l’utilisateur clique sur un des boutons dans l’interface de pagination, nous devons afficher la page demandée de données. Étant donné que les paramètres de pagination sont spécifiés par le biais de la chaîne de requête pour afficher la page demandée de données, utilisez `Response.Redirect(url)` pour que l’utilisateur s navigateur redemander le `Paging.aspx` page avec les paramètres appropriés de la pagination. Par exemple, pour afficher la deuxième page de données, nous redirige l’utilisateur à `Paging.aspx?pageIndex=1`.

Pour ce faire, créez un `RedirectUser(sendUserToPageIndex)` méthode qui redirige l’utilisateur vers `Paging.aspx?pageIndex=sendUserToPageIndex`. Ensuite, appelez cette méthode à partir du bouton quatre `Click` gestionnaires d’événements. Dans le `FirstPage` `Click` Gestionnaire d’événements, appelez `RedirectUser(0)`, de les envoyer à la première page ; dans le `PrevPage` `Click` Gestionnaire d’événements, utilisez `PageIndex - 1` en tant que l’index de page ; et ainsi de suite.


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample8.vb)]

Avec le `Click` terminer des gestionnaires d’événements, les enregistrements de DataList s peuvent être contactés via en cliquant sur les boutons. Prenez un moment pour l’essayer !

## <a name="disabling-paging-interface-controls"></a>La désactivation de la pagination des contrôles d’Interface

Actuellement, les quatre boutons sont activées, quel que soit la page affichée. Toutefois, nous souhaitons désactiver les boutons de premier et précédent lors de l’affichage de la première page de données et les boutons suivant et dernier lors de l’affichage de la dernière page. Le `PagedDataSource` objet retourné par les opérations de mappage ObjectDataSource `Select()` méthode a des propriétés [ `IsFirstPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx) et [ `IsLastPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) que nous pouvons examiner pour déterminer si nous consultons la première ou dernière page de données.

Ajoutez le code suivant pour les opérations de mappage ObjectDataSource `Selected` Gestionnaire d’événements :


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

Avec cet ajout, les boutons premier et précédent seront désactivées lors de l’affichage de la première page, tandis que les boutons suivant et dernier seront désactivés lors de l’affichage de la dernière page.

S permettent de réaliser l’interface de pagination en informant l’utilisateur qu’ils page re actuellement affiché et le nombre total de pages existent. Ajoutez un contrôle Web Label à la page et définissez son `ID` propriété `CurrentPageNumber`. Définissez ses `Text` propriété ObjectDataSource s sélectionnés Gestionnaire d’événements tel qu’il inclut la page actuelle affichée (`PageIndex + 1`) et le nombre total de pages (`PageCount`).


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample10.vb)]

La figure 10 illustre `Paging.aspx` lors de la première visite. Étant donné que la chaîne de requête est vide, le contrôle DataList est par défaut affichant les quatre premiers produits ; les boutons premier et précédent sont désactivées. Cliquez sur Suivant pour afficher les quatre enregistrements (voir Figure 11) ; le premier et précédent sont maintenant activées.


[![La première Page de données s’affiche.](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image22.png)

**Figure 10**: la première Page de données s’affiche ([cliquez pour afficher l’image en taille réelle](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image24.png))


[![La deuxième Page de données s’affiche.](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image25.png)

**Figure 11**: la deuxième Page de données s’affiche ([cliquez pour afficher l’image en taille réelle](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image27.png))


> [!NOTE]
> L’interface de pagination peut encore être améliorée en permettant à l’utilisateur de spécifier le nombre de pages pour afficher par page. Par exemple, la liste des options de taille de page comme 5, 10, 25, 50 et tous les pu être ajouté à un contrôle DropDownList. Lors de la sélection d’une taille de page, l’utilisateur doit être redirigé vers `Paging.aspx?pageIndex=0&pageSize=selectedPageSize`. Je laisse l’implémentation de cette amélioration en guise d’exercice pour le lecteur.


## <a name="using-custom-paging"></a>À l’aide de la pagination personnalisée

Les pages DataList via ses données à l’aide de la technique de la pagination par défaut inefficace. Lors de la pagination via suffisamment grandes quantités de données, il est impératif que la pagination personnalisée est utilisée. Bien que les détails d’implémentation diffèrent légèrement, les concepts sous-jacents à l’implémentation de la pagination personnalisée dans un contrôle DataList sont identiques à celles de la pagination par défaut. Avec la pagination personnalisée, utilisez la `ProductBLL` classe s `GetProductsPaged` (méthode) (au lieu de `GetProductsAsPagedDataSource`). Comme indiqué dans le [efficacement la pagination par le biais d’importants volumes de données](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) didacticiel, `GetProductsPaged` doivent être passés début index et maximum numéro de ligne de lignes à retourner. Ces paramètres peuvent être gérés via la chaîne de requête à l’instar du `pageIndex` et `pageSize` paramètres utilisés par défaut la pagination.

Depuis s il y a aucune `PagedDataSource` avec la pagination personnalisée, les autres techniques doivent être utilisés pour déterminer le nombre total d’enregistrements en cours de pagination via et si nous re affichant la première ou dernière page de données. Le `TotalNumberOfProducts()` méthode dans la `ProductsBLL` classe retourne le nombre total de produits en cours de pagination via. Pour déterminer si la première page de données est affichée, examinez l’index de ligne de début s’il est égal à zéro, puis la première page est affichée. Si l’index de ligne de début plus le nombre de lignes maximal à retourner est supérieur ou égal au nombre total d’enregistrements en cours de pagination par le biais de la dernière page est affichée.

Nous allons explorer l’implémentation de la pagination personnalisée en détail dans le didacticiel suivant.

## <a name="summary"></a>Récapitulatif

Alors que Repeater ni DataList offre le hors de la prise en charge la pagination de zone trouvé dans le contrôle GridView, DetailsView et contrôle FormView, ces fonctionnalités peuvent être ajoutées avec un minimum d’effort. Le moyen le plus simple pour implémenter la pagination par défaut consiste à encapsuler l’ensemble des produits au sein d’un `PagedDataSource` , puis lier la `PagedDataSource` pour le contrôle DataList ou Repeater. Dans ce didacticiel, nous avons ajouté la `GetProductsAsPagedDataSource` méthode à la `ProductsBLL` classe pour retourner le `PagedDataSource`. Le `ProductsBLL` classe contient déjà les méthodes nécessaires pour la pagination personnalisée `GetProductsPaged` et `TotalNumberOfProducts`.

En même temps que la récupération de l’ensemble d’enregistrements à afficher pour la pagination personnalisée précis ou tous les enregistrements dans un `PagedDataSource` pour la pagination par défaut, nous devons également ajouter manuellement l’interface de pagination. Pour ce didacticiel, nous avons créé un ensuite, le précédent, tout d’abord, dernière interface avec quatre contrôles bouton Web. En outre, un contrôle d’étiquette affichant le numéro de page actuel et le nombre total de pages a été ajouté.

Dans le didacticiel suivant, nous allons voir comment ajouter la prise en charge de tri pour les contrôles DataList et Repeater. Nous verrons également comment créer un contrôle DataList qui peut être paginé et trié (avec exemples à l’aide de la valeur par défaut et la pagination personnalisée).

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Liz Shulok, Ken Pespisa et Bernadette Leigh. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](sorting-data-in-a-datalist-or-repeater-control-cs.md)
> [Suivant](sorting-data-in-a-datalist-or-repeater-control-vb.md)
