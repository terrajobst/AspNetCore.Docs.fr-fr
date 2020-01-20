---
title: ASP.NET Core Blazor l’injection de dépendances
author: guardrex
description: Découvrez comment Blazor applications peuvent injecter des services dans des composants.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/08/2020
no-loc:
- Blazor
- SignalR
uid: blazor/dependency-injection
ms.openlocfilehash: 6930d721f04fd5f7cad2ba472724497a157fda0f
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76159974"
---
# <a name="aspnet-core-opno-locblazor-dependency-injection"></a><span data-ttu-id="eb287-103">ASP.NET Core Blazor l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="eb287-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="eb287-104">Par [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="eb287-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="eb287-105"> prend en charge l' [injection de dépendances (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="eb287-105"> supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="eb287-106">Les applications peuvent utiliser des services intégrés en les injectant dans des composants.</span><span class="sxs-lookup"><span data-stu-id="eb287-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="eb287-107">Les applications peuvent également définir et inscrire des services personnalisés et les rendre disponibles dans toute l’application via DI.</span><span class="sxs-lookup"><span data-stu-id="eb287-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="eb287-108">La méthode DI est une technique permettant d’accéder à des services configurés dans un emplacement central.</span><span class="sxs-lookup"><span data-stu-id="eb287-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="eb287-109">Cela peut être utile dans Blazor applications pour :</span><span class="sxs-lookup"><span data-stu-id="eb287-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="eb287-110">Partager une seule instance d’une classe de service sur de nombreux composants, appelé service *Singleton* .</span><span class="sxs-lookup"><span data-stu-id="eb287-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="eb287-111">Découplez les composants des classes de service concrètes à l’aide d’abstractions de référence.</span><span class="sxs-lookup"><span data-stu-id="eb287-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="eb287-112">Par exemple, considérez une interface `IDataAccess` pour accéder aux données de l’application.</span><span class="sxs-lookup"><span data-stu-id="eb287-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="eb287-113">L’interface est implémentée par une classe `DataAccess` concrète et inscrite en tant que service dans le conteneur de services de l’application.</span><span class="sxs-lookup"><span data-stu-id="eb287-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="eb287-114">Lorsqu’un composant utilise DI pour recevoir une implémentation `IDataAccess`, le composant n’est pas couplé au type concret.</span><span class="sxs-lookup"><span data-stu-id="eb287-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="eb287-115">L’implémentation peut être permutée, peut-être pour une implémentation factice dans les tests unitaires.</span><span class="sxs-lookup"><span data-stu-id="eb287-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="eb287-116">Services par défaut</span><span class="sxs-lookup"><span data-stu-id="eb287-116">Default services</span></span>

<span data-ttu-id="eb287-117">Les services par défaut sont automatiquement ajoutés à la collection de services de l’application.</span><span class="sxs-lookup"><span data-stu-id="eb287-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="eb287-118">Service</span><span class="sxs-lookup"><span data-stu-id="eb287-118">Service</span></span> | <span data-ttu-id="eb287-119">Durée de vie</span><span class="sxs-lookup"><span data-stu-id="eb287-119">Lifetime</span></span> | <span data-ttu-id="eb287-120">Description</span><span class="sxs-lookup"><span data-stu-id="eb287-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="eb287-121">Singleton</span><span class="sxs-lookup"><span data-stu-id="eb287-121">Singleton</span></span> | <span data-ttu-id="eb287-122">Fournit des méthodes pour envoyer des requêtes HTTP et recevoir des réponses HTTP d’une ressource identifiée par un URI.</span><span class="sxs-lookup"><span data-stu-id="eb287-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span><br><br><span data-ttu-id="eb287-123">L’instance de `HttpClient` dans une application Blazor webassembly utilise le navigateur pour gérer le trafic HTTP en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="eb287-123">The instance of `HttpClient` in a Blazor WebAssembly app uses the browser for handling the HTTP traffic in the background.</span></span><br><br>Blazor<span data-ttu-id="eb287-124"> applications serveur n’incluent pas d' `HttpClient` configuré en tant que service par défaut.</span><span class="sxs-lookup"><span data-stu-id="eb287-124"> Server apps don't include an `HttpClient` configured as a service by default.</span></span> <span data-ttu-id="eb287-125">Fournissez une `HttpClient` à une application Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="eb287-125">Provide an `HttpClient` to a Blazor Server app.</span></span><br><br><span data-ttu-id="eb287-126">Pour plus d'informations, consultez <xref:blazor/call-web-api>.</span><span class="sxs-lookup"><span data-stu-id="eb287-126">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="eb287-127">Singleton (Blazor webassembly)</span><span class="sxs-lookup"><span data-stu-id="eb287-127">Singleton (Blazor WebAssembly)</span></span><br><span data-ttu-id="eb287-128">Étendue (serveurBlazor)</span><span class="sxs-lookup"><span data-stu-id="eb287-128">Scoped (Blazor Server)</span></span> | <span data-ttu-id="eb287-129">Représente une instance d’un Runtime JavaScript dans laquelle les appels JavaScript sont distribués.</span><span class="sxs-lookup"><span data-stu-id="eb287-129">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="eb287-130">Pour plus d'informations, consultez <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="eb287-130">For more information, see <xref:blazor/javascript-interop>.</span></span> |
| `NavigationManager` | <span data-ttu-id="eb287-131">Singleton (Blazor webassembly)</span><span class="sxs-lookup"><span data-stu-id="eb287-131">Singleton (Blazor WebAssembly)</span></span><br><span data-ttu-id="eb287-132">Étendue (serveurBlazor)</span><span class="sxs-lookup"><span data-stu-id="eb287-132">Scoped (Blazor Server)</span></span> | <span data-ttu-id="eb287-133">Contient des assistances pour l’utilisation des URI et de l’état de navigation.</span><span class="sxs-lookup"><span data-stu-id="eb287-133">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="eb287-134">Pour plus d’informations, consultez [URI et assistance de l’état de navigation](xref:blazor/routing#uri-and-navigation-state-helpers).</span><span class="sxs-lookup"><span data-stu-id="eb287-134">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="eb287-135">Un fournisseur de services personnalisé ne fournit pas automatiquement les services par défaut indiqués dans le tableau.</span><span class="sxs-lookup"><span data-stu-id="eb287-135">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="eb287-136">Si vous utilisez un fournisseur de services personnalisé et que vous avez besoin de l’un des services répertoriés dans le tableau, ajoutez les services requis au nouveau fournisseur de services.</span><span class="sxs-lookup"><span data-stu-id="eb287-136">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="eb287-137">Ajouter des services à une application</span><span class="sxs-lookup"><span data-stu-id="eb287-137">Add services to an app</span></span>

<span data-ttu-id="eb287-138">Après avoir créé une nouvelle application, examinez la méthode `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="eb287-138">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="eb287-139">Une <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, qui est une liste d’objets de descripteur de service (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>), est passée à la méthode `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="eb287-139">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="eb287-140">Les services sont ajoutés en fournissant des descripteurs de service à la collection de services.</span><span class="sxs-lookup"><span data-stu-id="eb287-140">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="eb287-141">L’exemple suivant illustre le concept avec l’interface `IDataAccess` et son implémentation concrète `DataAccess`:</span><span class="sxs-lookup"><span data-stu-id="eb287-141">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="eb287-142">Les services peuvent être configurés avec les durées de vie indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="eb287-142">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="eb287-143">Durée de vie</span><span class="sxs-lookup"><span data-stu-id="eb287-143">Lifetime</span></span> | <span data-ttu-id="eb287-144">Description</span><span class="sxs-lookup"><span data-stu-id="eb287-144">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor<span data-ttu-id="eb287-145"> applications webassembly n’ont pas actuellement de concept d’étendues DI.</span><span class="sxs-lookup"><span data-stu-id="eb287-145"> WebAssembly apps don't currently have a concept of DI scopes.</span></span> <span data-ttu-id="eb287-146">les services inscrits au `Scoped`se comportent comme des services `Singleton`.</span><span class="sxs-lookup"><span data-stu-id="eb287-146">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="eb287-147">Toutefois, le modèle d’hébergement de serveur Blazor prend en charge la durée de vie `Scoped`.</span><span class="sxs-lookup"><span data-stu-id="eb287-147">However, the Blazor Server hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="eb287-148">Dans les applications Blazor Server, l’inscription d’un service étendu est limitée à la *connexion*.</span><span class="sxs-lookup"><span data-stu-id="eb287-148">In Blazor Server apps, a scoped service registration is scoped to the *connection*.</span></span> <span data-ttu-id="eb287-149">Pour cette raison, il est préférable d’utiliser les services délimités pour les services qui doivent être étendus à l’utilisateur actuel, même si l’objectif actuel est d’exécuter côté client dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="eb287-149">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="eb287-150">DI crée une *seule instance* du service.</span><span class="sxs-lookup"><span data-stu-id="eb287-150">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="eb287-151">Tous les composants qui requièrent un service `Singleton` reçoivent une instance du même service.</span><span class="sxs-lookup"><span data-stu-id="eb287-151">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="eb287-152">Chaque fois qu’un composant obtient une instance d’un service `Transient` à partir du conteneur de service, il reçoit une *nouvelle instance* du service.</span><span class="sxs-lookup"><span data-stu-id="eb287-152">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="eb287-153">Le système DI est basé sur le système DI dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eb287-153">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="eb287-154">Pour plus d'informations, consultez <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="eb287-154">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="eb287-155">Demander un service dans un composant</span><span class="sxs-lookup"><span data-stu-id="eb287-155">Request a service in a component</span></span>

<span data-ttu-id="eb287-156">Une fois les services ajoutés à la collection de services, injectez les services dans les composants à l’aide de l' [\@injecter](xref:mvc/views/razor#inject) la directive Razor.</span><span class="sxs-lookup"><span data-stu-id="eb287-156">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="eb287-157">`@inject` a deux paramètres :</span><span class="sxs-lookup"><span data-stu-id="eb287-157">`@inject` has two parameters:</span></span>

* <span data-ttu-id="eb287-158">Tapez &ndash; le type du service à injecter.</span><span class="sxs-lookup"><span data-stu-id="eb287-158">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="eb287-159">Propriété &ndash; le nom de la propriété qui reçoit le service d’application injecté.</span><span class="sxs-lookup"><span data-stu-id="eb287-159">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="eb287-160">La propriété ne nécessite pas de création manuelle.</span><span class="sxs-lookup"><span data-stu-id="eb287-160">The property doesn't require manual creation.</span></span> <span data-ttu-id="eb287-161">Le compilateur crée la propriété.</span><span class="sxs-lookup"><span data-stu-id="eb287-161">The compiler creates the property.</span></span>

<span data-ttu-id="eb287-162">Pour plus d'informations, consultez <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="eb287-162">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="eb287-163">Utilisez plusieurs instructions `@inject` pour injecter différents services.</span><span class="sxs-lookup"><span data-stu-id="eb287-163">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="eb287-164">L'exemple suivant montre comment utiliser `@inject`.</span><span class="sxs-lookup"><span data-stu-id="eb287-164">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="eb287-165">Le service qui implémente `Services.IDataAccess` est injecté dans la `DataRepository`de propriété du composant.</span><span class="sxs-lookup"><span data-stu-id="eb287-165">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="eb287-166">Notez que le code utilise uniquement l’abstraction `IDataAccess` :</span><span class="sxs-lookup"><span data-stu-id="eb287-166">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-razor[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="eb287-167">En interne, la propriété générée (`DataRepository`) utilise l’attribut `InjectAttribute`.</span><span class="sxs-lookup"><span data-stu-id="eb287-167">Internally, the generated property (`DataRepository`) uses the `InjectAttribute` attribute.</span></span> <span data-ttu-id="eb287-168">En règle générale, cet attribut n’est pas utilisé directement.</span><span class="sxs-lookup"><span data-stu-id="eb287-168">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="eb287-169">Si une classe de base est requise pour les composants et les propriétés injectées sont également requises pour la classe de base, ajoutez manuellement l' `InjectAttribute`:</span><span class="sxs-lookup"><span data-stu-id="eb287-169">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="eb287-170">Dans les composants dérivés de la classe de base, la directive `@inject` n’est pas obligatoire.</span><span class="sxs-lookup"><span data-stu-id="eb287-170">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="eb287-171">Le `InjectAttribute` de la classe de base est suffisant :</span><span class="sxs-lookup"><span data-stu-id="eb287-171">The `InjectAttribute` of the base class is sufficient:</span></span>

```razor
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="eb287-172">Utiliser DI dans les services</span><span class="sxs-lookup"><span data-stu-id="eb287-172">Use DI in services</span></span>

<span data-ttu-id="eb287-173">Les services complexes peuvent nécessiter des services supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="eb287-173">Complex services might require additional services.</span></span> <span data-ttu-id="eb287-174">Dans l’exemple précédent, `DataAccess` peut nécessiter le service `HttpClient` par défaut.</span><span class="sxs-lookup"><span data-stu-id="eb287-174">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="eb287-175">`@inject` (ou le `InjectAttribute`) ne peut pas être utilisé dans les services.</span><span class="sxs-lookup"><span data-stu-id="eb287-175">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="eb287-176">L' *injection de constructeur* doit être utilisée à la place.</span><span class="sxs-lookup"><span data-stu-id="eb287-176">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="eb287-177">Les services requis sont ajoutés en ajoutant des paramètres au constructeur du service.</span><span class="sxs-lookup"><span data-stu-id="eb287-177">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="eb287-178">Lorsque DI crée le service, il reconnaît les services dont il a besoin dans le constructeur et les fournit en conséquence.</span><span class="sxs-lookup"><span data-stu-id="eb287-178">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="eb287-179">Conditions préalables pour l’injection de constructeur :</span><span class="sxs-lookup"><span data-stu-id="eb287-179">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="eb287-180">Un constructeur doit exister dont les arguments peuvent tous être remplis par DI.</span><span class="sxs-lookup"><span data-stu-id="eb287-180">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="eb287-181">Les paramètres supplémentaires non couverts par DI sont autorisés s’ils spécifient des valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="eb287-181">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="eb287-182">Le constructeur applicable doit être *public*.</span><span class="sxs-lookup"><span data-stu-id="eb287-182">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="eb287-183">Un constructeur applicable doit exister.</span><span class="sxs-lookup"><span data-stu-id="eb287-183">One applicable constructor must exist.</span></span> <span data-ttu-id="eb287-184">En cas d’ambiguïté, DI lève une exception.</span><span class="sxs-lookup"><span data-stu-id="eb287-184">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a><span data-ttu-id="eb287-185">Classes de composants de base de l’utilitaire pour gérer une étendue DI</span><span class="sxs-lookup"><span data-stu-id="eb287-185">Utility base component classes to manage a DI scope</span></span>

<span data-ttu-id="eb287-186">Dans ASP.NET Core applications, les services délimités sont généralement étendus à la requête actuelle.</span><span class="sxs-lookup"><span data-stu-id="eb287-186">In ASP.NET Core apps, scoped services are typically scoped to the current request.</span></span> <span data-ttu-id="eb287-187">Une fois la demande terminée, tous les services délimités ou temporaires sont supprimés par le système DI.</span><span class="sxs-lookup"><span data-stu-id="eb287-187">After the request completes, any scoped or transient services are disposed by the DI system.</span></span> <span data-ttu-id="eb287-188">Dans les applications Blazor Server, l’étendue de la demande est limitée à la durée de la connexion client, ce qui peut entraîner des services transitoires et de portée de vie bien plus longs que prévu.</span><span class="sxs-lookup"><span data-stu-id="eb287-188">In Blazor Server apps, the request scope lasts for the duration of the client connection, which can result in transient and scoped services living much longer than expected.</span></span>

<span data-ttu-id="eb287-189">Pour étendre les services à la durée de vie d’un composant, peut utiliser les classes de base `OwningComponentBase` et `OwningComponentBase<TService>`.</span><span class="sxs-lookup"><span data-stu-id="eb287-189">To scope services to the lifetime of a component, can use the `OwningComponentBase` and `OwningComponentBase<TService>` base classes.</span></span> <span data-ttu-id="eb287-190">Ces classes de base exposent une propriété `ScopedServices` de type `IServiceProvider` qui résolvent les services dont la portée est limitée à la durée de vie du composant.</span><span class="sxs-lookup"><span data-stu-id="eb287-190">These base classes expose a `ScopedServices` property of type `IServiceProvider` that resolve services that are scoped to the lifetime of the component.</span></span> <span data-ttu-id="eb287-191">Pour créer un composant qui hérite d’une classe de base dans Razor, utilisez la directive `@inherits`.</span><span class="sxs-lookup"><span data-stu-id="eb287-191">To author a component that inherits from a base class in Razor, use the `@inherits` directive.</span></span>

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
> <span data-ttu-id="eb287-192">Les services injectés dans le composant à l’aide de `@inject` ou le `InjectAttribute` ne sont pas créés dans l’étendue du composant et sont liés à l’étendue de la demande.</span><span class="sxs-lookup"><span data-stu-id="eb287-192">Services injected into the component using `@inject` or the `InjectAttribute` aren't created in the component's scope and are tied to the request scope.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="eb287-193">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="eb287-193">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
