---
title: Créer votre première application Composants Razor
author: guardrex
description: Créez une application Composants Razor pas à pas et découvrez les concepts de base des Composants Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/24/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: 2a987b3f2e687cd9d4dffa2c573c938e68ea3cc8
ms.sourcegitcommit: 7d6019f762fc5b8cbedcd69801e8310f51a17c18
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419363"
---
# <a name="build-your-first-razor-components-app"></a><span data-ttu-id="d6734-103">Créer votre première application Composants Razor</span><span class="sxs-lookup"><span data-stu-id="d6734-103">Build your first Razor Components app</span></span>

<span data-ttu-id="d6734-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d6734-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="d6734-105">Ce tutoriel vous montre comment créer une application avec les Composants Razor et illustre les concepts de base des Composants Razor.</span><span class="sxs-lookup"><span data-stu-id="d6734-105">This tutorial shows you how to build an app with Razor Components and demonstrates basic Razor Components concepts.</span></span> <span data-ttu-id="d6734-106">Vous pouvez profiter de ce tutoriel en utilisant soit un projet basé sur les Composants Razor (pris en charge dans .NET Core 3.0 ou version ultérieure) ou un projet Blazor (pris en charge dans une prochaine version de .NET Core).</span><span class="sxs-lookup"><span data-stu-id="d6734-106">You can enjoy this tutorial using either a Razor Components-based project (supported in .NET Core 3.0 or later) or using a Blazor-based project (supported in a future release of .NET Core).</span></span>

<span data-ttu-id="d6734-107">Pour une expérience à l’aide de Composants Razor ASP.NET Core (*recommandé*) :</span><span class="sxs-lookup"><span data-stu-id="d6734-107">For an experience using ASP.NET Core Razor Components (*recommended*):</span></span>

* <span data-ttu-id="d6734-108">Suivez les instructions dans <xref:razor-components/get-started> pour créer un projet basé sur les Composants Razor.</span><span class="sxs-lookup"><span data-stu-id="d6734-108">Follow the guidance in <xref:razor-components/get-started> to create a Razor Components-based project.</span></span>
* <span data-ttu-id="d6734-109">Attribuez un nom au projet `RazorComponents`.</span><span class="sxs-lookup"><span data-stu-id="d6734-109">Name the project `RazorComponents`.</span></span>

<span data-ttu-id="d6734-110">Pour une expérience à l’aide de Blazor :</span><span class="sxs-lookup"><span data-stu-id="d6734-110">For an experience using Blazor:</span></span>

* <span data-ttu-id="d6734-111">Suivez les instructions dans <xref:spa/blazor/get-started> pour créer un projet basé sur Blazor.</span><span class="sxs-lookup"><span data-stu-id="d6734-111">Follow the guidance in <xref:spa/blazor/get-started> to create a Blazor-based project.</span></span>
* <span data-ttu-id="d6734-112">Attribuez un nom au projet `Blazor`.</span><span class="sxs-lookup"><span data-stu-id="d6734-112">Name the project `Blazor`.</span></span>

<span data-ttu-id="d6734-113">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d6734-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="d6734-114">Pour connaître les prérequis, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="d6734-114">See the following topics for prerequisites:</span></span>

## <a name="build-components"></a><span data-ttu-id="d6734-115">Construire des composants</span><span class="sxs-lookup"><span data-stu-id="d6734-115">Build components</span></span>

1. <span data-ttu-id="d6734-116">Accédez à chacune des trois pages de l’application dans le dossier *Components/Pages* (*Pages* dans Blazor) : Accueil, Compteur et Extraire des données.</span><span class="sxs-lookup"><span data-stu-id="d6734-116">Browse to each of the app's three pages in the *Components/Pages* folder (*Pages* in Blazor): Home, Counter, and Fetch data.</span></span> <span data-ttu-id="d6734-117">Ces pages sont implémentées par les fichiers du composant Razor : *Index.razor*, *Counter.razor* et *FetchData.razor*.</span><span class="sxs-lookup"><span data-stu-id="d6734-117">These pages are implemented by Razor Component files: *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span> <span data-ttu-id="d6734-118">(Blazor continue d’utiliser l’extension de fichier *.cshtml* : *Index.cshtml*, *Counter.cshtml* et *FetchData.cshtml*.)</span><span class="sxs-lookup"><span data-stu-id="d6734-118">(Blazor continues to use the *.cshtml* file extension: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.)</span></span>

1. <span data-ttu-id="d6734-119">Sur la page Counter, sélectionnez le bouton **Click me** pour incrémenter le compteur sans actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="d6734-119">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="d6734-120">L’incrémentation d’un compteur dans une page web requiert normalement l’écriture JavaScript, mais le projet Composants Razor fournit une meilleure approche à l’aide C#.</span><span class="sxs-lookup"><span data-stu-id="d6734-120">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

1. <span data-ttu-id="d6734-121">Examinez l’implémentation du composant Counter dans le fichier *Counter.razor*.</span><span class="sxs-lookup"><span data-stu-id="d6734-121">Examine the implementation of the Counter component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="d6734-122">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* dans Blazor) :</span><span class="sxs-lookup"><span data-stu-id="d6734-122">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="d6734-123">L’interface utilisateur du composant Counter est définie à l’aide de HTML.</span><span class="sxs-lookup"><span data-stu-id="d6734-123">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="d6734-124">La logique de rendu dynamique (par exemple, les boucles, conditions, expressions) est ajoutée à l’aide d’une syntaxe C# intégrée appelée [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="d6734-124">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="d6734-125">Le balisage HTML et la logique de rendu C# sont convertis en une classe de composants au moment de la génération.</span><span class="sxs-lookup"><span data-stu-id="d6734-125">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="d6734-126">Le nom de la classe .NET générée correspond à celui du fichier.</span><span class="sxs-lookup"><span data-stu-id="d6734-126">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="d6734-127">Les membres de la classe de composants sont définis dans un bloc `@functions`.</span><span class="sxs-lookup"><span data-stu-id="d6734-127">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="d6734-128">Dans le bloc `@functions`, l’état du composant (propriétés, champs) et les méthodes sont spécifiés pour la gestion des événements ou pour définir une autre logique de composant.</span><span class="sxs-lookup"><span data-stu-id="d6734-128">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="d6734-129">Ces membres sont ensuite utilisés dans le cadre de la logique de rendu du composant et la gestion des événements.</span><span class="sxs-lookup"><span data-stu-id="d6734-129">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="d6734-130">Lorsque le bouton **Click me** est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="d6734-130">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="d6734-131">Le gestionnaire `onclick` enregistré du composant Counter est appelé (méthode `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="d6734-131">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="d6734-132">Le composant Counter regénère son arborescence de rendu.</span><span class="sxs-lookup"><span data-stu-id="d6734-132">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="d6734-133">La nouvelle arborescence de rendu est comparée à la précédente.</span><span class="sxs-lookup"><span data-stu-id="d6734-133">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="d6734-134">Seules les modifications apportées à Document Object Model (DOM) sont appliquées.</span><span class="sxs-lookup"><span data-stu-id="d6734-134">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="d6734-135">Le compte affiché est mis à jour.</span><span class="sxs-lookup"><span data-stu-id="d6734-135">The displayed count is updated.</span></span>

1. <span data-ttu-id="d6734-136">Modifiez la logique C# du composant Counter pour que l’incrément du compte passe de un à deux.</span><span class="sxs-lookup"><span data-stu-id="d6734-136">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="d6734-137">Régénérez et exécutez l’application pour voir les modifications.</span><span class="sxs-lookup"><span data-stu-id="d6734-137">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="d6734-138">Sélectionnez le bouton **Click me** et le compteur passe à deux incréments.</span><span class="sxs-lookup"><span data-stu-id="d6734-138">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="d6734-139">Utiliser des composants</span><span class="sxs-lookup"><span data-stu-id="d6734-139">Use components</span></span>

<span data-ttu-id="d6734-140">Incluez un composant dans un autre composant à l’aide d’une syntaxe de type HTML.</span><span class="sxs-lookup"><span data-stu-id="d6734-140">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="d6734-141">Ajoutez le composant Counter au composant Index (Home) de l’application en ajoutant un élément `<Counter />` au composant Index.</span><span class="sxs-lookup"><span data-stu-id="d6734-141">Add the Counter component to the app's Index (Home) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="d6734-142">Si vous utilisez Blazor pour cette expérience, un composant Survey Prompt (élément `<SurveyPrompt>`) se trouve dans le composant Index.</span><span class="sxs-lookup"><span data-stu-id="d6734-142">If you're using Blazor for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="d6734-143">Remplacez l’élément `<SurveyPrompt>` par l’élément `<Counter>`.</span><span class="sxs-lookup"><span data-stu-id="d6734-143">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span>

   <span data-ttu-id="d6734-144">*Components/Pages/Index.razor* (*Pages/Index.cshtml* dans Blazor) :</span><span class="sxs-lookup"><span data-stu-id="d6734-144">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.razor?highlight=7)]

1. <span data-ttu-id="d6734-145">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="d6734-145">Rebuild and run the app.</span></span> <span data-ttu-id="d6734-146">La page d’accueil a son propre compteur.</span><span class="sxs-lookup"><span data-stu-id="d6734-146">The Home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="d6734-147">Paramètres de composant</span><span class="sxs-lookup"><span data-stu-id="d6734-147">Component parameters</span></span>

<span data-ttu-id="d6734-148">Les composants peuvent également avoir des paramètres.</span><span class="sxs-lookup"><span data-stu-id="d6734-148">Components can also have parameters.</span></span> <span data-ttu-id="d6734-149">Les paramètres des composants sont définis à l’aide de propriétés non publiques sur la classe de composants décorée avec `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="d6734-149">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="d6734-150">Utilisez des attributs pour spécifier des arguments pour un composant dans le balisage.</span><span class="sxs-lookup"><span data-stu-id="d6734-150">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="d6734-151">Mettez à jour le code C# `@functions` du composant :</span><span class="sxs-lookup"><span data-stu-id="d6734-151">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="d6734-152">Ajoutez une propriété `IncrementAmount` décorée avec l’attribut `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="d6734-152">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="d6734-153">Modifiez la méthode `IncrementCount` pour utiliser `IncrementAmount` lorsque vous augmentez la valeur de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="d6734-153">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="d6734-154">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* dans Blazor) :</span><span class="sxs-lookup"><span data-stu-id="d6734-154">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Counter.razor?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="d6734-155">Spécifiez un paramètre `IncrementAmount` dans l’élément `<Counter>` du composant Home à l’aide d’un attribut.</span><span class="sxs-lookup"><span data-stu-id="d6734-155">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="d6734-156">Définissez la valeur pour incrémenter le compteur par 10.</span><span class="sxs-lookup"><span data-stu-id="d6734-156">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="d6734-157">*Components/Pages/Index.razor* (*Pages/Index.cshtml* dans Blazor) :</span><span class="sxs-lookup"><span data-stu-id="d6734-157">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Index.razor?highlight=7)]

1. <span data-ttu-id="d6734-158">Rechargez la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="d6734-158">Reload the Home page.</span></span> <span data-ttu-id="d6734-159">Le compteur s’incrémente de dix unités chaque fois que le bouton **Cliquer ici** est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="d6734-159">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="d6734-160">Le compteur sur la page Counter s’incrémente d’une unité.</span><span class="sxs-lookup"><span data-stu-id="d6734-160">The counter on the Counter page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="d6734-161">Acheminer vers les composants</span><span class="sxs-lookup"><span data-stu-id="d6734-161">Route to components</span></span>

<span data-ttu-id="d6734-162">La directive `@page` en haut du fichier *Counter.razor* spécifie que ce composant est un point de terminaison de routage.</span><span class="sxs-lookup"><span data-stu-id="d6734-162">The `@page` directive at the top of the *Counter.razor* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="d6734-163">Le composant Counter gère les requêtes envoyées à `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="d6734-163">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="d6734-164">Sans la directive `@page`, le composant ne gère pas les requêtes acheminées, mais le composant peut toujours être utilisé par d’autres composants.</span><span class="sxs-lookup"><span data-stu-id="d6734-164">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="d6734-165">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="d6734-165">Dependency injection</span></span>

<span data-ttu-id="d6734-166">Les services enregistrés dans le conteneur de services de l’application sont disponibles pour les composants via [l’injection de dépendance (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d6734-166">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d6734-167">Injectez des services dans un composant à l’aide de la directive `@inject`.</span><span class="sxs-lookup"><span data-stu-id="d6734-167">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="d6734-168">Examinez les directives du composant FetchData dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="d6734-168">Examine the directives of the FetchData component in the sample app.</span></span>

<span data-ttu-id="d6734-169">Dans l’exemple d’application Razor Components, le service `WeatherForecastService` est inscrit en tant que [singleton](xref:fundamentals/dependency-injection#service-lifetimes), de sorte qu’une instance du service est disponible dans toute l’application.</span><span class="sxs-lookup"><span data-stu-id="d6734-169">In the Razor Components sample app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span> <span data-ttu-id="d6734-170">La directive `@inject` est utilisée pour injecter l’instance du service `WeatherForecastService` dans le composant.</span><span class="sxs-lookup"><span data-stu-id="d6734-170">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component.</span></span>

<span data-ttu-id="d6734-171">*Components/Pages/FetchData.razor* :</span><span class="sxs-lookup"><span data-stu-id="d6734-171">*Components/Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="d6734-172">Le composant FetchData utilise le service injecté, comme `ForecastService`, pour récupérer un tableau d’objets `WeatherForecast` :</span><span class="sxs-lookup"><span data-stu-id="d6734-172">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="d6734-173">Dans la version Blazor de l’exemple d’application, `HttpClient` est injecté pour obtenir des données de prévisions météorologiques à partir des fichiers *weather.json* du dossier *wwwroot/sample-data* :</span><span class="sxs-lookup"><span data-stu-id="d6734-173">In the Blazor version of the sample app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder:</span></span>

<span data-ttu-id="d6734-174">*Pages/FetchData.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="d6734-174">*Pages/FetchData.cshtml*:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=7)]

<span data-ttu-id="d6734-175">Dans les deux exemples d’applications, une boucle [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) est utilisée pour restituer chaque instance de prévision sous forme de ligne dans la table des données météorologiques :</span><span class="sxs-lookup"><span data-stu-id="d6734-175">In both sample apps, a [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="d6734-176">Générer une liste de tâches</span><span class="sxs-lookup"><span data-stu-id="d6734-176">Build a todo list</span></span>

<span data-ttu-id="d6734-177">Ajoutez un nouveau composant à l’application qui implémente une liste de tâches simple.</span><span class="sxs-lookup"><span data-stu-id="d6734-177">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="d6734-178">Ajoutez un fichier vide à l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="d6734-178">Add an empty file to the sample app:</span></span>

   * <span data-ttu-id="d6734-179">Pour l’expérience Razor Components, ajoutez un fichier *Todo.razor* au dossier *Components/Pages*.</span><span class="sxs-lookup"><span data-stu-id="d6734-179">For the Razor Components experience, add a *Todo.razor* file to the *Components/Pages* folder.</span></span>
   * <span data-ttu-id="d6734-180">Pour l’expérience Blazor, ajoutez un fichier *Todo.cshtml* au dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="d6734-180">For the Blazor experience, add a *Todo.cshtml* file to the *Pages* folder.</span></span>

1. <span data-ttu-id="d6734-181">Fournissez le balisage initial pour le composant :</span><span class="sxs-lookup"><span data-stu-id="d6734-181">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="d6734-182">Ajoutez le composant Todo à la barre de navigation.</span><span class="sxs-lookup"><span data-stu-id="d6734-182">Add the Todo component to the navigation bar.</span></span>

   <span data-ttu-id="d6734-183">Le composant NavMenu (*Components/Shared/NavMenu.razor* ou *Shared/NavMenu.cshtml* dans Blazor) est utilisé dans la disposition de l’application.</span><span class="sxs-lookup"><span data-stu-id="d6734-183">The NavMenu component (*Components/Shared/NavMenu.razor* or *Shared/NavMenu.cshtml* in Blazor) is used in the app's layout.</span></span> <span data-ttu-id="d6734-184">Les dispositions sont des composants qui vous permettent d’éviter la duplication de contenu dans l’application.</span><span class="sxs-lookup"><span data-stu-id="d6734-184">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="d6734-185">Pour plus d'informations, consultez <xref:razor-components/layouts>.</span><span class="sxs-lookup"><span data-stu-id="d6734-185">For more information, see <xref:razor-components/layouts>.</span></span>

   <span data-ttu-id="d6734-186">Ajoutez un `<NavLink>` pour le composant Todo en ajoutant le balisage d’élément de liste suivant sous les éléments de liste existants dans le fichier *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* dans Blazor) :</span><span class="sxs-lookup"><span data-stu-id="d6734-186">Add a `<NavLink>` for the Todo component by adding the following list item markup below the existing list items in the *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* in Blazor) file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="d6734-187">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="d6734-187">Rebuild and run the app.</span></span> <span data-ttu-id="d6734-188">Consultez la nouvelle page Todo pour vérifier que le lien vers le composant Todo fonctionne.</span><span class="sxs-lookup"><span data-stu-id="d6734-188">Visit the new Todo page to confirm that the link to the Todo component works.</span></span>

1. <span data-ttu-id="d6734-189">Ajoutez un fichier *TodoItem.cs* à la racine du projet pour contenir une classe qui représente un élément Todo.</span><span class="sxs-lookup"><span data-stu-id="d6734-189">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="d6734-190">Utilisez le code C# suivant pour la classe `TodoItem` :</span><span class="sxs-lookup"><span data-stu-id="d6734-190">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/TodoItem.cs)]

1. <span data-ttu-id="d6734-191">Retournez au composant Todo (*Components/Pages/Todo.razor* ou *Pages/Todo.cshtml* dans Blazor) :</span><span class="sxs-lookup"><span data-stu-id="d6734-191">Return to the Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   * <span data-ttu-id="d6734-192">Ajoutez un champ pour les éléments Todo dans un bloc `@functions`.</span><span class="sxs-lookup"><span data-stu-id="d6734-192">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="d6734-193">Le composant Todo utilise ce champ pour maintenir l’état de la liste de tâches.</span><span class="sxs-lookup"><span data-stu-id="d6734-193">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="d6734-194">Ajoutez un balisage de liste non triée et une boucle `foreach` pour effectuer le rendu de chaque élément todo en tant qu’élément de liste.</span><span class="sxs-lookup"><span data-stu-id="d6734-194">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="d6734-195">L’application nécessite des éléments d’interface utilisateur pour ajouter des éléments todo à la liste.</span><span class="sxs-lookup"><span data-stu-id="d6734-195">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="d6734-196">Ajoutez une entrée de texte et un bouton sous la liste :</span><span class="sxs-lookup"><span data-stu-id="d6734-196">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="d6734-197">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="d6734-197">Rebuild and run the app.</span></span> <span data-ttu-id="d6734-198">Lorsque le bouton **Add todo** est sélectionné, rien ne se produit car aucun gestionnaire d’événements n’est lié au bouton.</span><span class="sxs-lookup"><span data-stu-id="d6734-198">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="d6734-199">Ajoutez une méthode `AddTodo` au composant Todo et enregistrez-la pour les clics sur le bouton à l’aide de l’attribut `onclick` :</span><span class="sxs-lookup"><span data-stu-id="d6734-199">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   <span data-ttu-id="d6734-200">La méthode C# `AddTodo` est appelée lorsque le bouton est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="d6734-200">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="d6734-201">Pour obtenir le titre du nouvel élément todo, ajoutez un champ de chaîne `newTodo` et liez-le à la valeur du texte d’entrée à l’aide de l’attribut `bind` :</span><span class="sxs-lookup"><span data-stu-id="d6734-201">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. <span data-ttu-id="d6734-202">Mettez à jour la méthode `AddTodo` pour ajouter `TodoItem` avec le titre spécifié à la liste.</span><span class="sxs-lookup"><span data-stu-id="d6734-202">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="d6734-203">Supprimez la valeur du texte d’entrée en définissant `newTodo` sur une chaîne vide :</span><span class="sxs-lookup"><span data-stu-id="d6734-203">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="d6734-204">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="d6734-204">Rebuild and run the app.</span></span> <span data-ttu-id="d6734-205">Ajoutez quelques éléments todo à la liste Todo pour tester le nouveau code.</span><span class="sxs-lookup"><span data-stu-id="d6734-205">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="d6734-206">Le texte du titre pour chaque élément todo peut être rendu modifiable et une case à cocher peut aider l’utilisateur à effectuer le suivi des éléments terminés.</span><span class="sxs-lookup"><span data-stu-id="d6734-206">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="d6734-207">Ajoutez une entrée de case à cocher pour chaque élément todo et liez sa valeur à la propriété `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="d6734-207">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="d6734-208">Remplacez `@todo.Title` par un élément `<input>` lié à `@todo.Title` :</span><span class="sxs-lookup"><span data-stu-id="d6734-208">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="d6734-209">Pour vérifier que ces valeurs sont liées, mettez à jour l’en-tête `<h1>` pour afficher le nombre d’éléments todo qui ne sont pas terminés (`IsDone` est `false`).</span><span class="sxs-lookup"><span data-stu-id="d6734-209">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="d6734-210">Le composant Todo complet (*Components/Pages/Todo.razor* ou *Pages/Todo.cshtml* dans Blazor) :</span><span class="sxs-lookup"><span data-stu-id="d6734-210">The completed Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Todo.razor)]

1. <span data-ttu-id="d6734-211">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="d6734-211">Rebuild and run the app.</span></span> <span data-ttu-id="d6734-212">Ajoutez des éléments todo pour tester le nouveau code.</span><span class="sxs-lookup"><span data-stu-id="d6734-212">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="d6734-213">Publier et déployer l’application</span><span class="sxs-lookup"><span data-stu-id="d6734-213">Publish and deploy the app</span></span>

<span data-ttu-id="d6734-214">Pour publier l’application, consultez <xref:host-and-deploy/razor-components/index#publish-the-app>.</span><span class="sxs-lookup"><span data-stu-id="d6734-214">To publish the app, see <xref:host-and-deploy/razor-components/index#publish-the-app>.</span></span>
