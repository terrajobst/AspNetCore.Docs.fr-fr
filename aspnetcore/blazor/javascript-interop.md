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
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a>ASP.NET Core Blazor l’interopérabilité JavaScript

Par [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)et [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Une application Blazor peut appeler des fonctions JavaScript à partir de méthodes .NET et .NET à partir de code JavaScript.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="invoke-javascript-functions-from-net-methods"></a>Appeler des fonctions JavaScript à partir de méthodes .NET

Il arrive parfois que le code .NET soit requis pour appeler une fonction JavaScript. Par exemple, un appel JavaScript peut exposer des fonctionnalités de navigateur ou des fonctionnalités d’une bibliothèque JavaScript à l’application. Ce scénario s’appelle *l’interopérabilité de JavaScript* (*js Interop*).

Pour appeler JavaScript à partir de .NET, utilisez l’abstraction `IJSRuntime`. Pour émettre des appels d’interopérabilité JS, injectez le `IJSRuntime` abstraction dans votre composant. La méthode `InvokeAsync<T>` accepte un identificateur pour la fonction JavaScript que vous souhaitez appeler, ainsi qu’un nombre quelconque d’arguments sérialisables JSON. L’identificateur de fonction est relatif à la portée globale (`window`). Si vous souhaitez appeler `window.someScope.someFunction`, l’identificateur est `someScope.someFunction`. Il n’est pas nécessaire d’inscrire la fonction avant qu’elle ne soit appelée. Le type de retour `T` doit également être sérialisable JSON. `T` doit correspondre au type .NET qui correspond le mieux au type JSON retourné.

Pour les applications Blazor Server avec le prérendu activé, l’appel à JavaScript n’est pas possible pendant le prérendu initial. Les appels Interop JavaScript doivent être différés jusqu’à ce que la connexion avec le navigateur soit établie. Pour plus d’informations, consultez la section [détecter le prérendu d’une application Blazor](#detect-when-a-blazor-app-is-prerendering) .

L’exemple suivant est basé sur [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un décodeur basé sur JavaScript expérimental. L’exemple montre comment appeler une fonction JavaScript à partir d' C# une méthode. La fonction JavaScript accepte un tableau d’octets d' C# une méthode, décode le tableau et retourne le texte au composant pour l’affichage.

À l’intérieur de l’élément `<head>` de *wwwroot/index.html* (Blazor webassembly) ou de *pages/_Host. cshtml* (serveurBlazor), fournissez une fonction JavaScript qui utilise `TextDecoder` pour décoder un tableau passé et retourner la valeur décodée :

[!code-html[](javascript-interop/samples_snapshot/index-script-convertarray.html)]

Du code JavaScript, tel que le code illustré dans l’exemple précédent, peut également être chargé à partir d’un fichier JavaScript ( *. js*) avec une référence au fichier de script :

```html
<script src="exampleJsInterop.js"></script>
```

Le composant suivant :

* Appelle la fonction JavaScript `convertArray` à l’aide de `JSRuntime` lorsqu’un bouton de composant (**convertir un tableau**) est sélectionné.
* Une fois la fonction JavaScript appelée, le tableau passé est converti en chaîne. La chaîne est retournée au composant pour l’affichage.

[!code-razor[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="use-of-ijsruntime"></a>Utilisation de IJSRuntime

Pour utiliser l’abstraction `IJSRuntime`, adoptez l’une des approches suivantes :

* Injecter l’abstraction `IJSRuntime` dans le composant Razor ( *. Razor*) :

  [!code-razor[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  À l’intérieur de l’élément `<head>` de *wwwroot/index.html* (Blazor webassembly) ou de *pages/_Host. cshtml* (serveurBlazor), fournissez une fonction JavaScript `handleTickerChanged`. La fonction est appelée avec `IJSRuntime.InvokeVoidAsync` et ne retourne pas de valeur :

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* Injecter l’abstraction `IJSRuntime` dans une classe ( *. cs*) :

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  À l’intérieur de l’élément `<head>` de *wwwroot/index.html* (Blazor webassembly) ou de *pages/_Host. cshtml* (serveurBlazor), fournissez une fonction JavaScript `handleTickerChanged`. La fonction est appelée avec `JSRuntime.InvokeAsync` et retourne une valeur :

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* Pour la génération de contenu dynamique avec [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), utilisez l’attribut `[Inject]` :

  ```razor
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

Dans l’exemple d’application côté client qui accompagne cette rubrique, deux fonctions JavaScript sont disponibles pour l’application qui interagit avec le DOM pour recevoir l’entrée d’utilisateur et afficher un message d’accueil :

* `showPrompt` &ndash; génère une invite pour accepter l’entrée d’utilisateur (nom de l’utilisateur) et retourne le nom à l’appelant.
* `displayWelcome` &ndash; assigne un message de bienvenue de l’appelant à un objet DOM avec une `id` de `welcome`.

*wwwroot/exampleJsInterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

Placez la balise `<script>` qui fait référence au fichier JavaScript dans le fichier *wwwroot/index.html* (Blazor assembly) ou le fichier *pages/_Host. cshtml* (serveurBlazor).

*wwwroot/index.html* (Blazor webassembly) :

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=22)]

*Pages/_Host. cshtml* (serveurBlazor) :

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=35)]

Ne placez pas de balise `<script>` dans un fichier de composant, car la balise `<script>` ne peut pas être mise à jour dynamiquement.

Les méthodes .NET interagissent avec les fonctions JavaScript dans le fichier *exampleJsInterop. js* en appelant `IJSRuntime.InvokeAsync<T>`.

L’abstraction `IJSRuntime` est asynchrone pour permettre les scénarios de serveur Blazor. Si l’application est une application Blazor webassembly et que vous souhaitez appeler une fonction JavaScript de manière synchrone, vous devez effectuer un cast aval pour `IJSInProcessRuntime` et appeler `Invoke<T>` à la place. Nous recommandons que la plupart des bibliothèques d’interopérabilité JS utilisent les API Async pour s’assurer que les bibliothèques sont disponibles dans tous les scénarios.

L’exemple d’application comprend un composant pour illustrer l’interopérabilité de JS. Le composant :

* Reçoit une entrée d’utilisateur via une invite JavaScript.
* Retourne le texte du composant pour traitement.
* Appelle une deuxième fonction JavaScript qui interagit avec le DOM pour afficher un message d’accueil.

*Pages/JSInterop. Razor*:

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

1. Lorsque `TriggerJsPrompt` est exécuté en sélectionnant le bouton d' **invite de commandes JavaScript** du composant, la fonction JavaScript `showPrompt` fournie dans le fichier *wwwroot/exampleJsInterop. js* est appelée.
1. La fonction `showPrompt` accepte l’entrée d’utilisateur (nom de l’utilisateur), qui est codée au format HTML et renvoyée au composant. Le composant stocke le nom de l’utilisateur dans une variable locale, `name`.
1. La chaîne stockée dans `name` est incorporée dans un message de bienvenue, qui est transmis à une fonction JavaScript, `displayWelcome`, qui affiche le message de bienvenue dans une balise d’en-tête.

## <a name="call-a-void-javascript-function"></a>Appeler une fonction JavaScript void

Les fonctions JavaScript qui retournent [void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) ou [non définies](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) sont appelées avec `IJSRuntime.InvokeVoidAsync`.

## <a name="detect-when-a-opno-locblazor-app-is-prerendering"></a>Détecter quand un Blazor application est prérendu
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a>Capturer des références à des éléments

Certains scénarios d’interopérabilité JS requièrent des références à des éléments HTML. Par exemple, une bibliothèque d’interface utilisateur peut nécessiter une référence d’élément pour l’initialisation, ou vous devrez peut-être appeler des API de type commande sur un élément, par exemple `focus` ou `play`.

Capturer des références aux éléments HTML dans un composant à l’aide de l’approche suivante :

* Ajoutez un attribut `@ref` à l’élément HTML.
* Définissez un champ de type `ElementReference` dont le nom correspond à la valeur de l’attribut `@ref`.

L’exemple suivant illustre la capture d’une référence à l’élément `username` `<input>` :

```razor
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> Utilisez uniquement une référence d’élément pour muter le contenu d’un élément vide qui n’interagit pas avec Blazor. Ce scénario est utile quand une API tierce fournit du contenu à l’élément. Étant donné que Blazor n’interagit pas avec l’élément, il n’existe aucun risque de conflit entre la représentation de Blazorde l’élément et le DOM.
>
> Dans l’exemple suivant, il est *dangereux* de faire muter le contenu de la liste non triée (`ul`), car Blazor interagit avec le DOM pour remplir les éléments de liste de cet élément (`<li>`) :
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
> Si l’interopérabilité JS muter le contenu de l’élément `MyList` et Blazor tente d’appliquer des différences à l’élément, les différences ne correspondent pas au DOM.

En ce qui concerne le code .NET, un `ElementReference` est un handle opaque. La *seule* chose que vous pouvez faire avec `ElementReference` est de la passer au code JavaScript via l’interopérabilité js. Dans ce cas, le code JavaScript reçoit une instance `HTMLElement`, qu’il peut utiliser avec les API DOM normales.

Par exemple, le code suivant définit une méthode d’extension .NET qui permet de définir le focus sur un élément :

*exampleJsInterop. js*:

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

Pour appeler une fonction JavaScript qui ne retourne pas de valeur, utilisez `IJSRuntime.InvokeVoidAsync`. Le code suivant définit le focus sur l’entrée de nom d’utilisateur en appelant la fonction JavaScript précédente avec l' `ElementReference`capturée :

[!code-razor[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

Pour utiliser une méthode d’extension, créez une méthode d’extension statique qui reçoit l’instance `IJSRuntime` :

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

La méthode `Focus` est appelée directement sur l’objet. L’exemple suivant suppose que la méthode `Focus` est disponible à partir de l’espace de noms `JsInteropClasses` :

[!code-razor[](javascript-interop/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> La variable `username` n’est remplie qu’après le rendu du composant. Si un `ElementReference` non rempli est passé au code JavaScript, le code JavaScript reçoit la valeur `null`. Pour manipuler des références d’élément après que le composant a terminé le rendu (pour définir le focus initial sur un élément), utilisez les [méthodes de cycle de vie des composants OnAfterRenderAsync ou OnAfterRender](xref:blazor/lifecycle#after-component-render).

Quand vous utilisez des types génériques et que vous retournez une valeur, utilisez [ValueTask\<t >](xref:System.Threading.Tasks.ValueTask`1):

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

`GenericMethod` est appelé directement sur l’objet avec un type. L’exemple suivant suppose que la `GenericMethod` est disponible à partir de l’espace de noms `JsInteropClasses` :

[!code-razor[](javascript-interop/samples_snapshot/component3.razor?highlight=17)]

## <a name="reference-elements-across-components"></a>Éléments de référence sur les composants

Un `ElementReference` est garanti uniquement valide dans la méthode `OnAfterRender` d’un composant (et une référence d’élément est un `struct`), de sorte qu’une référence d’élément ne peut pas être transmise entre les composants.

Pour qu’un composant parent rende une référence d’élément disponible pour d’autres composants, le composant parent peut :

* Autorisez les composants enfants à inscrire des rappels.
* Appelez les rappels inscrits pendant l’événement `OnAfterRender` avec la référence d’élément passée. Indirectement, cette approche permet aux composants enfants d’interagir avec la référence d’élément du parent.

L’exemple de Blazor webassembly suivant illustre l’approche.

Dans le `<head>` de *wwwroot/index.html*:

```html
<style>
    .red { color: red }
</style>
```

Dans le `<body>` de *wwwroot/index.html*:

```html
<script>
    function setElementClass(element, className) {
        /** @type {HTMLElement} **/
        var myElement = element;
        myElement.classList.add(className);
    }
</script>
```

*Pages/index. Razor* (composant parent) :

```razor
@page "/"

<h1 @ref="_title">Hello, world!</h1>

Welcome to your new app.

<SurveyPrompt Parent="this" Title="How is Blazor working for you?" />
```

*Pages/index. Razor. cs*:

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

*Shared/SurveyPrompt. Razor* (composant enfant) :

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

*Shared/SurveyPrompt. Razor. cs*:

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

## <a name="invoke-net-methods-from-javascript-functions"></a>Appeler des méthodes .NET à partir de fonctions JavaScript

### <a name="static-net-method-call"></a>Appel de méthode .NET statique

Pour appeler une méthode .NET statique à partir de JavaScript, utilisez les fonctions `DotNet.invokeMethod` ou `DotNet.invokeMethodAsync`. Transmettez l’identificateur de la méthode statique que vous souhaitez appeler, le nom de l’assembly contenant la fonction et les arguments éventuels. La version asynchrone est recommandée pour prendre en charge les scénarios de serveur Blazor. La méthode .NET doit être publique, statique et avoir l’attribut `[JSInvokable]`. L’appel de méthodes génériques ouvertes n’est pas pris en charge actuellement.

L’exemple d’application comprend C# une méthode pour retourner un tableau de `int`s. L’attribut `JSInvokable` est appliqué à la méthode.

*Pages/JsInterop. Razor*:

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

JavaScript traité au client appelle la C# méthode .net.

*wwwroot/exampleJsInterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

Quand le bouton déclencher l’ReturnArrayAsync de la **méthode statique .net** est sélectionné, examinez la sortie de la console dans les outils de développement Web du navigateur.

La sortie de la console est la suivante :

```console
Array(4) [ 1, 2, 3, 4 ]
```

La quatrième valeur de tableau fait l’objet d’un push dans le tableau (`data.push(4);`) retourné par `ReturnArrayAsync`.

Par défaut, l’identificateur de méthode est le nom de la méthode, mais vous pouvez spécifier un identificateur différent à l’aide du constructeur `JSInvokableAttribute` :

```csharp
@code {
    [JSInvokable("DifferentMethodName")]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

Dans le fichier JavaScript côté client :

```javascript
returnArrayAsyncJs: function () {
  DotNet.invokeMethodAsync('BlazorSample', 'DifferentMethodName')
    .then(data => {
      data.push(4);
      console.log(data);
    });
}
```

### <a name="instance-method-call"></a>Appel de méthode d’instance

Vous pouvez également appeler des méthodes d’instance .NET à partir de JavaScript. Pour appeler une méthode d’instance .NET à partir de JavaScript :

* Transmettez l’instance .NET à JavaScript en l’encapsulant dans une instance de `DotNetObjectReference`. L’instance .NET est passée par référence à JavaScript.
* Appeler des méthodes d’instance .NET sur l’instance à l’aide des fonctions `invokeMethod` ou `invokeMethodAsync`. L’instance .NET peut également être passée comme argument lors de l’appel d’autres méthodes .NET à partir de JavaScript.

> [!NOTE]
> L’exemple d’application enregistre les messages dans la console côté client. Pour les exemples suivants présentés dans l’exemple d’application, examinez la sortie de console du navigateur dans les outils de développement du navigateur.

Quand le bouton de **méthode d’instance .net de déclenchement HelloHelper. SayHello** est sélectionné, `ExampleJsInterop.CallHelloHelperSayHello` est appelé et passe un nom, `Blazor`, à la méthode.

*Pages/JsInterop. Razor*:

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

`CallHelloHelperSayHello` appelle la fonction JavaScript `sayHello` avec une nouvelle instance de `HelloHelper`.

*JsInteropClasses/ExampleJsInterop. cs*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

*wwwroot/exampleJsInterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

Le nom est passé au constructeur de `HelloHelper`, qui définit la propriété `HelloHelper.Name`. Lorsque la fonction JavaScript `sayHello` est exécutée, `HelloHelper.SayHello` retourne le message `Hello, {Name}!`, qui est écrit dans la console par la fonction JavaScript.

*JsInteropClasses/HelloHelper. cs*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Sortie de la console dans les outils de développement Web du navigateur :

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a>Partager du code d’interopérabilité dans une bibliothèque de classes

Le code d’interopérabilité JS peut être inclus dans une bibliothèque de classes, ce qui vous permet de partager le code dans un package NuGet.

La bibliothèque de classes gère l’incorporation des ressources JavaScript dans l’assembly généré. Les fichiers JavaScript sont placés dans le dossier *wwwroot* . Les outils s’occupent de l’incorporation des ressources lors de la génération de la bibliothèque.

Le package NuGet créé est référencé dans le fichier projet de l’application de la même façon que n’importe quel package NuGet. Une fois le package restauré, le code d’application peut appeler JavaScript comme s’il C#avait été.

Pour plus d’informations, consultez <xref:blazor/class-libraries>.

## <a name="harden-js-interop-calls"></a>Sécuriser les appels d’interopérabilité JS

L’interopérabilité de JS peut échouer en raison d’erreurs réseau et doit être considérée comme non fiable. Par défaut, une application Blazor Server expire les appels d’interopérabilité JS sur le serveur après une minute. Si une application peut tolérer un délai d’expiration plus agressif, par exemple 10 secondes, définissez le délai d’expiration à l’aide de l’une des approches suivantes :

* Globalement dans `Startup.ConfigureServices`, spécifiez le délai d’expiration :

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* Par appel dans le code du composant, un appel unique peut spécifier le délai d’attente :

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

Pour plus d’informations sur l’épuisement des ressources, consultez <xref:security/blazor/server>.

## <a name="perform-large-data-transfers-in-opno-locblazor-server-apps"></a>Effectuer des transferts de données volumineux dans des applications Blazor Server

Dans certains scénarios, de grandes quantités de données doivent être transférées entre JavaScript et Blazor. En général, les transferts de données volumineux se produisent dans les cas suivants :

* Les API du système de fichiers du navigateur sont utilisées pour charger ou télécharger un fichier.
* L’interopérabilité avec une bibliothèque tierce est nécessaire.

Dans Blazor Server, une limitation est en place pour empêcher le passage de messages volumineux uniques susceptibles d’entraîner des problèmes de performances.

Tenez compte des conseils suivants lors du développement de code qui transfère des données entre JavaScript et Blazor:

* Découpez les données en éléments plus petits et envoyez les segments de données de façon séquentielle jusqu’à ce que toutes les données soient reçues par le serveur.
* N’allouez pas d’objets volumineux C# en JavaScript et dans du code.
* Ne bloquez pas le thread d’interface utilisateur principal pendant de longues périodes lors de l’envoi ou de la réception de données.
* Libérez la mémoire consommée lorsque le processus est terminé ou annulé.
* Appliquer les exigences supplémentaires suivantes pour des raisons de sécurité :
  * Déclarez la taille maximale du fichier ou des données qui peut être passée.
  * Déclarez le taux de téléchargement minimal du client au serveur.
* Une fois les données reçues par le serveur, les données peuvent être :
  * Stocké temporairement dans une mémoire tampon jusqu’à ce que tous les segments soient collectés.
  * Consommé immédiatement. Par exemple, les données peuvent être stockées immédiatement dans une base de données ou écrites sur le disque à mesure que chaque segment est reçu.

La classe de chargeur de fichiers suivante gère l’interopérabilité de JS avec le client. La classe de chargeur utilise l’interopérabilité JS pour :

* Interroger le client pour envoyer un segment de données.
* Abandonner la transaction si le délai d’interrogation est dépassé.

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

Dans l'exemple précédent :

* Le `_maxBase64SegmentSize` est défini sur `8192`, calculé à partir de `_maxBase64SegmentSize = _segmentSize * 4 / 3`.
* Les API de gestion de mémoire .NET Core de bas niveau sont utilisées pour stocker les segments de mémoire sur le serveur dans `_uploadedSegments`.
* Une méthode `ReceiveFile` est utilisée pour gérer le téléchargement via l’interopérabilité JS :
  * La taille du fichier est déterminée en octets par le biais de l’interopérabilité JS avec `_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`.
  * Le nombre de segments à recevoir est calculé et stocké dans `numberOfSegments`.
  * Les segments sont demandés dans une boucle `for` via l’interopérabilité JS avec `_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`. Tous les segments, mais le dernier doit être de 8 192 octets avant le décodage. Le client est contraint d’envoyer les données de manière efficace.
  * Pour chaque segment reçu, les vérifications sont effectuées avant le décodage avec <xref:System.Convert.TryFromBase64String*>.
  * Un flux avec les données est retourné sous la forme d’un nouveau <xref:System.IO.Stream> (`SegmentedStream`) une fois le téléchargement terminé.

La classe de flux segmentée expose la liste de segments sous la forme d’un <xref:System.IO.Stream>ReadOnly non identifiable :

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

Le code suivant implémente les fonctions JavaScript pour recevoir les données :

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

## <a name="additional-resources"></a>Ressources supplémentaires

* [Exemple InteropComponent. Razor (référentiel GitHub dotnet/AspNetCore, branche de version 3,1)](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
