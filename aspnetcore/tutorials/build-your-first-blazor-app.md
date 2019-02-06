---
title: Créer votre première application Blazor
author: guardrex
description: Créez une application Blazor pas à pas et découvrez rapidement les fonctionnalités de base de l’infrastructure Composants Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 99396e200eb121524abccc20a7d461062c94ecc7
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667995"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="700d2-103">Créer votre première application Blazor</span><span class="sxs-lookup"><span data-stu-id="700d2-103">Build your first Blazor app</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="700d2-104">Dans ce tutoriel, vous créez une application Blazor pas à pas et découvrez rapidement les fonctionnalités de base de l’infrastructure Composants Razor.</span><span class="sxs-lookup"><span data-stu-id="700d2-104">In this tutorial, you build a Blazor app step-by-step and quickly learn the basic features of the Razor Components framework.</span></span>

<span data-ttu-id="700d2-105">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="700d2-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="700d2-106">Consultez la rubrique [Bien démarrer](xref:razor-components/get-started) pour connaître les prérequis.</span><span class="sxs-lookup"><span data-stu-id="700d2-106">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

<span data-ttu-id="700d2-107">Pour créer le projet dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="700d2-107">To create the project in Visual Studio:</span></span>

1. <span data-ttu-id="700d2-108">Sélectionnez **Fichier** > **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="700d2-108">Select **File** > **New** > **Project**.</span></span> <span data-ttu-id="700d2-109">Sélectionnez **Web** > **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="700d2-109">Select **Web** > **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="700d2-110">Nommez le projet « BlazorApp1 » dans le champ **Nom**.</span><span class="sxs-lookup"><span data-stu-id="700d2-110">Name the project "BlazorApp1" in the **Name** field.</span></span> <span data-ttu-id="700d2-111">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="700d2-111">Select **OK**.</span></span>

    ![Nouveau projet ASP.NET Core](build-your-first-blazor-app/_static/new-aspnet-core-project.png)

1. <span data-ttu-id="700d2-113">La boîte de dialogue **Nouvelle application web ASP.NET Core** s’affiche.</span><span class="sxs-lookup"><span data-stu-id="700d2-113">The **New ASP.NET Core Web Application** dialog appears.</span></span> <span data-ttu-id="700d2-114">Assurez-vous que **.NET Core** est sélectionné en haut.</span><span class="sxs-lookup"><span data-stu-id="700d2-114">Make sure **.NET Core** is selected at the top.</span></span> <span data-ttu-id="700d2-115">Sélectionnez **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="700d2-115">Select **ASP.NET Core 2.1**.</span></span> <span data-ttu-id="700d2-116">Choisissez le modèle **Blazor** et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="700d2-116">Choose the **Blazor** template and select **OK**.</span></span>

    ![Boîte de dialogue Nouvelle application Blazor](build-your-first-blazor-app/_static/new-blazor-app-dialog.png)

1. <span data-ttu-id="700d2-118">Une fois que le projet est créé, appuyez sur **Ctrl-F5** pour exécuter l’application *sans le débogueur*.</span><span class="sxs-lookup"><span data-stu-id="700d2-118">Once the project is created, press **Ctrl-F5** to run the app *without the debugger*.</span></span> <span data-ttu-id="700d2-119">L’exécution avec le débogueur (**F5**) n’est pas prise en charge pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="700d2-119">Running with the debugger (**F5**) isn't supported at this time.</span></span>

> [!NOTE]
> <span data-ttu-id="700d2-120">Si vous n’utilisez pas Visual Studio, créez l’application Blazor à l’invite de commandes sur Windows, macOS ou Linux :</span><span class="sxs-lookup"><span data-stu-id="700d2-120">If not using Visual Studio, create the Blazor app at a command prompt on Windows, macOS, or Linux:</span></span>
>
> ```console
> dotnet new --install "Microsoft.AspNetCore.Blazor.Templates"
> dotnet new blazor -o BlazorApp1
> cd BlazorApp1
> dotnet run
> ```
>
> <span data-ttu-id="700d2-121">Accédez à l’application à l’aide du port et de l’adresse localhost fournis dans la sortie de la fenêtre de console après l’exécution de `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="700d2-121">Navigate to the app using the localhost address and port provided in the console window output after `dotnet run` is executed.</span></span> <span data-ttu-id="700d2-122">Utilisez **Ctrl-C** dans la fenêtre de console pour arrêter l’application.</span><span class="sxs-lookup"><span data-stu-id="700d2-122">Use **Ctrl-C** in the console window to shutdown the app.</span></span>

<span data-ttu-id="700d2-123">L’application Blazor s’exécute dans le navigateur :</span><span class="sxs-lookup"><span data-stu-id="700d2-123">The Blazor app runs in the browser:</span></span>

![Page d’accueil d’application Blazor](https://user-images.githubusercontent.com/1874516/39509497-5515c3ea-4d9b-11e8-887f-019ea4fdb3ee.png)

## <a name="build-components"></a><span data-ttu-id="700d2-125">Construire des composants</span><span class="sxs-lookup"><span data-stu-id="700d2-125">Build components</span></span>

1. <span data-ttu-id="700d2-126">Accédez à chacune des trois pages de l’application : Accueil, Compteur et Extraire des données.</span><span class="sxs-lookup"><span data-stu-id="700d2-126">Browse to each of the app's three pages: Home, Counter, and Fetch data.</span></span>

    <span data-ttu-id="700d2-127">Ces trois pages sont implémentées par les trois fichiers Razor dans le dossier *Pages* : *Index.cshtml*, *Counter.cshtml* et *FetchData.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="700d2-127">These three pages are implemented by the three Razor files in the *Pages* folder: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.</span></span> <span data-ttu-id="700d2-128">Chacun de ces fichiers implémente un composant qui est compilé et exécuté côté client dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="700d2-128">Each of these files implements a component that's compiled and executed client-side in the browser.</span></span>

1. <span data-ttu-id="700d2-129">Sélectionnez le bouton sur la page Compteur.</span><span class="sxs-lookup"><span data-stu-id="700d2-129">Select the button on the Counter page.</span></span>

    ![Page d’accueil d’application Blazor](https://user-images.githubusercontent.com/1874516/39509525-6e367c66-4d9b-11e8-9978-e52a9750c34b.png)

    <span data-ttu-id="700d2-131">À chaque fois que le bouton est sélectionné, le compteur est incrémenté sans actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="700d2-131">Each time the button is selected, the counter is incremented without a page refresh.</span></span> <span data-ttu-id="700d2-132">En règle générale, ce type de comportement côté client est contrôlé dans JavaScript ; mais ici, il est implémenté dans C# et .NET par le composant `Counter`.</span><span class="sxs-lookup"><span data-stu-id="700d2-132">Normally, this kind of client-side behavior is handled in JavaScript; but here, it's implemented in C# and .NET by the `Counter` component.</span></span>

1. <span data-ttu-id="700d2-133">Examinons l’implémentation du composant `Counter` dans le fichier *Counter.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="700d2-133">Take a look at the implementation of the `Counter` component in the *Counter.cshtml* file:</span></span>

    ```cshtml
    @page "/counter"

    <h1>Counter</h1>

    <p>Current count: @currentCount</p>

    <button class="btn btn-primary" onclick="@IncrementCount">Click me</button>

    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount++;
        }
    }
    ```

    <span data-ttu-id="700d2-134">L’interface utilisateur pour le composant `Counter` est définie à l’aide de code HTML normal.</span><span class="sxs-lookup"><span data-stu-id="700d2-134">The UI for the `Counter` component is defined using normal HTML.</span></span> <span data-ttu-id="700d2-135">La logique de rendu dynamique (par exemple, les boucles, conditions, expressions) est ajoutée à l’aide d’une syntaxe C# intégrée appelée [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="700d2-135">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor).</span></span> <span data-ttu-id="700d2-136">Le balisage HTML et la logique de rendu C# sont convertis en une classe de composants au moment de la génération.</span><span class="sxs-lookup"><span data-stu-id="700d2-136">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="700d2-137">Le nom de la classe .NET générée correspond à celui du fichier.</span><span class="sxs-lookup"><span data-stu-id="700d2-137">The name of the generated .NET class matches the name of the file.</span></span>

    <span data-ttu-id="700d2-138">Les membres de la classe de composants sont définis dans un bloc `@functions`.</span><span class="sxs-lookup"><span data-stu-id="700d2-138">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="700d2-139">Dans le bloc `@functions`, l’état du composant (propriétés, champs) et des méthodes peuvent être spécifiés pour la gestion des événements ou pour définir une autre logique de composant.</span><span class="sxs-lookup"><span data-stu-id="700d2-139">In the `@functions` block, component state (properties, fields) and methods can be specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="700d2-140">Ces membres peuvent ensuite servir dans le cadre de la logique de rendu du composant et la gestion des événements.</span><span class="sxs-lookup"><span data-stu-id="700d2-140">These members can then be used as part of the component's rendering logic and for handling events.</span></span>

    <span data-ttu-id="700d2-141">Lorsque le bouton est sélectionné, le gestionnaire `onclick` du composant `Counter` est appelé (méthode `IncrementCount`) et le composant `Counter` régénère son arborescence de rendu.</span><span class="sxs-lookup"><span data-stu-id="700d2-141">When the button is selected, the `Counter` component's registered `onclick` handler is called (the `IncrementCount` method) and the `Counter` component regenerates its render tree.</span></span> <span data-ttu-id="700d2-142">Blazor compare la nouvelle arborescence de rendu avec la précédente et applique les modifications au modèle DOM (Document Object Model) du navigateur.</span><span class="sxs-lookup"><span data-stu-id="700d2-142">Blazor compares the new render tree against the previous one and applies any modifications to the browser Document Object Model (DOM).</span></span> <span data-ttu-id="700d2-143">Le compte affiché est mis à jour.</span><span class="sxs-lookup"><span data-stu-id="700d2-143">The displayed count is updated.</span></span>

1. <span data-ttu-id="700d2-144">Mettez à jour le balisage pour le composant `Counter` pour rendre l’en-tête de niveau supérieur plus *passionnant*.</span><span class="sxs-lookup"><span data-stu-id="700d2-144">Update the markup for the `Counter` component to make the top-level header more *exciting*.</span></span>

    ```cshtml
    @page "/counter"
    <h1><em>Counter!!</em></h1>
    ```

1. <span data-ttu-id="700d2-145">Modifiez aussi la logique C# du composant `Counter` pour que l’incrément du compte passe de un à deux.</span><span class="sxs-lookup"><span data-stu-id="700d2-145">Also modify the C# logic of the `Counter` component to make the count increment by two instead of one.</span></span>

    ```cshtml
    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount += 2;
        }
    }
    ```

1. <span data-ttu-id="700d2-146">Actualisez la page du compteur dans le navigateur pour voir les modifications.</span><span class="sxs-lookup"><span data-stu-id="700d2-146">Refresh the counter page in the browser to see the changes.</span></span>

    ![Compteur passionnant](https://user-images.githubusercontent.com/1874516/39509668-e8949a92-4d9b-11e8-91e9-b6a494695d92.png)

## <a name="use-components"></a><span data-ttu-id="700d2-148">Utiliser des composants</span><span class="sxs-lookup"><span data-stu-id="700d2-148">Use components</span></span>

<span data-ttu-id="700d2-149">Une fois un composant défini, il peut être utilisé pour implémenter d’autres composants.</span><span class="sxs-lookup"><span data-stu-id="700d2-149">After a component is defined, the component can be used to implement other components.</span></span> <span data-ttu-id="700d2-150">Le balisage pour l’utilisation d’un composant ressemble à une balise HTML où le nom de la balise est le type du composant.</span><span class="sxs-lookup"><span data-stu-id="700d2-150">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

1. <span data-ttu-id="700d2-151">Ajoutez un composant `Counter` à la page Accueil de l’application (*Index.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="700d2-151">Add a `Counter` component to the Home page of the app (*Index.cshtml*).</span></span>

    ```cshtml
    @page "/"

    <h1>Hello, world!</h1>

    Welcome to your new app.

    <SurveyPrompt Title="How is Blazor working for you?" />

    <Counter />
    ```

1. <span data-ttu-id="700d2-152">Actualisez la page d’accueil dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="700d2-152">Refresh the home page in the browser.</span></span> <span data-ttu-id="700d2-153">Notez l’instance distincte du composant `Counter` sur la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="700d2-153">Note the separate instance of the `Counter` component on the Home page.</span></span>

    ![Page d’accueil Blazor avec compteur](https://user-images.githubusercontent.com/1874516/39509718-224483f6-4d9c-11e8-9030-b4c7228d669d.png)

## <a name="component-parameters"></a><span data-ttu-id="700d2-155">Paramètres de composant</span><span class="sxs-lookup"><span data-stu-id="700d2-155">Component parameters</span></span>

<span data-ttu-id="700d2-156">Les composants peuvent également avoir des paramètres qui sont définies à l’aide de propriétés privées sur la classe de composants décorée avec `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="700d2-156">Components can also have parameters, which are defined using private properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="700d2-157">Utilisez des attributs pour spécifier des arguments pour un composant dans le balisage.</span><span class="sxs-lookup"><span data-stu-id="700d2-157">Use attributes to specify arguments for a component in markup.</span></span> 

1. <span data-ttu-id="700d2-158">Mettez à jour le composant `Counter` pour avoir un paramètre `IncrementAmount` dont la valeur par défaut est 1.</span><span class="sxs-lookup"><span data-stu-id="700d2-158">Update the `Counter` component to have an `IncrementAmount` parameter that defaults to 1.</span></span>

    ```cshtml
    @functions {
        int currentCount = 0;

        [Parameter]
        private int IncrementAmount { get; set; } = 1;

        void IncrementCount()
        {
            currentCount += IncrementAmount;
        }
    }
    ```

    > [!NOTE]
    > <span data-ttu-id="700d2-159">À partir de Visual Studio, vous pouvez rapidement ajouter un paramètre de composant à l’aide de l’extrait de code `para`.</span><span class="sxs-lookup"><span data-stu-id="700d2-159">From Visual Studio, you can quickly add a component parameter by using the `para` snippet.</span></span> <span data-ttu-id="700d2-160">Tapez `para`, puis appuyez sur la touche `Tab` deux fois.</span><span class="sxs-lookup"><span data-stu-id="700d2-160">Type `para` and then press the `Tab` key twice.</span></span>

1. <span data-ttu-id="700d2-161">Sur la page d’accueil (*Index.cshtml*), modifiez la quantité d’incrémentation pour `Counter` sur 10 en définissant un attribut qui correspond au nom de la propriété du composant pour `IncrementCount`.</span><span class="sxs-lookup"><span data-stu-id="700d2-161">On the Home page (*Index.cshtml*), change the increment amount for the `Counter` to 10 by setting an attribute that matches the name of the component's property for `IncrementCount`.</span></span>

    ```cshtml
    <Counter IncrementAmount="10" />
    ```

1. <span data-ttu-id="700d2-162">Rechargez la page.</span><span class="sxs-lookup"><span data-stu-id="700d2-162">Reload the page.</span></span>

    <span data-ttu-id="700d2-163">L’incrémentation du compteur sur la page d’accueil se fait maintenant par 10, tandis que le compteur sur la page Compteur incrémente toujours par 1.</span><span class="sxs-lookup"><span data-stu-id="700d2-163">The counter on the Home page now increments by 10, while the counter on the Counter page still increments by 1.</span></span>

    ![Compteur Blazor par 10](https://user-images.githubusercontent.com/1874516/39509798-618f0720-4d9c-11e8-9125-3d4c634dff46.png)

## <a name="route-to-components"></a><span data-ttu-id="700d2-165">Acheminer vers les composants</span><span class="sxs-lookup"><span data-stu-id="700d2-165">Route to components</span></span>

<span data-ttu-id="700d2-166">La directive `@page` en haut du fichier *Counter.cshtml* spécifie que ce composant est une page vers laquelle les requêtes peuvent être acheminées.</span><span class="sxs-lookup"><span data-stu-id="700d2-166">The `@page` directive at the top of the *Counter.cshtml* file specifies that this component is a page to which requests can be routed.</span></span> <span data-ttu-id="700d2-167">Plus précisément, le composant `Counter` gère les requêtes envoyées à `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="700d2-167">Specifically, the `Counter` component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="700d2-168">Sans la directive `@page`, le composant ne gèrerait pas les requêtes acheminées, mais le composant pourrait toujours être utilisé par d’autres composants.</span><span class="sxs-lookup"><span data-stu-id="700d2-168">Without the `@page` directive, the component wouldn't handle routed requests, but the component could still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="700d2-169">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="700d2-169">Dependency injection</span></span>

<span data-ttu-id="700d2-170">Les services enregistrés auprès du fournisseur de services de l’application sont disponibles pour les composants via [l’injection de dépendance (DI)](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="700d2-170">Services registered with the app's service provider are available to components via [dependency injection (DI)](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection).</span></span> <span data-ttu-id="700d2-171">Les services peuvent être injectés dans un composant à l’aide de la directive `@inject`.</span><span class="sxs-lookup"><span data-stu-id="700d2-171">Services can be injected into a component using the `@inject` directive.</span></span>

<span data-ttu-id="700d2-172">Examinons l’implémentation du composant `FetchData` dans *FetchData.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="700d2-172">Take a look at the implementation of the `FetchData` component in *FetchData.cshtml*.</span></span> <span data-ttu-id="700d2-173">La directive `@inject` est utilisée pour injecter une instance [HttpClient](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient) dans le composant.</span><span class="sxs-lookup"><span data-stu-id="700d2-173">The `@inject` directive is used to inject an [HttpClient](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient) instance into the component.</span></span>

```cshtml
@page "/fetchdata"
@inject HttpClient Http
```

<span data-ttu-id="700d2-174">Le composant `FetchData` utilise le `HttpClient` injecté pour récupérer des données JSON à partir du serveur lorsque le composant est initialisé.</span><span class="sxs-lookup"><span data-stu-id="700d2-174">The `FetchData` component uses the injected `HttpClient` to retrieve JSON data from the server when the component is initialized.</span></span> <span data-ttu-id="700d2-175">En coulisses, le `HttpClient` fourni par le runtime Blazor est implémenté à l’aide d’une interopération JavaScript pour appeler l’API Fetch sous-jacente du navigateur afin d’envoyer la requête (à partir de C#, il est possible d’appeler n’importe quelle bibliothèque JavaScript ou API du navigateur).</span><span class="sxs-lookup"><span data-stu-id="700d2-175">Under the covers, the `HttpClient` provided by the Blazor runtime is implemented using JavaScript interop to call the underlying browser's Fetch API to send the request (from C#, it's possible to call any JavaScript library or browser API).</span></span> <span data-ttu-id="700d2-176">Les données récupérées sont désérialisées dans la variable C# `forecasts` en tant que tableau d’objets `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="700d2-176">The retrieved data is deserialized into the `forecasts` C# variable as an array of `WeatherForecast` objects.</span></span>

```cshtml
@functions {
    WeatherForecast[] forecasts;

    protected override async Task OnInitAsync()
    {
        forecasts = await Http.GetJsonAsync<WeatherForecast[]>("/sample-data/weather.json");
    }

    class WeatherForecast
    {
        public DateTime Date { get; set; }
        public int TemperatureC { get; set; }
        public int TemperatureF { get; set; }
        public string Summary { get; set; }
    }
}
```

<span data-ttu-id="700d2-177">Une boucle `@foreach` est utilisée pour restituer chaque instance de prévision sous forme de ligne dans la table sur la météo.</span><span class="sxs-lookup"><span data-stu-id="700d2-177">A `@foreach` loop is used to render each forecast instance as a row in the weather table.</span></span>

```cshtml
<table class="table">
    <thead>
        <tr>
            <th>Date</th>
            <th>Temp. (C)</th>
            <th>Temp. (F)</th>
            <th>Summary</th>
        </tr>
    </thead>
    <tbody>
        @foreach (var forecast in forecasts)
        {
            <tr>
                <td>@forecast.Date.ToShortDateString()</td>
                <td>@forecast.TemperatureC</td>
                <td>@forecast.TemperatureF</td>
                <td>@forecast.Summary</td>
            </tr>
        }
    </tbody>
</table>
```

## <a name="build-a-todo-list"></a><span data-ttu-id="700d2-178">Générer une liste de tâches</span><span class="sxs-lookup"><span data-stu-id="700d2-178">Build a todo list</span></span>

<span data-ttu-id="700d2-179">Ajoutez une nouvelle page à l’application qui implémente une liste de tâches simple.</span><span class="sxs-lookup"><span data-stu-id="700d2-179">Add a new page to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="700d2-180">Ajoutez un fichier texte vide au dossier *Pages* nommé *Todo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="700d2-180">Add an empty text file to the *Pages* folder named *Todo.cshtml*.</span></span>

1. <span data-ttu-id="700d2-181">Fournissez le balisage initial pour la page.</span><span class="sxs-lookup"><span data-stu-id="700d2-181">Provide the initial markup for the page.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>
    ```

1. <span data-ttu-id="700d2-182">Ajouter la page Todo de la barre de navigation en mettant à jour *Shared/NavMenu.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="700d2-182">Add the Todo page to the navigation bar by updating *Shared/NavMenu.cshtml*.</span></span> <span data-ttu-id="700d2-183">Ajoutez un `NavLink` pour la page Todo en ajoutant le balisage d’élément de liste suivant sous les éléments de liste existants.</span><span class="sxs-lookup"><span data-stu-id="700d2-183">Add a `NavLink` for the Todo page by adding the following list item markup below the existing list items.</span></span>

    ```cshtml
    <li class="nav-item px-3">
        <NavLink class="nav-link" href="todo">
            <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
        </NavLink>
    </li>
    ```

1. <span data-ttu-id="700d2-184">Actualisez l’application dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="700d2-184">Refresh the app in the browser.</span></span> <span data-ttu-id="700d2-185">Consultez la nouvelle page Todo.</span><span class="sxs-lookup"><span data-stu-id="700d2-185">See the new Todo page.</span></span>

    ![Démarrer la page Todo Blazor](https://user-images.githubusercontent.com/1874516/39509907-bb27e77a-4d9c-11e8-91e7-ea1e7c01729e.png)

1. <span data-ttu-id="700d2-187">Ajoutez un fichier *TodoItem.cs* à la racine du projet pour contenir une classe afin de représenter les éléments des tâches.</span><span class="sxs-lookup"><span data-stu-id="700d2-187">Add a *TodoItem.cs* file to the root of the project to hold a class to represent the todo items.</span></span>

1. <span data-ttu-id="700d2-188">Utilisez le code C# suivant pour la classe `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="700d2-188">Use the following C# code for the `TodoItem` class.</span></span>

    ```csharp
    public class TodoItem
    {
        public string Title { get; set; }
        public bool IsDone { get; set; }
    }
    ```

1. <span data-ttu-id="700d2-189">Revenez au composant `Todo` dans *Todo.cshtml* et ajoutez un champ pour les tâches Todo dans un bloc `@functions`.</span><span class="sxs-lookup"><span data-stu-id="700d2-189">Go back to the `Todo` component in *Todo.cshtml* and add a field for the todos in a `@functions` block.</span></span> <span data-ttu-id="700d2-190">Le composant `Todo` utilise ce champ pour maintenir l’état de la liste de tâches.</span><span class="sxs-lookup"><span data-stu-id="700d2-190">The `Todo` component uses this field to maintain the state of the todo list.</span></span>

    ```cshtml
    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. <span data-ttu-id="700d2-191">Ajoutez un balisage de liste non triée et une boucle `foreach` pour effectuer le rendu de chaque élément todo en tant qu’élément de liste.</span><span class="sxs-lookup"><span data-stu-id="700d2-191">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>
    ```

1. <span data-ttu-id="700d2-192">L’application nécessite des éléments d’interface utilisateur pour ajouter des éléments todo à la liste.</span><span class="sxs-lookup"><span data-stu-id="700d2-192">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="700d2-193">Ajoutez une entrée de texte et un bouton sous la liste.</span><span class="sxs-lookup"><span data-stu-id="700d2-193">Add a text input and a button below the list.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>

    <input placeholder="Something todo" />
    <button>Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. <span data-ttu-id="700d2-194">Actualisez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="700d2-194">Refresh the browser.</span></span>

    ![Ajouter un bouton todo](https://user-images.githubusercontent.com/1874516/39512402-bc88ab46-4da5-11e8-9e3f-87b875b56383.png)

    <span data-ttu-id="700d2-196">Rien ne se produit lorsque le bouton **Add todo** est sélectionné, car aucun gestionnaire d’événements n’est lié au bouton.</span><span class="sxs-lookup"><span data-stu-id="700d2-196">Nothing happens when the **Add todo** button is selected because no event handler is wired up to the button.</span></span>

1. <span data-ttu-id="700d2-197">Ajoutez une méthode `AddTodo` au composant `Todo` et enregistrez-la pour les clics sur le bouton à l’aide de l’attribut `onclick`.</span><span class="sxs-lookup"><span data-stu-id="700d2-197">Add an `AddTodo` method to the `Todo` component and register it for button clicks using the `onclick` attribute.</span></span>

    ```cshtml
    <input placeholder="Something todo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();

        void AddTodo()
        {
            // Todo: Add the todo
        }
    }
    ```

    <span data-ttu-id="700d2-198">La méthode C# `AddTodo` est appelée à chaque fois que le bouton est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="700d2-198">The `AddTodo` C# method is called every time the button is selected.</span></span>

1. <span data-ttu-id="700d2-199">Pour obtenir le titre du nouvel élément todo, ajoutez un champ de chaîne `newTodo` et liez-le à la valeur du texte d’entrée à l’aide de l’attribut `bind`.</span><span class="sxs-lookup"><span data-stu-id="700d2-199">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute.</span></span>

    ```csharp
    IList<TodoItem> todos = new List<TodoItem>();
    string newTodo;
    ```

    ```cshtml
    <input placeholder="Something todo" bind="@newTodo" />
    ```

1. <span data-ttu-id="700d2-200">Mettez à jour la méthode `AddTodo` pour ajouter `TodoItem` avec le titre spécifié à la liste.</span><span class="sxs-lookup"><span data-stu-id="700d2-200">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="700d2-201">N’oubliez pas de supprimer la valeur du texte d’entrée en définissant `newTodo` sur une chaîne vide.</span><span class="sxs-lookup"><span data-stu-id="700d2-201">Don't forget to clear the value of the text input by setting `newTodo` to an empty string.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>

    <input placeholder="Something todo" bind="@newTodo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
        string newTodo;

        void AddTodo()
        {
            if (!string.IsNullOrWhiteSpace(newTodo))
            {
                todos.Add(new TodoItem { Title = newTodo });
                newTodo = string.Empty;
            }
        }
    }
    ```

1. <span data-ttu-id="700d2-202">Actualisez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="700d2-202">Refresh the browser.</span></span> <span data-ttu-id="700d2-203">Ajoutez quelques éléments todo à la liste de tâches.</span><span class="sxs-lookup"><span data-stu-id="700d2-203">Add some todos to the todo list.</span></span>

    ![Ajouter des éléments todo](https://user-images.githubusercontent.com/1874516/39512531-2d2ff62e-4da6-11e8-8b83-291b0efc821b.png)

1. <span data-ttu-id="700d2-205">Enfin, à quoi sert une liste de tâches sans case à cocher ?</span><span class="sxs-lookup"><span data-stu-id="700d2-205">Lastly, what's a todo list without check boxes?</span></span> <span data-ttu-id="700d2-206">Le texte du titre pour chaque élément todo peut être modifié également.</span><span class="sxs-lookup"><span data-stu-id="700d2-206">The title text for each todo item can be made editable as well.</span></span> <span data-ttu-id="700d2-207">Ajoutez une entrée de case à cocher et une entrée de texte pour chaque élément todo et liez leurs valeurs aux propriétés `Title` et `IsDone`, respectivement.</span><span class="sxs-lookup"><span data-stu-id="700d2-207">Add a check box input and text input for each todo item and bind their values to the `Title` and `IsDone` properties, respectively.</span></span>

    ```cshtml
    <ul>
        @foreach (var todo in todos)
        {
            <li>
                <input type="checkbox" bind="@todo.IsDone" />
                <input bind="@todo.Title" />
            </li>
        }
    </ul>
    ```

1. <span data-ttu-id="700d2-208">Pour vérifier que ces valeurs sont liées, mettez à jour l’en-tête `h1` pour afficher le nombre d’éléments todo qui ne sont pas encore effectués (`IsDone` est `false`).</span><span class="sxs-lookup"><span data-stu-id="700d2-208">To verify that these values are bound, update the `h1` header to show a count of the number of todo items that are not yet done (`IsDone` is `false`).</span></span>

    ```cshtml
    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
    ```

1. <span data-ttu-id="700d2-209">Le composant `Todo` terminé doit ressembler à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="700d2-209">The completed `Todo` component should look like this:</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>
                <input type="checkbox" bind="@todo.IsDone" />
                <input bind="@todo.Title" />
            </li>
        }
    </ul>

    <input placeholder="Something todo" bind="@newTodo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
        string newTodo;

        void AddTodo()
        {
            if (!string.IsNullOrWhiteSpace(newTodo))
            {
                todos.Add(new TodoItem { Title = newTodo });
                newTodo = string.Empty;
            }
        }
    }
    ```

<span data-ttu-id="700d2-210">Actualisez l’application dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="700d2-210">Refresh the app in the browser.</span></span> <span data-ttu-id="700d2-211">Essayez d’ajouter quelques éléments todo.</span><span class="sxs-lookup"><span data-stu-id="700d2-211">Try adding some todo items.</span></span>

![Liste de tâches Blazor terminée](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

## <a name="publish-and-deploy"></a><span data-ttu-id="700d2-213">Publier et déployer</span><span class="sxs-lookup"><span data-stu-id="700d2-213">Publish and deploy</span></span>

<span data-ttu-id="700d2-214">Lorsque vous utilisez Visual Studio, procédez comme suit pour publier l’application Todo Blazor sur Azure :</span><span class="sxs-lookup"><span data-stu-id="700d2-214">When using Visual Studio, perform the following steps to publish the Todo Blazor app to Azure:</span></span>

1. <span data-ttu-id="700d2-215">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions**, puis sélectionnez **Publier**.</span><span class="sxs-lookup"><span data-stu-id="700d2-215">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span>

1. <span data-ttu-id="700d2-216">Dans la boîte de dialogue **Choisir une cible de publication**, sélectionnez **App Service** et **Créer**.</span><span class="sxs-lookup"><span data-stu-id="700d2-216">In the **Pick a publish target** dialog, select **App Service** and **Create New**.</span></span> <span data-ttu-id="700d2-217">Sélectionnez **Publier**.</span><span class="sxs-lookup"><span data-stu-id="700d2-217">Select **Publish**.</span></span>

    ![Choisir une cible de publication](build-your-first-blazor-app/_static/blazor-publish-pick-target.png)

1. <span data-ttu-id="700d2-219">Dans la boîte de dialogue **Créer App Service**, choisissez un nom pour l’application et sélectionnez l’abonnement, le groupe de ressources et le plan d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="700d2-219">In the **Create App Service** dialog, choose a name for the app and select the subscription, resource group, and hosting plan.</span></span> <span data-ttu-id="700d2-220">Sélectionnez **Créer** pour créer le service d’application et publier l’application.</span><span class="sxs-lookup"><span data-stu-id="700d2-220">Select **Create** to create the app service and publish the app.</span></span>

    ![Créer App Service](build-your-first-blazor-app/_static/blazor-publish-create-appservice2.png)

<span data-ttu-id="700d2-222">Patientez environ une minute pour que l’application soit déployée.</span><span class="sxs-lookup"><span data-stu-id="700d2-222">Wait a minute or so for the app to be deployed.</span></span>

<span data-ttu-id="700d2-223">L’application doit maintenant s’exécuter dans Azure.</span><span class="sxs-lookup"><span data-stu-id="700d2-223">The app should now be running in Azure.</span></span> <span data-ttu-id="700d2-224">Marquez l’élément todo pour créer votre première application Blazor comme *terminé*.</span><span class="sxs-lookup"><span data-stu-id="700d2-224">Mark the todo item to build your first Blazor app as *done*.</span></span> <span data-ttu-id="700d2-225">Bravo !</span><span class="sxs-lookup"><span data-stu-id="700d2-225">Nice job!</span></span>

![Blazor sur Azure](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

> [!NOTE]
> <span data-ttu-id="700d2-227">Si vous n’utilisez pas Visual Studio, publiez l’application Blazor à l’invite de commandes sur Windows, macOS ou Linux :</span><span class="sxs-lookup"><span data-stu-id="700d2-227">If not using Visual Studio, publish the Blazor app at a command prompt on Windows, macOS, or Linux:</span></span>
>
> ```console
> dotnet publish -c Release
> ```
>
> <span data-ttu-id="700d2-228">Le déploiement est créé dans le dossier */bin/Release/\<target-framework>/publish*.</span><span class="sxs-lookup"><span data-stu-id="700d2-228">The deployment is created in the */bin/Release/\<target-framework>/publish* folder.</span></span> <span data-ttu-id="700d2-229">Déplacez le contenu du dossier *publish* sur le serveur ou le service d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="700d2-229">Move the contents of the *publish* folder to the server or hosting service.</span></span>
>
> <span data-ttu-id="700d2-230">Pour plus d’informations, consultez la rubrique [Héberger et déployer](xref:host-and-deploy/razor-components/index#publish-the-app).</span><span class="sxs-lookup"><span data-stu-id="700d2-230">For more information, see the [Host and deploy](xref:host-and-deploy/razor-components/index#publish-the-app) topic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="700d2-231">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="700d2-231">Additional resources</span></span>

<span data-ttu-id="700d2-232">Pour un exemple d’application Blazor plus impliqué, consultez [l’exemple Flight Finder](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="700d2-232">For a more involved Blazor sample app, check out the [Flight Finder sample](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor) on GitHub.</span></span>

![Blazor Flight Finder](https://msdnshared.blob.core.windows.net/media/2018/03/blazor-flight-finder.png)
