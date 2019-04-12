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
# <a name="razor-components-dependency-injection"></a>Injection de dépendances de composants de Razor

Par [Rainer Stropek](https://www.timecockpit.com)

Prend en charge les composants de Razor [l’injection de dépendance (DI)](xref:fundamentals/dependency-injection). Applications peuvent utiliser les services intégrés en les insérant dans les composants. Les applications peuvent également définir et inscrire les services personnalisés et les rendre disponibles dans toute l’application via l’injection de dépendances.

## <a name="dependency-injection"></a>Injection de dépendances

L’injection de dépendances est une technique permettant d’accéder aux services configurés dans un emplacement central. Il se peut que cela peut être utile dans les applications de composants de Razor :

* Partager une seule instance d’une classe de service entre de nombreux composants, appelés un *singleton* service.
* Découpler les composants à partir de classes de service concret à l’aide des abstractions de référence. Par exemple, considérez une interface `IDataAccess` pour accéder aux données dans l’application. L’interface est implémentée par un concret `DataAccess` classe et l’inscrit en tant que service dans le conteneur de service de l’application. Quand un composant utilise l’injection de dépendances pour recevoir un `IDataAccess` implémentation, le composant n’est pas couplée avec le type concret. L’implémentation peut être échangée, peut-être pour une implémentation factice dans les tests unitaires.

Pour plus d'informations, consultez <xref:fundamentals/dependency-injection>.

## <a name="add-services-to-di"></a>Ajouter des services à l’injection de dépendances

Après avoir créé une nouvelle application, examinez le `Startup.ConfigureServices` méthode :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

Le `ConfigureServices` est transmis à la méthode un <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, qui est une liste d’objets de descripteurs de service (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>). Services sont ajoutés en fournissant des descripteurs de service à la collection de service. L’exemple suivant illustre le concept avec la `IDataAccess` interface et son implémentation concrète `DataAccess`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

Services peuvent être configurés avec les durées de vie indiquées dans le tableau suivant.

| Durée de vie | Description |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | L’injection de dépendances crée un *SIS* du service. Tous les composants nécessitant un `Singleton` service reçoivent une instance du même service. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | Chaque fois qu’un composant obtient une instance d’un `Transient` service à partir du conteneur de service, il reçoit un *nouvelle instance* du service. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor du côté client n’a actuellement le concept d’étendues de l’injection de dépendances. `Scoped` se comporte comme `Singleton`. Toutefois, les composants ASP.NET Core Razor prend en charge la `Scoped` durée de vie. Dans un composant Razor, une inscription de service délimité est limitée à la connexion. Pour cette raison, l’utilisation de services délimités est préféré pour les services qui doivent être étendus à l’utilisateur actuel, même si l’objectif actuel est de s’exécuter côté client dans le navigateur. |

Le système de l’injection de dépendances est basé sur le système de l’injection de dépendances dans ASP.NET Core. Pour plus d'informations, consultez <xref:fundamentals/dependency-injection>.

## <a name="default-services"></a>Services par défaut

Services par défaut sont automatiquement ajoutés à la collection de service de l’application.

| Service | Description |
| ------- | ----------- |
| <xref:System.Net.Http.HttpClient> | Fournit des méthodes pour envoyer des requêtes HTTP et recevoir des réponses HTTP à partir d’une ressource identifiée par un URI (singleton). Notez que cette instance de `HttpClient` utilise le navigateur pour gérer le trafic HTTP en arrière-plan. [HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) est automatiquement défini sur le préfixe URI de base de l’application. `HttpClient` est fournie uniquement pour les applications Blazor côté client. |
| `IJSRuntime` | Représente une instance d’un runtime JavaScript à laquelle les appels peuvent être distribués. Pour plus d'informations, consultez <xref:razor-components/javascript-interop>. |
| `IUriHelper` | Contient les programmes d’assistance pour travailler avec l’état de l’URI et de navigation (singleton). `IUriHelper` est fourni pour les applications Blazor et composants de Razor. |

Il est possible d’utiliser un fournisseur de services personnalisé au lieu du fournisseur de service par défaut ajouté par le modèle par défaut. Un fournisseur de services personnalisée ne fournit pas automatiquement les services par défaut répertoriées dans le tableau. Si vous utilisez un fournisseur de services personnalisé et que vous avez besoin tout service indiqué dans le tableau, ajouter les services requis pour le nouveau fournisseur de services.

## <a name="request-a-service-in-a-component"></a>Demandez un service dans un composant

Une fois que les services sont ajoutés à la collection de service, injecter les services de modèles Razor de composants à l’aide de la [ @inject ](xref:mvc/views/razor#section-4) directive Razor. `@inject` a deux paramètres :

* Nom du type : Le type du service à injecter.
* Nom de la propriété : Le nom de la propriété réception injecté app service. Notez que la propriété ne nécessite pas la création manuelle. Le compilateur crée la propriété.

Pour plus d'informations, consultez <xref:mvc/views/dependency-injection>.

Utilisez plusieurs `@inject` instructions pour injecter des différents services.

L'exemple suivant montre comment utiliser `@inject`. Le service qui implémente `Services.IDataAccess` est injecté dans la propriété du composant `DataRepository`. Notez comment le code est uniquement à l’aide de la `IDataAccess` abstraction :

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.cshtml?highlight=2-3,23)]

En interne, la propriété générée (`DataRepository`) est décorée avec le `InjectAttribute` attribut. En règle générale, cet attribut n’est pas utilisé directement. Si une classe de base est requise pour les composants et propriétés injectées sont également requises pour la classe de base `InjectAttribute` peuvent être ajoutées manuellement :

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

Dans les composants dérivés de la classe de base, le `@inject` directive n’est pas nécessaire. Le `InjectAttribute` de la classe de base est suffisante :

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="dependency-injection-in-services"></a>Injection de dépendances dans les services

Services complexes peuvent nécessiter des services supplémentaires. Dans l’exemple précédent, `DataAccess` peut nécessiter la `HttpClient` service par défaut. `@inject` (ou `InjectAttribute`) n’est pas disponible pour une utilisation dans les services. *L’injection de constructeur* doit être utilisé à la place. Les services requis sont ajoutés en ajoutant des paramètres pour le constructeur du service. Lors de l’injection de dépendances crée le service, il reconnaît les services, il requiert dans le constructeur et les fournit en conséquence.

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

Conditions préalables pour l’injection de constructeur :

* Il doit y avoir un constructeur dont les arguments peuvent tous être satisfaits par l’injection de dépendances. Notez que les paramètres supplémentaires non couvertes par l’injection de dépendances sont autorisées si elles spécifient des valeurs par défaut.
* Le constructeur applicable doit être *public*.
* Il ne doit exister qu’un seul constructeur applicable. Dans le cas d’une ambiguïté, l’injection de dépendances lève une exception.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
