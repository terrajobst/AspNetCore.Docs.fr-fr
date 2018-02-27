---
title: "État de session et d’application dans ASP.NET Core"
author: rick-anderson
description: "Aborde la conservation de l’état de session utilisateur et d’application entre chaque requête."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/app-state
ms.openlocfilehash: f4ed38f7395e3f4fe939584c1f3f5b0dba93724c
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2018
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a>Présentation de l’état de session et d’application dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/) et [Diana LaRose](https://github.com/DianaLaRose)

HTTP est un protocole sans état. Un serveur web traite chaque requête HTTP comme une requête indépendante et ne conserve pas les valeurs utilisateur des requêtes précédentes. Cet article décrit différentes façons de conserver l’état de session et d’application d’une requête à l’autre. 

## <a name="session-state"></a>État de session

L’état de session est une fonctionnalité ASP.NET Core que vous pouvez utiliser pour enregistrer et stocker des données utilisateur pendant que l’utilisateur navigue dans votre application web. Constitué d’une table de hachage ou de dictionnaires sur le serveur, l’état de session conserve les données entre chaque requête reçue d’un navigateur. Les données de session sont stockées dans un cache.

ASP.NET Core maintient l’état de session en donnant au client un cookie qui contient l’ID de session, qui est envoyée au serveur avec chaque demande. Le serveur utilise l’ID de session pour extraire les données de session. Étant donné que le cookie de session est spécifique au navigateur, vous ne peuvez pas partager des sessions dans les navigateurs. Les cookies de session sont supprimées uniquement lorsque la session se termine. Si un cookie est reçu pour une session qui a expiré, une nouvelle session qui utilise le même cookie de session est créée. 

Le serveur conserve une session pendant une période limitée après la dernière demande. Vous pouvez définir le délai d’expiration d'une session ou utiliser la valeur par défaut de 20 minutes. L'état de session est idéal pour le stockage des données utilisateur qui sont spécifiques à une session particulière, mais ne doivent pas être persistantes de façon permanente. Les données sont supprimées du magasin de stockage lorsque vous appelez `Session.Clear` ou à l’expiration de la session dans le magasin de données. Le serveur ne peut pas détecter que le navigateur est fermé ou que le cookie de session est supprimé.

> [!WARNING]
> Ne stockez pas de données sensibles dans une session. Le client risque de ne pas fermer le navigateur et de ne pas effacer le cookie de session (et certains navigateurs maintiennent les cookies de session actifs entre les fenêtres). De plus, une session n’est pas toujours limitée à un seul utilisateur ; l’utilisateur suivant risque donc de continuer avec la même session.

Le fournisseur de session en mémoire stocke les données de session sur le serveur local. Si vous envisagez d’exécuter votre application web sur une batterie de serveurs, vous devez utiliser des sessions rémanentes pour lier chaque session à un serveur spécifique. La plateforme Sites Web Microsoft Azure utilise par défaut des sessions rémanentes (Application Request Routing ou ARR). Toutefois, les sessions rémanentes peuvent impacter l’extensibilité et compliquer les mises à jour des applications web. Une meilleure option consiste à utiliser les caches distribués Redis ou SQL Server, qui ne nécessitent pas de sessions rémanentes. Pour plus d’informations, consultez [Utilisation d’un cache distribué](xref:performance/caching/distributed). Pour plus d’informations sur la configuration des fournisseurs de services, consultez [Configuration de Session](#configuring-session), plus loin dans cet article.

<a name="temp"></a>
## <a name="tempdata"></a>TempData

ASP.NET Core MVC expose la propriété [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) sur un [contrôleur](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0). Cette propriété stocke les données jusqu’à ce qu’elles soient lues. Vous pouvez utiliser les méthodes `Keep` et `Peek` pour examiner les données sans suppression. `TempData` est particulièrement utile pour la redirection, quand des données sont nécessaires pour plusieurs requêtes. `TempData` est implémenté par des fournisseurs TempData, par exemple, à l’aide de cookies ou de l’état de session.

<a name="tempdata-providers"></a>
### <a name="tempdata-providers"></a>Fournisseurs TempData

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Dans ASP.NET Core 2.0 et les versions ultérieures, le fournisseur TempData basé sur les cookies est utilisé par défaut pour stocker TempData dans des cookies.

Les données de cookie sont encodées avec [Base64UrlTextEncoder](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0). Étant donné que le cookie est chiffré et mémorisé en bloc, la limite de taille d’un cookie définie dans ASP.NET Core 1.x ne s’applique pas. Les données de cookie ne sont pas compressées, car la compression de données chiffrées peut entraîner des problèmes de sécurité, notamment des attaques [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) et [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)). Pour plus d’informations sur le fournisseur TempData basé sur les cookies, consultez [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Dans ASP.NET Core 1.0 et 1.1, le fournisseur TempData d’état de session est utilisé par défaut.

--------------

<a name="choose-temp"></a>
### <a name="choosing-a-tempdata-provider"></a>Choix d’un fournisseur TempData

Pour choisir un fournisseur TempData, vous devez tenir compte de plusieurs points. Par exemple :

1. L’application utilise-t-elle déjà l’état de session à d’autres fins ? Si c’est le cas, l’utilisation du fournisseur TempData d’état de session n’entraîne pas de surcoût pour l’application (hormis la taille des données).
2. L’application utilise-t-elle TempData seulement de façon ponctuelle, pour des quantités de données relativement petites (jusqu’à 500 octets) ? Si c’est le cas, le fournisseur TempData de cookies ajoute un petit coût à chaque requête traitée par TempData. Si ce n’est pas le cas, le fournisseur TempData d’état de session peut être utile pour éviter l’aller-retour d’une grande quantité de données dans chaque requête jusqu’à ce que le TempData soit consommé.
3. L’application s’exécute-t-elle dans une batterie de serveurs Web ? Si c’est le cas, aucune configuration supplémentaire n’est nécessaire pour utiliser le fournisseur TempData de cookies.

> [!NOTE]
> La plupart des clients web (par exemple, les navigateurs web) appliquent des limites pour la taille maximale de chaque cookie et/ou le nombre total de cookies. Par conséquent, lorsque vous utilisez le fournisseur TempData de cookies, vérifiez que l’application ne dépasse pas ces limites. Examinez la taille totale des données, en prenant en compte les surcharges de chiffrement et de mémorisation en bloc.

<a name="config-temp"></a>
### <a name="configure-the-tempdata-provider"></a>Configuration du fournisseur TempData

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Le fournisseur TempData basé sur les cookies est activé par défaut. Le code de classe `Startup` suivant configure le fournisseur TempData basé sur les sessions :

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Le code de classe `Startup` suivant configure le fournisseur TempData basé sur les sessions :

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

L’ordre est essentiel pour les composants d’intergiciel (middleware). Dans l’exemple précédent, une exception de type `InvalidOperationException` se produit quand `UseSession` est appelé après `UseMvcWithDefaultRoute`. Pour plus de détails, consultez [Ordre des intergiciels (middleware)](xref:fundamentals/middleware/index#ordering).

> [!IMPORTANT]
> Si vous ciblez .NET Framework et utilisez le fournisseur basé sur les sessions, ajoutez le package NuGet [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) à votre projet.

## <a name="query-strings"></a>Chaînes de requête

Vous pouvez passer une quantité limitée de données d’une requête à une autre en l’ajoutant à la chaîne de la nouvelle requête. Cela est utile pour capturer l’état de manière persistante et permettre ainsi le partage de liens avec état incorporé par e-mail ou sur les réseaux sociaux. Toutefois, pour cette raison même, n’utilisez jamais de chaînes de requête sur des données sensibles. En plus de faciliter le partage des données, l’ajout de données dans des chaînes de requête peut favoriser les attaques par [falsification de requête intersites (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) et conduire des utilisateurs authentifiés à visiter des sites malveillants. Les attaquants peuvent ensuite dérober des données utilisateur à partir de votre application ou effectuer des actions malveillantes en utilisant un compte d’utilisateur. Chaque état de session ou d’application conservé doit garantir une protection contre les attaques CSRF. Pour plus d’informations sur les attaques CSRF, consultez [Prévention des attaques par falsification de requête intersites (XSRF/CSRF) dans ASP.NET Core](../security/anti-request-forgery.md).

## <a name="post-data-and-hidden-fields"></a>Publication de données et champs masqués

Les données peuvent être enregistrées dans des champs de formulaire masqués et être republiées lors de la requête suivante. Cela est courant dans les formulaires de plusieurs pages. Toutefois, comme le client peut potentiellement falsifier les données, le serveur doit toujours les revalider. 

## <a name="cookies"></a>Cookies

Les cookies sont un moyen de stocker des données utilisateur dans les applications web. Les cookies sont envoyés avec chaque requête. Il est donc essentiel de limiter leur taille au minimum. Dans l’idéal, un cookie doit uniquement stocker un identificateur et les données proprement dites qui sont stockées sur le serveur. La plupart des navigateurs limitent les cookies à 4 096 octets. De plus, seul un nombre limité de cookies est disponible pour chaque domaine.  

Pour éviter les risques de falsification, les cookies doivent être validés sur le serveur. La durabilité des cookies sur un client dépend du délai d’expiration et de l’interaction de l’utilisateur, mais les cookies sont généralement la forme la plus durable de persistance des données sur le client.

Les cookies servent souvent à personnaliser le contenu pour un utilisateur connu. Dans la plupart des cas, l’utilisateur est identifié, mais pas authentifié. Vous pouvez généralement sécuriser un cookie en stockant le nom d’utilisateur, le nom de compte ou un ID d’utilisateur unique (par exemple, un GUID) dans le cookie. Vous pouvez ensuite utiliser le cookie pour accéder à l’infrastructure de personnalisation utilisateur d’un site.

## <a name="httpcontextitems"></a>HttpContext.Items

La collection `Items` est un bon emplacement pour stocker des données qui sont nécessaires uniquement pendant le traitement d’une requête particulière. Le contenu de la collection est supprimé après chaque requête. La collection `Items` est surtout utile pour permettre à des composants ou des intergiciels (middleware) de communiquer à différents moments de l’exécution d’une requête quand ils ne peuvent pas passer les paramètres de façon directe. Pour plus d’informations, consultez [Utilisation de la collection HttpContext.Items](#working-with-httpcontextitems), plus loin dans cet article.

## <a name="cache"></a>d'instance/de clé

La mise en cache est un moyen efficace de stocker et récupérer des données. Vous pouvez contrôler la durée de vie des éléments mis en cache en fonction de l’heure et d’autres critères. Découvrez-en plus sur la [mise en cache](../performance/caching/index.md).

<a name="session"></a>
## <a name="working-with-session-state"></a>Utilisation de l’état de session

### <a name="configuring-session"></a>Configuration de Session

Le package `Microsoft.AspNetCore.Session` fournit un middleware pour gérer l’état de session. Pour activer le middleware Session, `Startup` doit contenir les éléments suivants :

- Un des caches mémoire [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache). L’implémentation `IDistributedCache` est utilisée comme magasin de stockage pour la session.
- Un appel [AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_), qui nécessite le package NuGet « Microsoft.AspNetCore.Session ».
- Un appel [UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_).

Le code suivant montre comment configurer le fournisseur de session en mémoire.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

Vous pouvez faire référence à Session à partir de `HttpContext` une fois qu’il a été installé et configuré.

Si vous essayez d’accéder à `Session` avant d’avoir appelé `UseSession`, l’exception `InvalidOperationException: Session has not been configured for this application or request` est levée.

Si vous essayez de créer un `Session` (aucun cookie de session n’ayant encore été créé) après avoir commencé à écrire dans le flux `Response`, l’exception `InvalidOperationException: The session cannot be established after the response has started` est levée. L’exception est enregistrée dans le journal de serveur web, mais elle n’est pas affichée dans le navigateur.

### <a name="loading-session-asynchronously"></a>Chargement asynchrone de Session 

Le fournisseur de session par défaut dans ASP.NET Core charge l’enregistrement de session de façon asynchrone à partir du magasin sous-jacent [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) uniquement si la méthode [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) est appelée explicitement avant les méthodes `TryGetValue`, `Set` ou `Remove`. Si la méthode `LoadAsync` n’est pas appelée en premier, l’enregistrement de session sous-jacent est chargé de façon synchrone, ce qui peut impacter la capacité de mise à l’échelle de l’application.

Pour forcer les applications à appliquer ce modèle, incluez les implémentations [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) et [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) dans un wrapper, avec des versions qui lèvent une exception si la méthode `LoadAsync` n’est pas appelée avant `TryGetValue`, `Set` ou `Remove`. Inscrivez ensuite les versions incluses dans le wrapper auprès du conteneur de services.

### <a name="implementation-details"></a>Détails de l’implémentation

Session utilise un cookie pour suivre et identifier les requêtes reçues d’un navigateur. Par défaut, ce cookie est nommé « AspNet.Session » et utilise un chemin « / ». Étant donné que le cookie par défaut ne spécifie pas de domaine, il n’est pas mis à disposition du script côté client dans la page (car `CookieHttpOnly` a la valeur `true` par défaut).

Pour substituer les valeurs de session par défaut, utilisez `SessionOptions` :

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

Le serveur utilise la propriété `IdleTimeout` pour déterminer la durée pendant laquelle une session peut rester inactive avant que son contenu soit abandonné. Cette propriété est indépendante du délai d’expiration du cookie. Chaque requête qui passe par le middleware Session (en lecture ou en écriture) réinitialise le délai d’expiration.

Étant donné que `Session` *n’est pas bloquante*, si deux requêtes tentent de modifier le contenu de la session, la dernière se substitue à la première. `Session` est implémentée comme une *session cohérente*, ce qui signifie que tout le contenu est stocké au même emplacement. Deux requêtes qui modifient des parties différentes de la session (clés différentes) peuvent toujours avoir un impact l’une sur l’autre.

### <a name="setting-and-getting-session-values"></a>Définition et obtention des valeurs Session

Session est accessible via la propriété `Session` sur `HttpContext`. Cette propriété est une implémentation [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession).

L’exemple suivant montre comment définir et obtenir une valeur int et une valeur string :

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

Si vous ajoutez les méthodes d’extension suivantes, vous pouvez définir et obtenir des objets sérialisables pour Session :

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

L’exemple suivant montre comment définir et obtenir un objet sérialisable :

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a>Utilisation de la collection HttpContext.Items

L’abstraction `HttpContext` fournit la prise en charge d’une collection de dictionnaires de type `IDictionary<object, object>`, appelée `Items`. Cette collection est disponible dès le début d’une requête *HttpRequest*, mais elle est supprimée à la fin de chaque requête. Vous pouvez y accéder en attribuant une valeur à une entrée de clé, ou en demandant la valeur d’une clé particulière.

Dans l’exemple suivant, le [middleware](xref:fundamentals/middleware/index) ajoute `isVerified` à la collection `Items`.

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

Plus tard dans le pipeline, un autre middleware peut y accéder :

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

Si un middleware est destiné uniquement à l’usage d’une seule application, les clés `string` sont acceptables. En revanche, un middleware devant être partagé entre plusieurs applications doit utiliser des clés d’objet uniques afin d’éviter tout risque de collision de clés. Si vous concevez un middleware pour fonctionner sur plusieurs applications, utilisez une clé d’objet unique qui est définie dans la classe du middleware de la façon suivante :

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

Tout autre code peut accéder à la valeur stockée dans `HttpContext.Items` à l’aide de la clé exposée par la classe du middleware :

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

Cette approche a également l’avantage d’éviter la répétition de « chaînes magiques » à plusieurs endroits dans le code.

<a name="appstate-errors"></a>

## <a name="application-state-data"></a>Données d’état d’application

Utilisez [l’injection de dépendances](xref:fundamentals/dependency-injection) pour rendre les données accessibles à tous les utilisateurs :

1. Définissez un service qui contient les données (par exemple, une classe nommée `MyAppData`).

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. Ajoutez la classe de service `ConfigureServices` (par exemple `services.AddSingleton<MyAppData>();`).
3. Consommez la classe de service de données dans chaque contrôleur :

```csharp
public class MyController : Controller
{
    public MyController(MyAppData myService)
    {
        // Do something with the service (read some data from it, 
        // store it in a private field/property, etc.)
    }
} 
```

## <a name="common-errors-when-working-with-session"></a>Erreurs courantes lors de l’utilisation de Session

* « Impossible de résoudre le service pour le type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' lors de la tentative d’activation de 'Microsoft.AspNetCore.Session.DistributedSessionStore'. »

  Cette erreur est généralement due à un échec de configuration d’au moins une implémentation `IDistributedCache`. Pour plus d’informations, consultez [Utilisation d’un cache distribué](xref:performance/caching/distributed) et [Mise en cache mémoire](xref:performance/caching/memory).

* Dans le cas où le middleware Session ne parvient pas à rendre une session persistance (par exemple, si la base de données n’est pas disponible), il enregistre l’exception dans le journal et la supprime. La requête continue ensuite normalement, ce qui entraîne un comportement très imprévisible.

Voici un exemple type :

Un utilisateur stocke un panier d’achat dans la session. L’utilisateur ajoute un élément, mais la validation échoue. L’application n’a pas connaissance de l’échec et affiche incorrectement un message du type « L’élément a été ajouté ».

La méthode recommandée pour rechercher les erreurs de ce type consiste à appeler `await feature.Session.CommitAsync();` à partir du code d’application quand vous avez terminé l’écriture dans Session. Vous pouvez ensuite traiter l’erreur comme vous le souhaitez. Le processus est le même quand vous appelez `LoadAsync`.

### <a name="additional-resources"></a>Ressources supplémentaires

* [ASP.NET Core 1.x : exemple de code utilisé dans ce document](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [ASP.NET Core 2.x : exemple de code utilisé dans ce document](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
