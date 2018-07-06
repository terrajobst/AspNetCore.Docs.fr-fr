---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
title: Création d’une Interface d’utilisateur de tri personnalisée (VB) | Microsoft Docs
author: rick-anderson
description: Lors de l’affichage d’une longue liste des données triées, il peut être très utile de regrouper les données associées en introduisant des lignes du séparateur. Dans ce didacticiel, nous verrons comment cre...
ms.author: aspnetcontent
ms.date: 08/15/2006
ms.assetid: f3897a74-cc6a-4032-8f68-465f155e296a
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 8e94bbda0ca89b409515db1e223637e3a554cd31
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835866"
---
<a name="creating-a-customized-sorting-user-interface-vb"></a>Création d’une Interface d’utilisateur de tri personnalisée (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_VB.exe) ou [télécharger le PDF](creating-a-customized-sorting-user-interface-vb/_static/datatutorial27vb1.pdf)

> Lors de l’affichage d’une longue liste des données triées, il peut être très utile de regrouper les données associées en introduisant des lignes du séparateur. Dans ce didacticiel, nous allons voir comment créer une interface utilisateur tri.


## <a name="introduction"></a>Introduction

Affichage d’une longue liste de lorsque les données triées contenant uniquement un certain nombre de valeurs différentes dans la colonne triée, un utilisateur final peut s’avérer difficile de discerner où, exactement, les limites de différence se produisent. Par exemple, il existe des 81 produits dans la base de données, mais les choix de catégorie différente seulement neuf (huit catégories uniques ainsi que les `NULL` option). Prenons le cas d’un utilisateur souhaitant en examinant les produits qui relèvent de la catégorie de la mer. À partir d’une page qui répertorie *tous les* des produits dans un GridView unique, l’utilisateur peut décider de son meilleur résultat consiste à trier les résultats par catégorie, qui regroupe tous les produits Seafood ensemble. Après le tri par catégorie, l’utilisateur doit ensuite de recherche dans la liste, recherchez où les produits regroupés en mer commencer et se terminer. Dans la mesure où les résultats sont triés par ordre alphabétique par nom de la catégorie recherche les produits de la mer n’est pas difficile, mais elle nécessite toujours étroitement analyse la liste des éléments dans la grille.

Pour aider à mettre en surbrillance les limites entre les groupes triés, de nombreux sites Web utilisent une interface utilisateur qui ajoute un séparateur entre ces groupes. Séparateurs comme celles indiquées dans la Figure 1 permet à un utilisateur de plus rapidement trouver un groupe particulier et identifier ses limites, ainsi que déterminer quels groupes distincts existent dans les données.


[![Chaque groupe de catégories est clairement identifié](creating-a-customized-sorting-user-interface-vb/_static/image2.png)](creating-a-customized-sorting-user-interface-vb/_static/image1.png)

**Figure 1**: chaque groupe de catégories est clairement identifié ([cliquez pour afficher l’image en taille réelle](creating-a-customized-sorting-user-interface-vb/_static/image3.png))


Dans ce didacticiel, nous allons voir comment créer une interface utilisateur tri.

## <a name="step-1-creating-a-standard-sortable-gridview"></a>Étape 1 : Création d’un GridView Standard, pouvant être trié

Avant de nous explorons comment augmenter le contrôle GridView pour fournir l’interface de tri améliorée, permettent de s tout d’abord créer un GridView standard, pouvant être trié qui répertorie les produits. Commencez par ouvrir le `CustomSortingUI.aspx` page dans le `PagingAndSorting` dossier. Ajouter un GridView à la page, définissez son `ID` propriété `ProductList`et le lier à un nouveau ObjectDataSource. Configurer l’ObjectDataSource à utiliser le `ProductsBLL` classe s `GetProducts()` méthode de sélection des enregistrements.

Ensuite, configurez le contrôle GridView tel qu’il contient uniquement le `ProductName`, `CategoryName`, `SupplierName`, et `UnitPrice` BoundFields et le CheckBoxField abandonné. Enfin, configurez le contrôle GridView pour prendre en charge le tri en cochant la case à cocher Activer le tri dans la balise active de GridView s (ou en définissant son `AllowSorting` propriété `true`). Après avoir apporté ces ajouts à la `CustomSortingUI.aspx` page, le balisage déclaratif doit ressembler à ce qui suit :


[!code-aspx[Main](creating-a-customized-sorting-user-interface-vb/samples/sample1.aspx)]

Prenez un moment pour consulter notre progression jusqu'à présent dans un navigateur. Figure 2 montre le contrôle GridView sortable lorsque ses données sont triées par catégorie dans l’ordre alphabétique.


[![Les opérations de mappage GridView pouvant être trié les données sont triées par catégorie](creating-a-customized-sorting-user-interface-vb/_static/image5.png)](creating-a-customized-sorting-user-interface-vb/_static/image4.png)

**Figure 2**: s GridView pouvant être trié les données sont triées par catégorie ([cliquez pour afficher l’image en taille réelle](creating-a-customized-sorting-user-interface-vb/_static/image6.png))


## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>Étape 2 : Exploration des Techniques permettant d’ajouter les lignes du séparateur

Avec GridView générique, pouvant être trié complète, reste qu’à être en mesure d’ajouter les lignes du séparateur dans le contrôle GridView avant chaque groupe trié. Mais comment ces lignes possible d’injecter dans le contrôle GridView ? Pour l’essentiel, nous devons effectuer une itération dans les lignes s GridView, déterminer où les différences entre les valeurs dans la colonne triée et puis ajoutez la ligne de séparateur approprié. Lorsque vous réfléchissez à ce problème, cela semble naturel que la solution se trouve quelque part dans le s GridView `RowDataBound` Gestionnaire d’événements. Comme expliqué dans la [mise en forme en fonction lors de données personnalisées](../custom-formatting/custom-formatting-based-upon-data-vb.md) didacticiel, ce gestionnaire d’événements est couramment utilisé lors de la mise en forme au niveau des lignes selon les données de ligne s. Toutefois, le `RowDataBound` Gestionnaire d’événements n’est pas la solution ici, comme les lignes ne peuvent pas être ajoutés au GridView par programmation à partir de ce gestionnaire d’événements. Les opérations de mappage GridView `Rows` collection, en fait, est en lecture seule.

Pour ajouter des lignes supplémentaires au GridView, nous avons trois possibilités :

- Ajoutez ces lignes de séparateur de métadonnées aux données réelles qui sont liées au GridView
- Une fois que le contrôle GridView a été lié aux données, ajoutez d’autres `TableRow` contrôlent la collecte des instances pour les opérations de mappage GridView
- Créer un contrôle serveur personnalisé qui étend le contrôle GridView et remplace ces méthodes responsables du développement de la structure de s GridView

Création d’un contrôle serveur personnalisé serait la meilleure approche si cette fonctionnalité était nécessaire sur de nombreuses pages web ou sur plusieurs sites Web. Toutefois, elle entraînerait un peu de code et une exploration approfondie dans les profondeurs des mécanismes internes s GridView. Par conséquent, nous étudierons pas cette option pour ce didacticiel.

Les deux autres options d’ajout d’une ligne séparateur pour les données réelles en cours liées au GridView et la manipulation de la collection de contrôles GridView s après son été lié - attaquer le problème différemment et méritent une discussion.

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>Ajout de lignes aux données liées au GridView

Lorsque le contrôle GridView est lié à une source de données, il crée un `GridViewRow` pour chaque enregistrement retourné par la source de données. Par conséquent, nous pouvons injecter les lignes du séparateur nécessaires en ajoutant des enregistrements de séparateur à la source de données avant de le lier au contrôle GridView. La figure 3 illustre ce concept.


![Une Technique consiste à ajouter des lignes de séparateur à la Source de données](creating-a-customized-sorting-user-interface-vb/_static/image7.png)

**Figure 3**: une Technique implique l’ajout de lignes de séparateur à la Source de données


Utiliser les enregistrements de séparateur à terme entre guillemets, car il n’existe aucun enregistrement de séparation particulière ; au lieu de cela, nous devons d’une certaine manière indicateur servant à un enregistrement particulier dans la source de données comme un séparateur au lieu d’une ligne de données normale. Pour nos exemples, nous re liaison un `ProductsDataTable` instance au GridView, qui se compose de `ProductRows`. Nous pouvons également marquer un enregistrement en tant que ligne de séparateur en définissant son `CategoryID` propriété `-1` (étant donné que ce type t n’est pas parvenu valeur existe normalement).

Pour utiliser cette technique d nous devez effectuer les étapes suivantes :

1. Récupérer par programme les données à lier au contrôle GridView (un `ProductsDataTable` instance)
2. Trier les données selon le s GridView `SortExpression` et `SortDirection` propriétés
3. Effectuer une itération dans le `ProductsRows` dans le `ProductsDataTable`, cherchez où se situent les différences dans la colonne triée
4. À chaque limite de groupe, injecter un enregistrement de séparateur `ProductsRow` instance dans le DataTable, ce qui l’a s `CategoryID` la valeur `-1` (ou toute désignation a été décidée pour marquer un enregistrement comme enregistrement séparateur)
5. Après l’injection les lignes du séparateur, lier par programmation les données au GridView

Outre ces cinq étapes, d nous devons également fournir un gestionnaire d’événements pour les opérations de mappage GridView `RowDataBound` événement. Ici, nous d vérifier chacune `DataRow` et déterminer s’il concernait un séparateur de ligne, un dont `CategoryID` paramètre était `-1`. Dans ce cas, nous d souhaiterez probablement ajuster sa mise en forme ou le texte affiché dans l’ou les cellules.

À l’aide de cette technique pour injecter les tri des limites des groupes nécessite un peu plus de travail qu’indiqué ci-dessus, que vous devez également fournir un gestionnaire d’événements pour les opérations de mappage GridView `Sorting` effectuer le suivi de l’événement et conserver de la `SortExpression` et `SortDirection` valeurs.

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>Manipulation de la s GridView contrôler la collecte après lui s été lié aux données

Plutôt que les données de messagerie avant de le lier au contrôle GridView, nous pouvons ajouter les lignes du séparateur *après* l’a été lié aux données au GridView. Le processus de liaison de données crée la hiérarchie des contrôles GridView s, qui, en réalité, est simplement un `Table` instance composées d’une collection de lignes, chacune d’elles est composé d’une collection de cellules. Plus précisément, la collection de contrôles GridView s contient un `Table` objet racine, un `GridViewRow` (qui est dérivée de la `TableRow` classe) pour chaque enregistrement dans le `DataSource` lié au GridView et un `TableCell` objet dans chaque `GridViewRow` instance pour chaque champ de données dans le `DataSource`.

Pour ajouter des lignes du séparateur entre chaque groupe de tri, nous pouvons manipuler directement cette hiérarchie de contrôle une fois qu’il a été créé. Nous pouvons être certain que la hiérarchie des contrôles GridView s a été créée pour la dernière fois au moment que du rendu de la page. Par conséquent, cette approche remplace le `Page` classe s `Render` méthode, à quel point la hiérarchie des contrôles finale s GridView est mis à jour pour inclure les lignes du séparateur nécessaires. Figure 4 illustre ce processus.


[![Une autre Technique manipule la hiérarchie des contrôles GridView s](creating-a-customized-sorting-user-interface-vb/_static/image9.png)](creating-a-customized-sorting-user-interface-vb/_static/image8.png)

**Figure 4**: une autre Technique manipule la hiérarchie des contrôles de s GridView ([cliquez pour afficher l’image en taille réelle](creating-a-customized-sorting-user-interface-vb/_static/image10.png))


Pour ce didacticiel, nous allons utiliser cette dernière approche pour personnaliser l’expérience utilisateur de tri.

> [!NOTE]
> Le code je m présenter dans ce didacticiel est basé sur l’exemple fourni dans [Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) entrée de blog de s, [lecture un peu avec le regroupement de tri GridView](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx).


## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>Étape 3 : Ajout du séparateur de lignes à la hiérarchie des contrôles GridView s

Étant donné que nous ne souhaitons pas ajouter les lignes de séparateur à la hiérarchie des contrôles GridView s après que sa hiérarchie de contrôle a été créé et créé pour la dernière fois sur cette page, visitez, nous souhaitons effectuer cet ajout à la fin du cycle de vie de page, mais avant le c GridView réelle ontrôle hiérarchie a été rendu en HTML. Le dernier point possible auquel nous pouvons effectuer cette opération est la `Page` classe s `Render` événement que nous pouvons remplacer dans notre classe code-behind à l’aide de la signature de méthode suivante :


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample2.vb)]

Lorsque le `Page` classe d’origine `Render` méthode est appelée `base.Render(writer)` chacun des contrôles dans la page s’affichera, générer la balise en fonction de leur hiérarchie des contrôles. Par conséquent, il est impératif que nous les appelons `base.Render(writer)`, de sorte que la page est rendue, hiérarchie avant d’appeler des contrôles et que nous manipuler le s GridView `base.Render(writer)`, de sorte que les lignes du séparateur ont été ajoutés à la hiérarchie des contrôles GridView s avant s été rendu.

Pour injecter des en-têtes de groupe de tri, nous devons tout d’abord vous assurer que l’utilisateur a demandé que les données triées. Par défaut, le contenu de s GridView n’est pas trié, et par conséquent nous n’avez pas besoin d’entrer n’importe quel groupe de tri des en-têtes.

> [!NOTE]
> Si vous souhaitez que le contrôle GridView à trier par une colonne particulière lors du premier chargement de la page, appelez les opérations de mappage GridView `Sort` méthode sur la première visite de page (mais pas sur les publications ultérieures). Pour ce faire, ajoutez cet appel dans le `Page_Load` Gestionnaire d’événements au sein d’un `if (!Page.IsPostBack)` conditionnel. Faire référence à la [la pagination et tri des données de rapport](paging-and-sorting-report-data-vb.md) informations didacticiels pour en savoir plus sur la `Sort` (méthode).


En supposant que les données ont été triées, la tâche suivante consiste à déterminer quelle colonne données a été triées par et puis pour analyser les lignes de différences dans la colonne s vous recherchez des valeurs. Le code suivant permet de s’assurer que les données ont été triées et recherche la colonne par laquelle les données ont été triées :


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample3.vb)]

Si le contrôle GridView présente encore être triées, le s GridView `SortExpression` propriété n’est pas été définie. Par conséquent, nous voulons uniquement ajouter les lignes de séparateur si cette propriété a une valeur. Le cas échéant, nous devons ensuite déterminer l’index de la colonne par laquelle les données a été triées. Cela s’effectue en parcourant le s GridView `Columns` collection, la recherche de la colonne dont la propriété `SortExpression` propriété est égale à la s GridView `SortExpression` propriété. En plus de l’index de colonne s, nous extrayons également le `HeaderText` propriété, qui est utilisée pour afficher les lignes du séparateur.

Avec l’index de la colonne par laquelle les données sont triées, l’étape finale consiste pour énumérer les lignes du contrôle GridView. Pour chaque ligne, nous devons déterminer si la valeur de s colonne triée diffère de la valeur de s ligne s triées colonne précédente. Si, par conséquent, nous devons injecter un nouveau `GridViewRow` instance dans la hiérarchie des contrôles. Cela s’effectue par le code suivant :


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample4.vb)]

Ce code commence par référencer par programme le `Table` de l’objet trouvé à la racine de la hiérarchie des contrôles GridView s et la création d’une variable chaîne nommée `lastValue`. `lastValue` est utilisé pour comparer la valeur de colonne s triées de ligne actuelle avec la valeur précédente de la ligne s. Ensuite, les opérations de mappage GridView `Rows` collection est énumérée, et pour chaque ligne, la valeur de la colonne triée est stockée dans le `currentValue` variable.

> [!NOTE]
> Pour déterminer la valeur de la colonne triée ligne particulière, j’utilise la cellule s `Text` propriété. Cela fonctionne bien pour BoundFields, mais ne sera pas fonctionnent comme vous le souhaitez pour TemplateField, CheckBoxFields et ainsi de suite. Nous allons examiner comment faire pour prendre en compte pour les autres champs de GridView, peu de temps.


Le `currentValue` et `lastValue` variables sont ensuite comparés. S’ils sont différents, nous devons ajouter une nouvelle ligne de séparateur à la hiérarchie des contrôles. Pour ce faire, vous devez déterminer l’index de la `GridViewRow` dans le `Table` objet s `Rows` collection, création de nouveaux `GridViewRow` et `TableCell` instances, puis en ajoutant le `TableCell` et `GridViewRow` à la hiérarchie des contrôles.

Remarque que le séparateur de lignes s Solitaire `TableCell` est mis en forme tel qu’il s’étend sur toute la largeur du contrôle GridView, est mis en forme à l’aide de la `SortHeaderRowStyle` classe CSS et a son `Text` tel qu’il affiche à la fois le groupe de tri nom de propriété (par exemple, la catégorie) et la valeur de s groupe (par exemple, boissons). Enfin, `lastValue` est mis à jour à la valeur de `currentValue`.

La classe CSS utilisée pour mettre en forme la ligne d’en-tête de groupe tri `SortHeaderRowStyle` doit être spécifié dans le `Styles.css` fichier. N’hésitez pas à utiliser les paramètres de style vous séduire ; J’ai utilisé les éléments suivants :


[!code-css[Main](creating-a-customized-sorting-user-interface-vb/samples/sample5.css)]

Avec le code actuel, l’interface de tri ajoute des en-têtes de groupe de tri lors du tri par n’importe quel BoundField (voir Figure 5, qui montre une capture d’écran lors du tri par fournisseur). Toutefois, lors du tri par n’importe quel autre type de champ (par exemple, un CheckBoxField ou d’un TemplateField), les en-têtes de groupe de tri sont nulle part à rechercher (voir Figure 6).


[![L’Interface de tri inclut les en-têtes de groupe de tri lors du tri par BoundFields](creating-a-customized-sorting-user-interface-vb/_static/image12.png)](creating-a-customized-sorting-user-interface-vb/_static/image11.png)

**Figure 5**: le tri Interface inclut tri en-têtes lors de tri des groupes par BoundFields ([cliquez pour afficher l’image en taille réelle](creating-a-customized-sorting-user-interface-vb/_static/image13.png))


[![Les en-têtes de groupe de tri sont manquants lors de tri une CheckBoxField](creating-a-customized-sorting-user-interface-vb/_static/image15.png)](creating-a-customized-sorting-user-interface-vb/_static/image14.png)

**Figure 6**: les en-têtes de groupe de tri sont manquants lors de tri une CheckBoxField ([cliquez pour afficher l’image en taille réelle](creating-a-customized-sorting-user-interface-vb/_static/image16.png))


Les en-têtes de groupe de tri sont manquants lors du tri par un CheckBoxField est parce que le code utilise actuellement uniquement la `TableCell` s `Text` propriété afin de déterminer la valeur de la colonne triée pour chaque ligne. Pour CheckBoxFields, le `TableCell` s `Text` propriété est une chaîne vide ; au lieu de cela, la valeur est disponible via un contrôle de case à cocher Web qui se trouve dans le `TableCell` s `Controls` collection.

Pour gérer les types de champs autres que BoundFields, nous avons besoin compléter le code où le `currentValue` variable est assignée pour vérifier l’existence d’une case à cocher dans la `TableCell` s `Controls` collection. Au lieu d’utiliser `currentValue = gvr.Cells(sortColumnIndex).Text`, remplacez ce code par le suivant :


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample6.vb)]

Ce code examine la colonne triée `TableCell` pour la ligne actuelle pour déterminer s’il existe des contrôles dans le `Controls` collection. S’il existe et que le premier contrôle est une case à cocher, le `currentValue` variable est définie sur Oui ou non, en fonction de la case à cocher s `Checked` propriété. Sinon, la valeur est obtenue à partir de la `TableCell` s `Text` propriété. Cette logique peut être répliquée pour gérer le tri pour n’importe quel TemplateField qui peut-être exister dans le contrôle GridView.

Avec l’ajout de code ci-dessus, les en-têtes de groupe de tri sont désormais présentes lors du tri par le CheckBoxField abandonné (voir la Figure 7).


[![Les en-têtes de groupe de tri sont désormais présentes lorsque tri un CheckBoxField](creating-a-customized-sorting-user-interface-vb/_static/image18.png)](creating-a-customized-sorting-user-interface-vb/_static/image17.png)

**Figure 7**: les en-têtes de groupe de tri sont désormais présentes lorsque tri un CheckBoxField ([cliquez pour afficher l’image en taille réelle](creating-a-customized-sorting-user-interface-vb/_static/image19.png))


> [!NOTE]
> Si vous disposez de produits avec `NULL` de base de données des valeurs pour le `CategoryID`, `SupplierID`, ou `UnitPrice` champs, ces valeurs seront affichent comme des chaînes vides dans le contrôle GridView par défaut, ce qui signifie le texte de ligne s séparateur pour les produits avec `NULL`valeurs seront lues comme catégorie : (autrement dit, il s aucun nom de catégorie : comme avec la catégorie : boissons). Si vous souhaitez une valeur affichée ici vous pouvez définir le BoundFields [ `NullDisplayText` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx) au texte que vous souhaitez afficher, ou vous pouvez ajouter une instruction conditionnelle dans la méthode Render quand vous assignez le `currentValue` pour le séparateur ligne s `Text` propriété.


## <a name="summary"></a>Récapitulatif

Le contrôle GridView n’inclut pas de nombreuses options intégrées pour la personnalisation de l’interface de tri. Toutefois, avec un peu de code de bas niveau, il s possible de modifier la hiérarchie des contrôles GridView s pour créer une interface plus personnalisée. Dans ce didacticiel, nous avons vu comment ajouter une ligne de séparateur de groupe de tri pour un GridView triable, qui identifie plus facilement les groupes distincts et les limites de ces groupes. Pour obtenir des exemples supplémentaires d’interfaces de tri personnalisés, consultez [Scott Guthrie](https://weblogs.asp.net/scottgu/) s [quelques ASP.NET 2.0 GridView tri trucs et astuces](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx) entrée de blog.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](sorting-custom-paged-data-vb.md)
