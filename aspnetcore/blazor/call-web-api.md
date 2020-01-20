---
title: Appeler une API Web à partir de ASP.NET Core Blazor
author: guardrex
description: Découvrez comment appeler une API Web à partir d’une application Blazor à l’aide des applications auxiliaires JSON, y compris la création de demandes de partage des ressources Cross-Origin (CORS).
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/call-web-api
ms.openlocfilehash: 66605f38a6fcaedebc92b0946dca1e5f28b593c6
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160065"
---
# <a name="call-a-web-api-from-aspnet-core-opno-locblazor"></a><span data-ttu-id="10ac2-103">Appeler une API Web à partir de ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="10ac2-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="10ac2-104">Par [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27)et [Juan de la Cruz](https://github.com/juandelacruz23)</span><span class="sxs-lookup"><span data-stu-id="10ac2-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Juan De la Cruz](https://github.com/juandelacruz23)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="10ac2-105">[Blazor les applications Webassembly](xref:blazor/hosting-models#blazor-webassembly) appellent des API Web à l’aide d’un service `HttpClient` préconfiguré.</span><span class="sxs-lookup"><span data-stu-id="10ac2-105">[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="10ac2-106">Composez des requêtes, qui peuvent inclure des options de l' [API d’extraction](https://developer.mozilla.org/docs/Web/API/Fetch_API) JavaScript, à l’aide de Blazor des applications d’assistance JSON ou avec <xref:System.Net.Http.HttpRequestMessage>.</span><span class="sxs-lookup"><span data-stu-id="10ac2-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span>

<span data-ttu-id="10ac2-107">les applications [Blazor Server](xref:blazor/hosting-models#blazor-server) appellent des API Web à l’aide d’instances <xref:System.Net.Http.HttpClient> généralement créées à l’aide de <xref:System.Net.Http.IHttpClientFactory>.</span><span class="sxs-lookup"><span data-stu-id="10ac2-107">[Blazor Server](xref:blazor/hosting-models#blazor-server) apps call web APIs using <xref:System.Net.Http.HttpClient> instances typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="10ac2-108">Pour plus d'informations, consultez <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="10ac2-108">For more information, see <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="10ac2-109">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample)) &ndash; sélectionnez l’application *BlazorWebAssemblySample* .</span><span class="sxs-lookup"><span data-stu-id="10ac2-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample)) &ndash; Select the *BlazorWebAssemblySample* app.</span></span>

<span data-ttu-id="10ac2-110">Consultez les composants suivants dans l’exemple d’application *BlazorWebAssemblySample* :</span><span class="sxs-lookup"><span data-stu-id="10ac2-110">See the following components in the *BlazorWebAssemblySample* sample app:</span></span>

* <span data-ttu-id="10ac2-111">Appeler l’API Web (*pages/CallWebAPI. Razor*)</span><span class="sxs-lookup"><span data-stu-id="10ac2-111">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="10ac2-112">Testeur de requêtes HTTP (*composants/HTTPRequestTester. Razor*)</span><span class="sxs-lookup"><span data-stu-id="10ac2-112">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="packages"></a><span data-ttu-id="10ac2-113">Packages</span><span class="sxs-lookup"><span data-stu-id="10ac2-113">Packages</span></span>

<span data-ttu-id="10ac2-114">Référencez le [Microsoft. AspNetCore.Blazorexpérimental. ](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/)Package NuGet httpclient dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="10ac2-114">Reference the *experimental* [Microsoft.AspNetCore.Blazor.HttpClient](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.HttpClient/) NuGet package in the project file.</span></span> <span data-ttu-id="10ac2-115">`Microsoft.AspNetCore.Blazor.HttpClient` est basé sur `HttpClient` et [System. Text. JSON](https://www.nuget.org/packages/System.Text.Json/).</span><span class="sxs-lookup"><span data-stu-id="10ac2-115">`Microsoft.AspNetCore.Blazor.HttpClient` is based on `HttpClient` and [System.Text.Json](https://www.nuget.org/packages/System.Text.Json/).</span></span>

<span data-ttu-id="10ac2-116">Pour utiliser une API stable, utilisez le package [Microsoft. Aspnet. WebApi. client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) , qui utilise [Newtonsoft. JSON](https://www.nuget.org/packages/Newtonsoft.Json/)/[JSON.net](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="10ac2-116">To use a stable API, use the [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) package, which uses [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/)/[Json.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span> <span data-ttu-id="10ac2-117">L’utilisation de l’API stable dans `Microsoft.AspNet.WebApi.Client` ne fournit pas les applications auxiliaires JSON décrites dans cette rubrique, qui sont propres au package de `Microsoft.AspNetCore.Blazor.HttpClient` expérimental.</span><span class="sxs-lookup"><span data-stu-id="10ac2-117">Using the stable API in `Microsoft.AspNet.WebApi.Client` doesn't provide the JSON helpers described in this topic, which are unique to the experimental `Microsoft.AspNetCore.Blazor.HttpClient` package.</span></span>

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="10ac2-118">Applications auxiliaires HttpClient et JSON</span><span class="sxs-lookup"><span data-stu-id="10ac2-118">HttpClient and JSON helpers</span></span>

<span data-ttu-id="10ac2-119">Dans une application Blazor webassembly, [httpclient](xref:fundamentals/http-requests) est disponible en tant que service préconfiguré pour effectuer des demandes auprès du serveur d’origine.</span><span class="sxs-lookup"><span data-stu-id="10ac2-119">In a Blazor WebAssembly app, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span>

<span data-ttu-id="10ac2-120">Une application Blazor Server n’inclut pas de service `HttpClient` par défaut.</span><span class="sxs-lookup"><span data-stu-id="10ac2-120">A Blazor Server app doesn't include an `HttpClient` service by default.</span></span> <span data-ttu-id="10ac2-121">Fournissez une `HttpClient` à l’application à l’aide de l' [infrastructure de fabrique httpclient](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="10ac2-121">Provide an `HttpClient` to the app using the [HttpClient factory infrastructure](xref:fundamentals/http-requests).</span></span>

<span data-ttu-id="10ac2-122">les `HttpClient` et les applications auxiliaires JSON servent également à appeler des points de terminaison d’API Web tiers.</span><span class="sxs-lookup"><span data-stu-id="10ac2-122">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="10ac2-123">`HttpClient` est implémenté à l’aide de l' [API FETCH](https://developer.mozilla.org/docs/Web/API/Fetch_API) du navigateur et est soumis à ses limitations, y compris l’application de la même stratégie d’origine.</span><span class="sxs-lookup"><span data-stu-id="10ac2-123">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="10ac2-124">L’adresse de base du client est définie sur l’adresse du serveur d’origine.</span><span class="sxs-lookup"><span data-stu-id="10ac2-124">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="10ac2-125">Injectez une instance de `HttpClient` à l’aide de la directive `@inject` :</span><span class="sxs-lookup"><span data-stu-id="10ac2-125">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```razor
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="10ac2-126">Dans les exemples suivants, une API Web todo traite les opérations de création, lecture, mise à jour et suppression (CRUD).</span><span class="sxs-lookup"><span data-stu-id="10ac2-126">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="10ac2-127">Les exemples sont basés sur une classe `TodoItem` qui stocke les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="10ac2-127">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="10ac2-128">ID (`Id`, `long`) &ndash; ID unique de l’élément.</span><span class="sxs-lookup"><span data-stu-id="10ac2-128">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="10ac2-129">Nom (`Name`, `string`) &ndash; nom de l’élément.</span><span class="sxs-lookup"><span data-stu-id="10ac2-129">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="10ac2-130">État (`IsComplete`, `bool`) &ndash; indiquant si l’élément TODO est terminé.</span><span class="sxs-lookup"><span data-stu-id="10ac2-130">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="10ac2-131">Les méthodes d’assistance JSON envoient des demandes à un URI (une API Web dans les exemples suivants) et traitent la réponse :</span><span class="sxs-lookup"><span data-stu-id="10ac2-131">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="10ac2-132">`GetJsonAsync` &ndash; envoie une requête HTTP obtenir et analyse le corps de la réponse JSON pour créer un objet.</span><span class="sxs-lookup"><span data-stu-id="10ac2-132">`GetJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="10ac2-133">Dans le code suivant, les `_todoItems` sont affichés par le composant.</span><span class="sxs-lookup"><span data-stu-id="10ac2-133">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="10ac2-134">La méthode `GetTodoItems` est déclenchée lorsque le rendu du composant est terminé ([OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)).</span><span class="sxs-lookup"><span data-stu-id="10ac2-134">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitializedAsync](xref:blazor/lifecycle#component-initialization-methods)).</span></span> <span data-ttu-id="10ac2-135">Pour obtenir un exemple complet, consultez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="10ac2-135">See the sample app for a complete example.</span></span>

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* <span data-ttu-id="10ac2-136">`PostJsonAsync` &ndash; envoie une requête HTTP POSTÉRIEURe, y compris du contenu encodé JSON, et analyse le corps de la réponse JSON pour créer un objet.</span><span class="sxs-lookup"><span data-stu-id="10ac2-136">`PostJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="10ac2-137">Dans le code suivant, `_newItemName` est fourni par un élément lié du composant.</span><span class="sxs-lookup"><span data-stu-id="10ac2-137">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="10ac2-138">La méthode `AddItem` est déclenchée en sélectionnant un élément `<button>`.</span><span class="sxs-lookup"><span data-stu-id="10ac2-138">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="10ac2-139">Pour obtenir un exemple complet, consultez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="10ac2-139">See the sample app for a complete example.</span></span>

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  <input @bind="_newItemName" placeholder="New Todo Item" />
  <button @onclick="@AddItem">Add</button>

  @code {
      private string _newItemName;

      private async Task AddItem()
      {
          var addItem = new TodoItem { Name = _newItemName, IsComplete = false };
          await Http.PostJsonAsync("api/TodoItems", addItem);
      }
  }
  ```

* <span data-ttu-id="10ac2-140">`PutJsonAsync` &ndash; envoie une requête HTTP PUT, y compris du contenu encodé JSON.</span><span class="sxs-lookup"><span data-stu-id="10ac2-140">`PutJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="10ac2-141">Dans le code suivant, `_editItem` valeurs pour `Name` et `IsCompleted` sont fournies par les éléments dépendants du composant.</span><span class="sxs-lookup"><span data-stu-id="10ac2-141">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="10ac2-142">Le `Id` de l’élément est défini lorsque l’élément est sélectionné dans une autre partie de l’interface utilisateur et `EditItem` est appelé.</span><span class="sxs-lookup"><span data-stu-id="10ac2-142">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="10ac2-143">La méthode `SaveItem` est déclenchée en sélectionnant l’élément Save `<button>`.</span><span class="sxs-lookup"><span data-stu-id="10ac2-143">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="10ac2-144">Pour obtenir un exemple complet, consultez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="10ac2-144">See the sample app for a complete example.</span></span>

  ```razor
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
          await Http.PutJsonAsync($"api/TodoItems/{_editItem.Id}, _editItem);
  }
  ```

<span data-ttu-id="10ac2-145"><xref:System.Net.Http> comprend des méthodes d’extension supplémentaires pour envoyer des requêtes HTTP et recevoir des réponses HTTP.</span><span class="sxs-lookup"><span data-stu-id="10ac2-145"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="10ac2-146">[Httpclient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) est utilisé pour envoyer une requête HTTP DELETE à une API Web.</span><span class="sxs-lookup"><span data-stu-id="10ac2-146">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="10ac2-147">Dans le code suivant, l’élément delete `<button>` appelle la méthode `DeleteItem`.</span><span class="sxs-lookup"><span data-stu-id="10ac2-147">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="10ac2-148">L’élément `<input>` lié fournit les `id` de l’élément à supprimer.</span><span class="sxs-lookup"><span data-stu-id="10ac2-148">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="10ac2-149">Pour obtenir un exemple complet, consultez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="10ac2-149">See the sample app for a complete example.</span></span>

```razor
@using System.Net.Http
@inject HttpClient Http

<input @bind="_id" />
<button @onclick="@DeleteItem">Delete</button>

@code {
    private long _id;

    private async Task DeleteItem() =>
        await Http.DeleteAsync($"api/TodoItems/{_id}");
}
```

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="10ac2-150">Partage des ressources cross-origin (CORS)</span><span class="sxs-lookup"><span data-stu-id="10ac2-150">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="10ac2-151">La sécurité du navigateur empêche une page Web d’effectuer des demandes vers un autre domaine que celui qui a servi la page Web.</span><span class="sxs-lookup"><span data-stu-id="10ac2-151">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="10ac2-152">Cette restriction est appelée *stratégie de même origine*.</span><span class="sxs-lookup"><span data-stu-id="10ac2-152">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="10ac2-153">La stratégie de même origine empêche un site malveillant de lire des données sensibles à partir d’un autre site.</span><span class="sxs-lookup"><span data-stu-id="10ac2-153">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="10ac2-154">Pour effectuer des demandes à partir du navigateur vers un point de terminaison avec une origine différente, le *point de terminaison* doit activer le [partage des ressources Cross-Origin (cors)](https://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="10ac2-154">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="10ac2-155">L' [exemple d’applicationBlazor Webassembly (BlazorWebAssemblySample)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) illustre l’utilisation de cors dans le composant appeler l’API Web (*pages/CallWebAPI. Razor*).</span><span class="sxs-lookup"><span data-stu-id="10ac2-155">The [Blazor WebAssembly sample app (BlazorWebAssemblySample)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="10ac2-156">Pour permettre à d’autres sites d’effectuer des demandes de partage de ressources Cross-Origin (CORS) à votre application, consultez <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="10ac2-156">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="10ac2-157">HttpClient et HttpRequestMessage avec les options de demande d’API Fetch</span><span class="sxs-lookup"><span data-stu-id="10ac2-157">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="10ac2-158">Quand vous exécutez sur webassembly dans une application Blazor webassembly, utilisez [httpclient](xref:fundamentals/http-requests) et <xref:System.Net.Http.HttpRequestMessage> pour personnaliser les demandes.</span><span class="sxs-lookup"><span data-stu-id="10ac2-158">When running on WebAssembly in a Blazor WebAssembly app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="10ac2-159">Par exemple, vous pouvez spécifier l’URI de demande, la méthode HTTP et tous les en-têtes de demande souhaités.</span><span class="sxs-lookup"><span data-stu-id="10ac2-159">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

```razor
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
            RequestUri = new Uri("https://localhost:10000/api/TodoItems"),
            Content = 
                new StringContent(
                    @"{""name"":""A New Todo Item"",""isComplete"":false}")
        };

        requestMessage.Content.Headers.ContentType = 
            new System.Net.Http.Headers.MediaTypeHeaderValue(
                "application/json");

        requestMessage.Content.Headers.TryAddWithoutValidation(
            "x-custom-header", "value");

        var response = await Http.SendAsync(requestMessage);
        var responseStatusCode = response.StatusCode;
        var responseBody = await response.Content.ReadAsStringAsync();
    }
}
```

<span data-ttu-id="10ac2-160">Pour plus d’informations sur les options de l’API FETCH, consultez [MDN Web docs : WindowOrWorkerGlobalScope. Fetch () :P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span><span class="sxs-lookup"><span data-stu-id="10ac2-160">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="10ac2-161">Lors de l’envoi d’informations d’identification (cookies/en-têtes d’autorisation) sur les demandes CORS, l’en-tête `Authorization` doit être autorisé par la stratégie CORS.</span><span class="sxs-lookup"><span data-stu-id="10ac2-161">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="10ac2-162">La stratégie suivante comprend la configuration pour :</span><span class="sxs-lookup"><span data-stu-id="10ac2-162">The following policy includes configuration for:</span></span>

* <span data-ttu-id="10ac2-163">Origines des demandes (`http://localhost:5000`, `https://localhost:5001`).</span><span class="sxs-lookup"><span data-stu-id="10ac2-163">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="10ac2-164">Toute méthode (verbe).</span><span class="sxs-lookup"><span data-stu-id="10ac2-164">Any method (verb).</span></span>
* <span data-ttu-id="10ac2-165">`Content-Type` et `Authorization` en-têtes.</span><span class="sxs-lookup"><span data-stu-id="10ac2-165">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="10ac2-166">Pour autoriser un en-tête personnalisé (par exemple, `x-custom-header`), répertoriez l’en-tête lors de l’appel de <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span><span class="sxs-lookup"><span data-stu-id="10ac2-166">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="10ac2-167">Informations d’identification définies par le code JavaScript côté client (`credentials` propriété définie sur `include`).</span><span class="sxs-lookup"><span data-stu-id="10ac2-167">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="10ac2-168">Pour plus d’informations, consultez <xref:security/cors> et le composant testeur de requêtes HTTP de l’exemple d’application (*composants/HTTPRequestTester. Razor*).</span><span class="sxs-lookup"><span data-stu-id="10ac2-168">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="10ac2-169">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="10ac2-169">Additional resources</span></span>

* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [<span data-ttu-id="10ac2-170">Configuration du point de terminaison HTTPs Kestrel</span><span class="sxs-lookup"><span data-stu-id="10ac2-170">Kestrel HTTPS endpoint configuration</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [<span data-ttu-id="10ac2-171">Cross Origin Resource Sharing (CORS) au W3C</span><span class="sxs-lookup"><span data-stu-id="10ac2-171">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
