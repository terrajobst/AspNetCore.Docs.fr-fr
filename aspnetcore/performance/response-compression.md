---
title: Compression des réponses dans ASP.NET Core
author: guardrex
description: Découvrez ce qu’est la compression des réponses et comment utiliser le middleware de compression des réponses dans les applications ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: performance/response-compression
ms.openlocfilehash: d5d2da3dc0a8a452de97d98161d429389d2f7638
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815617"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="a7fba-103">Compression des réponses dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a7fba-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="a7fba-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a7fba-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a7fba-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a7fba-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a7fba-106">La bande passante réseau est une ressource limitée.</span><span class="sxs-lookup"><span data-stu-id="a7fba-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="a7fba-107">Le fait de réduire la taille de la réponse augmente généralement la réactivité d’une application, parfois de manière considérable.</span><span class="sxs-lookup"><span data-stu-id="a7fba-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="a7fba-108">Une façon de réduire la taille de la charge utile consiste à compresser les réponses de l’application.</span><span class="sxs-lookup"><span data-stu-id="a7fba-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="a7fba-109">Quand utiliser l’intergiciel (middleware) de compression de réponse</span><span class="sxs-lookup"><span data-stu-id="a7fba-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="a7fba-110">Utilisez les technologies de compression de réponse basées sur le serveur dans IIS, Apache ou Nginx.</span><span class="sxs-lookup"><span data-stu-id="a7fba-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="a7fba-111">Les performances de l’intergiciel (middleware) ne correspondront probablement pas à celles des modules de serveur.</span><span class="sxs-lookup"><span data-stu-id="a7fba-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="a7fba-112">[Serveur HTTP.sys](xref:fundamentals/servers/httpsys) serveur et [Kestrel](xref:fundamentals/servers/kestrel) serveur n’offrent actuellement pas prise en charge de la compression intégrée.</span><span class="sxs-lookup"><span data-stu-id="a7fba-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="a7fba-113">Utilisez l’intergiciel (middleware) de compression de réponse dans les cas suivants :</span><span class="sxs-lookup"><span data-stu-id="a7fba-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="a7fba-114">Impossibilité d’utiliser les technologies de compression suivantes basées sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="a7fba-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="a7fba-115">Module de compression dynamique IIS</span><span class="sxs-lookup"><span data-stu-id="a7fba-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="a7fba-116">Module Apache mod_deflate</span><span class="sxs-lookup"><span data-stu-id="a7fba-116">Apache mod_deflate module</span></span>](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="a7fba-117">Décompression et compression Nginx</span><span class="sxs-lookup"><span data-stu-id="a7fba-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="a7fba-118">Hébergement directement sur :</span><span class="sxs-lookup"><span data-stu-id="a7fba-118">Hosting directly on:</span></span>
  * <span data-ttu-id="a7fba-119">[Serveur HTTP.sys](xref:fundamentals/servers/httpsys) (anciennement appelé WebListener)</span><span class="sxs-lookup"><span data-stu-id="a7fba-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="a7fba-120">Serveur kestrel</span><span class="sxs-lookup"><span data-stu-id="a7fba-120">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="a7fba-121">Compression des réponses</span><span class="sxs-lookup"><span data-stu-id="a7fba-121">Response compression</span></span>

<span data-ttu-id="a7fba-122">En règle générale, toute réponse ne compressée pas en mode natif peut bénéficier de la compression de réponse.</span><span class="sxs-lookup"><span data-stu-id="a7fba-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="a7fba-123">Pas en mode natif compressés en général, les réponses sont : CSS, JavaScript, HTML, XML et JSON.</span><span class="sxs-lookup"><span data-stu-id="a7fba-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="a7fba-124">Vous ne doivent pas compresser des ressources compressées en mode natif, telles que des fichiers PNG.</span><span class="sxs-lookup"><span data-stu-id="a7fba-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="a7fba-125">Si vous tentez de compresser davantage une réponse compressée en mode natif, une petite réduction supplémentaire dans le temps de taille et la transmission sera probablement être été éclipsée par le temps de traitement de la compression.</span><span class="sxs-lookup"><span data-stu-id="a7fba-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="a7fba-126">Ne pas compresser les fichiers inférieurs à environ 150-1000 octets (selon le contenu du fichier et l’efficacité de la compression).</span><span class="sxs-lookup"><span data-stu-id="a7fba-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="a7fba-127">La surcharge de la compression des fichiers de petite taille peut produire un fichier compressé plus volumineux que le fichier non compressé.</span><span class="sxs-lookup"><span data-stu-id="a7fba-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="a7fba-128">Quand un client peut traiter du contenu compressé, il doit en informer le serveur en envoyant l'en-tête `Accept-Encoding` avec la requête.</span><span class="sxs-lookup"><span data-stu-id="a7fba-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="a7fba-129">Quand un serveur envoie du contenu compressé, il doit inclure dans l’en-tête `Content-Encoding` des informations sur le mode d’encodage de la réponse compressée.</span><span class="sxs-lookup"><span data-stu-id="a7fba-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="a7fba-130">Les désignations d'encodage de contenu prises en charge par l’intergiciel sont affichées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="a7fba-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="a7fba-131">Valeurs d’en-tête `Accept-Encoding`</span><span class="sxs-lookup"><span data-stu-id="a7fba-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="a7fba-132">Intergiciel pris en charge</span><span class="sxs-lookup"><span data-stu-id="a7fba-132">Middleware Supported</span></span> | <span data-ttu-id="a7fba-133">Description</span><span class="sxs-lookup"><span data-stu-id="a7fba-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="a7fba-134">Oui (valeur par défaut)</span><span class="sxs-lookup"><span data-stu-id="a7fba-134">Yes (default)</span></span>        | [<span data-ttu-id="a7fba-135">Format de données compressées Brotli</span><span class="sxs-lookup"><span data-stu-id="a7fba-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="a7fba-136">Non</span><span class="sxs-lookup"><span data-stu-id="a7fba-136">No</span></span>                   | [<span data-ttu-id="a7fba-137">Format de données compressées DEFLATE</span><span class="sxs-lookup"><span data-stu-id="a7fba-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="a7fba-138">Non</span><span class="sxs-lookup"><span data-stu-id="a7fba-138">No</span></span>                   | [<span data-ttu-id="a7fba-139">Échange efficace de XML W3C</span><span class="sxs-lookup"><span data-stu-id="a7fba-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="a7fba-140">Oui</span><span class="sxs-lookup"><span data-stu-id="a7fba-140">Yes</span></span>                  | [<span data-ttu-id="a7fba-141">Format de fichier GZIP</span><span class="sxs-lookup"><span data-stu-id="a7fba-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="a7fba-142">Oui</span><span class="sxs-lookup"><span data-stu-id="a7fba-142">Yes</span></span>                  | <span data-ttu-id="a7fba-143">Identificateur « Aucun codage » : La réponse ne doit pas être encodée.</span><span class="sxs-lookup"><span data-stu-id="a7fba-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="a7fba-144">Non</span><span class="sxs-lookup"><span data-stu-id="a7fba-144">No</span></span>                   | [<span data-ttu-id="a7fba-145">Format de transfert de réseau d’Archives Java</span><span class="sxs-lookup"><span data-stu-id="a7fba-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="a7fba-146">Oui</span><span class="sxs-lookup"><span data-stu-id="a7fba-146">Yes</span></span>                  | <span data-ttu-id="a7fba-147">N'importe quel encodage de contenu disponible non explicitement demandé</span><span class="sxs-lookup"><span data-stu-id="a7fba-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="a7fba-148">Valeurs d’en-tête `Accept-Encoding`</span><span class="sxs-lookup"><span data-stu-id="a7fba-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="a7fba-149">Intergiciel pris en charge</span><span class="sxs-lookup"><span data-stu-id="a7fba-149">Middleware Supported</span></span> | <span data-ttu-id="a7fba-150">Description</span><span class="sxs-lookup"><span data-stu-id="a7fba-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="a7fba-151">Non</span><span class="sxs-lookup"><span data-stu-id="a7fba-151">No</span></span>                   | [<span data-ttu-id="a7fba-152">Format de données compressées Brotli</span><span class="sxs-lookup"><span data-stu-id="a7fba-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="a7fba-153">Non</span><span class="sxs-lookup"><span data-stu-id="a7fba-153">No</span></span>                   | [<span data-ttu-id="a7fba-154">Format de données compressées DEFLATE</span><span class="sxs-lookup"><span data-stu-id="a7fba-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="a7fba-155">Non</span><span class="sxs-lookup"><span data-stu-id="a7fba-155">No</span></span>                   | [<span data-ttu-id="a7fba-156">Échange efficace de XML W3C</span><span class="sxs-lookup"><span data-stu-id="a7fba-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="a7fba-157">Oui (valeur par défaut)</span><span class="sxs-lookup"><span data-stu-id="a7fba-157">Yes (default)</span></span>        | [<span data-ttu-id="a7fba-158">Format de fichier GZIP</span><span class="sxs-lookup"><span data-stu-id="a7fba-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="a7fba-159">Oui</span><span class="sxs-lookup"><span data-stu-id="a7fba-159">Yes</span></span>                  | <span data-ttu-id="a7fba-160">Identificateur « Aucun codage » : La réponse ne doit pas être encodée.</span><span class="sxs-lookup"><span data-stu-id="a7fba-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="a7fba-161">Non</span><span class="sxs-lookup"><span data-stu-id="a7fba-161">No</span></span>                   | [<span data-ttu-id="a7fba-162">Format de transfert de réseau d’Archives Java</span><span class="sxs-lookup"><span data-stu-id="a7fba-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="a7fba-163">Oui</span><span class="sxs-lookup"><span data-stu-id="a7fba-163">Yes</span></span>                  | <span data-ttu-id="a7fba-164">N'importe quel encodage de contenu disponible non explicitement demandé</span><span class="sxs-lookup"><span data-stu-id="a7fba-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="a7fba-165">Pour plus d’informations, consultez la [liste IANA officielle des encodages de contenu](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="a7fba-165">For more information, see the [IANA Official Content Coding List](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="a7fba-166">L’intergiciel vous permet d’ajouter des fournisseurs de compression supplémentaires pour des valeurs d’en-tête `Accept-Encoding` personnalisées.</span><span class="sxs-lookup"><span data-stu-id="a7fba-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="a7fba-167">Pour plus d’informations, consultez [Fournisseurs personnalisés](#custom-providers) ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="a7fba-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="a7fba-168">L’intergiciel peut réagir à la pondération de la valeur de qualité (qvalue, `q`) envoyée par le client pour hiérarchiser les schémas de compression.</span><span class="sxs-lookup"><span data-stu-id="a7fba-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="a7fba-169">Pour plus d’informations, consultez [7231 relative aux RFC : Encodage](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="a7fba-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="a7fba-170">Les algorithmes de compression donnent lieu à un compromis entre vitesse de compression et efficacité de la compression.</span><span class="sxs-lookup"><span data-stu-id="a7fba-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="a7fba-171">L’*Efficacité* dans ce contexte fait référence à la taille de la sortie après la compression.</span><span class="sxs-lookup"><span data-stu-id="a7fba-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="a7fba-172">La plus petite taille est obtenue par la compression la plus *optimale*.</span><span class="sxs-lookup"><span data-stu-id="a7fba-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="a7fba-173">Les en-têtes impliqués dans la demande, l'envoi, la mise en cache et la réception de contenu compressé sont décrits dans le tableau ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="a7fba-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="a7fba-174">Header</span><span class="sxs-lookup"><span data-stu-id="a7fba-174">Header</span></span>             | <span data-ttu-id="a7fba-175">Rôle</span><span class="sxs-lookup"><span data-stu-id="a7fba-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="a7fba-176">Envoyé à partir du client au serveur pour indiquer les schémas d’encodage de contenu acceptables pour le client.</span><span class="sxs-lookup"><span data-stu-id="a7fba-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="a7fba-177">Envoyé à partir du serveur au client pour indiquer l'encodage du contenu de la charge utile.</span><span class="sxs-lookup"><span data-stu-id="a7fba-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="a7fba-178">Au moment de la compression, l'en-tête `Content-Length` est supprimé car le contenu du corps change quand la réponse est compressée.</span><span class="sxs-lookup"><span data-stu-id="a7fba-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="a7fba-179">Durant la compression, l’en-tête `Content-MD5` est supprimé puisque le contenu du corps a changé et que le hachage n’est plus valide.</span><span class="sxs-lookup"><span data-stu-id="a7fba-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="a7fba-180">Spécifie le type MIME du contenu.</span><span class="sxs-lookup"><span data-stu-id="a7fba-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="a7fba-181">Chaque réponse doit spécifier son `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="a7fba-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="a7fba-182">L’intergiciel vérifie cette valeur pour déterminer si la réponse doit être compressée.</span><span class="sxs-lookup"><span data-stu-id="a7fba-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="a7fba-183">L’intergiciel (middleware) spécifie un ensemble de [par défaut des types MIME](#mime-types) qui peut donc coder, mais vous pouvez remplacer ou ajouter des types MIME.</span><span class="sxs-lookup"><span data-stu-id="a7fba-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="a7fba-184">Quand l’en-tête `Vary` est envoyé par le serveur avec la valeur `Accept-Encoding` à des clients et proxys, ces derniers doivent mettre en cache les réponses (vary) en fonction de la valeur de l’en-tête `Accept-Encoding` de la requête.</span><span class="sxs-lookup"><span data-stu-id="a7fba-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="a7fba-185">Si du contenu est retourné avec l'en-tête `Vary: Accept-Encoding`, les réponses (compressées et non compressées) sont mises en cache séparément.</span><span class="sxs-lookup"><span data-stu-id="a7fba-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="a7fba-186">Explorez les fonctionnalités de l’intergiciel de Compression de réponse avec le [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="a7fba-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="a7fba-187">L’exemple illustre :</span><span class="sxs-lookup"><span data-stu-id="a7fba-187">The sample illustrates:</span></span>

* <span data-ttu-id="a7fba-188">La compression des réponses d’application à l’aide de Gzip et fournisseurs de compression personnalisé.</span><span class="sxs-lookup"><span data-stu-id="a7fba-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="a7fba-189">Comment ajouter un type MIME à la liste par défaut des types MIME pour la compression.</span><span class="sxs-lookup"><span data-stu-id="a7fba-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="a7fba-190">Package</span><span class="sxs-lookup"><span data-stu-id="a7fba-190">Package</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a7fba-191">Pour inclure l’intergiciel (middleware) dans un projet, ajoutez une référence à la [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app), qui inclut le [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span><span class="sxs-lookup"><span data-stu-id="a7fba-191">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="a7fba-192">Pour inclure l’intergiciel (middleware) dans un projet, ajoutez une référence à la [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage), qui inclut le [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span><span class="sxs-lookup"><span data-stu-id="a7fba-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="a7fba-193">Pour inclure l’intergiciel (middleware) dans un projet, ajoutez une référence à la [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span><span class="sxs-lookup"><span data-stu-id="a7fba-193">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="a7fba-194">Configuration</span><span class="sxs-lookup"><span data-stu-id="a7fba-194">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a7fba-195">Le code suivant montre comment activer le Middleware de Compression de réponse pour les types MIME par défaut et les fournisseurs de compression ([Brotli](#brotli-compression-provider) et [Gzip](#gzip-compression-provider)) :</span><span class="sxs-lookup"><span data-stu-id="a7fba-195">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a7fba-196">Le code suivant montre comment activer le Middleware de Compression de réponse pour les types MIME par défaut et le [fournisseur de Compression Gzip](#gzip-compression-provider):</span><span class="sxs-lookup"><span data-stu-id="a7fba-196">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

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

<span data-ttu-id="a7fba-197">Remarques :</span><span class="sxs-lookup"><span data-stu-id="a7fba-197">Notes:</span></span>

* <span data-ttu-id="a7fba-198">`app.UseResponseCompression` doit être appelé avant `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="a7fba-198">`app.UseResponseCompression` must be called before `app.UseMvc`.</span></span>
* <span data-ttu-id="a7fba-199">Utiliser un outil tel que [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), ou [Postman](https://www.getpostman.com/) pour définir le `Accept-Encoding` en-tête de demande et d’étudier les en-têtes de réponse, la taille et le corps.</span><span class="sxs-lookup"><span data-stu-id="a7fba-199">Use a tool such as [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="a7fba-200">Soumettez une requête à l’exemple d’application sans l’en-tête `Accept-Encoding` et observez que la réponse n’est pas compressée.</span><span class="sxs-lookup"><span data-stu-id="a7fba-200">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="a7fba-201">Les en-têtes  `Content-Encoding` et `Vary` ne sont pas présents sur la réponse.</span><span class="sxs-lookup"><span data-stu-id="a7fba-201">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Fenêtre Fiddler indiquant le résultat d’une requête sans l’en-tête Accept-Encoding.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a7fba-204">Soumettre une demande de l’exemple d’application avec le `Accept-Encoding: br` en-tête (compression Brotli) et observez que la réponse est compressée.</span><span class="sxs-lookup"><span data-stu-id="a7fba-204">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="a7fba-205">Les en-têtes `Content-Encoding` et `Vary` sont présents sur la réponse.</span><span class="sxs-lookup"><span data-stu-id="a7fba-205">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Fenêtre Fiddler montrant le résultat d’une demande avec l’en-tête Accept-Encoding et la valeur br.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a7fba-209">Soumettez une requête à l’exemple d’application avec l’en-tête `Accept-Encoding: gzip` et observez que la réponse est compressée.</span><span class="sxs-lookup"><span data-stu-id="a7fba-209">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="a7fba-210">Les en-têtes `Content-Encoding` et `Vary` sont présents sur la réponse.</span><span class="sxs-lookup"><span data-stu-id="a7fba-210">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Fenêtre Fiddler montrant le résultat d’une demande avec l’en-tête Accept-Encoding et la valeur gzip.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a><span data-ttu-id="a7fba-214">Fournisseurs</span><span class="sxs-lookup"><span data-stu-id="a7fba-214">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="a7fba-215">Fournisseur de Compression Brotli</span><span class="sxs-lookup"><span data-stu-id="a7fba-215">Brotli Compression Provider</span></span>

<span data-ttu-id="a7fba-216">Utilisez le <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> pour compresser les réponses avec le [format de données compressées Brotli](https://tools.ietf.org/html/rfc7932).</span><span class="sxs-lookup"><span data-stu-id="a7fba-216">Use the <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="a7fba-217">Si aucun fournisseur de compression n’est ajoutés explicitement à la <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="a7fba-217">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="a7fba-218">Le fournisseur de Compression Brotli est ajouté par défaut dans le tableau de fournisseurs de compression avec la [fournisseur de compression Gzip](#gzip-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="a7fba-218">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="a7fba-219">La compression par défaut de compression Brotli lorsque le format de données compressées Brotli est pris en charge par le client.</span><span class="sxs-lookup"><span data-stu-id="a7fba-219">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="a7fba-220">Si Brotli n’est pas pris en charge par le client, la compression par défaut Gzip lorsque le client prend en charge la compression Gzip.</span><span class="sxs-lookup"><span data-stu-id="a7fba-220">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="a7fba-221">Le fournisseur de Compression Brotoli doit être ajouté lorsque des fournisseurs de compression sont ajoutés explicitement :</span><span class="sxs-lookup"><span data-stu-id="a7fba-221">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="a7fba-222">Définir la compression au niveau avec <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="a7fba-222">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span></span> <span data-ttu-id="a7fba-223">Le fournisseur de Compression Brotli par défaut est le niveau de compression plus rapide ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), ce qui ne peut pas produire la compression la plus efficace.</span><span class="sxs-lookup"><span data-stu-id="a7fba-223">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="a7fba-224">Si la compression la plus efficace est souhaitée, configurez l’intergiciel de compression optimale.</span><span class="sxs-lookup"><span data-stu-id="a7fba-224">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="a7fba-225">Niveau de compression</span><span class="sxs-lookup"><span data-stu-id="a7fba-225">Compression Level</span></span> | <span data-ttu-id="a7fba-226">Description</span><span class="sxs-lookup"><span data-stu-id="a7fba-226">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="a7fba-227">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="a7fba-227">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="a7fba-228">La compression doit se terminer aussi rapidement que possible, même si le résultat n’est pas compressé de manière optimale.</span><span class="sxs-lookup"><span data-stu-id="a7fba-228">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="a7fba-229">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="a7fba-229">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="a7fba-230">Aucune compression ne doit être effectuée.</span><span class="sxs-lookup"><span data-stu-id="a7fba-230">No compression should be performed.</span></span> |
| [<span data-ttu-id="a7fba-231">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="a7fba-231">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="a7fba-232">Les réponses doivent être compressées optimale, même si la compression prend plus de temps.</span><span class="sxs-lookup"><span data-stu-id="a7fba-232">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="gzip-compression-provider"></a><span data-ttu-id="a7fba-233">Fournisseur de Compression GZIP</span><span class="sxs-lookup"><span data-stu-id="a7fba-233">Gzip Compression Provider</span></span>

<span data-ttu-id="a7fba-234">Utilisez le <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> pour compresser les réponses avec le [format de fichier Gzip](https://tools.ietf.org/html/rfc1952).</span><span class="sxs-lookup"><span data-stu-id="a7fba-234">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="a7fba-235">Si aucun fournisseur de compression n’est ajoutés explicitement à la <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="a7fba-235">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="a7fba-236">Le fournisseur de la Compression Gzip est ajouté par défaut dans le tableau de fournisseurs de compression avec la [fournisseur de Compression Brotli](#brotli-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="a7fba-236">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="a7fba-237">La compression par défaut de compression Brotli lorsque le format de données compressées Brotli est pris en charge par le client.</span><span class="sxs-lookup"><span data-stu-id="a7fba-237">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="a7fba-238">Si Brotli n’est pas pris en charge par le client, la compression par défaut Gzip lorsque le client prend en charge la compression Gzip.</span><span class="sxs-lookup"><span data-stu-id="a7fba-238">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="a7fba-239">Le fournisseur de la Compression Gzip est ajouté au tableau de fournisseurs de compression par défaut.</span><span class="sxs-lookup"><span data-stu-id="a7fba-239">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="a7fba-240">Par défaut, la compression est Gzip lorsque le client prend en charge la compression Gzip.</span><span class="sxs-lookup"><span data-stu-id="a7fba-240">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="a7fba-241">Le fournisseur de la Compression Gzip doit être ajouté lorsque des fournisseurs de compression sont ajoutés explicitement :</span><span class="sxs-lookup"><span data-stu-id="a7fba-241">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5)]

::: moniker-end

<span data-ttu-id="a7fba-242">Définir la compression au niveau avec <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="a7fba-242">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="a7fba-243">Le fournisseur de la Compression Gzip par défaut est le niveau de compression plus rapide ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), ce qui ne peut pas produire la compression la plus efficace.</span><span class="sxs-lookup"><span data-stu-id="a7fba-243">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="a7fba-244">Si la compression la plus efficace est souhaitée, configurez l’intergiciel de compression optimale.</span><span class="sxs-lookup"><span data-stu-id="a7fba-244">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="a7fba-245">Niveau de compression</span><span class="sxs-lookup"><span data-stu-id="a7fba-245">Compression Level</span></span> | <span data-ttu-id="a7fba-246">Description</span><span class="sxs-lookup"><span data-stu-id="a7fba-246">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="a7fba-247">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="a7fba-247">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="a7fba-248">La compression doit se terminer aussi rapidement que possible, même si le résultat n’est pas compressé de manière optimale.</span><span class="sxs-lookup"><span data-stu-id="a7fba-248">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="a7fba-249">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="a7fba-249">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="a7fba-250">Aucune compression ne doit être effectuée.</span><span class="sxs-lookup"><span data-stu-id="a7fba-250">No compression should be performed.</span></span> |
| [<span data-ttu-id="a7fba-251">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="a7fba-251">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="a7fba-252">Les réponses doivent être compressées optimale, même si la compression prend plus de temps.</span><span class="sxs-lookup"><span data-stu-id="a7fba-252">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="custom-providers"></a><span data-ttu-id="a7fba-253">Fournisseurs personnalisés</span><span class="sxs-lookup"><span data-stu-id="a7fba-253">Custom providers</span></span>

<span data-ttu-id="a7fba-254">Créer des implémentations de compression personnalisé avec <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span><span class="sxs-lookup"><span data-stu-id="a7fba-254">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="a7fba-255"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> représente le contenu que cet encodage `ICompressionProvider` produit.</span><span class="sxs-lookup"><span data-stu-id="a7fba-255">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="a7fba-256">L’intergiciel utilise ces informations pour choisir le fournisseur en fonction de la liste spécifiée dans l'en-tête `Accept-Encoding` de la requête.</span><span class="sxs-lookup"><span data-stu-id="a7fba-256">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="a7fba-257">À l’aide de l’exemple d’application, le client envoie une requête avec l'en-tête `Accept-Encoding: mycustomcompression`.</span><span class="sxs-lookup"><span data-stu-id="a7fba-257">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="a7fba-258">L’intergiciel utilise l’implémentation de compression personnalisée et retourne la réponse avec un en-tête `Content-Encoding: mycustomcompression`.</span><span class="sxs-lookup"><span data-stu-id="a7fba-258">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="a7fba-259">Pour que cette implémentation fonctionne, le client doit pouvoir décompresser l’encodage personnalisé.</span><span class="sxs-lookup"><span data-stu-id="a7fba-259">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="a7fba-260">Soumettez une requête à l’exemple d’application avec l’en-tête `Accept-Encoding: mycustomcompression` et observez les en-têtes de réponse.</span><span class="sxs-lookup"><span data-stu-id="a7fba-260">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="a7fba-261">Les en-têtes `Vary` et `Content-Encoding` sont présents sur la réponse.</span><span class="sxs-lookup"><span data-stu-id="a7fba-261">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="a7fba-262">Le corps de la réponse (non affiché) n’est pas compressé par l’exemple.</span><span class="sxs-lookup"><span data-stu-id="a7fba-262">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="a7fba-263">Il n’y a pas d’implémentation de compression dans la classe `CustomCompressionProvider` de l’exemple.</span><span class="sxs-lookup"><span data-stu-id="a7fba-263">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="a7fba-264">Toutefois, ce dernier montre où vous pouvez implémenter un tel algorithme de compression.</span><span class="sxs-lookup"><span data-stu-id="a7fba-264">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Fenêtre Fiddler montrant le résultat d’une demande avec l’en-tête Accept-Encoding et la valeur mycustomcompression.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="a7fba-267">types MIME</span><span class="sxs-lookup"><span data-stu-id="a7fba-267">MIME types</span></span>

<span data-ttu-id="a7fba-268">L’intergiciel (middleware) spécifie un ensemble de types MIME pour la compression par défaut :</span><span class="sxs-lookup"><span data-stu-id="a7fba-268">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="a7fba-269">Remplacer ou ajouter des types MIME avec les options de l’intergiciel de Compression des réponses.</span><span class="sxs-lookup"><span data-stu-id="a7fba-269">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="a7fba-270">Notez que les caractères génériques MIME types, tels que `text/*` ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="a7fba-270">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="a7fba-271">L’exemple d’application ajoute un type MIME pour `image/svg+xml` et compresse et sert à ASP.NET Core image de bannière (*banner.svg*).</span><span class="sxs-lookup"><span data-stu-id="a7fba-271">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="a7fba-272">Compression avec protocole sécurisé</span><span class="sxs-lookup"><span data-stu-id="a7fba-272">Compression with secure protocol</span></span>

<span data-ttu-id="a7fba-273">Les réponses compressées sur des connexions sécurisées peuvent être contrôlées à l'aide de l'option `EnableForHttps` (désactivée par défaut).</span><span class="sxs-lookup"><span data-stu-id="a7fba-273">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="a7fba-274">L’utilisation de la compression avec des pages générées dynamiquement peut mener à des problèmes de sécurité tels que les attaques [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) et [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)).</span><span class="sxs-lookup"><span data-stu-id="a7fba-274">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="a7fba-275">Ajout de l’en-tête Vary</span><span class="sxs-lookup"><span data-stu-id="a7fba-275">Adding the Vary header</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="a7fba-276">Lors de la compression des réponses basés sur le `Accept-Encoding` en-tête, il existe potentiellement plusieurs versions compressées de la réponse et une version non compressée.</span><span class="sxs-lookup"><span data-stu-id="a7fba-276">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="a7fba-277">Pour indiquer les caches de client et de proxy que plusieurs versions existent et doivent être stockées, les `Vary` en-tête est ajouté avec un `Accept-Encoding` valeur.</span><span class="sxs-lookup"><span data-stu-id="a7fba-277">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="a7fba-278">Dans ASP.NET Core 2.0 ou version ultérieure, l’intergiciel (middleware) ajoute le `Vary` en-tête automatiquement lors de la réponse est compressée.</span><span class="sxs-lookup"><span data-stu-id="a7fba-278">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="a7fba-279">Lors de la compression des réponses basés sur le `Accept-Encoding` en-tête, il existe potentiellement plusieurs versions compressées de la réponse et une version non compressée.</span><span class="sxs-lookup"><span data-stu-id="a7fba-279">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="a7fba-280">Pour indiquer les caches de client et de proxy que plusieurs versions existent et doivent être stockées, les `Vary` en-tête est ajouté avec un `Accept-Encoding` valeur.</span><span class="sxs-lookup"><span data-stu-id="a7fba-280">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="a7fba-281">Dans ASP.NET Core 1.x, ajout de la `Vary` en-tête à la réponse s’effectue manuellement :</span><span class="sxs-lookup"><span data-stu-id="a7fba-281">In ASP.NET Core 1.x, adding the `Vary` header to the response is accomplished manually:</span></span>

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="a7fba-282">Problème au niveau de l’intergiciel s'il est derrière un proxy inverse Nginx</span><span class="sxs-lookup"><span data-stu-id="a7fba-282">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="a7fba-283">Quand une requête est transmise par Nginx, l'en-tête `Accept-Encoding` est supprimé.</span><span class="sxs-lookup"><span data-stu-id="a7fba-283">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="a7fba-284">Suppression de la `Accept-Encoding` en-tête empêche l’intergiciel (middleware) de la compression de la réponse.</span><span class="sxs-lookup"><span data-stu-id="a7fba-284">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="a7fba-285">Pour plus d’informations, consultez [NGINX : La compression et décompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="a7fba-285">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="a7fba-286">Ce problème est suivi par [déterminer compression pass-through pour Nginx (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="a7fba-286">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="a7fba-287">Utilisation de la compression dynamique IIS</span><span class="sxs-lookup"><span data-stu-id="a7fba-287">Working with IIS dynamic compression</span></span>

<span data-ttu-id="a7fba-288">Si vous avez une active IIS Compression Module dynamique configuré au niveau du serveur que vous souhaitez désactiver pour une application, désactiver le module avec un ajout à la *web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="a7fba-288">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="a7fba-289">Pour plus d’informations, consultez [Désactivation de modules IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="a7fba-289">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="a7fba-290">Résolution de problèmes</span><span class="sxs-lookup"><span data-stu-id="a7fba-290">Troubleshooting</span></span>

<span data-ttu-id="a7fba-291">Utilisez un outil tel que [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/) ou [Postman](https://www.getpostman.com/) pour définir l'en-tête `Accept-Encoding` de la requête et étudier les en-têtes, la taille et le corps de la réponse.</span><span class="sxs-lookup"><span data-stu-id="a7fba-291">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="a7fba-292">Par défaut, intergiciel de Compression des réponses compresse les réponses qui remplissent les conditions suivantes :</span><span class="sxs-lookup"><span data-stu-id="a7fba-292">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="a7fba-293">Le `Accept-Encoding` en-tête est présent avec la valeur `br`, `gzip`, `*`, ou de l’encodage personnalisé qui correspond à un fournisseur de compression personnalisé que vous avez établi.</span><span class="sxs-lookup"><span data-stu-id="a7fba-293">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="a7fba-294">La valeur ne doit pas être `identity` ou avoir une valeur de qualité (qvalue, `q`) égale à 0 (zéro).</span><span class="sxs-lookup"><span data-stu-id="a7fba-294">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="a7fba-295">Le type MIME (`Content-Type`) doit être défini et doit correspondre à un type MIME configuré sur le <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="a7fba-295">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="a7fba-296">La requête ne doit pas inclure l'en-tête `Content-Range`.</span><span class="sxs-lookup"><span data-stu-id="a7fba-296">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="a7fba-297">La requête doit utiliser un protocole non sécurisé (http), sauf si un protocole sécurisé (https) est configuré dans les options de l’intergiciel de compression de la réponse.</span><span class="sxs-lookup"><span data-stu-id="a7fba-297">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="a7fba-298">*Notez le danger [décrit ci-dessus](#compression-with-secure-protocol) lors de l’activation de la compression de contenu sécurisée.*</span><span class="sxs-lookup"><span data-stu-id="a7fba-298">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="a7fba-299">L'en-tête `Accept-Encoding` est présent avec la valeur `gzip`, `*`, ou un encodage personnalisé qui correspond à un fournisseur de compression personnalisé que vous avez établi.</span><span class="sxs-lookup"><span data-stu-id="a7fba-299">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="a7fba-300">La valeur ne doit pas être `identity` ou avoir une valeur de qualité (qvalue, `q`) égale à 0 (zéro).</span><span class="sxs-lookup"><span data-stu-id="a7fba-300">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="a7fba-301">Le type MIME (`Content-Type`) doit être défini et doit correspondre à un type MIME configuré sur le <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="a7fba-301">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="a7fba-302">La requête ne doit pas inclure l'en-tête `Content-Range`.</span><span class="sxs-lookup"><span data-stu-id="a7fba-302">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="a7fba-303">La requête doit utiliser un protocole non sécurisé (http), sauf si un protocole sécurisé (https) est configuré dans les options de l’intergiciel de compression de la réponse.</span><span class="sxs-lookup"><span data-stu-id="a7fba-303">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="a7fba-304">*Notez le danger [décrit ci-dessus](#compression-with-secure-protocol) lors de l’activation de la compression de contenu sécurisée.*</span><span class="sxs-lookup"><span data-stu-id="a7fba-304">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="a7fba-305">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="a7fba-305">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="a7fba-306">Développeur de Mozilla réseau : Encodage</span><span class="sxs-lookup"><span data-stu-id="a7fba-306">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="a7fba-307">RFC 7231 relative aux Section 3.1.2.1 : Contenu Codings</span><span class="sxs-lookup"><span data-stu-id="a7fba-307">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="a7fba-308">RFC 7230 Section 4.2.3 : Codage gzip</span><span class="sxs-lookup"><span data-stu-id="a7fba-308">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="a7fba-309">Version de spécification du format fichier GZIP 4.3</span><span class="sxs-lookup"><span data-stu-id="a7fba-309">GZIP file format specification version 4.3</span></span>](https://www.ietf.org/rfc/rfc1952.txt)
