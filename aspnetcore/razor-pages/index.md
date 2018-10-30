---
title: Présentation de Razor Pages dans ASP.NET Core
author: Rick-Anderson
description: Découvrez comment Razor Pages dans ASP.NET Core permet de développer des scénarios orientés page de façon plus simple et plus productive qu’avec MVC.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/12/2018
uid: razor-pages/index
ms.openlocfilehash: 7fc048e427fd49e2142160615a12989fd4f40303
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207613"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="3d8b6-103">Présentation de Razor Pages dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3d8b6-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="3d8b6-104">De [Rick Anderson](https://twitter.com/RickAndMSFT) et [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="3d8b6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="3d8b6-105">Razor Pages constitue un nouvel aspect d’ASP.NET Core MVC qui permet de développer des scénarios orientés page de façon plus simple et plus productive.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-105">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="3d8b6-106">Si vous cherchez un didacticiel qui utilise l’approche Model-View-Controller, consultez [Bien démarrer avec ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="3d8b6-107">Ce document fournit une introduction à Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="3d8b6-108">Il ne s’agit pas d’un didacticiel pas à pas.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="3d8b6-109">Si certaines sections vous semblent trop techniques, consultez [Bien démarrer avec Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="3d8b6-110">Pour une vue d’ensemble d’ASP.NET Core, consultez [Introduction à ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3d8b6-111">Prérequis</span><span class="sxs-lookup"><span data-stu-id="3d8b6-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="3d8b6-112">Création d’un projet Razor Pages</span><span class="sxs-lookup"><span data-stu-id="3d8b6-112">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3d8b6-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3d8b6-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3d8b6-114">Pour obtenir des instructions détaillées sur la création d’un projet Razor Pages avec Visual Studio, consultez [Bien démarrer avec Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-114">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3d8b6-115">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="3d8b6-115">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="3d8b6-116">Exécutez `dotnet new webapp` à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-116">Run `dotnet new webapp` from the command line.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="3d8b6-117">Exécutez `dotnet new razor` à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-117">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

<span data-ttu-id="3d8b6-118">Ouvrez le fichier *.csproj* généré à partir de Visual Studio pour Mac.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-118">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3d8b6-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3d8b6-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="3d8b6-120">Exécutez `dotnet new webapp` à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-120">Run `dotnet new webapp` from the command line.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="3d8b6-121">Exécutez `dotnet new razor` à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-121">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3d8b6-122">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="3d8b6-122">.NET Core CLI</span></span>](#tab/netcore-cli)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="3d8b6-123">Exécutez `dotnet new webapp` à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-123">Run `dotnet new webapp` from the command line.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="3d8b6-124">Exécutez `dotnet new razor` à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-124">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

---

## <a name="razor-pages"></a><span data-ttu-id="3d8b6-125">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="3d8b6-125">Razor Pages</span></span>

<span data-ttu-id="3d8b6-126">Razor Pages est activé dans *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-126">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="3d8b6-127">Considérez une page de base : <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="3d8b6-127">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="3d8b6-128">Le code précédent ressemble beaucoup à un fichier vue Razor.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-128">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="3d8b6-129">Ce qui le rend différent est la directive `@page`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-129">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="3d8b6-130">`@page` fait du fichier une action MVC, ce qui signifie qu’il gère les demandes directement, sans passer par un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-130">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="3d8b6-131">`@page` doit être la première directive Razor sur une page.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-131">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="3d8b6-132">`@page` affecte le comportement d’autres constructions Razor.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-132">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="3d8b6-133">Une page similaire, utilisant une classe `PageModel`, est illustrée dans les deux fichiers suivants.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-133">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="3d8b6-134">Le fichier *Pages/Index2.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-134">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="3d8b6-135">Le modèle de page *Pages/Index2.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-135">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="3d8b6-136">Par convention, le fichier de classe `PageModel` a le même nom que le fichier Page Razor, avec *.cs* en plus.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-136">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="3d8b6-137">Par exemple, la page Razor précédente est *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-137">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="3d8b6-138">Le fichier contenant la classe `PageModel` se nomme *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-138">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="3d8b6-139">Les associations des chemins d’URL aux pages sont déterminées par l’emplacement de la page dans le système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-139">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="3d8b6-140">Le tableau suivant montre un chemin Page Razor et l’URL correspondante :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-140">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="3d8b6-141">Nom et chemin de fichier</span><span class="sxs-lookup"><span data-stu-id="3d8b6-141">File name and path</span></span>               | <span data-ttu-id="3d8b6-142">URL correspondante</span><span class="sxs-lookup"><span data-stu-id="3d8b6-142">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="3d8b6-143">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3d8b6-143">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="3d8b6-144">`/` ou `/Index`</span><span class="sxs-lookup"><span data-stu-id="3d8b6-144">`/` or `/Index`</span></span> |
| <span data-ttu-id="3d8b6-145">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3d8b6-145">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="3d8b6-146">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3d8b6-146">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="3d8b6-147">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3d8b6-147">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="3d8b6-148">`/Store` ou `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="3d8b6-148">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="3d8b6-149">Remarques :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-149">Notes:</span></span>

* <span data-ttu-id="3d8b6-150">Le runtime recherche les fichiers Razor Pages dans le dossier *Pages* par défaut.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-150">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="3d8b6-151">`Index` est la page par défaut quand une URL n’inclut pas de page.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-151">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="3d8b6-152">Écriture d’un formulaire de base</span><span class="sxs-lookup"><span data-stu-id="3d8b6-152">Writing a basic form</span></span>

<span data-ttu-id="3d8b6-153">Razor Pages est conçu pour que les modèles courants utilisés avec les navigateurs web soient faciles à implémenter lors de la création d’une application.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-153">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="3d8b6-154">La [liaison de modèle](xref:mvc/models/model-binding), les [Tag Helpers](xref:mvc/views/tag-helpers/intro)et les assistances HTML *fonctionnent tous* avec les propriétés définies dans une classe Page Razor.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-154">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="3d8b6-155">Considérez une page qui implémente un formulaire « Nous contacter » de base pour le modèle `Contact` :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-155">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="3d8b6-156">Pour les exemples de ce document, le `DbContext` est initialisé dans le fichier [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-156">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="3d8b6-157">Le modèle de données :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-157">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="3d8b6-158">Le contexte de la base de données :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-158">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="3d8b6-159">Le fichier vue *Pages/Create.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-159">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="3d8b6-160">Le modèle de page *Pages/Create.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-160">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="3d8b6-161">Par convention, la classe `PageModel` se nomme `<PageName>Model` et se trouve dans le même espace de noms que la page.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-161">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="3d8b6-162">La classe `PageModel` permet de séparer la logique d’une page de sa présentation.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-162">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="3d8b6-163">Elle définit des gestionnaires de page pour les demandes envoyées à la page et les données utilisées pour l’afficher.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-163">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="3d8b6-164">Cette séparation permet de gérer les dépendances entre les pages au moyen de [l’injection de dépendances](xref:fundamentals/dependency-injection) et d’effectuer des [tests unitaires](xref:test/razor-pages-tests) sur les pages.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-164">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="3d8b6-165">La page a une *méthode de gestionnaire* `OnPostAsync`, qui s’exécute sur les requêtes `POST` (quand un utilisateur poste le formulaire).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-165">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="3d8b6-166">Vous pouvez ajouter des méthodes de gestionnaire pour n’importe quel verbe HTTP.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-166">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="3d8b6-167">Les gestionnaires les plus courants sont :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-167">The most common handlers are:</span></span>

* <span data-ttu-id="3d8b6-168">`OnGet` pour initialiser l’état nécessaire pour la page.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-168">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="3d8b6-169">Exemple [OnGet](#OnGet).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-169">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="3d8b6-170">`OnPost` pour gérer les envois de formulaire.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-170">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="3d8b6-171">Le suffixe de nommage `Async` est facultatif, mais souvent utilisé par convention pour les fonctions asynchrones.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-171">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="3d8b6-172">Le code `OnPostAsync` dans l’exemple précédent est similaire à celui que vous écririez normalement dans un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-172">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="3d8b6-173">Le code précédent est typique de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-173">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="3d8b6-174">La plupart des primitives MVC comme la [liaison de modèle](xref:mvc/models/model-binding), la [validation](xref:mvc/models/validation) et les résultats d’action sont partagées.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-174">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="3d8b6-175">La méthode `OnPostAsync` précédente :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-175">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="3d8b6-176">Le flux de base de `OnPostAsync` :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-176">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="3d8b6-177">Recherchez les erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-177">Check for validation errors.</span></span>

*  <span data-ttu-id="3d8b6-178">S’il n’y a aucune erreur, enregistrez les données et redirigez.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-178">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="3d8b6-179">S’il y a des erreurs, réaffichez la page avec des messages de validation.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-179">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="3d8b6-180">La validation côté client est identique à celle des applications ASP.NET Core MVC traditionnelles.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-180">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="3d8b6-181">Dans de nombreux cas, les erreurs de validation sont détectées sur le client et jamais envoyées au serveur.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-181">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="3d8b6-182">Quand les données sont entrées correctement, la méthode de gestionnaire `OnPostAsync` appelle la méthode d’assistance `RedirectToPage` pour retourner une instance de `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-182">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="3d8b6-183">`RedirectToPage` est un nouveau résultat d’action, semblable à `RedirectToAction` ou `RedirectToRoute`, mais personnalisé pour les pages.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-183">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="3d8b6-184">Dans l’exemple précédent, il redirige vers la page Index racine (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-184">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="3d8b6-185">`RedirectToPage` est détaillé dans la section [Génération d’URL pour les pages](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-185">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="3d8b6-186">Quand le formulaire envoyé comporte des erreurs de validation (qui sont passées au serveur), la méthode de gestionnaire `OnPostAsync` appelle la méthode d’assistance `Page`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-186">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="3d8b6-187">`Page` retourne une instance de `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-187">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="3d8b6-188">Le retour de `Page` est similaire à la façon dont les actions dans les contrôleurs retournent `View`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-188">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="3d8b6-189">`PageResult` est le type de retour <!-- Review  --> par défaut pour une méthode de gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-189">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="3d8b6-190">Une méthode de gestionnaire qui retourne `void` restitue la page.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-190">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="3d8b6-191">La propriété `Customer` utilise l’attribut `[BindProperty]` pour accepter la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-191">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="3d8b6-192">Par défaut, les pages Razor lient les propriétés uniquement avec les verbes non-GET.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-192">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="3d8b6-193">La liaison aux propriétés peut réduire la quantité de code à écrire.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-193">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="3d8b6-194">Elle réduit la quantité de code en utilisant la même propriété pour afficher les champs de formulaire (`<input asp-for="Customer.Name" />`) et accepter l’entrée.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-194">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

> [!NOTE]
> <span data-ttu-id="3d8b6-195">Pour des raisons de sécurité, vous devez choisir de lier les données de requête GET aux propriétés du modèle de page.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-195">For security reasons, you must opt in to binding GET request data to page model properties.</span></span> <span data-ttu-id="3d8b6-196">Vérifiez l’entrée utilisateur avant de la mapper à des propriétés.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-196">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="3d8b6-197">Le choix de ce comportement est utile pour les scénarios qui reposent sur des valeurs de route ou de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-197">Opting in to this behavior is useful when addressing scenarios which rely on query string or route values.</span></span>
>
> <span data-ttu-id="3d8b6-198">Pour lier une propriété sur des requêtes GET, affectez à la propriété `SupportsGet` de l’attribut `[BindProperty]` la valeur `true` : `[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="3d8b6-198">To bind a property on GET requests, set the `[BindProperty]` attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>

<span data-ttu-id="3d8b6-199">La page d’accueil (*Index.cshtml*) :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-199">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="3d8b6-200">La classe `PageModel` associée (*Index.cshtml.cs*) :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-200">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="3d8b6-201">Le fichier *Index.cshtml* contient le balisage suivant pour créer un lien d’édition pour chaque contact :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-201">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="3d8b6-202">Le [tag helper d’ancre](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) a utilisé l’attribut `asp-route-{value}` pour générer un lien vers la page Edit.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-202">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="3d8b6-203">Le lien contient des données d’itinéraire avec l’ID de contact.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-203">The link contains route data with the contact ID.</span></span> <span data-ttu-id="3d8b6-204">Par exemple, `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-204">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="3d8b6-205">Le fichier *Pages/Edit.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-205">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="3d8b6-206">La première ligne contient la directive `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-206">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="3d8b6-207">La contrainte de routage `"{id:int}"` indique à la page qu’elle doit accepter les requêtes qui contiennent des données d’itinéraire `int`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-207">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="3d8b6-208">Si une requête à la page ne contient de données d’itinéraire qui peuvent être converties en `int`, le runtime retourne une erreur HTTP 404 (introuvable).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-208">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="3d8b6-209">Pour que l’ID soit facultatif, ajoutez `?` à la contrainte de route :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-209">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="3d8b6-210">Le fichier *Pages/Edit.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-210">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="3d8b6-211">Le fichier *Index.cshtml* contient également le balisage pour créer un bouton Supprimer pour chaque contact client :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-211">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="3d8b6-212">Lorsque le bouton Supprimer est rendu en HTML, son `formaction` comprend des paramètres pour :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-212">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="3d8b6-213">L’ID du contact client spécifié par l’attribut `asp-route-id`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-213">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="3d8b6-214">Le `handler` spécifié par l’attribut `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-214">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="3d8b6-215">Voici un exemple de bouton Supprimer rendu avec un ID de contact client de `1`:</span><span class="sxs-lookup"><span data-stu-id="3d8b6-215">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="3d8b6-216">Quand le bouton est sélectionné, une demande `POST` de forumaire est envoyée au serveur.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-216">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="3d8b6-217">Par convention, le nom de la méthode de gestionnaire est sélectionné en fonction de la valeur du paramètre `handler` conformément au schéma `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-217">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="3d8b6-218">Étant donné que le `handler` est `delete` dans cet exemple, la méthode de gestionnaire `OnPostDeleteAsync` est utilisée pour traiter la demande `POST`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-218">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="3d8b6-219">Si `asp-page-handler` est défini avec une autre valeur, telle que `remove`, une méthode de gestionnaire de page avec le nom `OnPostRemoveAsync` est sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-219">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="3d8b6-220">La méthode `OnPostDeleteAsync` :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-220">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="3d8b6-221">Accepte l’`id` de la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-221">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="3d8b6-222">Interroge la base de données pour le contact client avec `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-222">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="3d8b6-223">Si le contact client est trouvé, il est supprimé de la liste des contacts client.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-223">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="3d8b6-224">La base de données est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-224">The database is updated.</span></span>
* <span data-ttu-id="3d8b6-225">Appelle `RedirectToPage` pour rediriger vers la page Index racine (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-225">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="mark-page-properties-as-required"></a><span data-ttu-id="3d8b6-226">Marquer les propriétés de page comme Required</span><span class="sxs-lookup"><span data-stu-id="3d8b6-226">Mark page properties as required</span></span>

<span data-ttu-id="3d8b6-227">Les propriétés définies sur `PageModel` peuvent être décorées avec l’attribut [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-227">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

<span data-ttu-id="3d8b6-228">Pour plus d’informations, consultez [Validation de modèle](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-228">See [Model validation](xref:mvc/models/validation) for more information.</span></span>

## <a name="manage-head-requests-with-the-onget-handler"></a><span data-ttu-id="3d8b6-229">Gérer des demandes HEAD avec le gestionnaire OnGet</span><span class="sxs-lookup"><span data-stu-id="3d8b6-229">Manage HEAD requests with the OnGet handler</span></span>

<span data-ttu-id="3d8b6-230">Les requêtes HEAD vous permettent de récupérer les en-têtes pour une ressource spécifique.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-230">HEAD requests allow you to retrieve the headers for a specific resource.</span></span> <span data-ttu-id="3d8b6-231">Contrairement aux requêtes GET, les requêtes HEAD ne renvoient un corps de réponse.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-231">Unlike GET requests, HEAD requests don't return a response body.</span></span> 

<span data-ttu-id="3d8b6-232">En règle générale, un gestionnaire HEAD est créé et appelé pour des demandes HEAD :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-232">Ordinarily, a HEAD handler is created and called for HEAD requests:</span></span> 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="3d8b6-233">si aucun gestionnaire HEAD (`OnHead`) n’est défini, les pages Razor reviennent à l’appel du gestionnaire de page GET (`OnGet`) dans ASP.NET Core 2.1 ou une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-233">If no HEAD handler (`OnHead`) is defined, Razor Pages falls back to calling the GET page handler (`OnGet`) in ASP.NET Core 2.1 or later.</span></span> <span data-ttu-id="3d8b6-234">Dans ASP.NET Core 2.1 et 2.2, ce comportement se produit avec la version [SetCompatibilityVersion](xref:mvc/compatibility-version) dans `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-234">In ASP.NET Core 2.1 and 2.2, this behavior occurs with the [SetCompatibilityVersion](xref:mvc/compatibility-version) in `Startup.Configure`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="3d8b6-235">Les modèles par défaut génèrent l'appel `SetCompatibilityVersion` dans ASP.NET Core 2.1 et 2.2.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-235">The default templates generate the `SetCompatibilityVersion` call in ASP.NET Core 2.1 and 2.2.</span></span>

<span data-ttu-id="3d8b6-236">`SetCompatibilityVersion` définit efficacement l’option Pages Razor `AllowMappingHeadRequestsToGetHandler` sur `true`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-236">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="3d8b6-237">Au lieu d’adhérer à tous les comportements de la version 2.1 avec `SetCompatibilityVersion`, vous pouvez explicitement adhérer à des comportements spécifiques.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-237">Rather than opting into all 2.1 behaviors with `SetCompatibilityVersion`, you can explicitly opt-in to specific behaviors.</span></span> <span data-ttu-id="3d8b6-238">Le code suivant adhère aux demandes HEAD de mappage avec le gestionnaire GET.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-238">The following code opts into the mapping HEAD requests to the GET handler.</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

::: moniker-end

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="3d8b6-239">XSRF/CSRF et Razor Pages</span><span class="sxs-lookup"><span data-stu-id="3d8b6-239">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="3d8b6-240">Vous n’avez aucun code à écrire pour la [validation anti-contrefaçon](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-240">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="3d8b6-241">La validation et la génération de jetons anti-contrefaçon sont automatiquement incluses dans Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-241">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="3d8b6-242">Utilisation de dispositions, partiels, modèles et Tag Helpers avec Razor Pages</span><span class="sxs-lookup"><span data-stu-id="3d8b6-242">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="3d8b6-243">Les pages Razor fonctionnent avec toutes les fonctionnalités du moteur de vue Razor.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-243">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="3d8b6-244">Les dispositions, partiels, modèles, Tag Helpers, *_ViewStart.cshtml* et *_ViewImports.cshtml* fonctionnent de la même façon que pour les vues Razor classiques.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-244">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="3d8b6-245">Nous allons nettoyer un peu cette page en tirant parti de certaines de ces fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-245">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="3d8b6-246">Ajoutez une [page de disposition](xref:mvc/views/layout) à *Pages/Shared/_Layout.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-246">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="3d8b6-247">Ajoutez une [page de disposition](xref:mvc/views/layout) à *Pages/_Layout.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-247">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

::: moniker-end

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="3d8b6-248">La [disposition](xref:mvc/views/layout) :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-248">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="3d8b6-249">Contrôle la disposition de chaque page (à moins que la page ne refuse la disposition).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-249">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="3d8b6-250">Importe des structures HTML telles que JavaScript et les feuilles de style.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-250">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="3d8b6-251">Pour plus d’informations, consultez [Page de disposition](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-251">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="3d8b6-252">La propriété [Layout](xref:mvc/views/layout#specifying-a-layout) est définie dans *Pages/_ViewStart.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-252">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="3d8b6-253">La disposition est dans le dossier *Pages/Shared*.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-253">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="3d8b6-254">Les pages recherchent d’autres vues (dispositions, modèles, partiels) hiérarchiquement, en commençant dans le même dossier que la page active.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-254">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="3d8b6-255">Une disposition dans le dossier *Pages/Shared* peut être utilisée à partir de n’importe quelle page Razor sous le dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-255">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="3d8b6-256">Le fichier de disposition doit être placé dans le dossier *Pages/Shared*.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-256">The layout file should go in the *Pages/Shared* folder.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="3d8b6-257">La disposition est dans le dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-257">The layout is in the *Pages* folder.</span></span> <span data-ttu-id="3d8b6-258">Les pages recherchent d’autres vues (dispositions, modèles, partiels) hiérarchiquement, en commençant dans le même dossier que la page active.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-258">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="3d8b6-259">Une disposition dans le dossier *Pages* peut être utilisée à partir de n’importe quelle page Razor sous le dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-259">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

::: moniker-end

<span data-ttu-id="3d8b6-260">Nous vous recommandons de **ne pas** placer le fichier de disposition dans le dossier *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-260">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="3d8b6-261">*Views/Shared* est un modèle de vues MVC.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-261">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="3d8b6-262">Les pages Razor sont censées s’appuyer sur la hiérarchie des dossiers, pas sur les conventions de chemins.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-262">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="3d8b6-263">La recherche de vue à partir d’une page Razor inclut le dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-263">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="3d8b6-264">Les dispositions, modèles et partiels que vous utilisez avec les contrôleurs MVC et les vues Razor conventionnelles *fonctionnent simplement*.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-264">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="3d8b6-265">Ajoutez un fichier *Pages/_ViewImports.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-265">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="3d8b6-266">`@namespace` est expliqué plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-266">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="3d8b6-267">La directive `@addTagHelper` permet de bénéficier des [Tag Helpers intégrés](xref:mvc/views/tag-helpers/builtin-th/Index) dans toutes les pages du dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-267">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="3d8b6-268">Quand la directive `@namespace` est utilisée explicitement sur une page :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-268">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="3d8b6-269">La directive définit l’espace de noms pour la page.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-269">The directive sets the namespace for the page.</span></span> <span data-ttu-id="3d8b6-270">La directive `@model` n’a pas besoin d’inclure l’espace de noms.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-270">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="3d8b6-271">Quand la directive `@namespace` est contenue dans *_ViewImports.cshtml*, l’espace de noms spécifié fournit le préfixe de l’espace de noms généré dans la Page qui importe la directive `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-271">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="3d8b6-272">Le reste de l’espace de noms généré (la partie suffixe) est le chemin relatif séparé par un point entre le dossier contenant *_ViewImports.cshtml* et le dossier contenant la page.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-272">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="3d8b6-273">Par exemple, la classe `PageModel` *Pages/Customers/Edit.cshtml.cs* définit explicitement l’espace de noms :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-273">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="3d8b6-274">Le fichier *Pages/_ViewImports.cshtml* définit l’espace de noms suivant :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-274">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="3d8b6-275">L’espace de noms généré pour la page Razor *Pages/Customers/Edit.cshtml* est identique à la classe `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-275">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="3d8b6-276">`@namespace` *fonctionne également avec les vues Razor classiques*.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-276">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="3d8b6-277">Le fichier de vue *Pages/Create.cshtml* d’origine :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-277">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="3d8b6-278">Le fichier vue *Pages/Create.cshtml* mis à jour :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-278">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="3d8b6-279">Le [projet de démarrage de pages Razor](#rpvs17) contient *Pages/_ValidationScriptsPartial.cshtml*, qui connecte la validation côté client.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-279">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="3d8b6-280">Pour plus d'informations sur les affichages partiels, consultez <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-280">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="3d8b6-281">Génération d’URL pour les pages</span><span class="sxs-lookup"><span data-stu-id="3d8b6-281">URL generation for Pages</span></span>

<span data-ttu-id="3d8b6-282">La page `Create`, illustrée précédemment, utilise `RedirectToPage` :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-282">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="3d8b6-283">L’application a la structure de fichiers/dossiers suivante :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-283">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="3d8b6-284">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="3d8b6-284">*/Pages*</span></span>

  * <span data-ttu-id="3d8b6-285">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3d8b6-285">*Index.cshtml*</span></span>
  * <span data-ttu-id="3d8b6-286">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="3d8b6-286">*/Customers*</span></span>

    * <span data-ttu-id="3d8b6-287">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3d8b6-287">*Create.cshtml*</span></span>
    * <span data-ttu-id="3d8b6-288">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3d8b6-288">*Edit.cshtml*</span></span>
    * <span data-ttu-id="3d8b6-289">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3d8b6-289">*Index.cshtml*</span></span>

<span data-ttu-id="3d8b6-290">Une fois l’opération réussie, les pages *Pages/Customers/Create.cshtml* et *Pages/Customers/Edit.cshtml* redirigent vers *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-290">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="3d8b6-291">La chaîne `/Index` fait partie de l’URI pour accéder à la page précédente.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-291">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="3d8b6-292">La chaîne `/Index` peut être utilisée pour générer l’URI de la page *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-292">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="3d8b6-293">Exemple :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-293">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="3d8b6-294">Le nom de la page est le chemin de la page à partir du dossier racine */Pages* avec un `/` devant (par exemple, `/Index`).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-294">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="3d8b6-295">Les exemples de génération d’URL précédents offrent des options améliorées et des capacités fonctionnelles sur le codage en dur d’une URL.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-295">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="3d8b6-296">La génération d’URL utilise le [routage](xref:mvc/controllers/routing) et peut générer et encoder des paramètres en fonction de la façon dont l’itinéraire est défini dans le chemin de destination.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-296">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="3d8b6-297">La génération d’URL pour les pages prend en charge les noms relatifs.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-297">URL generation for pages supports relative names.</span></span> <span data-ttu-id="3d8b6-298">Le tableau suivant montre la page Index sélectionnée avec différents paramètres `RedirectToPage` à partir de *Pages/Customers/Create.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-298">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="3d8b6-299">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="3d8b6-299">RedirectToPage(x)</span></span>| <span data-ttu-id="3d8b6-300">Page</span><span class="sxs-lookup"><span data-stu-id="3d8b6-300">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="3d8b6-301">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="3d8b6-301">RedirectToPage("/Index")</span></span> | <span data-ttu-id="3d8b6-302">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="3d8b6-302">*Pages/Index*</span></span> |
| <span data-ttu-id="3d8b6-303">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="3d8b6-303">RedirectToPage("./Index");</span></span> | <span data-ttu-id="3d8b6-304">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="3d8b6-304">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="3d8b6-305">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="3d8b6-305">RedirectToPage("../Index")</span></span> | <span data-ttu-id="3d8b6-306">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="3d8b6-306">*Pages/Index*</span></span> |
| <span data-ttu-id="3d8b6-307">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="3d8b6-307">RedirectToPage("Index")</span></span>  | <span data-ttu-id="3d8b6-308">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="3d8b6-308">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="3d8b6-309">`RedirectToPage("Index")`, `RedirectToPage("./Index")` et `RedirectToPage("../Index")` sont des <em>noms relatifs</em>.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-309">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are <em>relative names</em>.</span></span> <span data-ttu-id="3d8b6-310">Le paramètre `RedirectToPage` est <em>combiné</em> avec le chemin de la page active pour calculer le nom de la page de destination.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-310">The `RedirectToPage` parameter is <em>combined</em> with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="3d8b6-311">La liaison de nom relatif est utile lors de la création de sites avec une structure complexe.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-311">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="3d8b6-312">Si vous utilisez des noms relatifs pour établir une liaison entre les pages d’un dossier, vous pouvez renommer ce dossier.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-312">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="3d8b6-313">Tous les liens fonctionneront encore (car ils n’incluent pas le nom du dossier).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-313">All the links still work (because they didn't include the folder name).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="viewdata-attribute"></a><span data-ttu-id="3d8b6-314">Attribut ViewData</span><span class="sxs-lookup"><span data-stu-id="3d8b6-314">ViewData attribute</span></span>

<span data-ttu-id="3d8b6-315">Les données peuvent être passées à une page avec [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-315">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="3d8b6-316">Les valeurs des propriétés définies sur des contrôleurs ou sur des modèles de page Razor décorés avec `[ViewData]` sont stockées et chargées à partir de [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-316">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="3d8b6-317">Dans l’exemple suivant, `AboutModel` contient une propriété `Title` décorée avec `[ViewData]`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-317">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="3d8b6-318">La propriété `Title` a pour valeur le titre de la page À propos de :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-318">The `Title` property is set to the title of the About page:</span></span>

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

<span data-ttu-id="3d8b6-319">Dans la page À propos de, accédez à la propriété `Title` en tant que propriété de modèle :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-319">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="3d8b6-320">Dans la disposition, le titre est lu à partir du dictionnaire ViewData :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-320">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="3d8b6-321">TempData</span><span class="sxs-lookup"><span data-stu-id="3d8b6-321">TempData</span></span>

<span data-ttu-id="3d8b6-322">ASP.NET Core expose la propriété [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) sur un [contrôleur](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-322">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="3d8b6-323">Cette propriété stocke les données jusqu’à ce qu’elles soient lues.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-323">This property stores data until it's read.</span></span> <span data-ttu-id="3d8b6-324">Vous pouvez utiliser les méthodes `Keep` et `Peek` pour examiner les données sans suppression.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-324">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="3d8b6-325">`TempData` est utile pour la redirection, quand des données sont nécessaires pour plusieurs requêtes.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-325">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="3d8b6-326">L’attribut `[TempData]` est une nouveauté dans ASP.NET Core 2.0. Il est pris en charge sur les contrôleurs et les pages.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-326">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="3d8b6-327">Le code suivant définit la valeur de `Message` à l’aide de `TempData` :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-327">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="3d8b6-328">Le balisage suivant dans le fichier *Pages/Customers/Index.cshtml* affiche la valeur de `Message` à l’aide de `TempData`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-328">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="3d8b6-329">Le modèle de page *Pages/Customers/Index.cshtml.cs* applique l’attribut `[TempData]` à la propriété `Message`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-329">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="3d8b6-330">Pour plus d’informations, consultez [TempData](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-330">See [TempData](xref:fundamentals/app-state#tempdata) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="3d8b6-331">Plusieurs gestionnaires par page</span><span class="sxs-lookup"><span data-stu-id="3d8b6-331">Multiple handlers per page</span></span>

<span data-ttu-id="3d8b6-332">La page suivante génère un balisage pour deux gestionnaires de page à l’aide du Tag Helper `asp-page-handler` :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-332">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="3d8b6-333">Le formulaire dans l’exemple précédent contient deux boutons d’envoi, chacun utilisant `FormActionTagHelper` pour envoyer à une URL différente.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-333">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="3d8b6-334">L’attribut `asp-page-handler` est un complément de `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-334">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="3d8b6-335">`asp-page-handler` génère des URL qui envoient à chacune des méthodes de gestionnaire définies par une page.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-335">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="3d8b6-336">`asp-page` n’est pas spécifié, car l’exemple établit une liaison à la page active.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-336">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="3d8b6-337">Le modèle de page :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-337">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="3d8b6-338">Le code précédent utilise des *méthodes de gestionnaire nommées*.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-338">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="3d8b6-339">Pour créer des méthodes de gestionnaire nommées, il faut prendre le texte dans le nom après `On<HTTP Verb>` et avant `Async` (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-339">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="3d8b6-340">Dans l’exemple précédent, les méthodes de page sont OnPost**JoinList**Async et OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-340">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="3d8b6-341">Avec *OnPost* et *Async* supprimés, les noms des gestionnaires sont `JoinList` et `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-341">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="3d8b6-342">Avec le code précédent, le chemin d’URL qui envoie à `OnPostJoinListAsync` est `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-342">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="3d8b6-343">Le chemin d’URL qui envoie à `OnPostJoinListUCAsync` est `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-343">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="3d8b6-344">Routes personnalisées</span><span class="sxs-lookup"><span data-stu-id="3d8b6-344">Custom routes</span></span>

<span data-ttu-id="3d8b6-345">Utilisez la directive `@page` pour :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-345">Use the `@page` directive to:</span></span>

* <span data-ttu-id="3d8b6-346">Spécifier une route personnalisée vers une page.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-346">Specify a custom route to a page.</span></span> <span data-ttu-id="3d8b6-347">Par exemple, la route vers la page À propos peut être définie sur `/Some/Other/Path` avec `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-347">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="3d8b6-348">Ajouter des segments à la route par défaut d’une page.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-348">Append segments to a page's default route.</span></span> <span data-ttu-id="3d8b6-349">Par exemple, un segment « item » peut être ajouté à la route par défaut d’une page avec `@page "item"`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-349">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="3d8b6-350">Ajouter des paramètres à la route par défaut d’une page.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-350">Append parameters to a page's default route.</span></span> <span data-ttu-id="3d8b6-351">Par exemple, un paramètre d’ID, `id`, peut être nécessaire pour une page avec `@page "{id}"`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-351">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="3d8b6-352">Un chemin relatif racine désigné par un tilde (`~`) au début du chemin est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-352">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="3d8b6-353">Par exemple, `@page "~/Some/Other/Path"` est identique à `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-353">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="3d8b6-354">Vous pouvez remplacer la chaîne de requête `?handler=JoinList` dans l’URL par un segment de routage `/JoinList` en spécifiant le modèle de routage `@page "{handler?}"`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-354">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="3d8b6-355">Si vous ne voulez pas avoir la chaîne de requête `?handler=JoinList` dans l’URL, vous pouvez changer l’itinéraire pour placer le nom du gestionnaire dans la partie chemin de l’URL.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-355">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="3d8b6-356">Vous pouvez personnaliser l’itinéraire en ajoutant un modèle d’itinéraire placé entre des guillemets doubles après la directive `@page`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-356">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="3d8b6-357">Avec le code précédent, le chemin d’URL qui envoie à `OnPostJoinListAsync` est `http://localhost:5000/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-357">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="3d8b6-358">Le chemin d’URL qui envoie à `OnPostJoinListUCAsync` est `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-358">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="3d8b6-359">Le `?` suivant `handler` signifie que le paramètre d’itinéraire est facultatif.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-359">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="3d8b6-360">Configuration et paramètres</span><span class="sxs-lookup"><span data-stu-id="3d8b6-360">Configuration and settings</span></span>

<span data-ttu-id="3d8b6-361">Pour configurer les options avancées, utilisez la méthode d’extension `AddRazorPagesOptions` sur le générateur MVC :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-361">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="3d8b6-362">Actuellement, vous pouvez utiliser `RazorPagesOptions` pour définir le répertoire racine pour les pages, ou ajouter des conventions de modèle d’application pour les pages.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-362">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="3d8b6-363">Nous permettrons à l’avenir une plus grande extensibilité en ce sens.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-363">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="3d8b6-364">Pour précompiler des vues, consultez [Compilation de vue Razor](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-364">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="3d8b6-365">[Téléchargez ou affichez des exemples de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="3d8b6-365">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="3d8b6-366">Consultez [Bien démarrer avec Razor Pages](xref:tutorials/razor-pages/razor-pages-start) qui s’appuie sur cette introduction.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-366">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="3d8b6-367">Spécifier que les pages Razor se trouvent à la racine du contenu</span><span class="sxs-lookup"><span data-stu-id="3d8b6-367">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="3d8b6-368">Par défaut, les pages Razor sont associées à la racine */Pages*.</span><span class="sxs-lookup"><span data-stu-id="3d8b6-368">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="3d8b6-369">Ajoutez [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) à [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) pour spécifier que vos pages Razor se trouvent à la racine de contenu ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) de l’application :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-369">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="3d8b6-370">Spécifier que les pages Razor se trouvent dans un répertoire racine personnalisé</span><span class="sxs-lookup"><span data-stu-id="3d8b6-370">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="3d8b6-371">Ajoutez [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) à [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) pour spécifier que vos pages Razor se trouvent dans le répertoire racine personnalisé de l’application (fournissez un chemin relatif) :</span><span class="sxs-lookup"><span data-stu-id="3d8b6-371">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="additional-resources"></a><span data-ttu-id="3d8b6-372">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="3d8b6-372">Additional resources</span></span>

* <xref:index>
* <xref:mvc/views/razor>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>
