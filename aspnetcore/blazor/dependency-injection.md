---
title: ASP.NET Core l’injection de dépendances éblouissantes
author: guardrex
description: Découvrez comment les applications éblouissantes peuvent injecter des services dans des composants.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/dependency-injection
ms.openlocfilehash: a2bfa0cbe951e817ed6264f1a151d5a716cd795c
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310354"
---
# <a name="aspnet-core-blazor-dependency-injection"></a><span data-ttu-id="d48f3-103">ASP.NET Core l’injection de dépendances éblouissantes</span><span class="sxs-lookup"><span data-stu-id="d48f3-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="d48f3-104">Par [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="d48f3-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="d48f3-105">Éblouissant prend en charge l' [injection de dépendances (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d48f3-105">Blazor supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d48f3-106">Les applications peuvent utiliser des services intégrés en les injectant dans des composants.</span><span class="sxs-lookup"><span data-stu-id="d48f3-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="d48f3-107">Les applications peuvent également définir et inscrire des services personnalisés et les rendre disponibles dans toute l’application via DI.</span><span class="sxs-lookup"><span data-stu-id="d48f3-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="d48f3-108">La méthode DI est une technique permettant d’accéder à des services configurés dans un emplacement central.</span><span class="sxs-lookup"><span data-stu-id="d48f3-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="d48f3-109">Cela peut être utile dans les applications éblouissantes pour :</span><span class="sxs-lookup"><span data-stu-id="d48f3-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="d48f3-110">Partager une seule instance d’une classe de service sur de nombreux composants, appelé service *Singleton* .</span><span class="sxs-lookup"><span data-stu-id="d48f3-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="d48f3-111">Découplez les composants des classes de service concrètes à l’aide d’abstractions de référence.</span><span class="sxs-lookup"><span data-stu-id="d48f3-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="d48f3-112">Par exemple, considérez une `IDataAccess` interface pour accéder aux données dans l’application.</span><span class="sxs-lookup"><span data-stu-id="d48f3-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="d48f3-113">L’interface est implémentée par une `DataAccess` classe concrète et inscrite en tant que service dans le conteneur de services de l’application.</span><span class="sxs-lookup"><span data-stu-id="d48f3-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="d48f3-114">Quand un composant utilise di pour recevoir une `IDataAccess` implémentation, le composant n’est pas couplé au type concret.</span><span class="sxs-lookup"><span data-stu-id="d48f3-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="d48f3-115">L’implémentation peut être permutée, peut-être pour une implémentation factice dans les tests unitaires.</span><span class="sxs-lookup"><span data-stu-id="d48f3-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="d48f3-116">Services par défaut</span><span class="sxs-lookup"><span data-stu-id="d48f3-116">Default services</span></span>

<span data-ttu-id="d48f3-117">Les services par défaut sont automatiquement ajoutés à la collection de services de l’application.</span><span class="sxs-lookup"><span data-stu-id="d48f3-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="d48f3-118">de diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="d48f3-118">Service</span></span> | <span data-ttu-id="d48f3-119">Durée de vie</span><span class="sxs-lookup"><span data-stu-id="d48f3-119">Lifetime</span></span> | <span data-ttu-id="d48f3-120">Description</span><span class="sxs-lookup"><span data-stu-id="d48f3-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="d48f3-121">Singleton</span><span class="sxs-lookup"><span data-stu-id="d48f3-121">Singleton</span></span> | <span data-ttu-id="d48f3-122">Fournit des méthodes pour envoyer des requêtes HTTP et recevoir des réponses HTTP d’une ressource identifiée par un URI.</span><span class="sxs-lookup"><span data-stu-id="d48f3-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span> <span data-ttu-id="d48f3-123">Notez que cette instance de `HttpClient` utilise le navigateur pour gérer le trafic HTTP en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="d48f3-123">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="d48f3-124">[Httpclient. BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) est automatiquement défini sur le préfixe URI de base de l’application.</span><span class="sxs-lookup"><span data-stu-id="d48f3-124">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="d48f3-125">Pour plus d'informations, consultez <xref:blazor/call-web-api>.</span><span class="sxs-lookup"><span data-stu-id="d48f3-125">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="d48f3-126">Singleton</span><span class="sxs-lookup"><span data-stu-id="d48f3-126">Singleton</span></span> | <span data-ttu-id="d48f3-127">Représente une instance d’un Runtime JavaScript dans laquelle les appels JavaScript sont distribués.</span><span class="sxs-lookup"><span data-stu-id="d48f3-127">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="d48f3-128">Pour plus d'informations, consultez <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="d48f3-128">For more information, see <xref:blazor/javascript-interop>.</span></span> |
| `NavigationManager` | <span data-ttu-id="d48f3-129">Singleton</span><span class="sxs-lookup"><span data-stu-id="d48f3-129">Singleton</span></span> | <span data-ttu-id="d48f3-130">Contient des assistances pour l’utilisation des URI et de l’état de navigation.</span><span class="sxs-lookup"><span data-stu-id="d48f3-130">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="d48f3-131">Pour plus d’informations, consultez [URI et assistance de l’état de navigation](xref:blazor/routing#uri-and-navigation-state-helpers).</span><span class="sxs-lookup"><span data-stu-id="d48f3-131">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="d48f3-132">Un fournisseur de services personnalisé ne fournit pas automatiquement les services par défaut indiqués dans le tableau.</span><span class="sxs-lookup"><span data-stu-id="d48f3-132">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="d48f3-133">Si vous utilisez un fournisseur de services personnalisé et que vous avez besoin de l’un des services répertoriés dans le tableau, ajoutez les services requis au nouveau fournisseur de services.</span><span class="sxs-lookup"><span data-stu-id="d48f3-133">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="d48f3-134">Ajouter des services à une application</span><span class="sxs-lookup"><span data-stu-id="d48f3-134">Add services to an app</span></span>

<span data-ttu-id="d48f3-135">Après avoir créé une nouvelle application, examinez la `Startup.ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="d48f3-135">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="d48f3-136">Une `ConfigureServices` <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>, qui est une liste d’objets descripteurs de service (), est passée à la méthode. <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection></span><span class="sxs-lookup"><span data-stu-id="d48f3-136">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="d48f3-137">Les services sont ajoutés en fournissant des descripteurs de service à la collection de services.</span><span class="sxs-lookup"><span data-stu-id="d48f3-137">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="d48f3-138">L’exemple suivant illustre le concept avec l' `IDataAccess` interface et son implémentation `DataAccess`concrète :</span><span class="sxs-lookup"><span data-stu-id="d48f3-138">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="d48f3-139">Les services peuvent être configurés avec les durées de vie indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="d48f3-139">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="d48f3-140">Durée de vie</span><span class="sxs-lookup"><span data-stu-id="d48f3-140">Lifetime</span></span> | <span data-ttu-id="d48f3-141">Description</span><span class="sxs-lookup"><span data-stu-id="d48f3-141">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | <span data-ttu-id="d48f3-142">Le côté client de éblouissant n’a actuellement pas de concept d’étendues DI.</span><span class="sxs-lookup"><span data-stu-id="d48f3-142">Blazor client-side doesn't currently have a concept of DI scopes.</span></span> <span data-ttu-id="d48f3-143">`Scoped`-les services inscrits se `Singleton` comportent comme des services.</span><span class="sxs-lookup"><span data-stu-id="d48f3-143">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="d48f3-144">Toutefois, le modèle d’hébergement côté serveur prend en `Scoped` charge la durée de vie.</span><span class="sxs-lookup"><span data-stu-id="d48f3-144">However, the server-side hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="d48f3-145">Dans un composant Razor, l’inscription d’un service étendu est limitée à la connexion.</span><span class="sxs-lookup"><span data-stu-id="d48f3-145">In a Razor component, a scoped service registration is scoped to the connection.</span></span> <span data-ttu-id="d48f3-146">Pour cette raison, il est préférable d’utiliser les services délimités pour les services qui doivent être étendus à l’utilisateur actuel, même si l’objectif actuel est d’exécuter côté client dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="d48f3-146">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="d48f3-147">DI crée une *seule instance* du service.</span><span class="sxs-lookup"><span data-stu-id="d48f3-147">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="d48f3-148">Tous les composants qui `Singleton` requièrent un service reçoivent une instance du même service.</span><span class="sxs-lookup"><span data-stu-id="d48f3-148">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="d48f3-149">Chaque fois qu’un composant obtient une instance d' `Transient` un service à partir du conteneur de service, il reçoit une *nouvelle instance* du service.</span><span class="sxs-lookup"><span data-stu-id="d48f3-149">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="d48f3-150">Le système DI est basé sur le système DI dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d48f3-150">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="d48f3-151">Pour plus d'informations, consultez <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="d48f3-151">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="d48f3-152">Demander un service dans un composant</span><span class="sxs-lookup"><span data-stu-id="d48f3-152">Request a service in a component</span></span>

<span data-ttu-id="d48f3-153">Une fois les services ajoutés à la collection de services, injectez les services dans les [ \@](xref:mvc/views/razor#inject) composants à l’aide de la directive Razor Inject.</span><span class="sxs-lookup"><span data-stu-id="d48f3-153">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="d48f3-154">`@inject`a deux paramètres :</span><span class="sxs-lookup"><span data-stu-id="d48f3-154">`@inject` has two parameters:</span></span>

* <span data-ttu-id="d48f3-155">Tapez &ndash; le type du service à injecter.</span><span class="sxs-lookup"><span data-stu-id="d48f3-155">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="d48f3-156">Propriété &ndash; nom de la propriété qui reçoit le service d’application injecté.</span><span class="sxs-lookup"><span data-stu-id="d48f3-156">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="d48f3-157">La propriété ne nécessite pas de création manuelle.</span><span class="sxs-lookup"><span data-stu-id="d48f3-157">The property doesn't require manual creation.</span></span> <span data-ttu-id="d48f3-158">Le compilateur crée la propriété.</span><span class="sxs-lookup"><span data-stu-id="d48f3-158">The compiler creates the property.</span></span>

<span data-ttu-id="d48f3-159">Pour plus d'informations, consultez <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="d48f3-159">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="d48f3-160">Utilisez plusieurs `@inject` instructions pour injecter différents services.</span><span class="sxs-lookup"><span data-stu-id="d48f3-160">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="d48f3-161">L'exemple suivant montre comment utiliser `@inject`.</span><span class="sxs-lookup"><span data-stu-id="d48f3-161">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="d48f3-162">Le service qui `Services.IDataAccess` implémente est injecté dans la propriété `DataRepository`du composant.</span><span class="sxs-lookup"><span data-stu-id="d48f3-162">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="d48f3-163">Notez la manière dont le code utilise uniquement `IDataAccess` l’abstraction :</span><span class="sxs-lookup"><span data-stu-id="d48f3-163">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="d48f3-164">En interne, la propriété générée (`DataRepository`) est décorée avec `InjectAttribute` l’attribut.</span><span class="sxs-lookup"><span data-stu-id="d48f3-164">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="d48f3-165">En règle générale, cet attribut n’est pas utilisé directement.</span><span class="sxs-lookup"><span data-stu-id="d48f3-165">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="d48f3-166">Si une classe de base est requise pour les composants et les propriétés injectées sont également requises pour la classe de base `InjectAttribute`, ajoutez manuellement le :</span><span class="sxs-lookup"><span data-stu-id="d48f3-166">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="d48f3-167">Dans les composants dérivés de la classe de `@inject` base, la directive n’est pas obligatoire.</span><span class="sxs-lookup"><span data-stu-id="d48f3-167">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="d48f3-168">Le `InjectAttribute` de la classe de base est suffisant :</span><span class="sxs-lookup"><span data-stu-id="d48f3-168">The `InjectAttribute` of the base class is sufficient:</span></span>

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="d48f3-169">Utiliser DI dans les services</span><span class="sxs-lookup"><span data-stu-id="d48f3-169">Use DI in services</span></span>

<span data-ttu-id="d48f3-170">Les services complexes peuvent nécessiter des services supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="d48f3-170">Complex services might require additional services.</span></span> <span data-ttu-id="d48f3-171">Dans l’exemple précédent, `DataAccess` peut nécessiter `HttpClient` le service par défaut.</span><span class="sxs-lookup"><span data-stu-id="d48f3-171">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="d48f3-172">`@inject`(ou) `InjectAttribute`n’est pas disponible pour une utilisation dans les services.</span><span class="sxs-lookup"><span data-stu-id="d48f3-172">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="d48f3-173">L' *injection de constructeur* doit être utilisée à la place.</span><span class="sxs-lookup"><span data-stu-id="d48f3-173">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="d48f3-174">Les services requis sont ajoutés en ajoutant des paramètres au constructeur du service.</span><span class="sxs-lookup"><span data-stu-id="d48f3-174">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="d48f3-175">Lorsque DI crée le service, il reconnaît les services dont il a besoin dans le constructeur et les fournit en conséquence.</span><span class="sxs-lookup"><span data-stu-id="d48f3-175">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="d48f3-176">Conditions préalables pour l’injection de constructeur :</span><span class="sxs-lookup"><span data-stu-id="d48f3-176">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="d48f3-177">Un constructeur doit exister dont les arguments peuvent tous être remplis par DI.</span><span class="sxs-lookup"><span data-stu-id="d48f3-177">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="d48f3-178">Les paramètres supplémentaires non couverts par DI sont autorisés s’ils spécifient des valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="d48f3-178">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="d48f3-179">Le constructeur applicable doit être *public*.</span><span class="sxs-lookup"><span data-stu-id="d48f3-179">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="d48f3-180">Un constructeur applicable doit exister.</span><span class="sxs-lookup"><span data-stu-id="d48f3-180">One applicable constructor must exist.</span></span> <span data-ttu-id="d48f3-181">En cas d’ambiguïté, DI lève une exception.</span><span class="sxs-lookup"><span data-stu-id="d48f3-181">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d48f3-182">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d48f3-182">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
