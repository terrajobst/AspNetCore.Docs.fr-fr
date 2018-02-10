---
title: "ASP.NET Core MVC avec EF Core - trier, filtrer, la pagination - 3 sur 10"
author: tdykstra
description: "Dans ce didacticiel, vous allez ajouter le tri, le filtre et la pagination à une la page à l’aide d’ASP.NET Core et Entity Framework Core."
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: feb4a50c9e5602064e7d493b6991485949903f47
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-aspnet-core-mvc-tutorial-3-of-10"></a>Le tri, le filtre, la pagination et le regroupement - Didacticiel EF Core avec ASP.NET Core MVC (partie 3 sur 10)

Par [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)

L’exemple d’application web Contoso University montre comment créer des applications web ASP.NET Core MVC à l’aide d’Entity Framework Core et Visual Studio. Pour plus d’informations sur la série de didacticiels, consultez [le premier didacticiel de la série](intro.md).

Dans le didacticiel précédent, vous avez implémenté un ensemble de pages web pour les opérations CRUD de base pour les entités Student. Dans ce didacticiel, vous allez ajouter le tri, le filtre et la fonctionnalité de pagination à la page d’Index des étudiants. Vous allez également créer une page qui effectue le regroupement simple.

L’illustration suivante montre à quoi ressemblera la page une lorsque vous avez terminé. Les en-têtes de colonne sont des liens sur lesquels l’utilisateur peut cliquer pour trier par colonne. Cliquer sur un en-tête de colonne à plusieurs reprises bascule entre l'ordre de tri croissant et décroissant.

![Page d’index les étudiants](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Ajouter des liens de tri de colonne à la Page d’Index des étudiants

Pour ajouter le tri à la page d’Index des étudiants, vous allez modifier la méthode `Index` du contrôleur Student et ajouter du code à la vue de l’Index des étudiants.

### <a name="add-sorting-functionality-to-the-index-method"></a>Ajouter les fonctionnalités de tri à la méthode Index

Dans *StudentsController.cs*, remplacez la méthode `Index` avec le code suivant :

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

Ce code reçoit un paramètre `sortOrder` à partir de la chaîne de requête dans l’URL. La valeur de chaîne de requête est fournie par ASP.NET Core MVC en tant que paramètre à la méthode d’action. Le paramètre sera une chaîne qui est « Name » ou « Date », éventuellement suivie d’un underscore et de la chaîne « desc » pour spécifier l’ordre décroissant. L'ordre de tri par défaut est le tri croissant.

La première fois que la page d’Index est demandée, il n’aucune chaîne de requête. Les étudiants sont affichés dans l’ordre croissant par nom, qui est la valeur par défaut établie dans ce cas dans l'instruction `switch`. Lorsque l’utilisateur clique sur le lien hypertexte de la colonne titre, la valeur `sortOrder` appropriée est fournie dans la chaîne de requête.

Les deux éléments `ViewData`(NameSortParm et DateSortParm) sont utilisés par la vue pour configurer des liens hypertexte du titre de colonne avec les valeurs de chaîne de requête appropriée.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

Il s’agit d’instructions ternaires. La première condition spécifie que si le paramètre `sortOrder` est null ou vide, NameSortParm doit être défini sur « name_desc » ; sinon, il doit être défini sur une chaîne vide. Ces deux instructions permettent à la view de définir les liens hypertexte de titre de colonne comme suit :

|  Ordre de tri en cours  | Lien hypertexte du nom | Lien hypertexte de la date |
|:--------------------:|:-------------------:|:--------------:|
| par ordre croissant du nom  | descending          | ascending      |
| par ordre décroissant du nom  | ascending           | ascending      |
| par ordre croissant de la date      | ascending           | descending     |
| par ordre croissant de la date      | ascending           | ascending      |

La méthode utilise LINQ to Entities pour spécifier la colonne à trier. Le code crée une variable `IQueryable` avant l’instruction switch, il modifie dans l’instruction switch et appelle la méthode `ToListAsync` après l'instruction `switch`. Lorsque vous créez et modifiez des variables `IQueryable`, aucune requête n’est envoyée à la base de données. La requête n’est pas exécutée tant que vous convertissez l'objet `IQueryable` dans une collection en appelant une méthode comme `ToListAsync`. Par conséquent, ce code génère une requête unique qui n’est pas exécutée jusqu'à l'instruction `return View`.

Ce code peut devenir verbeux avec un grand nombre de colonnes. [Le dernier didacticiel de cette série](advanced.md#dynamic-linq) montre comment écrire du code qui vous permet de passer le nom de la colonne `OrderBy` dans une variable de chaîne.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Ajouter des liens hypertexte de titre de colonne à la vue de l’Index des étudiants

Remplacez le code dans *Views/Students/Index.cshtml*, avec le code suivant pour ajouter des liens hypertexte de titre de colonne. Les lignes modifiées sont mises en surbrillance.

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

Ce code utilise les informations contenues dans les propriétés `ViewData` pour configurer des liens hypertexte avec les valeurs de chaîne de la requête appropriées.

Exécuter l’application, sélectionnez l'onglet **Students**, puis cliquez sur les en-têtes **nom** et **Date d’inscription** de colonne pour vérifier que le tri fonctionne.

![Page d’index étudiants dans l’ordre de nom](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Ajouter une zone de recherche à la page d’Index des étudiants

Pour ajouter le filtre sur la page d’Index des étudiants, vous allez ajouter une zone de texte et un bouton d’envoi à la vue et apporter les modifications correspondantes dans la méthode `Index`. La zone de texte vous permet d’entrer une chaîne à rechercher dans les champs prénom et nom.

### <a name="add-filtering-functionality-to-the-index-method"></a>Ajouter des fonctionnalités de filtre à la méthode Index

Dans *StudentsController.cs*, remplacez la méthode `Index` avec le code suivant (les modifications sont mises en surbrillance).

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

Vous avez ajouté un paramètre `searchString` à la méthode `Index`. La valeur de chaîne de recherche est reçue à partir d’une zone de texte que vous ajouterez à la vue Index. Vous avez également ajouté à l’instruction LINQ where clause qui sélectionne uniquement les étudiants dont le prénom ou le nom contient la chaîne de recherche. L’instruction qui ajoute la clause where est exécutée uniquement s’il existe une valeur à rechercher.

> [!NOTE]
> Ici vous appelez le méthode `Where` sur une objet `IQueryable` et le filtre seront traités sur le serveur. Dans certains scénarios vous pouvez appeler la méthode `Where` comme méthode d’extension sur une collection en mémoire. (Par exemple, supposons que vous modifiez la référence à `_context.Students` ainsi qu’au lieu d’un `DbSet` EF il fait référence à une méthode de référentiel qui retourne une collection `IEnumerable`.) Le résultat serait normalement le même, mais dans certains cas, peut être différent.
>
>Par exemple, l’implémentation du .NET Framework de la méthode `Contains` effectue une comparaison en respectant la casse par défaut, mais dans SQL Server, cela est déterminé par le paramètre de classement de l’instance de SQL Server. Ce paramètre par défaut à la casse. Vous pouvez appeler la méthode `ToUpper` pour rendre le test exeplictement non sensible à la casse : *Where(s = > s.LastName.ToUpper(). Contains(searchString.ToUpper())*. Cel permet de s’assurer que les résultats restent les mêmes si vous modifiez le code ultérieurement pour utiliser un référentiel qui retourne une collection `IEnumerable` au lieu d’un objet `IQueryable`. (Lorsque vous appelez une méthode `Contains` sur une collection `IEnumerable`, vous obtenez l’implémentation du .NET Framework ; lorsque vous l'appelez sur un objet `IQueryable`, vous obtenez l’implémentation du fournisseur de base de données.) Toutefois, il est d’une baisse des performances de cette solution. Le code `ToUpper` placerait une fonction dans la clause WHERE de l’instruction SELECT de TSQL. Cela empêche l’optimiseur d’utiliser un index. Étant donné que SQL est installé principalement sans respecter la casse, il est préférable d’éviter le `ToUpper` code jusqu'à ce que vous migrez vers un magasin de données qui respecte la casse.

### <a name="add-a-search-box-to-the-student-index-view"></a>Ajouter une zone de recherche à la vue d’Index étudiant

Dans *Views/Student/Index.cshtml*, ajoutez le code en surbrillance immédiatement avant l’ouverture de balise table afin de créer une légende, une zone de texte et un **recherche** bouton.

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

Ce code utilise le [Tag helper](xref:mvc/views/tag-helpers/intro) `<form>` pour ajouter la zone de texte de recherche et le bouton. Par défaut, le Tag helper `<form>` envoie des données de formulaire avec un POST, ce qui signifie que les paramètres sont passés dans le corps du message HTTP et non dans l’URL sous forme de chaînes de requête. Lorsque vous spécifiez HTTP GET, les données du formulaire sont passées dans l’URL sous forme de chaînes de requête, ce qui permet aux utilisateurs de faire un bookmark de l’URL. Les instructions W3C recommandent d'utiliser GET lorsque l’action ne produit pas une mise à jour.

Exécuter l’application, sélectionnez l'onglet **Students**, entrez une chaîne de recherche, puis cliquez sur Rechercher pour vérifier que le filtre fonctionne.

![Page d’index de stagiaires de filtrage](sort-filter-page/_static/filtering.png)

Notez que l’URL contient la chaîne de recherche.

```html
http://localhost:5813/Students?SearchString=an
```

Si vous créez un signet de cette page, vous obtenez la liste filtrée lorsque vous utilisez le signet. Ajout de `method="get"` à la `form` balise est ce qui a provoqué la chaîne de requête à générer.

À ce stade, si vous cliquez sur un lien de tri de titre de colonne vous perdez la valeur de filtre que vous avez entré dans le **recherche** boîte. Vous allez résoudre cela dans la section suivante.

## <a name="add-paging-functionality-to-the-students-index-page"></a>Ajouter la fonctionnalité de pagination à la page d’Index des étudiants

Pour ajouter la pagination à la page d’Index des étudiants, vous allez créer une classe `PaginatedList` qui utilise les instructions `Skip` et `Take` pour filtrer les données sur le serveur au lieu de toujours récupérer toutes les lignes de la table. Ensuite, vous allez apporter des modifications supplémentaires dans la méthode `Index` et ajouter des boutons de la pagination à la vue `Index`. L’illustration suivante montre les boutons de la pagination.

![Page avec des liens de pagination d’index les étudiants](sort-filter-page/_static/paging.png)

Dans le dossier du projet, créez `PaginatedList.cs`, puis remplacez le code du modèle par le code suivant.

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

La méthode `CreateAsync` dans ce code prend la taille de la page et le numéro de page et applique les instructions `Skip` et `Take` pour le `IQueryable`. Lorsque `ToListAsync` est appelée sur le `IQueryable`, il retourne une liste contenant uniquement la page demandée. Les propriétés `HasPreviousPage` et `HasNextPage` peuvent être utilisées pour activer ou désactiver les boutons de pagination **précédent** et **suivant** .

Une méthode `CreateAsync` est utilisée au lieu d’un constructeur pour créer l’objet `PaginatedList<T>`, car les constructeurs ne peuvent pas exécuter du code asynchrone.

## <a name="add-paging-functionality-to-the-index-method"></a>Ajouter la fonctionnalité de pagination pour la méthode Index

Dans *StudentsController.cs*, remplacez la méthode `Index` avec le code suivant.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

Ce code ajoute un paramètre de numéro de page, un paramètre de commande de tri actuelle et un paramètre de filtre actuel pour la signature de méthode.

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

La première fois que la page s’affiche, ou si l’utilisateur n’a pas cliqué sur un échange ou le lien de tri, tous les paramètres sont null. Si un lien de la pagination est activé, la variable de page contient le numéro de page à afficher.

L'élément `ViewData` nommé CurrentSort fournit l’ordre de tri actuel à la vue, car il doit être inclus dans les liens de la pagination afin de conserver l’ordre de tri lors de la pagination.

L'élément `ViewData` nommé CurrentFilter fournit la chaîne de filtre actuel à la vue. Cette valeur doit être incluse dans les liens de la pagination afin de conserver les paramètres de filtre lors de la pagination, et elle doit être restaurée à la zone de texte lorsque la page s’affiche de nouveau.

Si la chaîne de recherche est modifiée au cours de la pagination, la page doit être réinitialisé à 1, parce que le nouveau filtre pour afficher des données différentes. La chaîne de recherche est modifiée quand une valeur est entrée dans la zone de texte et le bouton d’envoi est enfoncé. Dans ce cas, le paramètre `searchString` n’est pas null.

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

À la fin de la méthode `Index`, la méthode `PaginatedList.CreateAsync` convertit la requête de student à une seule page d’étudiants dans un type de collection qui prend en charge la pagination. Cette page unique des étudiants est ensuite passée à la vue.

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

Le méthode `PaginatedList.CreateAsync` prend un numéro de page. Les deux points d’interrogation représentent l’opérateur de fusion null. L’opérateur de fusion null définit une valeur par défaut pour un type nullable ; l’expression `(page ?? 1)` signifie de retourner la valeur de `page` si elle a une valeur, ou retourne 1 si `page` a la valeur null.

## <a name="add-paging-links-to-the-student-index-view"></a>Ajouter des liens de pagination à la vue Student Index

Dans *Views/Students/Index.cshtml*, remplacez le code existant par le code suivant. Les modifications sont mises en surbrillance.

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

La déclaration `@model` en haut de la page spécifie que la vue obtient désormais un objet `PaginatedList<T>` au lieu d’un `List<T>` objet.

Les liens d’en-tête de colonne permettent de passer la chaîne de recherche en cours au contrôleur afin que l’utilisateur puisse trier les résultats de filtre de la chaîne de requête :

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

Les boutons de pagination sont affichés par les tag helpers :

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

![Page avec des liens de pagination d’index des étudiants](sort-filter-page/_static/paging.png)

Cliquez sur les liens de la pagination dans différents ordres de tri pour s’assurer que la pagination fonctionne. Entrez une chaîne de recherche, puis recommencez la pagination pour vérifier que la pagination fonctionne également correctement avec le tri et le filtre.

## <a name="create-an-about-page-that-shows-student-statistics"></a>Créer une page à propos qui affiche les statistiques des étudiants

Pour la page **About** du site de Web Contoso University, vous afficherez le nombre d’étudiants inscrits pour chaque date d’inscription. Cela nécessite des regroupements simples et des calculs sur les groupes. Pour ce faire, vous devez procédez comme suit :

* Créez une classe de modèle d’affichage pour les données dont vous avez besoin pour passer à la vue.

* Modifiez la méthode à propos du contrôleur Home.

* Modifiez la vue About.

### <a name="create-the-view-model"></a>Créer le modèle d’affichage

Créer un dossier *SchoolViewModels* dans le dossier *Models*.

Dans le nouveau dossier, ajoutez un fichier de classe *EnrollmentDateGroup.cs* et remplacez le code du modèle par le code suivant :

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a>Modifier le contrôleur Home

Dans *HomeController.cs*, ajoutez le code suivant à l’aide d’instructions en haut du fichier :

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

Ajoutez une variable de classe pour le contexte de base de données immédiatement après l’accolade ouvrante de la classe et obtenez une instance du contexte à partir de l'injection de dépendance ASP.NET Core :

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

Remplacez la méthode `About` par le code suivant :

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

L’instruction LINQ regroupe les entités étudiant par date d’inscription, calcule le nombre d’entités dans chaque groupe et stocke les résultats dans une collection des objets de modèles de vue `EnrollmentDateGroup`.
> [!NOTE] 
> Dans la 1.0 version de Entity Framework Core, le jeu de résultats entier est retourné au client, et le regroupement est effectué sur le client. Dans certains scénarios, cela pourrait créer des problèmes de performances. Veillez à tester les performances avec les volumes de données de production et si nécessaire utilisez du SQL pour effectuer le regroupement sur le serveur. Pour plus d’informations sur l’utilisation de SQL, consultez [le dernier didacticiel de cette série](advanced.md).

### <a name="modify-the-about-view"></a>Modifier la vue About

Remplacez le code dans le fichier *Views/Home/About.cshtml* avec le code suivant :

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

Exécutez l’application et accédez à la page About. Le nombre d’étudiants pour chaque date d’inscription s’affiche dans une table.

![Sur la page](sort-filter-page/_static/about.png)

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez vu comment effectuer le tri, le filtre, la pagination et le regroupement. Dans l’étape suivante du didacticiel, vous allez apprendre à gérer les modifications du modèle de données à l’aide des migrations.

>[!div class="step-by-step"]
[Précédent](crud.md)
[Suivant](migrations.md)  
