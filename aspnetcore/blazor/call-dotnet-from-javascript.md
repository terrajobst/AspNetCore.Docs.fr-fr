---
title: Appeler des méthodes .NET à partir de fonctions JavaScript dans ASP.NET Core Blazor
author: guardrex
description: Découvrez comment appeler des méthodes .NET à partir de fonctions JavaScript dans des applications Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/24/2020
no-loc:
- Blazor
- SignalR
uid: blazor/call-dotnet-from-javascript
ms.openlocfilehash: dbf44fe7923998c65119e42d97c304890fa95523
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218789"
---
# <a name="call-net-methods-from-javascript-functions-in-aspnet-core-opno-locblazor"></a>Appeler des méthodes .NET à partir de fonctions JavaScript dans ASP.NET Core Blazor

Par [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), [Shashikant Rudrawadi](http://wisne.co)et [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Une application Blazor peut appeler des fonctions JavaScript à partir de méthodes .NET et de méthodes .NET à partir de fonctions JavaScript. Ces scénarios portent le nom de *l’interopérabilité de JavaScript* (*js Interop*).

Cet article traite de l’appel de méthodes .NET à partir de JavaScript. Pour plus d’informations sur l’appel de fonctions JavaScript à partir de .NET, consultez <xref:blazor/call-javascript-from-dotnet>.

[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="static-net-method-call"></a>Appel de méthode .NET statique

Pour appeler une méthode .NET statique à partir de JavaScript, utilisez les fonctions `DotNet.invokeMethod` ou `DotNet.invokeMethodAsync`. Transmettez l’identificateur de la méthode statique que vous souhaitez appeler, le nom de l’assembly contenant la fonction et les arguments éventuels. La version asynchrone est recommandée pour prendre en charge les scénarios de serveur Blazor. La méthode .NET doit être publique, statique et avoir l’attribut `[JSInvokable]`. L’appel de méthodes génériques ouvertes n’est pas pris en charge actuellement.

L’exemple d’application comprend C# une méthode pour retourner un tableau de `int`. L’attribut `JSInvokable` est appliqué à la méthode.

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

## <a name="instance-method-call"></a>Appel de méthode d’instance

Vous pouvez également appeler des méthodes d’instance .NET à partir de JavaScript. Pour appeler une méthode d’instance .NET à partir de JavaScript :

* Passer l’instance .NET par référence à JavaScript :
  * Effectuez un appel statique à `DotNetObjectReference.Create`.
  * Encapsulez l’instance dans une instance de `DotNetObjectReference` et appelez `Create` sur l’instance de `DotNetObjectReference`. Supprimez les objets `DotNetObjectReference` (un exemple apparaît plus loin dans cette section).
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

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=11-18)]

*wwwroot/exampleJsInterop. js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

Le nom est passé au constructeur de `HelloHelper`, qui définit la propriété `HelloHelper.Name`. Lorsque la fonction JavaScript `sayHello` est exécutée, `HelloHelper.SayHello` retourne le message `Hello, {Name}!`, qui est écrit dans la console par la fonction JavaScript.

*JsInteropClasses/HelloHelper. cs*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Sortie de la console dans les outils de développement Web du navigateur :

```console
Hello, Blazor!
```

Pour éviter une fuite de mémoire et autoriser garbage collection sur un composant qui crée un `DotNetObjectReference`, adoptez l’une des approches suivantes :

* Supprimez l’objet dans la classe qui a créé l’instance `DotNetObjectReference` :

  ```csharp
  public class ExampleJsInterop : IDisposable
  {
      private readonly IJSRuntime _jsRuntime;
      private DotNetObjectReference<HelloHelper> _objRef;

      public ExampleJsInterop(IJSRuntime jsRuntime)
      {
          _jsRuntime = jsRuntime;
      }

      public ValueTask<string> CallHelloHelperSayHello(string name)
      {
          _objRef = DotNetObjectReference.Create(new HelloHelper(name));

          return _jsRuntime.InvokeAsync<string>(
              "exampleJsFunctions.sayHello",
              _objRef);
      }

      public void Dispose()
      {
          _objRef?.Dispose();
      }
  }
  ```

  Le modèle précédent présenté dans la classe `ExampleJsInterop` peut également être implémenté dans un composant :

  ```razor
  @page "/JSInteropComponent"
  @using BlazorSample.JsInteropClasses
  @implements IDisposable
  @inject IJSRuntime JSRuntime

  <h1>JavaScript Interop</h1>

  <button type="button" class="btn btn-primary" @onclick="TriggerNetInstanceMethod">
      Trigger .NET instance method HelloHelper.SayHello
  </button>

  @code {
      private DotNetObjectReference<HelloHelper> _objRef;

      public async Task TriggerNetInstanceMethod()
      {
          _objRef = DotNetObjectReference.Create(new HelloHelper("Blazor"));

          await JSRuntime.InvokeAsync<string>(
              "exampleJsFunctions.sayHello",
              _objRef);
      }

      public void Dispose()
      {
          _objRef?.Dispose();
      }
  }
  ```

* Lorsque le composant ou la classe ne supprime pas la `DotNetObjectReference`, supprimez l’objet sur le client en appelant `.dispose()`:

  ```javascript
  window.myFunction = (dotnetHelper) => {
    dotnetHelper.invokeMethod('BlazorSample', 'MyMethod');
    dotnetHelper.dispose();
  }
  ```

## <a name="component-instance-method-call"></a>Appel de méthode d’instance de composant

Pour appeler les méthodes .NET d’un composant :

* Utilisez la fonction `invokeMethod` ou `invokeMethodAsync` pour effectuer un appel de méthode statique au composant.
* La méthode statique du composant encapsule l’appel à sa méthode d’instance en tant que `Action`appelée.

Dans le code JavaScript côté client :

```javascript
function updateMessageCallerJS() {
  DotNet.invokeMethod('BlazorSample', 'UpdateMessageCaller');
}
```

*Pages/JSInteropComponent. Razor*:

```razor
@page "/JSInteropComponent"

<p>
    Message: @_message
</p>

<p>
    <button onclick="updateMessageCallerJS()">Call JS Method</button>
</p>

@code {
    private static Action _action;
    private string _message = "Select the button.";

    protected override void OnInitialized()
    {
        _action = UpdateMessage;
    }

    private void UpdateMessage()
    {
        _message = "UpdateMessage Called!";
        StateHasChanged();
    }

    [JSInvokable]
    public static void UpdateMessageCaller()
    {
        _action.Invoke();
    }
}
```

Lorsqu’il existe plusieurs composants, chacun avec des méthodes d’instance à appeler, utilisez une classe d’assistance pour appeler les méthodes d’instance (comme `Action`s) de chaque composant.

Dans l’exemple suivant :

* Le composant `JSInterop` contient plusieurs composants `ListItem`.
* Chaque composant `ListItem` se compose d’un message et d’un bouton.
* Quand un bouton `ListItem` composant est sélectionné, la méthode `UpdateMessage` de `ListItem`modifie le texte de l’élément de liste et masque le bouton.

*MessageUpdateInvokeHelper.cs*:

```csharp
using System;
using Microsoft.JSInterop;

public class MessageUpdateInvokeHelper
{
    private Action _action;

    public MessageUpdateInvokeHelper(Action action)
    {
        _action = action;
    }

    [JSInvokable("BlazorSample")]
    public void UpdateMessageCaller()
    {
        _action.Invoke();
    }
}
```

Dans le code JavaScript côté client :

```javascript
window.updateMessageCallerJS = (dotnetHelper) => {
    dotnetHelper.invokeMethod('BlazorSample', 'UpdateMessageCaller');
    dotnetHelper.dispose();
}
```

*Shared/ListItem. Razor*:

```razor
@inject IJSRuntime JsRuntime

<li>
    @_message
    <button @onclick="InteropCall" style="display:@_display">InteropCall</button>
</li>

@code {
    private string _message = "Select one of these list item buttons.";
    private string _display = "inline-block";
    private MessageUpdateInvokeHelper _messageUpdateInvokeHelper;

    protected override void OnInitialized()
    {
        _messageUpdateInvokeHelper = new MessageUpdateInvokeHelper(UpdateMessage);
    }

    protected async Task InteropCall()
    {
        await JsRuntime.InvokeVoidAsync("updateMessageCallerJS",
            DotNetObjectReference.Create(_messageUpdateInvokeHelper));
    }

    private void UpdateMessage()
    {
        _message = "UpdateMessage Called!";
        _display = "none";
        StateHasChanged();
    }
}
```

*Pages/JSInterop. Razor*:

```razor
@page "/JSInterop"

<h1>List of components</h1>

<ul>
    <ListItem />
    <ListItem />
    <ListItem />
    <ListItem />
</ul>
```

[!INCLUDE[Share interop code in a class library](~/includes/blazor-share-interop-code.md)]

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:blazor/call-javascript-from-dotnet>
* [Exemple InteropComponent. Razor (référentiel GitHub dotnet/AspNetCore, branche de version 3,1)](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
* [Effectuer des transferts de données volumineux dans des applications Blazor Server](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)
