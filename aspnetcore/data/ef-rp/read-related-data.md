---
title: Pages Razor avec EF Core dans ASP.NET Core - Lire des données associées - 6 sur 8
author: rick-anderson
description: Dans ce tutoriel, nous allons lire et afficher des données associées, autrement dit des données chargées par Entity Framework dans des propriétés de navigation.
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/read-related-data
ms.openlocfilehash: a4864a749ed9c839c2a22f179759a3f28db525ee
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68914210"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="e925d-103">Pages Razor avec EF Core dans ASP.NET Core - Lire des données associées - 6 sur 8</span><span class="sxs-lookup"><span data-stu-id="e925d-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="e925d-104">Par [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e925d-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e925d-105">Ce tutoriel montre comment lire et afficher des données associées.</span><span class="sxs-lookup"><span data-stu-id="e925d-105">This tutorial shows how to read and display related data.</span></span> <span data-ttu-id="e925d-106">Les données associées sont des données qu’EF Core charge dans des propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="e925d-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="e925d-107">Les illustrations suivantes montrent les pages terminées pour ce tutoriel :</span><span class="sxs-lookup"><span data-stu-id="e925d-107">The following illustrations show the completed pages for this tutorial:</span></span>

![Page d’index des cours](read-related-data/_static/courses-index30.png)

![Page d’index des formateurs](read-related-data/_static/instructors-index30.png)

## <a name="eager-explicit-and-lazy-loading"></a><span data-ttu-id="e925d-110">Chargement hâtif, explicite et différé</span><span class="sxs-lookup"><span data-stu-id="e925d-110">Eager, explicit, and lazy loading</span></span>

<span data-ttu-id="e925d-111">EF Core peut charger des données associées dans les propriétés de navigation d’une entité de plusieurs manières :</span><span class="sxs-lookup"><span data-stu-id="e925d-111">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="e925d-112">[Chargement hâtif](/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="e925d-112">[Eager loading](/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="e925d-113">Le chargement hâtif a lieu quand une requête pour un type d’entité charge également des entités associées.</span><span class="sxs-lookup"><span data-stu-id="e925d-113">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="e925d-114">Quand une entité est lue, ses données associées sont récupérées.</span><span class="sxs-lookup"><span data-stu-id="e925d-114">When an entity is read, its related data is retrieved.</span></span> <span data-ttu-id="e925d-115">Cela génère en général une requête de jointure unique qui récupère toutes les données nécessaires.</span><span class="sxs-lookup"><span data-stu-id="e925d-115">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="e925d-116">EF Core émet plusieurs requêtes pour certains types de chargement hâtif.</span><span class="sxs-lookup"><span data-stu-id="e925d-116">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="e925d-117">Il peut s’avérer plus efficace d’émettre plusieurs requêtes plutôt qu’une seule très grande.</span><span class="sxs-lookup"><span data-stu-id="e925d-117">Issuing multiple queries can be more efficient than a giant single query.</span></span> <span data-ttu-id="e925d-118">Le chargement hâtif est spécifié avec les méthodes `Include` et `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="e925d-118">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Exemple de chargement hâtif](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="e925d-120">Le chargement hâtif envoie plusieurs requêtes quand une navigation dans la collection est incluse :</span><span class="sxs-lookup"><span data-stu-id="e925d-120">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="e925d-121">Une requête pour la requête principale</span><span class="sxs-lookup"><span data-stu-id="e925d-121">One query for the main query</span></span> 
  * <span data-ttu-id="e925d-122">Une requête pour chaque « périmètre » de collection dans l’arborescence de la charge.</span><span class="sxs-lookup"><span data-stu-id="e925d-122">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="e925d-123">Requêtes distinctes avec `Load` : Les données peuvent être récupérées dans des requêtes distinctes, et EF Core « corrige » les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="e925d-123">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="e925d-124">Quand EF Core « corrige », cela signifie que les propriétés de navigation sont renseignées automatiquement.</span><span class="sxs-lookup"><span data-stu-id="e925d-124">"Fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="e925d-125">Les requêtes distinctes avec `Load` s’apparentent plus au chargement explicite qu’au chargement hâtif.</span><span class="sxs-lookup"><span data-stu-id="e925d-125">Separate queries with `Load` is more like explicit loading than eager loading.</span></span>

  ![Exemple de requêtes distinctes](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="e925d-127">Remarque : EF Core corrige automatiquement les propriétés de navigation vers d’autres entités qui étaient précédemment chargées dans l’instance de contexte.</span><span class="sxs-lookup"><span data-stu-id="e925d-127">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="e925d-128">Même si les données pour une propriété de navigation ne sont *pas* explicitement incluses, la propriété peut toujours être renseignée si toutes ou une partie des entités associées ont été précédemment chargées.</span><span class="sxs-lookup"><span data-stu-id="e925d-128">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="e925d-129">[Chargement explicite](/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="e925d-129">[Explicit loading](/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="e925d-130">Quand l’entité est lue pour la première fois, les données associées ne sont pas récupérées.</span><span class="sxs-lookup"><span data-stu-id="e925d-130">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="e925d-131">Vous devez écrire du code pour récupérer les données associées en cas de besoin.</span><span class="sxs-lookup"><span data-stu-id="e925d-131">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="e925d-132">En cas de chargement explicite avec des requêtes distinctes, plusieurs requêtes sont envoyées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="e925d-132">Explicit loading with separate queries results in multiple queries sent to the database.</span></span> <span data-ttu-id="e925d-133">Avec le chargement explicite, le code spécifie les propriétés de navigation à charger.</span><span class="sxs-lookup"><span data-stu-id="e925d-133">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="e925d-134">Utilisez la méthode `Load` pour effectuer le chargement explicite.</span><span class="sxs-lookup"><span data-stu-id="e925d-134">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="e925d-135">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="e925d-135">For example:</span></span>

  ![Exemple de chargement explicite](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="e925d-137">[Chargement différé](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="e925d-137">[Lazy loading](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="e925d-138">[Le chargement différé a été ajouté à EF Core dans la version 2.1](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="e925d-138">[Lazy loading was added to EF Core in version 2.1](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="e925d-139">Quand l’entité est lue pour la première fois, les données associées ne sont pas récupérées.</span><span class="sxs-lookup"><span data-stu-id="e925d-139">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="e925d-140">Lors du premier accès à une propriété de navigation, les données requises pour cette propriété de navigation sont récupérées automatiquement.</span><span class="sxs-lookup"><span data-stu-id="e925d-140">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="e925d-141">Une requête est envoyée à la base de données chaque fois qu’une propriété de navigation fait pour la première fois l’objet d’un accès.</span><span class="sxs-lookup"><span data-stu-id="e925d-141">A query is sent to the database each time a navigation property is accessed for the first time.</span></span>

## <a name="create-course-pages"></a><span data-ttu-id="e925d-142">Créer des pages Course</span><span class="sxs-lookup"><span data-stu-id="e925d-142">Create Course pages</span></span>

<span data-ttu-id="e925d-143">L’entité `Course` comprend une propriété de navigation qui contient l’entité `Department` associée.</span><span class="sxs-lookup"><span data-stu-id="e925d-143">The `Course` entity includes a navigation property that contains the related `Department` entity.</span></span>

![Course.Department](read-related-data/_static/dep-crs.png)

<span data-ttu-id="e925d-145">Pour afficher le nom du service (« department ») affecté pour un cours (« course ») :</span><span class="sxs-lookup"><span data-stu-id="e925d-145">To display the name of the assigned department for a course:</span></span>

* <span data-ttu-id="e925d-146">Chargez l’entité `Department` associée dans la propriété de navigation `Course.Department`.</span><span class="sxs-lookup"><span data-stu-id="e925d-146">Load the related `Department` entity into the `Course.Department` navigation property.</span></span>
* <span data-ttu-id="e925d-147">Obtenez le nom à partir de la propriété `Name` de l’entité `Department`.</span><span class="sxs-lookup"><span data-stu-id="e925d-147">Get the name from the `Department` entity's `Name` property.</span></span>

<a name="scaffold"></a>

### <a name="scaffold-course-pages"></a><span data-ttu-id="e925d-148">Générer automatiquement des modèles de pages Course</span><span class="sxs-lookup"><span data-stu-id="e925d-148">Scaffold Course pages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e925d-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e925d-149">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e925d-150">Suivez les instructions dans [Générer automatiquement des modèles de pages Student](xref:data/ef-rp/intro#scaffold-student-pages) avec les exceptions suivantes :</span><span class="sxs-lookup"><span data-stu-id="e925d-150">Follow the instructions in [Scaffold Student pages](xref:data/ef-rp/intro#scaffold-student-pages) with the following exceptions:</span></span>

  * <span data-ttu-id="e925d-151">Créez un dossier *Pages/Courses*.</span><span class="sxs-lookup"><span data-stu-id="e925d-151">Create a *Pages/Courses* folder.</span></span>
  * <span data-ttu-id="e925d-152">Utilisez `Course` pour la classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="e925d-152">Use `Course` for the model class.</span></span>
  * <span data-ttu-id="e925d-153">Utilisez la classe de contexte existante au lieu d’en créer une nouvelle.</span><span class="sxs-lookup"><span data-stu-id="e925d-153">Use the existing context class instead of creating a new one.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e925d-154">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e925d-154">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e925d-155">Créez un dossier *Pages/Courses*.</span><span class="sxs-lookup"><span data-stu-id="e925d-155">Create a *Pages/Courses* folder.</span></span>

* <span data-ttu-id="e925d-156">Exécutez la commande suivante pour générer automatiquement des modèles de pages Course.</span><span class="sxs-lookup"><span data-stu-id="e925d-156">Run the following command to scaffold the Course pages.</span></span>

  <span data-ttu-id="e925d-157">**Sur Windows :**</span><span class="sxs-lookup"><span data-stu-id="e925d-157">**On Windows:**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

  <span data-ttu-id="e925d-158">**Sur Linux ou macOS :**</span><span class="sxs-lookup"><span data-stu-id="e925d-158">**On Linux or macOS:**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages/Courses --referenceScriptLibraries
  ```

---

* <span data-ttu-id="e925d-159">Ouvrez *Pages/Courses/Index.cshtml.cs* et examinez la méthode `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="e925d-159">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="e925d-160">Le moteur de génération de modèles automatique a spécifié le chargement hâtif pour la propriété de navigation `Department`.</span><span class="sxs-lookup"><span data-stu-id="e925d-160">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="e925d-161">La méthode `Include` spécifie le chargement hâtif.</span><span class="sxs-lookup"><span data-stu-id="e925d-161">The `Include` method specifies eager loading.</span></span>

* <span data-ttu-id="e925d-162">Exécutez l’application et sélectionnez le lien **Courses**.</span><span class="sxs-lookup"><span data-stu-id="e925d-162">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="e925d-163">La colonne Department affiche le `DepartmentID`, ce qui n’est pas utile.</span><span class="sxs-lookup"><span data-stu-id="e925d-163">The department column displays the `DepartmentID`, which isn't useful.</span></span>

### <a name="display-the-department-name"></a><span data-ttu-id="e925d-164">Afficher le nom du service (« department »)</span><span class="sxs-lookup"><span data-stu-id="e925d-164">Display the department name</span></span>

<span data-ttu-id="e925d-165">Mettez à jour Pages/Courses/Index.cshtml.cs avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e925d-165">Update Pages/Courses/Index.cshtml.cs with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Courses/Index.cshtml.cs?highlight=18,22,24)]

<span data-ttu-id="e925d-166">Le code précédent remplace la propriété `Course` par `Courses` et ajoute `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="e925d-166">The preceding code changes the `Course` property to `Courses` and adds `AsNoTracking`.</span></span> <span data-ttu-id="e925d-167">`AsNoTracking` améliore les performances, car les entités retournées ne sont pas suivies.</span><span class="sxs-lookup"><span data-stu-id="e925d-167">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="e925d-168">Les entités n’ont pas besoin d’être suivies, car elles ne sont pas mises à jour dans le contexte actuel.</span><span class="sxs-lookup"><span data-stu-id="e925d-168">The entities don't need to be tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="e925d-169">Mettez à jour *Pages/Courses/Index.cshtml* avec le code suivant.</span><span class="sxs-lookup"><span data-stu-id="e925d-169">Update *Pages/Courses/Index.cshtml* with the following code.</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Index.cshtml?highlight=5,8,16-18,20,23,26,32,35-37,45)]

<span data-ttu-id="e925d-170">Les modifications suivantes ont été apportées au code généré automatiquement :</span><span class="sxs-lookup"><span data-stu-id="e925d-170">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="e925d-171">Le nom de propriété `Course` est changé en `Courses`.</span><span class="sxs-lookup"><span data-stu-id="e925d-171">Changed the `Course` property name to `Courses`.</span></span>
* <span data-ttu-id="e925d-172">Ajout d’une colonne **Number** qui affiche la valeur de la propriété `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="e925d-172">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="e925d-173">Par défaut, les clés primaires ne sont pas générées automatiquement, car elles ne sont normalement pas significatives pour les utilisateurs finaux.</span><span class="sxs-lookup"><span data-stu-id="e925d-173">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="e925d-174">Toutefois, dans le cas présent la clé primaire est significative.</span><span class="sxs-lookup"><span data-stu-id="e925d-174">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="e925d-175">Modification de la colonne **Department** afin d’afficher le nom du département.</span><span class="sxs-lookup"><span data-stu-id="e925d-175">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="e925d-176">Le code affiche la propriété `Name` de l’entité `Department` qui est chargée dans la propriété de navigation `Department` :</span><span class="sxs-lookup"><span data-stu-id="e925d-176">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="e925d-177">Exécutez l’application et sélectionnez l’onglet **Courses** pour afficher la liste avec les noms des départements.</span><span class="sxs-lookup"><span data-stu-id="e925d-177">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Page d’index des cours](read-related-data/_static/courses-index30.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a><span data-ttu-id="e925d-179">Chargement de données associées avec Select</span><span class="sxs-lookup"><span data-stu-id="e925d-179">Loading related data with Select</span></span>

<span data-ttu-id="e925d-180">La méthode `OnGetAsync` charge les données associées avec la méthode `Include`.</span><span class="sxs-lookup"><span data-stu-id="e925d-180">The `OnGetAsync` method loads related data with the `Include` method.</span></span> <span data-ttu-id="e925d-181">La méthode `Select` est autre solution qui charge uniquement les données associées nécessaires.</span><span class="sxs-lookup"><span data-stu-id="e925d-181">The `Select` method is an alternative that loads only the related data needed.</span></span> <span data-ttu-id="e925d-182">Pour les éléments uniques, comme `Department.Name`, il utilise une jointure interne SQL.</span><span class="sxs-lookup"><span data-stu-id="e925d-182">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="e925d-183">Pour les collections, il utilise un autre accès à la base de données, mais c’est aussi le cas de l’opérateur `Include` sur les collections.</span><span class="sxs-lookup"><span data-stu-id="e925d-183">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="e925d-184">Le code suivant charge les données associées avec la méthode `Select` :</span><span class="sxs-lookup"><span data-stu-id="e925d-184">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=6)]

<span data-ttu-id="e925d-185">Voici le `CourseViewModel` :</span><span class="sxs-lookup"><span data-stu-id="e925d-185">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="e925d-186">Pour obtenir un exemple complet, consultez [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml) et [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs).</span><span class="sxs-lookup"><span data-stu-id="e925d-186">See [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-instructor-pages"></a><span data-ttu-id="e925d-187">Créer des pages Instructor</span><span class="sxs-lookup"><span data-stu-id="e925d-187">Create Instructor pages</span></span>

<span data-ttu-id="e925d-188">Cette section génère automatiquement des modèles de pages Instructor et ajoute les cours et les inscriptions associés à la page d’index des formateurs.</span><span class="sxs-lookup"><span data-stu-id="e925d-188">This section scaffolds Instructor pages and adds related Courses and Enrollments to the Instructors Index page.</span></span>

<a name="IP"></a>
<span data-ttu-id="e925d-189">![Page d’index des formateurs](read-related-data/_static/instructors-index30.png)</span><span class="sxs-lookup"><span data-stu-id="e925d-189">![Instructors Index page](read-related-data/_static/instructors-index30.png)</span></span>

<span data-ttu-id="e925d-190">Cette page lit et affiche les données associées comme suit :</span><span class="sxs-lookup"><span data-stu-id="e925d-190">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="e925d-191">La liste des formateurs affiche des données associées de l’entité `OfficeAssignment` (Office dans l’image précédente).</span><span class="sxs-lookup"><span data-stu-id="e925d-191">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="e925d-192">Il existe une relation un-à-zéro-ou-un entre les entités `Instructor` et `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="e925d-192">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="e925d-193">Le chargement hâtif est utilisé pour les entités `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="e925d-193">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="e925d-194">Le chargement hâtif est généralement plus efficace quand les données associées doivent être affichées.</span><span class="sxs-lookup"><span data-stu-id="e925d-194">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="e925d-195">Ici, les affectations de bureau pour les formateurs sont affichées.</span><span class="sxs-lookup"><span data-stu-id="e925d-195">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="e925d-196">Quand l’utilisateur sélectionne un formateur, les entités `Course` associées sont affichées.</span><span class="sxs-lookup"><span data-stu-id="e925d-196">When the user selects an instructor, related `Course` entities are displayed.</span></span> <span data-ttu-id="e925d-197">Il existe une relation plusieurs-à-plusieurs entre les entités `Instructor` et `Course`.</span><span class="sxs-lookup"><span data-stu-id="e925d-197">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="e925d-198">Le chargement hâtif est utilisé pour les entités `Course` et leurs entités `Department` associées.</span><span class="sxs-lookup"><span data-stu-id="e925d-198">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="e925d-199">Dans le cas présent, des requêtes distinctes peuvent être plus efficaces, car seuls les cours du formateur sélectionné sont nécessaires.</span><span class="sxs-lookup"><span data-stu-id="e925d-199">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="e925d-200">Cet exemple montre comment utiliser le chargement hâtif pour des propriétés de navigation dans des entités qui se trouvent dans des propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="e925d-200">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="e925d-201">Quand l’utilisateur sélectionne un cours, les données associées de l’entité `Enrollments` s’affichent.</span><span class="sxs-lookup"><span data-stu-id="e925d-201">When the user selects a course, related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="e925d-202">Dans l’image précédente, le nom et la note de l’étudiant sont affichés.</span><span class="sxs-lookup"><span data-stu-id="e925d-202">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="e925d-203">Il existe une relation un-à-plusieurs entre les entités `Course` et `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="e925d-203">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model"></a><span data-ttu-id="e925d-204">Création d'un modèle de vue</span><span class="sxs-lookup"><span data-stu-id="e925d-204">Create a view model</span></span>

<span data-ttu-id="e925d-205">La page sur les formateurs affiche les données de trois tables différentes.</span><span class="sxs-lookup"><span data-stu-id="e925d-205">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="e925d-206">Un modèle de vue comprenant trois propriétés représentant les trois tables est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="e925d-206">A view model is needed that includes three properties representing the three tables.</span></span>

<span data-ttu-id="e925d-207">Créez *SchoolViewModels/InstructorIndexData.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e925d-207">Create *SchoolViewModels/InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-instructor-pages"></a><span data-ttu-id="e925d-208">Générer automatiquement des modèles de pages Instructor</span><span class="sxs-lookup"><span data-stu-id="e925d-208">Scaffold Instructor pages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e925d-209">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e925d-209">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e925d-210">Suivez les instructions dans [Générer automatiquement des modèles de pages Student](xref:data/ef-rp/intro#scaffold-student-pages) avec les exceptions suivantes :</span><span class="sxs-lookup"><span data-stu-id="e925d-210">Follow the instructions in [Scaffold the student pages](xref:data/ef-rp/intro#scaffold-student-pages) with the following exceptions:</span></span>

  * <span data-ttu-id="e925d-211">Créez un dossier *Pages/Instructors*.</span><span class="sxs-lookup"><span data-stu-id="e925d-211">Create a *Pages/Instructors* folder.</span></span>
  * <span data-ttu-id="e925d-212">Utilisez `Instructor` pour la classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="e925d-212">Use `Instructor` for the model class.</span></span>
  * <span data-ttu-id="e925d-213">Utilisez la classe de contexte existante au lieu d’en créer une nouvelle.</span><span class="sxs-lookup"><span data-stu-id="e925d-213">Use the existing context class instead of creating a new one.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e925d-214">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e925d-214">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e925d-215">Créez un dossier *Pages/Instructors*.</span><span class="sxs-lookup"><span data-stu-id="e925d-215">Create a *Pages/Instructors* folder.</span></span>

* <span data-ttu-id="e925d-216">Exécutez la commande suivante pour générer automatiquement des modèles de pages Instructor.</span><span class="sxs-lookup"><span data-stu-id="e925d-216">Run the following command to scaffold the Instructor pages.</span></span>

  <span data-ttu-id="e925d-217">**Sur Windows :**</span><span class="sxs-lookup"><span data-stu-id="e925d-217">**On Windows:**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

  <span data-ttu-id="e925d-218">**Sur Linux ou macOS :**</span><span class="sxs-lookup"><span data-stu-id="e925d-218">**On Linux or macOS:**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages/Instructors --referenceScriptLibraries
  ```

---

<span data-ttu-id="e925d-219">Pour voir à quoi ressemble la page générée automatiquement avant de la mettre à jour, exécutez l’application et accédez à la page Instructors.</span><span class="sxs-lookup"><span data-stu-id="e925d-219">To see what the scaffolded page looks like before you update it, run the app and navigate to the Instructors page.</span></span>

<span data-ttu-id="e925d-220">Mettez à jour *Pages/Instructors/Index.cshtml.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e925d-220">Update *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,19-53)]

<span data-ttu-id="e925d-221">La méthode `OnGetAsync` accepte des données de route facultatives pour l’ID du formateur sélectionné.</span><span class="sxs-lookup"><span data-stu-id="e925d-221">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="e925d-222">Examinez la requête dans le fichier *Pages/Instructors/Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="e925d-222">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_EagerLoading)]

<span data-ttu-id="e925d-223">Le code spécifie un chargement hâtif pour les propriétés de navigation suivantes :</span><span class="sxs-lookup"><span data-stu-id="e925d-223">The code specifies eager loading for the following navigation properties:</span></span>

* `Instructor.OfficeAssignment`
* `Instructor.CourseAssignments`
  * `CourseAssignments.Course`
    * `Course.Department`
    * `Course.Enrollments`
      * `Enrollment.Student`

<span data-ttu-id="e925d-224">Notez la répétition des méthodes `Include` et `ThenInclude` pour `CourseAssignments` et `Course`.</span><span class="sxs-lookup"><span data-stu-id="e925d-224">Notice the repetition of `Include` and `ThenInclude` methods for `CourseAssignments` and `Course`.</span></span> <span data-ttu-id="e925d-225">Cette répétition est nécessaire pour spécifier un chargement hâtif pour deux propriétés de navigation de l’entité `Course`.</span><span class="sxs-lookup"><span data-stu-id="e925d-225">This repetition is necessary to specify eager loading for two navigation properties of the `Course` entity.</span></span>

<span data-ttu-id="e925d-226">Le code suivant s’exécute quand un formateur est sélectionné (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="e925d-226">The following code executes when an instructor is selected (`id != null`).</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_SelectInstructor)]

<span data-ttu-id="e925d-227">Le formateur sélectionné est récupéré à partir de la liste des formateurs dans le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="e925d-227">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="e925d-228">La propriété `Courses` du modèle de vue est chargée avec les entités `Course` de la propriété de navigation `CourseAssignments` de ce formateur.</span><span class="sxs-lookup"><span data-stu-id="e925d-228">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="e925d-229">La méthode `Where` retourne une collection.</span><span class="sxs-lookup"><span data-stu-id="e925d-229">The `Where` method returns a collection.</span></span> <span data-ttu-id="e925d-230">Mais dans ce cas, le filtre sélectionne une entité unique.</span><span class="sxs-lookup"><span data-stu-id="e925d-230">But in this case, the filter will select a single entity.</span></span> <span data-ttu-id="e925d-231">La méthode `Single` est donc appelée pour convertir la collection en une seule entité `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="e925d-231">so the `Single` method is called to convert the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="e925d-232">L’entité `Instructor` fournit l’accès à la propriété `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="e925d-232">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="e925d-233">`CourseAssignments` fournit l’accès aux entités `Course` associées.</span><span class="sxs-lookup"><span data-stu-id="e925d-233">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Instructor-to-Courses m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="e925d-235">La méthode `Single` est utilisée sur une collection quand la collection ne compte qu’un seul élément.</span><span class="sxs-lookup"><span data-stu-id="e925d-235">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="e925d-236">La méthode `Single` lève une exception si la collection est vide ou s’il y a plusieurs éléments.</span><span class="sxs-lookup"><span data-stu-id="e925d-236">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="e925d-237">Une alternative est `SingleOrDefault`, qui renvoie une valeur par défaut (Null dans ce cas) si la collection est vide.</span><span class="sxs-lookup"><span data-stu-id="e925d-237">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span>

<span data-ttu-id="e925d-238">Le code suivant renseigne la propriété `Enrollments` du modèle de vue quand un cours est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="e925d-238">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_SelectCourse)]

### <a name="update-the-instructors-index-page"></a><span data-ttu-id="e925d-239">Mettre à jour la page d’index des formateurs</span><span class="sxs-lookup"><span data-stu-id="e925d-239">Update the instructors Index page</span></span>

<span data-ttu-id="e925d-240">Mettez à jour *Pages/Instructors/Index.cshtml* avec le code suivant.</span><span class="sxs-lookup"><span data-stu-id="e925d-240">Update *Pages/Instructors/Index.cshtml* with the following code.</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Instructors/Index.cshtml?highlight=1,5,8,16-21,25-32,43-57,67-102,104-126)]

<span data-ttu-id="e925d-241">Le code précédent apporte les modifications suivantes :</span><span class="sxs-lookup"><span data-stu-id="e925d-241">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="e925d-242">Il met à jour la directive `page` en remplaçant `@page` par `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="e925d-242">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="e925d-243">`"{id:int?}"` est un modèle de route.</span><span class="sxs-lookup"><span data-stu-id="e925d-243">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="e925d-244">Le modèle de route change les chaînes de requête entières dans l’URL en données de route.</span><span class="sxs-lookup"><span data-stu-id="e925d-244">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="e925d-245">Par exemple, si vous cliquez sur le lien **Select** pour un formateur avec seulement la directive `@page`, une URL comme celle-ci est générée :</span><span class="sxs-lookup"><span data-stu-id="e925d-245">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

  `https://localhost:5001/Instructors?id=2`

  <span data-ttu-id="e925d-246">Quand la directive de page est `@page "{id:int?}"`, l’URL est :</span><span class="sxs-lookup"><span data-stu-id="e925d-246">When the page directive is `@page "{id:int?}"`, the URL is:</span></span>

  `https://localhost:5001/Instructors/2`

* <span data-ttu-id="e925d-247">Ajoute une colonne **Office** qui affiche `item.OfficeAssignment.Location` seulement si `item.OfficeAssignment` n’a pas la valeur Null.</span><span class="sxs-lookup"><span data-stu-id="e925d-247">Adds an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="e925d-248">Comme il s’agit d’une relation un-à-zéro-ou-un, il se peut qu’il n’y ait pas d’entité OfficeAssignment associée.</span><span class="sxs-lookup"><span data-stu-id="e925d-248">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="e925d-249">Ajoute une colonne **Courses** qui affiche les cours animés par chaque formateur.</span><span class="sxs-lookup"><span data-stu-id="e925d-249">Adds a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="e925d-250">Consultez [Conversion de ligne explicite avec `@:`](xref:mvc/views/razor#explicit-line-transition-with-) pour en savoir plus sur cette syntaxe razor.</span><span class="sxs-lookup"><span data-stu-id="e925d-250">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="e925d-251">Ajoute du code qui ajoute dynamiquement `class="success"` à l’élément `tr` du formateur et du cours sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="e925d-251">Adds code that dynamically adds `class="success"` to the `tr` element of the selected instructor and course.</span></span> <span data-ttu-id="e925d-252">Cela définit une couleur d’arrière-plan pour la ligne sélectionnée à l’aide d’une classe d’amorçage.</span><span class="sxs-lookup"><span data-stu-id="e925d-252">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="e925d-253">Ajoute un nouveau lien hypertexte libellé **Select**.</span><span class="sxs-lookup"><span data-stu-id="e925d-253">Adds a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="e925d-254">Ce lien envoie l’ID du formateur sélectionné à la méthode `Index`, et définit une couleur d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="e925d-254">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

* <span data-ttu-id="e925d-255">Ajoute un tableau de cours pour l’instructeur sélectionné.</span><span class="sxs-lookup"><span data-stu-id="e925d-255">Adds a table of courses for the selected Instructor.</span></span>

* <span data-ttu-id="e925d-256">Ajoute un tableau d’inscriptions d’étudiants pour le cours sélectionné.</span><span class="sxs-lookup"><span data-stu-id="e925d-256">Adds a table of student enrollments for the selected course.</span></span>

<span data-ttu-id="e925d-257">Exécutez l’application et sélectionnez l’onglet **Instructors**. La page affiche le `Location` (bureau) de l’entité `OfficeAssignment` associée.</span><span class="sxs-lookup"><span data-stu-id="e925d-257">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="e925d-258">Si `OfficeAssignment` a la valeur Null, une cellule de tableau vide est affichée.</span><span class="sxs-lookup"><span data-stu-id="e925d-258">If `OfficeAssignment` is null, an empty table cell is displayed.</span></span>

<span data-ttu-id="e925d-259">Cliquez sur le lien **Select** pour un formateur.</span><span class="sxs-lookup"><span data-stu-id="e925d-259">Click on the **Select** link for an instructor.</span></span> <span data-ttu-id="e925d-260">Le style de ligne change et les cours attribués à ce formateur s’affichent.</span><span class="sxs-lookup"><span data-stu-id="e925d-260">The row style changes and courses assigned to that instructor are displayed.</span></span>

<span data-ttu-id="e925d-261">Sélectionnez un cours pour afficher la liste des étudiants inscrits et leurs notes.</span><span class="sxs-lookup"><span data-stu-id="e925d-261">Select a course to see the list of enrolled students and their grades.</span></span>

![Page d’index des formateurs avec un formateur et un cours sélectionnés](read-related-data/_static/instructors-index30.png)

## <a name="using-single"></a><span data-ttu-id="e925d-263">Utilisation de Single</span><span class="sxs-lookup"><span data-stu-id="e925d-263">Using Single</span></span>

<span data-ttu-id="e925d-264">La méthode `Single` peut passer la condition `Where` au lieu d’appeler la méthode `Where` séparément :</span><span class="sxs-lookup"><span data-stu-id="e925d-264">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="e925d-265">L’utilisation de `Single` avec une condition Where est une question de préférence personnelle.</span><span class="sxs-lookup"><span data-stu-id="e925d-265">Use of `Single` with a Where condition is a matter of personal preference.</span></span> <span data-ttu-id="e925d-266">Elle n’offre pas d’avantages par rapport à la méthode `Where`.</span><span class="sxs-lookup"><span data-stu-id="e925d-266">It provides no benefits over using the `Where` method.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="e925d-267">Chargement explicite</span><span class="sxs-lookup"><span data-stu-id="e925d-267">Explicit loading</span></span>

<span data-ttu-id="e925d-268">Le code actuel spécifie le chargement hâtif pour `Enrollments` et `Students` :</span><span class="sxs-lookup"><span data-stu-id="e925d-268">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_EagerLoading&highlight=6-9)]

<span data-ttu-id="e925d-269">Supposez que les utilisateurs souhaitent rarement voir les inscriptions à un cours.</span><span class="sxs-lookup"><span data-stu-id="e925d-269">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="e925d-270">Dans ce cas, une optimisation consisterait à charger les données d’inscription uniquement si elles sont demandées.</span><span class="sxs-lookup"><span data-stu-id="e925d-270">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="e925d-271">Dans cette section, la méthode `OnGetAsync` est mise à jour de façon à utiliser le chargement explicite de `Enrollments` et `Students`.</span><span class="sxs-lookup"><span data-stu-id="e925d-271">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="e925d-272">Mettez à jour *Pages/Instructors/Index.cshtml.cs* avec le code suivant.</span><span class="sxs-lookup"><span data-stu-id="e925d-272">Update *Pages/Instructors/Index.cshtml.cs* with the following code.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Instructors/Index.cshtml.cs?highlight=31-35,52-56)]

<span data-ttu-id="e925d-273">Le code précédent supprime les appels de méthode *ThenInclude* pour les données sur les inscriptions et les étudiants.</span><span class="sxs-lookup"><span data-stu-id="e925d-273">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="e925d-274">Si un cours est sélectionné, le code de chargement explicite récupère :</span><span class="sxs-lookup"><span data-stu-id="e925d-274">If a course is selected, the explicit loading code retrieves:</span></span>

* <span data-ttu-id="e925d-275">Les entités `Enrollment` pour le cours sélectionné.</span><span class="sxs-lookup"><span data-stu-id="e925d-275">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="e925d-276">Les entités `Student` pour chaque `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="e925d-276">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="e925d-277">Notez que le code précédent commente `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="e925d-277">Notice that the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="e925d-278">Les propriétés de navigation peuvent être chargées explicitement uniquement pour les entités suivies.</span><span class="sxs-lookup"><span data-stu-id="e925d-278">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="e925d-279">Testez l’application.</span><span class="sxs-lookup"><span data-stu-id="e925d-279">Test the app.</span></span> <span data-ttu-id="e925d-280">Du point de vue d’un utilisateur, l’application se comporte de façon identique à la version précédente.</span><span class="sxs-lookup"><span data-stu-id="e925d-280">From a user's perspective, the app behaves identically to the previous version.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e925d-281">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="e925d-281">Next steps</span></span>

<span data-ttu-id="e925d-282">Le didacticiel suivant montre comment mettre à jour les données associées.</span><span class="sxs-lookup"><span data-stu-id="e925d-282">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="e925d-283">[Tutoriel précédent](xref:data/ef-rp/complex-data-model)
>[Tutoriel suivant](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="e925d-283">[Previous tutorial](xref:data/ef-rp/complex-data-model)
[Next tutorial](xref:data/ef-rp/update-related-data)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e925d-284">Dans ce didacticiel, nous allons lire et afficher des données associées.</span><span class="sxs-lookup"><span data-stu-id="e925d-284">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="e925d-285">Les données associées sont des données qu’EF Core charge dans des propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="e925d-285">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="e925d-286">Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, [téléchargez ou affichez l’application terminée](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="e925d-286">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="e925d-287">[Télécharger les instructions](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="e925d-287">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="e925d-288">Les illustrations suivantes montrent les pages terminées pour ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="e925d-288">The following illustrations show the completed pages for this tutorial:</span></span>

![Page d’index des cours](read-related-data/_static/courses-index.png)

![Page d’index des formateurs](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="e925d-291">Chargement hâtif, explicite et différé de données associées</span><span class="sxs-lookup"><span data-stu-id="e925d-291">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="e925d-292">EF Core peut charger des données associées dans les propriétés de navigation d’une entité de plusieurs manières :</span><span class="sxs-lookup"><span data-stu-id="e925d-292">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="e925d-293">[Chargement hâtif](/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="e925d-293">[Eager loading](/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="e925d-294">Le chargement hâtif a lieu quand une requête pour un type d’entité charge également des entités associées.</span><span class="sxs-lookup"><span data-stu-id="e925d-294">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="e925d-295">Quand l’entité est lue, ses données associées sont récupérées.</span><span class="sxs-lookup"><span data-stu-id="e925d-295">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="e925d-296">Cela génère en général une requête de jointure unique qui récupère toutes les données nécessaires.</span><span class="sxs-lookup"><span data-stu-id="e925d-296">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="e925d-297">EF Core émet plusieurs requêtes pour certains types de chargement hâtif.</span><span class="sxs-lookup"><span data-stu-id="e925d-297">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="e925d-298">L’émission de requêtes multiples peut être plus efficace que ce n’était le cas pour certaines requêtes dans EF6 où une seule requête était émise.</span><span class="sxs-lookup"><span data-stu-id="e925d-298">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="e925d-299">Le chargement hâtif est spécifié avec les méthodes `Include` et `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="e925d-299">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Exemple de chargement hâtif](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="e925d-301">Le chargement hâtif envoie plusieurs requêtes quand une navigation dans la collection est incluse :</span><span class="sxs-lookup"><span data-stu-id="e925d-301">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="e925d-302">Une requête pour la requête principale</span><span class="sxs-lookup"><span data-stu-id="e925d-302">One query for the main query</span></span> 
  * <span data-ttu-id="e925d-303">Une requête pour chaque « périmètre » de collection dans l’arborescence de la charge.</span><span class="sxs-lookup"><span data-stu-id="e925d-303">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="e925d-304">Requêtes distinctes avec `Load` : Les données peuvent être récupérées dans des requêtes distinctes, et EF Core « corrige » les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="e925d-304">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="e925d-305">Le terme « corrige » signifie qu’EF Core renseigne automatiquement les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="e925d-305">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="e925d-306">Les requêtes distinctes avec `Load` s’apparentent plus au chargement explicite qu’au chargement hâtif.</span><span class="sxs-lookup"><span data-stu-id="e925d-306">Separate queries with `Load` is more like explicit loading than eager loading.</span></span>

  ![Exemple de requêtes distinctes](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="e925d-308">Remarque : EF Core corrige automatiquement les propriétés de navigation vers d’autres entités qui étaient précédemment chargées dans l’instance de contexte.</span><span class="sxs-lookup"><span data-stu-id="e925d-308">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="e925d-309">Même si les données pour une propriété de navigation ne sont *pas* explicitement incluses, la propriété peut toujours être renseignée si toutes ou une partie des entités associées ont été précédemment chargées.</span><span class="sxs-lookup"><span data-stu-id="e925d-309">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="e925d-310">[Chargement explicite](/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="e925d-310">[Explicit loading](/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="e925d-311">Quand l’entité est lue pour la première fois, les données associées ne sont pas récupérées.</span><span class="sxs-lookup"><span data-stu-id="e925d-311">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="e925d-312">Vous devez écrire du code pour récupérer les données associées en cas de besoin.</span><span class="sxs-lookup"><span data-stu-id="e925d-312">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="e925d-313">En cas de chargement explicite avec des requêtes distinctes, plusieurs requêtes sont envoyées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="e925d-313">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="e925d-314">Avec le chargement explicite, le code spécifie les propriétés de navigation à charger.</span><span class="sxs-lookup"><span data-stu-id="e925d-314">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="e925d-315">Utilisez la méthode `Load` pour effectuer le chargement explicite.</span><span class="sxs-lookup"><span data-stu-id="e925d-315">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="e925d-316">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="e925d-316">For example:</span></span>

  ![Exemple de chargement explicite](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="e925d-318">[Chargement différé](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="e925d-318">[Lazy loading](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="e925d-319">[Le chargement différé a été ajouté à EF Core dans la version 2.1](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="e925d-319">[Lazy loading was added to EF Core in version 2.1](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="e925d-320">Quand l’entité est lue pour la première fois, les données associées ne sont pas récupérées.</span><span class="sxs-lookup"><span data-stu-id="e925d-320">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="e925d-321">Lors du premier accès à une propriété de navigation, les données requises pour cette propriété de navigation sont récupérées automatiquement.</span><span class="sxs-lookup"><span data-stu-id="e925d-321">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="e925d-322">Une requête est envoyée à la base de données chaque fois qu’une propriété de navigation est sollicitée pour la première fois.</span><span class="sxs-lookup"><span data-stu-id="e925d-322">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="e925d-323">L’opérateur `Select` charge uniquement les données associées nécessaires.</span><span class="sxs-lookup"><span data-stu-id="e925d-323">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-course-page-that-displays-department-name"></a><span data-ttu-id="e925d-324">Créer une page Course qui affiche le nom du département</span><span class="sxs-lookup"><span data-stu-id="e925d-324">Create a Course page that displays department name</span></span>

<span data-ttu-id="e925d-325">L’entité Course comprend une propriété de navigation qui contient l’entité `Department`.</span><span class="sxs-lookup"><span data-stu-id="e925d-325">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="e925d-326">L’entité `Department` contient le département auquel le cours est affecté.</span><span class="sxs-lookup"><span data-stu-id="e925d-326">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="e925d-327">Pour afficher le nom du département affecté dans une liste de cours</span><span class="sxs-lookup"><span data-stu-id="e925d-327">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="e925d-328">Obtenez la propriété `Name` à partir de l’entité `Department`.</span><span class="sxs-lookup"><span data-stu-id="e925d-328">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="e925d-329">L’entité `Department` provient de la propriété de navigation `Course.Department`.</span><span class="sxs-lookup"><span data-stu-id="e925d-329">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![Course.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>

### <a name="scaffold-the-course-model"></a><span data-ttu-id="e925d-331">Génération automatique du modèle Course</span><span class="sxs-lookup"><span data-stu-id="e925d-331">Scaffold the Course model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e925d-332">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e925d-332">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="e925d-333">Suivez les instructions fournies dans [Générer automatiquement le modèle d’étudiant](xref:data/ef-rp/intro#scaffold-student-pages) et utilisez `Course` pour la classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="e925d-333">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-student-pages) and use `Course` for the model class.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e925d-334">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e925d-334">Visual Studio Code</span></span>](#tab/visual-studio-code)

 <span data-ttu-id="e925d-335">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="e925d-335">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

---

<span data-ttu-id="e925d-336">La commande précédente génère automatiquement le modèle `Course`.</span><span class="sxs-lookup"><span data-stu-id="e925d-336">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="e925d-337">Ouvrez le projet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e925d-337">Open the project in Visual Studio.</span></span>

<span data-ttu-id="e925d-338">Ouvrez *Pages/Courses/Index.cshtml.cs* et examinez la méthode `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="e925d-338">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="e925d-339">Le moteur de génération de modèles automatique a spécifié le chargement hâtif pour la propriété de navigation `Department`.</span><span class="sxs-lookup"><span data-stu-id="e925d-339">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="e925d-340">La méthode `Include` spécifie le chargement hâtif.</span><span class="sxs-lookup"><span data-stu-id="e925d-340">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="e925d-341">Exécutez l’application et sélectionnez le lien **Courses**.</span><span class="sxs-lookup"><span data-stu-id="e925d-341">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="e925d-342">La colonne Department affiche le `DepartmentID`, ce qui n’est pas utile.</span><span class="sxs-lookup"><span data-stu-id="e925d-342">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="e925d-343">Mettez à jour la méthode `OnGetAsync` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e925d-343">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="e925d-344">Le code précédent ajoute `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="e925d-344">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="e925d-345">`AsNoTracking` améliore les performances, car les entités retournées ne sont pas suivies.</span><span class="sxs-lookup"><span data-stu-id="e925d-345">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="e925d-346">Les entités ne sont pas suivies, car elles ne sont pas mises à jour dans le contexte actuel.</span><span class="sxs-lookup"><span data-stu-id="e925d-346">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="e925d-347">Mettez à jour *Pages/Courses/Index.cshtml* avec le balisage en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="e925d-347">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="e925d-348">Les modifications suivantes ont été apportées au code généré automatiquement :</span><span class="sxs-lookup"><span data-stu-id="e925d-348">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="e925d-349">Changement de l’en-tête : Index a été remplacé par Course.</span><span class="sxs-lookup"><span data-stu-id="e925d-349">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="e925d-350">Ajout d’une colonne **Number** qui affiche la valeur de la propriété `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="e925d-350">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="e925d-351">Par défaut, les clés primaires ne sont pas générées automatiquement, car elles ne sont normalement pas significatives pour les utilisateurs finaux.</span><span class="sxs-lookup"><span data-stu-id="e925d-351">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="e925d-352">Toutefois, dans le cas présent la clé primaire est significative.</span><span class="sxs-lookup"><span data-stu-id="e925d-352">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="e925d-353">Modification de la colonne **Department** afin d’afficher le nom du département.</span><span class="sxs-lookup"><span data-stu-id="e925d-353">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="e925d-354">Le code affiche la propriété `Name` de l’entité `Department` qui est chargée dans la propriété de navigation `Department` :</span><span class="sxs-lookup"><span data-stu-id="e925d-354">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="e925d-355">Exécutez l’application et sélectionnez l’onglet **Courses** pour afficher la liste avec les noms des départements.</span><span class="sxs-lookup"><span data-stu-id="e925d-355">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Page d’index des cours](read-related-data/_static/courses-index.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a><span data-ttu-id="e925d-357">Chargement de données associées avec Select</span><span class="sxs-lookup"><span data-stu-id="e925d-357">Loading related data with Select</span></span>

<span data-ttu-id="e925d-358">La méthode `OnGetAsync` charge les données associées avec la méthode `Include` :</span><span class="sxs-lookup"><span data-stu-id="e925d-358">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="e925d-359">L’opérateur `Select` charge uniquement les données associées nécessaires.</span><span class="sxs-lookup"><span data-stu-id="e925d-359">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="e925d-360">Pour les éléments uniques, comme `Department.Name`, il utilise une jointure interne SQL.</span><span class="sxs-lookup"><span data-stu-id="e925d-360">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="e925d-361">Pour les collections, il utilise un autre accès à la base de données, mais c’est aussi le cas de l’opérateur `Include` sur les collections.</span><span class="sxs-lookup"><span data-stu-id="e925d-361">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="e925d-362">Le code suivant charge les données associées avec la méthode `Select` :</span><span class="sxs-lookup"><span data-stu-id="e925d-362">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="e925d-363">Voici le `CourseViewModel` :</span><span class="sxs-lookup"><span data-stu-id="e925d-363">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="e925d-364">Pour obtenir un exemple complet, consultez [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) et [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs).</span><span class="sxs-lookup"><span data-stu-id="e925d-364">See [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="e925d-365">Créer une page Instructors qui affiche les cours et les inscriptions</span><span class="sxs-lookup"><span data-stu-id="e925d-365">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="e925d-366">Dans cette section, nous allons créer la page Instructors.</span><span class="sxs-lookup"><span data-stu-id="e925d-366">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="e925d-367">![Page d’index des formateurs](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="e925d-367">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="e925d-368">Cette page lit et affiche les données associées comme suit :</span><span class="sxs-lookup"><span data-stu-id="e925d-368">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="e925d-369">La liste des formateurs affiche des données associées de l’entité `OfficeAssignment` (Office dans l’image précédente).</span><span class="sxs-lookup"><span data-stu-id="e925d-369">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="e925d-370">Il existe une relation un-à-zéro-ou-un entre les entités `Instructor` et `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="e925d-370">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="e925d-371">Le chargement hâtif est utilisé pour les entités `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="e925d-371">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="e925d-372">Le chargement hâtif est généralement plus efficace quand les données associées doivent être affichées.</span><span class="sxs-lookup"><span data-stu-id="e925d-372">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="e925d-373">Ici, les affectations de bureau pour les formateurs sont affichées.</span><span class="sxs-lookup"><span data-stu-id="e925d-373">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="e925d-374">Quand l’utilisateur sélectionne un formateur (Harui dans l’image précédente), les entités `Course` associées sont affichées.</span><span class="sxs-lookup"><span data-stu-id="e925d-374">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="e925d-375">Il existe une relation plusieurs-à-plusieurs entre les entités `Instructor` et `Course`.</span><span class="sxs-lookup"><span data-stu-id="e925d-375">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="e925d-376">Le chargement hâtif est utilisé pour les entités `Course` et leurs entités `Department` associées.</span><span class="sxs-lookup"><span data-stu-id="e925d-376">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="e925d-377">Dans le cas présent, des requêtes distinctes peuvent être plus efficaces, car seuls les cours du formateur sélectionné sont nécessaires.</span><span class="sxs-lookup"><span data-stu-id="e925d-377">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="e925d-378">Cet exemple montre comment utiliser le chargement hâtif pour des propriétés de navigation dans des entités qui se trouvent dans des propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="e925d-378">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="e925d-379">Quand l’utilisateur sélectionne un cours (Chemistry dans l’image précédente), les données associées de l’entité `Enrollments` sont affichées.</span><span class="sxs-lookup"><span data-stu-id="e925d-379">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="e925d-380">Dans l’image précédente, le nom et la note de l’étudiant sont affichés.</span><span class="sxs-lookup"><span data-stu-id="e925d-380">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="e925d-381">Il existe une relation un-à-plusieurs entre les entités `Course` et `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="e925d-381">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="e925d-382">Créer un modèle de vue pour la vue d’index des formateurs</span><span class="sxs-lookup"><span data-stu-id="e925d-382">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="e925d-383">La page sur les formateurs affiche les données de trois tables différentes.</span><span class="sxs-lookup"><span data-stu-id="e925d-383">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="e925d-384">Nous allons créer un modèle de vue qui comprend les trois entités représentant les trois tables.</span><span class="sxs-lookup"><span data-stu-id="e925d-384">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="e925d-385">Dans le dossier *SchoolViewModels*, créez *InstructorIndexData.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e925d-385">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="e925d-386">Générer automatiquement le modèle Instructor</span><span class="sxs-lookup"><span data-stu-id="e925d-386">Scaffold the Instructor model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e925d-387">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e925d-387">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="e925d-388">Suivez les instructions fournies dans [Générer automatiquement le modèle d’étudiant](xref:data/ef-rp/intro#scaffold-student-pages) et utilisez `Instructor` pour la classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="e925d-388">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-student-pages) and use `Instructor` for the model class.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e925d-389">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e925d-389">Visual Studio Code</span></span>](#tab/visual-studio-code)

 <span data-ttu-id="e925d-390">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="e925d-390">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

---

<span data-ttu-id="e925d-391">La commande précédente génère automatiquement le modèle `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="e925d-391">The preceding command scaffolds the `Instructor` model.</span></span> 
<span data-ttu-id="e925d-392">Exécutez l’application et accédez à la page des formateurs.</span><span class="sxs-lookup"><span data-stu-id="e925d-392">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="e925d-393">Remplacez *Pages/Instructors/Index.cshtml.cs* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e925d-393">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

<span data-ttu-id="e925d-394">La méthode `OnGetAsync` accepte des données de route facultatives pour l’ID du formateur sélectionné.</span><span class="sxs-lookup"><span data-stu-id="e925d-394">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="e925d-395">Examinez la requête dans le fichier *Pages/Instructors/Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="e925d-395">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="e925d-396">La requête comporte deux Include :</span><span class="sxs-lookup"><span data-stu-id="e925d-396">The query has two includes:</span></span>

* <span data-ttu-id="e925d-397">`OfficeAssignment`: Affiché dans la [vue Instructors](#IP).</span><span class="sxs-lookup"><span data-stu-id="e925d-397">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="e925d-398">`CourseAssignments`: Qui affiche les cours dispensés.</span><span class="sxs-lookup"><span data-stu-id="e925d-398">`CourseAssignments`: Which brings in the courses taught.</span></span>

### <a name="update-the-instructors-index-page"></a><span data-ttu-id="e925d-399">Mettre à jour la page d’index des formateurs</span><span class="sxs-lookup"><span data-stu-id="e925d-399">Update the instructors Index page</span></span>

<span data-ttu-id="e925d-400">Mettez à jour *Pages/Instructors/Index.cshtml* avec le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="e925d-400">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="e925d-401">Le balisage précédent apporte les modifications suivantes :</span><span class="sxs-lookup"><span data-stu-id="e925d-401">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="e925d-402">Il met à jour la directive `page` en remplaçant `@page` par `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="e925d-402">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="e925d-403">`"{id:int?}"` est un modèle de route.</span><span class="sxs-lookup"><span data-stu-id="e925d-403">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="e925d-404">Le modèle de route change les chaînes de requête entières dans l’URL en données de route.</span><span class="sxs-lookup"><span data-stu-id="e925d-404">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="e925d-405">Par exemple, si vous cliquez sur le lien **Select** pour un formateur avec seulement la directive `@page`, une URL comme celle-ci est générée :</span><span class="sxs-lookup"><span data-stu-id="e925d-405">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

  `http://localhost:1234/Instructors?id=2`

  <span data-ttu-id="e925d-406">Quand la directive de page est `@page "{id:int?}"`, l’URL précédente est :</span><span class="sxs-lookup"><span data-stu-id="e925d-406">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

  `http://localhost:1234/Instructors/2`

* <span data-ttu-id="e925d-407">Le titre de la page est **Instructors**.</span><span class="sxs-lookup"><span data-stu-id="e925d-407">Page title is **Instructors**.</span></span>
* <span data-ttu-id="e925d-408">Il ajoute une colonne **Office** qui affiche `item.OfficeAssignment.Location` uniquement si `item.OfficeAssignment` n’est pas Null.</span><span class="sxs-lookup"><span data-stu-id="e925d-408">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="e925d-409">Comme il s’agit d’une relation un-à-zéro-ou-un, il se peut qu’il n’y ait pas d’entité OfficeAssignment associée.</span><span class="sxs-lookup"><span data-stu-id="e925d-409">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="e925d-410">Vous avez ajouté une colonne **Courses** qui affiche les cours animés par chaque formateur.</span><span class="sxs-lookup"><span data-stu-id="e925d-410">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="e925d-411">Consultez [Conversion de ligne explicite avec `@:`](xref:mvc/views/razor#explicit-line-transition-with-) pour en savoir plus sur cette syntaxe razor.</span><span class="sxs-lookup"><span data-stu-id="e925d-411">See [Explicit line transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="e925d-412">Vous avez ajouté un code qui ajoute dynamiquement `class="success"` à l’élément `tr` du formateur sélectionné.</span><span class="sxs-lookup"><span data-stu-id="e925d-412">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="e925d-413">Cela définit une couleur d’arrière-plan pour la ligne sélectionnée à l’aide d’une classe d’amorçage.</span><span class="sxs-lookup"><span data-stu-id="e925d-413">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="e925d-414">Ajout d’un nouveau lien hypertexte libellé **Select**.</span><span class="sxs-lookup"><span data-stu-id="e925d-414">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="e925d-415">Ce lien envoie l’ID du formateur sélectionné à la méthode `Index`, et définit une couleur d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="e925d-415">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="e925d-416">Exécutez l’application et sélectionnez le lien **Instructors**. La page affiche le `Location` (bureau) de l’entité `OfficeAssignment` associée.</span><span class="sxs-lookup"><span data-stu-id="e925d-416">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="e925d-417">Si OfficeAssignment\` est Null, une cellule de table vide est affichée.</span><span class="sxs-lookup"><span data-stu-id="e925d-417">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

<span data-ttu-id="e925d-418">Cliquez sur le lien **Select**.</span><span class="sxs-lookup"><span data-stu-id="e925d-418">Click on the **Select** link.</span></span> <span data-ttu-id="e925d-419">Le style de ligne change.</span><span class="sxs-lookup"><span data-stu-id="e925d-419">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="e925d-420">Ajouter les cours dispensés par le formateur sélectionné</span><span class="sxs-lookup"><span data-stu-id="e925d-420">Add courses taught by selected instructor</span></span>

<span data-ttu-id="e925d-421">Mettez à jour la méthode `OnGetAsync` dans *Pages/Instructors/Index.cshtml.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e925d-421">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="e925d-422">Ajouter `public int CourseID { get; set; }`</span><span class="sxs-lookup"><span data-stu-id="e925d-422">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="e925d-423">Examinez la requête mise à jour :</span><span class="sxs-lookup"><span data-stu-id="e925d-423">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="e925d-424">La requête précédente ajoute les entités `Department`.</span><span class="sxs-lookup"><span data-stu-id="e925d-424">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="e925d-425">Le code suivant s’exécute quand un formateur est sélectionné (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="e925d-425">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="e925d-426">Le formateur sélectionné est récupéré à partir de la liste des formateurs dans le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="e925d-426">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="e925d-427">La propriété `Courses` du modèle de vue est chargée avec les entités `Course` de la propriété de navigation `CourseAssignments` de ce formateur.</span><span class="sxs-lookup"><span data-stu-id="e925d-427">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="e925d-428">La méthode `Where` retourne une collection.</span><span class="sxs-lookup"><span data-stu-id="e925d-428">The `Where` method returns a collection.</span></span> <span data-ttu-id="e925d-429">Dans la méthode `Where` précédente, une seule entité `Instructor` est retournée.</span><span class="sxs-lookup"><span data-stu-id="e925d-429">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="e925d-430">La méthode `Single` convertit la collection en une seule entité `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="e925d-430">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="e925d-431">L’entité `Instructor` fournit l’accès à la propriété `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="e925d-431">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="e925d-432">`CourseAssignments` fournit l’accès aux entités `Course` associées.</span><span class="sxs-lookup"><span data-stu-id="e925d-432">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Instructor-to-Courses m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="e925d-434">La méthode `Single` est utilisée sur une collection quand la collection ne compte qu’un seul élément.</span><span class="sxs-lookup"><span data-stu-id="e925d-434">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="e925d-435">La méthode `Single` lève une exception si la collection est vide ou s’il y a plusieurs éléments.</span><span class="sxs-lookup"><span data-stu-id="e925d-435">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="e925d-436">Une alternative est `SingleOrDefault`, qui retourne une valeur par défaut (Null dans le cas présent) si la collection est vide.</span><span class="sxs-lookup"><span data-stu-id="e925d-436">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="e925d-437">L’utilisation de `SingleOrDefault` sur une collection vide :</span><span class="sxs-lookup"><span data-stu-id="e925d-437">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="e925d-438">Génère une exception (à cause de la tentative de trouver une propriété `Courses` sur une référence Null).</span><span class="sxs-lookup"><span data-stu-id="e925d-438">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="e925d-439">Le message d’exception indique moins clairement la cause du problème.</span><span class="sxs-lookup"><span data-stu-id="e925d-439">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="e925d-440">Le code suivant renseigne la propriété `Enrollments` du modèle de vue quand un cours est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="e925d-440">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="e925d-441">Ajoutez le balisage suivant à la fin de la page Razor *Pages/Instructors/Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="e925d-441">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="e925d-442">Le balisage précédent affiche une liste de cours associés à un formateur quand un formateur est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="e925d-442">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="e925d-443">Testez l’application.</span><span class="sxs-lookup"><span data-stu-id="e925d-443">Test the app.</span></span> <span data-ttu-id="e925d-444">Cliquez sur un lien **Select** dans la page des formateurs.</span><span class="sxs-lookup"><span data-stu-id="e925d-444">Click on a **Select** link on the instructors page.</span></span>

### <a name="show-student-data"></a><span data-ttu-id="e925d-445">Afficher les données sur les étudiants</span><span class="sxs-lookup"><span data-stu-id="e925d-445">Show student data</span></span>

<span data-ttu-id="e925d-446">Dans cette section, nous allons mettre à jour l’application afin d’afficher les données sur les étudiants pour le cours sélectionné.</span><span class="sxs-lookup"><span data-stu-id="e925d-446">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="e925d-447">Mettez à jour la requête dans la méthode `OnGetAsync` dans *Pages/Instructors/Index.cshtml.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e925d-447">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="e925d-448">Mettez à jour *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e925d-448">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="e925d-449">Ajoutez le balisage suivant à la fin du fichier :</span><span class="sxs-lookup"><span data-stu-id="e925d-449">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="e925d-450">Le balisage précédent affiche une liste des étudiants qui sont inscrits au cours sélectionné.</span><span class="sxs-lookup"><span data-stu-id="e925d-450">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="e925d-451">Actualisez la page et sélectionnez un formateur.</span><span class="sxs-lookup"><span data-stu-id="e925d-451">Refresh the page and select an instructor.</span></span> <span data-ttu-id="e925d-452">Sélectionnez un cours pour afficher la liste des étudiants inscrits et leurs notes.</span><span class="sxs-lookup"><span data-stu-id="e925d-452">Select a course to see the list of enrolled students and their grades.</span></span>

![Page d’index des formateurs avec un formateur et un cours sélectionnés](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="e925d-454">Utilisation de Single</span><span class="sxs-lookup"><span data-stu-id="e925d-454">Using Single</span></span>

<span data-ttu-id="e925d-455">La méthode `Single` peut passer la condition `Where` au lieu d’appeler la méthode `Where` séparément :</span><span class="sxs-lookup"><span data-stu-id="e925d-455">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="e925d-456">L’approche précédente faisant appel à `Single` ne présente aucun avantage par rapport à l’utilisation de `Where`.</span><span class="sxs-lookup"><span data-stu-id="e925d-456">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="e925d-457">Certains développeurs préfèrent le style d’approche `Single`.</span><span class="sxs-lookup"><span data-stu-id="e925d-457">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="e925d-458">Chargement explicite</span><span class="sxs-lookup"><span data-stu-id="e925d-458">Explicit loading</span></span>

<span data-ttu-id="e925d-459">Le code actuel spécifie le chargement hâtif pour `Enrollments` et `Students` :</span><span class="sxs-lookup"><span data-stu-id="e925d-459">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="e925d-460">Supposez que les utilisateurs souhaitent rarement voir les inscriptions à un cours.</span><span class="sxs-lookup"><span data-stu-id="e925d-460">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="e925d-461">Dans ce cas, une optimisation consisterait à charger les données d’inscription uniquement si elles sont demandées.</span><span class="sxs-lookup"><span data-stu-id="e925d-461">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="e925d-462">Dans cette section, la méthode `OnGetAsync` est mise à jour de façon à utiliser le chargement explicite de `Enrollments` et `Students`.</span><span class="sxs-lookup"><span data-stu-id="e925d-462">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="e925d-463">Mettez à jour la méthode `OnGetAsync` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e925d-463">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="e925d-464">Le code précédent supprime les appels de méthode *ThenInclude* pour les données sur les inscriptions et les étudiants.</span><span class="sxs-lookup"><span data-stu-id="e925d-464">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="e925d-465">Si un cours est sélectionné, le code en surbrillance récupère :</span><span class="sxs-lookup"><span data-stu-id="e925d-465">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="e925d-466">Les entités `Enrollment` pour le cours sélectionné.</span><span class="sxs-lookup"><span data-stu-id="e925d-466">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="e925d-467">Les entités `Student` pour chaque `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="e925d-467">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="e925d-468">Notez que le code précédent commente `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="e925d-468">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="e925d-469">Les propriétés de navigation peuvent être chargées explicitement uniquement pour les entités suivies.</span><span class="sxs-lookup"><span data-stu-id="e925d-469">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="e925d-470">Testez l’application.</span><span class="sxs-lookup"><span data-stu-id="e925d-470">Test the app.</span></span> <span data-ttu-id="e925d-471">Du point de vue des utilisateurs, l’application se comporte comme la version précédente.</span><span class="sxs-lookup"><span data-stu-id="e925d-471">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="e925d-472">Le didacticiel suivant montre comment mettre à jour les données associées.</span><span class="sxs-lookup"><span data-stu-id="e925d-472">The next tutorial shows how to update related data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e925d-473">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e925d-473">Additional resources</span></span>

* [<span data-ttu-id="e925d-474">Version YouTube de ce tutoriel (partie 1)</span><span class="sxs-lookup"><span data-stu-id="e925d-474">YouTube version of this tutorial (part1)</span></span>](https://www.youtube.com/watch?v=PzKimUDmrvE)
* [<span data-ttu-id="e925d-475">Version YouTube de ce tutoriel (partie 2)</span><span class="sxs-lookup"><span data-stu-id="e925d-475">YouTube version of this tutorial (part2)</span></span>](https://www.youtube.com/watch?v=xvDDrIHv5ko)

>[!div class="step-by-step"]
><span data-ttu-id="e925d-476">[Précédent](xref:data/ef-rp/complex-data-model)
>[Suivant](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="e925d-476">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>

::: moniker-end