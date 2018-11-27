---
title: Ajouter un modèle à une application de pages Razor dans ASP.NET Core
author: rick-anderson
description: Découvrez comment ajouter des classes pour gérer des films dans une base de données à l’aide d’Entity Framework Core (EF Core).
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: c4b23f75da298e4ee804f649219c2ce466b6d6ea
ms.sourcegitcommit: 710fc5fcac258cc8415976dc66bdb355b3e061d5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52299441"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a>Ajouter un modèle à une application de pages Razor dans ASP.NET Core

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Ajouter un modèle de données

Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **RazorPagesMovie** > **Ajouter** > **Nouveau dossier**. Nommez le dossier *Models*.

Cliquez avec le bouton droit sur le dossier *Models*. Sélectionnez **Ajouter** > **Classe**. Nommez la classe **Movie** et remplacez le contenu de la classe `Movie` par le code suivant :

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a>Générer automatiquement le modèle de film

Dans cette section, le modèle de film est généré automatiquement. Autrement dit, l’outil de génération de modèles automatique génère des pages pour les opérations de création, de lecture, de mise à jour et de suppression (CRUD) pour le modèle de film.

Créer un dossier *Pages/Movies* :

* Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *Pages*, puis choisissez **Ajouter** > **Nouveau dossier**.
* Nommez le dossier *Movies*.

Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *Pages/Movies*, puis choisissez **Ajouter** > **Nouvel élément généré automatiquement**.

![Image illustrant les instructions précédentes.](model/_static/sca.png)

Dans la boîte de dialogue **Ajouter un modèle automatique**, sélectionnez **Razor Pages avec Entity Framework (CRUD)** > **Ajouter**.

![Image illustrant les instructions précédentes.](model/_static/add_scaffold.png)

Renseignez la boîte de dialogue **Pages Razor avec Entity Framework (CRUD)** :

* Dans la liste déroulante **Classe de modèle**, sélectionnez **Film (RazorPagesMovie.Models)**.
* Dans la ligne **Classe du contexte de données**, sélectionnez le signe (plus) **+** et acceptez le nom généré **RazorPagesMovie.Models.RazorPagesMovieContext**.
* Sélectionnez **Ajouter**.

![Image illustrant les instructions précédentes.](model/_static/arp.png)

Le processus de génération de modèles automatique crée et met à jour les fichiers suivants :

### <a name="files-created"></a>Fichiers créés

* *Pages/Movies* : Create, Delete, Details, Edit, Index. Ces pages sont détaillées dans le tutoriel suivant.
* *Data/RazorPagesMovieContext.cs*

### <a name="file-updated"></a>Fichier mis à jour

* *Startup.cs* : Les changements apportés à ce fichier sont détaillés dans la section suivante.
* *appSettings.JSON* : La chaîne de connexion utilisée pour se connecter à une base de données locale est ajoutée.

## <a name="examine-the-context-registered-with-dependency-injection"></a>Examiner le contexte inscrit avec l’injection de dépendances

ASP.NET Core comprend [l’injection de dépendances](xref:fundamentals/dependency-injection). Des services (tels que le contexte de base de données EF Core) sont inscrits avec l’injection de dépendances au démarrage de l’application. Ces services sont affectés aux composants qui les nécessitent (par exemple les Pages Razor) par le biais de paramètres de constructeur. Le code du constructeur qui obtient une instance de contexte de base de données est indiqué plus loin dans le tutoriel.

L’outil de génération de modèles automatique a créé automatiquement un contexte de base de données et l’a inscrit dans le conteneur d’injection de dépendances.

Examinez la méthode `Startup.ConfigureServices`. La ligne en surbrillance a été ajoutée par l’outil de génération de modèles automatique :

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

La classe principale qui coordonne les fonctionnalités d’EF Core pour un modèle de données spécifié est la classe de contexte de base de données. Le contexte de données est dérivé de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Il spécifie les entités qui sont incluses dans le modèle de données. Dans ce projet, la classe est nommée `RazorPagesMovieContext`.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

Le code précédent crée une propriété [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) pour le jeu d’entités. Dans la terminologie Entity Framework, un jeu d’entités correspond généralement à une table de base de données. Une entité correspond à une ligne dans la table.

Le nom de la chaîne de connexion est transmis au contexte en appelant une méthode sur un objet [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions). Pour le développement local, le [système de configuration ASP.NET Core](xref:fundamentals/configuration/index) lit la chaîne de connexion à partir du fichier *appsettings.json*.

<a name="pmc"></a>
## <a name="perform-initial-migration"></a>Effectuer la migration initiale

Dans cette section, effectuez les tâches suivantes à l’aide de la console du gestionnaire de package :

* Ajouter une migration initiale
* Mettez à jour la base de données avec la migration initiale.

Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** > **Console du gestionnaire de package**.

  ![Menu Console du Gestionnaire de package](../first-mvc-app/adding-model/_static/pmc.png)

Dans la console du Gestionnaire de package, entrez les commandes suivantes :

```PMC
Add-Migration Initial
Update-Database
```

Vous pouvez aussi utiliser les commandes .NET Core CLI suivantes à partir du dossier de projet :

```console
dotnet ef migrations add Initial
dotnet ef database update
```

Ignorez le message d’avertissement suivant ; vous le traiterez dans un prochain tutoriel :

```console
Microsoft.EntityFrameworkCore.Model.Validation[30000]
      No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.
```

La commande `Add-Migration` génère le code nécessaire à la création du schéma de base de données initial. Le schéma est basé sur le modèle spécifié dans `RazorPagesMovieContext` (dans le fichier *Data/RazorPagesMovieContext.cs*). L’argument `Initial` est utilisé pour nommer les migrations. Vous pouvez utiliser n’importe quel nom, mais par convention, choisissez un nom qui décrit la migration. Pour plus d’informations, consultez [Présentation des migrations](xref:data/ef-mvc/migrations#introduction-to-migrations).

La commande `Update-Database` exécute la méthode `Up` dans le fichier *Migrations/{horodatage}_InitialCreate.cs*, ce qui entraîne la création de la base de données.

Si vous obtenez l’erreur :

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

Vous avez manqué [l’étape des migrations](#pmc).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Ajouter un modèle de données

Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **RazorPagesMovie** > **Ajouter** > **Nouveau dossier**. Nommez le dossier *Models*.

Cliquez avec le bouton droit sur le dossier *Models*. Sélectionnez **Ajouter** > **Classe**. Nommez la classe **Movie**, puis ajoutez les propriétés suivantes :

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>Ajouter une chaîne de connexion de base de données

Ajoutez une chaîne de connexion au fichier *appsettings.json*.

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>Inscrire le contexte de base de données

Inscrivez le contexte de base de données auprès du conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) dans la [méthode ConfigureServices de la classe Startup](xref:fundamentals/startup#the-startup-class) (*Startup.cs*) :

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

Générez le projet pour vérifier qu’il ne comporte aucune erreur.

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Ajouter un outil de génération de modèles automatique et effectuer la migration initiale

Dans cette section, effectuez les tâches suivantes à l’aide de la console du gestionnaire de package :

* Ajoutez le package de génération de code web Visual Studio. Ce package est nécessaire à l’exécution du moteur de génération de modèles automatique.
* Ajoutez une migration initiale.
* Mettez à jour la base de données avec la migration initiale.

Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** > **Console du gestionnaire de package**.

  ![Menu Console du Gestionnaire de package](../first-mvc-app/adding-model/_static/pmc.png)

Dans la console du Gestionnaire de package, entrez les commandes suivantes :

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

Vous pouvez aussi utiliser les commandes .NET Core CLI suivantes :

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

Ignorez le message suivant :

```console
Microsoft.EntityFrameworkCore.Model.Validation[30000]
      No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'
```

Vous le traiterez dans le prochain tutoriel.

La commande `Install-Package` installe les outils nécessaires à l’exécution du moteur de génération de modèles automatique.

La commande `Add-Migration` génère le code nécessaire à la création du schéma de base de données initial. Le schéma est basé sur le modèle spécifié dans le fichier `DbContext` (dans *Models/MovieContext.cs*). L’argument `Initial` est utilisé pour nommer les migrations. Vous pouvez utiliser n’importe quel nom, mais par convention, choisissez un nom qui décrit la migration. Pour plus d’informations, consultez [Présentation des migrations](xref:data/ef-mvc/migrations#introduction-to-migrations).

La commande `Update-Database` exécute la méthode `Up` dans le fichier *Migrations/{horodatage}_InitialCreate.cs*, ce qui entraîne la création de la base de données.

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a>Tester l’application

* Exécutez l’application et ajoutez `/Movies` à l’URL dans le navigateur (`http://localhost:port/movies`).
* Testez le lien **Créer**.

  ![Créer une page](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Testez les liens **Modifier**, **Détails** et **Supprimer**.

Si vous obtenez une exception SQL, vérifiez que vous avez exécuté les migrations et mis à jour la base de données.

Le prochain didacticiel décrit les fichiers créés par la génération de modèles automatique.

> [!div class="step-by-step"]
> [Précédent : Bien démarrer](xref:tutorials/razor-pages/razor-pages-start)
> [Suivant : Pages Razor obtenues par génération de modèles automatique](xref:tutorials/razor-pages/page)
