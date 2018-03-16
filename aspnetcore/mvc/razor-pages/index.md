---
title: "Présentation des pages Razor dans ASP.NET Core"
author: Rick-Anderson
description: "Découvrez comment les pages Razor dans ASP.NET Core permettent de développer des scénarios orientés page de façon plus simple et plus productive qu’avec MVC."
manager: wpickett
ms.author: riande
ms.date: 09/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: mvc/razor-pages/index
ms.openlocfilehash: cb80c38fd0284d5153aebfe7bb515722623a4a34
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="2dc49-103">Présentation des pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2dc49-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="2dc49-104">De [Rick Anderson](https://twitter.com/RickAndMSFT) et [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="2dc49-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="2dc49-105">Les pages Razor sont une nouvelle fonctionnalité d’ASP.NET Core MVC qui permet de développer des scénarios orientés page de façon plus simple et plus productive.</span><span class="sxs-lookup"><span data-stu-id="2dc49-105">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="2dc49-106">Si vous cherchez un didacticiel qui utilise l’approche Model-View-Controller, consultez [Bien démarrer avec ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="2dc49-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="2dc49-107">Ce document fournit une introduction aux pages Razor.</span><span class="sxs-lookup"><span data-stu-id="2dc49-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="2dc49-108">Il ne s’agit pas d’un didacticiel pas à pas.</span><span class="sxs-lookup"><span data-stu-id="2dc49-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="2dc49-109">Si certaines sections vous semblent trop techniques, consultez [Bien démarrer avec les pages Razor](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="2dc49-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="2dc49-110">Pour une vue d’ensemble d’ASP.NET Core, consultez [Introduction à ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="2dc49-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="2dc49-111">Prérequis d’ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="2dc49-111">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="2dc49-112">Installez [.NET Core](https://www.microsoft.com/net/core) 2.0.0 ou ultérieur.</span><span class="sxs-lookup"><span data-stu-id="2dc49-112">Install [.NET Core](https://www.microsoft.com/net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="2dc49-113">Si vous utilisez Visual Studio, installez [Visual Studio](https://www.visualstudio.com/vs/) 2017 version 15.3 ou ultérieure avec les charges de travail suivantes :</span><span class="sxs-lookup"><span data-stu-id="2dc49-113">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 2017 version 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="2dc49-114">**Développement web et ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="2dc49-114">**ASP.NET and web development**</span></span>
* <span data-ttu-id="2dc49-115">**Développement multiplateforme .NET Core**</span><span class="sxs-lookup"><span data-stu-id="2dc49-115">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="2dc49-116">Création d’un projet de pages Razor</span><span class="sxs-lookup"><span data-stu-id="2dc49-116">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2dc49-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2dc49-117">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="2dc49-118">Pour obtenir des instructions détaillées sur la création d’un projet de pages Razor avec Visual Studio, consultez [Bien démarrer avec les pages Razor](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="2dc49-118">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2dc49-119">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="2dc49-119">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="2dc49-120">Exécutez `dotnet new razor` à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="2dc49-120">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="2dc49-121">Ouvrez le fichier *.csproj* généré à partir de Visual Studio pour Mac.</span><span class="sxs-lookup"><span data-stu-id="2dc49-121">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2dc49-122">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2dc49-122">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="2dc49-123">Exécutez `dotnet new razor` à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="2dc49-123">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2dc49-124">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="2dc49-124">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="2dc49-125">Exécutez `dotnet new razor` à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="2dc49-125">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="2dc49-126">Pages Razor</span><span class="sxs-lookup"><span data-stu-id="2dc49-126">Razor Pages</span></span>

<span data-ttu-id="2dc49-127">La fonctionnalité Pages Razor est activée dans *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="2dc49-127">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="2dc49-128">Considérez une page de base : <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="2dc49-128">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="2dc49-129">Le code précédent ressemble beaucoup à un fichier vue Razor.</span><span class="sxs-lookup"><span data-stu-id="2dc49-129">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="2dc49-130">Ce qui le rend différent est la directive `@page`.</span><span class="sxs-lookup"><span data-stu-id="2dc49-130">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="2dc49-131">`@page` fait du fichier une action MVC, ce qui signifie qu’il gère les demandes directement, sans passer par un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="2dc49-131">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="2dc49-132">`@page` doit être la première directive Razor sur une page.</span><span class="sxs-lookup"><span data-stu-id="2dc49-132">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="2dc49-133">`@page` affecte le comportement d’autres constructions Razor.</span><span class="sxs-lookup"><span data-stu-id="2dc49-133">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="2dc49-134">Une page similaire, utilisant une classe `PageModel`, est illustrée dans les deux fichiers suivants.</span><span class="sxs-lookup"><span data-stu-id="2dc49-134">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="2dc49-135">Le fichier *Pages/Index2.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2dc49-135">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="2dc49-136">Le modèle de page *Pages/Index2.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="2dc49-136">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="2dc49-137">Par convention, le fichier de classe `PageModel` a le même nom que le fichier Page Razor, avec *.cs* en plus.</span><span class="sxs-lookup"><span data-stu-id="2dc49-137">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="2dc49-138">Par exemple, la page Razor précédente est *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2dc49-138">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="2dc49-139">Le fichier contenant la classe `PageModel` se nomme *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="2dc49-139">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="2dc49-140">Les associations des chemins d’URL aux pages sont déterminées par l’emplacement de la page dans le système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="2dc49-140">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="2dc49-141">Le tableau suivant montre un chemin Page Razor et l’URL correspondante :</span><span class="sxs-lookup"><span data-stu-id="2dc49-141">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="2dc49-142">Nom et chemin de fichier</span><span class="sxs-lookup"><span data-stu-id="2dc49-142">File name and path</span></span>               | <span data-ttu-id="2dc49-143">URL correspondante</span><span class="sxs-lookup"><span data-stu-id="2dc49-143">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="2dc49-144">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2dc49-144">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="2dc49-145">`/` ou `/Index`</span><span class="sxs-lookup"><span data-stu-id="2dc49-145">`/` or `/Index`</span></span> |
| <span data-ttu-id="2dc49-146">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2dc49-146">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="2dc49-147">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2dc49-147">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="2dc49-148">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2dc49-148">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="2dc49-149">`/Store` ou `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="2dc49-149">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="2dc49-150">Remarques :</span><span class="sxs-lookup"><span data-stu-id="2dc49-150">Notes:</span></span>

* <span data-ttu-id="2dc49-151">Le runtime recherche les fichiers Pages Razor dans le dossier *Pages* par défaut.</span><span class="sxs-lookup"><span data-stu-id="2dc49-151">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="2dc49-152">`Index` est la page par défaut quand une URL n’inclut pas de page.</span><span class="sxs-lookup"><span data-stu-id="2dc49-152">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="2dc49-153">Écriture d’un formulaire de base</span><span class="sxs-lookup"><span data-stu-id="2dc49-153">Writing a basic form</span></span>

<span data-ttu-id="2dc49-154">Les fonctionnalités des pages Razor sont conçues pour simplifier les modèles courants utilisés avec les navigateurs web.</span><span class="sxs-lookup"><span data-stu-id="2dc49-154">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="2dc49-155">La [liaison de modèle](xref:mvc/models/model-binding), les [Tag Helpers](xref:mvc/views/tag-helpers/intro)et les assistances HTML *fonctionnent tous* avec les propriétés définies dans une classe Page Razor.</span><span class="sxs-lookup"><span data-stu-id="2dc49-155">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="2dc49-156">Considérez une page qui implémente un formulaire « Nous contacter » de base pour le modèle `Contact` :</span><span class="sxs-lookup"><span data-stu-id="2dc49-156">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="2dc49-157">Pour les exemples de ce document, le `DbContext` est initialisé dans le fichier [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).</span><span class="sxs-lookup"><span data-stu-id="2dc49-157">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="2dc49-158">Le modèle de données :</span><span class="sxs-lookup"><span data-stu-id="2dc49-158">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="2dc49-159">Le contexte de la base de données :</span><span class="sxs-lookup"><span data-stu-id="2dc49-159">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="2dc49-160">Le fichier vue *Pages/Create.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2dc49-160">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="2dc49-161">Le modèle de page *Pages/Create.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="2dc49-161">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="2dc49-162">Par convention, la classe `PageModel` se nomme `<PageName>Model` et se trouve dans le même espace de noms que la page.</span><span class="sxs-lookup"><span data-stu-id="2dc49-162">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="2dc49-163">La classe `PageModel` permet de séparer la logique d’une page de sa présentation.</span><span class="sxs-lookup"><span data-stu-id="2dc49-163">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="2dc49-164">Elle définit des gestionnaires de page pour les demandes envoyées à la page et les données utilisées pour l’afficher.</span><span class="sxs-lookup"><span data-stu-id="2dc49-164">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="2dc49-165">Cette séparation permet de gérer les dépendances entre les pages au moyen de [l’injection de dépendances](xref:fundamentals/dependency-injection) et d’effectuer des [tests unitaires](xref:testing/razor-pages-testing) sur les pages.</span><span class="sxs-lookup"><span data-stu-id="2dc49-165">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:testing/razor-pages-testing) the pages.</span></span>

<span data-ttu-id="2dc49-166">La page a une *méthode de gestionnaire* `OnPostAsync`, qui s’exécute sur les requêtes `POST` (quand un utilisateur poste le formulaire).</span><span class="sxs-lookup"><span data-stu-id="2dc49-166">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="2dc49-167">Vous pouvez ajouter des méthodes de gestionnaire pour n’importe quel verbe HTTP.</span><span class="sxs-lookup"><span data-stu-id="2dc49-167">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="2dc49-168">Les gestionnaires les plus courants sont :</span><span class="sxs-lookup"><span data-stu-id="2dc49-168">The most common handlers are:</span></span>

* <span data-ttu-id="2dc49-169">`OnGet` pour initialiser l’état nécessaire pour la page.</span><span class="sxs-lookup"><span data-stu-id="2dc49-169">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="2dc49-170">Exemple [OnGet](#OnGet).</span><span class="sxs-lookup"><span data-stu-id="2dc49-170">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="2dc49-171">`OnPost` pour gérer les envois de formulaire.</span><span class="sxs-lookup"><span data-stu-id="2dc49-171">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="2dc49-172">Le suffixe de nommage `Async` est facultatif, mais souvent utilisé par convention pour les fonctions asynchrones.</span><span class="sxs-lookup"><span data-stu-id="2dc49-172">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="2dc49-173">Le code `OnPostAsync` dans l’exemple précédent est similaire à celui que vous écririez normalement dans un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="2dc49-173">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="2dc49-174">Le code précédent est typique des pages Razor.</span><span class="sxs-lookup"><span data-stu-id="2dc49-174">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="2dc49-175">La plupart des primitives MVC comme la [liaison de modèle](xref:mvc/models/model-binding), la [validation](xref:mvc/models/validation) et les résultats d’action sont partagées.</span><span class="sxs-lookup"><span data-stu-id="2dc49-175">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="2dc49-176">La méthode `OnPostAsync` précédente :</span><span class="sxs-lookup"><span data-stu-id="2dc49-176">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="2dc49-177">Le flux de base de `OnPostAsync` :</span><span class="sxs-lookup"><span data-stu-id="2dc49-177">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="2dc49-178">Recherchez les erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="2dc49-178">Check for validation errors.</span></span>

*  <span data-ttu-id="2dc49-179">S’il n’y a aucune erreur, enregistrez les données et redirigez.</span><span class="sxs-lookup"><span data-stu-id="2dc49-179">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="2dc49-180">S’il y a des erreurs, réaffichez la page avec des messages de validation.</span><span class="sxs-lookup"><span data-stu-id="2dc49-180">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="2dc49-181">La validation côté client est identique à celle des applications ASP.NET Core MVC traditionnelles.</span><span class="sxs-lookup"><span data-stu-id="2dc49-181">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="2dc49-182">Dans de nombreux cas, les erreurs de validation sont détectées sur le client et jamais envoyées au serveur.</span><span class="sxs-lookup"><span data-stu-id="2dc49-182">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="2dc49-183">Quand les données sont entrées correctement, la méthode de gestionnaire `OnPostAsync` appelle la méthode d’assistance `RedirectToPage` pour retourner une instance de `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="2dc49-183">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="2dc49-184">`RedirectToPage` est un nouveau résultat d’action, semblable à `RedirectToAction` ou `RedirectToRoute`, mais personnalisé pour les pages.</span><span class="sxs-lookup"><span data-stu-id="2dc49-184">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="2dc49-185">Dans l’exemple précédent, il redirige vers la page Index racine (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="2dc49-185">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="2dc49-186">`RedirectToPage` est détaillé dans la section [Génération d’URL pour les pages](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="2dc49-186">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="2dc49-187">Quand le formulaire envoyé comporte des erreurs de validation (qui sont passées au serveur), la méthode de gestionnaire `OnPostAsync` appelle la méthode d’assistance `Page`.</span><span class="sxs-lookup"><span data-stu-id="2dc49-187">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="2dc49-188">`Page` retourne une instance de `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="2dc49-188">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="2dc49-189">Le retour de `Page` est similaire à la façon dont les actions dans les contrôleurs retournent `View`.</span><span class="sxs-lookup"><span data-stu-id="2dc49-189">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="2dc49-190">`PageResult` est le type de retour <!-- Review  --> par défaut pour une méthode de gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="2dc49-190">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="2dc49-191">Une méthode de gestionnaire qui retourne `void` restitue la page.</span><span class="sxs-lookup"><span data-stu-id="2dc49-191">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="2dc49-192">La propriété `Customer` utilise l’attribut `[BindProperty]` pour accepter la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="2dc49-192">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="2dc49-193">Par défaut, les pages Razor lient les propriétés uniquement avec les verbes non-GET.</span><span class="sxs-lookup"><span data-stu-id="2dc49-193">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="2dc49-194">La liaison aux propriétés peut réduire la quantité de code à écrire.</span><span class="sxs-lookup"><span data-stu-id="2dc49-194">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="2dc49-195">Elle réduit la quantité de code en utilisant la même propriété pour afficher les champs de formulaire (`<input asp-for="Customer.Name" />`) et accepter l’entrée.</span><span class="sxs-lookup"><span data-stu-id="2dc49-195">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

> [!NOTE]
> <span data-ttu-id="2dc49-196">Pour des raisons de sécurité, vous devez choisir de lier les données de requête GET aux propriétés du modèle de page.</span><span class="sxs-lookup"><span data-stu-id="2dc49-196">For security reasons, you must opt in to binding GET request data to page model properties.</span></span> <span data-ttu-id="2dc49-197">Vérifiez l’entrée utilisateur avant de la mapper à des propriétés.</span><span class="sxs-lookup"><span data-stu-id="2dc49-197">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="2dc49-198">Le choix de ce comportement est utile lors de la création de fonctionnalités qui reposent sur des valeurs de route ou de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="2dc49-198">Opting in to this behavior is useful when building features which rely on query string or route values.</span></span>
>
> <span data-ttu-id="2dc49-199">Pour lier une propriété sur des requêtes GET, affectez à la propriété `SupportsGet` de l’attribut `[BindProperty]` la valeur `true` : `[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="2dc49-199">To bind a property on GET requests, set the `[BindProperty]` attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>

<span data-ttu-id="2dc49-200">La page d’accueil (*Index.cshtml*) :</span><span class="sxs-lookup"><span data-stu-id="2dc49-200">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="2dc49-201">Le fichier code-behind *Index.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="2dc49-201">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="2dc49-202">Le fichier *Index.cshtml* contient le balisage suivant pour créer un lien d’édition pour chaque contact :</span><span class="sxs-lookup"><span data-stu-id="2dc49-202">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="2dc49-203">Le [tag helper d’ancre](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) a utilisé l’attribut `asp-route-{value}` pour générer un lien vers la page Edit.</span><span class="sxs-lookup"><span data-stu-id="2dc49-203">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="2dc49-204">Le lien contient des données d’itinéraire avec l’ID de contact.</span><span class="sxs-lookup"><span data-stu-id="2dc49-204">The link contains route data with the contact ID.</span></span> <span data-ttu-id="2dc49-205">Par exemple, `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="2dc49-205">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="2dc49-206">Le fichier *Pages/Edit.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2dc49-206">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="2dc49-207">La première ligne contient la directive `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="2dc49-207">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="2dc49-208">La contrainte de routage `"{id:int}"` indique à la page qu’elle doit accepter les requêtes qui contiennent des données d’itinéraire `int`.</span><span class="sxs-lookup"><span data-stu-id="2dc49-208">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="2dc49-209">Si une requête à la page ne contient de données d’itinéraire qui peuvent être converties en `int`, le runtime retourne une erreur HTTP 404 (introuvable).</span><span class="sxs-lookup"><span data-stu-id="2dc49-209">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="2dc49-210">Le fichier *Pages/Edit.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="2dc49-210">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="2dc49-211">Le fichier *Index.cshtml* contient également le balisage pour créer un bouton Supprimer pour chaque contact client :</span><span class="sxs-lookup"><span data-stu-id="2dc49-211">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="2dc49-212">Lorsque le bouton Supprimer est rendu en HTML, son `formaction` comprend des paramètres pour :</span><span class="sxs-lookup"><span data-stu-id="2dc49-212">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="2dc49-213">L’ID du contact client spécifié par l’attribut `asp-route-id`.</span><span class="sxs-lookup"><span data-stu-id="2dc49-213">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="2dc49-214">Le `handler` spécifié par l’attribut `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="2dc49-214">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="2dc49-215">Voici un exemple de bouton Supprimer rendu avec un ID de contact client de `1`:</span><span class="sxs-lookup"><span data-stu-id="2dc49-215">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="2dc49-216">Quand le bouton est sélectionné, une demande `POST` de forumaire est envoyée au serveur.</span><span class="sxs-lookup"><span data-stu-id="2dc49-216">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="2dc49-217">Par convention, le nom de la méthode de gestionnaire est sélectionné en fonction de la valeur du paramètre `handler` conformément au schéma `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="2dc49-217">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="2dc49-218">Étant donné que le `handler` est `delete` dans cet exemple, la méthode de gestionnaire `OnPostDeleteAsync` est utilisée pour traiter la demande `POST`.</span><span class="sxs-lookup"><span data-stu-id="2dc49-218">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="2dc49-219">Si `asp-page-handler` est défini avec une autre valeur, telle que `remove`, une méthode de gestionnaire de page avec le nom `OnPostRemoveAsync` est sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="2dc49-219">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="2dc49-220">La méthode `OnPostDeleteAsync` :</span><span class="sxs-lookup"><span data-stu-id="2dc49-220">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="2dc49-221">Accepte l’`id` de la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="2dc49-221">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="2dc49-222">Interroge la base de données pour le contact client avec `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="2dc49-222">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="2dc49-223">Si le contact client est trouvé, il est supprimé de la liste des contacts client.</span><span class="sxs-lookup"><span data-stu-id="2dc49-223">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="2dc49-224">La base de données est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="2dc49-224">The database is updated.</span></span>
* <span data-ttu-id="2dc49-225">Appelle `RedirectToPage` pour rediriger vers la page Index racine (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="2dc49-225">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="2dc49-226">XSRF/CSRF et pages Razor</span><span class="sxs-lookup"><span data-stu-id="2dc49-226">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="2dc49-227">Vous n’avez aucun code à écrire pour la [validation anti-contrefaçon](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="2dc49-227">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="2dc49-228">La validation et la génération de jetons anti-contrefaçon sont automatiquement incluses dans les pages Razor.</span><span class="sxs-lookup"><span data-stu-id="2dc49-228">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="2dc49-229">Utilisation de dispositions, partiels, modèles et Tag Helpers avec les pages Razor</span><span class="sxs-lookup"><span data-stu-id="2dc49-229">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="2dc49-230">Les pages Razor fonctionnent avec toutes les fonctionnalités du moteur de vue Razor.</span><span class="sxs-lookup"><span data-stu-id="2dc49-230">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="2dc49-231">Les dispositions, partiels, modèles, Tag Helpers, *_ViewStart.cshtml* et *_ViewImports.cshtml* fonctionnent de la même façon que pour les vues Razor classiques.</span><span class="sxs-lookup"><span data-stu-id="2dc49-231">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="2dc49-232">Nous allons nettoyer un peu cette page en tirant parti de certaines de ces fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="2dc49-232">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="2dc49-233">Ajoutez une [page de disposition](xref:mvc/views/layout) à *Pages/_Layout.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2dc49-233">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="2dc49-234">La [disposition](xref:mvc/views/layout) :</span><span class="sxs-lookup"><span data-stu-id="2dc49-234">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="2dc49-235">Contrôle la disposition de chaque page (à moins que la page ne refuse la disposition).</span><span class="sxs-lookup"><span data-stu-id="2dc49-235">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="2dc49-236">Importe des structures HTML telles que JavaScript et les feuilles de style.</span><span class="sxs-lookup"><span data-stu-id="2dc49-236">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="2dc49-237">Pour plus d’informations, consultez [Page de disposition](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="2dc49-237">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="2dc49-238">La propriété [Layout](xref:mvc/views/layout#specifying-a-layout) est définie dans *Pages/_ViewStart.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2dc49-238">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="2dc49-239">**Remarque :** La disposition est dans le dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="2dc49-239">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="2dc49-240">Les pages recherchent d’autres vues (dispositions, modèles, partiels) hiérarchiquement, en commençant dans le même dossier que la page active.</span><span class="sxs-lookup"><span data-stu-id="2dc49-240">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="2dc49-241">Une disposition dans le dossier *Pages* peut être utilisée à partir de n’importe quelle page Razor sous le dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="2dc49-241">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="2dc49-242">Nous vous recommandons de **ne pas** placer le fichier de disposition dans le dossier *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="2dc49-242">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="2dc49-243">*Views/Shared* est un modèle de vues MVC.</span><span class="sxs-lookup"><span data-stu-id="2dc49-243">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="2dc49-244">Les pages Razor sont censées s’appuyer sur la hiérarchie des dossiers, pas sur les conventions de chemins.</span><span class="sxs-lookup"><span data-stu-id="2dc49-244">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="2dc49-245">La recherche de vue à partir d’une page Razor inclut le dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="2dc49-245">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="2dc49-246">Les dispositions, modèles et partiels que vous utilisez avec les contrôleurs MVC et les vues Razor conventionnelles *fonctionnent simplement*.</span><span class="sxs-lookup"><span data-stu-id="2dc49-246">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="2dc49-247">Ajoutez un fichier *Pages/_ViewImports.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2dc49-247">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="2dc49-248">`@namespace` est expliqué plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="2dc49-248">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="2dc49-249">La directive `@addTagHelper` permet de bénéficier des [Tag Helpers intégrés](xref:mvc/views/tag-helpers/builtin-th/Index) dans toutes les pages du dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="2dc49-249">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="2dc49-250">Quand la directive `@namespace` est utilisée explicitement sur une page :</span><span class="sxs-lookup"><span data-stu-id="2dc49-250">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="2dc49-251">La directive définit l’espace de noms pour la page.</span><span class="sxs-lookup"><span data-stu-id="2dc49-251">The directive sets the namespace for the page.</span></span> <span data-ttu-id="2dc49-252">La directive `@model` n’a pas besoin d’inclure l’espace de noms.</span><span class="sxs-lookup"><span data-stu-id="2dc49-252">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="2dc49-253">Quand la directive `@namespace` est contenue dans *_ViewImports.cshtml*, l’espace de noms spécifié fournit le préfixe de l’espace de noms généré dans la Page qui importe la directive `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="2dc49-253">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="2dc49-254">Le reste de l’espace de noms généré (la partie suffixe) est le chemin relatif séparé par un point entre le dossier contenant *_ViewImports.cshtml* et le dossier contenant la page.</span><span class="sxs-lookup"><span data-stu-id="2dc49-254">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="2dc49-255">Par exemple, le fichier code-behind *Pages/Customers/Edit.cshtml.cs* définit explicitement l’espace de noms :</span><span class="sxs-lookup"><span data-stu-id="2dc49-255">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="2dc49-256">Le fichier *Pages/_ViewImports.cshtml* définit l’espace de noms suivant :</span><span class="sxs-lookup"><span data-stu-id="2dc49-256">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="2dc49-257">L’espace de noms généré pour la page Razor *Pages/Customers/Edit.cshtml* est identique au fichier code-behind.</span><span class="sxs-lookup"><span data-stu-id="2dc49-257">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="2dc49-258">La directive `@namespace` a été conçue pour que les classes C# ajoutées à un projet et au code généré par les pages *fonctionnent simplement* sans avoir à ajouter de directive `@using` pour le fichier code-behind.</span><span class="sxs-lookup"><span data-stu-id="2dc49-258">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="2dc49-259">**Remarque :** `@namespace` fonctionne également avec les vues Razor classiques.</span><span class="sxs-lookup"><span data-stu-id="2dc49-259">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="2dc49-260">Le fichier de vue *Pages/Create.cshtml* d’origine :</span><span class="sxs-lookup"><span data-stu-id="2dc49-260">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="2dc49-261">Le fichier vue *Pages/Create.cshtml* mis à jour :</span><span class="sxs-lookup"><span data-stu-id="2dc49-261">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="2dc49-262">Le [projet de démarrage de pages Razor](#rpvs17) contient *Pages/_ValidationScriptsPartial.cshtml*, qui connecte la validation côté client.</span><span class="sxs-lookup"><span data-stu-id="2dc49-262">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="2dc49-263">Génération d’URL pour les pages</span><span class="sxs-lookup"><span data-stu-id="2dc49-263">URL generation for Pages</span></span>

<span data-ttu-id="2dc49-264">La page `Create`, illustrée précédemment, utilise `RedirectToPage` :</span><span class="sxs-lookup"><span data-stu-id="2dc49-264">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="2dc49-265">L’application a la structure de fichiers/dossiers suivante :</span><span class="sxs-lookup"><span data-stu-id="2dc49-265">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="2dc49-266">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="2dc49-266">*/Pages*</span></span>

  * <span data-ttu-id="2dc49-267">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2dc49-267">*Index.cshtml*</span></span>
  * <span data-ttu-id="2dc49-268">*/Customer*</span><span class="sxs-lookup"><span data-stu-id="2dc49-268">*/Customer*</span></span>

    * <span data-ttu-id="2dc49-269">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2dc49-269">*Create.cshtml*</span></span>
    * <span data-ttu-id="2dc49-270">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2dc49-270">*Edit.cshtml*</span></span>
    * <span data-ttu-id="2dc49-271">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2dc49-271">*Index.cshtml*</span></span>

<span data-ttu-id="2dc49-272">Une fois l’opération réussie, les pages *Pages/Customers/Create.cshtml* et *Pages/Customers/Edit.cshtml* redirigent vers *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2dc49-272">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="2dc49-273">La chaîne `/Index` fait partie de l’URI pour accéder à la page précédente.</span><span class="sxs-lookup"><span data-stu-id="2dc49-273">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="2dc49-274">La chaîne `/Index` peut être utilisée pour générer l’URI de la page *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2dc49-274">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="2dc49-275">Exemple :</span><span class="sxs-lookup"><span data-stu-id="2dc49-275">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="2dc49-276">Le nom de la page est le chemin de la page à partir du dossier racine */Pages* (y compris un `/` de début, par exemple `/Index`).</span><span class="sxs-lookup"><span data-stu-id="2dc49-276">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="2dc49-277">Les exemples de génération d’URL précédents sont beaucoup plus riches en fonctionnalités que le simple codage en dur d’une URL.</span><span class="sxs-lookup"><span data-stu-id="2dc49-277">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="2dc49-278">La génération d’URL utilise le [routage](xref:mvc/controllers/routing) et peut générer et encoder des paramètres en fonction de la façon dont l’itinéraire est défini dans le chemin de destination.</span><span class="sxs-lookup"><span data-stu-id="2dc49-278">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="2dc49-279">La génération d’URL pour les pages prend en charge les noms relatifs.</span><span class="sxs-lookup"><span data-stu-id="2dc49-279">URL generation for pages supports relative names.</span></span> <span data-ttu-id="2dc49-280">Le tableau suivant montre la page Index sélectionnée avec différents paramètres `RedirectToPage` à partir de *Pages/Customers/Create.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2dc49-280">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="2dc49-281">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="2dc49-281">RedirectToPage(x)</span></span>| <span data-ttu-id="2dc49-282">Page</span><span class="sxs-lookup"><span data-stu-id="2dc49-282">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="2dc49-283">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="2dc49-283">RedirectToPage("/Index")</span></span> | <span data-ttu-id="2dc49-284">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="2dc49-284">*Pages/Index*</span></span> |
| <span data-ttu-id="2dc49-285">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="2dc49-285">RedirectToPage("./Index");</span></span> | <span data-ttu-id="2dc49-286">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="2dc49-286">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="2dc49-287">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="2dc49-287">RedirectToPage("../Index")</span></span> | <span data-ttu-id="2dc49-288">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="2dc49-288">*Pages/Index*</span></span> |
| <span data-ttu-id="2dc49-289">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="2dc49-289">RedirectToPage("Index")</span></span>  | <span data-ttu-id="2dc49-290">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="2dc49-290">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="2dc49-291">`RedirectToPage("Index")`, `RedirectToPage("./Index")` et `RedirectToPage("../Index")` sont des *noms relatifs*.</span><span class="sxs-lookup"><span data-stu-id="2dc49-291">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="2dc49-292">Le paramètre `RedirectToPage` est *combiné* avec le chemin de la page active pour calculer le nom de la page de destination.</span><span class="sxs-lookup"><span data-stu-id="2dc49-292">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="2dc49-293">La liaison de nom relatif est utile lors de la création de sites avec une structure complexe.</span><span class="sxs-lookup"><span data-stu-id="2dc49-293">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="2dc49-294">Si vous utilisez des noms relatifs pour établir une liaison entre les pages d’un dossier, vous pouvez renommer ce dossier.</span><span class="sxs-lookup"><span data-stu-id="2dc49-294">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="2dc49-295">Tous les liens fonctionneront encore (car ils n’incluent pas le nom du dossier).</span><span class="sxs-lookup"><span data-stu-id="2dc49-295">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="2dc49-296">TempData</span><span class="sxs-lookup"><span data-stu-id="2dc49-296">TempData</span></span>

<span data-ttu-id="2dc49-297">ASP.NET Core expose la propriété [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) sur un [contrôleur](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="2dc49-297">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="2dc49-298">Cette propriété stocke les données jusqu’à ce qu’elles soient lues.</span><span class="sxs-lookup"><span data-stu-id="2dc49-298">This property stores data until it's read.</span></span> <span data-ttu-id="2dc49-299">Vous pouvez utiliser les méthodes `Keep` et `Peek` pour examiner les données sans suppression.</span><span class="sxs-lookup"><span data-stu-id="2dc49-299">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="2dc49-300">`TempData` est utile pour la redirection, quand des données sont nécessaires pour plusieurs requêtes.</span><span class="sxs-lookup"><span data-stu-id="2dc49-300">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="2dc49-301">L’attribut `[TempData]` est une nouveauté dans ASP.NET Core 2.0. Il est pris en charge sur les contrôleurs et les pages.</span><span class="sxs-lookup"><span data-stu-id="2dc49-301">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="2dc49-302">Le code suivant définit la valeur de `Message` à l’aide de `TempData` :</span><span class="sxs-lookup"><span data-stu-id="2dc49-302">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="2dc49-303">Le balisage suivant dans le fichier *Pages/Customers/Index.cshtml* affiche la valeur de `Message` à l’aide de `TempData`.</span><span class="sxs-lookup"><span data-stu-id="2dc49-303">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="2dc49-304">Le modèle de page *Pages/Customers/Index.cshtml.cs* applique l’attribut `[TempData]` à la propriété `Message`.</span><span class="sxs-lookup"><span data-stu-id="2dc49-304">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="2dc49-305">Pour plus d’informations, consultez [TempData](xref:fundamentals/app-state#temp).</span><span class="sxs-lookup"><span data-stu-id="2dc49-305">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="2dc49-306">Plusieurs gestionnaires par page</span><span class="sxs-lookup"><span data-stu-id="2dc49-306">Multiple handlers per page</span></span>

<span data-ttu-id="2dc49-307">La page suivante génère un balisage pour deux gestionnaires de page à l’aide du Tag Helper `asp-page-handler` :</span><span class="sxs-lookup"><span data-stu-id="2dc49-307">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="2dc49-308">Le formulaire dans l’exemple précédent contient deux boutons d’envoi, chacun utilisant `FormActionTagHelper` pour envoyer à une URL différente.</span><span class="sxs-lookup"><span data-stu-id="2dc49-308">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="2dc49-309">L’attribut `asp-page-handler` est un complément de `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="2dc49-309">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="2dc49-310">`asp-page-handler` génère des URL qui envoient à chacune des méthodes de gestionnaire définies par une page.</span><span class="sxs-lookup"><span data-stu-id="2dc49-310">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="2dc49-311">`asp-page` n’est pas spécifié, car l’exemple établit une liaison à la page active.</span><span class="sxs-lookup"><span data-stu-id="2dc49-311">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="2dc49-312">Le modèle de page :</span><span class="sxs-lookup"><span data-stu-id="2dc49-312">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="2dc49-313">Le code précédent utilise des *méthodes de gestionnaire nommées*.</span><span class="sxs-lookup"><span data-stu-id="2dc49-313">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="2dc49-314">Pour créer des méthodes de gestionnaire nommées, il faut prendre le texte dans le nom après `On<HTTP Verb>` et avant `Async` (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="2dc49-314">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="2dc49-315">Dans l’exemple précédent, les méthodes de page sont OnPost**JoinList**Async et OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="2dc49-315">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="2dc49-316">Avec *OnPost* et *Async* supprimés, les noms des gestionnaires sont `JoinList` et `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="2dc49-316">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="2dc49-317">Avec le code précédent, le chemin d’URL qui envoie à `OnPostJoinListAsync` est `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="2dc49-317">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="2dc49-318">Le chemin d’URL qui envoie à `OnPostJoinListUCAsync` est `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="2dc49-318">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="2dc49-319">Personnalisation du routage</span><span class="sxs-lookup"><span data-stu-id="2dc49-319">Customizing Routing</span></span>

<span data-ttu-id="2dc49-320">Si vous ne voulez pas avoir la chaîne de requête `?handler=JoinList` dans l’URL, vous pouvez changer l’itinéraire pour placer le nom du gestionnaire dans la partie chemin de l’URL.</span><span class="sxs-lookup"><span data-stu-id="2dc49-320">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="2dc49-321">Vous pouvez personnaliser l’itinéraire en ajoutant un modèle d’itinéraire placé entre des guillemets doubles après la directive `@page`.</span><span class="sxs-lookup"><span data-stu-id="2dc49-321">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="2dc49-322">L’itinéraire précédent place le nom du gestionnaire dans le chemin d’URL au lieu de la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="2dc49-322">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="2dc49-323">Le `?` suivant `handler` signifie que le paramètre d’itinéraire est facultatif.</span><span class="sxs-lookup"><span data-stu-id="2dc49-323">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="2dc49-324">Vous pouvez utiliser `@page` pour ajouter des segments et des paramètres supplémentaires à l’itinéraire d’une page.</span><span class="sxs-lookup"><span data-stu-id="2dc49-324">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="2dc49-325">Tout ce qui y figure est **ajouté** à l’itinéraire par défaut de la page.</span><span class="sxs-lookup"><span data-stu-id="2dc49-325">Whatever's there's **appended** to the default route of the page.</span></span> <span data-ttu-id="2dc49-326">L’utilisation d’un chemin absolu ou virtuel pour changer l’itinéraire de la page (comme `"~/Some/Other/Path"`) n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="2dc49-326">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) isn't supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="2dc49-327">Configuration et paramètres</span><span class="sxs-lookup"><span data-stu-id="2dc49-327">Configuration and settings</span></span>

<span data-ttu-id="2dc49-328">Pour configurer les options avancées, utilisez la méthode d’extension `AddRazorPagesOptions` sur le générateur MVC :</span><span class="sxs-lookup"><span data-stu-id="2dc49-328">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="2dc49-329">Actuellement, vous pouvez utiliser `RazorPagesOptions` pour définir le répertoire racine pour les pages, ou ajouter des conventions de modèle d’application pour les pages.</span><span class="sxs-lookup"><span data-stu-id="2dc49-329">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="2dc49-330">Nous permettrons à l’avenir une plus grande extensibilité en ce sens.</span><span class="sxs-lookup"><span data-stu-id="2dc49-330">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="2dc49-331">Pour précompiler des vues, consultez [Compilation de vue Razor](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="2dc49-331">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="2dc49-332">[Téléchargez ou affichez des exemples de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="2dc49-332">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="2dc49-333">Consultez [Bien démarrer avec les pages Razor](xref:tutorials/razor-pages/razor-pages-start) qui s’appuie sur cette introduction.</span><span class="sxs-lookup"><span data-stu-id="2dc49-333">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="2dc49-334">Spécifier que les pages Razor se trouvent à la racine du contenu</span><span class="sxs-lookup"><span data-stu-id="2dc49-334">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="2dc49-335">Par défaut, les pages Razor sont associées à la racine */Pages*.</span><span class="sxs-lookup"><span data-stu-id="2dc49-335">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="2dc49-336">Ajoutez [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) à [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) pour spécifier que vos pages Razor se trouvent à la racine de contenu ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) de l’application :</span><span class="sxs-lookup"><span data-stu-id="2dc49-336">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="2dc49-337">Spécifier que les pages Razor se trouvent dans un répertoire racine personnalisé</span><span class="sxs-lookup"><span data-stu-id="2dc49-337">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="2dc49-338">Ajoutez [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) à [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) pour spécifier que vos pages Razor se trouvent dans le répertoire racine personnalisé de l’application (fournissez un chemin relatif) :</span><span class="sxs-lookup"><span data-stu-id="2dc49-338">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="2dc49-339">Voir aussi</span><span class="sxs-lookup"><span data-stu-id="2dc49-339">See also</span></span>

* [<span data-ttu-id="2dc49-340">Présentation d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2dc49-340">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="2dc49-341">Bien démarrer avec les pages Razor</span><span class="sxs-lookup"><span data-stu-id="2dc49-341">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="2dc49-342">Conventions d’autorisation des pages Razor</span><span class="sxs-lookup"><span data-stu-id="2dc49-342">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="2dc49-343">Itinéraire personnalisé des pages Razor et fournisseurs de modèle de page</span><span class="sxs-lookup"><span data-stu-id="2dc49-343">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* [<span data-ttu-id="2dc49-344">Test unitaire et d’intégration des pages Razor</span><span class="sxs-lookup"><span data-stu-id="2dc49-344">Razor Pages unit and integration testing</span></span>](xref:testing/razor-pages-testing)
