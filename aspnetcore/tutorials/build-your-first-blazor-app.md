---
title: Créer votre première application Blazor
author: guardrex
description: Créez une application Blazor étape par étape.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/23/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 2f0410eb75a6ef3f3d361c75bce7e79ae6d42a61
ms.sourcegitcommit: 983b31449fe398e6e922eb13e9eb6f4287ec91e8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2019
ms.locfileid: "70017503"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="c3eee-103">Créer votre première application Blazor</span><span class="sxs-lookup"><span data-stu-id="c3eee-103">Build your first Blazor app</span></span>

<span data-ttu-id="c3eee-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c3eee-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c3eee-105">Ce tutoriel vous montre comment créer et modifier une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="c3eee-105">This tutorial shows you how to build and modify a Blazor app.</span></span>

<span data-ttu-id="c3eee-106">Suivez les instructions de l’article <xref:blazor/get-started> afin de créer un projet Blazor pour ce tutoriel.</span><span class="sxs-lookup"><span data-stu-id="c3eee-106">Follow the guidance in the <xref:blazor/get-started> article to create a Blazor project for this tutorial.</span></span> <span data-ttu-id="c3eee-107">Nommez le projet *ToDoList*.</span><span class="sxs-lookup"><span data-stu-id="c3eee-107">Name the project *ToDoList*.</span></span>

## <a name="build-components"></a><span data-ttu-id="c3eee-108">Construire des composants</span><span class="sxs-lookup"><span data-stu-id="c3eee-108">Build components</span></span>

1. <span data-ttu-id="c3eee-109">Accédez à chacune des trois pages de l’application dans le dossier *Pages* : Accueil, Compteur et Extraire des données.</span><span class="sxs-lookup"><span data-stu-id="c3eee-109">Browse to each of the app's three pages in the *Pages* folder: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="c3eee-110">Ces pages sont implémentées par les fichiers de composants Razor *Index.razor*, *Counter.razor* et *FetchData.razor*.</span><span class="sxs-lookup"><span data-stu-id="c3eee-110">These pages are implemented by the Razor component files *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span>

1. <span data-ttu-id="c3eee-111">Sur la page Counter, sélectionnez le bouton **Click me** pour incrémenter le compteur sans actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="c3eee-111">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="c3eee-112">L’incrémentation d’un compteur dans une page web nécessite normalement l’écriture de code JavaScript. Toutefois, Blazor fournit une meilleure approche grâce à l’utilisation du langage C#.</span><span class="sxs-lookup"><span data-stu-id="c3eee-112">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

1. <span data-ttu-id="c3eee-113">Examinez l’implémentation du composant `Counter` dans le fichier *Counter.razor*.</span><span class="sxs-lookup"><span data-stu-id="c3eee-113">Examine the implementation of the `Counter` component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="c3eee-114">*Pages/Counter.razor* :</span><span class="sxs-lookup"><span data-stu-id="c3eee-114">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="c3eee-115">L’interface utilisateur du composant `Counter` est définie à l’aide de HTML.</span><span class="sxs-lookup"><span data-stu-id="c3eee-115">The UI of the `Counter` component is defined using HTML.</span></span> <span data-ttu-id="c3eee-116">La logique de rendu dynamique (par exemple, les boucles, conditions, expressions) est ajoutée à l’aide d’une syntaxe C# intégrée appelée [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="c3eee-116">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="c3eee-117">Le balisage HTML et la logique de rendu C# sont convertis en une classe de composants au moment de la génération.</span><span class="sxs-lookup"><span data-stu-id="c3eee-117">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="c3eee-118">Le nom de la classe .NET générée correspond à celui du fichier.</span><span class="sxs-lookup"><span data-stu-id="c3eee-118">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="c3eee-119">Les membres de la classe de composants sont définis dans un bloc `@code`.</span><span class="sxs-lookup"><span data-stu-id="c3eee-119">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="c3eee-120">Dans le bloc `@code`, l’état du composant (propriétés, champs) et les méthodes sont spécifiés pour la gestion des événements ou pour définir une autre logique de composant.</span><span class="sxs-lookup"><span data-stu-id="c3eee-120">In the `@code` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="c3eee-121">Ces membres sont ensuite utilisés dans le cadre de la logique de rendu du composant et la gestion des événements.</span><span class="sxs-lookup"><span data-stu-id="c3eee-121">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="c3eee-122">Lorsque le bouton **Click me** est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="c3eee-122">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="c3eee-123">Le gestionnaire `onclick` enregistré du composant `Counter` est appelé (méthode `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="c3eee-123">The `Counter` component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="c3eee-124">Le composant `Counter` regénère son arborescence de rendu.</span><span class="sxs-lookup"><span data-stu-id="c3eee-124">The `Counter` component regenerates its render tree.</span></span>
   * <span data-ttu-id="c3eee-125">La nouvelle arborescence de rendu est comparée à la précédente.</span><span class="sxs-lookup"><span data-stu-id="c3eee-125">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="c3eee-126">Seules les modifications apportées à Document Object Model (DOM) sont appliquées.</span><span class="sxs-lookup"><span data-stu-id="c3eee-126">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="c3eee-127">Le compte affiché est mis à jour.</span><span class="sxs-lookup"><span data-stu-id="c3eee-127">The displayed count is updated.</span></span>

1. <span data-ttu-id="c3eee-128">Modifiez la logique C# du composant `Counter` pour que l’incrément du compte passe de un à deux.</span><span class="sxs-lookup"><span data-stu-id="c3eee-128">Modify the C# logic of the `Counter` component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="c3eee-129">Régénérez et exécutez l’application pour voir les modifications.</span><span class="sxs-lookup"><span data-stu-id="c3eee-129">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="c3eee-130">Sélectionnez le bouton **Click me**.</span><span class="sxs-lookup"><span data-stu-id="c3eee-130">Select the **Click me** button.</span></span> <span data-ttu-id="c3eee-131">Le compteur est incrémenté de deux.</span><span class="sxs-lookup"><span data-stu-id="c3eee-131">The counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="c3eee-132">Utiliser des composants</span><span class="sxs-lookup"><span data-stu-id="c3eee-132">Use components</span></span>

<span data-ttu-id="c3eee-133">Incluez un composant dans un autre composant utilisant une syntaxe HTML.</span><span class="sxs-lookup"><span data-stu-id="c3eee-133">Include a component in another component using an HTML syntax.</span></span>

1. <span data-ttu-id="c3eee-134">Ajoutez le composant `Counter` au composant `Index` de l’application en ajoutant un élément `<Counter />` au composant `Index` (*Index.razor*).</span><span class="sxs-lookup"><span data-stu-id="c3eee-134">Add the `Counter` component to the app's `Index` component by adding a `<Counter />` element to the `Index` component (*Index.razor*).</span></span>

   <span data-ttu-id="c3eee-135">Si vous utilisez Blazor côté client pour cette expérience, un composant `SurveyPrompt` est utilisé dans le composant `Index`.</span><span class="sxs-lookup"><span data-stu-id="c3eee-135">If you're using Blazor client-side for this experience, a `SurveyPrompt` component is used by the `Index` component.</span></span> <span data-ttu-id="c3eee-136">Remplacez l’élément `<SurveyPrompt>` par un élément `<Counter />`.</span><span class="sxs-lookup"><span data-stu-id="c3eee-136">Replace the `<SurveyPrompt>` element with a `<Counter />` element.</span></span> <span data-ttu-id="c3eee-137">Si vous utilisez une application côté serveur Blazor pour cette expérience, ajoutez l’élément `<Counter />` au composant `Index` :</span><span class="sxs-lookup"><span data-stu-id="c3eee-137">If you're using a Blazor server-side app for this experience, add the `<Counter />` element to the `Index` component:</span></span>

   <span data-ttu-id="c3eee-138">*Pages/Index.razor* :</span><span class="sxs-lookup"><span data-stu-id="c3eee-138">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. <span data-ttu-id="c3eee-139">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="c3eee-139">Rebuild and run the app.</span></span> <span data-ttu-id="c3eee-140">Le composant `Index` a son propre compteur.</span><span class="sxs-lookup"><span data-stu-id="c3eee-140">The `Index` component has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="c3eee-141">Paramètres de composant</span><span class="sxs-lookup"><span data-stu-id="c3eee-141">Component parameters</span></span>

<span data-ttu-id="c3eee-142">Les composants peuvent également avoir des paramètres.</span><span class="sxs-lookup"><span data-stu-id="c3eee-142">Components can also have parameters.</span></span> <span data-ttu-id="c3eee-143">Les paramètres des composants sont définis à l’aide de propriétés publiques sur la classe de composants décorée avec `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="c3eee-143">Component parameters are defined using public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="c3eee-144">Utilisez des attributs pour spécifier des arguments pour un composant dans le balisage.</span><span class="sxs-lookup"><span data-stu-id="c3eee-144">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="c3eee-145">Mettez à jour le code C# `@code` du composant :</span><span class="sxs-lookup"><span data-stu-id="c3eee-145">Update the component's `@code` C# code:</span></span>

   * <span data-ttu-id="c3eee-146">Ajoutez une propriété `IncrementAmount` décorée avec l’attribut `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="c3eee-146">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="c3eee-147">Modifiez la méthode `IncrementCount` pour utiliser `IncrementAmount` lorsque vous augmentez la valeur de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="c3eee-147">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="c3eee-148">*Pages/Counter.razor* :</span><span class="sxs-lookup"><span data-stu-id="c3eee-148">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="c3eee-149">Spécifiez un paramètre `IncrementAmount` dans l’élément `<Counter>` du composant `Index` à l’aide d’un attribut.</span><span class="sxs-lookup"><span data-stu-id="c3eee-149">Specify an `IncrementAmount` parameter in the `Index` component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="c3eee-150">Définissez la valeur pour incrémenter le compteur par 10.</span><span class="sxs-lookup"><span data-stu-id="c3eee-150">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="c3eee-151">*Pages/Index.razor* :</span><span class="sxs-lookup"><span data-stu-id="c3eee-151">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. <span data-ttu-id="c3eee-152">Rechargez le composant `Index`.</span><span class="sxs-lookup"><span data-stu-id="c3eee-152">Reload the `Index` component.</span></span> <span data-ttu-id="c3eee-153">Le compteur s’incrémente de dix unités chaque fois que le bouton **Cliquer ici** est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="c3eee-153">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="c3eee-154">Le compteur dans le composant `Counter` continue à être incrémenté d’un.</span><span class="sxs-lookup"><span data-stu-id="c3eee-154">The counter in the `Counter` component continues to increment by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="c3eee-155">Acheminer vers les composants</span><span class="sxs-lookup"><span data-stu-id="c3eee-155">Route to components</span></span>

<span data-ttu-id="c3eee-156">La directive `@page` en haut du fichier *Counter.razor* spécifie que le composant `Counter` est un point de terminaison de routage.</span><span class="sxs-lookup"><span data-stu-id="c3eee-156">The `@page` directive at the top of the *Counter.razor* file specifies that the `Counter` component is a routing endpoint.</span></span> <span data-ttu-id="c3eee-157">Le composant `Counter` gère les requêtes envoyées à `/counter`.</span><span class="sxs-lookup"><span data-stu-id="c3eee-157">The `Counter` component handles requests sent to `/counter`.</span></span> <span data-ttu-id="c3eee-158">Sans la directive `@page`, un composant ne gère pas les requêtes acheminées, mais le composant peut toujours être utilisé par d’autres composants.</span><span class="sxs-lookup"><span data-stu-id="c3eee-158">Without the `@page` directive, a component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="c3eee-159">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="c3eee-159">Dependency injection</span></span>

<span data-ttu-id="c3eee-160">Les services enregistrés dans le conteneur de services de l’application sont disponibles pour les composants via [l’injection de dépendance (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c3eee-160">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c3eee-161">Injectez des services dans un composant à l’aide de la directive `@inject`.</span><span class="sxs-lookup"><span data-stu-id="c3eee-161">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="c3eee-162">Examinez les directives du composant `FetchData`.</span><span class="sxs-lookup"><span data-stu-id="c3eee-162">Examine the directives of the `FetchData` component.</span></span>

<span data-ttu-id="c3eee-163">S’il fonctionne avec une application côté serveur Blazor, le service `WeatherForecastService` est inscrit en tant que [singleton](xref:fundamentals/dependency-injection#service-lifetimes), de sorte qu’une instance du service est disponible dans toute l’application.</span><span class="sxs-lookup"><span data-stu-id="c3eee-163">If working with a Blazor server-side app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span> <span data-ttu-id="c3eee-164">La directive `@inject` est utilisée pour injecter l’instance du service `WeatherForecastService` dans le composant.</span><span class="sxs-lookup"><span data-stu-id="c3eee-164">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component.</span></span>

<span data-ttu-id="c3eee-165">*Pages/FetchData.razor* :</span><span class="sxs-lookup"><span data-stu-id="c3eee-165">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="c3eee-166">Le composant `FetchData` utilise le service injecté, comme `ForecastService`, pour récupérer un tableau d’objets `WeatherForecast` :</span><span class="sxs-lookup"><span data-stu-id="c3eee-166">The `FetchData` component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="c3eee-167">S’il fonctionne avec une application côté client Blazor, `HttpClient` est injecté pour obtenir des données de prévisions météorologiques à partir des fichiers *weather.json* du dossier *wwwroot/sample-data* :</span><span class="sxs-lookup"><span data-stu-id="c3eee-167">If working with a Blazor client-side app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder:</span></span>

<span data-ttu-id="c3eee-168">*Pages/FetchData.razor* :</span><span class="sxs-lookup"><span data-stu-id="c3eee-168">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

<span data-ttu-id="c3eee-169">Une boucle [\@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) est utilisée pour restituer chaque instance de prévision sous forme de ligne dans la table des données météo :</span><span class="sxs-lookup"><span data-stu-id="c3eee-169">A [\@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]


## <a name="build-a-todo-list"></a><span data-ttu-id="c3eee-170">Générer une liste de tâches</span><span class="sxs-lookup"><span data-stu-id="c3eee-170">Build a todo list</span></span>

<span data-ttu-id="c3eee-171">Ajoutez un nouveau composant à l’application qui implémente une liste de tâches simple.</span><span class="sxs-lookup"><span data-stu-id="c3eee-171">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="c3eee-172">Ajoutez un fichier vide nommé *Todo.razor* à l’application dans le dossier *Pages* :</span><span class="sxs-lookup"><span data-stu-id="c3eee-172">Add an empty file named *Todo.razor* to the app in the *Pages* folder:</span></span>

1. <span data-ttu-id="c3eee-173">Fournissez le balisage initial pour le composant :</span><span class="sxs-lookup"><span data-stu-id="c3eee-173">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="c3eee-174">Ajoutez le composant `Todo` à la barre de navigation.</span><span class="sxs-lookup"><span data-stu-id="c3eee-174">Add the `Todo` component to the navigation bar.</span></span>

   <span data-ttu-id="c3eee-175">Le composant `NavMenu` (*Shared/NavMenu.razor*) est utilisé dans la disposition de l’application.</span><span class="sxs-lookup"><span data-stu-id="c3eee-175">The `NavMenu` component (*Shared/NavMenu.razor*) is used in the app's layout.</span></span> <span data-ttu-id="c3eee-176">Les dispositions sont des composants qui vous permettent d’éviter la duplication de contenu dans l’application.</span><span class="sxs-lookup"><span data-stu-id="c3eee-176">Layouts are components that allow you to avoid duplication of content in the app.</span></span>

   <span data-ttu-id="c3eee-177">Ajoutez un élément `<NavLink>` pour le composant `Todo` en ajoutant le balisage d’élément de liste suivant sous les éléments de liste existants dans le fichier *Shared/NavMenu.razor* :</span><span class="sxs-lookup"><span data-stu-id="c3eee-177">Add a `<NavLink>` element for the `Todo` component by adding the following list item markup below the existing list items in the *Shared/NavMenu.razor* file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="c3eee-178">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="c3eee-178">Rebuild and run the app.</span></span> <span data-ttu-id="c3eee-179">Consultez la nouvelle page Todo pour vérifier que le lien vers le composant `Todo` fonctionne.</span><span class="sxs-lookup"><span data-stu-id="c3eee-179">Visit the new Todo page to confirm that the link to the `Todo` component works.</span></span>

1. <span data-ttu-id="c3eee-180">Ajoutez un fichier *TodoItem.cs* à la racine du projet pour contenir une classe qui représente un élément Todo.</span><span class="sxs-lookup"><span data-stu-id="c3eee-180">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="c3eee-181">Utilisez le code C# suivant pour la classe `TodoItem` :</span><span class="sxs-lookup"><span data-stu-id="c3eee-181">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="c3eee-182">Revenez au composant `Todo` (*Pages/Todo.razorl*) :</span><span class="sxs-lookup"><span data-stu-id="c3eee-182">Return to the `Todo` component (*Pages/Todo.razor*):</span></span>

   * <span data-ttu-id="c3eee-183">Ajoutez un champ pour les éléments Todo dans un bloc `@code`.</span><span class="sxs-lookup"><span data-stu-id="c3eee-183">Add a field for the todo items in an `@code` block.</span></span> <span data-ttu-id="c3eee-184">Le composant `Todo` utilise ce champ pour maintenir l’état de la liste de tâches.</span><span class="sxs-lookup"><span data-stu-id="c3eee-184">The `Todo` component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="c3eee-185">Ajoutez un balisage de liste non triée et une boucle `foreach` pour effectuer le rendu de chaque élément todo en tant qu’élément de liste (`<li>`).</span><span class="sxs-lookup"><span data-stu-id="c3eee-185">Add unordered list markup and a `foreach` loop to render each todo item as a list item (`<li>`).</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="c3eee-186">L’application nécessite des éléments d’interface utilisateur pour ajouter des éléments todo à la liste.</span><span class="sxs-lookup"><span data-stu-id="c3eee-186">The app requires UI elements for adding todo items to the list.</span></span> <span data-ttu-id="c3eee-187">Ajoutez une entrée de texte (`<input>`) et un bouton (`<button>`) sous la liste non ordonnée (`<ul>...</ul>`) :</span><span class="sxs-lookup"><span data-stu-id="c3eee-187">Add a text input (`<input>`) and a button (`<button>`) below the unordered list (`<ul>...</ul>`):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="c3eee-188">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="c3eee-188">Rebuild and run the app.</span></span> <span data-ttu-id="c3eee-189">Lorsque le bouton **Add todo** est sélectionné, rien ne se produit car aucun gestionnaire d’événements n’est lié au bouton.</span><span class="sxs-lookup"><span data-stu-id="c3eee-189">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="c3eee-190">Ajoutez une méthode `AddTodo` au composant `Todo` et enregistrez-la pour les sélections du bouton à l’aide de l’attribut `@onclick`.</span><span class="sxs-lookup"><span data-stu-id="c3eee-190">Add an `AddTodo` method to the `Todo` component and register it for button selections using the `@onclick` attribute.</span></span> <span data-ttu-id="c3eee-191">La méthode C# `AddTodo` est appelée lorsque le bouton est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="c3eee-191">The `AddTodo` C# method is called when the button is selected:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. <span data-ttu-id="c3eee-192">Pour obtenir le titre du nouvel élément todo, ajoutez un champ de chaîne `newTodo` en haut du bloc `@code` et liez-le à la valeur du texte d’entrée à l’aide de l’attribut `bind` dans l’élément `<input>` :</span><span class="sxs-lookup"><span data-stu-id="c3eee-192">To get the title of the new todo item, add a `newTodo` string field at the top of the `@code` block and bind it to the value of the text input using the `bind` attribute in the `<input>` element:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. <span data-ttu-id="c3eee-193">Mettez à jour la méthode `AddTodo` pour ajouter `TodoItem` avec le titre spécifié à la liste.</span><span class="sxs-lookup"><span data-stu-id="c3eee-193">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="c3eee-194">Supprimez la valeur du texte d’entrée en définissant `newTodo` sur une chaîne vide :</span><span class="sxs-lookup"><span data-stu-id="c3eee-194">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="c3eee-195">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="c3eee-195">Rebuild and run the app.</span></span> <span data-ttu-id="c3eee-196">Ajoutez quelques éléments todo à la liste Todo pour tester le nouveau code.</span><span class="sxs-lookup"><span data-stu-id="c3eee-196">Add some todo items to the todo list to test the new code.</span></span>

1. <span data-ttu-id="c3eee-197">Le texte du titre pour chaque élément todo peut être rendu modifiable et une case à cocher peut aider l’utilisateur à effectuer le suivi des éléments terminés.</span><span class="sxs-lookup"><span data-stu-id="c3eee-197">The title text for each todo item can be made editable, and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="c3eee-198">Ajoutez une entrée de case à cocher pour chaque élément todo et liez sa valeur à la propriété `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="c3eee-198">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="c3eee-199">Remplacez `@todo.Title` par un élément `<input>` lié à `@todo.Title` :</span><span class="sxs-lookup"><span data-stu-id="c3eee-199">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="c3eee-200">Pour vérifier que ces valeurs sont liées, mettez à jour l’en-tête `<h1>` pour afficher le nombre d’éléments todo qui ne sont pas terminés (`IsDone` est `false`).</span><span class="sxs-lookup"><span data-stu-id="c3eee-200">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="c3eee-201">Le composant `Todo` (*Pages/Todo.razor*) terminé :</span><span class="sxs-lookup"><span data-stu-id="c3eee-201">The completed `Todo` component (*Pages/Todo.razor*):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="c3eee-202">Régénérez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="c3eee-202">Rebuild and run the app.</span></span> <span data-ttu-id="c3eee-203">Ajoutez des éléments todo pour tester le nouveau code.</span><span class="sxs-lookup"><span data-stu-id="c3eee-203">Add todo items to test the new code.</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/components>
