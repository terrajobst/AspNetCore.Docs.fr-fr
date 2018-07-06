---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: Négociation de l’API Web ASP.NET de contenu | Microsoft Docs
author: MikeWasson
description: Décrit la façon dont ASP.NET Web API implémente la négociation de contenu HTTP.
ms.author: aspnetcontent
ms.date: 05/20/2012
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: 2314a263a12c74e80c08391ae03425955a82458a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810315"
---
<a name="content-negotiation-in-aspnet-web-api"></a><span data-ttu-id="6be1b-103">Négociation de contenu dans l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6be1b-103">Content Negotiation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="6be1b-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6be1b-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="6be1b-105">Cet article décrit la façon dont ASP.NET Web API implémente la négociation de contenu.</span><span class="sxs-lookup"><span data-stu-id="6be1b-105">This article describes how ASP.NET Web API implements content negotiation.</span></span>

<span data-ttu-id="6be1b-106">La spécification HTTP (RFC 2616) définit la négociation de contenu en tant que « le processus de sélection de la meilleure représentation pour une réponse particulière lorsqu’il existe plusieurs représentations disponibles ».</span><span class="sxs-lookup"><span data-stu-id="6be1b-106">The HTTP specification (RFC 2616) defines content negotiation as "the process of selecting the best representation for a given response when there are multiple representations available."</span></span> <span data-ttu-id="6be1b-107">Le principal mécanisme de négociation de contenu HTTP sont ces en-têtes de demande :</span><span class="sxs-lookup"><span data-stu-id="6be1b-107">The primary mechanism for content negotiation in HTTP are these request headers:</span></span>

- <span data-ttu-id="6be1b-108">**Accepter :** quels types de médias sont acceptables pour la réponse, tels que « application/json », « application/xml », ou un type de média personnalisé tel que &quot;application/vnd.example+xml&quot;</span><span class="sxs-lookup"><span data-stu-id="6be1b-108">**Accept:** Which media types are acceptable for the response, such as "application/json," "application/xml," or a custom media type such as &quot;application/vnd.example+xml&quot;</span></span>
- <span data-ttu-id="6be1b-109">**Accept-Charset :** les jeux de caractères sont acceptables, telles que UTF-8 ou ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="6be1b-109">**Accept-Charset:** Which character sets are acceptable, such as UTF-8 or ISO 8859-1.</span></span>
- <span data-ttu-id="6be1b-110">**Encodage :** les encodages de contenu sont acceptables, tel que gzip.</span><span class="sxs-lookup"><span data-stu-id="6be1b-110">**Accept-Encoding:** Which content encodings are acceptable, such as gzip.</span></span>
- <span data-ttu-id="6be1b-111">**Accept-Language :** le langage naturel par défaut, tel que « en-us ».</span><span class="sxs-lookup"><span data-stu-id="6be1b-111">**Accept-Language:** The preferred natural language, such as "en-us".</span></span>

<span data-ttu-id="6be1b-112">Le serveur peut également consulter les autres parties de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="6be1b-112">The server can also look at other portions of the HTTP request.</span></span> <span data-ttu-id="6be1b-113">Par exemple, si la demande contient un en-tête X-Requested-With, indiquant une requête AJAX, le serveur peuvent être par défaut au format JSON s’il n’existe aucun en-tête Accept.</span><span class="sxs-lookup"><span data-stu-id="6be1b-113">For example, if the request contains an X-Requested-With header, indicating an AJAX request, the server might default to JSON if there is no Accept header.</span></span>

<span data-ttu-id="6be1b-114">Dans cet article, nous allons examiner comment les API Web utilise les en-têtes Accept et Accept-Charset.</span><span class="sxs-lookup"><span data-stu-id="6be1b-114">In this article, we'll look at how Web API uses the Accept and Accept-Charset headers.</span></span> <span data-ttu-id="6be1b-115">(Pour l’instant, il est sans prise en charge intégrée pour Accept-Encoding ou Accept-Language.)</span><span class="sxs-lookup"><span data-stu-id="6be1b-115">(At this time, there is no built-in support for Accept-Encoding or Accept-Language.)</span></span>

## <a name="serialization"></a><span data-ttu-id="6be1b-116">Sérialisation</span><span class="sxs-lookup"><span data-stu-id="6be1b-116">Serialization</span></span>

<span data-ttu-id="6be1b-117">Si un contrôleur d’API Web renvoie une ressource en tant que type CLR, le pipeline sérialise la valeur de retour et les écrit dans le corps de réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="6be1b-117">If a Web API controller returns a resource as CLR type, the pipeline serializes the return value and writes it into the HTTP response body.</span></span>

<span data-ttu-id="6be1b-118">Par exemple, considérez l’action du contrôleur suivant :</span><span class="sxs-lookup"><span data-stu-id="6be1b-118">For example, consider the following controller action:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

<span data-ttu-id="6be1b-119">Un client peut envoyer cette requête HTTP :</span><span class="sxs-lookup"><span data-stu-id="6be1b-119">A client might send this HTTP request:</span></span>

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

<span data-ttu-id="6be1b-120">En réponse, le serveur peut envoyer :</span><span class="sxs-lookup"><span data-stu-id="6be1b-120">In response, the server might send:</span></span>

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

<span data-ttu-id="6be1b-121">Dans cet exemple, le client a demandé JSON, Javascript ou « tout ce que » (\*/\*).</span><span class="sxs-lookup"><span data-stu-id="6be1b-121">In this example, the client requested either JSON, Javascript, or "anything" (\*/\*).</span></span> <span data-ttu-id="6be1b-122">Le serveur répond avec une représentation JSON de la `Product` objet.</span><span class="sxs-lookup"><span data-stu-id="6be1b-122">The server responsed with a JSON representation of the `Product` object.</span></span> <span data-ttu-id="6be1b-123">Notez que l’en-tête Content-Type dans la réponse est définie sur &quot;application/json&quot;.</span><span class="sxs-lookup"><span data-stu-id="6be1b-123">Notice that the Content-Type header in the response is set to &quot;application/json&quot;.</span></span>

<span data-ttu-id="6be1b-124">Un contrôleur peut également retourner un **HttpResponseMessage** objet.</span><span class="sxs-lookup"><span data-stu-id="6be1b-124">A controller can also return an **HttpResponseMessage** object.</span></span> <span data-ttu-id="6be1b-125">Pour spécifier un objet CLR pour le corps de réponse, appelez le **CreateResponse** méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="6be1b-125">To specify a CLR object for the response body, call the **CreateResponse** extension method:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

<span data-ttu-id="6be1b-126">Cette option vous donne davantage de contrôle sur les détails de la réponse.</span><span class="sxs-lookup"><span data-stu-id="6be1b-126">This option gives you more control over the details of the response.</span></span> <span data-ttu-id="6be1b-127">Vous pouvez définir le code d’état, ajouter des en-têtes HTTP et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="6be1b-127">You can set the status code, add HTTP headers, and so forth.</span></span>

<span data-ttu-id="6be1b-128">L’objet qui sérialise la ressource est appelé un *formateur média*.</span><span class="sxs-lookup"><span data-stu-id="6be1b-128">The object that serializes the resource is called a *media formatter*.</span></span> <span data-ttu-id="6be1b-129">Formateurs de médias dérivent le **MediaTypeFormatter** classe.</span><span class="sxs-lookup"><span data-stu-id="6be1b-129">Media formatters derive from the **MediaTypeFormatter** class.</span></span> <span data-ttu-id="6be1b-130">API Web fournit les formateurs de médias pour XML et JSON, et vous pouvez créer des formateurs personnalisés pour prendre en charge d’autres types de médias.</span><span class="sxs-lookup"><span data-stu-id="6be1b-130">Web API provides media formatters for XML and JSON, and you can create custom formatters to support other media types.</span></span> <span data-ttu-id="6be1b-131">Pour plus d’informations sur l’écriture d’un formateur personnalisé, consultez [formateurs de médias](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="6be1b-131">For information about writing a custom formatter, see [Media Formatters](media-formatters.md).</span></span>

## <a name="how-content-negotiation-works"></a><span data-ttu-id="6be1b-132">Fonctionnement de la négociation contenu comment</span><span class="sxs-lookup"><span data-stu-id="6be1b-132">How Content Negotiation Works</span></span>

<span data-ttu-id="6be1b-133">Tout d’abord, le pipeline Obtient le **IContentNegotiator** service à partir de la **HttpConfiguration** objet.</span><span class="sxs-lookup"><span data-stu-id="6be1b-133">First, the pipeline gets the **IContentNegotiator** service from the **HttpConfiguration** object.</span></span> <span data-ttu-id="6be1b-134">Il obtient également la liste des formateurs de médias à partir de la **HttpConfiguration.Formatters** collection.</span><span class="sxs-lookup"><span data-stu-id="6be1b-134">It also gets the list of media formatters from the **HttpConfiguration.Formatters** collection.</span></span>

<span data-ttu-id="6be1b-135">Ensuite, le pipeline appelle **IContentNegotiatior.Negotiate**, en passant dans :</span><span class="sxs-lookup"><span data-stu-id="6be1b-135">Next, the pipeline calls **IContentNegotiatior.Negotiate**, passing in:</span></span>

- <span data-ttu-id="6be1b-136">Le type d’objet à sérialiser</span><span class="sxs-lookup"><span data-stu-id="6be1b-136">The type of object to serialize</span></span>
- <span data-ttu-id="6be1b-137">La collection de formateurs de médias</span><span class="sxs-lookup"><span data-stu-id="6be1b-137">The collection of media formatters</span></span>
- <span data-ttu-id="6be1b-138">La requête HTTP</span><span class="sxs-lookup"><span data-stu-id="6be1b-138">The HTTP request</span></span>

<span data-ttu-id="6be1b-139">Le **Negotiate** méthode retourne deux informations :</span><span class="sxs-lookup"><span data-stu-id="6be1b-139">The **Negotiate** method returns two pieces of information:</span></span>

- <span data-ttu-id="6be1b-140">Le formateur à utiliser</span><span class="sxs-lookup"><span data-stu-id="6be1b-140">Which formatter to use</span></span>
- <span data-ttu-id="6be1b-141">Le type de média pour la réponse</span><span class="sxs-lookup"><span data-stu-id="6be1b-141">The media type for the response</span></span>

<span data-ttu-id="6be1b-142">Si aucun formateur n’est trouvée, le **Negotiate** retourne de la méthode **null**et du client reçoit HTTP 406 (non Acceptable).</span><span class="sxs-lookup"><span data-stu-id="6be1b-142">If no formatter is found, the **Negotiate** method returns **null**, and the client recevies HTTP error 406 (Not Acceptable).</span></span>

<span data-ttu-id="6be1b-143">Le code suivant montre comment un contrôleur peut appeler directement la négociation de contenu :</span><span class="sxs-lookup"><span data-stu-id="6be1b-143">The following code shows how a controller can directly invoke content negotiation:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

<span data-ttu-id="6be1b-144">Ce code est équivalent à la que le pipeline effectue automatiquement.</span><span class="sxs-lookup"><span data-stu-id="6be1b-144">This code is equivalent to the what the pipeline does automatically.</span></span>

## <a name="default-content-negotiator"></a><span data-ttu-id="6be1b-145">Négociateur de contenu par défaut</span><span class="sxs-lookup"><span data-stu-id="6be1b-145">Default Content Negotiator</span></span>

<span data-ttu-id="6be1b-146">Le **DefaultContentNegotiator** classe fournit l’implémentation par défaut de **IContentNegotiator**.</span><span class="sxs-lookup"><span data-stu-id="6be1b-146">The **DefaultContentNegotiator** class provides the default implementation of **IContentNegotiator**.</span></span> <span data-ttu-id="6be1b-147">Elle utilise plusieurs critères pour sélectionner un formateur.</span><span class="sxs-lookup"><span data-stu-id="6be1b-147">It uses several criteria to select a formatter.</span></span>

<span data-ttu-id="6be1b-148">Tout d’abord, le formateur doit être en mesure de sérialiser le type.</span><span class="sxs-lookup"><span data-stu-id="6be1b-148">First, the formatter must be able to serialize the type.</span></span> <span data-ttu-id="6be1b-149">Cela est vérifiée en appelant **MediaTypeFormatter.CanWriteType**.</span><span class="sxs-lookup"><span data-stu-id="6be1b-149">This is verified by calling **MediaTypeFormatter.CanWriteType**.</span></span>

<span data-ttu-id="6be1b-150">Ensuite, le négociateur de contenu examine chaque formateur et évalue la manière dont elle correspond à la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="6be1b-150">Next, the content negotiator looks at each formatter and evaluates how well it matches the HTTP request.</span></span> <span data-ttu-id="6be1b-151">Pour évaluer la correspondance, le négociateur de contenu examine deux choses sur le formateur :</span><span class="sxs-lookup"><span data-stu-id="6be1b-151">To evaluate the match, the content negotiator looks at two things on the formatter:</span></span>

- <span data-ttu-id="6be1b-152">Le **SupportedMediaTypes** collection qui contient une liste de types de médias pris en charge.</span><span class="sxs-lookup"><span data-stu-id="6be1b-152">The **SupportedMediaTypes** collection, which contains a list of supported media types.</span></span> <span data-ttu-id="6be1b-153">Négociateur de contenu a essaie de correspondre à cette liste par rapport à l’en-tête Accept de la demande.</span><span class="sxs-lookup"><span data-stu-id="6be1b-153">The content negotiator tries to match this list against the request Accept header.</span></span> <span data-ttu-id="6be1b-154">Notez que l’en-tête Accept peut contenir que plages.</span><span class="sxs-lookup"><span data-stu-id="6be1b-154">Note that the Accept header can include ranges.</span></span> <span data-ttu-id="6be1b-155">Par exemple, « text/plain » est une correspondance pour le texte /\* ou \* / \*.</span><span class="sxs-lookup"><span data-stu-id="6be1b-155">For example, "text/plain" is a match for text/\* or \*/\*.</span></span>
- <span data-ttu-id="6be1b-156">Le **MediaTypeMappings** collection qui contient une liste de **MediaTypeMapping** objets.</span><span class="sxs-lookup"><span data-stu-id="6be1b-156">The **MediaTypeMappings** collection, which contains a list of **MediaTypeMapping** objects.</span></span> <span data-ttu-id="6be1b-157">Le **MediaTypeMapping** classe offre un moyen générique pour faire correspondre des requêtes HTTP avec les types de médias.</span><span class="sxs-lookup"><span data-stu-id="6be1b-157">The **MediaTypeMapping** class provides a generic way to match HTTP requests with media types.</span></span> <span data-ttu-id="6be1b-158">Par exemple, il peut mapper un en-tête HTTP personnalisé à un type de média spécifique.</span><span class="sxs-lookup"><span data-stu-id="6be1b-158">For example, it could map a custom HTTP header to a particular media type.</span></span>

<span data-ttu-id="6be1b-159">S’il existe plusieurs correspond à, la correspondance avec l’emporte de facteur de qualité la plus élevée.</span><span class="sxs-lookup"><span data-stu-id="6be1b-159">If there are multiple matches, the match with the highest quality factor wins.</span></span> <span data-ttu-id="6be1b-160">Exemple :</span><span class="sxs-lookup"><span data-stu-id="6be1b-160">For example:</span></span>

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

<span data-ttu-id="6be1b-161">Dans cet exemple, application/json est un facteur implicite de qualité de 1.0, il est donc par défaut sur application/xml.</span><span class="sxs-lookup"><span data-stu-id="6be1b-161">In this example, application/json has an implied quality factor of 1.0, so it is preferred over application/xml.</span></span>

<span data-ttu-id="6be1b-162">Si aucune correspondance n’est trouvée, le négociateur de contenu tente de faire correspondre sur le type de média du corps de la demande, le cas échéant.</span><span class="sxs-lookup"><span data-stu-id="6be1b-162">If no matches are found, the content negotiator tries to match on the media type of the request body, if any.</span></span> <span data-ttu-id="6be1b-163">Par exemple, si la demande contient des données JSON, le négociateur de contenu recherche un formateur JSON.</span><span class="sxs-lookup"><span data-stu-id="6be1b-163">For example, if the request contains JSON data, the content negotiator looks for a JSON formatter.</span></span>

<span data-ttu-id="6be1b-164">S’il existe toujours aucune correspondance, le négociateur de contenu récupère simplement le premier formateur pouvant sérialiser le type.</span><span class="sxs-lookup"><span data-stu-id="6be1b-164">If there are still no matches, the content negotiator simply picks the first formatter that can serialize the type.</span></span>

## <a name="selecting-a-character-encoding"></a><span data-ttu-id="6be1b-165">Sélection d’un encodage de caractères</span><span class="sxs-lookup"><span data-stu-id="6be1b-165">Selecting a Character Encoding</span></span>

<span data-ttu-id="6be1b-166">Une fois un formateur est sélectionné, le négociateur de contenu choisit le meilleur codage de caractères en examinant le **SupportedEncodings** propriété sur le module de formatage et correspondant à l’en-tête Accept-Charset dans la demande (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="6be1b-166">After a formatter is selected, the content negotiator chooses the best character encoding by looking at the **SupportedEncodings** property on the formatter, and matching it against the Accept-Charset header in the request (if any).</span></span>
