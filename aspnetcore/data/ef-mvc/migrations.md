---
title: ASP.NET Core MVC avec EF Core - Migrations - 4 sur 10
author: rick-anderson
description: Dans ce didacticiel, vous utilisez la fonctionnalité Migrations d’EF Core pour gérer les modifications du modèle de données dans une application ASP.NET Core MVC.
ms.author: tdykstra
ms.date: 03/15/2018
uid: data/ef-mvc/migrations
ms.openlocfilehash: 556d7d4ad05679ebfce6c909b29610482bb3f350
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011465"
---
# <a name="aspnet-core-mvc-with-ef-core---migrations---4-of-10"></a><span data-ttu-id="aed66-103">ASP.NET Core MVC avec EF Core - Migrations - 4 sur 10</span><span class="sxs-lookup"><span data-stu-id="aed66-103">ASP.NET Core MVC with EF Core - Migrations - 4 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="aed66-104">Par [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="aed66-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="aed66-105">L’exemple d’application web Contoso University montre comment créer des applications web ASP.NET Core MVC avec Entity Framework Core et Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="aed66-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="aed66-106">Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](intro.md).</span><span class="sxs-lookup"><span data-stu-id="aed66-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="aed66-107">Dans ce didacticiel, vous utilisez la fonctionnalité Migrations d’EF Core pour gérer les modifications du modèle de données.</span><span class="sxs-lookup"><span data-stu-id="aed66-107">In this tutorial, you start using the EF Core migrations feature for managing data model changes.</span></span> <span data-ttu-id="aed66-108">Dans les didacticiels suivants, vous allez ajouter d’autres migrations au fil de la modification du modèle de données.</span><span class="sxs-lookup"><span data-stu-id="aed66-108">In later tutorials, you'll add more migrations as you change the data model.</span></span>

## <a name="introduction-to-migrations"></a><span data-ttu-id="aed66-109">Introduction aux migrations</span><span class="sxs-lookup"><span data-stu-id="aed66-109">Introduction to migrations</span></span>

<span data-ttu-id="aed66-110">Quand vous développez une nouvelle application, votre modèle de données change fréquemment et, chaque fois que le modèle change, il n’est plus en synchronisation avec la base de données.</span><span class="sxs-lookup"><span data-stu-id="aed66-110">When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database.</span></span> <span data-ttu-id="aed66-111">Vous avez démarré ce didacticiel en configurant Entity Framework pour créer la base de données si elle n’existait pas.</span><span class="sxs-lookup"><span data-stu-id="aed66-111">You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="aed66-112">Ensuite, chaque fois que vous modifiez le modèle de données, en ajoutant, supprimant ou changeant des classes d’entité ou votre classe DbContext, vous pouvez supprimer la base de données : EF en crée alors une nouvelle qui correspond au modèle et l’alimente avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="aed66-112">Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.</span></span>

<span data-ttu-id="aed66-113">Cette méthode pour conserver la base de données en synchronisation avec le modèle de données fonctionne bien jusqu’au déploiement de l’application en production.</span><span class="sxs-lookup"><span data-stu-id="aed66-113">This method of keeping the database in sync with the data model works well until you deploy the application to production.</span></span> <span data-ttu-id="aed66-114">Quand l’application s’exécute en production, elle stocke généralement les données que vous voulez conserver, et vous ne voulez pas tout perdre chaque fois que vous apportez une modification, comme ajouter une nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="aed66-114">When the application is running in production it's usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column.</span></span> <span data-ttu-id="aed66-115">La fonctionnalité Migrations d’EF Core résout ce problème en permettant à EF de mettre à jour le schéma de base de données au lieu de créer une nouvelle base de données.</span><span class="sxs-lookup"><span data-stu-id="aed66-115">The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating  a new database.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="aed66-116">Packages NuGet Entity Framework Core pour les migrations</span><span class="sxs-lookup"><span data-stu-id="aed66-116">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="aed66-117">Pour effectuer des migrations, vous pouvez utiliser la **console du Gestionnaire de package** ou l’interface de ligne de commande (CLI).</span><span class="sxs-lookup"><span data-stu-id="aed66-117">To work with migrations, you can use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span>  <span data-ttu-id="aed66-118">Ces didacticiels montrent comment utiliser des commandes CLI.</span><span class="sxs-lookup"><span data-stu-id="aed66-118">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="aed66-119">Vous trouverez des informations sur la console du Gestionnaire de package [à la fin de ce didacticiel](#pmc).</span><span class="sxs-lookup"><span data-stu-id="aed66-119">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="aed66-120">Les outils EF de l’interface de ligne de commande (CLI) sont fournis dans [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="aed66-120">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="aed66-121">Pour installer ce package, ajoutez-le à la collection `DotNetCliToolReference` dans le fichier *.csproj*, comme indiqué.</span><span class="sxs-lookup"><span data-stu-id="aed66-121">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="aed66-122">**Remarque :** Vous devez installer ce package en modifiant le fichier *.csproj*. Vous ne pouvez pas utiliser la commande `install-package` ou le GUI (interface graphique utilisateur) du Gestionnaire de package.</span><span class="sxs-lookup"><span data-stu-id="aed66-122">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span> <span data-ttu-id="aed66-123">Vous pouvez modifier le fichier *.csproj* en cliquant sur le nom du projet dans **l’Explorateur de solutions** et en sélectionnant **Modifier ContosoUniversity.csproj**.</span><span class="sxs-lookup"><span data-stu-id="aed66-123">You can edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
<span data-ttu-id="aed66-124">(Les numéros de version de cet exemple étaient les plus récents lors de la rédaction de ce didacticiel.)</span><span class="sxs-lookup"><span data-stu-id="aed66-124">(The version numbers in this example were current when the tutorial was written.)</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="aed66-125">Changer la chaîne de connexion</span><span class="sxs-lookup"><span data-stu-id="aed66-125">Change the connection string</span></span>

<span data-ttu-id="aed66-126">Dans le fichier *appsettings.json*, remplacez le nom de la base de données dans la chaîne de connexion par ContosoUniversity2 ou par un autre nom que vous n’avez pas utilisé sur l’ordinateur que vous utilisez.</span><span class="sxs-lookup"><span data-stu-id="aed66-126">In the *appsettings.json* file, change the name of the database in the connection string to ContosoUniversity2 or some other name that you haven't used on the computer you're using.</span></span>

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="aed66-127">Cette modification configure le projet de façon à ce que la première migration crée une nouvelle base de données.</span><span class="sxs-lookup"><span data-stu-id="aed66-127">This change sets up the project so that the first migration will create a new database.</span></span> <span data-ttu-id="aed66-128">Ce n’est pas obligatoire pour commencer à utiliser les migrations, mais vous verrez plus tard pourquoi c’est judicieux.</span><span class="sxs-lookup"><span data-stu-id="aed66-128">This isn't required to get started with migrations, but you'll see later why it's a good idea.</span></span>

> [!NOTE]
> <span data-ttu-id="aed66-129">Au lieu de changer le nom de la base de données, vous pouvez la supprimer.</span><span class="sxs-lookup"><span data-stu-id="aed66-129">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="aed66-130">Utilisez **l’Explorateur d’objets SQL Server** (SSOX) ou la commande CLI `database drop` :</span><span class="sxs-lookup"><span data-stu-id="aed66-130">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```
> <span data-ttu-id="aed66-131">La section suivante explique comment exécuter des commandes CLI.</span><span class="sxs-lookup"><span data-stu-id="aed66-131">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="aed66-132">Créer une migration initiale</span><span class="sxs-lookup"><span data-stu-id="aed66-132">Create an initial migration</span></span>

<span data-ttu-id="aed66-133">Enregistrez vos modifications et générez le projet.</span><span class="sxs-lookup"><span data-stu-id="aed66-133">Save your changes and build the project.</span></span> <span data-ttu-id="aed66-134">Ouvrez ensuite une fenêtre Commande et accédez au dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="aed66-134">Then open a command window and navigate to the project folder.</span></span> <span data-ttu-id="aed66-135">Voici un moyen rapide pour le faire :</span><span class="sxs-lookup"><span data-stu-id="aed66-135">Here's a quick way to do that:</span></span>

* <span data-ttu-id="aed66-136">Dans **l’Explorateur de solutions**, cliquez sur le projet et choisissez **Ouvrir dans l’Explorateur de fichiers** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="aed66-136">In **Solution Explorer**, right-click the project and choose **Open in File Explorer** from the context menu.</span></span>

  ![Élément de menu Ouvrir dans l’Explorateur de fichiers](migrations/_static/open-in-file-explorer.png)

* <span data-ttu-id="aed66-138">Entrez « cmd » dans la barre d’adresses et appuyez sur Entrée.</span><span class="sxs-lookup"><span data-stu-id="aed66-138">Enter "cmd" in the address bar and press Enter.</span></span>

  ![Ouvrir une fenêtre Commande](migrations/_static/open-command-window.png)

<span data-ttu-id="aed66-140">Entrez la commande suivante dans la fenêtre Commande :</span><span class="sxs-lookup"><span data-stu-id="aed66-140">Enter the following command in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="aed66-141">Vous voyez une sortie similaire à celle-ci dans la fenêtre Commande :</span><span class="sxs-lookup"><span data-stu-id="aed66-141">You see output like the following in the command window:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> <span data-ttu-id="aed66-142">Si vous voyez un message d’erreur *Aucun exécutable trouvé correspondant à la commande « dotnet-ef »*, consultez [ce billet de blog](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) pour résoudre ce problème.</span><span class="sxs-lookup"><span data-stu-id="aed66-142">If you see an error message *No executable found matching command "dotnet-ef"*, see [this blog post](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) for help troubleshooting.</span></span>

<span data-ttu-id="aed66-143">Si vous voyez un message d’erreur « *Impossible d’accéder au fichier... ContosoUniversity.dll, car il est utilisé par un autre processus.* », recherchez l’icône IIS Express dans la barre d’état système de Windows, cliquez avec le bouton droit, puis cliquez sur **ContosoUniversity > Arrêter le Site**.</span><span class="sxs-lookup"><span data-stu-id="aed66-143">If you see an error message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*", find the IIS Express icon in the Windows System Tray, and right-click it, then click **ContosoUniversity > Stop Site**.</span></span>

## <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="aed66-144">Examiner les méthodes Up et Down</span><span class="sxs-lookup"><span data-stu-id="aed66-144">Examine the Up and Down methods</span></span>

<span data-ttu-id="aed66-145">Quand vous avez exécuté la commande `migrations add`, EF a généré le code qui crée la base de données à partir de zéro.</span><span class="sxs-lookup"><span data-stu-id="aed66-145">When you executed the `migrations add` command, EF generated the code that will create the database from scratch.</span></span> <span data-ttu-id="aed66-146">Ce code se trouve dans le dossier *Migrations*, dans le fichier nommé *\<horodatage>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="aed66-146">This code is in the *Migrations* folder, in the file named *\<timestamp>_InitialCreate.cs*.</span></span> <span data-ttu-id="aed66-147">La méthode `Up` de la classe `InitialCreate` crée les tables de base de données qui correspondent aux jeux d’entités du modèle de données, et la méthode `Down` les supprime, comme indiqué dans l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="aed66-147">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them, as shown in the following example.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

<span data-ttu-id="aed66-148">La fonctionnalité Migrations appelle la méthode `Up` pour implémenter les modifications du modèle de données pour une migration.</span><span class="sxs-lookup"><span data-stu-id="aed66-148">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="aed66-149">Quand vous entrez une commande pour annuler la mise à jour, Migrations appelle la méthode `Down`.</span><span class="sxs-lookup"><span data-stu-id="aed66-149">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span>

<span data-ttu-id="aed66-150">Ce code est celui de la migration initiale qui a été créé quand vous avez entré la commande `migrations add InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="aed66-150">This code is for the initial migration that was created when you entered the `migrations add InitialCreate` command.</span></span> <span data-ttu-id="aed66-151">Le paramètre de nom de la migration (« InitialCreate » dans l’exemple) est utilisé comme nom de fichier ; vous pouvez le choisir librement.</span><span class="sxs-lookup"><span data-stu-id="aed66-151">The migration name parameter ("InitialCreate" in the example) is used for the file name and can be whatever you want.</span></span> <span data-ttu-id="aed66-152">Nous vous conseillons néanmoins de choisir un mot ou une expression qui résume ce qui est effectué dans la migration.</span><span class="sxs-lookup"><span data-stu-id="aed66-152">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="aed66-153">Par exemple, vous pouvez nommer une migration ultérieure « AjouterTableDépartement ».</span><span class="sxs-lookup"><span data-stu-id="aed66-153">For example, you might name a later migration "AddDepartmentTable".</span></span>

<span data-ttu-id="aed66-154">Si vous avez créé la migration initiale alors que la base de données existait déjà, le code de création de la base de données est généré, mais il n’est pas nécessaire de l’exécuter, car la base de données correspond déjà au modèle de données.</span><span class="sxs-lookup"><span data-stu-id="aed66-154">If you created the initial migration when the database already exists, the database creation code is generated but it doesn't have to run because the database already matches the data model.</span></span> <span data-ttu-id="aed66-155">Quand vous déployez l’application sur un autre environnement où la base de données n’existe pas encore, ce code est exécuté pour créer votre base de données : il est donc judicieux de le tester au préalable.</span><span class="sxs-lookup"><span data-stu-id="aed66-155">When you deploy the app to another environment where the database doesn't exist yet, this code will run to create your database, so it's a good idea to test it first.</span></span> <span data-ttu-id="aed66-156">C’est la raison pour laquelle vous avez précédemment changé le nom de la base de données dans la chaîne de connexion : les migrations doivent pouvoir créer une base de données à partir de zéro.</span><span class="sxs-lookup"><span data-stu-id="aed66-156">That's why you changed the name of the database in the connection string earlier -- so that migrations can create a new one from scratch.</span></span>

## <a name="the-data-model-snapshot"></a><span data-ttu-id="aed66-157">Capture instantanée du modèle de données</span><span class="sxs-lookup"><span data-stu-id="aed66-157">The data model snapshot</span></span>

<span data-ttu-id="aed66-158">Migrations crée une *capture instantanée* du schéma de base de données actuel dans *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="aed66-158">Migrations creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="aed66-159">Quand vous ajoutez une migration, EF détermine ce qui a changé en comparant le modèle de données au fichier de capture instantanée.</span><span class="sxs-lookup"><span data-stu-id="aed66-159">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="aed66-160">Lors de la suppression d’une migration, utilisez la commande [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span><span class="sxs-lookup"><span data-stu-id="aed66-160">When deleting a migration, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span> <span data-ttu-id="aed66-161">`dotnet ef migrations remove` supprime la migration et garantit que la capture instantanée est correctement réinitialisée.</span><span class="sxs-lookup"><span data-stu-id="aed66-161">`dotnet ef migrations remove` deletes the migration and ensures the snapshot is correctly reset.</span></span>

<span data-ttu-id="aed66-162">Pour plus d’informations sur l’utilisation du fichier de capture instantanée, consultez [Migrations EF Core dans les environnements d’équipe](/ef/core/managing-schemas/migrations/teams).</span><span class="sxs-lookup"><span data-stu-id="aed66-162">See [EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams) for more information about how the snapshot file is used.</span></span>

## <a name="apply-the-migration-to-the-database"></a><span data-ttu-id="aed66-163">Appliquer la migration à la base de données</span><span class="sxs-lookup"><span data-stu-id="aed66-163">Apply the migration to the database</span></span>

<span data-ttu-id="aed66-164">Dans la fenêtre Commande, entrez la commande suivante pour créer la base de données et ses tables.</span><span class="sxs-lookup"><span data-stu-id="aed66-164">In the command window, enter the following command to create the database and tables in it.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="aed66-165">La sortie de la commande est similaire à la commande `migrations add`, à ceci près que vous voyez des journaux pour les commandes SQL qui configurent la base de données.</span><span class="sxs-lookup"><span data-stu-id="aed66-165">The output from the command is similar to the `migrations add` command, except that you see logs for the SQL commands that set up the database.</span></span> <span data-ttu-id="aed66-166">La plupart des journaux sont omis dans l’exemple de sortie suivant.</span><span class="sxs-lookup"><span data-stu-id="aed66-166">Most of the logs are omitted in the following sample output.</span></span> <span data-ttu-id="aed66-167">Si vous préférez ne pas voir ce niveau de détail dans les messages des journaux, vous pouvez changer le niveau de journalisation dans le fichier *appsettings.Development.json*.</span><span class="sxs-lookup"><span data-stu-id="aed66-167">If you prefer not to see this level of detail in log messages, you can change the log level in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="aed66-168">Pour plus d’informations, consultez [Introduction à la journalisation](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="aed66-168">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

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

<span data-ttu-id="aed66-169">Utilisez **l’Explorateur d’objets SQL Server** pour inspecter la base de données, comme vous l’avez fait dans le premier didacticiel.</span><span class="sxs-lookup"><span data-stu-id="aed66-169">Use **SQL Server Object Explorer** to inspect the database as you did in the first tutorial.</span></span>  <span data-ttu-id="aed66-170">Vous pouvez noter l’ajout d’une table __EFMigrationsHistory, qui fait le suivi des migrations qui ont été appliquées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="aed66-170">You'll notice the addition of an __EFMigrationsHistory table that keeps track of which migrations have been applied to the database.</span></span> <span data-ttu-id="aed66-171">Visualisez les données de cette table : vous y voyez une ligne pour la première migration.</span><span class="sxs-lookup"><span data-stu-id="aed66-171">View the data in that table and you'll see one row for the first migration.</span></span> <span data-ttu-id="aed66-172">(Le dernier journal dans l’exemple de sortie CLI précédent montre l’instruction INSERT qui crée cette ligne.)</span><span class="sxs-lookup"><span data-stu-id="aed66-172">(The last log in the preceding CLI output example shows the INSERT statement that creates this row.)</span></span>

<span data-ttu-id="aed66-173">Exécutez l’application pour vérifier que tout fonctionne toujours comme avant.</span><span class="sxs-lookup"><span data-stu-id="aed66-173">Run the application to verify that everything still works the same as before.</span></span>

![Page Index des étudiants](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="aed66-175">Interface CLI ou console du Gestionnaire de package</span><span class="sxs-lookup"><span data-stu-id="aed66-175">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="aed66-176">Les outils EF pour la gestion des migrations sont disponibles à partir de commandes CLI .NET Core ou d’applets de commande PowerShell dans la fenêtre **Console du Gestionnaire de package** de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="aed66-176">The EF tooling for managing migrations is available from .NET Core CLI commands or from PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="aed66-177">Ce didacticiel montre comment utiliser l’interface CLI, mais vous pouvez utiliser la console du Gestionnaire de package si vous préférez.</span><span class="sxs-lookup"><span data-stu-id="aed66-177">This tutorial shows how to use the CLI, but you can use the PMC if you prefer.</span></span>

<span data-ttu-id="aed66-178">Les commandes EF pour la console du Gestionnaire de package se trouvent dans le package [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools).</span><span class="sxs-lookup"><span data-stu-id="aed66-178">The EF commands for the PMC commands are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="aed66-179">Ce package est déjà inclus dans le métapackage [Microsoft.AspNetCore.All](xref:fundamentals/metapackage). Vous n’avez donc pas à l’installer.</span><span class="sxs-lookup"><span data-stu-id="aed66-179">This package is already included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="aed66-180">**Important :** Il ne s’agit pas du même package que celui que vous installez pour l’interface CLI en modifiant le fichier *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="aed66-180">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="aed66-181">Le nom de celui-ci se termine par `Tools`, contrairement au nom du package CLI qui se termine par `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="aed66-181">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="aed66-182">Pour plus d’informations sur les commandes CLI, consultez [Interface CLI .NET Core](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="aed66-182">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="aed66-183">Pour plus d’informations sur les commandes de la console du Gestionnaire de package, consultez [Console du Gestionnaire de package (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="aed66-183">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="summary"></a><span data-ttu-id="aed66-184">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="aed66-184">Summary</span></span>

<span data-ttu-id="aed66-185">Dans ce didacticiel, vous avez vu comment créer et appliquer votre première migration.</span><span class="sxs-lookup"><span data-stu-id="aed66-185">In this tutorial, you've seen how to create and apply your first migration.</span></span> <span data-ttu-id="aed66-186">Dans le didacticiel suivant, vous allez aborder des sujets plus avancés en développant le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="aed66-186">In the next tutorial, you'll begin looking at more advanced topics by expanding the data model.</span></span> <span data-ttu-id="aed66-187">Au cour de ce processus, vous allez créer et appliquer d’autres migrations.</span><span class="sxs-lookup"><span data-stu-id="aed66-187">Along the way you'll create and apply additional migrations.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="aed66-188">[Précédent](sort-filter-page.md)
> [Suivant](complex-data-model.md)</span><span class="sxs-lookup"><span data-stu-id="aed66-188">[Previous](sort-filter-page.md)
[Next](complex-data-model.md)</span></span>
