---
title: Ajouter un modèle dans une application ASP.NET Core MVC
author: rick-anderson
description: Ajoutez un modèle à une application ASP.NET Core simple.
ms.author: riande
ms.date: 8/15/2019
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 5ad31a2536ad70590eaa767cf20068512241f36b
ms.sourcegitcommit: 14b25156e34c82ed0495b4aff5776ac5b1950b5e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71295473"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="90e4e-103">Ajouter un modèle dans une application ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="90e4e-103">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="90e4e-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Ryan Nowak](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="90e4e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="90e4e-105">Dans cette section, vous allez ajouter des classes pour la gestion des films dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="90e4e-105">In this section, you add classes for managing movies in a database.</span></span> <span data-ttu-id="90e4e-106">Ces classes constituent la partie « **M**odèle » de l’application **M**VC.</span><span class="sxs-lookup"><span data-stu-id="90e4e-106">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="90e4e-107">Vous utilisez ces classes avec [Entity Framework Core](/ef/core) (EF Core) pour travailler avec une base de données.</span><span class="sxs-lookup"><span data-stu-id="90e4e-107">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="90e4e-108">EF Core est un framework de mappage relationnel d’objets qui simplifie le code d’accès aux données à écrire.</span><span class="sxs-lookup"><span data-stu-id="90e4e-108">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span>

<span data-ttu-id="90e4e-109">Les classes de modèle que vous créez portent le nom de classes OCT (« **O**bjet **C**LR **T**raditionnel ») **,** car elles n’ont pas de dépendances envers EF Core.</span><span class="sxs-lookup"><span data-stu-id="90e4e-109">The model classes you create are known as POCO classes (from **P**lain **O**ld **C**LR **O**bjects) because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="90e4e-110">Elles définissent simplement les propriétés des données stockées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="90e4e-110">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="90e4e-111">Dans ce didacticiel, vous écrivez d’abord les classes du modèle, puis EF Core crée la base de données.</span><span class="sxs-lookup"><span data-stu-id="90e4e-111">In this tutorial, you write the model classes first, and EF Core creates the database.</span></span> <span data-ttu-id="90e4e-112">Une autre approche que nous ne décrivons pas ici consiste à générer les classes de modèle à partir d’une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="90e4e-112">An alternate approach not covered here is to generate model classes from an existing database.</span></span> <span data-ttu-id="90e4e-113">Pour plus d’informations sur cette approche, consultez [ASP.NET Core - Base de données existante](/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="90e4e-113">For information about that approach, see [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="90e4e-114">Ajouter une classe de modèle de données</span><span class="sxs-lookup"><span data-stu-id="90e4e-114">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="90e4e-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="90e4e-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="90e4e-116">Cliquez avec le bouton droit sur le dossier *Models* > **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="90e4e-116">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="90e4e-117">Nommez le fichier *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="90e4e-117">Name the file *Movie.cs*.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="90e4e-118">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="90e4e-118">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="90e4e-119">Ajoutez un fichier nommé *Movie.cs* au dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="90e4e-119">Add a file named *Movie.cs* to the *Models* folder.</span></span>

---

<span data-ttu-id="90e4e-120">Mettez le fichier *Movie.cs* à jour avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="90e4e-120">Update the *Movie.cs* file with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Models/Movie.cs)]

<span data-ttu-id="90e4e-121">La classe `Movie` contient un champ `Id`, qui est nécessaire à la base de données pour la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="90e4e-121">The `Movie` class contains an `Id` field, which is required by the database for the primary key.</span></span>

<span data-ttu-id="90e4e-122">L’attribut [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) de `ReleaseDate` spécifie le type de données (`Date`).</span><span class="sxs-lookup"><span data-stu-id="90e4e-122">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute on `ReleaseDate` specifies the type of the data (`Date`).</span></span> <span data-ttu-id="90e4e-123">Avec cet attribut :</span><span class="sxs-lookup"><span data-stu-id="90e4e-123">With this attribute:</span></span>

  * <span data-ttu-id="90e4e-124">L’utilisateur n’est pas obligé d’entrer les informations de temps dans le champ de date.</span><span class="sxs-lookup"><span data-stu-id="90e4e-124">The user is not required to enter time information in the date field.</span></span>
  * <span data-ttu-id="90e4e-125">Seule la date est affichée, pas les informations de temps.</span><span class="sxs-lookup"><span data-stu-id="90e4e-125">Only the date is displayed, not time information.</span></span>

<span data-ttu-id="90e4e-126">Les [DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) sont traitées dans un prochain didacticiel.</span><span class="sxs-lookup"><span data-stu-id="90e4e-126">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.</span></span>

## <a name="add-nuget-packages"></a><span data-ttu-id="90e4e-127">Ajouter des packages NuGet</span><span class="sxs-lookup"><span data-stu-id="90e4e-127">Add NuGet packages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="90e4e-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="90e4e-128">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="90e4e-129">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** > **Console du gestionnaire de package** (PMC).</span><span class="sxs-lookup"><span data-stu-id="90e4e-129">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

![Menu Console du Gestionnaire de package](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="90e4e-131">Dans la console du gestionnaire de package, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="90e4e-131">In the PMC, run the following command:</span></span>

```powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="90e4e-132">La commande précédente ajoute le fournisseur EF Core SQL Server.</span><span class="sxs-lookup"><span data-stu-id="90e4e-132">The preceding command adds the EF Core SQL Server provider.</span></span> <span data-ttu-id="90e4e-133">Le package du fournisseur installe le package EF Core en tant que dépendance.</span><span class="sxs-lookup"><span data-stu-id="90e4e-133">The provider package installs the EF Core package as a dependency.</span></span> <span data-ttu-id="90e4e-134">Plus loin dans ce tutoriel, d’autres packages sont installés automatiquement lors de l’étape de génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="90e4e-134">Additional packages are installed automatically in the scaffolding step later in the tutorial.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="90e4e-135">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="90e4e-135">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

---

<a name="dc"></a>

## <a name="create-a-database-context-class"></a><span data-ttu-id="90e4e-136">Créer une classe de contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="90e4e-136">Create a database context class</span></span>

<span data-ttu-id="90e4e-137">Une classe de contexte de base de données est nécessaire pour coordonner les fonctionnalités d’EF Core (créer, lire, mettre à jour, supprimer) pour le modèle `Movie`.</span><span class="sxs-lookup"><span data-stu-id="90e4e-137">A database context class is needed to coordinate EF Core functionality (Create, Read, Update, Delete) for the `Movie` model.</span></span> <span data-ttu-id="90e4e-138">Le contexte de base de données est dérivé de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) et il spécifie les entités à inclure dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="90e4e-138">The database context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) and specifies the entities to include in the data model.</span></span>

<span data-ttu-id="90e4e-139">Créez un dossier nommé *Data*.</span><span class="sxs-lookup"><span data-stu-id="90e4e-139">Create a *Data* folder.</span></span>

<span data-ttu-id="90e4e-140">Ajoutez un fichier *Data/MvcMovieContext.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="90e4e-140">Add a *Data/MvcMovieContext.cs* file with the following code:</span></span> 

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/zDocOnly/MvcMovieContext.cs?name=snippet)]

<span data-ttu-id="90e4e-141">Le code précédent crée une propriété [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) pour le jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="90e4e-141">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="90e4e-142">Dans la terminologie Entity Framework, un jeu d’entités correspond généralement à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="90e4e-142">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="90e4e-143">Une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="90e4e-143">An entity corresponds to a row in the table.</span></span>

<a name="reg"></a>

## <a name="register-the-database-context"></a><span data-ttu-id="90e4e-144">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="90e4e-144">Register the database context</span></span>

<span data-ttu-id="90e4e-145">ASP.NET Core comprend [l’injection de dépendances (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="90e4e-145">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="90e4e-146">Les services (tels que le contexte de base de données EF Core) doivent être inscrits auprès de l’injection de dépendances au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="90e4e-146">Services (such as the EF Core DB context) must be registered with DI during application startup.</span></span> <span data-ttu-id="90e4e-147">Ces services sont affectés aux composants qui les nécessitent (par exemple les Pages Razor) par le biais de paramètres de constructeur.</span><span class="sxs-lookup"><span data-stu-id="90e4e-147">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="90e4e-148">Le code du constructeur qui obtient une instance de contexte de base de données est indiqué plus loin dans le tutoriel.</span><span class="sxs-lookup"><span data-stu-id="90e4e-148">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span> <span data-ttu-id="90e4e-149">Dans cette section, vous allez inscrire le contexte de base de données auprès du conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="90e4e-149">In this section, you register the database context with the DI container.</span></span>

<span data-ttu-id="90e4e-150">En tête du fichier *Startup.cs*, ajoutez les instructions `using` suivantes :</span><span class="sxs-lookup"><span data-stu-id="90e4e-150">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Data;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="90e4e-151">Ajoutez le code en surbrillance suivant dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="90e4e-151">Add the following highlighted code in `Startup.ConfigureServices`:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="90e4e-152">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="90e4e-152">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="90e4e-153">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="90e4e-153">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

---

<span data-ttu-id="90e4e-154">Le nom de la chaîne de connexion est transmis au contexte en appelant une méthode sur un objet [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="90e4e-154">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="90e4e-155">Pour le développement local, le [système de configuration ASP.NET Core](xref:fundamentals/configuration/index) lit la chaîne de connexion à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="90e4e-155">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="cs"></a>

## <a name="add-a-database-connection-string"></a><span data-ttu-id="90e4e-156">Ajouter une chaîne de connexion de base de données</span><span class="sxs-lookup"><span data-stu-id="90e4e-156">Add a database connection string</span></span>

<span data-ttu-id="90e4e-157">Ajoutez une chaîne de connexion au fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="90e4e-157">Add a connection string to the *appsettings.json* file:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="90e4e-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="90e4e-158">Visual Studio</span></span>](#tab/visual-studio)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings.json?highlight=10-12)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="90e4e-159">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="90e4e-159">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings_SQLite.json?highlight=10-12)]

---

<span data-ttu-id="90e4e-160">Générez le projet en tant que vérification des erreurs du compilateur.</span><span class="sxs-lookup"><span data-stu-id="90e4e-160">Build the project as a check for compiler errors.</span></span>

## <a name="scaffold-movie-pages"></a><span data-ttu-id="90e4e-161">Générer automatiquement des pages de film</span><span class="sxs-lookup"><span data-stu-id="90e4e-161">Scaffold movie pages</span></span>

<span data-ttu-id="90e4e-162">Utilisez l’outil de génération de modèles automatique pour créer, lire, mettre à jour et supprimer des pages du modèle de film.</span><span class="sxs-lookup"><span data-stu-id="90e4e-162">Use the scaffolding tool to produce Create, Read, Update, and Delete (CRUD) pages for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="90e4e-163">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="90e4e-163">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="90e4e-164">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *Contrôleurs*, puis choisissez **Ajouter > Nouvel élément généré automatiquement**.</span><span class="sxs-lookup"><span data-stu-id="90e4e-164">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![affichage de l’étape ci-dessus](adding-model/_static/add_controller21.png)

<span data-ttu-id="90e4e-166">Dans la boîte de dialogue **Ajouter un modèle automatique**, sélectionnez **Contrôleur MVC avec vues, utilisant Entity Framework > Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="90e4e-166">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Boîte de dialogue Ajouter un modèle automatique](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="90e4e-168">Renseignez la boîte de dialogue **Ajouter un contrôleur** :</span><span class="sxs-lookup"><span data-stu-id="90e4e-168">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="90e4e-169">**Classe du modèle :** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="90e4e-169">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="90e4e-170">**Classe de contexte de données :** *MvcMovieContext (MvcMovie.Data)*</span><span class="sxs-lookup"><span data-stu-id="90e4e-170">**Data context class:** *MvcMovieContext (MvcMovie.Data)*</span></span>

![Ajouter un contexte de données](adding-model/_static/dc3.png)

* <span data-ttu-id="90e4e-172">**Affichages :** conservez la valeur par défaut de chaque option activée</span><span class="sxs-lookup"><span data-stu-id="90e4e-172">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="90e4e-173">**Nom du contrôleur :** conservez la valeur par défaut *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="90e4e-173">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="90e4e-174">Sélectionnez **Ajouter**</span><span class="sxs-lookup"><span data-stu-id="90e4e-174">Select **Add**</span></span>

<span data-ttu-id="90e4e-175">Visual Studio crée :</span><span class="sxs-lookup"><span data-stu-id="90e4e-175">Visual Studio creates:</span></span>

* <span data-ttu-id="90e4e-176">Un contrôleur de films (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="90e4e-176">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="90e4e-177">Des fichiers de vues Razor pour les pages Create, Delete, Details, Edit et Index (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="90e4e-177">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="90e4e-178">La création automatique de ces fichiers est appelée *génération de modèles automatique*.</span><span class="sxs-lookup"><span data-stu-id="90e4e-178">The automatic creation of these files is known as *scaffolding*.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="90e4e-179">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="90e4e-179">Visual Studio Code</span></span>](#tab/visual-studio-code) 

* <span data-ttu-id="90e4e-180">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="90e4e-180">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="90e4e-181">Sur Linux, exportez le chemin de l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="90e4e-181">On Linux, export the scaffold tool path:</span></span>

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="90e4e-182">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="90e4e-182">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="90e4e-183">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="90e4e-183">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="90e4e-184">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="90e4e-184">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="90e4e-185">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="90e4e-185">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of tabs                  -->

<span data-ttu-id="90e4e-186">Vous ne pouvez pas encore utiliser les pages générées automatiquement, car la base de données n’existe pas.</span><span class="sxs-lookup"><span data-stu-id="90e4e-186">You can't use the scaffolded pages yet because the database doesn't exist.</span></span> <span data-ttu-id="90e4e-187">Si vous exécutez l’application et cliquez sur le lien **Movie App**, vous recevez un message d’erreur du type : *Impossible d’ouvrir la base de données* ou *La table suivante n’existe pas : Movie*.</span><span class="sxs-lookup"><span data-stu-id="90e4e-187">If you run the app and click on the **Movie App** link, you get a *Cannot open database* or *no such table: Movie* error message.</span></span>

<a name="migration"></a>

## <a name="initial-migration"></a><span data-ttu-id="90e4e-188">Migration initiale</span><span class="sxs-lookup"><span data-stu-id="90e4e-188">Initial migration</span></span>

<span data-ttu-id="90e4e-189">Utilisez la fonctionnalité [Migrations](xref:data/ef-mvc/migrations) d’EF Core pour créer la base de données.</span><span class="sxs-lookup"><span data-stu-id="90e4e-189">Use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to create the database.</span></span> <span data-ttu-id="90e4e-190">La fonctionnalité Migrations est un ensemble d’outils qui vous permettent de créer et de mettre à jour une base de données pour qu’elle corresponde à votre modèle de données.</span><span class="sxs-lookup"><span data-stu-id="90e4e-190">Migrations is a set of tools that let you create and update a database to match your data model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="90e4e-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="90e4e-191">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="90e4e-192">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** > **Console du gestionnaire de package** (PMC).</span><span class="sxs-lookup"><span data-stu-id="90e4e-192">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

<span data-ttu-id="90e4e-193">Dans la console du Gestionnaire de package, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="90e4e-193">In the PMC, enter the following commands:</span></span>

```console
Add-Migration InitialCreate
Update-Database
```

* <span data-ttu-id="90e4e-194">`Add-Migration InitialCreate`: Génère un fichier de migration *Migrations/{horodatage}_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="90e4e-194">`Add-Migration InitialCreate`: Generates an *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="90e4e-195">L’argument `InitialCreate` est le nom de la migration.</span><span class="sxs-lookup"><span data-stu-id="90e4e-195">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="90e4e-196">Vous pouvez utiliser n’importe quel nom, mais par convention, un nom décrivant la migration est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="90e4e-196">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="90e4e-197">Étant donné qu’il s’agit de la première migration, la classe générée contient du code permettant de créer le schéma de la base de données.</span><span class="sxs-lookup"><span data-stu-id="90e4e-197">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="90e4e-198">Le schéma de base de données est basé sur le modèle spécifié dans la classe `MvcMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="90e4e-198">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span>

* <span data-ttu-id="90e4e-199">`Update-Database`: Met à jour la base de données vers la dernière migration, qui a été créée par la commande précédente.</span><span class="sxs-lookup"><span data-stu-id="90e4e-199">`Update-Database`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="90e4e-200">La commande exécute la méthode `Up` dans le fichier *Migrations/{horodatage}_InitialCreate.cs*, ce qui entraîne la création de la base de données.</span><span class="sxs-lookup"><span data-stu-id="90e4e-200">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

  <span data-ttu-id="90e4e-201">La commande « database update » génère l’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="90e4e-201">The database update command generates the following warning:</span></span> 

  > <span data-ttu-id="90e4e-202">Aucun type n’a été spécifié pour la colonne décimale 'Price' sur le type d’entité 'Movie'.</span><span class="sxs-lookup"><span data-stu-id="90e4e-202">No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="90e4e-203">Les valeurs sont tronquées en mode silencieux si elles ne sont pas compatibles avec la précision et l’échelle par défaut.</span><span class="sxs-lookup"><span data-stu-id="90e4e-203">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="90e4e-204">Spécifiez explicitement le type de colonne SQL Server capable d’accueillir toutes les valeurs en utilisant ’HasColumnType()’.</span><span class="sxs-lookup"><span data-stu-id="90e4e-204">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.</span></span>

  <span data-ttu-id="90e4e-205">Vous pouvez ignorer cet avertissement, il sera corrigé dans un prochain tutoriel.</span><span class="sxs-lookup"><span data-stu-id="90e4e-205">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

[!INCLUDE [more information on the PMC tools for EF Core](~/includes/ef-pmc.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="90e4e-206">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="90e4e-206">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="90e4e-207">Exécutez les commandes CLI .NET Core suivantes :</span><span class="sxs-lookup"><span data-stu-id="90e4e-207">Run the following .NET Core CLI commands:</span></span>

```dotnetcli
dotnet ef migrations add InitialCreate
dotnet ef database update
```

* <span data-ttu-id="90e4e-208">`ef migrations add InitialCreate`: Génère un fichier de migration *Migrations/{horodatage}_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="90e4e-208">`ef migrations add InitialCreate`: Generates an *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="90e4e-209">L’argument `InitialCreate` est le nom de la migration.</span><span class="sxs-lookup"><span data-stu-id="90e4e-209">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="90e4e-210">Vous pouvez utiliser n’importe quel nom, mais par convention, un nom décrivant la migration est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="90e4e-210">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="90e4e-211">Étant donné qu’il s’agit de la première migration, la classe générée contient du code permettant de créer le schéma de la base de données.</span><span class="sxs-lookup"><span data-stu-id="90e4e-211">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="90e4e-212">Le schéma de base de donénes est basé sur le modèle spécifié dans la classe `MvcMovieContext` (dans *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="90e4e-212">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span>

* <span data-ttu-id="90e4e-213">`ef database update`: Met à jour la base de données vers la dernière migration, qui a été créée par la commande précédente.</span><span class="sxs-lookup"><span data-stu-id="90e4e-213">`ef database update`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="90e4e-214">La commande exécute la méthode `Up` dans le fichier *Migrations/{horodatage}_InitialCreate.cs*, ce qui entraîne la création de la base de données.</span><span class="sxs-lookup"><span data-stu-id="90e4e-214">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [ more information on the CLI tools for EF Core](~/includes/ef-cli.md)]

---

### <a name="the-initialcreate-class"></a><span data-ttu-id="90e4e-215">La classe InitialCreate</span><span class="sxs-lookup"><span data-stu-id="90e4e-215">The InitialCreate class</span></span>

<span data-ttu-id="90e4e-216">Examinez le fichier de migration *Migrations/{horodatage}_InitialCreate.cs* :</span><span class="sxs-lookup"><span data-stu-id="90e4e-216">Examine the *Migrations/{timestamp}_InitialCreate.cs* migration file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Migrations/20190805165915_InitialCreate.cs?name=snippet)]

 <span data-ttu-id="90e4e-217">La méthode `Up` crée la table Movie et configure `Id` comme la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="90e4e-217">The `Up` method creates the Movie table and configures `Id` as the primary key.</span></span> <span data-ttu-id="90e4e-218">La méthode `Down` rétablit les modifications de schéma provoquées par la migration `Up`.</span><span class="sxs-lookup"><span data-stu-id="90e4e-218">The `Down` method reverts the schema changes made by the `Up` migration.</span></span>

<a name="test"></a>

## <a name="test-the-app"></a><span data-ttu-id="90e4e-219">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="90e4e-219">Test the app</span></span>

* <span data-ttu-id="90e4e-220">Exécutez l’application et cliquez sur le lien **Movie App**.</span><span class="sxs-lookup"><span data-stu-id="90e4e-220">Run the app and click the **Movie App** link.</span></span>

  <span data-ttu-id="90e4e-221">Si vous obtenez une exception similaire à celle-ci :</span><span class="sxs-lookup"><span data-stu-id="90e4e-221">If you get an exception similar to one of the following:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="90e4e-222">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="90e4e-222">Visual Studio</span></span>](#tab/visual-studio)

  ```console
  SqlException: Cannot open database "MvcMovieContext-1" requested by the login. The login failed.
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="90e4e-223">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="90e4e-223">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

  ```console
  SqliteException: SQLite Error 1: 'no such table: Movie'.
  ```

---
  <span data-ttu-id="90e4e-224">Il est probable que vous n’ayez pas effectué l’[étape de migration](#migration).</span><span class="sxs-lookup"><span data-stu-id="90e4e-224">You probably missed the [migrations step](#migration).</span></span>

* <span data-ttu-id="90e4e-225">Testez la page **Create**.</span><span class="sxs-lookup"><span data-stu-id="90e4e-225">Test the **Create** page.</span></span> <span data-ttu-id="90e4e-226">Entrez et envoyez des données.</span><span class="sxs-lookup"><span data-stu-id="90e4e-226">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="90e4e-227">Vous ne pourrez peut-être pas entrer de virgules décimales dans le champ `Price`.</span><span class="sxs-lookup"><span data-stu-id="90e4e-227">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="90e4e-228">Pour prendre en charge la [validation jQuery](https://jqueryvalidation.org/) pour les paramètres régionaux autres que « Anglais » qui utilisent une virgule (« , ») comme décimale et des formats de date autres que le format « Anglais (États-Unis »), l’application doit être localisée.</span><span class="sxs-lookup"><span data-stu-id="90e4e-228">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="90e4e-229">Pour obtenir des instructions sur la localisation, consultez [ce problème GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="90e4e-229">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="90e4e-230">Testez les pages **Edit**, **Details** et **Delete**.</span><span class="sxs-lookup"><span data-stu-id="90e4e-230">Test the **Edit**, **Details**, and **Delete** pages.</span></span>

## <a name="dependency-injection-in-the-controller"></a><span data-ttu-id="90e4e-231">Injection de dépendances dans le contrôleur</span><span class="sxs-lookup"><span data-stu-id="90e4e-231">Dependency injection in the controller</span></span>

<span data-ttu-id="90e4e-232">Ouvrez le fichier *Controllers/MoviesController.cs* et examinez le constructeur :</span><span class="sxs-lookup"><span data-stu-id="90e4e-232">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller (or use the old one as I did in the 3.0 upgrade) because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="90e4e-233">Le constructeur utilise une [injection de dépendance](xref:fundamentals/dependency-injection) pour injecter le contexte de base de données (`MvcMovieContext`) dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="90e4e-233">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="90e4e-234">Le contexte de base de données est utilisé dans chacune des méthodes la [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="90e4e-234">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="90e4e-235">Modèles fortement typés et mot clé @model</span><span class="sxs-lookup"><span data-stu-id="90e4e-235">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="90e4e-236">Plus tôt dans ce didacticiel, vous avez vu comment un contrôleur peut passer des données ou des objets à une vue en utilisant le dictionnaire `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="90e4e-236">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="90e4e-237">Le dictionnaire `ViewData` est un objet dynamique qui fournit un moyen pratique d’effectuer une liaison tardive pour passer des informations à une vue.</span><span class="sxs-lookup"><span data-stu-id="90e4e-237">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="90e4e-238">Le modèle MVC fournit également la possibilité de passer des objets de modèle fortement typés à une vue.</span><span class="sxs-lookup"><span data-stu-id="90e4e-238">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="90e4e-239">Cette approche fortement typée permet de vérifier votre code au moment de la compilation.</span><span class="sxs-lookup"><span data-stu-id="90e4e-239">This strongly typed approach enables compile time code checking.</span></span> <span data-ttu-id="90e4e-240">Le mécanisme de génération de modèles automatique a utilisé cette approche (c’est-à-dire passer un modèle fortement typé) avec la classe `MoviesController` et des vues.</span><span class="sxs-lookup"><span data-stu-id="90e4e-240">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views.</span></span>

<span data-ttu-id="90e4e-241">Examinez la méthode `Details` générée dans le fichier *Controllers/MoviesController.cs* :</span><span class="sxs-lookup"><span data-stu-id="90e4e-241">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="90e4e-242">Le paramètre `id` est généralement passé en tant que données de routage.</span><span class="sxs-lookup"><span data-stu-id="90e4e-242">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="90e4e-243">Par exemple, `https://localhost:5001/movies/details/1` définit :</span><span class="sxs-lookup"><span data-stu-id="90e4e-243">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="90e4e-244">Le contrôleur sur le contrôleur `movies` (le premier segment de l’URL).</span><span class="sxs-lookup"><span data-stu-id="90e4e-244">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="90e4e-245">L’action sur `details` (le deuxième segment de l’URL).</span><span class="sxs-lookup"><span data-stu-id="90e4e-245">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="90e4e-246">L’ID sur 1 (le dernier segment de l’URL).</span><span class="sxs-lookup"><span data-stu-id="90e4e-246">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="90e4e-247">Vous pouvez aussi passer `id` avec une requête de chaîne, comme suit :</span><span class="sxs-lookup"><span data-stu-id="90e4e-247">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="90e4e-248">Le paramètre `id` est défini comme [type nullable](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) au cas où la valeur d’ID n’est pas fournie.</span><span class="sxs-lookup"><span data-stu-id="90e4e-248">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="90e4e-249">Une [expression lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) est passée à `FirstOrDefaultAsync` pour sélectionner les entités de film qui correspondent aux données de routage ou à la valeur de la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="90e4e-249">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="90e4e-250">Si un film est trouvé, une instance du modèle `Movie` est passée à la vue `Details` :</span><span class="sxs-lookup"><span data-stu-id="90e4e-250">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="90e4e-251">Examinez le contenu du fichier *Views/Movies/Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="90e4e-251">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="90e4e-252">L’instruction `@model` située en haut du fichier de la vue spécifie le type d’objet attendu par la vue.</span><span class="sxs-lookup"><span data-stu-id="90e4e-252">The `@model` statement at the top of the view file specifies the type of object that the view expects.</span></span> <span data-ttu-id="90e4e-253">Lorsque le contrôleur de film était créé, l’instruction `@model` suivante était incluse :</span><span class="sxs-lookup"><span data-stu-id="90e4e-253">When the movie controller was created, the following `@model` statement was included:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="90e4e-254">Cette directive `@model` autorise l’accès au film que le contrôleur a passé à la vue.</span><span class="sxs-lookup"><span data-stu-id="90e4e-254">This `@model` directive allows access to the movie that the controller passed to the view.</span></span> <span data-ttu-id="90e4e-255">L’objet `Model` est fortement typé.</span><span class="sxs-lookup"><span data-stu-id="90e4e-255">The `Model` object is strongly typed.</span></span> <span data-ttu-id="90e4e-256">Par exemple, dans la vue *Details.cshtml*, le code passe chaque champ du film aux Helpers HTML `DisplayNameFor` et `DisplayFor` avec l’objet `Model` fortement typé.</span><span class="sxs-lookup"><span data-stu-id="90e4e-256">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="90e4e-257">Les méthodes et les vues `Create` et `Edit` passent aussi un objet du modèle `Movie`.</span><span class="sxs-lookup"><span data-stu-id="90e4e-257">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="90e4e-258">Examinez la vue *Index.cshtml* et la méthode `Index` dans le contrôleur Movies.</span><span class="sxs-lookup"><span data-stu-id="90e4e-258">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="90e4e-259">Notez comment le code crée un objet `List` quand il appelle la méthode `View`.</span><span class="sxs-lookup"><span data-stu-id="90e4e-259">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="90e4e-260">Le code passe cette liste `Movies` de la méthode d’action `Index` à la vue :</span><span class="sxs-lookup"><span data-stu-id="90e4e-260">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="90e4e-261">Lors de la création du contrôleur du film, la génération de modèles automatique a inclus l’instruction `@model` suivante en haut du fichier *Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="90e4e-261">When the movies controller was created, scaffolding included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="90e4e-262">La directive `@model` vous permet d’accéder à la liste des films que le contrôleur a passé à la vue en utilisant un objet `Model` qui est fortement typé.</span><span class="sxs-lookup"><span data-stu-id="90e4e-262">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="90e4e-263">Par exemple, dans la vue *Index.cshtml*, le code boucle dans les films avec une instruction `foreach` sur l’objet `Model` fortement typé :</span><span class="sxs-lookup"><span data-stu-id="90e4e-263">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="90e4e-264">Comme l’objet `Model` est fortement typé (en tant qu’objet `IEnumerable<Movie>`), chaque élément de la boucle est typé en tant que `Movie`.</span><span class="sxs-lookup"><span data-stu-id="90e4e-264">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="90e4e-265">Entre autres avantages, cela signifie que votre code est vérifié au moment de la compilation :</span><span class="sxs-lookup"><span data-stu-id="90e4e-265">Among other benefits, this means that you get compile time checking of the code.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="90e4e-266">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="90e4e-266">Additional resources</span></span>

* [<span data-ttu-id="90e4e-267">Les Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="90e4e-267">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="90e4e-268">Globalisation et localisation</span><span class="sxs-lookup"><span data-stu-id="90e4e-268">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="90e4e-269">[Précédent : Ajout d’une vue](adding-view.md)
> [Suivant : Utilisation de SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="90e4e-269">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="90e4e-270">Ajouter une classe de modèle de données</span><span class="sxs-lookup"><span data-stu-id="90e4e-270">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="90e4e-271">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="90e4e-271">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="90e4e-272">Cliquez avec le bouton droit sur le dossier *Models* > **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="90e4e-272">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="90e4e-273">Nommez la classe **Movie**.</span><span class="sxs-lookup"><span data-stu-id="90e4e-273">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="90e4e-274">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="90e4e-274">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="90e4e-275">Ajoutez une classe au dossier *Modèles* nommé *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="90e4e-275">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="90e4e-276">Générer automatiquement le modèle de film</span><span class="sxs-lookup"><span data-stu-id="90e4e-276">Scaffold the movie model</span></span>

<span data-ttu-id="90e4e-277">Dans cette section, le modèle de film est généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="90e4e-277">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="90e4e-278">Autrement dit, l’outil de génération de modèles automatique génère des pages pour les opérations de création, de lecture, de mise à jour et de suppression (CRUD) pour le modèle de film.</span><span class="sxs-lookup"><span data-stu-id="90e4e-278">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="90e4e-279">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="90e4e-279">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="90e4e-280">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *Contrôleurs*, puis choisissez **Ajouter > Nouvel élément généré automatiquement**.</span><span class="sxs-lookup"><span data-stu-id="90e4e-280">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![affichage de l’étape ci-dessus](adding-model/_static/add_controller21.png)

<span data-ttu-id="90e4e-282">Dans la boîte de dialogue **Ajouter un modèle automatique**, sélectionnez **Contrôleur MVC avec vues, utilisant Entity Framework > Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="90e4e-282">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Boîte de dialogue Ajouter un modèle automatique](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="90e4e-284">Renseignez la boîte de dialogue **Ajouter un contrôleur** :</span><span class="sxs-lookup"><span data-stu-id="90e4e-284">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="90e4e-285">**Classe du modèle :** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="90e4e-285">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="90e4e-286">**Classe de contexte de données :** sélectionnez l’icône **+** et ajoutez le **MvcMovie.Models.MvcMovieContext** par défaut</span><span class="sxs-lookup"><span data-stu-id="90e4e-286">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Ajouter un contexte de données](adding-model/_static/dc.png)

* <span data-ttu-id="90e4e-288">**Affichages :** conservez la valeur par défaut de chaque option activée</span><span class="sxs-lookup"><span data-stu-id="90e4e-288">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="90e4e-289">**Nom du contrôleur :** conservez la valeur par défaut *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="90e4e-289">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="90e4e-290">Sélectionnez **Ajouter**</span><span class="sxs-lookup"><span data-stu-id="90e4e-290">Select **Add**</span></span>

![Boîte de dialogue Ajouter un contrôleur](adding-model/_static/add_controller2.png)

<span data-ttu-id="90e4e-292">Visual Studio crée :</span><span class="sxs-lookup"><span data-stu-id="90e4e-292">Visual Studio creates:</span></span>

* <span data-ttu-id="90e4e-293">Une [classe de contexte de base de données](xref:data/ef-mvc/intro#create-the-database-context) Entity Framework Core (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="90e4e-293">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="90e4e-294">Un contrôleur de films (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="90e4e-294">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="90e4e-295">Des fichiers de vues Razor pour les pages Create, Delete, Details, Edit et Index (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="90e4e-295">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="90e4e-296">La création automatique du contexte de base de données et de méthodes d’action et de vues [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (créer, lire, mettre à jour et supprimer) porte le nom de *génération de modèles automatique*.</span><span class="sxs-lookup"><span data-stu-id="90e4e-296">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="90e4e-297">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="90e4e-297">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="90e4e-298">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="90e4e-298">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="90e4e-299">Installez l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="90e4e-299">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="90e4e-300">Sur Linux, exportez le chemin de l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="90e4e-300">On Linux, export the scaffold tool path:</span></span>

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="90e4e-301">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="90e4e-301">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="90e4e-302">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="90e4e-302">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="90e4e-303">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="90e4e-303">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="90e4e-304">Installez l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="90e4e-304">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="90e4e-305">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="90e4e-305">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of VS tabs                  -->

<span data-ttu-id="90e4e-306">Si vous exécutez l’application et que vous cliquez sur le lien **Mvc Movie**, vous recevez une erreur semblable à la suivante :</span><span class="sxs-lookup"><span data-stu-id="90e4e-306">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="90e4e-307">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="90e4e-307">Visual Studio</span></span>](#tab/visual-studio)

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="90e4e-308">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="90e4e-308">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

``` error
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)

```

---

<span data-ttu-id="90e4e-309">Vous devez créer la base de données, et vous utilisez pour cela la fonctionnalité [Migrations](xref:data/ef-mvc/migrations) d’EF Core.</span><span class="sxs-lookup"><span data-stu-id="90e4e-309">You need to create the database, and you use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="90e4e-310">Les migrations permettent de créer une base de données qui correspond à votre modèle de données, et de mettre à jour le schéma de base de données quand votre modèle de données change.</span><span class="sxs-lookup"><span data-stu-id="90e4e-310">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="90e4e-311">Migration initiale</span><span class="sxs-lookup"><span data-stu-id="90e4e-311">Initial migration</span></span>

<span data-ttu-id="90e4e-312">Dans cette section, vous devez effectuer les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="90e4e-312">In this section, the following tasks are completed:</span></span>

* <span data-ttu-id="90e4e-313">Ajouter une migration initiale</span><span class="sxs-lookup"><span data-stu-id="90e4e-313">Add an initial migration.</span></span>
* <span data-ttu-id="90e4e-314">Mettez à jour la base de données avec la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="90e4e-314">Update the database with the initial migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="90e4e-315">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="90e4e-315">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="90e4e-316">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** > **Console du gestionnaire de package** (PMC).</span><span class="sxs-lookup"><span data-stu-id="90e4e-316">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

   ![Menu Console du Gestionnaire de package](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. <span data-ttu-id="90e4e-318">Dans la console du Gestionnaire de package, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="90e4e-318">In the PMC, enter the following commands:</span></span>

   ```console
   Add-Migration Initial
   Update-Database
   ```

   <span data-ttu-id="90e4e-319">La commande `Add-Migration` génère le code nécessaire à la création du schéma de base de données initial.</span><span class="sxs-lookup"><span data-stu-id="90e4e-319">The `Add-Migration` command generates code to create the initial database schema.</span></span>

   <span data-ttu-id="90e4e-320">Le schéma de base de données est basé sur le modèle spécifié dans la classe `MvcMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="90e4e-320">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span> <span data-ttu-id="90e4e-321">L’argument `Initial` est le nom de la migration.</span><span class="sxs-lookup"><span data-stu-id="90e4e-321">The `Initial` argument is the migration name.</span></span> <span data-ttu-id="90e4e-322">Vous pouvez utiliser n’importe quel nom, mais par convention, un nom décrivant la migration est utilisé.</span><span class="sxs-lookup"><span data-stu-id="90e4e-322">Any name can be used, but by convention, a name that describes the migration is used.</span></span> <span data-ttu-id="90e4e-323">Pour plus d'informations, consultez <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="90e4e-323">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

   <span data-ttu-id="90e4e-324">La commande `Update-Database` exécute la méthode `Up` dans le fichier *Migrations/{horodatage}_InitialCreate.cs*, ce qui entraîne la création de la base de données.</span><span class="sxs-lookup"><span data-stu-id="90e4e-324">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="90e4e-325">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="90e4e-325">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

<span data-ttu-id="90e4e-326">La commande `ef migrations add InitialCreate` génère le code nécessaire à la création du schéma de base de données initial.</span><span class="sxs-lookup"><span data-stu-id="90e4e-326">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span>

<span data-ttu-id="90e4e-327">Le schéma de base de donénes est basé sur le modèle spécifié dans la classe `MvcMovieContext` (dans *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="90e4e-327">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="90e4e-328">L’argument `InitialCreate` est le nom de la migration.</span><span class="sxs-lookup"><span data-stu-id="90e4e-328">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="90e4e-329">Vous pouvez utiliser n’importe quel nom, mais par convention, un nom décrivant la migration est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="90e4e-329">Any name can be used, but by convention, a name is selected that describes the migration.</span></span>

---

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="90e4e-330">Examiner le contexte inscrit avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="90e4e-330">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="90e4e-331">ASP.NET Core comprend [l’injection de dépendances (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="90e4e-331">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="90e4e-332">Des services (tels que le contexte de base de données EF Core) sont inscrits avec l’injection de dépendances au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="90e4e-332">Services (such as the EF Core DB context) are registered with DI during application startup.</span></span> <span data-ttu-id="90e4e-333">Ces services sont affectés aux composants qui les nécessitent (par exemple les Pages Razor) par le biais de paramètres de constructeur.</span><span class="sxs-lookup"><span data-stu-id="90e4e-333">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="90e4e-334">Le code du constructeur qui obtient une instance de contexte de base de données est indiqué plus loin dans le tutoriel.</span><span class="sxs-lookup"><span data-stu-id="90e4e-334">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="90e4e-335">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="90e4e-335">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="90e4e-336">L’outil de génération de modèles automatique a créé automatiquement un contexte de base de données et l’a inscrit dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="90e4e-336">The scaffolding tool automatically created a DB context and registered it with the DI container.</span></span>

<span data-ttu-id="90e4e-337">Examinez la méthode `Startup.ConfigureServices` suivante.</span><span class="sxs-lookup"><span data-stu-id="90e4e-337">Examine the following `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="90e4e-338">La ligne en surbrillance a été ajoutée par l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="90e4e-338">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=14-15)]

<span data-ttu-id="90e4e-339">`MvcMovieContext` coordonne les fonctionnalités d’EF Core (Create, Read, Update, Delete, etc.) pour le modèle `Movie`.</span><span class="sxs-lookup"><span data-stu-id="90e4e-339">The `MvcMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="90e4e-340">Le contexte de données (`MvcMovieContext`) est dérivé de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="90e4e-340">The data context (`MvcMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="90e4e-341">Il spécifie les entités qui sont incluses dans le modèle de données :</span><span class="sxs-lookup"><span data-stu-id="90e4e-341">The data context specifies which entities are included in the data model:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Data/MvcMovieContext.cs)]

<span data-ttu-id="90e4e-342">Le code précédent crée une propriété [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) pour le jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="90e4e-342">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="90e4e-343">Dans la terminologie Entity Framework, un jeu d’entités correspond généralement à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="90e4e-343">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="90e4e-344">Une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="90e4e-344">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="90e4e-345">Le nom de la chaîne de connexion est transmis au contexte en appelant une méthode sur un objet [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="90e4e-345">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="90e4e-346">Pour le développement local, le [système de configuration ASP.NET Core](xref:fundamentals/configuration/index) lit la chaîne de connexion à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="90e4e-346">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="90e4e-347">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="90e4e-347">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="90e4e-348">Vous avez créé un contexte de base de données et vous l’avez inscrit dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="90e4e-348">You created a DB context and registered it with the DI container.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="90e4e-349">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="90e4e-349">Test the app</span></span>

* <span data-ttu-id="90e4e-350">Exécutez l’application et ajoutez `/Movies` à l’URL dans le navigateur (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="90e4e-350">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="90e4e-351">Si vous obtenez une exception de base de données similaire à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="90e4e-351">If you get a database exception similar to the following:</span></span>

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="90e4e-352">Vous avez manqué [l’étape des migrations](#pmc).</span><span class="sxs-lookup"><span data-stu-id="90e4e-352">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="90e4e-353">Testez le lien **Créer**.</span><span class="sxs-lookup"><span data-stu-id="90e4e-353">Test the **Create** link.</span></span> <span data-ttu-id="90e4e-354">Entrez et envoyez des données.</span><span class="sxs-lookup"><span data-stu-id="90e4e-354">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="90e4e-355">Vous ne pourrez peut-être pas entrer de virgules décimales dans le champ `Price`.</span><span class="sxs-lookup"><span data-stu-id="90e4e-355">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="90e4e-356">Pour prendre en charge la [validation jQuery](https://jqueryvalidation.org/) pour les paramètres régionaux autres que « Anglais » qui utilisent une virgule (« , ») comme décimale et des formats de date autres que le format « Anglais (États-Unis »), l’application doit être localisée.</span><span class="sxs-lookup"><span data-stu-id="90e4e-356">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="90e4e-357">Pour obtenir des instructions sur la localisation, consultez [ce problème GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="90e4e-357">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="90e4e-358">Testez les liens **Modifier**, **Détails** et **Supprimer**.</span><span class="sxs-lookup"><span data-stu-id="90e4e-358">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="90e4e-359">Examiner la classe `Startup` :</span><span class="sxs-lookup"><span data-stu-id="90e4e-359">Examine the `Startup` class:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="90e4e-360">Le code précédent mis en surbrillance montre le contexte de la base de données des films qui est ajouté au conteneur [d’injection de dépendance](xref:fundamentals/dependency-injection) :</span><span class="sxs-lookup"><span data-stu-id="90e4e-360">The preceding highlighted code shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container:</span></span>

* <span data-ttu-id="90e4e-361">`services.AddDbContext<MvcMovieContext>(options =>` spécifie la base de données à utiliser et la chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="90e4e-361">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span>
* <span data-ttu-id="90e4e-362">`=>` est un [opérateur lambda](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span><span class="sxs-lookup"><span data-stu-id="90e4e-362">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span></span>

<span data-ttu-id="90e4e-363">Ouvrez le fichier *Controllers/MoviesController.cs* et examinez le constructeur :</span><span class="sxs-lookup"><span data-stu-id="90e4e-363">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="90e4e-364">Le constructeur utilise une [injection de dépendance](xref:fundamentals/dependency-injection) pour injecter le contexte de base de données (`MvcMovieContext`) dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="90e4e-364">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="90e4e-365">Le contexte de base de données est utilisé dans chacune des méthodes la [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="90e4e-365">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="90e4e-366">Modèles fortement typés et mot clé @model</span><span class="sxs-lookup"><span data-stu-id="90e4e-366">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="90e4e-367">Plus tôt dans ce didacticiel, vous avez vu comment un contrôleur peut passer des données ou des objets à une vue en utilisant le dictionnaire `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="90e4e-367">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="90e4e-368">Le dictionnaire `ViewData` est un objet dynamique qui fournit un moyen pratique d’effectuer une liaison tardive pour passer des informations à une vue.</span><span class="sxs-lookup"><span data-stu-id="90e4e-368">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="90e4e-369">Le modèle MVC fournit également la possibilité de passer des objets de modèle fortement typés à une vue.</span><span class="sxs-lookup"><span data-stu-id="90e4e-369">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="90e4e-370">Cette approche fortement typée permet une meilleure vérification de votre code au moment de la compilation.</span><span class="sxs-lookup"><span data-stu-id="90e4e-370">This strongly typed approach enables better compile time checking of your code.</span></span> <span data-ttu-id="90e4e-371">Le mécanisme de génération de modèles automatique a utilisé cette approche (c’est-à-dire passer un modèle fortement typé) avec la classe `MoviesController` et les vues quand il a créé les méthodes et les vues.</span><span class="sxs-lookup"><span data-stu-id="90e4e-371">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="90e4e-372">Examinez la méthode `Details` générée dans le fichier *Controllers/MoviesController.cs* :</span><span class="sxs-lookup"><span data-stu-id="90e4e-372">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="90e4e-373">Le paramètre `id` est généralement passé en tant que données de routage.</span><span class="sxs-lookup"><span data-stu-id="90e4e-373">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="90e4e-374">Par exemple, `https://localhost:5001/movies/details/1` définit :</span><span class="sxs-lookup"><span data-stu-id="90e4e-374">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="90e4e-375">Le contrôleur sur le contrôleur `movies` (le premier segment de l’URL).</span><span class="sxs-lookup"><span data-stu-id="90e4e-375">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="90e4e-376">L’action sur `details` (le deuxième segment de l’URL).</span><span class="sxs-lookup"><span data-stu-id="90e4e-376">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="90e4e-377">L’ID sur 1 (le dernier segment de l’URL).</span><span class="sxs-lookup"><span data-stu-id="90e4e-377">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="90e4e-378">Vous pouvez aussi passer `id` avec une requête de chaîne, comme suit :</span><span class="sxs-lookup"><span data-stu-id="90e4e-378">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="90e4e-379">Le paramètre `id` est défini comme [type nullable](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) au cas où la valeur d’ID n’est pas fournie.</span><span class="sxs-lookup"><span data-stu-id="90e4e-379">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="90e4e-380">Une [expression lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) est passée à `FirstOrDefaultAsync` pour sélectionner les entités de film qui correspondent aux données de routage ou à la valeur de la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="90e4e-380">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="90e4e-381">Si un film est trouvé, une instance du modèle `Movie` est passée à la vue `Details` :</span><span class="sxs-lookup"><span data-stu-id="90e4e-381">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="90e4e-382">Examinez le contenu du fichier *Views/Movies/Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="90e4e-382">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="90e4e-383">En incluant une instruction `@model` en haut du fichier de la vue, vous pouvez spécifier le type d’objet attendu par la vue.</span><span class="sxs-lookup"><span data-stu-id="90e4e-383">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="90e4e-384">Quand vous avez créé le contrôleur pour les films, l’instruction `@model` suivante a été incluse automatiquement en haut du fichier *Details.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="90e4e-384">When you created the movie controller, the following `@model` statement was automatically included at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="90e4e-385">Cette directive `@model` vous permet d’accéder au film que le contrôleur a passé à la vue en utilisant un objet `Model` qui est fortement typé.</span><span class="sxs-lookup"><span data-stu-id="90e4e-385">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="90e4e-386">Par exemple, dans la vue *Details.cshtml*, le code passe chaque champ du film aux Helpers HTML `DisplayNameFor` et `DisplayFor` avec l’objet `Model` fortement typé.</span><span class="sxs-lookup"><span data-stu-id="90e4e-386">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="90e4e-387">Les méthodes et les vues `Create` et `Edit` passent aussi un objet du modèle `Movie`.</span><span class="sxs-lookup"><span data-stu-id="90e4e-387">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="90e4e-388">Examinez la vue *Index.cshtml* et la méthode `Index` dans le contrôleur Movies.</span><span class="sxs-lookup"><span data-stu-id="90e4e-388">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="90e4e-389">Notez comment le code crée un objet `List` quand il appelle la méthode `View`.</span><span class="sxs-lookup"><span data-stu-id="90e4e-389">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="90e4e-390">Le code passe cette liste `Movies` de la méthode d’action `Index` à la vue :</span><span class="sxs-lookup"><span data-stu-id="90e4e-390">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="90e4e-391">Quand vous avez créé le contrôleur pour les films, la génération de modèles automatique a inclus automatiquement l’instruction `@model` suivante en haut du fichier *Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="90e4e-391">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="90e4e-392">La directive `@model` vous permet d’accéder à la liste des films que le contrôleur a passé à la vue en utilisant un objet `Model` qui est fortement typé.</span><span class="sxs-lookup"><span data-stu-id="90e4e-392">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="90e4e-393">Par exemple, dans la vue *Index.cshtml*, le code boucle dans les films avec une instruction `foreach` sur l’objet `Model` fortement typé :</span><span class="sxs-lookup"><span data-stu-id="90e4e-393">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="90e4e-394">Comme l’objet `Model` est fortement typé (en tant qu’objet `IEnumerable<Movie>`), chaque élément de la boucle est typé en tant que `Movie`.</span><span class="sxs-lookup"><span data-stu-id="90e4e-394">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="90e4e-395">Entre autres avantages, cela signifie que votre code est vérifié au moment de la compilation :</span><span class="sxs-lookup"><span data-stu-id="90e4e-395">Among other benefits, this means that you get compile time checking of the code:</span></span>

## <a name="additional-resources"></a><span data-ttu-id="90e4e-396">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="90e4e-396">Additional resources</span></span>

* [<span data-ttu-id="90e4e-397">Les Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="90e4e-397">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="90e4e-398">Globalisation et localisation</span><span class="sxs-lookup"><span data-stu-id="90e4e-398">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="90e4e-399">[Précédent : Ajout d’une vue](adding-view.md)
> [Suivant : Utilisation de SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="90e4e-399">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>

::: moniker-end
