---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: L’accès aux données de votre modèle à partir d’un contrôleur | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: a3f3f4a030650ff65b070528c5efa1605be764a0
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38120151"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="7d538-102">L’accès aux données de votre modèle à partir d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="7d538-102">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="7d538-103">par [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="7d538-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="7d538-104">Dans cette section, vous allez créer un nouveau `MoviesController` classe et d’écrire du code qui Récupère les données de film et l’affiche dans le navigateur à l’aide d’un modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="7d538-104">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="7d538-105">**Générez l’application** avant de passer à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="7d538-105">**Build the application** before going on to the next step.</span></span> <span data-ttu-id="7d538-106">Si vous ne générez l’application, vous obtiendrez une erreur d’ajout d’un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="7d538-106">If you don't build the application, you'll get an error adding a controller.</span></span>

<span data-ttu-id="7d538-107">Dans l’Explorateur de solutions, cliquez sur le *contrôleurs* dossier, puis cliquez sur **ajouter**, puis **contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="7d538-107">In Solution Explorer, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="7d538-108">Dans le **ajouter une structure** boîte de dialogue, cliquez sur **contrôleur MVC 5 avec vues, utilisant Entity Framework**, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="7d538-108">In the **Add Scaffold** dialog box, click **MVC 5 Controller with views, using Entity Framework**, and then click **Add**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- <span data-ttu-id="7d538-109">Sélectionnez **Movie (MvcMovie.Models)** pour la classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="7d538-109">Select **Movie (MvcMovie.Models)** for the Model class.</span></span>
- <span data-ttu-id="7d538-110">Sélectionnez **MovieDBContext (MvcMovie.Models)** pour la classe de contexte de données.</span><span class="sxs-lookup"><span data-stu-id="7d538-110">Select **MovieDBContext (MvcMovie.Models)** for the Data context class.</span></span>
- <span data-ttu-id="7d538-111">Entrez le nom du contrôleur **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="7d538-111">For the Controller name enter **MoviesController**.</span></span>

  <span data-ttu-id="7d538-112">L’image ci-dessous montre la boîte de dialogue terminé.</span><span class="sxs-lookup"><span data-stu-id="7d538-112">The image below shows the completed dialog.</span></span>  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

<span data-ttu-id="7d538-113">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="7d538-113">Click **Add**.</span></span> <span data-ttu-id="7d538-114">(Si vous obtenez une erreur, vous probablement n’a pas généré l’application avant de commencer l’ajout du contrôleur.) Visual Studio crée les fichiers et dossiers suivants :</span><span class="sxs-lookup"><span data-stu-id="7d538-114">(If you get an error, you probably didn't build the application before starting adding the controller.) Visual Studio creates the following files and folders:</span></span>

- <span data-ttu-id="7d538-115">*Un MoviesController.cs* de fichiers dans le *contrôleurs* dossier.</span><span class="sxs-lookup"><span data-stu-id="7d538-115">*A MoviesController.cs* file in the *Controllers* folder.</span></span>
- <span data-ttu-id="7d538-116">Un *Views\Movies* dossier.</span><span class="sxs-lookup"><span data-stu-id="7d538-116">A *Views\Movies* folder.</span></span>
- <span data-ttu-id="7d538-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, et *Index.cshtml* dans le nouveau *Views\Movies* dossier.</span><span class="sxs-lookup"><span data-stu-id="7d538-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="7d538-118">Visual Studio créé automatiquement la [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (créer, lire, mettre à jour et supprimer) des méthodes d’action et des vues pour vous (la création automatique de méthodes d’action CRUD et de vues est appelée génération de modèles automatique).</span><span class="sxs-lookup"><span data-stu-id="7d538-118">Visual Studio automatically created the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="7d538-119">Vous disposez maintenant d’une application web entièrement fonctionnelles qui vous permet de créer, répertorier, modifier et supprimer des entrées de film.</span><span class="sxs-lookup"><span data-stu-id="7d538-119">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="7d538-120">Exécutez l’application et cliquez sur le **MVC Movie** lien (ou accédez à la `Movies` contrôleur en ajoutant */Movies* à l’URL dans la barre d’adresses de votre navigateur).</span><span class="sxs-lookup"><span data-stu-id="7d538-120">Run the application and click on the **MVC Movie** link (or browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser).</span></span> <span data-ttu-id="7d538-121">Étant donné que l’application repose sur le routage par défaut (définie dans le *application\_start\routeconfig* fichier), la demande de navigateur `http://localhost:xxxxx/Movies` est acheminé vers la valeur par défaut `Index` méthode d’action de la `Movies` contrôleur.</span><span class="sxs-lookup"><span data-stu-id="7d538-121">Because the application is relying on the default routing (defined in the *App\_Start\RouteConfig.cs* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="7d538-122">En d’autres termes, la demande de navigateur `http://localhost:xxxxx/Movies` est en fait identique à la demande de navigateur `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="7d538-122">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="7d538-123">Le résultat est une liste vide de films, car vous n’avez pas ajouté.</span><span class="sxs-lookup"><span data-stu-id="7d538-123">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a><span data-ttu-id="7d538-124">Création d’un film</span><span class="sxs-lookup"><span data-stu-id="7d538-124">Creating a Movie</span></span>

<span data-ttu-id="7d538-125">Sélectionnez le lien **Créer nouveau**.</span><span class="sxs-lookup"><span data-stu-id="7d538-125">Select the **Create New** link.</span></span> <span data-ttu-id="7d538-126">Entrez des détails sur un film, puis activez la **créer** bouton.</span><span class="sxs-lookup"><span data-stu-id="7d538-126">Enter some details about a movie and then click the **Create** button.</span></span>


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="7d538-127">Vous n’êtes peut-être pas en mesure d’entrer des séparateurs décimaux ou des virgules dans le champ prix.</span><span class="sxs-lookup"><span data-stu-id="7d538-127">You may not be able to enter decimal points or commas in the Price field.</span></span> <span data-ttu-id="7d538-128">Pour prendre en charge la validation jQuery pour les paramètres régionaux non anglais qui utilisent une virgule (&quot;,&quot;) pour une virgule décimale et les formats de date non anglais des États-Unis, vous devez inclure *globalize.js* et votre propre  *cultures/globalize.cultures.js* fichier (à partir de [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) et JavaScript pour utiliser `Globalize.parseFloat`.</span><span class="sxs-lookup"><span data-stu-id="7d538-128">To support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, and non US-English date formats, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="7d538-129">Je montrerai comment effectuer cette opération dans le didacticiel suivant.</span><span class="sxs-lookup"><span data-stu-id="7d538-129">I'll show how to do this in the next tutorial.</span></span> <span data-ttu-id="7d538-130">Pour le moment, entrez simplement des nombres entiers tels que 10.</span><span class="sxs-lookup"><span data-stu-id="7d538-130">For now, just enter whole numbers like 10.</span></span>


<span data-ttu-id="7d538-131">En cliquant sur le **créer** bouton provoque le formulaire à être publié sur le serveur, où les informations de film sont enregistrées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="7d538-131">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="7d538-132">Vous êtes ensuite redirigé vers le */Movies* URL, où vous pouvez voir le film nouvellement créé dans la liste.</span><span class="sxs-lookup"><span data-stu-id="7d538-132">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="7d538-133">Créez deux ou trois autres entrées.</span><span class="sxs-lookup"><span data-stu-id="7d538-133">Create a couple more movie entries.</span></span> <span data-ttu-id="7d538-134">Essayez les liens **Edit**, **Details** et **Delete**, qui sont tous opérationnels.</span><span class="sxs-lookup"><span data-stu-id="7d538-134">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="7d538-135">Examen du Code généré</span><span class="sxs-lookup"><span data-stu-id="7d538-135">Examining the Generated Code</span></span>

<span data-ttu-id="7d538-136">Ouvrez le *Controllers\MoviesController.cs* de fichiers et d’examiner le généré `Index` (méthode).</span><span class="sxs-lookup"><span data-stu-id="7d538-136">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="7d538-137">Une partie du contrôleur de film avec la `Index` méthode est indiquée ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="7d538-137">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="7d538-138">Une demande à la `Movies` contrôleur retourne toutes les entrées dans le `Movies` table, puis transmet les résultats à le `Index` vue.</span><span class="sxs-lookup"><span data-stu-id="7d538-138">A request to the `Movies` controller returns all the entries in the `Movies` table and then passes the results to the `Index` view.</span></span> <span data-ttu-id="7d538-139">La ligne suivante à partir de la `MoviesController` classe instancie un contexte de base de données de film, comme décrit précédemment.</span><span class="sxs-lookup"><span data-stu-id="7d538-139">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="7d538-140">Vous pouvez utiliser le contexte de base de données de film pour interroger, modifier et supprimer des films.</span><span class="sxs-lookup"><span data-stu-id="7d538-140">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="7d538-141">Modèles fortement typés et @model mot clé</span><span class="sxs-lookup"><span data-stu-id="7d538-141">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="7d538-142">Précédemment dans ce didacticiel, vous l’avez vu comment un contrôleur peut passer des données ou des objets à un modèle de vue en utilisant le `ViewBag` objet.</span><span class="sxs-lookup"><span data-stu-id="7d538-142">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="7d538-143">Le `ViewBag` est un objet dynamique qui offre un moyen pratique de la liaison tardive pour passer des informations à une vue.</span><span class="sxs-lookup"><span data-stu-id="7d538-143">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="7d538-144">MVC fournit également la possibilité de passer *fortement* des objets typés à un modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="7d538-144">MVC also provides the ability to pass *strongly* typed objects to a view template.</span></span> <span data-ttu-id="7d538-145">Cette approche fortement typée permet une meilleure compilation au moment de la vérification de votre code et plus riche [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) dans l’éditeur Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7d538-145">This strongly typed approach enables better compile-time checking of your code and richer [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) in the Visual Studio editor.</span></span> <span data-ttu-id="7d538-146">Le mécanisme de génération de modèles automatique dans Visual Studio utilisé cette approche (autrement dit, en passant un *fortement* modèle typé) avec le `MoviesController` modèles de classe et de la vue lorsqu’il créé les méthodes et les vues.</span><span class="sxs-lookup"><span data-stu-id="7d538-146">The scaffolding mechanism in Visual Studio used this approach (that is, passing a *strongly* typed model) with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="7d538-147">Dans le *Controllers\MoviesController.cs* fichier examiner généré `Details` (méthode).</span><span class="sxs-lookup"><span data-stu-id="7d538-147">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="7d538-148">Le `Details` méthode est indiquée ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="7d538-148">The `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

<span data-ttu-id="7d538-149">Le `id` paramètre est généralement passé en tant que données d’itinéraire, par exemple `http://localhost:1234/movies/details/1` définira le contrôleur au contrôleur de films, l’action à `details` et `id` à 1.</span><span class="sxs-lookup"><span data-stu-id="7d538-149">The `id` parameter is generally passed as route data, for example `http://localhost:1234/movies/details/1` will set the controller to the movie controller, the action to `details` and the `id` to 1.</span></span> <span data-ttu-id="7d538-150">Vous pouvez également transmettre l’id avec une chaîne de requête comme suit :</span><span class="sxs-lookup"><span data-stu-id="7d538-150">You could also pass in the id with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="7d538-151">Si un `Movie` est trouvé, une instance de la `Movie` modèle est passé à la `Details` vue :</span><span class="sxs-lookup"><span data-stu-id="7d538-151">If a `Movie` is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

<span data-ttu-id="7d538-152">Examinez le contenu de la *Views\Movies\Details.cshtml* fichier :</span><span class="sxs-lookup"><span data-stu-id="7d538-152">Examine the contents of the *Views\Movies\Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

<span data-ttu-id="7d538-153">En incluant un `@model` instruction en haut du fichier de modèle de vue, vous pouvez spécifier le type d’objet attendu par la vue.</span><span class="sxs-lookup"><span data-stu-id="7d538-153">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="7d538-154">Quand vous avez créé le contrôleur pour les films, Visual Studio a inclus automatiquement l’instruction `@model` suivante en haut du fichier *Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="7d538-154">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="7d538-155">Cette directive `@model` vous permet d’accéder au film que le contrôleur a passé à la vue en utilisant un objet `Model` qui est fortement typé.</span><span class="sxs-lookup"><span data-stu-id="7d538-155">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="7d538-156">Par exemple, dans le *Details.cshtml* modèle, le code passe chaque champ du film à la `DisplayNameFor` et [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers avec fortement typée `Model` objet.</span><span class="sxs-lookup"><span data-stu-id="7d538-156">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="7d538-157">Le `Create` et `Edit` méthodes et modèles de vue également passer un objet de modèle de film.</span><span class="sxs-lookup"><span data-stu-id="7d538-157">The `Create` and `Edit` methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="7d538-158">Examiner le *Index.cshtml* afficher le modèle et le `Index` méthode dans le *MoviesController.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="7d538-158">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="7d538-159">Notez comment le code crée un [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) de l’objet lorsqu’il appelle le `View` méthode d’assistance dans le `Index` méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="7d538-159">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="7d538-160">Le code transmet ensuite `Movies` liste à partir de la `Index` méthode d’action à la vue :</span><span class="sxs-lookup"><span data-stu-id="7d538-160">The code then passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

<span data-ttu-id="7d538-161">Lorsque vous avez créé le contrôleur de films, Visual Studio inclus automatiquement ce qui suit `@model` instruction en haut de la *Index.cshtml* fichier :</span><span class="sxs-lookup"><span data-stu-id="7d538-161">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

<span data-ttu-id="7d538-162">Cela `@model` directive vous permet d’accéder à la liste de films que le contrôleur a passé à la vue en utilisant un `Model` objet fortement typé.</span><span class="sxs-lookup"><span data-stu-id="7d538-162">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="7d538-163">Par exemple, dans le *Index.cshtml* modèle, le code effectue une itération sur les films en effectuant un `foreach` instruction sur fortement typée `Model` objet :</span><span class="sxs-lookup"><span data-stu-id="7d538-163">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="7d538-164">Étant donné que le `Model` objet est fortement typé (comme un `IEnumerable<Movie>` objet), chaque `item` objet dans la boucle est typé en tant que `Movie`.</span><span class="sxs-lookup"><span data-stu-id="7d538-164">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="7d538-165">Entre autres avantages, cela signifie que vous obtenez la vérification au moment de la compilation du code et prise en charge d’IntelliSense dans l’éditeur de code complète :</span><span class="sxs-lookup"><span data-stu-id="7d538-165">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="7d538-167">Utilisation de SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="7d538-167">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="7d538-168">Entity Framework Code First a détecté que la chaîne de connexion de base de données qui a été fournie vers lequel pointe un `Movies` base de données qui n’existe pas encore, c’est pourquoi Code First créé la base de données automatiquement.</span><span class="sxs-lookup"><span data-stu-id="7d538-168">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="7d538-169">Vous pouvez vérifier qu’il a été créé en consultant le *application\_données* dossier.</span><span class="sxs-lookup"><span data-stu-id="7d538-169">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="7d538-170">Si vous ne voyez pas le *Movies.mdf* de fichiers, cliquez sur le **afficher tous les fichiers** situé dans le **l’Explorateur de solutions** barre d’outils, cliquez sur le **Actualiser** bouton, puis développez le *application\_données* dossier.</span><span class="sxs-lookup"><span data-stu-id="7d538-170">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image9.png)

<span data-ttu-id="7d538-171">Double-cliquez sur *Movies.mdf* pour ouvrir **Explorateur de serveurs**, puis développez le **Tables** dossier pour afficher le tableau de films.</span><span class="sxs-lookup"><span data-stu-id="7d538-171">Double-click *Movies.mdf* to open **SERVER EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span> <span data-ttu-id="7d538-172">Notez l’icône de clé en regard de code.</span><span class="sxs-lookup"><span data-stu-id="7d538-172">Note the key icon next to ID.</span></span> <span data-ttu-id="7d538-173">Par défaut, EF fait d’une propriété nommée ID de la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="7d538-173">By default, EF will make a property named ID the primary key.</span></span> <span data-ttu-id="7d538-174">Pour plus d’informations sur EF et MVC, consultez Didacticiel excellente de Tom Dykstra sur [MVC et Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="7d538-174">For more information on EF and MVC, see Tom Dykstra's excellent tutorial on [MVC and EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="7d538-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="7d538-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span></span>

<span data-ttu-id="7d538-176">Avec le bouton droit le `Movies` de table et sélectionnez **afficher les données de Table** pour afficher les données que vous avez créé.</span><span class="sxs-lookup"><span data-stu-id="7d538-176">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

<span data-ttu-id="7d538-177">Avec le bouton droit le `Movies` de table et sélectionnez **ouvrir la définition de Table** pour afficher la table de structure que Entity Framework Code First créé pour vous.</span><span class="sxs-lookup"><span data-stu-id="7d538-177">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

<span data-ttu-id="7d538-178">Notez comment le schéma de la `Movies` table correspond à la `Movie` classe que vous avez créé précédemment.</span><span class="sxs-lookup"><span data-stu-id="7d538-178">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="7d538-179">Entity Framework Code First automatiquement créé ce schéma pour vous en fonction de votre `Movie` classe.</span><span class="sxs-lookup"><span data-stu-id="7d538-179">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="7d538-180">Lorsque vous avez terminé, fermez la connexion en double-cliquant sur *MovieDBContext* et en sélectionnant **fermer la connexion**.</span><span class="sxs-lookup"><span data-stu-id="7d538-180">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="7d538-181">(Si vous ne fermez pas la connexion, vous pouvez obtenir une erreur la prochaine fois que vous exécutez le projet).</span><span class="sxs-lookup"><span data-stu-id="7d538-181">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="7d538-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="7d538-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span></span>

<span data-ttu-id="7d538-183">Vous disposez maintenant d’une base de données et de pages pour afficher, modifier, mettre à jour et supprimer les données.</span><span class="sxs-lookup"><span data-stu-id="7d538-183">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="7d538-184">Dans le didacticiel suivant, nous allons examiner le reste du code généré automatiquement et ajoutez un `SearchIndex` (méthode) et un `SearchIndex` vue qui vous permet de rechercher des films dans cette base de données.</span><span class="sxs-lookup"><span data-stu-id="7d538-184">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span> <span data-ttu-id="7d538-185">Pour plus d’informations sur l’utilisation d’Entity Framework avec MVC, consultez [création d’un modèle de données Entity Framework pour une Application ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="7d538-185">For more information on using Entity Framework with MVC, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7d538-186">[Précédent](creating-a-connection-string.md)
> [Suivant](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="7d538-186">[Previous](creating-a-connection-string.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
