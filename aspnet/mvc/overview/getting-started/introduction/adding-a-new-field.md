---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Ajout d’un nouveau champ | Documents Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 0dac798eba586cdcc232cedd262e610b954004df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-field"></a><span data-ttu-id="1613a-102">Ajout d’un nouveau champ</span><span class="sxs-lookup"><span data-stu-id="1613a-102">Adding a New Field</span></span>
====================
<span data-ttu-id="1613a-103">par [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="1613a-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="1613a-104">Dans cette section, vous utiliserez des Migrations Entity Framework Code First pour migrer certaines modifications apportées aux classes de modèle pour la modification est appliquée à la base de données.</span><span class="sxs-lookup"><span data-stu-id="1613a-104">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="1613a-105">Par défaut, lorsque vous utilisez Entity Framework Code First pour créer automatiquement une base de données, comme vous l’avez fait précédemment dans ce didacticiel, le premier Code ajoute une table pour faciliter le suivi si le schéma de la base de données est synchronisé avec les classes de modèle qu’il a été généré à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="1613a-105">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="1613a-106">Si elles ne sont pas synchronisés, Entity Framework génère une erreur.</span><span class="sxs-lookup"><span data-stu-id="1613a-106">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="1613a-107">Cela rend plus faciles à identifier les problèmes au moment du développement que vous pouvez sinon uniquement découvrir (par des erreurs obscurs) au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="1613a-107">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="1613a-108">Configuration des Migrations Code First pour les modifications apportées au modèle</span><span class="sxs-lookup"><span data-stu-id="1613a-108">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="1613a-109">Accédez à l’Explorateur de solutions.</span><span class="sxs-lookup"><span data-stu-id="1613a-109">Navigate to Solution Explorer.</span></span> <span data-ttu-id="1613a-110">Cliquez avec le bouton droit sur le *Movies.mdf* fichier et sélectionnez **supprimer** pour supprimer la base de données de films.</span><span class="sxs-lookup"><span data-stu-id="1613a-110">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span> <span data-ttu-id="1613a-111">Si vous ne voyez pas le *Movies.mdf* de fichiers, cliquez sur le **afficher tous les fichiers** icône ci-dessous dans le cadre rouge.</span><span class="sxs-lookup"><span data-stu-id="1613a-111">If you don't see the *Movies.mdf* file, click on the **Show All Files** icon shown below in the red outline.</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="1613a-112">Générez l’application pour vous assurer, qu'il n’y a aucune erreur.</span><span class="sxs-lookup"><span data-stu-id="1613a-112">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="1613a-113">À partir de la **outils** menu, cliquez sur **Gestionnaire de Package NuGet** , puis **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="1613a-113">From the **Tools** menu, click **NuGet Package Manager** and then **Package Manager Console**.</span></span>

![Ajout manuel de Pack](adding-a-new-field/_static/image2.png)

<span data-ttu-id="1613a-115">Dans le **Package Manager Console** fenêtre à la `PM>` invite Entrez</span><span class="sxs-lookup"><span data-stu-id="1613a-115">In the **Package Manager Console** window at the `PM>` prompt enter</span></span>

<span data-ttu-id="1613a-116">Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext</span><span class="sxs-lookup"><span data-stu-id="1613a-116">Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext</span></span>

![](adding-a-new-field/_static/image3.png)

<span data-ttu-id="1613a-117">Le **Enable-Migrations** (illustré ci-dessus) de commande crée un *Configuration.cs* fichier dans une nouvelle *Migrations* dossier.</span><span class="sxs-lookup"><span data-stu-id="1613a-117">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field/_static/image4.png)

<span data-ttu-id="1613a-118">Visual Studio ouvre le *Configuration.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="1613a-118">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="1613a-119">Remplacez le `Seed` méthode dans le *Configuration.cs* fichier avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="1613a-119">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

<span data-ttu-id="1613a-120">Placez le curseur sur la ligne ondulée rouge sous `Movie` et cliquez sur `Show Potential Fixes` puis cliquez sur **à l’aide de** **MvcMovie.Models ;**</span><span class="sxs-lookup"><span data-stu-id="1613a-120">Hover over the red squiggly line under `Movie` and click `Show Potential Fixes` and then click **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field/_static/image5.png)

<span data-ttu-id="1613a-121">Cela ajoute les éléments suivants à l’aide d’instruction :</span><span class="sxs-lookup"><span data-stu-id="1613a-121">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> <span data-ttu-id="1613a-122">Code appelle la méthode Migrations First le `Seed` méthode après chaque migration (autrement dit, l’appel **mise à jour de la base de données** dans la Console du Gestionnaire de Package), et cette méthode met à jour les lignes qui ont déjà été insérés, ou les insère si elles n’existent pas encore.</span><span class="sxs-lookup"><span data-stu-id="1613a-122">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>
> 
> <span data-ttu-id="1613a-123">Le [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) méthode dans le code suivant effectue une opération « upsert » :</span><span class="sxs-lookup"><span data-stu-id="1613a-123">The [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method in the following code performs an "upsert" operation:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> <span data-ttu-id="1613a-124">Étant donné que la [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) méthode s’exécute avec chaque migration, vous ne peut pas simplement insérer des données, car les lignes que vous essayez d’ajouter sera déjà présente après la migration initiale qui crée la base de données.</span><span class="sxs-lookup"><span data-stu-id="1613a-124">Because the [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) method runs with every migration, you can't just insert data, because the rows you are trying to add will already be there after the first migration that creates the database.</span></span> <span data-ttu-id="1613a-125">Le «[upsert](http://en.wikipedia.org/wiki/Upsert)« opération empêche les erreurs qui se passerait si vous essayez d’insérer une ligne qui existe déjà, mais elle substitue à toutes les modifications apportées aux données que vous avez apportées lors du test de l’application.</span><span class="sxs-lookup"><span data-stu-id="1613a-125">The "[upsert](http://en.wikipedia.org/wiki/Upsert)" operation prevents errors that would happen if you try to insert a row that already exists, but it overrides any changes to data that you may have made while testing the application.</span></span> <span data-ttu-id="1613a-126">Avec des données de test dans certaines tables vous pouvez qui doit se produire : dans certains cas lorsque vous modifiez des données pendant le test vous souhaitez que vos modifications restant après les mises à jour de la base de données.</span><span class="sxs-lookup"><span data-stu-id="1613a-126">With test data in some tables you might not want that to happen: in some cases when you change data while testing you want your changes to remain after database updates.</span></span> <span data-ttu-id="1613a-127">Dans ce cas, vous souhaitez effectuer une opération d’insertion conditionnel : insérer une ligne uniquement si elle n’existe pas déjà.</span><span class="sxs-lookup"><span data-stu-id="1613a-127">In that case you want to do a conditional insert operation: insert a row only if it doesn't already exist.</span></span>   
> 
> <span data-ttu-id="1613a-128">Le premier paramètre passé à la [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) méthode spécifie la propriété à utiliser pour vérifier si une ligne existe déjà.</span><span class="sxs-lookup"><span data-stu-id="1613a-128">The first parameter passed to the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method specifies the property to use to check if a row already exists.</span></span> <span data-ttu-id="1613a-129">Pour les données de film de test que vous fournissez, le `Title` propriété peut être utilisée à cet effet dans la mesure où chaque titre de la liste est unique :</span><span class="sxs-lookup"><span data-stu-id="1613a-129">For the test movie data that you are providing, the `Title` property can be used for this purpose since each title in the list is unique:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> <span data-ttu-id="1613a-130">Ce code part du principe que les titres sont uniques.</span><span class="sxs-lookup"><span data-stu-id="1613a-130">This code assumes that titles are unique.</span></span> <span data-ttu-id="1613a-131">Si vous ajoutez manuellement un titre en double, vous obtenez l’exception suivante, la prochaine fois que vous effectuez une migration.</span><span class="sxs-lookup"><span data-stu-id="1613a-131">If you manually add a duplicate title, you'll get the following exception the next time you perform a migration.</span></span>   
> 
>  <span data-ttu-id="1613a-132">*La séquence contient plusieurs éléments*</span><span class="sxs-lookup"><span data-stu-id="1613a-132">*Sequence contains more than one element*</span></span>  
> 
> <span data-ttu-id="1613a-133">Pour plus d’informations sur la [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) méthode, consultez [soyez prudent avec EF 4.3 AddOrUpdate méthode](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...</span><span class="sxs-lookup"><span data-stu-id="1613a-133">For more information about the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method, see [Take care with EF 4.3 AddOrUpdate Method](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)..</span></span>


<span data-ttu-id="1613a-134">**Appuyez sur CTRL-MAJ + B pour générer le projet.** (Les étapes suivantes échoue si vous ne créez pas à ce stade.)</span><span class="sxs-lookup"><span data-stu-id="1613a-134">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if you don't build at this point.)</span></span>

<span data-ttu-id="1613a-135">L’étape suivante consiste à créer un `DbMigration` classe pour la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="1613a-135">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="1613a-136">Cette migration crée une base de données, c’est pourquoi vous avez supprimé le *movie.mdf* fichier à l’étape précédente.</span><span class="sxs-lookup"><span data-stu-id="1613a-136">This migration creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="1613a-137">Dans le **Package Manager Console** fenêtre, entrez la commande `add-migration Initial` pour créer la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="1613a-137">In the **Package Manager Console** window, enter the command `add-migration Initial` to create the initial migration.</span></span> <span data-ttu-id="1613a-138">Le nom « Initial » est arbitraire et est utilisé pour nommer le fichier de migration créé.</span><span class="sxs-lookup"><span data-stu-id="1613a-138">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field/_static/image6.png)

<span data-ttu-id="1613a-139">Migrations Code First crée un autre fichier de classe dans le *Migrations* dossier (avec le nom *{DateStamp}\_Initial.cs* ), et cette classe contient le code qui crée le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="1613a-139">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="1613a-140">Le nom de fichier de migration est préalable fixe avec un horodatage pour aider à avec le classement.</span><span class="sxs-lookup"><span data-stu-id="1613a-140">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="1613a-141">Examinez le *{DateStamp}\_Initial.cs* fichier, il contient les instructions pour créer le `Movies` table pour la base de données de film.</span><span class="sxs-lookup"><span data-stu-id="1613a-141">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the `Movies` table for the Movie DB.</span></span> <span data-ttu-id="1613a-142">Lorsque vous mettez à jour la base de données dans les instructions ci-dessous, cela *{DateStamp}\_Initial.cs* fichier sera exécuté et créer le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="1613a-142">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="1613a-143">Le **Seed** méthode s’exécute pour remplir la base de données avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="1613a-143">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="1613a-144">Dans le **Package Manager Console**, entrez la commande `update-database` pour créer la base de données et exécuter le `Seed` (méthode).</span><span class="sxs-lookup"><span data-stu-id="1613a-144">In the **Package Manager Console**, enter the command `update-database` to create the database and run the `Seed` method.</span></span>

![](adding-a-new-field/_static/image7.png)

<span data-ttu-id="1613a-145">Si vous obtenez une erreur qui indique une table existe déjà et ne peut pas être créée, c’est probablement que vous avez exécuté l’application une fois que vous avez supprimé la base de données et avant l’exécution de `update-database`.</span><span class="sxs-lookup"><span data-stu-id="1613a-145">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="1613a-146">Dans ce cas, supprimez le *Movies.mdf* de fichiers à nouveau, puis réessayez la `update-database` commande.</span><span class="sxs-lookup"><span data-stu-id="1613a-146">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="1613a-147">Si vous obtenez toujours une erreur, supprimez le dossier migrations et le contenu, puis démarrer avec les instructions en haut de cette page (qui est de supprimer le *Movies.mdf* de fichier, puis passez à Enable-Migrations).</span><span class="sxs-lookup"><span data-stu-id="1613a-147">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span> <span data-ttu-id="1613a-148">Si vous obtenez toujours une erreur, ouvrez l’Explorateur d’objets SQL Server et supprimer la base de données de la liste.</span><span class="sxs-lookup"><span data-stu-id="1613a-148">If you still get an eror, open SQL Server Object Explorer and remove the database from the list.</span></span>

<span data-ttu-id="1613a-149">Exécutez l’application et accédez à la */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="1613a-149">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="1613a-150">Les données de valeur initiale s’affiche.</span><span class="sxs-lookup"><span data-stu-id="1613a-150">The seed data is displayed.</span></span>

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="1613a-151">Ajout d’une propriété Rating au modèle Movie</span><span class="sxs-lookup"><span data-stu-id="1613a-151">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="1613a-152">Commencez par ajouter un nouveau `Rating` à l’objet de propriété `Movie` classe.</span><span class="sxs-lookup"><span data-stu-id="1613a-152">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="1613a-153">Ouvrez le *Models\Movie.cs* et ajoutez le `Rating` propriété similaire à celle-ci :</span><span class="sxs-lookup"><span data-stu-id="1613a-153">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

<span data-ttu-id="1613a-154">Le texte complet `Movie` classe se présente comme le code suivant :</span><span class="sxs-lookup"><span data-stu-id="1613a-154">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

<span data-ttu-id="1613a-155">Générez l’application (Ctrl + Maj + B).</span><span class="sxs-lookup"><span data-stu-id="1613a-155">Build the application (Ctrl+Shift+B).</span></span>

<span data-ttu-id="1613a-156">Étant donné que vous avez ajouté un nouveau champ à la `Movie` (classe), vous devez également mettre à jour la liaison *liste blanche* pour cette nouvelle propriété sera incluse.</span><span class="sxs-lookup"><span data-stu-id="1613a-156">Because you've added a new field to the `Movie` class, you also need to update the binding *white list* so this new property will be included.</span></span> <span data-ttu-id="1613a-157">Mise à jour la `bind` attribut `Create` et `Edit` méthodes d’action pour inclure le `Rating` propriété :</span><span class="sxs-lookup"><span data-stu-id="1613a-157">Update the `bind` attribute for `Create` and `Edit` action methods to include the `Rating` property:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

<span data-ttu-id="1613a-158">Vous devez aussi mettre à jour les modèles de vue pour afficher, créer et modifier la nouvelle propriété `Rating` dans la vue du navigateur.</span><span class="sxs-lookup"><span data-stu-id="1613a-158">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="1613a-159">Ouvrez le *\Views\Movies\Index.cshtml* et ajoutez un `<th>Rating</th>` juste après l’en-tête de colonne le **prix** colonne.</span><span class="sxs-lookup"><span data-stu-id="1613a-159">Open the *\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="1613a-160">Ajoutez ensuite une `<td>` colonne vers la fin du modèle pour afficher le `@item.Rating` valeur.</span><span class="sxs-lookup"><span data-stu-id="1613a-160">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="1613a-161">Voici quelles mis à jour *Index.cshtml* afficher le modèle ressemble à :</span><span class="sxs-lookup"><span data-stu-id="1613a-161">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

<span data-ttu-id="1613a-162">Ensuite, ouvrez le *\Views\Movies\Create.cshtml* et ajoutez le `Rating` champ avec le balisage suivant distinctive.</span><span class="sxs-lookup"><span data-stu-id="1613a-162">Next, open the *\Views\Movies\Create.cshtml* file and add the `Rating` field with the following highlighed markup.</span></span> <span data-ttu-id="1613a-163">Cela restitue une zone de texte afin que vous pouvez spécifier un classement lors de la création d’un film de nouveau.</span><span class="sxs-lookup"><span data-stu-id="1613a-163">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

<span data-ttu-id="1613a-164">Vous avez maintenant mise à jour le code d’application pour prendre en charge la nouvelle `Rating` propriété.</span><span class="sxs-lookup"><span data-stu-id="1613a-164">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="1613a-165">Exécutez l’application et accédez à la */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="1613a-165">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="1613a-166">Dans ce cas, cependant, vous verrez les erreurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="1613a-166">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field/_static/image9.png)  
  
<span data-ttu-id="1613a-167">Le modèle de sauvegarde le contexte 'MovieDBContext' a changé depuis la création de la base de données.</span><span class="sxs-lookup"><span data-stu-id="1613a-167">The model backing the 'MovieDBContext' context has changed since the database was created.</span></span> <span data-ttu-id="1613a-168">Envisagez d’utiliser les Migrations Code First pour mettre à jour la base de données (https://go.microsoft.com/fwlink/?LinkId=238269).</span><span class="sxs-lookup"><span data-stu-id="1613a-168">Consider using Code First Migrations to update the database (https://go.microsoft.com/fwlink/?LinkId=238269).</span></span>

![](adding-a-new-field/_static/image10.png)

<span data-ttu-id="1613a-169">Vous voyez cette erreur, car la mise à jour `Movie` classe de modèle dans l’application est maintenant différent de celui du schéma de la `Movie` table de base de données existante.</span><span class="sxs-lookup"><span data-stu-id="1613a-169">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="1613a-170">(Il n’existe pas de colonne `Rating` dans la table de base de données.)</span><span class="sxs-lookup"><span data-stu-id="1613a-170">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="1613a-171">Plusieurs approches sont possibles pour résoudre l’erreur :</span><span class="sxs-lookup"><span data-stu-id="1613a-171">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="1613a-172">Laissez Entity Framework supprimer et recréer automatiquement la base de données sur la base du nouveau schéma de classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="1613a-172">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="1613a-173">Cette approche est très utile au début du cycle de développement quand vous effectuez un développement actif sur une base de données de test. Elle permet de faire évoluer rapidement le schéma de modèle et de base de données ensemble.</span><span class="sxs-lookup"><span data-stu-id="1613a-173">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="1613a-174">L’inconvénient, cependant, est que vous perdez les données existantes dans la base de données, donc vous *ne* à utiliser cette approche sur une base de données de production !</span><span class="sxs-lookup"><span data-stu-id="1613a-174">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="1613a-175">L’utilisation d’un initialiseur pour amorcer automatiquement une base de données avec des données de test est souvent un moyen efficace pour développer une application.</span><span class="sxs-lookup"><span data-stu-id="1613a-175">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span> <span data-ttu-id="1613a-176">Pour plus d’informations sur les initialiseurs de base de données Entity Framework, consultez [didacticiel d’ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="1613a-176">For more information on Entity Framework database initializers, see [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="1613a-177">Modifier explicitement le schéma de la base de données existante pour le faire correspondre aux classes du modèle.</span><span class="sxs-lookup"><span data-stu-id="1613a-177">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="1613a-178">L’avantage de cette approche est que vous conservez vos données.</span><span class="sxs-lookup"><span data-stu-id="1613a-178">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="1613a-179">Vous pouvez apporter cette modification manuellement ou en créant un script de modification de la base de données.</span><span class="sxs-lookup"><span data-stu-id="1613a-179">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="1613a-180">Utilisez Migrations Code First pour mettre à jour le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="1613a-180">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="1613a-181">Pour ce didacticiel, nous allons utiliser les migrations Code First.</span><span class="sxs-lookup"><span data-stu-id="1613a-181">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="1613a-182">Mettre à jour la méthode de valeur initiale afin qu’il fournit une valeur pour la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="1613a-182">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="1613a-183">Ouvrez le fichier de Migrations\Configuration.cs et ajouter un champ d’évaluation pour chaque objet de la vidéo.</span><span class="sxs-lookup"><span data-stu-id="1613a-183">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

<span data-ttu-id="1613a-184">Générez la solution, puis ouvrez le **Package Manager Console** fenêtre et entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="1613a-184">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration Rating`

<span data-ttu-id="1613a-185">Le `add-migration` commande indique à l’infrastructure de migration pour examiner le modèle de film actuel par le schéma de base de données de film actuel et de créer le code nécessaire pour migrer la base de données vers le nouveau modèle.</span><span class="sxs-lookup"><span data-stu-id="1613a-185">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="1613a-186">Le nom *évaluation* est arbitraire et est utilisé pour nommer le fichier de migration.</span><span class="sxs-lookup"><span data-stu-id="1613a-186">The name *Rating* is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="1613a-187">Il est utile d’utiliser un nom explicite pour l’étape de migration.</span><span class="sxs-lookup"><span data-stu-id="1613a-187">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="1613a-188">Une fois cette commande, Visual Studio ouvre le fichier de classe qui définit la nouvelle `DbMigration` classe dérivée, puis, dans le `Up` (méthode), vous pouvez voir le code qui crée la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="1613a-188">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

<span data-ttu-id="1613a-189">Générez la solution, puis entrez le `update-database` dans les **Package Manager Console** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="1613a-189">Build the solution, and then enter the `update-database` command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="1613a-190">L’illustration suivante montre la sortie dans le **Package Manager Console** fenêtre (le cachet de date ajoutant *évaluation* seront différents.)</span><span class="sxs-lookup"><span data-stu-id="1613a-190">The following image shows the output in the **Package Manager Console** window (The date stamp prepending *Rating* will be different.)</span></span>

![](adding-a-new-field/_static/image11.png)

<span data-ttu-id="1613a-191">Exécuter à nouveau l’application et accédez à l’URL /Movies.</span><span class="sxs-lookup"><span data-stu-id="1613a-191">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="1613a-192">Vous pouvez voir le nouveau champ de contrôle d’accès.</span><span class="sxs-lookup"><span data-stu-id="1613a-192">You can see the new Rating field.</span></span>

![](adding-a-new-field/_static/image12.png)

<span data-ttu-id="1613a-193">Cliquez sur le **créer un nouveau** pour ajouter un nouveau film.</span><span class="sxs-lookup"><span data-stu-id="1613a-193">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="1613a-194">Notez que vous pouvez ajouter un contrôle d’accès.</span><span class="sxs-lookup"><span data-stu-id="1613a-194">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field/_static/image13.png)

<span data-ttu-id="1613a-196">Cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="1613a-196">Click **Create**.</span></span> <span data-ttu-id="1613a-197">La nouvelle séquence, y compris l’évaluation, figure désormais dans les liste de films :</span><span class="sxs-lookup"><span data-stu-id="1613a-197">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

<span data-ttu-id="1613a-199">Maintenant que le projet est l’utilisation de migrations, vous n’aurez pas à supprimer de la base de données lorsque vous ajoutez un nouveau champ, ou sinon mettre à jour le schéma.</span><span class="sxs-lookup"><span data-stu-id="1613a-199">Now that the project is using migrations, you won't need to drop the database when you add a new field or otherwise update the schema.</span></span> <span data-ttu-id="1613a-200">Dans la section suivante, nous allons apporter d’autres modifications de schéma et utilisez les migrations à mettre à jour la base de données.</span><span class="sxs-lookup"><span data-stu-id="1613a-200">In the next section, we'll make more schema changes and use migrations to update the database.</span></span>

<span data-ttu-id="1613a-201">Vous devez également ajouter le `Rating` champ pour les modèles d’affichage Modifier, les détails et les supprimer.</span><span class="sxs-lookup"><span data-stu-id="1613a-201">You should also add the `Rating` field to the Edit, Details, and Delete view templates.</span></span>

<span data-ttu-id="1613a-202">Vous pouvez entrer la commande « mise à jour de base de données » dans le **Package Manager Console** à nouveau fenêtre et aucun code de migration sont exécuté, car le schéma correspond au modèle.</span><span class="sxs-lookup"><span data-stu-id="1613a-202">You could enter the "update-database" command in the **Package Manager Console** window again and no migration code would run, because the schema matches the model.</span></span> <span data-ttu-id="1613a-203">Toutefois, la « mise à jour de base de données » en cours d’exécution sera exécuté le `Seed` (méthode), et si vous avez modifié les données de valeur initiale, les modifications seront perdues, car le `Seed` méthode upserts données.</span><span class="sxs-lookup"><span data-stu-id="1613a-203">However, running "update-database" will run the `Seed` method again, and if you changed any of the Seed data, the changes will be lost because the `Seed` method upserts data.</span></span> <span data-ttu-id="1613a-204">Vous pouvez en savoir plus sur les `Seed` méthode dans de Tom Dykstra populaires [didacticiel d’ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="1613a-204">You can read more about the `Seed` method in Tom Dykstra's popular [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="1613a-205">Dans cette section, vous avez vu comment vous pouvez modifier les objets de modèle et synchroniser la base de données avec les modifications.</span><span class="sxs-lookup"><span data-stu-id="1613a-205">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="1613a-206">Vous avez également appris une méthode pour remplir une base de données nouvellement créée avec les exemples de données permettent de tester des scénarios.</span><span class="sxs-lookup"><span data-stu-id="1613a-206">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="1613a-207">Il s’agit d’une introduction rapide à Code First, consultez [création d’un modèle de données Entity Framework pour une Application ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) pour un didacticiel plus complète sur le sujet.</span><span class="sxs-lookup"><span data-stu-id="1613a-207">This was just a quick introduction to Code First, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) for a more complete tutorial on the subject.</span></span> <span data-ttu-id="1613a-208">Ensuite, nous allons voir comment vous pouvez ajouter la logique de validation plus riche pour les classes du modèle et activer des règles d’entreprise être appliqué.</span><span class="sxs-lookup"><span data-stu-id="1613a-208">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1613a-209">[Précédent](adding-search.md)
> [Suivant](adding-validation.md)</span><span class="sxs-lookup"><span data-stu-id="1613a-209">[Previous](adding-search.md)
[Next](adding-validation.md)</span></span>
