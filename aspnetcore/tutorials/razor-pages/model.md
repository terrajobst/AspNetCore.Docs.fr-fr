---
title: Ajouter un modèle à une application de pages Razor dans ASP.NET Core
author: rick-anderson
description: Découvrez comment ajouter des classes pour gérer des films dans une base de données à l’aide d’Entity Framework Core (EF Core).
ms.author: riande
monikerRange: '>= aspnetcore-2.2'
ms.date: 12/3/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: b9e8ece6da2c25a77ef0396eb22a19efde58563e
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396114"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="c7e9d-103">Ajouter un modèle à une application de pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7e9d-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="c7e9d-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c7e9d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="c7e9d-105">Dans cette section, des classes sont ajoutées pour la gestion des films dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="c7e9d-106">Ces classes sont utilisées avec [Entity Framework Core](/ef/core) (EF Core) pour utiliser une base de données.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="c7e9d-107">EF Core est un framework de mappage relationnel d’objets qui simplifie le code d’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="c7e9d-108">Les classes de modèle portent le nom de classes OCT (« Objet CLR Traditionnel »), car elles n’ont pas de dépendances envers EF Core.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="c7e9d-109">Elles définissent les propriétés des données stockées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-109">They define the properties of the data that are stored in the database.</span></span>

<span data-ttu-id="c7e9d-110">[Affichez ou téléchargez](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start) l’exemple.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-110">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start) sample.</span></span>

## <a name="add-a-data-model"></a><span data-ttu-id="c7e9d-111">Ajouter un modèle de données</span><span class="sxs-lookup"><span data-stu-id="c7e9d-111">Add a data model</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c7e9d-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c7e9d-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c7e9d-113">Cliquez avec le bouton droit sur le projet **RazorPagesMovie** > **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="c7e9d-114">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-114">Name the folder *Models*.</span></span>

<span data-ttu-id="c7e9d-115">Cliquez avec le bouton droit sur le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-115">Right click the *Models* folder.</span></span> <span data-ttu-id="c7e9d-116">Sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="c7e9d-117">Nommez la classe **Movie**.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c7e9d-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c7e9d-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c7e9d-119">Ajoutez un dossier nommé *Models*.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="c7e9d-120">Ajoutez une classe au dossier *Modèles* nommé *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c7e9d-121">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="c7e9d-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c7e9d-122">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **RazorPagesMovie**, puis sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="c7e9d-123">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="c7e9d-124">Cliquez avec le bouton droit sur le dossier *Models*, puis sélectionnez **Ajouter** > **Nouveau fichier**.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="c7e9d-125">Dans la boîte de dialogue **Nouveau fichier** :</span><span class="sxs-lookup"><span data-stu-id="c7e9d-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="c7e9d-126">Dans le volet gauche, sélectionnez **Général**.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="c7e9d-127">Dans le volet central, sélectionnez **Classe vide**.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-127">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="c7e9d-128">Nommez la classe **Movie**, puis sélectionnez **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- End of VS tabs -->

---

<span data-ttu-id="c7e9d-129">Générez le projet pour vérifier qu’il n’y a pas d’erreur de compilation.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="c7e9d-130">Générer automatiquement le modèle de film</span><span class="sxs-lookup"><span data-stu-id="c7e9d-130">Scaffold the movie model</span></span>

<span data-ttu-id="c7e9d-131">Dans cette section, le modèle de film est généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="c7e9d-132">Autrement dit, l’outil de génération de modèles automatique génère des pages pour les opérations de création, de lecture, de mise à jour et de suppression (CRUD) pour le modèle de film.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c7e9d-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c7e9d-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c7e9d-134">Créer un dossier *Pages/Movies* :</span><span class="sxs-lookup"><span data-stu-id="c7e9d-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="c7e9d-135">Cliquez avec le bouton droit sur le dossier *Pages* > **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="c7e9d-136">Nommez le dossier *Movies*.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-136">Name the folder *Movies*</span></span>

<span data-ttu-id="c7e9d-137">Cliquez avec le bouton droit sur le dossier *Pages/Movies* > **Ajouter** > **Nouvel élément généré automatiquement**.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/sca.png)

<span data-ttu-id="c7e9d-139">Dans la boîte de dialogue **Ajouter un modèle automatique**, sélectionnez **Pages Razor avec Entity Framework (CRUD)**  >  **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/add_scaffold.png)

<span data-ttu-id="c7e9d-141">Renseignez la boîte de dialogue **Pages Razor avec Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="c7e9d-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="c7e9d-142">Dans la liste déroulante **Classe de modèle**, sélectionnez **Film (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="c7e9d-143">Dans la ligne **Classe du contexte de données**, sélectionnez le signe (plus) **+** et acceptez le nom généré **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-143">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="c7e9d-144">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-144">Select **Add**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/arp.png)

<span data-ttu-id="c7e9d-146">Le fichier *appsettings.json* est mis à jour avec la chaîne de connexion utilisée pour se connecter à une base de données locale.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-146">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c7e9d-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c7e9d-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="c7e9d-148">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="c7e9d-148">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="c7e9d-149">Installez l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="c7e9d-149">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="c7e9d-150">**Pour Windows 10** : Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="c7e9d-150">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="c7e9d-151">**Pour macOS et Linux** : Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="c7e9d-151">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c7e9d-152">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="c7e9d-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c7e9d-153">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="c7e9d-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="c7e9d-154">Installez l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="c7e9d-154">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```
* <span data-ttu-id="c7e9d-155">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="c7e9d-155">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="c7e9d-156">Les commandes précédentes génèrent l’avertissement suivant : « Aucun type n’a été spécifié pour la colonne décimale ‘Price’ sur le type d’entité ‘Movie’.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-156">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="c7e9d-157">Les valeurs sont tronquées en mode silencieux si elles ne sont pas compatibles avec la précision et l’échelle par défaut.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-157">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="c7e9d-158">Spécifiez explicitement le type de colonne SQL Server capable d’accueillir toutes les valeurs en utilisant 'HasColumnType()'. »</span><span class="sxs-lookup"><span data-stu-id="c7e9d-158">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="c7e9d-159">Vous pouvez ignorer cet avertissement, il sera corrigé dans un prochain tutoriel.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-159">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="c7e9d-160">Le processus de génération de modèles automatique crée et met à jour les fichiers suivants :</span><span class="sxs-lookup"><span data-stu-id="c7e9d-160">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="c7e9d-161">Fichiers créés</span><span class="sxs-lookup"><span data-stu-id="c7e9d-161">Files created</span></span>

* <span data-ttu-id="c7e9d-162">*Pages/Movies* : Create, Delete, Details, Edit, Index.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-162">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="c7e9d-163">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="c7e9d-163">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="c7e9d-164">Fichier mis à jour</span><span class="sxs-lookup"><span data-stu-id="c7e9d-164">File updated</span></span>

* <span data-ttu-id="c7e9d-165">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="c7e9d-165">*Startup.cs*</span></span>

<span data-ttu-id="c7e9d-166">Les fichiers créés et mis à jour sont expliqués dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-166">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="c7e9d-167">Migration initiale</span><span class="sxs-lookup"><span data-stu-id="c7e9d-167">Initial migration</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c7e9d-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c7e9d-168">Visual Studio</span></span>](#tab/visual-studio)

<!-- VS -------------------------->

<span data-ttu-id="c7e9d-169">Dans cette section, la console du gestionnaire de package est utilisée pour :</span><span class="sxs-lookup"><span data-stu-id="c7e9d-169">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="c7e9d-170">Ajouter une migration initiale</span><span class="sxs-lookup"><span data-stu-id="c7e9d-170">Add an initial migration.</span></span>
* <span data-ttu-id="c7e9d-171">Mettez à jour la base de données avec la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-171">Update the database with the initial migration.</span></span>

<span data-ttu-id="c7e9d-172">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** > **Console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-172">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu Console du Gestionnaire de package](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="c7e9d-174">Dans la console du Gestionnaire de package, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="c7e9d-174">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c7e9d-175">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c7e9d-175">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- Mac -------------------------->

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c7e9d-176">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="c7e9d-176">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="c7e9d-177">La commande `ef migrations add InitialCreate` génère le code nécessaire à la création du schéma de base de données initial.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-177">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="c7e9d-178">Le schéma est basé sur le modèle spécifié dans `DbContext` (dans le fichier *RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="c7e9d-178">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="c7e9d-179">L’argument `InitialCreate` est utilisé pour nommer les migrations.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-179">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="c7e9d-180">Vous pouvez utiliser n’importe quel nom, mais par convention, un nom décrivant la migration est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-180">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="c7e9d-181">La commande `ef database update` exécute la méthode `Up` dans le fichier *Migrations/\<horodatage>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-181">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="c7e9d-182">La méthode `Up` crée la base de données.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-182">The `Up` method creates the database.</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c7e9d-183">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c7e9d-183">Visual Studio</span></span>](#tab/visual-studio)

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="c7e9d-184">Examiner le contexte inscrit avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="c7e9d-184">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="c7e9d-185">ASP.NET Core comprend [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c7e9d-185">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c7e9d-186">Des services (tels que le contexte de base de données EF Core) sont inscrits avec l’injection de dépendances au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-186">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="c7e9d-187">Ces services sont affectés aux composants qui les nécessitent (par exemple les Pages Razor) par le biais de paramètres de constructeur.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-187">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="c7e9d-188">Le code du constructeur qui obtient une instance de contexte de base de données est indiqué plus loin dans le tutoriel.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-188">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="c7e9d-189">L’outil de génération de modèles automatique a créé automatiquement un contexte de base de données et l’a inscrit dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-189">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="c7e9d-190">Examinez la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-190">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="c7e9d-191">La ligne en surbrillance a été ajoutée par l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="c7e9d-191">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="c7e9d-192">`RazorPagesMovieContext` coordonne les fonctionnalités d’EF Core (Create, Read, Update, Delete, etc.) pour le modèle `Movie`.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-192">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="c7e9d-193">Le contexte de données (`RazorPagesMovieContext`) est dérivé de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="c7e9d-193">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="c7e9d-194">Il spécifie les entités qui sont incluses dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-194">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="c7e9d-195">Le code précédent crée une propriété [DbSet/\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) pour le jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-195">The preceding code creates a [DbSet/\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="c7e9d-196">Dans la terminologie Entity Framework, un jeu d’entités correspond généralement à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-196">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="c7e9d-197">Une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-197">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="c7e9d-198">Le nom de la chaîne de connexion est transmis au contexte en appelant une méthode sur un objet [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="c7e9d-198">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="c7e9d-199">Pour le développement local, le [système de configuration ASP.NET Core](xref:fundamentals/configuration/index) lit la chaîne de connexion à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-199">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>
<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c7e9d-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c7e9d-200">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c7e9d-201">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="c7e9d-201">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<!-- End of VS tabs -->

---

<span data-ttu-id="c7e9d-202">La commande `Add-Migration` génère le code nécessaire à la création du schéma de base de données initial.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-202">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="c7e9d-203">Le schéma est basé sur le modèle spécifié dans `RazorPagesMovieContext` (dans le fichier *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="c7e9d-203">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="c7e9d-204">L’argument `Initial` est utilisé pour nommer les migrations.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-204">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="c7e9d-205">Vous pouvez utiliser n’importe quel nom, mais par convention, un nom décrivant la migration est utilisé.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-205">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="c7e9d-206">Pour plus d’informations, consultez [Présentation des migrations](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="c7e9d-206">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="c7e9d-207">La commande `Update-Database` exécute la méthode `Up` dans le fichier *Migrations/{horodatage}_InitialCreate.cs*, ce qui entraîne la création de la base de données.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-207">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="c7e9d-208">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="c7e9d-208">Test the app</span></span>

* <span data-ttu-id="c7e9d-209">Exécutez l’application et ajoutez `/Movies` à l’URL dans le navigateur (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="c7e9d-209">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="c7e9d-210">Si vous obtenez l’erreur :</span><span class="sxs-lookup"><span data-stu-id="c7e9d-210">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="c7e9d-211">Vous avez manqué [l’étape des migrations](#pmc).</span><span class="sxs-lookup"><span data-stu-id="c7e9d-211">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="c7e9d-212">Testez le lien **Créer**.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-212">Test the **Create** link.</span></span>

  ![Créer une page](model/_static/conan.png)
  
  > [!NOTE]
  > <span data-ttu-id="c7e9d-214">Vous ne pourrez peut-être pas entrer de virgules décimales dans le champ `Price`.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-214">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="c7e9d-215">Pour prendre en charge la [validation jQuery](https://jqueryvalidation.org/) pour les paramètres régionaux autres que « Anglais » qui utilisent une virgule (« , ») comme décimale et des formats de date autres que le format « Anglais (États-Unis »), l’application doit être localisée.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-215">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="c7e9d-216">Pour obtenir des instructions sur la localisation, consultez [ce problème GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="c7e9d-216">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="c7e9d-217">Testez les liens **Modifier**, **Détails** et **Supprimer**.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-217">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="c7e9d-218">Le prochain didacticiel décrit les fichiers créés par la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="c7e9d-218">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c7e9d-219">[Précédent : Bien démarrer](xref:tutorials/razor-pages/razor-pages-start)
> [Suivant : Pages Razor obtenues par génération de modèles automatique](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="c7e9d-219">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
