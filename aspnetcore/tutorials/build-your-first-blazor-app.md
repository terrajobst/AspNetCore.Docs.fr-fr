---
title: Créer votre première application Blazor
author: guardrex
description: Créez une application Blazor étape par étape.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 310eb211f68076d6f52d6427940e07736d833e5b
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614658"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="6de42-103">Créer votre première application Blazor</span><span class="sxs-lookup"><span data-stu-id="6de42-103">Build your first Blazor app</span></span>

<span data-ttu-id="6de42-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6de42-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="6de42-105">Ce tutoriel vous montre comment créer une application avec Razor Components côté serveur ou avec Blazor côté client.</span><span class="sxs-lookup"><span data-stu-id="6de42-105">This tutorial shows you how to build an app with server-side Razor Components or client-side Blazor.</span></span>

<span data-ttu-id="6de42-106">Pour utiliser ASP.NET Core Razor Components côté serveur (*recommandé*) :</span><span class="sxs-lookup"><span data-stu-id="6de42-106">For an experience using server-side ASP.NET Core Razor Components (*recommended*):</span></span>

* <span data-ttu-id="6de42-107">Suivez les instructions concernant l’*expérience Razor Components* dans <xref:blazor/get-started#server-side-razor-components-experience> pour créer un projet Razor Components.</span><span class="sxs-lookup"><span data-stu-id="6de42-107">Follow the *Razor Components experience* guidance in <xref:blazor/get-started#server-side-razor-components-experience> to create a Razor Components-based project.</span></span>
* <span data-ttu-id="6de42-108">Attribuez un nom au projet `RazorComponents`.</span><span class="sxs-lookup"><span data-stu-id="6de42-108">Name the project `RazorComponents`.</span></span>

<span data-ttu-id="6de42-109">Pour une expérience à l’aide de Blazor :</span><span class="sxs-lookup"><span data-stu-id="6de42-109">For an experience using Blazor:</span></span>

* <span data-ttu-id="6de42-110">Suivez les instructions concernant l’*expérience Blazor* dans <xref:blazor/get-started#client-side-blazor-experience> pour créer un projet Blazor.</span><span class="sxs-lookup"><span data-stu-id="6de42-110">Follow the *Blazor experience* guidance in <xref:blazor/get-started#client-side-blazor-experience> to create a Blazor-based project.</span></span>
* <span data-ttu-id="6de42-111">Attribuez un nom au projet `Blazor`.</span><span class="sxs-lookup"><span data-stu-id="6de42-111">Name the project `Blazor`.</span></span>

<span data-ttu-id="6de42-112">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="6de42-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="6de42-113">Pour connaître les prérequis, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="6de42-113">See the following topics for prerequisites:</span></span>

## <a name="build-components"></a><span data-ttu-id="6de42-114">Construire des composants</span><span class="sxs-lookup"><span data-stu-id="6de42-114">Build components</span></span>

1. <span data-ttu-id="6de42-115">Accédez à chacune des trois pages de l’application dans le dossier *Components/Pages* (*Pages* dans Blazor) : Accueil, Compteur et Extraire des données.</span><span class="sxs-lookup"><span data-stu-id="6de42-115">Browse to each of the app's three pages in the *Components/Pages* folder (*Pages* in Blazor): Home, Counter, and Fetch data.</span></span> <span data-ttu-id="6de42-116">Ces pages sont implémentées par les fichiers du composant Razor : *Index.razor*, *Counter.razor* et *FetchData.razor*.</span><span class="sxs-lookup"><span data-stu-id="6de42-116">These pages are implemented by Razor Component files: *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span> <span data-ttu-id="6de42-117">(Blazor continue d’utiliser l’extension de fichier *.cshtml* : *Index.cshtml*, *Counter.cshtml* et *FetchData.cshtml*.)</span><span class="sxs-lookup"><span data-stu-id="6de42-117">(Blazor continues to use the *.cshtml* file extension: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.)</span></span>

1. <span data-ttu-id="6de42-118">Sur la page Counter, sélectionnez le bouton **Click me** pour incrémenter le compteur sans actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="6de42-118">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="6de42-119">L’incrémentation d’un compteur dans une page web nécessite normalement l’écriture de code JavaScript. Toutefois, Blazor fournit une meilleure approche grâce à l’utilisation du langage C#.</span><span class="sxs-lookup"><span data-stu-id="6de42-119">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

1. <span data-ttu-id="6de42-120">Examinez l’implémentation du composant Counter dans le fichier *Counter.razor*.</span><span class="sxs-lookup"><span data-stu-id="6de42-120">Examine the implementation of the Counter component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="6de42-121">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* dans Blazor) :</span><span class="sxs-lookup"><span data-stu-id="6de42-121">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="6de42-122">L’interface utilisateur du composant Counter est définie à l’aide de HTML.</span><span class="sxs-lookup"><span data-stu-id="6de42-122">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="6de42-123">La logique de rendu dynamique (par exemple, les boucles, conditions, expressions) est ajoutée à l’aide d’une syntaxe C# intégrée appelée [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="6de42-123">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="6de42-124">Le balisage HTML et la logique de rendu C# sont convertis en une classe de composants au moment de la génération.</span><span class="sxs-lookup"><span data-stu-id="6de42-124">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="6de42-125">Le nom de la classe .NET générée correspond à celui du fichier.</span><span class="sxs-lookup"><span data-stu-id="6de42-125">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="6de42-126">Les membres de la classe de composants sont définis dans un bloc `@functions`.</span><span class="sxs-lookup"><span data-stu-id="6de42-126">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="6de42-127">Dans le bloc `@functions`, l’état du composant (propriétés, champs) et les méthodes sont spécifiés pour la gestion des événements ou pour définir une autre logique de composant.</span><span class="sxs-lookup"><span data-stu-id="6de42-127">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="6de42-128">Ces membres sont ensuite utilisés dans le cadre de la logique de rendu du composant et la gestion des événements.</span><span class="sxs-lookup"><span data-stu-id="6de42-128">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="6de42-129">Lorsque le bouton **Click me** est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="6de42-129">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="6de42-130">Le gestionnaire `onclick` enregistré du composant Counter est appelé (méthode `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="6de42-130">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="6de42-131">Le composant Counter regénère son arborescence de rendu.</span><span class="sxs-lookup"><span data-stu-id="6de42-131">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="6de42-132">La nouvelle arborescence de rendu est comparée à la précédente.</span><span class="sxs-lookup"><span data-stu-id="6de42-132">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="6de42-133">Seules les modifications apportées à Document Object Model (DOM) sont appliquées.</span><span class="sxs-lookup"><span data-stu-id="6de42-133">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="6de42-134">Le compte affiché est mis à jour.</span><span class="sxs-lookup"><span data-stu-id="6de42-134">The displayed count is updated.</span></span>

1. <span data-ttu-id="6de42-135">Modifiez la logique C# du composant Counter pour que l’incrément du compte passe de un à deux.</span><span class="sxs-lookup"><span data-stu-id="6de42-135">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="6de42-136">Régénérez et exécutez l’application pour voir les modifications.</span><span class="sxs-lookup"><span data-stu-id="6de42-136">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="6de42-137">Sélectionnez le bouton **Click me** et le compteur passe à deux incréments.</span><span class="sxs-lookup"><span data-stu-id="6de42-137">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="6de42-138">Utiliser des composants</span><span class="sxs-lookup"><span data-stu-id="6de42-138">Use components</span></span>

<span data-ttu-id="6de42-139">Incluez un composant dans un autre composant à l’aide d’une syntaxe de type HTML.</span><span class="sxs-lookup"><span data-stu-id="6de42-139">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="6de42-140">Ajoutez le composant Counter au composant Index (Home) de l’application en ajoutant un élément `<Counter />` au composant Index.</span><span class="sxs-lookup"><span data-stu-id="6de42-140">Add the Counter component to the app's Index (Home) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="6de42-141">Si vous utilisez Blazor pour cette expérience, un composant Survey Prompt (élément `<SurveyPrompt>`) se trouve dans le composant Index.</span><span class="sxs-lookup"><span data-stu-id="6de42-141">If you're using Blazor for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="6de42-142">Remplacez l’élément `<SurveyPrompt>` par l’élément `<Counter>`.</span><span class="sxs-lookup"><span data-stu-id="6de42-142">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span>

   <span data-ttu-id="6de42-143">*Components/Pages/Index.razor* (*Pages/Index.cshtml* dans Blazor) :</span><span class="sxs-lookup"><span data-stu-id="6de42-143">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index.razor?highlight=7)]

1. <span data-ttu-id="6de42-144">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="6de42-144">Rebuild and run the app.</span></span> <span data-ttu-id="6de42-145">La page d’accueil a son propre compteur.</span><span class="sxs-lookup"><span data-stu-id="6de42-145">The Home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="6de42-146">Paramètres de composant</span><span class="sxs-lookup"><span data-stu-id="6de42-146">Component parameters</span></span>

<span data-ttu-id="6de42-147">Les composants peuvent également avoir des paramètres.</span><span class="sxs-lookup"><span data-stu-id="6de42-147">Components can also have parameters.</span></span> <span data-ttu-id="6de42-148">Les paramètres des composants sont définis à l’aide de propriétés non publiques sur la classe de composants décorée avec `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="6de42-148">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="6de42-149">Utilisez des attributs pour spécifier des arguments pour un composant dans le balisage.</span><span class="sxs-lookup"><span data-stu-id="6de42-149">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="6de42-150">Mettez à jour le code C# `@functions` du composant :</span><span class="sxs-lookup"><span data-stu-id="6de42-150">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="6de42-151">Ajoutez une propriété `IncrementAmount` décorée avec l’attribut `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="6de42-151">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="6de42-152">Modifiez la méthode `IncrementCount` pour utiliser `IncrementAmount` lorsque vous augmentez la valeur de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="6de42-152">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="6de42-153">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* dans Blazor) :</span><span class="sxs-lookup"><span data-stu-id="6de42-153">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/Components/Pages/Counter.razor?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="6de42-154">Spécifiez un paramètre `IncrementAmount` dans l’élément `<Counter>` du composant Home à l’aide d’un attribut.</span><span class="sxs-lookup"><span data-stu-id="6de42-154">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="6de42-155">Définissez la valeur pour incrémenter le compteur par 10.</span><span class="sxs-lookup"><span data-stu-id="6de42-155">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="6de42-156">*Components/Pages/Index.razor* (*Pages/Index.cshtml* dans Blazor) :</span><span class="sxs-lookup"><span data-stu-id="6de42-156">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/Components/Pages/Index.razor?highlight=7)]

1. <span data-ttu-id="6de42-157">Rechargez la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="6de42-157">Reload the Home page.</span></span> <span data-ttu-id="6de42-158">Le compteur s’incrémente de dix unités chaque fois que le bouton **Cliquer ici** est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="6de42-158">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="6de42-159">Le compteur sur la page Counter s’incrémente d’une unité.</span><span class="sxs-lookup"><span data-stu-id="6de42-159">The counter on the Counter page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="6de42-160">Acheminer vers les composants</span><span class="sxs-lookup"><span data-stu-id="6de42-160">Route to components</span></span>

<span data-ttu-id="6de42-161">La directive `@page` en haut du fichier *Counter.razor* spécifie que ce composant est un point de terminaison de routage.</span><span class="sxs-lookup"><span data-stu-id="6de42-161">The `@page` directive at the top of the *Counter.razor* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="6de42-162">Le composant Counter gère les requêtes envoyées à `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="6de42-162">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="6de42-163">Sans la directive `@page`, le composant ne gère pas les requêtes acheminées, mais le composant peut toujours être utilisé par d’autres composants.</span><span class="sxs-lookup"><span data-stu-id="6de42-163">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="6de42-164">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="6de42-164">Dependency injection</span></span>

<span data-ttu-id="6de42-165">Les services enregistrés dans le conteneur de services de l’application sont disponibles pour les composants via [l’injection de dépendance (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6de42-165">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6de42-166">Injectez des services dans un composant à l’aide de la directive `@inject`.</span><span class="sxs-lookup"><span data-stu-id="6de42-166">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="6de42-167">Examinez les directives du composant FetchData dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="6de42-167">Examine the directives of the FetchData component in the sample app.</span></span>

<span data-ttu-id="6de42-168">Dans l’exemple d’application Razor Components, le service `WeatherForecastService` est inscrit en tant que [singleton](xref:fundamentals/dependency-injection#service-lifetimes), de sorte qu’une instance du service est disponible dans toute l’application.</span><span class="sxs-lookup"><span data-stu-id="6de42-168">In the Razor Components sample app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span> <span data-ttu-id="6de42-169">La directive `@inject` est utilisée pour injecter l’instance du service `WeatherForecastService` dans le composant.</span><span class="sxs-lookup"><span data-stu-id="6de42-169">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component.</span></span>

<span data-ttu-id="6de42-170">*Components/Pages/FetchData.razor* :</span><span class="sxs-lookup"><span data-stu-id="6de42-170">*Components/Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="6de42-171">Le composant FetchData utilise le service injecté, comme `ForecastService`, pour récupérer un tableau d’objets `WeatherForecast` :</span><span class="sxs-lookup"><span data-stu-id="6de42-171">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="6de42-172">Dans la version Blazor de l’exemple d’application, `HttpClient` est injecté pour obtenir des données de prévisions météorologiques à partir des fichiers *weather.json* du dossier *wwwroot/sample-data* :</span><span class="sxs-lookup"><span data-stu-id="6de42-172">In the Blazor version of the sample app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder:</span></span>

<span data-ttu-id="6de42-173">*Pages/FetchData.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6de42-173">*Pages/FetchData.cshtml*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=7)]

<span data-ttu-id="6de42-174">Dans les deux exemples d’applications, une boucle [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) est utilisée pour restituer chaque instance de prévision sous forme de ligne dans la table des données météorologiques :</span><span class="sxs-lookup"><span data-stu-id="6de42-174">In both sample apps, a [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="6de42-175">Générer une liste de tâches</span><span class="sxs-lookup"><span data-stu-id="6de42-175">Build a todo list</span></span>

<span data-ttu-id="6de42-176">Ajoutez un nouveau composant à l’application qui implémente une liste de tâches simple.</span><span class="sxs-lookup"><span data-stu-id="6de42-176">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="6de42-177">Ajoutez un fichier vide à l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="6de42-177">Add an empty file to the sample app:</span></span>

   * <span data-ttu-id="6de42-178">Pour l’expérience Razor Components, ajoutez un fichier *Todo.razor* au dossier *Components/Pages*.</span><span class="sxs-lookup"><span data-stu-id="6de42-178">For the Razor Components experience, add a *Todo.razor* file to the *Components/Pages* folder.</span></span>
   * <span data-ttu-id="6de42-179">Pour l’expérience Blazor, ajoutez un fichier *Todo.cshtml* au dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="6de42-179">For the Blazor experience, add a *Todo.cshtml* file to the *Pages* folder.</span></span>

1. <span data-ttu-id="6de42-180">Fournissez le balisage initial pour le composant :</span><span class="sxs-lookup"><span data-stu-id="6de42-180">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="6de42-181">Ajoutez le composant Todo à la barre de navigation.</span><span class="sxs-lookup"><span data-stu-id="6de42-181">Add the Todo component to the navigation bar.</span></span>

   <span data-ttu-id="6de42-182">Le composant NavMenu (*Components/Shared/NavMenu.razor* ou *Shared/NavMenu.cshtml* dans Blazor) est utilisé dans la disposition de l’application.</span><span class="sxs-lookup"><span data-stu-id="6de42-182">The NavMenu component (*Components/Shared/NavMenu.razor* or *Shared/NavMenu.cshtml* in Blazor) is used in the app's layout.</span></span> <span data-ttu-id="6de42-183">Les dispositions sont des composants qui vous permettent d’éviter la duplication de contenu dans l’application.</span><span class="sxs-lookup"><span data-stu-id="6de42-183">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="6de42-184">Pour plus d'informations, consultez <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="6de42-184">For more information, see <xref:blazor/layouts>.</span></span>

   <span data-ttu-id="6de42-185">Ajoutez un `<NavLink>` pour le composant Todo en ajoutant le balisage d’élément de liste suivant sous les éléments de liste existants dans le fichier *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* dans Blazor) :</span><span class="sxs-lookup"><span data-stu-id="6de42-185">Add a `<NavLink>` for the Todo component by adding the following list item markup below the existing list items in the *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* in Blazor) file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="6de42-186">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="6de42-186">Rebuild and run the app.</span></span> <span data-ttu-id="6de42-187">Consultez la nouvelle page Todo pour vérifier que le lien vers le composant Todo fonctionne.</span><span class="sxs-lookup"><span data-stu-id="6de42-187">Visit the new Todo page to confirm that the link to the Todo component works.</span></span>

1. <span data-ttu-id="6de42-188">Ajoutez un fichier *TodoItem.cs* à la racine du projet pour contenir une classe qui représente un élément Todo.</span><span class="sxs-lookup"><span data-stu-id="6de42-188">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="6de42-189">Utilisez le code C# suivant pour la classe `TodoItem` :</span><span class="sxs-lookup"><span data-stu-id="6de42-189">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/TodoItem.cs)]

1. <span data-ttu-id="6de42-190">Retournez au composant Todo (*Components/Pages/Todo.razor* ou *Pages/Todo.cshtml* dans Blazor) :</span><span class="sxs-lookup"><span data-stu-id="6de42-190">Return to the Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   * <span data-ttu-id="6de42-191">Ajoutez un champ pour les éléments Todo dans un bloc `@functions`.</span><span class="sxs-lookup"><span data-stu-id="6de42-191">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="6de42-192">Le composant Todo utilise ce champ pour maintenir l’état de la liste de tâches.</span><span class="sxs-lookup"><span data-stu-id="6de42-192">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="6de42-193">Ajoutez un balisage de liste non triée et une boucle `foreach` pour effectuer le rendu de chaque élément todo en tant qu’élément de liste.</span><span class="sxs-lookup"><span data-stu-id="6de42-193">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="6de42-194">L’application nécessite des éléments d’interface utilisateur pour ajouter des éléments todo à la liste.</span><span class="sxs-lookup"><span data-stu-id="6de42-194">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="6de42-195">Ajoutez une entrée de texte et un bouton sous la liste :</span><span class="sxs-lookup"><span data-stu-id="6de42-195">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="6de42-196">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="6de42-196">Rebuild and run the app.</span></span> <span data-ttu-id="6de42-197">Lorsque le bouton **Add todo** est sélectionné, rien ne se produit car aucun gestionnaire d’événements n’est lié au bouton.</span><span class="sxs-lookup"><span data-stu-id="6de42-197">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="6de42-198">Ajoutez une méthode `AddTodo` au composant Todo et enregistrez-la pour les clics sur le bouton à l’aide de l’attribut `onclick` :</span><span class="sxs-lookup"><span data-stu-id="6de42-198">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   <span data-ttu-id="6de42-199">La méthode C# `AddTodo` est appelée lorsque le bouton est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="6de42-199">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="6de42-200">Pour obtenir le titre du nouvel élément todo, ajoutez un champ de chaîne `newTodo` et liez-le à la valeur du texte d’entrée à l’aide de l’attribut `bind` :</span><span class="sxs-lookup"><span data-stu-id="6de42-200">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo">
   ```

1. <span data-ttu-id="6de42-201">Mettez à jour la méthode `AddTodo` pour ajouter `TodoItem` avec le titre spécifié à la liste.</span><span class="sxs-lookup"><span data-stu-id="6de42-201">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="6de42-202">Supprimez la valeur du texte d’entrée en définissant `newTodo` sur une chaîne vide :</span><span class="sxs-lookup"><span data-stu-id="6de42-202">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="6de42-203">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="6de42-203">Rebuild and run the app.</span></span> <span data-ttu-id="6de42-204">Ajoutez quelques éléments todo à la liste Todo pour tester le nouveau code.</span><span class="sxs-lookup"><span data-stu-id="6de42-204">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="6de42-205">Le texte du titre pour chaque élément todo peut être rendu modifiable et une case à cocher peut aider l’utilisateur à effectuer le suivi des éléments terminés.</span><span class="sxs-lookup"><span data-stu-id="6de42-205">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="6de42-206">Ajoutez une entrée de case à cocher pour chaque élément todo et liez sa valeur à la propriété `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="6de42-206">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="6de42-207">Remplacez `@todo.Title` par un élément `<input>` lié à `@todo.Title` :</span><span class="sxs-lookup"><span data-stu-id="6de42-207">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="6de42-208">Pour vérifier que ces valeurs sont liées, mettez à jour l’en-tête `<h1>` pour afficher le nombre d’éléments todo qui ne sont pas terminés (`IsDone` est `false`).</span><span class="sxs-lookup"><span data-stu-id="6de42-208">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="6de42-209">Le composant Todo complet (*Components/Pages/Todo.razor* ou *Pages/Todo.cshtml* dans Blazor) :</span><span class="sxs-lookup"><span data-stu-id="6de42-209">The completed Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/Components/Pages/Todo.razor)]

1. <span data-ttu-id="6de42-210">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="6de42-210">Rebuild and run the app.</span></span> <span data-ttu-id="6de42-211">Ajoutez des éléments todo pour tester le nouveau code.</span><span class="sxs-lookup"><span data-stu-id="6de42-211">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="6de42-212">Publier et déployer l’application</span><span class="sxs-lookup"><span data-stu-id="6de42-212">Publish and deploy the app</span></span>

<span data-ttu-id="6de42-213">Pour publier l’application, consultez <xref:host-and-deploy/blazor/index>.</span><span class="sxs-lookup"><span data-stu-id="6de42-213">To publish the app, see <xref:host-and-deploy/blazor/index>.</span></span>
