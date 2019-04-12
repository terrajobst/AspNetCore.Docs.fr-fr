---
title: Injection de dépendances de composants de Razor
author: guardrex
description: Découvrez comment les applications Blazor et composants de Razor peuvent utiliser des services en les insérant dans les composants.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
uid: razor-components/dependency-injection
ms.openlocfilehash: 40aec2e3a5032039c7d921f67d7d333b03c07fb1
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2019
ms.locfileid: "59515487"
---
# <a name="razor-components-dependency-injection"></a><span data-ttu-id="60c9c-103">Injection de dépendances de composants de Razor</span><span class="sxs-lookup"><span data-stu-id="60c9c-103">Razor Components dependency injection</span></span>

<span data-ttu-id="60c9c-104">Par [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="60c9c-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="60c9c-105">Prend en charge les composants de Razor [l’injection de dépendance (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="60c9c-105">Razor Components supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="60c9c-106">Applications peuvent utiliser les services intégrés en les insérant dans les composants.</span><span class="sxs-lookup"><span data-stu-id="60c9c-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="60c9c-107">Les applications peuvent également définir et inscrire les services personnalisés et les rendre disponibles dans toute l’application via l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="60c9c-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="60c9c-108">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="60c9c-108">Dependency injection</span></span>

<span data-ttu-id="60c9c-109">L’injection de dépendances est une technique permettant d’accéder aux services configurés dans un emplacement central.</span><span class="sxs-lookup"><span data-stu-id="60c9c-109">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="60c9c-110">Il se peut que cela peut être utile dans les applications de composants de Razor :</span><span class="sxs-lookup"><span data-stu-id="60c9c-110">This can be useful in Razor Components apps to:</span></span>

* <span data-ttu-id="60c9c-111">Partager une seule instance d’une classe de service entre de nombreux composants, appelés un *singleton* service.</span><span class="sxs-lookup"><span data-stu-id="60c9c-111">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="60c9c-112">Découpler les composants à partir de classes de service concret à l’aide des abstractions de référence.</span><span class="sxs-lookup"><span data-stu-id="60c9c-112">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="60c9c-113">Par exemple, considérez une interface `IDataAccess` pour accéder aux données dans l’application.</span><span class="sxs-lookup"><span data-stu-id="60c9c-113">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="60c9c-114">L’interface est implémentée par un concret `DataAccess` classe et l’inscrit en tant que service dans le conteneur de service de l’application.</span><span class="sxs-lookup"><span data-stu-id="60c9c-114">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="60c9c-115">Quand un composant utilise l’injection de dépendances pour recevoir un `IDataAccess` implémentation, le composant n’est pas couplée avec le type concret.</span><span class="sxs-lookup"><span data-stu-id="60c9c-115">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="60c9c-116">L’implémentation peut être échangée, peut-être pour une implémentation factice dans les tests unitaires.</span><span class="sxs-lookup"><span data-stu-id="60c9c-116">The implementation can be swapped, perhaps to a mock implementation in unit tests.</span></span>

<span data-ttu-id="60c9c-117">Pour plus d'informations, consultez <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="60c9c-117">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="add-services-to-di"></a><span data-ttu-id="60c9c-118">Ajouter des services à l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="60c9c-118">Add services to DI</span></span>

<span data-ttu-id="60c9c-119">Après avoir créé une nouvelle application, examinez le `Startup.ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="60c9c-119">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="60c9c-120">Le `ConfigureServices` est transmis à la méthode un <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, qui est une liste d’objets de descripteurs de service (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span><span class="sxs-lookup"><span data-stu-id="60c9c-120">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="60c9c-121">Services sont ajoutés en fournissant des descripteurs de service à la collection de service.</span><span class="sxs-lookup"><span data-stu-id="60c9c-121">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="60c9c-122">L’exemple suivant illustre le concept avec la `IDataAccess` interface et son implémentation concrète `DataAccess`:</span><span class="sxs-lookup"><span data-stu-id="60c9c-122">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="60c9c-123">Services peuvent être configurés avec les durées de vie indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="60c9c-123">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="60c9c-124">Durée de vie</span><span class="sxs-lookup"><span data-stu-id="60c9c-124">Lifetime</span></span> | <span data-ttu-id="60c9c-125">Description</span><span class="sxs-lookup"><span data-stu-id="60c9c-125">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="60c9c-126">L’injection de dépendances crée un *SIS* du service.</span><span class="sxs-lookup"><span data-stu-id="60c9c-126">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="60c9c-127">Tous les composants nécessitant un `Singleton` service reçoivent une instance du même service.</span><span class="sxs-lookup"><span data-stu-id="60c9c-127">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="60c9c-128">Chaque fois qu’un composant obtient une instance d’un `Transient` service à partir du conteneur de service, il reçoit un *nouvelle instance* du service.</span><span class="sxs-lookup"><span data-stu-id="60c9c-128">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | <span data-ttu-id="60c9c-129">Blazor du côté client n’a actuellement le concept d’étendues de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="60c9c-129">Client-side Blazor doesn't currently have the concept of DI scopes.</span></span> <span data-ttu-id="60c9c-130">`Scoped` se comporte comme `Singleton`.</span><span class="sxs-lookup"><span data-stu-id="60c9c-130">`Scoped` behaves like `Singleton`.</span></span> <span data-ttu-id="60c9c-131">Toutefois, les composants ASP.NET Core Razor prend en charge la `Scoped` durée de vie.</span><span class="sxs-lookup"><span data-stu-id="60c9c-131">However, ASP.NET Core Razor Components support the `Scoped` lifetime.</span></span> <span data-ttu-id="60c9c-132">Dans un composant Razor, une inscription de service délimité est limitée à la connexion.</span><span class="sxs-lookup"><span data-stu-id="60c9c-132">In a Razor Component, a scoped service registration is scoped to the connection.</span></span> <span data-ttu-id="60c9c-133">Pour cette raison, l’utilisation de services délimités est préféré pour les services qui doivent être étendus à l’utilisateur actuel, même si l’objectif actuel est de s’exécuter côté client dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="60c9c-133">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |

<span data-ttu-id="60c9c-134">Le système de l’injection de dépendances est basé sur le système de l’injection de dépendances dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="60c9c-134">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="60c9c-135">Pour plus d'informations, consultez <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="60c9c-135">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="default-services"></a><span data-ttu-id="60c9c-136">Services par défaut</span><span class="sxs-lookup"><span data-stu-id="60c9c-136">Default services</span></span>

<span data-ttu-id="60c9c-137">Services par défaut sont automatiquement ajoutés à la collection de service de l’application.</span><span class="sxs-lookup"><span data-stu-id="60c9c-137">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="60c9c-138">Service</span><span class="sxs-lookup"><span data-stu-id="60c9c-138">Service</span></span> | <span data-ttu-id="60c9c-139">Description</span><span class="sxs-lookup"><span data-stu-id="60c9c-139">Description</span></span> |
| ------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="60c9c-140">Fournit des méthodes pour envoyer des requêtes HTTP et recevoir des réponses HTTP à partir d’une ressource identifiée par un URI (singleton).</span><span class="sxs-lookup"><span data-stu-id="60c9c-140">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI (singleton).</span></span> <span data-ttu-id="60c9c-141">Notez que cette instance de `HttpClient` utilise le navigateur pour gérer le trafic HTTP en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="60c9c-141">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="60c9c-142">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) est automatiquement défini sur le préfixe URI de base de l’application.</span><span class="sxs-lookup"><span data-stu-id="60c9c-142">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="60c9c-143">`HttpClient` est fournie uniquement pour les applications Blazor côté client.</span><span class="sxs-lookup"><span data-stu-id="60c9c-143">`HttpClient` is only provided to client-side Blazor apps.</span></span> |
| `IJSRuntime` | <span data-ttu-id="60c9c-144">Représente une instance d’un runtime JavaScript à laquelle les appels peuvent être distribués.</span><span class="sxs-lookup"><span data-stu-id="60c9c-144">Represents an instance of a JavaScript runtime to which calls may be dispatched.</span></span> <span data-ttu-id="60c9c-145">Pour plus d'informations, consultez <xref:razor-components/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="60c9c-145">For more information, see <xref:razor-components/javascript-interop>.</span></span> |
| `IUriHelper` | <span data-ttu-id="60c9c-146">Contient les programmes d’assistance pour travailler avec l’état de l’URI et de navigation (singleton).</span><span class="sxs-lookup"><span data-stu-id="60c9c-146">Contains helpers for working with URIs and navigation state (singleton).</span></span> <span data-ttu-id="60c9c-147">`IUriHelper` est fourni pour les applications Blazor et composants de Razor.</span><span class="sxs-lookup"><span data-stu-id="60c9c-147">`IUriHelper` is provided to both Blazor and Razor Components apps.</span></span> |

<span data-ttu-id="60c9c-148">Il est possible d’utiliser un fournisseur de services personnalisé au lieu du fournisseur de service par défaut ajouté par le modèle par défaut.</span><span class="sxs-lookup"><span data-stu-id="60c9c-148">It's possible to use a custom service provider instead of the default service provider added by the default template.</span></span> <span data-ttu-id="60c9c-149">Un fournisseur de services personnalisée ne fournit pas automatiquement les services par défaut répertoriées dans le tableau.</span><span class="sxs-lookup"><span data-stu-id="60c9c-149">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="60c9c-150">Si vous utilisez un fournisseur de services personnalisé et que vous avez besoin tout service indiqué dans le tableau, ajouter les services requis pour le nouveau fournisseur de services.</span><span class="sxs-lookup"><span data-stu-id="60c9c-150">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="60c9c-151">Demandez un service dans un composant</span><span class="sxs-lookup"><span data-stu-id="60c9c-151">Request a service in a component</span></span>

<span data-ttu-id="60c9c-152">Une fois que les services sont ajoutés à la collection de service, injecter les services de modèles Razor de composants à l’aide de la [ @inject ](xref:mvc/views/razor#section-4) directive Razor.</span><span class="sxs-lookup"><span data-stu-id="60c9c-152">After services are added to the service collection, inject the services into the components' Razor templates using the [@inject](xref:mvc/views/razor#section-4) Razor directive.</span></span> <span data-ttu-id="60c9c-153">`@inject` a deux paramètres :</span><span class="sxs-lookup"><span data-stu-id="60c9c-153">`@inject` has two parameters:</span></span>

* <span data-ttu-id="60c9c-154">Nom du type : Le type du service à injecter.</span><span class="sxs-lookup"><span data-stu-id="60c9c-154">Type name: The type of the service to inject.</span></span>
* <span data-ttu-id="60c9c-155">Nom de la propriété : Le nom de la propriété réception injecté app service.</span><span class="sxs-lookup"><span data-stu-id="60c9c-155">Property name: The name of the property receiving the injected app service.</span></span> <span data-ttu-id="60c9c-156">Notez que la propriété ne nécessite pas la création manuelle.</span><span class="sxs-lookup"><span data-stu-id="60c9c-156">Note that the property doesn't require manual creation.</span></span> <span data-ttu-id="60c9c-157">Le compilateur crée la propriété.</span><span class="sxs-lookup"><span data-stu-id="60c9c-157">The compiler creates the property.</span></span>

<span data-ttu-id="60c9c-158">Pour plus d'informations, consultez <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="60c9c-158">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="60c9c-159">Utilisez plusieurs `@inject` instructions pour injecter des différents services.</span><span class="sxs-lookup"><span data-stu-id="60c9c-159">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="60c9c-160">L'exemple suivant montre comment utiliser `@inject`.</span><span class="sxs-lookup"><span data-stu-id="60c9c-160">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="60c9c-161">Le service qui implémente `Services.IDataAccess` est injecté dans la propriété du composant `DataRepository`.</span><span class="sxs-lookup"><span data-stu-id="60c9c-161">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="60c9c-162">Notez comment le code est uniquement à l’aide de la `IDataAccess` abstraction :</span><span class="sxs-lookup"><span data-stu-id="60c9c-162">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.cshtml?highlight=2-3,23)]

<span data-ttu-id="60c9c-163">En interne, la propriété générée (`DataRepository`) est décorée avec le `InjectAttribute` attribut.</span><span class="sxs-lookup"><span data-stu-id="60c9c-163">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="60c9c-164">En règle générale, cet attribut n’est pas utilisé directement.</span><span class="sxs-lookup"><span data-stu-id="60c9c-164">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="60c9c-165">Si une classe de base est requise pour les composants et propriétés injectées sont également requises pour la classe de base `InjectAttribute` peuvent être ajoutées manuellement :</span><span class="sxs-lookup"><span data-stu-id="60c9c-165">If a base class is required for components and injected properties are also required for the base class, `InjectAttribute` can be manually added:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // Dependency injection works even if using the
    // InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="60c9c-166">Dans les composants dérivés de la classe de base, le `@inject` directive n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="60c9c-166">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="60c9c-167">Le `InjectAttribute` de la classe de base est suffisante :</span><span class="sxs-lookup"><span data-stu-id="60c9c-167">The `InjectAttribute` of the base class is sufficient:</span></span>

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="dependency-injection-in-services"></a><span data-ttu-id="60c9c-168">Injection de dépendances dans les services</span><span class="sxs-lookup"><span data-stu-id="60c9c-168">Dependency injection in services</span></span>

<span data-ttu-id="60c9c-169">Services complexes peuvent nécessiter des services supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="60c9c-169">Complex services might require additional services.</span></span> <span data-ttu-id="60c9c-170">Dans l’exemple précédent, `DataAccess` peut nécessiter la `HttpClient` service par défaut.</span><span class="sxs-lookup"><span data-stu-id="60c9c-170">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="60c9c-171">`@inject` (ou `InjectAttribute`) n’est pas disponible pour une utilisation dans les services.</span><span class="sxs-lookup"><span data-stu-id="60c9c-171">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="60c9c-172">*L’injection de constructeur* doit être utilisé à la place.</span><span class="sxs-lookup"><span data-stu-id="60c9c-172">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="60c9c-173">Les services requis sont ajoutés en ajoutant des paramètres pour le constructeur du service.</span><span class="sxs-lookup"><span data-stu-id="60c9c-173">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="60c9c-174">Lors de l’injection de dépendances crée le service, il reconnaît les services, il requiert dans le constructeur et les fournit en conséquence.</span><span class="sxs-lookup"><span data-stu-id="60c9c-174">When dependency injection creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="60c9c-175">Conditions préalables pour l’injection de constructeur :</span><span class="sxs-lookup"><span data-stu-id="60c9c-175">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="60c9c-176">Il doit y avoir un constructeur dont les arguments peuvent tous être satisfaits par l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="60c9c-176">There must be one constructor whose arguments can all be fulfilled by dependency injection.</span></span> <span data-ttu-id="60c9c-177">Notez que les paramètres supplémentaires non couvertes par l’injection de dépendances sont autorisées si elles spécifient des valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="60c9c-177">Note that additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="60c9c-178">Le constructeur applicable doit être *public*.</span><span class="sxs-lookup"><span data-stu-id="60c9c-178">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="60c9c-179">Il ne doit exister qu’un seul constructeur applicable.</span><span class="sxs-lookup"><span data-stu-id="60c9c-179">There must only be one applicable constructor.</span></span> <span data-ttu-id="60c9c-180">Dans le cas d’une ambiguïté, l’injection de dépendances lève une exception.</span><span class="sxs-lookup"><span data-stu-id="60c9c-180">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="60c9c-181">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="60c9c-181">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
