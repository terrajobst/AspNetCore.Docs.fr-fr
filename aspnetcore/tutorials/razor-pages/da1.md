---
title: Mettre à jour les pages générées dans une application ASP.NET Core
author: rick-anderson
description: Découvrez comment mettre à jour les pages générées dans une application ASP.NET Core.
ms.author: riande
ms.date: 12/20/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 0f6535462fe2d308825bf7289c10d2b0690cebd4
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666214"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="69037-103">Mettre à jour les pages générées dans une application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="69037-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="69037-104">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="69037-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="69037-105">L’application de gestion des films générée est un bon début, mais la présentation n’est pas idéale.</span><span class="sxs-lookup"><span data-stu-id="69037-105">The scaffolded movie app has a good start, but the presentation isn't ideal.</span></span> <span data-ttu-id="69037-106">**ReleaseDate** doit être **Release Date** (en deux mots).</span><span class="sxs-lookup"><span data-stu-id="69037-106">**ReleaseDate** should be **Release Date** (two words).</span></span>

![Application de gestion des films ouverte dans Chrome](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="69037-108">Mettre à jour le code généré</span><span class="sxs-lookup"><span data-stu-id="69037-108">Update the generated code</span></span>

<span data-ttu-id="69037-109">Ouvrez le fichier *Models/Movie.cs*, puis ajoutez les lignes affichées en surbrillance dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="69037-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateFixed.cs?name=snippet_1&highlight=3,12,17)]

<span data-ttu-id="69037-110">L’annotation de données `[Column(TypeName = "decimal(18, 2)")]` permet à Entity Framework Core de mapper correctement `Price` en devise dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="69037-110">The `[Column(TypeName = "decimal(18, 2)")]` data annotation enables Entity Framework Core to correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="69037-111">Pour plus d’informations, consultez [Types de données](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="69037-111">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="69037-112">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) est abordé dans le tutoriel suivant.</span><span class="sxs-lookup"><span data-stu-id="69037-112">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) is covered in the next tutorial.</span></span> <span data-ttu-id="69037-113">L’attribut [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) spécifie les éléments à afficher pour le nom d’un champ (dans le cas présent, « Release Date » au lieu de « ReleaseDate »).</span><span class="sxs-lookup"><span data-stu-id="69037-113">The [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="69037-114">L’attribut [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) spécifie le type des données (Date). Les informations d’heures stockées dans le champ ne s’affichent donc pas.</span><span class="sxs-lookup"><span data-stu-id="69037-114">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field isn't displayed.</span></span>

<span data-ttu-id="69037-115">Accédez à Pages/Movies, puis placez le curseur sur un lien **Modifier** pour afficher l’URL cible.</span><span class="sxs-lookup"><span data-stu-id="69037-115">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Fenêtre de navigateur avec la souris sur le lien Edit et l’URL de lien http://localhost:1234/Movies/Edit/5 affichée](~/tutorials/razor-pages/da1/edit7.png)

<span data-ttu-id="69037-117">Les liens **Edit**, **Details**, et **Delete** sont générés par le [Tag Helper d’ancre](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) dans le fichier *Pages/Movies/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="69037-117">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="69037-118">Les [Tag Helpers](xref:mvc/views/tag-helpers/intro) permettent au code côté serveur de participer à la création et au rendu des éléments HTML dans les fichiers Razor.</span><span class="sxs-lookup"><span data-stu-id="69037-118">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="69037-119">Dans le code précédent, `AnchorTagHelper` génère dynamiquement la valeur d’attribut HTML `href` dans la page Razor (l’itinéraire est relatif), `asp-page` et l’ID de l’itinéraire (`asp-route-id`).</span><span class="sxs-lookup"><span data-stu-id="69037-119">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="69037-120">Pour plus d’informations, consultez [Génération d’URL pour les pages](xref:razor-pages/index#url-generation-for-pages).</span><span class="sxs-lookup"><span data-stu-id="69037-120">See [URL generation for Pages](xref:razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="69037-121">Utilisez **Afficher la Source** dans votre navigateur favori pour examiner le balisage généré.</span><span class="sxs-lookup"><span data-stu-id="69037-121">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="69037-122">Une partie du code HTML généré est affichée ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="69037-122">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="69037-123">Les liens générés dynamiquement passent l’ID de film avec une chaîne de requête (par exemple `?id=1` dans `https://localhost:5001/Movies/Details?id=1`).</span><span class="sxs-lookup"><span data-stu-id="69037-123">The dynamically-generated links pass the movie ID with a query string (for example, the `?id=1` in  `https://localhost:5001/Movies/Details?id=1`).</span></span>

### <a name="add-route-template"></a><span data-ttu-id="69037-124">Ajouter un modèle de route</span><span class="sxs-lookup"><span data-stu-id="69037-124">Add route template</span></span>

<span data-ttu-id="69037-125">Mettez à jour les pages Razor Edit, Details et Delete pour utiliser le modèle d’itinéraire « {id:int} ».</span><span class="sxs-lookup"><span data-stu-id="69037-125">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="69037-126">Remplacez la directive de chacune de ces pages (`@page`) par `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="69037-126">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="69037-127">Exécuter l’application, puis affichez le code source.</span><span class="sxs-lookup"><span data-stu-id="69037-127">Run the app and then view source.</span></span> <span data-ttu-id="69037-128">Le code HTML généré ajoute l’ID à la partie de chemin de l’URL :</span><span class="sxs-lookup"><span data-stu-id="69037-128">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="69037-129">Une requête à la page avec le modèle d’itinéraire « {id:int} » qui n’inclut **pas** l’entier retourne une erreur HTTP 404 (introuvable).</span><span class="sxs-lookup"><span data-stu-id="69037-129">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="69037-130">Par exemple, `http://localhost:5000/Movies/Details` retourne une erreur 404.</span><span class="sxs-lookup"><span data-stu-id="69037-130">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="69037-131">Pour que l’ID soit facultatif, ajoutez `?` à la contrainte de route :</span><span class="sxs-lookup"><span data-stu-id="69037-131">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="69037-132">Pour tester le comportement de `@page "{id:int?}"` :</span><span class="sxs-lookup"><span data-stu-id="69037-132">To test the behavior of `@page "{id:int?}"`:</span></span>

* <span data-ttu-id="69037-133">Définissez la directive de page dans *Pages/Movies/Details.cshtml* sur `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="69037-133">Set the page directive in *Pages/Movies/Details.cshtml* to `@page "{id:int?}"`.</span></span>
* <span data-ttu-id="69037-134">Définissez un point d’arrêt dans `public async Task<IActionResult> OnGetAsync(int? id)` (dans *Pages/Movies/Details.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="69037-134">Set a break point in `public async Task<IActionResult> OnGetAsync(int? id)` (in *Pages/Movies/Details.cshtml.cs*).</span></span>
* <span data-ttu-id="69037-135">Accédez à `https://localhost:5001/Movies/Details/`.</span><span class="sxs-lookup"><span data-stu-id="69037-135">Navigate to `https://localhost:5001/Movies/Details/`.</span></span>

<span data-ttu-id="69037-136">Avec la directive `@page "{id:int}"`, le point d’arrêt n’est jamais atteint.</span><span class="sxs-lookup"><span data-stu-id="69037-136">With the `@page "{id:int}"` directive, the break point is never hit.</span></span> <span data-ttu-id="69037-137">Le moteur de routage retourne l’erreur HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="69037-137">The routing engine returns HTTP 404.</span></span> <span data-ttu-id="69037-138">Avec `@page "{id:int?}"`, la méthode `OnGetAsync` retourne `NotFound` (HTTP 404).</span><span class="sxs-lookup"><span data-stu-id="69037-138">Using `@page "{id:int?}"`, the `OnGetAsync` method returns `NotFound` (HTTP 404).</span></span>

### <a name="review-concurrency-exception-handling"></a><span data-ttu-id="69037-139">Passer en revue la gestion des exceptions d’accès concurrentiel</span><span class="sxs-lookup"><span data-stu-id="69037-139">Review concurrency exception handling</span></span>

<span data-ttu-id="69037-140">Passez en revue la méthode `OnPostAsync` dans le fichier *Pages/Movies/Edit.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="69037-140">Review the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="69037-141">Le code précédent détecte les exceptions d’accès concurrentiel quand un client supprime le film et que l’autre client envoie les modifications apportées au film.</span><span class="sxs-lookup"><span data-stu-id="69037-141">The previous code detects concurrency exceptions when the one client deletes the movie and the other client posts changes to the movie.</span></span>

<span data-ttu-id="69037-142">Pour tester le bloc `catch` :</span><span class="sxs-lookup"><span data-stu-id="69037-142">To test the `catch` block:</span></span>

* <span data-ttu-id="69037-143">Définissez un point d'arrêt sur `catch (DbUpdateConcurrencyException)`</span><span class="sxs-lookup"><span data-stu-id="69037-143">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="69037-144">Sélectionnez **Edit** (Modifier) pour un film, apportez des modifications, mais ne cliquez pas sur **Save** (Enregistrer).</span><span class="sxs-lookup"><span data-stu-id="69037-144">Select **Edit** for a movie, make changes, but don't enter **Save**.</span></span>
* <span data-ttu-id="69037-145">Dans une autre fenêtre de navigateur, sélectionnez le lien **Delete** du même film, puis supprimez le film.</span><span class="sxs-lookup"><span data-stu-id="69037-145">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="69037-146">Dans la fenêtre de navigateur précédente, postez les modifications apportées au film.</span><span class="sxs-lookup"><span data-stu-id="69037-146">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="69037-147">Dans le code destiné à la production, il est nécessaire de détecter les conflits d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="69037-147">Production code may want to detect concurrency conflicts.</span></span> <span data-ttu-id="69037-148">Pour plus d’informations, consultez [Gérer les conflits d’accès concurrentiel](xref:data/ef-rp/concurrency).</span><span class="sxs-lookup"><span data-stu-id="69037-148">See [Handle concurrency conflicts](xref:data/ef-rp/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="69037-149">Validation de la publication et de la liaison</span><span class="sxs-lookup"><span data-stu-id="69037-149">Posting and binding review</span></span>

<span data-ttu-id="69037-150">Examinez le fichier *Pages/Movies/Edit.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="69037-150">Examine the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/SnapShots/Edit.cshtml.cs?name=snippet2)]

<span data-ttu-id="69037-151">Quand une requête HTTP GET est effectuée sur la page Movies/Edit (par exemple, `http://localhost:5000/Movies/Edit/2`) :</span><span class="sxs-lookup"><span data-stu-id="69037-151">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="69037-152">La méthode `OnGetAsync` extrait le film de la base de données et retourne la méthode `Page`.</span><span class="sxs-lookup"><span data-stu-id="69037-152">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span>
* <span data-ttu-id="69037-153">La méthode `Page` restitue la page Razor *Pages/Movies/Edit.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="69037-153">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="69037-154">Le fichier *Pages/Movies/Edit.cshtml* contient la directive de modèle (`@model RazorPagesMovie.Pages.Movies.EditModel`), ce qui rend le modèle Movie disponible sur la page.</span><span class="sxs-lookup"><span data-stu-id="69037-154">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the movie model available on the page.</span></span>
* <span data-ttu-id="69037-155">Le formulaire d’édition s’affiche avec les valeurs relatives au film.</span><span class="sxs-lookup"><span data-stu-id="69037-155">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="69037-156">Quand la page Movies/Edit est postée :</span><span class="sxs-lookup"><span data-stu-id="69037-156">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="69037-157">Les valeurs de formulaire affichées dans la page sont liées à la propriété `Movie`.</span><span class="sxs-lookup"><span data-stu-id="69037-157">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="69037-158">L’attribut `[BindProperty]` active la [liaison de données](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="69037-158">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="69037-159">S’il y a des erreurs dans l’état du modèle (par exemple, si `ReleaseDate` ne peut pas être converti en date), le formulaire est à nouveau affiché avec les valeurs soumises.</span><span class="sxs-lookup"><span data-stu-id="69037-159">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is redisplayed with the submitted values.</span></span>
* <span data-ttu-id="69037-160">S’il n’y a aucune erreur de modèle, le film est enregistré.</span><span class="sxs-lookup"><span data-stu-id="69037-160">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="69037-161">Les méthodes HTTP GET dans les pages Razor Index, Create et Delete suivent un modèle similaire.</span><span class="sxs-lookup"><span data-stu-id="69037-161">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="69037-162">La méthode HTTP POST `OnPostAsync` dans la page Razor Create suit un modèle semblable à la méthode `OnPostAsync` dans la page Razor Edit.</span><span class="sxs-lookup"><span data-stu-id="69037-162">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="69037-163">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="69037-163">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="69037-164">[Précédent : Utilisation d’une base de données](xref:tutorials/razor-pages/sql)
> [Suivant : Ajouter une fonction de recherche](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="69037-164">[Previous: Working with a database](xref:tutorials/razor-pages/sql)
[Next: Add search](xref:tutorials/razor-pages/search)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="69037-165">L’application de gestion des films générée est un bon début, mais la présentation n’est pas idéale.</span><span class="sxs-lookup"><span data-stu-id="69037-165">The scaffolded movie app has a good start, but the presentation isn't ideal.</span></span> <span data-ttu-id="69037-166">**ReleaseDate** doit être **Release Date** (en deux mots).</span><span class="sxs-lookup"><span data-stu-id="69037-166">**ReleaseDate** should be **Release Date** (two words).</span></span>

![Application de gestion des films ouverte dans Chrome](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="69037-168">Mettre à jour le code généré</span><span class="sxs-lookup"><span data-stu-id="69037-168">Update the generated code</span></span>

<span data-ttu-id="69037-169">Ouvrez le fichier *Models/Movie.cs*, puis ajoutez les lignes affichées en surbrillance dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="69037-169">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateFixed.cs?name=snippet_1&highlight=3,12,17)]

<span data-ttu-id="69037-170">L’annotation de données `[Column(TypeName = "decimal(18, 2)")]` permet à Entity Framework Core de mapper correctement `Price` en devise dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="69037-170">The `[Column(TypeName = "decimal(18, 2)")]` data annotation enables Entity Framework Core to correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="69037-171">Pour plus d’informations, consultez [Types de données](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="69037-171">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="69037-172">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) est abordé dans le tutoriel suivant.</span><span class="sxs-lookup"><span data-stu-id="69037-172">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) is covered in the next tutorial.</span></span> <span data-ttu-id="69037-173">L’attribut [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) spécifie les éléments à afficher pour le nom d’un champ (dans le cas présent, « Release Date » au lieu de « ReleaseDate »).</span><span class="sxs-lookup"><span data-stu-id="69037-173">The [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="69037-174">L’attribut [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) spécifie le type des données (Date). Les informations d’heures stockées dans le champ ne s’affichent donc pas.</span><span class="sxs-lookup"><span data-stu-id="69037-174">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field isn't displayed.</span></span>

<span data-ttu-id="69037-175">Accédez à Pages/Movies, puis placez le curseur sur un lien **Modifier** pour afficher l’URL cible.</span><span class="sxs-lookup"><span data-stu-id="69037-175">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Fenêtre de navigateur avec la souris sur le lien Edit et l’URL de lien http://localhost:1234/Movies/Edit/5 affichée](~/tutorials/razor-pages/da1/edit7.png)

<span data-ttu-id="69037-177">Les liens **Edit**, **Details**, et **Delete** sont générés par le [Tag Helper d’ancre](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) dans le fichier *Pages/Movies/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="69037-177">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="69037-178">Les [Tag Helpers](xref:mvc/views/tag-helpers/intro) permettent au code côté serveur de participer à la création et au rendu des éléments HTML dans les fichiers Razor.</span><span class="sxs-lookup"><span data-stu-id="69037-178">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="69037-179">Dans le code précédent, `AnchorTagHelper` génère dynamiquement la valeur d’attribut HTML `href` dans la page Razor (l’itinéraire est relatif), `asp-page` et l’ID de l’itinéraire (`asp-route-id`).</span><span class="sxs-lookup"><span data-stu-id="69037-179">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="69037-180">Pour plus d’informations, consultez [Génération d’URL pour les pages](xref:razor-pages/index#url-generation-for-pages).</span><span class="sxs-lookup"><span data-stu-id="69037-180">See [URL generation for Pages](xref:razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="69037-181">Utilisez **Afficher la Source** dans votre navigateur favori pour examiner le balisage généré.</span><span class="sxs-lookup"><span data-stu-id="69037-181">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="69037-182">Une partie du code HTML généré est affichée ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="69037-182">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="69037-183">Les liens générés dynamiquement passent l’ID de film avec une chaîne de requête (par exemple `?id=1` dans `https://localhost:5001/Movies/Details?id=1`).</span><span class="sxs-lookup"><span data-stu-id="69037-183">The dynamically-generated links pass the movie ID with a query string (for example, the `?id=1` in  `https://localhost:5001/Movies/Details?id=1`).</span></span>

<span data-ttu-id="69037-184">Mettez à jour les pages Razor Edit, Details et Delete pour utiliser le modèle d’itinéraire « {id:int} ».</span><span class="sxs-lookup"><span data-stu-id="69037-184">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="69037-185">Remplacez la directive de chacune de ces pages (`@page`) par `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="69037-185">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="69037-186">Exécuter l’application, puis affichez le code source.</span><span class="sxs-lookup"><span data-stu-id="69037-186">Run the app and then view source.</span></span> <span data-ttu-id="69037-187">Le code HTML généré ajoute l’ID à la partie de chemin de l’URL :</span><span class="sxs-lookup"><span data-stu-id="69037-187">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="69037-188">Une requête à la page avec le modèle d’itinéraire « {id:int} » qui n’inclut **pas** l’entier retourne une erreur HTTP 404 (introuvable).</span><span class="sxs-lookup"><span data-stu-id="69037-188">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="69037-189">Par exemple, `http://localhost:5000/Movies/Details` retourne une erreur 404.</span><span class="sxs-lookup"><span data-stu-id="69037-189">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="69037-190">Pour que l’ID soit facultatif, ajoutez `?` à la contrainte de route :</span><span class="sxs-lookup"><span data-stu-id="69037-190">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="69037-191">Pour tester le comportement de `@page "{id:int?}"` :</span><span class="sxs-lookup"><span data-stu-id="69037-191">To test the behavior of `@page "{id:int?}"`:</span></span>

* <span data-ttu-id="69037-192">Définissez la directive de page dans *Pages/Movies/Details.cshtml* sur `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="69037-192">Set the page directive in *Pages/Movies/Details.cshtml* to `@page "{id:int?}"`.</span></span>
* <span data-ttu-id="69037-193">Définissez un point d’arrêt dans `public async Task<IActionResult> OnGetAsync(int? id)` (dans *Pages/Movies/Details.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="69037-193">Set a break point in `public async Task<IActionResult> OnGetAsync(int? id)` (in *Pages/Movies/Details.cshtml.cs*).</span></span>
* <span data-ttu-id="69037-194">Accédez à `https://localhost:5001/Movies/Details/`.</span><span class="sxs-lookup"><span data-stu-id="69037-194">Navigate to `https://localhost:5001/Movies/Details/`.</span></span>

<span data-ttu-id="69037-195">Avec la directive `@page "{id:int}"`, le point d’arrêt n’est jamais atteint.</span><span class="sxs-lookup"><span data-stu-id="69037-195">With the `@page "{id:int}"` directive, the break point is never hit.</span></span> <span data-ttu-id="69037-196">Le moteur de routage retourne l’erreur HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="69037-196">The routing engine returns HTTP 404.</span></span> <span data-ttu-id="69037-197">Avec `@page "{id:int?}"`, la méthode `OnGetAsync` retourne `NotFound` (HTTP 404).</span><span class="sxs-lookup"><span data-stu-id="69037-197">Using `@page "{id:int?}"`, the `OnGetAsync` method returns `NotFound` (HTTP 404).</span></span>

### <a name="review-concurrency-exception-handling"></a><span data-ttu-id="69037-198">Passer en revue la gestion des exceptions d’accès concurrentiel</span><span class="sxs-lookup"><span data-stu-id="69037-198">Review concurrency exception handling</span></span>

<span data-ttu-id="69037-199">Passez en revue la méthode `OnPostAsync` dans le fichier *Pages/Movies/Edit.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="69037-199">Review the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="69037-200">Le code précédent détecte les exceptions d’accès concurrentiel quand un client supprime le film et que l’autre client envoie les modifications apportées au film.</span><span class="sxs-lookup"><span data-stu-id="69037-200">The previous code detects concurrency exceptions when the one client deletes the movie and the other client posts changes to the movie.</span></span>

<span data-ttu-id="69037-201">Pour tester le bloc `catch` :</span><span class="sxs-lookup"><span data-stu-id="69037-201">To test the `catch` block:</span></span>

* <span data-ttu-id="69037-202">Définissez un point d'arrêt sur `catch (DbUpdateConcurrencyException)`</span><span class="sxs-lookup"><span data-stu-id="69037-202">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="69037-203">Sélectionnez **Edit** (Modifier) pour un film, apportez des modifications, mais ne cliquez pas sur **Save** (Enregistrer).</span><span class="sxs-lookup"><span data-stu-id="69037-203">Select **Edit** for a movie, make changes, but don't enter **Save**.</span></span>
* <span data-ttu-id="69037-204">Dans une autre fenêtre de navigateur, sélectionnez le lien **Delete** du même film, puis supprimez le film.</span><span class="sxs-lookup"><span data-stu-id="69037-204">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="69037-205">Dans la fenêtre de navigateur précédente, postez les modifications apportées au film.</span><span class="sxs-lookup"><span data-stu-id="69037-205">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="69037-206">Dans le code destiné à la production, il est nécessaire de détecter les conflits d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="69037-206">Production code may want to detect concurrency conflicts.</span></span> <span data-ttu-id="69037-207">Pour plus d’informations, consultez [Gérer les conflits d’accès concurrentiel](xref:data/ef-rp/concurrency).</span><span class="sxs-lookup"><span data-stu-id="69037-207">See [Handle concurrency conflicts](xref:data/ef-rp/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="69037-208">Validation de la publication et de la liaison</span><span class="sxs-lookup"><span data-stu-id="69037-208">Posting and binding review</span></span>

<span data-ttu-id="69037-209">Examinez le fichier *Pages/Movies/Edit.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="69037-209">Examine the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit21.cshtml.cs?name=snippet2)]

<span data-ttu-id="69037-210">Quand une requête HTTP GET est effectuée sur la page Movies/Edit (par exemple, `http://localhost:5000/Movies/Edit/2`) :</span><span class="sxs-lookup"><span data-stu-id="69037-210">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="69037-211">La méthode `OnGetAsync` extrait le film de la base de données et retourne la méthode `Page`.</span><span class="sxs-lookup"><span data-stu-id="69037-211">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span> 
* <span data-ttu-id="69037-212">La méthode `Page` restitue la page Razor *Pages/Movies/Edit.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="69037-212">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="69037-213">Le fichier *Pages/Movies/Edit.cshtml* contient la directive de modèle (`@model RazorPagesMovie.Pages.Movies.EditModel`), ce qui rend le modèle Movie disponible sur la page.</span><span class="sxs-lookup"><span data-stu-id="69037-213">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the movie model available on the page.</span></span>
* <span data-ttu-id="69037-214">Le formulaire d’édition s’affiche avec les valeurs relatives au film.</span><span class="sxs-lookup"><span data-stu-id="69037-214">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="69037-215">Quand la page Movies/Edit est postée :</span><span class="sxs-lookup"><span data-stu-id="69037-215">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="69037-216">Les valeurs de formulaire affichées dans la page sont liées à la propriété `Movie`.</span><span class="sxs-lookup"><span data-stu-id="69037-216">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="69037-217">L’attribut `[BindProperty]` active la [liaison de données](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="69037-217">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="69037-218">S’il existe des erreurs dans l’état du modèle (par exemple, si `ReleaseDate` ne peut pas être converti en date), le formulaire est affiché avec les valeurs soumises.</span><span class="sxs-lookup"><span data-stu-id="69037-218">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is displayed with the submitted values.</span></span>
* <span data-ttu-id="69037-219">S’il n’y a aucune erreur de modèle, le film est enregistré.</span><span class="sxs-lookup"><span data-stu-id="69037-219">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="69037-220">Les méthodes HTTP GET dans les pages Razor Index, Create et Delete suivent un modèle similaire.</span><span class="sxs-lookup"><span data-stu-id="69037-220">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="69037-221">La méthode HTTP POST `OnPostAsync` dans la page Razor Create suit un modèle semblable à la méthode `OnPostAsync` dans la page Razor Edit.</span><span class="sxs-lookup"><span data-stu-id="69037-221">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

<span data-ttu-id="69037-222">La fonction de recherche est ajoutée dans le prochain didacticiel.</span><span class="sxs-lookup"><span data-stu-id="69037-222">Search is added in the next tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="69037-223">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="69037-223">Additional resources</span></span>

* [<span data-ttu-id="69037-224">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="69037-224">YouTube version of this tutorial</span></span>](https://youtu.be/yLnnleREMtQ)

> [!div class="step-by-step"]
> <span data-ttu-id="69037-225">[Précédent : Utilisation d’une base de données](xref:tutorials/razor-pages/sql)
> [Suivant : Ajouter une fonction de recherche](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="69037-225">[Previous: Working with a database](xref:tutorials/razor-pages/sql)
[Next: Add search](xref:tutorials/razor-pages/search)</span></span>

::: moniker-end
