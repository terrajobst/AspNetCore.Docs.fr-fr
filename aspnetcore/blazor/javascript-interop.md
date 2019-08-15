---
title: ASP.NET Core l’interopérabilité avec le JavaScript éblouissant
author: guardrex
description: Découvrez comment appeler des fonctions JavaScript à partir de méthodes .NET et .NET à partir de JavaScript dans les applications éblouissantes.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/javascript-interop
ms.openlocfilehash: ffd25fe0288159681f7fc052fc09e1f6fc425404
ms.sourcegitcommit: f5f0ff65d4e2a961939762fb00e654491a2c772a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030304"
---
# <a name="aspnet-core-blazor-javascript-interop"></a>ASP.NET Core l’interopérabilité avec le JavaScript éblouissant

Par [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27)et [Luke Latham](https://github.com/guardrex)

Une application éblouissant peut appeler des fonctions JavaScript à partir de méthodes .NET et .NET à partir de code JavaScript.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="invoke-javascript-functions-from-net-methods"></a>Appeler des fonctions JavaScript à partir de méthodes .NET

Il arrive parfois que le code .NET soit requis pour appeler une fonction JavaScript. Par exemple, un appel JavaScript peut exposer des fonctionnalités de navigateur ou des fonctionnalités d’une bibliothèque JavaScript à l’application.

Pour appeler JavaScript à partir de .net, utilisez `IJSRuntime` l’abstraction. La `InvokeAsync<T>` méthode prend un identificateur pour la fonction JavaScript que vous souhaitez appeler, ainsi que n’importe quel nombre d’arguments sérialisables JSON. L’identificateur de fonction est relatif à la portée globale (`window`). Si vous souhaitez appeler `window.someScope.someFunction`, l’identificateur est. `someScope.someFunction` Il n’est pas nécessaire d’inscrire la fonction avant qu’elle ne soit appelée. Le type `T` de retour doit également être sérialisable JSON.

Pour les applications côté serveur:

* Plusieurs demandes utilisateur sont traitées par l’application côté serveur. N’appelez `JSRuntime.Current` pas dans un composant pour appeler des fonctions JavaScript.
* Injectez `IJSRuntime` l’abstraction et utilisez l’objet injecté pour émettre des appels Interop JavaScript.
* Lorsqu’une application éblouissant est prérendue, l’appel à JavaScript n’est pas possible, car une connexion avec le navigateur n’a pas été établie. Pour plus d’informations, consultez la section [détecter quand une application éblouissant est un prérendu](#detect-when-a-blazor-app-is-prerendering) .

L’exemple suivant est basé sur [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un décodeur basé sur JavaScript expérimental. L’exemple montre comment appeler une fonction JavaScript à partir d' C# une méthode. La fonction JavaScript accepte un tableau d’octets d' C# une méthode, décode le tableau et retourne le texte au composant pour l’affichage.

À l' `<head>` intérieur de l’élément de *wwwroot/index.html* (éblouissant Client-Side) ou *pages/_Host. cshtml* (le côté serveur de éblouissant), fournissez une `TextDecoder` fonction qui utilise pour décoder un tableau passé:

[!code-html[](javascript-interop/samples_snapshot/index-script.html)]

Du code JavaScript, tel que le code illustré dans l’exemple précédent, peut également être chargé à partir d’un fichier JavaScript ( *. js*) avec une référence au fichier de script:

```html
<script src="exampleJsInterop.js"></script>
```

Le composant suivant:

* Appelle la `ConvertArray` fonction JavaScript à l' `JsRuntime` aide de lorsqu’un bouton de composant (**convertir un tableau**) est sélectionné.
* Une fois la fonction JavaScript appelée, le tableau passé est converti en chaîne. La chaîne est retournée au composant pour l’affichage.

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

Pour utiliser l' `IJSRuntime` abstraction, adoptez l’une des approches suivantes:

* Injecter `IJSRuntime` l’abstraction dans le composant Razor ( *. Razor*):

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

* Injecter `IJSRuntime` l’abstraction dans une classe ( *. cs*):

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

* Pour la génération de contenu dynamique avec [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), `[Inject]` utilisez l’attribut:

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

Dans l’exemple d’application côté client qui accompagne cette rubrique, deux fonctions JavaScript sont disponibles pour l’application côté client qui interagit avec le DOM pour recevoir l’entrée d’utilisateur et afficher un message d’accueil:

* `showPrompt`&ndash; Génère une invite pour accepter l’entrée d’utilisateur (nom de l’utilisateur) et retourne le nom à l’appelant.
* `displayWelcome`Assigne un message de bienvenue de l’appelant à un objet DOM avec `id` un `welcome`de. &ndash;

*wwwroot/exampleJsInterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

Placez la `<script>` balise qui référence le fichier JavaScript dans le fichier *wwwroot/index.html* (The éblouissant Client-Side) ou le fichier *pages/_Host. cshtml* (éblouissant côté serveur).

*wwwroot/index.html* (Éblouissant côté client):

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

*Pages/_Host. cshtml* (Le côté serveur de éblouissant):

[!code-cshtml[](javascript-interop/samples_snapshot/_Host.cshtml?highlight=29)]

Ne placez `<script>` pas de balise dans un fichier de `<script>` composant parce que la balise ne peut pas être mise à jour dynamiquement.

Les méthodes .NET interagissent avec les fonctions JavaScript dans le fichier *exampleJsInterop. js* en appelant `IJSRuntime.InvokeAsync<T>`.

L' `IJSRuntime` abstraction est asynchrone pour permettre les scénarios côté serveur. Si l’application s’exécute côté client et que vous souhaitez appeler une fonction JavaScript de manière synchrone, vous `IJSInProcessRuntime` devez effectuer `Invoke<T>` un cast aval vers et appeler à la place. Nous recommandons que la plupart des bibliothèques d’interopérabilité JavaScript utilisent les API Async pour s’assurer que les bibliothèques sont disponibles dans tous les scénarios, côté client ou côté serveur.

L’exemple d’application comprend un composant pour illustrer l’interopérabilité JavaScript. Le composant:

* Reçoit une entrée d’utilisateur via une invite JavaScript.
* Retourne le texte du composant pour traitement.
* Appelle une deuxième fonction JavaScript qui interagit avec le DOM pour afficher un message d’accueil.

*Pages/JSInterop. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. Lorsque `TriggerJsPrompt` est exécuté en sélectionnant le bouton d' **invite de commandes JavaScript** du composant `showPrompt` , la fonction JavaScript fournie dans le fichier *wwwroot/exampleJsInterop. js* est appelée.
1. La `showPrompt` fonction accepte l’entrée d’utilisateur (nom de l’utilisateur), qui est codée au format HTML et renvoyée au composant. Le composant stocke le nom de l’utilisateur dans une variable `name`locale,.
1. La chaîne stockée `name` dans est incorporée dans un message de bienvenue, qui est transmis à une `displayWelcome`fonction JavaScript,, qui affiche le message de bienvenue dans une balise de titre.

## <a name="call-a-void-javascript-function"></a>Appeler une fonction JavaScript void

Les fonctions JavaScript qui retournent [void (0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) ou [non défini](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) sont `IJSRuntime.InvokeAsync<object>`appelées avec `null`, qui retourne.

## <a name="detect-when-a-blazor-app-is-prerendering"></a>Détecter quand une application éblouissant est prérendu
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a>Capturer des références à des éléments

Certains scénarios d' [interopérabilité JavaScript](xref:blazor/javascript-interop) requièrent des références aux éléments HTML. Par exemple, une bibliothèque d’interface utilisateur peut nécessiter une référence d’élément pour l’initialisation, ou vous devrez peut-être appeler des API de type commande `focus` sur `play`un élément, tel que ou.

Capturer des références aux éléments HTML dans un composant à l’aide de l’approche suivante:

* Ajoutez un `@ref` attribut à l’élément HTML.
* Définissez un champ de type `ElementReference` dont le nom correspond à la valeur `@ref` de l’attribut.

L’exemple suivant illustre la capture d’une référence `username` à l' `<input>` élément:

```cshtml
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!NOTE]
> N’utilisez **pas** de références d’élément capturées comme méthode de remplissage ou de manipulation du DOM quand éblouissant interagit avec les éléments référencés. Cela peut interférer avec le modèle de rendu déclaratif.

En ce qui concerne le code .net, un `ElementReference` est un handle opaque. La *seule* chose que vous pouvez faire `ElementReference` avec est de la passer au code JavaScript via l’interopérabilité JavaScript. Dans ce cas, le code JavaScript reçoit une `HTMLElement` instance, qu’il peut utiliser avec les API DOM normales.

Par exemple, le code suivant définit une méthode d’extension .NET qui permet de définir le focus sur un élément:

*exampleJsInterop. js*:

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

Utilisez `IJSRuntime.InvokeAsync<T>` et appelez `exampleJsFunctions.focusElement` avec un `ElementReference` pour concentrer un élément:

```cshtml
@inject IJSRuntime JSRuntime

<input @ref="username" />
<button @onclick="SetFocus">Set focus on username</button>

@code {
    private ElementReference username;

    public async void SetFocus()
    {
        await JSRuntime.InvokeAsync<object>(
                "exampleJsFunctions.focusElement", username);
    }
}
```

Pour utiliser une méthode d’extension pour le focus d’un élément, créez une méthode d’extension `IJSRuntime` statique qui reçoit l’instance:

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

La méthode est appelée directement sur l’objet. L’exemple suivant suppose que la méthode statique `Focus` est disponible à partir de `JsInteropClasses` l’espace de noms:

```cshtml
@inject IJSRuntime JSRuntime
@using JsInteropClasses

<input @ref="username" />
<button @onclick="SetFocus">Set focus on username</button>

@code {
    private ElementReference username;

    public async Task SetFocus()
    {
        await username.Focus(JSRuntime);
    }
}
```

> [!IMPORTANT]
> La `username` variable est remplie uniquement après le rendu du composant. Si un non rempli `ElementReference` est passé à du code JavaScript, le code JavaScript reçoit la `null`valeur. Pour manipuler des références d’élément une fois que le composant a terminé le rendu (pour définir le focus initial sur `OnAfterRenderAsync` un `OnAfterRender` élément), utilisez les méthodes de cycle de vie des [composants](xref:blazor/components#lifecycle-methods)ou.

<!-- HOLD https://github.com/aspnet/AspNetCore.Docs/pull/13818
Capture a reference to an HTML element in a component by adding an `@ref` attribute to the HTML element. The following example shows capturing a reference to the `username` `<input>` element:

```cshtml
<input @ref="username" ... />
```

> [!NOTE]
> Do **not** use captured element references as a way of populating or manipulating the DOM when Blazor interacts with the elements referenced. Doing so may interfere with the declarative rendering model.

As far as .NET code is concerned, an `ElementReference` is an opaque handle. The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JavaScript interop. When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.

For example, the following code defines a .NET extension method that enables setting the focus on an element:

*exampleJsInterop.js*:

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

Use `IJSRuntime.InvokeAsync<T>` and call `exampleJsFunctions.focusElement` with an `ElementReference` to focus an element:

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,9-10)]

To use an extension method to focus an element, create a static extension method that receives the `IJSRuntime` instance:

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

The method is called directly on the object. The following example assumes that the static `Focus` method is available from the `JsInteropClasses` namespace:

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,10)]

> [!IMPORTANT]
> The `username` variable is only populated after the component is rendered. If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`. To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:blazor/components#lifecycle-methods).
-->

## <a name="invoke-net-methods-from-javascript-functions"></a>Appeler des méthodes .NET à partir de fonctions JavaScript

### <a name="static-net-method-call"></a>Appel de méthode .NET statique

Pour appeler une méthode .net statique à partir de JavaScript, `DotNet.invokeMethod` utilisez `DotNet.invokeMethodAsync` les fonctions ou. Transmettez l’identificateur de la méthode statique que vous souhaitez appeler, le nom de l’assembly contenant la fonction et les arguments éventuels. La version asynchrone est recommandée pour prendre en charge les scénarios côté serveur. Pour appeler une méthode .net à partir de JavaScript, la méthode .net doit être publique, statique et avoir `[JSInvokable]` l’attribut. Par défaut, l’identificateur de méthode est le nom de la méthode, mais vous pouvez spécifier un identificateur différent à `JSInvokableAttribute` l’aide du constructeur. L’appel de méthodes génériques ouvertes n’est pas pris en charge actuellement.

L’exemple d’application comprend C# une méthode pour retourner un tableau `int`de. L' `JSInvokable` attribut est appliqué à la méthode.

*Pages/JsInterop. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

JavaScript traité au client appelle la C# méthode .net.

*wwwroot/exampleJsInterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-14)]

Quand le bouton déclencher l’ReturnArrayAsync de la **méthode statique .net** est sélectionné, examinez la sortie de la console dans les outils de développement Web du navigateur.

La sortie de la console est la suivante:

```console
Array(4) [ 1, 2, 3, 4 ]
```

La quatrième valeur de tableau fait l’objet d’un`data.push(4);`push dans le `ReturnArrayAsync`tableau () retourné par.

### <a name="instance-method-call"></a>Appel de méthode d’instance

Vous pouvez également appeler des méthodes d’instance .NET à partir de JavaScript. Pour appeler une méthode d’instance .NET à partir de JavaScript:

* Transmettez l’instance .net à JavaScript en l’encapsulant `DotNetObjectRef` dans une instance. L’instance .NET est passée par référence à JavaScript.
* Appeler des méthodes d’instance .net sur l’instance `invokeMethod` à `invokeMethodAsync` l’aide des fonctions ou. L’instance .NET peut également être passée comme argument lors de l’appel d’autres méthodes .NET à partir de JavaScript.

> [!NOTE]
> L’exemple d’application enregistre les messages dans la console côté client. Pour les exemples suivants présentés dans l’exemple d’application, examinez la sortie de console du navigateur dans les outils de développement du navigateur.

Lorsque le bouton de **méthode d’instance .net de déclenchement HelloHelper. SayHello** est sélectionné, `ExampleJsInterop.CallHelloHelperSayHello` est appelé et passe un nom, `Blazor`, à la méthode.

*Pages/JsInterop. Razor*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

`CallHelloHelperSayHello`appelle la fonction `sayHello` JavaScript avec une nouvelle instance de `HelloHelper`.

*JsInteropClasses/ExampleJsInterop. cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

*wwwroot/exampleJsInterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-18)]

Le nom est passé au `HelloHelper`constructeur de, qui définit la `HelloHelper.Name` propriété. Lorsque la fonction `sayHello` JavaScript est exécutée `HelloHelper.SayHello` , retourne `Hello, {Name}!` le message, qui est écrit dans la console par la fonction JavaScript.

*JsInteropClasses/HelloHelper.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Sortie de la console dans les outils de développement Web du navigateur:

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a>Partager du code d’interopérabilité dans une bibliothèque de classes

Le code JavaScript Interop peut être inclus dans une bibliothèque de classes, ce qui vous permet de partager le code dans un package NuGet.

La bibliothèque de classes gère l’incorporation des ressources JavaScript dans l’assembly généré. Les fichiers JavaScript sont placés dans le dossier *wwwroot* . Les outils s’occupent de l’incorporation des ressources lors de la génération de la bibliothèque.

Le package NuGet créé est référencé dans le fichier projet de l’application de la même façon que n’importe quel package NuGet. Une fois le package restauré, le code d’application peut appeler JavaScript comme s’il C#avait été.

Pour plus d'informations, consultez <xref:blazor/class-libraries>.
