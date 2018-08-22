---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
title: Tri des données dans un contrôle DataList ou Repeater (c#) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons examiner comment inclure la prise en charge dans les contrôles DataList et Repeater de tri, ainsi que comment construire DataList ou Repeater dont les données peuvent...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: f52c302a-1b7c-46fe-8a13-8412c95cbf6d
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 05fbc51d5341a4d3d634cbbc05c0e66a827b0394
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835031"
---
<a name="sorting-data-in-a-datalist-or-repeater-control-c"></a>Tri des données dans un contrôle DataList ou Repeater (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_CS.exe) ou [télécharger le PDF](sorting-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial45cs1.pdf)

> Dans ce didacticiel, nous allons examiner comment inclure la prise en charge dans les contrôles DataList et Repeater de tri, ainsi que comment construire DataList ou Repeater dont les données peuvent être triées et la pagination.


## <a name="introduction"></a>Introduction

Dans le [didacticiel précédent](paging-report-data-in-a-datalist-or-repeater-control-cs.md) nous avons examiné comment ajouter la prise en charge la pagination pour un contrôle DataList. Nous avons créé une nouvelle méthode dans le `ProductsBLL` classe (`GetProductsAsPagedDataSource`) qui a retourné un `PagedDataSource` objet. Lorsqu’il est lié à un contrôle DataList ou le Repeater, DataList ou Repeater afficherait simplement la page demandée de données. Cette technique est similaire à celui utilisé en interne par les contrôles GridView, DetailsView et FormView pour fournir la fonctionnalité de pagination de leur valeur par défaut.

En plus d’offrir la prise en charge la pagination, le contrôle GridView inclut également prêt à l’emploi prise en charge de tri. Repeater ni DataList fournit des fonctionnalités de tri intégrées ; Toutefois, les fonctionnalités de tri peuvent être ajoutées avec un peu de code. Dans ce didacticiel, nous allons examiner comment inclure la prise en charge dans les contrôles DataList et Repeater de tri, ainsi que comment construire DataList ou Repeater dont les données peuvent être triées et la pagination.

## <a name="a-review-of-sorting"></a>Une revue de tri

Comme nous l’avons vu dans la [la pagination et tri des données de rapport](../paging-and-sorting/paging-and-sorting-report-data-cs.md) didacticiel, le contrôle GridView fournit prêts à l’emploi prise en charge de tri. Chaque champ GridView peut être associé à `SortExpression`, ce qui indique le champ de données en fonction desquelles trier les données. Lorsque les opérations de mappage GridView `AllowSorting` propriété est définie sur `true`, chaque champ GridView qui a un `SortExpression` valeur de propriété a son en-tête restitué sous la forme d’un LinkButton. Lorsqu’un utilisateur clique sur un en-tête de s GridView champ particulier, une publication (postback) se produit et les données sont triées selon le champ utilisateur a cliqué dessu s `SortExpression`.

Le contrôle GridView a un `SortExpression` propriété, qui stocke le `SortExpression` du champ GridView, les données sont triées par. En outre, un `SortDirection` propriété indique si les données sont soit triée dans l’ordre croissant ou décroissant (si un utilisateur clique sur un lien particulier d’en-tête champ s GridView deux fois de suite, l’ordre de tri est activé ou désactivé).

Lorsque le contrôle GridView est lié à son contrôle de source de données, il transmet ses `SortExpression` et `SortDirection` propriétés aux données de contrôle de code source. Le contrôle de source de données récupère les données et les trie en fonction de fourni `SortExpression` et `SortDirection` propriétés. Après le tri des données, le contrôle de source de données retourne au GridView.

Pour répliquer cette fonctionnalité avec les contrôles DataList ou Repeater, nous devons :

- Créer une interface de tri
- N’oubliez pas le champ de données à trier et s’il faut trier dans l’ordre croissant ou décroissant
- Demander à ObjectDataSource pour trier les données par un champ de données particulier

Nous les aborderons ces trois tâches dans les étapes 3 et 4. Ensuite, nous allons examiner comment inclure à la fois la pagination et tri de prise en charge dans un contrôle DataList ou le Repeater.

## <a name="step-2-displaying-the-products-in-a-repeater"></a>Étape 2 : Afficher les produits dans un répéteur

Avant de nous soucier d’implémenter les fonctionnalités associées au tri, permettent de s commencez par répertorier les produits dans un contrôle Repeater. Commencez par ouvrir le `Sorting.aspx` page dans le `PagingSortingDataListRepeater` dossier. Ajouter un contrôle Repeater à la page web, en définissant son `ID` propriété `SortableProducts`. À partir de la balise active Repeater s, créer un nouveau ObjectDataSource nommé `ProductsDataSource` et configurez-le pour récupérer des données à partir de la `ProductsBLL` classe s `GetProducts()` (méthode). Sélectionnez (aucun) option dans la liste déroulante dans les onglets INSERT, UPDATE et DELETE.


[![Créer un ObjectDataSource et configurez-le pour utiliser la méthode GetProductsAsPagedDataSource()](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**Figure 1**: créer un ObjectDataSource et configurez-le pour utiliser le `GetProductsAsPagedDataSource()` (méthode) ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image3.png))


[![Définir les listes de liste déroulante dans la mise à jour, insertion et supprimer des onglets à (None)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image4.png)

**Figure 2**: définir les listes de liste déroulante dans la mise à jour, insertion et supprimer des onglets à (None) ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image6.png))


À la différence avec le contrôle DataList, Visual Studio ne crée pas automatiquement un `ItemTemplate` pour le contrôle Repeater après sa liaison à une source de données. En outre, nous devons ajouter cela `ItemTemplate` de façon déclarative, comme la balise active du contrôle s Repeater ne dispose pas de l’option Modifier les modèles trouvée dans le contrôle DataList s. Let s utilisent le même `ItemTemplate` du didacticiel précédent, qui affiche le nom de produit s, le fournisseur et la catégorie.

Après avoir ajouté le `ItemTemplate`, le balisage déclaratif s Repeater et ObjectDataSource doit ressembler à ce qui suit :


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample1.aspx)]

Figure 3 illustre cette page lorsqu’ils sont affichés via un navigateur.


[![Chaque produit s, nom de fournisseur et la catégorie s’affiche.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**Figure 3**: chaque produit s nom, fournisseur et la catégorie s’affiche ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))


## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>Étape 3 : Demandant l’ObjectDataSource pour trier les données

Pour trier les données affichées dans le répéteur, nous devons informer ObjectDataSource de l’expression de tri par lequel les données doivent être triées. Avant de l’ObjectDataSource récupère ses données, il déclenche tout d’abord sa [ `Selecting` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx), qui fournit une opportunité pour nous permettre de spécifier une expression de tri. Le `Selecting` un objet de type est passé au gestionnaire d’événements [ `ObjectDataSourceSelectingEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx), ce qui a une propriété nommée [ `Arguments` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx) de type [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx). Le `DataSourceSelectArguments` classe est conçu pour transmettre les demandes liées aux données à partir d’un consommateur de données au contrôle de source de données et inclut un [ `SortExpression` propriété](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.sortexpression.aspx).

Pour transmettre les informations de tri à partir de la page ASP.NET à ObjectDataSource, créez un gestionnaire d’événements pour le `Selecting` événement et utilisez le code suivant :


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

Le *sortExpression* valeur doit être attribuée le nom du champ de données pour trier des données (par exemple, ProductName). Aucune propriété liés à la direction de tri est, par conséquent, si vous souhaitez trier les données dans l’ordre décroissant, ajoutez la chaîne DESC à le *sortExpression* valeur (par exemple, ProductName DESC).

Continuez et essayez d’autres valeurs codées en dur pour *sortExpression* et les résultats des tests dans un navigateur. Comme illustré dans la Figure 4, lorsque vous utilisez DESC ProductName comme le *sortExpression*, les produits sont triés par leur nom dans l’ordre alphabétique inverse.


[![Les produits sont triés par leur nom dans l’ordre alphabétique inverse](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**Figure 4**: les produits sont triés par leur nom dans l’ordre alphabétique inverse ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))


## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>Étape 4 : Création de l’Interface de tri et la mémorisation de l’Expression de tri et la Direction

Tension de tri de prise en charge dans le contrôle GridView convertit chaque champ pouvant être trié s en-tête de texte un LinkButton qui, lorsque vous cliquez dessus, trie les données en conséquence. Une telle interface de tri de sens pour le contrôle GridView, où ses données sont parfaitement présentées dans les colonnes. Toutefois, pour les contrôles DataList et Repeater, une interface de tri différente est nécessaire. Une interface commune de tri pour une liste de données (par opposition à une grille de données), est une liste déroulante, qui fournit les champs par lequel les données peuvent être triées. Permettent d’implémenter une telle interface pour ce didacticiel s.

Ajouter un contrôle DropDownList Web ci-dessus le `SortableProducts` Repeater et définissez son `ID` propriété `SortBy`. À partir de la fenêtre Propriétés, cliquez sur le bouton de sélection dans le `Items` propriété pour afficher l’éditeur de collections ListItem. Ajouter `ListItem` s pour trier les données par le `ProductName`, `CategoryName`, et `SupplierName` champs. Ajoutez également un `ListItem` pour trier les produits par leur nom dans l’ordre alphabétique inverse.

Le `ListItem` `Text` propriétés peuvent être définies à n’importe quelle valeur (par exemple, nom), mais la `Value` propriétés doivent être définies sur le nom du champ de données (par exemple, ProductName). Pour trier les résultats dans l’ordre décroissant, ajoutez la chaîne DESC pour le nom de champ de données, comme ProductName DESC.


![Ajouter un élément ListItem pour chacun des champs de données pouvant être trié](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**Figure 5**: ajouter un `ListItem` pour chacun des champs de données pouvant être trié


Enfin, ajoutez un contrôle Web de bouton à droite de la liste DropDownList. Définissez ses `ID` à `RefreshRepeater` et son `Text` propriété pour l’actualisation.

Après avoir créé le `ListItem` s et en ajoutant le bouton d’actualisation, la syntaxe déclarative des s DropDownList et bouton doit ressembler à ce qui suit :


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

Avec l’objet DropDownList de tri est terminée, nous devons ensuite mettre à jour de l’ObjectDataSource s `Selecting` afin qu’il utilise le Gestionnaire d’événements `SortBy``ListItem` s `Value` propriété par opposition à une expression de tri codées en dur.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample4.cs)]

À ce stade, lors de la visite de tout d’abord la page, les produits sont initialement triés par le `ProductName` champ de données, tel qu’il s le `SortBy` `ListItem` sélectionné par défaut (voir Figure 6). En sélectionnant une autre option comme catégorie de tri et en cliquant sur Actualiser pour provoquer une publication (postback) et retrier les données par le nom de catégorie, comme le montre la Figure 7.


[![Les produits sont initialement triés par leur nom](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)

**Figure 6**: les produits sont initialement triés par leur nom ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image16.png))


[![Les produits sont maintenant triés par catégorie](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)

**Figure 7**: les produits sont maintenant triés par catégorie ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image19.png))


> [!NOTE]
> En cliquant sur le bouton d’actualisation génère les données automatiquement retriée car l’état d’affichage s Repeater a été désactivé, ce qui provoque le Repeater relier à sa source de données sur chaque publication (postback). Si vous avez déjà quitté l’état d’affichage Repeater s activée, modifier le tri déroulante liste a gagné t a aucun effet sur l’ordre de tri. Pour résoudre ce problème, créez un gestionnaire d’événements pour le bouton Actualiser s `Click` rebind le répéteur à sa source de données et événements (en appelant le Repeater s `DataBind()` méthode).


## <a name="remembering-the-sort-expression-and-direction"></a>Mémorisation de l’Expression de tri et la Direction

Lors de la création d’un triable DataList ou Repeater sur une page où non tri liés publications (postback) peut se produire, il impératif s que l’expression de tri et la direction être mémorisées entre les postbacks. Par exemple, imaginez que nous le Repeater mis à jour de ce didacticiel pour inclure un bouton Supprimer avec chaque produit. Lorsque l’utilisateur clique sur le bouton Supprimer nous d exécuter du code pour supprimer le produit sélectionné, puis à relier les données pour le contrôle Repeater. Si les détails de tri ne sont pas préservées de publication (postback), les données affichées sur l’écran reviendra à l’ordre de tri d’origine.

Pour ce didacticiel, la liste DropDownList implicitement enregistre le tri expression et le sens dans son état d’affichage pour nous. Si nous utilisions une interface de tri différents un avec, par exemple, boutons LinkButton qui font fournies les différentes options de tri d nous devons prendre soin de mémoriser l’ordre de tri entre les postbacks. Cela peut être accompli en stockant les paramètres de tri dans l’état d’affichage de page s, en incluant le paramètre de tri dans la chaîne de requête, ou via une autre technique de persistance d’état.

Exemples de futures de ce didacticiel Explorez comment conserver les détails de tri dans l’état d’affichage de page s.

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>Étape 5 : Ajout du tri de prise en charge pour un contrôle DataList qui utilise la pagination par défaut

Dans le [didacticiel précédent](paging-report-data-in-a-datalist-or-repeater-control-cs.md) nous avons examiné comment implémenter la pagination par défaut avec une DataList. Permettent d’étendre cet exemple précédent afin d’inclure la possibilité de trier les données paginées s. Commencez par ouvrir le `SortingWithDefaultPaging.aspx` et `Paging.aspx` des pages dans le `PagingSortingDataListRepeater` dossier. À partir de la `Paging.aspx` page, cliquez sur le bouton de la Source pour afficher le balisage déclaratif page s. Copiez le texte sélectionné (voir Figure 8) et collez-le dans le balisage déclaratif de `SortingWithDefaultPaging.aspx` entre le `<asp:Content>` balises.


[![Répliquer le balisage déclaratif dans le &lt;asp : Content&gt; balises de Paging.aspx à SortingWithDefaultPaging.aspx](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)

**Figure 8**: répliquer le balisage déclaratif dans le `<asp:Content>` balises à partir de `Paging.aspx` à `SortingWithDefaultPaging.aspx` ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image22.png))


Après avoir copié le balisage déclaratif, copiez les méthodes et propriétés dans le `Paging.aspx` page classe code-behind de s à la classe code-behind pour `SortingWithDefaultPaging.aspx`. Ensuite, prenez un moment pour afficher la `SortingWithDefaultPaging.aspx` page dans un navigateur. Il doit présenter les mêmes fonctionnalités et la même apparence que `Paging.aspx`.

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>Amélioration de ProductsBLL pour inclure une valeur par défaut, la pagination et le tri (méthode)

Dans le didacticiel précédent, nous avons créé un `GetProductsAsPagedDataSource(pageIndex, pageSize)` méthode dans le `ProductsBLL` classe qui a retourné un `PagedDataSource` objet. Cela `PagedDataSource` objet a été rempli avec *tous les* des produits (par le biais de la couche BLL s `GetProducts()` méthode), mais lorsqu’elle est liée à la DataList uniquement les enregistrements correspondant au spécifié *pageIndex* et *pageSize* des paramètres d’entrée ont été affichés.

Précédemment dans ce didacticiel, nous avons ajouté une prise en charge tri en spécifiant l’expression de tri à partir de l’ObjectDataSource s `Selecting` Gestionnaire d’événements. Cela fonctionne bien quand ObjectDataSource est retourné à un objet pouvant être triées, comme le `ProductsDataTable` retourné par le `GetProducts()` (méthode). Toutefois, le `PagedDataSource` objet retourné par la `GetProductsAsPagedDataSource` méthode ne prend pas en charge le tri de sa source de données interne. Au lieu de cela, nous avons besoin de trier les résultats retournés par la `GetProducts()` méthode *avant* nous l’expliquons la `PagedDataSource`.

Pour ce faire, créez une nouvelle méthode dans le `ProductsBLL` (classe), `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`. Pour trier les `ProductsDataTable` retourné par la `GetProducts()` (méthode), spécifiez la `Sort` propriété de sa valeur par défaut `DataTableView`:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

Le `GetProductsSortedAsPagedDataSource` méthode diffère légèrement de la `GetProductsAsPagedDataSource` méthode créé dans le didacticiel précédent. En particulier, `GetProductsSortedAsPagedDataSource` accepte un paramètre d’entrée supplémentaire `sortExpression` et assigne cette valeur à la `Sort` propriété de la `ProductDataTable` s `DefaultView`. Quelques lignes de code plus tard, le `PagedDataSource` objet s source de données est assigné le `ProductDataTable` s `DefaultView`.

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>Appel de la méthode GetProductsSortedAsPagedDataSource et en spécifiant la valeur pour le paramètre d’entrée SortExpression

Avec la `GetProductsSortedAsPagedDataSource` terminée, l’étape suivante consiste à fournir la valeur de ce paramètre. ObjectDataSource dans `SortingWithDefaultPaging.aspx` est actuellement configuré pour appeler le `GetProductsAsPagedDataSource` et transmet les deux paramètres d’entrée via ses deux `QueryStringParameters`, lesquels sont spécifiés dans le `SelectParameters` collection. Ces deux `QueryStringParameters` indiquer que la source pour le `GetProductsAsPagedDataSource` méthode s *pageIndex* et *pageSize* paramètres proviennent les champs de chaîne de requête `pageIndex` et `pageSize`.

Mettre à jour de l’ObjectDataSource s `SelectMethod` propriété afin qu’elle appelle la nouvelle `GetProductsSortedAsPagedDataSource` (méthode). Ensuite, ajoutez une nouvelle `QueryStringParameter` afin que le *sortExpression* paramètre d’entrée est accessible à partir du champ querystring `sortExpression`. Définir le `QueryStringParameter` s `DefaultValue` ProductName.

Après ces modifications, le balisage déclaratif de s ObjectDataSource doit ressembler à :


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample6.aspx)]

À ce stade, le `SortingWithDefaultPaging.aspx` page triera ses résultats par ordre alphabétique par nom de produit (voir Figure 9). C’est pourquoi, par défaut, une valeur de ProductName est passée en tant que le `GetProductsSortedAsPagedDataSource` méthode s *sortExpression* paramètre.


[![Par défaut, les résultats sont triés par nom de produit](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)

**Figure 9**: par défaut, les résultats sont triés par `ProductName` ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image25.png))


Si vous ajoutez manuellement un `sortExpression` champ querystring comme `SortingWithDefaultPaging.aspx?sortExpression=CategoryName` les résultats sont triés par spécifié `sortExpression`. Toutefois, cela `sortExpression` paramètre n’est pas inclus dans la chaîne de requête lors du déplacement vers une autre page de données. En fait, en cliquant sur la page suivante ou dernière boutons nous revenez dans `Paging.aspx`! En outre, s’il y a actuellement aucun tri de l’interface. Un utilisateur peut modifier l’ordre de tri des données paginées impérativement en manipulant directement de la chaîne de requête.

## <a name="creating-the-sorting-interface"></a>Création de l’Interface de tri

Nous devons tout d’abord mettre à jour le `RedirectUser` méthode pour envoyer l’utilisateur à `SortingWithDefaultPaging.aspx` (au lieu de `Paging.aspx`) et pour inclure la `sortExpression` valeur dans la chaîne de requête. Nous devons également ajouter un en lecture seule au niveau des pages nommé `SortExpression` propriété. Cette propriété, similaire à la `PageIndex` et `PageSize` propriétés créées dans le didacticiel précédent, retourne la valeur de la `sortExpression` champ querystring si elle existe, la valeur par défaut valeur (ProductName) dans le cas contraire.

Actuellement le `RedirectUser` méthode accepte uniquement un paramètre d’entrée unique l’index de la page à afficher. Toutefois, il peut arriver que vous souhaitiez rediriger l’utilisateur vers une page de données à l’aide d’une expression de tri autre que Nouveautés spécifiée dans la chaîne de requête spécifique. Dans un instant, nous allons créer l’interface de tri pour cette page, ce qui inclut une série de contrôles Web de bouton pour trier les données par une colonne spécifiée. Lorsqu’un de ces boutons est activé, nous voulons rediriger l’utilisateur en passant la valeur d’expression de tri approprié. Pour fournir cette fonctionnalité, créez deux versions de la `RedirectUser` (méthode). La première condition doit accepter simplement l’index de page à afficher, tandis que l’autre accepte l’expression de tri et les index de page.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

Dans le premier exemple dans ce didacticiel, nous avons créé une interface de tri à l’aide d’un contrôle DropDownList. Pour cet exemple, s permettent d’utiliser les trois contrôles de bouton Web positionnées au-dessus du contrôle DataList, un pour le tri par `ProductName`, un pour `CategoryName`et l’autre pour `SupplierName`. Ajoutez les trois contrôles de bouton Web, en définissant leurs `ID` et `Text` propriétés en conséquence :


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample8.aspx)]

Ensuite, créez un `Click` pour chaque gestionnaire d’événements. Les gestionnaires d’événements doivent appeler la `RedirectUser` méthode, en retournant l’utilisateur à la première page à l’aide de l’expression de tri approprié.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

Lors de la première visite la page, les données sont triées par ordre alphabétique par nom de produit (voir la Figure 9). Cliquez sur le bouton suivant pour passer à la deuxième page de données, puis cliquez sur le tri par un bouton de la catégorie. Cela nous ramène à la première page de données, triées par nom de catégorie (voir Figure 10). De même, en cliquant sur le tri par un bouton de fournisseur trie les données par le fournisseur à partir de la première page de données. Le choix de tri est mémorisé comme les données sont paginées par le biais. Figure 11 illustre la page après le tri par catégorie et ensuite passer à la page treizième de données.


[![Les produits sont triées par catégorie](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)

**Figure 10**: les produits sont triées par catégorie ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image28.png))


[![L’Expression de tri est mémorisée lorsque la pagination via le données](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image29.png)

**Figure 11**: l’Expression de tri est mémorisée lorsque la pagination via le données ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image31.png))


## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>Étape 6 : La pagination personnalisée dans les enregistrements dans un répéteur

L’exemple de DataList examiné à l’étape 5 pages via ses données à l’aide de la technique de la pagination par défaut inefficace. Lors de la pagination via suffisamment grandes quantités de données, il est impératif que la pagination personnalisée est utilisée. Dans le [efficacement la pagination par le biais d’importants volumes de données](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) et [tri des données paginées personnalisées](../paging-and-sorting/sorting-custom-paged-data-cs.md) didacticiels, nous avons examiné les différences entre la pagination personnalisée et méthodes créées par défaut et dans la couche BLL pour utilisation de la pagination et le tri des données paginées personnalisées personnalisé. En particulier, dans ces deux didacticiels précédents, nous avons ajouté les trois méthodes suivantes pour la `ProductsBLL` classe :

- `GetProductsPaged(startRowIndex, maximumRows)` Retourne un sous-ensemble particulier d’enregistrements en commençant à *startRowIndex* et ne dépassant ne pas *maximumRows*.
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)` Retourne un sous-ensemble particulier d’enregistrements triés selon spécifié *sortExpression* paramètre d’entrée.
- `TotalNumberOfProducts()` fournit le nombre total d’enregistrements dans la `Products` table de base de données.

Ces méthodes peuvent être utilisées pour page efficacement et de tri des données à l’aide d’un contrôle DataList ou Repeater. Pour illustrer ceci, laisser s démarrer en créant un contrôle Repeater avec prise en charge la pagination personnalisée ; Nous allons ensuite ajouter des fonctionnalités de tri.

Ouvrir le `SortingWithCustomPaging.aspx` page dans le `PagingSortingDataListRepeater` dossier et ajoutez un répéteur à la page, le paramètre son `ID` propriété `Products`. À partir de la balise active Repeater s, créer un nouveau ObjectDataSource nommé `ProductsDataSource`. Configurez-le pour sélectionner ses données à partir de la `ProductsBLL` classe s `GetProductsPaged` (méthode).


[![Configurer pour utiliser la méthode de GetProductsPaged ProductsBLL classe s ObjectDataSource](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image32.png)

**Figure 12**: configurer ObjectDataSource à utiliser le `ProductsBLL` classe s `GetProductsPaged` (méthode) ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image34.png))


Définir les listes déroulantes dans la mise à jour, insertion et supprimer des onglets à (None) et puis cliquez sur le bouton suivant. L’Assistant Configurer la Source de données invite désormais l’utilisateur pour les sources de la `GetProductsPaged` méthode s *startRowIndex* et *maximumRows* paramètres d’entrée. En réalité, ces paramètres d’entrée sont ignorées. Au lieu de cela, le *startRowIndex* et *maximumRows* valeurs sont transmises dans les `Arguments` propriété dans le s ObjectDataSource `Selecting` Gestionnaire d’événements, tout comme la façon dont nous avons spécifié le *sortExpression* dans cette démonstration premier didacticiel s. Par conséquent, laissez le paramètre source listes déroulantes dans l’Assistant définie sur None.


[![Conservez le paramètre Sources None](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image35.png)

**Figure 13**: laissez le paramètre Sources défini sur None ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image37.png))


> [!NOTE]
> Faire *pas* définir le s ObjectDataSource `EnablePaging` propriété `true`. Cela entraîne l’ObjectDataSource inclure automatiquement son propre *startRowIndex* et *maximumRows* paramètres pour le `SelectMethod` liste de paramètres existant s. Le `EnablePaging` propriété est utile lors de la liaison personnalisée, car ces contrôles attendent un comportement particulier à partir de l’ObjectDataSource s de données à un contrôle GridView, DetailsView ou FormView paginées uniquement disponible quand `EnablePaging` propriété est `true`. Dans la mesure où nous devons ajouter manuellement la prise en charge la pagination pour les contrôles DataList et Repeater, laissez cette propriété la valeur `false` (la valeur par défaut), comme nous allons intégrer dans les fonctionnalités nécessaires directement au sein de notre page ASP.NET.


Enfin, définissez le Repeater s `ItemTemplate` afin que le nom de produit s, la catégorie et le fournisseur sont affichés. Après ces modifications, la syntaxe déclarative des s Repeater et ObjectDataSource doit ressembler à ce qui suit :


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample10.aspx)]

Prenez un moment pour visiter la page via un navigateur et notez qu’aucun enregistrement n’est retournés. Il s’agit, car nous ve encore pour spécifier le *startRowIndex* et *maximumRows* les valeurs de paramètre ; par conséquent, les valeurs de 0 sont passées dans les deux. Pour spécifier ces valeurs, créez un gestionnaire d’événements pour les opérations de mappage ObjectDataSource `Selecting` événements et définir ces paramètres de valeurs par programmation aux codées en dur les valeurs de 0 et 5, respectivement :


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample11.cs)]

Avec cette modification, la page, lorsqu’ils sont affichés via un navigateur, affiche les cinq premiers produits.


[![Les cinq premiers enregistrements sont affichés.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image38.png)

**Figure 14**: le premier cinq enregistrements sont affichés ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image40.png))


> [!NOTE]
> Les produits répertoriés dans la Figure 14 se produisent à trier par nom de produit, car le `GetProductsPaged` procédure stockée qui effectue la requête de la pagination personnalisée efficace trie les résultats par `ProductName`.


Pour autoriser l’utilisateur à parcourir les pages, que nous devons effectuer le suivi de l’index de ligne de début et le nombre maximal de lignes et n’oubliez pas ces valeurs entre les postbacks. Dans l’exemple de la pagination par défaut, nous avons utilisé les champs de chaîne de requête pour conserver ces valeurs ; pour cette démonstration, permettent de conserver ces informations dans l’état d’affichage de page s s. Créez les deux propriétés suivantes :


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample12.cs)]

Ensuite, mettez à jour le code dans le Gestionnaire d’événements de sélection afin qu’il utilise le `StartRowIndex` et `MaximumRows` propriétés au lieu des valeurs codées en dur de 0 à 5 :


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample13.cs)]

À ce stade notre page affiche toujours uniquement les cinq premiers enregistrements. Toutefois, avec ces propriétés en place, nous vous êtes prêt à créer notre interface de pagination.

## <a name="adding-the-paging-interface"></a>Ajout de l’Interface de pagination

Let s utiliser le même premier, précédent, en regard, dernière la pagination de l’interface utilisée dans l’exemple de la pagination par défaut, notamment le Web de l’étiquette de contrôle qui affiche la page de données est en cours de visualisation et le nombre total de pages existent. Ajoutez les quatre contrôles bouton Web et étiquette en dessous de répéteur.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample14.aspx)]

Ensuite, créez `Click` gestionnaires d’événements pour les quatre boutons. Lorsqu’un de ces boutons est activé, nous devons mettre à jour le `StartRowIndex` et relier les données pour le contrôle Repeater. Le code des première, boutons Précédent et suivant est assez simple, mais pour le dernier bouton comment nous déterminer l’index de ligne de début de la dernière page de données ? Pour cet index, ainsi que la capacité à déterminer que si les boutons suivant et dernier doivent être activés. nous devons savoir combien d’enregistrements au total est en cours de pagination par le biais de calcul. Nous pouvons le déterminer en appelant le `ProductsBLL` classe s `TotalNumberOfProducts()` (méthode). S permettent de créer une propriété en lecture seule, au niveau de la page nommée `TotalRowCount` qui retourne les résultats de la `TotalNumberOfProducts()` méthode :


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample15.cs)]

Avec cette propriété, nous pouvons maintenant déterminer le dernier index de ligne de début page s. Plus précisément, il s le résultat entier de la `TotalRowCount` moins 1 divisée par `MaximumRows`, multipliée par `MaximumRows`. Nous pouvons maintenant écrire le `Click` gestionnaires d’événements pour les quatre boutons d’interface de pagination :


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample16.cs)]

Enfin, nous devons désactiver les boutons de premier et précédent dans l’interface de pagination lors de l’affichage de la première page de données et les boutons suivant et dernier lors de l’affichage de la dernière page. Pour ce faire, ajoutez le code suivant pour les opérations de mappage ObjectDataSource `Selecting` Gestionnaire d’événements :


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample17.cs)]

Après avoir ajouté ces `Click` gestionnaires d’événements et le code pour activer ou désactiver les éléments d’interface de pagination en fonction de l’index de ligne de début actuelle, la page de test dans un navigateur. Lorsque la Figure 15 illustre, lors de la première visite la page de la première et les boutons précédent sont désactivées. Clic sur suivant montre la deuxième page de données, tandis qu’en cliquant sur la dernière affiche la dernière page (voir les Figures 16 et 17). Lors de l’affichage de la dernière page de données, le suivant et le dernier boutons sont désactivés.


[![Les boutons Précédent et dernière sont désactivées lors de l’affichage de la première Page de produits](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image41.png)

**Figure 15**: le précédent et dernier boutons sont désactivées lors de l’affichage de la première Page de produits ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image43.png))


[![La deuxième Page de produits sont Dispalyed](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image44.png)

**Figure 16**: la deuxième Page de produits sont Dispalyed ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image46.png))


[![En cliquant sur la dernière affiche la dernière Page de données](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image47.png)

**Figure 17**: en cliquant sur la dernière affiche la Page de données finale ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image49.png))


## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>Étape 7 : Y compris le tri de la prise en charge avec personnalisé paginée Repeater

Maintenant que la pagination personnalisée a été implémentée, nous re prêt à inclure le tri en charge. Le `ProductsBLL` classe s `GetProductsPagedAndSorted` méthode a les mêmes *startRowIndex* et *maximumRows* d’entrée de paramètres en tant que `GetProductsPaged`, mais permet à un autre  *sortExpression* paramètre d’entrée. Pour utiliser le `GetProductsPagedAndSorted` méthode à partir de `SortingWithCustomPaging.aspx`, nous devons effectuer les étapes suivantes :

1. Modifier les opérations de mappage ObjectDataSource `SelectMethod` propriété à partir de `GetProductsPaged` à `GetProductsPagedAndSorted`.
2. Ajouter un *sortExpression* `Parameter` objet pour les opérations de mappage ObjectDataSource `SelectParameters` collection.
3. Créer une privée, au niveau des pages `SortExpression` propriété qui conserve sa valeur entre les postbacks via l’état d’affichage de page s.
4. Mettre à jour de l’ObjectDataSource s `Selecting` Gestionnaire d’événements pour affecter les opérations de mappage ObjectDataSource *sortExpression* paramètre la valeur de niveau de la page `SortExpression` propriété.
5. Créez l’interface de tri.

Commencez par la mise à jour de l’ObjectDataSource s `SelectMethod` propriété et en ajoutant un *sortExpression* `Parameter`. Assurez-vous que le *sortExpression* `Parameter` s `Type` propriété est définie sur `String`. À l’issue de ces deux premières tâches, le balisage déclaratif de s ObjectDataSource doit ressembler à ce qui suit :


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample18.aspx)]

Ensuite, nous avons besoin d’un niveau de la page `SortExpression` propriété dont la valeur est sérialisée en état d’affichage. Si aucune valeur d’expression de tri a été définie, utilisez ProductName comme la valeur par défaut :


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample19.cs)]

Avant de l’ObjectDataSource appelle le `GetProductsPagedAndSorted` méthode nous devons définir la *sortExpression* `Parameter` à la valeur de la `SortExpression` propriété. Dans le `Selecting` Gestionnaire d’événements, ajoutez la ligne de code suivante :


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample20.cs)]

Ne reste qu’à implémenter l’interface de tri. Comme nous l’avons fait dans le dernier exemple, autorise les s à disposer de l’interface de tri implémenté à l’aide de trois contrôles de bouton Web permettant aux utilisateurs de trier les résultats par nom de produit, catégorie ou le fournisseur.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample21.aspx)]

Créer `Click` gestionnaires d’événements pour ces trois contrôles de bouton. Réinitialisation de l’événement de gestionnaire, la `StartRowIndex` à 0, définissez la `SortExpression` à la valeur appropriée et les données pour le contrôle Repeater la reliaison :


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample22.cs)]

S résume-t-elle est ! Pendant que vous rencontrez un nombre d’étapes de la pagination personnalisée et le tri implémenté, les étapes ont été très similaires à celles nécessaires pour la pagination par défaut. Figure 18 montre les produits lors de l’affichage de la dernière page de données triés par catégorie.


[![La dernière Page de données, trié par catégorie, s’affiche.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image50.png)

**Figure 18**: la dernière Page de données, trié par catégorie, s’affiche ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image52.png))


> [!NOTE]
> Dans les exemples précédents, lors du tri par le fournisseur de que nom fournisseur a été utilisé en tant que l’expression de tri. Toutefois, pour l’implémentation de la pagination personnalisée, nous devons utiliser CompanyName. Il s’agit, car la procédure stockée chargée d’implémenter la pagination personnalisée `GetProductsPagedAndSorted` transmet l’expression de tri dans le `ROW_NUMBER()` mot clé, le `ROW_NUMBER()` mot clé nécessite le nom de colonne réel plutôt qu’un alias. Par conséquent, nous devons utiliser `CompanyName` (le nom de la colonne dans la `Suppliers` table) au lieu de l’alias utilisé dans le `SELECT` requête (`SupplierName`) pour l’expression de tri.


## <a name="summary"></a>Récapitulatif

Ni la DataList ni Repeater offrent la prise en charge du tri intégrée, mais avec un peu de code et une interface de tri personnalisée, ces fonctionnalités peuvent être ajoutées. Lorsque vous implémentez le tri, mais ne pas la pagination, l’expression de tri peut être spécifiée via la `DataSourceSelectArguments` objet passé dans les opérations de mappage ObjectDataSource `Select` (méthode). Cela `DataSourceSelectArguments` objet s `SortExpression` propriété peut être affectée dans le s ObjectDataSource `Selecting` Gestionnaire d’événements.

Pour ajouter des fonctionnalités de tri à un contrôle DataList ou le Repeater qui fournit déjà la prise en charge la pagination, la plus simple consiste à personnaliser la couche de logique métier pour inclure une méthode qui accepte une expression de tri. Ces informations peuvent ensuite être transmises dans un paramètre dans le s ObjectDataSource `SelectParameters`.

Ce didacticiel terminé notre examen de pagination et tri avec les contrôles DataList et Repeater. Notre prochain et dernier didacticiel examine comment ajouter des contrôles de bouton Web pour les modèles de s contrôles DataList et Repeater afin de fournir des fonctionnalités personnalisées, initiée par l’utilisateur sur une base par élément.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été David Suru. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](paging-report-data-in-a-datalist-or-repeater-control-cs.md)
> [Suivant](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
