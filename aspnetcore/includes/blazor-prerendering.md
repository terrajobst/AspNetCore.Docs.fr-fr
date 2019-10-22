<span data-ttu-id="e227d-101">Bien qu’une application de serveur éblouissante soit prérendue, certaines actions, telles que l’appel en JavaScript, ne sont pas possibles, car une connexion avec le navigateur n’a pas été établie.</span><span class="sxs-lookup"><span data-stu-id="e227d-101">While a Blazor Server app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="e227d-102">Les composants peuvent avoir besoin d’être restitués différemment lorsqu’ils sont prérendus.</span><span class="sxs-lookup"><span data-stu-id="e227d-102">Components may need to render differently when prerendered.</span></span>

<span data-ttu-id="e227d-103">Pour différer les appels Interop JavaScript jusqu’à ce que la connexion avec le navigateur soit établie, vous pouvez utiliser l’événement `OnAfterRenderAsync` cycle de vie du composant.</span><span class="sxs-lookup"><span data-stu-id="e227d-103">To delay JavaScript interop calls until after the connection with the browser is established, you can use the `OnAfterRenderAsync` component lifecycle event.</span></span> <span data-ttu-id="e227d-104">Cet événement est appelé uniquement une fois que l’application est entièrement rendue et que la connexion cliente est établie.</span><span class="sxs-lookup"><span data-stu-id="e227d-104">This event is only called after the app is fully rendered and the client connection is established.</span></span>

```cshtml
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<div @ref="divElement">Text during render</div>

@code {
    private ElementReference divElement;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            await JSRuntime.InvokeVoidAsync(
                "setElementText", divElement, "Text after render");
        }
    }
}
```

<span data-ttu-id="e227d-105">Pour l’exemple de code précédent, fournissez une fonction JavaScript `setElementText` à l’intérieur de l’élément `<head>` de *wwwroot/index.html* (éblouissante webassembly) ou *pages/_Host. cshtml* (serveur éblouissant).</span><span class="sxs-lookup"><span data-stu-id="e227d-105">For the preceding example code, provide a `setElementText` JavaScript function inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> <span data-ttu-id="e227d-106">La fonction est appelée avec `IJSRuntime.InvokeVoidAsync` et ne retourne pas de valeur :</span><span class="sxs-lookup"><span data-stu-id="e227d-106">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

```html
<!--  -->
<script>
  window.setElementText = (element, text) => element.innerText = text;
</script>
```

> [!WARNING]
> <span data-ttu-id="e227d-107">L’exemple précédent modifie directement le Document Object Model (DOM) à des fins de démonstration uniquement.</span><span class="sxs-lookup"><span data-stu-id="e227d-107">The preceding example modifies the Document Object Model (DOM) directly for demonstration purposes only.</span></span> <span data-ttu-id="e227d-108">La modification directe du DOM avec JavaScript n’est pas recommandée dans la plupart des scénarios, car JavaScript peut interférer avec le suivi des modifications de éblouissant.</span><span class="sxs-lookup"><span data-stu-id="e227d-108">Directly modifying the DOM with JavaScript isn't recommended in most scenarios because JavaScript can interfere with Blazor's change tracking.</span></span>

<span data-ttu-id="e227d-109">Le composant suivant montre comment utiliser l’interopérabilité JavaScript dans le cadre de la logique d’initialisation d’un composant d’une manière compatible avec le prérendu.</span><span class="sxs-lookup"><span data-stu-id="e227d-109">The following component demonstrates how to use JavaScript interop as part of a component's initialization logic in a way that's compatible with prerendering.</span></span> <span data-ttu-id="e227d-110">Le composant montre qu’il est possible de déclencher une mise à jour de rendu depuis l’intérieur de `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="e227d-110">The component shows that it's possible to trigger a rendering update from inside `OnAfterRenderAsync`.</span></span> <span data-ttu-id="e227d-111">Le développeur doit éviter de créer une boucle infinie dans ce scénario.</span><span class="sxs-lookup"><span data-stu-id="e227d-111">The developer must avoid creating an infinite loop in this scenario.</span></span>

<span data-ttu-id="e227d-112">Lorsque `JSRuntime.InvokeAsync` est appelée, `ElementRef` est utilisé uniquement dans `OnAfterRenderAsync` et non dans une méthode de cycle de vie antérieure, car il n’y a pas d’élément JavaScript tant que le composant n’est pas rendu.</span><span class="sxs-lookup"><span data-stu-id="e227d-112">Where `JSRuntime.InvokeAsync` is called, `ElementRef` is only used in `OnAfterRenderAsync` and not in any earlier lifecycle method because there's no JavaScript element until after the component is rendered.</span></span>

<span data-ttu-id="e227d-113">`StateHasChanged` est appelé pour rerestituer le composant avec le nouvel état obtenu à partir de l’appel JavaScript Interop.</span><span class="sxs-lookup"><span data-stu-id="e227d-113">`StateHasChanged` is called to rerender the component with the new state obtained from the JavaScript interop call.</span></span> <span data-ttu-id="e227d-114">Le code ne crée pas de boucle infinie car `StateHasChanged` est appelé uniquement lorsque `infoFromJs` est `null`.</span><span class="sxs-lookup"><span data-stu-id="e227d-114">The code doesn't create an infinite loop because `StateHasChanged` is only called when `infoFromJs` is `null`.</span></span>

```cshtml
@page "/prerendered-interop"
@using Microsoft.AspNetCore.Components
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<p>
    Get value via JS interop call:
    <strong id="val-get-by-interop">@(infoFromJs ?? "No value yet")</strong>
</p>

Set value via JS interop call:
<div id="val-set-by-interop" @ref="divElement"></div>

@code {
    private string infoFromJs;
    private ElementReference divElement;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender && infoFromJs == null)
        {
            infoFromJs = await JSRuntime.InvokeAsync<string>(
                "setElementText", divElement, "Hello from interop call!");

            StateHasChanged();
        }
    }
}
```

<span data-ttu-id="e227d-115">Pour l’exemple de code précédent, fournissez une fonction JavaScript `setElementText` à l’intérieur de l’élément `<head>` de *wwwroot/index.html* (éblouissante webassembly) ou *pages/_Host. cshtml* (serveur éblouissant).</span><span class="sxs-lookup"><span data-stu-id="e227d-115">For the preceding example code, provide a `setElementText` JavaScript function inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> <span data-ttu-id="e227d-116">La fonction est appelée avec `IJSRuntime.InvokeAsync` et retourne une valeur :</span><span class="sxs-lookup"><span data-stu-id="e227d-116">The function is called with `IJSRuntime.InvokeAsync` and returns a value:</span></span>

```html
<script>
  window.setElementText = (element, text) => {
    element.innerText = text;
    return text;
  };
</script>
```

> [!WARNING]
> <span data-ttu-id="e227d-117">L’exemple précédent modifie directement le Document Object Model (DOM) à des fins de démonstration uniquement.</span><span class="sxs-lookup"><span data-stu-id="e227d-117">The preceding example modifies the Document Object Model (DOM) directly for demonstration purposes only.</span></span> <span data-ttu-id="e227d-118">La modification directe du DOM avec JavaScript n’est pas recommandée dans la plupart des scénarios, car JavaScript peut interférer avec le suivi des modifications de éblouissant.</span><span class="sxs-lookup"><span data-stu-id="e227d-118">Directly modifying the DOM with JavaScript isn't recommended in most scenarios because JavaScript can interfere with Blazor's change tracking.</span></span>
