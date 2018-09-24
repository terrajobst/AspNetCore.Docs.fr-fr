---
title: Ajouter un nouveau champ à une page Razor dans ASP.NET Core
author: rick-anderson
description: Montre comment ajouter un nouveau champ à une page Razor avec Entity Framework Core
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: d6d59ff336095e2f1b8b2e9a0338b7791605ad7a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46010895"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="516f7-103">Ajouter un nouveau champ à une page Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="516f7-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="516f7-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="516f7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="516f7-105">Dans cette section, vous utilisez la fonctionnalité Migrations Code First [d’Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) pour ajouter un nouveau champ au modèle et migrer ce changement dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="516f7-105">In this section you use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="516f7-106">Quand vous utilisez EF Code First pour créer automatiquement une base de données, Code First :</span><span class="sxs-lookup"><span data-stu-id="516f7-106">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="516f7-107">Ajoute une table à la base de données pour déterminer si le schéma de la base de données est synchronisé avec les classes de modèle à partir desquelles il a été généré.</span><span class="sxs-lookup"><span data-stu-id="516f7-107">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="516f7-108">Si les classes de modèle ne sont pas synchronisées avec la base de données, EF lève une exception.</span><span class="sxs-lookup"><span data-stu-id="516f7-108">If the model classes aren't in sync with the DB, EF throws an exception.</span></span> 

<span data-ttu-id="516f7-109">La vérification automatique de la synchronisation du schéma et du modèle facilite la détection des problèmes d’incohérence et de code de base de données.</span><span class="sxs-lookup"><span data-stu-id="516f7-109">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="516f7-110">Ajout d’une propriété Rating au modèle Movie</span><span class="sxs-lookup"><span data-stu-id="516f7-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="516f7-111">Ouvrez le fichier *Models/Movie.cs* et ajoutez une propriété `Rating` :</span><span class="sxs-lookup"><span data-stu-id="516f7-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]

::: moniker-end

<span data-ttu-id="516f7-112">Générez l’application (Ctrl+Maj+B).</span><span class="sxs-lookup"><span data-stu-id="516f7-112">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="516f7-113">Modifiez *Pages/Movies/Index.cshtml*et ajoutez un champ `Rating` :</span><span class="sxs-lookup"><span data-stu-id="516f7-113">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="516f7-114">Ajoutez le champ `Rating` aux pages Delete et Details.</span><span class="sxs-lookup"><span data-stu-id="516f7-114">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="516f7-115">Mettez à jour *Create.cshtml* avec un champ `Rating`.</span><span class="sxs-lookup"><span data-stu-id="516f7-115">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="516f7-116">Vous pouvez copier/coller l’élément `<div>` précédent et laisser IntelliSense vous aider à mettre à jour les champs.</span><span class="sxs-lookup"><span data-stu-id="516f7-116">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="516f7-117">IntelliSense fonctionne avec des [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="516f7-117">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Le développeur a tapé la lettre R comme valeur d’attribut asp-for dans le deuxième élément étiquette de la vue.](new-field/_static/cr.png)

<span data-ttu-id="516f7-121">Le code suivant montre *Create.cshtml* avec un champ `Rating` :</span><span class="sxs-lookup"><span data-stu-id="516f7-121">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="516f7-122">Ajoutez le champ `Rating` à la Page Edit.</span><span class="sxs-lookup"><span data-stu-id="516f7-122">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="516f7-123">L’application ne fonctionne pas tant que la base de données n’est pas mise à jour pour inclure le nouveau champ.</span><span class="sxs-lookup"><span data-stu-id="516f7-123">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="516f7-124">Si vous l’exécutez à présent, l’application lève une `SqlException` :</span><span class="sxs-lookup"><span data-stu-id="516f7-124">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="516f7-125">Cette erreur est due au fait que la classe du modèle Movie mise à jour est différente du schéma de la table Movie de la base de données.</span><span class="sxs-lookup"><span data-stu-id="516f7-125">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="516f7-126">(Il n’existe pas de colonne `Rating` dans la table de base de données.)</span><span class="sxs-lookup"><span data-stu-id="516f7-126">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="516f7-127">Plusieurs approches sont possibles pour résoudre l’erreur :</span><span class="sxs-lookup"><span data-stu-id="516f7-127">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="516f7-128">Laisser Entity Framework supprimer et recréer automatiquement la base de données à l’aide du nouveau schéma de classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="516f7-128">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="516f7-129">Cette approche est très utile au début du cycle de développement. Elle permet de faire évoluer rapidement le modèle et le schéma de base de données ensemble.</span><span class="sxs-lookup"><span data-stu-id="516f7-129">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="516f7-130">L’inconvénient est que vous perdez les données existantes dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="516f7-130">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="516f7-131">Il ne faut pas adopter cette approche sur une base de données de production.</span><span class="sxs-lookup"><span data-stu-id="516f7-131">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="516f7-132">La suppression de la base de données lors des changements de schéma et l’utilisation d’un initialiseur pour amorcer automatiquement la base de données avec des données de test est souvent un moyen efficace pour développer une application.</span><span class="sxs-lookup"><span data-stu-id="516f7-132">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="516f7-133">Modifier explicitement le schéma de la base de données existante pour le faire correspondre aux classes du modèle.</span><span class="sxs-lookup"><span data-stu-id="516f7-133">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="516f7-134">L’avantage de cette approche est que vous conservez vos données.</span><span class="sxs-lookup"><span data-stu-id="516f7-134">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="516f7-135">Vous pouvez apporter cette modification manuellement ou en créant un script de modification de la base de données.</span><span class="sxs-lookup"><span data-stu-id="516f7-135">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="516f7-136">Utilisez les migrations Code First pour mettre à jour le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="516f7-136">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="516f7-137">Pour ce didacticiel, nous allons utiliser les migrations Code First.</span><span class="sxs-lookup"><span data-stu-id="516f7-137">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="516f7-138">Mettez à jour la classe `SeedData` pour qu’elle fournisse une valeur pour la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="516f7-138">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="516f7-139">Vous pouvez voir un exemple de modification ci-dessous, mais elle doit être appliquée à chaque bloc `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="516f7-139">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="516f7-140">Consultez le [fichier SeedData.cs complet](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="516f7-140">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="516f7-141">Consultez le [fichier SeedData.cs complet](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="516f7-141">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).</span></span>

::: moniker-end

<span data-ttu-id="516f7-142">Générez la solution.</span><span class="sxs-lookup"><span data-stu-id="516f7-142">Build the solution.</span></span>

<a name="pmc"></a> <span data-ttu-id="516f7-143">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet > Console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="516f7-143">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="516f7-144">Dans la console du Gestionnaire de package, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="516f7-144">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="516f7-145">La commande `Add-Migration` indique au framework qu’il doit :</span><span class="sxs-lookup"><span data-stu-id="516f7-145">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="516f7-146">Comparer le modèle `Movie` au schéma de base de données `Movie`</span><span class="sxs-lookup"><span data-stu-id="516f7-146">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="516f7-147">Créer du code pour migrer le schéma de base de données vers le nouveau modèle</span><span class="sxs-lookup"><span data-stu-id="516f7-147">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="516f7-148">Le nom « Rating » est arbitraire et utilisé pour nommer le fichier de migration.</span><span class="sxs-lookup"><span data-stu-id="516f7-148">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="516f7-149">Il est utile d’utiliser un nom explicite pour le fichier de migration.</span><span class="sxs-lookup"><span data-stu-id="516f7-149">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a> <span data-ttu-id="516f7-150">Si vous supprimez tous les enregistrements de la base de données, l’initialiseur amorce la base de données et inclut le champ `Rating`.</span><span class="sxs-lookup"><span data-stu-id="516f7-150">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="516f7-151">Pour ce faire, utilisez les liens de suppression disponibles dans le navigateur ou à partir de [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox).</span><span class="sxs-lookup"><span data-stu-id="516f7-151">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="516f7-152">Pour supprimer la base de données à partir de SSOX</span><span class="sxs-lookup"><span data-stu-id="516f7-152">To delete the database from SSOX:</span></span>

* <span data-ttu-id="516f7-153">Sélectionnez la base de données dans SSOX.</span><span class="sxs-lookup"><span data-stu-id="516f7-153">Select the database in SSOX.</span></span>
* <span data-ttu-id="516f7-154">Cliquez avec le bouton droit sur la base de données, puis sélectionnez *Supprimer*.</span><span class="sxs-lookup"><span data-stu-id="516f7-154">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="516f7-155">Cochez **Fermer les connexions existantes**.</span><span class="sxs-lookup"><span data-stu-id="516f7-155">Check **Close existing connections**.</span></span>
* <span data-ttu-id="516f7-156">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="516f7-156">Select **OK**.</span></span>
* <span data-ttu-id="516f7-157">Dans la [console du gestionnaire de package](xref:tutorials/razor-pages/new-field#pmc), mettez à jour la base de données :</span><span class="sxs-lookup"><span data-stu-id="516f7-157">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="516f7-158">Exécutez l’application et vérifiez que vous pouvez créer/modifier/afficher des films avec un champ `Rating`.</span><span class="sxs-lookup"><span data-stu-id="516f7-158">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="516f7-159">Si la base de données ne fait pas l’objet d’un seed, arrêtez IIS Express puis exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="516f7-159">If the database isn't seeded, stop IIS Express, and then run the app.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="516f7-160">[Précédent : Ajout de la recherche](xref:tutorials/razor-pages/search)
> [Suivant : Ajout de la validation](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="516f7-160">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
