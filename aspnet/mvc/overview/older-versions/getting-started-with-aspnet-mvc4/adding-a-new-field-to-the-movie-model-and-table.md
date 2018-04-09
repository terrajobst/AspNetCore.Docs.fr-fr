---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Ajout d’un nouveau champ à la Table et au modèle de film | Documents Microsoft
author: Rick-Anderson
description: 'Remarque : Une version mise à jour de ce didacticiel est disponible ici qui utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sécurisé, beaucoup plus simple à suivre et de démonstration...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: d8a42e9acdce687ab6e9742071dd2949f244622f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a><span data-ttu-id="291fa-104">Ajout d’un nouveau champ à la Table et au modèle de film</span><span class="sxs-lookup"><span data-stu-id="291fa-104">Adding a New Field to the Movie Model and Table</span></span>
====================
<span data-ttu-id="291fa-105">par [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="291fa-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="291fa-106">Une version mise à jour de ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) qui utilise ASP.NET MVC 5 et Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="291fa-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="291fa-107">Il est plus sécurisé, beaucoup plus simple à suivre et illustre plusieurs fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="291fa-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="291fa-108">Dans cette section, vous utiliserez des Migrations Entity Framework Code First pour migrer certaines modifications apportées aux classes de modèle pour la modification est appliquée à la base de données.</span><span class="sxs-lookup"><span data-stu-id="291fa-108">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="291fa-109">Par défaut, lorsque vous utilisez Entity Framework Code First pour créer automatiquement une base de données, comme vous l’avez fait précédemment dans ce didacticiel, le premier Code ajoute une table pour faciliter le suivi si le schéma de la base de données est synchronisé avec les classes de modèle qu’il a été généré à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="291fa-109">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="291fa-110">Si elles ne sont pas synchronisés, Entity Framework génère une erreur.</span><span class="sxs-lookup"><span data-stu-id="291fa-110">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="291fa-111">Cela rend plus faciles à identifier les problèmes au moment du développement que vous pouvez sinon uniquement découvrir (par des erreurs obscurs) au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="291fa-111">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="291fa-112">Configuration des Migrations Code First pour les modifications apportées au modèle</span><span class="sxs-lookup"><span data-stu-id="291fa-112">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="291fa-113">Si vous utilisez Visual Studio 2012, double-cliquez sur le *Movies.mdf* fichier à partir de l’Explorateur de solutions pour ouvrir l’outil de base de données.</span><span class="sxs-lookup"><span data-stu-id="291fa-113">If you are using Visual Studio 2012, double click the *Movies.mdf* file from Solution Explorer to open the database tool.</span></span> <span data-ttu-id="291fa-114">Visual Studio Express pour Web affiche l’Explorateur de base de données, que Visual Studio 2012 affiche l’Explorateur de serveurs.</span><span class="sxs-lookup"><span data-stu-id="291fa-114">Visual Studio Express for Web will show Database Explorer, Visual Studio 2012 will show Server Explorer.</span></span> <span data-ttu-id="291fa-115">Si vous utilisez Visual Studio 2010, utilisez l’Explorateur d’objets SQL Server.</span><span class="sxs-lookup"><span data-stu-id="291fa-115">If you are using Visual Studio 2010, use SQL Server Object Explorer.</span></span>

<span data-ttu-id="291fa-116">Dans l’outil de base de données (Explorateur de base de données, l’Explorateur de serveurs ou l’Explorateur d’objets SQL Server), avec le bouton droit, cliquez sur `MovieDBContext` et sélectionnez **supprimer** pour supprimer la base de données de films.</span><span class="sxs-lookup"><span data-stu-id="291fa-116">In the database tool (Database Explorer, Server Explorer or SQL Server Object Explorer), right click on `MovieDBContext` and select **Delete** to drop the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

<span data-ttu-id="291fa-117">Accédez à l’Explorateur de solutions.</span><span class="sxs-lookup"><span data-stu-id="291fa-117">Navigate back to Solution Explorer.</span></span> <span data-ttu-id="291fa-118">Cliquez avec le bouton droit sur le *Movies.mdf* fichier et sélectionnez **supprimer** pour supprimer la base de données de films.</span><span class="sxs-lookup"><span data-stu-id="291fa-118">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

<span data-ttu-id="291fa-119">Générez l’application pour vous assurer, qu'il n’y a aucune erreur.</span><span class="sxs-lookup"><span data-stu-id="291fa-119">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="291fa-120">À partir de la **outils** menu, cliquez sur **Gestionnaire de Package de bibliothèque** , puis **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="291fa-120">From the **Tools** menu, click **Library Package Manager** and then **Package Manager Console**.</span></span>

![Ajout manuel de Pack](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

<span data-ttu-id="291fa-122">Dans le **Package Manager Console** fenêtre à la `PM>` invite Entrez « Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext ».</span><span class="sxs-lookup"><span data-stu-id="291fa-122">In the **Package Manager Console** window at the `PM>` prompt enter "Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext".</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

<span data-ttu-id="291fa-123">Le **Enable-Migrations** (illustré ci-dessus) de commande crée un *Configuration.cs* fichier dans une nouvelle *Migrations* dossier.</span><span class="sxs-lookup"><span data-stu-id="291fa-123">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

<span data-ttu-id="291fa-124">Visual Studio ouvre le *Configuration.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="291fa-124">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="291fa-125">Remplacez le `Seed` méthode dans le *Configuration.cs* fichier avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="291fa-125">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

<span data-ttu-id="291fa-126">Cliquez avec le bouton droit sur la ligne ondulée rouge sous `Movie` et sélectionnez **résoudre** puis **à l’aide de** **MvcMovie.Models ;**</span><span class="sxs-lookup"><span data-stu-id="291fa-126">Right click on the red squiggly line under `Movie` and select **Resolve** then **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

<span data-ttu-id="291fa-127">Cela ajoute les éléments suivants à l’aide d’instruction :</span><span class="sxs-lookup"><span data-stu-id="291fa-127">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="291fa-128">Code appelle la méthode Migrations First le `Seed` méthode après chaque migration (autrement dit, l’appel **mise à jour de la base de données** dans la Console du Gestionnaire de Package), et cette méthode met à jour les lignes qui ont déjà été insérés, ou les insère si elles n’existent pas encore.</span><span class="sxs-lookup"><span data-stu-id="291fa-128">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>


<span data-ttu-id="291fa-129">**Appuyez sur CTRL-MAJ + B pour générer le projet.** (Les étapes suivantes échoue si votre ne créez pas à ce stade.)</span><span class="sxs-lookup"><span data-stu-id="291fa-129">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if your don't build at this point.)</span></span>

<span data-ttu-id="291fa-130">L’étape suivante consiste à créer un `DbMigration` classe pour la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="291fa-130">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="291fa-131">Cette migration vers crée une base de données, c’est pourquoi vous avez supprimé le *movie.mdf* fichier à l’étape précédente.</span><span class="sxs-lookup"><span data-stu-id="291fa-131">This migration to creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="291fa-132">Dans le **Package Manager Console** fenêtre, entrez la commande « Ajouter-migration initiale » pour créer la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="291fa-132">In the **Package Manager Console** window, enter the command "add-migration Initial" to create the initial migration.</span></span> <span data-ttu-id="291fa-133">Le nom « Initial » est arbitraire et est utilisé pour nommer le fichier de migration créé.</span><span class="sxs-lookup"><span data-stu-id="291fa-133">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

<span data-ttu-id="291fa-134">Migrations Code First crée un autre fichier de classe dans le *Migrations* dossier (avec le nom *{DateStamp}\_Initial.cs* ), et cette classe contient le code qui crée le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="291fa-134">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="291fa-135">Le nom de fichier de migration est préalable fixe avec un horodatage pour aider à avec le classement.</span><span class="sxs-lookup"><span data-stu-id="291fa-135">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="291fa-136">Examinez le *{DateStamp}\_Initial.cs* fichier, il contient les instructions pour créer la table de films pour la base de données de film.</span><span class="sxs-lookup"><span data-stu-id="291fa-136">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the Movies table for the Movie DB.</span></span> <span data-ttu-id="291fa-137">Lorsque vous mettez à jour la base de données dans les instructions ci-dessous, cela *{DateStamp}\_Initial.cs* fichier sera exécuté et créer le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="291fa-137">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="291fa-138">Le **Seed** méthode s’exécute pour remplir la base de données avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="291fa-138">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="291fa-139">Dans le **Package Manager Console**, entrez la commande « mise à jour de base de données » pour créer la base de données et exécuter le **Seed** (méthode).</span><span class="sxs-lookup"><span data-stu-id="291fa-139">In the **Package Manager Console**, enter the command "update-database" to create the database and run the **Seed** method.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

<span data-ttu-id="291fa-140">Si vous obtenez une erreur qui indique une table existe déjà et ne peut pas être créée, c’est probablement que vous avez exécuté l’application une fois que vous avez supprimé la base de données et avant l’exécution de `update-database`.</span><span class="sxs-lookup"><span data-stu-id="291fa-140">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="291fa-141">Dans ce cas, supprimez le *Movies.mdf* de fichiers à nouveau, puis réessayez la `update-database` commande.</span><span class="sxs-lookup"><span data-stu-id="291fa-141">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="291fa-142">Si vous obtenez toujours une erreur, supprimez le dossier migrations et le contenu, puis démarrer avec les instructions en haut de cette page (qui est de supprimer le *Movies.mdf* de fichier, puis passez à Enable-Migrations).</span><span class="sxs-lookup"><span data-stu-id="291fa-142">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span>

<span data-ttu-id="291fa-143">Exécutez l’application et accédez à la */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="291fa-143">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="291fa-144">Les données de valeur initiale s’affiche.</span><span class="sxs-lookup"><span data-stu-id="291fa-144">The seed data is displayed.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="291fa-145">Ajout d’une propriété Rating au modèle Movie</span><span class="sxs-lookup"><span data-stu-id="291fa-145">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="291fa-146">Commencez par ajouter un nouveau `Rating` à l’objet de propriété `Movie` classe.</span><span class="sxs-lookup"><span data-stu-id="291fa-146">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="291fa-147">Ouvrez le *Models\Movie.cs* et ajoutez le `Rating` propriété similaire à celle-ci :</span><span class="sxs-lookup"><span data-stu-id="291fa-147">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

<span data-ttu-id="291fa-148">Le texte complet `Movie` classe se présente comme le code suivant :</span><span class="sxs-lookup"><span data-stu-id="291fa-148">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

<span data-ttu-id="291fa-149">Générez l’application à l’aide de la **générer** &gt; **générer un film** menu de commande ou en appuyant sur CTRL-MAJ-B.</span><span class="sxs-lookup"><span data-stu-id="291fa-149">Build the application using the **Build** &gt;**Build Movie** menu command or by pressing CTRL-SHIFT-B.</span></span>

<span data-ttu-id="291fa-150">Maintenant que vous avez mis à jour le `Model` (classe), vous devez également mettre à jour le *\Views\Movies\Index.cshtml* et *\Views\Movies\Create.cshtml* afficher les modèles afin d’afficher la nouvelle `Rating`propriété dans la vue du navigateur.</span><span class="sxs-lookup"><span data-stu-id="291fa-150">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to display the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="291fa-151">Ouvrez le<em>\Views\Movies\Index.cshtml</em> et ajoutez un `<th>Rating</th>` juste après l’en-tête de colonne le <strong>prix</strong> colonne.</span><span class="sxs-lookup"><span data-stu-id="291fa-151">Open the<em>\Views\Movies\Index.cshtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="291fa-152">Ajoutez ensuite une `<td>` colonne vers la fin du modèle pour afficher le `@item.Rating` valeur.</span><span class="sxs-lookup"><span data-stu-id="291fa-152">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="291fa-153">Voici quelles mis à jour <em>Index.cshtml</em> afficher le modèle ressemble à :</span><span class="sxs-lookup"><span data-stu-id="291fa-153">Below is what the updated <em>Index.cshtml</em> view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

<span data-ttu-id="291fa-154">Ensuite, ouvrez le *\Views\Movies\Create.cshtml* et ajoutez le balisage suivant à la fin du formulaire.</span><span class="sxs-lookup"><span data-stu-id="291fa-154">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="291fa-155">Cela restitue une zone de texte afin que vous pouvez spécifier un classement lors de la création d’un film de nouveau.</span><span class="sxs-lookup"><span data-stu-id="291fa-155">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

<span data-ttu-id="291fa-156">Vous avez maintenant mise à jour le code d’application pour prendre en charge la nouvelle `Rating` propriété.</span><span class="sxs-lookup"><span data-stu-id="291fa-156">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="291fa-157">Maintenant, exécutez l’application et accédez à la */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="291fa-157">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="291fa-158">Dans ce cas, cependant, vous verrez les erreurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="291fa-158">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

<span data-ttu-id="291fa-159">Vous voyez cette erreur, car la mise à jour `Movie` classe de modèle dans l’application est maintenant différent de celui du schéma de la `Movie` table de base de données existante.</span><span class="sxs-lookup"><span data-stu-id="291fa-159">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="291fa-160">(Il n’existe pas de colonne `Rating` dans la table de base de données.)</span><span class="sxs-lookup"><span data-stu-id="291fa-160">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="291fa-161">Plusieurs approches sont possibles pour résoudre l’erreur :</span><span class="sxs-lookup"><span data-stu-id="291fa-161">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="291fa-162">Laissez Entity Framework supprimer et recréer automatiquement la base de données sur la base du nouveau schéma de classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="291fa-162">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="291fa-163">Cette approche est très utile lors de l’exécution de développement actif sur une base de données de test ; Il permet de faire évoluer rapidement le schéma de modèle et de la base de données entre eux.</span><span class="sxs-lookup"><span data-stu-id="291fa-163">This approach is very convenient when doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="291fa-164">L’inconvénient, cependant, est que vous perdez les données existantes dans la base de données, donc vous *ne* à utiliser cette approche sur une base de données de production !</span><span class="sxs-lookup"><span data-stu-id="291fa-164">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="291fa-165">À l’aide d’un initialiseur pour amorcer automatiquement d’une base de données avec des données de test est souvent développer une application de façon productive.</span><span class="sxs-lookup"><span data-stu-id="291fa-165">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="291fa-166">Pour plus d’informations sur les initialiseurs de base de données Entity Framework, consultez de Tom Dykstra [didacticiel d’ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="291fa-166">For more information on Entity Framework database initializers, see Tom Dykstra's [ASP.NET MVC/Entity Framework tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="291fa-167">Modifier explicitement le schéma de la base de données existante pour le faire correspondre aux classes du modèle.</span><span class="sxs-lookup"><span data-stu-id="291fa-167">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="291fa-168">L’avantage de cette approche est que vous conservez vos données.</span><span class="sxs-lookup"><span data-stu-id="291fa-168">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="291fa-169">Vous pouvez apporter cette modification manuellement ou en créant un script de modification de la base de données.</span><span class="sxs-lookup"><span data-stu-id="291fa-169">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="291fa-170">Utilisez Migrations Code First pour mettre à jour le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="291fa-170">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="291fa-171">Pour ce didacticiel, nous allons utiliser les migrations Code First.</span><span class="sxs-lookup"><span data-stu-id="291fa-171">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="291fa-172">Mettre à jour la méthode de valeur initiale afin qu’il fournit une valeur pour la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="291fa-172">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="291fa-173">Ouvrez le fichier de Migrations\Configuration.cs et ajouter un champ d’évaluation pour chaque objet de la vidéo.</span><span class="sxs-lookup"><span data-stu-id="291fa-173">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

<span data-ttu-id="291fa-174">Générez la solution, puis ouvrez le **Package Manager Console** fenêtre et entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="291fa-174">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration AddRatingMig`

<span data-ttu-id="291fa-175">Le `add-migration` commande indique à l’infrastructure de migration pour examiner le modèle de film actuel par le schéma de base de données de film actuel et de créer le code nécessaire pour migrer la base de données vers le nouveau modèle.</span><span class="sxs-lookup"><span data-stu-id="291fa-175">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="291fa-176">Le AddRatingMig est arbitraire et est utilisé pour nommer le fichier de migration.</span><span class="sxs-lookup"><span data-stu-id="291fa-176">The AddRatingMig is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="291fa-177">Il est utile d’utiliser un nom explicite pour l’étape de migration.</span><span class="sxs-lookup"><span data-stu-id="291fa-177">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="291fa-178">Une fois cette commande, Visual Studio ouvre le fichier de classe qui définit la nouvelle `DbMIgration` classe dérivée, puis, dans le `Up` (méthode), vous pouvez voir le code qui crée la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="291fa-178">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

<span data-ttu-id="291fa-179">Générez la solution, puis entrez la commande « mise à jour de base de données » dans le **Package Manager Console** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="291fa-179">Build the solution, and then enter the "update-database" command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="291fa-180">L’illustration suivante montre la sortie dans le **Package Manager Console** fenêtre (le cachet de date ajoutant AddRatingMig sera différent.)</span><span class="sxs-lookup"><span data-stu-id="291fa-180">The following image shows the output in the **Package Manager Console** window (The date stamp prepending AddRatingMig will be different.)</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

<span data-ttu-id="291fa-181">Exécuter à nouveau l’application et accédez à l’URL /Movies.</span><span class="sxs-lookup"><span data-stu-id="291fa-181">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="291fa-182">Vous pouvez voir le nouveau champ de contrôle d’accès.</span><span class="sxs-lookup"><span data-stu-id="291fa-182">You can see the new Rating field.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

<span data-ttu-id="291fa-183">Cliquez sur le **créer un nouveau** pour ajouter un nouveau film.</span><span class="sxs-lookup"><span data-stu-id="291fa-183">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="291fa-184">Notez que vous pouvez ajouter un contrôle d’accès.</span><span class="sxs-lookup"><span data-stu-id="291fa-184">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

<span data-ttu-id="291fa-186">Cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="291fa-186">Click **Create**.</span></span> <span data-ttu-id="291fa-187">La nouvelle séquence, y compris l’évaluation, figure désormais dans les liste de films :</span><span class="sxs-lookup"><span data-stu-id="291fa-187">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

<span data-ttu-id="291fa-189">Vous devez également ajouter le `Rating` champ SearchIndex, détails et modifier les modèles d’affichage.</span><span class="sxs-lookup"><span data-stu-id="291fa-189">You should also add the `Rating` field to the Edit, Details and SearchIndex view templates.</span></span>

<span data-ttu-id="291fa-190">Vous pouvez entrer la commande « mise à jour de base de données » dans le **Package Manager Console** fenêtre à nouveau et aucune modification devant être apportées, car le schéma correspond au modèle.</span><span class="sxs-lookup"><span data-stu-id="291fa-190">You could enter the "update-database" command in the **Package Manager Console** window again and no changes would be made, because the schema matches the model.</span></span>

<span data-ttu-id="291fa-191">Dans cette section, vous avez vu comment vous pouvez modifier les objets de modèle et synchroniser la base de données avec les modifications.</span><span class="sxs-lookup"><span data-stu-id="291fa-191">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="291fa-192">Vous avez également appris une méthode pour remplir une base de données nouvellement créée avec les exemples de données permettent de tester des scénarios.</span><span class="sxs-lookup"><span data-stu-id="291fa-192">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="291fa-193">Ensuite, nous allons voir comment vous pouvez ajouter la logique de validation plus riche pour les classes du modèle et activer des règles d’entreprise être appliqué.</span><span class="sxs-lookup"><span data-stu-id="291fa-193">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="291fa-194">[Précédent](examining-the-edit-methods-and-edit-view.md)
> [Suivant](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="291fa-194">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
