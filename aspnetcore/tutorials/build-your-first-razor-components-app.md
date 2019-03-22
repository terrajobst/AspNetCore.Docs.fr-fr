---
title: Créer votre première application Composants Razor
author: guardrex
description: Créez une application Composants Razor pas à pas et découvrez les concepts de base des Composants Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/14/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: c0f7b27fdfc770f8001625ecb3bf8d50af517b99
ms.sourcegitcommit: 10e14b85490f064395e9b2f423d21e3c2d39ed8b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2019
ms.locfileid: "57978421"
---
# <a name="build-your-first-razor-components-app"></a><span data-ttu-id="b81cb-103">Créer votre première application Composants Razor</span><span class="sxs-lookup"><span data-stu-id="b81cb-103">Build your first Razor Components app</span></span>

<span data-ttu-id="b81cb-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b81cb-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="b81cb-105">Ce tutoriel vous montre comment créer une application avec les Composants Razor et illustre les concepts de base des Composants Razor.</span><span class="sxs-lookup"><span data-stu-id="b81cb-105">This tutorial shows you how to build an app with Razor Components and demonstrates basic Razor Components concepts.</span></span> <span data-ttu-id="b81cb-106">Vous pouvez profiter de ce tutoriel en utilisant soit un projet basé sur les Composants Razor (pris en charge dans .NET Core 3.0 ou version ultérieure) ou un projet Blazor (pris en charge dans une prochaine version de .NET Core).</span><span class="sxs-lookup"><span data-stu-id="b81cb-106">You can enjoy this tutorial using either a Razor Components-based project (supported in .NET Core 3.0 or later) or using a Blazor-based project (supported in a future release of .NET Core).</span></span>

<span data-ttu-id="b81cb-107">Pour une expérience à l’aide de Composants Razor ASP.NET Core (*recommandé*) :</span><span class="sxs-lookup"><span data-stu-id="b81cb-107">For an experience using ASP.NET Core Razor Components (*recommended*):</span></span>

* <span data-ttu-id="b81cb-108">Suivez les instructions dans <xref:razor-components/get-started> pour créer un projet basé sur les Composants Razor.</span><span class="sxs-lookup"><span data-stu-id="b81cb-108">Follow the guidance in <xref:razor-components/get-started> to create a Razor Components-based project.</span></span>
* <span data-ttu-id="b81cb-109">Attribuez un nom au projet `RazorComponents`.</span><span class="sxs-lookup"><span data-stu-id="b81cb-109">Name the project `RazorComponents`.</span></span>

<span data-ttu-id="b81cb-110">Pour une expérience à l’aide de Blazor :</span><span class="sxs-lookup"><span data-stu-id="b81cb-110">For an experience using Blazor:</span></span>

* <span data-ttu-id="b81cb-111">Suivez les instructions dans <xref:spa/blazor/get-started> pour créer un projet basé sur Blazor.</span><span class="sxs-lookup"><span data-stu-id="b81cb-111">Follow the guidance in <xref:spa/blazor/get-started> to create a Blazor-based project.</span></span>
* <span data-ttu-id="b81cb-112">Attribuez un nom au projet `Blazor`.</span><span class="sxs-lookup"><span data-stu-id="b81cb-112">Name the project `Blazor`.</span></span>

<span data-ttu-id="b81cb-113">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="b81cb-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="b81cb-114">Pour connaître les prérequis, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="b81cb-114">See the following topics for prerequisites:</span></span>

## <a name="build-components"></a><span data-ttu-id="b81cb-115">Construire des composants</span><span class="sxs-lookup"><span data-stu-id="b81cb-115">Build components</span></span>

1. <span data-ttu-id="b81cb-116">Accédez à chacune des trois pages de l’application dans le dossier *Components/Pages* (*Pages* dans Blazor) : Accueil, Compteur et Extraire des données.</span><span class="sxs-lookup"><span data-stu-id="b81cb-116">Browse to each of the app's three pages in the *Components/Pages* folder (*Pages* in Blazor): Home, Counter, and Fetch data.</span></span> <span data-ttu-id="b81cb-117">Ces pages sont implémentées par les fichiers du composant Razor : *Index.razor*, *Counter.razor* et *FetchData.razor*.</span><span class="sxs-lookup"><span data-stu-id="b81cb-117">These pages are implemented by Razor Component files: *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span> <span data-ttu-id="b81cb-118">(Blazor continue d’utiliser l’extension de fichier *.cshtml* : *Index.cshtml*, *Counter.cshtml* et *FetchData.cshtml*.)</span><span class="sxs-lookup"><span data-stu-id="b81cb-118">(Blazor continues to use the *.cshtml* file extension: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.)</span></span>

1. <span data-ttu-id="b81cb-119">Sur la page Counter, sélectionnez le bouton **Click me** pour incrémenter le compteur sans actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="b81cb-119">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="b81cb-120">L’incrémentation d’un compteur dans une page web requiert normalement l’écriture JavaScript, mais le projet Composants Razor fournit une meilleure approche à l’aide C#.</span><span class="sxs-lookup"><span data-stu-id="b81cb-120">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

1. <span data-ttu-id="b81cb-121">Examinez l’implémentation du composant Counter dans le fichier *Counter.razor*.</span><span class="sxs-lookup"><span data-stu-id="b81cb-121">Examine the implementation of the Counter component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="b81cb-122">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* dans Blazor) :</span><span class="sxs-lookup"><span data-stu-id="b81cb-122">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="b81cb-123">L’interface utilisateur du composant Counter est définie à l’aide de HTML.</span><span class="sxs-lookup"><span data-stu-id="b81cb-123">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="b81cb-124">La logique de rendu dynamique (par exemple, les boucles, conditions, expressions) est ajoutée à l’aide d’une syntaxe C# intégrée appelée [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="b81cb-124">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="b81cb-125">Le balisage HTML et la logique de rendu C# sont convertis en une classe de composants au moment de la génération.</span><span class="sxs-lookup"><span data-stu-id="b81cb-125">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="b81cb-126">Le nom de la classe .NET générée correspond à celui du fichier.</span><span class="sxs-lookup"><span data-stu-id="b81cb-126">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="b81cb-127">Les membres de la classe de composants sont définis dans un bloc `@functions`.</span><span class="sxs-lookup"><span data-stu-id="b81cb-127">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="b81cb-128">Dans le bloc `@functions`, l’état du composant (propriétés, champs) et les méthodes sont spécifiés pour la gestion des événements ou pour définir une autre logique de composant.</span><span class="sxs-lookup"><span data-stu-id="b81cb-128">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="b81cb-129">Ces membres sont ensuite utilisés dans le cadre de la logique de rendu du composant et la gestion des événements.</span><span class="sxs-lookup"><span data-stu-id="b81cb-129">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="b81cb-130">Lorsque le bouton **Click me** est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="b81cb-130">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="b81cb-131">Le gestionnaire `onclick` enregistré du composant Counter est appelé (méthode `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="b81cb-131">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="b81cb-132">Le composant Counter regénère son arborescence de rendu.</span><span class="sxs-lookup"><span data-stu-id="b81cb-132">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="b81cb-133">La nouvelle arborescence de rendu est comparée à la précédente.</span><span class="sxs-lookup"><span data-stu-id="b81cb-133">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="b81cb-134">Seules les modifications apportées à Document Object Model (DOM) sont appliquées.</span><span class="sxs-lookup"><span data-stu-id="b81cb-134">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="b81cb-135">Le compte affiché est mis à jour.</span><span class="sxs-lookup"><span data-stu-id="b81cb-135">The displayed count is updated.</span></span>

1. <span data-ttu-id="b81cb-136">Modifiez la logique C# du composant Counter pour que l’incrément du compte passe de un à deux.</span><span class="sxs-lookup"><span data-stu-id="b81cb-136">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="b81cb-137">Régénérez et exécutez l’application pour voir les modifications.</span><span class="sxs-lookup"><span data-stu-id="b81cb-137">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="b81cb-138">Sélectionnez le bouton **Click me** et le compteur passe à deux incréments.</span><span class="sxs-lookup"><span data-stu-id="b81cb-138">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="b81cb-139">Utiliser des composants</span><span class="sxs-lookup"><span data-stu-id="b81cb-139">Use components</span></span>

<span data-ttu-id="b81cb-140">Incluez un composant dans un autre composant à l’aide d’une syntaxe de type HTML.</span><span class="sxs-lookup"><span data-stu-id="b81cb-140">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="b81cb-141">Ajoutez le composant Counter au composant Index (page d’accueil) de l’application en ajoutant un élément `<Counter />` au composant Index.</span><span class="sxs-lookup"><span data-stu-id="b81cb-141">Add the Counter component to the app's Index (home page) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="b81cb-142">Si vous utilisez Blazor pour cette expérience, un composant Survey Prompt (élément `<SurveyPrompt>`) se trouve dans le composant Index.</span><span class="sxs-lookup"><span data-stu-id="b81cb-142">If you're using Blazor for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="b81cb-143">Remplacez l’élément `<SurveyPrompt>` par l’élément `<Counter>`.</span><span class="sxs-lookup"><span data-stu-id="b81cb-143">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span>

   <span data-ttu-id="b81cb-144">*Components/Pages/Index.razor* (*Pages/Index.cshtml* dans Blazor) :</span><span class="sxs-lookup"><span data-stu-id="b81cb-144">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.razor?highlight=7)]

1. <span data-ttu-id="b81cb-145">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="b81cb-145">Rebuild and run the app.</span></span> <span data-ttu-id="b81cb-146">La page d’accueil a son propre compteur.</span><span class="sxs-lookup"><span data-stu-id="b81cb-146">The home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="b81cb-147">Paramètres de composant</span><span class="sxs-lookup"><span data-stu-id="b81cb-147">Component parameters</span></span>

<span data-ttu-id="b81cb-148">Les composants peuvent également avoir des paramètres.</span><span class="sxs-lookup"><span data-stu-id="b81cb-148">Components can also have parameters.</span></span> <span data-ttu-id="b81cb-149">Les paramètres des composants sont définis à l’aide de propriétés non publiques sur la classe de composants décorée avec `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="b81cb-149">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="b81cb-150">Utilisez des attributs pour spécifier des arguments pour un composant dans le balisage.</span><span class="sxs-lookup"><span data-stu-id="b81cb-150">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="b81cb-151">Mettez à jour le code C# `@functions` du composant :</span><span class="sxs-lookup"><span data-stu-id="b81cb-151">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="b81cb-152">Ajoutez une propriété `IncrementAmount` décorée avec l’attribut `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="b81cb-152">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="b81cb-153">Modifiez la méthode `IncrementCount` pour utiliser `IncrementAmount` lorsque vous augmentez la valeur de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="b81cb-153">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="b81cb-154">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* dans Blazor) :</span><span class="sxs-lookup"><span data-stu-id="b81cb-154">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Counter.razor?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="b81cb-155">Spécifiez un paramètre `IncrementAmount` dans l’élément `<Counter>` du composant Home à l’aide d’un attribut.</span><span class="sxs-lookup"><span data-stu-id="b81cb-155">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="b81cb-156">Définissez la valeur pour incrémenter le compteur par 10.</span><span class="sxs-lookup"><span data-stu-id="b81cb-156">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="b81cb-157">*Components/Pages/Index.razor* (*Pages/Index.cshtml* dans Blazor) :</span><span class="sxs-lookup"><span data-stu-id="b81cb-157">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Index.razor?highlight=7)]

1. <span data-ttu-id="b81cb-158">Rechargez la page.</span><span class="sxs-lookup"><span data-stu-id="b81cb-158">Reload the page.</span></span> <span data-ttu-id="b81cb-159">Le compteur de la page d’accueil s’incrémente de dix unités à chaque fois que le bouton **Click me** est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="b81cb-159">The home page counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="b81cb-160">Le compteur sur la page *Counter* s’incrémente d’une unité.</span><span class="sxs-lookup"><span data-stu-id="b81cb-160">The counter on the *Counter* page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="b81cb-161">Acheminer vers les composants</span><span class="sxs-lookup"><span data-stu-id="b81cb-161">Route to components</span></span>

<span data-ttu-id="b81cb-162">La directive `@page` en haut du fichier *Counter.razor* spécifie que ce composant est un point de terminaison de routage.</span><span class="sxs-lookup"><span data-stu-id="b81cb-162">The `@page` directive at the top of the *Counter.razor* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="b81cb-163">Le composant Counter gère les requêtes envoyées à `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="b81cb-163">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="b81cb-164">Sans la directive `@page`, le composant ne gère pas les requêtes acheminées, mais le composant peut toujours être utilisé par d’autres composants.</span><span class="sxs-lookup"><span data-stu-id="b81cb-164">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="b81cb-165">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="b81cb-165">Dependency injection</span></span>

<span data-ttu-id="b81cb-166">Les services enregistrés dans le conteneur de services de l’application sont disponibles pour les composants via [l’injection de dépendance (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b81cb-166">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="b81cb-167">Injectez des services dans un composant à l’aide de la directive `@inject`.</span><span class="sxs-lookup"><span data-stu-id="b81cb-167">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="b81cb-168">Examinez les directives du composant FetchData.</span><span class="sxs-lookup"><span data-stu-id="b81cb-168">Examine the directives of the FetchData component.</span></span> <span data-ttu-id="b81cb-169">La directive `@inject` est utilisée pour injecter l’instance du service `WeatherForecastService` dans le composant :</span><span class="sxs-lookup"><span data-stu-id="b81cb-169">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component:</span></span>

<span data-ttu-id="b81cb-170">*Components/Pages/FetchData.razor* (*Pages/FetchData.cshtml* dans Blazor) :</span><span class="sxs-lookup"><span data-stu-id="b81cb-170">*Components/Pages/FetchData.razor* (*Pages/FetchData.cshtml* in Blazor):</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="b81cb-171">Le service `WeatherForecastService` est inscrit en tant que [singleton](xref:fundamentals/dependency-injection#service-lifetimes), de sorte qu’une instance du service est disponible dans toute l’application.</span><span class="sxs-lookup"><span data-stu-id="b81cb-171">The `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span>

<span data-ttu-id="b81cb-172">Le composant FetchData utilise le service injecté, comme `ForecastService`, pour récupérer un tableau d’objets `WeatherForecast` :</span><span class="sxs-lookup"><span data-stu-id="b81cb-172">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="b81cb-173">Une boucle [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) est utilisée pour restituer chaque instance de prévision sous forme de ligne dans la table sur la météo :</span><span class="sxs-lookup"><span data-stu-id="b81cb-173">A [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="b81cb-174">Générer une liste de tâches</span><span class="sxs-lookup"><span data-stu-id="b81cb-174">Build a todo list</span></span>

<span data-ttu-id="b81cb-175">Ajoutez une nouvelle page à l’application qui implémente une liste de tâches simple.</span><span class="sxs-lookup"><span data-stu-id="b81cb-175">Add a new page to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="b81cb-176">Ajoutez un fichier vide au dossier *Components/Pages* (dossier *Pages* dans Blazor) nommé *Todo.razor*.</span><span class="sxs-lookup"><span data-stu-id="b81cb-176">Add an empty file to the *Components/Pages* folder (*Pages* folder in Blazor) named *Todo.razor*.</span></span>

1. <span data-ttu-id="b81cb-177">Fournissez le balisage initial pour la page :</span><span class="sxs-lookup"><span data-stu-id="b81cb-177">Provide the initial markup for the page:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="b81cb-178">Ajoutez la page Todo à la barre de navigation.</span><span class="sxs-lookup"><span data-stu-id="b81cb-178">Add the Todo page to the navigation bar.</span></span>

   <span data-ttu-id="b81cb-179">Le composant NavMenu (*Components/Shared/NavMenu.razor* ou *Shared/NavMenu.cshtml* dans Blazor) est utilisé dans la disposition de l’application.</span><span class="sxs-lookup"><span data-stu-id="b81cb-179">The NavMenu component (*Components/Shared/NavMenu.razor* or *Shared/NavMenu.cshtml* in Blazor) is used in the app's layout.</span></span> <span data-ttu-id="b81cb-180">Les dispositions sont des composants qui vous permettent d’éviter la duplication de contenu dans l’application.</span><span class="sxs-lookup"><span data-stu-id="b81cb-180">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="b81cb-181">Pour plus d'informations, consultez <xref:razor-components/layouts>.</span><span class="sxs-lookup"><span data-stu-id="b81cb-181">For more information, see <xref:razor-components/layouts>.</span></span>

   <span data-ttu-id="b81cb-182">Ajoutez un `<NavLink>` pour la page Todo en ajoutant le balisage d’élément de liste suivant sous les éléments de liste existants dans le fichier *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* dans Blazor) :</span><span class="sxs-lookup"><span data-stu-id="b81cb-182">Add a `<NavLink>` for the Todo page by adding the following list item markup below the existing list items in the *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* in Blazor) file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="b81cb-183">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="b81cb-183">Rebuild and run the app.</span></span> <span data-ttu-id="b81cb-184">Consultez la nouvelle page Todo pour confirmer que le lien vers la page Todo fonctionne.</span><span class="sxs-lookup"><span data-stu-id="b81cb-184">Visit the new Todo page to confirm that the link to the Todo page works.</span></span>

1. <span data-ttu-id="b81cb-185">Ajoutez un fichier *TodoItem.cs* à la racine du projet pour contenir une classe qui représente un élément Todo.</span><span class="sxs-lookup"><span data-stu-id="b81cb-185">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="b81cb-186">Utilisez le code C# suivant pour la classe `TodoItem` :</span><span class="sxs-lookup"><span data-stu-id="b81cb-186">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/TodoItem.cs)]

1. <span data-ttu-id="b81cb-187">Retournez au composant Todo (*Components/Pages/Todo.razor* ou *Pages/Todo.cshtml* dans Blazor) :</span><span class="sxs-lookup"><span data-stu-id="b81cb-187">Return to the Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   * <span data-ttu-id="b81cb-188">Ajoutez un champ pour les éléments Todo dans un bloc `@functions`.</span><span class="sxs-lookup"><span data-stu-id="b81cb-188">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="b81cb-189">Le composant Todo utilise ce champ pour maintenir l’état de la liste de tâches.</span><span class="sxs-lookup"><span data-stu-id="b81cb-189">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="b81cb-190">Ajoutez un balisage de liste non triée et une boucle `foreach` pour effectuer le rendu de chaque élément todo en tant qu’élément de liste.</span><span class="sxs-lookup"><span data-stu-id="b81cb-190">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="b81cb-191">L’application nécessite des éléments d’interface utilisateur pour ajouter des éléments todo à la liste.</span><span class="sxs-lookup"><span data-stu-id="b81cb-191">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="b81cb-192">Ajoutez une entrée de texte et un bouton sous la liste :</span><span class="sxs-lookup"><span data-stu-id="b81cb-192">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="b81cb-193">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="b81cb-193">Rebuild and run the app.</span></span> <span data-ttu-id="b81cb-194">Lorsque le bouton **Add todo** est sélectionné, rien ne se produit car aucun gestionnaire d’événements n’est lié au bouton.</span><span class="sxs-lookup"><span data-stu-id="b81cb-194">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="b81cb-195">Ajoutez une méthode `AddTodo` au composant Todo et enregistrez-la pour les clics sur le bouton à l’aide de l’attribut `onclick` :</span><span class="sxs-lookup"><span data-stu-id="b81cb-195">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   <span data-ttu-id="b81cb-196">La méthode C# `AddTodo` est appelée lorsque le bouton est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="b81cb-196">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="b81cb-197">Pour obtenir le titre du nouvel élément todo, ajoutez un champ de chaîne `newTodo` et liez-le à la valeur du texte d’entrée à l’aide de l’attribut `bind` :</span><span class="sxs-lookup"><span data-stu-id="b81cb-197">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. <span data-ttu-id="b81cb-198">Mettez à jour la méthode `AddTodo` pour ajouter `TodoItem` avec le titre spécifié à la liste.</span><span class="sxs-lookup"><span data-stu-id="b81cb-198">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="b81cb-199">Supprimez la valeur du texte d’entrée en définissant `newTodo` sur une chaîne vide :</span><span class="sxs-lookup"><span data-stu-id="b81cb-199">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="b81cb-200">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="b81cb-200">Rebuild and run the app.</span></span> <span data-ttu-id="b81cb-201">Ajoutez quelques éléments todo à la liste Todo pour tester le nouveau code.</span><span class="sxs-lookup"><span data-stu-id="b81cb-201">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="b81cb-202">Le texte du titre pour chaque élément todo peut être rendu modifiable et une case à cocher peut aider l’utilisateur à effectuer le suivi des éléments terminés.</span><span class="sxs-lookup"><span data-stu-id="b81cb-202">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="b81cb-203">Ajoutez une entrée de case à cocher pour chaque élément todo et liez sa valeur à la propriété `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="b81cb-203">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="b81cb-204">Remplacez `@todo.Title` par un élément `<input>` lié à `@todo.Title` :</span><span class="sxs-lookup"><span data-stu-id="b81cb-204">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="b81cb-205">Pour vérifier que ces valeurs sont liées, mettez à jour l’en-tête `<h1>` pour afficher le nombre d’éléments todo qui ne sont pas terminés (`IsDone` est `false`).</span><span class="sxs-lookup"><span data-stu-id="b81cb-205">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="b81cb-206">Le composant Todo complet (*Components/Pages/Todo.razor* ou *Pages/Todo.cshtml* dans Blazor) :</span><span class="sxs-lookup"><span data-stu-id="b81cb-206">The completed Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Todo.razor)]

1. <span data-ttu-id="b81cb-207">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="b81cb-207">Rebuild and run the app.</span></span> <span data-ttu-id="b81cb-208">Ajoutez des éléments todo pour tester le nouveau code.</span><span class="sxs-lookup"><span data-stu-id="b81cb-208">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="b81cb-209">Publier et déployer l’application</span><span class="sxs-lookup"><span data-stu-id="b81cb-209">Publish and deploy the app</span></span>

<span data-ttu-id="b81cb-210">Pour publier l’application, consultez <xref:host-and-deploy/razor-components/index#publish-the-app>.</span><span class="sxs-lookup"><span data-stu-id="b81cb-210">To publish the app, see <xref:host-and-deploy/razor-components/index#publish-the-app>.</span></span>
