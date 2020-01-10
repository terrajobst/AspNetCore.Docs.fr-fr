---
title: Effectuer des requêtes HTTP en utilisant IHttpClientFactory dans ASP.NET Core
author: stevejgordon
description: Découvrez plus d’informations sur l’utilisation de l’interface IHttpClientFactory pour gérer des instances HttpClient logiques dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/16/2019
uid: fundamentals/http-requests
ms.openlocfilehash: 482f8e28c23c621cecaf9ce111d89e9166ea6d85
ms.sourcegitcommit: da2fb2d78ce70accdba903ccbfdcfffdd0112123
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/07/2020
ms.locfileid: "75722724"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="a8275-103">Effectuer des requêtes HTTP en utilisant IHttpClientFactory dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8275-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a8275-104">Par [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT)et [Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="a8275-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="a8275-105">Une <xref:System.Net.Http.IHttpClientFactory> peut être inscrite et utilisée pour configurer et créer des instances de <xref:System.Net.Http.HttpClient> dans une application.</span><span class="sxs-lookup"><span data-stu-id="a8275-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="a8275-106">`IHttpClientFactory` offre les avantages suivants :</span><span class="sxs-lookup"><span data-stu-id="a8275-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="a8275-107">Fournit un emplacement central pour le nommage et la configuration d’instance de `HttpClient` logiques.</span><span class="sxs-lookup"><span data-stu-id="a8275-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="a8275-108">Par exemple, un client nommé *GitHub* peut être inscrit et configuré pour accéder à [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="a8275-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="a8275-109">Un client par défaut peut être inscrit pour un accès général.</span><span class="sxs-lookup"><span data-stu-id="a8275-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="a8275-110">Codifie le concept d’intergiciel (middleware) sortant via la délégation de gestionnaires dans `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="a8275-111">Fournit des extensions pour l’intergiciel (middleware) basé sur Polly pour tirer parti des gestionnaires de délégation dans `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="a8275-112">Gère le regroupement et la durée de vie des instances de `HttpClientMessageHandler` sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="a8275-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="a8275-113">La gestion automatique évite les problèmes courants liés au DNS (Domain Name System) qui se produisent lors de la gestion manuelle des durées de vie de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="a8275-114">Ajoute une expérience de journalisation configurable (via `ILogger`) pour toutes les requêtes envoyées via des clients créés par la fabrique.</span><span class="sxs-lookup"><span data-stu-id="a8275-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="a8275-115">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="a8275-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="a8275-116">L’exemple de code dans cette version de rubrique utilise <xref:System.Text.Json> pour désérialiser le contenu JSON renvoyé dans les réponses HTTP.</span><span class="sxs-lookup"><span data-stu-id="a8275-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="a8275-117">Pour obtenir des exemples qui utilisent des `Json.NET` et des `ReadAsAsync<T>`, utilisez le sélecteur de version pour sélectionner une version 2. x de cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="a8275-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="a8275-118">Modèles de consommation</span><span class="sxs-lookup"><span data-stu-id="a8275-118">Consumption patterns</span></span>

<span data-ttu-id="a8275-119">Vous pouvez utiliser `IHttpClientFactory` dans une application de plusieurs façons :</span><span class="sxs-lookup"><span data-stu-id="a8275-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="a8275-120">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="a8275-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="a8275-121">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="a8275-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="a8275-122">Clients typés</span><span class="sxs-lookup"><span data-stu-id="a8275-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="a8275-123">Clients générés</span><span class="sxs-lookup"><span data-stu-id="a8275-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="a8275-124">La meilleure approche dépend des exigences de l’application.</span><span class="sxs-lookup"><span data-stu-id="a8275-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="a8275-125">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="a8275-125">Basic usage</span></span>

<span data-ttu-id="a8275-126">`IHttpClientFactory` peut être inscrit en appelant `AddHttpClient`:</span><span class="sxs-lookup"><span data-stu-id="a8275-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="a8275-127">Un `IHttpClientFactory` peut être demandé à l’aide [de l’injection de dépendances (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a8275-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a8275-128">Le code suivant utilise `IHttpClientFactory` pour créer une instance `HttpClient` :</span><span class="sxs-lookup"><span data-stu-id="a8275-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="a8275-129">L’utilisation de `IHttpClientFactory` comme dans l’exemple précédent est un bon moyen de refactoriser une application existante.</span><span class="sxs-lookup"><span data-stu-id="a8275-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="a8275-130">Elle n’a aucun impact sur l’utilisation de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="a8275-131">Dans les emplacements où les instances de `HttpClient` sont créées dans une application existante, remplacez-les par des appels à <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="a8275-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="a8275-132">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="a8275-132">Named clients</span></span>

<span data-ttu-id="a8275-133">Les clients nommés sont un bon choix lorsque :</span><span class="sxs-lookup"><span data-stu-id="a8275-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="a8275-134">L’application requiert de nombreuses utilisations distinctes de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="a8275-135">De nombreuses `HttpClient`s ont une configuration différente.</span><span class="sxs-lookup"><span data-stu-id="a8275-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="a8275-136">La configuration d’un `HttpClient` nommé peut être spécifiée lors de l’inscription dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a8275-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="a8275-137">Dans le code précédent, le client est configuré avec :</span><span class="sxs-lookup"><span data-stu-id="a8275-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="a8275-138">Adresse de base `https://api.github.com/`.</span><span class="sxs-lookup"><span data-stu-id="a8275-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="a8275-139">Deux en-têtes nécessaires à l’utilisation de l’API GitHub.</span><span class="sxs-lookup"><span data-stu-id="a8275-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="a8275-140">CreateClient</span><span class="sxs-lookup"><span data-stu-id="a8275-140">CreateClient</span></span>

<span data-ttu-id="a8275-141">Chaque fois que <xref:System.Net.Http.IHttpClientFactory.CreateClient*> est appelée :</span><span class="sxs-lookup"><span data-stu-id="a8275-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="a8275-142">Une nouvelle instance de `HttpClient` est créée.</span><span class="sxs-lookup"><span data-stu-id="a8275-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="a8275-143">L’action de configuration est appelée.</span><span class="sxs-lookup"><span data-stu-id="a8275-143">The configuration action is called.</span></span>

<span data-ttu-id="a8275-144">Pour créer un client nommé, transmettez son nom à `CreateClient`:</span><span class="sxs-lookup"><span data-stu-id="a8275-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="a8275-145">Dans le code précédent, la requête n’a pas besoin de spécifier un nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="a8275-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="a8275-146">Le code peut passer uniquement le chemin d’accès, dans la mesure où l’adresse de base configurée pour le client est utilisée.</span><span class="sxs-lookup"><span data-stu-id="a8275-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="a8275-147">Clients typés</span><span class="sxs-lookup"><span data-stu-id="a8275-147">Typed clients</span></span>

<span data-ttu-id="a8275-148">Clients typés :</span><span class="sxs-lookup"><span data-stu-id="a8275-148">Typed clients:</span></span>

* <span data-ttu-id="a8275-149">Fournissent les mêmes fonctionnalités que les clients nommés, sans qu’il soit nécessaire d’utiliser des chaînes comme clés.</span><span class="sxs-lookup"><span data-stu-id="a8275-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="a8275-150">Bénéficie de l’aide d’IntelliSense et du compilateur lors de l’utilisation des clients.</span><span class="sxs-lookup"><span data-stu-id="a8275-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="a8275-151">Fournissent un emplacement unique pour configurer et interagir avec un `HttpClient` particulier.</span><span class="sxs-lookup"><span data-stu-id="a8275-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="a8275-152">Par exemple, un seul client typé peut être utilisé :</span><span class="sxs-lookup"><span data-stu-id="a8275-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="a8275-153">Pour un seul point de terminaison backend.</span><span class="sxs-lookup"><span data-stu-id="a8275-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="a8275-154">Pour encapsuler toute la logique traitant le point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="a8275-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="a8275-155">Utilisez DI et pouvez les injecter lorsque cela est requis dans l’application.</span><span class="sxs-lookup"><span data-stu-id="a8275-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="a8275-156">Un client typé accepte un paramètre `HttpClient` dans son constructeur :</span><span class="sxs-lookup"><span data-stu-id="a8275-156">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="a8275-157">Dans le code précédent :</span><span class="sxs-lookup"><span data-stu-id="a8275-157">In the preceding code:</span></span>

* <span data-ttu-id="a8275-158">La configuration est déplacée vers le client typé.</span><span class="sxs-lookup"><span data-stu-id="a8275-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="a8275-159">L’objet `HttpClient` est exposé en tant que propriété publique.</span><span class="sxs-lookup"><span data-stu-id="a8275-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="a8275-160">Des méthodes spécifiques à l’API peuvent être créées pour exposer les fonctionnalités de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="a8275-161">Par exemple, la méthode `GetAspNetDocsIssues` encapsule du code pour récupérer des problèmes ouverts.</span><span class="sxs-lookup"><span data-stu-id="a8275-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="a8275-162">Le code suivant appelle <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> dans `Startup.ConfigureServices` pour inscrire une classe de client typée :</span><span class="sxs-lookup"><span data-stu-id="a8275-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="a8275-163">Le client typé est inscrit comme étant transitoire avec injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="a8275-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="a8275-164">Le client typé peut être injecté et utilisé directement :</span><span class="sxs-lookup"><span data-stu-id="a8275-164">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="a8275-165">La configuration d’un client typé peut être spécifiée lors de l’inscription dans `Startup.ConfigureServices`, plutôt que dans le constructeur du client typé :</span><span class="sxs-lookup"><span data-stu-id="a8275-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="a8275-166">Le `HttpClient` peut être encapsulé dans un client typé.</span><span class="sxs-lookup"><span data-stu-id="a8275-166">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="a8275-167">Au lieu de l’exposer en tant que propriété, définissez une méthode qui appelle l’instance de `HttpClient` en interne :</span><span class="sxs-lookup"><span data-stu-id="a8275-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="a8275-168">Dans le code précédent, le `HttpClient` est stocké dans un champ privé.</span><span class="sxs-lookup"><span data-stu-id="a8275-168">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="a8275-169">L’accès à l' `HttpClient` est par la méthode `GetRepos` publique.</span><span class="sxs-lookup"><span data-stu-id="a8275-169">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="a8275-170">Clients générés</span><span class="sxs-lookup"><span data-stu-id="a8275-170">Generated clients</span></span>

<span data-ttu-id="a8275-171">les `IHttpClientFactory` peuvent être utilisées en association avec des bibliothèques tierces, [telles que la](https://github.com/paulcbetts/refit)réutilisation.</span><span class="sxs-lookup"><span data-stu-id="a8275-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="a8275-172">Refit est une bibliothèque REST pour .NET.</span><span class="sxs-lookup"><span data-stu-id="a8275-172">Refit is a REST library for .NET.</span></span> <span data-ttu-id="a8275-173">Il convertit les API REST en interfaces dynamiques.</span><span class="sxs-lookup"><span data-stu-id="a8275-173">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="a8275-174">Une implémentation de l’interface est générée dynamiquement par le `RestService`, avec `HttpClient` pour faire les appels HTTP externes.</span><span class="sxs-lookup"><span data-stu-id="a8275-174">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="a8275-175">Une interface et une réponse sont définies pour représenter l’API externe et sa réponse :</span><span class="sxs-lookup"><span data-stu-id="a8275-175">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="a8275-176">Vous pouvez ajouter un client typé en utilisant Refit pour générer l’implémentation :</span><span class="sxs-lookup"><span data-stu-id="a8275-176">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="a8275-177">L’interface définie peut être utilisée quand c’est nécessaire, avec l’implémentation fournie par l’injection de dépendances et Refit :</span><span class="sxs-lookup"><span data-stu-id="a8275-177">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="a8275-178">Middleware pour les requêtes sortantes</span><span class="sxs-lookup"><span data-stu-id="a8275-178">Outgoing request middleware</span></span>

<span data-ttu-id="a8275-179">`HttpClient` présente le concept de délégation de gestionnaires qui peuvent être liés entre eux pour les requêtes HTTP sortantes.</span><span class="sxs-lookup"><span data-stu-id="a8275-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="a8275-180">`IHttpClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="a8275-180">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="a8275-181">Simplifie la définition des gestionnaires à appliquer pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="a8275-181">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="a8275-182">Prend en charge l’inscription et le chaînage de plusieurs gestionnaires pour générer un pipeline d’intergiciel (middleware) de demande sortante.</span><span class="sxs-lookup"><span data-stu-id="a8275-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="a8275-183">Chacun de ces gestionnaires peut effectuer un travail avant et après la requête sortante.</span><span class="sxs-lookup"><span data-stu-id="a8275-183">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="a8275-184">Ce modèle :</span><span class="sxs-lookup"><span data-stu-id="a8275-184">This pattern:</span></span>

  * <span data-ttu-id="a8275-185">Est semblable au pipeline d’intergiciel (middleware) entrant dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8275-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="a8275-186">Fournit un mécanisme permettant de gérer les problèmes transversaux autour des requêtes HTTP, par exemple :</span><span class="sxs-lookup"><span data-stu-id="a8275-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="a8275-187">mettre en cache</span><span class="sxs-lookup"><span data-stu-id="a8275-187">caching</span></span>
    * <span data-ttu-id="a8275-188">gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="a8275-188">error handling</span></span>
    * <span data-ttu-id="a8275-189">serialization</span><span class="sxs-lookup"><span data-stu-id="a8275-189">serialization</span></span>
    * <span data-ttu-id="a8275-190">enregistrer</span><span class="sxs-lookup"><span data-stu-id="a8275-190">logging</span></span>

<span data-ttu-id="a8275-191">Pour créer un gestionnaire de délégation :</span><span class="sxs-lookup"><span data-stu-id="a8275-191">To create a delegating handler:</span></span>

* <span data-ttu-id="a8275-192">Dérivez de <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="a8275-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="a8275-193">Substituez <xref:System.Net.Http.DelegatingHandler.SendAsync*></span><span class="sxs-lookup"><span data-stu-id="a8275-193">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="a8275-194">Exécutez le code avant de passer la requête au gestionnaire suivant dans le pipeline :</span><span class="sxs-lookup"><span data-stu-id="a8275-194">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="a8275-195">Le code précédent vérifie si l’en-tête `X-API-KEY` est dans la demande.</span><span class="sxs-lookup"><span data-stu-id="a8275-195">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="a8275-196">Si `X-API-KEY` est manquant, <xref:System.Net.HttpStatusCode.BadRequest> est retourné.</span><span class="sxs-lookup"><span data-stu-id="a8275-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="a8275-197">Plusieurs gestionnaires peuvent être ajoutés à la configuration d’un `HttpClient` avec <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span><span class="sxs-lookup"><span data-stu-id="a8275-197">More than one handler can be added to the configuration for an `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="a8275-198">Dans le code précédent, le `ValidateHeaderHandler` est inscrit avec une injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="a8275-198">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="a8275-199">`IHttpClientFactory` crée une étendue DI distincte pour chaque gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="a8275-199">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="a8275-200">Les gestionnaires peuvent dépendre des services de toute portée.</span><span class="sxs-lookup"><span data-stu-id="a8275-200">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="a8275-201">Les services dont dépendent les gestionnaires sont supprimés lorsque le gestionnaire est supprimé.</span><span class="sxs-lookup"><span data-stu-id="a8275-201">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="a8275-202">Une fois inscrit, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> peut être appelé en passant en entrée le type pour le gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="a8275-202">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="a8275-203">Vous pouvez inscrire plusieurs gestionnaires dans l’ordre où ils doivent être exécutés.</span><span class="sxs-lookup"><span data-stu-id="a8275-203">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="a8275-204">Chaque gestionnaire wrappe le gestionnaire suivant jusqu’à ce que le dernier `HttpClientHandler` exécute la requête :</span><span class="sxs-lookup"><span data-stu-id="a8275-204">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="a8275-205">Utilisez l’une des approches suivantes pour partager l’état de chaque requête avec les gestionnaires de messages :</span><span class="sxs-lookup"><span data-stu-id="a8275-205">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="a8275-206">Transmettez les données dans le gestionnaire à l’aide de [HttpRequestMessage. Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span><span class="sxs-lookup"><span data-stu-id="a8275-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="a8275-207">Utilisez <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> pour accéder à la requête en cours.</span><span class="sxs-lookup"><span data-stu-id="a8275-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="a8275-208">Créez un objet de stockage <xref:System.Threading.AsyncLocal`1> personnalisé pour passer les données.</span><span class="sxs-lookup"><span data-stu-id="a8275-208">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="a8275-209">Utiliser les gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="a8275-209">Use Polly-based handlers</span></span>

<span data-ttu-id="a8275-210">`IHttpClientFactory` s’intègre à la bibliothèque tierce [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="a8275-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="a8275-211">Polly est une bibliothèque complète de gestion des erreurs transitoires et de résilience pour .NET.</span><span class="sxs-lookup"><span data-stu-id="a8275-211">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="a8275-212">Elle permet aux développeurs de formuler facilement et de façon thread-safe des stratégies, comme Retry (Nouvelle tentative), Circuit Breaker (Disjoncteur), Timeout (Délai d’attente), Bulkhead Isolation (Isolation par cloisonnement) et Fallback (Alternative de repli).</span><span class="sxs-lookup"><span data-stu-id="a8275-212">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="a8275-213">Des méthodes d’extension sont fournies pour permettre l’utilisation de stratégies Polly avec les instances configurées de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-213">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="a8275-214">Les extensions Polly prennent en charge l’ajout de gestionnaires basés sur Polly aux clients.</span><span class="sxs-lookup"><span data-stu-id="a8275-214">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="a8275-215">Polly requiert le package NuGet [Microsoft. extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) .</span><span class="sxs-lookup"><span data-stu-id="a8275-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="a8275-216">Gérer les erreurs temporaires</span><span class="sxs-lookup"><span data-stu-id="a8275-216">Handle transient faults</span></span>

<span data-ttu-id="a8275-217">Les erreurs se produisent généralement lorsque les appels HTTP externes sont temporaires.</span><span class="sxs-lookup"><span data-stu-id="a8275-217">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="a8275-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> permet de définir une stratégie pour gérer les erreurs temporaires.</span><span class="sxs-lookup"><span data-stu-id="a8275-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="a8275-219">Les stratégies configurées avec `AddTransientHttpErrorPolicy` gèrent les réponses suivantes :</span><span class="sxs-lookup"><span data-stu-id="a8275-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="a8275-220">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="a8275-220">HTTP 5xx</span></span>
* <span data-ttu-id="a8275-221">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="a8275-221">HTTP 408</span></span>

<span data-ttu-id="a8275-222">`AddTransientHttpErrorPolicy` permet d’accéder à un objet `PolicyBuilder` configuré pour gérer les erreurs qui représentent une erreur temporaire possible :</span><span class="sxs-lookup"><span data-stu-id="a8275-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="a8275-223">Dans le code précédent, une stratégie `WaitAndRetryAsync` est définie.</span><span class="sxs-lookup"><span data-stu-id="a8275-223">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="a8275-224">Les requêtes qui ont échoué sont retentées jusqu’à trois fois avec un délai de 600 ms entre les tentatives.</span><span class="sxs-lookup"><span data-stu-id="a8275-224">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="a8275-225">Sélectionner dynamiquement des stratégies</span><span class="sxs-lookup"><span data-stu-id="a8275-225">Dynamically select policies</span></span>

<span data-ttu-id="a8275-226">Les méthodes d’extension sont fournies pour ajouter des gestionnaires basés sur Polly, par exemple, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span><span class="sxs-lookup"><span data-stu-id="a8275-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="a8275-227">La surcharge de `AddPolicyHandler` suivante inspecte la demande pour décider de la stratégie à appliquer :</span><span class="sxs-lookup"><span data-stu-id="a8275-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="a8275-228">Dans le code précédent, si la requête sortante est un HTTP GET, un délai d’attente de 10 secondes est appliqué.</span><span class="sxs-lookup"><span data-stu-id="a8275-228">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="a8275-229">Pour toutes les autres méthodes HTTP, un délai d’attente de 30 secondes est utilisé.</span><span class="sxs-lookup"><span data-stu-id="a8275-229">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="a8275-230">Ajouter plusieurs gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="a8275-230">Add multiple Polly handlers</span></span>

<span data-ttu-id="a8275-231">Il est courant d’imbriquer les stratégies Polly :</span><span class="sxs-lookup"><span data-stu-id="a8275-231">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="a8275-232">Dans l'exemple précédent :</span><span class="sxs-lookup"><span data-stu-id="a8275-232">In the preceding example:</span></span>

* <span data-ttu-id="a8275-233">Deux gestionnaires sont ajoutés.</span><span class="sxs-lookup"><span data-stu-id="a8275-233">Two handlers are added.</span></span>
* <span data-ttu-id="a8275-234">Le premier gestionnaire utilise <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> pour ajouter une stratégie de nouvelle tentative.</span><span class="sxs-lookup"><span data-stu-id="a8275-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="a8275-235">Les requêtes qui ont échoué sont retentées jusqu’à trois fois.</span><span class="sxs-lookup"><span data-stu-id="a8275-235">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="a8275-236">Le deuxième `AddTransientHttpErrorPolicy` appel ajoute une stratégie de disjoncteur.</span><span class="sxs-lookup"><span data-stu-id="a8275-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="a8275-237">Les demandes externes supplémentaires sont bloquées pendant 30 secondes si 5 tentatives ayant échoué se produisent de façon séquentielle.</span><span class="sxs-lookup"><span data-stu-id="a8275-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="a8275-238">Les stratégies de disjoncteur sont avec état.</span><span class="sxs-lookup"><span data-stu-id="a8275-238">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="a8275-239">Tous les appels effectués via ce client partagent le même état du circuit.</span><span class="sxs-lookup"><span data-stu-id="a8275-239">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="a8275-240">Ajouter des stratégies à partir du Registre Polly</span><span class="sxs-lookup"><span data-stu-id="a8275-240">Add policies from the Polly registry</span></span>

<span data-ttu-id="a8275-241">Une approche de la gestion des stratégies régulièrement utilisées consiste à les définir une seule fois et à les inscrire avec un `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="a8275-241">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="a8275-242">Dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a8275-242">In the following code:</span></span>

* <span data-ttu-id="a8275-243">Les stratégies « régulières » et « longues » sont ajoutées.</span><span class="sxs-lookup"><span data-stu-id="a8275-243">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="a8275-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*> ajoute les stratégies « régulières » et « longues » à partir du Registre.</span><span class="sxs-lookup"><span data-stu-id="a8275-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="a8275-245">Pour plus d’informations sur les intégrations de `IHttpClientFactory` et Polly, consultez le [wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="a8275-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="a8275-246">HttpClient et gestion de la durée de vie</span><span class="sxs-lookup"><span data-stu-id="a8275-246">HttpClient and lifetime management</span></span>

<span data-ttu-id="a8275-247">Une nouvelle instance `HttpClient` est retournée à chaque fois que `CreateClient` est appelé sur `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="a8275-247">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="a8275-248">Un <xref:System.Net.Http.HttpMessageHandler> est créé par client nommé.</span><span class="sxs-lookup"><span data-stu-id="a8275-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="a8275-249">La fabrique gère les durées de vie des instances `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="a8275-249">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="a8275-250">`IHttpClientFactory` regroupe dans un pool les instances de `HttpMessageHandler` créées par la fabrique pour réduire la consommation des ressources.</span><span class="sxs-lookup"><span data-stu-id="a8275-250">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="a8275-251">Une instance de `HttpMessageHandler` peut être réutilisée à partir du pool quand vous créez une instance de `HttpClient` si sa durée de vie n’a pas expiré.</span><span class="sxs-lookup"><span data-stu-id="a8275-251">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="a8275-252">Le regroupement de gestionnaires en pools est souhaitable, car chaque gestionnaire gère habituellement ses propres connexions HTTP sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="a8275-252">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="a8275-253">La création de plus de gestionnaires que nécessaire peut entraîner des délais de connexion.</span><span class="sxs-lookup"><span data-stu-id="a8275-253">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="a8275-254">Certains gestionnaires maintiennent également les connexions ouvertes indéfiniment, ce qui peut empêcher le gestionnaire de réagir aux modifications DNS (Domain Name System).</span><span class="sxs-lookup"><span data-stu-id="a8275-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="a8275-255">La durée de vie par défaut d’un gestionnaire est de deux minutes.</span><span class="sxs-lookup"><span data-stu-id="a8275-255">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="a8275-256">La valeur par défaut peut être substituée pour chaque client nommé :</span><span class="sxs-lookup"><span data-stu-id="a8275-256">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="a8275-257">les instances de `HttpClient` peuvent généralement être traitées comme des objets .NET qui **ne nécessitent pas** de suppression.</span><span class="sxs-lookup"><span data-stu-id="a8275-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="a8275-258">La suppression annule les requêtes sortantes et garantit que l’instance `HttpClient` donnée ne peut pas être utilisée après avoir appelé <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="a8275-258">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="a8275-259">`IHttpClientFactory` effectue le suivi et libère les ressources utilisées par les instances `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-259">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="a8275-260">Le fait de conserver une seule instance `HttpClient` active pendant une longue durée est un modèle commun utilisé avant le lancement de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="a8275-260">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="a8275-261">Ce modèle devient inutile après la migration vers `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="a8275-261">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="a8275-262">Alternatives à IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="a8275-262">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="a8275-263">L’utilisation de `IHttpClientFactory` dans une application DI-Enabled évite les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="a8275-263">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="a8275-264">Problèmes d’épuisement des ressources par le regroupement `HttpMessageHandler` instances.</span><span class="sxs-lookup"><span data-stu-id="a8275-264">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="a8275-265">Problèmes DNS périmés par le cycle `HttpMessageHandler` instances à intervalles réguliers.</span><span class="sxs-lookup"><span data-stu-id="a8275-265">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="a8275-266">Il existe d’autres façons de résoudre les problèmes précédents à l’aide d’une instance de <xref:System.Net.Http.SocketsHttpHandler> à long terme.</span><span class="sxs-lookup"><span data-stu-id="a8275-266">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="a8275-267">Créez une instance de `SocketsHttpHandler` lorsque l’application démarre et utilisez-la pendant toute la durée de vie de l’application.</span><span class="sxs-lookup"><span data-stu-id="a8275-267">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="a8275-268">Configurez <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> à une valeur appropriée en fonction des temps d’actualisation DNS.</span><span class="sxs-lookup"><span data-stu-id="a8275-268">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="a8275-269">Créez `HttpClient` instances à l’aide d' `new HttpClient(handler, disposeHandler: false)` en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="a8275-269">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="a8275-270">Les approches précédentes résolvent les problèmes de gestion des ressources qui `IHttpClientFactory` résolus de la même façon.</span><span class="sxs-lookup"><span data-stu-id="a8275-270">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="a8275-271">Le `SocketsHttpHandler` partage les connexions entre les instances de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-271">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="a8275-272">Ce partage empêche l’épuisement des sockets.</span><span class="sxs-lookup"><span data-stu-id="a8275-272">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="a8275-273">Le `SocketsHttpHandler` cycle les connexions en fonction des `PooledConnectionLifetime` afin d’éviter les problèmes DNS périmés.</span><span class="sxs-lookup"><span data-stu-id="a8275-273">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="a8275-274">Cookies</span><span class="sxs-lookup"><span data-stu-id="a8275-274">Cookies</span></span>

<span data-ttu-id="a8275-275">Les instances de `HttpMessageHandler` regroupées entraînent le partage de `CookieContainer` objets.</span><span class="sxs-lookup"><span data-stu-id="a8275-275">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="a8275-276">Le partage d’objets `CookieContainer` imprévus aboutit souvent à un code incorrect.</span><span class="sxs-lookup"><span data-stu-id="a8275-276">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="a8275-277">Pour les applications qui nécessitent des cookies, utilisez l’une des deux opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="a8275-277">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="a8275-278">Désactivation de la gestion automatique des cookies</span><span class="sxs-lookup"><span data-stu-id="a8275-278">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="a8275-279">Éviter les `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="a8275-279">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="a8275-280">Appelez <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> pour désactiver la gestion automatique des cookies :</span><span class="sxs-lookup"><span data-stu-id="a8275-280">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="a8275-281">Journalisation</span><span class="sxs-lookup"><span data-stu-id="a8275-281">Logging</span></span>

<span data-ttu-id="a8275-282">Les clients créés via `IHttpClientFactory` enregistrent les messages de journalisation pour toutes les requêtes.</span><span class="sxs-lookup"><span data-stu-id="a8275-282">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="a8275-283">Activez le niveau d’information approprié dans la configuration de la journalisation pour voir les messages du journal par défaut.</span><span class="sxs-lookup"><span data-stu-id="a8275-283">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="a8275-284">Une journalisation supplémentaire, comme celle des en-têtes des requêtes, est incluse seulement au niveau de trace.</span><span class="sxs-lookup"><span data-stu-id="a8275-284">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="a8275-285">La catégorie de journal utilisée pour chaque client comprend le nom du client.</span><span class="sxs-lookup"><span data-stu-id="a8275-285">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="a8275-286">Un client nommé *MyNamedClient*, par exemple, journalise des messages avec la catégorie « System .net. http. httpclient ». **MyNamedClient**. LogicalHandler".</span><span class="sxs-lookup"><span data-stu-id="a8275-286">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="a8275-287">Les messages avec le suffixe *LogicalHandler* se produisent en dehors du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="a8275-287">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="a8275-288">Lors d’une requête, les messages sont journalisés avant que d’autres gestionnaires du pipeline l’aient traitée.</span><span class="sxs-lookup"><span data-stu-id="a8275-288">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="a8275-289">Lors d’une réponse, les messages sont journalisés une fois que tous les autres gestionnaires du pipeline ont reçu la réponse.</span><span class="sxs-lookup"><span data-stu-id="a8275-289">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="a8275-290">La journalisation se produit également à l’intérieur du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="a8275-290">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="a8275-291">Dans l’exemple *MyNamedClient* , ces messages sont journalisés avec la catégorie de journal « System .net. http. httpclient ». **MyNamedClient**. ClientHandler".</span><span class="sxs-lookup"><span data-stu-id="a8275-291">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="a8275-292">Pour la requête, cela se produit une fois que tous les autres gestionnaires ont été exécutés et immédiatement avant l’envoi de la demande.</span><span class="sxs-lookup"><span data-stu-id="a8275-292">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="a8275-293">Lors de la réponse, cette journalisation inclut l’état de la réponse avant qu’elle repasse à travers le pipeline de gestionnaires.</span><span class="sxs-lookup"><span data-stu-id="a8275-293">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="a8275-294">L’activation de la journalisation à l’extérieur et à l’intérieur du pipeline permet l’inspection des changements apportés par les autres gestionnaires du pipeline.</span><span class="sxs-lookup"><span data-stu-id="a8275-294">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="a8275-295">Cela peut inclure des modifications apportées aux en-têtes de demande ou au code d’état de réponse.</span><span class="sxs-lookup"><span data-stu-id="a8275-295">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="a8275-296">L’inclusion du nom du client dans la catégorie journal active le filtrage des journaux pour des clients nommés spécifiques.</span><span class="sxs-lookup"><span data-stu-id="a8275-296">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="a8275-297">Configurer le HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="a8275-297">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="a8275-298">Il peut être nécessaire de contrôler la configuration du `HttpMessageHandler` interne utilisé par un client.</span><span class="sxs-lookup"><span data-stu-id="a8275-298">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="a8275-299">Un `IHttpClientBuilder` est retourné quand vous ajoutez des clients nommés ou typés.</span><span class="sxs-lookup"><span data-stu-id="a8275-299">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="a8275-300">La méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> peut être utilisée pour définir un délégué.</span><span class="sxs-lookup"><span data-stu-id="a8275-300">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="a8275-301">Le délégué est utilisé pour créer et configurer le `HttpMessageHandler` principal utilisé par ce client :</span><span class="sxs-lookup"><span data-stu-id="a8275-301">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="a8275-302">Utiliser IHttpClientFactory dans une application console</span><span class="sxs-lookup"><span data-stu-id="a8275-302">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="a8275-303">Dans une application console, ajoutez les références de package suivantes au projet :</span><span class="sxs-lookup"><span data-stu-id="a8275-303">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="a8275-304">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="a8275-304">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="a8275-305">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="a8275-305">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="a8275-306">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="a8275-306">In the following example:</span></span>

* <span data-ttu-id="a8275-307"><xref:System.Net.Http.IHttpClientFactory> est inscrit dans le conteneur de service dans de l’[hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="a8275-307"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="a8275-308">`MyService` crée une instance de fabrique cliente à partir du service, qui est utilisée pour créer un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-308">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="a8275-309">`HttpClient` est utilisé pour récupérer une page web.</span><span class="sxs-lookup"><span data-stu-id="a8275-309">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="a8275-310">`Main` crée une étendue pour exécuter la méthode `GetPage` du service et écrire les 500 premiers caractères du contenu de la page web dans la console.</span><span class="sxs-lookup"><span data-stu-id="a8275-310">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="a8275-311">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="a8275-311">Additional resources</span></span>

* [<span data-ttu-id="a8275-312">Utiliser HttpClientFactory pour implémenter des requêtes HTTP résilientes</span><span class="sxs-lookup"><span data-stu-id="a8275-312">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="a8275-313">Implémenter de nouvelles tentatives d’appel HTTP avec interruption exponentielle avec des stratégies Polly et HttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="a8275-313">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="a8275-314">Implémenter le modèle Disjoncteur</span><span class="sxs-lookup"><span data-stu-id="a8275-314">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)
* [<span data-ttu-id="a8275-315">Comment sérialiser et désérialiser JSON dans .NET</span><span class="sxs-lookup"><span data-stu-id="a8275-315">How to serialize and deserialize JSON in .NET</span></span>](/dotnet/standard/serialization/system-text-json-how-to)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="a8275-316">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) et [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="a8275-316">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="a8275-317">Une <xref:System.Net.Http.IHttpClientFactory> peut être inscrite et utilisée pour configurer et créer des instances de <xref:System.Net.Http.HttpClient> dans une application.</span><span class="sxs-lookup"><span data-stu-id="a8275-317">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="a8275-318">Elle offre les avantages suivants :</span><span class="sxs-lookup"><span data-stu-id="a8275-318">It offers the following benefits:</span></span>

* <span data-ttu-id="a8275-319">Fournit un emplacement central pour le nommage et la configuration d’instance de `HttpClient` logiques.</span><span class="sxs-lookup"><span data-stu-id="a8275-319">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="a8275-320">Par exemple, un client *github* peut être inscrit et configuré pour accéder à [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="a8275-320">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="a8275-321">Un client par défaut peut être inscrit à d’autres fins.</span><span class="sxs-lookup"><span data-stu-id="a8275-321">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="a8275-322">Codifie le concept de middleware (intergiciel) sortant via la délégation de gestionnaires dans `HttpClient` et fournit des extensions permettant au middleware Polly d’en tirer parti.</span><span class="sxs-lookup"><span data-stu-id="a8275-322">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="a8275-323">Gère le regroupement et la durée de vie des instances de `HttpClientMessageHandler` sous-jacentes pour éviter les problèmes DNS courants qui se produisent lors de la gestion manuelle des durées de vie de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-323">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="a8275-324">Ajoute une expérience de journalisation configurable (via `ILogger`) pour toutes les requêtes envoyées via des clients créés par la fabrique.</span><span class="sxs-lookup"><span data-stu-id="a8275-324">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="a8275-325">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a8275-325">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="a8275-326">Modèles de consommation</span><span class="sxs-lookup"><span data-stu-id="a8275-326">Consumption patterns</span></span>

<span data-ttu-id="a8275-327">Vous pouvez utiliser `IHttpClientFactory` dans une application de plusieurs façons :</span><span class="sxs-lookup"><span data-stu-id="a8275-327">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="a8275-328">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="a8275-328">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="a8275-329">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="a8275-329">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="a8275-330">Clients typés</span><span class="sxs-lookup"><span data-stu-id="a8275-330">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="a8275-331">Clients générés</span><span class="sxs-lookup"><span data-stu-id="a8275-331">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="a8275-332">Aucune d’entre elles n’est meilleure qu’une autre.</span><span class="sxs-lookup"><span data-stu-id="a8275-332">None of them are strictly superior to another.</span></span> <span data-ttu-id="a8275-333">La meilleure approche dépend des contraintes de l’application.</span><span class="sxs-lookup"><span data-stu-id="a8275-333">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="a8275-334">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="a8275-334">Basic usage</span></span>

<span data-ttu-id="a8275-335">Vous pouvez inscrire la `IHttpClientFactory` en appelant la méthode d’extension `AddHttpClient` sur la `IServiceCollection`, à l’intérieur la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a8275-335">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="a8275-336">Une fois inscrit, le code peut accepter un `IHttpClientFactory` partout où des services peuvent être injectés avec [une injection de dépendance](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a8275-336">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a8275-337">La `IHttpClientFactory` peut être utilisée pour créer une instance de `HttpClient` :</span><span class="sxs-lookup"><span data-stu-id="a8275-337">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="a8275-338">L’utilisation de `IHttpClientFactory` de cette façon est un excellent moyen de refactoriser une application existante.</span><span class="sxs-lookup"><span data-stu-id="a8275-338">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="a8275-339">Elle n’a aucun impact sur la façon dont `HttpClient` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="a8275-339">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="a8275-340">Dans les endroits où les instances de `HttpClient` sont actuellement créées, remplacez ces occurrences par un appel à <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="a8275-340">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="a8275-341">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="a8275-341">Named clients</span></span>

<span data-ttu-id="a8275-342">Si une application nécessite plusieurs utilisations distinctes de `HttpClient`, chacune avec une configuration différente, une option consiste à utiliser des **clients nommés**.</span><span class="sxs-lookup"><span data-stu-id="a8275-342">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="a8275-343">La configuration d’un `HttpClient` nommé peut être spécifiée lors de l’inscription dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a8275-343">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="a8275-344">Dans le code précédent, `AddHttpClient` est appelé en fournissant le nom *github*.</span><span class="sxs-lookup"><span data-stu-id="a8275-344">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="a8275-345">Une configuration par défaut est appliquée à ce client : l’adresse de base et deux en-têtes nécessaires pour utiliser l’API GitHub.</span><span class="sxs-lookup"><span data-stu-id="a8275-345">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="a8275-346">Chaque fois que `CreateClient` est appelée, une nouvelle instance de `HttpClient` est créée et l’action de configuration est appelée.</span><span class="sxs-lookup"><span data-stu-id="a8275-346">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="a8275-347">Pour utiliser un client nommé, un paramètre de chaîne peut être passé à `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-347">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="a8275-348">Spécifiez le nom du client à créer :</span><span class="sxs-lookup"><span data-stu-id="a8275-348">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="a8275-349">Dans le code précédent, la requête n’a pas besoin de spécifier un nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="a8275-349">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="a8275-350">Elle peut simplement passer le chemin, car l’adresse de base configurée pour le client est utilisée.</span><span class="sxs-lookup"><span data-stu-id="a8275-350">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="a8275-351">Clients typés</span><span class="sxs-lookup"><span data-stu-id="a8275-351">Typed clients</span></span>

<span data-ttu-id="a8275-352">Clients typés :</span><span class="sxs-lookup"><span data-stu-id="a8275-352">Typed clients:</span></span>

* <span data-ttu-id="a8275-353">Fournissent les mêmes fonctionnalités que les clients nommés, sans qu’il soit nécessaire d’utiliser des chaînes comme clés.</span><span class="sxs-lookup"><span data-stu-id="a8275-353">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="a8275-354">Bénéficie de l’aide d’IntelliSense et du compilateur lors de l’utilisation des clients.</span><span class="sxs-lookup"><span data-stu-id="a8275-354">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="a8275-355">Fournissent un emplacement unique pour configurer et interagir avec un `HttpClient` particulier.</span><span class="sxs-lookup"><span data-stu-id="a8275-355">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="a8275-356">Par exemple, un même client typé peut être utilisé pour un point de terminaison de backend et pour encapsuler la logique relative à ce point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="a8275-356">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="a8275-357">Fonctionnent avec l’injection de dépendances et peuvent être injectés là où c’est nécessaire dans votre application.</span><span class="sxs-lookup"><span data-stu-id="a8275-357">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="a8275-358">Un client typé accepte un paramètre `HttpClient` dans son constructeur :</span><span class="sxs-lookup"><span data-stu-id="a8275-358">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="a8275-359">Dans le code précédent, la configuration est déplacée dans le client typé.</span><span class="sxs-lookup"><span data-stu-id="a8275-359">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="a8275-360">L’objet `HttpClient` est exposé en tant que propriété publique.</span><span class="sxs-lookup"><span data-stu-id="a8275-360">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="a8275-361">Il est possible de définir des méthodes d’API spécifiques qui exposent les fonctionnalités de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-361">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="a8275-362">La méthode `GetAspNetDocsIssues` encapsule le code nécessaire pour interroger et analyser les problèmes ouverts les plus récents d’un dépôt GitHub.</span><span class="sxs-lookup"><span data-stu-id="a8275-362">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="a8275-363">Pour inscrire un client typé, la méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> générique peut être utilisée dans `Startup.ConfigureServices`, en spécifiant la classe du client typé :</span><span class="sxs-lookup"><span data-stu-id="a8275-363">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="a8275-364">Le client typé est inscrit comme étant transitoire avec injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="a8275-364">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="a8275-365">Le client typé peut être injecté et utilisé directement :</span><span class="sxs-lookup"><span data-stu-id="a8275-365">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="a8275-366">Si vous préférez, vous pouvez spécifier la configuration d’un client typé lors de l’inscription dans `Startup.ConfigureServices` au lieu de le faire dans le constructeur du client typé :</span><span class="sxs-lookup"><span data-stu-id="a8275-366">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="a8275-367">Il est possible d’encapsuler entièrement le `HttpClient` dans un client typé.</span><span class="sxs-lookup"><span data-stu-id="a8275-367">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="a8275-368">Au lieu de l’exposer en tant que propriété, vous pouvez fournir des méthodes publiques qui appellent l’instance de `HttpClient` en interne.</span><span class="sxs-lookup"><span data-stu-id="a8275-368">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="a8275-369">Dans le code précédent, le `HttpClient` est stocké en tant que champ privé.</span><span class="sxs-lookup"><span data-stu-id="a8275-369">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="a8275-370">Tous les accès nécessaires pour effectuer des appels externes passent par la méthode `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="a8275-370">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="a8275-371">Clients générés</span><span class="sxs-lookup"><span data-stu-id="a8275-371">Generated clients</span></span>

<span data-ttu-id="a8275-372">`IHttpClientFactory` peut être utilisé en combinaison avec d’autres bibliothèques tierces, comme [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="a8275-372">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="a8275-373">Refit est une bibliothèque REST pour .NET.</span><span class="sxs-lookup"><span data-stu-id="a8275-373">Refit is a REST library for .NET.</span></span> <span data-ttu-id="a8275-374">Il convertit les API REST en interfaces dynamiques.</span><span class="sxs-lookup"><span data-stu-id="a8275-374">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="a8275-375">Une implémentation de l’interface est générée dynamiquement par le `RestService`, avec `HttpClient` pour faire les appels HTTP externes.</span><span class="sxs-lookup"><span data-stu-id="a8275-375">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="a8275-376">Une interface et une réponse sont définies pour représenter l’API externe et sa réponse :</span><span class="sxs-lookup"><span data-stu-id="a8275-376">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="a8275-377">Vous pouvez ajouter un client typé en utilisant Refit pour générer l’implémentation :</span><span class="sxs-lookup"><span data-stu-id="a8275-377">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="a8275-378">L’interface définie peut être utilisée quand c’est nécessaire, avec l’implémentation fournie par l’injection de dépendances et Refit :</span><span class="sxs-lookup"><span data-stu-id="a8275-378">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="a8275-379">Middleware pour les requêtes sortantes</span><span class="sxs-lookup"><span data-stu-id="a8275-379">Outgoing request middleware</span></span>

<span data-ttu-id="a8275-380">`HttpClient` intègre déjà le concept de délégation des gestionnaires qui peuvent être liés ensemble pour les requêtes HTTP sortantes.</span><span class="sxs-lookup"><span data-stu-id="a8275-380">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="a8275-381">Le `IHttpClientFactory` permet de définir facilement les gestionnaires à appliquer pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="a8275-381">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="a8275-382">Il prend en charge l’inscription et le chaînage de plusieurs gestionnaires pour créer un pipeline de middlewares pour les requêtes sortantes.</span><span class="sxs-lookup"><span data-stu-id="a8275-382">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="a8275-383">Chacun de ces gestionnaires peut effectuer un travail avant et après la requête sortante.</span><span class="sxs-lookup"><span data-stu-id="a8275-383">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="a8275-384">Ce modèle est similaire au pipeline de middlewares entrants dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8275-384">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="a8275-385">Le modèle fournit un mécanisme permettant de gérer les problèmes transversaux liés aux des requêtes HTTP, notamment la mise en cache, la gestion des erreurs, la sérialisation et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="a8275-385">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="a8275-386">Pour créer un gestionnaire, définissez une classe dérivant de <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="a8275-386">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="a8275-387">Remplacez la méthode `SendAsync` de façon à exécuter du code avant de passer la requête au gestionnaire suivant dans le pipeline :</span><span class="sxs-lookup"><span data-stu-id="a8275-387">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="a8275-388">Le code précédent définit un gestionnaire de base.</span><span class="sxs-lookup"><span data-stu-id="a8275-388">The preceding code defines a basic handler.</span></span> <span data-ttu-id="a8275-389">Il vérifie si un en-tête `X-API-KEY` a été inclus dans la requête.</span><span class="sxs-lookup"><span data-stu-id="a8275-389">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="a8275-390">Si l’en-tête est manquant, il peut éviter l’appel HTTP et retourner une réponse appropriée.</span><span class="sxs-lookup"><span data-stu-id="a8275-390">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="a8275-391">Au cours de l’inscription, un ou plusieurs gestionnaires peuvent être ajoutés à la configuration d’un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-391">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="a8275-392">Cette tâche est accomplie via des méthodes d’extension sur le <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="a8275-392">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="a8275-393">Dans le code précédent, le `ValidateHeaderHandler` est inscrit avec une injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="a8275-393">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="a8275-394">`IHttpClientFactory` crée une étendue DI distincte pour chaque gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="a8275-394">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="a8275-395">Les gestionnaires peuvent librement dépendre des services de n’importe quelle étendue.</span><span class="sxs-lookup"><span data-stu-id="a8275-395">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="a8275-396">Les services dont dépendent les gestionnaires sont supprimés lorsque le gestionnaire est supprimé.</span><span class="sxs-lookup"><span data-stu-id="a8275-396">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="a8275-397">Une fois inscrit, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> peut être appelé en passant en entrée le type pour le gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="a8275-397">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="a8275-398">Vous pouvez inscrire plusieurs gestionnaires dans l’ordre où ils doivent être exécutés.</span><span class="sxs-lookup"><span data-stu-id="a8275-398">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="a8275-399">Chaque gestionnaire wrappe le gestionnaire suivant jusqu’à ce que le dernier `HttpClientHandler` exécute la requête :</span><span class="sxs-lookup"><span data-stu-id="a8275-399">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="a8275-400">Utilisez l’une des approches suivantes pour partager l’état de chaque requête avec les gestionnaires de messages :</span><span class="sxs-lookup"><span data-stu-id="a8275-400">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="a8275-401">Passez des données dans le gestionnaire en utilisant `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="a8275-401">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="a8275-402">Utilisez `IHttpContextAccessor` pour accéder à la requête en cours.</span><span class="sxs-lookup"><span data-stu-id="a8275-402">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="a8275-403">Créez un objet de stockage `AsyncLocal` personnalisé pour passer les données.</span><span class="sxs-lookup"><span data-stu-id="a8275-403">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="a8275-404">Utiliser les gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="a8275-404">Use Polly-based handlers</span></span>

<span data-ttu-id="a8275-405">`IHttpClientFactory` s’intègre à une bibliothèque tierce très utilisée, appelée [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="a8275-405">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="a8275-406">Polly est une bibliothèque complète de gestion des erreurs transitoires et de résilience pour .NET.</span><span class="sxs-lookup"><span data-stu-id="a8275-406">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="a8275-407">Elle permet aux développeurs de formuler facilement et de façon thread-safe des stratégies, comme Retry (Nouvelle tentative), Circuit Breaker (Disjoncteur), Timeout (Délai d’attente), Bulkhead Isolation (Isolation par cloisonnement) et Fallback (Alternative de repli).</span><span class="sxs-lookup"><span data-stu-id="a8275-407">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="a8275-408">Des méthodes d’extension sont fournies pour permettre l’utilisation de stratégies Polly avec les instances configurées de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-408">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="a8275-409">Les extensions Polly :</span><span class="sxs-lookup"><span data-stu-id="a8275-409">The Polly extensions:</span></span>

* <span data-ttu-id="a8275-410">Prennent en charge l’ajout de gestionnaires Polly à des clients.</span><span class="sxs-lookup"><span data-stu-id="a8275-410">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="a8275-411">Peuvent être utilisées après l’installation du package NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="a8275-411">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="a8275-412">Ce package n’est pas inclus dans le framework partagé ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8275-412">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="a8275-413">Gérer les erreurs temporaires</span><span class="sxs-lookup"><span data-stu-id="a8275-413">Handle transient faults</span></span>

<span data-ttu-id="a8275-414">Les erreurs courantes se produisent lorsque des appels HTTP externes sont temporaires.</span><span class="sxs-lookup"><span data-stu-id="a8275-414">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="a8275-415">Une méthode d’extension pratique nommée `AddTransientHttpErrorPolicy` est incluse : elle permet de définir une stratégie pour gérer les erreurs temporaires.</span><span class="sxs-lookup"><span data-stu-id="a8275-415">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="a8275-416">Les stratégies configurées avec cette méthode d’extension gèrent `HttpRequestException`, les réponses HTTP 5xx et les réponses HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="a8275-416">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="a8275-417">L’extension `AddTransientHttpErrorPolicy` peut être utilisée dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a8275-417">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a8275-418">L’extension fournit l’accès à un objet `PolicyBuilder` configuré pour gérer les erreurs représentant une erreur temporaire possible :</span><span class="sxs-lookup"><span data-stu-id="a8275-418">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="a8275-419">Dans le code précédent, une stratégie `WaitAndRetryAsync` est définie.</span><span class="sxs-lookup"><span data-stu-id="a8275-419">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="a8275-420">Les requêtes qui ont échoué sont retentées jusqu’à trois fois avec un délai de 600 ms entre les tentatives.</span><span class="sxs-lookup"><span data-stu-id="a8275-420">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="a8275-421">Sélectionner dynamiquement des stratégies</span><span class="sxs-lookup"><span data-stu-id="a8275-421">Dynamically select policies</span></span>

<span data-ttu-id="a8275-422">Il existe d’autres méthodes d’extension que vous pouvez utiliser pour ajouter des gestionnaires Polly.</span><span class="sxs-lookup"><span data-stu-id="a8275-422">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="a8275-423">Une de ces extensions est `AddPolicyHandler`, qui a plusieurs surcharges.</span><span class="sxs-lookup"><span data-stu-id="a8275-423">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="a8275-424">Une de ces surcharges permet l’inspection de la requête lors de la définition de la stratégie à appliquer :</span><span class="sxs-lookup"><span data-stu-id="a8275-424">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="a8275-425">Dans le code précédent, si la requête sortante est un HTTP GET, un délai d’attente de 10 secondes est appliqué.</span><span class="sxs-lookup"><span data-stu-id="a8275-425">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="a8275-426">Pour toutes les autres méthodes HTTP, un délai d’attente de 30 secondes est utilisé.</span><span class="sxs-lookup"><span data-stu-id="a8275-426">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="a8275-427">Ajouter plusieurs gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="a8275-427">Add multiple Polly handlers</span></span>

<span data-ttu-id="a8275-428">Il est courant d’imbriquer des stratégies Polly pour fournir des fonctionnalités améliorées :</span><span class="sxs-lookup"><span data-stu-id="a8275-428">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="a8275-429">Dans l’exemple précédent, deux gestionnaires sont ajoutés.</span><span class="sxs-lookup"><span data-stu-id="a8275-429">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="a8275-430">Le premier utilise l’extension `AddTransientHttpErrorPolicy` pour ajouter une stratégie de nouvelle tentative.</span><span class="sxs-lookup"><span data-stu-id="a8275-430">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="a8275-431">Les requêtes qui ont échoué sont retentées jusqu’à trois fois.</span><span class="sxs-lookup"><span data-stu-id="a8275-431">Failed requests are retried up to three times.</span></span> <span data-ttu-id="a8275-432">Le deuxième appel à `AddTransientHttpErrorPolicy` ajoute une stratégie de disjoncteur.</span><span class="sxs-lookup"><span data-stu-id="a8275-432">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="a8275-433">Les requêtes externes supplémentaires sont bloquées pendant 30 secondes si cinq tentatives successives échouent.</span><span class="sxs-lookup"><span data-stu-id="a8275-433">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="a8275-434">Les stratégies de disjoncteur sont avec état.</span><span class="sxs-lookup"><span data-stu-id="a8275-434">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="a8275-435">Tous les appels effectués via ce client partagent le même état du circuit.</span><span class="sxs-lookup"><span data-stu-id="a8275-435">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="a8275-436">Ajouter des stratégies à partir du Registre Polly</span><span class="sxs-lookup"><span data-stu-id="a8275-436">Add policies from the Polly registry</span></span>

<span data-ttu-id="a8275-437">Une approche de la gestion des stratégies régulièrement utilisées consiste à les définir une seule fois et à les inscrire avec un `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="a8275-437">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="a8275-438">Il existe une méthode d’extension qui permet l’ajout d’un gestionnaire avec une stratégie du Registre :</span><span class="sxs-lookup"><span data-stu-id="a8275-438">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="a8275-439">Dans le code précédent, deux stratégies sont inscrites lorsque `PolicyRegistry` est ajouté à `ServiceCollection`.</span><span class="sxs-lookup"><span data-stu-id="a8275-439">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="a8275-440">Pour utiliser une stratégie du Registre, la méthode `AddPolicyHandlerFromRegistry` est utilisée, en passant le nom de la stratégie à appliquer.</span><span class="sxs-lookup"><span data-stu-id="a8275-440">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="a8275-441">Vous trouverez plus d’informations sur les intégrations de `IHttpClientFactory` et de Polly dans le [wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="a8275-441">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="a8275-442">HttpClient et gestion de la durée de vie</span><span class="sxs-lookup"><span data-stu-id="a8275-442">HttpClient and lifetime management</span></span>

<span data-ttu-id="a8275-443">Une nouvelle instance `HttpClient` est retournée à chaque fois que `CreateClient` est appelé sur `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="a8275-443">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="a8275-444">Il existe un <xref:System.Net.Http.HttpMessageHandler> par client nommé.</span><span class="sxs-lookup"><span data-stu-id="a8275-444">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="a8275-445">La fabrique gère les durées de vie des instances `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="a8275-445">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="a8275-446">`IHttpClientFactory` regroupe dans un pool les instances de `HttpMessageHandler` créées par la fabrique pour réduire la consommation des ressources.</span><span class="sxs-lookup"><span data-stu-id="a8275-446">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="a8275-447">Une instance de `HttpMessageHandler` peut être réutilisée à partir du pool quand vous créez une instance de `HttpClient` si sa durée de vie n’a pas expiré.</span><span class="sxs-lookup"><span data-stu-id="a8275-447">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="a8275-448">Le regroupement de gestionnaires en pools est souhaitable, car chaque gestionnaire gère habituellement ses propres connexions HTTP sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="a8275-448">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="a8275-449">La création de plus de gestionnaires que nécessaire peut entraîner des délais de connexion.</span><span class="sxs-lookup"><span data-stu-id="a8275-449">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="a8275-450">Certains gestionnaires conservent aussi les connexions indéfiniment ouvertes, ce qui peut empêcher le gestionnaire de réagir aux changements du DNS.</span><span class="sxs-lookup"><span data-stu-id="a8275-450">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="a8275-451">La durée de vie par défaut d’un gestionnaire est de deux minutes.</span><span class="sxs-lookup"><span data-stu-id="a8275-451">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="a8275-452">La valeur par défaut peut être remplacée pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="a8275-452">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="a8275-453">Pour la remplacer, appelez <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> sur le `IHttpClientBuilder` qui est retourné lors de la création du client :</span><span class="sxs-lookup"><span data-stu-id="a8275-453">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="a8275-454">La suppression du client n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="a8275-454">Disposal of the client isn't required.</span></span> <span data-ttu-id="a8275-455">La suppression annule les requêtes sortantes et garantit que l’instance `HttpClient` donnée ne peut pas être utilisée après avoir appelé <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="a8275-455">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="a8275-456">`IHttpClientFactory` effectue le suivi et libère les ressources utilisées par les instances `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-456">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="a8275-457">Les instances `HttpClient` peuvent généralement être traitées en tant qu’objets .NET ne nécessitant pas une suppression.</span><span class="sxs-lookup"><span data-stu-id="a8275-457">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="a8275-458">Le fait de conserver une seule instance `HttpClient` active pendant une longue durée est un modèle commun utilisé avant le lancement de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="a8275-458">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="a8275-459">Ce modèle devient inutile après la migration vers `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="a8275-459">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="a8275-460">Alternatives à IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="a8275-460">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="a8275-461">L’utilisation de `IHttpClientFactory` dans une application DI-Enabled évite les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="a8275-461">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="a8275-462">Problèmes d’épuisement des ressources par le regroupement `HttpMessageHandler` instances.</span><span class="sxs-lookup"><span data-stu-id="a8275-462">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="a8275-463">Problèmes DNS périmés par le cycle `HttpMessageHandler` instances à intervalles réguliers.</span><span class="sxs-lookup"><span data-stu-id="a8275-463">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="a8275-464">Il existe d’autres façons de résoudre les problèmes précédents à l’aide d’une instance de <xref:System.Net.Http.SocketsHttpHandler> à long terme.</span><span class="sxs-lookup"><span data-stu-id="a8275-464">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="a8275-465">Créez une instance de `SocketsHttpHandler` lorsque l’application démarre et utilisez-la pendant toute la durée de vie de l’application.</span><span class="sxs-lookup"><span data-stu-id="a8275-465">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="a8275-466">Configurez <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> à une valeur appropriée en fonction des temps d’actualisation DNS.</span><span class="sxs-lookup"><span data-stu-id="a8275-466">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="a8275-467">Créez `HttpClient` instances à l’aide d' `new HttpClient(handler, disposeHandler: false)` en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="a8275-467">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="a8275-468">Les approches précédentes résolvent les problèmes de gestion des ressources qui `IHttpClientFactory` résolus de la même façon.</span><span class="sxs-lookup"><span data-stu-id="a8275-468">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="a8275-469">Le `SocketsHttpHandler` partage les connexions entre les instances de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-469">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="a8275-470">Ce partage empêche l’épuisement des sockets.</span><span class="sxs-lookup"><span data-stu-id="a8275-470">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="a8275-471">Le `SocketsHttpHandler` cycle les connexions en fonction des `PooledConnectionLifetime` afin d’éviter les problèmes DNS périmés.</span><span class="sxs-lookup"><span data-stu-id="a8275-471">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="a8275-472">Cookies</span><span class="sxs-lookup"><span data-stu-id="a8275-472">Cookies</span></span>

<span data-ttu-id="a8275-473">Les instances de `HttpMessageHandler` regroupées entraînent le partage de `CookieContainer` objets.</span><span class="sxs-lookup"><span data-stu-id="a8275-473">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="a8275-474">Le partage d’objets `CookieContainer` imprévus aboutit souvent à un code incorrect.</span><span class="sxs-lookup"><span data-stu-id="a8275-474">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="a8275-475">Pour les applications qui nécessitent des cookies, utilisez l’une des deux opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="a8275-475">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="a8275-476">Désactivation de la gestion automatique des cookies</span><span class="sxs-lookup"><span data-stu-id="a8275-476">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="a8275-477">Éviter les `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="a8275-477">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="a8275-478">Appelez <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> pour désactiver la gestion automatique des cookies :</span><span class="sxs-lookup"><span data-stu-id="a8275-478">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="a8275-479">Journalisation</span><span class="sxs-lookup"><span data-stu-id="a8275-479">Logging</span></span>

<span data-ttu-id="a8275-480">Les clients créés via `IHttpClientFactory` enregistrent les messages de journalisation pour toutes les requêtes.</span><span class="sxs-lookup"><span data-stu-id="a8275-480">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="a8275-481">Activez le niveau d’informations approprié dans votre configuration de journalisation pour voir les messages de journalisation par défaut.</span><span class="sxs-lookup"><span data-stu-id="a8275-481">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="a8275-482">Une journalisation supplémentaire, comme celle des en-têtes des requêtes, est incluse seulement au niveau de trace.</span><span class="sxs-lookup"><span data-stu-id="a8275-482">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="a8275-483">La catégorie de journal utilisée pour chaque client comprend le nom du client.</span><span class="sxs-lookup"><span data-stu-id="a8275-483">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="a8275-484">Par exemple, un client nommé *MyNamedClient* journalise les messages avec la catégorie `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="a8275-484">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="a8275-485">Les messages avec le suffixe *LogicalHandler* se produisent en dehors du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="a8275-485">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="a8275-486">Lors d’une requête, les messages sont journalisés avant que d’autres gestionnaires du pipeline l’aient traitée.</span><span class="sxs-lookup"><span data-stu-id="a8275-486">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="a8275-487">Lors d’une réponse, les messages sont journalisés une fois que tous les autres gestionnaires du pipeline ont reçu la réponse.</span><span class="sxs-lookup"><span data-stu-id="a8275-487">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="a8275-488">La journalisation se produit également à l’intérieur du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="a8275-488">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="a8275-489">Dans l’exemple *MyNamedClient*, ces messages sont journalisés avec la catégorie de journalisation `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="a8275-489">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="a8275-490">Pour la requête, cela se produit après que tous les autres gestionnaires ont été exécutés et immédiatement avant l’envoi de la requête sur le réseau.</span><span class="sxs-lookup"><span data-stu-id="a8275-490">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="a8275-491">Lors de la réponse, cette journalisation inclut l’état de la réponse avant qu’elle repasse à travers le pipeline de gestionnaires.</span><span class="sxs-lookup"><span data-stu-id="a8275-491">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="a8275-492">L’activation de la journalisation à l’extérieur et à l’intérieur du pipeline permet l’inspection des changements apportés par les autres gestionnaires du pipeline.</span><span class="sxs-lookup"><span data-stu-id="a8275-492">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="a8275-493">Par exemple, cela peut comprendre des changements apportés aux en-têtes des requêtes ou au code d’état de la réponse.</span><span class="sxs-lookup"><span data-stu-id="a8275-493">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="a8275-494">L’ajout du nom du client dans la catégorie de journalisation permet si nécessaire de filtrer le journal pour des clients nommés spécifiques.</span><span class="sxs-lookup"><span data-stu-id="a8275-494">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="a8275-495">Configurer le HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="a8275-495">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="a8275-496">Il peut être nécessaire de contrôler la configuration du `HttpMessageHandler` interne utilisé par un client.</span><span class="sxs-lookup"><span data-stu-id="a8275-496">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="a8275-497">Un `IHttpClientBuilder` est retourné quand vous ajoutez des clients nommés ou typés.</span><span class="sxs-lookup"><span data-stu-id="a8275-497">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="a8275-498">La méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> peut être utilisée pour définir un délégué.</span><span class="sxs-lookup"><span data-stu-id="a8275-498">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="a8275-499">Le délégué est utilisé pour créer et configurer le `HttpMessageHandler` principal utilisé par ce client :</span><span class="sxs-lookup"><span data-stu-id="a8275-499">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="a8275-500">Utiliser IHttpClientFactory dans une application console</span><span class="sxs-lookup"><span data-stu-id="a8275-500">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="a8275-501">Dans une application console, ajoutez les références de package suivantes au projet :</span><span class="sxs-lookup"><span data-stu-id="a8275-501">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="a8275-502">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="a8275-502">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="a8275-503">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="a8275-503">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="a8275-504">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="a8275-504">In the following example:</span></span>

* <span data-ttu-id="a8275-505"><xref:System.Net.Http.IHttpClientFactory> est inscrit dans le conteneur de service dans de l’[hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="a8275-505"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="a8275-506">`MyService` crée une instance de fabrique cliente à partir du service, qui est utilisée pour créer un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-506">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="a8275-507">`HttpClient` est utilisé pour récupérer une page web.</span><span class="sxs-lookup"><span data-stu-id="a8275-507">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="a8275-508">`Main` crée une étendue pour exécuter la méthode `GetPage` du service et écrire les 500 premiers caractères du contenu de la page web dans la console.</span><span class="sxs-lookup"><span data-stu-id="a8275-508">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="a8275-509">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="a8275-509">Additional resources</span></span>

* [<span data-ttu-id="a8275-510">Utiliser HttpClientFactory pour implémenter des requêtes HTTP résilientes</span><span class="sxs-lookup"><span data-stu-id="a8275-510">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="a8275-511">Implémenter de nouvelles tentatives d’appel HTTP avec interruption exponentielle avec des stratégies Polly et HttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="a8275-511">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="a8275-512">Implémenter le modèle Disjoncteur</span><span class="sxs-lookup"><span data-stu-id="a8275-512">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="a8275-513">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) et [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="a8275-513">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="a8275-514">Une <xref:System.Net.Http.IHttpClientFactory> peut être inscrite et utilisée pour configurer et créer des instances de <xref:System.Net.Http.HttpClient> dans une application.</span><span class="sxs-lookup"><span data-stu-id="a8275-514">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="a8275-515">Elle offre les avantages suivants :</span><span class="sxs-lookup"><span data-stu-id="a8275-515">It offers the following benefits:</span></span>

* <span data-ttu-id="a8275-516">Fournit un emplacement central pour le nommage et la configuration d’instance de `HttpClient` logiques.</span><span class="sxs-lookup"><span data-stu-id="a8275-516">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="a8275-517">Par exemple, un client *github* peut être inscrit et configuré pour accéder à [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="a8275-517">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="a8275-518">Un client par défaut peut être inscrit à d’autres fins.</span><span class="sxs-lookup"><span data-stu-id="a8275-518">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="a8275-519">Codifie le concept de middleware (intergiciel) sortant via la délégation de gestionnaires dans `HttpClient` et fournit des extensions permettant au middleware Polly d’en tirer parti.</span><span class="sxs-lookup"><span data-stu-id="a8275-519">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="a8275-520">Gère le regroupement et la durée de vie des instances de `HttpClientMessageHandler` sous-jacentes pour éviter les problèmes DNS courants qui se produisent lors de la gestion manuelle des durées de vie de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-520">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="a8275-521">Ajoute une expérience de journalisation configurable (via `ILogger`) pour toutes les requêtes envoyées via des clients créés par la fabrique.</span><span class="sxs-lookup"><span data-stu-id="a8275-521">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="a8275-522">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a8275-522">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a8275-523">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="a8275-523">Prerequisites</span></span>

<span data-ttu-id="a8275-524">Les projets ciblant .NET Framework nécessitent l’installation du package NuGet [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/).</span><span class="sxs-lookup"><span data-stu-id="a8275-524">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="a8275-525">Les projets qui ciblent .NET Core et référencent le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) incluent déjà le package `Microsoft.Extensions.Http`.</span><span class="sxs-lookup"><span data-stu-id="a8275-525">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="a8275-526">Modèles de consommation</span><span class="sxs-lookup"><span data-stu-id="a8275-526">Consumption patterns</span></span>

<span data-ttu-id="a8275-527">Vous pouvez utiliser `IHttpClientFactory` dans une application de plusieurs façons :</span><span class="sxs-lookup"><span data-stu-id="a8275-527">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="a8275-528">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="a8275-528">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="a8275-529">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="a8275-529">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="a8275-530">Clients typés</span><span class="sxs-lookup"><span data-stu-id="a8275-530">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="a8275-531">Clients générés</span><span class="sxs-lookup"><span data-stu-id="a8275-531">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="a8275-532">Aucune d’entre elles n’est meilleure qu’une autre.</span><span class="sxs-lookup"><span data-stu-id="a8275-532">None of them are strictly superior to another.</span></span> <span data-ttu-id="a8275-533">La meilleure approche dépend des contraintes de l’application.</span><span class="sxs-lookup"><span data-stu-id="a8275-533">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="a8275-534">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="a8275-534">Basic usage</span></span>

<span data-ttu-id="a8275-535">Vous pouvez inscrire la `IHttpClientFactory` en appelant la méthode d’extension `AddHttpClient` sur la `IServiceCollection`, à l’intérieur la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a8275-535">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="a8275-536">Une fois inscrit, le code peut accepter un `IHttpClientFactory` partout où des services peuvent être injectés avec [une injection de dépendance](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a8275-536">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a8275-537">La `IHttpClientFactory` peut être utilisée pour créer une instance de `HttpClient` :</span><span class="sxs-lookup"><span data-stu-id="a8275-537">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="a8275-538">L’utilisation de `IHttpClientFactory` de cette façon est un excellent moyen de refactoriser une application existante.</span><span class="sxs-lookup"><span data-stu-id="a8275-538">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="a8275-539">Elle n’a aucun impact sur la façon dont `HttpClient` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="a8275-539">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="a8275-540">Dans les endroits où les instances de `HttpClient` sont actuellement créées, remplacez ces occurrences par un appel à <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="a8275-540">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="a8275-541">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="a8275-541">Named clients</span></span>

<span data-ttu-id="a8275-542">Si une application nécessite plusieurs utilisations distinctes de `HttpClient`, chacune avec une configuration différente, une option consiste à utiliser des **clients nommés**.</span><span class="sxs-lookup"><span data-stu-id="a8275-542">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="a8275-543">La configuration d’un `HttpClient` nommé peut être spécifiée lors de l’inscription dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a8275-543">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="a8275-544">Dans le code précédent, `AddHttpClient` est appelé en fournissant le nom *github*.</span><span class="sxs-lookup"><span data-stu-id="a8275-544">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="a8275-545">Une configuration par défaut est appliquée à ce client : l’adresse de base et deux en-têtes nécessaires pour utiliser l’API GitHub.</span><span class="sxs-lookup"><span data-stu-id="a8275-545">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="a8275-546">Chaque fois que `CreateClient` est appelée, une nouvelle instance de `HttpClient` est créée et l’action de configuration est appelée.</span><span class="sxs-lookup"><span data-stu-id="a8275-546">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="a8275-547">Pour utiliser un client nommé, un paramètre de chaîne peut être passé à `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-547">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="a8275-548">Spécifiez le nom du client à créer :</span><span class="sxs-lookup"><span data-stu-id="a8275-548">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="a8275-549">Dans le code précédent, la requête n’a pas besoin de spécifier un nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="a8275-549">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="a8275-550">Elle peut simplement passer le chemin, car l’adresse de base configurée pour le client est utilisée.</span><span class="sxs-lookup"><span data-stu-id="a8275-550">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="a8275-551">Clients typés</span><span class="sxs-lookup"><span data-stu-id="a8275-551">Typed clients</span></span>

<span data-ttu-id="a8275-552">Clients typés :</span><span class="sxs-lookup"><span data-stu-id="a8275-552">Typed clients:</span></span>

* <span data-ttu-id="a8275-553">Fournissent les mêmes fonctionnalités que les clients nommés, sans qu’il soit nécessaire d’utiliser des chaînes comme clés.</span><span class="sxs-lookup"><span data-stu-id="a8275-553">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="a8275-554">Bénéficie de l’aide d’IntelliSense et du compilateur lors de l’utilisation des clients.</span><span class="sxs-lookup"><span data-stu-id="a8275-554">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="a8275-555">Fournissent un emplacement unique pour configurer et interagir avec un `HttpClient` particulier.</span><span class="sxs-lookup"><span data-stu-id="a8275-555">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="a8275-556">Par exemple, un même client typé peut être utilisé pour un point de terminaison de backend et pour encapsuler la logique relative à ce point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="a8275-556">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="a8275-557">Fonctionnent avec l’injection de dépendances et peuvent être injectés là où c’est nécessaire dans votre application.</span><span class="sxs-lookup"><span data-stu-id="a8275-557">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="a8275-558">Un client typé accepte un paramètre `HttpClient` dans son constructeur :</span><span class="sxs-lookup"><span data-stu-id="a8275-558">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="a8275-559">Dans le code précédent, la configuration est déplacée dans le client typé.</span><span class="sxs-lookup"><span data-stu-id="a8275-559">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="a8275-560">L’objet `HttpClient` est exposé en tant que propriété publique.</span><span class="sxs-lookup"><span data-stu-id="a8275-560">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="a8275-561">Il est possible de définir des méthodes d’API spécifiques qui exposent les fonctionnalités de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-561">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="a8275-562">La méthode `GetAspNetDocsIssues` encapsule le code nécessaire pour interroger et analyser les problèmes ouverts les plus récents d’un dépôt GitHub.</span><span class="sxs-lookup"><span data-stu-id="a8275-562">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="a8275-563">Pour inscrire un client typé, la méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> générique peut être utilisée dans `Startup.ConfigureServices`, en spécifiant la classe du client typé :</span><span class="sxs-lookup"><span data-stu-id="a8275-563">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="a8275-564">Le client typé est inscrit comme étant transitoire avec injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="a8275-564">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="a8275-565">Le client typé peut être injecté et utilisé directement :</span><span class="sxs-lookup"><span data-stu-id="a8275-565">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="a8275-566">Si vous préférez, vous pouvez spécifier la configuration d’un client typé lors de l’inscription dans `Startup.ConfigureServices` au lieu de le faire dans le constructeur du client typé :</span><span class="sxs-lookup"><span data-stu-id="a8275-566">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="a8275-567">Il est possible d’encapsuler entièrement le `HttpClient` dans un client typé.</span><span class="sxs-lookup"><span data-stu-id="a8275-567">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="a8275-568">Au lieu de l’exposer en tant que propriété, vous pouvez fournir des méthodes publiques qui appellent l’instance de `HttpClient` en interne.</span><span class="sxs-lookup"><span data-stu-id="a8275-568">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="a8275-569">Dans le code précédent, le `HttpClient` est stocké en tant que champ privé.</span><span class="sxs-lookup"><span data-stu-id="a8275-569">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="a8275-570">Tous les accès nécessaires pour effectuer des appels externes passent par la méthode `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="a8275-570">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="a8275-571">Clients générés</span><span class="sxs-lookup"><span data-stu-id="a8275-571">Generated clients</span></span>

<span data-ttu-id="a8275-572">`IHttpClientFactory` peut être utilisé en combinaison avec d’autres bibliothèques tierces, comme [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="a8275-572">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="a8275-573">Refit est une bibliothèque REST pour .NET.</span><span class="sxs-lookup"><span data-stu-id="a8275-573">Refit is a REST library for .NET.</span></span> <span data-ttu-id="a8275-574">Il convertit les API REST en interfaces dynamiques.</span><span class="sxs-lookup"><span data-stu-id="a8275-574">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="a8275-575">Une implémentation de l’interface est générée dynamiquement par le `RestService`, avec `HttpClient` pour faire les appels HTTP externes.</span><span class="sxs-lookup"><span data-stu-id="a8275-575">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="a8275-576">Une interface et une réponse sont définies pour représenter l’API externe et sa réponse :</span><span class="sxs-lookup"><span data-stu-id="a8275-576">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="a8275-577">Vous pouvez ajouter un client typé en utilisant Refit pour générer l’implémentation :</span><span class="sxs-lookup"><span data-stu-id="a8275-577">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="a8275-578">L’interface définie peut être utilisée quand c’est nécessaire, avec l’implémentation fournie par l’injection de dépendances et Refit :</span><span class="sxs-lookup"><span data-stu-id="a8275-578">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="a8275-579">Middleware pour les requêtes sortantes</span><span class="sxs-lookup"><span data-stu-id="a8275-579">Outgoing request middleware</span></span>

<span data-ttu-id="a8275-580">`HttpClient` intègre déjà le concept de délégation des gestionnaires qui peuvent être liés ensemble pour les requêtes HTTP sortantes.</span><span class="sxs-lookup"><span data-stu-id="a8275-580">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="a8275-581">Le `IHttpClientFactory` permet de définir facilement les gestionnaires à appliquer pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="a8275-581">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="a8275-582">Il prend en charge l’inscription et le chaînage de plusieurs gestionnaires pour créer un pipeline de middlewares pour les requêtes sortantes.</span><span class="sxs-lookup"><span data-stu-id="a8275-582">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="a8275-583">Chacun de ces gestionnaires peut effectuer un travail avant et après la requête sortante.</span><span class="sxs-lookup"><span data-stu-id="a8275-583">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="a8275-584">Ce modèle est similaire au pipeline de middlewares entrants dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8275-584">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="a8275-585">Le modèle fournit un mécanisme permettant de gérer les problèmes transversaux liés aux des requêtes HTTP, notamment la mise en cache, la gestion des erreurs, la sérialisation et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="a8275-585">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="a8275-586">Pour créer un gestionnaire, définissez une classe dérivant de <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="a8275-586">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="a8275-587">Remplacez la méthode `SendAsync` de façon à exécuter du code avant de passer la requête au gestionnaire suivant dans le pipeline :</span><span class="sxs-lookup"><span data-stu-id="a8275-587">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="a8275-588">Le code précédent définit un gestionnaire de base.</span><span class="sxs-lookup"><span data-stu-id="a8275-588">The preceding code defines a basic handler.</span></span> <span data-ttu-id="a8275-589">Il vérifie si un en-tête `X-API-KEY` a été inclus dans la requête.</span><span class="sxs-lookup"><span data-stu-id="a8275-589">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="a8275-590">Si l’en-tête est manquant, il peut éviter l’appel HTTP et retourner une réponse appropriée.</span><span class="sxs-lookup"><span data-stu-id="a8275-590">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="a8275-591">Au cours de l’inscription, un ou plusieurs gestionnaires peuvent être ajoutés à la configuration d’un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-591">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="a8275-592">Cette tâche est accomplie via des méthodes d’extension sur le <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="a8275-592">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="a8275-593">Dans le code précédent, le `ValidateHeaderHandler` est inscrit avec une injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="a8275-593">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="a8275-594">Le gestionnaire **doit** être inscrit dans l’injection de dépendances en tant que service temporaire, sans étendue.</span><span class="sxs-lookup"><span data-stu-id="a8275-594">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="a8275-595">Si le gestionnaire est inscrit en tant que service étendu et que des services dont dépend le gestionnaire peuvent être supprimés :</span><span class="sxs-lookup"><span data-stu-id="a8275-595">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="a8275-596">Les services du gestionnaire peuvent être supprimés avant que le gestionnaire ne soit hors de portée.</span><span class="sxs-lookup"><span data-stu-id="a8275-596">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="a8275-597">Les services du gestionnaire supprimés entraînent un échec du gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="a8275-597">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="a8275-598">Une fois inscrit, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> peut être appelé en passant en entrée le type du gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="a8275-598">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="a8275-599">Vous pouvez inscrire plusieurs gestionnaires dans l’ordre où ils doivent être exécutés.</span><span class="sxs-lookup"><span data-stu-id="a8275-599">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="a8275-600">Chaque gestionnaire wrappe le gestionnaire suivant jusqu’à ce que le dernier `HttpClientHandler` exécute la requête :</span><span class="sxs-lookup"><span data-stu-id="a8275-600">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="a8275-601">Utilisez l’une des approches suivantes pour partager l’état de chaque requête avec les gestionnaires de messages :</span><span class="sxs-lookup"><span data-stu-id="a8275-601">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="a8275-602">Passez des données dans le gestionnaire en utilisant `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="a8275-602">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="a8275-603">Utilisez `IHttpContextAccessor` pour accéder à la requête en cours.</span><span class="sxs-lookup"><span data-stu-id="a8275-603">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="a8275-604">Créez un objet de stockage `AsyncLocal` personnalisé pour passer les données.</span><span class="sxs-lookup"><span data-stu-id="a8275-604">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="a8275-605">Utiliser les gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="a8275-605">Use Polly-based handlers</span></span>

<span data-ttu-id="a8275-606">`IHttpClientFactory` s’intègre à une bibliothèque tierce très utilisée, appelée [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="a8275-606">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="a8275-607">Polly est une bibliothèque complète de gestion des erreurs transitoires et de résilience pour .NET.</span><span class="sxs-lookup"><span data-stu-id="a8275-607">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="a8275-608">Elle permet aux développeurs de formuler facilement et de façon thread-safe des stratégies, comme Retry (Nouvelle tentative), Circuit Breaker (Disjoncteur), Timeout (Délai d’attente), Bulkhead Isolation (Isolation par cloisonnement) et Fallback (Alternative de repli).</span><span class="sxs-lookup"><span data-stu-id="a8275-608">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="a8275-609">Des méthodes d’extension sont fournies pour permettre l’utilisation de stratégies Polly avec les instances configurées de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-609">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="a8275-610">Les extensions Polly :</span><span class="sxs-lookup"><span data-stu-id="a8275-610">The Polly extensions:</span></span>

* <span data-ttu-id="a8275-611">Prennent en charge l’ajout de gestionnaires Polly à des clients.</span><span class="sxs-lookup"><span data-stu-id="a8275-611">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="a8275-612">Peuvent être utilisées après l’installation du package NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="a8275-612">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="a8275-613">Ce package n’est pas inclus dans le framework partagé ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8275-613">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="a8275-614">Gérer les erreurs temporaires</span><span class="sxs-lookup"><span data-stu-id="a8275-614">Handle transient faults</span></span>

<span data-ttu-id="a8275-615">Les erreurs courantes se produisent lorsque des appels HTTP externes sont temporaires.</span><span class="sxs-lookup"><span data-stu-id="a8275-615">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="a8275-616">Une méthode d’extension pratique nommée `AddTransientHttpErrorPolicy` est incluse : elle permet de définir une stratégie pour gérer les erreurs temporaires.</span><span class="sxs-lookup"><span data-stu-id="a8275-616">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="a8275-617">Les stratégies configurées avec cette méthode d’extension gèrent `HttpRequestException`, les réponses HTTP 5xx et les réponses HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="a8275-617">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="a8275-618">L’extension `AddTransientHttpErrorPolicy` peut être utilisée dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a8275-618">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a8275-619">L’extension fournit l’accès à un objet `PolicyBuilder` configuré pour gérer les erreurs représentant une erreur temporaire possible :</span><span class="sxs-lookup"><span data-stu-id="a8275-619">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="a8275-620">Dans le code précédent, une stratégie `WaitAndRetryAsync` est définie.</span><span class="sxs-lookup"><span data-stu-id="a8275-620">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="a8275-621">Les requêtes qui ont échoué sont retentées jusqu’à trois fois avec un délai de 600 ms entre les tentatives.</span><span class="sxs-lookup"><span data-stu-id="a8275-621">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="a8275-622">Sélectionner dynamiquement des stratégies</span><span class="sxs-lookup"><span data-stu-id="a8275-622">Dynamically select policies</span></span>

<span data-ttu-id="a8275-623">Il existe d’autres méthodes d’extension que vous pouvez utiliser pour ajouter des gestionnaires Polly.</span><span class="sxs-lookup"><span data-stu-id="a8275-623">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="a8275-624">Une de ces extensions est `AddPolicyHandler`, qui a plusieurs surcharges.</span><span class="sxs-lookup"><span data-stu-id="a8275-624">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="a8275-625">Une de ces surcharges permet l’inspection de la requête lors de la définition de la stratégie à appliquer :</span><span class="sxs-lookup"><span data-stu-id="a8275-625">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="a8275-626">Dans le code précédent, si la requête sortante est un HTTP GET, un délai d’attente de 10 secondes est appliqué.</span><span class="sxs-lookup"><span data-stu-id="a8275-626">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="a8275-627">Pour toutes les autres méthodes HTTP, un délai d’attente de 30 secondes est utilisé.</span><span class="sxs-lookup"><span data-stu-id="a8275-627">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="a8275-628">Ajouter plusieurs gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="a8275-628">Add multiple Polly handlers</span></span>

<span data-ttu-id="a8275-629">Il est courant d’imbriquer des stratégies Polly pour fournir des fonctionnalités améliorées :</span><span class="sxs-lookup"><span data-stu-id="a8275-629">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="a8275-630">Dans l’exemple précédent, deux gestionnaires sont ajoutés.</span><span class="sxs-lookup"><span data-stu-id="a8275-630">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="a8275-631">Le premier utilise l’extension `AddTransientHttpErrorPolicy` pour ajouter une stratégie de nouvelle tentative.</span><span class="sxs-lookup"><span data-stu-id="a8275-631">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="a8275-632">Les requêtes qui ont échoué sont retentées jusqu’à trois fois.</span><span class="sxs-lookup"><span data-stu-id="a8275-632">Failed requests are retried up to three times.</span></span> <span data-ttu-id="a8275-633">Le deuxième appel à `AddTransientHttpErrorPolicy` ajoute une stratégie de disjoncteur.</span><span class="sxs-lookup"><span data-stu-id="a8275-633">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="a8275-634">Les requêtes externes supplémentaires sont bloquées pendant 30 secondes si cinq tentatives successives échouent.</span><span class="sxs-lookup"><span data-stu-id="a8275-634">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="a8275-635">Les stratégies de disjoncteur sont avec état.</span><span class="sxs-lookup"><span data-stu-id="a8275-635">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="a8275-636">Tous les appels effectués via ce client partagent le même état du circuit.</span><span class="sxs-lookup"><span data-stu-id="a8275-636">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="a8275-637">Ajouter des stratégies à partir du Registre Polly</span><span class="sxs-lookup"><span data-stu-id="a8275-637">Add policies from the Polly registry</span></span>

<span data-ttu-id="a8275-638">Une approche de la gestion des stratégies régulièrement utilisées consiste à les définir une seule fois et à les inscrire avec un `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="a8275-638">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="a8275-639">Il existe une méthode d’extension qui permet l’ajout d’un gestionnaire avec une stratégie du Registre :</span><span class="sxs-lookup"><span data-stu-id="a8275-639">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="a8275-640">Dans le code précédent, deux stratégies sont inscrites lorsque `PolicyRegistry` est ajouté à `ServiceCollection`.</span><span class="sxs-lookup"><span data-stu-id="a8275-640">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="a8275-641">Pour utiliser une stratégie du Registre, la méthode `AddPolicyHandlerFromRegistry` est utilisée, en passant le nom de la stratégie à appliquer.</span><span class="sxs-lookup"><span data-stu-id="a8275-641">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="a8275-642">Vous trouverez plus d’informations sur les intégrations de `IHttpClientFactory` et de Polly dans le [wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="a8275-642">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="a8275-643">HttpClient et gestion de la durée de vie</span><span class="sxs-lookup"><span data-stu-id="a8275-643">HttpClient and lifetime management</span></span>

<span data-ttu-id="a8275-644">Une nouvelle instance `HttpClient` est retournée à chaque fois que `CreateClient` est appelé sur `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="a8275-644">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="a8275-645">Il existe un <xref:System.Net.Http.HttpMessageHandler> par client nommé.</span><span class="sxs-lookup"><span data-stu-id="a8275-645">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="a8275-646">La fabrique gère les durées de vie des instances `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="a8275-646">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="a8275-647">`IHttpClientFactory` regroupe dans un pool les instances de `HttpMessageHandler` créées par la fabrique pour réduire la consommation des ressources.</span><span class="sxs-lookup"><span data-stu-id="a8275-647">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="a8275-648">Une instance de `HttpMessageHandler` peut être réutilisée à partir du pool quand vous créez une instance de `HttpClient` si sa durée de vie n’a pas expiré.</span><span class="sxs-lookup"><span data-stu-id="a8275-648">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="a8275-649">Le regroupement de gestionnaires en pools est souhaitable, car chaque gestionnaire gère habituellement ses propres connexions HTTP sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="a8275-649">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="a8275-650">La création de plus de gestionnaires que nécessaire peut entraîner des délais de connexion.</span><span class="sxs-lookup"><span data-stu-id="a8275-650">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="a8275-651">Certains gestionnaires conservent aussi les connexions indéfiniment ouvertes, ce qui peut empêcher le gestionnaire de réagir aux changements du DNS.</span><span class="sxs-lookup"><span data-stu-id="a8275-651">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="a8275-652">La durée de vie par défaut d’un gestionnaire est de deux minutes.</span><span class="sxs-lookup"><span data-stu-id="a8275-652">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="a8275-653">La valeur par défaut peut être remplacée pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="a8275-653">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="a8275-654">Pour la remplacer, appelez <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> sur le `IHttpClientBuilder` qui est retourné lors de la création du client :</span><span class="sxs-lookup"><span data-stu-id="a8275-654">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="a8275-655">La suppression du client n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="a8275-655">Disposal of the client isn't required.</span></span> <span data-ttu-id="a8275-656">La suppression annule les requêtes sortantes et garantit que l’instance `HttpClient` donnée ne peut pas être utilisée après avoir appelé <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="a8275-656">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="a8275-657">`IHttpClientFactory` effectue le suivi et libère les ressources utilisées par les instances `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-657">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="a8275-658">Les instances `HttpClient` peuvent généralement être traitées en tant qu’objets .NET ne nécessitant pas une suppression.</span><span class="sxs-lookup"><span data-stu-id="a8275-658">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="a8275-659">Le fait de conserver une seule instance `HttpClient` active pendant une longue durée est un modèle commun utilisé avant le lancement de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="a8275-659">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="a8275-660">Ce modèle devient inutile après la migration vers `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="a8275-660">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="a8275-661">Alternatives à IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="a8275-661">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="a8275-662">L’utilisation de `IHttpClientFactory` dans une application DI-Enabled évite les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="a8275-662">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="a8275-663">Problèmes d’épuisement des ressources par le regroupement `HttpMessageHandler` instances.</span><span class="sxs-lookup"><span data-stu-id="a8275-663">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="a8275-664">Problèmes DNS périmés par le cycle `HttpMessageHandler` instances à intervalles réguliers.</span><span class="sxs-lookup"><span data-stu-id="a8275-664">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="a8275-665">Il existe d’autres façons de résoudre les problèmes précédents à l’aide d’une instance de <xref:System.Net.Http.SocketsHttpHandler> à long terme.</span><span class="sxs-lookup"><span data-stu-id="a8275-665">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="a8275-666">Créez une instance de `SocketsHttpHandler` lorsque l’application démarre et utilisez-la pendant toute la durée de vie de l’application.</span><span class="sxs-lookup"><span data-stu-id="a8275-666">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="a8275-667">Configurez <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> à une valeur appropriée en fonction des temps d’actualisation DNS.</span><span class="sxs-lookup"><span data-stu-id="a8275-667">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="a8275-668">Créez `HttpClient` instances à l’aide d' `new HttpClient(handler, disposeHandler: false)` en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="a8275-668">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="a8275-669">Les approches précédentes résolvent les problèmes de gestion des ressources qui `IHttpClientFactory` résolus de la même façon.</span><span class="sxs-lookup"><span data-stu-id="a8275-669">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="a8275-670">Le `SocketsHttpHandler` partage les connexions entre les instances de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-670">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="a8275-671">Ce partage empêche l’épuisement des sockets.</span><span class="sxs-lookup"><span data-stu-id="a8275-671">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="a8275-672">Le `SocketsHttpHandler` cycle les connexions en fonction des `PooledConnectionLifetime` afin d’éviter les problèmes DNS périmés.</span><span class="sxs-lookup"><span data-stu-id="a8275-672">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="a8275-673">Cookies</span><span class="sxs-lookup"><span data-stu-id="a8275-673">Cookies</span></span>

<span data-ttu-id="a8275-674">Les instances de `HttpMessageHandler` regroupées entraînent le partage de `CookieContainer` objets.</span><span class="sxs-lookup"><span data-stu-id="a8275-674">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="a8275-675">Le partage d’objets `CookieContainer` imprévus aboutit souvent à un code incorrect.</span><span class="sxs-lookup"><span data-stu-id="a8275-675">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="a8275-676">Pour les applications qui nécessitent des cookies, utilisez l’une des deux opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="a8275-676">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="a8275-677">Désactivation de la gestion automatique des cookies</span><span class="sxs-lookup"><span data-stu-id="a8275-677">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="a8275-678">Éviter les `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="a8275-678">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="a8275-679">Appelez <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> pour désactiver la gestion automatique des cookies :</span><span class="sxs-lookup"><span data-stu-id="a8275-679">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="a8275-680">Journalisation</span><span class="sxs-lookup"><span data-stu-id="a8275-680">Logging</span></span>

<span data-ttu-id="a8275-681">Les clients créés via `IHttpClientFactory` enregistrent les messages de journalisation pour toutes les requêtes.</span><span class="sxs-lookup"><span data-stu-id="a8275-681">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="a8275-682">Activez le niveau d’informations approprié dans votre configuration de journalisation pour voir les messages de journalisation par défaut.</span><span class="sxs-lookup"><span data-stu-id="a8275-682">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="a8275-683">Une journalisation supplémentaire, comme celle des en-têtes des requêtes, est incluse seulement au niveau de trace.</span><span class="sxs-lookup"><span data-stu-id="a8275-683">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="a8275-684">La catégorie de journal utilisée pour chaque client comprend le nom du client.</span><span class="sxs-lookup"><span data-stu-id="a8275-684">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="a8275-685">Par exemple, un client nommé *MyNamedClient* journalise les messages avec la catégorie `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="a8275-685">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="a8275-686">Les messages avec le suffixe *LogicalHandler* se produisent en dehors du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="a8275-686">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="a8275-687">Lors d’une requête, les messages sont journalisés avant que d’autres gestionnaires du pipeline l’aient traitée.</span><span class="sxs-lookup"><span data-stu-id="a8275-687">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="a8275-688">Lors d’une réponse, les messages sont journalisés une fois que tous les autres gestionnaires du pipeline ont reçu la réponse.</span><span class="sxs-lookup"><span data-stu-id="a8275-688">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="a8275-689">La journalisation se produit également à l’intérieur du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="a8275-689">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="a8275-690">Dans l’exemple *MyNamedClient*, ces messages sont journalisés avec la catégorie de journalisation `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="a8275-690">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="a8275-691">Pour la requête, cela se produit après que tous les autres gestionnaires ont été exécutés et immédiatement avant l’envoi de la requête sur le réseau.</span><span class="sxs-lookup"><span data-stu-id="a8275-691">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="a8275-692">Lors de la réponse, cette journalisation inclut l’état de la réponse avant qu’elle repasse à travers le pipeline de gestionnaires.</span><span class="sxs-lookup"><span data-stu-id="a8275-692">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="a8275-693">L’activation de la journalisation à l’extérieur et à l’intérieur du pipeline permet l’inspection des changements apportés par les autres gestionnaires du pipeline.</span><span class="sxs-lookup"><span data-stu-id="a8275-693">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="a8275-694">Par exemple, cela peut comprendre des changements apportés aux en-têtes des requêtes ou au code d’état de la réponse.</span><span class="sxs-lookup"><span data-stu-id="a8275-694">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="a8275-695">L’ajout du nom du client dans la catégorie de journalisation permet si nécessaire de filtrer le journal pour des clients nommés spécifiques.</span><span class="sxs-lookup"><span data-stu-id="a8275-695">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="a8275-696">Configurer le HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="a8275-696">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="a8275-697">Il peut être nécessaire de contrôler la configuration du `HttpMessageHandler` interne utilisé par un client.</span><span class="sxs-lookup"><span data-stu-id="a8275-697">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="a8275-698">Un `IHttpClientBuilder` est retourné quand vous ajoutez des clients nommés ou typés.</span><span class="sxs-lookup"><span data-stu-id="a8275-698">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="a8275-699">La méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> peut être utilisée pour définir un délégué.</span><span class="sxs-lookup"><span data-stu-id="a8275-699">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="a8275-700">Le délégué est utilisé pour créer et configurer le `HttpMessageHandler` principal utilisé par ce client :</span><span class="sxs-lookup"><span data-stu-id="a8275-700">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="a8275-701">Utiliser IHttpClientFactory dans une application console</span><span class="sxs-lookup"><span data-stu-id="a8275-701">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="a8275-702">Dans une application console, ajoutez les références de package suivantes au projet :</span><span class="sxs-lookup"><span data-stu-id="a8275-702">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="a8275-703">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="a8275-703">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="a8275-704">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="a8275-704">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="a8275-705">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="a8275-705">In the following example:</span></span>

* <span data-ttu-id="a8275-706"><xref:System.Net.Http.IHttpClientFactory> est inscrit dans le conteneur de service dans de l’[hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="a8275-706"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="a8275-707">`MyService` crée une instance de fabrique cliente à partir du service, qui est utilisée pour créer un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8275-707">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="a8275-708">`HttpClient` est utilisé pour récupérer une page web.</span><span class="sxs-lookup"><span data-stu-id="a8275-708">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="a8275-709">`Main` crée une étendue pour exécuter la méthode `GetPage` du service et écrire les 500 premiers caractères du contenu de la page web dans la console.</span><span class="sxs-lookup"><span data-stu-id="a8275-709">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="a8275-710">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="a8275-710">Additional resources</span></span>

* [<span data-ttu-id="a8275-711">Utiliser HttpClientFactory pour implémenter des requêtes HTTP résilientes</span><span class="sxs-lookup"><span data-stu-id="a8275-711">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="a8275-712">Implémenter de nouvelles tentatives d’appel HTTP avec interruption exponentielle avec des stratégies Polly et HttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="a8275-712">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="a8275-713">Implémenter le modèle Disjoncteur</span><span class="sxs-lookup"><span data-stu-id="a8275-713">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
