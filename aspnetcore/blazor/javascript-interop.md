---
title: ASP.NET Core Blazor l’interopérabilité JavaScript
author: guardrex
description: Découvrez comment appeler des fonctions JavaScript à partir de méthodes .NET et .NET à partir de JavaScript dans des applications Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/javascript-interop
ms.openlocfilehash: 4edef123bc1fe41845b8060b9c3b8e77ffd2969d
ms.sourcegitcommit: c81ef12a1b6e6ac838e5e07042717cf492e6635b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76885470"
---
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a><span data-ttu-id="2a714-103">ASP.NET Core Blazor l’interopérabilité JavaScript</span><span class="sxs-lookup"><span data-stu-id="2a714-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="2a714-104">Par [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2a714-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="2a714-105">Une application Blazor peut appeler des fonctions JavaScript à partir de méthodes .NET et .NET à partir de code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2a714-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="2a714-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2a714-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="2a714-107">Appeler des fonctions JavaScript à partir de méthodes .NET</span><span class="sxs-lookup"><span data-stu-id="2a714-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="2a714-108">Il arrive parfois que le code .NET soit requis pour appeler une fonction JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2a714-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="2a714-109">Par exemple, un appel JavaScript peut exposer des fonctionnalités de navigateur ou des fonctionnalités d’une bibliothèque JavaScript à l’application.</span><span class="sxs-lookup"><span data-stu-id="2a714-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span> <span data-ttu-id="2a714-110">Ce scénario s’appelle *l’interopérabilité de JavaScript* (*js Interop*).</span><span class="sxs-lookup"><span data-stu-id="2a714-110">This scenario is called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="2a714-111">Pour appeler JavaScript à partir de .NET, utilisez l’abstraction `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="2a714-111">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="2a714-112">Pour émettre des appels d’interopérabilité JS, injectez le `IJSRuntime` abstraction dans votre composant.</span><span class="sxs-lookup"><span data-stu-id="2a714-112">To issue JS interop calls, inject the `IJSRuntime` abstraction in your component.</span></span> <span data-ttu-id="2a714-113">La méthode `InvokeAsync<T>` accepte un identificateur pour la fonction JavaScript que vous souhaitez appeler, ainsi qu’un nombre quelconque d’arguments sérialisables JSON.</span><span class="sxs-lookup"><span data-stu-id="2a714-113">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="2a714-114">L’identificateur de fonction est relatif à la portée globale (`window`).</span><span class="sxs-lookup"><span data-stu-id="2a714-114">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="2a714-115">Si vous souhaitez appeler `window.someScope.someFunction`, l’identificateur est `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="2a714-115">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="2a714-116">Il n’est pas nécessaire d’inscrire la fonction avant qu’elle ne soit appelée.</span><span class="sxs-lookup"><span data-stu-id="2a714-116">There's no need to register the function before it's called.</span></span> <span data-ttu-id="2a714-117">Le type de retour `T` doit également être sérialisable JSON.</span><span class="sxs-lookup"><span data-stu-id="2a714-117">The return type `T` must also be JSON serializable.</span></span> <span data-ttu-id="2a714-118">`T` doit correspondre au type .NET qui correspond le mieux au type JSON retourné.</span><span class="sxs-lookup"><span data-stu-id="2a714-118">`T` should match the .NET type that best maps to the JSON type returned.</span></span>

<span data-ttu-id="2a714-119">Pour les applications Blazor Server avec le prérendu activé, l’appel à JavaScript n’est pas possible pendant le prérendu initial.</span><span class="sxs-lookup"><span data-stu-id="2a714-119">For Blazor Server apps with prerendering enabled, calling into JavaScript isn't possible during the initial prerendering.</span></span> <span data-ttu-id="2a714-120">Les appels Interop JavaScript doivent être différés jusqu’à ce que la connexion avec le navigateur soit établie.</span><span class="sxs-lookup"><span data-stu-id="2a714-120">JavaScript interop calls must be deferred until after the connection with the browser is established.</span></span> <span data-ttu-id="2a714-121">Pour plus d’informations, consultez la section [détecter le prérendu d’une application Blazor](#detect-when-a-blazor-app-is-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="2a714-121">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="2a714-122">L’exemple suivant est basé sur [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un décodeur basé sur JavaScript expérimental.</span><span class="sxs-lookup"><span data-stu-id="2a714-122">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="2a714-123">L’exemple montre comment appeler une fonction JavaScript à partir d' C# une méthode.</span><span class="sxs-lookup"><span data-stu-id="2a714-123">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="2a714-124">La fonction JavaScript accepte un tableau d’octets d' C# une méthode, décode le tableau et retourne le texte au composant pour l’affichage.</span><span class="sxs-lookup"><span data-stu-id="2a714-124">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="2a714-125">À l’intérieur de l’élément `<head>` de *wwwroot/index.html* (Blazor webassembly) ou de *pages/_Host. cshtml* (serveurBlazor), fournissez une fonction JavaScript qui utilise `TextDecoder` pour décoder un tableau passé et retourner la valeur décodée :</span><span class="sxs-lookup"><span data-stu-id="2a714-125">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a JavaScript function that uses `TextDecoder` to decode a passed array and return the decoded value:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script-convertarray.html)]

<span data-ttu-id="2a714-126">Du code JavaScript, tel que le code illustré dans l’exemple précédent, peut également être chargé à partir d’un fichier JavaScript ( *. js*) avec une référence au fichier de script :</span><span class="sxs-lookup"><span data-stu-id="2a714-126">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="2a714-127">Le composant suivant :</span><span class="sxs-lookup"><span data-stu-id="2a714-127">The following component:</span></span>

* <span data-ttu-id="2a714-128">Appelle la fonction JavaScript `convertArray` à l’aide de `JSRuntime` lorsqu’un bouton de composant (**convertir un tableau**) est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="2a714-128">Invokes the `convertArray` JavaScript function using `JSRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="2a714-129">Une fois la fonction JavaScript appelée, le tableau passé est converti en chaîne.</span><span class="sxs-lookup"><span data-stu-id="2a714-129">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="2a714-130">La chaîne est retournée au composant pour l’affichage.</span><span class="sxs-lookup"><span data-stu-id="2a714-130">The string is returned to the component for display.</span></span>

[!code-razor[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="use-of-ijsruntime"></a><span data-ttu-id="2a714-131">Utilisation de IJSRuntime</span><span class="sxs-lookup"><span data-stu-id="2a714-131">Use of IJSRuntime</span></span>

<span data-ttu-id="2a714-132">Pour utiliser l’abstraction `IJSRuntime`, adoptez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="2a714-132">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="2a714-133">Injecter l’abstraction `IJSRuntime` dans le composant Razor ( *. Razor*) :</span><span class="sxs-lookup"><span data-stu-id="2a714-133">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-razor[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  <span data-ttu-id="2a714-134">À l’intérieur de l’élément `<head>` de *wwwroot/index.html* (Blazor webassembly) ou de *pages/_Host. cshtml* (serveurBlazor), fournissez une fonction JavaScript `handleTickerChanged`.</span><span class="sxs-lookup"><span data-stu-id="2a714-134">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="2a714-135">La fonction est appelée avec `IJSRuntime.InvokeVoidAsync` et ne retourne pas de valeur :</span><span class="sxs-lookup"><span data-stu-id="2a714-135">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* <span data-ttu-id="2a714-136">Injecter l’abstraction `IJSRuntime` dans une classe ( *. cs*) :</span><span class="sxs-lookup"><span data-stu-id="2a714-136">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  <span data-ttu-id="2a714-137">À l’intérieur de l’élément `<head>` de *wwwroot/index.html* (Blazor webassembly) ou de *pages/_Host. cshtml* (serveurBlazor), fournissez une fonction JavaScript `handleTickerChanged`.</span><span class="sxs-lookup"><span data-stu-id="2a714-137">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="2a714-138">La fonction est appelée avec `JSRuntime.InvokeAsync` et retourne une valeur :</span><span class="sxs-lookup"><span data-stu-id="2a714-138">The function is called with `JSRuntime.InvokeAsync` and returns a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* <span data-ttu-id="2a714-139">Pour la génération de contenu dynamique avec [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), utilisez l’attribut `[Inject]` :</span><span class="sxs-lookup"><span data-stu-id="2a714-139">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```razor
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="2a714-140">Dans l’exemple d’application côté client qui accompagne cette rubrique, deux fonctions JavaScript sont disponibles pour l’application qui interagit avec le DOM pour recevoir l’entrée d’utilisateur et afficher un message d’accueil :</span><span class="sxs-lookup"><span data-stu-id="2a714-140">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="2a714-141">`showPrompt` &ndash; génère une invite pour accepter l’entrée d’utilisateur (nom de l’utilisateur) et retourne le nom à l’appelant.</span><span class="sxs-lookup"><span data-stu-id="2a714-141">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="2a714-142">`displayWelcome` &ndash; assigne un message de bienvenue de l’appelant à un objet DOM avec une `id` de `welcome`.</span><span class="sxs-lookup"><span data-stu-id="2a714-142">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="2a714-143">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="2a714-143">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="2a714-144">Placez la balise `<script>` qui fait référence au fichier JavaScript dans le fichier *wwwroot/index.html* (Blazor assembly) ou le fichier *pages/_Host. cshtml* (serveurBlazor).</span><span class="sxs-lookup"><span data-stu-id="2a714-144">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="2a714-145">*wwwroot/index.html* (Blazor webassembly) :</span><span class="sxs-lookup"><span data-stu-id="2a714-145">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=22)]

<span data-ttu-id="2a714-146">*Pages/_Host. cshtml* (serveurBlazor) :</span><span class="sxs-lookup"><span data-stu-id="2a714-146">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=35)]

<span data-ttu-id="2a714-147">Ne placez pas de balise `<script>` dans un fichier de composant, car la balise `<script>` ne peut pas être mise à jour dynamiquement.</span><span class="sxs-lookup"><span data-stu-id="2a714-147">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="2a714-148">Les méthodes .NET interagissent avec les fonctions JavaScript dans le fichier *exampleJsInterop. js* en appelant `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="2a714-148">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="2a714-149">L’abstraction `IJSRuntime` est asynchrone pour permettre les scénarios de serveur Blazor.</span><span class="sxs-lookup"><span data-stu-id="2a714-149">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="2a714-150">Si l’application est une application Blazor webassembly et que vous souhaitez appeler une fonction JavaScript de manière synchrone, vous devez effectuer un cast aval pour `IJSInProcessRuntime` et appeler `Invoke<T>` à la place.</span><span class="sxs-lookup"><span data-stu-id="2a714-150">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="2a714-151">Nous recommandons que la plupart des bibliothèques d’interopérabilité JS utilisent les API Async pour s’assurer que les bibliothèques sont disponibles dans tous les scénarios.</span><span class="sxs-lookup"><span data-stu-id="2a714-151">We recommend that most JS interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="2a714-152">L’exemple d’application comprend un composant pour illustrer l’interopérabilité de JS.</span><span class="sxs-lookup"><span data-stu-id="2a714-152">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="2a714-153">Le composant :</span><span class="sxs-lookup"><span data-stu-id="2a714-153">The component:</span></span>

* <span data-ttu-id="2a714-154">Reçoit une entrée d’utilisateur via une invite JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2a714-154">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="2a714-155">Retourne le texte du composant pour traitement.</span><span class="sxs-lookup"><span data-stu-id="2a714-155">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="2a714-156">Appelle une deuxième fonction JavaScript qui interagit avec le DOM pour afficher un message d’accueil.</span><span class="sxs-lookup"><span data-stu-id="2a714-156">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="2a714-157">*Pages/JSInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="2a714-157">*Pages/JSInterop.razor*:</span></span>

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

1. <span data-ttu-id="2a714-158">Lorsque `TriggerJsPrompt` est exécuté en sélectionnant le bouton d' **invite de commandes JavaScript** du composant, la fonction JavaScript `showPrompt` fournie dans le fichier *wwwroot/exampleJsInterop. js* est appelée.</span><span class="sxs-lookup"><span data-stu-id="2a714-158">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="2a714-159">La fonction `showPrompt` accepte l’entrée d’utilisateur (nom de l’utilisateur), qui est codée au format HTML et renvoyée au composant.</span><span class="sxs-lookup"><span data-stu-id="2a714-159">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="2a714-160">Le composant stocke le nom de l’utilisateur dans une variable locale, `name`.</span><span class="sxs-lookup"><span data-stu-id="2a714-160">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="2a714-161">La chaîne stockée dans `name` est incorporée dans un message de bienvenue, qui est transmis à une fonction JavaScript, `displayWelcome`, qui affiche le message de bienvenue dans une balise d’en-tête.</span><span class="sxs-lookup"><span data-stu-id="2a714-161">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="2a714-162">Appeler une fonction JavaScript void</span><span class="sxs-lookup"><span data-stu-id="2a714-162">Call a void JavaScript function</span></span>

<span data-ttu-id="2a714-163">Les fonctions JavaScript qui retournent [void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) ou [non définies](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) sont appelées avec `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="2a714-163">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-opno-locblazor-app-is-prerendering"></a><span data-ttu-id="2a714-164">Détecter quand un Blazor application est prérendu</span><span class="sxs-lookup"><span data-stu-id="2a714-164">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="2a714-165">Capturer des références à des éléments</span><span class="sxs-lookup"><span data-stu-id="2a714-165">Capture references to elements</span></span>

<span data-ttu-id="2a714-166">Certains scénarios d’interopérabilité JS requièrent des références à des éléments HTML.</span><span class="sxs-lookup"><span data-stu-id="2a714-166">Some JS interop scenarios require references to HTML elements.</span></span> <span data-ttu-id="2a714-167">Par exemple, une bibliothèque d’interface utilisateur peut nécessiter une référence d’élément pour l’initialisation, ou vous devrez peut-être appeler des API de type commande sur un élément, par exemple `focus` ou `play`.</span><span class="sxs-lookup"><span data-stu-id="2a714-167">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="2a714-168">Capturer des références aux éléments HTML dans un composant à l’aide de l’approche suivante :</span><span class="sxs-lookup"><span data-stu-id="2a714-168">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="2a714-169">Ajoutez un attribut `@ref` à l’élément HTML.</span><span class="sxs-lookup"><span data-stu-id="2a714-169">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="2a714-170">Définissez un champ de type `ElementReference` dont le nom correspond à la valeur de l’attribut `@ref`.</span><span class="sxs-lookup"><span data-stu-id="2a714-170">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="2a714-171">L’exemple suivant illustre la capture d’une référence à l’élément `username` `<input>` :</span><span class="sxs-lookup"><span data-stu-id="2a714-171">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```razor
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> <span data-ttu-id="2a714-172">Utilisez uniquement une référence d’élément pour muter le contenu d’un élément vide qui n’interagit pas avec Blazor.</span><span class="sxs-lookup"><span data-stu-id="2a714-172">Only use an element reference to mutate the contents of an empty element that doesn't interact with Blazor.</span></span> <span data-ttu-id="2a714-173">Ce scénario est utile quand une API tierce fournit du contenu à l’élément.</span><span class="sxs-lookup"><span data-stu-id="2a714-173">This scenario is useful when a 3rd party API supplies content to the element.</span></span> <span data-ttu-id="2a714-174">Étant donné que Blazor n’interagit pas avec l’élément, il n’existe aucun risque de conflit entre la représentation de Blazorde l’élément et le DOM.</span><span class="sxs-lookup"><span data-stu-id="2a714-174">Because Blazor doesn't interact with the element, there's no possibility of a conflict between Blazor's representation of the element and the DOM.</span></span>
>
> <span data-ttu-id="2a714-175">Dans l’exemple suivant, il est *dangereux* de faire muter le contenu de la liste non triée (`ul`), car Blazor interagit avec le DOM pour remplir les éléments de liste de cet élément (`<li>`) :</span><span class="sxs-lookup"><span data-stu-id="2a714-175">In the following example, it's *dangerous* to mutate the contents of the unordered list (`ul`) because Blazor interacts with the DOM to populate this element's list items (`<li>`):</span></span>
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
> <span data-ttu-id="2a714-176">Si l’interopérabilité JS muter le contenu de l’élément `MyList` et Blazor tente d’appliquer des différences à l’élément, les différences ne correspondent pas au DOM.</span><span class="sxs-lookup"><span data-stu-id="2a714-176">If JS interop mutates the contents of element `MyList` and Blazor attempts to apply diffs to the element, the diffs won't match the DOM.</span></span>

<span data-ttu-id="2a714-177">En ce qui concerne le code .NET, un `ElementReference` est un handle opaque.</span><span class="sxs-lookup"><span data-stu-id="2a714-177">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="2a714-178">La *seule* chose que vous pouvez faire avec `ElementReference` est de la passer au code JavaScript via l’interopérabilité js.</span><span class="sxs-lookup"><span data-stu-id="2a714-178">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JS interop.</span></span> <span data-ttu-id="2a714-179">Dans ce cas, le code JavaScript reçoit une instance `HTMLElement`, qu’il peut utiliser avec les API DOM normales.</span><span class="sxs-lookup"><span data-stu-id="2a714-179">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="2a714-180">Par exemple, le code suivant définit une méthode d’extension .NET qui permet de définir le focus sur un élément :</span><span class="sxs-lookup"><span data-stu-id="2a714-180">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="2a714-181">*exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="2a714-181">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="2a714-182">Pour appeler une fonction JavaScript qui ne retourne pas de valeur, utilisez `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="2a714-182">To call a JavaScript function that doesn't return a value, use `IJSRuntime.InvokeVoidAsync`.</span></span> <span data-ttu-id="2a714-183">Le code suivant définit le focus sur l’entrée de nom d’utilisateur en appelant la fonction JavaScript précédente avec l' `ElementReference`capturée :</span><span class="sxs-lookup"><span data-stu-id="2a714-183">The following code sets the focus on the username input by calling the preceding JavaScript function with the captured `ElementReference`:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="2a714-184">Pour utiliser une méthode d’extension, créez une méthode d’extension statique qui reçoit l’instance `IJSRuntime` :</span><span class="sxs-lookup"><span data-stu-id="2a714-184">To use an extension method, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="2a714-185">La méthode `Focus` est appelée directement sur l’objet.</span><span class="sxs-lookup"><span data-stu-id="2a714-185">The `Focus` method is called directly on the object.</span></span> <span data-ttu-id="2a714-186">L’exemple suivant suppose que la méthode `Focus` est disponible à partir de l’espace de noms `JsInteropClasses` :</span><span class="sxs-lookup"><span data-stu-id="2a714-186">The following example assumes that the `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> <span data-ttu-id="2a714-187">La variable `username` n’est remplie qu’après le rendu du composant.</span><span class="sxs-lookup"><span data-stu-id="2a714-187">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="2a714-188">Si un `ElementReference` non rempli est passé au code JavaScript, le code JavaScript reçoit la valeur `null`.</span><span class="sxs-lookup"><span data-stu-id="2a714-188">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="2a714-189">Pour manipuler des références d’élément après que le composant a terminé le rendu (pour définir le focus initial sur un élément), utilisez les [méthodes de cycle de vie des composants OnAfterRenderAsync ou OnAfterRender](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="2a714-189">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the [OnAfterRenderAsync or OnAfterRender component lifecycle methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="2a714-190">Quand vous utilisez des types génériques et que vous retournez une valeur, utilisez [ValueTask\<t >](xref:System.Threading.Tasks.ValueTask`1):</span><span class="sxs-lookup"><span data-stu-id="2a714-190">When working with generic types and returning a value, use [ValueTask\<T>](xref:System.Threading.Tasks.ValueTask`1):</span></span>

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

<span data-ttu-id="2a714-191">`GenericMethod` est appelé directement sur l’objet avec un type.</span><span class="sxs-lookup"><span data-stu-id="2a714-191">`GenericMethod` is called directly on the object with a type.</span></span> <span data-ttu-id="2a714-192">L’exemple suivant suppose que la `GenericMethod` est disponible à partir de l’espace de noms `JsInteropClasses` :</span><span class="sxs-lookup"><span data-stu-id="2a714-192">The following example assumes that the `GenericMethod` is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component3.razor?highlight=17)]

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="2a714-193">Appeler des méthodes .NET à partir de fonctions JavaScript</span><span class="sxs-lookup"><span data-stu-id="2a714-193">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="2a714-194">Appel de méthode .NET statique</span><span class="sxs-lookup"><span data-stu-id="2a714-194">Static .NET method call</span></span>

<span data-ttu-id="2a714-195">Pour appeler une méthode .NET statique à partir de JavaScript, utilisez les fonctions `DotNet.invokeMethod` ou `DotNet.invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="2a714-195">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="2a714-196">Transmettez l’identificateur de la méthode statique que vous souhaitez appeler, le nom de l’assembly contenant la fonction et les arguments éventuels.</span><span class="sxs-lookup"><span data-stu-id="2a714-196">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="2a714-197">La version asynchrone est recommandée pour prendre en charge les scénarios de serveur Blazor.</span><span class="sxs-lookup"><span data-stu-id="2a714-197">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="2a714-198">Pour appeler une méthode .NET à partir de JavaScript, la méthode .NET doit être publique, statique et avoir l’attribut `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="2a714-198">To invoke a .NET method from JavaScript, the .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="2a714-199">Par défaut, l’identificateur de méthode est le nom de la méthode, mais vous pouvez spécifier un identificateur différent à l’aide du constructeur `JSInvokableAttribute`.</span><span class="sxs-lookup"><span data-stu-id="2a714-199">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="2a714-200">L’appel de méthodes génériques ouvertes n’est pas pris en charge actuellement.</span><span class="sxs-lookup"><span data-stu-id="2a714-200">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="2a714-201">L’exemple d’application comprend C# une méthode pour retourner un tableau de `int`s.</span><span class="sxs-lookup"><span data-stu-id="2a714-201">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="2a714-202">L’attribut `JSInvokable` est appliqué à la méthode.</span><span class="sxs-lookup"><span data-stu-id="2a714-202">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="2a714-203">*Pages/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="2a714-203">*Pages/JsInterop.razor*:</span></span>

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

<span data-ttu-id="2a714-204">JavaScript traité au client appelle la C# méthode .net.</span><span class="sxs-lookup"><span data-stu-id="2a714-204">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="2a714-205">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="2a714-205">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="2a714-206">Quand le bouton déclencher l’ReturnArrayAsync de la **méthode statique .net** est sélectionné, examinez la sortie de la console dans les outils de développement Web du navigateur.</span><span class="sxs-lookup"><span data-stu-id="2a714-206">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="2a714-207">La sortie de la console est la suivante :</span><span class="sxs-lookup"><span data-stu-id="2a714-207">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="2a714-208">La quatrième valeur de tableau fait l’objet d’un push dans le tableau (`data.push(4);`) retourné par `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="2a714-208">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="2a714-209">Appel de méthode d’instance</span><span class="sxs-lookup"><span data-stu-id="2a714-209">Instance method call</span></span>

<span data-ttu-id="2a714-210">Vous pouvez également appeler des méthodes d’instance .NET à partir de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2a714-210">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="2a714-211">Pour appeler une méthode d’instance .NET à partir de JavaScript :</span><span class="sxs-lookup"><span data-stu-id="2a714-211">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="2a714-212">Transmettez l’instance .NET à JavaScript en l’encapsulant dans une instance de `DotNetObjectReference`.</span><span class="sxs-lookup"><span data-stu-id="2a714-212">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectReference` instance.</span></span> <span data-ttu-id="2a714-213">L’instance .NET est passée par référence à JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2a714-213">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="2a714-214">Appeler des méthodes d’instance .NET sur l’instance à l’aide des fonctions `invokeMethod` ou `invokeMethodAsync`.</span><span class="sxs-lookup"><span data-stu-id="2a714-214">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="2a714-215">L’instance .NET peut également être passée comme argument lors de l’appel d’autres méthodes .NET à partir de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2a714-215">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="2a714-216">L’exemple d’application enregistre les messages dans la console côté client.</span><span class="sxs-lookup"><span data-stu-id="2a714-216">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="2a714-217">Pour les exemples suivants présentés dans l’exemple d’application, examinez la sortie de console du navigateur dans les outils de développement du navigateur.</span><span class="sxs-lookup"><span data-stu-id="2a714-217">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="2a714-218">Quand le bouton de **méthode d’instance .net de déclenchement HelloHelper. SayHello** est sélectionné, `ExampleJsInterop.CallHelloHelperSayHello` est appelé et passe un nom, `Blazor`, à la méthode.</span><span class="sxs-lookup"><span data-stu-id="2a714-218">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="2a714-219">*Pages/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="2a714-219">*Pages/JsInterop.razor*:</span></span>

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

<span data-ttu-id="2a714-220">`CallHelloHelperSayHello` appelle la fonction JavaScript `sayHello` avec une nouvelle instance de `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="2a714-220">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="2a714-221">*JsInteropClasses/ExampleJsInterop. cs*:</span><span class="sxs-lookup"><span data-stu-id="2a714-221">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="2a714-222">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="2a714-222">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="2a714-223">Le nom est passé au constructeur de `HelloHelper`, qui définit la propriété `HelloHelper.Name`.</span><span class="sxs-lookup"><span data-stu-id="2a714-223">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="2a714-224">Lorsque la fonction JavaScript `sayHello` est exécutée, `HelloHelper.SayHello` retourne le message `Hello, {Name}!`, qui est écrit dans la console par la fonction JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2a714-224">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="2a714-225">*JsInteropClasses/HelloHelper.cs*:</span><span class="sxs-lookup"><span data-stu-id="2a714-225">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="2a714-226">Sortie de la console dans les outils de développement Web du navigateur :</span><span class="sxs-lookup"><span data-stu-id="2a714-226">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="2a714-227">Partager du code d’interopérabilité dans une bibliothèque de classes</span><span class="sxs-lookup"><span data-stu-id="2a714-227">Share interop code in a class library</span></span>

<span data-ttu-id="2a714-228">Le code d’interopérabilité JS peut être inclus dans une bibliothèque de classes, ce qui vous permet de partager le code dans un package NuGet.</span><span class="sxs-lookup"><span data-stu-id="2a714-228">JS interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="2a714-229">La bibliothèque de classes gère l’incorporation des ressources JavaScript dans l’assembly généré.</span><span class="sxs-lookup"><span data-stu-id="2a714-229">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="2a714-230">Les fichiers JavaScript sont placés dans le dossier *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="2a714-230">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="2a714-231">Les outils s’occupent de l’incorporation des ressources lors de la génération de la bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="2a714-231">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="2a714-232">Le package NuGet créé est référencé dans le fichier projet de l’application de la même façon que n’importe quel package NuGet.</span><span class="sxs-lookup"><span data-stu-id="2a714-232">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="2a714-233">Une fois le package restauré, le code d’application peut appeler JavaScript comme s’il C#avait été.</span><span class="sxs-lookup"><span data-stu-id="2a714-233">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="2a714-234">Pour plus d'informations, consultez <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="2a714-234">For more information, see <xref:blazor/class-libraries>.</span></span>

## <a name="harden-js-interop-calls"></a><span data-ttu-id="2a714-235">Sécuriser les appels d’interopérabilité JS</span><span class="sxs-lookup"><span data-stu-id="2a714-235">Harden JS interop calls</span></span>

<span data-ttu-id="2a714-236">L’interopérabilité de JS peut échouer en raison d’erreurs réseau et doit être considérée comme non fiable.</span><span class="sxs-lookup"><span data-stu-id="2a714-236">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="2a714-237">Par défaut, une application Blazor Server expire les appels d’interopérabilité JS sur le serveur après une minute.</span><span class="sxs-lookup"><span data-stu-id="2a714-237">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="2a714-238">Si une application peut tolérer un délai d’expiration plus agressif, par exemple 10 secondes, définissez le délai d’expiration à l’aide de l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="2a714-238">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="2a714-239">Globalement dans `Startup.ConfigureServices`, spécifiez le délai d’expiration :</span><span class="sxs-lookup"><span data-stu-id="2a714-239">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="2a714-240">Par appel dans le code du composant, un appel unique peut spécifier le délai d’attente :</span><span class="sxs-lookup"><span data-stu-id="2a714-240">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="2a714-241">Pour plus d’informations sur l’épuisement des ressources, consultez <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="2a714-241">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2a714-242">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="2a714-242">Additional resources</span></span>

* [<span data-ttu-id="2a714-243">Exemple InteropComponent. Razor (référentiel GitHub dotnet/AspNetCore, branche de version 3,1)</span><span class="sxs-lookup"><span data-stu-id="2a714-243">InteropComponent.razor example (dotnet/AspNetCore GitHub repository, 3.1 release branch)</span></span>](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
