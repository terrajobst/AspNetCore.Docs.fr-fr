---
title: Créer votre première Blazor application
author: guardrex
description: Créez un Blazor application pas à pas.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
no-loc:
- Blazor
uid: tutorials/first-blazor-app
ms.openlocfilehash: 8830dcf26b58b5f5fdd36b60298e7b365f99bdd9
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655308"
---
# <a name="build-your-first-opno-locblazor-app"></a><span data-ttu-id="26b63-103">Créer votre première Blazor application</span><span class="sxs-lookup"><span data-stu-id="26b63-103">Build your first Blazor app</span></span>

<span data-ttu-id="26b63-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="26b63-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="26b63-105">Ce didacticiel vous montre comment créer et modifier une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="26b63-105">This tutorial shows you how to build and modify a Blazor app.</span></span>

<span data-ttu-id="26b63-106">Suivez les instructions de l’article <xref:blazor/get-started> pour créer un projet Blazor pour ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="26b63-106">Follow the guidance in the <xref:blazor/get-started> article to create a Blazor project for this tutorial.</span></span> <span data-ttu-id="26b63-107">Nommez le projet *ToDoList*.</span><span class="sxs-lookup"><span data-stu-id="26b63-107">Name the project *ToDoList*.</span></span>

## <a name="build-components"></a><span data-ttu-id="26b63-108">Construire des composants</span><span class="sxs-lookup"><span data-stu-id="26b63-108">Build components</span></span>

1. <span data-ttu-id="26b63-109">Accédez à chacune des trois pages de l’application dans le dossier *pages* : Hébergement, compteur et extraction de données.</span><span class="sxs-lookup"><span data-stu-id="26b63-109">Browse to each of the app's three pages in the *Pages* folder: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="26b63-110">Ces pages sont implémentées par les fichiers de composants Razor *Index.razor*, *Counter.razor* et *FetchData.razor*.</span><span class="sxs-lookup"><span data-stu-id="26b63-110">These pages are implemented by the Razor component files *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span>

1. <span data-ttu-id="26b63-111">Sur la page Counter, sélectionnez le bouton **Click me** pour incrémenter le compteur sans actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="26b63-111">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="26b63-112">L’incrémentation d’un compteur dans une page Web nécessite normalement l’écriture de code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="26b63-112">Incrementing a counter in a webpage normally requires writing JavaScript.</span></span> <span data-ttu-id="26b63-113">Avec Blazor, vous pouvez écrire C# à la place.</span><span class="sxs-lookup"><span data-stu-id="26b63-113">With Blazor, you can write C# instead.</span></span>

1. <span data-ttu-id="26b63-114">Examinez l’implémentation du composant `Counter` dans le fichier *Counter.razor*.</span><span class="sxs-lookup"><span data-stu-id="26b63-114">Examine the implementation of the `Counter` component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="26b63-115">*Pages/Counter.razor* :</span><span class="sxs-lookup"><span data-stu-id="26b63-115">*Pages/Counter.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="26b63-116">L’interface utilisateur du composant `Counter` est définie à l’aide de HTML.</span><span class="sxs-lookup"><span data-stu-id="26b63-116">The UI of the `Counter` component is defined using HTML.</span></span> <span data-ttu-id="26b63-117">La logique de rendu dynamique (par exemple, les boucles, conditions, expressions) est ajoutée à l’aide d’une syntaxe C# intégrée appelée [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="26b63-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="26b63-118">Le balisage HTML et la logique de rendu C# sont convertis en une classe de composants au moment de la génération.</span><span class="sxs-lookup"><span data-stu-id="26b63-118">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="26b63-119">Le nom de la classe .NET générée correspond à celui du fichier.</span><span class="sxs-lookup"><span data-stu-id="26b63-119">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="26b63-120">Les membres de la classe de composants sont définis dans un bloc `@code`.</span><span class="sxs-lookup"><span data-stu-id="26b63-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="26b63-121">Dans le bloc `@code`, l’état du composant (propriétés, champs) et les méthodes sont spécifiés pour la gestion des événements ou pour définir une autre logique de composant.</span><span class="sxs-lookup"><span data-stu-id="26b63-121">In the `@code` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="26b63-122">Ces membres sont ensuite utilisés dans le cadre de la logique de rendu du composant et la gestion des événements.</span><span class="sxs-lookup"><span data-stu-id="26b63-122">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="26b63-123">Lorsque le bouton **Click me** est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="26b63-123">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="26b63-124">Le gestionnaire `Counter` enregistré du composant `onclick` est appelé (méthode `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="26b63-124">The `Counter` component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="26b63-125">Le composant `Counter` regénère son arborescence de rendu.</span><span class="sxs-lookup"><span data-stu-id="26b63-125">The `Counter` component regenerates its render tree.</span></span>
   * <span data-ttu-id="26b63-126">La nouvelle arborescence de rendu est comparée à la précédente.</span><span class="sxs-lookup"><span data-stu-id="26b63-126">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="26b63-127">Seules les modifications apportées à Document Object Model (DOM) sont appliquées.</span><span class="sxs-lookup"><span data-stu-id="26b63-127">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="26b63-128">Le compte affiché est mis à jour.</span><span class="sxs-lookup"><span data-stu-id="26b63-128">The displayed count is updated.</span></span>

1. <span data-ttu-id="26b63-129">Modifiez la logique C# du composant `Counter` pour que l’incrément du compte passe de un à deux.</span><span class="sxs-lookup"><span data-stu-id="26b63-129">Modify the C# logic of the `Counter` component to make the count increment by two instead of one.</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="26b63-130">Régénérez et exécutez l’application pour voir les modifications.</span><span class="sxs-lookup"><span data-stu-id="26b63-130">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="26b63-131">Sélectionnez le bouton **Click me**.</span><span class="sxs-lookup"><span data-stu-id="26b63-131">Select the **Click me** button.</span></span> <span data-ttu-id="26b63-132">Le compteur est incrémenté de deux.</span><span class="sxs-lookup"><span data-stu-id="26b63-132">The counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="26b63-133">Utiliser des composants</span><span class="sxs-lookup"><span data-stu-id="26b63-133">Use components</span></span>

<span data-ttu-id="26b63-134">Incluez un composant dans un autre composant utilisant une syntaxe HTML.</span><span class="sxs-lookup"><span data-stu-id="26b63-134">Include a component in another component using an HTML syntax.</span></span>

1. <span data-ttu-id="26b63-135">Ajoutez le composant `Counter` au composant `Index` de l’application en ajoutant un élément `<Counter />` au composant `Index` (*Index.razor*).</span><span class="sxs-lookup"><span data-stu-id="26b63-135">Add the `Counter` component to the app's `Index` component by adding a `<Counter />` element to the `Index` component (*Index.razor*).</span></span>

   <span data-ttu-id="26b63-136">Si vous utilisez Blazor webassembly pour cette expérience, un composant `SurveyPrompt` est utilisé par le composant `Index`.</span><span class="sxs-lookup"><span data-stu-id="26b63-136">If you're using Blazor WebAssembly for this experience, a `SurveyPrompt` component is used by the `Index` component.</span></span> <span data-ttu-id="26b63-137">Remplacez l’élément `<SurveyPrompt>` par un élément `<Counter />`.</span><span class="sxs-lookup"><span data-stu-id="26b63-137">Replace the `<SurveyPrompt>` element with a `<Counter />` element.</span></span> <span data-ttu-id="26b63-138">Si vous utilisez une application Blazor Server pour cette expérience, ajoutez l’élément `<Counter />` au composant `Index` :</span><span class="sxs-lookup"><span data-stu-id="26b63-138">If you're using a Blazor Server app for this experience, add the `<Counter />` element to the `Index` component:</span></span>

   <span data-ttu-id="26b63-139">*Pages/Index.razor* :</span><span class="sxs-lookup"><span data-stu-id="26b63-139">*Pages/Index.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. <span data-ttu-id="26b63-140">Régénérez et exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="26b63-140">Rebuild and run the app.</span></span> <span data-ttu-id="26b63-141">Le composant `Index` a son propre compteur.</span><span class="sxs-lookup"><span data-stu-id="26b63-141">The `Index` component has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="26b63-142">Paramètres de composant</span><span class="sxs-lookup"><span data-stu-id="26b63-142">Component parameters</span></span>

<span data-ttu-id="26b63-143">Les composants peuvent également avoir des paramètres.</span><span class="sxs-lookup"><span data-stu-id="26b63-143">Components can also have parameters.</span></span> <span data-ttu-id="26b63-144">Les paramètres de composant sont définis à l’aide de propriétés publiques sur la classe de composant avec l’attribut `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="26b63-144">Component parameters are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="26b63-145">Utilisez des attributs pour spécifier des arguments pour un composant dans le balisage.</span><span class="sxs-lookup"><span data-stu-id="26b63-145">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="26b63-146">Mettez à jour le code C# `@code` du composant comme suit :</span><span class="sxs-lookup"><span data-stu-id="26b63-146">Update the component's `@code` C# code as follows:</span></span>

   * <span data-ttu-id="26b63-147">Ajoutez une propriété de `IncrementAmount` publique avec l’attribut `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="26b63-147">Add a public `IncrementAmount` property with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="26b63-148">Modifiez la méthode `IncrementCount` pour utiliser la propriété `IncrementAmount` lors de l’incrémentation de la valeur de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="26b63-148">Change the `IncrementCount` method to use the `IncrementAmount` property when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="26b63-149">*Pages/Counter.razor* :</span><span class="sxs-lookup"><span data-stu-id="26b63-149">*Pages/Counter.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

   <!-- Add back when supported.
       > [!NOTE]
       > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
   -->

1. <span data-ttu-id="26b63-150">Spécifiez un paramètre `IncrementAmount` dans l’élément `Index` du composant `<Counter>` à l’aide d’un attribut.</span><span class="sxs-lookup"><span data-stu-id="26b63-150">Specify an `IncrementAmount` parameter in the `Index` component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="26b63-151">Définissez la valeur pour incrémenter le compteur par 10.</span><span class="sxs-lookup"><span data-stu-id="26b63-151">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="26b63-152">*Pages/Index.razor* :</span><span class="sxs-lookup"><span data-stu-id="26b63-152">*Pages/Index.razor*:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. <span data-ttu-id="26b63-153">Rechargez le composant `Index`.</span><span class="sxs-lookup"><span data-stu-id="26b63-153">Reload the `Index` component.</span></span> <span data-ttu-id="26b63-154">Le compteur s’incrémente de dix unités chaque fois que le bouton **Cliquer ici** est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="26b63-154">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="26b63-155">Le compteur dans le composant `Counter` continue à être incrémenté d’un.</span><span class="sxs-lookup"><span data-stu-id="26b63-155">The counter in the `Counter` component continues to increment by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="26b63-156">Acheminer vers les composants</span><span class="sxs-lookup"><span data-stu-id="26b63-156">Route to components</span></span>

<span data-ttu-id="26b63-157">La directive `@page` en haut du fichier *Counter.razor* spécifie que le composant `Counter` est un point de terminaison de routage.</span><span class="sxs-lookup"><span data-stu-id="26b63-157">The `@page` directive at the top of the *Counter.razor* file specifies that the `Counter` component is a routing endpoint.</span></span> <span data-ttu-id="26b63-158">Le composant `Counter` gère les requêtes envoyées à `/counter`.</span><span class="sxs-lookup"><span data-stu-id="26b63-158">The `Counter` component handles requests sent to `/counter`.</span></span> <span data-ttu-id="26b63-159">Sans la directive `@page`, un composant ne gère pas les requêtes acheminées, mais le composant peut toujours être utilisé par d’autres composants.</span><span class="sxs-lookup"><span data-stu-id="26b63-159">Without the `@page` directive, a component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="26b63-160">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="26b63-160">Dependency injection</span></span>

### <a name="opno-locblazor-server-experience"></a><span data-ttu-id="26b63-161">expérience du serveur de Blazor</span><span class="sxs-lookup"><span data-stu-id="26b63-161">Blazor Server experience</span></span>

<span data-ttu-id="26b63-162">Si vous utilisez une application Blazor Server, le service `WeatherForecastService` est inscrit en tant que [Singleton](xref:fundamentals/dependency-injection#service-lifetimes) dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="26b63-162">If working with a Blazor Server app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="26b63-163">Une instance du service est disponible dans l’ensemble de l’application via l' [injection de dépendances (di)](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="26b63-163">An instance of the service is available throughout the app via [dependency injection (DI)](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="26b63-164">La directive `@inject` est utilisée pour injecter l’instance du service `WeatherForecastService` dans le composant `FetchData`.</span><span class="sxs-lookup"><span data-stu-id="26b63-164">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the `FetchData` component.</span></span>

<span data-ttu-id="26b63-165">*Pages/FetchData.razor* :</span><span class="sxs-lookup"><span data-stu-id="26b63-165">*Pages/FetchData.razor*:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="26b63-166">Le composant `FetchData` utilise le service injecté, comme `ForecastService`, pour récupérer un tableau d’objets `WeatherForecast` :</span><span class="sxs-lookup"><span data-stu-id="26b63-166">The `FetchData` component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

### <a name="opno-locblazor-webassembly-experience"></a><span data-ttu-id="26b63-167">expérience d' Blazor webassembly</span><span class="sxs-lookup"><span data-stu-id="26b63-167">Blazor WebAssembly experience</span></span>

<span data-ttu-id="26b63-168">Si vous utilisez une application Blazor webassembly, `HttpClient` est injecté pour obtenir des données de prévision météorologiques à partir du fichier *Weather. JSON* dans le dossier *wwwroot/Sample-Data* .</span><span class="sxs-lookup"><span data-stu-id="26b63-168">If working with a Blazor WebAssembly app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder.</span></span>

<span data-ttu-id="26b63-169">*Pages/FetchData.razor* :</span><span class="sxs-lookup"><span data-stu-id="26b63-169">*Pages/FetchData.razor*:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

<span data-ttu-id="26b63-170">Une boucle [`@foreach`](/dotnet/csharp/language-reference/keywords/foreach-in) est utilisée pour afficher chaque instance de prévision sous la forme d’une ligne dans la table des données météorologiques :</span><span class="sxs-lookup"><span data-stu-id="26b63-170">An [`@foreach`](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="26b63-171">Générer une liste de tâches</span><span class="sxs-lookup"><span data-stu-id="26b63-171">Build a todo list</span></span>

<span data-ttu-id="26b63-172">Ajoutez un nouveau composant à l’application qui implémente une liste de tâches simple.</span><span class="sxs-lookup"><span data-stu-id="26b63-172">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="26b63-173">Ajoutez un fichier vide nommé *Todo.razor* à l’application dans le dossier *Pages* :</span><span class="sxs-lookup"><span data-stu-id="26b63-173">Add an empty file named *Todo.razor* to the app in the *Pages* folder:</span></span>

1. <span data-ttu-id="26b63-174">Fournissez le balisage initial pour le composant :</span><span class="sxs-lookup"><span data-stu-id="26b63-174">Provide the initial markup for the component:</span></span>

   ```razor
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="26b63-175">Ajoutez le composant `Todo` à la barre de navigation.</span><span class="sxs-lookup"><span data-stu-id="26b63-175">Add the `Todo` component to the navigation bar.</span></span>

   <span data-ttu-id="26b63-176">Le composant `NavMenu` (*Shared/NavMenu.razor*) est utilisé dans la disposition de l’application.</span><span class="sxs-lookup"><span data-stu-id="26b63-176">The `NavMenu` component (*Shared/NavMenu.razor*) is used in the app's layout.</span></span> <span data-ttu-id="26b63-177">Les dispositions sont des composants qui vous permettent d’éviter la duplication de contenu dans l’application.</span><span class="sxs-lookup"><span data-stu-id="26b63-177">Layouts are components that allow you to avoid duplication of content in the app.</span></span>

   <span data-ttu-id="26b63-178">Ajoutez un élément `<NavLink>` pour le composant `Todo` en ajoutant le balisage d’élément de liste suivant sous les éléments de liste existants dans le fichier *Shared/NavMenu.razor* :</span><span class="sxs-lookup"><span data-stu-id="26b63-178">Add a `<NavLink>` element for the `Todo` component by adding the following list item markup below the existing list items in the *Shared/NavMenu.razor* file:</span></span>

   ```razor
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="26b63-179">Régénérez et exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="26b63-179">Rebuild and run the app.</span></span> <span data-ttu-id="26b63-180">Consultez la nouvelle page Todo pour vérifier que le lien vers le composant `Todo` fonctionne.</span><span class="sxs-lookup"><span data-stu-id="26b63-180">Visit the new Todo page to confirm that the link to the `Todo` component works.</span></span>

1. <span data-ttu-id="26b63-181">Ajoutez un fichier *TodoItem.cs* à la racine du projet pour contenir une classe qui représente un élément Todo.</span><span class="sxs-lookup"><span data-stu-id="26b63-181">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="26b63-182">Utilisez le code C# suivant pour la classe `TodoItem` :</span><span class="sxs-lookup"><span data-stu-id="26b63-182">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="26b63-183">Revenez au composant `Todo` (*Pages/Todo.razorl*) :</span><span class="sxs-lookup"><span data-stu-id="26b63-183">Return to the `Todo` component (*Pages/Todo.razor*):</span></span>

   * <span data-ttu-id="26b63-184">Ajoutez un champ pour les éléments Todo dans un bloc `@code`.</span><span class="sxs-lookup"><span data-stu-id="26b63-184">Add a field for the todo items in an `@code` block.</span></span> <span data-ttu-id="26b63-185">Le composant `Todo` utilise ce champ pour maintenir l’état de la liste de tâches.</span><span class="sxs-lookup"><span data-stu-id="26b63-185">The `Todo` component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="26b63-186">Ajoutez un balisage de liste non triée et une boucle `foreach` pour effectuer le rendu de chaque élément todo en tant qu’élément de liste (`<li>`).</span><span class="sxs-lookup"><span data-stu-id="26b63-186">Add unordered list markup and a `foreach` loop to render each todo item as a list item (`<li>`).</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="26b63-187">L’application nécessite des éléments d’interface utilisateur pour ajouter des éléments todo à la liste.</span><span class="sxs-lookup"><span data-stu-id="26b63-187">The app requires UI elements for adding todo items to the list.</span></span> <span data-ttu-id="26b63-188">Ajoutez une entrée de texte (`<input>`) et un bouton (`<button>`) sous la liste non ordonnée (`<ul>...</ul>`) :</span><span class="sxs-lookup"><span data-stu-id="26b63-188">Add a text input (`<input>`) and a button (`<button>`) below the unordered list (`<ul>...</ul>`):</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="26b63-189">Régénérez et exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="26b63-189">Rebuild and run the app.</span></span> <span data-ttu-id="26b63-190">Lorsque le bouton **Add todo** est sélectionné, rien ne se produit car aucun gestionnaire d’événements n’est lié au bouton.</span><span class="sxs-lookup"><span data-stu-id="26b63-190">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="26b63-191">Ajoutez une méthode `AddTodo` au composant `Todo` et enregistrez-la pour les sélections du bouton à l’aide de l’attribut `@onclick`.</span><span class="sxs-lookup"><span data-stu-id="26b63-191">Add an `AddTodo` method to the `Todo` component and register it for button selections using the `@onclick` attribute.</span></span> <span data-ttu-id="26b63-192">La méthode C# `AddTodo` est appelée lorsque le bouton est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="26b63-192">The `AddTodo` C# method is called when the button is selected:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. <span data-ttu-id="26b63-193">Pour obtenir le titre du nouvel élément todo, ajoutez un champ de chaîne `newTodo` en haut du bloc `@code` et liez-le à la valeur du texte d’entrée à l’aide de l’attribut `bind` dans l’élément `<input>` :</span><span class="sxs-lookup"><span data-stu-id="26b63-193">To get the title of the new todo item, add a `newTodo` string field at the top of the `@code` block and bind it to the value of the text input using the `bind` attribute in the `<input>` element:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```razor
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. <span data-ttu-id="26b63-194">Mettez à jour la méthode `AddTodo` pour ajouter `TodoItem` avec le titre spécifié à la liste.</span><span class="sxs-lookup"><span data-stu-id="26b63-194">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="26b63-195">Supprimez la valeur du texte d’entrée en définissant `newTodo` sur une chaîne vide :</span><span class="sxs-lookup"><span data-stu-id="26b63-195">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="26b63-196">Régénérez et exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="26b63-196">Rebuild and run the app.</span></span> <span data-ttu-id="26b63-197">Ajoutez quelques éléments todo à la liste Todo pour tester le nouveau code.</span><span class="sxs-lookup"><span data-stu-id="26b63-197">Add some todo items to the todo list to test the new code.</span></span>

1. <span data-ttu-id="26b63-198">Le texte du titre pour chaque élément todo peut être rendu modifiable et une case à cocher peut aider l’utilisateur à effectuer le suivi des éléments terminés.</span><span class="sxs-lookup"><span data-stu-id="26b63-198">The title text for each todo item can be made editable, and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="26b63-199">Ajoutez une entrée de case à cocher pour chaque élément todo et liez sa valeur à la propriété `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="26b63-199">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="26b63-200">Remplacez `@todo.Title` par un élément `<input>` lié à `@todo.Title` :</span><span class="sxs-lookup"><span data-stu-id="26b63-200">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="26b63-201">Pour vérifier que ces valeurs sont liées, mettez à jour l’en-tête `<h1>` pour afficher le nombre d’éléments todo qui ne sont pas terminés (`IsDone` est `false`).</span><span class="sxs-lookup"><span data-stu-id="26b63-201">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```razor
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="26b63-202">Le composant `Todo` (*Pages/Todo.razor*) terminé :</span><span class="sxs-lookup"><span data-stu-id="26b63-202">The completed `Todo` component (*Pages/Todo.razor*):</span></span>

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="26b63-203">Régénérez et exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="26b63-203">Rebuild and run the app.</span></span> <span data-ttu-id="26b63-204">Ajoutez des éléments todo pour tester le nouveau code.</span><span class="sxs-lookup"><span data-stu-id="26b63-204">Add todo items to test the new code.</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/components>
