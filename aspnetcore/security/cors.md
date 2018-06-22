---
title: Activer les demandes Cross-Origin (CORS) dans ASP.NET Core
author: rick-anderson
description: Découvrez comment CORS comme une norme pour autoriser ou rejeter les demandes cross-origin dans une application ASP.NET Core.
ms.author: riande
ms.date: 05/17/2017
uid: security/cors
ms.openlocfilehash: 2920917d0a488e72afb94d65bdc6d7034c6f66a9
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278659"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Activer les demandes Cross-Origin (CORS) dans ASP.NET Core

Par [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), et [Tom Dykstra](https://github.com/tdykstra)

La sécurité du navigateur empêche une page web d’effectuer des demandes AJAX vers un autre domaine. Cette restriction est appelée la *stratégie de même origine* et empêche un site malveillant de lire des données sensibles à partir d’un autre site. Toutefois, vous pouvez parfois permettre aux autres sites de faire des demandes cross-origin à votre API web.

[Cross-origine partage de ressources](http://www.w3.org/TR/cors/) (CORS) est une norme W3C qui permet à un serveur d’abaisser la stratégie de même origine. À l’aide de CORS, un serveur peut autoriser explicitement certaines demandes cross-origin lors du refus d’autres. CORS est plus sûre et plus flexible que des techniques antérieures telles que [JSONP](https://wikipedia.org/wiki/JSONP). Cette rubrique montre comment activer CORS dans une application ASP.NET Core.

## <a name="what-is-same-origin"></a>Que veut dire « même origine » ?

Deux URL ayant la même origine, s’ils ont des ports, des hôtes et des schémas identiques. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Ces deux URL ayant la même origine :

* `http://example.com/foo.html`

* `http://example.com/bar.html`

Ces URL ont des origines différentes aux deux précédentes :

* `http://example.net` -Autre domaine

* `http://www.example.com/foo.html` -Autre sous-domaine

* `https://example.com/foo.html` -Autre schéma

* `http://example.com:9000/foo.html` -Autre port

> [!NOTE]
> Internet Explorer ne considère pas le port lors de la comparaison des origines.

## <a name="setting-up-cors"></a>Configuration des règles CORS

Pour configurer des règles CORS pour votre application, ajoutez le package `Microsoft.AspNetCore.Cors` dans votre projet.

Ajouter les services CORS dans Startup.cs :

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a>L’activation de CORS avec intergiciel (middleware)

Afin d’activer CORS pour toute l'application, ajoutez l’intergiciel (middleware) CORS à votre pipeline de demande avec la méthode d'extension `UseCors`. Notez que l’intergiciel (middleware) CORS doit précéder tous les points de terminaison définis dans votre application pour lesquels vous souhaitez prendre en charge les demandes cross-origin (par ex. avant tout appel à `UseMvc`).

Vous pouvez spécifier une stratégie cross-origine lors de l’ajout de l’intergiciel (middleware) CORS à l’aide la classe `CorsPolicyBuilder`. Il existe deux manières de procéder. La première consiste à appeler UseCors avec une expression lambda :


[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

**Remarque :** l’URL doit être spécifiée sans barre oblique de fin (`/`). Si l’URL se termine par `/`, la comparaison retournera `false` et aucun en-tête n’est renvoyé.

L’expression lambda prend un objet `CorsPolicyBuilder`. Vous trouverez une liste des [options de configuration](#cors-policy-options) plus loin dans cette rubrique. Dans cet exemple, la stratégie autorise les demandes cross-origin de `http://example.com` et aucune autre origine.

Notez que `CorsPolicyBuilder` possède une API Fluent, et vous pouvez donc enchaîner des appels à la méthode :

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

La deuxième approche consiste à définir une ou plusieurs stratégies CORS nommées, puis sélectionnez la stratégie par nom au moment de l’exécution.

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

Cet exemple ajoute une stratégie CORS nommée « AllowSpecificOrigin ». Pour sélectionner la stratégie, passez le nom à `UseCors`.

## <a name="enabling-cors-in-mvc"></a>L’activation de CORS dans MVC

Vous pouvez également utiliser MVC pour appliquer des CORS spécifiques par action, par contrôleur, ou pour tous les contrôleurs. Lors de l’utilisation de MVC pour activer CORS, les mêmes services CORS sont employés, mais pas l’intergiciel (middleware) CORS

### <a name="per-action"></a>Par action

Afin de spécifier une stratégie CORS pour une action spécifique, ajoutez l'attribut `[EnableCors]` à l’action. Spécifiez le nom de la stratégie.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a>Par contrôleur

Afin de spécifier la stratégie CORS pour un contrôleur spécifique, ajoutez l'attribut `[EnableCors]` à la classe de contrôleur. Spécifiez le nom de la stratégie.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a>Global

Vous pouvez activer CORS globalement pour tous les contrôleurs en ajoutant le filtre `CorsAuthorizationFilterFactory`à la collection de filtres globaux :

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

L’ordre de priorité est : Action, contrôleur, global. Les stratégies au niveau de l’action sont prioritaires sur les stratégies au niveau du contrôleur et les stratégies au niveau du contrôleur sont prioritaires sur les stratégies globales.

### <a name="disable-cors"></a>Désactiver les CORS

Pour désactiver les CORS pour un contrôleur ou une action, utilisez l'attribut `[DisableCors]`.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a>Options de stratégie CORS

Cette section décrit les différentes options que vous pouvez définir dans une stratégie CORS.

* [Définir les origines autorisées](#set-the-allowed-origins)

* [Définir les méthodes HTTP autorisées](#set-the-allowed-http-methods)

* [Définir les en-têtes de requête autorisée](#set-the-allowed-request-headers)

* [Définir les en-têtes de réponse exposé](#set-the-exposed-response-headers)

* [Informations d’identification dans les demandes cross-origin](#credentials-in-cross-origin-requests)

* [Définir en amont le délai d’expiration](#set-the-preflight-expiration-time)

Pour certaines options, il peut être utile de lire en premier [Comment fonctionne CORS](#how-cors-works).

### <a name="set-the-allowed-origins"></a>Définir les origines autorisées

Pour autoriser un ou plusieurs origines spécifiques :

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

Pour autoriser toutes les origines :

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

Réfléchissez bien avant d’autoriser des demandes à partir de n’importe quelle origine. Cela signifie que tout site Web peut effectuer des appels d’AJAX à votre API.

### <a name="set-the-allowed-http-methods"></a>Définir les méthodes HTTP autorisées

Pour autoriser toutes les méthodes HTTP :

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

Cela affecte les demandes de contrôle préliminaire et l’en-tête de l’accès en contrôle-Allow-Methods.

### <a name="set-the-allowed-request-headers"></a>Définir les en-têtes de requête autorisée

Une demande préliminaire CORS peut inclure un en-tête Access-Control-Request-Headers, qui répertorie les en-têtes HTTP définis par l’application (appelés « en-têtes de demande d’auteur »).

À des en-têtes d’autorisation spécifiques :

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

Pour autoriser tous les en-têtes de demande d'auteur:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

Les navigateurs ne sont pas entièrement cohérents dans leur définition Access-Control-Request-Headers. Si vous définissez les en-têtes sur tout élément autre que « * », vous devez inclure au moins « accept », « content-type » et « origin », ainsi que tous les en-têtes personnalisés que vous souhaitez prendre en charge.

### <a name="set-the-exposed-response-headers"></a>Définir les en-têtes de réponse exposé

Par défaut, le navigateur n’expose pas tous les en-têtes de réponse à l’application. (Consultez [ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header).) Les en-têtes de réponse qui sont disponibles par défaut sont :

* Cache-Control

* Content-Language

* Type de contenu

* Arrive à expiration

* Dernière modification

* Pragma

La spécification CORS appelle ces en-têtes : *les en-têtes de réponse simple*. Pour rendre d’autres en-têtes accessibles à l’application :

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a>Informations d’identification dans les demandes cross-origin

Les informations d’identification nécessitent un traitement particulier dans une demande CORS. Par défaut, le navigateur n’envoie pas d’informations d’identification avec une demande cross-origin. Les informations d’identification incluent les cookies, ainsi que des schémas d’authentification HTTP. Pour envoyer des informations d’identification avec une demande cross-origin, le client doit définir la propriété `XMLHttpRequest.withCredentials` sur true.

À l’aide de XMLHttpRequest directement :

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

Dans jQuery :

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

En outre, le serveur doit autoriser les informations d’identification. Pour autoriser les informations d’identification cross-origine :

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

La réponse HTTP inclut désormais un en-tête Access-Control-Allow-Credentials, lequel indique au navigateur que le serveur autorise les informations d’identification pour une demande cross-origin.

Si le navigateur envoie des informations d’identification, mais que la réponse n’inclut aucun en-tête Access-contrôle-Allow-Credentials valide, le navigateur n'exposera pas la réponse à l’application et la requête AJAX échouera.

Soyez prudent lorsque vous autorisez les informations d’identification cross-origin. Un site web à un autre domaine peut envoyer les informations d’identification d’un utilisateur connecté à l’application de la part de l’utilisateur sans que celui-ci le sache. La spécification CORS indique également que la définition des origines sur « * » (toutes les origines) n’est pas valide si l'en-tête `Access-Control-Allow-Credentials` est présent.

### <a name="set-the-preflight-expiration-time"></a>Définir en amont le délai d’expiration

L’en-tête `Access-contrôle-Max-Age` spécifie la durée pendant laquelle la réponse à la demande préliminaire peut être mise en cache. Pour définir cet en-tête :
 Pour définir cet en-tête :

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a>Fonctionnement des règles CORS

Cette section décrit ce qui se passe dans une demande CORS au niveau des messages HTTP. Il est important de comprendre le fonctionnement de CORS afin que la stratégie CORS puisse être configurée correctement et analysée lorsque des comportements inattendus se produisent.

La spécification CORS introduit plusieurs nouveaux en-têtes HTTP qui permettent les demandes cross-origin. Si un navigateur prend en charge CORS, il définit ces en-têtes automatiquement pour les demandes cross-origin. Du code JavaScript personnalisé n’est pas nécessaire pour activer CORS.

Voici un exemple de demande cross-origin. Le `Origin` en-tête fournit le domaine du site qui effectue la demande :

```
GET http://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: http://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: http://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

Si le serveur autorise la demande, il définit l’en-tête `Access-Control-Allow-Origin` dans la réponse. La valeur de cet en-tête correspond à l’en-tête Origin de la demande ou à la valeur de caractère générique « * », ce qui signifie que toute origine est autorisée :

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

Si la réponse n’inclut pas l’en-tête `Access-Control-Allow-Origin`, la requête AJAX échoue. Plus précisément, le navigateur n’autorise pas la demande. Même si le serveur retourne une réponse correcte, le navigateur ne rend pas la réponse disponible à l’application cliente.

### <a name="preflight-requests"></a>Demandes préliminaires

Pour certaines requêtes CORS, le navigateur envoie une demande supplémentaire, appelée « demande préliminaire, » avant d’envoyer la demande réelle de la ressource. Le navigateur peut ignorer la demande préliminaire si les conditions suivantes sont remplies :

* La méthode de demande est GET, HEAD ou POST

* L’application ne définit aucun en-tête de demande autre que `Accept`, `Accept-Language`, `Content-Language`, `Content-Type` ou Last-Event-ID, et

* L’en-tête `Content-Type` (si défini) est une des opérations suivantes :

  * application/x-www-form-urlencoded

  * multipart/form-data

  * texte brut

La règle sur les en-têtes de demande s’applique aux en-têtes que l’application définit en appelant `setRequestHeader` sur l’objet XMLHttpRequest. (La spécification CORS les appelle « en-têtes de demande auteur ».) La règle ne s’applique pas aux en-têtes que le navigateur peut définir, telles que User-Agent, Host ou Content-Length.

Voici un exemple d’une requête préliminaire :

```
OPTIONS http://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: http://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

La demande préliminaire utilise la méthode HTTP OPTIONS. Elle inclut deux en-têtes spéciales :

* Access-Control-Request-Method : Méthode HTTP qui sera utilisée pour la demande réelle.

* Access-Control-Request-Headers : Liste des en-têtes de demande que l’application est définie sur la demande réelle. (Là encore, cela n’inclut pas les en-têtes qui définit par le navigateur.)

Voici un exemple de réponse, en supposant que le serveur autorise la demande :

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

La réponse inclut un en-tête `Access-Control-Allow-Methods` qui répertorie les méthodes autorisées et éventuellement un en-tête `Access-Control-Allow-Headers`, qui répertorie les en-têtes autorisés. Si la demande préliminaire réussit, le navigateur envoie la demande réelle, comme décrit précédemment.
