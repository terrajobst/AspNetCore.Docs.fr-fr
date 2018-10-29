---
title: Mise en cache de la réponse dans ASP.NET Core
author: rick-anderson
description: Découvrez comment utiliser la mise en cache de réponse pour diminuer la bande passante et améliorer les performances des applications ASP.NET Core.
ms.author: riande
ms.date: 09/20/2017
uid: performance/caching/response
ms.openlocfilehash: 99093cd281ffa8dddc574dc27254c0175e2651b3
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207366"
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="425a9-103">Mise en cache de la réponse dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="425a9-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="425a9-104">Par [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="425a9-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

> [!NOTE]
> <span data-ttu-id="425a9-105">Réponse mise en cache dans les Pages Razor est disponible dans ASP.NET Core 2.1 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="425a9-105">Response caching in Razor Pages is available in ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="425a9-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="425a9-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="425a9-107">La mise en cache de la réponse réduit le nombre de demandes que le client ou le proxy fait à un serveur web.</span><span class="sxs-lookup"><span data-stu-id="425a9-107">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="425a9-108">La mise en cache de la réponse réduit également la quantité de travail que le serveur web exécute pour générer une réponse.</span><span class="sxs-lookup"><span data-stu-id="425a9-108">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="425a9-109">La mise en cache de la réponse est contrôlée par des en-têtes qui spécifient comment vous souhaitez que le client, le proxy et l'intergiciel (middleware) mettent en cache les réponses.</span><span class="sxs-lookup"><span data-stu-id="425a9-109">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="425a9-110">Le serveur web peut mettre en cache les réponses lorsque vous ajoutez [l'intergiciel (middleware) de mise en cache de réponse](xref:performance/caching/middleware).</span><span class="sxs-lookup"><span data-stu-id="425a9-110">The web server can cache responses when you add [Response Caching Middleware](xref:performance/caching/middleware).</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="425a9-111">La mise en cache de réponse HTTP</span><span class="sxs-lookup"><span data-stu-id="425a9-111">HTTP-based response caching</span></span>

<span data-ttu-id="425a9-112">Le [spécification HTTP 1.1 Caching](https://tools.ietf.org/html/rfc7234) décrit le comportement de caches d’Internet.</span><span class="sxs-lookup"><span data-stu-id="425a9-112">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="425a9-113">L’en-tête HTTP principal utilisé pour la mise en cache est [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), qui est utilisé pour spécifier le cache *directives*.</span><span class="sxs-lookup"><span data-stu-id="425a9-113">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="425a9-114">Les directives contrôlent le comportement de mise en cache comme les demandes de parvenir à partir de clients aux serveurs et que les réponses de parvenir à partir des serveurs aux clients.</span><span class="sxs-lookup"><span data-stu-id="425a9-114">The directives control caching behavior as requests make their way from clients to servers and as reponses make their way from servers back to clients.</span></span> <span data-ttu-id="425a9-115">Déplacent des demandes et réponses via des serveurs proxy et les serveurs proxy doivent être conforme à la spécification de la mise en cache à HTTP 1.1.</span><span class="sxs-lookup"><span data-stu-id="425a9-115">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="425a9-116">Les directives `Cache-Control` courantes sont affichées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="425a9-116">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="425a9-117">Directive</span><span class="sxs-lookup"><span data-stu-id="425a9-117">Directive</span></span>                                                       | <span data-ttu-id="425a9-118">Action</span><span class="sxs-lookup"><span data-stu-id="425a9-118">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="425a9-119">public</span><span class="sxs-lookup"><span data-stu-id="425a9-119">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="425a9-120">Un cache peut stocker la réponse.</span><span class="sxs-lookup"><span data-stu-id="425a9-120">A cache may store the response.</span></span> |
| [<span data-ttu-id="425a9-121">private</span><span class="sxs-lookup"><span data-stu-id="425a9-121">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="425a9-122">La réponse ne doit pas être stockée par un cache partagé.</span><span class="sxs-lookup"><span data-stu-id="425a9-122">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="425a9-123">Un cache privé peut stocker et réutiliser la réponse.</span><span class="sxs-lookup"><span data-stu-id="425a9-123">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="425a9-124">max-age</span><span class="sxs-lookup"><span data-stu-id="425a9-124">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="425a9-125">Le client n’accepte pas de réponse dont l'âge est supérieur au nombre de secondes spécifié.</span><span class="sxs-lookup"><span data-stu-id="425a9-125">The client won't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="425a9-126">Exemples : `max-age=60` (60 secondes), `max-age=2592000` (1 mois)</span><span class="sxs-lookup"><span data-stu-id="425a9-126">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="425a9-127">no-cache</span><span class="sxs-lookup"><span data-stu-id="425a9-127">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="425a9-128">**Sur les demandes**: un cache ne doit pas utiliser d’une réponse stockée pour satisfaire la demande.</span><span class="sxs-lookup"><span data-stu-id="425a9-128">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="425a9-129">Remarque : Le serveur d’origine génère à nouveau la réponse pour le client et l’intergiciel (middleware) met à jour la réponse stockée dans son cache.</span><span class="sxs-lookup"><span data-stu-id="425a9-129">Note: The origin server re-generates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="425a9-130">**En cas de réponses**: la réponse ne doit pas être utilisée pour une demande ultérieure sans validation sur le serveur d’origine.</span><span class="sxs-lookup"><span data-stu-id="425a9-130">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="425a9-131">non-store</span><span class="sxs-lookup"><span data-stu-id="425a9-131">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="425a9-132">**Sur les demandes**: un cache ne doit pas stocker la demande.</span><span class="sxs-lookup"><span data-stu-id="425a9-132">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="425a9-133">**En cas de réponses**: un cache ne doit pas stocker n’importe quelle partie de la réponse.</span><span class="sxs-lookup"><span data-stu-id="425a9-133">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="425a9-134">Les autres en-têtes de cache qui jouent un rôle dans la mise en cache sont affichés dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="425a9-134">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="425a9-135">Header</span><span class="sxs-lookup"><span data-stu-id="425a9-135">Header</span></span>                                                     | <span data-ttu-id="425a9-136">Fonction</span><span class="sxs-lookup"><span data-stu-id="425a9-136">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="425a9-137">Âge</span><span class="sxs-lookup"><span data-stu-id="425a9-137">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="425a9-138">Une estimation de la durée en secondes écoulées depuis que la réponse a été générée ou validée sur le serveur d’origine.</span><span class="sxs-lookup"><span data-stu-id="425a9-138">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="425a9-139">Arrive à expiration</span><span class="sxs-lookup"><span data-stu-id="425a9-139">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="425a9-140">Date/heure après laquelle la réponse est considérée comme obsolète.</span><span class="sxs-lookup"><span data-stu-id="425a9-140">The date/time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="425a9-141">pragma</span><span class="sxs-lookup"><span data-stu-id="425a9-141">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="425a9-142">Existe pour la compatibilité descendante avec les caches HTTP/1.0 pour affecter le comportement `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="425a9-142">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="425a9-143">Si l'en-tête `Cache-Control` est présent, l'en-tête `Pragma` est ignoré.</span><span class="sxs-lookup"><span data-stu-id="425a9-143">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="425a9-144">Vary</span><span class="sxs-lookup"><span data-stu-id="425a9-144">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="425a9-145">Spécifie qu’une réponse mise en cache ne doit pas être envoyée avant que tous les champs d'en-tête `Vary` correspondent dans la demande d’origine et la nouvelle demande de la réponse mise en cache.</span><span class="sxs-lookup"><span data-stu-id="425a9-145">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="425a9-146">La mise en cache basée sur HTTP respecte les directives Cache-Control de la demande</span><span class="sxs-lookup"><span data-stu-id="425a9-146">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="425a9-147">La [spécification de la mise en cache HTTP 1.1 pour l’en-tête Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) requiert qu'un cache respecte un en-tête `Cache-Control` valide envoyé par le client.</span><span class="sxs-lookup"><span data-stu-id="425a9-147">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="425a9-148">Un client peut envoyer des demandes avec une valeur d’en-tête `no-cache` et forcer le serveur à générer une nouvelle réponse pour chaque demande.</span><span class="sxs-lookup"><span data-stu-id="425a9-148">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="425a9-149">Le fait de toujours respecter les en-têtes de demande `Cache-Control` du client a du sens si vous avez pour objectif la mise en cache HTTP.</span><span class="sxs-lookup"><span data-stu-id="425a9-149">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="425a9-150">Sous la spécification officielle, la mise en cache vise à réduire la latence et la charge réseau pour satisfaire les demandes sur un réseau via les clients, les proxies et les serveurs.</span><span class="sxs-lookup"><span data-stu-id="425a9-150">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="425a9-151">Ce n’est pas nécessairement un moyen de contrôler la charge sur un serveur d’origine.</span><span class="sxs-lookup"><span data-stu-id="425a9-151">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="425a9-152">Aucun contrôle n’est en cours développeur sur ce comportement de mise en cache lorsque vous utilisez le [intergiciel de mise en cache des réponses](xref:performance/caching/middleware) , car l’intergiciel (middleware) adhère à la mise en cache de la spécification officielle.</span><span class="sxs-lookup"><span data-stu-id="425a9-152">There's no current developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="425a9-153">[Les améliorations futures à l’intergiciel](https://github.com/aspnet/ResponseCaching/issues/96) autorise la configuration de l’intergiciel (middleware) pour ignorer une demande de `Cache-Control` en-tête lorsque vous décidez de fournir une réponse mise en cache.</span><span class="sxs-lookup"><span data-stu-id="425a9-153">[Future enhancements to the middleware](https://github.com/aspnet/ResponseCaching/issues/96) will permit configuring the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="425a9-154">Cela vous proposera une opportunité pour mieux contrôler la charge sur votre serveur lorsque vous utilisez l’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="425a9-154">This will offer you an opportunity to better control the load on your server when you use the middleware.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="425a9-155">Autre technologie de mise en cache dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="425a9-155">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="425a9-156">La mise en cache en mémoire</span><span class="sxs-lookup"><span data-stu-id="425a9-156">In-memory caching</span></span>

<span data-ttu-id="425a9-157">La mise en cache utilise la mémoire serveur pour stocker les données mises en cache.</span><span class="sxs-lookup"><span data-stu-id="425a9-157">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="425a9-158">Ce type de mise en cache est adapté à un ou plusieurs serveurs à l’aide de *sessions rémanentes*.</span><span class="sxs-lookup"><span data-stu-id="425a9-158">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="425a9-159">Les sessions rémanentes signifient que les demandes des clients sont toujours acheminées vers le même serveur pour traitement.</span><span class="sxs-lookup"><span data-stu-id="425a9-159">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="425a9-160">Pour plus d’informations, consultez [mettre en Cache en mémoire](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="425a9-160">For more information, see [Cache in-memory](xref:performance/caching/memory).</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="425a9-161">Cache distribué</span><span class="sxs-lookup"><span data-stu-id="425a9-161">Distributed Cache</span></span>

<span data-ttu-id="425a9-162">Utilisez un cache distribué pour stocker des données en mémoire lorsque l’application est hébergée dans une batterie de serveurs ou sur le cloud.</span><span class="sxs-lookup"><span data-stu-id="425a9-162">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="425a9-163">Le cache est partagé entre les serveurs qui traitent les demandes.</span><span class="sxs-lookup"><span data-stu-id="425a9-163">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="425a9-164">Un client peut soumettre une demande traitée par n’importe quel serveur dans le groupe si les données mises en cache pour le client sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="425a9-164">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="425a9-165">ASP.NET Core offre des caches distribués Redis et SQL Server.</span><span class="sxs-lookup"><span data-stu-id="425a9-165">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="425a9-166">Pour plus d'informations, consultez <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="425a9-166">For more information, see <xref:performance/caching/distributed>.</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="425a9-167">Tag helper de cache</span><span class="sxs-lookup"><span data-stu-id="425a9-167">Cache Tag Helper</span></span>

<span data-ttu-id="425a9-168">Vous pouvez mettre en cache le contenu à partir d’une vue MVC ou d'une page Razor avec Tag helper de cache.</span><span class="sxs-lookup"><span data-stu-id="425a9-168">You can cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="425a9-169">Tag helper de cache utilise la mise en cache en mémoire pour stocker les données.</span><span class="sxs-lookup"><span data-stu-id="425a9-169">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="425a9-170">Pour plus d’informations, consultez [Tag helper de cache dans ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="425a9-170">For more information, see [Cache Tag Helper in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="425a9-171">Tag Helper Cache distribué</span><span class="sxs-lookup"><span data-stu-id="425a9-171">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="425a9-172">Vous pouvez mettre en cache le contenu à partir d’une vue MVC ou d'une page Razor dans des scénarios de cloud distribué ou des scénarios de batterie de serveurs web avec Tag helper de cache distribué.</span><span class="sxs-lookup"><span data-stu-id="425a9-172">You can cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="425a9-173">Tag helper de cache distribué utilise SQL Server ou Redis pour stocker les données.</span><span class="sxs-lookup"><span data-stu-id="425a9-173">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="425a9-174">Pour plus d’informations, consultez [Tag Helper Cache distribué](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="425a9-174">For more information, see [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="425a9-175">Attribut de ResponseCache</span><span class="sxs-lookup"><span data-stu-id="425a9-175">ResponseCache attribute</span></span>

<span data-ttu-id="425a9-176">Le [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) spécifie les paramètres nécessaires pour définir les en-têtes appropriés dans la mise en cache de réponse.</span><span class="sxs-lookup"><span data-stu-id="425a9-176">The [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="425a9-177">Désactivez la mise en cache du contenu qui contient des informations pour les clients authentifiés.</span><span class="sxs-lookup"><span data-stu-id="425a9-177">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="425a9-178">La mise en cache doit seulement être activée pour le contenu qui ne change pas selon l’identité d’un utilisateur ou si un utilisateur est connecté.</span><span class="sxs-lookup"><span data-stu-id="425a9-178">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="425a9-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varie en fonction de la réponse stockée par les valeurs de la liste donnée de clés de requête.</span><span class="sxs-lookup"><span data-stu-id="425a9-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="425a9-180">Lorsqu’une valeur unique `*` n’est fourni, le middleware varie par toutes les réponses de demandent des paramètres de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="425a9-180">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span> <span data-ttu-id="425a9-181">`VaryByQueryKeys` nécessite ASP.NET Core 1.1 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="425a9-181">`VaryByQueryKeys` requires ASP.NET Core 1.1 or later.</span></span>

<span data-ttu-id="425a9-182">Le Middleware de la mise en cache de réponse doit être activé pour définir le `VaryByQueryKeys` propriété ; sinon, une exception runtime est levée.</span><span class="sxs-lookup"><span data-stu-id="425a9-182">The Response Caching Middleware must be enabled to set the `VaryByQueryKeys` property; otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="425a9-183">Il n’est pas un en-tête HTTP correspondant pour le `VaryByQueryKeys` propriété.</span><span class="sxs-lookup"><span data-stu-id="425a9-183">There isn't a corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="425a9-184">La propriété est une fonctionnalité HTTP gérée par le Middleware de la mise en cache de réponse.</span><span class="sxs-lookup"><span data-stu-id="425a9-184">The property is an HTTP feature handled by the Response Caching Middleware.</span></span> <span data-ttu-id="425a9-185">L’intergiciel (middleware) traiter une réponse mise en cache, la chaîne de requête et la valeur de chaîne de requête doivent correspondre à une demande précédente.</span><span class="sxs-lookup"><span data-stu-id="425a9-185">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="425a9-186">Par exemple, considérez la séquence de requêtes et les résultats présentés dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="425a9-186">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="425a9-187">Demande</span><span class="sxs-lookup"><span data-stu-id="425a9-187">Request</span></span>                          | <span data-ttu-id="425a9-188">Résultat</span><span class="sxs-lookup"><span data-stu-id="425a9-188">Result</span></span>                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | <span data-ttu-id="425a9-189">Retourné à partir du serveur</span><span class="sxs-lookup"><span data-stu-id="425a9-189">Returned from server</span></span>     |
| `http://example.com?key1=value1` | <span data-ttu-id="425a9-190">Retourné à partir de l’intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="425a9-190">Returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="425a9-191">Retourné à partir du serveur</span><span class="sxs-lookup"><span data-stu-id="425a9-191">Returned from server</span></span>     |

<span data-ttu-id="425a9-192">La première requête est retournée par le serveur et mis en cache dans l’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="425a9-192">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="425a9-193">La deuxième demande est retournée par l’intergiciel (middleware), car la chaîne de requête correspond à la demande précédente.</span><span class="sxs-lookup"><span data-stu-id="425a9-193">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="425a9-194">La troisième requête n’est pas dans le cache de l’intergiciel (middleware), car la valeur de chaîne de requête ne correspond pas à une demande précédente.</span><span class="sxs-lookup"><span data-stu-id="425a9-194">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="425a9-195">Le `ResponseCacheAttribute` est utilisé pour configurer et créer (via `IFilterFactory`) un [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span><span class="sxs-lookup"><span data-stu-id="425a9-195">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span></span> <span data-ttu-id="425a9-196">Le `ResponseCacheFilter` effectue le travail de mise à jour les en-têtes HTTP appropriés et les fonctionnalités de la réponse.</span><span class="sxs-lookup"><span data-stu-id="425a9-196">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="425a9-197">Le filtre :</span><span class="sxs-lookup"><span data-stu-id="425a9-197">The filter:</span></span>

* <span data-ttu-id="425a9-198">Supprime tous les en-têtes existants pour `Vary`, `Cache-Control`, et `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="425a9-198">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="425a9-199">Écrit les en-têtes appropriés, selon les propriétés définies dans le `ResponseCacheAttribute`.</span><span class="sxs-lookup"><span data-stu-id="425a9-199">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="425a9-200">Met à jour de la réponse mise en cache de la fonctionnalité HTTP si `VaryByQueryKeys` est défini.</span><span class="sxs-lookup"><span data-stu-id="425a9-200">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="425a9-201">Vary</span><span class="sxs-lookup"><span data-stu-id="425a9-201">Vary</span></span>

<span data-ttu-id="425a9-202">Cet en-tête est écrit uniquement lorsque la propriété `VaryByHeader` est définie.</span><span class="sxs-lookup"><span data-stu-id="425a9-202">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="425a9-203">Il est défini sur la valeur de la propriété `Vary`.</span><span class="sxs-lookup"><span data-stu-id="425a9-203">It's set to the `Vary` property's value.</span></span> <span data-ttu-id="425a9-204">L’exemple suivant utilise la propriété `VaryByHeader` :</span><span class="sxs-lookup"><span data-stu-id="425a9-204">The following sample uses the `VaryByHeader` property:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

<span data-ttu-id="425a9-205">Vous pouvez afficher les en-têtes de réponse avec les outils réseau de votre navigateur.</span><span class="sxs-lookup"><span data-stu-id="425a9-205">You can view the response headers with your browser's network tools.</span></span> <span data-ttu-id="425a9-206">L’illustration suivante montre la sortie Edge F12 dans l'onglet **Réseau** lorsque la méthode d’action `About2` est actualisée :</span><span class="sxs-lookup"><span data-stu-id="425a9-206">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed:</span></span>

![Sous l’onglet réseau de périmètre F12 sortie lorsque la méthode d’action About2 est appelée.](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="425a9-208">NoStore et Location.None</span><span class="sxs-lookup"><span data-stu-id="425a9-208">NoStore and Location.None</span></span>

<span data-ttu-id="425a9-209">`NoStore` remplace la plupart des autres propriétés.</span><span class="sxs-lookup"><span data-stu-id="425a9-209">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="425a9-210">Lorsque cette propriété a la valeur `true`, l'en-tête `Cache-Control` est défini sur `no-store`.</span><span class="sxs-lookup"><span data-stu-id="425a9-210">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="425a9-211">Si `Location` a la valeur `None`:</span><span class="sxs-lookup"><span data-stu-id="425a9-211">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="425a9-212">`Cache-Control` a la valeur `no-store,no-cache`.</span><span class="sxs-lookup"><span data-stu-id="425a9-212">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="425a9-213">`Pragma` a la valeur `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="425a9-213">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="425a9-214">Si `NoStore` est `false` et `Location` est `None`, `Cache-Control` et `Pragma` sont définies sur `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="425a9-214">If `NoStore` is `false` and `Location` is `None`, `Cache-Control` and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="425a9-215">Vous définissez généralement `NoStore` à `true` sur les pages d’erreur.</span><span class="sxs-lookup"><span data-stu-id="425a9-215">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="425a9-216">Exemple :</span><span class="sxs-lookup"><span data-stu-id="425a9-216">For example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

<span data-ttu-id="425a9-217">Il en résulte dans les en-têtes suivants :</span><span class="sxs-lookup"><span data-stu-id="425a9-217">This results in the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="425a9-218">Emplacement et la durée</span><span class="sxs-lookup"><span data-stu-id="425a9-218">Location and Duration</span></span>

<span data-ttu-id="425a9-219">Pour activer la mise en cache, `Duration` doit être définie sur une valeur positive et `Location` doit avoir la valeur `Any` (la valeur par défaut) ou `Client`.</span><span class="sxs-lookup"><span data-stu-id="425a9-219">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="425a9-220">Dans ce cas, le `Cache-Control` en-tête est défini sur la valeur d’emplacement suivie de la `max-age` de la réponse.</span><span class="sxs-lookup"><span data-stu-id="425a9-220">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="425a9-221">`Location`d’options de `Any` et `Client` traduisent `Cache-Control` valeurs d’en-tête de `public` et `private`, respectivement.</span><span class="sxs-lookup"><span data-stu-id="425a9-221">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="425a9-222">Comme mentionné précédemment, le paramètre `Location` à `None` définit à la fois `Cache-Control` et `Pragma` en-têtes à `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="425a9-222">As noted previously, setting `Location` to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="425a9-223">Vous trouverez ci-dessous un exemple montrant les en-têtes produits en définissant `Duration` et en conservant la valeur `Location` par défaut :</span><span class="sxs-lookup"><span data-stu-id="425a9-223">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

<span data-ttu-id="425a9-224">Cela génère l’en-tête suivant :</span><span class="sxs-lookup"><span data-stu-id="425a9-224">This produces the following header:</span></span>

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a><span data-ttu-id="425a9-225">Profils de cache</span><span class="sxs-lookup"><span data-stu-id="425a9-225">Cache profiles</span></span>

<span data-ttu-id="425a9-226">Au lieu de répéter les paramètres `ResponseCache` sur plusieurs attributs d’action de contrôleur, les profils de cache peuvent être configurés en tant qu’options lorsque vous configurez MVC dans la méthode `ConfigureServices` dans `Startup`.</span><span class="sxs-lookup"><span data-stu-id="425a9-226">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="425a9-227">Les valeurs d’un profil de cache référencé sont utilisées en tant que valeurs par défaut par l'attribut `ResponseCache` et sont remplacées par les propriétés spécifiées dans l’attribut.</span><span class="sxs-lookup"><span data-stu-id="425a9-227">Values found in a referenced cache profile are used as the defaults by the `ResponseCache` attribute and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="425a9-228">Configuration d’un profil de cache :</span><span class="sxs-lookup"><span data-stu-id="425a9-228">Setting up a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="425a9-229">Faisant référence à un profil de cache :</span><span class="sxs-lookup"><span data-stu-id="425a9-229">Referencing a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

<span data-ttu-id="425a9-230">L'attribut `ResponseCache` peut être appliqué à la fois aux actions (méthodes) et aux contrôleurs (classes).</span><span class="sxs-lookup"><span data-stu-id="425a9-230">The `ResponseCache` attribute can be applied both to actions (methods) and controllers (classes).</span></span> <span data-ttu-id="425a9-231">Les attributs au niveau de la méthode remplacent les paramètres spécifiés dans les attributs au niveau de la classe.</span><span class="sxs-lookup"><span data-stu-id="425a9-231">Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="425a9-232">Dans l’exemple ci-dessus, un attribut de niveau classe spécifie une durée de 30 secondes, pendant un attribut de niveau de la méthode fait référence à un profil de cache avec une durée définie sur 60 secondes.</span><span class="sxs-lookup"><span data-stu-id="425a9-232">In the above example, a class-level attribute specifies a duration of 30 seconds, while a method-level attribute references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="425a9-233">L’en-tête qui en résulte :</span><span class="sxs-lookup"><span data-stu-id="425a9-233">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a><span data-ttu-id="425a9-234">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="425a9-234">Additional resources</span></span>

* [<span data-ttu-id="425a9-235">Stockage des réponses dans les Caches</span><span class="sxs-lookup"><span data-stu-id="425a9-235">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="425a9-236">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="425a9-236">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
