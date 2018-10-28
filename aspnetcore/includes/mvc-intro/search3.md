<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](~/tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="4dd3f-101">Maintenant, quand vous soumettez une recherche, l’URL contient la chaîne de requête de la recherche.</span><span class="sxs-lookup"><span data-stu-id="4dd3f-101">Now when you submit a search, the URL contains the search query string.</span></span> <span data-ttu-id="4dd3f-102">La recherche accède également à la méthode d’action `HttpGet Index`, même si vous avez une méthode `HttpPost Index`.</span><span class="sxs-lookup"><span data-stu-id="4dd3f-102">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![Fenêtre de navigateur montrant la chaîne de recherche « ghost » dans l’URL et les films retournés, Ghostbusters et Ghostbusters 2, qui contiennent le mot « ghost »](~/tutorials/first-mvc-app/search/_static/search_get.png)

<span data-ttu-id="4dd3f-104">La mise en forme suivante montre la modification apportée à la balise `form` :</span><span class="sxs-lookup"><span data-stu-id="4dd3f-104">The following markup shows the change to the `form` tag:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a><span data-ttu-id="4dd3f-105">Ajout de la recherche par genre</span><span class="sxs-lookup"><span data-stu-id="4dd3f-105">Adding Search by genre</span></span>

<span data-ttu-id="4dd3f-106">Ajoutez la classe `MovieGenreViewModel` suivante au dossier *Models* :</span><span class="sxs-lookup"><span data-stu-id="4dd3f-106">Add the following `MovieGenreViewModel` class to the *Models* folder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

<span data-ttu-id="4dd3f-107">Le modèle de vue movie-genre contiendra :</span><span class="sxs-lookup"><span data-stu-id="4dd3f-107">The movie-genre view model will contain:</span></span>

   * <span data-ttu-id="4dd3f-108">Une liste de films.</span><span class="sxs-lookup"><span data-stu-id="4dd3f-108">A list of movies.</span></span>
   * <span data-ttu-id="4dd3f-109">Une `SelectList` contenant la liste des genres.</span><span class="sxs-lookup"><span data-stu-id="4dd3f-109">A `SelectList` containing the list of genres.</span></span> <span data-ttu-id="4dd3f-110">Cela permet à l’utilisateur de sélectionner un genre dans la liste.</span><span class="sxs-lookup"><span data-stu-id="4dd3f-110">This allows the user to select a genre from the list.</span></span>
   * <span data-ttu-id="4dd3f-111">`MovieGenre`, qui contient le genre sélectionné.</span><span class="sxs-lookup"><span data-stu-id="4dd3f-111">`MovieGenre`, which contains the selected genre.</span></span>
   * <span data-ttu-id="4dd3f-112">`SearchString`, qui contient le texte que les utilisateurs entrent dans la zone de texte de recherche.</span><span class="sxs-lookup"><span data-stu-id="4dd3f-112">`SearchString`, which contains the text users enter in the search text box.</span></span>

<span data-ttu-id="4dd3f-113">Remplacez la méthode `Index` dans `MoviesController.cs` par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="4dd3f-113">Replace the `Index` method in `MoviesController.cs` with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

<span data-ttu-id="4dd3f-114">Le code suivant est une requête `LINQ` qui récupère tous les genres de la base de données.</span><span class="sxs-lookup"><span data-stu-id="4dd3f-114">The following code is a `LINQ` query that retrieves all the genres from the database.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

<span data-ttu-id="4dd3f-115">La `SelectList` de genres est créée en projetant les genres distincts (nous ne voulons pas que notre liste de sélection ait des genres en doublon).</span><span class="sxs-lookup"><span data-stu-id="4dd3f-115">The `SelectList` of genres is created by projecting the distinct genres (we don't want our select list to have duplicate genres).</span></span>

<span data-ttu-id="4dd3f-116">Quand l’utilisateur recherche l’élément, la valeur de recherche est conservée dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="4dd3f-116">When the user searches for the item, the search value is retained in the search box.</span></span> <span data-ttu-id="4dd3f-117">Pour conserver la valeur de recherche, remplissez la propriété `SearchString` avec la valeur de recherche.</span><span class="sxs-lookup"><span data-stu-id="4dd3f-117">To retain the search value,  populate the `SearchString` property with the search value.</span></span> <span data-ttu-id="4dd3f-118">La valeur de recherche est le paramètre `searchString` de l’action de contrôleur `Index`.</span><span class="sxs-lookup"><span data-stu-id="4dd3f-118">The search value is the `searchString` parameter for the `Index` controller action.</span></span>

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
```

## <a name="adding-search-by-genre-to-the-index-view"></a><span data-ttu-id="4dd3f-119">Ajout de la recherche par genre à la vue Index</span><span class="sxs-lookup"><span data-stu-id="4dd3f-119">Adding search by genre to the Index view</span></span>

<span data-ttu-id="4dd3f-120">Mettez à jour `Index.cshtml` comme suit :</span><span class="sxs-lookup"><span data-stu-id="4dd3f-120">Update `Index.cshtml` as follows:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

<span data-ttu-id="4dd3f-121">Examinez l’expression lambda utilisée dans le Helper HTML suivant :</span><span class="sxs-lookup"><span data-stu-id="4dd3f-121">Examine the lambda expression used in the following HTML Helper:</span></span>

`@Html.DisplayNameFor(model => model.Movies[0].Title)`
 
<span data-ttu-id="4dd3f-122">Dans le code précédent, le Helper HTML `DisplayNameFor` inspecte la propriété `Title` référencée dans l’expression lambda pour déterminer le nom d’affichage.</span><span class="sxs-lookup"><span data-stu-id="4dd3f-122">In the preceding code, the `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="4dd3f-123">Comme l’expression lambda est inspectée et non pas évaluée, vous ne recevez pas de violation d’accès quand `model`, `model.Movies` ou `model.Movies[0]` sont `null` ou vides.</span><span class="sxs-lookup"><span data-stu-id="4dd3f-123">Since the lambda expression is inspected rather than evaluated, you don't receive an access violation when `model`, `model.Movies`, or `model.Movies[0]` are `null` or empty.</span></span> <span data-ttu-id="4dd3f-124">Quand l’expression lambda est évaluée (par exemple `@Html.DisplayFor(modelItem => item.Title)`), les valeurs des propriétés du modèle sont évaluées.</span><span class="sxs-lookup"><span data-stu-id="4dd3f-124">When the lambda expression is evaluated (for example, `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<span data-ttu-id="4dd3f-125">Testez l’application en effectuant une recherche par genre, par titre de film et selon ces deux critères.</span><span class="sxs-lookup"><span data-stu-id="4dd3f-125">Test the app by searching by genre, by movie title, and by both.</span></span>
