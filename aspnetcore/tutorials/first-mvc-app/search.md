---
title: Ajouter une fonction de recherche à une application ASP.NET Core MVC
author: rick-anderson
description: Montre comment ajouter une fonction de recherche à une application ASP.NET MVC de base
ms.author: riande
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: ca3b0baeddd31e10243689091d435767079bb979
ms.sourcegitcommit: a3926eae3f687013027a2828830c12a89add701f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65450846"
---
# <a name="add-search-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="d3b92-103">Ajouter une fonction de recherche à une application ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="d3b92-103">Add search to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="d3b92-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d3b92-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d3b92-105">Dans cette section, vous ajoutez une fonctionnalité de recherche à la méthode d’action `Index` qui vous permet de rechercher des films par *genre* ou par *nom*.</span><span class="sxs-lookup"><span data-stu-id="d3b92-105">In this section, you add search capability to the `Index` action method that lets you search movies by *genre* or *name*.</span></span>

<span data-ttu-id="d3b92-106">Mettez à jour la méthode `Index` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="d3b92-106">Update the `Index` method with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

<span data-ttu-id="d3b92-107">La première ligne de la méthode d’action `Index` crée une requête [LINQ](/dotnet/standard/using-linq) pour sélectionner les films :</span><span class="sxs-lookup"><span data-stu-id="d3b92-107">The first line of the `Index` action method creates a [LINQ](/dotnet/standard/using-linq) query to select the movies:</span></span>

```csharp
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="d3b92-108">La requête est *seulement* définie à ce stade, elle n’a **pas** été exécutée sur la base de données.</span><span class="sxs-lookup"><span data-stu-id="d3b92-108">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="d3b92-109">Si le paramètre `searchString` contient une chaîne, la requête de films est modifiée de façon à filtrer sur la valeur de la chaîne de recherche :</span><span class="sxs-lookup"><span data-stu-id="d3b92-109">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull2)]

<span data-ttu-id="d3b92-110">Le code `s => s.Title.Contains()` ci-dessus est une [expression lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="d3b92-110">The `s => s.Title.Contains()` code above is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="d3b92-111">Les expressions lambda sont utilisées dans les requêtes [LINQ](/dotnet/standard/using-linq) basées sur une méthode en tant qu’arguments pour les méthodes d’opérateur de requête standard, comme la méthode [Where](/dotnet/api/system.linq.enumerable.where) ou `Contains` (utilisée dans le code ci-dessus).</span><span class="sxs-lookup"><span data-stu-id="d3b92-111">Lambdas are used in method-based [LINQ](/dotnet/standard/using-linq) queries as arguments to standard query operator methods such as the [Where](/dotnet/api/system.linq.enumerable.where) method or `Contains` (used in the code above).</span></span> <span data-ttu-id="d3b92-112">Les requêtes LINQ ne sont pas exécutées quand elles sont définies ou quand elles sont modifiées en appelant une méthode, comme `Where`, `Contains` ou `OrderBy`.</span><span class="sxs-lookup"><span data-stu-id="d3b92-112">LINQ queries are not executed when they're defined or when they're modified by calling a method such as `Where`, `Contains`, or `OrderBy`.</span></span> <span data-ttu-id="d3b92-113">Au lieu de cela, l’exécution de la requête est différée.</span><span class="sxs-lookup"><span data-stu-id="d3b92-113">Rather, query execution is deferred.</span></span>  <span data-ttu-id="d3b92-114">Cela signifie que l’évaluation d’une expression est retardée jusqu’à ce que sa valeur réalisée fasse l’objet d’une itération réelle ou que la méthode `ToListAsync` soit appelée.</span><span class="sxs-lookup"><span data-stu-id="d3b92-114">That means that the evaluation of an expression is delayed until its realized value is actually iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="d3b92-115">Pour plus d’informations sur l’exécution différée des requêtes, consultez [Exécution de requête](/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span><span class="sxs-lookup"><span data-stu-id="d3b92-115">For more information about deferred query execution, see [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span></span>

<span data-ttu-id="d3b92-116">Remarque : La méthode [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) est exécutée sur la base de données, et non pas dans le code C# ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="d3b92-116">Note: The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the c# code shown above.</span></span> <span data-ttu-id="d3b92-117">Le respect de la casse pour la requête dépend de la base de données et du classement.</span><span class="sxs-lookup"><span data-stu-id="d3b92-117">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="d3b92-118">Sur SQL Server, [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) est mappé à [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), qui ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="d3b92-118">On SQL Server, [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="d3b92-119">Dans SQLite, avec le classement par défaut, elle respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="d3b92-119">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="d3b92-120">Accédez à `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="d3b92-120">Navigate to `/Movies/Index`.</span></span> <span data-ttu-id="d3b92-121">Ajoutez une chaîne de requête comme `?searchString=Ghost` à l’URL.</span><span class="sxs-lookup"><span data-stu-id="d3b92-121">Append a query string such as `?searchString=Ghost` to the URL.</span></span> <span data-ttu-id="d3b92-122">Les films filtrés sont affichés.</span><span class="sxs-lookup"><span data-stu-id="d3b92-122">The filtered movies are displayed.</span></span>

![Vue Index](~/tutorials/first-mvc-app/search/_static/ghost.png)

<span data-ttu-id="d3b92-124">Si vous changez la signature de la méthode `Index` pour y inclure un paramètre nommé `id`, le paramètre`id` correspondra à l’espace réservé facultatif `{id}` pour les routes par défaut définies dans *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d3b92-124">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the optional `{id}` placeholder for the default routes set in *Startup.cs*.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

<span data-ttu-id="d3b92-125">Remplacez le paramètre par `id` et toutes les occurrences de `searchString` par `id`.</span><span class="sxs-lookup"><span data-stu-id="d3b92-125">Change the parameter to `id` and all occurrences of `searchString` change to `id`.</span></span>

<span data-ttu-id="d3b92-126">La méthode `Index` précédente :</span><span class="sxs-lookup"><span data-stu-id="d3b92-126">The previous `Index` method:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,6,8&name=snippet_1stSearch)]

<span data-ttu-id="d3b92-127">La méthode `Index` mise à jour avec le paramètre `id` :</span><span class="sxs-lookup"><span data-stu-id="d3b92-127">The updated `Index` method with `id` parameter:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,6,8&name=snippet_SearchID)]

<span data-ttu-id="d3b92-128">Vous pouvez maintenant passer le titre de la recherche en tant que données de routage (un segment de l’URL) et non pas en tant que valeur de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="d3b92-128">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![Vue Index avec le mot « ghost » ajouté à l’URL, et une liste des films retournés avec deux films, Ghostbusters et Ghostbusters 2](~/tutorials/first-mvc-app/search/_static/g2.png)

<span data-ttu-id="d3b92-130">Cependant, vous ne pouvez pas attendre des utilisateurs qu’ils modifient l’URL à chaque fois qu’ils veulent rechercher un film.</span><span class="sxs-lookup"><span data-stu-id="d3b92-130">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="d3b92-131">Vous allez donc maintenant ajouter des éléments d’interface utilisateur pour les aider à filtrer les films.</span><span class="sxs-lookup"><span data-stu-id="d3b92-131">So now you'll add UI elements to help them filter movies.</span></span> <span data-ttu-id="d3b92-132">Si vous avez changé la signature de la méthode `Index` pour tester comment passer le paramètre `ID` lié à une route, rétablissez-la de façon à ce qu’elle prenne un paramètre nommé `searchString` :</span><span class="sxs-lookup"><span data-stu-id="d3b92-132">If you changed the signature of the `Index` method to test how to pass the route-bound `ID` parameter, change it back so that it takes a parameter named `searchString`:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,6,8&name=snippet_1stSearch)]

<span data-ttu-id="d3b92-133">Ouvrez le fichier *Views/Movies/Index.cshtml* et ajoutez le balisage `<form>` mis en surbrillance ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="d3b92-133">Open the *Views/Movies/Index.cshtml* file, and add the `<form>` markup highlighted below:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

<span data-ttu-id="d3b92-134">La balise HTML `<form>` utilise le [Tag Helper de formulaire](xref:mvc/views/working-with-forms), de façon que quand vous envoyez le formulaire, la chaîne de filtrage soit envoyée à l’action `Index` du contrôleur de films.</span><span class="sxs-lookup"><span data-stu-id="d3b92-134">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms), so when you submit the form, the filter string is posted to the `Index` action of the movies controller.</span></span> <span data-ttu-id="d3b92-135">Enregistrez vos modifications puis testez le filtre.</span><span class="sxs-lookup"><span data-stu-id="d3b92-135">Save your changes and then test the filter.</span></span>

![Vue Index avec le mot « ghost » tapé dans la zone de texte du filtre Title](~/tutorials/first-mvc-app/search/_static/filter.png)

<span data-ttu-id="d3b92-137">Contrairement à ce que vous pourriez penser, une surcharge de `[HttpPost]` dans la méthode `Index` n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="d3b92-137">There's no `[HttpPost]` overload of the `Index` method as you might expect.</span></span> <span data-ttu-id="d3b92-138">Vous n’en avez pas besoin, car la méthode ne change pas l’état de l’application, elle filtre seulement les données.</span><span class="sxs-lookup"><span data-stu-id="d3b92-138">You don't need it, because the method isn't changing the state of the app, just filtering data.</span></span>

<span data-ttu-id="d3b92-139">Vous pourriez ajouter la méthode `[HttpPost] Index` suivante.</span><span class="sxs-lookup"><span data-stu-id="d3b92-139">You could add the following `[HttpPost] Index` method.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

<span data-ttu-id="d3b92-140">Le paramètre `notUsed` est utilisé pour créer une surcharge pour la méthode `Index`.</span><span class="sxs-lookup"><span data-stu-id="d3b92-140">The `notUsed` parameter is used to create an overload for the `Index` method.</span></span> <span data-ttu-id="d3b92-141">Nous parlons de ceci plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="d3b92-141">We'll talk about that later in the tutorial.</span></span>

<span data-ttu-id="d3b92-142">Si vous ajoutez cette méthode, le demandeur de l’action correspondrait à la méthode `[HttpPost] Index` et la méthode `[HttpPost] Index` s’exécuterait comme indiqué dans l’image ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="d3b92-142">If you add this method, the action invoker would match the `[HttpPost] Index` method, and the `[HttpPost] Index` method would run as shown in the image below.</span></span>

![Fenêtre de navigateur avec la réponse de l’application de « From HttpPost Index: » avec filtrage sur « ghost »](~/tutorials/first-mvc-app/search/_static/fo.png)

<span data-ttu-id="d3b92-144">Cependant, même si vous ajoutez cette version `[HttpPost]` de la méthode `Index`, il existe une limitation dans la façon dont tout ceci a été implémenté.</span><span class="sxs-lookup"><span data-stu-id="d3b92-144">However, even if you add this `[HttpPost]` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="d3b92-145">Imaginez que vous voulez insérer un signet pour une recherche spécifique, ou que vous voulez envoyer un lien à vos amis sur lequel ils peuvent cliquer pour afficher la même liste filtrée de films.</span><span class="sxs-lookup"><span data-stu-id="d3b92-145">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="d3b92-146">Notez que l’URL de la requête HTTP POST est identique à l’URL de la requête GET (localhost:xxxxx/Movies/Index) : il n’existe aucune information de recherche dans l’URL.</span><span class="sxs-lookup"><span data-stu-id="d3b92-146">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL.</span></span> <span data-ttu-id="d3b92-147">Les informations de la chaîne de recherche sont envoyées au serveur en tant que [valeur d’un champ de formulaire](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data).</span><span class="sxs-lookup"><span data-stu-id="d3b92-147">The search string information is sent to the server as a [form field value](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data).</span></span> <span data-ttu-id="d3b92-148">Vous pouvez vérifier ceci avec les outils de développement du navigateur ou avec l’excellent [outil Fiddler](http://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="d3b92-148">You can verify that with the browser Developer tools or the excellent [Fiddler tool](http://www.telerik.com/fiddler).</span></span> <span data-ttu-id="d3b92-149">L’illustration ci-dessous montre les outils de développement du navigateur Chrome :</span><span class="sxs-lookup"><span data-stu-id="d3b92-149">The image below shows the Chrome browser Developer tools:</span></span>

![Onglet Réseau des outils de développement dans Microsoft Edge montrant un corps de demande avec une valeur « ghost » pour searchString](~/tutorials/first-mvc-app/search/_static/f12_rb.png)

<span data-ttu-id="d3b92-151">Vous pouvez voir le paramètre de recherche et le jeton [XSRF](xref:security/anti-request-forgery) dans le corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="d3b92-151">You can see the search parameter and [XSRF](xref:security/anti-request-forgery) token in the request body.</span></span> <span data-ttu-id="d3b92-152">Notez que, comme indiqué dans le didacticiel précédent, le [Tag Helper de formulaire](xref:mvc/views/working-with-forms) génère un jeton [XSRF](xref:security/anti-request-forgery) anti-contrefaçon.</span><span class="sxs-lookup"><span data-stu-id="d3b92-152">Note, as mentioned in the previous tutorial, the [Form Tag Helper](xref:mvc/views/working-with-forms) generates an [XSRF](xref:security/anti-request-forgery) anti-forgery token.</span></span> <span data-ttu-id="d3b92-153">Nous ne modifions pas les données : nous n’avons donc pas besoin de valider le jeton dans la méthode du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="d3b92-153">We're not modifying data, so we don't need to validate the token in the controller method.</span></span>

<span data-ttu-id="d3b92-154">Comme le paramètre de recherche se trouve dans le corps de la demande et pas dans l’URL, vous ne pouvez pas capturer ces informations de recherche pour les insérer dans un signet ou les partager avec d’autres personnes.</span><span class="sxs-lookup"><span data-stu-id="d3b92-154">Because the search parameter is in the request body and not the URL, you can't capture that search information to bookmark or share with others.</span></span> <span data-ttu-id="d3b92-155">Résolvez ce problème en spécifiant que la requête doit être `HTTP GET` :</span><span class="sxs-lookup"><span data-stu-id="d3b92-155">Fix this by specifying the request should be `HTTP GET`:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexGet.cshtml?highlight=12&range=1-23)]

<span data-ttu-id="d3b92-156">Maintenant, quand vous soumettez une recherche, l’URL contient la chaîne de requête de la recherche.</span><span class="sxs-lookup"><span data-stu-id="d3b92-156">Now when you submit a search, the URL contains the search query string.</span></span> <span data-ttu-id="d3b92-157">La recherche accède également à la méthode d’action `HttpGet Index`, même si vous avez une méthode `HttpPost Index`.</span><span class="sxs-lookup"><span data-stu-id="d3b92-157">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![Fenêtre de navigateur montrant la chaîne de recherche « ghost » dans l’URL et les films retournés, Ghostbusters et Ghostbusters 2, qui contiennent le mot « ghost »](~/tutorials/first-mvc-app/search/_static/search_get.png)

<span data-ttu-id="d3b92-159">La mise en forme suivante montre la modification apportée à la balise `form` :</span><span class="sxs-lookup"><span data-stu-id="d3b92-159">The following markup shows the change to the `form` tag:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="add-search-by-genre"></a><span data-ttu-id="d3b92-160">Ajouter la recherche par genre</span><span class="sxs-lookup"><span data-stu-id="d3b92-160">Add Search by genre</span></span>

<span data-ttu-id="d3b92-161">Ajoutez la classe `MovieGenreViewModel` suivante au dossier *Models* :</span><span class="sxs-lookup"><span data-stu-id="d3b92-161">Add the following `MovieGenreViewModel` class to the *Models* folder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

<span data-ttu-id="d3b92-162">Le modèle de vue movie-genre contiendra :</span><span class="sxs-lookup"><span data-stu-id="d3b92-162">The movie-genre view model will contain:</span></span>

* <span data-ttu-id="d3b92-163">Une liste de films.</span><span class="sxs-lookup"><span data-stu-id="d3b92-163">A list of movies.</span></span>
* <span data-ttu-id="d3b92-164">Une `SelectList` contenant la liste des genres.</span><span class="sxs-lookup"><span data-stu-id="d3b92-164">A `SelectList` containing the list of genres.</span></span> <span data-ttu-id="d3b92-165">Cela permet à l’utilisateur de sélectionner un genre dans la liste.</span><span class="sxs-lookup"><span data-stu-id="d3b92-165">This allows the user to select a genre from the list.</span></span>
* <span data-ttu-id="d3b92-166">`MovieGenre`, qui contient le genre sélectionné.</span><span class="sxs-lookup"><span data-stu-id="d3b92-166">`MovieGenre`, which contains the selected genre.</span></span>
* <span data-ttu-id="d3b92-167">`SearchString`, qui contient le texte que les utilisateurs entrent dans la zone de texte de recherche.</span><span class="sxs-lookup"><span data-stu-id="d3b92-167">`SearchString`, which contains the text users enter in the search text box.</span></span>

<span data-ttu-id="d3b92-168">Remplacez la méthode `Index` dans `MoviesController.cs` par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="d3b92-168">Replace the `Index` method in `MoviesController.cs` with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

<span data-ttu-id="d3b92-169">Le code suivant est une requête `LINQ` qui récupère tous les genres de la base de données.</span><span class="sxs-lookup"><span data-stu-id="d3b92-169">The following code is a `LINQ` query that retrieves all the genres from the database.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_LINQ)]

<span data-ttu-id="d3b92-170">La `SelectList` de genres est créée en projetant les genres distincts (nous ne voulons pas que notre liste de sélection ait des genres en doublon).</span><span class="sxs-lookup"><span data-stu-id="d3b92-170">The `SelectList` of genres is created by projecting the distinct genres (we don't want our select list to have duplicate genres).</span></span>

<span data-ttu-id="d3b92-171">Quand l’utilisateur recherche l’élément, la valeur de recherche est conservée dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="d3b92-171">When the user searches for the item, the search value is retained in the search box.</span></span>

## <a name="add-search-by-genre-to-the-index-view"></a><span data-ttu-id="d3b92-172">Ajouter la recherche par genre à la vue Index</span><span class="sxs-lookup"><span data-stu-id="d3b92-172">Add search by genre to the Index view</span></span>

<span data-ttu-id="d3b92-173">Mettez à jour `Index.cshtml` comme suit :</span><span class="sxs-lookup"><span data-stu-id="d3b92-173">Update `Index.cshtml` as follows:</span></span>

[!code-cshtml[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,19,28,31,34,37,43)]

<span data-ttu-id="d3b92-174">Examinez l’expression lambda utilisée dans le Helper HTML suivant :</span><span class="sxs-lookup"><span data-stu-id="d3b92-174">Examine the lambda expression used in the following HTML Helper:</span></span>

`@Html.DisplayNameFor(model => model.Movies[0].Title)`

<span data-ttu-id="d3b92-175">Dans le code précédent, le Helper HTML `DisplayNameFor` inspecte la propriété `Title` référencée dans l’expression lambda pour déterminer le nom d’affichage.</span><span class="sxs-lookup"><span data-stu-id="d3b92-175">In the preceding code, the `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="d3b92-176">Comme l’expression lambda est inspectée et non pas évaluée, vous ne recevez pas de violation d’accès quand `model`, `model.Movies` ou `model.Movies[0]` sont `null` ou vides.</span><span class="sxs-lookup"><span data-stu-id="d3b92-176">Since the lambda expression is inspected rather than evaluated, you don't receive an access violation when `model`, `model.Movies`, or `model.Movies[0]` are `null` or empty.</span></span> <span data-ttu-id="d3b92-177">Quand l’expression lambda est évaluée (par exemple `@Html.DisplayFor(modelItem => item.Title)`), les valeurs des propriétés du modèle sont évaluées.</span><span class="sxs-lookup"><span data-stu-id="d3b92-177">When the lambda expression is evaluated (for example, `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<span data-ttu-id="d3b92-178">Testez l’application en effectuant une recherche par genre, par titre de film et selon ces deux critères :</span><span class="sxs-lookup"><span data-stu-id="d3b92-178">Test the app by searching by genre, by movie title, and by both:</span></span>

![Fenêtre du navigateur montrant les résultats de https://localhost:5001/Movies?MovieGenre=Comedy&SearchString=2](~/tutorials/first-mvc-app/search/_static/s2.png)

> [!div class="step-by-step"]
> <span data-ttu-id="d3b92-180">[Précédent](controller-methods-views.md)
> [Suivant](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="d3b92-180">[Previous](controller-methods-views.md)
[Next](new-field.md)</span></span>
