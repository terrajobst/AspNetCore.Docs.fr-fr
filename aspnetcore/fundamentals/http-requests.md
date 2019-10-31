---
title: Effectuer des requêtes HTTP en utilisant IHttpClientFactory dans ASP.NET Core
author: stevejgordon
description: Découvrez plus d’informations sur l’utilisation de l’interface IHttpClientFactory pour gérer des instances HttpClient logiques dans ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/27/2019
uid: fundamentals/http-requests
ms.openlocfilehash: a963833acfa12889c8ae3dac443962682e1cb931
ms.sourcegitcommit: 032113208bb55ecfb2faeb6d3e9ea44eea827950
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2019
ms.locfileid: "73190586"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="5417b-103">Effectuer des requêtes HTTP en utilisant IHttpClientFactory dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5417b-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5417b-104">Par [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT)et [Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="5417b-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="5417b-105">Une <xref:System.Net.Http.IHttpClientFactory> peut être inscrite et utilisée pour configurer et créer des instances de <xref:System.Net.Http.HttpClient> dans une application.</span><span class="sxs-lookup"><span data-stu-id="5417b-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="5417b-106">`IHttpClientFactory` offre les avantages suivants :</span><span class="sxs-lookup"><span data-stu-id="5417b-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="5417b-107">Fournit un emplacement central pour le nommage et la configuration d’instance de `HttpClient` logiques.</span><span class="sxs-lookup"><span data-stu-id="5417b-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="5417b-108">Par exemple, un client nommé *GitHub* peut être inscrit et configuré pour accéder à [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="5417b-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="5417b-109">Un client par défaut peut être inscrit pour un accès général.</span><span class="sxs-lookup"><span data-stu-id="5417b-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="5417b-110">Codifie le concept d’intergiciel (middleware) sortant via la délégation de gestionnaires dans `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5417b-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="5417b-111">Fournit des extensions pour l’intergiciel (middleware) basé sur Polly pour tirer parti des gestionnaires de délégation dans `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5417b-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="5417b-112">Gère le regroupement et la durée de vie des instances de `HttpClientMessageHandler` sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="5417b-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="5417b-113">La gestion automatique évite les problèmes courants liés au DNS (Domain Name System) qui se produisent lors de la gestion manuelle des durées de vie de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5417b-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="5417b-114">Ajoute une expérience de journalisation configurable (via `ILogger`) pour toutes les requêtes envoyées via des clients créés par la fabrique.</span><span class="sxs-lookup"><span data-stu-id="5417b-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="5417b-115">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="5417b-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="5417b-116">L’exemple de code dans cette version de rubrique utilise <xref:System.Text.Json> pour désérialiser le contenu JSON renvoyé dans les réponses HTTP.</span><span class="sxs-lookup"><span data-stu-id="5417b-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="5417b-117">Pour obtenir des exemples qui utilisent des `Json.NET` et des `ReadAsAsync<T>`, utilisez le sélecteur de version pour sélectionner une version 2. x de cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="5417b-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="5417b-118">Modèles de consommation</span><span class="sxs-lookup"><span data-stu-id="5417b-118">Consumption patterns</span></span>

<span data-ttu-id="5417b-119">Vous pouvez utiliser `IHttpClientFactory` dans une application de plusieurs façons :</span><span class="sxs-lookup"><span data-stu-id="5417b-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="5417b-120">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="5417b-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="5417b-121">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="5417b-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="5417b-122">Clients typés</span><span class="sxs-lookup"><span data-stu-id="5417b-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="5417b-123">Clients générés</span><span class="sxs-lookup"><span data-stu-id="5417b-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="5417b-124">La meilleure approche dépend des exigences de l’application.</span><span class="sxs-lookup"><span data-stu-id="5417b-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="5417b-125">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="5417b-125">Basic usage</span></span>

<span data-ttu-id="5417b-126">`IHttpClientFactory` peut être inscrit en appelant `AddHttpClient`:</span><span class="sxs-lookup"><span data-stu-id="5417b-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="5417b-127">Un `IHttpClientFactory` peut être demandé à l’aide [de l’injection de dépendances (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="5417b-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="5417b-128">Le code suivant utilise `IHttpClientFactory` pour créer une instance `HttpClient` :</span><span class="sxs-lookup"><span data-stu-id="5417b-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="5417b-129">L’utilisation de `IHttpClientFactory` comme dans l’exemple précédent est un bon moyen de refactoriser une application existante.</span><span class="sxs-lookup"><span data-stu-id="5417b-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="5417b-130">Elle n’a aucun impact sur l’utilisation de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5417b-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="5417b-131">Dans les emplacements où les instances de `HttpClient` sont créées dans une application existante, remplacez-les par des appels à <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="5417b-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="5417b-132">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="5417b-132">Named clients</span></span>

<span data-ttu-id="5417b-133">Les clients nommés sont un bon choix lorsque :</span><span class="sxs-lookup"><span data-stu-id="5417b-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="5417b-134">L’application requiert de nombreuses utilisations distinctes de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5417b-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="5417b-135">De nombreuses `HttpClient`s ont une configuration différente.</span><span class="sxs-lookup"><span data-stu-id="5417b-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="5417b-136">La configuration d’un `HttpClient` nommé peut être spécifiée lors de l’inscription dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5417b-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="5417b-137">Dans le code précédent, le client est configuré avec :</span><span class="sxs-lookup"><span data-stu-id="5417b-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="5417b-138">Adresse de base `https://api.github.com/`.</span><span class="sxs-lookup"><span data-stu-id="5417b-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="5417b-139">Deux en-têtes nécessaires à l’utilisation de l’API GitHub.</span><span class="sxs-lookup"><span data-stu-id="5417b-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="5417b-140">CreateClient</span><span class="sxs-lookup"><span data-stu-id="5417b-140">CreateClient</span></span>

<span data-ttu-id="5417b-141">Chaque fois que <xref:System.Net.Http.IHttpClientFactory.CreateClient*> est appelée :</span><span class="sxs-lookup"><span data-stu-id="5417b-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="5417b-142">Une nouvelle instance de `HttpClient` est créée.</span><span class="sxs-lookup"><span data-stu-id="5417b-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="5417b-143">L’action de configuration est appelée.</span><span class="sxs-lookup"><span data-stu-id="5417b-143">The configuration action is called.</span></span>

<span data-ttu-id="5417b-144">Pour créer un client nommé, transmettez son nom à `CreateClient`:</span><span class="sxs-lookup"><span data-stu-id="5417b-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="5417b-145">Dans le code précédent, la requête n’a pas besoin de spécifier un nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="5417b-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="5417b-146">Le code peut passer uniquement le chemin d’accès, dans la mesure où l’adresse de base configurée pour le client est utilisée.</span><span class="sxs-lookup"><span data-stu-id="5417b-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="5417b-147">Clients typés</span><span class="sxs-lookup"><span data-stu-id="5417b-147">Typed clients</span></span>

<span data-ttu-id="5417b-148">Clients typés :</span><span class="sxs-lookup"><span data-stu-id="5417b-148">Typed clients:</span></span>

* <span data-ttu-id="5417b-149">Fournissent les mêmes fonctionnalités que les clients nommés, sans qu’il soit nécessaire d’utiliser des chaînes comme clés.</span><span class="sxs-lookup"><span data-stu-id="5417b-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="5417b-150">Bénéficie de l’aide d’IntelliSense et du compilateur lors de l’utilisation des clients.</span><span class="sxs-lookup"><span data-stu-id="5417b-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="5417b-151">Fournissent un emplacement unique pour configurer et interagir avec un `HttpClient` particulier.</span><span class="sxs-lookup"><span data-stu-id="5417b-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="5417b-152">Par exemple, un seul client typé peut être utilisé :</span><span class="sxs-lookup"><span data-stu-id="5417b-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="5417b-153">Pour un seul point de terminaison backend.</span><span class="sxs-lookup"><span data-stu-id="5417b-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="5417b-154">Pour encapsuler toute la logique traitant le point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="5417b-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="5417b-155">Utilisez DI et pouvez les injecter lorsque cela est requis dans l’application.</span><span class="sxs-lookup"><span data-stu-id="5417b-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="5417b-156">Un client typé accepte un `HttpClient` dans son constructeur :</span><span class="sxs-lookup"><span data-stu-id="5417b-156">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="5417b-157">Dans le code précédent :</span><span class="sxs-lookup"><span data-stu-id="5417b-157">In the preceding code:</span></span>

* <span data-ttu-id="5417b-158">La configuration est déplacée vers le client typé.</span><span class="sxs-lookup"><span data-stu-id="5417b-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="5417b-159">L’objet `HttpClient` est exposé en tant que propriété publique.</span><span class="sxs-lookup"><span data-stu-id="5417b-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="5417b-160">Des méthodes spécifiques à l’API peuvent être créées pour exposer les fonctionnalités de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5417b-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="5417b-161">Par exemple, la méthode `GetAspNetDocsIssues` encapsule du code pour récupérer des problèmes ouverts.</span><span class="sxs-lookup"><span data-stu-id="5417b-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="5417b-162">Le code suivant appelle <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> dans `Startup.ConfigureServices` pour inscrire une classe de client typée :</span><span class="sxs-lookup"><span data-stu-id="5417b-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="5417b-163">Le client typé est inscrit comme étant transitoire avec injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="5417b-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="5417b-164">Le client typé peut être injecté et utilisé directement :</span><span class="sxs-lookup"><span data-stu-id="5417b-164">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="5417b-165">La configuration d’un client typé peut être spécifiée lors de l’inscription dans `Startup.ConfigureServices`, plutôt que dans le constructeur du client typé :</span><span class="sxs-lookup"><span data-stu-id="5417b-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="5417b-166">Le `HttpClient` peut être encapsulé dans un client typé.</span><span class="sxs-lookup"><span data-stu-id="5417b-166">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="5417b-167">Au lieu de l’exposer en tant que propriété, définissez une méthode qui appelle l’instance de `HttpClient` en interne :</span><span class="sxs-lookup"><span data-stu-id="5417b-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="5417b-168">Dans le code précédent, le `HttpClient` est stocké dans un champ privé.</span><span class="sxs-lookup"><span data-stu-id="5417b-168">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="5417b-169">L’accès à l' `HttpClient` est par la méthode `GetRepos` publique.</span><span class="sxs-lookup"><span data-stu-id="5417b-169">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="5417b-170">Clients générés</span><span class="sxs-lookup"><span data-stu-id="5417b-170">Generated clients</span></span>

<span data-ttu-id="5417b-171">les `IHttpClientFactory` peuvent être utilisées en association avec des bibliothèques tierces, [telles que la](https://github.com/paulcbetts/refit)réutilisation.</span><span class="sxs-lookup"><span data-stu-id="5417b-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="5417b-172">Refit est une bibliothèque REST pour .NET.</span><span class="sxs-lookup"><span data-stu-id="5417b-172">Refit is a REST library for .NET.</span></span> <span data-ttu-id="5417b-173">Il convertit les API REST en interfaces dynamiques.</span><span class="sxs-lookup"><span data-stu-id="5417b-173">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="5417b-174">Une implémentation de l’interface est générée dynamiquement par le `RestService`, avec `HttpClient` pour faire les appels HTTP externes.</span><span class="sxs-lookup"><span data-stu-id="5417b-174">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="5417b-175">Une interface et une réponse sont définies pour représenter l’API externe et sa réponse :</span><span class="sxs-lookup"><span data-stu-id="5417b-175">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="5417b-176">Vous pouvez ajouter un client typé en utilisant Refit pour générer l’implémentation :</span><span class="sxs-lookup"><span data-stu-id="5417b-176">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="5417b-177">L’interface définie peut être utilisée quand c’est nécessaire, avec l’implémentation fournie par l’injection de dépendances et Refit :</span><span class="sxs-lookup"><span data-stu-id="5417b-177">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="5417b-178">Middleware pour les requêtes sortantes</span><span class="sxs-lookup"><span data-stu-id="5417b-178">Outgoing request middleware</span></span>

<span data-ttu-id="5417b-179">`HttpClient` présente le concept de délégation de gestionnaires qui peuvent être liés entre eux pour les requêtes HTTP sortantes.</span><span class="sxs-lookup"><span data-stu-id="5417b-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="5417b-180">`IHttpClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="5417b-180">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="5417b-181">Simplifie la définition des gestionnaires à appliquer pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="5417b-181">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="5417b-182">Prend en charge l’inscription et le chaînage de plusieurs gestionnaires pour générer un pipeline d’intergiciel (middleware) de demande sortante.</span><span class="sxs-lookup"><span data-stu-id="5417b-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="5417b-183">Chacun de ces gestionnaires peut effectuer un travail avant et après la requête sortante.</span><span class="sxs-lookup"><span data-stu-id="5417b-183">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="5417b-184">Ce modèle :</span><span class="sxs-lookup"><span data-stu-id="5417b-184">This pattern:</span></span>

  * <span data-ttu-id="5417b-185">Est semblable au pipeline d’intergiciel (middleware) entrant dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5417b-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="5417b-186">Fournit un mécanisme permettant de gérer les problèmes transversaux autour des requêtes HTTP, par exemple :</span><span class="sxs-lookup"><span data-stu-id="5417b-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="5417b-187">mettre en cache</span><span class="sxs-lookup"><span data-stu-id="5417b-187">caching</span></span>
    * <span data-ttu-id="5417b-188">gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="5417b-188">error handling</span></span>
    * <span data-ttu-id="5417b-189">sérialisation</span><span class="sxs-lookup"><span data-stu-id="5417b-189">serialization</span></span>
    * <span data-ttu-id="5417b-190">enregistrer</span><span class="sxs-lookup"><span data-stu-id="5417b-190">logging</span></span>

<span data-ttu-id="5417b-191">Pour créer un gestionnaire de délégation :</span><span class="sxs-lookup"><span data-stu-id="5417b-191">To create a delegating handler:</span></span>

* <span data-ttu-id="5417b-192">Dérivez de <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="5417b-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="5417b-193">Substituez <xref:System.Net.Http.DelegatingHandler.SendAsync*></span><span class="sxs-lookup"><span data-stu-id="5417b-193">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="5417b-194">Exécutez le code avant de passer la requête au gestionnaire suivant dans le pipeline :</span><span class="sxs-lookup"><span data-stu-id="5417b-194">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="5417b-195">Le code précédent vérifie si l’en-tête `X-API-KEY` est dans la demande.</span><span class="sxs-lookup"><span data-stu-id="5417b-195">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="5417b-196">Si `X-API-KEY` est manquant, <xref:System.Net.HttpStatusCode.BadRequest> est retourné.</span><span class="sxs-lookup"><span data-stu-id="5417b-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="5417b-197">Plusieurs gestionnaires peuvent être ajoutés à la configuration d’un `HttpClient` avec <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span><span class="sxs-lookup"><span data-stu-id="5417b-197">More than one handler can be added to the configuration for a `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="5417b-198">Dans le code précédent, le `ValidateHeaderHandler` est inscrit avec une injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="5417b-198">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="5417b-199">`IHttpClientFactory` crée une étendue DI distincte pour chaque gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="5417b-199">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="5417b-200">Les gestionnaires peuvent dépendre des services de toute portée.</span><span class="sxs-lookup"><span data-stu-id="5417b-200">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="5417b-201">Les services dont dépendent les gestionnaires sont supprimés lorsque le gestionnaire est supprimé.</span><span class="sxs-lookup"><span data-stu-id="5417b-201">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="5417b-202">Une fois inscrit, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> peut être appelé en passant en entrée le type pour le gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="5417b-202">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="5417b-203">Vous pouvez inscrire plusieurs gestionnaires dans l’ordre où ils doivent être exécutés.</span><span class="sxs-lookup"><span data-stu-id="5417b-203">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="5417b-204">Chaque gestionnaire wrappe le gestionnaire suivant jusqu’à ce que le dernier `HttpClientHandler` exécute la requête :</span><span class="sxs-lookup"><span data-stu-id="5417b-204">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="5417b-205">Utilisez l’une des approches suivantes pour partager l’état de chaque requête avec les gestionnaires de messages :</span><span class="sxs-lookup"><span data-stu-id="5417b-205">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="5417b-206">Transmettez les données dans le gestionnaire à l’aide de [HttpRequestMessage. Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span><span class="sxs-lookup"><span data-stu-id="5417b-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="5417b-207">Utilisez <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> pour accéder à la requête en cours.</span><span class="sxs-lookup"><span data-stu-id="5417b-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="5417b-208">Créez un objet de stockage <xref:System.Threading.AsyncLocal`1> personnalisé pour passer les données.</span><span class="sxs-lookup"><span data-stu-id="5417b-208">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="5417b-209">Utiliser les gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="5417b-209">Use Polly-based handlers</span></span>

<span data-ttu-id="5417b-210">`IHttpClientFactory` s’intègre à la bibliothèque tierce [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="5417b-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="5417b-211">Polly est une bibliothèque complète de gestion des erreurs transitoires et de résilience pour .NET.</span><span class="sxs-lookup"><span data-stu-id="5417b-211">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="5417b-212">Elle permet aux développeurs de formuler facilement et de façon thread-safe des stratégies, comme Retry (Nouvelle tentative), Circuit Breaker (Disjoncteur), Timeout (Délai d’attente), Bulkhead Isolation (Isolation par cloisonnement) et Fallback (Alternative de repli).</span><span class="sxs-lookup"><span data-stu-id="5417b-212">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="5417b-213">Des méthodes d’extension sont fournies pour permettre l’utilisation de stratégies Polly avec les instances configurées de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5417b-213">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="5417b-214">Les extensions Polly prennent en charge l’ajout de gestionnaires basés sur Polly aux clients.</span><span class="sxs-lookup"><span data-stu-id="5417b-214">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="5417b-215">Polly requiert le package NuGet [Microsoft. extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) .</span><span class="sxs-lookup"><span data-stu-id="5417b-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="5417b-216">Gérer les erreurs temporaires</span><span class="sxs-lookup"><span data-stu-id="5417b-216">Handle transient faults</span></span>

<span data-ttu-id="5417b-217">Les erreurs se produisent généralement lorsque les appels HTTP externes sont temporaires.</span><span class="sxs-lookup"><span data-stu-id="5417b-217">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="5417b-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> permet de définir une stratégie pour gérer les erreurs temporaires.</span><span class="sxs-lookup"><span data-stu-id="5417b-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="5417b-219">Les stratégies configurées avec `AddTransientHttpErrorPolicy` gèrent les réponses suivantes :</span><span class="sxs-lookup"><span data-stu-id="5417b-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="5417b-220">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="5417b-220">HTTP 5xx</span></span>
* <span data-ttu-id="5417b-221">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="5417b-221">HTTP 408</span></span>

<span data-ttu-id="5417b-222">`AddTransientHttpErrorPolicy` permet d’accéder à un objet `PolicyBuilder` configuré pour gérer les erreurs qui représentent une erreur temporaire possible :</span><span class="sxs-lookup"><span data-stu-id="5417b-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="5417b-223">Dans le code précédent, une stratégie `WaitAndRetryAsync` est définie.</span><span class="sxs-lookup"><span data-stu-id="5417b-223">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="5417b-224">Les requêtes qui ont échoué sont retentées jusqu’à trois fois avec un délai de 600 ms entre les tentatives.</span><span class="sxs-lookup"><span data-stu-id="5417b-224">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="5417b-225">Sélectionner dynamiquement des stratégies</span><span class="sxs-lookup"><span data-stu-id="5417b-225">Dynamically select policies</span></span>

<span data-ttu-id="5417b-226">Les méthodes d’extension sont fournies pour ajouter des gestionnaires basés sur Polly, par exemple, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span><span class="sxs-lookup"><span data-stu-id="5417b-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="5417b-227">La surcharge de `AddPolicyHandler` suivante inspecte la demande pour décider de la stratégie à appliquer :</span><span class="sxs-lookup"><span data-stu-id="5417b-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="5417b-228">Dans le code précédent, si la requête sortante est un HTTP GET, un délai d’attente de 10 secondes est appliqué.</span><span class="sxs-lookup"><span data-stu-id="5417b-228">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="5417b-229">Pour toutes les autres méthodes HTTP, un délai d’attente de 30 secondes est utilisé.</span><span class="sxs-lookup"><span data-stu-id="5417b-229">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="5417b-230">Ajouter plusieurs gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="5417b-230">Add multiple Polly handlers</span></span>

<span data-ttu-id="5417b-231">Il est courant d’imbriquer les stratégies Polly :</span><span class="sxs-lookup"><span data-stu-id="5417b-231">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="5417b-232">Dans l'exemple précédent :</span><span class="sxs-lookup"><span data-stu-id="5417b-232">In the preceding example:</span></span>

* <span data-ttu-id="5417b-233">Deux gestionnaires sont ajoutés.</span><span class="sxs-lookup"><span data-stu-id="5417b-233">Two handlers are added.</span></span>
* <span data-ttu-id="5417b-234">Le premier gestionnaire utilise <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> pour ajouter une stratégie de nouvelle tentative.</span><span class="sxs-lookup"><span data-stu-id="5417b-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="5417b-235">Les requêtes qui ont échoué sont retentées jusqu’à trois fois.</span><span class="sxs-lookup"><span data-stu-id="5417b-235">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="5417b-236">Le deuxième `AddTransientHttpErrorPolicy` appel ajoute une stratégie de disjoncteur.</span><span class="sxs-lookup"><span data-stu-id="5417b-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="5417b-237">Les demandes externes supplémentaires sont bloquées pendant 30 secondes si 5 tentatives ayant échoué se produisent de façon séquentielle.</span><span class="sxs-lookup"><span data-stu-id="5417b-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="5417b-238">Les stratégies de disjoncteur sont avec état.</span><span class="sxs-lookup"><span data-stu-id="5417b-238">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="5417b-239">Tous les appels effectués via ce client partagent le même état du circuit.</span><span class="sxs-lookup"><span data-stu-id="5417b-239">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="5417b-240">Ajouter des stratégies à partir du Registre Polly</span><span class="sxs-lookup"><span data-stu-id="5417b-240">Add policies from the Polly registry</span></span>

<span data-ttu-id="5417b-241">Une approche de la gestion des stratégies régulièrement utilisées consiste à les définir une seule fois et à les inscrire avec un `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="5417b-241">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="5417b-242">Dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="5417b-242">In the following code:</span></span>

* <span data-ttu-id="5417b-243">Les stratégies « régulières » et « longues » sont ajoutées.</span><span class="sxs-lookup"><span data-stu-id="5417b-243">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="5417b-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*> ajoute les stratégies « régulières » et « longues » à partir du Registre.</span><span class="sxs-lookup"><span data-stu-id="5417b-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="5417b-245">Pour plus d’informations sur les intégrations de `IHttpClientFactory` et Polly, consultez le [wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="5417b-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="5417b-246">HttpClient et gestion de la durée de vie</span><span class="sxs-lookup"><span data-stu-id="5417b-246">HttpClient and lifetime management</span></span>

<span data-ttu-id="5417b-247">Une nouvelle instance `HttpClient` est retournée à chaque fois que `CreateClient` est appelé sur `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="5417b-247">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="5417b-248">Un <xref:System.Net.Http.HttpMessageHandler> est créé par client nommé.</span><span class="sxs-lookup"><span data-stu-id="5417b-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="5417b-249">La fabrique gère les durées de vie des instances `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="5417b-249">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="5417b-250">`IHttpClientFactory` regroupe dans un pool les instances de `HttpMessageHandler` créées par la fabrique pour réduire la consommation des ressources.</span><span class="sxs-lookup"><span data-stu-id="5417b-250">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="5417b-251">Une instance de `HttpMessageHandler` peut être réutilisée à partir du pool quand vous créez une instance de `HttpClient` si sa durée de vie n’a pas expiré.</span><span class="sxs-lookup"><span data-stu-id="5417b-251">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="5417b-252">Le regroupement de gestionnaires en pools est souhaitable, car chaque gestionnaire gère habituellement ses propres connexions HTTP sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="5417b-252">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="5417b-253">La création de plus de gestionnaires que nécessaire peut entraîner des délais de connexion.</span><span class="sxs-lookup"><span data-stu-id="5417b-253">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="5417b-254">Certains gestionnaires maintiennent également les connexions ouvertes indéfiniment, ce qui peut empêcher le gestionnaire de réagir aux modifications DNS (Domain Name System).</span><span class="sxs-lookup"><span data-stu-id="5417b-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="5417b-255">La durée de vie par défaut d’un gestionnaire est de deux minutes.</span><span class="sxs-lookup"><span data-stu-id="5417b-255">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="5417b-256">La valeur par défaut peut être substituée pour chaque client nommé :</span><span class="sxs-lookup"><span data-stu-id="5417b-256">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="5417b-257">les instances de `HttpClient` peuvent généralement être traitées comme des objets .NET qui **ne nécessitent pas** de suppression.</span><span class="sxs-lookup"><span data-stu-id="5417b-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="5417b-258">La suppression annule les requêtes sortantes et garantit que l’instance `HttpClient` donnée ne peut pas être utilisée après avoir appelé <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="5417b-258">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="5417b-259">`IHttpClientFactory` effectue le suivi et libère les ressources utilisées par les instances `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5417b-259">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="5417b-260">Le fait de conserver une seule instance `HttpClient` active pendant une longue durée est un modèle commun utilisé avant le lancement de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="5417b-260">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="5417b-261">Ce modèle devient inutile après la migration vers `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="5417b-261">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="5417b-262">Journalisation</span><span class="sxs-lookup"><span data-stu-id="5417b-262">Logging</span></span>

<span data-ttu-id="5417b-263">Les clients créés via `IHttpClientFactory` enregistrent les messages de journalisation pour toutes les requêtes.</span><span class="sxs-lookup"><span data-stu-id="5417b-263">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="5417b-264">Activez le niveau d’information approprié dans la configuration de la journalisation pour voir les messages du journal par défaut.</span><span class="sxs-lookup"><span data-stu-id="5417b-264">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="5417b-265">Une journalisation supplémentaire, comme celle des en-têtes des requêtes, est incluse seulement au niveau de trace.</span><span class="sxs-lookup"><span data-stu-id="5417b-265">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="5417b-266">La catégorie de journal utilisée pour chaque client comprend le nom du client.</span><span class="sxs-lookup"><span data-stu-id="5417b-266">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="5417b-267">Un client nommé *MyNamedClient*, par exemple, journalise des messages avec la catégorie « System .net. http. httpclient ». **MyNamedClient**. LogicalHandler".</span><span class="sxs-lookup"><span data-stu-id="5417b-267">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="5417b-268">Les messages avec le suffixe *LogicalHandler* se produisent en dehors du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="5417b-268">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="5417b-269">Lors d’une requête, les messages sont journalisés avant que d’autres gestionnaires du pipeline l’aient traitée.</span><span class="sxs-lookup"><span data-stu-id="5417b-269">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="5417b-270">Lors d’une réponse, les messages sont journalisés une fois que tous les autres gestionnaires du pipeline ont reçu la réponse.</span><span class="sxs-lookup"><span data-stu-id="5417b-270">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="5417b-271">La journalisation se produit également à l’intérieur du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="5417b-271">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="5417b-272">Dans l’exemple *MyNamedClient* , ces messages sont journalisés avec la catégorie de journal « System .net. http. httpclient ». **MyNamedClient**. ClientHandler".</span><span class="sxs-lookup"><span data-stu-id="5417b-272">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="5417b-273">Pour la requête, cela se produit une fois que tous les autres gestionnaires ont été exécutés et immédiatement avant l’envoi de la demande.</span><span class="sxs-lookup"><span data-stu-id="5417b-273">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="5417b-274">Lors de la réponse, cette journalisation inclut l’état de la réponse avant qu’elle repasse à travers le pipeline de gestionnaires.</span><span class="sxs-lookup"><span data-stu-id="5417b-274">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="5417b-275">L’activation de la journalisation à l’extérieur et à l’intérieur du pipeline permet l’inspection des changements apportés par les autres gestionnaires du pipeline.</span><span class="sxs-lookup"><span data-stu-id="5417b-275">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="5417b-276">Cela peut inclure des modifications apportées aux en-têtes de demande ou au code d’état de réponse.</span><span class="sxs-lookup"><span data-stu-id="5417b-276">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="5417b-277">L’inclusion du nom du client dans la catégorie journal active le filtrage des journaux pour des clients nommés spécifiques.</span><span class="sxs-lookup"><span data-stu-id="5417b-277">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="5417b-278">Configurer le HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="5417b-278">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="5417b-279">Il peut être nécessaire de contrôler la configuration du `HttpMessageHandler` interne utilisé par un client.</span><span class="sxs-lookup"><span data-stu-id="5417b-279">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="5417b-280">Un `IHttpClientBuilder` est retourné quand vous ajoutez des clients nommés ou typés.</span><span class="sxs-lookup"><span data-stu-id="5417b-280">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="5417b-281">La méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> peut être utilisée pour définir un délégué.</span><span class="sxs-lookup"><span data-stu-id="5417b-281">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="5417b-282">Le délégué est utilisé pour créer et configurer le `HttpMessageHandler` principal utilisé par ce client :</span><span class="sxs-lookup"><span data-stu-id="5417b-282">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="5417b-283">Utiliser IHttpClientFactory dans une application console</span><span class="sxs-lookup"><span data-stu-id="5417b-283">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="5417b-284">Dans une application console, ajoutez les références de package suivantes au projet :</span><span class="sxs-lookup"><span data-stu-id="5417b-284">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="5417b-285">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="5417b-285">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="5417b-286">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="5417b-286">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="5417b-287">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="5417b-287">In the following example:</span></span>

* <span data-ttu-id="5417b-288"><xref:System.Net.Http.IHttpClientFactory> est inscrit dans le conteneur de service dans de l’[hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="5417b-288"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="5417b-289">`MyService` crée une instance de fabrique cliente à partir du service, qui est utilisée pour créer un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5417b-289">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="5417b-290">`HttpClient` est utilisé pour récupérer une page web.</span><span class="sxs-lookup"><span data-stu-id="5417b-290">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="5417b-291">`Main` crée une étendue pour exécuter la méthode `GetPage` du service et écrire les 500 premiers caractères du contenu de la page web dans la console.</span><span class="sxs-lookup"><span data-stu-id="5417b-291">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="5417b-292">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="5417b-292">Additional resources</span></span>

* [<span data-ttu-id="5417b-293">Utiliser HttpClientFactory pour implémenter des requêtes HTTP résilientes</span><span class="sxs-lookup"><span data-stu-id="5417b-293">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="5417b-294">Implémenter de nouvelles tentatives d’appel HTTP avec interruption exponentielle avec des stratégies Polly et HttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="5417b-294">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="5417b-295">Implémenter le modèle Disjoncteur</span><span class="sxs-lookup"><span data-stu-id="5417b-295">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="5417b-296">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) et [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="5417b-296">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="5417b-297">Une <xref:System.Net.Http.IHttpClientFactory> peut être inscrite et utilisée pour configurer et créer des instances de <xref:System.Net.Http.HttpClient> dans une application.</span><span class="sxs-lookup"><span data-stu-id="5417b-297">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="5417b-298">Elle offre les avantages suivants :</span><span class="sxs-lookup"><span data-stu-id="5417b-298">It offers the following benefits:</span></span>

* <span data-ttu-id="5417b-299">Fournit un emplacement central pour le nommage et la configuration d’instance de `HttpClient` logiques.</span><span class="sxs-lookup"><span data-stu-id="5417b-299">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="5417b-300">Par exemple, un client *github* peut être inscrit et configuré pour accéder à [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="5417b-300">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="5417b-301">Un client par défaut peut être inscrit à d’autres fins.</span><span class="sxs-lookup"><span data-stu-id="5417b-301">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="5417b-302">Codifie le concept de middleware (intergiciel) sortant via la délégation de gestionnaires dans `HttpClient` et fournit des extensions permettant au middleware Polly d’en tirer parti.</span><span class="sxs-lookup"><span data-stu-id="5417b-302">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="5417b-303">Gère le regroupement et la durée de vie des instances de `HttpClientMessageHandler` sous-jacentes pour éviter les problèmes DNS courants qui se produisent lors de la gestion manuelle des durées de vie de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5417b-303">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="5417b-304">Ajoute une expérience de journalisation configurable (via `ILogger`) pour toutes les requêtes envoyées via des clients créés par la fabrique.</span><span class="sxs-lookup"><span data-stu-id="5417b-304">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="5417b-305">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5417b-305">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="5417b-306">Modèles de consommation</span><span class="sxs-lookup"><span data-stu-id="5417b-306">Consumption patterns</span></span>

<span data-ttu-id="5417b-307">Vous pouvez utiliser `IHttpClientFactory` dans une application de plusieurs façons :</span><span class="sxs-lookup"><span data-stu-id="5417b-307">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="5417b-308">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="5417b-308">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="5417b-309">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="5417b-309">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="5417b-310">Clients typés</span><span class="sxs-lookup"><span data-stu-id="5417b-310">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="5417b-311">Clients générés</span><span class="sxs-lookup"><span data-stu-id="5417b-311">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="5417b-312">Aucune d’entre elles n’est meilleure qu’une autre.</span><span class="sxs-lookup"><span data-stu-id="5417b-312">None of them are strictly superior to another.</span></span> <span data-ttu-id="5417b-313">La meilleure approche dépend des contraintes de l’application.</span><span class="sxs-lookup"><span data-stu-id="5417b-313">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="5417b-314">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="5417b-314">Basic usage</span></span>

<span data-ttu-id="5417b-315">Vous pouvez inscrire la `IHttpClientFactory` en appelant la méthode d’extension `AddHttpClient` sur la `IServiceCollection`, à l’intérieur la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5417b-315">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="5417b-316">Une fois inscrit, le code peut accepter un `IHttpClientFactory` partout où des services peuvent être injectés avec [une injection de dépendance](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="5417b-316">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="5417b-317">La `IHttpClientFactory` peut être utilisée pour créer une instance de `HttpClient` :</span><span class="sxs-lookup"><span data-stu-id="5417b-317">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="5417b-318">L’utilisation de `IHttpClientFactory` de cette façon est un excellent moyen de refactoriser une application existante.</span><span class="sxs-lookup"><span data-stu-id="5417b-318">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="5417b-319">Elle n’a aucun impact sur la façon dont `HttpClient` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="5417b-319">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="5417b-320">Dans les endroits où les instances de `HttpClient` sont actuellement créées, remplacez ces occurrences par un appel à <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="5417b-320">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="5417b-321">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="5417b-321">Named clients</span></span>

<span data-ttu-id="5417b-322">Si une application nécessite plusieurs utilisations distinctes de `HttpClient`, chacune avec une configuration différente, une option consiste à utiliser des **clients nommés**.</span><span class="sxs-lookup"><span data-stu-id="5417b-322">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="5417b-323">La configuration d’un `HttpClient` nommé peut être spécifiée lors de l’inscription dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5417b-323">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="5417b-324">Dans le code précédent, `AddHttpClient` est appelé en fournissant le nom *github*.</span><span class="sxs-lookup"><span data-stu-id="5417b-324">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="5417b-325">Une configuration par défaut est appliquée à ce client : l’adresse de base et deux en-têtes nécessaires pour utiliser l’API GitHub.</span><span class="sxs-lookup"><span data-stu-id="5417b-325">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="5417b-326">Chaque fois que `CreateClient` est appelée, une nouvelle instance de `HttpClient` est créée et l’action de configuration est appelée.</span><span class="sxs-lookup"><span data-stu-id="5417b-326">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="5417b-327">Pour utiliser un client nommé, un paramètre de chaîne peut être passé à `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="5417b-327">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="5417b-328">Spécifiez le nom du client à créer :</span><span class="sxs-lookup"><span data-stu-id="5417b-328">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="5417b-329">Dans le code précédent, la requête n’a pas besoin de spécifier un nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="5417b-329">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="5417b-330">Elle peut simplement passer le chemin, car l’adresse de base configurée pour le client est utilisée.</span><span class="sxs-lookup"><span data-stu-id="5417b-330">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="5417b-331">Clients typés</span><span class="sxs-lookup"><span data-stu-id="5417b-331">Typed clients</span></span>

<span data-ttu-id="5417b-332">Clients typés :</span><span class="sxs-lookup"><span data-stu-id="5417b-332">Typed clients:</span></span>

* <span data-ttu-id="5417b-333">Fournissent les mêmes fonctionnalités que les clients nommés, sans qu’il soit nécessaire d’utiliser des chaînes comme clés.</span><span class="sxs-lookup"><span data-stu-id="5417b-333">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="5417b-334">Bénéficie de l’aide d’IntelliSense et du compilateur lors de l’utilisation des clients.</span><span class="sxs-lookup"><span data-stu-id="5417b-334">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="5417b-335">Fournissent un emplacement unique pour configurer et interagir avec un `HttpClient` particulier.</span><span class="sxs-lookup"><span data-stu-id="5417b-335">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="5417b-336">Par exemple, un même client typé peut être utilisé pour un point de terminaison de backend et pour encapsuler la logique relative à ce point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="5417b-336">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="5417b-337">Fonctionnent avec l’injection de dépendances et peuvent être injectés là où c’est nécessaire dans votre application.</span><span class="sxs-lookup"><span data-stu-id="5417b-337">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="5417b-338">Un client typé accepte un `HttpClient` dans son constructeur :</span><span class="sxs-lookup"><span data-stu-id="5417b-338">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="5417b-339">Dans le code précédent, la configuration est déplacée dans le client typé.</span><span class="sxs-lookup"><span data-stu-id="5417b-339">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="5417b-340">L’objet `HttpClient` est exposé en tant que propriété publique.</span><span class="sxs-lookup"><span data-stu-id="5417b-340">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="5417b-341">Il est possible de définir des méthodes d’API spécifiques qui exposent les fonctionnalités de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5417b-341">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="5417b-342">La méthode `GetAspNetDocsIssues` encapsule le code nécessaire pour interroger et analyser les problèmes ouverts les plus récents d’un dépôt GitHub.</span><span class="sxs-lookup"><span data-stu-id="5417b-342">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="5417b-343">Pour inscrire un client typé, la méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> générique peut être utilisée dans `Startup.ConfigureServices`, en spécifiant la classe du client typé :</span><span class="sxs-lookup"><span data-stu-id="5417b-343">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="5417b-344">Le client typé est inscrit comme étant transitoire avec injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="5417b-344">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="5417b-345">Le client typé peut être injecté et utilisé directement :</span><span class="sxs-lookup"><span data-stu-id="5417b-345">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="5417b-346">Si vous préférez, vous pouvez spécifier la configuration d’un client typé lors de l’inscription dans `Startup.ConfigureServices` au lieu de le faire dans le constructeur du client typé :</span><span class="sxs-lookup"><span data-stu-id="5417b-346">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="5417b-347">Il est possible d’encapsuler entièrement le `HttpClient` dans un client typé.</span><span class="sxs-lookup"><span data-stu-id="5417b-347">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="5417b-348">Au lieu de l’exposer en tant que propriété, vous pouvez fournir des méthodes publiques qui appellent l’instance de `HttpClient` en interne.</span><span class="sxs-lookup"><span data-stu-id="5417b-348">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="5417b-349">Dans le code précédent, le `HttpClient` est stocké en tant que champ privé.</span><span class="sxs-lookup"><span data-stu-id="5417b-349">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="5417b-350">Tous les accès nécessaires pour effectuer des appels externes passent par la méthode `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="5417b-350">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="5417b-351">Clients générés</span><span class="sxs-lookup"><span data-stu-id="5417b-351">Generated clients</span></span>

<span data-ttu-id="5417b-352">`IHttpClientFactory` peut être utilisé en combinaison avec d’autres bibliothèques tierces, comme [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="5417b-352">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="5417b-353">Refit est une bibliothèque REST pour .NET.</span><span class="sxs-lookup"><span data-stu-id="5417b-353">Refit is a REST library for .NET.</span></span> <span data-ttu-id="5417b-354">Il convertit les API REST en interfaces dynamiques.</span><span class="sxs-lookup"><span data-stu-id="5417b-354">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="5417b-355">Une implémentation de l’interface est générée dynamiquement par le `RestService`, avec `HttpClient` pour faire les appels HTTP externes.</span><span class="sxs-lookup"><span data-stu-id="5417b-355">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="5417b-356">Une interface et une réponse sont définies pour représenter l’API externe et sa réponse :</span><span class="sxs-lookup"><span data-stu-id="5417b-356">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="5417b-357">Vous pouvez ajouter un client typé en utilisant Refit pour générer l’implémentation :</span><span class="sxs-lookup"><span data-stu-id="5417b-357">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="5417b-358">L’interface définie peut être utilisée quand c’est nécessaire, avec l’implémentation fournie par l’injection de dépendances et Refit :</span><span class="sxs-lookup"><span data-stu-id="5417b-358">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="5417b-359">Middleware pour les requêtes sortantes</span><span class="sxs-lookup"><span data-stu-id="5417b-359">Outgoing request middleware</span></span>

<span data-ttu-id="5417b-360">`HttpClient` intègre déjà le concept de délégation des gestionnaires qui peuvent être liés ensemble pour les requêtes HTTP sortantes.</span><span class="sxs-lookup"><span data-stu-id="5417b-360">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="5417b-361">Le `IHttpClientFactory` permet de définir facilement les gestionnaires à appliquer pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="5417b-361">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="5417b-362">Il prend en charge l’inscription et le chaînage de plusieurs gestionnaires pour créer un pipeline de middlewares pour les requêtes sortantes.</span><span class="sxs-lookup"><span data-stu-id="5417b-362">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="5417b-363">Chacun de ces gestionnaires peut effectuer un travail avant et après la requête sortante.</span><span class="sxs-lookup"><span data-stu-id="5417b-363">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="5417b-364">Ce modèle est similaire au pipeline de middlewares entrants dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5417b-364">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="5417b-365">Le modèle fournit un mécanisme permettant de gérer les problèmes transversaux liés aux des requêtes HTTP, notamment la mise en cache, la gestion des erreurs, la sérialisation et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="5417b-365">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="5417b-366">Pour créer un gestionnaire, définissez une classe dérivant de <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="5417b-366">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="5417b-367">Remplacez la méthode `SendAsync` de façon à exécuter du code avant de passer la requête au gestionnaire suivant dans le pipeline :</span><span class="sxs-lookup"><span data-stu-id="5417b-367">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="5417b-368">Le code précédent définit un gestionnaire de base.</span><span class="sxs-lookup"><span data-stu-id="5417b-368">The preceding code defines a basic handler.</span></span> <span data-ttu-id="5417b-369">Il vérifie si un en-tête `X-API-KEY` a été inclus dans la requête.</span><span class="sxs-lookup"><span data-stu-id="5417b-369">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="5417b-370">Si l’en-tête est manquant, il peut éviter l’appel HTTP et retourner une réponse appropriée.</span><span class="sxs-lookup"><span data-stu-id="5417b-370">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="5417b-371">Lors de l’inscription, un ou plusieurs gestionnaires peuvent être ajoutés à la configuration pour un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5417b-371">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="5417b-372">Cette tâche est accomplie via des méthodes d’extension sur le <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5417b-372">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="5417b-373">Dans le code précédent, le `ValidateHeaderHandler` est inscrit avec une injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="5417b-373">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="5417b-374">`IHttpClientFactory` crée une étendue DI distincte pour chaque gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="5417b-374">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="5417b-375">Les gestionnaires peuvent librement dépendre des services de n’importe quelle étendue.</span><span class="sxs-lookup"><span data-stu-id="5417b-375">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="5417b-376">Les services dont dépendent les gestionnaires sont supprimés lorsque le gestionnaire est supprimé.</span><span class="sxs-lookup"><span data-stu-id="5417b-376">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="5417b-377">Une fois inscrit, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> peut être appelé en passant en entrée le type pour le gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="5417b-377">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="5417b-378">Vous pouvez inscrire plusieurs gestionnaires dans l’ordre où ils doivent être exécutés.</span><span class="sxs-lookup"><span data-stu-id="5417b-378">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="5417b-379">Chaque gestionnaire wrappe le gestionnaire suivant jusqu’à ce que le dernier `HttpClientHandler` exécute la requête :</span><span class="sxs-lookup"><span data-stu-id="5417b-379">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="5417b-380">Utilisez l’une des approches suivantes pour partager l’état de chaque requête avec les gestionnaires de messages :</span><span class="sxs-lookup"><span data-stu-id="5417b-380">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="5417b-381">Passez des données dans le gestionnaire en utilisant `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="5417b-381">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="5417b-382">Utilisez `IHttpContextAccessor` pour accéder à la requête en cours.</span><span class="sxs-lookup"><span data-stu-id="5417b-382">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="5417b-383">Créez un objet de stockage `AsyncLocal` personnalisé pour passer les données.</span><span class="sxs-lookup"><span data-stu-id="5417b-383">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="5417b-384">Utiliser les gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="5417b-384">Use Polly-based handlers</span></span>

<span data-ttu-id="5417b-385">`IHttpClientFactory` s’intègre à une bibliothèque tierce très utilisée, appelée [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="5417b-385">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="5417b-386">Polly est une bibliothèque complète de gestion des erreurs transitoires et de résilience pour .NET.</span><span class="sxs-lookup"><span data-stu-id="5417b-386">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="5417b-387">Elle permet aux développeurs de formuler facilement et de façon thread-safe des stratégies, comme Retry (Nouvelle tentative), Circuit Breaker (Disjoncteur), Timeout (Délai d’attente), Bulkhead Isolation (Isolation par cloisonnement) et Fallback (Alternative de repli).</span><span class="sxs-lookup"><span data-stu-id="5417b-387">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="5417b-388">Des méthodes d’extension sont fournies pour permettre l’utilisation de stratégies Polly avec les instances configurées de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5417b-388">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="5417b-389">Les extensions Polly :</span><span class="sxs-lookup"><span data-stu-id="5417b-389">The Polly extensions:</span></span>

* <span data-ttu-id="5417b-390">Prennent en charge l’ajout de gestionnaires Polly à des clients.</span><span class="sxs-lookup"><span data-stu-id="5417b-390">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="5417b-391">Peuvent être utilisées après l’installation du package NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="5417b-391">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="5417b-392">Ce package n’est pas inclus dans le framework partagé ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5417b-392">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="5417b-393">Gérer les erreurs temporaires</span><span class="sxs-lookup"><span data-stu-id="5417b-393">Handle transient faults</span></span>

<span data-ttu-id="5417b-394">Les erreurs courantes se produisent lorsque des appels HTTP externes sont temporaires.</span><span class="sxs-lookup"><span data-stu-id="5417b-394">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="5417b-395">Une méthode d’extension pratique nommée `AddTransientHttpErrorPolicy` est incluse : elle permet de définir une stratégie pour gérer les erreurs temporaires.</span><span class="sxs-lookup"><span data-stu-id="5417b-395">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="5417b-396">Les stratégies configurées avec cette méthode d’extension gèrent `HttpRequestException`, les réponses HTTP 5xx et les réponses HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="5417b-396">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="5417b-397">L’extension `AddTransientHttpErrorPolicy` peut être utilisée dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5417b-397">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5417b-398">L’extension fournit l’accès à un objet `PolicyBuilder` configuré pour gérer les erreurs représentant une erreur temporaire possible :</span><span class="sxs-lookup"><span data-stu-id="5417b-398">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="5417b-399">Dans le code précédent, une stratégie `WaitAndRetryAsync` est définie.</span><span class="sxs-lookup"><span data-stu-id="5417b-399">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="5417b-400">Les requêtes qui ont échoué sont retentées jusqu’à trois fois avec un délai de 600 ms entre les tentatives.</span><span class="sxs-lookup"><span data-stu-id="5417b-400">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="5417b-401">Sélectionner dynamiquement des stratégies</span><span class="sxs-lookup"><span data-stu-id="5417b-401">Dynamically select policies</span></span>

<span data-ttu-id="5417b-402">Il existe d’autres méthodes d’extension que vous pouvez utiliser pour ajouter des gestionnaires Polly.</span><span class="sxs-lookup"><span data-stu-id="5417b-402">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="5417b-403">Une de ces extensions est `AddPolicyHandler`, qui a plusieurs surcharges.</span><span class="sxs-lookup"><span data-stu-id="5417b-403">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="5417b-404">Une de ces surcharges permet l’inspection de la requête lors de la définition de la stratégie à appliquer :</span><span class="sxs-lookup"><span data-stu-id="5417b-404">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="5417b-405">Dans le code précédent, si la requête sortante est un HTTP GET, un délai d’attente de 10 secondes est appliqué.</span><span class="sxs-lookup"><span data-stu-id="5417b-405">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="5417b-406">Pour toutes les autres méthodes HTTP, un délai d’attente de 30 secondes est utilisé.</span><span class="sxs-lookup"><span data-stu-id="5417b-406">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="5417b-407">Ajouter plusieurs gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="5417b-407">Add multiple Polly handlers</span></span>

<span data-ttu-id="5417b-408">Il est courant d’imbriquer des stratégies Polly pour fournir des fonctionnalités améliorées :</span><span class="sxs-lookup"><span data-stu-id="5417b-408">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="5417b-409">Dans l’exemple précédent, deux gestionnaires sont ajoutés.</span><span class="sxs-lookup"><span data-stu-id="5417b-409">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="5417b-410">Le premier utilise l’extension `AddTransientHttpErrorPolicy` pour ajouter une stratégie de nouvelle tentative.</span><span class="sxs-lookup"><span data-stu-id="5417b-410">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="5417b-411">Les requêtes qui ont échoué sont retentées jusqu’à trois fois.</span><span class="sxs-lookup"><span data-stu-id="5417b-411">Failed requests are retried up to three times.</span></span> <span data-ttu-id="5417b-412">Le deuxième appel à `AddTransientHttpErrorPolicy` ajoute une stratégie de disjoncteur.</span><span class="sxs-lookup"><span data-stu-id="5417b-412">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="5417b-413">Les requêtes externes supplémentaires sont bloquées pendant 30 secondes si cinq tentatives successives échouent.</span><span class="sxs-lookup"><span data-stu-id="5417b-413">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="5417b-414">Les stratégies de disjoncteur sont avec état.</span><span class="sxs-lookup"><span data-stu-id="5417b-414">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="5417b-415">Tous les appels effectués via ce client partagent le même état du circuit.</span><span class="sxs-lookup"><span data-stu-id="5417b-415">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="5417b-416">Ajouter des stratégies à partir du Registre Polly</span><span class="sxs-lookup"><span data-stu-id="5417b-416">Add policies from the Polly registry</span></span>

<span data-ttu-id="5417b-417">Une approche de la gestion des stratégies régulièrement utilisées consiste à les définir une seule fois et à les inscrire avec un `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="5417b-417">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="5417b-418">Il existe une méthode d’extension qui permet l’ajout d’un gestionnaire avec une stratégie du Registre :</span><span class="sxs-lookup"><span data-stu-id="5417b-418">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="5417b-419">Dans le code précédent, deux stratégies sont inscrites lorsque `PolicyRegistry` est ajouté à `ServiceCollection`.</span><span class="sxs-lookup"><span data-stu-id="5417b-419">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="5417b-420">Pour utiliser une stratégie du Registre, la méthode `AddPolicyHandlerFromRegistry` est utilisée, en passant le nom de la stratégie à appliquer.</span><span class="sxs-lookup"><span data-stu-id="5417b-420">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="5417b-421">Vous trouverez plus d’informations sur les intégrations de `IHttpClientFactory` et de Polly dans le [wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="5417b-421">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="5417b-422">HttpClient et gestion de la durée de vie</span><span class="sxs-lookup"><span data-stu-id="5417b-422">HttpClient and lifetime management</span></span>

<span data-ttu-id="5417b-423">Une nouvelle instance `HttpClient` est retournée à chaque fois que `CreateClient` est appelé sur `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="5417b-423">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="5417b-424">Il existe un <xref:System.Net.Http.HttpMessageHandler> par client nommé.</span><span class="sxs-lookup"><span data-stu-id="5417b-424">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="5417b-425">La fabrique gère les durées de vie des instances `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="5417b-425">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="5417b-426">`IHttpClientFactory` regroupe dans un pool les instances de `HttpMessageHandler` créées par la fabrique pour réduire la consommation des ressources.</span><span class="sxs-lookup"><span data-stu-id="5417b-426">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="5417b-427">Une instance de `HttpMessageHandler` peut être réutilisée à partir du pool quand vous créez une instance de `HttpClient` si sa durée de vie n’a pas expiré.</span><span class="sxs-lookup"><span data-stu-id="5417b-427">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="5417b-428">Le regroupement de gestionnaires en pools est souhaitable, car chaque gestionnaire gère habituellement ses propres connexions HTTP sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="5417b-428">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="5417b-429">La création de plus de gestionnaires que nécessaire peut entraîner des délais de connexion.</span><span class="sxs-lookup"><span data-stu-id="5417b-429">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="5417b-430">Certains gestionnaires conservent aussi les connexions indéfiniment ouvertes, ce qui peut empêcher le gestionnaire de réagir aux changements du DNS.</span><span class="sxs-lookup"><span data-stu-id="5417b-430">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="5417b-431">La durée de vie par défaut d’un gestionnaire est de deux minutes.</span><span class="sxs-lookup"><span data-stu-id="5417b-431">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="5417b-432">La valeur par défaut peut être remplacée pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="5417b-432">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="5417b-433">Pour la remplacer, appelez <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> sur le `IHttpClientBuilder` qui est retourné lors de la création du client :</span><span class="sxs-lookup"><span data-stu-id="5417b-433">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="5417b-434">La suppression du client n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="5417b-434">Disposal of the client isn't required.</span></span> <span data-ttu-id="5417b-435">La suppression annule les requêtes sortantes et garantit que l’instance `HttpClient` donnée ne peut pas être utilisée après avoir appelé <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="5417b-435">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="5417b-436">`IHttpClientFactory` effectue le suivi et libère les ressources utilisées par les instances `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5417b-436">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="5417b-437">Les instances `HttpClient` peuvent généralement être traitées en tant qu’objets .NET ne nécessitant pas une suppression.</span><span class="sxs-lookup"><span data-stu-id="5417b-437">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="5417b-438">Le fait de conserver une seule instance `HttpClient` active pendant une longue durée est un modèle commun utilisé avant le lancement de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="5417b-438">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="5417b-439">Ce modèle devient inutile après la migration vers `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="5417b-439">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="5417b-440">Journalisation</span><span class="sxs-lookup"><span data-stu-id="5417b-440">Logging</span></span>

<span data-ttu-id="5417b-441">Les clients créés via `IHttpClientFactory` enregistrent les messages de journalisation pour toutes les requêtes.</span><span class="sxs-lookup"><span data-stu-id="5417b-441">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="5417b-442">Activez le niveau d’informations approprié dans votre configuration de journalisation pour voir les messages de journalisation par défaut.</span><span class="sxs-lookup"><span data-stu-id="5417b-442">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="5417b-443">Une journalisation supplémentaire, comme celle des en-têtes des requêtes, est incluse seulement au niveau de trace.</span><span class="sxs-lookup"><span data-stu-id="5417b-443">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="5417b-444">La catégorie de journal utilisée pour chaque client comprend le nom du client.</span><span class="sxs-lookup"><span data-stu-id="5417b-444">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="5417b-445">Par exemple, un client nommé *MyNamedClient* journalise les messages avec la catégorie `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="5417b-445">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="5417b-446">Les messages avec le suffixe *LogicalHandler* se produisent en dehors du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="5417b-446">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="5417b-447">Lors d’une requête, les messages sont journalisés avant que d’autres gestionnaires du pipeline l’aient traitée.</span><span class="sxs-lookup"><span data-stu-id="5417b-447">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="5417b-448">Lors d’une réponse, les messages sont journalisés une fois que tous les autres gestionnaires du pipeline ont reçu la réponse.</span><span class="sxs-lookup"><span data-stu-id="5417b-448">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="5417b-449">La journalisation se produit également à l’intérieur du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="5417b-449">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="5417b-450">Dans l’exemple *MyNamedClient*, ces messages sont journalisés avec la catégorie de journalisation `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="5417b-450">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="5417b-451">Pour la requête, cela se produit après que tous les autres gestionnaires ont été exécutés et immédiatement avant l’envoi de la requête sur le réseau.</span><span class="sxs-lookup"><span data-stu-id="5417b-451">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="5417b-452">Lors de la réponse, cette journalisation inclut l’état de la réponse avant qu’elle repasse à travers le pipeline de gestionnaires.</span><span class="sxs-lookup"><span data-stu-id="5417b-452">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="5417b-453">L’activation de la journalisation à l’extérieur et à l’intérieur du pipeline permet l’inspection des changements apportés par les autres gestionnaires du pipeline.</span><span class="sxs-lookup"><span data-stu-id="5417b-453">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="5417b-454">Par exemple, cela peut comprendre des changements apportés aux en-têtes des requêtes ou au code d’état de la réponse.</span><span class="sxs-lookup"><span data-stu-id="5417b-454">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="5417b-455">L’ajout du nom du client dans la catégorie de journalisation permet si nécessaire de filtrer le journal pour des clients nommés spécifiques.</span><span class="sxs-lookup"><span data-stu-id="5417b-455">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="5417b-456">Configurer le HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="5417b-456">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="5417b-457">Il peut être nécessaire de contrôler la configuration du `HttpMessageHandler` interne utilisé par un client.</span><span class="sxs-lookup"><span data-stu-id="5417b-457">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="5417b-458">Un `IHttpClientBuilder` est retourné quand vous ajoutez des clients nommés ou typés.</span><span class="sxs-lookup"><span data-stu-id="5417b-458">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="5417b-459">La méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> peut être utilisée pour définir un délégué.</span><span class="sxs-lookup"><span data-stu-id="5417b-459">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="5417b-460">Le délégué est utilisé pour créer et configurer le `HttpMessageHandler` principal utilisé par ce client :</span><span class="sxs-lookup"><span data-stu-id="5417b-460">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="5417b-461">Utiliser IHttpClientFactory dans une application console</span><span class="sxs-lookup"><span data-stu-id="5417b-461">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="5417b-462">Dans une application console, ajoutez les références de package suivantes au projet :</span><span class="sxs-lookup"><span data-stu-id="5417b-462">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="5417b-463">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="5417b-463">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="5417b-464">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="5417b-464">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="5417b-465">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="5417b-465">In the following example:</span></span>

* <span data-ttu-id="5417b-466"><xref:System.Net.Http.IHttpClientFactory> est inscrit dans le conteneur de service dans de l’[hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="5417b-466"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="5417b-467">`MyService` crée une instance de fabrique cliente à partir du service, qui est utilisée pour créer un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5417b-467">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="5417b-468">`HttpClient` est utilisé pour récupérer une page web.</span><span class="sxs-lookup"><span data-stu-id="5417b-468">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="5417b-469">`Main` crée une étendue pour exécuter la méthode `GetPage` du service et écrire les 500 premiers caractères du contenu de la page web dans la console.</span><span class="sxs-lookup"><span data-stu-id="5417b-469">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="5417b-470">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="5417b-470">Additional resources</span></span>

* [<span data-ttu-id="5417b-471">Utiliser HttpClientFactory pour implémenter des requêtes HTTP résilientes</span><span class="sxs-lookup"><span data-stu-id="5417b-471">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="5417b-472">Implémenter de nouvelles tentatives d’appel HTTP avec interruption exponentielle avec des stratégies Polly et HttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="5417b-472">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="5417b-473">Implémenter le modèle Disjoncteur</span><span class="sxs-lookup"><span data-stu-id="5417b-473">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<span data-ttu-id="5417b-474">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) et [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="5417b-474">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="5417b-475">Une <xref:System.Net.Http.IHttpClientFactory> peut être inscrite et utilisée pour configurer et créer des instances de <xref:System.Net.Http.HttpClient> dans une application.</span><span class="sxs-lookup"><span data-stu-id="5417b-475">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="5417b-476">Elle offre les avantages suivants :</span><span class="sxs-lookup"><span data-stu-id="5417b-476">It offers the following benefits:</span></span>

* <span data-ttu-id="5417b-477">Fournit un emplacement central pour le nommage et la configuration d’instance de `HttpClient` logiques.</span><span class="sxs-lookup"><span data-stu-id="5417b-477">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="5417b-478">Par exemple, un client *github* peut être inscrit et configuré pour accéder à [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="5417b-478">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="5417b-479">Un client par défaut peut être inscrit à d’autres fins.</span><span class="sxs-lookup"><span data-stu-id="5417b-479">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="5417b-480">Codifie le concept de middleware (intergiciel) sortant via la délégation de gestionnaires dans `HttpClient` et fournit des extensions permettant au middleware Polly d’en tirer parti.</span><span class="sxs-lookup"><span data-stu-id="5417b-480">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="5417b-481">Gère le regroupement et la durée de vie des instances de `HttpClientMessageHandler` sous-jacentes pour éviter les problèmes DNS courants qui se produisent lors de la gestion manuelle des durées de vie de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5417b-481">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="5417b-482">Ajoute une expérience de journalisation configurable (via `ILogger`) pour toutes les requêtes envoyées via des clients créés par la fabrique.</span><span class="sxs-lookup"><span data-stu-id="5417b-482">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="5417b-483">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5417b-483">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5417b-484">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="5417b-484">Prerequisites</span></span>

<span data-ttu-id="5417b-485">Les projets ciblant .NET Framework nécessitent l’installation du package NuGet [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/).</span><span class="sxs-lookup"><span data-stu-id="5417b-485">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="5417b-486">Les projets qui ciblent .NET Core et référencent le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) incluent déjà le package `Microsoft.Extensions.Http`.</span><span class="sxs-lookup"><span data-stu-id="5417b-486">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="5417b-487">Modèles de consommation</span><span class="sxs-lookup"><span data-stu-id="5417b-487">Consumption patterns</span></span>

<span data-ttu-id="5417b-488">Vous pouvez utiliser `IHttpClientFactory` dans une application de plusieurs façons :</span><span class="sxs-lookup"><span data-stu-id="5417b-488">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="5417b-489">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="5417b-489">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="5417b-490">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="5417b-490">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="5417b-491">Clients typés</span><span class="sxs-lookup"><span data-stu-id="5417b-491">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="5417b-492">Clients générés</span><span class="sxs-lookup"><span data-stu-id="5417b-492">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="5417b-493">Aucune d’entre elles n’est meilleure qu’une autre.</span><span class="sxs-lookup"><span data-stu-id="5417b-493">None of them are strictly superior to another.</span></span> <span data-ttu-id="5417b-494">La meilleure approche dépend des contraintes de l’application.</span><span class="sxs-lookup"><span data-stu-id="5417b-494">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="5417b-495">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="5417b-495">Basic usage</span></span>

<span data-ttu-id="5417b-496">Vous pouvez inscrire la `IHttpClientFactory` en appelant la méthode d’extension `AddHttpClient` sur la `IServiceCollection`, à l’intérieur la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5417b-496">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="5417b-497">Une fois inscrit, le code peut accepter un `IHttpClientFactory` partout où des services peuvent être injectés avec [une injection de dépendance](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="5417b-497">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="5417b-498">La `IHttpClientFactory` peut être utilisée pour créer une instance de `HttpClient` :</span><span class="sxs-lookup"><span data-stu-id="5417b-498">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="5417b-499">L’utilisation de `IHttpClientFactory` de cette façon est un excellent moyen de refactoriser une application existante.</span><span class="sxs-lookup"><span data-stu-id="5417b-499">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="5417b-500">Elle n’a aucun impact sur la façon dont `HttpClient` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="5417b-500">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="5417b-501">Dans les endroits où les instances de `HttpClient` sont actuellement créées, remplacez ces occurrences par un appel à <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="5417b-501">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="5417b-502">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="5417b-502">Named clients</span></span>

<span data-ttu-id="5417b-503">Si une application nécessite plusieurs utilisations distinctes de `HttpClient`, chacune avec une configuration différente, une option consiste à utiliser des **clients nommés**.</span><span class="sxs-lookup"><span data-stu-id="5417b-503">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="5417b-504">La configuration d’un `HttpClient` nommé peut être spécifiée lors de l’inscription dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5417b-504">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="5417b-505">Dans le code précédent, `AddHttpClient` est appelé en fournissant le nom *github*.</span><span class="sxs-lookup"><span data-stu-id="5417b-505">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="5417b-506">Une configuration par défaut est appliquée à ce client : l’adresse de base et deux en-têtes nécessaires pour utiliser l’API GitHub.</span><span class="sxs-lookup"><span data-stu-id="5417b-506">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="5417b-507">Chaque fois que `CreateClient` est appelée, une nouvelle instance de `HttpClient` est créée et l’action de configuration est appelée.</span><span class="sxs-lookup"><span data-stu-id="5417b-507">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="5417b-508">Pour utiliser un client nommé, un paramètre de chaîne peut être passé à `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="5417b-508">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="5417b-509">Spécifiez le nom du client à créer :</span><span class="sxs-lookup"><span data-stu-id="5417b-509">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="5417b-510">Dans le code précédent, la requête n’a pas besoin de spécifier un nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="5417b-510">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="5417b-511">Elle peut simplement passer le chemin, car l’adresse de base configurée pour le client est utilisée.</span><span class="sxs-lookup"><span data-stu-id="5417b-511">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="5417b-512">Clients typés</span><span class="sxs-lookup"><span data-stu-id="5417b-512">Typed clients</span></span>

<span data-ttu-id="5417b-513">Clients typés :</span><span class="sxs-lookup"><span data-stu-id="5417b-513">Typed clients:</span></span>

* <span data-ttu-id="5417b-514">Fournissent les mêmes fonctionnalités que les clients nommés, sans qu’il soit nécessaire d’utiliser des chaînes comme clés.</span><span class="sxs-lookup"><span data-stu-id="5417b-514">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="5417b-515">Bénéficie de l’aide d’IntelliSense et du compilateur lors de l’utilisation des clients.</span><span class="sxs-lookup"><span data-stu-id="5417b-515">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="5417b-516">Fournissent un emplacement unique pour configurer et interagir avec un `HttpClient` particulier.</span><span class="sxs-lookup"><span data-stu-id="5417b-516">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="5417b-517">Par exemple, un même client typé peut être utilisé pour un point de terminaison de backend et pour encapsuler la logique relative à ce point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="5417b-517">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="5417b-518">Fonctionnent avec l’injection de dépendances et peuvent être injectés là où c’est nécessaire dans votre application.</span><span class="sxs-lookup"><span data-stu-id="5417b-518">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="5417b-519">Un client typé accepte un `HttpClient` dans son constructeur :</span><span class="sxs-lookup"><span data-stu-id="5417b-519">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="5417b-520">Dans le code précédent, la configuration est déplacée dans le client typé.</span><span class="sxs-lookup"><span data-stu-id="5417b-520">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="5417b-521">L’objet `HttpClient` est exposé en tant que propriété publique.</span><span class="sxs-lookup"><span data-stu-id="5417b-521">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="5417b-522">Il est possible de définir des méthodes d’API spécifiques qui exposent les fonctionnalités de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5417b-522">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="5417b-523">La méthode `GetAspNetDocsIssues` encapsule le code nécessaire pour interroger et analyser les problèmes ouverts les plus récents d’un dépôt GitHub.</span><span class="sxs-lookup"><span data-stu-id="5417b-523">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="5417b-524">Pour inscrire un client typé, la méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> générique peut être utilisée dans `Startup.ConfigureServices`, en spécifiant la classe du client typé :</span><span class="sxs-lookup"><span data-stu-id="5417b-524">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="5417b-525">Le client typé est inscrit comme étant transitoire avec injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="5417b-525">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="5417b-526">Le client typé peut être injecté et utilisé directement :</span><span class="sxs-lookup"><span data-stu-id="5417b-526">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="5417b-527">Si vous préférez, vous pouvez spécifier la configuration d’un client typé lors de l’inscription dans `Startup.ConfigureServices` au lieu de le faire dans le constructeur du client typé :</span><span class="sxs-lookup"><span data-stu-id="5417b-527">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="5417b-528">Il est possible d’encapsuler entièrement le `HttpClient` dans un client typé.</span><span class="sxs-lookup"><span data-stu-id="5417b-528">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="5417b-529">Au lieu de l’exposer en tant que propriété, vous pouvez fournir des méthodes publiques qui appellent l’instance de `HttpClient` en interne.</span><span class="sxs-lookup"><span data-stu-id="5417b-529">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="5417b-530">Dans le code précédent, le `HttpClient` est stocké en tant que champ privé.</span><span class="sxs-lookup"><span data-stu-id="5417b-530">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="5417b-531">Tous les accès nécessaires pour effectuer des appels externes passent par la méthode `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="5417b-531">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="5417b-532">Clients générés</span><span class="sxs-lookup"><span data-stu-id="5417b-532">Generated clients</span></span>

<span data-ttu-id="5417b-533">`IHttpClientFactory` peut être utilisé en combinaison avec d’autres bibliothèques tierces, comme [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="5417b-533">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="5417b-534">Refit est une bibliothèque REST pour .NET.</span><span class="sxs-lookup"><span data-stu-id="5417b-534">Refit is a REST library for .NET.</span></span> <span data-ttu-id="5417b-535">Il convertit les API REST en interfaces dynamiques.</span><span class="sxs-lookup"><span data-stu-id="5417b-535">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="5417b-536">Une implémentation de l’interface est générée dynamiquement par le `RestService`, avec `HttpClient` pour faire les appels HTTP externes.</span><span class="sxs-lookup"><span data-stu-id="5417b-536">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="5417b-537">Une interface et une réponse sont définies pour représenter l’API externe et sa réponse :</span><span class="sxs-lookup"><span data-stu-id="5417b-537">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="5417b-538">Vous pouvez ajouter un client typé en utilisant Refit pour générer l’implémentation :</span><span class="sxs-lookup"><span data-stu-id="5417b-538">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="5417b-539">L’interface définie peut être utilisée quand c’est nécessaire, avec l’implémentation fournie par l’injection de dépendances et Refit :</span><span class="sxs-lookup"><span data-stu-id="5417b-539">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="5417b-540">Middleware pour les requêtes sortantes</span><span class="sxs-lookup"><span data-stu-id="5417b-540">Outgoing request middleware</span></span>

<span data-ttu-id="5417b-541">`HttpClient` intègre déjà le concept de délégation des gestionnaires qui peuvent être liés ensemble pour les requêtes HTTP sortantes.</span><span class="sxs-lookup"><span data-stu-id="5417b-541">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="5417b-542">Le `IHttpClientFactory` permet de définir facilement les gestionnaires à appliquer pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="5417b-542">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="5417b-543">Il prend en charge l’inscription et le chaînage de plusieurs gestionnaires pour créer un pipeline de middlewares pour les requêtes sortantes.</span><span class="sxs-lookup"><span data-stu-id="5417b-543">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="5417b-544">Chacun de ces gestionnaires peut effectuer un travail avant et après la requête sortante.</span><span class="sxs-lookup"><span data-stu-id="5417b-544">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="5417b-545">Ce modèle est similaire au pipeline de middlewares entrants dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5417b-545">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="5417b-546">Le modèle fournit un mécanisme permettant de gérer les problèmes transversaux liés aux des requêtes HTTP, notamment la mise en cache, la gestion des erreurs, la sérialisation et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="5417b-546">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="5417b-547">Pour créer un gestionnaire, définissez une classe dérivant de <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="5417b-547">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="5417b-548">Remplacez la méthode `SendAsync` de façon à exécuter du code avant de passer la requête au gestionnaire suivant dans le pipeline :</span><span class="sxs-lookup"><span data-stu-id="5417b-548">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="5417b-549">Le code précédent définit un gestionnaire de base.</span><span class="sxs-lookup"><span data-stu-id="5417b-549">The preceding code defines a basic handler.</span></span> <span data-ttu-id="5417b-550">Il vérifie si un en-tête `X-API-KEY` a été inclus dans la requête.</span><span class="sxs-lookup"><span data-stu-id="5417b-550">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="5417b-551">Si l’en-tête est manquant, il peut éviter l’appel HTTP et retourner une réponse appropriée.</span><span class="sxs-lookup"><span data-stu-id="5417b-551">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="5417b-552">Lors de l’inscription, un ou plusieurs gestionnaires peuvent être ajoutés à la configuration pour un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5417b-552">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="5417b-553">Cette tâche est accomplie via des méthodes d’extension sur le <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="5417b-553">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="5417b-554">Dans le code précédent, le `ValidateHeaderHandler` est inscrit avec une injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="5417b-554">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="5417b-555">Le gestionnaire **doit** être inscrit dans l’injection de dépendances en tant que service temporaire, sans étendue.</span><span class="sxs-lookup"><span data-stu-id="5417b-555">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="5417b-556">Si le gestionnaire est inscrit en tant que service étendu et que des services dont dépend le gestionnaire peuvent être supprimés :</span><span class="sxs-lookup"><span data-stu-id="5417b-556">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="5417b-557">Les services du gestionnaire peuvent être supprimés avant que le gestionnaire ne soit hors de portée.</span><span class="sxs-lookup"><span data-stu-id="5417b-557">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="5417b-558">Les services du gestionnaire supprimés entraînent un échec du gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="5417b-558">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="5417b-559">Une fois inscrit, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> peut être appelé en passant en entrée le type du gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="5417b-559">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="5417b-560">Vous pouvez inscrire plusieurs gestionnaires dans l’ordre où ils doivent être exécutés.</span><span class="sxs-lookup"><span data-stu-id="5417b-560">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="5417b-561">Chaque gestionnaire wrappe le gestionnaire suivant jusqu’à ce que le dernier `HttpClientHandler` exécute la requête :</span><span class="sxs-lookup"><span data-stu-id="5417b-561">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="5417b-562">Utilisez l’une des approches suivantes pour partager l’état de chaque requête avec les gestionnaires de messages :</span><span class="sxs-lookup"><span data-stu-id="5417b-562">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="5417b-563">Passez des données dans le gestionnaire en utilisant `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="5417b-563">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="5417b-564">Utilisez `IHttpContextAccessor` pour accéder à la requête en cours.</span><span class="sxs-lookup"><span data-stu-id="5417b-564">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="5417b-565">Créez un objet de stockage `AsyncLocal` personnalisé pour passer les données.</span><span class="sxs-lookup"><span data-stu-id="5417b-565">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="5417b-566">Utiliser les gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="5417b-566">Use Polly-based handlers</span></span>

<span data-ttu-id="5417b-567">`IHttpClientFactory` s’intègre à une bibliothèque tierce très utilisée, appelée [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="5417b-567">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="5417b-568">Polly est une bibliothèque complète de gestion des erreurs transitoires et de résilience pour .NET.</span><span class="sxs-lookup"><span data-stu-id="5417b-568">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="5417b-569">Elle permet aux développeurs de formuler facilement et de façon thread-safe des stratégies, comme Retry (Nouvelle tentative), Circuit Breaker (Disjoncteur), Timeout (Délai d’attente), Bulkhead Isolation (Isolation par cloisonnement) et Fallback (Alternative de repli).</span><span class="sxs-lookup"><span data-stu-id="5417b-569">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="5417b-570">Des méthodes d’extension sont fournies pour permettre l’utilisation de stratégies Polly avec les instances configurées de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5417b-570">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="5417b-571">Les extensions Polly :</span><span class="sxs-lookup"><span data-stu-id="5417b-571">The Polly extensions:</span></span>

* <span data-ttu-id="5417b-572">Prennent en charge l’ajout de gestionnaires Polly à des clients.</span><span class="sxs-lookup"><span data-stu-id="5417b-572">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="5417b-573">Peuvent être utilisées après l’installation du package NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="5417b-573">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="5417b-574">Ce package n’est pas inclus dans le framework partagé ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5417b-574">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="5417b-575">Gérer les erreurs temporaires</span><span class="sxs-lookup"><span data-stu-id="5417b-575">Handle transient faults</span></span>

<span data-ttu-id="5417b-576">Les erreurs courantes se produisent lorsque des appels HTTP externes sont temporaires.</span><span class="sxs-lookup"><span data-stu-id="5417b-576">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="5417b-577">Une méthode d’extension pratique nommée `AddTransientHttpErrorPolicy` est incluse : elle permet de définir une stratégie pour gérer les erreurs temporaires.</span><span class="sxs-lookup"><span data-stu-id="5417b-577">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="5417b-578">Les stratégies configurées avec cette méthode d’extension gèrent `HttpRequestException`, les réponses HTTP 5xx et les réponses HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="5417b-578">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="5417b-579">L’extension `AddTransientHttpErrorPolicy` peut être utilisée dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5417b-579">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5417b-580">L’extension fournit l’accès à un objet `PolicyBuilder` configuré pour gérer les erreurs représentant une erreur temporaire possible :</span><span class="sxs-lookup"><span data-stu-id="5417b-580">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="5417b-581">Dans le code précédent, une stratégie `WaitAndRetryAsync` est définie.</span><span class="sxs-lookup"><span data-stu-id="5417b-581">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="5417b-582">Les requêtes qui ont échoué sont retentées jusqu’à trois fois avec un délai de 600 ms entre les tentatives.</span><span class="sxs-lookup"><span data-stu-id="5417b-582">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="5417b-583">Sélectionner dynamiquement des stratégies</span><span class="sxs-lookup"><span data-stu-id="5417b-583">Dynamically select policies</span></span>

<span data-ttu-id="5417b-584">Il existe d’autres méthodes d’extension que vous pouvez utiliser pour ajouter des gestionnaires Polly.</span><span class="sxs-lookup"><span data-stu-id="5417b-584">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="5417b-585">Une de ces extensions est `AddPolicyHandler`, qui a plusieurs surcharges.</span><span class="sxs-lookup"><span data-stu-id="5417b-585">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="5417b-586">Une de ces surcharges permet l’inspection de la requête lors de la définition de la stratégie à appliquer :</span><span class="sxs-lookup"><span data-stu-id="5417b-586">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="5417b-587">Dans le code précédent, si la requête sortante est un HTTP GET, un délai d’attente de 10 secondes est appliqué.</span><span class="sxs-lookup"><span data-stu-id="5417b-587">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="5417b-588">Pour toutes les autres méthodes HTTP, un délai d’attente de 30 secondes est utilisé.</span><span class="sxs-lookup"><span data-stu-id="5417b-588">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="5417b-589">Ajouter plusieurs gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="5417b-589">Add multiple Polly handlers</span></span>

<span data-ttu-id="5417b-590">Il est courant d’imbriquer des stratégies Polly pour fournir des fonctionnalités améliorées :</span><span class="sxs-lookup"><span data-stu-id="5417b-590">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="5417b-591">Dans l’exemple précédent, deux gestionnaires sont ajoutés.</span><span class="sxs-lookup"><span data-stu-id="5417b-591">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="5417b-592">Le premier utilise l’extension `AddTransientHttpErrorPolicy` pour ajouter une stratégie de nouvelle tentative.</span><span class="sxs-lookup"><span data-stu-id="5417b-592">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="5417b-593">Les requêtes qui ont échoué sont retentées jusqu’à trois fois.</span><span class="sxs-lookup"><span data-stu-id="5417b-593">Failed requests are retried up to three times.</span></span> <span data-ttu-id="5417b-594">Le deuxième appel à `AddTransientHttpErrorPolicy` ajoute une stratégie de disjoncteur.</span><span class="sxs-lookup"><span data-stu-id="5417b-594">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="5417b-595">Les requêtes externes supplémentaires sont bloquées pendant 30 secondes si cinq tentatives successives échouent.</span><span class="sxs-lookup"><span data-stu-id="5417b-595">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="5417b-596">Les stratégies de disjoncteur sont avec état.</span><span class="sxs-lookup"><span data-stu-id="5417b-596">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="5417b-597">Tous les appels effectués via ce client partagent le même état du circuit.</span><span class="sxs-lookup"><span data-stu-id="5417b-597">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="5417b-598">Ajouter des stratégies à partir du Registre Polly</span><span class="sxs-lookup"><span data-stu-id="5417b-598">Add policies from the Polly registry</span></span>

<span data-ttu-id="5417b-599">Une approche de la gestion des stratégies régulièrement utilisées consiste à les définir une seule fois et à les inscrire avec un `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="5417b-599">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="5417b-600">Il existe une méthode d’extension qui permet l’ajout d’un gestionnaire avec une stratégie du Registre :</span><span class="sxs-lookup"><span data-stu-id="5417b-600">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="5417b-601">Dans le code précédent, deux stratégies sont inscrites lorsque `PolicyRegistry` est ajouté à `ServiceCollection`.</span><span class="sxs-lookup"><span data-stu-id="5417b-601">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="5417b-602">Pour utiliser une stratégie du Registre, la méthode `AddPolicyHandlerFromRegistry` est utilisée, en passant le nom de la stratégie à appliquer.</span><span class="sxs-lookup"><span data-stu-id="5417b-602">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="5417b-603">Vous trouverez plus d’informations sur les intégrations de `IHttpClientFactory` et de Polly dans le [wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="5417b-603">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="5417b-604">HttpClient et gestion de la durée de vie</span><span class="sxs-lookup"><span data-stu-id="5417b-604">HttpClient and lifetime management</span></span>

<span data-ttu-id="5417b-605">Une nouvelle instance `HttpClient` est retournée à chaque fois que `CreateClient` est appelé sur `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="5417b-605">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="5417b-606">Il existe un <xref:System.Net.Http.HttpMessageHandler> par client nommé.</span><span class="sxs-lookup"><span data-stu-id="5417b-606">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="5417b-607">La fabrique gère les durées de vie des instances `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="5417b-607">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="5417b-608">`IHttpClientFactory` regroupe dans un pool les instances de `HttpMessageHandler` créées par la fabrique pour réduire la consommation des ressources.</span><span class="sxs-lookup"><span data-stu-id="5417b-608">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="5417b-609">Une instance de `HttpMessageHandler` peut être réutilisée à partir du pool quand vous créez une instance de `HttpClient` si sa durée de vie n’a pas expiré.</span><span class="sxs-lookup"><span data-stu-id="5417b-609">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="5417b-610">Le regroupement de gestionnaires en pools est souhaitable, car chaque gestionnaire gère habituellement ses propres connexions HTTP sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="5417b-610">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="5417b-611">La création de plus de gestionnaires que nécessaire peut entraîner des délais de connexion.</span><span class="sxs-lookup"><span data-stu-id="5417b-611">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="5417b-612">Certains gestionnaires conservent aussi les connexions indéfiniment ouvertes, ce qui peut empêcher le gestionnaire de réagir aux changements du DNS.</span><span class="sxs-lookup"><span data-stu-id="5417b-612">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="5417b-613">La durée de vie par défaut d’un gestionnaire est de deux minutes.</span><span class="sxs-lookup"><span data-stu-id="5417b-613">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="5417b-614">La valeur par défaut peut être remplacée pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="5417b-614">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="5417b-615">Pour la remplacer, appelez <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> sur le `IHttpClientBuilder` qui est retourné lors de la création du client :</span><span class="sxs-lookup"><span data-stu-id="5417b-615">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="5417b-616">La suppression du client n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="5417b-616">Disposal of the client isn't required.</span></span> <span data-ttu-id="5417b-617">La suppression annule les requêtes sortantes et garantit que l’instance `HttpClient` donnée ne peut pas être utilisée après avoir appelé <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="5417b-617">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="5417b-618">`IHttpClientFactory` effectue le suivi et libère les ressources utilisées par les instances `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5417b-618">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="5417b-619">Les instances `HttpClient` peuvent généralement être traitées en tant qu’objets .NET ne nécessitant pas une suppression.</span><span class="sxs-lookup"><span data-stu-id="5417b-619">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="5417b-620">Le fait de conserver une seule instance `HttpClient` active pendant une longue durée est un modèle commun utilisé avant le lancement de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="5417b-620">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="5417b-621">Ce modèle devient inutile après la migration vers `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="5417b-621">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="5417b-622">Journalisation</span><span class="sxs-lookup"><span data-stu-id="5417b-622">Logging</span></span>

<span data-ttu-id="5417b-623">Les clients créés via `IHttpClientFactory` enregistrent les messages de journalisation pour toutes les requêtes.</span><span class="sxs-lookup"><span data-stu-id="5417b-623">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="5417b-624">Activez le niveau d’informations approprié dans votre configuration de journalisation pour voir les messages de journalisation par défaut.</span><span class="sxs-lookup"><span data-stu-id="5417b-624">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="5417b-625">Une journalisation supplémentaire, comme celle des en-têtes des requêtes, est incluse seulement au niveau de trace.</span><span class="sxs-lookup"><span data-stu-id="5417b-625">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="5417b-626">La catégorie de journal utilisée pour chaque client comprend le nom du client.</span><span class="sxs-lookup"><span data-stu-id="5417b-626">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="5417b-627">Par exemple, un client nommé *MyNamedClient* journalise les messages avec la catégorie `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="5417b-627">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="5417b-628">Les messages avec le suffixe *LogicalHandler* se produisent en dehors du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="5417b-628">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="5417b-629">Lors d’une requête, les messages sont journalisés avant que d’autres gestionnaires du pipeline l’aient traitée.</span><span class="sxs-lookup"><span data-stu-id="5417b-629">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="5417b-630">Lors d’une réponse, les messages sont journalisés une fois que tous les autres gestionnaires du pipeline ont reçu la réponse.</span><span class="sxs-lookup"><span data-stu-id="5417b-630">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="5417b-631">La journalisation se produit également à l’intérieur du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="5417b-631">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="5417b-632">Dans l’exemple *MyNamedClient*, ces messages sont journalisés avec la catégorie de journalisation `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="5417b-632">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="5417b-633">Pour la requête, cela se produit après que tous les autres gestionnaires ont été exécutés et immédiatement avant l’envoi de la requête sur le réseau.</span><span class="sxs-lookup"><span data-stu-id="5417b-633">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="5417b-634">Lors de la réponse, cette journalisation inclut l’état de la réponse avant qu’elle repasse à travers le pipeline de gestionnaires.</span><span class="sxs-lookup"><span data-stu-id="5417b-634">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="5417b-635">L’activation de la journalisation à l’extérieur et à l’intérieur du pipeline permet l’inspection des changements apportés par les autres gestionnaires du pipeline.</span><span class="sxs-lookup"><span data-stu-id="5417b-635">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="5417b-636">Par exemple, cela peut comprendre des changements apportés aux en-têtes des requêtes ou au code d’état de la réponse.</span><span class="sxs-lookup"><span data-stu-id="5417b-636">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="5417b-637">L’ajout du nom du client dans la catégorie de journalisation permet si nécessaire de filtrer le journal pour des clients nommés spécifiques.</span><span class="sxs-lookup"><span data-stu-id="5417b-637">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="5417b-638">Configurer le HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="5417b-638">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="5417b-639">Il peut être nécessaire de contrôler la configuration du `HttpMessageHandler` interne utilisé par un client.</span><span class="sxs-lookup"><span data-stu-id="5417b-639">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="5417b-640">Un `IHttpClientBuilder` est retourné quand vous ajoutez des clients nommés ou typés.</span><span class="sxs-lookup"><span data-stu-id="5417b-640">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="5417b-641">La méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> peut être utilisée pour définir un délégué.</span><span class="sxs-lookup"><span data-stu-id="5417b-641">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="5417b-642">Le délégué est utilisé pour créer et configurer le `HttpMessageHandler` principal utilisé par ce client :</span><span class="sxs-lookup"><span data-stu-id="5417b-642">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="5417b-643">Utiliser IHttpClientFactory dans une application console</span><span class="sxs-lookup"><span data-stu-id="5417b-643">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="5417b-644">Dans une application console, ajoutez les références de package suivantes au projet :</span><span class="sxs-lookup"><span data-stu-id="5417b-644">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="5417b-645">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="5417b-645">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="5417b-646">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="5417b-646">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="5417b-647">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="5417b-647">In the following example:</span></span>

* <span data-ttu-id="5417b-648"><xref:System.Net.Http.IHttpClientFactory> est inscrit dans le conteneur de service dans de l’[hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="5417b-648"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="5417b-649">`MyService` crée une instance de fabrique cliente à partir du service, qui est utilisée pour créer un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5417b-649">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="5417b-650">`HttpClient` est utilisé pour récupérer une page web.</span><span class="sxs-lookup"><span data-stu-id="5417b-650">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="5417b-651">`Main` crée une étendue pour exécuter la méthode `GetPage` du service et écrire les 500 premiers caractères du contenu de la page web dans la console.</span><span class="sxs-lookup"><span data-stu-id="5417b-651">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="5417b-652">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="5417b-652">Additional resources</span></span>

* [<span data-ttu-id="5417b-653">Utiliser HttpClientFactory pour implémenter des requêtes HTTP résilientes</span><span class="sxs-lookup"><span data-stu-id="5417b-653">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="5417b-654">Implémenter de nouvelles tentatives d’appel HTTP avec interruption exponentielle avec des stratégies Polly et HttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="5417b-654">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="5417b-655">Implémenter le modèle Disjoncteur</span><span class="sxs-lookup"><span data-stu-id="5417b-655">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
