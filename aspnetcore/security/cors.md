---
title: Autoriser les demandes de Cross-Origin (CORS) dans ASP.NET Core
author: rick-anderson
description: Découvrez comment CORS comme une norme pour autoriser ou rejeter les demandes cross-origin dans une application ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2018
uid: security/cors
ms.openlocfilehash: f0e01cfa618184d8a3b19c06212dc3914183a2e4
ms.sourcegitcommit: e7fafb153b9de7595c2558a0133f8d1c33a3bddb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52458541"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Autoriser les demandes de Cross-Origin (CORS) dans ASP.NET Core

Par [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), et [Tom Dykstra](https://github.com/tdykstra)

Sécurité des navigateurs empêche une page web d’effectuer des requêtes vers un autre domaine que celui qui a servi la page web. Cette restriction est appelée le *stratégie de même origine*. La stratégie de même origine empêche un site malveillant de lire des données sensibles à partir d’un autre site. Parfois, vous souhaiterez autoriser de qu'autres sites apporter les demandes cross-origin à votre application.

[Cross-origine partage de ressources](https://www.w3.org/TR/cors/) (CORS) est une norme W3C qui permet à un serveur d’abaisser la stratégie de même origine. À l’aide de CORS, un serveur peut autoriser explicitement certaines demandes cross-origin lors du refus d’autres. CORS est plus sûre et plus flexible que les techniques précédentes, telles que [JSONP](https://wikipedia.org/wiki/JSONP). Cette rubrique montre comment activer CORS dans une application ASP.NET Core.

## <a name="same-origin"></a>Même origine

Deux URL ont la même origine que s’ils ont des schémas identiques, les hôtes et les ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).

Ces deux URL ayant la même origine :

* `https://example.com/foo.html`
* `https://example.com/bar.html`

Ces URL ont différentes origines que les deux URL précédentes :

* `https://example.net` &ndash; Autre domaine
* `https://www.example.com/foo.html` &ndash; Autre sous-domaine
* `http://example.com/foo.html` &ndash; Schéma différent
* `https://example.com:9000/foo.html` &ndash; Port différent

> [!NOTE]
> Internet Explorer ne considère pas le port lors de la comparaison des origines.

## <a name="register-cors-services"></a>Inscrire les services CORS

::: moniker range=">= aspnetcore-2.1"

Référence le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app) ou ajouter une référence de package à la [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Référence le [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ou ajouter une référence de package à la [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Ajouter une référence de package à la [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.

::: moniker-end

Appelez <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> dans `Startup.ConfigureServices` pour ajouter des services CORS au conteneur de service de l’application :

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a>Activer CORS

Après avoir inscrit des services CORS, utilisez une des approches suivantes pour activer CORS dans une application ASP.NET Core :

* [Intergiciel (middleware) CORS](#enable-cors-with-cors-middleware) &ndash; stratégies CORS s’appliquent globalement à l’application via l’intergiciel (middleware).
* [CORS dans MVC](#enable-cors-in-mvc) &ndash; stratégies CORS s’appliquent par action ou par le contrôleur. Intergiciel (middleware) CORS n’est pas utilisé.

### <a name="enable-cors-with-cors-middleware"></a>Activer CORS avec un intergiciel (middleware) CORS

Intergiciel (middleware) CORS gère les demandes cross-origin à l’application. Pour activer l’intergiciel (middleware) CORS dans le pipeline de traitement de la demande, appelez le <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> méthode d’extension dans `Startup.Configure`.

Intergiciel (middleware) CORS doivent précéder n’importe quel point de terminaison défini dans votre application où vous souhaitez prendre en charge les demandes cross-origin (par exemple, avant l’appel à `UseMvc` intergiciel (middleware) de MVC/Razor Pages).

Un *stratégie multi-origines* peut être spécifié lors de l’ajout de l’intergiciel (middleware) CORS à l’aide de la <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> classe. Il existe deux approches permettant de définir une stratégie CORS :

* Appelez `UseCors` avec une expression lambda :

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  L’expression lambda prend un objet <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>. [Options de configuration](#cors-policy-options), tel que `WithOrigins`, sont décrits plus loin dans cette rubrique. Dans l’exemple précédent, la stratégie autorise les demandes cross-origin à partir de `https://example.com` et aucune autre origine.

  L’URL doit être spécifié sans barre oblique de fin (`/`). Si l’URL se termine avec `/`, la comparaison retourne `false` et aucun en-tête n’est retourné.

  `CorsPolicyBuilder` possède une API fluent, vous pouvez chaîner des appels de méthode :

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* Définir une ou plusieurs stratégies CORS nommées et sélectionnez la stratégie par nom lors de l’exécution. L’exemple suivant ajoute une stratégie CORS défini par l’utilisateur nommée *AllowSpecificOrigin*. Pour sélectionner la stratégie, passez le nom à `UseCors`:

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a>Activer CORS dans MVC

Vous pouvez également utiliser MVC pour appliquer des stratégies CORS spécifiques par action ou par le contrôleur. Lorsque vous utilisez MVC pour activer CORS, les services CORS inscrits sont utilisés. L’intergiciel (middleware) CORS n’est pas utilisé.

### <a name="per-action"></a>Par action

Pour spécifier une stratégie CORS pour une action spécifique, ajoutez le [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) d’attribut à l’action. Spécifiez le nom de la stratégie.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a>Par contrôleur

Pour spécifier la stratégie CORS pour un contrôleur spécifique, ajoutez le [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) d’attribut à la classe de contrôleur. Spécifiez le nom de la stratégie.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

L’ordre de priorité est :

1. action
1. contrôleur

### <a name="disable-cors"></a>Désactiver CORS

Pour désactiver CORS pour un contrôleur ou d’action, utilisez la [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribut :

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a>Options de stratégie CORS

Cette section décrit les différentes options que vous pouvez définir dans une stratégie CORS. Le <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> méthode est appelée dans `Startup.ConfigureServices`.

* [Définir les origines autorisées](#set-the-allowed-origins)
* [Définir les méthodes HTTP autorisées](#set-the-allowed-http-methods)
* [Définir les en-têtes de requête](#set-the-allowed-request-headers)
* [Définir les en-têtes de réponse exposé](#set-the-exposed-response-headers)
* [Informations d’identification dans les demandes cross-origin](#credentials-in-cross-origin-requests)
* [Définir en amont le délai d’expiration](#set-the-preflight-expiration-time)

Pour certaines options, il peut être utile de lire le [CORS comment fonctionne](#how-cors-works) section tout d’abord.

### <a name="set-the-allowed-origins"></a>Définir les origines autorisées

L’intergiciel (middleware) CORS dans ASP.NET Core MVC présente quelques façons de spécifier les origines autorisées :

* <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*> &ndash; Permet de spécifier une ou plusieurs URL. L’URL peut inclure le schéma, le nom d’hôte et le port sans informations de chemin d’accès. Par exemple, `https://example.com`. L’URL doit être spécifié sans barre oblique de fin (`/`).

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-25&highlight=4-5)]

* <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Autorise les demandes CORS à partir de toutes les origines avec n’importe quel schéma (`http` ou `https`).

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=29-33&highlight=4)]

  Réfléchissez bien avant d’autoriser des demandes à partir de n’importe quelle origine. Autoriser les demandes à partir de toute origine signifie que *n’importe quel site Web* peut apporter les demandes cross-origin à votre application.

  ::: moniker range=">= aspnetcore-2.2"

  > [!NOTE]
  > Spécification `AllowAnyOrigin` et `AllowCredentials` est une configuration non sécurisée et peut entraîner la falsification de requête intersites. Le service CORS retourne une réponse non valide de CORS lorsqu’une application est configurée avec les deux méthodes.

  ::: moniker-end

  ::: moniker range="< aspnetcore-2.2"

  > [!NOTE]
  > Spécification `AllowAnyOrigin` et `AllowCredentials` est une configuration non sécurisée et peut entraîner la falsification de requête intersites. Envisagez de spécifier une liste exacte des origines, si le client doit autoriser lui-même pour accéder aux ressources du serveur.

  ::: moniker-end

  Ce paramètre affecte les demandes préliminaires et `Access-Control-Allow-Origin` en-tête. Pour plus d’informations, consultez le [demandes de contrôle en amont](#preflight-requests) section.

::: moniker range=">= aspnetcore-2.0"

* <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Définit le <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> propriété de la stratégie comme étant une fonction qui autorise les origines correspondre à un domaine configuré avec des caractères génériques lors de l’évaluation si l’origine est autorisée.

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a>Définir les méthodes HTTP autorisées

Pour autoriser toutes les méthodes HTTP, appeler <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=46-51&highlight=5)]

Ce paramètre affecte les demandes préliminaires et `Access-Control-Allow-Methods` en-tête. Pour plus d’informations, consultez le [demandes de contrôle en amont](#preflight-requests) section.

### <a name="set-the-allowed-request-headers"></a>Définir les en-têtes de requête

Pour autoriser des en-têtes spécifiques à envoyer dans une demande CORS, appelé *créer des en-têtes de demande*, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> et spécifiez les en-têtes autorisés :

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Pour autoriser tous les créer des en-têtes de demande, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Ce paramètre affecte les demandes préliminaires et `Access-Control-Request-Headers` en-tête. Pour plus d’informations, consultez le [demandes de contrôle en amont](#preflight-requests) section.

::: moniker range=">= aspnetcore-2.2"

Une recherche de correspondance de stratégie intergiciel (middleware) CORS spécifiée par des en-têtes spécifiques `WithHeaders` n’est possible que lorsque les en-têtes envoyés `Access-Control-Request-Headers` correspondre exactement les en-têtes indiqués dans `WithHeaders`.

Par exemple, considérez une application configurée comme suit :

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

Intergiciel (middleware) CORS refuse une demande préliminaire avec l’en-tête de demande suivant, car `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) n’est pas répertoriée dans `WithHeaders`:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

L’application retourne un *200 OK* réponse mais n’envoie pas les en-têtes CORS précédent. Par conséquent, le navigateur ne tente pas la demande cross-origin.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Intergiciel (middleware) CORS permet toujours quatre en-têtes dans le `Access-Control-Request-Headers` à envoyer, indépendamment des valeurs configurées dans CorsPolicy.Headers. Cette liste d’en-têtes comprend :

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

Par exemple, considérez une application configurée comme suit :

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

Intergiciel (middleware) CORS répond correctement à une demande préliminaire avec l’en-tête de demande suivant, car `Content-Language` est toujours dans la liste verte :

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a>Définir les en-têtes de réponse exposé

Par défaut, le navigateur n’expose pas tous les en-têtes de réponse à l’application. Pour plus d’informations, consultez [W3C Cross-Origin Resource Sharing (terminologie) : en-tête de réponse Simple](https://www.w3.org/TR/cors/#simple-response-header).

Les en-têtes de réponse sont disponibles par défaut sont :

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

La spécification CORS appelle ces en-têtes *les en-têtes de réponse simple*. Pour les autres en-têtes disponibles pour l’application, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>Informations d’identification dans les demandes cross-origin

Les informations d’identification nécessitent un traitement particulier dans une demande CORS. Par défaut, le navigateur n’envoie pas les informations d’identification avec une demande de cross-origin. Informations d’identification incluent les cookies et les schémas d’authentification HTTP. Pour envoyer des informations d’identification avec une demande de cross-origin, le client doit définir `XMLHttpRequest.withCredentials` à `true`.

À l’aide de `XMLHttpRequest` directement :

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

Dans jQuery :

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

En outre, le serveur doit autoriser les informations d’identification. Pour autoriser les informations d’identification de cross-origin, appeler <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

La réponse HTTP inclut un `Access-Control-Allow-Credentials` en-tête, qui indique au navigateur que le serveur autorise les informations d’identification pour une demande de cross-origin.

Si le navigateur envoie les informations d’identification, mais la réponse n’inclut pas une valide `Access-Control-Allow-Credentials` en-tête, le navigateur n’expose pas la réponse à l’application, et la demande cross-origin échoue.

Soyez prudent lorsque vous autorisez les informations d’identification cross-origin. Un site Web à un autre domaine peut envoyer des informations d’identification d’un utilisateur de connecté à l’application sur l’utilisateur sans avoir connaissance de l’utilisateur.

La spécification CORS indique également ce paramètre pour les origines `"*"` (toutes les origines) n’est pas valide si le `Access-Control-Allow-Credentials` en-tête est présent.

### <a name="preflight-requests"></a>Demandes préliminaires

Pour certaines requêtes CORS, le navigateur envoie une demande supplémentaire avant d’effectuer la demande réelle. Cette requête est appelée un *demande préliminaire*. Le navigateur peut ignorer la demande préliminaire si les conditions suivantes sont remplies :

* La méthode de demande est GET, HEAD ou POST.
* L’application ne définit pas les en-têtes de demande autre que `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, ou `Last-Event-ID`.
* Le `Content-Type` en-tête, si définis, dispose d’une des valeurs suivantes :
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

La règle sur les en-têtes de requête définie pour la demande du client s’applique aux en-têtes de l’application définit en appelant `setRequestHeader` sur la `XMLHttpRequest` objet. La spécification CORS appelle ces en-têtes *créer des en-têtes de demande*. La règle ne s’applique pas aux en-têtes le navigateur peut définir comme `User-Agent`, `Host`, ou `Content-Length`.

Voici un exemple d’une requête préliminaire :

```
OPTIONS https://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: https://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

La demande de contrôle préliminaire utilise la méthode HTTP OPTIONS. Il comprend deux en-têtes spéciaux :

* `Access-Control-Request-Method`: La méthode HTTP qui sera utilisée pour la demande réelle.
* `Access-Control-Request-Headers`: Liste des en-têtes de demande que l’application définit sur la demande réelle. Comme indiqué précédemment, cela n’inclut pas les en-têtes qui définit le navigateur, telles que `User-Agent`.

Une demande préliminaire CORS peut inclure un `Access-Control-Request-Headers` en-tête, qui indique au serveur les en-têtes qui sont envoyées avec la demande réelle.

Pour autoriser des en-têtes spécifiques, appeler <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Pour autoriser tous les créer des en-têtes de demande, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Navigateurs ne sont pas entièrement cohérents dans la façon dont elles définies `Access-Control-Request-Headers`. Si vous définissez les en-têtes sur n’importe quelle autre que `"*"` (ou utilisez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), vous devez inclure au moins `Accept`, `Content-Type`, et `Origin`, ainsi que tous les en-têtes personnalisés que vous souhaitez prendre en charge.

Voici un exemple de réponse à la demande préliminaire (en supposant que le serveur autorise la demande) :

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

La réponse inclut un `Access-Control-Allow-Methods` en-tête qui répertorie les méthodes autorisées et éventuellement un `Access-Control-Allow-Headers` en-tête qui répertorie les en-têtes autorisés. Si la demande préliminaire réussit, le navigateur envoie la demande réelle.

Si la demande préliminaire est refusée, l’application retourne un *200 OK* réponse mais n’envoie pas les en-têtes CORS précédent. Par conséquent, le navigateur ne tente pas la demande cross-origin.

### <a name="set-the-preflight-expiration-time"></a>Définir en amont le délai d’expiration

Le `Access-Control-Max-Age` en-tête spécifie la durée pendant laquelle la réponse à la demande préliminaire peut être mis en cache. Pour définir cet en-tête, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

## <a name="how-cors-works"></a>Fonctionnement de CORS

Cette section décrit ce qui se passe dans une demande CORS au niveau des messages HTTP. Il est important de comprendre le fonctionnement de CORS afin que la stratégie CORS peut être configurée correctement et de débogage lorsque des comportements inattendus se produisent.

La spécification CORS introduit plusieurs nouveaux en-têtes HTTP qui permettent les demandes cross-origin. Si un navigateur prend en charge CORS, il définit ces en-têtes automatiquement pour les demandes cross-origin. Du code JavaScript personnalisé n’est pas nécessaire pour activer CORS.

Voici un exemple d’une demande de cross-origin. Le `Origin` en-tête fournit le domaine du site qui effectue la demande :

```
GET https://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: https://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: https://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

Si le serveur autorise la demande, il définit le `Access-Control-Allow-Origin` en-tête dans la réponse. La valeur de cet en-tête correspond soit à la `Origin` en-tête à partir de la demande, soit la valeur de caractère générique `"*"`, ce qui signifie que toute origine est autorisée :

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

Si la réponse n’inclut pas le `Access-Control-Allow-Origin` en-tête, la demande cross-origin échoue. Plus précisément, le navigateur n’autorise pas la demande. Même si le serveur retourne une réponse correcte, le navigateur ne rend pas la réponse disponible dans l’application cliente.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
