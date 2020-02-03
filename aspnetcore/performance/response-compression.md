---
title: Compression des réponses en ASP.NET Core
author: guardrex
description: En savoir plus sur la compression de la réponse et comment utiliser un intergiciel (middleware) de Compression de réponse dans les applications ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
uid: performance/response-compression
ms.openlocfilehash: b8a84418a3258e9ac43b4eadd8564c0708590bce
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726964"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="6a8a5-103">Compression des réponses en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6a8a5-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="6a8a5-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6a8a5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6a8a5-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6a8a5-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6a8a5-106">La bande passante réseau est une ressource limitée.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="6a8a5-107">Le fait de réduire la taille de la réponse augmente généralement la réactivité d’une application, parfois de manière considérable.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="6a8a5-108">Une façon de réduire la taille de la charge utile consiste à compresser les réponses de l’application.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="6a8a5-109">Quand utiliser l’intergiciel (middleware) de compression de réponse</span><span class="sxs-lookup"><span data-stu-id="6a8a5-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="6a8a5-110">Utilisez les technologies de compression des réponses basées sur le serveur dans IIS, Apache ou nginx.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="6a8a5-111">Les performances de l’intergiciel (middleware) ne correspondront probablement pas à celles des modules de serveur.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="6a8a5-112">Le serveur de serveur [http. sys](xref:fundamentals/servers/httpsys) et le serveur [Kestrel](xref:fundamentals/servers/kestrel) n’offrent pas de prise en charge intégrée de la compression.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="6a8a5-113">Utilisez l’intergiciel (middleware) de compression des réponses lorsque vous êtes :</span><span class="sxs-lookup"><span data-stu-id="6a8a5-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="6a8a5-114">Impossible d’utiliser les technologies de compression basées sur le serveur suivantes :</span><span class="sxs-lookup"><span data-stu-id="6a8a5-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="6a8a5-115">Module de compression dynamique IIS</span><span class="sxs-lookup"><span data-stu-id="6a8a5-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="6a8a5-116">Module Apache mod_deflate</span><span class="sxs-lookup"><span data-stu-id="6a8a5-116">Apache mod_deflate module</span></span>](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="6a8a5-117">Compression et décompression Nginx</span><span class="sxs-lookup"><span data-stu-id="6a8a5-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="6a8a5-118">Hébergement direct :</span><span class="sxs-lookup"><span data-stu-id="6a8a5-118">Hosting directly on:</span></span>
  * <span data-ttu-id="6a8a5-119">[Serveur http. sys](xref:fundamentals/servers/httpsys) (anciennement appelé webListener)</span><span class="sxs-lookup"><span data-stu-id="6a8a5-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="6a8a5-120">Serveur Kestrel</span><span class="sxs-lookup"><span data-stu-id="6a8a5-120">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="6a8a5-121">Compression des réponses</span><span class="sxs-lookup"><span data-stu-id="6a8a5-121">Response compression</span></span>

<span data-ttu-id="6a8a5-122">En règle générale, toute réponse qui n’est pas compressée en mode natif peut tirer parti de la compression des réponses.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="6a8a5-123">Les réponses qui ne sont pas compressées en mode natif incluent généralement : CSS, JavaScript, HTML, XML et JSON.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="6a8a5-124">Vous ne devez pas compresser les ressources natives compressées, telles que les fichiers PNG.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="6a8a5-125">Si vous tentez de compresser davantage une réponse compressée en mode natif, toute petite réduction supplémentaire de taille et de temps de transmission sera probablement survenue du temps nécessaire au traitement de la compression.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="6a8a5-126">Ne compressez pas les fichiers d’une taille inférieure à environ 150-1000 octets (en fonction du contenu du fichier et de l’efficacité de la compression).</span><span class="sxs-lookup"><span data-stu-id="6a8a5-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="6a8a5-127">La surcharge liée à la compression de petits fichiers peut entraîner la création d’un fichier compressé plus volumineux que le fichier non compressé.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="6a8a5-128">Lorsqu’un client peut traiter du contenu compressé, le client doit informer le serveur de ses fonctionnalités en envoyant l’en-tête `Accept-Encoding` avec la demande.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="6a8a5-129">Lorsqu’un serveur envoie du contenu compressé, il doit inclure des informations dans l’en-tête `Content-Encoding` sur la façon dont la réponse compressée est encodée.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="6a8a5-130">Les désignations de codage de contenu prises en charge par l’intergiciel (middleware) sont indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="6a8a5-131">valeurs d’en-tête `Accept-Encoding`</span><span class="sxs-lookup"><span data-stu-id="6a8a5-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="6a8a5-132">Intergiciel pris en charge</span><span class="sxs-lookup"><span data-stu-id="6a8a5-132">Middleware Supported</span></span> | <span data-ttu-id="6a8a5-133">Description</span><span class="sxs-lookup"><span data-stu-id="6a8a5-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="6a8a5-134">Oui (valeur par défaut)</span><span class="sxs-lookup"><span data-stu-id="6a8a5-134">Yes (default)</span></span>        | [<span data-ttu-id="6a8a5-135">Format de données compressées Brotli</span><span class="sxs-lookup"><span data-stu-id="6a8a5-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="6a8a5-136">Non</span><span class="sxs-lookup"><span data-stu-id="6a8a5-136">No</span></span>                   | [<span data-ttu-id="6a8a5-137">Format de données compressées compressé</span><span class="sxs-lookup"><span data-stu-id="6a8a5-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="6a8a5-138">Non</span><span class="sxs-lookup"><span data-stu-id="6a8a5-138">No</span></span>                   | [<span data-ttu-id="6a8a5-139">Échange XML efficace W3C</span><span class="sxs-lookup"><span data-stu-id="6a8a5-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="6a8a5-140">Oui</span><span class="sxs-lookup"><span data-stu-id="6a8a5-140">Yes</span></span>                  | [<span data-ttu-id="6a8a5-141">Format de fichier gzip</span><span class="sxs-lookup"><span data-stu-id="6a8a5-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="6a8a5-142">Oui</span><span class="sxs-lookup"><span data-stu-id="6a8a5-142">Yes</span></span>                  | <span data-ttu-id="6a8a5-143">Identificateur "No encoding" : la réponse ne doit pas être encodée.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="6a8a5-144">Non</span><span class="sxs-lookup"><span data-stu-id="6a8a5-144">No</span></span>                   | [<span data-ttu-id="6a8a5-145">Format de transfert réseau pour les archives Java</span><span class="sxs-lookup"><span data-stu-id="6a8a5-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="6a8a5-146">Oui</span><span class="sxs-lookup"><span data-stu-id="6a8a5-146">Yes</span></span>                  | <span data-ttu-id="6a8a5-147">N'importe quel encodage de contenu disponible non explicitement demandé</span><span class="sxs-lookup"><span data-stu-id="6a8a5-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="6a8a5-148">valeurs d’en-tête `Accept-Encoding`</span><span class="sxs-lookup"><span data-stu-id="6a8a5-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="6a8a5-149">Intergiciel pris en charge</span><span class="sxs-lookup"><span data-stu-id="6a8a5-149">Middleware Supported</span></span> | <span data-ttu-id="6a8a5-150">Description</span><span class="sxs-lookup"><span data-stu-id="6a8a5-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="6a8a5-151">Non</span><span class="sxs-lookup"><span data-stu-id="6a8a5-151">No</span></span>                   | [<span data-ttu-id="6a8a5-152">Format de données compressées Brotli</span><span class="sxs-lookup"><span data-stu-id="6a8a5-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="6a8a5-153">Non</span><span class="sxs-lookup"><span data-stu-id="6a8a5-153">No</span></span>                   | [<span data-ttu-id="6a8a5-154">Format de données compressées compressé</span><span class="sxs-lookup"><span data-stu-id="6a8a5-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="6a8a5-155">Non</span><span class="sxs-lookup"><span data-stu-id="6a8a5-155">No</span></span>                   | [<span data-ttu-id="6a8a5-156">Échange XML efficace W3C</span><span class="sxs-lookup"><span data-stu-id="6a8a5-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="6a8a5-157">Oui (valeur par défaut)</span><span class="sxs-lookup"><span data-stu-id="6a8a5-157">Yes (default)</span></span>        | [<span data-ttu-id="6a8a5-158">Format de fichier gzip</span><span class="sxs-lookup"><span data-stu-id="6a8a5-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="6a8a5-159">Oui</span><span class="sxs-lookup"><span data-stu-id="6a8a5-159">Yes</span></span>                  | <span data-ttu-id="6a8a5-160">Identificateur "No encoding" : la réponse ne doit pas être encodée.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="6a8a5-161">Non</span><span class="sxs-lookup"><span data-stu-id="6a8a5-161">No</span></span>                   | [<span data-ttu-id="6a8a5-162">Format de transfert réseau pour les archives Java</span><span class="sxs-lookup"><span data-stu-id="6a8a5-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="6a8a5-163">Oui</span><span class="sxs-lookup"><span data-stu-id="6a8a5-163">Yes</span></span>                  | <span data-ttu-id="6a8a5-164">N'importe quel encodage de contenu disponible non explicitement demandé</span><span class="sxs-lookup"><span data-stu-id="6a8a5-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="6a8a5-165">Pour plus d’informations, consultez la [liste de codage de contenu officielle IANA](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="6a8a5-165">For more information, see the [IANA Official Content Coding List](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="6a8a5-166">L’intergiciel (middleware) vous permet d’ajouter des fournisseurs de compression supplémentaires pour les valeurs d’en-tête `Accept-Encoding` personnalisées.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="6a8a5-167">Pour plus d’informations, consultez [fournisseurs personnalisés](#custom-providers) ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="6a8a5-168">L’intergiciel (middleware) peut réagir à la valeur de qualité (qvalue, `q`) pondérée lorsqu’il est envoyé par le client pour hiérarchiser les schémas de compression.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="6a8a5-169">Pour plus d’informations, consultez [RFC 7231 : Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="6a8a5-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="6a8a5-170">Les algorithmes de compression sont soumis à un compromis entre la vitesse de compression et l’efficacité de la compression.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="6a8a5-171">L' *efficacité* dans ce contexte fait référence à la taille de la sortie après compression.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="6a8a5-172">La plus petite taille est obtenue par la compression la plus *optimale* .</span><span class="sxs-lookup"><span data-stu-id="6a8a5-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="6a8a5-173">Les en-têtes impliqués dans la demande, l'envoi, la mise en cache et la réception de contenu compressé sont décrits dans le tableau ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="6a8a5-174">En-tête</span><span class="sxs-lookup"><span data-stu-id="6a8a5-174">Header</span></span>             | <span data-ttu-id="6a8a5-175">Role</span><span class="sxs-lookup"><span data-stu-id="6a8a5-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="6a8a5-176">Envoyée du client au serveur pour indiquer les schémas de codage de contenu acceptables pour le client.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="6a8a5-177">Envoyé à partir du serveur au client pour indiquer l'encodage du contenu de la charge utile.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="6a8a5-178">En cas de compression, l’en-tête `Content-Length` est supprimé, car le contenu du corps change lorsque la réponse est compressée.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="6a8a5-179">En cas de compression, l’en-tête `Content-MD5` est supprimé, car le contenu du corps a changé et le hachage n’est plus valide.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="6a8a5-180">Spécifie le type MIME du contenu.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="6a8a5-181">Chaque réponse doit spécifier son `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="6a8a5-182">L’intergiciel vérifie cette valeur pour déterminer si la réponse doit être compressée.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="6a8a5-183">L’intergiciel (middleware) spécifie un ensemble de [types MIME par défaut](#mime-types) qu’il peut encoder, mais vous pouvez remplacer ou ajouter des types MIME.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="6a8a5-184">Lorsqu’il est envoyé par le serveur avec une valeur de `Accept-Encoding` aux clients et proxys, l’en-tête `Vary` indique au client ou au proxy qu’il doit mettre en cache (variation) les réponses en fonction de la valeur de l’en-tête `Accept-Encoding` de la demande.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="6a8a5-185">Le résultat de la restitution du contenu avec l’en-tête `Vary: Accept-Encoding` est que les réponses compressées et non compressées sont mises en cache séparément.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="6a8a5-186">Explorez les fonctionnalités de l’intergiciel de compression des réponses avec l' [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="6a8a5-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="6a8a5-187">L’exemple illustre :</span><span class="sxs-lookup"><span data-stu-id="6a8a5-187">The sample illustrates:</span></span>

* <span data-ttu-id="6a8a5-188">La compression des réponses de l’application à l’aide de gzip et des fournisseurs de compression personnalisés.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="6a8a5-189">Comment ajouter un type MIME à la liste par défaut des types MIME pour la compression.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="6a8a5-190">Package</span><span class="sxs-lookup"><span data-stu-id="6a8a5-190">Package</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6a8a5-191">L’intergiciel (middleware) de compression des réponses est fourni par le package [Microsoft. AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) , qui est implicitement inclus dans les applications ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-191">Response Compression Middleware is provided by the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package, which is implicitly included in ASP.NET Core apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="6a8a5-192">Pour inclure l’intergiciel (middleware) dans un projet, ajoutez une référence au AspNetCore [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), qui inclut le package [Microsoft. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) .</span><span class="sxs-lookup"><span data-stu-id="6a8a5-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="6a8a5-193">Configuration</span><span class="sxs-lookup"><span data-stu-id="6a8a5-193">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6a8a5-194">Le code suivant montre comment activer l’intergiciel (middleware) de compression des réponses pour les types MIME et les fournisseurs de compression par défaut ([Brotli](#brotli-compression-provider) et [gzip](#gzip-compression-provider)) :</span><span class="sxs-lookup"><span data-stu-id="6a8a5-194">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="6a8a5-195">Le code suivant montre comment activer l’intergiciel (middleware) de compression des réponses pour les types MIME par défaut et le [fournisseur de compression gzip](#gzip-compression-provider):</span><span class="sxs-lookup"><span data-stu-id="6a8a5-195">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

::: moniker-end

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddResponseCompression();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        app.UseResponseCompression();
    }
}
```

<span data-ttu-id="6a8a5-196">Remarques :</span><span class="sxs-lookup"><span data-stu-id="6a8a5-196">Notes:</span></span>

* <span data-ttu-id="6a8a5-197">`app.UseResponseCompression` doit être appelé avant tout middleware qui compresse les réponses.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-197">`app.UseResponseCompression` must be called before any middleware that compresses responses.</span></span> <span data-ttu-id="6a8a5-198">Pour plus d’informations, consultez <xref:fundamentals/middleware/index#middleware-order>.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-198">For more information, see <xref:fundamentals/middleware/index#middleware-order>.</span></span>
* <span data-ttu-id="6a8a5-199">Utilisez un outil tel que [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)ou [postal](https://www.getpostman.com/) pour définir le `Accept-Encoding` en-tête de demande et étudier les en-têtes, la taille et le corps de la réponse.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-199">Use a tool such as [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="6a8a5-200">Envoyez une demande à l’exemple d’application sans l’en-tête `Accept-Encoding` et observez que la réponse n’est pas compressée.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-200">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="6a8a5-201">Les en-têtes `Content-Encoding` et `Vary` ne sont pas présents dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-201">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Fenêtre Fiddler indiquant le résultat d’une requête sans l’en-tête Accept-Encoding.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6a8a5-204">Envoyez une demande à l’exemple d’application avec l’en-tête `Accept-Encoding: br` (compression Brotli) et observez que la réponse est compressée.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-204">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="6a8a5-205">Les en-têtes `Content-Encoding` et `Vary` sont présents dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-205">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Fenêtre Fiddler présentant le résultat d’une demande avec l’en-tête Accept-Encoding et la valeur br.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="6a8a5-209">Envoyez une demande à l’exemple d’application avec l’en-tête `Accept-Encoding: gzip` et observez que la réponse est compressée.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-209">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="6a8a5-210">Les en-têtes `Content-Encoding` et `Vary` sont présents dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-210">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Fenêtre Fiddler présentant le résultat d’une demande avec l’en-tête Accept-Encoding et une valeur de gzip.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a><span data-ttu-id="6a8a5-214">Fournisseurs</span><span class="sxs-lookup"><span data-stu-id="6a8a5-214">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="6a8a5-215">Fournisseur de compression Brotli</span><span class="sxs-lookup"><span data-stu-id="6a8a5-215">Brotli Compression Provider</span></span>

<span data-ttu-id="6a8a5-216">Utilisez la <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> pour compresser les réponses avec le [format de données compressées Brotli](https://tools.ietf.org/html/rfc7932).</span><span class="sxs-lookup"><span data-stu-id="6a8a5-216">Use the <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="6a8a5-217">Si aucun fournisseur de compression n’est explicitement ajouté au <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="6a8a5-217">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="6a8a5-218">Le fournisseur de compression Brotli est ajouté par défaut au groupe de fournisseurs de compression avec le [fournisseur de compression gzip](#gzip-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="6a8a5-218">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="6a8a5-219">La compression prend par défaut la compression Brotli lorsque le format de données compressées Brotli est pris en charge par le client.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-219">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="6a8a5-220">Si Brotli n’est pas pris en charge par le client, la compression est définie par défaut sur gzip lorsque le client prend en charge la compression gzip.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-220">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="6a8a5-221">Le fournisseur de compression Brotoli doit être ajouté lors de l’ajout explicite de fournisseurs de compression :</span><span class="sxs-lookup"><span data-stu-id="6a8a5-221">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6a8a5-222">Définissez le niveau de compression avec <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-222">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span></span> <span data-ttu-id="6a8a5-223">Par défaut, le fournisseur de compression Brotli a le niveau de compression le plus rapide ([CompressionLevel. plus rapide](xref:System.IO.Compression.CompressionLevel)), qui peut ne pas produire la compression la plus efficace.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-223">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="6a8a5-224">Si vous souhaitez obtenir la compression la plus efficace possible, configurez l’intergiciel (middleware) pour une compression optimale.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-224">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="6a8a5-225">Niveau de compression</span><span class="sxs-lookup"><span data-stu-id="6a8a5-225">Compression Level</span></span> | <span data-ttu-id="6a8a5-226">Description</span><span class="sxs-lookup"><span data-stu-id="6a8a5-226">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="6a8a5-227">CompressionLevel. plus rapide</span><span class="sxs-lookup"><span data-stu-id="6a8a5-227">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="6a8a5-228">La compression doit se terminer aussi rapidement que possible, même si le résultat n’est pas compressé de manière optimale.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-228">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="6a8a5-229">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="6a8a5-229">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="6a8a5-230">Aucune compression ne doit être effectuée.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-230">No compression should be performed.</span></span> |
| [<span data-ttu-id="6a8a5-231">CompressionLevel. optimal</span><span class="sxs-lookup"><span data-stu-id="6a8a5-231">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="6a8a5-232">Les réponses doivent être compressées de façon optimale, même si la compression prend plus de temps.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-232">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<BrotliCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

::: moniker-end

### <a name="gzip-compression-provider"></a><span data-ttu-id="6a8a5-233">Fournisseur de compression gzip</span><span class="sxs-lookup"><span data-stu-id="6a8a5-233">Gzip Compression Provider</span></span>

<span data-ttu-id="6a8a5-234">Utilisez la <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> pour compresser les réponses avec le [format de fichier gzip](https://tools.ietf.org/html/rfc1952).</span><span class="sxs-lookup"><span data-stu-id="6a8a5-234">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="6a8a5-235">Si aucun fournisseur de compression n’est explicitement ajouté au <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="6a8a5-235">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="6a8a5-236">Le fournisseur de compression gzip est ajouté par défaut au tableau de fournisseurs de compression avec le [fournisseur de compression Brotli](#brotli-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="6a8a5-236">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="6a8a5-237">La compression prend par défaut la compression Brotli lorsque le format de données compressées Brotli est pris en charge par le client.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-237">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="6a8a5-238">Si Brotli n’est pas pris en charge par le client, la compression est définie par défaut sur gzip lorsque le client prend en charge la compression gzip.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-238">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="6a8a5-239">Le fournisseur de compression gzip est ajouté par défaut au tableau de fournisseurs de compression.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-239">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="6a8a5-240">La compression est définie par défaut sur gzip lorsque le client prend en charge la compression gzip.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-240">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="6a8a5-241">Le fournisseur de compression gzip doit être ajouté lors de l’ajout explicite de fournisseurs de compression :</span><span class="sxs-lookup"><span data-stu-id="6a8a5-241">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

<span data-ttu-id="6a8a5-242">Définissez le niveau de compression avec <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-242">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="6a8a5-243">Par défaut, le fournisseur de compression gzip a le niveau de compression le plus rapide ([CompressionLevel. plus rapide](xref:System.IO.Compression.CompressionLevel)), qui peut ne pas produire la compression la plus efficace.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-243">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="6a8a5-244">Si vous souhaitez obtenir la compression la plus efficace possible, configurez l’intergiciel (middleware) pour une compression optimale.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-244">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="6a8a5-245">Niveau de compression</span><span class="sxs-lookup"><span data-stu-id="6a8a5-245">Compression Level</span></span> | <span data-ttu-id="6a8a5-246">Description</span><span class="sxs-lookup"><span data-stu-id="6a8a5-246">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="6a8a5-247">CompressionLevel. plus rapide</span><span class="sxs-lookup"><span data-stu-id="6a8a5-247">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="6a8a5-248">La compression doit se terminer aussi rapidement que possible, même si le résultat n’est pas compressé de manière optimale.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-248">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="6a8a5-249">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="6a8a5-249">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="6a8a5-250">Aucune compression ne doit être effectuée.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-250">No compression should be performed.</span></span> |
| [<span data-ttu-id="6a8a5-251">CompressionLevel. optimal</span><span class="sxs-lookup"><span data-stu-id="6a8a5-251">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="6a8a5-252">Les réponses doivent être compressées de façon optimale, même si la compression prend plus de temps.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-252">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<GzipCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

### <a name="custom-providers"></a><span data-ttu-id="6a8a5-253">Fournisseurs personnalisés</span><span class="sxs-lookup"><span data-stu-id="6a8a5-253">Custom providers</span></span>

<span data-ttu-id="6a8a5-254">Créez des implémentations de compression personnalisées avec <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-254">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="6a8a5-255">Le <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> représente l’encodage de contenu que cette `ICompressionProvider` produit.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-255">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="6a8a5-256">L’intergiciel utilise ces informations pour choisir le fournisseur en fonction de la liste spécifiée dans l’en-tête `Accept-Encoding` de la requête.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-256">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="6a8a5-257">À l’aide de l’exemple d’application, le client envoie une demande avec l’en-tête `Accept-Encoding: mycustomcompression`.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-257">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="6a8a5-258">L’intergiciel utilise l’implémentation de compression personnalisée et retourne la réponse avec un en-tête de `Content-Encoding: mycustomcompression`.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-258">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="6a8a5-259">Le client doit être en mesure de décompresser l’encodage personnalisé pour qu’une implémentation de compression personnalisée fonctionne.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-259">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/3.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6a8a5-260">Envoyez une demande à l’exemple d’application avec l’en-tête `Accept-Encoding: mycustomcompression` et observez les en-têtes de réponse.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-260">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="6a8a5-261">Les en-têtes `Vary` et `Content-Encoding` sont présents dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-261">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="6a8a5-262">Le corps de la réponse (non affiché) n’est pas compressé par l’exemple.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-262">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="6a8a5-263">Il n’existe pas d’implémentation de compression dans la classe `CustomCompressionProvider` de l’exemple.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-263">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="6a8a5-264">Toutefois, l’exemple illustre l’emplacement où vous implémentez un tel algorithme de compression.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-264">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Fenêtre Fiddler présentant le résultat d’une demande avec l’en-tête Accept-Encoding et une valeur de mycustomcompression.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="6a8a5-267">types MIME</span><span class="sxs-lookup"><span data-stu-id="6a8a5-267">MIME types</span></span>

<span data-ttu-id="6a8a5-268">L’intergiciel (middleware) spécifie un ensemble de types MIME par défaut pour la compression :</span><span class="sxs-lookup"><span data-stu-id="6a8a5-268">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="6a8a5-269">Remplacez ou ajoutez des types MIME par les options de l’intergiciel (middleware) de compression des réponses.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-269">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="6a8a5-270">Notez que les types MIME génériques, tels que les `text/*` ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-270">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="6a8a5-271">L’exemple d’application ajoute un type MIME pour `image/svg+xml` et compresse et sert l’image de bannière ASP.NET Core (*Banner. svg*).</span><span class="sxs-lookup"><span data-stu-id="6a8a5-271">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="6a8a5-272">Compression avec protocole sécurisé</span><span class="sxs-lookup"><span data-stu-id="6a8a5-272">Compression with secure protocol</span></span>

<span data-ttu-id="6a8a5-273">Les réponses compressées sur des connexions sécurisées peuvent être contrôlées à l’aide de l’option `EnableForHttps`, qui est désactivée par défaut.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-273">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="6a8a5-274">L’utilisation de la compression avec des pages générées dynamiquement peut entraîner des problèmes de sécurité tels que les attaques de [criminalité](https://wikipedia.org/wiki/CRIME_(security_exploit)) et de [violation](https://wikipedia.org/wiki/BREACH_(security_exploit)) .</span><span class="sxs-lookup"><span data-stu-id="6a8a5-274">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="6a8a5-275">Ajout de l’en-tête Vary</span><span class="sxs-lookup"><span data-stu-id="6a8a5-275">Adding the Vary header</span></span>

<span data-ttu-id="6a8a5-276">Lors de la compression des réponses basées sur l’en-tête `Accept-Encoding`, il existe potentiellement plusieurs versions compressées de la réponse et une version non compressée.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-276">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="6a8a5-277">Pour indiquer au client et aux caches proxy que plusieurs versions existent et doivent être stockées, l’en-tête `Vary` est ajouté avec une valeur de `Accept-Encoding`.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-277">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="6a8a5-278">Dans ASP.NET Core 2,0 ou version ultérieure, l’intergiciel ajoute l’en-tête de `Vary` automatiquement lorsque la réponse est compressée.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-278">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="6a8a5-279">Problème d’intergiciel lors de l’arrière-plan d’un proxy inverse Nginx</span><span class="sxs-lookup"><span data-stu-id="6a8a5-279">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="6a8a5-280">Lorsqu’une demande est traitée par un proxy par nginx, l’en-tête `Accept-Encoding` est supprimé.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-280">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="6a8a5-281">La suppression de l’en-tête `Accept-Encoding` empêche l’intergiciel de compresser la réponse.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-281">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="6a8a5-282">Pour plus d’informations, consultez [Nginx : compression et décompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="6a8a5-282">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="6a8a5-283">Ce problème est suivi par la [compression directe pour Nginx (ASPNET/BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="6a8a5-283">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="6a8a5-284">Utilisation de la compression dynamique IIS</span><span class="sxs-lookup"><span data-stu-id="6a8a5-284">Working with IIS dynamic compression</span></span>

<span data-ttu-id="6a8a5-285">Si vous disposez d’un module de compression dynamique IIS configuré au niveau du serveur que vous souhaitez désactiver pour une application, désactivez le module avec un ajout au fichier *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="6a8a5-285">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="6a8a5-286">Pour plus d’informations, consultez [Désactivation de modules IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="6a8a5-286">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="6a8a5-287">Dépannage</span><span class="sxs-lookup"><span data-stu-id="6a8a5-287">Troubleshooting</span></span>

<span data-ttu-id="6a8a5-288">Utilisez un outil tel que [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)ou [postal](https://www.getpostman.com/), qui vous permet de définir l’en-tête de demande `Accept-Encoding` et d’étudier les en-têtes, la taille et le corps de la réponse.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-288">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="6a8a5-289">Par défaut, l’intergiciel (middleware) de compression des réponses compresse les réponses qui remplissent les conditions suivantes :</span><span class="sxs-lookup"><span data-stu-id="6a8a5-289">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="6a8a5-290">L’en-tête `Accept-Encoding` est présent avec une valeur de `br`, `gzip`, `*`ou un encodage personnalisé qui correspond à un fournisseur de compression personnalisé que vous avez établi.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-290">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="6a8a5-291">La valeur ne doit pas être `identity` ou avoir une valeur de qualité (qvalue, `q`) égale à 0 (zéro).</span><span class="sxs-lookup"><span data-stu-id="6a8a5-291">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="6a8a5-292">Le type MIME (`Content-Type`) doit être défini et doit correspondre à un type MIME configuré sur le <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-292">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="6a8a5-293">La requête ne doit pas inclure l’en-tête `Content-Range`.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-293">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="6a8a5-294">La requête doit utiliser un protocole non sécurisé (http), sauf si un protocole sécurisé (https) est configuré dans les options de l’intergiciel de compression de la réponse.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-294">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="6a8a5-295">*Notez le danger [décrit ci-dessus](#compression-with-secure-protocol) lors de l’activation de la compression de contenu sécurisé.*</span><span class="sxs-lookup"><span data-stu-id="6a8a5-295">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="6a8a5-296">L’en-tête `Accept-Encoding` est présent avec une valeur de `gzip`, `*`ou un encodage personnalisé qui correspond à un fournisseur de compression personnalisé que vous avez établi.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-296">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="6a8a5-297">La valeur ne doit pas être `identity` ou avoir une valeur de qualité (qvalue, `q`) égale à 0 (zéro).</span><span class="sxs-lookup"><span data-stu-id="6a8a5-297">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="6a8a5-298">Le type MIME (`Content-Type`) doit être défini et doit correspondre à un type MIME configuré sur le <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-298">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="6a8a5-299">La requête ne doit pas inclure l’en-tête `Content-Range`.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-299">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="6a8a5-300">La requête doit utiliser un protocole non sécurisé (http), sauf si un protocole sécurisé (https) est configuré dans les options de l’intergiciel de compression de la réponse.</span><span class="sxs-lookup"><span data-stu-id="6a8a5-300">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="6a8a5-301">*Notez le danger [décrit ci-dessus](#compression-with-secure-protocol) lors de l’activation de la compression de contenu sécurisé.*</span><span class="sxs-lookup"><span data-stu-id="6a8a5-301">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="6a8a5-302">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="6a8a5-302">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="6a8a5-303">Réseau des développeurs Mozilla : accepter-encodage</span><span class="sxs-lookup"><span data-stu-id="6a8a5-303">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="6a8a5-304">RFC 7231 section 3.1.2.1 : codages de contenu</span><span class="sxs-lookup"><span data-stu-id="6a8a5-304">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="6a8a5-305">RFC 7230, section 4.2.3 : codage gzip</span><span class="sxs-lookup"><span data-stu-id="6a8a5-305">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="6a8a5-306">Spécification de format de fichier GZIP version 4,3</span><span class="sxs-lookup"><span data-stu-id="6a8a5-306">GZIP file format specification version 4.3</span></span>](https://www.ietf.org/rfc/rfc1952.txt)
