---
title: Appeler une API Web à partir de ASP.NET Core éblouissant
author: guardrex
description: Découvrez comment appeler une API Web à partir d’une application éblouissant à l’aide des applications auxiliaires JSON, y compris la création de demandes de partage des ressources Cross-Origin (CORS).
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/25/2019
uid: blazor/call-web-api
ms.openlocfilehash: 1a13f9f1f9e660b39a1df584e49198c4bbb61533
ms.sourcegitcommit: 47cc13ab90913af9a2887cef0896bb4e9aba4dd5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/26/2019
ms.locfileid: "68948189"
---
# <a name="call-a-web-api-from-aspnet-core-blazor"></a><span data-ttu-id="4ae72-103">Appeler une API Web à partir de ASP.NET Core éblouissant</span><span class="sxs-lookup"><span data-stu-id="4ae72-103">Call a web API from ASP.NET Core Blazor</span></span>

<span data-ttu-id="4ae72-104">Par [Luke Latham](https://github.com/guardrex) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="4ae72-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="4ae72-105">Les applications côté client éblouissantes appellent des API Web à l’aide d' `HttpClient` un service préconfiguré.</span><span class="sxs-lookup"><span data-stu-id="4ae72-105">Blazor client-side apps call web APIs using a preconfigured `HttpClient` service.</span></span> <span data-ttu-id="4ae72-106">Composez des demandes, qui peuvent inclure des options de l' [API d’extraction](https://developer.mozilla.org/docs/Web/API/Fetch_API) JavaScript, à l' <xref:System.Net.Http.HttpRequestMessage>aide des applications auxiliaires de l’API JSON ou de.</span><span class="sxs-lookup"><span data-stu-id="4ae72-106">Compose requests, which can include JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) options, using Blazor JSON helpers or with <xref:System.Net.Http.HttpRequestMessage>.</span></span>

<span data-ttu-id="4ae72-107">Les applications côté serveur éblouissantes appellent des API Web <xref:System.Net.Http.HttpClient> à l’aide d' <xref:System.Net.Http.IHttpClientFactory>instances généralement créées à l’aide de.</span><span class="sxs-lookup"><span data-stu-id="4ae72-107">Blazor server-side apps call web APIs using <xref:System.Net.Http.HttpClient> instances typically created using <xref:System.Net.Http.IHttpClientFactory>.</span></span> <span data-ttu-id="4ae72-108">Pour plus d'informations, consultez <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="4ae72-108">For more information, see <xref:fundamentals/http-requests>.</span></span>

<span data-ttu-id="4ae72-109">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4ae72-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4ae72-110">Pour obtenir des exemples côté client éblouissants, consultez les composants suivants dans l’exemple d’application:</span><span class="sxs-lookup"><span data-stu-id="4ae72-110">For Blazor client-side examples, see the following components in the sample app:</span></span>

* <span data-ttu-id="4ae72-111">Appeler l’API Web (*pages/CallWebAPI. Razor*)</span><span class="sxs-lookup"><span data-stu-id="4ae72-111">Call Web API (*Pages/CallWebAPI.razor*)</span></span>
* <span data-ttu-id="4ae72-112">Testeur de requêtes HTTP (*composants/HTTPRequestTester. Razor*)</span><span class="sxs-lookup"><span data-stu-id="4ae72-112">HTTP Request Tester (*Components/HTTPRequestTester.razor*)</span></span>

## <a name="httpclient-and-json-helpers"></a><span data-ttu-id="4ae72-113">Applications auxiliaires HttpClient et JSON</span><span class="sxs-lookup"><span data-stu-id="4ae72-113">HttpClient and JSON helpers</span></span>

<span data-ttu-id="4ae72-114">Dans les applications côté client éblouissantes, [httpclient](xref:fundamentals/http-requests) est disponible en tant que service préconfiguré pour effectuer des requêtes sur le serveur d’origine.</span><span class="sxs-lookup"><span data-stu-id="4ae72-114">In Blazor client-side apps, [HttpClient](xref:fundamentals/http-requests) is available as a preconfigured service for making requests back to the origin server.</span></span> <span data-ttu-id="4ae72-115">`HttpClient`et les auxiliaires JSON sont également utilisés pour appeler des points de terminaison d’API Web tiers.</span><span class="sxs-lookup"><span data-stu-id="4ae72-115">`HttpClient` and JSON helpers are also used to call third-party web API endpoints.</span></span> <span data-ttu-id="4ae72-116">`HttpClient`est implémenté à l’aide de l' [API FETCH](https://developer.mozilla.org/docs/Web/API/Fetch_API) du navigateur et est soumis à ses limitations, y compris l’application de la même stratégie d’origine.</span><span class="sxs-lookup"><span data-stu-id="4ae72-116">`HttpClient` is implemented using the browser [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) and is subject to its limitations, including enforcement of the same origin policy.</span></span>

<span data-ttu-id="4ae72-117">L’adresse de base du client est définie sur l’adresse du serveur d’origine.</span><span class="sxs-lookup"><span data-stu-id="4ae72-117">The client's base address is set to the originating server's address.</span></span> <span data-ttu-id="4ae72-118">Injecter `HttpClient` une instance à `@inject` l’aide de la directive:</span><span class="sxs-lookup"><span data-stu-id="4ae72-118">Inject an `HttpClient` instance using the `@inject` directive:</span></span>

```cshtml
@using System.Net.Http
@inject HttpClient Http
```

<span data-ttu-id="4ae72-119">Dans les exemples suivants, une API Web todo traite les opérations de création, lecture, mise à jour et suppression (CRUD).</span><span class="sxs-lookup"><span data-stu-id="4ae72-119">In the following examples, a Todo web API processes create, read, update, and delete (CRUD) operations.</span></span> <span data-ttu-id="4ae72-120">Les exemples sont basés sur une `TodoItem` classe qui stocke les éléments suivants:</span><span class="sxs-lookup"><span data-stu-id="4ae72-120">The examples are based on a `TodoItem` class that stores the:</span></span>

* <span data-ttu-id="4ae72-121">ID (`Id`, `long`) &ndash; ID unique de l’élément.</span><span class="sxs-lookup"><span data-stu-id="4ae72-121">ID (`Id`, `long`) &ndash; Unique ID of the item.</span></span>
* <span data-ttu-id="4ae72-122">Nom (`Name`, `string`) &ndash; nom de l’élément.</span><span class="sxs-lookup"><span data-stu-id="4ae72-122">Name (`Name`, `string`) &ndash; Name of the item.</span></span>
* <span data-ttu-id="4ae72-123">État (`IsComplete`, `bool`) &ndash; indiquant si l’élément TODO est terminé.</span><span class="sxs-lookup"><span data-stu-id="4ae72-123">Status (`IsComplete`, `bool`) &ndash; Indication if the Todo item is finished.</span></span>

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

<span data-ttu-id="4ae72-124">Les méthodes d’assistance JSON envoient des demandes à un URI (une API Web dans les exemples suivants) et traitent la réponse:</span><span class="sxs-lookup"><span data-stu-id="4ae72-124">JSON helper methods send requests to a URI (a web API in the following examples) and process the response:</span></span>

* <span data-ttu-id="4ae72-125">`GetJsonAsync`&ndash; Envoie une requête http obtenir et analyse le corps de la réponse JSON pour créer un objet.</span><span class="sxs-lookup"><span data-stu-id="4ae72-125">`GetJsonAsync` &ndash; Sends an HTTP GET request and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="4ae72-126">Dans le code suivant, les `_todoItems` sont affichés par le composant.</span><span class="sxs-lookup"><span data-stu-id="4ae72-126">In the following code, the `_todoItems` are displayed by the component.</span></span> <span data-ttu-id="4ae72-127">La `GetTodoItems` méthode est déclenchée lorsque le rendu du composant est terminé ([OnInitAsync](xref:blazor/components#lifecycle-methods)).</span><span class="sxs-lookup"><span data-stu-id="4ae72-127">The `GetTodoItems` method is triggered when the component is finished rendering ([OnInitAsync](xref:blazor/components#lifecycle-methods)).</span></span> <span data-ttu-id="4ae72-128">Pour obtenir un exemple complet, consultez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="4ae72-128">See the sample app for a complete example.</span></span>

  ```cshtml
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] _todoItems;

      protected override async Task OnInitAsync() => 
          _todoItems = await Http.GetJsonAsync<TodoItem[]>("api/todo");
  }
  ```

* <span data-ttu-id="4ae72-129">`PostJsonAsync`&ndash; Envoie une requête http postérieure, y compris du contenu encodé JSON, et analyse le corps de la réponse JSON pour créer un objet.</span><span class="sxs-lookup"><span data-stu-id="4ae72-129">`PostJsonAsync` &ndash; Sends an HTTP POST request, including JSON-encoded content, and parses the JSON response body to create an object.</span></span>

  <span data-ttu-id="4ae72-130">Dans le code suivant, `_newItemName` est fourni par un élément lié du composant.</span><span class="sxs-lookup"><span data-stu-id="4ae72-130">In the following code, `_newItemName` is provided by a bound element of the component.</span></span> <span data-ttu-id="4ae72-131">La `AddItem` méthode est déclenchée par la sélection `<button>` d’un élément.</span><span class="sxs-lookup"><span data-stu-id="4ae72-131">The `AddItem` method is triggered by selecting a `<button>` element.</span></span> <span data-ttu-id="4ae72-132">Pour obtenir un exemple complet, consultez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="4ae72-132">See the sample app for a complete example.</span></span>

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

* <span data-ttu-id="4ae72-133">`PutJsonAsync`&ndash; Envoie une requête HTTP PUT, y compris du contenu encodé JSON.</span><span class="sxs-lookup"><span data-stu-id="4ae72-133">`PutJsonAsync` &ndash; Sends an HTTP PUT request, including JSON-encoded content.</span></span>

  <span data-ttu-id="4ae72-134">Dans le code suivant, `_editItem` les valeurs `Name` pour `IsCompleted` et sont fournies par les éléments dépendants du composant.</span><span class="sxs-lookup"><span data-stu-id="4ae72-134">In the following code, `_editItem` values for `Name` and `IsCompleted` are provided by bound elements of the component.</span></span> <span data-ttu-id="4ae72-135">L’élément `Id` est défini lorsque l’élément est sélectionné dans une autre partie de l’interface utilisateur `EditItem` et est appelé.</span><span class="sxs-lookup"><span data-stu-id="4ae72-135">The item's `Id` is set when the item is selected in another part of the UI and `EditItem` is called.</span></span> <span data-ttu-id="4ae72-136">La `SaveItem` méthode est déclenchée par la sélection de `<button>` l’élément Save.</span><span class="sxs-lookup"><span data-stu-id="4ae72-136">The `SaveItem` method is triggered by selecting the Save `<button>` element.</span></span> <span data-ttu-id="4ae72-137">Pour obtenir un exemple complet, consultez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="4ae72-137">See the sample app for a complete example.</span></span>

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

<span data-ttu-id="4ae72-138"><xref:System.Net.Http>contient des méthodes d’extension supplémentaires pour envoyer des requêtes HTTP et recevoir des réponses HTTP.</span><span class="sxs-lookup"><span data-stu-id="4ae72-138"><xref:System.Net.Http> includes additional extension methods for sending HTTP requests and receiving HTTP responses.</span></span> <span data-ttu-id="4ae72-139">[Httpclient. DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) est utilisé pour envoyer une requête HTTP DELETE à une API Web.</span><span class="sxs-lookup"><span data-stu-id="4ae72-139">[HttpClient.DeleteAsync](xref:System.Net.Http.HttpClient.DeleteAsync*) is used to send an HTTP DELETE request to a web API.</span></span>

<span data-ttu-id="4ae72-140">Dans le code suivant, l’élément `<button>` delete appelle la `DeleteItem` méthode.</span><span class="sxs-lookup"><span data-stu-id="4ae72-140">In the following code, the Delete `<button>` element calls the `DeleteItem` method.</span></span> <span data-ttu-id="4ae72-141">L’élément `<input>` lié fournit le `id` de l’élément à supprimer.</span><span class="sxs-lookup"><span data-stu-id="4ae72-141">The bound `<input>` element supplies the `id` of the item to delete.</span></span> <span data-ttu-id="4ae72-142">Pour obtenir un exemple complet, consultez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="4ae72-142">See the sample app for a complete example.</span></span>

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

## <a name="cross-origin-resource-sharing-cors"></a><span data-ttu-id="4ae72-143">Partage des ressources Cross-Origin (CORS)</span><span class="sxs-lookup"><span data-stu-id="4ae72-143">Cross-origin resource sharing (CORS)</span></span>

<span data-ttu-id="4ae72-144">La sécurité du navigateur empêche une page Web d’effectuer des demandes vers un autre domaine que celui qui a servi la page Web.</span><span class="sxs-lookup"><span data-stu-id="4ae72-144">Browser security prevents a webpage from making requests to a different domain than the one that served the webpage.</span></span> <span data-ttu-id="4ae72-145">Cette restriction est appelée *stratégie de même origine*.</span><span class="sxs-lookup"><span data-stu-id="4ae72-145">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="4ae72-146">La stratégie de même origine empêche un site malveillant de lire des données sensibles à partir d’un autre site.</span><span class="sxs-lookup"><span data-stu-id="4ae72-146">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="4ae72-147">Pour effectuer des demandes à partir du navigateur vers un point de terminaison avec une origine différente, le *point de terminaison* doit activer le [partage des ressources Cross-Origin (cors)](https://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="4ae72-147">To make requests from the browser to an endpoint with a different origin, the *endpoint* must enable [cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/).</span></span>

<span data-ttu-id="4ae72-148">L’exemple d’application illustre l’utilisation de CORS dans le composant appeler l’API Web (*pages/CallWebAPI. Razor*).</span><span class="sxs-lookup"><span data-stu-id="4ae72-148">The sample app demonstrates the use of CORS in the Call Web API component (*Pages/CallWebAPI.razor*).</span></span>

<span data-ttu-id="4ae72-149">Pour permettre à d’autres sites d’effectuer des demandes de partage de ressources Cross-Origin (CORS) <xref:security/cors>à votre application, consultez.</span><span class="sxs-lookup"><span data-stu-id="4ae72-149">To allow other sites to make cross-origin resource sharing (CORS) requests to your app, see <xref:security/cors>.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="4ae72-150">HttpClient et HttpRequestMessage avec les options de demande d’API Fetch</span><span class="sxs-lookup"><span data-stu-id="4ae72-150">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="4ae72-151">Quand vous exécutez sur webassembly dans une application cliente éblouissant, utilisez [httpclient](xref:fundamentals/http-requests) et <xref:System.Net.Http.HttpRequestMessage> pour personnaliser les demandes.</span><span class="sxs-lookup"><span data-stu-id="4ae72-151">When running on WebAssembly in a Blazor client-side app, use [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> to customize requests.</span></span> <span data-ttu-id="4ae72-152">Par exemple, vous pouvez spécifier l’URI de demande, la méthode HTTP et tous les en-têtes de demande souhaités.</span><span class="sxs-lookup"><span data-stu-id="4ae72-152">For example, you can specify the request URI, HTTP method, and any desired request headers.</span></span>

<span data-ttu-id="4ae72-153">Fournissez des options de demande à l' [API](https://developer.mozilla.org/docs/Web/API/Fetch_API) d’extraction `WebAssemblyHttpMessageHandler.FetchArgs` JavaScript sous-jacente à l’aide de la propriété sur la demande.</span><span class="sxs-lookup"><span data-stu-id="4ae72-153">Supply request options to the underlying JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) using the `WebAssemblyHttpMessageHandler.FetchArgs` property on the request.</span></span> <span data-ttu-id="4ae72-154">Comme indiqué dans l’exemple suivant, la `credentials` propriété est définie sur l’une des valeurs suivantes:</span><span class="sxs-lookup"><span data-stu-id="4ae72-154">As shown in the following example, the `credentials` property is set to any of the following values:</span></span>

* <span data-ttu-id="4ae72-155">`FetchCredentialsOption.Include`(«Include») &ndash; Conseille au navigateur d’envoyer des informations d’identification (telles que des en-têtes de cookies ou d’authentification http) même pour les demandes Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="4ae72-155">`FetchCredentialsOption.Include` ("include") &ndash; Advises the browser to send credentials (such as cookies or HTTP authentication headers) even for cross-origin requests.</span></span> <span data-ttu-id="4ae72-156">Autorisé uniquement lorsque la stratégie CORS est configurée pour autoriser les informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="4ae72-156">Only allowed when the CORS policy is configured to allow credentials.</span></span>
* <span data-ttu-id="4ae72-157">`FetchCredentialsOption.Omit`(«omettre») &ndash; Conseille au navigateur de ne jamais envoyer d’informations d’identification (par exemple, cookies ou en-têtes http auth).</span><span class="sxs-lookup"><span data-stu-id="4ae72-157">`FetchCredentialsOption.Omit` ("omit") &ndash; Advises the browser never to send credentials (such as cookies or HTTP auth headers).</span></span>
* <span data-ttu-id="4ae72-158">`FetchCredentialsOption.SameOrigin`(«même-Origin») &ndash; Conseille au navigateur d’envoyer des informations d’identification (par exemple, cookies ou en-têtes http auth) uniquement si l’URL cible est sur la même origine que l’application appelante.</span><span class="sxs-lookup"><span data-stu-id="4ae72-158">`FetchCredentialsOption.SameOrigin` ("same-origin") &ndash; Advises the browser to send credentials (such as cookies or HTTP auth headers) only if the target URL is on the same origin as the calling application.</span></span>

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

<span data-ttu-id="4ae72-159">Pour plus d’informations sur les options de l' [API FETCH, consultez MDN Web docs: WindowOrWorkerGlobalScope. Fetch ():P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span><span class="sxs-lookup"><span data-stu-id="4ae72-159">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="4ae72-160">Lors de l’envoi d’informations d’identification (cookies/en-têtes d’autorisation `Authorization` ) sur les demandes cors, l’en-tête doit être autorisé par la stratégie cors.</span><span class="sxs-lookup"><span data-stu-id="4ae72-160">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="4ae72-161">La stratégie suivante comprend la configuration pour:</span><span class="sxs-lookup"><span data-stu-id="4ae72-161">The following policy includes configuration for:</span></span>

* <span data-ttu-id="4ae72-162">Origines des demandes`http://localhost:5000`( `https://localhost:5001`,).</span><span class="sxs-lookup"><span data-stu-id="4ae72-162">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="4ae72-163">Toute méthode (verbe).</span><span class="sxs-lookup"><span data-stu-id="4ae72-163">Any method (verb).</span></span>
* <span data-ttu-id="4ae72-164">`Content-Type`et `Authorization` en-têtes.</span><span class="sxs-lookup"><span data-stu-id="4ae72-164">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="4ae72-165">Pour autoriser un en-tête personnalisé (par `x-custom-header`exemple,), répertoriez l' <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>en-tête lors de l’appel de.</span><span class="sxs-lookup"><span data-stu-id="4ae72-165">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="4ae72-166">Informations d’identification définies par le code JavaScript côté client`credentials` (la propriété `include`a la valeur).</span><span class="sxs-lookup"><span data-stu-id="4ae72-166">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="4ae72-167">Pour plus d’informations, <xref:security/cors> consultez et le composant testeur de requêtes http de l’exemple d’application (*composants/HTTPRequestTester. Razor*).</span><span class="sxs-lookup"><span data-stu-id="4ae72-167">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4ae72-168">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="4ae72-168">Additional resources</span></span>

* <xref:fundamentals/http-requests>
* [<span data-ttu-id="4ae72-169">Cross Origin Resource Sharing (CORS) au W3C</span><span class="sxs-lookup"><span data-stu-id="4ae72-169">Cross Origin Resource Sharing (CORS) at W3C</span></span>](https://www.w3.org/TR/cors/)
