---
title: Injection de dépendances dans ASP.NET Core
author: guardrex
description: Découvrez comment ASP.NET Core implémente l’injection de dépendances et comment l’utiliser.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: fundamentals/dependency-injection
ms.openlocfilehash: d9eb6a01e096c7e8cbcb0979e24331a8d5316a14
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207652"
---
# <a name="dependency-injection-in-aspnet-core"></a>Injection de dépendances dans ASP.NET Core

Par [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com) et [Luke Latham](https://github.com/guardrex)

ASP.NET Core prend en charge le modèle de conception de logiciel avec injection de dépendances (DI), une technique permettant d’obtenir une [inversion de contrôle (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) entre les classes et leurs dépendances.

Pour plus d’informations spécifiques à l’injection de dépendances au sein des contrôleurs MVC, consultez <xref:mvc/controllers/dependency-injection>.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="overview-of-dependency-injection"></a>Vue d’ensemble de l’injection de dépendances

Une *dépendance* est un objet qui nécessite un autre objet. Examinez la classe `MyDependency` suivante avec une méthode `WriteMessage` dont dépendent d’autres classes dans une application :

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

::: moniker range=">= aspnetcore-2.1"

Une instance de la classe `MyDependency` peut être créée pour rendre la méthode `WriteMessage` disponible pour une classe. La classe `MyDependency` est une dépendance de la classe `IndexModel` :

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

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

Une instance de la classe `MyDependency` peut être créée pour rendre la méthode `WriteMessage` disponible pour une classe. La classe `MyDependency` est une dépendance de la classe `HomeController` :

```csharp
public class HomeController : Controller
{
    MyDependency _dependency = new MyDependency();

    public async Task<IActionResult> Index()
    {
        await _dependency.WriteMessage(
            "HomeController.Index created this message.");

        return View();
    }
}
```

::: moniker-end

La classe est créee et dépend directement de l’instance `MyDependency`. Les dépendances de code (comme l’exemple précédent) posent problème et doivent être évitées pour les raisons suivantes :

* Pour remplacer `MyDependency` par une autre implémentation, la classe doit être modifiée.
* Si `MyDependency` possède des dépendances, elles doivent être configurées par la classe. Dans un grand projet comportant plusieurs classes dépendant de `MyDependency`, le code de configuration est disséminé dans l’application.
* Cette implémentation complique le test unitaire. L’application doit utiliser une classe `MyDependency` fictive ou stub, ce qui est impossible avec cette approche.

L’injection de dépendances résout ces problèmes via :

* L’utilisation d’une interface pour abstraire l’implémentation des dépendances.
* L’inscription de la dépendance dans un conteneur de service. ASP.NET Core fournit un conteneur de service intégré, [IServiceProvider](/dotnet/api/system.iserviceprovider). Les services sont inscrits dans la méthode `Startup.ConfigureServices` de l’application.
* *Injection* du service dans le constructeur de la classe où il est utilisé. Le framework prend la responsabilité de la création d’une instance de la dépendance et de sa suppression lorsqu’elle n’est plus nécessaire.

Dans l’[exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), l’interface `IMyDependency` définit une méthode que le service fournit à l’application :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

Cette interface est implémentée par un type concret, `MyDependency` :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

`MyDependency` demande un [ILogger&lt;TCategoryName&gt; ](/dotnet/api/microsoft.extensions.logging.ilogger-1) dans son constructeur. Il n’est pas rare que l’injection de dépendances soit utilisée de manière chaînée. Dans ce cas, chaque dépendance demandée demande à son tour ses propres dépendances. Le conteneur résout les dépendances dans le graphique et retourne le service entièrement résolu. L’ensemble collectif de dépendances qui doivent être résolues est généralement appelé *arborescence des dépendances*, *graphique de dépendance* ou *graphique d’objet*.

`IMyDependency` et `ILogger<TCategoryName>` doivent être inscrits dans le conteneur de service. `IMyDependency` est inscrit dans `Startup.ConfigureServices`. `ILogger<TCategoryName>` est inscrit par l’infrastructure d’abstractions de journalisation. Il s’agit donc d’un [service fourni par le framework](#framework-provided-services) et inscrit par défaut par l’infrastructure.

Dans l’exemple d’application, le service `IMyDependency` est inscrit avec le type concret `MyDependency`. L’inscription ajuste la durée de vie du service à la durée de vie d’une requête unique. Les [durées de vie du service](#service-lifetimes) sont décrites plus loin dans cette rubrique.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

> [!NOTE]
> Chaque méthode d’extension `services.Add{SERVICE_NAME}` ajoute (et éventuellement configure) des services. Par exemple, `services.AddMvc()` ajoute les services dont Razor Pages et MVC ont besoin. Il est recommandé que les applications suivent cette convention. Placez les méthodes d’extension dans l’espace de noms [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) pour encapsuler des groupes d’inscriptions de service.

Si le constructeur du service exige une primitive, comme `string`, celle-ci peut être injectée à l’aide de la [configuration](xref:fundamentals/configuration/index) ou du [modèle d’options](xref:fundamentals/configuration/options) :

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

Une instance du service est demandée via le constructeur d’une classe dans laquelle le service est utilisé et assigné à un champ privé. Le champ est utilisé pour accéder au service en fonction des besoins tout au long de la classe.

Dans l’exemple d’application, l’instance `IMyDependency` est demandée et utilisée pour appeler la méthode `WriteMessage` du service :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/MyDependencyController.cs?name=snippet1&highlight=3,5-8,13-14)]

::: moniker-end

## <a name="framework-provided-services"></a>Services fournis par le framework

La méthode `Startup.ConfigureServices` est chargée de définir les services utilisés par l’application, notamment les fonctionnalités de plateforme comme Entity Framework Core et ASP.NET Core MVC. Au départ, la valeur `IServiceCollection` fournie à `ConfigureServices` a les services suivants définis (en fonction de [la manière dont l’hôte a été configuré](xref:fundamentals/host/index)) :

| Type de service | Durée de vie |
| ------------ | -------- |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | Transient |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | Singleton |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartup](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | Transient |
| [Microsoft.AspNetCore.Hosting.Server.IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | Singleton |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | Transient |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](/dotnet/api/microsoft.extensions.logging.ilogger) | Singleton |
| [Microsoft.Extensions.Logging.ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | Singleton |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | Singleton |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | Transient |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.ioptions-1) | Singleton |
| [System.Diagnostics.DiagnosticSource](/dotnet/core/api/system.diagnostics.diagnosticsource) | Singleton |
| [System.Diagnostics.DiagnosticListener](/dotnet/core/api/system.diagnostics.diagnosticlistener) | Singleton |

Lorsqu’une méthode d’extension de collection de services est disponible pour inscrire un service (et ses services dépendants, si nécessaire), la convention consiste à utiliser une seule méthode d’extension `Add{SERVICE_NAME}` pour inscrire tous les services requis par ce service. Le code suivant est un exemple montrant comment ajouter des services supplémentaires au conteneur en utilisant les méthodes d’extension [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity) et [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc) :

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

Pour plus d’informations, consultez la classe [ServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection) dans la documentation de l’API.

## <a name="service-lifetimes"></a>Durées de vie du service

Choisissez une durée de vie appropriée pour chaque service inscrit. Vous pouvez configurer les services ASP.NET Core avec les durées de vie suivantes :

**Transient**

Des services à durée de vie temporaire (Transient) sont créés chaque fois qu’ils sont demandés. Cette durée de vie convient parfaitement aux services légers et sans état.

**Scoped**

Les services à durée de vie délimitée (Scoped) sont créés une seule fois par requête.

> [!WARNING]
> Si vous utilisez un service Scoped dans un middleware, injectez le service dans la méthode `Invoke` ou `InvokeAsync`. Ne faites pas l’injection via l’injection du constructeur, car elle force le service à se comporter comme un singleton. Pour plus d'informations, consultez <xref:fundamentals/middleware/index>.

**Singleton**

Les services avec une durée de vie singleton sont créés la première fois qu’ils sont demandés (ou lorsque `ConfigureServices` est exécuté et qu’une instance est spécifiée avec l’inscription du service). Chaque requête ultérieure utilise la même instance. Si l’application exige un comportement singleton, il est recommandé d’autoriser le conteneur de service à gérer la durée de vie du service. N’implémentez pas le modèle de conception singleton et fournissez le code utilisateur pour gérer la durée de vie de l’objet dans la classe.

> [!WARNING]
> Il est dangereux de résoudre un service délimité depuis un singleton. L’état du service risque de ne pas être correct lors du traitement des requêtes suivantes.

### <a name="constructor-injection-behavior"></a>Comportement d’injection de constructeurs

Les services peuvent être résolus par deux mécanismes :

* `IServiceProvider`
* [ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; autorise la création d’objet sans inscription du service dans le conteneur d’injection de dépendances. `ActivatorUtilities` est utilisé avec les abstractions orientées utilisateur, telles que les Tag Helpers, les contrôleurs MVC et les classeurs de modèles.

Les constructeurs peuvent accepter des arguments qui ne sont pas fournis par l’injection de dépendances, mais les arguments doivent affecter des valeurs par défaut.

Lorsque des services sont résolus par `IServiceProvider` ou `ActivatorUtilities`, l’injection de constructeurs exige un constructeur *public*.

Lorsque des services sont résolus par `ActivatorUtilities`, l’injection de constructeurs exige qu’un seul constructeur applicable existe. Les surcharges de constructeurs sont prises en charge, mais une seule peut exister dont les arguments peuvent tous être satisfaits par l’injection de dépendances.

## <a name="entity-framework-contexts"></a>Contextes Entity Framework

Vous devez ajouter des contextes Entity Framework au conteneur de services en utilisant la durée de vie délimitée. Ceci est géré automatiquement par un appel à la méthode [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) lors de l’inscription du contexte de base de données. Les services qui utilisent le contexte de base de données doivent également utiliser la durée de vie délimitée.

## <a name="lifetime-and-registration-options"></a>Options de durée de vie et d’inscription

Pour illustrer la différence entre les options de durée de vie et d’inscription, considérez les interfaces suivantes qui représentent des tâches en tant qu’opération avec un identificateur unique, `OperationId`. Selon la façon dont la durée de vie d’un service d’opérations est configurée pour les interfaces suivantes, le conteneur fournit la même instance ou une instance différente du service lorsqu’une classe le demande :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

Les interfaces sont implémentées dans la classe `Operation`. Le constructeur `Operation` génère un GUID s’il n’est pas fourni :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

Un `OperationService` est inscrit, dépendant de chacun des autres types `Operation`. Lorsque `OperationService` est demandé via l’injection de dépendances, il reçoit une nouvelle instance de chaque service ou une instance existante en fonction de la durée de vie du service dépendant.

* Si des services temporaires sont créés à la demande, le `OperationId` du service `IOperationTransient` est différent du `OperationId` de `OperationService`. `OperationService` reçoit une nouvelle instance de la classe `IOperationTransient`. La nouvelle instance génère un autre `OperationId`.
* Si des services délimités sont créés pour chaque requête, le `OperationId` du `IOperationScoped` service est identique à celui de `OperationService` au sein d’une requête. Entre les requêtes, les deux services partagent une valeur `OperationId` différente.
* Si des services singleton et d’instances singleton sont créés une fois et utilisés sur toutes les requêtes et tous les services, le `OperationId` est constant entre toutes les requêtes de service.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

Dans `Startup.ConfigureServices`, chaque type est ajouté au conteneur en fonction de sa durée de vie nommée :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=12-15,18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

Le service `IOperationSingletonInstance` utilise une instance spécifique avec un ID connu `Guid.Empty`. L’utilisation de ce type est facilement identifiable (son GUID n’affiche que des zéros).

::: moniker range=">= aspnetcore-2.1"

L’exemple d’application montre les durées de vie des objets au sein et entre des requêtes individuelles. L’exemple d’application `IndexModel` demande chaque type `IOperation` et `OperationService`. La page affiche ensuite l’ensemble de la classe du modèle de page et des valeurs `OperationId` du service via des assignations de propriété :

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

L’exemple d’application montre les durées de vie des objets au sein et entre des requêtes individuelles. L’exemple d’application inclut un `OperationsController` qui demande chaque type `IOperation` et `OperationService`. L’action `Index` définit les services dans `ViewBag` pour l’affichage des valeurs `OperationId` du service :

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/OperationsController.cs?name=snippet1)]

::: moniker-end

Les deux sorties suivantes montrent les résultats de deux requêtes :

**Première requête :**

Opérations du contrôleur :

Transient: d233e165-f417-469b-a866-1cf1935d2518  
Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Instance: 00000000-0000-0000-0000-000000000000

Opérations `OperationService` :

Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64  
Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Instance: 00000000-0000-0000-0000-000000000000

**Deuxième requête :**

Opérations du contrôleur :

Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0  
Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Instance: 00000000-0000-0000-0000-000000000000

Opérations `OperationService` :

Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf  
Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Instance: 00000000-0000-0000-0000-000000000000

Observez les valeurs `OperationId` qui varient au sein d’une requête et entre les requêtes :

* Les objets *Transient* sont toujours différents. Notez que les valeurs `OperationId` transitoires pour la première et la deuxième demande sont différentes pour les deux opérations `OperationService` et entre les requêtes. Une nouvelle instance est fournie à chaque service et requête.
* Les objets *Scoped* sont les mêmes au sein d’une requête, mais ils diffèrent entre les requêtes.
* Les objets *Singleton* sont les mêmes pour chaque objet et chaque requête, qu’une instance `Operation` soit fournie dans `ConfigureServices` ou non.

## <a name="call-services-from-main"></a>Appeler des services à partir de Main

Créez un [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) avec [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) pour résoudre un service Scoped dans le scope de l’application. Cette approche est pratique pour accéder à un service Scoped au démarrage pour exécuter des tâches d’initialisation. L’exemple suivant montre comment obtenir un contexte pour `MyScopedService` dans `Program.Main` :

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

## <a name="scope-validation"></a>Validation de l’étendue

::: moniker range=">= aspnetcore-2.0"

Quand l’application s’exécute dans l’environnement de développement, le fournisseur de services par défaut effectue des contrôles pour vérifier que :

* Les services Scoped ne sont pas résolus directement ou indirectement à partir du fournisseur de services racine.
* Les services Scoped ne sont pas directement ou indirectement injectés dans des singletons.

::: moniker-end

Le fournisseur de services racine est créé quand [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) est appelé. La durée de vie du fournisseur de services racine correspond à la durée de vie de l’application/du serveur quand le fournisseur démarre avec l’application et qu’il est supprimé quand l’application s’arrête.

Les services Scoped sont supprimés par le conteneur qui les a créés. Si un service Scoped est créé dans le conteneur racine, la durée de vie du service est promue en singleton, car elle est supprimée par le conteneur racine seulement quand l’application/le serveur est arrêté. La validation des étendues du service permet de traiter ces situations quand `BuildServiceProvider` est appelé.

Pour plus d'informations, consultez <xref:fundamentals/host/web-host#scope-validation>.

## <a name="request-services"></a>Services de requête

Les services disponibles au sein d’une requête ASP.NET Core à partir de `HttpContext` sont exposés par le biais de la collection [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices).

Les services de requête représentent les services configurés et demandés dans le cadre de l’application. Lorsque les objets spécifient des dépendances, ceux-ci sont satisfaits par les types trouvés dans `RequestServices`, pas dans `ApplicationServices`.

En règle générale, l’application ne doit pas utiliser directement ces propriétés. Demandez plutôt les types nécessaires à la classe via des constructeurs de classe et autorisez le framework à injecter les dépendances. Vous obtenez ainsi des classes plus faciles à tester (consultez les rubriques [Tester et déboguer](xref:test/index)).

> [!NOTE]
> Préférez demander des dépendances en tant que paramètres de constructeur plutôt qu’accéder à la collection `RequestServices`.

## <a name="design-services-for-dependency-injection"></a>Conception de services pour l’injection de dépendances

Les bonnes pratiques permettent de :

* Concevoir des services afin d’utiliser l’injection de dépendances pour obtenir leurs dépendances.
* Éviter les appels de méthode statiques avec état (une pratique appelée [static cling](https://deviq.com/static-cling/)).
* Éviter une instanciation directe de classes dépendantes au sein de services. L’instanciation directe associe le code à une implémentation particulière.

En suivant les [principes SOLID de conception orientée objet](https://deviq.com/solid/), les classes d’application ont naturellement tendance à être petites, bien factorisées et facilement testées.

Si une classe semble avoir trop de dépendances injectées, cela signifie généralement que la classe a trop de responsabilités et viole le [principe de responsabilité unique](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility). Essayez de refactoriser la classe en déplaçant certaines de ses responsabilités dans une nouvelle classe. N’oubliez pas que les classes du modèle de page Razor Pages et les classes du contrôleur MVC doivent se concentrer sur les problèmes d’interface utilisateur. Les règles d’entreprise et les détails d’implémentation de l’accès aux données doivent être conservés dans les classes appropriées à ces [préoccupations distinctes](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).

### <a name="disposal-of-services"></a>Suppression des services

Le conteneur appelle `Dispose` pour les types `IDisposable` qu’il crée. Si une instance est ajoutée au conteneur par le code utilisateur, elle n’est pas supprimée automatiquement.

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

::: moniker range="= aspnetcore-1.0"

> [!NOTE]
> Dans ASP.NET Core 1.0, les appels du conteneur suppriment *tous* les objets `IDisposable`, y compris ceux qu’il n’a pas créés.

::: moniker-end

## <a name="default-service-container-replacement"></a>Remplacement de conteneur de services par défaut

Le conteneur de services intégré a vocation à répondre aux besoins du framework et de la plupart des applications consommatrices. Nous vous recommandons d’utiliser le conteneur intégré, sauf si vous avez besoin d’une fonctionnalité spécifique qu’il ne prend pas en charge. Voici quelques-unes des fonctionnalités prises en charge dans des conteneurs tiers qui sont introuvables dans le conteneur intégré :

* Injection de propriétés
* Injection en fonction du nom
* Conteneurs enfants
* Gestion personnalisée de la durée de vie
* `Func<T>` prend en charge l’initialisation tardive

Pour obtenir une liste de certains des conteneurs qui prennent en charge des adaptateurs, consultez le [fichier readme.md relatif à l’injection de dépendances](https://github.com/aspnet/DependencyInjection#using-other-containers-with-microsoftextensionsdependencyinjection).

L’exemple suivant remplace le conteneur intégré par [Autofac](https://autofac.org/) :

* Installez les packages de conteneurs appropriés :

  * [Autofac](https://www.nuget.org/packages/Autofac/)
  * [Autofac.Extensions.DependencyInjection](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)

* Configurez le conteneur dans `Startup.ConfigureServices` et retournez un `IServiceProvider` :

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

    Pour utiliser un conteneur tiers, `Startup.ConfigureServices` doit retourner `IServiceProvider`.

* Configurez Autofac dans `DefaultModule` :

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

Au moment de l’exécution, Autofac est utilisé pour résoudre les types et injecter des dépendances. Pour en savoir plus sur l’utilisation d’Autofac avec ASP.NET Core, consultez la [documentation Autofac](https://docs.autofac.org/en/latest/integration/aspnetcore.html).

### <a name="thread-safety"></a>Sécurité des threads

Les services singleton doivent être thread-safe. Si un service singleton a une dépendance vis-à-vis d’un service Transient, ce dernier peut également avoir besoin d’être thread-safe selon la manière dont il est utilisé par le singleton.

La méthode de fabrique d’un service individuel, comme le deuxième argument de [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider, TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), ne doit être thread-safe. Comme un constructeur de type (`static`), elle est forcément appelée une fois par un seul thread.

## <a name="recommendations"></a>Recommandations

* La résolution de service basée sur `async/await` et `Task` n’est pas prise en charge. C# ne prend pas en charge les constructeurs asynchrones. Par conséquent, le modèle recommandé consiste à utiliser des méthodes asynchrones après avoir résolu de façon synchrone le service.

* Évitez de stocker des données et des configurations directement dans le conteneur de services. Par exemple, le panier d’achat d’un utilisateur ne doit en général pas être ajouté au conteneur de services. La configuration doit utiliser le [modèle d’options](xref:fundamentals/configuration/options). De même, évitez les objets « conteneurs de données » qui n’existent que pour autoriser l’accès à un autre objet. Il est préférable de demander l’élément réel par le biais de l’injection de dépendance.

* Évitez l’accès statique aux services (par exemple avec [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) à utiliser ailleurs).

* Évitez d’utiliser le *modèle de localisation de service*. Par exemple, n’appelez pas <xref:System.IServiceProvider.GetService*> pour obtenir une instance de service lorsque vous pouvez utiliser l’injection de dépendance à la place. Une autre variante du localisateur de service à éviter est l’injection d’une fabrique qui résout les dépendances au moment de l’exécution. Ces deux pratiques combinent des stratégies [Inversion de contrôle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion).

* Évitez l’accès statique à `HttpContext` (par exemple, [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).

Comme pour toutes les recommandations, vous pouvez vous trouver dans des situations où il est nécessaire d’ignorer une recommandation. Les exceptions sont rares et représentent principalement des cas spéciaux dans le framework lui-même.

L’injection de dépendance constitue une *alternative* aux modèles d’accès aux objets statiques/globaux. Il est possible que vous ne bénéficiez pas des avantages de l’injection de dépendances si vous la combinez avec l’accès aux objets statiques.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:fundamentals/startup>
* <xref:test/index>
* <xref:fundamentals/middleware/extensibility>
* [Écrire un code clair dans ASP.NET Core avec l’injection de dépendance (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [Prélude à la conception d’une application gérée par conteneur : à qui appartient le conteneur ?](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)
* [Principe des dépendances explicites](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [Conteneurs d’inversion de contrôle et modèle d’injection de dépendances (Martin Fowler)](https://www.martinfowler.com/articles/injection.html)
* [New is Glue (un code de « collage » dans une implémentation particulière)](https://ardalis.com/new-is-glue)
* [Comment inscrire un service avec plusieurs interfaces dans l’injection de dépendance ASP.NET Core](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)
