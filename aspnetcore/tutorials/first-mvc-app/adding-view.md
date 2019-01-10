---
title: Ajouter une vue à une application ASP.NET Core MVC
author: rick-anderson
description: Ajout d’une vue dans une application ASP.NET Core MVC simple
ms.author: riande
ms.date: 03/04/2017
uid: tutorials/first-mvc-app/adding-view
ms.openlocfilehash: 5267e5a49bb6ecdd4cef671989f111eae7a64ec4
ms.sourcegitcommit: 4e87712029de2aceb1cf2c52e9e3dda8195a5b8e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53381814"
---
# <a name="add-a-view-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="ff6e7-103">Ajouter une vue à une application ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="ff6e7-103">Add a view to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="ff6e7-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ff6e7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ff6e7-105">Dans cette section, vous modifiez la classe `HelloWorldController` de façon à utiliser les fichiers de [vue](xref:mvc/views/razor) Razor pour encapsuler proprement le processus de génération des réponses HTML à un client.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-105">In this section you modify the `HelloWorldController` class to use [Razor](xref:mvc/views/razor) view files to cleanly encapsulate the process of generating HTML responses to a client.</span></span>

<span data-ttu-id="ff6e7-106">Vous créez un fichier de modèle de vue à l’aide de Razor.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-106">You create a view template file using Razor.</span></span> <span data-ttu-id="ff6e7-107">Les modèles de vue Razor ont l’extension de fichier *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-107">Razor-based view templates have a *.cshtml* file extension.</span></span> <span data-ttu-id="ff6e7-108">Ils offrent un moyen élégant pour créer une sortie HTML avec C#.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-108">They provide an elegant way to create HTML output with C#.</span></span>

<span data-ttu-id="ff6e7-109">Actuellement, la méthode `Index` retourne une chaîne avec un message qui est codé en dur dans la classe du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-109">Currently the `Index` method returns a string with a message that's hard-coded in the controller class.</span></span> <span data-ttu-id="ff6e7-110">Dans la classe `HelloWorldController`, remplacez la méthode `Index` par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ff6e7-110">In the `HelloWorldController` class, replace the `Index` method with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

<span data-ttu-id="ff6e7-111">Le code précédent retourne un objet `View`.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-111">The preceding code returns a `View` object.</span></span> <span data-ttu-id="ff6e7-112">Il utilise un modèle de vue pour générer une réponse HTML au navigateur.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-112">It uses a view template to generate an HTML response to the browser.</span></span> <span data-ttu-id="ff6e7-113">Les méthodes du contrôleur (également appelées méthodes d’action), comme la méthode `Index` ci-dessus, retournent généralement un [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) (ou une classe dérivée de `ActionResult`), et non une chaîne ou un type similaire.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-113">Controller methods (also known as action methods) such as the `Index` method above, generally return an [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) (or a class derived from `ActionResult`), not a type like string.</span></span>

## <a name="add-a-view"></a><span data-ttu-id="ff6e7-114">Ajouter une vue</span><span class="sxs-lookup"><span data-stu-id="ff6e7-114">Add a view</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ff6e7-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ff6e7-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ff6e7-116">Cliquez avec le bouton droit sur le dossier *Vues*, cliquez sur **Ajouter > Nouveau dossier**, puis nommez le dossier *HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-116">Right click on the *Views* folder, and then **Add > New Folder** and name the folder *HelloWorld*.</span></span>

* <span data-ttu-id="ff6e7-117">Cliquez avec le bouton droit sur le dossier *Vues/HelloWorld*, puis cliquez sur **Ajouter > Nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-117">Right click on the *Views/HelloWorld* folder, and then **Add > New Item**.</span></span>

* <span data-ttu-id="ff6e7-118">Dans la boîte de dialogue **Ajouter un nouvel élément - MvcMovie**</span><span class="sxs-lookup"><span data-stu-id="ff6e7-118">In the **Add New Item - MvcMovie** dialog</span></span>

  * <span data-ttu-id="ff6e7-119">Dans la zone de recherche située en haut à droite, entrez *vue*</span><span class="sxs-lookup"><span data-stu-id="ff6e7-119">In the search box in the upper-right, enter *view*</span></span>

  * <span data-ttu-id="ff6e7-120">Sélectionnez **Vue Razor**</span><span class="sxs-lookup"><span data-stu-id="ff6e7-120">Select **Razor View**</span></span>

  * <span data-ttu-id="ff6e7-121">Conservez la valeur de la zone **Nom**, *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-121">Keep the **Name** box value, *Index.cshtml*.</span></span>

  * <span data-ttu-id="ff6e7-122">Sélectionnez **Ajouter**</span><span class="sxs-lookup"><span data-stu-id="ff6e7-122">Select **Add**</span></span>

![Boîte de dialogue Ajouter un nouvel élément](adding-view/_static/add_view.png)

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ff6e7-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ff6e7-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ff6e7-125">Ajoutez une vue `Index` pour `HelloWorldController`.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-125">Add an `Index` view for the `HelloWorldController`.</span></span>

* <span data-ttu-id="ff6e7-126">Ajoutez un nouveau dossier nommé *Views/HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-126">Add a new folder named *Views/HelloWorld*.</span></span>
* <span data-ttu-id="ff6e7-127">Ajoutez un nouveau fichier à la *vues/HelloWorld* nom du dossier *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-127">Add a new file to the *Views/HelloWorld* folder name *Index.cshtml*.</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ff6e7-128">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="ff6e7-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ff6e7-129">Cliquez avec le bouton droit sur le dossier *Vues*, cliquez sur **Ajouter > Nouveau dossier**, puis nommez le dossier *HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-129">Right click on the *Views* folder, and then **Add > New Folder** and name the folder *HelloWorld*.</span></span>
* <span data-ttu-id="ff6e7-130">Cliquez avec le bouton droit sur le dossier *Vues/HelloWorld*, puis cliquez sur **Ajouter > Nouveau fichier**.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-130">Right click on the *Views/HelloWorld* folder, and then **Add > New File**.</span></span>
* <span data-ttu-id="ff6e7-131">Dans la boîte de dialogue **Nouveau fichier** :</span><span class="sxs-lookup"><span data-stu-id="ff6e7-131">In the **New File** dialog:</span></span>

  * <span data-ttu-id="ff6e7-132">Sélectionnez **Web** dans le volet gauche.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-132">Select **Web** in the left pane.</span></span>
  * <span data-ttu-id="ff6e7-133">Sélectionnez **Fichier HTML vide** dans le volet central.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-133">Select **Empty HTML file** in the center pane.</span></span>
  * <span data-ttu-id="ff6e7-134">Tapez *Index.cshtml* dans la zone **Nom**.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-134">Type *Index.cshtml* in the **Name** box.</span></span>
  * <span data-ttu-id="ff6e7-135">Sélectionnez **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-135">Select **New**.</span></span>

![Boîte de dialogue Ajouter un nouvel élément](adding-view/_static/add_view.png)

---  
<!-- End of VS tabs -->

<span data-ttu-id="ff6e7-137">Remplacez le contenu du fichier vue Razor *Views/HelloWorld/Index.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ff6e7-137">Replace the contents of the *Views/HelloWorld/Index.cshtml* Razor view file with the following:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/HelloWorld/Index1.cshtml?highlight=7)]

<span data-ttu-id="ff6e7-138">Accédez à `https://localhost:xxxx/HelloWorld`.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-138">Navigate to `https://localhost:xxxx/HelloWorld`.</span></span> <span data-ttu-id="ff6e7-139">La méthode `Index` dans `HelloWorldController` n’a pas accompli beaucoup d’actions. Elle a exécuté l’instruction `return View();`, laquelle spécifiait que la méthode doit utiliser un fichier de modèle de vue pour restituer une réponse au navigateur.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-139">The `Index` method in the `HelloWorldController` didn't do much; it ran the statement `return View();`, which specified that the method should use a view template file to render a response to the browser.</span></span> <span data-ttu-id="ff6e7-140">Étant donné que vous n’avez pas explicitement spécifié le nom du fichier de modèle de vue, MVC a utilisé par défaut le fichier vue *Index.cshtml* présent dans le dossier */Views/HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-140">Because you didn't explicitly specify the name of the view template file, MVC defaulted to using the *Index.cshtml* view file in the */Views/HelloWorld* folder.</span></span> <span data-ttu-id="ff6e7-141">L’image ci-dessous montre la chaîne « Hello from our View Template! »</span><span class="sxs-lookup"><span data-stu-id="ff6e7-141">The image below shows the string "Hello from our View Template!"</span></span> <span data-ttu-id="ff6e7-142">codée en dur dans la vue.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-142">hard-coded in the view.</span></span>

![Fenêtre du navigateur](~/tutorials/first-mvc-app/adding-view/_static/hell_template.png)

## <a name="change-views-and-layout-pages"></a><span data-ttu-id="ff6e7-144">Changer les vues et les pages de disposition</span><span class="sxs-lookup"><span data-stu-id="ff6e7-144">Change views and layout pages</span></span>

<span data-ttu-id="ff6e7-145">Sélectionnez les liens du menu (**MvcMovie**, **Accueil** et **Confidentialité**).</span><span class="sxs-lookup"><span data-stu-id="ff6e7-145">Select the menu links (**MvcMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="ff6e7-146">Chaque page affiche la même disposition de menu.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-146">Each page shows the same menu layout.</span></span> <span data-ttu-id="ff6e7-147">La disposition du menu est implémentée dans le fichier *Views/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-147">The menu layout is implemented in the *Views/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="ff6e7-148">Ouvrez le fichier *Views/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-148">Open the *Views/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="ff6e7-149">Les modèles de [disposition](xref:mvc/views/layout) vous permettent de spécifier la disposition du conteneur HTML de votre site dans un emplacement unique, puis de l’appliquer sur plusieurs pages de votre site.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-149">[Layout](xref:mvc/views/layout) templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="ff6e7-150">Recherchez la ligne `@RenderBody()`.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-150">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="ff6e7-151">`RenderBody` est un espace réservé dans lequel toutes les pages spécifiques aux vues que vous créez s’affichent, *encapsulées* dans la page de disposition.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-151">`RenderBody` is a placeholder where all the view-specific pages you create show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="ff6e7-152">Par exemple, si vous sélectionnez le lien **Confidentialité**, la vue **Views/Home/Privacy.cshtml** est restituée dans la méthode `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-152">For example, if you select the **Privacy** link, the **Views/Home/Privacy.cshtml** view is rendered inside the `RenderBody` method.</span></span>

## <a name="change-the-title-and-menu-link-in-the-layout-file"></a><span data-ttu-id="ff6e7-153">Changer le lien de titre et de menu dans le fichier de disposition</span><span class="sxs-lookup"><span data-stu-id="ff6e7-153">Change the title and menu link in the layout file</span></span>

* <span data-ttu-id="ff6e7-154">Dans l’élément de titre, remplacez `MvcMovie` par `Movie App`.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-154">In the title element, change `MvcMovie` to `Movie App`.</span></span>
* <span data-ttu-id="ff6e7-155">Modifier l’élément d’ancrage `<a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">MvcMovie</a>` par `<a class="navbar-brand" asp-controller="Movies" asp-action="Index">Movie App</a>`.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-155">Change the anchor element `<a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">MvcMovie</a>` to `<a class="navbar-brand" asp-controller="Movies" asp-action="Index">Movie App</a>`.</span></span>

<span data-ttu-id="ff6e7-156">Le balisage suivant illustre les changements en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="ff6e7-156">The following markup shows the highlighted changes:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Shared/_Layout.cshtml?highlight=6,24)]

<span data-ttu-id="ff6e7-157">Dans le balisage précédent, l’[attribut Tag Helper Ancre](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) `asp-area` a été omis, car cette application n’utilise pas de [zones](xref:mvc/controllers/areas).</span><span class="sxs-lookup"><span data-stu-id="ff6e7-157">In the preceding markup, the `asp-area` [anchor Tag Helper attribute](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) was omitted because this app is not using [Areas](xref:mvc/controllers/areas).</span></span>

<!-- Routing has changed in 2.2, it's going to the last route.
>[!WARNING]
> We haven't implemented the `Movies` controller yet, so if you click the `Movie App` link, you get a 404 (Not found) error.
-->

<span data-ttu-id="ff6e7-158">**Remarque** : Le contrôleur `Movies` n’a pas encore été implémenté.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-158">**Note**: The `Movies` controller has not been implemented.</span></span> <span data-ttu-id="ff6e7-159">À ce stade, le lien `Movie App` ne fonctionne pas.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-159">At this point, the `Movie App` link is not functional.</span></span>

<span data-ttu-id="ff6e7-160">Enregistrer vos modifications et sélectionnez le lien **Confidentialité**.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-160">Save your changes and select the **Privacy** link.</span></span> <span data-ttu-id="ff6e7-161">Notez comment le titre sur l’onglet du navigateur affiche **Confidentialité - Movie App** au lieu de **Confidentialité - Mvc Movie** :</span><span class="sxs-lookup"><span data-stu-id="ff6e7-161">Notice how the title on the browser tab displays **Privacy - Movie App** instead of **Privacy - Mvc Movie**:</span></span>

![Onglet Confidentialité](~/tutorials/first-mvc-app/adding-view/_static/about2.png)

<span data-ttu-id="ff6e7-163">Sélectionnez le lien **Accueil** et notez que le titre et le texte d’ancrage affichent également **Movie App**.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-163">Select the **Home** link and notice that the title and anchor text also display **Movie App**.</span></span> <span data-ttu-id="ff6e7-164">Nous avons pu effectuer ce changement une fois dans le modèle de disposition et avoir le nouveau texte de lien et le nouveau titre reflétés sur toutes les pages du site.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-164">We were able to make the change once in the layout template and have all pages on the site reflect the new link text and new title.</span></span>

<span data-ttu-id="ff6e7-165">Examinez le fichier *Views/_ViewStart.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="ff6e7-165">Examine the *Views/_ViewStart.cshtml* file:</span></span>

```HTML
@{
    Layout = "_Layout";
}
```

<span data-ttu-id="ff6e7-166">Le fichier *Views/_ViewStart.cshtml* introduit le fichier *Views/Shared/_Layout.cshtml* dans chaque vue.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-166">The *Views/_ViewStart.cshtml* file brings in the *Views/Shared/_Layout.cshtml* file to each view.</span></span> <span data-ttu-id="ff6e7-167">La propriété `Layout` peut être utilisée pour définir un mode de disposition différent ou lui affecter la valeur `null`. Aucun fichier de disposition n’est donc utilisé.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-167">The `Layout` property can be used to set a different layout view, or set it to `null` so no layout file will be used.</span></span>

<span data-ttu-id="ff6e7-168">Modifiez le titre et l’élément `<h2>` de le fichier de vue *Views/HelloWorld/Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="ff6e7-168">Change the title and `<h2>` element of the *Views/HelloWorld/Index.cshtml* view file:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index2.cshtml?highlight=2,5)]

<span data-ttu-id="ff6e7-169">Le titre et l’élément `<h2>` sont légèrement différents afin que vous puissiez voir quel morceau du code modifie l’affichage.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-169">The title and `<h2>` element are slightly different so you can see which bit of code changes the display.</span></span>

<span data-ttu-id="ff6e7-170">Dans le code ci-dessus, `ViewData["Title"] = "Movie List";` définit la propriété `Title` du dictionnaire `ViewData` sur « Movie List ».</span><span class="sxs-lookup"><span data-stu-id="ff6e7-170">`ViewData["Title"] = "Movie List";` in the code above sets the `Title` property of the `ViewData` dictionary to "Movie List".</span></span> <span data-ttu-id="ff6e7-171">La propriété `Title` est utilisée dans l’élément HTML `<title>` dans la page de disposition :</span><span class="sxs-lookup"><span data-stu-id="ff6e7-171">The `Title` property is used in the `<title>` HTML element in the layout page:</span></span>

```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

<span data-ttu-id="ff6e7-172">Enregistrez la modification et accédez à `https://localhost:xxxx/HelloWorld`.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-172">Save the change and navigate to `https://localhost:xxxx/HelloWorld`.</span></span> <span data-ttu-id="ff6e7-173">Notez que le titre du navigateur, l’en-tête principal et les en-têtes secondaires ont changé.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-173">Notice that the browser title, the primary heading, and the secondary headings have changed.</span></span> <span data-ttu-id="ff6e7-174">(Si vous ne voyez pas les changements dans le navigateur, vous voyez peut-être le contenu mis en cache.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-174">(If you don't see changes in the browser, you might be viewing cached content.</span></span> <span data-ttu-id="ff6e7-175">Appuyez sur Ctrl+F5 dans votre navigateur pour forcer le chargement de la réponse du serveur.) Le titre du navigateur est créé avec la valeur `ViewData["Title"]` que nous avons définie dans le modèle de vue *Index.cshtml* et la chaîne « - Movie App » ajoutée dans le fichier de disposition.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-175">Press Ctrl+F5 in your browser to force the response from the server to be loaded.) The browser title is created with `ViewData["Title"]` we set in the *Index.cshtml* view template and the additional "- Movie App" added in the layout file.</span></span>

<span data-ttu-id="ff6e7-176">Notez également comment le contenu du modèle de vue *Index.cshtml* a été fusionné avec le modèle de vue *Views/Shared/_Layout.cshtml* et qu’une seule réponse HTML a été envoyée au navigateur.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-176">Also notice how the content in the *Index.cshtml* view template was merged with the *Views/Shared/_Layout.cshtml* view template and a single HTML response was sent to the browser.</span></span> <span data-ttu-id="ff6e7-177">Les modèles de disposition permettent d’apporter facilement des modifications qui s’appliquent à toutes les pages de votre application.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-177">Layout templates make it really easy to make changes that apply across all of the pages in your application.</span></span> <span data-ttu-id="ff6e7-178">Pour en savoir plus, consultez [Disposition](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="ff6e7-178">To learn more see [Layout](xref:mvc/views/layout).</span></span>

![Vue Movie List](~/tutorials/first-mvc-app/adding-view/_static/hell3.png)

<span data-ttu-id="ff6e7-180">Nos quelques « données » (dans le cas présent, le message « Hello from our View Template! »</span><span class="sxs-lookup"><span data-stu-id="ff6e7-180">Our little bit of "data" (in this case the "Hello from our View Template!"</span></span> <span data-ttu-id="ff6e7-181">) sont toutefois codées en dur.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-181">message) is hard-coded, though.</span></span> <span data-ttu-id="ff6e7-182">L’application MVC a une vue (« V») et vous avez un contrôleur (« C »), mais pas encore de modèle (« M »).</span><span class="sxs-lookup"><span data-stu-id="ff6e7-182">The MVC application has a "V" (view) and you've got a "C" (controller), but no "M" (model) yet.</span></span>

## <a name="passing-data-from-the-controller-to-the-view"></a><span data-ttu-id="ff6e7-183">Passage de données du contrôleur vers la vue</span><span class="sxs-lookup"><span data-stu-id="ff6e7-183">Passing Data from the Controller to the View</span></span>

<span data-ttu-id="ff6e7-184">Les actions du contrôleur sont appelées en réponse à une demande d’URL entrante.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-184">Controller actions are invoked in response to an incoming URL request.</span></span> <span data-ttu-id="ff6e7-185">Une classe de contrôleur est l’endroit où le code est écrit et qui gère les demandes du navigateur entrantes.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-185">A controller class is where the code is written that handles the incoming browser requests.</span></span> <span data-ttu-id="ff6e7-186">Le contrôleur récupère les données d’une source de données et détermine le type de réponse à envoyer au navigateur.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-186">The controller retrieves data from a data source and decides what type of response to send back to the browser.</span></span> <span data-ttu-id="ff6e7-187">Il est possible d’utiliser des modèles de vue à partir d’un contrôleur pour générer et mettre en forme une réponse HTML au navigateur.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-187">View templates can be used from a controller to generate and format an HTML response to the browser.</span></span>

<span data-ttu-id="ff6e7-188">Les contrôleurs sont chargés de fournir les données nécessaires pour qu’un modèle de vue restitue une réponse.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-188">Controllers are responsible for providing the data required in order for a view template to render a response.</span></span> <span data-ttu-id="ff6e7-189">Une meilleure pratique : N’oubliez pas que les modèles de vue ne doivent **pas** exécuter de logique métier ou interagir directement avec une base de données.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-189">A best practice: View templates should **not** perform business logic or interact with a database directly.</span></span> <span data-ttu-id="ff6e7-190">Au lieu de cela, un modèle de vue doit fonctionner uniquement avec les données que le contrôleur lui fournit.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-190">Rather, a view template should work only with the data that's provided to it by the controller.</span></span> <span data-ttu-id="ff6e7-191">Préserver cette « séparation des intérêts » permet de maintenir le code clair, testable et facile à gérer.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-191">Maintaining this "separation of concerns" helps keep the code clean, testable, and maintainable.</span></span>

<span data-ttu-id="ff6e7-192">Actuellement, le `Welcome` méthode présente dans la classe `HelloWorldController` prend un paramètre `name` et un paramètre `ID`, puis sort les valeurs directement dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-192">Currently, the `Welcome` method in the `HelloWorldController` class takes a `name` and a `ID` parameter and then outputs the values directly to the browser.</span></span> <span data-ttu-id="ff6e7-193">Au lieu que le contrôleur restitue cette réponse sous forme de chaîne, changez le contrôleur pour qu’il utilise un modèle de vue à la place.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-193">Rather than have the controller render this response as a string, change the controller to use a view template instead.</span></span> <span data-ttu-id="ff6e7-194">Comme le modèle de vue génère une réponse dynamique, les bits de données appropriés doivent être passés du contrôleur à la vue pour générer la réponse.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-194">The view template generates a dynamic response, which means that appropriate bits of data must be passed from the controller to the view in order to generate the response.</span></span> <span data-ttu-id="ff6e7-195">Pour cela, le contrôleur doit placer les données dynamiques (paramètres) dont le modèle de vue a besoin dans un dictionnaire `ViewData` auquel le modèle de vue peut ensuite accéder.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-195">Do this by having the controller put the dynamic data (parameters) that the view template needs in a `ViewData` dictionary that the view template can then access.</span></span>

<span data-ttu-id="ff6e7-196">Dans *HelloWorldController.cs*, modifiez la méthode `Welcome` pour ajouter une valeur `Message` et `NumTimes` au dictionnaire `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-196">In *HelloWorldController.cs*, change the `Welcome` method to add a `Message` and `NumTimes` value to the `ViewData` dictionary.</span></span> <span data-ttu-id="ff6e7-197">Le dictionnaire `ViewData` est un objet dynamique, ce qui signifie que n’importe quel type peut être utilisé, l’objet `ViewData` ne possède aucune propriété définie tant que vous ne placez pas d’élément dedans.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-197">The `ViewData` dictionary is a dynamic object, which means any type can be used; the `ViewData` object has no defined properties until you put something inside it.</span></span> <span data-ttu-id="ff6e7-198">Le [système de liaison de données MVC](xref:mvc/models/model-binding) mappe automatiquement les paramètres nommés (`name` et `numTimes`) provenant de la chaîne de requête dans la barre d’adresse aux paramètres de votre méthode.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-198">The [MVC model binding system](xref:mvc/models/model-binding) automatically maps the named parameters (`name` and `numTimes`) from the query string in the address bar to parameters in your method.</span></span> <span data-ttu-id="ff6e7-199">Le fichier *HelloWorldController.cs* complet ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="ff6e7-199">The complete *HelloWorldController.cs* file looks like this:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

<span data-ttu-id="ff6e7-200">L’objet dictionnaire `ViewData` contient des données qui seront passées à la vue.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-200">The `ViewData` dictionary object contains data that will be passed to the view.</span></span> 

<span data-ttu-id="ff6e7-201">Créez un modèle de vue Welcome nommé *Views/HelloWorld/Welcome.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-201">Create a Welcome view template named *Views/HelloWorld/Welcome.cshtml*.</span></span>

<span data-ttu-id="ff6e7-202">Vous allez créer une boucle dans le modèle de vue *Welcome.cshtml* qui affiche « Hello » `NumTimes`.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-202">You'll create a loop in the *Welcome.cshtml* view template that displays "Hello" `NumTimes`.</span></span> <span data-ttu-id="ff6e7-203">Remplacez le contenu de *Views/HelloWorld/Welcome.cshtml* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ff6e7-203">Replace the contents of *Views/HelloWorld/Welcome.cshtml* with the following:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

<span data-ttu-id="ff6e7-204">Enregistrez vos modifications et accédez à l’URL suivante :</span><span class="sxs-lookup"><span data-stu-id="ff6e7-204">Save your changes and browse to the following URL:</span></span>

`https://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

<span data-ttu-id="ff6e7-205">Les données sont extraites de l’URL et passées au contrôleur à l’aide du [classeur de modèles MVC](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="ff6e7-205">Data is taken from the URL and passed to the controller using the [MVC model binder](xref:mvc/models/model-binding) .</span></span> <span data-ttu-id="ff6e7-206">Le contrôleur empaquette les données dans un dictionnaire `ViewData` et passe cet objet à la vue.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-206">The controller packages the data into a `ViewData` dictionary and passes that object to the view.</span></span> <span data-ttu-id="ff6e7-207">La vue restitue ensuite les données au format HTML dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-207">The view then renders the data as HTML to the browser.</span></span>

![Vue Confidentialité affichant une étiquette Welcome et l’expression « Hello Rick » affichée quatre fois](~/tutorials/first-mvc-app/adding-view/_static/rick2.png)

<span data-ttu-id="ff6e7-209">Dans l’exemple ci-dessus, le dictionnaire `ViewData` a été utilisé pour passer des données du contrôleur à une vue.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-209">In the sample above, the `ViewData` dictionary was used to pass data from the controller to a view.</span></span> <span data-ttu-id="ff6e7-210">Plus loin dans ce didacticiel, un modèle de vue est utilisé pour passer les données d’un contrôleur à une vue.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-210">Later in the tutorial, a view model is used to pass data from a controller to a view.</span></span> <span data-ttu-id="ff6e7-211">L’approche basée sur le modèle de vue pour passer des données est généralement préférée à l’approche basée sur le dictionnaire `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-211">The view model approach to passing data is generally much preferred over the `ViewData` dictionary approach.</span></span> <span data-ttu-id="ff6e7-212">Pour plus d’informations, consultez [Quand utiliser ViewBag, ViewData ou TempData ](http://www.rachelappel.com/when-to-use-viewbag-viewdata-or-tempdata-in-asp-net-mvc-3-applications/).</span><span class="sxs-lookup"><span data-stu-id="ff6e7-212">See [When to use ViewBag, ViewData, or TempData ](http://www.rachelappel.com/when-to-use-viewbag-viewdata-or-tempdata-in-asp-net-mvc-3-applications/) for more information.</span></span>

<span data-ttu-id="ff6e7-213">Dans le didacticiel suivant, une base de données de films est créée.</span><span class="sxs-lookup"><span data-stu-id="ff6e7-213">In the next tutorial, a database of movies is created.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ff6e7-214">[Précédent](adding-controller.md)
> [Suivant](adding-model.md)</span><span class="sxs-lookup"><span data-stu-id="ff6e7-214">[Previous](adding-controller.md)
[Next](adding-model.md)</span></span>
