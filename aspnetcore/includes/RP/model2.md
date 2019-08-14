<a name="dc"></a>

### <a name="add-a-database-context-class"></a><span data-ttu-id="6d909-101">Ajouter une classe de contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="6d909-101">Add a database context class</span></span>

<span data-ttu-id="6d909-102">Dans le projet RazorPagesMovie, créez un nouveau dossier nommé *Data*.</span><span class="sxs-lookup"><span data-stu-id="6d909-102">In the RazorPagesMovie project, create a new folder called *Data*.</span></span> <span data-ttu-id="6d909-103">Ajoutez la classe `RazorPagesMovieContext` suivante au dossier *Data* :</span><span class="sxs-lookup"><span data-stu-id="6d909-103">Add the following `RazorPagesMovieContext` class to the *Data* folder:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="6d909-104">Le code précédent crée une propriété `DbSet` pour le jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="6d909-104">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="6d909-105">Dans la terminologie Entity Framework, un jeu d’entités correspond généralement à une table de base de données, et une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="6d909-105">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="6d909-106">Ajouter une chaîne de connexion de base de données</span><span class="sxs-lookup"><span data-stu-id="6d909-106">Add a database connection string</span></span>

<span data-ttu-id="6d909-107">Ajoutez une chaîne de connexion au fichier *appsettings.JSON* comme indiqué dans le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="6d909-107">Add a connection string to the *appsettings.json* file as shown in the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a><span data-ttu-id="6d909-108">Ajouter des packages NuGet des outils EF</span><span class="sxs-lookup"><span data-stu-id="6d909-108">Add NuGet packages and EF tools</span></span>

<span data-ttu-id="6d909-109">Ouvrez un terminal pour le projet RazorPagesMovie.</span><span class="sxs-lookup"><span data-stu-id="6d909-109">Open a terminal for the RazorPagesMovie project.</span></span>  <span data-ttu-id="6d909-110">Cliquez avec le bouton droit sur le nom du projet dans la barre de création/mise en page et accédez à **Outils > Ouvrir** dans Terminal.</span><span class="sxs-lookup"><span data-stu-id="6d909-110">Right click the project name in the design/layout bar and go to **Tools > Open** in Terminal.</span></span> <span data-ttu-id="6d909-111">Exécutez les commandes CLI .NET Core suivantes dans le terminal :</span><span class="sxs-lookup"><span data-stu-id="6d909-111">Run the following .NET Core CLI commands in the Termial:</span></span>

```console
dotnet tool install --global dotnet-ef --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SQLite --version 3.0.0-*
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
```

<span data-ttu-id="6d909-112">Les commandes précédentes ajoutent des outils Entity Framework Core pour la CLI .NET et plusieurs packages au projet.</span><span class="sxs-lookup"><span data-stu-id="6d909-112">The preceding commands add Entity Framework Core Tools for the .NET CLI and several packages to the project.</span></span> <span data-ttu-id="6d909-113">Le package `Microsoft.VisualStudio.Web.CodeGeneration.Design` est nécessaire à la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="6d909-113">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="6d909-114">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="6d909-114">Register the database context</span></span>

<span data-ttu-id="6d909-115">En tête du fichier *Startup.cs*, ajoutez les instructions `using` suivantes :</span><span class="sxs-lookup"><span data-stu-id="6d909-115">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="6d909-116">Inscrivez le contexte de base de données auprès du conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6d909-116">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-9)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="6d909-117">Ajouter les packages NuGet nécessaires</span><span class="sxs-lookup"><span data-stu-id="6d909-117">Add required NuGet packages</span></span>

<span data-ttu-id="6d909-118">Exécutez la commande CLI .NET Core suivante pour ajouter SQLite et CodeGeneration.Design au projet :</span><span class="sxs-lookup"><span data-stu-id="6d909-118">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design

```

<span data-ttu-id="6d909-119">Le package `Microsoft.VisualStudio.Web.CodeGeneration.Design` est nécessaire à la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="6d909-119">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="6d909-120">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="6d909-120">Register the database context</span></span>

<span data-ttu-id="6d909-121">En tête du fichier *Startup.cs*, ajoutez les instructions `using` suivantes :</span><span class="sxs-lookup"><span data-stu-id="6d909-121">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="6d909-122">Inscrivez le contexte de base de données auprès du conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6d909-122">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="6d909-123">Générez le projet en tant que vérification des erreurs.</span><span class="sxs-lookup"><span data-stu-id="6d909-123">Build the project as a check for errors.</span></span>
::: moniker-end
