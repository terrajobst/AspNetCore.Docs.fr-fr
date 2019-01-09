---
title: Mise en cache de la réponse dans ASP.NET Core
author: rick-anderson
description: Découvrez comment utiliser la mise en cache de réponse pour diminuer la bande passante et améliorer les performances des applications ASP.NET Core.
ms.author: riande
ms.date: 01/07/2018
uid: performance/caching/response
ms.openlocfilehash: 5fbcaddff6e53d01a19ba8a7455c719feb614326
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098946"
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="70da8-103">Mise en cache de la réponse dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="70da8-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="70da8-104">Par [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="70da8-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

> [!NOTE]
> <span data-ttu-id="70da8-105">La mise en cache de la réponse dans les Pages Razor est disponible dans ASP.NET Core 2.1 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="70da8-105">Response caching in Razor Pages is available in ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="70da8-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="70da8-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="70da8-107">La mise en cache de la réponse réduit le nombre de demandes que le client ou le proxy fait à un serveur web.</span><span class="sxs-lookup"><span data-stu-id="70da8-107">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="70da8-108">La mise en cache de la réponse réduit également la quantité de travail que le serveur web exécute pour générer une réponse.</span><span class="sxs-lookup"><span data-stu-id="70da8-108">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="70da8-109">La mise en cache de la réponse est contrôlée par des en-têtes qui spécifient comment vous souhaitez que le client, le proxy et l'intergiciel (middleware) mettent en cache les réponses.</span><span class="sxs-lookup"><span data-stu-id="70da8-109">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="70da8-110">Le [ResponseCache attribut](#responsecache-attribute) participe à la définition des en-têtes, les clients peuvent honorer lors de la mise en cache des réponses de la mise en cache de réponse.</span><span class="sxs-lookup"><span data-stu-id="70da8-110">The [ResponseCache attribute](#responsecache-attribute) participates in setting response caching headers, which clients may honor when caching responses.</span></span> <span data-ttu-id="70da8-111">[Intergiciel de mise en cache des réponses](xref:performance/caching/middleware) peut être utilisé pour le cache des réponses sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="70da8-111">[Response Caching Middleware](xref:performance/caching/middleware) can be used to cache responses on the server.</span></span> <span data-ttu-id="70da8-112">L’intergiciel (middleware) peut utiliser `ResponseCache` attribut des propriétés pour influencer le comportement de mise en cache côté serveur.</span><span class="sxs-lookup"><span data-stu-id="70da8-112">The middleware can use `ResponseCache` attribute properties to influence server-side caching behavior.</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="70da8-113">La mise en cache de réponse HTTP</span><span class="sxs-lookup"><span data-stu-id="70da8-113">HTTP-based response caching</span></span>

<span data-ttu-id="70da8-114">La [spécification de la mise en cache HTTP 1.1](https://tools.ietf.org/html/rfc7234) décrit le comportement des caches sur Internet.</span><span class="sxs-lookup"><span data-stu-id="70da8-114">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="70da8-115">L’en-tête HTTP principal utilisé pour la mise en cache est [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), qui est utilisé pour spécifier des *directives* de cache.</span><span class="sxs-lookup"><span data-stu-id="70da8-115">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="70da8-116"> Les directives contrôlent le comportement de mise en cache quand des demandes transitent des clients vers les serveurs et quand des réponses transitent des serveurs vers les clients.</span><span class="sxs-lookup"><span data-stu-id="70da8-116">The directives control caching behavior as requests make their way from clients to servers and as reponses make their way from servers back to clients.</span></span> <span data-ttu-id="70da8-117">Les demandes et les réponses transitent via des serveurs proxy, qui doivent être conformes à la spécification de la mise en cache HTTP 1.1.</span><span class="sxs-lookup"><span data-stu-id="70da8-117">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="70da8-118">Les directives `Cache-Control` courantes sont affichées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="70da8-118">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="70da8-119">Directive</span><span class="sxs-lookup"><span data-stu-id="70da8-119">Directive</span></span>                                                       | <span data-ttu-id="70da8-120">Action</span><span class="sxs-lookup"><span data-stu-id="70da8-120">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="70da8-121">public</span><span class="sxs-lookup"><span data-stu-id="70da8-121">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="70da8-122">Un cache peut stocker la réponse.</span><span class="sxs-lookup"><span data-stu-id="70da8-122">A cache may store the response.</span></span> |
| [<span data-ttu-id="70da8-123">private</span><span class="sxs-lookup"><span data-stu-id="70da8-123">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="70da8-124">La réponse ne doit pas être stockée par un cache partagé.</span><span class="sxs-lookup"><span data-stu-id="70da8-124">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="70da8-125">Un cache privé peut stocker et réutiliser la réponse.</span><span class="sxs-lookup"><span data-stu-id="70da8-125">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="70da8-126">max-age</span><span class="sxs-lookup"><span data-stu-id="70da8-126">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="70da8-127">Le client n’accepte pas de réponse dont l'âge est supérieur au nombre de secondes spécifié.</span><span class="sxs-lookup"><span data-stu-id="70da8-127">The client won't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="70da8-128">Exemples : `max-age=60` (60 secondes), `max-age=2592000` (1 mois)</span><span class="sxs-lookup"><span data-stu-id="70da8-128">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="70da8-129">no-cache</span><span class="sxs-lookup"><span data-stu-id="70da8-129">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="70da8-130">**Sur les demandes**: Un cache ne doit pas utiliser une réponse stockée pour satisfaire la requête.</span><span class="sxs-lookup"><span data-stu-id="70da8-130">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="70da8-131">Remarque : Le serveur d’origine génère à nouveau la réponse pour le client, et l’intergiciel (middleware) met à jour la réponse stockée dans son cache.</span><span class="sxs-lookup"><span data-stu-id="70da8-131">Note: The origin server re-generates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="70da8-132">**En cas de réponses**: La réponse ne doit pas être utilisée pour une demande ultérieure sans validation sur le serveur d’origine.</span><span class="sxs-lookup"><span data-stu-id="70da8-132">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="70da8-133">non-store</span><span class="sxs-lookup"><span data-stu-id="70da8-133">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="70da8-134">**Sur les demandes**: Un cache ne doit pas stocker la demande.</span><span class="sxs-lookup"><span data-stu-id="70da8-134">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="70da8-135">**En cas de réponses**: Un cache ne doit pas stocker n’importe quelle partie de la réponse.</span><span class="sxs-lookup"><span data-stu-id="70da8-135">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="70da8-136">Les autres en-têtes de cache qui jouent un rôle dans la mise en cache sont affichés dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="70da8-136">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="70da8-137">Header</span><span class="sxs-lookup"><span data-stu-id="70da8-137">Header</span></span>                                                     | <span data-ttu-id="70da8-138">Fonction</span><span class="sxs-lookup"><span data-stu-id="70da8-138">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="70da8-139">Âge</span><span class="sxs-lookup"><span data-stu-id="70da8-139">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="70da8-140">Une estimation de la durée en secondes écoulées depuis que la réponse a été générée ou validée sur le serveur d’origine.</span><span class="sxs-lookup"><span data-stu-id="70da8-140">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="70da8-141">Arrive à expiration</span><span class="sxs-lookup"><span data-stu-id="70da8-141">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="70da8-142">Date/heure après laquelle la réponse est considérée comme obsolète.</span><span class="sxs-lookup"><span data-stu-id="70da8-142">The date/time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="70da8-143">pragma</span><span class="sxs-lookup"><span data-stu-id="70da8-143">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="70da8-144">Existe pour la compatibilité descendante avec les caches HTTP/1.0 pour affecter le comportement `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="70da8-144">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="70da8-145">Si l'en-tête `Cache-Control` est présent, l'en-tête `Pragma` est ignoré.</span><span class="sxs-lookup"><span data-stu-id="70da8-145">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="70da8-146">Vary</span><span class="sxs-lookup"><span data-stu-id="70da8-146">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="70da8-147">Spécifie qu’une réponse mise en cache ne doit pas être envoyée avant que tous les champs d'en-tête `Vary` correspondent dans la demande d’origine et la nouvelle demande de la réponse mise en cache.</span><span class="sxs-lookup"><span data-stu-id="70da8-147">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="70da8-148">La mise en cache basée sur HTTP respecte les directives Cache-Control de la demande</span><span class="sxs-lookup"><span data-stu-id="70da8-148">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="70da8-149">La [spécification de la mise en cache HTTP 1.1 pour l’en-tête Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) requiert qu'un cache respecte un en-tête `Cache-Control` valide envoyé par le client.</span><span class="sxs-lookup"><span data-stu-id="70da8-149">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="70da8-150">Un client peut envoyer des demandes avec une valeur d’en-tête `no-cache` et forcer le serveur à générer une nouvelle réponse pour chaque demande.</span><span class="sxs-lookup"><span data-stu-id="70da8-150">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="70da8-151">Le fait de toujours respecter les en-têtes de demande `Cache-Control` du client a du sens si vous avez pour objectif la mise en cache HTTP.</span><span class="sxs-lookup"><span data-stu-id="70da8-151">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="70da8-152">Sous la spécification officielle, la mise en cache vise à réduire la latence et la charge réseau pour satisfaire les demandes sur un réseau via les clients, les proxies et les serveurs.</span><span class="sxs-lookup"><span data-stu-id="70da8-152">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="70da8-153">Ce n’est pas nécessairement un moyen de contrôler la charge sur un serveur d’origine.</span><span class="sxs-lookup"><span data-stu-id="70da8-153">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="70da8-154">Il n’existe aucun contrôle de développeur sur ce comportement de mise en cache lors de l’utilisation la [intergiciel de mise en cache des réponses](xref:performance/caching/middleware) , car l’intergiciel (middleware) adhère à la mise en cache de la spécification officielle.</span><span class="sxs-lookup"><span data-stu-id="70da8-154">There's no developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="70da8-155">[Planifié des améliorations apportées à l’intergiciel (middleware)](https://github.com/aspnet/AspNetCore/issues/2612) constituent une opportunité pour configurer l’intergiciel (middleware) pour ignorer une demande de `Cache-Control` en-tête lorsque vous décidez de fournir une réponse mise en cache.</span><span class="sxs-lookup"><span data-stu-id="70da8-155">[Planned enhancements to the middleware](https://github.com/aspnet/AspNetCore/issues/2612) are an opportunity to configure the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="70da8-156">Améliorations prévues offrent une opportunité pour une meilleure charge de serveur de contrôle.</span><span class="sxs-lookup"><span data-stu-id="70da8-156">Planned enhancements provide an opportunity to better control server load.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="70da8-157">Autre technologie de mise en cache dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="70da8-157">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="70da8-158">La mise en cache en mémoire</span><span class="sxs-lookup"><span data-stu-id="70da8-158">In-memory caching</span></span>

<span data-ttu-id="70da8-159">La mise en cache utilise la mémoire serveur pour stocker les données mises en cache.</span><span class="sxs-lookup"><span data-stu-id="70da8-159">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="70da8-160">Ce type de mise en cache est adapté à un ou plusieurs serveurs à l’aide de *sessions rémanentes*.</span><span class="sxs-lookup"><span data-stu-id="70da8-160">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="70da8-161">Les sessions rémanentes signifient que les demandes des clients sont toujours acheminées vers le même serveur pour traitement.</span><span class="sxs-lookup"><span data-stu-id="70da8-161">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="70da8-162">Pour plus d’informations, consultez [mettre en Cache en mémoire](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="70da8-162">For more information, see [Cache in-memory](xref:performance/caching/memory).</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="70da8-163">Cache distribué</span><span class="sxs-lookup"><span data-stu-id="70da8-163">Distributed Cache</span></span>

<span data-ttu-id="70da8-164">Utilisez un cache distribué pour stocker des données en mémoire lorsque l’application est hébergée dans une batterie de serveurs ou sur le cloud.</span><span class="sxs-lookup"><span data-stu-id="70da8-164">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="70da8-165">Le cache est partagé entre les serveurs qui traitent les demandes.</span><span class="sxs-lookup"><span data-stu-id="70da8-165">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="70da8-166">Un client peut soumettre une demande traitée par n’importe quel serveur dans le groupe si les données mises en cache pour le client sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="70da8-166">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="70da8-167">ASP.NET Core offre des caches distribués Redis et SQL Server.</span><span class="sxs-lookup"><span data-stu-id="70da8-167">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="70da8-168">Pour plus d'informations, consultez <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="70da8-168">For more information, see <xref:performance/caching/distributed>.</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="70da8-169">Tag helper de cache</span><span class="sxs-lookup"><span data-stu-id="70da8-169">Cache Tag Helper</span></span>

<span data-ttu-id="70da8-170">Vous pouvez mettre en cache le contenu à partir d’une vue MVC ou d'une page Razor avec Tag helper de cache.</span><span class="sxs-lookup"><span data-stu-id="70da8-170">You can cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="70da8-171">Le tag helper de cache utilise la mise en cache en mémoire pour stocker les données.</span><span class="sxs-lookup"><span data-stu-id="70da8-171">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="70da8-172">Pour plus d’informations, consultez [Tag helper de cache dans ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="70da8-172">For more information, see [Cache Tag Helper in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="70da8-173">Tag Helper Cache distribué</span><span class="sxs-lookup"><span data-stu-id="70da8-173">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="70da8-174">Vous pouvez mettre en cache le contenu à partir d’une vue MVC ou d'une page Razor dans des scénarios de cloud distribué ou des scénarios de batterie de serveurs web avec le tag helper de cache distribué.</span><span class="sxs-lookup"><span data-stu-id="70da8-174">You can cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="70da8-175">Le tag helper de cache distribué utilise SQL Server ou Redis pour stocker les données.</span><span class="sxs-lookup"><span data-stu-id="70da8-175">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="70da8-176">Pour plus d’informations, consultez [Tag Helper Cache distribué](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="70da8-176">For more information, see [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="70da8-177">Attribut de ResponseCache</span><span class="sxs-lookup"><span data-stu-id="70da8-177">ResponseCache attribute</span></span>

<span data-ttu-id="70da8-178">Le [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) spécifie les paramètres nécessaires pour définir les en-têtes appropriés dans la mise en cache de réponse.</span><span class="sxs-lookup"><span data-stu-id="70da8-178">The [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="70da8-179">Désactivez la mise en cache du contenu qui contient des informations pour les clients authentifiés.</span><span class="sxs-lookup"><span data-stu-id="70da8-179">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="70da8-180">La mise en cache doit seulement être activée pour le contenu qui ne change pas selon l’identité d’un utilisateur ou si un utilisateur est connecté.</span><span class="sxs-lookup"><span data-stu-id="70da8-180">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="70da8-181">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varie en fonction de la réponse stockée par les valeurs de la liste donnée de clés de requête.</span><span class="sxs-lookup"><span data-stu-id="70da8-181">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="70da8-182">Lorsqu’une valeur unique `*` n’est fourni, le middleware varie par toutes les réponses de demandent des paramètres de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="70da8-182">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span> <span data-ttu-id="70da8-183">`VaryByQueryKeys` nécessite ASP.NET Core 1.1 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="70da8-183">`VaryByQueryKeys` requires ASP.NET Core 1.1 or later.</span></span>

<span data-ttu-id="70da8-184">[Intergiciel de mise en cache des réponses](xref:performance/caching/middleware) doit être activé pour définir le `VaryByQueryKeys` propriété ; sinon, une exception runtime est levée.</span><span class="sxs-lookup"><span data-stu-id="70da8-184">[Response Caching Middleware](xref:performance/caching/middleware) must be enabled to set the `VaryByQueryKeys` property; otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="70da8-185">Il n’existe pas d’en-tête HTTP correspondant pour la propriété `VaryByQueryKeys`.</span><span class="sxs-lookup"><span data-stu-id="70da8-185">There isn't a corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="70da8-186">La propriété est une fonctionnalité HTTP gérée par l’intergiciel de mise en cache de réponse.</span><span class="sxs-lookup"><span data-stu-id="70da8-186">The property is an HTTP feature handled by the Response Caching Middleware.</span></span> <span data-ttu-id="70da8-187">Pour que l’intergiciel traite une réponse mise en cache, la chaîne de requête et la valeur de la chaîne de requête doivent correspondre à une demande précédente.</span><span class="sxs-lookup"><span data-stu-id="70da8-187">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="70da8-188">Par exemple, considérez la séquence des demandes et des résultats présentés dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="70da8-188">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="70da8-189">Demande</span><span class="sxs-lookup"><span data-stu-id="70da8-189">Request</span></span>                          | <span data-ttu-id="70da8-190">Résultat</span><span class="sxs-lookup"><span data-stu-id="70da8-190">Result</span></span>                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | <span data-ttu-id="70da8-191">Retourné à partir du serveur</span><span class="sxs-lookup"><span data-stu-id="70da8-191">Returned from server</span></span>     |
| `http://example.com?key1=value1` | <span data-ttu-id="70da8-192">Retourné à partir de l’intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="70da8-192">Returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="70da8-193">Retourné à partir du serveur</span><span class="sxs-lookup"><span data-stu-id="70da8-193">Returned from server</span></span>     |

<span data-ttu-id="70da8-194">La première requête est retournée par le serveur et mis en cache dans l’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="70da8-194">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="70da8-195">La deuxième demande est retournée par l’intergiciel (middleware), car la chaîne de requête correspond à la demande précédente.</span><span class="sxs-lookup"><span data-stu-id="70da8-195">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="70da8-196">La troisième requête n’est pas dans le cache de l’intergiciel (middleware), car la valeur de chaîne de requête ne correspond pas à une demande précédente.</span><span class="sxs-lookup"><span data-stu-id="70da8-196">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="70da8-197">Le `ResponseCacheAttribute` est utilisé pour configurer et créer (via `IFilterFactory`) un [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span><span class="sxs-lookup"><span data-stu-id="70da8-197">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span></span> <span data-ttu-id="70da8-198">Le `ResponseCacheFilter` effectue le travail de mise à jour les en-têtes HTTP appropriés et les fonctionnalités de la réponse.</span><span class="sxs-lookup"><span data-stu-id="70da8-198">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="70da8-199">Le filtre :</span><span class="sxs-lookup"><span data-stu-id="70da8-199">The filter:</span></span>

* <span data-ttu-id="70da8-200">Supprime tous les en-têtes existants pour `Vary`, `Cache-Control`, et `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="70da8-200">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="70da8-201">Écrit les en-têtes appropriés, selon les propriétés définies dans le `ResponseCacheAttribute`.</span><span class="sxs-lookup"><span data-stu-id="70da8-201">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="70da8-202">Met à jour de la réponse mise en cache de la fonctionnalité HTTP si `VaryByQueryKeys` est défini.</span><span class="sxs-lookup"><span data-stu-id="70da8-202">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="70da8-203">Vary</span><span class="sxs-lookup"><span data-stu-id="70da8-203">Vary</span></span>

<span data-ttu-id="70da8-204">Cet en-tête est écrit uniquement lorsque la propriété `VaryByHeader` est définie.</span><span class="sxs-lookup"><span data-stu-id="70da8-204">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="70da8-205">Il est défini sur la valeur de la propriété `Vary`.</span><span class="sxs-lookup"><span data-stu-id="70da8-205">It's set to the `Vary` property's value.</span></span> <span data-ttu-id="70da8-206">L’exemple suivant utilise la propriété `VaryByHeader` :</span><span class="sxs-lookup"><span data-stu-id="70da8-206">The following sample uses the `VaryByHeader` property:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

<span data-ttu-id="70da8-207">Vous pouvez afficher les en-têtes de réponse avec les outils réseau de votre navigateur.</span><span class="sxs-lookup"><span data-stu-id="70da8-207">You can view the response headers with your browser's network tools.</span></span> <span data-ttu-id="70da8-208">L’illustration suivante montre la sortie Edge F12 dans l'onglet **Réseau** lorsque la méthode d’action `About2` est actualisée :</span><span class="sxs-lookup"><span data-stu-id="70da8-208">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed:</span></span>

![Sous l’onglet réseau de périmètre F12 sortie lorsque la méthode d’action About2 est appelée.](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="70da8-210">NoStore et Location.None</span><span class="sxs-lookup"><span data-stu-id="70da8-210">NoStore and Location.None</span></span>

<span data-ttu-id="70da8-211">`NoStore` remplace la plupart des autres propriétés.</span><span class="sxs-lookup"><span data-stu-id="70da8-211">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="70da8-212">Lorsque cette propriété a la valeur `true`, l'en-tête `Cache-Control` est défini sur `no-store`.</span><span class="sxs-lookup"><span data-stu-id="70da8-212">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="70da8-213">Si `Location` a la valeur `None`:</span><span class="sxs-lookup"><span data-stu-id="70da8-213">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="70da8-214">`Cache-Control` a la valeur `no-store,no-cache`.</span><span class="sxs-lookup"><span data-stu-id="70da8-214">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="70da8-215">`Pragma` a la valeur `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="70da8-215">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="70da8-216">Si `NoStore` est `false` et `Location` est `None`, `Cache-Control` et `Pragma` sont définies sur `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="70da8-216">If `NoStore` is `false` and `Location` is `None`, `Cache-Control` and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="70da8-217">Vous définissez généralement `NoStore` à `true` sur les pages d’erreur.</span><span class="sxs-lookup"><span data-stu-id="70da8-217">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="70da8-218">Exemple :</span><span class="sxs-lookup"><span data-stu-id="70da8-218">For example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

<span data-ttu-id="70da8-219">Il en résulte dans les en-têtes suivants :</span><span class="sxs-lookup"><span data-stu-id="70da8-219">This results in the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="70da8-220">Emplacement et durée</span><span class="sxs-lookup"><span data-stu-id="70da8-220">Location and Duration</span></span>

<span data-ttu-id="70da8-221">Pour activer la mise en cache, `Duration` doit être définie sur une valeur positive et `Location` doit avoir la valeur `Any` (la valeur par défaut) ou `Client`.</span><span class="sxs-lookup"><span data-stu-id="70da8-221">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="70da8-222">Dans ce cas, le `Cache-Control` en-tête est défini sur la valeur d’emplacement suivie de la `max-age` de la réponse.</span><span class="sxs-lookup"><span data-stu-id="70da8-222">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="70da8-223">`Location`d’options de `Any` et `Client` traduisent `Cache-Control` valeurs d’en-tête de `public` et `private`, respectivement.</span><span class="sxs-lookup"><span data-stu-id="70da8-223">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="70da8-224">Comme mentionné précédemment, le paramètre `Location` à `None` définit à la fois `Cache-Control` et `Pragma` en-têtes à `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="70da8-224">As noted previously, setting `Location` to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="70da8-225">Vous trouverez ci-dessous un exemple montrant les en-têtes produits en définissant `Duration` et en conservant la valeur `Location` par défaut :</span><span class="sxs-lookup"><span data-stu-id="70da8-225">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

<span data-ttu-id="70da8-226">Cela génère l’en-tête suivant :</span><span class="sxs-lookup"><span data-stu-id="70da8-226">This produces the following header:</span></span>

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a><span data-ttu-id="70da8-227">Profils de cache</span><span class="sxs-lookup"><span data-stu-id="70da8-227">Cache profiles</span></span>

<span data-ttu-id="70da8-228">Au lieu de répéter les paramètres `ResponseCache` sur plusieurs attributs d’action de contrôleur, les profils de cache peuvent être configurés en tant qu’options lorsque vous configurez MVC dans la méthode `ConfigureServices` dans `Startup`.</span><span class="sxs-lookup"><span data-stu-id="70da8-228">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="70da8-229">Les valeurs d’un profil de cache référencé sont utilisées en tant que valeurs par défaut par l'attribut `ResponseCache` et sont remplacées par les propriétés spécifiées dans l’attribut.</span><span class="sxs-lookup"><span data-stu-id="70da8-229">Values found in a referenced cache profile are used as the defaults by the `ResponseCache` attribute and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="70da8-230">Configuration d’un profil de cache :</span><span class="sxs-lookup"><span data-stu-id="70da8-230">Setting up a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="70da8-231">En faisant référence à un profil de cache :</span><span class="sxs-lookup"><span data-stu-id="70da8-231">Referencing a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

<span data-ttu-id="70da8-232">L'attribut `ResponseCache` peut être appliqué à la fois aux actions (méthodes) et aux contrôleurs (classes).</span><span class="sxs-lookup"><span data-stu-id="70da8-232">The `ResponseCache` attribute can be applied both to actions (methods) and controllers (classes).</span></span> <span data-ttu-id="70da8-233">Les attributs au niveau de la méthode remplacent les paramètres spécifiés dans les attributs au niveau de la classe.</span><span class="sxs-lookup"><span data-stu-id="70da8-233">Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="70da8-234">Dans l’exemple ci-dessus, un attribut de niveau classe spécifie une durée de 30 secondes, pendant un attribut de niveau de la méthode fait référence à un profil de cache avec une durée définie sur 60 secondes.</span><span class="sxs-lookup"><span data-stu-id="70da8-234">In the above example, a class-level attribute specifies a duration of 30 seconds, while a method-level attribute references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="70da8-235">L’en-tête qui en résulte :</span><span class="sxs-lookup"><span data-stu-id="70da8-235">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a><span data-ttu-id="70da8-236">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="70da8-236">Additional resources</span></span>

* [<span data-ttu-id="70da8-237">Stockage des réponses dans les Caches</span><span class="sxs-lookup"><span data-stu-id="70da8-237">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="70da8-238">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="70da8-238">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
