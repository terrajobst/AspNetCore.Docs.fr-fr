---
uid: mvc/overview/getting-started/introduction/adding-search
title: Recherche | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/22/2015
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 70a9b08a76a07350bac696f1c74e6397d2aa6ee0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832549"
---
<a name="search"></a><span data-ttu-id="b142a-102">Rechercher</span><span class="sxs-lookup"><span data-stu-id="b142a-102">Search</span></span>
====================
<span data-ttu-id="b142a-103">par [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="b142a-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a><span data-ttu-id="b142a-104">Ajout d’une méthode de recherche et de la vue de la recherche</span><span class="sxs-lookup"><span data-stu-id="b142a-104">Adding a Search Method and Search View</span></span>

<span data-ttu-id="b142a-105">Dans cette section, vous ajouterez des possibilités de recherche le `Index` méthode d’action qui vous permet de rechercher des films par genre ou le nom.</span><span class="sxs-lookup"><span data-stu-id="b142a-105">In this section you'll add search capability to the `Index` action method that lets you search movies by genre or name.</span></span>

## <a name="updating-the-index-form"></a><span data-ttu-id="b142a-106">La mise à jour de la forme d’Index</span><span class="sxs-lookup"><span data-stu-id="b142a-106">Updating the Index Form</span></span>

<span data-ttu-id="b142a-107">Commencez par la mise à jour le `Index` méthode d’action à l’objet existant `MoviesController` classe.</span><span class="sxs-lookup"><span data-stu-id="b142a-107">Start by updating the `Index` action method to the existing `MoviesController` class.</span></span> <span data-ttu-id="b142a-108">Voici le code :</span><span class="sxs-lookup"><span data-stu-id="b142a-108">Here's the code:</span></span>

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

<span data-ttu-id="b142a-109">La première ligne de la `Index` méthode crée les éléments suivants [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) requête pour sélectionner les films :</span><span class="sxs-lookup"><span data-stu-id="b142a-109">The first line of the `Index` method creates the following [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query to select the movies:</span></span>

[!code-csharp[Main](adding-search/samples/sample2.cs)]

<span data-ttu-id="b142a-110">La requête est définie à ce stade, mais n’a pas encore été exécutée par rapport à la base de données.</span><span class="sxs-lookup"><span data-stu-id="b142a-110">The query is defined at this point, but hasn't yet been run against the database.</span></span>

<span data-ttu-id="b142a-111">Si le `searchString` paramètre contient une chaîne, la requête de films est modifiée pour filtrer sur la valeur de la chaîne de recherche, en utilisant le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b142a-111">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string, using the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample3.cs)]

<span data-ttu-id="b142a-112">Le code `s => s.Title` ci-dessus est une [expression lambda](https://msdn.microsoft.com/library/bb397687.aspx).</span><span class="sxs-lookup"><span data-stu-id="b142a-112">The `s => s.Title` code above is a [Lambda Expression](https://msdn.microsoft.com/library/bb397687.aspx).</span></span> <span data-ttu-id="b142a-113">Les expressions lambda sont utilisées dans fondées sur une méthode [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) de requêtes en tant qu’arguments aux méthodes d’opérateur de requête standard telles que la [où](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) méthode utilisée dans le code ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="b142a-113">Lambdas are used in method-based [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) queries as arguments to standard query operator methods such as the [Where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) method used in the above code.</span></span> <span data-ttu-id="b142a-114">Requêtes LINQ ne sont pas exécutées quand elles sont définies ou quand elles sont modifiées en appelant une méthode telle que `Where` ou `OrderBy`.</span><span class="sxs-lookup"><span data-stu-id="b142a-114">LINQ queries are not executed when they are defined or when they are modified by calling a method such as `Where` or `OrderBy`.</span></span> <span data-ttu-id="b142a-115">Au lieu de cela, l’exécution de requête est différée, ce qui signifie que l’évaluation d’une expression est retardée jusqu'à ce que sa valeur réalisée est une itération réelle ou le [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) méthode est appelée.</span><span class="sxs-lookup"><span data-stu-id="b142a-115">Instead, query execution is deferred, which means that the evaluation of an expression is delayed until its realized value is actually iterated over or the [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) method is called.</span></span> <span data-ttu-id="b142a-116">Dans le `Search` exemple, la requête est exécutée dans le *Index.cshtml* vue.</span><span class="sxs-lookup"><span data-stu-id="b142a-116">In the `Search` sample, the query is executed in the *Index.cshtml* view.</span></span> <span data-ttu-id="b142a-117">Pour plus d’informations sur l’exécution différée des requêtes, consultez [Exécution de requête](https://msdn.microsoft.com/library/bb738633.aspx).</span><span class="sxs-lookup"><span data-stu-id="b142a-117">For more information about deferred query execution, see [Query Execution](https://msdn.microsoft.com/library/bb738633.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="b142a-118">Le [Contains](https://msdn.microsoft.com/library/bb155125.aspx) méthode est exécutée sur la base de données, pas le code c# ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="b142a-118">The [Contains](https://msdn.microsoft.com/library/bb155125.aspx) method is run on the database, not the c# code above.</span></span> <span data-ttu-id="b142a-119">Sur la base de données, [Contains](https://msdn.microsoft.com/library/bb155125.aspx) est mappé à [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), qui respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="b142a-119">On the database, [Contains](https://msdn.microsoft.com/library/bb155125.aspx) maps to [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), which is case insensitive.</span></span>

<span data-ttu-id="b142a-120">Maintenant vous pouvez mettre à jour le `Index` vue qui affiche le formulaire à l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b142a-120">Now you can update the `Index` view that will display the form to the user.</span></span>

<span data-ttu-id="b142a-121">Exécutez l’application et accédez à */Movies/Index*.</span><span class="sxs-lookup"><span data-stu-id="b142a-121">Run the application and navigate to */Movies/Index*.</span></span> <span data-ttu-id="b142a-122">Ajoutez une chaîne de requête comme `?searchString=ghost` à l’URL.</span><span class="sxs-lookup"><span data-stu-id="b142a-122">Append a query string such as `?searchString=ghost` to the URL.</span></span> <span data-ttu-id="b142a-123">Les films filtrés sont affichés.</span><span class="sxs-lookup"><span data-stu-id="b142a-123">The filtered movies are displayed.</span></span>

![SearchQryStr](adding-search/_static/image1.png)

<span data-ttu-id="b142a-125">Si vous modifiez la signature de la `Index` méthode d’avoir un paramètre nommé `id`, le `id` paramètre correspond à la `{id}` espace réservé pour la valeur par défaut achemine ensemble dans le *application\_Start RouteConfig.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="b142a-125">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the `{id}` placeholder for the default routes set in the *App\_Start\RouteConfig.cs* file.</span></span>

[!code-json[Main](adding-search/samples/sample4.json)]

<span data-ttu-id="b142a-126">La version d’origine `Index` méthode ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="b142a-126">The original `Index` method looks like this::</span></span>

[!code-csharp[Main](adding-search/samples/sample5.cs)]

<span data-ttu-id="b142a-127">Modifié `Index` méthode se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="b142a-127">The modified `Index` method would look as follows:</span></span>

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

<span data-ttu-id="b142a-128">Vous pouvez maintenant passer le titre de la recherche en tant que données de routage (un segment de l’URL) et non pas en tant que valeur de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="b142a-128">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![](adding-search/_static/image2.png)

<span data-ttu-id="b142a-129">Cependant, vous ne pouvez pas attendre des utilisateurs qu’ils modifient l’URL à chaque fois qu’ils veulent rechercher un film.</span><span class="sxs-lookup"><span data-stu-id="b142a-129">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="b142a-130">Maintenant vous vous allez ajouter une interface utilisateur pour aider à les filtrer les films.</span><span class="sxs-lookup"><span data-stu-id="b142a-130">So now you you'll add UI to help them filter movies.</span></span> <span data-ttu-id="b142a-131">Si vous avez modifié la signature de la `Index` méthode pour tester comment passer le paramètre ID de la limite de l’itinéraire, modifiez-le pour que votre `Index` méthode prend un paramètre de chaîne nommé `searchString`:</span><span class="sxs-lookup"><span data-stu-id="b142a-131">If you changed the signature of the `Index` method to test how to pass the route-bound ID parameter, change it back so that your `Index` method takes a string parameter named `searchString`:</span></span>

[!code-csharp[Main](adding-search/samples/sample7.cs)]

<span data-ttu-id="b142a-132">Ouvrez le *Views\Movies\Index.cshtml* de fichiers et juste après `@Html.ActionLink("Create New", "Create")`, ajoutez le balisage de formulaire mis en surbrillance ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="b142a-132">Open the *Views\Movies\Index.cshtml* file, and just after `@Html.ActionLink("Create New", "Create")`, add the form markup highlighted below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

<span data-ttu-id="b142a-133">Le `Html.BeginForm` helper crée une ouverture `<form>` balise.</span><span class="sxs-lookup"><span data-stu-id="b142a-133">The `Html.BeginForm` helper creates an opening `<form>` tag.</span></span> <span data-ttu-id="b142a-134">Le `Html.BeginForm` helper, le formulaire à publier vers lui-même lorsque l’utilisateur envoie le formulaire en cliquant sur le **filtre** bouton.</span><span class="sxs-lookup"><span data-stu-id="b142a-134">The `Html.BeginForm` helper causes the form to post to itself when the user submits the form by clicking the **Filter** button.</span></span>

<span data-ttu-id="b142a-135">Visual Studio 2013 a une amélioration appréciable lors de l’affichage et modification des fichiers de vue.</span><span class="sxs-lookup"><span data-stu-id="b142a-135">Visual Studio 2013 has a nice improvement when displaying and editing View files.</span></span> <span data-ttu-id="b142a-136">Lorsque vous exécutez l’application avec un fichier d’affichage ouvert, Visual Studio 2013 appelle la méthode d’action de contrôleur correct pour afficher la vue.</span><span class="sxs-lookup"><span data-stu-id="b142a-136">When you run the application with a view file open, Visual Studio 2013 invokes the correct controller action method to display the view.</span></span>

![](adding-search/_static/image3.png)

<span data-ttu-id="b142a-137">Avec la vue Index ouvert dans Visual Studio (comme indiqué dans l’image ci-dessus), appuyez sur CTRL F5 ou F5 pour exécuter l’application et essayez de rechercher un film.</span><span class="sxs-lookup"><span data-stu-id="b142a-137">With the Index view open in Visual Studio (as shown in the image above), tap Ctr F5 or F5 to run the application and then try searching for a movie.</span></span>

![](adding-search/_static/image4.png)

<span data-ttu-id="b142a-138">Il existe aucune `HttpPost` surcharge de la `Index` (méthode).</span><span class="sxs-lookup"><span data-stu-id="b142a-138">There's no `HttpPost` overload of the `Index` method.</span></span> <span data-ttu-id="b142a-139">Vous n’en avez besoin, car la méthode ne change pas l’état de l’application, filtrage des données.</span><span class="sxs-lookup"><span data-stu-id="b142a-139">You don't need it, because the method isn't changing the state of the application, just filtering data.</span></span>

<span data-ttu-id="b142a-140">Vous pourriez ajouter la méthode `HttpPost Index` suivante.</span><span class="sxs-lookup"><span data-stu-id="b142a-140">You could add the following `HttpPost Index` method.</span></span> <span data-ttu-id="b142a-141">Dans ce cas, le demandeur d’action correspondrait à la `HttpPost Index` (méthode) et le `HttpPost Index` méthode s’exécuterait comme indiqué dans l’image ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="b142a-141">In that case, the action invoker would match the `HttpPost Index` method, and the `HttpPost Index` method would run as shown in the image below.</span></span>

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

<span data-ttu-id="b142a-143">Cependant, même si vous ajoutez cette version `HttpPost` de la méthode `Index`, il existe une limitation dans la façon dont tout ceci a été implémenté.</span><span class="sxs-lookup"><span data-stu-id="b142a-143">However, even if you add this `HttpPost` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="b142a-144">Imaginez que vous voulez insérer un signet pour une recherche spécifique, ou que vous voulez envoyer un lien à vos amis sur lequel ils peuvent cliquer pour afficher la même liste filtrée de films.</span><span class="sxs-lookup"><span data-stu-id="b142a-144">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="b142a-145">Notez que l’URL de la requête HTTP POST est identique à l’URL de la requête GET (localhost : XXXXX/Movies/Index) : il n’existe aucune information de recherche dans l’URL proprement dite.</span><span class="sxs-lookup"><span data-stu-id="b142a-145">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL itself.</span></span> <span data-ttu-id="b142a-146">Droite désormais, les informations de chaîne de recherche sont envoyées au serveur en tant que valeur d’un champ de formulaire.</span><span class="sxs-lookup"><span data-stu-id="b142a-146">Right now, the search string information is sent to the server as a form field value.</span></span> <span data-ttu-id="b142a-147">Cela signifie que vous ne pouvez pas capturer ces informations de recherche pour créer un signet ou envoyer à vos amis dans une URL.</span><span class="sxs-lookup"><span data-stu-id="b142a-147">This means you can't capture that search information to bookmark or send to friends in a URL.</span></span>

<span data-ttu-id="b142a-148">La solution consiste à utiliser une surcharge de `BeginForm` qui spécifie que la requête POST doit ajouter les informations de recherche à l’URL et qu’il doit être acheminé vers le `HttpGet` version de la `Index` (méthode).</span><span class="sxs-lookup"><span data-stu-id="b142a-148">The solution is to use an overload of `BeginForm` that specifies that the POST request should add the search information to the URL and that it should be routed to the `HttpGet` version of the `Index` method.</span></span> <span data-ttu-id="b142a-149">Remplacer l’existant sans paramètre `BeginForm` méthode avec le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="b142a-149">Replace the existing parameterless `BeginForm` method with the following markup:</span></span>

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

<span data-ttu-id="b142a-151">Maintenant lorsque vous soumettez une recherche, l’URL contient une chaîne de requête de recherche.</span><span class="sxs-lookup"><span data-stu-id="b142a-151">Now when you submit a search, the URL contains a search query string.</span></span> <span data-ttu-id="b142a-152">La recherche accède également à la méthode d’action `HttpGet Index`, même si vous avez une méthode `HttpPost Index`.</span><span class="sxs-lookup"><span data-stu-id="b142a-152">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a><span data-ttu-id="b142a-154">Ajout de la recherche par Genre</span><span class="sxs-lookup"><span data-stu-id="b142a-154">Adding Search by Genre</span></span>

<span data-ttu-id="b142a-155">Si vous avez ajouté le `HttpPost` version de la `Index` (méthode), supprimez maintenant.</span><span class="sxs-lookup"><span data-stu-id="b142a-155">If you added the `HttpPost` version of the `Index` method, delete it now.</span></span>

<span data-ttu-id="b142a-156">Ensuite, vous allez ajouter une fonctionnalité pour permettre aux utilisateurs de rechercher des films par genre.</span><span class="sxs-lookup"><span data-stu-id="b142a-156">Next, you'll add a feature to let users search for movies by genre.</span></span> <span data-ttu-id="b142a-157">Remplacez la méthode `Index` par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b142a-157">Replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample11.cs)]

<span data-ttu-id="b142a-158">Cette version de la `Index` méthode prend un paramètre supplémentaire, à savoir `movieGenre`.</span><span class="sxs-lookup"><span data-stu-id="b142a-158">This version of the `Index` method takes an additional parameter, namely `movieGenre`.</span></span> <span data-ttu-id="b142a-159">Les premières lignes de code créent un `List` objet pour contenir les genres de film à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="b142a-159">The first few lines of code create a `List` object to hold movie genres from the database.</span></span>

<span data-ttu-id="b142a-160">Le code suivant est une requête LINQ qui récupère tous les genres dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="b142a-160">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](adding-search/samples/sample12.cs)]

<span data-ttu-id="b142a-161">Le code utilise le `AddRange` méthode du générique `List` collection pour ajouter tous les genres distincts à la liste.</span><span class="sxs-lookup"><span data-stu-id="b142a-161">The code uses the `AddRange` method of the generic `List` collection to add all the distinct genres to the list.</span></span> <span data-ttu-id="b142a-162">(Sans le `Distinct` modificateur, genres en doublon seraient ajoutés, par exemple, comédie est ajoutée à deux reprises dans notre exemple).</span><span class="sxs-lookup"><span data-stu-id="b142a-162">(Without the `Distinct` modifier, duplicate genres would be added — for example, comedy would be added twice in our sample).</span></span> <span data-ttu-id="b142a-163">Le code stocke ensuite la liste des genres dans le `ViewBag.MovieGenre` objet.</span><span class="sxs-lookup"><span data-stu-id="b142a-163">The code then stores the list of genres in the `ViewBag.MovieGenre` object.</span></span> <span data-ttu-id="b142a-164">Stockage des données de catégorie (de tel un genre de film) en tant qu’un [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) de l’objet dans un `ViewBag`, puis accéder aux données de catégorie dans une zone de liste déroulante est une approche courante pour les applications MVC.</span><span class="sxs-lookup"><span data-stu-id="b142a-164">Storing category data (such a movie genre's) as a [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) object in a `ViewBag`, then accessing the category data in a dropdown list box is a typical approach for MVC applications.</span></span>

<span data-ttu-id="b142a-165">Le code suivant montre comment vérifier la `movieGenre` paramètre.</span><span class="sxs-lookup"><span data-stu-id="b142a-165">The following code shows how to check the `movieGenre` parameter.</span></span> <span data-ttu-id="b142a-166">S’il n’est pas vide, le code plus contraint la requête de films pour limiter les films sélectionnés pour le genre spécifié.</span><span class="sxs-lookup"><span data-stu-id="b142a-166">If it's not empty, the code further constrains the movies query to limit the selected movies to the specified genre.</span></span>

[!code-csharp[Main](adding-search/samples/sample13.cs)]

<span data-ttu-id="b142a-167">Comme indiqué précédemment, la requête n’est pas exécutée sur la base de données jusqu'à ce que la liste de films est itérée (ce qui se passe dans la vue, après le `Index` retourne de la méthode d’action).</span><span class="sxs-lookup"><span data-stu-id="b142a-167">As stated previously, the query is not run on the data base until the movie list is iterated over (which happens in the View, after the `Index` action method returns).</span></span>

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a><span data-ttu-id="b142a-168">Ajout de balisage à la vue Index pour prendre en charge la recherche par Genre</span><span class="sxs-lookup"><span data-stu-id="b142a-168">Adding Markup to the Index View to Support Search by Genre</span></span>

<span data-ttu-id="b142a-169">Ajouter un `Html.DropDownList` helper pour le *Views\Movies\Index.cshtml* de fichiers, juste avant la `TextBox` helper.</span><span class="sxs-lookup"><span data-stu-id="b142a-169">Add an `Html.DropDownList` helper to the *Views\Movies\Index.cshtml* file, just before the `TextBox` helper.</span></span> <span data-ttu-id="b142a-170">Le balisage complet est indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="b142a-170">The completed markup is shown below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

<span data-ttu-id="b142a-171">Dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b142a-171">In the following code:</span></span>

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

<span data-ttu-id="b142a-172">Le paramètre « MovieGenre » fournit la clé pour le `DropDownList` helper pour rechercher un `IEnumerable<SelectListItem>` dans le `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="b142a-172">The parameter "MovieGenre" provides the key for the `DropDownList` helper to find a `IEnumerable<SelectListItem>` in the `ViewBag`.</span></span> <span data-ttu-id="b142a-173">Le `ViewBag` a été renseigné dans la méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="b142a-173">The `ViewBag` was populated in the action method:</span></span>

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

<span data-ttu-id="b142a-174">Le paramètre « All » fournit une étiquette d’option.</span><span class="sxs-lookup"><span data-stu-id="b142a-174">The parameter "All" provides an option label.</span></span> <span data-ttu-id="b142a-175">Si vous examinez ce choix dans votre navigateur, vous verrez que son attribut de « valeur » est vide.</span><span class="sxs-lookup"><span data-stu-id="b142a-175">If you inspect that choice in your browser, you'll see that its "value" attribute is empty.</span></span> <span data-ttu-id="b142a-176">Étant donné que notre contrôleur filtre uniquement `if` la chaîne n’est pas `null` ou est vide, l’envoi d’une valeur vide pour `movieGenre` montre tous les genres.</span><span class="sxs-lookup"><span data-stu-id="b142a-176">Since our controller only filters `if` the string is not `null` or empty, submitting an empty value for `movieGenre` shows all genres.</span></span>

<span data-ttu-id="b142a-177">Vous pouvez également définir une option pour être sélectionnée par défaut.</span><span class="sxs-lookup"><span data-stu-id="b142a-177">You can also set an option to be selected by default.</span></span> <span data-ttu-id="b142a-178">Si vous souhaitiez « Comédie » comme option de valeur par défaut, vous modifieriez le code dans le contrôleur comme suit :</span><span class="sxs-lookup"><span data-stu-id="b142a-178">If you wanted "Comedy" as your default option, you would change the code in the Controller like so:</span></span>

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

<span data-ttu-id="b142a-179">Exécutez l’application et accédez à */Movies/Index*.</span><span class="sxs-lookup"><span data-stu-id="b142a-179">Run the application and browse to */Movies/Index*.</span></span> <span data-ttu-id="b142a-180">Essayez une recherche par genre, par le nom du film et par les deux critères.</span><span class="sxs-lookup"><span data-stu-id="b142a-180">Try a search by genre, by movie name, and by both criteria.</span></span>

![](adding-search/_static/image8.png)

<span data-ttu-id="b142a-181">Dans cette section, vous avez créé une méthode d’action de recherche et de la vue qui permettre aux utilisateurs de rechercher par titre de film et genre.</span><span class="sxs-lookup"><span data-stu-id="b142a-181">In this section you created a search action method and view that let users search by movie title and genre.</span></span> <span data-ttu-id="b142a-182">Dans la section suivante, vous explorerez comment ajouter une propriété à la `Movie` modèle et comment ajouter un initialiseur qui crée automatiquement une base de données de test.</span><span class="sxs-lookup"><span data-stu-id="b142a-182">In the next section, you'll look at how to add a property to the `Movie` model and how to add an initializer that will automatically create a test database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b142a-183">[Précédent](examining-the-edit-methods-and-edit-view.md)
> [Suivant](adding-a-new-field.md)</span><span class="sxs-lookup"><span data-stu-id="b142a-183">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-a-new-field.md)</span></span>
