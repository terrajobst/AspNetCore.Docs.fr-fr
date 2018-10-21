---
title: Mise en cache de la réponse dans ASP.NET Core
author: rick-anderson
description: Découvrez comment utiliser la mise en cache de réponse pour diminuer la bande passante et améliorer les performances des applications ASP.NET Core.
ms.author: riande
ms.date: 09/20/2017
uid: performance/caching/response
ms.openlocfilehash: 4bf61502738d70760679ec98c8f2f303eca9d504
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477487"
---
# <a name="response-caching-in-aspnet-core"></a>Mise en cache de la réponse dans ASP.NET Core

Par [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), et [Luke Latham](https://github.com/guardrex)

> [!NOTE]
> Réponse mise en cache dans les Pages Razor est disponible dans ASP.NET Core 2.1 ou version ultérieure.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

La mise en cache de la réponse réduit le nombre de demandes que le client ou le proxy fait à un serveur web. La mise en cache de la réponse réduit également la quantité de travail que le serveur web exécute pour générer une réponse. La mise en cache de la réponse est contrôlée par des en-têtes qui spécifient comment vous souhaitez que le client, le proxy et l'intergiciel (middleware) mettent en cache les réponses.

Le serveur web peut mettre en cache les réponses lorsque vous ajoutez [l'intergiciel (middleware) de mise en cache de réponse](xref:performance/caching/middleware).

## <a name="http-based-response-caching"></a>La mise en cache de réponse HTTP

Le [spécification HTTP 1.1 Caching](https://tools.ietf.org/html/rfc7234) décrit le comportement de caches d’Internet. L’en-tête HTTP principal utilisé pour la mise en cache est [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), qui est utilisé pour spécifier le cache *directives*. Les directives contrôlent le comportement de mise en cache comme les demandes de parvenir à partir de clients aux serveurs et que les réponses de parvenir à partir des serveurs aux clients. Déplacent des demandes et réponses via des serveurs proxy et les serveurs proxy doivent être conforme à la spécification de la mise en cache à HTTP 1.1.

Les directives `Cache-Control` courantes sont affichées dans le tableau suivant.

| Directive                                                       | Action |
| --------------------------------------------------------------- | ------ |
| [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | Un cache peut stocker la réponse. |
| [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | La réponse ne doit pas être stockée par un cache partagé. Un cache privé peut stocker et réutiliser la réponse. |
| [max-age](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | Le client n’accepte pas de réponse dont l'âge est supérieur au nombre de secondes spécifié. Exemples : `max-age=60` (60 secondes), `max-age=2592000` (1 mois) |
| [no-cache](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **Sur les demandes**: un cache ne doit pas utiliser d’une réponse stockée pour satisfaire la demande. Remarque : Le serveur d’origine génère à nouveau la réponse pour le client et l’intergiciel (middleware) met à jour la réponse stockée dans son cache.<br><br>**En cas de réponses**: la réponse ne doit pas être utilisée pour une demande ultérieure sans validation sur le serveur d’origine. |
| [non-store](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **Sur les demandes**: un cache ne doit pas stocker la demande.<br><br>**En cas de réponses**: un cache ne doit pas stocker n’importe quelle partie de la réponse. |

Les autres en-têtes de cache qui jouent un rôle dans la mise en cache sont affichés dans le tableau suivant.

| Header                                                     | Fonction |
| ---------------------------------------------------------- | -------- |
| [Âge](https://tools.ietf.org/html/rfc7234#section-5.1)     | Une estimation de la durée en secondes écoulées depuis que la réponse a été générée ou validée sur le serveur d’origine. |
| [Arrive à expiration](https://tools.ietf.org/html/rfc7234#section-5.3) | Date/heure après laquelle la réponse est considérée comme obsolète. |
| [pragma](https://tools.ietf.org/html/rfc7234#section-5.4)  | Existe pour la compatibilité descendante avec les caches HTTP/1.0 pour affecter le comportement `no-cache`. Si l'en-tête `Cache-Control` est présent, l'en-tête `Pragma` est ignoré. |
| [Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | Spécifie qu’une réponse mise en cache ne doit pas être envoyée avant que tous les champs d'en-tête `Vary` correspondent dans la demande d’origine et la nouvelle demande de la réponse mise en cache. |

## <a name="http-based-caching-respects-request-cache-control-directives"></a>La mise en cache basée sur HTTP respecte les directives Cache-Control de la demande

La [spécification de la mise en cache HTTP 1.1 pour l’en-tête Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) requiert qu'un cache respecte un en-tête `Cache-Control` valide envoyé par le client. Un client peut envoyer des demandes avec une valeur d’en-tête `no-cache` et forcer le serveur à générer une nouvelle réponse pour chaque demande.

Le fait de toujours respecter les en-têtes de demande `Cache-Control` du client a du sens si vous avez pour objectif la mise en cache HTTP. Sous la spécification officielle, la mise en cache vise à réduire la latence et la charge réseau pour satisfaire les demandes sur un réseau via les clients, les proxies et les serveurs. Ce n’est pas nécessairement un moyen de contrôler la charge sur un serveur d’origine.

Aucun contrôle n’est en cours développeur sur ce comportement de mise en cache lorsque vous utilisez le [intergiciel de mise en cache des réponses](xref:performance/caching/middleware) , car l’intergiciel (middleware) adhère à la mise en cache de la spécification officielle. [Les améliorations futures à l’intergiciel](https://github.com/aspnet/ResponseCaching/issues/96) autorise la configuration de l’intergiciel (middleware) pour ignorer une demande de `Cache-Control` en-tête lorsque vous décidez de fournir une réponse mise en cache. Cela vous proposera une opportunité pour mieux contrôler la charge sur votre serveur lorsque vous utilisez l’intergiciel (middleware).

## <a name="other-caching-technology-in-aspnet-core"></a>Autre technologie de mise en cache dans ASP.NET Core

### <a name="in-memory-caching"></a>La mise en cache en mémoire

La mise en cache utilise la mémoire serveur pour stocker les données mises en cache. Ce type de mise en cache est adapté à un ou plusieurs serveurs à l’aide de *sessions rémanentes*. Les sessions rémanentes signifient que les demandes des clients sont toujours acheminées vers le même serveur pour traitement.

Pour plus d’informations, consultez [mettre en Cache en mémoire](xref:performance/caching/memory).

### <a name="distributed-cache"></a>Cache distribué

Utilisez un cache distribué pour stocker des données en mémoire lorsque l’application est hébergée dans une batterie de serveurs ou sur le cloud. Le cache est partagé entre les serveurs qui traitent les demandes. Un client peut soumettre une demande traitée par n’importe quel serveur dans le groupe si les données mises en cache pour le client sont disponibles. ASP.NET Core offre des caches distribués Redis et SQL Server.

Pour plus d’informations, consultez [Utiliser un cache distribué](xref:performance/caching/distributed).

### <a name="cache-tag-helper"></a>Tag helper de cache

Vous pouvez mettre en cache le contenu à partir d’une vue MVC ou d'une page Razor avec Tag helper de cache. Tag helper de cache utilise la mise en cache en mémoire pour stocker les données.

Pour plus d’informations, consultez [Tag helper de cache dans ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

### <a name="distributed-cache-tag-helper"></a>Tag Helper Cache distribué

Vous pouvez mettre en cache le contenu à partir d’une vue MVC ou d'une page Razor dans des scénarios de cloud distribué ou des scénarios de batterie de serveurs web avec Tag helper de cache distribué. Tag helper de cache distribué utilise SQL Server ou Redis pour stocker les données.

Pour plus d’informations, consultez [Tag Helper Cache distribué](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).

## <a name="responsecache-attribute"></a>Attribut de ResponseCache

Le [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) spécifie les paramètres nécessaires pour définir les en-têtes appropriés dans la mise en cache de réponse.

> [!WARNING]
> Désactivez la mise en cache du contenu qui contient des informations pour les clients authentifiés. La mise en cache doit seulement être activée pour le contenu qui ne change pas selon l’identité d’un utilisateur ou si un utilisateur est connecté.

[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varie en fonction de la réponse stockée par les valeurs de la liste donnée de clés de requête. Lorsqu’une valeur unique `*` n’est fourni, le middleware varie par toutes les réponses de demandent des paramètres de chaîne de requête. `VaryByQueryKeys` nécessite ASP.NET Core 1.1 ou version ultérieure.

Le Middleware de la mise en cache de réponse doit être activé pour définir le `VaryByQueryKeys` propriété ; sinon, une exception runtime est levée. Il n’est pas un en-tête HTTP correspondant pour le `VaryByQueryKeys` propriété. La propriété est une fonctionnalité HTTP gérée par le Middleware de la mise en cache de réponse. L’intergiciel (middleware) traiter une réponse mise en cache, la chaîne de requête et la valeur de chaîne de requête doivent correspondre à une demande précédente. Par exemple, considérez la séquence de requêtes et les résultats présentés dans le tableau suivant.

| Demande                          | Résultat                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | Retourné à partir du serveur     |
| `http://example.com?key1=value1` | Retourné à partir de l’intergiciel (middleware) |
| `http://example.com?key1=value2` | Retourné à partir du serveur     |

La première requête est retournée par le serveur et mis en cache dans l’intergiciel (middleware). La deuxième demande est retournée par l’intergiciel (middleware), car la chaîne de requête correspond à la demande précédente. La troisième requête n’est pas dans le cache de l’intergiciel (middleware), car la valeur de chaîne de requête ne correspond pas à une demande précédente. 

Le `ResponseCacheAttribute` est utilisé pour configurer et créer (via `IFilterFactory`) un [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter). Le `ResponseCacheFilter` effectue le travail de mise à jour les en-têtes HTTP appropriés et les fonctionnalités de la réponse. Le filtre :

* Supprime tous les en-têtes existants pour `Vary`, `Cache-Control`, et `Pragma`. 
* Écrit les en-têtes appropriés, selon les propriétés définies dans le `ResponseCacheAttribute`. 
* Met à jour de la réponse mise en cache de la fonctionnalité HTTP si `VaryByQueryKeys` est défini.

### <a name="vary"></a>Vary

Cet en-tête est écrit uniquement lorsque la propriété `VaryByHeader` est définie. Il est défini sur la valeur de la propriété `Vary`. L’exemple suivant utilise la propriété `VaryByHeader` :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

Vous pouvez afficher les en-têtes de réponse avec les outils réseau de votre navigateur. L’illustration suivante montre la sortie Edge F12 dans l'onglet **Réseau** lorsque la méthode d’action `About2` est actualisée :

![Sous l’onglet réseau de périmètre F12 sortie lorsque la méthode d’action About2 est appelée.](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a>NoStore et Location.None

`NoStore` remplace la plupart des autres propriétés. Lorsque cette propriété a la valeur `true`, l'en-tête `Cache-Control` est défini sur `no-store`. Si `Location` a la valeur `None`:

* `Cache-Control` a la valeur `no-store,no-cache`.
* `Pragma` a la valeur `no-cache`.

Si `NoStore` est `false` et `Location` est `None`, `Cache-Control` et `Pragma` sont définies sur `no-cache`.

Vous définissez généralement `NoStore` à `true` sur les pages d’erreur. Exemple :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

Il en résulte dans les en-têtes suivants :

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>Emplacement et la durée

Pour activer la mise en cache, `Duration` doit être définie sur une valeur positive et `Location` doit avoir la valeur `Any` (la valeur par défaut) ou `Client`. Dans ce cas, le `Cache-Control` en-tête est défini sur la valeur d’emplacement suivie de la `max-age` de la réponse.

> [!NOTE]
> `Location`d’options de `Any` et `Client` traduisent `Cache-Control` valeurs d’en-tête de `public` et `private`, respectivement. Comme mentionné précédemment, le paramètre `Location` à `None` définit à la fois `Cache-Control` et `Pragma` en-têtes à `no-cache`.

Vous trouverez ci-dessous un exemple montrant les en-têtes produits en définissant `Duration` et en conservant la valeur `Location` par défaut :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

Cela génère l’en-tête suivant :

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a>Profils de cache

Au lieu de répéter les paramètres `ResponseCache` sur plusieurs attributs d’action de contrôleur, les profils de cache peuvent être configurés en tant qu’options lorsque vous configurez MVC dans la méthode `ConfigureServices` dans `Startup`. Les valeurs d’un profil de cache référencé sont utilisées en tant que valeurs par défaut par l'attribut `ResponseCache` et sont remplacées par les propriétés spécifiées dans l’attribut.

Configuration d’un profil de cache :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

Faisant référence à un profil de cache :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

L'attribut `ResponseCache` peut être appliqué à la fois aux actions (méthodes) et aux contrôleurs (classes). Les attributs au niveau de la méthode remplacent les paramètres spécifiés dans les attributs au niveau de la classe.

Dans l’exemple ci-dessus, un attribut de niveau classe spécifie une durée de 30 secondes, pendant un attribut de niveau de la méthode fait référence à un profil de cache avec une durée définie sur 60 secondes.

L’en-tête qui en résulte :

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a>Ressources supplémentaires

* [Stockage des réponses dans les Caches](https://tools.ietf.org/html/rfc7234#section-3)
* [Cache-Control](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [Mettre en cache en mémoire](xref:performance/caching/memory)
* [Utiliser un cache distribué](xref:performance/caching/distributed)
* [Détecter les modifications à l’aide de jetons de modification](xref:fundamentals/change-tokens)
* [Intergiciel de mise en cache des réponses](xref:performance/caching/middleware)
* [Tag Helper de cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Tag Helper de cache distribué](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
