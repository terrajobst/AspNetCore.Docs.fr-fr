---
title: Réponse mise en cache d’intergiciel (middleware) dans ASP.NET Core
author: guardrex
description: Découvrez comment configurer et utiliser le middleware de mise en cache des réponses dans ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/20/2019
uid: performance/caching/middleware
ms.openlocfilehash: ddbd547ec0fdd09da1f3e3ce30f8fa35a34870c2
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897366"
---
# <a name="response-caching-middleware-in-aspnet-core"></a>Réponse mise en cache d’intergiciel (middleware) dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex) et [John Luo](https://github.com/JunTaoLuo)

[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).

Cet article explique comment configurer l’intergiciel de mise en cache des réponses dans une application ASP.NET Core. L’intergiciel (middleware) détermine lorsque les réponses sont mis en cache, les magasins de réponses et les réponses sert à partir du cache. Pour une introduction à la mise en cache HTTP et le `ResponseCache` d’attribut, consultez [la mise en cache de réponse](xref:performance/caching/response).

## <a name="package"></a>Package

::: moniker range=">= aspnetcore-2.1"

Référence le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app) ou ajouter une référence de package à la [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) package.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Référence le [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ou ajouter une référence de package à la [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) package.

::: moniker-end

::: moniker range="= aspnetcore-1.1"

Ajouter une référence de package à la [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) package.

::: moniker-end

## <a name="configuration"></a>Configuration

Dans `Startup.ConfigureServices`, ajoutez l’intergiciel (middleware) à la collection de service.

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=9)]

Configurer l’application pour utiliser l’intergiciel (middleware) avec la `UseResponseCaching` méthode d’extension, qui ajoute l’intergiciel (middleware) au pipeline de traitement de la demande. L’exemple d’application ajoute un [ `Cache-Control` ](https://tools.ietf.org/html/rfc7234#section-5.2) en-tête à la réponse qui met en cache des réponses d’être mis en cache pendant 10 secondes. L’exemple envoie une [ `Vary` ](https://tools.ietf.org/html/rfc7231#section-7.1.4) en-tête pour configurer l’intergiciel (middleware) pour répondre à un réponse mise en cache uniquement si le [ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4) en-tête des demandes suivantes correspondent à ceux de la demande d’origine. Dans l’exemple de code qui suit, [CacheControlHeaderValue](/dotnet/api/microsoft.net.http.headers.cachecontrolheadervalue) et [HeaderNames](/dotnet/api/microsoft.net.http.headers.headernames) nécessitent un `using` instruction pour la [Microsoft.Net.Http.Headers](/dotnet/api/microsoft.net.http.headers) espace de noms.

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=17,22-29)]

Intergiciel de mise en cache des réponses met uniquement en cache les réponses du serveur qui entraînent un code d’état 200 (OK). Autres réponses, y compris [pages d’erreurs](xref:fundamentals/error-handling), sont ignorés par l’intergiciel (middleware).

> [!WARNING]
> Réponses contenant le contenu pour les clients authentifiés doivent être marqués comme non mis en cache pour empêcher l’intergiciel (middleware) à partir de stockage et traitement des ces réponses. Consultez [Conditions pour la mise en cache](#conditions-for-caching) pour plus d’informations sur la façon dont l’intergiciel (middleware) détermine si une réponse est mis en cache.

## <a name="options"></a>Options

L’intergiciel (middleware) offre trois options de contrôle de réponse mise en cache.

| Option                | Description |
| --------------------- | ----------- |
| UseCaseSensitivePaths | Détermine si les réponses sont mises en cache sur les chemins d’accès de la casse. La valeur par défaut est `false`. |
| MaximumBodySize       | La plus grande taille mis en cache pour le corps de réponse en octets. La valeur par défaut est `64 * 1024 * 1024` (64 Mo). |
| SizeLimit             | La limite de taille de l’intergiciel de cache de réponse en octets. La valeur par défaut est `100 * 1024 * 1024` (100 Mo). |

L’exemple suivant configure l’intergiciel (middleware) pour :

* Cache des réponses inférieures ou égales à 1 024 octets.
* Store les réponses par les chemins d’accès de la casse (par exemple, `/page1` et `/Page1` sont stockés séparément).

```csharp
services.AddResponseCaching(options =>
{
    options.UseCaseSensitivePaths = true;
    options.MaximumBodySize = 1024;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

Lorsque vous utilisez les contrôleurs MVC/API Web ou des modèles de page Pages Razor, le `ResponseCache` attribut spécifie les paramètres nécessaires pour définir les en-têtes appropriés pour la mise en cache de réponse. Le seul paramètre de la `ResponseCache` attribut nécessitant strictement l’intergiciel (middleware) est `VaryByQueryKeys`, qui ne correspond pas à un en-tête HTTP réelle. Pour plus d’informations, consultez [ResponseCache attribut](xref:performance/caching/response#responsecache-attribute).

Lorsque vous N'utilisez pas le `ResponseCache` attribut, la mise en cache de réponse peut être modifié avec le `VaryByQueryKeys` fonctionnalité. Utilisez le `ResponseCachingFeature` directement à partir de la `IFeatureCollection` de la `HttpContext`:

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();
if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

En utilisant une seule valeur égale à `*` dans `VaryByQueryKeys` varie le cache par tous les paramètres de requête de demande.

## <a name="http-headers-used-by-response-caching-middleware"></a>En-têtes HTTP utilisés par l’intergiciel de mise en cache des réponses

Réponse mise en cache à l’intergiciel (middleware) est configuré à l’aide d’en-têtes HTTP.

| Header | Détails |
| ------ | ------- |
| Autorisation | La réponse n’est pas mis en cache si l’en-tête existe. |
| Cache-Control | Le middleware considère uniquement la mise en cache des réponses marquées avec la `public` directive de cache. Contrôler la mise en cache avec les paramètres suivants :<ul><li>max-age</li><li>obsolescence maximale&#8224;</li><li>frais de min</li><li>doit-revalidate.</li><li>non-cache</li><li>non-store</li><li>only-if-cached</li><li>private</li><li>public</li><li>s-maxage</li><li>proxy-revalidate&#8225;</li></ul>&#8224;Si aucune limite n’est spécifiée pour `max-stale`, l’intergiciel (middleware) n’effectue aucune action.<br>&#8225;`proxy-revalidate`a le même effet que `must-revalidate`.<br><br>Pour plus d’informations, consultez [7231 relative aux RFC : Demander des Directives de Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2.1). |
| Pragma | Un `Pragma: no-cache` en-tête dans la requête produit le même effet que `Cache-Control: no-cache`. Cet en-tête est remplacé par les directives pertinentes dans le `Cache-Control` en-tête, le cas échéant. Considéré comme pour la compatibilité descendante avec HTTP/1.0. |
| Set-Cookie | La réponse n’est pas mis en cache si l’en-tête existe. N’importe quel intergiciel (middleware) dans le pipeline de traitement de la requête qui définit un ou plusieurs cookies empêche l’intergiciel de mise en cache de réponse de la mise en cache la réponse (par exemple, le [fournisseur TempData basé sur cookie](xref:fundamentals/app-state#tempdata)).  |
| Vary | Le `Vary` en-tête est utilisé pour faire varier la réponse mise en cache par un autre en-tête. Par exemple, mettre en cache les réponses par l’encodage en incluant le `Vary: Accept-Encoding` en-tête, qui met en cache des réponses aux requêtes avec des en-têtes `Accept-Encoding: gzip` et `Accept-Encoding: text/plain` séparément. Une réponse avec une valeur d’en-tête de `*` n’est jamais stocké. |
| Arrive à expiration | Une réponse jugée obsolète par cet en-tête n’est pas stockée ou récupérée sauf substitution par d’autres `Cache-Control` en-têtes. |
| If-None-Match | La réponse complète est pris en charge à partir du cache si la valeur n’est pas `*` et `ETag` de la réponse ne correspond à aucune des valeurs fournies. Sinon, une réponse 304 (non modifié) est pris en charge. |
| If-Modified-Since | Si le `If-None-Match` en-tête n’est pas présent, une réponse complète est traitée à partir du cache si la date de la réponse mise en cache est plus récente que la valeur fournie. Sinon, une réponse 304 (non modifié) est pris en charge. |
| Date | Lors du service de cache, le `Date` en-tête est défini par l’intergiciel (middleware) si elle n’a pas été fourni sur la réponse d’origine. |
| Content-Length | Lors du service de cache, le `Content-Length` en-tête est défini par l’intergiciel (middleware) si elle n’a pas été fourni sur la réponse d’origine. |
| Âge | Le `Age` en-tête envoyé dans la réponse d’origine est ignorée. L’intergiciel (middleware) calcule une nouvelle valeur avec une réponse mise en cache. |

## <a name="caching-respects-request-cache-control-directives"></a>La mise en cache respecte les directives de contrôle de Cache de demande

L’intergiciel (middleware) respecte les règles de la [spécification HTTP 1.1 Caching](https://tools.ietf.org/html/rfc7234#section-5.2). Les règles requièrent un cache d’honorer valide `Cache-Control` en-tête envoyé par le client. Sous la spécification, un client peut envoyer des requêtes avec un `no-cache` valeur d’en-tête et force le serveur génère une nouvelle réponse pour chaque demande. Actuellement, il n’existe aucun contrôle du développeur sur ce comportement de mise en cache lors de l’utilisation de l’intergiciel (middleware), car l’intergiciel (middleware) conforme à la spécification de mise en cache officielle.

Pour contrôler le comportement de mise en cache plus, explorez les autres fonctionnalités de mise en cache d’ASP.NET Core. Consultez les rubriques suivantes :

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a>Résolution des problèmes

Si le comportement de mise en cache n’est pas comme prévu, vérifiez que les réponses sont mis en cache et capable de servies à partir du cache. Examinez les en-têtes entrants de la demande et la réponse sortante. Activer [journalisation](xref:fundamentals/logging/index) pour faciliter le débogage.

Lorsque le test et résolution des problèmes de comportement de mise en cache, un navigateur peut définir des en-têtes de demande qui affectent la mise en cache de façon non souhaitée. Par exemple, un navigateur peut-être définir le `Cache-Control` en-tête à `no-cache` ou `max-age=0` lors de l’actualisation d’une page. Les outils suivants peuvent définir explicitement les en-têtes de demande et sont préférables pour tester la mise en cache :

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>Conditions pour la mise en cache

* La demande doit avoir pour résultat dans une réponse du serveur avec un code d’état 200 (OK).
* La méthode de demande doit être GET ou HEAD.
* Dans `Startup.Configure`, intergiciel (middleware) de réponse mise en cache doit être placé avant un intergiciel (middleware) qui nécessitent la compression. Pour plus d'informations, consultez <xref:fundamentals/middleware/index>.
* Le `Authorization` en-tête ne doit pas être présent.
* `Cache-Control` paramètres de l’en-tête doivent être valides, et la réponse doit être marquée `public` et pas marquée `private`.
* Le `Pragma: no-cache` en-tête ne doit pas être présent si la `Cache-Control` en-tête n’est pas présent, comme le `Cache-Control` en-tête remplace le `Pragma` en-tête lorsqu’il est présent.
* Le `Set-Cookie` en-tête ne doit pas être présent.
* `Vary` paramètres de l’en-tête doivent être valide et non égal à `*`.
* Le `Content-Length` valeur d’en-tête (si défini) doit correspondre à la taille du corps de réponse.
* Le [IHttpSendFileFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpsendfilefeature) n’est pas utilisé.
* La réponse ne doit pas être obsolète comme spécifié par le `Expires` en-tête et le `max-age` et `s-maxage` directives de mettre en cache.
* Mise en mémoire tampon de réponse doit être réussie et la taille de la réponse doit être inférieure à la configuration ou par défaut `SizeLimit`.
* La réponse doit être mis en cache en fonction de la [RFC 7234](https://tools.ietf.org/html/rfc7234) spécifications. Par exemple, le `no-store` directive ne doit pas exister dans les champs d’en-tête de demande ou réponse. Consultez *Section 3 : Stockage des réponses dans les Caches* de [RFC 7234](https://tools.ietf.org/html/rfc7234) pour plus d’informations.

> [!NOTE]
> Jeux d’attaques par le système anti-contrefaçon pour générer des jetons sécurisés pour empêcher Cross-Site demande falsification de la `Cache-Control` et `Pragma` en-têtes à `no-cache` afin que les réponses ne sont pas mises en cache. Pour plus d’informations sur la désactivation des jetons anti-contrefaçon pour les éléments de formulaire HTML, consultez [configuration anti-contrefaçon ASP.NET Core](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration).

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
