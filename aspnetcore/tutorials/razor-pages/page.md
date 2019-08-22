---
title: Pages Razor obtenues par génération de modèles automatique dans ASP.NET Core
author: rick-anderson
description: Décrit l’obtention de pages Razor par génération de modèles automatique.
ms.author: riande
ms.date: 08/17/2019
uid: tutorials/razor-pages/page
ms.openlocfilehash: 00a8458b9bee4d30c5774a980ff5c23fb8872737
ms.sourcegitcommit: 38cac2552029fc19428722bb204ff9e16eb94225
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/18/2019
ms.locfileid: "69573144"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="01823-103">Pages Razor obtenues par génération de modèles automatique dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="01823-103">Scaffolded Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="01823-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="01823-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="01823-105">Ce didacticiel décrit les pages Razor créées par génération de modèles automatique au cours du [didacticiel précédent](xref:tutorials/razor-pages/model).</span><span class="sxs-lookup"><span data-stu-id="01823-105">This tutorial examines the Razor Pages created by scaffolding in the [previous tutorial](xref:tutorials/razor-pages/model).</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="01823-106">Pages Create, Delete, Details et Edit</span><span class="sxs-lookup"><span data-stu-id="01823-106">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="01823-107">Examinez le modèle de page *Pages/Movies/Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="01823-107">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="01823-108">Les pages Razor sont dérivées de `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="01823-108">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="01823-109">Par convention, la classe dérivée de `PageModel` s’appelle `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="01823-109">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="01823-110">Le constructeur utilise l’[injection de dépendances](xref:fundamentals/dependency-injection) pour ajouter `RazorPagesMovieContext` à la page.</span><span class="sxs-lookup"><span data-stu-id="01823-110">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="01823-111">Toutes les pages obtenues par génération de modèles automatique suivent ce modèle.</span><span class="sxs-lookup"><span data-stu-id="01823-111">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="01823-112">Consultez [Code asynchrone](xref:data/ef-rp/intro#asynchronous-code) pour plus d’informations sur la programmation asynchrone avec Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="01823-112">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programming with Entity Framework.</span></span>

<span data-ttu-id="01823-113">Quand une requête est effectuée pour la page, la méthode `OnGetAsync` retourne une liste de films à la page Razor.</span><span class="sxs-lookup"><span data-stu-id="01823-113">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="01823-114">`OnGetAsync` ou `OnGet` est appelé sur une page Razor pour initialiser l’état de la page.</span><span class="sxs-lookup"><span data-stu-id="01823-114">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="01823-115">Dans ce cas, `OnGetAsync` obtient une liste de films et les affiche.</span><span class="sxs-lookup"><span data-stu-id="01823-115">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="01823-116">Si `OnGet` retourne `void` ou que `OnGetAsync` retourne `Task`, aucune méthode de retour n’est utilisée.</span><span class="sxs-lookup"><span data-stu-id="01823-116">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="01823-117">Lorsque le type de retour est `IActionResult` ou `Task<IActionResult>`, une instruction de retour doit être spécifiée.</span><span class="sxs-lookup"><span data-stu-id="01823-117">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="01823-118">Par exemple, la méthode *Pages/Movies/Create.cshtml.cs* `OnPostAsync` :</span><span class="sxs-lookup"><span data-stu-id="01823-118">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> <span data-ttu-id="01823-119">Examinez la page Razor *Pages/Movies/Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="01823-119">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml)]

<span data-ttu-id="01823-120">Razor peut passer du HTML au C# ou à des balises spécifiques à Razor.</span><span class="sxs-lookup"><span data-stu-id="01823-120">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="01823-121">Quand un symbole `@` est suivi d’un [mot clé réservé Razor](xref:mvc/views/razor#razor-reserved-keywords), il est converti en balise spécifique à Razor. Sinon, il est converti en C#.</span><span class="sxs-lookup"><span data-stu-id="01823-121">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="01823-122">La directive Razor `@page` transforme le fichier en une action MVC, ce qui lui permet de prendre en charge des requêtes.</span><span class="sxs-lookup"><span data-stu-id="01823-122">The `@page` Razor directive makes the file into an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="01823-123">`@page` doit être la première directive Razor sur une page.</span><span class="sxs-lookup"><span data-stu-id="01823-123">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="01823-124">`@page` est un exemple de conversion en balise spécifique à Razor.</span><span class="sxs-lookup"><span data-stu-id="01823-124">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="01823-125">Pour plus d’informations, consultez [Syntaxe Razor](xref:mvc/views/razor#razor-syntax).</span><span class="sxs-lookup"><span data-stu-id="01823-125">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="01823-126">Examinez l’expression lambda utilisée dans le HTML Helper suivant :</span><span class="sxs-lookup"><span data-stu-id="01823-126">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="01823-127">Le HTML Helper `DisplayNameFor` inspecte la propriété `Title` référencée dans l’expression lambda pour déterminer le nom d’affichage.</span><span class="sxs-lookup"><span data-stu-id="01823-127">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="01823-128">L’expression lambda est inspectée plutôt qu’évaluée.</span><span class="sxs-lookup"><span data-stu-id="01823-128">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="01823-129">Cela signifie qu’il n’existe pas de violation d’accès quand `model`, `model.Movie` ou `model.Movie[0]` ont une valeur `null` ou vide.</span><span class="sxs-lookup"><span data-stu-id="01823-129">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="01823-130">Quand l’expression lambda est évaluée (par exemple avec `@Html.DisplayFor(modelItem => item.Title)`), les valeurs de propriété du modèle sont évaluées.</span><span class="sxs-lookup"><span data-stu-id="01823-130">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>

### <a name="the-model-directive"></a><span data-ttu-id="01823-131">Directive @model</span><span class="sxs-lookup"><span data-stu-id="01823-131">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="01823-132">La directive `@model` spécifie le type du modèle passé à la page Razor.</span><span class="sxs-lookup"><span data-stu-id="01823-132">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="01823-133">Dans l’exemple précédent, la ligne `@model` rend la classe dérivée `PageModel` accessible à la page Razor.</span><span class="sxs-lookup"><span data-stu-id="01823-133">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="01823-134">Le modèle est utilisé dans les [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) `@Html.DisplayNameFor` et `@Html.DisplayFor` de la page.</span><span class="sxs-lookup"><span data-stu-id="01823-134">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="01823-135">La page de disposition</span><span class="sxs-lookup"><span data-stu-id="01823-135">The layout page</span></span>

<span data-ttu-id="01823-136">Sélectionnez les liens du menu (**RazorPagesMovie**, **Accueil** et **Confidentialité**).</span><span class="sxs-lookup"><span data-stu-id="01823-136">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="01823-137">Chaque page affiche la même disposition de menu.</span><span class="sxs-lookup"><span data-stu-id="01823-137">Each page shows the same menu layout.</span></span> <span data-ttu-id="01823-138">La disposition du menu est implémentée dans le fichier *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="01823-138">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="01823-139">Ouvrez le fichier *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="01823-139">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="01823-140">Les modèles de [Disposition](xref:mvc/views/layout) permettent que la disposition du conteneur HTML soit :</span><span class="sxs-lookup"><span data-stu-id="01823-140">[Layout](xref:mvc/views/layout) templates allow the HTML container layout to be:</span></span>

* <span data-ttu-id="01823-141">spécifiée à un seul endroit ;</span><span class="sxs-lookup"><span data-stu-id="01823-141">Specified in one place.</span></span>
* <span data-ttu-id="01823-142">appliquée à plusieurs pages du site.</span><span class="sxs-lookup"><span data-stu-id="01823-142">Applied in multiple pages in the site.</span></span>

<span data-ttu-id="01823-143">Recherchez la ligne `@RenderBody()`.</span><span class="sxs-lookup"><span data-stu-id="01823-143">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="01823-144">`RenderBody` est un espace réservé dans lequel toutes les vues propres à la page s’affichent, *encapsulées* dans la page de disposition.</span><span class="sxs-lookup"><span data-stu-id="01823-144">`RenderBody` is a placeholder where all the page-specific views show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="01823-145">Par exemple, sélectionnez le lien **Confidentialité** et la vue *Pages/Privacy.cshtml* est restituée dans la méthode `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="01823-145">For example, select the **Privacy** link and the *Pages/Privacy.cshtml* view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>

### <a name="viewdata-and-layout"></a><span data-ttu-id="01823-146">ViewData et disposition</span><span class="sxs-lookup"><span data-stu-id="01823-146">ViewData and layout</span></span>

<span data-ttu-id="01823-147">Considérez la balise suivante du fichier *Pages/Movies/Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="01823-147">Consider the following markup from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="01823-148">La balise précédente en surbrillance est un exemple de passage de Razor au C#.</span><span class="sxs-lookup"><span data-stu-id="01823-148">The preceding highlighted markup is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="01823-149">Les caractères `{` et `}` délimitent un bloc de code C#.</span><span class="sxs-lookup"><span data-stu-id="01823-149">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="01823-150">La classe de base `PageModel` contient une propriété de dictionnaire `ViewData` qui peut être utilisée pour ajouter des données et les passer à une vue.</span><span class="sxs-lookup"><span data-stu-id="01823-150">The `PageModel` base class contains a `ViewData` dictionary property that can be used to add data that and pass it to a View.</span></span> <span data-ttu-id="01823-151">Des objets sont ajoutés au dictionnaire `ViewData` à l’aide d’un modèle clé/valeur.</span><span class="sxs-lookup"><span data-stu-id="01823-151">Objects are added to the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="01823-152">Dans l’exemple précédent, la propriété `"Title"` est ajoutée au dictionnaire `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="01823-152">In the preceding sample, the `"Title"` property is added to the `ViewData` dictionary.</span></span>

<span data-ttu-id="01823-153">La propriété `"Title"` est utilisée dans le fichier  *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="01823-153">The `"Title"` property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="01823-154">La balise suivante montre les premières lignes du fichier *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="01823-154">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6)]

<span data-ttu-id="01823-155">La ligne `@*Markup removed for brevity.*@` est un commentaire Razor.</span><span class="sxs-lookup"><span data-stu-id="01823-155">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="01823-156">Contrairement aux commentaires HTML (`<!-- -->`), les commentaires Razor ne sont pas envoyés au client.</span><span class="sxs-lookup"><span data-stu-id="01823-156">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="01823-157">Mettre à jour la disposition</span><span class="sxs-lookup"><span data-stu-id="01823-157">Update the layout</span></span>

<span data-ttu-id="01823-158">Changez l’élément `<title>` dans le fichier *Pages/Shared/_Layout.cshtml* pour afficher **Movie** au lieu de **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="01823-158">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="01823-159">Recherchez l’élément anchor suivant dans le fichier *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="01823-159">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="01823-160">Remplacez l’élément précédent par la balise suivante :</span><span class="sxs-lookup"><span data-stu-id="01823-160">Replace the preceding element with the following markup:</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="01823-161">L’élément anchor précédent est un [Tag Helper](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="01823-161">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="01823-162">Dans le cas présent, il s’agit du [Tag Helper d’ancre](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="01823-162">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="01823-163">L’attribut et la valeur du Tag Helper `asp-page="/Movies/Index"` créent un lien vers la page Razor `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="01823-163">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="01823-164">La valeur de l’attribut `asp-area` est vide : la zone n’est donc pas utilisée dans le lien.</span><span class="sxs-lookup"><span data-stu-id="01823-164">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="01823-165">Pour plus d’informations, consultez [Zones](xref:mvc/controllers/areas).</span><span class="sxs-lookup"><span data-stu-id="01823-165">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="01823-166">Enregistrez vos changements, puis testez l’application en cliquant sur le lien **RpMovie**.</span><span class="sxs-lookup"><span data-stu-id="01823-166">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="01823-167">Consultez le fichier [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml) dans GitHub si vous rencontrez des problèmes.</span><span class="sxs-lookup"><span data-stu-id="01823-167">See the [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="01823-168">Testez les autres liens (**Home**, **RpMovie**, **Create**, **Edit** et **Delete**).</span><span class="sxs-lookup"><span data-stu-id="01823-168">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="01823-169">Chaque page définit le titre, que vous pouvez voir dans l’onglet du navigateur. Quand vous définissez un signet pour une page, le titre est affecté au signet.</span><span class="sxs-lookup"><span data-stu-id="01823-169">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span>

> [!NOTE]
> <span data-ttu-id="01823-170">Vous ne pourrez peut-être pas entrer de virgules décimales dans le champ `Price`.</span><span class="sxs-lookup"><span data-stu-id="01823-170">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="01823-171">Pour prendre en charge la [validation jQuery](https://jqueryvalidation.org/) pour les paramètres régionaux autres que l’anglais qui utilisent une virgule (« , ») comme décimale et des formats de date autres que l’anglais des États-Unis, vous devez effectuer des étapes pour localiser votre application.</span><span class="sxs-lookup"><span data-stu-id="01823-171">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="01823-172">Consultez cette page [GitHub problème 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) pour savoir comment ajouter une virgule décimale.</span><span class="sxs-lookup"><span data-stu-id="01823-172">See this [GitHub issue 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="01823-173">La propriété `Layout` est définie dans le fichier *Pages/_ViewStart.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="01823-173">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/_ViewStart.cshtml)]

<span data-ttu-id="01823-174">Le code précédent définit le fichier de disposition *Pages/Shared/_Layout.cshtml* pour tous les fichiers Razor du dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="01823-174">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="01823-175">Pour plus d’informations, consultez [Disposition](xref:razor-pages/index#layout).</span><span class="sxs-lookup"><span data-stu-id="01823-175">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="01823-176">Le modèle de page Create</span><span class="sxs-lookup"><span data-stu-id="01823-176">The Create page model</span></span>

<span data-ttu-id="01823-177">Examinez le modèle de page *Pages/Movies/Create.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="01823-177">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="01823-178">La méthode `OnGet` initialise l’état nécessaire pour la page.</span><span class="sxs-lookup"><span data-stu-id="01823-178">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="01823-179">La page Create n’ayant aucun état à initialiser, `Page` est retourné.</span><span class="sxs-lookup"><span data-stu-id="01823-179">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="01823-180">Plus loin dans le tutoriel, un exemple d’état d’initialisation `OnGet` est affiché.</span><span class="sxs-lookup"><span data-stu-id="01823-180">Later in the tutorial, an example of `OnGet` initializing state is shown.</span></span> <span data-ttu-id="01823-181">La méthode `Page` crée un objet `PageResult` qui affiche la page *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="01823-181">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="01823-182">La propriété `Movie` utilise l’attribut `[BindProperty]` pour adhérer à la [liaison de données](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="01823-182">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="01823-183">Quand le formulaire Create publie les valeurs de formulaire, le runtime ASP.NET Core lie les valeurs publiées au modèle `Movie`.</span><span class="sxs-lookup"><span data-stu-id="01823-183">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="01823-184">La méthode `OnPostAsync` est exécutée quand la page publie les données de formulaire :</span><span class="sxs-lookup"><span data-stu-id="01823-184">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="01823-185">S’il existe des erreurs liées au modèle, le formulaire est réaffiché, ainsi que toutes les données de formulaire publiées.</span><span class="sxs-lookup"><span data-stu-id="01823-185">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="01823-186">La plupart des erreurs de modèle peuvent être interceptées côté client avant la publication du formulaire.</span><span class="sxs-lookup"><span data-stu-id="01823-186">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="01823-187">Voici un exemple d’erreur de modèle : la publication pour le champ de date d’une valeur qui ne peut pas être convertie en date.</span><span class="sxs-lookup"><span data-stu-id="01823-187">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="01823-188">La validation côté client et la validation du modèle sont présentées plus loin dans le tutoriel.</span><span class="sxs-lookup"><span data-stu-id="01823-188">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="01823-189">S’il n’existe pas d’erreurs de modèle, les données sont enregistrées et le navigateur est redirigé vers la page Index.</span><span class="sxs-lookup"><span data-stu-id="01823-189">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="01823-190">Page Razor Create</span><span class="sxs-lookup"><span data-stu-id="01823-190">The Create Razor Page</span></span>

<span data-ttu-id="01823-191">Examinez le fichier de la page Razor *Pages/Movies/Create.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="01823-191">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="01823-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01823-192">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="01823-193">Visual Studio affiche la balise suivante dans une police différenciée en gras utilisée pour les Tag Helpers :</span><span class="sxs-lookup"><span data-stu-id="01823-193">Visual Studio displays the following tags in a distinctive bold font used for Tag Helpers:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

![Vue VS17 de la page Create.cshtml](page/_static/th3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="01823-195">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="01823-195">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="01823-196">Les Tag Helpers suivants sont affichées dans la balise précédente :</span><span class="sxs-lookup"><span data-stu-id="01823-196">The following Tag Helpers are shown in the preceding markup:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="01823-197">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="01823-197">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="01823-198">Visual Studio affiche la balise suivante dans une police différenciée en gras utilisée pour les Tag Helpers :</span><span class="sxs-lookup"><span data-stu-id="01823-198">Visual Studio displays the following tags in a distinctive bold font used for Tag Helpers:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

---

<span data-ttu-id="01823-199">L’élément `<form method="post">` est un [Tag Helper de formulaire](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="01823-199">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="01823-200">Le Tag Helper de formulaire inclut automatiquement un [jeton de protection contre les falsifications](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="01823-200">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="01823-201">Le moteur de génération de modèles automatique crée le code Razor pour chaque champ du modèle (sauf l’ID) de la manière suivante :</span><span class="sxs-lookup"><span data-stu-id="01823-201">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="01823-202">Les [Tag Helpers de validation](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` et `<span asp-validation-for`) affichent des erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="01823-202">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and `<span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="01823-203">La validation est traitée de manière plus détaillée plus loin dans cette série.</span><span class="sxs-lookup"><span data-stu-id="01823-203">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="01823-204">Le [Tag Helper d’étiquette](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) génère la légende de l’étiquette et l’attribut `for` pour la propriété `Title`.</span><span class="sxs-lookup"><span data-stu-id="01823-204">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="01823-205">Le [Tag Helper d’entrée](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) utilise les attributs [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) et produit les attributs HTML nécessaires à la validation jQuery côté client.</span><span class="sxs-lookup"><span data-stu-id="01823-205">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

<span data-ttu-id="01823-206">Pour plus d’informations sur les Tag Helpers, comme `<form method="post">`, consultez [Tag Helpers dans ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="01823-206">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="01823-207">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="01823-207">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="01823-208">[Précédent : Ajout d’un modèle](xref:tutorials/razor-pages/model)
> [Suivant : Base de données](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="01823-208">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Database](xref:tutorials/razor-pages/sql)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="01823-209">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="01823-209">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="01823-210">Ce didacticiel décrit les pages Razor créées par génération de modèles automatique au cours du [didacticiel précédent](xref:tutorials/razor-pages/model).</span><span class="sxs-lookup"><span data-stu-id="01823-210">This tutorial examines the Razor Pages created by scaffolding in the [previous tutorial](xref:tutorials/razor-pages/model).</span></span>

<span data-ttu-id="01823-211">[Affichez ou téléchargez](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) l’exemple.</span><span class="sxs-lookup"><span data-stu-id="01823-211">[View or download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="01823-212">Pages Create, Delete, Details et Edit</span><span class="sxs-lookup"><span data-stu-id="01823-212">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="01823-213">Examinez le modèle de page *Pages/Movies/Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="01823-213">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="01823-214">Les pages Razor sont dérivées de `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="01823-214">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="01823-215">Par convention, la classe dérivée de `PageModel` s’appelle `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="01823-215">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="01823-216">Le constructeur utilise l’[injection de dépendances](xref:fundamentals/dependency-injection) pour ajouter `RazorPagesMovieContext` à la page.</span><span class="sxs-lookup"><span data-stu-id="01823-216">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="01823-217">Toutes les pages obtenues par génération de modèles automatique suivent ce modèle.</span><span class="sxs-lookup"><span data-stu-id="01823-217">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="01823-218">Consultez [Code asynchrone](xref:data/ef-rp/intro#asynchronous-code) pour plus d’informations sur la programmation asynchrone avec Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="01823-218">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programming with Entity Framework.</span></span>

<span data-ttu-id="01823-219">Quand une requête est effectuée pour la page, la méthode `OnGetAsync` retourne une liste de films à la page Razor.</span><span class="sxs-lookup"><span data-stu-id="01823-219">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="01823-220">`OnGetAsync` ou `OnGet` est appelé sur une page Razor pour initialiser l’état de la page.</span><span class="sxs-lookup"><span data-stu-id="01823-220">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="01823-221">Dans ce cas, `OnGetAsync` obtient une liste de films et les affiche.</span><span class="sxs-lookup"><span data-stu-id="01823-221">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="01823-222">Si `OnGet` retourne `void` ou que `OnGetAsync` retourne `Task`, aucune méthode de retour n’est utilisée.</span><span class="sxs-lookup"><span data-stu-id="01823-222">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="01823-223">Lorsque le type de retour est `IActionResult` ou `Task<IActionResult>`, une instruction de retour doit être spécifiée.</span><span class="sxs-lookup"><span data-stu-id="01823-223">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="01823-224">Par exemple, la méthode *Pages/Movies/Create.cshtml.cs* `OnPostAsync` :</span><span class="sxs-lookup"><span data-stu-id="01823-224">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> <span data-ttu-id="01823-225">Examinez la page Razor *Pages/Movies/Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="01823-225">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="01823-226">Razor peut passer du HTML au C# ou à des balises spécifiques à Razor.</span><span class="sxs-lookup"><span data-stu-id="01823-226">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="01823-227">Quand un symbole `@` est suivi d’un [mot clé réservé Razor](xref:mvc/views/razor#razor-reserved-keywords), il est converti en balise spécifique à Razor. Sinon, il est converti en C#.</span><span class="sxs-lookup"><span data-stu-id="01823-227">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="01823-228">La directive Razor `@page` transforme le fichier en une action MVC, ce qui lui permet de prendre en charge des requêtes.</span><span class="sxs-lookup"><span data-stu-id="01823-228">The `@page` Razor directive makes the file into an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="01823-229">`@page` doit être la première directive Razor sur une page.</span><span class="sxs-lookup"><span data-stu-id="01823-229">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="01823-230">`@page` est un exemple de conversion en balise spécifique à Razor.</span><span class="sxs-lookup"><span data-stu-id="01823-230">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="01823-231">Pour plus d’informations, consultez [Syntaxe Razor](xref:mvc/views/razor#razor-syntax).</span><span class="sxs-lookup"><span data-stu-id="01823-231">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="01823-232">Examinez l’expression lambda utilisée dans le HTML Helper suivant :</span><span class="sxs-lookup"><span data-stu-id="01823-232">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="01823-233">Le HTML Helper `DisplayNameFor` inspecte la propriété `Title` référencée dans l’expression lambda pour déterminer le nom d’affichage.</span><span class="sxs-lookup"><span data-stu-id="01823-233">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="01823-234">L’expression lambda est inspectée plutôt qu’évaluée.</span><span class="sxs-lookup"><span data-stu-id="01823-234">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="01823-235">Cela signifie qu’il n’existe pas de violation d’accès quand `model`, `model.Movie` ou `model.Movie[0]` ont une valeur `null` ou vide.</span><span class="sxs-lookup"><span data-stu-id="01823-235">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="01823-236">Quand l’expression lambda est évaluée (par exemple avec `@Html.DisplayFor(modelItem => item.Title)`), les valeurs de propriété du modèle sont évaluées.</span><span class="sxs-lookup"><span data-stu-id="01823-236">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>

### <a name="the-model-directive"></a><span data-ttu-id="01823-237">Directive @model</span><span class="sxs-lookup"><span data-stu-id="01823-237">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="01823-238">La directive `@model` spécifie le type du modèle passé à la page Razor.</span><span class="sxs-lookup"><span data-stu-id="01823-238">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="01823-239">Dans l’exemple précédent, la ligne `@model` rend la classe dérivée `PageModel` accessible à la page Razor.</span><span class="sxs-lookup"><span data-stu-id="01823-239">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="01823-240">Le modèle est utilisé dans les [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) `@Html.DisplayNameFor` et `@Html.DisplayFor` de la page.</span><span class="sxs-lookup"><span data-stu-id="01823-240">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="01823-241">La page de disposition</span><span class="sxs-lookup"><span data-stu-id="01823-241">The layout page</span></span>

<span data-ttu-id="01823-242">Sélectionnez les liens du menu (**RazorPagesMovie**, **Accueil** et **Confidentialité**).</span><span class="sxs-lookup"><span data-stu-id="01823-242">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="01823-243">Chaque page affiche la même disposition de menu.</span><span class="sxs-lookup"><span data-stu-id="01823-243">Each page shows the same menu layout.</span></span> <span data-ttu-id="01823-244">La disposition du menu est implémentée dans le fichier *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="01823-244">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="01823-245">Ouvrez le fichier *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="01823-245">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="01823-246">Les modèles de [disposition](xref:mvc/views/layout) vous permettent de spécifier la disposition du conteneur HTML de votre site dans un emplacement unique, puis de l’appliquer sur plusieurs pages de votre site.</span><span class="sxs-lookup"><span data-stu-id="01823-246">[Layout](xref:mvc/views/layout) templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="01823-247">Recherchez la ligne `@RenderBody()`.</span><span class="sxs-lookup"><span data-stu-id="01823-247">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="01823-248">`RenderBody` est un espace réservé dans lequel toutes les vues spécifiques à une page que vous créez s’affichent, *encapsulées* dans la page de disposition.</span><span class="sxs-lookup"><span data-stu-id="01823-248">`RenderBody` is a placeholder where all the page-specific views you create show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="01823-249">Par exemple, si vous sélectionnez le lien **Confidentialité**, la vue **Pages/Privacy.cshtml** est restituée dans la méthode `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="01823-249">For example, if you select the **Privacy** link, the **Pages/Privacy.cshtml** view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>

### <a name="viewdata-and-layout"></a><span data-ttu-id="01823-250">ViewData et disposition</span><span class="sxs-lookup"><span data-stu-id="01823-250">ViewData and layout</span></span>

<span data-ttu-id="01823-251">Considérez le code suivant du fichier *Pages/Movies/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="01823-251">Consider the following code from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="01823-252">Le code précédent en surbrillance est un exemple de passage de Razor au C#.</span><span class="sxs-lookup"><span data-stu-id="01823-252">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="01823-253">Les caractères `{` et `}` délimitent un bloc de code C#.</span><span class="sxs-lookup"><span data-stu-id="01823-253">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="01823-254">La classe de base `PageModel` a une propriété de dictionnaire `ViewData` qui permet d’ajouter des données à passer à une vue.</span><span class="sxs-lookup"><span data-stu-id="01823-254">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="01823-255">Vous pouvez ajouter des objets au dictionnaire `ViewData` à l’aide d’un modèle clé/valeur.</span><span class="sxs-lookup"><span data-stu-id="01823-255">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="01823-256">Dans l’exemple précédent, la propriété « Title » est ajoutée au dictionnaire `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="01823-256">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span>

<span data-ttu-id="01823-257">La propriété « Title » est utilisée dans le fichier *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="01823-257">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="01823-258">La balise suivante montre les premières lignes du fichier *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="01823-258">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

<span data-ttu-id="01823-259">La ligne `@*Markup removed for brevity.*@` est un commentaire Razor qui n’apparaît pas dans votre fichier de disposition.</span><span class="sxs-lookup"><span data-stu-id="01823-259">The line `@*Markup removed for brevity.*@` is a Razor comment which doesn't appear in your layout file.</span></span> <span data-ttu-id="01823-260">Contrairement aux commentaires HTML (`<!-- -->`), les commentaires Razor ne sont pas envoyés au client.</span><span class="sxs-lookup"><span data-stu-id="01823-260">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="01823-261">Mettre à jour la disposition</span><span class="sxs-lookup"><span data-stu-id="01823-261">Update the layout</span></span>

<span data-ttu-id="01823-262">Changez l’élément `<title>` dans le fichier *Pages/Shared/_Layout.cshtml* pour afficher **Movie** au lieu de **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="01823-262">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="01823-263">Recherchez l’élément anchor suivant dans le fichier *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="01823-263">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="01823-264">Remplacez l’élément précédent par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="01823-264">Replace the preceding element with the following markup.</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="01823-265">L’élément anchor précédent est un [Tag Helper](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="01823-265">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="01823-266">Dans le cas présent, il s’agit du [Tag Helper d’ancre](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="01823-266">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="01823-267">L’attribut et la valeur du Tag Helper `asp-page="/Movies/Index"` créent un lien vers la page Razor `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="01823-267">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="01823-268">La valeur de l’attribut `asp-area` est vide : la zone n’est donc pas utilisée dans le lien.</span><span class="sxs-lookup"><span data-stu-id="01823-268">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="01823-269">Pour plus d’informations, consultez [Zones](xref:mvc/controllers/areas).</span><span class="sxs-lookup"><span data-stu-id="01823-269">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="01823-270">Enregistrez vos changements, puis testez l’application en cliquant sur le lien **RpMovie**.</span><span class="sxs-lookup"><span data-stu-id="01823-270">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="01823-271">Consultez le fichier [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) dans GitHub si vous rencontrez des problèmes.</span><span class="sxs-lookup"><span data-stu-id="01823-271">See the [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="01823-272">Testez les autres liens (**Home**, **RpMovie**, **Create**, **Edit** et **Delete**).</span><span class="sxs-lookup"><span data-stu-id="01823-272">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="01823-273">Chaque page définit le titre, que vous pouvez voir dans l’onglet du navigateur. Quand vous définissez un signet pour une page, le titre est affecté au signet.</span><span class="sxs-lookup"><span data-stu-id="01823-273">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span>

> [!NOTE]
> <span data-ttu-id="01823-274">Vous ne pourrez peut-être pas entrer de virgules décimales dans le champ `Price`.</span><span class="sxs-lookup"><span data-stu-id="01823-274">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="01823-275">Pour prendre en charge la [validation jQuery](https://jqueryvalidation.org/) pour les paramètres régionaux autres que l’anglais qui utilisent une virgule (« , ») comme décimale et des formats de date autres que l’anglais des États-Unis, vous devez effectuer des étapes pour localiser votre application.</span><span class="sxs-lookup"><span data-stu-id="01823-275">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="01823-276">Consultez la page [GitHub problème 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) pour savoir comment ajouter une virgule décimale.</span><span class="sxs-lookup"><span data-stu-id="01823-276">This [GitHub issue 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="01823-277">La propriété `Layout` est définie dans le fichier *Pages/_ViewStart.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="01823-277">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

<span data-ttu-id="01823-278">Le code précédent définit le fichier de disposition *Pages/Shared/_Layout.cshtml* pour tous les fichiers Razor du dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="01823-278">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="01823-279">Pour plus d’informations, consultez [Disposition](xref:razor-pages/index#layout).</span><span class="sxs-lookup"><span data-stu-id="01823-279">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="01823-280">Le modèle de page Create</span><span class="sxs-lookup"><span data-stu-id="01823-280">The Create page model</span></span>

<span data-ttu-id="01823-281">Examinez le modèle de page *Pages/Movies/Create.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="01823-281">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="01823-282">La méthode `OnGet` initialise l’état nécessaire pour la page.</span><span class="sxs-lookup"><span data-stu-id="01823-282">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="01823-283">La page Create n’ayant aucun état à initialiser, `Page` est retourné.</span><span class="sxs-lookup"><span data-stu-id="01823-283">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="01823-284">Ce tutoriel illustre plus loin l’initialisation de l’état par la méthode `OnGet`.</span><span class="sxs-lookup"><span data-stu-id="01823-284">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="01823-285">La méthode `Page` crée un objet `PageResult` qui affiche la page *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="01823-285">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="01823-286">La propriété `Movie` utilise l’attribut `[BindProperty]` pour adhérer à la [liaison de données](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="01823-286">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="01823-287">Quand le formulaire Create publie les valeurs de formulaire, le runtime ASP.NET Core lie les valeurs publiées au modèle `Movie`.</span><span class="sxs-lookup"><span data-stu-id="01823-287">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="01823-288">La méthode `OnPostAsync` est exécutée quand la page publie les données de formulaire :</span><span class="sxs-lookup"><span data-stu-id="01823-288">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="01823-289">S’il existe des erreurs liées au modèle, le formulaire est réaffiché, ainsi que toutes les données de formulaire publiées.</span><span class="sxs-lookup"><span data-stu-id="01823-289">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="01823-290">La plupart des erreurs de modèle peuvent être interceptées côté client avant la publication du formulaire.</span><span class="sxs-lookup"><span data-stu-id="01823-290">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="01823-291">Voici un exemple d’erreur de modèle : la publication pour le champ de date d’une valeur qui ne peut pas être convertie en date.</span><span class="sxs-lookup"><span data-stu-id="01823-291">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="01823-292">La validation côté client et la validation du modèle sont présentées plus loin dans le tutoriel.</span><span class="sxs-lookup"><span data-stu-id="01823-292">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="01823-293">S’il n’existe pas d’erreurs de modèle, les données sont enregistrées et le navigateur est redirigé vers la page Index.</span><span class="sxs-lookup"><span data-stu-id="01823-293">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="01823-294">Page Razor Create</span><span class="sxs-lookup"><span data-stu-id="01823-294">The Create Razor Page</span></span>

<span data-ttu-id="01823-295">Examinez le fichier de la page Razor *Pages/Movies/Create.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="01823-295">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="01823-296">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01823-296">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="01823-297">Visual Studio affiche la balise `<form method="post">` dans une police différenciée en gras utilisée pour les Tag Helpers :</span><span class="sxs-lookup"><span data-stu-id="01823-297">Visual Studio displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers:</span></span>

![Vue VS17 de la page Create.cshtml](page/_static/th.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="01823-299">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="01823-299">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="01823-300">Pour plus d’informations sur les Tag Helpers, comme `<form method="post">`, consultez [Tag Helpers dans ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="01823-300">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="01823-301">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="01823-301">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="01823-302">Visual Studio pour Mac affiche la balise `<form method="post">` dans une police différenciée en gras utilisée pour les Tag Helpers.</span><span class="sxs-lookup"><span data-stu-id="01823-302">Visual Studio for Mac displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers.</span></span>

---

<span data-ttu-id="01823-303">L’élément `<form method="post">` est un [Tag Helper de formulaire](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="01823-303">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="01823-304">Le Tag Helper de formulaire inclut automatiquement un [jeton de protection contre les falsifications](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="01823-304">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="01823-305">Le moteur de génération de modèles automatique crée le code Razor pour chaque champ du modèle (sauf l’ID) de la manière suivante :</span><span class="sxs-lookup"><span data-stu-id="01823-305">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="01823-306">Les [Tag Helpers de validation](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` et `<span asp-validation-for`) affichent des erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="01823-306">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and `<span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="01823-307">La validation est traitée de manière plus détaillée plus loin dans cette série.</span><span class="sxs-lookup"><span data-stu-id="01823-307">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="01823-308">Le [Tag Helper d’étiquette](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) génère la légende de l’étiquette et l’attribut `for` pour la propriété `Title`.</span><span class="sxs-lookup"><span data-stu-id="01823-308">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="01823-309">Le [Tag Helper d’entrée](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) utilise les attributs [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) et produit les attributs HTML nécessaires à la validation jQuery côté client.</span><span class="sxs-lookup"><span data-stu-id="01823-309">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="01823-310">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="01823-310">Additional resources</span></span>

* [<span data-ttu-id="01823-311">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="01823-311">YouTube version of this tutorial</span></span>](https://youtu.be/zxgKjPYnOMM)

> [!div class="step-by-step"]
> <span data-ttu-id="01823-312">[Précédent : Ajout d’un modèle](xref:tutorials/razor-pages/model)
> [Suivant : Base de données](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="01823-312">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Database](xref:tutorials/razor-pages/sql)</span></span>

::: moniker-end
