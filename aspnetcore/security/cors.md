---
title: Activer les requêtes Cross-Origin (CORS) dans ASP.NET Core
author: rick-anderson
description: Découvrez comment CORS comme norme pour autoriser ou rejeter des demandes Cross-Origin dans une application ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
uid: security/cors
ms.openlocfilehash: e0e0e1abf1ecaa12038b3ee1bdaa384d979be254
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666249"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Activer les requêtes Cross-Origin (CORS) dans ASP.NET Core

De [Rick Anderson](https://twitter.com/RickAndMSFT)

Cet article explique comment activer CORS dans une application ASP.NET Core.

La sécurité du navigateur empêche une page Web d’effectuer des demandes vers un autre domaine que celui qui a servi la page Web. Cette restriction est appelée *stratégie de même origine*. La stratégie de même origine empêche un site malveillant de lire des données sensibles à partir d’un autre site. Parfois, vous souhaiterez peut-être autoriser d’autres sites à effectuer des demandes Cross-Origin à votre application. Pour plus d’informations, consultez l' [article Mozilla cors](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

[Partage des ressources Cross-Origin](https://www.w3.org/TR/cors/) (cors) :

* Est une norme W3C qui permet à un serveur d’assouplir la stratégie de même origine.
* N’est **pas** une fonctionnalité de sécurité, cors assouplit la sécurité. Une API n’est pas plus sûre en autorisant CORS. Pour plus d’informations, consultez fonctionnement de [cors](#how-cors).
* Permet à un serveur d’autoriser explicitement certaines demandes Cross-Origin tout en rejetant d’autres.
* Est plus sûr et plus flexible que les techniques antérieures, telles que [JSONP](/dotnet/framework/wcf/samples/jsonp).

[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="same-origin"></a>Même origine

Deux URL ont la même origine si elles ont des schémas, des hôtes et des ports identiques ([RFC 6454](https://tools.ietf.org/html/rfc6454)).

Ces deux URL ont le même origine :

* `https://example.com/foo.html`
* `https://example.com/bar.html`

Ces URL ont des origines différentes des deux précédentes URL :

* `https://example.net` &ndash; domaine différent
* `https://www.example.com/foo.html` &ndash; sous-domaine différent
* `http://example.com/foo.html` &ndash; schéma différent
* `https://example.com:9000/foo.html` &ndash; port différent

Internet Explorer ne prend pas en compte le port lors de la comparaison des origines.

## <a name="cors-with-named-policy-and-middleware"></a>CORS avec la stratégie et l’intergiciel (middleware) nommés

L’intergiciel (middleware) CORS gère les demandes Cross-Origin. Le code suivant active CORS pour l’application entière avec l’origine spécifiée :

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

Le code précédent :

* Définit le nom de la stratégie sur «\_myAllowSpecificOrigins ». Le nom de la stratégie est arbitraire.
* Appelle la méthode d’extension <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>, qui active CORS.
* Appelle <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> avec une [expression lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). L’expression lambda prend un objet <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>. Les [options de configuration](#cors-policy-options), telles que `WithOrigins`, sont décrites plus loin dans cet article.

L’appel de la méthode <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> ajoute des services CORS au conteneur de services de l’application :

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

Pour plus d’informations, consultez les [options de stratégie cors](#cpo) dans ce document.

La méthode <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> peut chaîner des méthodes, comme indiqué dans le code suivant :

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

Remarque : l’URL **ne doit pas** contenir de barre oblique de fin (`/`). Si l’URL se termine par `/`, la comparaison retourne `false` et aucun en-tête n’est retourné.

::: moniker range=">= aspnetcore-3.0"

<a name="acpall"></a>

### <a name="apply-cors-policies-to-all-endpoints"></a>Appliquer des stratégies CORS à tous les points de terminaison

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
> Avec le routage de point de terminaison, l’intergiciel (middleware) CORS doit être configuré pour s’exécuter entre les appels à `UseRouting` et `UseEndpoints`. Une configuration incorrecte entraîne l’arrêt du fonctionnement correct de l’intergiciel.

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

Avec le routage de point de terminaison, CORS peut être activé sur une base par point de terminaison à l’aide de la `RequireCors` ensemble de méthodes d’extension.

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

L’attribut [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) fournit une alternative à l’application de cors globalement. L’attribut `[EnableCors]` active CORS pour les points de terminaison sélectionnés, plutôt que tous les points de terminaison.

Utilisez `[EnableCors]` pour spécifier la stratégie par défaut et `[EnableCors("{Policy String}")]` pour spécifier une stratégie.

L’attribut `[EnableCors]` peut être appliqué aux éléments suivants :

* `PageModel` de page Razor
* Contrôleur
* Méthode d’action du contrôleur

Vous pouvez appliquer différentes stratégies à Controller/page-Model/action avec l’attribut `[EnableCors]`. Lorsque l’attribut `[EnableCors]` est appliqué à une méthode Controllers/Model-Model/action et que CORS est activé dans l’intergiciel (middleware), les deux stratégies sont appliquées. Nous vous recommandons de combiner les stratégies. Utilisez l’attribut `[EnableCors]` ou l’intergiciel (middleware), pas les deux dans la même application.

Le code suivant applique une stratégie différente à chaque méthode :

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

Le code suivant crée une stratégie CORS par défaut et une stratégie nommée `"AnotherPolicy"`:

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a>Désactiver CORS

L’attribut [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) désactive cors pour le contrôleur/page-Model/action.

<a name="cpo"></a>

## <a name="cors-policy-options"></a>Options de stratégie CORS

Cette section décrit les différentes options qui peuvent être définies dans une stratégie CORS :

* [Définir les origines autorisées](#set-the-allowed-origins)
* [Définir les méthodes HTTP autorisées](#set-the-allowed-http-methods)
* [Définir les en-têtes de demande autorisés](#set-the-allowed-request-headers)
* [Définir les en-têtes de réponse exposés](#set-the-exposed-response-headers)
* [Informations d’identification dans les demandes Cross-Origin](#credentials-in-cross-origin-requests)
* [Définir le délai d’expiration du prévols](#set-the-preflight-expiration-time)

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> est appelé dans `Startup.ConfigureServices`. Pour certaines options, il peut être utile de lire d’abord la section [How cors Works](#how-cors) .

## <a name="set-the-allowed-origins"></a>Définir les origines autorisées

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; autorise les requêtes CORS de toutes les origines avec n’importe quel schéma (`http` ou `https`). `AllowAnyOrigin` n’est pas sécurisé, car *tout site Web* peut effectuer des demandes Cross-Origin à l’application.

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> La spécification de `AllowAnyOrigin` et `AllowCredentials` est une configuration non sécurisée et peut entraîner une falsification de requête intersites. Le service CORS retourne une réponse CORS non valide lorsqu’une application est configurée avec les deux méthodes.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> La spécification de `AllowAnyOrigin` et `AllowCredentials` est une configuration non sécurisée et peut entraîner une falsification de requête intersites. Pour une application sécurisée, spécifiez une liste exacte d’origines si le client doit s’autoriser à accéder aux ressources du serveur.

::: moniker-end

`AllowAnyOrigin` affecte les demandes préliminaires et l’en-tête `Access-Control-Allow-Origin`. Pour plus d’informations, consultez la section [demandes préliminaires](#preflight-requests) .

::: moniker range=">= aspnetcore-2.0"

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; définit la propriété <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> de la stratégie pour qu’elle soit une fonction qui permet aux origines de correspondre à un domaine générique configuré lors de l’évaluation si l’origine est autorisée.

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a>Définir les méthodes HTTP autorisées

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

* Autorise toute méthode HTTP :
* Affecte les demandes préliminaires et l’en-tête `Access-Control-Allow-Methods`. Pour plus d’informations, consultez la section [demandes préliminaires](#preflight-requests) .

### <a name="set-the-allowed-request-headers"></a>Définir les en-têtes de demande autorisés

Pour autoriser l’envoi d’en-têtes spécifiques dans une demande CORS, appelée *créer des en-têtes de demande*, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> et spécifiez les en-têtes autorisés :

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Pour autoriser tous les en-têtes de demande d’auteur, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Ce paramètre affecte les demandes préliminaires et l’en-tête de `Access-Control-Request-Headers`. Pour plus d’informations, consultez la section [demandes préliminaires](#preflight-requests) .

::: moniker range=">= aspnetcore-2.2"

Une stratégie d’intergiciel (middleware) CORS correspond aux en-têtes spécifiques spécifiés par `WithHeaders` est possible uniquement lorsque les en-têtes envoyés dans `Access-Control-Request-Headers` correspondent exactement aux en-têtes indiqués dans `WithHeaders`.

Par exemple, considérez une application configurée comme suit :

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

L’intergiciel (middleware) CORS refuse une demande préliminaire avec l’en-tête de demande suivant, car `Content-Language` ([HeaderNames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) n’est pas listé dans `WithHeaders`:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

L’application renvoie une réponse *200 OK* , mais n’envoie pas les en-têtes cors. Par conséquent, le navigateur n’essaie pas la demande Cross-Origin.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

L’intergiciel (middleware) CORS autorise toujours l’envoi de quatre en-têtes dans le `Access-Control-Request-Headers` indépendamment des valeurs configurées dans les en-têtes CorsPolicy. Cette liste d’en-têtes comprend les éléments suivants :

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

Par exemple, considérez une application configurée comme suit :

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

L’intergiciel (middleware) CORS répond correctement à une demande préliminaire avec l’en-tête de demande suivant, car `Content-Language` est toujours la liste blanche :

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a>Définir les en-têtes de réponse exposés

Par défaut, le navigateur n’expose pas tous les en-têtes de réponse à l’application. Pour plus d’informations, consultez la page [relative au partage des ressources Cross-Origin W3C (terminologie) : en-tête de réponse simple](https://www.w3.org/TR/cors/#simple-response-header).

Les en-têtes de réponse qui sont disponibles par défaut sont :

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

La spécification CORS appelle ces en *-têtes de réponse simples*en-têtes. Pour mettre d’autres en-têtes à la disposition de l’application, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>Informations d’identification dans les demandes Cross-Origin

Les informations d’identification nécessitent un traitement particulier dans une demande CORS. Par défaut, le navigateur n’envoie pas d’informations d’identification avec une demande Cross-Origin. Les informations d’identification incluent les cookies et les schémas d’authentification HTTP. Pour envoyer des informations d’identification avec une demande Cross-Origin, le client doit définir `XMLHttpRequest.withCredentials` sur `true`.

Utilisation directe de `XMLHttpRequest` :

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

Le serveur doit autoriser les informations d’identification. Pour autoriser les informations d’identification Cross-Origin, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

La réponse HTTP comprend un en-tête `Access-Control-Allow-Credentials`, qui indique au navigateur que le serveur autorise les informations d’identification pour une demande Cross-Origin.

Si le navigateur envoie des informations d’identification mais que la réponse n’inclut pas d’en-tête de `Access-Control-Allow-Credentials` valide, le navigateur n’expose pas la réponse à l’application, et la demande Cross-Origin échoue.

L’autorisation des informations d’identification Cross-Origin est un risque pour la sécurité. Un site Web dans un autre domaine peut envoyer les informations d’identification d’un utilisateur connecté à l’application pour le compte de l’utilisateur sans la connaissance de l’utilisateur. <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

La spécification CORS indique également que le paramètre Origins to `"*"` (All Origins) n’est pas valide si l’en-tête `Access-Control-Allow-Credentials` est présent.

### <a name="preflight-requests"></a>Demandes préliminaires

Pour certaines demandes CORS, le navigateur envoie une demande supplémentaire avant d’effectuer la demande réelle. Cette demande porte le nom de *demande préliminaire*. Le navigateur peut ignorer la demande préliminaire si les conditions suivantes sont remplies :

* La méthode de demande est : obtenir, début ou publication.
* L’application ne définit pas les en-têtes de requête autres que `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`ou `Last-Event-ID`.
* L’en-tête `Content-Type`, s’il est défini, a l’une des valeurs suivantes :
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

La règle sur les en-têtes de demande définie pour la demande du client s’applique aux en-têtes définis par l’application en appelant `setRequestHeader` sur l’objet `XMLHttpRequest`. La spécification CORS appelle ces en-têtes *créer des en-* têtes de demande. La règle ne s’applique pas aux en-têtes que le navigateur peut définir, par exemple `User-Agent`, `Host`ou `Content-Length`.

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

* `Access-Control-Request-Method`: méthode HTTP qui sera utilisée pour la demande réelle.
* `Access-Control-Request-Headers`: liste des en-têtes de requête que l’application définit sur la demande réelle. Comme indiqué précédemment, cela n’inclut pas les en-têtes définis par le navigateur, tels que les `User-Agent`.

Une demande préliminaire CORS peut inclure un en-tête `Access-Control-Request-Headers`, qui indique au serveur les en-têtes envoyés avec la demande réelle.

Pour autoriser des en-têtes spécifiques, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Pour autoriser tous les en-têtes de demande d’auteur, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Les navigateurs ne sont pas entièrement cohérents dans la façon dont ils définissent `Access-Control-Request-Headers`. Si vous définissez les en-têtes sur une valeur autre que `"*"` (ou utilisez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), vous devez inclure au moins `Accept`, `Content-Type`et `Origin`, ainsi que tous les en-têtes personnalisés que vous souhaitez prendre en charge.

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

La réponse comprend un en-tête `Access-Control-Allow-Methods` qui répertorie les méthodes autorisées et éventuellement un en-tête `Access-Control-Allow-Headers`, qui répertorie les en-têtes autorisés. Si la demande préliminaire est réussie, le navigateur envoie la demande réelle.

Si la demande préliminaire est refusée, l’application renvoie une réponse *200 OK* , mais n’envoie pas les en-têtes cors. Par conséquent, le navigateur n’essaie pas la demande Cross-Origin.

### <a name="set-the-preflight-expiration-time"></a>Définir en amont le délai d’expiration

L’en-tête `Access-Control-Max-Age` spécifie la durée pendant laquelle la réponse à la demande préliminaire peut être mise en cache. Pour définir cet en-tête, appelez <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:

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
  * Les navigateurs (sans CORS) ne peuvent pas effectuer de demandes Cross-Origin. Avant CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) était utilisé pour contourner cette restriction. JSONP n’utilise pas XHR, il utilise la balise `<script>` pour recevoir la réponse. Les scripts sont autorisés à être chargés sur plusieurs origines.

La [spécification cors](https://www.w3.org/TR/cors/) a introduit plusieurs nouveaux en-têtes HTTP qui permettent des demandes Cross-Origin. Si un navigateur prend en charge CORS, il définit ces en-têtes automatiquement pour les demandes cross-origin. Du code JavaScript personnalisé n’est pas nécessaire pour activer CORS.

Voici un exemple de demande Cross-Origin. L’en-tête `Origin` fournit le domaine du site qui effectue la requête. L’en-tête `Origin` est obligatoire et doit être différent de l’hôte.

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

Si le serveur autorise la demande, il définit l’en-tête `Access-Control-Allow-Origin` dans la réponse. La valeur de cet en-tête correspond à l’en-tête `Origin` de la requête ou à la valeur de caractère générique `"*"`, ce qui signifie que toute origine est autorisée :

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

Si la réponse n’inclut pas l’en-tête `Access-Control-Allow-Origin`, la demande Cross-Origin échoue. Plus précisément, le navigateur n’autorise pas la demande. Même si le serveur retourne une réponse correcte, le navigateur ne rend pas la réponse disponible pour l’application cliente.

<a name="test"></a>

## <a name="test-cors"></a>Tester CORS

Pour tester CORS :

1. [Créez un projet d’API](xref:tutorials/first-web-api). Vous pouvez également [Télécharger l’exemple](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).
1. Activez CORS à l’aide de l’une des approches décrites dans ce document. Par exemple :

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");` ne doit être utilisé que pour tester un exemple d’application semblable à l' [exemple de code de téléchargement](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).

1. Créez un projet d’application Web (Razor Pages ou MVC). L’exemple utilise Razor Pages. Vous pouvez créer l’application Web dans la même solution que le projet d’API.
1. Ajoutez le code en surbrillance suivant au fichier *index. cshtml* :

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. Dans le code précédent, remplacez `url: 'https://<web app>.azurewebsites.net/api/values/1',` par l’URL de l’application déployée.
1. Déployez le projet API. Par exemple, [déployez sur Azure](xref:host-and-deploy/azure-apps/index).
1. Exécutez l’Razor Pages ou l’application MVC à partir du bureau, puis cliquez sur le bouton **test** . Utilisez les outils F12 pour passer en revue les messages d’erreur.
1. Supprimez l’origine localhost de `WithOrigins` et déployez l’application. Vous pouvez également exécuter l’application cliente avec un autre port. Par exemple, exécutez à partir de Visual Studio.
1. Testez avec l’application cliente. Les échecs CORS retournent une erreur, mais le message d’erreur n’est pas disponible pour JavaScript. Utilisez l’onglet Console des outils F12 pour afficher l’erreur. En fonction du navigateur, vous recevez une erreur (dans la console outils F12) semblable à ce qui suit :

   * Utilisation de Microsoft Edge :

     **SEC7120 : [CORS] l’origine `https://localhost:44375` n’a pas trouvé `https://localhost:44375` dans l’en-tête de réponse Access-Control-allow-Origin pour la ressource Cross-Origin à `https://webapi.azurewebsites.net/api/values/1`**

   * Utilisation de chrome :

     **L’accès à XMLHttpRequest à `https://webapi.azurewebsites.net/api/values/1` à partir de l’origine `https://localhost:44375` a été bloqué par la stratégie CORS : aucun en-tête « Access-Control-allow-Origin » n’est présent sur la ressource demandée.**
     
Les points de terminaison compatibles CORS peuvent être testés à l’aide d’un outil tel que [Fiddler](https://www.telerik.com/fiddler) ou [postal](https://www.getpostman.com/). Lors de l’utilisation d’un outil, l’origine de la demande spécifiée par l’en-tête `Origin` doit différer de l’hôte recevant la demande. Si la requête n’est pas *une origine croisée* en fonction de la valeur de l’en-tête `Origin` :

* Il n’est pas nécessaire d’utiliser un intergiciel (middleware) CORS pour traiter la requête.
* Les en-têtes CORS ne sont pas retournés dans la réponse.

## <a name="cors-in-iis"></a>CORS dans IIS

Lors du déploiement sur IIS, CORS doit s’exécuter avant l’authentification Windows si le serveur n’est pas configuré pour autoriser l’accès anonyme. Pour prendre en charge ce scénario, le [module cors IIS](https://www.iis.net/downloads/microsoft/iis-cors-module) doit être installé et configuré pour l’application.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Partage des ressources Cross-Origin (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [Prise en main du module IIS CORS](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)
