---
title: Compression des réponses dans ASP.NET Core
author: guardrex
description: En savoir plus sur la compression de la réponse et comment utiliser un intergiciel (middleware) de Compression de réponse dans les applications ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2018
uid: performance/response-compression
ms.openlocfilehash: 3a01c2d572c0026944347f736f9658a7872e6c35
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028282"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="6f666-103">Compression des réponses dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6f666-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="6f666-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6f666-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6f666-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6f666-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6f666-106">La bande passante réseau est une ressource limitée.</span><span class="sxs-lookup"><span data-stu-id="6f666-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="6f666-107">Le fait de réduire la taille de la réponse augmente généralement la réactivité d’une application, parfois de manière considérable.</span><span class="sxs-lookup"><span data-stu-id="6f666-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="6f666-108">Une façon de réduire la taille de la charge utile consiste à compresser les réponses de l’application.</span><span class="sxs-lookup"><span data-stu-id="6f666-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="6f666-109">Quand utiliser l’intergiciel (middleware) de compression de réponse</span><span class="sxs-lookup"><span data-stu-id="6f666-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="6f666-110">Utilisez les technologies de compression de réponse basées sur le serveur dans IIS, Apache ou Nginx.</span><span class="sxs-lookup"><span data-stu-id="6f666-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="6f666-111">Les performances de l’intergiciel (middleware) ne correspondront probablement pas à celles des modules de serveur.</span><span class="sxs-lookup"><span data-stu-id="6f666-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="6f666-112">Les serveurs [HTTP.sys](xref:fundamentals/servers/httpsys) et [Kestrel](xref:fundamentals/servers/kestrel) ne prennent pas actuellement en charge la compression intégrée.</span><span class="sxs-lookup"><span data-stu-id="6f666-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) and [Kestrel](xref:fundamentals/servers/kestrel) don't currently offer built-in compression support.</span></span>

<span data-ttu-id="6f666-113">Utilisez l’intergiciel (middleware) de compression de réponse dans les cas suivants :</span><span class="sxs-lookup"><span data-stu-id="6f666-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="6f666-114">Impossibilité d’utiliser les technologies de compression suivantes basées sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="6f666-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="6f666-115">Module de compression dynamique IIS</span><span class="sxs-lookup"><span data-stu-id="6f666-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="6f666-116">Module Apache mod_deflate</span><span class="sxs-lookup"><span data-stu-id="6f666-116">Apache mod_deflate module</span></span>](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="6f666-117">Décompression et compression Nginx</span><span class="sxs-lookup"><span data-stu-id="6f666-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="6f666-118">Hébergement directement sur :</span><span class="sxs-lookup"><span data-stu-id="6f666-118">Hosting directly on:</span></span>
  * <span data-ttu-id="6f666-119">[Serveur HTTP.sys](xref:fundamentals/servers/httpsys) (anciennement appelé [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="6f666-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>
  * [<span data-ttu-id="6f666-120">Kestrel</span><span class="sxs-lookup"><span data-stu-id="6f666-120">Kestrel</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="6f666-121">Compression des réponses</span><span class="sxs-lookup"><span data-stu-id="6f666-121">Response compression</span></span>

<span data-ttu-id="6f666-122">En règle générale, toute réponse ne compressée pas en mode natif peut bénéficier de la compression de réponse.</span><span class="sxs-lookup"><span data-stu-id="6f666-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="6f666-123">Les réponses pas en mode natif compressés en général : CSS, JavaScript, HTML, XML et JSON.</span><span class="sxs-lookup"><span data-stu-id="6f666-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="6f666-124">Vous ne doivent pas compresser des ressources compressées en mode natif, telles que des fichiers PNG.</span><span class="sxs-lookup"><span data-stu-id="6f666-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="6f666-125">Si vous tentez de compresser davantage une réponse compressée en mode natif, une petite réduction supplémentaire dans le temps de taille et la transmission sera probablement être été éclipsée par le temps de traitement de la compression.</span><span class="sxs-lookup"><span data-stu-id="6f666-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="6f666-126">Ne pas compresser les fichiers inférieurs à environ 150-1000 octets (selon le contenu du fichier et l’efficacité de la compression).</span><span class="sxs-lookup"><span data-stu-id="6f666-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="6f666-127">La surcharge de la compression des fichiers de petite taille peut produire un fichier compressé plus volumineux que le fichier non compressé.</span><span class="sxs-lookup"><span data-stu-id="6f666-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="6f666-128">Quand un client peut traiter du contenu compressé, il doit en informer le serveur en envoyant l'en-tête `Accept-Encoding` avec la requête.</span><span class="sxs-lookup"><span data-stu-id="6f666-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="6f666-129">Quand un serveur envoie du contenu compressé, il doit inclure dans l’en-tête `Content-Encoding` des informations sur le mode d’encodage de la réponse compressée.</span><span class="sxs-lookup"><span data-stu-id="6f666-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="6f666-130">Les désignations d'encodage de contenu prises en charge par l’intergiciel sont affichées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="6f666-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="6f666-131">Valeurs d’en-tête `Accept-Encoding`</span><span class="sxs-lookup"><span data-stu-id="6f666-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="6f666-132">Intergiciel pris en charge</span><span class="sxs-lookup"><span data-stu-id="6f666-132">Middleware Supported</span></span> | <span data-ttu-id="6f666-133">Description</span><span class="sxs-lookup"><span data-stu-id="6f666-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="6f666-134">Oui (valeur par défaut)</span><span class="sxs-lookup"><span data-stu-id="6f666-134">Yes (default)</span></span>        | [<span data-ttu-id="6f666-135">Format de données compressées Brotli</span><span class="sxs-lookup"><span data-stu-id="6f666-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="6f666-136">Non</span><span class="sxs-lookup"><span data-stu-id="6f666-136">No</span></span>                   | [<span data-ttu-id="6f666-137">Format de données compressées DEFLATE</span><span class="sxs-lookup"><span data-stu-id="6f666-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="6f666-138">Non</span><span class="sxs-lookup"><span data-stu-id="6f666-138">No</span></span>                   | [<span data-ttu-id="6f666-139">Échange efficace de XML W3C</span><span class="sxs-lookup"><span data-stu-id="6f666-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="6f666-140">Oui</span><span class="sxs-lookup"><span data-stu-id="6f666-140">Yes</span></span>                  | [<span data-ttu-id="6f666-141">Format de fichier GZIP</span><span class="sxs-lookup"><span data-stu-id="6f666-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="6f666-142">Oui</span><span class="sxs-lookup"><span data-stu-id="6f666-142">Yes</span></span>                  | <span data-ttu-id="6f666-143">Identificateur "No encoding" : la réponse ne doit pas être encodée.</span><span class="sxs-lookup"><span data-stu-id="6f666-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="6f666-144">Non</span><span class="sxs-lookup"><span data-stu-id="6f666-144">No</span></span>                   | [<span data-ttu-id="6f666-145">Format de transfert de réseau d’Archives Java</span><span class="sxs-lookup"><span data-stu-id="6f666-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="6f666-146">Oui</span><span class="sxs-lookup"><span data-stu-id="6f666-146">Yes</span></span>                  | <span data-ttu-id="6f666-147">N'importe quel encodage de contenu disponible non explicitement demandé</span><span class="sxs-lookup"><span data-stu-id="6f666-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="6f666-148">Valeurs d’en-tête `Accept-Encoding`</span><span class="sxs-lookup"><span data-stu-id="6f666-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="6f666-149">Intergiciel pris en charge</span><span class="sxs-lookup"><span data-stu-id="6f666-149">Middleware Supported</span></span> | <span data-ttu-id="6f666-150">Description</span><span class="sxs-lookup"><span data-stu-id="6f666-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="6f666-151">Non</span><span class="sxs-lookup"><span data-stu-id="6f666-151">No</span></span>                   | [<span data-ttu-id="6f666-152">Format de données compressées Brotli</span><span class="sxs-lookup"><span data-stu-id="6f666-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="6f666-153">Non</span><span class="sxs-lookup"><span data-stu-id="6f666-153">No</span></span>                   | [<span data-ttu-id="6f666-154">Format de données compressées DEFLATE</span><span class="sxs-lookup"><span data-stu-id="6f666-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="6f666-155">Non</span><span class="sxs-lookup"><span data-stu-id="6f666-155">No</span></span>                   | [<span data-ttu-id="6f666-156">Échange efficace de XML W3C</span><span class="sxs-lookup"><span data-stu-id="6f666-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="6f666-157">Oui (valeur par défaut)</span><span class="sxs-lookup"><span data-stu-id="6f666-157">Yes (default)</span></span>        | [<span data-ttu-id="6f666-158">Format de fichier GZIP</span><span class="sxs-lookup"><span data-stu-id="6f666-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="6f666-159">Oui</span><span class="sxs-lookup"><span data-stu-id="6f666-159">Yes</span></span>                  | <span data-ttu-id="6f666-160">Identificateur "No encoding" : la réponse ne doit pas être encodée.</span><span class="sxs-lookup"><span data-stu-id="6f666-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="6f666-161">Non</span><span class="sxs-lookup"><span data-stu-id="6f666-161">No</span></span>                   | [<span data-ttu-id="6f666-162">Format de transfert de réseau d’Archives Java</span><span class="sxs-lookup"><span data-stu-id="6f666-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="6f666-163">Oui</span><span class="sxs-lookup"><span data-stu-id="6f666-163">Yes</span></span>                  | <span data-ttu-id="6f666-164">N'importe quel encodage de contenu disponible non explicitement demandé</span><span class="sxs-lookup"><span data-stu-id="6f666-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="6f666-165">Pour plus d’informations, consultez la [liste IANA officielle des encodages de contenu](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="6f666-165">For more information, see the [IANA Official Content Coding List](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="6f666-166">L’intergiciel vous permet d’ajouter des fournisseurs de compression supplémentaires pour des valeurs d’en-tête `Accept-Encoding` personnalisées.</span><span class="sxs-lookup"><span data-stu-id="6f666-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="6f666-167">Pour plus d’informations, consultez [Fournisseurs personnalisés](#custom-providers) ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="6f666-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="6f666-168">L’intergiciel peut réagir à la pondération de la valeur de qualité (qvalue, `q`) envoyée par le client pour hiérarchiser les schémas de compression.</span><span class="sxs-lookup"><span data-stu-id="6f666-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="6f666-169">Pour plus d’informations, consultez le [document RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="6f666-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="6f666-170">Les algorithmes de compression donnent lieu à un compromis entre vitesse de compression et efficacité de la compression.</span><span class="sxs-lookup"><span data-stu-id="6f666-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="6f666-171">L’*Efficacité* dans ce contexte fait référence à la taille de la sortie après la compression.</span><span class="sxs-lookup"><span data-stu-id="6f666-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="6f666-172">La plus petite taille est obtenue par la compression la plus *optimale*.</span><span class="sxs-lookup"><span data-stu-id="6f666-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="6f666-173">Les en-têtes impliqués dans la demande, l'envoi, la mise en cache et la réception de contenu compressé sont décrits dans le tableau ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="6f666-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="6f666-174">Header</span><span class="sxs-lookup"><span data-stu-id="6f666-174">Header</span></span>             | <span data-ttu-id="6f666-175">Rôle</span><span class="sxs-lookup"><span data-stu-id="6f666-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="6f666-176">Envoyé à partir du client au serveur pour indiquer les schémas d’encodage de contenu acceptables pour le client.</span><span class="sxs-lookup"><span data-stu-id="6f666-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="6f666-177">Envoyé à partir du serveur au client pour indiquer l'encodage du contenu de la charge utile.</span><span class="sxs-lookup"><span data-stu-id="6f666-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="6f666-178">Au moment de la compression, l'en-tête `Content-Length` est supprimé car le contenu du corps change quand la réponse est compressée.</span><span class="sxs-lookup"><span data-stu-id="6f666-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="6f666-179">Durant la compression, l’en-tête `Content-MD5` est supprimé puisque le contenu du corps a changé et que le hachage n’est plus valide.</span><span class="sxs-lookup"><span data-stu-id="6f666-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="6f666-180">Spécifie le type MIME du contenu.</span><span class="sxs-lookup"><span data-stu-id="6f666-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="6f666-181">Chaque réponse doit spécifier son `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="6f666-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="6f666-182">L’intergiciel vérifie cette valeur pour déterminer si la réponse doit être compressée.</span><span class="sxs-lookup"><span data-stu-id="6f666-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="6f666-183">L’intergiciel (middleware) spécifie un ensemble de [par défaut des types MIME](#mime-types) qui peut donc coder, mais vous pouvez remplacer ou ajouter des types MIME.</span><span class="sxs-lookup"><span data-stu-id="6f666-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="6f666-184">Quand l’en-tête `Vary` est envoyé par le serveur avec la valeur `Accept-Encoding` à des clients et proxys, ces derniers doivent mettre en cache les réponses (vary) en fonction de la valeur de l’en-tête `Accept-Encoding` de la requête.</span><span class="sxs-lookup"><span data-stu-id="6f666-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="6f666-185">Si du contenu est retourné avec l'en-tête `Vary: Accept-Encoding`, les réponses (compressées et non compressées) sont mises en cache séparément.</span><span class="sxs-lookup"><span data-stu-id="6f666-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="6f666-186">Explorez les fonctionnalités de l’intergiciel de Compression de réponse avec le [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="6f666-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="6f666-187">L’exemple illustre :</span><span class="sxs-lookup"><span data-stu-id="6f666-187">The sample illustrates:</span></span>

* <span data-ttu-id="6f666-188">La compression des réponses d’application à l’aide de Gzip et fournisseurs de compression personnalisé.</span><span class="sxs-lookup"><span data-stu-id="6f666-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="6f666-189">Comment ajouter un type MIME à la liste par défaut des types MIME pour la compression.</span><span class="sxs-lookup"><span data-stu-id="6f666-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="6f666-190">Package</span><span class="sxs-lookup"><span data-stu-id="6f666-190">Package</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6f666-191">Pour inclure l’intergiciel (middleware) dans un projet, ajoutez une référence à la [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app), qui inclut le [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span><span class="sxs-lookup"><span data-stu-id="6f666-191">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6f666-192">Pour inclure l’intergiciel (middleware) dans un projet, ajoutez une référence à la [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage), qui inclut le [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span><span class="sxs-lookup"><span data-stu-id="6f666-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6f666-193">Pour inclure l’intergiciel (middleware) dans un projet, ajoutez une référence à la [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span><span class="sxs-lookup"><span data-stu-id="6f666-193">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="6f666-194">Configuration</span><span class="sxs-lookup"><span data-stu-id="6f666-194">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6f666-195">Le code suivant montre comment activer le Middleware de Compression de réponse pour les types MIME par défaut et les fournisseurs de compression ([Brotli](#brotli-compression-provider) et [Gzip](#gzip-compression-provider)) :</span><span class="sxs-lookup"><span data-stu-id="6f666-195">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="6f666-196">Le code suivant montre comment activer le Middleware de Compression de réponse pour les types MIME par défaut et le [fournisseur de Compression Gzip](#gzip-compression-provider):</span><span class="sxs-lookup"><span data-stu-id="6f666-196">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

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

> [!NOTE]
> <span data-ttu-id="6f666-197">Utilisez un outil tel que [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), ou [Postman](https://www.getpostman.com/) pour définir l'en-tête `Accept-Encoding` de la requête et étudier les en-têtes, la taille et le corps de la réponse.</span><span class="sxs-lookup"><span data-stu-id="6f666-197">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="6f666-198">Soumettez une requête à l’exemple d’application sans l’en-tête `Accept-Encoding` et observez que la réponse n’est pas compressée.</span><span class="sxs-lookup"><span data-stu-id="6f666-198">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="6f666-199">Les en-têtes  `Content-Encoding` et `Vary` ne sont pas présents sur la réponse.</span><span class="sxs-lookup"><span data-stu-id="6f666-199">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Fenêtre Fiddler indiquant le résultat d’une requête sans l’en-tête Accept-Encoding.](response-compression/_static/request-uncompressed.png)

<span data-ttu-id="6f666-202">Soumettez une requête à l’exemple d’application avec l’en-tête `Accept-Encoding: gzip` et observez que la réponse est compressée.</span><span class="sxs-lookup"><span data-stu-id="6f666-202">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="6f666-203">Les en-têtes `Content-Encoding` et `Vary` sont présents sur la réponse.</span><span class="sxs-lookup"><span data-stu-id="6f666-203">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Fenêtre Fiddler montrant le résultat d’une demande avec l’en-tête Accept-Encoding et la valeur gzip.](response-compression/_static/request-compressed.png)

## <a name="providers"></a><span data-ttu-id="6f666-207">Fournisseurs</span><span class="sxs-lookup"><span data-stu-id="6f666-207">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="6f666-208">Fournisseur de Compression Brotli</span><span class="sxs-lookup"><span data-stu-id="6f666-208">Brotli Compression Provider</span></span>

<span data-ttu-id="6f666-209">Utilisez le `BrotliCompressionProvider` pour compresser les réponses avec le [format de données compressées Brotli](https://tools.ietf.org/html/rfc7932).</span><span class="sxs-lookup"><span data-stu-id="6f666-209">Use the `BrotliCompressionProvider` to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="6f666-210">Si aucun fournisseur de compression n’est ajoutés explicitement à la <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="6f666-210">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="6f666-211">Le fournisseur de Compression Brotli est ajouté par défaut dans le tableau de fournisseurs de compression avec la [fournisseur de compression Gzip](#gzip-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="6f666-211">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="6f666-212">La compression par défaut de compression Brotli lorsque le format de données compressées Brotli est pris en charge par le client.</span><span class="sxs-lookup"><span data-stu-id="6f666-212">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="6f666-213">Si Brotli n’est pas pris en charge par le client, la compression par défaut Gzip lorsque le client prend en charge la compression Gzip.</span><span class="sxs-lookup"><span data-stu-id="6f666-213">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="6f666-214">Le fournisseur de Compression Brotoli doit être ajouté lorsque des fournisseurs de compression sont ajoutés explicitement :</span><span class="sxs-lookup"><span data-stu-id="6f666-214">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

<span data-ttu-id="6f666-215">Définir la compression au niveau avec `BrotliCompressionProviderOptions`.</span><span class="sxs-lookup"><span data-stu-id="6f666-215">Set the compression level with `BrotliCompressionProviderOptions`.</span></span> <span data-ttu-id="6f666-216">Le fournisseur de Compression Brotli par défaut est le niveau de compression plus rapide ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), ce qui ne peut pas produire la compression la plus efficace.</span><span class="sxs-lookup"><span data-stu-id="6f666-216">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="6f666-217">Si la compression la plus efficace est souhaitée, configurez l’intergiciel de compression optimale.</span><span class="sxs-lookup"><span data-stu-id="6f666-217">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="6f666-218">Niveau de compression</span><span class="sxs-lookup"><span data-stu-id="6f666-218">Compression Level</span></span> | <span data-ttu-id="6f666-219">Description</span><span class="sxs-lookup"><span data-stu-id="6f666-219">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="6f666-220">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="6f666-220">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="6f666-221">La compression doit se terminer aussi rapidement que possible, même si le résultat n’est pas compressé de manière optimale.</span><span class="sxs-lookup"><span data-stu-id="6f666-221">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="6f666-222">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="6f666-222">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="6f666-223">Aucune compression ne doit être effectuée.</span><span class="sxs-lookup"><span data-stu-id="6f666-223">No compression should be performed.</span></span> |
| [<span data-ttu-id="6f666-224">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="6f666-224">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="6f666-225">Les réponses doivent être compressées optimale, même si la compression prend plus de temps.</span><span class="sxs-lookup"><span data-stu-id="6f666-225">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="gzip-compression-provider"></a><span data-ttu-id="6f666-226">Fournisseur de Compression GZIP</span><span class="sxs-lookup"><span data-stu-id="6f666-226">Gzip Compression Provider</span></span>

<span data-ttu-id="6f666-227">Utilisez le <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> pour compresser les réponses avec le [format de fichier Gzip](https://tools.ietf.org/html/rfc1952).</span><span class="sxs-lookup"><span data-stu-id="6f666-227">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="6f666-228">Si aucun fournisseur de compression n’est ajoutés explicitement à la <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="6f666-228">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="6f666-229">Le fournisseur de la Compression Gzip est ajouté par défaut dans le tableau de fournisseurs de compression avec la [fournisseur de Compression Brotli](#brotli-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="6f666-229">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="6f666-230">La compression par défaut de compression Brotli lorsque le format de données compressées Brotli est pris en charge par le client.</span><span class="sxs-lookup"><span data-stu-id="6f666-230">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="6f666-231">Si Brotli n’est pas pris en charge par le client, la compression par défaut Gzip lorsque le client prend en charge la compression Gzip.</span><span class="sxs-lookup"><span data-stu-id="6f666-231">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="6f666-232">Le fournisseur de la Compression Gzip est ajouté au tableau de fournisseurs de compression par défaut.</span><span class="sxs-lookup"><span data-stu-id="6f666-232">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="6f666-233">Par défaut, la compression est Gzip lorsque le client prend en charge la compression Gzip.</span><span class="sxs-lookup"><span data-stu-id="6f666-233">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="6f666-234">Le fournisseur de la Compression Gzip doit être ajouté lorsque des fournisseurs de compression sont ajoutés explicitement :</span><span class="sxs-lookup"><span data-stu-id="6f666-234">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5)]

::: moniker-end

<span data-ttu-id="6f666-235">Définir la compression au niveau avec <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="6f666-235">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="6f666-236">Le fournisseur de la Compression Gzip par défaut est le niveau de compression plus rapide ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), ce qui ne peut pas produire la compression la plus efficace.</span><span class="sxs-lookup"><span data-stu-id="6f666-236">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="6f666-237">Si la compression la plus efficace est souhaitée, configurez l’intergiciel de compression optimale.</span><span class="sxs-lookup"><span data-stu-id="6f666-237">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="6f666-238">Niveau de compression</span><span class="sxs-lookup"><span data-stu-id="6f666-238">Compression Level</span></span> | <span data-ttu-id="6f666-239">Description</span><span class="sxs-lookup"><span data-stu-id="6f666-239">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="6f666-240">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="6f666-240">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="6f666-241">La compression doit se terminer aussi rapidement que possible, même si le résultat n’est pas compressé de manière optimale.</span><span class="sxs-lookup"><span data-stu-id="6f666-241">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="6f666-242">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="6f666-242">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="6f666-243">Aucune compression ne doit être effectuée.</span><span class="sxs-lookup"><span data-stu-id="6f666-243">No compression should be performed.</span></span> |
| [<span data-ttu-id="6f666-244">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="6f666-244">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="6f666-245">Les réponses doivent être compressées optimale, même si la compression prend plus de temps.</span><span class="sxs-lookup"><span data-stu-id="6f666-245">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="custom-providers"></a><span data-ttu-id="6f666-246">Fournisseurs personnalisés</span><span class="sxs-lookup"><span data-stu-id="6f666-246">Custom providers</span></span>

<span data-ttu-id="6f666-247">Créer des implémentations de compression personnalisé avec <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span><span class="sxs-lookup"><span data-stu-id="6f666-247">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="6f666-248"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> représente le contenu que cet encodage `ICompressionProvider` produit.</span><span class="sxs-lookup"><span data-stu-id="6f666-248">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="6f666-249">L’intergiciel utilise ces informations pour choisir le fournisseur en fonction de la liste spécifiée dans l'en-tête `Accept-Encoding` de la requête.</span><span class="sxs-lookup"><span data-stu-id="6f666-249">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="6f666-250">À l’aide de l’exemple d’application, le client envoie une requête avec l'en-tête `Accept-Encoding: mycustomcompression`.</span><span class="sxs-lookup"><span data-stu-id="6f666-250">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="6f666-251">L’intergiciel utilise l’implémentation de compression personnalisée et retourne la réponse avec un en-tête `Content-Encoding: mycustomcompression`.</span><span class="sxs-lookup"><span data-stu-id="6f666-251">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="6f666-252">Pour que cette implémentation fonctionne, le client doit pouvoir décompresser l’encodage personnalisé.</span><span class="sxs-lookup"><span data-stu-id="6f666-252">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

```csharp
public class CustomCompressionProvider : ICompressionProvider
{
    public string EncodingName => "mycustomcompression";
    public bool SupportsFlush => true;

    public Stream CreateStream(Stream outputStream)
    {
        // Create a custom compression stream wrapper here
        return outputStream;
    }
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=6,12-15)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6,12-15)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6f666-253">Soumettez une requête à l’exemple d’application avec l’en-tête `Accept-Encoding: mycustomcompression` et observez les en-têtes de réponse.</span><span class="sxs-lookup"><span data-stu-id="6f666-253">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="6f666-254">Les en-têtes `Vary` et `Content-Encoding` sont présents sur la réponse.</span><span class="sxs-lookup"><span data-stu-id="6f666-254">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="6f666-255">Le corps de la réponse (non affiché) n’est pas compressé par l’exemple.</span><span class="sxs-lookup"><span data-stu-id="6f666-255">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="6f666-256">Il n’y a pas d’implémentation de compression dans la classe `CustomCompressionProvider` de l’exemple.</span><span class="sxs-lookup"><span data-stu-id="6f666-256">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="6f666-257">Toutefois, ce dernier montre où vous pouvez implémenter un tel algorithme de compression.</span><span class="sxs-lookup"><span data-stu-id="6f666-257">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Fenêtre Fiddler montrant le résultat d’une demande avec l’en-tête Accept-Encoding et la valeur mycustomcompression.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="6f666-260">types MIME</span><span class="sxs-lookup"><span data-stu-id="6f666-260">MIME types</span></span>

<span data-ttu-id="6f666-261">L’intergiciel (middleware) spécifie un ensemble de types MIME pour la compression par défaut :</span><span class="sxs-lookup"><span data-stu-id="6f666-261">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="6f666-262">Remplacer ou ajouter des types MIME avec les options de l’intergiciel de Compression des réponses.</span><span class="sxs-lookup"><span data-stu-id="6f666-262">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="6f666-263">Notez que les caractères génériques MIME types, tels que `text/*` ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="6f666-263">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="6f666-264">L’exemple d’application ajoute un type MIME pour `image/svg+xml` et compresse et sert à ASP.NET Core image de bannière (*banner.svg*).</span><span class="sxs-lookup"><span data-stu-id="6f666-264">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7-9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="6f666-265">Compression avec protocole sécurisé</span><span class="sxs-lookup"><span data-stu-id="6f666-265">Compression with secure protocol</span></span>

<span data-ttu-id="6f666-266">Les réponses compressées sur des connexions sécurisées peuvent être contrôlées à l'aide de l'option `EnableForHttps` (désactivée par défaut).</span><span class="sxs-lookup"><span data-stu-id="6f666-266">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="6f666-267">L’utilisation de la compression avec des pages générées dynamiquement peut mener à des problèmes de sécurité tels que les attaques [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) et [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)).</span><span class="sxs-lookup"><span data-stu-id="6f666-267">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="6f666-268">Ajout de l’en-tête Vary</span><span class="sxs-lookup"><span data-stu-id="6f666-268">Adding the Vary header</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6f666-269">Lors de la compression des réponses basés sur le `Accept-Encoding` en-tête, il existe potentiellement plusieurs versions compressées de la réponse et une version non compressée.</span><span class="sxs-lookup"><span data-stu-id="6f666-269">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="6f666-270">Pour indiquer les caches de client et de proxy que plusieurs versions existent et doivent être stockées, les `Vary` en-tête est ajouté avec un `Accept-Encoding` valeur.</span><span class="sxs-lookup"><span data-stu-id="6f666-270">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="6f666-271">Dans ASP.NET Core 2.0 ou version ultérieure, l’intergiciel (middleware) ajoute le `Vary` en-tête automatiquement lors de la réponse est compressée.</span><span class="sxs-lookup"><span data-stu-id="6f666-271">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6f666-272">Lors de la compression des réponses basés sur le `Accept-Encoding` en-tête, il existe potentiellement plusieurs versions compressées de la réponse et une version non compressée.</span><span class="sxs-lookup"><span data-stu-id="6f666-272">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="6f666-273">Pour indiquer les caches de client et de proxy que plusieurs versions existent et doivent être stockées, les `Vary` en-tête est ajouté avec un `Accept-Encoding` valeur.</span><span class="sxs-lookup"><span data-stu-id="6f666-273">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="6f666-274">Dans ASP.NET Core 1.x, ajout de la `Vary` en-tête à la réponse s’effectue manuellement :</span><span class="sxs-lookup"><span data-stu-id="6f666-274">In ASP.NET Core 1.x, adding the `Vary` header to the response is accomplished manually:</span></span>

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="6f666-275">Problème au niveau de l’intergiciel s'il est derrière un proxy inverse Nginx</span><span class="sxs-lookup"><span data-stu-id="6f666-275">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="6f666-276">Quand une requête est transmise par Nginx, l'en-tête `Accept-Encoding` est supprimé.</span><span class="sxs-lookup"><span data-stu-id="6f666-276">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="6f666-277">L’intergiciel ne peut donc pas compresser la réponse.</span><span class="sxs-lookup"><span data-stu-id="6f666-277">This prevents the middleware from compressing the response.</span></span> <span data-ttu-id="6f666-278">Pour plus d’informations, consultez [NGINX : Compression et décompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="6f666-278">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="6f666-279">Ce problème est suivi par [Comprendre la compression pass-through pour Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="6f666-279">This issue is tracked by [Figure out pass-through compression for Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="6f666-280">Utilisation de la compression dynamique IIS</span><span class="sxs-lookup"><span data-stu-id="6f666-280">Working with IIS dynamic compression</span></span>

<span data-ttu-id="6f666-281">Si vous avez une active IIS Compression Module dynamique configuré au niveau du serveur que vous souhaitez désactiver pour une application, désactiver le module avec un ajout à la *web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="6f666-281">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="6f666-282">Pour plus d’informations, consultez [Désactivation de modules IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="6f666-282">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="6f666-283">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="6f666-283">Troubleshooting</span></span>

<span data-ttu-id="6f666-284">Utiliser un outil tel que [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), ou [Postman](https://www.getpostman.com/), qui vous permettent de définir le `Accept-Encoding` en-tête de demande et d’étudier les en-têtes de réponse, la taille et le corps.</span><span class="sxs-lookup"><span data-stu-id="6f666-284">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="6f666-285">Par défaut, intergiciel de Compression des réponses compresse les réponses qui remplissent les conditions suivantes :</span><span class="sxs-lookup"><span data-stu-id="6f666-285">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="6f666-286">Le `Accept-Encoding` en-tête est présent avec la valeur `br`, `gzip`, `*`, ou de l’encodage personnalisé qui correspond à un fournisseur de compression personnalisé que vous avez établi.</span><span class="sxs-lookup"><span data-stu-id="6f666-286">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="6f666-287">La valeur ne doit pas être `identity` ou avoir une valeur de qualité (qvalue, `q`) égale à 0 (zéro).</span><span class="sxs-lookup"><span data-stu-id="6f666-287">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="6f666-288">Le type MIME (`Content-Type`) doit être défini et doit correspondre à un type MIME configuré sur le <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="6f666-288">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="6f666-289">La requête ne doit pas inclure l'en-tête `Content-Range`.</span><span class="sxs-lookup"><span data-stu-id="6f666-289">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="6f666-290">La requête doit utiliser un protocole non sécurisé (http), sauf si un protocole sécurisé (https) est configuré dans les options de l’intergiciel de compression de la réponse.</span><span class="sxs-lookup"><span data-stu-id="6f666-290">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="6f666-291">*Notez le danger [décrit ci-dessus](#compression-with-secure-protocol) lors de l’activation de la compression de contenu sécurisée.*</span><span class="sxs-lookup"><span data-stu-id="6f666-291">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="6f666-292">L'en-tête `Accept-Encoding` est présent avec la valeur `gzip`, `*`, ou un encodage personnalisé qui correspond à un fournisseur de compression personnalisé que vous avez établi.</span><span class="sxs-lookup"><span data-stu-id="6f666-292">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="6f666-293">La valeur ne doit pas être `identity` ou avoir une valeur de qualité (qvalue, `q`) égale à 0 (zéro).</span><span class="sxs-lookup"><span data-stu-id="6f666-293">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="6f666-294">Le type MIME (`Content-Type`) doit être défini et doit correspondre à un type MIME configuré sur le <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="6f666-294">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="6f666-295">La requête ne doit pas inclure l'en-tête `Content-Range`.</span><span class="sxs-lookup"><span data-stu-id="6f666-295">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="6f666-296">La requête doit utiliser un protocole non sécurisé (http), sauf si un protocole sécurisé (https) est configuré dans les options de l’intergiciel de compression de la réponse.</span><span class="sxs-lookup"><span data-stu-id="6f666-296">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="6f666-297">*Notez le danger [décrit ci-dessus](#compression-with-secure-protocol) lors de l’activation de la compression de contenu sécurisée.*</span><span class="sxs-lookup"><span data-stu-id="6f666-297">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="6f666-298">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="6f666-298">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="6f666-299">Réseau de développeurs de Mozilla : Encodage</span><span class="sxs-lookup"><span data-stu-id="6f666-299">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="6f666-300">RFC 7231 relative aux Section 3.1.2.1 : Codings contenus</span><span class="sxs-lookup"><span data-stu-id="6f666-300">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="6f666-301">RFC 7230 Section 4.2.3 : Codage de Gzip</span><span class="sxs-lookup"><span data-stu-id="6f666-301">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="6f666-302">Version de spécification du format fichier GZIP 4.3</span><span class="sxs-lookup"><span data-stu-id="6f666-302">GZIP file format specification version 4.3</span></span>](http://www.ietf.org/rfc/rfc1952.txt)
