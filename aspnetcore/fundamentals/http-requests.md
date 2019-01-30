---
title: Lancer des requêtes HTTP
author: stevejgordon
description: Découvrez plus d’informations sur l’utilisation de l’interface IHttpClientFactory pour gérer des instances HttpClient logiques dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 01/25/2019
uid: fundamentals/http-requests
ms.openlocfilehash: 4fc4e602b809563ea78b6a3af5e5eb5c0ebeddea
ms.sourcegitcommit: c6db8b14521814f1f7e528d7aa06e474e4c04a1f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2019
ms.locfileid: "55065033"
---
# <a name="initiate-http-requests"></a>Lancer des requêtes HTTP

By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) et [Steve Gordon](https://github.com/stevejgordon)

Une <xref:System.Net.Http.IHttpClientFactory> peut être inscrite et utilisée pour configurer et créer des instances de <xref:System.Net.Http.HttpClient> dans une application. Elle offre les avantages suivants :

* Fournit un emplacement central pour le nommage et la configuration d’instance de `HttpClient` logiques. Par exemple, un client *github* peut être inscrit et configuré pour accéder à GitHub. Un client par défaut peut être inscrit à d’autres fins.
* Codifie le concept de middleware (intergiciel) sortant via la délégation de gestionnaires dans `HttpClient` et fournit des extensions permettant au middleware Polly d’en tirer parti.
* Gère le regroupement et la durée de vie des instances de `HttpClientMessageHandler` sous-jacentes pour éviter les problèmes DNS courants qui se produisent lors de la gestion manuelle des durées de vie de `HttpClient`.
* Ajoute une expérience de journalisation configurable (via `ILogger`) pour toutes les requêtes envoyées via des clients créés par la fabrique.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prérequis

Les projets ciblant .NET Framework nécessitent l’installation du package NuGet [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/). Les projets qui ciblent .NET Core et référencent le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) incluent déjà le package `Microsoft.Extensions.Http`.

## <a name="consumption-patterns"></a>Modèles de consommation

Vous pouvez utiliser `IHttpClientFactory` dans une application de plusieurs façons :

* [Utilisation de base](#basic-usage)
* [Clients nommés](#named-clients)
* [Clients typés](#typed-clients)
* [Clients générés](#generated-clients)

Aucune d’entre elles n’est meilleure qu’une autre. La meilleure approche dépend des contraintes de l’application.

### <a name="basic-usage"></a>Utilisation de base

Vous pouvez inscrire la `IHttpClientFactory` en appelant la méthode d’extension `AddHttpClient` sur la `IServiceCollection`, à l’intérieur la méthode `Startup.ConfigureServices`.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

Une fois inscrit, le code peut accepter un `IHttpClientFactory` partout où des services peuvent être injectés avec [une injection de dépendance](xref:fundamentals/dependency-injection). La `IHttpClientFactory` peut être utilisée pour créer une instance de `HttpClient` :

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

L’utilisation de `IHttpClientFactory` de cette façon est un excellent moyen de refactoriser une application existante. Elle n’a aucun impact sur la façon dont `HttpClient` est utilisé. Dans les endroits où les instances de `HttpClient` sont actuellement créées, remplacez ces occurrences par un appel à <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.

### <a name="named-clients"></a>Clients nommés

Si une application nécessite plusieurs utilisations distinctes de `HttpClient`, chacune avec une configuration différente, une option consiste à utiliser des **clients nommés**. La configuration d’un `HttpClient` nommé peut être spécifiée lors de l’inscription dans `Startup.ConfigureServices`.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

Dans le code précédent, `AddHttpClient` est appelé en fournissant le nom *github*. Une configuration par défaut est appliquée à ce client : l’adresse de base et deux en-têtes nécessaires pour utiliser l’API GitHub.

Chaque fois que `CreateClient` est appelée, une nouvelle instance de `HttpClient` est créée et l’action de configuration est appelée.

Pour utiliser un client nommé, un paramètre de chaîne peut être passé à `CreateClient`. Spécifiez le nom du client à créer :

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

Dans le code précédent, la requête n’a pas besoin de spécifier un nom d’hôte. Elle peut simplement passer le chemin, car l’adresse de base configurée pour le client est utilisée.

### <a name="typed-clients"></a>Clients typés

Les clients typés fournissent les mêmes fonctionnalités que les clients nommés, sans qu’il soit nécessaire d’utiliser des chaînes comme clés. L’approche des clients typés bénéficie de l’aide d’IntelliSense et du compilateur lors de l’utilisation des clients. Ils fournissent un emplacement unique pour configurer et interagir avec un `HttpClient` particulier. Par exemple, un même client typé peut être utilisé pour un point de terminaison de backend et pour encapsuler la logique relative à ce point de terminaison. Un autre avantage est qu’ils fonctionnent avec l’injection de dépendances et qu’ils peuvent être injectés là où c’est nécessaire dans votre application.

Un client typé accepte un `HttpClient` dans son constructeur :

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

Dans le code précédent, la configuration est déplacée dans le client typé. L’objet `HttpClient` est exposé en tant que propriété publique. Il est possible de définir des méthodes d’API spécifiques qui exposent les fonctionnalités de `HttpClient`. La méthode `GetAspNetDocsIssues` encapsule le code nécessaire pour interroger et analyser les problèmes ouverts les plus récents d’un dépôt GitHub.

Pour inscrire un client typé, la méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> générique peut être utilisée dans `Startup.ConfigureServices`, en spécifiant la classe du client typé :

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

Le client typé est inscrit comme étant transitoire avec injection de dépendances. Le client typé peut être injecté et utilisé directement :

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

Si vous préférez, vous pouvez spécifier la configuration d’un client typé lors de l’inscription dans `Startup.ConfigureServices` au lieu de le faire dans le constructeur du client typé :

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

Il est possible d’encapsuler entièrement le `HttpClient` dans un client typé. Au lieu de l’exposer en tant que propriété, vous pouvez fournir des méthodes publiques qui appellent l’instance de `HttpClient` en interne.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

Dans le code précédent, le `HttpClient` est stocké en tant que champ privé. Tous les accès nécessaires pour effectuer des appels externes passent par la méthode `GetRepos`.

### <a name="generated-clients"></a>Clients générés

`IHttpClientFactory` peut être utilisé en combinaison avec d’autres bibliothèques tierces, comme [Refit](https://github.com/paulcbetts/refit). Refit est une bibliothèque REST pour .NET. Il convertit les API REST en interfaces dynamiques. Une implémentation de l’interface est générée dynamiquement par le `RestService`, avec `HttpClient` pour faire les appels HTTP externes.

Une interface et une réponse sont définies pour représenter l’API externe et sa réponse :

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

Vous pouvez ajouter un client typé en utilisant Refit pour générer l’implémentation :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

L’interface définie peut être utilisée quand c’est nécessaire, avec l’implémentation fournie par l’injection de dépendances et Refit :

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a>Middleware pour les requêtes sortantes

`HttpClient` intègre déjà le concept de délégation des gestionnaires qui peuvent être liés ensemble pour les requêtes HTTP sortantes. Le `IHttpClientFactory` permet de définir facilement les gestionnaires à appliquer pour chaque client nommé. Il prend en charge l’inscription et le chaînage de plusieurs gestionnaires pour créer un pipeline de middlewares pour les requêtes sortantes. Chacun de ces gestionnaires peut effectuer un travail avant et après la requête sortante. Ce modèle est similaire au pipeline de middlewares entrants dans ASP.NET Core. Le modèle fournit un mécanisme permettant de gérer les problèmes transversaux liés aux des requêtes HTTP, notamment la mise en cache, la gestion des erreurs, la sérialisation et la journalisation.

Pour créer un gestionnaire, définissez une classe dérivant de <xref:System.Net.Http.DelegatingHandler>. Remplacez la méthode `SendAsync` de façon à exécuter du code avant de passer la requête au gestionnaire suivant dans le pipeline :

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

Le code précédent définit un gestionnaire de base. Il vérifie si un en-tête `X-API-KEY` a été inclus dans la requête. Si l’en-tête est manquant, il peut éviter l’appel HTTP et retourner une réponse appropriée.

Lors de l’inscription, un ou plusieurs gestionnaires peuvent être ajoutés à la configuration pour un `HttpClient`. Cette tâche est accomplie via des méthodes d’extension sur le <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

::: moniker range=">= aspnetcore-2.2"

Dans le code précédent, le `ValidateHeaderHandler` est inscrit avec une injection de dépendances. `IHttpClientFactory` crée une étendue DI distincte pour chaque gestionnaire. Les gestionnaires peuvent librement dépendre des services de n’importe quelle étendue. Les services dont dépendent les gestionnaires sont supprimés lorsque le gestionnaire est supprimé.

Une fois inscrit, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> peut être appelé en passant en entrée le type pour le gestionnaire.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Dans le code précédent, le `ValidateHeaderHandler` est inscrit avec une injection de dépendances. Le gestionnaire **doit** être inscrit dans l’injection de dépendances en tant que service temporaire, sans étendue. Si le gestionnaire est enregistré en tant que service avec une étendue et si tous les services dont le gestionnaire dépend sont disponibles, les services du gestionnaire peuvent être supprimés avant que le gestionnaire soit hors de portée, ce qui provoquerait l’échec du gestionnaire.

Une fois inscrit, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> peut être appelé en passant en entrée le type du gestionnaire.

::: moniker-end

Vous pouvez inscrire plusieurs gestionnaires dans l’ordre où ils doivent être exécutés. Chaque gestionnaire wrappe le gestionnaire suivant jusqu’à ce que le dernier `HttpClientHandler` exécute la requête :

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

Utilisez l’une des approches suivantes pour partager l’état de chaque requête avec les gestionnaires de messages :

* Passez des données dans le gestionnaire en utilisant `HttpRequestMessage.Properties`.
* Utilisez `IHttpContextAccessor` pour accéder à la requête en cours.
* Créez un objet de stockage `AsyncLocal` personnalisé pour passer les données.

## <a name="use-polly-based-handlers"></a>Utiliser les gestionnaires Polly

`IHttpClientFactory` s’intègre à une bibliothèque tierce très utilisée, appelée [Polly](https://github.com/App-vNext/Polly). Polly est une bibliothèque complète de gestion des erreurs transitoires et de résilience pour .NET. Elle permet aux développeurs de formuler facilement et de façon thread-safe des stratégies, comme Retry (Nouvelle tentative), Circuit Breaker (Disjoncteur), Timeout (Délai d’attente), Bulkhead Isolation (Isolation par cloisonnement) et Fallback (Alternative de repli).

Des méthodes d’extension sont fournies pour permettre l’utilisation de stratégies Polly avec les instances configurées de `HttpClient`. Les extensions Polly sont disponibles dans le package NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/). Ce package n’est pas inclus dans le métapackage [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Pour utiliser les extensions, vous devez inclure un `<PackageReference />` explicite dans le projet.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/HttpClientFactorySample.csproj?highlight=9)]

Après la restauration de ce package, les méthodes d’extension sont disponibles pour prendre en charge l’ajout de gestionnaires Polly à des clients.

### <a name="handle-transient-faults"></a>Gérer les erreurs temporaires

Les erreurs courantes se produisent lorsque des appels HTTP externes sont temporaires. Une méthode d’extension pratique nommée `AddTransientHttpErrorPolicy` est incluse : elle permet de définir une stratégie pour gérer les erreurs temporaires. Les stratégies configurées avec cette méthode d’extension gèrent `HttpRequestException`, les réponses HTTP 5xx et les réponses HTTP 408.

L’extension `AddTransientHttpErrorPolicy` peut être utilisée dans `Startup.ConfigureServices`. L’extension fournit l’accès à un objet `PolicyBuilder` configuré pour gérer les erreurs représentant une erreur temporaire possible :

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

Dans le code précédent, une stratégie `WaitAndRetryAsync` est définie. Les requêtes qui ont échoué sont retentées jusqu’à trois fois avec un délai de 600 ms entre les tentatives.

### <a name="dynamically-select-policies"></a>Sélectionner dynamiquement des stratégies

Il existe d’autres méthodes d’extension que vous pouvez utiliser pour ajouter des gestionnaires Polly. Une de ces extensions est `AddPolicyHandler`, qui a plusieurs surcharges. Une de ces surcharges permet l’inspection de la requête lors de la définition de la stratégie à appliquer :

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

Dans le code précédent, si la requête sortante est une opération GET, un délai d’attente de 10 secondes est appliqué. Pour toutes les autres méthodes HTTP, un délai d’attente de 30 secondes est utilisé.

### <a name="add-multiple-polly-handlers"></a>Ajouter plusieurs gestionnaires Polly

Il est courant d’imbriquer des stratégies Polly pour fournir des fonctionnalités améliorées :

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

Dans l’exemple précédent, deux gestionnaires sont ajoutés. Le premier utilise l’extension `AddTransientHttpErrorPolicy` pour ajouter une stratégie de nouvelle tentative. Les requêtes qui ont échoué sont retentées jusqu’à trois fois. Le deuxième appel à `AddTransientHttpErrorPolicy` ajoute une stratégie de disjoncteur. Les requêtes externes supplémentaires sont bloquées pendant 30 secondes si cinq tentatives successives échouent. Les stratégies de disjoncteur sont avec état. Tous les appels effectués via ce client partagent le même état du circuit.

### <a name="add-policies-from-the-polly-registry"></a>Ajouter des stratégies à partir du Registre Polly

Une approche de la gestion des stratégies régulièrement utilisées consiste à les définir une seule fois et à les inscrire avec un `PolicyRegistry`. Il existe une méthode d’extension qui permet l’ajout d’un gestionnaire avec une stratégie du Registre :

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

Dans le code précédent, deux stratégies sont inscrites lorsque `PolicyRegistry` est ajouté à `ServiceCollection`. Pour utiliser une stratégie du Registre, la méthode `AddPolicyHandlerFromRegistry` est utilisée, en passant le nom de la stratégie à appliquer.

Vous trouverez plus d’informations sur les intégrations de `IHttpClientFactory` et de Polly dans le [wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).

## <a name="httpclient-and-lifetime-management"></a>HttpClient et gestion de la durée de vie

Une nouvelle instance `HttpClient` est retournée à chaque fois que `CreateClient` est appelé sur `IHttpClientFactory`. Il existe un <xref:System.Net.Http.HttpMessageHandler> par client nommé. La fabrique gère les durées de vie des instances `HttpMessageHandler`.

`IHttpClientFactory` regroupe dans un pool les instances de `HttpMessageHandler` créées par la fabrique pour réduire la consommation des ressources. Une instance de `HttpMessageHandler` peut être réutilisée à partir du pool quand vous créez une instance de `HttpClient` si sa durée de vie n’a pas expiré.

Le regroupement de gestionnaires en pools est souhaitable, car chaque gestionnaire gère habituellement ses propres connexions HTTP sous-jacentes. La création de plus de gestionnaires que nécessaire peut entraîner des délais de connexion. Certains gestionnaires conservent aussi les connexions indéfiniment ouvertes, ce qui peut empêcher le gestionnaire de réagir aux changements du DNS.

La durée de vie par défaut d’un gestionnaire est de deux minutes. La valeur par défaut peut être remplacée pour chaque client nommé. Pour la remplacer, appelez <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> sur le `IHttpClientBuilder` qui est retourné lors de la création du client :

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

La suppression du client n’est pas nécessaire. La suppression annule les requêtes sortantes et garantit que l’instance `HttpClient` donnée ne peut pas être utilisée après avoir appelé <xref:System.IDisposable.Dispose*>. `IHttpClientFactory` effectue le suivi et libère les ressources utilisées par les instances `HttpClient`. Les instances `HttpClient` peuvent généralement être traitées en tant qu’objets .NET ne nécessitant pas une suppression.

Le fait de conserver une seule instance `HttpClient` active pendant une longue durée est un modèle commun utilisé avant le lancement de `IHttpClientFactory`. Ce modèle devient inutile après la migration vers `IHttpClientFactory`.

## <a name="logging"></a>Journalisation

Les clients créés via `IHttpClientFactory` enregistrent les messages de journalisation pour toutes les requêtes. Activez le niveau d’informations approprié dans votre configuration de journalisation pour voir les messages de journalisation par défaut. Une journalisation supplémentaire, comme celle des en-têtes des requêtes, est incluse seulement au niveau de trace.

La catégorie de journal utilisée pour chaque client comprend le nom du client. Par exemple, un client nommé *MyNamedClient* journalise les messages avec la catégorie `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`. Les messages avec le suffixe *LogicalHandler* se produisent en dehors du pipeline du gestionnaire de requêtes. Lors d’une requête, les messages sont journalisés avant que d’autres gestionnaires du pipeline l’aient traitée. Lors d’une réponse, les messages sont journalisés une fois que tous les autres gestionnaires du pipeline ont reçu la réponse.

La journalisation se produit également à l’intérieur du pipeline du gestionnaire de requêtes. Dans l’exemple *MyNamedClient*, ces messages sont journalisés avec la catégorie de journalisation `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`. Pour la requête, cela se produit après que tous les autres gestionnaires ont été exécutés et immédiatement avant l’envoi de la requête sur le réseau. Lors de la réponse, cette journalisation inclut l’état de la réponse avant qu’elle repasse à travers le pipeline de gestionnaires.

L’activation de la journalisation à l’extérieur et à l’intérieur du pipeline permet l’inspection des changements apportés par les autres gestionnaires du pipeline. Par exemple, cela peut comprendre des changements apportés aux en-têtes des requêtes ou au code d’état de la réponse.

L’ajout du nom du client dans la catégorie de journalisation permet si nécessaire de filtrer le journal pour des clients nommés spécifiques.

## <a name="configure-the-httpmessagehandler"></a>Configurer le HttpMessageHandler

Il peut être nécessaire de contrôler la configuration du `HttpMessageHandler` interne utilisé par un client.

Un `IHttpClientBuilder` est retourné quand vous ajoutez des clients nommés ou typés. La méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> peut être utilisée pour définir un délégué. Le délégué est utilisé pour créer et configurer le `HttpMessageHandler` principal utilisé par ce client :

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]
