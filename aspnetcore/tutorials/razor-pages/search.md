---
title: Ajouter une fonction de recherche aux pages Razor dans ASP.NET Core
author: rick-anderson
description: Montre comment ajouter une fonction de recherche à des pages Razor dans ASP.NET Core MVC
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/search
ms.openlocfilehash: 80292f8cfecd5363fb8acc8578f9bb0ca9ee5969
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090161"
---
# <a name="add-search-to-aspnet-core-razor-pages"></a><span data-ttu-id="008fd-103">Ajouter une fonction de recherche aux pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="008fd-103">Add search to ASP.NET Core Razor Pages</span></span>

<span data-ttu-id="008fd-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="008fd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="008fd-105">Dans ce document, la fonction de recherche est ajoutée à la page Index qui permet la recherche de films par *genre* ou par *nom*.</span><span class="sxs-lookup"><span data-stu-id="008fd-105">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="008fd-106">Ajoutez les propriétés en surbrillance suivantes à *Pages/Movies/Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="008fd-106">Add the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

::: moniker-end

* <span data-ttu-id="008fd-107">`SearchString` : contient le texte que les utilisateurs entrent dans la zone de texte de recherche.</span><span class="sxs-lookup"><span data-stu-id="008fd-107">`SearchString`: contains the text users enter in the search text box.</span></span>
* <span data-ttu-id="008fd-108">`Genres` : contient la liste des genres.</span><span class="sxs-lookup"><span data-stu-id="008fd-108">`Genres`: contains the list of genres.</span></span> <span data-ttu-id="008fd-109">Cela permet à l’utilisateur de sélectionner un genre dans la liste.</span><span class="sxs-lookup"><span data-stu-id="008fd-109">This allows the user to select a genre from the list.</span></span>
* <span data-ttu-id="008fd-110">`MovieGenre` : contient le genre spécifique sélectionné par l’utilisateur (par exemple, « Western »).</span><span class="sxs-lookup"><span data-stu-id="008fd-110">`MovieGenre`: contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="008fd-111">Vous allez utiliser les propriétés `Genres` et `MovieGenre` plus loin dans ce document.</span><span class="sxs-lookup"><span data-stu-id="008fd-111">You'll work with the `Genres` and `MovieGenre` properties later in this document.</span></span>

<span data-ttu-id="008fd-112">Mettez à jour la méthode `OnGetAsync` de la page Index avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="008fd-112">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="008fd-113">La première ligne de la méthode `OnGetAsync` crée une requête [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) pour sélectionner les films :</span><span class="sxs-lookup"><span data-stu-id="008fd-113">The first line of the `OnGetAsync` method creates a [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="008fd-114">La requête est *seulement* définie à ce stade ; elle n’a **pas** été exécutée sur la base de données.</span><span class="sxs-lookup"><span data-stu-id="008fd-114">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="008fd-115">Si le paramètre `searchString` contient une chaîne, la requête de films est modifiée de façon à filtrer sur la chaîne de recherche :</span><span class="sxs-lookup"><span data-stu-id="008fd-115">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="008fd-116">Le code `s => s.Title.Contains()` est une [expression lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="008fd-116">The `s => s.Title.Contains()` code is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="008fd-117">Les expressions lambda sont utilisées dans les requêtes [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) basées sur une méthode comme arguments pour les méthodes d’opérateur de requête standard, telles que la méthode [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) ou `Contains` (utilisée dans le code précédent).</span><span class="sxs-lookup"><span data-stu-id="008fd-117">Lambdas are used in method-based [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="008fd-118">Les requêtes LINQ ne sont pas exécutées quand elles sont définies ou quand elles sont modifiées en appelant une méthode (comme `Where`, `Contains` ou `OrderBy`).</span><span class="sxs-lookup"><span data-stu-id="008fd-118">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="008fd-119">Au lieu de cela, l’exécution de la requête est différée.</span><span class="sxs-lookup"><span data-stu-id="008fd-119">Rather, query execution is deferred.</span></span> <span data-ttu-id="008fd-120">Cela signifie que l’évaluation d’une expression est retardée jusqu’à ce que sa valeur réalisée fasse l’objet d’une itération réelle ou que la méthode `ToListAsync` soit appelée.</span><span class="sxs-lookup"><span data-stu-id="008fd-120">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="008fd-121">Pour plus d’informations, consultez [Exécution de requête](/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span><span class="sxs-lookup"><span data-stu-id="008fd-121">See [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="008fd-122">**Remarque :** La méthode [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) est exécutée sur la base de données, et non pas dans le code C#.</span><span class="sxs-lookup"><span data-stu-id="008fd-122">**Note:** The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="008fd-123">Le respect de la casse pour la requête dépend de la base de données et du classement.</span><span class="sxs-lookup"><span data-stu-id="008fd-123">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="008fd-124">Sur SQL Server, `Contains` est mappée à [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), qui ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="008fd-124">On SQL Server, `Contains` maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="008fd-125">Dans SQLite, avec le classement par défaut, elle respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="008fd-125">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="008fd-126">Enfin, la dernière ligne de la méthode `OnGetAsync` renseigne la propriété `SearchString` avec la valeur de recherche de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="008fd-126">Lastly, the final line of the `OnGetAsync` method populates the `SearchString` property with the user's search value.</span></span> <span data-ttu-id="008fd-127">Une fois la propriété `SearchString` renseignée, la valeur de recherche est conservée dans la zone de recherche après l’exécution de la recherche.</span><span class="sxs-lookup"><span data-stu-id="008fd-127">With the `SearchString` property populated, the search value is retained in the search box after the search executes.</span></span>

<span data-ttu-id="008fd-128">Accédez à la page Movies, puis ajoutez une chaîne de requête telle que `?searchString=Ghost` à l’URL (par exemple, `http://localhost:5000/Movies?searchString=Ghost`).</span><span class="sxs-lookup"><span data-stu-id="008fd-128">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="008fd-129">Les films filtrés sont affichés.</span><span class="sxs-lookup"><span data-stu-id="008fd-129">The filtered movies are displayed.</span></span>

![Vue Index](search/_static/ghost.png)

<span data-ttu-id="008fd-131">Si le modèle d’itinéraire suivant est ajouté à la page d’index, la chaîne de recherche peut être passée comme un segment d’URL (par exemple, `http://localhost:5000/Movies/Ghost`).</span><span class="sxs-lookup"><span data-stu-id="008fd-131">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/Ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="008fd-132">La contrainte d’itinéraire précédente permet de rechercher le titre comme données d’itinéraire (un segment d’URL) et non comme valeur de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="008fd-132">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="008fd-133">Le `?` dans `"{searchString?}"` signifie qu’il s’agit d’un paramètre d’itinéraire facultatif.</span><span class="sxs-lookup"><span data-stu-id="008fd-133">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Vue Index avec le mot « ghost » ajouté à l’URL et une liste de films retournée contenant deux films, Ghostbusters et Ghostbusters 2](search/_static/g2.png)

<span data-ttu-id="008fd-135">Cependant, vous ne pouvez pas attendre des utilisateurs qu’ils modifient l’URL pour rechercher un film.</span><span class="sxs-lookup"><span data-stu-id="008fd-135">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="008fd-136">Dans cette étape, une interface utilisateur est ajoutée pour filtrer les films.</span><span class="sxs-lookup"><span data-stu-id="008fd-136">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="008fd-137">Si vous avez ajouté la contrainte d’itinéraire `"{searchString?}"`, supprimez-la.</span><span class="sxs-lookup"><span data-stu-id="008fd-137">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="008fd-138">Ouvrez le fichier *Pages/Movies/Index.cshtml*, puis ajoutez la balise `<form>` mise en surbrillance dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="008fd-138">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="008fd-139">La balise HTML `<form>` utilise le [Tag Helper de formulaire](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="008fd-139">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="008fd-140">Quand le formulaire est envoyé, la chaîne de filtrage est envoyée à la page *Pages/Movies/Index*.</span><span class="sxs-lookup"><span data-stu-id="008fd-140">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="008fd-141">Enregistrez les modifications apportées, puis testez le filtre.</span><span class="sxs-lookup"><span data-stu-id="008fd-141">Save the changes and test the filter.</span></span>

![Vue Index avec le mot « ghost » tapé dans la zone de texte de filtre des titres](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="008fd-143">Rechercher par genre</span><span class="sxs-lookup"><span data-stu-id="008fd-143">Search by genre</span></span>

<span data-ttu-id="008fd-144">Mettez à jour la méthode `OnGetAsync` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="008fd-144">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="008fd-145">Le code suivant est une requête LINQ qui récupère tous les genres dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="008fd-145">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="008fd-146">La liste `SelectList` de genres est créée en projetant des différents genres.</span><span class="sxs-lookup"><span data-stu-id="008fd-146">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There's no start line.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="008fd-147">Ajout d’une fonction de recherche par genre</span><span class="sxs-lookup"><span data-stu-id="008fd-147">Adding search by genre</span></span>

<span data-ttu-id="008fd-148">Mettez à jour *Index.cshtml* comme suit :</span><span class="sxs-lookup"><span data-stu-id="008fd-148">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="008fd-149">Testez l’application en effectuant une recherche par genre, par titre de film et selon ces deux critères.</span><span class="sxs-lookup"><span data-stu-id="008fd-149">Test the app by searching by genre, by movie title, and by both.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="008fd-150">[Précédent : Mise à jour des pages](xref:tutorials/razor-pages/da1)
> [Suivant : Ajout d’un nouveau champ](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="008fd-150">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>
