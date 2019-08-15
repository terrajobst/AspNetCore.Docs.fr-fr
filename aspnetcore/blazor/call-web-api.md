---
title: Appeler une API Web à partir de ASP.NET Core éblouissant
author: guardrex
description: Découvrez comment appeler une API Web à partir d’une application éblouissant à l’aide des applications auxiliaires JSON, y compris la création de demandes de partage des ressources Cross-Origin (CORS).
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/call-web-api
ms.openlocfilehash: 60ebd01bc07da22cd1dcd0b16297ee54c97867fc
ms.sourcegitcommit: f5f0ff65d4e2a961939762fb00e654491a2c772a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030383"
---
# <a name="call-a-web-api-from-aspnet-core-blazor"></a>Appeler une API Web à partir de ASP.NET Core éblouissant

Par [Luke Latham](https://github.com/guardrex) et [Daniel Roth](https://github.com/danroth27)

Les applications côté client éblouissantes appellent des API Web à l’aide d' `HttpClient` un service préconfiguré. Composez des demandes, qui peuvent inclure des options de l' [API d’extraction](https://developer.mozilla.org/docs/Web/API/Fetch_API) JavaScript, à l' <xref:System.Net.Http.HttpRequestMessage>aide des applications auxiliaires de l’API JSON ou de.

Les applications côté serveur éblouissantes appellent des API Web <xref:System.Net.Http.HttpClient> à l’aide d' <xref:System.Net.Http.IHttpClientFactory>instances généralement créées à l’aide de. Pour plus d'informations, consultez <xref:fundamentals/http-requests>.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

Pour obtenir des exemples côté client éblouissants, consultez les composants suivants dans l’exemple d’application:

* Appeler l’API Web (*pages/CallWebAPI. Razor*)
* Testeur de requêtes HTTP (*composants/HTTPRequestTester. Razor*)

## <a name="httpclient-and-json-helpers"></a>Applications auxiliaires HttpClient et JSON

Dans les applications côté client éblouissantes, [httpclient](xref:fundamentals/http-requests) est disponible en tant que service préconfiguré pour effectuer des requêtes sur le serveur d’origine. Pour utiliser `HttpClient` les applications auxiliaires JSON, ajoutez une référence `Microsoft.AspNetCore.Blazor.HttpClient`de package à. `HttpClient`et les auxiliaires JSON sont également utilisés pour appeler des points de terminaison d’API Web tiers. `HttpClient`est implémenté à l’aide de l' [API FETCH](https://developer.mozilla.org/docs/Web/API/Fetch_API) du navigateur et est soumis à ses limitations, y compris l’application de la même stratégie d’origine.

L’adresse de base du client est définie sur l’adresse du serveur d’origine. Injecter `HttpClient` une instance à `@inject` l’aide de la directive:

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

Dans les exemples suivants, une API Web todo traite les opérations de création, lecture, mise à jour et suppression (CRUD). Les exemples sont basés sur une `TodoItem` classe qui stocke les éléments suivants:

* ID (`Id`, `long`) &ndash; ID unique de l’élément.
* Nom (`Name`, `string`) &ndash; nom de l’élément.
* État (`IsComplete`, `bool`) &ndash; indiquant si l’élément TODO est terminé.

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

Les méthodes d’assistance JSON envoient des demandes à un URI (une API Web dans les exemples suivants) et traitent la réponse:

* `GetJsonAsync`&ndash; Envoie une requête http obtenir et analyse le corps de la réponse JSON pour créer un objet.

  Dans le code suivant, les `_todoItems` sont affichés par le composant. La `GetTodoItems` méthode est déclenchée lorsque le rendu du composant est terminé ([OnInitializedAsync](xref:blazor/components#lifecycle-methods)). Pour obtenir un exemple complet, consultez l’exemple d’application.

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/todo");
  }
  ```

* `PostJsonAsync`&ndash; Envoie une requête http postérieure, y compris du contenu encodé JSON, et analyse le corps de la réponse JSON pour créer un objet.

  Dans le code suivant, `_newItemName` est fourni par un élément lié du composant. La `AddItem` méthode est déclenchée par la sélection `<button>` d’un élément. Pour obtenir un exemple complet, consultez l’exemple d’application.

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  <input @bind="_newItemName" placeholder="New Todo Item" />
  <button @onclick="@AddItem">Add</button>

  @code {
      private string _newItemName;

      private async Task AddItem()
      {
          var addItem = new TodoItem { Name = _newItemName, IsComplete = false };
          await Http.PostJsonAsync("api/todo", addItem);
      }
  }
  ```

* `PutJsonAsync`&ndash; Envoie une requête HTTP PUT, y compris du contenu encodé JSON.

  Dans le code suivant, `_editItem` les valeurs `Name` pour `IsCompleted` et sont fournies par les éléments dépendants du composant. L’élément `Id` est défini lorsque l’élément est sélectionné dans une autre partie de l’interface utilisateur `EditItem` et est appelé. La `SaveItem` méthode est déclenchée par la sélection de `<button>` l’élément Save. Pour obtenir un exemple complet, consultez l’exemple d’application.

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  <input type="checkbox" @bind="_editItem.IsComplete" />
  <input @bind="_editItem.Name" />
  <button @onclick="@SaveItem">Save</button>

  @code {
      private TodoItem _editItem = new TodoItem();

      private void EditItem(long id)
      {
          var editItem = _todoItems.Single(i => i.Id == id);
          _editItem = new TodoItem { Id = editItem.Id, Name = editItem.Name, 
              IsComplete = editItem.IsComplete };
      }

      private async Task SaveItem() =>
          await Http.PutJsonAsync($"api/todo/{_editItem.Id}, _editItem);
  }
  ```

<xref:System.Net.Http>contient des méthodes d’extension supplémentaires pour envoyer des requêtes HTTP et recevoir des réponses HTTP. [Httpclient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) est utilisé pour envoyer une requête HTTP DELETE à une API Web.

Dans le code suivant, l’élément `<button>` delete appelle la `DeleteItem` méthode. L’élément `<input>` lié fournit le `id` de l’élément à supprimer. Pour obtenir un exemple complet, consultez l’exemple d’application.

```cshtml
@using System.Net.Http
@inject HttpClient Http

<input @bind="_id" />
<button @onclick="@DeleteItem">Delete</button>

@code {
    private long _id;

    private async Task DeleteItem() =>
        await Http.DeleteAsync($"api/todo/{_id}");
}
```

## <a name="cross-origin-resource-sharing-cors"></a>Partage des ressources Cross-Origin (CORS)

La sécurité du navigateur empêche une page Web d’effectuer des demandes vers un autre domaine que celui qui a servi la page Web. Cette restriction est appelée *stratégie de même origine*. La stratégie de même origine empêche un site malveillant de lire des données sensibles à partir d’un autre site. Pour effectuer des demandes à partir du navigateur vers un point de terminaison avec une origine différente, le *point de terminaison* doit activer le [partage des ressources Cross-Origin (cors)](https://www.w3.org/TR/cors/).

L’exemple d’application illustre l’utilisation de CORS dans le composant appeler l’API Web (*pages/CallWebAPI. Razor*).

Pour permettre à d’autres sites d’effectuer des demandes de partage de ressources Cross-Origin (CORS) <xref:security/cors>à votre application, consultez.

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a>HttpClient et HttpRequestMessage avec les options de demande d’API Fetch

Quand vous exécutez sur webassembly dans une application cliente éblouissant, utilisez [httpclient](xref:fundamentals/http-requests) et <xref:System.Net.Http.HttpRequestMessage> pour personnaliser les demandes. Par exemple, vous pouvez spécifier l’URI de demande, la méthode HTTP et tous les en-têtes de demande souhaités.

Fournissez des options de demande à l' [API](https://developer.mozilla.org/docs/Web/API/Fetch_API) d’extraction `WebAssemblyHttpMessageHandler.FetchArgs` JavaScript sous-jacente à l’aide de la propriété sur la demande. Comme indiqué dans l’exemple suivant, la `credentials` propriété est définie sur l’une des valeurs suivantes:

* `FetchCredentialsOption.Include`(«Include») &ndash; Conseille au navigateur d’envoyer des informations d’identification (telles que des en-têtes de cookies ou d’authentification http) même pour les demandes Cross-Origin. Autorisé uniquement lorsque la stratégie CORS est configurée pour autoriser les informations d’identification.
* `FetchCredentialsOption.Omit`(«omettre») &ndash; Conseille au navigateur de ne jamais envoyer d’informations d’identification (par exemple, cookies ou en-têtes http auth).
* `FetchCredentialsOption.SameOrigin`(«même-Origin») &ndash; Conseille au navigateur d’envoyer des informations d’identification (par exemple, cookies ou en-têtes http auth) uniquement si l’URL cible est sur la même origine que l’application appelante.

```cshtml
@using System.Net.Http
@using System.Net.Http.Headers
@inject HttpClient Http

@code {
    private async Task PostRequest()
    {
        Http.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", "{OAUTH TOKEN}");

        var requestMessage = new HttpRequestMessage()
        {
            Method = new HttpMethod("POST"),
            RequestUri = new Uri("https://localhost:10000/api/todo"),
            Content = 
                new StringContent(
                    @"{""name"":""A New Todo Item"",""isComplete"":false}")
        };

        requestMessage.Content.Headers.ContentType = 
            new System.Net.Http.Headers.MediaTypeHeaderValue(
                "application/json");

        requestMessage.Content.Headers.TryAddWithoutValidation(
            "x-custom-header", "value");
        
        requestMessage.Properties[WebAssemblyHttpMessageHandler.FetchArgs] = new
        { 
            credentials = FetchCredentialsOption.Include
        };

        var response = await Http.SendAsync(requestMessage);
        var responseStatusCode = response.StatusCode;
        var responseBody = await response.Content.ReadAsStringAsync();
    }
}
```

Pour plus d’informations sur les options de l' [API FETCH, consultez MDN Web docs: WindowOrWorkerGlobalScope. Fetch ():P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).

Lors de l’envoi d’informations d’identification (cookies/en-têtes d’autorisation `Authorization` ) sur les demandes cors, l’en-tête doit être autorisé par la stratégie cors.

La stratégie suivante comprend la configuration pour:

* Origines des demandes`http://localhost:5000`( `https://localhost:5001`,).
* Toute méthode (verbe).
* `Content-Type`et `Authorization` en-têtes. Pour autoriser un en-tête personnalisé (par `x-custom-header`exemple,), répertoriez l' <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>en-tête lors de l’appel de.
* Informations d’identification définies par le code JavaScript côté client`credentials` (la propriété `include`a la valeur).

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

Pour plus d’informations, <xref:security/cors> consultez et le composant testeur de requêtes http de l’exemple d’application (*composants/HTTPRequestTester. Razor*).

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:fundamentals/http-requests>
* [Cross Origin Resource Sharing (CORS) au W3C](https://www.w3.org/TR/cors/)
