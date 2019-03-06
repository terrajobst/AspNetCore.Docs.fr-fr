---
title: Mise en cache de la réponse dans ASP.NET Core
author: rick-anderson
description: Découvrez comment utiliser la mise en cache de réponse pour diminuer la bande passante et améliorer les performances des applications ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 02/28/2019
uid: performance/caching/response
ms.openlocfilehash: efcf443b1487827fe6cf4d43b6dda69adf4d61fb
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345744"
---
# <a name="response-caching-in-aspnet-core"></a>Mise en cache de la réponse dans ASP.NET Core

Par [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), et [Luke Latham](https://github.com/guardrex)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

La mise en cache de la réponse réduit le nombre de demandes que le client ou le proxy fait à un serveur web. La mise en cache de la réponse réduit également la quantité de travail que le serveur web exécute pour générer une réponse. La mise en cache de la réponse est contrôlée par des en-têtes qui spécifient comment vous souhaitez que le client, le proxy et l'intergiciel (middleware) mettent en cache les réponses.

Le [ResponseCache attribut](#responsecache-attribute) participe à la définition des en-têtes, les clients peuvent honorer lors de la mise en cache des réponses de la mise en cache de réponse. [Intergiciel de mise en cache des réponses](xref:performance/caching/middleware) peut être utilisé pour le cache des réponses sur le serveur. L’intergiciel (middleware) peut utiliser <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> propriétés pour influencer le comportement de mise en cache côté serveur.

## <a name="http-based-response-caching"></a>La mise en cache de réponse HTTP

La [spécification de la mise en cache HTTP 1.1](https://tools.ietf.org/html/rfc7234) décrit le comportement des caches sur Internet. L’en-tête HTTP principal utilisé pour la mise en cache est [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), qui est utilisé pour spécifier des *directives* de cache. Les directives contrôlent le comportement de mise en cache comme les demandes de parvenir à partir de clients aux serveurs et que les réponses de parvenir à partir des serveurs aux clients. Les demandes et les réponses transitent via des serveurs proxy, qui doivent être conformes à la spécification de la mise en cache HTTP 1.1.

Les directives `Cache-Control` courantes sont affichées dans le tableau suivant.

| Directive                                                       | Action |
| --------------------------------------------------------------- | ------ |
| [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | Un cache peut stocker la réponse. |
| [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | La réponse ne doit pas être stockée par un cache partagé. Un cache privé peut stocker et réutiliser la réponse. |
| [max-age](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | Le client n’accepte pas une réponse dont âge est supérieur au nombre spécifié de secondes. Exemples : `max-age=60` (60 secondes), `max-age=2592000` (1 mois) |
| [no-cache](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **Sur les demandes**: Un cache ne doit pas utiliser une réponse stockée pour satisfaire la requête. Le serveur d’origine régénère la réponse pour le client, et l’intergiciel (middleware) met à jour la réponse stockée dans son cache.<br><br>**En cas de réponses**: La réponse ne doit pas être utilisée pour une demande ultérieure sans validation sur le serveur d’origine. |
| [no-store](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **Sur les demandes**: Un cache ne doit pas stocker la demande.<br><br>**En cas de réponses**: Un cache ne doit pas stocker n’importe quelle partie de la réponse. |

Les autres en-têtes de cache qui jouent un rôle dans la mise en cache sont affichés dans le tableau suivant.

| Header                                                     | Fonction |
| ---------------------------------------------------------- | -------- |
| [Âge](https://tools.ietf.org/html/rfc7234#section-5.1)     | Une estimation de la durée en secondes écoulées depuis que la réponse a été générée ou validée sur le serveur d’origine. |
| [Arrive à expiration](https://tools.ietf.org/html/rfc7234#section-5.3) | Heure après laquelle la réponse est considérée comme obsolète. |
| [Pragma](https://tools.ietf.org/html/rfc7234#section-5.4)  | Existe pour la compatibilité descendante avec les caches HTTP/1.0 pour affecter le comportement `no-cache`. Si l'en-tête `Cache-Control` est présent, l'en-tête `Pragma` est ignoré. |
| [Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | Spécifie qu’une réponse mise en cache ne doit pas être envoyée avant que tous les champs d'en-tête `Vary` correspondent dans la demande d’origine et la nouvelle demande de la réponse mise en cache. |

## <a name="http-based-caching-respects-request-cache-control-directives"></a>La mise en cache basée sur HTTP respecte les directives Cache-Control de la demande

La [spécification de la mise en cache HTTP 1.1 pour l’en-tête Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) requiert qu'un cache respecte un en-tête `Cache-Control` valide envoyé par le client. Un client peut envoyer des demandes avec une valeur d’en-tête `no-cache` et forcer le serveur à générer une nouvelle réponse pour chaque demande.

Le fait de toujours respecter les en-têtes de demande `Cache-Control` du client a du sens si vous avez pour objectif la mise en cache HTTP. Sous la spécification officielle, la mise en cache vise à réduire la latence et la charge réseau pour satisfaire les demandes sur un réseau via les clients, les proxies et les serveurs. Ce n’est pas nécessairement un moyen de contrôler la charge sur un serveur d’origine.

Il n’existe aucun contrôle de développeur sur ce comportement de mise en cache lors de l’utilisation la [intergiciel de mise en cache des réponses](xref:performance/caching/middleware) , car l’intergiciel (middleware) adhère à la mise en cache de la spécification officielle. [Planifié des améliorations apportées à l’intergiciel (middleware)](https://github.com/aspnet/AspNetCore/issues/2612) constituent une opportunité pour configurer l’intergiciel (middleware) pour ignorer une demande de `Cache-Control` en-tête lorsque vous décidez de fournir une réponse mise en cache. Améliorations prévues offrent une opportunité pour une meilleure charge de serveur de contrôle.

## <a name="other-caching-technology-in-aspnet-core"></a>Autre technologie de mise en cache dans ASP.NET Core

### <a name="in-memory-caching"></a>La mise en cache en mémoire

La mise en cache utilise la mémoire serveur pour stocker les données mises en cache. Ce type de mise en cache est adapté à un ou plusieurs serveurs à l’aide de *sessions rémanentes*. Les sessions rémanentes signifient que les demandes des clients sont toujours acheminées vers le même serveur pour traitement.

Pour plus d'informations, consultez <xref:performance/caching/memory>.

### <a name="distributed-cache"></a>Cache distribué

Utilisez un cache distribué pour stocker des données en mémoire lorsque l’application est hébergée dans une batterie de serveurs ou sur le cloud. Le cache est partagé entre les serveurs qui traitent les demandes. Un client peut soumettre une demande traitée par n’importe quel serveur dans le groupe si les données mises en cache pour le client sont disponibles. ASP.NET Core offre des caches distribués Redis et SQL Server.

Pour plus d'informations, consultez <xref:performance/caching/distributed>.

### <a name="cache-tag-helper"></a>Tag helper de cache

Mettre en cache le contenu à partir d’une vue MVC ou de la Page Razor avec le Tag Helper Cache. Le tag helper de cache utilise la mise en cache en mémoire pour stocker les données.

Pour plus d'informations, consultez <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>.

### <a name="distributed-cache-tag-helper"></a>Tag Helper Cache distribué

Mettre en cache le contenu à partir d’une vue MVC ou de la Page Razor dans les scénarios de batterie de serveurs web ou de cloud distribués avec le Tag Helper Cache distribué. Le tag helper de cache distribué utilise SQL Server ou Redis pour stocker les données.

Pour plus d'informations, consultez <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>.

## <a name="responsecache-attribute"></a>Attribut de ResponseCache

Le <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> spécifie les paramètres nécessaires pour définir les en-têtes appropriés dans la mise en cache de réponse.

> [!WARNING]
> Désactivez la mise en cache du contenu qui contient des informations pour les clients authentifiés. La mise en cache doit seulement être activée pour le contenu qui ne change pas selon l’identité d’un utilisateur ou si un utilisateur est connecté.

<xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> varie en fonction de la réponse stockée par les valeurs de la liste donnée de clés de requête. Lorsqu’une valeur unique `*` n’est fourni, le middleware varie par toutes les réponses de demandent des paramètres de chaîne de requête.

[Intergiciel de mise en cache des réponses](xref:performance/caching/middleware) doit être activé pour définir le <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> propriété. Sinon, une exception runtime est levée. Il n’existe pas d’en-tête HTTP correspondant pour la propriété <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys>. La propriété est une fonctionnalité HTTP gérée par l’intergiciel de mise en cache des réponses. Pour que l’intergiciel traite une réponse mise en cache, la chaîne de requête et la valeur de la chaîne de requête doivent correspondre à une demande précédente. Par exemple, considérez la séquence des demandes et des résultats présentés dans le tableau suivant.

| Demande                          | Résultat                    |
| -------------------------------- | ------------------------- |
| `http://example.com?key1=value1` | Retourné par le serveur. |
| `http://example.com?key1=value1` | Retourné à partir de l’intergiciel (middleware). |
| `http://example.com?key1=value2` | Retourné par le serveur. |

La première requête est retournée par le serveur et mis en cache dans l’intergiciel (middleware). La deuxième demande est retournée par l’intergiciel (middleware), car la chaîne de requête correspond à la demande précédente. La troisième requête n’est pas dans le cache de l’intergiciel (middleware), car la valeur de chaîne de requête ne correspond pas à une demande précédente.

Le <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> est utilisé pour configurer et créer (via <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>) un <xref:Microsoft.AspNetCore.Mvc.Internal.ResponseCacheFilter>. Le <xref:Microsoft.AspNetCore.Mvc.Internal.ResponseCacheFilter> effectue le travail de mise à jour les en-têtes HTTP appropriés et les fonctionnalités de la réponse. Le filtre :

* Supprime tous les en-têtes existants pour `Vary`, `Cache-Control`, et `Pragma`.
* Écrit les en-têtes appropriés, selon les propriétés définies dans le <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>.
* Met à jour de la réponse mise en cache de la fonctionnalité HTTP si <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> est défini.

### <a name="vary"></a>Vary

Cet en-tête est écrit uniquement lorsque la propriété <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> est définie. La propriété définie sur la `Vary` valeur de la propriété. L’exemple suivant utilise la propriété <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> :

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache1.cshtml.cs?name=snippet)]

À l’aide de l’exemple d’application, afficher les en-têtes de réponse avec les outils de navigateur réseau. Les en-têtes de réponse suivants sont envoyés avec la réponse de page Cache1 :

```
Cache-Control: public,max-age=30
Vary: User-Agent
```

### <a name="nostore-and-locationnone"></a>NoStore et Location.None

<xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> remplace la plupart des autres propriétés. Lorsque cette propriété a la valeur `true`, l'en-tête `Cache-Control` est défini sur `no-store`. Si <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> a la valeur `None`:

* `Cache-Control` a la valeur `no-store,no-cache`.
* `Pragma` a la valeur `no-cache`.

Si <xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> est `false` et <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> est `None`, `Cache-Control`, et `Pragma` sont définies sur `no-cache`.

<xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> est généralement défini sur `true` pour les pages d’erreur. La page Cache2 dans l’exemple d’application génère des en-têtes de réponse que demandez au client de ne pas stocker la réponse.

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache2.cshtml.cs?name=snippet)]

L’exemple d’application renvoie la page Cache2 avec les en-têtes suivants :

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>Emplacement et durée

Pour activer la mise en cache, <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> doit être définie sur une valeur positive et <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> doit avoir la valeur `Any` (la valeur par défaut) ou `Client`. Dans ce cas, le `Cache-Control` en-tête est défini sur la valeur d’emplacement suivie de la `max-age` de la réponse.

> [!NOTE]
> <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>d’options de `Any` et `Client` traduisent `Cache-Control` valeurs d’en-tête de `public` et `private`, respectivement. Comme mentionné précédemment, le paramètre <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> à `None` définit à la fois `Cache-Control` et `Pragma` en-têtes à `no-cache`.

L’exemple suivant montre le modèle de page Cache3 à partir de l’exemple d’application et les en-têtes produites en affectant à <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> et en conservant la valeur par défaut <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> valeur :

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache3.cshtml.cs?name=snippet)]

L’exemple d’application renvoie la page Cache3 avec l’en-tête suivant :

```
Cache-Control: public,max-age=10
```

### <a name="cache-profiles"></a>Profils de cache

Au lieu de répéter les paramètres de cache de réponse sur nombreux attributs d’action de contrôleur, de le cache profils peuvent être configurés en tant qu’options lorsque vous configurez les Pages Razor MVC/dans `Startup.ConfigureServices`. Valeurs trouvées dans un profil de cache référencés sont utilisés en tant que les valeurs par défaut par le <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> et sont remplacées par les propriétés spécifiées sur l’attribut.

Configurer un profil de cache. L’exemple suivant montre un profil de cache de deuxième 30 dans l’exemple d’application `Startup.ConfigureServices`:

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

Références de modèle de page de l’application exemple Cache4 le `Default30` profil de cache :

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache4.cshtml.cs?name=snippet)]

Le <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> peut être appliqué à :

* Gestionnaires de Page Razor (classes) &ndash; attributs ne peut pas être appliqués aux méthodes de gestionnaire.
* Contrôleurs MVC (classes).
* Actions MVC (méthodes) &ndash; attributs au niveau de la méthode remplacent les paramètres spécifiés dans les attributs de niveau classe.

L’en-tête résultant appliqué à la réponse de page Cache4 par le `Default30` profil de cache :

```
Cache-Control: public,max-age=30
```

## <a name="additional-resources"></a>Ressources supplémentaires

* [Stockage des réponses dans les Caches](https://tools.ietf.org/html/rfc7234#section-3)
* [Cache-Control](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
