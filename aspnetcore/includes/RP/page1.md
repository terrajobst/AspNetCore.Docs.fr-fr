# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="55225-101">Pages Razor obtenues par génération de modèles automatique dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="55225-101">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="55225-102">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="55225-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="55225-103">Ce didacticiel décrit les pages Razor créées par génération de modèles automatique dans le didacticiel précédent.</span><span class="sxs-lookup"><span data-stu-id="55225-103">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial.</span></span> 

<span data-ttu-id="55225-104">[Affichez ou téléchargez](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) l’exemple.</span><span class="sxs-lookup"><span data-stu-id="55225-104">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="55225-105">Pages Create, Delete, Details et Edit.</span><span class="sxs-lookup"><span data-stu-id="55225-105">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="55225-106">Examinez le modèle de page *Pages/Movies/Index.cshtml.cs* : [!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="55225-106">Examine the *Pages/Movies/Index.cshtml.cs* Page Model: [!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span></span>

<span data-ttu-id="55225-107">Les pages Razor sont dérivées de `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="55225-107">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="55225-108">Par convention, la classe dérivée de `PageModel` s’appelle `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="55225-108">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="55225-109">Le constructeur utilise l’[injection de dépendances](xref:fundamentals/dependency-injection) pour ajouter `MovieContext` à la page.</span><span class="sxs-lookup"><span data-stu-id="55225-109">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="55225-110">Toutes les pages obtenues par génération de modèles automatique suivent ce modèle.</span><span class="sxs-lookup"><span data-stu-id="55225-110">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="55225-111">Consultez [Code asynchrone](xref:data/ef-rp/intro#asynchronous-code) pour plus d’informations sur la programmation asynchrone avec Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="55225-111">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="55225-112">Quand une requête est effectuée pour la page, la méthode `OnGetAsync` retourne une liste de films à la page Razor.</span><span class="sxs-lookup"><span data-stu-id="55225-112">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="55225-113">`OnGetAsync` ou `OnGet` est appelé sur une page Razor pour initialiser l’état de la page.</span><span class="sxs-lookup"><span data-stu-id="55225-113">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="55225-114">Dans ce cas, `OnGetAsync` obtient une liste de films et les affiche.</span><span class="sxs-lookup"><span data-stu-id="55225-114">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span> 

<span data-ttu-id="55225-115">Si `OnGet` retourne `void` ou que `OnGetAsync` retourne `Task`, aucune méthode de retour n’est utilisée.</span><span class="sxs-lookup"><span data-stu-id="55225-115">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="55225-116">Lorsque le type de retour est `IActionResult` ou `Task<IActionResult>`, une instruction de retour doit être spécifiée.</span><span class="sxs-lookup"><span data-stu-id="55225-116">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="55225-117">Par exemple, la méthode *Pages/Movies/Create.cshtml.cs* `OnPostAsync` :</span><span class="sxs-lookup"><span data-stu-id="55225-117">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

<!-- TODO - replace with snippet
[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
 -->

```csharp
public async Task<IActionResult> OnPostAsync()
{
    if (!ModelState.IsValid)
    {
        return Page();
    }

    _context.Movie.Add(Movie);
    await _context.SaveChangesAsync();

    return RedirectToPage("./Index");
}
```
<span data-ttu-id="55225-118">Examinez la page Razor *Pages/Movies/Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="55225-118">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="55225-119">Razor peut passer du HTML au C# ou à des balises spécifiques à Razor.</span><span class="sxs-lookup"><span data-stu-id="55225-119">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="55225-120">Quand un symbole `@` est suivi d’un [mot clé réservé Razor](xref:mvc/views/razor#razor-reserved-keywords), il est converti en balise spécifique à Razor. Sinon, il est converti en C#.</span><span class="sxs-lookup"><span data-stu-id="55225-120">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="55225-121">La directive Razor `@page` transforme le fichier en action MVC &mdash;, ce qui lui permet de prendre en charge les requêtes.</span><span class="sxs-lookup"><span data-stu-id="55225-121">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="55225-122">`@page` doit être la première directive Razor sur une page.</span><span class="sxs-lookup"><span data-stu-id="55225-122">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="55225-123">`@page` est un exemple de conversion en balise spécifique à Razor.</span><span class="sxs-lookup"><span data-stu-id="55225-123">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="55225-124">Pour plus d’informations, consultez [Syntaxe Razor](xref:mvc/views/razor#razor-syntax).</span><span class="sxs-lookup"><span data-stu-id="55225-124">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="55225-125">Examinez l’expression lambda utilisée dans le HTML Helper suivant :</span><span class="sxs-lookup"><span data-stu-id="55225-125">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="55225-126">Le HTML Helper `DisplayNameFor` inspecte la propriété `Title` référencée dans l’expression lambda pour déterminer le nom d’affichage.</span><span class="sxs-lookup"><span data-stu-id="55225-126">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="55225-127">L’expression lambda est inspectée plutôt qu’évaluée.</span><span class="sxs-lookup"><span data-stu-id="55225-127">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="55225-128">Cela signifie qu’il n’existe pas de violation d’accès quand `model`, `model.Movie` ou `model.Movie[0]` ont une valeur `null` ou vide.</span><span class="sxs-lookup"><span data-stu-id="55225-128">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="55225-129">Quand l’expression lambda est évaluée (par exemple avec `@Html.DisplayFor(modelItem => item.Title)`), les valeurs de propriété du modèle sont évaluées.</span><span class="sxs-lookup"><span data-stu-id="55225-129">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="55225-130">Directive @model </span><span class="sxs-lookup"><span data-stu-id="55225-130">The @model directive</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="55225-131">La directive `@model` spécifie le type du modèle passé à la page Razor.</span><span class="sxs-lookup"><span data-stu-id="55225-131">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="55225-132">Dans l’exemple précédent, la ligne `@model` rend la classe dérivée `PageModel` accessible à la page Razor.</span><span class="sxs-lookup"><span data-stu-id="55225-132">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="55225-133">Le modèle est utilisé dans les [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) `@Html.DisplayNameFor` et `@Html.DisplayName` de la page.</span><span class="sxs-lookup"><span data-stu-id="55225-133">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayName` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="55225-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData et disposition</span><span class="sxs-lookup"><span data-stu-id="55225-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="55225-135">Examinons le code ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="55225-135">Consider the following code:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="55225-136">Le code précédent en surbrillance est un exemple de passage de Razor au C#.</span><span class="sxs-lookup"><span data-stu-id="55225-136">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="55225-137">Les caractères `{` et `}` délimitent un bloc de code C#.</span><span class="sxs-lookup"><span data-stu-id="55225-137">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="55225-138">La classe de base `PageModel` a une propriété de dictionnaire `ViewData` qui permet d’ajouter des données à passer à une vue.</span><span class="sxs-lookup"><span data-stu-id="55225-138">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="55225-139">Vous pouvez ajouter des objets au dictionnaire `ViewData` à l’aide d’un modèle clé/valeur.</span><span class="sxs-lookup"><span data-stu-id="55225-139">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="55225-140">Dans l’exemple précédent, la propriété « Title » est ajoutée au dictionnaire `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="55225-140">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> <span data-ttu-id="55225-141">La propriété « Title » est utilisée dans le fichier *Pages/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="55225-141">The "Title" property is used in the *Pages/_Layout.cshtml* file.</span></span> <span data-ttu-id="55225-142">La balise suivante montre les premières lignes du fichier *Pages/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="55225-142">The following markup shows the first few lines of the *Pages/_Layout.cshtml* file.</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

<span data-ttu-id="55225-143">La ligne `@*Markup removed for brevity.*@` est un commentaire Razor.</span><span class="sxs-lookup"><span data-stu-id="55225-143">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="55225-144">Contrairement aux commentaires HTML (`<!-- -->`), les commentaires Razor ne sont pas envoyés au client.</span><span class="sxs-lookup"><span data-stu-id="55225-144">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="55225-145">Exécutez l’application, puis testez les liens du projet (**Home**, **About**, **Contact**, **Create**, **Edit** et **Delete**).</span><span class="sxs-lookup"><span data-stu-id="55225-145">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="55225-146">Chaque page définit le titre, que vous pouvez voir dans l’onglet du navigateur. Quand vous définissez un signet pour une page, le titre est affecté au signet.</span><span class="sxs-lookup"><span data-stu-id="55225-146">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="55225-147">*Pages/Index.cshtml* et *Pages/Movies/Index.cshtml* ont le même titre, mais vous pouvez les modifier pour avoir des valeurs distinctes.</span><span class="sxs-lookup"><span data-stu-id="55225-147">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

<span data-ttu-id="55225-148">La propriété `Layout` est définie dans le fichier *Pages/_ViewStart.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="55225-148">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="55225-149">Le code précédent définit le fichier de disposition *Pages/_Layout.cshtml* pour tous les fichiers Razor du dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="55225-149">The preceding markup sets the layout file to *Pages/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="55225-150">Pour plus d’informations, consultez [Disposition](xref:mvc/razor-pages/index#layout).</span><span class="sxs-lookup"><span data-stu-id="55225-150">See [Layout](xref:mvc/razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="55225-151">Mettre à jour la disposition</span><span class="sxs-lookup"><span data-stu-id="55225-151">Update the layout</span></span>

<span data-ttu-id="55225-152">Changez l’élément `<title>` du fichier *Pages/_Layout.cshtml* pour utiliser une chaîne plus courte.</span><span class="sxs-lookup"><span data-stu-id="55225-152">Change the `<title>` element in the *Pages/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="55225-153">Recherchez l’élément anchor suivant dans le fichier *Pages/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="55225-153">Find the following anchor element in the *Pages/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="55225-154">Remplacez l’élément précédent par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="55225-154">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="55225-155">L’élément anchor précédent est un [Tag Helper](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="55225-155">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="55225-156">Dans le cas présent, il s’agit du [Tag Helper d’ancre](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="55225-156">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="55225-157">L’attribut et la valeur du Tag Helper `asp-page="/Movies/Index"` créent un lien vers la page Razor `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="55225-157">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="55225-158">Enregistrez vos changements, puis testez l’application en cliquant sur le lien **RpMovie**.</span><span class="sxs-lookup"><span data-stu-id="55225-158">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="55225-159">Consultez le fichier [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) dans GitHub.</span><span class="sxs-lookup"><span data-stu-id="55225-159">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="55225-160">Le modèle de page Create</span><span class="sxs-lookup"><span data-stu-id="55225-160">The Create page model</span></span>

<span data-ttu-id="55225-161">Examinez le modèle de page *Pages/Movies/Create.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="55225-161">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="55225-162">La méthode `OnGet` initialise l’état nécessaire pour la page.</span><span class="sxs-lookup"><span data-stu-id="55225-162">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="55225-163">La page Create n’a aucun état à initialiser.</span><span class="sxs-lookup"><span data-stu-id="55225-163">The Create page doesn't have any state to initialize.</span></span> <span data-ttu-id="55225-164">La méthode `Page` crée un objet `PageResult` qui affiche la page *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="55225-164">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="55225-165">La propriété `Movie` utilise l’attribut `[BindProperty]` pour adhérer à la [liaison de données](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="55225-165">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="55225-166">Quand le formulaire Create publie les valeurs de formulaire, le runtime ASP.NET Core lie les valeurs publiées au modèle `Movie`.</span><span class="sxs-lookup"><span data-stu-id="55225-166">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="55225-167">La méthode `OnPostAsync` est exécutée quand la page publie les données de formulaire :</span><span class="sxs-lookup"><span data-stu-id="55225-167">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="55225-168">S’il existe des erreurs liées au modèle, le formulaire est réaffiché, ainsi que toutes les données de formulaire publiées.</span><span class="sxs-lookup"><span data-stu-id="55225-168">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="55225-169">La plupart des erreurs de modèle peuvent être interceptées côté client avant la publication du formulaire.</span><span class="sxs-lookup"><span data-stu-id="55225-169">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="55225-170">Voici un exemple d’erreur de modèle : la publication pour le champ de date d’une valeur qui ne peut pas être convertie en date.</span><span class="sxs-lookup"><span data-stu-id="55225-170">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="55225-171">Nous aborderons de manière plus détaillée la validation côté client et la validation du modèle plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="55225-171">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="55225-172">S’il n’existe pas d’erreurs de modèle, les données sont enregistrées et le navigateur est redirigé vers la page Index.</span><span class="sxs-lookup"><span data-stu-id="55225-172">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="55225-173">Page Razor Create</span><span class="sxs-lookup"><span data-stu-id="55225-173">The Create Razor Page</span></span>

<span data-ttu-id="55225-174">Examinez le fichier de la page Razor *Pages/Movies/Create.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="55225-174">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).


![VS17 view of Create.cshtml page](page/_static/th.png)
-->
