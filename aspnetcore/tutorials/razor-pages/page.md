---
title: Pages Razor obtenues par génération de modèles automatique dans ASP.NET Core
author: rick-anderson
description: Décrit l’obtention de pages Razor par génération de modèles automatique.
ms.author: riande
ms.date: 08/17/2019
uid: tutorials/razor-pages/page
ms.openlocfilehash: 594fd6186cc73aa054fc9a1478850fa01e481ef2
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034197"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="9c975-103">Pages Razor obtenues par génération de modèles automatique dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9c975-103">Scaffolded Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9c975-104">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9c975-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9c975-105">Ce didacticiel décrit les pages Razor créées par génération de modèles automatique au cours du [didacticiel précédent](xref:tutorials/razor-pages/model).</span><span class="sxs-lookup"><span data-stu-id="9c975-105">This tutorial examines the Razor Pages created by scaffolding in the [previous tutorial](xref:tutorials/razor-pages/model).</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="9c975-106">Pages Create, Delete, Details et Edit</span><span class="sxs-lookup"><span data-stu-id="9c975-106">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="9c975-107">Examinez le modèle de page *Pages/Movies/Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="9c975-107">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="9c975-108">Les pages Razor sont dérivées de `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="9c975-108">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="9c975-109">Par convention, la classe dérivée de `PageModel` s’appelle `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="9c975-109">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="9c975-110">Le constructeur utilise l’[injection de dépendances](xref:fundamentals/dependency-injection) pour ajouter `RazorPagesMovieContext` à la page.</span><span class="sxs-lookup"><span data-stu-id="9c975-110">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="9c975-111">Toutes les pages obtenues par génération de modèles automatique suivent ce modèle.</span><span class="sxs-lookup"><span data-stu-id="9c975-111">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="9c975-112">Consultez [Code asynchrone](xref:data/ef-rp/intro#asynchronous-code) pour plus d’informations sur la programmation asynchrone avec Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9c975-112">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programming with Entity Framework.</span></span>

<span data-ttu-id="9c975-113">Quand une requête est effectuée pour la page, la méthode `OnGetAsync` retourne une liste de films à la page Razor.</span><span class="sxs-lookup"><span data-stu-id="9c975-113">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="9c975-114">`OnGetAsync` ou `OnGet` est appelé pour initialiser l’état de la page.</span><span class="sxs-lookup"><span data-stu-id="9c975-114">`OnGetAsync` or `OnGet` is called to initialize the state of the page.</span></span> <span data-ttu-id="9c975-115">Dans ce cas, `OnGetAsync` obtient une liste de films et les affiche.</span><span class="sxs-lookup"><span data-stu-id="9c975-115">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="9c975-116">Lorsque `OnGet` retourne `void` ou `OnGetAsync` retourne`Task`, aucune instruction return n’est utilisée.</span><span class="sxs-lookup"><span data-stu-id="9c975-116">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return statement is used.</span></span> <span data-ttu-id="9c975-117">Lorsque le type de retour est `IActionResult` ou `Task<IActionResult>`, une instruction de retour doit être spécifiée.</span><span class="sxs-lookup"><span data-stu-id="9c975-117">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="9c975-118">Par exemple, la méthode *Pages/Movies/Create.cshtml.cs* `OnPostAsync` :</span><span class="sxs-lookup"><span data-stu-id="9c975-118">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> <span data-ttu-id="9c975-119">Examinez la page Razor *Pages/Movies/Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="9c975-119">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml)]

<span data-ttu-id="9c975-120">Razor peut passer du HTML au C# ou à des balises spécifiques à Razor.</span><span class="sxs-lookup"><span data-stu-id="9c975-120">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="9c975-121">Quand un symbole `@` est suivi d’un [mot clé réservé Razor](xref:mvc/views/razor#razor-reserved-keywords), il est converti en balise spécifique à Razor. Sinon, il est converti en C#.</span><span class="sxs-lookup"><span data-stu-id="9c975-121">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

### <a name="the-page-directive"></a><span data-ttu-id="9c975-122">Directive @page</span><span class="sxs-lookup"><span data-stu-id="9c975-122">The @page directive</span></span>

<span data-ttu-id="9c975-123">La directive Razor `@page` fait du fichier une action MVC, ce qui signifie qu’il peut gérer les demandes.</span><span class="sxs-lookup"><span data-stu-id="9c975-123">The `@page` Razor directive makes the file an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="9c975-124">`@page` doit être la première directive Razor sur une page.</span><span class="sxs-lookup"><span data-stu-id="9c975-124">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="9c975-125">`@page` est un exemple de conversion en balise spécifique à Razor.</span><span class="sxs-lookup"><span data-stu-id="9c975-125">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="9c975-126">Pour plus d’informations, consultez [Syntaxe Razor](xref:mvc/views/razor#razor-syntax).</span><span class="sxs-lookup"><span data-stu-id="9c975-126">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="9c975-127">Examinez l’expression lambda utilisée dans le HTML Helper suivant :</span><span class="sxs-lookup"><span data-stu-id="9c975-127">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title)
```

<span data-ttu-id="9c975-128">Le HTML Helper `DisplayNameFor` inspecte la propriété `Title` référencée dans l’expression lambda pour déterminer le nom d’affichage.</span><span class="sxs-lookup"><span data-stu-id="9c975-128">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="9c975-129">L’expression lambda est inspectée plutôt qu’évaluée.</span><span class="sxs-lookup"><span data-stu-id="9c975-129">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="9c975-130">Cela signifie qu’il n’y a aucune violation d’accès lorsque `model`, `model.Movie`ou `model.Movie[0]` est `null` ou vide.</span><span class="sxs-lookup"><span data-stu-id="9c975-130">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` is `null` or empty.</span></span> <span data-ttu-id="9c975-131">Quand l’expression lambda est évaluée (par exemple avec `@Html.DisplayFor(modelItem => item.Title)`), les valeurs de propriété du modèle sont évaluées.</span><span class="sxs-lookup"><span data-stu-id="9c975-131">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>

### <a name="the-model-directive"></a><span data-ttu-id="9c975-132">Directive @model</span><span class="sxs-lookup"><span data-stu-id="9c975-132">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="9c975-133">La directive `@model` spécifie le type du modèle passé à la page Razor.</span><span class="sxs-lookup"><span data-stu-id="9c975-133">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="9c975-134">Dans l’exemple précédent, la ligne `@model` rend la classe dérivée `PageModel` accessible à la page Razor.</span><span class="sxs-lookup"><span data-stu-id="9c975-134">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="9c975-135">Le modèle est utilisé dans les [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) `@Html.DisplayNameFor` et `@Html.DisplayFor` de la page.</span><span class="sxs-lookup"><span data-stu-id="9c975-135">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="9c975-136">La page de disposition</span><span class="sxs-lookup"><span data-stu-id="9c975-136">The layout page</span></span>

<span data-ttu-id="9c975-137">Sélectionnez les liens du menu (**RazorPagesMovie**, **Accueil** et **Confidentialité**).</span><span class="sxs-lookup"><span data-stu-id="9c975-137">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="9c975-138">Chaque page affiche la même disposition de menu.</span><span class="sxs-lookup"><span data-stu-id="9c975-138">Each page shows the same menu layout.</span></span> <span data-ttu-id="9c975-139">La disposition du menu est implémentée dans le fichier *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9c975-139">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="9c975-140">Ouvrez le fichier *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9c975-140">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="9c975-141">Les modèles de [Disposition](xref:mvc/views/layout) permettent que la disposition du conteneur HTML soit :</span><span class="sxs-lookup"><span data-stu-id="9c975-141">[Layout](xref:mvc/views/layout) templates allow the HTML container layout to be:</span></span>

* <span data-ttu-id="9c975-142">spécifiée à un seul endroit ;</span><span class="sxs-lookup"><span data-stu-id="9c975-142">Specified in one place.</span></span>
* <span data-ttu-id="9c975-143">appliquée à plusieurs pages du site.</span><span class="sxs-lookup"><span data-stu-id="9c975-143">Applied in multiple pages in the site.</span></span>

<span data-ttu-id="9c975-144">Recherchez la ligne `@RenderBody()`.</span><span class="sxs-lookup"><span data-stu-id="9c975-144">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="9c975-145">`RenderBody` est un espace réservé dans lequel toutes les vues propres à la page s’affichent, *encapsulées* dans la page de disposition.</span><span class="sxs-lookup"><span data-stu-id="9c975-145">`RenderBody` is a placeholder where all the page-specific views show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="9c975-146">Par exemple, sélectionnez le lien **Confidentialité** et la vue *Pages/Privacy.cshtml* est restituée dans la méthode `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="9c975-146">For example, select the **Privacy** link and the *Pages/Privacy.cshtml* view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>

### <a name="viewdata-and-layout"></a><span data-ttu-id="9c975-147">ViewData et disposition</span><span class="sxs-lookup"><span data-stu-id="9c975-147">ViewData and layout</span></span>

<span data-ttu-id="9c975-148">Considérez la balise suivante du fichier *Pages/Movies/Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="9c975-148">Consider the following markup from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="9c975-149">La balise précédente en surbrillance est un exemple de passage de Razor au C#.</span><span class="sxs-lookup"><span data-stu-id="9c975-149">The preceding highlighted markup is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="9c975-150">Les caractères `{` et `}` délimitent un bloc de code C#.</span><span class="sxs-lookup"><span data-stu-id="9c975-150">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="9c975-151">La classe de base `PageModel` contient une propriété de dictionnaire `ViewData` qui peut être utilisée pour passer des données à une vue.</span><span class="sxs-lookup"><span data-stu-id="9c975-151">The `PageModel` base class contains a `ViewData` dictionary property that can be used to pass data to a View.</span></span> <span data-ttu-id="9c975-152">Des objets sont ajoutés au dictionnaire `ViewData` à l’aide d’un modèle clé/valeur.</span><span class="sxs-lookup"><span data-stu-id="9c975-152">Objects are added to the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="9c975-153">Dans l’exemple précédent, la propriété `"Title"` est ajoutée au dictionnaire `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="9c975-153">In the preceding sample, the `"Title"` property is added to the `ViewData` dictionary.</span></span>

<span data-ttu-id="9c975-154">La propriété `"Title"` est utilisée dans le fichier  *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9c975-154">The `"Title"` property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="9c975-155">La balise suivante montre les premières lignes du fichier *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9c975-155">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6)]

<span data-ttu-id="9c975-156">La ligne `@*Markup removed for brevity.*@` est un commentaire Razor.</span><span class="sxs-lookup"><span data-stu-id="9c975-156">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="9c975-157">Contrairement aux commentaires HTML (`<!-- -->`), les commentaires Razor ne sont pas envoyés au client.</span><span class="sxs-lookup"><span data-stu-id="9c975-157">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="9c975-158">Mettre à jour la disposition</span><span class="sxs-lookup"><span data-stu-id="9c975-158">Update the layout</span></span>

<span data-ttu-id="9c975-159">Changez l’élément `<title>` dans le fichier *Pages/Shared/_Layout.cshtml* pour afficher **Movie** au lieu de **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="9c975-159">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="9c975-160">Recherchez l’élément anchor suivant dans le fichier *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9c975-160">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="9c975-161">Remplacez l’élément précédent par la balise suivante :</span><span class="sxs-lookup"><span data-stu-id="9c975-161">Replace the preceding element with the following markup:</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="9c975-162">L’élément anchor précédent est un [Tag Helper](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="9c975-162">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="9c975-163">Dans le cas présent, il s’agit du [Tag Helper d’ancre](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="9c975-163">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="9c975-164">L’attribut et la valeur du Tag Helper `asp-page="/Movies/Index"` créent un lien vers la page Razor `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="9c975-164">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="9c975-165">La valeur de l’attribut `asp-area` est vide : la zone n’est donc pas utilisée dans le lien.</span><span class="sxs-lookup"><span data-stu-id="9c975-165">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="9c975-166">Pour plus d’informations, consultez [Zones](xref:mvc/controllers/areas).</span><span class="sxs-lookup"><span data-stu-id="9c975-166">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="9c975-167">Enregistrez vos changements, puis testez l’application en cliquant sur le lien **RpMovie**.</span><span class="sxs-lookup"><span data-stu-id="9c975-167">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="9c975-168">Consultez le fichier [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml) dans GitHub si vous rencontrez des problèmes.</span><span class="sxs-lookup"><span data-stu-id="9c975-168">See the [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="9c975-169">Testez les autres liens (**Home**, **RpMovie**, **Create**, **Edit** et **Delete**).</span><span class="sxs-lookup"><span data-stu-id="9c975-169">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="9c975-170">Chaque page définit le titre, que vous pouvez voir dans l’onglet navigateur. Lorsque vous ajoutez une page à un signet, le titre est utilisé pour le signet.</span><span class="sxs-lookup"><span data-stu-id="9c975-170">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span>

> [!NOTE]
> <span data-ttu-id="9c975-171">Vous ne pourrez peut-être pas entrer de virgules décimales dans le champ `Price`.</span><span class="sxs-lookup"><span data-stu-id="9c975-171">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="9c975-172">Pour prendre en charge la [validation jQuery](https://jqueryvalidation.org/) pour les paramètres régionaux autres que l’anglais qui utilisent une virgule (« , ») comme décimale et des formats de date autres que l’anglais des États-Unis, vous devez effectuer des étapes pour localiser votre application.</span><span class="sxs-lookup"><span data-stu-id="9c975-172">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="9c975-173">Consultez cette page [GitHub problème 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) pour savoir comment ajouter une virgule décimale.</span><span class="sxs-lookup"><span data-stu-id="9c975-173">See this [GitHub issue 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="9c975-174">La propriété `Layout` est définie dans le fichier *Pages/_ViewStart.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="9c975-174">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/_ViewStart.cshtml)]

<span data-ttu-id="9c975-175">Le code précédent définit le fichier de disposition *Pages/Shared/_Layout.cshtml* pour tous les fichiers Razor du dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="9c975-175">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="9c975-176">Pour plus d’informations, consultez [Disposition](xref:razor-pages/index#layout).</span><span class="sxs-lookup"><span data-stu-id="9c975-176">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="9c975-177">Le modèle de page Create</span><span class="sxs-lookup"><span data-stu-id="9c975-177">The Create page model</span></span>

<span data-ttu-id="9c975-178">Examinez le modèle de page *Pages/Movies/Create.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="9c975-178">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="9c975-179">La méthode `OnGet` initialise l’état nécessaire pour la page.</span><span class="sxs-lookup"><span data-stu-id="9c975-179">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="9c975-180">La page Create n’ayant aucun état à initialiser, `Page` est retourné.</span><span class="sxs-lookup"><span data-stu-id="9c975-180">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="9c975-181">Plus loin dans le tutoriel, un exemple d’état d’initialisation `OnGet` est affiché.</span><span class="sxs-lookup"><span data-stu-id="9c975-181">Later in the tutorial, an example of `OnGet` initializing state is shown.</span></span> <span data-ttu-id="9c975-182">La méthode `Page` crée un objet `PageResult` qui affiche la page *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9c975-182">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="9c975-183">La propriété `Movie` utilise l’attribut `[BindProperty]` pour adhérer à la [liaison de données](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="9c975-183">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="9c975-184">Quand le formulaire Create publie les valeurs de formulaire, le runtime ASP.NET Core lie les valeurs publiées au modèle `Movie`.</span><span class="sxs-lookup"><span data-stu-id="9c975-184">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="9c975-185">La méthode `OnPostAsync` est exécutée quand la page publie les données de formulaire :</span><span class="sxs-lookup"><span data-stu-id="9c975-185">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="9c975-186">S’il existe des erreurs liées au modèle, le formulaire est réaffiché, ainsi que toutes les données de formulaire publiées.</span><span class="sxs-lookup"><span data-stu-id="9c975-186">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="9c975-187">La plupart des erreurs de modèle peuvent être interceptées côté client avant la publication du formulaire.</span><span class="sxs-lookup"><span data-stu-id="9c975-187">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="9c975-188">Voici un exemple d’erreur de modèle : la publication pour le champ de date d’une valeur qui ne peut pas être convertie en date.</span><span class="sxs-lookup"><span data-stu-id="9c975-188">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="9c975-189">La validation côté client et la validation du modèle sont présentées plus loin dans le tutoriel.</span><span class="sxs-lookup"><span data-stu-id="9c975-189">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="9c975-190">S’il n’existe pas d’erreurs de modèle, les données sont enregistrées et le navigateur est redirigé vers la page Index.</span><span class="sxs-lookup"><span data-stu-id="9c975-190">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="9c975-191">Page Razor Create</span><span class="sxs-lookup"><span data-stu-id="9c975-191">The Create Razor Page</span></span>

<span data-ttu-id="9c975-192">Examinez le fichier de la page Razor *Pages/Movies/Create.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="9c975-192">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9c975-193">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c975-193">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9c975-194">Visual Studio affiche la balise suivante dans une police différenciée en gras utilisée pour les Tag Helpers :</span><span class="sxs-lookup"><span data-stu-id="9c975-194">Visual Studio displays the following tags in a distinctive bold font used for Tag Helpers:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

![Vue VS17 de la page Create.cshtml](page/_static/th3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9c975-196">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9c975-196">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9c975-197">Les Tag Helpers suivants sont affichées dans la balise précédente :</span><span class="sxs-lookup"><span data-stu-id="9c975-197">The following Tag Helpers are shown in the preceding markup:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9c975-198">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="9c975-198">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9c975-199">Visual Studio affiche la balise suivante dans une police différenciée en gras utilisée pour les Tag Helpers :</span><span class="sxs-lookup"><span data-stu-id="9c975-199">Visual Studio displays the following tags in a distinctive bold font used for Tag Helpers:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

---

<span data-ttu-id="9c975-200">L’élément `<form method="post">` est un [Tag Helper de formulaire](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="9c975-200">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="9c975-201">Le Tag Helper de formulaire inclut automatiquement un [jeton de protection contre les falsifications](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="9c975-201">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="9c975-202">Le moteur de génération de modèles automatique crée le code Razor pour chaque champ du modèle (sauf l’ID) de la manière suivante :</span><span class="sxs-lookup"><span data-stu-id="9c975-202">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="9c975-203">Les [Tag Helpers de validation](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` et `<span asp-validation-for`) affichent des erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="9c975-203">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and `<span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="9c975-204">La validation est traitée de manière plus détaillée plus loin dans cette série.</span><span class="sxs-lookup"><span data-stu-id="9c975-204">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="9c975-205">Le [Tag Helper d’étiquette](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) génère la légende de l’étiquette et l’attribut `for` pour la propriété `Title`.</span><span class="sxs-lookup"><span data-stu-id="9c975-205">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="9c975-206">Le [Tag Helper d’entrée](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) utilise les attributs [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) et produit les attributs HTML nécessaires à la validation jQuery côté client.</span><span class="sxs-lookup"><span data-stu-id="9c975-206">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

<span data-ttu-id="9c975-207">Pour plus d’informations sur les Tag Helpers, comme `<form method="post">`, consultez [Tag Helpers dans ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="9c975-207">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9c975-208">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9c975-208">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9c975-209">[Précédent : ajout d’un modèle](xref:tutorials/razor-pages/model)
> [suivant : base de données](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="9c975-209">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Database](xref:tutorials/razor-pages/sql)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9c975-210">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9c975-210">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9c975-211">Ce didacticiel décrit les pages Razor créées par génération de modèles automatique au cours du [didacticiel précédent](xref:tutorials/razor-pages/model).</span><span class="sxs-lookup"><span data-stu-id="9c975-211">This tutorial examines the Razor Pages created by scaffolding in the [previous tutorial](xref:tutorials/razor-pages/model).</span></span>

<span data-ttu-id="9c975-212">[Affichez ou téléchargez](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) l’exemple de code.</span><span class="sxs-lookup"><span data-stu-id="9c975-212">[View or download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="9c975-213">Pages Create, Delete, Details et Edit</span><span class="sxs-lookup"><span data-stu-id="9c975-213">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="9c975-214">Examinez le modèle de page *Pages/Movies/Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="9c975-214">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="9c975-215">Les pages Razor sont dérivées de `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="9c975-215">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="9c975-216">Par convention, la classe dérivée de `PageModel` s’appelle `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="9c975-216">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="9c975-217">Le constructeur utilise l’[injection de dépendances](xref:fundamentals/dependency-injection) pour ajouter `RazorPagesMovieContext` à la page.</span><span class="sxs-lookup"><span data-stu-id="9c975-217">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="9c975-218">Toutes les pages obtenues par génération de modèles automatique suivent ce modèle.</span><span class="sxs-lookup"><span data-stu-id="9c975-218">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="9c975-219">Consultez [Code asynchrone](xref:data/ef-rp/intro#asynchronous-code) pour plus d’informations sur la programmation asynchrone avec Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9c975-219">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programming with Entity Framework.</span></span>

<span data-ttu-id="9c975-220">Quand une requête est effectuée pour la page, la méthode `OnGetAsync` retourne une liste de films à la page Razor.</span><span class="sxs-lookup"><span data-stu-id="9c975-220">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="9c975-221">`OnGetAsync` ou `OnGet` est appelé sur une page Razor pour initialiser l’état de la page.</span><span class="sxs-lookup"><span data-stu-id="9c975-221">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="9c975-222">Dans ce cas, `OnGetAsync` obtient une liste de films et les affiche.</span><span class="sxs-lookup"><span data-stu-id="9c975-222">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="9c975-223">Si `OnGet` retourne `void` ou que `OnGetAsync` retourne `Task`, aucune méthode de retour n’est utilisée.</span><span class="sxs-lookup"><span data-stu-id="9c975-223">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="9c975-224">Lorsque le type de retour est `IActionResult` ou `Task<IActionResult>`, une instruction de retour doit être spécifiée.</span><span class="sxs-lookup"><span data-stu-id="9c975-224">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="9c975-225">Par exemple, la méthode *Pages/Movies/Create.cshtml.cs* `OnPostAsync` :</span><span class="sxs-lookup"><span data-stu-id="9c975-225">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> <span data-ttu-id="9c975-226">Examinez la page Razor *Pages/Movies/Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="9c975-226">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="9c975-227">Razor peut passer du HTML au C# ou à des balises spécifiques à Razor.</span><span class="sxs-lookup"><span data-stu-id="9c975-227">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="9c975-228">Quand un symbole `@` est suivi d’un [mot clé réservé Razor](xref:mvc/views/razor#razor-reserved-keywords), il est converti en balise spécifique à Razor. Sinon, il est converti en C#.</span><span class="sxs-lookup"><span data-stu-id="9c975-228">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="9c975-229">La directive Razor `@page` transforme le fichier en une action MVC, ce qui lui permet de prendre en charge des requêtes.</span><span class="sxs-lookup"><span data-stu-id="9c975-229">The `@page` Razor directive makes the file into an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="9c975-230">`@page` doit être la première directive Razor sur une page.</span><span class="sxs-lookup"><span data-stu-id="9c975-230">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="9c975-231">`@page` est un exemple de conversion en balise spécifique à Razor.</span><span class="sxs-lookup"><span data-stu-id="9c975-231">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="9c975-232">Pour plus d’informations, consultez [Syntaxe Razor](xref:mvc/views/razor#razor-syntax).</span><span class="sxs-lookup"><span data-stu-id="9c975-232">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="9c975-233">Examinez l’expression lambda utilisée dans le HTML Helper suivant :</span><span class="sxs-lookup"><span data-stu-id="9c975-233">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title)
```

<span data-ttu-id="9c975-234">Le HTML Helper `DisplayNameFor` inspecte la propriété `Title` référencée dans l’expression lambda pour déterminer le nom d’affichage.</span><span class="sxs-lookup"><span data-stu-id="9c975-234">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="9c975-235">L’expression lambda est inspectée plutôt qu’évaluée.</span><span class="sxs-lookup"><span data-stu-id="9c975-235">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="9c975-236">Cela signifie qu’il n’existe pas de violation d’accès quand `model`, `model.Movie` ou `model.Movie[0]` ont une valeur `null` ou vide.</span><span class="sxs-lookup"><span data-stu-id="9c975-236">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="9c975-237">Quand l’expression lambda est évaluée (par exemple avec `@Html.DisplayFor(modelItem => item.Title)`), les valeurs de propriété du modèle sont évaluées.</span><span class="sxs-lookup"><span data-stu-id="9c975-237">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>

### <a name="the-model-directive"></a><span data-ttu-id="9c975-238">Directive @model</span><span class="sxs-lookup"><span data-stu-id="9c975-238">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="9c975-239">La directive `@model` spécifie le type du modèle passé à la page Razor.</span><span class="sxs-lookup"><span data-stu-id="9c975-239">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="9c975-240">Dans l’exemple précédent, la ligne `@model` rend la classe dérivée `PageModel` accessible à la page Razor.</span><span class="sxs-lookup"><span data-stu-id="9c975-240">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="9c975-241">Le modèle est utilisé dans les [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) `@Html.DisplayNameFor` et `@Html.DisplayFor` de la page.</span><span class="sxs-lookup"><span data-stu-id="9c975-241">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="9c975-242">La page de disposition</span><span class="sxs-lookup"><span data-stu-id="9c975-242">The layout page</span></span>

<span data-ttu-id="9c975-243">Sélectionnez les liens du menu (**RazorPagesMovie**, **Accueil** et **Confidentialité**).</span><span class="sxs-lookup"><span data-stu-id="9c975-243">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="9c975-244">Chaque page affiche la même disposition de menu.</span><span class="sxs-lookup"><span data-stu-id="9c975-244">Each page shows the same menu layout.</span></span> <span data-ttu-id="9c975-245">La disposition du menu est implémentée dans le fichier *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9c975-245">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="9c975-246">Ouvrez le fichier *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9c975-246">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="9c975-247">Les modèles de [disposition](xref:mvc/views/layout) vous permettent de spécifier la disposition du conteneur HTML de votre site dans un emplacement unique, puis de l’appliquer sur plusieurs pages de votre site.</span><span class="sxs-lookup"><span data-stu-id="9c975-247">[Layout](xref:mvc/views/layout) templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="9c975-248">Recherchez la ligne `@RenderBody()`.</span><span class="sxs-lookup"><span data-stu-id="9c975-248">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="9c975-249">`RenderBody` est un espace réservé dans lequel toutes les vues spécifiques à une page que vous créez s’affichent, *encapsulées* dans la page de disposition.</span><span class="sxs-lookup"><span data-stu-id="9c975-249">`RenderBody` is a placeholder where all the page-specific views you create show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="9c975-250">Par exemple, si vous sélectionnez le lien **Confidentialité**, la vue **Pages/Privacy.cshtml** est restituée dans la méthode `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="9c975-250">For example, if you select the **Privacy** link, the **Pages/Privacy.cshtml** view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>

### <a name="viewdata-and-layout"></a><span data-ttu-id="9c975-251">ViewData et disposition</span><span class="sxs-lookup"><span data-stu-id="9c975-251">ViewData and layout</span></span>

<span data-ttu-id="9c975-252">Considérez le code suivant du fichier *Pages/Movies/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9c975-252">Consider the following code from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="9c975-253">Le code précédent en surbrillance est un exemple de passage de Razor au C#.</span><span class="sxs-lookup"><span data-stu-id="9c975-253">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="9c975-254">Les caractères `{` et `}` délimitent un bloc de code C#.</span><span class="sxs-lookup"><span data-stu-id="9c975-254">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="9c975-255">La classe de base `PageModel` a une propriété de dictionnaire `ViewData` qui permet d’ajouter des données à passer à une vue.</span><span class="sxs-lookup"><span data-stu-id="9c975-255">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="9c975-256">Vous pouvez ajouter des objets au dictionnaire `ViewData` à l’aide d’un modèle clé/valeur.</span><span class="sxs-lookup"><span data-stu-id="9c975-256">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="9c975-257">Dans l’exemple précédent, la propriété « Title » est ajoutée au dictionnaire `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="9c975-257">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span>

<span data-ttu-id="9c975-258">La propriété « Title » est utilisée dans le fichier *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9c975-258">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="9c975-259">La balise suivante montre les premières lignes du fichier *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9c975-259">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

<span data-ttu-id="9c975-260">La ligne `@*Markup removed for brevity.*@` est un commentaire Razor qui n’apparaît pas dans votre fichier de disposition.</span><span class="sxs-lookup"><span data-stu-id="9c975-260">The line `@*Markup removed for brevity.*@` is a Razor comment which doesn't appear in your layout file.</span></span> <span data-ttu-id="9c975-261">Contrairement aux commentaires HTML (`<!-- -->`), les commentaires Razor ne sont pas envoyés au client.</span><span class="sxs-lookup"><span data-stu-id="9c975-261">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="9c975-262">Mettre à jour la disposition</span><span class="sxs-lookup"><span data-stu-id="9c975-262">Update the layout</span></span>

<span data-ttu-id="9c975-263">Changez l’élément `<title>` dans le fichier *Pages/Shared/_Layout.cshtml* pour afficher **Movie** au lieu de **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="9c975-263">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="9c975-264">Recherchez l’élément anchor suivant dans le fichier *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9c975-264">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="9c975-265">Remplacez l’élément précédent par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="9c975-265">Replace the preceding element with the following markup.</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="9c975-266">L’élément anchor précédent est un [Tag Helper](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="9c975-266">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="9c975-267">Dans le cas présent, il s’agit du [Tag Helper d’ancre](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="9c975-267">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="9c975-268">L’attribut et la valeur du Tag Helper `asp-page="/Movies/Index"` créent un lien vers la page Razor `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="9c975-268">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="9c975-269">La valeur de l’attribut `asp-area` est vide : la zone n’est donc pas utilisée dans le lien.</span><span class="sxs-lookup"><span data-stu-id="9c975-269">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="9c975-270">Pour plus d’informations, consultez [Zones](xref:mvc/controllers/areas).</span><span class="sxs-lookup"><span data-stu-id="9c975-270">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="9c975-271">Enregistrez vos changements, puis testez l’application en cliquant sur le lien **RpMovie**.</span><span class="sxs-lookup"><span data-stu-id="9c975-271">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="9c975-272">Consultez le fichier [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) dans GitHub si vous rencontrez des problèmes.</span><span class="sxs-lookup"><span data-stu-id="9c975-272">See the [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="9c975-273">Testez les autres liens (**Home**, **RpMovie**, **Create**, **Edit** et **Delete**).</span><span class="sxs-lookup"><span data-stu-id="9c975-273">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="9c975-274">Chaque page définit le titre, que vous pouvez voir dans l’onglet navigateur. Lorsque vous ajoutez une page à un signet, le titre est utilisé pour le signet.</span><span class="sxs-lookup"><span data-stu-id="9c975-274">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span>

> [!NOTE]
> <span data-ttu-id="9c975-275">Vous ne pourrez peut-être pas entrer de virgules décimales dans le champ `Price`.</span><span class="sxs-lookup"><span data-stu-id="9c975-275">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="9c975-276">Pour prendre en charge la [validation jQuery](https://jqueryvalidation.org/) pour les paramètres régionaux autres que l’anglais qui utilisent une virgule (« , ») comme décimale et des formats de date autres que l’anglais des États-Unis, vous devez effectuer des étapes pour localiser votre application.</span><span class="sxs-lookup"><span data-stu-id="9c975-276">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="9c975-277">Consultez la page [GitHub problème 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) pour savoir comment ajouter une virgule décimale.</span><span class="sxs-lookup"><span data-stu-id="9c975-277">This [GitHub issue 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="9c975-278">La propriété `Layout` est définie dans le fichier *Pages/_ViewStart.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="9c975-278">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

<span data-ttu-id="9c975-279">Le code précédent définit le fichier de disposition *Pages/Shared/_Layout.cshtml* pour tous les fichiers Razor du dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="9c975-279">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="9c975-280">Pour plus d’informations, consultez [Disposition](xref:razor-pages/index#layout).</span><span class="sxs-lookup"><span data-stu-id="9c975-280">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="9c975-281">Le modèle de page Create</span><span class="sxs-lookup"><span data-stu-id="9c975-281">The Create page model</span></span>

<span data-ttu-id="9c975-282">Examinez le modèle de page *Pages/Movies/Create.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="9c975-282">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="9c975-283">La méthode `OnGet` initialise l’état nécessaire pour la page.</span><span class="sxs-lookup"><span data-stu-id="9c975-283">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="9c975-284">La page Create n’ayant aucun état à initialiser, `Page` est retourné.</span><span class="sxs-lookup"><span data-stu-id="9c975-284">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="9c975-285">Ce tutoriel illustre plus loin l’initialisation de l’état par la méthode `OnGet`.</span><span class="sxs-lookup"><span data-stu-id="9c975-285">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="9c975-286">La méthode `Page` crée un objet `PageResult` qui affiche la page *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9c975-286">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="9c975-287">La propriété `Movie` utilise l’attribut `[BindProperty]` pour adhérer à la [liaison de données](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="9c975-287">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="9c975-288">Quand le formulaire Create publie les valeurs de formulaire, le runtime ASP.NET Core lie les valeurs publiées au modèle `Movie`.</span><span class="sxs-lookup"><span data-stu-id="9c975-288">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="9c975-289">La méthode `OnPostAsync` est exécutée quand la page publie les données de formulaire :</span><span class="sxs-lookup"><span data-stu-id="9c975-289">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="9c975-290">S’il existe des erreurs liées au modèle, le formulaire est réaffiché, ainsi que toutes les données de formulaire publiées.</span><span class="sxs-lookup"><span data-stu-id="9c975-290">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="9c975-291">La plupart des erreurs de modèle peuvent être interceptées côté client avant la publication du formulaire.</span><span class="sxs-lookup"><span data-stu-id="9c975-291">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="9c975-292">Voici un exemple d’erreur de modèle : la publication pour le champ de date d’une valeur qui ne peut pas être convertie en date.</span><span class="sxs-lookup"><span data-stu-id="9c975-292">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="9c975-293">La validation côté client et la validation du modèle sont présentées plus loin dans le tutoriel.</span><span class="sxs-lookup"><span data-stu-id="9c975-293">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="9c975-294">S’il n’existe pas d’erreurs de modèle, les données sont enregistrées et le navigateur est redirigé vers la page Index.</span><span class="sxs-lookup"><span data-stu-id="9c975-294">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="9c975-295">Page Razor Create</span><span class="sxs-lookup"><span data-stu-id="9c975-295">The Create Razor Page</span></span>

<span data-ttu-id="9c975-296">Examinez le fichier de la page Razor *Pages/Movies/Create.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="9c975-296">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9c975-297">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c975-297">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9c975-298">Visual Studio affiche la balise `<form method="post">` dans une police différenciée en gras utilisée pour les Tag Helpers :</span><span class="sxs-lookup"><span data-stu-id="9c975-298">Visual Studio displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers:</span></span>

![Vue VS17 de la page Create.cshtml](page/_static/th.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9c975-300">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9c975-300">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9c975-301">Pour plus d’informations sur les Tag Helpers, comme `<form method="post">`, consultez [Tag Helpers dans ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="9c975-301">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9c975-302">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="9c975-302">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9c975-303">Visual Studio pour Mac affiche la balise `<form method="post">` dans une police différenciée en gras utilisée pour les Tag Helpers.</span><span class="sxs-lookup"><span data-stu-id="9c975-303">Visual Studio for Mac displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers.</span></span>

---

<span data-ttu-id="9c975-304">L’élément `<form method="post">` est un [Tag Helper de formulaire](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="9c975-304">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="9c975-305">Le Tag Helper de formulaire inclut automatiquement un [jeton de protection contre les falsifications](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="9c975-305">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="9c975-306">Le moteur de génération de modèles automatique crée le code Razor pour chaque champ du modèle (sauf l’ID) de la manière suivante :</span><span class="sxs-lookup"><span data-stu-id="9c975-306">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="9c975-307">Les [Tag Helpers de validation](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` et `<span asp-validation-for`) affichent des erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="9c975-307">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and `<span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="9c975-308">La validation est traitée de manière plus détaillée plus loin dans cette série.</span><span class="sxs-lookup"><span data-stu-id="9c975-308">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="9c975-309">Le [Tag Helper d’étiquette](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) génère la légende de l’étiquette et l’attribut `for` pour la propriété `Title`.</span><span class="sxs-lookup"><span data-stu-id="9c975-309">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="9c975-310">Le [Tag Helper d’entrée](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) utilise les attributs [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) et produit les attributs HTML nécessaires à la validation jQuery côté client.</span><span class="sxs-lookup"><span data-stu-id="9c975-310">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9c975-311">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9c975-311">Additional resources</span></span>

* [<span data-ttu-id="9c975-312">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="9c975-312">YouTube version of this tutorial</span></span>](https://youtu.be/zxgKjPYnOMM)

> [!div class="step-by-step"]
> <span data-ttu-id="9c975-313">[Précédent : ajout d’un modèle](xref:tutorials/razor-pages/model)
> [suivant : base de données](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="9c975-313">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Database](xref:tutorials/razor-pages/sql)</span></span>

::: moniker-end
