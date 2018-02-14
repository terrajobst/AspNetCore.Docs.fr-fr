---
title: "Pages Razor avec EF Core - Lire des données associées - 6 sur 8"
author: rick-anderson
description: "Dans ce didacticiel, nous allons lire et afficher des données associées, autrement dit des données chargées par Entity Framework dans des propriétés de navigation."
manager: wpickett
ms.author: riande
ms.date: 11/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/read-related-data
ms.openlocfilehash: ccb1e95ae2b43fd0a4c4b1ac9ed58a4d474ab3b6
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/31/2018
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a><span data-ttu-id="26da5-103">Lire des données associées - EF Core avec Pages Razor (6 sur 8)</span><span class="sxs-lookup"><span data-stu-id="26da5-103">Reading related data - EF Core with Razor Pages (6 of 8)</span></span>

<span data-ttu-id="26da5-104">Par [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="26da5-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="26da5-105">Dans ce didacticiel, nous allons lire et afficher des données associées.</span><span class="sxs-lookup"><span data-stu-id="26da5-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="26da5-106">Les données associées sont des données qu’EF Core charge dans des propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="26da5-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="26da5-107">Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, téléchargez l’[application terminée pour cette phase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span><span class="sxs-lookup"><span data-stu-id="26da5-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="26da5-108">Les illustrations suivantes montrent les pages terminées pour ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="26da5-108">The following illustrations show the completed pages for this tutorial:</span></span>

![Page d’index des cours](read-related-data/_static/courses-index.png)

![Page d’index des formateurs](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="26da5-111">Chargement hâtif, explicite et différé de données associées</span><span class="sxs-lookup"><span data-stu-id="26da5-111">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="26da5-112">EF Core peut charger des données associées dans les propriétés de navigation d’une entité de plusieurs manières :</span><span class="sxs-lookup"><span data-stu-id="26da5-112">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="26da5-113">[Chargement hâtif](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="26da5-113">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="26da5-114">Le chargement hâtif a lieu quand une requête pour un type d’entité charge également des entités associées.</span><span class="sxs-lookup"><span data-stu-id="26da5-114">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="26da5-115">Quand l’entité est lue, ses données associées sont récupérées.</span><span class="sxs-lookup"><span data-stu-id="26da5-115">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="26da5-116">Cela génère en général une requête de jointure unique qui récupère toutes les données nécessaires.</span><span class="sxs-lookup"><span data-stu-id="26da5-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="26da5-117">EF Core émet plusieurs requêtes pour certains types de chargement hâtif.</span><span class="sxs-lookup"><span data-stu-id="26da5-117">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="26da5-118">L’émission de requêtes multiples peut être plus efficace que ce n’était le cas pour certaines requêtes dans EF6 où une seule requête était émise.</span><span class="sxs-lookup"><span data-stu-id="26da5-118">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="26da5-119">Le chargement hâtif est spécifié avec les méthodes `Include` et `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="26da5-119">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

 ![Exemple de chargement hâtif](read-related-data/_static/eager-loading.png)
 
 <span data-ttu-id="26da5-121">Le chargement hâtif envoie plusieurs requêtes quand une navigation dans la collection est incluse :</span><span class="sxs-lookup"><span data-stu-id="26da5-121">Eager loading sends multiple queries when a collection nvavigation is included:</span></span>

 * <span data-ttu-id="26da5-122">Une requête pour la requête principale</span><span class="sxs-lookup"><span data-stu-id="26da5-122">One query for the main query</span></span> 
 * <span data-ttu-id="26da5-123">Une requête pour chaque « périmètre » de collection dans l’arborescence de la charge.</span><span class="sxs-lookup"><span data-stu-id="26da5-123">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="26da5-124">Requêtes distinctes avec `Load` : les données peuvent être récupérées dans des requêtes distinctes, et EF Core « corrige » les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="26da5-124">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="26da5-125">Le terme « corrige » signifie qu’EF Core renseigne automatiquement les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="26da5-125">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="26da5-126">Les requêtes distinctes avec `Load` s’apparentent plus au chargement explicite qu’au chargement hâtif.</span><span class="sxs-lookup"><span data-stu-id="26da5-126">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

 ![Exemple de requêtes distinctes](read-related-data/_static/separate-queries.png)

 <span data-ttu-id="26da5-128">Remarque : EF Core corrige automatiquement les propriétés de navigation vers d’autres entités qui étaient précédemment chargées dans l’instance de contexte.</span><span class="sxs-lookup"><span data-stu-id="26da5-128">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="26da5-129">Même si les données pour une propriété de navigation ne sont *pas* explicitement incluses, la propriété peut toujours être renseignée si toutes ou une partie des entités associées ont été précédemment chargées.</span><span class="sxs-lookup"><span data-stu-id="26da5-129">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="26da5-130">[Chargement explicite](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="26da5-130">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="26da5-131">Quand l’entité est lue pour la première fois, les données associées ne sont pas récupérées.</span><span class="sxs-lookup"><span data-stu-id="26da5-131">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="26da5-132">Vous devez écrire du code pour récupérer les données associées en cas de besoin.</span><span class="sxs-lookup"><span data-stu-id="26da5-132">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="26da5-133">En cas de chargement explicite avec des requêtes distinctes, plusieurs requêtes sont envoyées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="26da5-133">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="26da5-134">Avec le chargement explicite, le code spécifie les propriétés de navigation à charger.</span><span class="sxs-lookup"><span data-stu-id="26da5-134">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="26da5-135">Utilisez la méthode `Load` pour effectuer le chargement explicite.</span><span class="sxs-lookup"><span data-stu-id="26da5-135">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="26da5-136">Exemple :</span><span class="sxs-lookup"><span data-stu-id="26da5-136">For example:</span></span>

 ![Exemple de chargement explicite](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="26da5-138">[Chargement différé](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="26da5-138">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="26da5-139">[EF Core ne prend pas en charge le chargement différé actuellement](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="26da5-139">[EF Core doesn't currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="26da5-140">Quand l’entité est lue pour la première fois, les données associées ne sont pas récupérées.</span><span class="sxs-lookup"><span data-stu-id="26da5-140">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="26da5-141">Lors du premier accès à une propriété de navigation, les données requises pour cette propriété de navigation sont récupérées automatiquement.</span><span class="sxs-lookup"><span data-stu-id="26da5-141">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="26da5-142">Une requête est envoyée à la base de données chaque fois qu’une propriété de navigation est sollicitée pour la première fois.</span><span class="sxs-lookup"><span data-stu-id="26da5-142">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="26da5-143">L’opérateur `Select` charge uniquement les données associées nécessaires.</span><span class="sxs-lookup"><span data-stu-id="26da5-143">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="26da5-144">Créer une page Courses qui affiche le nom du département</span><span class="sxs-lookup"><span data-stu-id="26da5-144">Create a Courses page that displays department name</span></span>

<span data-ttu-id="26da5-145">L’entité Course comprend une propriété de navigation qui contient l’entité `Department`.</span><span class="sxs-lookup"><span data-stu-id="26da5-145">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="26da5-146">L’entité `Department` contient le département auquel le cours est affecté.</span><span class="sxs-lookup"><span data-stu-id="26da5-146">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="26da5-147">Pour afficher le nom du département affecté dans une liste de cours</span><span class="sxs-lookup"><span data-stu-id="26da5-147">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="26da5-148">Obtenez la propriété `Name` à partir de l’entité `Department`.</span><span class="sxs-lookup"><span data-stu-id="26da5-148">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="26da5-149">L’entité `Department` provient de la propriété de navigation `Course.Department`.</span><span class="sxs-lookup"><span data-stu-id="26da5-149">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![Course.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="26da5-151">Génération automatique du modèle Course</span><span class="sxs-lookup"><span data-stu-id="26da5-151">Scaffold the Course model</span></span>

* <span data-ttu-id="26da5-152">Quittez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26da5-152">Exit Visual Studio.</span></span>
* <span data-ttu-id="26da5-153">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="26da5-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="26da5-154">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="26da5-154">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

<span data-ttu-id="26da5-155">La commande précédente génère automatiquement le modèle `Course`.</span><span class="sxs-lookup"><span data-stu-id="26da5-155">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="26da5-156">Ouvrez le projet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26da5-156">Open the project in Visual Studio.</span></span>

<span data-ttu-id="26da5-157">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="26da5-157">Build the project.</span></span> <span data-ttu-id="26da5-158">La build génère des erreurs telles que celle-ci :</span><span class="sxs-lookup"><span data-stu-id="26da5-158">The build generates errors like the following:</span></span>

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="26da5-159">Remplacez globalement `_context.Course` par `_context.Courses` (autrement dit, ajoutez un « s » à `Course`).</span><span class="sxs-lookup"><span data-stu-id="26da5-159">Globally change `_context.Course` to `_context.Courses` (that is, add an "s" to `Course`).</span></span> <span data-ttu-id="26da5-160">7 occurrences sont trouvées et mises à jour.</span><span class="sxs-lookup"><span data-stu-id="26da5-160">7 occurrences are found and updated.</span></span>

<span data-ttu-id="26da5-161">Ouvrez *Pages/Courses/Index.cshtml.cs* et examinez la méthode `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="26da5-161">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="26da5-162">Le moteur de génération de modèles automatique a spécifié le chargement hâtif pour la propriété de navigation `Department`.</span><span class="sxs-lookup"><span data-stu-id="26da5-162">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="26da5-163">La méthode `Include` spécifie le chargement hâtif.</span><span class="sxs-lookup"><span data-stu-id="26da5-163">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="26da5-164">Exécutez l’application et sélectionnez le lien **Courses**.</span><span class="sxs-lookup"><span data-stu-id="26da5-164">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="26da5-165">La colonne Department affiche le `DepartmentID`, ce qui n’est pas utile.</span><span class="sxs-lookup"><span data-stu-id="26da5-165">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="26da5-166">Mettez à jour la méthode `OnGetAsync` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="26da5-166">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="26da5-167">Le code précédent ajoute `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="26da5-167">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="26da5-168">`AsNoTracking` améliore les performances, car les entités retournées ne sont pas suivies.</span><span class="sxs-lookup"><span data-stu-id="26da5-168">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="26da5-169">Les entités ne sont pas suivies, car elles ne sont pas mises à jour dans le contexte actuel.</span><span class="sxs-lookup"><span data-stu-id="26da5-169">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="26da5-170">Mettez à jour *Views/Courses/Index.cshtml* avec le balisage en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="26da5-170">Update *Views/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="26da5-171">Les modifications suivantes ont été apportées au code généré automatiquement :</span><span class="sxs-lookup"><span data-stu-id="26da5-171">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="26da5-172">Changement de l’en-tête : Index a été remplacé par Course.</span><span class="sxs-lookup"><span data-stu-id="26da5-172">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="26da5-173">Ajout d’une colonne **Number** qui affiche la valeur de propriété `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="26da5-173">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="26da5-174">Par défaut, les clés primaires ne sont pas générées automatiquement, car elles ne sont normalement pas significatives pour les utilisateurs finaux.</span><span class="sxs-lookup"><span data-stu-id="26da5-174">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="26da5-175">Toutefois, dans le cas présent la clé primaire est significative.</span><span class="sxs-lookup"><span data-stu-id="26da5-175">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="26da5-176">Modification de la colonne **Department** afin d’afficher le nom du département.</span><span class="sxs-lookup"><span data-stu-id="26da5-176">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="26da5-177">Le code affiche la propriété `Name` de l’entité `Department` qui est chargée dans la propriété de navigation `Department` :</span><span class="sxs-lookup"><span data-stu-id="26da5-177">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="26da5-178">Exécutez l’application et sélectionnez l’onglet **Courses** pour afficher la liste avec les noms des départements.</span><span class="sxs-lookup"><span data-stu-id="26da5-178">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Page d’index des cours](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="26da5-180">Chargement de données associées avec Select</span><span class="sxs-lookup"><span data-stu-id="26da5-180">Loading related data with Select</span></span>

<span data-ttu-id="26da5-181">La méthode `OnGetAsync` charge les données associées avec la méthode `Include` :</span><span class="sxs-lookup"><span data-stu-id="26da5-181">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="26da5-182">L’opérateur `Select` charge uniquement les données associées nécessaires.</span><span class="sxs-lookup"><span data-stu-id="26da5-182">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="26da5-183">Pour les éléments uniques, comme `Department.Name`, il utilise une jointure interne SQL.</span><span class="sxs-lookup"><span data-stu-id="26da5-183">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="26da5-184">Pour les collections, il utilise un autre accès à la base de données, mais c’est aussi le cas de l’opérateur .`Include`</span><span class="sxs-lookup"><span data-stu-id="26da5-184">For collections it uses another database access, but so does the .`Include`</span></span> <span data-ttu-id="26da5-185">sur les collections.</span><span class="sxs-lookup"><span data-stu-id="26da5-185">operator on collections.</span></span>

<span data-ttu-id="26da5-186">Le code suivant charge les données associées avec la méthode `Select` :</span><span class="sxs-lookup"><span data-stu-id="26da5-186">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="26da5-187">Voici le `CourseViewModel` :</span><span class="sxs-lookup"><span data-stu-id="26da5-187">The `CourseViewModel`:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="26da5-188">Pour obtenir un exemple complet, consultez [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) et [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs).</span><span class="sxs-lookup"><span data-stu-id="26da5-188">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="26da5-189">Créer une page Instructors qui affiche les cours et les inscriptions</span><span class="sxs-lookup"><span data-stu-id="26da5-189">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="26da5-190">Dans cette section, nous allons créer la page Instructors.</span><span class="sxs-lookup"><span data-stu-id="26da5-190">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="26da5-191">![Page d’index des formateurs](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="26da5-191">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="26da5-192">Cette page lit et affiche les données associées comme suit :</span><span class="sxs-lookup"><span data-stu-id="26da5-192">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="26da5-193">La liste des formateurs affiche des données associées de l’entité `OfficeAssignment` (Office dans l’image précédente).</span><span class="sxs-lookup"><span data-stu-id="26da5-193">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="26da5-194">Il existe une relation un-à-zéro-ou-un entre les entités `Instructor` et `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="26da5-194">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="26da5-195">Le chargement hâtif est utilisé pour les entités `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="26da5-195">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="26da5-196">Le chargement hâtif est généralement plus efficace quand les données associées doivent être affichées.</span><span class="sxs-lookup"><span data-stu-id="26da5-196">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="26da5-197">Ici, les affectations de bureau pour les formateurs sont affichées.</span><span class="sxs-lookup"><span data-stu-id="26da5-197">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="26da5-198">Quand l’utilisateur sélectionne un formateur (Harui dans l’image précédente), les entités `Course` associées sont affichées.</span><span class="sxs-lookup"><span data-stu-id="26da5-198">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="26da5-199">Il existe une relation plusieurs-à-plusieurs entre les entités `Instructor` et `Course`.</span><span class="sxs-lookup"><span data-stu-id="26da5-199">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="26da5-200">Nous utilisons le chargement hâtif pour les entités `Course` et leurs entités `Department` associées.</span><span class="sxs-lookup"><span data-stu-id="26da5-200">Eager loading for the `Course` entities and their related `Department` entities is used.</span></span> <span data-ttu-id="26da5-201">Dans le cas présent, des requêtes distinctes peuvent être plus efficaces, car seuls les cours du formateur sélectionné sont nécessaires.</span><span class="sxs-lookup"><span data-stu-id="26da5-201">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="26da5-202">Cet exemple montre comment utiliser le chargement hâtif pour des propriétés de navigation dans des entités qui se trouvent dans des propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="26da5-202">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="26da5-203">Quand l’utilisateur sélectionne un cours (Chemistry dans l’image précédente), les données associées de l’entité `Enrollments` sont affichées.</span><span class="sxs-lookup"><span data-stu-id="26da5-203">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="26da5-204">Dans l’image précédente, le nom et la note de l’étudiant sont affichés.</span><span class="sxs-lookup"><span data-stu-id="26da5-204">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="26da5-205">Il existe une relation un-à-plusieurs entre les entités `Course` et `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="26da5-205">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="26da5-206">Créer un modèle d’affichage pour l’affichage d’index des formateurs</span><span class="sxs-lookup"><span data-stu-id="26da5-206">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="26da5-207">La page sur les formateurs affiche les données de trois tables différentes.</span><span class="sxs-lookup"><span data-stu-id="26da5-207">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="26da5-208">Nous allons créer un modèle d’affichage qui comprend les trois entités représentant les trois tables.</span><span class="sxs-lookup"><span data-stu-id="26da5-208">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="26da5-209">Dans le dossier *SchoolViewModels*, créez *InstructorIndexData.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="26da5-209">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="26da5-210">Générer automatiquement le modèle Instructor</span><span class="sxs-lookup"><span data-stu-id="26da5-210">Scaffold the Instructor model</span></span>

* <span data-ttu-id="26da5-211">Quittez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26da5-211">Exit Visual Studio.</span></span>
* <span data-ttu-id="26da5-212">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="26da5-212">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="26da5-213">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="26da5-213">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

<span data-ttu-id="26da5-214">La commande précédente génère automatiquement le modèle `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="26da5-214">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="26da5-215">Ouvrez le projet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26da5-215">Open the project in Visual Studio.</span></span>

<span data-ttu-id="26da5-216">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="26da5-216">Build the project.</span></span> <span data-ttu-id="26da5-217">La build génère des erreurs.</span><span class="sxs-lookup"><span data-stu-id="26da5-217">The build generates errors.</span></span>

<span data-ttu-id="26da5-218">Remplacez globalement `_context.Instructor` par `_context.Instructors` (autrement dit, ajoutez un « s » à `Instructor`).</span><span class="sxs-lookup"><span data-stu-id="26da5-218">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="26da5-219">7 occurrences sont trouvées et mises à jour.</span><span class="sxs-lookup"><span data-stu-id="26da5-219">7 occurrences are found and updated.</span></span>

<span data-ttu-id="26da5-220">Exécutez l’application et accédez à la page des formateurs.</span><span class="sxs-lookup"><span data-stu-id="26da5-220">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="26da5-221">Remplacez *Pages/Instructors/Index.cshtml.cs* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="26da5-221">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

<span data-ttu-id="26da5-222">La méthode `OnGetAsync` accepte des données de route facultatives pour l’ID du formateur sélectionné.</span><span class="sxs-lookup"><span data-stu-id="26da5-222">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="26da5-223">Examinez la requête dans la page *Pages/Instructors/Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="26da5-223">Examine the query on the *Pages/Instructors/Index.cshtml* page:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="26da5-224">La requête comporte deux Include :</span><span class="sxs-lookup"><span data-stu-id="26da5-224">The query has two includes:</span></span>

* <span data-ttu-id="26da5-225">`OfficeAssignment` : affiché dans l’[affichage Instructors](#IP).</span><span class="sxs-lookup"><span data-stu-id="26da5-225">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="26da5-226">`CourseAssignments` : qui affiche les cours dispensés.</span><span class="sxs-lookup"><span data-stu-id="26da5-226">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="26da5-227">Mettre à jour la page d’index des formateurs</span><span class="sxs-lookup"><span data-stu-id="26da5-227">Update the instructors Index page</span></span>

<span data-ttu-id="26da5-228">Mettez à jour *Pages/Instructors/Index.cshtml* avec le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="26da5-228">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="26da5-229">Le balisage précédent apporte les modifications suivantes :</span><span class="sxs-lookup"><span data-stu-id="26da5-229">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="26da5-230">Il met à jour la directive `page` en remplaçant `@page` par `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="26da5-230">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="26da5-231">`"{id:int?}"` est un modèle de route.</span><span class="sxs-lookup"><span data-stu-id="26da5-231">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="26da5-232">Le modèle de route change les chaînes de requête entières dans l’URL en données de route.</span><span class="sxs-lookup"><span data-stu-id="26da5-232">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="26da5-233">Par exemple, si vous cliquez sur le lien **Select** pour un formateur, une URL comme celle-ci est générée :</span><span class="sxs-lookup"><span data-stu-id="26da5-233">For example, clicking on the **Select** link for an instructor when the page directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="26da5-234">Quand la directive de page est `@page "{id:int?}"`, l’URL précédente est :</span><span class="sxs-lookup"><span data-stu-id="26da5-234">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="26da5-235">Le titre de la page est **Instructors**.</span><span class="sxs-lookup"><span data-stu-id="26da5-235">Page title is **Instructors**.</span></span>
* <span data-ttu-id="26da5-236">Il ajoute une colonne **Office** qui affiche `item.OfficeAssignment.Location` uniquement si `item.OfficeAssignment` n’est pas Null.</span><span class="sxs-lookup"><span data-stu-id="26da5-236">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="26da5-237">Comme il s’agit d’une relation un-à-zéro-ou-un, il se peut qu’il n’y ait pas d’entité OfficeAssignment associée.</span><span class="sxs-lookup"><span data-stu-id="26da5-237">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="26da5-238">Ajout d’une colonne **Courses** qui affiche les cours dispensés par chaque formateur.</span><span class="sxs-lookup"><span data-stu-id="26da5-238">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="26da5-239">Pour en savoir plus sur cette syntaxe razor, consultez [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) (Transition de ligne explicite avec @:).</span><span class="sxs-lookup"><span data-stu-id="26da5-239">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="26da5-240">Ajout de code qui ajoute dynamiquement `class="success"` à l’élément `tr` du formateur sélectionné.</span><span class="sxs-lookup"><span data-stu-id="26da5-240">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="26da5-241">Cela définit une couleur d’arrière-plan pour la ligne sélectionnée à l’aide d’une classe d’amorçage.</span><span class="sxs-lookup"><span data-stu-id="26da5-241">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="26da5-242">Ajout d’un nouveau lien hypertexte libellé **Select**.</span><span class="sxs-lookup"><span data-stu-id="26da5-242">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="26da5-243">Ce lien envoie l’ID du formateur sélectionné à la méthode `Index`, et définit une couleur d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="26da5-243">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="26da5-244">Exécutez l’application et sélectionnez le lien **Instructors**. La page affiche le `Location` (bureau) de l’entité `OfficeAssignment` associée.</span><span class="sxs-lookup"><span data-stu-id="26da5-244">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="26da5-245">Si OfficeAssignment\` est Null, une cellule de table vide est affichée.</span><span class="sxs-lookup"><span data-stu-id="26da5-245">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Page d’index des formateurs sans aucun élément sélectionné](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="26da5-247">Cliquez sur le lien **Select**.</span><span class="sxs-lookup"><span data-stu-id="26da5-247">Click on the **Select** link.</span></span> <span data-ttu-id="26da5-248">Le style de ligne change.</span><span class="sxs-lookup"><span data-stu-id="26da5-248">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="26da5-249">Ajouter les cours dispensés par le formateur sélectionné</span><span class="sxs-lookup"><span data-stu-id="26da5-249">Add courses taught by selected instructor</span></span>

<span data-ttu-id="26da5-250">Mettez à jour la méthode `OnGetAsync` dans *Pages/Instructors/Index.cshtml.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="26da5-250">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

<span data-ttu-id="26da5-251">Examinez la requête mise à jour :</span><span class="sxs-lookup"><span data-stu-id="26da5-251">Examine the updated query:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="26da5-252">La requête précédente ajoute les entités `Department`.</span><span class="sxs-lookup"><span data-stu-id="26da5-252">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="26da5-253">Le code suivant s’exécute quand un formateur est sélectionné (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="26da5-253">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="26da5-254">Le formateur sélectionné est récupéré à partir de la liste des formateurs dans le modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="26da5-254">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="26da5-255">La propriété `Courses` du modèle d’affichage est chargée avec les entités `Course` de la propriété de navigation `CourseAssignments` de ce formateur.</span><span class="sxs-lookup"><span data-stu-id="26da5-255">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="26da5-256">La méthode `Where` retourne une collection.</span><span class="sxs-lookup"><span data-stu-id="26da5-256">The `Where` method returns a collection.</span></span> <span data-ttu-id="26da5-257">Dans la méthode `Where` précédente, une seule entité `Instructor` est retournée.</span><span class="sxs-lookup"><span data-stu-id="26da5-257">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="26da5-258">La méthode `Single` convertit la collection en une seule entité `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="26da5-258">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="26da5-259">L’entité `Instructor` fournit l’accès à la propriété `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="26da5-259">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="26da5-260">`CourseAssignments` fournit l’accès aux entités `Course` associées.</span><span class="sxs-lookup"><span data-stu-id="26da5-260">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Instructor-to-Courses m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="26da5-262">La méthode `Single` est utilisée sur une collection quand la collection ne compte qu’un seul élément.</span><span class="sxs-lookup"><span data-stu-id="26da5-262">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="26da5-263">La méthode `Single` lève une exception si la collection est vide ou s’il y a plusieurs éléments.</span><span class="sxs-lookup"><span data-stu-id="26da5-263">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="26da5-264">Une alternative est `SingleOrDefault`, qui retourne une valeur par défaut (Null dans le cas présent) si la collection est vide.</span><span class="sxs-lookup"><span data-stu-id="26da5-264">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="26da5-265">L’utilisation de `SingleOrDefault` sur une collection vide :</span><span class="sxs-lookup"><span data-stu-id="26da5-265">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="26da5-266">Génère une exception (à cause de la tentative de trouver une propriété `Courses` sur une référence Null).</span><span class="sxs-lookup"><span data-stu-id="26da5-266">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="26da5-267">Le message d’exception indique moins clairement la cause du problème.</span><span class="sxs-lookup"><span data-stu-id="26da5-267">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="26da5-268">Le code suivant renseigne la propriété `Enrollments` du modèle d’affichage quand un cours est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="26da5-268">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="26da5-269">Ajoutez le balisage suivant à la fin de la page Razor *Pages/Courses/Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="26da5-269">Add the following markup to the end of the *Pages/Courses/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

<span data-ttu-id="26da5-270">Le balisage précédent affiche une liste de cours associés à un formateur quand un formateur est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="26da5-270">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="26da5-271">Testez l’application.</span><span class="sxs-lookup"><span data-stu-id="26da5-271">Test the app.</span></span> <span data-ttu-id="26da5-272">Cliquez sur un lien **Select** dans la page des formateurs.</span><span class="sxs-lookup"><span data-stu-id="26da5-272">Click on a **Select** link on the instructors page.</span></span>

![Page d’index des formateurs avec un formateur sélectionné](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="26da5-274">Afficher les données sur les étudiants</span><span class="sxs-lookup"><span data-stu-id="26da5-274">Show student data</span></span>

<span data-ttu-id="26da5-275">Dans cette section, nous allons mettre à jour l’application afin d’afficher les données sur les étudiants pour le cours sélectionné.</span><span class="sxs-lookup"><span data-stu-id="26da5-275">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="26da5-276">Mettez à jour la requête dans la méthode `OnGetAsync` dans *Pages/Instructors/Index.cshtml.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="26da5-276">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="26da5-277">Mettez à jour *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="26da5-277">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="26da5-278">Ajoutez le balisage suivant à la fin du fichier :</span><span class="sxs-lookup"><span data-stu-id="26da5-278">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="26da5-279">Le balisage précédent affiche une liste des étudiants qui sont inscrits au cours sélectionné.</span><span class="sxs-lookup"><span data-stu-id="26da5-279">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="26da5-280">Actualisez la page et sélectionnez un formateur.</span><span class="sxs-lookup"><span data-stu-id="26da5-280">Refresh the page and select an instructor.</span></span> <span data-ttu-id="26da5-281">Sélectionnez un cours pour afficher la liste des étudiants inscrits et leurs notes.</span><span class="sxs-lookup"><span data-stu-id="26da5-281">Select a course to see the list of enrolled students and their grades.</span></span>

![Page d’index des formateurs avec un formateur et un cours sélectionnés](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="26da5-283">Utilisation de Single</span><span class="sxs-lookup"><span data-stu-id="26da5-283">Using Single</span></span>

<span data-ttu-id="26da5-284">La méthode `Single` peut passer la condition `Where` au lieu d’appeler la méthode `Where` séparément :</span><span class="sxs-lookup"><span data-stu-id="26da5-284">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="26da5-285">L’approche précédente faisant appel à `Single` ne présente aucun avantage par rapport à l’utilisation de `Where`.</span><span class="sxs-lookup"><span data-stu-id="26da5-285">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="26da5-286">Certains développeurs préfèrent le style d’approche `Single`.</span><span class="sxs-lookup"><span data-stu-id="26da5-286">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="26da5-287">Chargement explicite</span><span class="sxs-lookup"><span data-stu-id="26da5-287">Explicit loading</span></span>

<span data-ttu-id="26da5-288">Le code actuel spécifie le chargement hâtif pour `Enrollments` et `Students` :</span><span class="sxs-lookup"><span data-stu-id="26da5-288">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="26da5-289">Supposez que les utilisateurs souhaitent rarement voir les inscriptions à un cours.</span><span class="sxs-lookup"><span data-stu-id="26da5-289">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="26da5-290">Dans ce cas, une optimisation consisterait à charger les données d’inscription uniquement si elles sont demandées.</span><span class="sxs-lookup"><span data-stu-id="26da5-290">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="26da5-291">Dans cette section, la méthode `OnGetAsync` est mise à jour de façon à utiliser le chargement explicite de `Enrollments` et `Students`.</span><span class="sxs-lookup"><span data-stu-id="26da5-291">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="26da5-292">Mettez à jour la méthode `OnGetAsync` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="26da5-292">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="26da5-293">Le code précédent supprime les appels de méthode *ThenInclude* pour les données sur les inscriptions et les étudiants.</span><span class="sxs-lookup"><span data-stu-id="26da5-293">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="26da5-294">Si un cours est sélectionné, le code en surbrillance récupère :</span><span class="sxs-lookup"><span data-stu-id="26da5-294">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="26da5-295">Les entités `Enrollment` pour le cours sélectionné.</span><span class="sxs-lookup"><span data-stu-id="26da5-295">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="26da5-296">Les entités `Student` pour chaque `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="26da5-296">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="26da5-297">Notez que le code précédent commente `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="26da5-297">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="26da5-298">Les propriétés de navigation peuvent être chargées explicitement uniquement pour les entités suivies.</span><span class="sxs-lookup"><span data-stu-id="26da5-298">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="26da5-299">Testez l’application.</span><span class="sxs-lookup"><span data-stu-id="26da5-299">Test the app.</span></span> <span data-ttu-id="26da5-300">Du point de vue des utilisateurs, l’application se comporte comme la version précédente.</span><span class="sxs-lookup"><span data-stu-id="26da5-300">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="26da5-301">Le didacticiel suivant montre comment mettre à jour les données associées.</span><span class="sxs-lookup"><span data-stu-id="26da5-301">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="26da5-302">[Précédent](xref:data/ef-rp/complex-data-model)
>[Suivant](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="26da5-302">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
