---
title: Pages Razor avec EF Core dans ASP.NET Core - Migrations - 4 sur 8
author: rick-anderson
description: Dans ce didacticiel, vous allez commencer à utiliser la fonctionnalité de migrations EF Core pour gérer les modifications du modèle de données dans une application ASP.NET Core MVC.
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/migrations
ms.openlocfilehash: 1803c6d3956121e4e7091f4f951917425e87c335
ms.sourcegitcommit: 7d6019f762fc5b8cbedcd69801e8310f51a17c18
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419470"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a><span data-ttu-id="84b2c-103">Pages Razor avec EF Core dans ASP.NET Core - Migrations - 4 sur 8</span><span class="sxs-lookup"><span data-stu-id="84b2c-103">Razor Pages with EF Core in ASP.NET Core - Migrations - 4 of 8</span></span>

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="84b2c-104">Par [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="84b2c-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

<span data-ttu-id="84b2c-105">Dans ce didacticiel, nous allons utiliser la fonctionnalité de migrations EF Core pour gérer les modifications du modèle de données.</span><span class="sxs-lookup"><span data-stu-id="84b2c-105">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="84b2c-106">Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, téléchargez [l’application terminée](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="84b2c-106">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

<span data-ttu-id="84b2c-107">Quand une nouvelle application est développée, le modèle de données change fréquemment.</span><span class="sxs-lookup"><span data-stu-id="84b2c-107">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="84b2c-108">Chaque fois que le modèle change, il est désynchronisé avec la base de données.</span><span class="sxs-lookup"><span data-stu-id="84b2c-108">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="84b2c-109">Ce didacticiel commence par configurer Entity Framework pour créer la base de données si elle n’existe pas.</span><span class="sxs-lookup"><span data-stu-id="84b2c-109">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="84b2c-110">Chaque fois que le modèle de données change :</span><span class="sxs-lookup"><span data-stu-id="84b2c-110">Each time the data model changes:</span></span>

* <span data-ttu-id="84b2c-111">La base de données est supprimée</span><span class="sxs-lookup"><span data-stu-id="84b2c-111">The DB is dropped.</span></span>
* <span data-ttu-id="84b2c-112">EF crée une nouvelle base de données qui correspond au modèle</span><span class="sxs-lookup"><span data-stu-id="84b2c-112">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="84b2c-113">L’application amorce la base de données avec des données de test</span><span class="sxs-lookup"><span data-stu-id="84b2c-113">The app seeds the DB with test data.</span></span>

<span data-ttu-id="84b2c-114">Cette approche pour conserver la synchronisation de la base de données avec le modèle de données fonctionne bien jusqu’à ce que vous déployiez l’application en production.</span><span class="sxs-lookup"><span data-stu-id="84b2c-114">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="84b2c-115">Quand l’application s’exécute en production, elle stocke généralement des données qui doivent être tenues à jour.</span><span class="sxs-lookup"><span data-stu-id="84b2c-115">When the app is running in production, it's usually storing data that needs to be maintained.</span></span> <span data-ttu-id="84b2c-116">L’application ne peut pas commencer avec une base de données de test chaque fois qu’une modification est apportée (par exemple en cas d’ajout d’une nouvelle colonne).</span><span class="sxs-lookup"><span data-stu-id="84b2c-116">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="84b2c-117">La fonctionnalité Migrations d’EF Core résout ce problème en permettant à EF Core de mettre à jour le schéma de base de données au lieu de créer une nouvelle base de données.</span><span class="sxs-lookup"><span data-stu-id="84b2c-117">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="84b2c-118">Plutôt que de supprimer et de recréer la base de données quand le modèle de données change, les migrations mettent à jour le schéma et conservent les données existantes.</span><span class="sxs-lookup"><span data-stu-id="84b2c-118">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="drop-the-database"></a><span data-ttu-id="84b2c-119">Supprimer la base de données</span><span class="sxs-lookup"><span data-stu-id="84b2c-119">Drop the database</span></span>

<span data-ttu-id="84b2c-120">Utilisez **l’Explorateur d’objets SQL Server** (SSOX) ou la commande `database drop` :</span><span class="sxs-lookup"><span data-stu-id="84b2c-120">Use **SQL Server Object Explorer** (SSOX) or the `database drop` command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="84b2c-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="84b2c-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="84b2c-122">Dans la **console du Gestionnaire de package**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="84b2c-122">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
```

<span data-ttu-id="84b2c-123">Exécutez `Get-Help about_EntityFrameworkCore` à partir de la console du Gestionnaire de package pour obtenir des informations d’aide.</span><span class="sxs-lookup"><span data-stu-id="84b2c-123">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="84b2c-124">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="84b2c-124">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="84b2c-125">Ouvrez une fenêtre de commande et accédez au dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="84b2c-125">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="84b2c-126">Le dossier du projet contient le fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="84b2c-126">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="84b2c-127">Entrez ce qui suit dans la fenêtre de commande :</span><span class="sxs-lookup"><span data-stu-id="84b2c-127">Enter the following in the command window:</span></span>

 ```console
 dotnet ef database drop
 ```

---

## <a name="create-an-initial-migration-and-update-the-db"></a><span data-ttu-id="84b2c-128">Créer une migration initiale et mettre à jour la base de données</span><span class="sxs-lookup"><span data-stu-id="84b2c-128">Create an initial migration and update the DB</span></span>

<span data-ttu-id="84b2c-129">Générez le projet et créez la première migration.</span><span class="sxs-lookup"><span data-stu-id="84b2c-129">Build the project and create the first migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="84b2c-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="84b2c-130">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="84b2c-131">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="84b2c-131">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="84b2c-132">Examiner les méthodes Up et Down</span><span class="sxs-lookup"><span data-stu-id="84b2c-132">Examine the Up and Down methods</span></span>

<span data-ttu-id="84b2c-133">La commande EF Core `migrations add` a généré du code pour créer la base de données.</span><span class="sxs-lookup"><span data-stu-id="84b2c-133">The EF Core `migrations add` command  generated code to create the DB.</span></span> <span data-ttu-id="84b2c-134">Ce code de migrations se trouve dans le fichier *Migrations\<horodatage> _InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="84b2c-134">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="84b2c-135">La méthode `Up` de la classe `InitialCreate` crée les tables de base de données qui correspondent aux jeux d’entités du modèle de données.</span><span class="sxs-lookup"><span data-stu-id="84b2c-135">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="84b2c-136">La méthode `Down` les supprime, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="84b2c-136">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu21/Migrations/20180626224812_InitialCreate.cs?range=7-24,77-88)]

<span data-ttu-id="84b2c-137">Les migrations appellent la méthode `Up` pour implémenter les modifications du modèle de données pour une migration.</span><span class="sxs-lookup"><span data-stu-id="84b2c-137">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="84b2c-138">Quand vous entrez une commande pour restaurer la mise à jour, les migrations appellent la méthode `Down`.</span><span class="sxs-lookup"><span data-stu-id="84b2c-138">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="84b2c-139">Le code précédent concerne la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="84b2c-139">The preceding code is for the initial migration.</span></span> <span data-ttu-id="84b2c-140">Ce code a été créé quand la commande `migrations add InitialCreate` a été exécutée.</span><span class="sxs-lookup"><span data-stu-id="84b2c-140">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="84b2c-141">Le paramètre de nom de migration (« InitialCreate » dans l’exemple) est utilisé comme nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="84b2c-141">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="84b2c-142">Le nom de la migration peut être n’importe quel nom de fichier valide.</span><span class="sxs-lookup"><span data-stu-id="84b2c-142">The migration name can be any valid file name.</span></span> <span data-ttu-id="84b2c-143">Nous vous conseillons de choisir un mot ou une expression qui résume ce qui est effectué dans la migration.</span><span class="sxs-lookup"><span data-stu-id="84b2c-143">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="84b2c-144">Par exemple, une migration ajoutant une table de département pourrait se nommer « TableAjoutDépartement ».</span><span class="sxs-lookup"><span data-stu-id="84b2c-144">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="84b2c-145">Si la migration initiale est créée et que la base de données existe :</span><span class="sxs-lookup"><span data-stu-id="84b2c-145">If the initial migration is created and the DB exists:</span></span>

* <span data-ttu-id="84b2c-146">Le code de création de base de données est généré</span><span class="sxs-lookup"><span data-stu-id="84b2c-146">The DB creation code is generated.</span></span>
* <span data-ttu-id="84b2c-147">Le code de création de base de données n’a pas besoin de s’exécuter, car la base de données correspond déjà au modèle de données.</span><span class="sxs-lookup"><span data-stu-id="84b2c-147">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="84b2c-148">Si le code de création de base de données est exécuté, il n’apporte aucune modification, car la base de données correspond déjà au modèle de données.</span><span class="sxs-lookup"><span data-stu-id="84b2c-148">If the DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="84b2c-149">Quand l’application est déployée sur un nouvel environnement, vous devez exécuter le code de création de base de données pour créer la base de données.</span><span class="sxs-lookup"><span data-stu-id="84b2c-149">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="84b2c-150">Comme la base de données a été supprimée et n’existe pas, les migrations créent une autre base de données.</span><span class="sxs-lookup"><span data-stu-id="84b2c-150">Previously the DB was dropped and doesn't exist, so migrations creates the new DB.</span></span>

### <a name="the-data-model-snapshot"></a><span data-ttu-id="84b2c-151">Capture instantanée du modèle de données</span><span class="sxs-lookup"><span data-stu-id="84b2c-151">The data model snapshot</span></span>

<span data-ttu-id="84b2c-152">Les migrations créent un *instantané* du schéma de base de données actuel dans *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="84b2c-152">Migrations create a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="84b2c-153">Quand vous ajoutez une migration, EF détermine ce qui a changé en comparant le modèle de données au fichier de capture instantanée.</span><span class="sxs-lookup"><span data-stu-id="84b2c-153">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="84b2c-154">Pour supprimer une migration, utilisez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="84b2c-154">To delete a migration, use the following command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="84b2c-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="84b2c-155">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="84b2c-156">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="84b2c-156">Remove-Migration</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="84b2c-157">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="84b2c-157">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations remove
```

<span data-ttu-id="84b2c-158">Pour plus d’informations, consultez [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span><span class="sxs-lookup"><span data-stu-id="84b2c-158">For more information, see  [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span></span>

---

<span data-ttu-id="84b2c-159">Pour supprimer les migrations, la commande supprime la migration et garantit que l’instantané est correctement réinitialisé.</span><span class="sxs-lookup"><span data-stu-id="84b2c-159">The remove migrations command deletes the migration and ensures the snapshot is correctly reset.</span></span>

### <a name="remove-ensurecreated-and-test-the-app"></a><span data-ttu-id="84b2c-160">Supprimer EnsureCreated et tester l’application</span><span class="sxs-lookup"><span data-stu-id="84b2c-160">Remove EnsureCreated and test the app</span></span>

<span data-ttu-id="84b2c-161">Dans les phases initiales de développement, nous avons utilisé `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="84b2c-161">For early development, `EnsureCreated` was used.</span></span> <span data-ttu-id="84b2c-162">Dans ce tutoriel, nous utilisons des migrations.</span><span class="sxs-lookup"><span data-stu-id="84b2c-162">In this tutorial, migrations are used.</span></span> <span data-ttu-id="84b2c-163">La commande `EnsureCreated` a les limitations suivantes :</span><span class="sxs-lookup"><span data-stu-id="84b2c-163">`EnsureCreated` has the following limitations:</span></span>

* <span data-ttu-id="84b2c-164">Elle ignore les migrations et crée la base de données et le schéma</span><span class="sxs-lookup"><span data-stu-id="84b2c-164">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="84b2c-165">Elle ne crée pas de table de migrations</span><span class="sxs-lookup"><span data-stu-id="84b2c-165">Doesn't create a migrations table.</span></span>
* <span data-ttu-id="84b2c-166">Elle ne peut *pas* être utilisée avec des migrations</span><span class="sxs-lookup"><span data-stu-id="84b2c-166">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="84b2c-167">Elle est conçue pour effectuer des tests et un prototypage rapide, où la base de données est supprimée et recréée fréquemment.</span><span class="sxs-lookup"><span data-stu-id="84b2c-167">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="84b2c-168">Supprimez `EnsureCreated` :</span><span class="sxs-lookup"><span data-stu-id="84b2c-168">Remove `EnsureCreated`:</span></span>

```csharp
context.Database.EnsureCreated();
```

<span data-ttu-id="84b2c-169">Exécutez l’application et vérifiez que la base de données est amorcée.</span><span class="sxs-lookup"><span data-stu-id="84b2c-169">Run the app and verify the DB is seeded.</span></span>

### <a name="inspect-the-database"></a><span data-ttu-id="84b2c-170">Inspecter la base de données</span><span class="sxs-lookup"><span data-stu-id="84b2c-170">Inspect the database</span></span>

<span data-ttu-id="84b2c-171">Utilisez **l’Explorateur d’objets SQL Server** pour inspecter la base de données.</span><span class="sxs-lookup"><span data-stu-id="84b2c-171">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="84b2c-172">Notez l’ajout d’une table `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="84b2c-172">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="84b2c-173">La table `__EFMigrationsHistory` effectue le suivi des migrations qui ont été appliquées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="84b2c-173">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="84b2c-174">Visualisez les données dans la table `__EFMigrationsHistory` ; elle affiche une ligne pour la première migration.</span><span class="sxs-lookup"><span data-stu-id="84b2c-174">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="84b2c-175">Le dernier journal dans l’exemple de sortie CLI précédent montre l’instruction INSERT qui crée cette ligne.</span><span class="sxs-lookup"><span data-stu-id="84b2c-175">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="84b2c-176">Exécutez l’application et vérifiez que tout fonctionne.</span><span class="sxs-lookup"><span data-stu-id="84b2c-176">Run the app and verify that everything works.</span></span>

## <a name="applying-migrations-in-production"></a><span data-ttu-id="84b2c-177">Application de migrations en production</span><span class="sxs-lookup"><span data-stu-id="84b2c-177">Applying migrations in production</span></span>

<span data-ttu-id="84b2c-178">Nous vous recommandons de faire en sorte que les applications de production n’appellent **pas** [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="84b2c-178">We recommend production apps should **not** call [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="84b2c-179">`Migrate` ne doit pas être appelée à partir d’une application dans la batterie de serveurs,</span><span class="sxs-lookup"><span data-stu-id="84b2c-179">`Migrate` shouldn't be called from an app in server farm.</span></span> <span data-ttu-id="84b2c-180">par exemple si l’application a été déployée dans le cloud avec montée en puissance parallèle (plusieurs instances de l’application sont en cours d’exécution).</span><span class="sxs-lookup"><span data-stu-id="84b2c-180">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="84b2c-181">La migration de base de données doit être effectuée dans le cadre du déploiement et de manière contrôlée.</span><span class="sxs-lookup"><span data-stu-id="84b2c-181">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="84b2c-182">Parmi les approches de migration de base de données de production, citons :</span><span class="sxs-lookup"><span data-stu-id="84b2c-182">Production database migration approaches include:</span></span>

* <span data-ttu-id="84b2c-183">L’utilisation de migrations pour créer des scripts SQL et l’utilisation de scripts SQL dans le déploiement</span><span class="sxs-lookup"><span data-stu-id="84b2c-183">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="84b2c-184">L’exécution de `dotnet ef database update` à partir d’un environnement contrôlé</span><span class="sxs-lookup"><span data-stu-id="84b2c-184">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="84b2c-185">EF Core utilise la table `__MigrationsHistory` pour voir si des migrations doivent s’exécuter.</span><span class="sxs-lookup"><span data-stu-id="84b2c-185">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="84b2c-186">Si la base de données est à jour, aucune migration n’est exécutée.</span><span class="sxs-lookup"><span data-stu-id="84b2c-186">If the DB is up-to-date, no migration is run.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="84b2c-187">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="84b2c-187">Troubleshooting</span></span>

<span data-ttu-id="84b2c-188">Téléchargez [l’application terminée](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="84b2c-188">Download the [completed app](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="84b2c-189">L’application génère l’exception suivante :</span><span class="sxs-lookup"><span data-stu-id="84b2c-189">The app generates the following exception:</span></span>

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="84b2c-190">Solution : Exécutez `dotnet ef database update`.</span><span class="sxs-lookup"><span data-stu-id="84b2c-190">Solution: Run `dotnet ef database update`</span></span>

### <a name="additional-resources"></a><span data-ttu-id="84b2c-191">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="84b2c-191">Additional resources</span></span>

* [<span data-ttu-id="84b2c-192">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="84b2c-192">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=OWSUuMLKTJo)
* <span data-ttu-id="84b2c-193">[CLI .NET Core](/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="84b2c-193">[.NET Core CLI](/ef/core/miscellaneous/cli/dotnet).</span></span>
* [<span data-ttu-id="84b2c-194">Console du Gestionnaire de package (Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="84b2c-194">Package Manager Console (Visual Studio)</span></span>](/ef/core/miscellaneous/cli/powershell)

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="84b2c-195">[Précédent](xref:data/ef-rp/sort-filter-page)
> [Suivant](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="84b2c-195">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
