---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: "L’accès aux données de votre modèle à partir d’un contrôleur | Documents Microsoft"
author: Rick-Anderson
description: "Remarque : Une version mise à jour de ce didacticiel est disponible ici qui utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sécurisé, beaucoup plus simple à suivre et de démonstration..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: f323fe37da739d957a609dc7ca4e71a3c3ab475e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="ec2f3-104">L’accès aux données de votre modèle à partir d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="ec2f3-104">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="ec2f3-105">Par [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="ec2f3-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="ec2f3-106">Une version mise à jour de ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) qui utilise ASP.NET MVC 5 et Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="ec2f3-107">Il est plus sécurisé, beaucoup plus simple à suivre et illustre plusieurs fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="ec2f3-108">Dans cette section, vous allez créer un nouveau `MoviesController` classe et d’écrire du code qui extrait les données de film et l’affiche dans le navigateur à l’aide d’un modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-108">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="ec2f3-109">**Générez l’application** avant de passer à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-109">**Build the application** before going on to the next step.</span></span>

<span data-ttu-id="ec2f3-110">Avec le bouton droit le *contrôleurs* dossier et créer un nouveau `MoviesController` contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-110">Right-click the *Controllers* folder and create a new `MoviesController` controller.</span></span> <span data-ttu-id="ec2f3-111">Les options ci-dessous n’apparaîtront pas tant que vous générez votre application.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-111">The options below will not appear until you build your application.</span></span> <span data-ttu-id="ec2f3-112">Sélectionnez les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="ec2f3-112">Select the following options:</span></span>

- <span data-ttu-id="ec2f3-113">Nom du contrôleur : **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-113">Controller name: **MoviesController**.</span></span> <span data-ttu-id="ec2f3-114">(Il s’agit de la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-114">(This is the default.</span></span> <span data-ttu-id="ec2f3-115">)</span><span class="sxs-lookup"><span data-stu-id="ec2f3-115">)</span></span>
- <span data-ttu-id="ec2f3-116">Modèle : **contrôleur MVC avec des actions de lecture/écriture et de vues, utilisant Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-116">Template: **MVC Controller with read/write actions and views, using Entity Framework**.</span></span>
- <span data-ttu-id="ec2f3-117">Classe de modèle : **film (MvcMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-117">Model class: **Movie (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="ec2f3-118">Classe de contexte de données : **MovieDBContext (MvcMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-118">Data context class: **MovieDBContext (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="ec2f3-119">Affichages : **Razor (CSHTML)**.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-119">Views: **Razor (CSHTML)**.</span></span> <span data-ttu-id="ec2f3-120">(La valeur par défaut.)</span><span class="sxs-lookup"><span data-stu-id="ec2f3-120">(The default.)</span></span>

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="ec2f3-122">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-122">Click **Add**.</span></span> <span data-ttu-id="ec2f3-123">Visual Studio Express crée les fichiers et dossiers suivants :</span><span class="sxs-lookup"><span data-stu-id="ec2f3-123">Visual Studio Express creates the following files and folders:</span></span>

- <span data-ttu-id="ec2f3-124">*Un MoviesController.cs* fichier dans le fichier *contrôleurs* dossier.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-124">*A MoviesController.cs* file in the project's *Controllers* folder.</span></span>
- <span data-ttu-id="ec2f3-125">A *films* dossier du projet *vues* dossier.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-125">A *Movies* folder in the project's *Views* folder.</span></span>
- <span data-ttu-id="ec2f3-126">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, et *Index.cshtml* dans le nouveau *Views\Movies* dossier.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-126">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="ec2f3-127">ASP.NET MVC 4 créée automatiquement la CRUD (créer, lire, mettre à jour et supprimer) des méthodes d’action et des vues pour vous (la création automatique de méthodes d’action CRUD et de vues est appelée génération de modèles automatique).</span><span class="sxs-lookup"><span data-stu-id="ec2f3-127">ASP.NET MVC 4 automatically created the CRUD (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="ec2f3-128">Vous disposez maintenant d’une application web entièrement fonctionnel qui vous permet de créer, répertorier, modifier et supprimer des entrées de film.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-128">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="ec2f3-129">Exécutez l’application et accédez à la `Movies` contrôleur en ajoutant */Movies* à l’URL dans la barre d’adresses de votre navigateur.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-129">Run the application and browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser.</span></span> <span data-ttu-id="ec2f3-130">Étant donné que l’application repose sur le routage par défaut (défini dans le *Global.asax* fichier), la demande de navigateur `http://localhost:xxxxx/Movies` est acheminé vers la valeur par défaut `Index` méthode d’action de la `Movies` contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-130">Because the application is relying on the default routing (defined in the *Global.asax* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="ec2f3-131">En d’autres termes, la demande de navigateur `http://localhost:xxxxx/Movies` est en fait identique à la demande du navigateur `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-131">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="ec2f3-132">Le résultat est une liste vide de films, car vous n’avez pas ajouté.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-132">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a><span data-ttu-id="ec2f3-133">Création d’un film</span><span class="sxs-lookup"><span data-stu-id="ec2f3-133">Creating a Movie</span></span>

<span data-ttu-id="ec2f3-134">Sélectionnez le lien **Créer nouveau**.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-134">Select the **Create New** link.</span></span> <span data-ttu-id="ec2f3-135">Entrez des détails sur un film, puis activez la **créer** bouton.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-135">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image3.png)

<span data-ttu-id="ec2f3-136">En cliquant sur le **créer** bouton, le formulaire est publié sur le serveur, où les informations de film sont enregistrées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-136">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="ec2f3-137">Vous êtes alors redirigé vers la */Movies* URL, où vous pouvez consulter le film nouvellement créé dans la liste.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-137">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

<span data-ttu-id="ec2f3-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span><span class="sxs-lookup"><span data-stu-id="ec2f3-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span></span>

<span data-ttu-id="ec2f3-139">Créez deux ou trois autres entrées.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-139">Create a couple more movie entries.</span></span> <span data-ttu-id="ec2f3-140">Essayez les liens **Edit**, **Details** et **Delete**, qui sont tous opérationnels.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-140">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="ec2f3-141">Examiner le Code généré</span><span class="sxs-lookup"><span data-stu-id="ec2f3-141">Examining the Generated Code</span></span>

<span data-ttu-id="ec2f3-142">Ouvrez le *Controllers\MoviesController.cs* de fichiers et d’examiner générées `Index` (méthode).</span><span class="sxs-lookup"><span data-stu-id="ec2f3-142">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="ec2f3-143">Une partie du contrôleur vidéo avec la `Index` méthode est indiquée ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-143">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="ec2f3-144">La ligne suivante à partir de la `MoviesController` classe instancie un contexte de base de données de film, comme décrit précédemment.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-144">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="ec2f3-145">Vous pouvez utiliser le contexte de base de données de film à interroger, modifier et supprimer des films.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-145">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

<span data-ttu-id="ec2f3-146">Une demande pour le `Movies` contrôleur retourne toutes les entrées de la `Movies` table de la base de données de film puis transmet les résultats à le `Index` vue.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-146">A request to the `Movies` controller returns all the entries in the `Movies` table of the movie database and then passes the results to the `Index` view.</span></span>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="ec2f3-147">Fortement typée de modèles et les @model (mot clé)</span><span class="sxs-lookup"><span data-stu-id="ec2f3-147">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="ec2f3-148">Plus haut dans ce didacticiel, vous avez vu comment un contrôleur peut passer des données ou des objets à un modèle de vue à l’aide de la `ViewBag` objet.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-148">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="ec2f3-149">Le `ViewBag` est un objet dynamique qui fournit un moyen pratique de la liaison tardive pour passer des informations à une vue.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-149">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="ec2f3-150">ASP.NET MVC offre également la possibilité de passer fortement typée des données ou des objets à un modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-150">ASP.NET MVC also provides the ability to pass strongly typed data or objects to a view template.</span></span> <span data-ttu-id="ec2f3-151">Cela fortement typées approche permet une meilleure compilation la vérification de votre code et le plus riche IntelliSense dans l’éditeur Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-151">This strongly typed approach enables better compile-time checking of your code and richer IntelliSense in the Visual Studio editor.</span></span> <span data-ttu-id="ec2f3-152">Le mécanisme de génération de modèles automatique dans Visual Studio utilisé cette approche avec la `MoviesController` classe et afficher des modèles lors de sa création les méthodes et les vues.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-152">The scaffolding mechanism in Visual Studio used this approach with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="ec2f3-153">Dans le *Controllers\MoviesController.cs* fichier examiner générées `Details` (méthode).</span><span class="sxs-lookup"><span data-stu-id="ec2f3-153">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="ec2f3-154">Une partie du contrôleur vidéo avec la `Details` méthode est indiquée ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-154">A portion of the movie controller with the `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

<span data-ttu-id="ec2f3-155">Si un `Movie` est trouvé, une instance de la `Movie` modèle est passé à l’affichage des détails.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-155">If a `Movie` is found, an instance of the `Movie` model is passed to the Details view.</span></span> <span data-ttu-id="ec2f3-156">Examinez le contenu de la *Views\Movies\Details.cshtml* fichier.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-156">Examine the contents of the *Views\Movies\Details.cshtml* file.</span></span>

<span data-ttu-id="ec2f3-157">En incluant un `@model` instruction en haut du fichier de modèle de vue, vous pouvez spécifier le type d’objet qui attend de la vue.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-157">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="ec2f3-158">Quand vous avez créé le contrôleur pour les films, Visual Studio a inclus automatiquement l’instruction `@model` suivante en haut du fichier *Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ec2f3-158">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

<span data-ttu-id="ec2f3-159">Cette directive `@model` vous permet d’accéder au film que le contrôleur a passé à la vue en utilisant un objet `Model` qui est fortement typé.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-159">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="ec2f3-160">Par exemple, dans le *Details.cshtml* modèle, le code transmet chaque champ de vidéo à la `DisplayNameFor` et [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) programmes d’assistance HTML avec fortement typé `Model` objet.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-160">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="ec2f3-161">Les méthodes de créer et modifier et les modèles d’affichage également passer un objet du modèle.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-161">The Create and Edit methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="ec2f3-162">Examinez le *Index.cshtml* afficher le modèle et le `Index` méthode dans le *MoviesController.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-162">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="ec2f3-163">Notez comment le code crée un [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) de l’objet lorsqu’il appelle le `View` méthode d’assistance dans le `Index` méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-163">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="ec2f3-164">Le code transmet ensuite `Movies` liste à partir du contrôleur à la vue :</span><span class="sxs-lookup"><span data-stu-id="ec2f3-164">The code then passes this `Movies` list from the controller to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

<span data-ttu-id="ec2f3-165">Lorsque vous avez créé le contrôleur de film, Visual Studio Express inclus automatiquement les éléments suivants `@model` instruction en haut de la *Index.cshtml* fichier :</span><span class="sxs-lookup"><span data-stu-id="ec2f3-165">When you created the movie controller, Visual Studio Express automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="ec2f3-166">Cela `@model` directive vous permet d’accéder à la liste de films que le contrôleur est passé à la vue par à l’aide un `Model` objet fortement typé.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-166">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="ec2f3-167">Par exemple, dans le *Index.cshtml* modèle, le code effectue une itération sur les films à cette occasion un `foreach` instruction sur fortement typé `Model` objet :</span><span class="sxs-lookup"><span data-stu-id="ec2f3-167">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="ec2f3-168">Étant donné que la `Model` objet est fortement typé (comme un `IEnumerable<Movie>` objet), chaque `item` objet dans la boucle est de type `Movie`.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-168">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="ec2f3-169">Entre autres avantages, cela signifie que vous obtenez la vérification de la compilation du code et prise en charge d’IntelliSense dans l’éditeur de code complète :</span><span class="sxs-lookup"><span data-stu-id="ec2f3-169">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="ec2f3-171">Utilisation de SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="ec2f3-171">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="ec2f3-172">Entity Framework Code First a détecté que la chaîne de connexion de base de données qui a été fournie sur laquelle pointe un `Movies` base de données qui n’existe pas encore, Code First créé la base de données automatiquement.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-172">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="ec2f3-173">Vous pouvez vérifier qu’elle a été créée en examinant le *application\_données* dossier.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-173">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="ec2f3-174">Si vous ne voyez pas le *Movies.mdf* de fichiers, cliquez sur le **afficher tous les fichiers** situé dans le **l’Explorateur de solutions** barre d’outils, cliquez sur le **Actualiser** bouton, puis cliquez sur le *application\_données* dossier.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-174">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="ec2f3-175">Double-cliquez sur *Movies.mdf* pour ouvrir **l’Explorateur de base de données**, puis développez le **Tables** dossier pour afficher la table de films.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-175">Double-click *Movies.mdf* to open **DATABASE EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span>

<span data-ttu-id="ec2f3-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="ec2f3-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span></span>

> [!NOTE]
> <span data-ttu-id="ec2f3-177">Si l’Explorateur de base de données n’apparaît pas, à partir de la **outils** menu, sélectionnez **se connecter à la base de données**, puis annulez la **choisir la Source de données** boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-177">If the database explorer doesn't appear, from the **TOOLS** menu, select **Connect to Database**, then cancel the **Choose Data Source** dialog.</span></span> <span data-ttu-id="ec2f3-178">Cela obligera ouvrir l’Explorateur de base de données.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-178">This will force open the database explorer.</span></span>


> [!NOTE]
> <span data-ttu-id="ec2f3-179">Si vous utilisez VWD ou Visual Studio 2010 et que vous obtenez une erreur semblable à une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="ec2f3-179">If you are using VWD or Visual Studio 2010 and get an error similar to any of the following following:</span></span>
> 
> - <span data-ttu-id="ec2f3-180">La base de données ' C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF' ne peut pas être ouvert, car il s’agit de la version 706.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-180">The database 'C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES.MDF' cannot be opened because it is version 706.</span></span> <span data-ttu-id="ec2f3-181">Ce serveur prend en charge la version 655 et les versions antérieure.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-181">This server supports version 655 and earlier.</span></span> <span data-ttu-id="ec2f3-182">Un chemin d’accès vers une version antérieure n’est pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-182">A downgrade path is not supported.</span></span>
> - <span data-ttu-id="ec2f3-183">&quot;Exception InvalidOperation a été gérée par le code utilisateur&quot; le SqlConnection fourni ne spécifie pas de catalogue initial.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-183">&quot;InvalidOperation Exception was unhandled by user code&quot; The supplied SqlConnection does not specify an initial catalog.</span></span>
> 
> <span data-ttu-id="ec2f3-184">Vous devez installer le [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) et [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span><span class="sxs-lookup"><span data-stu-id="ec2f3-184">You need to install the [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) and [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span></span> <span data-ttu-id="ec2f3-185">Vérifiez le `MovieDBContext` chaîne de connexion spécifiée dans la page précédente.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-185">Verify the `MovieDBContext` connection string specified on the previous page.</span></span>


<span data-ttu-id="ec2f3-186">Avec le bouton droit le `Movies` de table et sélectionnez **afficher les données de Table** pour afficher les données que vous avez créé.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-186">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image8.png)

<span data-ttu-id="ec2f3-187">Avec le bouton droit le `Movies` de table et sélectionnez **ouvrir la définition de Table** pour visualiser la table de la structure que Entity Framework Code First créé pour vous.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-187">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

<span data-ttu-id="ec2f3-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span><span class="sxs-lookup"><span data-stu-id="ec2f3-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span></span>

![](accessing-your-models-data-from-a-controller/_static/image10.png)

<span data-ttu-id="ec2f3-189">Notez comment le schéma de la `Movies` table correspond à la `Movie` classe que vous avez créé précédemment.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-189">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="ec2f3-190">Entity Framework Code First automatiquement créé ce schéma pour vous en fonction de votre `Movie` classe.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-190">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="ec2f3-191">Lorsque vous avez terminé, fermez la connexion en cliquant *MovieDBContext* et en sélectionnant **fermer la connexion**.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-191">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="ec2f3-192">(Si vous ne fermez pas la connexion, vous pouvez obtenir une erreur la prochaine fois que vous exécutez le projet).</span><span class="sxs-lookup"><span data-stu-id="ec2f3-192">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="ec2f3-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="ec2f3-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span></span>

<span data-ttu-id="ec2f3-194">Vous avez maintenant la base de données et une page de liste simple pour afficher le contenu à partir de celui-ci.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-194">You now have the database and a simple listing page to display content from it.</span></span> <span data-ttu-id="ec2f3-195">Dans l’étape suivante du didacticiel, nous allons examiner le reste du code du modèle généré automatiquement et ajouter un `SearchIndex` (méthode) et un `SearchIndex` vue qui vous permet de rechercher des vidéos dans cette base de données.</span><span class="sxs-lookup"><span data-stu-id="ec2f3-195">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ec2f3-196">[Précédent](adding-a-model.md)
[Suivant](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="ec2f3-196">[Previous](adding-a-model.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
