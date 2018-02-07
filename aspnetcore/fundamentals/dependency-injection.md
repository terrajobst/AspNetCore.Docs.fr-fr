---
title: "Injection de dépendance dans ASP.NET Core"
author: ardalis
description: "Découvrez comment ASP.NET Core implémente injection de dépendance et comment l’utiliser."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/dependency-injection
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7a5a0991694b2c7caa79dbc09f6471d614f67dac
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-dependency-injection-in-aspnet-core"></a>Introduction à l’Injection de dépendance dans ASP.NET Core

<a name="fundamentals-dependency-injection"></a>

Par [Steve Smith](https://ardalis.com/) et [Scott Addie](https://scottaddie.com)

ASP.NET Core est conçu de toutes pièces pour prendre en charge et tirer parti de l’injection de dépendances. Les applications ASP.NET Core peuvent tirer parti des services d’infrastructure intégrés en les injectant dans les méthodes de la classe Startup, et les services d’application peuvent être configurés pour l’injection également. Le conteneur de services par défaut fourni par ASP.NET Core fournit une fonctionnalité minimale définie et n’est pas destiné à remplacer les autres conteneurs.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-dependency-injection"></a>Qu'est-ce que l’Injection de dépendance ?

L'injection de dépendance (DI) est une technique de couplage lâche entre les objets et leurs collaborateurs et les dépendances. Au lieu d’instancier directement des collaborateurs, ou à l’aide de références statiques, les objets qu'une classe a besoin pour effectuer ses actions sont fournis à la classe d’une certaine manière. En règle générale, les classes déclarent leurs dépendances via leur constructeur, ce qui leur permet de suivre le [principe de dépendances explicites](http://deviq.com/explicit-dependencies-principle/). Cette approche est appelée « injection de constructeur ».

Lorsque les classes conçues avec injection de dépendance à l'esprit, ils sont plus faiblement couplés, car ils n’ont pas de dépendances directes, codées en dur sur leurs collaborateurs. Cela suit le [principe d’Inversion de dépendance](http://deviq.com/dependency-inversion-principle/), ce qui indique que *« les modules de niveaux élevés ne dépendent pas des modules de niveaux plus faibles ; les deux doivent dépendre sur des abstractions. »* Au lieu de référencer des implémentations spécifiques, les classes demandent des abstractions (généralement `interfaces`) qui sont fournies lors de la construction de classe. Extraire des dépendances dans les interfaces et fournir des implémentations de ces interfaces en tant que paramètres sont également un exemple de [design pattern stratégie](http://deviq.com/strategy-design-pattern/).

Lorsqu’un système est conçu pour utiliser l'injection de dépendance, avec de nombreuses classes demandant leurs dépendances via son constructeur (ou leurs propriétés), il est utile de disposer d’une classe dédiée à la création de ces classes avec leurs dépendances associées. Ces classes sont appelés *conteneurs*, ou plus spécifiquement, conteneurs d'[Inversion de contrôle (IoC)](http://deviq.com/inversion-of-control/) ou conteneurs d’Injection de dépendance (DI). Un conteneur est essentiellement une fabrique qui est chargée de fournir des instances de types qui sont demandées à partir de celui-ci. Si un type donné a déclaré qu’il possède des dépendances et le conteneur a été configuré pour fournir les types de dépendances, il crée les dépendances dans le cadre de la création de l’instance demandée. De cette façon, les graphiques de dépendance complexe peuvent être fournis aux classes sans nécessiter le besoin de n’importe quel construction d’objet codé en dur. En plus de la création d’objets avec leurs dépendances, les conteneurs gèrent généralement une durée de vie des objets dans l’application.

ASP.NET Core inclut un conteneur simple intégré (représenté par l'interface `IServiceProvider`) qui prend en charge l’injection de constructeur par défaut, et ASP.NET rend certains services disponibles via DI. Le conteneur d'ASP.NET fait référence aux types qu’il gère en tant que *services*. Dans le reste de cet article, *services* fait référence aux types qui sont gérés par le conteneur d’inversion de contrôle d’ASP.NET Core. Vous configurez les services du conteneur intégré dans la méthode `ConfigureServices` dans votre application classe `Startup`.

> [!NOTE]
> Martin Fowler a écrit un article complet sur [les conteneurs d’Inversion de contrôle et le modèle d’Injection de dépendance](https://www.martinfowler.com/articles/injection.html). Microsoft Patterns and Practices a également une excellente description de l'[Injection de dépendance](https://msdn.microsoft.com/library/hh323705.aspx).

> [!NOTE]
> Cet article couvre l’Injection de dépendance telle qu’elle s’applique à toutes les applications ASP.NET. L'Injection de dépendances au sein des contrôleurs MVC est couverte dans [Injection de dépendance et contrôleurs](../mvc/controllers/dependency-injection.md).

### <a name="constructor-injection-behavior"></a>Comportement de l’Injection de constructeur

L'Injection de constructeur requiert que le constructeur en question soit *public*. Sinon, votre application génère une `InvalidOperationException`:

> A suitable constructor for type 'YourType' couldn't be located. Ensure the type is concrete and services are registered for all parameters of a public constructor.


L'Injection de constructeur requiert qu’un seul constructeur applicable existe. Les srcharges de constructeur sont prises en charge, mais seulement une surcharge peut exister dont les arguments peuvent tous être satisfaits par injectionl' de dépendances. Si plusieurs existent, votre application lève une `InvalidOperationException`:

> Multiple constructors accepting all given argument types have been found in type 'YourType'. There should only be one applicable constructor.

Les constructeurs peuvent accepter des arguments qui ne sont pas fournies par injection de dépendances, mais ils doivent prendre en charge les valeurs par défaut. Exemple :

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

## <a name="using-framework-provided-services"></a>Utiliser les Services fournis par le Framework

La méthode `ConfigureServices` dans la classe `Startup` est chargée de définir les services que l’application utilise, notamment les fonctionnalités de plateforme comme Entity Framework Core et ASP.NET Core MVC. Au départ, la méthode `IServiceCollection` fournie à `ConfigureServices` a les services suivants définis (en fonction de [la configuration de l’hôte](xref:fundamentals/hosting)) :

| Type de service | Durée de vie |
| ----- | ------- |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) | Singleton |
| [Microsoft.Extensions.Logging.ILoggerFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.iloggerfactory) | Singleton |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) | Singleton |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | Transient |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.ihttpcontextfactory) | Transient |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) | Singleton |
| [System.Diagnostics.DiagnosticSource](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | Singleton |
| [System.Diagnostics.DiagnosticListener](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartupfilter) | Transient |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.objectpool.objectpoolprovider) | Singleton |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.iconfigureoptions-1) | Transient |
| [Microsoft.AspNetCore.Hosting.Server.IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartup](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartup) | Singleton |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | Singleton |

Voici un exemple montrant comment ajouter des services supplémentaires pour le conteneur à l’aide d’un nombre de méthodes d’extension comme `AddDbContext`, `AddIdentity`, et `AddMvc`.

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

Les fonctionnalités et l'intergiciel (middleware) fournies par ASP.NET, tels que MVC, respectent une convention à l’aide d’une méthode d’extension Add*ServiceName* pour inscrire tous les services requis par cette fonctionnalité.

>[!TIP]
> Vous pouvez demander à certains services fournis par le framework dans la méthode `Startup` via leurs listes de paramètres - voir [démarrage de l’Application](startup.md) pour plus d’informations.

## <a name="registering-your-own-services"></a>L’inscription de vos propres Services

Vous pouvez enregistrer vos propres services d’application comme suit. Le premier type générique représente le type (en général une interface) qui vous est demandé à partir du conteneur. Le deuxième type générique représente le type concret qui sera instancié par le conteneur et utilisé pour répondre à ces demandes.

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> Chaque méthode d’extension `services.Add<ServiceName>` ajoute (et potentiellement configure) des services. Par exemple, `services.AddMvc()` ajoute les services que MVC requiert. Il est recommandé de suivre cette convention, placer des méthodes d’extension dans l'espace de noms `Microsoft.Extensions.DependencyInjection` pour encapsuler des groupes d’enregistrements de service.

La méthode `AddTransient` est utilisée pour mapper les types abstraits aux services concrets qui sont instanciés séparément pour chaque objet qui en a besoin. Il s’agit de la *durée de vie* du service, et les options de durée de vie supplémentaires sont décrites ci-dessous. Il est important de choisir une durée de vie appropriée pour chacun des services que vous inscrivez. Une nouvelle instance du service doit être fournie pour chaque classe qui le demande ? Une instance doit être utilisée dans une requête web donné ? Ou, si une seule instance doit être utilisée pour la durée de vie de l’application ?

Dans l’exemple de cet article, il existe un contrôleur simple qui affiche les noms de caractères, appelées `CharactersController`. Sa méthode `Index` affiche la liste actuelle des caractères qui ont été stockés dans l’application et initialise la collection avec un certain nombre de caractères si aucune n’existe. Notez que bien que cette application utilise Entity Framework Core et la classe `ApplicationDbContext` pour la persistance, aucune qui est visible dans le contrôleur. Au lieu de cela, le mécanisme d’accès de données spécifique a été abstrait derrière une interface, `ICharacterRepository`, qui suit le [repository pattern](http://deviq.com/repository-pattern/). Une instance de `ICharacterRepository` est demandée via le constructeur et attribué à un champ privé, ce qui est ensuite utilisé pour accéder aux caractères comme nécessaire.

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

Le `ICharacterRepository` définit les deux méthodes, le contrôleur doit fonctionner avec des instances de `Character`.

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

Cette interface est ensuite implémentée par un type concret, `CharacterRepository`, qui est utilisé lors de l’exécution.

> [!NOTE]
> La méthode DI est utilisée avec la classe `CharacterRepository` est un modèle général que vous pouvez suivre pour tous les services de votre application, pas seulement dans les "repositories" ou les classes d’accès aux données.

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

Notez que `CharacterRepository` demande un `ApplicationDbContext` dans son constructeur. Il n’est pas rare pour l’injection de dépendance d'utiliser de manière chaînée comme celui-ci, avec chaque dépendance en demandant ses propres dépendances requises. Le conteneur est responsable de la résolution de toutes les dépendances dans le graphique et en retournant le service entièrement résolu.

> [!NOTE]
> La création de l’objet demandé et tous les objets qu’il nécessite et tous les objets que ceux-ci nécessitent, est parfois appelé un *graphique d’objets*. De même, l’ensemble de dépendances qui doivent être résolus est généralement appelé une *arborescence des dépendances* ou *graphique de dépendance*.

Dans ce cas, les deux `ICharacterRepository` et à son tour le `ApplicationDbContext` doivent être inscrits auprès du conteneur de services dans `ConfigureServices` dans `Startup`. `ApplicationDbContext` est configuré avec l’appel à la méthode d’extension `AddDbContext<T>`. Le code suivant illustre l’inscription du type `CharacterRepository`.

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

Les contextes d’Entity Framework doivent être ajoutés pour le conteneur de services à l’aide de la durée de vie `Scoped`. Cela est prise en charge automatiquement si vous utilisez les méthodes d’aide comme indiqué ci-dessus. Les repositories qui utiliseront Entity Framework doivent utiliser la même durée de vie.

>[!WARNING]
> Le risque principal avec lequel il faut être prudent est de résoudre un service `Scoped` depuis un singleton. Il est probable dans ce cas que le service aura état incorrect lors du traitement des demandes suivantes.

Les services qui ont des dépendances doivent les enregistrer dans le conteneur. Si le constructeur d’un service nécessite un type primitif, tel qu’un `string`, cela peut être ajoutée à l’aide de la [configuration](xref:fundamentals/configuration/index) et du [modèle d’options](xref:fundamentals/configuration/options).

## <a name="service-lifetimes-and-registration-options"></a>Durée de vie du service et Options d’enregistrement

Les services ASP.NET peuvent être configurés avec les durées de vie suivantes :

**Transient**

Les services avec une durée de vie temporaires (Transient) sont créées chaque fois qu’ils sont interrogés. Cette durée de vie convient le mieux pour les services légers, sans état.

**Scoped**

Les services avec une durée de vie étendue (Scoped) sont créés une fois par demande.

**Singleton**

Les services avec une durée de vie Singleton sont créés à la première fois qu’ils sont interrogés (ou lorsque `ConfigureServices` est exécuté si vous spécifiez une instance là) et ensuite toutes les requêtes suivantes utiliseront la même instance. Si votre application requiert un comportement singleton, permettre au conteneur des services de gérer la durée de vie du service est recommandé au lieu d’implémenter le modèle de conception singleton et la gestion de durée de vie de votre objet dans la classe.

Les services peuvent être inscrits avec le conteneur de plusieurs façons. Nous avons déjà vu comment inscrire une implémentation de service avec un type donné en spécifiant le type concret à utiliser. En outre, une fabrique peut être spécifiée, qui sera ensuite être utilisé pour créer l’instance à la demande. La troisième méthode consiste à spécifier directement l’instance du type à utiliser, dans laquelle le conteneur de cas ne tentera jamais de créer une instance (ni s’il dispose de l’instance).

Pour illustrer la différence entre ces options d’enregistrement et de la durée de vie, considérez une interface simple qui représente une ou plusieurs tâches en tant qu’*opération* avec un identificateur unique, `OperationId`. Selon la façon dont nous configurons la durée de vie pour ce service, le conteneur fournit les identiques ou différentes instances du service à la classe de demande. Pour indiquer clairement à quelle durée de vie est demandée, nous allons créer un type par l’option de durée de vie :

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

Nous implémentons ces interfaces à l’aide d’une seule classe, `Operation`, qui accepte un `Guid` dans son constructeur, ou utilise un nouveau `Guid` si aucun n’est fourni.

Ensuite, dans `ConfigureServices`, chaque type est ajouté au conteneur en fonction de sa durée de vie nommée :

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

Notez que le service `IOperationSingletonInstance` utilise une instance spécifique avec un ID connu de `Guid.Empty` pour que ça soit clair lorsque ce type est en cours d’utilisation (son Guid sera composé de zéros). Nous avons également inscrit un `OperationService` qui dépend de chacune des autres types `Operation`, de sorte qu’il soit clair dans une demande si ce service est mise en route de la même instance que le contrôleur, ou un autre, pour chaque type d’opération. Tout ce que ce service fait est d'exposer ses dépendances en tant que propriétés, afin qu’ils puissent être affichés dans la vue.

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

Pour illustrer les durées de vie d’objet dans et entre les demandes individuelles distinctes à l’application, l’exemple inclut un `OperationsController` qui demandes chaque type de type `IOperation` comme `OperationService`. L'action `Index` affiche tous les valeurs du contrôleur et du service `OperationId`.

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

Maintenant deux demandes distinctes sont apportées à cette action de contrôleur :

![La vue des opérations de l’application web exemple d’Injection de dépendance en cours d’exécution dans Microsoft Edge présentant les valeurs d’ID d’opération (GUID) pour Transient, Scoped, Singleton et contrôleur de l’Instance et fonctionnement des opérations de Service pour la première demande.](dependency-injection/_static/lifetimes_request1.png)

![Les opérations afficher indiquant les valeurs de l’ID d’opération pour une deuxième demande.](dependency-injection/_static/lifetimes_request2.png)

Observez parmi les différentes valeurs `OperationId` dans une demande et entre les demandes.

* Les objets *Transient* sont toujours différents ; une nouvelle instance est fournie pour chaque contrôleur et tous les services.

* Les objets *Scoped* sont les mêmes au sein d’une demande, mais diffère entre les différentes demandes

* *Singleton* objets sont les mêmes pour tous les objets et toutes les demandes (quel que soit la fourniture d’une instance dans `ConfigureServices`)

## <a name="request-services"></a>Request Services

Les services disponibles dans ASP.NET demande à partir de `HttpContext` sont exposés via la collection `RequestServices`.

![HttpContext Request Services Intellisense boîte de dialogue contextuelle indiquant que la demande de Services Obtient ou définit le IServiceProvider qui fournit l’accès au conteneur de service de la demande.](dependency-injection/_static/request-services.png)

Les Requeste Services représentent les services de configuration et de la demande dans le cadre de votre application. Lorsque vos objets de spécifient les dépendances, ceux-ci sont satisfaites par les types trouvés dans `RequestServices`, et non `ApplicationServices`.

En règle générale, vous ne devez pas utiliser ces propriétés directement, préférant à la place demander les types de vos classes que vous avez besoin via votre constructeur de classe et laissant le framework injecter ces dépendances. Cela génère des classes qui sont plus faciles à tester (voir [test](../testing/index.md)) et sont plus faiblement couplées.

> [!NOTE]
> Préférez de demander des dépendances en tant que paramètres du constructeur plutôt que d'accéder à la collection `RequestServices`.

## <a name="designing-your-services-for-dependency-injection"></a>Conception de vos Services pour l’Injection de dépendance

Vous devez concevoir vos services afin d'utiliser l’injection de dépendances pour obtenir leurs collaborateurs. Cela signifie d'éviter l’utilisation d’appels de méthode statiques avec état (ce qui entraîne une erreur de code appelée [static cling](http://deviq.com/static-cling/)) et l’instanciation directe de classes dépendantes dans vos services. Il peut être utile de se souvenir de l’expression suivante : [New is Glue](https://ardalis.com/new-is-glue), lorsque vous choisissez s’il faut instancier un type à la demande via l’injection de dépendances. En suivant les [principes SOLID de conception orientée objet](http://deviq.com/solid/), vos classes tendront naturellement à être petites, bien factorisées et facilement testées.

Que se passe-t-il si vous trouvez que vos classes ont tendance à avoir trop de dépendances injectées ? Il s’agit généralement d’un signe que votre classe essaie de faire trop et qu’il est probablement en train de violer le [principe de responsabilité unique](http://deviq.com/single-responsibility-principle/). Voyez si vous pouvez refactoriser la classe en déplaçant certaines de ses responsabilités dans une nouvelle classe. N’oubliez pas que vos classes `Controller` doivent se concentrer sur les problèmes de l’interface utilisateur, afin que les données métiers et l'implémentation des détails de règles d’accès soient conservés dans les classes appropriées à ces [séparations de préoccupations](http://deviq.com/separation-of-concerns/).

En ce qui concerne l’accès aux données en particulier, vous pouvez injecter le `DbContext` dans vos contrôleurs (en supposant que vous avez ajouté EF au conteneur de services dans `ConfigureServices`). Certains développeurs préfèrent utiliser une interface repository pour la base de données plutôt que d’injecter le `DbContext` directement. À l’aide d’une interface pour encapsuler la logique d’accès au données dans un seul emplacement peut réduire le nombre d’entrées que vous devrez changer lorsque votre base de données est modifiée.

### <a name="disposing-of-services"></a>Suppression des services

Le conteneur appelle `Dispose` pour les types `IDisposable` qu’il crée. Toutefois, si vous ajoutez une instance au conteneur vous-même, elle ne sera pas supprimée.

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
> Dans la version 1.0, le conteneur appelé dispose sur *tous les* objets `IDisposable`, y compris ceux qu’il n’avez pas créé.

## <a name="replacing-the-default-services-container"></a>Remplacer le conteneur de services par défaut

Le conteneur de services intégrés est destiné à répondre aux des besoins de base de l’infrastructure et à la plupart des applications clientes construites dessus. Toutefois, les développeurs peuvent remplacer le conteneur intégré avec leur conteneur par défaut. La méthode `ConfigureServices` retourne généralement `void`, mais si sa signature est modifiée afin de retourner `IServiceProvider`, un autre conteneur peut être configuré et retourné. Il existe de nombreux conteneurs d’inversion de contrôle pour .NET. Dans cet exemple, le package [Autofac](https://autofac.org/) est utilisé.

Tout d’abord, installez les packages de conteneur approprié :

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

Ensuite, configurez le conteneur dans `ConfigureServices` et renvoyer un `IServiceProvider`:

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
> Lorsque vous utilisez un conteneur de DI tiers, vous devez modifier `ConfigureServices` afin qu’elle retourne `IServiceProvider` au lieu de `void`.

Enfin, configurez normalement Autofac dans `DefaultModule`:

```csharp
public class DefaultModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
    }
}
```

Lors de l’exécution, Autofac servira à résoudre les types et injecter des dépendances. [En savoir plus sur l’utilisation de Autofac et ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html).

### <a name="thread-safety"></a>Sécurité des threads

Les services de singleton doivent être thread-safe. Si un service singleton a une dépendance sur un service temporaire, le service temporaire deviez également être thread-safe en fonction de son utilisation par le singleton.

## <a name="recommendations"></a>Recommandations

Lorsque vous travaillez avec l’injection de dépendances, gardez à l’esprit les recommandations suivantes :

* L'injection de dépendance est pour les objets qui ont des dépendances complexes. Les contrôleurs, les services, les adaptateurs et les repositories sont des exemples d’objets qui peuvent être ajoutés à l'injection de dépendance.

* Évitez de stocker des données et configuration directement dans l'injection de dépendance. Par exemple, un panier d’achat d’un utilisateur ne doit pas être généralement ajouté au conteneur de services. La configuration doit utiliser le [modèle d’options](xref:fundamentals/configuration/options). De même, évitez les objets « détenteur de données » qui n’existent que pour autoriser l’accès à un autre objet. Il est préférable de demander l’élément réel nécessaire via DI, si possible.

* Évitez l’accès aux services statiques.

* Évitez l’emplacement du service dans votre code d’application.

* Évitez l’accès statique à `HttpContext`.

> [!NOTE]
> Comme toutes les recommandations, vous pouvez rencontrer des situations dans lesquelles en ignorant une est nécessaire. Nous avons trouvé des exceptions être rares -- principalement des cas très spéciaux au sein de l’infrastructure elle-même.

N’oubliez pas, l’injection de dépendance est une *alternative* pour les modèles d’accès d'objet static/global. Vous ne pourrez pas bénéficier des avantages de l'injection de dépendance si vous la combiner avec l’accès aux objets statiques.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Démarrage d’une application](startup.md)

* [Test](../testing/index.md)

* [Ecrire du code propre ASP.NET Core avec l’Injection de dépendances (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)

* [Conception de l’Application gérée par le conteneur, prélude : Où le conteneur appartient-il ?](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)

* [Principe des dépendances explicites](http://deviq.com/explicit-dependencies-principle/)

* [Inversion des conteneurs de contrôle et le modèle d’Injection de dépendance](https://www.martinfowler.com/articles/injection.html) (Fowler)
