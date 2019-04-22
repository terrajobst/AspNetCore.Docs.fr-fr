---
title: Autoriser les demandes de Cross-Origin (CORS) dans ASP.NET Core
author: rick-anderson
description: Découvrez comment CORS comme une norme pour autoriser ou rejeter les demandes cross-origin dans une application ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: security/cors
ms.openlocfilehash: fe5b750c44e5fad9ba80efb2cc8116d0a64b1a17
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59068295"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Autoriser les demandes de Cross-Origin (CORS) dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Cet article explique comment activer CORS dans une application ASP.NET Core.

Sécurité des navigateurs empêche une page web d’effectuer des requêtes vers un autre domaine que celui qui a servi la page web. Cette restriction est appelée le *stratégie de même origine*. La stratégie de même origine empêche un site malveillant de lire des données sensibles à partir d’un autre site. Parfois, vous souhaiterez autoriser de qu'autres sites apporter les demandes cross-origin à votre application. Pour plus d’informations, consultez le [article de Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

[Cross-Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) :

* Est un W3C standard qui permet à un serveur d’assouplir la stratégie de même origine.
* Est **pas** une fonctionnalité de sécurité, CORS assouplit la sécurité. Une API n’est pas plus sûre en autorisant CORS. Pour plus d’informations, consultez [CORS comment fonctionne](#how-cors).
* Permet à un serveur autoriser explicitement certaines demandes cross-origin tout en refusant d’autres.
* Est plus sûre et plus flexible que les techniques précédentes, telles que [JSONP](/dotnet/framework/wcf/samples/jsonp).

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/2.2-stage-samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

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

Internet Explorer ne considère pas le port lors de la comparaison des origines.

## <a name="cors-with-named-policy-and-middleware"></a>CORS avec la stratégie nommée et intergiciel (middleware)

Intergiciel (middleware) CORS gère les demandes cross-origin. Le code suivant active CORS pour toute l’application avec l’origine spécifiée :

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

Le code précédent :

* Définit le nom de la stratégie «\_myAllowSpecificOrigins ». Le nom de la stratégie est arbitraire.
* Appelle le <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> méthode d’extension, ce qui permet de CORS.
* Appels <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> avec un [expression lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). L’expression lambda prend un objet <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>. [Options de configuration](#cors-policy-options), tel que `WithOrigins`, sont décrits plus loin dans cet article.

Le <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> appel de méthode ajoute des services CORS au conteneur de service de l’application :

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

Pour plus d’informations, consultez [les options de stratégie CORS](#cpo) dans ce document.

Le <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> méthode pouvez chaîner des méthodes, comme indiqué dans le code suivant :

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

Le code en surbrillance suivant applique les stratégies CORS pour toutes les applications points de terminaison via l’intergiciel (middleware) CORS :

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseHsts();
    }

    app.UseCors();

    app.UseHttpsRedirection();
    app.UseMvc();
}
```

Consultez [activer de CORS dans les Pages Razor, contrôleurs et méthodes d’action](#ecors) pour appliquer la stratégie CORS au niveau de la page / / action de contrôleur.

Remarque :

* `UseCors` doit être appelé avant `UseMvc`.
* L’URL doit **pas** contiennent une barre oblique (`/`). Si l’URL se termine avec `/`, la comparaison retourne `false` et aucun en-tête n’est retourné.

Consultez [Test CORS](#test) pour obtenir des instructions sur le test du code précédent.

<a name="ecors"></a>

## <a name="enable-cors-with-attributes"></a>Activer CORS avec attributs

Le [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribut fournit une alternative à l’application de CORS dans le monde entier. Le `[EnableCors]` attribut Active CORS pour les points de terminaison sélectionnés, plutôt que tous les points de terminaison.

Utilisez `[EnableCors]` pour spécifier la stratégie par défaut et `[EnableCors("{Policy String}")]` pour spécifier une stratégie.

Le `[EnableCors]` attribut peut être appliqué aux :

* Page Razor `PageModel`
* Contrôleur
* Méthode d’action de contrôleur

Vous pouvez appliquer des stratégies différentes à contrôleur/page-modèle/une action avec la `[EnableCors]` attribut. Lorsque le `[EnableCors]` attribut est appliqué à une méthode d’action/modèle-page/contrôleurs et CORS est activé dans l’intergiciel (middleware), les deux stratégies sont appliquées. Nous déconseillons de combinaison de stratégies. Utilisez le `[EnableCors]` attribut ou un intergiciel (middleware), pas à la fois dans la même application.

Le code suivant applique une stratégie différente à chaque méthode :

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

Le code suivant crée une stratégie par défaut CORS et une stratégie nommée `"AnotherPolicy"`:

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a>Désactiver CORS

Le [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribut désactive CORS pour la contrôleur/page-modèle/une action.

<a name="cpo"></a>

## <a name="cors-policy-options"></a>Options de stratégie CORS

Cette section décrit les différentes options qui peuvent être définies dans une stratégie CORS :

* [Définir les origines autorisées](#set-the-allowed-origins)
* [Définir les méthodes HTTP autorisées](#set-the-allowed-http-methods)
* [Définir les en-têtes de requête](#set-the-allowed-request-headers)
* [Définir les en-têtes de réponse exposé](#set-the-exposed-response-headers)
* [Informations d’identification dans les demandes cross-origin](#credentials-in-cross-origin-requests)
* [Définir en amont le délai d’expiration](#set-the-preflight-expiration-time)

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> est appelé dans `Startup.ConfigureServices`. Pour certaines options, il peut être utile de lire le [CORS comment fonctionne](#how-cors) section tout d’abord.

## <a name="set-the-allowed-origins"></a>Définir les origines autorisées

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Autorise les demandes CORS à partir de toutes les origines avec n’importe quel schéma (`http` ou `https`). `AllowAnyOrigin` n’est pas sécurisé, car *n’importe quel site Web* peut apporter les demandes cross-origin à l’application.

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> Spécification `AllowAnyOrigin` et `AllowCredentials` est une configuration non sécurisée et peut entraîner la falsification de requête intersites. Le service CORS retourne une réponse non valide de CORS lorsqu’une application est configurée avec les deux méthodes.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> Spécification `AllowAnyOrigin` et `AllowCredentials` est une configuration non sécurisée et peut entraîner la falsification de requête intersites. Pour une application sécurisée, spécifiez une liste exacte des origines si le client doit autoriser lui-même pour accéder aux ressources du serveur.

::: moniker-end

<!-- REVIEW required
I changed from
Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration. **This** setting affects preflight requests and the ...
to
**`AllowAnyOrigin`** affects preflight requests and the

to remove the ambiguous **This**.
-->

`AllowAnyOrigin` affecte des demandes de contrôle en amont et le `Access-Control-Allow-Origin` en-tête. Pour plus d’informations, consultez le [demandes de contrôle en amont](#preflight-requests) section.

::: moniker range=">= aspnetcore-2.0"

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Définit le <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> propriété de la stratégie comme étant une fonction qui autorise les origines correspondre à un domaine générique configuré lors de l’évaluation si l’origine est autorisée.

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a>Définir les méthodes HTTP autorisées

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

* Permet de n’importe quelle méthode HTTP :
* Affecte des demandes de contrôle en amont et le `Access-Control-Allow-Methods` en-tête. Pour plus d’informations, consultez le [demandes de contrôle en amont](#preflight-requests) section.

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

Par défaut, le navigateur n’expose pas tous les en-têtes de réponse à l’application. Pour plus d’informations, consultez [W3C Cross-Origin Resource Sharing (terminologie) : En-tête de réponse simple](https://www.w3.org/TR/cors/#simple-response-header).

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

À l’aide de jQuery :

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

À l’aide de la [obtenir l’API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

Le serveur doit autoriser les informations d’identification. Pour autoriser les informations d’identification de cross-origin, appeler <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

La réponse HTTP inclut un `Access-Control-Allow-Credentials` en-tête, qui indique au navigateur que le serveur autorise les informations d’identification pour une demande de cross-origin.

Si le navigateur envoie les informations d’identification, mais la réponse n’inclut pas une valide `Access-Control-Allow-Credentials` en-tête, le navigateur n’expose pas la réponse à l’application, et la demande cross-origin échoue.

Autoriser les informations d’identification de cross-origin est un risque de sécurité. Un site Web à un autre domaine peut envoyer des informations d’identification d’un utilisateur de connecté à l’application sur l’utilisateur sans avoir connaissance de l’utilisateur. <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

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

<a name="how-cors"></a>

## <a name="how-cors-works"></a>Fonctionnement de CORS

Cette section décrit ce qui se passe dans un [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) demande au niveau des messages HTTP.

* CORS est **pas** une fonctionnalité de sécurité. CORS est une norme W3C qui permet à un serveur d’assouplir la stratégie de même origine.
  * Par exemple, un acteur malveillant pourrait utiliser [empêcher Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) par rapport à votre site avant d’exécuter une requête intersites à leur site CORS est activé pour dérober des informations.
* Votre API n’est pas plus sûre en autorisant CORS.
  * C’est à appliquer CORS pour le client (navigateur). Le serveur exécute la requête et renvoie la réponse, c’est le client qui renvoie une erreur et les blocs de la réponse. Par exemple, un des outils suivants affiche la réponse du serveur :
    * [Fiddler](https://www.telerik.com/fiddler)
    * [Postman](https://www.getpostman.com/)
    * [.NET HttpClient](/dotnet/csharp/tutorials/console-webapiclient)
    * Un navigateur web en entrant l’URL dans la barre d’adresses.
* C’est un moyen pour un serveur pour permettre des navigateurs pour exécuter une cross-origine [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) ou [API Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) demande sinon est interdite.
  * Navigateurs (sans CORS) ne peut pas faire des demandes cross-origin. Avant de CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) a été utilisé pour contourner cette restriction. JSONP n’utilise pas XHR, il utilise le `<script>` balise pour recevoir la réponse. Les scripts sont autorisés à être chargé cross-origin.

Le [spécification CORS](https://www.w3.org/TR/cors/) introduit plusieurs nouveaux en-têtes HTTP qui permettent les requêtes cross-origin. Si un navigateur prend en charge CORS, il définit ces en-têtes automatiquement pour les demandes cross-origin. Du code JavaScript personnalisé n’est pas nécessaire pour activer CORS.

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

<a name="test"></a>

## <a name="test-cors"></a>Testez CORS

Pour tester CORS :

1. [Créer un projet d’API](xref:tutorials/first-web-api). Vous pouvez également [télécharger l’exemple](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cors/sample/Cors).
1. Activer CORS à l’aide d’une des approches dans ce document. Exemple :

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");` doit uniquement être utilisé pour tester un exemple d’application similaire à la [télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/cors/sample/Cors).

1. Créer un projet d’application web (Pages Razor ou MVC). L’exemple utilise les Pages Razor. Vous pouvez créer l’application web dans la même solution que le projet d’API.
1. Ajoutez le code en surbrillance suivant à la *Index.cshtml* fichier :

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. Dans le code précédent, remplacez `url: 'https://<web app>.azurewebsites.net/api/values/1',` par l’URL de l’application déployée.
1. Déployer le projet d’API. Par exemple, [déployer vers Azure](xref:host-and-deploy/azure-apps/index).
1. Exécutez l’application Pages Razor ou MVC à partir du bureau et cliquez sur le **Test** bouton. Utiliser les outils F12 pour passer en revue les messages d’erreur.
1. Supprimer l’origine de localhost à partir de `WithOrigins` et déployer l’application. Vous pouvez également exécuter l’application cliente avec un autre port. Par exemple, exécutez à partir de Visual Studio.
1. Tester avec l’application cliente. Défaillances CORS renvoient une erreur, mais le message d’erreur n’est pas disponible à JavaScript. Utilisez l’onglet de la console dans les outils F12 pour afficher l’erreur. Selon le navigateur, vous obtenez une erreur (dans la console Outils F12) similaire à ce qui suit :

   * À l’aide de Microsoft Edge :

     **SEC7120 : [CORS] l’origine `https://localhost:44375` n’a pas trouvé `https://localhost:44375` dans l’en-tête de réponse Access-Control-Allow-Origin pour des ressources cross-origin à `https://webapi.azurewebsites.net/api/values/1`**

   * À l’aide de Chrome :

     **Accès à XMLHttpRequest au `https://webapi.azurewebsites.net/api/values/1` à partir de l’origine `https://localhost:44375` a été bloqué par la stratégie CORS : Aucun en-tête 'Access-Control-Allow-Origin' n’est présent sur la ressource demandée.**

## <a name="additional-resources"></a>Ressources supplémentaires

* [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
