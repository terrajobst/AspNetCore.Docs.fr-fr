---
title: Présentation des pages Razor dans ASP.NET Core
author: Rick-Anderson
description: Découvrez comment les pages Razor dans ASP.NET Core permettent de développer des scénarios orientés page de façon plus simple et plus productive qu’avec MVC.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 10/07/2019
uid: razor-pages/index
ms.openlocfilehash: 67cc4f9b261372996d612f922c9f491f53948ece
ms.sourcegitcommit: ddc813f0f1fb293861a01597532919945b0e7fe5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74412069"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="684a8-103">Présentation des pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="684a8-103">Introduction to Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="684a8-104">De [Rick Anderson](https://twitter.com/RickAndMSFT) et [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="684a8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="684a8-105">Razor Pages can make coding page-focused scenarios easier and more productive than using controllers and views.</span><span class="sxs-lookup"><span data-stu-id="684a8-105">Razor Pages can make coding page-focused scenarios easier and more productive than using controllers and views.</span></span>

<span data-ttu-id="684a8-106">Si vous cherchez un didacticiel qui utilise l’approche Model-View-Controller, consultez [Bien démarrer avec ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="684a8-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="684a8-107">Ce document fournit une introduction aux pages Razor.</span><span class="sxs-lookup"><span data-stu-id="684a8-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="684a8-108">Il ne s’agit pas d’un didacticiel pas à pas.</span><span class="sxs-lookup"><span data-stu-id="684a8-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="684a8-109">Si certaines sections vous semblent trop techniques, consultez [Bien démarrer avec les pages Razor](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="684a8-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="684a8-110">Pour une vue d’ensemble d’ASP.NET Core, consultez [Introduction à ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="684a8-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="684a8-111">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="684a8-111">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="684a8-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="684a8-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="684a8-113">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="684a8-113">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="684a8-114">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="684a8-114">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="684a8-115">Créer un projet Razor Pages</span><span class="sxs-lookup"><span data-stu-id="684a8-115">Create a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="684a8-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="684a8-116">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="684a8-117">Pour obtenir des instructions sur la création d’un projet Razor Pages, consultez [Bien démarrer avec Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="684a8-117">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="684a8-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="684a8-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="684a8-119">Exécutez `dotnet new webapp` à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="684a8-119">Run `dotnet new webapp` from the command line.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="684a8-120">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="684a8-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="684a8-121">Exécutez `dotnet new webapp` à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="684a8-121">Run `dotnet new webapp` from the command line.</span></span>

<span data-ttu-id="684a8-122">Ouvrez le fichier *.csproj* généré à partir de Visual Studio pour Mac.</span><span class="sxs-lookup"><span data-stu-id="684a8-122">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="684a8-123">Pages Razor</span><span class="sxs-lookup"><span data-stu-id="684a8-123">Razor Pages</span></span>

<span data-ttu-id="684a8-124">La fonctionnalité Pages Razor est activée dans *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="684a8-124">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/3.0sample/RazorPagesIntro/Startup.cs?name=snippet_Startup&highlight=12,36)]

<span data-ttu-id="684a8-125">Considérez une page de base : <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="684a8-125">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index.cshtml?highlight=1)]

<span data-ttu-id="684a8-126">Le code précédent ressemble beaucoup à un [fichier vue Razor](xref:tutorials/first-mvc-app/adding-view) utilisé dans une application ASP.NET Core avec des contrôleurs et des vues.</span><span class="sxs-lookup"><span data-stu-id="684a8-126">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="684a8-127">What makes it different is the [@page](xref:mvc/views/razor#page) directive.</span><span class="sxs-lookup"><span data-stu-id="684a8-127">What makes it different is the [@page](xref:mvc/views/razor#page) directive.</span></span> <span data-ttu-id="684a8-128">`@page` fait du fichier une action MVC, ce qui signifie qu’il gère les demandes directement, sans passer par un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="684a8-128">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="684a8-129">`@page` doit être la première directive Razor sur une page.</span><span class="sxs-lookup"><span data-stu-id="684a8-129">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="684a8-130">`@page` affects the behavior of other [Razor](xref:mvc/views/razor) constructs.</span><span class="sxs-lookup"><span data-stu-id="684a8-130">`@page` affects the behavior of other [Razor](xref:mvc/views/razor) constructs.</span></span> <span data-ttu-id="684a8-131">Razor Pages file names have a *.cshtml* suffix.</span><span class="sxs-lookup"><span data-stu-id="684a8-131">Razor Pages file names have a *.cshtml* suffix.</span></span>

<span data-ttu-id="684a8-132">Une page similaire, utilisant une classe `PageModel`, est illustrée dans les deux fichiers suivants.</span><span class="sxs-lookup"><span data-stu-id="684a8-132">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="684a8-133">Le fichier *Pages/Index2.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="684a8-133">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="684a8-134">Le modèle de page *Pages/Index2.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="684a8-134">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="684a8-135">Par convention, le fichier de classe `PageModel` a le même nom que le fichier Page Razor, avec *.cs* en plus.</span><span class="sxs-lookup"><span data-stu-id="684a8-135">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="684a8-136">Par exemple, la page Razor précédente est *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="684a8-136">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="684a8-137">Le fichier contenant la classe `PageModel` se nomme *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="684a8-137">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="684a8-138">Les associations des chemins d’URL aux pages sont déterminées par l’emplacement de la page dans le système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="684a8-138">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="684a8-139">Le tableau suivant montre un chemin Page Razor et l’URL correspondante :</span><span class="sxs-lookup"><span data-stu-id="684a8-139">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="684a8-140">Nom et chemin de fichier</span><span class="sxs-lookup"><span data-stu-id="684a8-140">File name and path</span></span>               | <span data-ttu-id="684a8-141">URL correspondante</span><span class="sxs-lookup"><span data-stu-id="684a8-141">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="684a8-142">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="684a8-142">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="684a8-143">`/` ou `/Index`</span><span class="sxs-lookup"><span data-stu-id="684a8-143">`/` or `/Index`</span></span> |
| <span data-ttu-id="684a8-144">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="684a8-144">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="684a8-145">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="684a8-145">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="684a8-146">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="684a8-146">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="684a8-147">`/Store` ou `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="684a8-147">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="684a8-148">Remarques :</span><span class="sxs-lookup"><span data-stu-id="684a8-148">Notes:</span></span>

* <span data-ttu-id="684a8-149">Le runtime recherche les fichiers Pages Razor dans le dossier *Pages* par défaut.</span><span class="sxs-lookup"><span data-stu-id="684a8-149">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="684a8-150">`Index` est la page par défaut quand une URL n’inclut pas de page.</span><span class="sxs-lookup"><span data-stu-id="684a8-150">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="684a8-151">Écrire un formulaire de base</span><span class="sxs-lookup"><span data-stu-id="684a8-151">Write a basic form</span></span>

<span data-ttu-id="684a8-152">Les pages Razor sont conçues pour que les modèles courants utilisés avec les navigateurs web soient faciles à implémenter lors de la création d’une application.</span><span class="sxs-lookup"><span data-stu-id="684a8-152">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="684a8-153">La [liaison de modèle](xref:mvc/models/model-binding), les [Tag Helpers](xref:mvc/views/tag-helpers/intro) et les assistances HTML *fonctionnent tous* avec les propriétés définies dans une classe Page Razor.</span><span class="sxs-lookup"><span data-stu-id="684a8-153">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="684a8-154">Considérez une page qui implémente un formulaire « Nous contacter » de base pour le modèle `Contact` :</span><span class="sxs-lookup"><span data-stu-id="684a8-154">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="684a8-155">Pour les exemples de ce document, le `DbContext` est initialisé dans le fichier [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24).</span><span class="sxs-lookup"><span data-stu-id="684a8-155">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24) file.</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Startup.cs?name=snippet)]

<span data-ttu-id="684a8-156">Le modèle de données :</span><span class="sxs-lookup"><span data-stu-id="684a8-156">The data model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Models/Customer.cs)]

<span data-ttu-id="684a8-157">Le contexte de la base de données :</span><span class="sxs-lookup"><span data-stu-id="684a8-157">The db context:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Data/CustomerDbContext.cs)]

<span data-ttu-id="684a8-158">Le fichier vue *Pages/Create.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="684a8-158">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="684a8-159">Le modèle de page *Pages/Create.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="684a8-159">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="684a8-160">Par convention, la classe `PageModel` se nomme `<PageName>Model` et se trouve dans le même espace de noms que la page.</span><span class="sxs-lookup"><span data-stu-id="684a8-160">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="684a8-161">La classe `PageModel` permet de séparer la logique d’une page de sa présentation.</span><span class="sxs-lookup"><span data-stu-id="684a8-161">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="684a8-162">Elle définit des gestionnaires de page pour les demandes envoyées à la page et les données utilisées pour l’afficher.</span><span class="sxs-lookup"><span data-stu-id="684a8-162">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="684a8-163">This separation allows:</span><span class="sxs-lookup"><span data-stu-id="684a8-163">This separation allows:</span></span>

* <span data-ttu-id="684a8-164">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="684a8-164">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* [<span data-ttu-id="684a8-165">Test unitaire</span><span class="sxs-lookup"><span data-stu-id="684a8-165">Unit testing</span></span>](xref:test/razor-pages-tests)

<span data-ttu-id="684a8-166">La page a une *méthode de gestionnaire* `OnPostAsync`, qui s’exécute sur les requêtes `POST` (quand un utilisateur poste le formulaire).</span><span class="sxs-lookup"><span data-stu-id="684a8-166">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="684a8-167">Handler methods for any HTTP verb can be added.</span><span class="sxs-lookup"><span data-stu-id="684a8-167">Handler methods for any HTTP verb can be added.</span></span> <span data-ttu-id="684a8-168">Les gestionnaires les plus courants sont :</span><span class="sxs-lookup"><span data-stu-id="684a8-168">The most common handlers are:</span></span>

* <span data-ttu-id="684a8-169">`OnGet` pour initialiser l’état nécessaire pour la page.</span><span class="sxs-lookup"><span data-stu-id="684a8-169">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="684a8-170">In the preceding code, the `OnGet` method displays the *CreateModel.cshtml* Razor Page.</span><span class="sxs-lookup"><span data-stu-id="684a8-170">In the preceding code, the `OnGet` method displays the *CreateModel.cshtml* Razor Page.</span></span>
* <span data-ttu-id="684a8-171">`OnPost` pour gérer les envois de formulaire.</span><span class="sxs-lookup"><span data-stu-id="684a8-171">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="684a8-172">Le suffixe de nommage `Async` est facultatif, mais souvent utilisé par convention pour les fonctions asynchrones.</span><span class="sxs-lookup"><span data-stu-id="684a8-172">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="684a8-173">Le code précédent est typique des pages Razor.</span><span class="sxs-lookup"><span data-stu-id="684a8-173">The preceding code is typical for Razor Pages.</span></span>

<span data-ttu-id="684a8-174">If you're familiar with ASP.NET apps using controllers and views:</span><span class="sxs-lookup"><span data-stu-id="684a8-174">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="684a8-175">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span><span class="sxs-lookup"><span data-stu-id="684a8-175">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="684a8-176">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results work the same with Controllers and Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="684a8-176">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results work the same with Controllers and Razor Pages.</span></span> 

<span data-ttu-id="684a8-177">La méthode `OnPostAsync` précédente :</span><span class="sxs-lookup"><span data-stu-id="684a8-177">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="684a8-178">Le flux de base de `OnPostAsync` :</span><span class="sxs-lookup"><span data-stu-id="684a8-178">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="684a8-179">Recherchez les erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="684a8-179">Check for validation errors.</span></span>

* <span data-ttu-id="684a8-180">S’il n’y a aucune erreur, enregistrez les données et redirigez.</span><span class="sxs-lookup"><span data-stu-id="684a8-180">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="684a8-181">S’il y a des erreurs, réaffichez la page avec des messages de validation.</span><span class="sxs-lookup"><span data-stu-id="684a8-181">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="684a8-182">Dans de nombreux cas, les erreurs de validation sont détectées sur le client et jamais envoyées au serveur.</span><span class="sxs-lookup"><span data-stu-id="684a8-182">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="684a8-183">Le fichier vue *Pages/Create.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="684a8-183">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="684a8-184">The rendered HTML from *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="684a8-184">The rendered HTML from *Pages/Create.cshtml*:</span></span>

[!code-html[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.html)]

<span data-ttu-id="684a8-185">In the previous code, posting the form:</span><span class="sxs-lookup"><span data-stu-id="684a8-185">In the previous code, posting the form:</span></span>

* <span data-ttu-id="684a8-186">With valid data:</span><span class="sxs-lookup"><span data-stu-id="684a8-186">With valid data:</span></span>

  * <span data-ttu-id="684a8-187">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> helper method.</span><span class="sxs-lookup"><span data-stu-id="684a8-187">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> helper method.</span></span> <span data-ttu-id="684a8-188">`RedirectToPage` retourne une instance de <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult>.</span><span class="sxs-lookup"><span data-stu-id="684a8-188">`RedirectToPage` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult>.</span></span> <span data-ttu-id="684a8-189">`RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="684a8-189">`RedirectToPage`:</span></span>

    * <span data-ttu-id="684a8-190">Is an action result.</span><span class="sxs-lookup"><span data-stu-id="684a8-190">Is an action result.</span></span>
    * <span data-ttu-id="684a8-191">Is similar to `RedirectToAction` or `RedirectToRoute` (used in controllers and views).</span><span class="sxs-lookup"><span data-stu-id="684a8-191">Is similar to `RedirectToAction` or `RedirectToRoute` (used in controllers and views).</span></span>
    * <span data-ttu-id="684a8-192">Is customized for pages.</span><span class="sxs-lookup"><span data-stu-id="684a8-192">Is customized for pages.</span></span> <span data-ttu-id="684a8-193">Dans l’exemple précédent, il redirige vers la page Index racine (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="684a8-193">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="684a8-194">`RedirectToPage` est détaillé dans la section [Génération d’URL pour les pages](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="684a8-194">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

* <span data-ttu-id="684a8-195">With validation errors that are passed to the server:</span><span class="sxs-lookup"><span data-stu-id="684a8-195">With validation errors that are passed to the server:</span></span>

  * <span data-ttu-id="684a8-196">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*> helper method.</span><span class="sxs-lookup"><span data-stu-id="684a8-196">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*> helper method.</span></span> <span data-ttu-id="684a8-197">`Page` retourne une instance de <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>.</span><span class="sxs-lookup"><span data-stu-id="684a8-197">`Page` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>.</span></span> <span data-ttu-id="684a8-198">Le retour de `Page` est similaire à la façon dont les actions dans les contrôleurs retournent `View`.</span><span class="sxs-lookup"><span data-stu-id="684a8-198">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="684a8-199">`PageResult` is the default return type for a handler method.</span><span class="sxs-lookup"><span data-stu-id="684a8-199">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="684a8-200">Une méthode de gestionnaire qui retourne `void` restitue la page.</span><span class="sxs-lookup"><span data-stu-id="684a8-200">A handler method that returns `void` renders the page.</span></span>
  * <span data-ttu-id="684a8-201">In the preceding example, posting the form with no value results in [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) returning false.</span><span class="sxs-lookup"><span data-stu-id="684a8-201">In the preceding example, posting the form with no value results in [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) returning false.</span></span> <span data-ttu-id="684a8-202">In this sample, no validation errors are displayed on the client.</span><span class="sxs-lookup"><span data-stu-id="684a8-202">In this sample, no validation errors are displayed on the client.</span></span> <span data-ttu-id="684a8-203">Validation error handing is covered later in this document.</span><span class="sxs-lookup"><span data-stu-id="684a8-203">Validation error handing is covered later in this document.</span></span>

  [!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=3-6)]

* <span data-ttu-id="684a8-204">With validation errors detected by client side validation:</span><span class="sxs-lookup"><span data-stu-id="684a8-204">With validation errors detected by client side validation:</span></span>

  * <span data-ttu-id="684a8-205">Data is **not** posted to the server.</span><span class="sxs-lookup"><span data-stu-id="684a8-205">Data is **not** posted to the server.</span></span>
  * <span data-ttu-id="684a8-206">Client-side validation is explained later in this document.</span><span class="sxs-lookup"><span data-stu-id="684a8-206">Client-side validation is explained later in this document.</span></span>

<span data-ttu-id="684a8-207">The `Customer` property uses [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute to opt in to model binding:</span><span class="sxs-lookup"><span data-stu-id="684a8-207">The `Customer` property uses [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute to opt in to model binding:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=15-16)]

<span data-ttu-id="684a8-208">`[BindProperty]` should **not** be used on models containing properties that should not be changed by the client.</span><span class="sxs-lookup"><span data-stu-id="684a8-208">`[BindProperty]` should **not** be used on models containing properties that should not be changed by the client.</span></span> <span data-ttu-id="684a8-209">For more information, see [Overposting](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="684a8-209">For more information, see [Overposting](xref:data/ef-rp/crud#overposting).</span></span>

<span data-ttu-id="684a8-210">Par défaut, Razor Pages lie les propriétés seulement avec des verbes non-`GET`.</span><span class="sxs-lookup"><span data-stu-id="684a8-210">Razor Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="684a8-211">Binding to properties removes the need to writing code to convert HTTP data to the model type.</span><span class="sxs-lookup"><span data-stu-id="684a8-211">Binding to properties removes the need to writing code to convert HTTP data to the model type.</span></span> <span data-ttu-id="684a8-212">Elle réduit la quantité de code en utilisant la même propriété pour afficher les champs de formulaire (`<input asp-for="Customer.Name">`) et accepter l’entrée.</span><span class="sxs-lookup"><span data-stu-id="684a8-212">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="684a8-213">Reviewing the *Pages/Create.cshtml* view file:</span><span class="sxs-lookup"><span data-stu-id="684a8-213">Reviewing the *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml?highlight=3,9)]

* <span data-ttu-id="684a8-214">In the preceding code, the [input tag helper](xref:mvc/views/working-with-forms#the-input-tag-helper) `<input asp-for="Customer.Name" />` binds the HTML `<input>` element to the `Customer.Name` model expression.</span><span class="sxs-lookup"><span data-stu-id="684a8-214">In the preceding code, the [input tag helper](xref:mvc/views/working-with-forms#the-input-tag-helper) `<input asp-for="Customer.Name" />` binds the HTML `<input>` element to the `Customer.Name` model expression.</span></span>
* <span data-ttu-id="684a8-215">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available) makes Tag Helpers available.</span><span class="sxs-lookup"><span data-stu-id="684a8-215">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available) makes Tag Helpers available.</span></span>

### <a name="the-home-page"></a><span data-ttu-id="684a8-216">The home page</span><span class="sxs-lookup"><span data-stu-id="684a8-216">The home page</span></span>

<span data-ttu-id="684a8-217">*Index.cshtml* is the home page:</span><span class="sxs-lookup"><span data-stu-id="684a8-217">*Index.cshtml* is the home page:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml)]

<span data-ttu-id="684a8-218">La classe `PageModel` associée (*Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="684a8-218">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="684a8-219">The *Index.cshtml* file contains the following markup:</span><span class="sxs-lookup"><span data-stu-id="684a8-219">The *Index.cshtml* file contains the following markup:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=21)]

<span data-ttu-id="684a8-220">The `<a /a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span><span class="sxs-lookup"><span data-stu-id="684a8-220">The `<a /a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="684a8-221">Le lien contient des données d’itinéraire avec l’ID de contact.</span><span class="sxs-lookup"><span data-stu-id="684a8-221">The link contains route data with the contact ID.</span></span> <span data-ttu-id="684a8-222">Par exemple, `https://localhost:5001/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="684a8-222">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="684a8-223">Les [Tag Helpers](xref:mvc/views/tag-helpers/intro) permettent au code côté serveur de participer à la création et au rendu des éléments HTML dans les fichiers Razor.</span><span class="sxs-lookup"><span data-stu-id="684a8-223">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>

<span data-ttu-id="684a8-224">The *Index.cshtml* file contains markup to create a delete button for each customer contact:</span><span class="sxs-lookup"><span data-stu-id="684a8-224">The *Index.cshtml* file contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=22-23)]

<span data-ttu-id="684a8-225">The rendered HTML:</span><span class="sxs-lookup"><span data-stu-id="684a8-225">The rendered HTML:</span></span>

```HTML
<button type="submit" formaction="/Customers?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="684a8-226">When the delete button is rendered in HTML, its [formaction](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) includes parameters for:</span><span class="sxs-lookup"><span data-stu-id="684a8-226">When the delete button is rendered in HTML, its [formaction](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) includes parameters for:</span></span>

* <span data-ttu-id="684a8-227">The customer contact ID, specified by the `asp-route-id` attribute.</span><span class="sxs-lookup"><span data-stu-id="684a8-227">The customer contact ID, specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="684a8-228">The `handler`, specified by the `asp-page-handler` attribute.</span><span class="sxs-lookup"><span data-stu-id="684a8-228">The `handler`, specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="684a8-229">Quand le bouton est sélectionné, une demande `POST` de formulaire est envoyée au serveur.</span><span class="sxs-lookup"><span data-stu-id="684a8-229">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="684a8-230">Par convention, le nom de la méthode de gestionnaire est sélectionné en fonction de la valeur du paramètre `handler` conformément au schéma `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="684a8-230">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="684a8-231">Étant donné que le `handler` est `delete` dans cet exemple, la méthode de gestionnaire `OnPostDeleteAsync` est utilisée pour traiter la demande `POST`.</span><span class="sxs-lookup"><span data-stu-id="684a8-231">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="684a8-232">Si `asp-page-handler` est défini sur une autre valeur, comme `remove`, une méthode de gestionnaire avec le nom `OnPostRemoveAsync` est sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="684a8-232">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="684a8-233">La méthode `OnPostDeleteAsync` :</span><span class="sxs-lookup"><span data-stu-id="684a8-233">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="684a8-234">Gets the `id` from the query string.</span><span class="sxs-lookup"><span data-stu-id="684a8-234">Gets the `id` from the query string.</span></span>
* <span data-ttu-id="684a8-235">Interroge la base de données pour le contact client avec `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="684a8-235">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="684a8-236">If the customer contact is found, it's removed and the database is updated.</span><span class="sxs-lookup"><span data-stu-id="684a8-236">If the customer contact is found, it's removed and the database is updated.</span></span>
* <span data-ttu-id="684a8-237">Appelle <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> pour rediriger vers la page Index racine (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="684a8-237">Calls <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> to redirect to the root Index page (`/Index`).</span></span>

### <a name="the-editcshtml-file"></a><span data-ttu-id="684a8-238">The Edit.cshtml file</span><span class="sxs-lookup"><span data-stu-id="684a8-238">The Edit.cshtml file</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml?highlight=1)]

<span data-ttu-id="684a8-239">La première ligne contient la directive `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="684a8-239">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="684a8-240">La contrainte de routage `"{id:int}"` indique à la page qu’elle doit accepter les requêtes qui contiennent des données d’itinéraire `int`.</span><span class="sxs-lookup"><span data-stu-id="684a8-240">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="684a8-241">Si une requête à la page ne contient de données d’itinéraire qui peuvent être converties en `int`, le runtime retourne une erreur HTTP 404 (introuvable).</span><span class="sxs-lookup"><span data-stu-id="684a8-241">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="684a8-242">Pour que l’ID soit facultatif, ajoutez `?` à la contrainte de route :</span><span class="sxs-lookup"><span data-stu-id="684a8-242">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="684a8-243">The *Edit.cshtml.cs* file:</span><span class="sxs-lookup"><span data-stu-id="684a8-243">The *Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml.cs?name=snippet)]

## <a name="validation"></a><span data-ttu-id="684a8-244">Validation</span><span class="sxs-lookup"><span data-stu-id="684a8-244">Validation</span></span>

<span data-ttu-id="684a8-245">Validation rules:</span><span class="sxs-lookup"><span data-stu-id="684a8-245">Validation rules:</span></span>

* <span data-ttu-id="684a8-246">Are declaratively specified in the model class.</span><span class="sxs-lookup"><span data-stu-id="684a8-246">Are declaratively specified in the model class.</span></span>
* <span data-ttu-id="684a8-247">Are enforced everywhere in the app.</span><span class="sxs-lookup"><span data-stu-id="684a8-247">Are enforced everywhere in the app.</span></span>

<span data-ttu-id="684a8-248">The <xref:System.ComponentModel.DataAnnotations> namespace provides a set of built-in validation attributes that are applied declaratively to a class or property.</span><span class="sxs-lookup"><span data-stu-id="684a8-248">The <xref:System.ComponentModel.DataAnnotations> namespace provides a set of built-in validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="684a8-249">DataAnnotations also contains formatting attributes like [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) that help with formatting and don't provide any validation.</span><span class="sxs-lookup"><span data-stu-id="684a8-249">DataAnnotations also contains formatting attributes like [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) that help with formatting and don't provide any validation.</span></span>

<span data-ttu-id="684a8-250">Consider the `Customer` model:</span><span class="sxs-lookup"><span data-stu-id="684a8-250">Consider the `Customer` model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Models/Customer.cs)]

<span data-ttu-id="684a8-251">Using the following *Create.cshtml* view file:</span><span class="sxs-lookup"><span data-stu-id="684a8-251">Using the following *Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=3,8-9,15-99)]

<span data-ttu-id="684a8-252">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="684a8-252">The preceding code:</span></span>

* <span data-ttu-id="684a8-253">Includes jQuery and jQuery validation scripts.</span><span class="sxs-lookup"><span data-stu-id="684a8-253">Includes jQuery and jQuery validation scripts.</span></span>
* <span data-ttu-id="684a8-254">Uses the `<div />` and `<span />` [Tag Helpers](xref:mvc/views/tag-helpers/intro) to enable:</span><span class="sxs-lookup"><span data-stu-id="684a8-254">Uses the `<div />` and `<span />` [Tag Helpers](xref:mvc/views/tag-helpers/intro) to enable:</span></span>

  * <span data-ttu-id="684a8-255">Client-side validation.</span><span class="sxs-lookup"><span data-stu-id="684a8-255">Client-side validation.</span></span>
  * <span data-ttu-id="684a8-256">Validation error rendering.</span><span class="sxs-lookup"><span data-stu-id="684a8-256">Validation error rendering.</span></span>

* <span data-ttu-id="684a8-257">Génère le code HTML suivant :</span><span class="sxs-lookup"><span data-stu-id="684a8-257">Generates the following HTML:</span></span>

  [!code-html[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create5.html)]

<span data-ttu-id="684a8-258">Posting the Create form without a name value displays the error message "The Name field is required."</span><span class="sxs-lookup"><span data-stu-id="684a8-258">Posting the Create form without a name value displays the error message "The Name field is required."</span></span> <span data-ttu-id="684a8-259">on the form.</span><span class="sxs-lookup"><span data-stu-id="684a8-259">on the form.</span></span> <span data-ttu-id="684a8-260">If JavaScript is enabled on the client, the browser displays the error without posting to the server.</span><span class="sxs-lookup"><span data-stu-id="684a8-260">If JavaScript is enabled on the client, the browser displays the error without posting to the server.</span></span>

<span data-ttu-id="684a8-261">The `[StringLength(10)]` attribute generates `data-val-length-max="10"` on the rendered HTML.</span><span class="sxs-lookup"><span data-stu-id="684a8-261">The `[StringLength(10)]` attribute generates `data-val-length-max="10"` on the rendered HTML.</span></span> <span data-ttu-id="684a8-262">`data-val-length-max` prevents browsers from entering more than the maximum length specified.</span><span class="sxs-lookup"><span data-stu-id="684a8-262">`data-val-length-max` prevents browsers from entering more than the maximum length specified.</span></span> <span data-ttu-id="684a8-263">If a tool such as [Fiddler](https://www.telerik.com/fiddler) is used to edit and replay the post:</span><span class="sxs-lookup"><span data-stu-id="684a8-263">If a tool such as [Fiddler](https://www.telerik.com/fiddler) is used to edit and replay the post:</span></span>

* <span data-ttu-id="684a8-264">With the name longer than 10.</span><span class="sxs-lookup"><span data-stu-id="684a8-264">With the name longer than 10.</span></span>
* <span data-ttu-id="684a8-265">The error message "The field Name must be a string with a maximum length of 10."</span><span class="sxs-lookup"><span data-stu-id="684a8-265">The error message "The field Name must be a string with a maximum length of 10."</span></span> <span data-ttu-id="684a8-266">is returned.</span><span class="sxs-lookup"><span data-stu-id="684a8-266">is returned.</span></span>

<span data-ttu-id="684a8-267">Consider the following `Movie` model:</span><span class="sxs-lookup"><span data-stu-id="684a8-267">Consider the following `Movie` model:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet1)]

<span data-ttu-id="684a8-268">The validation attributes specify behavior to enforce on the model properties they're applied to:</span><span class="sxs-lookup"><span data-stu-id="684a8-268">The validation attributes specify behavior to enforce on the model properties they're applied to:</span></span>

* <span data-ttu-id="684a8-269">The `Required` and `MinimumLength` attributes indicate that a property must have a value, but nothing prevents a user from entering white space to satisfy this validation.</span><span class="sxs-lookup"><span data-stu-id="684a8-269">The `Required` and `MinimumLength` attributes indicate that a property must have a value, but nothing prevents a user from entering white space to satisfy this validation.</span></span>
* <span data-ttu-id="684a8-270">L’attribut `RegularExpression` sert à limiter les caractères pouvant être entrés.</span><span class="sxs-lookup"><span data-stu-id="684a8-270">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="684a8-271">Dans le code précédent, « Genre » :</span><span class="sxs-lookup"><span data-stu-id="684a8-271">In the preceding code, "Genre":</span></span>

  * <span data-ttu-id="684a8-272">Doit utiliser seulement des lettres.</span><span class="sxs-lookup"><span data-stu-id="684a8-272">Must only use letters.</span></span>
  * <span data-ttu-id="684a8-273">La première lettre doit être une majuscule.</span><span class="sxs-lookup"><span data-stu-id="684a8-273">The first letter is required to be uppercase.</span></span> <span data-ttu-id="684a8-274">Les espaces, les chiffres et les caractères spéciaux ne sont pas autorisés.</span><span class="sxs-lookup"><span data-stu-id="684a8-274">White space, numbers, and special characters are not allowed.</span></span>

* <span data-ttu-id="684a8-275">L’expression `RegularExpression` « Rating » :</span><span class="sxs-lookup"><span data-stu-id="684a8-275">The `RegularExpression` "Rating":</span></span>

  * <span data-ttu-id="684a8-276">Nécessite que le premier caractère soit une lettre majuscule.</span><span class="sxs-lookup"><span data-stu-id="684a8-276">Requires that the first character be an uppercase letter.</span></span>
  * <span data-ttu-id="684a8-277">Allows special characters and numbers in subsequent spaces.</span><span class="sxs-lookup"><span data-stu-id="684a8-277">Allows special characters and numbers in subsequent spaces.</span></span> <span data-ttu-id="684a8-278">« PG-13 » est valide pour une évaluation, mais échoue pour un « Genre ».</span><span class="sxs-lookup"><span data-stu-id="684a8-278">"PG-13" is valid for a rating, but fails for a "Genre".</span></span>

* <span data-ttu-id="684a8-279">L’attribut `Range` contraint une valeur à une plage spécifiée.</span><span class="sxs-lookup"><span data-stu-id="684a8-279">The `Range` attribute constrains a value to within a specified range.</span></span>
* <span data-ttu-id="684a8-280">The `StringLength` attribute sets the maximum length of a string property, and optionally its minimum length.</span><span class="sxs-lookup"><span data-stu-id="684a8-280">The `StringLength` attribute sets the maximum length of a string property, and optionally its minimum length.</span></span>
* <span data-ttu-id="684a8-281">Les types valeur (tels que `decimal`, `int`, `float` et `DateTime`) sont obligatoires par nature et n’ont pas besoin de l’attribut `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="684a8-281">Value types (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="684a8-282">The Create page for the `Movie` model shows displays errors with invalid values:</span><span class="sxs-lookup"><span data-stu-id="684a8-282">The Create page for the `Movie` model shows displays errors with invalid values:</span></span>

![Formulaire de vue Movie avec plusieurs erreurs de validation jQuery côté client](~/tutorials/razor-pages/validation/_static/val.png)

<span data-ttu-id="684a8-284">Pour plus d'informations, voir :</span><span class="sxs-lookup"><span data-stu-id="684a8-284">For more information, see:</span></span>

* [<span data-ttu-id="684a8-285">Add validation to the Movie app</span><span class="sxs-lookup"><span data-stu-id="684a8-285">Add validation to the Movie app</span></span>](xref:tutorials/razor-pages/validation)
* <span data-ttu-id="684a8-286">[Model validation in ASP.NET Core](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="684a8-286">[Model validation in ASP.NET Core](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="684a8-287">Gérer les requêtes HEAD avec un gestionnaire OnGet de secours</span><span class="sxs-lookup"><span data-stu-id="684a8-287">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="684a8-288">`HEAD` requests allow retrieving the headers for a specific resource.</span><span class="sxs-lookup"><span data-stu-id="684a8-288">`HEAD` requests allow retrieving the headers for a specific resource.</span></span> <span data-ttu-id="684a8-289">Contrairement aux requêtes `GET`, les requêtes `HEAD` ne retournent pas un corps de réponse.</span><span class="sxs-lookup"><span data-stu-id="684a8-289">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="684a8-290">En règle générale, un gestionnaire `OnHead` est créé et appelé pour les requêtes `HEAD` :</span><span class="sxs-lookup"><span data-stu-id="684a8-290">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Privacy.cshtml.cs?name=snippet)]

<span data-ttu-id="684a8-291">Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span><span class="sxs-lookup"><span data-stu-id="684a8-291">Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="684a8-292">XSRF/CSRF et pages Razor</span><span class="sxs-lookup"><span data-stu-id="684a8-292">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="684a8-293">Razor Pages are protected by [Antiforgery validation](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="684a8-293">Razor Pages are protected by [Antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="684a8-294">The [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span><span class="sxs-lookup"><span data-stu-id="684a8-294">The [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="684a8-295">Utilisation de dispositions, partiels, modèles et Tag Helpers avec les pages Razor</span><span class="sxs-lookup"><span data-stu-id="684a8-295">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="684a8-296">Les pages Razor fonctionnent avec toutes les fonctionnalités du moteur de vue Razor.</span><span class="sxs-lookup"><span data-stu-id="684a8-296">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="684a8-297">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, and *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span><span class="sxs-lookup"><span data-stu-id="684a8-297">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, and *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="684a8-298">Nous allons nettoyer un peu cette page en tirant parti de certaines de ces fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="684a8-298">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="684a8-299">Ajoutez une [page de disposition](xref:mvc/views/layout) à *Pages/Shared/_Layout.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="684a8-299">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Shared/_Layout2.cshtml?hightlight=12)]

<span data-ttu-id="684a8-300">La [disposition](xref:mvc/views/layout) :</span><span class="sxs-lookup"><span data-stu-id="684a8-300">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="684a8-301">Contrôle la disposition de chaque page (à moins que la page ne refuse la disposition).</span><span class="sxs-lookup"><span data-stu-id="684a8-301">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="684a8-302">Importe des structures HTML telles que JavaScript et les feuilles de style.</span><span class="sxs-lookup"><span data-stu-id="684a8-302">Imports HTML structures such as JavaScript and stylesheets.</span></span>
* <span data-ttu-id="684a8-303">The contents of the Razor page are rendered where `@RenderBody()` is called.</span><span class="sxs-lookup"><span data-stu-id="684a8-303">The contents of the Razor page are rendered where `@RenderBody()` is called.</span></span>

<span data-ttu-id="684a8-304">For more information, see [layout page](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="684a8-304">For more information, see [layout page](xref:mvc/views/layout).</span></span>

<span data-ttu-id="684a8-305">La propriété [Layout](xref:mvc/views/layout#specifying-a-layout) est définie dans *Pages/_ViewStart.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="684a8-305">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="684a8-306">La disposition est dans le dossier *Pages/Shared*.</span><span class="sxs-lookup"><span data-stu-id="684a8-306">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="684a8-307">Les pages recherchent d’autres vues (dispositions, modèles, partiels) hiérarchiquement, en commençant dans le même dossier que la page active.</span><span class="sxs-lookup"><span data-stu-id="684a8-307">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="684a8-308">Une disposition dans le dossier *Pages/Shared* peut être utilisée à partir de n’importe quelle page Razor sous le dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="684a8-308">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="684a8-309">Le fichier de disposition doit être placé dans le dossier *Pages/Shared*.</span><span class="sxs-lookup"><span data-stu-id="684a8-309">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="684a8-310">Nous vous recommandons de **ne pas** placer le fichier de disposition dans le dossier *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="684a8-310">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="684a8-311">*Views/Shared* est un modèle de vues MVC.</span><span class="sxs-lookup"><span data-stu-id="684a8-311">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="684a8-312">Les pages Razor sont censées s’appuyer sur la hiérarchie des dossiers, pas sur les conventions de chemins.</span><span class="sxs-lookup"><span data-stu-id="684a8-312">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="684a8-313">La recherche de vue à partir d’une page Razor inclut le dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="684a8-313">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="684a8-314">The layouts, templates, and partials used with MVC controllers and conventional Razor views *just work*.</span><span class="sxs-lookup"><span data-stu-id="684a8-314">The layouts, templates, and partials used with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="684a8-315">Ajoutez un fichier *Pages/_ViewImports.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="684a8-315">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="684a8-316">`@namespace` est expliqué plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="684a8-316">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="684a8-317">La directive `@addTagHelper` permet de bénéficier des [Tag Helpers intégrés](xref:mvc/views/tag-helpers/builtin-th/Index) dans toutes les pages du dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="684a8-317">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="684a8-318">The `@namespace` directive set on a page:</span><span class="sxs-lookup"><span data-stu-id="684a8-318">The `@namespace` directive set on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="684a8-319">The `@namespace` directive sets the namespace for the page.</span><span class="sxs-lookup"><span data-stu-id="684a8-319">The `@namespace` directive sets the namespace for the page.</span></span> <span data-ttu-id="684a8-320">La directive `@model` n’a pas besoin d’inclure l’espace de noms.</span><span class="sxs-lookup"><span data-stu-id="684a8-320">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="684a8-321">Quand la directive `@namespace` est contenue dans *_ViewImports.cshtml*, l’espace de noms spécifié fournit le préfixe de l’espace de noms généré dans la Page qui importe la directive `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="684a8-321">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="684a8-322">Le reste de l’espace de noms généré (la partie suffixe) est le chemin relatif séparé par un point entre le dossier contenant *_ViewImports.cshtml* et le dossier contenant la page.</span><span class="sxs-lookup"><span data-stu-id="684a8-322">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="684a8-323">Par exemple, la classe `PageModel` *Pages/Customers/Edit.cshtml.cs* définit explicitement l’espace de noms :</span><span class="sxs-lookup"><span data-stu-id="684a8-323">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="684a8-324">Le fichier *Pages/_ViewImports.cshtml* définit l’espace de noms suivant :</span><span class="sxs-lookup"><span data-stu-id="684a8-324">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="684a8-325">L’espace de noms généré pour la page Razor *Pages/Customers/Edit.cshtml* est identique à la classe `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="684a8-325">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="684a8-326">`@namespace` *fonctionne également avec les vues Razor classiques*.</span><span class="sxs-lookup"><span data-stu-id="684a8-326">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="684a8-327">Consider the *Pages/Create.cshtml* view file:</span><span class="sxs-lookup"><span data-stu-id="684a8-327">Consider the *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=2-3)]

<span data-ttu-id="684a8-328">The updated *Pages/Create.cshtml* view file with *_ViewImports.cshtml* and the preceding layout file:</span><span class="sxs-lookup"><span data-stu-id="684a8-328">The updated *Pages/Create.cshtml* view file with *_ViewImports.cshtml* and the preceding layout file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.cshtml?highlight=2)]

<span data-ttu-id="684a8-329">In the preceding code, the *_ViewImports.cshtml* imported the namespace and Tag Helpers.</span><span class="sxs-lookup"><span data-stu-id="684a8-329">In the preceding code, the *_ViewImports.cshtml* imported the namespace and Tag Helpers.</span></span> <span data-ttu-id="684a8-330">The layout file imported the JavaScript files.</span><span class="sxs-lookup"><span data-stu-id="684a8-330">The layout file imported the JavaScript files.</span></span>

<span data-ttu-id="684a8-331">Le [projet de démarrage de pages Razor](#rpvs17) contient *Pages/_ValidationScriptsPartial.cshtml*, qui connecte la validation côté client.</span><span class="sxs-lookup"><span data-stu-id="684a8-331">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="684a8-332">Pour plus d'informations sur les affichages partiels, consultez <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="684a8-332">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="684a8-333">Génération d’URL pour les pages</span><span class="sxs-lookup"><span data-stu-id="684a8-333">URL generation for Pages</span></span>

<span data-ttu-id="684a8-334">La page `Create`, illustrée précédemment, utilise `RedirectToPage` :</span><span class="sxs-lookup"><span data-stu-id="684a8-334">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=28)]

<span data-ttu-id="684a8-335">L’application a la structure de fichiers/dossiers suivante :</span><span class="sxs-lookup"><span data-stu-id="684a8-335">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="684a8-336">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="684a8-336">*/Pages*</span></span>

  * <span data-ttu-id="684a8-337">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="684a8-337">*Index.cshtml*</span></span>
  * <span data-ttu-id="684a8-338">*Privacy.cshtml*</span><span class="sxs-lookup"><span data-stu-id="684a8-338">*Privacy.cshtml*</span></span>
  * <span data-ttu-id="684a8-339">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="684a8-339">*/Customers*</span></span>

    * <span data-ttu-id="684a8-340">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="684a8-340">*Create.cshtml*</span></span>
    * <span data-ttu-id="684a8-341">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="684a8-341">*Edit.cshtml*</span></span>
    * <span data-ttu-id="684a8-342">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="684a8-342">*Index.cshtml*</span></span>

<span data-ttu-id="684a8-343">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Customers/Index.cshtml* after success.</span><span class="sxs-lookup"><span data-stu-id="684a8-343">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Customers/Index.cshtml* after success.</span></span> <span data-ttu-id="684a8-344">The string `./Index` is a relative page name used to access the preceding page.</span><span class="sxs-lookup"><span data-stu-id="684a8-344">The string `./Index` is a relative page name used to access the preceding page.</span></span> <span data-ttu-id="684a8-345">It is used to generate URLs to the *Pages/Customers/Index.cshtml* page.</span><span class="sxs-lookup"><span data-stu-id="684a8-345">It is used to generate URLs to the *Pages/Customers/Index.cshtml* page.</span></span> <span data-ttu-id="684a8-346">Exemple :</span><span class="sxs-lookup"><span data-stu-id="684a8-346">For example:</span></span>

* `Url.Page("./Index", ...)`
* `<a asp-page="./Index">Customers Index Page</a>`
* `RedirectToPage("./Index")`

<span data-ttu-id="684a8-347">The absolute page name `/Index` is used to generate URLs to the *Pages/Index.cshtml* page.</span><span class="sxs-lookup"><span data-stu-id="684a8-347">The absolute page name `/Index` is used to generate URLs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="684a8-348">Exemple :</span><span class="sxs-lookup"><span data-stu-id="684a8-348">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">Home Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="684a8-349">Le nom de la page est le chemin de la page à partir du dossier racine */Pages* avec un `/` devant (par exemple, `/Index`).</span><span class="sxs-lookup"><span data-stu-id="684a8-349">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="684a8-350">The preceding URL generation samples offer enhanced options and functional capabilities over hard-coding a URL.</span><span class="sxs-lookup"><span data-stu-id="684a8-350">The preceding URL generation samples offer enhanced options and functional capabilities over hard-coding a URL.</span></span> <span data-ttu-id="684a8-351">La génération d’URL utilise le [routage](xref:mvc/controllers/routing) et peut générer et encoder des paramètres en fonction de la façon dont l’itinéraire est défini dans le chemin de destination.</span><span class="sxs-lookup"><span data-stu-id="684a8-351">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="684a8-352">La génération d’URL pour les pages prend en charge les noms relatifs.</span><span class="sxs-lookup"><span data-stu-id="684a8-352">URL generation for pages supports relative names.</span></span> <span data-ttu-id="684a8-353">The following table shows which Index page is selected using different `RedirectToPage` parameters in *Pages/Customers/Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="684a8-353">The following table shows which Index page is selected using different `RedirectToPage` parameters in *Pages/Customers/Create.cshtml*.</span></span>

| <span data-ttu-id="684a8-354">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="684a8-354">RedirectToPage(x)</span></span>| <span data-ttu-id="684a8-355">Page</span><span class="sxs-lookup"><span data-stu-id="684a8-355">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="684a8-356">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="684a8-356">RedirectToPage("/Index")</span></span> | <span data-ttu-id="684a8-357">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="684a8-357">*Pages/Index*</span></span> |
| <span data-ttu-id="684a8-358">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="684a8-358">RedirectToPage("./Index");</span></span> | <span data-ttu-id="684a8-359">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="684a8-359">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="684a8-360">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="684a8-360">RedirectToPage("../Index")</span></span> | <span data-ttu-id="684a8-361">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="684a8-361">*Pages/Index*</span></span> |
| <span data-ttu-id="684a8-362">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="684a8-362">RedirectToPage("Index")</span></span>  | <span data-ttu-id="684a8-363">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="684a8-363">*Pages/Customers/Index*</span></span> |

<!-- Test via ~/razor-pages/index/3.0sample/RazorPagesContacts/Pages/Customers/Details.cshtml.cs -->

<span data-ttu-id="684a8-364">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")` are *relative names*.</span><span class="sxs-lookup"><span data-stu-id="684a8-364">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")` are *relative names*.</span></span> <span data-ttu-id="684a8-365">Le paramètre `RedirectToPage` est *combiné* avec le chemin de la page active pour calculer le nom de la page de destination.</span><span class="sxs-lookup"><span data-stu-id="684a8-365">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>

<span data-ttu-id="684a8-366">La liaison de nom relatif est utile lors de la création de sites avec une structure complexe.</span><span class="sxs-lookup"><span data-stu-id="684a8-366">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="684a8-367">When relative names are used to link between pages in a folder:</span><span class="sxs-lookup"><span data-stu-id="684a8-367">When relative names are used to link between pages in a folder:</span></span>

* <span data-ttu-id="684a8-368">Renaming a folder doesn't break the relative links.</span><span class="sxs-lookup"><span data-stu-id="684a8-368">Renaming a folder doesn't break the relative links.</span></span>
* <span data-ttu-id="684a8-369">Links are not broken because they don't include the folder name.</span><span class="sxs-lookup"><span data-stu-id="684a8-369">Links are not broken because they don't include the folder name.</span></span>

<span data-ttu-id="684a8-370">Pour rediriger vers une page située dans une autre [Zone](xref:mvc/controllers/areas), spécifiez la zone :</span><span class="sxs-lookup"><span data-stu-id="684a8-370">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="684a8-371">Pour plus d’informations, consultez <xref:mvc/controllers/areas> et <xref:razor-pages/razor-pages-conventions>.</span><span class="sxs-lookup"><span data-stu-id="684a8-371">For more information, see <xref:mvc/controllers/areas> and <xref:razor-pages/razor-pages-conventions>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="684a8-372">Attribut ViewData</span><span class="sxs-lookup"><span data-stu-id="684a8-372">ViewData attribute</span></span>

<span data-ttu-id="684a8-373">Data can be passed to a page with <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute>.</span><span class="sxs-lookup"><span data-stu-id="684a8-373">Data can be passed to a page with <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute>.</span></span> <span data-ttu-id="684a8-374">Properties with the `[ViewData]` attribute have their values stored and loaded from the <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary>.</span><span class="sxs-lookup"><span data-stu-id="684a8-374">Properties with the `[ViewData]` attribute have their values stored and loaded from the <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary>.</span></span>

<span data-ttu-id="684a8-375">In the following example, the `AboutModel` applies the `[ViewData]` attribute to the `Title` property:</span><span class="sxs-lookup"><span data-stu-id="684a8-375">In the following example, the `AboutModel` applies the `[ViewData]` attribute to the `Title` property:</span></span>

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

<span data-ttu-id="684a8-376">Dans la page À propos de, accédez à la propriété `Title` en tant que propriété de modèle :</span><span class="sxs-lookup"><span data-stu-id="684a8-376">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="684a8-377">Dans la disposition, le titre est lu à partir du dictionnaire ViewData :</span><span class="sxs-lookup"><span data-stu-id="684a8-377">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="684a8-378">TempData</span><span class="sxs-lookup"><span data-stu-id="684a8-378">TempData</span></span>

<span data-ttu-id="684a8-379">ASP.NET Core exposes the <xref:Microsoft.AspNetCore.Mvc.Controller.TempData>.</span><span class="sxs-lookup"><span data-stu-id="684a8-379">ASP.NET Core exposes the <xref:Microsoft.AspNetCore.Mvc.Controller.TempData>.</span></span> <span data-ttu-id="684a8-380">Cette propriété stocke les données jusqu’à ce qu’elles soient lues.</span><span class="sxs-lookup"><span data-stu-id="684a8-380">This property stores data until it's read.</span></span> <span data-ttu-id="684a8-381">Vous pouvez utiliser les méthodes <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*> et <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> pour examiner les données sans suppression.</span><span class="sxs-lookup"><span data-stu-id="684a8-381">The <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*> and <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="684a8-382">`TempData` is useful for redirection, when data is needed for more than a single request.</span><span class="sxs-lookup"><span data-stu-id="684a8-382">`TempData` is useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="684a8-383">Le code suivant définit la valeur de `Message` à l’aide de `TempData` :</span><span class="sxs-lookup"><span data-stu-id="684a8-383">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="684a8-384">Le balisage suivant dans le fichier *Pages/Customers/Index.cshtml* affiche la valeur de `Message` à l’aide de `TempData`.</span><span class="sxs-lookup"><span data-stu-id="684a8-384">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="684a8-385">Le modèle de page *Pages/Customers/Index.cshtml.cs* applique l’attribut `[TempData]` à la propriété `Message`.</span><span class="sxs-lookup"><span data-stu-id="684a8-385">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="684a8-386">For more information, see [TempData](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="684a8-386">For more information, see [TempData](xref:fundamentals/app-state#tempdata).</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="684a8-387">Plusieurs gestionnaires par page</span><span class="sxs-lookup"><span data-stu-id="684a8-387">Multiple handlers per page</span></span>

<span data-ttu-id="684a8-388">La page suivante génère un balisage pour deux gestionnaires en utilisant le Tag Helper `asp-page-handler` :</span><span class="sxs-lookup"><span data-stu-id="684a8-388">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<span data-ttu-id="684a8-389">Le formulaire dans l’exemple précédent contient deux boutons d’envoi, chacun utilisant `FormActionTagHelper` pour envoyer à une URL différente.</span><span class="sxs-lookup"><span data-stu-id="684a8-389">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="684a8-390">L’attribut `asp-page-handler` est un complément de `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="684a8-390">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="684a8-391">`asp-page-handler` génère des URL qui envoient à chacune des méthodes de gestionnaire définies par une page.</span><span class="sxs-lookup"><span data-stu-id="684a8-391">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="684a8-392">`asp-page` n’est pas spécifié, car l’exemple établit une liaison à la page active.</span><span class="sxs-lookup"><span data-stu-id="684a8-392">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="684a8-393">Le modèle de page :</span><span class="sxs-lookup"><span data-stu-id="684a8-393">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="684a8-394">Le code précédent utilise des *méthodes de gestionnaire nommées*.</span><span class="sxs-lookup"><span data-stu-id="684a8-394">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="684a8-395">Pour créer des méthodes de gestionnaire nommées, il faut prendre le texte dans le nom après `On<HTTP Verb>` et avant `Async` (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="684a8-395">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="684a8-396">Dans l’exemple précédent, les méthodes de page sont OnPost**JoinList**Async et OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="684a8-396">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="684a8-397">Avec *OnPost* et *Async* supprimés, les noms des gestionnaires sont `JoinList` et `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="684a8-397">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="684a8-398">Avec le code précédent, le chemin d’URL qui envoie à `OnPostJoinListAsync` est `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="684a8-398">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="684a8-399">Le chemin d’URL qui envoie à `OnPostJoinListUCAsync` est `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="684a8-399">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="684a8-400">Routes personnalisées</span><span class="sxs-lookup"><span data-stu-id="684a8-400">Custom routes</span></span>

<span data-ttu-id="684a8-401">Utilisez la directive `@page` pour :</span><span class="sxs-lookup"><span data-stu-id="684a8-401">Use the `@page` directive to:</span></span>

* <span data-ttu-id="684a8-402">Spécifier une route personnalisée vers une page.</span><span class="sxs-lookup"><span data-stu-id="684a8-402">Specify a custom route to a page.</span></span> <span data-ttu-id="684a8-403">Par exemple, la route vers la page À propos peut être définie sur `/Some/Other/Path` avec `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="684a8-403">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="684a8-404">Ajouter des segments à la route par défaut d’une page.</span><span class="sxs-lookup"><span data-stu-id="684a8-404">Append segments to a page's default route.</span></span> <span data-ttu-id="684a8-405">Par exemple, un segment « item » peut être ajouté à la route par défaut d’une page avec `@page "item"`.</span><span class="sxs-lookup"><span data-stu-id="684a8-405">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="684a8-406">Ajouter des paramètres à la route par défaut d’une page.</span><span class="sxs-lookup"><span data-stu-id="684a8-406">Append parameters to a page's default route.</span></span> <span data-ttu-id="684a8-407">Par exemple, un paramètre d’ID, `id`, peut être nécessaire pour une page avec `@page "{id}"`.</span><span class="sxs-lookup"><span data-stu-id="684a8-407">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="684a8-408">Un chemin relatif racine désigné par un tilde (`~`) au début du chemin est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="684a8-408">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="684a8-409">Par exemple, `@page "~/Some/Other/Path"` est identique à `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="684a8-409">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="684a8-410">Vous pouvez remplacer la chaîne de requête `?handler=JoinList` dans l’URL par un segment de routage `/JoinList` en spécifiant le modèle de routage `@page "{handler?}"`.</span><span class="sxs-lookup"><span data-stu-id="684a8-410">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="684a8-411">Si vous ne voulez pas avoir la chaîne de requête `?handler=JoinList` dans l’URL, vous pouvez changer l’itinéraire pour placer le nom du gestionnaire dans la partie chemin de l’URL.</span><span class="sxs-lookup"><span data-stu-id="684a8-411">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="684a8-412">Vous pouvez personnaliser l’itinéraire en ajoutant un modèle d’itinéraire placé entre des guillemets doubles après la directive `@page`.</span><span class="sxs-lookup"><span data-stu-id="684a8-412">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="684a8-413">Avec le code précédent, le chemin d’URL qui envoie à `OnPostJoinListAsync` est `https://localhost:5001/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="684a8-413">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="684a8-414">Le chemin d’URL qui envoie à `OnPostJoinListUCAsync` est `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="684a8-414">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="684a8-415">Le `?` suivant `handler` signifie que le paramètre d’itinéraire est facultatif.</span><span class="sxs-lookup"><span data-stu-id="684a8-415">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="advanced-configuration-and-settings"></a><span data-ttu-id="684a8-416">Advanced configuration and settings</span><span class="sxs-lookup"><span data-stu-id="684a8-416">Advanced configuration and settings</span></span>

<span data-ttu-id="684a8-417">The configuration and settings in following sections is not required by most apps.</span><span class="sxs-lookup"><span data-stu-id="684a8-417">The configuration and settings in following sections is not required by most apps.</span></span>

<span data-ttu-id="684a8-418">To configure advanced options, use the extension method <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>:</span><span class="sxs-lookup"><span data-stu-id="684a8-418">To configure advanced options, use the extension method <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupRPoptions.cs?name=snippet)]

<span data-ttu-id="684a8-419">Use the <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions> to set the root directory for pages, or add application model conventions for pages.</span><span class="sxs-lookup"><span data-stu-id="684a8-419">Use the <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions> to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="684a8-420">For more information on conventions, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="684a8-420">For more information on conventions, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization).</span></span>

<span data-ttu-id="684a8-421">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="684a8-421">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation).</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="684a8-422">Spécifier que les pages Razor se trouvent à la racine du contenu</span><span class="sxs-lookup"><span data-stu-id="684a8-422">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="684a8-423">Par défaut, les pages Razor sont associées à la racine */Pages*.</span><span class="sxs-lookup"><span data-stu-id="684a8-423">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="684a8-424">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) (<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>) of the app:</span><span class="sxs-lookup"><span data-stu-id="684a8-424">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) (<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>) of the app:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesAtContentRoot.cs?name=snippet)]

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="684a8-425">Spécifier que les pages Razor se trouvent dans un répertoire racine personnalisé</span><span class="sxs-lookup"><span data-stu-id="684a8-425">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="684a8-426">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcCoreBuilderExtensions.WithRazorPagesRoot*> to specify that Razor Pages are at a custom root directory in the app (provide a relative path):</span><span class="sxs-lookup"><span data-stu-id="684a8-426">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcCoreBuilderExtensions.WithRazorPagesRoot*> to specify that Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesRoot.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="684a8-427">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="684a8-427">Additional resources</span></span>

* <span data-ttu-id="684a8-428">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction</span><span class="sxs-lookup"><span data-stu-id="684a8-428">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction</span></span>
* [<span data-ttu-id="684a8-429">Download or view sample code</span><span class="sxs-lookup"><span data-stu-id="684a8-429">Download or view sample code</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/3.0sample)
* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="684a8-430">De [Rick Anderson](https://twitter.com/RickAndMSFT) et [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="684a8-430">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="684a8-431">Les pages Razor constituent un nouvel aspect d’ASP.NET Core MVC qui permet de développer des scénarios orientés page de façon plus simple et plus productive.</span><span class="sxs-lookup"><span data-stu-id="684a8-431">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="684a8-432">Si vous cherchez un didacticiel qui utilise l’approche Model-View-Controller, consultez [Bien démarrer avec ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="684a8-432">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="684a8-433">Ce document fournit une introduction aux pages Razor.</span><span class="sxs-lookup"><span data-stu-id="684a8-433">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="684a8-434">Il ne s’agit pas d’un didacticiel pas à pas.</span><span class="sxs-lookup"><span data-stu-id="684a8-434">It's not a step by step tutorial.</span></span> <span data-ttu-id="684a8-435">Si certaines sections vous semblent trop techniques, consultez [Bien démarrer avec les pages Razor](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="684a8-435">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="684a8-436">Pour une vue d’ensemble d’ASP.NET Core, consultez [Introduction à ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="684a8-436">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="684a8-437">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="684a8-437">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="684a8-438">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="684a8-438">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="684a8-439">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="684a8-439">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="684a8-440">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="684a8-440">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="684a8-441">Créer un projet Razor Pages</span><span class="sxs-lookup"><span data-stu-id="684a8-441">Create a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="684a8-442">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="684a8-442">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="684a8-443">Pour obtenir des instructions sur la création d’un projet Razor Pages, consultez [Bien démarrer avec Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="684a8-443">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="684a8-444">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="684a8-444">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="684a8-445">Exécutez `dotnet new webapp` à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="684a8-445">Run `dotnet new webapp` from the command line.</span></span>

<span data-ttu-id="684a8-446">Ouvrez le fichier *.csproj* généré à partir de Visual Studio pour Mac.</span><span class="sxs-lookup"><span data-stu-id="684a8-446">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="684a8-447">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="684a8-447">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="684a8-448">Exécutez `dotnet new webapp` à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="684a8-448">Run `dotnet new webapp` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="684a8-449">Pages Razor</span><span class="sxs-lookup"><span data-stu-id="684a8-449">Razor Pages</span></span>

<span data-ttu-id="684a8-450">La fonctionnalité Pages Razor est activée dans *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="684a8-450">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="684a8-451">Considérez une page de base : <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="684a8-451">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="684a8-452">Le code précédent ressemble beaucoup à un [fichier vue Razor](xref:tutorials/first-mvc-app/adding-view) utilisé dans une application ASP.NET Core avec des contrôleurs et des vues.</span><span class="sxs-lookup"><span data-stu-id="684a8-452">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="684a8-453">Ce qui le rend différent est la directive `@page`.</span><span class="sxs-lookup"><span data-stu-id="684a8-453">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="684a8-454">`@page` fait du fichier une action MVC, ce qui signifie qu’il gère les demandes directement, sans passer par un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="684a8-454">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="684a8-455">`@page` doit être la première directive Razor sur une page.</span><span class="sxs-lookup"><span data-stu-id="684a8-455">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="684a8-456">`@page` affecte le comportement d’autres constructions Razor.</span><span class="sxs-lookup"><span data-stu-id="684a8-456">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="684a8-457">Une page similaire, utilisant une classe `PageModel`, est illustrée dans les deux fichiers suivants.</span><span class="sxs-lookup"><span data-stu-id="684a8-457">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="684a8-458">Le fichier *Pages/Index2.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="684a8-458">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="684a8-459">Le modèle de page *Pages/Index2.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="684a8-459">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="684a8-460">Par convention, le fichier de classe `PageModel` a le même nom que le fichier Page Razor, avec *.cs* en plus.</span><span class="sxs-lookup"><span data-stu-id="684a8-460">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="684a8-461">Par exemple, la page Razor précédente est *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="684a8-461">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="684a8-462">Le fichier contenant la classe `PageModel` se nomme *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="684a8-462">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="684a8-463">Les associations des chemins d’URL aux pages sont déterminées par l’emplacement de la page dans le système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="684a8-463">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="684a8-464">Le tableau suivant montre un chemin Page Razor et l’URL correspondante :</span><span class="sxs-lookup"><span data-stu-id="684a8-464">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="684a8-465">Nom et chemin de fichier</span><span class="sxs-lookup"><span data-stu-id="684a8-465">File name and path</span></span>               | <span data-ttu-id="684a8-466">URL correspondante</span><span class="sxs-lookup"><span data-stu-id="684a8-466">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="684a8-467">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="684a8-467">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="684a8-468">`/` ou `/Index`</span><span class="sxs-lookup"><span data-stu-id="684a8-468">`/` or `/Index`</span></span> |
| <span data-ttu-id="684a8-469">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="684a8-469">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="684a8-470">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="684a8-470">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="684a8-471">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="684a8-471">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="684a8-472">`/Store` ou `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="684a8-472">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="684a8-473">Remarques :</span><span class="sxs-lookup"><span data-stu-id="684a8-473">Notes:</span></span>

* <span data-ttu-id="684a8-474">Le runtime recherche les fichiers Pages Razor dans le dossier *Pages* par défaut.</span><span class="sxs-lookup"><span data-stu-id="684a8-474">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="684a8-475">`Index` est la page par défaut quand une URL n’inclut pas de page.</span><span class="sxs-lookup"><span data-stu-id="684a8-475">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="684a8-476">Écrire un formulaire de base</span><span class="sxs-lookup"><span data-stu-id="684a8-476">Write a basic form</span></span>

<span data-ttu-id="684a8-477">Les pages Razor sont conçues pour que les modèles courants utilisés avec les navigateurs web soient faciles à implémenter lors de la création d’une application.</span><span class="sxs-lookup"><span data-stu-id="684a8-477">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="684a8-478">La [liaison de modèle](xref:mvc/models/model-binding), les [Tag Helpers](xref:mvc/views/tag-helpers/intro) et les assistances HTML *fonctionnent tous* avec les propriétés définies dans une classe Page Razor.</span><span class="sxs-lookup"><span data-stu-id="684a8-478">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="684a8-479">Considérez une page qui implémente un formulaire « Nous contacter » de base pour le modèle `Contact` :</span><span class="sxs-lookup"><span data-stu-id="684a8-479">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="684a8-480">Pour les exemples de ce document, le `DbContext` est initialisé dans le fichier [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).</span><span class="sxs-lookup"><span data-stu-id="684a8-480">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="684a8-481">Le modèle de données :</span><span class="sxs-lookup"><span data-stu-id="684a8-481">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="684a8-482">Le contexte de la base de données :</span><span class="sxs-lookup"><span data-stu-id="684a8-482">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="684a8-483">Le fichier vue *Pages/Create.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="684a8-483">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="684a8-484">Le modèle de page *Pages/Create.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="684a8-484">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="684a8-485">Par convention, la classe `PageModel` se nomme `<PageName>Model` et se trouve dans le même espace de noms que la page.</span><span class="sxs-lookup"><span data-stu-id="684a8-485">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="684a8-486">La classe `PageModel` permet de séparer la logique d’une page de sa présentation.</span><span class="sxs-lookup"><span data-stu-id="684a8-486">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="684a8-487">Elle définit des gestionnaires de page pour les demandes envoyées à la page et les données utilisées pour l’afficher.</span><span class="sxs-lookup"><span data-stu-id="684a8-487">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="684a8-488">This separation allows:</span><span class="sxs-lookup"><span data-stu-id="684a8-488">This separation allows:</span></span>

* <span data-ttu-id="684a8-489">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="684a8-489">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="684a8-490">[Unit testing](xref:test/razor-pages-tests) the pages.</span><span class="sxs-lookup"><span data-stu-id="684a8-490">[Unit testing](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="684a8-491">La page a une *méthode de gestionnaire* `OnPostAsync`, qui s’exécute sur les requêtes `POST` (quand un utilisateur poste le formulaire).</span><span class="sxs-lookup"><span data-stu-id="684a8-491">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="684a8-492">Vous pouvez ajouter des méthodes de gestionnaire pour n’importe quel verbe HTTP.</span><span class="sxs-lookup"><span data-stu-id="684a8-492">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="684a8-493">Les gestionnaires les plus courants sont :</span><span class="sxs-lookup"><span data-stu-id="684a8-493">The most common handlers are:</span></span>

* <span data-ttu-id="684a8-494">`OnGet` pour initialiser l’état nécessaire pour la page.</span><span class="sxs-lookup"><span data-stu-id="684a8-494">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="684a8-495">Exemple [OnGet](#OnGet).</span><span class="sxs-lookup"><span data-stu-id="684a8-495">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="684a8-496">`OnPost` pour gérer les envois de formulaire.</span><span class="sxs-lookup"><span data-stu-id="684a8-496">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="684a8-497">Le suffixe de nommage `Async` est facultatif, mais souvent utilisé par convention pour les fonctions asynchrones.</span><span class="sxs-lookup"><span data-stu-id="684a8-497">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="684a8-498">Le code précédent est typique des pages Razor.</span><span class="sxs-lookup"><span data-stu-id="684a8-498">The preceding code is typical for Razor Pages.</span></span>

<span data-ttu-id="684a8-499">If you're familiar with ASP.NET apps using controllers and views:</span><span class="sxs-lookup"><span data-stu-id="684a8-499">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="684a8-500">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span><span class="sxs-lookup"><span data-stu-id="684a8-500">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="684a8-501">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), [Validation](xref:mvc/models/validation),  and action results are shared.</span><span class="sxs-lookup"><span data-stu-id="684a8-501">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), [Validation](xref:mvc/models/validation),  and action results are shared.</span></span>

<span data-ttu-id="684a8-502">La méthode `OnPostAsync` précédente :</span><span class="sxs-lookup"><span data-stu-id="684a8-502">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="684a8-503">Le flux de base de `OnPostAsync` :</span><span class="sxs-lookup"><span data-stu-id="684a8-503">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="684a8-504">Recherchez les erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="684a8-504">Check for validation errors.</span></span>

* <span data-ttu-id="684a8-505">S’il n’y a aucune erreur, enregistrez les données et redirigez.</span><span class="sxs-lookup"><span data-stu-id="684a8-505">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="684a8-506">S’il y a des erreurs, réaffichez la page avec des messages de validation.</span><span class="sxs-lookup"><span data-stu-id="684a8-506">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="684a8-507">La validation côté client est identique à celle des applications ASP.NET Core MVC traditionnelles.</span><span class="sxs-lookup"><span data-stu-id="684a8-507">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="684a8-508">Dans de nombreux cas, les erreurs de validation sont détectées sur le client et jamais envoyées au serveur.</span><span class="sxs-lookup"><span data-stu-id="684a8-508">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="684a8-509">Quand les données sont entrées correctement, la méthode de gestionnaire `OnPostAsync` appelle la méthode d’assistance `RedirectToPage` pour retourner une instance de `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="684a8-509">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="684a8-510">`RedirectToPage` est un nouveau résultat d’action, semblable à `RedirectToAction` ou `RedirectToRoute`, mais personnalisé pour les pages.</span><span class="sxs-lookup"><span data-stu-id="684a8-510">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="684a8-511">Dans l’exemple précédent, il redirige vers la page Index racine (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="684a8-511">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="684a8-512">`RedirectToPage` est détaillé dans la section [Génération d’URL pour les pages](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="684a8-512">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="684a8-513">Quand le formulaire envoyé comporte des erreurs de validation (qui sont passées au serveur), la méthode de gestionnaire `OnPostAsync` appelle la méthode d’assistance `Page`.</span><span class="sxs-lookup"><span data-stu-id="684a8-513">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="684a8-514">`Page` retourne une instance de `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="684a8-514">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="684a8-515">Le retour de `Page` est similaire à la façon dont les actions dans les contrôleurs retournent `View`.</span><span class="sxs-lookup"><span data-stu-id="684a8-515">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="684a8-516">`PageResult` is the default return type for a handler method.</span><span class="sxs-lookup"><span data-stu-id="684a8-516">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="684a8-517">Une méthode de gestionnaire qui retourne `void` restitue la page.</span><span class="sxs-lookup"><span data-stu-id="684a8-517">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="684a8-518">La propriété `Customer` utilise l’attribut `[BindProperty]` pour accepter la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="684a8-518">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="684a8-519">Par défaut, Razor Pages lie les propriétés seulement avec des verbes non-`GET`.</span><span class="sxs-lookup"><span data-stu-id="684a8-519">Razor Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="684a8-520">La liaison aux propriétés peut réduire la quantité de code à écrire.</span><span class="sxs-lookup"><span data-stu-id="684a8-520">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="684a8-521">Elle réduit la quantité de code en utilisant la même propriété pour afficher les champs de formulaire (`<input asp-for="Customer.Name">`) et accepter l’entrée.</span><span class="sxs-lookup"><span data-stu-id="684a8-521">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="684a8-522">La page d’accueil (*Index.cshtml*) :</span><span class="sxs-lookup"><span data-stu-id="684a8-522">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="684a8-523">La classe `PageModel` associée (*Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="684a8-523">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="684a8-524">Le fichier *Index.cshtml* contient le balisage suivant pour créer un lien d’édition pour chaque contact :</span><span class="sxs-lookup"><span data-stu-id="684a8-524">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="684a8-525">The `<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span><span class="sxs-lookup"><span data-stu-id="684a8-525">The `<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="684a8-526">Le lien contient des données d’itinéraire avec l’ID de contact.</span><span class="sxs-lookup"><span data-stu-id="684a8-526">The link contains route data with the contact ID.</span></span> <span data-ttu-id="684a8-527">Par exemple, `https://localhost:5001/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="684a8-527">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="684a8-528">Les [Tag Helpers](xref:mvc/views/tag-helpers/intro) permettent au code côté serveur de participer à la création et au rendu des éléments HTML dans les fichiers Razor.</span><span class="sxs-lookup"><span data-stu-id="684a8-528">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="684a8-529">Tag Helpers are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span><span class="sxs-lookup"><span data-stu-id="684a8-529">Tag Helpers are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="684a8-530">Le fichier *Pages/Edit.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="684a8-530">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="684a8-531">La première ligne contient la directive `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="684a8-531">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="684a8-532">La contrainte de routage `"{id:int}"` indique à la page qu’elle doit accepter les requêtes qui contiennent des données d’itinéraire `int`.</span><span class="sxs-lookup"><span data-stu-id="684a8-532">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="684a8-533">Si une requête à la page ne contient de données d’itinéraire qui peuvent être converties en `int`, le runtime retourne une erreur HTTP 404 (introuvable).</span><span class="sxs-lookup"><span data-stu-id="684a8-533">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="684a8-534">Pour que l’ID soit facultatif, ajoutez `?` à la contrainte de route :</span><span class="sxs-lookup"><span data-stu-id="684a8-534">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="684a8-535">Le fichier *Pages/Edit.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="684a8-535">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="684a8-536">Le fichier *Index.cshtml* contient également le balisage pour créer un bouton Supprimer pour chaque contact client :</span><span class="sxs-lookup"><span data-stu-id="684a8-536">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="684a8-537">Lorsque le bouton Supprimer est rendu en HTML, son `formaction` comprend des paramètres pour :</span><span class="sxs-lookup"><span data-stu-id="684a8-537">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="684a8-538">L’ID du contact client spécifié par l’attribut `asp-route-id`.</span><span class="sxs-lookup"><span data-stu-id="684a8-538">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="684a8-539">Le `handler` spécifié par l’attribut `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="684a8-539">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="684a8-540">Voici un exemple de bouton Supprimer rendu avec un ID de contact client de `1`:</span><span class="sxs-lookup"><span data-stu-id="684a8-540">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="684a8-541">Quand le bouton est sélectionné, une demande `POST` de formulaire est envoyée au serveur.</span><span class="sxs-lookup"><span data-stu-id="684a8-541">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="684a8-542">Par convention, le nom de la méthode de gestionnaire est sélectionné en fonction de la valeur du paramètre `handler` conformément au schéma `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="684a8-542">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="684a8-543">Étant donné que le `handler` est `delete` dans cet exemple, la méthode de gestionnaire `OnPostDeleteAsync` est utilisée pour traiter la demande `POST`.</span><span class="sxs-lookup"><span data-stu-id="684a8-543">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="684a8-544">Si `asp-page-handler` est défini sur une autre valeur, comme `remove`, une méthode de gestionnaire avec le nom `OnPostRemoveAsync` est sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="684a8-544">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span> <span data-ttu-id="684a8-545">The following code shows the `OnPostDeleteAsync` handler:</span><span class="sxs-lookup"><span data-stu-id="684a8-545">The following code shows the `OnPostDeleteAsync` handler:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="684a8-546">La méthode `OnPostDeleteAsync` :</span><span class="sxs-lookup"><span data-stu-id="684a8-546">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="684a8-547">Accepte l’`id` de la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="684a8-547">Accepts the `id` from the query string.</span></span> <span data-ttu-id="684a8-548">If the *Index.cshtml* page directive contained routing constraint `"{id:int?}"`, `id` would come from route data.</span><span class="sxs-lookup"><span data-stu-id="684a8-548">If the *Index.cshtml* page directive contained routing constraint `"{id:int?}"`, `id` would come from route data.</span></span> <span data-ttu-id="684a8-549">The route data for `id` is specified in the URI such as `https://localhost:5001/Customers/2`.</span><span class="sxs-lookup"><span data-stu-id="684a8-549">The route data for `id` is specified in the URI such as `https://localhost:5001/Customers/2`.</span></span>
* <span data-ttu-id="684a8-550">Interroge la base de données pour le contact client avec `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="684a8-550">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="684a8-551">Si le contact client est trouvé, il est supprimé de la liste des contacts client.</span><span class="sxs-lookup"><span data-stu-id="684a8-551">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="684a8-552">La base de données est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="684a8-552">The database is updated.</span></span>
* <span data-ttu-id="684a8-553">Appelle `RedirectToPage` pour rediriger vers la page Index racine (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="684a8-553">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

## <a name="mark-page-properties-as-required"></a><span data-ttu-id="684a8-554">Marquer les propriétés de page comme Required</span><span class="sxs-lookup"><span data-stu-id="684a8-554">Mark page properties as required</span></span>

<span data-ttu-id="684a8-555">Les propriétés définies sur `PageModel` peuvent être décorées avec l’attribut [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) :</span><span class="sxs-lookup"><span data-stu-id="684a8-555">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

<span data-ttu-id="684a8-556">Pour plus d’informations, consultez [Validation de modèle](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="684a8-556">For more information, see [Model validation](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="684a8-557">Gérer les requêtes HEAD avec un gestionnaire OnGet de secours</span><span class="sxs-lookup"><span data-stu-id="684a8-557">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="684a8-558">Les requêtes `HEAD` vous permettent de récupérer les en-têtes pour une ressource spécifique.</span><span class="sxs-lookup"><span data-stu-id="684a8-558">`HEAD` requests allow you to retrieve the headers for a specific resource.</span></span> <span data-ttu-id="684a8-559">Contrairement aux requêtes `GET`, les requêtes `HEAD` ne retournent pas un corps de réponse.</span><span class="sxs-lookup"><span data-stu-id="684a8-559">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="684a8-560">En règle générale, un gestionnaire `OnHead` est créé et appelé pour les requêtes `HEAD` :</span><span class="sxs-lookup"><span data-stu-id="684a8-560">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span> 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="684a8-561">Dans ASP.NET Core 2.1 ou ultérieur, Razor Pages se rabat sur un appel du gestionnaire `OnGet` si aucun gestionnaire `OnHead` n’est défini.</span><span class="sxs-lookup"><span data-stu-id="684a8-561">In ASP.NET Core 2.1 or later, Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span> <span data-ttu-id="684a8-562">Ce comportement est activé par l’appel à [SetCompatibilityVersion](xref:mvc/compatibility-version) dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="684a8-562">This behavior is enabled by the call to [SetCompatibilityVersion](xref:mvc/compatibility-version) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="684a8-563">Les modèles par défaut génèrent l'appel `SetCompatibilityVersion` dans ASP.NET Core 2.1 et 2.2.</span><span class="sxs-lookup"><span data-stu-id="684a8-563">The default templates generate the `SetCompatibilityVersion` call in ASP.NET Core 2.1 and 2.2.</span></span> <span data-ttu-id="684a8-564">`SetCompatibilityVersion` définit efficacement l’option Pages Razor `AllowMappingHeadRequestsToGetHandler` sur `true`.</span><span class="sxs-lookup"><span data-stu-id="684a8-564">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="684a8-565">Au lieu d’adhérer à tous les comportements avec `SetCompatibilityVersion`, vous pouvez adhérer explicitement à des comportements *spécifiques*.</span><span class="sxs-lookup"><span data-stu-id="684a8-565">Rather than opting in to all behaviors with `SetCompatibilityVersion`, you can explicitly opt in to *specific* behaviors.</span></span> <span data-ttu-id="684a8-566">Le code suivant adhère à l’autorisation de mappage des requêtes `HEAD` au gestionnaire `OnGet` :</span><span class="sxs-lookup"><span data-stu-id="684a8-566">The following code opts in to allowing `HEAD` requests to be mapped to the `OnGet` handler:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="684a8-567">XSRF/CSRF et pages Razor</span><span class="sxs-lookup"><span data-stu-id="684a8-567">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="684a8-568">Vous n’avez aucun code à écrire pour la [validation anti-contrefaçon](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="684a8-568">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="684a8-569">La validation et la génération de jetons anti-contrefaçon sont automatiquement incluses dans les pages Razor.</span><span class="sxs-lookup"><span data-stu-id="684a8-569">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="684a8-570">Utilisation de dispositions, partiels, modèles et Tag Helpers avec les pages Razor</span><span class="sxs-lookup"><span data-stu-id="684a8-570">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="684a8-571">Les pages Razor fonctionnent avec toutes les fonctionnalités du moteur de vue Razor.</span><span class="sxs-lookup"><span data-stu-id="684a8-571">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="684a8-572">Les dispositions, partiels, modèles, Tag Helpers, *_ViewStart.cshtml* et *_ViewImports.cshtml* fonctionnent de la même façon que pour les vues Razor classiques.</span><span class="sxs-lookup"><span data-stu-id="684a8-572">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="684a8-573">Nous allons nettoyer un peu cette page en tirant parti de certaines de ces fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="684a8-573">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="684a8-574">Ajoutez une [page de disposition](xref:mvc/views/layout) à *Pages/Shared/_Layout.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="684a8-574">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="684a8-575">La [disposition](xref:mvc/views/layout) :</span><span class="sxs-lookup"><span data-stu-id="684a8-575">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="684a8-576">Contrôle la disposition de chaque page (à moins que la page ne refuse la disposition).</span><span class="sxs-lookup"><span data-stu-id="684a8-576">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="684a8-577">Importe des structures HTML telles que JavaScript et les feuilles de style.</span><span class="sxs-lookup"><span data-stu-id="684a8-577">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="684a8-578">Pour plus d’informations, consultez [Page de disposition](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="684a8-578">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="684a8-579">La propriété [Layout](xref:mvc/views/layout#specifying-a-layout) est définie dans *Pages/_ViewStart.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="684a8-579">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="684a8-580">La disposition est dans le dossier *Pages/Shared*.</span><span class="sxs-lookup"><span data-stu-id="684a8-580">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="684a8-581">Les pages recherchent d’autres vues (dispositions, modèles, partiels) hiérarchiquement, en commençant dans le même dossier que la page active.</span><span class="sxs-lookup"><span data-stu-id="684a8-581">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="684a8-582">Une disposition dans le dossier *Pages/Shared* peut être utilisée à partir de n’importe quelle page Razor sous le dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="684a8-582">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="684a8-583">Le fichier de disposition doit être placé dans le dossier *Pages/Shared*.</span><span class="sxs-lookup"><span data-stu-id="684a8-583">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="684a8-584">Nous vous recommandons de **ne pas** placer le fichier de disposition dans le dossier *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="684a8-584">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="684a8-585">*Views/Shared* est un modèle de vues MVC.</span><span class="sxs-lookup"><span data-stu-id="684a8-585">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="684a8-586">Les pages Razor sont censées s’appuyer sur la hiérarchie des dossiers, pas sur les conventions de chemins.</span><span class="sxs-lookup"><span data-stu-id="684a8-586">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="684a8-587">La recherche de vue à partir d’une page Razor inclut le dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="684a8-587">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="684a8-588">Les dispositions, modèles et partiels que vous utilisez avec les contrôleurs MVC et les vues Razor conventionnelles *fonctionnent simplement*.</span><span class="sxs-lookup"><span data-stu-id="684a8-588">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="684a8-589">Ajoutez un fichier *Pages/_ViewImports.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="684a8-589">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="684a8-590">`@namespace` est expliqué plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="684a8-590">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="684a8-591">La directive `@addTagHelper` permet de bénéficier des [Tag Helpers intégrés](xref:mvc/views/tag-helpers/builtin-th/Index) dans toutes les pages du dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="684a8-591">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="684a8-592">Quand la directive `@namespace` est utilisée explicitement sur une page :</span><span class="sxs-lookup"><span data-stu-id="684a8-592">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="684a8-593">La directive définit l’espace de noms pour la page.</span><span class="sxs-lookup"><span data-stu-id="684a8-593">The directive sets the namespace for the page.</span></span> <span data-ttu-id="684a8-594">La directive `@model` n’a pas besoin d’inclure l’espace de noms.</span><span class="sxs-lookup"><span data-stu-id="684a8-594">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="684a8-595">Quand la directive `@namespace` est contenue dans *_ViewImports.cshtml*, l’espace de noms spécifié fournit le préfixe de l’espace de noms généré dans la Page qui importe la directive `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="684a8-595">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="684a8-596">Le reste de l’espace de noms généré (la partie suffixe) est le chemin relatif séparé par un point entre le dossier contenant *_ViewImports.cshtml* et le dossier contenant la page.</span><span class="sxs-lookup"><span data-stu-id="684a8-596">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="684a8-597">Par exemple, la classe `PageModel` *Pages/Customers/Edit.cshtml.cs* définit explicitement l’espace de noms :</span><span class="sxs-lookup"><span data-stu-id="684a8-597">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="684a8-598">Le fichier *Pages/_ViewImports.cshtml* définit l’espace de noms suivant :</span><span class="sxs-lookup"><span data-stu-id="684a8-598">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="684a8-599">L’espace de noms généré pour la page Razor *Pages/Customers/Edit.cshtml* est identique à la classe `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="684a8-599">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="684a8-600">`@namespace` *fonctionne également avec les vues Razor classiques*.</span><span class="sxs-lookup"><span data-stu-id="684a8-600">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="684a8-601">Le fichier de vue *Pages/Create.cshtml* d’origine :</span><span class="sxs-lookup"><span data-stu-id="684a8-601">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="684a8-602">Le fichier vue *Pages/Create.cshtml* mis à jour :</span><span class="sxs-lookup"><span data-stu-id="684a8-602">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="684a8-603">Le [projet de démarrage de pages Razor](#rpvs17) contient *Pages/_ValidationScriptsPartial.cshtml*, qui connecte la validation côté client.</span><span class="sxs-lookup"><span data-stu-id="684a8-603">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="684a8-604">Pour plus d'informations sur les affichages partiels, consultez <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="684a8-604">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="684a8-605">Génération d’URL pour les pages</span><span class="sxs-lookup"><span data-stu-id="684a8-605">URL generation for Pages</span></span>

<span data-ttu-id="684a8-606">La page `Create`, illustrée précédemment, utilise `RedirectToPage` :</span><span class="sxs-lookup"><span data-stu-id="684a8-606">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="684a8-607">L’application a la structure de fichiers/dossiers suivante :</span><span class="sxs-lookup"><span data-stu-id="684a8-607">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="684a8-608">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="684a8-608">*/Pages*</span></span>

  * <span data-ttu-id="684a8-609">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="684a8-609">*Index.cshtml*</span></span>
  * <span data-ttu-id="684a8-610">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="684a8-610">*/Customers*</span></span>

    * <span data-ttu-id="684a8-611">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="684a8-611">*Create.cshtml*</span></span>
    * <span data-ttu-id="684a8-612">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="684a8-612">*Edit.cshtml*</span></span>
    * <span data-ttu-id="684a8-613">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="684a8-613">*Index.cshtml*</span></span>

<span data-ttu-id="684a8-614">Une fois l’opération réussie, les pages *Pages/Customers/Create.cshtml* et *Pages/Customers/Edit.cshtml* redirigent vers *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="684a8-614">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="684a8-615">La chaîne `/Index` fait partie de l’URI pour accéder à la page précédente.</span><span class="sxs-lookup"><span data-stu-id="684a8-615">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="684a8-616">La chaîne `/Index` peut être utilisée pour générer l’URI de la page *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="684a8-616">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="684a8-617">Exemple :</span><span class="sxs-lookup"><span data-stu-id="684a8-617">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="684a8-618">Le nom de la page est le chemin de la page à partir du dossier racine */Pages* avec un `/` devant (par exemple, `/Index`).</span><span class="sxs-lookup"><span data-stu-id="684a8-618">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="684a8-619">Les exemples de génération d’URL précédents offrent des options améliorées et des capacités fonctionnelles sur le codage en dur d’une URL.</span><span class="sxs-lookup"><span data-stu-id="684a8-619">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="684a8-620">La génération d’URL utilise le [routage](xref:mvc/controllers/routing) et peut générer et encoder des paramètres en fonction de la façon dont l’itinéraire est défini dans le chemin de destination.</span><span class="sxs-lookup"><span data-stu-id="684a8-620">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="684a8-621">La génération d’URL pour les pages prend en charge les noms relatifs.</span><span class="sxs-lookup"><span data-stu-id="684a8-621">URL generation for pages supports relative names.</span></span> <span data-ttu-id="684a8-622">Le tableau suivant montre la page Index sélectionnée avec différents paramètres `RedirectToPage` à partir de *Pages/Customers/Create.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="684a8-622">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="684a8-623">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="684a8-623">RedirectToPage(x)</span></span>| <span data-ttu-id="684a8-624">Page</span><span class="sxs-lookup"><span data-stu-id="684a8-624">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="684a8-625">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="684a8-625">RedirectToPage("/Index")</span></span> | <span data-ttu-id="684a8-626">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="684a8-626">*Pages/Index*</span></span> |
| <span data-ttu-id="684a8-627">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="684a8-627">RedirectToPage("./Index");</span></span> | <span data-ttu-id="684a8-628">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="684a8-628">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="684a8-629">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="684a8-629">RedirectToPage("../Index")</span></span> | <span data-ttu-id="684a8-630">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="684a8-630">*Pages/Index*</span></span> |
| <span data-ttu-id="684a8-631">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="684a8-631">RedirectToPage("Index")</span></span>  | <span data-ttu-id="684a8-632">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="684a8-632">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="684a8-633">`RedirectToPage("Index")`, `RedirectToPage("./Index")` et `RedirectToPage("../Index")` sont des *noms relatifs*.</span><span class="sxs-lookup"><span data-stu-id="684a8-633">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="684a8-634">Le paramètre `RedirectToPage` est *combiné* avec le chemin de la page active pour calculer le nom de la page de destination.</span><span class="sxs-lookup"><span data-stu-id="684a8-634">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="684a8-635">La liaison de nom relatif est utile lors de la création de sites avec une structure complexe.</span><span class="sxs-lookup"><span data-stu-id="684a8-635">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="684a8-636">Si vous utilisez des noms relatifs pour établir une liaison entre les pages d’un dossier, vous pouvez renommer ce dossier.</span><span class="sxs-lookup"><span data-stu-id="684a8-636">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="684a8-637">Tous les liens fonctionneront encore (car ils n’incluent pas le nom du dossier).</span><span class="sxs-lookup"><span data-stu-id="684a8-637">All the links still work (because they didn't include the folder name).</span></span>

<span data-ttu-id="684a8-638">Pour rediriger vers une page située dans une autre [Zone](xref:mvc/controllers/areas), spécifiez la zone :</span><span class="sxs-lookup"><span data-stu-id="684a8-638">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="684a8-639">Pour plus d'informations, consultez <xref:mvc/controllers/areas>.</span><span class="sxs-lookup"><span data-stu-id="684a8-639">For more information, see <xref:mvc/controllers/areas>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="684a8-640">Attribut ViewData</span><span class="sxs-lookup"><span data-stu-id="684a8-640">ViewData attribute</span></span>

<span data-ttu-id="684a8-641">Les données peuvent être passées à une page avec [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span><span class="sxs-lookup"><span data-stu-id="684a8-641">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="684a8-642">Les valeurs des propriétés définies sur des contrôleurs ou sur des modèles de page Razor décorés avec `[ViewData]` sont stockées et chargées à partir de [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span><span class="sxs-lookup"><span data-stu-id="684a8-642">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="684a8-643">Dans l’exemple suivant, `AboutModel` contient une propriété `Title` décorée avec `[ViewData]`.</span><span class="sxs-lookup"><span data-stu-id="684a8-643">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="684a8-644">La propriété `Title` a pour valeur le titre de la page À propos de :</span><span class="sxs-lookup"><span data-stu-id="684a8-644">The `Title` property is set to the title of the About page:</span></span>

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

<span data-ttu-id="684a8-645">Dans la page À propos de, accédez à la propriété `Title` en tant que propriété de modèle :</span><span class="sxs-lookup"><span data-stu-id="684a8-645">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="684a8-646">Dans la disposition, le titre est lu à partir du dictionnaire ViewData :</span><span class="sxs-lookup"><span data-stu-id="684a8-646">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="684a8-647">TempData</span><span class="sxs-lookup"><span data-stu-id="684a8-647">TempData</span></span>

<span data-ttu-id="684a8-648">ASP.NET Core expose la propriété [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) sur un [contrôleur](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="684a8-648">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="684a8-649">Cette propriété stocke les données jusqu’à ce qu’elles soient lues.</span><span class="sxs-lookup"><span data-stu-id="684a8-649">This property stores data until it's read.</span></span> <span data-ttu-id="684a8-650">Vous pouvez utiliser les méthodes `Keep` et `Peek` pour examiner les données sans suppression.</span><span class="sxs-lookup"><span data-stu-id="684a8-650">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="684a8-651">`TempData` est utile pour la redirection, quand des données sont nécessaires pour plusieurs requêtes.</span><span class="sxs-lookup"><span data-stu-id="684a8-651">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="684a8-652">Le code suivant définit la valeur de `Message` à l’aide de `TempData` :</span><span class="sxs-lookup"><span data-stu-id="684a8-652">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="684a8-653">Le balisage suivant dans le fichier *Pages/Customers/Index.cshtml* affiche la valeur de `Message` à l’aide de `TempData`.</span><span class="sxs-lookup"><span data-stu-id="684a8-653">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="684a8-654">Le modèle de page *Pages/Customers/Index.cshtml.cs* applique l’attribut `[TempData]` à la propriété `Message`.</span><span class="sxs-lookup"><span data-stu-id="684a8-654">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="684a8-655">Pour plus d’informations, consultez [TempData](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="684a8-655">For more information, see [TempData](xref:fundamentals/app-state#tempdata) .</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="684a8-656">Plusieurs gestionnaires par page</span><span class="sxs-lookup"><span data-stu-id="684a8-656">Multiple handlers per page</span></span>

<span data-ttu-id="684a8-657">La page suivante génère un balisage pour deux gestionnaires en utilisant le Tag Helper `asp-page-handler` :</span><span class="sxs-lookup"><span data-stu-id="684a8-657">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="684a8-658">Le formulaire dans l’exemple précédent contient deux boutons d’envoi, chacun utilisant `FormActionTagHelper` pour envoyer à une URL différente.</span><span class="sxs-lookup"><span data-stu-id="684a8-658">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="684a8-659">L’attribut `asp-page-handler` est un complément de `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="684a8-659">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="684a8-660">`asp-page-handler` génère des URL qui envoient à chacune des méthodes de gestionnaire définies par une page.</span><span class="sxs-lookup"><span data-stu-id="684a8-660">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="684a8-661">`asp-page` n’est pas spécifié, car l’exemple établit une liaison à la page active.</span><span class="sxs-lookup"><span data-stu-id="684a8-661">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="684a8-662">Le modèle de page :</span><span class="sxs-lookup"><span data-stu-id="684a8-662">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="684a8-663">Le code précédent utilise des *méthodes de gestionnaire nommées*.</span><span class="sxs-lookup"><span data-stu-id="684a8-663">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="684a8-664">Pour créer des méthodes de gestionnaire nommées, il faut prendre le texte dans le nom après `On<HTTP Verb>` et avant `Async` (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="684a8-664">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="684a8-665">Dans l’exemple précédent, les méthodes de page sont OnPost**JoinList**Async et OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="684a8-665">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="684a8-666">Avec *OnPost* et *Async* supprimés, les noms des gestionnaires sont `JoinList` et `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="684a8-666">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="684a8-667">Avec le code précédent, le chemin d’URL qui envoie à `OnPostJoinListAsync` est `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="684a8-667">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="684a8-668">Le chemin d’URL qui envoie à `OnPostJoinListUCAsync` est `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="684a8-668">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="684a8-669">Routes personnalisées</span><span class="sxs-lookup"><span data-stu-id="684a8-669">Custom routes</span></span>

<span data-ttu-id="684a8-670">Utilisez la directive `@page` pour :</span><span class="sxs-lookup"><span data-stu-id="684a8-670">Use the `@page` directive to:</span></span>

* <span data-ttu-id="684a8-671">Spécifier une route personnalisée vers une page.</span><span class="sxs-lookup"><span data-stu-id="684a8-671">Specify a custom route to a page.</span></span> <span data-ttu-id="684a8-672">Par exemple, la route vers la page À propos peut être définie sur `/Some/Other/Path` avec `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="684a8-672">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="684a8-673">Ajouter des segments à la route par défaut d’une page.</span><span class="sxs-lookup"><span data-stu-id="684a8-673">Append segments to a page's default route.</span></span> <span data-ttu-id="684a8-674">Par exemple, un segment « item » peut être ajouté à la route par défaut d’une page avec `@page "item"`.</span><span class="sxs-lookup"><span data-stu-id="684a8-674">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="684a8-675">Ajouter des paramètres à la route par défaut d’une page.</span><span class="sxs-lookup"><span data-stu-id="684a8-675">Append parameters to a page's default route.</span></span> <span data-ttu-id="684a8-676">Par exemple, un paramètre d’ID, `id`, peut être nécessaire pour une page avec `@page "{id}"`.</span><span class="sxs-lookup"><span data-stu-id="684a8-676">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="684a8-677">Un chemin relatif racine désigné par un tilde (`~`) au début du chemin est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="684a8-677">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="684a8-678">Par exemple, `@page "~/Some/Other/Path"` est identique à `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="684a8-678">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="684a8-679">Vous pouvez remplacer la chaîne de requête `?handler=JoinList` dans l’URL par un segment de routage `/JoinList` en spécifiant le modèle de routage `@page "{handler?}"`.</span><span class="sxs-lookup"><span data-stu-id="684a8-679">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="684a8-680">Si vous ne voulez pas avoir la chaîne de requête `?handler=JoinList` dans l’URL, vous pouvez changer l’itinéraire pour placer le nom du gestionnaire dans la partie chemin de l’URL.</span><span class="sxs-lookup"><span data-stu-id="684a8-680">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="684a8-681">Vous pouvez personnaliser l’itinéraire en ajoutant un modèle d’itinéraire placé entre des guillemets doubles après la directive `@page`.</span><span class="sxs-lookup"><span data-stu-id="684a8-681">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="684a8-682">Avec le code précédent, le chemin d’URL qui envoie à `OnPostJoinListAsync` est `https://localhost:5001/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="684a8-682">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="684a8-683">Le chemin d’URL qui envoie à `OnPostJoinListUCAsync` est `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="684a8-683">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="684a8-684">Le `?` suivant `handler` signifie que le paramètre d’itinéraire est facultatif.</span><span class="sxs-lookup"><span data-stu-id="684a8-684">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="684a8-685">Configuration et paramètres</span><span class="sxs-lookup"><span data-stu-id="684a8-685">Configuration and settings</span></span>

<span data-ttu-id="684a8-686">Pour configurer les options avancées, utilisez la méthode d’extension `AddRazorPagesOptions` sur le générateur MVC :</span><span class="sxs-lookup"><span data-stu-id="684a8-686">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="684a8-687">Actuellement, vous pouvez utiliser `RazorPagesOptions` pour définir le répertoire racine pour les pages, ou ajouter des conventions de modèle d’application pour les pages.</span><span class="sxs-lookup"><span data-stu-id="684a8-687">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="684a8-688">Nous permettrons à l’avenir une plus grande extensibilité en ce sens.</span><span class="sxs-lookup"><span data-stu-id="684a8-688">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="684a8-689">Pour précompiler des vues, consultez [Compilation de vue Razor](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="684a8-689">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="684a8-690">[Téléchargez ou affichez des exemples de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="684a8-690">[Download or view sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="684a8-691">Consultez [Bien démarrer avec les pages Razor](xref:tutorials/razor-pages/razor-pages-start) qui s’appuie sur cette introduction.</span><span class="sxs-lookup"><span data-stu-id="684a8-691">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="684a8-692">Spécifier que les pages Razor se trouvent à la racine du contenu</span><span class="sxs-lookup"><span data-stu-id="684a8-692">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="684a8-693">Par défaut, les pages Razor sont associées à la racine */Pages*.</span><span class="sxs-lookup"><span data-stu-id="684a8-693">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="684a8-694">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span><span class="sxs-lookup"><span data-stu-id="684a8-694">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="684a8-695">Spécifier que les pages Razor se trouvent dans un répertoire racine personnalisé</span><span class="sxs-lookup"><span data-stu-id="684a8-695">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="684a8-696">Ajoutez [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) à [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) pour spécifier que vos pages Razor se trouvent dans le répertoire racine personnalisé de l’application (fournissez un chemin relatif) :</span><span class="sxs-lookup"><span data-stu-id="684a8-696">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="additional-resources"></a><span data-ttu-id="684a8-697">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="684a8-697">Additional resources</span></span>

* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>

::: moniker-end
