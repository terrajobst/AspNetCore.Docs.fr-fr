---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Prise en charge BSON dans ASP.NET Web API 2.1 | Documents Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 53ad705fad6d2225cecca4d73355bd6ebfcf56d5
ms.sourcegitcommit: 459cb3289741a3f46325e605a617dc926ee0563d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2018
---
<a name="bson-support-in-aspnet-web-api-21"></a><span data-ttu-id="296ec-102">Prise en charge BSON dans ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="296ec-102">BSON Support in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="296ec-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="296ec-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="296ec-104">Web API 2.1 prend en charge BSON.</span><span class="sxs-lookup"><span data-stu-id="296ec-104">Web API 2.1 introduces support for BSON.</span></span> <span data-ttu-id="296ec-105">Cette rubrique montre comment utiliser BSON dans votre contrôleur Web API (côté serveur) et dans une application cliente .NET.</span><span class="sxs-lookup"><span data-stu-id="296ec-105">This topic shows how to use BSON in your Web API controller (server side) and in a .NET client app.</span></span>

## <a name="what-is-bson"></a><span data-ttu-id="296ec-106">Qu’est BSON ?</span><span class="sxs-lookup"><span data-stu-id="296ec-106">What is BSON?</span></span>

<span data-ttu-id="296ec-107">[BSON](http://bsonspec.org/) est un format de sérialisation binaire.</span><span class="sxs-lookup"><span data-stu-id="296ec-107">[BSON](http://bsonspec.org/) is a binary serialization format.</span></span> <span data-ttu-id="296ec-108">« BSON » signifie « JSON binaire », mais BSON and JSON sont sérialisées très différemment.</span><span class="sxs-lookup"><span data-stu-id="296ec-108">"BSON" stands for "Binary JSON", but BSON and JSON are serialized very differently.</span></span> <span data-ttu-id="296ec-109">BSON est « JSON-like », car les objets sont représentés sous forme de paires nom-valeur, semblables au format JSON.</span><span class="sxs-lookup"><span data-stu-id="296ec-109">BSON is "JSON-like", because objects are represented as name-value pairs, similar to JSON.</span></span> <span data-ttu-id="296ec-110">Contrairement à JSON, les types de données numériques sont stockés sous forme d’octets, pas des chaînes</span><span class="sxs-lookup"><span data-stu-id="296ec-110">Unlike JSON, numeric data types are stored as bytes, not strings</span></span>

<span data-ttu-id="296ec-111">BSON a été conçu pour être léger, facile à analyser et rapide pour encoder/décoder.</span><span class="sxs-lookup"><span data-stu-id="296ec-111">BSON was designed to be lightweight, easy to scan, and fast to encode/decode.</span></span>

- <span data-ttu-id="296ec-112">BSON est comparable au format JSON.</span><span class="sxs-lookup"><span data-stu-id="296ec-112">BSON is comparable in size to JSON.</span></span> <span data-ttu-id="296ec-113">En fonction des données, une charge utile BSON peut être inférieure ou supérieure à une charge utile JSON.</span><span class="sxs-lookup"><span data-stu-id="296ec-113">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="296ec-114">Pour la sérialisation des données binaires, tel qu’un fichier image, BSON est inférieure à JSON, car les données binaires ne sont pas codée en base64.</span><span class="sxs-lookup"><span data-stu-id="296ec-114">For serializing binary data, such as an image file, BSON is smaller than JSON, because the binary data is not base64-encoded.</span></span>
- <span data-ttu-id="296ec-115">BSON documents sont faciles à analyser, car les éléments sont préfixés avec un champ de longueur, un analyseur pouvez sauter des éléments sans les décoder.</span><span class="sxs-lookup"><span data-stu-id="296ec-115">BSON documents are easy to scan because elements are prefixed with a length field, so a parser can skip elements without decoding them.</span></span>
- <span data-ttu-id="296ec-116">Codage et décodage sont efficaces, car les types de données numériques sont stockées sous forme de nombres, pas des chaînes.</span><span class="sxs-lookup"><span data-stu-id="296ec-116">Encoding and decoding are efficient, because numeric data types are stored as numbers, not strings.</span></span>

<span data-ttu-id="296ec-117">Clients natifs, tels que les applications clientes .NET, peuvent bénéficier de l’aide BSON à la place de textuelles de formats comme JSON ou XML.</span><span class="sxs-lookup"><span data-stu-id="296ec-117">Native clients, such as .NET client apps, can benefit from using BSON in place of text-based formats such as JSON or XML.</span></span> <span data-ttu-id="296ec-118">Pour les clients de navigateur, vous souhaiterez probablement stick avec JSON, car JavaScript peut convertir directement de la charge utile JSON.</span><span class="sxs-lookup"><span data-stu-id="296ec-118">For browser clients, you will probably want to stick with JSON, because JavaScript can directly convert the JSON payload.</span></span>

<span data-ttu-id="296ec-119">Heureusement, API Web utilise [négociation de contenu](content-negotiation.md), de sorte que votre API peut prendre en charge les deux formats et choisir le client.</span><span class="sxs-lookup"><span data-stu-id="296ec-119">Fortunately, Web API uses [content negotiation](content-negotiation.md), so your API can support both formats and let the client choose.</span></span>

## <a name="enabling-bson-on-the-server"></a><span data-ttu-id="296ec-120">L’activation de BSON sur le serveur</span><span class="sxs-lookup"><span data-stu-id="296ec-120">Enabling BSON on the Server</span></span>

<span data-ttu-id="296ec-121">Dans la configuration de votre API Web, ajoutez le **BsonMediaTypeFormatter** à la collection de formateurs.</span><span class="sxs-lookup"><span data-stu-id="296ec-121">In your Web API configuration, add the **BsonMediaTypeFormatter** to the formatters collection.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

<span data-ttu-id="296ec-122">Maintenant si le client demande « application/bson », API Web utilise le formateur BSON.</span><span class="sxs-lookup"><span data-stu-id="296ec-122">Now if the client requests "application/bson", Web API will use the BSON formatter.</span></span>

<span data-ttu-id="296ec-123">Pour associer BSON avec d’autres types de média, ajoutez-les à la collection SupportedMediaTypes.</span><span class="sxs-lookup"><span data-stu-id="296ec-123">To associate BSON with other media types, add them to the SupportedMediaTypes collection.</span></span> <span data-ttu-id="296ec-124">Le code suivant ajoute « application/vnd.contoso » pour les types de médias pris en charge :</span><span class="sxs-lookup"><span data-stu-id="296ec-124">The following code adds "application/vnd.contoso" to the supported media types:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a><span data-ttu-id="296ec-125">Exemple de Session HTTP</span><span class="sxs-lookup"><span data-stu-id="296ec-125">Example HTTP Session</span></span>

<span data-ttu-id="296ec-126">Pour cet exemple, nous allons utiliser la classe de modèle suivant plus de contrôleur d’API Web simple :</span><span class="sxs-lookup"><span data-stu-id="296ec-126">For this example, we'll use the following model class plus a simple Web API controller:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

<span data-ttu-id="296ec-127">Un client peut envoyer la requête HTTP suivante :</span><span class="sxs-lookup"><span data-stu-id="296ec-127">A client might send the following HTTP request:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

<span data-ttu-id="296ec-128">Voici la réponse :</span><span class="sxs-lookup"><span data-stu-id="296ec-128">Here is the response:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

<span data-ttu-id="296ec-129">Ici, j’ai remplacé les données binaires avec &quot;.&quot; caractères.</span><span class="sxs-lookup"><span data-stu-id="296ec-129">Here I've replaced the binary data with &quot;.&quot; characters.</span></span> <span data-ttu-id="296ec-130">La capture d’écran suivante de montre de Fiddler les valeurs hexadécimales bruts.</span><span class="sxs-lookup"><span data-stu-id="296ec-130">The following screen shot from Fiddler shows the raw hex values.</span></span>

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a><span data-ttu-id="296ec-131">À l’aide de BSON avec HttpClient</span><span class="sxs-lookup"><span data-stu-id="296ec-131">Using BSON with HttpClient</span></span>

<span data-ttu-id="296ec-132">Applications de clients .NET peuvent utiliser le formateur BSON **HttpClient**.</span><span class="sxs-lookup"><span data-stu-id="296ec-132">.NET clients apps can use the BSON formatter with **HttpClient**.</span></span> <span data-ttu-id="296ec-133">Pour plus d’informations sur **HttpClient**, consultez [appeler une Web API d’un Client .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="296ec-133">For more information about **HttpClient**, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="296ec-134">Le code suivant envoie une demande GET qui accepte BSON, puis les désérialise la charge BSON dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="296ec-134">The following code sends a GET request that accepts BSON, and then deserializes the BSON payload in the response.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

<span data-ttu-id="296ec-135">Pour demander BSON à partir du serveur, définissez l’en-tête Accept pour « application/bson » :</span><span class="sxs-lookup"><span data-stu-id="296ec-135">To request BSON from the server, set the Accept header to "application/bson":</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

<span data-ttu-id="296ec-136">Pour désérialiser le corps de réponse, utilisez le **BsonMediaTypeFormatter**.</span><span class="sxs-lookup"><span data-stu-id="296ec-136">To deserialize the response body, use the **BsonMediaTypeFormatter**.</span></span> <span data-ttu-id="296ec-137">Ce module de formatage n’est pas dans la collection de modules de formatage par défaut, afin que vous deviez spécifier lorsque vous lisez le corps de réponse :</span><span class="sxs-lookup"><span data-stu-id="296ec-137">This formatter is not in the default formatters collection, so you have to specify it when you read the response body:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

<span data-ttu-id="296ec-138">L’exemple suivant montre comment envoyer une demande POST qui contient BSON.</span><span class="sxs-lookup"><span data-stu-id="296ec-138">The next example shows how to send a POST request that contains BSON.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

<span data-ttu-id="296ec-139">Une grande partie de ce code est identique à l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="296ec-139">Much of this code is the same as the previous example.</span></span> <span data-ttu-id="296ec-140">Mais, dans le **PostAsync** (méthode), spécifiez **BsonMediaTypeFormatter** en tant que le module de formatage :</span><span class="sxs-lookup"><span data-stu-id="296ec-140">But in the **PostAsync** method, specify **BsonMediaTypeFormatter** as the formatter:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a><span data-ttu-id="296ec-141">Sérialisation des Types primitifs de niveau supérieur</span><span class="sxs-lookup"><span data-stu-id="296ec-141">Serializing Top-Level Primitive Types</span></span>

<span data-ttu-id="296ec-142">Chaque document BSON est une liste de paires clé/valeur. La spécification BSON ne définit pas une syntaxe pour la sérialisation d’une seule valeur brute, tel qu’un entier ou une chaîne.</span><span class="sxs-lookup"><span data-stu-id="296ec-142">Every BSON document is a list of key/value pairs.The BSON specification does not define a syntax for serializing a single raw value, such as an integer or string.</span></span>

<span data-ttu-id="296ec-143">Pour contourner cette limitation, le **BsonMediaTypeFormatter** traite des types primitifs comme un cas spécial.</span><span class="sxs-lookup"><span data-stu-id="296ec-143">To work around this limitation, the **BsonMediaTypeFormatter** treats primitive types as a special case.</span></span> <span data-ttu-id="296ec-144">Avant de sérialiser, il convertit la valeur dans une paire clé/valeur avec la clé « Valeur ».</span><span class="sxs-lookup"><span data-stu-id="296ec-144">Before serializing, it converts the value into a key/value pair with the key "Value".</span></span> <span data-ttu-id="296ec-145">Par exemple, supposons que votre contrôleur d’API retourne un entier :</span><span class="sxs-lookup"><span data-stu-id="296ec-145">For example, suppose your API controller returns an integer:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

<span data-ttu-id="296ec-146">Avant la sérialisation, le formateur BSON convertit à la paire clé/valeur suivantes :</span><span class="sxs-lookup"><span data-stu-id="296ec-146">Before serializing, the BSON formatter converts this to the following key/value pair:</span></span>

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

<span data-ttu-id="296ec-147">Lorsque vous désérialisez, le formateur convertit les données à la valeur d’origine.</span><span class="sxs-lookup"><span data-stu-id="296ec-147">When you deserialize, the formatter converts the data back to the original value.</span></span> <span data-ttu-id="296ec-148">Toutefois, les clients à l’aide d’un analyseur BSON différent devrez gérer ce cas, si votre API web renvoie les valeurs brutes.</span><span class="sxs-lookup"><span data-stu-id="296ec-148">However, clients using a different BSON parser will need to handle this case, if your web API returns raw values.</span></span> <span data-ttu-id="296ec-149">En règle générale, vous devez envisager de retourner des données structurées, plutôt que les valeurs brutes.</span><span class="sxs-lookup"><span data-stu-id="296ec-149">In general, you should consider returning structured data, rather than raw values.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="296ec-150">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="296ec-150">Additional Resources</span></span>

[<span data-ttu-id="296ec-151">Exemple d’API BSON Web</span><span class="sxs-lookup"><span data-stu-id="296ec-151">Web API BSON Sample</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[<span data-ttu-id="296ec-152">Formateurs de médias</span><span class="sxs-lookup"><span data-stu-id="296ec-152">Media Formatters</span></span>](media-formatters.md)
