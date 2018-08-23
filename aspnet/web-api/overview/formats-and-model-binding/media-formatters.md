---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Formateurs de médias dans ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 7b7ba2fb3f1bba0447e700c84a017266cba305e6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831404"
---
<a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="4c789-102">Formateurs de médias dans ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="4c789-102">Media Formatters in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="4c789-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4c789-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="4c789-104">Ce didacticiel montre comment prendre en charge des formats supplémentaires dans l’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4c789-104">This tutorial shows how to support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="4c789-105">Types de média Internet</span><span class="sxs-lookup"><span data-stu-id="4c789-105">Internet Media Types</span></span>

<span data-ttu-id="4c789-106">Un type de média, également appelé type MIME, identifie le format d’un élément de données.</span><span class="sxs-lookup"><span data-stu-id="4c789-106">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="4c789-107">Dans HTTP, les types de médias décrivent le format du corps du message.</span><span class="sxs-lookup"><span data-stu-id="4c789-107">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="4c789-108">Un type de média se compose de deux chaînes, un type et un sous-type.</span><span class="sxs-lookup"><span data-stu-id="4c789-108">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="4c789-109">Exemple :</span><span class="sxs-lookup"><span data-stu-id="4c789-109">For example:</span></span>

- <span data-ttu-id="4c789-110">texte/html</span><span class="sxs-lookup"><span data-stu-id="4c789-110">text/html</span></span>
- <span data-ttu-id="4c789-111">image/png</span><span class="sxs-lookup"><span data-stu-id="4c789-111">image/png</span></span>
- <span data-ttu-id="4c789-112">application/json</span><span class="sxs-lookup"><span data-stu-id="4c789-112">application/json</span></span>

<span data-ttu-id="4c789-113">Lorsqu’un message HTTP contienne un corps d’entité, l’en-tête Content-Type spécifie le format du corps du message.</span><span class="sxs-lookup"><span data-stu-id="4c789-113">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="4c789-114">Cela indique le destinataire d’analyser le contenu du corps du message.</span><span class="sxs-lookup"><span data-stu-id="4c789-114">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="4c789-115">Par exemple, si une réponse HTTP contient une image PNG, la réponse peut contenir les en-têtes suivants.</span><span class="sxs-lookup"><span data-stu-id="4c789-115">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="4c789-116">Lorsque le client envoie un message de demande, il peut inclure un en-tête Accept.</span><span class="sxs-lookup"><span data-stu-id="4c789-116">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="4c789-117">L’en-tête Accept indique que le serveur quels supports type (s) le client veut à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="4c789-117">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="4c789-118">Exemple :</span><span class="sxs-lookup"><span data-stu-id="4c789-118">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="4c789-119">Cet en-tête indique au serveur que le client veut HTML, XHTML ou XML.</span><span class="sxs-lookup"><span data-stu-id="4c789-119">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="4c789-120">Le type de support détermine la manière dont les API Web sérialise et désérialise le corps du message HTTP.</span><span class="sxs-lookup"><span data-stu-id="4c789-120">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="4c789-121">API Web dispose d’une prise en charge intégrée des données XML, JSON, BSON et form-UrlEncode données, et peut prendre en charge les types de médias supplémentaires en écrivant un *formateur média*.</span><span class="sxs-lookup"><span data-stu-id="4c789-121">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="4c789-122">Pour créer un formateur de médias, dérivez à partir d’une de ces classes :</span><span class="sxs-lookup"><span data-stu-id="4c789-122">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="4c789-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="4c789-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="4c789-124">Cette classe utilise la lecture asynchrone et les méthodes d’écriture.</span><span class="sxs-lookup"><span data-stu-id="4c789-124">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="4c789-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="4c789-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="4c789-126">Cette classe est dérivée de **MediaTypeFormatter** , mais utilise des méthodes de lecture/écriture Synchronous.</span><span class="sxs-lookup"><span data-stu-id="4c789-126">This class derives from **MediaTypeFormatter** but uses sychronous read/write methods.</span></span>

<span data-ttu-id="4c789-127">Dérivant de **BufferedMediaTypeFormatter** est plus simple, car il n’existe aucun code asynchrone, mais cela signifie également que le thread appelant peut bloquer pendant les e/s.</span><span class="sxs-lookup"><span data-stu-id="4c789-127">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="4c789-128">Exemple : Création d’un formateur de médias CSV</span><span class="sxs-lookup"><span data-stu-id="4c789-128">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="4c789-129">L’exemple suivant montre un formateur de type de média qui peut sérialiser un objet de produit dans un format de valeurs séparées par des virgules (CSV).</span><span class="sxs-lookup"><span data-stu-id="4c789-129">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="4c789-130">Cet exemple utilise le type de produit défini dans le didacticiel [d’une API Web qui prend en charge les opérations CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span><span class="sxs-lookup"><span data-stu-id="4c789-130">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="4c789-131">Voici la définition de l’objet Product :</span><span class="sxs-lookup"><span data-stu-id="4c789-131">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="4c789-132">Pour implémenter un module de formatage de volume partagé de cluster, définissez une classe qui dérive de **BufferedMediaTypeFormater**:</span><span class="sxs-lookup"><span data-stu-id="4c789-132">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormater**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="4c789-133">Dans le constructeur, ajoutez les types de médias qui prend en charge par le formateur.</span><span class="sxs-lookup"><span data-stu-id="4c789-133">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="4c789-134">Dans cet exemple, le formateur prend en charge un seul type de support, &quot;texte/csv&quot;:</span><span class="sxs-lookup"><span data-stu-id="4c789-134">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="4c789-135">Remplacer le **CanWriteType** méthode pour indiquer leurs types le formateur peut sérialiser :</span><span class="sxs-lookup"><span data-stu-id="4c789-135">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="4c789-136">Dans cet exemple, le module de formatage peut sérialiser unique `Product` objets ainsi que des collections de `Product` objets.</span><span class="sxs-lookup"><span data-stu-id="4c789-136">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="4c789-137">De même, remplacez le **CanReadType** méthode pour indiquer leurs types le formateur peut désérialiser.</span><span class="sxs-lookup"><span data-stu-id="4c789-137">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="4c789-138">Dans cet exemple, le formateur ne prend pas en charge la désérialisation, par conséquent, la méthode retourne simplement **false**.</span><span class="sxs-lookup"><span data-stu-id="4c789-138">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="4c789-139">Enfin, vous pouvez substituer le **WriteToStream** (méthode).</span><span class="sxs-lookup"><span data-stu-id="4c789-139">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="4c789-140">Cette méthode sérialise un type en l’écrivant dans un flux.</span><span class="sxs-lookup"><span data-stu-id="4c789-140">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="4c789-141">Si votre formateur prend en charge la désérialisation, vous devez également substituer la **ReadFromStream** (méthode).</span><span class="sxs-lookup"><span data-stu-id="4c789-141">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="4c789-142">Ajout d’un formateur de média pour le Pipeline de l’API Web</span><span class="sxs-lookup"><span data-stu-id="4c789-142">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="4c789-143">Pour ajouter un support de type formateur au pipeline API Web, utilisez le **formateurs** propriété sur le **HttpConfiguration** objet.</span><span class="sxs-lookup"><span data-stu-id="4c789-143">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="4c789-144">Encodages de caractères</span><span class="sxs-lookup"><span data-stu-id="4c789-144">Character Encodings</span></span>

<span data-ttu-id="4c789-145">Si vous le souhaitez, un formateur de médias peut prendre en charge plusieurs codages de caractères, telles que UTF-8 ou ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="4c789-145">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="4c789-146">Dans le constructeur, ajoutez un ou plusieurs [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types à la **SupportedEncodings** collection.</span><span class="sxs-lookup"><span data-stu-id="4c789-146">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="4c789-147">Placez le premier de codage par défaut.</span><span class="sxs-lookup"><span data-stu-id="4c789-147">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="4c789-148">Dans le **WriteToStream** et **ReadFromStream** appeler des méthodes, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) pour sélectionner l’encodage de caractères par défaut.</span><span class="sxs-lookup"><span data-stu-id="4c789-148">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="4c789-149">Cette méthode correspond aux en-têtes de demande par rapport à la liste des encodages pris en charge.</span><span class="sxs-lookup"><span data-stu-id="4c789-149">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="4c789-150">Utilisez retourné **Encoding** lorsque vous lisez ou écrivez à partir du flux :</span><span class="sxs-lookup"><span data-stu-id="4c789-150">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
