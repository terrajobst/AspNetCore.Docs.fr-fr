<a name="dc"></a>

### <a name="add-a-database-context-class"></a>Ajouter une classe de contexte de base de données

Dans le projet RazorPagesMovie, créez un nouveau dossier nommé *Data*. Ajoutez la classe `RazorPagesMovieContext` suivante au dossier *Data* :

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

Le code précédent crée une propriété `DbSet` pour le jeu d’entités. Dans la terminologie Entity Framework, un jeu d’entités correspond généralement à une table de base de données, et une entité correspond à une ligne dans la table.

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a>Ajouter une chaîne de connexion de base de données

Ajoutez une chaîne de connexion au fichier *appsettings.JSON* comme indiqué dans le code en surbrillance suivant :

::: moniker range=">= aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a>Ajouter des packages NuGet des outils EF

Ouvrez un terminal pour le projet RazorPagesMovie.  Cliquez avec le bouton droit sur le nom du projet dans la barre de création/mise en page et accédez à **Outils > Ouvrir** dans Terminal. Exécutez les commandes CLI .NET Core suivantes dans le terminal :

```dotnetcli
dotnet tool install --global dotnet-ef --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SQLite --version 3.0.0-*
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
```

Les commandes précédentes ajoutent des outils Entity Framework Core pour la CLI .NET et plusieurs packages au projet. Le package `Microsoft.VisualStudio.Web.CodeGeneration.Design` est nécessaire à la génération de modèles automatique.

<a name="reg"></a>

### <a name="register-the-database-context"></a>Inscrire le contexte de base de données

En tête du fichier *Startup.cs*, ajoutez les instructions `using` suivantes :

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

Inscrivez le contexte de base de données auprès du conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) dans `Startup.ConfigureServices`.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-9)]

### <a name="add-required-nuget-packages"></a>Ajouter les packages NuGet nécessaires

Exécutez la commande CLI .NET Core suivante pour ajouter SQLite et CodeGeneration.Design au projet :

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
```

Le package `Microsoft.VisualStudio.Web.CodeGeneration.Design` est nécessaire à la génération de modèles automatique.

<a name="reg"></a>

### <a name="register-the-database-context"></a>Inscrire le contexte de base de données

En tête du fichier *Startup.cs*, ajoutez les instructions `using` suivantes :

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

Inscrivez le contexte de base de données auprès du conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) dans `Startup.ConfigureServices`.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

Générez le projet en tant que vérification des erreurs.
::: moniker-end
