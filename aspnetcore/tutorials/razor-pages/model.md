---
title: Ajouter un modèle à une application de pages Razor dans ASP.NET Core
author: rick-anderson
description: Découvrez comment ajouter des classes pour gérer des films dans une base de données à l’aide d’Entity Framework Core (EF Core).
ms.author: riande
ms.date: 07/22/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: 18cf9aea930a7989bb844bc6c40dfa1ce84b7b4d
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082600"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="fad1f-103">Ajouter un modèle à une application de pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fad1f-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="fad1f-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fad1f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fad1f-105">Dans cette section, des classes sont ajoutées pour la gestion des films dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="fad1f-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="fad1f-106">Ces classes sont utilisées avec [Entity Framework Core](/ef/core) (EF Core) pour utiliser une base de données.</span><span class="sxs-lookup"><span data-stu-id="fad1f-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="fad1f-107">EF Core est un framework de mappage relationnel d’objets qui simplifie l’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="fad1f-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="fad1f-108">Les classes de modèle portent le nom de classes OCT (« Objet CLR Traditionnel »), car elles n’ont pas de dépendances envers EF Core.</span><span class="sxs-lookup"><span data-stu-id="fad1f-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="fad1f-109">Elles définissent les propriétés des données stockées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="fad1f-109">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="fad1f-110">Ajouter un modèle de données</span><span class="sxs-lookup"><span data-stu-id="fad1f-110">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fad1f-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fad1f-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fad1f-112">Cliquez avec le bouton droit sur le projet **RazorPagesMovie** > **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-112">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="fad1f-113">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="fad1f-113">Name the folder *Models*.</span></span>

<span data-ttu-id="fad1f-114">Cliquez avec le bouton droit sur le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="fad1f-114">Right click the *Models* folder.</span></span> <span data-ttu-id="fad1f-115">Sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-115">Select **Add** > **Class**.</span></span> <span data-ttu-id="fad1f-116">Nommez la classe **Movie**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-116">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fad1f-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fad1f-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="fad1f-118">Ajoutez un dossier nommé *Models*.</span><span class="sxs-lookup"><span data-stu-id="fad1f-118">Add a folder named *Models*.</span></span>
* <span data-ttu-id="fad1f-119">Ajoutez une classe au dossier *Modèles* nommé *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="fad1f-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fad1f-120">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="fad1f-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="fad1f-121">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **RazorPagesMovie**, puis sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-121">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="fad1f-122">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="fad1f-122">Name the folder *Models*.</span></span>
* <span data-ttu-id="fad1f-123">Cliquez avec le bouton droit sur le dossier *Modèles*, puis sélectionnez **Ajouter** > **Nouveau fichier**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-123">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="fad1f-124">Dans la boîte de dialogue **Nouveau fichier** :</span><span class="sxs-lookup"><span data-stu-id="fad1f-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="fad1f-125">Dans le volet gauche, sélectionnez **Général**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="fad1f-126">Dans le volet central, sélectionnez **Classe vide**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="fad1f-127">Nommez la classe **Movie**, puis sélectionnez **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="fad1f-128">Générez le projet pour vérifier qu’il n’y a pas d’erreur de compilation.</span><span class="sxs-lookup"><span data-stu-id="fad1f-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="fad1f-129">Générer automatiquement le modèle de film</span><span class="sxs-lookup"><span data-stu-id="fad1f-129">Scaffold the movie model</span></span>

<span data-ttu-id="fad1f-130">Dans cette section, le modèle de film est généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="fad1f-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="fad1f-131">Autrement dit, l’outil de génération de modèles automatique génère des pages pour les opérations de création, de lecture, de mise à jour et de suppression (CRUD) pour le modèle de film.</span><span class="sxs-lookup"><span data-stu-id="fad1f-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fad1f-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fad1f-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fad1f-133">Créer un dossier *Pages/Movies* :</span><span class="sxs-lookup"><span data-stu-id="fad1f-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="fad1f-134">Cliquez avec le bouton droit sur le dossier *Pages* > **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="fad1f-135">Nommez le dossier *Movies*.</span><span class="sxs-lookup"><span data-stu-id="fad1f-135">Name the folder *Movies*</span></span>

<span data-ttu-id="fad1f-136">Cliquez avec le bouton droit sur le dossier *Pages/Movies* > **Ajouter** > **Nouvel élément généré automatiquement**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/sca.png)

<span data-ttu-id="fad1f-138">Dans la boîte de dialogue **Ajouter un modèle automatique**, sélectionnez **Razor Pages avec Entity Framework (CRUD)** > **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/add_scaffold.png)

<span data-ttu-id="fad1f-140">Renseignez la boîte de dialogue **Pages Razor avec Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="fad1f-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="fad1f-141">Dans la liste déroulante **Classe de modèle**, sélectionnez **Film (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="fad1f-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="fad1f-142">Dans la ligne **Classe du contexte de données**, sélectionnez le signe (plus) **+** et changez le nom généré de RazorPagesMovie.**Models**.RazorPagesMovieContext en RazorPagesMovie.**Data**.RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="fad1f-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="fad1f-143">[Cette modification](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) n'est pas requise.</span><span class="sxs-lookup"><span data-stu-id="fad1f-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="fad1f-144">Elle crée la classe de contexte de base de données avec l’espace de noms correct.</span><span class="sxs-lookup"><span data-stu-id="fad1f-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="fad1f-145">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-145">Select **Add**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/3/arp.png)

<span data-ttu-id="fad1f-147">Le fichier *appsettings.json* est mis à jour avec la chaîne de connexion utilisée pour se connecter à une base de données locale.</span><span class="sxs-lookup"><span data-stu-id="fad1f-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fad1f-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fad1f-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="fad1f-149">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="fad1f-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="fad1f-150">Installez l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="fad1f-150">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator --version 3.0.0-*
   ```

* <span data-ttu-id="fad1f-151">**Pour Windows 10** : Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="fad1f-151">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="fad1f-152">**Pour macOS et Linux** : Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="fad1f-152">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fad1f-153">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="fad1f-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="fad1f-154">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="fad1f-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="fad1f-155">Installez l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="fad1f-155">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator --version 3.0.0-*
   ```

* <span data-ttu-id="fad1f-156">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="fad1f-156">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

### <a name="files-created"></a><span data-ttu-id="fad1f-157">Fichiers créés</span><span class="sxs-lookup"><span data-stu-id="fad1f-157">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fad1f-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fad1f-158">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fad1f-159">Le processus de génération de modèles automatique crée et met à jour les fichiers suivants :</span><span class="sxs-lookup"><span data-stu-id="fad1f-159">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="fad1f-160">*Pages/Movies* : Create, Delete, Details, Edit, Index.</span><span class="sxs-lookup"><span data-stu-id="fad1f-160">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="fad1f-161">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="fad1f-161">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="fad1f-162">Mis à jour</span><span class="sxs-lookup"><span data-stu-id="fad1f-162">Updated</span></span>

* <span data-ttu-id="fad1f-163">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="fad1f-163">*Startup.cs*</span></span>

<span data-ttu-id="fad1f-164">Les fichiers créés et mis à jour sont expliqués dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="fad1f-164">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="fad1f-165">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="fad1f-165">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="fad1f-166">Le processus de génération de modèles automatique crée les fichiers suivants :</span><span class="sxs-lookup"><span data-stu-id="fad1f-166">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="fad1f-167">*Pages/Movies* : Create, Delete, Details, Edit, Index.</span><span class="sxs-lookup"><span data-stu-id="fad1f-167">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="fad1f-168">Les fichiers créés sont expliqués dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="fad1f-168">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="fad1f-169">Migration initiale</span><span class="sxs-lookup"><span data-stu-id="fad1f-169">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fad1f-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fad1f-170">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fad1f-171">Dans cette section, la console du gestionnaire de package est utilisée pour :</span><span class="sxs-lookup"><span data-stu-id="fad1f-171">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="fad1f-172">Ajouter une migration initiale</span><span class="sxs-lookup"><span data-stu-id="fad1f-172">Add an initial migration.</span></span>
* <span data-ttu-id="fad1f-173">Mettez à jour la base de données avec la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="fad1f-173">Update the database with the initial migration.</span></span>

<span data-ttu-id="fad1f-174">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** > **Console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-174">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu Console du Gestionnaire de package](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="fad1f-176">Dans la console du Gestionnaire de package, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="fad1f-176">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fad1f-177">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fad1f-177">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fad1f-178">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="fad1f-178">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="fad1f-179">Les commandes précédentes génèrent l’avertissement suivant : « Aucun type n’a été spécifié pour la colonne décimale ‘Price’ sur le type d’entité ‘Movie’.</span><span class="sxs-lookup"><span data-stu-id="fad1f-179">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="fad1f-180">Les valeurs sont tronquées en mode silencieux si elles ne sont pas compatibles avec la précision et l’échelle par défaut.</span><span class="sxs-lookup"><span data-stu-id="fad1f-180">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="fad1f-181">Spécifiez explicitement le type de colonne SQL Server capable d’accueillir toutes les valeurs en utilisant 'HasColumnType()'. »</span><span class="sxs-lookup"><span data-stu-id="fad1f-181">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="fad1f-182">Vous pouvez ignorer cet avertissement, il sera corrigé dans un prochain tutoriel.</span><span class="sxs-lookup"><span data-stu-id="fad1f-182">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="fad1f-183">La commande `ef migrations add InitialCreate` génère le code nécessaire à la création du schéma de base de données initial.</span><span class="sxs-lookup"><span data-stu-id="fad1f-183">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="fad1f-184">Le schéma est basé sur le modèle spécifié dans `DbContext` (dans le fichier *RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="fad1f-184">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="fad1f-185">L’argument `InitialCreate` est utilisé pour nommer les migrations.</span><span class="sxs-lookup"><span data-stu-id="fad1f-185">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="fad1f-186">Vous pouvez utiliser n’importe quel nom, mais par convention, un nom décrivant la migration est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="fad1f-186">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="fad1f-187">La commande `ef database update` exécute la méthode `Up` dans le fichier *Migrations/\<horodatage>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="fad1f-187">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="fad1f-188">La méthode `Up` crée la base de données.</span><span class="sxs-lookup"><span data-stu-id="fad1f-188">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fad1f-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fad1f-189">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="fad1f-190">Examiner le contexte inscrit avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="fad1f-190">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="fad1f-191">ASP.NET Core comprend [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="fad1f-191">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="fad1f-192">Des services (tels que le contexte de base de données EF Core) sont inscrits avec l’injection de dépendances au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="fad1f-192">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="fad1f-193">Ces services sont affectés aux composants qui les nécessitent (par exemple les Pages Razor) par le biais de paramètres de constructeur.</span><span class="sxs-lookup"><span data-stu-id="fad1f-193">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="fad1f-194">Le code du constructeur qui obtient une instance de contexte de base de données est indiqué plus loin dans le tutoriel.</span><span class="sxs-lookup"><span data-stu-id="fad1f-194">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="fad1f-195">L’outil de génération de modèles automatique a créé automatiquement un contexte de base de données et l’a inscrit dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="fad1f-195">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="fad1f-196">Examinez la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fad1f-196">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="fad1f-197">La ligne en surbrillance a été ajoutée par l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="fad1f-197">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="fad1f-198">`RazorPagesMovieContext` coordonne les fonctionnalités d’EF Core (Create, Read, Update, Delete, etc.) pour le modèle `Movie`.</span><span class="sxs-lookup"><span data-stu-id="fad1f-198">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="fad1f-199">Le contexte de données (`RazorPagesMovieContext`) est dérivé de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="fad1f-199">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="fad1f-200">Il spécifie les entités qui sont incluses dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="fad1f-200">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="fad1f-201">Le code précédent crée une propriété [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) pour le jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="fad1f-201">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="fad1f-202">Dans la terminologie Entity Framework, un jeu d’entités correspond généralement à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="fad1f-202">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="fad1f-203">Une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="fad1f-203">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="fad1f-204">Le nom de la chaîne de connexion est transmis au contexte en appelant une méthode sur un objet [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="fad1f-204">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="fad1f-205">Pour le développement local, le [système de configuration ASP.NET Core](xref:fundamentals/configuration/index) lit la chaîne de connexion à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="fad1f-205">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fad1f-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fad1f-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="fad1f-207">Examinez la méthode `Up`.</span><span class="sxs-lookup"><span data-stu-id="fad1f-207">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fad1f-208">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="fad1f-208">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="fad1f-209">Examinez la méthode `Up`.</span><span class="sxs-lookup"><span data-stu-id="fad1f-209">Examine the `Up` method.</span></span>

---

<span data-ttu-id="fad1f-210">La commande `Add-Migration` génère le code nécessaire à la création du schéma de base de données initial.</span><span class="sxs-lookup"><span data-stu-id="fad1f-210">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="fad1f-211">Le schéma est basé sur le modèle spécifié dans `RazorPagesMovieContext` (dans le fichier *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="fad1f-211">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="fad1f-212">L’argument `Initial` est utilisé pour nommer les migrations.</span><span class="sxs-lookup"><span data-stu-id="fad1f-212">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="fad1f-213">Vous pouvez utiliser n’importe quel nom, mais par convention, un nom décrivant la migration est utilisé.</span><span class="sxs-lookup"><span data-stu-id="fad1f-213">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="fad1f-214">Pour plus d'informations, consultez <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="fad1f-214">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="fad1f-215">La commande `Update-Database` exécute la méthode `Up` dans le fichier *Migrations/{horodatage}_InitialCreate.cs*, ce qui entraîne la création de la base de données.</span><span class="sxs-lookup"><span data-stu-id="fad1f-215">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="fad1f-216">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="fad1f-216">Test the app</span></span>

* <span data-ttu-id="fad1f-217">Exécutez l’application et ajoutez `/Movies` à l’URL dans le navigateur (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="fad1f-217">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="fad1f-218">Si vous obtenez l’erreur :</span><span class="sxs-lookup"><span data-stu-id="fad1f-218">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="fad1f-219">Vous avez manqué [l’étape des migrations](#pmc).</span><span class="sxs-lookup"><span data-stu-id="fad1f-219">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="fad1f-220">Testez le lien **Créer**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-220">Test the **Create** link.</span></span>

  ![Create page](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="fad1f-222">Vous ne pourrez peut-être pas entrer de virgules décimales dans le champ `Price`.</span><span class="sxs-lookup"><span data-stu-id="fad1f-222">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="fad1f-223">Pour prendre en charge la [validation jQuery](https://jqueryvalidation.org/) pour les paramètres régionaux autres que « Anglais » qui utilisent une virgule (« , ») comme décimale et des formats de date autres que le format « Anglais (États-Unis »), l’application doit être localisée.</span><span class="sxs-lookup"><span data-stu-id="fad1f-223">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="fad1f-224">Pour obtenir des instructions sur la localisation, consultez [ce problème GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="fad1f-224">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="fad1f-225">Testez les liens **Modifier**, **Détails** et **Supprimer**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-225">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="fad1f-226">Le prochain didacticiel décrit les fichiers créés par la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="fad1f-226">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fad1f-227">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="fad1f-227">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fad1f-228">[Précédent : Bien démarrer](xref:tutorials/razor-pages/razor-pages-start)
> [Suivant : Pages Razor obtenues par génération de modèles automatique](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="fad1f-228">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="fad1f-229">Dans cette section, des classes sont ajoutées pour la gestion des films dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="fad1f-229">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="fad1f-230">Ces classes sont utilisées avec [Entity Framework Core](/ef/core) (EF Core) pour utiliser une base de données.</span><span class="sxs-lookup"><span data-stu-id="fad1f-230">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="fad1f-231">EF Core est un framework de mappage relationnel d’objets qui simplifie le code d’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="fad1f-231">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="fad1f-232">Les classes de modèle portent le nom de classes OCT (« Objet CLR Traditionnel »), car elles n’ont pas de dépendances envers EF Core.</span><span class="sxs-lookup"><span data-stu-id="fad1f-232">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="fad1f-233">Elles définissent les propriétés des données stockées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="fad1f-233">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="fad1f-234">Ajouter un modèle de données</span><span class="sxs-lookup"><span data-stu-id="fad1f-234">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fad1f-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fad1f-235">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fad1f-236">Cliquez avec le bouton droit sur le projet **RazorPagesMovie** > **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-236">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="fad1f-237">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="fad1f-237">Name the folder *Models*.</span></span>

<span data-ttu-id="fad1f-238">Cliquez avec le bouton droit sur le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="fad1f-238">Right click the *Models* folder.</span></span> <span data-ttu-id="fad1f-239">Sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-239">Select **Add** > **Class**.</span></span> <span data-ttu-id="fad1f-240">Nommez la classe **Movie**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-240">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fad1f-241">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fad1f-241">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="fad1f-242">Ajoutez un dossier nommé *Models*.</span><span class="sxs-lookup"><span data-stu-id="fad1f-242">Add a folder named *Models*.</span></span>
* <span data-ttu-id="fad1f-243">Ajoutez une classe au dossier *Modèles* nommé *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="fad1f-243">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fad1f-244">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="fad1f-244">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="fad1f-245">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **RazorPagesMovie**, puis sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-245">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="fad1f-246">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="fad1f-246">Name the folder *Models*.</span></span>
* <span data-ttu-id="fad1f-247">Cliquez avec le bouton droit sur le dossier *Modèles*, puis sélectionnez **Ajouter** > **Nouveau fichier**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-247">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="fad1f-248">Dans la boîte de dialogue **Nouveau fichier** :</span><span class="sxs-lookup"><span data-stu-id="fad1f-248">In the **New File** dialog:</span></span>

  * <span data-ttu-id="fad1f-249">Dans le volet gauche, sélectionnez **Général**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-249">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="fad1f-250">Dans le volet central, sélectionnez **Classe vide**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-250">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="fad1f-251">Nommez la classe **Movie**, puis sélectionnez **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-251">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="fad1f-252">Générez le projet pour vérifier qu’il n’y a pas d’erreur de compilation.</span><span class="sxs-lookup"><span data-stu-id="fad1f-252">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="fad1f-253">Générer automatiquement le modèle de film</span><span class="sxs-lookup"><span data-stu-id="fad1f-253">Scaffold the movie model</span></span>

<span data-ttu-id="fad1f-254">Dans cette section, le modèle de film est généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="fad1f-254">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="fad1f-255">Autrement dit, l’outil de génération de modèles automatique génère des pages pour les opérations de création, de lecture, de mise à jour et de suppression (CRUD) pour le modèle de film.</span><span class="sxs-lookup"><span data-stu-id="fad1f-255">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fad1f-256">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fad1f-256">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fad1f-257">Créer un dossier *Pages/Movies* :</span><span class="sxs-lookup"><span data-stu-id="fad1f-257">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="fad1f-258">Cliquez avec le bouton droit sur le dossier *Pages* > **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-258">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="fad1f-259">Nommez le dossier *Movies*.</span><span class="sxs-lookup"><span data-stu-id="fad1f-259">Name the folder *Movies*</span></span>

<span data-ttu-id="fad1f-260">Cliquez avec le bouton droit sur le dossier *Pages/Movies* > **Ajouter** > **Nouvel élément généré automatiquement**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-260">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/sca.png)

<span data-ttu-id="fad1f-262">Dans la boîte de dialogue **Ajouter un modèle automatique**, sélectionnez **Razor Pages avec Entity Framework (CRUD)** > **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-262">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/add_scaffold.png)

<span data-ttu-id="fad1f-264">Renseignez la boîte de dialogue **Pages Razor avec Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="fad1f-264">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="fad1f-265">Dans la liste déroulante **Classe de modèle**, sélectionnez **Film (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="fad1f-265">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="fad1f-266">Dans la ligne **Classe du contexte de données**, sélectionnez le signe (plus) **+** et acceptez le nom généré **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-266">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="fad1f-267">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-267">Select **Add**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/arp.png)

<span data-ttu-id="fad1f-269">Le fichier *appsettings.json* est mis à jour avec la chaîne de connexion utilisée pour se connecter à une base de données locale.</span><span class="sxs-lookup"><span data-stu-id="fad1f-269">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fad1f-270">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fad1f-270">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="fad1f-271">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="fad1f-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="fad1f-272">Installez l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="fad1f-272">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="fad1f-273">**Pour Windows 10** : Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="fad1f-273">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="fad1f-274">**Pour macOS et Linux** : Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="fad1f-274">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fad1f-275">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="fad1f-275">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="fad1f-276">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="fad1f-276">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="fad1f-277">Installez l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="fad1f-277">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="fad1f-278">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="fad1f-278">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="fad1f-279">Le processus de génération de modèles automatique crée et met à jour les fichiers suivants :</span><span class="sxs-lookup"><span data-stu-id="fad1f-279">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="fad1f-280">Fichiers créés</span><span class="sxs-lookup"><span data-stu-id="fad1f-280">Files created</span></span>

* <span data-ttu-id="fad1f-281">*Pages/Movies* : Create, Delete, Details, Edit, Index.</span><span class="sxs-lookup"><span data-stu-id="fad1f-281">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="fad1f-282">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="fad1f-282">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="fad1f-283">Fichier mis à jour</span><span class="sxs-lookup"><span data-stu-id="fad1f-283">File updated</span></span>

* <span data-ttu-id="fad1f-284">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="fad1f-284">*Startup.cs*</span></span>

<span data-ttu-id="fad1f-285">Les fichiers créés et mis à jour sont expliqués dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="fad1f-285">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="fad1f-286">Migration initiale</span><span class="sxs-lookup"><span data-stu-id="fad1f-286">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fad1f-287">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fad1f-287">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fad1f-288">Dans cette section, la console du gestionnaire de package est utilisée pour :</span><span class="sxs-lookup"><span data-stu-id="fad1f-288">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="fad1f-289">Ajouter une migration initiale</span><span class="sxs-lookup"><span data-stu-id="fad1f-289">Add an initial migration.</span></span>
* <span data-ttu-id="fad1f-290">Mettez à jour la base de données avec la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="fad1f-290">Update the database with the initial migration.</span></span>

<span data-ttu-id="fad1f-291">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** > **Console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-291">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu Console du Gestionnaire de package](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="fad1f-293">Dans la console du Gestionnaire de package, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="fad1f-293">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fad1f-294">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fad1f-294">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fad1f-295">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="fad1f-295">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="fad1f-296">Les commandes précédentes génèrent l’avertissement suivant : « Aucun type n’a été spécifié pour la colonne décimale ‘Price’ sur le type d’entité ‘Movie’.</span><span class="sxs-lookup"><span data-stu-id="fad1f-296">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="fad1f-297">Les valeurs sont tronquées en mode silencieux si elles ne sont pas compatibles avec la précision et l’échelle par défaut.</span><span class="sxs-lookup"><span data-stu-id="fad1f-297">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="fad1f-298">Spécifiez explicitement le type de colonne SQL Server capable d’accueillir toutes les valeurs en utilisant 'HasColumnType()'. »</span><span class="sxs-lookup"><span data-stu-id="fad1f-298">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="fad1f-299">Vous pouvez ignorer cet avertissement, il sera corrigé dans un prochain tutoriel.</span><span class="sxs-lookup"><span data-stu-id="fad1f-299">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="fad1f-300">La commande `ef migrations add InitialCreate` génère le code nécessaire à la création du schéma de base de données initial.</span><span class="sxs-lookup"><span data-stu-id="fad1f-300">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="fad1f-301">Le schéma est basé sur le modèle spécifié dans `DbContext` (dans le fichier *RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="fad1f-301">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="fad1f-302">L’argument `InitialCreate` est utilisé pour nommer les migrations.</span><span class="sxs-lookup"><span data-stu-id="fad1f-302">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="fad1f-303">Vous pouvez utiliser n’importe quel nom, mais par convention, un nom décrivant la migration est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="fad1f-303">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="fad1f-304">La commande `ef database update` exécute la méthode `Up` dans le fichier *Migrations/\<horodatage>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="fad1f-304">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="fad1f-305">La méthode `Up` crée la base de données.</span><span class="sxs-lookup"><span data-stu-id="fad1f-305">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fad1f-306">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fad1f-306">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="fad1f-307">Examiner le contexte inscrit avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="fad1f-307">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="fad1f-308">ASP.NET Core comprend [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="fad1f-308">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="fad1f-309">Des services (tels que le contexte de base de données EF Core) sont inscrits avec l’injection de dépendances au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="fad1f-309">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="fad1f-310">Ces services sont affectés aux composants qui les nécessitent (par exemple les Pages Razor) par le biais de paramètres de constructeur.</span><span class="sxs-lookup"><span data-stu-id="fad1f-310">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="fad1f-311">Le code du constructeur qui obtient une instance de contexte de base de données est indiqué plus loin dans le tutoriel.</span><span class="sxs-lookup"><span data-stu-id="fad1f-311">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="fad1f-312">L’outil de génération de modèles automatique a créé automatiquement un contexte de base de données et l’a inscrit dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="fad1f-312">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="fad1f-313">Examinez la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fad1f-313">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="fad1f-314">La ligne en surbrillance a été ajoutée par l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="fad1f-314">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="fad1f-315">`RazorPagesMovieContext` coordonne les fonctionnalités d’EF Core (Create, Read, Update, Delete, etc.) pour le modèle `Movie`.</span><span class="sxs-lookup"><span data-stu-id="fad1f-315">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="fad1f-316">Le contexte de données (`RazorPagesMovieContext`) est dérivé de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="fad1f-316">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="fad1f-317">Il spécifie les entités qui sont incluses dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="fad1f-317">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="fad1f-318">Le code précédent crée une propriété [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) pour le jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="fad1f-318">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="fad1f-319">Dans la terminologie Entity Framework, un jeu d’entités correspond généralement à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="fad1f-319">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="fad1f-320">Une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="fad1f-320">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="fad1f-321">Le nom de la chaîne de connexion est transmis au contexte en appelant une méthode sur un objet [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="fad1f-321">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="fad1f-322">Pour le développement local, le [système de configuration ASP.NET Core](xref:fundamentals/configuration/index) lit la chaîne de connexion à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="fad1f-322">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fad1f-323">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fad1f-323">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="fad1f-324">Examinez la méthode `Up`.</span><span class="sxs-lookup"><span data-stu-id="fad1f-324">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fad1f-325">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="fad1f-325">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="fad1f-326">Examinez la méthode `Up`.</span><span class="sxs-lookup"><span data-stu-id="fad1f-326">Examine the `Up` method.</span></span>

---

<span data-ttu-id="fad1f-327">La commande `Add-Migration` génère le code nécessaire à la création du schéma de base de données initial.</span><span class="sxs-lookup"><span data-stu-id="fad1f-327">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="fad1f-328">Le schéma est basé sur le modèle spécifié dans `RazorPagesMovieContext` (dans le fichier *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="fad1f-328">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="fad1f-329">L’argument `Initial` est utilisé pour nommer les migrations.</span><span class="sxs-lookup"><span data-stu-id="fad1f-329">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="fad1f-330">Vous pouvez utiliser n’importe quel nom, mais par convention, un nom décrivant la migration est utilisé.</span><span class="sxs-lookup"><span data-stu-id="fad1f-330">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="fad1f-331">Pour plus d'informations, consultez <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="fad1f-331">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="fad1f-332">La commande `Update-Database` exécute la méthode `Up` dans le fichier *Migrations/{horodatage}_InitialCreate.cs*, ce qui entraîne la création de la base de données.</span><span class="sxs-lookup"><span data-stu-id="fad1f-332">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="fad1f-333">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="fad1f-333">Test the app</span></span>

* <span data-ttu-id="fad1f-334">Exécutez l’application et ajoutez `/Movies` à l’URL dans le navigateur (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="fad1f-334">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="fad1f-335">Si vous obtenez l’erreur :</span><span class="sxs-lookup"><span data-stu-id="fad1f-335">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="fad1f-336">Vous avez manqué [l’étape des migrations](#pmc).</span><span class="sxs-lookup"><span data-stu-id="fad1f-336">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="fad1f-337">Testez le lien **Créer**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-337">Test the **Create** link.</span></span>

  ![Create page](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="fad1f-339">Vous ne pourrez peut-être pas entrer de virgules décimales dans le champ `Price`.</span><span class="sxs-lookup"><span data-stu-id="fad1f-339">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="fad1f-340">Pour prendre en charge la [validation jQuery](https://jqueryvalidation.org/) pour les paramètres régionaux autres que « Anglais » qui utilisent une virgule (« , ») comme décimale et des formats de date autres que le format « Anglais (États-Unis »), l’application doit être localisée.</span><span class="sxs-lookup"><span data-stu-id="fad1f-340">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="fad1f-341">Pour obtenir des instructions sur la localisation, consultez [ce problème GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="fad1f-341">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="fad1f-342">Testez les liens **Modifier**, **Détails** et **Supprimer**.</span><span class="sxs-lookup"><span data-stu-id="fad1f-342">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="fad1f-343">Le prochain didacticiel décrit les fichiers créés par la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="fad1f-343">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fad1f-344">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="fad1f-344">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fad1f-345">[Précédent : Bien démarrer](xref:tutorials/razor-pages/razor-pages-start)
> [Suivant : Pages Razor obtenues par génération de modèles automatique](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="fad1f-345">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
