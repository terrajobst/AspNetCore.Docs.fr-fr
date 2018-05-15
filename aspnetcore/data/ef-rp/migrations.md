---
title: Pages Razor avec EF Core dans ASP.NET Core - Migrations - 4 sur 8
author: rick-anderson
description: Dans ce didacticiel, vous allez commencer à utiliser la fonctionnalité de migrations EF Core pour gérer les modifications du modèle de données dans une application ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/migrations
ms.openlocfilehash: 690beaabeab098cf9b764730b1bf1bd04bf6b003
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a><span data-ttu-id="9ad22-103">Pages Razor avec EF Core dans ASP.NET Core - Migrations - 4 sur 8</span><span class="sxs-lookup"><span data-stu-id="9ad22-103">Razor Pages with EF Core in ASP.NET Core - Migrations - 4 of 8</span></span>

<span data-ttu-id="9ad22-104">Par [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9ad22-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="9ad22-105">Dans ce didacticiel, nous allons utiliser la fonctionnalité de migrations EF Core pour gérer les modifications du modèle de données.</span><span class="sxs-lookup"><span data-stu-id="9ad22-105">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="9ad22-106">Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, téléchargez [l’application terminée pour cette phase](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="9ad22-106">If you run into problems you can't solve, download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="9ad22-107">Quand une nouvelle application est développée, le modèle de données change fréquemment.</span><span class="sxs-lookup"><span data-stu-id="9ad22-107">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="9ad22-108">Chaque fois que le modèle change, il est désynchronisé avec la base de données.</span><span class="sxs-lookup"><span data-stu-id="9ad22-108">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="9ad22-109">Ce didacticiel commence par configurer Entity Framework pour créer la base de données si elle n’existe pas.</span><span class="sxs-lookup"><span data-stu-id="9ad22-109">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="9ad22-110">Chaque fois que le modèle de données change :</span><span class="sxs-lookup"><span data-stu-id="9ad22-110">Each time the data model changes:</span></span>

* <span data-ttu-id="9ad22-111">La base de données est supprimée</span><span class="sxs-lookup"><span data-stu-id="9ad22-111">The DB is dropped.</span></span>
* <span data-ttu-id="9ad22-112">EF crée une nouvelle base de données qui correspond au modèle</span><span class="sxs-lookup"><span data-stu-id="9ad22-112">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="9ad22-113">L’application amorce la base de données avec des données de test</span><span class="sxs-lookup"><span data-stu-id="9ad22-113">The app seeds the DB with test data.</span></span>

<span data-ttu-id="9ad22-114">Cette approche pour conserver la synchronisation de la base de données avec le modèle de données fonctionne bien jusqu’à ce que vous déployiez l’application en production.</span><span class="sxs-lookup"><span data-stu-id="9ad22-114">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="9ad22-115">Quand l’application s’exécute en production, elle stocke généralement des données qui doivent être tenues à jour.</span><span class="sxs-lookup"><span data-stu-id="9ad22-115">When the app is running in production, it's usually storing data that needs to be maintained.</span></span> <span data-ttu-id="9ad22-116">L’application ne peut pas commencer avec une base de données de test chaque fois qu’une modification est apportée (par exemple en cas d’ajout d’une nouvelle colonne).</span><span class="sxs-lookup"><span data-stu-id="9ad22-116">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="9ad22-117">La fonctionnalité Migrations d’EF Core résout ce problème en permettant à EF Core de mettre à jour le schéma de base de données au lieu de créer une nouvelle base de données.</span><span class="sxs-lookup"><span data-stu-id="9ad22-117">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="9ad22-118">Plutôt que de supprimer et de recréer la base de données quand le modèle de données change, les migrations mettent à jour le schéma et conservent les données existantes.</span><span class="sxs-lookup"><span data-stu-id="9ad22-118">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="9ad22-119">Packages NuGet Entity Framework Core pour les migrations</span><span class="sxs-lookup"><span data-stu-id="9ad22-119">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="9ad22-120">Pour travailler avec des migrations, utilisez la **console du Gestionnaire de package** ou l’interface de ligne de commande (CLI).</span><span class="sxs-lookup"><span data-stu-id="9ad22-120">To work with migrations, use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span> <span data-ttu-id="9ad22-121">Ces didacticiels montrent comment utiliser des commandes CLI.</span><span class="sxs-lookup"><span data-stu-id="9ad22-121">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="9ad22-122">Vous trouverez des informations sur la console du Gestionnaire de package [à la fin de ce didacticiel](#pmc).</span><span class="sxs-lookup"><span data-stu-id="9ad22-122">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="9ad22-123">Les outils EF Core de l’interface CLI sont fournis dans [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="9ad22-123">The EF Core tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="9ad22-124">Pour installer ce package, ajoutez-le à la collection `DotNetCliToolReference` dans le fichier *.csproj*, comme indiqué.</span><span class="sxs-lookup"><span data-stu-id="9ad22-124">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="9ad22-125">**Remarque :** Ce package doit être installé en modifiant le fichier *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="9ad22-125">**Note:** This package must be installed by editing the *.csproj* file.</span></span> <span data-ttu-id="9ad22-126">Vous ne pouvez pas utiliser la commande `install-package` ou l’interface graphique utilisateur du Gestionnaire de package pour installer ce package.</span><span class="sxs-lookup"><span data-stu-id="9ad22-126">The`install-package` command or the package manager GUI cannot be used to install this package.</span></span> <span data-ttu-id="9ad22-127">Modifiez le fichier *.csproj* en cliquant sur le nom du projet dans **l’Explorateur de solutions** et en sélectionnant **Modifier ContosoUniversity.csproj**.</span><span class="sxs-lookup"><span data-stu-id="9ad22-127">Edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

<span data-ttu-id="9ad22-128">Le balisage suivant montre le fichier *.csproj* mis à jour avec les outils CLI EF Core mis en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="9ad22-128">The following markup shows the updated *.csproj* file with the EF Core CLI tools highlighted:</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
<span data-ttu-id="9ad22-129">Les numéros de version dans l’exemple précédent étaient les plus récents lors de la rédaction de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="9ad22-129">The version numbers in the preceding example were current when the tutorial was written.</span></span> <span data-ttu-id="9ad22-130">Utilisez la même version pour les outils CLI EF Core qui se trouvent dans les autres packages.</span><span class="sxs-lookup"><span data-stu-id="9ad22-130">Use the same version for the EF Core CLI tools found in the other packages.</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="9ad22-131">Changer la chaîne de connexion</span><span class="sxs-lookup"><span data-stu-id="9ad22-131">Change the connection string</span></span>

<span data-ttu-id="9ad22-132">Dans le fichier *appsettings.json*, remplacez le nom de la base de données dans la chaîne de connexion par ContosoUniversity2.</span><span class="sxs-lookup"><span data-stu-id="9ad22-132">In the *appsettings.json* file, change the name of the DB in the connection string to ContosoUniversity2.</span></span>

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="9ad22-133">La modification du nom de la base de données dans la chaîne de connexion fait en sorte que la première migration crée une nouvelle base de données.</span><span class="sxs-lookup"><span data-stu-id="9ad22-133">Changing the DB name in the connection string causes the first migration to create a new DB.</span></span> <span data-ttu-id="9ad22-134">Une nouvelle base de données est créée car il n’en existe aucune portant ce nom.</span><span class="sxs-lookup"><span data-stu-id="9ad22-134">A new DB is created because one with that name doesn't exist.</span></span> <span data-ttu-id="9ad22-135">La modification de la chaîne de connexion n’est pas obligatoire pour la prise en main des migrations.</span><span class="sxs-lookup"><span data-stu-id="9ad22-135">Changing the connection string isn't required for getting started with migrations.</span></span>

<span data-ttu-id="9ad22-136">Une alternative au changement de nom de la base de données consiste à supprimer la base de données.</span><span class="sxs-lookup"><span data-stu-id="9ad22-136">An alternative to changing the DB name is deleting the DB.</span></span> <span data-ttu-id="9ad22-137">Utilisez **l’Explorateur d’objets SQL Server** (SSOX) ou la commande CLI `database drop` :</span><span class="sxs-lookup"><span data-stu-id="9ad22-137">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>

 ```console
 dotnet ef database drop
 ```

<span data-ttu-id="9ad22-138">La section suivante explique comment exécuter des commandes CLI.</span><span class="sxs-lookup"><span data-stu-id="9ad22-138">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="9ad22-139">Créer une migration initiale</span><span class="sxs-lookup"><span data-stu-id="9ad22-139">Create an initial migration</span></span>

<span data-ttu-id="9ad22-140">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="9ad22-140">Build the project.</span></span>

<span data-ttu-id="9ad22-141">Ouvrez une fenêtre de commande et accédez au dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="9ad22-141">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="9ad22-142">Le dossier du projet contient le fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="9ad22-142">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="9ad22-143">Entrez ce qui suit dans la fenêtre de commande :</span><span class="sxs-lookup"><span data-stu-id="9ad22-143">Enter the following in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="9ad22-144">La fenêtre de commande affiche des informations semblables à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="9ad22-144">The command window displays information similar to the following:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="9ad22-145">Si la migration échoue avec le message « *Impossible d’accéder au fichier ContosoUniversity.dll, car il est utilisé par un autre processus.* »</span><span class="sxs-lookup"><span data-stu-id="9ad22-145">If the migration fails with the message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*"</span></span> <span data-ttu-id="9ad22-146">s’affiche :</span><span class="sxs-lookup"><span data-stu-id="9ad22-146">is displayed:</span></span>

* <span data-ttu-id="9ad22-147">Arrêtez IIS Express.</span><span class="sxs-lookup"><span data-stu-id="9ad22-147">Stop IIS Express.</span></span>

   * <span data-ttu-id="9ad22-148">Quittez et redémarrez Visual Studio, ou</span><span class="sxs-lookup"><span data-stu-id="9ad22-148">Exit and restart Visual Studio, or</span></span>
   * <span data-ttu-id="9ad22-149">Recherchez l’icône IIS Express dans la barre d’état système de Windows.</span><span class="sxs-lookup"><span data-stu-id="9ad22-149">Find the IIS Express icon in the Windows System Tray.</span></span>
   * <span data-ttu-id="9ad22-150">Cliquez avec le bouton droit sur l’icône IIS Express, puis cliquez sur **ContosoUniversity > Arrêter le site**.</span><span class="sxs-lookup"><span data-stu-id="9ad22-150">Right-click the IIS Express icon, and then click **ContosoUniversity > Stop Site**.</span></span>

<span data-ttu-id="9ad22-151">Si le message d’erreur « Échec de la build. »</span><span class="sxs-lookup"><span data-stu-id="9ad22-151">If the error message "Build failed."</span></span> <span data-ttu-id="9ad22-152">s’affiche, réexécutez la commande.</span><span class="sxs-lookup"><span data-stu-id="9ad22-152">is displayed, run the command again.</span></span> <span data-ttu-id="9ad22-153">Si vous obtenez cette erreur, laissez une note au bas de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="9ad22-153">If you get this error, leave a note at the bottom of this tutorial.</span></span>

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="9ad22-154">Examiner les méthodes Up et Down</span><span class="sxs-lookup"><span data-stu-id="9ad22-154">Examine the Up and Down methods</span></span>

<span data-ttu-id="9ad22-155">La commande EF Core `migrations add` a généré du code à partir duquel créer la base de données.</span><span class="sxs-lookup"><span data-stu-id="9ad22-155">The EF Core command `migrations add` generated code to create the DB from.</span></span> <span data-ttu-id="9ad22-156">Ce code de migrations se trouve dans le fichier *Migrations\<horodatage> _InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="9ad22-156">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="9ad22-157">La méthode `Up` de la classe `InitialCreate` crée les tables de base de données qui correspondent aux jeux d’entités du modèle de données.</span><span class="sxs-lookup"><span data-stu-id="9ad22-157">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="9ad22-158">La méthode `Down` les supprime, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="9ad22-158">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

<span data-ttu-id="9ad22-159">Les migrations appellent la méthode `Up` pour implémenter les modifications du modèle de données pour une migration.</span><span class="sxs-lookup"><span data-stu-id="9ad22-159">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="9ad22-160">Quand vous entrez une commande pour restaurer la mise à jour, les migrations appellent la méthode `Down`.</span><span class="sxs-lookup"><span data-stu-id="9ad22-160">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="9ad22-161">Le code précédent concerne la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="9ad22-161">The preceding code is for the initial migration.</span></span> <span data-ttu-id="9ad22-162">Ce code a été créé quand la commande `migrations add InitialCreate` a été exécutée.</span><span class="sxs-lookup"><span data-stu-id="9ad22-162">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="9ad22-163">Le paramètre de nom de migration (« InitialCreate » dans l’exemple) est utilisé comme nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="9ad22-163">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="9ad22-164">Le nom de la migration peut être n’importe quel nom de fichier valide.</span><span class="sxs-lookup"><span data-stu-id="9ad22-164">The migration name can be any valid file name.</span></span> <span data-ttu-id="9ad22-165">Nous vous conseillons de choisir un mot ou une expression qui résume ce qui est effectué dans la migration.</span><span class="sxs-lookup"><span data-stu-id="9ad22-165">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="9ad22-166">Par exemple, une migration ajoutant une table de département pourrait se nommer « TableAjoutDépartement ».</span><span class="sxs-lookup"><span data-stu-id="9ad22-166">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="9ad22-167">Si la migration initiale est créée et que la base de données existe :</span><span class="sxs-lookup"><span data-stu-id="9ad22-167">If the initial migration is created and the DB exists:</span></span>

* <span data-ttu-id="9ad22-168">Le code de création de base de données est généré</span><span class="sxs-lookup"><span data-stu-id="9ad22-168">The DB creation code is generated.</span></span>
* <span data-ttu-id="9ad22-169">Le code de création de base de données n’a pas besoin de s’exécuter, car la base de données correspond déjà au modèle de données.</span><span class="sxs-lookup"><span data-stu-id="9ad22-169">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="9ad22-170">Si le code de création de base de données est exécuté, il n’apporte aucune modification, car la base de données correspond déjà au modèle de données.</span><span class="sxs-lookup"><span data-stu-id="9ad22-170">If the DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="9ad22-171">Quand l’application est déployée sur un nouvel environnement, vous devez exécuter le code de création de base de données pour créer la base de données.</span><span class="sxs-lookup"><span data-stu-id="9ad22-171">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="9ad22-172">Précédemment, la chaîne de connexion a été changée de façon à utiliser un nouveau nom pour la base de données.</span><span class="sxs-lookup"><span data-stu-id="9ad22-172">Previously the connection string was changed to use a new name for the DB.</span></span> <span data-ttu-id="9ad22-173">Comme la base de données spécifiée n’existe pas, les migrations créent la base de données.</span><span class="sxs-lookup"><span data-stu-id="9ad22-173">The specified DB doesn't exist, so migrations creates the DB.</span></span>

### <a name="the-data-model-snapshot"></a><span data-ttu-id="9ad22-174">Capture instantanée du modèle de données</span><span class="sxs-lookup"><span data-stu-id="9ad22-174">The data model snapshot</span></span>

<span data-ttu-id="9ad22-175">Migrations crée une *capture instantanée* du schéma de base de données actuel dans *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="9ad22-175">Migrations creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="9ad22-176">Quand vous ajoutez une migration, EF détermine ce qui a changé en comparant le modèle de données au fichier de capture instantanée.</span><span class="sxs-lookup"><span data-stu-id="9ad22-176">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="9ad22-177">Lors de la suppression d’une migration, utilisez la commande [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span><span class="sxs-lookup"><span data-stu-id="9ad22-177">When deleting a migration, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span> <span data-ttu-id="9ad22-178">`dotnet ef migrations remove` supprime la migration et garantit que la capture instantanée est correctement réinitialisée.</span><span class="sxs-lookup"><span data-stu-id="9ad22-178">`dotnet ef migrations remove` deletes the migration and ensures the snapshot is correctly reset.</span></span>

<span data-ttu-id="9ad22-179">Pour plus d’informations sur l’utilisation du fichier de capture instantanée, consultez [Migrations EF Core dans les environnements d’équipe](/ef/core/managing-schemas/migrations/teams).</span><span class="sxs-lookup"><span data-stu-id="9ad22-179">See [EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams) for more information about how the snapshot file is used.</span></span>

## <a name="remove-ensurecreated"></a><span data-ttu-id="9ad22-180">Supprimer EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="9ad22-180">Remove EnsureCreated</span></span>

<span data-ttu-id="9ad22-181">Pour les phases initiales de développement, nous avons utilisé la commande `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="9ad22-181">For early development, the `EnsureCreated` command was used.</span></span> <span data-ttu-id="9ad22-182">Dans ce didacticiel, nous utilisons des migrations.</span><span class="sxs-lookup"><span data-stu-id="9ad22-182">In this tutorial, migrations is used.</span></span> <span data-ttu-id="9ad22-183">La commande `EnsureCreated` a les limitations suivantes :</span><span class="sxs-lookup"><span data-stu-id="9ad22-183">`EnsureCreated` has the following limitations:</span></span>

* <span data-ttu-id="9ad22-184">Elle ignore les migrations et crée la base de données et le schéma</span><span class="sxs-lookup"><span data-stu-id="9ad22-184">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="9ad22-185">Elle ne crée pas de table de migrations</span><span class="sxs-lookup"><span data-stu-id="9ad22-185">Doesn't create a migrations table.</span></span>
* <span data-ttu-id="9ad22-186">Elle ne peut *pas* être utilisée avec des migrations</span><span class="sxs-lookup"><span data-stu-id="9ad22-186">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="9ad22-187">Elle est conçue pour effectuer des tests et un prototypage rapide, où la base de données est supprimée et recréée fréquemment.</span><span class="sxs-lookup"><span data-stu-id="9ad22-187">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="9ad22-188">Supprimez la ligne suivante de `DbInitializer` :</span><span class="sxs-lookup"><span data-stu-id="9ad22-188">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a><span data-ttu-id="9ad22-189">Appliquer la migration à la base de données pendant le développement</span><span class="sxs-lookup"><span data-stu-id="9ad22-189">Apply the migration to the DB in development</span></span>

<span data-ttu-id="9ad22-190">Dans la fenêtre de commande, entrez la commande suivante pour créer la base de données et les tables.</span><span class="sxs-lookup"><span data-stu-id="9ad22-190">In the command window, enter the following to create the DB and tables.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="9ad22-191">Remarque : Si la commande `update` retourne l’erreur « Échec de la build. » :</span><span class="sxs-lookup"><span data-stu-id="9ad22-191">Note: If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="9ad22-192">Réexécutez la commande.</span><span class="sxs-lookup"><span data-stu-id="9ad22-192">Run the command again.</span></span>
* <span data-ttu-id="9ad22-193">Si elle échoue à nouveau, quittez Visual Studio, puis exécutez la commande `update`.</span><span class="sxs-lookup"><span data-stu-id="9ad22-193">If it fails again, exit Visual Studio and then run the `update` command.</span></span>
* <span data-ttu-id="9ad22-194">Laissez un message au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="9ad22-194">Leave a message at the bottom of the page.</span></span>

<span data-ttu-id="9ad22-195">La sortie de la commande est similaire à la sortie de la commande `migrations add`.</span><span class="sxs-lookup"><span data-stu-id="9ad22-195">The output from the command is similar to the `migrations add` command output.</span></span> <span data-ttu-id="9ad22-196">Dans la commande précédente, les journaux des commandes SQL qui configurent la base de données sont affichés.</span><span class="sxs-lookup"><span data-stu-id="9ad22-196">In the preceding command, logs for the SQL commands that set up the DB are displayed.</span></span> <span data-ttu-id="9ad22-197">La plupart des journaux sont omis dans l’exemple de sortie suivant :</span><span class="sxs-lookup"><span data-stu-id="9ad22-197">Most of the logs are omitted in the following sample output:</span></span>

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

<span data-ttu-id="9ad22-198">Pour réduire le niveau de détail dans les messages des journaux, changez les niveaux de journalisation dans le fichier *appsettings.Development.json*.</span><span class="sxs-lookup"><span data-stu-id="9ad22-198">To reduce the level of detail in log messages, change the log levels in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="9ad22-199">Pour plus d’informations, consultez [Introduction à la journalisation](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="9ad22-199">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

<span data-ttu-id="9ad22-200">Utilisez **l’Explorateur d’objets SQL Server** pour inspecter la base de données.</span><span class="sxs-lookup"><span data-stu-id="9ad22-200">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="9ad22-201">Notez l’ajout d’une table `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="9ad22-201">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="9ad22-202">La table `__EFMigrationsHistory` effectue le suivi des migrations qui ont été appliquées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="9ad22-202">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="9ad22-203">Visualisez les données dans la table `__EFMigrationsHistory` ; elle affiche une ligne pour la première migration.</span><span class="sxs-lookup"><span data-stu-id="9ad22-203">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="9ad22-204">Le dernier journal dans l’exemple de sortie CLI précédent montre l’instruction INSERT qui crée cette ligne.</span><span class="sxs-lookup"><span data-stu-id="9ad22-204">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="9ad22-205">Exécutez l’application et vérifiez que tout fonctionne.</span><span class="sxs-lookup"><span data-stu-id="9ad22-205">Run the app and verify that everything works.</span></span>

## <a name="applying-migrations-in-production"></a><span data-ttu-id="9ad22-206">Application de migrations en production</span><span class="sxs-lookup"><span data-stu-id="9ad22-206">Applying migrations in production</span></span>

<span data-ttu-id="9ad22-207">Nous vous recommandons de faire en sorte que les applications de production n’appellent **pas** [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="9ad22-207">We recommend production apps should **not** call [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="9ad22-208">`Migrate` ne doit pas être appelée à partir d’une application dans la batterie de serveurs,</span><span class="sxs-lookup"><span data-stu-id="9ad22-208">`Migrate` shouldn't be called from an app in server farm.</span></span> <span data-ttu-id="9ad22-209">par exemple si l’application a été déployée dans le cloud avec montée en puissance parallèle (plusieurs instances de l’application sont en cours d’exécution).</span><span class="sxs-lookup"><span data-stu-id="9ad22-209">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="9ad22-210">La migration de base de données doit être effectuée dans le cadre du déploiement et de manière contrôlée.</span><span class="sxs-lookup"><span data-stu-id="9ad22-210">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="9ad22-211">Parmi les approches de migration de base de données de production, citons :</span><span class="sxs-lookup"><span data-stu-id="9ad22-211">Production database migration approaches include:</span></span>

* <span data-ttu-id="9ad22-212">L’utilisation de migrations pour créer des scripts SQL et l’utilisation de scripts SQL dans le déploiement</span><span class="sxs-lookup"><span data-stu-id="9ad22-212">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="9ad22-213">L’exécution de `dotnet ef database update` à partir d’un environnement contrôlé</span><span class="sxs-lookup"><span data-stu-id="9ad22-213">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="9ad22-214">EF Core utilise la table `__MigrationsHistory` pour voir si des migrations doivent s’exécuter.</span><span class="sxs-lookup"><span data-stu-id="9ad22-214">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="9ad22-215">Si la base de données est à jour, aucune migration n’est exécutée.</span><span class="sxs-lookup"><span data-stu-id="9ad22-215">If the DB is up to date, no migration is run.</span></span>

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="9ad22-216">Interface CLI ou console du Gestionnaire de package</span><span class="sxs-lookup"><span data-stu-id="9ad22-216">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="9ad22-217">Les outils EF Core pour la gestion des migrations sont disponibles à partir :</span><span class="sxs-lookup"><span data-stu-id="9ad22-217">The EF Core tooling for managing migrations is available from:</span></span>

* <span data-ttu-id="9ad22-218">Des commandes CLI .NET Core.</span><span class="sxs-lookup"><span data-stu-id="9ad22-218">.NET Core CLI commands.</span></span>
* <span data-ttu-id="9ad22-219">Des applets de commande PowerShell dans la fenêtre **Console du Gestionnaire de package** de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9ad22-219">The PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span>

<span data-ttu-id="9ad22-220">Ce didacticiel montre comment utiliser l’interface CLI, mais certains développeurs préfèrent utiliser la console du Gestionnaire de package.</span><span class="sxs-lookup"><span data-stu-id="9ad22-220">This tutorial shows how to use the CLI, some developers prefer using the PMC.</span></span>

<span data-ttu-id="9ad22-221">Les commandes EF Core pour la console du Gestionnaire de package se trouvent dans le package [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools).</span><span class="sxs-lookup"><span data-stu-id="9ad22-221">The EF Core commands for the PMC are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="9ad22-222">Ce package est inclus dans le métapackage [Microsoft.AspNetCore.All](xref:fundamentals/metapackage). Vous n’avez donc pas à l’installer.</span><span class="sxs-lookup"><span data-stu-id="9ad22-222">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="9ad22-223">**Important :** Il ne s’agit pas du même package que celui que vous installez pour l’interface CLI en modifiant le fichier *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="9ad22-223">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="9ad22-224">Le nom de celui-ci se termine par `Tools`, contrairement au nom du package CLI qui se termine par `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="9ad22-224">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="9ad22-225">Pour plus d’informations sur les commandes CLI, consultez [Interface CLI .NET Core](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="9ad22-225">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="9ad22-226">Pour plus d’informations sur les commandes de la console du Gestionnaire de package, consultez [Console du Gestionnaire de package (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="9ad22-226">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="9ad22-227">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="9ad22-227">Troubleshooting</span></span>

<span data-ttu-id="9ad22-228">Téléchargez [l’application terminée pour cette phase](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="9ad22-228">Download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="9ad22-229">L’application génère l’exception suivante :</span><span class="sxs-lookup"><span data-stu-id="9ad22-229">The app generates the following exception:</span></span>

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="9ad22-230">Solution : Exécutez `dotnet ef database update`.</span><span class="sxs-lookup"><span data-stu-id="9ad22-230">Solution: Run `dotnet ef database update`</span></span>

<span data-ttu-id="9ad22-231">Si la commande `update` retourne l’erreur « Échec de la build. » :</span><span class="sxs-lookup"><span data-stu-id="9ad22-231">If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="9ad22-232">Réexécutez la commande.</span><span class="sxs-lookup"><span data-stu-id="9ad22-232">Run the command again.</span></span>
* <span data-ttu-id="9ad22-233">Laissez un message au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="9ad22-233">Leave a message at the bottom of the page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9ad22-234">[Précédent](xref:data/ef-rp/sort-filter-page)
> [Suivant](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="9ad22-234">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
