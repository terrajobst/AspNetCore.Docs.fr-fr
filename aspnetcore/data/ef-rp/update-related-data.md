---
title: Pages Razor avec EF Core - Mise à jour de données associées - 7 sur 8
author: rick-anderson
description: Dans ce didacticiel, vous allez mettre à jour des données associées en mettant à jour des propriétés de navigation et des champs de clé étrangère.
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/update-related-data
ms.openlocfilehash: 5c91c91ab938f3aa4abc55049c54f399469f6163
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/31/2018
---
# <a name="updating-related-data---ef-core-razor-pages-7-of-8"></a><span data-ttu-id="dcf1e-103">Mise à jour des données associées - Pages Razor avec EF Core (7 sur 8)</span><span class="sxs-lookup"><span data-stu-id="dcf1e-103">Updating related data - EF Core Razor Pages (7 of 8)</span></span>

<span data-ttu-id="dcf1e-104">De [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dcf1e-104">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="dcf1e-105">Ce didacticiel illustre la mise à jour de données associées.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-105">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="dcf1e-106">Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, téléchargez l’[application terminée pour cette phase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span><span class="sxs-lookup"><span data-stu-id="dcf1e-106">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span></span>

<span data-ttu-id="dcf1e-107">Les illustrations suivantes montrent certaines des pages terminées.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-107">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="dcf1e-108">![Page de modification de cours](update-related-data/_static/course-edit.png)
![Page de modification de formateur](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="dcf1e-108">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="dcf1e-109">Examinez et testez les pages des cours Create et Edit.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-109">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="dcf1e-110">Créez un nouveau cours.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-110">Create a new course.</span></span> <span data-ttu-id="dcf1e-111">Le service est sélectionné par sa clé primaire (entier), pas par son nom.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-111">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="dcf1e-112">Modifiez le nouveau cours.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-112">Edit the new course.</span></span> <span data-ttu-id="dcf1e-113">Lorsque vous avez terminé le test, supprimez le nouveau cours.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-113">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="dcf1e-114">Créer une classe de base pour partager le code commun</span><span class="sxs-lookup"><span data-stu-id="dcf1e-114">Create a base class to share common code</span></span>

<span data-ttu-id="dcf1e-115">Les pages de création et de modification de cours ont chacune besoin d’une liste de noms de départements.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-115">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="dcf1e-116">Créez la classe de base *Pages/Courses/DepartmentNamePageModel.cshtml.cs* pour les pages Create et Edit :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-116">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="dcf1e-117">Le code précédent crée un [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) pour contenir la liste des noms de département. </span><span class="sxs-lookup"><span data-stu-id="dcf1e-117">The preceding code creates a [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="dcf1e-118">Si `selectedDepartment` est spécifié, ce département est sélectionné dans le `SelectList`. </span><span class="sxs-lookup"><span data-stu-id="dcf1e-118">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="dcf1e-119">Les classes de modèle de page Create et Edit doivent dériver de `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-119">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="dcf1e-120">Personnaliser les pages de cours</span><span class="sxs-lookup"><span data-stu-id="dcf1e-120">Customize the Courses Pages</span></span>

<span data-ttu-id="dcf1e-121">Quand une entité de cours est créée, elle doit avoir une relation à un département existant.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-121">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="dcf1e-122">Pour ajouter un département lors de la création d’un cours, la classe de base pour Create et Edit contient une liste déroulante de sélection du département.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-122">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="dcf1e-123">La liste déroulante définit la propriété de clé étrangère `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-123">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="dcf1e-124">EF Core utilise la clé étrangère `Course.DepartmentID` pour charger la propriété de navigation `Department`.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-124">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![Créer le cours](update-related-data/_static/ddl.png)

<span data-ttu-id="dcf1e-126">Mettez à jour le modèle de la page Create avec le code suivant : </span><span class="sxs-lookup"><span data-stu-id="dcf1e-126">Update the Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-)]

<span data-ttu-id="dcf1e-127">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-127">The preceding code:</span></span>

* <span data-ttu-id="dcf1e-128">Dérive de `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-128">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="dcf1e-129">Utilise `TryUpdateModelAsync` pour empêcher la [sur-validation](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="dcf1e-129">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="dcf1e-130">Remplace `ViewData["DepartmentID"]` par `DepartmentNameSL` (à partir de la classe de base).</span><span class="sxs-lookup"><span data-stu-id="dcf1e-130">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="dcf1e-131">`ViewData["DepartmentID"]` est remplacé par le `DepartmentNameSL` fortement typé.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-131">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="dcf1e-132">Les modèles fortement typés sont préférables aux modèles faiblement typés.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-132">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="dcf1e-133">Pour plus d’informations, consultez [Données faiblement typées (ViewData et ViewBag)](xref:mvc/views/overview#VD_VB).</span><span class="sxs-lookup"><span data-stu-id="dcf1e-133">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="dcf1e-134">Mettre à jour la page de création de cours</span><span class="sxs-lookup"><span data-stu-id="dcf1e-134">Update the Courses Create page</span></span>

<span data-ttu-id="dcf1e-135">Mettez à jour *Pages/Courses/Create.cshtml* avec le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-135">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="dcf1e-136">Le balisage précédent apporte les modifications suivantes :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-136">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="dcf1e-137">Modifie la légende de **DepartmentID** en **Department**. </span><span class="sxs-lookup"><span data-stu-id="dcf1e-137">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="dcf1e-138">Il remplace `"ViewBag.DepartmentID"` par `DepartmentNameSL` (à partir de la classe de base).</span><span class="sxs-lookup"><span data-stu-id="dcf1e-138">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="dcf1e-139">Il ajoute l’option « Select Department ».</span><span class="sxs-lookup"><span data-stu-id="dcf1e-139">Adds the "Select Department" option.</span></span> <span data-ttu-id="dcf1e-140">Cette modification entraîne l’affichage de « Select Department » plutôt que du premier département.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-140">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="dcf1e-141">Il ajoute un message de validation quand le département n’est pas sélectionné.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-141">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="dcf1e-142">La Page Razor utilise le [Tag Helper Select](xref:mvc/views/working-with-forms#the-select-tag-helper) :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-142">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="dcf1e-143">Testez la page Create.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-143">Test the Create page.</span></span> <span data-ttu-id="dcf1e-144">La page Create affiche le nom du département plutôt que son ID.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-144">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="dcf1e-145">Mise à jour de la page Edit d'un cours.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-145">Update the Courses Edit page.</span></span>

<span data-ttu-id="dcf1e-146">Mettez à jour le modèle de modification de cours avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-146">Update the edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-)]

<span data-ttu-id="dcf1e-147">Les modifications sont semblables à celles effectuées dans le modèle de page Create.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-147">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="dcf1e-148">Dans le code précédent, `PopulateDepartmentsDropDownList` transmet l’ID de département, ce qui sélectionne le département spécifié dans la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-148">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="dcf1e-149">Mettez à jour *Pages/Courses/Edit.cshtml* avec le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-149">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="dcf1e-150">Le balisage précédent apporte les modifications suivantes :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-150">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="dcf1e-151">Affiche l’identificateur du cours.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-151">Displays the course ID.</span></span> <span data-ttu-id="dcf1e-152">En général la clé primaire (PK) d’une entité n’est pas affichée.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-152">Generally the Primary Key (PK) of an entity isn't displayed.</span></span> <span data-ttu-id="dcf1e-153">Les clés primaires sont généralement sans signification pour les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-153">PKs are usually meaningless to users.</span></span> <span data-ttu-id="dcf1e-154">Dans ce cas, la clé est le numéro de cours.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-154">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="dcf1e-155">Modifie la légende de **DepartmentID** en **Department**. </span><span class="sxs-lookup"><span data-stu-id="dcf1e-155">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="dcf1e-156">Il remplace `"ViewBag.DepartmentID"` par `DepartmentNameSL` (à partir de la classe de base).</span><span class="sxs-lookup"><span data-stu-id="dcf1e-156">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="dcf1e-157">Il ajoute l’option « Select Department ».</span><span class="sxs-lookup"><span data-stu-id="dcf1e-157">Adds the "Select Department" option.</span></span> <span data-ttu-id="dcf1e-158">Cette modification entraîne l’affichage de « Select Department » plutôt que du premier département.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-158">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="dcf1e-159">Il ajoute un message de validation quand le département n’est pas sélectionné.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-159">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="dcf1e-160">La page contient un champ masqué (`<input type="hidden">`) pour le numéro de cours.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-160">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="dcf1e-161">L’ajout d’un Tag Helper `<label>` avec `asp-for="Course.CourseID"` n’élimine pas la nécessité de la présence du champ masqué.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-161">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="dcf1e-162">`<input type="hidden">` est obligatoire pour que le numéro de cours soit inclus dans les données publiées quand l’utilisateur clique sur **Save**.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-162">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="dcf1e-163">Testez le code mis à jour.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-163">Test the updated code.</span></span> <span data-ttu-id="dcf1e-164">Créez, modifiez et supprimez un cours.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-164">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="dcf1e-165">Ajouter AsNoTracking aux modèles de page Details et Delete</span><span class="sxs-lookup"><span data-stu-id="dcf1e-165">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="dcf1e-166">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) peut améliorer les performances quand le suivi n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-166">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking isn't required.</span></span> <span data-ttu-id="dcf1e-167">Ajoutez `AsNoTracking` dans les modèles de page Details et Delete.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-167">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="dcf1e-168">Le code suivant montre le modèle de page Delete mis à jour :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-168">The following code shows the updated Delete page model:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="dcf1e-169">Mettez à jour la méthode `OnGetAsync` dans le fichier *Pages/Courses/Details.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-169">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="dcf1e-170">Modifier les pages Delete et Details</span><span class="sxs-lookup"><span data-stu-id="dcf1e-170">Modify the Delete and Details pages</span></span>

<span data-ttu-id="dcf1e-171">Mettez à jour la page Razor Delete avec le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-171">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="dcf1e-172">Apportez les mêmes modifications à la page Details.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-172">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="dcf1e-173">Tester les pages de cours</span><span class="sxs-lookup"><span data-stu-id="dcf1e-173">Test the Course pages</span></span>

<span data-ttu-id="dcf1e-174">Testez les fonctionnalités de création, de modification, de détails et de suppression.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-174">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="dcf1e-175">Mettre à jour les pages d'instructeur</span><span class="sxs-lookup"><span data-stu-id="dcf1e-175">Update the instructor pages</span></span>

<span data-ttu-id="dcf1e-176">Les sections suivantes mettent à jour les pages d'instructeur.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-176">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="dcf1e-177">Ajouter l’emplacement du bureau</span><span class="sxs-lookup"><span data-stu-id="dcf1e-177">Add office location</span></span>

<span data-ttu-id="dcf1e-178">Lors de la modification d’un enregistrement de formateur, vous souhaiterez peut-être mettre à jour l’attribution de bureau du formateur.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-178">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="dcf1e-179">L’entité `Instructor` a une relation un-à-zéro-ou-un avec l’entité `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-179">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="dcf1e-180">Le code de formateur doit gérer ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-180">The instructor code must handle:</span></span>

* <span data-ttu-id="dcf1e-181">Si l’utilisateur efface l’attribution d'un bureau, supprimer l'entité `OfficeAssignment`. </span><span class="sxs-lookup"><span data-stu-id="dcf1e-181">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="dcf1e-182">Si l’utilisateur entre une attribution et qu'elle était vide, créer une nouvelle entité `OfficeAssignment`. </span><span class="sxs-lookup"><span data-stu-id="dcf1e-182">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="dcf1e-183">Si l’utilisateur change l’attribution de bureau, mettre à jour l’entité `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-183">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="dcf1e-184">Mettez à jour le modèle de page Edit d'instructeur avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-184">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-)]

<span data-ttu-id="dcf1e-185">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-185">The preceding code:</span></span>

- <span data-ttu-id="dcf1e-186">Obtient l'entité `Instructor` en cours à partir de la base de données à l’aide d’un chargement hâtif de la propriété de navigation `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-186">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
- <span data-ttu-id="dcf1e-187">Met à jour l’entité `Instructor` récupérée avec les valeurs du classeur de modèles.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-187">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="dcf1e-188">`TryUpdateModel` empêche la [survalidation](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="dcf1e-188">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
- <span data-ttu-id="dcf1e-189">Si l’emplacement du bureau est vide, définit `Instructor.OfficeAssignment` avec la valeur null.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-189">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="dcf1e-190">Lorsque `Instructor.OfficeAssignment` est null, la ligne correspondante dans la table `OfficeAssignment` est supprimée.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-190">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="dcf1e-191">Mettre à jour la page de modification de formateur</span><span class="sxs-lookup"><span data-stu-id="dcf1e-191">Update the instructor Edit page</span></span>

<span data-ttu-id="dcf1e-192">Mettez à jour *Pages/Instructors/Edit.cshtml* avec l’emplacement du bureau :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-192">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="dcf1e-193">Vérifiez que vous pouvez modifier l'emplacement de bureau d'un instructeur.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-193">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="dcf1e-194">Ajouter des affectations de cours à la page Edit de l'instructeur</span><span class="sxs-lookup"><span data-stu-id="dcf1e-194">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="dcf1e-195">Les instructeurs peuvent enseigner dans n’importe quel nombre de cours.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-195">Instructors may teach any number of courses.</span></span> <span data-ttu-id="dcf1e-196">Dans cette section, vous ajoutez la possibilité de modifier les affectations de cours.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-196">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="dcf1e-197">L’illustration suivante montre la page de modification de l'instructeur mise à jour :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-197">The following image shows the updated instructor Edit page:</span></span>

![Page de modification de formateur avec des cours](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="dcf1e-199">`Course` et `Instructor` entretiennent une relation plusieurs-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-199">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="dcf1e-200">Pour ajouter et supprimer des relations, vous ajoutez et supprimez des entités à partir du jeu d’entités de jointures `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-200">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="dcf1e-201">Les cases à cocher permettent de changer les cours auxquels un formateur est assigné.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-201">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="dcf1e-202">Une case à cocher est affichée pour chaque cours dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-202">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="dcf1e-203">Les cours auxquels le formateur est affecté sont cochés.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-203">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="dcf1e-204">L’utilisateur peut cocher ou décocher les cases pour modifier les affectations de cours.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-204">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="dcf1e-205">Si le nombre de cours était beaucoup plus élevé :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-205">If the number of courses were much greater:</span></span>

* <span data-ttu-id="dcf1e-206">Vous utiliseriez sans doute une interface utilisateur différente pour afficher les cours.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-206">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="dcf1e-207">La méthode de manipulation d’une entité de jointure pour créer ou supprimer des relations ne changerait pas.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-207">The method of manipulating a join entity to create or delete relationships wouldn't change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="dcf1e-208">Ajouter des classes pour prendre en charge Create et Edit des pages d'instructeur</span><span class="sxs-lookup"><span data-stu-id="dcf1e-208">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="dcf1e-209">Créez *SchoolViewModels/AssignedCourseData.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-209">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="dcf1e-210">La classe `AssignedCourseData` contient des données pour créer les cases à cocher pour les cours attribués par un formateur.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-210">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="dcf1e-211">Créez la classe de base *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-211">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="dcf1e-212">`InstructorCoursesPageModel` est la classe de base que vous utiliserez pour les modèles de page Edit et Create.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-212">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="dcf1e-213">`PopulateAssignedCourseData` lit toutes les entités `Course` pour renseigner `AssignedCourseDataList`.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-213">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="dcf1e-214">Pour chaque cours, le code définit le `CourseID`, le titre, et si le formateur est affecté au cours.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-214">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="dcf1e-215">Un [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) est utilisé pour créer des recherches efficaces.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-215">A [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="dcf1e-216">Modèle de page de modification de formateur</span><span class="sxs-lookup"><span data-stu-id="dcf1e-216">Instructors Edit page model</span></span>

<span data-ttu-id="dcf1e-217">Mettez à jour le modèle de page Edit d'instructeur avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-217">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-)]

<span data-ttu-id="dcf1e-218">Le code précédent gère les changements d’affectation de bureau.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-218">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="dcf1e-219">Mettez à jour la vue Razor Instructeur :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-219">Update the instructor Razor View:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="dcf1e-220">Quand vous collez le code dans Visual Studio, les sauts de ligne sont modifiés de telle manière que le code est rompu.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-220">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="dcf1e-221">Appuyez une fois sur Ctrl + Z pour annuler la mise en forme automatique.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-221">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="dcf1e-222">Ctrl + Z corrige les sauts de ligne afin qu’ils aient l’aspect visible ici.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-222">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="dcf1e-223">La mise en retrait ne doit pas nécessairement être parfaite, mais les lignes `@</tr><tr>`, `@:<td>`, `@:</td>` et `@:</tr>` doivent chacune être sur une seule distincte comme indiqué.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-223">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="dcf1e-224">Avec le bloc de nouveau code sélectionné, appuyez trois fois sur Tab pour aligner le nouveau code avec le code existant.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-224">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="dcf1e-225">Votez ou vérifiez l’état de ce bogue [avec ce lien](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="dcf1e-225">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="dcf1e-226">Le code précédent crée une table HTML avec trois colonnes.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-226">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="dcf1e-227">Chaque colonne a une case à cocher et une légende contenant le numéro et le titre du cours.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-227">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="dcf1e-228">Les cases à cocher ont toutes le même nom (« selectedCourses »).</span><span class="sxs-lookup"><span data-stu-id="dcf1e-228">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="dcf1e-229">L’utilisation du même nom signale au classeur de modèles qu’il doit les traiter en tant que groupe.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-229">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="dcf1e-230">L’attribut de valeur de chaque case à cocher est défini sur `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-230">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="dcf1e-231">Quand la page est publiée, le classeur de modèles transmet un tableau composé des valeurs `CourseID` correspondant uniquement aux cases à cocher sélectionnées.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-231">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="dcf1e-232">Lors de l’affichage initial des cases à cocher, les cours affectés au formateur ont des attributs cochés.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-232">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="dcf1e-233">Exécutez l’application et testez la page Edit des formateurs mise à jour.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-233">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="dcf1e-234">Changez certaines affectations de cours.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-234">Change some course assignments.</span></span> <span data-ttu-id="dcf1e-235">Les modifications sont répercutées dans la page Index.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-235">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="dcf1e-236">Remarque : L’approche adoptée ici pour modifier les données de cours des formateurs fonctionne bien quand il y a un nombre limité de cours.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-236">Note: The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="dcf1e-237">Pour les collections qui sont beaucoup plus grandes, une interface utilisateur différente et une autre méthode de mise à jour seraient plus utiles et plus efficaces.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-237">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="dcf1e-238">Mise à jour de la page de création des instructeurs</span><span class="sxs-lookup"><span data-stu-id="dcf1e-238">Update the instructors Create page</span></span>

<span data-ttu-id="dcf1e-239">Mettez à jour le modèle de page de création de formateur avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-239">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="dcf1e-240">Le code précédent est similaire au code de *Pages/Instructors/Edit.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-240">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="dcf1e-241">Mettez à jour la page Razor de création de formateur avec le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-241">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="dcf1e-242">Testez la page de création de l'instructeur.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-242">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="dcf1e-243">Mettre à jour la page Delete</span><span class="sxs-lookup"><span data-stu-id="dcf1e-243">Update the Delete page</span></span>

<span data-ttu-id="dcf1e-244">Mettez à jour le modèle de page de suppression avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-244">Update the Delete page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-)]

<span data-ttu-id="dcf1e-245">Le code précédent apporte les modifications suivantes :</span><span class="sxs-lookup"><span data-stu-id="dcf1e-245">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="dcf1e-246">Utilise un chargement hâtif pour la propriété de navigation `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-246">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="dcf1e-247">Les `CourseAssignments` doivent être inclus ou ils ne seront pas supprimés lorsque l'instructeur est supprimé.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-247">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="dcf1e-248">Pour éviter d’avoir à les lire, configurez la suppression en cascade dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-248">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="dcf1e-249">Si le formateur à supprimer est attribué en tant qu’administrateur d’un département, supprime l’attribution de l'instructeur de ces départements.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-249">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="dcf1e-250">[Précédent](xref:data/ef-rp/read-related-data)
[Suivant](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="dcf1e-250">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>
