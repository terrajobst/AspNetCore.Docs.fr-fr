---
title: Appeler des fonctions JavaScript à partir de méthodes .NET dans ASP.NET Core Blazor
author: guardrex
description: Découvrez comment appeler des fonctions JavaScript à partir de méthodes .NET dans des applications Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/19/2020
no-loc:
- Blazor
- SignalR
uid: blazor/call-javascript-from-dotnet
ms.openlocfilehash: 7a27b6f1be2ef296d5b2b2a4f566e0cdedbe6480
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659711"
---
# <a name="call-javascript-functions-from-net-methods-in-aspnet-core-opno-locblazor"></a><span data-ttu-id="273b1-103">Appeler des fonctions JavaScript à partir de méthodes .NET dans ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="273b1-103">Call JavaScript functions from .NET methods in ASP.NET Core Blazor</span></span>

<span data-ttu-id="273b1-104">Par [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="273b1-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="273b1-105">Une application Blazor peut appeler des fonctions JavaScript à partir de méthodes .NET et de méthodes .NET à partir de fonctions JavaScript.</span><span class="sxs-lookup"><span data-stu-id="273b1-105">A Blazor app can invoke JavaScript functions from .NET methods and .NET methods from JavaScript functions.</span></span> <span data-ttu-id="273b1-106">Ces scénarios portent le nom de *l’interopérabilité de JavaScript* (*js Interop*).</span><span class="sxs-lookup"><span data-stu-id="273b1-106">These scenarios are called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="273b1-107">Cet article traite de l’appel de fonctions JavaScript à partir de .NET.</span><span class="sxs-lookup"><span data-stu-id="273b1-107">This article covers invoking JavaScript functions from .NET.</span></span> <span data-ttu-id="273b1-108">Pour plus d’informations sur la façon d’appeler des méthodes .NET à partir de JavaScript, consultez <xref:blazor/call-dotnet-from-javascript>.</span><span class="sxs-lookup"><span data-stu-id="273b1-108">For information on how to call .NET methods from JavaScript, see <xref:blazor/call-dotnet-from-javascript>.</span></span>

<span data-ttu-id="273b1-109">[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="273b1-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="273b1-110">Pour appeler JavaScript à partir de .NET, utilisez l’abstraction `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="273b1-110">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="273b1-111">Pour émettre des appels d’interopérabilité JS, injectez le `IJSRuntime` abstraction dans votre composant.</span><span class="sxs-lookup"><span data-stu-id="273b1-111">To issue JS interop calls, inject the `IJSRuntime` abstraction in your component.</span></span> <span data-ttu-id="273b1-112">La méthode `InvokeAsync<T>` accepte un identificateur pour la fonction JavaScript que vous souhaitez appeler, ainsi qu’un nombre quelconque d’arguments sérialisables JSON.</span><span class="sxs-lookup"><span data-stu-id="273b1-112">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="273b1-113">L’identificateur de fonction est relatif à la portée globale (`window`).</span><span class="sxs-lookup"><span data-stu-id="273b1-113">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="273b1-114">Si vous souhaitez appeler `window.someScope.someFunction`, l’identificateur est `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="273b1-114">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="273b1-115">Il n’est pas nécessaire d’inscrire la fonction avant qu’elle ne soit appelée.</span><span class="sxs-lookup"><span data-stu-id="273b1-115">There's no need to register the function before it's called.</span></span> <span data-ttu-id="273b1-116">Le type de retour `T` doit également être sérialisable JSON.</span><span class="sxs-lookup"><span data-stu-id="273b1-116">The return type `T` must also be JSON serializable.</span></span> <span data-ttu-id="273b1-117">`T` doit correspondre au type .NET qui correspond le mieux au type JSON retourné.</span><span class="sxs-lookup"><span data-stu-id="273b1-117">`T` should match the .NET type that best maps to the JSON type returned.</span></span>

<span data-ttu-id="273b1-118">Pour les applications Blazor Server avec le prérendu activé, l’appel à JavaScript n’est pas possible pendant le prérendu initial.</span><span class="sxs-lookup"><span data-stu-id="273b1-118">For Blazor Server apps with prerendering enabled, calling into JavaScript isn't possible during the initial prerendering.</span></span> <span data-ttu-id="273b1-119">Les appels Interop JavaScript doivent être différés jusqu’à ce que la connexion avec le navigateur soit établie.</span><span class="sxs-lookup"><span data-stu-id="273b1-119">JavaScript interop calls must be deferred until after the connection with the browser is established.</span></span> <span data-ttu-id="273b1-120">Pour plus d’informations, consultez la section [détecter quand une application serveur Blazor est un prérendu](#detect-when-a-blazor-server-app-is-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="273b1-120">For more information, see the [Detect when a Blazor Server app is prerendering](#detect-when-a-blazor-server-app-is-prerendering) section.</span></span>

<span data-ttu-id="273b1-121">L’exemple suivant est basé sur [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un décodeur basé sur JavaScript.</span><span class="sxs-lookup"><span data-stu-id="273b1-121">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), a JavaScript-based decoder.</span></span> <span data-ttu-id="273b1-122">L’exemple montre comment appeler une fonction JavaScript à partir d' C# une méthode.</span><span class="sxs-lookup"><span data-stu-id="273b1-122">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="273b1-123">La fonction JavaScript accepte un tableau d’octets d' C# une méthode, décode le tableau et retourne le texte au composant pour l’affichage.</span><span class="sxs-lookup"><span data-stu-id="273b1-123">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="273b1-124">À l’intérieur de l’élément `<head>` de *wwwroot/index.html* (Blazor webassembly) ou de *pages/_Host. cshtml* (serveurBlazor), fournissez une fonction JavaScript qui utilise `TextDecoder` pour décoder un tableau passé et retourner la valeur décodée :</span><span class="sxs-lookup"><span data-stu-id="273b1-124">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a JavaScript function that uses `TextDecoder` to decode a passed array and return the decoded value:</span></span>

[!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-convertarray.html)]

<span data-ttu-id="273b1-125">Du code JavaScript, tel que le code illustré dans l’exemple précédent, peut également être chargé à partir d’un fichier JavaScript ( *. js*) avec une référence au fichier de script :</span><span class="sxs-lookup"><span data-stu-id="273b1-125">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="273b1-126">Le composant suivant :</span><span class="sxs-lookup"><span data-stu-id="273b1-126">The following component:</span></span>

* <span data-ttu-id="273b1-127">Appelle la fonction JavaScript `convertArray` à l’aide de `JSRuntime` lorsqu’un bouton de composant (**convertir un tableau**) est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="273b1-127">Invokes the `convertArray` JavaScript function using `JSRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="273b1-128">Une fois la fonction JavaScript appelée, le tableau passé est converti en chaîne.</span><span class="sxs-lookup"><span data-stu-id="273b1-128">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="273b1-129">La chaîne est retournée au composant pour l’affichage.</span><span class="sxs-lookup"><span data-stu-id="273b1-129">The string is returned to the component for display.</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="ijsruntime"></a><span data-ttu-id="273b1-130">IJSRuntime</span><span class="sxs-lookup"><span data-stu-id="273b1-130">IJSRuntime</span></span>

<span data-ttu-id="273b1-131">Pour utiliser l’abstraction `IJSRuntime`, adoptez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="273b1-131">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="273b1-132">Injecter l’abstraction `IJSRuntime` dans le composant Razor ( *. Razor*) :</span><span class="sxs-lookup"><span data-stu-id="273b1-132">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-razor[](call-javascript-from-dotnet/samples_snapshot/inject-abstraction.razor?highlight=1)]

  <span data-ttu-id="273b1-133">À l’intérieur de l’élément `<head>` de *wwwroot/index.html* (Blazor webassembly) ou de *pages/_Host. cshtml* (serveurBlazor), fournissez une fonction JavaScript `handleTickerChanged`.</span><span class="sxs-lookup"><span data-stu-id="273b1-133">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="273b1-134">La fonction est appelée avec `IJSRuntime.InvokeVoidAsync` et ne retourne pas de valeur :</span><span class="sxs-lookup"><span data-stu-id="273b1-134">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

  [!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-handleTickerChanged1.html)]

* <span data-ttu-id="273b1-135">Injecter l’abstraction `IJSRuntime` dans une classe ( *. cs*) :</span><span class="sxs-lookup"><span data-stu-id="273b1-135">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](call-javascript-from-dotnet/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  <span data-ttu-id="273b1-136">À l’intérieur de l’élément `<head>` de *wwwroot/index.html* (Blazor webassembly) ou de *pages/_Host. cshtml* (serveurBlazor), fournissez une fonction JavaScript `handleTickerChanged`.</span><span class="sxs-lookup"><span data-stu-id="273b1-136">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="273b1-137">La fonction est appelée avec `JSRuntime.InvokeAsync` et retourne une valeur :</span><span class="sxs-lookup"><span data-stu-id="273b1-137">The function is called with `JSRuntime.InvokeAsync` and returns a value:</span></span>

  [!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-handleTickerChanged2.html)]

* <span data-ttu-id="273b1-138">Pour la génération de contenu dynamique avec [BuildRenderTree](xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic), utilisez l’attribut `[Inject]` :</span><span class="sxs-lookup"><span data-stu-id="273b1-138">For dynamic content generation with [BuildRenderTree](xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```razor
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="273b1-139">Dans l’exemple d’application côté client qui accompagne cette rubrique, deux fonctions JavaScript sont disponibles pour l’application qui interagit avec le DOM pour recevoir l’entrée d’utilisateur et afficher un message d’accueil :</span><span class="sxs-lookup"><span data-stu-id="273b1-139">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="273b1-140">`showPrompt` &ndash; génère une invite pour accepter l’entrée d’utilisateur (nom de l’utilisateur) et retourne le nom à l’appelant.</span><span class="sxs-lookup"><span data-stu-id="273b1-140">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="273b1-141">`displayWelcome` &ndash; assigne un message de bienvenue de l’appelant à un objet DOM avec une `id` de `welcome`.</span><span class="sxs-lookup"><span data-stu-id="273b1-141">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="273b1-142">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="273b1-142">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="273b1-143">Placez la balise `<script>` qui fait référence au fichier JavaScript dans le fichier *wwwroot/index.html* (Blazor assembly) ou le fichier *pages/_Host. cshtml* (serveurBlazor).</span><span class="sxs-lookup"><span data-stu-id="273b1-143">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="273b1-144">*wwwroot/index.html* (Blazor webassembly) :</span><span class="sxs-lookup"><span data-stu-id="273b1-144">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=22)]

<span data-ttu-id="273b1-145">*Pages/_Host. cshtml* (serveurBlazor) :</span><span class="sxs-lookup"><span data-stu-id="273b1-145">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=35)]

<span data-ttu-id="273b1-146">Ne placez pas de balise `<script>` dans un fichier de composant, car la balise `<script>` ne peut pas être mise à jour dynamiquement.</span><span class="sxs-lookup"><span data-stu-id="273b1-146">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="273b1-147">Les méthodes .NET interagissent avec les fonctions JavaScript dans le fichier *exampleJsInterop. js* en appelant `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="273b1-147">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="273b1-148">L’abstraction `IJSRuntime` est asynchrone pour permettre les scénarios de serveur Blazor.</span><span class="sxs-lookup"><span data-stu-id="273b1-148">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="273b1-149">Si l’application est une application Blazor webassembly et que vous souhaitez appeler une fonction JavaScript de manière synchrone, vous devez effectuer un cast aval pour `IJSInProcessRuntime` et appeler `Invoke<T>` à la place.</span><span class="sxs-lookup"><span data-stu-id="273b1-149">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="273b1-150">Nous recommandons que la plupart des bibliothèques d’interopérabilité JS utilisent les API Async pour s’assurer que les bibliothèques sont disponibles dans tous les scénarios.</span><span class="sxs-lookup"><span data-stu-id="273b1-150">We recommend that most JS interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="273b1-151">L’exemple d’application comprend un composant pour illustrer l’interopérabilité de JS.</span><span class="sxs-lookup"><span data-stu-id="273b1-151">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="273b1-152">Le composant :</span><span class="sxs-lookup"><span data-stu-id="273b1-152">The component:</span></span>

* <span data-ttu-id="273b1-153">Reçoit une entrée d’utilisateur via une invite JavaScript.</span><span class="sxs-lookup"><span data-stu-id="273b1-153">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="273b1-154">Retourne le texte du composant pour traitement.</span><span class="sxs-lookup"><span data-stu-id="273b1-154">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="273b1-155">Appelle une deuxième fonction JavaScript qui interagit avec le DOM pour afficher un message d’accueil.</span><span class="sxs-lookup"><span data-stu-id="273b1-155">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="273b1-156">*Pages/JSInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="273b1-156">*Pages/JSInterop.razor*:</span></span>

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
        var name = await JSRuntime.InvokeAsync<string>(
                "exampleJsFunctions.showPrompt",
                "What's your name?");

        await JSRuntime.InvokeVoidAsync(
                "exampleJsFunctions.displayWelcome",
                $"Hello {name}! Welcome to Blazor!");
    }
}
```

1. <span data-ttu-id="273b1-157">Lorsque `TriggerJsPrompt` est exécuté en sélectionnant le bouton d' **invite de commandes JavaScript** du composant, la fonction JavaScript `showPrompt` fournie dans le fichier *wwwroot/exampleJsInterop. js* est appelée.</span><span class="sxs-lookup"><span data-stu-id="273b1-157">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="273b1-158">La fonction `showPrompt` accepte l’entrée d’utilisateur (nom de l’utilisateur), qui est codée au format HTML et renvoyée au composant.</span><span class="sxs-lookup"><span data-stu-id="273b1-158">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="273b1-159">Le composant stocke le nom de l’utilisateur dans une variable locale, `name`.</span><span class="sxs-lookup"><span data-stu-id="273b1-159">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="273b1-160">La chaîne stockée dans `name` est incorporée dans un message de bienvenue, qui est transmis à une fonction JavaScript, `displayWelcome`, qui affiche le message de bienvenue dans une balise d’en-tête.</span><span class="sxs-lookup"><span data-stu-id="273b1-160">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="273b1-161">Appeler une fonction JavaScript void</span><span class="sxs-lookup"><span data-stu-id="273b1-161">Call a void JavaScript function</span></span>

<span data-ttu-id="273b1-162">Les fonctions JavaScript qui retournent [void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) ou [non définies](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) sont appelées avec `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="273b1-162">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-opno-locblazor-server-app-is-prerendering"></a><span data-ttu-id="273b1-163">Détecter quand une application Blazor Server est prérendu</span><span class="sxs-lookup"><span data-stu-id="273b1-163">Detect when a Blazor Server app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="273b1-164">Capturer des références à des éléments</span><span class="sxs-lookup"><span data-stu-id="273b1-164">Capture references to elements</span></span>

<span data-ttu-id="273b1-165">Certains scénarios d’interopérabilité JS requièrent des références à des éléments HTML.</span><span class="sxs-lookup"><span data-stu-id="273b1-165">Some JS interop scenarios require references to HTML elements.</span></span> <span data-ttu-id="273b1-166">Par exemple, une bibliothèque d’interface utilisateur peut nécessiter une référence d’élément pour l’initialisation, ou vous devrez peut-être appeler des API de type commande sur un élément, par exemple `focus` ou `play`.</span><span class="sxs-lookup"><span data-stu-id="273b1-166">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="273b1-167">Capturer des références aux éléments HTML dans un composant à l’aide de l’approche suivante :</span><span class="sxs-lookup"><span data-stu-id="273b1-167">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="273b1-168">Ajoutez un attribut `@ref` à l’élément HTML.</span><span class="sxs-lookup"><span data-stu-id="273b1-168">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="273b1-169">Définissez un champ de type `ElementReference` dont le nom correspond à la valeur de l’attribut `@ref`.</span><span class="sxs-lookup"><span data-stu-id="273b1-169">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="273b1-170">L’exemple suivant illustre la capture d’une référence à l’élément `username` `<input>` :</span><span class="sxs-lookup"><span data-stu-id="273b1-170">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```razor
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> <span data-ttu-id="273b1-171">Utilisez uniquement une référence d’élément pour muter le contenu d’un élément vide qui n’interagit pas avec Blazor.</span><span class="sxs-lookup"><span data-stu-id="273b1-171">Only use an element reference to mutate the contents of an empty element that doesn't interact with Blazor.</span></span> <span data-ttu-id="273b1-172">Ce scénario est utile quand une API tierce fournit du contenu à l’élément.</span><span class="sxs-lookup"><span data-stu-id="273b1-172">This scenario is useful when a 3rd party API supplies content to the element.</span></span> <span data-ttu-id="273b1-173">Étant donné que Blazor n’interagit pas avec l’élément, il n’existe aucun risque de conflit entre la représentation de Blazorde l’élément et le DOM.</span><span class="sxs-lookup"><span data-stu-id="273b1-173">Because Blazor doesn't interact with the element, there's no possibility of a conflict between Blazor's representation of the element and the DOM.</span></span>
>
> <span data-ttu-id="273b1-174">Dans l’exemple suivant, il est *dangereux* de faire muter le contenu de la liste non triée (`ul`), car Blazor interagit avec le DOM pour remplir les éléments de liste de cet élément (`<li>`) :</span><span class="sxs-lookup"><span data-stu-id="273b1-174">In the following example, it's *dangerous* to mutate the contents of the unordered list (`ul`) because Blazor interacts with the DOM to populate this element's list items (`<li>`):</span></span>
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
> <span data-ttu-id="273b1-175">Si l’interopérabilité JS muter le contenu de l’élément `MyList` et Blazor tente d’appliquer des différences à l’élément, les différences ne correspondent pas au DOM.</span><span class="sxs-lookup"><span data-stu-id="273b1-175">If JS interop mutates the contents of element `MyList` and Blazor attempts to apply diffs to the element, the diffs won't match the DOM.</span></span>

<span data-ttu-id="273b1-176">En ce qui concerne le code .NET, un `ElementReference` est un handle opaque.</span><span class="sxs-lookup"><span data-stu-id="273b1-176">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="273b1-177">La *seule* chose que vous pouvez faire avec `ElementReference` est de la passer au code JavaScript via l’interopérabilité js.</span><span class="sxs-lookup"><span data-stu-id="273b1-177">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JS interop.</span></span> <span data-ttu-id="273b1-178">Dans ce cas, le code JavaScript reçoit une instance `HTMLElement`, qu’il peut utiliser avec les API DOM normales.</span><span class="sxs-lookup"><span data-stu-id="273b1-178">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="273b1-179">Par exemple, le code suivant définit une méthode d’extension .NET qui permet de définir le focus sur un élément :</span><span class="sxs-lookup"><span data-stu-id="273b1-179">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="273b1-180">*exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="273b1-180">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="273b1-181">Pour appeler une fonction JavaScript qui ne retourne pas de valeur, utilisez `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="273b1-181">To call a JavaScript function that doesn't return a value, use `IJSRuntime.InvokeVoidAsync`.</span></span> <span data-ttu-id="273b1-182">Le code suivant définit le focus sur l’entrée de nom d’utilisateur en appelant la fonction JavaScript précédente avec l' `ElementReference`capturée :</span><span class="sxs-lookup"><span data-stu-id="273b1-182">The following code sets the focus on the username input by calling the preceding JavaScript function with the captured `ElementReference`:</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="273b1-183">Pour utiliser une méthode d’extension, créez une méthode d’extension statique qui reçoit l’instance `IJSRuntime` :</span><span class="sxs-lookup"><span data-stu-id="273b1-183">To use an extension method, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="273b1-184">La méthode `Focus` est appelée directement sur l’objet.</span><span class="sxs-lookup"><span data-stu-id="273b1-184">The `Focus` method is called directly on the object.</span></span> <span data-ttu-id="273b1-185">L’exemple suivant suppose que la méthode `Focus` est disponible à partir de l’espace de noms `JsInteropClasses` :</span><span class="sxs-lookup"><span data-stu-id="273b1-185">The following example assumes that the `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> <span data-ttu-id="273b1-186">La variable `username` n’est remplie qu’après le rendu du composant.</span><span class="sxs-lookup"><span data-stu-id="273b1-186">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="273b1-187">Si un `ElementReference` non rempli est passé au code JavaScript, le code JavaScript reçoit la valeur `null`.</span><span class="sxs-lookup"><span data-stu-id="273b1-187">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="273b1-188">Pour manipuler des références d’élément après que le composant a terminé le rendu (pour définir le focus initial sur un élément), utilisez les [méthodes de cycle de vie des composants OnAfterRenderAsync ou OnAfterRender](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="273b1-188">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the [OnAfterRenderAsync or OnAfterRender component lifecycle methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="273b1-189">Quand vous utilisez des types génériques et que vous retournez une valeur, utilisez [ValueTask\<t >](xref:System.Threading.Tasks.ValueTask`1):</span><span class="sxs-lookup"><span data-stu-id="273b1-189">When working with generic types and returning a value, use [ValueTask\<T>](xref:System.Threading.Tasks.ValueTask`1):</span></span>

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

<span data-ttu-id="273b1-190">`GenericMethod` est appelé directement sur l’objet avec un type.</span><span class="sxs-lookup"><span data-stu-id="273b1-190">`GenericMethod` is called directly on the object with a type.</span></span> <span data-ttu-id="273b1-191">L’exemple suivant suppose que la `GenericMethod` est disponible à partir de l’espace de noms `JsInteropClasses` :</span><span class="sxs-lookup"><span data-stu-id="273b1-191">The following example assumes that the `GenericMethod` is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component3.razor?highlight=17)]

## <a name="reference-elements-across-components"></a><span data-ttu-id="273b1-192">Éléments de référence sur les composants</span><span class="sxs-lookup"><span data-stu-id="273b1-192">Reference elements across components</span></span>

<span data-ttu-id="273b1-193">Un `ElementReference` est garanti uniquement valide dans la méthode `OnAfterRender` d’un composant (et une référence d’élément est un `struct`), de sorte qu’une référence d’élément ne peut pas être transmise entre les composants.</span><span class="sxs-lookup"><span data-stu-id="273b1-193">An `ElementReference` is only guaranteed valid in a component's `OnAfterRender` method (and an element reference is a `struct`), so an element reference can't be passed between components.</span></span>

<span data-ttu-id="273b1-194">Pour qu’un composant parent rende une référence d’élément disponible pour d’autres composants, le composant parent peut :</span><span class="sxs-lookup"><span data-stu-id="273b1-194">For a parent component to make an element reference available to other components, the parent component can:</span></span>

* <span data-ttu-id="273b1-195">Autorisez les composants enfants à inscrire des rappels.</span><span class="sxs-lookup"><span data-stu-id="273b1-195">Allow child components to register callbacks.</span></span>
* <span data-ttu-id="273b1-196">Appelez les rappels inscrits pendant l’événement `OnAfterRender` avec la référence d’élément passée.</span><span class="sxs-lookup"><span data-stu-id="273b1-196">Invoke the registered callbacks during the `OnAfterRender` event with the passed element reference.</span></span> <span data-ttu-id="273b1-197">Indirectement, cette approche permet aux composants enfants d’interagir avec la référence d’élément du parent.</span><span class="sxs-lookup"><span data-stu-id="273b1-197">Indirectly, this approach allows child components to interact with the parent's element reference.</span></span>

<span data-ttu-id="273b1-198">L’exemple de Blazor webassembly suivant illustre l’approche.</span><span class="sxs-lookup"><span data-stu-id="273b1-198">The following Blazor WebAssembly example illustrates the approach.</span></span>

<span data-ttu-id="273b1-199">Dans le `<head>` de *wwwroot/index.html*:</span><span class="sxs-lookup"><span data-stu-id="273b1-199">In the `<head>` of *wwwroot/index.html*:</span></span>

```html
<style>
    .red { color: red }
</style>
```

<span data-ttu-id="273b1-200">Dans le `<body>` de *wwwroot/index.html*:</span><span class="sxs-lookup"><span data-stu-id="273b1-200">In the `<body>` of *wwwroot/index.html*:</span></span>

```html
<script>
    function setElementClass(element, className) {
        /** @type {HTMLElement} **/
        var myElement = element;
        myElement.classList.add(className);
    }
</script>
```

<span data-ttu-id="273b1-201">*Pages/index. Razor* (composant parent) :</span><span class="sxs-lookup"><span data-stu-id="273b1-201">*Pages/Index.razor* (parent component):</span></span>

```razor
@page "/"

<h1 @ref="_title">Hello, world!</h1>

Welcome to your new app.

<SurveyPrompt Parent="this" Title="How is Blazor working for you?" />
```

<span data-ttu-id="273b1-202">*Pages/index. Razor. cs*:</span><span class="sxs-lookup"><span data-stu-id="273b1-202">*Pages/Index.razor.cs*:</span></span>

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

<span data-ttu-id="273b1-203">*Shared/SurveyPrompt. Razor* (composant enfant) :</span><span class="sxs-lookup"><span data-stu-id="273b1-203">*Shared/SurveyPrompt.razor* (child component):</span></span>

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

<span data-ttu-id="273b1-204">*Shared/SurveyPrompt. Razor. cs*:</span><span class="sxs-lookup"><span data-stu-id="273b1-204">*Shared/SurveyPrompt.razor.cs*:</span></span>

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

## <a name="harden-js-interop-calls"></a><span data-ttu-id="273b1-205">Sécuriser les appels d’interopérabilité JS</span><span class="sxs-lookup"><span data-stu-id="273b1-205">Harden JS interop calls</span></span>

<span data-ttu-id="273b1-206">L’interopérabilité de JS peut échouer en raison d’erreurs réseau et doit être considérée comme non fiable.</span><span class="sxs-lookup"><span data-stu-id="273b1-206">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="273b1-207">Par défaut, une application Blazor Server expire les appels d’interopérabilité JS sur le serveur après une minute.</span><span class="sxs-lookup"><span data-stu-id="273b1-207">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="273b1-208">Si une application peut tolérer un délai d’expiration plus agressif, par exemple 10 secondes, définissez le délai d’expiration à l’aide de l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="273b1-208">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="273b1-209">Globalement dans `Startup.ConfigureServices`, spécifiez le délai d’expiration :</span><span class="sxs-lookup"><span data-stu-id="273b1-209">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="273b1-210">Par appel dans le code du composant, un appel unique peut spécifier le délai d’attente :</span><span class="sxs-lookup"><span data-stu-id="273b1-210">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="273b1-211">Pour plus d’informations sur l’épuisement des ressources, consultez <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="273b1-211">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>

[!INCLUDE[Share interop code in a class library](~/includes/blazor-share-interop-code.md)]

## <a name="additional-resources"></a><span data-ttu-id="273b1-212">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="273b1-212">Additional resources</span></span>

* <xref:blazor/call-dotnet-from-javascript>
* [<span data-ttu-id="273b1-213">Exemple InteropComponent. Razor (référentiel GitHub dotnet/AspNetCore, branche de version 3,1)</span><span class="sxs-lookup"><span data-stu-id="273b1-213">InteropComponent.razor example (dotnet/AspNetCore GitHub repository, 3.1 release branch)</span></span>](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
* <span data-ttu-id="273b1-214">[Effectuer des transferts de données volumineux dans des applications Blazor Server](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)</span><span class="sxs-lookup"><span data-stu-id="273b1-214">[Perform large data transfers in Blazor Server apps](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)</span></span>