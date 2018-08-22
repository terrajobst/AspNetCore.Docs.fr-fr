---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
title: Contrôles (VB) de Web de données imbriquées | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous explorerons comment utiliser un répéteur imbriquée dans une autre Repeater. Les exemples illustre comment remplir le Repeater interne à la fois d...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 8b7fcf7b-722b-498d-a4e4-7c93701e0c95
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 297d76da5bf049ec68a351562f96f3587b059b55
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833192"
---
<a name="nested-data-web-controls-vb"></a>Contrôles Web de données imbriquées (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_VB.exe) ou [télécharger le PDF](nested-data-web-controls-vb/_static/datatutorial32vb1.pdf)

> Dans ce didacticiel, nous explorerons comment utiliser un répéteur imbriquée dans une autre Repeater. Les exemples illustre comment remplir le Repeater interne de façon déclarative et par programme.


## <a name="introduction"></a>Introduction

En plus de code HTML statique et la syntaxe de liaison de données, les modèles peuvent également inclure les contrôles Web et contrôles utilisateur. Ces contrôles Web peuvent avoir leurs propriétés affectées via la syntaxe de liaison de données déclarative, ou sont accessibles par programmation dans les gestionnaires d’événements côté serveur approprié.

En incorporant des contrôles dans un modèle, vous pouvez personnaliser l’apparence et l’expérience utilisateur et l’améliorée. Par exemple, dans le [à l’aide de TemplateFields dans le contrôle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) didacticiel, nous avons vu comment personnaliser l’affichage de s GridView en ajoutant un contrôle calendrier dans TemplateField pour afficher un employé à la date d’embauche s ; dans le [Ajout Contrôles de validation pour l’édition et insertion des Interfaces](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) et [personnalisation de l’Interface de Modification de données](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) didacticiels, nous avons vu comment personnaliser l’édition et insertion interfaces en ajoutant la validation contrôles, zones de texte, DropDownList et d’autres contrôles Web.

Modèles peuvent également contenir d’autres contrôles Web de données. Autrement dit, nous pouvons avoir un contrôle DataList qui contient un autre contrôle DataList (ou Repeater ou GridView ou DetailsView et ainsi de suite) au sein de ses modèles. Le défi lié à une telle interface est lié les données appropriées pour les contrôle Web de données internes. Il existe différentes approches, allant d’options déclaratives à l’aide de l’ObjectDataSource par programmation à celles.

Dans ce didacticiel, nous explorerons comment utiliser un répéteur imbriquée dans une autre Repeater. Le Repeater externe contient un élément pour chaque catégorie dans la base de données, affichant le nom de catégorie s et la description. Chaque élément de catégorie s Repeater interne affiche les informations concernant chaque produit appartenant à cette catégorie (voir Figure 1) dans une liste à puces. Nos exemples illustre comment remplir le Repeater interne de façon déclarative et par programme.


[![Chaque catégorie, ainsi que ses produits, sont répertoriés.](nested-data-web-controls-vb/_static/image2.png)](nested-data-web-controls-vb/_static/image1.png)

**Figure 1**: chaque catégorie, ainsi que ses produits, sont répertoriés ([cliquez pour afficher l’image en taille réelle](nested-data-web-controls-vb/_static/image3.png))


## <a name="step-1-creating-the-category-listing"></a>Étape 1 : Création de la liste des catégories

Lors de la création d’une page qui utilise des contrôles de Web de données imbriqués, I s’avérer utile pour concevoir, créer et tester le contrôle Web de données extérieur tout d’abord, sans vous soucier de même contrôle imbriqué interne. Par conséquent, permettent de s démarrer en passant en revue les étapes nécessaires pour ajouter un répéteur à la page qui répertorie le nom et la description pour chaque catégorie.

Commencez par ouvrir le `NestedControls.aspx` page dans le `DataListRepeaterBasics` dossier et ajouter un contrôle Repeater à la page, en définissant son `ID` propriété `CategoryList`. À partir de la balise active Repeater s, choisissez de créer un nouveau ObjectDataSource nommé `CategoriesDataSource`.


[![Nom de la nouvelle ObjectDataSource CategoriesDataSource](nested-data-web-controls-vb/_static/image5.png)](nested-data-web-controls-vb/_static/image4.png)

**Figure 2**: nommez le nouveau ObjectDataSource `CategoriesDataSource` ([cliquez pour afficher l’image en taille réelle](nested-data-web-controls-vb/_static/image6.png))


Configurer l’ObjectDataSource afin qu’il extrait ses données à partir de la `CategoriesBLL` classe s `GetCategories` (méthode).


[![Configurer pour utiliser la classe CategoriesBLL s méthode GetCategories ObjectDataSource](nested-data-web-controls-vb/_static/image8.png)](nested-data-web-controls-vb/_static/image7.png)

**Figure 3**: configurer ObjectDataSource à utiliser le `CategoriesBLL` classe s `GetCategories` (méthode) ([cliquez pour afficher l’image en taille réelle](nested-data-web-controls-vb/_static/image9.png))


Pour spécifier le modèle de s Repeater contenu nous avons besoin accéder à la vue de Source et d’entrer manuellement la syntaxe déclarative. Ajouter un `ItemTemplate` qui affiche le nom de catégorie s dans un `<h4>` élément et la description de la catégorie s dans un élément de paragraphe (`<p>`). En outre, s permettent de séparer chaque catégorie avec une règle horizontale (`<hr>`). Après avoir apporté ces modifications votre page doit contenir la syntaxe déclarative pour le Repeater et ObjectDataSource est similaire à ce qui suit :


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample1.aspx)]

Figure 4 illustre notre progression lorsqu’ils sont affichés via un navigateur.


[![Chaque nom de catégorie et la Description sont répertorié, séparés par une barre horizontale](nested-data-web-controls-vb/_static/image11.png)](nested-data-web-controls-vb/_static/image10.png)

**Figure 4**: s de chaque catégorie nom et la Description est répertorié, séparés par une barre horizontale ([cliquez pour afficher l’image en taille réelle](nested-data-web-controls-vb/_static/image12.png))


## <a name="step-2-adding-the-nested-product-repeater"></a>Étape 2 : Ajout de répéteur imbriquées produit

Avec la liste complète des catégories, la tâche suivante consiste à ajouter un répéteur à la `CategoryList` s `ItemTemplate` qui affiche des informations sur les produits appartenant à la catégorie appropriée. Il existe plusieurs façons, que nous pouvons récupérer les données pour ce Repeater interne, dont deux, nous allons Explorer peu de temps. Pour l’instant, laissez s créer simplement les produits Repeater dans le `CategoryList` Repeater s `ItemTemplate`. Plus précisément, vous autorise à s pour disposer de l’affichage Repeater chaque produit dans une liste à puces avec chaque élément de liste y compris le nom de produit s et le prix du produit.

Pour créer ce Repeater que nous avons besoin d’entrer manuellement la syntaxe déclarative du Repeater s interne et les modèles dans le `CategoryList` s `ItemTemplate`. Ajoutez le balisage suivant dans le `CategoryList` Repeater s `ItemTemplate`:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>Étape 3 : Les produits spécifiques à la catégorie de liaison pour le contrôle Repeater ProductsByCategoryList

Si vous visitez la page via un navigateur à ce stade, votre écran ont le même aspect que dans la Figure 4, car nous ve encore à lier des données pour le contrôle Repeater. Il existe quelques façons que nous pouvons récupérer les enregistrements produit approprié et les lier à la répétition, certaines plus efficace que d’autres. Le défi principal ici Obtient les produits appropriés pour la catégorie spécifiée.

Les données à lier au contrôle Repeater interne soit sont accessibles de manière déclarative, via un ObjectDataSource dans le `CategoryList` Repeater s `ItemTemplate`, ou par programmation, à partir de la page de code-behind s de page ASP.NET. De même, ces données peuvent être liées au Repeater interne soit de manière déclarative - via le Repeater interne s `DataSourceID` propriété ou via la syntaxe de liaison de données déclarative ou par programmation en référençant le Repeater interne dans le `CategoryList` Repeater s `ItemDataBound` Gestionnaire d’événements, définition par programmation son `DataSource` propriété et en appelant son `DataBind()` (méthode). Permettent d’Explorer chacune de ces approches s.

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>Accès aux données de façon déclarative avec un contrôle ObjectDataSource et`ItemDataBound`Gestionnaire d’événements

Dans la mesure où ve utilisé ObjectDataSource largement tout au long de cette série de didacticiels, le choix plus naturel pour accéder à des données pour cet exemple se cantonner à ObjectDataSource. Le `ProductsBLL` classe a un `GetProductsByCategoryID(categoryID)` méthode qui retourne des informations sur les produits qui appartiennent à spécifié *`categoryID`*. Par conséquent, nous pouvons ajouter un ObjectDataSource pour le `CategoryList` Repeater s `ItemTemplate` et configurez-le pour accéder à ses données à partir de cette méthode de classe s.

Malheureusement, Repeater n t permet à ses modèles d’être modifiée via la vue de conception afin que nous devons ajouter la syntaxe déclarative pour ce contrôle ObjectDataSource manuellement. La syntaxe suivante indique le `CategoryList` Repeater s `ItemTemplate` après avoir ajouté cette nouvelle ObjectDataSource (`ProductsByCategoryDataSource`) :


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample3.aspx)]

Lorsque vous utilisez l’approche ObjectDataSource nous devons définir la `ProductsByCategoryList` Repeater s `DataSourceID` propriété le `ID` de ObjectDataSource (`ProductsByCategoryDataSource`). Remarquez également que notre ObjectDataSource a un `<asp:Parameter>` élément qui spécifie le *`categoryID`* valeur qui sera passé dans le `GetProductsByCategoryID(categoryID)` (méthode). Mais comment spécifier cette valeur ? Dans l’idéal, nous d être en mesure de définir uniquement les `DefaultValue` propriété de la `<asp:Parameter>` élément à l’aide de la syntaxe de liaison de données, comme suit :


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample4.aspx)]

Malheureusement, syntaxe de liaison de données est uniquement valide dans les contrôles qui ont un `DataBinding` événement. Le `Parameter` classe ne dispose pas d’un tel événement, et par conséquent, la syntaxe ci-dessus est non conforme et entraîne une erreur d’exécution.

Pour définir cette valeur, nous devons créer un gestionnaire d’événements pour le `CategoryList` Repeater s `ItemDataBound` événement. N’oubliez pas que le `ItemDataBound` événement est déclenché une fois pour chaque élément lié pour le contrôle Repeater. Par conséquent, chaque fois que cet événement se déclenche pour le contrôle Repeater externe nous pouvons attribuer actuel `CategoryID` valeur pour le `ProductsByCategoryDataSource` ObjectDataSource s `CategoryID` paramètre.

Créer un gestionnaire d’événements pour le `CategoryList` Repeater s `ItemDataBound` événement avec le code suivant :


[!code-vb[Main](nested-data-web-controls-vb/samples/sample5.vb)]

Ce gestionnaire d’événements démarre en veillant à ce que nous re traitement comportant des données d’élément au lieu de l’élément d’en-tête, pied de page ou séparateur. Ensuite, nous faisons référence réelle `CategoriesRow` instance qui a simplement été liée à l’actuel `RepeaterItem`. Enfin, nous faisons référence ObjectDataSource dans le `ItemTemplate` et affecter ses `CategoryID` valeur de paramètre à la `CategoryID` d’actuel `RepeaterItem`.

Avec ce gestionnaire d’événements, le `ProductsByCategoryList` Repeater dans chaque `RepeaterItem` est lié à ces produits dans le `RepeaterItem` catégorie de s. La figure 5 illustre une capture d’écran de la sortie résultante.


[![Le Repeater externe répertorie chaque catégorie ; Celui interne répertorie les produits pour cette catégorie](nested-data-web-controls-vb/_static/image14.png)](nested-data-web-controls-vb/_static/image13.png)

**Figure 5**: le Repeater répertorie chaque catégorie externe ; les listes d’un seul interne les produits pour cette catégorie ([cliquez pour afficher l’image en taille réelle](nested-data-web-controls-vb/_static/image15.png))


## <a name="accessing-the-products-by-category-data-programmatically"></a>Accès par programme aux produits par catégorie des données

Au lieu d’utiliser un ObjectDataSource pour récupérer les produits pour la catégorie actuelle, nous pouvons créer une méthode dans notre classe code-behind de pages ASP.NET (ou dans le `App_Code` dossier ou dans un projet de bibliothèque de classes distinct) qui retourne l’ensemble approprié de produits quand il est passé dans un `CategoryID`. Imaginez que nous avions une telle méthode dans notre classe code-behind de pages ASP.NET et qu’il a été nommé `GetProductsInCategory(categoryID)`. Avec cette méthode en place nous pouvons lier les produits pour la catégorie actuelle au Repeater interne à l’aide de la syntaxe déclarative suivante :


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample6.aspx)]

Le Repeater s `DataSource` propriété utilise la syntaxe de liaison de données pour indiquer que ses données provient de la `GetProductsInCategory(categoryID)` (méthode). Dans la mesure où `Eval("CategoryID")` retourne une valeur de type `Object`, nous effectuer un cast de l’objet à un `Integer` avant de le transmettre dans le `GetProductsInCategory(categoryID)` (méthode). Notez que le `CategoryID` accessibles via la liaison de données syntaxe Voici le `CategoryID` dans le *externe* Repeater (`CategoryList`), celui que s liés aux enregistrements dans la `Categories` table. Par conséquent, nous savons que `CategoryID` ne peut pas être une base de données `NULL` valeur, c’est pourquoi nous pouvons convertir aveuglément le `Eval` méthode sans vérifier si nous re traiter avec un `DBNull`.

Avec cette approche, nous devons créer la `GetProductsInCategory(categoryID)` (méthode) et de sorte qu’elle récupère l’ensemble approprié de produits donné fourni *`categoryID`*. Pour cela, nous pouvons simplement retourner la `ProductsDataTable` retourné par la `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` (méthode). S permettent de créer le `GetProductsInCategory(categoryID)` méthode dans la classe code-behind pour notre `NestedControls.aspx` page. Pour cela, en utilisant le code suivant :


[!code-vb[Main](nested-data-web-controls-vb/samples/sample7.vb)]

Cette méthode crée simplement une instance de la `ProductsBLL` méthode et retourne les résultats de la `GetProductsByCategoryID(categoryID)` (méthode). Notez que la méthode doit être marquée `Public` ou `Protected`; si la méthode est marquée `Private`, il ne sera pas accessible à partir du balisage déclaratif de s de page ASP.NET.

Après avoir apporté ces modifications à utiliser cette technique de nouveau, prenez un moment pour afficher la page via un navigateur. La sortie doit être identique à la sortie lorsque vous utilisez ObjectDataSource et `ItemDataBound` approche de gestionnaire d’événements (voir la Figure 5 pour voir un écran capture).

> [!NOTE]
> Il peut sembler busywork pour créer le `GetProductsInCategory(categoryID)` méthode dans la classe code-behind de s de page ASP.NET. Après tout, cette méthode crée simplement une instance de la `ProductsBLL` classe et retourne les résultats de ses `GetProductsByCategoryID(categoryID)` (méthode). Pourquoi ne pas simplement appeler cette méthode directement à partir de la syntaxe de liaison de données dans le répéteur interne, telles que : `DataSource='<%# ProductsBLL.GetProductsByCategoryID(CType(Eval("CategoryID"), Integer)) %>'`? Bien que cette syntaxe a gagné travail t avec notre implémentation actuelle de la `ProductsBLL` classe (dans la mesure où le `GetProductsByCategoryID(categoryID)` méthode est une méthode d’instance), vous pourriez modifier `ProductsBLL` à inclure statique `GetProductsByCategoryID(categoryID)` méthode ou inclure dans la classe statique `Instance()` méthode pour retourner une nouvelle instance de la `ProductsBLL` classe.


Bien que ces modifications élimine la nécessité pour le `GetProductsInCategory(categoryID)` méthode dans la classe code-behind de pages ASP.NET, la méthode de classe code-behind nous donne davantage de flexibilité dans l’utilisation avec les données récupérées, comme nous allons le voir sous peu.

## <a name="retrieving-all-of-the-product-information-at-once"></a>Récupération de toutes les informations de produit à la fois

Les deux techniques précédente nous ve examiné saisir ces produits pour la catégorie actuelle en effectuant un appel à la `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` (méthode) (la première approche fait par le biais d’ObjectDataSource, le deuxième, troisième et le `GetProductsInCategory(categoryID)` méthode dans le classe code-behind). Chaque fois que cette méthode est appelée, les appels de la couche de logique métier à la couche d’accès aux données, qui interroge la base de données avec une instruction SQL qui retourne des lignes à partir de la `Products` table dont `CategoryID` champ correspond au paramètre d’entrée fourni.

Étant donné *N* filets de catégories dans le système, cette approche *N* + 1 appelle à la requête d’une base de données de base de données pour obtenir toutes les catégories, puis *N* appelle pour obtenir les produits spécifique à chaque catégorie. Nous pouvons, toutefois, récupérer toutes les données nécessaires en seulement deux bases de données appelle un appel pour obtenir toutes les catégories et l’autre pour obtenir tous les produits. Une fois que nous avons tous les produits, nous pouvons filtrer ces produits ainsi que seuls les produits en cours de mise en correspondance `CategoryID` sont liés à cette catégorie s Repeater interne.

Pour fournir cette fonctionnalité, nous avons besoin uniquement apportez une légère modification à la `GetProductsInCategory(categoryID)` méthode dans notre classe code-behind de pages ASP.NET. Plutôt qu’aveuglément retournant les résultats de la `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` (méthode), nous pouvons à la place d’abord accéder au *tous les* des produits (si elles réalisées t été accessibles déjà), puis retourner simplement la vue filtrée de la produits selon le passé dans `CategoryID`.


[!code-vb[Main](nested-data-web-controls-vb/samples/sample8.vb)]

Notez l’ajout de la variable au niveau des pages, `allProducts`. Il contient des informations sur tous les produits et est rempli la première fois le `GetProductsInCategory(categoryID)` méthode est appelée. Après avoir vérifié que le `allProducts` objet a été créé et rempli, la méthode filtre les résultats de s de DataTable de telle sorte que seules les lignes dont la propriété `CategoryID` correspond à spécifié `CategoryID` sont accessibles. Cette approche réduit le nombre de fois où la base de données est accessible à partir de *N* + 1 jusqu'à deux.

Cette amélioration n’introduit pas de toute modification apportée dans le balisage rendu de la page, ni qu’il apporte retour moins d’enregistrements que l’autre approche. Simplement, il réduit le nombre d’appels à la base de données.

> [!NOTE]
> Une raison quelconque, peut intuitivement que ce qui réduit le nombre d’accès de base de données sans faute améliorera les performances. Toutefois, cela ne peut pas être le cas. Si vous avez un grand nombre de produits dont `CategoryID` est `NULL`, par exemple, puis l’appel à la `GetProducts` méthode retourne un nombre de produits qui ne sont jamais affichées. En outre, le retour de tous les produits peut être inutile si vous re affiche uniquement un sous-ensemble des catégories, qui peut être le cas si vous avez implémenté la pagination.


Comme toujours, lorsqu’il s’agit de l’analyse des performances des deux techniques, la mesure uniquement surefire consiste à exécuter des tests contrôlés adaptées à vos scénarios de cas courants de s d’application.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons vu comment imbriquer des données d’un contrôle Web dans une autre, examinant en particulier comment avoir un répéteur externe à afficher un élément pour chaque catégorie avec un répéteur interne répertoriant les produits pour chaque catégorie dans une liste à puces. Le défi principal dans la création d’une interface utilisateur imbriquée réside dans l’accès et la liaison des données correctes pour le contrôle Web de données interne. Il existe une variété de techniques disponibles, que parmi lesquels nous avons examiné dans ce didacticiel. La première approche examinée utilisé un ObjectDataSource dans les contrôle Web s de données externes `ItemTemplate` qui a été lié au contrôle Web interne de données via ses `DataSourceID` propriété. La deuxième technique accédé aux données via une méthode dans la classe de code-behind de s de page ASP.NET. Cette méthode peut ensuite être liée aux données internes contrôle Web s `DataSource` propriété via la syntaxe de liaison de données.

Bien que l’interface utilisateur imbriqués examinée dans ce didacticiel utilisé un répéteur imbriqué dans un répéteur, ces techniques peuvent être étendues pour les autres contrôles Web de données. Vous pouvez imbriquer un répéteur au sein d’un GridView ou un GridView au sein d’un contrôle DataList et ainsi de suite.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Zack Jones et Liz Shulok. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
