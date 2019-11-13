---
title: ASP.NET Core Blazor l’injection de dépendances
author: guardrex
description: Découvrez comment Blazor applications peuvent injecter des services dans des composants.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/dependency-injection
ms.openlocfilehash: a39d913636afc55ac9d831de923ba7ae8db1216b
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963076"
---
# <a name="aspnet-core-opno-locblazor-dependency-injection"></a><span data-ttu-id="462c7-103">ASP.NET Core Blazor l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="462c7-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="462c7-104">Par [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="462c7-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="462c7-105"> prend en charge l' [injection de dépendances (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="462c7-105"> supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="462c7-106">Les applications peuvent utiliser des services intégrés en les injectant dans des composants.</span><span class="sxs-lookup"><span data-stu-id="462c7-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="462c7-107">Les applications peuvent également définir et inscrire des services personnalisés et les rendre disponibles dans toute l’application via DI.</span><span class="sxs-lookup"><span data-stu-id="462c7-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="462c7-108">La méthode DI est une technique permettant d’accéder à des services configurés dans un emplacement central.</span><span class="sxs-lookup"><span data-stu-id="462c7-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="462c7-109">Cela peut être utile dans Blazor applications pour :</span><span class="sxs-lookup"><span data-stu-id="462c7-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="462c7-110">Partager une seule instance d’une classe de service sur de nombreux composants, appelé service *Singleton* .</span><span class="sxs-lookup"><span data-stu-id="462c7-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="462c7-111">Découplez les composants des classes de service concrètes à l’aide d’abstractions de référence.</span><span class="sxs-lookup"><span data-stu-id="462c7-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="462c7-112">Par exemple, considérez une interface `IDataAccess` pour accéder aux données dans l’application.</span><span class="sxs-lookup"><span data-stu-id="462c7-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="462c7-113">L’interface est implémentée par une classe `DataAccess` concrète et inscrite en tant que service dans le conteneur de services de l’application.</span><span class="sxs-lookup"><span data-stu-id="462c7-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="462c7-114">Quand un composant utilise DI pour recevoir une implémentation `IDataAccess`, le composant n’est pas couplé au type concret.</span><span class="sxs-lookup"><span data-stu-id="462c7-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="462c7-115">L’implémentation peut être permutée, peut-être pour une implémentation factice dans les tests unitaires.</span><span class="sxs-lookup"><span data-stu-id="462c7-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="462c7-116">Services par défaut</span><span class="sxs-lookup"><span data-stu-id="462c7-116">Default services</span></span>

<span data-ttu-id="462c7-117">Les services par défaut sont automatiquement ajoutés à la collection de services de l’application.</span><span class="sxs-lookup"><span data-stu-id="462c7-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="462c7-118">Service</span><span class="sxs-lookup"><span data-stu-id="462c7-118">Service</span></span> | <span data-ttu-id="462c7-119">Durée de vie</span><span class="sxs-lookup"><span data-stu-id="462c7-119">Lifetime</span></span> | <span data-ttu-id="462c7-120">Description</span><span class="sxs-lookup"><span data-stu-id="462c7-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="462c7-121">Singleton</span><span class="sxs-lookup"><span data-stu-id="462c7-121">Singleton</span></span> | <span data-ttu-id="462c7-122">Fournit des méthodes pour envoyer des requêtes HTTP et recevoir des réponses HTTP d’une ressource identifiée par un URI.</span><span class="sxs-lookup"><span data-stu-id="462c7-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span> <span data-ttu-id="462c7-123">Notez que cette instance de `HttpClient` utilise le navigateur pour gérer le trafic HTTP en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="462c7-123">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="462c7-124">[Httpclient. BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) est automatiquement défini sur le préfixe URI de base de l’application.</span><span class="sxs-lookup"><span data-stu-id="462c7-124">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="462c7-125">Pour plus d'informations, consultez <xref:blazor/call-web-api>.</span><span class="sxs-lookup"><span data-stu-id="462c7-125">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="462c7-126">Singleton</span><span class="sxs-lookup"><span data-stu-id="462c7-126">Singleton</span></span> | <span data-ttu-id="462c7-127">Représente une instance d’un Runtime JavaScript dans laquelle les appels JavaScript sont distribués.</span><span class="sxs-lookup"><span data-stu-id="462c7-127">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="462c7-128">Pour plus d'informations, consultez <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="462c7-128">For more information, see <xref:blazor/javascript-interop>.</span></span> |
| `NavigationManager` | <span data-ttu-id="462c7-129">Singleton</span><span class="sxs-lookup"><span data-stu-id="462c7-129">Singleton</span></span> | <span data-ttu-id="462c7-130">Contient des assistances pour l’utilisation des URI et de l’état de navigation.</span><span class="sxs-lookup"><span data-stu-id="462c7-130">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="462c7-131">Pour plus d’informations, consultez [URI et assistance de l’état de navigation](xref:blazor/routing#uri-and-navigation-state-helpers).</span><span class="sxs-lookup"><span data-stu-id="462c7-131">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="462c7-132">Un fournisseur de services personnalisé ne fournit pas automatiquement les services par défaut indiqués dans le tableau.</span><span class="sxs-lookup"><span data-stu-id="462c7-132">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="462c7-133">Si vous utilisez un fournisseur de services personnalisé et que vous avez besoin de l’un des services répertoriés dans le tableau, ajoutez les services requis au nouveau fournisseur de services.</span><span class="sxs-lookup"><span data-stu-id="462c7-133">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="462c7-134">Ajouter des services à une application</span><span class="sxs-lookup"><span data-stu-id="462c7-134">Add services to an app</span></span>

<span data-ttu-id="462c7-135">Après avoir créé une nouvelle application, examinez la méthode `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="462c7-135">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="462c7-136">La méthode `ConfigureServices` reçoit une <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, qui est une liste d’objets descripteurs de service (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span><span class="sxs-lookup"><span data-stu-id="462c7-136">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="462c7-137">Les services sont ajoutés en fournissant des descripteurs de service à la collection de services.</span><span class="sxs-lookup"><span data-stu-id="462c7-137">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="462c7-138">L’exemple suivant illustre le concept avec l’interface `IDataAccess` et son implémentation concrète `DataAccess` :</span><span class="sxs-lookup"><span data-stu-id="462c7-138">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="462c7-139">Les services peuvent être configurés avec les durées de vie indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="462c7-139">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="462c7-140">Durée de vie</span><span class="sxs-lookup"><span data-stu-id="462c7-140">Lifetime</span></span> | <span data-ttu-id="462c7-141">Description</span><span class="sxs-lookup"><span data-stu-id="462c7-141">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor<span data-ttu-id="462c7-142"> applications webassembly n’ont pas actuellement de concept d’étendues DI.</span><span class="sxs-lookup"><span data-stu-id="462c7-142"> WebAssembly apps don't currently have a concept of DI scopes.</span></span> <span data-ttu-id="462c7-143">les services inscrits au `Scoped`se comportent comme des services `Singleton`.</span><span class="sxs-lookup"><span data-stu-id="462c7-143">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="462c7-144">Toutefois, le modèle d’hébergement de serveur Blazor prend en charge la durée de vie `Scoped`.</span><span class="sxs-lookup"><span data-stu-id="462c7-144">However, the Blazor Server hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="462c7-145">Dans les applications Blazor Server, l’inscription d’un service étendu est limitée à la *connexion*.</span><span class="sxs-lookup"><span data-stu-id="462c7-145">In Blazor Server apps, a scoped service registration is scoped to the *connection*.</span></span> <span data-ttu-id="462c7-146">Pour cette raison, il est préférable d’utiliser les services délimités pour les services qui doivent être étendus à l’utilisateur actuel, même si l’objectif actuel est d’exécuter côté client dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="462c7-146">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="462c7-147">DI crée une *seule instance* du service.</span><span class="sxs-lookup"><span data-stu-id="462c7-147">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="462c7-148">Tous les composants nécessitant un service `Singleton` reçoivent une instance du même service.</span><span class="sxs-lookup"><span data-stu-id="462c7-148">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="462c7-149">Chaque fois qu’un composant obtient une instance d’un service `Transient` à partir du conteneur de service, il reçoit une *nouvelle instance* du service.</span><span class="sxs-lookup"><span data-stu-id="462c7-149">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="462c7-150">Le système DI est basé sur le système DI dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="462c7-150">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="462c7-151">Pour plus d'informations, consultez <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="462c7-151">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="462c7-152">Demander un service dans un composant</span><span class="sxs-lookup"><span data-stu-id="462c7-152">Request a service in a component</span></span>

<span data-ttu-id="462c7-153">Une fois les services ajoutés à la collection de services, injectez les services dans les composants à l’aide de la directive Razor [\@inject](xref:mvc/views/razor#inject) .</span><span class="sxs-lookup"><span data-stu-id="462c7-153">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="462c7-154">`@inject` a deux paramètres :</span><span class="sxs-lookup"><span data-stu-id="462c7-154">`@inject` has two parameters:</span></span>

* <span data-ttu-id="462c7-155">Tapez &ndash; pour le type de service à injecter.</span><span class="sxs-lookup"><span data-stu-id="462c7-155">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="462c7-156">Propriété &ndash; nom de la propriété qui reçoit le service d’application injecté.</span><span class="sxs-lookup"><span data-stu-id="462c7-156">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="462c7-157">La propriété ne nécessite pas de création manuelle.</span><span class="sxs-lookup"><span data-stu-id="462c7-157">The property doesn't require manual creation.</span></span> <span data-ttu-id="462c7-158">Le compilateur crée la propriété.</span><span class="sxs-lookup"><span data-stu-id="462c7-158">The compiler creates the property.</span></span>

<span data-ttu-id="462c7-159">Pour plus d'informations, consultez <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="462c7-159">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="462c7-160">Utilisez plusieurs instructions `@inject` pour injecter différents services.</span><span class="sxs-lookup"><span data-stu-id="462c7-160">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="462c7-161">L'exemple suivant montre comment utiliser `@inject`.</span><span class="sxs-lookup"><span data-stu-id="462c7-161">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="462c7-162">Le service qui implémente `Services.IDataAccess` est injecté dans la propriété du composant `DataRepository`.</span><span class="sxs-lookup"><span data-stu-id="462c7-162">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="462c7-163">Notez que le code utilise uniquement l’abstraction `IDataAccess` :</span><span class="sxs-lookup"><span data-stu-id="462c7-163">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="462c7-164">En interne, la propriété générée (`DataRepository`) est décorée avec l’attribut `InjectAttribute`.</span><span class="sxs-lookup"><span data-stu-id="462c7-164">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="462c7-165">En règle générale, cet attribut n’est pas utilisé directement.</span><span class="sxs-lookup"><span data-stu-id="462c7-165">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="462c7-166">Si une classe de base est requise pour les composants et les propriétés injectées sont également requises pour la classe de base, ajoutez manuellement le `InjectAttribute` :</span><span class="sxs-lookup"><span data-stu-id="462c7-166">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="462c7-167">Dans les composants dérivés de la classe de base, la directive `@inject` n’est pas obligatoire.</span><span class="sxs-lookup"><span data-stu-id="462c7-167">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="462c7-168">Le `InjectAttribute` de la classe de base est suffisant :</span><span class="sxs-lookup"><span data-stu-id="462c7-168">The `InjectAttribute` of the base class is sufficient:</span></span>

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="462c7-169">Utiliser DI dans les services</span><span class="sxs-lookup"><span data-stu-id="462c7-169">Use DI in services</span></span>

<span data-ttu-id="462c7-170">Les services complexes peuvent nécessiter des services supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="462c7-170">Complex services might require additional services.</span></span> <span data-ttu-id="462c7-171">Dans l’exemple précédent, `DataAccess` peut nécessiter le service par défaut `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="462c7-171">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="462c7-172">`@inject` (ou `InjectAttribute`) n’est pas disponible pour une utilisation dans les services.</span><span class="sxs-lookup"><span data-stu-id="462c7-172">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="462c7-173">L' *injection de constructeur* doit être utilisée à la place.</span><span class="sxs-lookup"><span data-stu-id="462c7-173">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="462c7-174">Les services requis sont ajoutés en ajoutant des paramètres au constructeur du service.</span><span class="sxs-lookup"><span data-stu-id="462c7-174">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="462c7-175">Lorsque DI crée le service, il reconnaît les services dont il a besoin dans le constructeur et les fournit en conséquence.</span><span class="sxs-lookup"><span data-stu-id="462c7-175">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="462c7-176">Conditions préalables pour l’injection de constructeur :</span><span class="sxs-lookup"><span data-stu-id="462c7-176">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="462c7-177">Un constructeur doit exister dont les arguments peuvent tous être remplis par DI.</span><span class="sxs-lookup"><span data-stu-id="462c7-177">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="462c7-178">Les paramètres supplémentaires non couverts par DI sont autorisés s’ils spécifient des valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="462c7-178">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="462c7-179">Le constructeur applicable doit être *public*.</span><span class="sxs-lookup"><span data-stu-id="462c7-179">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="462c7-180">Un constructeur applicable doit exister.</span><span class="sxs-lookup"><span data-stu-id="462c7-180">One applicable constructor must exist.</span></span> <span data-ttu-id="462c7-181">En cas d’ambiguïté, DI lève une exception.</span><span class="sxs-lookup"><span data-stu-id="462c7-181">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a><span data-ttu-id="462c7-182">Classes de composants de base de l’utilitaire pour gérer une étendue DI</span><span class="sxs-lookup"><span data-stu-id="462c7-182">Utility base component classes to manage a DI scope</span></span>

<span data-ttu-id="462c7-183">Dans ASP.NET Core applications, les services délimités sont généralement étendus à la requête actuelle.</span><span class="sxs-lookup"><span data-stu-id="462c7-183">In ASP.NET Core apps, scoped services are typically scoped to the current request.</span></span> <span data-ttu-id="462c7-184">Une fois la demande terminée, tous les services délimités ou temporaires sont supprimés par le système DI.</span><span class="sxs-lookup"><span data-stu-id="462c7-184">After the request completes, any scoped or transient services are disposed by the DI system.</span></span> <span data-ttu-id="462c7-185">Dans les applications Blazor Server, l’étendue de la demande est limitée à la durée de la connexion client, ce qui peut entraîner des services transitoires et de portée de vie bien plus longs que prévu.</span><span class="sxs-lookup"><span data-stu-id="462c7-185">In Blazor Server apps, the request scope lasts for the duration of the client connection, which can result in transient and scoped services living much longer than expected.</span></span>

<span data-ttu-id="462c7-186">Pour étendre les services à la durée de vie d’un composant, peut utiliser les classes de base `OwningComponentBase` et `OwningComponentBase<TService>`.</span><span class="sxs-lookup"><span data-stu-id="462c7-186">To scope services to the lifetime of a component, can use the `OwningComponentBase` and `OwningComponentBase<TService>` base classes.</span></span> <span data-ttu-id="462c7-187">Ces classes de base exposent une propriété `ScopedServices` de type `IServiceProvider` qui résout les services dont la portée est limitée à la durée de vie du composant.</span><span class="sxs-lookup"><span data-stu-id="462c7-187">These base classes expose a `ScopedServices` property of type `IServiceProvider` that resolve services that are scoped to the lifetime of the component.</span></span> <span data-ttu-id="462c7-188">Pour créer un composant qui hérite d’une classe de base dans Razor, utilisez la directive `@inherits`.</span><span class="sxs-lookup"><span data-stu-id="462c7-188">To author a component that inherits from a base class in Razor, use the `@inherits` directive.</span></span>

```cshtml
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
> <span data-ttu-id="462c7-189">Les services injectés dans le composant à l’aide de `@inject` ou le `InjectAttribute` ne sont pas créés dans l’étendue du composant et sont liés à l’étendue de la demande.</span><span class="sxs-lookup"><span data-stu-id="462c7-189">Services injected into the component using `@inject` or the `InjectAttribute` aren't created in the component's scope and are tied to the request scope.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="462c7-190">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="462c7-190">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
