---
title: Ajouter un modèle dans une application ASP.NET Core MVC
author: rick-anderson
description: Ajoutez un modèle à une application ASP.NET Core simple.
ms.author: riande
ms.date: 12/8/2017
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 1e562116de8e6a88666f578f7255e325735c10a9
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272320"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a>Ajouter un modèle dans une application ASP.NET Core MVC

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model1.md)]

Cliquez avec le bouton droit sur le dossier *Models* > **Ajouter** > **Classe**. Nommez la classe **Movie**, puis ajoutez les propriétés suivantes :

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

Le champ `ID` est nécessaire à la base de données pour la clé primaire. 

Générez le projet pour vérifier qu’il ne comporte aucune erreur. Vous avez désormais un **M**odèle dans votre application **M**VC.

## <a name="scaffolding-a-controller"></a>Génération de modèles automatique pour un contrôleur

::: moniker range=">= aspnetcore-2.1"

Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *Contrôleurs*, puis choisissez **Ajouter > Nouvel élément généré automatiquement**.

![affichage de l’étape ci-dessus](adding-model/_static/add_controller21.png)

Dans la boîte de dialogue **Ajouter un modèle automatique**, appuyez sur **Contrôleur MVC avec vues, utilisant Entity Framework > Ajouter**.

![Boîte de dialogue Ajouter un modèle automatique](adding-model/_static/add_scaffold21.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *Contrôleurs* **> Ajouter > Contrôleur**.

![affichage de l’étape ci-dessus](adding-model/_static/add_controller.png)

Si la boîte de dialogue **Ajouter des dépendances MVC** apparaît :

* [Effectuez la mise à jour de Visual Studio vers la dernière version](https://www.visualstudio.com/downloads/). Les versions de Visual Studio antérieures à 15.5 affichent cette boîte de dialogue.
* Si vous ne pouvez pas effectuer la mise à jour, sélectionnez **ADD**, puis suivez à nouveau les étapes pour ajouter un contrôleur.

Dans la boîte de dialogue **Ajouter un modèle automatique**, appuyez sur **Contrôleur MVC avec vues, utilisant Entity Framework > Ajouter**.

![Boîte de dialogue Ajouter un modèle automatique](adding-model/_static/add_scaffold2.png)

::: moniker-end

Renseignez la boîte de dialogue **Ajouter un contrôleur** :

* **Classe de modèle :** *Movie (MvcMovie.Models)*
* **Classe de contexte de données :** sélectionnez l’icône **+** et ajoutez le **MvcMovie.Models.MvcMovieContext** par défaut.

![Ajouter un contexte de données](adding-model/_static/dc.png)

* **Affichages :** conservez la valeur par défaut de chaque option activée.
* **Nom du contrôleur :** conservez la valeur par défaut *MoviesController*.
* Appuyez sur **Ajouter**.

![Boîte de dialogue Ajouter un contrôleur](adding-model/_static/add_controller2.png)

Visual Studio crée :

* Une [classe de contexte de base de données](xref:data/ef-mvc/intro#create-the-database-context) Entity Framework Core (*Data/MvcMovieContext.cs*)
* Un contrôleur de films (*Controllers/MoviesController.cs*)
* Des fichiers de vues Razor pour les pages Create, Delete, Details, Edit et Index (<em>Views/Movies/&ast;.cshtml</em>)

La création automatique du contexte de base de données et de méthodes d’action et de vues [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (créer, lire, mettre à jour et supprimer) porte le nom de *génération de modèles automatique*. Vous aurez bientôt une application web entièrement opérationnelle qui vous permettra de gérer une base de données de films.

Si vous exécutez l’application et que vous cliquez sur le lien **Mvc Movie**, vous recevez une erreur semblable à la suivante :

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

Vous devez créer la base de données, et vous utiliserez pour cela la fonctionnalité [Migrations](xref:data/ef-mvc/migrations) d’EF Core. Les migrations permettent de créer une base de données qui correspond à votre modèle de données, et de mettre à jour le schéma de base de données quand votre modèle de données change.

## <a name="add-ef-tooling-and-perform-initial-migration"></a>Ajouter les outils EF et effectuer la migration initiale

Dans cette section, vous allez utiliser la console du Gestionnaire de package pour :

* Ajouter le package d’outils Entity Framework Core. Ce package est nécessaire pour ajouter des migrations et mettre à jour la base de données
* Ajouter une migration initiale
* Mettez à jour la base de données avec la migration initiale.

Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet > Console du Gestionnaire de package**.

<!-- following image shared with uid: tutorials/razor-pages/model -->![Menu Console du Gestionnaire de package](adding-model/_static/pmc.png)

Dans la console du Gestionnaire de package, entrez les commandes suivantes :

::: moniker range=">= aspnetcore-2.1"
``` PMC
Add-Migration Initial
Update-Database
```

Ignorez le message d’erreur suivant ; nous le traiterons dans le prochain tutoriel :

*Microsoft.EntityFrameworkCore.Model.Validation[30000]*  
      *Aucun type n’a été spécifié pour la colonne décimale 'Price' sur le type d’entité 'Movie'. Les valeurs sont tronquées en mode silencieux si elles ne sont pas compatibles avec la précision et l’échelle par défaut. Spécifiez explicitement le type de colonne SQL Server capable d’accueillir toutes les valeurs en utilisant 'ForHasColumnType()'.*

::: moniker-end
::: moniker range="<= aspnetcore-2.0"

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

**Remarque :** Si vous recevez une erreur avec la commande `Install-Package`, ouvrez le Gestionnaire de Package NuGet et recherchez le package `Microsoft.EntityFrameworkCore.Tools`. Ceci vous permet d’installer le package ou de vérifier s’il est déjà installé. Vous pouvez aussi appliquer [l’approche CLI](#cli) si vous rencontrez des problèmes avec la console du Gestionnaire de package.

::: moniker-end

La commande `Add-Migration` crée le code nécessaire à la création du schéma de base de données initial. Le schéma est basé sur le modèle spécifié dans le fichier `DbContext` (dans *Data/MvcMovieContext.cs*). L’argument `Initial` est utilisé pour nommer les migrations. Vous pouvez utiliser n’importe quel nom, mais par convention, choisissez un nom qui décrit la migration. Pour plus d’informations, consultez [Présentation des migrations](xref:data/ef-mvc/migrations#introduction-to-migrations).

La commande `Update-Database` exécute la méthode `Up` dans le fichier *Migrations/\<date et heure>_Initial.cs*, ce qui entraîne la création de la base de données.

<a name="cli"></a> Vous pouvez effectuer les étapes qui précèdent à l’aide de l’interface de ligne de commande (CLI) plutôt que la console du Gestionnaire de package :

* Ajoutez les [outils EF Core](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) au fichier *.csproj*.
* Exécutez les commandes suivantes à partir de la console (dans le répertoire du projet) :

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```

  Si vous exécutez l’application et que vous obtenez l’erreur :

  ```text
  SqlException: Cannot open database "Movie" requested by the login.
  The login failed.
  Login failed for user 'user name'.
  ```

Vous n’avez probablement pas exécuté `dotnet ef database update`.

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model3.md)]

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]
::: moniker-end

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model4.md)]

![Menu contextuel d’IntelliSense sur un élément de modèle qui répertorie les propriétés disponibles pour ID, Price, Release Date et Title.](adding-model/_static/ints.png)

## <a name="additional-resources"></a>Ressources supplémentaires

* [Les Tag Helpers](xref:mvc/views/tag-helpers/intro)
* [Globalisation et localisation](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [Précédent : Ajout d’une vue](adding-view.md)
> [Suivant : Utilisation de SQL](working-with-sql.md)  
