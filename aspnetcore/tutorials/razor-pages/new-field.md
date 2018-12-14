---
title: Ajouter un nouveau champ à une page Razor dans ASP.NET Core
author: rick-anderson
description: Montre comment ajouter un nouveau champ à une page Razor avec Entity Framework Core
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/5/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: e280bc9553113982a1f1a77eabab32575c905237
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862289"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="d8440-103">Ajouter un nouveau champ à une page Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d8440-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="d8440-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d8440-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="d8440-105">Dans cette section, Migrations [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First est utilisé pour :</span><span class="sxs-lookup"><span data-stu-id="d8440-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="d8440-106">Ajouter un nouveau champ au modèle.</span><span class="sxs-lookup"><span data-stu-id="d8440-106">Add a new field to the model.</span></span>
* <span data-ttu-id="d8440-107">Migrer la nouvelle modification du schéma des champs vers la base de données.</span><span class="sxs-lookup"><span data-stu-id="d8440-107">Migrate the new field schema change to the database.</span></span>

<span data-ttu-id="d8440-108">Quand vous utilisez EF Code First pour créer automatiquement une base de données, Code First :</span><span class="sxs-lookup"><span data-stu-id="d8440-108">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="d8440-109">Ajoute une table à la base de données pour déterminer si le schéma de la base de données est synchronisé avec les classes de modèle à partir desquelles il a été généré.</span><span class="sxs-lookup"><span data-stu-id="d8440-109">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="d8440-110">Si les classes de modèle ne sont pas synchronisées avec la base de données, EF lève une exception.</span><span class="sxs-lookup"><span data-stu-id="d8440-110">If the model classes aren't in sync with the DB, EF throws an exception.</span></span>

<span data-ttu-id="d8440-111">La vérification automatique de la synchronisation du schéma et du modèle facilite la détection des problèmes d’incohérence et de code de base de données.</span><span class="sxs-lookup"><span data-stu-id="d8440-111">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="d8440-112">Ajout d’une propriété Rating au modèle Movie</span><span class="sxs-lookup"><span data-stu-id="d8440-112">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="d8440-113">Ouvrez le fichier *Models/Movie.cs* et ajoutez une propriété `Rating` :</span><span class="sxs-lookup"><span data-stu-id="d8440-113">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="d8440-114">Générez l'application.</span><span class="sxs-lookup"><span data-stu-id="d8440-114">Build the app.</span></span>

<span data-ttu-id="d8440-115">Modifiez *Pages/Movies/Index.cshtml*et ajoutez un champ `Rating` :</span><span class="sxs-lookup"><span data-stu-id="d8440-115">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexRating.cshtml.?highlight=40-42,61-63)]

<span data-ttu-id="d8440-116">Mettez à jour les pages suivantes :</span><span class="sxs-lookup"><span data-stu-id="d8440-116">Update the following pages:</span></span>

* <span data-ttu-id="d8440-117">Ajoutez le champ `Rating` aux pages Delete et Details.</span><span class="sxs-lookup"><span data-stu-id="d8440-117">Add the `Rating` field to the Delete and Details pages.</span></span>
* <span data-ttu-id="d8440-118">Mettez à jour [Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) avec un champ `Rating`.</span><span class="sxs-lookup"><span data-stu-id="d8440-118">Update [Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) with a `Rating` field.</span></span>
* <span data-ttu-id="d8440-119">Ajoutez le champ `Rating` à la Page Edit.</span><span class="sxs-lookup"><span data-stu-id="d8440-119">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="d8440-120">L’application ne fonctionne pas tant que la base de données n’est pas mise à jour pour inclure le nouveau champ.</span><span class="sxs-lookup"><span data-stu-id="d8440-120">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="d8440-121">Si vous l’exécutez à présent, l’application lève une `SqlException` :</span><span class="sxs-lookup"><span data-stu-id="d8440-121">If run now, the app throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="d8440-122">Cette erreur est due au fait que la classe du modèle Movie mise à jour est différente du schéma de la table Movie de la base de données.</span><span class="sxs-lookup"><span data-stu-id="d8440-122">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="d8440-123">(Il n’existe pas de colonne `Rating` dans la table de base de données.)</span><span class="sxs-lookup"><span data-stu-id="d8440-123">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="d8440-124">Plusieurs approches sont possibles pour résoudre l’erreur :</span><span class="sxs-lookup"><span data-stu-id="d8440-124">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="d8440-125">Laisser Entity Framework supprimer et recréer automatiquement la base de données avec le nouveau schéma de classes du modèle.</span><span class="sxs-lookup"><span data-stu-id="d8440-125">Have the Entity Framework automatically drop and re-create the database using the new model class schema.</span></span> <span data-ttu-id="d8440-126">Cette approche est très utile au début du cycle de développement. Elle permet de faire évoluer rapidement le modèle et le schéma de base de données ensemble.</span><span class="sxs-lookup"><span data-stu-id="d8440-126">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="d8440-127">L’inconvénient est que vous perdez les données existantes dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="d8440-127">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="d8440-128">N’utilisez pas cette approche sur une base de données de production !</span><span class="sxs-lookup"><span data-stu-id="d8440-128">Don't use this approach on a production database!</span></span> <span data-ttu-id="d8440-129">La suppression de la base de données lors des changements de schéma et l’utilisation d’un initialiseur pour amorcer automatiquement la base de données avec des données de test est souvent un moyen efficace pour développer une application.</span><span class="sxs-lookup"><span data-stu-id="d8440-129">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="d8440-130">Modifier explicitement le schéma de la base de données existante pour le faire correspondre aux classes du modèle.</span><span class="sxs-lookup"><span data-stu-id="d8440-130">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="d8440-131">L’avantage de cette approche est que vous conservez vos données.</span><span class="sxs-lookup"><span data-stu-id="d8440-131">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="d8440-132">Vous pouvez apporter cette modification manuellement ou en créant un script de modification de la base de données.</span><span class="sxs-lookup"><span data-stu-id="d8440-132">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="d8440-133">Utilisez les migrations Code First pour mettre à jour le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="d8440-133">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="d8440-134">Pour ce didacticiel, nous allons utiliser les migrations Code First.</span><span class="sxs-lookup"><span data-stu-id="d8440-134">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="d8440-135">Mettez à jour la classe `SeedData` pour qu’elle fournisse une valeur pour la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="d8440-135">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="d8440-136">Vous pouvez voir un exemple de modification ci-dessous, mais elle doit être appliquée à chaque bloc `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="d8440-136">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="d8440-137">Consultez le [fichier SeedData.cs complet](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="d8440-137">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="d8440-138">Générez la solution.</span><span class="sxs-lookup"><span data-stu-id="d8440-138">Build the solution.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d8440-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d8440-139">Visual Studio</span></span>](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a><span data-ttu-id="d8440-140">Ajouter une migration pour le champ d’évaluation</span><span class="sxs-lookup"><span data-stu-id="d8440-140">Add a migration for the rating field</span></span>

<span data-ttu-id="d8440-141">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet > Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="d8440-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="d8440-142">Dans la console du Gestionnaire de package, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="d8440-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="d8440-143">La commande `Add-Migration` indique au framework qu’il doit :</span><span class="sxs-lookup"><span data-stu-id="d8440-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="d8440-144">Comparer le modèle `Movie` au schéma de base de données `Movie`</span><span class="sxs-lookup"><span data-stu-id="d8440-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="d8440-145">Créer du code pour migrer le schéma de base de données vers le nouveau modèle</span><span class="sxs-lookup"><span data-stu-id="d8440-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="d8440-146">Le nom « Rating » est arbitraire et utilisé pour nommer le fichier de migration.</span><span class="sxs-lookup"><span data-stu-id="d8440-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="d8440-147">Il est utile d’utiliser un nom explicite pour le fichier de migration.</span><span class="sxs-lookup"><span data-stu-id="d8440-147">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a>

<span data-ttu-id="d8440-148">Si vous supprimez tous les enregistrements de la base de données, l’initialiseur amorce la base de données et inclut le champ `Rating`.</span><span class="sxs-lookup"><span data-stu-id="d8440-148">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="d8440-149">Pour ce faire, utilisez les liens de suppression disponibles dans le navigateur ou à partir de [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox).</span><span class="sxs-lookup"><span data-stu-id="d8440-149">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="d8440-150">Pour supprimer la base de données à partir de SSOX</span><span class="sxs-lookup"><span data-stu-id="d8440-150">To delete the database from SSOX:</span></span>

* <span data-ttu-id="d8440-151">Sélectionnez la base de données dans SSOX.</span><span class="sxs-lookup"><span data-stu-id="d8440-151">Select the database in SSOX.</span></span>
* <span data-ttu-id="d8440-152">Cliquez avec le bouton droit sur la base de données, puis sélectionnez *Supprimer*.</span><span class="sxs-lookup"><span data-stu-id="d8440-152">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="d8440-153">Cochez **Fermer les connexions existantes**.</span><span class="sxs-lookup"><span data-stu-id="d8440-153">Check **Close existing connections**.</span></span>
* <span data-ttu-id="d8440-154">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="d8440-154">Select **OK**.</span></span>
* <span data-ttu-id="d8440-155">Dans la [console du gestionnaire de package](xref:tutorials/razor-pages/new-field#pmc), mettez à jour la base de données :</span><span class="sxs-lookup"><span data-stu-id="d8440-155">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d8440-156">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d8440-156">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d8440-157"><!-- copy/paste this tab to the next. Not worth an include  --> SQLite ne prend pas en charge les migrations.</span><span class="sxs-lookup"><span data-stu-id="d8440-157"><!-- copy/paste this tab to the next. Not worth an include  --> SQLite doesn't support migrations.</span></span>

* <span data-ttu-id="d8440-158">Supprimez la base de données ou changez le nom de la base de données dans le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d8440-158">Delete the database or change the database name in the *appsettings.json* file.</span></span>
* <span data-ttu-id="d8440-159">Supprimez le dossier *Migrations* (et tous les fichiers du dossier).</span><span class="sxs-lookup"><span data-stu-id="d8440-159">Delete the *Migrations* folder (and all the files in the folder).</span></span>

<span data-ttu-id="d8440-160">Exécutez les commandes CLI .NET Core suivantes :</span><span class="sxs-lookup"><span data-stu-id="d8440-160">Run the following .NET Core CLI commands:</span></span>

```console
dotnet ef migrations add Rating
dotnet ef database update
```

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d8440-161">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="d8440-161">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="d8440-162">SQLite ne prend pas en charge les migrations.</span><span class="sxs-lookup"><span data-stu-id="d8440-162">SQLite doesn't support migrations.</span></span>

* <span data-ttu-id="d8440-163">Supprimez la base de données ou changez le nom de la base de données dans le fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d8440-163">Delete the database or change the database name in the *appsettings.json* file.</span></span>
* <span data-ttu-id="d8440-164">Supprimez le dossier *Migrations* (et tous les fichiers du dossier).</span><span class="sxs-lookup"><span data-stu-id="d8440-164">Delete the *Migrations* folder (and all the files in the folder).</span></span>

<span data-ttu-id="d8440-165">Exécutez les commandes CLI .NET Core suivantes :</span><span class="sxs-lookup"><span data-stu-id="d8440-165">Run the following .NET Core CLI commands:</span></span>

```console
dotnet ef migrations add Rating
dotnet ef database update
```

---  
<!-- End of VS tabs -->

<span data-ttu-id="d8440-166">Exécutez l’application et vérifiez que vous pouvez créer/modifier/afficher des films avec un champ `Rating`.</span><span class="sxs-lookup"><span data-stu-id="d8440-166">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="d8440-167">Si la base de données n’est pas amorcée, définissez un point d’arrêt dans la méthode `SeedData.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="d8440-167">If the database isn't seeded, set a break point in the `SeedData.Initialize` method.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d8440-168">[Précédent : Ajout de la recherche](xref:tutorials/razor-pages/search)
> [Suivant : Ajout de la validation](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="d8440-168">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
