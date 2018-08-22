---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
title: Pagination et tri rapports de données (c#) | Microsoft Docs
author: rick-anderson
description: Pagination et tri sont deux fonctionnalités très courantes lors de l’affichage des données dans une application en ligne. Dans ce didacticiel, nous allons un premier aperçu de l’ajout du tri et...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 811a6ef2-ec66-4c8e-a089-6f795056e288
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 5ebef919deeda409cfa6805b603f67ef96ff003e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824319"
---
<a name="paging-and-sorting-report-data-c"></a>La pagination et tri des données de rapport (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_CS.exe) ou [télécharger le PDF](paging-and-sorting-report-data-cs/_static/datatutorial24cs1.pdf)

> Pagination et tri sont deux fonctionnalités très courantes lors de l’affichage des données dans une application en ligne. Dans ce didacticiel, nous allons un premier aperçu de l’ajout de tri et de pagination à vos rapports, nous allons créer puis à des didacticiels dans les futures.


## <a name="introduction"></a>Introduction

Pagination et tri sont deux fonctionnalités très courantes lors de l’affichage des données dans une application en ligne. Par exemple, lors de la recherche pour la documentation d’ASP.NET à une librairie en ligne, il peut y avoir des centaines d’ouvrages, mais le rapport affichant les résultats de recherche répertorie les correspondances seulement dix par page. En outre, les résultats peuvent être triées par titre, prix, nombre de pages, nom de l’auteur et ainsi de suite. Tandis que le passé des 23 didacticiels ont examiné comment créer toute une gamme de rapports, notamment les interfaces permettant l’ajout, modification et suppression de données, nous ve ne pas examiné comment trier les données et la seule pagination exemples nous ve vu ont été des contrôles DetailsView et FormView contrôles.

Dans ce didacticiel, nous allons voir comment ajouter le tri et de pagination à vos rapports, ce qui peuvent être effectués en activant simplement quelques cases à cocher. Malheureusement, cette implémentation simpliste a ses inconvénients de que l’interface de tri laisse un peu plus tard à désirer et les routines de pagination ne sont pas conçus pour la pagination efficace dans de grands jeux de résultats. Didacticiels futures explorerons comment surmonter les restrictions de l’out-of-the-box pagination et tri des solutions.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Étape 1 : Ajout de la pagination et tri des Pages Web didacticiels

Avant de commencer ce didacticiel, laissez s tout d’abord prendre un moment pour ajouter les pages ASP.NET que nous aurons besoin de ce didacticiel et les trois suivants. Commencez par créer un nouveau dossier dans le projet nommé `PagingAndSorting`. Ensuite, ajoutez les pages ASP.NET cinq suivantes à ce dossier, présentant toutes les configurer pour utiliser la page maître `Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`


![Créez un dossier PagingAndSorting et ajouter les Pages ASP.NET didacticiel](paging-and-sorting-report-data-cs/_static/image1.png)

**Figure 1**: créez un dossier PagingAndSorting et ajouter les Pages ASP.NET didacticiel


Ensuite, ouvrez le `Default.aspx` page et faites glisser le `SectionLevelTutorialListing.ascx` contrôle utilisateur à partir de la `UserControls` dossier sur l’aire de conception. Ce contrôle utilisateur, que nous avons créée dans le [Pages maîtres et Navigation du Site](../introduction/master-pages-and-site-navigation-cs.md) didacticiel, énumère le plan du site et affiche ces didacticiels dans la section en cours dans une liste à puces.


![Ajouter le contrôle utilisateur de SectionLevelTutorialListing.ascx à Default.aspx](paging-and-sorting-report-data-cs/_static/image2.png)

**Figure 2**: ajouter le contrôle utilisateur de SectionLevelTutorialListing.ascx à Default.aspx


Pour afficher la pagination et tri des didacticiels, que nous allons créer la liste à puces, nous avons besoin de les ajouter au plan du site. Ouvrez le `Web.sitemap` fichier, puis ajoutez le balisage suivant après la balise de nœud de mappage site Édition, insertion et suppression :


[!code-xml[Main](paging-and-sorting-report-data-cs/samples/sample1.xml)]


![Mettre à jour le plan du Site pour inclure les nouvelles Pages ASP.NET](paging-and-sorting-report-data-cs/_static/image3.png)

**Figure 3**: mettre à jour le plan du Site pour inclure les nouvelles Pages ASP.NET


## <a name="step-2-displaying-product-information-in-a-gridview"></a>Étape 2 : Affichage des informations sur les produits dans un GridView

Avant de nous implémentons réellement la pagination et des fonctionnalités de tri, permettent de s tout d’abord créer un standard non-srotable, non paginable GridView qui répertorie les informations de produit. Il s’agit d’une tâche nous ve fait plusieurs fois avant tout au long de cette série de didacticiels, par conséquent, ces étapes doit être familier. Commencez par ouvrir le `SimplePagingSorting.aspx` page et faites glisser un contrôle GridView à partir de la boîte à outils vers le concepteur, en définissant son `ID` propriété `Products`. Ensuite, créez un nouveau ObjectDataSource qui utilise la classe ProductsBLL s `GetProducts()` méthode pour retourner toutes les informations de produit.


![Récupérer des informations sur tous les produits à l’aide de la méthode GetProducts()](paging-and-sorting-report-data-cs/_static/image4.png)

**Figure 4**: récupérer des informations sur tous les produits à l’aide de la méthode GetProducts()


Dans la mesure où ce rapport est un rapport en lecture seule s’il n’avez besoin mapper le s ObjectDataSource `Insert()`, `Update()`, ou `Delete()` méthodes correspondant `ProductsBLL` méthodes ; par conséquent, choisissez (aucun) dans la liste déroulante pour la mise à jour, insertion, et supprimer les tabulations.


![Choisissez (aucun) Option dans la liste déroulante dans la mise à jour, insertion et supprimer des onglets](paging-and-sorting-report-data-cs/_static/image5.png)

**Figure 5**: choisissez (aucun) Option dans la liste déroulante dans la mise à jour, insertion et supprimer des onglets


Ensuite, permettent de s personnaliser les champs de s GridView afin qu’uniquement des noms de produits, fournisseurs, catégories, prix et états supprimées sont affichées. En outre, vous pouvez effectuer la mise en forme au niveau du champ change, notamment ajuster le `HeaderText` de propriétés ou de mise en forme le prix sous forme de devise. Après ces modifications, votre balisage déclaratif de GridView s doit ressembler à ce qui suit :


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample2.aspx)]

Figure 6 illustre notre progression jusqu'à présent lorsqu’ils sont affichés via un navigateur. Notez que la page répertorie tous les produits dans un seul écran, en affichant chaque nom de produit s, catégorie, fournisseur, les prix et supprimées d’état.


[![Chacun des produits répertoriés](paging-and-sorting-report-data-cs/_static/image7.png)](paging-and-sorting-report-data-cs/_static/image6.png)

**Figure 6**: chacun des produits sont répertoriés ([cliquez pour afficher l’image en taille réelle](paging-and-sorting-report-data-cs/_static/image8.png))


## <a name="step-3-adding-paging-support"></a>Étape 3 : Ajout de prise en charge la pagination

Liste *tous les* des produits sur un seul écran peut entraîner la surcharge d’informations pour l’utilisateur qui lit les données. Pour aider à rendre les résultats plus gérables, nous pouvons divisez les données dans les pages de données plus petits et autoriser l’utilisateur à parcourir les données d’une page à la fois. Pour accomplir cela simplement cocher la case à cocher Activer la pagination de la balise active de s GridView (Cela définit les opérations de mappage GridView [ `AllowPaging` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx) à `true`).


[![La case Activer la pagination pour ajouter la prise en charge la pagination](paging-and-sorting-report-data-cs/_static/image10.png)](paging-and-sorting-report-data-cs/_static/image9.png)

**Figure 7**: case à cocher Activer la pagination pour ajouter la prise en charge la pagination ([cliquez pour afficher l’image en taille réelle](paging-and-sorting-report-data-cs/_static/image11.png))


L’activation de la pagination limite le nombre d’enregistrements affichés par page et ajoute un *interface de pagination* au GridView. L’interface de pagination par défaut, illustrée dans la Figure 7, est une série de numéros de page, permettant à l’utilisateur accéder rapidement à partir d’une page de données à un autre. Cette interface de pagination doit vous sembler familière, comme nous ve vu lors de l’ajout de prise en charge la pagination pour les contrôles DetailsView et FormView dans ces didacticiels.

Le contrôle DetailsView et FormView contrôles montrent uniquement un seul enregistrement par page. Le contrôle GridView, toutefois, consulte sa [ `PageSize` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx) pour déterminer le nombre d’enregistrements à afficher par page (cette propriété par défaut est une valeur de 10).

Cette interface de pagination s GridView, DetailsView et FormView peut être personnalisée en utilisant les propriétés suivantes :

- `PagerStyle` Indique les informations de style pour l’interface de pagination ; peut spécifier de paramètres tels que `BackColor`, `ForeColor`, `CssClass`, `HorizontalAlign`, et ainsi de suite.
- `PagerSettings` contient une multitude de propriétés que vous pouvez personnaliser les fonctionnalités de l’interface de pagination ; `PageButtonCount` indique le nombre maximal de numéros de page numérique affichée dans l’interface de pagination (la valeur par défaut est 10) ; le [ `Mode` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx) indique la façon dont l’interface de pagination fonctionne et peut être définie sur : 

    - `NextPrevious` montre un boutons suivant et précédent, autorisant l’utilisateur à l’étape avancer ou reculer une page à la fois
    - `NextPreviousFirstLast` en plus des boutons suivant et précédent, premier et dernier boutons sont également inclus, permettant à l’utilisateur à accéder rapidement à la première ou dernière page de données
    - `Numeric` présente une série de numéros de page, permettant à l’utilisateur d’accéder immédiatement à n’importe quelle page
    - `NumericFirstLast` Outre les numéros de page, inclut les boutons première et dernière, autorisant l’utilisateur à accéder rapidement à la première ou dernière page de données ; les premier/dernier les boutons sont affichés uniquement si tous les nombres de pages numériques ne rentrent pas

En outre, le GridView, DetailsView et FormView offrent tous les `PageIndex` et `PageCount` propriétés, qui indiquent la page actuelle en cours de visualisation et le nombre total de pages de données, respectivement. Le `PageIndex` propriété est indexée en commençant à 0, ce qui signifie que, lors de l’affichage de la première page de données `PageIndex` est égale à 0. `PageCount`, quant à eux, démarre comptage à 1, ce qui signifie que `PageIndex` est limité pour les valeurs comprises entre 0 et `PageCount - 1`.

Permettent de prendre un moment pour améliorer l’apparence par défaut de notre interface de pagination de GridView s s. Plus précisément, vous autorise à s pour disposer de l’interface de pagination alignés à droite avec un arrière-plan gris clair. Au lieu de définir ces propriétés directement par le biais de la s GridView `PagerStyle` propriété, s permettent de créer une classe CSS dans `Styles.css` nommé `PagerRowStyle` , puis attribuez le `PagerStyle` s `CssClass` propriété via notre thème. Commencez par ouvrir `Styles.css` et de la définition de classe ajoutant le code CSS suivant :


[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample3.css)]

Ensuite, ouvrez le `GridView.skin` de fichiers dans le `DataWebControls` dossier au sein de la `App_Themes` dossier. Comme expliqué dans la *Pages maîtres et Navigation du Site* les fichiers de didacticiel, peau peuvent être utilisés pour spécifier les valeurs de propriété par défaut pour un contrôle Web. Par conséquent, augmenter les paramètres existants pour inclure le paramètre de la `PagerStyle` s `CssClass` propriété `PagerRowStyle`. En outre, s permettent de configurer l’interface de pagination à afficher au maximum cinq pages numériques de boutons à l’aide de la `NumericFirstLast` interface de pagination.


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>L’expérience utilisateur de pagination

La figure 8 illustre la page web quand consultées via un navigateur une fois que la case Activer la pagination de GridView s a été vérifiée et le `PagerStyle` et `PagerSettings` configurations ont été apportées par le biais du `GridView.skin` fichier. Notez comment seuls dix enregistrements sont affichés, et l’interface de pagination indique que nous consultons la première page de données.


[![Avec la pagination est activée, uniquement un sous-ensemble des enregistrements sont affichés à la fois](paging-and-sorting-report-data-cs/_static/image13.png)](paging-and-sorting-report-data-cs/_static/image12.png)

**Figure 8**: avec la pagination activé, uniquement un sous-ensemble des enregistrements sont affichés à la fois ([cliquez pour afficher l’image en taille réelle](paging-and-sorting-report-data-cs/_static/image14.png))


Lorsque l’utilisateur clique sur l’un des numéros de page dans l’interface de pagination, s’ensuit une publication (postback) et la page recharge montrant que les enregistrements de la page s requis. La figure 9 illustre les résultats après vous être inscrit pour afficher la dernière page de données. Notez que la dernière page a uniquement un seul enregistrement ; Il s’agit, car il existe des 81 enregistrements au total, ce qui entraîne des huit pages de 10 enregistrements par page, plus une page avec un seul enregistrement.


[![En cliquant sur un numéro de Page provoque une publication (postback) et montre le sous-ensemble approprié d’enregistrements](paging-and-sorting-report-data-cs/_static/image16.png)](paging-and-sorting-report-data-cs/_static/image15.png)

**Figure 9**: en cliquant sur un numéro de Page provoque une publication (postback) et montre le sous-ensemble d’enregistrements appropriés ([cliquez pour afficher l’image en taille réelle](paging-and-sorting-report-data-cs/_static/image17.png))


## <a name="paging-s-server-side-workflow"></a>Flux de travail de la pagination s côté serveur

Lorsque l’utilisateur final clique sur un bouton dans l’interface de pagination, s’ensuit une publication (postback) et commence le flux de travail côté serveur suivant :

1. Le GridView s (ou DetailsView ou FormView) `PageIndexChanging` se déclenche des événements
2. ObjectDataSource ré-demande *tous les* des données à partir de la couche BLL ; le s GridView `PageIndex` et `PageSize` les valeurs de propriété sont utilisées pour déterminer les enregistrements retournés à partir de la couche BLL à afficher dans le contrôle GridView
3. Les opérations de mappage GridView `PageIndexChanged` se déclenche des événements

À l’étape 2, ObjectDataSource ré-demande toutes les données à partir de sa source de données. Ce style de pagination est communément appelé *la pagination par défaut*, tel qu’il s le comportement de pagination utilisé par défaut lors de la définition du `AllowPaging` propriété `true`. Valeur par défaut la pagination des données contrôle Web naïvement récupère tous les enregistrements pour chaque page de données, même si seul un sous-ensemble des enregistrements réellement rendues dans le code HTML que s envoyée au navigateur. À moins que la base de données est mis en cache par la couche BLL ou ObjectDataSource, la pagination par défaut est rédhibitoire pour suffisamment grands jeux de résultats ou des applications web avec plusieurs utilisateurs simultanés.

Dans le didacticiel suivant, nous allons examiner comment implémenter *la pagination personnalisée*. Avec la pagination personnalisée vous pouvez demander spécifiquement à ObjectDataSource afin de récupérer l’ensemble d’enregistrements nécessaires pour la page demandée de données précis. Comme vous pouvez l’imaginer, la pagination personnalisée améliore considérablement l’efficacité de la pagination de grands jeux de résultats.

> [!NOTE]
> Alors que la pagination par défaut ne c'est-à-dire pas lors de la pagination via suffisamment grands jeux de résultats ou pour les sites comportant de nombreux utilisateurs simultanés, sachez que la pagination personnalisée nécessite plus de modifications et d’efforts pour implémenter et n’est pas aussi simple que la vérification d’une case à cocher (comme c’est par défaut la pagination). Par conséquent, la pagination par défaut peut être le choix idéal pour les sites Web de petite taille, avec un trafic faible ou lorsque la pagination de résultats relativement petit définit, tel qu’il s beaucoup plus facile et plus rapide à implémenter.


Par exemple, si nous savons que nous aurons jamais plus de 100 produits dans notre base de données, le gain de performances minimal apprécié par la pagination personnalisée est probablement compensé par l’effort requis pour l’implémenter. Si, toutefois, nous avons un jour peut-être des milliers, voire des dizaines de milliers de produits, *pas* implémentant la pagination personnalisée est nuire considérablement l’évolutivité de notre application.

## <a name="step-4-customizing-the-paging-experience"></a>Étape 4 : Personnalisation de l’expérience de la pagination

Contrôles data Web fournissent un nombre de propriétés qui peut être utilisé pour améliorer l’expérience de la pagination utilisateur s. Le `PageCount` propriété, par exemple, indique que le nombre total de pages sont, tandis que le `PageIndex` propriété indique la page actuelle qui est visitée et peut être définie pour déplacer rapidement un utilisateur à une page spécifique. Pour illustrer comment utiliser ces propriétés pour améliorer l’expérience de la pagination utilisateur s, permettent d’ajouter une étiquette s Web le contrôle à notre page qui informe l’utilisateur quelle page ils re actuellement consultant, ainsi que d’un contrôle DropDownList qui leur permet d’accéder rapidement à n’importe quelle page donnée .

Tout d’abord, ajoutez un contrôle Web Label à votre page, en définissant son `ID` propriété `PagingInformation`et effacer son `Text` propriété. Ensuite, créez un gestionnaire d’événements pour les opérations de mappage GridView `DataBound` événement et ajoutez le code suivant :


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample5.cs)]

Ce gestionnaire d’événements affecte le `PagingInformation` étiquette s `Text` propriété à un message informant l’utilisateur de la page qu’ils visitent `Products.PageIndex + 1` hors le nombre total de pages `Products.PageCount` (nous ajouter 1 au `Products.PageIndex` propriété, car `PageIndex` est indexée en commençant à 0). J’ai choisi l’attribuer à cette étiquette s `Text` propriété dans le `DataBound` Gestionnaire d’événements par opposition à la `PageIndexChanged` Gestionnaire d’événements, car le `DataBound` événement est déclenché chaque fois que les données sont liées au GridView tandis que le `PageIndexChanged` Gestionnaire d’événements se déclenche lorsque l’index de page est modifiée. Lorsque le contrôle GridView est initialement lié aux données sur la première page à consulter, le `PageIndexChanging` événement déclencheur de t ne (alors que le `DataBound` événement).

Avec cet ajout, l’utilisateur voit désormais un message indiquant quelle page ils visitent et le nombre total de pages de données est.


[![Le numéro de Page actuel et le nombre Total de Pages sont affichés.](paging-and-sorting-report-data-cs/_static/image19.png)](paging-and-sorting-report-data-cs/_static/image18.png)

**Figure 10**: le numéro de Page actuel et le nombre Total de Pages sont affichés ([cliquez pour afficher l’image en taille réelle](paging-and-sorting-report-data-cs/_static/image20.png))


Outre le contrôle d’étiquette, permettent de s également ajouter un contrôle DropDownList qui répertorie les numéros de page dans le contrôle GridView avec la page actuellement affichée est sélectionnée. L’idée ici est que l’utilisateur peut accéder rapidement à partir de la page actuelle à l’autre en sélectionnant simplement le nouvel index de page dans la liste DropDownList. Commencez par ajouter un contrôle DropDownList vers le concepteur, en définissant son `ID` propriété `PageList` et la vérification de l’option Activer AutoPostBack à partir de sa balise active.

Ensuite, revenez à la `DataBound` Gestionnaire d’événements et ajoutez le code suivant :


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample6.cs)]

Ce code commence en supprimant les éléments dans le `PageList` DropDownList. Cela peut sembler superflu, dans la mesure où un t n’expliquerait pas attendre le nombre de pages à modifier, mais les autres utilisateurs peuvent être à l’aide du système simultanément, l’ajout ou suppression d’enregistrements à partir de la `Products` table. Insertions ou des suppressions peuvent modifier le nombre de pages de données.

Ensuite, nous devons créer les numéros de page à nouveau et celui qui mappe au GridView actuel `PageIndex` sélectionné par défaut. Effectuer cette opération avec une boucle allant de 0 à `PageCount - 1`, ajout d’une nouvelle `ListItem` dans chaque itération et le paramètre son `Selected` propriété sur true si l’index d’itération actuelle est égale à la s GridView `PageIndex` propriété.

Enfin, nous devons créer un gestionnaire d’événements pour les opérations de mappage DropDownList `SelectedIndexChanged` événement qui se déclenche chaque fois que l’utilisateur choisir un autre élément dans la liste. Pour créer ce gestionnaire d’événements, double-cliquez sur l’objet DropDownList dans le concepteur, puis ajoutez le code suivant :


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample7.cs)]

Comme le montre la Figure 11, changez simplement le s GridView `PageIndex` propriété entraîne les données à être reliée au GridView. Dans le s GridView `DataBound` Gestionnaire d’événements, la liste DropDownList appropriée `ListItem` est sélectionné.


[![L’utilisateur est automatiquement dirigé vers la sixième Page lors de la sélection de l’élément de liste déroulante de Page 6](paging-and-sorting-report-data-cs/_static/image22.png)](paging-and-sorting-report-data-cs/_static/image21.png)

**Figure 11**: l’utilisateur est automatiquement dirigé vers la sixième Page lors de la sélection de l’élément de liste déroulante de Page 6 ([cliquez pour afficher l’image en taille réelle](paging-and-sorting-report-data-cs/_static/image23.png))


## <a name="step-5-adding-bi-directional-sorting-support"></a>Étape 5 : Ajout de prise en charge du tri bidirectionnelle

Ajout de prise en charge du tri bidirectionnelle est aussi simple que d’ajouter la prise en charge la pagination simplement cocher l’option Activer le tri à partir de la balise active de s GridView (qui définit les opérations de mappage GridView [ `AllowSorting` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx) à `true`). Cela affiche chacun des en-têtes des champs s GridView en tant que type LinkButton qui, lorsque vous cliquez sur, provoquer une publication (postback) et retourner les données triées par la colonne sélectionnée dans l’ordre croissant. Cliquez à nouveau sur le même en-tête LinkButton retrie les données dans l’ordre décroissant.

> [!NOTE]
> Si vous utilisez une couche d’accès aux données personnalisées plutôt que d’un DataSet typé, peut-être pas une option d’activer le tri dans la balise active de s GridView. Uniquement les contrôles GridView liés à des sources de données qui prennent en charge de tri ont cette case à cocher est disponible. Le DataSet typé fournit la prise en charge du tri d’out-of-the-box dans la mesure où l’objet DataTable ADO.NET fournit un `Sort` méthode qui, lorsqu’elle est appelée, trie les s DataTable DataRows à l’aide des critères spécifiés.


Si votre couche DAL ne retourne pas les objets en mode natif la prise en charge de tri que vous devrez configurer ObjectDataSource pour transmettre des informations de tri à la couche de logique métier, qui peut trier les données ou les données triée par la couche DAL. Nous allons découvrir comment trier les données à la logique métier et couches d’accès aux données dans un futur didacticiel.

Le type de tri LinkButton est rendus sous forme de liens hypertexte HTML, dont les couleurs actuelles (bleues pour un lien non visité et rouge foncé pour un lien visité) entrer en conflit avec la couleur d’arrière-plan de la ligne d’en-tête. Au lieu de cela, let s ont tous les liens de ligne d’en-tête affichés en blanc, indépendamment de si elles ve été visité ou non. Cela est possible en ajoutant le code suivant à la `Styles.css` classe :


[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample8.css)]

Cette syntaxe indique l’utilisation de texte blanc lors de l’affichage des liens hypertexte dans un élément qui utilise la classe HeaderStyle.

Après cet ajout CSS, lorsque vous visitez la page via un navigateur votre écran doit ressembler à la Figure 12. En particulier, la Figure 12 montre les résultats après un clic sur le lien d’en-tête prix champ s.


[![Les résultats ont été triés par le prix unitaire dans l’ordre croissant](paging-and-sorting-report-data-cs/_static/image25.png)](paging-and-sorting-report-data-cs/_static/image24.png)

**Figure 12**: les résultats ont été triés par le prix unitaire par ordre croissant ([cliquez pour afficher l’image en taille réelle](paging-and-sorting-report-data-cs/_static/image26.png))


## <a name="examining-the-sorting-workflow"></a>Examiner le flux de travail de tri

GridView tous les champs le BoundField, CheckBoxField, TemplateField, et ainsi de suite ont un `SortExpression` propriété qui indique l’expression qui doit être utilisée pour trier les données de cas de clic sur ce champ s tri lien d’en-tête. Ce GridView comporte également un `SortExpression` propriété. Lorsqu’un en-tête de tri utilisateur clique sur le LinkButton, le contrôle GridView affecte ce champ s `SortExpression` valeur son `SortExpression` propriété. Ensuite, les données sont ré-récupérées à partir de l’ObjectDataSource et triées en fonction de la s GridView `SortExpression` propriété. La liste suivante décrit en détail la séquence d’étapes qui se passe réellement lorsqu’un utilisateur final trie les données dans un GridView :

1. Les opérations de mappage GridView [événement Sorting](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx) se déclenche
2. Les opérations de mappage GridView [ `SortExpression` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) est défini sur le `SortExpression` du champ dont l’utilisateur a cliqué sur LinkButton l’en-tête tri
3. ObjectDataSource nouveau récupère toutes les données à partir de la couche BLL, puis trie les données à l’aide de la s GridView `SortExpression`
4. Les opérations de mappage GridView `PageIndex` propriété est réinitialisée à 0, ce qui signifie que, lors du tri de l’utilisateur est renvoyé à la première page de données (en supposant la prise en charge la pagination a été implémenté)
5. Les opérations de mappage GridView [ `Sorted` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx) se déclenche

Comme avec la pagination par défaut, la valeur par défaut, option de tri ré-récupère *tous les* des enregistrements à partir de la couche BLL. Lors de l’utilisation de tri sans pagination ou lors de l’utilisation de tri avec valeur par défaut la pagination, là s aucun moyen de contourner ces performances d’accès (moins de la mise en cache de la base de données). Toutefois, comme nous le verrons dans un futur didacticiel, il s possible de trier efficacement les données lors de l’utilisation de la pagination personnalisée.

Lors de la liaison d’ObjectDataSource pour le contrôle GridView dans la liste déroulante dans la balise active de s GridView, chaque champ GridView a automatiquement son `SortExpression` propriété affectée au nom du champ de données dans le `ProductsRow` classe. Par exemple, le `ProductName` BoundField s `SortExpression` a la valeur `ProductName`, comme illustré dans le balisage déclaratif suivant :


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample9.aspx)]

Un champ peut être configuré afin qu’il s ne pouvant pas être trié en effaçant les sa `SortExpression` propriété (l’affectation d’une chaîne vide). Pour illustrer cela, imaginez que nous ne savais pas t vouliez permettre à nos clients de trier de nos produits par prix. Le `UnitPrice` BoundField s `SortExpression` propriété peut être supprimée à partir du balisage déclaratif ou via la boîte de dialogue champs (qui est accessible en cliquant sur le lien Modifier les colonnes dans la balise active de GridView s).


![Les résultats ont été triés par le prix unitaire dans l’ordre croissant](paging-and-sorting-report-data-cs/_static/image27.png)

**Figure 13**: les résultats ont été triés par le prix unitaire dans l’ordre croissant


Une fois le `SortExpression` propriété a été supprimée pour le `UnitPrice` BoundField, l’en-tête est restitué sous forme de texte plutôt que sous forme de lien, ce qui empêche les utilisateurs de trier les données par prix.


[![En supprimant la propriété SortExpression, les utilisateurs peuvent trier ne sont plus les produits par prix](paging-and-sorting-report-data-cs/_static/image29.png)](paging-and-sorting-report-data-cs/_static/image28.png)

**Figure 14**: en supprimant la propriété SortExpression, les utilisateurs peuvent trier n’est plus le prix de produits ([cliquez pour afficher l’image en taille réelle](paging-and-sorting-report-data-cs/_static/image30.png))


## <a name="programmatically-sorting-the-gridview"></a>Tri par programmation le contrôle GridView

Vous pouvez également trier le contenu du contrôle GridView par programmation à l’aide de la s GridView [ `Sort` méthode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx). Il suffit de passer le `SortExpression` valeur lequel trier avec le [ `SortDirection` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending` ou `Descending`), et les données de s GridView sera retriées.

Imaginez que la raison pour laquelle nous avons désactivé en triant le `UnitPrice` a été car nous n’avons pas inquiets que nos clients achèterait simplement les produits moins cher. Toutefois, nous souhaitons les encourager à acheter des produits les plus chers, donc nous d comme ils soient en mesure de trier les produits par prix, mais uniquement du prix de la plus coûteux pour le moins.

Pour accomplir cela ajouter un contrôle bouton Web à la page, définissez son `ID` propriété `SortPriceDescending`et son `Text` propriété trier par prix. Ensuite, créez un gestionnaire d’événements pour le bouton s `Click` événement en double-cliquant sur le contrôle de bouton dans le concepteur. Ajoutez le code suivant à ce gestionnaire d’événements :


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample10.cs)]

Cliquez sur ce bouton renvoie l’utilisateur à la première page avec les produits triés par tarif, du plus cher moins coûteuse (voir Figure 15).


[![En cliquant sur le bouton trie les produits à partir de la plus coûteuse à la moins](paging-and-sorting-report-data-cs/_static/image32.png)](paging-and-sorting-report-data-cs/_static/image31.png)

**Figure 15**: en cliquant sur le bouton trie la produits à partir de la plus coûteuse à la moins ([cliquez pour afficher l’image en taille réelle](paging-and-sorting-report-data-cs/_static/image33.png))


## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, que nous avons vu comment implémenter la pagination et des fonctionnalités de tri par défaut, qui ont été aussi simples que la vérification d’une case à cocher ! Quand un utilisateur trie ou parcourt des données, un flux de travail similaire qui se déroule :

1. S’ensuit une publication (postback)
2. Les données de contrôle Web s niveau préalable se déclenche des événements (`PageIndexChanging` ou `Sorting`)
3. Toutes les données sont ré-récupérées par ObjectDataSource
4. Les données de contrôle Web s postérieures se déclenche des événements de niveau (`PageIndexChanged` ou `Sorted`)

Tandis que l’implémentation de la pagination de base et le tri est un jeu d’enfant, plus l’effort doit être exercé d’utiliser la pagination personnalisée plus efficace ou pour améliorer davantage l’interface de pagination ou de tri. Didacticiels futures explorerons ces rubriques.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Next](efficiently-paging-through-large-amounts-of-data-cs.md)
