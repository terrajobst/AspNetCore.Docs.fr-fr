---
title: Ajouter un modèle à une application de pages Razor dans ASP.NET Core
author: rick-anderson
description: Découvrez comment ajouter des classes pour gérer des films dans une base de données à l’aide d’Entity Framework Core (EF Core).
ms.author: riande
ms.date: 12/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: f6dbac81b4efceb30c379ab06dd715005d879228
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658934"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="a7923-103">Ajouter un modèle à une application de pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a7923-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="a7923-104">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a7923-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<!-- In the next update on the CLI version, let the scaffolder do the same work the VS driven scaffolder does. That is, create the DB context, etc -->

<span data-ttu-id="a7923-105">Dans cette section, des classes sont ajoutées pour la gestion des films dans une [base de données SQLite](https://www.sqlite.org/index.html)multiplateforme.</span><span class="sxs-lookup"><span data-stu-id="a7923-105">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="a7923-106">Les applications créées à partir d’un modèle de ASP.NET Core utilisent une base de données SQLite.</span><span class="sxs-lookup"><span data-stu-id="a7923-106">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="a7923-107">Les classes de modèle de l’application sont utilisées avec [Entity Framework Core (EF Core)](/ef/core) ([fournisseur de base de données SQLite EF Core](/ef/core/providers/sqlite)) pour fonctionner avec la base de données.</span><span class="sxs-lookup"><span data-stu-id="a7923-107">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="a7923-108">EF Core est un framework de mappage relationnel d’objets qui simplifie l’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="a7923-108">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="a7923-109">Les classes de modèle portent le nom de classes OCT (« Objet CLR Traditionnel »), car elles n’ont pas de dépendances envers EF Core.</span><span class="sxs-lookup"><span data-stu-id="a7923-109">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="a7923-110">Elles définissent les propriétés des données stockées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="a7923-110">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="a7923-111">Ajouter un modèle de données</span><span class="sxs-lookup"><span data-stu-id="a7923-111">Add a data model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a7923-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a7923-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a7923-113">Cliquez avec le bouton droit sur le projet **RazorPagesMovie** > **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="a7923-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="a7923-114">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="a7923-114">Name the folder *Models*.</span></span>

<span data-ttu-id="a7923-115">Cliquez avec le bouton droit sur le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="a7923-115">Right click the *Models* folder.</span></span> <span data-ttu-id="a7923-116">Sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="a7923-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="a7923-117">Nommez la classe **Movie**.</span><span class="sxs-lookup"><span data-stu-id="a7923-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="a7923-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a7923-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a7923-119">Ajoutez un dossier nommé *Models*.</span><span class="sxs-lookup"><span data-stu-id="a7923-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="a7923-120">Ajoutez une classe au dossier *Modèles* nommé *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="a7923-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="a7923-121">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="a7923-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a7923-122">Dans Panneau Solutions, cliquez avec le bouton droit sur le projet **RazorPagesMovie** , puis sélectionnez **Ajouter** > **nouveau dossier...** . Nommez le dossier *modèles*.</span><span class="sxs-lookup"><span data-stu-id="a7923-122">In Solution Pad, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder...**. Name the folder *Models*.</span></span>
* <span data-ttu-id="a7923-123">Cliquez avec le bouton droit sur le dossier *Models* , puis sélectionnez **Ajouter** > **nouveau fichier...** .</span><span class="sxs-lookup"><span data-stu-id="a7923-123">Right-click the *Models* folder, and then select **Add** > **New File...**.</span></span>
* <span data-ttu-id="a7923-124">Dans la boîte de dialogue **Nouveau fichier** :</span><span class="sxs-lookup"><span data-stu-id="a7923-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="a7923-125">Dans le volet gauche, sélectionnez **Général**.</span><span class="sxs-lookup"><span data-stu-id="a7923-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="a7923-126">Dans le volet central, sélectionnez **Classe vide**.</span><span class="sxs-lookup"><span data-stu-id="a7923-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="a7923-127">Nommez la classe **Movie**, puis sélectionnez **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="a7923-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="a7923-128">Générez le projet pour vérifier qu’il n’y a pas d’erreur de compilation.</span><span class="sxs-lookup"><span data-stu-id="a7923-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="a7923-129">Générer automatiquement le modèle de film</span><span class="sxs-lookup"><span data-stu-id="a7923-129">Scaffold the movie model</span></span>

<span data-ttu-id="a7923-130">Dans cette section, le modèle de film est généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="a7923-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="a7923-131">Autrement dit, l’outil de génération de modèles automatique génère des pages pour les opérations de création, de lecture, de mise à jour et de suppression (CRUD) pour le modèle de film.</span><span class="sxs-lookup"><span data-stu-id="a7923-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a7923-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a7923-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a7923-133">Créer un dossier *Pages/Movies* :</span><span class="sxs-lookup"><span data-stu-id="a7923-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="a7923-134">Cliquez avec le bouton droit sur le dossier *Pages* > **Ajouter** > **nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="a7923-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="a7923-135">Nommez le dossier *Movies*.</span><span class="sxs-lookup"><span data-stu-id="a7923-135">Name the folder *Movies*</span></span>

<span data-ttu-id="a7923-136">Cliquez avec le bouton droit sur le dossier *pages/movies* > **Ajoutez** > **nouvel élément de génération de modèles**automatique.</span><span class="sxs-lookup"><span data-stu-id="a7923-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/sca.png)

<span data-ttu-id="a7923-138">Dans la boîte de dialogue **Ajouter une structure** , sélectionnez **Razor pages à l’aide de Entity Framework (CRUD)** > **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="a7923-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/add_scaffold.png)

<span data-ttu-id="a7923-140">Renseignez la boîte de dialogue **Pages Razor avec Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="a7923-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="a7923-141">Dans la liste déroulante **Classe de modèle**, sélectionnez **Film (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="a7923-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="a7923-142">Dans la ligne **Classe du contexte de données**, sélectionnez le signe (plus) **+** et changez le nom généré de RazorPagesMovie.**Models**.RazorPagesMovieContext en RazorPagesMovie.**Data**.RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="a7923-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="a7923-143">[Cette modification](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) n'est pas requise.</span><span class="sxs-lookup"><span data-stu-id="a7923-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="a7923-144">Elle crée la classe de contexte de base de données avec l’espace de noms correct.</span><span class="sxs-lookup"><span data-stu-id="a7923-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="a7923-145">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="a7923-145">Select **Add**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/3/arp.png)

<span data-ttu-id="a7923-147">Le fichier *appsettings.json* est mis à jour avec la chaîne de connexion utilisée pour se connecter à une base de données locale.</span><span class="sxs-lookup"><span data-stu-id="a7923-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="a7923-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a7923-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="a7923-149">Ouvrez une fenêtre de commande dans le répertoire du projet (celui qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="a7923-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="a7923-150">Installez l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="a7923-150">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="a7923-151">**Pour Windows**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="a7923-151">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="a7923-152">**Pour macOS et Linux**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="a7923-152">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  dotnet tool install --global dotnet-aspnet-codegenerator
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="a7923-153">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="a7923-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a7923-154">Créer un dossier *Pages/Movies* :</span><span class="sxs-lookup"><span data-stu-id="a7923-154">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="a7923-155">Cliquez avec le bouton droit sur le dossier *Pages* > **Ajouter** > **nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="a7923-155">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="a7923-156">Nommez le dossier *Movies*.</span><span class="sxs-lookup"><span data-stu-id="a7923-156">Name the folder *Movies*</span></span>

<span data-ttu-id="a7923-157">Cliquez avec le bouton droit sur le dossier *pages/movies* > **Ajouter** > **nouvelle génération de modèles automatique...** .</span><span class="sxs-lookup"><span data-stu-id="a7923-157">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolding...**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/scaMac.png)

<span data-ttu-id="a7923-159">Dans la boîte de dialogue **nouvelle génération de modèles** automatique, sélectionnez **Razor pages à l’aide de Entity Framework (CRUD)** > **suivant**.</span><span class="sxs-lookup"><span data-stu-id="a7923-159">In the **New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Next**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/add_scaffoldMac.png)

<span data-ttu-id="a7923-161">Renseignez la boîte de dialogue **Pages Razor avec Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="a7923-161">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="a7923-162">Dans la liste déroulante **classe de modèle** , sélectionnez ou tapez **Movie (RazorPagesMovie. Models)** .</span><span class="sxs-lookup"><span data-stu-id="a7923-162">In the **Model class** drop down, select, or type, **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="a7923-163">Dans la ligne de la **classe de contexte de données** , tapez le nom de la nouvelle classe, RazorPagesMovie. **Données**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="a7923-163">In the **Data context class** row, type the name for the new class, RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="a7923-164">[Cette modification](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) n'est pas requise.</span><span class="sxs-lookup"><span data-stu-id="a7923-164">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="a7923-165">Elle crée la classe de contexte de base de données avec l’espace de noms correct.</span><span class="sxs-lookup"><span data-stu-id="a7923-165">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="a7923-166">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="a7923-166">Select **Add**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/arpMac.png)

<span data-ttu-id="a7923-168">Le fichier *appsettings.json* est mis à jour avec la chaîne de connexion utilisée pour se connecter à une base de données locale.</span><span class="sxs-lookup"><span data-stu-id="a7923-168">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

### <a name="add-ef-tools"></a><span data-ttu-id="a7923-169">Ajouter des outils EF</span><span class="sxs-lookup"><span data-stu-id="a7923-169">Add EF tools</span></span>

<span data-ttu-id="a7923-170">Exécutez la commande CLI .NET Core suivante :</span><span class="sxs-lookup"><span data-stu-id="a7923-170">Run the following .NET Core CLI command:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef
```

<span data-ttu-id="a7923-171">La commande précédente ajoute les outils de Entity Framework Core pour le CLI .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a7923-171">The preceding command adds the Entity Framework Core Tools for the .NET Core CLI.</span></span>

---

### <a name="files-created"></a><span data-ttu-id="a7923-172">Fichiers créés</span><span class="sxs-lookup"><span data-stu-id="a7923-172">Files created</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a7923-173">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a7923-173">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a7923-174">Le processus de génération de modèles automatique crée et met à jour les fichiers suivants :</span><span class="sxs-lookup"><span data-stu-id="a7923-174">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="a7923-175">*Pages/Movies* : Create, Delete, Details, Edit, Index.</span><span class="sxs-lookup"><span data-stu-id="a7923-175">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="a7923-176">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="a7923-176">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="a7923-177">Mis à jour</span><span class="sxs-lookup"><span data-stu-id="a7923-177">Updated</span></span>

* <span data-ttu-id="a7923-178">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="a7923-178">*Startup.cs*</span></span>

<span data-ttu-id="a7923-179">Les fichiers créés et mis à jour sont expliqués dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="a7923-179">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="a7923-180">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="a7923-180">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a7923-181">Le processus de génération de modèles automatique crée et met à jour les fichiers suivants :</span><span class="sxs-lookup"><span data-stu-id="a7923-181">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="a7923-182">*Pages/Movies* : Create, Delete, Details, Edit, Index.</span><span class="sxs-lookup"><span data-stu-id="a7923-182">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="a7923-183">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="a7923-183">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="a7923-184">Mis à jour</span><span class="sxs-lookup"><span data-stu-id="a7923-184">Updated</span></span>

* <span data-ttu-id="a7923-185">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="a7923-185">*Startup.cs*</span></span>

<span data-ttu-id="a7923-186">Les fichiers créés et mis à jour sont expliqués dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="a7923-186">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="a7923-187">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a7923-187">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a7923-188">Le processus de génération de modèles automatique crée les fichiers suivants :</span><span class="sxs-lookup"><span data-stu-id="a7923-188">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="a7923-189">*Pages/Movies* : Create, Delete, Details, Edit, Index.</span><span class="sxs-lookup"><span data-stu-id="a7923-189">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="a7923-190">Les fichiers créés sont expliqués dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="a7923-190">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="a7923-191">Migration initiale</span><span class="sxs-lookup"><span data-stu-id="a7923-191">Initial migration</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a7923-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a7923-192">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a7923-193">Dans cette section, la console du gestionnaire de package est utilisée pour :</span><span class="sxs-lookup"><span data-stu-id="a7923-193">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="a7923-194">Ajouter une migration initiale</span><span class="sxs-lookup"><span data-stu-id="a7923-194">Add an initial migration.</span></span>
* <span data-ttu-id="a7923-195">Mettez à jour la base de données avec la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="a7923-195">Update the database with the initial migration.</span></span>

<span data-ttu-id="a7923-196">Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet** > **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="a7923-196">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu Console du Gestionnaire de package](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="a7923-198">Dans la console du Gestionnaire de package, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="a7923-198">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-code"></a>[<span data-ttu-id="a7923-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a7923-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="a7923-200">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="a7923-200">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="a7923-201">Les commandes précédentes génèrent l’avertissement suivant : « aucun type n’a été spécifié pour la colonne décimale «Price » sur le type d’entité « Movie ».</span><span class="sxs-lookup"><span data-stu-id="a7923-201">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="a7923-202">Les valeurs sont tronquées en mode silencieux si elles ne sont pas compatibles avec la précision et l’échelle par défaut.</span><span class="sxs-lookup"><span data-stu-id="a7923-202">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="a7923-203">Spécifiez explicitement le type de colonne SQL Server capable d’accueillir toutes les valeurs en utilisant 'HasColumnType()'. »</span><span class="sxs-lookup"><span data-stu-id="a7923-203">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="a7923-204">Vous pouvez ignorer cet avertissement, il sera corrigé dans un prochain tutoriel.</span><span class="sxs-lookup"><span data-stu-id="a7923-204">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="a7923-205">La commande migrations génère du code pour créer le schéma de base de données initial.</span><span class="sxs-lookup"><span data-stu-id="a7923-205">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="a7923-206">Le schéma est basé sur le modèle spécifié dans `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="a7923-206">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="a7923-207">L’argument `InitialCreate` est utilisé pour nommer les migrations.</span><span class="sxs-lookup"><span data-stu-id="a7923-207">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="a7923-208">Vous pouvez utiliser n’importe quel nom, mais par convention, un nom décrivant la migration est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="a7923-208">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="a7923-209">La commande `update` exécute la méthode `Up` dans les migrations qui n’ont pas été appliquées.</span><span class="sxs-lookup"><span data-stu-id="a7923-209">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="a7923-210">Dans ce cas, `update` exécute la méthode `Up` dans les *migrations/\<horodatage > _InitialCreate fichier. cs* , ce qui crée la base de données.</span><span class="sxs-lookup"><span data-stu-id="a7923-210">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a7923-211">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a7923-211">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="a7923-212">Examiner le contexte inscrit avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="a7923-212">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="a7923-213">ASP.NET Core comprend [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a7923-213">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a7923-214">Des services (tels que le contexte de base de données EF Core) sont inscrits avec l’injection de dépendances au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="a7923-214">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="a7923-215">Ces services sont affectés aux composants qui les nécessitent (par exemple les Pages Razor) par le biais de paramètres de constructeur.</span><span class="sxs-lookup"><span data-stu-id="a7923-215">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="a7923-216">Le code du constructeur qui obtient une instance de contexte de base de données est indiqué plus loin dans le tutoriel.</span><span class="sxs-lookup"><span data-stu-id="a7923-216">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="a7923-217">L’outil de génération de modèles automatique a créé automatiquement un contexte de base de données et l’a inscrit dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="a7923-217">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="a7923-218">Examinez la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a7923-218">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="a7923-219">La ligne en surbrillance a été ajoutée par l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="a7923-219">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="a7923-220">`RazorPagesMovieContext` coordonne les fonctionnalités d’EF Core (Create, Read, Update, Delete, etc.) pour le modèle `Movie`.</span><span class="sxs-lookup"><span data-stu-id="a7923-220">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="a7923-221">Le contexte de données (`RazorPagesMovieContext`) est dérivé de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="a7923-221">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="a7923-222">Il spécifie les entités qui sont incluses dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="a7923-222">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="a7923-223">Le code précédent crée une propriété [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) pour le jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="a7923-223">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="a7923-224">Dans la terminologie Entity Framework, un jeu d’entités correspond généralement à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="a7923-224">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="a7923-225">Une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="a7923-225">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="a7923-226">Le nom de la chaîne de connexion est transmis au contexte en appelant une méthode sur un objet [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="a7923-226">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="a7923-227">Pour le développement local, le [système de configuration ASP.NET Core](xref:fundamentals/configuration/index) lit la chaîne de connexion à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a7923-227">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="a7923-228">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a7923-228">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a7923-229">Examinez la méthode `Up`.</span><span class="sxs-lookup"><span data-stu-id="a7923-229">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="a7923-230">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="a7923-230">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a7923-231">Examinez la méthode `Up`.</span><span class="sxs-lookup"><span data-stu-id="a7923-231">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="a7923-232">Test de l'application</span><span class="sxs-lookup"><span data-stu-id="a7923-232">Test the app</span></span>

* <span data-ttu-id="a7923-233">Exécutez l’application et ajoutez `/Movies` à l’URL dans le navigateur (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="a7923-233">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="a7923-234">Si vous obtenez l’erreur :</span><span class="sxs-lookup"><span data-stu-id="a7923-234">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="a7923-235">Vous avez manqué [l’étape des migrations](#pmc).</span><span class="sxs-lookup"><span data-stu-id="a7923-235">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="a7923-236">Testez le lien **Créer**.</span><span class="sxs-lookup"><span data-stu-id="a7923-236">Test the **Create** link.</span></span>

  ![Create page](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="a7923-238">Vous ne pourrez peut-être pas entrer de virgules décimales dans le champ `Price`.</span><span class="sxs-lookup"><span data-stu-id="a7923-238">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="a7923-239">Pour prendre en charge la [validation jQuery](https://jqueryvalidation.org/) pour les paramètres régionaux autres que « Anglais » qui utilisent une virgule (« , ») comme décimale et des formats de date autres que le format « Anglais (États-Unis »), l’application doit être localisée.</span><span class="sxs-lookup"><span data-stu-id="a7923-239">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="a7923-240">Pour obtenir des instructions sur la localisation, consultez [ce problème GitHub](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="a7923-240">For globalization instructions, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="a7923-241">Testez les liens **Edit**, **Details** et **Delete**.</span><span class="sxs-lookup"><span data-stu-id="a7923-241">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="a7923-242">Le prochain didacticiel décrit les fichiers créés par la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="a7923-242">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a7923-243">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="a7923-243">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a7923-244">[Précédent : Bien démarrer](xref:tutorials/razor-pages/razor-pages-start)
> [Suivant : Pages Razor obtenues par génération de modèles automatique](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="a7923-244">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a7923-245">Dans cette section, des classes sont ajoutées pour la gestion des films dans une [base de données SQLite](https://www.sqlite.org/index.html)multiplateforme.</span><span class="sxs-lookup"><span data-stu-id="a7923-245">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="a7923-246">Les applications créées à partir d’un modèle de ASP.NET Core utilisent une base de données SQLite.</span><span class="sxs-lookup"><span data-stu-id="a7923-246">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="a7923-247">Les classes de modèle de l’application sont utilisées avec [Entity Framework Core (EF Core)](/ef/core) ([fournisseur de base de données SQLite EF Core](/ef/core/providers/sqlite)) pour fonctionner avec la base de données.</span><span class="sxs-lookup"><span data-stu-id="a7923-247">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="a7923-248">EF Core est un framework de mappage relationnel d’objets qui simplifie l’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="a7923-248">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="a7923-249">Les classes de modèle portent le nom de classes OCT (« Objet CLR Traditionnel »), car elles n’ont pas de dépendances envers EF Core.</span><span class="sxs-lookup"><span data-stu-id="a7923-249">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="a7923-250">Elles définissent les propriétés des données stockées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="a7923-250">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="a7923-251">Ajouter un modèle de données</span><span class="sxs-lookup"><span data-stu-id="a7923-251">Add a data model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a7923-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a7923-252">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a7923-253">Cliquez avec le bouton droit sur le projet **RazorPagesMovie** > **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="a7923-253">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="a7923-254">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="a7923-254">Name the folder *Models*.</span></span>

<span data-ttu-id="a7923-255">Cliquez avec le bouton droit sur le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="a7923-255">Right click the *Models* folder.</span></span> <span data-ttu-id="a7923-256">Sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="a7923-256">Select **Add** > **Class**.</span></span> <span data-ttu-id="a7923-257">Nommez la classe **Movie**.</span><span class="sxs-lookup"><span data-stu-id="a7923-257">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="a7923-258">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a7923-258">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a7923-259">Ajoutez un dossier nommé *Models*.</span><span class="sxs-lookup"><span data-stu-id="a7923-259">Add a folder named *Models*.</span></span>
* <span data-ttu-id="a7923-260">Ajoutez une classe au dossier *Modèles* nommé *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="a7923-260">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="a7923-261">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="a7923-261">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a7923-262">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **RazorPagesMovie**, puis sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="a7923-262">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="a7923-263">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="a7923-263">Name the folder *Models*.</span></span>
* <span data-ttu-id="a7923-264">Cliquez avec le bouton droit sur le dossier *Models* , puis sélectionnez **Ajouter** > **nouveau fichier**.</span><span class="sxs-lookup"><span data-stu-id="a7923-264">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="a7923-265">Dans la boîte de dialogue **Nouveau fichier** :</span><span class="sxs-lookup"><span data-stu-id="a7923-265">In the **New File** dialog:</span></span>

  * <span data-ttu-id="a7923-266">Dans le volet gauche, sélectionnez **Général**.</span><span class="sxs-lookup"><span data-stu-id="a7923-266">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="a7923-267">Dans le volet central, sélectionnez **Classe vide**.</span><span class="sxs-lookup"><span data-stu-id="a7923-267">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="a7923-268">Nommez la classe **Movie**, puis sélectionnez **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="a7923-268">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="a7923-269">Générez le projet pour vérifier qu’il n’y a pas d’erreur de compilation.</span><span class="sxs-lookup"><span data-stu-id="a7923-269">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="a7923-270">Générer automatiquement le modèle de film</span><span class="sxs-lookup"><span data-stu-id="a7923-270">Scaffold the movie model</span></span>

<span data-ttu-id="a7923-271">Dans cette section, le modèle de film est généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="a7923-271">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="a7923-272">Autrement dit, l’outil de génération de modèles automatique génère des pages pour les opérations de création, de lecture, de mise à jour et de suppression (CRUD) pour le modèle de film.</span><span class="sxs-lookup"><span data-stu-id="a7923-272">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a7923-273">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a7923-273">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a7923-274">Créer un dossier *Pages/Movies* :</span><span class="sxs-lookup"><span data-stu-id="a7923-274">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="a7923-275">Cliquez avec le bouton droit sur le dossier *Pages* > **Ajouter** > **nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="a7923-275">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="a7923-276">Nommez le dossier *Movies*.</span><span class="sxs-lookup"><span data-stu-id="a7923-276">Name the folder *Movies*</span></span>

<span data-ttu-id="a7923-277">Cliquez avec le bouton droit sur le dossier *pages/movies* > **Ajoutez** > **nouvel élément de génération de modèles**automatique.</span><span class="sxs-lookup"><span data-stu-id="a7923-277">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/sca.png)

<span data-ttu-id="a7923-279">Dans la boîte de dialogue **Ajouter une structure** , sélectionnez **Razor pages à l’aide de Entity Framework (CRUD)** > **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="a7923-279">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/add_scaffold.png)

<span data-ttu-id="a7923-281">Renseignez la boîte de dialogue **Pages Razor avec Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="a7923-281">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="a7923-282">Dans la liste déroulante **Classe de modèle**, sélectionnez **Film (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="a7923-282">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="a7923-283">Dans la ligne **Classe du contexte de données**, sélectionnez le signe (plus) **+** et acceptez le nom généré **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="a7923-283">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="a7923-284">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="a7923-284">Select **Add**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/arp.png)

<span data-ttu-id="a7923-286">Le fichier *appsettings.json* est mis à jour avec la chaîne de connexion utilisée pour se connecter à une base de données locale.</span><span class="sxs-lookup"><span data-stu-id="a7923-286">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="a7923-287">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a7923-287">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="a7923-288">Ouvrez une fenêtre de commande dans le répertoire du projet (celui qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="a7923-288">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="a7923-289">**Pour Windows**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="a7923-289">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="a7923-290">**Pour macOS et Linux**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="a7923-290">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="a7923-291">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="a7923-291">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a7923-292">Créer un dossier *Pages/Movies* :</span><span class="sxs-lookup"><span data-stu-id="a7923-292">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="a7923-293">Cliquez avec le bouton droit sur le dossier *Pages* > **Ajouter** > **nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="a7923-293">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="a7923-294">Nommez le dossier *Movies*.</span><span class="sxs-lookup"><span data-stu-id="a7923-294">Name the folder *Movies*</span></span>

<span data-ttu-id="a7923-295">Cliquez avec le bouton droit sur le dossier *pages/movies* > **Ajoutez** > **nouvel élément de génération de modèles**automatique.</span><span class="sxs-lookup"><span data-stu-id="a7923-295">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/scaMac.png)

<span data-ttu-id="a7923-297">Dans la boîte de dialogue **Ajouter une nouvelle génération de modèles** automatique, sélectionnez **Razor pages à l’aide de Entity Framework (CRUD)** > **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="a7923-297">In the **Add New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/add_scaffoldMac.png)

<span data-ttu-id="a7923-299">Renseignez la boîte de dialogue **Pages Razor avec Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="a7923-299">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="a7923-300">Dans la liste déroulante **classe de modèle** , sélectionnez ou tapez **Movie**.</span><span class="sxs-lookup"><span data-stu-id="a7923-300">In the **Model class** drop down, select or type **Movie**.</span></span>
* <span data-ttu-id="a7923-301">Dans la ligne de la **classe de contexte de données** , tapez Select the **RazorPagesMovieContext** This crée une nouvelle classe de contexte de base de données avec l’espace de noms correct.</span><span class="sxs-lookup"><span data-stu-id="a7923-301">In the **Data context class** row, type select the **RazorPagesMovieContext** this will create a new db context class with the correct namespace.</span></span> <span data-ttu-id="a7923-302">Dans ce cas, il s’agit de **RazorPagesMovie. Models. RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="a7923-302">In this case it will be  **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="a7923-303">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="a7923-303">Select **Add**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/arpMac.png)

<span data-ttu-id="a7923-305">Le fichier *appsettings.json* est mis à jour avec la chaîne de connexion utilisée pour se connecter à une base de données locale.</span><span class="sxs-lookup"><span data-stu-id="a7923-305">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

---

<span data-ttu-id="a7923-306">Le processus de génération de modèles automatique crée et met à jour les fichiers suivants :</span><span class="sxs-lookup"><span data-stu-id="a7923-306">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="a7923-307">Fichiers créés</span><span class="sxs-lookup"><span data-stu-id="a7923-307">Files created</span></span>

* <span data-ttu-id="a7923-308">*Pages/Movies* : Create, Delete, Details, Edit, Index.</span><span class="sxs-lookup"><span data-stu-id="a7923-308">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="a7923-309">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="a7923-309">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="a7923-310">Fichier mis à jour</span><span class="sxs-lookup"><span data-stu-id="a7923-310">File updated</span></span>

* <span data-ttu-id="a7923-311">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="a7923-311">*Startup.cs*</span></span>

<span data-ttu-id="a7923-312">Les fichiers créés et mis à jour sont expliqués dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="a7923-312">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="a7923-313">Migration initiale</span><span class="sxs-lookup"><span data-stu-id="a7923-313">Initial migration</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a7923-314">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a7923-314">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a7923-315">Dans cette section, la console du gestionnaire de package est utilisée pour :</span><span class="sxs-lookup"><span data-stu-id="a7923-315">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="a7923-316">Ajouter une migration initiale</span><span class="sxs-lookup"><span data-stu-id="a7923-316">Add an initial migration.</span></span>
* <span data-ttu-id="a7923-317">Mettez à jour la base de données avec la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="a7923-317">Update the database with the initial migration.</span></span>

<span data-ttu-id="a7923-318">Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet** > **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="a7923-318">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu Console du Gestionnaire de package](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="a7923-320">Dans la console du Gestionnaire de package, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="a7923-320">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="a7923-321">La commande `Add-Migration` génère le code nécessaire à la création du schéma de base de données initial.</span><span class="sxs-lookup"><span data-stu-id="a7923-321">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="a7923-322">Le schéma est basé sur le modèle spécifié dans `DbContext` (dans le fichier *RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="a7923-322">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="a7923-323">L’argument `InitialCreate` est utilisé pour nommer la migration.</span><span class="sxs-lookup"><span data-stu-id="a7923-323">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="a7923-324">Vous pouvez utiliser n’importe quel nom, mais par convention, un nom décrivant la migration est utilisé.</span><span class="sxs-lookup"><span data-stu-id="a7923-324">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="a7923-325">Pour plus d’informations, consultez <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="a7923-325">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="a7923-326">La commande `Update-Database` exécute la méthode `Up` dans le fichier *Migrations/\<horodatage>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="a7923-326">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="a7923-327">La méthode `Up` crée la base de données.</span><span class="sxs-lookup"><span data-stu-id="a7923-327">The `Up` method creates the database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="a7923-328">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a7923-328">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="a7923-329">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="a7923-329">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="a7923-330">Les commandes précédentes génèrent l’avertissement suivant : «*aucun type n’a été spécifié pour la colonne décimale «Price » sur le type d’entité « Movie ». Les valeurs sont tronquées en mode silencieux si elles ne tiennent pas dans la précision et l’échelle par défaut. Spécifiez explicitement le type de colonne SQL Server qui peut prendre en charge toutes les valeurs à l’aide de’HasColumnType () '.* Vous pouvez ignorer cet avertissement. il sera corrigé dans un didacticiel ultérieur.</span><span class="sxs-lookup"><span data-stu-id="a7923-330">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a7923-331">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a7923-331">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="a7923-332">Examiner le contexte inscrit avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="a7923-332">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="a7923-333">ASP.NET Core comprend [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a7923-333">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a7923-334">Des services (tels que le contexte de base de données EF Core) sont inscrits avec l’injection de dépendances au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="a7923-334">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="a7923-335">Ces services sont affectés aux composants qui les nécessitent (par exemple les Pages Razor) par le biais de paramètres de constructeur.</span><span class="sxs-lookup"><span data-stu-id="a7923-335">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="a7923-336">Le code du constructeur qui obtient une instance de contexte de base de données est indiqué plus loin dans le tutoriel.</span><span class="sxs-lookup"><span data-stu-id="a7923-336">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="a7923-337">L’outil de génération de modèles automatique a créé automatiquement un contexte de base de données et l’a inscrit dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="a7923-337">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="a7923-338">Examinez la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a7923-338">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="a7923-339">La ligne en surbrillance a été ajoutée par l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="a7923-339">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="a7923-340">`RazorPagesMovieContext` coordonne les fonctionnalités d’EF Core (Create, Read, Update, Delete, etc.) pour le modèle `Movie`.</span><span class="sxs-lookup"><span data-stu-id="a7923-340">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="a7923-341">Le contexte de données (`RazorPagesMovieContext`) est dérivé de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="a7923-341">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="a7923-342">Il spécifie les entités qui sont incluses dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="a7923-342">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="a7923-343">Le code précédent crée une propriété [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) pour le jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="a7923-343">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="a7923-344">Dans la terminologie Entity Framework, un jeu d’entités correspond généralement à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="a7923-344">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="a7923-345">Une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="a7923-345">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="a7923-346">Le nom de la chaîne de connexion est transmis au contexte en appelant une méthode sur un objet [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="a7923-346">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="a7923-347">Pour le développement local, le [système de configuration ASP.NET Core](xref:fundamentals/configuration/index) lit la chaîne de connexion à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a7923-347">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="a7923-348">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a7923-348">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a7923-349">Examinez la méthode `Up`.</span><span class="sxs-lookup"><span data-stu-id="a7923-349">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="a7923-350">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="a7923-350">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a7923-351">Examinez la méthode `Up`.</span><span class="sxs-lookup"><span data-stu-id="a7923-351">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="a7923-352">Test de l'application</span><span class="sxs-lookup"><span data-stu-id="a7923-352">Test the app</span></span>

* <span data-ttu-id="a7923-353">Exécutez l’application et ajoutez `/Movies` à l’URL dans le navigateur (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="a7923-353">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="a7923-354">Si vous obtenez l’erreur :</span><span class="sxs-lookup"><span data-stu-id="a7923-354">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="a7923-355">Vous avez manqué [l’étape des migrations](#pmc).</span><span class="sxs-lookup"><span data-stu-id="a7923-355">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="a7923-356">Testez le lien **Créer**.</span><span class="sxs-lookup"><span data-stu-id="a7923-356">Test the **Create** link.</span></span>

  ![Create page](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="a7923-358">Vous ne pourrez peut-être pas entrer de virgules décimales dans le champ `Price`.</span><span class="sxs-lookup"><span data-stu-id="a7923-358">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="a7923-359">Pour prendre en charge la [validation jQuery](https://jqueryvalidation.org/) pour les paramètres régionaux autres que « Anglais » qui utilisent une virgule (« , ») comme décimale et des formats de date autres que le format « Anglais (États-Unis »), l’application doit être localisée.</span><span class="sxs-lookup"><span data-stu-id="a7923-359">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="a7923-360">Pour obtenir des instructions sur la localisation, consultez [ce problème GitHub](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="a7923-360">For globalization instructions, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="a7923-361">Testez les liens **Edit**, **Details** et **Delete**.</span><span class="sxs-lookup"><span data-stu-id="a7923-361">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="a7923-362">Le prochain didacticiel décrit les fichiers créés par la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="a7923-362">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a7923-363">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="a7923-363">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a7923-364">[Précédent : Bien démarrer](xref:tutorials/razor-pages/razor-pages-start)
> [Suivant : Pages Razor obtenues par génération de modèles automatique](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="a7923-364">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
