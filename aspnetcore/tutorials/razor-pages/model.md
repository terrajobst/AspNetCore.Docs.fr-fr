---
title: Ajouter un modèle à une application de pages Razor dans ASP.NET Core
author: rick-anderson
description: Découvrez comment ajouter des classes pour gérer des films dans une base de données à l’aide d’Entity Framework Core (EF Core).
manager: wpickett
ms.author: riande
ms.date: 5/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/model
ms.openlocfilehash: 50b1b01448ad870a2889db7dfe8367ab9e661840
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729961"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="b64fc-103">Ajouter un modèle à une application de pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b64fc-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="b64fc-104">Ajouter un modèle de données</span><span class="sxs-lookup"><span data-stu-id="b64fc-104">Add a data model</span></span>

<span data-ttu-id="b64fc-105">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **RazorPagesMovie** > **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="b64fc-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="b64fc-106">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="b64fc-106">Name the folder *Models*.</span></span>

<span data-ttu-id="b64fc-107">Cliquez avec le bouton droit sur le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="b64fc-107">Right click the *Models* folder.</span></span> <span data-ttu-id="b64fc-108">Sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="b64fc-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="b64fc-109">Nommez la classe **Movie**, puis ajoutez les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="b64fc-109">Name the class **Movie** and add the following properties:</span></span>

<span data-ttu-id="b64fc-110">Remplacez le contenu de la classe `Movie` par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b64fc-110">Replace the contents of the `Movie` class with the following code:</span></span>

<span data-ttu-id="b64fc-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="b64fc-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="b64fc-112">Générer automatiquement le modèle de film</span><span class="sxs-lookup"><span data-stu-id="b64fc-112">Scaffold the movie model</span></span>

<span data-ttu-id="b64fc-113">Dans cette section, le modèle de film est généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="b64fc-113">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="b64fc-114">Autrement dit, l’outil de génération de modèles automatique génère des pages pour les opérations de création, de lecture, de mise à jour et de suppression (CRUD) pour le modèle de film.</span><span class="sxs-lookup"><span data-stu-id="b64fc-114">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="b64fc-115">Créer un dossier *Pages/Movies* :</span><span class="sxs-lookup"><span data-stu-id="b64fc-115">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="b64fc-116">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *Pages*, puis choisissez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="b64fc-116">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="b64fc-117">Nommez le dossier *Movies*.</span><span class="sxs-lookup"><span data-stu-id="b64fc-117">Name the folder *Movies*</span></span>

<span data-ttu-id="b64fc-118">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *Pages/Movies*, puis choisissez **Ajouter** > **Nouvel élément généré automatiquement**.</span><span class="sxs-lookup"><span data-stu-id="b64fc-118">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/sca.png)

<span data-ttu-id="b64fc-120">Dans la boîte de dialogue **Ajouter un modèle automatique**, sélectionnez **Pages Razor avec Entity Framework (CRUD)** > **AJOUTER**.</span><span class="sxs-lookup"><span data-stu-id="b64fc-120">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/add_scaffold.png)

<span data-ttu-id="b64fc-122">Renseignez la boîte de dialogue **Pages Razor avec Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="b64fc-122">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="b64fc-123">Dans la liste déroulante **Classe de modèle**, sélectionnez **Film (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="b64fc-123">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="b64fc-124">Dans la ligne **Classe du contexte de données**, sélectionnez le signe (plus) **+** et acceptez le nom généré **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="b64fc-124">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="b64fc-125">Dans la liste déroulante **Classe du contexte de données**, sélectionnez **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="b64fc-125">In the **Data context class** drop down,  select **RazorPagesMovie.Models.RazorPagesMovieContext**</span></span>
* <span data-ttu-id="b64fc-126">Sélectionnez **Ajouter** .</span><span class="sxs-lookup"><span data-stu-id="b64fc-126">Select **Add**.</span></span>

![Image illustrant les instructions précédentes.](model/_static/arp.png)

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="b64fc-128">Effectuer la migration initiale</span><span class="sxs-lookup"><span data-stu-id="b64fc-128">Perform initial migration</span></span>

<span data-ttu-id="b64fc-129">Dans cette section, effectuez les tâches suivantes à l’aide de la console du gestionnaire de package :</span><span class="sxs-lookup"><span data-stu-id="b64fc-129">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="b64fc-130">Ajouter une migration initiale</span><span class="sxs-lookup"><span data-stu-id="b64fc-130">Add an initial migration.</span></span>
* <span data-ttu-id="b64fc-131">Mettez à jour la base de données avec la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="b64fc-131">Update the database with the initial migration.</span></span>

<span data-ttu-id="b64fc-132">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** > **Console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="b64fc-132">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu Console du Gestionnaire de package](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="b64fc-134">Dans la console du Gestionnaire de package, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="b64fc-134">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="b64fc-135">Vous pouvez aussi utiliser les commandes .NET Core CLI suivantes :</span><span class="sxs-lookup"><span data-stu-id="b64fc-135">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="b64fc-136">Ignorez le message d’avertissement suivant ; vous le traiterez dans le prochain tutoriel :</span><span class="sxs-lookup"><span data-stu-id="b64fc-136">Ignore the following warning message, you fix that in the next tutorial:</span></span>

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

<span data-ttu-id="b64fc-137">La commande `Add-Migration` génère le code nécessaire à la création du schéma de base de données initial.</span><span class="sxs-lookup"><span data-stu-id="b64fc-137">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="b64fc-138">Le schéma est basé sur le modèle spécifié dans le fichier `DbContext` (dans *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="b64fc-138">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="b64fc-139">L’argument `Initial` est utilisé pour nommer les migrations.</span><span class="sxs-lookup"><span data-stu-id="b64fc-139">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="b64fc-140">Vous pouvez utiliser n’importe quel nom, mais par convention, choisissez un nom qui décrit la migration.</span><span class="sxs-lookup"><span data-stu-id="b64fc-140">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="b64fc-141">Pour plus d’informations, consultez [Présentation des migrations](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="b64fc-141">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="b64fc-142">La commande `Update-Database` exécute la méthode `Up` dans le fichier *Migrations/{horodatage}_InitialCreate.cs*, ce qui entraîne la création de la base de données.</span><span class="sxs-lookup"><span data-stu-id="b64fc-142">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="b64fc-143">Si vous obtenez l’erreur :</span><span class="sxs-lookup"><span data-stu-id="b64fc-143">If you get the error:</span></span>

<span data-ttu-id="b64fc-144">SqlException: impossible d’ouvrir la base de données 'RazorPagesMovieContext-GUID' demandée par la connexion.</span><span class="sxs-lookup"><span data-stu-id="b64fc-144">SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login.</span></span> <span data-ttu-id="b64fc-145">La connexion a échoué.</span><span class="sxs-lookup"><span data-stu-id="b64fc-145">The login failed.</span></span>
<span data-ttu-id="b64fc-146">Échec de la connexion de l’utilisateur 'nom utilisateur'.</span><span class="sxs-lookup"><span data-stu-id="b64fc-146">Login failed for user 'User-name'.</span></span>

<span data-ttu-id="b64fc-147">Vous avez manqué [l’étape des migrations](#pmc).</span><span class="sxs-lookup"><span data-stu-id="b64fc-147">You missed the [migrations step](#pmc).</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="b64fc-148">Ajouter un modèle de données</span><span class="sxs-lookup"><span data-stu-id="b64fc-148">Add a data model</span></span>

<span data-ttu-id="b64fc-149">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **RazorPagesMovie** > **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="b64fc-149">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="b64fc-150">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="b64fc-150">Name the folder *Models*.</span></span>

<span data-ttu-id="b64fc-151">Cliquez avec le bouton droit sur le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="b64fc-151">Right click the *Models* folder.</span></span> <span data-ttu-id="b64fc-152">Sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="b64fc-152">Select **Add** > **Class**.</span></span> <span data-ttu-id="b64fc-153">Nommez la classe **Movie**, puis ajoutez les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="b64fc-153">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="b64fc-154">Ajouter une chaîne de connexion de base de données</span><span class="sxs-lookup"><span data-stu-id="b64fc-154">Add a database connection string</span></span>

<span data-ttu-id="b64fc-155">Ajoutez une chaîne de connexion au fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="b64fc-155">Add a connection string to the *appsettings.json* file.</span></span>

<span data-ttu-id="b64fc-156">[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span><span class="sxs-lookup"><span data-stu-id="b64fc-156">[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span></span>

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="b64fc-157">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="b64fc-157">Register the database context</span></span>

<span data-ttu-id="b64fc-158">Inscrivez le contexte de base de données auprès du conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) dans la [méthode ConfigureServices de la classe Startup](xref:fundamentals/startup#the-startup-class) (*Startup.cs*) :</span><span class="sxs-lookup"><span data-stu-id="b64fc-158">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

<span data-ttu-id="b64fc-159">[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]</span><span class="sxs-lookup"><span data-stu-id="b64fc-159">[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]</span></span>

<span data-ttu-id="b64fc-160">Générez le projet pour vérifier qu’il ne comporte aucune erreur.</span><span class="sxs-lookup"><span data-stu-id="b64fc-160">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="b64fc-161">Ajouter un outil de génération de modèles automatique et effectuer la migration initiale</span><span class="sxs-lookup"><span data-stu-id="b64fc-161">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="b64fc-162">Dans cette section, effectuez les tâches suivantes à l’aide de la console du gestionnaire de package :</span><span class="sxs-lookup"><span data-stu-id="b64fc-162">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="b64fc-163">Ajoutez le package de génération de code web Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b64fc-163">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="b64fc-164">Ce package est nécessaire à l’exécution du moteur de génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="b64fc-164">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="b64fc-165">Ajoutez une migration initiale.</span><span class="sxs-lookup"><span data-stu-id="b64fc-165">Add an initial migration.</span></span>
* <span data-ttu-id="b64fc-166">Mettez à jour la base de données avec la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="b64fc-166">Update the database with the initial migration.</span></span>

<span data-ttu-id="b64fc-167">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** > **Console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="b64fc-167">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu Console du Gestionnaire de package](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="b64fc-169">Dans la console du Gestionnaire de package, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="b64fc-169">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="b64fc-170">Vous pouvez aussi utiliser les commandes .NET Core CLI suivantes :</span><span class="sxs-lookup"><span data-stu-id="b64fc-170">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="b64fc-171">Ignorez le message suivant :</span><span class="sxs-lookup"><span data-stu-id="b64fc-171">Ignore the following message:</span></span>

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

<span data-ttu-id="b64fc-172">Vous le traiterez dans le prochain tutoriel.</span><span class="sxs-lookup"><span data-stu-id="b64fc-172">You fix that in the next tutorial.</span></span>

<span data-ttu-id="b64fc-173">La commande `Install-Package` installe les outils nécessaires à l’exécution du moteur de génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="b64fc-173">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="b64fc-174">La commande `Add-Migration` génère le code nécessaire à la création du schéma de base de données initial.</span><span class="sxs-lookup"><span data-stu-id="b64fc-174">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="b64fc-175">Le schéma est basé sur le modèle spécifié dans le fichier `DbContext` (dans *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="b64fc-175">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="b64fc-176">L’argument `Initial` est utilisé pour nommer les migrations.</span><span class="sxs-lookup"><span data-stu-id="b64fc-176">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="b64fc-177">Vous pouvez utiliser n’importe quel nom, mais par convention, choisissez un nom qui décrit la migration.</span><span class="sxs-lookup"><span data-stu-id="b64fc-177">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="b64fc-178">Pour plus d’informations, consultez [Présentation des migrations](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="b64fc-178">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="b64fc-179">La commande `Update-Database` exécute la méthode `Up` dans le fichier *Migrations/{horodatage}_InitialCreate.cs*, ce qui entraîne la création de la base de données.</span><span class="sxs-lookup"><span data-stu-id="b64fc-179">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="b64fc-180">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="b64fc-180">Test the app</span></span>

* <span data-ttu-id="b64fc-181">Exécutez l’application et ajoutez `/Movies` à l’URL dans le navigateur (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="b64fc-181">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="b64fc-182">Testez le lien **Créer**.</span><span class="sxs-lookup"><span data-stu-id="b64fc-182">Test the **Create** link.</span></span>

  ![Créer une page](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="b64fc-184">Testez les liens **Modifier**, **Détails** et **Supprimer**.</span><span class="sxs-lookup"><span data-stu-id="b64fc-184">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="b64fc-185">Si vous obtenez une exception SQL, vérifiez que vous avez exécuté les migrations et mis à jour la base de données.</span><span class="sxs-lookup"><span data-stu-id="b64fc-185">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="b64fc-186">Le prochain didacticiel décrit les fichiers créés par la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="b64fc-186">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b64fc-187">[Précédent : Bien démarrer](xref:tutorials/razor-pages/razor-pages-start)
> [Suivant : Pages Razor obtenues par génération de modèles automatique](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="b64fc-187">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
