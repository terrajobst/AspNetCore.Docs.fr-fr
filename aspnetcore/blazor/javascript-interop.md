---
title: ASP.NET Core l’interopérabilité avec le JavaScript éblouissant
author: guardrex
description: Découvrez comment appeler des fonctions JavaScript à partir de méthodes .NET et .NET à partir de JavaScript dans les applications éblouissantes.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/javascript-interop
ms.openlocfilehash: b30bce6ef3ebf1cd2f4f3fe8d046e1db9b6929d5
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71924646"
---
# <a name="aspnet-core-blazor-javascript-interop"></a><span data-ttu-id="e0e63-103">ASP.NET Core l’interopérabilité avec le JavaScript éblouissant</span><span class="sxs-lookup"><span data-stu-id="e0e63-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="e0e63-104">Par [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e0e63-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="e0e63-105">Une application éblouissant peut appeler des fonctions JavaScript à partir de méthodes .NET et .NET à partir de code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e0e63-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="e0e63-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e0e63-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="e0e63-107">Appeler des fonctions JavaScript à partir de méthodes .NET</span><span class="sxs-lookup"><span data-stu-id="e0e63-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="e0e63-108">Il arrive parfois que le code .NET soit requis pour appeler une fonction JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e0e63-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="e0e63-109">Par exemple, un appel JavaScript peut exposer des fonctionnalités de navigateur ou des fonctionnalités d’une bibliothèque JavaScript à l’application.</span><span class="sxs-lookup"><span data-stu-id="e0e63-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="e0e63-110">Pour appeler JavaScript à partir de .net, utilisez `IJSRuntime` l’abstraction.</span><span class="sxs-lookup"><span data-stu-id="e0e63-110">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="e0e63-111">La `InvokeAsync<T>` méthode prend un identificateur pour la fonction JavaScript que vous souhaitez appeler, ainsi que n’importe quel nombre d’arguments sérialisables JSON.</span><span class="sxs-lookup"><span data-stu-id="e0e63-111">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="e0e63-112">L’identificateur de fonction est relatif à la portée globale (`window`).</span><span class="sxs-lookup"><span data-stu-id="e0e63-112">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="e0e63-113">Si vous souhaitez appeler `window.someScope.someFunction`, l’identificateur est. `someScope.someFunction`</span><span class="sxs-lookup"><span data-stu-id="e0e63-113">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="e0e63-114">Il n’est pas nécessaire d’inscrire la fonction avant qu’elle ne soit appelée.</span><span class="sxs-lookup"><span data-stu-id="e0e63-114">There's no need to register the function before it's called.</span></span> <span data-ttu-id="e0e63-115">Le type `T` de retour doit également être sérialisable JSON.</span><span class="sxs-lookup"><span data-stu-id="e0e63-115">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="e0e63-116">Pour les applications serveur éblouissantes :</span><span class="sxs-lookup"><span data-stu-id="e0e63-116">For Blazor Server apps:</span></span>

* <span data-ttu-id="e0e63-117">Plusieurs demandes utilisateur sont traitées par l’application serveur éblouissante.</span><span class="sxs-lookup"><span data-stu-id="e0e63-117">Multiple user requests are processed by the Blazor Server app.</span></span> <span data-ttu-id="e0e63-118">N’appelez `JSRuntime.Current` pas dans un composant pour appeler des fonctions JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e0e63-118">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="e0e63-119">Injectez `IJSRuntime` l’abstraction et utilisez l’objet injecté pour émettre des appels Interop JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e0e63-119">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>
* <span data-ttu-id="e0e63-120">Lorsqu’une application éblouissant est prérendue, l’appel à JavaScript n’est pas possible, car une connexion avec le navigateur n’a pas été établie.</span><span class="sxs-lookup"><span data-stu-id="e0e63-120">While a Blazor app is prerendering, calling into JavaScript isn't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="e0e63-121">Pour plus d’informations, consultez la section [détecter quand une application éblouissant est un prérendu](#detect-when-a-blazor-app-is-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="e0e63-121">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="e0e63-122">L’exemple suivant est basé sur [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un décodeur basé sur JavaScript expérimental.</span><span class="sxs-lookup"><span data-stu-id="e0e63-122">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="e0e63-123">L’exemple montre comment appeler une fonction JavaScript à partir d' C# une méthode.</span><span class="sxs-lookup"><span data-stu-id="e0e63-123">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="e0e63-124">La fonction JavaScript accepte un tableau d’octets d' C# une méthode, décode le tableau et retourne le texte au composant pour l’affichage.</span><span class="sxs-lookup"><span data-stu-id="e0e63-124">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="e0e63-125">À l’intérieur de l’élément `<head>` de *wwwroot/index.html* (éblouissant webassembly) ou *pages/_Host. cshtml* (serveur éblouissant), fournissez une fonction qui utilise `TextDecoder` pour décoder un tableau passé :</span><span class="sxs-lookup"><span data-stu-id="e0e63-125">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a function that uses `TextDecoder` to decode a passed array:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script.html)]

<span data-ttu-id="e0e63-126">Du code JavaScript, tel que le code illustré dans l’exemple précédent, peut également être chargé à partir d’un fichier JavaScript ( *. js*) avec une référence au fichier de script :</span><span class="sxs-lookup"><span data-stu-id="e0e63-126">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="e0e63-127">Le composant suivant :</span><span class="sxs-lookup"><span data-stu-id="e0e63-127">The following component:</span></span>

* <span data-ttu-id="e0e63-128">Appelle la `ConvertArray` fonction JavaScript à l' `JsRuntime` aide de lorsqu’un bouton de composant (**convertir un tableau**) est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="e0e63-128">Invokes the `ConvertArray` JavaScript function using `JsRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="e0e63-129">Une fois la fonction JavaScript appelée, le tableau passé est converti en chaîne.</span><span class="sxs-lookup"><span data-stu-id="e0e63-129">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="e0e63-130">La chaîne est retournée au composant pour l’affichage.</span><span class="sxs-lookup"><span data-stu-id="e0e63-130">The string is returned to the component for display.</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

<span data-ttu-id="e0e63-131">Pour utiliser l' `IJSRuntime` abstraction, adoptez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="e0e63-131">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="e0e63-132">Injecter `IJSRuntime` l’abstraction dans le composant Razor ( *. Razor*) :</span><span class="sxs-lookup"><span data-stu-id="e0e63-132">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

* <span data-ttu-id="e0e63-133">Injecter `IJSRuntime` l’abstraction dans une classe ( *. cs*) :</span><span class="sxs-lookup"><span data-stu-id="e0e63-133">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

* <span data-ttu-id="e0e63-134">Pour la génération de contenu dynamique avec [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), `[Inject]` utilisez l’attribut :</span><span class="sxs-lookup"><span data-stu-id="e0e63-134">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="e0e63-135">Dans l’exemple d’application côté client qui accompagne cette rubrique, deux fonctions JavaScript sont disponibles pour l’application qui interagit avec le DOM pour recevoir l’entrée d’utilisateur et afficher un message d’accueil :</span><span class="sxs-lookup"><span data-stu-id="e0e63-135">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="e0e63-136">`showPrompt`&ndash; Génère une invite pour accepter l’entrée d’utilisateur (nom de l’utilisateur) et retourne le nom à l’appelant.</span><span class="sxs-lookup"><span data-stu-id="e0e63-136">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="e0e63-137">`displayWelcome`Assigne un message de bienvenue de l’appelant à un objet DOM avec `id` un `welcome`de. &ndash;</span><span class="sxs-lookup"><span data-stu-id="e0e63-137">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="e0e63-138">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="e0e63-138">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="e0e63-139">Placez la balise `<script>` qui fait référence au fichier JavaScript dans le fichier *wwwroot/index.html* (l’assembly éblouissant) ou le fichier *pages/_Host. cshtml* (serveur éblouissant).</span><span class="sxs-lookup"><span data-stu-id="e0e63-139">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="e0e63-140">*wwwroot/index.html* (Webassembly éblouissant) :</span><span class="sxs-lookup"><span data-stu-id="e0e63-140">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="e0e63-141">*Pages/_Host. cshtml* (serveur éblouissant) :</span><span class="sxs-lookup"><span data-stu-id="e0e63-141">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/_Host.cshtml?highlight=29)]

<span data-ttu-id="e0e63-142">Ne placez `<script>` pas de balise dans un fichier de `<script>` composant parce que la balise ne peut pas être mise à jour dynamiquement.</span><span class="sxs-lookup"><span data-stu-id="e0e63-142">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="e0e63-143">Les méthodes .NET interagissent avec les fonctions JavaScript dans le fichier *exampleJsInterop. js* en appelant `IJSRuntime.InvokeAsync<T>`.</span><span class="sxs-lookup"><span data-stu-id="e0e63-143">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="e0e63-144">L' `IJSRuntime` abstraction est asynchrone pour permettre les scénarios de serveur éblouissants.</span><span class="sxs-lookup"><span data-stu-id="e0e63-144">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="e0e63-145">Si l’application est une application de webassembly éblouissante et que vous souhaitez appeler une fonction JavaScript de manière synchrone `IJSInProcessRuntime` , vous `Invoke<T>` devez effectuer un casting vers et appeler à la place.</span><span class="sxs-lookup"><span data-stu-id="e0e63-145">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="e0e63-146">Nous recommandons que la plupart des bibliothèques d’interopérabilité JavaScript utilisent les API Async pour s’assurer que les bibliothèques sont disponibles dans tous les scénarios.</span><span class="sxs-lookup"><span data-stu-id="e0e63-146">We recommend that most JavaScript interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="e0e63-147">L’exemple d’application comprend un composant pour illustrer l’interopérabilité JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e0e63-147">The sample app includes a component to demonstrate JavaScript interop.</span></span> <span data-ttu-id="e0e63-148">Le composant :</span><span class="sxs-lookup"><span data-stu-id="e0e63-148">The component:</span></span>

* <span data-ttu-id="e0e63-149">Reçoit une entrée d’utilisateur via une invite JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e0e63-149">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="e0e63-150">Retourne le texte du composant pour traitement.</span><span class="sxs-lookup"><span data-stu-id="e0e63-150">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="e0e63-151">Appelle une deuxième fonction JavaScript qui interagit avec le DOM pour afficher un message d’accueil.</span><span class="sxs-lookup"><span data-stu-id="e0e63-151">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="e0e63-152">*Pages/JSInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="e0e63-152">*Pages/JSInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. <span data-ttu-id="e0e63-153">Lorsque `TriggerJsPrompt` est exécuté en sélectionnant le bouton d' **invite de commandes JavaScript** du composant `showPrompt` , la fonction JavaScript fournie dans le fichier *wwwroot/exampleJsInterop. js* est appelée.</span><span class="sxs-lookup"><span data-stu-id="e0e63-153">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="e0e63-154">La `showPrompt` fonction accepte l’entrée d’utilisateur (nom de l’utilisateur), qui est codée au format HTML et renvoyée au composant.</span><span class="sxs-lookup"><span data-stu-id="e0e63-154">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="e0e63-155">Le composant stocke le nom de l’utilisateur dans une variable `name`locale,.</span><span class="sxs-lookup"><span data-stu-id="e0e63-155">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="e0e63-156">La chaîne stockée `name` dans est incorporée dans un message de bienvenue, qui est transmis à une `displayWelcome`fonction JavaScript,, qui affiche le message de bienvenue dans une balise de titre.</span><span class="sxs-lookup"><span data-stu-id="e0e63-156">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="e0e63-157">Appeler une fonction JavaScript void</span><span class="sxs-lookup"><span data-stu-id="e0e63-157">Call a void JavaScript function</span></span>

<span data-ttu-id="e0e63-158">Les fonctions JavaScript qui retournent [void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) ou [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) sont appelées avec `IJSRuntime.InvokeVoidAsync`.</span><span class="sxs-lookup"><span data-stu-id="e0e63-158">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-blazor-app-is-prerendering"></a><span data-ttu-id="e0e63-159">Détecter quand une application éblouissant est prérendu</span><span class="sxs-lookup"><span data-stu-id="e0e63-159">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="e0e63-160">Capturer des références à des éléments</span><span class="sxs-lookup"><span data-stu-id="e0e63-160">Capture references to elements</span></span>

<span data-ttu-id="e0e63-161">Certains scénarios d' [interopérabilité JavaScript](xref:blazor/javascript-interop) requièrent des références aux éléments HTML.</span><span class="sxs-lookup"><span data-stu-id="e0e63-161">Some [JavaScript interop](xref:blazor/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="e0e63-162">Par exemple, une bibliothèque d’interface utilisateur peut nécessiter une référence d’élément pour l’initialisation, ou vous devrez peut-être appeler des API de type commande `focus` sur `play`un élément, tel que ou.</span><span class="sxs-lookup"><span data-stu-id="e0e63-162">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="e0e63-163">Capturer des références aux éléments HTML dans un composant à l’aide de l’approche suivante :</span><span class="sxs-lookup"><span data-stu-id="e0e63-163">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="e0e63-164">Ajoutez un `@ref` attribut à l’élément HTML.</span><span class="sxs-lookup"><span data-stu-id="e0e63-164">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="e0e63-165">Définissez un champ de type `ElementReference` dont le nom correspond à la valeur `@ref` de l’attribut.</span><span class="sxs-lookup"><span data-stu-id="e0e63-165">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="e0e63-166">L’exemple suivant illustre la capture d’une référence `username` à l' `<input>` élément :</span><span class="sxs-lookup"><span data-stu-id="e0e63-166">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```cshtml
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!NOTE]
> <span data-ttu-id="e0e63-167">N’utilisez **pas** de références d’élément capturées comme méthode de remplissage du DOM.</span><span class="sxs-lookup"><span data-stu-id="e0e63-167">Do **not** use captured element references as a way of populating the DOM.</span></span> <span data-ttu-id="e0e63-168">Cela peut interférer avec le modèle de rendu déclaratif.</span><span class="sxs-lookup"><span data-stu-id="e0e63-168">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="e0e63-169">En ce qui concerne le code .net, un `ElementReference` est un handle opaque.</span><span class="sxs-lookup"><span data-stu-id="e0e63-169">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="e0e63-170">La *seule* chose que vous pouvez faire `ElementReference` avec est de la passer au code JavaScript via l’interopérabilité JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e0e63-170">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="e0e63-171">Dans ce cas, le code JavaScript reçoit une `HTMLElement` instance, qu’il peut utiliser avec les API DOM normales.</span><span class="sxs-lookup"><span data-stu-id="e0e63-171">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="e0e63-172">Par exemple, le code suivant définit une méthode d’extension .NET qui permet de définir le focus sur un élément :</span><span class="sxs-lookup"><span data-stu-id="e0e63-172">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="e0e63-173">*exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="e0e63-173">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="e0e63-174">Utilisez `IJSRuntime.InvokeAsync<T>` et appelez `exampleJsFunctions.focusElement` avec un `ElementReference` pour concentrer un élément :</span><span class="sxs-lookup"><span data-stu-id="e0e63-174">Use `IJSRuntime.InvokeAsync<T>` and call `exampleJsFunctions.focusElement` with an `ElementReference` to focus an element:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="e0e63-175">Pour utiliser une méthode d’extension pour le focus d’un élément, créez une méthode d’extension `IJSRuntime` statique qui reçoit l’instance :</span><span class="sxs-lookup"><span data-stu-id="e0e63-175">To use an extension method to focus an element, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="e0e63-176">La méthode est appelée directement sur l’objet.</span><span class="sxs-lookup"><span data-stu-id="e0e63-176">The method is called directly on the object.</span></span> <span data-ttu-id="e0e63-177">L’exemple suivant suppose que la méthode statique `Focus` est disponible à partir de `JsInteropClasses` l’espace de noms :</span><span class="sxs-lookup"><span data-stu-id="e0e63-177">The following example assumes that the static `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,12)]

> [!IMPORTANT]
> <span data-ttu-id="e0e63-178">La `username` variable est remplie uniquement après le rendu du composant.</span><span class="sxs-lookup"><span data-stu-id="e0e63-178">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="e0e63-179">Si un non rempli `ElementReference` est passé à du code JavaScript, le code JavaScript reçoit la `null`valeur.</span><span class="sxs-lookup"><span data-stu-id="e0e63-179">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="e0e63-180">Pour manipuler des références d’élément une fois que le composant a terminé le rendu (pour définir le focus initial sur `OnAfterRenderAsync` un `OnAfterRender` élément), utilisez les méthodes de cycle de vie des [composants](xref:blazor/components#lifecycle-methods)ou.</span><span class="sxs-lookup"><span data-stu-id="e0e63-180">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:blazor/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="e0e63-181">Appeler des méthodes .NET à partir de fonctions JavaScript</span><span class="sxs-lookup"><span data-stu-id="e0e63-181">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="e0e63-182">Appel de méthode .NET statique</span><span class="sxs-lookup"><span data-stu-id="e0e63-182">Static .NET method call</span></span>

<span data-ttu-id="e0e63-183">Pour appeler une méthode .net statique à partir de JavaScript, `DotNet.invokeMethod` utilisez `DotNet.invokeMethodAsync` les fonctions ou.</span><span class="sxs-lookup"><span data-stu-id="e0e63-183">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="e0e63-184">Transmettez l’identificateur de la méthode statique que vous souhaitez appeler, le nom de l’assembly contenant la fonction et les arguments éventuels.</span><span class="sxs-lookup"><span data-stu-id="e0e63-184">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="e0e63-185">La version asynchrone est préférable à la prise en charge des scénarios de serveur éblouissants.</span><span class="sxs-lookup"><span data-stu-id="e0e63-185">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="e0e63-186">Pour appeler une méthode .net à partir de JavaScript, la méthode .net doit être publique, statique et avoir `[JSInvokable]` l’attribut.</span><span class="sxs-lookup"><span data-stu-id="e0e63-186">To invoke a .NET method from JavaScript, the .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="e0e63-187">Par défaut, l’identificateur de méthode est le nom de la méthode, mais vous pouvez spécifier un identificateur différent à `JSInvokableAttribute` l’aide du constructeur.</span><span class="sxs-lookup"><span data-stu-id="e0e63-187">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="e0e63-188">L’appel de méthodes génériques ouvertes n’est pas pris en charge actuellement.</span><span class="sxs-lookup"><span data-stu-id="e0e63-188">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="e0e63-189">L’exemple d’application comprend C# une méthode pour retourner un tableau `int`de.</span><span class="sxs-lookup"><span data-stu-id="e0e63-189">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="e0e63-190">L' `JSInvokable` attribut est appliqué à la méthode.</span><span class="sxs-lookup"><span data-stu-id="e0e63-190">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="e0e63-191">*Pages/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="e0e63-191">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

<span data-ttu-id="e0e63-192">JavaScript traité au client appelle la C# méthode .net.</span><span class="sxs-lookup"><span data-stu-id="e0e63-192">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="e0e63-193">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="e0e63-193">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="e0e63-194">Quand le bouton déclencher l’ReturnArrayAsync de la **méthode statique .net** est sélectionné, examinez la sortie de la console dans les outils de développement Web du navigateur.</span><span class="sxs-lookup"><span data-stu-id="e0e63-194">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="e0e63-195">La sortie de la console est la suivante :</span><span class="sxs-lookup"><span data-stu-id="e0e63-195">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="e0e63-196">La quatrième valeur de tableau fait l’objet d’un`data.push(4);`push dans le `ReturnArrayAsync`tableau () retourné par.</span><span class="sxs-lookup"><span data-stu-id="e0e63-196">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="e0e63-197">Appel de méthode d’instance</span><span class="sxs-lookup"><span data-stu-id="e0e63-197">Instance method call</span></span>

<span data-ttu-id="e0e63-198">Vous pouvez également appeler des méthodes d’instance .NET à partir de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e0e63-198">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="e0e63-199">Pour appeler une méthode d’instance .NET à partir de JavaScript :</span><span class="sxs-lookup"><span data-stu-id="e0e63-199">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="e0e63-200">Transmettez l’instance .net à JavaScript en l’encapsulant `DotNetObjectReference` dans une instance.</span><span class="sxs-lookup"><span data-stu-id="e0e63-200">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectReference` instance.</span></span> <span data-ttu-id="e0e63-201">L’instance .NET est passée par référence à JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e0e63-201">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="e0e63-202">Appeler des méthodes d’instance .net sur l’instance `invokeMethod` à `invokeMethodAsync` l’aide des fonctions ou.</span><span class="sxs-lookup"><span data-stu-id="e0e63-202">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="e0e63-203">L’instance .NET peut également être passée comme argument lors de l’appel d’autres méthodes .NET à partir de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e0e63-203">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="e0e63-204">L’exemple d’application enregistre les messages dans la console côté client.</span><span class="sxs-lookup"><span data-stu-id="e0e63-204">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="e0e63-205">Pour les exemples suivants présentés dans l’exemple d’application, examinez la sortie de console du navigateur dans les outils de développement du navigateur.</span><span class="sxs-lookup"><span data-stu-id="e0e63-205">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="e0e63-206">Lorsque le bouton de **méthode d’instance .net de déclenchement HelloHelper. SayHello** est sélectionné, `ExampleJsInterop.CallHelloHelperSayHello` est appelé et passe un nom, `Blazor`, à la méthode.</span><span class="sxs-lookup"><span data-stu-id="e0e63-206">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="e0e63-207">*Pages/JsInterop. Razor*:</span><span class="sxs-lookup"><span data-stu-id="e0e63-207">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

<span data-ttu-id="e0e63-208">`CallHelloHelperSayHello`appelle la fonction `sayHello` JavaScript avec une nouvelle instance de `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="e0e63-208">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="e0e63-209">*JsInteropClasses/ExampleJsInterop. cs*:</span><span class="sxs-lookup"><span data-stu-id="e0e63-209">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="e0e63-210">*wwwroot/exampleJsInterop. js*:</span><span class="sxs-lookup"><span data-stu-id="e0e63-210">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="e0e63-211">Le nom est passé au `HelloHelper`constructeur de, qui définit la `HelloHelper.Name` propriété.</span><span class="sxs-lookup"><span data-stu-id="e0e63-211">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="e0e63-212">Lorsque la fonction `sayHello` JavaScript est exécutée `HelloHelper.SayHello` , retourne `Hello, {Name}!` le message, qui est écrit dans la console par la fonction JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e0e63-212">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="e0e63-213">*JsInteropClasses/HelloHelper.cs*:</span><span class="sxs-lookup"><span data-stu-id="e0e63-213">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="e0e63-214">Sortie de la console dans les outils de développement Web du navigateur :</span><span class="sxs-lookup"><span data-stu-id="e0e63-214">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="e0e63-215">Partager du code d’interopérabilité dans une bibliothèque de classes</span><span class="sxs-lookup"><span data-stu-id="e0e63-215">Share interop code in a class library</span></span>

<span data-ttu-id="e0e63-216">Le code JavaScript Interop peut être inclus dans une bibliothèque de classes, ce qui vous permet de partager le code dans un package NuGet.</span><span class="sxs-lookup"><span data-stu-id="e0e63-216">JavaScript interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="e0e63-217">La bibliothèque de classes gère l’incorporation des ressources JavaScript dans l’assembly généré.</span><span class="sxs-lookup"><span data-stu-id="e0e63-217">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="e0e63-218">Les fichiers JavaScript sont placés dans le dossier *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="e0e63-218">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="e0e63-219">Les outils s’occupent de l’incorporation des ressources lors de la génération de la bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="e0e63-219">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="e0e63-220">Le package NuGet créé est référencé dans le fichier projet de l’application de la même façon que n’importe quel package NuGet.</span><span class="sxs-lookup"><span data-stu-id="e0e63-220">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="e0e63-221">Une fois le package restauré, le code d’application peut appeler JavaScript comme s’il C#avait été.</span><span class="sxs-lookup"><span data-stu-id="e0e63-221">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="e0e63-222">Pour plus d'informations, consultez <xref:blazor/class-libraries>.</span><span class="sxs-lookup"><span data-stu-id="e0e63-222">For more information, see <xref:blazor/class-libraries>.</span></span>

## <a name="harden-js-interop-calls"></a><span data-ttu-id="e0e63-223">Sécuriser les appels d’interopérabilité JS</span><span class="sxs-lookup"><span data-stu-id="e0e63-223">Harden JS interop calls</span></span>

<span data-ttu-id="e0e63-224">L’interopérabilité de JS peut échouer en raison d’erreurs réseau et doit être considérée comme non fiable.</span><span class="sxs-lookup"><span data-stu-id="e0e63-224">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="e0e63-225">Par défaut, une application de serveur éblouissante expire des appels d’interopérabilité JS sur le serveur après une minute.</span><span class="sxs-lookup"><span data-stu-id="e0e63-225">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="e0e63-226">Si une application peut tolérer un délai d’expiration plus agressif, par exemple 10 secondes, définissez le délai d’expiration à l’aide de l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="e0e63-226">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="e0e63-227">Globalement dans `Startup.ConfigureServices`, spécifiez le délai d’expiration :</span><span class="sxs-lookup"><span data-stu-id="e0e63-227">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="e0e63-228">Par appel dans le code du composant, un appel unique peut spécifier le délai d’attente :</span><span class="sxs-lookup"><span data-stu-id="e0e63-228">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="e0e63-229">Pour plus d’informations sur l’épuisement des <xref:security/blazor/server>ressources, consultez.</span><span class="sxs-lookup"><span data-stu-id="e0e63-229">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>
