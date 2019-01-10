---
title: Méthodes et vues de contrôleur dans ASP.NET Core
author: rick-anderson
description: Découvrez comment utiliser des méthodes et des vues de contrôleur ainsi que DataAnnotations dans ASP.NET Core.
ms.author: riande
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: e6bdaec6dfe681582a54bd39ce1c76bebe6f5fb2
ms.sourcegitcommit: e1cc4c1ef6c9e07918a609d5ad7fadcb6abe3e12
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/03/2019
ms.locfileid: "53997251"
---
# <a name="controller-methods-and-views-in-aspnet-core"></a><span data-ttu-id="68378-103">Méthodes et vues de contrôleur dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="68378-103">Controller methods and views in ASP.NET Core</span></span>

<span data-ttu-id="68378-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="68378-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="68378-105">Nous avons une bonne ébauche de l’application de films, mais sa présentation n’est pas idéale.</span><span class="sxs-lookup"><span data-stu-id="68378-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="68378-106">Nous ne voulons pas voir l’heure (« 12:00:00 AM » dans l’image ci-dessous) et **ReleaseDate** doit être composé de deux mots.</span><span class="sxs-lookup"><span data-stu-id="68378-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Vue Index : Release Date apparaît en un seul mot (sans espace) et chaque date de sortie d’un film a comme heure « 12 AM »](working-with-sql/_static/m55.png)

<span data-ttu-id="68378-108">Ouvrez le fichier *Models/Movie.cs*, puis ajoutez les lignes en surbrillance ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="68378-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie22/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]

<span data-ttu-id="68378-109">Nous abordons [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) dans le prochain tutoriel.</span><span class="sxs-lookup"><span data-stu-id="68378-109">We cover [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) in the next tutorial.</span></span> <span data-ttu-id="68378-110">L’attribut [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) spécifie les éléments à afficher pour le nom d’un champ (dans le cas présent, « Release Date » au lieu de « ReleaseDate »).</span><span class="sxs-lookup"><span data-stu-id="68378-110">The [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="68378-111">L’attribut [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) spécifie le type des données (Date). Les informations d’heures stockées dans le champ ne s’affichent donc pas.</span><span class="sxs-lookup"><span data-stu-id="68378-111">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field isn't displayed.</span></span>

<span data-ttu-id="68378-112">L’annotation de données `[Column(TypeName = "decimal(18, 2)")]` est nécessaire pour qu’Entity Framework Core puisse correctement mapper `Price` en devise dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="68378-112">The `[Column(TypeName = "decimal(18, 2)")]` data annotation is required so Entity Framework Core can correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="68378-113">Pour plus d’informations, consultez [Types de données](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="68378-113">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="68378-114">Accédez au contrôleur `Movies` et maintenez le pointeur de la souris sur un lien **Edit** pour afficher l’URL cible.</span><span class="sxs-lookup"><span data-stu-id="68378-114">Browse to the `Movies` controller and hold the mouse pointer over an **Edit** link to see the target URL.</span></span>

![Fenêtre de navigateur avec la souris sur le lien Edit et l’URL de lien https://localhost:5001/Movies/Edit/5 affichée](~/tutorials/first-mvc-app/controller-methods-views/_static/edit7.png)

<span data-ttu-id="68378-116">Les liens **Edit**, **Details** et **Delete** sont générés par le Tag Helper Anchor Core MVC dans le fichier *Views/Movies/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="68378-116">The **Edit**, **Details**, and **Delete** links are generated by the Core MVC Anchor Tag Helper in the *Views/Movies/Index.cshtml* file.</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1-3&range=46-50)]

<span data-ttu-id="68378-117">Les [Tag Helpers](xref:mvc/views/tag-helpers/intro) permettent au code côté serveur de participer à la création et au rendu des éléments HTML dans les fichiers Razor.</span><span class="sxs-lookup"><span data-stu-id="68378-117">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="68378-118">Dans le code ci-dessus, le `AnchorTagHelper` génère dynamiquement la valeur d’attribut `href` HTML à partir de l’ID d’itinéraire et de la méthode d’action de contrôleur. Utilisez **Afficher la Source** dans votre navigateur favori ou les outils de développement pour examiner le balisage généré.</span><span class="sxs-lookup"><span data-stu-id="68378-118">In the code above, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the controller action method and route id. You use **View Source** from your favorite browser or use the developer tools to examine the generated markup.</span></span> <span data-ttu-id="68378-119">Une partie du code HTML généré est affichée ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="68378-119">A portion of the generated HTML is shown below:</span></span>

```html
 <td>
    <a href="/Movies/Edit/4"> Edit </a> |
    <a href="/Movies/Details/4"> Details </a> |
    <a href="/Movies/Delete/4"> Delete </a>
</td>
```

<span data-ttu-id="68378-120">Rappelez-vous le format du [routage](xref:mvc/controllers/routing) défini dans le fichier *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="68378-120">Recall the format for [routing](xref:mvc/controllers/routing) set in the *Startup.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

<span data-ttu-id="68378-121">ASP.NET Core traduit `https://localhost:5001/Movies/Edit/4` en une requête à la méthode d’action `Edit` du contrôleur `Movies` avec un paramètre `Id` de 4.</span><span class="sxs-lookup"><span data-stu-id="68378-121">ASP.NET Core translates `https://localhost:5001/Movies/Edit/4` into a request to the `Edit` action method of the `Movies` controller with the parameter `Id` of 4.</span></span> <span data-ttu-id="68378-122">(Les méthodes de contrôleur sont également appelées méthodes d’action.)</span><span class="sxs-lookup"><span data-stu-id="68378-122">(Controller methods are also known as action methods.)</span></span>

<span data-ttu-id="68378-123">Les [Tag Helpers](xref:mvc/views/tag-helpers/intro) sont l’une des nouvelles fonctionnalités les plus populaires dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="68378-123">[Tag Helpers](xref:mvc/views/tag-helpers/intro) are one of the most popular new features in ASP.NET Core.</span></span> <span data-ttu-id="68378-124">Pour plus d'informations, consultez [Ressources supplémentaires](#additional-resources).</span><span class="sxs-lookup"><span data-stu-id="68378-124">For more information, see [Additional resources](#additional-resources).</span></span>

<span data-ttu-id="68378-125">Ouvrez le contrôleur `Movies` et examinez les deux méthodes d’action `Edit`.</span><span class="sxs-lookup"><span data-stu-id="68378-125">Open the `Movies` controller and examine the two `Edit` action methods.</span></span> <span data-ttu-id="68378-126">Le code suivant montre la méthode `HTTP GET Edit`, qui extrait le film et renseigne le formulaire de modification généré par le fichier Razor *Edit.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="68378-126">The following code shows the `HTTP GET Edit` method, which fetches the movie and populates the edit form generated by the *Edit.cshtml* Razor file.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit1)]

<span data-ttu-id="68378-127">Le code suivant montre la méthode `HTTP POST Edit`, qui traite les valeurs de film publiées :</span><span class="sxs-lookup"><span data-stu-id="68378-127">The following code shows the `HTTP POST Edit` method, which processes the posted movie values:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]

<span data-ttu-id="68378-128">Le code suivant montre la méthode `HTTP POST Edit`, qui traite les valeurs de film publiées :</span><span class="sxs-lookup"><span data-stu-id="68378-128">The following code shows the `HTTP POST Edit` method, which processes the posted movie values:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

<span data-ttu-id="68378-129">L’attribut `[Bind]` est l’un des moyens qui permettent d’assurer une protection contre la [sur-publication](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost).</span><span class="sxs-lookup"><span data-stu-id="68378-129">The `[Bind]` attribute is one way to protect against [over-posting](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost).</span></span> <span data-ttu-id="68378-130">Vous devez inclure dans l’attribut `[Bind]` uniquement les propriétés que vous souhaitez modifier.</span><span class="sxs-lookup"><span data-stu-id="68378-130">You should only include properties in the `[Bind]` attribute that you want to change.</span></span> <span data-ttu-id="68378-131">Pour plus d’informations, consultez [Protéger votre contrôleur contre la sur-publication](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="68378-131">For more information, see [Protect your controller from over-posting](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application).</span></span> <span data-ttu-id="68378-132">Les [ViewModels](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/) fournissent une alternative pour empêcher la sur-publication.</span><span class="sxs-lookup"><span data-stu-id="68378-132">[ViewModels](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/) provide an alternative approach to prevent over-posting.</span></span>

<span data-ttu-id="68378-133">Notez que la deuxième méthode d’action `Edit` est précédée de l’attribut `[HttpPost]`.</span><span class="sxs-lookup"><span data-stu-id="68378-133">Notice the second `Edit` action method is preceded by the `[HttpPost]` attribute.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit2&highlight=1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2&highlight=4)]

::: moniker-end

<span data-ttu-id="68378-134">L’attribut `HttpPost` indique que cette méthode `Edit` peut être appelée *uniquement* pour les requêtes `POST`.</span><span class="sxs-lookup"><span data-stu-id="68378-134">The `HttpPost` attribute specifies that this `Edit` method can be invoked *only* for `POST` requests.</span></span> <span data-ttu-id="68378-135">Vous pouvez appliquer l’attribut `[HttpGet]` à la première méthode Edit, mais cela n’est pas nécessaire car `[HttpGet]` est la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="68378-135">You could apply the `[HttpGet]` attribute to the first edit method, but that's not necessary because `[HttpGet]` is the default.</span></span>

<span data-ttu-id="68378-136">L’attribut `ValidateAntiForgeryToken` est utilisé pour [lutter contre la falsification de requête](xref:security/anti-request-forgery). Il est associé à un jeton anti-contrefaçon généré dans le fichier de la vue Edit (*Views/Movies/Edit.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="68378-136">The `ValidateAntiForgeryToken` attribute is used to [prevent forgery of a request](xref:security/anti-request-forgery) and is paired up with an anti-forgery token generated in the edit view file (*Views/Movies/Edit.cshtml*).</span></span> <span data-ttu-id="68378-137">Le fichier de la vue Edit génère le jeton anti-contrefaçon avec le [Tag Helper Form](xref:mvc/views/working-with-forms).</span><span class="sxs-lookup"><span data-stu-id="68378-137">The edit view file generates the anti-forgery token with the [Form Tag Helper](xref:mvc/views/working-with-forms).</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/Edit.cshtml?range=9)]

<span data-ttu-id="68378-138">Le [Tag Helper Form](xref:mvc/views/working-with-forms) génère un jeton anti-contrefaçon masqué qui doit correspondre au jeton anti-contrefaçon généré par `[ValidateAntiForgeryToken]` dans la méthode `Edit` du contrôleur Movies.</span><span class="sxs-lookup"><span data-stu-id="68378-138">The [Form Tag Helper](xref:mvc/views/working-with-forms) generates a hidden anti-forgery token that must match the `[ValidateAntiForgeryToken]` generated anti-forgery token in the `Edit` method of the Movies controller.</span></span> <span data-ttu-id="68378-139">Pour plus d’informations, consultez [Protection contre la falsification de requête](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="68378-139">For more information, see [Anti-Request Forgery](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="68378-140">La méthode `HttpGet Edit` prend le paramètre `ID` du film, recherche le film à l’aide de la méthode Entity Framework `SingleOrDefaultAsync`, et retourne le film sélectionné à la vue Edit.</span><span class="sxs-lookup"><span data-stu-id="68378-140">The `HttpGet Edit` method takes the movie `ID` parameter, looks up the movie using the Entity Framework `SingleOrDefaultAsync` method, and returns the selected movie to the Edit view.</span></span> <span data-ttu-id="68378-141">Si un film est introuvable, l’erreur `NotFound` (HTTP 404) est retournée.</span><span class="sxs-lookup"><span data-stu-id="68378-141">If a movie cannot be found, `NotFound` (HTTP 404) is returned.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit1)]

<span data-ttu-id="68378-142">Quand le système de génération de modèles automatique a créé la vue Edit, il a examiné la classe `Movie` et a créé le code pour restituer les éléments `<label>` et `<input>` de chaque propriété de la classe.</span><span class="sxs-lookup"><span data-stu-id="68378-142">When the scaffolding system created the Edit view, it examined the `Movie` class and created code to render `<label>` and `<input>` elements for each property of the class.</span></span> <span data-ttu-id="68378-143">L’exemple suivant montre la vue Edit qui a été générée par le système de génération de modèles automatique de Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="68378-143">The following example shows the Edit view that was generated by the Visual Studio scaffolding system:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/EditOriginal.cshtml)]

<span data-ttu-id="68378-144">Notez que le modèle de vue comporte une instruction `@model MvcMovie.Models.Movie` en haut du fichier.</span><span class="sxs-lookup"><span data-stu-id="68378-144">Notice how the view template has a `@model MvcMovie.Models.Movie` statement at the top of the file.</span></span> <span data-ttu-id="68378-145">`@model MvcMovie.Models.Movie` indique que la vue s’attend à ce que le modèle pour le modèle de vue soit de type `Movie`.</span><span class="sxs-lookup"><span data-stu-id="68378-145">`@model MvcMovie.Models.Movie` specifies that the view expects the model for the view template to be of type `Movie`.</span></span>

<span data-ttu-id="68378-146">Le code de génération de modèles automatique utilise plusieurs méthodes Tag Helper afin de rationaliser le balisage HTML.</span><span class="sxs-lookup"><span data-stu-id="68378-146">The scaffolded code uses several Tag Helper methods to streamline the HTML markup.</span></span> <span data-ttu-id="68378-147">Le [Tag Helper Label](xref:mvc/views/working-with-forms) affiche le nom du champ (« Title », « ReleaseDate », « Genre » ou « Price »).</span><span class="sxs-lookup"><span data-stu-id="68378-147">The - [Label Tag Helper](xref:mvc/views/working-with-forms) displays the name of the field ("Title", "ReleaseDate", "Genre", or "Price").</span></span> <span data-ttu-id="68378-148">Le [Tag Helper Input](xref:mvc/views/working-with-forms) affiche l’élément `<input>` HTML.</span><span class="sxs-lookup"><span data-stu-id="68378-148">The [Input Tag Helper](xref:mvc/views/working-with-forms) renders an HTML `<input>` element.</span></span> <span data-ttu-id="68378-149">Le [Tag Helper Validation](xref:mvc/views/working-with-forms) affiche les messages de validation associés à cette propriété.</span><span class="sxs-lookup"><span data-stu-id="68378-149">The [Validation Tag Helper](xref:mvc/views/working-with-forms) displays any validation messages associated with that property.</span></span>

<span data-ttu-id="68378-150">Exécutez l’application et accédez à l’URL `/Movies`.</span><span class="sxs-lookup"><span data-stu-id="68378-150">Run the application and navigate to the `/Movies` URL.</span></span> <span data-ttu-id="68378-151">Cliquez sur un lien **Edit**.</span><span class="sxs-lookup"><span data-stu-id="68378-151">Click an **Edit** link.</span></span> <span data-ttu-id="68378-152">Dans le navigateur, affichez la source de la page.</span><span class="sxs-lookup"><span data-stu-id="68378-152">In the browser, view the source for the page.</span></span> <span data-ttu-id="68378-153">Le code HTML généré pour l’élément `<form>` est indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="68378-153">The generated HTML for the `<form>` element is shown below.</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/edit_view_source.html?highlight=1,6,10,17,24,28)]

<span data-ttu-id="68378-154">Les éléments `<input>` sont dans un élément `HTML <form>` dont l’attribut `action` est défini de façon à publier à l’URL `/Movies/Edit/id`.</span><span class="sxs-lookup"><span data-stu-id="68378-154">The `<input>` elements are in an `HTML <form>` element whose `action` attribute is set to post to the `/Movies/Edit/id` URL.</span></span> <span data-ttu-id="68378-155">Les données du formulaire sont publiées au serveur en cas de clic sur le bouton `Save`.</span><span class="sxs-lookup"><span data-stu-id="68378-155">The form data will be posted to the server when the `Save` button is clicked.</span></span> <span data-ttu-id="68378-156">La dernière ligne avant l’élément `</form>` de fermeture montre le jeton [XSRF](xref:security/anti-request-forgery) masqué généré par le [Tag Helper Form](xref:mvc/views/working-with-forms).</span><span class="sxs-lookup"><span data-stu-id="68378-156">The last line before the closing `</form>` element shows the hidden [XSRF](xref:security/anti-request-forgery) token generated by the [Form Tag Helper](xref:mvc/views/working-with-forms).</span></span>

## <a name="processing-the-post-request"></a><span data-ttu-id="68378-157">Traitement de la requête POST</span><span class="sxs-lookup"><span data-stu-id="68378-157">Processing the POST Request</span></span>

<span data-ttu-id="68378-158">Le code suivant montre la version `[HttpPost]` de la méthode d’action `Edit`.</span><span class="sxs-lookup"><span data-stu-id="68378-158">The following listing shows the `[HttpPost]` version of the `Edit` action method.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

<span data-ttu-id="68378-159">L’attribut `[ValidateAntiForgeryToken]` valide le jeton [XSRF](xref:security/anti-request-forgery) masqué généré par le générateur de jetons anti-contrefaçon dans le [Tag Helper Form](xref:mvc/views/working-with-forms)</span><span class="sxs-lookup"><span data-stu-id="68378-159">The `[ValidateAntiForgeryToken]` attribute validates the hidden [XSRF](xref:security/anti-request-forgery) token generated by the anti-forgery token generator in the [Form Tag Helper](xref:mvc/views/working-with-forms)</span></span>

<span data-ttu-id="68378-160">Le système de [liaison de modèle](xref:mvc/models/model-binding) prend les valeurs de formulaire publiées et crée un objet `Movie` qui est passé en tant que paramètre `movie`.</span><span class="sxs-lookup"><span data-stu-id="68378-160">The [model binding](xref:mvc/models/model-binding) system takes the posted form values and creates a `Movie` object that's passed as the `movie` parameter.</span></span> <span data-ttu-id="68378-161">La méthode `ModelState.IsValid` vérifie que les données envoyées dans le formulaire peuvent être utilisées pour changer (modifier ou mettre à jour) un objet `Movie`.</span><span class="sxs-lookup"><span data-stu-id="68378-161">The `ModelState.IsValid` method verifies that the data submitted in the form can be used to modify (edit or update) a `Movie` object.</span></span> <span data-ttu-id="68378-162">Si les données sont valides, elles sont enregistrées.</span><span class="sxs-lookup"><span data-stu-id="68378-162">If the data is valid, it's saved.</span></span> <span data-ttu-id="68378-163">Les données de film mises à jour (modifiées) sont enregistrées dans la base de données en appelant la méthode `SaveChangesAsync` du contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="68378-163">The updated (edited) movie data is saved to the database by calling the `SaveChangesAsync` method of database context.</span></span> <span data-ttu-id="68378-164">Après avoir enregistré les données, le code redirige l’utilisateur vers la méthode d’action `Index` de la classe `MoviesController`, qui affiche la collection de films, avec notamment les modifications qui viennent d’être apportées.</span><span class="sxs-lookup"><span data-stu-id="68378-164">After saving the data, the code redirects the user to the `Index` action method of the `MoviesController` class, which displays the movie collection, including the changes just made.</span></span>

<span data-ttu-id="68378-165">Avant que le formulaire soit publié sur le serveur, la validation côté client vérifie les règles de validation sur les champs.</span><span class="sxs-lookup"><span data-stu-id="68378-165">Before the form is posted to the server, client-side validation checks any validation rules on the fields.</span></span> <span data-ttu-id="68378-166">En cas d’erreur de validation, un message d’erreur s’affiche et le formulaire n’est pas publié.</span><span class="sxs-lookup"><span data-stu-id="68378-166">If there are any validation errors, an error message is displayed and the form isn't posted.</span></span> <span data-ttu-id="68378-167">Si JavaScript est désactivé, aucune validation côté client n’est effectuée, mais le serveur détecte les valeurs publiées qui ne sont pas valides, et les valeurs de formulaire sont réaffichées avec des messages d’erreur.</span><span class="sxs-lookup"><span data-stu-id="68378-167">If JavaScript is disabled, you won't have client-side validation but the server will detect the posted values that are not valid, and the form values will be redisplayed with error messages.</span></span> <span data-ttu-id="68378-168">Plus loin dans ce didacticiel, nous examinerons la [Validation du modèle](xref:mvc/models/validation) plus en détail.</span><span class="sxs-lookup"><span data-stu-id="68378-168">Later in the tutorial we examine [Model Validation](xref:mvc/models/validation) in more detail.</span></span> <span data-ttu-id="68378-169">Le [Tag Helper Validation](xref:mvc/views/working-with-forms) dans le modèle de vue *Views/Movies/Edit.cshtml* se charge de l’affichage des messages d’erreur appropriés.</span><span class="sxs-lookup"><span data-stu-id="68378-169">The [Validation Tag Helper](xref:mvc/views/working-with-forms) in the *Views/Movies/Edit.cshtml* view template takes care of displaying appropriate error messages.</span></span>

![Vue Edit : une exception pour une valeur Price incorrecte « abc » indique que le champ Price doit être un nombre.](~/tutorials/first-mvc-app/controller-methods-views/_static/val.png)

<span data-ttu-id="68378-172">Toutes les méthodes `HttpGet` du contrôleur Movies suivent un modèle similaire.</span><span class="sxs-lookup"><span data-stu-id="68378-172">All the `HttpGet` methods in the movie controller follow a similar pattern.</span></span> <span data-ttu-id="68378-173">Elles reçoivent un objet de film (ou une liste d’objets, dans le cas de `Index`) et passent l’objet (modèle) à la vue.</span><span class="sxs-lookup"><span data-stu-id="68378-173">They get a movie object (or list of objects, in the case of `Index`), and pass the object (model) to the view.</span></span> <span data-ttu-id="68378-174">La méthode `Create` passe un objet de film vide à la vue `Create`.</span><span class="sxs-lookup"><span data-stu-id="68378-174">The `Create` method passes an empty movie object to the `Create` view.</span></span> <span data-ttu-id="68378-175">Toutes les méthodes qui créent, modifient, suppriment ou changent d’une quelconque manière des données le font dans la surcharge `[HttpPost]` de la méthode.</span><span class="sxs-lookup"><span data-stu-id="68378-175">All the methods that create, edit, delete, or otherwise modify data do so in the `[HttpPost]` overload of the method.</span></span> <span data-ttu-id="68378-176">Modifier des données dans une méthode `HTTP GET` présente un risque pour la sécurité.</span><span class="sxs-lookup"><span data-stu-id="68378-176">Modifying data in an `HTTP GET` method is a security risk.</span></span> <span data-ttu-id="68378-177">La modification des données dans une méthode `HTTP GET` enfreint également les bonnes pratiques HTTP et le modèle architectural [REST](http://rest.elkstein.org/), qui spécifie que les requêtes GET ne doivent pas changer l’état de votre application.</span><span class="sxs-lookup"><span data-stu-id="68378-177">Modifying data in an `HTTP GET` method also violates HTTP best practices and the architectural [REST](http://rest.elkstein.org/) pattern, which specifies that GET requests shouldn't change the state of your application.</span></span> <span data-ttu-id="68378-178">En d’autres termes, une opération GET doit être sûre, ne doit avoir aucun effet secondaire et ne doit pas modifier vos données persistantes.</span><span class="sxs-lookup"><span data-stu-id="68378-178">In other words, performing a GET operation should be a safe operation that has no side effects and doesn't modify your persisted data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="68378-179">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="68378-179">Additional resources</span></span>

* [<span data-ttu-id="68378-180">Globalisation et localisation</span><span class="sxs-lookup"><span data-stu-id="68378-180">Globalization and localization</span></span>](xref:fundamentals/localization)
* [<span data-ttu-id="68378-181">Introduction aux Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="68378-181">Introduction to Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="68378-182">Créer des Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="68378-182">Author Tag Helpers</span></span>](xref:mvc/views/tag-helpers/authoring)
* [<span data-ttu-id="68378-183">Protection contre la falsification de requête</span><span class="sxs-lookup"><span data-stu-id="68378-183">Anti-Request Forgery</span></span>](xref:security/anti-request-forgery)
* <span data-ttu-id="68378-184">Protéger votre contrôleur contre la [sur-publication](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application)</span><span class="sxs-lookup"><span data-stu-id="68378-184">Protect your controller from [over-posting](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application)</span></span>
* [<span data-ttu-id="68378-185">ViewModels</span><span class="sxs-lookup"><span data-stu-id="68378-185">ViewModels</span></span>](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/)
* [<span data-ttu-id="68378-186">Tag Helper Form</span><span class="sxs-lookup"><span data-stu-id="68378-186">Form Tag Helper</span></span>](xref:mvc/views/working-with-forms)
* [<span data-ttu-id="68378-187">Tag Helper Input</span><span class="sxs-lookup"><span data-stu-id="68378-187">Input Tag Helper</span></span>](xref:mvc/views/working-with-forms)
* [<span data-ttu-id="68378-188">Tag Helper Label</span><span class="sxs-lookup"><span data-stu-id="68378-188">Label Tag Helper</span></span>](xref:mvc/views/working-with-forms)
* [<span data-ttu-id="68378-189">Tag Helper Select</span><span class="sxs-lookup"><span data-stu-id="68378-189">Select Tag Helper</span></span>](xref:mvc/views/working-with-forms)
* [<span data-ttu-id="68378-190">Tag Helper Validation</span><span class="sxs-lookup"><span data-stu-id="68378-190">Validation Tag Helper</span></span>](xref:mvc/views/working-with-forms)

> [!div class="step-by-step"]
> <span data-ttu-id="68378-191">[Précédent](working-with-sql.md)
> [Suivant](search.md)</span><span class="sxs-lookup"><span data-stu-id="68378-191">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
