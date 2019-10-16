---
title: Ajouter un nouveau champ à une page Razor dans ASP.NET Core
author: rick-anderson
description: Montre comment ajouter un nouveau champ à une page Razor avec Entity Framework Core
ms.author: riande
ms.custom: mvc
ms.date: 7/23/2019
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 1b08e1515afe656b95be9fb436caa00cd53ab9ad
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334102"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="d9e84-103">Ajouter un nouveau champ à une page Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d9e84-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="d9e84-104">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d9e84-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="d9e84-105">Dans cette section, Migrations [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First est utilisé pour :</span><span class="sxs-lookup"><span data-stu-id="d9e84-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="d9e84-106">Ajouter un nouveau champ au modèle.</span><span class="sxs-lookup"><span data-stu-id="d9e84-106">Add a new field to the model.</span></span>
* <span data-ttu-id="d9e84-107">Migrer la nouvelle modification du schéma des champs vers la base de données.</span><span class="sxs-lookup"><span data-stu-id="d9e84-107">Migrate the new field schema change to the database.</span></span>

<span data-ttu-id="d9e84-108">Quand vous utilisez EF Code First pour créer automatiquement une base de données, Code First :</span><span class="sxs-lookup"><span data-stu-id="d9e84-108">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="d9e84-109">Ajoute une table `__EFMigrationsHistory` à la base de données pour déterminer si le schéma de la base de données est synchronisé avec les classes de modèle à partir desquelles il a été généré.</span><span class="sxs-lookup"><span data-stu-id="d9e84-109">Adds an `__EFMigrationsHistory` table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="d9e84-110">Si les classes de modèle ne sont pas synchronisées avec la base de données, EF lève une exception.</span><span class="sxs-lookup"><span data-stu-id="d9e84-110">If the model classes aren't in sync with the DB, EF throws an exception.</span></span>

<span data-ttu-id="d9e84-111">La vérification automatique de la synchronisation du schéma et du modèle facilite la détection des problèmes d’incohérence et de code de base de données.</span><span class="sxs-lookup"><span data-stu-id="d9e84-111">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="d9e84-112">Ajout d’une propriété Rating au modèle Movie</span><span class="sxs-lookup"><span data-stu-id="d9e84-112">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="d9e84-113">Ouvrez le fichier *Models/Movie.cs* et ajoutez une propriété `Rating` :</span><span class="sxs-lookup"><span data-stu-id="d9e84-113">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="d9e84-114">Générez l'application.</span><span class="sxs-lookup"><span data-stu-id="d9e84-114">Build the app.</span></span>

<span data-ttu-id="d9e84-115">Modifiez *Pages/Movies/Index.cshtml*et ajoutez un champ `Rating` :</span><span class="sxs-lookup"><span data-stu-id="d9e84-115">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/SnapShots/IndexRating.cshtml?highlight=40-42,62-64)]

<span data-ttu-id="d9e84-116">Mettez à jour les pages suivantes :</span><span class="sxs-lookup"><span data-stu-id="d9e84-116">Update the following pages:</span></span>

* <span data-ttu-id="d9e84-117">Ajoutez le champ `Rating` aux pages Delete et Details.</span><span class="sxs-lookup"><span data-stu-id="d9e84-117">Add the `Rating` field to the Delete and Details pages.</span></span>
* <span data-ttu-id="d9e84-118">Mettez à jour [Create.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml) avec un champ `Rating`.</span><span class="sxs-lookup"><span data-stu-id="d9e84-118">Update [Create.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml) with a `Rating` field.</span></span>
* <span data-ttu-id="d9e84-119">Ajoutez le champ `Rating` à la Page Edit.</span><span class="sxs-lookup"><span data-stu-id="d9e84-119">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="d9e84-120">L’application ne fonctionne pas tant que la base de données n’est pas mise à jour pour inclure le nouveau champ.</span><span class="sxs-lookup"><span data-stu-id="d9e84-120">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="d9e84-121">L’exécution de l’application sans mettre à jour la base de données lève une `SqlException` :</span><span class="sxs-lookup"><span data-stu-id="d9e84-121">Running the app without updating the database throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="d9e84-122">L’exception `SqlException` est due au fait que la classe de modèle Movie mise à jour est différente du schéma de la table Movie de la base de données.</span><span class="sxs-lookup"><span data-stu-id="d9e84-122">The `SqlException` exception is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="d9e84-123">(Il n’existe pas de colonne `Rating` dans la table de base de données.)</span><span class="sxs-lookup"><span data-stu-id="d9e84-123">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="d9e84-124">Plusieurs approches sont possibles pour résoudre l’erreur :</span><span class="sxs-lookup"><span data-stu-id="d9e84-124">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="d9e84-125">Laisser Entity Framework supprimer et recréer automatiquement la base de données avec le nouveau schéma de classes du modèle.</span><span class="sxs-lookup"><span data-stu-id="d9e84-125">Have the Entity Framework automatically drop and re-create the database using the new model class schema.</span></span> <span data-ttu-id="d9e84-126">Cette approche est très utile au début du cycle de développement. Elle permet de faire évoluer rapidement le modèle et le schéma de base de données ensemble.</span><span class="sxs-lookup"><span data-stu-id="d9e84-126">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="d9e84-127">L’inconvénient est que vous perdez les données existantes dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="d9e84-127">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="d9e84-128">N’utilisez pas cette approche sur une base de données de production !</span><span class="sxs-lookup"><span data-stu-id="d9e84-128">Don't use this approach on a production database!</span></span> <span data-ttu-id="d9e84-129">La suppression de la base de données lors des changements de schéma et l’utilisation d’un initialiseur pour amorcer automatiquement la base de données avec des données de test est souvent un moyen efficace pour développer une application.</span><span class="sxs-lookup"><span data-stu-id="d9e84-129">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="d9e84-130">Modifiez explicitement le schéma de la base de données existante pour le faire correspondre aux classes du modèle.</span><span class="sxs-lookup"><span data-stu-id="d9e84-130">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="d9e84-131">L’avantage de cette approche est que vous conservez vos données.</span><span class="sxs-lookup"><span data-stu-id="d9e84-131">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="d9e84-132">Vous pouvez apporter cette modification manuellement ou en créant un script de modification de la base de données.</span><span class="sxs-lookup"><span data-stu-id="d9e84-132">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="d9e84-133">Utilisez Migrations Code First pour mettre à jour le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="d9e84-133">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="d9e84-134">Pour ce didacticiel, nous allons utiliser les migrations Code First.</span><span class="sxs-lookup"><span data-stu-id="d9e84-134">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="d9e84-135">Mettez à jour la classe `SeedData` pour qu’elle fournisse une valeur pour la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="d9e84-135">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="d9e84-136">Vous pouvez voir un exemple de modification ci-dessous, mais elle doit être appliquée à chaque bloc `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="d9e84-136">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="d9e84-137">Consultez le [fichier SeedData.cs complet](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="d9e84-137">See the [completed SeedData.cs file](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="d9e84-138">Générez la solution.</span><span class="sxs-lookup"><span data-stu-id="d9e84-138">Build the solution.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d9e84-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d9e84-139">Visual Studio</span></span>](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a><span data-ttu-id="d9e84-140">Ajouter une migration pour le champ d’évaluation</span><span class="sxs-lookup"><span data-stu-id="d9e84-140">Add a migration for the rating field</span></span>

<span data-ttu-id="d9e84-141">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet > Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="d9e84-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="d9e84-142">Dans la console du gestionnaire de package, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="d9e84-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="d9e84-143">La commande `Add-Migration` indique au framework qu’il doit :</span><span class="sxs-lookup"><span data-stu-id="d9e84-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="d9e84-144">Comparer le modèle `Movie` au schéma de base de données `Movie`</span><span class="sxs-lookup"><span data-stu-id="d9e84-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="d9e84-145">Créer du code pour migrer le schéma de base de données vers le nouveau modèle</span><span class="sxs-lookup"><span data-stu-id="d9e84-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="d9e84-146">Le nom « Rating » est arbitraire et est utilisé pour nommer le fichier de migration.</span><span class="sxs-lookup"><span data-stu-id="d9e84-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="d9e84-147">Il est utile d’utiliser un nom explicite pour le fichier de migration.</span><span class="sxs-lookup"><span data-stu-id="d9e84-147">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="d9e84-148">La commande `Update-Database` indique à l’infrastructure d’appliquer les modifications de schéma à la base de données et de conserver les données existantes.</span><span class="sxs-lookup"><span data-stu-id="d9e84-148">The `Update-Database` command tells the framework to apply the schema changes to the database and to preserve existing data.</span></span>

<a name="ssox"></a>

<span data-ttu-id="d9e84-149">Si vous supprimez tous les enregistrements de la base de données, l’initialiseur amorce la base de données et inclut le champ `Rating`.</span><span class="sxs-lookup"><span data-stu-id="d9e84-149">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="d9e84-150">Pour ce faire, utilisez les liens de suppression disponibles dans le navigateur ou à partir de [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox).</span><span class="sxs-lookup"><span data-stu-id="d9e84-150">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span>

<span data-ttu-id="d9e84-151">Une autre option consiste à supprimer la base de données et à utiliser des migrations pour recréer la base de données.</span><span class="sxs-lookup"><span data-stu-id="d9e84-151">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="d9e84-152">Pour supprimer la base de données dans SSOX :</span><span class="sxs-lookup"><span data-stu-id="d9e84-152">To delete the database in SSOX:</span></span>

* <span data-ttu-id="d9e84-153">Sélectionnez la base de données dans SSOX.</span><span class="sxs-lookup"><span data-stu-id="d9e84-153">Select the database in SSOX.</span></span>
* <span data-ttu-id="d9e84-154">Cliquez avec le bouton droit sur la base de données, puis sélectionnez *Supprimer*.</span><span class="sxs-lookup"><span data-stu-id="d9e84-154">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="d9e84-155">Cochez **Fermer les connexions existantes**.</span><span class="sxs-lookup"><span data-stu-id="d9e84-155">Check **Close existing connections**.</span></span>
* <span data-ttu-id="d9e84-156">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="d9e84-156">Select **OK**.</span></span>
* <span data-ttu-id="d9e84-157">Dans la [console du gestionnaire de package](xref:tutorials/razor-pages/new-field#pmc), mettez à jour la base de données :</span><span class="sxs-lookup"><span data-stu-id="d9e84-157">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="d9e84-158">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="d9e84-158">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="d9e84-159">Supprimer et recréer la base de données</span><span class="sxs-lookup"><span data-stu-id="d9e84-159">Drop and re-create the database</span></span>

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

<span data-ttu-id="d9e84-160">Supprimer le dossier de migration.</span><span class="sxs-lookup"><span data-stu-id="d9e84-160">Delete the migration folder.</span></span>  <span data-ttu-id="d9e84-161">Utilisez les commandes suivantes pour recréer la base de données.</span><span class="sxs-lookup"><span data-stu-id="d9e84-161">Use the following commands to recreate the database.</span></span>

```dotnetcli
dotnet ef database drop
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

<span data-ttu-id="d9e84-162">Exécutez l’application et vérifiez que vous pouvez créer/modifier/afficher des films avec un champ `Rating`.</span><span class="sxs-lookup"><span data-stu-id="d9e84-162">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="d9e84-163">Si la base de données n’est pas amorcée, définissez un point d’arrêt dans la méthode `SeedData.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="d9e84-163">If the database isn't seeded, set a break point in the `SeedData.Initialize` method.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d9e84-164">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d9e84-164">Additional resources</span></span>

* [<span data-ttu-id="d9e84-165">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="d9e84-165">YouTube version of this tutorial</span></span>](https://youtu.be/3i7uMxiGGR8)

> [!div class="step-by-step"]
> <span data-ttu-id="d9e84-166">[Précédent : Ajout de la recherche](xref:tutorials/razor-pages/search)
> [Suivant : Ajout de la validation](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="d9e84-166">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="d9e84-167">Dans cette section, Migrations [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First est utilisé pour :</span><span class="sxs-lookup"><span data-stu-id="d9e84-167">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="d9e84-168">Ajouter un nouveau champ au modèle.</span><span class="sxs-lookup"><span data-stu-id="d9e84-168">Add a new field to the model.</span></span>
* <span data-ttu-id="d9e84-169">Migrer la nouvelle modification du schéma des champs vers la base de données.</span><span class="sxs-lookup"><span data-stu-id="d9e84-169">Migrate the new field schema change to the database.</span></span>

<span data-ttu-id="d9e84-170">Quand vous utilisez EF Code First pour créer automatiquement une base de données, Code First :</span><span class="sxs-lookup"><span data-stu-id="d9e84-170">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="d9e84-171">Ajoute une table à la base de données pour déterminer si le schéma de la base de données est synchronisé avec les classes de modèle à partir desquelles il a été généré.</span><span class="sxs-lookup"><span data-stu-id="d9e84-171">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="d9e84-172">Si les classes de modèle ne sont pas synchronisées avec la base de données, EF lève une exception.</span><span class="sxs-lookup"><span data-stu-id="d9e84-172">If the model classes aren't in sync with the DB, EF throws an exception.</span></span>

<span data-ttu-id="d9e84-173">La vérification automatique de la synchronisation du schéma et du modèle facilite la détection des problèmes d’incohérence et de code de base de données.</span><span class="sxs-lookup"><span data-stu-id="d9e84-173">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="d9e84-174">Ajout d’une propriété Rating au modèle Movie</span><span class="sxs-lookup"><span data-stu-id="d9e84-174">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="d9e84-175">Ouvrez le fichier *Models/Movie.cs* et ajoutez une propriété `Rating` :</span><span class="sxs-lookup"><span data-stu-id="d9e84-175">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="d9e84-176">Générez l'application.</span><span class="sxs-lookup"><span data-stu-id="d9e84-176">Build the app.</span></span>

<span data-ttu-id="d9e84-177">Modifiez *Pages/Movies/Index.cshtml*et ajoutez un champ `Rating` :</span><span class="sxs-lookup"><span data-stu-id="d9e84-177">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexRating.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="d9e84-178">Mettez à jour les pages suivantes :</span><span class="sxs-lookup"><span data-stu-id="d9e84-178">Update the following pages:</span></span>

* <span data-ttu-id="d9e84-179">Ajoutez le champ `Rating` aux pages Delete et Details.</span><span class="sxs-lookup"><span data-stu-id="d9e84-179">Add the `Rating` field to the Delete and Details pages.</span></span>
* <span data-ttu-id="d9e84-180">Mettez à jour [Create.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) avec un champ `Rating`.</span><span class="sxs-lookup"><span data-stu-id="d9e84-180">Update [Create.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) with a `Rating` field.</span></span>
* <span data-ttu-id="d9e84-181">Ajoutez le champ `Rating` à la Page Edit.</span><span class="sxs-lookup"><span data-stu-id="d9e84-181">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="d9e84-182">L’application ne fonctionne pas tant que la base de données n’est pas mise à jour pour inclure le nouveau champ.</span><span class="sxs-lookup"><span data-stu-id="d9e84-182">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="d9e84-183">Si vous l’exécutez à présent, l’application lève une `SqlException` :</span><span class="sxs-lookup"><span data-stu-id="d9e84-183">If run now, the app throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="d9e84-184">Cette erreur est due au fait que la classe du modèle Movie mise à jour est différente du schéma de la table Movie de la base de données.</span><span class="sxs-lookup"><span data-stu-id="d9e84-184">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="d9e84-185">(Il n’existe pas de colonne `Rating` dans la table de base de données.)</span><span class="sxs-lookup"><span data-stu-id="d9e84-185">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="d9e84-186">Plusieurs approches sont possibles pour résoudre l’erreur :</span><span class="sxs-lookup"><span data-stu-id="d9e84-186">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="d9e84-187">Laisser Entity Framework supprimer et recréer automatiquement la base de données avec le nouveau schéma de classes du modèle.</span><span class="sxs-lookup"><span data-stu-id="d9e84-187">Have the Entity Framework automatically drop and re-create the database using the new model class schema.</span></span> <span data-ttu-id="d9e84-188">Cette approche est très utile au début du cycle de développement. Elle permet de faire évoluer rapidement le modèle et le schéma de base de données ensemble.</span><span class="sxs-lookup"><span data-stu-id="d9e84-188">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="d9e84-189">L’inconvénient est que vous perdez les données existantes dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="d9e84-189">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="d9e84-190">N’utilisez pas cette approche sur une base de données de production !</span><span class="sxs-lookup"><span data-stu-id="d9e84-190">Don't use this approach on a production database!</span></span> <span data-ttu-id="d9e84-191">La suppression de la base de données lors des changements de schéma et l’utilisation d’un initialiseur pour amorcer automatiquement la base de données avec des données de test est souvent un moyen efficace pour développer une application.</span><span class="sxs-lookup"><span data-stu-id="d9e84-191">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="d9e84-192">Modifiez explicitement le schéma de la base de données existante pour le faire correspondre aux classes du modèle.</span><span class="sxs-lookup"><span data-stu-id="d9e84-192">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="d9e84-193">L’avantage de cette approche est que vous conservez vos données.</span><span class="sxs-lookup"><span data-stu-id="d9e84-193">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="d9e84-194">Vous pouvez apporter cette modification manuellement ou en créant un script de modification de la base de données.</span><span class="sxs-lookup"><span data-stu-id="d9e84-194">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="d9e84-195">Utilisez Migrations Code First pour mettre à jour le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="d9e84-195">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="d9e84-196">Pour ce didacticiel, nous allons utiliser les migrations Code First.</span><span class="sxs-lookup"><span data-stu-id="d9e84-196">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="d9e84-197">Mettez à jour la classe `SeedData` pour qu’elle fournisse une valeur pour la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="d9e84-197">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="d9e84-198">Vous pouvez voir un exemple de modification ci-dessous, mais elle doit être appliquée à chaque bloc `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="d9e84-198">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="d9e84-199">Consultez le [fichier SeedData.cs complet](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="d9e84-199">See the [completed SeedData.cs file](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="d9e84-200">Générez la solution.</span><span class="sxs-lookup"><span data-stu-id="d9e84-200">Build the solution.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d9e84-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d9e84-201">Visual Studio</span></span>](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a><span data-ttu-id="d9e84-202">Ajouter une migration pour le champ d’évaluation</span><span class="sxs-lookup"><span data-stu-id="d9e84-202">Add a migration for the rating field</span></span>

<span data-ttu-id="d9e84-203">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet > Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="d9e84-203">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="d9e84-204">Dans la console du gestionnaire de package, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="d9e84-204">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="d9e84-205">La commande `Add-Migration` indique au framework qu’il doit :</span><span class="sxs-lookup"><span data-stu-id="d9e84-205">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="d9e84-206">Comparer le modèle `Movie` au schéma de base de données `Movie`</span><span class="sxs-lookup"><span data-stu-id="d9e84-206">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="d9e84-207">Créer du code pour migrer le schéma de base de données vers le nouveau modèle</span><span class="sxs-lookup"><span data-stu-id="d9e84-207">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="d9e84-208">Le nom « Rating » est arbitraire et est utilisé pour nommer le fichier de migration.</span><span class="sxs-lookup"><span data-stu-id="d9e84-208">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="d9e84-209">Il est utile d’utiliser un nom explicite pour le fichier de migration.</span><span class="sxs-lookup"><span data-stu-id="d9e84-209">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="d9e84-210">La commande `Update-Database` demande à l’infrastructure d’appliquer les modifications de schéma à la base de données.</span><span class="sxs-lookup"><span data-stu-id="d9e84-210">The `Update-Database` command tells the framework to apply the schema changes to the database.</span></span>

<a name="ssox"></a>

<span data-ttu-id="d9e84-211">Si vous supprimez tous les enregistrements de la base de données, l’initialiseur amorce la base de données et inclut le champ `Rating`.</span><span class="sxs-lookup"><span data-stu-id="d9e84-211">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="d9e84-212">Pour ce faire, utilisez les liens de suppression disponibles dans le navigateur ou à partir de [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox).</span><span class="sxs-lookup"><span data-stu-id="d9e84-212">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span>

<span data-ttu-id="d9e84-213">Une autre option consiste à supprimer la base de données et à utiliser des migrations pour recréer la base de données.</span><span class="sxs-lookup"><span data-stu-id="d9e84-213">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="d9e84-214">Pour supprimer la base de données dans SSOX :</span><span class="sxs-lookup"><span data-stu-id="d9e84-214">To delete the database in SSOX:</span></span>

* <span data-ttu-id="d9e84-215">Sélectionnez la base de données dans SSOX.</span><span class="sxs-lookup"><span data-stu-id="d9e84-215">Select the database in SSOX.</span></span>
* <span data-ttu-id="d9e84-216">Cliquez avec le bouton droit sur la base de données, puis sélectionnez *Supprimer*.</span><span class="sxs-lookup"><span data-stu-id="d9e84-216">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="d9e84-217">Cochez **Fermer les connexions existantes**.</span><span class="sxs-lookup"><span data-stu-id="d9e84-217">Check **Close existing connections**.</span></span>
* <span data-ttu-id="d9e84-218">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="d9e84-218">Select **OK**.</span></span>
* <span data-ttu-id="d9e84-219">Dans la [console du gestionnaire de package](xref:tutorials/razor-pages/new-field#pmc), mettez à jour la base de données :</span><span class="sxs-lookup"><span data-stu-id="d9e84-219">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="d9e84-220">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="d9e84-220">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="d9e84-221">Supprimer et recréer la base de données</span><span class="sxs-lookup"><span data-stu-id="d9e84-221">Drop and re-create the database</span></span>

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

<span data-ttu-id="d9e84-222">Supprimer la base de données et utiliser des migrations pour recréer la base de données.</span><span class="sxs-lookup"><span data-stu-id="d9e84-222">Delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="d9e84-223">Pour supprimer la base de données, supprimez le fichier de base de données (*MvcMovie.db*).</span><span class="sxs-lookup"><span data-stu-id="d9e84-223">To delete the database, delete the database file (*MvcMovie.db*).</span></span> <span data-ttu-id="d9e84-224">Exécutez ensuite la commande `ef database update` :</span><span class="sxs-lookup"><span data-stu-id="d9e84-224">Then run the `ef database update` command:</span></span>

```dotnetcli
dotnet ef database update
```

---

<span data-ttu-id="d9e84-225">Exécutez l’application et vérifiez que vous pouvez créer/modifier/afficher des films avec un champ `Rating`.</span><span class="sxs-lookup"><span data-stu-id="d9e84-225">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="d9e84-226">Si la base de données n’est pas amorcée, définissez un point d’arrêt dans la méthode `SeedData.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="d9e84-226">If the database isn't seeded, set a break point in the `SeedData.Initialize` method.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d9e84-227">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d9e84-227">Additional resources</span></span>

* [<span data-ttu-id="d9e84-228">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="d9e84-228">YouTube version of this tutorial</span></span>](https://youtu.be/3i7uMxiGGR8)

> [!div class="step-by-step"]
> <span data-ttu-id="d9e84-229">[Précédent : Ajout de la recherche](xref:tutorials/razor-pages/search)
> [Suivant : Ajout de la validation](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="d9e84-229">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>

::: moniker-end
