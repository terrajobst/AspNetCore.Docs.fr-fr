---
title: Activer les requêtes Cross-Origin (CORS) dans ASP.NET Core
author: rick-anderson
description: Découvrez comment CORS comme norme pour autoriser ou rejeter des demandes Cross-Origin dans une application ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: security/cors
ms.openlocfilehash: a02b3497684979c1a9e792437f9f1a4c467600f0
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187255"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Activer les requêtes Cross-Origin (CORS) dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Cet article explique comment activer CORS dans une application ASP.NET Core.

La sécurité du navigateur empêche une page Web d’effectuer des demandes vers un autre domaine que celui qui a servi la page Web. Cette restriction est appelée *stratégie de même origine*. La stratégie de même origine empêche un site malveillant de lire des données sensibles à partir d’un autre site. Parfois, vous souhaiterez peut-être autoriser d’autres sites à effectuer des demandes Cross-Origin à votre application. Pour plus d’informations, consultez l' [article Mozilla cors](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

[Partage des ressources Cross-Origin](https://www.w3.org/TR/cors/) (CORS) :

* Est une norme W3C qui permet à un serveur d’assouplir la stratégie de même origine.
* N’est **pas** une fonctionnalité de sécurité, cors assouplit la sécurité. Une API n’est pas plus sûre en autorisant CORS. Pour plus d’informations, consultez fonctionnement de [cors](#how-cors).
* Permet à un serveur d’autoriser explicitement certaines demandes Cross-Origin tout en rejetant d’autres.
* Est plus sûr et plus flexible que les techniques antérieures, telles que [JSONP](/dotnet/framework/wcf/samples/jsonp).

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="same-origin"></a>Même origine

Deux URL ont la même origine si elles ont des schémas, des hôtes et des ports identiques ([RFC 6454](https://tools.ietf.org/html/rfc6454)).

Ces deux URL ont le même origine :

* `https://example.com/foo.html`
* `https://example.com/bar.html`

Ces URL ont des origines différentes des deux précédentes URL :

* `https://example.net`&ndash; Domaine différent
* `https://www.example.com/foo.html`&ndash; Autre sous-domaine
* `http://example.com/foo.html`&ndash; Schéma différent
* `https://example.com:9000/foo.html`&ndash; Autre port

Internet Explorer ne prend pas en compte le port lors de la comparaison des origines.

## <a name="cors-with-named-policy-and-middleware"></a>CORS avec la stratégie et l’intergiciel (middleware) nommés

L’intergiciel (middleware) CORS gère les demandes Cross-Origin. Le code suivant active CORS pour l’application entière avec l’origine spécifiée :

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

Le code précédent :

* Définit le nom de la stratégie\_sur « myAllowSpecificOrigins ». Le nom de la stratégie est arbitraire.
* Appelle la <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> méthode d’extension, qui active cors.
* Appelle <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> avec une [expression lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). L’expression lambda prend un objet <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>. Les [options de configuration](#cors-policy-options), `WithOrigins`telles que, sont décrites plus loin dans cet article.

L' <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> appel de méthode ajoute des services cors au conteneur de services de l’application :

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

Pour plus d’informations, consultez les [options de stratégie cors](#cpo) dans ce document.

La <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> méthode peut chaîner des méthodes, comme indiqué dans le code suivant :

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

Remarque : L’URL **ne doit pas** contenir de barre oblique finale`/`(). Si l’URL se termine `/`par, la comparaison `false` retourne et aucun en-tête n’est retourné.

::: moniker range=">= aspnetcore-3.0"

Le code suivant applique des stratégies CORS à tous les points de terminaison des applications par le biais de l’intergiciel (middleware) CORS :
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // Preceding code ommitted.
    app.UseRouting();

    app.UseCors();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });

    // Following code ommited.
}
```

> [!WARNING]
> Avec le routage de point de terminaison, l’intergiciel (middleware) cors doit être configuré `UseRouting` pour `UseEndpoints`s’exécuter entre les appels à et. Une configuration incorrecte entraîne l’arrêt du fonctionnement correct de l’intergiciel.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"
Le code suivant applique des stratégies CORS à tous les points de terminaison des applications par le biais de l’intergiciel (middleware) CORS :
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
Remarque : `UseCors` doit être appelé avant `UseMvc`.

::: moniker-end

Consultez [activer cors dans Razor pages, les contrôleurs et les méthodes d’action](#ecors) pour appliquer la stratégie cors au niveau de la page/du contrôleur/action.

Consultez la page [test cors](#test) pour obtenir des instructions sur le test du code précédent.

<a name="ecors"></a>

::: moniker range=">= aspnetcore-3.0"

## <a name="enable-cors-with-endpoint-routing"></a>Activer cors avec routage du point de terminaison

Avec le routage de point de terminaison, cors peut être activé sur une base par `RequireCors` point de terminaison à l’aide de l’ensemble de méthodes d’extension.

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

De même, CORS peut également être activé pour tous les contrôleurs :

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```
::: moniker-end

## <a name="enable-cors-with-attributes"></a>Activer CORS avec des attributs

L' [ &lbrack;attributEnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) fournit une alternative à l’application de cors globalement. L' `[EnableCors]` attribut Active cors pour les points de terminaison sélectionnés, plutôt que tous les points de terminaison.

Utilisez `[EnableCors]` pour spécifier la stratégie par défaut `[EnableCors("{Policy String}")]` et spécifier une stratégie.

L' `[EnableCors]` attribut peut être appliqué aux éléments suivants :

* Page Razor`PageModel`
* Contrôleur
* Méthode d’action du contrôleur

Vous pouvez appliquer différentes stratégies à Controller/page-Model/action avec l' `[EnableCors]` attribut. Lorsque l' `[EnableCors]` attribut est appliqué à une méthode Controllers/Model-Model/action et que cors est activé dans l’intergiciel (middleware), les deux stratégies sont appliquées. Nous vous recommandons de combiner les stratégies. Utilisez l' `[EnableCors]` attribut ou l’intergiciel (middleware), pas les deux dans la même application.

Le code suivant applique une stratégie différente à chaque méthode :

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

Le code suivant crée une stratégie CORS par défaut et une stratégie `"AnotherPolicy"`nommée :

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a>Désactiver CORS

[ L'&lbrack;attributDisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) désactive cors pour le contrôleur/page-Model/action.

<a name="cpo"></a>

## <a name="cors-policy-options"></a>Options de stratégie CORS

Cette section décrit les différentes options qui peuvent être définies dans une stratégie CORS :

* [Définir les origines autorisées](#set-the-allowed-origins)
* [Définir les méthodes HTTP autorisées](#set-the-allowed-http-methods)
* [Définir les en-têtes de demande autorisés](#set-the-allowed-request-headers)
* [Définir les en-têtes de réponse exposés](#set-the-exposed-response-headers)
* [Informations d’identification dans les demandes Cross-Origin](#credentials-in-cross-origin-requests)
* [Définir en amont le délai d’expiration](#set-the-preflight-expiration-time)

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>est appelé dans `Startup.ConfigureServices`. Pour certaines options, il peut être utile de lire d’abord la section [How cors Works](#how-cors) .

## <a name="set-the-allowed-origins"></a>Définir les origines autorisées

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>Autorise les requêtes cors de toutes les origines avec n'`http` importe `https`quel schéma (ou). &ndash; `AllowAnyOrigin`n’est pas sécurisé, car *un site Web* peut effectuer des demandes Cross-Origin à l’application.

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> La `AllowAnyOrigin` spécification `AllowCredentials` de et de est une configuration non sécurisée et peut entraîner une falsification de requête intersites. Le service CORS retourne une réponse CORS non valide lorsqu’une application est configurée avec les deux méthodes.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> La `AllowAnyOrigin` spécification `AllowCredentials` de et de est une configuration non sécurisée et peut entraîner une falsification de requête intersites. Pour une application sécurisée, spécifiez une liste exacte d’origines si le client doit s’autoriser à accéder aux ressources du serveur.

::: moniker-end

`AllowAnyOrigin`affecte les demandes préliminaires et `Access-Control-Allow-Origin` l’en-tête. Pour plus d’informations, consultez la section [demandes préliminaires](#preflight-requests) .

::: moniker range=">= aspnetcore-2.0"

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>&ndash; Définit la<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> propriété de la stratégie pour qu’elle soit une fonction qui permet aux origines de correspondre à un domaine générique configuré lors de l’évaluation si l’origine est autorisée.

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a>Définir les méthodes HTTP autorisées

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

* Autorise toute méthode HTTP :
* Affecte les demandes préliminaires et `Access-Control-Allow-Methods` l’en-tête. Pour plus d’informations, consultez la section [demandes préliminaires](#preflight-requests) .

### <a name="set-the-allowed-request-headers"></a>Définir les en-têtes de demande autorisés

Pour autoriser l’envoi d’en-têtes spécifiques dans une demande cors, appelée *créer des en-têtes de demande*, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> et spécifiez les en-têtes autorisés :

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Pour autoriser tous les en-têtes de demande <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>d’auteur, appelez :

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Ce paramètre affecte les demandes préliminaires et `Access-Control-Request-Headers` l’en-tête. Pour plus d’informations, consultez la section [demandes préliminaires](#preflight-requests) .

::: moniker range=">= aspnetcore-2.2"

Une stratégie d’intergiciel (middleware) cors correspondant à des en `WithHeaders` -têtes spécifiques spécifiés par n’est possible `Access-Control-Request-Headers` que lorsque les en-têtes envoyés `WithHeaders`dans correspondent exactement aux en-têtes indiqués dans.

Par exemple, considérez une application configurée comme suit :

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

L’intergiciel (middleware) cors refuse une demande préliminaire avec l’en-tête `Content-Language` de demande suivant, car ([HeaderNames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) n’est pas listé dans `WithHeaders`:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

L’application renvoie une réponse *200 OK* , mais n’envoie pas les en-têtes cors. Par conséquent, le navigateur n’essaie pas la demande Cross-Origin.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

L’intergiciel (middleware) cors autorise toujours l’envoi `Access-Control-Request-Headers` de quatre en-têtes dans le, quelles que soient les valeurs configurées dans les en-têtes CorsPolicy. Cette liste d’en-têtes comprend les éléments suivants :

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

Par exemple, considérez une application configurée comme suit :

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

L’intergiciel (middleware) cors répond correctement à une demande préliminaire avec l’en- `Content-Language` tête de demande suivant, car est toujours ajouté à la liste blanche :

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a>Définir les en-têtes de réponse exposés

Par défaut, le navigateur n’expose pas tous les en-têtes de réponse à l’application. Pour plus d’informations, [consultez partage des ressources Cross-Origin W3C (terminologie) : En-tête](https://www.w3.org/TR/cors/#simple-response-header)de réponse simple.

Les en-têtes de réponse qui sont disponibles par défaut sont :

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

La spécification CORS appelle ces en *-têtes de réponse simples*en-têtes. Pour mettre d’autres en-têtes à la disposition de <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>l’application, appelez :

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>Informations d’identification dans les demandes Cross-Origin

Les informations d’identification nécessitent un traitement particulier dans une demande CORS. Par défaut, le navigateur n’envoie pas d’informations d’identification avec une demande Cross-Origin. Les informations d’identification incluent les cookies et les schémas d’authentification HTTP. Pour envoyer des informations d’identification avec une demande Cross-Origin, le client `XMLHttpRequest.withCredentials` doit `true`affecter à la valeur.

Utilisation `XMLHttpRequest` directe :

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

Utilisation de jQuery :

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

Utilisation de l' [API FETCH](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

Le serveur doit autoriser les informations d’identification. Pour autoriser les informations d’identification Cross-Origin <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>, appelez :

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

La réponse http comprend un `Access-Control-Allow-Credentials` en-tête qui indique au navigateur que le serveur autorise les informations d’identification pour une demande Cross-Origin.

Si le navigateur envoie des informations d’identification mais que la réponse n' `Access-Control-Allow-Credentials` inclut pas d’en-tête valide, le navigateur n’expose pas la réponse à l’application, et la demande Cross-Origin échoue.

L’autorisation des informations d’identification Cross-Origin est un risque pour la sécurité. Un site Web dans un autre domaine peut envoyer les informations d’identification d’un utilisateur connecté à l’application pour le compte de l’utilisateur sans la connaissance de l’utilisateur. <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

La spécification cors indique également que le paramètre Origins to `"*"` (All Origins) n’est pas valide si l' `Access-Control-Allow-Credentials` en-tête est présent.

### <a name="preflight-requests"></a>Demandes préliminaires

Pour certaines demandes CORS, le navigateur envoie une demande supplémentaire avant d’effectuer la demande réelle. Cette demande porte le nom de *demande préliminaire*. Le navigateur peut ignorer la demande préliminaire si les conditions suivantes sont remplies :

* La méthode de demande est : obtenir, début ou publication.
* L’application ne définit pas les en-têtes `Accept`de `Accept-Language`requête autres `Content-Type`que, `Last-Event-ID`, `Content-Language`, ou.
* L' `Content-Type` en-tête, s’il est défini, a l’une des valeurs suivantes :
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

La règle sur les en-têtes de demande définie pour la demande du client s’applique aux en-têtes `setRequestHeader` définis par `XMLHttpRequest` l’application en appelant sur l’objet. La spécification CORS appelle ces en-têtes *créer des en-* têtes de demande. La règle ne s’applique pas aux en-têtes que le navigateur peut `User-Agent`définir `Host`, tels `Content-Length`que, ou.

Voici un exemple de demande préliminaire :

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

La requête de pré-vol utilise la méthode HTTP OPTIONS. Il comprend deux en-têtes spéciaux :

* `Access-Control-Request-Method`: Méthode HTTP qui sera utilisée pour la demande réelle.
* `Access-Control-Request-Headers`: Liste des en-têtes de requête que l’application définit sur la demande réelle. Comme indiqué précédemment, cela n’inclut pas les en-têtes définis par le navigateur `User-Agent`, tels que.

Une demande préliminaire cors peut inclure un `Access-Control-Request-Headers` en-tête, qui indique au serveur les en-têtes envoyés avec la demande réelle.

Pour autoriser des en-têtes spécifiques <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>, appelez :

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Pour autoriser tous les en-têtes de demande <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>d’auteur, appelez :

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Les navigateurs ne sont pas entièrement cohérents `Access-Control-Request-Headers`dans la façon dont ils sont définis. Si vous définissez des en-têtes autres que `"*"` (ou utilisez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), vous devez inclure au moins `Accept`, `Content-Type`, et `Origin`, ainsi que tous les en-têtes personnalisés que vous souhaitez prendre en charge.

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

La réponse comprend un `Access-Control-Allow-Methods` en-tête qui répertorie les méthodes autorisées et `Access-Control-Allow-Headers` éventuellement un en-tête, qui répertorie les en-têtes autorisés. Si la demande préliminaire est réussie, le navigateur envoie la demande réelle.

Si la demande préliminaire est refusée, l’application renvoie une réponse *200 OK* , mais n’envoie pas les en-têtes cors. Par conséquent, le navigateur n’essaie pas la demande Cross-Origin.

### <a name="set-the-preflight-expiration-time"></a>Définir en amont le délai d’expiration

L' `Access-Control-Max-Age` en-tête spécifie la durée pendant laquelle la réponse à la demande préliminaire peut être mise en cache. Pour définir cet en-tête <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>, appelez :

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a>Fonctionnement de CORS

Cette section décrit ce qui se produit dans une demande [cors](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) au niveau des messages http.

* CORS n’est **pas** une fonctionnalité de sécurité. CORS est une norme W3C qui permet à un serveur d’assouplir la stratégie de même origine.
  * Par exemple, un acteur malveillant peut utiliser l’option [empêcher les scripts inter-sites (XSS)](xref:security/cross-site-scripting) sur votre site et exécuter une requête intersite vers son site Active cors pour voler des informations.
* Votre API n’est pas plus sûre en autorisant CORS.
  * C’est au client (navigateur) d’appliquer CORS. Le serveur exécute la requête et retourne la réponse, c’est le client qui retourne une erreur et bloque la réponse. Par exemple, l’un des outils suivants affiche la réponse du serveur :
    * [Fiddler](https://www.telerik.com/fiddler)
    * [Postman](https://www.getpostman.com/)
    * [HttpClient .NET](/dotnet/csharp/tutorials/console-webapiclient)
    * Un navigateur Web en entrant l’URL dans la barre d’adresses.
* C’est un moyen pour un serveur d’autoriser les navigateurs à exécuter une requête d’API [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) ou [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) Cross-Origin qui serait sinon interdite.
  * Les navigateurs (sans CORS) ne peuvent pas effectuer de demandes Cross-Origin. Avant CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) était utilisé pour contourner cette restriction. JSONP n’utilise pas XHR, elle utilise `<script>` la balise pour recevoir la réponse. Les scripts sont autorisés à être chargés sur plusieurs origines.

La [spécification cors](https://www.w3.org/TR/cors/) a introduit plusieurs nouveaux en-têtes HTTP qui permettent des demandes Cross-Origin. Si un navigateur prend en charge CORS, il définit ces en-têtes automatiquement pour les demandes cross-origin. Du code JavaScript personnalisé n’est pas nécessaire pour activer CORS.

Voici un exemple de demande Cross-Origin. L' `Origin` en-tête fournit le domaine du site qui effectue la requête :

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

Si le serveur autorise la demande, il définit l' `Access-Control-Allow-Origin` en-tête dans la réponse. La valeur de cet en-tête correspond `Origin` à l’en-tête de la demande ou `"*"`à la valeur du caractère générique, ce qui signifie que toute origine est autorisée :

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

Si la réponse n’inclut pas `Access-Control-Allow-Origin` l’en-tête, la demande Cross-Origin échoue. Plus précisément, le navigateur n’autorise pas la demande. Même si le serveur retourne une réponse correcte, le navigateur ne rend pas la réponse disponible pour l’application cliente.

<a name="test"></a>

## <a name="test-cors"></a>Test CORS

Pour tester CORS :

1. [Créez un projet d’API](xref:tutorials/first-web-api). Vous pouvez également [Télécharger l’exemple](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).
1. Activez CORS à l’aide de l’une des approches décrites dans ce document. Par exemple :

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");`doit uniquement être utilisé pour tester un exemple d’application semblable à l' [exemple de code de téléchargement](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).

1. Créez un projet d’application Web (Razor Pages ou MVC). L’exemple utilise Razor Pages. Vous pouvez créer l’application Web dans la même solution que le projet d’API.
1. Ajoutez le code en surbrillance suivant au fichier *index. cshtml* :

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. Dans le code précédent, remplacez `url: 'https://<web app>.azurewebsites.net/api/values/1',` par l’URL de l’application déployée.
1. Déployez le projet API. Par exemple, [déployez sur Azure](xref:host-and-deploy/azure-apps/index).
1. Exécutez l’Razor Pages ou l’application MVC à partir du bureau, puis cliquez sur le bouton **test** . Utilisez les outils F12 pour passer en revue les messages d’erreur.
1. Supprimez l’origine localhost `WithOrigins` de et déployez l’application. Vous pouvez également exécuter l’application cliente avec un autre port. Par exemple, exécutez à partir de Visual Studio.
1. Testez avec l’application cliente. Les échecs CORS retournent une erreur, mais le message d’erreur n’est pas disponible pour JavaScript. Utilisez l’onglet Console des outils F12 pour afficher l’erreur. En fonction du navigateur, vous recevez une erreur (dans la console outils F12) semblable à ce qui suit :

   * Utilisation de Microsoft Edge :

     **SEC7120 : [cors] l’origine `https://localhost:44375` n’a pas `https://localhost:44375` été trouvée dans l’en-tête de réponse Access-Control-allow-Origin pour la ressource Cross-Origin à l’adresse`https://webapi.azurewebsites.net/api/values/1`**

   * Utilisation de chrome :

     **L’accès à XMLHttpRequest `https://webapi.azurewebsites.net/api/values/1` à partir `https://localhost:44375` de l’origine a été bloqué par la stratégie cors : Aucun en-tête « Access-Control-allow-Origin » n’est présent sur la ressource demandée.**

## <a name="additional-resources"></a>Ressources supplémentaires

* [Partage des ressources Cross-Origin (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
