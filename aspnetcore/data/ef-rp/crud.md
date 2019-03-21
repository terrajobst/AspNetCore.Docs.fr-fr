---
title: Pages Razor avec EF Core dans ASP.NET Core - CRUD - 2 sur 8
author: rick-anderson
description: Montre comment créer, lire, mettre à jour et supprimer avec EF Core
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/crud
ms.openlocfilehash: c104498a4411ac36672ffbb9660948d4e96bf1ee
ms.sourcegitcommit: 10e14b85490f064395e9b2f423d21e3c2d39ed8b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58142389"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---crud---2-of-8"></a><span data-ttu-id="49657-103">Pages Razor avec EF Core dans ASP.NET Core - CRUD - 2 sur 8</span><span class="sxs-lookup"><span data-stu-id="49657-103">Razor Pages with EF Core in ASP.NET Core - CRUD - 2 of 8</span></span>

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="49657-104">Par [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="49657-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

<span data-ttu-id="49657-105">Dans ce didacticiel, nous allons examiner et personnaliser le code CRUD (créer, lire, mettre à jour, supprimer) généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="49657-105">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="49657-106">Pour que ces tutoriels soient moins complexes et traitent exclusivement d’EF Core, nous avons utilisé le code EF Core dans les modèles de page.</span><span class="sxs-lookup"><span data-stu-id="49657-106">To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the page models.</span></span> <span data-ttu-id="49657-107">Certains développeurs utilisent un modèle de référentiel ou de couche de service pour créer une couche d’abstraction entre l’interface utilisateur (Pages Razor) et la couche d’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="49657-107">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="49657-108">Dans ce tutoriel, nous examinons les pages Create, Edit, Delete et Details de Razor Pages, qui sont dans le dossier *Students*.</span><span class="sxs-lookup"><span data-stu-id="49657-108">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Students* folder are examined.</span></span>

<span data-ttu-id="49657-109">Le code généré automatiquement utilise le modèle suivant pour les pages Create, Edit et Delete :</span><span class="sxs-lookup"><span data-stu-id="49657-109">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="49657-110">Obtenir et afficher les données demandées avec la méthode HTTP GET `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="49657-110">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="49657-111">Enregistrer les modifications dans les données avec la méthode HTTP POST `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="49657-111">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="49657-112">Les pages Index et Details obtiennent et affichent les données demandées avec la méthode HTTP GET `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="49657-112">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="singleordefaultasync-vs-firstordefaultasync"></a><span data-ttu-id="49657-113">SingleOrDefaultAsync vs. FirstOrDefaultAsync</span><span class="sxs-lookup"><span data-stu-id="49657-113">SingleOrDefaultAsync vs. FirstOrDefaultAsync</span></span>

<span data-ttu-id="49657-114">Le code généré utilise [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_), qui est généralement préféré à [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="49657-114">The generated code uses [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_), which is generally preferred over [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>

 <span data-ttu-id="49657-115">`FirstOrDefaultAsync` est plus efficace que `SingleOrDefaultAsync` pour récupérer une entité :</span><span class="sxs-lookup"><span data-stu-id="49657-115">`FirstOrDefaultAsync` is more efficient than `SingleOrDefaultAsync` at fetching one entity:</span></span>

* <span data-ttu-id="49657-116">Sauf si le code doit vérifier que la requête ne retourne pas plusieurs entités.</span><span class="sxs-lookup"><span data-stu-id="49657-116">Unless the code needs to verify that there's not more than one entity returned from the query.</span></span>
* <span data-ttu-id="49657-117">`SingleOrDefaultAsync` récupère davantage de données et effectue un travail inutile.</span><span class="sxs-lookup"><span data-stu-id="49657-117">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="49657-118">`SingleOrDefaultAsync` lève une exception si plusieurs entités correspondent à la partie filtre.</span><span class="sxs-lookup"><span data-stu-id="49657-118">`SingleOrDefaultAsync` throws an exception if there's more than one entity that fits the filter part.</span></span>
* <span data-ttu-id="49657-119">`FirstOrDefaultAsync` ne lève pas d’exception si plusieurs entités correspondent à la partie filtre.</span><span class="sxs-lookup"><span data-stu-id="49657-119">`FirstOrDefaultAsync` doesn't throw if there's more than one entity that fits the filter part.</span></span>

<a name="FindAsync"></a>

### <a name="findasync"></a><span data-ttu-id="49657-120">FindAsync</span><span class="sxs-lookup"><span data-stu-id="49657-120">FindAsync</span></span>

<span data-ttu-id="49657-121">Dans une grande partie du code généré automatiquement, vous pouvez utiliser [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) à la place de `FirstOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="49657-121">In much of the scaffolded code, [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync`.</span></span>

<span data-ttu-id="49657-122">`FindAsync`:</span><span class="sxs-lookup"><span data-stu-id="49657-122">`FindAsync`:</span></span>

* <span data-ttu-id="49657-123">Recherche une entité avec la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="49657-123">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="49657-124">Si une entité avec la clé primaire est suivie par le contexte, elle est retournée sans qu’une requête soit envoyée à la base de données.</span><span class="sxs-lookup"><span data-stu-id="49657-124">If an entity with the PK is being tracked by the context, it's returned without a request to the DB.</span></span>
* <span data-ttu-id="49657-125">Est simple et concise.</span><span class="sxs-lookup"><span data-stu-id="49657-125">Is simple and concise.</span></span>
* <span data-ttu-id="49657-126">Est optimisée pour rechercher une entité unique.</span><span class="sxs-lookup"><span data-stu-id="49657-126">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="49657-127">Peut avoir des avantages de performances dans certaines situations, mais c’est rarement le cas pour les applications web standard.</span><span class="sxs-lookup"><span data-stu-id="49657-127">Can have perf benefits in some situations, but that rarely happens for typical web apps.</span></span>
* <span data-ttu-id="49657-128">Utilise implicitement [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) au lieu de [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="49657-128">Implicitly uses [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>

<span data-ttu-id="49657-129">Toutefois, si vous voulez exécuter `Include` sur d’autres entités, `FindAsync` n’est plus approprié.</span><span class="sxs-lookup"><span data-stu-id="49657-129">But if you want to `Include` other entities, then `FindAsync` is no longer appropriate.</span></span> <span data-ttu-id="49657-130">Cela signifie que vous devez peut-être abandonner `FindAsync` et passer à une requête à mesure que votre application progresse.</span><span class="sxs-lookup"><span data-stu-id="49657-130">This means that you may need to abandon `FindAsync` and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="49657-131">Personnaliser la page Details</span><span class="sxs-lookup"><span data-stu-id="49657-131">Customize the Details page</span></span>

<span data-ttu-id="49657-132">Accédez à la page `Pages/Students`.</span><span class="sxs-lookup"><span data-stu-id="49657-132">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="49657-133">Les liens **Edit**, **Details**, et **Delete** sont générés par le [Tag helper anchor](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) dans le fichier *Pages/Student/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="49657-133">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[](intro/samples/cu21/Pages/Students/Index1.cshtml?name=snippet)]

<span data-ttu-id="49657-134">Exécutez l’application et sélectionnez un lien **Details**.</span><span class="sxs-lookup"><span data-stu-id="49657-134">Run the app and select a **Details** link.</span></span> <span data-ttu-id="49657-135">L’URL est au format `http://localhost:5000/Students/Details?id=2`.</span><span class="sxs-lookup"><span data-stu-id="49657-135">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="49657-136">L’ID d’étudiant est transmis à l’aide d’une chaîne de requête (`?id=2`).</span><span class="sxs-lookup"><span data-stu-id="49657-136">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="49657-137">Mettez à jour les Pages Razor Edit, Details et Delete pour utiliser le modèle de route `"{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="49657-137">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="49657-138">Remplacez la directive de chacune de ces pages (`@page`) par `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="49657-138">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="49657-139">Une demande à la page avec le modèle de route «{id:int}» qui n'inclut **pas** une valeur de route avec un entier retourne une erreur HTTP 404 (not found). </span><span class="sxs-lookup"><span data-stu-id="49657-139">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="49657-140">Par exemple, `http://localhost:5000/Students/Details` retourne une erreur 404.</span><span class="sxs-lookup"><span data-stu-id="49657-140">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="49657-141">Pour que l’ID soit facultatif, ajoutez `?` à la contrainte de route :</span><span class="sxs-lookup"><span data-stu-id="49657-141">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="49657-142">Exécutez l’application, cliquez sur un lien Détails et vérifiez que l’URL passe l’ID en tant que données de route (`http://localhost:5000/Students/Details/2`).</span><span class="sxs-lookup"><span data-stu-id="49657-142">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="49657-143">Ne changez pas `@page` en `@page "{id:int}"` globalement, cela casserait les liens vers les pages Home et Create.</span><span class="sxs-lookup"><span data-stu-id="49657-143">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="49657-144">Ajouter des données associées</span><span class="sxs-lookup"><span data-stu-id="49657-144">Add related data</span></span>

<span data-ttu-id="49657-145">Le code généré automatiquement pour la page Index des étudiants n’inclut pas la propriété `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="49657-145">The scaffolded code for the Students Index page doesn't include the `Enrollments` property.</span></span> <span data-ttu-id="49657-146">Dans cette section, le contenu de la collection `Enrollments` s’affiche dans la page Details.</span><span class="sxs-lookup"><span data-stu-id="49657-146">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="49657-147">Le méthode `OnGetAsync` de *Pages/Students/Details.cshtml.cs* utilise la méthode `FirstOrDefaultAsync` pour récupérer une seule entité `Student`.</span><span class="sxs-lookup"><span data-stu-id="49657-147">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="49657-148">Ajoutez le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="49657-148">Add the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="49657-149">Les méthodes [Include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) et [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) forcent le contexte à charger la propriété de navigation `Student.Enrollments` et, dans chaque inscription, la propriété de navigation `Enrollment.Course`.</span><span class="sxs-lookup"><span data-stu-id="49657-149">The [Include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) and [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="49657-150">Ces méthodes sont examinées en détail dans le tutoriel sur la lecture des données associées.</span><span class="sxs-lookup"><span data-stu-id="49657-150">These methods are examined in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="49657-151">La méthode [AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) améliore les performances dans les scénarios où les entités retournées ne sont pas mises à jour dans le contexte actuel.</span><span class="sxs-lookup"><span data-stu-id="49657-151">The [AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="49657-152">Le sujet `AsNoTracking` est abordé plus loin dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="49657-152">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="49657-153">Afficher les inscriptions associées sur la page Details</span><span class="sxs-lookup"><span data-stu-id="49657-153">Display related enrollments on the Details page</span></span>

<span data-ttu-id="49657-154">Ouvrez *Pages/Students/Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="49657-154">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="49657-155">Ajoutez le code en surbrillance suivant pour afficher la liste des inscriptions :</span><span class="sxs-lookup"><span data-stu-id="49657-155">Add the following highlighted code to display a list of enrollments:</span></span>

[!code-cshtml[](intro/samples/cu21/Pages/Students/Details.cshtml?highlight=32-53)]

<span data-ttu-id="49657-156">Si la mise en retrait du code est incorrecte, une fois que le code est collé, appuyez sur CTRL + K + D pour résoudre ce problème.</span><span class="sxs-lookup"><span data-stu-id="49657-156">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="49657-157">Le code précédent effectue une itération sur les entités dans la propriété de navigation `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="49657-157">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="49657-158">Pour chaque inscription, il affiche le titre du cours et le niveau.</span><span class="sxs-lookup"><span data-stu-id="49657-158">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="49657-159">Le titre du cours est récupéré à partir de l’entité de cours qui est stockée dans la propriété de navigation `Course` de l’entité Enrollments.</span><span class="sxs-lookup"><span data-stu-id="49657-159">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="49657-160">Exécutez l’application, sélectionnez l'onglet **Students**, puis cliquez sur le lien **Details** pour un étudiant.</span><span class="sxs-lookup"><span data-stu-id="49657-160">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="49657-161">La liste des cours et les notes de l’étudiant sélectionné s’affiche.</span><span class="sxs-lookup"><span data-stu-id="49657-161">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="49657-162">Mettre à jour la page Create</span><span class="sxs-lookup"><span data-stu-id="49657-162">Update the Create page</span></span>

<span data-ttu-id="49657-163">Mettez à jour la méthode `OnPostAsync` dans *Pages/Students/Create.cshtml.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="49657-163">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>

### <a name="tryupdatemodelasync"></a><span data-ttu-id="49657-164">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="49657-164">TryUpdateModelAsync</span></span>

<span data-ttu-id="49657-165">Examinez le code [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) :</span><span class="sxs-lookup"><span data-stu-id="49657-165">Examine the [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="49657-166">Dans le code précédent, `TryUpdateModelAsync<Student>` tente de mettre à jour l’objet `emptyStudent` en utilisant les valeurs de formulaire publiées à partir de la propriété [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) dans le [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel).</span><span class="sxs-lookup"><span data-stu-id="49657-166">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel).</span></span> <span data-ttu-id="49657-167">`TryUpdateModelAsync` met uniquement à jour les propriétés répertoriées (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span><span class="sxs-lookup"><span data-stu-id="49657-167">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="49657-168">Dans l’exemple précédent :</span><span class="sxs-lookup"><span data-stu-id="49657-168">In the preceding sample:</span></span>

* <span data-ttu-id="49657-169">Le deuxième argument (`"student", // Prefix`) est le préfixe utilisé pour rechercher des valeurs.</span><span class="sxs-lookup"><span data-stu-id="49657-169">The second argument (`"student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="49657-170">Il ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="49657-170">It's not case sensitive.</span></span>
* <span data-ttu-id="49657-171">Les valeurs de formulaire publiées sont converties vers les types dans le modèle `Student` à l’aide de la [liaison de modèle](xref:mvc/models/model-binding#how-model-binding-works).</span><span class="sxs-lookup"><span data-stu-id="49657-171">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>

### <a name="overposting"></a><span data-ttu-id="49657-172">Sur-publication</span><span class="sxs-lookup"><span data-stu-id="49657-172">Overposting</span></span>

<span data-ttu-id="49657-173">L’utilisation de `TryUpdateModel` pour mettre à jour des champs avec des valeurs publiées est une bonne pratique de sécurité, car cela empêche la sur-publication.</span><span class="sxs-lookup"><span data-stu-id="49657-173">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="49657-174">Par exemple, supposez que l’entité Student comprend une propriété `Secret` que cette page web ne doit pas mettre à jour ou ajouter :</span><span class="sxs-lookup"><span data-stu-id="49657-174">For example, suppose the Student entity includes a `Secret` property that this web page shouldn't update or add:</span></span>

[!code-csharp[](intro/samples/cu21/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="49657-175">Même si l’application n’a pas de champ `Secret` dans la page Razor de création/mise à jour, un pirate pourrait définir la valeur `Secret` par sur-publication.</span><span class="sxs-lookup"><span data-stu-id="49657-175">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="49657-176">Un pirate pourrait utiliser un outil tel que Fiddler, ou écrire du JavaScript, pour publier une valeur de formulaire `Secret`.</span><span class="sxs-lookup"><span data-stu-id="49657-176">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="49657-177">Le code d’origine ne limite pas les champs que le classeur de modèles utilise quand il crée une instance de Student.</span><span class="sxs-lookup"><span data-stu-id="49657-177">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="49657-178">La valeur spécifiée par le pirate pour le champ de formulaire `Secret`, quelle qu’elle soit, est mise à jour dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="49657-178">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="49657-179">L’illustration suivante montre l’outil Fiddler en train d’ajouter le champ `Secret` (avec la valeur « OverPost ») aux valeurs de formulaire publiées.</span><span class="sxs-lookup"><span data-stu-id="49657-179">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Fiddler ajoutant un champ Secret](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="49657-181">La valeur « OverPost » est ajoutée avec succès à la propriété `Secret` de la ligne insérée.</span><span class="sxs-lookup"><span data-stu-id="49657-181">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="49657-182">Le concepteur de l’application n’a jamais prévu que la propriété `Secret` soit définie avec la page Create.</span><span class="sxs-lookup"><span data-stu-id="49657-182">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>

### <a name="view-model"></a><span data-ttu-id="49657-183">Modèle d’affichage</span><span class="sxs-lookup"><span data-stu-id="49657-183">View model</span></span>

<span data-ttu-id="49657-184">Un modèle d’affichage contient généralement un sous-ensemble des propriétés incluses dans le modèle utilisé par l’application.</span><span class="sxs-lookup"><span data-stu-id="49657-184">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="49657-185">Le modèle d’application est souvent appelé modèle de domaine.</span><span class="sxs-lookup"><span data-stu-id="49657-185">The application model is often called the domain model.</span></span> <span data-ttu-id="49657-186">En règle générale, le modèle de domaine contient toutes les propriétés requises par l’entité correspondante dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="49657-186">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="49657-187">Le modèle d’affichage contient uniquement les propriétés nécessaires pour la couche d’interface utilisateur (par exemple, la page Create).</span><span class="sxs-lookup"><span data-stu-id="49657-187">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="49657-188">En plus du modèle d’affichage, certaines applications utilisent un modèle de liaison ou d’entrée pour transmettre des données entre la classe de modèles de pages de Pages Razor et le navigateur.</span><span class="sxs-lookup"><span data-stu-id="49657-188">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages page model class and the browser.</span></span> <span data-ttu-id="49657-189">Considérez le modèle d’affichage `Student` suivant :</span><span class="sxs-lookup"><span data-stu-id="49657-189">Consider the following `Student` view model:</span></span>

[!code-csharp[](intro/samples/cu21/Models/StudentVM.cs)]

<span data-ttu-id="49657-190">Les modèles d’affichage fournissent une alternative pour empêcher la sur-publication.</span><span class="sxs-lookup"><span data-stu-id="49657-190">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="49657-191">Le modèle d’affichage contient uniquement les propriétés à afficher ou à mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="49657-191">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="49657-192">Le code suivant utilise le modèle d’affichage `StudentVM` pour créer un étudiant :</span><span class="sxs-lookup"><span data-stu-id="49657-192">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="49657-193">La méthode [SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) définit les valeurs de cet objet en lisant les valeurs d’un autre objet [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues).</span><span class="sxs-lookup"><span data-stu-id="49657-193">The [SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="49657-194">`SetValues` utilise la correspondance de nom de propriété.</span><span class="sxs-lookup"><span data-stu-id="49657-194">`SetValues` uses property name matching.</span></span> <span data-ttu-id="49657-195">Le type de modèle d’affichage ne doit pas nécessairement être lié au type de modèle. Il doit simplement avoir des propriétés qui correspondent.</span><span class="sxs-lookup"><span data-stu-id="49657-195">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="49657-196">L’utilisation de `StudentVM` exige que [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) soit mis à jour pour utiliser `StudentVM` plutôt que `Student`.</span><span class="sxs-lookup"><span data-stu-id="49657-196">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="49657-197">Dans les pages Razor, la classe dérivée `PageModel` est le modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="49657-197">In Razor Pages, the `PageModel` derived class is the view model.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="49657-198">Mettre à jour la page Edit</span><span class="sxs-lookup"><span data-stu-id="49657-198">Update the Edit page</span></span>

<span data-ttu-id="49657-199">Mettez à jour le modèle de page pour la page Edit.</span><span class="sxs-lookup"><span data-stu-id="49657-199">Update the page model for the Edit page.</span></span> <span data-ttu-id="49657-200">Les modifications principales apparaissent en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="49657-200">The major changes are highlighted:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="49657-201">Les modifications de code sont semblables à celles de la page Create, à quelques exceptions près :</span><span class="sxs-lookup"><span data-stu-id="49657-201">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="49657-202">`OnPostAsync` a un paramètre facultatif `id`.</span><span class="sxs-lookup"><span data-stu-id="49657-202">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="49657-203">L’étudiant actuel est récupéré à partir de la base de données (on ne crée pas un étudiant vide).</span><span class="sxs-lookup"><span data-stu-id="49657-203">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="49657-204">`FirstOrDefaultAsync` a été remplacée par [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync).</span><span class="sxs-lookup"><span data-stu-id="49657-204">`FirstOrDefaultAsync` has been replaced with [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync).</span></span> <span data-ttu-id="49657-205">`FindAsync` est un bon choix lors de la sélection d’une entité à partir de la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="49657-205">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="49657-206">Pour plus d’informations, consultez [FindAsync](#FindAsync).</span><span class="sxs-lookup"><span data-stu-id="49657-206">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="49657-207">Tester les pages Edit et Create</span><span class="sxs-lookup"><span data-stu-id="49657-207">Test the Edit and Create pages</span></span>

<span data-ttu-id="49657-208">Créez et modifiez quelques entités d’étudiants.</span><span class="sxs-lookup"><span data-stu-id="49657-208">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="49657-209">États des entités</span><span class="sxs-lookup"><span data-stu-id="49657-209">Entity States</span></span>

<span data-ttu-id="49657-210">Le contexte de base de données effectue un suivi afin de savoir si les entités en mémoire sont synchronisées avec les lignes correspondantes dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="49657-210">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="49657-211">Les informations de synchronisation du contexte de base de données déterminent ce qui se produit quand [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) est appelé.</span><span class="sxs-lookup"><span data-stu-id="49657-211">The DB context sync information determines what happens when [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span> <span data-ttu-id="49657-212">Par exemple, quand une nouvelle entité est passée à la méthode [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync), l’état de cette entité est défini sur [Added](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added).</span><span class="sxs-lookup"><span data-stu-id="49657-212">For example, when a new entity is passed to the [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) method, that entity's state is set to [Added](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added).</span></span> <span data-ttu-id="49657-213">Quand `SaveChangesAsync` est appelée, le contexte de base de données émet une commande SQL INSERT.</span><span class="sxs-lookup"><span data-stu-id="49657-213">When `SaveChangesAsync` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="49657-214">Une entité peut être dans l’un des [états suivants](/dotnet/api/microsoft.entityframeworkcore.entitystate) :</span><span class="sxs-lookup"><span data-stu-id="49657-214">An entity may be in one of the [following states](/dotnet/api/microsoft.entityframeworkcore.entitystate):</span></span>

* <span data-ttu-id="49657-215">`Added`: L’entité n’existe pas encore dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="49657-215">`Added`: The entity doesn't yet exist in the DB.</span></span> <span data-ttu-id="49657-216">La méthode `SaveChanges` émet une instruction INSERT.</span><span class="sxs-lookup"><span data-stu-id="49657-216">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="49657-217">`Unchanged`: Aucune modification ne doit être enregistrée avec cette entité.</span><span class="sxs-lookup"><span data-stu-id="49657-217">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="49657-218">Une entité est dans cet état quand elle est lue à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="49657-218">An entity has this status when it's read from the DB.</span></span>

* <span data-ttu-id="49657-219">`Modified`: Tout ou partie des valeurs de propriété de l’entité ont été modifiées.</span><span class="sxs-lookup"><span data-stu-id="49657-219">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="49657-220">La méthode `SaveChanges` émet une instruction UPDATE.</span><span class="sxs-lookup"><span data-stu-id="49657-220">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="49657-221">`Deleted`: L’entité a été marquée pour suppression.</span><span class="sxs-lookup"><span data-stu-id="49657-221">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="49657-222">La méthode `SaveChanges` émet une instruction DELETE.</span><span class="sxs-lookup"><span data-stu-id="49657-222">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="49657-223">`Detached`: L’entité n’est pas suivie par le contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="49657-223">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="49657-224">Dans une application de bureau, les changements d’état sont généralement définis automatiquement.</span><span class="sxs-lookup"><span data-stu-id="49657-224">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="49657-225">Une entité est lue, des modifications sont apportées et l’état d’entité passe automatiquement à `Modified`.</span><span class="sxs-lookup"><span data-stu-id="49657-225">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="49657-226">L’appel de `SaveChanges` génère une instruction SQL UPDATE qui met à jour uniquement les propriétés modifiées.</span><span class="sxs-lookup"><span data-stu-id="49657-226">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="49657-227">Dans une application web, le `DbContext` qui lit une entité et affiche les données est supprimé après le rendu d’une page.</span><span class="sxs-lookup"><span data-stu-id="49657-227">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="49657-228">Quand la méthode `OnPostAsync` d’une page est appelée, une nouvelle requête web est faite avec une nouvelle instance du `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="49657-228">When a page's `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="49657-229">La relecture de l’entité dans ce nouveau contexte simule le traitement de bureau.</span><span class="sxs-lookup"><span data-stu-id="49657-229">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="49657-230">Mettre à jour la page Delete</span><span class="sxs-lookup"><span data-stu-id="49657-230">Update the Delete page</span></span>

<span data-ttu-id="49657-231">Dans cette section, nous ajoutons du code pour implémenter un message d’erreur personnalisé quand l’appel à `SaveChanges` échoue.</span><span class="sxs-lookup"><span data-stu-id="49657-231">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="49657-232">Ajoutez une chaîne contenant les messages d’erreur possibles :</span><span class="sxs-lookup"><span data-stu-id="49657-232">Add a string to contain possible error messages:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="49657-233">Remplacez la méthode `OnGetAsync` par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="49657-233">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="49657-234">Le code précédent contient le paramètre facultatif `saveChangesError`.</span><span class="sxs-lookup"><span data-stu-id="49657-234">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="49657-235">`saveChangesError` indique si la méthode a été appelée après un échec de suppression de l’objet Student.</span><span class="sxs-lookup"><span data-stu-id="49657-235">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="49657-236">L’opération de suppression peut échouer en raison de problèmes réseau temporaires.</span><span class="sxs-lookup"><span data-stu-id="49657-236">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="49657-237">Les erreurs réseau temporaires sont probablement dans le cloud.</span><span class="sxs-lookup"><span data-stu-id="49657-237">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="49657-238">`saveChangesError` a la valeur false quand la méthode `OnGetAsync` de la page Delete est appelée à partir de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="49657-238">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="49657-239">Quand `OnGetAsync` est appelée par `OnPostAsync` (car l’opération de suppression a échoué), le paramètre `saveChangesError` a la valeur true.</span><span class="sxs-lookup"><span data-stu-id="49657-239">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="49657-240">La méthode OnPostAsync des pages Delete</span><span class="sxs-lookup"><span data-stu-id="49657-240">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="49657-241">Remplacez `OnPostAsync` par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="49657-241">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="49657-242">Le code précédent récupère l’entité sélectionnée, puis appelle la méthode [Remove](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) pour définir l’état de l’entité sur `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="49657-242">The preceding code retrieves the selected entity, then calls the [Remove](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="49657-243">Lorsque `SaveChanges` est appelée, une commande SQL DELETE est générée.</span><span class="sxs-lookup"><span data-stu-id="49657-243">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="49657-244">Si `Remove` échoue :</span><span class="sxs-lookup"><span data-stu-id="49657-244">If `Remove` fails:</span></span>

* <span data-ttu-id="49657-245">L’exception de la base de données est interceptée.</span><span class="sxs-lookup"><span data-stu-id="49657-245">The DB exception is caught.</span></span>
* <span data-ttu-id="49657-246">La méthode `OnGetAsync` des pages est appelée avec `saveChangesError=true`.</span><span class="sxs-lookup"><span data-stu-id="49657-246">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="49657-247">Mise à jour de la Page Razor Delete</span><span class="sxs-lookup"><span data-stu-id="49657-247">Update the Delete Razor Page</span></span>

<span data-ttu-id="49657-248">Ajoutez le message d’erreur mis en surbrillance suivant à la Page Razor Delete.</span><span class="sxs-lookup"><span data-stu-id="49657-248">Add the following highlighted error message to the Delete Razor Page.</span></span>
<!--
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?name=snippet&highlight=11)]
-->
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="49657-249">Testez la suppression.</span><span class="sxs-lookup"><span data-stu-id="49657-249">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="49657-250">Erreurs courantes</span><span class="sxs-lookup"><span data-stu-id="49657-250">Common errors</span></span>

<span data-ttu-id="49657-251">Students/Index ou d’autres liens ne fonctionnent pas :</span><span class="sxs-lookup"><span data-stu-id="49657-251">Students/Index or other links don't work:</span></span>

<span data-ttu-id="49657-252">Vérifiez que la Page Razor contient la bonne directive `@page`.</span><span class="sxs-lookup"><span data-stu-id="49657-252">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="49657-253">Par exemple, la page Student/Index Razor Page ne doit **pas** contenir de modèle d’itinéraire :</span><span class="sxs-lookup"><span data-stu-id="49657-253">For example, The Students/Index Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="49657-254">Chaque page Razor doit inclure la directive `@page`.</span><span class="sxs-lookup"><span data-stu-id="49657-254">Each Razor Page must include the `@page` directive.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="49657-255">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="49657-255">Additional resources</span></span>

* [<span data-ttu-id="49657-256">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="49657-256">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=K4X1MT2jt6o)

> [!div class="step-by-step"]
> <span data-ttu-id="49657-257">[Précédent](xref:data/ef-rp/intro)
> [Suivant](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="49657-257">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
