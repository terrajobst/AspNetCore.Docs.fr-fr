---
title: Injection de dépendances dans ASP.NET Core
author: ardalis
description: Découvrez comment ASP.NET Core implémente l’injection de dépendances et comment l’utiliser.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/dependency-injection
ms.openlocfilehash: 8a105f835dddfcd0e9f32059e644f60dc1fdbbe1
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="dependency-injection-in-aspnet-core"></a>Injection de dépendances dans ASP.NET Core

<a name="fundamentals-dependency-injection"></a>

Par [Steve Smith](https://ardalis.com/) et [Scott Addie](https://scottaddie.com)

ASP.NET Core est conçu à la base pour prendre en charge et exploiter l’injection de dépendances. Les applications ASP.NET Core peuvent tirer parti des services de framework intégrés en les injectant dans des méthodes de la classe Startup et les services d’application peuvent également être configurés pour l’injection. Le conteneur de services par défaut fourni par ASP.NET Core offre un ensemble minimal de fonctionnalités et n’a pas vocation à remplacer d’autres conteneurs.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-dependency-injection"></a>Qu’est-ce que l’injection de dépendances ?

L’injection de dépendances est une technique de couplage faible entre des objets et leurs collaborateurs, ou dépendances. Au lieu d’instancier directement des collaborateurs ou d’utiliser des références statiques, les objets dont a besoin une classe pour effectuer ses opérations sont fournis à la classe d’une certaine manière. Le plus souvent, les classes déclarent leurs dépendances par le biais de leur constructeur, ce qui leur permet de suivre le [principe des dépendances explicites](http://deviq.com/explicit-dependencies-principle/). Cette approche est appelée « injection de constructeurs ».

Lorsque les classes sont conçues avec l’injection de dépendances à l’esprit, leur couplage est plus faible car elles n’ont de dépendances directes codées en dur vis-à-vis de leurs collaborateurs. Cette conception suit le [principe de l’inversion de dépendances](http://deviq.com/dependency-inversion-principle/), qui stipule que les *« modules de niveau supérieur ne doivent pas dépendre de modules de niveau inférieur ; tous doivent dépendre d’abstractions »*. Au lieu de référencer des implémentations spécifiques, les classes demandent des abstractions (généralement `interfaces`) qui leur sont fournies lors de la construction de la classe. L’extraction de dépendances dans des interfaces et la fourniture des implémentations de ces interfaces en tant que paramètres sont également des exemples du [modèle de conception de stratégie](http://deviq.com/strategy-design-pattern/).

Lorsqu’un système est conçu pour utiliser l’injection de dépendances, avec de nombreuses classes qui demandent leurs dépendances par le biais de leur constructeur (ou leurs propriétés), il est judicieux d’avoir une classe dédiée à la création de ces classes avec leurs dépendances associées. Ces classes sont appelés des *conteneurs*, ou plus particulièrement, des conteneurs d’[inversion de contrôle](http://deviq.com/inversion-of-control/) ou des conteneurs d’injection de dépendances. Un conteneur est grosso modo une fabrique chargée de fournir des instances de types demandées à partir d’elle-même. Si un type donné a déclaré qu’il possède des dépendances et que le conteneur a été configuré pour fournir les types de dépendances, il crée les dépendances dans le cadre de la création de l’instance demandée. De cette façon, des graphiques de dépendance complexes peuvent être fournis aux classes sans exiger de construction d’objet codé en dur. En plus de créer des objets avec leurs dépendances, les conteneurs gèrent généralement les durées de vie des objets au sein de l’application.

ASP.NET Core inclut un simple conteneur intégré (représenté par l’interface `IServiceProvider`) qui prend en charge l’injection de constructeurs par défaut, et ASP.NET rend certains services disponibles par le biais de l’injection de dépendances. Le conteneur d’ASP.NET fait référence aux types qu’il gère comme des *services*. Dans le reste de cet article, les *services* font référence aux types gérés par le conteneur d’inversion de contrôle d’ASP.NET Core. Vous configurez les services du conteneur intégré dans la méthode `ConfigureServices` de la classe `Startup` de votre application.

> [!NOTE]
> Martin Fowler a écrit un article complet sur les [conteneurs d’inversion de contrôle et le modèle d’injection de dépendances](https://www.martinfowler.com/articles/injection.html). Le groupe Microsoft Patterns and Practices fait également une excellente description de l’[injection de dépendances](https://msdn.microsoft.com/library/hh323705.aspx).

> [!NOTE]
> Cet article traite de l’injection de dépendance telle qu’elle s’applique à toutes les applications ASP.NET. L’injection de dépendances au sein des contrôleurs MVC est abordée dans [Injection de dépendances et contrôleurs](../mvc/controllers/dependency-injection.md).

### <a name="constructor-injection-behavior"></a>Comportement d’injection de constructeurs

L’injection de constructeurs exige que le constructeur en question soit *public*. Sinon, votre application lève une `InvalidOperationException` :

> Impossible de trouver un constructeur approprié pour le type 'VotreType'. Vérifiez que le type est concret et que des services sont inscrits pour tous les paramètres d’un constructeur public.

L’injection de constructeurs exige qu’un seul constructeur applicable existe. Les surcharges de constructeurs sont prises en charge, mais une seule peut exister dont les arguments peuvent tous être satisfaits par l’injection de dépendances. S’il en existe plusieurs, votre application lève une `InvalidOperationException` :

> Plusieurs constructeurs acceptant tous les types d’argument donnés ont été trouvés dans le type 'VotreType'. Il ne doit y avoir qu’un seul constructeur applicable.

Les constructeurs peuvent accepter des arguments qui ne sont pas fournis par l’injection de dépendances, mais ceux-ci doivent prendre en charge les valeurs par défaut. Exemple :

```csharp
// throws InvalidOperationException: Unable to resolve service for type 'System.String'...
public CharactersController(ICharacterRepository characterRepository, string title)
{
    _characterRepository = characterRepository;
    _title = title;
}

// runs without error
public CharactersController(ICharacterRepository characterRepository, string title = "Characters")
{
    _characterRepository = characterRepository;
    _title = title;
}
```

## <a name="using-framework-provided-services"></a>Utilisation des services fournis par le framework

La méthode `ConfigureServices` dans la classe `Startup` est chargée de définir les services utilisés par l’application, notamment les fonctionnalités de plateforme comme Entity Framework Core et ASP.NET Core MVC. Au départ, la valeur `IServiceCollection` fournie à `ConfigureServices` a les services suivants définis (en fonction de [la manière dont l’hôte a été configuré](xref:fundamentals/hosting)) :

| Type de service | Durée de vie |
| ----- | ------- |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | Singleton |
| [Microsoft.Extensions.Logging.ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | Singleton |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](/dotnet/api/microsoft.extensions.logging.ilogger) | Singleton |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | Temporaire |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | Temporaire |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.ioptions-1) | Singleton |
| [System.Diagnostics.DiagnosticSource](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | Singleton |
| [System.Diagnostics.DiagnosticListener](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | Temporaire |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | Singleton |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | Temporaire |
| [Microsoft.AspNetCore.Hosting.Server.IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartup](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | Singleton |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | Singleton |

Voici un exemple qui montre comment ajouter des services supplémentaires au conteneur à l’aide de plusieurs méthodes d’extension comme `AddDbContext`, `AddIdentity` et `AddMvc`.

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

Les fonctionnalités et l’intergiciel (middleware) fournis par ASP.NET, comme MVC, respectent une convention visant à utiliser une seule méthode d’extension Add*ServiceName* pour inscrire tous les services requis par cette fonctionnalité.

> [!TIP]
> Vous pouvez demander certains services fournis par le framework au sein des méthodes `Startup` par le biais de leurs listes de paramètres. Consultez [Démarrage d’une application](startup.md) pour plus d’informations.

## <a name="registering-services"></a>Inscription de services

Vous pouvez inscrire vos propres services d’application comme suit. Le premier type générique représente le type (en général une interface) demandé à partir du conteneur. Le deuxième type générique représente le type concret instancié par le conteneur et utilisé pour répondre à de telles demandes.

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> Chaque méthode d’extension `services.Add<ServiceName>` ajoute (et éventuellement configure) des services. Par exemple, `services.AddMvc()` ajoute les services dont MVC a besoin. Il est recommandé de suivre cette convention, en plaçant des méthodes d’extension dans l’espace de noms `Microsoft.Extensions.DependencyInjection`, pour encapsuler des groupes d’inscriptions de services.

La méthode `AddTransient` est utilisée pour mapper des types abstraits sur des services concrets instanciés séparément pour chaque objet qui en a besoin. Il s’agit de la *durée de vie* du service. D’autres options de durée de vie sont décrites ci-dessous. Il est important de choisir une durée de vie appropriée pour chacun des services que vous inscrivez. Une nouvelle instance du service doit-elle être fournie à chaque classe qui le demande ? Une seule instance doit-elle être utilisée tout au long d’une requête web donnée ? Ou une seule instance doit-elle être utilisée pendant la durée de vie de l’application ?

Dans l’exemple de cet article, il existe un simple contrôleur qui affiche les noms des caractères, appelé `CharactersController`. Sa méthode `Index` affiche la liste actuelle des caractères stockés dans l’application et initialise la collection avec quelques caractères, s’il n’en existe aucun. Notez que bien que cette application utilise Entity Framework Core et la classe `ApplicationDbContext` pour sa persistance, rien de tout cela n’apparaît dans le contrôleur. Au lieu de cela, le mécanisme d’accès aux données spécifique est rendu abstrait derrière une interface, `ICharacterRepository`, qui suit le [modèle de référentiel](http://deviq.com/repository-pattern/). Une instance de `ICharacterRepository` est demandée via le constructeur et attribuée à un champ privé, qui est ensuite utilisé pour accéder aux caractères selon les besoins.

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

`ICharacterRepository` définit les deux méthodes dont le contrôleur a besoin pour utiliser les instances de `Character`.

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

Cette interface est à son tour implémentée par un type concret, `CharacterRepository`, qui est utilisé au moment de l’exécution.

> [!NOTE]
> La façon dont l’injection de dépendances est utilisée avec la classe `CharacterRepository` est un modèle général que vous pouvez suivre pour tous vos services d’application, pas seulement dans les « référentiels » ou les classes d’accès aux données.

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

Notez que `CharacterRepository` exige un `ApplicationDbContext` dans son constructeur. Il n’est pas rare que l’injection de dépendances soit utilisée de manière chaînée comme cela, avec chaque dépendance demandée demandant à son tour ses propres dépendances. Le conteneur est chargé de résoudre toutes les dépendances dans le graphique et de retourner le service entièrement résolu.

> [!NOTE]
> La création de l’objet demandé, et de tous les objets dont il a besoin, et de tous les objets dont ceux-ci ont besoin, est parfois appelée *graphique d’objet*. De même, l’ensemble collectif de dépendances qui doivent être résolues est généralement appelé *arborescence des dépendances* ou *graphique de dépendance*.

Dans ce cas, à la fois `ICharacterRepository` et à son tour `ApplicationDbContext` doivent être inscrits auprès du conteneur de services dans `ConfigureServices` dans `Startup`. `ApplicationDbContext` est configuré avec l’appel à la méthode d’extension `AddDbContext<T>`. Le code suivant illustre l’inscription du type `CharacterRepository`.

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

Vous devez ajouter des contextes Entity Framework au conteneur de services en utilisant la durée de vie `Scoped`. Cet ajout est automatique si vous utilisez les méthodes d’assistance comme indiqué ci-dessus. Les référentiels qui utilisent Entity Framework doivent utiliser la même durée de vie.

> [!WARNING]
> Le principal danger à surveiller a trait à la résolution d’un service `Scoped` depuis un singleton. Il est probable dans ce cas que l’état du service ne soit pas correct lors du traitement des requêtes suivantes.

Les services qui ont des dépendances doivent les inscrire dans le conteneur. Si le constructeur d’un service exige une primitive, comme `string`, celle-ci peut être injectée à l’aide de la [configuration](xref:fundamentals/configuration/index) et du [modèle d’options](xref:fundamentals/configuration/options).

## <a name="service-lifetimes-and-registration-options"></a>Durée de vie du service et options d’inscription

Vous pouvez configurer les services ASP.NET avec les durées de vie suivantes :

**Transient**

Des services à durée de vie temporaire sont créés chaque fois qu’ils sont demandés. Cette durée de vie convient parfaitement aux services légers et sans état.

**Scoped**

Les services à durée de vie délimitée sont créés une seule fois par requête.

> [!WARNING]
> Si vous utilisez un service délimité dans un middleware, injectez le service dans la méthode `Invoke` ou `InvokeAsync`. Ne faites pas l’injection via l’injection du constructeur, car elle force le service à se comporter comme un singleton.

**Singleton**

Les services à durée de vie singleton sont créés la première fois qu’ils sont demandés (ou lorsque `ConfigureServices` est exécuté si vous y spécifiez une instance), puis chaque requête suivante utilise la même instance. Si votre application exige un comportement singleton, il est recommandé d’autoriser le conteneur de services à gérer la durée de vie du service au lieu d’implémenter le modèle de conception singleton et de gérer la durée de vie de votre objet dans la classe vous-même.

Vous pouvez inscrire des services auprès du conteneur de plusieurs façons. Nous avons déjà vu comment inscrire une implémentation de service avec un type donné en spécifiant le type concret à utiliser. En outre, une fabrique peut être spécifiée. Elle est ensuite utilisée pour créer l’instance à la demande. La troisième méthode consiste à spécifier directement l’instance du type à utiliser, auquel cas le conteneur n’essaie jamais de créer une instance (ni d’en disposer).

Pour illustrer la différence entre ces options de durée de vie et d’inscription, considérez une interface simple qui représente une ou plusieurs tâches en tant qu’*opération* avec un identificateur unique, `OperationId`. Selon la façon dont nous configurons la durée de vie de ce service, le conteneur fournit les mêmes instances ou des instances différentes du service à la classe qui effectue la requête. Pour indiquer clairement quelle durée de vie est demandée, nous allons créer un seul type par option de durée de vie :

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

Nous implémentons ces interfaces à l’aide d’une seule classe, `Operation`, qui accepte un `Guid` dans son constructeur ou utilise un nouveau `Guid` si aucun n’est fourni.

Ensuite, dans `ConfigureServices`, chaque type est ajouté au conteneur en fonction de sa durée de vie nommée :

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

Notez que le service `IOperationSingletonInstance` utilise une instance spécifique avec un ID connu de `Guid.Empty` pour que l’utilisation de ce type soit clairement indiquée (son GUID ne contient que des zéros). Nous avons également inscrit un `OperationService` qui dépend de chacun des autres types `Operation`, pour qu’il soit clairement déterminé au sein d’une demande si ce service obtient la même instance que le contrôleur, ou une nouvelle, pour chaque type d’opération. Ce service se contente d’exposer ses dépendances comme des propriétés, afin de pouvoir les présenter dans l’affichage.

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

Pour illustrer les durées de vie des objets dans et entre les requêtes individuelles distinctes faites à l’application, l’exemple inclut un `OperationsController` qui demande chaque type `IOperation` ainsi qu’un `OperationService`. L’action `Index` affiche alors toutes les valeurs `OperationId` du contrôleur et du service.

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

Maintenant, deux requêtes distinctes sont effectuées auprès de cette action de contrôleur :

![Affichage des opérations de l’exemple d’application web d’injection de dépendances en cours d’exécution dans Microsoft Edge présentant les valeurs d’ID d’opération (GUID) des opérations temporaires, délimitées, singleton du contrôleur d’instance et du service d’opération pour la première requête.](dependency-injection/_static/lifetimes_request1.png)

![Affichage des opérations montrant les valeurs d’ID d’opération pour une deuxième requête.](dependency-injection/_static/lifetimes_request2.png)

Observez les valeurs `OperationId` qui varient au sein d’une requête et entre les requêtes.

* Les objets *temporaires* sont toujours différents ; une nouvelle instance est fournie à chaque contrôleur et à chaque service.

* Les objets *délimités* sont les mêmes au sein d’une requête, mais ils diffèrent entre les différentes requêtes.

* Les objets *singleton* sont les mêmes pour chaque objet et chaque requête (qu’une instance soit fournie dans `ConfigureServices` ou non).

## <a name="resolve-a-scoped-service-within-the-application-scope"></a>Résoudre un service délimité dans l’étendue de l’application

Créez un [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) avec [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) pour résoudre un service délimité dans l’étendue de l’application. Cette approche est pratique pour accéder à un service délimité au démarrage pour exécuter des tâches d’initialisation. L’exemple suivant montre comment obtenir un contexte pour `MyScopedService` dans `Program.Main` :

```csharp
public static void Main(string[] args)
{
    var host = BuildWebHost(args);

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

Quand l’application s’exécute dans l’environnement de développement sur ASP.NET Core 2.0 ou ultérieur, le fournisseur de services par défaut effectue des contrôles pour vérifier que :

* Les services délimités ne sont pas résolus directement ou indirectement à partir du fournisseur de services racine.
* Les services délimités ne sont pas directement ou indirectement injectés dans des singletons.

Le fournisseur de services racine est créé quand [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) est appelé. La durée de vie du fournisseur de services racine correspond à la durée de vie de l’application/du serveur quand le fournisseur démarre avec l’application et qu’il est supprimé quand l’application s’arrête.

Les services délimités sont supprimés par le conteneur qui les a créés. Si un service délimité est créé dans le conteneur racine, la durée de vie du service est promue en singleton, car elle est supprimée par le conteneur racine seulement quand l’application/le serveur est arrêté. La validation des étendues du service permet de traiter ces situations quand `BuildServiceProvider` est appelé.

Pour plus d’informations, consultez [Validation des étendues dans la rubrique Hébergement](xref:fundamentals/hosting#scope-validation).

## <a name="request-services"></a>Services de requête

Les services disponibles au sein d’une requête ASP.NET à partir de `HttpContext` sont exposés par le biais de la collection `RequestServices`.

![Boîte de dialogue contextuelle Intellisense des services de requête HttpContext indiquant que les services de requête obtiennent ou définissent la valeur IServiceProvider qui fournit l’accès au conteneur de services de la requête.](dependency-injection/_static/request-services.png)

Les services de requête représentent les services que vous configurez et demandez dans le cadre de votre application. Lorsque vos objets spécifient des dépendances, ceux-ci sont satisfaits par les types trouvés dans `RequestServices`, pas dans `ApplicationServices`.

En règle générale, vous ne devez pas utiliser ces propriétés directement, mais plutôt préférer demander les types dont vos classes ont besoin par le biais du constructeur de votre classe et laisser le framework injecter ces dépendances. Vous obtenez ainsi des classes plus faciles à tester (consultez [Tester et déboguer](../testing/index.md)) et plus faiblement couplées.

> [!NOTE]
> Préférez demander des dépendances en tant que paramètres de constructeur plutôt qu’accéder à la collection `RequestServices`.

## <a name="designing-services-for-dependency-injection"></a>Conception de services pour l’injection de dépendances

Vous devez concevoir vos services pour qu’ils utilisent l’injection de dépendances afin d’obtenir leurs collaborateurs. Cela signifie que vous devez éviter d’utiliser des appels de méthode statique avec état (qui entraînent un « code smell » appelé « [static cling](http://deviq.com/static-cling/) ») et d’instancier directement des classes dépendantes au sein de vos services. Cela peut vous aider de mémoriser l’expression « [New is Glue](https://ardalis.com/new-is-glue) », quand vous choisissez d’instancier ou non un type ou de le demander par le biais de l’injection de dépendances. En suivant les [principes SOLID de conception orientée objet](http://deviq.com/solid/), vos classes ont naturellement tendance à être petites, bien factorisées et facilement testées.

Que se passe-t-il si vous trouvez que vos classes ont tendance à avoir trop de dépendances injectées ? C’est généralement le signe que votre classe essaie d’en faire trop et qu’elle enfreint probablement le [principe de responsabilité unique](http://deviq.com/single-responsibility-principle/). Essayez de refactoriser la classe en déplaçant certaines de ses responsabilités dans une nouvelle classe. N’oubliez pas que vos classes `Controller` doivent se concentrer sur les préoccupations de l’interface utilisateur, donc les règles d’entreprise et les détails d’implémentation de l’accès aux données doivent être conservés dans les classes appropriées à ces [préoccupations distinctes](http://deviq.com/separation-of-concerns/).

En ce qui concerne l’accès aux données en particulier, vous pouvez injecter `DbContext` dans vos contrôleurs (en supposant que vous ayez ajouté EF au conteneur de services dans `ConfigureServices`). Certains développeurs préfèrent utiliser une interface de référentiel pour la base de données plutôt que d’injecter directement `DbContext`. L’utilisation d’une interface pour encapsuler la logique d’accès aux données à un seul emplacement permet de réduire le nombre d’emplacements à modifier quand votre base de données évolue.

### <a name="disposing-of-services"></a>Mise à disposition des services

Le conteneur appelle `Dispose` pour les types `IDisposable` qu’il crée. Toutefois, si vous ajoutez une instance au conteneur vous-même, elle n’est pas mise à disposition.

Exemple :

```csharp
// Services implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}


public void ConfigureServices(IServiceCollection services)
{
    // container will create the instance(s) of these types and will dispose them
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // container didn't create instance so it will NOT dispose it
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

> [!NOTE]
> Dans la version 1.0, le conteneur appelé dispose de *tous* les objets `IDisposable`, y compris de ceux qu’il n’a pas créés.

## <a name="replacing-the-default-services-container"></a>Remplacement du conteneur de services par défaut

Le conteneur de services intégré a vocation à répondre aux besoins élémentaires du framework et de la plupart des applications consommatrices qui s’appuient sur ce dernier. Toutefois, les développeurs peuvent remplacer le conteneur intégré par leur conteneur préféré. La méthode `ConfigureServices` retourne généralement `void`, mais si sa signature est modifiée afin de retourner `IServiceProvider`, un autre conteneur peut être configuré et retourné. Il existe de nombreux conteneurs d’inversion de contrôle disponibles pour .NET. Dans cet exemple, le package [Autofac](https://autofac.org/) est utilisé.

Tout d’abord, installez les packages de conteneurs appropriés :

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

Ensuite, configurez le conteneur dans `ConfigureServices` et retournez un `IServiceProvider` :

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

> [!NOTE]
> Lorsque vous utilisez un conteneur d’injection de dépendances tiers, vous devez modifier `ConfigureServices` afin de retourner `IServiceProvider` au lieu de `void`.

Enfin, configurez Autofac normalement dans `DefaultModule` :

```csharp
public class DefaultModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
    }
}
```

Au moment de l’exécution, Autofac est utilisé pour résoudre les types et injecter des dépendances. [En savoir plus sur l’utilisation d’Autofac et d’ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html).

### <a name="thread-safety"></a>Sécurité des threads

Les services singleton doivent être thread-safe. Si un service singleton a une dépendance vis-à-vis d’un service temporaire, ce dernier peut également avoir besoin d’être thread-safe selon la manière dont il est utilisé par le singleton.

## <a name="recommendations"></a>Recommandations

Lorsque vous utilisez l’injection de dépendances, gardez à l’esprit les recommandations suivantes :

* L’injection de dépendances est destinée aux objets qui ont des dépendances complexes. Les contrôleurs, les services, les adaptateurs et les référentiels sont tous des exemples d’objets susceptibles d’être ajoutés à l’injection de dépendances.

* Évitez de stocker des données et des configurations directement dans l’injection de dépendances. Par exemple, le panier d’achat d’un utilisateur ne doit en général pas être ajouté au conteneur de services. La configuration doit utiliser le [modèle d’options](xref:fundamentals/configuration/options). De même, évitez les objets « conteneurs de données » qui n’existent que pour autoriser l’accès à un autre objet. Il est préférable de demander l’élément réel dont vous avez besoin par le biais de l’injection de dépendances, si possible.

* Évitez l’accès statique aux services.

* Évitez l’emplacement du service dans votre code d’application.

* Évitez l’accès statique à `HttpContext`.

> [!NOTE]
> Comme pour toutes les recommandations, vous pouvez vous trouver dans des situations où il est nécessaire d’en ignorer une. À notre sens, ces exceptions sont rares. Elles concernent principalement des cas très spéciaux au sein du framework lui-même.

N’oubliez pas que l’injection de dépendances est une *alternative* aux modèles d’accès aux objets statiques/globaux. Vous ne pourrez pas bénéficier des avantages de l’injection de dépendances si vous la combinez avec l’accès aux objets statiques.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Démarrage d’une application](xref:fundamentals/startup)
* [Test et débogage](xref:testing/index)
* [Activation d’intergiciel (middleware) basée sur une fabrique](xref:fundamentals/middleware/extensibility)
* [Écrire un code clair dans ASP.NET Core avec l’injection de dépendance (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [Prélude à la conception d’une application gérée par conteneur : à qui appartient le conteneur ?](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)
* [Principe des dépendances explicites](http://deviq.com/explicit-dependencies-principle/)
* [Conteneurs d’inversion de contrôle et modèle d’injection de dépendances](https://www.martinfowler.com/articles/injection.html) (Fowler)
