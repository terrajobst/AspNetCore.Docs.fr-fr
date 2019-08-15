---
title: Injection de dépendances dans ASP.NET Core
author: guardrex
description: Découvrez comment ASP.NET Core implémente l’injection de dépendances et comment l’utiliser.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2019
uid: fundamentals/dependency-injection
ms.openlocfilehash: a984bb766e6876db4f8ed4c850a1984ba87d627d
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022291"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="9067f-103">Injection de dépendances dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9067f-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="9067f-104">Par [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9067f-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9067f-105">ASP.NET Core prend en charge le modèle de conception de logiciel avec injection de dépendances (DI), une technique permettant d’obtenir une [inversion de contrôle (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) entre les classes et leurs dépendances.</span><span class="sxs-lookup"><span data-stu-id="9067f-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="9067f-106">Pour plus d’informations spécifiques à l’injection de dépendances au sein des contrôleurs MVC, consultez <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="9067f-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="9067f-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9067f-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="9067f-108">Vue d’ensemble de l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="9067f-108">Overview of dependency injection</span></span>

<span data-ttu-id="9067f-109">Une *dépendance* est un objet qui nécessite un autre objet.</span><span class="sxs-lookup"><span data-stu-id="9067f-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="9067f-110">Examinez la classe `MyDependency` suivante avec une méthode `WriteMessage` dont dépendent d’autres classes dans une application :</span><span class="sxs-lookup"><span data-stu-id="9067f-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

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

<span data-ttu-id="9067f-111">Une instance de la classe `MyDependency` peut être créée pour rendre la méthode `WriteMessage` disponible pour une classe.</span><span class="sxs-lookup"><span data-stu-id="9067f-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="9067f-112">La classe `MyDependency` est une dépendance de la classe `IndexModel` :</span><span class="sxs-lookup"><span data-stu-id="9067f-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

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

<span data-ttu-id="9067f-113">La classe est créee et dépend directement de l’instance `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="9067f-113">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="9067f-114">Les dépendances de code (comme l’exemple précédent) posent problème et doivent être évitées pour les raisons suivantes :</span><span class="sxs-lookup"><span data-stu-id="9067f-114">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="9067f-115">Pour remplacer `MyDependency` par une autre implémentation, la classe doit être modifiée.</span><span class="sxs-lookup"><span data-stu-id="9067f-115">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="9067f-116">Si `MyDependency` possède des dépendances, elles doivent être configurées par la classe.</span><span class="sxs-lookup"><span data-stu-id="9067f-116">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="9067f-117">Dans un grand projet comportant plusieurs classes dépendant de `MyDependency`, le code de configuration est disséminé dans l’application.</span><span class="sxs-lookup"><span data-stu-id="9067f-117">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="9067f-118">Cette implémentation complique le test unitaire.</span><span class="sxs-lookup"><span data-stu-id="9067f-118">This implementation is difficult to unit test.</span></span> <span data-ttu-id="9067f-119">L’application doit utiliser une classe `MyDependency` fictive ou stub, ce qui est impossible avec cette approche.</span><span class="sxs-lookup"><span data-stu-id="9067f-119">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="9067f-120">L’injection de dépendances résout ces problèmes via :</span><span class="sxs-lookup"><span data-stu-id="9067f-120">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="9067f-121">L’utilisation d’une interface ou classe de base pour extraire l’implémentation des dépendances.</span><span class="sxs-lookup"><span data-stu-id="9067f-121">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="9067f-122">L’inscription de la dépendance dans un conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="9067f-122">Registration of the dependency in a service container.</span></span> <span data-ttu-id="9067f-123">ASP.NET Core fournit un conteneur de service intégré, <xref:System.IServiceProvider>.</span><span class="sxs-lookup"><span data-stu-id="9067f-123">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="9067f-124">Les services sont inscrits dans la méthode `Startup.ConfigureServices` de l’application.</span><span class="sxs-lookup"><span data-stu-id="9067f-124">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="9067f-125">*Injection* du service dans le constructeur de la classe où il est utilisé.</span><span class="sxs-lookup"><span data-stu-id="9067f-125">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="9067f-126">Le framework prend la responsabilité de la création d’une instance de la dépendance et de sa suppression lorsqu’elle n’est plus nécessaire.</span><span class="sxs-lookup"><span data-stu-id="9067f-126">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="9067f-127">Dans l’[exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), l’interface `IMyDependency` définit une méthode que le service fournit à l’application :</span><span class="sxs-lookup"><span data-stu-id="9067f-127">In the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

<span data-ttu-id="9067f-128">Cette interface est implémentée par un type concret, `MyDependency` :</span><span class="sxs-lookup"><span data-stu-id="9067f-128">This interface is implemented by a concrete type, `MyDependency`:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

<span data-ttu-id="9067f-129">`MyDependency` exige un <xref:Microsoft.Extensions.Logging.ILogger`1> dans son constructeur.</span><span class="sxs-lookup"><span data-stu-id="9067f-129">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="9067f-130">Il n’est pas rare que l’injection de dépendances soit utilisée de manière chaînée.</span><span class="sxs-lookup"><span data-stu-id="9067f-130">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="9067f-131">Dans ce cas, chaque dépendance demandée demande à son tour ses propres dépendances.</span><span class="sxs-lookup"><span data-stu-id="9067f-131">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="9067f-132">Le conteneur résout les dépendances dans le graphique et retourne le service entièrement résolu.</span><span class="sxs-lookup"><span data-stu-id="9067f-132">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="9067f-133">L’ensemble collectif de dépendances qui doivent être résolues est généralement appelé *arborescence des dépendances*, *graphique de dépendance* ou *graphique d’objet*.</span><span class="sxs-lookup"><span data-stu-id="9067f-133">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="9067f-134">`IMyDependency` et `ILogger<TCategoryName>` doivent être inscrits dans le conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="9067f-134">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="9067f-135">`IMyDependency` est inscrit dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9067f-135">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9067f-136">`ILogger<TCategoryName>` est inscrit par l’infrastructure d’abstractions de journalisation. Il s’agit donc d’un [service fourni par le framework](#framework-provided-services) et inscrit par défaut par l’infrastructure.</span><span class="sxs-lookup"><span data-stu-id="9067f-136">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="9067f-137">Le conteneur résout `ILogger<TCategoryName>` en tirant parti de [types ouverts (génériques)](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), ce qui élimine la nécessité d’inscrire chaque [type construit (générique)](/dotnet/csharp/language-reference/language-specification/types#constructed-types) :</span><span class="sxs-lookup"><span data-stu-id="9067f-137">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<T>), typeof(Logger<T>));
```

<span data-ttu-id="9067f-138">Dans l’exemple d’application, le service `IMyDependency` est inscrit avec le type concret `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="9067f-138">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="9067f-139">L’inscription ajuste la durée de vie du service à la durée de vie d’une requête unique.</span><span class="sxs-lookup"><span data-stu-id="9067f-139">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="9067f-140">Les [durées de vie du service](#service-lifetimes) sont décrites plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="9067f-140">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> <span data-ttu-id="9067f-141">Chaque méthode d’extension `services.Add{SERVICE_NAME}` ajoute (et éventuellement configure) des services.</span><span class="sxs-lookup"><span data-stu-id="9067f-141">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="9067f-142">Par exemple, `services.AddMvc()` ajoute les services dont Razor Pages et MVC ont besoin.</span><span class="sxs-lookup"><span data-stu-id="9067f-142">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="9067f-143">Il est recommandé que les applications suivent cette convention.</span><span class="sxs-lookup"><span data-stu-id="9067f-143">We recommended that apps follow this convention.</span></span> <span data-ttu-id="9067f-144">Placez les méthodes d’extension dans l’espace de noms [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) pour encapsuler des groupes d’inscriptions de service.</span><span class="sxs-lookup"><span data-stu-id="9067f-144">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="9067f-145">Si le constructeur du service exige un [type intégré](/dotnet/csharp/language-reference/keywords/built-in-types-table), comme un `string`, le type peut être injecté à l’aide de la [configuration](xref:fundamentals/configuration/index) ou du [modèle d’options](xref:fundamentals/configuration/options) :</span><span class="sxs-lookup"><span data-stu-id="9067f-145">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

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

<span data-ttu-id="9067f-146">Une instance du service est demandée via le constructeur d’une classe dans laquelle le service est utilisé et assigné à un champ privé.</span><span class="sxs-lookup"><span data-stu-id="9067f-146">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="9067f-147">Le champ est utilisé pour accéder au service en fonction des besoins tout au long de la classe.</span><span class="sxs-lookup"><span data-stu-id="9067f-147">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="9067f-148">Dans l’exemple d’application, l’instance `IMyDependency` est demandée et utilisée pour appeler la méthode `WriteMessage` du service :</span><span class="sxs-lookup"><span data-stu-id="9067f-148">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="framework-provided-services"></a><span data-ttu-id="9067f-149">Services fournis par le framework</span><span class="sxs-lookup"><span data-stu-id="9067f-149">Framework-provided services</span></span>

<span data-ttu-id="9067f-150">La méthode `Startup.ConfigureServices` est chargée de définir les services utilisés par l’application, notamment les fonctionnalités de plateforme comme Entity Framework Core et ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="9067f-150">The `Startup.ConfigureServices` method is responsible for defining the services the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="9067f-151">Au départ, la valeur `IServiceCollection` fournie à `ConfigureServices` a les services suivants définis (en fonction de [la manière dont l’hôte a été configuré](xref:fundamentals/index#host)) :</span><span class="sxs-lookup"><span data-stu-id="9067f-151">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/index#host)):</span></span>

| <span data-ttu-id="9067f-152">Type de service</span><span class="sxs-lookup"><span data-stu-id="9067f-152">Service Type</span></span> | <span data-ttu-id="9067f-153">Durée de vie</span><span class="sxs-lookup"><span data-stu-id="9067f-153">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="9067f-154">Transient</span><span class="sxs-lookup"><span data-stu-id="9067f-154">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> | <span data-ttu-id="9067f-155">Singleton</span><span class="sxs-lookup"><span data-stu-id="9067f-155">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> | <span data-ttu-id="9067f-156">Singleton</span><span class="sxs-lookup"><span data-stu-id="9067f-156">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="9067f-157">Singleton</span><span class="sxs-lookup"><span data-stu-id="9067f-157">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="9067f-158">Transient</span><span class="sxs-lookup"><span data-stu-id="9067f-158">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="9067f-159">Singleton</span><span class="sxs-lookup"><span data-stu-id="9067f-159">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="9067f-160">Transient</span><span class="sxs-lookup"><span data-stu-id="9067f-160">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="9067f-161">Singleton</span><span class="sxs-lookup"><span data-stu-id="9067f-161">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="9067f-162">Singleton</span><span class="sxs-lookup"><span data-stu-id="9067f-162">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="9067f-163">Singleton</span><span class="sxs-lookup"><span data-stu-id="9067f-163">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="9067f-164">Transient</span><span class="sxs-lookup"><span data-stu-id="9067f-164">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="9067f-165">Singleton</span><span class="sxs-lookup"><span data-stu-id="9067f-165">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="9067f-166">Singleton</span><span class="sxs-lookup"><span data-stu-id="9067f-166">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="9067f-167">Singleton</span><span class="sxs-lookup"><span data-stu-id="9067f-167">Singleton</span></span> |

<span data-ttu-id="9067f-168">Lorsqu’une méthode d’extension de collection de services est disponible pour inscrire un service (et ses services dépendants, si nécessaire), la convention consiste à utiliser une seule méthode d’extension `Add{SERVICE_NAME}` pour inscrire tous les services requis par ce service.</span><span class="sxs-lookup"><span data-stu-id="9067f-168">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="9067f-169">Le code suivant est un exemple montrant comment ajouter des services supplémentaires au conteneur en utilisant les méthodes d’extension [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*> et <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> :</span><span class="sxs-lookup"><span data-stu-id="9067f-169">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>, and <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>:</span></span>

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

<span data-ttu-id="9067f-170">Pour plus d’informations, consultez la classe <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> dans la documentation de l’API.</span><span class="sxs-lookup"><span data-stu-id="9067f-170">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> class in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="9067f-171">Durées de vie du service</span><span class="sxs-lookup"><span data-stu-id="9067f-171">Service lifetimes</span></span>

<span data-ttu-id="9067f-172">Choisissez une durée de vie appropriée pour chaque service inscrit.</span><span class="sxs-lookup"><span data-stu-id="9067f-172">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="9067f-173">Vous pouvez configurer les services ASP.NET Core avec les durées de vie suivantes :</span><span class="sxs-lookup"><span data-stu-id="9067f-173">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="9067f-174">Transient</span><span class="sxs-lookup"><span data-stu-id="9067f-174">Transient</span></span>

<span data-ttu-id="9067f-175">Des services à durée de vie temporaire (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) sont créés chaque fois qu’ils sont demandés à partir du conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="9067f-175">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="9067f-176">Cette durée de vie convient parfaitement aux services légers et sans état.</span><span class="sxs-lookup"><span data-stu-id="9067f-176">This lifetime works best for lightweight, stateless services.</span></span>

### <a name="scoped"></a><span data-ttu-id="9067f-177">Délimité</span><span class="sxs-lookup"><span data-stu-id="9067f-177">Scoped</span></span>

<span data-ttu-id="9067f-178">Les services à durée de vie délimitée (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) sont créés une seule fois par requête de client (connexion).</span><span class="sxs-lookup"><span data-stu-id="9067f-178">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="9067f-179">Si vous utilisez un service Scoped dans un middleware, injectez le service dans la méthode `Invoke` ou `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="9067f-179">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="9067f-180">Ne faites pas l’injection via l’injection du constructeur, car elle force le service à se comporter comme un singleton.</span><span class="sxs-lookup"><span data-stu-id="9067f-180">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="9067f-181">Pour plus d’informations, consultez <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span><span class="sxs-lookup"><span data-stu-id="9067f-181">For more information, see <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span></span>

### <a name="singleton"></a><span data-ttu-id="9067f-182">Singleton</span><span class="sxs-lookup"><span data-stu-id="9067f-182">Singleton</span></span>

<span data-ttu-id="9067f-183">Les services avec une durée de vie singleton (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) sont créés la première fois qu’ils sont demandés (ou quand `Startup.ConfigureServices` est exécuté et qu’une instance est spécifiée avec l’inscription du service).</span><span class="sxs-lookup"><span data-stu-id="9067f-183">Singleton lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="9067f-184">Chaque requête ultérieure utilise la même instance.</span><span class="sxs-lookup"><span data-stu-id="9067f-184">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="9067f-185">Si l’application exige un comportement singleton, il est recommandé d’autoriser le conteneur de service à gérer la durée de vie du service.</span><span class="sxs-lookup"><span data-stu-id="9067f-185">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="9067f-186">N’implémentez pas le modèle de conception singleton et fournissez le code utilisateur pour gérer la durée de vie de l’objet dans la classe.</span><span class="sxs-lookup"><span data-stu-id="9067f-186">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="9067f-187">Il est dangereux de résoudre un service délimité depuis un singleton.</span><span class="sxs-lookup"><span data-stu-id="9067f-187">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="9067f-188">L’état du service risque de ne pas être correct lors du traitement des requêtes suivantes.</span><span class="sxs-lookup"><span data-stu-id="9067f-188">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="9067f-189">Méthodes d’inscription du service</span><span class="sxs-lookup"><span data-stu-id="9067f-189">Service registration methods</span></span>

<span data-ttu-id="9067f-190">Chaque méthode d’extension d’inscription du service propose des surcharges qui sont utiles dans des scénarios spécifiques.</span><span class="sxs-lookup"><span data-stu-id="9067f-190">Each service registration extension method offers overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="9067f-191">Méthode</span><span class="sxs-lookup"><span data-stu-id="9067f-191">Method</span></span> | <span data-ttu-id="9067f-192">Automatique</span><span class="sxs-lookup"><span data-stu-id="9067f-192">Automatic</span></span><br><span data-ttu-id="9067f-193">object</span><span class="sxs-lookup"><span data-stu-id="9067f-193">object</span></span><br><span data-ttu-id="9067f-194">suppression</span><span class="sxs-lookup"><span data-stu-id="9067f-194">disposal</span></span> | <span data-ttu-id="9067f-195">Multiple</span><span class="sxs-lookup"><span data-stu-id="9067f-195">Multiple</span></span><br><span data-ttu-id="9067f-196">implémentations</span><span class="sxs-lookup"><span data-stu-id="9067f-196">implementations</span></span> | <span data-ttu-id="9067f-197">Passage d’args</span><span class="sxs-lookup"><span data-stu-id="9067f-197">Pass args</span></span> |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><span data-ttu-id="9067f-198">Exemple :</span><span class="sxs-lookup"><span data-stu-id="9067f-198">Example:</span></span><br>`services.AddScoped<IMyDep, MyDep>();` | <span data-ttu-id="9067f-199">Oui</span><span class="sxs-lookup"><span data-stu-id="9067f-199">Yes</span></span> | <span data-ttu-id="9067f-200">Oui</span><span class="sxs-lookup"><span data-stu-id="9067f-200">Yes</span></span> | <span data-ttu-id="9067f-201">Non</span><span class="sxs-lookup"><span data-stu-id="9067f-201">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><span data-ttu-id="9067f-202">Exemples :</span><span class="sxs-lookup"><span data-stu-id="9067f-202">Examples:</span></span><br>`services.AddScoped<IMyDep>(sp => new MyDep());`<br>`services.AddScoped<IMyDep>(sp => new MyDep("A string!"));` | <span data-ttu-id="9067f-203">Oui</span><span class="sxs-lookup"><span data-stu-id="9067f-203">Yes</span></span> | <span data-ttu-id="9067f-204">Oui</span><span class="sxs-lookup"><span data-stu-id="9067f-204">Yes</span></span> | <span data-ttu-id="9067f-205">Oui</span><span class="sxs-lookup"><span data-stu-id="9067f-205">Yes</span></span> |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><span data-ttu-id="9067f-206">Exemple :</span><span class="sxs-lookup"><span data-stu-id="9067f-206">Example:</span></span><br>`services.AddScoped<MyDep>();` | <span data-ttu-id="9067f-207">Oui</span><span class="sxs-lookup"><span data-stu-id="9067f-207">Yes</span></span> | <span data-ttu-id="9067f-208">Non</span><span class="sxs-lookup"><span data-stu-id="9067f-208">No</span></span> | <span data-ttu-id="9067f-209">Non</span><span class="sxs-lookup"><span data-stu-id="9067f-209">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(new {IMPLEMENTATION})`<br><span data-ttu-id="9067f-210">Exemples :</span><span class="sxs-lookup"><span data-stu-id="9067f-210">Examples:</span></span><br>`services.AddScoped<IMyDep>(new MyDep());`<br>`services.AddScoped<IMyDep>(new MyDep("A string!"));` | <span data-ttu-id="9067f-211">Non</span><span class="sxs-lookup"><span data-stu-id="9067f-211">No</span></span> | <span data-ttu-id="9067f-212">OUI</span><span class="sxs-lookup"><span data-stu-id="9067f-212">Yes</span></span> | <span data-ttu-id="9067f-213">Oui</span><span class="sxs-lookup"><span data-stu-id="9067f-213">Yes</span></span> |
| `Add{LIFETIME}(new {IMPLEMENTATION})`<br><span data-ttu-id="9067f-214">Exemples :</span><span class="sxs-lookup"><span data-stu-id="9067f-214">Examples:</span></span><br>`services.AddScoped(new MyDep());`<br>`services.AddScoped(new MyDep("A string!"));` | <span data-ttu-id="9067f-215">Non</span><span class="sxs-lookup"><span data-stu-id="9067f-215">No</span></span> | <span data-ttu-id="9067f-216">Non</span><span class="sxs-lookup"><span data-stu-id="9067f-216">No</span></span> | <span data-ttu-id="9067f-217">Oui</span><span class="sxs-lookup"><span data-stu-id="9067f-217">Yes</span></span> |

<span data-ttu-id="9067f-218">Pour plus d’informations sur la suppression de type, consultez la section [Suppression des services](#disposal-of-services).</span><span class="sxs-lookup"><span data-stu-id="9067f-218">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="9067f-219">Un scénario courant d’implémentations multiples est la [simulation de types à des fins de test](xref:test/integration-tests#inject-mock-services).</span><span class="sxs-lookup"><span data-stu-id="9067f-219">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="9067f-220">Les méthodes `TryAdd{LIFETIME}` inscrivent uniquement le service si aucune implémentation n’est inscrite.</span><span class="sxs-lookup"><span data-stu-id="9067f-220">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="9067f-221">Dans l’exemple suivant, la première ligne inscrit `MyDependency` pour `IMyDependency`.</span><span class="sxs-lookup"><span data-stu-id="9067f-221">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="9067f-222">La deuxième ligne n’a aucun effet car `IMyDependency` a déjà une implémentation inscrite :</span><span class="sxs-lookup"><span data-stu-id="9067f-222">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="9067f-223">Pour plus d'informations, voir :</span><span class="sxs-lookup"><span data-stu-id="9067f-223">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="9067f-224">Les méthodes [TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) inscrivent uniquement le service en l’absence d’une implémentation *du même type*.</span><span class="sxs-lookup"><span data-stu-id="9067f-224">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="9067f-225">Plusieurs services sont résolus par le biais de `IEnumerable<{SERVICE}>`.</span><span class="sxs-lookup"><span data-stu-id="9067f-225">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="9067f-226">Lors de l’inscription de services, le développeur ne souhaite ajouter une instance que si une instance du même type n’a pas déjà été ajoutée.</span><span class="sxs-lookup"><span data-stu-id="9067f-226">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="9067f-227">En général, cette méthode est utilisée par les créateurs de bibliothèque pour éviter d’inscrire deux copies d’une instance dans le conteneur.</span><span class="sxs-lookup"><span data-stu-id="9067f-227">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="9067f-228">Dans l’exemple suivant, la première ligne inscrit `MyDep` pour `IMyDep1`.</span><span class="sxs-lookup"><span data-stu-id="9067f-228">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="9067f-229">La deuxième ligne inscrit `MyDep` pour `IMyDep2`.</span><span class="sxs-lookup"><span data-stu-id="9067f-229">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="9067f-230">La troisième ligne n’a aucun effet car `IMyDep1` a déjà une implémentation inscrite de `MyDep` :</span><span class="sxs-lookup"><span data-stu-id="9067f-230">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="9067f-231">Comportement d’injection de constructeurs</span><span class="sxs-lookup"><span data-stu-id="9067f-231">Constructor injection behavior</span></span>

<span data-ttu-id="9067f-232">Les services peuvent être résolus par deux mécanismes :</span><span class="sxs-lookup"><span data-stu-id="9067f-232">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="9067f-233"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Autorise la création d’objet sans inscription du service dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="9067f-233"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="9067f-234">`ActivatorUtilities` est utilisé avec les abstractions orientées utilisateur, telles que les Tag Helpers, les contrôleurs MVC et les classeurs de modèles.</span><span class="sxs-lookup"><span data-stu-id="9067f-234">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="9067f-235">Les constructeurs peuvent accepter des arguments qui ne sont pas fournis par l’injection de dépendances, mais les arguments doivent affecter des valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="9067f-235">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="9067f-236">Lorsque des services sont résolus par `IServiceProvider` ou `ActivatorUtilities`, l’injection de constructeurs exige un constructeur *public*.</span><span class="sxs-lookup"><span data-stu-id="9067f-236">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="9067f-237">Lorsque des services sont résolus par `ActivatorUtilities`, l’injection de constructeurs exige qu’un seul constructeur applicable existe.</span><span class="sxs-lookup"><span data-stu-id="9067f-237">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exists.</span></span> <span data-ttu-id="9067f-238">Les surcharges de constructeurs sont prises en charge, mais une seule peut exister dont les arguments peuvent tous être satisfaits par l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="9067f-238">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="9067f-239">Contextes Entity Framework</span><span class="sxs-lookup"><span data-stu-id="9067f-239">Entity Framework contexts</span></span>

<span data-ttu-id="9067f-240">Les contextes Entity Framework sont généralement ajoutés au conteneur de service en utilisant la [durée de vie limitée](#service-lifetimes), car la portée des opérations de base de données d’application web est normalement limitée à la requête du client.</span><span class="sxs-lookup"><span data-stu-id="9067f-240">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="9067f-241">La durée de vie par défaut est limitée si aucune durée de vie n’est spécifiée par une surcharge [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) lors de l’inscription du contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="9067f-241">The default lifetime is scoped if a lifetime isn't specified by an [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) overload when registering the database context.</span></span> <span data-ttu-id="9067f-242">Un service d’une durée de vie donnée ne doit pas utiliser un contexte de base de données dont la durée de vie est plus courte que celle du service.</span><span class="sxs-lookup"><span data-stu-id="9067f-242">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="9067f-243">Options de durée de vie et d’inscription</span><span class="sxs-lookup"><span data-stu-id="9067f-243">Lifetime and registration options</span></span>

<span data-ttu-id="9067f-244">Pour illustrer la différence entre les options de durée de vie et d’inscription, considérez les interfaces suivantes qui représentent des tâches en tant qu’opération avec un identificateur unique, `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="9067f-244">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="9067f-245">Selon la façon dont la durée de vie d’un service d’opérations est configurée pour les interfaces suivantes, le conteneur fournit la même instance ou une instance différente du service lorsqu’une classe le demande :</span><span class="sxs-lookup"><span data-stu-id="9067f-245">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

<span data-ttu-id="9067f-246">Les interfaces sont implémentées dans la classe `Operation`.</span><span class="sxs-lookup"><span data-stu-id="9067f-246">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="9067f-247">Le constructeur `Operation` génère un GUID s’il n’est pas fourni :</span><span class="sxs-lookup"><span data-stu-id="9067f-247">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

<span data-ttu-id="9067f-248">Un `OperationService` est inscrit, dépendant de chacun des autres types `Operation`.</span><span class="sxs-lookup"><span data-stu-id="9067f-248">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="9067f-249">Lorsque `OperationService` est demandé via l’injection de dépendances, il reçoit une nouvelle instance de chaque service ou une instance existante en fonction de la durée de vie du service dépendant.</span><span class="sxs-lookup"><span data-stu-id="9067f-249">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="9067f-250">Quand des services temporaires sont créés à la demande à partir du conteneur, le `OperationId` du service `IOperationTransient` est différent du `OperationId` de `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="9067f-250">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="9067f-251">`OperationService` reçoit une nouvelle instance de la classe `IOperationTransient`.</span><span class="sxs-lookup"><span data-stu-id="9067f-251">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="9067f-252">La nouvelle instance génère un autre `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="9067f-252">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="9067f-253">Quand des services délimités sont créés pour chaque requête de client, le `OperationId` du `IOperationScoped` service est identique à celui de `OperationService` au sein d’une requête de client.</span><span class="sxs-lookup"><span data-stu-id="9067f-253">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="9067f-254">Entre les requêtes de client, les deux services partagent une valeur `OperationId` différente.</span><span class="sxs-lookup"><span data-stu-id="9067f-254">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="9067f-255">Quand des services singleton et d’instances singleton sont créés une fois et utilisés sur toutes les requêtes de client et tous les services, le `OperationId` est constant entre toutes les requêtes de service.</span><span class="sxs-lookup"><span data-stu-id="9067f-255">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

<span data-ttu-id="9067f-256">Dans `Startup.ConfigureServices`, chaque type est ajouté au conteneur en fonction de sa durée de vie nommée :</span><span class="sxs-lookup"><span data-stu-id="9067f-256">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

<span data-ttu-id="9067f-257">Le service `IOperationSingletonInstance` utilise une instance spécifique avec un ID connu `Guid.Empty`.</span><span class="sxs-lookup"><span data-stu-id="9067f-257">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="9067f-258">L’utilisation de ce type est facilement identifiable (son GUID n’affiche que des zéros).</span><span class="sxs-lookup"><span data-stu-id="9067f-258">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="9067f-259">L’exemple d’application montre les durées de vie des objets au sein et entre des requêtes individuelles.</span><span class="sxs-lookup"><span data-stu-id="9067f-259">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="9067f-260">L’exemple d’application `IndexModel` demande chaque type `IOperation` et `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="9067f-260">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="9067f-261">La page affiche ensuite l’ensemble de la classe du modèle de page et des valeurs `OperationId` du service via des assignations de propriété :</span><span class="sxs-lookup"><span data-stu-id="9067f-261">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

<span data-ttu-id="9067f-262">Les deux sorties suivantes montrent les résultats de deux requêtes :</span><span class="sxs-lookup"><span data-stu-id="9067f-262">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="9067f-263">**Première requête :**</span><span class="sxs-lookup"><span data-stu-id="9067f-263">**First request:**</span></span>

<span data-ttu-id="9067f-264">Opérations du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="9067f-264">Controller operations:</span></span>

<span data-ttu-id="9067f-265">Transient: d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="9067f-265">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="9067f-266">Délimité : 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="9067f-266">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="9067f-267">Singleton : 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="9067f-267">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="9067f-268">Instance : 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="9067f-268">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="9067f-269">Opérations `OperationService` :</span><span class="sxs-lookup"><span data-stu-id="9067f-269">`OperationService` operations:</span></span>

<span data-ttu-id="9067f-270">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="9067f-270">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="9067f-271">Délimité : 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="9067f-271">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="9067f-272">Singleton : 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="9067f-272">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="9067f-273">Instance : 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="9067f-273">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="9067f-274">**Deuxième requête :**</span><span class="sxs-lookup"><span data-stu-id="9067f-274">**Second request:**</span></span>

<span data-ttu-id="9067f-275">Opérations du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="9067f-275">Controller operations:</span></span>

<span data-ttu-id="9067f-276">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="9067f-276">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="9067f-277">Délimité : 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="9067f-277">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="9067f-278">Singleton : 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="9067f-278">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="9067f-279">Instance : 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="9067f-279">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="9067f-280">Opérations `OperationService` :</span><span class="sxs-lookup"><span data-stu-id="9067f-280">`OperationService` operations:</span></span>

<span data-ttu-id="9067f-281">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="9067f-281">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="9067f-282">Délimité : 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="9067f-282">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="9067f-283">Singleton : 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="9067f-283">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="9067f-284">Instance : 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="9067f-284">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="9067f-285">Observez les valeurs `OperationId` qui varient au sein d’une requête et entre les requêtes :</span><span class="sxs-lookup"><span data-stu-id="9067f-285">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="9067f-286">Les objets *Transient* sont toujours différents.</span><span class="sxs-lookup"><span data-stu-id="9067f-286">*Transient* objects are always different.</span></span> <span data-ttu-id="9067f-287">Les valeurs `OperationId` transitoires pour la première et la deuxième requêtes de client sont différentes pour les deux opérations `OperationService` et entre les requêtes de client.</span><span class="sxs-lookup"><span data-stu-id="9067f-287">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="9067f-288">Une nouvelle instance est fournie à chaque requête de service et requête de client.</span><span class="sxs-lookup"><span data-stu-id="9067f-288">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="9067f-289">Les objets *Scoped* sont les mêmes au sein d’une requête de client, mais ils diffèrent entre les requêtes de client.</span><span class="sxs-lookup"><span data-stu-id="9067f-289">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="9067f-290">Les objets *Singleton* sont les mêmes pour chaque objet et chaque requête, qu’une instance `Operation` soit fournie dans `Startup.ConfigureServices` ou non.</span><span class="sxs-lookup"><span data-stu-id="9067f-290">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="9067f-291">Appeler des services à partir de Main</span><span class="sxs-lookup"><span data-stu-id="9067f-291">Call services from main</span></span>

<span data-ttu-id="9067f-292">Créez un <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> avec [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) pour résoudre un service délimité dans l’étendue de l’application.</span><span class="sxs-lookup"><span data-stu-id="9067f-292">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="9067f-293">Cette approche est pratique pour accéder à un service Scoped au démarrage pour exécuter des tâches d’initialisation.</span><span class="sxs-lookup"><span data-stu-id="9067f-293">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="9067f-294">L’exemple suivant montre comment obtenir un contexte pour `MyScopedService` dans `Program.Main` :</span><span class="sxs-lookup"><span data-stu-id="9067f-294">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="9067f-295">Validation de l’étendue</span><span class="sxs-lookup"><span data-stu-id="9067f-295">Scope validation</span></span>

<span data-ttu-id="9067f-296">Quand l’application s’exécute dans l’environnement de développement, le fournisseur de services par défaut effectue des contrôles pour vérifier que :</span><span class="sxs-lookup"><span data-stu-id="9067f-296">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="9067f-297">Les services Scoped ne sont pas résolus directement ou indirectement à partir du fournisseur de services racine.</span><span class="sxs-lookup"><span data-stu-id="9067f-297">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="9067f-298">Les services Scoped ne sont pas directement ou indirectement injectés dans des singletons.</span><span class="sxs-lookup"><span data-stu-id="9067f-298">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="9067f-299">Le fournisseur de services racine est créé quand <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> est appelé.</span><span class="sxs-lookup"><span data-stu-id="9067f-299">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="9067f-300">La durée de vie du fournisseur de services racine correspond à la durée de vie de l’application/du serveur quand le fournisseur démarre avec l’application et qu’il est supprimé quand l’application s’arrête.</span><span class="sxs-lookup"><span data-stu-id="9067f-300">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="9067f-301">Les services Scoped sont supprimés par le conteneur qui les a créés.</span><span class="sxs-lookup"><span data-stu-id="9067f-301">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="9067f-302">Si un service Scoped est créé dans le conteneur racine, la durée de vie du service est promue en singleton, car elle est supprimée par le conteneur racine seulement quand l’application/le serveur est arrêté.</span><span class="sxs-lookup"><span data-stu-id="9067f-302">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="9067f-303">La validation des étendues du service permet de traiter ces situations quand `BuildServiceProvider` est appelé.</span><span class="sxs-lookup"><span data-stu-id="9067f-303">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="9067f-304">Pour plus d’informations, consultez <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="9067f-304">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="9067f-305">Services de requête</span><span class="sxs-lookup"><span data-stu-id="9067f-305">Request Services</span></span>

<span data-ttu-id="9067f-306">Les services disponibles au sein d’une requête ASP.NET Core à partir de `HttpContext` sont exposés par le biais de la collection [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices).</span><span class="sxs-lookup"><span data-stu-id="9067f-306">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="9067f-307">Les services de requête représentent les services configurés et demandés dans le cadre de l’application.</span><span class="sxs-lookup"><span data-stu-id="9067f-307">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="9067f-308">Lorsque les objets spécifient des dépendances, ceux-ci sont satisfaits par les types trouvés dans `RequestServices`, pas dans `ApplicationServices`.</span><span class="sxs-lookup"><span data-stu-id="9067f-308">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="9067f-309">En règle générale, l’application ne doit pas utiliser directement ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="9067f-309">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="9067f-310">Demandez plutôt les types nécessaires à la classe via des constructeurs de classe et autorisez le framework à injecter les dépendances.</span><span class="sxs-lookup"><span data-stu-id="9067f-310">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="9067f-311">Cela génère des classes qui sont plus faciles à tester.</span><span class="sxs-lookup"><span data-stu-id="9067f-311">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="9067f-312">Préférez demander des dépendances en tant que paramètres de constructeur plutôt qu’accéder à la collection `RequestServices`.</span><span class="sxs-lookup"><span data-stu-id="9067f-312">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="9067f-313">Conception de services pour l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="9067f-313">Design services for dependency injection</span></span>

<span data-ttu-id="9067f-314">Les bonnes pratiques permettent de :</span><span class="sxs-lookup"><span data-stu-id="9067f-314">Best practices are to:</span></span>

* <span data-ttu-id="9067f-315">Concevoir des services afin d’utiliser l’injection de dépendances pour obtenir leurs dépendances.</span><span class="sxs-lookup"><span data-stu-id="9067f-315">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="9067f-316">Éviter les appels de méthode statiques avec état.</span><span class="sxs-lookup"><span data-stu-id="9067f-316">Avoid stateful, static method calls.</span></span>
* <span data-ttu-id="9067f-317">Éviter une instanciation directe de classes dépendantes au sein de services.</span><span class="sxs-lookup"><span data-stu-id="9067f-317">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="9067f-318">L’instanciation directe associe le code à une implémentation particulière.</span><span class="sxs-lookup"><span data-stu-id="9067f-318">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="9067f-319">Limiter la taille des classes d’application, faire en sorte qu’elles soient bien factorisées et facilement testées.</span><span class="sxs-lookup"><span data-stu-id="9067f-319">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="9067f-320">Si une classe semble avoir trop de dépendances injectées, cela signifie généralement que la classe a trop de responsabilités et viole le [principe de responsabilité unique](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span><span class="sxs-lookup"><span data-stu-id="9067f-320">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="9067f-321">Essayez de refactoriser la classe en déplaçant certaines de ses responsabilités dans une nouvelle classe.</span><span class="sxs-lookup"><span data-stu-id="9067f-321">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="9067f-322">N’oubliez pas que les classes du modèle de page Razor Pages et les classes du contrôleur MVC doivent se concentrer sur les problèmes d’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="9067f-322">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="9067f-323">Les règles d’entreprise et les détails d’implémentation de l’accès aux données doivent être conservés dans les classes appropriées à ces [préoccupations distinctes](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span><span class="sxs-lookup"><span data-stu-id="9067f-323">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="9067f-324">Suppression des services</span><span class="sxs-lookup"><span data-stu-id="9067f-324">Disposal of services</span></span>

<span data-ttu-id="9067f-325">Le conteneur appelle <xref:System.IDisposable.Dispose*> pour les types <xref:System.IDisposable> qu’il crée.</span><span class="sxs-lookup"><span data-stu-id="9067f-325">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="9067f-326">Si une instance est ajoutée au conteneur par le code utilisateur, elle n’est pas supprimée automatiquement.</span><span class="sxs-lookup"><span data-stu-id="9067f-326">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

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

## <a name="default-service-container-replacement"></a><span data-ttu-id="9067f-327">Remplacement de conteneur de services par défaut</span><span class="sxs-lookup"><span data-stu-id="9067f-327">Default service container replacement</span></span>

<span data-ttu-id="9067f-328">Le conteneur de services intégré a vocation à répondre aux besoins du framework et de la plupart des applications consommatrices.</span><span class="sxs-lookup"><span data-stu-id="9067f-328">The built-in service container is meant to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="9067f-329">Nous vous recommandons d’utiliser le conteneur intégré, sauf si vous avez besoin d’une fonctionnalité spécifique qu’il ne prend pas en charge.</span><span class="sxs-lookup"><span data-stu-id="9067f-329">We recommend using the built-in container unless you need a specific feature that it doesn't support.</span></span> <span data-ttu-id="9067f-330">Voici quelques-unes des fonctionnalités prises en charge dans des conteneurs tiers qui sont introuvables dans le conteneur intégré :</span><span class="sxs-lookup"><span data-stu-id="9067f-330">Some of the features supported in 3rd party containers not found in the built-in container:</span></span>

* <span data-ttu-id="9067f-331">Injection de propriétés</span><span class="sxs-lookup"><span data-stu-id="9067f-331">Property injection</span></span>
* <span data-ttu-id="9067f-332">Injection en fonction du nom</span><span class="sxs-lookup"><span data-stu-id="9067f-332">Injection based on name</span></span>
* <span data-ttu-id="9067f-333">Conteneurs enfants</span><span class="sxs-lookup"><span data-stu-id="9067f-333">Child containers</span></span>
* <span data-ttu-id="9067f-334">Gestion personnalisée de la durée de vie</span><span class="sxs-lookup"><span data-stu-id="9067f-334">Custom lifetime management</span></span>
* <span data-ttu-id="9067f-335">`Func<T>` prend en charge l’initialisation tardive</span><span class="sxs-lookup"><span data-stu-id="9067f-335">`Func<T>` support for lazy initialization</span></span>

<span data-ttu-id="9067f-336">Pour obtenir une liste de certains des conteneurs qui prennent en charge des adaptateurs, consultez le [fichier readme.md relatif à l’injection de dépendances](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="9067f-336">See the [Dependency Injection readme.md file](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) for a list of some of the containers that support adapters.</span></span>

<span data-ttu-id="9067f-337">L’exemple suivant remplace le conteneur intégré par [Autofac](https://autofac.org/) :</span><span class="sxs-lookup"><span data-stu-id="9067f-337">The following sample replaces the built-in container with [Autofac](https://autofac.org/):</span></span>

* <span data-ttu-id="9067f-338">Installez les packages de conteneurs appropriés :</span><span class="sxs-lookup"><span data-stu-id="9067f-338">Install the appropriate container package(s):</span></span>

  * [<span data-ttu-id="9067f-339">Autofac</span><span class="sxs-lookup"><span data-stu-id="9067f-339">Autofac</span></span>](https://www.nuget.org/packages/Autofac/)
  * [<span data-ttu-id="9067f-340">Autofac.Extensions.DependencyInjection</span><span class="sxs-lookup"><span data-stu-id="9067f-340">Autofac.Extensions.DependencyInjection</span></span>](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)

* <span data-ttu-id="9067f-341">Configurez le conteneur dans `Startup.ConfigureServices` et retournez un `IServiceProvider` :</span><span class="sxs-lookup"><span data-stu-id="9067f-341">Configure the container in `Startup.ConfigureServices` and return an `IServiceProvider`:</span></span>

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

    <span data-ttu-id="9067f-342">Pour utiliser un conteneur tiers, `Startup.ConfigureServices` doit retourner `IServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="9067f-342">To use a 3rd party container, `Startup.ConfigureServices` must return `IServiceProvider`.</span></span>

* <span data-ttu-id="9067f-343">Configurez Autofac dans `DefaultModule` :</span><span class="sxs-lookup"><span data-stu-id="9067f-343">Configure Autofac in `DefaultModule`:</span></span>

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

<span data-ttu-id="9067f-344">Au moment de l’exécution, Autofac est utilisé pour résoudre les types et injecter des dépendances.</span><span class="sxs-lookup"><span data-stu-id="9067f-344">At runtime, Autofac is used to resolve types and inject dependencies.</span></span> <span data-ttu-id="9067f-345">Pour en savoir plus sur l’utilisation d’Autofac avec ASP.NET Core, consultez la [documentation Autofac](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span><span class="sxs-lookup"><span data-stu-id="9067f-345">To learn more about using Autofac with ASP.NET Core, see the [Autofac documentation](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="9067f-346">Sécurité des threads</span><span class="sxs-lookup"><span data-stu-id="9067f-346">Thread safety</span></span>

<span data-ttu-id="9067f-347">Créez des services singleton thread-safe.</span><span class="sxs-lookup"><span data-stu-id="9067f-347">Create thread-safe singleton services.</span></span> <span data-ttu-id="9067f-348">Si un service singleton a une dépendance vis-à-vis d’un service Transient, ce dernier peut également nécessiter la cohérence de thread, en fonction de la manière dont il est utilisé par le singleton.</span><span class="sxs-lookup"><span data-stu-id="9067f-348">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="9067f-349">La méthode de fabrique d’un service individuel, comme le deuxième argument de [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), n’a pas besoin d’être thread-safe.</span><span class="sxs-lookup"><span data-stu-id="9067f-349">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="9067f-350">Comme un constructeur de type (`static`), elle est forcément appelée une fois par un seul thread.</span><span class="sxs-lookup"><span data-stu-id="9067f-350">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="9067f-351">Recommandations</span><span class="sxs-lookup"><span data-stu-id="9067f-351">Recommendations</span></span>

* <span data-ttu-id="9067f-352">La résolution de service basée sur `async/await` et `Task` n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="9067f-352">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="9067f-353">C# ne prend pas en charge les constructeurs asynchrones ; par conséquent, le modèle recommandé consiste à utiliser des méthodes asynchrones après avoir résolu le service de façon synchrone.</span><span class="sxs-lookup"><span data-stu-id="9067f-353">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="9067f-354">Évitez de stocker des données et des configurations directement dans le conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="9067f-354">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="9067f-355">Par exemple, le panier d’achat d’un utilisateur ne doit en général pas être ajouté au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="9067f-355">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="9067f-356">La configuration doit utiliser le [modèle d’options](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="9067f-356">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="9067f-357">De même, évitez les objets « conteneurs de données » qui n’existent que pour autoriser l’accès à un autre objet.</span><span class="sxs-lookup"><span data-stu-id="9067f-357">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="9067f-358">Il est préférable de demander l’élément réel par le biais de l’injection de dépendance.</span><span class="sxs-lookup"><span data-stu-id="9067f-358">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="9067f-359">Évitez l’accès statique aux services (par exemple avec [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) à utiliser ailleurs).</span><span class="sxs-lookup"><span data-stu-id="9067f-359">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="9067f-360">Évitez d’utiliser le *modèle de localisation de service*.</span><span class="sxs-lookup"><span data-stu-id="9067f-360">Avoid using the *service locator pattern*.</span></span> <span data-ttu-id="9067f-361">Par exemple, n’appelez pas <xref:System.IServiceProvider.GetService*> pour obtenir une instance de service si vous pouvez utiliser l’injection de dépendance à la place :</span><span class="sxs-lookup"><span data-stu-id="9067f-361">For example, don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

  <span data-ttu-id="9067f-362">**Incorrect :**</span><span class="sxs-lookup"><span data-stu-id="9067f-362">**Incorrect:**</span></span>

  ```csharp
  public class MyClass()
  {
      public void MyMethod()
      {
          var optionsMonitor = 
              _services.GetService<IOptionsMonitor<MyOptions>>();
          var option = optionsMonitor.CurrentValue.Option;

          ...
      }
  }
  ```

  <span data-ttu-id="9067f-363">**Correct** :</span><span class="sxs-lookup"><span data-stu-id="9067f-363">**Correct**:</span></span>

  ```csharp
  public class MyClass
  {
      private readonly IOptionsMonitor<MyOptions> _optionsMonitor;

      public MyClass(IOptionsMonitor<MyOptions> optionsMonitor)
      {
          _optionsMonitor = optionsMonitor;
      }

      public void MyMethod()
      {
          var option = _optionsMonitor.CurrentValue.Option;

          ...
      }
  }
  ```

* <span data-ttu-id="9067f-364">Une autre variante du localisateur de service à éviter est l’injection d’une fabrique qui résout les dépendances au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="9067f-364">Another service locator variation to avoid is injecting a factory that resolves dependencies at runtime.</span></span> <span data-ttu-id="9067f-365">Ces deux pratiques combinent des stratégies [Inversion de contrôle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion).</span><span class="sxs-lookup"><span data-stu-id="9067f-365">Both of these practices mix [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

* <span data-ttu-id="9067f-366">Évitez l’accès statique à `HttpContext` (par exemple, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span><span class="sxs-lookup"><span data-stu-id="9067f-366">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="9067f-367">Comme pour toutes les recommandations, vous pouvez vous trouver dans des situations où il est nécessaire d’ignorer une recommandation.</span><span class="sxs-lookup"><span data-stu-id="9067f-367">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="9067f-368">Les exceptions sont rares et représentent principalement des cas spéciaux dans le framework lui-même.</span><span class="sxs-lookup"><span data-stu-id="9067f-368">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="9067f-369">L’injection de dépendance constitue une *alternative* aux modèles d’accès aux objets statiques/globaux.</span><span class="sxs-lookup"><span data-stu-id="9067f-369">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="9067f-370">Il est possible que vous ne bénéficiez pas des avantages de l’injection de dépendances si vous la combinez avec l’accès aux objets statiques.</span><span class="sxs-lookup"><span data-stu-id="9067f-370">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9067f-371">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9067f-371">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="9067f-372">Écrire un code clair dans ASP.NET Core avec l’injection de dépendance (MSDN)</span><span class="sxs-lookup"><span data-stu-id="9067f-372">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="9067f-373">Principe des dépendances explicites</span><span class="sxs-lookup"><span data-stu-id="9067f-373">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [<span data-ttu-id="9067f-374">Conteneurs d’inversion de contrôle et modèle d’injection de dépendances (Martin Fowler)</span><span class="sxs-lookup"><span data-stu-id="9067f-374">Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
* [<span data-ttu-id="9067f-375">Comment inscrire un service avec plusieurs interfaces dans l’injection de dépendance ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9067f-375">How to register a service with multiple interfaces in ASP.NET Core DI</span></span>](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)
