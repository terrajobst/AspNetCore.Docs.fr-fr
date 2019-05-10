---
title: Pages Razor avec EF Core dans ASP.NET Core - Mise à jour de données associées - 7 sur 8
author: rick-anderson
description: Dans ce didacticiel, vous allez mettre à jour des données associées en mettant à jour des propriétés de navigation et des champs de clé étrangère.
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/update-related-data
ms.openlocfilehash: b996e7f9c8e422060f6378a6004a53906e2af7c7
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087094"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---update-related-data---7-of-8"></a><span data-ttu-id="bc928-103">Pages Razor avec EF Core dans ASP.NET Core - Mise à jour de données associées - 7 sur 8</span><span class="sxs-lookup"><span data-stu-id="bc928-103">Razor Pages with EF Core in ASP.NET Core - Update Related Data - 7 of 8</span></span>

<span data-ttu-id="bc928-104">De [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bc928-104">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="bc928-105">Ce didacticiel illustre la mise à jour de données associées.</span><span class="sxs-lookup"><span data-stu-id="bc928-105">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="bc928-106">Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, [téléchargez ou affichez l’application terminée](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="bc928-106">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="bc928-107">[Télécharger les instructions](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="bc928-107">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="bc928-108">Les illustrations suivantes montrent certaines des pages terminées.</span><span class="sxs-lookup"><span data-stu-id="bc928-108">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="bc928-109">![Page de modification de cours](update-related-data/_static/course-edit.png)
![Page de modification de formateur](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="bc928-109">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="bc928-110">Examinez et testez les pages des cours Create et Edit.</span><span class="sxs-lookup"><span data-stu-id="bc928-110">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="bc928-111">Créez un nouveau cours.</span><span class="sxs-lookup"><span data-stu-id="bc928-111">Create a new course.</span></span> <span data-ttu-id="bc928-112">Le service est sélectionné par sa clé primaire (entier), pas par son nom.</span><span class="sxs-lookup"><span data-stu-id="bc928-112">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="bc928-113">Modifiez le nouveau cours.</span><span class="sxs-lookup"><span data-stu-id="bc928-113">Edit the new course.</span></span> <span data-ttu-id="bc928-114">Lorsque vous avez terminé le test, supprimez le nouveau cours.</span><span class="sxs-lookup"><span data-stu-id="bc928-114">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="bc928-115">Créer une classe de base pour partager le code commun</span><span class="sxs-lookup"><span data-stu-id="bc928-115">Create a base class to share common code</span></span>

<span data-ttu-id="bc928-116">Les pages de création et de modification de cours ont chacune besoin d’une liste de noms de départements.</span><span class="sxs-lookup"><span data-stu-id="bc928-116">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="bc928-117">Créez la classe de base *Pages/Courses/DepartmentNamePageModel.cshtml.cs* pour les pages Create et Edit :</span><span class="sxs-lookup"><span data-stu-id="bc928-117">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="bc928-118">Le code précédent crée un [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) pour contenir la liste des noms de département. </span><span class="sxs-lookup"><span data-stu-id="bc928-118">The preceding code creates a [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="bc928-119">Si `selectedDepartment` est spécifié, ce département est sélectionné dans le `SelectList`. </span><span class="sxs-lookup"><span data-stu-id="bc928-119">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="bc928-120">Les classes de modèle de page Create et Edit doivent dériver de `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="bc928-120">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="bc928-121">Personnaliser les pages de cours</span><span class="sxs-lookup"><span data-stu-id="bc928-121">Customize the Courses Pages</span></span>

<span data-ttu-id="bc928-122">Quand une entité de cours est créée, elle doit avoir une relation à un département existant.</span><span class="sxs-lookup"><span data-stu-id="bc928-122">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="bc928-123">Pour ajouter un département lors de la création d’un cours, la classe de base pour Create et Edit contient une liste déroulante de sélection du département.</span><span class="sxs-lookup"><span data-stu-id="bc928-123">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="bc928-124">La liste déroulante définit la propriété de clé étrangère `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="bc928-124">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="bc928-125">EF Core utilise la clé étrangère `Course.DepartmentID` pour charger la propriété de navigation `Department`.</span><span class="sxs-lookup"><span data-stu-id="bc928-125">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![Créer le cours](update-related-data/_static/ddl.png)

<span data-ttu-id="bc928-127">Mettez à jour le modèle de la page Create avec le code suivant : </span><span class="sxs-lookup"><span data-stu-id="bc928-127">Update the Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-999)]

<span data-ttu-id="bc928-128">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="bc928-128">The preceding code:</span></span>

* <span data-ttu-id="bc928-129">Dérive de `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="bc928-129">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="bc928-130">Utilise `TryUpdateModelAsync` pour empêcher la [sur-validation](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="bc928-130">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="bc928-131">Remplace `ViewData["DepartmentID"]` par `DepartmentNameSL` (à partir de la classe de base).</span><span class="sxs-lookup"><span data-stu-id="bc928-131">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="bc928-132">`ViewData["DepartmentID"]` est remplacé par le `DepartmentNameSL` fortement typé.</span><span class="sxs-lookup"><span data-stu-id="bc928-132">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="bc928-133">Les modèles fortement typés sont préférables aux modèles faiblement typés.</span><span class="sxs-lookup"><span data-stu-id="bc928-133">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="bc928-134">Pour plus d’informations, consultez [Données faiblement typées (ViewData et ViewBag)](xref:mvc/views/overview#VD_VB).</span><span class="sxs-lookup"><span data-stu-id="bc928-134">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="bc928-135">Mettre à jour la page de création de cours</span><span class="sxs-lookup"><span data-stu-id="bc928-135">Update the Courses Create page</span></span>

<span data-ttu-id="bc928-136">Mettez à jour *Pages/Courses/Create.cshtml* avec le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="bc928-136">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="bc928-137">Le balisage précédent apporte les modifications suivantes :</span><span class="sxs-lookup"><span data-stu-id="bc928-137">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="bc928-138">Modifie la légende de **DepartmentID** en **Department**. </span><span class="sxs-lookup"><span data-stu-id="bc928-138">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="bc928-139">Il remplace `"ViewBag.DepartmentID"` par `DepartmentNameSL` (à partir de la classe de base).</span><span class="sxs-lookup"><span data-stu-id="bc928-139">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="bc928-140">Il ajoute l’option « Select Department ».</span><span class="sxs-lookup"><span data-stu-id="bc928-140">Adds the "Select Department" option.</span></span> <span data-ttu-id="bc928-141">Cette modification entraîne l’affichage de « Select Department » plutôt que du premier département.</span><span class="sxs-lookup"><span data-stu-id="bc928-141">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="bc928-142">Il ajoute un message de validation quand le département n’est pas sélectionné.</span><span class="sxs-lookup"><span data-stu-id="bc928-142">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="bc928-143">La Page Razor utilise le [Tag Helper Select](xref:mvc/views/working-with-forms#the-select-tag-helper) :</span><span class="sxs-lookup"><span data-stu-id="bc928-143">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="bc928-144">Testez la page Create.</span><span class="sxs-lookup"><span data-stu-id="bc928-144">Test the Create page.</span></span> <span data-ttu-id="bc928-145">La page Create affiche le nom du département plutôt que son ID.</span><span class="sxs-lookup"><span data-stu-id="bc928-145">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="bc928-146">Mise à jour de la page Edit d'un cours.</span><span class="sxs-lookup"><span data-stu-id="bc928-146">Update the Courses Edit page.</span></span>

<span data-ttu-id="bc928-147">Mettez à jour le modèle de modification de cours avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="bc928-147">Update the edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-999)]

<span data-ttu-id="bc928-148">Les modifications sont semblables à celles effectuées dans le modèle de page Create.</span><span class="sxs-lookup"><span data-stu-id="bc928-148">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="bc928-149">Dans le code précédent, `PopulateDepartmentsDropDownList` transmet l’ID de département, ce qui sélectionne le département spécifié dans la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="bc928-149">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="bc928-150">Mettez à jour *Pages/Courses/Edit.cshtml* avec le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="bc928-150">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="bc928-151">Le balisage précédent apporte les modifications suivantes :</span><span class="sxs-lookup"><span data-stu-id="bc928-151">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="bc928-152">Affiche l’identificateur du cours.</span><span class="sxs-lookup"><span data-stu-id="bc928-152">Displays the course ID.</span></span> <span data-ttu-id="bc928-153">En général la clé primaire (PK) d’une entité n’est pas affichée.</span><span class="sxs-lookup"><span data-stu-id="bc928-153">Generally the Primary Key (PK) of an entity isn't displayed.</span></span> <span data-ttu-id="bc928-154">Les clés primaires sont généralement sans signification pour les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="bc928-154">PKs are usually meaningless to users.</span></span> <span data-ttu-id="bc928-155">Dans ce cas, la clé est le numéro de cours.</span><span class="sxs-lookup"><span data-stu-id="bc928-155">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="bc928-156">Modifie la légende de **DepartmentID** en **Department**. </span><span class="sxs-lookup"><span data-stu-id="bc928-156">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="bc928-157">Il remplace `"ViewBag.DepartmentID"` par `DepartmentNameSL` (à partir de la classe de base).</span><span class="sxs-lookup"><span data-stu-id="bc928-157">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="bc928-158">La page contient un champ masqué (`<input type="hidden">`) pour le numéro de cours.</span><span class="sxs-lookup"><span data-stu-id="bc928-158">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="bc928-159">L’ajout d’un Tag Helper `<label>` avec `asp-for="Course.CourseID"` n’élimine pas la nécessité de la présence du champ masqué.</span><span class="sxs-lookup"><span data-stu-id="bc928-159">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="bc928-160">`<input type="hidden">` est obligatoire pour que le numéro de cours soit inclus dans les données publiées quand l’utilisateur clique sur **Save**.</span><span class="sxs-lookup"><span data-stu-id="bc928-160">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="bc928-161">Testez le code mis à jour.</span><span class="sxs-lookup"><span data-stu-id="bc928-161">Test the updated code.</span></span> <span data-ttu-id="bc928-162">Créez, modifiez et supprimez un cours.</span><span class="sxs-lookup"><span data-stu-id="bc928-162">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="bc928-163">Ajouter AsNoTracking aux modèles de page Details et Delete</span><span class="sxs-lookup"><span data-stu-id="bc928-163">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="bc928-164">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) peut améliorer les performances quand le suivi n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="bc928-164">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking isn't required.</span></span> <span data-ttu-id="bc928-165">Ajoutez `AsNoTracking` dans les modèles de page Details et Delete.</span><span class="sxs-lookup"><span data-stu-id="bc928-165">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="bc928-166">Le code suivant montre le modèle de page Delete mis à jour :</span><span class="sxs-lookup"><span data-stu-id="bc928-166">The following code shows the updated Delete page model:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="bc928-167">Mettez à jour la méthode `OnGetAsync` dans le fichier *Pages/Courses/Details.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="bc928-167">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="bc928-168">Modifier les pages Delete et Details</span><span class="sxs-lookup"><span data-stu-id="bc928-168">Modify the Delete and Details pages</span></span>

<span data-ttu-id="bc928-169">Mettez à jour la page Razor Delete avec le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="bc928-169">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="bc928-170">Apportez les mêmes modifications à la page Details.</span><span class="sxs-lookup"><span data-stu-id="bc928-170">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="bc928-171">Tester les pages de cours</span><span class="sxs-lookup"><span data-stu-id="bc928-171">Test the Course pages</span></span>

<span data-ttu-id="bc928-172">Testez les fonctionnalités de création, de modification, de détails et de suppression.</span><span class="sxs-lookup"><span data-stu-id="bc928-172">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="bc928-173">Mettre à jour les pages d'instructeur</span><span class="sxs-lookup"><span data-stu-id="bc928-173">Update the instructor pages</span></span>

<span data-ttu-id="bc928-174">Les sections suivantes mettent à jour les pages d'instructeur.</span><span class="sxs-lookup"><span data-stu-id="bc928-174">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="bc928-175">Ajouter l’emplacement du bureau</span><span class="sxs-lookup"><span data-stu-id="bc928-175">Add office location</span></span>

<span data-ttu-id="bc928-176">Lors de la modification d’un enregistrement de formateur, vous souhaiterez peut-être mettre à jour l’attribution de bureau du formateur.</span><span class="sxs-lookup"><span data-stu-id="bc928-176">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="bc928-177">L’entité `Instructor` a une relation un-à-zéro-ou-un avec l’entité `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="bc928-177">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="bc928-178">Le code de formateur doit gérer ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="bc928-178">The instructor code must handle:</span></span>

* <span data-ttu-id="bc928-179">Si l’utilisateur efface l’attribution d'un bureau, supprimer l'entité `OfficeAssignment`. </span><span class="sxs-lookup"><span data-stu-id="bc928-179">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="bc928-180">Si l’utilisateur entre une attribution et qu'elle était vide, créer une nouvelle entité `OfficeAssignment`. </span><span class="sxs-lookup"><span data-stu-id="bc928-180">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="bc928-181">Si l’utilisateur change l’attribution de bureau, mettre à jour l’entité `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="bc928-181">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="bc928-182">Mettez à jour le modèle de page Edit d'instructeur avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="bc928-182">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-999)]

<span data-ttu-id="bc928-183">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="bc928-183">The preceding code:</span></span>

* <span data-ttu-id="bc928-184">Obtient l'entité `Instructor` en cours à partir de la base de données à l’aide d’un chargement hâtif de la propriété de navigation `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="bc928-184">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
* <span data-ttu-id="bc928-185">Met à jour l’entité `Instructor` récupérée avec les valeurs du classeur de modèles.</span><span class="sxs-lookup"><span data-stu-id="bc928-185">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="bc928-186">`TryUpdateModel` empêche la [survalidation](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="bc928-186">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="bc928-187">Si l’emplacement du bureau est vide, définit `Instructor.OfficeAssignment` avec la valeur null.</span><span class="sxs-lookup"><span data-stu-id="bc928-187">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="bc928-188">Lorsque `Instructor.OfficeAssignment` est null, la ligne correspondante dans la table `OfficeAssignment` est supprimée.</span><span class="sxs-lookup"><span data-stu-id="bc928-188">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="bc928-189">Mettre à jour la page de modification de formateur</span><span class="sxs-lookup"><span data-stu-id="bc928-189">Update the instructor Edit page</span></span>

<span data-ttu-id="bc928-190">Mettez à jour *Pages/Instructors/Edit.cshtml* avec l’emplacement du bureau :</span><span class="sxs-lookup"><span data-stu-id="bc928-190">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="bc928-191">Vérifiez que vous pouvez modifier l'emplacement de bureau d'un instructeur.</span><span class="sxs-lookup"><span data-stu-id="bc928-191">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="bc928-192">Ajouter des affectations de cours à la page Edit de l'instructeur</span><span class="sxs-lookup"><span data-stu-id="bc928-192">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="bc928-193">Les instructeurs peuvent enseigner dans n’importe quel nombre de cours.</span><span class="sxs-lookup"><span data-stu-id="bc928-193">Instructors may teach any number of courses.</span></span> <span data-ttu-id="bc928-194">Dans cette section, vous ajoutez la possibilité de modifier les affectations de cours.</span><span class="sxs-lookup"><span data-stu-id="bc928-194">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="bc928-195">L’illustration suivante montre la page de modification de l'instructeur mise à jour :</span><span class="sxs-lookup"><span data-stu-id="bc928-195">The following image shows the updated instructor Edit page:</span></span>

![Page de modification de formateur avec des cours](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="bc928-197">`Course` et `Instructor` entretiennent une relation plusieurs-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="bc928-197">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="bc928-198">Pour ajouter et supprimer des relations, vous ajoutez et supprimez des entités à partir du jeu d’entités de jointures `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="bc928-198">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="bc928-199">Les cases à cocher permettent de changer les cours auxquels un formateur est assigné.</span><span class="sxs-lookup"><span data-stu-id="bc928-199">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="bc928-200">Une case à cocher est affichée pour chaque cours dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="bc928-200">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="bc928-201">Les cours auxquels le formateur est affecté sont cochés.</span><span class="sxs-lookup"><span data-stu-id="bc928-201">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="bc928-202">L’utilisateur peut cocher ou décocher les cases pour modifier les affectations de cours.</span><span class="sxs-lookup"><span data-stu-id="bc928-202">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="bc928-203">Si le nombre de cours était beaucoup plus élevé :</span><span class="sxs-lookup"><span data-stu-id="bc928-203">If the number of courses were much greater:</span></span>

* <span data-ttu-id="bc928-204">Vous utiliseriez sans doute une interface utilisateur différente pour afficher les cours.</span><span class="sxs-lookup"><span data-stu-id="bc928-204">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="bc928-205">La méthode de manipulation d’une entité de jointure pour créer ou supprimer des relations ne changerait pas.</span><span class="sxs-lookup"><span data-stu-id="bc928-205">The method of manipulating a join entity to create or delete relationships wouldn't change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="bc928-206">Ajouter des classes pour prendre en charge Create et Edit des pages d'instructeur</span><span class="sxs-lookup"><span data-stu-id="bc928-206">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="bc928-207">Créez *SchoolViewModels/AssignedCourseData.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="bc928-207">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="bc928-208">La classe `AssignedCourseData` contient des données pour créer les cases à cocher pour les cours attribués par un formateur.</span><span class="sxs-lookup"><span data-stu-id="bc928-208">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="bc928-209">Créez la classe de base *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="bc928-209">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="bc928-210">`InstructorCoursesPageModel` est la classe de base que vous utiliserez pour les modèles de page Edit et Create.</span><span class="sxs-lookup"><span data-stu-id="bc928-210">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="bc928-211">`PopulateAssignedCourseData` lit toutes les entités `Course` pour renseigner `AssignedCourseDataList`.</span><span class="sxs-lookup"><span data-stu-id="bc928-211">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="bc928-212">Pour chaque cours, le code définit le `CourseID`, le titre, et si le formateur est affecté au cours.</span><span class="sxs-lookup"><span data-stu-id="bc928-212">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="bc928-213">Un [HashSet](/dotnet/api/system.collections.generic.hashset-1) est utilisé pour créer des recherches efficaces.</span><span class="sxs-lookup"><span data-stu-id="bc928-213">A [HashSet](/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="bc928-214">Modèle de page de modification de formateur</span><span class="sxs-lookup"><span data-stu-id="bc928-214">Instructors Edit page model</span></span>

<span data-ttu-id="bc928-215">Mettez à jour le modèle de page Edit d'instructeur avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="bc928-215">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-999)]

<span data-ttu-id="bc928-216">Le code précédent gère les changements d’affectation de bureau.</span><span class="sxs-lookup"><span data-stu-id="bc928-216">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="bc928-217">Mettez à jour la vue Razor Instructeur :</span><span class="sxs-lookup"><span data-stu-id="bc928-217">Update the instructor Razor View:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="bc928-218">Quand vous collez le code dans Visual Studio, les sauts de ligne sont modifiés de telle manière que le code est rompu.</span><span class="sxs-lookup"><span data-stu-id="bc928-218">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="bc928-219">Appuyez une fois sur Ctrl + Z pour annuler la mise en forme automatique.</span><span class="sxs-lookup"><span data-stu-id="bc928-219">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="bc928-220">Ctrl + Z corrige les sauts de ligne afin qu’ils aient l’aspect visible ici.</span><span class="sxs-lookup"><span data-stu-id="bc928-220">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="bc928-221">La mise en retrait ne doit pas nécessairement être parfaite, mais les lignes `@</tr><tr>`, `@:<td>`, `@:</td>` et `@:</tr>` doivent chacune être sur une seule distincte comme indiqué.</span><span class="sxs-lookup"><span data-stu-id="bc928-221">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="bc928-222">Avec le bloc de nouveau code sélectionné, appuyez trois fois sur Tab pour aligner le nouveau code avec le code existant.</span><span class="sxs-lookup"><span data-stu-id="bc928-222">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="bc928-223">Votez ou vérifiez l’état de ce bogue [avec ce lien](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="bc928-223">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="bc928-224">Le code précédent crée une table HTML avec trois colonnes.</span><span class="sxs-lookup"><span data-stu-id="bc928-224">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="bc928-225">Chaque colonne a une case à cocher et une légende contenant le numéro et le titre du cours.</span><span class="sxs-lookup"><span data-stu-id="bc928-225">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="bc928-226">Les cases à cocher ont toutes le même nom (« selectedCourses »).</span><span class="sxs-lookup"><span data-stu-id="bc928-226">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="bc928-227">L’utilisation du même nom signale au classeur de modèles qu’il doit les traiter en tant que groupe.</span><span class="sxs-lookup"><span data-stu-id="bc928-227">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="bc928-228">L’attribut de valeur de chaque case à cocher est défini sur `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="bc928-228">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="bc928-229">Quand la page est publiée, le classeur de modèles transmet un tableau composé des valeurs `CourseID` correspondant uniquement aux cases à cocher sélectionnées.</span><span class="sxs-lookup"><span data-stu-id="bc928-229">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="bc928-230">Lors de l’affichage initial des cases à cocher, les cours affectés au formateur ont des attributs cochés.</span><span class="sxs-lookup"><span data-stu-id="bc928-230">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="bc928-231">Exécutez l’application et testez la page Edit des formateurs mise à jour.</span><span class="sxs-lookup"><span data-stu-id="bc928-231">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="bc928-232">Changez certaines affectations de cours.</span><span class="sxs-lookup"><span data-stu-id="bc928-232">Change some course assignments.</span></span> <span data-ttu-id="bc928-233">Les modifications sont répercutées dans la page Index.</span><span class="sxs-lookup"><span data-stu-id="bc928-233">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="bc928-234">Remarque : L’approche adoptée ici pour modifier les données des cours des formateurs fonctionne bien le nombre de cours est limité.</span><span class="sxs-lookup"><span data-stu-id="bc928-234">Note: The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="bc928-235">Pour les collections qui sont beaucoup plus grandes, une interface utilisateur différente et une autre méthode de mise à jour seraient plus utiles et plus efficaces.</span><span class="sxs-lookup"><span data-stu-id="bc928-235">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="bc928-236">Mise à jour de la page de création des instructeurs</span><span class="sxs-lookup"><span data-stu-id="bc928-236">Update the instructors Create page</span></span>

<span data-ttu-id="bc928-237">Mettez à jour le modèle de page de création de formateur avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="bc928-237">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="bc928-238">Le code précédent est similaire au code de *Pages/Instructors/Edit.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="bc928-238">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="bc928-239">Mettez à jour la page Razor de création de formateur avec le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="bc928-239">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="bc928-240">Testez la page de création de l'instructeur.</span><span class="sxs-lookup"><span data-stu-id="bc928-240">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="bc928-241">Mettre à jour la page Delete</span><span class="sxs-lookup"><span data-stu-id="bc928-241">Update the Delete page</span></span>

<span data-ttu-id="bc928-242">Mettez à jour le modèle de page de suppression avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="bc928-242">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-999)]

<span data-ttu-id="bc928-243">Le code précédent apporte les modifications suivantes :</span><span class="sxs-lookup"><span data-stu-id="bc928-243">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="bc928-244">Utilise un chargement hâtif pour la propriété de navigation `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="bc928-244">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="bc928-245">Les `CourseAssignments` doivent être inclus ou ils ne seront pas supprimés lorsque l'instructeur est supprimé.</span><span class="sxs-lookup"><span data-stu-id="bc928-245">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="bc928-246">Pour éviter d’avoir à les lire, configurez la suppression en cascade dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="bc928-246">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="bc928-247">Si le formateur à supprimer est attribué en tant qu’administrateur d’un département, supprime l’attribution de l'instructeur de ces départements.</span><span class="sxs-lookup"><span data-stu-id="bc928-247">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bc928-248">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="bc928-248">Additional resources</span></span>

* [<span data-ttu-id="bc928-249">Version YouTube de ce tutoriel (Partie 1)</span><span class="sxs-lookup"><span data-stu-id="bc928-249">YouTube version of this tutorial (Part 1)</span></span>](https://www.youtube.com/watch?v=Csh6gkmwc9E)
* [<span data-ttu-id="bc928-250">Version YouTube de ce tutoriel (Partie 2)</span><span class="sxs-lookup"><span data-stu-id="bc928-250">YouTube version of this tutorial (Part 2)</span></span>](https://www.youtube.com/watch?v=mOAankB_Zgc)

> [!div class="step-by-step"]
> <span data-ttu-id="bc928-251">[Précédent](xref:data/ef-rp/read-related-data)
> [Suivant](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="bc928-251">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>
