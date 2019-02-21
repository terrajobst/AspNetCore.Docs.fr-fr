---
title: Mettre à jour les pages générées dans une application ASP.NET Core
author: rick-anderson
description: Découvrez comment mettre à jour les pages générées dans une application ASP.NET Core.
ms.author: riande
ms.date: 12/20/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 62385f33dc86609726305728fbc19dd9ff27dc87
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410191"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="798fe-103">Mettre à jour les pages générées dans une application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="798fe-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="798fe-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="798fe-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="798fe-105">L’application de gestion des films générée est un bon début, mais la présentation n’est pas idéale.</span><span class="sxs-lookup"><span data-stu-id="798fe-105">The scaffolded movie app has a good start, but the presentation isn't ideal.</span></span> <span data-ttu-id="798fe-106">**ReleaseDate** doit être **Release Date** (en deux mots).</span><span class="sxs-lookup"><span data-stu-id="798fe-106">**ReleaseDate** should be **Release Date** (two words).</span></span>

![Application de gestion des films ouverte dans Chrome](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="798fe-108">Mettre à jour le code généré</span><span class="sxs-lookup"><span data-stu-id="798fe-108">Update the generated code</span></span>

<span data-ttu-id="798fe-109">Ouvrez le fichier *Models/Movie.cs*, puis ajoutez les lignes affichées en surbrillance dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="798fe-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateFixed.cs?name=snippet_1&highlight=12,17)]

<span data-ttu-id="798fe-110">L’annotation de données `[Column(TypeName = "decimal(18, 2)")]` permet à Entity Framework Core de mapper correctement `Price` en devise dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="798fe-110">The `[Column(TypeName = "decimal(18, 2)")]` data annotation enables Entity Framework Core to correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="798fe-111">Pour plus d’informations, consultez [Types de données](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="798fe-111">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="798fe-112">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) est abordé dans le tutoriel suivant.</span><span class="sxs-lookup"><span data-stu-id="798fe-112">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) is covered in the next tutorial.</span></span> <span data-ttu-id="798fe-113">L’attribut [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) spécifie les éléments à afficher pour le nom d’un champ (dans le cas présent, « Release Date » au lieu de « ReleaseDate »).</span><span class="sxs-lookup"><span data-stu-id="798fe-113">The [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="798fe-114">L’attribut [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) spécifie le type des données (Date). Les informations d’heures stockées dans le champ ne s’affichent donc pas.</span><span class="sxs-lookup"><span data-stu-id="798fe-114">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field isn't displayed.</span></span>

<span data-ttu-id="798fe-115">Accédez à Pages/Movies, puis placez le curseur sur un lien **Modifier** pour afficher l’URL cible.</span><span class="sxs-lookup"><span data-stu-id="798fe-115">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Fenêtre de navigateur avec la souris sur le lien Edit et l’URL de lien http://localhost:1234/Movies/Edit/5 affichée](~/tutorials/razor-pages/da1/edit7.png)

<span data-ttu-id="798fe-117">Les liens **Edit**, **Details**, et **Delete** sont générés par le [Tag Helper d’ancre](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) dans le fichier *Pages/Movies/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="798fe-117">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="798fe-118">Les [Tag Helpers](xref:mvc/views/tag-helpers/intro) permettent au code côté serveur de participer à la création et au rendu des éléments HTML dans les fichiers Razor.</span><span class="sxs-lookup"><span data-stu-id="798fe-118">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="798fe-119">Dans le code précédent, `AnchorTagHelper` génère dynamiquement la valeur d’attribut HTML `href` dans la page Razor (l’itinéraire est relatif), `asp-page` et l’ID de l’itinéraire (`asp-route-id`).</span><span class="sxs-lookup"><span data-stu-id="798fe-119">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="798fe-120">Pour plus d’informations, consultez [Génération d’URL pour les pages](xref:razor-pages/index#url-generation-for-pages).</span><span class="sxs-lookup"><span data-stu-id="798fe-120">See [URL generation for Pages](xref:razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="798fe-121">Utilisez **Afficher la Source** dans votre navigateur favori pour examiner le balisage généré.</span><span class="sxs-lookup"><span data-stu-id="798fe-121">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="798fe-122">Une partie du code HTML généré est affichée ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="798fe-122">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="798fe-123">Les liens générés dynamiquement passent l’ID de film avec une chaîne de requête (par exemple `?id=1` dans `https://localhost:5001/Movies/Details?id=1`).</span><span class="sxs-lookup"><span data-stu-id="798fe-123">The dynamically-generated links pass the movie ID with a query string (for example, the `?id=1` in  `https://localhost:5001/Movies/Details?id=1`).</span></span>

<span data-ttu-id="798fe-124">Mettez à jour les pages Razor Edit, Details et Delete pour utiliser le modèle d’itinéraire « {id:int} ».</span><span class="sxs-lookup"><span data-stu-id="798fe-124">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="798fe-125">Remplacez la directive de chacune de ces pages (`@page`) par `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="798fe-125">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="798fe-126">Exécuter l’application, puis affichez le code source.</span><span class="sxs-lookup"><span data-stu-id="798fe-126">Run the app and then view source.</span></span> <span data-ttu-id="798fe-127">Le code HTML généré ajoute l’ID à la partie de chemin de l’URL :</span><span class="sxs-lookup"><span data-stu-id="798fe-127">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="798fe-128">Une requête à la page avec le modèle d’itinéraire « {id:int} » qui n’inclut **pas** l’entier retourne une erreur HTTP 404 (introuvable).</span><span class="sxs-lookup"><span data-stu-id="798fe-128">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="798fe-129">Par exemple, `http://localhost:5000/Movies/Details` retourne une erreur 404.</span><span class="sxs-lookup"><span data-stu-id="798fe-129">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="798fe-130">Pour que l’ID soit facultatif, ajoutez `?` à la contrainte de route :</span><span class="sxs-lookup"><span data-stu-id="798fe-130">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="798fe-131">Pour tester le comportement de `@page "{id:int?}"` :</span><span class="sxs-lookup"><span data-stu-id="798fe-131">To test the behavior of `@page "{id:int?}"`:</span></span>

* <span data-ttu-id="798fe-132">Définissez la directive de page dans *Pages/Movies/Details.cshtml* sur `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="798fe-132">Set the page directive in *Pages/Movies/Details.cshtml* to `@page "{id:int?}"`.</span></span>
* <span data-ttu-id="798fe-133">Définissez un point d’arrêt dans `public async Task<IActionResult> OnGetAsync(int? id)` (dans *Pages/Movies/Details.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="798fe-133">Set a break point in `public async Task<IActionResult> OnGetAsync(int? id)` (in *Pages/Movies/Details.cshtml.cs*).</span></span>
* <span data-ttu-id="798fe-134">Accédez à `https://localhost:5001/Movies/Details/`.</span><span class="sxs-lookup"><span data-stu-id="798fe-134">Navigate to `https://localhost:5001/Movies/Details/`.</span></span>

<span data-ttu-id="798fe-135">Avec la directive `@page "{id:int}"`, le point d’arrêt n’est jamais atteint.</span><span class="sxs-lookup"><span data-stu-id="798fe-135">With the `@page "{id:int}"` directive, the break point is never hit.</span></span> <span data-ttu-id="798fe-136">Le moteur de routage retourne l’erreur HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="798fe-136">The routing engine returns HTTP 404.</span></span> <span data-ttu-id="798fe-137">Avec `@page "{id:int?}"`, la méthode `OnGetAsync` retourne `NotFound` (HTTP 404).</span><span class="sxs-lookup"><span data-stu-id="798fe-137">Using `@page "{id:int?}"`, the `OnGetAsync` method returns `NotFound` (HTTP 404).</span></span>

<span data-ttu-id="798fe-138">Bien que ce ne soit pas recommandé, vous pouvez écrire la méthode `OnGetAsync` (dans *Pages/Movies/Delete.cshtml.cs*) comme :</span><span class="sxs-lookup"><span data-stu-id="798fe-138">Although not recommended, you could write the `OnGetAsync` method (in *Pages/Movies/Delete.cshtml.cs*) as:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Delete.cshtml.cs?name=snippet)]

<span data-ttu-id="798fe-139">Testez le code précédent :</span><span class="sxs-lookup"><span data-stu-id="798fe-139">Test the preceding code:</span></span>

* <span data-ttu-id="798fe-140">Sélectionnez un lien de **suppression**.</span><span class="sxs-lookup"><span data-stu-id="798fe-140">Select a **Delete** link.</span></span>
* <span data-ttu-id="798fe-141">Supprimez l’ID de l’URL.</span><span class="sxs-lookup"><span data-stu-id="798fe-141">Remove the ID from the URL.</span></span> <span data-ttu-id="798fe-142">Par exemple, remplacez `https://localhost:5001/Movies/Delete/8` par `https://localhost:5001/Movies/Delete`.</span><span class="sxs-lookup"><span data-stu-id="798fe-142">For example, change `https://localhost:5001/Movies/Delete/8` to `https://localhost:5001/Movies/Delete`.</span></span>
* <span data-ttu-id="798fe-143">Effectuez un pas à pas détaillé du code dans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="798fe-143">Step through the code in the debugger.</span></span>

### <a name="review-concurrency-exception-handling"></a><span data-ttu-id="798fe-144">Passer en revue la gestion des exceptions d’accès concurrentiel</span><span class="sxs-lookup"><span data-stu-id="798fe-144">Review concurrency exception handling</span></span>

<span data-ttu-id="798fe-145">Passez en revue la méthode `OnPostAsync` dans le fichier *Pages/Movies/Edit.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="798fe-145">Review the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="798fe-146">Le code précédent détecte les exceptions d’accès concurrentiel quand un client supprime le film et que l’autre client envoie les modifications apportées au film.</span><span class="sxs-lookup"><span data-stu-id="798fe-146">The previous code detects concurrency exceptions when the one client deletes the movie and the other client posts changes to the movie.</span></span>

<span data-ttu-id="798fe-147">Pour tester le bloc `catch` :</span><span class="sxs-lookup"><span data-stu-id="798fe-147">To test the `catch` block:</span></span>

* <span data-ttu-id="798fe-148">Définissez un point d'arrêt sur `catch (DbUpdateConcurrencyException)`</span><span class="sxs-lookup"><span data-stu-id="798fe-148">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="798fe-149">Sélectionnez **Edit** (Modifier) pour un film, apportez des modifications, mais ne cliquez pas sur **Save** (Enregistrer).</span><span class="sxs-lookup"><span data-stu-id="798fe-149">Select **Edit** for a movie, make changes, but don't enter **Save**.</span></span>
* <span data-ttu-id="798fe-150">Dans une autre fenêtre de navigateur, sélectionnez le lien **Delete** du même film, puis supprimez le film.</span><span class="sxs-lookup"><span data-stu-id="798fe-150">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="798fe-151">Dans la fenêtre de navigateur précédente, postez les modifications apportées au film.</span><span class="sxs-lookup"><span data-stu-id="798fe-151">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="798fe-152">Dans le code destiné à la production, il est nécessaire de détecter les conflits d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="798fe-152">Production code may want to detect concurrency conflicts.</span></span> <span data-ttu-id="798fe-153">Pour plus d’informations, consultez [Gérer les conflits d’accès concurrentiel](xref:data/ef-rp/concurrency).</span><span class="sxs-lookup"><span data-stu-id="798fe-153">See [Handle concurrency conflicts](xref:data/ef-rp/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="798fe-154">Validation de la publication et de la liaison</span><span class="sxs-lookup"><span data-stu-id="798fe-154">Posting and binding review</span></span>

<span data-ttu-id="798fe-155">Examinez le fichier *Pages/Movies/Edit.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="798fe-155">Examine the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit21.cshtml.cs?name=snippet2)]

<span data-ttu-id="798fe-156">Quand une requête HTTP GET est effectuée sur la page Movies/Edit (par exemple, `http://localhost:5000/Movies/Edit/2`) :</span><span class="sxs-lookup"><span data-stu-id="798fe-156">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="798fe-157">La méthode `OnGetAsync` extrait le film de la base de données et retourne la méthode `Page`.</span><span class="sxs-lookup"><span data-stu-id="798fe-157">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span> 
* <span data-ttu-id="798fe-158">La méthode `Page` restitue la page Razor *Pages/Movies/Edit.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="798fe-158">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="798fe-159">Le fichier *Pages/Movies/Edit.cshtml* contient la directive de modèle (`@model RazorPagesMovie.Pages.Movies.EditModel`), ce qui rend le modèle Movie disponible sur la page.</span><span class="sxs-lookup"><span data-stu-id="798fe-159">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the movie model available on the page.</span></span>
* <span data-ttu-id="798fe-160">Le formulaire d’édition s’affiche avec les valeurs relatives au film.</span><span class="sxs-lookup"><span data-stu-id="798fe-160">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="798fe-161">Quand la page Movies/Edit est postée :</span><span class="sxs-lookup"><span data-stu-id="798fe-161">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="798fe-162">Les valeurs de formulaire affichées dans la page sont liées à la propriété `Movie`.</span><span class="sxs-lookup"><span data-stu-id="798fe-162">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="798fe-163">L’attribut `[BindProperty]` active la [liaison de données](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="798fe-163">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="798fe-164">S’il existe des erreurs dans l’état du modèle (par exemple, si `ReleaseDate` ne peut pas être converti en date), le formulaire est affiché avec les valeurs soumises.</span><span class="sxs-lookup"><span data-stu-id="798fe-164">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is displayed with the submitted values.</span></span>
* <span data-ttu-id="798fe-165">S’il n’y a aucune erreur de modèle, le film est enregistré.</span><span class="sxs-lookup"><span data-stu-id="798fe-165">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="798fe-166">Les méthodes HTTP GET dans les pages Razor Index, Create et Delete suivent un modèle similaire.</span><span class="sxs-lookup"><span data-stu-id="798fe-166">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="798fe-167">La méthode HTTP POST `OnPostAsync` dans la page Razor Create suit un modèle semblable à la méthode `OnPostAsync` dans la page Razor Edit.</span><span class="sxs-lookup"><span data-stu-id="798fe-167">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

<span data-ttu-id="798fe-168">La fonction de recherche est ajoutée dans le prochain didacticiel.</span><span class="sxs-lookup"><span data-stu-id="798fe-168">Search is added in the next tutorial.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="798fe-169">[Précédent : Utilisation avec une base de données](xref:tutorials/razor-pages/sql)
> [Suivant : Ajouter une recherche](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="798fe-169">[Previous: Working with a database](xref:tutorials/razor-pages/sql)
[Next: Add search](xref:tutorials/razor-pages/search)</span></span>
