<span data-ttu-id="ae007-101">Bien qu’une application de serveur éblouissante soit prérendue, certaines actions, telles que l’appel en JavaScript, ne sont pas possibles, car une connexion avec le navigateur n’a pas été établie.</span><span class="sxs-lookup"><span data-stu-id="ae007-101">While a Blazor Server app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="ae007-102">Les composants peuvent avoir besoin d’être restitués différemment lorsqu’ils sont prérendus.</span><span class="sxs-lookup"><span data-stu-id="ae007-102">Components may need to render differently when prerendered.</span></span>

<span data-ttu-id="ae007-103">Pour différer les appels Interop JavaScript jusqu’à ce que la connexion avec le navigateur soit établie, vous pouvez utiliser l’événement `OnAfterRenderAsync` cycle de vie du composant.</span><span class="sxs-lookup"><span data-stu-id="ae007-103">To delay JavaScript interop calls until after the connection with the browser is established, you can use the `OnAfterRenderAsync` component lifecycle event.</span></span> <span data-ttu-id="ae007-104">Cet événement est appelé uniquement une fois que l’application est entièrement rendue et que la connexion cliente est établie.</span><span class="sxs-lookup"><span data-stu-id="ae007-104">This event is only called after the app is fully rendered and the client connection is established.</span></span>

```cshtml
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<input @ref="myInput" value="Value set during render" />

@code {
    private ElementReference myInput;

    protected override void OnAfterRender(bool firstRender)
    {
        if (firstRender)
        {
            JSRuntime.InvokeVoidAsync(
                "setElementValue", myInput, "Value set after render");
        }
    }
}
```

<span data-ttu-id="ae007-105">Le composant suivant montre comment utiliser l’interopérabilité JavaScript dans le cadre de la logique d’initialisation d’un composant d’une manière compatible avec le prérendu.</span><span class="sxs-lookup"><span data-stu-id="ae007-105">The following component demonstrates how to use JavaScript interop as part of a component's initialization logic in a way that's compatible with prerendering.</span></span> <span data-ttu-id="ae007-106">Le composant montre qu’il est possible de déclencher une mise à jour de rendu depuis l’intérieur de `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="ae007-106">The component shows that it's possible to trigger a rendering update from inside `OnAfterRenderAsync`.</span></span> <span data-ttu-id="ae007-107">Le développeur doit éviter de créer une boucle infinie dans ce scénario.</span><span class="sxs-lookup"><span data-stu-id="ae007-107">The developer must avoid creating an infinite loop in this scenario.</span></span>

<span data-ttu-id="ae007-108">Lorsque `JSRuntime.InvokeAsync` est appelée, `ElementRef` est utilisé uniquement dans `OnAfterRenderAsync` et non dans une méthode de cycle de vie antérieure, car il n’y a pas d’élément JavaScript tant que le composant n’est pas rendu.</span><span class="sxs-lookup"><span data-stu-id="ae007-108">Where `JSRuntime.InvokeAsync` is called, `ElementRef` is only used in `OnAfterRenderAsync` and not in any earlier lifecycle method because there's no JavaScript element until after the component is rendered.</span></span>

<span data-ttu-id="ae007-109">`StateHasChanged` est appelé pour rerestituer le composant avec le nouvel état obtenu à partir de l’appel JavaScript Interop.</span><span class="sxs-lookup"><span data-stu-id="ae007-109">`StateHasChanged` is called to rerender the component with the new state obtained from the JavaScript interop call.</span></span> <span data-ttu-id="ae007-110">Le code ne crée pas de boucle infinie car `StateHasChanged` est appelé uniquement lorsque `infoFromJs` est `null`.</span><span class="sxs-lookup"><span data-stu-id="ae007-110">The code doesn't create an infinite loop because `StateHasChanged` is only called when `infoFromJs` is `null`.</span></span>

```cshtml
@page "/prerendered-interop"
@using Microsoft.AspNetCore.Components
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<p>
    Get value via JS interop call:
    <strong id="val-get-by-interop">@(infoFromJs ?? "No value yet")</strong>
</p>

<p>
    Set value via JS interop call:
    <input id="val-set-by-interop" @ref="myElem" />
</p>

@code {
    private string infoFromJs;
    private ElementReference myElem;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender && infoFromJs == null)
        {
            infoFromJs = await JSRuntime.InvokeAsync<string>(
                "setElementValue", myElem, "Hello from interop call");

            StateHasChanged();
        }
    }
}
```
