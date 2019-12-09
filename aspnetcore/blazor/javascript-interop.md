---
title: ASP.NET Core Blazor l’interopérabilité JavaScript
author: guardrex
description: Découvrez comment appeler des fonctions JavaScript à partir de méthodes .NET et .NET à partir de JavaScript dans des applications Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/javascript-interop
ms.openlocfilehash: 05225b86701b7a5d5c84dd43afbef70dd1ece228
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944068"
---
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a>ASP.NET Core Blazor l’interopérabilité JavaScript

Par [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)et [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Une application Blazor peut appeler des fonctions JavaScript à partir de méthodes .NET et .NET à partir de code JavaScript.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="invoke-javascript-functions-from-net-methods"></a>Appeler des fonctions JavaScript à partir de méthodes .NET

Il arrive parfois que le code .NET soit requis pour appeler une fonction JavaScript. Par exemple, un appel JavaScript peut exposer des fonctionnalités de navigateur ou des fonctionnalités d’une bibliothèque JavaScript à l’application. Ce scénario s’appelle *l’interopérabilité de JavaScript* (*js Interop*).

Pour appeler JavaScript à partir de .NET, utilisez l’abstraction `IJSRuntime`. La méthode `InvokeAsync<T>` accepte un identificateur pour la fonction JavaScript que vous souhaitez appeler, ainsi qu’un nombre quelconque d’arguments sérialisables JSON. L’identificateur de fonction est relatif à la portée globale (`window`). Si vous souhaitez appeler `window.someScope.someFunction`, l’identificateur est `someScope.someFunction`. Il n’est pas nécessaire d’inscrire la fonction avant qu’elle ne soit appelée. Le type de retour `T` doit également être sérialisable JSON. `T` doit correspondre au type .NET qui correspond le mieux au type JSON retourné.

Pour les applications Blazor Server :

* Plusieurs demandes utilisateur sont traitées par l’application Blazor Server. N’appelez pas `JSRuntime.Current` dans un composant pour appeler des fonctions JavaScript.
* Injectez le `IJSRuntime` abstraction et utilisez l’objet injecté pour émettre des appels d’interopérabilité JS.
* Lorsqu’une application Blazor est prérendu, l’appel à JavaScript n’est pas possible, car une connexion au navigateur n’a pas été établie. Pour plus d’informations, consultez la section [détecter le prérendu d’une application Blazor](#detect-when-a-blazor-app-is-prerendering) .

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

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=15)]

*Pages/_Host. cshtml* (serveurBlazor) :

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=21)]

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

## <a name="invoke-net-methods-from-javascript-functions"></a>Appeler des méthodes .NET à partir de fonctions JavaScript

### <a name="static-net-method-call"></a>Appel de méthode .NET statique

Pour appeler une méthode .NET statique à partir de JavaScript, utilisez les fonctions `DotNet.invokeMethod` ou `DotNet.invokeMethodAsync`. Transmettez l’identificateur de la méthode statique que vous souhaitez appeler, le nom de l’assembly contenant la fonction et les arguments éventuels. La version asynchrone est recommandée pour prendre en charge les scénarios de serveur Blazor. Pour appeler une méthode .NET à partir de JavaScript, la méthode .NET doit être publique, statique et avoir l’attribut `[JSInvokable]`. Par défaut, l’identificateur de méthode est le nom de la méthode, mais vous pouvez spécifier un identificateur différent à l’aide du constructeur `JSInvokableAttribute`. L’appel de méthodes génériques ouvertes n’est pas pris en charge actuellement.

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

*JsInteropClasses/HelloHelper.cs*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Sortie de la console dans les outils de développement Web du navigateur :

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a>Partager du code d’interopérabilité dans une bibliothèque de classes

Le code d’interopérabilité JS peut être inclus dans une bibliothèque de classes, ce qui vous permet de partager le code dans un package NuGet.

La bibliothèque de classes gère l’incorporation des ressources JavaScript dans l’assembly généré. Les fichiers JavaScript sont placés dans le dossier *wwwroot* . Les outils s’occupent de l’incorporation des ressources lors de la génération de la bibliothèque.

Le package NuGet créé est référencé dans le fichier projet de l’application de la même façon que n’importe quel package NuGet. Une fois le package restauré, le code d’application peut appeler JavaScript comme s’il C#avait été.

Pour plus d'informations, consultez <xref:blazor/class-libraries>.

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

## <a name="additional-resources"></a>Ressources supplémentaires

* [Exemple InteropComponent. Razor (ASPNET/AspNetCore GitHub, version 3,0)](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
