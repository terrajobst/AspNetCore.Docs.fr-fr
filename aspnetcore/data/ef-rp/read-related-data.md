---
title: "Pages Razor avec EF Core - lire les données associées - 6 de 8"
author: rick-anderson
description: "Dans ce didacticiel vous lire et afficher les données associées, autrement dit, les données Entity Framework charge dans les propriétés de navigation."
keywords: "ASP.NET Core, Entity Framework Core, les données associées, jointures"
ms.author: riande
manager: wpickett
ms.date: 11/05/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/read-related-data
ms.openlocfilehash: ba9b17ecdcb605d39117d03230b1db37e8e4d0dd
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2017
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a><span data-ttu-id="f71cc-104">Lecture liés de données - Core EF avec les Pages Razor (6 de 8)</span><span class="sxs-lookup"><span data-stu-id="f71cc-104">Reading related data - EF Core with Razor Pages (6 of 8)</span></span>

<span data-ttu-id="f71cc-105">Par [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f71cc-105">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="f71cc-106">Dans ce didacticiel, les données associées sont lues et affichées.</span><span class="sxs-lookup"><span data-stu-id="f71cc-106">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="f71cc-107">Les données associées sont données EF Core charge dans les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="f71cc-107">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="f71cc-108">Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, téléchargez le [application terminée pour cette étape](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span><span class="sxs-lookup"><span data-stu-id="f71cc-108">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="f71cc-109">Les illustrations suivantes montrent les pages terminées pour ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="f71cc-109">The following illustrations show the completed pages for this tutorial:</span></span>

![Page d’Index de cours](read-related-data/_static/courses-index.png)

![Page d’Index instructeurs](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="f71cc-112">Chargement hâtif, explicit et différé de données connexes</span><span class="sxs-lookup"><span data-stu-id="f71cc-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="f71cc-113">Il existe plusieurs façons que EF Core chargez des données connexes dans les propriétés de navigation d’une entité :</span><span class="sxs-lookup"><span data-stu-id="f71cc-113">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="f71cc-114">[Chargement hâtif](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="f71cc-114">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="f71cc-115">Chargement hâtif est lorsqu’une requête pour un type d’entité charge également des entités associées.</span><span class="sxs-lookup"><span data-stu-id="f71cc-115">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="f71cc-116">Lors de la lecture de l’entité, ses données associées sont récupérées.</span><span class="sxs-lookup"><span data-stu-id="f71cc-116">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="f71cc-117">Cela entraîne généralement une requête de jointure unique qui extrait toutes les données que nécessaire.</span><span class="sxs-lookup"><span data-stu-id="f71cc-117">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="f71cc-118">EF Core émet plusieurs requêtes pour certains types de chargement hâtif.</span><span class="sxs-lookup"><span data-stu-id="f71cc-118">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="f71cc-119">Émission de requêtes multiples peut être plus efficace qu’était le cas pour certaines requêtes de EF6 où il a une seule requête.</span><span class="sxs-lookup"><span data-stu-id="f71cc-119">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="f71cc-120">Chargement hâtif est spécifié avec la `Include` et `ThenInclude` méthodes.</span><span class="sxs-lookup"><span data-stu-id="f71cc-120">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

 ![Exemple de chargement hâtif](read-related-data/_static/eager-loading.png)
 
 <span data-ttu-id="f71cc-122">Chargement hâtif envoie plusieurs requêtes lorsqu’une collection de nvavigation est incluse :</span><span class="sxs-lookup"><span data-stu-id="f71cc-122">Eager loading sends multiple queries when a collection nvavigation is included:</span></span>

 * <span data-ttu-id="f71cc-123">Une requête pour la requête principale</span><span class="sxs-lookup"><span data-stu-id="f71cc-123">One query for the main query</span></span> 
 * <span data-ttu-id="f71cc-124">Une requête pour chaque collection de « session » dans l’arborescence de la charge.</span><span class="sxs-lookup"><span data-stu-id="f71cc-124">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="f71cc-125">Séparer les requêtes avec `Load`: les données peuvent être récupérées dans des requêtes distinctes et EF Core » résout « les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="f71cc-125">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="f71cc-126">signifie « correctifs des » que EF Core remplit automatiquement les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="f71cc-126">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="f71cc-127">Séparer les requêtes avec `Load` est plus explicite de chargement chargement hâtif.</span><span class="sxs-lookup"><span data-stu-id="f71cc-127">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

 ![Exemple de requêtes distinctes](read-related-data/_static/separate-queries.png)

 <span data-ttu-id="f71cc-129">Remarque : EF Core résout automatiquement les propriétés de navigation pour toutes les autres entités qui ont été précédemment chargées dans l’instance de contexte.</span><span class="sxs-lookup"><span data-stu-id="f71cc-129">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="f71cc-130">Même si les données pour une propriété de navigation seront *pas* explicitement inclus, la propriété peut toujours être remplie si certaines ou toutes les entités connexes ont été précédemment chargées.</span><span class="sxs-lookup"><span data-stu-id="f71cc-130">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="f71cc-131">[Chargement explicite](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="f71cc-131">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="f71cc-132">Lors de la lecture de l’entité est tout d’abord, les données associées n’est pas récupérées.</span><span class="sxs-lookup"><span data-stu-id="f71cc-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="f71cc-133">Code doit être écrit pour récupérer les données associées lorsque cela est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="f71cc-133">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="f71cc-134">Chargement explicite avec des requêtes distinctes entraîne plusieurs requêtes envoyées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="f71cc-134">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="f71cc-135">Avec le chargement explicite, le code spécifie les propriétés de navigation doivent être chargées.</span><span class="sxs-lookup"><span data-stu-id="f71cc-135">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="f71cc-136">Utilisez la `Load` méthode pour effectuer le chargement explicite.</span><span class="sxs-lookup"><span data-stu-id="f71cc-136">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="f71cc-137">Exemple :</span><span class="sxs-lookup"><span data-stu-id="f71cc-137">For example:</span></span>

 ![Exemple de chargement explicite](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="f71cc-139">[Chargement différé](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="f71cc-139">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="f71cc-140">[EF Core ne prend pas en charge le chargement tardif](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="f71cc-140">[EF Core does not currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="f71cc-141">Lors de la lecture de l’entité est tout d’abord, les données associées n’est pas récupérées.</span><span class="sxs-lookup"><span data-stu-id="f71cc-141">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="f71cc-142">La première fois qu’une propriété de navigation est accessible, les données requises pour cette propriété de navigation sont automatiquement récupérées.</span><span class="sxs-lookup"><span data-stu-id="f71cc-142">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="f71cc-143">Une requête est envoyée à la base de données chaque fois qu’une propriété de navigation est accessible pour la première fois.</span><span class="sxs-lookup"><span data-stu-id="f71cc-143">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="f71cc-144">Le `Select` opérateur charge uniquement les données connexes nécessaires.</span><span class="sxs-lookup"><span data-stu-id="f71cc-144">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="f71cc-145">Créer une page de cours qui affiche le nom du service</span><span class="sxs-lookup"><span data-stu-id="f71cc-145">Create a Courses page that displays department name</span></span>

<span data-ttu-id="f71cc-146">L’entité de cours inclut une propriété de navigation qui contient le `Department` entité.</span><span class="sxs-lookup"><span data-stu-id="f71cc-146">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="f71cc-147">Le `Department` entité contient le service auquel appartient le cours.</span><span class="sxs-lookup"><span data-stu-id="f71cc-147">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="f71cc-148">Pour afficher le nom du service affecté dans une liste de cours :</span><span class="sxs-lookup"><span data-stu-id="f71cc-148">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="f71cc-149">Obtenir le `Name` propriété à partir de la `Department` entité.</span><span class="sxs-lookup"><span data-stu-id="f71cc-149">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="f71cc-150">Le `Department` provient de l’entité la `Course.Department` propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="f71cc-150">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![ourse. Service](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="f71cc-152">Structure du modèle de cours</span><span class="sxs-lookup"><span data-stu-id="f71cc-152">Scaffold the Course model</span></span>

* <span data-ttu-id="f71cc-153">Quittez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f71cc-153">Exit Visual Studio.</span></span>
* <span data-ttu-id="f71cc-154">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="f71cc-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="f71cc-155">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="f71cc-155">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

<span data-ttu-id="f71cc-156">Les structures de commande précédent la `Course` modèle.</span><span class="sxs-lookup"><span data-stu-id="f71cc-156">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="f71cc-157">Ouvrez le projet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f71cc-157">Open the project in Visual Studio.</span></span>

<span data-ttu-id="f71cc-158">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="f71cc-158">Build the project.</span></span> <span data-ttu-id="f71cc-159">La build génère des erreurs comme suit :</span><span class="sxs-lookup"><span data-stu-id="f71cc-159">The build generates errors like the following:</span></span>

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="f71cc-160">Modifier globalement `_context.Course` à `_context.Courses` (autrement dit, ajoutez un « s » pour `Course`).</span><span class="sxs-lookup"><span data-stu-id="f71cc-160">Globally change `_context.Course` to `_context.Courses` (that is, add an "s" to `Course`).</span></span> <span data-ttu-id="f71cc-161">7 occurrences sont trouvées et mis à jour.</span><span class="sxs-lookup"><span data-stu-id="f71cc-161">7 occurrences are found and updated.</span></span>

<span data-ttu-id="f71cc-162">Ouvrez *Pages/Courses/Index.cshtml.cs* et examinez le `OnGetAsync` (méthode).</span><span class="sxs-lookup"><span data-stu-id="f71cc-162">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="f71cc-163">Le moteur de génération de modèles automatique spécifié pour un chargement hâtif le `Department` propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="f71cc-163">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="f71cc-164">Le `Include` méthode spécifie un chargement hâtif.</span><span class="sxs-lookup"><span data-stu-id="f71cc-164">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="f71cc-165">Exécutez l’application et sélectionnez le **cours** lien.</span><span class="sxs-lookup"><span data-stu-id="f71cc-165">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="f71cc-166">La colonne Service affiche le `DepartmentID`, ce qui n’est pas utile.</span><span class="sxs-lookup"><span data-stu-id="f71cc-166">The department column displays the `DepartmentID`, which is not useful.</span></span>

<span data-ttu-id="f71cc-167">Mettez à jour la méthode `OnGetAsync` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="f71cc-167">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="f71cc-168">Le code précédent ajoute `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="f71cc-168">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="f71cc-169">`AsNoTracking`améliore les performances, car les entités retournées ne sont pas suivies.</span><span class="sxs-lookup"><span data-stu-id="f71cc-169">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="f71cc-170">Les entités ne sont pas suivies, car ils ne sont pas mis à jour dans le contexte actuel.</span><span class="sxs-lookup"><span data-stu-id="f71cc-170">The entities are not tracked because they are not updated in the current context.</span></span>

<span data-ttu-id="f71cc-171">Mise à jour *Views/Courses/Index.cshtml* avec le balisage mis en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="f71cc-171">Update *Views/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="f71cc-172">Les modifications suivantes ont été apportées au code de modèle généré automatiquement :</span><span class="sxs-lookup"><span data-stu-id="f71cc-172">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="f71cc-173">Modifié le titre de l’Index au cours.</span><span class="sxs-lookup"><span data-stu-id="f71cc-173">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="f71cc-174">Ajouter un **nombre** colonne qui affiche le `CourseID` valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="f71cc-174">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="f71cc-175">Par défaut, les clés primaires ne sont pas structurés, car ceux-ci ne sont généralement pas de sens pour les utilisateurs finaux.</span><span class="sxs-lookup"><span data-stu-id="f71cc-175">By default, primary keys aren't scaffolded because normally they are meaningless to end users.</span></span> <span data-ttu-id="f71cc-176">Toutefois, dans ce cas la clé primaire est significative.</span><span class="sxs-lookup"><span data-stu-id="f71cc-176">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="f71cc-177">Modifié le **service** colonne pour afficher le nom du service.</span><span class="sxs-lookup"><span data-stu-id="f71cc-177">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="f71cc-178">Le code affiche le `Name` propriété de la `Department` entité qui est chargée dans le `Department` propriété de navigation :</span><span class="sxs-lookup"><span data-stu-id="f71cc-178">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="f71cc-179">Exécutez l’application et sélectionnez le **cours** onglet pour afficher la liste avec les noms de service.</span><span class="sxs-lookup"><span data-stu-id="f71cc-179">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Page d’Index de cours](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="f71cc-181">Chargement liés de données avec Select</span><span class="sxs-lookup"><span data-stu-id="f71cc-181">Loading related data with Select</span></span>

<span data-ttu-id="f71cc-182">Le `OnGetAsync` méthode charge les données associées avec la `Include` méthode :</span><span class="sxs-lookup"><span data-stu-id="f71cc-182">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="f71cc-183">Le `Select` opérateur charge uniquement les données connexes nécessaires.</span><span class="sxs-lookup"><span data-stu-id="f71cc-183">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="f71cc-184">Pour les éléments uniques, comme le `Department.Name` qu’il utilise une jointure interne SQL.</span><span class="sxs-lookup"><span data-stu-id="f71cc-184">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="f71cc-185">Pour les regroupements qu’il utilise une autre base de données access, mais le.`Include`</span><span class="sxs-lookup"><span data-stu-id="f71cc-185">For collections it uses another database access, but so does the .`Include`</span></span> <span data-ttu-id="f71cc-186">opérateur sur des collections.</span><span class="sxs-lookup"><span data-stu-id="f71cc-186">operator on collections.</span></span>

<span data-ttu-id="f71cc-187">Le code suivant charge les données associées avec la `Select` méthode :</span><span class="sxs-lookup"><span data-stu-id="f71cc-187">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="f71cc-188">Le `CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="f71cc-188">The `CourseViewModel`:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="f71cc-189">Consultez [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) et [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) pour obtenir un exemple complet.</span><span class="sxs-lookup"><span data-stu-id="f71cc-189">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="f71cc-190">Créer une page de formateurs qui affiche des cours et des inscriptions</span><span class="sxs-lookup"><span data-stu-id="f71cc-190">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="f71cc-191">Dans cette section, la page instructeurs est créée.</span><span class="sxs-lookup"><span data-stu-id="f71cc-191">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="f71cc-192">![Page d’Index instructeurs](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="f71cc-192">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="f71cc-193">Cette page lit et affiche les données connexes comme suit :</span><span class="sxs-lookup"><span data-stu-id="f71cc-193">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="f71cc-194">La liste des instructeurs affiche les données connexes à partir de la `OfficeAssignment` entité (Office dans l’image précédente).</span><span class="sxs-lookup"><span data-stu-id="f71cc-194">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="f71cc-195">Le `Instructor` et `OfficeAssignment` sont des entités dans une relation un-à-zéro-ou-un.</span><span class="sxs-lookup"><span data-stu-id="f71cc-195">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="f71cc-196">Chargement hâtif est utilisé pour le `OfficeAssignment` entités.</span><span class="sxs-lookup"><span data-stu-id="f71cc-196">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="f71cc-197">Chargement hâtif est généralement plus efficace lorsque les données connexes doivent être affichée.</span><span class="sxs-lookup"><span data-stu-id="f71cc-197">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="f71cc-198">Dans ce cas, les affectations d’office pour les formateurs sont affichées.</span><span class="sxs-lookup"><span data-stu-id="f71cc-198">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="f71cc-199">Lorsque l’utilisateur sélectionne un formateur (Harui dans l’image précédente), lié `Course` les entités sont affichées.</span><span class="sxs-lookup"><span data-stu-id="f71cc-199">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="f71cc-200">Le `Instructor` et `Course` sont des entités dans une relation plusieurs-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="f71cc-200">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="f71cc-201">Eager de chargement pour le `Course` leur sont associées et les entités `Department` entités est utilisé.</span><span class="sxs-lookup"><span data-stu-id="f71cc-201">Eager loading for the `Course` entities and their related `Department` entities is used.</span></span> <span data-ttu-id="f71cc-202">Dans ce cas, les requêtes distinctes peuvent être plus efficaces, car seuls les cours pour l’enseignant sélectionné sont nécessaires.</span><span class="sxs-lookup"><span data-stu-id="f71cc-202">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="f71cc-203">Cet exemple montre comment utiliser un chargement hâtif pour les propriétés de navigation dans les entités qui se trouvent dans les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="f71cc-203">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="f71cc-204">Lorsque l’utilisateur sélectionne un cours (chimie dans l’image précédente), les données à partir d’inhérentes à la `Enrollments` entité est affichée.</span><span class="sxs-lookup"><span data-stu-id="f71cc-204">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="f71cc-205">Dans l’image précédente, la qualité et étudiant s’affichent.</span><span class="sxs-lookup"><span data-stu-id="f71cc-205">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="f71cc-206">Le `Course` et `Enrollment` sont des entités dans une relation un-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="f71cc-206">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="f71cc-207">Créer un modèle d’affichage pour l’affichage de l’Index de formateur</span><span class="sxs-lookup"><span data-stu-id="f71cc-207">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="f71cc-208">La page de formateurs affiche les données de trois tables différentes.</span><span class="sxs-lookup"><span data-stu-id="f71cc-208">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="f71cc-209">Un modèle d’affichage est créé qui inclut les trois entités représentant les trois tables.</span><span class="sxs-lookup"><span data-stu-id="f71cc-209">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="f71cc-210">Dans le *SchoolViewModels* dossier, créez *InstructorIndexData.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="f71cc-210">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="f71cc-211">Structure du modèle de formateur</span><span class="sxs-lookup"><span data-stu-id="f71cc-211">Scaffold the Instructor model</span></span>

* <span data-ttu-id="f71cc-212">Quittez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f71cc-212">Exit Visual Studio.</span></span>
* <span data-ttu-id="f71cc-213">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="f71cc-213">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="f71cc-214">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="f71cc-214">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

<span data-ttu-id="f71cc-215">Les structures de commande précédent la `Instructor` modèle.</span><span class="sxs-lookup"><span data-stu-id="f71cc-215">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="f71cc-216">Ouvrez le projet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f71cc-216">Open the project in Visual Studio.</span></span>

<span data-ttu-id="f71cc-217">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="f71cc-217">Build the project.</span></span> <span data-ttu-id="f71cc-218">La build génère des erreurs.</span><span class="sxs-lookup"><span data-stu-id="f71cc-218">The build generates errors.</span></span>

<span data-ttu-id="f71cc-219">Modifier globalement `_context.Instructor` à `_context.Instructors` (autrement dit, ajoutez un « s » pour `Instructor`).</span><span class="sxs-lookup"><span data-stu-id="f71cc-219">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="f71cc-220">7 occurrences sont trouvées et mis à jour.</span><span class="sxs-lookup"><span data-stu-id="f71cc-220">7 occurrences are found and updated.</span></span>

<span data-ttu-id="f71cc-221">Exécuter l’application et accédez à la page de formateurs.</span><span class="sxs-lookup"><span data-stu-id="f71cc-221">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="f71cc-222">Remplacez *Pages/Instructors/Index.cshtml.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="f71cc-222">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

<span data-ttu-id="f71cc-223">Le `OnGetAsync` méthode accepte les données d’itinéraire facultatif pour l’ID de l’enseignant sélectionné.</span><span class="sxs-lookup"><span data-stu-id="f71cc-223">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="f71cc-224">Examinez la requête sur le *Pages/Instructors/Index.cshtml* page :</span><span class="sxs-lookup"><span data-stu-id="f71cc-224">Examine the query on the *Pages/Instructors/Index.cshtml* page:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="f71cc-225">La requête comporte deux inclut :</span><span class="sxs-lookup"><span data-stu-id="f71cc-225">The query has two includes:</span></span>

* <span data-ttu-id="f71cc-226">`OfficeAssignment`: Affichée dans le [instructeurs vue](#IP).</span><span class="sxs-lookup"><span data-stu-id="f71cc-226">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="f71cc-227">`CourseAssignments`: Ce qui permet de bénéficier des dispensés.</span><span class="sxs-lookup"><span data-stu-id="f71cc-227">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="f71cc-228">Mise à jour de la page d’Index instructeurs</span><span class="sxs-lookup"><span data-stu-id="f71cc-228">Update the instructors Index page</span></span>

<span data-ttu-id="f71cc-229">Mise à jour *Pages/Instructors/Index.cshtml* par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="f71cc-229">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="f71cc-230">Le balisage précédent apporte les modifications suivantes :</span><span class="sxs-lookup"><span data-stu-id="f71cc-230">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="f71cc-231">Les mises à jour le `page` de `@page` à `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="f71cc-231">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="f71cc-232">`"{id:int?}"`est un modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="f71cc-232">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="f71cc-233">Le modèle d’itinéraire modifie les chaînes de requête entière dans l’URL pour les données d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="f71cc-233">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="f71cc-234">Par exemple, en cliquant sur le **sélectionnez** lien un formateur lorsque la directive de page génère une URL comme suit :</span><span class="sxs-lookup"><span data-stu-id="f71cc-234">For example, clicking on the **Select** link for an instructor when the page directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="f71cc-235">Lorsque la directive de page est `@page "{id:int?}"`, l’URL précédente est :</span><span class="sxs-lookup"><span data-stu-id="f71cc-235">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="f71cc-236">Titre de la page est **instructeurs**.</span><span class="sxs-lookup"><span data-stu-id="f71cc-236">Page title is **Instructors**.</span></span>
* <span data-ttu-id="f71cc-237">Ajouter un **Office** colonne affiche `item.OfficeAssignment.Location` uniquement si `item.OfficeAssignment` n’est pas null.</span><span class="sxs-lookup"><span data-stu-id="f71cc-237">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` is not null.</span></span> <span data-ttu-id="f71cc-238">Il s’agit d’une relation un-à-zéro-ou-un, il peuvent ne pas être une entité OfficeAssignment associée.</span><span class="sxs-lookup"><span data-stu-id="f71cc-238">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="f71cc-239">Ajouter un **cours** colonne qui affiche les cours dispensés par chaque formateur.</span><span class="sxs-lookup"><span data-stu-id="f71cc-239">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="f71cc-240">Consultez [explicite Transition ligne avec `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) pour plus d’informations sur cette syntaxe razor.</span><span class="sxs-lookup"><span data-stu-id="f71cc-240">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="f71cc-241">Code ajouté qui ajoute dynamiquement `class="success"` à la `tr` élément de l’enseignant sélectionné.</span><span class="sxs-lookup"><span data-stu-id="f71cc-241">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="f71cc-242">Cela définit la couleur d’arrière-plan de la ligne sélectionnée à l’aide d’une classe d’amorçage.</span><span class="sxs-lookup"><span data-stu-id="f71cc-242">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="f71cc-243">Ajouter un nouveau lien hypertexte étiqueté **sélectionnez**.</span><span class="sxs-lookup"><span data-stu-id="f71cc-243">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="f71cc-244">Ce lien envoie l’ID du formateur sélectionné à la `Index` méthode et définit une couleur d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="f71cc-244">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="f71cc-245">Exécutez l’application et sélectionnez le **instructeurs** onglet. La page affiche le `Location` (office) de la relation `OfficeAssignment` entité.</span><span class="sxs-lookup"><span data-stu-id="f71cc-245">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="f71cc-246">Si OfficeAssignment' est null, une cellule de table vide s’affiche.</span><span class="sxs-lookup"><span data-stu-id="f71cc-246">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Page d’Index instructeurs qu'aucun élément sélectionné](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="f71cc-248">Cliquez sur le **sélectionnez** lien.</span><span class="sxs-lookup"><span data-stu-id="f71cc-248">Click on the **Select** link.</span></span> <span data-ttu-id="f71cc-249">Les modifications de style de ligne.</span><span class="sxs-lookup"><span data-stu-id="f71cc-249">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="f71cc-250">Ajouter dispensés par un formateur sélectionné</span><span class="sxs-lookup"><span data-stu-id="f71cc-250">Add courses taught by selected instructor</span></span>

<span data-ttu-id="f71cc-251">Mise à jour la `OnGetAsync` méthode dans *Pages/Instructors/Index.cshtml.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="f71cc-251">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

<span data-ttu-id="f71cc-252">Examinez la requête de mise à jour :</span><span class="sxs-lookup"><span data-stu-id="f71cc-252">Examine the updated query:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="f71cc-253">La requête précédente ajoute le `Department` entités.</span><span class="sxs-lookup"><span data-stu-id="f71cc-253">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="f71cc-254">Le code suivant s’exécute lorsqu’un formateur est sélectionné (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="f71cc-254">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="f71cc-255">Le formateur sélectionné est récupéré à partir de la liste des formateurs dans le modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="f71cc-255">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="f71cc-256">Le modèle d’affichage `Courses` propriété est chargée avec le `Course` entités à partir de ce formateur `CourseAssignments` propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="f71cc-256">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="f71cc-257">Le `Where` méthode retourne une collection.</span><span class="sxs-lookup"><span data-stu-id="f71cc-257">The `Where` method returns a collection.</span></span> <span data-ttu-id="f71cc-258">Dans l’exemple précédent `Where` (méthode), un seul `Instructor` est retournée.</span><span class="sxs-lookup"><span data-stu-id="f71cc-258">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="f71cc-259">Le `Single` méthode convertit la collection en une seule `Instructor` entité.</span><span class="sxs-lookup"><span data-stu-id="f71cc-259">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="f71cc-260">Le `Instructor` entité fournit l’accès à la `CourseAssignments` propriété.</span><span class="sxs-lookup"><span data-stu-id="f71cc-260">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="f71cc-261">`CourseAssignments`fournit l’accès à la `Course` entités.</span><span class="sxs-lookup"><span data-stu-id="f71cc-261">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Formateur à cours m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="f71cc-263">Le `Single` méthode est utilisée sur une collection lorsque la collection comporte un seul élément.</span><span class="sxs-lookup"><span data-stu-id="f71cc-263">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="f71cc-264">Le `Single` méthode lève une exception si la collection est vide ou s’il existe plusieurs éléments.</span><span class="sxs-lookup"><span data-stu-id="f71cc-264">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="f71cc-265">Une alternative est `SingleOrDefault`, qui retourne une valeur par défaut (null dans ce cas) si la collection est vide.</span><span class="sxs-lookup"><span data-stu-id="f71cc-265">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="f71cc-266">À l’aide de `SingleOrDefault` sur une collection vide :</span><span class="sxs-lookup"><span data-stu-id="f71cc-266">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="f71cc-267">Une exception se produit (tente de trouver un `Courses` propriété sur une référence null).</span><span class="sxs-lookup"><span data-stu-id="f71cc-267">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="f71cc-268">Le message d’exception indique moins clairement la cause du problème.</span><span class="sxs-lookup"><span data-stu-id="f71cc-268">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="f71cc-269">Le code suivant remplit le modèle d’affichage `Enrollments` propriété lorsqu’un cours est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="f71cc-269">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="f71cc-270">Ajoutez le balisage suivant à la fin de la *Pages/Courses/Index.cshtml* Page Razor :</span><span class="sxs-lookup"><span data-stu-id="f71cc-270">Add the following markup to the end of the *Pages/Courses/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

<span data-ttu-id="f71cc-271">La balise précédente affiche une liste de cours liés à un formateur lorsqu’un formateur est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="f71cc-271">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="f71cc-272">Tester l’application.</span><span class="sxs-lookup"><span data-stu-id="f71cc-272">Test the app.</span></span> <span data-ttu-id="f71cc-273">Cliquez sur un **sélectionnez** lien dans la page de formateurs.</span><span class="sxs-lookup"><span data-stu-id="f71cc-273">Click on a **Select** link on the instructors page.</span></span>

![Formateur de page d’Index instructeurs sélectionné](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="f71cc-275">Afficher les données de l’étudiant</span><span class="sxs-lookup"><span data-stu-id="f71cc-275">Show student data</span></span>

<span data-ttu-id="f71cc-276">Dans cette section, l’application est mise à jour pour afficher les données de l’étudiant pour le cours sélectionné.</span><span class="sxs-lookup"><span data-stu-id="f71cc-276">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="f71cc-277">Mettre à jour de la requête dans le `OnGetAsync` méthode dans *Pages/Instructors/Index.cshtml.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="f71cc-277">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="f71cc-278">Mise à jour *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f71cc-278">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="f71cc-279">Ajoutez le balisage suivant à la fin du fichier :</span><span class="sxs-lookup"><span data-stu-id="f71cc-279">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="f71cc-280">La balise précédente affiche une liste des étudiants qui sont inscrits dans le cours sélectionné.</span><span class="sxs-lookup"><span data-stu-id="f71cc-280">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="f71cc-281">Actualisez la page et sélectionner un formateur.</span><span class="sxs-lookup"><span data-stu-id="f71cc-281">Refresh the page and select an instructor.</span></span> <span data-ttu-id="f71cc-282">Sélectionnez un cours pour afficher la liste des étudiants inscrits et leurs catégories.</span><span class="sxs-lookup"><span data-stu-id="f71cc-282">Select a course to see the list of enrolled students and their grades.</span></span>

![Formateur de page d’Index instructeurs et cours sélectionné](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="f71cc-284">À l’aide d’unique</span><span class="sxs-lookup"><span data-stu-id="f71cc-284">Using Single</span></span>

<span data-ttu-id="f71cc-285">Le `Single` méthode peut passer le `Where` condition au lieu d’appeler le `Where` méthode séparément :</span><span class="sxs-lookup"><span data-stu-id="f71cc-285">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="f71cc-286">L’exemple précédent `Single` approche ne présente aucun avantage principal à l’aide `Where`.</span><span class="sxs-lookup"><span data-stu-id="f71cc-286">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="f71cc-287">Certains développeurs préfèrent les `Single` approche de style.</span><span class="sxs-lookup"><span data-stu-id="f71cc-287">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="f71cc-288">Chargement explicite</span><span class="sxs-lookup"><span data-stu-id="f71cc-288">Explicit loading</span></span>

<span data-ttu-id="f71cc-289">Le code actuel spécifie un chargement hâtif pour `Enrollments` et `Students`:</span><span class="sxs-lookup"><span data-stu-id="f71cc-289">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="f71cc-290">Supposons que les utilisateurs souhaitent rarement voir les inscriptions dans un cours.</span><span class="sxs-lookup"><span data-stu-id="f71cc-290">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="f71cc-291">Dans ce cas, une optimisation serait pour charger uniquement les données d’inscription si elle est demandée.</span><span class="sxs-lookup"><span data-stu-id="f71cc-291">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="f71cc-292">Dans cette section, le `OnGetAsync` est mis à jour pour utiliser le chargement explicite de `Enrollments` et `Students`.</span><span class="sxs-lookup"><span data-stu-id="f71cc-292">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="f71cc-293">Mise à jour le `OnGetAsync` par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="f71cc-293">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="f71cc-294">Le code précédent supprime le *ThenInclude* des appels de méthode pour les données d’inscription et les étudiants.</span><span class="sxs-lookup"><span data-stu-id="f71cc-294">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="f71cc-295">Si un cours est sélectionné, le code en surbrillance récupère :</span><span class="sxs-lookup"><span data-stu-id="f71cc-295">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="f71cc-296">Le `Enrollment` entités pour le cours sélectionné.</span><span class="sxs-lookup"><span data-stu-id="f71cc-296">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="f71cc-297">Le `Student` entités pour chaque `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="f71cc-297">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="f71cc-298">Notez l’exemple précédent commentaire de code `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="f71cc-298">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="f71cc-299">Propriétés de navigation peuvent uniquement être chargées explicitement pour les entités de suivi.</span><span class="sxs-lookup"><span data-stu-id="f71cc-299">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="f71cc-300">Tester l’application.</span><span class="sxs-lookup"><span data-stu-id="f71cc-300">Test the app.</span></span> <span data-ttu-id="f71cc-301">Du point de vue des utilisateurs, l’application se comporte comme la version précédente.</span><span class="sxs-lookup"><span data-stu-id="f71cc-301">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="f71cc-302">Le didacticiel suivant montre comment mettre à jour les données associées.</span><span class="sxs-lookup"><span data-stu-id="f71cc-302">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="f71cc-303">[Précédent](xref:data/ef-rp/complex-data-model)
>[Suivant](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="f71cc-303">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>