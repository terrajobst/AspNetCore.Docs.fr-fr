---
title: ASP.NET Core MVC avec EF Core - Lire les données associées - 6 sur 10
author: rick-anderson
description: Dans ce didacticiel, vous allez lire et afficher les données associées, à savoir les données qu’Entity Framework charge dans les propriétés de navigation.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: d5c9b665a80003ef5029754d7ad1780b3254e97e
ms.sourcegitcommit: c6ed2f00c7a08223d79090396b85793718b0dd69
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37092982"
---
# <a name="aspnet-core-mvc-with-ef-core---read-related-data---6-of-10"></a><span data-ttu-id="83080-103">ASP.NET Core MVC avec EF Core - Lire les données associées - 6 sur 10</span><span class="sxs-lookup"><span data-stu-id="83080-103">ASP.NET Core MVC with EF Core - Read Related Data - 6 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="83080-104">Par [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="83080-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="83080-105">L’exemple d’application web Contoso University montre comment créer des applications web ASP.NET Core MVC avec Entity Framework Core et Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="83080-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="83080-106">Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](intro.md).</span><span class="sxs-lookup"><span data-stu-id="83080-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="83080-107">Dans le didacticiel précédent, vous a élaboré le modèle de données School.</span><span class="sxs-lookup"><span data-stu-id="83080-107">In the previous tutorial, you completed the School data model.</span></span> <span data-ttu-id="83080-108">Dans ce didacticiel, vous allez lire et afficher les données associées, à savoir les données qu’Entity Framework charge dans les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="83080-108">In this tutorial, you'll read and display related data -- that is, data that the Entity Framework loads into navigation properties.</span></span>

<span data-ttu-id="83080-109">Les illustrations suivantes montrent les pages que vous allez utiliser.</span><span class="sxs-lookup"><span data-stu-id="83080-109">The following illustrations show the pages that you'll work with.</span></span>

![Page d’index des cours](read-related-data/_static/courses-index.png)

![Page d’index des formateurs](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="83080-112">Chargement hâtif, explicite et différé des données associées</span><span class="sxs-lookup"><span data-stu-id="83080-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="83080-113">Il existe plusieurs façons de permettre à un logiciel de mappage relationnel objet (ORM) comme Entity Framework de charger les données associées dans les propriétés de navigation d’une entité :</span><span class="sxs-lookup"><span data-stu-id="83080-113">There are several ways that Object-Relational Mapping (ORM) software such as Entity Framework can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="83080-114">Chargement hâtif.</span><span class="sxs-lookup"><span data-stu-id="83080-114">Eager loading.</span></span> <span data-ttu-id="83080-115">Quand l’entité est lue, ses données associées sont également récupérées.</span><span class="sxs-lookup"><span data-stu-id="83080-115">When the entity is read, related data is retrieved along with it.</span></span> <span data-ttu-id="83080-116">Cela génère en général une requête de jointure unique qui récupère toutes les données nécessaires.</span><span class="sxs-lookup"><span data-stu-id="83080-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="83080-117">Vous spécifiez un chargement hâtif dans Entity Framework Core à l’aide des méthodes `Include` et `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="83080-117">You specify eager loading in Entity Framework Core by using the `Include` and `ThenInclude` methods.</span></span>

  ![Exemple de chargement hâtif](read-related-data/_static/eager-loading.png)

  <span data-ttu-id="83080-119">Vous pouvez récupérer une partie des données dans des requêtes distinctes et EF « corrige » les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="83080-119">You can retrieve some of the data in separate queries, and EF "fixes up" the navigation properties.</span></span>  <span data-ttu-id="83080-120">Autrement dit, EF ajoute automatiquement les entités récupérées séparément là où elles doivent figurer dans les propriétés de navigation des entités précédemment récupérées.</span><span class="sxs-lookup"><span data-stu-id="83080-120">That is, EF automatically adds the separately retrieved entities where they belong in navigation properties of previously retrieved entities.</span></span> <span data-ttu-id="83080-121">Pour la requête qui récupère les données associées, vous pouvez utiliser la méthode `Load` à la place d’une méthode renvoyant une liste ou un objet, telle que `ToList` ou `Single`.</span><span class="sxs-lookup"><span data-stu-id="83080-121">For the query that retrieves related data, you can use the `Load` method instead of a method that returns a list or object, such as `ToList` or `Single`.</span></span>

  ![Exemple de requêtes distinctes](read-related-data/_static/separate-queries.png)

* <span data-ttu-id="83080-123">Chargement explicite.</span><span class="sxs-lookup"><span data-stu-id="83080-123">Explicit loading.</span></span> <span data-ttu-id="83080-124">Quand l’entité est lue pour la première fois, les données associées ne sont pas récupérées.</span><span class="sxs-lookup"><span data-stu-id="83080-124">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="83080-125">Vous écrivez un code qui récupère les données associées si elles sont nécessaires.</span><span class="sxs-lookup"><span data-stu-id="83080-125">You write code that retrieves the related data if it's needed.</span></span> <span data-ttu-id="83080-126">Comme dans le cas du chargement hâtif avec des requêtes distinctes, le chargement explicite génère plusieurs requêtes envoyées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="83080-126">As in the case of eager loading with separate queries, explicit loading results in multiple queries sent to the database.</span></span> <span data-ttu-id="83080-127">La différence tient au fait qu’avec le chargement explicite, le code spécifie les propriétés de navigation à charger.</span><span class="sxs-lookup"><span data-stu-id="83080-127">The difference is that with explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="83080-128">Dans Entity Framework Core 1.1, vous pouvez utiliser la méthode `Load` pour effectuer le chargement explicite.</span><span class="sxs-lookup"><span data-stu-id="83080-128">In Entity Framework Core 1.1 you can use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="83080-129">Exemple :</span><span class="sxs-lookup"><span data-stu-id="83080-129">For example:</span></span>

  ![Exemple de chargement explicite](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="83080-131">Chargement différé.</span><span class="sxs-lookup"><span data-stu-id="83080-131">Lazy loading.</span></span> <span data-ttu-id="83080-132">Quand l’entité est lue pour la première fois, les données associées ne sont pas récupérées.</span><span class="sxs-lookup"><span data-stu-id="83080-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="83080-133">Toutefois, la première fois que vous essayez d’accéder à une propriété de navigation, les données requises pour cette propriété de navigation sont récupérées automatiquement.</span><span class="sxs-lookup"><span data-stu-id="83080-133">However, the first time you attempt to access a navigation property, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="83080-134">Une requête est envoyée à la base de données chaque fois que vous essayez d’obtenir des données à partir d’une propriété de navigation pour la première fois.</span><span class="sxs-lookup"><span data-stu-id="83080-134">A query is sent to the database each time you try to get data from a navigation property for the first time.</span></span> <span data-ttu-id="83080-135">Entity Framework Core 1.0 ne prend pas en charge le chargement différé.</span><span class="sxs-lookup"><span data-stu-id="83080-135">Entity Framework Core 1.0 doesn't support lazy loading.</span></span>

### <a name="performance-considerations"></a><span data-ttu-id="83080-136">Considérations sur les performances</span><span class="sxs-lookup"><span data-stu-id="83080-136">Performance considerations</span></span>

<span data-ttu-id="83080-137">Si vous savez que vous avez besoin des données associées pour toutes les entités récupérées, le chargement hâtif souvent offre des performances optimales, car une seule requête envoyée à la base de données est généralement plus efficace que les requêtes distinctes pour chaque entité récupérée.</span><span class="sxs-lookup"><span data-stu-id="83080-137">If you know you need related data for every entity retrieved, eager loading often offers the best performance, because a single query sent to the database is typically more efficient than separate queries for each entity retrieved.</span></span> <span data-ttu-id="83080-138">Par exemple, supposons que chaque département a dix cours associés.</span><span class="sxs-lookup"><span data-stu-id="83080-138">For example, suppose that each department has ten related courses.</span></span> <span data-ttu-id="83080-139">Le chargement hâtif de toutes les données associées générerait une seule requête (de jointure) et un seul aller-retour à la base de données.</span><span class="sxs-lookup"><span data-stu-id="83080-139">Eager loading of all related data would result in just a single (join) query and a single round trip to the database.</span></span> <span data-ttu-id="83080-140">Une requête distincte pour les cours pour chaque département entraînerait onze allers-retours à la base de données.</span><span class="sxs-lookup"><span data-stu-id="83080-140">A separate query for courses for each department would result in eleven round trips to the database.</span></span> <span data-ttu-id="83080-141">Les allers-retours supplémentaires à la base de données sont particulièrement nuisibles pour les performances lorsque la latence est élevée.</span><span class="sxs-lookup"><span data-stu-id="83080-141">The extra round trips to the database are especially detrimental to performance when latency is high.</span></span>

<span data-ttu-id="83080-142">En revanche, dans certains scénarios, les requêtes distinctes s’avèrent plus efficaces.</span><span class="sxs-lookup"><span data-stu-id="83080-142">On the other hand, in some scenarios separate queries is more efficient.</span></span> <span data-ttu-id="83080-143">Le chargement hâtif de toutes les données associées dans une seule requête peut entraîner une jointure très complexe à générer, que SQL Server ne peut pas traiter efficacement.</span><span class="sxs-lookup"><span data-stu-id="83080-143">Eager loading of all related data in one query might cause a very complex join to be generated, which SQL Server can't process efficiently.</span></span> <span data-ttu-id="83080-144">Ou, si vous avez besoin d’accéder aux propriétés de navigation d’entité uniquement pour un sous-ensemble des entités que vous traitez, des requêtes distinctes peuvent être plus performantes, car le chargement hâtif de tous les éléments en amont entraînerait la récupération de plus de données qu’il vous faut.</span><span class="sxs-lookup"><span data-stu-id="83080-144">Or if you need to access an entity's navigation properties only for a subset of a set of the entities you're processing, separate queries might perform better because eager loading of everything up front would retrieve more data than you need.</span></span> <span data-ttu-id="83080-145">Si les performances sont essentielles, il est préférable de tester les performances des deux façons afin d’effectuer le meilleur choix.</span><span class="sxs-lookup"><span data-stu-id="83080-145">If performance is critical, it's best to test performance both ways in order to make the best choice.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="83080-146">Créer une page Courses qui affiche le nom du service</span><span class="sxs-lookup"><span data-stu-id="83080-146">Create a Courses page that displays Department name</span></span>

<span data-ttu-id="83080-147">L’entité Course inclut une propriété de navigation qui contient l’entité Department du service auquel le cours est affecté.</span><span class="sxs-lookup"><span data-stu-id="83080-147">The Course entity includes a navigation property that contains the Department entity of the department that the course is assigned to.</span></span> <span data-ttu-id="83080-148">Pour afficher le nom du service affecté dans une liste de cours, vous devez obtenir la propriété Name de l’entité Department qui figure dans la propriété de navigation `Course.Department`.</span><span class="sxs-lookup"><span data-stu-id="83080-148">To display the name of the assigned department in a list of courses, you need to get the Name property from the Department entity that's in the `Course.Department` navigation property.</span></span>

<span data-ttu-id="83080-149">Créez un contrôleur nommé CoursesController pour le type d’entité Course, en utilisant les mêmes options pour le générateur de modèles automatique **Contrôleur MVC avec vues, utilisant Entity Framework** que vous avez utilisées précédemment pour le contrôleur Students, comme indiqué dans l’illustration suivante :</span><span class="sxs-lookup"><span data-stu-id="83080-149">Create a controller named CoursesController for the Course entity type, using the same options for the **MVC Controller with views, using Entity Framework** scaffolder that you did earlier for the Students controller, as shown in the following illustration:</span></span>

![Ajouter un contrôleur Courses](read-related-data/_static/add-courses-controller.png)

<span data-ttu-id="83080-151">Ouvrez *CoursesController.cs* et examinez la méthode `Index`.</span><span class="sxs-lookup"><span data-stu-id="83080-151">Open *CoursesController.cs* and examine the `Index` method.</span></span> <span data-ttu-id="83080-152">La génération de modèles automatique a spécifié un chargement hâtif pour la propriété de navigation `Department` à l’aide de la méthode `Include`.</span><span class="sxs-lookup"><span data-stu-id="83080-152">The automatic scaffolding has specified eager loading for the `Department` navigation property by using the `Include` method.</span></span>

<span data-ttu-id="83080-153">Remplacez la méthode `Index` par le code suivant qui utilise un nom plus approprié pour `IQueryable` qui renvoie les entités Course (`courses` à la place de `schoolContext`) :</span><span class="sxs-lookup"><span data-stu-id="83080-153">Replace the `Index` method with the following code that uses a more appropriate name for the `IQueryable` that returns Course entities (`courses` instead of `schoolContext`):</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="83080-154">Ouvrez *Views/Courses/Index.cshtml* et remplacez le code de modèle par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="83080-154">Open *Views/Courses/Index.cshtml* and replace the template code with the following code.</span></span> <span data-ttu-id="83080-155">Les modifications apparaissent en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="83080-155">The changes are highlighted:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="83080-156">Vous avez apporté les modifications suivantes au code généré automatiquement :</span><span class="sxs-lookup"><span data-stu-id="83080-156">You've made the following changes to the scaffolded code:</span></span>

* <span data-ttu-id="83080-157">Changement de l’en-tête : Index a été remplacé par Courses.</span><span class="sxs-lookup"><span data-stu-id="83080-157">Changed the heading from Index to Courses.</span></span>

* <span data-ttu-id="83080-158">Ajout d’une colonne **Number** qui affiche la valeur de la propriété `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="83080-158">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="83080-159">Par défaut, les clés primaires ne sont pas générées automatiquement, car elles ne sont normalement pas significatives pour les utilisateurs finaux.</span><span class="sxs-lookup"><span data-stu-id="83080-159">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="83080-160">Toutefois, dans le cas présent, la clé primaire est significative et vous voulez l’afficher.</span><span class="sxs-lookup"><span data-stu-id="83080-160">However, in this case the primary key is meaningful and you want to show it.</span></span>

* <span data-ttu-id="83080-161">Modification de la colonne **Department** afin d’afficher le nom du département.</span><span class="sxs-lookup"><span data-stu-id="83080-161">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="83080-162">Le code affiche la propriété `Name` de l’entité Department qui est chargée dans la propriété de navigation `Department` :</span><span class="sxs-lookup"><span data-stu-id="83080-162">The code displays the `Name` property of the Department entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="83080-163">Exécutez l’application et sélectionnez l’onglet **Courses** pour afficher la liste avec les noms des départements.</span><span class="sxs-lookup"><span data-stu-id="83080-163">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Page d’index des cours](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="83080-165">Créer une page Instructors qui affiche les cours et les inscriptions</span><span class="sxs-lookup"><span data-stu-id="83080-165">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="83080-166">Dans cette section, vous allez créer un contrôleur et une vue pour l’entité Instructor afin d’afficher la page Instructors :</span><span class="sxs-lookup"><span data-stu-id="83080-166">In this section, you'll create a controller and view for the Instructor entity in order to display the Instructors page:</span></span>

![Page d’index des formateurs](read-related-data/_static/instructors-index.png)

<span data-ttu-id="83080-168">Cette page lit et affiche les données associées comme suit :</span><span class="sxs-lookup"><span data-stu-id="83080-168">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="83080-169">La liste des formateurs affiche les données associées de l’entité OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="83080-169">The list of instructors displays related data from the OfficeAssignment entity.</span></span> <span data-ttu-id="83080-170">Il existe une relation un-à-zéro-ou-un entre les entités Instructor et OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="83080-170">The Instructor and OfficeAssignment entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="83080-171">Vous allez utiliser un chargement hâtif pour les entités OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="83080-171">You'll use eager loading for the OfficeAssignment entities.</span></span> <span data-ttu-id="83080-172">Comme expliqué précédemment, le chargement hâtif est généralement plus efficace lorsque vous avez besoin des données associées pour toutes les lignes extraites de la table primaire.</span><span class="sxs-lookup"><span data-stu-id="83080-172">As explained earlier, eager loading is typically more efficient when you need the related data for all retrieved rows of the primary table.</span></span> <span data-ttu-id="83080-173">Dans ce cas, vous souhaitez afficher les affectations de bureaux pour tous les formateurs affichés.</span><span class="sxs-lookup"><span data-stu-id="83080-173">In this case, you want to display office assignments for all displayed instructors.</span></span>

* <span data-ttu-id="83080-174">Lorsque l’utilisateur sélectionne un formateur, les entités Course associées sont affichées.</span><span class="sxs-lookup"><span data-stu-id="83080-174">When the user selects an instructor, related Course entities are displayed.</span></span> <span data-ttu-id="83080-175">Il existe une relation plusieurs à plusieurs entre les entités Instructor et Course.</span><span class="sxs-lookup"><span data-stu-id="83080-175">The Instructor and Course entities are in a many-to-many relationship.</span></span> <span data-ttu-id="83080-176">Vous allez utiliser un chargement hâtif pour les entités Course et les entités Department qui leur sont associées.</span><span class="sxs-lookup"><span data-stu-id="83080-176">You'll use eager loading for the Course entities and their related Department entities.</span></span> <span data-ttu-id="83080-177">Dans ce cas, des requêtes distinctes peuvent être plus efficaces, car vous avez besoin de cours uniquement pour le formateur sélectionné.</span><span class="sxs-lookup"><span data-stu-id="83080-177">In this case, separate queries might be more efficient because you need courses only for the selected instructor.</span></span> <span data-ttu-id="83080-178">Toutefois, cet exemple montre comment utiliser le chargement hâtif pour des propriétés de navigation dans des entités qui se trouvent elles-mêmes dans des propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="83080-178">However, this example shows how to use eager loading for navigation properties within entities that are themselves in navigation properties.</span></span>

* <span data-ttu-id="83080-179">Lorsque l’utilisateur sélectionne un cours, les données associées dans le jeu d’entités Enrollment s’affichent.</span><span class="sxs-lookup"><span data-stu-id="83080-179">When the user selects a course, related data from the Enrollments entity set is displayed.</span></span> <span data-ttu-id="83080-180">Il existe une relation un-à-plusieurs entre les entités Course et Enrollment.</span><span class="sxs-lookup"><span data-stu-id="83080-180">The Course and Enrollment entities are in a one-to-many relationship.</span></span> <span data-ttu-id="83080-181">Vous allez utiliser des requêtes distinctes pour les entités Enrollment et les entités Student qui leur sont associées.</span><span class="sxs-lookup"><span data-stu-id="83080-181">You'll use separate queries for Enrollment entities and their related Student entities.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="83080-182">Créer un modèle de vue pour la vue d’index des formateurs</span><span class="sxs-lookup"><span data-stu-id="83080-182">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="83080-183">La page Instructors affiche des données de trois tables différentes.</span><span class="sxs-lookup"><span data-stu-id="83080-183">The Instructors page shows data from three different tables.</span></span> <span data-ttu-id="83080-184">Par conséquent, vous allez créer un modèle de vue qui comprend trois propriétés, chacune contenant les données d’une des tables.</span><span class="sxs-lookup"><span data-stu-id="83080-184">Therefore, you'll create a view model that includes three properties, each holding the data for one of the tables.</span></span>

<span data-ttu-id="83080-185">Dans le dossier *SchoolViewModels*, créez *InstructorIndexData.cs* et remplacez le code existant par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="83080-185">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a><span data-ttu-id="83080-186">Créer les vues et le contrôleur de formateurs</span><span class="sxs-lookup"><span data-stu-id="83080-186">Create the Instructor controller and views</span></span>

<span data-ttu-id="83080-187">Créez un contrôleur de formateurs avec des actions de lecture/écriture EF comme indiqué dans l’illustration suivante :</span><span class="sxs-lookup"><span data-stu-id="83080-187">Create an Instructors controller with EF read/write actions as shown in the following illustration:</span></span>

![Ajouter le contrôleur de formateurs](read-related-data/_static/add-instructors-controller.png)

<span data-ttu-id="83080-189">Ouvrez *InstructorsController.cs* et ajoutez une instruction using pour l’espace de noms ViewModel :</span><span class="sxs-lookup"><span data-stu-id="83080-189">Open *InstructorsController.cs* and add a using statement for the ViewModels namespace:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

<span data-ttu-id="83080-190">Remplacez la méthode Index par le code suivant pour effectuer un chargement hâtif des données associées et le placer dans le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="83080-190">Replace the Index method with the following code to do eager loading of related data and put it in the view model.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

<span data-ttu-id="83080-191">La méthode accepte des données de route facultatives (`id`) et un paramètre de chaîne de requête (`courseID`) qui fournissent les valeurs d’ID du formateur sélectionné et du cours sélectionné.</span><span class="sxs-lookup"><span data-stu-id="83080-191">The method accepts optional route data (`id`) and a query string parameter (`courseID`) that provide the ID values of the selected instructor and selected course.</span></span> <span data-ttu-id="83080-192">Ces paramètres sont fournis par les liens hypertexte **Select** dans la page.</span><span class="sxs-lookup"><span data-stu-id="83080-192">The parameters are provided by the **Select** hyperlinks on the page.</span></span>

<span data-ttu-id="83080-193">Le code commence par créer une instance du modèle de vue et la placer dans la liste des formateurs.</span><span class="sxs-lookup"><span data-stu-id="83080-193">The code begins by creating an instance of the view model and putting in it the list of instructors.</span></span> <span data-ttu-id="83080-194">Le code spécifie un chargement hâtif pour les propriétés de navigation `Instructor.OfficeAssignment` et `Instructor.CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="83080-194">The code specifies eager loading for the `Instructor.OfficeAssignment` and the `Instructor.CourseAssignments` navigation properties.</span></span> <span data-ttu-id="83080-195">Dans la propriété `CourseAssignments`, la propriété `Course` est chargée et, dans ce cadre, les propriétés `Enrollments` et `Department` sont chargées, et dans chaque entité `Enrollment`, la propriété `Student` est chargée.</span><span class="sxs-lookup"><span data-stu-id="83080-195">Within the `CourseAssignments` property, the `Course` property is loaded, and within that, the `Enrollments` and `Department` properties are loaded, and within each `Enrollment` entity the `Student` property is loaded.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

<span data-ttu-id="83080-196">Étant donné que la vue nécessite toujours l’entité OfficeAssignment, il est plus efficace de l’extraire dans la même requête.</span><span class="sxs-lookup"><span data-stu-id="83080-196">Since the view always requires the OfficeAssignment entity, it's more efficient to fetch that in the same query.</span></span> <span data-ttu-id="83080-197">Les entités Course sont requises lorsqu’un formateur est sélectionné dans la page web, de sorte qu’une requête individuelle est meilleure que plusieurs requêtes seulement si la page s’affiche plus souvent avec un cours sélectionné que sans.</span><span class="sxs-lookup"><span data-stu-id="83080-197">Course entities are required when an instructor is selected in the web page, so a single query is better than multiple queries only if the page is displayed more often with a course selected than without.</span></span>

<span data-ttu-id="83080-198">Le code répète `CourseAssignments` et `Course`, car vous avez besoin de deux propriétés de `Course`.</span><span class="sxs-lookup"><span data-stu-id="83080-198">The code repeats `CourseAssignments` and `Course` because you need two properties from `Course`.</span></span> <span data-ttu-id="83080-199">La première chaîne d’appels `ThenInclude` obtient `CourseAssignment.Course`, `Course.Enrollments` et `Enrollment.Student`.</span><span class="sxs-lookup"><span data-stu-id="83080-199">The first string of `ThenInclude` calls gets `CourseAssignment.Course`, `Course.Enrollments`, and `Enrollment.Student`.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

<span data-ttu-id="83080-200">À ce stade dans le code, un autre `ThenInclude` serait pour les propriétés de navigation de `Student`, dont vous n’avez pas besoin.</span><span class="sxs-lookup"><span data-stu-id="83080-200">At that point in the code, another `ThenInclude` would be for navigation properties of `Student`, which you don't need.</span></span> <span data-ttu-id="83080-201">Toutefois, l’appel de `Include` recommence avec les propriétés `Instructor`, donc vous devez parcourir la chaîne à nouveau, cette fois en spécifiant `Course.Department` à la place de `Course.Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="83080-201">But calling `Include` starts over with `Instructor` properties, so you have to go through the chain again, this time specifying `Course.Department` instead of `Course.Enrollments`.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

<span data-ttu-id="83080-202">Le code suivant s’exécute quand un formateur a été sélectionné.</span><span class="sxs-lookup"><span data-stu-id="83080-202">The following code executes when an instructor was selected.</span></span> <span data-ttu-id="83080-203">Le formateur sélectionné est récupéré à partir de la liste des formateurs dans le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="83080-203">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="83080-204">La propriété `Courses` du modèle de vue est alors chargée avec les entités Course de la propriété de navigation `CourseAssignments` de ce formateur.</span><span class="sxs-lookup"><span data-stu-id="83080-204">The view model's `Courses` property is then loaded with the Course entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

<span data-ttu-id="83080-205">La méthode `Where` renvoie une collection, mais dans ce cas, les critères transmis à cette méthode entraînent le renvoi d’une seule entité Instructor.</span><span class="sxs-lookup"><span data-stu-id="83080-205">The `Where` method returns a collection, but in this case the criteria passed to that method result in only a single Instructor entity being returned.</span></span> <span data-ttu-id="83080-206">La méthode `Single` convertit la collection en une seule entité Instructor, ce qui vous permet d’accéder à la propriété `CourseAssignments` de cette entité.</span><span class="sxs-lookup"><span data-stu-id="83080-206">The `Single` method converts the collection into a single Instructor entity, which gives you access to that entity's `CourseAssignments` property.</span></span> <span data-ttu-id="83080-207">La propriété `CourseAssignments` contient des entités `CourseAssignment`, à partir desquelles vous souhaitez uniquement les entités `Course` associées.</span><span class="sxs-lookup"><span data-stu-id="83080-207">The `CourseAssignments` property contains `CourseAssignment` entities, from which you want only the related `Course` entities.</span></span>

<span data-ttu-id="83080-208">Vous utilisez la méthode `Single` sur une collection lorsque vous savez que la collection aura un seul élément.</span><span class="sxs-lookup"><span data-stu-id="83080-208">You use the `Single` method on a collection when you know the collection will have only one item.</span></span> <span data-ttu-id="83080-209">La méthode Single lève une exception si la collection transmise est vide ou s’il y a plusieurs éléments.</span><span class="sxs-lookup"><span data-stu-id="83080-209">The Single method throws an exception if the collection passed to it's empty or if there's more than one item.</span></span> <span data-ttu-id="83080-210">Une alternative est `SingleOrDefault`, qui renvoie une valeur par défaut (Null dans ce cas) si la collection est vide.</span><span class="sxs-lookup"><span data-stu-id="83080-210">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="83080-211">Toutefois, dans ce cas, cela entraînerait encore une exception (en tentant de trouver une propriété `Courses` sur une référence null) et le message d’exception indiquerait moins clairement la cause du problème.</span><span class="sxs-lookup"><span data-stu-id="83080-211">However, in this case that would still result in an exception (from trying to find a `Courses` property on a null reference), and the exception message would less clearly indicate the cause of the problem.</span></span> <span data-ttu-id="83080-212">Lorsque vous appelez la méthode `Single`, vous pouvez également transmettre la condition Where au lieu d’appeler séparément la méthode `Where` :</span><span class="sxs-lookup"><span data-stu-id="83080-212">When you call the `Single` method, you can also pass in the Where condition instead of calling the `Where` method separately:</span></span>

```csharp
.Single(i => i.ID == id.Value)
```

<span data-ttu-id="83080-213">À la place de :</span><span class="sxs-lookup"><span data-stu-id="83080-213">Instead of:</span></span>

```csharp
.Where(I => i.ID == id.Value).Single()
```

<span data-ttu-id="83080-214">Ensuite, si un cours a été sélectionné, le cours sélectionné est récupéré à partir de la liste des cours dans le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="83080-214">Next, if a course was selected, the selected course is retrieved from the list of courses in the view model.</span></span> <span data-ttu-id="83080-215">Ensuite, la propriété `Enrollments` du modèle de vue est chargée avec les entités Enrollment à partir de la propriété de navigation `Enrollments` de ce cours.</span><span class="sxs-lookup"><span data-stu-id="83080-215">Then the view model's `Enrollments` property is loaded with the Enrollment entities from that course's `Enrollments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a><span data-ttu-id="83080-216">Modifier la vue d’index des formateurs</span><span class="sxs-lookup"><span data-stu-id="83080-216">Modify the Instructor Index view</span></span>

<span data-ttu-id="83080-217">Dans *Views/Instructors/Index.cshtml*, remplacez le code du modèle par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="83080-217">In *Views/Instructors/Index.cshtml*, replace the template code with the following code.</span></span> <span data-ttu-id="83080-218">Les modifications apparaissent en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="83080-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

<span data-ttu-id="83080-219">Vous avez apporté les modifications suivantes au code existant :</span><span class="sxs-lookup"><span data-stu-id="83080-219">You've made the following changes to the existing code:</span></span>

* <span data-ttu-id="83080-220">Vous avez changé la classe de modèle en `InstructorIndexData`.</span><span class="sxs-lookup"><span data-stu-id="83080-220">Changed the model class to `InstructorIndexData`.</span></span>

* <span data-ttu-id="83080-221">Vous avez changé le titre de la page en remplaçant **Index** par **Instructors**.</span><span class="sxs-lookup"><span data-stu-id="83080-221">Changed the page title from **Index** to **Instructors**.</span></span>

* <span data-ttu-id="83080-222">Vous avez ajouté une colonne **Office** qui affiche `item.OfficeAssignment.Location` seulement si `item.OfficeAssignment` n’est pas Null.</span><span class="sxs-lookup"><span data-stu-id="83080-222">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="83080-223">(Comme il s’agit d’une relation un-à-zéro-ou-un, il se peut qu’il n’y ait pas d’entité OfficeAssignment associée.)</span><span class="sxs-lookup"><span data-stu-id="83080-223">(Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.)</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="83080-224">Vous avez ajouté une colonne **Courses** qui affiche les cours animés par chaque formateur.</span><span class="sxs-lookup"><span data-stu-id="83080-224">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="83080-225">Consultez [Conversion de ligne explicite avec `@:`](xref:mvc/views/razor#explicit-line-transition-with-) pour en savoir plus sur cette syntaxe razor.</span><span class="sxs-lookup"><span data-stu-id="83080-225">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="83080-226">Vous avez ajouté un code qui ajoute dynamiquement `class="success"` à l’élément `tr` du formateur sélectionné.</span><span class="sxs-lookup"><span data-stu-id="83080-226">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="83080-227">Cela définit une couleur d’arrière-plan pour la ligne sélectionnée à l’aide d’une classe d’amorçage.</span><span class="sxs-lookup"><span data-stu-id="83080-227">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="83080-228">Vous avez ajouté un nouveau lien hypertexte étiqueté **Select** immédiatement avant les autres liens dans chaque ligne, ce qui entraîne l’envoi de l’ID du formateur sélectionné à la méthode `Index`.</span><span class="sxs-lookup"><span data-stu-id="83080-228">Added a new hyperlink labeled **Select** immediately before the other links in each row, which causes the selected instructor's ID to be sent to the `Index` method.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="83080-229">Exécutez l’application et sélectionnez l’onglet **Instructors**. La page affiche la propriété Location des entités OfficeAssignment associées et une cellule de table vide lorsqu’il n’existe aucune entité OfficeAssignment associée.</span><span class="sxs-lookup"><span data-stu-id="83080-229">Run the app and select the **Instructors** tab. The page displays the Location property of related OfficeAssignment entities and an empty table cell when there's no related OfficeAssignment entity.</span></span>

![Page d’index des formateurs sans aucun élément sélectionné](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="83080-231">Dans le fichier *Views/Instructors/Index.cshtml*, après l’élément de fermeture de table (à la fin du fichier), ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="83080-231">In the *Views/Instructors/Index.cshtml* file, after the closing table element (at the end of the file), add the following code.</span></span> <span data-ttu-id="83080-232">Ce code affiche la liste des cours associés à un formateur quand un formateur est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="83080-232">This code displays a list of courses related to an instructor when an instructor is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

<span data-ttu-id="83080-233">Ce code lit la propriété `Courses` du modèle de vue pour afficher la liste des cours.</span><span class="sxs-lookup"><span data-stu-id="83080-233">This code reads the `Courses` property of the view model to display a list of courses.</span></span> <span data-ttu-id="83080-234">Il fournit également un lien hypertexte **Select** qui envoie l’ID du cours sélectionné à la méthode d’action `Index`.</span><span class="sxs-lookup"><span data-stu-id="83080-234">It also provides a **Select** hyperlink that sends the ID of the selected course to the `Index` action method.</span></span>

<span data-ttu-id="83080-235">Actualisez la page et sélectionnez un formateur.</span><span class="sxs-lookup"><span data-stu-id="83080-235">Refresh the page and select an instructor.</span></span> <span data-ttu-id="83080-236">Vous voyez à présent une grille qui affiche les cours affectés au formateur sélectionné et, pour chaque cours, vous voyez le nom du département affecté.</span><span class="sxs-lookup"><span data-stu-id="83080-236">Now you see a grid that displays courses assigned to the selected instructor, and for each course you see the name of the assigned department.</span></span>

![Page d’index des formateurs avec un formateur sélectionné](read-related-data/_static/instructors-index-instructor-selected.png)

<span data-ttu-id="83080-238">Après le bloc de code que vous venez d’ajouter, ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="83080-238">After the code block you just added, add the following code.</span></span> <span data-ttu-id="83080-239">Ceci affiche la liste des étudiants qui sont inscrits à un cours quand ce cours est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="83080-239">This displays a list of the students who are enrolled in a course when that course is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

<span data-ttu-id="83080-240">Ce code lit la propriété Enrollments du modèle de vue pour afficher la liste des étudiants inscrits dans ce cours.</span><span class="sxs-lookup"><span data-stu-id="83080-240">This code reads the Enrollments property of the view model in order to display a list of students enrolled in the course.</span></span>

<span data-ttu-id="83080-241">Actualisez la page à nouveau et sélectionnez un formateur.</span><span class="sxs-lookup"><span data-stu-id="83080-241">Refresh the page again and select an instructor.</span></span> <span data-ttu-id="83080-242">Ensuite, sélectionnez un cours pour afficher la liste des étudiants inscrits et leurs notes.</span><span class="sxs-lookup"><span data-stu-id="83080-242">Then select a course to see the list of enrolled students and their grades.</span></span>

![Page d’index des formateurs avec un formateur et un cours sélectionnés](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a><span data-ttu-id="83080-244">Chargement explicite</span><span class="sxs-lookup"><span data-stu-id="83080-244">Explicit loading</span></span>

<span data-ttu-id="83080-245">Lorsque vous avez récupéré la liste des formateurs dans *InstructorsController.cs*, vous avez spécifié un chargement hâtif pour la propriété de navigation `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="83080-245">When you retrieved the list of instructors in *InstructorsController.cs*, you specified eager loading for the `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="83080-246">Supposons que vous vous attendiez à ce que les utilisateurs ne souhaitent que rarement voir les inscriptions pour un formateur et un cours sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="83080-246">Suppose you expected users to only rarely want to see enrollments in a selected instructor and course.</span></span> <span data-ttu-id="83080-247">Dans ce cas, vous pouvez charger les données d’inscription uniquement si elles sont demandées.</span><span class="sxs-lookup"><span data-stu-id="83080-247">In that case, you might want to load the enrollment data only if it's requested.</span></span> <span data-ttu-id="83080-248">Pour voir un exemple illustrant comment effectuer un chargement explicite, remplacez la méthode `Index` par le code suivant, qui supprime le chargement hâtif pour Enrollments et charge explicitement cette propriété.</span><span class="sxs-lookup"><span data-stu-id="83080-248">To see an example of how to do explicit loading, replace the `Index` method with the following code, which removes eager loading for Enrollments and loads that property explicitly.</span></span> <span data-ttu-id="83080-249">Les modifications du code apparaissent en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="83080-249">The code changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

<span data-ttu-id="83080-250">Le nouveau code supprime les appels de la méthode *ThenInclude* pour les données d’inscription à partir du code qui extrait les entités de formateur.</span><span class="sxs-lookup"><span data-stu-id="83080-250">The new code drops the *ThenInclude* method calls for enrollment data from the code that retrieves instructor entities.</span></span> <span data-ttu-id="83080-251">Si un formateur et un cours sont sélectionnés, le code en surbrillance récupère les entités Enrollment pour le cours sélectionné et les entités Student pour chaque entité Enrollment.</span><span class="sxs-lookup"><span data-stu-id="83080-251">If an instructor and course are selected, the highlighted code retrieves Enrollment entities for the selected course, and Student entities for each Enrollment.</span></span>

<span data-ttu-id="83080-252">Exécutez l’application, accédez à la page d’index des formateurs et vous ne verrez aucune différence pour ce qui est affiché dans la page, bien que vous ayez modifié la façon dont les données sont récupérées.</span><span class="sxs-lookup"><span data-stu-id="83080-252">Run the app, go to the Instructors Index page now and you'll see no difference in what's displayed on the page, although you've changed how the data is retrieved.</span></span>

## <a name="summary"></a><span data-ttu-id="83080-253">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="83080-253">Summary</span></span>

<span data-ttu-id="83080-254">Vous avez maintenant utilisé le chargement hâtif avec une seule requête et avec plusieurs requêtes pour lire les données associées dans les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="83080-254">You've now used eager loading with one query and with multiple queries to read related data into navigation properties.</span></span> <span data-ttu-id="83080-255">Dans le didacticiel suivant, vous apprendrez à mettre à jour les données associées.</span><span class="sxs-lookup"><span data-stu-id="83080-255">In the next tutorial you'll learn how to update related data.</span></span>

::: moniker-end

>[!div class="step-by-step"]
><span data-ttu-id="83080-256">[Précédent](complex-data-model.md)
>[Suivant](update-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="83080-256">[Previous](complex-data-model.md)
[Next](update-related-data.md)</span></span>
