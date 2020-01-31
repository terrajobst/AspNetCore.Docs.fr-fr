---
title: Effectuer des requêtes HTTP en utilisant IHttpClientFactory dans ASP.NET Core
author: stevejgordon
description: Découvrez plus d’informations sur l’utilisation de l’interface IHttpClientFactory pour gérer des instances HttpClient logiques dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/16/2019
uid: fundamentals/http-requests
ms.openlocfilehash: 9b9da82191a587be0603ee114562e9a964f05250
ms.sourcegitcommit: fe41cff0b99f3920b727286944e5b652ca301640
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76870396"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="7313b-103">Effectuer des requêtes HTTP en utilisant IHttpClientFactory dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7313b-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7313b-104">Par [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT)et [Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="7313b-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="7313b-105">Une <xref:System.Net.Http.IHttpClientFactory> peut être inscrite et utilisée pour configurer et créer des instances de <xref:System.Net.Http.HttpClient> dans une application.</span><span class="sxs-lookup"><span data-stu-id="7313b-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="7313b-106">`IHttpClientFactory` offre les avantages suivants :</span><span class="sxs-lookup"><span data-stu-id="7313b-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="7313b-107">Fournit un emplacement central pour le nommage et la configuration d’instance de `HttpClient` logiques.</span><span class="sxs-lookup"><span data-stu-id="7313b-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="7313b-108">Par exemple, un client nommé *GitHub* peut être inscrit et configuré pour accéder à [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="7313b-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="7313b-109">Un client par défaut peut être inscrit pour un accès général.</span><span class="sxs-lookup"><span data-stu-id="7313b-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="7313b-110">Codifie le concept d’intergiciel (middleware) sortant via la délégation de gestionnaires dans `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="7313b-111">Fournit des extensions pour l’intergiciel (middleware) basé sur Polly pour tirer parti des gestionnaires de délégation dans `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="7313b-112">Gère le regroupement et la durée de vie des instances de `HttpClientMessageHandler` sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="7313b-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="7313b-113">La gestion automatique évite les problèmes courants liés au DNS (Domain Name System) qui se produisent lors de la gestion manuelle des durées de vie de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="7313b-114">Ajoute une expérience de journalisation configurable (via `ILogger`) pour toutes les requêtes envoyées via des clients créés par la fabrique.</span><span class="sxs-lookup"><span data-stu-id="7313b-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="7313b-115">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="7313b-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="7313b-116">L’exemple de code dans cette version de rubrique utilise <xref:System.Text.Json> pour désérialiser le contenu JSON renvoyé dans les réponses HTTP.</span><span class="sxs-lookup"><span data-stu-id="7313b-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="7313b-117">Pour obtenir des exemples qui utilisent des `Json.NET` et des `ReadAsAsync<T>`, utilisez le sélecteur de version pour sélectionner une version 2. x de cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="7313b-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="7313b-118">Modèles de consommation</span><span class="sxs-lookup"><span data-stu-id="7313b-118">Consumption patterns</span></span>

<span data-ttu-id="7313b-119">Vous pouvez utiliser `IHttpClientFactory` dans une application de plusieurs façons :</span><span class="sxs-lookup"><span data-stu-id="7313b-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="7313b-120">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="7313b-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="7313b-121">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="7313b-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="7313b-122">Clients typés</span><span class="sxs-lookup"><span data-stu-id="7313b-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="7313b-123">Clients générés</span><span class="sxs-lookup"><span data-stu-id="7313b-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="7313b-124">La meilleure approche dépend des exigences de l’application.</span><span class="sxs-lookup"><span data-stu-id="7313b-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="7313b-125">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="7313b-125">Basic usage</span></span>

<span data-ttu-id="7313b-126">`IHttpClientFactory` peut être inscrit en appelant `AddHttpClient`:</span><span class="sxs-lookup"><span data-stu-id="7313b-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="7313b-127">Un `IHttpClientFactory` peut être demandé à l’aide [de l’injection de dépendances (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7313b-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="7313b-128">Le code suivant utilise `IHttpClientFactory` pour créer une instance `HttpClient` :</span><span class="sxs-lookup"><span data-stu-id="7313b-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="7313b-129">L’utilisation de `IHttpClientFactory` comme dans l’exemple précédent est un bon moyen de refactoriser une application existante.</span><span class="sxs-lookup"><span data-stu-id="7313b-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="7313b-130">Elle n’a aucun impact sur l’utilisation de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="7313b-131">Dans les emplacements où les instances de `HttpClient` sont créées dans une application existante, remplacez-les par des appels à <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="7313b-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="7313b-132">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="7313b-132">Named clients</span></span>

<span data-ttu-id="7313b-133">Les clients nommés sont un bon choix lorsque :</span><span class="sxs-lookup"><span data-stu-id="7313b-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="7313b-134">L’application requiert de nombreuses utilisations distinctes de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="7313b-135">De nombreuses `HttpClient`s ont une configuration différente.</span><span class="sxs-lookup"><span data-stu-id="7313b-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="7313b-136">La configuration d’un `HttpClient` nommé peut être spécifiée lors de l’inscription dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7313b-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="7313b-137">Dans le code précédent, le client est configuré avec :</span><span class="sxs-lookup"><span data-stu-id="7313b-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="7313b-138">Adresse de base `https://api.github.com/`.</span><span class="sxs-lookup"><span data-stu-id="7313b-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="7313b-139">Deux en-têtes nécessaires à l’utilisation de l’API GitHub.</span><span class="sxs-lookup"><span data-stu-id="7313b-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="7313b-140">CreateClient</span><span class="sxs-lookup"><span data-stu-id="7313b-140">CreateClient</span></span>

<span data-ttu-id="7313b-141">Chaque fois que <xref:System.Net.Http.IHttpClientFactory.CreateClient*> est appelée :</span><span class="sxs-lookup"><span data-stu-id="7313b-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="7313b-142">Une nouvelle instance de `HttpClient` est créée.</span><span class="sxs-lookup"><span data-stu-id="7313b-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="7313b-143">L’action de configuration est appelée.</span><span class="sxs-lookup"><span data-stu-id="7313b-143">The configuration action is called.</span></span>

<span data-ttu-id="7313b-144">Pour créer un client nommé, transmettez son nom à `CreateClient`:</span><span class="sxs-lookup"><span data-stu-id="7313b-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="7313b-145">Dans le code précédent, la requête n’a pas besoin de spécifier un nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="7313b-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="7313b-146">Le code peut passer uniquement le chemin d’accès, dans la mesure où l’adresse de base configurée pour le client est utilisée.</span><span class="sxs-lookup"><span data-stu-id="7313b-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="7313b-147">Clients typés</span><span class="sxs-lookup"><span data-stu-id="7313b-147">Typed clients</span></span>

<span data-ttu-id="7313b-148">Clients typés :</span><span class="sxs-lookup"><span data-stu-id="7313b-148">Typed clients:</span></span>

* <span data-ttu-id="7313b-149">Fournissent les mêmes fonctionnalités que les clients nommés, sans qu’il soit nécessaire d’utiliser des chaînes comme clés.</span><span class="sxs-lookup"><span data-stu-id="7313b-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="7313b-150">Bénéficie de l’aide d’IntelliSense et du compilateur lors de l’utilisation des clients.</span><span class="sxs-lookup"><span data-stu-id="7313b-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="7313b-151">Fournissent un emplacement unique pour configurer et interagir avec un `HttpClient` particulier.</span><span class="sxs-lookup"><span data-stu-id="7313b-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="7313b-152">Par exemple, un seul client typé peut être utilisé :</span><span class="sxs-lookup"><span data-stu-id="7313b-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="7313b-153">Pour un seul point de terminaison backend.</span><span class="sxs-lookup"><span data-stu-id="7313b-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="7313b-154">Pour encapsuler toute la logique traitant le point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="7313b-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="7313b-155">Utilisez DI et pouvez les injecter lorsque cela est requis dans l’application.</span><span class="sxs-lookup"><span data-stu-id="7313b-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="7313b-156">Un client typé accepte un paramètre `HttpClient` dans son constructeur :</span><span class="sxs-lookup"><span data-stu-id="7313b-156">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="7313b-157">Dans le code précédent :</span><span class="sxs-lookup"><span data-stu-id="7313b-157">In the preceding code:</span></span>

* <span data-ttu-id="7313b-158">La configuration est déplacée vers le client typé.</span><span class="sxs-lookup"><span data-stu-id="7313b-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="7313b-159">L’objet `HttpClient` est exposé en tant que propriété publique.</span><span class="sxs-lookup"><span data-stu-id="7313b-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="7313b-160">Des méthodes spécifiques à l’API peuvent être créées pour exposer les fonctionnalités de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="7313b-161">Par exemple, la méthode `GetAspNetDocsIssues` encapsule du code pour récupérer des problèmes ouverts.</span><span class="sxs-lookup"><span data-stu-id="7313b-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="7313b-162">Le code suivant appelle <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> dans `Startup.ConfigureServices` pour inscrire une classe de client typée :</span><span class="sxs-lookup"><span data-stu-id="7313b-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="7313b-163">Le client typé est inscrit comme étant transitoire avec injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="7313b-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="7313b-164">Le client typé peut être injecté et utilisé directement :</span><span class="sxs-lookup"><span data-stu-id="7313b-164">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="7313b-165">La configuration d’un client typé peut être spécifiée lors de l’inscription dans `Startup.ConfigureServices`, plutôt que dans le constructeur du client typé :</span><span class="sxs-lookup"><span data-stu-id="7313b-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="7313b-166">Le `HttpClient` peut être encapsulé dans un client typé.</span><span class="sxs-lookup"><span data-stu-id="7313b-166">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="7313b-167">Au lieu de l’exposer en tant que propriété, définissez une méthode qui appelle l’instance de `HttpClient` en interne :</span><span class="sxs-lookup"><span data-stu-id="7313b-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="7313b-168">Dans le code précédent, le `HttpClient` est stocké dans un champ privé.</span><span class="sxs-lookup"><span data-stu-id="7313b-168">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="7313b-169">L’accès à l' `HttpClient` est par la méthode `GetRepos` publique.</span><span class="sxs-lookup"><span data-stu-id="7313b-169">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="7313b-170">Clients générés</span><span class="sxs-lookup"><span data-stu-id="7313b-170">Generated clients</span></span>

<span data-ttu-id="7313b-171">les `IHttpClientFactory` peuvent être utilisées en association avec des bibliothèques tierces, [telles que la](https://github.com/paulcbetts/refit)réutilisation.</span><span class="sxs-lookup"><span data-stu-id="7313b-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="7313b-172">Refit est une bibliothèque REST pour .NET.</span><span class="sxs-lookup"><span data-stu-id="7313b-172">Refit is a REST library for .NET.</span></span> <span data-ttu-id="7313b-173">Il convertit les API REST en interfaces dynamiques.</span><span class="sxs-lookup"><span data-stu-id="7313b-173">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="7313b-174">Une implémentation de l’interface est générée dynamiquement par le `RestService`, avec `HttpClient` pour faire les appels HTTP externes.</span><span class="sxs-lookup"><span data-stu-id="7313b-174">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="7313b-175">Une interface et une réponse sont définies pour représenter l’API externe et sa réponse :</span><span class="sxs-lookup"><span data-stu-id="7313b-175">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="7313b-176">Vous pouvez ajouter un client typé en utilisant Refit pour générer l’implémentation :</span><span class="sxs-lookup"><span data-stu-id="7313b-176">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="7313b-177">L’interface définie peut être utilisée quand c’est nécessaire, avec l’implémentation fournie par l’injection de dépendances et Refit :</span><span class="sxs-lookup"><span data-stu-id="7313b-177">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="7313b-178">Middleware pour les requêtes sortantes</span><span class="sxs-lookup"><span data-stu-id="7313b-178">Outgoing request middleware</span></span>

<span data-ttu-id="7313b-179">`HttpClient` présente le concept de délégation de gestionnaires qui peuvent être liés entre eux pour les requêtes HTTP sortantes.</span><span class="sxs-lookup"><span data-stu-id="7313b-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="7313b-180">`IHttpClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="7313b-180">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="7313b-181">Simplifie la définition des gestionnaires à appliquer pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="7313b-181">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="7313b-182">Prend en charge l’inscription et le chaînage de plusieurs gestionnaires pour générer un pipeline d’intergiciel (middleware) de demande sortante.</span><span class="sxs-lookup"><span data-stu-id="7313b-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="7313b-183">Chacun de ces gestionnaires peut effectuer un travail avant et après la requête sortante.</span><span class="sxs-lookup"><span data-stu-id="7313b-183">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="7313b-184">Ce modèle :</span><span class="sxs-lookup"><span data-stu-id="7313b-184">This pattern:</span></span>

  * <span data-ttu-id="7313b-185">Est semblable au pipeline d’intergiciel (middleware) entrant dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7313b-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="7313b-186">Fournit un mécanisme permettant de gérer les problèmes transversaux autour des requêtes HTTP, par exemple :</span><span class="sxs-lookup"><span data-stu-id="7313b-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="7313b-187">mettre en cache</span><span class="sxs-lookup"><span data-stu-id="7313b-187">caching</span></span>
    * <span data-ttu-id="7313b-188">gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="7313b-188">error handling</span></span>
    * <span data-ttu-id="7313b-189">serialization</span><span class="sxs-lookup"><span data-stu-id="7313b-189">serialization</span></span>
    * <span data-ttu-id="7313b-190">enregistrer</span><span class="sxs-lookup"><span data-stu-id="7313b-190">logging</span></span>

<span data-ttu-id="7313b-191">Pour créer un gestionnaire de délégation :</span><span class="sxs-lookup"><span data-stu-id="7313b-191">To create a delegating handler:</span></span>

* <span data-ttu-id="7313b-192">Dérivez de <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="7313b-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="7313b-193">Substituez <xref:System.Net.Http.DelegatingHandler.SendAsync*></span><span class="sxs-lookup"><span data-stu-id="7313b-193">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="7313b-194">Exécutez le code avant de passer la requête au gestionnaire suivant dans le pipeline :</span><span class="sxs-lookup"><span data-stu-id="7313b-194">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="7313b-195">Le code précédent vérifie si l’en-tête `X-API-KEY` est dans la demande.</span><span class="sxs-lookup"><span data-stu-id="7313b-195">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="7313b-196">Si `X-API-KEY` est manquant, <xref:System.Net.HttpStatusCode.BadRequest> est retourné.</span><span class="sxs-lookup"><span data-stu-id="7313b-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="7313b-197">Plusieurs gestionnaires peuvent être ajoutés à la configuration d’un `HttpClient` avec <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span><span class="sxs-lookup"><span data-stu-id="7313b-197">More than one handler can be added to the configuration for an `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="7313b-198">Dans le code précédent, le `ValidateHeaderHandler` est inscrit avec une injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="7313b-198">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="7313b-199">`IHttpClientFactory` crée une étendue DI distincte pour chaque gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="7313b-199">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="7313b-200">Les gestionnaires peuvent dépendre des services de toute portée.</span><span class="sxs-lookup"><span data-stu-id="7313b-200">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="7313b-201">Les services dont dépendent les gestionnaires sont supprimés lorsque le gestionnaire est supprimé.</span><span class="sxs-lookup"><span data-stu-id="7313b-201">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="7313b-202">Une fois inscrit, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> peut être appelé en passant en entrée le type pour le gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="7313b-202">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="7313b-203">Vous pouvez inscrire plusieurs gestionnaires dans l’ordre où ils doivent être exécutés.</span><span class="sxs-lookup"><span data-stu-id="7313b-203">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="7313b-204">Chaque gestionnaire wrappe le gestionnaire suivant jusqu’à ce que le dernier `HttpClientHandler` exécute la requête :</span><span class="sxs-lookup"><span data-stu-id="7313b-204">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="7313b-205">Utilisez l’une des approches suivantes pour partager l’état de chaque requête avec les gestionnaires de messages :</span><span class="sxs-lookup"><span data-stu-id="7313b-205">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="7313b-206">Transmettez les données dans le gestionnaire à l’aide de [HttpRequestMessage. Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span><span class="sxs-lookup"><span data-stu-id="7313b-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="7313b-207">Utilisez <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> pour accéder à la requête en cours.</span><span class="sxs-lookup"><span data-stu-id="7313b-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="7313b-208">Créez un objet de stockage <xref:System.Threading.AsyncLocal`1> personnalisé pour passer les données.</span><span class="sxs-lookup"><span data-stu-id="7313b-208">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="7313b-209">Utiliser les gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="7313b-209">Use Polly-based handlers</span></span>

<span data-ttu-id="7313b-210">`IHttpClientFactory` s’intègre à la bibliothèque tierce [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="7313b-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="7313b-211">Polly est une bibliothèque complète de gestion des erreurs transitoires et de résilience pour .NET.</span><span class="sxs-lookup"><span data-stu-id="7313b-211">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="7313b-212">Elle permet aux développeurs de formuler facilement et de façon thread-safe des stratégies, comme Retry (Nouvelle tentative), Circuit Breaker (Disjoncteur), Timeout (Délai d’attente), Bulkhead Isolation (Isolation par cloisonnement) et Fallback (Alternative de repli).</span><span class="sxs-lookup"><span data-stu-id="7313b-212">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="7313b-213">Des méthodes d’extension sont fournies pour permettre l’utilisation de stratégies Polly avec les instances configurées de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-213">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="7313b-214">Les extensions Polly prennent en charge l’ajout de gestionnaires basés sur Polly aux clients.</span><span class="sxs-lookup"><span data-stu-id="7313b-214">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="7313b-215">Polly requiert le package NuGet [Microsoft. extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) .</span><span class="sxs-lookup"><span data-stu-id="7313b-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="7313b-216">Gérer les erreurs temporaires</span><span class="sxs-lookup"><span data-stu-id="7313b-216">Handle transient faults</span></span>

<span data-ttu-id="7313b-217">Les erreurs se produisent généralement lorsque les appels HTTP externes sont temporaires.</span><span class="sxs-lookup"><span data-stu-id="7313b-217">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="7313b-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> permet de définir une stratégie pour gérer les erreurs temporaires.</span><span class="sxs-lookup"><span data-stu-id="7313b-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="7313b-219">Les stratégies configurées avec `AddTransientHttpErrorPolicy` gèrent les réponses suivantes :</span><span class="sxs-lookup"><span data-stu-id="7313b-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="7313b-220">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="7313b-220">HTTP 5xx</span></span>
* <span data-ttu-id="7313b-221">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="7313b-221">HTTP 408</span></span>

<span data-ttu-id="7313b-222">`AddTransientHttpErrorPolicy` permet d’accéder à un objet `PolicyBuilder` configuré pour gérer les erreurs qui représentent une erreur temporaire possible :</span><span class="sxs-lookup"><span data-stu-id="7313b-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="7313b-223">Dans le code précédent, une stratégie `WaitAndRetryAsync` est définie.</span><span class="sxs-lookup"><span data-stu-id="7313b-223">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="7313b-224">Les requêtes qui ont échoué sont retentées jusqu’à trois fois avec un délai de 600 ms entre les tentatives.</span><span class="sxs-lookup"><span data-stu-id="7313b-224">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="7313b-225">Sélectionner dynamiquement des stratégies</span><span class="sxs-lookup"><span data-stu-id="7313b-225">Dynamically select policies</span></span>

<span data-ttu-id="7313b-226">Les méthodes d’extension sont fournies pour ajouter des gestionnaires basés sur Polly, par exemple, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span><span class="sxs-lookup"><span data-stu-id="7313b-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="7313b-227">La surcharge de `AddPolicyHandler` suivante inspecte la demande pour décider de la stratégie à appliquer :</span><span class="sxs-lookup"><span data-stu-id="7313b-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="7313b-228">Dans le code précédent, si la requête sortante est un HTTP GET, un délai d’attente de 10 secondes est appliqué.</span><span class="sxs-lookup"><span data-stu-id="7313b-228">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="7313b-229">Pour toutes les autres méthodes HTTP, un délai d’attente de 30 secondes est utilisé.</span><span class="sxs-lookup"><span data-stu-id="7313b-229">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="7313b-230">Ajouter plusieurs gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="7313b-230">Add multiple Polly handlers</span></span>

<span data-ttu-id="7313b-231">Il est courant d’imbriquer les stratégies Polly :</span><span class="sxs-lookup"><span data-stu-id="7313b-231">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="7313b-232">Dans l'exemple précédent :</span><span class="sxs-lookup"><span data-stu-id="7313b-232">In the preceding example:</span></span>

* <span data-ttu-id="7313b-233">Deux gestionnaires sont ajoutés.</span><span class="sxs-lookup"><span data-stu-id="7313b-233">Two handlers are added.</span></span>
* <span data-ttu-id="7313b-234">Le premier gestionnaire utilise <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> pour ajouter une stratégie de nouvelle tentative.</span><span class="sxs-lookup"><span data-stu-id="7313b-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="7313b-235">Les requêtes qui ont échoué sont retentées jusqu’à trois fois.</span><span class="sxs-lookup"><span data-stu-id="7313b-235">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="7313b-236">Le deuxième `AddTransientHttpErrorPolicy` appel ajoute une stratégie de disjoncteur.</span><span class="sxs-lookup"><span data-stu-id="7313b-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="7313b-237">Les demandes externes supplémentaires sont bloquées pendant 30 secondes si 5 tentatives ayant échoué se produisent de façon séquentielle.</span><span class="sxs-lookup"><span data-stu-id="7313b-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="7313b-238">Les stratégies de disjoncteur sont avec état.</span><span class="sxs-lookup"><span data-stu-id="7313b-238">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="7313b-239">Tous les appels effectués via ce client partagent le même état du circuit.</span><span class="sxs-lookup"><span data-stu-id="7313b-239">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="7313b-240">Ajouter des stratégies à partir du Registre Polly</span><span class="sxs-lookup"><span data-stu-id="7313b-240">Add policies from the Polly registry</span></span>

<span data-ttu-id="7313b-241">Une approche de la gestion des stratégies régulièrement utilisées consiste à les définir une seule fois et à les inscrire avec un `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="7313b-241">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="7313b-242">Dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="7313b-242">In the following code:</span></span>

* <span data-ttu-id="7313b-243">Les stratégies « régulières » et « longues » sont ajoutées.</span><span class="sxs-lookup"><span data-stu-id="7313b-243">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="7313b-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*> ajoute les stratégies « régulières » et « longues » à partir du Registre.</span><span class="sxs-lookup"><span data-stu-id="7313b-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="7313b-245">Pour plus d’informations sur les intégrations de `IHttpClientFactory` et Polly, consultez le [wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="7313b-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="7313b-246">HttpClient et gestion de la durée de vie</span><span class="sxs-lookup"><span data-stu-id="7313b-246">HttpClient and lifetime management</span></span>

<span data-ttu-id="7313b-247">Une nouvelle instance `HttpClient` est retournée à chaque fois que `CreateClient` est appelé sur `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="7313b-247">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="7313b-248">Un <xref:System.Net.Http.HttpMessageHandler> est créé par client nommé.</span><span class="sxs-lookup"><span data-stu-id="7313b-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="7313b-249">La fabrique gère les durées de vie des instances `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="7313b-249">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="7313b-250">`IHttpClientFactory` regroupe dans un pool les instances de `HttpMessageHandler` créées par la fabrique pour réduire la consommation des ressources.</span><span class="sxs-lookup"><span data-stu-id="7313b-250">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="7313b-251">Une instance de `HttpMessageHandler` peut être réutilisée à partir du pool quand vous créez une instance de `HttpClient` si sa durée de vie n’a pas expiré.</span><span class="sxs-lookup"><span data-stu-id="7313b-251">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="7313b-252">Le regroupement de gestionnaires en pools est souhaitable, car chaque gestionnaire gère habituellement ses propres connexions HTTP sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="7313b-252">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="7313b-253">La création de plus de gestionnaires que nécessaire peut entraîner des délais de connexion.</span><span class="sxs-lookup"><span data-stu-id="7313b-253">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="7313b-254">Certains gestionnaires maintiennent également les connexions ouvertes indéfiniment, ce qui peut empêcher le gestionnaire de réagir aux modifications DNS (Domain Name System).</span><span class="sxs-lookup"><span data-stu-id="7313b-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="7313b-255">La durée de vie par défaut d’un gestionnaire est de deux minutes.</span><span class="sxs-lookup"><span data-stu-id="7313b-255">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="7313b-256">La valeur par défaut peut être substituée pour chaque client nommé :</span><span class="sxs-lookup"><span data-stu-id="7313b-256">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="7313b-257">les instances de `HttpClient` peuvent généralement être traitées comme des objets .NET qui **ne nécessitent pas** de suppression.</span><span class="sxs-lookup"><span data-stu-id="7313b-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="7313b-258">La suppression annule les requêtes sortantes et garantit que l’instance `HttpClient` donnée ne peut pas être utilisée après avoir appelé <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="7313b-258">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="7313b-259">`IHttpClientFactory` effectue le suivi et libère les ressources utilisées par les instances `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-259">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="7313b-260">Le fait de conserver une seule instance `HttpClient` active pendant une longue durée est un modèle commun utilisé avant le lancement de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="7313b-260">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="7313b-261">Ce modèle devient inutile après la migration vers `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="7313b-261">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="7313b-262">Alternatives à IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="7313b-262">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="7313b-263">L’utilisation de `IHttpClientFactory` dans une application DI-Enabled évite les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="7313b-263">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="7313b-264">Problèmes d’épuisement des ressources par le regroupement `HttpMessageHandler` instances.</span><span class="sxs-lookup"><span data-stu-id="7313b-264">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="7313b-265">Problèmes DNS périmés par le cycle `HttpMessageHandler` instances à intervalles réguliers.</span><span class="sxs-lookup"><span data-stu-id="7313b-265">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="7313b-266">Il existe d’autres façons de résoudre les problèmes précédents à l’aide d’une instance de <xref:System.Net.Http.SocketsHttpHandler> à long terme.</span><span class="sxs-lookup"><span data-stu-id="7313b-266">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="7313b-267">Créez une instance de `SocketsHttpHandler` lorsque l’application démarre et utilisez-la pendant toute la durée de vie de l’application.</span><span class="sxs-lookup"><span data-stu-id="7313b-267">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="7313b-268">Configurez <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> à une valeur appropriée en fonction des temps d’actualisation DNS.</span><span class="sxs-lookup"><span data-stu-id="7313b-268">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="7313b-269">Créez `HttpClient` instances à l’aide d' `new HttpClient(handler, disposeHandler: false)` en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="7313b-269">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="7313b-270">Les approches précédentes résolvent les problèmes de gestion des ressources qui `IHttpClientFactory` résolus de la même façon.</span><span class="sxs-lookup"><span data-stu-id="7313b-270">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="7313b-271">Le `SocketsHttpHandler` partage les connexions entre les instances de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-271">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="7313b-272">Ce partage empêche l’épuisement des sockets.</span><span class="sxs-lookup"><span data-stu-id="7313b-272">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="7313b-273">Le `SocketsHttpHandler` cycle les connexions en fonction des `PooledConnectionLifetime` afin d’éviter les problèmes DNS périmés.</span><span class="sxs-lookup"><span data-stu-id="7313b-273">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="7313b-274">Cookies</span><span class="sxs-lookup"><span data-stu-id="7313b-274">Cookies</span></span>

<span data-ttu-id="7313b-275">Les instances de `HttpMessageHandler` regroupées entraînent le partage de `CookieContainer` objets.</span><span class="sxs-lookup"><span data-stu-id="7313b-275">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="7313b-276">Le partage d’objets `CookieContainer` imprévus aboutit souvent à un code incorrect.</span><span class="sxs-lookup"><span data-stu-id="7313b-276">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="7313b-277">Pour les applications qui nécessitent des cookies, utilisez l’une des deux opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="7313b-277">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="7313b-278">Désactivation de la gestion automatique des cookies</span><span class="sxs-lookup"><span data-stu-id="7313b-278">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="7313b-279">Éviter les `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="7313b-279">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="7313b-280">Appelez <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> pour désactiver la gestion automatique des cookies :</span><span class="sxs-lookup"><span data-stu-id="7313b-280">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="7313b-281">Journalisation</span><span class="sxs-lookup"><span data-stu-id="7313b-281">Logging</span></span>

<span data-ttu-id="7313b-282">Les clients créés via `IHttpClientFactory` enregistrent les messages de journalisation pour toutes les requêtes.</span><span class="sxs-lookup"><span data-stu-id="7313b-282">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="7313b-283">Activez le niveau d’information approprié dans la configuration de la journalisation pour voir les messages du journal par défaut.</span><span class="sxs-lookup"><span data-stu-id="7313b-283">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="7313b-284">Une journalisation supplémentaire, comme celle des en-têtes des requêtes, est incluse seulement au niveau de trace.</span><span class="sxs-lookup"><span data-stu-id="7313b-284">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="7313b-285">La catégorie de journal utilisée pour chaque client comprend le nom du client.</span><span class="sxs-lookup"><span data-stu-id="7313b-285">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="7313b-286">Un client nommé *MyNamedClient*, par exemple, journalise des messages avec la catégorie « System .net. http. httpclient ». **MyNamedClient**. LogicalHandler".</span><span class="sxs-lookup"><span data-stu-id="7313b-286">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="7313b-287">Les messages avec le suffixe *LogicalHandler* se produisent en dehors du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="7313b-287">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="7313b-288">Lors d’une requête, les messages sont journalisés avant que d’autres gestionnaires du pipeline l’aient traitée.</span><span class="sxs-lookup"><span data-stu-id="7313b-288">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="7313b-289">Lors d’une réponse, les messages sont journalisés une fois que tous les autres gestionnaires du pipeline ont reçu la réponse.</span><span class="sxs-lookup"><span data-stu-id="7313b-289">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="7313b-290">La journalisation se produit également à l’intérieur du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="7313b-290">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="7313b-291">Dans l’exemple *MyNamedClient* , ces messages sont journalisés avec la catégorie de journal « System .net. http. httpclient ». **MyNamedClient**. ClientHandler".</span><span class="sxs-lookup"><span data-stu-id="7313b-291">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="7313b-292">Pour la requête, cela se produit une fois que tous les autres gestionnaires ont été exécutés et immédiatement avant l’envoi de la demande.</span><span class="sxs-lookup"><span data-stu-id="7313b-292">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="7313b-293">Lors de la réponse, cette journalisation inclut l’état de la réponse avant qu’elle repasse à travers le pipeline de gestionnaires.</span><span class="sxs-lookup"><span data-stu-id="7313b-293">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="7313b-294">L’activation de la journalisation à l’extérieur et à l’intérieur du pipeline permet l’inspection des changements apportés par les autres gestionnaires du pipeline.</span><span class="sxs-lookup"><span data-stu-id="7313b-294">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="7313b-295">Cela peut inclure des modifications apportées aux en-têtes de demande ou au code d’état de réponse.</span><span class="sxs-lookup"><span data-stu-id="7313b-295">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="7313b-296">L’inclusion du nom du client dans la catégorie journal active le filtrage des journaux pour des clients nommés spécifiques.</span><span class="sxs-lookup"><span data-stu-id="7313b-296">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="7313b-297">Configurer le HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="7313b-297">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="7313b-298">Il peut être nécessaire de contrôler la configuration du `HttpMessageHandler` interne utilisé par un client.</span><span class="sxs-lookup"><span data-stu-id="7313b-298">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="7313b-299">Un `IHttpClientBuilder` est retourné quand vous ajoutez des clients nommés ou typés.</span><span class="sxs-lookup"><span data-stu-id="7313b-299">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="7313b-300">La méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> peut être utilisée pour définir un délégué.</span><span class="sxs-lookup"><span data-stu-id="7313b-300">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="7313b-301">Le délégué est utilisé pour créer et configurer le `HttpMessageHandler` principal utilisé par ce client :</span><span class="sxs-lookup"><span data-stu-id="7313b-301">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="7313b-302">Utiliser IHttpClientFactory dans une application console</span><span class="sxs-lookup"><span data-stu-id="7313b-302">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="7313b-303">Dans une application console, ajoutez les références de package suivantes au projet :</span><span class="sxs-lookup"><span data-stu-id="7313b-303">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="7313b-304">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="7313b-304">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="7313b-305">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="7313b-305">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="7313b-306">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="7313b-306">In the following example:</span></span>

* <span data-ttu-id="7313b-307"><xref:System.Net.Http.IHttpClientFactory> est inscrit dans le conteneur de service dans de l’[hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="7313b-307"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="7313b-308">`MyService` crée une instance de fabrique cliente à partir du service, qui est utilisée pour créer un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-308">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="7313b-309">`HttpClient` est utilisé pour récupérer une page web.</span><span class="sxs-lookup"><span data-stu-id="7313b-309">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="7313b-310">`Main` crée une étendue pour exécuter la méthode `GetPage` du service et écrire les 500 premiers caractères du contenu de la page web dans la console.</span><span class="sxs-lookup"><span data-stu-id="7313b-310">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="header-propagation-middleware"></a><span data-ttu-id="7313b-311">Intergiciel (middleware) de propagation d’en-tête</span><span class="sxs-lookup"><span data-stu-id="7313b-311">Header propagation middleware</span></span>

<span data-ttu-id="7313b-312">La propagation d’en-tête est un intergiciel (middleware) ASP.NET Core pour propager les en-têtes HTTP de la requête entrante vers les demandes du client HTTP sortantes.</span><span class="sxs-lookup"><span data-stu-id="7313b-312">Header propagation is an ASP.NET Core middleware to propagate HTTP headers from the incoming request to the outgoing HTTP Client requests.</span></span> <span data-ttu-id="7313b-313">Pour utiliser la propagation d’en-tête :</span><span class="sxs-lookup"><span data-stu-id="7313b-313">To use header propagation:</span></span>

* <span data-ttu-id="7313b-314">Référencez le package [Microsoft. AspNetCore. HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation) .</span><span class="sxs-lookup"><span data-stu-id="7313b-314">Reference the [Microsoft.AspNetCore.HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation) package.</span></span>
* <span data-ttu-id="7313b-315">Configurez les intergiciels et les `HttpClient` dans `Startup`:</span><span class="sxs-lookup"><span data-stu-id="7313b-315">Configure the middleware and `HttpClient` in `Startup`:</span></span>

  [!code-csharp[](http-requests/samples/3.x/Startup.cs?highlight=5-9,21&name=snippet)]

* <span data-ttu-id="7313b-316">Le client comprend les en-têtes configurés pour les demandes sortantes :</span><span class="sxs-lookup"><span data-stu-id="7313b-316">The client includes the configured headers on outbound requests:</span></span>

  ```C#
  var client = clientFactory.CreateClient("MyForwardingClient");
  var response = client.GetAsync(...);
  ```

## <a name="additional-resources"></a><span data-ttu-id="7313b-317">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="7313b-317">Additional resources</span></span>

* [<span data-ttu-id="7313b-318">Utiliser HttpClientFactory pour implémenter des requêtes HTTP résilientes</span><span class="sxs-lookup"><span data-stu-id="7313b-318">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="7313b-319">Implémenter de nouvelles tentatives d’appel HTTP avec interruption exponentielle avec des stratégies Polly et HttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="7313b-319">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="7313b-320">Implémenter le modèle Disjoncteur</span><span class="sxs-lookup"><span data-stu-id="7313b-320">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)
* [<span data-ttu-id="7313b-321">Comment sérialiser et désérialiser JSON dans .NET</span><span class="sxs-lookup"><span data-stu-id="7313b-321">How to serialize and deserialize JSON in .NET</span></span>](/dotnet/standard/serialization/system-text-json-how-to)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="7313b-322">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) et [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="7313b-322">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="7313b-323">Une <xref:System.Net.Http.IHttpClientFactory> peut être inscrite et utilisée pour configurer et créer des instances de <xref:System.Net.Http.HttpClient> dans une application.</span><span class="sxs-lookup"><span data-stu-id="7313b-323">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="7313b-324">Elle offre les avantages suivants :</span><span class="sxs-lookup"><span data-stu-id="7313b-324">It offers the following benefits:</span></span>

* <span data-ttu-id="7313b-325">Fournit un emplacement central pour le nommage et la configuration d’instance de `HttpClient` logiques.</span><span class="sxs-lookup"><span data-stu-id="7313b-325">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="7313b-326">Par exemple, un client *github* peut être inscrit et configuré pour accéder à [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="7313b-326">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="7313b-327">Un client par défaut peut être inscrit à d’autres fins.</span><span class="sxs-lookup"><span data-stu-id="7313b-327">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="7313b-328">Codifie le concept de middleware (intergiciel) sortant via la délégation de gestionnaires dans `HttpClient` et fournit des extensions permettant au middleware Polly d’en tirer parti.</span><span class="sxs-lookup"><span data-stu-id="7313b-328">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="7313b-329">Gère le regroupement et la durée de vie des instances de `HttpClientMessageHandler` sous-jacentes pour éviter les problèmes DNS courants qui se produisent lors de la gestion manuelle des durées de vie de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-329">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="7313b-330">Ajoute une expérience de journalisation configurable (via `ILogger`) pour toutes les requêtes envoyées via des clients créés par la fabrique.</span><span class="sxs-lookup"><span data-stu-id="7313b-330">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="7313b-331">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7313b-331">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="7313b-332">Modèles de consommation</span><span class="sxs-lookup"><span data-stu-id="7313b-332">Consumption patterns</span></span>

<span data-ttu-id="7313b-333">Vous pouvez utiliser `IHttpClientFactory` dans une application de plusieurs façons :</span><span class="sxs-lookup"><span data-stu-id="7313b-333">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="7313b-334">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="7313b-334">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="7313b-335">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="7313b-335">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="7313b-336">Clients typés</span><span class="sxs-lookup"><span data-stu-id="7313b-336">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="7313b-337">Clients générés</span><span class="sxs-lookup"><span data-stu-id="7313b-337">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="7313b-338">Aucune d’entre elles n’est meilleure qu’une autre.</span><span class="sxs-lookup"><span data-stu-id="7313b-338">None of them are strictly superior to another.</span></span> <span data-ttu-id="7313b-339">La meilleure approche dépend des contraintes de l’application.</span><span class="sxs-lookup"><span data-stu-id="7313b-339">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="7313b-340">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="7313b-340">Basic usage</span></span>

<span data-ttu-id="7313b-341">Vous pouvez inscrire la `IHttpClientFactory` en appelant la méthode d’extension `AddHttpClient` sur la `IServiceCollection`, à l’intérieur la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7313b-341">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="7313b-342">Une fois inscrit, le code peut accepter un `IHttpClientFactory` partout où des services peuvent être injectés avec [une injection de dépendance](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7313b-342">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="7313b-343">La `IHttpClientFactory` peut être utilisée pour créer une instance de `HttpClient` :</span><span class="sxs-lookup"><span data-stu-id="7313b-343">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="7313b-344">L’utilisation de `IHttpClientFactory` de cette façon est un excellent moyen de refactoriser une application existante.</span><span class="sxs-lookup"><span data-stu-id="7313b-344">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="7313b-345">Elle n’a aucun impact sur la façon dont `HttpClient` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="7313b-345">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="7313b-346">Dans les endroits où les instances de `HttpClient` sont actuellement créées, remplacez ces occurrences par un appel à <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="7313b-346">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="7313b-347">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="7313b-347">Named clients</span></span>

<span data-ttu-id="7313b-348">Si une application nécessite plusieurs utilisations distinctes de `HttpClient`, chacune avec une configuration différente, une option consiste à utiliser des **clients nommés**.</span><span class="sxs-lookup"><span data-stu-id="7313b-348">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="7313b-349">La configuration d’un `HttpClient` nommé peut être spécifiée lors de l’inscription dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7313b-349">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="7313b-350">Dans le code précédent, `AddHttpClient` est appelé en fournissant le nom *github*.</span><span class="sxs-lookup"><span data-stu-id="7313b-350">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="7313b-351">Une configuration par défaut est appliquée à ce client : l’adresse de base et deux en-têtes nécessaires pour utiliser l’API GitHub.</span><span class="sxs-lookup"><span data-stu-id="7313b-351">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="7313b-352">Chaque fois que `CreateClient` est appelée, une nouvelle instance de `HttpClient` est créée et l’action de configuration est appelée.</span><span class="sxs-lookup"><span data-stu-id="7313b-352">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="7313b-353">Pour utiliser un client nommé, un paramètre de chaîne peut être passé à `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-353">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="7313b-354">Spécifiez le nom du client à créer :</span><span class="sxs-lookup"><span data-stu-id="7313b-354">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="7313b-355">Dans le code précédent, la requête n’a pas besoin de spécifier un nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="7313b-355">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="7313b-356">Elle peut simplement passer le chemin, car l’adresse de base configurée pour le client est utilisée.</span><span class="sxs-lookup"><span data-stu-id="7313b-356">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="7313b-357">Clients typés</span><span class="sxs-lookup"><span data-stu-id="7313b-357">Typed clients</span></span>

<span data-ttu-id="7313b-358">Clients typés :</span><span class="sxs-lookup"><span data-stu-id="7313b-358">Typed clients:</span></span>

* <span data-ttu-id="7313b-359">Fournissent les mêmes fonctionnalités que les clients nommés, sans qu’il soit nécessaire d’utiliser des chaînes comme clés.</span><span class="sxs-lookup"><span data-stu-id="7313b-359">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="7313b-360">Bénéficie de l’aide d’IntelliSense et du compilateur lors de l’utilisation des clients.</span><span class="sxs-lookup"><span data-stu-id="7313b-360">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="7313b-361">Fournissent un emplacement unique pour configurer et interagir avec un `HttpClient` particulier.</span><span class="sxs-lookup"><span data-stu-id="7313b-361">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="7313b-362">Par exemple, un même client typé peut être utilisé pour un point de terminaison de backend et pour encapsuler la logique relative à ce point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="7313b-362">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="7313b-363">Fonctionnent avec l’injection de dépendances et peuvent être injectés là où c’est nécessaire dans votre application.</span><span class="sxs-lookup"><span data-stu-id="7313b-363">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="7313b-364">Un client typé accepte un paramètre `HttpClient` dans son constructeur :</span><span class="sxs-lookup"><span data-stu-id="7313b-364">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="7313b-365">Dans le code précédent, la configuration est déplacée dans le client typé.</span><span class="sxs-lookup"><span data-stu-id="7313b-365">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="7313b-366">L’objet `HttpClient` est exposé en tant que propriété publique.</span><span class="sxs-lookup"><span data-stu-id="7313b-366">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="7313b-367">Il est possible de définir des méthodes d’API spécifiques qui exposent les fonctionnalités de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-367">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="7313b-368">La méthode `GetAspNetDocsIssues` encapsule le code nécessaire pour interroger et analyser les problèmes ouverts les plus récents d’un dépôt GitHub.</span><span class="sxs-lookup"><span data-stu-id="7313b-368">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="7313b-369">Pour inscrire un client typé, la méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> générique peut être utilisée dans `Startup.ConfigureServices`, en spécifiant la classe du client typé :</span><span class="sxs-lookup"><span data-stu-id="7313b-369">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="7313b-370">Le client typé est inscrit comme étant transitoire avec injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="7313b-370">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="7313b-371">Le client typé peut être injecté et utilisé directement :</span><span class="sxs-lookup"><span data-stu-id="7313b-371">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="7313b-372">Si vous préférez, vous pouvez spécifier la configuration d’un client typé lors de l’inscription dans `Startup.ConfigureServices` au lieu de le faire dans le constructeur du client typé :</span><span class="sxs-lookup"><span data-stu-id="7313b-372">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="7313b-373">Il est possible d’encapsuler entièrement le `HttpClient` dans un client typé.</span><span class="sxs-lookup"><span data-stu-id="7313b-373">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="7313b-374">Au lieu de l’exposer en tant que propriété, vous pouvez fournir des méthodes publiques qui appellent l’instance de `HttpClient` en interne.</span><span class="sxs-lookup"><span data-stu-id="7313b-374">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="7313b-375">Dans le code précédent, le `HttpClient` est stocké en tant que champ privé.</span><span class="sxs-lookup"><span data-stu-id="7313b-375">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="7313b-376">Tous les accès nécessaires pour effectuer des appels externes passent par la méthode `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="7313b-376">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="7313b-377">Clients générés</span><span class="sxs-lookup"><span data-stu-id="7313b-377">Generated clients</span></span>

<span data-ttu-id="7313b-378">`IHttpClientFactory` peut être utilisé en combinaison avec d’autres bibliothèques tierces, comme [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="7313b-378">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="7313b-379">Refit est une bibliothèque REST pour .NET.</span><span class="sxs-lookup"><span data-stu-id="7313b-379">Refit is a REST library for .NET.</span></span> <span data-ttu-id="7313b-380">Il convertit les API REST en interfaces dynamiques.</span><span class="sxs-lookup"><span data-stu-id="7313b-380">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="7313b-381">Une implémentation de l’interface est générée dynamiquement par le `RestService`, avec `HttpClient` pour faire les appels HTTP externes.</span><span class="sxs-lookup"><span data-stu-id="7313b-381">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="7313b-382">Une interface et une réponse sont définies pour représenter l’API externe et sa réponse :</span><span class="sxs-lookup"><span data-stu-id="7313b-382">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="7313b-383">Vous pouvez ajouter un client typé en utilisant Refit pour générer l’implémentation :</span><span class="sxs-lookup"><span data-stu-id="7313b-383">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="7313b-384">L’interface définie peut être utilisée quand c’est nécessaire, avec l’implémentation fournie par l’injection de dépendances et Refit :</span><span class="sxs-lookup"><span data-stu-id="7313b-384">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="7313b-385">Middleware pour les requêtes sortantes</span><span class="sxs-lookup"><span data-stu-id="7313b-385">Outgoing request middleware</span></span>

<span data-ttu-id="7313b-386">`HttpClient` intègre déjà le concept de délégation des gestionnaires qui peuvent être liés ensemble pour les requêtes HTTP sortantes.</span><span class="sxs-lookup"><span data-stu-id="7313b-386">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="7313b-387">Le `IHttpClientFactory` permet de définir facilement les gestionnaires à appliquer pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="7313b-387">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="7313b-388">Il prend en charge l’inscription et le chaînage de plusieurs gestionnaires pour créer un pipeline de middlewares pour les requêtes sortantes.</span><span class="sxs-lookup"><span data-stu-id="7313b-388">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="7313b-389">Chacun de ces gestionnaires peut effectuer un travail avant et après la requête sortante.</span><span class="sxs-lookup"><span data-stu-id="7313b-389">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="7313b-390">Ce modèle est similaire au pipeline de middlewares entrants dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7313b-390">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="7313b-391">Le modèle fournit un mécanisme permettant de gérer les problèmes transversaux liés aux des requêtes HTTP, notamment la mise en cache, la gestion des erreurs, la sérialisation et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="7313b-391">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="7313b-392">Pour créer un gestionnaire, définissez une classe dérivant de <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="7313b-392">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="7313b-393">Remplacez la méthode `SendAsync` de façon à exécuter du code avant de passer la requête au gestionnaire suivant dans le pipeline :</span><span class="sxs-lookup"><span data-stu-id="7313b-393">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="7313b-394">Le code précédent définit un gestionnaire de base.</span><span class="sxs-lookup"><span data-stu-id="7313b-394">The preceding code defines a basic handler.</span></span> <span data-ttu-id="7313b-395">Il vérifie si un en-tête `X-API-KEY` a été inclus dans la requête.</span><span class="sxs-lookup"><span data-stu-id="7313b-395">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="7313b-396">Si l’en-tête est manquant, il peut éviter l’appel HTTP et retourner une réponse appropriée.</span><span class="sxs-lookup"><span data-stu-id="7313b-396">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="7313b-397">Au cours de l’inscription, un ou plusieurs gestionnaires peuvent être ajoutés à la configuration d’un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-397">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="7313b-398">Cette tâche est accomplie via des méthodes d’extension sur le <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="7313b-398">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="7313b-399">Dans le code précédent, le `ValidateHeaderHandler` est inscrit avec une injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="7313b-399">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="7313b-400">`IHttpClientFactory` crée une étendue DI distincte pour chaque gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="7313b-400">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="7313b-401">Les gestionnaires peuvent librement dépendre des services de n’importe quelle étendue.</span><span class="sxs-lookup"><span data-stu-id="7313b-401">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="7313b-402">Les services dont dépendent les gestionnaires sont supprimés lorsque le gestionnaire est supprimé.</span><span class="sxs-lookup"><span data-stu-id="7313b-402">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="7313b-403">Une fois inscrit, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> peut être appelé en passant en entrée le type pour le gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="7313b-403">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="7313b-404">Vous pouvez inscrire plusieurs gestionnaires dans l’ordre où ils doivent être exécutés.</span><span class="sxs-lookup"><span data-stu-id="7313b-404">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="7313b-405">Chaque gestionnaire wrappe le gestionnaire suivant jusqu’à ce que le dernier `HttpClientHandler` exécute la requête :</span><span class="sxs-lookup"><span data-stu-id="7313b-405">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="7313b-406">Utilisez l’une des approches suivantes pour partager l’état de chaque requête avec les gestionnaires de messages :</span><span class="sxs-lookup"><span data-stu-id="7313b-406">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="7313b-407">Passez des données dans le gestionnaire en utilisant `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="7313b-407">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="7313b-408">Utilisez `IHttpContextAccessor` pour accéder à la requête en cours.</span><span class="sxs-lookup"><span data-stu-id="7313b-408">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="7313b-409">Créez un objet de stockage `AsyncLocal` personnalisé pour passer les données.</span><span class="sxs-lookup"><span data-stu-id="7313b-409">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="7313b-410">Utiliser les gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="7313b-410">Use Polly-based handlers</span></span>

<span data-ttu-id="7313b-411">`IHttpClientFactory` s’intègre à une bibliothèque tierce très utilisée, appelée [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="7313b-411">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="7313b-412">Polly est une bibliothèque complète de gestion des erreurs transitoires et de résilience pour .NET.</span><span class="sxs-lookup"><span data-stu-id="7313b-412">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="7313b-413">Elle permet aux développeurs de formuler facilement et de façon thread-safe des stratégies, comme Retry (Nouvelle tentative), Circuit Breaker (Disjoncteur), Timeout (Délai d’attente), Bulkhead Isolation (Isolation par cloisonnement) et Fallback (Alternative de repli).</span><span class="sxs-lookup"><span data-stu-id="7313b-413">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="7313b-414">Des méthodes d’extension sont fournies pour permettre l’utilisation de stratégies Polly avec les instances configurées de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-414">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="7313b-415">Les extensions Polly :</span><span class="sxs-lookup"><span data-stu-id="7313b-415">The Polly extensions:</span></span>

* <span data-ttu-id="7313b-416">Prennent en charge l’ajout de gestionnaires Polly à des clients.</span><span class="sxs-lookup"><span data-stu-id="7313b-416">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="7313b-417">Peuvent être utilisées après l’installation du package NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="7313b-417">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="7313b-418">Ce package n’est pas inclus dans le framework partagé ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7313b-418">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="7313b-419">Gérer les erreurs temporaires</span><span class="sxs-lookup"><span data-stu-id="7313b-419">Handle transient faults</span></span>

<span data-ttu-id="7313b-420">Les erreurs courantes se produisent lorsque des appels HTTP externes sont temporaires.</span><span class="sxs-lookup"><span data-stu-id="7313b-420">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="7313b-421">Une méthode d’extension pratique nommée `AddTransientHttpErrorPolicy` est incluse : elle permet de définir une stratégie pour gérer les erreurs temporaires.</span><span class="sxs-lookup"><span data-stu-id="7313b-421">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="7313b-422">Les stratégies configurées avec cette méthode d’extension gèrent `HttpRequestException`, les réponses HTTP 5xx et les réponses HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="7313b-422">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="7313b-423">L’extension `AddTransientHttpErrorPolicy` peut être utilisée dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7313b-423">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="7313b-424">L’extension fournit l’accès à un objet `PolicyBuilder` configuré pour gérer les erreurs représentant une erreur temporaire possible :</span><span class="sxs-lookup"><span data-stu-id="7313b-424">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="7313b-425">Dans le code précédent, une stratégie `WaitAndRetryAsync` est définie.</span><span class="sxs-lookup"><span data-stu-id="7313b-425">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="7313b-426">Les requêtes qui ont échoué sont retentées jusqu’à trois fois avec un délai de 600 ms entre les tentatives.</span><span class="sxs-lookup"><span data-stu-id="7313b-426">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="7313b-427">Sélectionner dynamiquement des stratégies</span><span class="sxs-lookup"><span data-stu-id="7313b-427">Dynamically select policies</span></span>

<span data-ttu-id="7313b-428">Il existe d’autres méthodes d’extension que vous pouvez utiliser pour ajouter des gestionnaires Polly.</span><span class="sxs-lookup"><span data-stu-id="7313b-428">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="7313b-429">Une de ces extensions est `AddPolicyHandler`, qui a plusieurs surcharges.</span><span class="sxs-lookup"><span data-stu-id="7313b-429">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="7313b-430">Une de ces surcharges permet l’inspection de la requête lors de la définition de la stratégie à appliquer :</span><span class="sxs-lookup"><span data-stu-id="7313b-430">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="7313b-431">Dans le code précédent, si la requête sortante est un HTTP GET, un délai d’attente de 10 secondes est appliqué.</span><span class="sxs-lookup"><span data-stu-id="7313b-431">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="7313b-432">Pour toutes les autres méthodes HTTP, un délai d’attente de 30 secondes est utilisé.</span><span class="sxs-lookup"><span data-stu-id="7313b-432">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="7313b-433">Ajouter plusieurs gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="7313b-433">Add multiple Polly handlers</span></span>

<span data-ttu-id="7313b-434">Il est courant d’imbriquer des stratégies Polly pour fournir des fonctionnalités améliorées :</span><span class="sxs-lookup"><span data-stu-id="7313b-434">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="7313b-435">Dans l’exemple précédent, deux gestionnaires sont ajoutés.</span><span class="sxs-lookup"><span data-stu-id="7313b-435">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="7313b-436">Le premier utilise l’extension `AddTransientHttpErrorPolicy` pour ajouter une stratégie de nouvelle tentative.</span><span class="sxs-lookup"><span data-stu-id="7313b-436">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="7313b-437">Les requêtes qui ont échoué sont retentées jusqu’à trois fois.</span><span class="sxs-lookup"><span data-stu-id="7313b-437">Failed requests are retried up to three times.</span></span> <span data-ttu-id="7313b-438">Le deuxième appel à `AddTransientHttpErrorPolicy` ajoute une stratégie de disjoncteur.</span><span class="sxs-lookup"><span data-stu-id="7313b-438">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="7313b-439">Les requêtes externes supplémentaires sont bloquées pendant 30 secondes si cinq tentatives successives échouent.</span><span class="sxs-lookup"><span data-stu-id="7313b-439">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="7313b-440">Les stratégies de disjoncteur sont avec état.</span><span class="sxs-lookup"><span data-stu-id="7313b-440">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="7313b-441">Tous les appels effectués via ce client partagent le même état du circuit.</span><span class="sxs-lookup"><span data-stu-id="7313b-441">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="7313b-442">Ajouter des stratégies à partir du Registre Polly</span><span class="sxs-lookup"><span data-stu-id="7313b-442">Add policies from the Polly registry</span></span>

<span data-ttu-id="7313b-443">Une approche de la gestion des stratégies régulièrement utilisées consiste à les définir une seule fois et à les inscrire avec un `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="7313b-443">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="7313b-444">Il existe une méthode d’extension qui permet l’ajout d’un gestionnaire avec une stratégie du Registre :</span><span class="sxs-lookup"><span data-stu-id="7313b-444">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="7313b-445">Dans le code précédent, deux stratégies sont inscrites lorsque `PolicyRegistry` est ajouté à `ServiceCollection`.</span><span class="sxs-lookup"><span data-stu-id="7313b-445">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="7313b-446">Pour utiliser une stratégie du Registre, la méthode `AddPolicyHandlerFromRegistry` est utilisée, en passant le nom de la stratégie à appliquer.</span><span class="sxs-lookup"><span data-stu-id="7313b-446">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="7313b-447">Vous trouverez plus d’informations sur les intégrations de `IHttpClientFactory` et de Polly dans le [wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="7313b-447">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="7313b-448">HttpClient et gestion de la durée de vie</span><span class="sxs-lookup"><span data-stu-id="7313b-448">HttpClient and lifetime management</span></span>

<span data-ttu-id="7313b-449">Une nouvelle instance `HttpClient` est retournée à chaque fois que `CreateClient` est appelé sur `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="7313b-449">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="7313b-450">Il existe un <xref:System.Net.Http.HttpMessageHandler> par client nommé.</span><span class="sxs-lookup"><span data-stu-id="7313b-450">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="7313b-451">La fabrique gère les durées de vie des instances `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="7313b-451">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="7313b-452">`IHttpClientFactory` regroupe dans un pool les instances de `HttpMessageHandler` créées par la fabrique pour réduire la consommation des ressources.</span><span class="sxs-lookup"><span data-stu-id="7313b-452">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="7313b-453">Une instance de `HttpMessageHandler` peut être réutilisée à partir du pool quand vous créez une instance de `HttpClient` si sa durée de vie n’a pas expiré.</span><span class="sxs-lookup"><span data-stu-id="7313b-453">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="7313b-454">Le regroupement de gestionnaires en pools est souhaitable, car chaque gestionnaire gère habituellement ses propres connexions HTTP sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="7313b-454">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="7313b-455">La création de plus de gestionnaires que nécessaire peut entraîner des délais de connexion.</span><span class="sxs-lookup"><span data-stu-id="7313b-455">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="7313b-456">Certains gestionnaires conservent aussi les connexions indéfiniment ouvertes, ce qui peut empêcher le gestionnaire de réagir aux changements du DNS.</span><span class="sxs-lookup"><span data-stu-id="7313b-456">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="7313b-457">La durée de vie par défaut d’un gestionnaire est de deux minutes.</span><span class="sxs-lookup"><span data-stu-id="7313b-457">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="7313b-458">La valeur par défaut peut être remplacée pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="7313b-458">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="7313b-459">Pour la remplacer, appelez <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> sur le `IHttpClientBuilder` qui est retourné lors de la création du client :</span><span class="sxs-lookup"><span data-stu-id="7313b-459">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="7313b-460">La suppression du client n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="7313b-460">Disposal of the client isn't required.</span></span> <span data-ttu-id="7313b-461">La suppression annule les requêtes sortantes et garantit que l’instance `HttpClient` donnée ne peut pas être utilisée après avoir appelé <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="7313b-461">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="7313b-462">`IHttpClientFactory` effectue le suivi et libère les ressources utilisées par les instances `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-462">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="7313b-463">Les instances `HttpClient` peuvent généralement être traitées en tant qu’objets .NET ne nécessitant pas une suppression.</span><span class="sxs-lookup"><span data-stu-id="7313b-463">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="7313b-464">Le fait de conserver une seule instance `HttpClient` active pendant une longue durée est un modèle commun utilisé avant le lancement de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="7313b-464">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="7313b-465">Ce modèle devient inutile après la migration vers `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="7313b-465">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="7313b-466">Alternatives à IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="7313b-466">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="7313b-467">L’utilisation de `IHttpClientFactory` dans une application DI-Enabled évite les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="7313b-467">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="7313b-468">Problèmes d’épuisement des ressources par le regroupement `HttpMessageHandler` instances.</span><span class="sxs-lookup"><span data-stu-id="7313b-468">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="7313b-469">Problèmes DNS périmés par le cycle `HttpMessageHandler` instances à intervalles réguliers.</span><span class="sxs-lookup"><span data-stu-id="7313b-469">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="7313b-470">Il existe d’autres façons de résoudre les problèmes précédents à l’aide d’une instance de <xref:System.Net.Http.SocketsHttpHandler> à long terme.</span><span class="sxs-lookup"><span data-stu-id="7313b-470">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="7313b-471">Créez une instance de `SocketsHttpHandler` lorsque l’application démarre et utilisez-la pendant toute la durée de vie de l’application.</span><span class="sxs-lookup"><span data-stu-id="7313b-471">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="7313b-472">Configurez <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> à une valeur appropriée en fonction des temps d’actualisation DNS.</span><span class="sxs-lookup"><span data-stu-id="7313b-472">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="7313b-473">Créez `HttpClient` instances à l’aide d' `new HttpClient(handler, disposeHandler: false)` en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="7313b-473">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="7313b-474">Les approches précédentes résolvent les problèmes de gestion des ressources qui `IHttpClientFactory` résolus de la même façon.</span><span class="sxs-lookup"><span data-stu-id="7313b-474">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="7313b-475">Le `SocketsHttpHandler` partage les connexions entre les instances de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-475">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="7313b-476">Ce partage empêche l’épuisement des sockets.</span><span class="sxs-lookup"><span data-stu-id="7313b-476">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="7313b-477">Le `SocketsHttpHandler` cycle les connexions en fonction des `PooledConnectionLifetime` afin d’éviter les problèmes DNS périmés.</span><span class="sxs-lookup"><span data-stu-id="7313b-477">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="7313b-478">Cookies</span><span class="sxs-lookup"><span data-stu-id="7313b-478">Cookies</span></span>

<span data-ttu-id="7313b-479">Les instances de `HttpMessageHandler` regroupées entraînent le partage de `CookieContainer` objets.</span><span class="sxs-lookup"><span data-stu-id="7313b-479">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="7313b-480">Le partage d’objets `CookieContainer` imprévus aboutit souvent à un code incorrect.</span><span class="sxs-lookup"><span data-stu-id="7313b-480">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="7313b-481">Pour les applications qui nécessitent des cookies, utilisez l’une des deux opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="7313b-481">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="7313b-482">Désactivation de la gestion automatique des cookies</span><span class="sxs-lookup"><span data-stu-id="7313b-482">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="7313b-483">Éviter les `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="7313b-483">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="7313b-484">Appelez <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> pour désactiver la gestion automatique des cookies :</span><span class="sxs-lookup"><span data-stu-id="7313b-484">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="7313b-485">Journalisation</span><span class="sxs-lookup"><span data-stu-id="7313b-485">Logging</span></span>

<span data-ttu-id="7313b-486">Les clients créés via `IHttpClientFactory` enregistrent les messages de journalisation pour toutes les requêtes.</span><span class="sxs-lookup"><span data-stu-id="7313b-486">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="7313b-487">Activez le niveau d’informations approprié dans votre configuration de journalisation pour voir les messages de journalisation par défaut.</span><span class="sxs-lookup"><span data-stu-id="7313b-487">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="7313b-488">Une journalisation supplémentaire, comme celle des en-têtes des requêtes, est incluse seulement au niveau de trace.</span><span class="sxs-lookup"><span data-stu-id="7313b-488">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="7313b-489">La catégorie de journal utilisée pour chaque client comprend le nom du client.</span><span class="sxs-lookup"><span data-stu-id="7313b-489">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="7313b-490">Par exemple, un client nommé *MyNamedClient* journalise les messages avec la catégorie `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="7313b-490">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="7313b-491">Les messages avec le suffixe *LogicalHandler* se produisent en dehors du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="7313b-491">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="7313b-492">Lors d’une requête, les messages sont journalisés avant que d’autres gestionnaires du pipeline l’aient traitée.</span><span class="sxs-lookup"><span data-stu-id="7313b-492">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="7313b-493">Lors d’une réponse, les messages sont journalisés une fois que tous les autres gestionnaires du pipeline ont reçu la réponse.</span><span class="sxs-lookup"><span data-stu-id="7313b-493">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="7313b-494">La journalisation se produit également à l’intérieur du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="7313b-494">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="7313b-495">Dans l’exemple *MyNamedClient*, ces messages sont journalisés avec la catégorie de journalisation `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="7313b-495">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="7313b-496">Pour la requête, cela se produit après que tous les autres gestionnaires ont été exécutés et immédiatement avant l’envoi de la requête sur le réseau.</span><span class="sxs-lookup"><span data-stu-id="7313b-496">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="7313b-497">Lors de la réponse, cette journalisation inclut l’état de la réponse avant qu’elle repasse à travers le pipeline de gestionnaires.</span><span class="sxs-lookup"><span data-stu-id="7313b-497">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="7313b-498">L’activation de la journalisation à l’extérieur et à l’intérieur du pipeline permet l’inspection des changements apportés par les autres gestionnaires du pipeline.</span><span class="sxs-lookup"><span data-stu-id="7313b-498">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="7313b-499">Par exemple, cela peut comprendre des changements apportés aux en-têtes des requêtes ou au code d’état de la réponse.</span><span class="sxs-lookup"><span data-stu-id="7313b-499">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="7313b-500">L’ajout du nom du client dans la catégorie de journalisation permet si nécessaire de filtrer le journal pour des clients nommés spécifiques.</span><span class="sxs-lookup"><span data-stu-id="7313b-500">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="7313b-501">Configurer le HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="7313b-501">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="7313b-502">Il peut être nécessaire de contrôler la configuration du `HttpMessageHandler` interne utilisé par un client.</span><span class="sxs-lookup"><span data-stu-id="7313b-502">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="7313b-503">Un `IHttpClientBuilder` est retourné quand vous ajoutez des clients nommés ou typés.</span><span class="sxs-lookup"><span data-stu-id="7313b-503">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="7313b-504">La méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> peut être utilisée pour définir un délégué.</span><span class="sxs-lookup"><span data-stu-id="7313b-504">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="7313b-505">Le délégué est utilisé pour créer et configurer le `HttpMessageHandler` principal utilisé par ce client :</span><span class="sxs-lookup"><span data-stu-id="7313b-505">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="7313b-506">Utiliser IHttpClientFactory dans une application console</span><span class="sxs-lookup"><span data-stu-id="7313b-506">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="7313b-507">Dans une application console, ajoutez les références de package suivantes au projet :</span><span class="sxs-lookup"><span data-stu-id="7313b-507">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="7313b-508">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="7313b-508">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="7313b-509">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="7313b-509">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="7313b-510">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="7313b-510">In the following example:</span></span>

* <span data-ttu-id="7313b-511"><xref:System.Net.Http.IHttpClientFactory> est inscrit dans le conteneur de service dans de l’[hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="7313b-511"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="7313b-512">`MyService` crée une instance de fabrique cliente à partir du service, qui est utilisée pour créer un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-512">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="7313b-513">`HttpClient` est utilisé pour récupérer une page web.</span><span class="sxs-lookup"><span data-stu-id="7313b-513">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="7313b-514">`Main` crée une étendue pour exécuter la méthode `GetPage` du service et écrire les 500 premiers caractères du contenu de la page web dans la console.</span><span class="sxs-lookup"><span data-stu-id="7313b-514">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="7313b-515">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="7313b-515">Additional resources</span></span>

* [<span data-ttu-id="7313b-516">Utiliser HttpClientFactory pour implémenter des requêtes HTTP résilientes</span><span class="sxs-lookup"><span data-stu-id="7313b-516">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="7313b-517">Implémenter de nouvelles tentatives d’appel HTTP avec interruption exponentielle avec des stratégies Polly et HttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="7313b-517">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="7313b-518">Implémenter le modèle Disjoncteur</span><span class="sxs-lookup"><span data-stu-id="7313b-518">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="7313b-519">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) et [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="7313b-519">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="7313b-520">Une <xref:System.Net.Http.IHttpClientFactory> peut être inscrite et utilisée pour configurer et créer des instances de <xref:System.Net.Http.HttpClient> dans une application.</span><span class="sxs-lookup"><span data-stu-id="7313b-520">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="7313b-521">Elle offre les avantages suivants :</span><span class="sxs-lookup"><span data-stu-id="7313b-521">It offers the following benefits:</span></span>

* <span data-ttu-id="7313b-522">Fournit un emplacement central pour le nommage et la configuration d’instance de `HttpClient` logiques.</span><span class="sxs-lookup"><span data-stu-id="7313b-522">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="7313b-523">Par exemple, un client *github* peut être inscrit et configuré pour accéder à [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="7313b-523">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="7313b-524">Un client par défaut peut être inscrit à d’autres fins.</span><span class="sxs-lookup"><span data-stu-id="7313b-524">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="7313b-525">Codifie le concept de middleware (intergiciel) sortant via la délégation de gestionnaires dans `HttpClient` et fournit des extensions permettant au middleware Polly d’en tirer parti.</span><span class="sxs-lookup"><span data-stu-id="7313b-525">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="7313b-526">Gère le regroupement et la durée de vie des instances de `HttpClientMessageHandler` sous-jacentes pour éviter les problèmes DNS courants qui se produisent lors de la gestion manuelle des durées de vie de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-526">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="7313b-527">Ajoute une expérience de journalisation configurable (via `ILogger`) pour toutes les requêtes envoyées via des clients créés par la fabrique.</span><span class="sxs-lookup"><span data-stu-id="7313b-527">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="7313b-528">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7313b-528">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7313b-529">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="7313b-529">Prerequisites</span></span>

<span data-ttu-id="7313b-530">Les projets ciblant .NET Framework nécessitent l’installation du package NuGet [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/).</span><span class="sxs-lookup"><span data-stu-id="7313b-530">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="7313b-531">Les projets qui ciblent .NET Core et référencent le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) incluent déjà le package `Microsoft.Extensions.Http`.</span><span class="sxs-lookup"><span data-stu-id="7313b-531">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="7313b-532">Modèles de consommation</span><span class="sxs-lookup"><span data-stu-id="7313b-532">Consumption patterns</span></span>

<span data-ttu-id="7313b-533">Vous pouvez utiliser `IHttpClientFactory` dans une application de plusieurs façons :</span><span class="sxs-lookup"><span data-stu-id="7313b-533">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="7313b-534">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="7313b-534">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="7313b-535">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="7313b-535">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="7313b-536">Clients typés</span><span class="sxs-lookup"><span data-stu-id="7313b-536">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="7313b-537">Clients générés</span><span class="sxs-lookup"><span data-stu-id="7313b-537">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="7313b-538">Aucune d’entre elles n’est meilleure qu’une autre.</span><span class="sxs-lookup"><span data-stu-id="7313b-538">None of them are strictly superior to another.</span></span> <span data-ttu-id="7313b-539">La meilleure approche dépend des contraintes de l’application.</span><span class="sxs-lookup"><span data-stu-id="7313b-539">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="7313b-540">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="7313b-540">Basic usage</span></span>

<span data-ttu-id="7313b-541">Vous pouvez inscrire la `IHttpClientFactory` en appelant la méthode d’extension `AddHttpClient` sur la `IServiceCollection`, à l’intérieur la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7313b-541">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="7313b-542">Une fois inscrit, le code peut accepter un `IHttpClientFactory` partout où des services peuvent être injectés avec [une injection de dépendance](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7313b-542">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="7313b-543">La `IHttpClientFactory` peut être utilisée pour créer une instance de `HttpClient` :</span><span class="sxs-lookup"><span data-stu-id="7313b-543">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="7313b-544">L’utilisation de `IHttpClientFactory` de cette façon est un excellent moyen de refactoriser une application existante.</span><span class="sxs-lookup"><span data-stu-id="7313b-544">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="7313b-545">Elle n’a aucun impact sur la façon dont `HttpClient` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="7313b-545">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="7313b-546">Dans les endroits où les instances de `HttpClient` sont actuellement créées, remplacez ces occurrences par un appel à <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="7313b-546">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="7313b-547">Clients nommés</span><span class="sxs-lookup"><span data-stu-id="7313b-547">Named clients</span></span>

<span data-ttu-id="7313b-548">Si une application nécessite plusieurs utilisations distinctes de `HttpClient`, chacune avec une configuration différente, une option consiste à utiliser des **clients nommés**.</span><span class="sxs-lookup"><span data-stu-id="7313b-548">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="7313b-549">La configuration d’un `HttpClient` nommé peut être spécifiée lors de l’inscription dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7313b-549">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="7313b-550">Dans le code précédent, `AddHttpClient` est appelé en fournissant le nom *github*.</span><span class="sxs-lookup"><span data-stu-id="7313b-550">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="7313b-551">Une configuration par défaut est appliquée à ce client : l’adresse de base et deux en-têtes nécessaires pour utiliser l’API GitHub.</span><span class="sxs-lookup"><span data-stu-id="7313b-551">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="7313b-552">Chaque fois que `CreateClient` est appelée, une nouvelle instance de `HttpClient` est créée et l’action de configuration est appelée.</span><span class="sxs-lookup"><span data-stu-id="7313b-552">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="7313b-553">Pour utiliser un client nommé, un paramètre de chaîne peut être passé à `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-553">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="7313b-554">Spécifiez le nom du client à créer :</span><span class="sxs-lookup"><span data-stu-id="7313b-554">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="7313b-555">Dans le code précédent, la requête n’a pas besoin de spécifier un nom d’hôte.</span><span class="sxs-lookup"><span data-stu-id="7313b-555">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="7313b-556">Elle peut simplement passer le chemin, car l’adresse de base configurée pour le client est utilisée.</span><span class="sxs-lookup"><span data-stu-id="7313b-556">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="7313b-557">Clients typés</span><span class="sxs-lookup"><span data-stu-id="7313b-557">Typed clients</span></span>

<span data-ttu-id="7313b-558">Clients typés :</span><span class="sxs-lookup"><span data-stu-id="7313b-558">Typed clients:</span></span>

* <span data-ttu-id="7313b-559">Fournissent les mêmes fonctionnalités que les clients nommés, sans qu’il soit nécessaire d’utiliser des chaînes comme clés.</span><span class="sxs-lookup"><span data-stu-id="7313b-559">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="7313b-560">Bénéficie de l’aide d’IntelliSense et du compilateur lors de l’utilisation des clients.</span><span class="sxs-lookup"><span data-stu-id="7313b-560">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="7313b-561">Fournissent un emplacement unique pour configurer et interagir avec un `HttpClient` particulier.</span><span class="sxs-lookup"><span data-stu-id="7313b-561">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="7313b-562">Par exemple, un même client typé peut être utilisé pour un point de terminaison de backend et pour encapsuler la logique relative à ce point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="7313b-562">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="7313b-563">Fonctionnent avec l’injection de dépendances et peuvent être injectés là où c’est nécessaire dans votre application.</span><span class="sxs-lookup"><span data-stu-id="7313b-563">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="7313b-564">Un client typé accepte un paramètre `HttpClient` dans son constructeur :</span><span class="sxs-lookup"><span data-stu-id="7313b-564">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="7313b-565">Dans le code précédent, la configuration est déplacée dans le client typé.</span><span class="sxs-lookup"><span data-stu-id="7313b-565">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="7313b-566">L’objet `HttpClient` est exposé en tant que propriété publique.</span><span class="sxs-lookup"><span data-stu-id="7313b-566">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="7313b-567">Il est possible de définir des méthodes d’API spécifiques qui exposent les fonctionnalités de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-567">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="7313b-568">La méthode `GetAspNetDocsIssues` encapsule le code nécessaire pour interroger et analyser les problèmes ouverts les plus récents d’un dépôt GitHub.</span><span class="sxs-lookup"><span data-stu-id="7313b-568">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="7313b-569">Pour inscrire un client typé, la méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> générique peut être utilisée dans `Startup.ConfigureServices`, en spécifiant la classe du client typé :</span><span class="sxs-lookup"><span data-stu-id="7313b-569">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="7313b-570">Le client typé est inscrit comme étant transitoire avec injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="7313b-570">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="7313b-571">Le client typé peut être injecté et utilisé directement :</span><span class="sxs-lookup"><span data-stu-id="7313b-571">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="7313b-572">Si vous préférez, vous pouvez spécifier la configuration d’un client typé lors de l’inscription dans `Startup.ConfigureServices` au lieu de le faire dans le constructeur du client typé :</span><span class="sxs-lookup"><span data-stu-id="7313b-572">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="7313b-573">Il est possible d’encapsuler entièrement le `HttpClient` dans un client typé.</span><span class="sxs-lookup"><span data-stu-id="7313b-573">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="7313b-574">Au lieu de l’exposer en tant que propriété, vous pouvez fournir des méthodes publiques qui appellent l’instance de `HttpClient` en interne.</span><span class="sxs-lookup"><span data-stu-id="7313b-574">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="7313b-575">Dans le code précédent, le `HttpClient` est stocké en tant que champ privé.</span><span class="sxs-lookup"><span data-stu-id="7313b-575">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="7313b-576">Tous les accès nécessaires pour effectuer des appels externes passent par la méthode `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="7313b-576">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="7313b-577">Clients générés</span><span class="sxs-lookup"><span data-stu-id="7313b-577">Generated clients</span></span>

<span data-ttu-id="7313b-578">`IHttpClientFactory` peut être utilisé en combinaison avec d’autres bibliothèques tierces, comme [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="7313b-578">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="7313b-579">Refit est une bibliothèque REST pour .NET.</span><span class="sxs-lookup"><span data-stu-id="7313b-579">Refit is a REST library for .NET.</span></span> <span data-ttu-id="7313b-580">Il convertit les API REST en interfaces dynamiques.</span><span class="sxs-lookup"><span data-stu-id="7313b-580">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="7313b-581">Une implémentation de l’interface est générée dynamiquement par le `RestService`, avec `HttpClient` pour faire les appels HTTP externes.</span><span class="sxs-lookup"><span data-stu-id="7313b-581">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="7313b-582">Une interface et une réponse sont définies pour représenter l’API externe et sa réponse :</span><span class="sxs-lookup"><span data-stu-id="7313b-582">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="7313b-583">Vous pouvez ajouter un client typé en utilisant Refit pour générer l’implémentation :</span><span class="sxs-lookup"><span data-stu-id="7313b-583">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="7313b-584">L’interface définie peut être utilisée quand c’est nécessaire, avec l’implémentation fournie par l’injection de dépendances et Refit :</span><span class="sxs-lookup"><span data-stu-id="7313b-584">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="7313b-585">Middleware pour les requêtes sortantes</span><span class="sxs-lookup"><span data-stu-id="7313b-585">Outgoing request middleware</span></span>

<span data-ttu-id="7313b-586">`HttpClient` intègre déjà le concept de délégation des gestionnaires qui peuvent être liés ensemble pour les requêtes HTTP sortantes.</span><span class="sxs-lookup"><span data-stu-id="7313b-586">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="7313b-587">Le `IHttpClientFactory` permet de définir facilement les gestionnaires à appliquer pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="7313b-587">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="7313b-588">Il prend en charge l’inscription et le chaînage de plusieurs gestionnaires pour créer un pipeline de middlewares pour les requêtes sortantes.</span><span class="sxs-lookup"><span data-stu-id="7313b-588">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="7313b-589">Chacun de ces gestionnaires peut effectuer un travail avant et après la requête sortante.</span><span class="sxs-lookup"><span data-stu-id="7313b-589">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="7313b-590">Ce modèle est similaire au pipeline de middlewares entrants dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7313b-590">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="7313b-591">Le modèle fournit un mécanisme permettant de gérer les problèmes transversaux liés aux des requêtes HTTP, notamment la mise en cache, la gestion des erreurs, la sérialisation et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="7313b-591">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="7313b-592">Pour créer un gestionnaire, définissez une classe dérivant de <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="7313b-592">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="7313b-593">Remplacez la méthode `SendAsync` de façon à exécuter du code avant de passer la requête au gestionnaire suivant dans le pipeline :</span><span class="sxs-lookup"><span data-stu-id="7313b-593">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="7313b-594">Le code précédent définit un gestionnaire de base.</span><span class="sxs-lookup"><span data-stu-id="7313b-594">The preceding code defines a basic handler.</span></span> <span data-ttu-id="7313b-595">Il vérifie si un en-tête `X-API-KEY` a été inclus dans la requête.</span><span class="sxs-lookup"><span data-stu-id="7313b-595">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="7313b-596">Si l’en-tête est manquant, il peut éviter l’appel HTTP et retourner une réponse appropriée.</span><span class="sxs-lookup"><span data-stu-id="7313b-596">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="7313b-597">Au cours de l’inscription, un ou plusieurs gestionnaires peuvent être ajoutés à la configuration d’un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-597">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="7313b-598">Cette tâche est accomplie via des méthodes d’extension sur le <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="7313b-598">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="7313b-599">Dans le code précédent, le `ValidateHeaderHandler` est inscrit avec une injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="7313b-599">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="7313b-600">Le gestionnaire **doit** être inscrit dans l’injection de dépendances en tant que service temporaire, sans étendue.</span><span class="sxs-lookup"><span data-stu-id="7313b-600">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="7313b-601">Si le gestionnaire est inscrit en tant que service étendu et que des services dont dépend le gestionnaire peuvent être supprimés :</span><span class="sxs-lookup"><span data-stu-id="7313b-601">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="7313b-602">Les services du gestionnaire peuvent être supprimés avant que le gestionnaire ne soit hors de portée.</span><span class="sxs-lookup"><span data-stu-id="7313b-602">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="7313b-603">Les services du gestionnaire supprimés entraînent un échec du gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="7313b-603">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="7313b-604">Une fois inscrit, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> peut être appelé en passant en entrée le type du gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="7313b-604">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="7313b-605">Vous pouvez inscrire plusieurs gestionnaires dans l’ordre où ils doivent être exécutés.</span><span class="sxs-lookup"><span data-stu-id="7313b-605">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="7313b-606">Chaque gestionnaire wrappe le gestionnaire suivant jusqu’à ce que le dernier `HttpClientHandler` exécute la requête :</span><span class="sxs-lookup"><span data-stu-id="7313b-606">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="7313b-607">Utilisez l’une des approches suivantes pour partager l’état de chaque requête avec les gestionnaires de messages :</span><span class="sxs-lookup"><span data-stu-id="7313b-607">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="7313b-608">Passez des données dans le gestionnaire en utilisant `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="7313b-608">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="7313b-609">Utilisez `IHttpContextAccessor` pour accéder à la requête en cours.</span><span class="sxs-lookup"><span data-stu-id="7313b-609">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="7313b-610">Créez un objet de stockage `AsyncLocal` personnalisé pour passer les données.</span><span class="sxs-lookup"><span data-stu-id="7313b-610">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="7313b-611">Utiliser les gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="7313b-611">Use Polly-based handlers</span></span>

<span data-ttu-id="7313b-612">`IHttpClientFactory` s’intègre à une bibliothèque tierce très utilisée, appelée [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="7313b-612">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="7313b-613">Polly est une bibliothèque complète de gestion des erreurs transitoires et de résilience pour .NET.</span><span class="sxs-lookup"><span data-stu-id="7313b-613">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="7313b-614">Elle permet aux développeurs de formuler facilement et de façon thread-safe des stratégies, comme Retry (Nouvelle tentative), Circuit Breaker (Disjoncteur), Timeout (Délai d’attente), Bulkhead Isolation (Isolation par cloisonnement) et Fallback (Alternative de repli).</span><span class="sxs-lookup"><span data-stu-id="7313b-614">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="7313b-615">Des méthodes d’extension sont fournies pour permettre l’utilisation de stratégies Polly avec les instances configurées de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-615">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="7313b-616">Les extensions Polly :</span><span class="sxs-lookup"><span data-stu-id="7313b-616">The Polly extensions:</span></span>

* <span data-ttu-id="7313b-617">Prennent en charge l’ajout de gestionnaires Polly à des clients.</span><span class="sxs-lookup"><span data-stu-id="7313b-617">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="7313b-618">Peuvent être utilisées après l’installation du package NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="7313b-618">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="7313b-619">Ce package n’est pas inclus dans le framework partagé ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7313b-619">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="7313b-620">Gérer les erreurs temporaires</span><span class="sxs-lookup"><span data-stu-id="7313b-620">Handle transient faults</span></span>

<span data-ttu-id="7313b-621">Les erreurs courantes se produisent lorsque des appels HTTP externes sont temporaires.</span><span class="sxs-lookup"><span data-stu-id="7313b-621">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="7313b-622">Une méthode d’extension pratique nommée `AddTransientHttpErrorPolicy` est incluse : elle permet de définir une stratégie pour gérer les erreurs temporaires.</span><span class="sxs-lookup"><span data-stu-id="7313b-622">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="7313b-623">Les stratégies configurées avec cette méthode d’extension gèrent `HttpRequestException`, les réponses HTTP 5xx et les réponses HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="7313b-623">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="7313b-624">L’extension `AddTransientHttpErrorPolicy` peut être utilisée dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7313b-624">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="7313b-625">L’extension fournit l’accès à un objet `PolicyBuilder` configuré pour gérer les erreurs représentant une erreur temporaire possible :</span><span class="sxs-lookup"><span data-stu-id="7313b-625">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="7313b-626">Dans le code précédent, une stratégie `WaitAndRetryAsync` est définie.</span><span class="sxs-lookup"><span data-stu-id="7313b-626">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="7313b-627">Les requêtes qui ont échoué sont retentées jusqu’à trois fois avec un délai de 600 ms entre les tentatives.</span><span class="sxs-lookup"><span data-stu-id="7313b-627">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="7313b-628">Sélectionner dynamiquement des stratégies</span><span class="sxs-lookup"><span data-stu-id="7313b-628">Dynamically select policies</span></span>

<span data-ttu-id="7313b-629">Il existe d’autres méthodes d’extension que vous pouvez utiliser pour ajouter des gestionnaires Polly.</span><span class="sxs-lookup"><span data-stu-id="7313b-629">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="7313b-630">Une de ces extensions est `AddPolicyHandler`, qui a plusieurs surcharges.</span><span class="sxs-lookup"><span data-stu-id="7313b-630">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="7313b-631">Une de ces surcharges permet l’inspection de la requête lors de la définition de la stratégie à appliquer :</span><span class="sxs-lookup"><span data-stu-id="7313b-631">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="7313b-632">Dans le code précédent, si la requête sortante est un HTTP GET, un délai d’attente de 10 secondes est appliqué.</span><span class="sxs-lookup"><span data-stu-id="7313b-632">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="7313b-633">Pour toutes les autres méthodes HTTP, un délai d’attente de 30 secondes est utilisé.</span><span class="sxs-lookup"><span data-stu-id="7313b-633">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="7313b-634">Ajouter plusieurs gestionnaires Polly</span><span class="sxs-lookup"><span data-stu-id="7313b-634">Add multiple Polly handlers</span></span>

<span data-ttu-id="7313b-635">Il est courant d’imbriquer des stratégies Polly pour fournir des fonctionnalités améliorées :</span><span class="sxs-lookup"><span data-stu-id="7313b-635">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="7313b-636">Dans l’exemple précédent, deux gestionnaires sont ajoutés.</span><span class="sxs-lookup"><span data-stu-id="7313b-636">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="7313b-637">Le premier utilise l’extension `AddTransientHttpErrorPolicy` pour ajouter une stratégie de nouvelle tentative.</span><span class="sxs-lookup"><span data-stu-id="7313b-637">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="7313b-638">Les requêtes qui ont échoué sont retentées jusqu’à trois fois.</span><span class="sxs-lookup"><span data-stu-id="7313b-638">Failed requests are retried up to three times.</span></span> <span data-ttu-id="7313b-639">Le deuxième appel à `AddTransientHttpErrorPolicy` ajoute une stratégie de disjoncteur.</span><span class="sxs-lookup"><span data-stu-id="7313b-639">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="7313b-640">Les requêtes externes supplémentaires sont bloquées pendant 30 secondes si cinq tentatives successives échouent.</span><span class="sxs-lookup"><span data-stu-id="7313b-640">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="7313b-641">Les stratégies de disjoncteur sont avec état.</span><span class="sxs-lookup"><span data-stu-id="7313b-641">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="7313b-642">Tous les appels effectués via ce client partagent le même état du circuit.</span><span class="sxs-lookup"><span data-stu-id="7313b-642">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="7313b-643">Ajouter des stratégies à partir du Registre Polly</span><span class="sxs-lookup"><span data-stu-id="7313b-643">Add policies from the Polly registry</span></span>

<span data-ttu-id="7313b-644">Une approche de la gestion des stratégies régulièrement utilisées consiste à les définir une seule fois et à les inscrire avec un `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="7313b-644">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="7313b-645">Il existe une méthode d’extension qui permet l’ajout d’un gestionnaire avec une stratégie du Registre :</span><span class="sxs-lookup"><span data-stu-id="7313b-645">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="7313b-646">Dans le code précédent, deux stratégies sont inscrites lorsque `PolicyRegistry` est ajouté à `ServiceCollection`.</span><span class="sxs-lookup"><span data-stu-id="7313b-646">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="7313b-647">Pour utiliser une stratégie du Registre, la méthode `AddPolicyHandlerFromRegistry` est utilisée, en passant le nom de la stratégie à appliquer.</span><span class="sxs-lookup"><span data-stu-id="7313b-647">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="7313b-648">Vous trouverez plus d’informations sur les intégrations de `IHttpClientFactory` et de Polly dans le [wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="7313b-648">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="7313b-649">HttpClient et gestion de la durée de vie</span><span class="sxs-lookup"><span data-stu-id="7313b-649">HttpClient and lifetime management</span></span>

<span data-ttu-id="7313b-650">Une nouvelle instance `HttpClient` est retournée à chaque fois que `CreateClient` est appelé sur `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="7313b-650">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="7313b-651">Il existe un <xref:System.Net.Http.HttpMessageHandler> par client nommé.</span><span class="sxs-lookup"><span data-stu-id="7313b-651">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="7313b-652">La fabrique gère les durées de vie des instances `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="7313b-652">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="7313b-653">`IHttpClientFactory` regroupe dans un pool les instances de `HttpMessageHandler` créées par la fabrique pour réduire la consommation des ressources.</span><span class="sxs-lookup"><span data-stu-id="7313b-653">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="7313b-654">Une instance de `HttpMessageHandler` peut être réutilisée à partir du pool quand vous créez une instance de `HttpClient` si sa durée de vie n’a pas expiré.</span><span class="sxs-lookup"><span data-stu-id="7313b-654">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="7313b-655">Le regroupement de gestionnaires en pools est souhaitable, car chaque gestionnaire gère habituellement ses propres connexions HTTP sous-jacentes.</span><span class="sxs-lookup"><span data-stu-id="7313b-655">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="7313b-656">La création de plus de gestionnaires que nécessaire peut entraîner des délais de connexion.</span><span class="sxs-lookup"><span data-stu-id="7313b-656">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="7313b-657">Certains gestionnaires conservent aussi les connexions indéfiniment ouvertes, ce qui peut empêcher le gestionnaire de réagir aux changements du DNS.</span><span class="sxs-lookup"><span data-stu-id="7313b-657">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="7313b-658">La durée de vie par défaut d’un gestionnaire est de deux minutes.</span><span class="sxs-lookup"><span data-stu-id="7313b-658">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="7313b-659">La valeur par défaut peut être remplacée pour chaque client nommé.</span><span class="sxs-lookup"><span data-stu-id="7313b-659">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="7313b-660">Pour la remplacer, appelez <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> sur le `IHttpClientBuilder` qui est retourné lors de la création du client :</span><span class="sxs-lookup"><span data-stu-id="7313b-660">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="7313b-661">La suppression du client n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="7313b-661">Disposal of the client isn't required.</span></span> <span data-ttu-id="7313b-662">La suppression annule les requêtes sortantes et garantit que l’instance `HttpClient` donnée ne peut pas être utilisée après avoir appelé <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="7313b-662">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="7313b-663">`IHttpClientFactory` effectue le suivi et libère les ressources utilisées par les instances `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-663">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="7313b-664">Les instances `HttpClient` peuvent généralement être traitées en tant qu’objets .NET ne nécessitant pas une suppression.</span><span class="sxs-lookup"><span data-stu-id="7313b-664">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="7313b-665">Le fait de conserver une seule instance `HttpClient` active pendant une longue durée est un modèle commun utilisé avant le lancement de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="7313b-665">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="7313b-666">Ce modèle devient inutile après la migration vers `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="7313b-666">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="7313b-667">Alternatives à IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="7313b-667">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="7313b-668">L’utilisation de `IHttpClientFactory` dans une application DI-Enabled évite les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="7313b-668">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="7313b-669">Problèmes d’épuisement des ressources par le regroupement `HttpMessageHandler` instances.</span><span class="sxs-lookup"><span data-stu-id="7313b-669">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="7313b-670">Problèmes DNS périmés par le cycle `HttpMessageHandler` instances à intervalles réguliers.</span><span class="sxs-lookup"><span data-stu-id="7313b-670">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="7313b-671">Il existe d’autres façons de résoudre les problèmes précédents à l’aide d’une instance de <xref:System.Net.Http.SocketsHttpHandler> à long terme.</span><span class="sxs-lookup"><span data-stu-id="7313b-671">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="7313b-672">Créez une instance de `SocketsHttpHandler` lorsque l’application démarre et utilisez-la pendant toute la durée de vie de l’application.</span><span class="sxs-lookup"><span data-stu-id="7313b-672">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="7313b-673">Configurez <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> à une valeur appropriée en fonction des temps d’actualisation DNS.</span><span class="sxs-lookup"><span data-stu-id="7313b-673">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="7313b-674">Créez `HttpClient` instances à l’aide d' `new HttpClient(handler, disposeHandler: false)` en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="7313b-674">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="7313b-675">Les approches précédentes résolvent les problèmes de gestion des ressources qui `IHttpClientFactory` résolus de la même façon.</span><span class="sxs-lookup"><span data-stu-id="7313b-675">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="7313b-676">Le `SocketsHttpHandler` partage les connexions entre les instances de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-676">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="7313b-677">Ce partage empêche l’épuisement des sockets.</span><span class="sxs-lookup"><span data-stu-id="7313b-677">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="7313b-678">Le `SocketsHttpHandler` cycle les connexions en fonction des `PooledConnectionLifetime` afin d’éviter les problèmes DNS périmés.</span><span class="sxs-lookup"><span data-stu-id="7313b-678">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="7313b-679">Cookies</span><span class="sxs-lookup"><span data-stu-id="7313b-679">Cookies</span></span>

<span data-ttu-id="7313b-680">Les instances de `HttpMessageHandler` regroupées entraînent le partage de `CookieContainer` objets.</span><span class="sxs-lookup"><span data-stu-id="7313b-680">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="7313b-681">Le partage d’objets `CookieContainer` imprévus aboutit souvent à un code incorrect.</span><span class="sxs-lookup"><span data-stu-id="7313b-681">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="7313b-682">Pour les applications qui nécessitent des cookies, utilisez l’une des deux opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="7313b-682">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="7313b-683">Désactivation de la gestion automatique des cookies</span><span class="sxs-lookup"><span data-stu-id="7313b-683">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="7313b-684">Éviter les `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="7313b-684">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="7313b-685">Appelez <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> pour désactiver la gestion automatique des cookies :</span><span class="sxs-lookup"><span data-stu-id="7313b-685">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="7313b-686">Journalisation</span><span class="sxs-lookup"><span data-stu-id="7313b-686">Logging</span></span>

<span data-ttu-id="7313b-687">Les clients créés via `IHttpClientFactory` enregistrent les messages de journalisation pour toutes les requêtes.</span><span class="sxs-lookup"><span data-stu-id="7313b-687">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="7313b-688">Activez le niveau d’informations approprié dans votre configuration de journalisation pour voir les messages de journalisation par défaut.</span><span class="sxs-lookup"><span data-stu-id="7313b-688">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="7313b-689">Une journalisation supplémentaire, comme celle des en-têtes des requêtes, est incluse seulement au niveau de trace.</span><span class="sxs-lookup"><span data-stu-id="7313b-689">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="7313b-690">La catégorie de journal utilisée pour chaque client comprend le nom du client.</span><span class="sxs-lookup"><span data-stu-id="7313b-690">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="7313b-691">Par exemple, un client nommé *MyNamedClient* journalise les messages avec la catégorie `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="7313b-691">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="7313b-692">Les messages avec le suffixe *LogicalHandler* se produisent en dehors du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="7313b-692">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="7313b-693">Lors d’une requête, les messages sont journalisés avant que d’autres gestionnaires du pipeline l’aient traitée.</span><span class="sxs-lookup"><span data-stu-id="7313b-693">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="7313b-694">Lors d’une réponse, les messages sont journalisés une fois que tous les autres gestionnaires du pipeline ont reçu la réponse.</span><span class="sxs-lookup"><span data-stu-id="7313b-694">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="7313b-695">La journalisation se produit également à l’intérieur du pipeline du gestionnaire de requêtes.</span><span class="sxs-lookup"><span data-stu-id="7313b-695">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="7313b-696">Dans l’exemple *MyNamedClient*, ces messages sont journalisés avec la catégorie de journalisation `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="7313b-696">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="7313b-697">Pour la requête, cela se produit après que tous les autres gestionnaires ont été exécutés et immédiatement avant l’envoi de la requête sur le réseau.</span><span class="sxs-lookup"><span data-stu-id="7313b-697">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="7313b-698">Lors de la réponse, cette journalisation inclut l’état de la réponse avant qu’elle repasse à travers le pipeline de gestionnaires.</span><span class="sxs-lookup"><span data-stu-id="7313b-698">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="7313b-699">L’activation de la journalisation à l’extérieur et à l’intérieur du pipeline permet l’inspection des changements apportés par les autres gestionnaires du pipeline.</span><span class="sxs-lookup"><span data-stu-id="7313b-699">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="7313b-700">Par exemple, cela peut comprendre des changements apportés aux en-têtes des requêtes ou au code d’état de la réponse.</span><span class="sxs-lookup"><span data-stu-id="7313b-700">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="7313b-701">L’ajout du nom du client dans la catégorie de journalisation permet si nécessaire de filtrer le journal pour des clients nommés spécifiques.</span><span class="sxs-lookup"><span data-stu-id="7313b-701">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="7313b-702">Configurer le HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="7313b-702">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="7313b-703">Il peut être nécessaire de contrôler la configuration du `HttpMessageHandler` interne utilisé par un client.</span><span class="sxs-lookup"><span data-stu-id="7313b-703">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="7313b-704">Un `IHttpClientBuilder` est retourné quand vous ajoutez des clients nommés ou typés.</span><span class="sxs-lookup"><span data-stu-id="7313b-704">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="7313b-705">La méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> peut être utilisée pour définir un délégué.</span><span class="sxs-lookup"><span data-stu-id="7313b-705">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="7313b-706">Le délégué est utilisé pour créer et configurer le `HttpMessageHandler` principal utilisé par ce client :</span><span class="sxs-lookup"><span data-stu-id="7313b-706">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="7313b-707">Utiliser IHttpClientFactory dans une application console</span><span class="sxs-lookup"><span data-stu-id="7313b-707">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="7313b-708">Dans une application console, ajoutez les références de package suivantes au projet :</span><span class="sxs-lookup"><span data-stu-id="7313b-708">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="7313b-709">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="7313b-709">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="7313b-710">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="7313b-710">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="7313b-711">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="7313b-711">In the following example:</span></span>

* <span data-ttu-id="7313b-712"><xref:System.Net.Http.IHttpClientFactory> est inscrit dans le conteneur de service dans de l’[hôte générique](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="7313b-712"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="7313b-713">`MyService` crée une instance de fabrique cliente à partir du service, qui est utilisée pour créer un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="7313b-713">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="7313b-714">`HttpClient` est utilisé pour récupérer une page web.</span><span class="sxs-lookup"><span data-stu-id="7313b-714">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="7313b-715">`Main` crée une étendue pour exécuter la méthode `GetPage` du service et écrire les 500 premiers caractères du contenu de la page web dans la console.</span><span class="sxs-lookup"><span data-stu-id="7313b-715">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="header-propagation-middleware"></a><span data-ttu-id="7313b-716">Intergiciel (middleware) de propagation d’en-tête</span><span class="sxs-lookup"><span data-stu-id="7313b-716">Header propagation middleware</span></span>

<span data-ttu-id="7313b-717">La propagation d’en-tête est un intergiciel (middleware) pris en charge par la communauté pour propager les en-têtes HTTP de la demande entrante aux demandes du client HTTP sortantes.</span><span class="sxs-lookup"><span data-stu-id="7313b-717">Header propagation is a community supported middleware to propagate HTTP headers from the incoming request to the outgoing HTTP Client requests.</span></span> <span data-ttu-id="7313b-718">Pour utiliser la propagation d’en-tête :</span><span class="sxs-lookup"><span data-stu-id="7313b-718">To use header propagation:</span></span>

* <span data-ttu-id="7313b-719">Référencez le port de la communauté pris en charge du package [HeaderPropagation](https://www.nuget.org/packages/HeaderPropagation).</span><span class="sxs-lookup"><span data-stu-id="7313b-719">Reference the community supported port of the package [HeaderPropagation](https://www.nuget.org/packages/HeaderPropagation).</span></span> <span data-ttu-id="7313b-720">ASP.NET Core 3,1 et versions ultérieures prennent en charge [Microsoft. AspNetCore. HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation).</span><span class="sxs-lookup"><span data-stu-id="7313b-720">ASP.NET Core 3.1 and later supports [Microsoft.AspNetCore.HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation).</span></span>

* <span data-ttu-id="7313b-721">Configurez les intergiciels et les `HttpClient` dans `Startup`:</span><span class="sxs-lookup"><span data-stu-id="7313b-721">Configure the middleware and `HttpClient` in `Startup`:</span></span>

  [!code-csharp[](http-requests/samples/2.x/Startup21.cs?highlight=5-9,25&name=snippet)]

* <span data-ttu-id="7313b-722">Le client comprend les en-têtes configurés pour les demandes sortantes :</span><span class="sxs-lookup"><span data-stu-id="7313b-722">The client includes the configured headers on outbound requests:</span></span>

  ```C#
  var client = clientFactory.CreateClient("MyForwardingClient");
  var response = client.GetAsync(...);
  ```

## <a name="additional-resources"></a><span data-ttu-id="7313b-723">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="7313b-723">Additional resources</span></span>

* [<span data-ttu-id="7313b-724">Utiliser HttpClientFactory pour implémenter des requêtes HTTP résilientes</span><span class="sxs-lookup"><span data-stu-id="7313b-724">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="7313b-725">Implémenter de nouvelles tentatives d’appel HTTP avec interruption exponentielle avec des stratégies Polly et HttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="7313b-725">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="7313b-726">Implémenter le modèle Disjoncteur</span><span class="sxs-lookup"><span data-stu-id="7313b-726">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
