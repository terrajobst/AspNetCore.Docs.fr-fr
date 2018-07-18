---
title: Ajouter un modèle à une application de pages Razor dans ASP.NET Core
author: rick-anderson
description: Découvrez comment ajouter des classes pour gérer des films dans une base de données à l’aide d’Entity Framework Core (EF Core).
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 9e0fd0ee98c84bb2336749640b2a3c103e4639b6
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938324"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="a605c-103">Ajouter un modèle à une application de pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a605c-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="a605c-104">Ajouter un modèle de données</span><span class="sxs-lookup"><span data-stu-id="a605c-104">Add a data model</span></span>

<span data-ttu-id="a605c-105">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **RazorPagesMovie** > **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="a605c-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="a605c-106">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="a605c-106">Name the folder *Models*.</span></span>

<span data-ttu-id="a605c-107">Cliquez avec le bouton droit sur le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="a605c-107">Right click the *Models* folder.</span></span> <span data-ttu-id="a605c-108">Sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="a605c-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="a605c-109">Nommez la classe **Movie**, puis ajoutez les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="a605c-109">Name the class **Movie** and add the following properties:</span></span>

<span data-ttu-id="a605c-110">Remplacez le contenu de la classe `Movie` par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a605c-110">Replace the contents of the `Movie` class with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="a605c-111">Générer automatiquement le modèle de film</span><span class="sxs-lookup"><span data-stu-id="a605c-111">Scaffold the movie model</span></span>

<span data-ttu-id="a605c-112">Dans cette section, le modèle de film est généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="a605c-112">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="a605c-113">Autrement dit, l’outil de génération de modèles automatique génère des pages pour les opérations de création, de lecture, de mise à jour et de suppression (CRUD) pour le modèle de film.</span><span class="sxs-lookup"><span data-stu-id="a605c-113">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="a605c-114">Créer un dossier *Pages/Movies* :</span><span class="sxs-lookup"><span data-stu-id="a605c-114">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="a605c-115">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *Pages*, puis choisissez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="a605c-115">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="a605c-116">Nommez le dossier *Movies*.</span><span class="sxs-lookup"><span data-stu-id="a605c-116">Name the folder *Movies*</span></span>

<span data-ttu-id="a605c-117">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *Pages/Movies*, puis choisissez **Ajouter** > **Nouvel élément généré automatiquement**.</span><span class="sxs-lookup"><span data-stu-id="a605c-117">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/sca.png)

<span data-ttu-id="a605c-119">Dans la boîte de dialogue **Ajouter un modèle automatique**, sélectionnez **Pages Razor avec Entity Framework (CRUD)** > **AJOUTER**.</span><span class="sxs-lookup"><span data-stu-id="a605c-119">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/add_scaffold.png)

<span data-ttu-id="a605c-121">Renseignez la boîte de dialogue **Pages Razor avec Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="a605c-121">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="a605c-122">Dans la liste déroulante **Classe de modèle**, sélectionnez **Film (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="a605c-122">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="a605c-123">Dans la ligne **Classe du contexte de données**, sélectionnez le signe (plus) **+** et acceptez le nom généré **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="a605c-123">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="a605c-124">Dans la liste déroulante **Classe du contexte de données**, sélectionnez **RazorPagesMovie.Models.RazorPagesMovieContext**</span><span class="sxs-lookup"><span data-stu-id="a605c-124">In the **Data context class** drop down, select **RazorPagesMovie.Models.RazorPagesMovieContext**</span></span>
* <span data-ttu-id="a605c-125">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="a605c-125">Select **Add**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/arp.png)

<span data-ttu-id="a605c-127">Le processus de génération de modèles automatique a créé et changé les fichiers suivants :</span><span class="sxs-lookup"><span data-stu-id="a605c-127">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="a605c-128">Fichiers créés</span><span class="sxs-lookup"><span data-stu-id="a605c-128">Files created</span></span>

* <span data-ttu-id="a605c-129">*Pages/Movies* Create, Delete, Details, Edit, Index.</span><span class="sxs-lookup"><span data-stu-id="a605c-129">*Pages/Movies* Create, Delete, Details, Edit, Index.</span></span> <span data-ttu-id="a605c-130">Ces pages sont détaillées dans le tutoriel suivant.</span><span class="sxs-lookup"><span data-stu-id="a605c-130">These pages are detailed in the next tutorial.</span></span>
* <span data-ttu-id="a605c-131">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="a605c-131">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="files-updates"></a><span data-ttu-id="a605c-132">Mises à jour de fichiers</span><span class="sxs-lookup"><span data-stu-id="a605c-132">Files updates</span></span>

* <span data-ttu-id="a605c-133">*Startup.cs* : Les changements apportés à ce fichier sont détaillés dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="a605c-133">*Startup.cs*: Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="a605c-134">*appSettings.JSON* : La chaîne de connexion utilisée pour se connecter à une base de données locale est ajoutée.</span><span class="sxs-lookup"><span data-stu-id="a605c-134">*appsettings.json*: The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="a605c-135">Examiner le contexte inscrit avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="a605c-135">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="a605c-136">ASP.NET Core comprend [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a605c-136">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a605c-137">Des services (tels que le contexte de base de données EF Core) sont inscrits avec l’injection de dépendances au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="a605c-137">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="a605c-138">Ces services sont affectés aux composants qui les nécessitent (par exemple les Pages Razor) par le biais de paramètres de constructeur.</span><span class="sxs-lookup"><span data-stu-id="a605c-138">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="a605c-139">Le code du constructeur qui obtient une instance de contexte de base de données est indiqué plus loin dans le tutoriel.</span><span class="sxs-lookup"><span data-stu-id="a605c-139">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="a605c-140">L’outil de génération de modèles automatique a créé automatiquement un contexte de base de données et l’a inscrit dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="a605c-140">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="a605c-141">Examinez la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a605c-141">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="a605c-142">La ligne en surbrillance a été ajoutée par l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="a605c-142">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="a605c-143">La classe principale qui coordonne les fonctionnalités d’EF Core pour un modèle de données spécifié est la classe de contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="a605c-143">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="a605c-144">Le contexte de données est dérivé de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="a605c-144">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="a605c-145">Il spécifie les entités qui sont incluses dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="a605c-145">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="a605c-146">Dans ce projet, la classe est nommée `RazorPagesMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="a605c-146">In this project, the class is named `RazorPagesMovieContext`.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="a605c-147">Le code précédent crée une propriété [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) pour le jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="a605c-147">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="a605c-148">Dans la terminologie Entity Framework, un jeu d’entités correspond généralement à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="a605c-148">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="a605c-149">Une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="a605c-149">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="a605c-150">Le nom de la chaîne de connexion est transmis au contexte en appelant une méthode sur un objet [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="a605c-150">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="a605c-151">Pour le développement local, le [système de configuration ASP.NET Core](xref:fundamentals/configuration/index) lit la chaîne de connexion à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a605c-151">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="a605c-152">Effectuer la migration initiale</span><span class="sxs-lookup"><span data-stu-id="a605c-152">Perform initial migration</span></span>

<span data-ttu-id="a605c-153">Dans cette section, effectuez les tâches suivantes à l’aide de la console du gestionnaire de package :</span><span class="sxs-lookup"><span data-stu-id="a605c-153">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="a605c-154">Ajouter une migration initiale</span><span class="sxs-lookup"><span data-stu-id="a605c-154">Add an initial migration.</span></span>
* <span data-ttu-id="a605c-155">Mettez à jour la base de données avec la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="a605c-155">Update the database with the initial migration.</span></span>

<span data-ttu-id="a605c-156">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** > **Console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="a605c-156">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu Console du Gestionnaire de package](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="a605c-158">Dans la console du Gestionnaire de package, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="a605c-158">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="a605c-159">Vous pouvez aussi utiliser les commandes .NET Core CLI suivantes :</span><span class="sxs-lookup"><span data-stu-id="a605c-159">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="a605c-160">Ignorez le message d’avertissement suivant ; vous le traiterez dans le prochain tutoriel :</span><span class="sxs-lookup"><span data-stu-id="a605c-160">Ignore the following warning message, you fix that in the next tutorial:</span></span>

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

<span data-ttu-id="a605c-161">La commande `Add-Migration` génère le code nécessaire à la création du schéma de base de données initial.</span><span class="sxs-lookup"><span data-stu-id="a605c-161">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="a605c-162">Le schéma est basé sur le modèle spécifié dans `RazorPagesMovieContext` (dans le fichier *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="a605c-162">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="a605c-163">L’argument `Initial` est utilisé pour nommer les migrations.</span><span class="sxs-lookup"><span data-stu-id="a605c-163">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="a605c-164">Vous pouvez utiliser n’importe quel nom, mais par convention, choisissez un nom qui décrit la migration.</span><span class="sxs-lookup"><span data-stu-id="a605c-164">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="a605c-165">Pour plus d’informations, consultez [Présentation des migrations](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="a605c-165">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="a605c-166">La commande `Update-Database` exécute la méthode `Up` dans le fichier *Migrations/{horodatage}_InitialCreate.cs*, ce qui entraîne la création de la base de données.</span><span class="sxs-lookup"><span data-stu-id="a605c-166">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="a605c-167">Si vous obtenez l’erreur :</span><span class="sxs-lookup"><span data-stu-id="a605c-167">If you get the error:</span></span>

<span data-ttu-id="a605c-168">SqlException: impossible d’ouvrir la base de données 'RazorPagesMovieContext-GUID' demandée par la connexion.</span><span class="sxs-lookup"><span data-stu-id="a605c-168">SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login.</span></span> <span data-ttu-id="a605c-169">La connexion a échoué.</span><span class="sxs-lookup"><span data-stu-id="a605c-169">The login failed.</span></span>
<span data-ttu-id="a605c-170">Échec de la connexion de l’utilisateur 'nom utilisateur'.</span><span class="sxs-lookup"><span data-stu-id="a605c-170">Login failed for user 'User-name'.</span></span>

<span data-ttu-id="a605c-171">Vous avez manqué [l’étape des migrations](#pmc).</span><span class="sxs-lookup"><span data-stu-id="a605c-171">You missed the [migrations step](#pmc).</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="a605c-172">Ajouter un modèle de données</span><span class="sxs-lookup"><span data-stu-id="a605c-172">Add a data model</span></span>

<span data-ttu-id="a605c-173">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **RazorPagesMovie** > **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="a605c-173">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="a605c-174">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="a605c-174">Name the folder *Models*.</span></span>

<span data-ttu-id="a605c-175">Cliquez avec le bouton droit sur le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="a605c-175">Right click the *Models* folder.</span></span> <span data-ttu-id="a605c-176">Sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="a605c-176">Select **Add** > **Class**.</span></span> <span data-ttu-id="a605c-177">Nommez la classe **Movie**, puis ajoutez les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="a605c-177">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="a605c-178">Ajouter une chaîne de connexion de base de données</span><span class="sxs-lookup"><span data-stu-id="a605c-178">Add a database connection string</span></span>

<span data-ttu-id="a605c-179">Ajoutez une chaîne de connexion au fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a605c-179">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="a605c-180">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="a605c-180">Register the database context</span></span>

<span data-ttu-id="a605c-181">Inscrivez le contexte de base de données auprès du conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) dans la [méthode ConfigureServices de la classe Startup](xref:fundamentals/startup#the-startup-class) (*Startup.cs*) :</span><span class="sxs-lookup"><span data-stu-id="a605c-181">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="a605c-182">Générez le projet pour vérifier qu’il ne comporte aucune erreur.</span><span class="sxs-lookup"><span data-stu-id="a605c-182">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="a605c-183">Ajouter un outil de génération de modèles automatique et effectuer la migration initiale</span><span class="sxs-lookup"><span data-stu-id="a605c-183">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="a605c-184">Dans cette section, effectuez les tâches suivantes à l’aide de la console du gestionnaire de package :</span><span class="sxs-lookup"><span data-stu-id="a605c-184">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="a605c-185">Ajoutez le package de génération de code web Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a605c-185">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="a605c-186">Ce package est nécessaire à l’exécution du moteur de génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="a605c-186">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="a605c-187">Ajoutez une migration initiale.</span><span class="sxs-lookup"><span data-stu-id="a605c-187">Add an initial migration.</span></span>
* <span data-ttu-id="a605c-188">Mettez à jour la base de données avec la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="a605c-188">Update the database with the initial migration.</span></span>

<span data-ttu-id="a605c-189">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** > **Console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="a605c-189">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu Console du Gestionnaire de package](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="a605c-191">Dans la console du Gestionnaire de package, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="a605c-191">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="a605c-192">Vous pouvez aussi utiliser les commandes .NET Core CLI suivantes :</span><span class="sxs-lookup"><span data-stu-id="a605c-192">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="a605c-193">Ignorez le message suivant :</span><span class="sxs-lookup"><span data-stu-id="a605c-193">Ignore the following message:</span></span>

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

<span data-ttu-id="a605c-194">Vous le traiterez dans le prochain tutoriel.</span><span class="sxs-lookup"><span data-stu-id="a605c-194">You fix that in the next tutorial.</span></span>

<span data-ttu-id="a605c-195">La commande `Install-Package` installe les outils nécessaires à l’exécution du moteur de génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="a605c-195">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="a605c-196">La commande `Add-Migration` génère le code nécessaire à la création du schéma de base de données initial.</span><span class="sxs-lookup"><span data-stu-id="a605c-196">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="a605c-197">Le schéma est basé sur le modèle spécifié dans le fichier `DbContext` (dans *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="a605c-197">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="a605c-198">L’argument `Initial` est utilisé pour nommer les migrations.</span><span class="sxs-lookup"><span data-stu-id="a605c-198">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="a605c-199">Vous pouvez utiliser n’importe quel nom, mais par convention, choisissez un nom qui décrit la migration.</span><span class="sxs-lookup"><span data-stu-id="a605c-199">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="a605c-200">Pour plus d’informations, consultez [Présentation des migrations](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="a605c-200">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="a605c-201">La commande `Update-Database` exécute la méthode `Up` dans le fichier *Migrations/{horodatage}_InitialCreate.cs*, ce qui entraîne la création de la base de données.</span><span class="sxs-lookup"><span data-stu-id="a605c-201">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="a605c-202">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="a605c-202">Test the app</span></span>

* <span data-ttu-id="a605c-203">Exécutez l’application et ajoutez `/Movies` à l’URL dans le navigateur (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="a605c-203">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="a605c-204">Testez le lien **Créer**.</span><span class="sxs-lookup"><span data-stu-id="a605c-204">Test the **Create** link.</span></span>

  ![Créer une page](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="a605c-206">Testez les liens **Modifier**, **Détails** et **Supprimer**.</span><span class="sxs-lookup"><span data-stu-id="a605c-206">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="a605c-207">Si vous obtenez une exception SQL, vérifiez que vous avez exécuté les migrations et mis à jour la base de données.</span><span class="sxs-lookup"><span data-stu-id="a605c-207">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="a605c-208">Le prochain didacticiel décrit les fichiers créés par la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="a605c-208">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a605c-209">[Précédent : Bien démarrer](xref:tutorials/razor-pages/razor-pages-start)
> [Suivant : Pages Razor obtenues par génération de modèles automatique](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="a605c-209">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
