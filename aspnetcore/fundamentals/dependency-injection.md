---
title: Injection de dépendances dans ASP.NET Core
author: guardrex
description: Découvrez comment ASP.NET Core implémente l’injection de dépendances et comment l’utiliser.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: fundamentals/dependency-injection
ms.openlocfilehash: 8312f3375296a8530ac2db3db46d062b7b9e76b9
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750602"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="dc95f-103">Injection de dépendances dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dc95f-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="dc95f-104">Par [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="dc95f-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="dc95f-105">ASP.NET Core prend en charge le modèle de conception de logiciel avec injection de dépendances (DI), une technique permettant d’obtenir une [inversion de contrôle (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) entre les classes et leurs dépendances.</span><span class="sxs-lookup"><span data-stu-id="dc95f-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="dc95f-106">Pour plus d’informations spécifiques à l’injection de dépendances au sein des contrôleurs MVC, consultez <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="dc95f-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="dc95f-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dc95f-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="dc95f-108">Vue d’ensemble de l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="dc95f-108">Overview of dependency injection</span></span>

<span data-ttu-id="dc95f-109">Une *dépendance* est un objet qui nécessite un autre objet.</span><span class="sxs-lookup"><span data-stu-id="dc95f-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="dc95f-110">Examinez la classe `MyDependency` suivante avec une méthode `WriteMessage` dont dépendent d’autres classes dans une application :</span><span class="sxs-lookup"><span data-stu-id="dc95f-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

```csharp
public class MyDependency
{
    public MyDependency()
    {
    }

    public Task WriteMessage(string message)
    {
        Console.WriteLine(
            $"MyDependency.WriteMessage called. Message: {message}");

        return Task.FromResult(0);
    }
}
```

<span data-ttu-id="dc95f-111">Une instance de la classe `MyDependency` peut être créée pour rendre la méthode `WriteMessage` disponible pour une classe.</span><span class="sxs-lookup"><span data-stu-id="dc95f-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="dc95f-112">La classe `MyDependency` est une dépendance de la classe `IndexModel` :</span><span class="sxs-lookup"><span data-stu-id="dc95f-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

```csharp
public class IndexModel : PageModel
{
    MyDependency _dependency = new MyDependency();

    public async Task OnGetAsync()
    {
        await _dependency.WriteMessage(
            "IndexModel.OnGetAsync created this message.");
    }
}
```

<span data-ttu-id="dc95f-113">La classe est créee et dépend directement de l’instance `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="dc95f-113">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="dc95f-114">Les dépendances de code (comme l’exemple précédent) posent problème et doivent être évitées pour les raisons suivantes :</span><span class="sxs-lookup"><span data-stu-id="dc95f-114">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="dc95f-115">Pour remplacer `MyDependency` par une autre implémentation, la classe doit être modifiée.</span><span class="sxs-lookup"><span data-stu-id="dc95f-115">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="dc95f-116">Si `MyDependency` possède des dépendances, elles doivent être configurées par la classe.</span><span class="sxs-lookup"><span data-stu-id="dc95f-116">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="dc95f-117">Dans un grand projet comportant plusieurs classes dépendant de `MyDependency`, le code de configuration est disséminé dans l’application.</span><span class="sxs-lookup"><span data-stu-id="dc95f-117">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="dc95f-118">Cette implémentation complique le test unitaire.</span><span class="sxs-lookup"><span data-stu-id="dc95f-118">This implementation is difficult to unit test.</span></span> <span data-ttu-id="dc95f-119">L’application doit utiliser une classe `MyDependency` fictive ou stub, ce qui est impossible avec cette approche.</span><span class="sxs-lookup"><span data-stu-id="dc95f-119">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="dc95f-120">L’injection de dépendances résout ces problèmes via :</span><span class="sxs-lookup"><span data-stu-id="dc95f-120">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="dc95f-121">L’utilisation d’une interface pour abstraire l’implémentation des dépendances.</span><span class="sxs-lookup"><span data-stu-id="dc95f-121">The use of an interface to abstract the dependency implementation.</span></span>
* <span data-ttu-id="dc95f-122">L’inscription de la dépendance dans un conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="dc95f-122">Registration of the dependency in a service container.</span></span> <span data-ttu-id="dc95f-123">ASP.NET Core fournit un conteneur de service intégré, [IServiceProvider](/dotnet/api/system.iserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="dc95f-123">ASP.NET Core provides a built-in service container, [IServiceProvider](/dotnet/api/system.iserviceprovider).</span></span> <span data-ttu-id="dc95f-124">Les services sont inscrits dans la méthode `Startup.ConfigureServices` de l’application.</span><span class="sxs-lookup"><span data-stu-id="dc95f-124">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="dc95f-125">*Injection* du service dans le constructeur de la classe où il est utilisé.</span><span class="sxs-lookup"><span data-stu-id="dc95f-125">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="dc95f-126">Le framework prend la responsabilité de la création d’une instance de la dépendance et de sa suppression lorsqu’elle n’est plus nécessaire.</span><span class="sxs-lookup"><span data-stu-id="dc95f-126">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="dc95f-127">Dans l’[exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), l’interface `IMyDependency` définit une méthode que le service fournit à l’application :</span><span class="sxs-lookup"><span data-stu-id="dc95f-127">In the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

<span data-ttu-id="dc95f-128">Cette interface est implémentée par un type concret, `MyDependency` :</span><span class="sxs-lookup"><span data-stu-id="dc95f-128">This interface is implemented by a concrete type, `MyDependency`:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

<span data-ttu-id="dc95f-129">`MyDependency` demande un [ILogger&lt;TCategoryName&gt; ](/dotnet/api/microsoft.extensions.logging.ilogger-1) dans son constructeur.</span><span class="sxs-lookup"><span data-stu-id="dc95f-129">`MyDependency` requests an [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) in its constructor.</span></span> <span data-ttu-id="dc95f-130">Il n’est pas rare que l’injection de dépendances soit utilisée de manière chaînée.</span><span class="sxs-lookup"><span data-stu-id="dc95f-130">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="dc95f-131">Dans ce cas, chaque dépendance demandée demande à son tour ses propres dépendances.</span><span class="sxs-lookup"><span data-stu-id="dc95f-131">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="dc95f-132">Le conteneur résout les dépendances dans le graphique et retourne le service entièrement résolu.</span><span class="sxs-lookup"><span data-stu-id="dc95f-132">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="dc95f-133">L’ensemble collectif de dépendances qui doivent être résolues est généralement appelé *arborescence des dépendances*, *graphique de dépendance* ou *graphique d’objet*.</span><span class="sxs-lookup"><span data-stu-id="dc95f-133">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="dc95f-134">`IMyDependency` et `ILogger<TCategoryName>` doivent être inscrits dans le conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="dc95f-134">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="dc95f-135">`IMyDependency` est inscrit dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="dc95f-135">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="dc95f-136">`ILogger<TCategoryName>` est inscrit par l’infrastructure d’abstractions de journalisation. Il s’agit donc d’un [service fourni par le framework](#framework-provided-services) et inscrit par défaut par l’infrastructure.</span><span class="sxs-lookup"><span data-stu-id="dc95f-136">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="dc95f-137">Le conteneur résout `ILogger<TCategoryName>` en tirant parti de [types ouverts (génériques)](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), ce qui élimine la nécessité d’inscrire chaque [type construit (générique)](/dotnet/csharp/language-reference/language-specification/types#constructed-types) :</span><span class="sxs-lookup"><span data-stu-id="dc95f-137">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<T>), typeof(Logger<T>));
```

<span data-ttu-id="dc95f-138">Dans l’exemple d’application, le service `IMyDependency` est inscrit avec le type concret `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="dc95f-138">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="dc95f-139">L’inscription ajuste la durée de vie du service à la durée de vie d’une requête unique.</span><span class="sxs-lookup"><span data-stu-id="dc95f-139">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="dc95f-140">Les [durées de vie du service](#service-lifetimes) sont décrites plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="dc95f-140">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> <span data-ttu-id="dc95f-141">Chaque méthode d’extension `services.Add{SERVICE_NAME}` ajoute (et éventuellement configure) des services.</span><span class="sxs-lookup"><span data-stu-id="dc95f-141">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="dc95f-142">Par exemple, `services.AddMvc()` ajoute les services dont Razor Pages et MVC ont besoin.</span><span class="sxs-lookup"><span data-stu-id="dc95f-142">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="dc95f-143">Il est recommandé que les applications suivent cette convention.</span><span class="sxs-lookup"><span data-stu-id="dc95f-143">We recommended that apps follow this convention.</span></span> <span data-ttu-id="dc95f-144">Placez les méthodes d’extension dans l’espace de noms [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) pour encapsuler des groupes d’inscriptions de service.</span><span class="sxs-lookup"><span data-stu-id="dc95f-144">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="dc95f-145">Si le constructeur du service exige un [type intégré](/dotnet/csharp/language-reference/keywords/built-in-types-table), comme un `string`, le type peut être injecté à l’aide de la [configuration](xref:fundamentals/configuration/index) ou du [modèle d’options](xref:fundamentals/configuration/options) :</span><span class="sxs-lookup"><span data-stu-id="dc95f-145">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

```csharp
public class MyDependency : IMyDependency
{
    public MyDependency(IConfiguration config)
    {
        var myStringValue = config["MyStringKey"];

        // Use myStringValue
    }

    ...
}
```

<span data-ttu-id="dc95f-146">Une instance du service est demandée via le constructeur d’une classe dans laquelle le service est utilisé et assigné à un champ privé.</span><span class="sxs-lookup"><span data-stu-id="dc95f-146">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="dc95f-147">Le champ est utilisé pour accéder au service en fonction des besoins tout au long de la classe.</span><span class="sxs-lookup"><span data-stu-id="dc95f-147">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="dc95f-148">Dans l’exemple d’application, l’instance `IMyDependency` est demandée et utilisée pour appeler la méthode `WriteMessage` du service :</span><span class="sxs-lookup"><span data-stu-id="dc95f-148">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="framework-provided-services"></a><span data-ttu-id="dc95f-149">Services fournis par le framework</span><span class="sxs-lookup"><span data-stu-id="dc95f-149">Framework-provided services</span></span>

<span data-ttu-id="dc95f-150">La méthode `Startup.ConfigureServices` est chargée de définir les services utilisés par l’application, notamment les fonctionnalités de plateforme comme Entity Framework Core et ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="dc95f-150">The `Startup.ConfigureServices` method is responsible for defining the services the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="dc95f-151">Au départ, la valeur `IServiceCollection` fournie à `ConfigureServices` a les services suivants définis (en fonction de [la manière dont l’hôte a été configuré](xref:fundamentals/index#host)) :</span><span class="sxs-lookup"><span data-stu-id="dc95f-151">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/index#host)):</span></span>

| <span data-ttu-id="dc95f-152">Type de service</span><span class="sxs-lookup"><span data-stu-id="dc95f-152">Service Type</span></span> | <span data-ttu-id="dc95f-153">Durée de vie</span><span class="sxs-lookup"><span data-stu-id="dc95f-153">Lifetime</span></span> |
| ------------ | -------- |
| [<span data-ttu-id="dc95f-154">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span><span class="sxs-lookup"><span data-stu-id="dc95f-154">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | <span data-ttu-id="dc95f-155">Transient</span><span class="sxs-lookup"><span data-stu-id="dc95f-155">Transient</span></span> |
| [<span data-ttu-id="dc95f-156">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="dc95f-156">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | <span data-ttu-id="dc95f-157">Singleton</span><span class="sxs-lookup"><span data-stu-id="dc95f-157">Singleton</span></span> |
| [<span data-ttu-id="dc95f-158">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="dc95f-158">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | <span data-ttu-id="dc95f-159">Singleton</span><span class="sxs-lookup"><span data-stu-id="dc95f-159">Singleton</span></span> |
| [<span data-ttu-id="dc95f-160">Microsoft.AspNetCore.Hosting.IStartup</span><span class="sxs-lookup"><span data-stu-id="dc95f-160">Microsoft.AspNetCore.Hosting.IStartup</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | <span data-ttu-id="dc95f-161">Singleton</span><span class="sxs-lookup"><span data-stu-id="dc95f-161">Singleton</span></span> |
| [<span data-ttu-id="dc95f-162">Microsoft.AspNetCore.Hosting.IStartupFilter</span><span class="sxs-lookup"><span data-stu-id="dc95f-162">Microsoft.AspNetCore.Hosting.IStartupFilter</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | <span data-ttu-id="dc95f-163">Transient</span><span class="sxs-lookup"><span data-stu-id="dc95f-163">Transient</span></span> |
| [<span data-ttu-id="dc95f-164">Microsoft.AspNetCore.Hosting.Server.IServer</span><span class="sxs-lookup"><span data-stu-id="dc95f-164">Microsoft.AspNetCore.Hosting.Server.IServer</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | <span data-ttu-id="dc95f-165">Singleton</span><span class="sxs-lookup"><span data-stu-id="dc95f-165">Singleton</span></span> |
| [<span data-ttu-id="dc95f-166">Microsoft.AspNetCore.Http.IHttpContextFactory</span><span class="sxs-lookup"><span data-stu-id="dc95f-166">Microsoft.AspNetCore.Http.IHttpContextFactory</span></span>](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | <span data-ttu-id="dc95f-167">Transient</span><span class="sxs-lookup"><span data-stu-id="dc95f-167">Transient</span></span> |
| [<span data-ttu-id="dc95f-168">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="dc95f-168">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.logging.ilogger) | <span data-ttu-id="dc95f-169">Singleton</span><span class="sxs-lookup"><span data-stu-id="dc95f-169">Singleton</span></span> |
| [<span data-ttu-id="dc95f-170">Microsoft.Extensions.Logging.ILoggerFactory</span><span class="sxs-lookup"><span data-stu-id="dc95f-170">Microsoft.Extensions.Logging.ILoggerFactory</span></span>](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | <span data-ttu-id="dc95f-171">Singleton</span><span class="sxs-lookup"><span data-stu-id="dc95f-171">Singleton</span></span> |
| [<span data-ttu-id="dc95f-172">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span><span class="sxs-lookup"><span data-stu-id="dc95f-172">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span></span>](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | <span data-ttu-id="dc95f-173">Singleton</span><span class="sxs-lookup"><span data-stu-id="dc95f-173">Singleton</span></span> |
| [<span data-ttu-id="dc95f-174">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="dc95f-174">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | <span data-ttu-id="dc95f-175">Transient</span><span class="sxs-lookup"><span data-stu-id="dc95f-175">Transient</span></span> |
| [<span data-ttu-id="dc95f-176">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="dc95f-176">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.ioptions-1) | <span data-ttu-id="dc95f-177">Singleton</span><span class="sxs-lookup"><span data-stu-id="dc95f-177">Singleton</span></span> |
| [<span data-ttu-id="dc95f-178">System.Diagnostics.DiagnosticSource</span><span class="sxs-lookup"><span data-stu-id="dc95f-178">System.Diagnostics.DiagnosticSource</span></span>](/dotnet/core/api/system.diagnostics.diagnosticsource) | <span data-ttu-id="dc95f-179">Singleton</span><span class="sxs-lookup"><span data-stu-id="dc95f-179">Singleton</span></span> |
| [<span data-ttu-id="dc95f-180">System.Diagnostics.DiagnosticListener</span><span class="sxs-lookup"><span data-stu-id="dc95f-180">System.Diagnostics.DiagnosticListener</span></span>](/dotnet/core/api/system.diagnostics.diagnosticlistener) | <span data-ttu-id="dc95f-181">Singleton</span><span class="sxs-lookup"><span data-stu-id="dc95f-181">Singleton</span></span> |

<span data-ttu-id="dc95f-182">Lorsqu’une méthode d’extension de collection de services est disponible pour inscrire un service (et ses services dépendants, si nécessaire), la convention consiste à utiliser une seule méthode d’extension `Add{SERVICE_NAME}` pour inscrire tous les services requis par ce service.</span><span class="sxs-lookup"><span data-stu-id="dc95f-182">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="dc95f-183">Le code suivant est un exemple montrant comment ajouter des services supplémentaires au conteneur en utilisant les méthodes d’extension [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity) et [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc) :</span><span class="sxs-lookup"><span data-stu-id="dc95f-183">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity), and [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddMvc();
}
```

<span data-ttu-id="dc95f-184">Pour plus d’informations, consultez la classe [ServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection) dans la documentation de l’API.</span><span class="sxs-lookup"><span data-stu-id="dc95f-184">For more information, see the [ServiceCollection Class](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection) in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="dc95f-185">Durées de vie du service</span><span class="sxs-lookup"><span data-stu-id="dc95f-185">Service lifetimes</span></span>

<span data-ttu-id="dc95f-186">Choisissez une durée de vie appropriée pour chaque service inscrit.</span><span class="sxs-lookup"><span data-stu-id="dc95f-186">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="dc95f-187">Vous pouvez configurer les services ASP.NET Core avec les durées de vie suivantes :</span><span class="sxs-lookup"><span data-stu-id="dc95f-187">ASP.NET Core services can be configured with the following lifetimes:</span></span>

<span data-ttu-id="dc95f-188">**Transient**</span><span class="sxs-lookup"><span data-stu-id="dc95f-188">**Transient**</span></span>

<span data-ttu-id="dc95f-189">Des services à durée de vie temporaire (Transient) sont créés chaque fois qu’ils sont demandés à partir du conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="dc95f-189">Transient lifetime services are created each time they're requested from the service container.</span></span> <span data-ttu-id="dc95f-190">Cette durée de vie convient parfaitement aux services légers et sans état.</span><span class="sxs-lookup"><span data-stu-id="dc95f-190">This lifetime works best for lightweight, stateless services.</span></span>

<span data-ttu-id="dc95f-191">**Scoped**</span><span class="sxs-lookup"><span data-stu-id="dc95f-191">**Scoped**</span></span>

<span data-ttu-id="dc95f-192">Les services à durée de vie délimitée (Scoped) sont créés une seule fois par requête de client (connexion).</span><span class="sxs-lookup"><span data-stu-id="dc95f-192">Scoped lifetime services are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="dc95f-193">Si vous utilisez un service Scoped dans un middleware, injectez le service dans la méthode `Invoke` ou `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="dc95f-193">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="dc95f-194">Ne faites pas l’injection via l’injection du constructeur, car elle force le service à se comporter comme un singleton.</span><span class="sxs-lookup"><span data-stu-id="dc95f-194">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="dc95f-195">Pour plus d'informations, consultez <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="dc95f-195">For more information, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="dc95f-196">**Singleton**</span><span class="sxs-lookup"><span data-stu-id="dc95f-196">**Singleton**</span></span>

<span data-ttu-id="dc95f-197">Les services avec une durée de vie singleton sont créés la première fois qu’ils sont demandés (ou lorsque `ConfigureServices` est exécuté et qu’une instance est spécifiée avec l’inscription du service).</span><span class="sxs-lookup"><span data-stu-id="dc95f-197">Singleton lifetime services are created the first time they're requested (or when `ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="dc95f-198">Chaque requête ultérieure utilise la même instance.</span><span class="sxs-lookup"><span data-stu-id="dc95f-198">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="dc95f-199">Si l’application exige un comportement singleton, il est recommandé d’autoriser le conteneur de service à gérer la durée de vie du service.</span><span class="sxs-lookup"><span data-stu-id="dc95f-199">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="dc95f-200">N’implémentez pas le modèle de conception singleton et fournissez le code utilisateur pour gérer la durée de vie de l’objet dans la classe.</span><span class="sxs-lookup"><span data-stu-id="dc95f-200">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="dc95f-201">Il est dangereux de résoudre un service délimité depuis un singleton.</span><span class="sxs-lookup"><span data-stu-id="dc95f-201">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="dc95f-202">L’état du service risque de ne pas être correct lors du traitement des requêtes suivantes.</span><span class="sxs-lookup"><span data-stu-id="dc95f-202">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

### <a name="constructor-injection-behavior"></a><span data-ttu-id="dc95f-203">Comportement d’injection de constructeurs</span><span class="sxs-lookup"><span data-stu-id="dc95f-203">Constructor injection behavior</span></span>

<span data-ttu-id="dc95f-204">Les services peuvent être résolus par deux mécanismes :</span><span class="sxs-lookup"><span data-stu-id="dc95f-204">Services can be resolved by two mechanisms:</span></span>

* `IServiceProvider`
* <span data-ttu-id="dc95f-205">[ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; autorise la création d’objet sans inscription du service dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="dc95f-205">[ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="dc95f-206">`ActivatorUtilities` est utilisé avec les abstractions orientées utilisateur, telles que les Tag Helpers, les contrôleurs MVC et les classeurs de modèles.</span><span class="sxs-lookup"><span data-stu-id="dc95f-206">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="dc95f-207">Les constructeurs peuvent accepter des arguments qui ne sont pas fournis par l’injection de dépendances, mais les arguments doivent affecter des valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="dc95f-207">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="dc95f-208">Lorsque des services sont résolus par `IServiceProvider` ou `ActivatorUtilities`, l’injection de constructeurs exige un constructeur *public*.</span><span class="sxs-lookup"><span data-stu-id="dc95f-208">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="dc95f-209">Lorsque des services sont résolus par `ActivatorUtilities`, l’injection de constructeurs exige qu’un seul constructeur applicable existe.</span><span class="sxs-lookup"><span data-stu-id="dc95f-209">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exists.</span></span> <span data-ttu-id="dc95f-210">Les surcharges de constructeurs sont prises en charge, mais une seule peut exister dont les arguments peuvent tous être satisfaits par l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="dc95f-210">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="dc95f-211">Contextes Entity Framework</span><span class="sxs-lookup"><span data-stu-id="dc95f-211">Entity Framework contexts</span></span>

<span data-ttu-id="dc95f-212">Les contextes Entity Framework sont généralement ajoutés au conteneur de service en utilisant la [durée de vie limitée](#service-lifetimes), car la portée des opérations de base de données d’application web est normalement limitée à la requête du client.</span><span class="sxs-lookup"><span data-stu-id="dc95f-212">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="dc95f-213">La durée de vie par défaut est limitée si aucune durée de vie n’est spécifiée par une surcharge <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*> au moment de l’inscription du contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="dc95f-213">The default lifetime is scoped if a lifetime isn't specified by an <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*> overload when registering the database context.</span></span> <span data-ttu-id="dc95f-214">Un service d’une durée de vie donnée ne doit pas utiliser un contexte de base de données dont la durée de vie est plus courte que celle du service.</span><span class="sxs-lookup"><span data-stu-id="dc95f-214">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="dc95f-215">Options de durée de vie et d’inscription</span><span class="sxs-lookup"><span data-stu-id="dc95f-215">Lifetime and registration options</span></span>

<span data-ttu-id="dc95f-216">Pour illustrer la différence entre les options de durée de vie et d’inscription, considérez les interfaces suivantes qui représentent des tâches en tant qu’opération avec un identificateur unique, `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="dc95f-216">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="dc95f-217">Selon la façon dont la durée de vie d’un service d’opérations est configurée pour les interfaces suivantes, le conteneur fournit la même instance ou une instance différente du service lorsqu’une classe le demande :</span><span class="sxs-lookup"><span data-stu-id="dc95f-217">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

<span data-ttu-id="dc95f-218">Les interfaces sont implémentées dans la classe `Operation`.</span><span class="sxs-lookup"><span data-stu-id="dc95f-218">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="dc95f-219">Le constructeur `Operation` génère un GUID s’il n’est pas fourni :</span><span class="sxs-lookup"><span data-stu-id="dc95f-219">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

<span data-ttu-id="dc95f-220">Un `OperationService` est inscrit, dépendant de chacun des autres types `Operation`.</span><span class="sxs-lookup"><span data-stu-id="dc95f-220">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="dc95f-221">Lorsque `OperationService` est demandé via l’injection de dépendances, il reçoit une nouvelle instance de chaque service ou une instance existante en fonction de la durée de vie du service dépendant.</span><span class="sxs-lookup"><span data-stu-id="dc95f-221">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="dc95f-222">Quand des services temporaires sont créés à la demande à partir du conteneur, le `OperationId` du service `IOperationTransient` est différent du `OperationId` de `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="dc95f-222">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="dc95f-223">`OperationService` reçoit une nouvelle instance de la classe `IOperationTransient`.</span><span class="sxs-lookup"><span data-stu-id="dc95f-223">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="dc95f-224">La nouvelle instance génère un autre `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="dc95f-224">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="dc95f-225">Quand des services délimités sont créés pour chaque requête de client, le `OperationId` du `IOperationScoped` service est identique à celui de `OperationService` au sein d’une requête de client.</span><span class="sxs-lookup"><span data-stu-id="dc95f-225">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="dc95f-226">Entre les requêtes de client, les deux services partagent une valeur `OperationId` différente.</span><span class="sxs-lookup"><span data-stu-id="dc95f-226">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="dc95f-227">Quand des services singleton et d’instances singleton sont créés une fois et utilisés sur toutes les requêtes de client et tous les services, le `OperationId` est constant entre toutes les requêtes de service.</span><span class="sxs-lookup"><span data-stu-id="dc95f-227">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

<span data-ttu-id="dc95f-228">Dans `Startup.ConfigureServices`, chaque type est ajouté au conteneur en fonction de sa durée de vie nommée :</span><span class="sxs-lookup"><span data-stu-id="dc95f-228">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

<span data-ttu-id="dc95f-229">Le service `IOperationSingletonInstance` utilise une instance spécifique avec un ID connu `Guid.Empty`.</span><span class="sxs-lookup"><span data-stu-id="dc95f-229">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="dc95f-230">L’utilisation de ce type est facilement identifiable (son GUID n’affiche que des zéros).</span><span class="sxs-lookup"><span data-stu-id="dc95f-230">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="dc95f-231">L’exemple d’application montre les durées de vie des objets au sein et entre des requêtes individuelles.</span><span class="sxs-lookup"><span data-stu-id="dc95f-231">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="dc95f-232">L’exemple d’application `IndexModel` demande chaque type `IOperation` et `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="dc95f-232">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="dc95f-233">La page affiche ensuite l’ensemble de la classe du modèle de page et des valeurs `OperationId` du service via des assignations de propriété :</span><span class="sxs-lookup"><span data-stu-id="dc95f-233">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

<span data-ttu-id="dc95f-234">Les deux sorties suivantes montrent les résultats de deux requêtes :</span><span class="sxs-lookup"><span data-stu-id="dc95f-234">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="dc95f-235">**Première requête :**</span><span class="sxs-lookup"><span data-stu-id="dc95f-235">**First request:**</span></span>

<span data-ttu-id="dc95f-236">Opérations du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="dc95f-236">Controller operations:</span></span>

<span data-ttu-id="dc95f-237">Transient: d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="dc95f-237">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="dc95f-238">Délimité : 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="dc95f-238">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="dc95f-239">Singleton : 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="dc95f-239">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="dc95f-240">Instance : 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="dc95f-240">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="dc95f-241">Opérations `OperationService` :</span><span class="sxs-lookup"><span data-stu-id="dc95f-241">`OperationService` operations:</span></span>

<span data-ttu-id="dc95f-242">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="dc95f-242">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="dc95f-243">Délimité : 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="dc95f-243">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="dc95f-244">Singleton : 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="dc95f-244">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="dc95f-245">Instance : 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="dc95f-245">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="dc95f-246">**Deuxième requête :**</span><span class="sxs-lookup"><span data-stu-id="dc95f-246">**Second request:**</span></span>

<span data-ttu-id="dc95f-247">Opérations du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="dc95f-247">Controller operations:</span></span>

<span data-ttu-id="dc95f-248">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="dc95f-248">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="dc95f-249">Délimité : 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="dc95f-249">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="dc95f-250">Singleton : 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="dc95f-250">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="dc95f-251">Instance : 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="dc95f-251">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="dc95f-252">Opérations `OperationService` :</span><span class="sxs-lookup"><span data-stu-id="dc95f-252">`OperationService` operations:</span></span>

<span data-ttu-id="dc95f-253">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="dc95f-253">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="dc95f-254">Délimité : 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="dc95f-254">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="dc95f-255">Singleton : 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="dc95f-255">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="dc95f-256">Instance : 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="dc95f-256">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="dc95f-257">Observez les valeurs `OperationId` qui varient au sein d’une requête et entre les requêtes :</span><span class="sxs-lookup"><span data-stu-id="dc95f-257">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="dc95f-258">Les objets *Transient* sont toujours différents.</span><span class="sxs-lookup"><span data-stu-id="dc95f-258">*Transient* objects are always different.</span></span> <span data-ttu-id="dc95f-259">Les valeurs `OperationId` transitoires pour la première et la deuxième requêtes de client sont différentes pour les deux opérations `OperationService` et entre les requêtes de client.</span><span class="sxs-lookup"><span data-stu-id="dc95f-259">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="dc95f-260">Une nouvelle instance est fournie à chaque requête de service et requête de client.</span><span class="sxs-lookup"><span data-stu-id="dc95f-260">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="dc95f-261">Les objets *Scoped* sont les mêmes au sein d’une requête de client, mais ils diffèrent entre les requêtes de client.</span><span class="sxs-lookup"><span data-stu-id="dc95f-261">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="dc95f-262">Les objets *Singleton* sont les mêmes pour chaque objet et chaque requête, qu’une instance `Operation` soit fournie dans `ConfigureServices` ou non.</span><span class="sxs-lookup"><span data-stu-id="dc95f-262">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="dc95f-263">Appeler des services à partir de Main</span><span class="sxs-lookup"><span data-stu-id="dc95f-263">Call services from main</span></span>

<span data-ttu-id="dc95f-264">Créez un [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) avec [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) pour résoudre un service Scoped dans le scope de l’application.</span><span class="sxs-lookup"><span data-stu-id="dc95f-264">Create an [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) with [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="dc95f-265">Cette approche est pratique pour accéder à un service Scoped au démarrage pour exécuter des tâches d’initialisation.</span><span class="sxs-lookup"><span data-stu-id="dc95f-265">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="dc95f-266">L’exemple suivant montre comment obtenir un contexte pour `MyScopedService` dans `Program.Main` :</span><span class="sxs-lookup"><span data-stu-id="dc95f-266">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = CreateWebHostBuilder(args).Build();

    using (var serviceScope = host.Services.CreateScope())
    {
        var services = serviceScope.ServiceProvider;

        try
        {
            var serviceContext = services.GetRequiredService<MyScopedService>();
            // Use the context here
        }
        catch (Exception ex)
        {
            var logger = services.GetRequiredService<ILogger<Program>>();
            logger.LogError(ex, "An error occurred.");
        }
    }

    host.Run();
}
```

## <a name="scope-validation"></a><span data-ttu-id="dc95f-267">Validation de l’étendue</span><span class="sxs-lookup"><span data-stu-id="dc95f-267">Scope validation</span></span>

<span data-ttu-id="dc95f-268">Quand l’application s’exécute dans l’environnement de développement, le fournisseur de services par défaut effectue des contrôles pour vérifier que :</span><span class="sxs-lookup"><span data-stu-id="dc95f-268">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="dc95f-269">Les services Scoped ne sont pas résolus directement ou indirectement à partir du fournisseur de services racine.</span><span class="sxs-lookup"><span data-stu-id="dc95f-269">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="dc95f-270">Les services Scoped ne sont pas directement ou indirectement injectés dans des singletons.</span><span class="sxs-lookup"><span data-stu-id="dc95f-270">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="dc95f-271">Le fournisseur de services racine est créé quand [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) est appelé.</span><span class="sxs-lookup"><span data-stu-id="dc95f-271">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="dc95f-272">La durée de vie du fournisseur de services racine correspond à la durée de vie de l’application/du serveur quand le fournisseur démarre avec l’application et qu’il est supprimé quand l’application s’arrête.</span><span class="sxs-lookup"><span data-stu-id="dc95f-272">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="dc95f-273">Les services Scoped sont supprimés par le conteneur qui les a créés.</span><span class="sxs-lookup"><span data-stu-id="dc95f-273">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="dc95f-274">Si un service Scoped est créé dans le conteneur racine, la durée de vie du service est promue en singleton, car elle est supprimée par le conteneur racine seulement quand l’application/le serveur est arrêté.</span><span class="sxs-lookup"><span data-stu-id="dc95f-274">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="dc95f-275">La validation des étendues du service permet de traiter ces situations quand `BuildServiceProvider` est appelé.</span><span class="sxs-lookup"><span data-stu-id="dc95f-275">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="dc95f-276">Pour plus d'informations, consultez <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="dc95f-276">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="dc95f-277">Services de requête</span><span class="sxs-lookup"><span data-stu-id="dc95f-277">Request Services</span></span>

<span data-ttu-id="dc95f-278">Les services disponibles au sein d’une requête ASP.NET Core à partir de `HttpContext` sont exposés par le biais de la collection [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices).</span><span class="sxs-lookup"><span data-stu-id="dc95f-278">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices) collection.</span></span>

<span data-ttu-id="dc95f-279">Les services de requête représentent les services configurés et demandés dans le cadre de l’application.</span><span class="sxs-lookup"><span data-stu-id="dc95f-279">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="dc95f-280">Lorsque les objets spécifient des dépendances, ceux-ci sont satisfaits par les types trouvés dans `RequestServices`, pas dans `ApplicationServices`.</span><span class="sxs-lookup"><span data-stu-id="dc95f-280">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="dc95f-281">En règle générale, l’application ne doit pas utiliser directement ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="dc95f-281">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="dc95f-282">Demandez plutôt les types nécessaires à la classe via des constructeurs de classe et autorisez le framework à injecter les dépendances.</span><span class="sxs-lookup"><span data-stu-id="dc95f-282">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="dc95f-283">Cela génère des classes qui sont plus faciles à tester.</span><span class="sxs-lookup"><span data-stu-id="dc95f-283">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="dc95f-284">Préférez demander des dépendances en tant que paramètres de constructeur plutôt qu’accéder à la collection `RequestServices`.</span><span class="sxs-lookup"><span data-stu-id="dc95f-284">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="dc95f-285">Conception de services pour l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="dc95f-285">Design services for dependency injection</span></span>

<span data-ttu-id="dc95f-286">Les bonnes pratiques permettent de :</span><span class="sxs-lookup"><span data-stu-id="dc95f-286">Best practices are to:</span></span>

* <span data-ttu-id="dc95f-287">Concevoir des services afin d’utiliser l’injection de dépendances pour obtenir leurs dépendances.</span><span class="sxs-lookup"><span data-stu-id="dc95f-287">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="dc95f-288">Éviter les appels de méthode statiques avec état.</span><span class="sxs-lookup"><span data-stu-id="dc95f-288">Avoid stateful, static method calls.</span></span>
* <span data-ttu-id="dc95f-289">Éviter une instanciation directe de classes dépendantes au sein de services.</span><span class="sxs-lookup"><span data-stu-id="dc95f-289">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="dc95f-290">L’instanciation directe associe le code à une implémentation particulière.</span><span class="sxs-lookup"><span data-stu-id="dc95f-290">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="dc95f-291">Limiter la taille des classes d’application, faire en sorte qu’elles soient bien factorisées et facilement testées.</span><span class="sxs-lookup"><span data-stu-id="dc95f-291">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="dc95f-292">Si une classe semble avoir trop de dépendances injectées, cela signifie généralement que la classe a trop de responsabilités et viole le [principe de responsabilité unique](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span><span class="sxs-lookup"><span data-stu-id="dc95f-292">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="dc95f-293">Essayez de refactoriser la classe en déplaçant certaines de ses responsabilités dans une nouvelle classe.</span><span class="sxs-lookup"><span data-stu-id="dc95f-293">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="dc95f-294">N’oubliez pas que les classes du modèle de page Razor Pages et les classes du contrôleur MVC doivent se concentrer sur les problèmes d’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="dc95f-294">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="dc95f-295">Les règles d’entreprise et les détails d’implémentation de l’accès aux données doivent être conservés dans les classes appropriées à ces [préoccupations distinctes](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span><span class="sxs-lookup"><span data-stu-id="dc95f-295">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="dc95f-296">Suppression des services</span><span class="sxs-lookup"><span data-stu-id="dc95f-296">Disposal of services</span></span>

<span data-ttu-id="dc95f-297">Le conteneur appelle `Dispose` pour les types `IDisposable` qu’il crée.</span><span class="sxs-lookup"><span data-stu-id="dc95f-297">The container calls `Dispose` for the `IDisposable` types it creates.</span></span> <span data-ttu-id="dc95f-298">Si une instance est ajoutée au conteneur par le code utilisateur, elle n’est pas supprimée automatiquement.</span><span class="sxs-lookup"><span data-stu-id="dc95f-298">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

```csharp
// Services that implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    // The container creates the following instances and disposes them automatically:
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // The container doesn't create the following instances, so it doesn't dispose of
    // the instances automatically:
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

## <a name="default-service-container-replacement"></a><span data-ttu-id="dc95f-299">Remplacement de conteneur de services par défaut</span><span class="sxs-lookup"><span data-stu-id="dc95f-299">Default service container replacement</span></span>

<span data-ttu-id="dc95f-300">Le conteneur de services intégré a vocation à répondre aux besoins du framework et de la plupart des applications consommatrices.</span><span class="sxs-lookup"><span data-stu-id="dc95f-300">The built-in service container is meant to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="dc95f-301">Nous vous recommandons d’utiliser le conteneur intégré, sauf si vous avez besoin d’une fonctionnalité spécifique qu’il ne prend pas en charge.</span><span class="sxs-lookup"><span data-stu-id="dc95f-301">We recommend using the built-in container unless you need a specific feature that it doesn't support.</span></span> <span data-ttu-id="dc95f-302">Voici quelques-unes des fonctionnalités prises en charge dans des conteneurs tiers qui sont introuvables dans le conteneur intégré :</span><span class="sxs-lookup"><span data-stu-id="dc95f-302">Some of the features supported in 3rd party containers not found in the built-in container:</span></span>

* <span data-ttu-id="dc95f-303">Injection de propriétés</span><span class="sxs-lookup"><span data-stu-id="dc95f-303">Property injection</span></span>
* <span data-ttu-id="dc95f-304">Injection en fonction du nom</span><span class="sxs-lookup"><span data-stu-id="dc95f-304">Injection based on name</span></span>
* <span data-ttu-id="dc95f-305">Conteneurs enfants</span><span class="sxs-lookup"><span data-stu-id="dc95f-305">Child containers</span></span>
* <span data-ttu-id="dc95f-306">Gestion personnalisée de la durée de vie</span><span class="sxs-lookup"><span data-stu-id="dc95f-306">Custom lifetime management</span></span>
* <span data-ttu-id="dc95f-307">`Func<T>` prend en charge l’initialisation tardive</span><span class="sxs-lookup"><span data-stu-id="dc95f-307">`Func<T>` support for lazy initialization</span></span>

<span data-ttu-id="dc95f-308">Pour obtenir une liste de certains des conteneurs qui prennent en charge des adaptateurs, consultez le [fichier readme.md relatif à l’injection de dépendances](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="dc95f-308">See the [Dependency Injection readme.md file](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) for a list of some of the containers that support adapters.</span></span>

<span data-ttu-id="dc95f-309">L’exemple suivant remplace le conteneur intégré par [Autofac](https://autofac.org/) :</span><span class="sxs-lookup"><span data-stu-id="dc95f-309">The following sample replaces the built-in container with [Autofac](https://autofac.org/):</span></span>

* <span data-ttu-id="dc95f-310">Installez les packages de conteneurs appropriés :</span><span class="sxs-lookup"><span data-stu-id="dc95f-310">Install the appropriate container package(s):</span></span>

  * [<span data-ttu-id="dc95f-311">Autofac</span><span class="sxs-lookup"><span data-stu-id="dc95f-311">Autofac</span></span>](https://www.nuget.org/packages/Autofac/)
  * [<span data-ttu-id="dc95f-312">Autofac.Extensions.DependencyInjection</span><span class="sxs-lookup"><span data-stu-id="dc95f-312">Autofac.Extensions.DependencyInjection</span></span>](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)

* <span data-ttu-id="dc95f-313">Configurez le conteneur dans `Startup.ConfigureServices` et retournez un `IServiceProvider` :</span><span class="sxs-lookup"><span data-stu-id="dc95f-313">Configure the container in `Startup.ConfigureServices` and return an `IServiceProvider`:</span></span>

    ```csharp
    public IServiceProvider ConfigureServices(IServiceCollection services)
    {
        services.AddMvc();
        // Add other framework services

        // Add Autofac
        var containerBuilder = new ContainerBuilder();
        containerBuilder.RegisterModule<DefaultModule>();
        containerBuilder.Populate(services);
        var container = containerBuilder.Build();
        return new AutofacServiceProvider(container);
    }
    ```

    <span data-ttu-id="dc95f-314">Pour utiliser un conteneur tiers, `Startup.ConfigureServices` doit retourner `IServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="dc95f-314">To use a 3rd party container, `Startup.ConfigureServices` must return `IServiceProvider`.</span></span>

* <span data-ttu-id="dc95f-315">Configurez Autofac dans `DefaultModule` :</span><span class="sxs-lookup"><span data-stu-id="dc95f-315">Configure Autofac in `DefaultModule`:</span></span>

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

<span data-ttu-id="dc95f-316">Au moment de l’exécution, Autofac est utilisé pour résoudre les types et injecter des dépendances.</span><span class="sxs-lookup"><span data-stu-id="dc95f-316">At runtime, Autofac is used to resolve types and inject dependencies.</span></span> <span data-ttu-id="dc95f-317">Pour en savoir plus sur l’utilisation d’Autofac avec ASP.NET Core, consultez la [documentation Autofac](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span><span class="sxs-lookup"><span data-stu-id="dc95f-317">To learn more about using Autofac with ASP.NET Core, see the [Autofac documentation](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="dc95f-318">Sécurité des threads</span><span class="sxs-lookup"><span data-stu-id="dc95f-318">Thread safety</span></span>

<span data-ttu-id="dc95f-319">Créez des services singleton thread-safe.</span><span class="sxs-lookup"><span data-stu-id="dc95f-319">Create thread-safe singleton services.</span></span> <span data-ttu-id="dc95f-320">Si un service singleton a une dépendance vis-à-vis d’un service Transient, ce dernier peut également nécessiter la cohérence de thread, en fonction de la manière dont il est utilisé par le singleton.</span><span class="sxs-lookup"><span data-stu-id="dc95f-320">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="dc95f-321">La méthode de fabrique d’un service individuel, comme le deuxième argument de [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider, TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), ne doit être thread-safe.</span><span class="sxs-lookup"><span data-stu-id="dc95f-321">The factory method of single service, such as the second argument to [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider,TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), doesn't need to be thread-safe.</span></span> <span data-ttu-id="dc95f-322">Comme un constructeur de type (`static`), elle est forcément appelée une fois par un seul thread.</span><span class="sxs-lookup"><span data-stu-id="dc95f-322">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="dc95f-323">Recommandations</span><span class="sxs-lookup"><span data-stu-id="dc95f-323">Recommendations</span></span>

* <span data-ttu-id="dc95f-324">La résolution de service basée sur `async/await` et `Task` n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="dc95f-324">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="dc95f-325">C# ne prend pas en charge les constructeurs asynchrones. Par conséquent, le modèle recommandé consiste à utiliser des méthodes asynchrones après avoir résolu de façon synchrone le service.</span><span class="sxs-lookup"><span data-stu-id="dc95f-325">C# does not support asynchronous constructors, therefore the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="dc95f-326">Évitez de stocker des données et des configurations directement dans le conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="dc95f-326">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="dc95f-327">Par exemple, le panier d’achat d’un utilisateur ne doit en général pas être ajouté au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="dc95f-327">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="dc95f-328">La configuration doit utiliser le [modèle d’options](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="dc95f-328">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="dc95f-329">De même, évitez les objets « conteneurs de données » qui n’existent que pour autoriser l’accès à un autre objet.</span><span class="sxs-lookup"><span data-stu-id="dc95f-329">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="dc95f-330">Il est préférable de demander l’élément réel par le biais de l’injection de dépendance.</span><span class="sxs-lookup"><span data-stu-id="dc95f-330">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="dc95f-331">Évitez l’accès statique aux services (par exemple avec [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) à utiliser ailleurs).</span><span class="sxs-lookup"><span data-stu-id="dc95f-331">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) for use elsewhere).</span></span>

* <span data-ttu-id="dc95f-332">Évitez d’utiliser le *modèle de localisation de service*.</span><span class="sxs-lookup"><span data-stu-id="dc95f-332">Avoid using the *service locator pattern*.</span></span> <span data-ttu-id="dc95f-333">Par exemple, n’appelez pas <xref:System.IServiceProvider.GetService*> pour obtenir une instance de service lorsque vous pouvez utiliser l’injection de dépendance à la place.</span><span class="sxs-lookup"><span data-stu-id="dc95f-333">For example, don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead.</span></span> <span data-ttu-id="dc95f-334">Une autre variante du localisateur de service à éviter est l’injection d’une fabrique qui résout les dépendances au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="dc95f-334">Another service locator variation to avoid is injecting a factory that resolves dependencies at runtime.</span></span> <span data-ttu-id="dc95f-335">Ces deux pratiques combinent des stratégies [Inversion de contrôle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion).</span><span class="sxs-lookup"><span data-stu-id="dc95f-335">Both of these practices mix [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

* <span data-ttu-id="dc95f-336">Évitez l’accès statique à `HttpContext` (par exemple, [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).</span><span class="sxs-lookup"><span data-stu-id="dc95f-336">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).</span></span>

<span data-ttu-id="dc95f-337">Comme pour toutes les recommandations, vous pouvez vous trouver dans des situations où il est nécessaire d’ignorer une recommandation.</span><span class="sxs-lookup"><span data-stu-id="dc95f-337">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="dc95f-338">Les exceptions sont rares et représentent principalement des cas spéciaux dans le framework lui-même.</span><span class="sxs-lookup"><span data-stu-id="dc95f-338">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="dc95f-339">L’injection de dépendance constitue une *alternative* aux modèles d’accès aux objets statiques/globaux.</span><span class="sxs-lookup"><span data-stu-id="dc95f-339">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="dc95f-340">Il est possible que vous ne bénéficiez pas des avantages de l’injection de dépendances si vous la combinez avec l’accès aux objets statiques.</span><span class="sxs-lookup"><span data-stu-id="dc95f-340">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dc95f-341">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="dc95f-341">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="dc95f-342">Écrire un code clair dans ASP.NET Core avec l’injection de dépendance (MSDN)</span><span class="sxs-lookup"><span data-stu-id="dc95f-342">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="dc95f-343">Principe des dépendances explicites</span><span class="sxs-lookup"><span data-stu-id="dc95f-343">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [<span data-ttu-id="dc95f-344">Conteneurs d’inversion de contrôle et modèle d’injection de dépendances (Martin Fowler)</span><span class="sxs-lookup"><span data-stu-id="dc95f-344">Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
* [<span data-ttu-id="dc95f-345">Comment inscrire un service avec plusieurs interfaces dans l’injection de dépendance ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dc95f-345">How to register a service with multiple interfaces in ASP.NET Core DI</span></span>](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)
