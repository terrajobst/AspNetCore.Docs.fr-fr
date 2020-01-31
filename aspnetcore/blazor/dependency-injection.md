---
title: ASP.NET Core Blazor l’injection de dépendances
author: guardrex
description: Découvrez comment Blazor applications peuvent injecter des services dans des composants.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2020
no-loc:
- Blazor
- SignalR
uid: blazor/dependency-injection
ms.openlocfilehash: 859fd484fc00104575f176fa7d3bf752895475a0
ms.sourcegitcommit: c81ef12a1b6e6ac838e5e07042717cf492e6635b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76885499"
---
# <a name="aspnet-core-blazor-dependency-injection"></a><span data-ttu-id="b7666-103">ASP.NET Core l’injection de dépendances éblouissantes</span><span class="sxs-lookup"><span data-stu-id="b7666-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="b7666-104">Par [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="b7666-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="b7666-105">Éblouissant prend en charge l' [injection de dépendances (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b7666-105">Blazor supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="b7666-106">Les applications peuvent utiliser des services intégrés en les injectant dans des composants.</span><span class="sxs-lookup"><span data-stu-id="b7666-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="b7666-107">Les applications peuvent également définir et inscrire des services personnalisés et les rendre disponibles dans toute l’application via DI.</span><span class="sxs-lookup"><span data-stu-id="b7666-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="b7666-108">La méthode DI est une technique permettant d’accéder à des services configurés dans un emplacement central.</span><span class="sxs-lookup"><span data-stu-id="b7666-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="b7666-109">Cela peut être utile dans les applications éblouissantes pour :</span><span class="sxs-lookup"><span data-stu-id="b7666-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="b7666-110">Partager une seule instance d’une classe de service sur de nombreux composants, appelé service *Singleton* .</span><span class="sxs-lookup"><span data-stu-id="b7666-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="b7666-111">Découplez les composants des classes de service concrètes à l’aide d’abstractions de référence.</span><span class="sxs-lookup"><span data-stu-id="b7666-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="b7666-112">Par exemple, considérez une interface `IDataAccess` pour accéder aux données de l’application.</span><span class="sxs-lookup"><span data-stu-id="b7666-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="b7666-113">L’interface est implémentée par une classe `DataAccess` concrète et inscrite en tant que service dans le conteneur de services de l’application.</span><span class="sxs-lookup"><span data-stu-id="b7666-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="b7666-114">Lorsqu’un composant utilise DI pour recevoir une implémentation `IDataAccess`, le composant n’est pas couplé au type concret.</span><span class="sxs-lookup"><span data-stu-id="b7666-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="b7666-115">L’implémentation peut être permutée, peut-être pour une implémentation factice dans les tests unitaires.</span><span class="sxs-lookup"><span data-stu-id="b7666-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="b7666-116">Services par défaut</span><span class="sxs-lookup"><span data-stu-id="b7666-116">Default services</span></span>

<span data-ttu-id="b7666-117">Les services par défaut sont automatiquement ajoutés à la collection de services de l’application.</span><span class="sxs-lookup"><span data-stu-id="b7666-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="b7666-118">Service</span><span class="sxs-lookup"><span data-stu-id="b7666-118">Service</span></span> | <span data-ttu-id="b7666-119">Durée de vie</span><span class="sxs-lookup"><span data-stu-id="b7666-119">Lifetime</span></span> | <span data-ttu-id="b7666-120">Description</span><span class="sxs-lookup"><span data-stu-id="b7666-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="b7666-121">Singleton</span><span class="sxs-lookup"><span data-stu-id="b7666-121">Singleton</span></span> | <span data-ttu-id="b7666-122">Fournit des méthodes pour envoyer des requêtes HTTP et recevoir des réponses HTTP d’une ressource identifiée par un URI.</span><span class="sxs-lookup"><span data-stu-id="b7666-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span><br><br><span data-ttu-id="b7666-123">L’instance de `HttpClient` dans une application de webassembly éblouissant utilise le navigateur pour gérer le trafic HTTP en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="b7666-123">The instance of `HttpClient` in a Blazor WebAssembly app uses the browser for handling the HTTP traffic in the background.</span></span><br><br><span data-ttu-id="b7666-124">Les applications serveur éblouissantes n’incluent pas une `HttpClient` configurée comme un service par défaut.</span><span class="sxs-lookup"><span data-stu-id="b7666-124">Blazor Server apps don't include an `HttpClient` configured as a service by default.</span></span> <span data-ttu-id="b7666-125">Fournissez une `HttpClient` à une application de serveur éblouissante.</span><span class="sxs-lookup"><span data-stu-id="b7666-125">Provide an `HttpClient` to a Blazor Server app.</span></span><br><br><span data-ttu-id="b7666-126">Pour plus d'informations, consultez <xref:blazor/call-web-api>.</span><span class="sxs-lookup"><span data-stu-id="b7666-126">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="b7666-127">Singleton (webassembly éblouissant)</span><span class="sxs-lookup"><span data-stu-id="b7666-127">Singleton (Blazor WebAssembly)</span></span><br><span data-ttu-id="b7666-128">Étendu (serveur éblouissant)</span><span class="sxs-lookup"><span data-stu-id="b7666-128">Scoped (Blazor Server)</span></span> | <span data-ttu-id="b7666-129">Représente une instance d’un Runtime JavaScript dans laquelle les appels JavaScript sont distribués.</span><span class="sxs-lookup"><span data-stu-id="b7666-129">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="b7666-130">Pour plus d'informations, consultez <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="b7666-130">For more information, see <xref:blazor/javascript-interop>.</span></span> |
| `NavigationManager` | <span data-ttu-id="b7666-131">Singleton (webassembly éblouissant)</span><span class="sxs-lookup"><span data-stu-id="b7666-131">Singleton (Blazor WebAssembly)</span></span><br><span data-ttu-id="b7666-132">Étendu (serveur éblouissant)</span><span class="sxs-lookup"><span data-stu-id="b7666-132">Scoped (Blazor Server)</span></span> | <span data-ttu-id="b7666-133">Contient des assistances pour l’utilisation des URI et de l’état de navigation.</span><span class="sxs-lookup"><span data-stu-id="b7666-133">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="b7666-134">Pour plus d’informations, consultez [URI et assistance de l’état de navigation](xref:blazor/routing#uri-and-navigation-state-helpers).</span><span class="sxs-lookup"><span data-stu-id="b7666-134">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="b7666-135">Un fournisseur de services personnalisé ne fournit pas automatiquement les services par défaut indiqués dans le tableau.</span><span class="sxs-lookup"><span data-stu-id="b7666-135">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="b7666-136">Si vous utilisez un fournisseur de services personnalisé et que vous avez besoin de l’un des services répertoriés dans le tableau, ajoutez les services requis au nouveau fournisseur de services.</span><span class="sxs-lookup"><span data-stu-id="b7666-136">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="b7666-137">Ajouter des services à une application</span><span class="sxs-lookup"><span data-stu-id="b7666-137">Add services to an app</span></span>

### <a name="blazor-webassembly"></a><span data-ttu-id="b7666-138">WebAssembly Blazor</span><span class="sxs-lookup"><span data-stu-id="b7666-138">Blazor WebAssembly</span></span>

<span data-ttu-id="b7666-139">Configurez les services de la collection de services de l’application dans la méthode `Main` de *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="b7666-139">Configure services for the app's service collection in the `Main` method of *Program.cs*.</span></span> <span data-ttu-id="b7666-140">Dans l’exemple suivant, l’implémentation de `MyDependency` est inscrite pour `IMyDependency`:</span><span class="sxs-lookup"><span data-stu-id="b7666-140">In the following example, the `MyDependency` implementation is registered for `IMyDependency`:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<IMyDependency, MyDependency>();
        builder.RootComponents.Add<App>("app");

        await builder.Build().RunAsync();
    }
}
```

<span data-ttu-id="b7666-141">Une fois l’hôte généré, les services sont accessibles à partir de l’étendue de l’injection de comptes racine avant le rendu des composants.</span><span class="sxs-lookup"><span data-stu-id="b7666-141">Once the host is built, services can be accessed from the root DI scope before any components are rendered.</span></span> <span data-ttu-id="b7666-142">Cela peut être utile pour l’exécution de la logique d’initialisation avant le rendu du contenu :</span><span class="sxs-lookup"><span data-stu-id="b7666-142">This can be useful for running initialization logic before rendering content:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<WeatherService>();
        builder.RootComponents.Add<App>("app");

        var host = builder.Build();

        var weatherService = host.Services.GetRequiredService<WeatherService>();
        await weatherService.InitializeWeatherAsync();

        await host.RunAsync();
    }
}
```

<span data-ttu-id="b7666-143">L’hôte fournit également une instance de configuration centrale pour l’application.</span><span class="sxs-lookup"><span data-stu-id="b7666-143">The host also provides a central configuration instance for the app.</span></span> <span data-ttu-id="b7666-144">En s’appuyant sur l’exemple précédent, l’URL du service météo est passée d’une source de configuration par défaut (par exemple, *appSettings. JSON*) à `InitializeWeatherAsync`:</span><span class="sxs-lookup"><span data-stu-id="b7666-144">Building on the preceding example, the weather service's URL is passed from a default configuration source (for example, *appsettings.json*) to `InitializeWeatherAsync`:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<WeatherService>();
        builder.RootComponents.Add<App>("app");

        var host = builder.Build();

        var weatherService = host.Services.GetRequiredService<WeatherService>();
        await weatherService.InitializeWeatherAsync(
            host.Configuration["WeatherServiceUrl"]);

        await host.RunAsync();
    }
}
```

### <a name="blazor-server"></a><span data-ttu-id="b7666-145">Serveur Blazor</span><span class="sxs-lookup"><span data-stu-id="b7666-145">Blazor Server</span></span>

<span data-ttu-id="b7666-146">Après avoir créé une nouvelle application, examinez la méthode `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="b7666-146">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="b7666-147">Une <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, qui est une liste d’objets de descripteur de service (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>), est passée à la méthode `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b7666-147">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="b7666-148">Les services sont ajoutés en fournissant des descripteurs de service à la collection de services.</span><span class="sxs-lookup"><span data-stu-id="b7666-148">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="b7666-149">L’exemple suivant illustre le concept avec l’interface `IDataAccess` et son implémentation concrète `DataAccess`:</span><span class="sxs-lookup"><span data-stu-id="b7666-149">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

### <a name="service-lifetime"></a><span data-ttu-id="b7666-150">Durée de vie du service</span><span class="sxs-lookup"><span data-stu-id="b7666-150">Service lifetime</span></span>

<span data-ttu-id="b7666-151">Les services peuvent être configurés avec les durées de vie indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="b7666-151">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="b7666-152">Durée de vie</span><span class="sxs-lookup"><span data-stu-id="b7666-152">Lifetime</span></span> | <span data-ttu-id="b7666-153">Description</span><span class="sxs-lookup"><span data-stu-id="b7666-153">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor<span data-ttu-id="b7666-154"> applications webassembly n’ont pas actuellement de concept d’étendues DI.</span><span class="sxs-lookup"><span data-stu-id="b7666-154"> WebAssembly apps don't currently have a concept of DI scopes.</span></span> <span data-ttu-id="b7666-155">les services inscrits au `Scoped`se comportent comme des services `Singleton`.</span><span class="sxs-lookup"><span data-stu-id="b7666-155">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="b7666-156">Toutefois, le modèle d’hébergement de serveur Blazor prend en charge la durée de vie `Scoped`.</span><span class="sxs-lookup"><span data-stu-id="b7666-156">However, the Blazor Server hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="b7666-157">Dans les applications Blazor Server, l’inscription d’un service étendu est limitée à la *connexion*.</span><span class="sxs-lookup"><span data-stu-id="b7666-157">In Blazor Server apps, a scoped service registration is scoped to the *connection*.</span></span> <span data-ttu-id="b7666-158">Pour cette raison, il est préférable d’utiliser les services délimités pour les services qui doivent être étendus à l’utilisateur actuel, même si l’objectif actuel est d’exécuter côté client dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="b7666-158">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="b7666-159">DI crée une *seule instance* du service.</span><span class="sxs-lookup"><span data-stu-id="b7666-159">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="b7666-160">Tous les composants qui requièrent un service `Singleton` reçoivent une instance du même service.</span><span class="sxs-lookup"><span data-stu-id="b7666-160">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="b7666-161">Chaque fois qu’un composant obtient une instance d’un service `Transient` à partir du conteneur de service, il reçoit une *nouvelle instance* du service.</span><span class="sxs-lookup"><span data-stu-id="b7666-161">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="b7666-162">Le système DI est basé sur le système DI dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b7666-162">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="b7666-163">Pour plus d'informations, consultez <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="b7666-163">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="b7666-164">Demander un service dans un composant</span><span class="sxs-lookup"><span data-stu-id="b7666-164">Request a service in a component</span></span>

<span data-ttu-id="b7666-165">Une fois les services ajoutés à la collection de services, injectez les services dans les composants à l’aide de l' [\@injecter](xref:mvc/views/razor#inject) la directive Razor.</span><span class="sxs-lookup"><span data-stu-id="b7666-165">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="b7666-166">`@inject` a deux paramètres :</span><span class="sxs-lookup"><span data-stu-id="b7666-166">`@inject` has two parameters:</span></span>

* <span data-ttu-id="b7666-167">Tapez &ndash; le type du service à injecter.</span><span class="sxs-lookup"><span data-stu-id="b7666-167">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="b7666-168">Propriété &ndash; le nom de la propriété qui reçoit le service d’application injecté.</span><span class="sxs-lookup"><span data-stu-id="b7666-168">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="b7666-169">La propriété ne nécessite pas de création manuelle.</span><span class="sxs-lookup"><span data-stu-id="b7666-169">The property doesn't require manual creation.</span></span> <span data-ttu-id="b7666-170">Le compilateur crée la propriété.</span><span class="sxs-lookup"><span data-stu-id="b7666-170">The compiler creates the property.</span></span>

<span data-ttu-id="b7666-171">Pour plus d'informations, consultez <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="b7666-171">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="b7666-172">Utilisez plusieurs instructions `@inject` pour injecter différents services.</span><span class="sxs-lookup"><span data-stu-id="b7666-172">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="b7666-173">L'exemple suivant montre comment utiliser `@inject`.</span><span class="sxs-lookup"><span data-stu-id="b7666-173">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="b7666-174">Le service qui implémente `Services.IDataAccess` est injecté dans la `DataRepository`de propriété du composant.</span><span class="sxs-lookup"><span data-stu-id="b7666-174">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="b7666-175">Notez que le code utilise uniquement l’abstraction `IDataAccess` :</span><span class="sxs-lookup"><span data-stu-id="b7666-175">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-razor[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="b7666-176">En interne, la propriété générée (`DataRepository`) utilise l’attribut `InjectAttribute`.</span><span class="sxs-lookup"><span data-stu-id="b7666-176">Internally, the generated property (`DataRepository`) uses the `InjectAttribute` attribute.</span></span> <span data-ttu-id="b7666-177">En règle générale, cet attribut n’est pas utilisé directement.</span><span class="sxs-lookup"><span data-stu-id="b7666-177">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="b7666-178">Si une classe de base est requise pour les composants et les propriétés injectées sont également requises pour la classe de base, ajoutez manuellement l' `InjectAttribute`:</span><span class="sxs-lookup"><span data-stu-id="b7666-178">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="b7666-179">Dans les composants dérivés de la classe de base, la directive `@inject` n’est pas obligatoire.</span><span class="sxs-lookup"><span data-stu-id="b7666-179">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="b7666-180">Le `InjectAttribute` de la classe de base est suffisant :</span><span class="sxs-lookup"><span data-stu-id="b7666-180">The `InjectAttribute` of the base class is sufficient:</span></span>

```razor
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="b7666-181">Utiliser DI dans les services</span><span class="sxs-lookup"><span data-stu-id="b7666-181">Use DI in services</span></span>

<span data-ttu-id="b7666-182">Les services complexes peuvent nécessiter des services supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="b7666-182">Complex services might require additional services.</span></span> <span data-ttu-id="b7666-183">Dans l’exemple précédent, `DataAccess` peut nécessiter le service `HttpClient` par défaut.</span><span class="sxs-lookup"><span data-stu-id="b7666-183">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="b7666-184">`@inject` (ou le `InjectAttribute`) ne peut pas être utilisé dans les services.</span><span class="sxs-lookup"><span data-stu-id="b7666-184">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="b7666-185">L' *injection de constructeur* doit être utilisée à la place.</span><span class="sxs-lookup"><span data-stu-id="b7666-185">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="b7666-186">Les services requis sont ajoutés en ajoutant des paramètres au constructeur du service.</span><span class="sxs-lookup"><span data-stu-id="b7666-186">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="b7666-187">Lorsque DI crée le service, il reconnaît les services dont il a besoin dans le constructeur et les fournit en conséquence.</span><span class="sxs-lookup"><span data-stu-id="b7666-187">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

```csharp
public class DataAccess : IDataAccess
{
    // The constructor receives an HttpClient via dependency
    // injection. HttpClient is a default service.
    public DataAccess(HttpClient client)
    {
        ...
    }
}
```

<span data-ttu-id="b7666-188">Conditions préalables pour l’injection de constructeur :</span><span class="sxs-lookup"><span data-stu-id="b7666-188">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="b7666-189">Un constructeur doit exister dont les arguments peuvent tous être remplis par DI.</span><span class="sxs-lookup"><span data-stu-id="b7666-189">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="b7666-190">Les paramètres supplémentaires non couverts par DI sont autorisés s’ils spécifient des valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="b7666-190">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="b7666-191">Le constructeur applicable doit être *public*.</span><span class="sxs-lookup"><span data-stu-id="b7666-191">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="b7666-192">Un constructeur applicable doit exister.</span><span class="sxs-lookup"><span data-stu-id="b7666-192">One applicable constructor must exist.</span></span> <span data-ttu-id="b7666-193">En cas d’ambiguïté, DI lève une exception.</span><span class="sxs-lookup"><span data-stu-id="b7666-193">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a><span data-ttu-id="b7666-194">Classes de composants de base de l’utilitaire pour gérer une étendue DI</span><span class="sxs-lookup"><span data-stu-id="b7666-194">Utility base component classes to manage a DI scope</span></span>

<span data-ttu-id="b7666-195">Dans ASP.NET Core applications, les services délimités sont généralement étendus à la requête actuelle.</span><span class="sxs-lookup"><span data-stu-id="b7666-195">In ASP.NET Core apps, scoped services are typically scoped to the current request.</span></span> <span data-ttu-id="b7666-196">Une fois la demande terminée, tous les services délimités ou temporaires sont supprimés par le système DI.</span><span class="sxs-lookup"><span data-stu-id="b7666-196">After the request completes, any scoped or transient services are disposed by the DI system.</span></span> <span data-ttu-id="b7666-197">Dans les applications Blazor Server, l’étendue de la demande est limitée à la durée de la connexion client, ce qui peut entraîner des services transitoires et de portée de vie bien plus longs que prévu.</span><span class="sxs-lookup"><span data-stu-id="b7666-197">In Blazor Server apps, the request scope lasts for the duration of the client connection, which can result in transient and scoped services living much longer than expected.</span></span>

<span data-ttu-id="b7666-198">Pour étendre les services à la durée de vie d’un composant, vous pouvez utiliser les classes de base `OwningComponentBase` et `OwningComponentBase<TService>`.</span><span class="sxs-lookup"><span data-stu-id="b7666-198">To scope services to the lifetime of a component, you can use the `OwningComponentBase` and `OwningComponentBase<TService>` base classes.</span></span> <span data-ttu-id="b7666-199">Ces classes de base exposent une propriété `ScopedServices` de type `IServiceProvider` qui résolvent les services dont la portée est limitée à la durée de vie du composant.</span><span class="sxs-lookup"><span data-stu-id="b7666-199">These base classes expose a `ScopedServices` property of type `IServiceProvider` that resolve services that are scoped to the lifetime of the component.</span></span> <span data-ttu-id="b7666-200">Pour créer un composant qui hérite d’une classe de base dans Razor, utilisez la directive `@inherits`.</span><span class="sxs-lookup"><span data-stu-id="b7666-200">To author a component that inherits from a base class in Razor, use the `@inherits` directive.</span></span>

```razor
@page "/users"
@attribute [Authorize]
@inherits OwningComponentBase<Data.ApplicationDbContext>

<h1>Users (@Service.Users.Count())</h1>
<ul>
    @foreach (var user in Service.Users)
    {
        <li>@user.UserName</li>
    }
</ul>
```

> [!NOTE]
> <span data-ttu-id="b7666-201">Les services injectés dans le composant à l’aide de `@inject` ou le `InjectAttribute` ne sont pas créés dans l’étendue du composant et sont liés à l’étendue de la demande.</span><span class="sxs-lookup"><span data-stu-id="b7666-201">Services injected into the component using `@inject` or the `InjectAttribute` aren't created in the component's scope and are tied to the request scope.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b7666-202">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="b7666-202">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
