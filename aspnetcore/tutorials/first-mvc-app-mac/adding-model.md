---
title: Ajouter un modèle à une application ASP.NET Core MVC avec Visual Studio pour Mac
author: rick-anderson
description: Ajoutez un modèle à une application ASP.NET Core simple.
ms.author: riande
ms.date: 09/22/2017
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: 6db079558ccf4515a37a90f7a9e2608333acd7cf
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961384"
---
[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="98a3f-103">Cliquez avec le bouton droit sur le dossier *Models*, puis sélectionnez **Ajouter** > **Nouveau fichier**.</span><span class="sxs-lookup"><span data-stu-id="98a3f-103">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span> 
* <span data-ttu-id="98a3f-104">Dans la boîte de dialogue **Nouveau fichier** :</span><span class="sxs-lookup"><span data-stu-id="98a3f-104">In the **New File** dialog:</span></span>

  * <span data-ttu-id="98a3f-105">Dans le volet gauche, sélectionnez **Général**.</span><span class="sxs-lookup"><span data-stu-id="98a3f-105">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="98a3f-106">Dans le volet central, sélectionnez **Classe vide**.</span><span class="sxs-lookup"><span data-stu-id="98a3f-106">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="98a3f-107">Nommez la classe **Movie**, puis sélectionnez **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="98a3f-107">Name the class **Movie** and select **New**.</span></span>

<span data-ttu-id="98a3f-108">Ajoutez les propriétés suivantes à la classe `Movie` :</span><span class="sxs-lookup"><span data-stu-id="98a3f-108">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="98a3f-109">Le champ `ID` est nécessaire à la base de données pour la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="98a3f-109">The `ID` field is required by the database for the primary key.</span></span>

<span data-ttu-id="98a3f-110">Générez le projet pour vérifier qu’il ne comporte aucune erreur.</span><span class="sxs-lookup"><span data-stu-id="98a3f-110">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="98a3f-111">Vous avez désormais un **M**odèle dans votre application **M**VC.</span><span class="sxs-lookup"><span data-stu-id="98a3f-111">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="98a3f-112">Préparer le projet pour la génération de modèles automatique</span><span class="sxs-lookup"><span data-stu-id="98a3f-112">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="98a3f-113">Cliquez avec le bouton droit sur le fichier projet, puis sélectionnez **Outils > Modifier le fichier**.</span><span class="sxs-lookup"><span data-stu-id="98a3f-113">Right click on the project file, and then select **Tools > Edit File**.</span></span>

  ![affichage de l’étape ci-dessus](adding-model/_static/1.png)

- <span data-ttu-id="98a3f-115">Ajoutez les packages NuGet en surbrillance suivants au fichier *MvcMovie.csproj* :</span><span class="sxs-lookup"><span data-stu-id="98a3f-115">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
  [!code-csharp[](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="98a3f-116">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="98a3f-116">Save the file.</span></span>

- <span data-ttu-id="98a3f-117">Créez un fichier *Models/MvcMovieContext.cs*, puis ajoutez la classe `MvcMovieContext` suivante : [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="98a3f-117">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:  [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span></span>
   
- <span data-ttu-id="98a3f-118">Ouvrez le fichier *Startup.cs*, puis ajoutez deux instructions using : [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span><span class="sxs-lookup"><span data-stu-id="98a3f-118">Open the *Startup.cs* file and add two usings:  [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span></span>

- <span data-ttu-id="98a3f-119">Ajoutez le contexte de base de données au fichier *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="98a3f-119">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="98a3f-120">Cela permet d’indiquer à Entity Framework quelles sont les classes de modèles incluses dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="98a3f-120">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="98a3f-121">Vous définissez un *jeu d’entités* d’objets Movies, lesquels sont représentés dans la base de données sous forme de table Movie.</span><span class="sxs-lookup"><span data-stu-id="98a3f-121">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="98a3f-122">Générez le projet pour vérifier qu’il n’existe aucune erreur.</span><span class="sxs-lookup"><span data-stu-id="98a3f-122">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="98a3f-123">Effectuer une génération de modèles automatique pour MovieController</span><span class="sxs-lookup"><span data-stu-id="98a3f-123">Scaffold the MovieController</span></span>

<span data-ttu-id="98a3f-124">Ouvrez une fenêtre de terminal dans le dossier de projet, puis exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="98a3f-124">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="98a3f-125">Si vous obtenez l’erreur `No executable found matching command "dotnet-aspnet-codegenerator", verify` :</span><span class="sxs-lookup"><span data-stu-id="98a3f-125">If you get the error `No executable found matching command "dotnet-aspnet-codegenerator", verify`:</span></span>

 * <span data-ttu-id="98a3f-126">Vous êtes dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="98a3f-126">You are in the project directory.</span></span> <span data-ttu-id="98a3f-127">Le répertoire du projet contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="98a3f-127">The project directory has the *Program.cs*, *Startup.cs* and *.csproj* files.</span></span>
 * <span data-ttu-id="98a3f-128">Votre version de dotnet est la 1.1 ou une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="98a3f-128">Your dotnet version is 1.1 or higher.</span></span> <span data-ttu-id="98a3f-129">Exécutez `dotnet` pour obtenir la version souhaitée.</span><span class="sxs-lookup"><span data-stu-id="98a3f-129">Run `dotnet` to get the version.</span></span>
 * <span data-ttu-id="98a3f-130">Vous avez ajouté l’élément `<DotNetCliToolReference>` au [fichier MvcMovie.csproj](#prepare-the-project-for-scaffolding).</span><span class="sxs-lookup"><span data-stu-id="98a3f-130">You have added the `<DotNetCliToolReference>` element to the [MvcMovie.csproj file](#prepare-the-project-for-scaffolding).</span></span>
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

<span data-ttu-id="98a3f-131">Le moteur de génération de modèles automatique crée les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="98a3f-131">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="98a3f-132">contrôleur de films (*Controllers/MoviesController.cs*) ;</span><span class="sxs-lookup"><span data-stu-id="98a3f-132">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="98a3f-133">fichiers de vues Razor pour les pages Create, Delete, Details, Edit et Index (*Views/Movies/\*.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="98a3f-133">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="98a3f-134">La création automatique de méthodes d’action et de vues [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (créer, lire, mettre à jour et supprimer) s’appelle la *génération de modèles automatique*.</span><span class="sxs-lookup"><span data-stu-id="98a3f-134">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="98a3f-135">Vous aurez bientôt une application web entièrement opérationnelle qui vous permettra de gérer une base de données de films.</span><span class="sxs-lookup"><span data-stu-id="98a3f-135">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

### <a name="add-the-files-to-visual-studio"></a><span data-ttu-id="98a3f-136">Ajouter les fichiers à Visual Studio</span><span class="sxs-lookup"><span data-stu-id="98a3f-136">Add the files to Visual Studio</span></span>

* <span data-ttu-id="98a3f-137">Ajoutez le fichier *MovieController.cs* au projet Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="98a3f-137">Add the *MovieController.cs* file to the Visual Studio project:</span></span>

  * <span data-ttu-id="98a3f-138">Cliquez avec le bouton droit sur le dossier *Controllers*, puis sélectionnez **Ajouter > Ajouter des fichiers**.</span><span class="sxs-lookup"><span data-stu-id="98a3f-138">Right-click on the *Controllers* folder and select **Add > Add Files**.</span></span>
  * <span data-ttu-id="98a3f-139">Sélectionnez le fichier *MovieController.cs*.</span><span class="sxs-lookup"><span data-stu-id="98a3f-139">Select the *MovieController.cs* file.</span></span>

* <span data-ttu-id="98a3f-140">Ajoutez le dossier *Movies* et les vues :</span><span class="sxs-lookup"><span data-stu-id="98a3f-140">Add the *Movies* folder and views:</span></span>

  * <span data-ttu-id="98a3f-141">Cliquez avec le bouton droit sur le dossier *Views*, puis sélectionnez **Ajouter > Ajouter un dossier existant**.</span><span class="sxs-lookup"><span data-stu-id="98a3f-141">Right-click on the *Views* folder and select **Add > Add Existing Folder**.</span></span>
  * <span data-ttu-id="98a3f-142">Accédez au dossier *Views*, sélectionnez *Views\Movies*, puis **Ouvrir**.</span><span class="sxs-lookup"><span data-stu-id="98a3f-142">Navigate to the *Views* folder, select *Views\Movies*, and then select **Open**.</span></span>
  * <span data-ttu-id="98a3f-143">Dans la boîte de dialogue **Sélectionnez les fichiers à ajouter à partir de Movies**, sélectionnez **Inclure tout**, puis **OK**.</span><span class="sxs-lookup"><span data-stu-id="98a3f-143">In the **Select files to add from Movies** dialog, select **Include All**, and then **OK**.</span></span>

[!INCLUDE [adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="98a3f-144">Vous disposez maintenant d’une base de données et de pages pour afficher, modifier, mettre à jour et supprimer les données.</span><span class="sxs-lookup"><span data-stu-id="98a3f-144">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="98a3f-145">Dans le prochain didacticiel, nous allons utiliser la base de données.</span><span class="sxs-lookup"><span data-stu-id="98a3f-145">In the next tutorial, we'll work with the database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="98a3f-146">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="98a3f-146">Additional resources</span></span>

* [<span data-ttu-id="98a3f-147">Les Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="98a3f-147">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="98a3f-148">Globalisation et localisation</span><span class="sxs-lookup"><span data-stu-id="98a3f-148">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="98a3f-149">[Précédent : Ajout d’une vue](adding-view.md)
> [Suivant : Utilisation de SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="98a3f-149">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
