---
title: Effectuer des requêtes HTTP en utilisant IHttpClientFactory dans ASP.NET Core
author: stevejgordon
description: Découvrez plus d’informations sur l’utilisation de l’interface IHttpClientFactory pour gérer des instances HttpClient logiques dans ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/27/2019
uid: fundamentals/http-requests
ms.openlocfilehash: 7a5b5c84775ea2482034ef9f3e8a2376036e66cb
ms.sourcegitcommit: a104ba258ae7c0b3ee7c6fa7eaea1ddeb8b6eb73
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74478738"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="de69a-103">Effectuer des requêtes HTTP en utilisant IHttpClientFactory dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="de69a-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="de69a-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="de69a-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="de69a-105">Une <xref:System.Net.Http.IHttpClientFactory> peut être inscrite et utilisée pour configurer et créer des instances de <xref:System.Net.Http.HttpClient> dans une application.</span><span class="sxs-lookup"><span data-stu-id="de69a-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="de69a-106">`IHttpClientFactory` offers the following benefits:</span><span class="sxs-lookup"><span data-stu-id="de69a-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="de69a-107">Fournit un emplacement central pour le nommage et la configuration d’instance de `HttpClient` logiques.</span><span class="sxs-lookup"><span data-stu-id="de69a-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="de69a-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="de69a-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="de69a-109">A default client can be registered for general access.</span><span class="sxs-lookup"><span data-stu-id="de69a-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="de69a-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="de69a-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="de69a-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="de69a-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="de69a-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span><span class="sxs-lookup"><span data-stu-id="de69a-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="de69a-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span><span class="sxs-lookup"><span data-stu-id="de69a-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="de69a-114">Ajoute une expérience de journalisation configurable (via `ILogger`) pour toutes les requêtes envoyées via des clients créés par la fabrique.</span><span class="sxs-lookup"><span data-stu-id="de69a-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="de69a-115">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="de69a-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="de69a-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span><span class="sxs-lookup"><span data-stu-id="de69a-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="de69a-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span><span class="sxs-lookup"><span data-stu-id="de69a-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="de69a-118">Modèles de consommation</span><span class="sxs-lookup"><span data-stu-id="de69a-118">Consumption patterns</span></span>

<span data-ttu-id="de69a-119">Vous pouvez utiliser `IHttpClientFactory` dans une application de plusieurs façons :</span><span class="sxs-lookup"><span data-stu-id="de69a-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="de69a-120">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="de69a-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="de69a-121">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="de69a-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="de69a-122">Clients typés</span><span class="sxs-lookup"><span data-stu-id="de69a-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="de69a-123">Clients générés</span><span class="sxs-lookup"><span data-stu-id="de69a-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="de69a-124">The best approach depends upon the app's requirements.</span><span class="sxs-lookup"><span data-stu-id="de69a-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="de69a-125">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="de69a-125">Basic usage</span></span>

<span data-ttu-id="de69a-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span><span class="sxs-lookup"><span data-stu-id="de69a-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="de69a-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="de69a-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="de69a-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span><span class="sxs-lookup"><span data-stu-id="de69a-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="de69a-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span><span class="sxs-lookup"><span data-stu-id="de69a-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="de69a-130">It has no impact on how `HttpClient` is used.</span><span class="sxs-lookup"><span data-stu-id="de69a-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="de69a-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="de69a-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="de69a-132">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="de69a-132">Named clients</span></span>

<span data-ttu-id="de69a-133">Named clients are a good choice when:</span><span class="sxs-lookup"><span data-stu-id="de69a-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="de69a-134">The app requires many distinct uses of `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="de69a-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="de69a-135">Many `HttpClient`s have different configuration.</span><span class="sxs-lookup"><span data-stu-id="de69a-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="de69a-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="de69a-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="de69a-137">In the preceding code the client is configured with:</span><span class="sxs-lookup"><span data-stu-id="de69a-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="de69a-138">The base address `https://api.github.com/`.</span><span class="sxs-lookup"><span data-stu-id="de69a-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="de69a-139">Two headers required to work with the GitHub API.</span><span class="sxs-lookup"><span data-stu-id="de69a-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="de69a-140">CreateClient</span><span class="sxs-lookup"><span data-stu-id="de69a-140">CreateClient</span></span>

<span data-ttu-id="de69a-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span><span class="sxs-lookup"><span data-stu-id="de69a-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="de69a-142">A new instance of `HttpClient` is created.</span><span class="sxs-lookup"><span data-stu-id="de69a-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="de69a-143">The configuration action is called.</span><span class="sxs-lookup"><span data-stu-id="de69a-143">The configuration action is called.</span></span>

<span data-ttu-id="de69a-144">To create a named client, pass its name into `CreateClient`:</span><span class="sxs-lookup"><span data-stu-id="de69a-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="de69a-145">Dans le code précédent, la requête n’a pas besoin de spécifier un nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="de69a-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="de69a-146">The code can pass just the path, since the base address configured for the client is used.</span><span class="sxs-lookup"><span data-stu-id="de69a-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="de69a-147">Clients typés</span><span class="sxs-lookup"><span data-stu-id="de69a-147">Typed clients</span></span>

<span data-ttu-id="de69a-148">Clients typés :</span><span class="sxs-lookup"><span data-stu-id="de69a-148">Typed clients:</span></span>

* <span data-ttu-id="de69a-149">Fournissent les mêmes fonctionnalités que les clients nommés, sans qu’il soit nécessaire d’utiliser des chaînes comme clés.</span><span class="sxs-lookup"><span data-stu-id="de69a-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="de69a-150">Bénéficie de l’aide d’IntelliSense et du compilateur lors de l’utilisation des clients.</span><span class="sxs-lookup"><span data-stu-id="de69a-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="de69a-151">Fournissent un emplacement unique pour configurer et interagir avec un `HttpClient` particulier.</span><span class="sxs-lookup"><span data-stu-id="de69a-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="de69a-152">For example, a single typed client might be used:</span><span class="sxs-lookup"><span data-stu-id="de69a-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="de69a-153">For a single backend endpoint.</span><span class="sxs-lookup"><span data-stu-id="de69a-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="de69a-154">To encapsulate all logic dealing with the endpoint.</span><span class="sxs-lookup"><span data-stu-id="de69a-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="de69a-155">Work with DI and can be injected where required in the app.</span><span class="sxs-lookup"><span data-stu-id="de69a-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="de69a-156">Un client typé accepte un `HttpClient` dans son constructeur :</span><span class="sxs-lookup"><span data-stu-id="de69a-156">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="de69a-157">Dans le code précédent :</span><span class="sxs-lookup"><span data-stu-id="de69a-157">In the preceding code:</span></span>

* <span data-ttu-id="de69a-158">The configuration is moved into the typed client.</span><span class="sxs-lookup"><span data-stu-id="de69a-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="de69a-159">L’objet `HttpClient` est exposé en tant que propriété publique.</span><span class="sxs-lookup"><span data-stu-id="de69a-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="de69a-160">API-specific methods can be created that expose `HttpClient` functionality.</span><span class="sxs-lookup"><span data-stu-id="de69a-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="de69a-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span><span class="sxs-lookup"><span data-stu-id="de69a-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="de69a-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span><span class="sxs-lookup"><span data-stu-id="de69a-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="de69a-163">Le client typé est inscrit comme étant transitoire avec injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="de69a-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="de69a-164">Le client typé peut être injecté et utilisé directement :</span><span class="sxs-lookup"><span data-stu-id="de69a-164">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="de69a-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span><span class="sxs-lookup"><span data-stu-id="de69a-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="de69a-166">The `HttpClient` can be encapsulated within a typed client.</span><span class="sxs-lookup"><span data-stu-id="de69a-166">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="de69a-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span><span class="sxs-lookup"><span data-stu-id="de69a-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="de69a-168">In the preceding code, the `HttpClient` is stored in a private field.</span><span class="sxs-lookup"><span data-stu-id="de69a-168">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="de69a-169">Access to the `HttpClient` is by the public `GetRepos` method.</span><span class="sxs-lookup"><span data-stu-id="de69a-169">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="de69a-170">Clients générés</span><span class="sxs-lookup"><span data-stu-id="de69a-170">Generated clients</span></span>

<span data-ttu-id="de69a-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="de69a-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="de69a-172">Refit est une bibliothèque REST pour .NET.</span><span class="sxs-lookup"><span data-stu-id="de69a-172">Refit is a REST library for .NET.</span></span> <span data-ttu-id="de69a-173">Il convertit les API REST en interfaces dynamiques.</span><span class="sxs-lookup"><span data-stu-id="de69a-173">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="de69a-174">Une implémentation de l’interface est générée dynamiquement par le `RestService`, avec `HttpClient` pour faire les appels HTTP externes.</span><span class="sxs-lookup"><span data-stu-id="de69a-174">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="de69a-175">Une interface et une réponse sont définies pour représenter l’API externe et sa réponse :</span><span class="sxs-lookup"><span data-stu-id="de69a-175">An interface and a reply are defined to represent the external API and its response:</span></span>

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

<span data-ttu-id="de69a-176">Vous pouvez ajouter un client typé en utilisant Refit pour générer l’implémentation :</span><span class="sxs-lookup"><span data-stu-id="de69a-176">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddControllers();
}
```

<span data-ttu-id="de69a-177">L’interface définie peut être utilisée quand c’est nécessaire, avec l’implémentation fournie par l’injection de dépendances et Refit :</span><span class="sxs-lookup"><span data-stu-id="de69a-177">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a><span data-ttu-id="de69a-178">Middleware pour les requêtes sortantes</span><span class="sxs-lookup"><span data-stu-id="de69a-178">Outgoing request middleware</span></span>

<span data-ttu-id="de69a-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span><span class="sxs-lookup"><span data-stu-id="de69a-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="de69a-180">`IHttpClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="de69a-180">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="de69a-181">Simplifies defining the handlers to apply for each named client.</span><span class="sxs-lookup"><span data-stu-id="de69a-181">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="de69a-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span><span class="sxs-lookup"><span data-stu-id="de69a-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="de69a-183">Chacun de ces gestionnaires peut effectuer un travail avant et après la requête sortante.</span><span class="sxs-lookup"><span data-stu-id="de69a-183">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="de69a-184">This pattern:</span><span class="sxs-lookup"><span data-stu-id="de69a-184">This pattern:</span></span>

  * <span data-ttu-id="de69a-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="de69a-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="de69a-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span><span class="sxs-lookup"><span data-stu-id="de69a-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="de69a-187">mettre en cache</span><span class="sxs-lookup"><span data-stu-id="de69a-187">caching</span></span>
    * <span data-ttu-id="de69a-188">gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="de69a-188">error handling</span></span>
    * <span data-ttu-id="de69a-189">sérialisation</span><span class="sxs-lookup"><span data-stu-id="de69a-189">serialization</span></span>
    * <span data-ttu-id="de69a-190">enregistrer</span><span class="sxs-lookup"><span data-stu-id="de69a-190">logging</span></span>

<span data-ttu-id="de69a-191">To create a delegating handler:</span><span class="sxs-lookup"><span data-stu-id="de69a-191">To create a delegating handler:</span></span>

* <span data-ttu-id="de69a-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="de69a-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="de69a-193">Substituez <xref:System.Net.Http.DelegatingHandler.SendAsync*></span><span class="sxs-lookup"><span data-stu-id="de69a-193">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="de69a-194">Execute code before passing the request to the next handler in the pipeline:</span><span class="sxs-lookup"><span data-stu-id="de69a-194">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="de69a-195">The preceding code checks if the `X-API-KEY` header is in the request.</span><span class="sxs-lookup"><span data-stu-id="de69a-195">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="de69a-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span><span class="sxs-lookup"><span data-stu-id="de69a-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="de69a-197">More than one handler can be added to the configuration for a `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span><span class="sxs-lookup"><span data-stu-id="de69a-197">More than one handler can be added to the configuration for a `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="de69a-198">Dans le code précédent, le `ValidateHeaderHandler` est inscrit avec une injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="de69a-198">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="de69a-199">`IHttpClientFactory` crée une étendue DI distincte pour chaque gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="de69a-199">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="de69a-200">Handlers can depend upon services of any scope.</span><span class="sxs-lookup"><span data-stu-id="de69a-200">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="de69a-201">Les services dont dépendent les gestionnaires sont supprimés lorsque le gestionnaire est supprimé.</span><span class="sxs-lookup"><span data-stu-id="de69a-201">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="de69a-202">Une fois inscrit, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> peut être appelé en passant en entrée le type pour le gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="de69a-202">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="de69a-203">Vous pouvez inscrire plusieurs gestionnaires dans l’ordre où ils doivent être exécutés.</span><span class="sxs-lookup"><span data-stu-id="de69a-203">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="de69a-204">Chaque gestionnaire wrappe le gestionnaire suivant jusqu’à ce que le dernier `HttpClientHandler` exécute la requête :</span><span class="sxs-lookup"><span data-stu-id="de69a-204">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="de69a-205">Utilisez l’une des approches suivantes pour partager l’état de chaque requête avec les gestionnaires de messages :</span><span class="sxs-lookup"><span data-stu-id="de69a-205">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="de69a-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span><span class="sxs-lookup"><span data-stu-id="de69a-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="de69a-207">Utilisez <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> pour accéder à la requête en cours.</span><span class="sxs-lookup"><span data-stu-id="de69a-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="de69a-208">Créez un objet de stockage <xref:System.Threading.AsyncLocal`1> personnalisé pour passer les données.</span><span class="sxs-lookup"><span data-stu-id="de69a-208">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="de69a-209">Utiliser les gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="de69a-209">Use Polly-based handlers</span></span>

<span data-ttu-id="de69a-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="de69a-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="de69a-211">Polly est une bibliothèque complète de gestion des erreurs transitoires et de résilience pour .NET.</span><span class="sxs-lookup"><span data-stu-id="de69a-211">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="de69a-212">Elle permet aux développeurs de formuler facilement et de façon thread-safe des stratégies, comme Retry (Nouvelle tentative), Circuit Breaker (Disjoncteur), Timeout (Délai d’attente), Bulkhead Isolation (Isolation par cloisonnement) et Fallback (Alternative de repli).</span><span class="sxs-lookup"><span data-stu-id="de69a-212">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="de69a-213">Des méthodes d’extension sont fournies pour permettre l’utilisation de stratégies Polly avec les instances configurées de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="de69a-213">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="de69a-214">The Polly extensions support adding Polly-based handlers to clients.</span><span class="sxs-lookup"><span data-stu-id="de69a-214">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="de69a-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span><span class="sxs-lookup"><span data-stu-id="de69a-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="de69a-216">Gérer les erreurs temporaires</span><span class="sxs-lookup"><span data-stu-id="de69a-216">Handle transient faults</span></span>

<span data-ttu-id="de69a-217">Faults typically occur when external HTTP calls are transient.</span><span class="sxs-lookup"><span data-stu-id="de69a-217">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="de69a-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span><span class="sxs-lookup"><span data-stu-id="de69a-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="de69a-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span><span class="sxs-lookup"><span data-stu-id="de69a-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="de69a-220">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="de69a-220">HTTP 5xx</span></span>
* <span data-ttu-id="de69a-221">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="de69a-221">HTTP 408</span></span>

<span data-ttu-id="de69a-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span><span class="sxs-lookup"><span data-stu-id="de69a-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="de69a-223">Dans le code précédent, une stratégie `WaitAndRetryAsync` est définie.</span><span class="sxs-lookup"><span data-stu-id="de69a-223">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="de69a-224">Les requêtes qui ont échoué sont retentées jusqu’à trois fois avec un délai de 600 ms entre les tentatives.</span><span class="sxs-lookup"><span data-stu-id="de69a-224">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="de69a-225">Sélectionner dynamiquement des stratégies</span><span class="sxs-lookup"><span data-stu-id="de69a-225">Dynamically select policies</span></span>

<span data-ttu-id="de69a-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span><span class="sxs-lookup"><span data-stu-id="de69a-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="de69a-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span><span class="sxs-lookup"><span data-stu-id="de69a-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="de69a-228">Dans le code précédent, si la requête sortante est un HTTP GET, un délai d’attente de 10 secondes est appliqué.</span><span class="sxs-lookup"><span data-stu-id="de69a-228">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="de69a-229">Pour toutes les autres méthodes HTTP, un délai d’attente de 30 secondes est utilisé.</span><span class="sxs-lookup"><span data-stu-id="de69a-229">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="de69a-230">Ajouter plusieurs gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="de69a-230">Add multiple Polly handlers</span></span>

<span data-ttu-id="de69a-231">It's common to nest Polly policies:</span><span class="sxs-lookup"><span data-stu-id="de69a-231">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="de69a-232">Dans l'exemple précédent :</span><span class="sxs-lookup"><span data-stu-id="de69a-232">In the preceding example:</span></span>

* <span data-ttu-id="de69a-233">Two handlers are added.</span><span class="sxs-lookup"><span data-stu-id="de69a-233">Two handlers are added.</span></span>
* <span data-ttu-id="de69a-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span><span class="sxs-lookup"><span data-stu-id="de69a-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="de69a-235">Les requêtes qui ont échoué sont retentées jusqu’à trois fois.</span><span class="sxs-lookup"><span data-stu-id="de69a-235">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="de69a-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span><span class="sxs-lookup"><span data-stu-id="de69a-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="de69a-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span><span class="sxs-lookup"><span data-stu-id="de69a-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="de69a-238">Les stratégies de disjoncteur sont avec état.</span><span class="sxs-lookup"><span data-stu-id="de69a-238">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="de69a-239">Tous les appels effectués via ce client partagent le même état du circuit.</span><span class="sxs-lookup"><span data-stu-id="de69a-239">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="de69a-240">Ajouter des stratégies à partir du Registre Polly</span><span class="sxs-lookup"><span data-stu-id="de69a-240">Add policies from the Polly registry</span></span>

<span data-ttu-id="de69a-241">Une approche de la gestion des stratégies régulièrement utilisées consiste à les définir une seule fois et à les inscrire avec un `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="de69a-241">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="de69a-242">Dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="de69a-242">In the following code:</span></span>

* <span data-ttu-id="de69a-243">The "regular" and "long" polices are added.</span><span class="sxs-lookup"><span data-stu-id="de69a-243">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="de69a-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span><span class="sxs-lookup"><span data-stu-id="de69a-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="de69a-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="de69a-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="de69a-246">HttpClient et gestion de la durée de vie</span><span class="sxs-lookup"><span data-stu-id="de69a-246">HttpClient and lifetime management</span></span>

<span data-ttu-id="de69a-247">Une nouvelle instance `HttpClient` est retournée à chaque fois que `CreateClient` est appelé sur `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="de69a-247">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="de69a-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span><span class="sxs-lookup"><span data-stu-id="de69a-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="de69a-249">La fabrique gère les durées de vie des instances `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="de69a-249">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="de69a-250">`IHttpClientFactory` regroupe dans un pool les instances de `HttpMessageHandler` créées par la fabrique pour réduire la consommation des ressources.</span><span class="sxs-lookup"><span data-stu-id="de69a-250">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="de69a-251">Une instance de `HttpMessageHandler` peut être réutilisée à partir du pool quand vous créez une instance de `HttpClient` si sa durée de vie n’a pas expiré.</span><span class="sxs-lookup"><span data-stu-id="de69a-251">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="de69a-252">Le regroupement de gestionnaires en pools est souhaitable, car chaque gestionnaire gère habituellement ses propres connexions HTTP sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="de69a-252">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="de69a-253">La création de plus de gestionnaires que nécessaire peut entraîner des délais de connexion.</span><span class="sxs-lookup"><span data-stu-id="de69a-253">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="de69a-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span><span class="sxs-lookup"><span data-stu-id="de69a-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="de69a-255">La durée de vie par défaut d’un gestionnaire est de deux minutes.</span><span class="sxs-lookup"><span data-stu-id="de69a-255">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="de69a-256">The default value can be overridden on a per named client basis:</span><span class="sxs-lookup"><span data-stu-id="de69a-256">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="de69a-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span><span class="sxs-lookup"><span data-stu-id="de69a-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="de69a-258">La suppression annule les requêtes sortantes et garantit que l’instance `HttpClient` donnée ne peut pas être utilisée après avoir appelé <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="de69a-258">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="de69a-259">`IHttpClientFactory` effectue le suivi et libère les ressources utilisées par les instances `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="de69a-259">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="de69a-260">Le fait de conserver une seule instance `HttpClient` active pendant une longue durée est un modèle commun utilisé avant le lancement de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="de69a-260">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="de69a-261">Ce modèle devient inutile après la migration vers `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="de69a-261">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="de69a-262">Alternatives to IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="de69a-262">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="de69a-263">Using `IHttpClientFactory` in a DI-enabled app avoids:</span><span class="sxs-lookup"><span data-stu-id="de69a-263">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="de69a-264">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span><span class="sxs-lookup"><span data-stu-id="de69a-264">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="de69a-265">Stale-DNS problems by cycling `HttpMessageHandler` instances at regular instances.</span><span class="sxs-lookup"><span data-stu-id="de69a-265">Stale-DNS problems by cycling `HttpMessageHandler` instances at regular instances.</span></span>

<span data-ttu-id="de69a-266">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span><span class="sxs-lookup"><span data-stu-id="de69a-266">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="de69a-267">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span><span class="sxs-lookup"><span data-stu-id="de69a-267">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="de69a-268">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span><span class="sxs-lookup"><span data-stu-id="de69a-268">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="de69a-269">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span><span class="sxs-lookup"><span data-stu-id="de69a-269">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="de69a-270">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span><span class="sxs-lookup"><span data-stu-id="de69a-270">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="de69a-271">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span><span class="sxs-lookup"><span data-stu-id="de69a-271">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="de69a-272">This sharing prevents socket exhaustion.</span><span class="sxs-lookup"><span data-stu-id="de69a-272">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="de69a-273">The `SocketsHttpHandler ` cycles connections according to `PooledConnectionLifetime` to avoid state-DNS problems.</span><span class="sxs-lookup"><span data-stu-id="de69a-273">The `SocketsHttpHandler ` cycles connections according to `PooledConnectionLifetime` to avoid state-DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="de69a-274">Cookies</span><span class="sxs-lookup"><span data-stu-id="de69a-274">Cookies</span></span>

<span data-ttu-id="de69a-275">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span><span class="sxs-lookup"><span data-stu-id="de69a-275">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="de69a-276">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span><span class="sxs-lookup"><span data-stu-id="de69a-276">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="de69a-277">For apps that require cookies, consider either:</span><span class="sxs-lookup"><span data-stu-id="de69a-277">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="de69a-278">Disabling automatic cookie handling</span><span class="sxs-lookup"><span data-stu-id="de69a-278">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="de69a-279">Avoiding `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="de69a-279">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="de69a-280">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span><span class="sxs-lookup"><span data-stu-id="de69a-280">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="de69a-281">Journalisation</span><span class="sxs-lookup"><span data-stu-id="de69a-281">Logging</span></span>

<span data-ttu-id="de69a-282">Les clients créés via `IHttpClientFactory` enregistrent les messages de journalisation pour toutes les requêtes.</span><span class="sxs-lookup"><span data-stu-id="de69a-282">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="de69a-283">Enable the appropriate information level in the logging configuration to see the default log messages.</span><span class="sxs-lookup"><span data-stu-id="de69a-283">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="de69a-284">Une journalisation supplémentaire, comme celle des en-têtes des requêtes, est incluse seulement au niveau de trace.</span><span class="sxs-lookup"><span data-stu-id="de69a-284">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="de69a-285">La catégorie de journal utilisée pour chaque client comprend le nom du client.</span><span class="sxs-lookup"><span data-stu-id="de69a-285">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="de69a-286">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span><span class="sxs-lookup"><span data-stu-id="de69a-286">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="de69a-287">Les messages avec le suffixe *LogicalHandler* se produisent en dehors du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="de69a-287">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="de69a-288">Lors d’une requête, les messages sont journalisés avant que d’autres gestionnaires du pipeline l’aient traitée.</span><span class="sxs-lookup"><span data-stu-id="de69a-288">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="de69a-289">Lors d’une réponse, les messages sont journalisés une fois que tous les autres gestionnaires du pipeline ont reçu la réponse.</span><span class="sxs-lookup"><span data-stu-id="de69a-289">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="de69a-290">La journalisation se produit également à l’intérieur du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="de69a-290">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="de69a-291">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span><span class="sxs-lookup"><span data-stu-id="de69a-291">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="de69a-292">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span><span class="sxs-lookup"><span data-stu-id="de69a-292">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="de69a-293">Lors de la réponse, cette journalisation inclut l’état de la réponse avant qu’elle repasse à travers le pipeline de gestionnaires.</span><span class="sxs-lookup"><span data-stu-id="de69a-293">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="de69a-294">L’activation de la journalisation à l’extérieur et à l’intérieur du pipeline permet l’inspection des changements apportés par les autres gestionnaires du pipeline.</span><span class="sxs-lookup"><span data-stu-id="de69a-294">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="de69a-295">This may include changes to request headers or to the response status code.</span><span class="sxs-lookup"><span data-stu-id="de69a-295">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="de69a-296">Including the name of the client in the log category enables log filtering for specific named clients.</span><span class="sxs-lookup"><span data-stu-id="de69a-296">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="de69a-297">Configurer le HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="de69a-297">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="de69a-298">Il peut être nécessaire de contrôler la configuration du `HttpMessageHandler` interne utilisé par un client.</span><span class="sxs-lookup"><span data-stu-id="de69a-298">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="de69a-299">Un `IHttpClientBuilder` est retourné quand vous ajoutez des clients nommés ou typés.</span><span class="sxs-lookup"><span data-stu-id="de69a-299">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="de69a-300">La méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> peut être utilisée pour définir un délégué.</span><span class="sxs-lookup"><span data-stu-id="de69a-300">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="de69a-301">Le délégué est utilisé pour créer et configurer le `HttpMessageHandler` principal utilisé par ce client :</span><span class="sxs-lookup"><span data-stu-id="de69a-301">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="de69a-302">Utiliser IHttpClientFactory dans une application console</span><span class="sxs-lookup"><span data-stu-id="de69a-302">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="de69a-303">Dans une application console, ajoutez les références de package suivantes au projet :</span><span class="sxs-lookup"><span data-stu-id="de69a-303">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="de69a-304">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="de69a-304">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="de69a-305">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="de69a-305">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="de69a-306">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="de69a-306">In the following example:</span></span>

* <span data-ttu-id="de69a-307"><xref:System.Net.Http.IHttpClientFactory> est inscrit dans le conteneur de service dans de l’[hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="de69a-307"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="de69a-308">`MyService` crée une instance de fabrique cliente à partir du service, qui est utilisée pour créer un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="de69a-308">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="de69a-309">`HttpClient` est utilisé pour récupérer une page web.</span><span class="sxs-lookup"><span data-stu-id="de69a-309">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="de69a-310">`Main` crée une étendue pour exécuter la méthode `GetPage` du service et écrire les 500 premiers caractères du contenu de la page web dans la console.</span><span class="sxs-lookup"><span data-stu-id="de69a-310">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="de69a-311">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="de69a-311">Additional resources</span></span>

* [<span data-ttu-id="de69a-312">Utiliser HttpClientFactory pour implémenter des requêtes HTTP résilientes</span><span class="sxs-lookup"><span data-stu-id="de69a-312">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="de69a-313">Implémenter de nouvelles tentatives d’appel HTTP avec interruption exponentielle avec des stratégies Polly et HttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="de69a-313">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="de69a-314">Implémenter le modèle Disjoncteur</span><span class="sxs-lookup"><span data-stu-id="de69a-314">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="de69a-315">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) et [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="de69a-315">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="de69a-316">Une <xref:System.Net.Http.IHttpClientFactory> peut être inscrite et utilisée pour configurer et créer des instances de <xref:System.Net.Http.HttpClient> dans une application.</span><span class="sxs-lookup"><span data-stu-id="de69a-316">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="de69a-317">Elle offre les avantages suivants :</span><span class="sxs-lookup"><span data-stu-id="de69a-317">It offers the following benefits:</span></span>

* <span data-ttu-id="de69a-318">Fournit un emplacement central pour le nommage et la configuration d’instance de `HttpClient` logiques.</span><span class="sxs-lookup"><span data-stu-id="de69a-318">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="de69a-319">Par exemple, un client *github* peut être inscrit et configuré pour accéder à [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="de69a-319">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="de69a-320">Un client par défaut peut être inscrit à d’autres fins.</span><span class="sxs-lookup"><span data-stu-id="de69a-320">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="de69a-321">Codifie le concept de middleware (intergiciel) sortant via la délégation de gestionnaires dans `HttpClient` et fournit des extensions permettant au middleware Polly d’en tirer parti.</span><span class="sxs-lookup"><span data-stu-id="de69a-321">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="de69a-322">Gère le regroupement et la durée de vie des instances de `HttpClientMessageHandler` sous-jacentes pour éviter les problèmes DNS courants qui se produisent lors de la gestion manuelle des durées de vie de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="de69a-322">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="de69a-323">Ajoute une expérience de journalisation configurable (via `ILogger`) pour toutes les requêtes envoyées via des clients créés par la fabrique.</span><span class="sxs-lookup"><span data-stu-id="de69a-323">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="de69a-324">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="de69a-324">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="de69a-325">Modèles de consommation</span><span class="sxs-lookup"><span data-stu-id="de69a-325">Consumption patterns</span></span>

<span data-ttu-id="de69a-326">Vous pouvez utiliser `IHttpClientFactory` dans une application de plusieurs façons :</span><span class="sxs-lookup"><span data-stu-id="de69a-326">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="de69a-327">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="de69a-327">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="de69a-328">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="de69a-328">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="de69a-329">Clients typés</span><span class="sxs-lookup"><span data-stu-id="de69a-329">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="de69a-330">Clients générés</span><span class="sxs-lookup"><span data-stu-id="de69a-330">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="de69a-331">Aucune d’entre elles n’est meilleure qu’une autre.</span><span class="sxs-lookup"><span data-stu-id="de69a-331">None of them are strictly superior to another.</span></span> <span data-ttu-id="de69a-332">La meilleure approche dépend des contraintes de l’application.</span><span class="sxs-lookup"><span data-stu-id="de69a-332">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="de69a-333">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="de69a-333">Basic usage</span></span>

<span data-ttu-id="de69a-334">Vous pouvez inscrire la `IHttpClientFactory` en appelant la méthode d’extension `AddHttpClient` sur la `IServiceCollection`, à l’intérieur la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="de69a-334">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="de69a-335">Une fois inscrit, le code peut accepter un `IHttpClientFactory` partout où des services peuvent être injectés avec [une injection de dépendance](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="de69a-335">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="de69a-336">La `IHttpClientFactory` peut être utilisée pour créer une instance de `HttpClient` :</span><span class="sxs-lookup"><span data-stu-id="de69a-336">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="de69a-337">L’utilisation de `IHttpClientFactory` de cette façon est un excellent moyen de refactoriser une application existante.</span><span class="sxs-lookup"><span data-stu-id="de69a-337">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="de69a-338">Elle n’a aucun impact sur la façon dont `HttpClient` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="de69a-338">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="de69a-339">Dans les endroits où les instances de `HttpClient` sont actuellement créées, remplacez ces occurrences par un appel à <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="de69a-339">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="de69a-340">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="de69a-340">Named clients</span></span>

<span data-ttu-id="de69a-341">Si une application nécessite plusieurs utilisations distinctes de `HttpClient`, chacune avec une configuration différente, une option consiste à utiliser des **clients nommés**.</span><span class="sxs-lookup"><span data-stu-id="de69a-341">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="de69a-342">La configuration d’un `HttpClient` nommé peut être spécifiée lors de l’inscription dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="de69a-342">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="de69a-343">Dans le code précédent, `AddHttpClient` est appelé en fournissant le nom *github*.</span><span class="sxs-lookup"><span data-stu-id="de69a-343">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="de69a-344">Une configuration par défaut est appliquée à ce client : l’adresse de base et deux en-têtes nécessaires pour utiliser l’API GitHub.</span><span class="sxs-lookup"><span data-stu-id="de69a-344">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="de69a-345">Chaque fois que `CreateClient` est appelée, une nouvelle instance de `HttpClient` est créée et l’action de configuration est appelée.</span><span class="sxs-lookup"><span data-stu-id="de69a-345">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="de69a-346">Pour utiliser un client nommé, un paramètre de chaîne peut être passé à `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="de69a-346">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="de69a-347">Spécifiez le nom du client à créer :</span><span class="sxs-lookup"><span data-stu-id="de69a-347">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="de69a-348">Dans le code précédent, la requête n’a pas besoin de spécifier un nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="de69a-348">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="de69a-349">Elle peut simplement passer le chemin, car l’adresse de base configurée pour le client est utilisée.</span><span class="sxs-lookup"><span data-stu-id="de69a-349">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="de69a-350">Clients typés</span><span class="sxs-lookup"><span data-stu-id="de69a-350">Typed clients</span></span>

<span data-ttu-id="de69a-351">Clients typés :</span><span class="sxs-lookup"><span data-stu-id="de69a-351">Typed clients:</span></span>

* <span data-ttu-id="de69a-352">Fournissent les mêmes fonctionnalités que les clients nommés, sans qu’il soit nécessaire d’utiliser des chaînes comme clés.</span><span class="sxs-lookup"><span data-stu-id="de69a-352">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="de69a-353">Bénéficie de l’aide d’IntelliSense et du compilateur lors de l’utilisation des clients.</span><span class="sxs-lookup"><span data-stu-id="de69a-353">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="de69a-354">Fournissent un emplacement unique pour configurer et interagir avec un `HttpClient` particulier.</span><span class="sxs-lookup"><span data-stu-id="de69a-354">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="de69a-355">Par exemple, un même client typé peut être utilisé pour un point de terminaison de backend et pour encapsuler la logique relative à ce point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="de69a-355">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="de69a-356">Fonctionnent avec l’injection de dépendances et peuvent être injectés là où c’est nécessaire dans votre application.</span><span class="sxs-lookup"><span data-stu-id="de69a-356">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="de69a-357">Un client typé accepte un `HttpClient` dans son constructeur :</span><span class="sxs-lookup"><span data-stu-id="de69a-357">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="de69a-358">Dans le code précédent, la configuration est déplacée dans le client typé.</span><span class="sxs-lookup"><span data-stu-id="de69a-358">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="de69a-359">L’objet `HttpClient` est exposé en tant que propriété publique.</span><span class="sxs-lookup"><span data-stu-id="de69a-359">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="de69a-360">Il est possible de définir des méthodes d’API spécifiques qui exposent les fonctionnalités de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="de69a-360">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="de69a-361">La méthode `GetAspNetDocsIssues` encapsule le code nécessaire pour interroger et analyser les problèmes ouverts les plus récents d’un dépôt GitHub.</span><span class="sxs-lookup"><span data-stu-id="de69a-361">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="de69a-362">Pour inscrire un client typé, la méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> générique peut être utilisée dans `Startup.ConfigureServices`, en spécifiant la classe du client typé :</span><span class="sxs-lookup"><span data-stu-id="de69a-362">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="de69a-363">Le client typé est inscrit comme étant transitoire avec injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="de69a-363">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="de69a-364">Le client typé peut être injecté et utilisé directement :</span><span class="sxs-lookup"><span data-stu-id="de69a-364">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="de69a-365">Si vous préférez, vous pouvez spécifier la configuration d’un client typé lors de l’inscription dans `Startup.ConfigureServices` au lieu de le faire dans le constructeur du client typé :</span><span class="sxs-lookup"><span data-stu-id="de69a-365">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="de69a-366">Il est possible d’encapsuler entièrement le `HttpClient` dans un client typé.</span><span class="sxs-lookup"><span data-stu-id="de69a-366">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="de69a-367">Au lieu de l’exposer en tant que propriété, vous pouvez fournir des méthodes publiques qui appellent l’instance de `HttpClient` en interne.</span><span class="sxs-lookup"><span data-stu-id="de69a-367">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="de69a-368">Dans le code précédent, le `HttpClient` est stocké en tant que champ privé.</span><span class="sxs-lookup"><span data-stu-id="de69a-368">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="de69a-369">Tous les accès nécessaires pour effectuer des appels externes passent par la méthode `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="de69a-369">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="de69a-370">Clients générés</span><span class="sxs-lookup"><span data-stu-id="de69a-370">Generated clients</span></span>

<span data-ttu-id="de69a-371">`IHttpClientFactory` peut être utilisé en combinaison avec d’autres bibliothèques tierces, comme [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="de69a-371">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="de69a-372">Refit est une bibliothèque REST pour .NET.</span><span class="sxs-lookup"><span data-stu-id="de69a-372">Refit is a REST library for .NET.</span></span> <span data-ttu-id="de69a-373">Il convertit les API REST en interfaces dynamiques.</span><span class="sxs-lookup"><span data-stu-id="de69a-373">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="de69a-374">Une implémentation de l’interface est générée dynamiquement par le `RestService`, avec `HttpClient` pour faire les appels HTTP externes.</span><span class="sxs-lookup"><span data-stu-id="de69a-374">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="de69a-375">Une interface et une réponse sont définies pour représenter l’API externe et sa réponse :</span><span class="sxs-lookup"><span data-stu-id="de69a-375">An interface and a reply are defined to represent the external API and its response:</span></span>

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

<span data-ttu-id="de69a-376">Vous pouvez ajouter un client typé en utilisant Refit pour générer l’implémentation :</span><span class="sxs-lookup"><span data-stu-id="de69a-376">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("https://localhost:5001");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="de69a-377">L’interface définie peut être utilisée quand c’est nécessaire, avec l’implémentation fournie par l’injection de dépendances et Refit :</span><span class="sxs-lookup"><span data-stu-id="de69a-377">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a><span data-ttu-id="de69a-378">Middleware pour les requêtes sortantes</span><span class="sxs-lookup"><span data-stu-id="de69a-378">Outgoing request middleware</span></span>

<span data-ttu-id="de69a-379">`HttpClient` intègre déjà le concept de délégation des gestionnaires qui peuvent être liés ensemble pour les requêtes HTTP sortantes.</span><span class="sxs-lookup"><span data-stu-id="de69a-379">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="de69a-380">Le `IHttpClientFactory` permet de définir facilement les gestionnaires à appliquer pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="de69a-380">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="de69a-381">Il prend en charge l’inscription et le chaînage de plusieurs gestionnaires pour créer un pipeline de middlewares pour les requêtes sortantes.</span><span class="sxs-lookup"><span data-stu-id="de69a-381">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="de69a-382">Chacun de ces gestionnaires peut effectuer un travail avant et après la requête sortante.</span><span class="sxs-lookup"><span data-stu-id="de69a-382">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="de69a-383">Ce modèle est similaire au pipeline de middlewares entrants dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="de69a-383">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="de69a-384">Le modèle fournit un mécanisme permettant de gérer les problèmes transversaux liés aux des requêtes HTTP, notamment la mise en cache, la gestion des erreurs, la sérialisation et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="de69a-384">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="de69a-385">Pour créer un gestionnaire, définissez une classe dérivant de <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="de69a-385">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="de69a-386">Remplacez la méthode `SendAsync` de façon à exécuter du code avant de passer la requête au gestionnaire suivant dans le pipeline :</span><span class="sxs-lookup"><span data-stu-id="de69a-386">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="de69a-387">Le code précédent définit un gestionnaire de base.</span><span class="sxs-lookup"><span data-stu-id="de69a-387">The preceding code defines a basic handler.</span></span> <span data-ttu-id="de69a-388">Il vérifie si un en-tête `X-API-KEY` a été inclus dans la requête.</span><span class="sxs-lookup"><span data-stu-id="de69a-388">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="de69a-389">Si l’en-tête est manquant, il peut éviter l’appel HTTP et retourner une réponse appropriée.</span><span class="sxs-lookup"><span data-stu-id="de69a-389">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="de69a-390">Lors de l’inscription, un ou plusieurs gestionnaires peuvent être ajoutés à la configuration pour un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="de69a-390">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="de69a-391">Cette tâche est accomplie via des méthodes d’extension sur le <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="de69a-391">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="de69a-392">Dans le code précédent, le `ValidateHeaderHandler` est inscrit avec une injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="de69a-392">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="de69a-393">`IHttpClientFactory` crée une étendue DI distincte pour chaque gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="de69a-393">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="de69a-394">Les gestionnaires peuvent librement dépendre des services de n’importe quelle étendue.</span><span class="sxs-lookup"><span data-stu-id="de69a-394">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="de69a-395">Les services dont dépendent les gestionnaires sont supprimés lorsque le gestionnaire est supprimé.</span><span class="sxs-lookup"><span data-stu-id="de69a-395">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="de69a-396">Une fois inscrit, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> peut être appelé en passant en entrée le type pour le gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="de69a-396">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="de69a-397">Vous pouvez inscrire plusieurs gestionnaires dans l’ordre où ils doivent être exécutés.</span><span class="sxs-lookup"><span data-stu-id="de69a-397">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="de69a-398">Chaque gestionnaire wrappe le gestionnaire suivant jusqu’à ce que le dernier `HttpClientHandler` exécute la requête :</span><span class="sxs-lookup"><span data-stu-id="de69a-398">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="de69a-399">Utilisez l’une des approches suivantes pour partager l’état de chaque requête avec les gestionnaires de messages :</span><span class="sxs-lookup"><span data-stu-id="de69a-399">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="de69a-400">Passez des données dans le gestionnaire en utilisant `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="de69a-400">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="de69a-401">Utilisez `IHttpContextAccessor` pour accéder à la requête en cours.</span><span class="sxs-lookup"><span data-stu-id="de69a-401">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="de69a-402">Créez un objet de stockage `AsyncLocal` personnalisé pour passer les données.</span><span class="sxs-lookup"><span data-stu-id="de69a-402">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="de69a-403">Utiliser les gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="de69a-403">Use Polly-based handlers</span></span>

<span data-ttu-id="de69a-404">`IHttpClientFactory` s’intègre à une bibliothèque tierce très utilisée, appelée [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="de69a-404">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="de69a-405">Polly est une bibliothèque complète de gestion des erreurs transitoires et de résilience pour .NET.</span><span class="sxs-lookup"><span data-stu-id="de69a-405">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="de69a-406">Elle permet aux développeurs de formuler facilement et de façon thread-safe des stratégies, comme Retry (Nouvelle tentative), Circuit Breaker (Disjoncteur), Timeout (Délai d’attente), Bulkhead Isolation (Isolation par cloisonnement) et Fallback (Alternative de repli).</span><span class="sxs-lookup"><span data-stu-id="de69a-406">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="de69a-407">Des méthodes d’extension sont fournies pour permettre l’utilisation de stratégies Polly avec les instances configurées de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="de69a-407">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="de69a-408">Les extensions Polly :</span><span class="sxs-lookup"><span data-stu-id="de69a-408">The Polly extensions:</span></span>

* <span data-ttu-id="de69a-409">Prennent en charge l’ajout de gestionnaires Polly à des clients.</span><span class="sxs-lookup"><span data-stu-id="de69a-409">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="de69a-410">Peuvent être utilisées après l’installation du package NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="de69a-410">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="de69a-411">Ce package n’est pas inclus dans le framework partagé ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="de69a-411">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="de69a-412">Gérer les erreurs temporaires</span><span class="sxs-lookup"><span data-stu-id="de69a-412">Handle transient faults</span></span>

<span data-ttu-id="de69a-413">Les erreurs courantes se produisent lorsque des appels HTTP externes sont temporaires.</span><span class="sxs-lookup"><span data-stu-id="de69a-413">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="de69a-414">Une méthode d’extension pratique nommée `AddTransientHttpErrorPolicy` est incluse : elle permet de définir une stratégie pour gérer les erreurs temporaires.</span><span class="sxs-lookup"><span data-stu-id="de69a-414">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="de69a-415">Les stratégies configurées avec cette méthode d’extension gèrent `HttpRequestException`, les réponses HTTP 5xx et les réponses HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="de69a-415">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="de69a-416">L’extension `AddTransientHttpErrorPolicy` peut être utilisée dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="de69a-416">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="de69a-417">L’extension fournit l’accès à un objet `PolicyBuilder` configuré pour gérer les erreurs représentant une erreur temporaire possible :</span><span class="sxs-lookup"><span data-stu-id="de69a-417">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="de69a-418">Dans le code précédent, une stratégie `WaitAndRetryAsync` est définie.</span><span class="sxs-lookup"><span data-stu-id="de69a-418">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="de69a-419">Les requêtes qui ont échoué sont retentées jusqu’à trois fois avec un délai de 600 ms entre les tentatives.</span><span class="sxs-lookup"><span data-stu-id="de69a-419">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="de69a-420">Sélectionner dynamiquement des stratégies</span><span class="sxs-lookup"><span data-stu-id="de69a-420">Dynamically select policies</span></span>

<span data-ttu-id="de69a-421">Il existe d’autres méthodes d’extension que vous pouvez utiliser pour ajouter des gestionnaires Polly.</span><span class="sxs-lookup"><span data-stu-id="de69a-421">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="de69a-422">Une de ces extensions est `AddPolicyHandler`, qui a plusieurs surcharges.</span><span class="sxs-lookup"><span data-stu-id="de69a-422">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="de69a-423">Une de ces surcharges permet l’inspection de la requête lors de la définition de la stratégie à appliquer :</span><span class="sxs-lookup"><span data-stu-id="de69a-423">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="de69a-424">Dans le code précédent, si la requête sortante est un HTTP GET, un délai d’attente de 10 secondes est appliqué.</span><span class="sxs-lookup"><span data-stu-id="de69a-424">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="de69a-425">Pour toutes les autres méthodes HTTP, un délai d’attente de 30 secondes est utilisé.</span><span class="sxs-lookup"><span data-stu-id="de69a-425">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="de69a-426">Ajouter plusieurs gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="de69a-426">Add multiple Polly handlers</span></span>

<span data-ttu-id="de69a-427">Il est courant d’imbriquer des stratégies Polly pour fournir des fonctionnalités améliorées :</span><span class="sxs-lookup"><span data-stu-id="de69a-427">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="de69a-428">Dans l’exemple précédent, deux gestionnaires sont ajoutés.</span><span class="sxs-lookup"><span data-stu-id="de69a-428">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="de69a-429">Le premier utilise l’extension `AddTransientHttpErrorPolicy` pour ajouter une stratégie de nouvelle tentative.</span><span class="sxs-lookup"><span data-stu-id="de69a-429">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="de69a-430">Les requêtes qui ont échoué sont retentées jusqu’à trois fois.</span><span class="sxs-lookup"><span data-stu-id="de69a-430">Failed requests are retried up to three times.</span></span> <span data-ttu-id="de69a-431">Le deuxième appel à `AddTransientHttpErrorPolicy` ajoute une stratégie de disjoncteur.</span><span class="sxs-lookup"><span data-stu-id="de69a-431">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="de69a-432">Les requêtes externes supplémentaires sont bloquées pendant 30 secondes si cinq tentatives successives échouent.</span><span class="sxs-lookup"><span data-stu-id="de69a-432">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="de69a-433">Les stratégies de disjoncteur sont avec état.</span><span class="sxs-lookup"><span data-stu-id="de69a-433">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="de69a-434">Tous les appels effectués via ce client partagent le même état du circuit.</span><span class="sxs-lookup"><span data-stu-id="de69a-434">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="de69a-435">Ajouter des stratégies à partir du Registre Polly</span><span class="sxs-lookup"><span data-stu-id="de69a-435">Add policies from the Polly registry</span></span>

<span data-ttu-id="de69a-436">Une approche de la gestion des stratégies régulièrement utilisées consiste à les définir une seule fois et à les inscrire avec un `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="de69a-436">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="de69a-437">Il existe une méthode d’extension qui permet l’ajout d’un gestionnaire avec une stratégie du Registre :</span><span class="sxs-lookup"><span data-stu-id="de69a-437">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="de69a-438">Dans le code précédent, deux stratégies sont inscrites lorsque `PolicyRegistry` est ajouté à `ServiceCollection`.</span><span class="sxs-lookup"><span data-stu-id="de69a-438">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="de69a-439">Pour utiliser une stratégie du Registre, la méthode `AddPolicyHandlerFromRegistry` est utilisée, en passant le nom de la stratégie à appliquer.</span><span class="sxs-lookup"><span data-stu-id="de69a-439">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="de69a-440">Vous trouverez plus d’informations sur les intégrations de `IHttpClientFactory` et de Polly dans le [wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="de69a-440">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="de69a-441">HttpClient et gestion de la durée de vie</span><span class="sxs-lookup"><span data-stu-id="de69a-441">HttpClient and lifetime management</span></span>

<span data-ttu-id="de69a-442">Une nouvelle instance `HttpClient` est retournée à chaque fois que `CreateClient` est appelé sur `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="de69a-442">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="de69a-443">Il existe un <xref:System.Net.Http.HttpMessageHandler> par client nommé.</span><span class="sxs-lookup"><span data-stu-id="de69a-443">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="de69a-444">La fabrique gère les durées de vie des instances `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="de69a-444">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="de69a-445">`IHttpClientFactory` regroupe dans un pool les instances de `HttpMessageHandler` créées par la fabrique pour réduire la consommation des ressources.</span><span class="sxs-lookup"><span data-stu-id="de69a-445">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="de69a-446">Une instance de `HttpMessageHandler` peut être réutilisée à partir du pool quand vous créez une instance de `HttpClient` si sa durée de vie n’a pas expiré.</span><span class="sxs-lookup"><span data-stu-id="de69a-446">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="de69a-447">Le regroupement de gestionnaires en pools est souhaitable, car chaque gestionnaire gère habituellement ses propres connexions HTTP sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="de69a-447">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="de69a-448">La création de plus de gestionnaires que nécessaire peut entraîner des délais de connexion.</span><span class="sxs-lookup"><span data-stu-id="de69a-448">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="de69a-449">Certains gestionnaires conservent aussi les connexions indéfiniment ouvertes, ce qui peut empêcher le gestionnaire de réagir aux changements du DNS.</span><span class="sxs-lookup"><span data-stu-id="de69a-449">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="de69a-450">La durée de vie par défaut d’un gestionnaire est de deux minutes.</span><span class="sxs-lookup"><span data-stu-id="de69a-450">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="de69a-451">La valeur par défaut peut être remplacée pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="de69a-451">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="de69a-452">Pour la remplacer, appelez <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> sur le `IHttpClientBuilder` qui est retourné lors de la création du client :</span><span class="sxs-lookup"><span data-stu-id="de69a-452">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="de69a-453">La suppression du client n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="de69a-453">Disposal of the client isn't required.</span></span> <span data-ttu-id="de69a-454">La suppression annule les requêtes sortantes et garantit que l’instance `HttpClient` donnée ne peut pas être utilisée après avoir appelé <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="de69a-454">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="de69a-455">`IHttpClientFactory` effectue le suivi et libère les ressources utilisées par les instances `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="de69a-455">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="de69a-456">Les instances `HttpClient` peuvent généralement être traitées en tant qu’objets .NET ne nécessitant pas une suppression.</span><span class="sxs-lookup"><span data-stu-id="de69a-456">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="de69a-457">Le fait de conserver une seule instance `HttpClient` active pendant une longue durée est un modèle commun utilisé avant le lancement de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="de69a-457">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="de69a-458">Ce modèle devient inutile après la migration vers `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="de69a-458">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="de69a-459">Alternatives to IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="de69a-459">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="de69a-460">Using `IHttpClientFactory` in a DI-enabled app avoids:</span><span class="sxs-lookup"><span data-stu-id="de69a-460">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="de69a-461">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span><span class="sxs-lookup"><span data-stu-id="de69a-461">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="de69a-462">Stale-DNS problems by cycling `HttpMessageHandler` instances at regular instances.</span><span class="sxs-lookup"><span data-stu-id="de69a-462">Stale-DNS problems by cycling `HttpMessageHandler` instances at regular instances.</span></span>

<span data-ttu-id="de69a-463">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span><span class="sxs-lookup"><span data-stu-id="de69a-463">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="de69a-464">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span><span class="sxs-lookup"><span data-stu-id="de69a-464">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="de69a-465">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span><span class="sxs-lookup"><span data-stu-id="de69a-465">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="de69a-466">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span><span class="sxs-lookup"><span data-stu-id="de69a-466">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="de69a-467">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span><span class="sxs-lookup"><span data-stu-id="de69a-467">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="de69a-468">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span><span class="sxs-lookup"><span data-stu-id="de69a-468">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="de69a-469">This sharing prevents socket exhaustion.</span><span class="sxs-lookup"><span data-stu-id="de69a-469">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="de69a-470">The `SocketsHttpHandler ` cycles connections according to `PooledConnectionLifetime` to avoid state-DNS problems.</span><span class="sxs-lookup"><span data-stu-id="de69a-470">The `SocketsHttpHandler ` cycles connections according to `PooledConnectionLifetime` to avoid state-DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="de69a-471">Cookies</span><span class="sxs-lookup"><span data-stu-id="de69a-471">Cookies</span></span>

<span data-ttu-id="de69a-472">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span><span class="sxs-lookup"><span data-stu-id="de69a-472">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="de69a-473">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span><span class="sxs-lookup"><span data-stu-id="de69a-473">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="de69a-474">For apps that require cookies, consider either:</span><span class="sxs-lookup"><span data-stu-id="de69a-474">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="de69a-475">Disabling automatic cookie handling</span><span class="sxs-lookup"><span data-stu-id="de69a-475">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="de69a-476">Avoiding `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="de69a-476">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="de69a-477">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span><span class="sxs-lookup"><span data-stu-id="de69a-477">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="de69a-478">Journalisation</span><span class="sxs-lookup"><span data-stu-id="de69a-478">Logging</span></span>

<span data-ttu-id="de69a-479">Les clients créés via `IHttpClientFactory` enregistrent les messages de journalisation pour toutes les requêtes.</span><span class="sxs-lookup"><span data-stu-id="de69a-479">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="de69a-480">Activez le niveau d’informations approprié dans votre configuration de journalisation pour voir les messages de journalisation par défaut.</span><span class="sxs-lookup"><span data-stu-id="de69a-480">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="de69a-481">Une journalisation supplémentaire, comme celle des en-têtes des requêtes, est incluse seulement au niveau de trace.</span><span class="sxs-lookup"><span data-stu-id="de69a-481">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="de69a-482">La catégorie de journal utilisée pour chaque client comprend le nom du client.</span><span class="sxs-lookup"><span data-stu-id="de69a-482">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="de69a-483">Par exemple, un client nommé *MyNamedClient* journalise les messages avec la catégorie `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="de69a-483">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="de69a-484">Les messages avec le suffixe *LogicalHandler* se produisent en dehors du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="de69a-484">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="de69a-485">Lors d’une requête, les messages sont journalisés avant que d’autres gestionnaires du pipeline l’aient traitée.</span><span class="sxs-lookup"><span data-stu-id="de69a-485">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="de69a-486">Lors d’une réponse, les messages sont journalisés une fois que tous les autres gestionnaires du pipeline ont reçu la réponse.</span><span class="sxs-lookup"><span data-stu-id="de69a-486">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="de69a-487">La journalisation se produit également à l’intérieur du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="de69a-487">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="de69a-488">Dans l’exemple *MyNamedClient*, ces messages sont journalisés avec la catégorie de journalisation `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="de69a-488">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="de69a-489">Pour la requête, cela se produit après que tous les autres gestionnaires ont été exécutés et immédiatement avant l’envoi de la requête sur le réseau.</span><span class="sxs-lookup"><span data-stu-id="de69a-489">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="de69a-490">Lors de la réponse, cette journalisation inclut l’état de la réponse avant qu’elle repasse à travers le pipeline de gestionnaires.</span><span class="sxs-lookup"><span data-stu-id="de69a-490">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="de69a-491">L’activation de la journalisation à l’extérieur et à l’intérieur du pipeline permet l’inspection des changements apportés par les autres gestionnaires du pipeline.</span><span class="sxs-lookup"><span data-stu-id="de69a-491">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="de69a-492">Par exemple, cela peut comprendre des changements apportés aux en-têtes des requêtes ou au code d’état de la réponse.</span><span class="sxs-lookup"><span data-stu-id="de69a-492">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="de69a-493">L’ajout du nom du client dans la catégorie de journalisation permet si nécessaire de filtrer le journal pour des clients nommés spécifiques.</span><span class="sxs-lookup"><span data-stu-id="de69a-493">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="de69a-494">Configurer le HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="de69a-494">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="de69a-495">Il peut être nécessaire de contrôler la configuration du `HttpMessageHandler` interne utilisé par un client.</span><span class="sxs-lookup"><span data-stu-id="de69a-495">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="de69a-496">Un `IHttpClientBuilder` est retourné quand vous ajoutez des clients nommés ou typés.</span><span class="sxs-lookup"><span data-stu-id="de69a-496">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="de69a-497">La méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> peut être utilisée pour définir un délégué.</span><span class="sxs-lookup"><span data-stu-id="de69a-497">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="de69a-498">Le délégué est utilisé pour créer et configurer le `HttpMessageHandler` principal utilisé par ce client :</span><span class="sxs-lookup"><span data-stu-id="de69a-498">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="de69a-499">Utiliser IHttpClientFactory dans une application console</span><span class="sxs-lookup"><span data-stu-id="de69a-499">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="de69a-500">Dans une application console, ajoutez les références de package suivantes au projet :</span><span class="sxs-lookup"><span data-stu-id="de69a-500">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="de69a-501">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="de69a-501">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="de69a-502">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="de69a-502">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="de69a-503">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="de69a-503">In the following example:</span></span>

* <span data-ttu-id="de69a-504"><xref:System.Net.Http.IHttpClientFactory> est inscrit dans le conteneur de service dans de l’[hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="de69a-504"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="de69a-505">`MyService` crée une instance de fabrique cliente à partir du service, qui est utilisée pour créer un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="de69a-505">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="de69a-506">`HttpClient` est utilisé pour récupérer une page web.</span><span class="sxs-lookup"><span data-stu-id="de69a-506">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="de69a-507">`Main` crée une étendue pour exécuter la méthode `GetPage` du service et écrire les 500 premiers caractères du contenu de la page web dans la console.</span><span class="sxs-lookup"><span data-stu-id="de69a-507">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="de69a-508">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="de69a-508">Additional resources</span></span>

* [<span data-ttu-id="de69a-509">Utiliser HttpClientFactory pour implémenter des requêtes HTTP résilientes</span><span class="sxs-lookup"><span data-stu-id="de69a-509">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="de69a-510">Implémenter de nouvelles tentatives d’appel HTTP avec interruption exponentielle avec des stratégies Polly et HttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="de69a-510">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="de69a-511">Implémenter le modèle Disjoncteur</span><span class="sxs-lookup"><span data-stu-id="de69a-511">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<span data-ttu-id="de69a-512">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) et [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="de69a-512">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="de69a-513">Une <xref:System.Net.Http.IHttpClientFactory> peut être inscrite et utilisée pour configurer et créer des instances de <xref:System.Net.Http.HttpClient> dans une application.</span><span class="sxs-lookup"><span data-stu-id="de69a-513">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="de69a-514">Elle offre les avantages suivants :</span><span class="sxs-lookup"><span data-stu-id="de69a-514">It offers the following benefits:</span></span>

* <span data-ttu-id="de69a-515">Fournit un emplacement central pour le nommage et la configuration d’instance de `HttpClient` logiques.</span><span class="sxs-lookup"><span data-stu-id="de69a-515">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="de69a-516">Par exemple, un client *github* peut être inscrit et configuré pour accéder à [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="de69a-516">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="de69a-517">Un client par défaut peut être inscrit à d’autres fins.</span><span class="sxs-lookup"><span data-stu-id="de69a-517">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="de69a-518">Codifie le concept de middleware (intergiciel) sortant via la délégation de gestionnaires dans `HttpClient` et fournit des extensions permettant au middleware Polly d’en tirer parti.</span><span class="sxs-lookup"><span data-stu-id="de69a-518">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="de69a-519">Gère le regroupement et la durée de vie des instances de `HttpClientMessageHandler` sous-jacentes pour éviter les problèmes DNS courants qui se produisent lors de la gestion manuelle des durées de vie de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="de69a-519">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="de69a-520">Ajoute une expérience de journalisation configurable (via `ILogger`) pour toutes les requêtes envoyées via des clients créés par la fabrique.</span><span class="sxs-lookup"><span data-stu-id="de69a-520">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="de69a-521">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="de69a-521">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="de69a-522">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="de69a-522">Prerequisites</span></span>

<span data-ttu-id="de69a-523">Les projets ciblant .NET Framework nécessitent l’installation du package NuGet [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/).</span><span class="sxs-lookup"><span data-stu-id="de69a-523">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="de69a-524">Les projets qui ciblent .NET Core et référencent le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) incluent déjà le package `Microsoft.Extensions.Http`.</span><span class="sxs-lookup"><span data-stu-id="de69a-524">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="de69a-525">Modèles de consommation</span><span class="sxs-lookup"><span data-stu-id="de69a-525">Consumption patterns</span></span>

<span data-ttu-id="de69a-526">Vous pouvez utiliser `IHttpClientFactory` dans une application de plusieurs façons :</span><span class="sxs-lookup"><span data-stu-id="de69a-526">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="de69a-527">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="de69a-527">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="de69a-528">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="de69a-528">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="de69a-529">Clients typés</span><span class="sxs-lookup"><span data-stu-id="de69a-529">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="de69a-530">Clients générés</span><span class="sxs-lookup"><span data-stu-id="de69a-530">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="de69a-531">Aucune d’entre elles n’est meilleure qu’une autre.</span><span class="sxs-lookup"><span data-stu-id="de69a-531">None of them are strictly superior to another.</span></span> <span data-ttu-id="de69a-532">La meilleure approche dépend des contraintes de l’application.</span><span class="sxs-lookup"><span data-stu-id="de69a-532">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="de69a-533">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="de69a-533">Basic usage</span></span>

<span data-ttu-id="de69a-534">Vous pouvez inscrire la `IHttpClientFactory` en appelant la méthode d’extension `AddHttpClient` sur la `IServiceCollection`, à l’intérieur la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="de69a-534">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="de69a-535">Une fois inscrit, le code peut accepter un `IHttpClientFactory` partout où des services peuvent être injectés avec [une injection de dépendance](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="de69a-535">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="de69a-536">La `IHttpClientFactory` peut être utilisée pour créer une instance de `HttpClient` :</span><span class="sxs-lookup"><span data-stu-id="de69a-536">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="de69a-537">L’utilisation de `IHttpClientFactory` de cette façon est un excellent moyen de refactoriser une application existante.</span><span class="sxs-lookup"><span data-stu-id="de69a-537">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="de69a-538">Elle n’a aucun impact sur la façon dont `HttpClient` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="de69a-538">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="de69a-539">Dans les endroits où les instances de `HttpClient` sont actuellement créées, remplacez ces occurrences par un appel à <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="de69a-539">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="de69a-540">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="de69a-540">Named clients</span></span>

<span data-ttu-id="de69a-541">Si une application nécessite plusieurs utilisations distinctes de `HttpClient`, chacune avec une configuration différente, une option consiste à utiliser des **clients nommés**.</span><span class="sxs-lookup"><span data-stu-id="de69a-541">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="de69a-542">La configuration d’un `HttpClient` nommé peut être spécifiée lors de l’inscription dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="de69a-542">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="de69a-543">Dans le code précédent, `AddHttpClient` est appelé en fournissant le nom *github*.</span><span class="sxs-lookup"><span data-stu-id="de69a-543">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="de69a-544">Une configuration par défaut est appliquée à ce client : l’adresse de base et deux en-têtes nécessaires pour utiliser l’API GitHub.</span><span class="sxs-lookup"><span data-stu-id="de69a-544">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="de69a-545">Chaque fois que `CreateClient` est appelée, une nouvelle instance de `HttpClient` est créée et l’action de configuration est appelée.</span><span class="sxs-lookup"><span data-stu-id="de69a-545">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="de69a-546">Pour utiliser un client nommé, un paramètre de chaîne peut être passé à `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="de69a-546">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="de69a-547">Spécifiez le nom du client à créer :</span><span class="sxs-lookup"><span data-stu-id="de69a-547">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="de69a-548">Dans le code précédent, la requête n’a pas besoin de spécifier un nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="de69a-548">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="de69a-549">Elle peut simplement passer le chemin, car l’adresse de base configurée pour le client est utilisée.</span><span class="sxs-lookup"><span data-stu-id="de69a-549">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="de69a-550">Clients typés</span><span class="sxs-lookup"><span data-stu-id="de69a-550">Typed clients</span></span>

<span data-ttu-id="de69a-551">Clients typés :</span><span class="sxs-lookup"><span data-stu-id="de69a-551">Typed clients:</span></span>

* <span data-ttu-id="de69a-552">Fournissent les mêmes fonctionnalités que les clients nommés, sans qu’il soit nécessaire d’utiliser des chaînes comme clés.</span><span class="sxs-lookup"><span data-stu-id="de69a-552">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="de69a-553">Bénéficie de l’aide d’IntelliSense et du compilateur lors de l’utilisation des clients.</span><span class="sxs-lookup"><span data-stu-id="de69a-553">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="de69a-554">Fournissent un emplacement unique pour configurer et interagir avec un `HttpClient` particulier.</span><span class="sxs-lookup"><span data-stu-id="de69a-554">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="de69a-555">Par exemple, un même client typé peut être utilisé pour un point de terminaison de backend et pour encapsuler la logique relative à ce point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="de69a-555">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="de69a-556">Fonctionnent avec l’injection de dépendances et peuvent être injectés là où c’est nécessaire dans votre application.</span><span class="sxs-lookup"><span data-stu-id="de69a-556">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="de69a-557">Un client typé accepte un `HttpClient` dans son constructeur :</span><span class="sxs-lookup"><span data-stu-id="de69a-557">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="de69a-558">Dans le code précédent, la configuration est déplacée dans le client typé.</span><span class="sxs-lookup"><span data-stu-id="de69a-558">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="de69a-559">L’objet `HttpClient` est exposé en tant que propriété publique.</span><span class="sxs-lookup"><span data-stu-id="de69a-559">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="de69a-560">Il est possible de définir des méthodes d’API spécifiques qui exposent les fonctionnalités de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="de69a-560">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="de69a-561">La méthode `GetAspNetDocsIssues` encapsule le code nécessaire pour interroger et analyser les problèmes ouverts les plus récents d’un dépôt GitHub.</span><span class="sxs-lookup"><span data-stu-id="de69a-561">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="de69a-562">Pour inscrire un client typé, la méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> générique peut être utilisée dans `Startup.ConfigureServices`, en spécifiant la classe du client typé :</span><span class="sxs-lookup"><span data-stu-id="de69a-562">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="de69a-563">Le client typé est inscrit comme étant transitoire avec injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="de69a-563">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="de69a-564">Le client typé peut être injecté et utilisé directement :</span><span class="sxs-lookup"><span data-stu-id="de69a-564">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="de69a-565">Si vous préférez, vous pouvez spécifier la configuration d’un client typé lors de l’inscription dans `Startup.ConfigureServices` au lieu de le faire dans le constructeur du client typé :</span><span class="sxs-lookup"><span data-stu-id="de69a-565">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="de69a-566">Il est possible d’encapsuler entièrement le `HttpClient` dans un client typé.</span><span class="sxs-lookup"><span data-stu-id="de69a-566">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="de69a-567">Au lieu de l’exposer en tant que propriété, vous pouvez fournir des méthodes publiques qui appellent l’instance de `HttpClient` en interne.</span><span class="sxs-lookup"><span data-stu-id="de69a-567">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="de69a-568">Dans le code précédent, le `HttpClient` est stocké en tant que champ privé.</span><span class="sxs-lookup"><span data-stu-id="de69a-568">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="de69a-569">Tous les accès nécessaires pour effectuer des appels externes passent par la méthode `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="de69a-569">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="de69a-570">Clients générés</span><span class="sxs-lookup"><span data-stu-id="de69a-570">Generated clients</span></span>

<span data-ttu-id="de69a-571">`IHttpClientFactory` peut être utilisé en combinaison avec d’autres bibliothèques tierces, comme [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="de69a-571">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="de69a-572">Refit est une bibliothèque REST pour .NET.</span><span class="sxs-lookup"><span data-stu-id="de69a-572">Refit is a REST library for .NET.</span></span> <span data-ttu-id="de69a-573">Il convertit les API REST en interfaces dynamiques.</span><span class="sxs-lookup"><span data-stu-id="de69a-573">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="de69a-574">Une implémentation de l’interface est générée dynamiquement par le `RestService`, avec `HttpClient` pour faire les appels HTTP externes.</span><span class="sxs-lookup"><span data-stu-id="de69a-574">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="de69a-575">Une interface et une réponse sont définies pour représenter l’API externe et sa réponse :</span><span class="sxs-lookup"><span data-stu-id="de69a-575">An interface and a reply are defined to represent the external API and its response:</span></span>

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

<span data-ttu-id="de69a-576">Vous pouvez ajouter un client typé en utilisant Refit pour générer l’implémentation :</span><span class="sxs-lookup"><span data-stu-id="de69a-576">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="de69a-577">L’interface définie peut être utilisée quand c’est nécessaire, avec l’implémentation fournie par l’injection de dépendances et Refit :</span><span class="sxs-lookup"><span data-stu-id="de69a-577">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a><span data-ttu-id="de69a-578">Middleware pour les requêtes sortantes</span><span class="sxs-lookup"><span data-stu-id="de69a-578">Outgoing request middleware</span></span>

<span data-ttu-id="de69a-579">`HttpClient` intègre déjà le concept de délégation des gestionnaires qui peuvent être liés ensemble pour les requêtes HTTP sortantes.</span><span class="sxs-lookup"><span data-stu-id="de69a-579">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="de69a-580">Le `IHttpClientFactory` permet de définir facilement les gestionnaires à appliquer pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="de69a-580">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="de69a-581">Il prend en charge l’inscription et le chaînage de plusieurs gestionnaires pour créer un pipeline de middlewares pour les requêtes sortantes.</span><span class="sxs-lookup"><span data-stu-id="de69a-581">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="de69a-582">Chacun de ces gestionnaires peut effectuer un travail avant et après la requête sortante.</span><span class="sxs-lookup"><span data-stu-id="de69a-582">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="de69a-583">Ce modèle est similaire au pipeline de middlewares entrants dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="de69a-583">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="de69a-584">Le modèle fournit un mécanisme permettant de gérer les problèmes transversaux liés aux des requêtes HTTP, notamment la mise en cache, la gestion des erreurs, la sérialisation et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="de69a-584">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="de69a-585">Pour créer un gestionnaire, définissez une classe dérivant de <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="de69a-585">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="de69a-586">Remplacez la méthode `SendAsync` de façon à exécuter du code avant de passer la requête au gestionnaire suivant dans le pipeline :</span><span class="sxs-lookup"><span data-stu-id="de69a-586">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="de69a-587">Le code précédent définit un gestionnaire de base.</span><span class="sxs-lookup"><span data-stu-id="de69a-587">The preceding code defines a basic handler.</span></span> <span data-ttu-id="de69a-588">Il vérifie si un en-tête `X-API-KEY` a été inclus dans la requête.</span><span class="sxs-lookup"><span data-stu-id="de69a-588">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="de69a-589">Si l’en-tête est manquant, il peut éviter l’appel HTTP et retourner une réponse appropriée.</span><span class="sxs-lookup"><span data-stu-id="de69a-589">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="de69a-590">Lors de l’inscription, un ou plusieurs gestionnaires peuvent être ajoutés à la configuration pour un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="de69a-590">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="de69a-591">Cette tâche est accomplie via des méthodes d’extension sur le <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="de69a-591">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="de69a-592">Dans le code précédent, le `ValidateHeaderHandler` est inscrit avec une injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="de69a-592">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="de69a-593">Le gestionnaire **doit** être inscrit dans l’injection de dépendances en tant que service temporaire, sans étendue.</span><span class="sxs-lookup"><span data-stu-id="de69a-593">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="de69a-594">Si le gestionnaire est inscrit en tant que service étendu et que des services dont dépend le gestionnaire peuvent être supprimés :</span><span class="sxs-lookup"><span data-stu-id="de69a-594">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="de69a-595">Les services du gestionnaire peuvent être supprimés avant que le gestionnaire ne soit hors de portée.</span><span class="sxs-lookup"><span data-stu-id="de69a-595">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="de69a-596">Les services du gestionnaire supprimés entraînent un échec du gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="de69a-596">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="de69a-597">Une fois inscrit, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> peut être appelé en passant en entrée le type du gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="de69a-597">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="de69a-598">Vous pouvez inscrire plusieurs gestionnaires dans l’ordre où ils doivent être exécutés.</span><span class="sxs-lookup"><span data-stu-id="de69a-598">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="de69a-599">Chaque gestionnaire wrappe le gestionnaire suivant jusqu’à ce que le dernier `HttpClientHandler` exécute la requête :</span><span class="sxs-lookup"><span data-stu-id="de69a-599">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="de69a-600">Utilisez l’une des approches suivantes pour partager l’état de chaque requête avec les gestionnaires de messages :</span><span class="sxs-lookup"><span data-stu-id="de69a-600">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="de69a-601">Passez des données dans le gestionnaire en utilisant `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="de69a-601">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="de69a-602">Utilisez `IHttpContextAccessor` pour accéder à la requête en cours.</span><span class="sxs-lookup"><span data-stu-id="de69a-602">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="de69a-603">Créez un objet de stockage `AsyncLocal` personnalisé pour passer les données.</span><span class="sxs-lookup"><span data-stu-id="de69a-603">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="de69a-604">Utiliser les gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="de69a-604">Use Polly-based handlers</span></span>

<span data-ttu-id="de69a-605">`IHttpClientFactory` s’intègre à une bibliothèque tierce très utilisée, appelée [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="de69a-605">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="de69a-606">Polly est une bibliothèque complète de gestion des erreurs transitoires et de résilience pour .NET.</span><span class="sxs-lookup"><span data-stu-id="de69a-606">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="de69a-607">Elle permet aux développeurs de formuler facilement et de façon thread-safe des stratégies, comme Retry (Nouvelle tentative), Circuit Breaker (Disjoncteur), Timeout (Délai d’attente), Bulkhead Isolation (Isolation par cloisonnement) et Fallback (Alternative de repli).</span><span class="sxs-lookup"><span data-stu-id="de69a-607">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="de69a-608">Des méthodes d’extension sont fournies pour permettre l’utilisation de stratégies Polly avec les instances configurées de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="de69a-608">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="de69a-609">Les extensions Polly :</span><span class="sxs-lookup"><span data-stu-id="de69a-609">The Polly extensions:</span></span>

* <span data-ttu-id="de69a-610">Prennent en charge l’ajout de gestionnaires Polly à des clients.</span><span class="sxs-lookup"><span data-stu-id="de69a-610">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="de69a-611">Peuvent être utilisées après l’installation du package NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="de69a-611">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="de69a-612">Ce package n’est pas inclus dans le framework partagé ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="de69a-612">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="de69a-613">Gérer les erreurs temporaires</span><span class="sxs-lookup"><span data-stu-id="de69a-613">Handle transient faults</span></span>

<span data-ttu-id="de69a-614">Les erreurs courantes se produisent lorsque des appels HTTP externes sont temporaires.</span><span class="sxs-lookup"><span data-stu-id="de69a-614">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="de69a-615">Une méthode d’extension pratique nommée `AddTransientHttpErrorPolicy` est incluse : elle permet de définir une stratégie pour gérer les erreurs temporaires.</span><span class="sxs-lookup"><span data-stu-id="de69a-615">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="de69a-616">Les stratégies configurées avec cette méthode d’extension gèrent `HttpRequestException`, les réponses HTTP 5xx et les réponses HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="de69a-616">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="de69a-617">L’extension `AddTransientHttpErrorPolicy` peut être utilisée dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="de69a-617">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="de69a-618">L’extension fournit l’accès à un objet `PolicyBuilder` configuré pour gérer les erreurs représentant une erreur temporaire possible :</span><span class="sxs-lookup"><span data-stu-id="de69a-618">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="de69a-619">Dans le code précédent, une stratégie `WaitAndRetryAsync` est définie.</span><span class="sxs-lookup"><span data-stu-id="de69a-619">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="de69a-620">Les requêtes qui ont échoué sont retentées jusqu’à trois fois avec un délai de 600 ms entre les tentatives.</span><span class="sxs-lookup"><span data-stu-id="de69a-620">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="de69a-621">Sélectionner dynamiquement des stratégies</span><span class="sxs-lookup"><span data-stu-id="de69a-621">Dynamically select policies</span></span>

<span data-ttu-id="de69a-622">Il existe d’autres méthodes d’extension que vous pouvez utiliser pour ajouter des gestionnaires Polly.</span><span class="sxs-lookup"><span data-stu-id="de69a-622">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="de69a-623">Une de ces extensions est `AddPolicyHandler`, qui a plusieurs surcharges.</span><span class="sxs-lookup"><span data-stu-id="de69a-623">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="de69a-624">Une de ces surcharges permet l’inspection de la requête lors de la définition de la stratégie à appliquer :</span><span class="sxs-lookup"><span data-stu-id="de69a-624">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="de69a-625">Dans le code précédent, si la requête sortante est un HTTP GET, un délai d’attente de 10 secondes est appliqué.</span><span class="sxs-lookup"><span data-stu-id="de69a-625">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="de69a-626">Pour toutes les autres méthodes HTTP, un délai d’attente de 30 secondes est utilisé.</span><span class="sxs-lookup"><span data-stu-id="de69a-626">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="de69a-627">Ajouter plusieurs gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="de69a-627">Add multiple Polly handlers</span></span>

<span data-ttu-id="de69a-628">Il est courant d’imbriquer des stratégies Polly pour fournir des fonctionnalités améliorées :</span><span class="sxs-lookup"><span data-stu-id="de69a-628">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="de69a-629">Dans l’exemple précédent, deux gestionnaires sont ajoutés.</span><span class="sxs-lookup"><span data-stu-id="de69a-629">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="de69a-630">Le premier utilise l’extension `AddTransientHttpErrorPolicy` pour ajouter une stratégie de nouvelle tentative.</span><span class="sxs-lookup"><span data-stu-id="de69a-630">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="de69a-631">Les requêtes qui ont échoué sont retentées jusqu’à trois fois.</span><span class="sxs-lookup"><span data-stu-id="de69a-631">Failed requests are retried up to three times.</span></span> <span data-ttu-id="de69a-632">Le deuxième appel à `AddTransientHttpErrorPolicy` ajoute une stratégie de disjoncteur.</span><span class="sxs-lookup"><span data-stu-id="de69a-632">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="de69a-633">Les requêtes externes supplémentaires sont bloquées pendant 30 secondes si cinq tentatives successives échouent.</span><span class="sxs-lookup"><span data-stu-id="de69a-633">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="de69a-634">Les stratégies de disjoncteur sont avec état.</span><span class="sxs-lookup"><span data-stu-id="de69a-634">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="de69a-635">Tous les appels effectués via ce client partagent le même état du circuit.</span><span class="sxs-lookup"><span data-stu-id="de69a-635">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="de69a-636">Ajouter des stratégies à partir du Registre Polly</span><span class="sxs-lookup"><span data-stu-id="de69a-636">Add policies from the Polly registry</span></span>

<span data-ttu-id="de69a-637">Une approche de la gestion des stratégies régulièrement utilisées consiste à les définir une seule fois et à les inscrire avec un `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="de69a-637">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="de69a-638">Il existe une méthode d’extension qui permet l’ajout d’un gestionnaire avec une stratégie du Registre :</span><span class="sxs-lookup"><span data-stu-id="de69a-638">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="de69a-639">Dans le code précédent, deux stratégies sont inscrites lorsque `PolicyRegistry` est ajouté à `ServiceCollection`.</span><span class="sxs-lookup"><span data-stu-id="de69a-639">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="de69a-640">Pour utiliser une stratégie du Registre, la méthode `AddPolicyHandlerFromRegistry` est utilisée, en passant le nom de la stratégie à appliquer.</span><span class="sxs-lookup"><span data-stu-id="de69a-640">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="de69a-641">Vous trouverez plus d’informations sur les intégrations de `IHttpClientFactory` et de Polly dans le [wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="de69a-641">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="de69a-642">HttpClient et gestion de la durée de vie</span><span class="sxs-lookup"><span data-stu-id="de69a-642">HttpClient and lifetime management</span></span>

<span data-ttu-id="de69a-643">Une nouvelle instance `HttpClient` est retournée à chaque fois que `CreateClient` est appelé sur `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="de69a-643">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="de69a-644">Il existe un <xref:System.Net.Http.HttpMessageHandler> par client nommé.</span><span class="sxs-lookup"><span data-stu-id="de69a-644">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="de69a-645">La fabrique gère les durées de vie des instances `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="de69a-645">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="de69a-646">`IHttpClientFactory` regroupe dans un pool les instances de `HttpMessageHandler` créées par la fabrique pour réduire la consommation des ressources.</span><span class="sxs-lookup"><span data-stu-id="de69a-646">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="de69a-647">Une instance de `HttpMessageHandler` peut être réutilisée à partir du pool quand vous créez une instance de `HttpClient` si sa durée de vie n’a pas expiré.</span><span class="sxs-lookup"><span data-stu-id="de69a-647">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="de69a-648">Le regroupement de gestionnaires en pools est souhaitable, car chaque gestionnaire gère habituellement ses propres connexions HTTP sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="de69a-648">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="de69a-649">La création de plus de gestionnaires que nécessaire peut entraîner des délais de connexion.</span><span class="sxs-lookup"><span data-stu-id="de69a-649">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="de69a-650">Certains gestionnaires conservent aussi les connexions indéfiniment ouvertes, ce qui peut empêcher le gestionnaire de réagir aux changements du DNS.</span><span class="sxs-lookup"><span data-stu-id="de69a-650">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="de69a-651">La durée de vie par défaut d’un gestionnaire est de deux minutes.</span><span class="sxs-lookup"><span data-stu-id="de69a-651">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="de69a-652">La valeur par défaut peut être remplacée pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="de69a-652">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="de69a-653">Pour la remplacer, appelez <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> sur le `IHttpClientBuilder` qui est retourné lors de la création du client :</span><span class="sxs-lookup"><span data-stu-id="de69a-653">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="de69a-654">La suppression du client n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="de69a-654">Disposal of the client isn't required.</span></span> <span data-ttu-id="de69a-655">La suppression annule les requêtes sortantes et garantit que l’instance `HttpClient` donnée ne peut pas être utilisée après avoir appelé <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="de69a-655">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="de69a-656">`IHttpClientFactory` effectue le suivi et libère les ressources utilisées par les instances `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="de69a-656">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="de69a-657">Les instances `HttpClient` peuvent généralement être traitées en tant qu’objets .NET ne nécessitant pas une suppression.</span><span class="sxs-lookup"><span data-stu-id="de69a-657">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="de69a-658">Le fait de conserver une seule instance `HttpClient` active pendant une longue durée est un modèle commun utilisé avant le lancement de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="de69a-658">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="de69a-659">Ce modèle devient inutile après la migration vers `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="de69a-659">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="de69a-660">Alternatives to IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="de69a-660">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="de69a-661">Using `IHttpClientFactory` in a DI-enabled app avoids:</span><span class="sxs-lookup"><span data-stu-id="de69a-661">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="de69a-662">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span><span class="sxs-lookup"><span data-stu-id="de69a-662">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="de69a-663">Stale-DNS problems by cycling `HttpMessageHandler` instances at regular instances.</span><span class="sxs-lookup"><span data-stu-id="de69a-663">Stale-DNS problems by cycling `HttpMessageHandler` instances at regular instances.</span></span>

<span data-ttu-id="de69a-664">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span><span class="sxs-lookup"><span data-stu-id="de69a-664">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="de69a-665">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span><span class="sxs-lookup"><span data-stu-id="de69a-665">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="de69a-666">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span><span class="sxs-lookup"><span data-stu-id="de69a-666">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="de69a-667">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span><span class="sxs-lookup"><span data-stu-id="de69a-667">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="de69a-668">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span><span class="sxs-lookup"><span data-stu-id="de69a-668">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="de69a-669">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span><span class="sxs-lookup"><span data-stu-id="de69a-669">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="de69a-670">This sharing prevents socket exhaustion.</span><span class="sxs-lookup"><span data-stu-id="de69a-670">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="de69a-671">The `SocketsHttpHandler ` cycles connections according to `PooledConnectionLifetime` to avoid state-DNS problems.</span><span class="sxs-lookup"><span data-stu-id="de69a-671">The `SocketsHttpHandler ` cycles connections according to `PooledConnectionLifetime` to avoid state-DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="de69a-672">Cookies</span><span class="sxs-lookup"><span data-stu-id="de69a-672">Cookies</span></span>

<span data-ttu-id="de69a-673">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span><span class="sxs-lookup"><span data-stu-id="de69a-673">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="de69a-674">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span><span class="sxs-lookup"><span data-stu-id="de69a-674">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="de69a-675">For apps that require cookies, consider either:</span><span class="sxs-lookup"><span data-stu-id="de69a-675">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="de69a-676">Disabling automatic cookie handling</span><span class="sxs-lookup"><span data-stu-id="de69a-676">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="de69a-677">Avoiding `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="de69a-677">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="de69a-678">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span><span class="sxs-lookup"><span data-stu-id="de69a-678">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="de69a-679">Journalisation</span><span class="sxs-lookup"><span data-stu-id="de69a-679">Logging</span></span>

<span data-ttu-id="de69a-680">Les clients créés via `IHttpClientFactory` enregistrent les messages de journalisation pour toutes les requêtes.</span><span class="sxs-lookup"><span data-stu-id="de69a-680">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="de69a-681">Activez le niveau d’informations approprié dans votre configuration de journalisation pour voir les messages de journalisation par défaut.</span><span class="sxs-lookup"><span data-stu-id="de69a-681">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="de69a-682">Une journalisation supplémentaire, comme celle des en-têtes des requêtes, est incluse seulement au niveau de trace.</span><span class="sxs-lookup"><span data-stu-id="de69a-682">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="de69a-683">La catégorie de journal utilisée pour chaque client comprend le nom du client.</span><span class="sxs-lookup"><span data-stu-id="de69a-683">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="de69a-684">Par exemple, un client nommé *MyNamedClient* journalise les messages avec la catégorie `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="de69a-684">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="de69a-685">Les messages avec le suffixe *LogicalHandler* se produisent en dehors du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="de69a-685">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="de69a-686">Lors d’une requête, les messages sont journalisés avant que d’autres gestionnaires du pipeline l’aient traitée.</span><span class="sxs-lookup"><span data-stu-id="de69a-686">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="de69a-687">Lors d’une réponse, les messages sont journalisés une fois que tous les autres gestionnaires du pipeline ont reçu la réponse.</span><span class="sxs-lookup"><span data-stu-id="de69a-687">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="de69a-688">La journalisation se produit également à l’intérieur du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="de69a-688">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="de69a-689">Dans l’exemple *MyNamedClient*, ces messages sont journalisés avec la catégorie de journalisation `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="de69a-689">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="de69a-690">Pour la requête, cela se produit après que tous les autres gestionnaires ont été exécutés et immédiatement avant l’envoi de la requête sur le réseau.</span><span class="sxs-lookup"><span data-stu-id="de69a-690">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="de69a-691">Lors de la réponse, cette journalisation inclut l’état de la réponse avant qu’elle repasse à travers le pipeline de gestionnaires.</span><span class="sxs-lookup"><span data-stu-id="de69a-691">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="de69a-692">L’activation de la journalisation à l’extérieur et à l’intérieur du pipeline permet l’inspection des changements apportés par les autres gestionnaires du pipeline.</span><span class="sxs-lookup"><span data-stu-id="de69a-692">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="de69a-693">Par exemple, cela peut comprendre des changements apportés aux en-têtes des requêtes ou au code d’état de la réponse.</span><span class="sxs-lookup"><span data-stu-id="de69a-693">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="de69a-694">L’ajout du nom du client dans la catégorie de journalisation permet si nécessaire de filtrer le journal pour des clients nommés spécifiques.</span><span class="sxs-lookup"><span data-stu-id="de69a-694">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="de69a-695">Configurer le HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="de69a-695">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="de69a-696">Il peut être nécessaire de contrôler la configuration du `HttpMessageHandler` interne utilisé par un client.</span><span class="sxs-lookup"><span data-stu-id="de69a-696">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="de69a-697">Un `IHttpClientBuilder` est retourné quand vous ajoutez des clients nommés ou typés.</span><span class="sxs-lookup"><span data-stu-id="de69a-697">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="de69a-698">La méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> peut être utilisée pour définir un délégué.</span><span class="sxs-lookup"><span data-stu-id="de69a-698">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="de69a-699">Le délégué est utilisé pour créer et configurer le `HttpMessageHandler` principal utilisé par ce client :</span><span class="sxs-lookup"><span data-stu-id="de69a-699">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="de69a-700">Utiliser IHttpClientFactory dans une application console</span><span class="sxs-lookup"><span data-stu-id="de69a-700">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="de69a-701">Dans une application console, ajoutez les références de package suivantes au projet :</span><span class="sxs-lookup"><span data-stu-id="de69a-701">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="de69a-702">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="de69a-702">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="de69a-703">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="de69a-703">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="de69a-704">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="de69a-704">In the following example:</span></span>

* <span data-ttu-id="de69a-705"><xref:System.Net.Http.IHttpClientFactory> est inscrit dans le conteneur de service dans de l’[hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="de69a-705"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="de69a-706">`MyService` crée une instance de fabrique cliente à partir du service, qui est utilisée pour créer un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="de69a-706">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="de69a-707">`HttpClient` est utilisé pour récupérer une page web.</span><span class="sxs-lookup"><span data-stu-id="de69a-707">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="de69a-708">`Main` crée une étendue pour exécuter la méthode `GetPage` du service et écrire les 500 premiers caractères du contenu de la page web dans la console.</span><span class="sxs-lookup"><span data-stu-id="de69a-708">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="de69a-709">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="de69a-709">Additional resources</span></span>

* [<span data-ttu-id="de69a-710">Utiliser HttpClientFactory pour implémenter des requêtes HTTP résilientes</span><span class="sxs-lookup"><span data-stu-id="de69a-710">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="de69a-711">Implémenter de nouvelles tentatives d’appel HTTP avec interruption exponentielle avec des stratégies Polly et HttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="de69a-711">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="de69a-712">Implémenter le modèle Disjoncteur</span><span class="sxs-lookup"><span data-stu-id="de69a-712">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
