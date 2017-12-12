---
title: "Les Pages Razor avec EF Core - mettre à jour les données connexes - 7 de 8"
author: rick-anderson
description: "Dans ce didacticiel vous allez mettre à jour les données associées en mettant à jour les propriétés de navigation et les champs de clé étrangère."
keywords: "ASP.NET Core, Entity Framework Core, les données associées, jointures"
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/update-related-data
ms.openlocfilehash: f07a33c19ba1be623fae14228f8fbc909d766816
ms.sourcegitcommit: 6e46abd65973dea796d364a514de9ec2e3e1c1ed
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/02/2017
---
# <a name="updating-related-data---ef-core-razor-pages-7-of-8"></a><span data-ttu-id="4fd0b-104">Mise à jour des données connexes - Pages Razor de noyaux EF (7 sur 8)</span><span class="sxs-lookup"><span data-stu-id="4fd0b-104">Updating related data - EF Core Razor Pages (7 of 8)</span></span>

<span data-ttu-id="4fd0b-105">Par [Tom Dykstra](https://github.com/tdykstra), et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4fd0b-105">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="4fd0b-106">Ce didacticiel illustre la mise à jour de données associées.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-106">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="4fd0b-107">Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, téléchargez le [application terminée pour cette étape](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span><span class="sxs-lookup"><span data-stu-id="4fd0b-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span></span>

<span data-ttu-id="4fd0b-108">Les illustrations suivantes montrent certaines pages terminées.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-108">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="4fd0b-109">![Page de modification du cours](update-related-data/_static/course-edit.png)
![formateur modifier la page](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="4fd0b-109">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="4fd0b-110">Examinez et testez les pages de cours créer et modifier.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-110">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="4fd0b-111">Créer un nouveau cours.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-111">Create a new course.</span></span> <span data-ttu-id="4fd0b-112">Le service est activée par sa clé primaire (entier), pas son nom.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-112">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="4fd0b-113">Modifiez le nouveau cours.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-113">Edit the new course.</span></span> <span data-ttu-id="4fd0b-114">Lorsque vous avez terminé le test, supprimez le cours de nouveau.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-114">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="4fd0b-115">Créer une classe de base pour partager le code commun</span><span class="sxs-lookup"><span data-stu-id="4fd0b-115">Create a base class to share common code</span></span>

<span data-ttu-id="4fd0b-116">Les pages cours/Create et cours/modifier besoin d’une liste de noms de service.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-116">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="4fd0b-117">Créer le *Pages/Courses/DepartmentNamePageModel.cshtml.cs* classe de base pour les pages créer et modifier :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-117">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="4fd0b-118">Le code précédent crée un [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) pour contenir la liste des noms de service.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-118">The preceding code creates a [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="4fd0b-119">Si `selectedDepartment` est spécifié, ce service est sélectionné dans le `SelectList`.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-119">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="4fd0b-120">Les classes de modèle de page de créer et modifier doit dériver `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-120">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="4fd0b-121">Personnaliser les Pages de cours</span><span class="sxs-lookup"><span data-stu-id="4fd0b-121">Customize the Courses Pages</span></span>

<span data-ttu-id="4fd0b-122">Lorsqu’une nouvelle entité de cours est créée, il doit avoir une relation à un service existant.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-122">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="4fd0b-123">Pour ajouter un service lors de la création d’un cours, la classe de base pour créer et modifier contient une liste déroulante pour sélectionner le service.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-123">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="4fd0b-124">Les jeux de liste déroulante, le `Course.DepartmentID` propriété de clé étrangère (FK).</span><span class="sxs-lookup"><span data-stu-id="4fd0b-124">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="4fd0b-125">EF Core utilise le `Course.DepartmentID` FK pour charger le `Department` propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-125">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![Créer des cours](update-related-data/_static/ddl.png)

<span data-ttu-id="4fd0b-127">Mettre à jour le modèle de la page Créer avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-127">Update the Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-)]

<span data-ttu-id="4fd0b-128">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-128">The preceding code:</span></span>

* <span data-ttu-id="4fd0b-129">Dérive de `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-129">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="4fd0b-130">Utilise `TryUpdateModelAsync` pour empêcher [sur-validation](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="4fd0b-130">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="4fd0b-131">Remplace `ViewData["DepartmentID"]` avec `DepartmentNameSL` (à partir de la classe de base).</span><span class="sxs-lookup"><span data-stu-id="4fd0b-131">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="4fd0b-132">`ViewData["DepartmentID"]`est remplacé par fortement typé `DepartmentNameSL`.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-132">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="4fd0b-133">Modèles fortement typés sont plutôt que faiblement typé.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-133">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="4fd0b-134">Pour plus d’informations, consultez [faiblement typé (ViewData et ViewBag) de données](xref:mvc/views/overview#VD_VB).</span><span class="sxs-lookup"><span data-stu-id="4fd0b-134">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="4fd0b-135">Mise à jour de la page de création de cours</span><span class="sxs-lookup"><span data-stu-id="4fd0b-135">Update the Courses Create page</span></span>

<span data-ttu-id="4fd0b-136">Mise à jour *Pages/Courses/Create.cshtml* par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-136">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="4fd0b-137">Le balisage précédent apporte les modifications suivantes :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-137">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="4fd0b-138">Modifier la légende de **DepartmentID** à **service**.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-138">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="4fd0b-139">Remplace `"ViewBag.DepartmentID"` avec `DepartmentNameSL` (à partir de la classe de base).</span><span class="sxs-lookup"><span data-stu-id="4fd0b-139">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="4fd0b-140">Ajoute l’option « Sélectionner département ».</span><span class="sxs-lookup"><span data-stu-id="4fd0b-140">Adds the "Select Department" option.</span></span> <span data-ttu-id="4fd0b-141">Cette modification rend « Sélectionnez département » plutôt que le premier service.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-141">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="4fd0b-142">Ajoute un message de validation lorsque le service n’est pas sélectionné.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-142">Adds a validation message when the department is not selected.</span></span>

<span data-ttu-id="4fd0b-143">La Page Razor utilise le [sélectionner une assistance de balise](xref:mvc/views/working-with-forms#the-select-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="4fd0b-143">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="4fd0b-144">Tester la page de création.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-144">Test the Create page.</span></span> <span data-ttu-id="4fd0b-145">La page Créer affiche le nom du service plutôt que l’ID de service.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-145">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="4fd0b-146">Mettre à jour de la page Modifier le cours.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-146">Update the Courses Edit page.</span></span>

<span data-ttu-id="4fd0b-147">Mettre à jour le modèle de page de modification avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-147">Update the edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-)]

<span data-ttu-id="4fd0b-148">Les modifications sont semblables à celles effectuées dans le modèle de page de création.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-148">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="4fd0b-149">Dans le code précédent, `PopulateDepartmentsDropDownList` transmet l’ID de service, ce qui permet de sélectionner le département spécifié dans la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-149">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="4fd0b-150">Mise à jour *Pages/Courses/Edit.cshtml* par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-150">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="4fd0b-151">Le balisage précédent apporte les modifications suivantes :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-151">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="4fd0b-152">Affiche l’identificateur du cours.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-152">Displays the course ID.</span></span> <span data-ttu-id="4fd0b-153">En général la clé primaire (PK) d’une entité n’est pas affichée.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-153">Generally the Primary Key (PK) of an entity is not displayed.</span></span> <span data-ttu-id="4fd0b-154">Clés primaires sont généralement sans signification pour les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-154">PKs are usually meaningless to users.</span></span> <span data-ttu-id="4fd0b-155">Dans ce cas, la clé est le nombre de cours.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-155">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="4fd0b-156">Modifier la légende de **DepartmentID** à **service**.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-156">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="4fd0b-157">Remplace `"ViewBag.DepartmentID"` avec `DepartmentNameSL` (à partir de la classe de base).</span><span class="sxs-lookup"><span data-stu-id="4fd0b-157">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="4fd0b-158">Ajoute l’option « Sélectionner département ».</span><span class="sxs-lookup"><span data-stu-id="4fd0b-158">Adds the "Select Department" option.</span></span> <span data-ttu-id="4fd0b-159">Cette modification rend « Sélectionnez département » plutôt que le premier service.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-159">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="4fd0b-160">Ajoute un message de validation lorsque le service n’est pas sélectionné.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-160">Adds a validation message when the department is not selected.</span></span>

<span data-ttu-id="4fd0b-161">La page contient un champ masqué (`<input type="hidden">`) pour le nombre de cours.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-161">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="4fd0b-162">Ajout d’un `<label>` d’assistance avec la balise `asp-for="Course.CourseID"` n’élimine la nécessité pour le champ masqué.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-162">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="4fd0b-163">`<input type="hidden">`est requis pour le nombre de cours à inclure dans les données publiées lorsque l’utilisateur clique sur **enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-163">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="4fd0b-164">Tester le code mis à jour.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-164">Test the updated code.</span></span> <span data-ttu-id="4fd0b-165">Créer, modifier et supprimer un cours.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-165">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="4fd0b-166">Ajouter AsNoTracking les détails et supprimer des modèles de page</span><span class="sxs-lookup"><span data-stu-id="4fd0b-166">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="4fd0b-167">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) peut améliorer les performances lorsque le suivi n’est pas obligatoire.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-167">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking is not required.</span></span> <span data-ttu-id="4fd0b-168">Ajouter `AsNoTracking` au modèle de page de suppression et de détails.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-168">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="4fd0b-169">Le code suivant montre le modèle mis à jour de la page Supprimer :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-169">The following code shows the updated Delete page model:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="4fd0b-170">Mise à jour la `OnGetAsync` méthode dans le *Pages/Courses/Details.cshtml.cs* fichier :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-170">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="4fd0b-171">Modifier les pages Delete et détails</span><span class="sxs-lookup"><span data-stu-id="4fd0b-171">Modify the Delete and Details pages</span></span>

<span data-ttu-id="4fd0b-172">Mise à jour de la page Supprimer un Razor avec le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-172">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="4fd0b-173">Apporter les mêmes modifications à la page de détails.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-173">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="4fd0b-174">Tester les pages de cours</span><span class="sxs-lookup"><span data-stu-id="4fd0b-174">Test the Course pages</span></span>

<span data-ttu-id="4fd0b-175">Test de créer, modifier, détails et supprimer.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-175">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="4fd0b-176">Mettre à jour les pages de formateur</span><span class="sxs-lookup"><span data-stu-id="4fd0b-176">Update the instructor pages</span></span>

<span data-ttu-id="4fd0b-177">Les sections suivantes mettre à jour les pages de formateur.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-177">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="4fd0b-178">Ajouter l’emplacement du bureau</span><span class="sxs-lookup"><span data-stu-id="4fd0b-178">Add office location</span></span>

<span data-ttu-id="4fd0b-179">Lorsque vous modifiez un enregistrement du formateur, vous pouvez mettre à jour l’attribution du formateur office.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-179">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="4fd0b-180">Le `Instructor` entité a une relation un-à-zéro-ou-un avec le `OfficeAssignment` entité.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-180">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="4fd0b-181">Le code de formateur doit gérer :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-181">The instructor code must handle:</span></span>

* <span data-ttu-id="4fd0b-182">Si l’utilisateur efface l’attribution d’office, supprimez le `OfficeAssignment` entité.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-182">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="4fd0b-183">Si l’utilisateur entre une attribution et il était vide, créez un nouveau `OfficeAssignment` entité.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-183">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="4fd0b-184">Si l’utilisateur modifie l’affectation d’office, mettre à jour le `OfficeAssignment` entité.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-184">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="4fd0b-185">Mettre à jour le modèle de page instructeurs modifier avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-185">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-)]

<span data-ttu-id="4fd0b-186">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-186">The preceding code:</span></span>

- <span data-ttu-id="4fd0b-187">Obtient les valeurs combinées `Instructor` entité à partir de la base de données à l’aide d’un chargement hâtif pour le `OfficeAssignment` propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-187">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
- <span data-ttu-id="4fd0b-188">Met à jour récupérées `Instructor` entité avec les valeurs de classeur de modèles.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-188">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="4fd0b-189">`TryUpdateModel`empêche [sur-validation](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="4fd0b-189">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
- <span data-ttu-id="4fd0b-190">Si l’emplacement du bureau est vide, définit `Instructor.OfficeAssignment` avec la valeur null.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-190">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="4fd0b-191">Lorsque `Instructor.OfficeAssignment` est null, la ligne correspondante dans la `OfficeAssignment` table est supprimée.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-191">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="4fd0b-192">Mettre à jour de la page Modifier le formateur</span><span class="sxs-lookup"><span data-stu-id="4fd0b-192">Update the instructor Edit page</span></span>

<span data-ttu-id="4fd0b-193">Mise à jour *Pages/Instructors/Edit.cshtml* avec l’emplacement du bureau :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-193">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="4fd0b-194">Vérifiez que vous pouvez modifier un emplacement office de formateurs.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-194">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="4fd0b-195">Ajouter des affectations de cours à la page Modifier le formateur</span><span class="sxs-lookup"><span data-stu-id="4fd0b-195">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="4fd0b-196">Instructeurs enseigner à n’importe quel nombre de cours.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-196">Instructors may teach any number of courses.</span></span> <span data-ttu-id="4fd0b-197">Dans cette section, vous ajoutez la possibilité de modifier les affectations de cours.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-197">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="4fd0b-198">L’illustration suivante montre le formateur de mise à jour de page de modification :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-198">The following image shows the updated instructor Edit page:</span></span>

![Page de modification du formateur en cours](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="4fd0b-200">`Course`et `Instructor` a une relation plusieurs-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-200">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="4fd0b-201">Pour ajouter et supprimer des relations, vous ajoutez et supprimez des entités à partir de la `CourseAssignments` joindre le jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-201">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="4fd0b-202">Cases à cocher Activer les modifications aux cours à que formateur est assigné.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-202">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="4fd0b-203">Une case à cocher s’affiche pour chaque cours dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-203">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="4fd0b-204">Cours auquel appartient le formateur sont vérifiées.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-204">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="4fd0b-205">L’utilisateur peut sélectionner ou désactivez les cases à cocher pour modifier les affectations de cours.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-205">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="4fd0b-206">Si le nombre de cours ont été nettement supérieur :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-206">If the number of courses were much greater:</span></span>

* <span data-ttu-id="4fd0b-207">Vous devrez sans doute utiliser une interface utilisateur différente pour afficher les cours.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-207">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="4fd0b-208">La méthode de manipulation d’une entité de jointure pour créer ou supprimer des relations ne modifie pas.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-208">The method of manipulating a join entity to create or delete relationships would not change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="4fd0b-209">Ajouter des classes pour prendre en charge créer et modifier des pages de formateur</span><span class="sxs-lookup"><span data-stu-id="4fd0b-209">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="4fd0b-210">Créer *SchoolViewModels/AssignedCourseData.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-210">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="4fd0b-211">La `AssignedCourseData` classe contient des données pour créer les cases à cocher pour les cours attribués par un formateur.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-211">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="4fd0b-212">Créer le *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* classe de base :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-212">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="4fd0b-213">Le `InstructorCoursesPageModel` est la classe de base utilisée pour la modifier et créer des modèles de page.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-213">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="4fd0b-214">`PopulateAssignedCourseData`lit tous les `Course` entités pour remplir `AssignedCourseDataList`.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-214">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="4fd0b-215">Pour chaque cours, le code définit la `CourseID`, titre et s’il faut ou non le formateur est affecté au cours.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-215">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="4fd0b-216">A [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) est utilisé pour créer des recherches efficaces.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-216">A [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="4fd0b-217">Modifier le modèle de page instructeurs</span><span class="sxs-lookup"><span data-stu-id="4fd0b-217">Instructors Edit page model</span></span>

<span data-ttu-id="4fd0b-218">Mettre à jour le modèle de page de modification de formateur par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-218">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-)]

<span data-ttu-id="4fd0b-219">Le code précédent gère les changements d’affectation d’office.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-219">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="4fd0b-220">Mettre à jour la vue Razor formateur :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-220">Update the instructor Razor View:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="4fd0b-221">Lorsque vous collez le code dans Visual Studio, les sauts de ligne sont modifiées d’une manière qui interrompt le code.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-221">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="4fd0b-222">Appuyez une fois sur Ctrl + Z pour annuler la mise en forme automatique.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-222">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="4fd0b-223">CTRL + Z résout les sauts de ligne afin qu’elles apparaîtront comme vous le voyez ici.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-223">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="4fd0b-224">La mise en retrait ne doit pas nécessairement être parfait, mais la `@</tr><tr>`, `@:<td>`, `@:</td>`, et `@:</tr>` lignes doivent chacune être sur une seule ligne comme indiqué.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-224">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="4fd0b-225">Avec le bloc de code nouveau sélectionné, appuyez sur Tab trois fois pour aligner le nouveau code avec le code existant.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-225">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="4fd0b-226">Voter pour ou vérifier l’état de ce bogue [ce lien](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="4fd0b-226">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="4fd0b-227">Le code précédent crée une table HTML qui possède trois colonnes.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-227">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="4fd0b-228">Chaque colonne possède une case à cocher et une légende contenant le numéro et le titre.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-228">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="4fd0b-229">Toutes les cases à cocher ont le même nom (« selectedCourses »).</span><span class="sxs-lookup"><span data-stu-id="4fd0b-229">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="4fd0b-230">En utilisant le même nom informe le classeur de modèles pour les traiter en tant que groupe.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-230">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="4fd0b-231">L’attribut value de chaque case à cocher a la valeur `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-231">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="4fd0b-232">Lorsque la page est publiée, le classeur de modèles passe un tableau comprenant le `CourseID` valeurs pour les cases à cocher sont sélectionnées.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-232">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="4fd0b-233">Lorsque les cases à cocher sont restitués initialement, cours affectés pour le formateur archivées par attributs.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-233">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="4fd0b-234">Exécutez l’application et tester la page de modification des instructeurs mis à jour.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-234">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="4fd0b-235">Modifier des attributions de cours.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-235">Change some course assignments.</span></span> <span data-ttu-id="4fd0b-236">Les modifications sont répercutées sur la page d’Index.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-236">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="4fd0b-237">Remarque : L’approche adoptée ici pour modifier les données de cours formateur fonctionne bien quand un nombre limité de cours.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-237">Note: The approach taken here to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="4fd0b-238">Pour les collections qui sont beaucoup plus volumineux, une interface utilisateur différente et une autre méthode de mise à jour serait plus utile et plus efficace.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-238">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="4fd0b-239">Mise à jour de la page de création de formateurs</span><span class="sxs-lookup"><span data-stu-id="4fd0b-239">Update the instructors Create page</span></span>

<span data-ttu-id="4fd0b-240">Mettre à jour le modèle de page de création de formateur par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-240">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="4fd0b-241">Le code précédent est similaire à la *Pages/Instructors/Edit.cshtml.cs* code.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-241">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="4fd0b-242">Mise à jour de la page de création de Razor formateur par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-242">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="4fd0b-243">Tester la page de création de formateur.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-243">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="4fd0b-244">Mise à jour de la page de suppression</span><span class="sxs-lookup"><span data-stu-id="4fd0b-244">Update the Delete page</span></span>

<span data-ttu-id="4fd0b-245">Mettre à jour le modèle de page de suppression par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-245">Update the Delete page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-)]

<span data-ttu-id="4fd0b-246">Le code précédent apporte les modifications suivantes :</span><span class="sxs-lookup"><span data-stu-id="4fd0b-246">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="4fd0b-247">Utilise un chargement hâtif pour le `CourseAssignments` propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-247">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="4fd0b-248">`CourseAssignments`doit être inclus ou ils ne sont pas supprimés lorsque le formateur est supprimé.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-248">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="4fd0b-249">Pour éviter d’avoir à les lire, configurez la suppression en cascade dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-249">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="4fd0b-250">Si le formateur à supprimer est attribué en tant qu’administrateur d’un service, supprime l’attribution de formateur de ces services.</span><span class="sxs-lookup"><span data-stu-id="4fd0b-250">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="4fd0b-251">[Précédent](xref:data/ef-rp/read-related-data)
[Suivant](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="4fd0b-251">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>