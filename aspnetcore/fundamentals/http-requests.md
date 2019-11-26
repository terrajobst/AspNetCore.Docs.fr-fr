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
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="e4375-103">Effectuer des requêtes HTTP en utilisant IHttpClientFactory dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e4375-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e4375-104">Par [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT)et [Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="e4375-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="e4375-105">Une <xref:System.Net.Http.IHttpClientFactory> peut être inscrite et utilisée pour configurer et créer des instances de <xref:System.Net.Http.HttpClient> dans une application.</span><span class="sxs-lookup"><span data-stu-id="e4375-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="e4375-106">`IHttpClientFactory` offre les avantages suivants :</span><span class="sxs-lookup"><span data-stu-id="e4375-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="e4375-107">Fournit un emplacement central pour le nommage et la configuration d’instance de `HttpClient` logiques.</span><span class="sxs-lookup"><span data-stu-id="e4375-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="e4375-108">Par exemple, un client nommé *GitHub* peut être inscrit et configuré pour accéder à [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="e4375-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="e4375-109">Un client par défaut peut être inscrit pour un accès général.</span><span class="sxs-lookup"><span data-stu-id="e4375-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="e4375-110">Codifie le concept d’intergiciel (middleware) sortant via la délégation de gestionnaires dans `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="e4375-111">Fournit des extensions pour l’intergiciel (middleware) basé sur Polly pour tirer parti des gestionnaires de délégation dans `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="e4375-112">Gère le regroupement et la durée de vie des instances de `HttpClientMessageHandler` sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="e4375-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="e4375-113">La gestion automatique évite les problèmes courants liés au DNS (Domain Name System) qui se produisent lors de la gestion manuelle des durées de vie de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="e4375-114">Ajoute une expérience de journalisation configurable (via `ILogger`) pour toutes les requêtes envoyées via des clients créés par la fabrique.</span><span class="sxs-lookup"><span data-stu-id="e4375-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="e4375-115">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="e4375-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="e4375-116">L’exemple de code dans cette version de rubrique utilise <xref:System.Text.Json> pour désérialiser le contenu JSON renvoyé dans les réponses HTTP.</span><span class="sxs-lookup"><span data-stu-id="e4375-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="e4375-117">Pour obtenir des exemples qui utilisent des `Json.NET` et des `ReadAsAsync<T>`, utilisez le sélecteur de version pour sélectionner une version 2. x de cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="e4375-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="e4375-118">Modèles de consommation</span><span class="sxs-lookup"><span data-stu-id="e4375-118">Consumption patterns</span></span>

<span data-ttu-id="e4375-119">Vous pouvez utiliser `IHttpClientFactory` dans une application de plusieurs façons :</span><span class="sxs-lookup"><span data-stu-id="e4375-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="e4375-120">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="e4375-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="e4375-121">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="e4375-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="e4375-122">Clients typés</span><span class="sxs-lookup"><span data-stu-id="e4375-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="e4375-123">Clients générés</span><span class="sxs-lookup"><span data-stu-id="e4375-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="e4375-124">La meilleure approche dépend des exigences de l’application.</span><span class="sxs-lookup"><span data-stu-id="e4375-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="e4375-125">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="e4375-125">Basic usage</span></span>

<span data-ttu-id="e4375-126">`IHttpClientFactory` peut être inscrit en appelant `AddHttpClient`:</span><span class="sxs-lookup"><span data-stu-id="e4375-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="e4375-127">Un `IHttpClientFactory` peut être demandé à l’aide [de l’injection de dépendances (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e4375-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e4375-128">Le code suivant utilise `IHttpClientFactory` pour créer une instance `HttpClient` :</span><span class="sxs-lookup"><span data-stu-id="e4375-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="e4375-129">L’utilisation de `IHttpClientFactory` comme dans l’exemple précédent est un bon moyen de refactoriser une application existante.</span><span class="sxs-lookup"><span data-stu-id="e4375-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="e4375-130">Elle n’a aucun impact sur l’utilisation de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="e4375-131">Dans les emplacements où les instances de `HttpClient` sont créées dans une application existante, remplacez-les par des appels à <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="e4375-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="e4375-132">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="e4375-132">Named clients</span></span>

<span data-ttu-id="e4375-133">Les clients nommés sont un bon choix lorsque :</span><span class="sxs-lookup"><span data-stu-id="e4375-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="e4375-134">L’application requiert de nombreuses utilisations distinctes de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="e4375-135">De nombreuses `HttpClient`s ont une configuration différente.</span><span class="sxs-lookup"><span data-stu-id="e4375-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="e4375-136">La configuration d’un `HttpClient` nommé peut être spécifiée lors de l’inscription dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e4375-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="e4375-137">Dans le code précédent, le client est configuré avec :</span><span class="sxs-lookup"><span data-stu-id="e4375-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="e4375-138">Adresse de base `https://api.github.com/`.</span><span class="sxs-lookup"><span data-stu-id="e4375-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="e4375-139">Deux en-têtes nécessaires à l’utilisation de l’API GitHub.</span><span class="sxs-lookup"><span data-stu-id="e4375-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="e4375-140">CreateClient</span><span class="sxs-lookup"><span data-stu-id="e4375-140">CreateClient</span></span>

<span data-ttu-id="e4375-141">Chaque fois que <xref:System.Net.Http.IHttpClientFactory.CreateClient*> est appelée :</span><span class="sxs-lookup"><span data-stu-id="e4375-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="e4375-142">Une nouvelle instance de `HttpClient` est créée.</span><span class="sxs-lookup"><span data-stu-id="e4375-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="e4375-143">L’action de configuration est appelée.</span><span class="sxs-lookup"><span data-stu-id="e4375-143">The configuration action is called.</span></span>

<span data-ttu-id="e4375-144">Pour créer un client nommé, transmettez son nom à `CreateClient`:</span><span class="sxs-lookup"><span data-stu-id="e4375-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="e4375-145">Dans le code précédent, la requête n’a pas besoin de spécifier un nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="e4375-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="e4375-146">Le code peut passer uniquement le chemin d’accès, dans la mesure où l’adresse de base configurée pour le client est utilisée.</span><span class="sxs-lookup"><span data-stu-id="e4375-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="e4375-147">Clients typés</span><span class="sxs-lookup"><span data-stu-id="e4375-147">Typed clients</span></span>

<span data-ttu-id="e4375-148">Clients typés :</span><span class="sxs-lookup"><span data-stu-id="e4375-148">Typed clients:</span></span>

* <span data-ttu-id="e4375-149">Fournissent les mêmes fonctionnalités que les clients nommés, sans qu’il soit nécessaire d’utiliser des chaînes comme clés.</span><span class="sxs-lookup"><span data-stu-id="e4375-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="e4375-150">Bénéficie de l’aide d’IntelliSense et du compilateur lors de l’utilisation des clients.</span><span class="sxs-lookup"><span data-stu-id="e4375-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="e4375-151">Fournissent un emplacement unique pour configurer et interagir avec un `HttpClient` particulier.</span><span class="sxs-lookup"><span data-stu-id="e4375-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="e4375-152">Par exemple, un seul client typé peut être utilisé :</span><span class="sxs-lookup"><span data-stu-id="e4375-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="e4375-153">Pour un seul point de terminaison backend.</span><span class="sxs-lookup"><span data-stu-id="e4375-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="e4375-154">Pour encapsuler toute la logique traitant le point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="e4375-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="e4375-155">Utilisez DI et pouvez les injecter lorsque cela est requis dans l’application.</span><span class="sxs-lookup"><span data-stu-id="e4375-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="e4375-156">Un client typé accepte un `HttpClient` dans son constructeur :</span><span class="sxs-lookup"><span data-stu-id="e4375-156">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="e4375-157">Dans le code précédent :</span><span class="sxs-lookup"><span data-stu-id="e4375-157">In the preceding code:</span></span>

* <span data-ttu-id="e4375-158">La configuration est déplacée vers le client typé.</span><span class="sxs-lookup"><span data-stu-id="e4375-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="e4375-159">L’objet `HttpClient` est exposé en tant que propriété publique.</span><span class="sxs-lookup"><span data-stu-id="e4375-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="e4375-160">Des méthodes spécifiques à l’API peuvent être créées pour exposer les fonctionnalités de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="e4375-161">Par exemple, la méthode `GetAspNetDocsIssues` encapsule du code pour récupérer des problèmes ouverts.</span><span class="sxs-lookup"><span data-stu-id="e4375-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="e4375-162">Le code suivant appelle <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> dans `Startup.ConfigureServices` pour inscrire une classe de client typée :</span><span class="sxs-lookup"><span data-stu-id="e4375-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="e4375-163">Le client typé est inscrit comme étant transitoire avec injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e4375-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="e4375-164">Le client typé peut être injecté et utilisé directement :</span><span class="sxs-lookup"><span data-stu-id="e4375-164">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="e4375-165">La configuration d’un client typé peut être spécifiée lors de l’inscription dans `Startup.ConfigureServices`, plutôt que dans le constructeur du client typé :</span><span class="sxs-lookup"><span data-stu-id="e4375-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="e4375-166">Le `HttpClient` peut être encapsulé dans un client typé.</span><span class="sxs-lookup"><span data-stu-id="e4375-166">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="e4375-167">Au lieu de l’exposer en tant que propriété, définissez une méthode qui appelle l’instance de `HttpClient` en interne :</span><span class="sxs-lookup"><span data-stu-id="e4375-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="e4375-168">Dans le code précédent, le `HttpClient` est stocké dans un champ privé.</span><span class="sxs-lookup"><span data-stu-id="e4375-168">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="e4375-169">L’accès à l' `HttpClient` est par la méthode `GetRepos` publique.</span><span class="sxs-lookup"><span data-stu-id="e4375-169">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="e4375-170">Clients générés</span><span class="sxs-lookup"><span data-stu-id="e4375-170">Generated clients</span></span>

<span data-ttu-id="e4375-171">les `IHttpClientFactory` peuvent être utilisées en association avec des bibliothèques tierces, [telles que la](https://github.com/paulcbetts/refit)réutilisation.</span><span class="sxs-lookup"><span data-stu-id="e4375-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="e4375-172">Refit est une bibliothèque REST pour .NET.</span><span class="sxs-lookup"><span data-stu-id="e4375-172">Refit is a REST library for .NET.</span></span> <span data-ttu-id="e4375-173">Il convertit les API REST en interfaces dynamiques.</span><span class="sxs-lookup"><span data-stu-id="e4375-173">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="e4375-174">Une implémentation de l’interface est générée dynamiquement par le `RestService`, avec `HttpClient` pour faire les appels HTTP externes.</span><span class="sxs-lookup"><span data-stu-id="e4375-174">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="e4375-175">Une interface et une réponse sont définies pour représenter l’API externe et sa réponse :</span><span class="sxs-lookup"><span data-stu-id="e4375-175">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="e4375-176">Vous pouvez ajouter un client typé en utilisant Refit pour générer l’implémentation :</span><span class="sxs-lookup"><span data-stu-id="e4375-176">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="e4375-177">L’interface définie peut être utilisée quand c’est nécessaire, avec l’implémentation fournie par l’injection de dépendances et Refit :</span><span class="sxs-lookup"><span data-stu-id="e4375-177">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="e4375-178">Middleware pour les requêtes sortantes</span><span class="sxs-lookup"><span data-stu-id="e4375-178">Outgoing request middleware</span></span>

<span data-ttu-id="e4375-179">`HttpClient` présente le concept de délégation de gestionnaires qui peuvent être liés entre eux pour les requêtes HTTP sortantes.</span><span class="sxs-lookup"><span data-stu-id="e4375-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="e4375-180">`IHttpClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="e4375-180">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="e4375-181">Simplifie la définition des gestionnaires à appliquer pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="e4375-181">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="e4375-182">Prend en charge l’inscription et le chaînage de plusieurs gestionnaires pour générer un pipeline d’intergiciel (middleware) de demande sortante.</span><span class="sxs-lookup"><span data-stu-id="e4375-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="e4375-183">Chacun de ces gestionnaires peut effectuer un travail avant et après la requête sortante.</span><span class="sxs-lookup"><span data-stu-id="e4375-183">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="e4375-184">Ce modèle :</span><span class="sxs-lookup"><span data-stu-id="e4375-184">This pattern:</span></span>

  * <span data-ttu-id="e4375-185">Est semblable au pipeline d’intergiciel (middleware) entrant dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e4375-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="e4375-186">Fournit un mécanisme permettant de gérer les problèmes transversaux autour des requêtes HTTP, par exemple :</span><span class="sxs-lookup"><span data-stu-id="e4375-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="e4375-187">mettre en cache</span><span class="sxs-lookup"><span data-stu-id="e4375-187">caching</span></span>
    * <span data-ttu-id="e4375-188">gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="e4375-188">error handling</span></span>
    * <span data-ttu-id="e4375-189">sérialisation</span><span class="sxs-lookup"><span data-stu-id="e4375-189">serialization</span></span>
    * <span data-ttu-id="e4375-190">enregistrer</span><span class="sxs-lookup"><span data-stu-id="e4375-190">logging</span></span>

<span data-ttu-id="e4375-191">Pour créer un gestionnaire de délégation :</span><span class="sxs-lookup"><span data-stu-id="e4375-191">To create a delegating handler:</span></span>

* <span data-ttu-id="e4375-192">Dérivez de <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="e4375-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="e4375-193">Substituez <xref:System.Net.Http.DelegatingHandler.SendAsync*></span><span class="sxs-lookup"><span data-stu-id="e4375-193">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="e4375-194">Exécutez le code avant de passer la requête au gestionnaire suivant dans le pipeline :</span><span class="sxs-lookup"><span data-stu-id="e4375-194">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="e4375-195">Le code précédent vérifie si l’en-tête `X-API-KEY` est dans la demande.</span><span class="sxs-lookup"><span data-stu-id="e4375-195">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="e4375-196">Si `X-API-KEY` est manquant, <xref:System.Net.HttpStatusCode.BadRequest> est retourné.</span><span class="sxs-lookup"><span data-stu-id="e4375-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="e4375-197">Plusieurs gestionnaires peuvent être ajoutés à la configuration d’un `HttpClient` avec <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span><span class="sxs-lookup"><span data-stu-id="e4375-197">More than one handler can be added to the configuration for a `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="e4375-198">Dans le code précédent, le `ValidateHeaderHandler` est inscrit avec une injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e4375-198">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="e4375-199">`IHttpClientFactory` crée une étendue DI distincte pour chaque gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="e4375-199">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="e4375-200">Les gestionnaires peuvent dépendre des services de toute portée.</span><span class="sxs-lookup"><span data-stu-id="e4375-200">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="e4375-201">Les services dont dépendent les gestionnaires sont supprimés lorsque le gestionnaire est supprimé.</span><span class="sxs-lookup"><span data-stu-id="e4375-201">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="e4375-202">Une fois inscrit, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> peut être appelé en passant en entrée le type pour le gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="e4375-202">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="e4375-203">Vous pouvez inscrire plusieurs gestionnaires dans l’ordre où ils doivent être exécutés.</span><span class="sxs-lookup"><span data-stu-id="e4375-203">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="e4375-204">Chaque gestionnaire wrappe le gestionnaire suivant jusqu’à ce que le dernier `HttpClientHandler` exécute la requête :</span><span class="sxs-lookup"><span data-stu-id="e4375-204">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="e4375-205">Utilisez l’une des approches suivantes pour partager l’état de chaque requête avec les gestionnaires de messages :</span><span class="sxs-lookup"><span data-stu-id="e4375-205">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="e4375-206">Transmettez les données dans le gestionnaire à l’aide de [HttpRequestMessage. Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span><span class="sxs-lookup"><span data-stu-id="e4375-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="e4375-207">Utilisez <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> pour accéder à la requête en cours.</span><span class="sxs-lookup"><span data-stu-id="e4375-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="e4375-208">Créez un objet de stockage <xref:System.Threading.AsyncLocal`1> personnalisé pour passer les données.</span><span class="sxs-lookup"><span data-stu-id="e4375-208">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="e4375-209">Utiliser les gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="e4375-209">Use Polly-based handlers</span></span>

<span data-ttu-id="e4375-210">`IHttpClientFactory` s’intègre à la bibliothèque tierce [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="e4375-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="e4375-211">Polly est une bibliothèque complète de gestion des erreurs transitoires et de résilience pour .NET.</span><span class="sxs-lookup"><span data-stu-id="e4375-211">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="e4375-212">Elle permet aux développeurs de formuler facilement et de façon thread-safe des stratégies, comme Retry (Nouvelle tentative), Circuit Breaker (Disjoncteur), Timeout (Délai d’attente), Bulkhead Isolation (Isolation par cloisonnement) et Fallback (Alternative de repli).</span><span class="sxs-lookup"><span data-stu-id="e4375-212">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="e4375-213">Des méthodes d’extension sont fournies pour permettre l’utilisation de stratégies Polly avec les instances configurées de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-213">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="e4375-214">Les extensions Polly prennent en charge l’ajout de gestionnaires basés sur Polly aux clients.</span><span class="sxs-lookup"><span data-stu-id="e4375-214">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="e4375-215">Polly requiert le package NuGet [Microsoft. extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) .</span><span class="sxs-lookup"><span data-stu-id="e4375-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="e4375-216">Gérer les erreurs temporaires</span><span class="sxs-lookup"><span data-stu-id="e4375-216">Handle transient faults</span></span>

<span data-ttu-id="e4375-217">Les erreurs se produisent généralement lorsque les appels HTTP externes sont temporaires.</span><span class="sxs-lookup"><span data-stu-id="e4375-217">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="e4375-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> permet de définir une stratégie pour gérer les erreurs temporaires.</span><span class="sxs-lookup"><span data-stu-id="e4375-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="e4375-219">Les stratégies configurées avec `AddTransientHttpErrorPolicy` gèrent les réponses suivantes :</span><span class="sxs-lookup"><span data-stu-id="e4375-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="e4375-220">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="e4375-220">HTTP 5xx</span></span>
* <span data-ttu-id="e4375-221">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="e4375-221">HTTP 408</span></span>

<span data-ttu-id="e4375-222">`AddTransientHttpErrorPolicy` permet d’accéder à un objet `PolicyBuilder` configuré pour gérer les erreurs qui représentent une erreur temporaire possible :</span><span class="sxs-lookup"><span data-stu-id="e4375-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="e4375-223">Dans le code précédent, une stratégie `WaitAndRetryAsync` est définie.</span><span class="sxs-lookup"><span data-stu-id="e4375-223">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="e4375-224">Les requêtes qui ont échoué sont retentées jusqu’à trois fois avec un délai de 600 ms entre les tentatives.</span><span class="sxs-lookup"><span data-stu-id="e4375-224">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="e4375-225">Sélectionner dynamiquement des stratégies</span><span class="sxs-lookup"><span data-stu-id="e4375-225">Dynamically select policies</span></span>

<span data-ttu-id="e4375-226">Les méthodes d’extension sont fournies pour ajouter des gestionnaires basés sur Polly, par exemple, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span><span class="sxs-lookup"><span data-stu-id="e4375-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="e4375-227">La surcharge de `AddPolicyHandler` suivante inspecte la demande pour décider de la stratégie à appliquer :</span><span class="sxs-lookup"><span data-stu-id="e4375-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="e4375-228">Dans le code précédent, si la requête sortante est un HTTP GET, un délai d’attente de 10 secondes est appliqué.</span><span class="sxs-lookup"><span data-stu-id="e4375-228">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="e4375-229">Pour toutes les autres méthodes HTTP, un délai d’attente de 30 secondes est utilisé.</span><span class="sxs-lookup"><span data-stu-id="e4375-229">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="e4375-230">Ajouter plusieurs gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="e4375-230">Add multiple Polly handlers</span></span>

<span data-ttu-id="e4375-231">Il est courant d’imbriquer les stratégies Polly :</span><span class="sxs-lookup"><span data-stu-id="e4375-231">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="e4375-232">Dans l'exemple précédent :</span><span class="sxs-lookup"><span data-stu-id="e4375-232">In the preceding example:</span></span>

* <span data-ttu-id="e4375-233">Deux gestionnaires sont ajoutés.</span><span class="sxs-lookup"><span data-stu-id="e4375-233">Two handlers are added.</span></span>
* <span data-ttu-id="e4375-234">Le premier gestionnaire utilise <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> pour ajouter une stratégie de nouvelle tentative.</span><span class="sxs-lookup"><span data-stu-id="e4375-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="e4375-235">Les requêtes qui ont échoué sont retentées jusqu’à trois fois.</span><span class="sxs-lookup"><span data-stu-id="e4375-235">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="e4375-236">Le deuxième `AddTransientHttpErrorPolicy` appel ajoute une stratégie de disjoncteur.</span><span class="sxs-lookup"><span data-stu-id="e4375-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="e4375-237">Les demandes externes supplémentaires sont bloquées pendant 30 secondes si 5 tentatives ayant échoué se produisent de façon séquentielle.</span><span class="sxs-lookup"><span data-stu-id="e4375-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="e4375-238">Les stratégies de disjoncteur sont avec état.</span><span class="sxs-lookup"><span data-stu-id="e4375-238">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="e4375-239">Tous les appels effectués via ce client partagent le même état du circuit.</span><span class="sxs-lookup"><span data-stu-id="e4375-239">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="e4375-240">Ajouter des stratégies à partir du Registre Polly</span><span class="sxs-lookup"><span data-stu-id="e4375-240">Add policies from the Polly registry</span></span>

<span data-ttu-id="e4375-241">Une approche de la gestion des stratégies régulièrement utilisées consiste à les définir une seule fois et à les inscrire avec un `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="e4375-241">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="e4375-242">Dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e4375-242">In the following code:</span></span>

* <span data-ttu-id="e4375-243">Les stratégies « régulières » et « longues » sont ajoutées.</span><span class="sxs-lookup"><span data-stu-id="e4375-243">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="e4375-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*> ajoute les stratégies « régulières » et « longues » à partir du Registre.</span><span class="sxs-lookup"><span data-stu-id="e4375-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="e4375-245">Pour plus d’informations sur les intégrations de `IHttpClientFactory` et Polly, consultez le [wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="e4375-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="e4375-246">HttpClient et gestion de la durée de vie</span><span class="sxs-lookup"><span data-stu-id="e4375-246">HttpClient and lifetime management</span></span>

<span data-ttu-id="e4375-247">Une nouvelle instance `HttpClient` est retournée à chaque fois que `CreateClient` est appelé sur `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="e4375-247">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="e4375-248">Un <xref:System.Net.Http.HttpMessageHandler> est créé par client nommé.</span><span class="sxs-lookup"><span data-stu-id="e4375-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="e4375-249">La fabrique gère les durées de vie des instances `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="e4375-249">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="e4375-250">`IHttpClientFactory` regroupe dans un pool les instances de `HttpMessageHandler` créées par la fabrique pour réduire la consommation des ressources.</span><span class="sxs-lookup"><span data-stu-id="e4375-250">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="e4375-251">Une instance de `HttpMessageHandler` peut être réutilisée à partir du pool quand vous créez une instance de `HttpClient` si sa durée de vie n’a pas expiré.</span><span class="sxs-lookup"><span data-stu-id="e4375-251">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="e4375-252">Le regroupement de gestionnaires en pools est souhaitable, car chaque gestionnaire gère habituellement ses propres connexions HTTP sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="e4375-252">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="e4375-253">La création de plus de gestionnaires que nécessaire peut entraîner des délais de connexion.</span><span class="sxs-lookup"><span data-stu-id="e4375-253">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="e4375-254">Certains gestionnaires maintiennent également les connexions ouvertes indéfiniment, ce qui peut empêcher le gestionnaire de réagir aux modifications DNS (Domain Name System).</span><span class="sxs-lookup"><span data-stu-id="e4375-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="e4375-255">La durée de vie par défaut d’un gestionnaire est de deux minutes.</span><span class="sxs-lookup"><span data-stu-id="e4375-255">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="e4375-256">La valeur par défaut peut être substituée pour chaque client nommé :</span><span class="sxs-lookup"><span data-stu-id="e4375-256">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="e4375-257">les instances de `HttpClient` peuvent généralement être traitées comme des objets .NET qui **ne nécessitent pas** de suppression.</span><span class="sxs-lookup"><span data-stu-id="e4375-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="e4375-258">La suppression annule les requêtes sortantes et garantit que l’instance `HttpClient` donnée ne peut pas être utilisée après avoir appelé <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="e4375-258">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="e4375-259">`IHttpClientFactory` effectue le suivi et libère les ressources utilisées par les instances `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-259">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="e4375-260">Le fait de conserver une seule instance `HttpClient` active pendant une longue durée est un modèle commun utilisé avant le lancement de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="e4375-260">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="e4375-261">Ce modèle devient inutile après la migration vers `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="e4375-261">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="e4375-262">Alternatives à IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="e4375-262">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="e4375-263">L’utilisation de `IHttpClientFactory` dans une application DI-Enabled évite les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="e4375-263">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="e4375-264">Problèmes d’épuisement des ressources par le regroupement `HttpMessageHandler` instances.</span><span class="sxs-lookup"><span data-stu-id="e4375-264">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="e4375-265">Problèmes DNS périmés par le cycle `HttpMessageHandler` instances sur des instances régulières.</span><span class="sxs-lookup"><span data-stu-id="e4375-265">Stale-DNS problems by cycling `HttpMessageHandler` instances at regular instances.</span></span>

<span data-ttu-id="e4375-266">Il existe d’autres façons de résoudre les problèmes précédents à l’aide d’une instance de <xref:System.Net.Http.SocketsHttpHandler> à long terme.</span><span class="sxs-lookup"><span data-stu-id="e4375-266">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="e4375-267">Créez une instance de `SocketsHttpHandler` lorsque l’application démarre et utilisez-la pendant toute la durée de vie de l’application.</span><span class="sxs-lookup"><span data-stu-id="e4375-267">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="e4375-268">Configurez <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> à une valeur appropriée en fonction des temps d’actualisation DNS.</span><span class="sxs-lookup"><span data-stu-id="e4375-268">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="e4375-269">Créez `HttpClient` instances à l’aide d' `new HttpClient(handler, dispostHandler: false)` en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="e4375-269">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="e4375-270">Les approches précédentes résolvent les problèmes de gestion des ressources qui `IHttpClientFactory` résolus de la même façon.</span><span class="sxs-lookup"><span data-stu-id="e4375-270">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="e4375-271">Le `SocketsHttpHandler` partage les connexions entre les instances de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-271">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="e4375-272">Ce partage empêche l’épuisement des sockets.</span><span class="sxs-lookup"><span data-stu-id="e4375-272">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="e4375-273">Le `SocketsHttpHandler ` cycle les connexions en fonction des `PooledConnectionLifetime` afin d’éviter les problèmes DNS d’État.</span><span class="sxs-lookup"><span data-stu-id="e4375-273">The `SocketsHttpHandler ` cycles connections according to `PooledConnectionLifetime` to avoid state-DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="e4375-274">Cookies</span><span class="sxs-lookup"><span data-stu-id="e4375-274">Cookies</span></span>

<span data-ttu-id="e4375-275">Les instances de `HttpMessageHandler` regroupées entraînent le partage de `CookieContainer` objets.</span><span class="sxs-lookup"><span data-stu-id="e4375-275">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="e4375-276">Le partage d’objets `CookieContainer` imprévus aboutit souvent à un code incorrect.</span><span class="sxs-lookup"><span data-stu-id="e4375-276">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="e4375-277">Pour les applications qui nécessitent des cookies, utilisez l’une des deux opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="e4375-277">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="e4375-278">Désactivation de la gestion automatique des cookies</span><span class="sxs-lookup"><span data-stu-id="e4375-278">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="e4375-279">Éviter les `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="e4375-279">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="e4375-280">Appelez <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> pour désactiver la gestion automatique des cookies :</span><span class="sxs-lookup"><span data-stu-id="e4375-280">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="e4375-281">Journalisation</span><span class="sxs-lookup"><span data-stu-id="e4375-281">Logging</span></span>

<span data-ttu-id="e4375-282">Les clients créés via `IHttpClientFactory` enregistrent les messages de journalisation pour toutes les requêtes.</span><span class="sxs-lookup"><span data-stu-id="e4375-282">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="e4375-283">Activez le niveau d’information approprié dans la configuration de la journalisation pour voir les messages du journal par défaut.</span><span class="sxs-lookup"><span data-stu-id="e4375-283">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="e4375-284">Une journalisation supplémentaire, comme celle des en-têtes des requêtes, est incluse seulement au niveau de trace.</span><span class="sxs-lookup"><span data-stu-id="e4375-284">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="e4375-285">La catégorie de journal utilisée pour chaque client comprend le nom du client.</span><span class="sxs-lookup"><span data-stu-id="e4375-285">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="e4375-286">Un client nommé *MyNamedClient*, par exemple, journalise des messages avec la catégorie « System .net. http. httpclient ». **MyNamedClient**. LogicalHandler".</span><span class="sxs-lookup"><span data-stu-id="e4375-286">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="e4375-287">Les messages avec le suffixe *LogicalHandler* se produisent en dehors du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="e4375-287">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="e4375-288">Lors d’une requête, les messages sont journalisés avant que d’autres gestionnaires du pipeline l’aient traitée.</span><span class="sxs-lookup"><span data-stu-id="e4375-288">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="e4375-289">Lors d’une réponse, les messages sont journalisés une fois que tous les autres gestionnaires du pipeline ont reçu la réponse.</span><span class="sxs-lookup"><span data-stu-id="e4375-289">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="e4375-290">La journalisation se produit également à l’intérieur du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="e4375-290">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="e4375-291">Dans l’exemple *MyNamedClient* , ces messages sont journalisés avec la catégorie de journal « System .net. http. httpclient ». **MyNamedClient**. ClientHandler".</span><span class="sxs-lookup"><span data-stu-id="e4375-291">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="e4375-292">Pour la requête, cela se produit une fois que tous les autres gestionnaires ont été exécutés et immédiatement avant l’envoi de la demande.</span><span class="sxs-lookup"><span data-stu-id="e4375-292">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="e4375-293">Lors de la réponse, cette journalisation inclut l’état de la réponse avant qu’elle repasse à travers le pipeline de gestionnaires.</span><span class="sxs-lookup"><span data-stu-id="e4375-293">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="e4375-294">L’activation de la journalisation à l’extérieur et à l’intérieur du pipeline permet l’inspection des changements apportés par les autres gestionnaires du pipeline.</span><span class="sxs-lookup"><span data-stu-id="e4375-294">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="e4375-295">Cela peut inclure des modifications apportées aux en-têtes de demande ou au code d’état de réponse.</span><span class="sxs-lookup"><span data-stu-id="e4375-295">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="e4375-296">L’inclusion du nom du client dans la catégorie journal active le filtrage des journaux pour des clients nommés spécifiques.</span><span class="sxs-lookup"><span data-stu-id="e4375-296">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="e4375-297">Configurer le HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="e4375-297">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="e4375-298">Il peut être nécessaire de contrôler la configuration du `HttpMessageHandler` interne utilisé par un client.</span><span class="sxs-lookup"><span data-stu-id="e4375-298">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="e4375-299">Un `IHttpClientBuilder` est retourné quand vous ajoutez des clients nommés ou typés.</span><span class="sxs-lookup"><span data-stu-id="e4375-299">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="e4375-300">La méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> peut être utilisée pour définir un délégué.</span><span class="sxs-lookup"><span data-stu-id="e4375-300">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="e4375-301">Le délégué est utilisé pour créer et configurer le `HttpMessageHandler` principal utilisé par ce client :</span><span class="sxs-lookup"><span data-stu-id="e4375-301">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="e4375-302">Utiliser IHttpClientFactory dans une application console</span><span class="sxs-lookup"><span data-stu-id="e4375-302">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="e4375-303">Dans une application console, ajoutez les références de package suivantes au projet :</span><span class="sxs-lookup"><span data-stu-id="e4375-303">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="e4375-304">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="e4375-304">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="e4375-305">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="e4375-305">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="e4375-306">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="e4375-306">In the following example:</span></span>

* <span data-ttu-id="e4375-307"><xref:System.Net.Http.IHttpClientFactory> est inscrit dans le conteneur de service dans de l’[hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="e4375-307"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="e4375-308">`MyService` crée une instance de fabrique cliente à partir du service, qui est utilisée pour créer un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-308">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="e4375-309">`HttpClient` est utilisé pour récupérer une page web.</span><span class="sxs-lookup"><span data-stu-id="e4375-309">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="e4375-310">`Main` crée une étendue pour exécuter la méthode `GetPage` du service et écrire les 500 premiers caractères du contenu de la page web dans la console.</span><span class="sxs-lookup"><span data-stu-id="e4375-310">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="e4375-311">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e4375-311">Additional resources</span></span>

* [<span data-ttu-id="e4375-312">Utiliser HttpClientFactory pour implémenter des requêtes HTTP résilientes</span><span class="sxs-lookup"><span data-stu-id="e4375-312">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="e4375-313">Implémenter de nouvelles tentatives d’appel HTTP avec interruption exponentielle avec des stratégies Polly et HttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="e4375-313">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="e4375-314">Implémenter le modèle Disjoncteur</span><span class="sxs-lookup"><span data-stu-id="e4375-314">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="e4375-315">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) et [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="e4375-315">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="e4375-316">Une <xref:System.Net.Http.IHttpClientFactory> peut être inscrite et utilisée pour configurer et créer des instances de <xref:System.Net.Http.HttpClient> dans une application.</span><span class="sxs-lookup"><span data-stu-id="e4375-316">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="e4375-317">Elle offre les avantages suivants :</span><span class="sxs-lookup"><span data-stu-id="e4375-317">It offers the following benefits:</span></span>

* <span data-ttu-id="e4375-318">Fournit un emplacement central pour le nommage et la configuration d’instance de `HttpClient` logiques.</span><span class="sxs-lookup"><span data-stu-id="e4375-318">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="e4375-319">Par exemple, un client *github* peut être inscrit et configuré pour accéder à [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="e4375-319">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="e4375-320">Un client par défaut peut être inscrit à d’autres fins.</span><span class="sxs-lookup"><span data-stu-id="e4375-320">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="e4375-321">Codifie le concept de middleware (intergiciel) sortant via la délégation de gestionnaires dans `HttpClient` et fournit des extensions permettant au middleware Polly d’en tirer parti.</span><span class="sxs-lookup"><span data-stu-id="e4375-321">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="e4375-322">Gère le regroupement et la durée de vie des instances de `HttpClientMessageHandler` sous-jacentes pour éviter les problèmes DNS courants qui se produisent lors de la gestion manuelle des durées de vie de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-322">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="e4375-323">Ajoute une expérience de journalisation configurable (via `ILogger`) pour toutes les requêtes envoyées via des clients créés par la fabrique.</span><span class="sxs-lookup"><span data-stu-id="e4375-323">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="e4375-324">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e4375-324">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="e4375-325">Modèles de consommation</span><span class="sxs-lookup"><span data-stu-id="e4375-325">Consumption patterns</span></span>

<span data-ttu-id="e4375-326">Vous pouvez utiliser `IHttpClientFactory` dans une application de plusieurs façons :</span><span class="sxs-lookup"><span data-stu-id="e4375-326">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="e4375-327">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="e4375-327">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="e4375-328">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="e4375-328">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="e4375-329">Clients typés</span><span class="sxs-lookup"><span data-stu-id="e4375-329">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="e4375-330">Clients générés</span><span class="sxs-lookup"><span data-stu-id="e4375-330">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="e4375-331">Aucune d’entre elles n’est meilleure qu’une autre.</span><span class="sxs-lookup"><span data-stu-id="e4375-331">None of them are strictly superior to another.</span></span> <span data-ttu-id="e4375-332">La meilleure approche dépend des contraintes de l’application.</span><span class="sxs-lookup"><span data-stu-id="e4375-332">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="e4375-333">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="e4375-333">Basic usage</span></span>

<span data-ttu-id="e4375-334">Vous pouvez inscrire la `IHttpClientFactory` en appelant la méthode d’extension `AddHttpClient` sur la `IServiceCollection`, à l’intérieur la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e4375-334">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="e4375-335">Une fois inscrit, le code peut accepter un `IHttpClientFactory` partout où des services peuvent être injectés avec [une injection de dépendance](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e4375-335">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e4375-336">La `IHttpClientFactory` peut être utilisée pour créer une instance de `HttpClient` :</span><span class="sxs-lookup"><span data-stu-id="e4375-336">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="e4375-337">L’utilisation de `IHttpClientFactory` de cette façon est un excellent moyen de refactoriser une application existante.</span><span class="sxs-lookup"><span data-stu-id="e4375-337">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="e4375-338">Elle n’a aucun impact sur la façon dont `HttpClient` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="e4375-338">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="e4375-339">Dans les endroits où les instances de `HttpClient` sont actuellement créées, remplacez ces occurrences par un appel à <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="e4375-339">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="e4375-340">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="e4375-340">Named clients</span></span>

<span data-ttu-id="e4375-341">Si une application nécessite plusieurs utilisations distinctes de `HttpClient`, chacune avec une configuration différente, une option consiste à utiliser des **clients nommés**.</span><span class="sxs-lookup"><span data-stu-id="e4375-341">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="e4375-342">La configuration d’un `HttpClient` nommé peut être spécifiée lors de l’inscription dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e4375-342">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="e4375-343">Dans le code précédent, `AddHttpClient` est appelé en fournissant le nom *github*.</span><span class="sxs-lookup"><span data-stu-id="e4375-343">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="e4375-344">Une configuration par défaut est appliquée à ce client : l’adresse de base et deux en-têtes nécessaires pour utiliser l’API GitHub.</span><span class="sxs-lookup"><span data-stu-id="e4375-344">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="e4375-345">Chaque fois que `CreateClient` est appelée, une nouvelle instance de `HttpClient` est créée et l’action de configuration est appelée.</span><span class="sxs-lookup"><span data-stu-id="e4375-345">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="e4375-346">Pour utiliser un client nommé, un paramètre de chaîne peut être passé à `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-346">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="e4375-347">Spécifiez le nom du client à créer :</span><span class="sxs-lookup"><span data-stu-id="e4375-347">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="e4375-348">Dans le code précédent, la requête n’a pas besoin de spécifier un nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="e4375-348">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="e4375-349">Elle peut simplement passer le chemin, car l’adresse de base configurée pour le client est utilisée.</span><span class="sxs-lookup"><span data-stu-id="e4375-349">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="e4375-350">Clients typés</span><span class="sxs-lookup"><span data-stu-id="e4375-350">Typed clients</span></span>

<span data-ttu-id="e4375-351">Clients typés :</span><span class="sxs-lookup"><span data-stu-id="e4375-351">Typed clients:</span></span>

* <span data-ttu-id="e4375-352">Fournissent les mêmes fonctionnalités que les clients nommés, sans qu’il soit nécessaire d’utiliser des chaînes comme clés.</span><span class="sxs-lookup"><span data-stu-id="e4375-352">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="e4375-353">Bénéficie de l’aide d’IntelliSense et du compilateur lors de l’utilisation des clients.</span><span class="sxs-lookup"><span data-stu-id="e4375-353">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="e4375-354">Fournissent un emplacement unique pour configurer et interagir avec un `HttpClient` particulier.</span><span class="sxs-lookup"><span data-stu-id="e4375-354">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="e4375-355">Par exemple, un même client typé peut être utilisé pour un point de terminaison de backend et pour encapsuler la logique relative à ce point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="e4375-355">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="e4375-356">Fonctionnent avec l’injection de dépendances et peuvent être injectés là où c’est nécessaire dans votre application.</span><span class="sxs-lookup"><span data-stu-id="e4375-356">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="e4375-357">Un client typé accepte un `HttpClient` dans son constructeur :</span><span class="sxs-lookup"><span data-stu-id="e4375-357">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="e4375-358">Dans le code précédent, la configuration est déplacée dans le client typé.</span><span class="sxs-lookup"><span data-stu-id="e4375-358">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="e4375-359">L’objet `HttpClient` est exposé en tant que propriété publique.</span><span class="sxs-lookup"><span data-stu-id="e4375-359">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="e4375-360">Il est possible de définir des méthodes d’API spécifiques qui exposent les fonctionnalités de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-360">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="e4375-361">La méthode `GetAspNetDocsIssues` encapsule le code nécessaire pour interroger et analyser les problèmes ouverts les plus récents d’un dépôt GitHub.</span><span class="sxs-lookup"><span data-stu-id="e4375-361">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="e4375-362">Pour inscrire un client typé, la méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> générique peut être utilisée dans `Startup.ConfigureServices`, en spécifiant la classe du client typé :</span><span class="sxs-lookup"><span data-stu-id="e4375-362">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="e4375-363">Le client typé est inscrit comme étant transitoire avec injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e4375-363">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="e4375-364">Le client typé peut être injecté et utilisé directement :</span><span class="sxs-lookup"><span data-stu-id="e4375-364">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="e4375-365">Si vous préférez, vous pouvez spécifier la configuration d’un client typé lors de l’inscription dans `Startup.ConfigureServices` au lieu de le faire dans le constructeur du client typé :</span><span class="sxs-lookup"><span data-stu-id="e4375-365">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="e4375-366">Il est possible d’encapsuler entièrement le `HttpClient` dans un client typé.</span><span class="sxs-lookup"><span data-stu-id="e4375-366">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="e4375-367">Au lieu de l’exposer en tant que propriété, vous pouvez fournir des méthodes publiques qui appellent l’instance de `HttpClient` en interne.</span><span class="sxs-lookup"><span data-stu-id="e4375-367">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="e4375-368">Dans le code précédent, le `HttpClient` est stocké en tant que champ privé.</span><span class="sxs-lookup"><span data-stu-id="e4375-368">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="e4375-369">Tous les accès nécessaires pour effectuer des appels externes passent par la méthode `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="e4375-369">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="e4375-370">Clients générés</span><span class="sxs-lookup"><span data-stu-id="e4375-370">Generated clients</span></span>

<span data-ttu-id="e4375-371">`IHttpClientFactory` peut être utilisé en combinaison avec d’autres bibliothèques tierces, comme [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="e4375-371">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="e4375-372">Refit est une bibliothèque REST pour .NET.</span><span class="sxs-lookup"><span data-stu-id="e4375-372">Refit is a REST library for .NET.</span></span> <span data-ttu-id="e4375-373">Il convertit les API REST en interfaces dynamiques.</span><span class="sxs-lookup"><span data-stu-id="e4375-373">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="e4375-374">Une implémentation de l’interface est générée dynamiquement par le `RestService`, avec `HttpClient` pour faire les appels HTTP externes.</span><span class="sxs-lookup"><span data-stu-id="e4375-374">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="e4375-375">Une interface et une réponse sont définies pour représenter l’API externe et sa réponse :</span><span class="sxs-lookup"><span data-stu-id="e4375-375">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="e4375-376">Vous pouvez ajouter un client typé en utilisant Refit pour générer l’implémentation :</span><span class="sxs-lookup"><span data-stu-id="e4375-376">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="e4375-377">L’interface définie peut être utilisée quand c’est nécessaire, avec l’implémentation fournie par l’injection de dépendances et Refit :</span><span class="sxs-lookup"><span data-stu-id="e4375-377">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="e4375-378">Middleware pour les requêtes sortantes</span><span class="sxs-lookup"><span data-stu-id="e4375-378">Outgoing request middleware</span></span>

<span data-ttu-id="e4375-379">`HttpClient` intègre déjà le concept de délégation des gestionnaires qui peuvent être liés ensemble pour les requêtes HTTP sortantes.</span><span class="sxs-lookup"><span data-stu-id="e4375-379">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="e4375-380">Le `IHttpClientFactory` permet de définir facilement les gestionnaires à appliquer pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="e4375-380">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="e4375-381">Il prend en charge l’inscription et le chaînage de plusieurs gestionnaires pour créer un pipeline de middlewares pour les requêtes sortantes.</span><span class="sxs-lookup"><span data-stu-id="e4375-381">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="e4375-382">Chacun de ces gestionnaires peut effectuer un travail avant et après la requête sortante.</span><span class="sxs-lookup"><span data-stu-id="e4375-382">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="e4375-383">Ce modèle est similaire au pipeline de middlewares entrants dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e4375-383">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="e4375-384">Le modèle fournit un mécanisme permettant de gérer les problèmes transversaux liés aux des requêtes HTTP, notamment la mise en cache, la gestion des erreurs, la sérialisation et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="e4375-384">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="e4375-385">Pour créer un gestionnaire, définissez une classe dérivant de <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="e4375-385">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="e4375-386">Remplacez la méthode `SendAsync` de façon à exécuter du code avant de passer la requête au gestionnaire suivant dans le pipeline :</span><span class="sxs-lookup"><span data-stu-id="e4375-386">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="e4375-387">Le code précédent définit un gestionnaire de base.</span><span class="sxs-lookup"><span data-stu-id="e4375-387">The preceding code defines a basic handler.</span></span> <span data-ttu-id="e4375-388">Il vérifie si un en-tête `X-API-KEY` a été inclus dans la requête.</span><span class="sxs-lookup"><span data-stu-id="e4375-388">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="e4375-389">Si l’en-tête est manquant, il peut éviter l’appel HTTP et retourner une réponse appropriée.</span><span class="sxs-lookup"><span data-stu-id="e4375-389">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="e4375-390">Lors de l’inscription, un ou plusieurs gestionnaires peuvent être ajoutés à la configuration pour un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-390">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="e4375-391">Cette tâche est accomplie via des méthodes d’extension sur le <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e4375-391">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="e4375-392">Dans le code précédent, le `ValidateHeaderHandler` est inscrit avec une injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e4375-392">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="e4375-393">`IHttpClientFactory` crée une étendue DI distincte pour chaque gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="e4375-393">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="e4375-394">Les gestionnaires peuvent librement dépendre des services de n’importe quelle étendue.</span><span class="sxs-lookup"><span data-stu-id="e4375-394">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="e4375-395">Les services dont dépendent les gestionnaires sont supprimés lorsque le gestionnaire est supprimé.</span><span class="sxs-lookup"><span data-stu-id="e4375-395">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="e4375-396">Une fois inscrit, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> peut être appelé en passant en entrée le type pour le gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="e4375-396">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="e4375-397">Vous pouvez inscrire plusieurs gestionnaires dans l’ordre où ils doivent être exécutés.</span><span class="sxs-lookup"><span data-stu-id="e4375-397">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="e4375-398">Chaque gestionnaire wrappe le gestionnaire suivant jusqu’à ce que le dernier `HttpClientHandler` exécute la requête :</span><span class="sxs-lookup"><span data-stu-id="e4375-398">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="e4375-399">Utilisez l’une des approches suivantes pour partager l’état de chaque requête avec les gestionnaires de messages :</span><span class="sxs-lookup"><span data-stu-id="e4375-399">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="e4375-400">Passez des données dans le gestionnaire en utilisant `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="e4375-400">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="e4375-401">Utilisez `IHttpContextAccessor` pour accéder à la requête en cours.</span><span class="sxs-lookup"><span data-stu-id="e4375-401">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="e4375-402">Créez un objet de stockage `AsyncLocal` personnalisé pour passer les données.</span><span class="sxs-lookup"><span data-stu-id="e4375-402">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="e4375-403">Utiliser les gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="e4375-403">Use Polly-based handlers</span></span>

<span data-ttu-id="e4375-404">`IHttpClientFactory` s’intègre à une bibliothèque tierce très utilisée, appelée [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="e4375-404">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="e4375-405">Polly est une bibliothèque complète de gestion des erreurs transitoires et de résilience pour .NET.</span><span class="sxs-lookup"><span data-stu-id="e4375-405">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="e4375-406">Elle permet aux développeurs de formuler facilement et de façon thread-safe des stratégies, comme Retry (Nouvelle tentative), Circuit Breaker (Disjoncteur), Timeout (Délai d’attente), Bulkhead Isolation (Isolation par cloisonnement) et Fallback (Alternative de repli).</span><span class="sxs-lookup"><span data-stu-id="e4375-406">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="e4375-407">Des méthodes d’extension sont fournies pour permettre l’utilisation de stratégies Polly avec les instances configurées de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-407">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="e4375-408">Les extensions Polly :</span><span class="sxs-lookup"><span data-stu-id="e4375-408">The Polly extensions:</span></span>

* <span data-ttu-id="e4375-409">Prennent en charge l’ajout de gestionnaires Polly à des clients.</span><span class="sxs-lookup"><span data-stu-id="e4375-409">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="e4375-410">Peuvent être utilisées après l’installation du package NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="e4375-410">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="e4375-411">Ce package n’est pas inclus dans le framework partagé ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e4375-411">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="e4375-412">Gérer les erreurs temporaires</span><span class="sxs-lookup"><span data-stu-id="e4375-412">Handle transient faults</span></span>

<span data-ttu-id="e4375-413">Les erreurs courantes se produisent lorsque des appels HTTP externes sont temporaires.</span><span class="sxs-lookup"><span data-stu-id="e4375-413">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="e4375-414">Une méthode d’extension pratique nommée `AddTransientHttpErrorPolicy` est incluse : elle permet de définir une stratégie pour gérer les erreurs temporaires.</span><span class="sxs-lookup"><span data-stu-id="e4375-414">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="e4375-415">Les stratégies configurées avec cette méthode d’extension gèrent `HttpRequestException`, les réponses HTTP 5xx et les réponses HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="e4375-415">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="e4375-416">L’extension `AddTransientHttpErrorPolicy` peut être utilisée dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e4375-416">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e4375-417">L’extension fournit l’accès à un objet `PolicyBuilder` configuré pour gérer les erreurs représentant une erreur temporaire possible :</span><span class="sxs-lookup"><span data-stu-id="e4375-417">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="e4375-418">Dans le code précédent, une stratégie `WaitAndRetryAsync` est définie.</span><span class="sxs-lookup"><span data-stu-id="e4375-418">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="e4375-419">Les requêtes qui ont échoué sont retentées jusqu’à trois fois avec un délai de 600 ms entre les tentatives.</span><span class="sxs-lookup"><span data-stu-id="e4375-419">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="e4375-420">Sélectionner dynamiquement des stratégies</span><span class="sxs-lookup"><span data-stu-id="e4375-420">Dynamically select policies</span></span>

<span data-ttu-id="e4375-421">Il existe d’autres méthodes d’extension que vous pouvez utiliser pour ajouter des gestionnaires Polly.</span><span class="sxs-lookup"><span data-stu-id="e4375-421">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="e4375-422">Une de ces extensions est `AddPolicyHandler`, qui a plusieurs surcharges.</span><span class="sxs-lookup"><span data-stu-id="e4375-422">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="e4375-423">Une de ces surcharges permet l’inspection de la requête lors de la définition de la stratégie à appliquer :</span><span class="sxs-lookup"><span data-stu-id="e4375-423">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="e4375-424">Dans le code précédent, si la requête sortante est un HTTP GET, un délai d’attente de 10 secondes est appliqué.</span><span class="sxs-lookup"><span data-stu-id="e4375-424">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="e4375-425">Pour toutes les autres méthodes HTTP, un délai d’attente de 30 secondes est utilisé.</span><span class="sxs-lookup"><span data-stu-id="e4375-425">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="e4375-426">Ajouter plusieurs gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="e4375-426">Add multiple Polly handlers</span></span>

<span data-ttu-id="e4375-427">Il est courant d’imbriquer des stratégies Polly pour fournir des fonctionnalités améliorées :</span><span class="sxs-lookup"><span data-stu-id="e4375-427">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="e4375-428">Dans l’exemple précédent, deux gestionnaires sont ajoutés.</span><span class="sxs-lookup"><span data-stu-id="e4375-428">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="e4375-429">Le premier utilise l’extension `AddTransientHttpErrorPolicy` pour ajouter une stratégie de nouvelle tentative.</span><span class="sxs-lookup"><span data-stu-id="e4375-429">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="e4375-430">Les requêtes qui ont échoué sont retentées jusqu’à trois fois.</span><span class="sxs-lookup"><span data-stu-id="e4375-430">Failed requests are retried up to three times.</span></span> <span data-ttu-id="e4375-431">Le deuxième appel à `AddTransientHttpErrorPolicy` ajoute une stratégie de disjoncteur.</span><span class="sxs-lookup"><span data-stu-id="e4375-431">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="e4375-432">Les requêtes externes supplémentaires sont bloquées pendant 30 secondes si cinq tentatives successives échouent.</span><span class="sxs-lookup"><span data-stu-id="e4375-432">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="e4375-433">Les stratégies de disjoncteur sont avec état.</span><span class="sxs-lookup"><span data-stu-id="e4375-433">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="e4375-434">Tous les appels effectués via ce client partagent le même état du circuit.</span><span class="sxs-lookup"><span data-stu-id="e4375-434">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="e4375-435">Ajouter des stratégies à partir du Registre Polly</span><span class="sxs-lookup"><span data-stu-id="e4375-435">Add policies from the Polly registry</span></span>

<span data-ttu-id="e4375-436">Une approche de la gestion des stratégies régulièrement utilisées consiste à les définir une seule fois et à les inscrire avec un `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="e4375-436">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="e4375-437">Il existe une méthode d’extension qui permet l’ajout d’un gestionnaire avec une stratégie du Registre :</span><span class="sxs-lookup"><span data-stu-id="e4375-437">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="e4375-438">Dans le code précédent, deux stratégies sont inscrites lorsque `PolicyRegistry` est ajouté à `ServiceCollection`.</span><span class="sxs-lookup"><span data-stu-id="e4375-438">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="e4375-439">Pour utiliser une stratégie du Registre, la méthode `AddPolicyHandlerFromRegistry` est utilisée, en passant le nom de la stratégie à appliquer.</span><span class="sxs-lookup"><span data-stu-id="e4375-439">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="e4375-440">Vous trouverez plus d’informations sur les intégrations de `IHttpClientFactory` et de Polly dans le [wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="e4375-440">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="e4375-441">HttpClient et gestion de la durée de vie</span><span class="sxs-lookup"><span data-stu-id="e4375-441">HttpClient and lifetime management</span></span>

<span data-ttu-id="e4375-442">Une nouvelle instance `HttpClient` est retournée à chaque fois que `CreateClient` est appelé sur `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="e4375-442">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="e4375-443">Il existe un <xref:System.Net.Http.HttpMessageHandler> par client nommé.</span><span class="sxs-lookup"><span data-stu-id="e4375-443">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="e4375-444">La fabrique gère les durées de vie des instances `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="e4375-444">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="e4375-445">`IHttpClientFactory` regroupe dans un pool les instances de `HttpMessageHandler` créées par la fabrique pour réduire la consommation des ressources.</span><span class="sxs-lookup"><span data-stu-id="e4375-445">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="e4375-446">Une instance de `HttpMessageHandler` peut être réutilisée à partir du pool quand vous créez une instance de `HttpClient` si sa durée de vie n’a pas expiré.</span><span class="sxs-lookup"><span data-stu-id="e4375-446">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="e4375-447">Le regroupement de gestionnaires en pools est souhaitable, car chaque gestionnaire gère habituellement ses propres connexions HTTP sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="e4375-447">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="e4375-448">La création de plus de gestionnaires que nécessaire peut entraîner des délais de connexion.</span><span class="sxs-lookup"><span data-stu-id="e4375-448">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="e4375-449">Certains gestionnaires conservent aussi les connexions indéfiniment ouvertes, ce qui peut empêcher le gestionnaire de réagir aux changements du DNS.</span><span class="sxs-lookup"><span data-stu-id="e4375-449">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="e4375-450">La durée de vie par défaut d’un gestionnaire est de deux minutes.</span><span class="sxs-lookup"><span data-stu-id="e4375-450">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="e4375-451">La valeur par défaut peut être remplacée pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="e4375-451">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="e4375-452">Pour la remplacer, appelez <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> sur le `IHttpClientBuilder` qui est retourné lors de la création du client :</span><span class="sxs-lookup"><span data-stu-id="e4375-452">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="e4375-453">La suppression du client n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="e4375-453">Disposal of the client isn't required.</span></span> <span data-ttu-id="e4375-454">La suppression annule les requêtes sortantes et garantit que l’instance `HttpClient` donnée ne peut pas être utilisée après avoir appelé <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="e4375-454">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="e4375-455">`IHttpClientFactory` effectue le suivi et libère les ressources utilisées par les instances `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-455">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="e4375-456">Les instances `HttpClient` peuvent généralement être traitées en tant qu’objets .NET ne nécessitant pas une suppression.</span><span class="sxs-lookup"><span data-stu-id="e4375-456">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="e4375-457">Le fait de conserver une seule instance `HttpClient` active pendant une longue durée est un modèle commun utilisé avant le lancement de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="e4375-457">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="e4375-458">Ce modèle devient inutile après la migration vers `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="e4375-458">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="e4375-459">Alternatives à IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="e4375-459">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="e4375-460">L’utilisation de `IHttpClientFactory` dans une application DI-Enabled évite les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="e4375-460">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="e4375-461">Problèmes d’épuisement des ressources par le regroupement `HttpMessageHandler` instances.</span><span class="sxs-lookup"><span data-stu-id="e4375-461">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="e4375-462">Problèmes DNS périmés par le cycle `HttpMessageHandler` instances sur des instances régulières.</span><span class="sxs-lookup"><span data-stu-id="e4375-462">Stale-DNS problems by cycling `HttpMessageHandler` instances at regular instances.</span></span>

<span data-ttu-id="e4375-463">Il existe d’autres façons de résoudre les problèmes précédents à l’aide d’une instance de <xref:System.Net.Http.SocketsHttpHandler> à long terme.</span><span class="sxs-lookup"><span data-stu-id="e4375-463">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="e4375-464">Créez une instance de `SocketsHttpHandler` lorsque l’application démarre et utilisez-la pendant toute la durée de vie de l’application.</span><span class="sxs-lookup"><span data-stu-id="e4375-464">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="e4375-465">Configurez <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> à une valeur appropriée en fonction des temps d’actualisation DNS.</span><span class="sxs-lookup"><span data-stu-id="e4375-465">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="e4375-466">Créez `HttpClient` instances à l’aide d' `new HttpClient(handler, dispostHandler: false)` en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="e4375-466">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="e4375-467">Les approches précédentes résolvent les problèmes de gestion des ressources qui `IHttpClientFactory` résolus de la même façon.</span><span class="sxs-lookup"><span data-stu-id="e4375-467">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="e4375-468">Le `SocketsHttpHandler` partage les connexions entre les instances de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-468">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="e4375-469">Ce partage empêche l’épuisement des sockets.</span><span class="sxs-lookup"><span data-stu-id="e4375-469">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="e4375-470">Le `SocketsHttpHandler ` cycle les connexions en fonction des `PooledConnectionLifetime` afin d’éviter les problèmes DNS d’État.</span><span class="sxs-lookup"><span data-stu-id="e4375-470">The `SocketsHttpHandler ` cycles connections according to `PooledConnectionLifetime` to avoid state-DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="e4375-471">Cookies</span><span class="sxs-lookup"><span data-stu-id="e4375-471">Cookies</span></span>

<span data-ttu-id="e4375-472">Les instances de `HttpMessageHandler` regroupées entraînent le partage de `CookieContainer` objets.</span><span class="sxs-lookup"><span data-stu-id="e4375-472">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="e4375-473">Le partage d’objets `CookieContainer` imprévus aboutit souvent à un code incorrect.</span><span class="sxs-lookup"><span data-stu-id="e4375-473">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="e4375-474">Pour les applications qui nécessitent des cookies, utilisez l’une des deux opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="e4375-474">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="e4375-475">Désactivation de la gestion automatique des cookies</span><span class="sxs-lookup"><span data-stu-id="e4375-475">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="e4375-476">Éviter les `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="e4375-476">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="e4375-477">Appelez <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> pour désactiver la gestion automatique des cookies :</span><span class="sxs-lookup"><span data-stu-id="e4375-477">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="e4375-478">Journalisation</span><span class="sxs-lookup"><span data-stu-id="e4375-478">Logging</span></span>

<span data-ttu-id="e4375-479">Les clients créés via `IHttpClientFactory` enregistrent les messages de journalisation pour toutes les requêtes.</span><span class="sxs-lookup"><span data-stu-id="e4375-479">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="e4375-480">Activez le niveau d’informations approprié dans votre configuration de journalisation pour voir les messages de journalisation par défaut.</span><span class="sxs-lookup"><span data-stu-id="e4375-480">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="e4375-481">Une journalisation supplémentaire, comme celle des en-têtes des requêtes, est incluse seulement au niveau de trace.</span><span class="sxs-lookup"><span data-stu-id="e4375-481">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="e4375-482">La catégorie de journal utilisée pour chaque client comprend le nom du client.</span><span class="sxs-lookup"><span data-stu-id="e4375-482">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="e4375-483">Par exemple, un client nommé *MyNamedClient* journalise les messages avec la catégorie `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="e4375-483">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="e4375-484">Les messages avec le suffixe *LogicalHandler* se produisent en dehors du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="e4375-484">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="e4375-485">Lors d’une requête, les messages sont journalisés avant que d’autres gestionnaires du pipeline l’aient traitée.</span><span class="sxs-lookup"><span data-stu-id="e4375-485">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="e4375-486">Lors d’une réponse, les messages sont journalisés une fois que tous les autres gestionnaires du pipeline ont reçu la réponse.</span><span class="sxs-lookup"><span data-stu-id="e4375-486">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="e4375-487">La journalisation se produit également à l’intérieur du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="e4375-487">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="e4375-488">Dans l’exemple *MyNamedClient*, ces messages sont journalisés avec la catégorie de journalisation `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="e4375-488">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="e4375-489">Pour la requête, cela se produit après que tous les autres gestionnaires ont été exécutés et immédiatement avant l’envoi de la requête sur le réseau.</span><span class="sxs-lookup"><span data-stu-id="e4375-489">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="e4375-490">Lors de la réponse, cette journalisation inclut l’état de la réponse avant qu’elle repasse à travers le pipeline de gestionnaires.</span><span class="sxs-lookup"><span data-stu-id="e4375-490">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="e4375-491">L’activation de la journalisation à l’extérieur et à l’intérieur du pipeline permet l’inspection des changements apportés par les autres gestionnaires du pipeline.</span><span class="sxs-lookup"><span data-stu-id="e4375-491">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="e4375-492">Par exemple, cela peut comprendre des changements apportés aux en-têtes des requêtes ou au code d’état de la réponse.</span><span class="sxs-lookup"><span data-stu-id="e4375-492">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="e4375-493">L’ajout du nom du client dans la catégorie de journalisation permet si nécessaire de filtrer le journal pour des clients nommés spécifiques.</span><span class="sxs-lookup"><span data-stu-id="e4375-493">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="e4375-494">Configurer le HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="e4375-494">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="e4375-495">Il peut être nécessaire de contrôler la configuration du `HttpMessageHandler` interne utilisé par un client.</span><span class="sxs-lookup"><span data-stu-id="e4375-495">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="e4375-496">Un `IHttpClientBuilder` est retourné quand vous ajoutez des clients nommés ou typés.</span><span class="sxs-lookup"><span data-stu-id="e4375-496">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="e4375-497">La méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> peut être utilisée pour définir un délégué.</span><span class="sxs-lookup"><span data-stu-id="e4375-497">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="e4375-498">Le délégué est utilisé pour créer et configurer le `HttpMessageHandler` principal utilisé par ce client :</span><span class="sxs-lookup"><span data-stu-id="e4375-498">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="e4375-499">Utiliser IHttpClientFactory dans une application console</span><span class="sxs-lookup"><span data-stu-id="e4375-499">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="e4375-500">Dans une application console, ajoutez les références de package suivantes au projet :</span><span class="sxs-lookup"><span data-stu-id="e4375-500">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="e4375-501">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="e4375-501">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="e4375-502">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="e4375-502">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="e4375-503">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="e4375-503">In the following example:</span></span>

* <span data-ttu-id="e4375-504"><xref:System.Net.Http.IHttpClientFactory> est inscrit dans le conteneur de service dans de l’[hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="e4375-504"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="e4375-505">`MyService` crée une instance de fabrique cliente à partir du service, qui est utilisée pour créer un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-505">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="e4375-506">`HttpClient` est utilisé pour récupérer une page web.</span><span class="sxs-lookup"><span data-stu-id="e4375-506">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="e4375-507">`Main` crée une étendue pour exécuter la méthode `GetPage` du service et écrire les 500 premiers caractères du contenu de la page web dans la console.</span><span class="sxs-lookup"><span data-stu-id="e4375-507">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="e4375-508">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e4375-508">Additional resources</span></span>

* [<span data-ttu-id="e4375-509">Utiliser HttpClientFactory pour implémenter des requêtes HTTP résilientes</span><span class="sxs-lookup"><span data-stu-id="e4375-509">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="e4375-510">Implémenter de nouvelles tentatives d’appel HTTP avec interruption exponentielle avec des stratégies Polly et HttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="e4375-510">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="e4375-511">Implémenter le modèle Disjoncteur</span><span class="sxs-lookup"><span data-stu-id="e4375-511">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<span data-ttu-id="e4375-512">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) et [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="e4375-512">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="e4375-513">Une <xref:System.Net.Http.IHttpClientFactory> peut être inscrite et utilisée pour configurer et créer des instances de <xref:System.Net.Http.HttpClient> dans une application.</span><span class="sxs-lookup"><span data-stu-id="e4375-513">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="e4375-514">Elle offre les avantages suivants :</span><span class="sxs-lookup"><span data-stu-id="e4375-514">It offers the following benefits:</span></span>

* <span data-ttu-id="e4375-515">Fournit un emplacement central pour le nommage et la configuration d’instance de `HttpClient` logiques.</span><span class="sxs-lookup"><span data-stu-id="e4375-515">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="e4375-516">Par exemple, un client *github* peut être inscrit et configuré pour accéder à [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="e4375-516">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="e4375-517">Un client par défaut peut être inscrit à d’autres fins.</span><span class="sxs-lookup"><span data-stu-id="e4375-517">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="e4375-518">Codifie le concept de middleware (intergiciel) sortant via la délégation de gestionnaires dans `HttpClient` et fournit des extensions permettant au middleware Polly d’en tirer parti.</span><span class="sxs-lookup"><span data-stu-id="e4375-518">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="e4375-519">Gère le regroupement et la durée de vie des instances de `HttpClientMessageHandler` sous-jacentes pour éviter les problèmes DNS courants qui se produisent lors de la gestion manuelle des durées de vie de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-519">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="e4375-520">Ajoute une expérience de journalisation configurable (via `ILogger`) pour toutes les requêtes envoyées via des clients créés par la fabrique.</span><span class="sxs-lookup"><span data-stu-id="e4375-520">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="e4375-521">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e4375-521">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4375-522">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="e4375-522">Prerequisites</span></span>

<span data-ttu-id="e4375-523">Les projets ciblant .NET Framework nécessitent l’installation du package NuGet [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/).</span><span class="sxs-lookup"><span data-stu-id="e4375-523">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="e4375-524">Les projets qui ciblent .NET Core et référencent le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) incluent déjà le package `Microsoft.Extensions.Http`.</span><span class="sxs-lookup"><span data-stu-id="e4375-524">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="e4375-525">Modèles de consommation</span><span class="sxs-lookup"><span data-stu-id="e4375-525">Consumption patterns</span></span>

<span data-ttu-id="e4375-526">Vous pouvez utiliser `IHttpClientFactory` dans une application de plusieurs façons :</span><span class="sxs-lookup"><span data-stu-id="e4375-526">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="e4375-527">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="e4375-527">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="e4375-528">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="e4375-528">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="e4375-529">Clients typés</span><span class="sxs-lookup"><span data-stu-id="e4375-529">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="e4375-530">Clients générés</span><span class="sxs-lookup"><span data-stu-id="e4375-530">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="e4375-531">Aucune d’entre elles n’est meilleure qu’une autre.</span><span class="sxs-lookup"><span data-stu-id="e4375-531">None of them are strictly superior to another.</span></span> <span data-ttu-id="e4375-532">La meilleure approche dépend des contraintes de l’application.</span><span class="sxs-lookup"><span data-stu-id="e4375-532">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="e4375-533">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="e4375-533">Basic usage</span></span>

<span data-ttu-id="e4375-534">Vous pouvez inscrire la `IHttpClientFactory` en appelant la méthode d’extension `AddHttpClient` sur la `IServiceCollection`, à l’intérieur la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e4375-534">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="e4375-535">Une fois inscrit, le code peut accepter un `IHttpClientFactory` partout où des services peuvent être injectés avec [une injection de dépendance](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e4375-535">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e4375-536">La `IHttpClientFactory` peut être utilisée pour créer une instance de `HttpClient` :</span><span class="sxs-lookup"><span data-stu-id="e4375-536">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="e4375-537">L’utilisation de `IHttpClientFactory` de cette façon est un excellent moyen de refactoriser une application existante.</span><span class="sxs-lookup"><span data-stu-id="e4375-537">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="e4375-538">Elle n’a aucun impact sur la façon dont `HttpClient` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="e4375-538">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="e4375-539">Dans les endroits où les instances de `HttpClient` sont actuellement créées, remplacez ces occurrences par un appel à <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="e4375-539">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="e4375-540">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="e4375-540">Named clients</span></span>

<span data-ttu-id="e4375-541">Si une application nécessite plusieurs utilisations distinctes de `HttpClient`, chacune avec une configuration différente, une option consiste à utiliser des **clients nommés**.</span><span class="sxs-lookup"><span data-stu-id="e4375-541">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="e4375-542">La configuration d’un `HttpClient` nommé peut être spécifiée lors de l’inscription dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e4375-542">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="e4375-543">Dans le code précédent, `AddHttpClient` est appelé en fournissant le nom *github*.</span><span class="sxs-lookup"><span data-stu-id="e4375-543">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="e4375-544">Une configuration par défaut est appliquée à ce client : l’adresse de base et deux en-têtes nécessaires pour utiliser l’API GitHub.</span><span class="sxs-lookup"><span data-stu-id="e4375-544">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="e4375-545">Chaque fois que `CreateClient` est appelée, une nouvelle instance de `HttpClient` est créée et l’action de configuration est appelée.</span><span class="sxs-lookup"><span data-stu-id="e4375-545">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="e4375-546">Pour utiliser un client nommé, un paramètre de chaîne peut être passé à `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-546">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="e4375-547">Spécifiez le nom du client à créer :</span><span class="sxs-lookup"><span data-stu-id="e4375-547">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="e4375-548">Dans le code précédent, la requête n’a pas besoin de spécifier un nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="e4375-548">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="e4375-549">Elle peut simplement passer le chemin, car l’adresse de base configurée pour le client est utilisée.</span><span class="sxs-lookup"><span data-stu-id="e4375-549">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="e4375-550">Clients typés</span><span class="sxs-lookup"><span data-stu-id="e4375-550">Typed clients</span></span>

<span data-ttu-id="e4375-551">Clients typés :</span><span class="sxs-lookup"><span data-stu-id="e4375-551">Typed clients:</span></span>

* <span data-ttu-id="e4375-552">Fournissent les mêmes fonctionnalités que les clients nommés, sans qu’il soit nécessaire d’utiliser des chaînes comme clés.</span><span class="sxs-lookup"><span data-stu-id="e4375-552">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="e4375-553">Bénéficie de l’aide d’IntelliSense et du compilateur lors de l’utilisation des clients.</span><span class="sxs-lookup"><span data-stu-id="e4375-553">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="e4375-554">Fournissent un emplacement unique pour configurer et interagir avec un `HttpClient` particulier.</span><span class="sxs-lookup"><span data-stu-id="e4375-554">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="e4375-555">Par exemple, un même client typé peut être utilisé pour un point de terminaison de backend et pour encapsuler la logique relative à ce point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="e4375-555">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="e4375-556">Fonctionnent avec l’injection de dépendances et peuvent être injectés là où c’est nécessaire dans votre application.</span><span class="sxs-lookup"><span data-stu-id="e4375-556">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="e4375-557">Un client typé accepte un `HttpClient` dans son constructeur :</span><span class="sxs-lookup"><span data-stu-id="e4375-557">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="e4375-558">Dans le code précédent, la configuration est déplacée dans le client typé.</span><span class="sxs-lookup"><span data-stu-id="e4375-558">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="e4375-559">L’objet `HttpClient` est exposé en tant que propriété publique.</span><span class="sxs-lookup"><span data-stu-id="e4375-559">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="e4375-560">Il est possible de définir des méthodes d’API spécifiques qui exposent les fonctionnalités de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-560">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="e4375-561">La méthode `GetAspNetDocsIssues` encapsule le code nécessaire pour interroger et analyser les problèmes ouverts les plus récents d’un dépôt GitHub.</span><span class="sxs-lookup"><span data-stu-id="e4375-561">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="e4375-562">Pour inscrire un client typé, la méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> générique peut être utilisée dans `Startup.ConfigureServices`, en spécifiant la classe du client typé :</span><span class="sxs-lookup"><span data-stu-id="e4375-562">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="e4375-563">Le client typé est inscrit comme étant transitoire avec injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e4375-563">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="e4375-564">Le client typé peut être injecté et utilisé directement :</span><span class="sxs-lookup"><span data-stu-id="e4375-564">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="e4375-565">Si vous préférez, vous pouvez spécifier la configuration d’un client typé lors de l’inscription dans `Startup.ConfigureServices` au lieu de le faire dans le constructeur du client typé :</span><span class="sxs-lookup"><span data-stu-id="e4375-565">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="e4375-566">Il est possible d’encapsuler entièrement le `HttpClient` dans un client typé.</span><span class="sxs-lookup"><span data-stu-id="e4375-566">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="e4375-567">Au lieu de l’exposer en tant que propriété, vous pouvez fournir des méthodes publiques qui appellent l’instance de `HttpClient` en interne.</span><span class="sxs-lookup"><span data-stu-id="e4375-567">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="e4375-568">Dans le code précédent, le `HttpClient` est stocké en tant que champ privé.</span><span class="sxs-lookup"><span data-stu-id="e4375-568">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="e4375-569">Tous les accès nécessaires pour effectuer des appels externes passent par la méthode `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="e4375-569">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="e4375-570">Clients générés</span><span class="sxs-lookup"><span data-stu-id="e4375-570">Generated clients</span></span>

<span data-ttu-id="e4375-571">`IHttpClientFactory` peut être utilisé en combinaison avec d’autres bibliothèques tierces, comme [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="e4375-571">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="e4375-572">Refit est une bibliothèque REST pour .NET.</span><span class="sxs-lookup"><span data-stu-id="e4375-572">Refit is a REST library for .NET.</span></span> <span data-ttu-id="e4375-573">Il convertit les API REST en interfaces dynamiques.</span><span class="sxs-lookup"><span data-stu-id="e4375-573">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="e4375-574">Une implémentation de l’interface est générée dynamiquement par le `RestService`, avec `HttpClient` pour faire les appels HTTP externes.</span><span class="sxs-lookup"><span data-stu-id="e4375-574">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="e4375-575">Une interface et une réponse sont définies pour représenter l’API externe et sa réponse :</span><span class="sxs-lookup"><span data-stu-id="e4375-575">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="e4375-576">Vous pouvez ajouter un client typé en utilisant Refit pour générer l’implémentation :</span><span class="sxs-lookup"><span data-stu-id="e4375-576">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="e4375-577">L’interface définie peut être utilisée quand c’est nécessaire, avec l’implémentation fournie par l’injection de dépendances et Refit :</span><span class="sxs-lookup"><span data-stu-id="e4375-577">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="e4375-578">Middleware pour les requêtes sortantes</span><span class="sxs-lookup"><span data-stu-id="e4375-578">Outgoing request middleware</span></span>

<span data-ttu-id="e4375-579">`HttpClient` intègre déjà le concept de délégation des gestionnaires qui peuvent être liés ensemble pour les requêtes HTTP sortantes.</span><span class="sxs-lookup"><span data-stu-id="e4375-579">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="e4375-580">Le `IHttpClientFactory` permet de définir facilement les gestionnaires à appliquer pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="e4375-580">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="e4375-581">Il prend en charge l’inscription et le chaînage de plusieurs gestionnaires pour créer un pipeline de middlewares pour les requêtes sortantes.</span><span class="sxs-lookup"><span data-stu-id="e4375-581">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="e4375-582">Chacun de ces gestionnaires peut effectuer un travail avant et après la requête sortante.</span><span class="sxs-lookup"><span data-stu-id="e4375-582">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="e4375-583">Ce modèle est similaire au pipeline de middlewares entrants dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e4375-583">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="e4375-584">Le modèle fournit un mécanisme permettant de gérer les problèmes transversaux liés aux des requêtes HTTP, notamment la mise en cache, la gestion des erreurs, la sérialisation et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="e4375-584">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="e4375-585">Pour créer un gestionnaire, définissez une classe dérivant de <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="e4375-585">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="e4375-586">Remplacez la méthode `SendAsync` de façon à exécuter du code avant de passer la requête au gestionnaire suivant dans le pipeline :</span><span class="sxs-lookup"><span data-stu-id="e4375-586">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="e4375-587">Le code précédent définit un gestionnaire de base.</span><span class="sxs-lookup"><span data-stu-id="e4375-587">The preceding code defines a basic handler.</span></span> <span data-ttu-id="e4375-588">Il vérifie si un en-tête `X-API-KEY` a été inclus dans la requête.</span><span class="sxs-lookup"><span data-stu-id="e4375-588">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="e4375-589">Si l’en-tête est manquant, il peut éviter l’appel HTTP et retourner une réponse appropriée.</span><span class="sxs-lookup"><span data-stu-id="e4375-589">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="e4375-590">Lors de l’inscription, un ou plusieurs gestionnaires peuvent être ajoutés à la configuration pour un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-590">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="e4375-591">Cette tâche est accomplie via des méthodes d’extension sur le <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e4375-591">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="e4375-592">Dans le code précédent, le `ValidateHeaderHandler` est inscrit avec une injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e4375-592">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="e4375-593">Le gestionnaire **doit** être inscrit dans l’injection de dépendances en tant que service temporaire, sans étendue.</span><span class="sxs-lookup"><span data-stu-id="e4375-593">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="e4375-594">Si le gestionnaire est inscrit en tant que service étendu et que des services dont dépend le gestionnaire peuvent être supprimés :</span><span class="sxs-lookup"><span data-stu-id="e4375-594">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="e4375-595">Les services du gestionnaire peuvent être supprimés avant que le gestionnaire ne soit hors de portée.</span><span class="sxs-lookup"><span data-stu-id="e4375-595">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="e4375-596">Les services du gestionnaire supprimés entraînent un échec du gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="e4375-596">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="e4375-597">Une fois inscrit, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> peut être appelé en passant en entrée le type du gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="e4375-597">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="e4375-598">Vous pouvez inscrire plusieurs gestionnaires dans l’ordre où ils doivent être exécutés.</span><span class="sxs-lookup"><span data-stu-id="e4375-598">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="e4375-599">Chaque gestionnaire wrappe le gestionnaire suivant jusqu’à ce que le dernier `HttpClientHandler` exécute la requête :</span><span class="sxs-lookup"><span data-stu-id="e4375-599">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="e4375-600">Utilisez l’une des approches suivantes pour partager l’état de chaque requête avec les gestionnaires de messages :</span><span class="sxs-lookup"><span data-stu-id="e4375-600">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="e4375-601">Passez des données dans le gestionnaire en utilisant `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="e4375-601">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="e4375-602">Utilisez `IHttpContextAccessor` pour accéder à la requête en cours.</span><span class="sxs-lookup"><span data-stu-id="e4375-602">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="e4375-603">Créez un objet de stockage `AsyncLocal` personnalisé pour passer les données.</span><span class="sxs-lookup"><span data-stu-id="e4375-603">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="e4375-604">Utiliser les gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="e4375-604">Use Polly-based handlers</span></span>

<span data-ttu-id="e4375-605">`IHttpClientFactory` s’intègre à une bibliothèque tierce très utilisée, appelée [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="e4375-605">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="e4375-606">Polly est une bibliothèque complète de gestion des erreurs transitoires et de résilience pour .NET.</span><span class="sxs-lookup"><span data-stu-id="e4375-606">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="e4375-607">Elle permet aux développeurs de formuler facilement et de façon thread-safe des stratégies, comme Retry (Nouvelle tentative), Circuit Breaker (Disjoncteur), Timeout (Délai d’attente), Bulkhead Isolation (Isolation par cloisonnement) et Fallback (Alternative de repli).</span><span class="sxs-lookup"><span data-stu-id="e4375-607">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="e4375-608">Des méthodes d’extension sont fournies pour permettre l’utilisation de stratégies Polly avec les instances configurées de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-608">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="e4375-609">Les extensions Polly :</span><span class="sxs-lookup"><span data-stu-id="e4375-609">The Polly extensions:</span></span>

* <span data-ttu-id="e4375-610">Prennent en charge l’ajout de gestionnaires Polly à des clients.</span><span class="sxs-lookup"><span data-stu-id="e4375-610">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="e4375-611">Peuvent être utilisées après l’installation du package NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="e4375-611">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="e4375-612">Ce package n’est pas inclus dans le framework partagé ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e4375-612">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="e4375-613">Gérer les erreurs temporaires</span><span class="sxs-lookup"><span data-stu-id="e4375-613">Handle transient faults</span></span>

<span data-ttu-id="e4375-614">Les erreurs courantes se produisent lorsque des appels HTTP externes sont temporaires.</span><span class="sxs-lookup"><span data-stu-id="e4375-614">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="e4375-615">Une méthode d’extension pratique nommée `AddTransientHttpErrorPolicy` est incluse : elle permet de définir une stratégie pour gérer les erreurs temporaires.</span><span class="sxs-lookup"><span data-stu-id="e4375-615">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="e4375-616">Les stratégies configurées avec cette méthode d’extension gèrent `HttpRequestException`, les réponses HTTP 5xx et les réponses HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="e4375-616">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="e4375-617">L’extension `AddTransientHttpErrorPolicy` peut être utilisée dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e4375-617">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e4375-618">L’extension fournit l’accès à un objet `PolicyBuilder` configuré pour gérer les erreurs représentant une erreur temporaire possible :</span><span class="sxs-lookup"><span data-stu-id="e4375-618">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="e4375-619">Dans le code précédent, une stratégie `WaitAndRetryAsync` est définie.</span><span class="sxs-lookup"><span data-stu-id="e4375-619">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="e4375-620">Les requêtes qui ont échoué sont retentées jusqu’à trois fois avec un délai de 600 ms entre les tentatives.</span><span class="sxs-lookup"><span data-stu-id="e4375-620">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="e4375-621">Sélectionner dynamiquement des stratégies</span><span class="sxs-lookup"><span data-stu-id="e4375-621">Dynamically select policies</span></span>

<span data-ttu-id="e4375-622">Il existe d’autres méthodes d’extension que vous pouvez utiliser pour ajouter des gestionnaires Polly.</span><span class="sxs-lookup"><span data-stu-id="e4375-622">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="e4375-623">Une de ces extensions est `AddPolicyHandler`, qui a plusieurs surcharges.</span><span class="sxs-lookup"><span data-stu-id="e4375-623">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="e4375-624">Une de ces surcharges permet l’inspection de la requête lors de la définition de la stratégie à appliquer :</span><span class="sxs-lookup"><span data-stu-id="e4375-624">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="e4375-625">Dans le code précédent, si la requête sortante est un HTTP GET, un délai d’attente de 10 secondes est appliqué.</span><span class="sxs-lookup"><span data-stu-id="e4375-625">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="e4375-626">Pour toutes les autres méthodes HTTP, un délai d’attente de 30 secondes est utilisé.</span><span class="sxs-lookup"><span data-stu-id="e4375-626">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="e4375-627">Ajouter plusieurs gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="e4375-627">Add multiple Polly handlers</span></span>

<span data-ttu-id="e4375-628">Il est courant d’imbriquer des stratégies Polly pour fournir des fonctionnalités améliorées :</span><span class="sxs-lookup"><span data-stu-id="e4375-628">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="e4375-629">Dans l’exemple précédent, deux gestionnaires sont ajoutés.</span><span class="sxs-lookup"><span data-stu-id="e4375-629">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="e4375-630">Le premier utilise l’extension `AddTransientHttpErrorPolicy` pour ajouter une stratégie de nouvelle tentative.</span><span class="sxs-lookup"><span data-stu-id="e4375-630">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="e4375-631">Les requêtes qui ont échoué sont retentées jusqu’à trois fois.</span><span class="sxs-lookup"><span data-stu-id="e4375-631">Failed requests are retried up to three times.</span></span> <span data-ttu-id="e4375-632">Le deuxième appel à `AddTransientHttpErrorPolicy` ajoute une stratégie de disjoncteur.</span><span class="sxs-lookup"><span data-stu-id="e4375-632">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="e4375-633">Les requêtes externes supplémentaires sont bloquées pendant 30 secondes si cinq tentatives successives échouent.</span><span class="sxs-lookup"><span data-stu-id="e4375-633">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="e4375-634">Les stratégies de disjoncteur sont avec état.</span><span class="sxs-lookup"><span data-stu-id="e4375-634">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="e4375-635">Tous les appels effectués via ce client partagent le même état du circuit.</span><span class="sxs-lookup"><span data-stu-id="e4375-635">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="e4375-636">Ajouter des stratégies à partir du Registre Polly</span><span class="sxs-lookup"><span data-stu-id="e4375-636">Add policies from the Polly registry</span></span>

<span data-ttu-id="e4375-637">Une approche de la gestion des stratégies régulièrement utilisées consiste à les définir une seule fois et à les inscrire avec un `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="e4375-637">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="e4375-638">Il existe une méthode d’extension qui permet l’ajout d’un gestionnaire avec une stratégie du Registre :</span><span class="sxs-lookup"><span data-stu-id="e4375-638">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="e4375-639">Dans le code précédent, deux stratégies sont inscrites lorsque `PolicyRegistry` est ajouté à `ServiceCollection`.</span><span class="sxs-lookup"><span data-stu-id="e4375-639">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="e4375-640">Pour utiliser une stratégie du Registre, la méthode `AddPolicyHandlerFromRegistry` est utilisée, en passant le nom de la stratégie à appliquer.</span><span class="sxs-lookup"><span data-stu-id="e4375-640">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="e4375-641">Vous trouverez plus d’informations sur les intégrations de `IHttpClientFactory` et de Polly dans le [wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="e4375-641">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="e4375-642">HttpClient et gestion de la durée de vie</span><span class="sxs-lookup"><span data-stu-id="e4375-642">HttpClient and lifetime management</span></span>

<span data-ttu-id="e4375-643">Une nouvelle instance `HttpClient` est retournée à chaque fois que `CreateClient` est appelé sur `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="e4375-643">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="e4375-644">Il existe un <xref:System.Net.Http.HttpMessageHandler> par client nommé.</span><span class="sxs-lookup"><span data-stu-id="e4375-644">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="e4375-645">La fabrique gère les durées de vie des instances `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="e4375-645">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="e4375-646">`IHttpClientFactory` regroupe dans un pool les instances de `HttpMessageHandler` créées par la fabrique pour réduire la consommation des ressources.</span><span class="sxs-lookup"><span data-stu-id="e4375-646">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="e4375-647">Une instance de `HttpMessageHandler` peut être réutilisée à partir du pool quand vous créez une instance de `HttpClient` si sa durée de vie n’a pas expiré.</span><span class="sxs-lookup"><span data-stu-id="e4375-647">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="e4375-648">Le regroupement de gestionnaires en pools est souhaitable, car chaque gestionnaire gère habituellement ses propres connexions HTTP sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="e4375-648">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="e4375-649">La création de plus de gestionnaires que nécessaire peut entraîner des délais de connexion.</span><span class="sxs-lookup"><span data-stu-id="e4375-649">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="e4375-650">Certains gestionnaires conservent aussi les connexions indéfiniment ouvertes, ce qui peut empêcher le gestionnaire de réagir aux changements du DNS.</span><span class="sxs-lookup"><span data-stu-id="e4375-650">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="e4375-651">La durée de vie par défaut d’un gestionnaire est de deux minutes.</span><span class="sxs-lookup"><span data-stu-id="e4375-651">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="e4375-652">La valeur par défaut peut être remplacée pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="e4375-652">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="e4375-653">Pour la remplacer, appelez <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> sur le `IHttpClientBuilder` qui est retourné lors de la création du client :</span><span class="sxs-lookup"><span data-stu-id="e4375-653">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="e4375-654">La suppression du client n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="e4375-654">Disposal of the client isn't required.</span></span> <span data-ttu-id="e4375-655">La suppression annule les requêtes sortantes et garantit que l’instance `HttpClient` donnée ne peut pas être utilisée après avoir appelé <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="e4375-655">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="e4375-656">`IHttpClientFactory` effectue le suivi et libère les ressources utilisées par les instances `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-656">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="e4375-657">Les instances `HttpClient` peuvent généralement être traitées en tant qu’objets .NET ne nécessitant pas une suppression.</span><span class="sxs-lookup"><span data-stu-id="e4375-657">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="e4375-658">Le fait de conserver une seule instance `HttpClient` active pendant une longue durée est un modèle commun utilisé avant le lancement de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="e4375-658">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="e4375-659">Ce modèle devient inutile après la migration vers `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="e4375-659">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="e4375-660">Alternatives à IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="e4375-660">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="e4375-661">L’utilisation de `IHttpClientFactory` dans une application DI-Enabled évite les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="e4375-661">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="e4375-662">Problèmes d’épuisement des ressources par le regroupement `HttpMessageHandler` instances.</span><span class="sxs-lookup"><span data-stu-id="e4375-662">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="e4375-663">Problèmes DNS périmés par le cycle `HttpMessageHandler` instances sur des instances régulières.</span><span class="sxs-lookup"><span data-stu-id="e4375-663">Stale-DNS problems by cycling `HttpMessageHandler` instances at regular instances.</span></span>

<span data-ttu-id="e4375-664">Il existe d’autres façons de résoudre les problèmes précédents à l’aide d’une instance de <xref:System.Net.Http.SocketsHttpHandler> à long terme.</span><span class="sxs-lookup"><span data-stu-id="e4375-664">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="e4375-665">Créez une instance de `SocketsHttpHandler` lorsque l’application démarre et utilisez-la pendant toute la durée de vie de l’application.</span><span class="sxs-lookup"><span data-stu-id="e4375-665">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="e4375-666">Configurez <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> à une valeur appropriée en fonction des temps d’actualisation DNS.</span><span class="sxs-lookup"><span data-stu-id="e4375-666">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="e4375-667">Créez `HttpClient` instances à l’aide d' `new HttpClient(handler, dispostHandler: false)` en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="e4375-667">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="e4375-668">Les approches précédentes résolvent les problèmes de gestion des ressources qui `IHttpClientFactory` résolus de la même façon.</span><span class="sxs-lookup"><span data-stu-id="e4375-668">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="e4375-669">Le `SocketsHttpHandler` partage les connexions entre les instances de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-669">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="e4375-670">Ce partage empêche l’épuisement des sockets.</span><span class="sxs-lookup"><span data-stu-id="e4375-670">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="e4375-671">Le `SocketsHttpHandler ` cycle les connexions en fonction des `PooledConnectionLifetime` afin d’éviter les problèmes DNS d’État.</span><span class="sxs-lookup"><span data-stu-id="e4375-671">The `SocketsHttpHandler ` cycles connections according to `PooledConnectionLifetime` to avoid state-DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="e4375-672">Cookies</span><span class="sxs-lookup"><span data-stu-id="e4375-672">Cookies</span></span>

<span data-ttu-id="e4375-673">Les instances de `HttpMessageHandler` regroupées entraînent le partage de `CookieContainer` objets.</span><span class="sxs-lookup"><span data-stu-id="e4375-673">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="e4375-674">Le partage d’objets `CookieContainer` imprévus aboutit souvent à un code incorrect.</span><span class="sxs-lookup"><span data-stu-id="e4375-674">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="e4375-675">Pour les applications qui nécessitent des cookies, utilisez l’une des deux opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="e4375-675">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="e4375-676">Désactivation de la gestion automatique des cookies</span><span class="sxs-lookup"><span data-stu-id="e4375-676">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="e4375-677">Éviter les `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="e4375-677">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="e4375-678">Appelez <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> pour désactiver la gestion automatique des cookies :</span><span class="sxs-lookup"><span data-stu-id="e4375-678">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="e4375-679">Journalisation</span><span class="sxs-lookup"><span data-stu-id="e4375-679">Logging</span></span>

<span data-ttu-id="e4375-680">Les clients créés via `IHttpClientFactory` enregistrent les messages de journalisation pour toutes les requêtes.</span><span class="sxs-lookup"><span data-stu-id="e4375-680">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="e4375-681">Activez le niveau d’informations approprié dans votre configuration de journalisation pour voir les messages de journalisation par défaut.</span><span class="sxs-lookup"><span data-stu-id="e4375-681">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="e4375-682">Une journalisation supplémentaire, comme celle des en-têtes des requêtes, est incluse seulement au niveau de trace.</span><span class="sxs-lookup"><span data-stu-id="e4375-682">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="e4375-683">La catégorie de journal utilisée pour chaque client comprend le nom du client.</span><span class="sxs-lookup"><span data-stu-id="e4375-683">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="e4375-684">Par exemple, un client nommé *MyNamedClient* journalise les messages avec la catégorie `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="e4375-684">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="e4375-685">Les messages avec le suffixe *LogicalHandler* se produisent en dehors du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="e4375-685">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="e4375-686">Lors d’une requête, les messages sont journalisés avant que d’autres gestionnaires du pipeline l’aient traitée.</span><span class="sxs-lookup"><span data-stu-id="e4375-686">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="e4375-687">Lors d’une réponse, les messages sont journalisés une fois que tous les autres gestionnaires du pipeline ont reçu la réponse.</span><span class="sxs-lookup"><span data-stu-id="e4375-687">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="e4375-688">La journalisation se produit également à l’intérieur du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="e4375-688">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="e4375-689">Dans l’exemple *MyNamedClient*, ces messages sont journalisés avec la catégorie de journalisation `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="e4375-689">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="e4375-690">Pour la requête, cela se produit après que tous les autres gestionnaires ont été exécutés et immédiatement avant l’envoi de la requête sur le réseau.</span><span class="sxs-lookup"><span data-stu-id="e4375-690">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="e4375-691">Lors de la réponse, cette journalisation inclut l’état de la réponse avant qu’elle repasse à travers le pipeline de gestionnaires.</span><span class="sxs-lookup"><span data-stu-id="e4375-691">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="e4375-692">L’activation de la journalisation à l’extérieur et à l’intérieur du pipeline permet l’inspection des changements apportés par les autres gestionnaires du pipeline.</span><span class="sxs-lookup"><span data-stu-id="e4375-692">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="e4375-693">Par exemple, cela peut comprendre des changements apportés aux en-têtes des requêtes ou au code d’état de la réponse.</span><span class="sxs-lookup"><span data-stu-id="e4375-693">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="e4375-694">L’ajout du nom du client dans la catégorie de journalisation permet si nécessaire de filtrer le journal pour des clients nommés spécifiques.</span><span class="sxs-lookup"><span data-stu-id="e4375-694">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="e4375-695">Configurer le HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="e4375-695">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="e4375-696">Il peut être nécessaire de contrôler la configuration du `HttpMessageHandler` interne utilisé par un client.</span><span class="sxs-lookup"><span data-stu-id="e4375-696">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="e4375-697">Un `IHttpClientBuilder` est retourné quand vous ajoutez des clients nommés ou typés.</span><span class="sxs-lookup"><span data-stu-id="e4375-697">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="e4375-698">La méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> peut être utilisée pour définir un délégué.</span><span class="sxs-lookup"><span data-stu-id="e4375-698">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="e4375-699">Le délégué est utilisé pour créer et configurer le `HttpMessageHandler` principal utilisé par ce client :</span><span class="sxs-lookup"><span data-stu-id="e4375-699">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="e4375-700">Utiliser IHttpClientFactory dans une application console</span><span class="sxs-lookup"><span data-stu-id="e4375-700">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="e4375-701">Dans une application console, ajoutez les références de package suivantes au projet :</span><span class="sxs-lookup"><span data-stu-id="e4375-701">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="e4375-702">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="e4375-702">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="e4375-703">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="e4375-703">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="e4375-704">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="e4375-704">In the following example:</span></span>

* <span data-ttu-id="e4375-705"><xref:System.Net.Http.IHttpClientFactory> est inscrit dans le conteneur de service dans de l’[hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="e4375-705"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="e4375-706">`MyService` crée une instance de fabrique cliente à partir du service, qui est utilisée pour créer un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="e4375-706">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="e4375-707">`HttpClient` est utilisé pour récupérer une page web.</span><span class="sxs-lookup"><span data-stu-id="e4375-707">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="e4375-708">`Main` crée une étendue pour exécuter la méthode `GetPage` du service et écrire les 500 premiers caractères du contenu de la page web dans la console.</span><span class="sxs-lookup"><span data-stu-id="e4375-708">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="e4375-709">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e4375-709">Additional resources</span></span>

* [<span data-ttu-id="e4375-710">Utiliser HttpClientFactory pour implémenter des requêtes HTTP résilientes</span><span class="sxs-lookup"><span data-stu-id="e4375-710">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="e4375-711">Implémenter de nouvelles tentatives d’appel HTTP avec interruption exponentielle avec des stratégies Polly et HttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="e4375-711">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="e4375-712">Implémenter le modèle Disjoncteur</span><span class="sxs-lookup"><span data-stu-id="e4375-712">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
