---
title: Filtres dans ASP.NET Core
author: ardalis
description: Découvrez comment les filtres fonctionnent et comment les utiliser dans ASP.NET Core MVC.
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2018
uid: mvc/controllers/filters
ms.openlocfilehash: d4fe49a9225b9980a956ef9c773ad631beb557ae
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207458"
---
# <a name="filters-in-aspnet-core"></a>Filtres dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/) et [Steve Smith](https://ardalis.com/)

Les *filtres* dans ASP.NET Core MVC vous permettent d’exécuter du code avant ou après des étapes spécifiques du pipeline de traitement des requêtes.

> [!IMPORTANT]
> Cette rubrique **ne s’applique pas** aux pages Razor. ASP.NET Core 2.1 et ultérieur prend en charge [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) et [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) pour les pages Razor. Pour plus d’informations, consultez [Méthodes de filtre pour les pages Razor](xref:razor-pages/filter).

 Les filtres intégrés gèrent notamment les tâches suivantes :

 * Autorisation (empêcher un utilisateur non autorisé d’accéder à des ressources).
 * Vérification de l’utilisation du protocole HTTPS par toutes les requêtes.
 * Mise en cache des réponses (court-circuitage du pipeline de requêtes pour retourner une réponse mise en cache). 

Il est possible de créer des filtres personnalisés pour gérer les problèmes transversaux. Les filtres peuvent éviter la duplication de code entre les actions. Par exemple, un filtre d’exceptions de gestion des erreurs peut servir à consolider la gestion des erreurs.

[Affichez ou téléchargez un exemple depuis GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).

## <a name="how-do-filters-work"></a>Fonctionnement des filtres

Les filtres s’exécutent dans le *pipeline des appels d’actions MVC*, parfois appelé *pipeline de filtres*.  Le pipeline de filtres s’exécute après la sélection par MVC de l’action à exécuter.

![La requête est traitée via un autre intergiciel, un intergiciel de routage, une sélection d’action et le pipeline d’appels d’actions MVC. Le traitement de la requête se poursuit via une sélection d’action, un intergiciel de routage et différents autres intergiciels avant de devenir une réponse envoyée au client.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>Types de filtres

Chaque type de filtre est exécuté dans une étape différente du pipeline de filtres.

* Les [filtres d’autorisations](#authorization-filters) s’exécutent en premier et sont utilisés pour déterminer si l’utilisateur actif est autorisé pour la requête active. Ils peuvent court-circuiter le pipeline si une requête n’est pas autorisée. 

* Les [filtres de ressources](#resource-filters) sont les premiers à traiter une requête après autorisation.  Ils peuvent exécuter du code avant le reste du pipeline de filtres, et après que le reste du pipeline est terminé. Ils sont pratiques pour implémenter la mise en cache ou pour court-circuiter le pipeline de filtres pour des raisons de performances. Comme ils s’exécutent avant la liaison de données, ils peuvent l’influencer.

* Les [filtres d’actions](#action-filters) peuvent exécuter du code juste avant et juste après l’appel d’une méthode d’action individuelle. Ils peuvent être utilisés pour manipuler les arguments passés à une action et le résultat retourné depuis l’action.

* Les [filtres d’exceptions](#exception-filters) sont utilisés pour appliquer des stratégies globales à des exceptions non gérées qui se produisent avant que des éléments aient été écrits dans le corps de la réponse.

* Les [filtres de résultats](#result-filters) peuvent exécuter du code juste avant et juste après l’exécution des résultats d’une action individuelle. Ils s’exécutent uniquement au terme de l’exécution de la méthode d’action. Ils sont utiles pour la logique qui doit entourer l’exécution de la vue ou du formateur.

Le diagramme suivant montre comment ces types de filtres interagissent dans le pipeline de filtres.

![La requête est traitée à travers les filtres d’autorisations, les filtres de ressources, la liaison de modèle, les filtres d’actions, l’exécution d’actions et la conversion des résultats d’actions, les filtres d’exceptions, les filtres de résultats et l’exécution de résultats. En sortie, la requête est traitée seulement par les filtres de résultats et les filtres de ressources avant de devenir une réponse envoyée au client.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>Implémentation

Les filtres prennent en charge les implémentations synchrones et asynchrones via différentes définitions d’interface. 

Les filtres synchrones qui peuvent exécuter du code avant et après leur étape de pipeline définissent les méthodes On*Stage*Executing et On*Stage*Executed. Par exemple, `OnActionExecuting` est appelée avant l’appel de la méthode d’action, et `OnActionExecuted` est appelée après que la méthode d’action retourne.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet1)]

Les filtres asynchrones définissent une méthode On*Stage*ExecutionAsync. Cette méthode prend un délégué*FilterType*ExecutionDelegate qui exécute l’étape du pipeline du filtre. Par exemple, `ActionExecutionDelegate` appelle la méthode d’action ou le filtre d’action suivant, et vous pouvez exécuter du code avant et après l’appel de cette méthode.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

Vous pouvez implémenter des interfaces pour plusieurs étapes de filtres dans une même classe. Par exemple, la classe [ActionFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute?view=aspnetcore-2.0) implémente `IActionFilter`, `IResultFilter` et leurs équivalents asynchrones.

> [!NOTE]
> Implémentez la version synchrone **ou bien** la version asynchrone d’une interface de filtre, mais pas les deux. Le framework vérifie d’abord si le filtre implémente l’interface asynchrone et, le cas échéant, il appelle cette interface. Dans le cas contraire, il appelle la ou les méthodes de l’interface synchrone. Si vous implémentez les deux interfaces sur une même classe, seule la méthode asynchrone est appelée. Quand vous utilisez des classes abstraites comme [ActionFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute?view=aspnetcore-2.0), vous devez remplacer seulement les méthodes synchrones ou bien la méthode asynchrone pour chaque type de filtre.

### <a name="ifilterfactory"></a>IFilterFactory

[IFilterFactory](/dotnet/api/microsoft.aspnetcore.mvc.filters.ifilterfactory) implémente [IFilterMetadata](/dotnet/api/microsoft.aspnetcore.mvc.filters.ifiltermetadata). Par conséquent, une instance de `IFilterFactory` peut être utilisée comme instance de `IFilterMetadata` n’importe où dans le pipeline de filtres. Quand le framework se prépare à appeler le filtre, il tente d’effectuer un cast en `IFilterFactory`. Si ce cast réussit, la méthode [CreateInstance](/dotnet/api/microsoft.aspnetcore.mvc.filters.ifilterfactory.createinstance) est appelée pour créer l’instance de `IFilterMetadata` qui sera appelée. La conception est flexible, car il n’est pas nécessaire de définir explicitement le pipeline de filtres exact quand l’application démarre.

Une autre approche pour la création de filtres est d’implémenter `IFilterFactory` sur vos propres implémentations d’attributs :

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

### <a name="built-in-filter-attributes"></a>Attributs de filtre intégrés

Le framework inclut les filtres intégrés basés sur des attributs que vous pouvez définir dans une sous-classe et personnaliser. Par exemple, le filtre de résultats suivant ajoute un en-tête à la réponse.

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/AddHeaderAttribute.cs?highlight=5,16)]

Les attributs autorisent les filtres à accepter des arguments, comme montré dans l’exemple ci-dessus. Vous pouvez ajouter cet attribut à une méthode de contrôleur ou d’action, et spécifier le nom et la valeur de l’en-tête HTTP :

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

Le résultat de l’action `Index` est montré ci-dessous : les en-têtes de réponse apparaissent dans la partie inférieure droite.

![Les outils de développement de Microsoft Edge affichant des en-têtes de réponse, notamment l’auteur Steve Smith@ardalis](filters/_static/add-header.png)

Plusieurs des interfaces de filtre ont des attributs correspondants qui peuvent être utilisés comme classes de base pour des implémentations personnalisées.

Les attributs de filtre :

* `ActionFilterAttribute`
* `ExceptionFilterAttribute`
* `ResultFilterAttribute`
* `FormatFilterAttribute`
* `ServiceFilterAttribute`
* `TypeFilterAttribute`

`TypeFilterAttribute` et `ServiceFilterAttribute` sont expliqués [ plus loin dans cet article](#dependency-injection).

## <a name="filter-scopes-and-order-of-execution"></a>Étendues de filtre et ordre d’exécution

Un filtre peut être ajouté au pipeline à une parmi trois *étendues*. Vous pouvez ajouter un filtre à une méthode d’action particulière ou à une classe de contrôleur en utilisant un attribut. Vous pouvez également enregistrer un filtre globalement pour l’ensemble des contrôleurs et des actions. Pour ajouter des filtres globalement, ajoutez-les à la collection `MvcOptions.Filters` dans `ConfigureServices` :

[!code-csharp[](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

### <a name="default-order-of-execution"></a>Ordre d’exécution par défaut

Quand il existe plusieurs filtres pour une étape particulière du pipeline, l’étendue détermine l’ordre par défaut de l’exécution du filtre.  Les filtres globaux entourent les filtres de classe, qui à leur tour entourent les filtres de méthode. On parle parfois d’imbrication en « poupées russes », car chaque accroissement d’étendue englobe l’étendue précédente, comme une [poupée gigogne](https://wikipedia.org/wiki/Matryoshka_doll). En règle générale, vous obtenez le comportement de remplacement souhaité sans avoir à déterminer l’ordre explicitement.

En raison de cette imbrication, le code *après* des filtres s’exécute dans l’ordre inverse du code *avant*. La séquence ressemble à ceci :

* Le code *avant* des filtres appliqués globalement
  * Le code *avant* des filtres appliqués aux contrôleurs
    * Le code *avant* des filtres appliqués aux méthodes d’action
    * Le code *après* des filtres appliqués aux méthodes d’action
  * Le code *après* des filtres appliqués aux contrôleurs
* Le code *après* des filtres appliqués globalement
  
Voici un exemple qui illustre l’ordre dans lequel les méthodes de filtre sont appelées pour les filtres d’actions synchrones.

| Séquence | Étendue de filtre | Méthode de filtre |
|:--------:|:------------:|:-------------:|
| 1 | Global | `OnActionExecuting` |
| 2 | Contrôleur | `OnActionExecuting` |
| 3 | Méthode | `OnActionExecuting` |
| 4 | Méthode | `OnActionExecuted` |
| 5 | Contrôleur | `OnActionExecuted` |
| 6 | Global | `OnActionExecuted` |

Cette séquence montre que :

* Le filtre de méthode est imbriqué dans le filtre de contrôleur.
* Le filtre de contrôleur est imbriqué dans le filtre global. 

En d’autres termes, si vous êtes à l’intérieur d’un méthode On*Stage*ExecutionAsync d’un filtre asynchrone, tous les filtres avec une étendue plus étroite s’exécutent alors que votre code est sur la pile.

> [!NOTE]
> Chaque contrôleur qui hérite de la classe de base `Controller` inclut des méthodes `OnActionExecuting` et `OnActionExecuted`. Ces méthodes encapsulent les filtres qui s’exécutent pour une action donnée : `OnActionExecuting` est appelée avant tous les filtres, et `OnActionExecuted` est appelée après tous les filtres.

### <a name="overriding-the-default-order"></a>Remplacement de l’ordre par défaut

Vous pouvez remplacer la séquence d’exécution par défaut en implémentant `IOrderedFilter`. Cette interface expose une propriété `Order` qui est prioritaire sur l’étendue afin de déterminer l’ordre d’exécution. Un filtre avec une valeur `Order` inférieure verra son code *avant* exécuté avant celui d’un filtre avec une valeur plus élevée de `Order`. Un filtre avec une valeur `Order` inférieure verra son code *après* exécuté après celui d’un filtre avec une valeur plus élevée de `Order`. Vous pouvez définir la propriété `Order` en utilisant un paramètre de constructeur :

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

Si vous avez les 3 mêmes filtres d’action de l’exemple précédent, mais que vous définissez la propriété `Order` du filtre de contrôleur et du filtre global respectivement sur 1 et sur 2, l’ordre d’exécution est inversé.

| Séquence | Étendue de filtre | Propriété`Order`  | Méthode de filtre |
|:--------:|:------------:|:-----------------:|:-------------:|
| 1 | Méthode | 0 | `OnActionExecuting` |
| 2 | Contrôleur | 1  | `OnActionExecuting` |
| 3 | Global | 2  | `OnActionExecuting` |
| 4 | Global | 2  | `OnActionExecuted` |
| 5 | Contrôleur | 1  | `OnActionExecuted` |
| 6 | Méthode | 0  | `OnActionExecuted` |

La propriété `Order` prévaut sur l’étendue lors de la détermination de l’ordre dans lequel les filtres s’exécutent. Les filtres sont d’abord classés par ordre, puis l’étendue est utilisée pour couper les liens. Tous les filtres intégrés implémentent `IOrderedFilter` et affectent 0 à la valeur `Order` par défaut. Pour les filtres intégrés, l’étendue détermine l’ordre, sauf si vous affectez à `Order` une valeur différente de zéro.

## <a name="cancellation-and-short-circuiting"></a>Annulation et court-circuit

Vous pouvez court-circuiter le pipeline de filtres à n’importe quel endroit en définissant la propriété `Result` sur le paramètre `context` fourni à la méthode de filtre. Par exemple, le filtre de ressources suivant empêche l’exécution du reste du pipeline.

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

Dans le code suivant, les filtres `ShortCircuitingResourceFilter` et `AddHeader` ciblent tous deux la méthode d’action `SomeResource`. Voici le `ShortCircuitingResourceFilter` :

* S’exécute en premier (puisqu’il s’agit d’un filtre de ressources et que `AddHeader` est un filtre d’action).
* Court-circuite le reste du pipeline.

Le filtre `AddHeader` ne s’exécute donc jamais pour l’action `SomeResource`. Ce comportement est le même si les deux filtres sont appliqués au niveau de la méthode d’action, à condition que `ShortCircuitingResourceFilter` soit exécuté en premier. `ShortCircuitingResourceFilter` s’exécute en premier en raison de son type de filtre ou de l’utilisation explicite de la propriété `Order`.

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a>Injection de dépendances

Les filtres peuvent être ajoutés par type ou par instance. Si vous ajoutez une instance, celle-ci sera utilisée pour chaque requête. Si vous ajoutez un type, il sera activé par type, ce qui signifie qu’une instance sera créée pour chaque requête et toutes les dépendances du constructeur seront remplies par [injection de dépendances](../../fundamentals/dependency-injection.md). L’ajout d’un filtre par type équivaut à `filters.Add(new TypeFilterAttribute(typeof(MyFilter)))`.

Les filtres qui sont implémentés en tant qu’attributs et ajoutés directement à des classes de contrôleur ou à de méthodes d’action ne peut pas avoir de dépendances de constructeur fournies par [injection de dépendances](../../fundamentals/dependency-injection.md). La raison en est que les paramètres du constructeur des attributs doivent être fournis là où ils sont appliqués. Il s’agit d’une limitation de la façon dont les attributs fonctionnent.

Si vos filtres ont des dépendances auxquelles vous devez accéder à partir de l’injection de dépendances, plusieurs approches sont prises en charge. Vous pouvez appliquer votre filtre à une méthode de classe ou d’action de l’une des façons suivantes :

* `ServiceFilterAttribute`
* `TypeFilterAttribute`
* `IFilterFactory` implémenté sur votre attribut

> [!NOTE]
> Un journaliseur est une dépendance que vous pouvez obtenir de l’injection de dépendances. Évitez cependant de créer et d’utiliser des filtres uniquement à des fins de journalisation, car les [fonctionnalités de journalisation intégrées du framework](xref:fundamentals/logging/index) peuvent parfois fournir ce dont vous avez besoin. Si vous voulez ajouter une journalisation à vos filtres, elle doit être centrée sur les problèmes du domaine métier ou sur un comportement spécifique à votre filtre, au lieu de porter sur les actions de MVC ou sur d’autres événements du framework.

### <a name="servicefilterattribute"></a>ServiceFilterAttribute

Les types d’implémentation du filtre de service sont enregistrés dans l’injection de dépendances. Un `ServiceFilterAttribute` récupère une instance du filtre à partir de l’injection de dépendances. Ajoutez `ServiceFilterAttribute` au conteneur dans `Startup.ConfigureServices`, et vous le référencez dans un attribut `[ServiceFilter]` :

[!code-csharp[](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=11)]

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

Lorsque vous utilisez `ServiceFilterAttribute`, la définition de `IsReusable` indique que l’instance de filtre *peut* être réutilisée en dehors de l’étendue de la requête dans laquelle il a été créé. Le framework ne fournit aucune garantie qu’une seule instance du filtre sera créée ou que le filtre ne sera pas demandé à nouveau par le conteneur d’injection de dépendances à un stade ultérieur. Évitez d’utiliser `IsReusable` lors de l’utilisation d’un filtre qui dépend de services avec une durée de vie autre que singleton.

L’utilisation de `ServiceFilterAttribute` sans inscription du type de filtre provoque une exception :

```
System.InvalidOperationException: No service for type
'FiltersSample.Filters.AddHeaderFilterWithDI' has been registered.
```

L'objet `ServiceFilterAttribute` implémente l'objet `IFilterFactory`. `IFilterFactory` expose la méthode `CreateInstance` pour la création d’une instance `IFilterMetadata`. La méthode `CreateInstance` charge le type spécifié à partir du conteneur de services (injection de dépendances).

### <a name="typefilterattribute"></a>TypeFilterAttribute

`TypeFilterAttribute` est similaire à `ServiceFilterAttribute`, mais son type n’est pas résolu directement à partir du conteneur d’injection de dépendances. Il instancie le type en utilisant `Microsoft.Extensions.DependencyInjection.ObjectFactory`.

En raison de cette différence :

* Les types qui sont référencés avec `TypeFilterAttribute` ne doivent pas d’abord être inscrits auprès du conteneur.  Leurs dépendances sont remplies par le conteneur. 
* `TypeFilterAttribute` peut éventuellement accepter des arguments de constructeur pour le type.

Lorsque vous utilisez `TypeFilterAttribute`, la définition de `IsReusable` indique que l’instance de filtre *peut* être réutilisée en dehors de l’étendue de la requête dans laquelle il a été créé. Le framework ne fournit aucune garantie qu’une seule instance du filtre sera créée. Évitez d’utiliser `IsReusable` lors de l’utilisation d’un filtre qui dépend de services avec une durée de vie autre que singleton.

L’exemple suivant montre comment passer des arguments à un type en utilisant `TypeFilterAttribute` :

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

### <a name="ifilterfactory-implemented-on-your-attribute"></a>IFilterFactory implémenté sur votre attribut

Si vous avez un filtre qui :

* Ne nécessite pas d’arguments.
* A des dépendances de constructeur qui doivent être remplies par l’injection de dépendances.

Vous pouvez utiliser votre propre attribut nommé sur les classes et méthodes à la place de `[TypeFilter(typeof(FilterType))]`. Le filtre suivant montre comment vous pouvez implémenter ceci :

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

Ce filtre peut être appliqué à des classes ou des méthodes avec la syntaxe `[SampleActionFilter]`, au lieu de devoir utiliser `[TypeFilter]` ou `[ServiceFilter]`.

## <a name="authorization-filters"></a>Filtres d’autorisations

Les *filtres d’autorisations* :

* Contrôlent l’accès aux méthodes d’action.
* Sont les premiers filtres à être exécutée dans le pipeline de filtres. 
* Ont une méthode avant, mais pas de méthode après. 

Vous devez écrire un filtre d’autorisation personnalisé seulement si vous écrivez votre propre framework d’autorisation. Préférez la configuration de vos stratégies d’autorisation ou l’écriture d’une stratégie d’autorisation personnalisée à l’écriture d’un filtre personnalisé. L’implémentation des filtres intégrés est responsable seulement de l’appel du système d’autorisation.

Vous ne devez pas lever d’exceptions dans les filtres d’autorisations, car rien ne gère les exceptions (les filtres d’exceptions ne les gèrent pas). Songez à émettre un challenge quand une exception se produit.

Découvrez plus d’informations sur [l’autorisation](xref:security/authorization/introduction).

## <a name="resource-filters"></a>Filtres de ressources

* Implémentez l’interface `IResourceFilter` ou `IAsyncResourceFilter`.
* Leur exécution inclut dans un wrapper la majeure partie du pipeline de filtres. 
* Seuls les [filtres d’autorisations](#authorization-filters) s’exécutent avant les filtres de ressources.

Les filtres de ressources sont utiles pour court-circuiter la majeure partie du travail effectué par une requête. Par exemple, un filtre de mise en cache peut éviter le reste du pipeline si la réponse est dans le cache.

Le [filtre de ressources de court-circuit](#short-circuiting-resource-filter) montré plus haut est un exemple de filtre de ressources. Un autre exemple est [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs) :

* Il empêche la liaison de données d’accéder aux données de formulaire. 
* Il est utile pour les chargements de fichiers volumineux et pour empêcher que le formulaire ne soit lu en mémoire.

## <a name="action-filters"></a>Filtres d’actions

*Filtres d’actions* :

* Implémentez l’interface `IActionFilter` ou `IAsyncActionFilter`.
* Leur exécution entoure l’exécution de méthodes d’action.

Voici un exemple de filtre d’actions :

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

[ActionExecutingContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionexecutingcontext) fournit les propriétés suivantes :

* `ActionArguments` : vous permet de manipuler les entrées de l’action.
* `Controller` : vous permet de manipuler l’instance de contrôleur. 
* `Result` : la définition de ce paramètre court-circuite l’exécution de la méthode d’action et les filtres d’actions suivants. Lever une exception empêche également l’exécution de la méthode d’action et des filtres suivants, mais elle est traitée comme une erreur au lieu d’un résultat correct.

[ActionExecutedContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionexecutedcontext) fournit `Controller` et `Result`, plus les propriétés suivantes :

* `Canceled` : sa valeur est true si l’exécution de l’action a été court-circuitée par un autre filtre.
* `Exception` : sa valeur est non Null si l’action ou un filtre d’actions suivant a levé une exception. La définition de cette propriété sur Null « gère » effectivement une exception, et `Result` est exécuté comme s’il avait été retourné normalement depuis la méthode d’action.

Pour un `IAsyncActionFilter`, un appel à `ActionExecutionDelegate` :

* Exécute tous les filtres d’actions suivants et la méthode d’action.
* Retourne `ActionExecutedContext`. 

Pour court-circuiter, affectez `ActionExecutingContext.Result` à une instance de résultat et n’appelez pas le `ActionExecutionDelegate`.

Le framework fournit un `ActionFilterAttribute` abstrait que vous pouvez placer dans une sous-classe. 

Vous pouvez utiliser un filtre d’actions pour valider l’état du modèle et retourner des erreurs si l’état n’est pas valide :

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

La méthode `OnActionExecuted` s’exécute après la méthode d’action, et peut voir et manipuler les résultats de l’action via la propriété `ActionExecutedContext.Result`. `ActionExecutedContext.Canceled` est défini sur true si l’exécution de l’action a été court-circuitée par un autre filtre. `ActionExecutedContext.Exception` est défini sur une valeur non Null si l’action ou un filtre d’actions suivant a levé une exception. Le fait de définir `ActionExecutedContext.Exception` avec la valeur null :

* « Gère » effectivement une exception.
* `ActionExectedContext.Result` est exécuté comme s’il avait été retourné normalement à partir de la méthode d’action.

## <a name="exception-filters"></a>Filtres d’exceptions

Les *filtres d’exceptions* implémentent l’interface `IExceptionFilter` ou `IAsyncExceptionFilter`. Ils peuvent être utilisés pour implémenter des stratégies de gestion des erreurs courantes pour une application. 

L’exemple de filtre d’exceptions suivant utilise une vue personnalisée des erreurs pour le développeur, pour afficher des détails sur les exceptions qui se produisent pendant que l’application est en développement :

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

Les filtres d’exceptions :

* N’ont pas d’événements avant et après. 
* Implémentent `OnException` ou `OnExceptionAsync`. 
* Gèrent les exceptions non gérées qui se produisent dans la création des contrôleurs, la [liaison de données](../models/model-binding.md), les filtres d’actions ou les méthodes d’action. 
* N’interceptent pas les exceptions qui se produisent dans l’exécution des filtres de ressources, des filtres de résultats ou des résultats MVC.

Pour gérer une exception, définissez la propriété `ExceptionContext.ExceptionHandled` ou écrivez une réponse. Ceci arrête la propagation de l’exception. Un filtre d’exceptions ne peut pas changer une exception en « réussite ». Seul un filtre d’actions peut le faire.

> [!NOTE]
> Dans ASP.NET Core 1.1, la réponse n’est pas envoyée si vous définissez `ExceptionHandled` avec la valeur true **et** que vous écrivez une réponse. Dans ce scénario, ASP.NET Core 1.0 envoie la réponse, et ASP.NET Core 1.1.2 revient au comportement de la version 1.0. Pour plus d’informations, consultez le [problème #5594](https://github.com/aspnet/Mvc/issues/5594) dans le dépôt GitHub. 

Les filtres d’exceptions :

* Conviennent pour intercepter des exceptions qui se produisent dans les actions MVC.
* N’offrent pas la même souplesse que le middleware (intergiciel) de gestion des erreurs. 

Privilégiez le middleware pour la gestion des exceptions. Utilisez uniquement des filtres d’exceptions si vous devez gérer les erreurs *différemment* en fonction de l’action MVC choisie. Par exemple, votre application peut avoir des méthodes d’action à la fois pour des points de terminaison d’API et pour des vues/HTML. Les points de terminaison d’API peuvent retourner des informations d’erreur au format JSON, tandis que les actions basées sur une vue peuvent retourner une page d’erreur au format HTML.

`ExceptionFilterAttribute` peut être sous-classé. 

## <a name="result-filters"></a>Filtres de résultats

* Implémentez l’interface `IResultFilter` ou `IAsyncResultFilter`.
* Leur exécution entoure l’exécution de résultats d’action. 

Voici un exemple de filtre de résultats qui ajoute un en-tête HTTP.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

Le type de résultat à exécuter dépend de l’action en question. Une action MVC retournant une vue inclut tous les traitements Razor dans le cadre de la `ViewResult` à exécuter. Une méthode d’API peut effectuer une sérialisation dans le cadre de l’exécution du résultat. Découvrez plus d’informations sur les [résultats d’actions](actions.md).

Les filtres de résultats sont exécutés seulement pour les résultats qui sont des réussites, quand l’action ou les filtres d’actions produisent un résultat d’action. Les filtres de résultats ne sont pas exécutés quand les filtres d’exceptions gèrent une exception.

La méthode `OnResultExecuting` peut court-circuiter l’exécution du résultat d’action et les filtres de résultats suivants en définissant `ResultExecutingContext.Cancel` sur true. D’une façon générale, vous devez écrire dans l’objet de réponse quand vous court-circuitez pour éviter de générer une réponse vide. La levée d’une exception :

* Empêche l’exécution du résultat d’action et des filtres suivants.
* Est traitée comme une erreur et non comme une réussite.

Quand la méthode `OnResultExecuted` s’exécute, la réponse a probablement déjà été envoyée au client et ne peut plus être changée (sauf si une exception a été levée). `ResultExecutedContext.Canceled` est défini sur true si l’exécution du résultat d’action a été court-circuitée par un autre filtre.

`ResultExecutedContext.Exception` est défini sur une valeur non Null si le résultat d’action ou un filtre de résultats suivant a levé une exception. Le fait de définir `Exception` sur Null « gère » effectivement une exception et évite à l’exception d’être à nouveau levée par MVC plus loin dans le pipeline. Quand vous gérez une exception dans un filtre de résultats, il peut être impossible d’écrire des données dans la réponse. Si le résultat d’une action lève une exception à mi-chemin de son exécution et que les en-têtes ont déjà été envoyés au client, il n’existe aucun mécanisme fiable pour envoyer un code d’échec.

Pour un `IAsyncResultFilter`, un appel à `await next` sur le `ResultExecutionDelegate` exécute tous les filtres de résultats suivants et le résultat de l’action. Pour court-circuiter, définissez `ResultExecutingContext.Cancel` sur true et n’appelez pas le `ResultExectionDelegate`.

Le framework fournit un `ResultFilterAttribute` abstrait que vous pouvez placer dans une sous-classe. La classe [AddHeaderAttribute](#add-header-attribute) ci-dessus est un exemple d’un attribut de filtre de résultats.

## <a name="using-middleware-in-the-filter-pipeline"></a>Utilisation d’un intergiciel dans le pipeline de filtres

Les filtres de ressources fonctionnent comme un [intergiciel](xref:fundamentals/middleware/index) dans la mesure où ils entourent l’exécution de tout ce qui vient plus tard dans le pipeline. Les filtres diffèrent cependant d’un intergiciel dans la mesure où ils font partie de MVC, ce qui signifie qu’ils ont accès au contexte et aux constructions MVC.

Dans ASP.NET Core 1.1, vous pouvez utiliser un intergiciel dans le pipeline de filtres. Vous pouvez faire cela si vous avez un composant d’intergiciel qui doit accéder aux données de route de MVC, ou qui doit s’exécuter seulement pour certains contrôleurs ou certaines actions.

Pour utiliser un intergiciel comme filtre, créez un type avec une méthode `Configure` qui spécifie l’intergiciel que vous voulez injecter dans le pipeline de filtres. Voici un exemple qui utilise l’intergiciel de localisation pour établir la culture d’une requête :

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

Vous pouvez ensuite utiliser `MiddlewareFilterAttribute` pour exécuter l’intergiciel pour un contrôleur ou une action sélectionnés, ou globalement :

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

Les filtres d’intergiciels s’exécutent à la même étape du pipeline de filtres en tant que filtres de ressources, avant la liaison de modèle et après le reste du pipeline.

## <a name="next-actions"></a>Actions suivantes

Pour expérimenter les filtres, [téléchargez, testez et modifiez l’exemple](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).
