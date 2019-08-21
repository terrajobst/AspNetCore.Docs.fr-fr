::: moniker range=">= aspnetcore-3.0"

<a name="dc"></a>

<span data-ttu-id="cd3c0-101">Créez un dossier nommé *Data*.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-101">Create a *Data* folder.</span></span>

<span data-ttu-id="cd3c0-102">Ajoutez la classe `MvcMovieContext` suivante au dossier *Data* :</span><span class="sxs-lookup"><span data-stu-id="cd3c0-102">Add the following `MvcMovieContext` class to the *Data* folder:</span></span>  

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/zDocOnly/MvcMovieContext.cs?name=snippet)]

<span data-ttu-id="cd3c0-103">Le code précédent crée une propriété `DbSet` pour le jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-103">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="cd3c0-104">Dans la terminologie Entity Framework, un jeu d’entités correspond généralement à une table de base de données, et une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-104">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="cd3c0-105">Ajouter une chaîne de connexion de base de données</span><span class="sxs-lookup"><span data-stu-id="cd3c0-105">Add a database connection string</span></span>

<span data-ttu-id="cd3c0-106">Ajoutez une chaîne de connexion au fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="cd3c0-106">Add a connection string to the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a><span data-ttu-id="cd3c0-107">Ajouter des packages NuGet des outils EF</span><span class="sxs-lookup"><span data-stu-id="cd3c0-107">Add NuGet packages and EF tools</span></span>

<span data-ttu-id="cd3c0-108">Exécutez les commandes CLI .NET Core suivantes :</span><span class="sxs-lookup"><span data-stu-id="cd3c0-108">Run the following .NET Core CLI commands:</span></span>

```console
dotnet tool install --global dotnet-ef --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SQLite --version 3.0.0-*
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
```

<span data-ttu-id="cd3c0-109">Les commandes précédentes ajoutent des outils Entity Framework Core pour la CLI .NET et plusieurs packages au projet.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-109">The preceding commands add Entity Framework Core Tools for the .NET CLI and several packages to the project.</span></span> <span data-ttu-id="cd3c0-110">Le package `Microsoft.VisualStudio.Web.CodeGeneration.Design` est nécessaire à la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-110">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="cd3c0-111">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="cd3c0-111">Register the database context</span></span>

<span data-ttu-id="cd3c0-112">En tête du fichier *Startup.cs*, ajoutez les instructions `using` suivantes :</span><span class="sxs-lookup"><span data-stu-id="cd3c0-112">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Data;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="cd3c0-113">Inscrivez le contexte de base de données auprès du conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-113">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

<span data-ttu-id="cd3c0-114">Générez le projet en tant que vérification des erreurs du compilateur.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-114">Build the project as a check for compiler errors.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="cd3c0-115">Ajoutez la classe `MvcMovieContext` suivante au dossier *Models* :</span><span class="sxs-lookup"><span data-stu-id="cd3c0-115">Add the following `MvcMovieContext` class to the *Models* folder:</span></span>  

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

<span data-ttu-id="cd3c0-116">Le code précédent crée une propriété `DbSet` pour le jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-116">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="cd3c0-117">Dans la terminologie Entity Framework, un jeu d’entités correspond généralement à une table de base de données, et une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-117">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="cd3c0-118">Ajouter une chaîne de connexion de base de données</span><span class="sxs-lookup"><span data-stu-id="cd3c0-118">Add a database connection string</span></span>

<span data-ttu-id="cd3c0-119">Ajoutez une chaîne de connexion au fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="cd3c0-119">Add a connection string to the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="cd3c0-120">Ajouter les packages NuGet nécessaires</span><span class="sxs-lookup"><span data-stu-id="cd3c0-120">Add required NuGet packages</span></span>

<span data-ttu-id="cd3c0-121">Exécutez la commande CLI .NET Core suivante pour ajouter SQLite et CodeGeneration.Design au projet :</span><span class="sxs-lookup"><span data-stu-id="cd3c0-121">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
```

<span data-ttu-id="cd3c0-122">Le package `Microsoft.VisualStudio.Web.CodeGeneration.Design` est nécessaire à la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-122">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="cd3c0-123">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="cd3c0-123">Register the database context</span></span>

<span data-ttu-id="cd3c0-124">En tête du fichier *Startup.cs*, ajoutez les instructions `using` suivantes :</span><span class="sxs-lookup"><span data-stu-id="cd3c0-124">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="cd3c0-125">Inscrivez le contexte de base de données auprès du conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-125">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="cd3c0-126">Générez le projet en tant que vérification des erreurs.</span><span class="sxs-lookup"><span data-stu-id="cd3c0-126">Build the project as a check for errors.</span></span>
::: moniker-end