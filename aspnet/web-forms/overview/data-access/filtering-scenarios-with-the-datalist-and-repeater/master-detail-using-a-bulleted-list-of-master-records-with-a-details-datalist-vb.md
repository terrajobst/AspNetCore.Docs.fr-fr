---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
title: Maître/détail à l’aide d’une liste à puces des enregistrements maîtres avec une DataList des détails (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel nous compressons le rapport de deux pages maître/détail du didacticiel précédent en une seule page affichant une liste à puces des noms de catégorie sur t...
ms.author: aspnetcontent
ms.date: 10/17/2006
ms.assetid: ee20742f-6fb7-49a0-a009-058fe363aacb
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 90629046869460869d6c85e179980811b7bede51
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841990"
---
<a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb"></a>Maître/détail à l’aide d’une liste à puces des enregistrements maîtres avec une DataList des détails (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_VB.exe) ou [télécharger le PDF](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/datatutorial35vb1.pdf)

> Dans ce didacticiel nous compressons le rapport de deux pages maître/détail du didacticiel précédent en une seule page affichant une liste à puces des noms de catégories sur le côté gauche de l’écran et les produits de la catégorie sélectionnée sur la droite de l’écran.


## <a name="introduction"></a>Introduction

Dans le [didacticiel précédent](master-detail-filtering-acess-two-pages-datalist-vb.md) nous avons vu comment séparer un rapport maître/détail sur deux pages. Dans la page maître, nous avons utilisé un contrôle Repeater pour afficher une liste à puces des catégories. Chaque nom de catégorie a été un lien hypertexte que, lorsque vous cliquez dessus, serait prennent l’utilisateur à la page de détails, où un contrôle DataList de deux colonnes vous a montré les produits appartenant à la catégorie sélectionnée.

Dans ce didacticiel nous compressons le didacticiel de deux pages en une seule page affichant une liste à puces des noms de catégories sur le côté gauche de l’écran avec chaque nom de catégorie restitué sous la forme d’un bouton de lien. Cliquant sur le nom de catégorie de type LinkButton induit une publication (postback) et lie les produits de s catégorie sélectionnée à un contrôle DataList de deux colonnes sur la droite de l’écran. Outre l’affichage de chaque nom de catégorie s, le Repeater sur la gauche montre nombre total de produits sont une catégorie donnée (voir Figure 1).


[![La catégorie s nom et le nombre Total de produits sont affichés sur la gauche](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image1.png)

**Figure 1**: la catégorie s nom et le nombre Total de produits sont affichés sur la gauche ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image3.png))


## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>Étape 1 : Afficher un répéteur dans la partie gauche de l’écran

Pour ce didacticiel, nous avons besoin d’afficher la liste à puces des catégories à gauche des produits s catégorie sélectionnée. Contenu dans une page web peut être positionnée à l’aide de balises HTML standard éléments paragraphe, les espaces insécables, `<table>` s et ainsi de suite ou via des techniques de feuille de style (CSS) en cascade. Tous nos didacticiels utilisaient jusqu’ici des techniques CSS pour le positionnement. Lorsque nous avons créé l’interface utilisateur de navigation dans notre page maître dans le [Pages maîtres et Navigation dans les sites](../introduction/master-pages-and-site-navigation-vb.md) didacticiel, nous avons utilisé *positionnement absolu*, indiquant le décalage de pixel précis pour la navigation liste et le contenu principal. Vous pouvez également les CSS peut être utilisées pour positionner un élément à droite ou gauche d’un autre via *flottante*. Nous pouvons afficher la liste à puces des catégories à gauche des produits s catégorie sélectionnée en flottante répéteur à gauche du contrôle DataList

Ouvrez le `CategoriesAndProducts.aspx` page à partir de la `DataListRepeaterFiltering` dossier et ajouter à la page un Repeater et DataList. Définir le Repeater s `ID` à `Categories` et le contrôle DataList s à `CategoryProducts`. Accédez à la vue de Source et placer les contrôles Repeater et DataList dans leurs propres `<div>` éléments. Autrement dit, placez le Repeater au sein d’un `<div>` élément premier et ensuite le contrôle DataList dans son propre `<div>` élément directement après le répéteur. Votre balisage à ce stade doit ressembler à ce qui suit :


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample1.aspx)]

Pour détacher le répéteur à gauche du contrôle DataList, nous devons utiliser le `float` attribut de style CSS, comme suit :


[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample2.html)]

Le `float: left;` flotte le premier `<div>` élément vers la gauche du second. Le `width` et `padding-right` paramètres indiquent la première `<div>` s `width` et la marge intérieure est ajoutée entre le `<div>` contenu de l’élément s et sa marge de droite. Pour plus d’informations sur flottante éléments en CSS extraire le [Floatutorial](http://css.maxdesign.com.au/floatutorial/).

Plutôt que de spécifier le paramètre de style directement par le biais de la première `<p>` élément s `style` d’attribut, permettent de s au lieu de cela créer une classe CSS dans `Styles.css` nommé `FloatLeft`:


[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample3.css)]

Puis nous pouvons remplacer le `<div>` avec `<div class="FloatLeft">`.

Après avoir ajouté la classe CSS et de configuration le balisage dans le `CategoriesAndProducts.aspx` page, accédez au concepteur. Vous devez voir le répéteur de virgule flottante à gauche du contrôle DataList (bien que le droit à la fois juste apparaissent maintenant comme cases grises depuis nous ve encore à configurer leurs sources de données ou des modèles).


[![Le contrôle Repeater est flotte à gauche du contrôle DataList](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image4.png)

**Figure 2**: le Repeater est flotte à gauche du contrôle DataList ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image6.png))


## <a name="step-2-determining-the-number-of-products-for-each-category"></a>Étape 2 : Déterminer le nombre de produits pour chaque catégorie

S Repeater et DataList entourant le balisage complet, nous vous êtes prêt pour lier les données de catégorie pour le répéteur de contrôler. Toutefois, comme le montre la liste à puces des catégories dans la Figure 1, en plus de chaque nom de s catégorie nous devons également afficher le nombre de produits de la catégorie. Pour accéder à ces informations, nous pouvons :

- **Déterminer ces informations à partir de la classe de code-behind de s de page ASP.NET.** Étant donné un particulier *`categoryID`* nous pouvons déterminer le nombre de produits associés en appelant le `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` (méthode). Cette méthode retourne un `ProductsDataTable` de l’objet dont la propriété `Count` propriété indique combien `ProductsRow` s existe, ce qui est le nombre de produits pour spécifié *`categoryID`*. Nous pouvons créer un `ItemDataBound` Gestionnaire d’événements pour le contrôle Repeater qui, pour chaque catégorie lié à la répétition, appelle le `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` (méthode) et inclut son nombre dans la sortie.
- **Mise à jour le `CategoriesDataTable` dans le DataSet typé à inclure un `NumberOfProducts` colonne.** Nous pouvons ensuite mettre à jour le `GetCategories()` méthode dans le `CategoriesDataTable` d’inclure ces informations, vous pouvez également `GetCategories()` en tant que-est et créer un nouveau `CategoriesDataTable` méthode appelée `GetCategoriesAndNumberOfProducts()`.

Permettent d’Explorer ces deux techniques s. La première approche est plus simple à implémenter, car nous n’avez pas besoin pour mettre à jour de la couche d’accès aux données ; Toutefois, elle nécessite plus de communications avec la base de données. L’appel à la `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` méthode dans le `ItemDataBound` Gestionnaire d’événements ajoute un appel de la base de données supplémentaires pour chaque catégorie affichée dans le répéteur. Avec cette technique, il existe *N* + appels de base de 1 données, où *N* est le nombre de catégories affichées dans le répéteur. Avec la deuxième approche, le nombre de produits est retourné avec les informations sur les catégories à partir de la `CategoriesBLL` classe s `GetCategories()` (ou `GetCategoriesAndNumberOfProducts()`) méthode, ce qui entraîne qu’un voyage à la base de données.

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>Détermination du nombre de produits dans le Gestionnaire d’événement ItemDataBound

Détermination du nombre de produits pour chaque catégorie dans le répéteur s `ItemDataBound` Gestionnaire d’événements ne nécessite pas de toute modification apportée à nos couche d’accès aux données existantes. Toutes les modifications peuvent être apportées directement dans le `CategoriesAndProducts.aspx` page. Commencez par ajouter un nouveau ObjectDataSource nommé `CategoriesDataSource` via la balise active Repeater s. Ensuite, configurez le `CategoriesDataSource` ObjectDataSource afin qu’il récupère ses données à partir de la `CategoriesBLL` classe s `GetCategories()` (méthode).


[![Configurer l’ObjectDataSource pour utiliser la classe CategoriesBLL s GetCategories() (méthode)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image7.png)

**Figure 3**: configurer ObjectDataSource à utiliser le `CategoriesBLL` classe s `GetCategories()` (méthode) ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image9.png))


Chaque élément dans le `Categories` Repeater doit être interactif et, lorsque vous cliquez dessus, provoquer la `CategoryProducts` DataList pour afficher ces produits pour la catégorie sélectionnée. Cela est possible en rendant chaque catégorie d’un lien hypertexte, liaison vers cette même page (`CategoriesAndProducts.aspx`), mais en passant le `CategoryID` via la chaîne de requête, tout comme nous l’avons vu dans le didacticiel précédent. L’avantage de cette approche est qu’une page affichant une produits de catégorie particulière s peut être marqué d’un signet et indexée par un moteur de recherche.

Ou bien, nous pouvons faire chaque catégorie LinkButton, qui est l’approche que nous allons utiliser pour ce didacticiel. Le LinkButton est rendu dans le navigateur de l’utilisateur s comme un lien hypertexte, mais, lorsque vous cliquez dessus, déclenche une publication (postback) ; lors de la publication, les opérations de mappage DataList ObjectDataSource doit être actualisé pour afficher les produits appartenant à la catégorie sélectionnée. Pour ce didacticiel, à l’aide d’un lien hypertexte plus judicieux que l’utilisation de LinkButton ; Toutefois, il peut être autres scénarios où il est plus avantageux d’à l’aide d’un bouton de lien. Tandis que l’approche de lien hypertexte serait idéal pour cet exemple, permettent de s au lieu de cela Explorer à l’aide du bouton de lien. Comme nous allons le voir, l’utilisation de LinkButton introduit des défis qui ne pourrait pas autrement se présenter avec un lien hypertexte. Par conséquent, à l’aide d’un LinkButton dans ce didacticiel sera mettez en surbrillance ces défis et aider à fournir des solutions pour les scénarios où nous souhaiterons peut-être utiliser un LinkButton au lieu d’un lien hypertexte.

> [!NOTE]
> Il est conseillé de répéter ce didacticiel à l’aide d’un contrôle de lien hypertexte ou `<a>` élément à la place le LinkButton.


Le balisage suivant montre la syntaxe déclarative pour le contrôle Repeater et ObjectDataSource. Notez que les modèles de s Repeater affichent une liste à puces avec chaque élément comme un LinkButton :


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample4.aspx)]

> [!NOTE]
> Pour ce didacticiel, le Repeater doit avoir son état d’affichage est activé (Notez l’omission de la `EnableViewState="False"` à partir de la syntaxe déclarative Repeater s). À l’étape 3 nous allons créer un gestionnaire d’événements pour le contrôle Repeater s `ItemCommand` événements dans laquelle nous mettrons à jour le contrôle DataList s ObjectDataSource s `SelectParameters` collection. Le Repeater s `ItemCommand`, toutefois, a gagné incendie de t si l’état d’affichage est désactivé. Consultez [embarrassante A une question de ASP.NET](http://scottonwriting.net/sowblog/posts/1263.aspx) et [sa solution](http://scottonwriting.net/sowBlog/posts/1268.aspx) pour plus d’informations sur la raison pour laquelle l’état d’affichage doit être activé pour un répéteur s `ItemCommand` l’événement.


Le LinkButton le `ID` valeur de propriété de `ViewCategory` n’a pas son `Text` jeu de propriétés. Si nous avions voulions seulement afficher le nom de catégorie, nous serait avez défini la propriété Text de façon déclarative, via la syntaxe de liaison de données, comme suit :


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample5.aspx)]

Toutefois, nous souhaitons afficher à la fois le nom de catégorie s *et* le nombre de produits appartenant à cette catégorie. Ces informations peuvent être extraites de répéteur s `ItemDataBound` Gestionnaire d’événements en effectuant un appel à la `ProductBLL` classe s `GetCategoriesByProductID(categoryID)` méthode et déterminer le nombre d’enregistrements est retourné dans le résultat `ProductsDataTable`, comme le code suivant illustre :


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample6.vb)]

Nous commençons en s’assurant que nous re fonctionne avec un élément de données (un dont `ItemType` est `Item` ou `AlternatingItem`), puis référencez la `CategoriesRow` instance qui a simplement été liée à l’actuel `RepeaterItem`. Ensuite, nous déterminons le nombre de produits pour cette catégorie en créant une instance de la `ProductsBLL` classe, appelant ses `GetCategoriesByProductID(categoryID)` méthode et déterminer le nombre d’enregistrements renvoyés à l’aide de la `Count` propriété. Enfin, le `ViewCategory` LinkButton dans ItemTemplate est références et son `Text` propriété est définie sur *CategoryName* (*NumberOfProductsInCategory*), où  *NumberOfProductsInCategory* est sous forme de nombre décimale.

> [!NOTE]
> Vous pouvez également nous aurions pu ajouter un *mise en forme de la fonction* à la classe code-behind de page s ASP.NET qui accepte une catégorie s `CategoryName` et `CategoryID` valeurs et retourne le `CategoryName` concaténé avec le nombre de produits de la catégorie (comme déterminé en appelant le `GetCategoriesByProductID(categoryID)` méthode). Les résultats d’une telle fonction mise en forme peut être affectées de façon déclarative à la s LinkButton propriété de texte en remplaçant le besoin du `ItemDataBound` Gestionnaire d’événements. Reportez-vous à la [à l’aide de TemplateFields dans le contrôle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) ou [mise en forme les contrôles DataList et Repeater sur données](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) didacticiels pour plus d’informations sur l’utilisation de fonctions de mise en forme.


Après avoir ajouté ce gestionnaire d’événements, prenez un moment pour tester la page via un navigateur. Notez la façon dont chaque catégorie est répertorié dans une liste à puces, affichant le nom de catégorie s et le nombre de produits de la catégorie (voir Figure 4).


[![Chaque nom de catégorie et le nombre de produits sont affichés.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image10.png)

**Figure 4**: s de chaque catégorie nom et le nombre de produits sont affichés ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image12.png))


## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>La mise à jour le`CategoriesDataTable`et`CategoriesTableAdapter`pour inclure le nombre de produits pour chaque catégorie

Au lieu de la détermination du nombre de produits pour chaque catégorie tel qu’il s lié à la répétition, nous pouvons simplifier ce processus en ajustant la `CategoriesDataTable` et `CategoriesTableAdapter` dans la couche d’accès aux données à inclure ces informations en mode natif. Pour ce faire, nous devons ajouter une nouvelle colonne à `CategoriesDataTable` pour contenir le nombre de produits associés. Pour ajouter une nouvelle colonne à un DataTable, ouvrez le DataSet typé (`App_Code\DAL\Northwind.xsd`), avec le bouton droit sur la table de données à modifier, puis sélectionnez Ajouter / colonne. Ajouter une nouvelle colonne à la `CategoriesDataTable` (voir Figure 5).


[![Ajouter une nouvelle colonne à la CategoriesDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image13.png)

**Figure 5**: ajouter une nouvelle colonne à la `CategoriesDataSource` ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image15.png))


Cela ajoutera une nouvelle colonne nommée `Column1`, que vous pouvez modifier en tapant simplement un nom différent. Renommer cette colonne `NumberOfProducts`. Ensuite, nous devons configurer les propriétés de cette colonne s. Cliquez sur la nouvelle colonne et accédez à la fenêtre Propriétés. Modifier la colonne s `DataType` propriété à partir de `System.String` à `System.Int32` et définir le `ReadOnly` propriété `True`, comme illustré dans la Figure 6.


![Définir le type de données et les propriétés en lecture seule de la nouvelle colonne](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image16.png)

**Figure 6**: définir le `DataType` et `ReadOnly` propriétés de la nouvelle colonne


Bien que le `CategoriesDataTable` a maintenant un `NumberOfProducts` colonne, sa valeur n’est pas définie par une des requêtes s TableAdapter correspondantes. Nous pouvons mettre à jour le `GetCategories()` retourné de méthode pour retourner ces informations si nous voulons que ces informations chaque fois les informations de catégorie sont récupérées. Si, toutefois, il nous suffit de saisir le nombre de produits associés pour les catégories dans de rares cas (par exemple, tout simplement pour ce didacticiel), nous pouvons laisser `GetCategories()` en tant que-est et créer une nouvelle méthode qui retourne cette information. S permettent d’utiliser cette dernière approche, création d’une nouvelle méthode nommée `GetCategoriesAndNumberOfProducts()`.

Pour ajouter de nouveaux `GetCategoriesAndNumberOfProducts()` (méthode), avec le bouton droit sur le `CategoriesTableAdapter` et choisissez Nouvelle requête. Cela permet d’afficher le TableAdapter interroger Assistant de Configuration, ce qui nous ve utilisé plusieurs fois dans les didacticiels précédents. Pour cette méthode, démarrez l’Assistant en indiquant que la requête utilise une instruction SQL ad hoc qui retourne des lignes.


[![Créer la méthode à l’aide d’une instruction SQL de Ad-Hoc](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image17.png)

**Figure 7**: créer la méthode à l’aide une instruction SQL de Ad-Hoc ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image19.png))


[![L’instruction SQL retourne des lignes](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image20.png)

**Figure 8**: retourne les instruction SQL les lignes ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image22.png))


L’écran suivant de l’Assistant nous invite pour la requête à utiliser. Pour retourner chaque catégorie s `CategoryID`, `CategoryName`, et `Description` champs, ainsi que le nombre de produits de la catégorie, utilisent la commande suivante `SELECT` instruction :


[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample7.sql)]


[![Spécifiez la requête à utiliser](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image23.png)

**Figure 9**: spécifiez la requête à utiliser ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image25.png))


Notez que la sous-requête qui calcule le nombre de produits de la catégorie a pour alias `NumberOfProducts`. Cette correspondance d’affectation de noms entraîne la valeur retournée par cette sous-requête à associer à la `CategoriesDataTable` s `NumberOfProducts` colonne.

Après avoir entré cette requête, la dernière étape consiste à choisir le nom de la nouvelle méthode. Utilisez `FillWithNumberOfProducts` et `GetCategoriesAndNumberOfProducts` pour le remplissage un DataTable et la retourner un DataTable des modèles, respectivement.


[![Nom de la nouvelle FillWithNumberOfProducts de méthodes de s TableAdapter et GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image26.png)

**Figure 10**: nommez le s TableAdapter de nouvelles méthodes `FillWithNumberOfProducts` et `GetCategoriesAndNumberOfProducts` ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image28.png))


À ce stade, la couche Data Access a été étendue pour inclure le nombre de produits par catégorie. Étant donné que tous nos couche de présentation achemine tous les appels à la couche DAL via une couche de logique métier distincts nous devons ajouter un correspondant `GetCategoriesAndNumberOfProducts` méthode à la `CategoriesBLL` classe :


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample8.vb)]

Avec la couche DAL et de la couche BLL terminée, nous re prêt pour lier ces données à la `Categories` Repeater dans `CategoriesAndProducts.aspx`! Si vous avez déjà créé déjà ObjectDataSource pour le répéteur du déterminer le nombre de produits dans le `ItemDataBound` section du Gestionnaire d’événements, supprimer ce ObjectDataSource et Repeater s `DataSourceID` propriété définition ; également ses le Repeater s `ItemDataBound` événement du Gestionnaire d’événements en supprimant le `Handles Categories.OnItemDataBound` syntaxe dans la classe code-behind ASP.NET.

Avec le contrôle Repeater dans son état d’origine, ajouter un nouveau ObjectDataSource nommé `CategoriesDataSource` via la balise active Repeater s. Configurer ObjectDataSource à utiliser le `CategoriesBLL` (classe), mais il lieu d’utiliser le `GetCategories()` (méthode), ont il utiliser `GetCategoriesAndNumberOfProducts()` à la place (voir Figure 11).


[![Configurer pour utiliser la méthode GetCategoriesAndNumberOfProducts ObjectDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image29.png)

**Figure 11**: configurer ObjectDataSource à utiliser le `GetCategoriesAndNumberOfProducts` (méthode) ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image31.png))


Ensuite, mettez à jour le `ItemTemplate` afin que les opérations de mappage LinkButton `Text` propriété est affectée façon déclarative à l’aide de la syntaxe de liaison de données et comprend à la fois le `CategoryName` et `NumberOfProducts` des champs de données. Le balisage déclaratif complet pour le contrôle Repeater et `CategoriesDataSource` ObjectDataSource suivi :


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample9.aspx)]

La sortie rendue en mettant à jour de la couche DAL pour inclure un `NumberOfProducts` colonne est identique à l’aide de la `ItemDataBound` approche de gestionnaire d’événements (vous référer à la Figure 4 pour voir un écran capture du répéteur montrant les noms de catégorie et le nombre de produits).

## <a name="step-3-displaying-the-selected-category-s-products"></a>Étape 3 : Afficher les produits de s catégorie sélectionnée

À ce stade, nous avons le `Categories` Repeater affichant la liste des catégories, ainsi que le nombre de produits dans chaque catégorie. Le Repeater utilise un LinkButton pour chaque catégorie que, lorsque vous cliquez dessus, entraîne une publication (postback), auquel point nous devons afficher ces produits pour la catégorie sélectionnée dans le `CategoryProducts` DataList.

Un défi de nous consiste à avoir le contrôle DataList afficher seulement ces produits pour la catégorie sélectionnée. Dans le [maître/détail utilisant un GridView maître avec un contrôle DetailsView détails](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) didacticiel, nous avons vu comment générer un GridView dont les lignes a pu être sélectionnée, avec la ligne sélectionnée s décrit en détail qui est affiché dans un contrôle DetailsView sur la même page. Les opérations de mappage GridView ObjectDataSource a retourné des informations concernant tous les produits à l’aide de la `ProductsBLL` s `GetProducts()` méthode lors de la s DetailsView ObjectDataSource récupéré des informations sur le produit sélectionné à l’aide la `GetProductsByProductID(productID)` (méthode). Le *`productID`* valeur du paramètre a été fournie de manière déclarative en l’associant à la valeur de la s GridView `SelectedValue` propriété. Malheureusement, le contrôle Repeater n’a pas un `SelectedValue` propriété et ne peut pas servir en tant que paramètre source.

> [!NOTE]
> Il s’agit d’un des défis qui apparaissent lorsque vous utilisez le LinkButton dans un répéteur. Si nous avions utilisé un lien hypertexte pour transmettre le `CategoryID` via la chaîne de requête au lieu de cela, nous pourrions utiliser ce champ de chaîne de requête comme source pour la valeur du paramètre s.


Avant de nous soucier de l’absence d’un `SelectedValue` propriété pour le contrôle Repeater, cependant, permettre de s tout d’abord lier le contrôle DataList à ObjectDataSource et spécifier ses `ItemTemplate`.

À partir de la balise active DataList s, choisir d’ajouter un nouveau ObjectDataSource nommé `CategoryProductsDataSource` et configurez-le pour utiliser le `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` (méthode). Étant donné que le contrôle DataList dans ce didacticiel offre une interface en lecture seule, n’hésitez pas à définir les listes déroulantes dans l’instruction INSERT, UPDATE et supprimer des onglets à (None).


[![Configurer pour utiliser la méthode de classe de ProductsBLL s GetProductsByCategoryID(categoryID) ObjectDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image32.png)

**Figure 12**: configurer ObjectDataSource à utiliser `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` (méthode) ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image34.png))


Dans la mesure où le `GetProductsByCategoryID(categoryID)` méthode attend un paramètre d’entrée (*`categoryID`*), l’Assistant Configurer la Source de données permet de spécifier la source du paramètre s. Les catégories avaient été répertoriés dans un GridView ou un contrôle DataList, nous d définition de la liste de liste déroulante de paramètre source au contrôle et le ControlID à la `ID` des données de contrôle Web. Toutefois, puisque le manque de répéteur un `SelectedValue` il ne peut pas être utilisé comme source de paramètre de propriété. Si vous activez, vous trouverez que la liste déroulante ControlID contienne uniquement un seul contrôle `ID``CategoryProducts`, le `ID` de DataList.

Pour l’instant, définition de la liste de liste déroulante de source de paramètre None. Nous allons se retrouvent assignation par programme cette valeur de paramètre lorsqu’une catégorie que LinkButton est activé dans le répéteur.


[![Faire pas spécifier une Source de paramètre pour la paramètre categoryID](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image35.png)

**Figure 13**: ne spécifiez pas d’une Source de paramètre pour le *`categoryID`* paramètre ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image37.png))


À l’issue de l’Assistant Configurer la Source de données, Visual Studio génère automatiquement le contrôle DataList s `ItemTemplate`. Remplacez cette valeur par défaut `ItemTemplate` avec le modèle nous utilisé dans le didacticiel précédent ; en outre, définissez le contrôle DataList s `RepeatColumns` propriété à 2. Après avoir apporté ces modifications le balisage déclaratif pour vos contrôles DataList et son ObjectDataSource associé doit ressembler à ce qui suit :


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample10.aspx)]

Actuellement, le `CategoryProductsDataSource` ObjectDataSource s *`categoryID`* paramètre n’est jamais défini, aucun produit n’est affichés lorsque vous affichez la page. Ce que nous devons faire est ont cette valeur de paramètre selon le `CategoryID` de la catégorie utilisateur a cliqué dessue dans le répéteur. Cela introduit deux défis : tout d’abord, comment nous déterminons quand un LinkButton dans le répéteur s `ItemTemplate` a été cliqué dessus ; et le deuxième comment nous déterminons le `CategoryID` de la catégorie correspondante, l’utilisateur a cliqué sur dont LinkButton ?

Le LinkButton comme les contrôles Button et ImageButton a un `Click` événement et un [ `Command` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx). Le `Click` événement est conçu pour Notez simplement que le LinkButton a été cliqué. Dans certains cas, toutefois, en plus de noter que le LinkButton a été cliqué nous devons également transmettre des informations supplémentaires au gestionnaire d’événements. Si c’est le cas, le LinkButton s [ `CommandName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx) et [ `CommandArgument` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx) propriétés peuvent être attribuées à ces informations supplémentaires. Ensuite, lorsque l’utilisateur clique sur le LinkButton, son `Command` se déclenche des événements (au lieu de son `Click` événement) et le Gestionnaire d’événements reçoit les valeurs de la `CommandName` et `CommandArgument` propriétés.

Lorsqu’un `Command` événement est déclenché à partir d’un modèle dans le répéteur, les opérations de mappage Repeater [ `ItemCommand` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx) se déclenche et est passé le `CommandName` et `CommandArgument` les valeurs de l’utilisateur a cliqué dessu LinkButton (ou bouton ou ImageButton). Par conséquent, pour déterminer quand une catégorie LinkButton dans le répéteur a été cliquée, nous devons effectuer les opérations suivantes :

1. Définir le `CommandName` propriété du contrôle LinkButton dans le répéteur s `ItemTemplate` à une valeur (Je ve utilisé ListProducts). En définissant ce paramètre `CommandName` valeur, le LinkButton s `Command` événement est déclenché lorsque l’utilisateur clique sur le LinkButton.
2. Définir le LinkButton s `CommandArgument` valeur à la propriété de l’élément actuel s `CategoryID`.
3. Créer un gestionnaire d’événements pour le contrôle Repeater s `ItemCommand` événement. Dans le gestionnaire, jeu d’événements le `CategoryProductsDataSource` ObjectDataSource s `CategoryID` paramètre à la valeur du passé en `CommandArgument`.

Ce qui suit `ItemTemplate` balisage pour le contrôle Repeater catégories implémente les étapes 1 et 2. Notez comment la `CommandArgument` est assignée à l’élément de données s `CategoryID` à l’aide de la syntaxe de liaison de données :


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample11.aspx)]

Chaque fois que la création d’un `ItemCommand` Gestionnaire d’événements qu’il est préférable de toujours à vérifier au préalable entrant `CommandName` valeur car *n’importe quel* `Command` événement déclenché par *n’importe quel* Button, LinkButton, ou ImageButton du Repeater entraîne le `ItemCommand` l’événement. Pendant que nous seulement disposons actuellement une telle LinkButton maintenant, dans le futur nous (ou un autre développeur de notre équipe) pourrais ajouter contrôles Web de bouton supplémentaires pour le contrôle Repeater qui, lorsque vous cliquez dessus, déclenche les mêmes `ItemCommand` Gestionnaire d’événements. Par conséquent, il est préférable de toujours s’assurer que vous vérifiez le `CommandName` propriété et poursuivre votre logique de programmation uniquement si elle correspond à la valeur attendue.

Après avoir vérifié que le passé dans `CommandName` ListProducts est égale à, le Gestionnaire d’événements puis assigne le `CategoryProductsDataSource` ObjectDataSource s `CategoryID` paramètre à la valeur du passé en `CommandArgument`. Cette modification de l’ObjectDataSource s `SelectParameters` entraîne automatiquement le contrôle DataList pour se relier à la source de données, montrant les produits pour la catégorie qui vient d’être sélectionnée.


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample12.vb)]

Avec ces ajouts, notre didacticiel est terminé ! Prenez un moment pour faire des tests dans un navigateur. Figure 14 illustre l’écran lors de la première visite la page. Dans la mesure où une catégorie doit encore être sélectionné, aucun produit n’est affichés. Cliquez sur une catégorie, c'est-à-dire les produits pour afficher ces produits dans la catégorie de produit dans une vue de deux colonnes (voir Figure 15).


[![Aucun produit n’est affichée lors de la première visite de la Page](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image38.png)

**Figure 14**: No produits sont affichées lors de la première visite de la Page ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image40.png))


[![En cliquant sur les listes de catégories produisent les produits correspondants à droite](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image41.png)

**Figure 15**: en cliquant sur la catégorie de produits répertorie les produits de mise en correspondance vers la droite ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image43.png))


## <a name="summary"></a>Récapitulatif

Comme nous l’avons vu dans ce didacticiel et précédente, les rapports maître/détail peuvent être répartis sur deux pages ou consolidées sur un. Pour afficher un rapport maître/détails sur une seule page, cependant, présente des défis sur la façon de mieux le maître de disposition et les enregistrements de détails sur la page. Dans le *maître/détail utilisant un GridView maître avec un contrôle DetailsView détails* didacticiel, nous avons dû les enregistrements de détails sont affichés ci-dessus les enregistrements maîtres ; dans ce didacticiel, nous avons utilisé les techniques CSS pour avoir la valeur float enregistrements principaux à le gauche des détails.

Ainsi que d’afficher les rapports maître/détails, il nous fallait également la possibilité de découvrir comment récupérer le nombre de produits associés à chaque catégorie, ainsi que la manière dont pour effectuer la logique côté serveur quand un LinkButton (ou bouton ou ImageButton) un clic sur depuis un Repeater.

Ce didacticiel terminé notre examen des rapports maître/détail avec les contrôles DataList et Repeater. Notre ensemble de didacticiels suivant illustre comment ajouter la modification et suppression de fonctionnalités pour le contrôle DataList.

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) un didacticiel sur flottante éléments CSS avec CSS
- [Positionnement CSS](http://www.brainjar.com/css/positioning/) plus d’informations sur le positionnement des éléments avec CSS
- [Disposition Out contenu avec HTML](http://www.w3schools.com/html/html_layout.asp) à l’aide de `<table>` s et autres éléments HTML de positionnement

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Zack Jones. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](master-detail-filtering-acess-two-pages-datalist-vb.md)
