---
title: Pages Razor avec EF Core dans ASP.NET Core - Mise à jour de données associées - 7 sur 8
author: rick-anderson
description: Dans ce didacticiel, vous allez mettre à jour des données associées en mettant à jour des propriétés de navigation et des champs de clé étrangère.
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/update-related-data
ms.openlocfilehash: e987971f60e5c5a9fb79e30440c7c986df64447e
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38189300"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---update-related-data---7-of-8"></a>Pages Razor avec EF Core dans ASP.NET Core - Mise à jour de données associées - 7 sur 8

De [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

Ce didacticiel illustre la mise à jour de données associées. Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, téléchargez l’[application terminée pour cette phase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).

Les illustrations suivantes montrent certaines des pages terminées.

![Page de modification de cours](update-related-data/_static/course-edit.png)
![Page de modification de formateur](update-related-data/_static/instructor-edit-courses.png)

Examinez et testez les pages des cours Create et Edit. Créez un nouveau cours. Le service est sélectionné par sa clé primaire (entier), pas par son nom. Modifiez le nouveau cours. Lorsque vous avez terminé le test, supprimez le nouveau cours.

## <a name="create-a-base-class-to-share-common-code"></a>Créer une classe de base pour partager le code commun

Les pages de création et de modification de cours ont chacune besoin d’une liste de noms de départements. Créez la classe de base *Pages/Courses/DepartmentNamePageModel.cshtml.cs* pour les pages Create et Edit :

[!code-csharp[](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

Le code précédent crée un [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) pour contenir la liste des noms de département.  Si `selectedDepartment` est spécifié, ce département est sélectionné dans le `SelectList`. 

Les classes de modèle de page Create et Edit doivent dériver de `DepartmentNamePageModel`.

## <a name="customize-the-courses-pages"></a>Personnaliser les pages de cours

Quand une entité de cours est créée, elle doit avoir une relation à un département existant. Pour ajouter un département lors de la création d’un cours, la classe de base pour Create et Edit contient une liste déroulante de sélection du département. La liste déroulante définit la propriété de clé étrangère `Course.DepartmentID`. EF Core utilise la clé étrangère `Course.DepartmentID` pour charger la propriété de navigation `Department`.

![Créer le cours](update-related-data/_static/ddl.png)

Mettez à jour le modèle de la page Create avec le code suivant : 

[!code-csharp[](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-999)]

Le code précédent :

* Dérive de `DepartmentNamePageModel`.
* Utilise `TryUpdateModelAsync` pour empêcher la [sur-validation](xref:data/ef-rp/crud#overposting).
* Remplace `ViewData["DepartmentID"]` par `DepartmentNameSL` (à partir de la classe de base).

`ViewData["DepartmentID"]` est remplacé par le `DepartmentNameSL` fortement typé. Les modèles fortement typés sont préférables aux modèles faiblement typés. Pour plus d’informations, consultez [Données faiblement typées (ViewData et ViewBag)](xref:mvc/views/overview#VD_VB).

### <a name="update-the-courses-create-page"></a>Mettre à jour la page de création de cours

Mettez à jour *Pages/Courses/Create.cshtml* avec le balisage suivant :

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

Le balisage précédent apporte les modifications suivantes :

* Modifie la légende de **DepartmentID** en **Department**. 
* Il remplace `"ViewBag.DepartmentID"` par `DepartmentNameSL` (à partir de la classe de base).
* Il ajoute l’option « Select Department ». Cette modification entraîne l’affichage de « Select Department » plutôt que du premier département.
* Il ajoute un message de validation quand le département n’est pas sélectionné.

La Page Razor utilise le [Tag Helper Select](xref:mvc/views/working-with-forms#the-select-tag-helper) :

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

Testez la page Create. La page Create affiche le nom du département plutôt que son ID.

### <a name="update-the-courses-edit-page"></a>Mise à jour de la page Edit d'un cours.

Mettez à jour le modèle de modification de cours avec le code suivant :

[!code-csharp[](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-999)]

Les modifications sont semblables à celles effectuées dans le modèle de page Create. Dans le code précédent, `PopulateDepartmentsDropDownList` transmet l’ID de département, ce qui sélectionne le département spécifié dans la liste déroulante.

Mettez à jour *Pages/Courses/Edit.cshtml* avec le balisage suivant :

[!code-cshtml[](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

Le balisage précédent apporte les modifications suivantes :

* Affiche l’identificateur du cours. En général la clé primaire (PK) d’une entité n’est pas affichée. Les clés primaires sont généralement sans signification pour les utilisateurs. Dans ce cas, la clé est le numéro de cours.
* Modifie la légende de **DepartmentID** en **Department**. 
* Il remplace `"ViewBag.DepartmentID"` par `DepartmentNameSL` (à partir de la classe de base).

La page contient un champ masqué (`<input type="hidden">`) pour le numéro de cours. L’ajout d’un Tag Helper `<label>` avec `asp-for="Course.CourseID"` n’élimine pas la nécessité de la présence du champ masqué. `<input type="hidden">` est obligatoire pour que le numéro de cours soit inclus dans les données publiées quand l’utilisateur clique sur **Save**.

Testez le code mis à jour. Créez, modifiez et supprimez un cours.

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a>Ajouter AsNoTracking aux modèles de page Details et Delete

[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) peut améliorer les performances quand le suivi n’est pas nécessaire. Ajoutez `AsNoTracking` dans les modèles de page Details et Delete. Le code suivant montre le modèle de page Delete mis à jour :

[!code-csharp[](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

Mettez à jour la méthode `OnGetAsync` dans le fichier *Pages/Courses/Details.cshtml.cs* :

[!code-csharp[](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a>Modifier les pages Delete et Details

Mettez à jour la page Razor Delete avec le balisage suivant :

[!code-cshtml[](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

Apportez les mêmes modifications à la page Details.

### <a name="test-the-course-pages"></a>Tester les pages de cours

Testez les fonctionnalités de création, de modification, de détails et de suppression.

## <a name="update-the-instructor-pages"></a>Mettre à jour les pages d'instructeur

Les sections suivantes mettent à jour les pages d'instructeur.

### <a name="add-office-location"></a>Ajouter l’emplacement du bureau

Lors de la modification d’un enregistrement de formateur, vous souhaiterez peut-être mettre à jour l’attribution de bureau du formateur. L’entité `Instructor` a une relation un-à-zéro-ou-un avec l’entité `OfficeAssignment`. Le code de formateur doit gérer ce qui suit :

* Si l’utilisateur efface l’attribution d'un bureau, supprimer l'entité `OfficeAssignment`. 
* Si l’utilisateur entre une attribution et qu'elle était vide, créer une nouvelle entité `OfficeAssignment`. 
* Si l’utilisateur change l’attribution de bureau, mettre à jour l’entité `OfficeAssignment`.

Mettez à jour le modèle de page Edit d'instructeur avec le code suivant :

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-999)]

Le code précédent :

- Obtient l'entité `Instructor` en cours à partir de la base de données à l’aide d’un chargement hâtif de la propriété de navigation `OfficeAssignment`.
- Met à jour l’entité `Instructor` récupérée avec les valeurs du classeur de modèles. `TryUpdateModel` empêche la [survalidation](xref:data/ef-rp/crud#overposting).
- Si l’emplacement du bureau est vide, définit `Instructor.OfficeAssignment` avec la valeur null. Lorsque `Instructor.OfficeAssignment` est null, la ligne correspondante dans la table `OfficeAssignment` est supprimée.

### <a name="update-the-instructor-edit-page"></a>Mettre à jour la page de modification de formateur

Mettez à jour *Pages/Instructors/Edit.cshtml* avec l’emplacement du bureau :

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

Vérifiez que vous pouvez modifier l'emplacement de bureau d'un instructeur.

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>Ajouter des affectations de cours à la page Edit de l'instructeur

Les instructeurs peuvent enseigner dans n’importe quel nombre de cours. Dans cette section, vous ajoutez la possibilité de modifier les affectations de cours. L’illustration suivante montre la page de modification de l'instructeur mise à jour :

![Page de modification de formateur avec des cours](update-related-data/_static/instructor-edit-courses.png)

`Course` et `Instructor` entretiennent une relation plusieurs-à-plusieurs. Pour ajouter et supprimer des relations, vous ajoutez et supprimez des entités à partir du jeu d’entités de jointures `CourseAssignments`.

Les cases à cocher permettent de changer les cours auxquels un formateur est assigné. Une case à cocher est affichée pour chaque cours dans la base de données. Les cours auxquels le formateur est affecté sont cochés. L’utilisateur peut cocher ou décocher les cases pour modifier les affectations de cours. Si le nombre de cours était beaucoup plus élevé :

* Vous utiliseriez sans doute une interface utilisateur différente pour afficher les cours.
* La méthode de manipulation d’une entité de jointure pour créer ou supprimer des relations ne changerait pas.

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a>Ajouter des classes pour prendre en charge Create et Edit des pages d'instructeur

Créez *SchoolViewModels/AssignedCourseData.cs* avec le code suivant :

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

La classe `AssignedCourseData` contient des données pour créer les cases à cocher pour les cours attribués par un formateur.

Créez la classe de base *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* :

[!code-csharp[](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

`InstructorCoursesPageModel` est la classe de base que vous utiliserez pour les modèles de page Edit et Create. `PopulateAssignedCourseData` lit toutes les entités `Course` pour renseigner `AssignedCourseDataList`. Pour chaque cours, le code définit le `CourseID`, le titre, et si le formateur est affecté au cours. Un [HashSet](/dotnet/api/system.collections.generic.hashset-1) est utilisé pour créer des recherches efficaces.

### <a name="instructors-edit-page-model"></a>Modèle de page de modification de formateur

Mettez à jour le modèle de page Edit d'instructeur avec le code suivant :

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-999)]

Le code précédent gère les changements d’affectation de bureau.

Mettez à jour la vue Razor Instructeur :

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> Quand vous collez le code dans Visual Studio, les sauts de ligne sont modifiés de telle manière que le code est rompu. Appuyez une fois sur Ctrl + Z pour annuler la mise en forme automatique. Ctrl + Z corrige les sauts de ligne afin qu’ils aient l’aspect visible ici. La mise en retrait ne doit pas nécessairement être parfaite, mais les lignes `@</tr><tr>`, `@:<td>`, `@:</td>` et `@:</tr>` doivent chacune être sur une seule distincte comme indiqué. Avec le bloc de nouveau code sélectionné, appuyez trois fois sur Tab pour aligner le nouveau code avec le code existant. Votez ou vérifiez l’état de ce bogue [avec ce lien](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).

Le code précédent crée une table HTML avec trois colonnes. Chaque colonne a une case à cocher et une légende contenant le numéro et le titre du cours. Les cases à cocher ont toutes le même nom (« selectedCourses »). L’utilisation du même nom signale au classeur de modèles qu’il doit les traiter en tant que groupe. L’attribut de valeur de chaque case à cocher est défini sur `CourseID`. Quand la page est publiée, le classeur de modèles transmet un tableau composé des valeurs `CourseID` correspondant uniquement aux cases à cocher sélectionnées.

Lors de l’affichage initial des cases à cocher, les cours affectés au formateur ont des attributs cochés.

Exécutez l’application et testez la page Edit des formateurs mise à jour. Changez certaines affectations de cours. Les modifications sont répercutées dans la page Index.

Remarque : L’approche adoptée ici pour modifier les données de cours des formateurs fonctionne bien quand il y a un nombre limité de cours. Pour les collections qui sont beaucoup plus grandes, une interface utilisateur différente et une autre méthode de mise à jour seraient plus utiles et plus efficaces.

### <a name="update-the-instructors-create-page"></a>Mise à jour de la page de création des instructeurs

Mettez à jour le modèle de page de création de formateur avec le code suivant :

[!code-csharp[](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

Le code précédent est similaire au code de *Pages/Instructors/Edit.cshtml.cs*.

Mettez à jour la page Razor de création de formateur avec le balisage suivant :

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

Testez la page de création de l'instructeur.

## <a name="update-the-delete-page"></a>Mettre à jour la page Delete

Mettez à jour le modèle de page de suppression avec le code suivant :

[!code-csharp[](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-999)]

Le code précédent apporte les modifications suivantes :

* Utilise un chargement hâtif pour la propriété de navigation `CourseAssignments`. Les `CourseAssignments` doivent être inclus ou ils ne seront pas supprimés lorsque l'instructeur est supprimé. Pour éviter d’avoir à les lire, configurez la suppression en cascade dans la base de données.

* Si le formateur à supprimer est attribué en tant qu’administrateur d’un département, supprime l’attribution de l'instructeur de ces départements.

> [!div class="step-by-step"]
> [Précédent](xref:data/ef-rp/read-related-data)
> [Suivant](xref:data/ef-rp/concurrency)
