---
title: Pages Razor avec EF Core - Migrations - 4 de 8
author: rick-anderson
description: "Dans ce didacticiel, vous démarrez à l’aide de la fonctionnalité de migrations EF principales pour la gestion des modifications du modèle de données dans une application ASP.NET MVC de base."
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/migrations
ms.openlocfilehash: 7b0a3f73efd1d30b903b3258bea2082792eb6e8c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="migrations---ef-core-with-razor-pages-tutorial-4-of-8"></a><span data-ttu-id="53ac7-103">Migrations - Core EF avec le didacticiel de Pages Razor (4 sur 8)</span><span class="sxs-lookup"><span data-stu-id="53ac7-103">Migrations - EF Core with Razor Pages tutorial (4 of 8)</span></span>

<span data-ttu-id="53ac7-104">Par [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="53ac7-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="53ac7-105">Dans ce didacticiel, la fonctionnalité de migrations EF principales pour la gestion des modifications du modèle de données est utilisée.</span><span class="sxs-lookup"><span data-stu-id="53ac7-105">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="53ac7-106">Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, téléchargez le [application terminée pour cette étape](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="53ac7-106">If you run into problems you can't solve, download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="53ac7-107">Lorsqu’une application est développée, le modèle de données modifications fréquemment.</span><span class="sxs-lookup"><span data-stu-id="53ac7-107">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="53ac7-108">Chaque fois que les modifications apportées au modèle, le modèle est désynchronisé avec la base de données.</span><span class="sxs-lookup"><span data-stu-id="53ac7-108">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="53ac7-109">Ce didacticiel est démarré par la configuration d’Entity Framework pour créer la base de données si elle n’existe pas.</span><span class="sxs-lookup"><span data-stu-id="53ac7-109">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="53ac7-110">Chaque fois que les données de modifications sur le modèle :</span><span class="sxs-lookup"><span data-stu-id="53ac7-110">Each time the data model changes:</span></span>

* <span data-ttu-id="53ac7-111">La base de données est supprimée.</span><span class="sxs-lookup"><span data-stu-id="53ac7-111">The DB is dropped.</span></span>
* <span data-ttu-id="53ac7-112">EF crée un nouveau qui correspond au modèle.</span><span class="sxs-lookup"><span data-stu-id="53ac7-112">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="53ac7-113">L’application est basée sur la base de données avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="53ac7-113">The app seeds the DB with test data.</span></span>

<span data-ttu-id="53ac7-114">Cette approche pour conserver la base de données synchronisée avec le modèle de données fonctionne bien jusqu'à ce que vous déployez l’application en production.</span><span class="sxs-lookup"><span data-stu-id="53ac7-114">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="53ac7-115">Lorsque l’application est en cours d’exécution en production, il est généralement stocker des données qui doivent être géré.</span><span class="sxs-lookup"><span data-stu-id="53ac7-115">When the app is running in production, it's usually storing data that needs to be maintained.</span></span> <span data-ttu-id="53ac7-116">L’application ne peut pas commencer par un test de base de données chaque fois qu’une modification est effectuée (par exemple, l’ajout d’une nouvelle colonne).</span><span class="sxs-lookup"><span data-stu-id="53ac7-116">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="53ac7-117">La fonctionnalité EF Core Migrations résout ce problème en activant Core EF mettre à jour le schéma de base de données au lieu de créer une nouvelle base de données.</span><span class="sxs-lookup"><span data-stu-id="53ac7-117">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="53ac7-118">Au lieu de supprimer et recréer la base de données lors de la modification de modèle de données, les migrations met à jour le schéma et conserve les données existantes.</span><span class="sxs-lookup"><span data-stu-id="53ac7-118">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="53ac7-119">Packages NuGet Entity Framework Core pour les migrations</span><span class="sxs-lookup"><span data-stu-id="53ac7-119">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="53ac7-120">Pour utiliser des migrations, utilisez le **Package Manager Console** (PMC) ou de l’interface de ligne de commande (CLI).</span><span class="sxs-lookup"><span data-stu-id="53ac7-120">To work with migrations, use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span> <span data-ttu-id="53ac7-121">Ces didacticiels montrent comment utiliser les commandes CLI.</span><span class="sxs-lookup"><span data-stu-id="53ac7-121">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="53ac7-122">Plus d’informations sur la CFP est à [la fin de ce didacticiel](#pmc).</span><span class="sxs-lookup"><span data-stu-id="53ac7-122">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="53ac7-123">Les outils EF principaux pour l’interface de ligne de commande (CLI) sont fournis dans [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="53ac7-123">The EF Core tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="53ac7-124">Pour installer ce package, ajoutez-la à la `DotNetCliToolReference` collection dans le *.csproj* de fichiers, comme indiqué.</span><span class="sxs-lookup"><span data-stu-id="53ac7-124">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="53ac7-125">**Remarque :** ce package doit être installé en modifiant le *.csproj* fichier.</span><span class="sxs-lookup"><span data-stu-id="53ac7-125">**Note:** This package must be installed by editing the *.csproj* file.</span></span> <span data-ttu-id="53ac7-126">Le`install-package` commande ou l’interface utilisateur graphique du Gestionnaire de package ne peut pas être utilisé pour installer ce package.</span><span class="sxs-lookup"><span data-stu-id="53ac7-126">The`install-package` command or the package manager GUI cannot be used to install this package.</span></span> <span data-ttu-id="53ac7-127">Modifier la *.csproj* fichier en cliquant sur le nom du projet dans **l’Explorateur de solutions** et en sélectionnant **ContosoUniversity.csproj de modifier**.</span><span class="sxs-lookup"><span data-stu-id="53ac7-127">Edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

<span data-ttu-id="53ac7-128">Le balisage suivant illustre la mise à jour *.csproj* fichier avec les outils EF Core CLI mis en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="53ac7-128">The following markup shows the updated *.csproj* file with the EF Core CLI tools highlighted:</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
<span data-ttu-id="53ac7-129">Les numéros de version dans l’exemple précédent étaient en cours lorsque le didacticiel a été écrit.</span><span class="sxs-lookup"><span data-stu-id="53ac7-129">The version numbers in the preceding example were current when the tutorial was written.</span></span> <span data-ttu-id="53ac7-130">Utilisez la même version pour les outils EF Core CLI trouvés dans les autres packages.</span><span class="sxs-lookup"><span data-stu-id="53ac7-130">Use the same version for the EF Core CLI tools found in the other packages.</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="53ac7-131">Modifier la chaîne de connexion</span><span class="sxs-lookup"><span data-stu-id="53ac7-131">Change the connection string</span></span>

<span data-ttu-id="53ac7-132">Dans le *appsettings.json* , modifiez le nom de la base de données dans la chaîne de connexion à ContosoUniversity2.</span><span class="sxs-lookup"><span data-stu-id="53ac7-132">In the *appsettings.json* file, change the name of the DB in the connection string to ContosoUniversity2.</span></span>

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="53ac7-133">La modification du nom de la base de données dans la chaîne de connexion entraîne la première migration créer une nouvelle base de données.</span><span class="sxs-lookup"><span data-stu-id="53ac7-133">Changing the DB name in the connection string causes the first migration to create a new DB.</span></span> <span data-ttu-id="53ac7-134">Une nouvelle base de données est créée, car il portant ce nom n’existe pas.</span><span class="sxs-lookup"><span data-stu-id="53ac7-134">A new DB is created because one with that name doesn't exist.</span></span> <span data-ttu-id="53ac7-135">Modification de la chaîne de connexion n’est pas requise pour la prise en main des migrations.</span><span class="sxs-lookup"><span data-stu-id="53ac7-135">Changing the connection string isn't required for getting started with migrations.</span></span>

<span data-ttu-id="53ac7-136">Une alternative à la modification du nom de la base de données supprime la base de données.</span><span class="sxs-lookup"><span data-stu-id="53ac7-136">An alternative to changing the DB name is deleting the DB.</span></span> <span data-ttu-id="53ac7-137">Utilisez **l’Explorateur d’objets SQL Server** (SSOX) ou le `database drop` commande CLI :</span><span class="sxs-lookup"><span data-stu-id="53ac7-137">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>

 ```console
 dotnet ef database drop
 ```

<span data-ttu-id="53ac7-138">La section suivante explique comment exécuter des commandes CLI.</span><span class="sxs-lookup"><span data-stu-id="53ac7-138">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="53ac7-139">Créer une migration initiale</span><span class="sxs-lookup"><span data-stu-id="53ac7-139">Create an initial migration</span></span>

<span data-ttu-id="53ac7-140">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="53ac7-140">Build the project.</span></span>

<span data-ttu-id="53ac7-141">Ouvrez une fenêtre de commande et accédez au dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="53ac7-141">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="53ac7-142">Le dossier du projet contient le *Startup.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="53ac7-142">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="53ac7-143">Dans la fenêtre de commande, entrez ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="53ac7-143">Enter the following in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="53ac7-144">La fenêtre de commande affiche des informations similaires à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="53ac7-144">The command window displays information similar to the following:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="53ac7-145">Si la migration échoue avec le message «*accéder au fichier... ContosoUniversity.dll, car il est utilisé par un autre processus.* "</span><span class="sxs-lookup"><span data-stu-id="53ac7-145">If the migration fails with the message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*"</span></span> <span data-ttu-id="53ac7-146">s’affiche :</span><span class="sxs-lookup"><span data-stu-id="53ac7-146">is displayed:</span></span>

* <span data-ttu-id="53ac7-147">Arrêter IIS Express.</span><span class="sxs-lookup"><span data-stu-id="53ac7-147">Stop IIS Express.</span></span>

   * <span data-ttu-id="53ac7-148">Quittez et redémarrez Visual Studio, ou</span><span class="sxs-lookup"><span data-stu-id="53ac7-148">Exit and restart Visual Studio, or</span></span>
   * <span data-ttu-id="53ac7-149">Rechercher l’icône IIS Express dans la barre d’état du système Windows.</span><span class="sxs-lookup"><span data-stu-id="53ac7-149">Find the IIS Express icon in the Windows System Tray.</span></span>
   * <span data-ttu-id="53ac7-150">Cliquez sur l’icône IIS Express, puis cliquez sur **ContosoUniversity > arrêter un Site**.</span><span class="sxs-lookup"><span data-stu-id="53ac7-150">Right-click the IIS Express icon, and then click **ContosoUniversity > Stop Site**.</span></span>

<span data-ttu-id="53ac7-151">Si le message d’erreur « Échec de la génération. »</span><span class="sxs-lookup"><span data-stu-id="53ac7-151">If the error message "Build failed."</span></span> <span data-ttu-id="53ac7-152">s’affiche, exécutez à nouveau la commande.</span><span class="sxs-lookup"><span data-stu-id="53ac7-152">is displayed, run the command again.</span></span> <span data-ttu-id="53ac7-153">Si vous obtenez cette erreur, laissez une note au bas de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="53ac7-153">If you get this error, leave a note at the bottom of this tutorial.</span></span>

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="53ac7-154">Examiner le haut et vers le bas de méthodes</span><span class="sxs-lookup"><span data-stu-id="53ac7-154">Examine the Up and Down methods</span></span>

<span data-ttu-id="53ac7-155">La commande EF Core `migrations add` généré le code pour créer la base de données à partir de.</span><span class="sxs-lookup"><span data-stu-id="53ac7-155">The EF Core command `migrations add` generated code to create the DB from.</span></span> <span data-ttu-id="53ac7-156">Ce code de migrations se trouve dans le *Migrations\<timestamp > _InitialCreate.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="53ac7-156">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="53ac7-157">Le `Up` méthode de la `InitialCreate` classe crée les tables de base de données qui correspondent aux jeux d’entités de modèle données.</span><span class="sxs-lookup"><span data-stu-id="53ac7-157">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="53ac7-158">Le `Down` méthode les supprime, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="53ac7-158">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

<span data-ttu-id="53ac7-159">Appels de migrations le `Up` méthode pour implémenter les modifications de modèle de données pour une migration.</span><span class="sxs-lookup"><span data-stu-id="53ac7-159">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="53ac7-160">Lorsque vous entrez une commande pour restaurer la mise à jour, les appels de migrations le `Down` (méthode).</span><span class="sxs-lookup"><span data-stu-id="53ac7-160">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="53ac7-161">Le code précédent est pour la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="53ac7-161">The preceding code is for the initial migration.</span></span> <span data-ttu-id="53ac7-162">Ce code a été créé lorsque le `migrations add InitialCreate` commande a été exécutée.</span><span class="sxs-lookup"><span data-stu-id="53ac7-162">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="53ac7-163">Le paramètre de nom de la migration (« InitialCreate » dans l’exemple) est utilisé pour le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="53ac7-163">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="53ac7-164">Le nom de la migration peut être n’importe quel nom de fichier valide.</span><span class="sxs-lookup"><span data-stu-id="53ac7-164">The migration name can be any valid file name.</span></span> <span data-ttu-id="53ac7-165">Il est conseillé de choisir un mot ou une expression qui résume ce qui est effectué dans la migration.</span><span class="sxs-lookup"><span data-stu-id="53ac7-165">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="53ac7-166">Par exemple, une migration à l’ajout d’un tableau de service peut être appelée « AddDepartmentTable ».</span><span class="sxs-lookup"><span data-stu-id="53ac7-166">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="53ac7-167">Si la migration initiale est créée et la base de données s’arrête :</span><span class="sxs-lookup"><span data-stu-id="53ac7-167">If the initial migration is created and the DB exits:</span></span>

* <span data-ttu-id="53ac7-168">Le code de création de base de données est généré.</span><span class="sxs-lookup"><span data-stu-id="53ac7-168">The DB creation code is generated.</span></span>
* <span data-ttu-id="53ac7-169">Le code de création de base de données n’a pas besoin de s’exécuter, car la base de données correspond déjà le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="53ac7-169">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="53ac7-170">Si le code de création de la base de données est exécuté, il n’apporte aucune modification car la base de données correspond déjà le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="53ac7-170">If the The DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="53ac7-171">Lorsque l’application est déployée sur un nouvel environnement, vous devez exécuter le code de création de base de données pour créer la base de données.</span><span class="sxs-lookup"><span data-stu-id="53ac7-171">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="53ac7-172">Précédemment, la chaîne de connexion a été modifiée pour utiliser un nouveau nom pour la base de données.</span><span class="sxs-lookup"><span data-stu-id="53ac7-172">Previously the connection string was changed to use a new name for the DB.</span></span> <span data-ttu-id="53ac7-173">La base de données spécifié n’existe pas, donc migrations crée la base de données.</span><span class="sxs-lookup"><span data-stu-id="53ac7-173">The specified DB doesn't exist, so migrations creates the DB.</span></span>

### <a name="examine-the-data-model-snapshot"></a><span data-ttu-id="53ac7-174">Examinez l’instantané de modèle de données</span><span class="sxs-lookup"><span data-stu-id="53ac7-174">Examine the data model snapshot</span></span>

<span data-ttu-id="53ac7-175">Migrations crée un *instantané* du schéma de base de données en cours dans *Migrations/SchoolContextModelSnapshot.cs*:</span><span class="sxs-lookup"><span data-stu-id="53ac7-175">Migrations creates a *snapshot* of the current DB schema in *Migrations/SchoolContextModelSnapshot.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

<span data-ttu-id="53ac7-176">Étant donné que le schéma de base de données actuels est représenté dans le code, EF Core ne doit pas interagir avec la base de données pour créer des migrations.</span><span class="sxs-lookup"><span data-stu-id="53ac7-176">Because the current DB schema is represented in code, EF Core doesn't have to interact with the DB to create migrations.</span></span> <span data-ttu-id="53ac7-177">Lorsque vous ajoutez une migration, EF Core détermine ce qui a changé en comparant le modèle de données pour le fichier d’instantané.</span><span class="sxs-lookup"><span data-stu-id="53ac7-177">When you add a migration, EF Core determines what changed by comparing the data model to the snapshot file.</span></span> <span data-ttu-id="53ac7-178">EF Core interagit avec la base de données uniquement lorsqu’il a mettre à jour la base de données.</span><span class="sxs-lookup"><span data-stu-id="53ac7-178">EF Core interacts with the DB only when it has to update the DB.</span></span>

<span data-ttu-id="53ac7-179">Le fichier d’instantané doit être synchronisé avec les migrations qui l’a créée.</span><span class="sxs-lookup"><span data-stu-id="53ac7-179">The snapshot file must be in sync with the migrations that created it.</span></span> <span data-ttu-id="53ac7-180">Une migration ne peut pas être supprimée en supprimant le fichier nommé  *\<timestamp > _\<migrationname > .cs*.</span><span class="sxs-lookup"><span data-stu-id="53ac7-180">A migration can't be removed by deleting the file named *\<timestamp>_\<migrationname>.cs*.</span></span> <span data-ttu-id="53ac7-181">Si ce fichier est supprimé, les migrations restantes sont synchronisées avec le fichier de capture instantanée de base de données.</span><span class="sxs-lookup"><span data-stu-id="53ac7-181">If that file is deleted, the remaining migrations are out of sync with the DB snapshot file.</span></span> <span data-ttu-id="53ac7-182">Pour supprimer la dernière migration ajoutée, utilisez la [supprimer des migrations d’ef dotnet](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) commande.</span><span class="sxs-lookup"><span data-stu-id="53ac7-182">To delete the last migration added, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span>

## <a name="remove-ensurecreated"></a><span data-ttu-id="53ac7-183">Supprimer EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="53ac7-183">Remove EnsureCreated</span></span>

<span data-ttu-id="53ac7-184">Pour le développement anticipée, le `EnsureCreated` commande a été utilisée.</span><span class="sxs-lookup"><span data-stu-id="53ac7-184">For early development, the `EnsureCreated` command was used.</span></span> <span data-ttu-id="53ac7-185">Dans ce didacticiel, les migrations est utilisé.</span><span class="sxs-lookup"><span data-stu-id="53ac7-185">In this tutorial, migrations is used.</span></span> <span data-ttu-id="53ac7-186">`EnsureCreated`présente les limitations suivantes :</span><span class="sxs-lookup"><span data-stu-id="53ac7-186">`EnsureCreated` has the following limitations:</span></span>

* <span data-ttu-id="53ac7-187">Ignore les migrations et crée la base de données et le schéma.</span><span class="sxs-lookup"><span data-stu-id="53ac7-187">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="53ac7-188">Ne crée pas une table de migration.</span><span class="sxs-lookup"><span data-stu-id="53ac7-188">Doesn't create a migrations table.</span></span>
* <span data-ttu-id="53ac7-189">Peut *pas* être utilisé avec des migrations.</span><span class="sxs-lookup"><span data-stu-id="53ac7-189">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="53ac7-190">Est conçu pour prototypage rapide ou essai où la base de données est supprimée et recréée fréquemment.</span><span class="sxs-lookup"><span data-stu-id="53ac7-190">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="53ac7-191">Supprimez la ligne suivante à partir de `DbInitializer`:</span><span class="sxs-lookup"><span data-stu-id="53ac7-191">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a><span data-ttu-id="53ac7-192">Appliquer la migration vers la base de données en cours de développement</span><span class="sxs-lookup"><span data-stu-id="53ac7-192">Apply the migration to the DB in development</span></span>

<span data-ttu-id="53ac7-193">Dans la fenêtre de commande, entrez la commande suivante pour créer la base de données et des tables.</span><span class="sxs-lookup"><span data-stu-id="53ac7-193">In the command window, enter the following to create the DB and tables.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="53ac7-194">Remarque : Si le `update` commande renvoie l’erreur « Échec de la Build. » :</span><span class="sxs-lookup"><span data-stu-id="53ac7-194">Note: If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="53ac7-195">Réexécutez la commande.</span><span class="sxs-lookup"><span data-stu-id="53ac7-195">Run the command again.</span></span>
* <span data-ttu-id="53ac7-196">Si elle échoue de nouveau, quittez Visual Studio, puis exécutez le `update` commande.</span><span class="sxs-lookup"><span data-stu-id="53ac7-196">If it fails again, exit Visual Studio and then run the `update` command.</span></span>
* <span data-ttu-id="53ac7-197">Laisser un message au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="53ac7-197">Leave a message at the bottom of the page.</span></span>

<span data-ttu-id="53ac7-198">La sortie de la commande est similaire à la `migrations add` sortie de commande.</span><span class="sxs-lookup"><span data-stu-id="53ac7-198">The output from the command is similar to the `migrations add` command output.</span></span> <span data-ttu-id="53ac7-199">Dans la commande précédente, les journaux pour les commandes SQL qui configurer la base de données sont affichés.</span><span class="sxs-lookup"><span data-stu-id="53ac7-199">In the preceding command, logs for the SQL commands that set up the DB are displayed.</span></span> <span data-ttu-id="53ac7-200">La plupart des journaux est omise dans la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="53ac7-200">Most of the logs are omitted in the following sample output:</span></span>

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

<span data-ttu-id="53ac7-201">Pour réduire le niveau de détail dans les messages de journal, peut modifier les niveaux de journal dans le *appsettings. Development.JSON* fichier.</span><span class="sxs-lookup"><span data-stu-id="53ac7-201">To reduce the level of detail in log messages, can change the log levels in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="53ac7-202">Pour plus d’informations, consultez [Introduction à la journalisation](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="53ac7-202">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

<span data-ttu-id="53ac7-203">Utilisez **l’Explorateur d’objets SQL Server** pour inspecter la base de données.</span><span class="sxs-lookup"><span data-stu-id="53ac7-203">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="53ac7-204">Notez l’ajout d’un `__EFMigrationsHistory` table.</span><span class="sxs-lookup"><span data-stu-id="53ac7-204">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="53ac7-205">Le `__EFMigrationsHistory` table effectue le suivi des migrations ont été appliquées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="53ac7-205">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="53ac7-206">Afficher les données dans le `__EFMigrationsHistory` table, il affiche une ligne pour la première migration.</span><span class="sxs-lookup"><span data-stu-id="53ac7-206">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="53ac7-207">Le dernier journal dans l’exemple de sortie CLI précédent montre l’instruction INSERT qui crée cette ligne.</span><span class="sxs-lookup"><span data-stu-id="53ac7-207">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="53ac7-208">Exécutez l’application et vérifier que tout fonctionne.</span><span class="sxs-lookup"><span data-stu-id="53ac7-208">Run the app and verify that everything works.</span></span>

## <a name="appling-migrations-in-production"></a><span data-ttu-id="53ac7-209">Migrations d’application en production</span><span class="sxs-lookup"><span data-stu-id="53ac7-209">Appling migrations in production</span></span>

<span data-ttu-id="53ac7-210">Nous vous recommandons d’applications de production doivent **pas** appeler [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="53ac7-210">We recommend production apps should **not** call [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="53ac7-211">`Migrate`ne doit pas être appelée à partir d’une application dans la batterie de serveurs.</span><span class="sxs-lookup"><span data-stu-id="53ac7-211">`Migrate` shouldn't be called from an app in server farm.</span></span> <span data-ttu-id="53ac7-212">Par exemple, si l’application a été cloud déployé avec montée en puissance parallèle (plusieurs instances de l’application sont en cours d’exécution).</span><span class="sxs-lookup"><span data-stu-id="53ac7-212">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="53ac7-213">Migration de base de données doit être effectuée dans le cadre du déploiement et d’une façon contrôlée.</span><span class="sxs-lookup"><span data-stu-id="53ac7-213">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="53ac7-214">Méthodes de migration de base de données de production sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="53ac7-214">Production database migration approaches include:</span></span>

* <span data-ttu-id="53ac7-215">L’utilisation de migrations pour créer des scripts SQL et à l’aide de scripts SQL dans le déploiement.</span><span class="sxs-lookup"><span data-stu-id="53ac7-215">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="53ac7-216">En cours d’exécution `dotnet ef database update` à partir d’un environnement contrôlé.</span><span class="sxs-lookup"><span data-stu-id="53ac7-216">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="53ac7-217">EF Core utilise le `__MigrationsHistory` table pour voir si les migrations doivent s’exécuter.</span><span class="sxs-lookup"><span data-stu-id="53ac7-217">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="53ac7-218">Si la base de données est à jour, aucune migration n’est exécutée.</span><span class="sxs-lookup"><span data-stu-id="53ac7-218">If the DB is up to date, no migration is run.</span></span>

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="53ac7-219">Par rapport à l’interface de ligne de commande (CLI) Console du Gestionnaire de package (PMC)</span><span class="sxs-lookup"><span data-stu-id="53ac7-219">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="53ac7-220">Le cœur d’EF pour les outils de gestion des migrations est disponible à partir de :</span><span class="sxs-lookup"><span data-stu-id="53ac7-220">The EF Core tooling for managing migrations is available from:</span></span>

* <span data-ttu-id="53ac7-221">Commandes CLI de base .NET.</span><span class="sxs-lookup"><span data-stu-id="53ac7-221">.NET Core CLI commands.</span></span>
* <span data-ttu-id="53ac7-222">Les applets de commande PowerShell dans Visual Studio **Package Manager Console** les fenêtre (PMC).</span><span class="sxs-lookup"><span data-stu-id="53ac7-222">The PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span>

<span data-ttu-id="53ac7-223">Ce didacticiel montre comment utiliser l’interface CLI, certains développeurs préfèrent à l’aide de la PMC.</span><span class="sxs-lookup"><span data-stu-id="53ac7-223">This tutorial shows how to use the CLI, some developers prefer using the PMC.</span></span>

<span data-ttu-id="53ac7-224">Les commandes de base de EF pour PMC se trouvent dans le [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span><span class="sxs-lookup"><span data-stu-id="53ac7-224">The EF Core commands for the PMC are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="53ac7-225">Ce package est inclus dans le [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, sans que vous ayez à installer.</span><span class="sxs-lookup"><span data-stu-id="53ac7-225">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="53ac7-226">**Important :** cela n’est pas le même package que celui que vous installez pour l’interface CLI en modifiant le *.csproj* fichier.</span><span class="sxs-lookup"><span data-stu-id="53ac7-226">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="53ac7-227">Le nom de celle-ci se termine dans `Tools`, contrairement au nom de package CLI qui se termine par `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="53ac7-227">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="53ac7-228">Pour plus d’informations sur les commandes CLI, consultez [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="53ac7-228">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="53ac7-229">Pour plus d’informations sur les commandes PMC, consultez [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="53ac7-229">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="53ac7-230">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="53ac7-230">Troubleshooting</span></span>

<span data-ttu-id="53ac7-231">Téléchargez le [application terminée pour cette étape](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="53ac7-231">Download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="53ac7-232">L’application génère l’exception suivante :</span><span class="sxs-lookup"><span data-stu-id="53ac7-232">The app generates the following exception:</span></span>

```text
`SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="53ac7-233">Solution : exécutez`dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="53ac7-233">Solution: Run `dotnet ef database update`</span></span>

<span data-ttu-id="53ac7-234">Si le `update` commande renvoie l’erreur « Échec de la Build. » :</span><span class="sxs-lookup"><span data-stu-id="53ac7-234">If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="53ac7-235">Réexécutez la commande.</span><span class="sxs-lookup"><span data-stu-id="53ac7-235">Run the command again.</span></span>
* <span data-ttu-id="53ac7-236">Laisser un message au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="53ac7-236">Leave a message at the bottom of the page.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="53ac7-237">[Précédent](xref:data/ef-rp/sort-filter-page)
[Suivant](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="53ac7-237">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
