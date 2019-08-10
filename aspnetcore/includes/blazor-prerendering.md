<span data-ttu-id="20fac-101">Bien qu’une application côté serveur éblouissante soit prérestitué, certaines actions, telles que l’appel de JavaScript, ne sont pas possibles, car une connexion avec le navigateur n’a pas été établie.</span><span class="sxs-lookup"><span data-stu-id="20fac-101">While a Blazor server-side app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="20fac-102">Les composants peuvent avoir besoin d’être restitués différemment lorsqu’ils sont prérendus.</span><span class="sxs-lookup"><span data-stu-id="20fac-102">Components may need to render differently when prerendered.</span></span>

<span data-ttu-id="20fac-103">Pour différer les appels Interop JavaScript jusqu’à ce que la connexion avec le navigateur soit établie, `OnAfterRenderAsync` vous pouvez utiliser l’événement du cycle de vie du composant.</span><span class="sxs-lookup"><span data-stu-id="20fac-103">To delay JavaScript interop calls until after the connection with the browser is established, you can use the `OnAfterRenderAsync` component lifecycle event.</span></span> <span data-ttu-id="20fac-104">Cet événement est appelé uniquement une fois que l’application est entièrement rendue et que la connexion cliente est établie.</span><span class="sxs-lookup"><span data-stu-id="20fac-104">This event is only called after the app is fully rendered and the client connection is established.</span></span>

```cshtml
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<input @ref="myInput" value="Value set during render" />

@code {
    private ElementRef myInput;

    protected override void OnAfterRender()
    {
        JSRuntime.InvokeAsync<object>(
            "setElementValue", myInput, "Value set after render");
    }
}
```

<span data-ttu-id="20fac-105">Le composant suivant montre comment utiliser l’interopérabilité JavaScript dans le cadre de la logique d’initialisation d’un composant d’une manière compatible avec le prérendu.</span><span class="sxs-lookup"><span data-stu-id="20fac-105">The following component demonstrates how to use JavaScript interop as part of a component's initialization logic in a way that's compatible with prerendering.</span></span> <span data-ttu-id="20fac-106">Le composant montre qu’il est possible de déclencher une mise à jour de `OnAfterRenderAsync`rendu depuis l’intérieur.</span><span class="sxs-lookup"><span data-stu-id="20fac-106">The component shows that it's possible to trigger a rendering update from inside `OnAfterRenderAsync`.</span></span> <span data-ttu-id="20fac-107">Le développeur doit éviter de créer une boucle infinie dans ce scénario.</span><span class="sxs-lookup"><span data-stu-id="20fac-107">The developer must avoid creating an infinite loop in this scenario.</span></span>

<span data-ttu-id="20fac-108">Où `JSRuntime.InvokeAsync` est appelé, `ElementRef` est utilisé uniquement dans `OnAfterRenderAsync` et non dans une méthode de cycle de vie antérieure, car il n’y a pas d’élément JavaScript tant que le composant n’est pas rendu.</span><span class="sxs-lookup"><span data-stu-id="20fac-108">Where `JSRuntime.InvokeAsync` is called, `ElementRef` is only used in `OnAfterRenderAsync` and not in any earlier lifecycle method because there's no JavaScript element until after the component is rendered.</span></span>

<span data-ttu-id="20fac-109">`StateHasChanged`est appelé pour restituer à nouveau le composant avec le nouvel état obtenu à partir de l’appel JavaScript Interop.</span><span class="sxs-lookup"><span data-stu-id="20fac-109">`StateHasChanged` is called to rerender the component with the new state obtained from the JavaScript interop call.</span></span> <span data-ttu-id="20fac-110">Le code ne crée pas de boucle infinie, car `StateHasChanged` est `null`appelé uniquement lorsque `infoFromJs` est.</span><span class="sxs-lookup"><span data-stu-id="20fac-110">The code doesn't create an infinite loop because `StateHasChanged` is only called when `infoFromJs` is `null`.</span></span>

```cshtml
@page "/prerendered-interop"
@using Microsoft.AspNetCore.Components
@using Microsoft.JSInterop
@inject IComponentContext ComponentContext
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
    private ElementRef myElem;

    protected override async Task OnAfterRenderAsync()
    {
        // TEMPORARY: Currently we need this guard to avoid making the interop
        // call during prerendering. Soon this will be unnecessary because we
        // will change OnAfterRenderAsync so that it won't run during the
        // prerendering phase.
        if (!ComponentContext.IsConnected)
        {
            return;
        }

        if (infoFromJs == null)
        {
            infoFromJs = await JSRuntime.InvokeAsync<string>(
                "setElementValue", myElem, "Hello from interop call");

            StateHasChanged();
        }
    }
}
```

<span data-ttu-id="20fac-111">Pour restituer de façon conditionnelle un contenu différent selon que l’application est actuellement prérendu du contenu, `IsConnected` utilisez la propriété `IComponentContext` sur le service.</span><span class="sxs-lookup"><span data-stu-id="20fac-111">To conditionally render different content based on whether the app is currently prerendering content, use the `IsConnected` property on the `IComponentContext` service.</span></span> <span data-ttu-id="20fac-112">En cas d’exécution côté serveur `IsConnected` , retourne `true` uniquement s’il existe une connexion active au client.</span><span class="sxs-lookup"><span data-stu-id="20fac-112">When running server-side, `IsConnected` only returns `true` if there's an active connection to the client.</span></span> <span data-ttu-id="20fac-113">Elle retourne `true` toujours lorsqu’elle est exécutée côté client.</span><span class="sxs-lookup"><span data-stu-id="20fac-113">It always returns `true` when running client-side.</span></span>

```cshtml
@page "/isconnected-example"
@using Microsoft.AspNetCore.Components.Services
@inject IComponentContext ComponentContext

<h1>IsConnected Example</h1>

<p>
    Current state:
    <strong id="connected-state">
        @(ComponentContext.IsConnected ? "connected" : "not connected")
    </strong>
</p>

<p>
    Clicks:
    <strong id="count">@count</strong>
    <button id="increment-count" @onclick="@(() => count++)">Click me</button>
</p>

@code {
    private int count;
}
```
