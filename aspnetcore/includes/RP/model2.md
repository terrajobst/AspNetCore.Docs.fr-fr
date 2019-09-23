<a name="dc"></a>

### <a name="add-a-database-context-class"></a><span data-ttu-id="5baa1-101">Ajouter une classe de contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="5baa1-101">Add a database context class</span></span>

<span data-ttu-id="5baa1-102">Dans le projet RazorPagesMovie, créez un nouveau dossier nommé *Data*.</span><span class="sxs-lookup"><span data-stu-id="5baa1-102">In the RazorPagesMovie project, create a new folder called *Data*.</span></span> <span data-ttu-id="5baa1-103">Ajoutez la classe `RazorPagesMovieContext` suivante au dossier *Data* :</span><span class="sxs-lookup"><span data-stu-id="5baa1-103">Add the following `RazorPagesMovieContext` class to the *Data* folder:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="5baa1-104">Le code précédent crée une propriété `DbSet` pour le jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="5baa1-104">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="5baa1-105">Dans la terminologie Entity Framework, un jeu d’entités correspond généralement à une table de base de données, et une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="5baa1-105">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="5baa1-106">Ajouter une chaîne de connexion de base de données</span><span class="sxs-lookup"><span data-stu-id="5baa1-106">Add a database connection string</span></span>

<span data-ttu-id="5baa1-107">Ajoutez une chaîne de connexion au fichier *appsettings.JSON* comme indiqué dans le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="5baa1-107">Add a connection string to the *appsettings.json* file as shown in the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a><span data-ttu-id="5baa1-108">Ajouter des packages NuGet des outils EF</span><span class="sxs-lookup"><span data-stu-id="5baa1-108">Add NuGet packages and EF tools</span></span>

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="5baa1-109">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="5baa1-109">Register the database context</span></span>

<span data-ttu-id="5baa1-110">En tête du fichier *Startup.cs*, ajoutez les instructions `using` suivantes :</span><span class="sxs-lookup"><span data-stu-id="5baa1-110">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="5baa1-111">Inscrivez le contexte de base de données auprès du conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5baa1-111">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-9)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="5baa1-112">Ajouter les packages NuGet nécessaires</span><span class="sxs-lookup"><span data-stu-id="5baa1-112">Add required NuGet packages</span></span>

<span data-ttu-id="5baa1-113">Exécutez la commande CLI .NET Core suivante pour ajouter SQLite et CodeGeneration.Design au projet :</span><span class="sxs-lookup"><span data-stu-id="5baa1-113">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
```

<span data-ttu-id="5baa1-114">Le package `Microsoft.VisualStudio.Web.CodeGeneration.Design` est nécessaire à la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="5baa1-114">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="5baa1-115">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="5baa1-115">Register the database context</span></span>

<span data-ttu-id="5baa1-116">En tête du fichier *Startup.cs*, ajoutez les instructions `using` suivantes :</span><span class="sxs-lookup"><span data-stu-id="5baa1-116">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="5baa1-117">Inscrivez le contexte de base de données auprès du conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5baa1-117">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="5baa1-118">Générez le projet en tant que vérification des erreurs.</span><span class="sxs-lookup"><span data-stu-id="5baa1-118">Build the project as a check for errors.</span></span>
::: moniker-end
