Bien qu’une application côté serveur éblouissante soit prérestitué, certaines actions, telles que l’appel de JavaScript, ne sont pas possibles, car une connexion avec le navigateur n’a pas été établie. Les composants peuvent avoir besoin d’être restitués différemment lorsqu’ils sont prérendus.

Pour différer les appels Interop JavaScript jusqu’à ce que la connexion avec le navigateur soit établie, `OnAfterRenderAsync` vous pouvez utiliser l’événement du cycle de vie du composant. Cet événement est appelé uniquement une fois que l’application est entièrement rendue et que la connexion cliente est établie.

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

Le composant suivant montre comment utiliser l’interopérabilité JavaScript dans le cadre de la logique d’initialisation d’un composant d’une manière compatible avec le prérendu. Le composant montre qu’il est possible de déclencher une mise à jour de `OnAfterRenderAsync`rendu depuis l’intérieur. Le développeur doit éviter de créer une boucle infinie dans ce scénario.

Où `JSRuntime.InvokeAsync` est appelé, `ElementRef` est utilisé uniquement dans `OnAfterRenderAsync` et non dans une méthode de cycle de vie antérieure, car il n’y a pas d’élément JavaScript tant que le composant n’est pas rendu.

`StateHasChanged`est appelé pour restituer à nouveau le composant avec le nouvel état obtenu à partir de l’appel JavaScript Interop. Le code ne crée pas de boucle infinie, car `StateHasChanged` est `null`appelé uniquement lorsque `infoFromJs` est.

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

Pour restituer de façon conditionnelle un contenu différent selon que l’application est actuellement prérendu du contenu, `IsConnected` utilisez la propriété `IComponentContext` sur le service. En cas d’exécution côté serveur `IsConnected` , retourne `true` uniquement s’il existe une connexion active au client. Elle retourne `true` toujours lorsqu’elle est exécutée côté client.

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
