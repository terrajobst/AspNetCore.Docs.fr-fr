---
title: Pages Razor avec EF Core dans ASP.NET Core - Lire des données associées - 6 sur 8
author: rick-anderson
description: Dans ce didacticiel, nous allons lire et afficher des données associées, autrement dit des données chargées par Entity Framework dans des propriétés de navigation.
ms.author: riande
ms.date: 11/05/2017
uid: data/ef-rp/read-related-data
ms.openlocfilehash: e23b8bb870b329bd705fbc9548df62524303893e
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912319"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="d5a6e-103">Pages Razor avec EF Core dans ASP.NET Core - Lire des données associées - 6 sur 8</span><span class="sxs-lookup"><span data-stu-id="d5a6e-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="d5a6e-104">Par [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d5a6e-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="d5a6e-105">Dans ce didacticiel, nous allons lire et afficher des données associées.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="d5a6e-106">Les données associées sont des données qu’EF Core charge dans des propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="d5a6e-107">Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, [téléchargez ou affichez l’application terminée](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="d5a6e-107">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="d5a6e-108">[Télécharger les instructions](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="d5a6e-108">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

<span data-ttu-id="d5a6e-109">Les illustrations suivantes montrent les pages terminées pour ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-109">The following illustrations show the completed pages for this tutorial:</span></span>

![Page d’index des cours](read-related-data/_static/courses-index.png)

![Page d’index des formateurs](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="d5a6e-112">Chargement hâtif, explicite et différé de données associées</span><span class="sxs-lookup"><span data-stu-id="d5a6e-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="d5a6e-113">EF Core peut charger des données associées dans les propriétés de navigation d’une entité de plusieurs manières :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-113">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="d5a6e-114">[Chargement hâtif](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="d5a6e-114">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="d5a6e-115">Le chargement hâtif a lieu quand une requête pour un type d’entité charge également des entités associées.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-115">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="d5a6e-116">Quand l’entité est lue, ses données associées sont récupérées.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-116">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="d5a6e-117">Cela génère en général une requête de jointure unique qui récupère toutes les données nécessaires.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-117">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="d5a6e-118">EF Core émet plusieurs requêtes pour certains types de chargement hâtif.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-118">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="d5a6e-119">L’émission de requêtes multiples peut être plus efficace que ce n’était le cas pour certaines requêtes dans EF6 où une seule requête était émise.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-119">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="d5a6e-120">Le chargement hâtif est spécifié avec les méthodes `Include` et `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-120">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Exemple de chargement hâtif](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="d5a6e-122">Le chargement hâtif envoie plusieurs requêtes quand une navigation dans la collection est incluse :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-122">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="d5a6e-123">Une requête pour la requête principale</span><span class="sxs-lookup"><span data-stu-id="d5a6e-123">One query for the main query</span></span> 
  * <span data-ttu-id="d5a6e-124">Une requête pour chaque « périmètre » de collection dans l’arborescence de la charge.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-124">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="d5a6e-125">Requêtes distinctes avec `Load` : les données peuvent être récupérées dans des requêtes distinctes, et EF Core « corrige » les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-125">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="d5a6e-126">Le terme « corrige » signifie qu’EF Core renseigne automatiquement les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-126">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="d5a6e-127">Les requêtes distinctes avec `Load` s’apparentent plus au chargement explicite qu’au chargement hâtif.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-127">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

  ![Exemple de requêtes distinctes](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="d5a6e-129">Remarque : EF Core corrige automatiquement les propriétés de navigation vers d’autres entités qui étaient précédemment chargées dans l’instance de contexte.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-129">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="d5a6e-130">Même si les données pour une propriété de navigation ne sont *pas* explicitement incluses, la propriété peut toujours être renseignée si toutes ou une partie des entités associées ont été précédemment chargées.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-130">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="d5a6e-131">[Chargement explicite](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="d5a6e-131">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="d5a6e-132">Quand l’entité est lue pour la première fois, les données associées ne sont pas récupérées.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="d5a6e-133">Vous devez écrire du code pour récupérer les données associées en cas de besoin.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-133">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="d5a6e-134">En cas de chargement explicite avec des requêtes distinctes, plusieurs requêtes sont envoyées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-134">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="d5a6e-135">Avec le chargement explicite, le code spécifie les propriétés de navigation à charger.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-135">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="d5a6e-136">Utilisez la méthode `Load` pour effectuer le chargement explicite.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-136">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="d5a6e-137">Exemple :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-137">For example:</span></span>

  ![Exemple de chargement explicite](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="d5a6e-139">[Chargement différé](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="d5a6e-139">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="d5a6e-140">[Le chargement différé a été ajouté à EF Core dans la version 2.1](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="d5a6e-140">[Lazy loading was added to EF Core in version 2.1](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="d5a6e-141">Quand l’entité est lue pour la première fois, les données associées ne sont pas récupérées.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-141">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="d5a6e-142">Lors du premier accès à une propriété de navigation, les données requises pour cette propriété de navigation sont récupérées automatiquement.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-142">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="d5a6e-143">Une requête est envoyée à la base de données chaque fois qu’une propriété de navigation est sollicitée pour la première fois.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-143">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="d5a6e-144">L’opérateur `Select` charge uniquement les données associées nécessaires.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-144">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="d5a6e-145">Créer une page Courses qui affiche le nom du département</span><span class="sxs-lookup"><span data-stu-id="d5a6e-145">Create a Courses page that displays department name</span></span>

<span data-ttu-id="d5a6e-146">L’entité Course comprend une propriété de navigation qui contient l’entité `Department`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-146">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="d5a6e-147">L’entité `Department` contient le département auquel le cours est affecté.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-147">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="d5a6e-148">Pour afficher le nom du département affecté dans une liste de cours</span><span class="sxs-lookup"><span data-stu-id="d5a6e-148">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="d5a6e-149">Obtenez la propriété `Name` à partir de l’entité `Department`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-149">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="d5a6e-150">L’entité `Department` provient de la propriété de navigation `Course.Department`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-150">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![Course.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="d5a6e-152">Génération automatique du modèle Course</span><span class="sxs-lookup"><span data-stu-id="d5a6e-152">Scaffold the Course model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d5a6e-153">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d5a6e-153">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="d5a6e-154">Suivez les instructions fournies dans [Générer automatiquement le modèle d’étudiant](xref:data/ef-rp/intro#scaffold-the-student-model) et utilisez `Course` pour la classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-154">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Course` for the model class.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d5a6e-155">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="d5a6e-155">.NET Core CLI</span></span>](#tab/netcore-cli)

 <span data-ttu-id="d5a6e-156">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-156">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

------

<span data-ttu-id="d5a6e-157">La commande précédente génère automatiquement le modèle `Course`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-157">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="d5a6e-158">Ouvrez le projet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-158">Open the project in Visual Studio.</span></span>

<span data-ttu-id="d5a6e-159">Ouvrez *Pages/Courses/Index.cshtml.cs* et examinez la méthode `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-159">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="d5a6e-160">Le moteur de génération de modèles automatique a spécifié le chargement hâtif pour la propriété de navigation `Department`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-160">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="d5a6e-161">La méthode `Include` spécifie le chargement hâtif.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-161">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="d5a6e-162">Exécutez l’application et sélectionnez le lien **Courses**.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-162">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="d5a6e-163">La colonne Department affiche le `DepartmentID`, ce qui n’est pas utile.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-163">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="d5a6e-164">Mettez à jour la méthode `OnGetAsync` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-164">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="d5a6e-165">Le code précédent ajoute `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-165">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="d5a6e-166">`AsNoTracking` améliore les performances, car les entités retournées ne sont pas suivies.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-166">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="d5a6e-167">Les entités ne sont pas suivies, car elles ne sont pas mises à jour dans le contexte actuel.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-167">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="d5a6e-168">Mettez à jour *Pages/Courses/Index.cshtml* avec le balisage en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-168">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="d5a6e-169">Les modifications suivantes ont été apportées au code généré automatiquement :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-169">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="d5a6e-170">Changement de l’en-tête : Index a été remplacé par Course.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-170">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="d5a6e-171">Ajout d’une colonne **Number** qui affiche la valeur de la propriété `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-171">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="d5a6e-172">Par défaut, les clés primaires ne sont pas générées automatiquement, car elles ne sont normalement pas significatives pour les utilisateurs finaux.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-172">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="d5a6e-173">Toutefois, dans le cas présent la clé primaire est significative.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-173">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="d5a6e-174">Modification de la colonne **Department** afin d’afficher le nom du département.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-174">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="d5a6e-175">Le code affiche la propriété `Name` de l’entité `Department` qui est chargée dans la propriété de navigation `Department` :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-175">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="d5a6e-176">Exécutez l’application et sélectionnez l’onglet **Courses** pour afficher la liste avec les noms des départements.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-176">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Page d’index des cours](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="d5a6e-178">Chargement de données associées avec Select</span><span class="sxs-lookup"><span data-stu-id="d5a6e-178">Loading related data with Select</span></span>

<span data-ttu-id="d5a6e-179">La méthode `OnGetAsync` charge les données associées avec la méthode `Include` :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-179">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="d5a6e-180">L’opérateur `Select` charge uniquement les données associées nécessaires.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-180">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="d5a6e-181">Pour les éléments uniques, comme `Department.Name`, il utilise une jointure interne SQL.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-181">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="d5a6e-182">Pour les collections, il utilise un autre accès à la base de données, mais c’est aussi le cas de l’opérateur `Include` sur les collections.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-182">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="d5a6e-183">Le code suivant charge les données associées avec la méthode `Select` :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-183">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="d5a6e-184">Voici le `CourseViewModel` :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-184">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="d5a6e-185">Pour obtenir un exemple complet, consultez [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) et [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs).</span><span class="sxs-lookup"><span data-stu-id="d5a6e-185">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="d5a6e-186">Créer une page Instructors qui affiche les cours et les inscriptions</span><span class="sxs-lookup"><span data-stu-id="d5a6e-186">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="d5a6e-187">Dans cette section, nous allons créer la page Instructors.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-187">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="d5a6e-188">![Page d’index des formateurs](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="d5a6e-188">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="d5a6e-189">Cette page lit et affiche les données associées comme suit :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-189">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="d5a6e-190">La liste des formateurs affiche des données associées de l’entité `OfficeAssignment` (Office dans l’image précédente).</span><span class="sxs-lookup"><span data-stu-id="d5a6e-190">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="d5a6e-191">Il existe une relation un-à-zéro-ou-un entre les entités `Instructor` et `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-191">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="d5a6e-192">Le chargement hâtif est utilisé pour les entités `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-192">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="d5a6e-193">Le chargement hâtif est généralement plus efficace quand les données associées doivent être affichées.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-193">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="d5a6e-194">Ici, les affectations de bureau pour les formateurs sont affichées.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-194">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="d5a6e-195">Quand l’utilisateur sélectionne un formateur (Harui dans l’image précédente), les entités `Course` associées sont affichées.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-195">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="d5a6e-196">Il existe une relation plusieurs-à-plusieurs entre les entités `Instructor` et `Course`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-196">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="d5a6e-197">Le chargement hâtif est utilisé pour les entités `Course` et leurs entités `Department` associées.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-197">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="d5a6e-198">Dans le cas présent, des requêtes distinctes peuvent être plus efficaces, car seuls les cours du formateur sélectionné sont nécessaires.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-198">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="d5a6e-199">Cet exemple montre comment utiliser le chargement hâtif pour des propriétés de navigation dans des entités qui se trouvent dans des propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-199">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="d5a6e-200">Quand l’utilisateur sélectionne un cours (Chemistry dans l’image précédente), les données associées de l’entité `Enrollments` sont affichées.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-200">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="d5a6e-201">Dans l’image précédente, le nom et la note de l’étudiant sont affichés.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-201">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="d5a6e-202">Il existe une relation un-à-plusieurs entre les entités `Course` et `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-202">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="d5a6e-203">Créer un modèle d’affichage pour l’affichage d’index des formateurs</span><span class="sxs-lookup"><span data-stu-id="d5a6e-203">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="d5a6e-204">La page sur les formateurs affiche les données de trois tables différentes.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-204">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="d5a6e-205">Nous allons créer un modèle d’affichage qui comprend les trois entités représentant les trois tables.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-205">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="d5a6e-206">Dans le dossier *SchoolViewModels*, créez *InstructorIndexData.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-206">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="d5a6e-207">Générer automatiquement le modèle Instructor</span><span class="sxs-lookup"><span data-stu-id="d5a6e-207">Scaffold the Instructor model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d5a6e-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d5a6e-208">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="d5a6e-209">Suivez les instructions fournies dans [Générer automatiquement le modèle d’étudiant](xref:data/ef-rp/intro#scaffold-the-student-model) et utilisez `Instructor` pour la classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-209">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Instructor` for the model class.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d5a6e-210">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="d5a6e-210">.NET Core CLI</span></span>](#tab/netcore-cli)

 <span data-ttu-id="d5a6e-211">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-211">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

------

<span data-ttu-id="d5a6e-212">La commande précédente génère automatiquement le modèle `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-212">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="d5a6e-213">Exécutez l’application et accédez à la page des formateurs.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-213">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="d5a6e-214">Remplacez *Pages/Instructors/Index.cshtml.cs* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-214">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

<span data-ttu-id="d5a6e-215">La méthode `OnGetAsync` accepte des données de route facultatives pour l’ID du formateur sélectionné.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-215">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="d5a6e-216">Examinez la requête dans le fichier *Pages/Instructors/Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-216">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="d5a6e-217">La requête comporte deux Include :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-217">The query has two includes:</span></span>

* <span data-ttu-id="d5a6e-218">`OfficeAssignment` : affiché dans l’[affichage Instructors](#IP).</span><span class="sxs-lookup"><span data-stu-id="d5a6e-218">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="d5a6e-219">`CourseAssignments` : qui affiche les cours dispensés.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-219">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="d5a6e-220">Mettre à jour la page d’index des formateurs</span><span class="sxs-lookup"><span data-stu-id="d5a6e-220">Update the instructors Index page</span></span>

<span data-ttu-id="d5a6e-221">Mettez à jour *Pages/Instructors/Index.cshtml* avec le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-221">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="d5a6e-222">Le balisage précédent apporte les modifications suivantes :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-222">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="d5a6e-223">Il met à jour la directive `page` en remplaçant `@page` par `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-223">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="d5a6e-224">`"{id:int?}"` est un modèle de route.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-224">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="d5a6e-225">Le modèle de route change les chaînes de requête entières dans l’URL en données de route.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-225">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="d5a6e-226">Par exemple, si vous cliquez sur le lien **Select** pour un formateur avec seulement la directive `@page`, une URL comme celle-ci est générée :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-226">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="d5a6e-227">Quand la directive de page est `@page "{id:int?}"`, l’URL précédente est :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-227">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="d5a6e-228">Le titre de la page est **Instructors**.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-228">Page title is **Instructors**.</span></span>
* <span data-ttu-id="d5a6e-229">Il ajoute une colonne **Office** qui affiche `item.OfficeAssignment.Location` uniquement si `item.OfficeAssignment` n’est pas Null.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-229">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="d5a6e-230">Comme il s’agit d’une relation un-à-zéro-ou-un, il se peut qu’il n’y ait pas d’entité OfficeAssignment associée.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-230">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="d5a6e-231">Ajout d’une colonne **Courses** qui affiche les cours dispensés par chaque formateur.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-231">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="d5a6e-232">Consultez [Conversion de ligne explicite avec `@:`](xref:mvc/views/razor#explicit-line-transition-with-) pour en savoir plus sur cette syntaxe razor.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-232">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="d5a6e-233">Vous avez ajouté un code qui ajoute dynamiquement `class="success"` à l’élément `tr` du formateur sélectionné.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-233">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="d5a6e-234">Cela définit une couleur d’arrière-plan pour la ligne sélectionnée à l’aide d’une classe d’amorçage.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-234">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="d5a6e-235">Ajout d’un nouveau lien hypertexte libellé **Select**.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-235">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="d5a6e-236">Ce lien envoie l’ID du formateur sélectionné à la méthode `Index`, et définit une couleur d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-236">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="d5a6e-237">Exécutez l’application et sélectionnez le lien **Instructors**. La page affiche le `Location` (bureau) de l’entité `OfficeAssignment` associée.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-237">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="d5a6e-238">Si OfficeAssignment\` est Null, une cellule de table vide est affichée.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-238">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Page d’index des formateurs sans aucun élément sélectionné](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="d5a6e-240">Cliquez sur le lien **Select**.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-240">Click on the **Select** link.</span></span> <span data-ttu-id="d5a6e-241">Le style de ligne change.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-241">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="d5a6e-242">Ajouter les cours dispensés par le formateur sélectionné</span><span class="sxs-lookup"><span data-stu-id="d5a6e-242">Add courses taught by selected instructor</span></span>

<span data-ttu-id="d5a6e-243">Mettez à jour la méthode `OnGetAsync` dans *Pages/Instructors/Index.cshtml.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-243">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="d5a6e-244">Ajouter `public int CourseID { get; set; }`</span><span class="sxs-lookup"><span data-stu-id="d5a6e-244">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="d5a6e-245">Examinez la requête mise à jour :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-245">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="d5a6e-246">La requête précédente ajoute les entités `Department`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-246">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="d5a6e-247">Le code suivant s’exécute quand un formateur est sélectionné (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="d5a6e-247">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="d5a6e-248">Le formateur sélectionné est récupéré à partir de la liste des formateurs dans le modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-248">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="d5a6e-249">La propriété `Courses` du modèle d’affichage est chargée avec les entités `Course` de la propriété de navigation `CourseAssignments` de ce formateur.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-249">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="d5a6e-250">La méthode `Where` retourne une collection.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-250">The `Where` method returns a collection.</span></span> <span data-ttu-id="d5a6e-251">Dans la méthode `Where` précédente, une seule entité `Instructor` est retournée.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-251">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="d5a6e-252">La méthode `Single` convertit la collection en une seule entité `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-252">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="d5a6e-253">L’entité `Instructor` fournit l’accès à la propriété `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-253">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="d5a6e-254">`CourseAssignments` fournit l’accès aux entités `Course` associées.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-254">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Instructor-to-Courses m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="d5a6e-256">La méthode `Single` est utilisée sur une collection quand la collection ne compte qu’un seul élément.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-256">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="d5a6e-257">La méthode `Single` lève une exception si la collection est vide ou s’il y a plusieurs éléments.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-257">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="d5a6e-258">Une alternative est `SingleOrDefault`, qui retourne une valeur par défaut (Null dans le cas présent) si la collection est vide.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-258">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="d5a6e-259">L’utilisation de `SingleOrDefault` sur une collection vide :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-259">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="d5a6e-260">Génère une exception (à cause de la tentative de trouver une propriété `Courses` sur une référence Null).</span><span class="sxs-lookup"><span data-stu-id="d5a6e-260">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="d5a6e-261">Le message d’exception indique moins clairement la cause du problème.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-261">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="d5a6e-262">Le code suivant renseigne la propriété `Enrollments` du modèle d’affichage quand un cours est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-262">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="d5a6e-263">Ajoutez le balisage suivant à la fin de la page Razor *Pages/Instructors/Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-263">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="d5a6e-264">Le balisage précédent affiche une liste de cours associés à un formateur quand un formateur est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-264">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="d5a6e-265">Testez l’application.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-265">Test the app.</span></span> <span data-ttu-id="d5a6e-266">Cliquez sur un lien **Select** dans la page des formateurs.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-266">Click on a **Select** link on the instructors page.</span></span>

![Page d’index des formateurs avec un formateur sélectionné](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="d5a6e-268">Afficher les données sur les étudiants</span><span class="sxs-lookup"><span data-stu-id="d5a6e-268">Show student data</span></span>

<span data-ttu-id="d5a6e-269">Dans cette section, nous allons mettre à jour l’application afin d’afficher les données sur les étudiants pour le cours sélectionné.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-269">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="d5a6e-270">Mettez à jour la requête dans la méthode `OnGetAsync` dans *Pages/Instructors/Index.cshtml.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-270">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="d5a6e-271">Mettez à jour *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-271">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="d5a6e-272">Ajoutez le balisage suivant à la fin du fichier :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-272">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="d5a6e-273">Le balisage précédent affiche une liste des étudiants qui sont inscrits au cours sélectionné.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-273">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="d5a6e-274">Actualisez la page et sélectionnez un formateur.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-274">Refresh the page and select an instructor.</span></span> <span data-ttu-id="d5a6e-275">Sélectionnez un cours pour afficher la liste des étudiants inscrits et leurs notes.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-275">Select a course to see the list of enrolled students and their grades.</span></span>

![Page d’index des formateurs avec un formateur et un cours sélectionnés](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="d5a6e-277">Utilisation de Single</span><span class="sxs-lookup"><span data-stu-id="d5a6e-277">Using Single</span></span>

<span data-ttu-id="d5a6e-278">La méthode `Single` peut passer la condition `Where` au lieu d’appeler la méthode `Where` séparément :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-278">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="d5a6e-279">L’approche précédente faisant appel à `Single` ne présente aucun avantage par rapport à l’utilisation de `Where`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-279">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="d5a6e-280">Certains développeurs préfèrent le style d’approche `Single`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-280">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="d5a6e-281">Chargement explicite</span><span class="sxs-lookup"><span data-stu-id="d5a6e-281">Explicit loading</span></span>

<span data-ttu-id="d5a6e-282">Le code actuel spécifie le chargement hâtif pour `Enrollments` et `Students` :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-282">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="d5a6e-283">Supposez que les utilisateurs souhaitent rarement voir les inscriptions à un cours.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-283">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="d5a6e-284">Dans ce cas, une optimisation consisterait à charger les données d’inscription uniquement si elles sont demandées.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-284">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="d5a6e-285">Dans cette section, la méthode `OnGetAsync` est mise à jour de façon à utiliser le chargement explicite de `Enrollments` et `Students`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-285">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="d5a6e-286">Mettez à jour la méthode `OnGetAsync` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-286">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="d5a6e-287">Le code précédent supprime les appels de méthode *ThenInclude* pour les données sur les inscriptions et les étudiants.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-287">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="d5a6e-288">Si un cours est sélectionné, le code en surbrillance récupère :</span><span class="sxs-lookup"><span data-stu-id="d5a6e-288">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="d5a6e-289">Les entités `Enrollment` pour le cours sélectionné.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-289">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="d5a6e-290">Les entités `Student` pour chaque `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-290">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="d5a6e-291">Notez que le code précédent commente `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-291">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="d5a6e-292">Les propriétés de navigation peuvent être chargées explicitement uniquement pour les entités suivies.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-292">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="d5a6e-293">Testez l’application.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-293">Test the app.</span></span> <span data-ttu-id="d5a6e-294">Du point de vue des utilisateurs, l’application se comporte comme la version précédente.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-294">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="d5a6e-295">Le didacticiel suivant montre comment mettre à jour les données associées.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-295">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="d5a6e-296">[Précédent](xref:data/ef-rp/complex-data-model)
>[Suivant](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="d5a6e-296">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
