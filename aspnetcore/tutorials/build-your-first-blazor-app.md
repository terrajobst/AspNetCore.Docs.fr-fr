---
title: Créer votre première application Blazor
author: guardrex
description: Créez une application Blazor étape par étape.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: c1b142ebdbd85eb10ddf8c8b70edd9782732a4f1
ms.sourcegitcommit: 3ee6ee0051c3d2c8d47a58cb17eef1a84a4c46a0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65621103"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="d6e55-103">Créer votre première application Blazor</span><span class="sxs-lookup"><span data-stu-id="d6e55-103">Build your first Blazor app</span></span>

<span data-ttu-id="d6e55-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d6e55-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d6e55-105">Ce tutoriel vous montre comment créer et modifier une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="d6e55-105">This tutorial shows you how to build and modify a Blazor app.</span></span>

<span data-ttu-id="d6e55-106">Suivez les instructions de l’article <xref:blazor/get-started> afin de créer un projet Blazor pour ce tutoriel.</span><span class="sxs-lookup"><span data-stu-id="d6e55-106">Follow the guidance in the <xref:blazor/get-started> article to create a Blazor project for this tutorial.</span></span>

## <a name="build-components"></a><span data-ttu-id="d6e55-107">Construire des composants</span><span class="sxs-lookup"><span data-stu-id="d6e55-107">Build components</span></span>

1. <span data-ttu-id="d6e55-108">Accédez à chacune des trois pages de l’application dans le dossier *Pages* : Accueil, Compteur et Extraire des données.</span><span class="sxs-lookup"><span data-stu-id="d6e55-108">Browse to each of the app's three pages in the *Pages* folder: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="d6e55-109">Ces pages sont implémentées par les fichiers de composants Razor *Index.razor*, *Counter.razor* et *FetchData.razor*.</span><span class="sxs-lookup"><span data-stu-id="d6e55-109">These pages are implemented by the Razor component files *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span>

1. <span data-ttu-id="d6e55-110">Sur la page Counter, sélectionnez le bouton **Click me** pour incrémenter le compteur sans actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="d6e55-110">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="d6e55-111">L’incrémentation d’un compteur dans une page web nécessite normalement l’écriture de code JavaScript. Toutefois, Blazor fournit une meilleure approche grâce à l’utilisation du langage C#.</span><span class="sxs-lookup"><span data-stu-id="d6e55-111">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

1. <span data-ttu-id="d6e55-112">Examinez l’implémentation du composant Counter dans le fichier *Counter.razor*.</span><span class="sxs-lookup"><span data-stu-id="d6e55-112">Examine the implementation of the Counter component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="d6e55-113">*Pages/Counter.razor* :</span><span class="sxs-lookup"><span data-stu-id="d6e55-113">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="d6e55-114">L’interface utilisateur du composant Counter est définie à l’aide de HTML.</span><span class="sxs-lookup"><span data-stu-id="d6e55-114">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="d6e55-115">La logique de rendu dynamique (par exemple, les boucles, conditions, expressions) est ajoutée à l’aide d’une syntaxe C# intégrée appelée [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="d6e55-115">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="d6e55-116">Le balisage HTML et la logique de rendu C# sont convertis en une classe de composants au moment de la génération.</span><span class="sxs-lookup"><span data-stu-id="d6e55-116">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="d6e55-117">Le nom de la classe .NET générée correspond à celui du fichier.</span><span class="sxs-lookup"><span data-stu-id="d6e55-117">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="d6e55-118">Les membres de la classe de composants sont définis dans un bloc `@functions`.</span><span class="sxs-lookup"><span data-stu-id="d6e55-118">Members of the component class are defined in an `@functions` block.</span></span> <span data-ttu-id="d6e55-119">Dans le bloc `@functions`, l’état du composant (propriétés, champs) et les méthodes sont spécifiés pour la gestion des événements ou pour définir une autre logique de composant.</span><span class="sxs-lookup"><span data-stu-id="d6e55-119">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="d6e55-120">Ces membres sont ensuite utilisés dans le cadre de la logique de rendu du composant et la gestion des événements.</span><span class="sxs-lookup"><span data-stu-id="d6e55-120">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="d6e55-121">Lorsque le bouton **Click me** est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="d6e55-121">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="d6e55-122">Le gestionnaire `onclick` enregistré du composant Counter est appelé (méthode `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="d6e55-122">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="d6e55-123">Le composant Counter regénère son arborescence de rendu.</span><span class="sxs-lookup"><span data-stu-id="d6e55-123">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="d6e55-124">La nouvelle arborescence de rendu est comparée à la précédente.</span><span class="sxs-lookup"><span data-stu-id="d6e55-124">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="d6e55-125">Seules les modifications apportées à Document Object Model (DOM) sont appliquées.</span><span class="sxs-lookup"><span data-stu-id="d6e55-125">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="d6e55-126">Le compte affiché est mis à jour.</span><span class="sxs-lookup"><span data-stu-id="d6e55-126">The displayed count is updated.</span></span>

1. <span data-ttu-id="d6e55-127">Modifiez la logique C# du composant Counter pour que l’incrément du compte passe de un à deux.</span><span class="sxs-lookup"><span data-stu-id="d6e55-127">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="d6e55-128">Régénérez et exécutez l’application pour voir les modifications.</span><span class="sxs-lookup"><span data-stu-id="d6e55-128">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="d6e55-129">Sélectionnez le bouton **Click me**.</span><span class="sxs-lookup"><span data-stu-id="d6e55-129">Select the **Click me** button.</span></span> <span data-ttu-id="d6e55-130">Le compteur est incrémenté de deux.</span><span class="sxs-lookup"><span data-stu-id="d6e55-130">The counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="d6e55-131">Utiliser des composants</span><span class="sxs-lookup"><span data-stu-id="d6e55-131">Use components</span></span>

<span data-ttu-id="d6e55-132">Incluez un composant dans un autre composant utilisant une syntaxe HTML.</span><span class="sxs-lookup"><span data-stu-id="d6e55-132">Include a component in another component using an HTML syntax.</span></span>

1. <span data-ttu-id="d6e55-133">Ajoutez le composant Counter au composant Index de l’application en ajoutant un élément `<Counter />` au composant Index (*Index.razor*).</span><span class="sxs-lookup"><span data-stu-id="d6e55-133">Add the Counter component to the app's Index component by adding a `<Counter />` element to the Index component (*Index.razor*).</span></span>

   <span data-ttu-id="d6e55-134">Si vous utilisez Blazor côté client pour cette expérience, un composant Survey Prompt (élément `<SurveyPrompt>`) se trouve dans le composant Index.</span><span class="sxs-lookup"><span data-stu-id="d6e55-134">If you're using Blazor client-side for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="d6e55-135">Remplacez l’élément `<SurveyPrompt>` par l’élément `<Counter>`.</span><span class="sxs-lookup"><span data-stu-id="d6e55-135">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span> <span data-ttu-id="d6e55-136">Si vous utilisez une application côté serveur Blazor pour cette expérience, ajoutez l’élément `<Counter>` au composant Index :</span><span class="sxs-lookup"><span data-stu-id="d6e55-136">If you're using a Blazor server-side app for this experience, add the `<Counter>` element to the Index component:</span></span>

   <span data-ttu-id="d6e55-137">*Pages/Index.razor* :</span><span class="sxs-lookup"><span data-stu-id="d6e55-137">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. <span data-ttu-id="d6e55-138">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="d6e55-138">Rebuild and run the app.</span></span> <span data-ttu-id="d6e55-139">Le composant Index a son propre compteur.</span><span class="sxs-lookup"><span data-stu-id="d6e55-139">The Index component has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="d6e55-140">Paramètres de composant</span><span class="sxs-lookup"><span data-stu-id="d6e55-140">Component parameters</span></span>

<span data-ttu-id="d6e55-141">Les composants peuvent également avoir des paramètres.</span><span class="sxs-lookup"><span data-stu-id="d6e55-141">Components can also have parameters.</span></span> <span data-ttu-id="d6e55-142">Les paramètres des composants sont définis à l’aide de propriétés non publiques sur la classe de composants décorée avec `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="d6e55-142">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="d6e55-143">Utilisez des attributs pour spécifier des arguments pour un composant dans le balisage.</span><span class="sxs-lookup"><span data-stu-id="d6e55-143">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="d6e55-144">Mettez à jour le code C# `@functions` du composant :</span><span class="sxs-lookup"><span data-stu-id="d6e55-144">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="d6e55-145">Ajoutez une propriété `IncrementAmount` décorée avec l’attribut `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="d6e55-145">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="d6e55-146">Modifiez la méthode `IncrementCount` pour utiliser `IncrementAmount` lorsque vous augmentez la valeur de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="d6e55-146">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="d6e55-147">*Pages/Counter.razor* :</span><span class="sxs-lookup"><span data-stu-id="d6e55-147">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="d6e55-148">Spécifiez un paramètre `IncrementAmount` dans l’élément `<Counter>` du composant Index à l’aide d’un attribut.</span><span class="sxs-lookup"><span data-stu-id="d6e55-148">Specify an `IncrementAmount` parameter in the Index component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="d6e55-149">Définissez la valeur pour incrémenter le compteur par 10.</span><span class="sxs-lookup"><span data-stu-id="d6e55-149">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="d6e55-150">*Pages/Index.razor* :</span><span class="sxs-lookup"><span data-stu-id="d6e55-150">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. <span data-ttu-id="d6e55-151">Recharger le composant Index.</span><span class="sxs-lookup"><span data-stu-id="d6e55-151">Reload the Index component.</span></span> <span data-ttu-id="d6e55-152">Le compteur s’incrémente de dix unités chaque fois que le bouton **Cliquer ici** est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="d6e55-152">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="d6e55-153">Le compteur dans le composant Counter continue à être incrémenté d’un.</span><span class="sxs-lookup"><span data-stu-id="d6e55-153">The counter in the Counter component continues to increment by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="d6e55-154">Acheminer vers les composants</span><span class="sxs-lookup"><span data-stu-id="d6e55-154">Route to components</span></span>

<span data-ttu-id="d6e55-155">La directive `@page` en haut du fichier *Counter.razor* spécifie que le composant Counter est un point de terminaison de routage.</span><span class="sxs-lookup"><span data-stu-id="d6e55-155">The `@page` directive at the top of the *Counter.razor* file specifies that the Counter component is a routing endpoint.</span></span> <span data-ttu-id="d6e55-156">Le composant Counter gère les requêtes envoyées à `/counter`.</span><span class="sxs-lookup"><span data-stu-id="d6e55-156">The Counter component handles requests sent to `/counter`.</span></span> <span data-ttu-id="d6e55-157">Sans la directive `@page`, un composant ne gère pas les requêtes acheminées, mais le composant peut toujours être utilisé par d’autres composants.</span><span class="sxs-lookup"><span data-stu-id="d6e55-157">Without the `@page` directive, a component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="d6e55-158">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="d6e55-158">Dependency injection</span></span>

<span data-ttu-id="d6e55-159">Les services enregistrés dans le conteneur de services de l’application sont disponibles pour les composants via [l’injection de dépendance (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d6e55-159">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d6e55-160">Injectez des services dans un composant à l’aide de la directive `@inject`.</span><span class="sxs-lookup"><span data-stu-id="d6e55-160">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="d6e55-161">Examinez les directives du composant FetchData.</span><span class="sxs-lookup"><span data-stu-id="d6e55-161">Examine the directives of the FetchData component.</span></span>

<span data-ttu-id="d6e55-162">S’il fonctionne avec une application côté serveur Blazor, le service `WeatherForecastService` est inscrit en tant que [singleton](xref:fundamentals/dependency-injection#service-lifetimes), de sorte qu’une instance du service est disponible dans toute l’application.</span><span class="sxs-lookup"><span data-stu-id="d6e55-162">If working with a Blazor server-side app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span> <span data-ttu-id="d6e55-163">La directive `@inject` est utilisée pour injecter l’instance du service `WeatherForecastService` dans le composant.</span><span class="sxs-lookup"><span data-stu-id="d6e55-163">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component.</span></span>

<span data-ttu-id="d6e55-164">*Pages/FetchData.razor* :</span><span class="sxs-lookup"><span data-stu-id="d6e55-164">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="d6e55-165">Le composant FetchData utilise le service injecté, comme `ForecastService`, pour récupérer un tableau d’objets `WeatherForecast` :</span><span class="sxs-lookup"><span data-stu-id="d6e55-165">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="d6e55-166">S’il fonctionne avec une application côté client Blazor, `HttpClient` est injecté pour obtenir des données de prévisions météorologiques à partir des fichiers *weather.json* du dossier *wwwroot/sample-data* :</span><span class="sxs-lookup"><span data-stu-id="d6e55-166">If working with a Blazor client-side app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder:</span></span>

<span data-ttu-id="d6e55-167">*Pages/FetchData.razor* :</span><span class="sxs-lookup"><span data-stu-id="d6e55-167">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

<span data-ttu-id="d6e55-168">Une boucle [\@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) est utilisée pour restituer chaque instance de prévision sous forme de ligne dans la table des données météo :</span><span class="sxs-lookup"><span data-stu-id="d6e55-168">A [\@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]


## <a name="build-a-todo-list"></a><span data-ttu-id="d6e55-169">Générer une liste de tâches</span><span class="sxs-lookup"><span data-stu-id="d6e55-169">Build a todo list</span></span>

<span data-ttu-id="d6e55-170">Ajoutez un nouveau composant à l’application qui implémente une liste de tâches simple.</span><span class="sxs-lookup"><span data-stu-id="d6e55-170">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="d6e55-171">Ajoutez un fichier vide nommé *Todo.razor* à l’application dans le dossier *Pages* :</span><span class="sxs-lookup"><span data-stu-id="d6e55-171">Add an empty file named *Todo.razor* to the app in the *Pages* folder:</span></span>

1. <span data-ttu-id="d6e55-172">Fournissez le balisage initial pour le composant :</span><span class="sxs-lookup"><span data-stu-id="d6e55-172">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="d6e55-173">Ajoutez le composant Todo à la barre de navigation.</span><span class="sxs-lookup"><span data-stu-id="d6e55-173">Add the Todo component to the navigation bar.</span></span>

   <span data-ttu-id="d6e55-174">Le composant NavMenu (*Shared/NavMenu.razor*) est utilisé dans la disposition de l’application.</span><span class="sxs-lookup"><span data-stu-id="d6e55-174">The NavMenu component (*Shared/NavMenu.razor*) is used in the app's layout.</span></span> <span data-ttu-id="d6e55-175">Les dispositions sont des composants qui vous permettent d’éviter la duplication de contenu dans l’application.</span><span class="sxs-lookup"><span data-stu-id="d6e55-175">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="d6e55-176">Pour plus d'informations, consultez <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="d6e55-176">For more information, see <xref:blazor/layouts>.</span></span>

   <span data-ttu-id="d6e55-177">Ajoutez un `<NavLink>` pour le composant Todo en ajoutant le balisage d’élément de liste suivant sous les éléments de liste existants dans le fichier *Shared/NavMenu.razor* :</span><span class="sxs-lookup"><span data-stu-id="d6e55-177">Add a `<NavLink>` for the Todo component by adding the following list item markup below the existing list items in the *Shared/NavMenu.razor* file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="d6e55-178">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="d6e55-178">Rebuild and run the app.</span></span> <span data-ttu-id="d6e55-179">Consultez la nouvelle page Todo pour vérifier que le lien vers le composant Todo fonctionne.</span><span class="sxs-lookup"><span data-stu-id="d6e55-179">Visit the new Todo page to confirm that the link to the Todo component works.</span></span>

1. <span data-ttu-id="d6e55-180">Si vous créez une application Blazor côté serveur, ajoutez l’espace de noms de l’application au fichier *\_Imports.razor*.</span><span class="sxs-lookup"><span data-stu-id="d6e55-180">If building a Blazor server-side app, add the app's namespace to the *\_Imports.razor* file.</span></span> <span data-ttu-id="d6e55-181">L’énoncé `@using` suivant suppose que l’espace de noms de l’application est `WebApplication` :</span><span class="sxs-lookup"><span data-stu-id="d6e55-181">The following `@using` statement assumes that the app's namespace is `WebApplication`:</span></span>

   ```cshtml
   @using WebApplication
   ```
   
   <span data-ttu-id="d6e55-182">Les applications Blazor côté client incluent l’espace de noms de l’application par défaut dans le fichier *\_Imports.razor*.</span><span class="sxs-lookup"><span data-stu-id="d6e55-182">Blazor client-side apps include the app's namespace by default in the *\_Imports.razor* file.</span></span>

1. <span data-ttu-id="d6e55-183">Ajoutez un fichier *TodoItem.cs* à la racine du projet pour contenir une classe qui représente un élément Todo.</span><span class="sxs-lookup"><span data-stu-id="d6e55-183">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="d6e55-184">Utilisez le code C# suivant pour la classe `TodoItem` :</span><span class="sxs-lookup"><span data-stu-id="d6e55-184">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="d6e55-185">Revenez au composant Todo (*Pages/Todo.razorl*) :</span><span class="sxs-lookup"><span data-stu-id="d6e55-185">Return to the Todo component (*Pages/Todo.razor*):</span></span>

   * <span data-ttu-id="d6e55-186">Ajoutez un champ pour les éléments Todo dans un bloc `@functions`.</span><span class="sxs-lookup"><span data-stu-id="d6e55-186">Add a field for the todo items in an `@functions` block.</span></span> <span data-ttu-id="d6e55-187">Le composant Todo utilise ce champ pour maintenir l’état de la liste de tâches.</span><span class="sxs-lookup"><span data-stu-id="d6e55-187">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="d6e55-188">Ajoutez un balisage de liste non triée et une boucle `foreach` pour effectuer le rendu de chaque élément todo en tant qu’élément de liste.</span><span class="sxs-lookup"><span data-stu-id="d6e55-188">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="d6e55-189">L’application nécessite des éléments d’interface utilisateur pour ajouter des éléments todo à la liste.</span><span class="sxs-lookup"><span data-stu-id="d6e55-189">The app requires UI elements for adding todo items to the list.</span></span> <span data-ttu-id="d6e55-190">Ajoutez une entrée de texte et un bouton sous la liste :</span><span class="sxs-lookup"><span data-stu-id="d6e55-190">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="d6e55-191">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="d6e55-191">Rebuild and run the app.</span></span> <span data-ttu-id="d6e55-192">Lorsque le bouton **Add todo** est sélectionné, rien ne se produit car aucun gestionnaire d’événements n’est lié au bouton.</span><span class="sxs-lookup"><span data-stu-id="d6e55-192">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="d6e55-193">Ajoutez une méthode `AddTodo` au composant Todo et enregistrez-la pour les clics sur le bouton à l’aide de l’attribut `onclick` :</span><span class="sxs-lookup"><span data-stu-id="d6e55-193">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   <span data-ttu-id="d6e55-194">La méthode C# `AddTodo` est appelée lorsque le bouton est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="d6e55-194">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="d6e55-195">Pour obtenir le titre du nouvel élément todo, ajoutez un champ de chaîne `newTodo` et liez-le à la valeur du texte d’entrée à l’aide de l’attribut `bind` :</span><span class="sxs-lookup"><span data-stu-id="d6e55-195">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo">
   ```

1. <span data-ttu-id="d6e55-196">Mettez à jour la méthode `AddTodo` pour ajouter `TodoItem` avec le titre spécifié à la liste.</span><span class="sxs-lookup"><span data-stu-id="d6e55-196">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="d6e55-197">Supprimez la valeur du texte d’entrée en définissant `newTodo` sur une chaîne vide :</span><span class="sxs-lookup"><span data-stu-id="d6e55-197">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="d6e55-198">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="d6e55-198">Rebuild and run the app.</span></span> <span data-ttu-id="d6e55-199">Ajoutez quelques éléments todo à la liste Todo pour tester le nouveau code.</span><span class="sxs-lookup"><span data-stu-id="d6e55-199">Add some todo items to the todo list to test the new code.</span></span>

1. <span data-ttu-id="d6e55-200">Le texte du titre pour chaque élément todo peut être rendu modifiable et une case à cocher peut aider l’utilisateur à effectuer le suivi des éléments terminés.</span><span class="sxs-lookup"><span data-stu-id="d6e55-200">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="d6e55-201">Ajoutez une entrée de case à cocher pour chaque élément todo et liez sa valeur à la propriété `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="d6e55-201">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="d6e55-202">Remplacez `@todo.Title` par un élément `<input>` lié à `@todo.Title` :</span><span class="sxs-lookup"><span data-stu-id="d6e55-202">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="d6e55-203">Pour vérifier que ces valeurs sont liées, mettez à jour l’en-tête `<h1>` pour afficher le nombre d’éléments todo qui ne sont pas terminés (`IsDone` est `false`).</span><span class="sxs-lookup"><span data-stu-id="d6e55-203">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="d6e55-204">Le composant Todo (*Pages/Todo.razor*) terminé :</span><span class="sxs-lookup"><span data-stu-id="d6e55-204">The completed Todo component (*Pages/Todo.razor*):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="d6e55-205">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="d6e55-205">Rebuild and run the app.</span></span> <span data-ttu-id="d6e55-206">Ajoutez des éléments todo pour tester le nouveau code.</span><span class="sxs-lookup"><span data-stu-id="d6e55-206">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="d6e55-207">Publier et déployer l’application</span><span class="sxs-lookup"><span data-stu-id="d6e55-207">Publish and deploy the app</span></span>

<span data-ttu-id="d6e55-208">Pour publier l’application, consultez <xref:host-and-deploy/blazor/index>.</span><span class="sxs-lookup"><span data-stu-id="d6e55-208">To publish the app, see <xref:host-and-deploy/blazor/index>.</span></span>
