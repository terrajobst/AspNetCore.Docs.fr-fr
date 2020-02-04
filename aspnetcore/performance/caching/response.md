---
title: Mise en cache de la réponse dans ASP.NET Core
author: rick-anderson
description: Découvrez comment utiliser la mise en cache des réponses pour réduire les besoins en bande passante et accroître les performances des applications ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 11/04/2019
uid: performance/caching/response
ms.openlocfilehash: ab5d1414ae72edade81ab55aef6b0fa5af30f0f4
ms.sourcegitcommit: 990a4c2e623c202a27f60bdf3902f250359c13be
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/03/2020
ms.locfileid: "76971976"
---
# <a name="response-caching-in-aspnet-core"></a>Mise en cache de la réponse dans ASP.NET Core

Par [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/)et [Luke Latham](https://github.com/guardrex)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/response/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

La mise en cache de la réponse réduit le nombre de demandes que le client ou le proxy fait à un serveur web. La mise en cache de la réponse réduit également la quantité de travail que le serveur web exécute pour générer une réponse. La mise en cache de la réponse est contrôlée par des en-têtes qui spécifient comment vous souhaitez que le client, le proxy et l'intergiciel (middleware) mettent en cache les réponses.

L' [attribut ResponseCache](#responsecache-attribute) participe à la définition des en-têtes de mise en cache des réponses. Les clients et les proxys intermédiaires doivent honorer les en-têtes pour la mise en cache des réponses sous la [spécification de Caching HTTP 1,1](https://tools.ietf.org/html/rfc7234).

Pour la mise en cache côté serveur qui suit la spécification de mise en cache HTTP 1,1, utilisez l’intergiciel (middleware) de [mise en](xref:performance/caching/middleware)cache des réponses. L’intergiciel peut utiliser les propriétés de <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> pour influencer le comportement de mise en cache côté serveur.

## <a name="http-based-response-caching"></a>Mise en cache des réponses HTTP

La [spécification de la mise en cache HTTP 1.1](https://tools.ietf.org/html/rfc7234) décrit le comportement des caches sur Internet. L’en-tête HTTP principal utilisé pour la mise en cache est [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), qui est utilisé pour spécifier des *directives* de cache. Les directives contrôlent le comportement de mise en cache en fonction des demandes en provenance des clients et des serveurs, tandis que les réponses sont rétablies des serveurs aux clients. Les demandes et les réponses transitent via des serveurs proxy, qui doivent être conformes à la spécification de la mise en cache HTTP 1.1.

Les directives `Cache-Control` courantes sont affichées dans le tableau suivant.

| Directive                                                       | Action |
| --------------------------------------------------------------- | ------ |
| [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | Un cache peut stocker la réponse. |
| [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | La réponse ne doit pas être stockée par un cache partagé. Un cache privé peut stocker et réutiliser la réponse. |
| [max-age](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | Le client n’accepte pas de réponse dont l’âge est supérieur au nombre de secondes spécifié. Exemples : `max-age=60` (60 secondes), `max-age=2592000` (1 mois) |
| [no-cache](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **Sur les demandes**: un cache ne doit pas utiliser une réponse stockée pour satisfaire la demande. Le serveur d’origine régénère la réponse pour le client, et l’intergiciel met à jour la réponse stockée dans son cache.<br><br>**Sur les réponses**: la réponse ne doit pas être utilisée pour une demande ultérieure sans validation sur le serveur d’origine. |
| [non-Store](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **Sur les demandes**: un cache ne doit pas stocker la demande.<br><br>**Sur les réponses**: un cache ne doit pas stocker une partie de la réponse. |

Les autres en-têtes de cache qui jouent un rôle dans la mise en cache sont affichés dans le tableau suivant.

| Header                                                     | Fonction |
| ---------------------------------------------------------- | -------- |
| [Vieillissement](https://tools.ietf.org/html/rfc7234#section-5.1)     | Une estimation de la durée en secondes écoulées depuis que la réponse a été générée ou validée sur le serveur d’origine. |
| [Expire](https://tools.ietf.org/html/rfc7234#section-5.3) | Heure après laquelle la réponse est considérée comme périmée. |
| [Pragma](https://tools.ietf.org/html/rfc7234#section-5.4)  | Existe pour la compatibilité descendante avec les caches HTTP/1.0 pour affecter le comportement `no-cache`. Si l'en-tête `Cache-Control` est présent, l'en-tête `Pragma` est ignoré. |
| [Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | Spécifie qu’une réponse mise en cache ne doit pas être envoyée avant que tous les champs d'en-tête `Vary` correspondent dans la demande d’origine et la nouvelle demande de la réponse mise en cache. |

## <a name="http-based-caching-respects-request-cache-control-directives"></a>La mise en cache basée sur HTTP respecte les directives Cache-Control de la demande

La [spécification de la mise en cache HTTP 1.1 pour l’en-tête Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) requiert qu'un cache respecte un en-tête `Cache-Control` valide envoyé par le client. Un client peut envoyer des demandes avec une valeur d’en-tête `no-cache` et forcer le serveur à générer une nouvelle réponse pour chaque demande.

Le fait de toujours respecter les en-têtes de demande `Cache-Control` du client a du sens si vous avez pour objectif la mise en cache HTTP. Sous la spécification officielle, la mise en cache vise à réduire la latence et la charge réseau pour satisfaire les demandes sur un réseau via les clients, les proxies et les serveurs. Ce n’est pas nécessairement un moyen de contrôler la charge sur un serveur d’origine.

Il n’existe aucun contrôle de développeur sur ce comportement de mise en cache lors de l’utilisation de l’intergiciel (middleware) de [mise en cache des réponses](xref:performance/caching/middleware) , car celui-ci adhère à la spécification officielle de mise en cache. [Les améliorations prévues pour l’intergiciel (middleware](https://github.com/dotnet/AspNetCore/issues/2612) ) permettent de configurer l’intergiciel afin d’ignorer l’en-tête de `Cache-Control` d’une demande lorsque vous décidez de traiter une réponse mise en cache. Les améliorations planifiées permettent de mieux contrôler la charge du serveur.

## <a name="other-caching-technology-in-aspnet-core"></a>Autres technologies de mise en cache dans ASP.NET Core

### <a name="in-memory-caching"></a>Mise en cache en mémoire

La mise en cache utilise la mémoire serveur pour stocker les données mises en cache. Ce type de mise en cache est adapté à un ou plusieurs serveurs à l’aide de *sessions rémanentes*. Les sessions rémanentes signifient que les demandes des clients sont toujours acheminées vers le même serveur pour traitement.

Pour plus d'informations, consultez <xref:performance/caching/memory>.

### <a name="distributed-cache"></a>Cache distribué

Utilisez un cache distribué pour stocker des données en mémoire lorsque l’application est hébergée dans une batterie de serveurs ou sur le cloud. Le cache est partagé entre les serveurs qui traitent les demandes. Un client peut soumettre une demande traitée par n’importe quel serveur dans le groupe si les données mises en cache pour le client sont disponibles. ASP.NET Core fonctionne avec les caches distribués SQL Server, [ReDim](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis)et [NCache](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/) .

Pour plus d'informations, consultez <xref:performance/caching/distributed>.

### <a name="cache-tag-helper"></a>Tag helper de cache

Mettez en cache le contenu à partir d’une vue MVC ou d’une page Razor avec le tag Helper cache. Le tag helper de cache utilise la mise en cache en mémoire pour stocker les données.

Pour plus d'informations, consultez <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>.

### <a name="distributed-cache-tag-helper"></a>Tag Helper Cache distribué

Mettez en cache le contenu à partir d’une vue MVC ou d’une page Razor dans des scénarios de batterie de serveurs Web ou de Cloud distribué avec le tag Helper de cache distribué. Le tag Helper de cache distribué utilise SQL Server, les [redims](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis)ou [NCache](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/) pour stocker des données.

Pour plus d'informations, consultez <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>.

## <a name="responsecache-attribute"></a>Attribut ResponseCache

L' <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> spécifie les paramètres nécessaires à la définition des en-têtes appropriés dans la mise en cache des réponses.

> [!WARNING]
> Désactivez la mise en cache du contenu qui contient des informations pour les clients authentifiés. La mise en cache doit seulement être activée pour le contenu qui ne change pas selon l’identité d’un utilisateur ou si un utilisateur est connecté.

<xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> fait varier la réponse stockée en fonction des valeurs de la liste donnée des clés de requête. Quand une seule valeur de `*` est fournie, l’intergiciel (middleware) varie les réponses par tous les paramètres de chaîne de requête de la demande.

L' [intergiciel de mise en cache des réponses](xref:performance/caching/middleware) doit être activé pour définir la propriété <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys>. Dans le cas contraire, une exception Runtime est levée. Il n’existe pas d’en-tête HTTP correspondant pour la propriété <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys>. La propriété est une fonctionnalité HTTP gérée par l’intergiciel (middleware) de mise en cache des réponses. Pour que l’intergiciel traite une réponse mise en cache, la chaîne de requête et la valeur de la chaîne de requête doivent correspondre à une demande précédente. Par exemple, considérez la séquence des demandes et des résultats présentés dans le tableau suivant.

| Requête                          | Résultat                    |
| -------------------------------- | ------------------------- |
| `http://example.com?key1=value1` | Retourné à partir du serveur. |
| `http://example.com?key1=value1` | Renvoyé par l’intergiciel (middleware). |
| `http://example.com?key1=value2` | Retourné à partir du serveur. |

La première demande est retournée par le serveur et mise en cache dans l’intergiciel (middleware). La deuxième demande est retournée par l’intergiciel (middleware), car la chaîne de requête correspond à la requête précédente. La troisième demande n’est pas dans le cache de l’intergiciel (middleware), car la valeur de la chaîne de requête ne correspond pas à une demande précédente.

Le <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> est utilisé pour configurer et créer (via <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>) un `Microsoft.AspNetCore.Mvc.Internal.ResponseCacheFilter`. Le `ResponseCacheFilter` effectue le travail de mise à jour des en-têtes HTTP appropriés et des fonctionnalités de la réponse. Le filtre :

* Supprime tous les en-têtes existants pour `Vary`, `Cache-Control`et `Pragma`.
* Écrit les en-têtes appropriés en fonction des propriétés définies dans l' <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>.
* Met à jour la fonctionnalité HTTP de mise en cache des réponses si <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> est définie.

### <a name="vary"></a>Vary

Cet en-tête est écrit uniquement lorsque la propriété <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> est définie. La propriété est définie sur la valeur de la propriété `Vary`. L’exemple suivant utilise la propriété <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> :

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache1.cshtml.cs?name=snippet)]

À l’aide de l’exemple d’application, affichez les en-têtes de réponse avec les outils réseau du navigateur. Les en-têtes de réponse suivants sont envoyés avec la réponse de la page Cache1 :

```
Cache-Control: public,max-age=30
Vary: User-Agent
```

### <a name="nostore-and-locationnone"></a>NoStore et location. None

<xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> remplace la plupart des autres propriétés. Lorsque cette propriété a la valeur `true`, l'en-tête `Cache-Control` est défini sur `no-store`. Si <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> a la valeur `None`:

* `Cache-Control` a la valeur `no-store,no-cache`.
* `Pragma` a la valeur `no-cache`.

Si <xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> est `false` et <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> est `None`, `Cache-Control`et `Pragma` sont définis sur `no-cache`.

<xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> est généralement défini sur `true` pour les pages d’erreurs. La page cache2 de l’exemple d’application génère des en-têtes de réponse qui indiquent au client de ne pas stocker la réponse.

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache2.cshtml.cs?name=snippet)]

L’exemple d’application retourne la page cache2 avec les en-têtes suivants :

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>Emplacement et durée

Pour activer la mise en cache, <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> doit être défini sur une valeur positive et <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> doit être soit `Any` (valeur par défaut), soit `Client`. L’infrastructure définit l’en-tête `Cache-Control` sur la valeur d’emplacement suivie par le `max-age` de la réponse.

les options de <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>de `Any` et de `Client` se traduisent en `Cache-Control` valeurs d’en-tête de `public` et `private`, respectivement. Comme indiqué dans la section [NoStore and location. None](#nostore-and-locationnone) , la définition de <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> sur `None` définit à la fois les en-têtes `Cache-Control` et `Pragma` sur `no-cache`.

`Location.Any` (`Cache-Control` défini sur `public`) indique que le *client ou un proxy intermédiaire* peut mettre en cache la valeur, y compris l' [intergiciel de mise en](xref:performance/caching/middleware)cache des réponses.

`Location.Client` (`Cache-Control` défini sur `private`) indique que *seul le client* peut mettre en cache la valeur. Aucun cache intermédiaire ne doit mettre en cache la valeur, y compris l' [intergiciel de mise en](xref:performance/caching/middleware)cache des réponses.

Les en-têtes de contrôle de cache fournissent simplement des conseils aux clients et aux proxies intermédiaires quand et comment mettre en cache des réponses. Il n’y a aucune garantie que les clients et les proxys honoreront la [spécification de mise en cache HTTP 1,1](https://tools.ietf.org/html/rfc7234). L’intergiciel (middleware) de [mise en cache des réponses](xref:performance/caching/middleware) suit toujours les règles de mise en cache établies par la spécification.

L’exemple suivant montre le modèle de page cache3 de l’exemple d’application et les en-têtes produits en définissant <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> et en conservant la valeur de <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> par défaut :

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache3.cshtml.cs?name=snippet)]

L’exemple d’application retourne la page cache3 avec l’en-tête suivant :

```
Cache-Control: public,max-age=10
```

### <a name="cache-profiles"></a>Profils de cache

Au lieu de dupliquer les paramètres du cache de réponse sur de nombreux attributs d’action de contrôleur, les profils de cache peuvent être configurés en tant qu’options lors de la configuration de MVC/Razor Pages dans `Startup.ConfigureServices`. Les valeurs trouvées dans un profil de cache référencé sont utilisées par défaut par le <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> et sont remplacées par les propriétés spécifiées sur l’attribut.

Configurez un profil de cache. L’exemple suivant montre un profil de cache de 30 secondes dans le `Startup.ConfigureServices`de l’exemple d’application :

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

Le modèle de page cache4 de l’exemple d’application fait référence au profil de cache `Default30` :

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache4.cshtml.cs?name=snippet)]

Le <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> peut être appliqué aux éléments suivants :

* Les gestionnaires de page Razor (classes) &ndash; attributs ne peuvent pas être appliqués aux méthodes de gestionnaire.
* Contrôleurs MVC (classes).
* Les actions MVC (méthodes) &ndash; les attributs au niveau de la méthode remplacent les paramètres spécifiés dans les attributs de niveau classe.

En-tête résultant appliqué à la réponse de la page cache4 par le profil de cache `Default30` :

```
Cache-Control: public,max-age=30
```

## <a name="additional-resources"></a>Ressources supplémentaires

* [Stockage des réponses dans les caches](https://tools.ietf.org/html/rfc7234#section-3)
* [Cache-Control](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
