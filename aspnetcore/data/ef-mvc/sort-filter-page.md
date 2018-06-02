---
title: ASP.NET Core MVC avec EF Core - Tri, filtre, changement de page - 3 sur 10
author: rick-anderson
description: Dans ce didacticiel, vous allez ajouter des fonctionnalités de tri, de filtrage et de changement de page à une page à l’aide d’ASP.NET Core et d’Entity Framework Core.
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 344e3a1806ff21d8ce335b2b407a8a93baf72c1b
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2018
---
# <a name="aspnet-core-mvc-with-ef-core---sort-filter-paging---3-of-10"></a>ASP.NET Core MVC avec EF Core - Tri, filtre, changement de page - 3 sur 10

Par [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)

L’exemple d’application web Contoso University montre comment créer des applications web ASP.NET Core MVC avec Entity Framework Core et Visual Studio. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](intro.md).

Dans le didacticiel précédent, vous avez implémenté un ensemble de pages web pour les opérations CRUD de base pour les entités Student. Dans ce didacticiel, vous allez ajouter les fonctionnalités de tri, de filtrage et de changement de page à la page d’index des étudiants. Vous allez également créer une page qui effectue un regroupement simple.

L’illustration suivante montre à quoi ressemblera la page quand vous aurez terminé. Les en-têtes des colonnes sont des liens sur lesquels l’utilisateur peut cliquer pour trier selon les colonnes. Cliquer de façon répétée sur un en-tête de colonne permet de changer l’ordre de tri (croissant ou décroissant).

![Page d’index des étudiants](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Ajouter des liens de tri de colonne à la page d’index des étudiants

Pour ajouter le tri à la page d’index des étudiants, vous allez modifier la méthode `Index` du contrôleur Students et ajouter du code à la vue de l’index des étudiants.

### <a name="add-sorting-functionality-to-the-index-method"></a>Ajouter la fonctionnalité de tri à la méthode Index

Dans *StudentsController.cs*, remplacez la méthode `Index` par le code suivant :

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

Ce code reçoit un paramètre `sortOrder` à partir de la chaîne de requête dans l’URL. La valeur de chaîne de requête est fournie par ASP.NET Core MVC en tant que paramètre à la méthode d’action. Le paramètre sera la chaîne « Name » ou « Date », éventuellement suivie d’un trait de soulignement et de la chaîne « desc » pour spécifier l’ordre décroissant. L'ordre de tri par défaut est le tri croissant.

La première fois que la page d’index est demandée, il n’y a pas de chaîne de requête. Les étudiants sont affichés dans l’ordre croissant par leur nom, ce qui correspond au paramétrage par défaut de l’instruction `switch`. Quand l’utilisateur clique sur un lien hypertexte d’en-tête de colonne, la valeur `sortOrder` appropriée est fournie dans la chaîne de requête.

Les deux éléments `ViewData` (NameSortParm et DateSortParm) sont utilisés par la vue pour configurer les liens hypertexte d’en-tête de colonne avec les valeurs de chaîne de requête appropriées.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

Il s’agit d’instructions ternaires. La première spécifie que si le paramètre `sortOrder` est null ou vide, NameSortParm doit être défini sur « name_desc » ; sinon, il doit être défini sur une chaîne vide. Ces deux instructions permettent à la vue de définir les liens hypertexte d’en-tête de colonne comme suit :

|  Ordre de tri actuel  | Lien hypertexte Nom de famille | Lien hypertexte Date |
|:--------------------:|:-------------------:|:--------------:|
| Nom de famille croissant  | descending          | ascending      |
| Nom de famille décroissant | ascending           | ascending      |
| Date croissante       | ascending           | descending     |
| Date décroissante      | ascending           | ascending      |

La méthode utilise LINQ to Entities pour spécifier la colonne d’après laquelle effectuer le tri. Le code crée une variable `IQueryable` avant l’instruction switch, la modifie dans l’instruction switch et appelle la méthode `ToListAsync` après l’instruction `switch`. Lorsque vous créez et modifiez des variables `IQueryable`, aucune requête n’est envoyée à la base de données. La requête n’est pas exécutée tant que vous ne convertissez pas l’objet `IQueryable` en collection en appelant une méthode telle que `ToListAsync`. Par conséquent, ce code génère une requête unique qui n’est pas exécutée avant l’instruction `return View`.

Ce code peut devenir très détaillé avec un grand nombre de colonnes. [Le dernier didacticiel de cette série](advanced.md#dynamic-linq) montre comment écrire du code qui vous permet de transmettre le nom de la colonne `OrderBy` dans une variable chaîne.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Ajouter des liens hypertexte d’en-tête de colonne dans la vue de l’index des étudiants

Remplacez le code dans *Views/Students/Index.cshtml* par le code suivant pour ajouter des liens hypertexte d’en-tête de colonne. Les lignes modifiées apparaissent en surbrillance.

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

Ce code utilise les informations figurant dans les propriétés `ViewData` pour configurer des liens hypertexte avec les valeurs de chaîne de requête appropriées.

Exécutez l’application, sélectionnez l’onglet **Students**, puis cliquez sur les en-têtes des colonnes **Last Name** et **Enrollment Date** pour vérifier que le tri fonctionne.

![Page d’index des étudiants dans l’ordre de leurs noms](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Ajouter une zone de recherche à la page d’index des étudiants

Pour ajouter le filtrage à la page d’index des étudiants, vous allez ajouter une zone de texte et un bouton d’envoi à la vue et apporter les modifications correspondantes dans la méthode `Index`. La zone de texte vous permet d’entrer une chaîne à rechercher dans les champs de prénom et de nom.

### <a name="add-filtering-functionality-to-the-index-method"></a>Ajouter la fonctionnalité de filtrage à la méthode Index

Dans *StudentsController.cs*, remplacez la méthode `Index` par le code suivant (les modifications apparaissent en surbrillance).

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

Vous avez ajouté un paramètre `searchString` à la méthode `Index`. La valeur de chaîne de recherche est reçue à partir d’une zone de texte que vous ajouterez à la vue Index. Vous avez également ajouté à l’instruction LINQ une clause where qui sélectionne uniquement les étudiants dont le prénom ou le nom contient la chaîne de recherche. L’instruction qui ajoute la clause where est exécutée uniquement s’il existe une valeur à rechercher.

> [!NOTE]
> Ici, vous appelez la méthode `Where` sur un objet `IQueryable`, et le filtre sera traité sur le serveur. Dans certains scénarios, vous pouvez appeler la méthode `Where` en tant que méthode d’extension sur une collection en mémoire. (Par exemple, supposons que vous changez la référence à `_context.Students` de sorte qu’à la place d’un `DbSet` EF, elle fasse référence à une méthode de référentiel qui renvoie une collection `IEnumerable`.) Le résultat serait normalement le même, mais pourrait être différent dans certains cas.
>
>Par exemple, l’implémentation par le .NET Framework de la méthode `Contains` effectue une comparaison respectant la casse par défaut, mais dans SQL Server, cela est déterminé par le paramètre de classement de l’instance SQL Server. Ce paramètre définit par défaut le non-respect de la casse. Vous pouvez appeler la méthode `ToUpper` pour rendre explicitement le test sensible à la casse :  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*. Cela garantit que les résultats resteront les mêmes si vous modifiez le code ultérieurement pour utiliser un référentiel qui renverra une collection `IEnumerable` à la place d’un objet `IQueryable`. (Lorsque vous appelez la méthode `Contains` sur une collection `IEnumerable`, vous obtenez l’implémentation du .NET Framework ; lorsque vous l’appelez sur un objet `IQueryable`, vous obtenez l’implémentation du fournisseur de base de données.) Toutefois, cette solution présente un coût en matière de performances. Le code `ToUpper` place une fonction dans la clause WHERE de l’instruction TSQL SELECT. Elle empêche l’optimiseur d’utiliser un index. Étant donné que SQL est généralement installé comme non sensible à la casse, il est préférable d’éviter le code `ToUpper` jusqu’à ce que vous ayez migré vers un magasin de données qui respecte la casse.

### <a name="add-a-search-box-to-the-student-index-view"></a>Ajouter une zone de recherche à la vue de l’index des étudiants

Dans *Views/Student/Index.cshtml*, ajoutez le code en surbrillance immédiatement avant la balise d’ouverture de table afin de créer une légende, une zone de texte et un bouton de **recherche**.

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

Ce code utilise le [Tag Helper](xref:mvc/views/tag-helpers/intro) `<form>` pour ajouter le bouton et la zone de texte de recherche. Par défaut, le Tag Helper `<form>` envoie les données de formulaire avec un POST, ce qui signifie que les paramètres sont transmis dans le corps du message HTTP et non pas dans l’URL sous forme de chaînes de requête. Lorsque vous spécifiez HTTP GET, les données de formulaire sont transmises dans l’URL sous forme de chaînes de requête, ce qui permet aux utilisateurs d’ajouter l’URL aux favoris. Les recommandations du W3C stipulent que vous devez utiliser GET quand l’action ne produit pas de mise à jour.

Exécutez l’application, sélectionnez l’onglet **Students**, entrez une chaîne de recherche, puis cliquez sur Rechercher pour vérifier que le filtrage fonctionne.

![Page d’index des étudiants avec filtrage](sort-filter-page/_static/filtering.png)

Notez que l’URL contient la chaîne de recherche.

```html
http://localhost:5813/Students?SearchString=an
```

Si vous marquez cette page d’un signet, vous obtenez la liste filtrée lorsque vous utilisez le signet. L’ajout de `method="get"` dans la balise `form` est ce qui a provoqué la génération de la chaîne de requête.

À ce stade, si vous cliquez sur un lien de tri d’en-tête de colonne, vous perdez la valeur de filtre que vous avez entrée dans la zone **Rechercher**. Vous corrigerez cela dans la section suivante.

## <a name="add-paging-functionality-to-the-students-index-page"></a>Ajouter la fonctionnalité de changement de page à la page d’index des étudiants

Pour ajouter le changement de page à la page d’index des étudiants, vous allez créer une classe `PaginatedList` qui utilise les instructions `Skip` et `Take` pour filtrer les données sur le serveur au lieu de toujours récupérer toutes les lignes de la table. Ensuite, vous apporterez des modifications supplémentaires dans la méthode `Index` et ajouterez des boutons de changement de page dans la vue `Index`. L’illustration suivante montre les boutons de changement de page.

![Page d’index des étudiants avec liens de changement de page](sort-filter-page/_static/paging.png)

Dans le dossier du projet, créez `PaginatedList.cs`, puis remplacez le code du modèle par le code suivant.

[!code-csharp[](intro/samples/cu/PaginatedList.cs)]

La méthode `CreateAsync` de ce code accepte la taille de page et le numéro de page, et applique les instructions `Skip` et `Take` appropriées à `IQueryable`. Quand la méthode `ToListAsync` est appelée sur `IQueryable`, elle renvoie une liste contenant uniquement la page demandée. Les propriétés `HasPreviousPage` et `HasNextPage` peuvent être utilisées pour activer ou désactiver les boutons de changement de page **Précédent** et **Suivant**.

Une méthode `CreateAsync` est utilisée à la place d’un constructeur pour créer l’objet `PaginatedList<T>`, car les constructeurs ne peuvent pas exécuter de code asynchrone.

## <a name="add-paging-functionality-to-the-index-method"></a>Ajouter la fonctionnalité de changement de page à la méthode Index

Dans *StudentsController.cs*, remplacez la méthode `Index` par le code suivant.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

Ce code ajoute un paramètre de numéro de page, un paramètre d’ordre de tri actuel et un paramètre de filtre actuel à la signature de la méthode.

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

La première fois que la page s’affiche, ou si l’utilisateur n’a pas cliqué sur un lien de changement de page ni de tri, tous les paramètres sont Null.  Si l’utilisateur clique sur un lien de changement de page, la variable de page contient le numéro de page à afficher.

L’élément `ViewData` nommé CurrentSort fournit à l’affichage l’ordre de tri actuel, car il doit être inclus dans les liens de changement de page pour que l’ordre de tri soit conservé lors du changement de page.

L’élément `ViewData` nommé CurrentFilter fournit à la vue la chaîne de filtre actuelle. Cette valeur doit être incluse dans les liens de changement de page pour que les paramètres de filtre soient conservés lors du changement de page, et elle doit être restaurée dans la zone de texte lorsque la page est réaffichée.

Si la chaîne de recherche est modifiée au cours du changement de page, la page doit être réinitialisée à 1, car le nouveau filtre peut entraîner l’affichage de données différentes. La chaîne de recherche est modifiée quand une valeur est entrée dans la zone de texte et que le bouton d’envoi est enfoncé. Dans ce cas, le paramètre `searchString` n’est pas Null.

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

À la fin de la méthode `Index`, la méthode `PaginatedList.CreateAsync` convertit la requête d’étudiant en une page individuelle d’étudiants dans un type de collection qui prend en charge le changement de page. Cette page individuelle d’étudiants est alors transmise à la vue.

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

La méthode `PaginatedList.CreateAsync` accepte un numéro de page. Les deux points d’interrogation représentent l’opérateur de fusion Null. L’opérateur de fusion Null définit une valeur par défaut pour un type nullable ; l’expression `(page ?? 1)` indique de renvoyer la valeur de `page` si elle a une valeur, ou de renvoyer 1 si `page` a la valeur Null.

## <a name="add-paging-links-to-the-student-index-view"></a>Ajouter des liens de changement de page à la vue de l’index des étudiants

Dans *Views/Instructors/Index.cshtml*, remplacez le code existant par le code suivant. Les modifications apparaissent en surbrillance.

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

L’instruction `@model` en haut de la page spécifie que la vue obtient désormais un objet `PaginatedList<T>` à la place d’un objet `List<T>`.

Les liens d’en-tête de colonne utilisent la chaîne de requête pour transmettre la chaîne de recherche actuelle au contrôleur afin que l’utilisateur puisse trier les résultats de filtrage :

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

Les boutons de changement de page sont affichés par des Tag Helpers :

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

Exécutez l’application et accédez à la page des étudiants.

![Page d’index des étudiants avec liens de changement de page](sort-filter-page/_static/paging.png)

Cliquez sur les liens de changement de page dans différents ordres de tri pour vérifier que le changement de page fonctionne. Ensuite, entrez une chaîne de recherche et essayez de changer de page à nouveau pour vérifier que le changement de page fonctionne correctement avec le tri et le filtrage.

## <a name="create-an-about-page-that-shows-student-statistics"></a>Créer une page About qui affiche les statistiques des étudiants

Pour la page **About** du site web de Contoso University, vous afficherez le nombre d’étudiants inscrits pour chaque date d’inscription. Cela nécessite un regroupement et des calculs simples sur les groupes. Pour ce faire, vous devez effectuer les opérations suivantes :

* Créez une classe de modèle de vue pour les données que vous devez transmettre à la vue.

* Modifiez la méthode About dans le contrôleur Home.

* Modifiez la vue About.

### <a name="create-the-view-model"></a>Créer le modèle de vue

Créez un dossier *SchoolViewModels* dans le dossier *Models*.

Dans le nouveau dossier, ajoutez un fichier de classe *EnrollmentDateGroup.cs* et remplacez le code du modèle par le code suivant :

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a>Modifier le contrôleur Home

Dans *HomeController.cs*, ajoutez les instructions using suivantes en haut du fichier :

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

Ajoutez une variable de classe pour le contexte de base de données immédiatement après l’accolade ouvrante de la classe et obtenez une instance du contexte à partir d’ASP.NET Core DI :

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

Remplacez la méthode `About` par le code suivant :

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

L’instruction LINQ regroupe les entités Student par date d’inscription, calcule le nombre d’entités dans chaque groupe et stocke les résultats dans une collection d’objets de modèle de vue `EnrollmentDateGroup`.
> [!NOTE] 
> Dans la version 1.0 d’Entity Framework Core, le jeu de résultats entier est renvoyé au client et le regroupement est effectué sur le client. Dans certains scénarios, cela peut générer des problèmes de performances. Veillez à tester les performances avec des volumes de données de production et, si nécessaire, utilisez des requêtes SQL brutes pour effectuer le regroupement sur le serveur. Pour obtenir des informations sur la façon d’utiliser des requêtes SQL brutes, consultez [le dernier didacticiel de cette série](advanced.md).

### <a name="modify-the-about-view"></a>Modifier la vue About

Remplacez le code du fichier *Views/Home/About.cshtml* par le code suivant :

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

Exécutez l’application et accédez à la page About. Le nombre d’étudiants pour chaque date d’inscription s’affiche dans une table.

![Page About](sort-filter-page/_static/about.png)

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez vu comment effectuer un tri, un filtrage, un changement de page et un regroupement. Dans le prochain didacticiel, vous allez apprendre à gérer les modifications du modèle de données à l’aide de migrations.

> [!div class="step-by-step"]
> [Précédent](crud.md)
> [Suivant](migrations.md)  
