---
title: Ajouter un modèle à une application de pages Razor dans ASP.NET Core
author: rick-anderson
description: Découvrez comment ajouter des classes pour gérer des films dans une base de données à l’aide d’Entity Framework Core (EF Core).
ms.author: riande
ms.date: 12/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: ef4671c9e7628c106b9f68ba5cbfd8a127e095d0
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75358027"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="7c59e-103">Ajouter un modèle à une application de pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7c59e-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="7c59e-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7c59e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<!-- In the next update on the CLI version, let the scaffolder do the same work the VS driven scaffolder does. That is, create the DB context, etc -->

<span data-ttu-id="7c59e-105">Dans cette section, des classes sont ajoutées pour la gestion des films dans une [base de données SQLite](https://www.sqlite.org/index.html)multiplateforme.</span><span class="sxs-lookup"><span data-stu-id="7c59e-105">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="7c59e-106">Les applications créées à partir d’un modèle de ASP.NET Core utilisent une base de données SQLite.</span><span class="sxs-lookup"><span data-stu-id="7c59e-106">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="7c59e-107">Les classes de modèle de l’application sont utilisées avec [Entity Framework Core (EF Core)](/ef/core) ([fournisseur de base de données SQLite EF Core](/ef/core/providers/sqlite)) pour fonctionner avec la base de données.</span><span class="sxs-lookup"><span data-stu-id="7c59e-107">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="7c59e-108">EF Core est un framework de mappage relationnel d’objets qui simplifie l’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="7c59e-108">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="7c59e-109">Les classes de modèle portent le nom de classes OCT (« Objet CLR Traditionnel »), car elles n’ont pas de dépendances envers EF Core.</span><span class="sxs-lookup"><span data-stu-id="7c59e-109">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="7c59e-110">Elles définissent les propriétés des données stockées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="7c59e-110">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="7c59e-111">Ajouter un modèle de données</span><span class="sxs-lookup"><span data-stu-id="7c59e-111">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7c59e-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c59e-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7c59e-113">Cliquez avec le bouton droit sur le projet **RazorPagesMovie** > **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="7c59e-114">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="7c59e-114">Name the folder *Models*.</span></span>

<span data-ttu-id="7c59e-115">Cliquez avec le bouton droit sur le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="7c59e-115">Right click the *Models* folder.</span></span> <span data-ttu-id="7c59e-116">Sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="7c59e-117">Nommez la classe **Movie**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7c59e-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c59e-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="7c59e-119">Ajoutez un dossier nommé *Models*.</span><span class="sxs-lookup"><span data-stu-id="7c59e-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="7c59e-120">Ajoutez une classe au dossier *Modèles* nommé *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="7c59e-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7c59e-121">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="7c59e-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="7c59e-122">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **RazorPagesMovie**, puis sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="7c59e-123">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="7c59e-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="7c59e-124">Cliquez avec le bouton droit sur le dossier *Models* , puis sélectionnez **Ajouter** > **nouveau fichier**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="7c59e-125">Dans la boîte de dialogue **Nouveau fichier** :</span><span class="sxs-lookup"><span data-stu-id="7c59e-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="7c59e-126">Dans le volet gauche, sélectionnez **Général**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="7c59e-127">Dans le volet central, sélectionnez **Classe vide**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-127">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="7c59e-128">Nommez la classe **Movie**, puis sélectionnez **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="7c59e-129">Générez le projet pour vérifier qu’il n’y a pas d’erreur de compilation.</span><span class="sxs-lookup"><span data-stu-id="7c59e-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="7c59e-130">Générer automatiquement le modèle de film</span><span class="sxs-lookup"><span data-stu-id="7c59e-130">Scaffold the movie model</span></span>

<span data-ttu-id="7c59e-131">Dans cette section, le modèle de film est généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="7c59e-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="7c59e-132">Autrement dit, l’outil de génération de modèles automatique génère des pages pour les opérations de création, de lecture, de mise à jour et de suppression (CRUD) pour le modèle de film.</span><span class="sxs-lookup"><span data-stu-id="7c59e-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7c59e-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c59e-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7c59e-134">Créer un dossier *Pages/Movies* :</span><span class="sxs-lookup"><span data-stu-id="7c59e-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="7c59e-135">Cliquez avec le bouton droit sur le dossier *Pages* > **Ajouter** > **nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="7c59e-136">Nommez le dossier *Movies*.</span><span class="sxs-lookup"><span data-stu-id="7c59e-136">Name the folder *Movies*</span></span>

<span data-ttu-id="7c59e-137">Cliquez avec le bouton droit sur le dossier *pages/movies* > **Ajoutez** > **nouvel élément de génération de modèles**automatique.</span><span class="sxs-lookup"><span data-stu-id="7c59e-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/sca.png)

<span data-ttu-id="7c59e-139">Dans la boîte de dialogue **Ajouter une structure** , sélectionnez **Razor pages à l’aide de Entity Framework (CRUD)** > **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/add_scaffold.png)

<span data-ttu-id="7c59e-141">Renseignez la boîte de dialogue **Pages Razor avec Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="7c59e-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="7c59e-142">Dans la liste déroulante **Classe de modèle**, sélectionnez **Film (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="7c59e-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="7c59e-143">Dans la ligne **Classe du contexte de données**, sélectionnez le signe (plus) **+** et changez le nom généré de RazorPagesMovie.**Models**.RazorPagesMovieContext en RazorPagesMovie.**Data**.RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="7c59e-143">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="7c59e-144">[Cette modification](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) n'est pas requise.</span><span class="sxs-lookup"><span data-stu-id="7c59e-144">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="7c59e-145">Elle crée la classe de contexte de base de données avec l’espace de noms correct.</span><span class="sxs-lookup"><span data-stu-id="7c59e-145">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="7c59e-146">Sélectionnez **Ajouter** .</span><span class="sxs-lookup"><span data-stu-id="7c59e-146">Select **Add**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/3/arp.png)

<span data-ttu-id="7c59e-148">Le fichier *appsettings.json* est mis à jour avec la chaîne de connexion utilisée pour se connecter à une base de données locale.</span><span class="sxs-lookup"><span data-stu-id="7c59e-148">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7c59e-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c59e-149">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="7c59e-150">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="7c59e-150">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="7c59e-151">Installez l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="7c59e-151">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="7c59e-152">**Pour Windows**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="7c59e-152">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="7c59e-153">**Pour macOS et Linux**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="7c59e-153">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7c59e-154">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="7c59e-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="7c59e-155">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="7c59e-155">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="7c59e-156">Installez l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="7c59e-156">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="7c59e-157">Exécutez la commande suivante : .</span><span class="sxs-lookup"><span data-stu-id="7c59e-157">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

---

### <a name="files-created"></a><span data-ttu-id="7c59e-158">Fichiers créés</span><span class="sxs-lookup"><span data-stu-id="7c59e-158">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7c59e-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c59e-159">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7c59e-160">Le processus de génération de modèles automatique crée et met à jour les fichiers suivants :</span><span class="sxs-lookup"><span data-stu-id="7c59e-160">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="7c59e-161">*Pages/Movies* : Create, Delete, Details, Edit, Index.</span><span class="sxs-lookup"><span data-stu-id="7c59e-161">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="7c59e-162">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="7c59e-162">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="7c59e-163">Mis à jour</span><span class="sxs-lookup"><span data-stu-id="7c59e-163">Updated</span></span>

* <span data-ttu-id="7c59e-164">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="7c59e-164">*Startup.cs*</span></span>

<span data-ttu-id="7c59e-165">Les fichiers créés et mis à jour sont expliqués dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="7c59e-165">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="7c59e-166">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="7c59e-166">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="7c59e-167">Le processus de génération de modèles automatique crée les fichiers suivants :</span><span class="sxs-lookup"><span data-stu-id="7c59e-167">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="7c59e-168">*Pages/Movies* : Create, Delete, Details, Edit, Index.</span><span class="sxs-lookup"><span data-stu-id="7c59e-168">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="7c59e-169">Les fichiers créés sont expliqués dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="7c59e-169">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="7c59e-170">Migration initiale</span><span class="sxs-lookup"><span data-stu-id="7c59e-170">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7c59e-171">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c59e-171">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7c59e-172">Dans cette section, la console du gestionnaire de package est utilisée pour :</span><span class="sxs-lookup"><span data-stu-id="7c59e-172">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="7c59e-173">Ajoutez une migration initiale.</span><span class="sxs-lookup"><span data-stu-id="7c59e-173">Add an initial migration.</span></span>
* <span data-ttu-id="7c59e-174">Mettez à jour la base de données avec la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="7c59e-174">Update the database with the initial migration.</span></span>

<span data-ttu-id="7c59e-175">Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet** > **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-175">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu Console du Gestionnaire de package](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="7c59e-177">Dans la console du gestionnaire de package, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="7c59e-177">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7c59e-178">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c59e-178">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7c59e-179">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="7c59e-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="7c59e-180">Les commandes précédentes génèrent l’avertissement suivant : « aucun type n’a été spécifié pour la colonne décimale «Price » sur le type d’entité « Movie ».</span><span class="sxs-lookup"><span data-stu-id="7c59e-180">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="7c59e-181">Les valeurs sont tronquées en mode silencieux si elles ne sont pas compatibles avec la précision et l’échelle par défaut.</span><span class="sxs-lookup"><span data-stu-id="7c59e-181">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="7c59e-182">Spécifiez explicitement le type de colonne SQL Server capable d’accueillir toutes les valeurs en utilisant 'HasColumnType()'. »</span><span class="sxs-lookup"><span data-stu-id="7c59e-182">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="7c59e-183">Vous pouvez ignorer cet avertissement, il sera corrigé dans un prochain tutoriel.</span><span class="sxs-lookup"><span data-stu-id="7c59e-183">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="7c59e-184">La commande migrations génère du code pour créer le schéma de base de données initial.</span><span class="sxs-lookup"><span data-stu-id="7c59e-184">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="7c59e-185">Le schéma est basé sur le modèle spécifié dans `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="7c59e-185">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="7c59e-186">L’argument `InitialCreate` est utilisé pour nommer les migrations.</span><span class="sxs-lookup"><span data-stu-id="7c59e-186">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="7c59e-187">Vous pouvez utiliser n’importe quel nom, mais par convention, un nom décrivant la migration est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="7c59e-187">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="7c59e-188">La commande `update` exécute la méthode `Up` dans les migrations qui n’ont pas été appliquées.</span><span class="sxs-lookup"><span data-stu-id="7c59e-188">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="7c59e-189">Dans ce cas, `update` exécute la méthode `Up` dans les *migrations/\<horodatage > _InitialCreate fichier. cs* , ce qui crée la base de données.</span><span class="sxs-lookup"><span data-stu-id="7c59e-189">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7c59e-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c59e-190">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="7c59e-191">Examiner le contexte inscrit avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="7c59e-191">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="7c59e-192">ASP.NET Core comprend [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7c59e-192">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="7c59e-193">Des services (tels que le contexte de base de données EF Core) sont inscrits avec l’injection de dépendances au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="7c59e-193">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="7c59e-194">Ces services sont affectés aux composants qui les nécessitent (par exemple les Pages Razor) par le biais de paramètres de constructeur.</span><span class="sxs-lookup"><span data-stu-id="7c59e-194">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="7c59e-195">Le code du constructeur qui obtient une instance de contexte de base de données est indiqué plus loin dans le tutoriel.</span><span class="sxs-lookup"><span data-stu-id="7c59e-195">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="7c59e-196">L’outil de génération de modèles automatique a créé automatiquement un contexte de base de données et l’a inscrit dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="7c59e-196">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="7c59e-197">Examinez la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7c59e-197">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="7c59e-198">La ligne en surbrillance a été ajoutée par l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="7c59e-198">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="7c59e-199">`RazorPagesMovieContext` coordonne les fonctionnalités d’EF Core (Create, Read, Update, Delete, etc.) pour le modèle `Movie`.</span><span class="sxs-lookup"><span data-stu-id="7c59e-199">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="7c59e-200">Le contexte de données (`RazorPagesMovieContext`) est dérivé de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="7c59e-200">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="7c59e-201">Il spécifie les entités qui sont incluses dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="7c59e-201">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="7c59e-202">Le code précédent crée une propriété [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) pour le jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="7c59e-202">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="7c59e-203">Dans la terminologie Entity Framework, un jeu d’entités correspond généralement à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="7c59e-203">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="7c59e-204">Une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="7c59e-204">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="7c59e-205">Le nom de la chaîne de connexion est transmis au contexte en appelant une méthode sur un objet [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="7c59e-205">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="7c59e-206">Pour le développement local, le [système de configuration ASP.NET Core](xref:fundamentals/configuration/index) lit la chaîne de connexion à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="7c59e-206">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7c59e-207">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c59e-207">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7c59e-208">Examinez la méthode `Up`.</span><span class="sxs-lookup"><span data-stu-id="7c59e-208">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7c59e-209">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="7c59e-209">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="7c59e-210">Examinez la méthode `Up`.</span><span class="sxs-lookup"><span data-stu-id="7c59e-210">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="7c59e-211">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="7c59e-211">Test the app</span></span>

* <span data-ttu-id="7c59e-212">Exécutez l’application et ajoutez `/Movies` à l’URL dans le navigateur (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="7c59e-212">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="7c59e-213">Si vous obtenez cette erreur :</span><span class="sxs-lookup"><span data-stu-id="7c59e-213">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="7c59e-214">Vous avez manqué [l’étape des migrations](#pmc).</span><span class="sxs-lookup"><span data-stu-id="7c59e-214">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="7c59e-215">Testez le lien **Créer**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-215">Test the **Create** link.</span></span>

  ![Page Créer](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="7c59e-217">Vous ne pourrez peut-être pas entrer de virgules décimales dans le champ `Price`.</span><span class="sxs-lookup"><span data-stu-id="7c59e-217">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="7c59e-218">Pour prendre en charge la [validation jQuery](https://jqueryvalidation.org/) pour les paramètres régionaux autres que « Anglais » qui utilisent une virgule (« , ») comme décimale et des formats de date autres que le format « Anglais (États-Unis »), l’application doit être localisée.</span><span class="sxs-lookup"><span data-stu-id="7c59e-218">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="7c59e-219">Pour obtenir des instructions sur la localisation, consultez [ce problème GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="7c59e-219">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="7c59e-220">Testez les liens **Modifier**, **Détails** et **Supprimer**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-220">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="7c59e-221">Le prochain didacticiel décrit les fichiers créés par la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="7c59e-221">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7c59e-222">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="7c59e-222">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7c59e-223">[Précédent : Bien démarrer](xref:tutorials/razor-pages/razor-pages-start)
> [Suivant : Pages Razor obtenues par génération de modèles automatique](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="7c59e-223">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7c59e-224">Dans cette section, des classes sont ajoutées pour la gestion des films dans une [base de données SQLite](https://www.sqlite.org/index.html)multiplateforme.</span><span class="sxs-lookup"><span data-stu-id="7c59e-224">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="7c59e-225">Les applications créées à partir d’un modèle de ASP.NET Core utilisent une base de données SQLite.</span><span class="sxs-lookup"><span data-stu-id="7c59e-225">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="7c59e-226">Les classes de modèle de l’application sont utilisées avec [Entity Framework Core (EF Core)](/ef/core) ([fournisseur de base de données SQLite EF Core](/ef/core/providers/sqlite)) pour fonctionner avec la base de données.</span><span class="sxs-lookup"><span data-stu-id="7c59e-226">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="7c59e-227">EF Core est un framework de mappage relationnel d’objets qui simplifie l’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="7c59e-227">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="7c59e-228">Les classes de modèle portent le nom de classes OCT (« Objet CLR Traditionnel »), car elles n’ont pas de dépendances envers EF Core.</span><span class="sxs-lookup"><span data-stu-id="7c59e-228">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="7c59e-229">Elles définissent les propriétés des données stockées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="7c59e-229">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="7c59e-230">Ajouter un modèle de données</span><span class="sxs-lookup"><span data-stu-id="7c59e-230">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7c59e-231">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c59e-231">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7c59e-232">Cliquez avec le bouton droit sur le projet **RazorPagesMovie** > **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-232">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="7c59e-233">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="7c59e-233">Name the folder *Models*.</span></span>

<span data-ttu-id="7c59e-234">Cliquez avec le bouton droit sur le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="7c59e-234">Right click the *Models* folder.</span></span> <span data-ttu-id="7c59e-235">Sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-235">Select **Add** > **Class**.</span></span> <span data-ttu-id="7c59e-236">Nommez la classe **Movie**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-236">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7c59e-237">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c59e-237">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="7c59e-238">Ajoutez un dossier nommé *Models*.</span><span class="sxs-lookup"><span data-stu-id="7c59e-238">Add a folder named *Models*.</span></span>
* <span data-ttu-id="7c59e-239">Ajoutez une classe au dossier *Modèles* nommé *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="7c59e-239">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7c59e-240">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="7c59e-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="7c59e-241">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **RazorPagesMovie**, puis sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-241">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="7c59e-242">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="7c59e-242">Name the folder *Models*.</span></span>
* <span data-ttu-id="7c59e-243">Cliquez avec le bouton droit sur le dossier *Models* , puis sélectionnez **Ajouter** > **nouveau fichier**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-243">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="7c59e-244">Dans la boîte de dialogue **Nouveau fichier** :</span><span class="sxs-lookup"><span data-stu-id="7c59e-244">In the **New File** dialog:</span></span>

  * <span data-ttu-id="7c59e-245">Dans le volet gauche, sélectionnez **Général**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-245">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="7c59e-246">Dans le volet central, sélectionnez **Classe vide**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-246">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="7c59e-247">Nommez la classe **Movie**, puis sélectionnez **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-247">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="7c59e-248">Générez le projet pour vérifier qu’il n’y a pas d’erreur de compilation.</span><span class="sxs-lookup"><span data-stu-id="7c59e-248">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="7c59e-249">Générer automatiquement le modèle de film</span><span class="sxs-lookup"><span data-stu-id="7c59e-249">Scaffold the movie model</span></span>

<span data-ttu-id="7c59e-250">Dans cette section, le modèle de film est généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="7c59e-250">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="7c59e-251">Autrement dit, l’outil de génération de modèles automatique génère des pages pour les opérations de création, de lecture, de mise à jour et de suppression (CRUD) pour le modèle de film.</span><span class="sxs-lookup"><span data-stu-id="7c59e-251">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7c59e-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c59e-252">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7c59e-253">Créer un dossier *Pages/Movies* :</span><span class="sxs-lookup"><span data-stu-id="7c59e-253">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="7c59e-254">Cliquez avec le bouton droit sur le dossier *Pages* > **Ajouter** > **nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-254">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="7c59e-255">Nommez le dossier *Movies*.</span><span class="sxs-lookup"><span data-stu-id="7c59e-255">Name the folder *Movies*</span></span>

<span data-ttu-id="7c59e-256">Cliquez avec le bouton droit sur le dossier *pages/movies* > **Ajoutez** > **nouvel élément de génération de modèles**automatique.</span><span class="sxs-lookup"><span data-stu-id="7c59e-256">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/sca.png)

<span data-ttu-id="7c59e-258">Dans la boîte de dialogue **Ajouter une structure** , sélectionnez **Razor pages à l’aide de Entity Framework (CRUD)** > **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-258">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/add_scaffold.png)

<span data-ttu-id="7c59e-260">Renseignez la boîte de dialogue **Pages Razor avec Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="7c59e-260">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="7c59e-261">Dans la liste déroulante **Classe de modèle**, sélectionnez **Film (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="7c59e-261">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="7c59e-262">Dans la ligne **Classe du contexte de données**, sélectionnez le signe (plus) **+** et acceptez le nom généré **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-262">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="7c59e-263">Sélectionnez **Ajouter** .</span><span class="sxs-lookup"><span data-stu-id="7c59e-263">Select **Add**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/arp.png)

<span data-ttu-id="7c59e-265">Le fichier *appsettings.json* est mis à jour avec la chaîne de connexion utilisée pour se connecter à une base de données locale.</span><span class="sxs-lookup"><span data-stu-id="7c59e-265">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7c59e-266">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c59e-266">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="7c59e-267">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="7c59e-267">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="7c59e-268">**Pour Windows**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="7c59e-268">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="7c59e-269">**Pour macOS et Linux**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="7c59e-269">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7c59e-270">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="7c59e-270">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="7c59e-271">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="7c59e-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="7c59e-272">Exécutez la commande suivante : .</span><span class="sxs-lookup"><span data-stu-id="7c59e-272">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="7c59e-273">Le processus de génération de modèles automatique crée et met à jour les fichiers suivants :</span><span class="sxs-lookup"><span data-stu-id="7c59e-273">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="7c59e-274">Fichiers créés</span><span class="sxs-lookup"><span data-stu-id="7c59e-274">Files created</span></span>

* <span data-ttu-id="7c59e-275">*Pages/Movies* : Create, Delete, Details, Edit, Index.</span><span class="sxs-lookup"><span data-stu-id="7c59e-275">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="7c59e-276">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="7c59e-276">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="7c59e-277">Fichier mis à jour</span><span class="sxs-lookup"><span data-stu-id="7c59e-277">File updated</span></span>

* <span data-ttu-id="7c59e-278">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="7c59e-278">*Startup.cs*</span></span>

<span data-ttu-id="7c59e-279">Les fichiers créés et mis à jour sont expliqués dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="7c59e-279">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="7c59e-280">Migration initiale</span><span class="sxs-lookup"><span data-stu-id="7c59e-280">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7c59e-281">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c59e-281">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7c59e-282">Dans cette section, la console du gestionnaire de package est utilisée pour :</span><span class="sxs-lookup"><span data-stu-id="7c59e-282">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="7c59e-283">Ajoutez une migration initiale.</span><span class="sxs-lookup"><span data-stu-id="7c59e-283">Add an initial migration.</span></span>
* <span data-ttu-id="7c59e-284">Mettez à jour la base de données avec la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="7c59e-284">Update the database with the initial migration.</span></span>

<span data-ttu-id="7c59e-285">Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet** > **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-285">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu Console du Gestionnaire de package](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="7c59e-287">Dans la console du gestionnaire de package, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="7c59e-287">In the PMC, enter the following commands:</span></span>

```Powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="7c59e-288">La commande `Add-Migration` génère du code pour créer le schéma de base de données initial.</span><span class="sxs-lookup"><span data-stu-id="7c59e-288">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="7c59e-289">Le schéma est basé sur le modèle spécifié dans `DbContext` (dans le fichier *RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="7c59e-289">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="7c59e-290">L’argument `InitialCreate` est utilisé pour nommer la migration.</span><span class="sxs-lookup"><span data-stu-id="7c59e-290">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="7c59e-291">Vous pouvez utiliser n’importe quel nom, mais par convention, un nom décrivant la migration est utilisé.</span><span class="sxs-lookup"><span data-stu-id="7c59e-291">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="7c59e-292">Pour plus d'informations, consultez <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="7c59e-292">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="7c59e-293">La commande `Update-Database` exécute la méthode `Up` dans le fichier *Migrations/\<horodatage>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="7c59e-293">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="7c59e-294">La méthode `Up` crée la base de données.</span><span class="sxs-lookup"><span data-stu-id="7c59e-294">The `Up` method creates the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7c59e-295">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c59e-295">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7c59e-296">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="7c59e-296">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="7c59e-297">Les commandes précédentes génèrent l’avertissement suivant : «*aucun type n’a été spécifié pour la colonne décimale «Price » sur le type d’entité « Movie ». Les valeurs sont tronquées en mode silencieux si elles ne tiennent pas dans la précision et l’échelle par défaut. Spécifiez explicitement le type de colonne SQL Server qui peut prendre en charge toutes les valeurs à l’aide de’HasColumnType () '.* Vous pouvez ignorer cet avertissement. il sera corrigé dans un didacticiel ultérieur.</span><span class="sxs-lookup"><span data-stu-id="7c59e-297">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7c59e-298">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c59e-298">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="7c59e-299">Examiner le contexte inscrit avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="7c59e-299">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="7c59e-300">ASP.NET Core comprend [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7c59e-300">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="7c59e-301">Des services (tels que le contexte de base de données EF Core) sont inscrits avec l’injection de dépendances au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="7c59e-301">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="7c59e-302">Ces services sont affectés aux composants qui les nécessitent (par exemple les Pages Razor) par le biais de paramètres de constructeur.</span><span class="sxs-lookup"><span data-stu-id="7c59e-302">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="7c59e-303">Le code du constructeur qui obtient une instance de contexte de base de données est indiqué plus loin dans le tutoriel.</span><span class="sxs-lookup"><span data-stu-id="7c59e-303">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="7c59e-304">L’outil de génération de modèles automatique a créé automatiquement un contexte de base de données et l’a inscrit dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="7c59e-304">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="7c59e-305">Examinez la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7c59e-305">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="7c59e-306">La ligne en surbrillance a été ajoutée par l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="7c59e-306">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="7c59e-307">`RazorPagesMovieContext` coordonne les fonctionnalités d’EF Core (Create, Read, Update, Delete, etc.) pour le modèle `Movie`.</span><span class="sxs-lookup"><span data-stu-id="7c59e-307">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="7c59e-308">Le contexte de données (`RazorPagesMovieContext`) est dérivé de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="7c59e-308">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="7c59e-309">Il spécifie les entités qui sont incluses dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="7c59e-309">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="7c59e-310">Le code précédent crée une propriété [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) pour le jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="7c59e-310">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="7c59e-311">Dans la terminologie Entity Framework, un jeu d’entités correspond généralement à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="7c59e-311">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="7c59e-312">Une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="7c59e-312">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="7c59e-313">Le nom de la chaîne de connexion est transmis au contexte en appelant une méthode sur un objet [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="7c59e-313">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="7c59e-314">Pour le développement local, le [système de configuration ASP.NET Core](xref:fundamentals/configuration/index) lit la chaîne de connexion à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="7c59e-314">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7c59e-315">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c59e-315">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7c59e-316">Examinez la méthode `Up`.</span><span class="sxs-lookup"><span data-stu-id="7c59e-316">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7c59e-317">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="7c59e-317">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="7c59e-318">Examinez la méthode `Up`.</span><span class="sxs-lookup"><span data-stu-id="7c59e-318">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="7c59e-319">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="7c59e-319">Test the app</span></span>

* <span data-ttu-id="7c59e-320">Exécutez l’application et ajoutez `/Movies` à l’URL dans le navigateur (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="7c59e-320">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="7c59e-321">Si vous obtenez cette erreur :</span><span class="sxs-lookup"><span data-stu-id="7c59e-321">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="7c59e-322">Vous avez manqué [l’étape des migrations](#pmc).</span><span class="sxs-lookup"><span data-stu-id="7c59e-322">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="7c59e-323">Testez le lien **Créer**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-323">Test the **Create** link.</span></span>

  ![Page Créer](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="7c59e-325">Vous ne pourrez peut-être pas entrer de virgules décimales dans le champ `Price`.</span><span class="sxs-lookup"><span data-stu-id="7c59e-325">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="7c59e-326">Pour prendre en charge la [validation jQuery](https://jqueryvalidation.org/) pour les paramètres régionaux autres que « Anglais » qui utilisent une virgule (« , ») comme décimale et des formats de date autres que le format « Anglais (États-Unis »), l’application doit être localisée.</span><span class="sxs-lookup"><span data-stu-id="7c59e-326">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="7c59e-327">Pour obtenir des instructions sur la localisation, consultez [ce problème GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="7c59e-327">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="7c59e-328">Testez les liens **Modifier**, **Détails** et **Supprimer**.</span><span class="sxs-lookup"><span data-stu-id="7c59e-328">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="7c59e-329">Le prochain didacticiel décrit les fichiers créés par la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="7c59e-329">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7c59e-330">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="7c59e-330">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7c59e-331">[Précédent : Bien démarrer](xref:tutorials/razor-pages/razor-pages-start)
> [Suivant : Pages Razor obtenues par génération de modèles automatique](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="7c59e-331">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
