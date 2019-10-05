---
title: Appeler une API Web à partir de ASP.NET Core éblouissant
author: guardrex
description: Découvrez comment appeler une API Web à partir d’une application éblouissant à l’aide des applications auxiliaires JSON, y compris la création de demandes de partage des ressources Cross-Origin (CORS).
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/call-web-api
ms.openlocfilehash: ea819da57c382b724098c55f2f799d7deea363f2
ms.sourcegitcommit: 4649814d1ae32248419da4e8f8242850fd8679a5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2019
ms.locfileid: "71975642"
---
# <a name="call-a-web-api-from-aspnet-core-blazor"></a><span data-ttu-id="ce52e-103">Appeler une API Web à partir de ASP.NET Core éblouissant</span><span class="sxs-lookup"><span data-stu-id="ce52e-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="ce52e-104">Par [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27)et [Juan de la Cruz](https://github.com/juandelacruz23)</span><span class="sxs-lookup"><span data-stu-id="ce52e-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Juan De la Cruz](https://github.com/juandelacruz23)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="ce52e-105">Les applications webassembly éblouissant appellent des API Web à l’aide d’un service `HttpClient` préconfiguré.</span><span class="sxs-lookup"><span data-stu-id="ce52e-105">Blazor WebAssembly apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="ce52e-106">Composez des demandes, qui peuvent inclure des options de l' [API d’extraction](https://developer.mozilla.org/docs/Web/API/Fetch_API) JavaScript, à l’aide des applications auxiliaires de l’antivirus de l’aide de éblouissant ou de <xref:System.Net.Http.HttpRequestMessage>.</span><span class="sxs-lookup"><span data-stu-id="ce52e-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span>

<span data-ttu-id="ce52e-107">Les applications serveur éblouissantes appellent des API Web à l’aide d’instances <xref:System.Net.Http.HttpClient> généralement créées à l’aide de <xref:System.Net.Http.IHttpClientFactory>.</span><span class="sxs-lookup"><span data-stu-id="ce52e-107">Blazor Server apps call web APIs using <xref:System.Net.Http.HttpClient> instances typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="ce52e-108">Pour plus d'informations, consultez <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="ce52e-108">For more information, see <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="ce52e-109">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ce52e-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ce52e-110">Pour obtenir des exemples de webassembly éblouissant, consultez les composants suivants dans l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="ce52e-110">For Blazor WebAssembly examples, see the following components in the sample app:</span></span>

* <span data-ttu-id="ce52e-111">Appeler l’API Web (*pages/CallWebAPI. Razor*)</span><span class="sxs-lookup"><span data-stu-id="ce52e-111">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="ce52e-112">Testeur de requêtes HTTP (*composants/HTTPRequestTester. Razor*)</span><span class="sxs-lookup"><span data-stu-id="ce52e-112">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="ce52e-113">Applications auxiliaires HttpClient et JSON</span><span class="sxs-lookup"><span data-stu-id="ce52e-113">HttpClient and JSON helpers</span></span>

<span data-ttu-id="ce52e-114">Dans les applications webassembly éblouissantes, [httpclient](xref:fundamentals/http-requests) est disponible en tant que service préconfiguré pour effectuer des demandes auprès du serveur d’origine.</span><span class="sxs-lookup"><span data-stu-id="ce52e-114">In Blazor WebAssembly apps, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span> <span data-ttu-id="ce52e-115">Pour utiliser les applications d’assistance JSON `HttpClient`, ajoutez une référence de package à `Microsoft.AspNetCore.Blazor.HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="ce52e-115">To use `HttpClient` JSON helpers, add a package reference to `Microsoft.AspNetCore.Blazor.HttpClient`.</span></span> <span data-ttu-id="ce52e-116">les `HttpClient` et les applications auxiliaires JSON servent également à appeler des points de terminaison d’API Web tiers.</span><span class="sxs-lookup"><span data-stu-id="ce52e-116">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="ce52e-117">`HttpClient` est implémenté à l’aide de l' [API FETCH](https://developer.mozilla.org/docs/Web/API/Fetch_API) du navigateur et est soumis à ses limitations, y compris l’application de la même stratégie d’origine.</span><span class="sxs-lookup"><span data-stu-id="ce52e-117">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="ce52e-118">L’adresse de base du client est définie sur l’adresse du serveur d’origine.</span><span class="sxs-lookup"><span data-stu-id="ce52e-118">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="ce52e-119">Injectez une instance `HttpClient` à l’aide de la directive `@inject` :</span><span class="sxs-lookup"><span data-stu-id="ce52e-119">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="ce52e-120">Dans les exemples suivants, une API Web todo traite les opérations de création, lecture, mise à jour et suppression (CRUD).</span><span class="sxs-lookup"><span data-stu-id="ce52e-120">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="ce52e-121">Les exemples sont basés sur une classe `TodoItem` qui stocke les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="ce52e-121">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="ce52e-122">ID (`Id`, `long`) &ndash; ID unique de l’élément.</span><span class="sxs-lookup"><span data-stu-id="ce52e-122">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="ce52e-123">Nom (`Name`, `string`) &ndash; nom de l’élément.</span><span class="sxs-lookup"><span data-stu-id="ce52e-123">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="ce52e-124">État (`IsComplete`, `bool`) &ndash; indiquant si l’élément TODO est terminé.</span><span class="sxs-lookup"><span data-stu-id="ce52e-124">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="ce52e-125">Les méthodes d’assistance JSON envoient des demandes à un URI (une API Web dans les exemples suivants) et traitent la réponse :</span><span class="sxs-lookup"><span data-stu-id="ce52e-125">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="ce52e-126">`GetJsonAsync` &ndash; envoie une requête HTTP obtenir et analyse le corps de la réponse JSON pour créer un objet.</span><span class="sxs-lookup"><span data-stu-id="ce52e-126">`GetJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="ce52e-127">Dans le code suivant, les `_todoItems` sont affichés par le composant.</span><span class="sxs-lookup"><span data-stu-id="ce52e-127">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="ce52e-128">La méthode `GetTodoItems` est déclenchée à la fin du rendu du composant ([OnInitializedAsync](xref:blazor/components#lifecycle-methods)).</span><span class="sxs-lookup"><span data-stu-id="ce52e-128">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitializedAsync](xref:blazor/components#lifecycle-methods)).</span></span> <span data-ttu-id="ce52e-129">Pour obtenir un exemple complet, consultez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="ce52e-129">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitializedAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* <span data-ttu-id="ce52e-130">`PostJsonAsync` &ndash; envoie une requête HTTP POSTÉRIEURe, y compris du contenu encodé JSON, et analyse le corps de la réponse JSON pour créer un objet.</span><span class="sxs-lookup"><span data-stu-id="ce52e-130">`PostJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="ce52e-131">Dans le code suivant, `_newItemName` est fourni par un élément lié du composant.</span><span class="sxs-lookup"><span data-stu-id="ce52e-131">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="ce52e-132">La méthode `AddItem` est déclenchée en sélectionnant un élément `<button>`.</span><span class="sxs-lookup"><span data-stu-id="ce52e-132">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="ce52e-133">Pour obtenir un exemple complet, consultez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="ce52e-133">See the sample app for a complete example.</span></span>

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
          await Http.PostJsonAsync("api/TodoItems", addItem);
      }
  }
  ```

* <span data-ttu-id="ce52e-134">`PutJsonAsync` &ndash; envoie une requête HTTP PUT, y compris du contenu encodé JSON.</span><span class="sxs-lookup"><span data-stu-id="ce52e-134">`PutJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="ce52e-135">Dans le code suivant, les valeurs `_editItem` pour les `Name` et `IsCompleted` sont fournies par les éléments dépendants du composant.</span><span class="sxs-lookup"><span data-stu-id="ce52e-135">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="ce52e-136">Le @no__t de l’élément est défini lorsque l’élément est sélectionné dans une autre partie de l’interface utilisateur et que `EditItem` est appelé.</span><span class="sxs-lookup"><span data-stu-id="ce52e-136">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="ce52e-137">La méthode `SaveItem` est déclenchée en sélectionnant l’élément Save `<button>`.</span><span class="sxs-lookup"><span data-stu-id="ce52e-137">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="ce52e-138">Pour obtenir un exemple complet, consultez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="ce52e-138">See the sample app for a complete example.</span></span>

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
          await Http.PutJsonAsync($"api/TodoItems/{_editItem.Id}, _editItem);
  }
  ```

<span data-ttu-id="ce52e-139"><xref:System.Net.Http> comprend des méthodes d’extension supplémentaires pour envoyer des requêtes HTTP et recevoir des réponses HTTP.</span><span class="sxs-lookup"><span data-stu-id="ce52e-139"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="ce52e-140">[Httpclient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) est utilisé pour envoyer une requête HTTP DELETE à une API Web.</span><span class="sxs-lookup"><span data-stu-id="ce52e-140">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="ce52e-141">Dans le code suivant, l’élément delete `<button>` appelle la méthode `DeleteItem`.</span><span class="sxs-lookup"><span data-stu-id="ce52e-141">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="ce52e-142">L’élément `<input>` lié fournit le `id` de l’élément à supprimer.</span><span class="sxs-lookup"><span data-stu-id="ce52e-142">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="ce52e-143">Pour obtenir un exemple complet, consultez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="ce52e-143">See the sample app for a complete example.</span></span>

```cshtml
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

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="ce52e-144">Partage des ressources Cross-Origin (CORS)</span><span class="sxs-lookup"><span data-stu-id="ce52e-144">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="ce52e-145">La sécurité du navigateur empêche une page Web d’effectuer des demandes vers un autre domaine que celui qui a servi la page Web.</span><span class="sxs-lookup"><span data-stu-id="ce52e-145">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="ce52e-146">Cette restriction est appelée *stratégie de même origine*.</span><span class="sxs-lookup"><span data-stu-id="ce52e-146">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="ce52e-147">La stratégie de même origine empêche un site malveillant de lire des données sensibles à partir d’un autre site.</span><span class="sxs-lookup"><span data-stu-id="ce52e-147">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="ce52e-148">Pour effectuer des demandes à partir du navigateur vers un point de terminaison avec une origine différente, le *point de terminaison* doit activer le [partage des ressources Cross-Origin (cors)](https://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="ce52e-148">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="ce52e-149">L’exemple d’application illustre l’utilisation de CORS dans le composant appeler l’API Web (*pages/CallWebAPI. Razor*).</span><span class="sxs-lookup"><span data-stu-id="ce52e-149">The sample app demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="ce52e-150">Pour permettre à d’autres sites d’effectuer des demandes de partage de ressources Cross-Origin (CORS) à votre application, consultez <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="ce52e-150">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="ce52e-151">HttpClient et HttpRequestMessage avec les options de demande d’API Fetch</span><span class="sxs-lookup"><span data-stu-id="ce52e-151">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="ce52e-152">Quand vous exécutez sur webassembly dans une application de webassembly éblouissante, utilisez [httpclient](xref:fundamentals/http-requests) et <xref:System.Net.Http.HttpRequestMessage> pour personnaliser les demandes.</span><span class="sxs-lookup"><span data-stu-id="ce52e-152">When running on WebAssembly in a Blazor WebAssembly app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="ce52e-153">Par exemple, vous pouvez spécifier l’URI de demande, la méthode HTTP et tous les en-têtes de demande souhaités.</span><span class="sxs-lookup"><span data-stu-id="ce52e-153">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

<span data-ttu-id="ce52e-154">Fournissez des options de demande à l' [API d’extraction](https://developer.mozilla.org/docs/Web/API/Fetch_API) JavaScript sous-jacente à l’aide de la propriété `WebAssemblyHttpMessageHandler.FetchArgs` sur la demande.</span><span class="sxs-lookup"><span data-stu-id="ce52e-154">Supply request options to the underlying JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) using the `WebAssemblyHttpMessageHandler.FetchArgs` property on the request.</span></span> <span data-ttu-id="ce52e-155">Comme indiqué dans l’exemple suivant, la propriété `credentials` est définie sur l’une des valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="ce52e-155">As shown in the following example, the `credentials` property is set to any of the following values:</span></span>

* <span data-ttu-id="ce52e-156">`FetchCredentialsOption.Include` (« Include ») &ndash; conseille au navigateur d’envoyer des informations d’identification (telles que des cookies ou des en-têtes d’authentification HTTP) même pour les demandes Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="ce52e-156">`FetchCredentialsOption.Include` ("include") &ndash; Advises the browser to send credentials (such as cookies or HTTP authentication headers) even for cross-origin requests.</span></span> <span data-ttu-id="ce52e-157">Autorisé uniquement lorsque la stratégie CORS est configurée pour autoriser les informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="ce52e-157">Only allowed when the CORS policy is configured to allow credentials.</span></span>
* <span data-ttu-id="ce52e-158">`FetchCredentialsOption.Omit` (« omettre ») &ndash; conseille au navigateur de ne jamais envoyer d’informations d’identification (telles que des cookies ou des en-têtes HTTP Auth).</span><span class="sxs-lookup"><span data-stu-id="ce52e-158">`FetchCredentialsOption.Omit` ("omit") &ndash; Advises the browser never to send credentials (such as cookies or HTTP auth headers).</span></span>
* <span data-ttu-id="ce52e-159">`FetchCredentialsOption.SameOrigin` (« même-Origin ») &ndash; conseille au navigateur d’envoyer des informations d’identification (telles que des cookies ou des en-têtes HTTP Auth) uniquement si l’URL cible est sur la même origine que l’application appelante.</span><span class="sxs-lookup"><span data-stu-id="ce52e-159">`FetchCredentialsOption.SameOrigin` ("same-origin") &ndash; Advises the browser to send credentials (such as cookies or HTTP auth headers) only if the target URL is on the same origin as the calling application.</span></span>

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

<span data-ttu-id="ce52e-160">Pour plus d’informations sur les options de l’API FETCH, consultez la page documentation @no__t 0MDN : WindowOrWorkerGlobalScope. Fetch () :P arameters @ no__t-0.</span><span class="sxs-lookup"><span data-stu-id="ce52e-160">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="ce52e-161">Lors de l’envoi d’informations d’identification (cookies/en-têtes d’autorisation) sur les demandes CORS, l’en-tête `Authorization` doit être autorisé par la stratégie CORS.</span><span class="sxs-lookup"><span data-stu-id="ce52e-161">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="ce52e-162">La stratégie suivante comprend la configuration pour :</span><span class="sxs-lookup"><span data-stu-id="ce52e-162">The following policy includes configuration for:</span></span>

* <span data-ttu-id="ce52e-163">Origines de la requête (`http://localhost:5000`, `https://localhost:5001`).</span><span class="sxs-lookup"><span data-stu-id="ce52e-163">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="ce52e-164">Toute méthode (verbe).</span><span class="sxs-lookup"><span data-stu-id="ce52e-164">Any method (verb).</span></span>
* <span data-ttu-id="ce52e-165">en-têtes `Content-Type` et `Authorization`.</span><span class="sxs-lookup"><span data-stu-id="ce52e-165">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="ce52e-166">Pour autoriser un en-tête personnalisé (par exemple, `x-custom-header`), répertoriez l’en-tête lors de l’appel de <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span><span class="sxs-lookup"><span data-stu-id="ce52e-166">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="ce52e-167">Informations d’identification définies par le code JavaScript côté client (la propriété `credentials` est définie sur `include`).</span><span class="sxs-lookup"><span data-stu-id="ce52e-167">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="ce52e-168">Pour plus d’informations, consultez <xref:security/cors> et le composant testeur de requêtes HTTP de l’exemple d’application (*composants/HTTPRequestTester. Razor*).</span><span class="sxs-lookup"><span data-stu-id="ce52e-168">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ce52e-169">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ce52e-169">Additional resources</span></span>

* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [<span data-ttu-id="ce52e-170">Configuration du point de terminaison HTTPs Kestrel</span><span class="sxs-lookup"><span data-stu-id="ce52e-170">Kestrel HTTPS endpoint configuration</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [<span data-ttu-id="ce52e-171">Cross Origin Resource Sharing (CORS) au W3C</span><span class="sxs-lookup"><span data-stu-id="ce52e-171">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
