---
title: "Ajout d’un nouveau champ à une page Razor"
author: rick-anderson
description: "Montre comment ajouter un nouveau champ à une page Razor avec Entity Framework Core"
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 36412e9d1f3143f0d1999d0e754e6627f0984ad5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="adding-a-new-field-to-a-razor-page"></a><span data-ttu-id="c962d-103">Ajout d’un nouveau champ à une page Razor</span><span class="sxs-lookup"><span data-stu-id="c962d-103">Adding a new field to a Razor Page</span></span>

<span data-ttu-id="c962d-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c962d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c962d-105">Dans cette section, vous allez utiliser la fonctionnalité Migrations Code First [d’Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) pour ajouter un nouveau champ au modèle et migrer ce changement dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="c962d-105">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="c962d-106">Quand vous utilisez EF Code First pour créer automatiquement une base de données, Code First ajoute une table à la base de données pour déterminer si le schéma de la base de données est synchronisé avec les classes de modèle à partir desquelles il a été généré.</span><span class="sxs-lookup"><span data-stu-id="c962d-106">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="c962d-107">S’ils ne sont pas synchronisés, EF lève une exception.</span><span class="sxs-lookup"><span data-stu-id="c962d-107">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="c962d-108">Cela facilite la recherche de problèmes d’incohérence de code/de bases de données.</span><span class="sxs-lookup"><span data-stu-id="c962d-108">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="c962d-109">Ajout d’une propriété Rating au modèle Movie</span><span class="sxs-lookup"><span data-stu-id="c962d-109">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="c962d-110">Ouvrez le fichier *Models/Movie.cs* et ajoutez une propriété `Rating` :</span><span class="sxs-lookup"><span data-stu-id="c962d-110">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

<span data-ttu-id="c962d-111">Générez l’application (Ctrl+Maj+B).</span><span class="sxs-lookup"><span data-stu-id="c962d-111">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="c962d-112">Modifiez *Pages/Movies/Index.cshtml*et ajoutez un champ `Rating` :</span><span class="sxs-lookup"><span data-stu-id="c962d-112">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="c962d-113">Ajoutez le champ `Rating` aux pages Delete et Details.</span><span class="sxs-lookup"><span data-stu-id="c962d-113">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="c962d-114">Mettez à jour *Create.cshtml* avec un champ `Rating`.</span><span class="sxs-lookup"><span data-stu-id="c962d-114">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="c962d-115">Vous pouvez copier/coller l’élément `<div>` précédent et laisser IntelliSense vous aider à mettre à jour les champs.</span><span class="sxs-lookup"><span data-stu-id="c962d-115">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="c962d-116">IntelliSense fonctionne avec des [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="c962d-116">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Le développeur a tapé la lettre R comme valeur d’attribut asp-for dans le deuxième élément étiquette de la vue.](new-field/_static/cr.png)

<span data-ttu-id="c962d-120">Le code suivant montre *Create.cshtml* avec un champ `Rating` :</span><span class="sxs-lookup"><span data-stu-id="c962d-120">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="c962d-121">Ajoutez le champ `Rating` à la Page Edit.</span><span class="sxs-lookup"><span data-stu-id="c962d-121">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="c962d-122">L’application ne fonctionne pas tant que la base de données n’est pas mise à jour pour inclure le nouveau champ.</span><span class="sxs-lookup"><span data-stu-id="c962d-122">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="c962d-123">Si vous l’exécutez à présent, l’application lève une `SqlException` :</span><span class="sxs-lookup"><span data-stu-id="c962d-123">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="c962d-124">Cette erreur est due au fait que la classe du modèle Movie mise à jour est différente du schéma de la table Movie de la base de données.</span><span class="sxs-lookup"><span data-stu-id="c962d-124">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="c962d-125">(Il n’existe pas de colonne `Rating` dans la table de base de données.)</span><span class="sxs-lookup"><span data-stu-id="c962d-125">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="c962d-126">Plusieurs approches sont possibles pour résoudre l’erreur :</span><span class="sxs-lookup"><span data-stu-id="c962d-126">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="c962d-127">Laisser Entity Framework supprimer et recréer automatiquement la base de données à l’aide du nouveau schéma de classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="c962d-127">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="c962d-128">Cette approche est très utile au début du cycle de développement. Elle permet de faire évoluer rapidement le modèle et le schéma de base de données ensemble.</span><span class="sxs-lookup"><span data-stu-id="c962d-128">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="c962d-129">L’inconvénient est que vous perdez les données existantes dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="c962d-129">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="c962d-130">Il ne faut pas adopter cette approche sur une base de données de production.</span><span class="sxs-lookup"><span data-stu-id="c962d-130">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="c962d-131">La suppression de la base de données lors des changements de schéma et l’utilisation d’un initialiseur pour amorcer automatiquement la base de données avec des données de test est souvent un moyen efficace pour développer une application.</span><span class="sxs-lookup"><span data-stu-id="c962d-131">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="c962d-132">Modifier explicitement le schéma de la base de données existante pour le faire correspondre aux classes du modèle.</span><span class="sxs-lookup"><span data-stu-id="c962d-132">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="c962d-133">L’avantage de cette approche est que vous conservez vos données.</span><span class="sxs-lookup"><span data-stu-id="c962d-133">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="c962d-134">Vous pouvez apporter cette modification manuellement ou en créant un script de modification de la base de données.</span><span class="sxs-lookup"><span data-stu-id="c962d-134">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="c962d-135">Utilisez les migrations Code First pour mettre à jour le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="c962d-135">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="c962d-136">Pour ce didacticiel, nous allons utiliser les migrations Code First.</span><span class="sxs-lookup"><span data-stu-id="c962d-136">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="c962d-137">Mettez à jour la classe `SeedData` pour qu’elle fournisse une valeur pour la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="c962d-137">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="c962d-138">Vous pouvez voir un exemple de modification ci-dessous, mais elle doit être appliquée à chaque bloc `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="c962d-138">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="c962d-139">Consultez le [fichier SeedData.cs complet](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="c962d-139">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="c962d-140">Générez la solution.</span><span class="sxs-lookup"><span data-stu-id="c962d-140">Build the solution.</span></span>

<a name="pmc"></a> <span data-ttu-id="c962d-141">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet > Console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="c962d-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="c962d-142">Dans la console du Gestionnaire de package, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="c962d-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="c962d-143">La commande `Add-Migration` indique au framework qu’il doit :</span><span class="sxs-lookup"><span data-stu-id="c962d-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="c962d-144">Comparer le modèle `Movie` au schéma de base de données `Movie`</span><span class="sxs-lookup"><span data-stu-id="c962d-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="c962d-145">Créer du code pour migrer le schéma de base de données vers le nouveau modèle</span><span class="sxs-lookup"><span data-stu-id="c962d-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="c962d-146">Le nom « Rating » est arbitraire et utilisé pour nommer le fichier de migration.</span><span class="sxs-lookup"><span data-stu-id="c962d-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="c962d-147">Il est utile d’utiliser un nom explicite pour le fichier de migration.</span><span class="sxs-lookup"><span data-stu-id="c962d-147">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a> <span data-ttu-id="c962d-148">Si vous supprimez tous les enregistrements de la base de données, l’initialiseur amorce la base de données et inclut le champ `Rating`.</span><span class="sxs-lookup"><span data-stu-id="c962d-148">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="c962d-149">Pour ce faire, utilisez les liens de suppression disponibles dans le navigateur ou à partir de [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox).</span><span class="sxs-lookup"><span data-stu-id="c962d-149">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="c962d-150">Pour supprimer la base de données à partir de SSOX</span><span class="sxs-lookup"><span data-stu-id="c962d-150">To delete the database from SSOX:</span></span>

* <span data-ttu-id="c962d-151">Sélectionnez la base de données dans SSOX.</span><span class="sxs-lookup"><span data-stu-id="c962d-151">Select the database in SSOX.</span></span>
* <span data-ttu-id="c962d-152">Cliquez avec le bouton droit sur la base de données, puis sélectionnez *Supprimer*.</span><span class="sxs-lookup"><span data-stu-id="c962d-152">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="c962d-153">Cochez **Fermer les connexions existantes**.</span><span class="sxs-lookup"><span data-stu-id="c962d-153">Check **Close existing connections**.</span></span>
* <span data-ttu-id="c962d-154">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="c962d-154">Select **OK**.</span></span>
* <span data-ttu-id="c962d-155">Dans la [console du gestionnaire de package](xref:tutorials/razor-pages/new-field#pmc), mettez à jour la base de données :</span><span class="sxs-lookup"><span data-stu-id="c962d-155">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="c962d-156">Exécutez l’application et vérifiez que vous pouvez créer/modifier/afficher des films avec un champ `Rating`.</span><span class="sxs-lookup"><span data-stu-id="c962d-156">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="c962d-157">Si la base de données ne fait pas l’objet d’un seed, arrêtez IIS Express puis exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="c962d-157">If the database isn't seeded, stop IIS Express, and then run the app.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c962d-158">[Précédent : Ajout de la recherche](xref:tutorials/razor-pages/search)
[Suivant : Ajout de la validation](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="c962d-158">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
