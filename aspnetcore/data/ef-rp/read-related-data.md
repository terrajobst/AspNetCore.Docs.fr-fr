---
title: "Pages Razor avec EF Core - lire les données associées - 6 de 8"
author: rick-anderson
description: "Dans ce didacticiel vous lire et afficher les données associées, autrement dit, les données Entity Framework charge dans les propriétés de navigation."
ms.author: riande
manager: wpickett
ms.date: 11/05/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/read-related-data
ms.openlocfilehash: d0cdb5aaa4b1129c3f2404d069e9781ca16260b7
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a><span data-ttu-id="040f4-103">Lecture liés de données - Core EF avec les Pages Razor (6 de 8)</span><span class="sxs-lookup"><span data-stu-id="040f4-103">Reading related data - EF Core with Razor Pages (6 of 8)</span></span>

<span data-ttu-id="040f4-104">Par [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="040f4-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="040f4-105">Dans ce didacticiel, les données associées sont lues et affichées.</span><span class="sxs-lookup"><span data-stu-id="040f4-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="040f4-106">Les données associées sont données EF Core charge dans les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="040f4-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="040f4-107">Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, téléchargez le [application terminée pour cette étape](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span><span class="sxs-lookup"><span data-stu-id="040f4-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="040f4-108">Les illustrations suivantes montrent les pages terminées pour ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="040f4-108">The following illustrations show the completed pages for this tutorial:</span></span>

![Page d’Index de cours](read-related-data/_static/courses-index.png)

![Page d’Index instructeurs](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="040f4-111">Chargement hâtif, explicit et différé de données connexes</span><span class="sxs-lookup"><span data-stu-id="040f4-111">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="040f4-112">Il existe plusieurs façons que EF Core chargez des données connexes dans les propriétés de navigation d’une entité :</span><span class="sxs-lookup"><span data-stu-id="040f4-112">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="040f4-113">[Chargement hâtif](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="040f4-113">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="040f4-114">Chargement hâtif est lorsqu’une requête pour un type d’entité charge également des entités associées.</span><span class="sxs-lookup"><span data-stu-id="040f4-114">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="040f4-115">Lors de la lecture de l’entité, ses données associées sont récupérées.</span><span class="sxs-lookup"><span data-stu-id="040f4-115">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="040f4-116">Cela entraîne généralement une requête de jointure unique qui extrait toutes les données que nécessaire.</span><span class="sxs-lookup"><span data-stu-id="040f4-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="040f4-117">EF Core émet plusieurs requêtes pour certains types de chargement hâtif.</span><span class="sxs-lookup"><span data-stu-id="040f4-117">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="040f4-118">Émission de requêtes multiples peut être plus efficace qu’était le cas pour certaines requêtes de EF6 où il a une seule requête.</span><span class="sxs-lookup"><span data-stu-id="040f4-118">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="040f4-119">Chargement hâtif est spécifié avec la `Include` et `ThenInclude` méthodes.</span><span class="sxs-lookup"><span data-stu-id="040f4-119">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

 ![Exemple de chargement hâtif](read-related-data/_static/eager-loading.png)
 
 <span data-ttu-id="040f4-121">Chargement hâtif envoie plusieurs requêtes lorsqu’une collection de nvavigation est incluse :</span><span class="sxs-lookup"><span data-stu-id="040f4-121">Eager loading sends multiple queries when a collection nvavigation is included:</span></span>

 * <span data-ttu-id="040f4-122">Une requête pour la requête principale</span><span class="sxs-lookup"><span data-stu-id="040f4-122">One query for the main query</span></span> 
 * <span data-ttu-id="040f4-123">Une requête pour chaque collection de « session » dans l’arborescence de la charge.</span><span class="sxs-lookup"><span data-stu-id="040f4-123">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="040f4-124">Séparer les requêtes avec `Load`: les données peuvent être récupérées dans des requêtes distinctes et EF Core » résout « les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="040f4-124">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="040f4-125">signifie « correctifs des » que EF Core remplit automatiquement les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="040f4-125">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="040f4-126">Séparer les requêtes avec `Load` est plus explicite de chargement chargement hâtif.</span><span class="sxs-lookup"><span data-stu-id="040f4-126">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

 ![Exemple de requêtes distinctes](read-related-data/_static/separate-queries.png)

 <span data-ttu-id="040f4-128">Remarque : EF Core résout automatiquement les propriétés de navigation pour toutes les autres entités qui ont été précédemment chargées dans l’instance de contexte.</span><span class="sxs-lookup"><span data-stu-id="040f4-128">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="040f4-129">Même si les données pour une propriété de navigation seront *pas* explicitement inclus, la propriété peut toujours être remplie si certaines ou toutes les entités connexes ont été précédemment chargées.</span><span class="sxs-lookup"><span data-stu-id="040f4-129">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="040f4-130">[Chargement explicite](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="040f4-130">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="040f4-131">Lors de la lecture de l’entité est tout d’abord, les données associées n’est pas récupérées.</span><span class="sxs-lookup"><span data-stu-id="040f4-131">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="040f4-132">Code doit être écrit pour récupérer les données associées lorsque cela est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="040f4-132">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="040f4-133">Chargement explicite avec des requêtes distinctes entraîne plusieurs requêtes envoyées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="040f4-133">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="040f4-134">Avec le chargement explicite, le code spécifie les propriétés de navigation doivent être chargées.</span><span class="sxs-lookup"><span data-stu-id="040f4-134">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="040f4-135">Utilisez la `Load` méthode pour effectuer le chargement explicite.</span><span class="sxs-lookup"><span data-stu-id="040f4-135">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="040f4-136">Exemple :</span><span class="sxs-lookup"><span data-stu-id="040f4-136">For example:</span></span>

 ![Exemple de chargement explicite](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="040f4-138">[Chargement différé](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="040f4-138">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="040f4-139">[EF Core ne prend pas en charge le chargement tardif](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="040f4-139">[EF Core does not currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="040f4-140">Lors de la lecture de l’entité est tout d’abord, les données associées n’est pas récupérées.</span><span class="sxs-lookup"><span data-stu-id="040f4-140">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="040f4-141">La première fois qu’une propriété de navigation est accessible, les données requises pour cette propriété de navigation sont automatiquement récupérées.</span><span class="sxs-lookup"><span data-stu-id="040f4-141">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="040f4-142">Une requête est envoyée à la base de données chaque fois qu’une propriété de navigation est accessible pour la première fois.</span><span class="sxs-lookup"><span data-stu-id="040f4-142">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="040f4-143">Le `Select` opérateur charge uniquement les données connexes nécessaires.</span><span class="sxs-lookup"><span data-stu-id="040f4-143">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="040f4-144">Créer une page de cours qui affiche le nom du service</span><span class="sxs-lookup"><span data-stu-id="040f4-144">Create a Courses page that displays department name</span></span>

<span data-ttu-id="040f4-145">L’entité de cours inclut une propriété de navigation qui contient le `Department` entité.</span><span class="sxs-lookup"><span data-stu-id="040f4-145">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="040f4-146">Le `Department` entité contient le service auquel appartient le cours.</span><span class="sxs-lookup"><span data-stu-id="040f4-146">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="040f4-147">Pour afficher le nom du service affecté dans une liste de cours :</span><span class="sxs-lookup"><span data-stu-id="040f4-147">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="040f4-148">Obtenir le `Name` propriété à partir de la `Department` entité.</span><span class="sxs-lookup"><span data-stu-id="040f4-148">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="040f4-149">Le `Department` provient de l’entité la `Course.Department` propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="040f4-149">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![ourse. Service](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="040f4-151">Structure du modèle de cours</span><span class="sxs-lookup"><span data-stu-id="040f4-151">Scaffold the Course model</span></span>

* <span data-ttu-id="040f4-152">Quittez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="040f4-152">Exit Visual Studio.</span></span>
* <span data-ttu-id="040f4-153">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="040f4-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="040f4-154">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="040f4-154">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

<span data-ttu-id="040f4-155">Les structures de commande précédent la `Course` modèle.</span><span class="sxs-lookup"><span data-stu-id="040f4-155">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="040f4-156">Ouvrez le projet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="040f4-156">Open the project in Visual Studio.</span></span>

<span data-ttu-id="040f4-157">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="040f4-157">Build the project.</span></span> <span data-ttu-id="040f4-158">La build génère des erreurs comme suit :</span><span class="sxs-lookup"><span data-stu-id="040f4-158">The build generates errors like the following:</span></span>

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="040f4-159">Modifier globalement `_context.Course` à `_context.Courses` (autrement dit, ajoutez un « s » pour `Course`).</span><span class="sxs-lookup"><span data-stu-id="040f4-159">Globally change `_context.Course` to `_context.Courses` (that is, add an "s" to `Course`).</span></span> <span data-ttu-id="040f4-160">7 occurrences sont trouvées et mis à jour.</span><span class="sxs-lookup"><span data-stu-id="040f4-160">7 occurrences are found and updated.</span></span>

<span data-ttu-id="040f4-161">Ouvrez *Pages/Courses/Index.cshtml.cs* et examinez le `OnGetAsync` (méthode).</span><span class="sxs-lookup"><span data-stu-id="040f4-161">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="040f4-162">Le moteur de génération de modèles automatique spécifié pour un chargement hâtif le `Department` propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="040f4-162">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="040f4-163">Le `Include` méthode spécifie un chargement hâtif.</span><span class="sxs-lookup"><span data-stu-id="040f4-163">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="040f4-164">Exécutez l’application et sélectionnez le **cours** lien.</span><span class="sxs-lookup"><span data-stu-id="040f4-164">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="040f4-165">La colonne Service affiche le `DepartmentID`, ce qui n’est pas utile.</span><span class="sxs-lookup"><span data-stu-id="040f4-165">The department column displays the `DepartmentID`, which is not useful.</span></span>

<span data-ttu-id="040f4-166">Mettez à jour la méthode `OnGetAsync` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="040f4-166">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="040f4-167">Le code précédent ajoute `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="040f4-167">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="040f4-168">`AsNoTracking`améliore les performances, car les entités retournées ne sont pas suivies.</span><span class="sxs-lookup"><span data-stu-id="040f4-168">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="040f4-169">Les entités ne sont pas suivies, car ils ne sont pas mis à jour dans le contexte actuel.</span><span class="sxs-lookup"><span data-stu-id="040f4-169">The entities are not tracked because they are not updated in the current context.</span></span>

<span data-ttu-id="040f4-170">Mise à jour *Views/Courses/Index.cshtml* avec le balisage mis en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="040f4-170">Update *Views/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="040f4-171">Les modifications suivantes ont été apportées au code de modèle généré automatiquement :</span><span class="sxs-lookup"><span data-stu-id="040f4-171">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="040f4-172">Modifié le titre de l’Index au cours.</span><span class="sxs-lookup"><span data-stu-id="040f4-172">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="040f4-173">Ajouter un **nombre** colonne qui affiche le `CourseID` valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="040f4-173">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="040f4-174">Par défaut, les clés primaires ne sont pas structurés, car ceux-ci ne sont généralement pas de sens pour les utilisateurs finaux.</span><span class="sxs-lookup"><span data-stu-id="040f4-174">By default, primary keys aren't scaffolded because normally they are meaningless to end users.</span></span> <span data-ttu-id="040f4-175">Toutefois, dans ce cas la clé primaire est significative.</span><span class="sxs-lookup"><span data-stu-id="040f4-175">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="040f4-176">Modifié le **service** colonne pour afficher le nom du service.</span><span class="sxs-lookup"><span data-stu-id="040f4-176">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="040f4-177">Le code affiche le `Name` propriété de la `Department` entité qui est chargée dans le `Department` propriété de navigation :</span><span class="sxs-lookup"><span data-stu-id="040f4-177">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="040f4-178">Exécutez l’application et sélectionnez le **cours** onglet pour afficher la liste avec les noms de service.</span><span class="sxs-lookup"><span data-stu-id="040f4-178">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Page d’Index de cours](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="040f4-180">Chargement liés de données avec Select</span><span class="sxs-lookup"><span data-stu-id="040f4-180">Loading related data with Select</span></span>

<span data-ttu-id="040f4-181">Le `OnGetAsync` méthode charge les données associées avec la `Include` méthode :</span><span class="sxs-lookup"><span data-stu-id="040f4-181">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="040f4-182">Le `Select` opérateur charge uniquement les données connexes nécessaires.</span><span class="sxs-lookup"><span data-stu-id="040f4-182">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="040f4-183">Pour les éléments uniques, comme le `Department.Name` qu’il utilise une jointure interne SQL.</span><span class="sxs-lookup"><span data-stu-id="040f4-183">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="040f4-184">Pour les regroupements qu’il utilise une autre base de données access, mais le.`Include`</span><span class="sxs-lookup"><span data-stu-id="040f4-184">For collections it uses another database access, but so does the .`Include`</span></span> <span data-ttu-id="040f4-185">opérateur sur des collections.</span><span class="sxs-lookup"><span data-stu-id="040f4-185">operator on collections.</span></span>

<span data-ttu-id="040f4-186">Le code suivant charge les données associées avec la `Select` méthode :</span><span class="sxs-lookup"><span data-stu-id="040f4-186">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="040f4-187">Le `CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="040f4-187">The `CourseViewModel`:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="040f4-188">Consultez [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) et [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) pour obtenir un exemple complet.</span><span class="sxs-lookup"><span data-stu-id="040f4-188">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="040f4-189">Créer une page de formateurs qui affiche des cours et des inscriptions</span><span class="sxs-lookup"><span data-stu-id="040f4-189">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="040f4-190">Dans cette section, la page instructeurs est créée.</span><span class="sxs-lookup"><span data-stu-id="040f4-190">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="040f4-191">![Page d’Index instructeurs](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="040f4-191">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="040f4-192">Cette page lit et affiche les données connexes comme suit :</span><span class="sxs-lookup"><span data-stu-id="040f4-192">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="040f4-193">La liste des instructeurs affiche les données connexes à partir de la `OfficeAssignment` entité (Office dans l’image précédente).</span><span class="sxs-lookup"><span data-stu-id="040f4-193">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="040f4-194">Le `Instructor` et `OfficeAssignment` sont des entités dans une relation un-à-zéro-ou-un.</span><span class="sxs-lookup"><span data-stu-id="040f4-194">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="040f4-195">Chargement hâtif est utilisé pour le `OfficeAssignment` entités.</span><span class="sxs-lookup"><span data-stu-id="040f4-195">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="040f4-196">Chargement hâtif est généralement plus efficace lorsque les données connexes doivent être affichée.</span><span class="sxs-lookup"><span data-stu-id="040f4-196">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="040f4-197">Dans ce cas, les affectations d’office pour les formateurs sont affichées.</span><span class="sxs-lookup"><span data-stu-id="040f4-197">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="040f4-198">Lorsque l’utilisateur sélectionne un formateur (Harui dans l’image précédente), lié `Course` les entités sont affichées.</span><span class="sxs-lookup"><span data-stu-id="040f4-198">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="040f4-199">Le `Instructor` et `Course` sont des entités dans une relation plusieurs-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="040f4-199">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="040f4-200">Eager de chargement pour le `Course` leur sont associées et les entités `Department` entités est utilisé.</span><span class="sxs-lookup"><span data-stu-id="040f4-200">Eager loading for the `Course` entities and their related `Department` entities is used.</span></span> <span data-ttu-id="040f4-201">Dans ce cas, les requêtes distinctes peuvent être plus efficaces, car seuls les cours pour l’enseignant sélectionné sont nécessaires.</span><span class="sxs-lookup"><span data-stu-id="040f4-201">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="040f4-202">Cet exemple montre comment utiliser un chargement hâtif pour les propriétés de navigation dans les entités qui se trouvent dans les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="040f4-202">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="040f4-203">Lorsque l’utilisateur sélectionne un cours (chimie dans l’image précédente), les données à partir d’inhérentes à la `Enrollments` entité est affichée.</span><span class="sxs-lookup"><span data-stu-id="040f4-203">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="040f4-204">Dans l’image précédente, la qualité et étudiant s’affichent.</span><span class="sxs-lookup"><span data-stu-id="040f4-204">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="040f4-205">Le `Course` et `Enrollment` sont des entités dans une relation un-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="040f4-205">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="040f4-206">Créer un modèle d’affichage pour l’affichage de l’Index de formateur</span><span class="sxs-lookup"><span data-stu-id="040f4-206">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="040f4-207">La page de formateurs affiche les données de trois tables différentes.</span><span class="sxs-lookup"><span data-stu-id="040f4-207">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="040f4-208">Un modèle d’affichage est créé qui inclut les trois entités représentant les trois tables.</span><span class="sxs-lookup"><span data-stu-id="040f4-208">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="040f4-209">Dans le *SchoolViewModels* dossier, créez *InstructorIndexData.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="040f4-209">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="040f4-210">Structure du modèle de formateur</span><span class="sxs-lookup"><span data-stu-id="040f4-210">Scaffold the Instructor model</span></span>

* <span data-ttu-id="040f4-211">Quittez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="040f4-211">Exit Visual Studio.</span></span>
* <span data-ttu-id="040f4-212">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="040f4-212">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="040f4-213">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="040f4-213">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

<span data-ttu-id="040f4-214">Les structures de commande précédent la `Instructor` modèle.</span><span class="sxs-lookup"><span data-stu-id="040f4-214">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="040f4-215">Ouvrez le projet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="040f4-215">Open the project in Visual Studio.</span></span>

<span data-ttu-id="040f4-216">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="040f4-216">Build the project.</span></span> <span data-ttu-id="040f4-217">La build génère des erreurs.</span><span class="sxs-lookup"><span data-stu-id="040f4-217">The build generates errors.</span></span>

<span data-ttu-id="040f4-218">Modifier globalement `_context.Instructor` à `_context.Instructors` (autrement dit, ajoutez un « s » pour `Instructor`).</span><span class="sxs-lookup"><span data-stu-id="040f4-218">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="040f4-219">7 occurrences sont trouvées et mis à jour.</span><span class="sxs-lookup"><span data-stu-id="040f4-219">7 occurrences are found and updated.</span></span>

<span data-ttu-id="040f4-220">Exécuter l’application et accédez à la page de formateurs.</span><span class="sxs-lookup"><span data-stu-id="040f4-220">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="040f4-221">Remplacez *Pages/Instructors/Index.cshtml.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="040f4-221">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

<span data-ttu-id="040f4-222">Le `OnGetAsync` méthode accepte les données d’itinéraire facultatif pour l’ID de l’enseignant sélectionné.</span><span class="sxs-lookup"><span data-stu-id="040f4-222">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="040f4-223">Examinez la requête sur le *Pages/Instructors/Index.cshtml* page :</span><span class="sxs-lookup"><span data-stu-id="040f4-223">Examine the query on the *Pages/Instructors/Index.cshtml* page:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="040f4-224">La requête comporte deux inclut :</span><span class="sxs-lookup"><span data-stu-id="040f4-224">The query has two includes:</span></span>

* <span data-ttu-id="040f4-225">`OfficeAssignment`: Affichée dans le [instructeurs vue](#IP).</span><span class="sxs-lookup"><span data-stu-id="040f4-225">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="040f4-226">`CourseAssignments`: Ce qui permet de bénéficier des dispensés.</span><span class="sxs-lookup"><span data-stu-id="040f4-226">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="040f4-227">Mise à jour de la page d’Index instructeurs</span><span class="sxs-lookup"><span data-stu-id="040f4-227">Update the instructors Index page</span></span>

<span data-ttu-id="040f4-228">Mise à jour *Pages/Instructors/Index.cshtml* par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="040f4-228">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="040f4-229">Le balisage précédent apporte les modifications suivantes :</span><span class="sxs-lookup"><span data-stu-id="040f4-229">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="040f4-230">Les mises à jour le `page` de `@page` à `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="040f4-230">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="040f4-231">`"{id:int?}"`est un modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="040f4-231">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="040f4-232">Le modèle d’itinéraire modifie les chaînes de requête entière dans l’URL pour les données d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="040f4-232">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="040f4-233">Par exemple, en cliquant sur le **sélectionnez** lien un formateur lorsque la directive de page génère une URL comme suit :</span><span class="sxs-lookup"><span data-stu-id="040f4-233">For example, clicking on the **Select** link for an instructor when the page directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="040f4-234">Lorsque la directive de page est `@page "{id:int?}"`, l’URL précédente est :</span><span class="sxs-lookup"><span data-stu-id="040f4-234">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="040f4-235">Titre de la page est **instructeurs**.</span><span class="sxs-lookup"><span data-stu-id="040f4-235">Page title is **Instructors**.</span></span>
* <span data-ttu-id="040f4-236">Ajouter un **Office** colonne affiche `item.OfficeAssignment.Location` uniquement si `item.OfficeAssignment` n’est pas null.</span><span class="sxs-lookup"><span data-stu-id="040f4-236">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` is not null.</span></span> <span data-ttu-id="040f4-237">Il s’agit d’une relation un-à-zéro-ou-un, il peuvent ne pas être une entité OfficeAssignment associée.</span><span class="sxs-lookup"><span data-stu-id="040f4-237">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="040f4-238">Ajouter un **cours** colonne qui affiche les cours dispensés par chaque formateur.</span><span class="sxs-lookup"><span data-stu-id="040f4-238">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="040f4-239">Consultez [explicite Transition ligne avec `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) pour plus d’informations sur cette syntaxe razor.</span><span class="sxs-lookup"><span data-stu-id="040f4-239">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="040f4-240">Code ajouté qui ajoute dynamiquement `class="success"` à la `tr` élément de l’enseignant sélectionné.</span><span class="sxs-lookup"><span data-stu-id="040f4-240">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="040f4-241">Cela définit la couleur d’arrière-plan de la ligne sélectionnée à l’aide d’une classe d’amorçage.</span><span class="sxs-lookup"><span data-stu-id="040f4-241">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="040f4-242">Ajouter un nouveau lien hypertexte étiqueté **sélectionnez**.</span><span class="sxs-lookup"><span data-stu-id="040f4-242">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="040f4-243">Ce lien envoie l’ID du formateur sélectionné à la `Index` méthode et définit une couleur d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="040f4-243">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="040f4-244">Exécutez l’application et sélectionnez le **instructeurs** onglet. La page affiche le `Location` (office) de la relation `OfficeAssignment` entité.</span><span class="sxs-lookup"><span data-stu-id="040f4-244">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="040f4-245">Si OfficeAssignment' est null, une cellule de table vide s’affiche.</span><span class="sxs-lookup"><span data-stu-id="040f4-245">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Page d’Index instructeurs qu'aucun élément sélectionné](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="040f4-247">Cliquez sur le **sélectionnez** lien.</span><span class="sxs-lookup"><span data-stu-id="040f4-247">Click on the **Select** link.</span></span> <span data-ttu-id="040f4-248">Les modifications de style de ligne.</span><span class="sxs-lookup"><span data-stu-id="040f4-248">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="040f4-249">Ajouter dispensés par un formateur sélectionné</span><span class="sxs-lookup"><span data-stu-id="040f4-249">Add courses taught by selected instructor</span></span>

<span data-ttu-id="040f4-250">Mise à jour la `OnGetAsync` méthode dans *Pages/Instructors/Index.cshtml.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="040f4-250">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

<span data-ttu-id="040f4-251">Examinez la requête de mise à jour :</span><span class="sxs-lookup"><span data-stu-id="040f4-251">Examine the updated query:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="040f4-252">La requête précédente ajoute le `Department` entités.</span><span class="sxs-lookup"><span data-stu-id="040f4-252">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="040f4-253">Le code suivant s’exécute lorsqu’un formateur est sélectionné (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="040f4-253">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="040f4-254">Le formateur sélectionné est récupéré à partir de la liste des formateurs dans le modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="040f4-254">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="040f4-255">Le modèle d’affichage `Courses` propriété est chargée avec le `Course` entités à partir de ce formateur `CourseAssignments` propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="040f4-255">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="040f4-256">Le `Where` méthode retourne une collection.</span><span class="sxs-lookup"><span data-stu-id="040f4-256">The `Where` method returns a collection.</span></span> <span data-ttu-id="040f4-257">Dans l’exemple précédent `Where` (méthode), un seul `Instructor` est retournée.</span><span class="sxs-lookup"><span data-stu-id="040f4-257">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="040f4-258">Le `Single` méthode convertit la collection en une seule `Instructor` entité.</span><span class="sxs-lookup"><span data-stu-id="040f4-258">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="040f4-259">Le `Instructor` entité fournit l’accès à la `CourseAssignments` propriété.</span><span class="sxs-lookup"><span data-stu-id="040f4-259">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="040f4-260">`CourseAssignments`fournit l’accès à la `Course` entités.</span><span class="sxs-lookup"><span data-stu-id="040f4-260">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Formateur à cours m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="040f4-262">Le `Single` méthode est utilisée sur une collection lorsque la collection comporte un seul élément.</span><span class="sxs-lookup"><span data-stu-id="040f4-262">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="040f4-263">Le `Single` méthode lève une exception si la collection est vide ou s’il existe plusieurs éléments.</span><span class="sxs-lookup"><span data-stu-id="040f4-263">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="040f4-264">Une alternative est `SingleOrDefault`, qui retourne une valeur par défaut (null dans ce cas) si la collection est vide.</span><span class="sxs-lookup"><span data-stu-id="040f4-264">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="040f4-265">À l’aide de `SingleOrDefault` sur une collection vide :</span><span class="sxs-lookup"><span data-stu-id="040f4-265">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="040f4-266">Une exception se produit (tente de trouver un `Courses` propriété sur une référence null).</span><span class="sxs-lookup"><span data-stu-id="040f4-266">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="040f4-267">Le message d’exception indique moins clairement la cause du problème.</span><span class="sxs-lookup"><span data-stu-id="040f4-267">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="040f4-268">Le code suivant remplit le modèle d’affichage `Enrollments` propriété lorsqu’un cours est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="040f4-268">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="040f4-269">Ajoutez le balisage suivant à la fin de la *Pages/Courses/Index.cshtml* Page Razor :</span><span class="sxs-lookup"><span data-stu-id="040f4-269">Add the following markup to the end of the *Pages/Courses/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

<span data-ttu-id="040f4-270">La balise précédente affiche une liste de cours liés à un formateur lorsqu’un formateur est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="040f4-270">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="040f4-271">Tester l’application.</span><span class="sxs-lookup"><span data-stu-id="040f4-271">Test the app.</span></span> <span data-ttu-id="040f4-272">Cliquez sur un **sélectionnez** lien dans la page de formateurs.</span><span class="sxs-lookup"><span data-stu-id="040f4-272">Click on a **Select** link on the instructors page.</span></span>

![Formateur de page d’Index instructeurs sélectionné](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="040f4-274">Afficher les données de l’étudiant</span><span class="sxs-lookup"><span data-stu-id="040f4-274">Show student data</span></span>

<span data-ttu-id="040f4-275">Dans cette section, l’application est mise à jour pour afficher les données de l’étudiant pour le cours sélectionné.</span><span class="sxs-lookup"><span data-stu-id="040f4-275">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="040f4-276">Mettre à jour de la requête dans le `OnGetAsync` méthode dans *Pages/Instructors/Index.cshtml.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="040f4-276">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="040f4-277">Mise à jour *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="040f4-277">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="040f4-278">Ajoutez le balisage suivant à la fin du fichier :</span><span class="sxs-lookup"><span data-stu-id="040f4-278">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="040f4-279">La balise précédente affiche une liste des étudiants qui sont inscrits dans le cours sélectionné.</span><span class="sxs-lookup"><span data-stu-id="040f4-279">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="040f4-280">Actualisez la page et sélectionner un formateur.</span><span class="sxs-lookup"><span data-stu-id="040f4-280">Refresh the page and select an instructor.</span></span> <span data-ttu-id="040f4-281">Sélectionnez un cours pour afficher la liste des étudiants inscrits et leurs catégories.</span><span class="sxs-lookup"><span data-stu-id="040f4-281">Select a course to see the list of enrolled students and their grades.</span></span>

![Formateur de page d’Index instructeurs et cours sélectionné](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="040f4-283">À l’aide d’unique</span><span class="sxs-lookup"><span data-stu-id="040f4-283">Using Single</span></span>

<span data-ttu-id="040f4-284">Le `Single` méthode peut passer le `Where` condition au lieu d’appeler le `Where` méthode séparément :</span><span class="sxs-lookup"><span data-stu-id="040f4-284">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="040f4-285">L’exemple précédent `Single` approche ne présente aucun avantage principal à l’aide `Where`.</span><span class="sxs-lookup"><span data-stu-id="040f4-285">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="040f4-286">Certains développeurs préfèrent les `Single` approche de style.</span><span class="sxs-lookup"><span data-stu-id="040f4-286">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="040f4-287">Chargement explicite</span><span class="sxs-lookup"><span data-stu-id="040f4-287">Explicit loading</span></span>

<span data-ttu-id="040f4-288">Le code actuel spécifie un chargement hâtif pour `Enrollments` et `Students`:</span><span class="sxs-lookup"><span data-stu-id="040f4-288">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="040f4-289">Supposons que les utilisateurs souhaitent rarement voir les inscriptions dans un cours.</span><span class="sxs-lookup"><span data-stu-id="040f4-289">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="040f4-290">Dans ce cas, une optimisation serait pour charger uniquement les données d’inscription si elle est demandée.</span><span class="sxs-lookup"><span data-stu-id="040f4-290">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="040f4-291">Dans cette section, le `OnGetAsync` est mis à jour pour utiliser le chargement explicite de `Enrollments` et `Students`.</span><span class="sxs-lookup"><span data-stu-id="040f4-291">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="040f4-292">Mise à jour le `OnGetAsync` par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="040f4-292">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="040f4-293">Le code précédent supprime le *ThenInclude* des appels de méthode pour les données d’inscription et les étudiants.</span><span class="sxs-lookup"><span data-stu-id="040f4-293">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="040f4-294">Si un cours est sélectionné, le code en surbrillance récupère :</span><span class="sxs-lookup"><span data-stu-id="040f4-294">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="040f4-295">Le `Enrollment` entités pour le cours sélectionné.</span><span class="sxs-lookup"><span data-stu-id="040f4-295">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="040f4-296">Le `Student` entités pour chaque `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="040f4-296">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="040f4-297">Notez l’exemple précédent commentaire de code `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="040f4-297">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="040f4-298">Propriétés de navigation peuvent uniquement être chargées explicitement pour les entités de suivi.</span><span class="sxs-lookup"><span data-stu-id="040f4-298">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="040f4-299">Tester l’application.</span><span class="sxs-lookup"><span data-stu-id="040f4-299">Test the app.</span></span> <span data-ttu-id="040f4-300">Du point de vue des utilisateurs, l’application se comporte comme la version précédente.</span><span class="sxs-lookup"><span data-stu-id="040f4-300">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="040f4-301">Le didacticiel suivant montre comment mettre à jour les données associées.</span><span class="sxs-lookup"><span data-stu-id="040f4-301">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="040f4-302">[Précédent](xref:data/ef-rp/complex-data-model)
>[Suivant](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="040f4-302">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
