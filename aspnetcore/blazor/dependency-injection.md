---
title: ASP.NET Core l’injection de dépendances éblouissantes
author: guardrex
description: Découvrez comment les applications éblouissantes peuvent injecter des services dans des composants.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: blazor/dependency-injection
ms.openlocfilehash: 074d7a669c900eb242c8329147b28d1c50652915
ms.sourcegitcommit: e5a74f882c14eaa0e5639ff082355e130559ba83
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71168086"
---
# <a name="aspnet-core-blazor-dependency-injection"></a>ASP.NET Core l’injection de dépendances éblouissantes

Par [Rainer Stropek](https://www.timecockpit.com)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Éblouissant prend en charge l' [injection de dépendances (di)](xref:fundamentals/dependency-injection). Les applications peuvent utiliser des services intégrés en les injectant dans des composants. Les applications peuvent également définir et inscrire des services personnalisés et les rendre disponibles dans toute l’application via DI.

La méthode DI est une technique permettant d’accéder à des services configurés dans un emplacement central. Cela peut être utile dans les applications éblouissantes pour :

* Partager une seule instance d’une classe de service sur de nombreux composants, appelé service *Singleton* .
* Découplez les composants des classes de service concrètes à l’aide d’abstractions de référence. Par exemple, considérez une `IDataAccess` interface pour accéder aux données dans l’application. L’interface est implémentée par une `DataAccess` classe concrète et inscrite en tant que service dans le conteneur de services de l’application. Quand un composant utilise di pour recevoir une `IDataAccess` implémentation, le composant n’est pas couplé au type concret. L’implémentation peut être permutée, peut-être pour une implémentation factice dans les tests unitaires.

## <a name="default-services"></a>Services par défaut

Les services par défaut sont automatiquement ajoutés à la collection de services de l’application.

| Service | Durée de vie | Description |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | Singleton | Fournit des méthodes pour envoyer des requêtes HTTP et recevoir des réponses HTTP d’une ressource identifiée par un URI. Notez que cette instance de `HttpClient` utilise le navigateur pour gérer le trafic HTTP en arrière-plan. [Httpclient. BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) est automatiquement défini sur le préfixe URI de base de l’application. Pour plus d'informations, consultez <xref:blazor/call-web-api>. |
| `IJSRuntime` | Singleton | Représente une instance d’un Runtime JavaScript dans laquelle les appels JavaScript sont distribués. Pour plus d'informations, consultez <xref:blazor/javascript-interop>. |
| `NavigationManager` | Singleton | Contient des assistances pour l’utilisation des URI et de l’état de navigation. Pour plus d’informations, consultez [URI et assistance de l’état de navigation](xref:blazor/routing#uri-and-navigation-state-helpers). |

Un fournisseur de services personnalisé ne fournit pas automatiquement les services par défaut indiqués dans le tableau. Si vous utilisez un fournisseur de services personnalisé et que vous avez besoin de l’un des services répertoriés dans le tableau, ajoutez les services requis au nouveau fournisseur de services.

## <a name="add-services-to-an-app"></a>Ajouter des services à une application

Après avoir créé une nouvelle application, examinez la `Startup.ConfigureServices` méthode :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

Une `ConfigureServices` <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>, qui est une liste d’objets descripteurs de service (), est passée à la méthode. <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> Les services sont ajoutés en fournissant des descripteurs de service à la collection de services. L’exemple suivant illustre le concept avec l' `IDataAccess` interface et son implémentation `DataAccess`concrète :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

Les services peuvent être configurés avec les durées de vie indiquées dans le tableau suivant.

| Durée de vie | Description |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Les applications webassembly éblouissantes n’ont pas actuellement de concept d’étendues DI. `Scoped`-les services inscrits se `Singleton` comportent comme des services. Toutefois, le modèle d’hébergement du serveur éblouissant prend `Scoped` en charge la durée de vie. Dans les applications serveur éblouissantes, l’inscription d’un service étendu est limitée à la *connexion*. Pour cette raison, il est préférable d’utiliser les services délimités pour les services qui doivent être étendus à l’utilisateur actuel, même si l’objectif actuel est d’exécuter côté client dans le navigateur. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | DI crée une *seule instance* du service. Tous les composants qui `Singleton` requièrent un service reçoivent une instance du même service. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | Chaque fois qu’un composant obtient une instance d' `Transient` un service à partir du conteneur de service, il reçoit une *nouvelle instance* du service. |

Le système DI est basé sur le système DI dans ASP.NET Core. Pour plus d'informations, consultez <xref:fundamentals/dependency-injection>.

## <a name="request-a-service-in-a-component"></a>Demander un service dans un composant

Une fois les services ajoutés à la collection de services, injectez les services dans les [ \@](xref:mvc/views/razor#inject) composants à l’aide de la directive Razor Inject. `@inject`a deux paramètres :

* Tapez &ndash; le type du service à injecter.
* Propriété &ndash; nom de la propriété qui reçoit le service d’application injecté. La propriété ne nécessite pas de création manuelle. Le compilateur crée la propriété.

Pour plus d'informations, consultez <xref:mvc/views/dependency-injection>.

Utilisez plusieurs `@inject` instructions pour injecter différents services.

L'exemple suivant montre comment utiliser `@inject`. Le service qui `Services.IDataAccess` implémente est injecté dans la propriété `DataRepository`du composant. Notez la manière dont le code utilise uniquement `IDataAccess` l’abstraction :

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

En interne, la propriété générée (`DataRepository`) est décorée avec `InjectAttribute` l’attribut. En règle générale, cet attribut n’est pas utilisé directement. Si une classe de base est requise pour les composants et les propriétés injectées sont également requises pour la classe de base `InjectAttribute`, ajoutez manuellement le :

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

Dans les composants dérivés de la classe de `@inject` base, la directive n’est pas obligatoire. Le `InjectAttribute` de la classe de base est suffisant :

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a>Utiliser DI dans les services

Les services complexes peuvent nécessiter des services supplémentaires. Dans l’exemple précédent, `DataAccess` peut nécessiter `HttpClient` le service par défaut. `@inject`(ou) `InjectAttribute`n’est pas disponible pour une utilisation dans les services. L' *injection de constructeur* doit être utilisée à la place. Les services requis sont ajoutés en ajoutant des paramètres au constructeur du service. Lorsque DI crée le service, il reconnaît les services dont il a besoin dans le constructeur et les fournit en conséquence.

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

* Un constructeur doit exister dont les arguments peuvent tous être remplis par DI. Les paramètres supplémentaires non couverts par DI sont autorisés s’ils spécifient des valeurs par défaut.
* Le constructeur applicable doit être *public*.
* Un constructeur applicable doit exister. En cas d’ambiguïté, DI lève une exception.

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a>Classes de composants de base de l’utilitaire pour gérer une étendue DI

Dans ASP.NET Core applications, les services délimités sont généralement étendus à la requête actuelle. Une fois la demande terminée, tous les services délimités ou temporaires sont supprimés par le système DI. Dans les applications serveur éblouissantes, l’étendue de la demande est valable pendant la durée de la connexion du client, ce qui peut entraîner des services transitoires et de portée de vie bien plus longs que prévu.

Pour étendre les services à la durée de vie d’un composant, `OwningComponentBase` peut `OwningComponentBase<TService>` utiliser les classes de base et. Ces classes de base exposent `ScopedServices` une `IServiceProvider` propriété de type qui résout les services dont la portée est limitée à la durée de vie du composant. Pour créer un composant qui hérite d’une classe de base dans Razor, utilisez `@inherits` la directive.

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
> Les services injectés dans le composant `@inject` à l' `InjectAttribute` aide de ou de ne sont pas créés dans l’étendue du composant et sont liés à l’étendue de la demande.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
