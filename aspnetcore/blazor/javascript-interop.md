---
title: ASP.NET Core Blazor l’interopérabilité JavaScript
author: guardrex
description: Découvrez comment appeler des fonctions JavaScript à partir de méthodes .NET et .NET à partir de JavaScript dans des applications Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
no-loc:
- Blazor
- SignalR
uid: blazor/javascript-interop
ms.openlocfilehash: c4f2444b60fc2d3a8af893df379cf62636a7bdd5
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213361"
---
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a><span data-ttu-id="c05b0-103">ASP.NET Core Blazor l’interopérabilité JavaScript</span><span class="sxs-lookup"><span data-stu-id="c05b0-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="c05b0-104">Par [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c05b0-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="c05b0-105">Une application Blazor peut appeler des fonctions JavaScript à partir de méthodes .NET et .NET à partir de code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c05b0-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="c05b0-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c05b0-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="c05b0-107">Appeler des fonctions JavaScript à partir de méthodes .NET</span><span class="sxs-lookup"><span data-stu-id="c05b0-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="c05b0-108">Il arrive parfois que le code .NET soit requis pour appeler une fonction JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c05b0-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="c05b0-109">Par exemple, un appel JavaScript peut exposer des fonctionnalités de navigateur ou des fonctionnalités d’une bibliothèque JavaScript à l’application.</span><span class="sxs-lookup"><span data-stu-id="c05b0-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span> <span data-ttu-id="c05b0-110">Ce scénario s’appelle *l’interopérabilité de JavaScript* (*js Interop*).</span><span class="sxs-lookup"><span data-stu-id="c05b0-110">This scenario is called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="c05b0-111">Pour appeler JavaScript à partir de .NET, utilisez l’abstraction `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-111">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="c05b0-112">Pour émettre des appels d’interopérabilité JS, injectez le `IJSRuntime` abstraction dans votre composant.</span><span class="sxs-lookup"><span data-stu-id="c05b0-112">To issue JS interop calls, inject the `IJSRuntime` abstraction in your component.</span></span> <span data-ttu-id="c05b0-113">La méthode `InvokeAsync<T>` accepte un identificateur pour la fonction JavaScript que vous souhaitez appeler, ainsi qu’un nombre quelconque d’arguments sérialisables JSON.</span><span class="sxs-lookup"><span data-stu-id="c05b0-113">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="c05b0-114">L’identificateur de fonction est relatif à la portée globale (`window`).</span><span class="sxs-lookup"><span data-stu-id="c05b0-114">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="c05b0-115">Si vous souhaitez appeler `window.someScope.someFunction`, l’identificateur est `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-115">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="c05b0-116">Il n’est pas nécessaire d’inscrire la fonction avant qu’elle ne soit appelée.</span><span class="sxs-lookup"><span data-stu-id="c05b0-116">There's no need to register the function before it's called.</span></span> <span data-ttu-id="c05b0-117">Le type de retour `T` doit également être sérialisable JSON.</span><span class="sxs-lookup"><span data-stu-id="c05b0-117">The return type `T` must also be JSON serializable.</span></span> <span data-ttu-id="c05b0-118">`T` doit correspondre au type .NET qui correspond le mieux au type JSON retourné.</span><span class="sxs-lookup"><span data-stu-id="c05b0-118">`T` should match the .NET type that best maps to the JSON type returned.</span></span>

<span data-ttu-id="c05b0-119">Pour les applications Blazor Server avec le prérendu activé, l’appel à JavaScript n’est pas possible pendant le prérendu initial.</span><span class="sxs-lookup"><span data-stu-id="c05b0-119">For Blazor Server apps with prerendering enabled, calling into JavaScript isn't possible during the initial prerendering.</span></span> <span data-ttu-id="c05b0-120">Les appels Interop JavaScript doivent être différés jusqu’à ce que la connexion avec le navigateur soit établie.</span><span class="sxs-lookup"><span data-stu-id="c05b0-120">JavaScript interop calls must be deferred until after the connection with the browser is established.</span></span> <span data-ttu-id="c05b0-121">Pour plus d’informations, consultez la section [détecter le prérendu d’une application Blazor](#detect-when-a-blazor-app-is-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="c05b0-121">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="c05b0-122">L’exemple suivant est basé sur [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un décodeur basé sur JavaScript expérimental.</span><span class="sxs-lookup"><span data-stu-id="c05b0-122">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="c05b0-123">L’exemple montre comment appeler une fonction JavaScript à partir d' C# une méthode.</span><span class="sxs-lookup"><span data-stu-id="c05b0-123">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="c05b0-124">La fonction JavaScript accepte un tableau d’octets d' C# une méthode, décode le tableau et retourne le texte au composant pour l’affichage.</span><span class="sxs-lookup"><span data-stu-id="c05b0-124">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="c05b0-125">À l’intérieur de l’élément `<head>` de *wwwroot/index.html* (Blazor webassembly) ou de *pages/_Host. cshtml* (serveurBlazor), fournissez une fonction JavaScript qui utilise `TextDecoder` pour décoder un tableau passé et retourner la valeur décodée :</span><span class="sxs-lookup"><span data-stu-id="c05b0-125">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a JavaScript function that uses `TextDecoder` to decode a passed array and return the decoded value:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script-convertarray.html)]

<span data-ttu-id="c05b0-126">Du code JavaScript, tel que le code illustré dans l’exemple précédent, peut également être chargé à partir d’un fichier JavaScript ( *. js*) avec une référence au fichier de script :</span><span class="sxs-lookup"><span data-stu-id="c05b0-126">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="c05b0-127">Le composant suivant :</span><span class="sxs-lookup"><span data-stu-id="c05b0-127">The following component:</span></span>

* <span data-ttu-id="c05b0-128">Appelle la fonction JavaScript `convertArray` à l’aide de `JSRuntime` lorsqu’un bouton de composant (**convertir un tableau**) est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="c05b0-128">Invokes the `convertArray` JavaScript function using `JSRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="c05b0-129">Une fois la fonction JavaScript appelée, le tableau passé est converti en chaîne.</span><span class="sxs-lookup"><span data-stu-id="c05b0-129">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="c05b0-130">La chaîne est retournée au composant pour l’affichage.</span><span class="sxs-lookup"><span data-stu-id="c05b0-130">The string is returned to the component for display.</span></span>

[!code-razor[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="use-of-ijsruntime"></a><span data-ttu-id="c05b0-131">Utilisation de IJSRuntime</span><span class="sxs-lookup"><span data-stu-id="c05b0-131">Use of IJSRuntime</span></span>

<span data-ttu-id="c05b0-132">Pour utiliser l’abstraction `IJSRuntime`, adoptez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="c05b0-132">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="c05b0-133">Injecter l’abstraction `IJSRuntime` dans le composant Razor ( *. Razor*) :</span><span class="sxs-lookup"><span data-stu-id="c05b0-133">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-razor[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  <span data-ttu-id="c05b0-134">À l’intérieur de l’élément `<head>` de *wwwroot/index.html* (Blazor webassembly) ou de *pages/_Host. cshtml* (serveurBlazor), fournissez une fonction JavaScript `handleTickerChanged`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-134">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="c05b0-135">La fonction est appelée avec `IJSRuntime.InvokeVoidAsync` et ne retourne pas de valeur :</span><span class="sxs-lookup"><span data-stu-id="c05b0-135">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* <span data-ttu-id="c05b0-136">Injecter l’abstraction `IJSRuntime` dans une classe ( *. cs*) :</span><span class="sxs-lookup"><span data-stu-id="c05b0-136">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  <span data-ttu-id="c05b0-137">À l’intérieur de l’élément `<head>` de *wwwroot/index.html* (Blazor webassembly) ou de *pages/_Host. cshtml* (serveurBlazor), fournissez une fonction JavaScript `handleTickerChanged`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-137">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="c05b0-138">La fonction est appelée avec `JSRuntime.InvokeAsync` et retourne une valeur :</span><span class="sxs-lookup"><span data-stu-id="c05b0-138">The function is called with `JSRuntime.InvokeAsync` and returns a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* <span data-ttu-id="c05b0-139">Pour la génération de contenu dynamique avec [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), utilisez l’attribut `[Inject]` :</span><span class="sxs-lookup"><span data-stu-id="c05b0-139">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```razor
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="c05b0-140">Dans l’exemple d’application côté client qui accompagne cette rubrique, deux fonctions JavaScript sont disponibles pour l’application qui interagit avec le DOM pour recevoir l’entrée d’utilisateur et afficher un message d’accueil :</span><span class="sxs-lookup"><span data-stu-id="c05b0-140">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="c05b0-141">`showPrompt` &ndash; génère une invite pour accepter l’entrée d’utilisateur (nom de l’utilisateur) et retourne le nom à l’appelant.</span><span class="sxs-lookup"><span data-stu-id="c05b0-141">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="c05b0-142">`displayWelcome` &ndash; assigne un message de bienvenue de l’appelant à un objet DOM avec une `id` de `welcome`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-142">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="c05b0-143">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="c05b0-143">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="c05b0-144">Placez la balise `<script>` qui fait référence au fichier JavaScript dans le fichier *wwwroot/index.html* (Blazor assembly) ou le fichier *pages/_Host. cshtml* (serveurBlazor).</span><span class="sxs-lookup"><span data-stu-id="c05b0-144">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="c05b0-145">*wwwroot/index.html* (Blazor webassembly) :</span><span class="sxs-lookup"><span data-stu-id="c05b0-145">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=22)]

<span data-ttu-id="c05b0-146">*Pages/_Host. cshtml* (serveurBlazor) :</span><span class="sxs-lookup"><span data-stu-id="c05b0-146">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=35)]

<span data-ttu-id="c05b0-147">Ne placez pas de balise `<script>` dans un fichier de composant, car la balise `<script>` ne peut pas être mise à jour dynamiquement.</span><span class="sxs-lookup"><span data-stu-id="c05b0-147">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="c05b0-148">Les méthodes .NET interagissent avec les fonctions JavaScript dans le fichier *exampleJsInterop. js* en appelant `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-148">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="c05b0-149">L’abstraction `IJSRuntime` est asynchrone pour permettre les scénarios de serveur Blazor.</span><span class="sxs-lookup"><span data-stu-id="c05b0-149">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="c05b0-150">Si l’application est une application Blazor webassembly et que vous souhaitez appeler une fonction JavaScript de manière synchrone, vous devez effectuer un cast aval pour `IJSInProcessRuntime` et appeler `Invoke<T>` à la place.</span><span class="sxs-lookup"><span data-stu-id="c05b0-150">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="c05b0-151">Nous recommandons que la plupart des bibliothèques d’interopérabilité JS utilisent les API Async pour s’assurer que les bibliothèques sont disponibles dans tous les scénarios.</span><span class="sxs-lookup"><span data-stu-id="c05b0-151">We recommend that most JS interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="c05b0-152">L’exemple d’application comprend un composant pour illustrer l’interopérabilité de JS.</span><span class="sxs-lookup"><span data-stu-id="c05b0-152">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="c05b0-153">Le composant :</span><span class="sxs-lookup"><span data-stu-id="c05b0-153">The component:</span></span>

* <span data-ttu-id="c05b0-154">Reçoit une entrée d’utilisateur via une invite JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c05b0-154">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="c05b0-155">Retourne le texte du composant pour traitement.</span><span class="sxs-lookup"><span data-stu-id="c05b0-155">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="c05b0-156">Appelle une deuxième fonction JavaScript qui interagit avec le DOM pour afficher un message d’accueil.</span><span class="sxs-lookup"><span data-stu-id="c05b0-156">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="c05b0-157">*Pages/JSInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="c05b0-157">*Pages/JSInterop.razor*:</span></span>

```razor
@page "/JSInterop"
@using BlazorSample.JsInteropClasses
@inject IJSRuntime JSRuntime

<h1>JavaScript Interop</h1>

<h2>Invoke JavaScript functions from .NET methods</h2>

<button type="button" class="btn btn-primary" @onclick="TriggerJsPrompt">
    Trigger JavaScript Prompt
</button>

<h3 id="welcome" style="color:green;font-style:italic"></h3>

@code {
    public async Task TriggerJsPrompt()
    {
        // showPrompt is implemented in wwwroot/exampleJsInterop.js
        var name = await JSRuntime.InvokeAsync<string>(
                "exampleJsFunctions.showPrompt",
                "What's your name?");
        // displayWelcome is implemented in wwwroot/exampleJsInterop.js
        await JSRuntime.InvokeVoidAsync(
                "exampleJsFunctions.displayWelcome",
                $"Hello {name}! Welcome to Blazor!");
    }
}
```

1. <span data-ttu-id="c05b0-158">Lorsque `TriggerJsPrompt` est exécuté en sélectionnant le bouton d' **invite de commandes JavaScript** du composant, la fonction JavaScript `showPrompt` fournie dans le fichier *wwwroot/exampleJsInterop. js* est appelée.</span><span class="sxs-lookup"><span data-stu-id="c05b0-158">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="c05b0-159">La fonction `showPrompt` accepte l’entrée d’utilisateur (nom de l’utilisateur), qui est codée au format HTML et renvoyée au composant.</span><span class="sxs-lookup"><span data-stu-id="c05b0-159">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="c05b0-160">Le composant stocke le nom de l’utilisateur dans une variable locale, `name`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-160">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="c05b0-161">La chaîne stockée dans `name` est incorporée dans un message de bienvenue, qui est transmis à une fonction JavaScript, `displayWelcome`, qui affiche le message de bienvenue dans une balise d’en-tête.</span><span class="sxs-lookup"><span data-stu-id="c05b0-161">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="c05b0-162">Appeler une fonction JavaScript void</span><span class="sxs-lookup"><span data-stu-id="c05b0-162">Call a void JavaScript function</span></span>

<span data-ttu-id="c05b0-163">Les fonctions JavaScript qui retournent [void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) ou [non définies](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) sont appelées avec `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-163">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-opno-locblazor-app-is-prerendering"></a><span data-ttu-id="c05b0-164">Détecter quand un Blazor application est prérendu</span><span class="sxs-lookup"><span data-stu-id="c05b0-164">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="c05b0-165">Capturer des références à des éléments</span><span class="sxs-lookup"><span data-stu-id="c05b0-165">Capture references to elements</span></span>

<span data-ttu-id="c05b0-166">Certains scénarios d’interopérabilité JS requièrent des références à des éléments HTML.</span><span class="sxs-lookup"><span data-stu-id="c05b0-166">Some JS interop scenarios require references to HTML elements.</span></span> <span data-ttu-id="c05b0-167">Par exemple, une bibliothèque d’interface utilisateur peut nécessiter une référence d’élément pour l’initialisation, ou vous devrez peut-être appeler des API de type commande sur un élément, par exemple `focus` ou `play`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-167">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="c05b0-168">Capturer des références aux éléments HTML dans un composant à l’aide de l’approche suivante :</span><span class="sxs-lookup"><span data-stu-id="c05b0-168">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="c05b0-169">Ajoutez un attribut `@ref` à l’élément HTML.</span><span class="sxs-lookup"><span data-stu-id="c05b0-169">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="c05b0-170">Définissez un champ de type `ElementReference` dont le nom correspond à la valeur de l’attribut `@ref`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-170">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="c05b0-171">L’exemple suivant illustre la capture d’une référence à l’élément `username` `<input>` :</span><span class="sxs-lookup"><span data-stu-id="c05b0-171">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```razor
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> <span data-ttu-id="c05b0-172">Utilisez uniquement une référence d’élément pour muter le contenu d’un élément vide qui n’interagit pas avec Blazor.</span><span class="sxs-lookup"><span data-stu-id="c05b0-172">Only use an element reference to mutate the contents of an empty element that doesn't interact with Blazor.</span></span> <span data-ttu-id="c05b0-173">Ce scénario est utile quand une API tierce fournit du contenu à l’élément.</span><span class="sxs-lookup"><span data-stu-id="c05b0-173">This scenario is useful when a 3rd party API supplies content to the element.</span></span> <span data-ttu-id="c05b0-174">Étant donné que Blazor n’interagit pas avec l’élément, il n’existe aucun risque de conflit entre la représentation de Blazorde l’élément et le DOM.</span><span class="sxs-lookup"><span data-stu-id="c05b0-174">Because Blazor doesn't interact with the element, there's no possibility of a conflict between Blazor's representation of the element and the DOM.</span></span>
>
> <span data-ttu-id="c05b0-175">Dans l’exemple suivant, il est *dangereux* de faire muter le contenu de la liste non triée (`ul`), car Blazor interagit avec le DOM pour remplir les éléments de liste de cet élément (`<li>`) :</span><span class="sxs-lookup"><span data-stu-id="c05b0-175">In the following example, it's *dangerous* to mutate the contents of the unordered list (`ul`) because Blazor interacts with the DOM to populate this element's list items (`<li>`):</span></span>
>
> ```razor
> <ul ref="MyList">
>     @foreach (var item in Todos)
>     {
>         <li>@item.Text</li>
>     }
> </ul>
> ```
>
> <span data-ttu-id="c05b0-176">Si l’interopérabilité JS muter le contenu de l’élément `MyList` et Blazor tente d’appliquer des différences à l’élément, les différences ne correspondent pas au DOM.</span><span class="sxs-lookup"><span data-stu-id="c05b0-176">If JS interop mutates the contents of element `MyList` and Blazor attempts to apply diffs to the element, the diffs won't match the DOM.</span></span>

<span data-ttu-id="c05b0-177">En ce qui concerne le code .NET, un `ElementReference` est un handle opaque.</span><span class="sxs-lookup"><span data-stu-id="c05b0-177">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="c05b0-178">La *seule* chose que vous pouvez faire avec `ElementReference` est de la passer au code JavaScript via l’interopérabilité js.</span><span class="sxs-lookup"><span data-stu-id="c05b0-178">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JS interop.</span></span> <span data-ttu-id="c05b0-179">Dans ce cas, le code JavaScript reçoit une instance `HTMLElement`, qu’il peut utiliser avec les API DOM normales.</span><span class="sxs-lookup"><span data-stu-id="c05b0-179">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="c05b0-180">Par exemple, le code suivant définit une méthode d’extension .NET qui permet de définir le focus sur un élément :</span><span class="sxs-lookup"><span data-stu-id="c05b0-180">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="c05b0-181">*exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="c05b0-181">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="c05b0-182">Pour appeler une fonction JavaScript qui ne retourne pas de valeur, utilisez `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-182">To call a JavaScript function that doesn't return a value, use `IJSRuntime.InvokeVoidAsync`.</span></span> <span data-ttu-id="c05b0-183">Le code suivant définit le focus sur l’entrée de nom d’utilisateur en appelant la fonction JavaScript précédente avec l' `ElementReference`capturée :</span><span class="sxs-lookup"><span data-stu-id="c05b0-183">The following code sets the focus on the username input by calling the preceding JavaScript function with the captured `ElementReference`:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="c05b0-184">Pour utiliser une méthode d’extension, créez une méthode d’extension statique qui reçoit l’instance `IJSRuntime` :</span><span class="sxs-lookup"><span data-stu-id="c05b0-184">To use an extension method, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="c05b0-185">La méthode `Focus` est appelée directement sur l’objet.</span><span class="sxs-lookup"><span data-stu-id="c05b0-185">The `Focus` method is called directly on the object.</span></span> <span data-ttu-id="c05b0-186">L’exemple suivant suppose que la méthode `Focus` est disponible à partir de l’espace de noms `JsInteropClasses` :</span><span class="sxs-lookup"><span data-stu-id="c05b0-186">The following example assumes that the `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> <span data-ttu-id="c05b0-187">La variable `username` n’est remplie qu’après le rendu du composant.</span><span class="sxs-lookup"><span data-stu-id="c05b0-187">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="c05b0-188">Si un `ElementReference` non rempli est passé au code JavaScript, le code JavaScript reçoit la valeur `null`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-188">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="c05b0-189">Pour manipuler des références d’élément après que le composant a terminé le rendu (pour définir le focus initial sur un élément), utilisez les [méthodes de cycle de vie des composants OnAfterRenderAsync ou OnAfterRender](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="c05b0-189">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the [OnAfterRenderAsync or OnAfterRender component lifecycle methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="c05b0-190">Quand vous utilisez des types génériques et que vous retournez une valeur, utilisez [ValueTask\<t >](xref:System.Threading.Tasks.ValueTask`1):</span><span class="sxs-lookup"><span data-stu-id="c05b0-190">When working with generic types and returning a value, use [ValueTask\<T>](xref:System.Threading.Tasks.ValueTask`1):</span></span>

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

<span data-ttu-id="c05b0-191">`GenericMethod` est appelé directement sur l’objet avec un type.</span><span class="sxs-lookup"><span data-stu-id="c05b0-191">`GenericMethod` is called directly on the object with a type.</span></span> <span data-ttu-id="c05b0-192">L’exemple suivant suppose que la `GenericMethod` est disponible à partir de l’espace de noms `JsInteropClasses` :</span><span class="sxs-lookup"><span data-stu-id="c05b0-192">The following example assumes that the `GenericMethod` is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component3.razor?highlight=17)]

## <a name="reference-elements-across-components"></a><span data-ttu-id="c05b0-193">Éléments de référence sur les composants</span><span class="sxs-lookup"><span data-stu-id="c05b0-193">Reference elements across components</span></span>

<span data-ttu-id="c05b0-194">Un `ElementReference` est garanti uniquement valide dans la méthode `OnAfterRender` d’un composant (et une référence d’élément est un `struct`), de sorte qu’une référence d’élément ne peut pas être transmise entre les composants.</span><span class="sxs-lookup"><span data-stu-id="c05b0-194">An `ElementReference` is only guaranteed valid in a component's `OnAfterRender` method (and an element reference is a `struct`), so an element reference can't be passed between components.</span></span>

<span data-ttu-id="c05b0-195">Pour qu’un composant parent rende une référence d’élément disponible pour d’autres composants, le composant parent peut :</span><span class="sxs-lookup"><span data-stu-id="c05b0-195">For a parent component to make an element reference available to other components, the parent component can:</span></span>

* <span data-ttu-id="c05b0-196">Autorisez les composants enfants à inscrire des rappels.</span><span class="sxs-lookup"><span data-stu-id="c05b0-196">Allow child components to register callbacks.</span></span>
* <span data-ttu-id="c05b0-197">Appelez les rappels inscrits pendant l’événement `OnAfterRender` avec la référence d’élément passée.</span><span class="sxs-lookup"><span data-stu-id="c05b0-197">Invoke the registered callbacks during the `OnAfterRender` event with the passed element reference.</span></span> <span data-ttu-id="c05b0-198">Indirectement, cette approche permet aux composants enfants d’interagir avec la référence d’élément du parent.</span><span class="sxs-lookup"><span data-stu-id="c05b0-198">Indirectly, this approach allows child components to interact with the parent's element reference.</span></span>

<span data-ttu-id="c05b0-199">L’exemple de Blazor webassembly suivant illustre l’approche.</span><span class="sxs-lookup"><span data-stu-id="c05b0-199">The following Blazor WebAssembly example illustrates the approach.</span></span>

<span data-ttu-id="c05b0-200">Dans le `<head>` de *wwwroot/index.html*:</span><span class="sxs-lookup"><span data-stu-id="c05b0-200">In the `<head>` of *wwwroot/index.html*:</span></span>

```html
<style>
    .red { color: red }
</style>
```

<span data-ttu-id="c05b0-201">Dans le `<body>` de *wwwroot/index.html*:</span><span class="sxs-lookup"><span data-stu-id="c05b0-201">In the `<body>` of *wwwroot/index.html*:</span></span>

```html
<script>
    function setElementClass(element, className) {
        /** @type {HTMLElement} **/
        var myElement = element;
        myElement.classList.add(className);
    }
</script>
```

<span data-ttu-id="c05b0-202">*Pages/index. Razor* (composant parent) :</span><span class="sxs-lookup"><span data-stu-id="c05b0-202">*Pages/Index.razor* (parent component):</span></span>

```razor
@page "/"

<h1 @ref="_title">Hello, world!</h1>

Welcome to your new app.

<SurveyPrompt Parent="this" Title="How is Blazor working for you?" />
```

<span data-ttu-id="c05b0-203">*Pages/index. Razor. cs*:</span><span class="sxs-lookup"><span data-stu-id="c05b0-203">*Pages/Index.razor.cs*:</span></span>

```csharp
using System;
using System.Collections.Generic;
using Microsoft.AspNetCore.Components;

namespace BlazorSample.Pages
{
    public partial class Index : 
        ComponentBase, IObservable<ElementReference>, IDisposable
    {
        private bool _disposing;
        private IList<IObserver<ElementReference>> _subscriptions = 
            new List<IObserver<ElementReference>>();
        private ElementReference _title;

        protected override void OnAfterRender(bool firstRender)
        {
            base.OnAfterRender(firstRender);

            foreach (var subscription in _subscriptions)
            {
                try
                {
                    subscription.OnNext(_title);
                }
                catch (Exception)
                {
                    throw;
                }
            }
        }

        public void Dispose()
        {
            _disposing = true;

            foreach (var subscription in _subscriptions)
            {
                try
                {
                    subscription.OnCompleted();
                }
                catch (Exception)
                {
                }
            }

            _subscriptions.Clear();
        }

        public IDisposable Subscribe(IObserver<ElementReference> observer)
        {
            if (_disposing)
            {
                throw new InvalidOperationException("Parent being disposed");
            }

            _subscriptions.Add(observer);

            return new Subscription(observer, this);
        }

        private class Subscription : IDisposable
        {
            public Subscription(IObserver<ElementReference> observer, Index self)
            {
                Observer = observer;
                Self = self;
            }

            public IObserver<ElementReference> Observer { get; }
            public Index Self { get; }

            public void Dispose()
            {
                Self._subscriptions.Remove(Observer);
            }
        }
    }
}
```

<span data-ttu-id="c05b0-204">*Shared/SurveyPrompt. Razor* (composant enfant) :</span><span class="sxs-lookup"><span data-stu-id="c05b0-204">*Shared/SurveyPrompt.razor* (child component):</span></span>

```razor
@inject IJSRuntime JS

<div class="alert alert-secondary mt-4" role="alert">
    <span class="oi oi-pencil mr-2" aria-hidden="true"></span>
    <strong>@Title</strong>

    <span class="text-nowrap">
        Please take our
        <a target="_blank" class="font-weight-bold" 
            href="https://go.microsoft.com/fwlink/?linkid=2109206">brief survey</a>
    </span>
    and tell us what you think.
</div>

@code {
    [Parameter]
    public string Title { get; set; }
}
```

<span data-ttu-id="c05b0-205">*Shared/SurveyPrompt. Razor. cs*:</span><span class="sxs-lookup"><span data-stu-id="c05b0-205">*Shared/SurveyPrompt.razor.cs*:</span></span>

```csharp
using System;
using Microsoft.AspNetCore.Components;

namespace BlazorSample.Shared
{
    public partial class SurveyPrompt : 
        ComponentBase, IObserver<ElementReference>, IDisposable
    {
        private IDisposable _subscription = null;

        [Parameter]
        public IObservable<ElementReference> Parent { get; set; }

        protected override void OnParametersSet()
        {
            base.OnParametersSet();

            if (_subscription != null)
            {
                _subscription.Dispose();
            }

            _subscription = Parent.Subscribe(this);
        }

        public void OnCompleted()
        {
            _subscription = null;
        }

        public void OnError(Exception error)
        {
            _subscription = null;
        }

        public void OnNext(ElementReference value)
        {
            JS.InvokeAsync<object>(
                "setElementClass", new object[] { value, "red" });
        }

        public void Dispose()
        {
            _subscription?.Dispose();
        }
    }
}
```

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="c05b0-206">Appeler des méthodes .NET à partir de fonctions JavaScript</span><span class="sxs-lookup"><span data-stu-id="c05b0-206">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="c05b0-207">Appel de méthode .NET statique</span><span class="sxs-lookup"><span data-stu-id="c05b0-207">Static .NET method call</span></span>

<span data-ttu-id="c05b0-208">Pour appeler une méthode .NET statique à partir de JavaScript, utilisez les fonctions `DotNet.invokeMethod` ou `DotNet.invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-208">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="c05b0-209">Transmettez l’identificateur de la méthode statique que vous souhaitez appeler, le nom de l’assembly contenant la fonction et les arguments éventuels.</span><span class="sxs-lookup"><span data-stu-id="c05b0-209">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="c05b0-210">La version asynchrone est recommandée pour prendre en charge les scénarios de serveur Blazor.</span><span class="sxs-lookup"><span data-stu-id="c05b0-210">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="c05b0-211">La méthode .NET doit être publique, statique et avoir l’attribut `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-211">The .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="c05b0-212">L’appel de méthodes génériques ouvertes n’est pas pris en charge actuellement.</span><span class="sxs-lookup"><span data-stu-id="c05b0-212">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="c05b0-213">L’exemple d’application comprend C# une méthode pour retourner un tableau de `int`s.</span><span class="sxs-lookup"><span data-stu-id="c05b0-213">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="c05b0-214">L’attribut `JSInvokable` est appliqué à la méthode.</span><span class="sxs-lookup"><span data-stu-id="c05b0-214">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="c05b0-215">*Pages/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="c05b0-215">*Pages/JsInterop.razor*:</span></span>

```razor
<button type="button" class="btn btn-primary"
        onclick="exampleJsFunctions.returnArrayAsyncJs()">
    Trigger .NET static method ReturnArrayAsync
</button>

@code {
    [JSInvokable]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

<span data-ttu-id="c05b0-216">JavaScript traité au client appelle la C# méthode .net.</span><span class="sxs-lookup"><span data-stu-id="c05b0-216">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="c05b0-217">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="c05b0-217">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="c05b0-218">Quand le bouton déclencher l’ReturnArrayAsync de la **méthode statique .net** est sélectionné, examinez la sortie de la console dans les outils de développement Web du navigateur.</span><span class="sxs-lookup"><span data-stu-id="c05b0-218">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="c05b0-219">La sortie de la console est la suivante :</span><span class="sxs-lookup"><span data-stu-id="c05b0-219">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="c05b0-220">La quatrième valeur de tableau fait l’objet d’un push dans le tableau (`data.push(4);`) retourné par `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-220">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

<span data-ttu-id="c05b0-221">Par défaut, l’identificateur de méthode est le nom de la méthode, mais vous pouvez spécifier un identificateur différent à l’aide du constructeur `JSInvokableAttribute` :</span><span class="sxs-lookup"><span data-stu-id="c05b0-221">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor:</span></span>

```csharp
@code {
    [JSInvokable("DifferentMethodName")]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

<span data-ttu-id="c05b0-222">Dans le fichier JavaScript côté client :</span><span class="sxs-lookup"><span data-stu-id="c05b0-222">In the client-side JavaScript file:</span></span>

```javascript
returnArrayAsyncJs: function () {
  DotNet.invokeMethodAsync('BlazorSample', 'DifferentMethodName')
    .then(data => {
      data.push(4);
      console.log(data);
    });
}
```

### <a name="instance-method-call"></a><span data-ttu-id="c05b0-223">Appel de méthode d’instance</span><span class="sxs-lookup"><span data-stu-id="c05b0-223">Instance method call</span></span>

<span data-ttu-id="c05b0-224">Vous pouvez également appeler des méthodes d’instance .NET à partir de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c05b0-224">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="c05b0-225">Pour appeler une méthode d’instance .NET à partir de JavaScript :</span><span class="sxs-lookup"><span data-stu-id="c05b0-225">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="c05b0-226">Transmettez l’instance .NET à JavaScript en l’encapsulant dans une instance de `DotNetObjectReference`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-226">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectReference` instance.</span></span> <span data-ttu-id="c05b0-227">L’instance .NET est passée par référence à JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c05b0-227">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="c05b0-228">Appeler des méthodes d’instance .NET sur l’instance à l’aide des fonctions `invokeMethod` ou `invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-228">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="c05b0-229">L’instance .NET peut également être passée comme argument lors de l’appel d’autres méthodes .NET à partir de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c05b0-229">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="c05b0-230">L’exemple d’application enregistre les messages dans la console côté client.</span><span class="sxs-lookup"><span data-stu-id="c05b0-230">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="c05b0-231">Pour les exemples suivants présentés dans l’exemple d’application, examinez la sortie de console du navigateur dans les outils de développement du navigateur.</span><span class="sxs-lookup"><span data-stu-id="c05b0-231">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="c05b0-232">Quand le bouton de **méthode d’instance .net de déclenchement HelloHelper. SayHello** est sélectionné, `ExampleJsInterop.CallHelloHelperSayHello` est appelé et passe un nom, `Blazor`, à la méthode.</span><span class="sxs-lookup"><span data-stu-id="c05b0-232">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="c05b0-233">*Pages/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="c05b0-233">*Pages/JsInterop.razor*:</span></span>

```razor
<button type="button" class="btn btn-primary" @onclick="TriggerNetInstanceMethod">
    Trigger .NET instance method HelloHelper.SayHello
</button>

@code {
    public async Task TriggerNetInstanceMethod()
    {
        var exampleJsInterop = new ExampleJsInterop(JSRuntime);
        await exampleJsInterop.CallHelloHelperSayHello("Blazor");
    }
}
```

<span data-ttu-id="c05b0-234">`CallHelloHelperSayHello` appelle la fonction JavaScript `sayHello` avec une nouvelle instance de `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-234">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="c05b0-235">*JsInteropClasses/ExampleJsInterop. cs*:</span><span class="sxs-lookup"><span data-stu-id="c05b0-235">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="c05b0-236">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="c05b0-236">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="c05b0-237">Le nom est passé au constructeur de `HelloHelper`, qui définit la propriété `HelloHelper.Name`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-237">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="c05b0-238">Lorsque la fonction JavaScript `sayHello` est exécutée, `HelloHelper.SayHello` retourne le message `Hello, {Name}!`, qui est écrit dans la console par la fonction JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c05b0-238">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="c05b0-239">*JsInteropClasses/HelloHelper. cs*:</span><span class="sxs-lookup"><span data-stu-id="c05b0-239">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="c05b0-240">Sortie de la console dans les outils de développement Web du navigateur :</span><span class="sxs-lookup"><span data-stu-id="c05b0-240">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="c05b0-241">Partager du code d’interopérabilité dans une bibliothèque de classes</span><span class="sxs-lookup"><span data-stu-id="c05b0-241">Share interop code in a class library</span></span>

<span data-ttu-id="c05b0-242">Le code d’interopérabilité JS peut être inclus dans une bibliothèque de classes, ce qui vous permet de partager le code dans un package NuGet.</span><span class="sxs-lookup"><span data-stu-id="c05b0-242">JS interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="c05b0-243">La bibliothèque de classes gère l’incorporation des ressources JavaScript dans l’assembly généré.</span><span class="sxs-lookup"><span data-stu-id="c05b0-243">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="c05b0-244">Les fichiers JavaScript sont placés dans le dossier *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="c05b0-244">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="c05b0-245">Les outils s’occupent de l’incorporation des ressources lors de la génération de la bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="c05b0-245">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="c05b0-246">Le package NuGet créé est référencé dans le fichier projet de l’application de la même façon que n’importe quel package NuGet.</span><span class="sxs-lookup"><span data-stu-id="c05b0-246">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="c05b0-247">Une fois le package restauré, le code d’application peut appeler JavaScript comme s’il C#avait été.</span><span class="sxs-lookup"><span data-stu-id="c05b0-247">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="c05b0-248">Pour plus d’informations, consultez <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="c05b0-248">For more information, see <xref:blazor/class-libraries>.</span></span>

## <a name="harden-js-interop-calls"></a><span data-ttu-id="c05b0-249">Sécuriser les appels d’interopérabilité JS</span><span class="sxs-lookup"><span data-stu-id="c05b0-249">Harden JS interop calls</span></span>

<span data-ttu-id="c05b0-250">L’interopérabilité de JS peut échouer en raison d’erreurs réseau et doit être considérée comme non fiable.</span><span class="sxs-lookup"><span data-stu-id="c05b0-250">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="c05b0-251">Par défaut, une application Blazor Server expire les appels d’interopérabilité JS sur le serveur après une minute.</span><span class="sxs-lookup"><span data-stu-id="c05b0-251">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="c05b0-252">Si une application peut tolérer un délai d’expiration plus agressif, par exemple 10 secondes, définissez le délai d’expiration à l’aide de l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="c05b0-252">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="c05b0-253">Globalement dans `Startup.ConfigureServices`, spécifiez le délai d’expiration :</span><span class="sxs-lookup"><span data-stu-id="c05b0-253">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="c05b0-254">Par appel dans le code du composant, un appel unique peut spécifier le délai d’attente :</span><span class="sxs-lookup"><span data-stu-id="c05b0-254">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="c05b0-255">Pour plus d’informations sur l’épuisement des ressources, consultez <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="c05b0-255">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>

## <a name="perform-large-data-transfers-in-opno-locblazor-server-apps"></a><span data-ttu-id="c05b0-256">Effectuer des transferts de données volumineux dans des applications Blazor Server</span><span class="sxs-lookup"><span data-stu-id="c05b0-256">Perform large data transfers in Blazor Server apps</span></span>

<span data-ttu-id="c05b0-257">Dans certains scénarios, de grandes quantités de données doivent être transférées entre JavaScript et Blazor.</span><span class="sxs-lookup"><span data-stu-id="c05b0-257">In some scenarios, large amounts of data must be transferred between JavaScript and Blazor.</span></span> <span data-ttu-id="c05b0-258">En général, les transferts de données volumineux se produisent dans les cas suivants :</span><span class="sxs-lookup"><span data-stu-id="c05b0-258">Typically, large data transfers occur when:</span></span>

* <span data-ttu-id="c05b0-259">Les API du système de fichiers du navigateur sont utilisées pour charger ou télécharger un fichier.</span><span class="sxs-lookup"><span data-stu-id="c05b0-259">Browser file system APIs are used to upload or download a file.</span></span>
* <span data-ttu-id="c05b0-260">L’interopérabilité avec une bibliothèque tierce est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="c05b0-260">Interop with a third party library is required.</span></span>

<span data-ttu-id="c05b0-261">Dans Blazor Server, une limitation est en place pour empêcher le passage de messages volumineux uniques susceptibles d’entraîner des problèmes de performances.</span><span class="sxs-lookup"><span data-stu-id="c05b0-261">In Blazor Server, a limitation is in place to prevent passing single large messages that may result in performance issues.</span></span>

<span data-ttu-id="c05b0-262">Tenez compte des conseils suivants lors du développement de code qui transfère des données entre JavaScript et Blazor:</span><span class="sxs-lookup"><span data-stu-id="c05b0-262">Consider the following guidance when developing code that transfers data between JavaScript and Blazor:</span></span>

* <span data-ttu-id="c05b0-263">Découpez les données en éléments plus petits et envoyez les segments de données de façon séquentielle jusqu’à ce que toutes les données soient reçues par le serveur.</span><span class="sxs-lookup"><span data-stu-id="c05b0-263">Slice the data into smaller pieces, and send the data segments sequentially until all of the data is received by the server.</span></span>
* <span data-ttu-id="c05b0-264">N’allouez pas d’objets volumineux C# en JavaScript et dans du code.</span><span class="sxs-lookup"><span data-stu-id="c05b0-264">Don't allocate large objects in JavaScript and C# code.</span></span>
* <span data-ttu-id="c05b0-265">Ne bloquez pas le thread d’interface utilisateur principal pendant de longues périodes lors de l’envoi ou de la réception de données.</span><span class="sxs-lookup"><span data-stu-id="c05b0-265">Don't block the main UI thread for long periods when sending or receiving data.</span></span>
* <span data-ttu-id="c05b0-266">Libérez la mémoire consommée lorsque le processus est terminé ou annulé.</span><span class="sxs-lookup"><span data-stu-id="c05b0-266">Free any memory consumed when the process is completed or cancelled.</span></span>
* <span data-ttu-id="c05b0-267">Appliquer les exigences supplémentaires suivantes pour des raisons de sécurité :</span><span class="sxs-lookup"><span data-stu-id="c05b0-267">Enforce the following additional requirements for security purposes:</span></span>
  * <span data-ttu-id="c05b0-268">Déclarez la taille maximale du fichier ou des données qui peut être passée.</span><span class="sxs-lookup"><span data-stu-id="c05b0-268">Declare the maximum file or data size that can be passed.</span></span>
  * <span data-ttu-id="c05b0-269">Déclarez le taux de téléchargement minimal du client au serveur.</span><span class="sxs-lookup"><span data-stu-id="c05b0-269">Declare the minimum upload rate from the client to the server.</span></span>
* <span data-ttu-id="c05b0-270">Une fois les données reçues par le serveur, les données peuvent être :</span><span class="sxs-lookup"><span data-stu-id="c05b0-270">After the data is received by the server, the data can be:</span></span>
  * <span data-ttu-id="c05b0-271">Stocké temporairement dans une mémoire tampon jusqu’à ce que tous les segments soient collectés.</span><span class="sxs-lookup"><span data-stu-id="c05b0-271">Temporarily stored in a memory buffer until all of the segments are collected.</span></span>
  * <span data-ttu-id="c05b0-272">Consommé immédiatement.</span><span class="sxs-lookup"><span data-stu-id="c05b0-272">Consumed immediately.</span></span> <span data-ttu-id="c05b0-273">Par exemple, les données peuvent être stockées immédiatement dans une base de données ou écrites sur le disque à mesure que chaque segment est reçu.</span><span class="sxs-lookup"><span data-stu-id="c05b0-273">For example, the data can be stored immediately in a database or written to disk as each segment is received.</span></span>

<span data-ttu-id="c05b0-274">La classe de chargeur de fichiers suivante gère l’interopérabilité de JS avec le client.</span><span class="sxs-lookup"><span data-stu-id="c05b0-274">The following file uploader class handles JS interop with the client.</span></span> <span data-ttu-id="c05b0-275">La classe de chargeur utilise l’interopérabilité JS pour :</span><span class="sxs-lookup"><span data-stu-id="c05b0-275">The uploader class uses JS interop to:</span></span>

* <span data-ttu-id="c05b0-276">Interroger le client pour envoyer un segment de données.</span><span class="sxs-lookup"><span data-stu-id="c05b0-276">Poll the client to send a data segment.</span></span>
* <span data-ttu-id="c05b0-277">Abandonner la transaction si le délai d’interrogation est dépassé.</span><span class="sxs-lookup"><span data-stu-id="c05b0-277">Abort the transaction if polling times out.</span></span>

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;
using Microsoft.JSInterop;

public class FileUploader : IDisposable
{
    private readonly IJSRuntime _jsRuntime;
    private readonly int _segmentSize = 6144;
    private readonly int _maxBase64SegmentSize = 8192;
    private readonly DotNetObjectReference<FileUploader> _thisReference;
    private List<IMemoryOwner<byte>> _uploadedSegments = 
        new List<IMemoryOwner<byte>>();

    public FileUploader(IJSRuntime jsRuntime)
    {
        _jsRuntime = jsRuntime;
    }

    public async Task<Stream> ReceiveFile(string selector, int maxSize)
    {
        var fileSize = 
            await _jsRuntime.InvokeAsync<int>("getFileSize", selector);

        if (fileSize > maxSize)
        {
            return null;
        }

        var numberOfSegments = Math.Floor(fileSize / (double)_segmentSize) + 1;
        var lastSegmentBytes = 0;
        string base64EncodedSegment;

        for (var i = 0; i < numberOfSegments; i++)
        {
            try
            {
                base64EncodedSegment = 
                    await _jsRuntime.InvokeAsync<string>(
                        "receiveSegment", i, selector);

                if (base64EncodedSegment.Length < _maxBase64SegmentSize && 
                    i < numberOfSegments - 1)
                {
                    return null;
                }
            }
            catch
            {
                return null;
            }

          var current = MemoryPool<byte>.Shared.Rent(_segmentSize);

          if (!Convert.TryFromBase64String(base64EncodedSegment, 
              current.Memory.Slice(0, _segmentSize).Span, out lastSegmentBytes))
          {
              return null;
          }

          _uploadedSegments.Add(current);
        }

        var segments = _uploadedSegments;
        _uploadedSegments = null;

        return new SegmentedStream(segments, _segmentSize, lastSegmentBytes);
    }

    public void Dispose()
    {
        if (_uploadedSegments != null)
        {
            foreach (var segment in _uploadedSegments)
            {
                segment.Dispose();
            }
        }
    }
}
```

<span data-ttu-id="c05b0-278">Dans l'exemple précédent :</span><span class="sxs-lookup"><span data-stu-id="c05b0-278">In the preceding example:</span></span>

* <span data-ttu-id="c05b0-279">Le `_maxBase64SegmentSize` est défini sur `8192`, calculé à partir de `_maxBase64SegmentSize = _segmentSize * 4 / 3`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-279">The `_maxBase64SegmentSize` is set to `8192`, which is calculated from `_maxBase64SegmentSize = _segmentSize * 4 / 3`.</span></span>
* <span data-ttu-id="c05b0-280">Les API de gestion de mémoire .NET Core de bas niveau sont utilisées pour stocker les segments de mémoire sur le serveur dans `_uploadedSegments`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-280">Low level .NET Core memory management APIs are used to store the memory segments on the server in `_uploadedSegments`.</span></span>
* <span data-ttu-id="c05b0-281">Une méthode `ReceiveFile` est utilisée pour gérer le téléchargement via l’interopérabilité JS :</span><span class="sxs-lookup"><span data-stu-id="c05b0-281">A `ReceiveFile` method is used to handle the upload through JS interop:</span></span>
  * <span data-ttu-id="c05b0-282">La taille du fichier est déterminée en octets par le biais de l’interopérabilité JS avec `_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-282">The file size is determined in bytes through JS interop with `_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`.</span></span>
  * <span data-ttu-id="c05b0-283">Le nombre de segments à recevoir est calculé et stocké dans `numberOfSegments`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-283">The number of segments to receive are calculated and stored in `numberOfSegments`.</span></span>
  * <span data-ttu-id="c05b0-284">Les segments sont demandés dans une boucle `for` via l’interopérabilité JS avec `_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`.</span><span class="sxs-lookup"><span data-stu-id="c05b0-284">The segments are requested in a `for` loop through JS interop with `_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`.</span></span> <span data-ttu-id="c05b0-285">Tous les segments, mais le dernier doit être de 8 192 octets avant le décodage.</span><span class="sxs-lookup"><span data-stu-id="c05b0-285">All segments but the last must be 8,192 bytes before decoding.</span></span> <span data-ttu-id="c05b0-286">Le client est contraint d’envoyer les données de manière efficace.</span><span class="sxs-lookup"><span data-stu-id="c05b0-286">The client is forced to send the data in an efficient manner.</span></span>
  * <span data-ttu-id="c05b0-287">Pour chaque segment reçu, les vérifications sont effectuées avant le décodage avec <xref:System.Convert.TryFromBase64String*>.</span><span class="sxs-lookup"><span data-stu-id="c05b0-287">For each segment received, checks are performed before decoding with <xref:System.Convert.TryFromBase64String*>.</span></span>
  * <span data-ttu-id="c05b0-288">Un flux avec les données est retourné sous la forme d’un nouveau <xref:System.IO.Stream> (`SegmentedStream`) une fois le téléchargement terminé.</span><span class="sxs-lookup"><span data-stu-id="c05b0-288">A stream with the data is returned as a new <xref:System.IO.Stream> (`SegmentedStream`) after the upload is complete.</span></span>

<span data-ttu-id="c05b0-289">La classe de flux segmentée expose la liste de segments sous la forme d’un <xref:System.IO.Stream>ReadOnly non identifiable :</span><span class="sxs-lookup"><span data-stu-id="c05b0-289">The segmented stream class exposes the list of segments as a readonly non-seekable <xref:System.IO.Stream>:</span></span>

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;

public class SegmentedStream : Stream
{
    private readonly ReadOnlySequence<byte> _sequence;
    private long _currentPosition = 0;

    public SegmentedStream(IList<IMemoryOwner<byte>> segments, int segmentSize, 
        int lastSegmentSize)
    {
        if (segments.Count == 1)
        {
            _sequence = new ReadOnlySequence<byte>(
                segments[0].Memory.Slice(0, lastSegmentSize));
            return;
        }

        var sequenceSegment = new BufferSegment<byte>(
            segments[0].Memory.Slice(0, segmentSize));
        var lastSegment = sequenceSegment;

        for (int i = 1; i < segments.Count; i++)
        {
            var isLastSegment = i + 1 == segments.Count;
            lastSegment = lastSegment.Append(segments[i].Memory.Slice(
                0, isLastSegment ? lastSegmentSize : segmentSize));
        }

        _sequence = new ReadOnlySequence<byte>(
            sequenceSegment, 0, lastSegment, lastSegmentSize);
    }

    public override long Position
    {
        get => throw new NotImplementedException();
        set => throw new NotImplementedException();
    }

    public override int Read(byte[] buffer, int offset, int count)
    {
        var bytesToWrite = (int)(_currentPosition + count < _sequence.Length ? 
            count : _sequence.Length - _currentPosition);
        var data = _sequence.Slice(_currentPosition, bytesToWrite);
        data.CopyTo(buffer.AsSpan(offset, bytesToWrite));
        _currentPosition += bytesToWrite;

        return bytesToWrite;
    }

    private class BufferSegment<T> : ReadOnlySequenceSegment<T>
    {
        public BufferSegment(ReadOnlyMemory<T> memory)
        {
            Memory = memory;
        }

        public BufferSegment<T> Append(ReadOnlyMemory<T> memory)
        {
            var segment = new BufferSegment<T>(memory)
            {
                RunningIndex = RunningIndex + Memory.Length
            };

            Next = segment;

            return segment;
        }
    }

    public override bool CanRead => true;

    public override bool CanSeek => false;

    public override bool CanWrite => false;

    public override long Length => throw new NotImplementedException();

    public override void Flush() => throw new NotImplementedException();

    public override long Seek(long offset, SeekOrigin origin) => 
        throw new NotImplementedException();

    public override void SetLength(long value) => 
        throw new NotImplementedException();

    public override void Write(byte[] buffer, int offset, int count) => 
        throw new NotImplementedException();
}
```

<span data-ttu-id="c05b0-290">Le code suivant implémente les fonctions JavaScript pour recevoir les données :</span><span class="sxs-lookup"><span data-stu-id="c05b0-290">The following code implements JavaScript functions to receive the data:</span></span>

```javascript
function getFileSize(selector) {
  const file = getFile(selector);
  return file.size;
}

async function receiveSegment(segmentNumber, selector) {
  const file = getFile(selector);
  var segments = getFileSegments(file);
  var index = segmentNumber * 6144;
  return await getNextChunk(file, index);
}

function getFile(selector) {
  const element = document.querySelector(selector);
  if (!element) {
    throw new Error('Invalid selector');
  }
  const files = element.files;
  if (!files || files.length === 0) {
    throw new Error(`Element ${elementId} doesn't contain any files.`);
  }
  const file = files[0];
  return file;
}

function getFileSegments(file) {
  const segments = Math.floor(size % 6144 === 0 ? size / 6144 : 1 + size / 6144);
  return segments;
}

async function getNextChunk(file, index) {
  const length = file.size - index <= 6144 ? file.size - index : 6144;
  const chunk = file.slice(index, index + length);
  index += length;
  const base64Chunk = await this.base64EncodeAsync(chunk);
  return { base64Chunk, index };
}

async function base64EncodeAsync(chunk) {
  const reader = new FileReader();
  const result = new Promise((resolve, reject) => {
    reader.addEventListener('load',
      () => {
        const base64Chunk = reader.result;
        const cleanChunk = 
          base64Chunk.replace('data:application/octet-stream;base64,', '');
        resolve(cleanChunk);
      },
      false);
    reader.addEventListener('error', reject);
  });
  reader.readAsDataURL(chunk);
  return result;
}
```

## <a name="additional-resources"></a><span data-ttu-id="c05b0-291">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="c05b0-291">Additional resources</span></span>

* [<span data-ttu-id="c05b0-292">Exemple InteropComponent. Razor (référentiel GitHub dotnet/AspNetCore, branche de version 3,1)</span><span class="sxs-lookup"><span data-stu-id="c05b0-292">InteropComponent.razor example (dotnet/AspNetCore GitHub repository, 3.1 release branch)</span></span>](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
